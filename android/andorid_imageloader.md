# Android 图片加载库

## 各个框架的对比

* Google Glide 

* Facebook Picasso

## Glide

* 使用

* 源码引入Glide


### Glide使用注意事项

1、引入largeHeap属性，让系统为App分配更多的独立内存。

2、禁止Glide内存缓存。设置skipMemoryCache(true)。

3、自定义GlideModule。设置MemoryCache和BitmapPool大小。

4、升级到Glide4.0，使用asDrawable代替asBitmap，drawable更省内存。

5、ImageView的scaleType为fitXY时，改为fitCenter/centerCrop/fitStart/fitEnd显示。

6、不使用application作为context。当context为application时，会把imageView是生命周期延长到整个运行过程中，imageView不能被回收，从而造成OOM异常。

7、使用application作为context。但是对ImageView使用弱引用或软引用，尽量使用SoftReference，当内存不足时，将及时回收无用的ImageView。

8、当列表在滑动的时候，调用Glide的pauseRequests()取消请求，滑动停止时，调用resumeRequests()恢复请求。

9、Try catch某些大内存分配的操作。考虑在catch里面尝试一次降级的内存分配操作。例如decode bitmap的时候，catch到OOM，可以尝试把采样比例再增加一倍之后，再次尝试decode。

10、BitmapFactory.Options和BitmapFactory.decodeStream获取原始图片的宽、高，绕过Java层加载Bitmap，再调用Glide的override(width,height)控制显示。

11、图片局部加载。参考：SubsamplingScaleImageView，先将图片下载到本地，然后去加载，只加载当前可视区域，在手指拖动的时候再去加载另外的区域。

12、Glide使用内存飙升记
解决:不要再ScrollView中使用Glide/Fresco加载图片,因为这两开源框架是根据RecyclerView回收池来复用图片,,,所以使用ScrollView会出现无法回收图片的问题

一、前言
Glide 是 Google 官方推荐的一款图片加载库，使用起来也非常的简单便利，Glide 它帮我们完成了很多很重要，但是却通用的功

能，例如：图片的加载压缩、展示、加载图片的内存管理等等。

对 Glide 还不熟悉的朋友，可以参考 《一篇好文，助你上手 Glide》

但是，在使用 Glide 的时候，有一些小技巧，可以让你的内存更优化，避免可能出现的 OOM。例如：虽然 Glide 会根据加载的控件

大小，优化加载后的图片尺寸，可如果加载的是一张全屏的大图，依然会是一个占用内存空间非常大的操作。

具体一张 Bitmap 到底占用了多少内存空间，可以参考《Bitmap 比你想的更费内存 | 吊打 OOM》

本文有些建议来自 Android TV App，而 Android TV 众多的智能电视和智能盒子，实际上硬件条件非常的恶劣，而 Android T

V 的 App ，为了美化，会用到大部分的图片，所以在图片使用方面，OOM 的问题就会被放大，而下面介绍的一些优化方

案，在 Android 手机硬件条件非常好的环境下，不使用影响也不大。

二、开始优化
2.1 配置好 TrimMemory 和 LowMemory
Glide 帮我们做了大部分内存管理方面的事情，实际上它还支持做的更好。

对于一个 App 而言，在系统内存环境不足的情况下，会回调一些 onTrimMemory() 或者 onLowMemory() 等方法，这些都是在提醒开

发者，当前设备的内存环境已经发生了变化，你最好调整你的内存使用策略，避免被系统清理掉或者出现 OOM 。

关于 onTrimMemroy() 相关内容，不了解的可以先参考《Android 开发，跳不过的内存管理》

而 Glide 也为我们提供了类似方法的接口，开发者只需要调用即可，它在内部会随着不同的内存情况，帮我们对缓存的图片进行优化。

在这里，你主要用到 Glide 的 trimMemory() 和 cleanMemroy() 方法，它们一个用来裁剪 Glide 缓存的图片内存空间，一个用来清

理 Glide 缓存的内存空间。

在使用 onTrimMemory() 之前，一般是实现 ComponentCallbacks2 接口，然后在 Application 中，通过 registerComponentCallback

s() 方法进行注册。当然，如果你嫌麻烦，还可以直接在 Application 中，重写对应的方法。

了解了这些，就可以根据我们的需要来配置在何时调用 Glide 的对应方法，我推荐的配置：

在 lowMemory 的时候，调用 Glide.cleanMemroy() 清理掉所有的内存缓存。
在 App 被置换到后台的时候，调用 Glide.cleanMemroy() 清理掉所有的内存缓存。
在其它情况的 onTrimMemroy() 回调中，直接调用 Glide.trimMemory() 方法来交给 Glide 处理内存情况。
那么对应的代码，如下：

/gm-app.png

既然知道需要调用 Glide 的这两个方法，我们还是需要了解到它内部到底帮我们做了什么。先来看看 Glide 对应的源码。

/gm-trim.png

在 Glide 的这些方法内，可以看到，它们都会去操作 memoryCache 和 bitmapPool 这两个对象，实际上它们是两个接口，这里如果做特殊处理，操作

的都是 Glide 对它们的默认实现，LruResourceCache 和 LruBitmapPool 。从名称上可以看出来，它们都是遵循 Lru 算法的。

就 Glide 而言，Memory Cache 是 Glide 用来在内存中缓存图片资源，使其在需要使用的时候立刻就可以使用，而不必执行磁

盘的 I/O 操作，而 BitmatPool 则是 Glide 维护了一个图片复用池，LruBitmapPool 使用 Lru 算法保留最近使用的尺寸的 Bitma

p，这不是本文的重点，大家了解一下即可。

其实 LruResourceCache 和 LruBitmapPool 中，对 clearMemory() 和 trimMemory() 的操作是类似的，这里就以 LruBitmapPool 举

例。

/gm-LruBitmapPool.png

在 LruBitmapPool 中，会根据回调的方法以及参数，调用 clearMemory() 或者 trimToSize()，其实最终都是调用的 trimToSiz

e() 方法。它用于裁剪当前缓存资源的个数。

/gm-trimtosize.png

可以看到，根据裁剪的目标尺寸，会去回收多余的 Bitmap 到合适的目标大小，以达到清理内存的目的。

2.2 配置 GlideModule
GlideModule 是 Glide 提供的一个配置接口，它会在第一次使用 Glide 的时候被调用，用于进行 Glide 的一些初始的配置。

具体 GlideModule 的使用，可以参见官方文档：

https://github.com/bumptech/glide/wiki/Configuration

GlideModule 是一个接口，需要实现其对应的方法。

/gm-module.png

这里我们只需要使用 applyOptions() 这个方法，它用于在 Glide 的默认配置的基础上，追加一些我们需要的配置。

而在这里，我们可以根据当前设备的内存情况，对其进行一个设定，使用 ActivityManager 获取当前设备的内存情况，如果是处于 lo

wMemory 的时候，将图片的 DecodeFormat 设置为 RGB_565 ， RGB_565 和默认的 ARGB_8888 比，每个像素会少 2 个byte，这

样，等于一张同样的图片，加载到内存中会少一半内存的占用（ARGB_8888 每个像素占 4 byte）。

/gm-apply.png

2.3 避免使用圆角的ImageView
在实际项目内，经常会用到一些带圆角的图片，或者直接就是圆形的图片。圆形的图片，多数用于一些用户的头像之类的显示效

果。

而在 Android 下，也有大量的类似 XxxImageView 的开源控件，用于操作 Bitmap 以达到一个圆角图片的效果，例如 Github 上比较火

的 RoundedImageView。

它们大部分的原理，是接收到你传递的 Bitmap ，然后再输出一个与原来 Bitmap 等大的新 Bitmap ，在此基础之上，进行圆角的一

些处理，这就导致了，实际上会在内存中，多持有一个 Bitmap ，一下一张图片占用的内存就被加倍了。

所以既然已经选择使用 Glide ，推荐使用 glide-transformations 这个开源库配合使用，glide-transformations 利用 Glide 的 bitmapTra

nsfrom() 接口，实现对加载的 Bitmap 的进行一些变换操作。

glide-transformations 的 Github 地址如下：

https://github.com/wasabeef/glide-transformations

glide-transformations 提供一系类对加载的图片的变换操作，从形状变换到色彩变换，全部支持，基本上满足大部分开发需要，并且

它会复用 Glide 的 BitmapPool ，来达到节约内存的目的。

具体 glide-transformations 的使用，可以查看 Github 上的文档，下面是它的一个效果图。



2.4 根据内存情况，裁剪你的图片
前面的介绍的一些优化点，都是一些推荐的通用做法，基本上用了前面介绍的办法，图片导致的 OOM 应该会大幅度减少。

接下来介绍一个在 Android TV 上，加载全屏大图的时候，优化内存问题的一个解决办法。

首先要明确一点，国内 Android TV 的硬件环境非常的不好，二百三百的智能盒子到处都在卖，毕竟也是跑的 Android 系统，你想想

你使用的是一款 299 的 Android 手机，你对它也不会有什么期待了。但是 Android TV 又是为了电视做的，所以大部分情况下，它都

是需要支持 1920 * 1280 之类的屏幕尺寸，导致它如果加载一张全屏的大图，消耗的内存是不忍直视的，如果在内存环境不好的情

况下，可能就直接 OOM 崩溃了。

所以，对于这种极端的情况，我想到了一个办法，根据当前的内存环境，按比例缩小需要显示的全屏图片，这样加载到内存中的图

片，就是按比例缩小的。

在这里就需要用到 DrawableRequestBuilder 的 override() 这个 Api 了，它可以接受一个 width 和 height ，来重新指定加载图片的

尺寸。

/gm-overr.png

既然 Glide 已经提供了标准的 Api ，那么我们还需要获取到当前运行设备的宽高。

/gm-display.png

这里推荐使用 getRealSize() 的方式获取屏幕的宽高，它可以真实的拿到当前屏幕的尺寸。其它 Api 在部分智能电视和盒子上，拿

到的尺寸会小，因为没有计算 StatusBar 或者 NavigationBar的高度，这些都是经验之谈。

同时，我们也需要用到 ComponentCallbacks2 这个接口，前面已经介绍过了，就不再赘述了。

/gm-listener.png

在其中，记录 trim 的 level 这个值，反应当前的内存级别，在使用的时候，通过 getBitmapSize() 裁剪出一个符合当前内存环境的

尺寸。

例子中只是对 TRIM_MENORY_RUNNING_LOW 进行了处理，会根据屏幕尺寸，缩放到 0.8f 倍的状态。如果要做的更多，可以将其

它几个 level 也加上，调整不同的缩放倍数。

/gm-fullscreenload.png

两个都输出一下，看看差别，同一张全屏的图片，不缩放和缩放 0.8f 的差别。

I/cxmyDev: bgImage byteCount : 8294400
I/cxmyDev: bgImage byteCount : 5308416
1
2
可以看到，优化的目的还是达到了。可以节约大概 3MB 左右的内存空间，而图片又不至于模糊到无法看的地步。


## Glide导致OOM处理

1. 当ImageView的scaleType设置为fitXY时，Glide会加载全分辨率的图片，尽管ImageView可能不需要这么大。


有2种选择

1. 更改ImageView的scaleType，比如改成fitCenter或者centerCrop
2. 或者加载的时候改为

## 参考

[官方网站](http://bumptech.github.io/glide/)

[Glide最全解析 郭霖的博客](https://blog.csdn.net/sinyu890807/column/info/15318)

[Glide 源码解析](https://github.com/android-cn/android-open-project-analysis/tree/master/tool-lib/image-cache/glide)

[Glide源码学习随笔](https://www.jianshu.com/p/e01f68802604)

[android图片加载库Glide](https://yq.aliyun.com/articles/63689/)

[Glide使用中的踩坑和填坑](http://www.zwdroid.top/2016/10/16/issues%20in%20using%20Glide/)

[探索Bitmap使用姿势](https://juejin.im/post/5c56bd57f265da2dc5388f33)

[Android中一张图片占据的内存大小是如何计算](https://juejin.im/post/5bc406b9f265da0aa664ea1e)

[图集功能，查看大图，进行手势缩放](https://github.com/crazyandcoder/ImageZoom)

[Glide](https://www.jianshu.com/p/cbeaf5826d03?tdsourcetag=s_pcqq_aiomsg)

[超级巨图Glide3.7和Glide4.1.1优化加载方案](https://blog.51cto.com/liangxiao/1966795)

[Glide 之磁盘缓存](http://liwenkun.me/2017/08/29/glide-disk-cache-strategy/)

[Glide4 使用教程](https://juejin.im/entry/5ad5555f51882555867fe935)

[Glide源码学习随笔](https://www.jianshu.com/p/e01f68802604)

[Glide入门教程——6.播放Gif & 视频](https://www.jianshu.com/p/1697f79d1579)

[Glide 这样用，更省内存！！！](https://juejin.im/post/59cf0f9e6fb9a00a4b0c73d4)

[Android 图片加载缓存问题：为什么你的Glide缓存没有起作用？](https://juejin.im/post/5ad3fd336fb9a028b5485713)

[Glide 核心设计二: 缓存管理](https://juejin.im/post/58b42cb9ac502e0069e0e370)

[Android glide使用过程中遇到的坑(进阶篇)](https://www.jianshu.com/p/deccde405e04)

[Android 高清加载巨图方案 拒绝压缩图片](http://www.jcodecraeer.com/a/anzhuokaifa/androidkaifa/2015/1021/3607.html)
## 图片Bitmap内存优化

#### 基础知识点
1. 一张图片占据多少内存？ 
2. 一张图片占用内存多少跟什么因素有关？
3. 一个像素占据多少字节？ARGB_8888占用4个字节
4. 如何获取当前手机可用内存
5. 图片分辨率、当前机器的Density、图片放置的位置的关系
6. 获取图片大小 getrowBytes()


#### 决定 Bitmap 占用内存大小的因素
决定 Bitmap 占用内存大小的因素，和图片文件在磁盘上占用的空间一点关系都没有，总结来说，有以下几点：

1. 色彩格式：比如 ARGB_8888 、RGB_5555 这种，单位像素占的内存空间不同。

2. 图片本身的像素尺寸。

3. 图片文件存放的 Drawable 目录。xhdpi 和 xxhdpi 可是不一样的。

4. 目标设备的 densityDpi 值。

#### 优化指南

1. 统一的bitmap加载器，选择Glide、Fresco、Picasso中的一个作为图片加载框架。实际开发中加载到view的图片的大小不应该超过view的大小，图片加载框架默认会对图片进行缓存，按view实际大小加载。在开发中为了减少apk的大小，一般只放一套3X图片，但是这些图片在小分辨率的手机上直接加载就会出现内存浪费。统一的bitmap加载器就可以很好的解决该问题。

2. 图片存在像素浪费，对于.9图，美工可能在出图时在拉伸与非拉伸区域都有大量的像素重复。而这些图片是可以缩小，但并不影响显示效果。

3. inSampleSize:缩放比例，在把图片载入内存之前，我们需要计算一个合适的缩放比例，避免不必要的大图载入。

4. 选择ARGB_8888/RBG_565/ARGB_4444/ALPHA_8，存在很大差异。

5. inBitmap：这个参数用来实现Bitmap内存的复用，但复用存在一些限制，具体体现在：在Android 4.4之前只能重用相同大小的Bitmap的内存，而Android 4.4及以后版本则只要后来的Bitmap比之前的小即可。使用inBitmap参数前，每创建一个Bitmap对象都会分配一块内存供其使用，而使用了inBitmap参数后，多个Bitmap可以复用一块内存，这样可以提高性能。


##### 高效加载大图片
我们在编写Android程序的时候经常要用到许多图片，不同图片总是会有不同的形状、不同的大小，但在大多数情况下，这些图片都会大于我们程序所需要的大小。比如说系统图片库里展示的图片大都是用手机摄像头拍出来的，这些图片的分辨率会比我们手机屏幕的分辨率高得多。大家应该知道，我们编写的应用程序都是有一定内存限制的，程序占用了过高的内存就容易出现OOM(OutOfMemory)异常。我们可以通过下面的代码看出每个应用程序最高可用内存是多少。
> 	int maxMemory = (int) (Runtime.getRuntime().maxMemory() / 1024);  Log.d("TAG", "Max memory is " + maxMemory + "KB");

因此在展示高分辨率图片的时候，最好先将图片进行压缩。压缩后的图片大小应该和用来展示它的控件大小相近，在一个很小的ImageView上显示一张超大的图片不会带来任何视觉上的好处，但却会占用我们相当多宝贵的内存，而且在性能上还可能会带来负面影响。下面我们就来看一看，如何对一张大图片进行适当的压缩，让它能够以最佳大小显示的同时，还能防止OOM的出现。
图片压缩就要用到这个类BitmapFactory

BitmapFactory这个类提供了多个解析方法(decodeByteArray, decodeFile, decodeResource等)用于创建Bitmap对象，我们应该根据图片的来源选择合适的方法。比如:
SD卡中的图片可以使用decodeFile方法
网络上的图片可以使用decodeStream方法
资源文件中的图片可以使用decodeResource方法。
这些方法会尝试为已经构建的bitmap分配内存，这时就会很容易导致OOM出现。为此每一种解析方法都提供了一个可选的BitmapFactory.Options参数，将这个参数的inJustDecodeBounds属性设置为true就可以让解析方法禁止为bitmap分配内存，返回值也不再是一个`Bitmap对象，而是null。虽然Bitmap是null了，但是BitmapFactory.Options的outWidth、outHeight和outMimeType属性都会被赋值。这个技巧让我们可以在加载图片之前就获取到图片的长宽值和MIME类型，从而根据情况对图片进行压缩。如下代码所示：

```java
BitmapFactory.Options options = new BitmapFactory.Options();  
options.inJustDecodeBounds = true;  
BitmapFactory.decodeResource(getResources(), R.id.myimage, options); 
int imageHeight = options.outHeight; 
int imageWidth = options.outWidth;  
String imageType = options.outMimeType;
```

为了避免OOM异常，最好在解析每张图片的时候都先检查一下图片的大小，除非你非常信任图片的来源，保证这些图片都不会超出你程序的可用内存。
现在图片的大小已经知道了，我们就可以决定是把整张图片加载到内存中还是加载一个压缩版的图片到内存中。以下几个因素是我们需要考虑的：
预估一下加载整张图片所需占用的内存。
为了加载这一张图片你所愿意提供多少内存。
用于展示这张图片的控件的实际大小。
当前设备的屏幕尺寸和分辨率。
比如，你的ImageView只有128*96像素的大小，只是为了显示一张缩略图，这时候把一张1024X768像素的图片完全加载到内存中显然是不值得的。
那我们怎样才能对图片进行压缩呢？通过设置BitmapFactory.Options中inSampleSize的值就可以实现。比如我们有一张2048X1536像素的图片，将inSampleSize的值设置为4，就可以把这张图片压缩成512X384像素。原本加载这张图片需要占用13M的内存，压缩后就只需要占用0.75M了(假设图片是ARGB_8888类型，即每个像素点占用4个字节)。下面的方法可以根据传入的宽和高，计算出合适的inSampleSize值：

```java
public static int calculateInSampleSize 
    (BitmapFactory.Options options,  int reqWidth, int reqHeight) {  
    // 源图片的高度和宽度  
    final int height = options.outHeight;  
    final int width = options.outWidth;  
    int inSampleSize = 1;  
    if (height 〉 reqHeight || width 〉 reqWidth) {  
        // 计算出实际宽高和目标宽高的比率  
        final int heightRatio = Math.round((float) height / (float) reqHeight);  
        final int widthRatio = Math.round((float) width / (float) reqWidth);  
        // 选择宽和高中最小的比率作为inSampleSize的值，这样可以保证最终图片的宽和高  
        // 一定都会大于等于目标的宽和高。  
        inSampleSize = heightRatio ﹤ widthRatio ? heightRatio : widthRatio;  
    }  
    return inSampleSize;  
}
```

使用这个方法，首先你要将``BitmapFactory.Options的inJustDecodeBounds属性设置为true，解析一次图片。然后将BitmapFactory.Options连同期望的宽度和高度一起传递到到calculateInSampleSize方法中，就可以得到合适的inSampleSize值了。之后再解析一次图片，使用新获取到的inSampleSize值，并把inJustDecodeBounds设置为false，就可以得到压缩后的图片了。

```java
public static Bitmap decodeSampledBitmapFromResource
  (Resources res, int resId,  int reqWidth, int reqHeight) {  
    // 第一次解析将inJustDecodeBounds设置为true，来获取图片大小  
    final BitmapFactory.Options options = new BitmapFactory.Options();  
    options.inJustDecodeBounds = true;  
    BitmapFactory.decodeResource(res, resId, options);  
    // 调用上面定义的方法计算inSampleSize值  
    options.inSampleSize = calculateInSampleSize(options, reqWidth, reqHeight);  
    // 使用获取到的inSampleSize值再次解析图片  
    options.inJustDecodeBounds = false;  
    return BitmapFactory.decodeResource(res, resId, options);  
}
```

#### 线上监控

线上的内存监控一般都是一些大公司在做，例如美团的Probe还有微信最近开源的Matrix

#### 参考

[Android 开发绕不过的坑：你的 Bitmap 究竟占多大内存？](http://bugly.qq.com/bbs/forum.php?mod=viewthread&tid=498#rd)

[Android内存优化之图片优化](https://juejin.im/post/5af84f4b51882542714fdaa9?utm_source=gold_browser_extension)

[Android 内存优化](http://yefangqingchen.com/2017/04/01/Android-%E5%86%85%E5%AD%98%E4%BC%98%E5%8C%96/)

[Android代码内存优化建议-Android资源篇](https://xiaozhuanlan.com/topic/7154902863)

[Android APP内存优化之图片优化](https://zmywly8866.github.io/2015/07/01/android-reduce-app-memory-use.html)

[Android高效加载大图、多图避免程序OOM](https://xiaozhuanlan.com/topic/2084735916)

[Android 内存优化 图片优化](http://yefangqingchen.com/2017/04/01/Android-%E5%86%85%E5%AD%98%E4%BC%98%E5%8C%96/)

[关于Android中图片大小、内存占用与drawable文件夹关系的研究与分析](https://blog.csdn.net/zhaokaiqiang1992/article/details/49787117)

[Android性能优化：那些关于Bitmap优化的小事](https://juejin.im/entry/5aa873996fb9a028db586153)

[Android有效解决加载大图片时内存溢出的问题](http://www.cnblogs.com/wanqieddy/archive/2011/11/25/2263381.html)

[腾讯](http://mp.weixin.qq.com/s?__biz=MzAxMzYyNDkyNA==&mid=2651332083&idx=1&sn=d5a1b24736d6f14ff24dfecf15e397a9&scene=0#wechat_redirect)


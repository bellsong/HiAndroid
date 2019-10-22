## Android启动速度优化实战

#### 现象
#### 原理
#### 工具
#### 方法
#### 原则

#### 现象

启动时间是APP一个重要的用户体验环节。用户接触App第一印象肯定是启动速度，如果启动速度慢则严重影响到App在用户心中的形象。因此手机启动速度是至关重要的，要求越快越好。

在CPU、内存以及IO的限制下，启动常见的问题有

1. 启动界面卡太久

2. 点击App图标后进入首页白屏

3. 从手指按下桌面图标开始，先"静止"一段时间，然后再启动，中间一点白色的间隙也没有

这其中的原因分别是什么呢？

#### 原理

App启动分两种：冷启动 热启动

1. 冷启动：当启动应用时，后台没有该应用的进程，这时系统会重新创建一个新的进程分配给该应用，这个启动方式就是冷启动。

    特点：冷启动因为系统会重新创建一个新的进程分配给它，所以会先创建和初始化application类，再创建和初始化MainActivity类（包括一系列的测量、布局、绘制），最后显示在界面上。

2. 热启动：当启动应用时，后台已有该应用的进程（例：按back键、home键，应用虽然会退出，但是该应用的进程是依然会保留在后台，可进入任务列表查看），所以在已有进程的情况下，这种启动会从已有的进程中来启动应用，这个方式叫热启动。

    特点：热启动因为会从已有的进程中来启动，所以热启动就不会走application这步了，而是直接走MainActivity（包括一系列的测量、布局、绘制），所以热启动的过程只需要创建和初始化一个MainActivity就行了，而不必创建和初始化application，因为一个应用从新进程的创建到进程的销毁，application只会初始化一次。

上面说到的白屏或者“静止”的原因在于Activity的setContentView，可以通过定义一个theme，然后给theme设置背景解决。

```java
<style name="StartTheme" parent="AppTheme">
    <item name="android:windowBackground">@mipmap/icon_splash</item>
</style>
```

注：setContentView的内部原理有兴趣的可以去看一下源码，看看在哪里耗时了。

《Android性能优化典范专题》介绍了Android性能问题的底层工作原理，以及如何通过工具来找出性能问题和提升建议。
主要从三方面展开：Andorid的渲染机制、内存和GC，电量优化

* 渲染性能
大多数用户感知到的卡顿等性能问题的最主要根源都是因为渲染性能。从产品设计的角度，希望App能够有更多的动画，图片等时尚元素来实现流畅的用 户体验。但是Android系统很有可能无法及时完成那些复杂的界面渲染操作。Android系统每隔16ms发出VSYNC信号，触发对UI进行渲染， 如果每次渲染都成功，就能够达到流畅的画面所需要的60fps，为了能够实现60fps，这意味着程序的大多数操作都必须在16ms内完成。

如果你的某个操作花费时间是24ms，系统在得到VSYNC信号的时候就无法进行正常渲染，这样就发生了丢帧现象。那么用户在32ms内看到的会是同一帧画面。

用户容易在UI执行动画或者滑动ListView的时候感知到卡顿不流畅，是因为这里的操作相对复杂，容易发生丢帧的现象，从而感觉卡顿。有很多原 因可以导致丢帧，也许是因为你的layout太过复杂，无法在16ms内完成渲染，有可能是因为你的UI上有层叠太多的绘制单元，还有可能是因为动画执行 的次数过多。这些都会导致CPU或者GPU负载过重。

我们可以通过一些工具来定位问题，比如可以使用HierarchyViewer来查找Activity中的布局是否过于复杂，也可以使用手机设置里 面的开发者选项，打开Show GPU Overdraw等选项进行观察。你还可以使用TraceView来观察CPU的执行情况，更加快捷的找到性能瓶颈。

* 过度绘制
Overdraw(过度绘制)描述的是屏幕上的某个像素在同一帧的时间内被绘制了多次。在多层次的UI结构里面，如果不可见的UI也在做绘制的操作，这就会导致某些像素区域被绘制了多次。这就浪费大量的CPU以及GPU资源。

当设计上追求更华丽的视觉效果的时候，我们就容易陷入采用越来越多的层叠组件来实现这种视觉效果的怪圈。这很容易导致大量的性能问题，为了获得最佳的性能，我们必须尽量减少Overdraw的情况发生。

幸运的是，我们可以通过手机设置里面的开发者选项，打开Show GPU Overdraw的选项，可以观察UI上的Overdraw情况。

蓝色，淡绿，淡红，深红代表了4种不同程度的Overdraw情况，我们的目标就是尽量减少红色Overdraw，看到更多的蓝色区域。

Overdraw有时候是因为你的UI布局存在大量重叠的部分，还有的时候是因为非必须的重叠背景。例如某个Activity有一个背景，然后里面 的Layout又有自己的背景，同时子View又分别有自己的背景。仅仅是通过移除非必须的背景图片，这就能够减少大量的红色Overdraw区域，增加 蓝色区域的占比。这一措施能够显著提升程序性能。

* Understanding VSYNC
为了理解App是如何进行渲染的，我们必须了解手机硬件是如何工作，那么就必须理解什么是VSYNC。

在讲解VSYNC之前，我们需要了解两个相关的概念：

Refresh Rate：代表了屏幕在一秒内刷新屏幕的次数，这取决于硬件的固定参数，例如60Hz。

Frame Rate：代表了GPU在一秒内绘制操作的帧数，例如30fps，60fps。

GPU会获取图形数据进行渲染，然后硬件负责把渲染后的内容呈现到屏幕上，他们两者不停的进行协作。

不幸的是，刷新频率和帧率并不是总能够保持相同的节奏。如果发生帧率与刷新频率不一致的情况，就会容易出现Tearing的现象(画面上下两部分显示内容发生断裂，来自不同的两帧数据发生重叠)。

理解图像渲染里面的双重与三重缓存机制，这个概念比较复杂，请移步查看这里：http://source.android.com/devices/graphics/index.html，还有这里http://article.yeeyan.org/view/37503/304664。

通常来说，帧率超过刷新频率只是一种理想的状况，在超过60fps的情况下，GPU所产生的帧数据会因为等待VSYNC的刷新信息而被Hold住，这样能够保持每次刷新都有实际的新的数据可以显示。但是我们遇到更多的情况是帧率小于刷新频率。

在这种情况下，某些帧显示的画面内容就会与上一帧的画面相同。糟糕的事情是，帧率从超过60fps突然掉到60fps以下，这样就会发生LAG，JANK，HITCHING等卡顿掉帧的不顺滑的情况。这也是用户感受不好的原因所在。

3)Tool:Profile GPU Rendering
性能问题如此的麻烦，幸好我们可以有工具来进行调试。打开手机里面的开发者选项，选择Profile GPU Rendering，选中On screen as bars的选项。

选择了这样以后，我们可以在手机画面上看到丰富的GPU绘制图形信息，分别关于StatusBar，NavBar，激活的程序Activity区域的GPU Rending信息。
screenshot.png

随着界面的刷新，界面上会滚动显示垂直的柱状图来表示每帧画面所需要渲染的时间，柱状图越高表示花费的渲染时间越长。
screenshot.png

中间有一根绿色的横线，代表16ms，我们需要确保每一帧花费的总时间都低于这条横线，这样才能够避免出现卡顿的问题。
screenshot.png

每一条柱状线都包含三部分，蓝色代表测量绘制Display List的时间，红色代表OpenGL渲染Display List所需要的时间，黄色代表CPU等待GPU处理的时间。

4)Why 60fps?
我们通常都会提到60fps与16ms，可是知道为何会是以程序是否达到60fps来作为App性能的衡量标准吗？这是因为人眼与大脑之间的协作无法感知超过60fps的画面更新。

12fps大概类似手动快速翻动书籍的帧率，这明显是可以感知到不够顺滑的。24fps使得人眼感知的是连续线性的运动，这其实是归功于运动模糊的 效果。24fps是电影胶圈通常使用的帧率，因为这个帧率已经足够支撑大部分电影画面需要表达的内容，同时能够最大的减少费用支出。但是低于30fps是 无法顺畅表现绚丽的画面内容的，此时就需要用到60fps来达到想要的效果，当然超过60fps是没有必要的。

开发app的性能目标就是保持60fps，这意味着每一帧你只有16ms=1000/60的时间来处理所有的任务。

5)Android, UI and the GPU
了解Android是如何利用GPU进行画面渲染有助于我们更好的理解性能问题。那么一个最实际的问题是：activity的画面是如何绘制到屏幕上的？那些复杂的XML布局文件又是如何能够被识别并绘制出来的？

Resterization栅格化是绘制那些Button，Shape，Path，String，Bitmap等组件最基础的操作。它把那些组件拆分到不同的像素上进行显示。这是一个很费时的操作，GPU的引入就是为了加快栅格化的操作。

CPU负责把UI组件计算成Polygons，Texture纹理，然后交给GPU进行栅格化渲染。

然而每次从CPU转移到GPU是一件很麻烦的事情，所幸的是OpenGL ES可以把那些需要渲染的纹理Hold在GPU Memory里面，在下次需要渲染的时候直接进行操作。所以如果你更新了GPU所hold住的纹理内容，那么之前保存的状态就丢失了。

在Android里面那些由主题所提供的资源，例如Bitmaps，Drawables都是一起打包到统一的Texture纹理当中，然后再传递到 GPU里面，这意味着每次你需要使用这些资源的时候，都是直接从纹理里面进行获取渲染的。当然随着UI组件的越来越丰富，有了更多演变的形态。例如显示图 片的时候，需要先经过CPU的计算加载到内存中，然后传递给GPU进行渲染。文字的显示更加复杂，需要先经过CPU换算成纹理，然后再交给GPU进行渲 染，回到CPU绘制单个字符的时候，再重新引用经过GPU渲染的内容。动画则是一个更加复杂的操作流程。

为了能够使得App流畅，我们需要在每一帧16ms以内处理完所有的CPU与GPU计算，绘制，渲染等等操作。

6)Invalidations, Layouts, and Performance
顺滑精妙的动画是app设计里面最重要的元素之一，这些动画能够显著提升用户体验。下面会讲解Android系统是如何处理UI组件的更新操作的。

通常来说，Android需要把XML布局文件转换成GPU能够识别并绘制的对象。这个操作是在DisplayList的帮助下完成的。DisplayList持有所有将要交给GPU绘制到屏幕上的数据信息。

在某个View第一次需要被渲染时，DisplayList会因此而被创建，当这个View要显示到屏幕上时，我们会执行GPU的绘制指令来进行渲 染。如果你在后续有执行类似移动这个View的位置等操作而需要再次渲染这个View时，我们就仅仅需要额外操作一次渲染指令就够了。然而如果你修改了 View中的某些可见组件，那么之前的DisplayList就无法继续使用了，我们需要回头重新创建一个DisplayList并且重新执行渲染指令并 更新到屏幕上。

需要注意的是：任何时候View中的绘制内容发生变化时，都会重新执行创建DisplayList，渲染DisplayList，更新到屏幕上等一 系列操作。这个流程的表现性能取决于你的View的复杂程度，View的状态变化以及渲染管道的执行性能。举个例子，假设某个Button的大小需要增大 到目前的两倍，在增大Button大小之前，需要通过父View重新计算并摆放其他子View的位置。修改View的大小会触发整个 HierarcyView的重新计算大小的操作。如果是修改View的位置则会触发HierarchView重新计算其他View的位置。如果布局很复 杂，这就会很容易导致严重的性能问题。我们需要尽量减少Overdraw。

我们可以通过前面介绍的Monitor GPU Rendering来查看渲染的表现性能如何，另外也可以通过开发者选项里面的Show GPU view updates来查看视图更新的操作，最后我们还可以通过HierarchyViewer这个工具来查看布局，使得布局尽量扁平化，移除非必需的UI组 件，这些操作能够减少Measure，Layout的计算时间。

7)Overdraw, Cliprect, QuickReject
引起性能问题的一个很重要的方面是因为过多复杂的绘制操作。我们可以通过工具来检测并修复标准UI组件的Overdraw问题，但是针对高度自定义的UI组件则显得有些力不从心。

有一个窍门是我们可以通过执行几个APIs方法来显著提升绘制操作的性能。前面有提到过，非可见的UI组件进行绘制更新会导致Overdraw。例 如Nav Drawer从前置可见的Activity滑出之后，如果还继续绘制那些在Nav Drawer里面不可见的UI组件，这就导致了Overdraw。为了解决这个问题，Android系统会通过避免绘制那些完全不可见的组件来尽量减少 Overdraw。那些Nav Drawer里面不可见的View就不会被执行浪费资源。

但是不幸的是，对于那些过于复杂的自定义的View(重写了onDraw方法)，Android系统无法检测具体在onDraw里面会执行什么操作，系统无法监控并自动优化，也就无法避免Overdraw了。但是我们可以通过canvas.clipRect()来 帮助系统识别那些可见的区域。这个方法可以指定一块矩形区域，只有在这个区域内才会被绘制，其他的区域会被忽视。这个API可以很好的帮助那些有多组重叠 组件的自定义View来控制显示的区域。同时clipRect方法还可以帮助节约CPU与GPU资源，在clipRect区域之外的绘制指令都不会被执行，那些部分内容在矩形区域内的组件，仍然会得到绘制。

除了clipRect方法之外，我们还可以使用canvas.quickreject()来判断是否没和某个矩形相交，从而跳过那些非矩形区域内的绘制操作。做了那些优化之后，我们可以通过上面介绍的Show GPU Overdraw来查看效果。

#### 工具

用vmstat采集的启动场景线程情况，线程数量和java线程切换的频率

#### 方法

1. 启动速度测量方法

实用工具 TraceView
分阶段排查，找出各个占用CPU的耗时点。
注意点：
整体的Trace，观察整个时间段情况。找出明显点耗时。
分小段，多次。因为时间过长容易把耗时占比高的稀释掉。加上线程启动时间差等叠加原因都会对性能产生影响。

推荐选择打点位置：开始：Application attachBaseContext，结束：核心View的dispatchDraw结束。

2. 优化思路

    针对第一次启动优化：

    1. 按开始初始化时间点进行分类：
        
        a. 立即初始化(又分为两种，必须在UI线程里初始化，不必须在UI线程里初始化)
        
        b. 可以首页加载后初始化
        
        c. 可以在第一次调用时初始化
    
    2. 欢迎界面提前初始化并展示
    
    3. profile查找性能瓶颈进行优化

    针对第二次启动优化:

    4. 对首页数据进行缓存
    
    5. 对网络请求支持AliEtag，返回304时则可以减少网络传输数据时间


优化方法

1、通过style 设置一个默认的启动图来过度，从交互体验上来提高启动速度

2、分析application和首屏的业务逻辑异步初始化第三方组件，防止阻塞主线程（或者延迟初始化（用的时候再初始化））

3、闪屏的2秒停顿可以利用起来，把一些耗时操作延迟到这里来初始化

4、同工具DDMS中的TraceView来检测耗时的点在哪里，做针对的处理

5、mainActivity的onCreate流程，特别是UI的布局与渲染操作，如果布局过于复杂很可能导致严重的启动性能问题；（可以考虑先把mainActivity需要的数据请求回来），根据首页的结构可以考虑懒加载。

## Threads
分析线程情况，当前启动的线程以及执行情况

优化步骤：
1. 线程
1） 分析线程数，检查线程池合理数。 去掉不必要的线程和线程池。控制线程的并发数。

2）延迟非必要线程的启动时机

3）降低线程优先级

4）统一线程池

注意：规范新线程的命名，方便后续排查

2. 减少GC

减少非必要缓存，频繁创建的对象如网络库和图片库的Byte数组 Buffer等做复用。

3. 主线程优化

4. 减少IO
空间换时间

SP优化
数据库操作 开启事务

5. 延时加载

6. 功能降级

7. 优化dex加载

8. Java类加载过程

#### 原则

提高app的启动速度，加快Application的执行时间也是一个很重要的方面，这里总结了几条原则：

* 尽量不将一些业务逻辑放于Application中；

* Application尽量不以静态变量的方式保存应用数据；

* 若App的大小不是特别大无需使用dex分包方案；

* 在Application中关于文件，数据库的操作尽量异步的方式

每当处理或者排查性能问题的时候，遵循这些原则：

持续测量： 用你的眼睛做优化从来就不是一个好主意。同一个动画看了几遍之后，你会开始想像它运行地越来越快。数字从来都不说谎。使用我们即将讨论的工具，在你做改动的前后多次测量应用程序的性能。
使用慢速设备：如果你真的想让所有的薄弱环节都暴露出来，慢速设备会给你更多帮助。有的性能问题也许不会出现在更新更强大的设备上，但不是所有的用户都会使用最新和最好的设备。
权衡利弊 ：性能优化完全是权衡的问题。你优化了一个东西 —— 往往是以损害另一个东西为代价的。很多情况下，损害的另一个东西可能是查找和修复问题的时间，也可能是位图的质量，或者是应该在一个特定数据结构中存储的大量数据。你要随时做好取舍的准备。


一般占用CPU时间，需要留意的地方：
1. 线程（一般用于初始化其他模块）
2. 网络请求 
3. 日志输出
4. GC 可能引起GC的原因：主线程中字符串拼接和扩容，容器的遍历和扩容，inflate界面；网络线程和图片加载频繁分配Byte，图片解码，HashMap操作

## 参考

### 启动时间统计

[如何统计Android App启动时间](https://www.jianshu.com/p/59a2ca7df681)

[Android 开发之 App 启动时间统计](https://www.jianshu.com/p/c967653a9468)

[应用启动速度(Launch-Time)的优化](https://www.jianshu.com/p/56971f2cf0ec)

[Android踩坑经验--App启动时间正确统计姿势](https://blog.csdn.net/longlong2015/article/details/88825956)

[Android：获取APP启动时间的踩坑经历](https://www.jianshu.com/p/c20743a0650e)

[App启动时间（翻译）](https://juejin.im/post/5bb082675188255ca1538e0e)

[【Android端 APP 启动时长获取】启动时长获取方案及具体实施](https://www.cnblogs.com/keke-xiaoxiami/p/6180563.html)

[Activity到底是什么时候显示到屏幕上的呢？](http://blog.desmondyao.com/android-show-time/)

### 启动优化方法

[Android 启动速度优化](https://juejin.im/entry/582adaad570c35006cdcb615)

[Android性能优化笔记（一）——启动优化](https://juejin.im/post/5c21ea325188254eaa5c45b1)

[Android 性能优化之启动优化](https://juejin.im/post/5d82020cf265da03d21176cb?utm_source=gold_browser_extension)

[Android应用或界面启动时间性能](https://blog.csdn.net/hdandan2015/article/details/78519797)

[胡凯，腾讯开发者，翻译了一系列的Google Android性能优化典范的文章](http://hukai.me/)

[Android性能优化（一）之启动加速35%](https://juejin.im/post/5874bff0128fe1006b443fa0)

[应用程序启动速度提升60% !](https://juejin.im/entry/5b8134cdf265da434a1fce4b?utm_source=gold_browser_extension)

[【掌阅出品】android 提升布局加载速度200%（X2C）](https://www.jianshu.com/p/c1b9ce20ceb3)

性能优化
http://hukai.me/
[胡凯，腾讯开发者，翻译了一系列的Google Android性能优化典范的文章。]()

https://hujiaweibujidao.github.io/
Hujiawei，魅族开发者，博客最近经常更新Android性能数据搜集统计的相关的文章，本人受益匪浅。

[一触即发 App启动优化最佳实践](https://zhuanlan.zhihu.com/p/23442027)

[android面试题-App应用启动分析与优化](https://www.jianshu.com/p/f0f73fefdd43)


[Android性能优化典范 - 第6季](https://mp.weixin.qq.com/s?__biz=MzA3NTYzODYzMg==&mid=2653578016&idx=1&sn=d997d1142bac09e3764c075392468ae5&chksm=84b3b127b3c4383197c7d1cf15ecec44d66a1119b033ae383f9e2126bb1be0abc93416622dc0&scene=21#wechat_redirect)

[写启动界面Splash的正确姿势,解决启动白屏](http://www.jianshu.com/p/cd6ef8d3d74d)

[Android开发之解决APP启动白屏或者黑屏闪现的问题](http://blog.csdn.net/minimicall/article/details/39854969)

[Android app启动时黑屏或者白屏的原因及解决办法](http://www.manongjc.com/article/1221.html)

[Android 启动APP时黑屏白屏的三个解决方案](http://www.cnblogs.com/liqw/p/4263418.html)

[冷启动秒开](http://www.jianshu.com/p/03c0fd3fc245)

[Android 官方关于启动说明](https://developer.android.com/topic/performance/vitals/launch-time)

[Android应用启动界面分析（Starting Window）](http://blog.csdn.net/xueshanhaizi/article/details/51262528)

[Android冷启动白屏解析，带你一步步分析和解决问题](http://blog.csdn.net/guolin_blog/article/details/51019856)

[Android应用启动优化:一种DelayLoad的实现和原理(上篇)](http://androidperformance.com/2015/11/18/Android-app-lunch-optimize-delay-load.html)

[如何统计Android App启动时间](https://cloud.tencent.com/developer/article/1371846)

[手机淘宝性能优化全记录](https://yq.aliyun.com/articles/2696?spm=a2c4e.11155435.0.0.2a91451fYsRxAr)
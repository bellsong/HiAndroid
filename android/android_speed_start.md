## 启动速度优化专项

#### 现象
#### 原理
#### 工具
#### 方法
#### 原则

#### 现象
一般Android启动白屏

在手指按下桌面图标的时候，先"静止"一段时间，然后再启动，而且中间一点白色的间隙也没有，又是为什么？

启动分两种：冷启动 热启动
>
1、冷启动：当启动应用时，后台没有该应用的进程，这时系统会重新创建一个新的进程分配给该应用，这个启动方式就是冷启动。

特点：冷启动因为系统会重新创建一个新的进程分配给它，所以会先创建和初始化application类，再创建和初始化MainActivity类（包括一系列的测量、布局、绘制），最后显示在界面上。

2、热启动：当启动应用时，后台已有该应用的进程（例：按back键、home键，应用虽然会退出，但是该应用的进程是依然会保留在后台，可进入任务列表查看），所以在已有进程的情况下，这种启动会从已有的进程中来启动应用，这个方式叫热启动。

特点：热启动因为会从已有的进程中来启动，所以热启动就不会走application这步了，而是直接走MainActivity（包括一系列的测量、布局、绘制），所以热启动的过程只需要创建和初始化一个MainActivity就行了，而不必创建和初始化application，因为一个应用从新进程的创建到进程的销毁，application只会初始化一次。

既然上述问题不是出在application，那么肯定就是在Activity了，我是这么想的，然后我就想着是不是SetContentView的时候花了很多时间呢？然后我又测试了一遍：

---
@Override
protected void onCreate(Bundle savedInstanceState) {
    super.onCreate(savedInstanceState);
    long startTime = System.currentTimeMillis();
    setContentView(R.layout.activity_start);
    Log.d(TAG, "time===" + (System.currentTimeMillis() - startTime));
}


然后打印出来的时间是：
time = 210

果真是setContentView导致的，那就很好解决了,我们不要setContentView就好了，可能还有人要问了，你不要setContentView你咋加载布局呢？别急，别忘了还有theme这个好东西啊！我们可以定义一个theme，然后给theme设置背景就好了：
<style name="StartTheme" parent="AppTheme">
    <item name="android:windowBackground">@mipmap/icon_splash</item>
</style>

注：setContentView的内部原理有兴趣的同学可以自己去百度看看，看看在哪里耗时了

提个建议：当初我也尝试过是这么做的，但有个问题，图片的内存会释放不掉，所以放在activity的super调用前，用流资源方式加载图片，设置到window的背景中去就好了

* [胡凯，腾讯开发者，翻译了一系列的Google Android性能优化典范的文章](http://hukai.me/)

[Android性能优化（一）之启动加速35%](https://juejin.im/post/5874bff0128fe1006b443fa0)

[应用程序启动速度提升60% !](https://juejin.im/entry/5b8134cdf265da434a1fce4b?utm_source=gold_browser_extension)

[【掌阅出品】android 提升布局加载速度200%（X2C）](https://www.jianshu.com/p/c1b9ce20ceb3)

#### 原理
[Android 官方关于启动说明](https://developer.android.com/topic/performance/vitals/launch-time)
[Android应用启动界面分析（Starting Window）](http://blog.csdn.net/xueshanhaizi/article/details/51262528)
[Android冷启动白屏解析，带你一步步分析和解决问题](http://blog.csdn.net/guolin_blog/article/details/51019856)
[Android应用启动优化:一种DelayLoad的实现和原理(上篇)](http://androidperformance.com/2015/11/18/Android-app-lunch-optimize-delay-load.html)

#### 原则
所以提高app的启动速度，加快Application的执行时间也是一个很重要的方面，这里我暂时总结了几条原则：

* 尽量不将一些业务逻辑放于Application中；

* Application尽量不以静态变量的方式保存应用数据；

* 若App的大小不是特别大无需使用dex分包方案；

* 在Application中关于文件，数据库的操作尽量

### 参考

#### 解决方案
[写启动界面Splash的正确姿势,解决启动白屏](http://www.jianshu.com/p/cd6ef8d3d74d)

[Android开发之解决APP启动白屏或者黑屏闪现的问题](http://blog.csdn.net/minimicall/article/details/39854969)

[Android app启动时黑屏或者白屏的原因及解决办法](http://www.manongjc.com/article/1221.html)

[Android 启动APP时黑屏白屏的三个解决方案](http://www.cnblogs.com/liqw/p/4263418.html)

  * [冷启动秒开](http://www.jianshu.com/p/03c0fd3fc245)


## 启动速度优化

#### 原理
#### 工具
#### 方法
#### 案例

#### Android性能优化典范专题
介绍了Android性能问题的底层工作原理，以及如何通过工具来找出性能问题和提升建议。
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

8)Memory Churn and performance
虽然Android有自动管理内存的机制，但是对内存的不恰当使用仍然容易引起严重的性能问题。在同一帧里面创建过多的对象是件需要特别引起注意的事情。

Android系统里面有一个Generational Heap Memory的模型，系统会根据内存中不同 的内存数据类型分别执行不同的GC操作。例如，最近刚分配的对象会放在Young Generation区域，这个区域的对象通常都是会快速被创建并且很快被销毁回收的，同时这个区域的GC操作速度也是比Old Generation区域的GC操作速度更快的。

除了速度差异之外，执行GC操作的时候，任何线程的任何操作都会需要暂停，等待GC操作完成之后，其他操作才能够继续运行。

通常来说，单个的GC并不会占用太多时间，但是大量不停的GC操作则会显著占用帧间隔时间(16ms)。如果在帧间隔时间里面做了过多的GC操作，那么自然其他类似计算，渲染等操作的可用时间就变得少了。

导致GC频繁执行有两个原因：

Memory Churn内存抖动，内存抖动是因为大量的对象被创建又在短时间内马上被释放。

瞬间产生大量的对象会严重占用Young Generation的内存区域，当达到阀值，剩余空间不够的时候，也会触发GC。即使每次分配的对象占用了很少的内存，但是他们叠加在一起会增加 Heap的压力，从而触发更多其他类型的GC。这个操作有可能会影响到帧率，并使得用户感知到性能问题。
screenshot.png

解决上面的问题有简洁直观方法，如果你在Memory Monitor里面查看到短时间发生了多次内存的涨跌，这意味着很有可能发生了内存抖动。

同时我们还可以通过Allocation Tracker来查看在短时间内，同一个栈中不断进出的相同对象。这是内存抖动的典型信号之一。

当你大致定位问题之后，接下去的问题修复也就显得相对直接简单了。例如，你需要避免在for循环里面分配对象占用内存，需要尝试把对象的创建移到循 环体之外，自定义View中的onDraw方法也需要引起注意，每次屏幕发生绘制以及动画执行过程中，onDraw方法都会被调用到，避免在onDraw 方法里面执行复杂的操作，避免创建对象。对于那些无法避免需要创建对象的情况，我们可以考虑对象池模型，通过对象池来解决频繁创建与销毁的问题，但是这里 需要注意结束使用之后，需要手动释放对象池中的对象。

9)Garbage Collection in Android
JVM的回收机制给开发人员带来很大的好处，不用时刻处理对象的分配与回收，可以更加专注于更加高级的代码实现。相比起Java，C与C++等语言 具备更高的执行效率，他们需要开发人员自己关注对象的分配与回收，但是在一个庞大的系统当中，还是免不了经常发生部分对象忘记回收的情况，这就是内存泄 漏。

原始JVM中的GC机制在Android中得到了很大程度上的优化。Android里面是一个三级Generation的内存模型，最近分配的对象 会存放在Young Generation区域，当这个对象在这个区域停留的时间达到一定程度，它会被移动到Old Generation，最后到Permanent Generation区域。

每一个级别的内存区域都有固定的大小，此后不断有新的对象被分配到此区域，当这些对象总的大小快达到这一级别内存区域的阀值时，会触发GC的操作，以便腾出空间来存放其他新的对象。
screenshot.png

前面提到过每次GC发生的时候，所有的线程都是暂停状态的。GC所占用的时间和它是哪一个Generation也有关系，Young Generation的每次GC操作时间是最短的，Old Generation其次，Permanent Generation最长。执行时间的长短也和当前Generation中的对象数量有关，遍历查找20000个对象比起遍历50个对象自然是要慢很多 的。

虽然Google的工程师在尽量缩短每次GC所花费的时间，但是特别注意GC引起的性能问题还是很有必要。如果不小心在最小的for循环单元里面执 行了创建对象的操作，这将很容易引起GC并导致性能问题。通过Memory Monitor我们可以查看到内存的占用情况，每一次瞬间的内存降低都是因为此时发生了GC操作，如果在短时间内发生大量的内存上涨与降低的事件，这说明 很有可能这里有性能问题。我们还可以通过Heap and Allocation Tracker工具来查看此时内存中分配的到底有哪些对象。

10)Performance Cost of Memory Leaks
虽然Java有自动回收的机制，可是这不意味着Java中不存在内存泄漏的问题，而内存泄漏会很容易导致严重的性能问题。

内存泄漏指的是那些程序不再使用的对象无法被GC识别，这样就导致这个对象一直留在内存当中，占用了宝贵的内存空间。显然，这还使得每级Generation的内存区域可用空间变小，GC就会更容易被触发，从而引起性能问题。

寻找内存泄漏并修复这个漏洞是件很棘手的事情，你需要对执行的代码很熟悉，清楚的知道在特定环境下是如何运行的，然后仔细排查。例如，你想知道程序 中的某个activity退出的时候，它之前所占用的内存是否有完整的释放干净了？首先你需要在activity处于前台的时候使用Heap Tool获取一份当前状态的内存快照，然后你需要创建一个几乎不这么占用内存的空白activity用来给前一个Activity进行跳转，其次在跳转到 这个空白的activity的时候主动调用System.gc()方法来确保触发一个GC操作。最后，如果前面这个activity的内存都有全部正确释 放，那么在空白activity被启动之后的内存快照中应该不会有前面那个activity中的任何对象了。
screenshot.png

如果你发现在空白activity的内存快照中有一些可疑的没有被释放的对象存在，那么接下去就应该使用Alocation Track Tool来仔细查找具体的可疑对象。我们可以从空白activity开始监听，启动到观察activity，然后再回到空白activity结束监听。这样操作以后，我们可以仔细观察那些对象，找出内存泄漏的真凶。
screenshot.png

11)Memory Performance
通常来说，Android对GC做了大量的优化操作，虽然执行GC操作的时候会暂停其他任务，可是大多数情况下，GC操作还是相对很安静并且高效的。但是如果我们对内存的使用不恰当，导致GC频繁执行，这样就会引起不小的性能问题。

为了寻找内存的性能问题，Android Studio提供了工具来帮助开发者。

Memory Monitor：查看整个app所占用的内存，以及发生GC的时刻，短时间内发生大量的GC操作是一个危险的信号。

Allocation Tracker：使用此工具来追踪内存的分配，前面有提到过。

Heap Tool：查看当前内存快照，便于对比分析哪些对象有可能是泄漏了的，请参考前面的Case。

12)Tool – Memory Monitor
Android Studio中的Memory Monitor可以很好的帮组我们查看程序的内存使用情况。

我的原则
每当处理或者排查性能问题的时候，我都遵循这些原则：

持续测量： 用你的眼睛做优化从来就不是一个好主意。同一个动画看了几遍之后，你会开始想像它运行地越来越快。数字从来都不说谎。使用我们即将讨论的工具，在你做改动的前后多次测量应用程序的性能。
使用慢速设备：如果你真的想让所有的薄弱环节都暴露出来，慢速设备会给你更多帮助。有的性能问题也许不会出现在更新更强大的设备上，但不是所有的用户都会使用最新和最好的设备。
权衡利弊 ：性能优化完全是权衡的问题。你优化了一个东西 —— 往往是以损害另一个东西为代价的。很多情况下，损害的另一个东西可能是查找和修复问题的时间，也可能是位图的质量，或者是应该在一个特定数据结构中存储的大量数据。你要随时做好取舍的准备。

支付宝安卓客户端启动速度的优化工作 《垃圾回收篇》https://www.atatech.org/articles/62980

通过优化Davild，抑制GC


原则：
用户接触App第一印象肯定是启动速度，如果启动速度慢则严重影响到App在用户心中的形象。因此手机启动速度是至关重要的，要求越快越好。

阿里巴巴主客优化整体思路如下：

针对第一次启动优化：
1.按开始初始化时间点进行分类：1.立即初始化(又分为两种，必须在UI线程里初始化，不必须在UI线程里初始化)，2.可以首页加载后初始化，3.可以在第一次调用时初始化
2.欢迎界面提前初始化并展示
3.profile查找性能瓶颈进行优化

针对第二次启动优化:
4.对首页数据进行缓存
5.对网络请求支持AliEtag，返回304时则可以减少网络传输数据时间


Android性能优化Tips整理
如何优化
优化本身是一个很大的主题，对于Android平台优化可以为以下几部分：
一是JAVA语法层次通用的优化，如尽量使用局部变量（栈变量），IO缓冲等。
二是通用的Android性能优化，如同步改异步，各种缓存的使用等
三是应用程序内部的性能优化，如内部逻辑、数据插入及查找、数据结构的安排与组织等

一、JAVA语法层次的优化
1. 类和对象使用技巧
1.  尽量少用new生成新对象
2.  使用clone方法生成新对象
3.  尽量使用局部变量栈变量
4.  减少方法调用
5.  使用final类和final/static/private方法
6.  让访问实例内变量的 getter/setter 方法变成final  
7.  避免不需要的 instanceof 操作  
8.  避免不需要的造型操作  
9.  尽量重用对象
10. 不要重复初始化变量  
11. 不要过分创建对象
2. Java IO技巧
1.  使用缓冲提高IO性能
2.  lnputStream比Reader高效，OutputStream比Writer高效
3   在适当的时候用byte替代char
4.  有缓冲的块操作IO要比缓冲的流字符IO快
5.  序列化时使用原子类型
6.  在finally块中关闭stream 
7.  SQL语句
8.  尽早释放资源
3. 异常Exceptions使用技巧
1.  避免使用异常来控制程序流程
2.  尽可能重用异常
3.  将trycatch 块移出循环  
4. 线程使用技巧
1.  在使用大量线程Threading的场合使用线程池管理
2.  防止过多的同步
3.  同步方法而不要同步整个代码段
4.  在追求速度的场合用ArrayList和HashMap代替Vector和Hashtable
5.  使用notify而不是notifyAll
6.  不要在循环中调用 synchronized同步方法   
7.  单线程应尽量使用 HashMap，ArrayList
5. 其它常用技巧
*   使用移位操作替代乘除法操作可以极大地提高性能
*   对Vector中最后位置的添加删除操作要远远快于埘第一个元素的添加删除操作
*   当复制数组时使用System.arraycop方法
*   使用复合赋值运算符
*   用int而不用其它基本类型
*   在进行数据库连接和网络连接时使用连接池
*   用压缩加快网络传输速度一种常用方法是把相关文件打包到一个jar文件中
*   在数据库应用程序中使用批处理功能
*   消除循环体中不必要的代码
*   为vectors 和 hashtables定义初始大小  
*   如果只是查找单个字符的话用charat代替startswith
*   在字符串相加的时候使用 charat()代替startswith() 如果该字符串只有一个字符的话  
*   对于 boolean 值避免不必要的等式判断  
*   对于常量字符串用string 代替 stringbuffer   
*   用stringtokenizer 代替 indexof 和substring  
*   使用条件操作符替代if cond else  结构 
*   不要在循环体中实例化变量  
*   确定 stringbuffer的容量  
*   不要总是使用取反操作符  
*   与一个接口 进行instanceof 操作  
*   采用在需要的时候才开始创建的策略  
*   通过 StringBuffer 的构造函数来设定他的初始化容量可以明显提升性能  
*   合理使用 javautilVector
*   不要将数组声明为public static final
*   HaspMap 的遍历
*   array数组和 ArrayList 的使用  
*   StringBufferStringBuilder 的区别
*   尽量使用基本数据类型代替对象   
*   用简单的数值计算代替复杂的函数计算比如查表方式解决三角函数问题  
*   使用具体类比使用接口效率高但结构弹性降低了但现代 IDE都可以解决这个问题 
*   考虑使用静态方法
*   应尽可能避免使用内在的GET/SET 方法 
*   避免枚举浮点数的使用   
*   二维数组比一维数组占用更多的内存空间大概是 10倍计算 
*   SQLite
*   奇偶判断

二、通用Android性能优化
布局优化
（原文参考：ImprovingLayout Performance）
• 尽量减少Android程序布局中View的层次，View层次越多，效率就越低
• 使用<include/>复用布局
• 使用ViewStub懒加载布局 (TODO:Android布局技巧：使用ViewStub提高UI性能)
• 使用ViewHolder、Thread使ListView滚动更加流畅
其它优化点
• 合理使用异步操作
• 懒加载：当前不需要的数据，不要加载，即按需加载。懒加载的范围是广泛的，可以是数据，可以是View，或者其它
• 使用缓存
• 图片缓存：包括MemoryCache和DiskCache，推荐使用官方DEMO中的Cache
参考：DisplayingBitmaps Efficiently
• 单例数据缓存：建立一个管理数据的类，管理所有数据，当主界面消失后，由于Application本身没有实际退出，因此，数据本身也没有释放掉，下次启动时，省去了加载数据的时间，当然，这并不是一个好的行为。
• 使用ListView、GridView的View缓存
• 使用Message自身的缓存，避免重复创建Message实例
• 线程池
• 数据池（可参考Message Pool的实现方式）
• ……
• 数据库优化
• SQL优化
• 建立索引
• 使用事务
• …...
• 算法优化
• 用快速排序代替冒泡排序
• 用二分查找代替线性查找
• ……
• 数据结构使用
• 不要全部使用ArrayList，合理使用LinkedList等易于插入和删除的集合
• 合理使用HashMap、HashSet来提高查找性能
• 使用SparseArray、SparseIntArray、SparseBooleanArray来替代某些特定的HashMap
• ……
• 其它策略
• 可以考虑延迟处理，避免在同一时间干过多的事情
三、应用程序内部的性能优化
该部分的优化应该是依据程序的不同而不同，没有万般皆准的法则，实际上，上述的性能优化点基本上已经能够解决很多性能问题了。
主要的优化手段有：

• 程序逻辑简化：分析代码，去掉冗余逻辑
• 数据结构的优化：对集合类的灵活使用，特别是HashMap的使用，极大的提高查找性能。
• 批量处理原则：对于需要循环调用地方，采用批量处理

# 启动性能优化

[Android 开发之 App 启动时间统计](https://www.jianshu.com/p/c967653a9468)

[应用启动速度(Launch-Time)的优化](https://www.jianshu.com/p/56971f2cf0ec)

性能优化
http://hukai.me/
[胡凯，腾讯开发者，翻译了一系列的Google Android性能优化典范的文章。]()

[Android性能优化（一）之启动加速35%](https://juejin.im/post/5874bff0128fe1006b443fa0)

[应用程序启动速度提升60% !](https://juejin.im/entry/5b8134cdf265da434a1fce4b?utm_source=gold_browser_extension)

https://hujiaweibujidao.github.io/
Hujiawei，魅族开发者，博客最近经常更新Android性能数据搜集统计的相关的文章，本人受益匪浅。

[【掌阅出品】android 提升布局加载速度200%（X2C）](https://www.jianshu.com/p/c1b9ce20ceb3)

[一触即发 App启动优化最佳实践](https://zhuanlan.zhihu.com/p/23442027)

[android面试题-App应用启动分析与优化](https://www.jianshu.com/p/f0f73fefdd43)

[Activity到底是什么时候显示到屏幕上的呢？](http://blog.desmondyao.com/android-show-time/)

[Android性能优化典范 - 第6季](https://mp.weixin.qq.com/s?__biz=MzA3NTYzODYzMg==&mid=2653578016&idx=1&sn=d997d1142bac09e3764c075392468ae5&chksm=84b3b127b3c4383197c7d1cf15ecec44d66a1119b033ae383f9e2126bb1be0abc93416622dc0&scene=21#wechat_redirect)


Android应用优化方案

前言：

前面两篇文章主要是讲关于activity、fragment生命周期方面的总结，这篇文章主要是总结在android应用开发过程的优化方案，还有一些常用的优化工具。优化的方向包括：启动速度、界面流畅性、内存使用情况、apk体积、耗电量、流量等方面。

app启动速度

1、通过style 设置一个默认的启动图来过度，从交互体验上来提高启动速度

2、分析application和首屏的业务逻辑异步初始化第三方组件，防止阻塞主线程（或者延迟初始化（用的时候再初始化））

3、闪屏的2秒停顿可以利用起来，把一些耗时操作延迟到这里来初始化

4、同工具DDMS中的TraceView来检测耗时的点在哪里，做针对的处理

5、mainActivity的onCreate流程，特别是UI的布局与渲染操作，如果布局过于复杂很可能导致严重的启动性能问题；（可以考虑先把mainActivity需要的数据请求回来），根据首页的结构可以考虑懒加载。

Android APP启动优化： wuxiaolong.me/2017/03/13/…

App启动速度优化之耗时检测处理： www.jianshu.com/p/a0e242d57…

使用 TraceView 找到卡顿的元凶： blog.csdn.net/u011240877/…

上面的几篇文章基本上描述了应用的启动流程，如何优化白屏，检测耗时以及一些SDK的懒加载等等...


启用问题和优化思路

在CPU、内存以及IO的限制下，启动问题显得尤为严重。

主要问题：
1.启动界面卡太久
2.进入首页白屏

实用工具
## TraceView
分阶段排查，找出各个占用CPU的耗时点。
注意点：
整体的Trace，观察整个时间段情况。找出明显点耗时。
分小段，多次。因为时间过长容易把耗时占比高的稀释掉。加上线程启动时间差等叠加原因都会对性能产生影响。

一般占用CPU时间，需要留意的地方：
1. 线程（一般用于初始化其他模块）
2. 网络请求 
3. 日志输出
4. GC 可能引起GC的原因：主线程中字符串拼接和扩容，容器的遍历和扩容，inflate界面；网络线程和图片加载频繁分配Byte，图片解码，HashMap操作

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

---


#1、优化dex加载
## 背景
由于app的体量较大，超过了65536的天坑，于是我们的安装包被迫分为2个dex文件，第2个文件在第一次启动时会进行安装。（当然，这里暂不讨论为什么不精减到一个的问题，这是后面努力的方向），然而安装的过程在低端手机上惨不忍睹，普通需要4秒以上。我们的多DEX支持的代码是从Google官方的MultiDex和MultiDexExtractor中取出来的，那么这块是否还有优化空间了呢？
[http://www.atatech.org/articles/56465/?frm=mail_daily&uid=130616](http://www.atatech.org/articles/56465/?frm=mail_daily&uid=130616)

##现状
经过分析，安装过程中比较耗时的有2个过程，一是把classes2.dex从apk中取出来的，然后以zip的形式保存在程序目录下面，这个过程为extract。

private static void extract(ZipFile apk, ZipEntry dexFile, File extractTo,
            String extractedFilePrefix) throws IOException, FileNotFoundException {

        InputStream in = apk.getInputStream(dexFile);
        ZipOutputStream out = null;
        File tmp = File.createTempFile(extractedFilePrefix, EXTRACTED_SUFFIX,
                extractTo.getParentFile());

        TMLog.i(TAG, "Extracting " + tmp.getPath());
        try {
            out = new ZipOutputStream(new BufferedOutputStream(new FileOutputStream(tmp)));
            out.setLevel(Deflater.BEST_SPEED);
            try {
                ZipEntry classesDex = new ZipEntry("classes.dex");
                // keep zip entry time since it is the criteria used by Dalvik
                classesDex.setTime(dexFile.getTime());
                out.putNextEntry(classesDex);

                byte[] buffer = new byte[BUFFER_SIZE];
                int length = in.read(buffer);
                while (length != -1) {
                    out.write(buffer, 0, length);
                    length = in.read(buffer);
                }
                out.closeEntry();
            } finally {
                out.close();
            }
            TMLog.i(TAG, "Renaming to " + extractTo.getPath());
            if (!tmp.renameTo(extractTo)) {
                throw new IOException("Failed to rename \"" + tmp.getAbsolutePath() +
                        "\" to \"" + extractTo.getAbsolutePath() + "\"");
            }
        } finally {
            closeQuietly(in);
            tmp.delete(); // return status ignored
        }
    }



二是对这个dex进行安装（系统优化），这个过程为install。目前的情况，我用两台低端手机各测三次，结果如下：(单位毫秒)

手机1，TCL 
06-14 17:46:48.073 24963-24963/com.tmall.wireless E/TTTT: extract time1873
06-14 17:46:50.553 24963-24963/com.tmall.wireless E/TTTT: install time2477
06-14 17:47:34.193 26517-26517/com.tmall.wireless E/TTTT: extract time1878
06-14 17:47:35.883 26517-26517/com.tmall.wireless E/TTTT: install time1679
06-14 17:48:35.193 28026-28026/com.tmall.wireless E/TTTT: extract time1960
06-14 17:48:36.893 28026-28026/com.tmall.wireless E/TTTT: install time1698

手机2，华为电信赠送机

06-14 17:48:37.698 25220-25220/com.tmall.wireless E/TTTT: extract time2518
06-14 17:48:41.158 25220-25220/com.tmall.wireless E/TTTT: install time3447
06-14 18:02:49.658 10344-10344/com.tmall.wireless E/TTTT: extract time2168
06-14 18:02:52.058 10344-10344/com.tmall.wireless E/TTTT: install time2396
06-14 18:03:20.908 11684-11684/com.tmall.wireless E/TTTT: extract time2207
06-14 18:03:23.138 11684-11684/com.tmall.wireless E/TTTT: install time2224

classes2.dex.zip 文件大小为约为2.1M

尝试一
我们知道，PathClassLoader是可以加载zip, dex等文件，看上述源代码这一行：

new ZipOutputStream(new BufferedOutputStream(new FileOutputStream(tmp)));
它是把文件流取出来又重新压缩，如果不经过压缩这一步，直接取出来就是dex，那是不是会快一点呢？
动手尝试后，发现更慢了。原因在于SD卡的写入速度。原始的dex文件有5M多，而我这手机的SD卡的写入速度了10Mb/S，约为每秒1M的写入速度，实验数据太难看，就不发出来污大家眼了。

尝试二
有了前面的经验，我尝试对压缩的参数进行调整，

            out = new ZipOutputStream(new BufferedOutputStream(new FileOutputStream(tmp)));
            out.setLevel(Deflater.BEST_COMPRESSION);
猜一下结果，

TCL
06-14 17:46:48.073 24963-24963/com.tmall.wireless E/TTTT: extract time2564
06-14 17:46:50.553 24963-24963/com.tmall.wireless E/TTTT: install time2311
06-14 17:47:34.193 26517-26517/com.tmall.wireless E/TTTT: extract time2870
06-14 17:47:35.883 26517-26517/com.tmall.wireless E/TTTT: install time1899
06-14 17:48:35.193 28026-28026/com.tmall.wireless E/TTTT: extract time2509
06-14 17:48:36.893 28026-28026/com.tmall.wireless E/TTTT: install time1769

华为手机
06-14 17:43:28.883 20248-20248/com.tmall.wireless E/TTTT: extract time4015
06-14 17:43:30.983 20248-20248/com.tmall.wireless E/TTTT: install time2099
06-14 17:44:01.123 21681-21681/com.tmall.wireless E/TTTT: extract time3703
06-14 17:44:02.803 21681-21681/com.tmall.wireless E/TTTT: install time1676
06-14 17:44:26.623 22339-22339/com.tmall.wireless E/TTTT: extract time3626
06-14 17:44:28.243 22339-22339/com.tmall.wireless E/TTTT: install time1613

classes2.dex.zip 文件大小为约为2.1M

根本原因是压缩花了大量的时间，文件大小几乎没变！

尝试三
            out = new ZipOutputStream(new BufferedOutputStream(new FileOutputStream(tmp)));
            out.setLevel(Deflater.BEST_SPEED);
时间数据如下：

TCL
06-14 17:30:44.152 12719-12719/com.tmall.wireless E/TTTT: extract time924
06-14 17:30:45.762 12719-12719/com.tmall.wireless E/TTTT: install time1605
06-14 17:31:35.252 14335-14335/com.tmall.wireless E/TTTT: extract time865
06-14 17:31:37.072 14335-14335/com.tmall.wireless E/TTTT: install time1821
06-14 17:32:44.102 16292-16292/com.tmall.wireless E/TTTT: extract time838
06-14 17:32:45.782 16292-16292/com.tmall.wireless E/TTTT: install time1675

华为

06-14 17:33:31.285 10842-10842/com.tmall.wireless E/TTTT: extract time1028
06-14 17:33:34.045 10842-10842/com.tmall.wireless E/TTTT: install time2155
06-14 17:34:07.255 12350-12350/com.tmall.wireless E/TTTT: extract time985
06-14 17:34:09.515 12350-12350/com.tmall.wireless E/TTTT: install time2357
06-14 17:34:46.095 13734-13734/com.tmall.wireless E/TTTT: extract time1020
06-14 17:34:48.425 13734-13734/com.tmall.wireless E/TTTT: install time2030

classes2.dex.zip文件大小约为2.28M

可以看出，在install时间波动不大的情况下，解压的时间缩短了约1s的样子，使得安装第2个dex的整体时间从4到5s降低到3到4s，虽然结果还是很难看，但也算有20%到25%的提升。。。。

为了追求解压速度与SD卡写入速度的一个平衡，要求速度足够快，文件又不能太大。这就需要综合了解了Deflater算法，从网上抠了别人的测试结果。

时间图：

压缩数据图：
7bfc85c0d74ff05806e0b5a0fa0c1df1

由此可以看出，BEST_COMPRESS(值为9）需要的时间最久，效果也最好，BEST_SPEED(1)最快，效果看起来是最差。但是要考虑到IO的吞吐性能与时间的综合因素，压缩参数321的收益是最大的，我考虑到移动处理器较慢的影响，最后使用了BEST_SPEED来作为程序参数。

结论
压缩是程序中经常会用到的算法，包括处理网络协议，图片，文件等，这时我们要选择适当的压缩算法，调整适当的参数，才能让算法的收益达到最大。同时，我们也要对默认参数保持足够多的警戒心。

## 参考

[Android 启动速度优化](https://juejin.im/entry/582adaad570c35006cdcb615)

[启动速度统计](https://blog.csdn.net/beyond_liyy/article/details/52273740)

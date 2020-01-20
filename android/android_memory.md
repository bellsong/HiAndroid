# 内存优化指南

### [内存查看和监控](./android_memory_monitor.md)	

### 内存上涨产生的原因

### 内存优化方案

1. [内存泄漏](./android_memory_leak.md)

2. [图片优化](./android_memory_bitmap.md)

3. 图片压缩

4. 缓存池大小

5. 内存抖动

6. Android Studio Inspection Code

### 工具使用

1. [LeakCanary](./android_tool_leakcanary.md)

2. [MAT使用指南](./android_tool_mat.md)

3. [Andorid Studio 自带的分析工具 Memory Profiler](https://developer.android.com/studio/profile/memory-profiler?hl=zh-cn)

4. [Native内存泄漏检测](./android_memory_native.md)

### 内存优化建议

1. 谨慎使用服务

	离开了 APP 还在运行服务是最糟糕的内存管理错误之一，当 APP 处在后台，我们应该停止服务，除非它需要运行的任务。我们可以使用 JobScheduler 替代实现，JobScheduler 把一些不是特别紧急的任务放到更合适的时机批量处理。如果必须使用一个服务，最佳方法是使用 IntentService ，限制服务寿命，所有请求处理完成后，IntentService 会自动停止。

2. 使用优化的数据容器
	
	考虑使用优化过数据的容器 SparseArray / SparseBooleanArray / LongSparseArray 代替 HashMap 等传统数据结构，通用 HashMap 的实现可以说是相当低效的内存，因为它需要为每个映射一个单独的条目对象。

3. 避免在 Android 上使用枚举

	枚举往往需要两倍多的内存，静态常量更多，我们应该严格避免在 Android 上使用枚举。

4. 使用 nano protobufs 序列化数据
	
	Protocol buffers 是一个语言中立，平台中立的，可扩展的机制，由谷歌进行序列化结构化数据，类似于 XML 设计的，但是更小，更快，更简单。如果需要为您的数据序列化与协议化，建议使用 nano protobufs。

5. 避免内存流失
	
	内存流失可能会导致出现大量的 GC 事件，如自定义组件的 onDraw() ，避免大量创建临时对象，比如 String ，以免频繁触发 GC。GC 事件通常不影响您的 APP 的性能，然而在很短的时间段，发生许多垃圾收集事件可以快速地吃了您的帧时间，系统上时间的都花费在 GC ，就有很少时间做其他的东西像渲染或音频流。

6. 使用ProGuard来剔除不需要的代码

	使用 ProGuard 来剔除不需要的代码，移除任何冗余的，不必要的，或臃肿的组件，资源或库完善 APP 的内存消耗。

7. 降低整体尺寸APK

	通过减少 APP 的整体规模显著减少 APP 的内存使用情况。文章：Android APK瘦身实践

8. 优化布局层次
	
	通过优化视图层次结构，以减少重叠的 UI 对象的数量来提高性能。文章：Android 渲染优化

9. 使用 Dagger 2依赖注入
	
	依赖注入框架可以简化您写的代码，并提供一个自适应环境测试和便于其他配置的更改。如果打算在您的 APP 使用依赖注入框架，可以考虑用 Dagger 2 ，Dagger 不使用反射扫描 APP 的代码，Dagger 是静态的，意味着它编译时不需要占用运行 Android 应用或内存的使用。

10. 请小心使用外部库
	
	外部库的代码往往是未针对移动环境下编写并用于工作在移动客户端上时，可能是低效的。当您决定使用外部库，您可能需要优化该库为移动设备。

11. 避免Bitmap浪费
	
	Bitmap是内存消耗的大头，当使用时要及时回收。另外配置：
inSampleSize：缩放比例，图片按需加载，避免不必要的大图载入。
decode format：解码格式，选择ARGB_8888/RBG_565/ARGB_4444/ALPHA_8，存在很大差异。

12. Cursor关闭
	
	如查询数据库的操作，使用到Cursor，也要对Cursor对象及时关闭。

13. 监听器的注销
	
	Android程序里面存在很多需要register与unregister的监听器，手动add的listener，需要记得及时remove这个listener。


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


GC是有代价的。这个代价包括：阻塞Java程序的执行，占用CPU资源，占用额外内存等。

Logcat里输出以下几种GC日志：
1. GC_EXPLICIT是Dalvik给开发人员提供的主动触发GC的API。读者可以参看Google Maps的设计来体会这个API的用法。
2. GC_FOR_ALLOC是分配对象失败时触发的GC，这个GC会将应用所有Java线程暂停运行，直到GC结束。
3. GC_CONCURRENT是Java虚拟机根据堆的当前状态触发的GC，这个GC在Dalvik单独的GC线程里运行，在部分时间里不影响应用Java线程的运行。

GC和堆的关系

Android上的应用程序一般会配置堆的初始值和最大值。每一次GC都会扫描堆里的所有对象，区分“活对象”和“死对象”。“活对象”不会被GC回收，随着“活对象”增加，堆会逐渐扩大。另外，堆的碎片化也会导致堆不断扩大。当堆增长达到最大值还不能完成对象分配时，会触发OOM——OOM前会执行一次GC尝试回收softreference，如果这个GC结束后还不能完成对象分配，进程崩溃。通常意义上来讲，频繁的GC会有助于减缓扩堆的速度。

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


### 参考

[实践App内存优化：如何有序地做内存分析与优化](https://juejin.im/post/5b1b5e29f265da6e01174b84)

[浅谈Android内存优化](https://juejin.im/post/5c978bc4e51d45101a372077)

[腾讯bugly Andoird内存优化](https://mp.weixin.qq.com/s/2MsEAR9pQfMr1Sfs7cPdWQ)

[Android 开发，跳不过的内存管理](https://mp.weixin.qq.com/s?__biz=MzIxNjc0ODExMA==&mid=2247484311&idx=1&sn=1fe0416bed4137dd45c6e9c153bb14f4&chksm=97851ab6a0f293a0cde28ff6d1091b2232e1758e9845a05549d01c62f412def742985d642630&scene=21#wechat_redirect)

[Android性能优化：全面带你了解 内存优化 & 解决方案](https://juejin.im/entry/5aea6d08f265da0b8f62601f)

[Google官方andorid性能优化](https://www.kancloud.cn/alex_wsc/better/202711)

[Android 系统不释放内存么？](https://juejin.im/entry/5b9af2de6fb9a05cdd2cf457?utm_source=gold_browser_extension)

[Android 内存泄漏 - 做一个有“洁癖”的开发者](https://www.jianshu.com/p/44d26d355a56)

[Bitmap 比你想的更费内存 | 吊打 OOM](https://mp.weixin.qq.com/s?__biz=MzIxNjc0ODExMA==&mid=2247484679&idx=1&sn=d738f5ec092c8490484b66cb1ab80eab&chksm=97851c26a0f29530c2359cec1bbe74d93b90e4a1ba8df751dd6469734c8e58280fb265442d0c&scene=21#wechat_redirect)

[Android内存分析和调优](https://blog.csdn.net/zhongnanjun_3/article/details/49330735)

[Android性能优化-内存篇](https://www.jianshu.com/p/829477754c19)

[Android内存调试工具总结](https://wertherzhang.com/android-memory-debug/)

[调查 RAM 使用情况](https://developer.android.com/studio/profile/investigate-ram?hl=zh-cn)

[我这样减少了26.5M Java内存！](https://wetest.qq.com/lab/view/359.html)

[Android 内存暴减的秘密？！](https://wetest.qq.com/lab/view/362.html)
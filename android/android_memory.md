## 内存优化 

1. 处理内存泄漏 工具
接入 LeakCanary

[通过MAT查看内存占用](https://blog.csdn.net/xiaanming/article/details/42396507)

adb shell dumpsys meminfo packagename -d

2. 图片分辨率 处理Bitmap
	获取图片大小 getrowBytes()
	防止图片位置

	当前机器的Density 图片放置的位置

	[Android代码内存优化建议-Android资源篇](https://xiaozhuanlan.com/topic/7154902863)
	[Android APP内存优化之图片优化](https://zmywly8866.github.io/2015/07/01/android-reduce-app-memory-use.html)

3. 图片压缩

4. 缓存池大小

5. 内存抖动

[Android 内存优化](http://yefangqingchen.com/2017/04/01/Android-%E5%86%85%E5%AD%98%E4%BC%98%E5%8C%96/)

[Andoird内存优化](https://mp.weixin.qq.com/s/2MsEAR9pQfMr1Sfs7cPdWQ)

[使用 Memory Profiler 查看 Java 堆和内存分配](https://developer.android.com/studio/profile/memory-profiler?hl=zh-cn)

[Android 内存优化](http://wuxiaolong.me/2017/04/15/memory/)

[Android性能优化：全面带你了解 内存优化 & 解决方案](https://juejin.im/entry/5aea6d08f265da0b8f62601f)

[Google官方andorid性能优化](https://www.kancloud.cn/alex_wsc/better/202711)

### 什么是内存泄漏

Android虚拟机的垃圾回收采用的是根搜索算法。GC会从根节点（GC Roots）开始对heap进行遍历。到最后，部分没有直接或者间接引用到GC Roots的就是需要回收的垃圾，会被GC回收掉。而内存泄漏出现的原因就是存在了无效的引用，导致本来需要被GC的对象没有被回收掉。

举个栗子

![](http://mmbiz.qpic.cn/mmbiz/kn3fIZB16Mp3y84Lhc1FN29wKuksClicITOibQWYvaD2Byq7hqCC51wicOcDoLXgicAGGictiaJhmLRqb4ehicJCicXVjg/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1)

mLeak是存储在静态区的静态变量，而Leak是内部类，其持有外部类Activity的引用。这样就导致Activity需要被销毁时，由于被mLeak所持有，所以系统不会对其进行GC，这样就造成了内存泄漏。

再举一个最常犯的栗子
![](http://mmbiz.qpic.cn/mmbiz/kn3fIZB16Mp3y84Lhc1FN29wKuksClicIvpTuo6NNUIpjrjPueOic8uToBfczO5rA0cdQpGK9fkDHdvZaxkvmlgg/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1)

如果我们在在调用Singleton的getInstance()方法时传入了Activity。那么当instance没有释放时，这个Activity会一直存在。因此造成内存泄露。

解决方法可以将new Singleton(context)改为new Singleton(context.getApplicationContext())即可，这样便和传入的Activity没关系了。

内存泄漏的检测

打开Android Studio，编译代码，在模拟器或者真机上运行App，然后点击[Anroid monitor]，在Android Monitor下点击Monitor对应的Tab，进入如下界面

![](http://mmbiz.qpic.cn/mmbiz/kn3fIZB16Mp3y84Lhc1FN29wKuksClicIDGicvgU8Ba8tqVzOyE3lVKiaNv70J2MHF98cPb6pPPoaWDGhcrSwIbrA/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1)

在Memory一栏中，可以观察不同时间App内存的动态使用情况，点击可以手动触发GC，点击可以进入HPROF Viewer界面，查看Java的Heap，如下图

![](http://mmbiz.qpic.cn/mmbiz/kn3fIZB16Mp3y84Lhc1FN29wKuksClicIkKlU2m8UQFZqSsXQiaEfonIRHybC7NOVfFic3moO2ZJrFjeIMgkf3BGg/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1)

Reference Tree代表指向该实例的引用，可以从这里面查看内存泄漏的原因，Shallow Size指的是该对象本身占用内存的大小，Retained Size代表该对象被释放后，垃圾回收器能回收的内存总和。

下面我们以掌上道聚城客户端为例，来一探内存泄漏检测的方法。

打开Android Studio，编译代码，运行掌上道聚城，然后开始尽情的耍我们的App啦，然后就从Memory Monitor里面观察App的内存使用曲线，突然发现，纳尼！！！怎么内存使用越来越大了，这就很有可能是发生内存泄漏了，然后点击手动进行GC，再点击观看JavaHeap，点击Analyzer Task，Android Monitor就可以为我们自动分析泄漏的Activity啦，分析出来如下图所示

![](http://mmbiz.qpic.cn/mmbiz/kn3fIZB16Mp3y84Lhc1FN29wKuksClicIWicdLDjWa0KownTuuB49eLwKFuMFfUN34q0JXfW3cPA3AkprGyvNdJQ/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1)

在Reference Tree里面，我们直接就可以看到持有该Activity的单例对象，直接定位到该单例中的代码，发现代码中出现了

![](http://mmbiz.qpic.cn/mmbiz/kn3fIZB16Mp3y84Lhc1FN29wKuksClicI9Q3wjyEnJ8vauhib0UXFaqR261U57iaeZj6bMDK1PlXjibibiaLOrNcuAEA/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1)

和刚刚举得栗子里出现的错误一模一样啊，这段代码是谁写的，拖出去······

我们修复了检查出的内存泄漏的问题，并将修复前和修复后的代码在相同的模拟器上运行并进行相同的操作，查看他们使用内存的情况，如下图所示

![](http://mmbiz.qpic.cn/mmbiz/kn3fIZB16Mp3y84Lhc1FN29wKuksClicIKG37QKiclEic3avYPsibfNCYhqkNqxmTfcDYtCu4EXmTGAJf3bqDK9Zpg/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1)

有内存泄漏的情况，占用内存约为43M

![](http://mmbiz.qpic.cn/mmbiz/kn3fIZB16Mp3y84Lhc1FN29wKuksClicIbNT8KhARyMalcyo211tvLdWnL3K0WmicNldlBhME4z8jP5ibf6emOOpA/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1)
修复了内存泄漏问题，占用内存为36M。

在修复了内存泄漏问题后，内存使用下降了16.3%！！！

掌握了Android Monitor的使用方法后，妈妈再也不担心我写的App会出现内存泄漏啦！！！

 * [基于Android Studio的内存泄漏检测与解决全攻略](http://wetest.qq.com/lab/view/?id=99)
  * [腾讯手机管家实战分析：内存突增是为神马？](http://bugly.qq.com/bbs/forum.php?mod=viewthread&tid=30&highlight=%E5%86%85%E5%AD%98%E7%AA%81%E5%A2%9E)
  内存泄露场景

1、注册了监听器，忘了反过注册；
>销毁时反注册

2、内部类，匿名内部类；
>静态内部类

3、WebView	
>不要在xml中声明，销毁时移除所有的view，直接将weview相关的放在一个进程中，退出时直接杀死进程

内存优化，主要就是去消除应用中的内存泄露、避免内存抖动。

1、安卓studio的内存分析工具 + mat可以很好的检测内存抖动和内存泄露

2、常见的内存泄露情况：

● 单例：生命周期很长，会引用生命周期比较短的变量，导致无法释放。例如activity泄露

● 静态变量：同样也是应为生命周期比较长

● 非静态内部类创建静态实例造成的内存泄漏

● handler内存泄露 （解决办法：Handler 声明为静态的，则其存活期跟 Activity 的生命周期就无关了。同时通过软引用的方式引入 Activity）

● 匿名内部类（匿名内部类会引用外部类，导致无法释放，比如各种回调）

● 资源使用完未关闭（BraodcastReceiver，ContentObserver，File，Cursor，Stream，Bitmap）

● 复用问题（bitmap释放）

Android 内存优化总结&实践

Android内存优化之OOM

[Android应用内存泄露分析、改善经验总结](zhuanlan.zhihu.com/p/20831913)

[参考链接]

[Android 内存优化——常见内存泄露及优化方案](https://juejin.im/entry/58ef30fd44d904006cdfcbb6)

[Android性能优化：那些关于Bitmap优化的小事](https://juejin.im/entry/5aa873996fb9a028db586153)

(https://medium.com/freenet-engineering/memory-leaks-in-android-identify-treat-and-avoid-d0b1233acc8#.bnwtaamwh)


内存泄露检测工具
[LeakCanary](http://www.jianshu.com/p/e9891d7512ff)

[Android内存分析命令](http://gityuan.com/2016/01/02/memory-analysis-command/)

[Android有效解决加载大图片时内存溢出的问题](http://www.cnblogs.com/wanqieddy/archive/2011/11/25/2263381.html)

[Android -> 如何避免Handler引起内存泄露](http://blog.csdn.net/feelang/article/details/39059705)

[Android - 利用LeakCanary检测内存泄露](http://cashow.github.io/android-detect-out-of-memory-with-leakcanary.html)


[内存泄露从入门到精通三部曲之基础知识篇](http://mp.weixin.qq.com/s?__biz=MzA3NTYzODYzMg==&mid=400674207&idx=1&sn=a9580ca0dffc62a6d7dbb8fd3d7a2ef1&scene=0&key=b410d3164f5f798e3f4b6de393face7f291ae1d5d6ce312646e1e72ba2b6849e52d3ef5d2d0e4e8579cc7841aac8b439&ascene=0&uin=MTYzMjY2MTE1&devicetype=iMac+MacBookPro10%2C1+OSX+OSX+10.11.1+build(15B42)&version=11020201&pass_ticket=hgYTL4MW7%2FI9mnat%2BT9S2RRS0IkFfm6yOLSy%2F4bguL4%3D)


[Android 开发绕不过的坑：你的 Bitmap 究竟占多大内存？](http://bugly.qq.com/bbs/forum.php?mod=viewthread&tid=498#rd)

[避免Android中Context引起的内存泄露](http://blog.csdn.net/boyupeng/article/details/46503221)

[腾讯](http://mp.weixin.qq.com/s?__biz=MzAxMzYyNDkyNA==&mid=2651332083&idx=1&sn=d5a1b24736d6f14ff24dfecf15e397a9&scene=0#wechat_redirect)

[内存分析命令](http://gityuan.com/2016/01/02/memory-analysis-command/)

[如何写一个匿名内部类，而又不引起内存泄漏](http://www.atatech.org/articles/58280)

## Leakcanary分享

**1. github地址**

[https://github.com/square/leakcanary](https://github.com/square/leakcanary)

[中文版翻译地址](http://www.liaohuqiu.net/cn/posts/leak-canary/)

**2. 接入指南**

- 引入leakcanary工程

		debugCompile 'com.squareup.leakcanary:leakcanary-android:1.4-beta1'
    	releaseCompile 'com.squareup.leakcanary:leakcanary-android-no-op:1.4-beta1'

> 目前官网稳定版本是 1.3.1，但是接入时发现一些机型dump文件有问题，在1.4-beta1上已解决

- Application中初始化Leakcanary类
	
		LeakCanary.install(Application); // Application 
		LeakCanary.install(this, PPMemLeakHandleService.class, AndroidExcludedRefs.createAppDefaults().build());

- 对需要检测的内存泄漏的地方使用 RefWather的watch方法
		
		RefWather().watch(Object)

- 生成的dump存放路径

		/sdcard/Download/leakcanary-packageName/


**3. Leakcanary实现原理**

- Leakcanary注册过程

	![](http://i.imgur.com/RBN9Q93.png)

	1. 实例化一个RefWather类，用来监控内存泄漏的对象
	2. 4.0以上版本默认开启对Activity页面的内存泄漏监控

- WeakReference和ReferenceQueue

		当GC线程扫描它所管辖的内存区域时，一旦发现了只具有弱引用的对象，不管当前内存空间足够与否(这一点与SoftReference不同)，
		都会回收它的内存。由于垃圾回收器是一个优先级很低的线程，因此不一定会很快发现那些只具有弱引用的对象。

		WeakReference和ReferenceQueue联合使用，如果弱引用所引用的对象被垃圾回收，Java虚拟机就会把这个弱引用加入到与之关联的引用队列中。
	
	在Leakcanary里，就是使用了WeakReference和ReferenceQueue机制来排查内存泄漏点

- RefWather工作原理

	内存泄漏的监控都是由RefWather的 watch 方法来进行监控的，其实现原理如下：

	- RefWatcher.watch(Object) 创建一个 KeyedWeakReference 到要被监控的对象，并生成一个唯一的标识reference key。
	- 在后台线程中检查引用是否被清除，如果没有，主动触发一次GC操作
	- 如果此时引用还是未被清除，此时把 heap 内存dump到 suspected_leak_heapdump.hprof 一个文件当中
		> Debug.dumpHprofData(String filePath);
	- 在另外一个进程中(默认是:leakcanary)中，HeapAnalyzerService 通过 HeapAnalyzer 类来对hprof文件进行解析
		> 这里解析hprof的操作是通过一个 HAHA 的开源项目来实现的
	- 通过唯一标识 reference key，HeapAnalyzer 在hprof文件中找到 KeyedWeakReference对象，定位内存泄漏。
	- HeapAnalyzer 计算 到 GC roots 的最短强引用路径，并确定是否泄漏。如果是，建立导致泄漏的引用链。
	- 引用链传递到 APP 进程中的 DisplayLeakService， 并以通知的形式展示出来。

	![](http://i.imgur.com/4wU1Ybs.png)

**4. 定制化Leakcanary**

HeapAnalyzer在分析出了内存泄漏结果后，将结果传递到 DisplayLeakService 中进行结果处理
DisplayLeakService类默认的处理是在通知栏中展示引用链，我们可以通过继承这个类，加上一些自己的处理操作，如：将内存泄漏结果上传到服务器进行分析

		protected void afterDefaultHandling(HeapDump heapDump, AnalysisResult result, String leakInfo) {}

只需要在注册Leakcanary时传递这个Service的class对象即可：
		
		LeakCanary.install(this, PPMemLeakHandleService.class, AndroidExcludedRefs.createAppDefaults().build());

HeapAnalyzer分析内存泄漏打印结果如下：
	![](http://i.imgur.com/EULOdSS.png)

**5. 更全面的分析内存泄漏问题**

有时候通过Leakcanary可以方便的找出内存泄漏的嫌疑点，但有可能分析的结果不太好排查内存泄漏问题，这时我们可以使用更高级的工具来分析原始的hprof文件。

MAT [MAT使用指南](http://androidperformance.com/2015/04/11/AndroidMemory-Usage-Of-MAT.html)

MAT(Memory Analyzer Tool)，一个基于Eclipse的内存分析工具，是一个快速、功能丰富的JAVA heap分析工具，它可以帮助我们查找内存泄漏和减少内存消耗。使用内存分析工具从众多的对象中进行分析，快速的计算出在内存中对象的占用大小，看看是谁阻止了垃圾收集器的回收工作，并可以通过报表直观的查看到可能造成这种结果的对象。

前面介绍到的由Android dump出来的 hprof 文件，如果要在 MAT 中打开，需要使用Android sdk提供的 hprof-conv 工具对文件进行转换：

		hprof-conv input.hprof out.hprof

然后通过 MAT 导入hprof文件即可

**6. 一些有用的资料**

[MAT使用指南](http://androidperformance.com/2015/04/11/AndroidMemory-Usage-Of-MAT.html)

[Android 内存剖析 – 发现潜在问题](http://www.importnew.com/2433.html)

[使用Android studio分析内存泄露](http://www.jianshu.com/p/c49f778e7acf)

[Android 内存泄漏总结](https://yq.aliyun.com/articles/3009)

[Android 应用内存泄漏的定位、分析与解决策略](https://www.diycode.cc/topics/475)

[MemoryMonitor](https://www.diycode.cc/projects/cundong/MemoryMonitor)

[Android 系统不释放内存么？](https://juejin.im/entry/5b9af2de6fb9a05cdd2cf457?utm_source=gold_browser_extension)

[内存泄露](http://blog.csdn.net/xiaochuanding/article/details/56286074?utm_source=gank.io&utm_medium=email)

[[Android] 内存泄漏调试经验分享 (一)](http://rayleeya.iteye.com/blog/727074)

##### WebView
[Android WebView Memory Leak WebView内存泄漏解决方案](http://my.oschina.net/zhibuji/blog/100580)
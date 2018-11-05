# 内存泄漏


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


[Android应用内存泄露分析、改善经验总结](zhuanlan.zhihu.com/p/20831913)

[Android应用内存泄露分析、改善经验总结](https://zmywly8866.github.io/2016/05/04/android-application-leak-analysis-and-fix.html)

[Android 内存优化——常见内存泄露及优化方案](https://juejin.im/entry/58ef30fd44d904006cdfcbb6)

[Android - 利用LeakCanary检测内存泄露](http://cashow.github.io/android-detect-out-of-memory-with-leakcanary.html)

[如何写一个匿名内部类，而又不引起内存泄漏](http://www.atatech.org/articles/58280)

[Android -> 如何避免Handler引起内存泄露](http://blog.csdn.net/feelang/article/details/39059705)
[避免Android中Context引起的内存泄露](http://blog.csdn.net/boyupeng/article/details/46503221)

[Android 内存泄漏总结](https://yq.aliyun.com/articles/3009)

[Android 应用内存泄漏的定位、分析与解决策略](https://www.diycode.cc/topics/475)

[Android 内存优化总结 & 实践](https://juejin.im/entry/58d4c7735c497d0057ead153)

[[Android] 内存泄漏调试经验分享 (一)](http://rayleeya.iteye.com/blog/727074)

##### WebView
[Android WebView Memory Leak WebView内存泄漏解决方案](http://my.oschina.net/zhibuji/blog/100580)


[内存泄露从入门到精通三部曲之基础知识篇](http://mp.weixin.qq.com/s?__biz=MzA3NTYzODYzMg==&mid=400674207&idx=1&sn=a9580ca0dffc62a6d7dbb8fd3d7a2ef1&scene=0&key=b410d3164f5f798e3f4b6de393face7f291ae1d5d6ce312646e1e72ba2b6849e52d3ef5d2d0e4e8579cc7841aac8b439&ascene=0&uin=MTYzMjY2MTE1&devicetype=iMac+MacBookPro10%2C1+OSX+OSX+10.11.1+build(15B42)&version=11020201&pass_ticket=hgYTL4MW7%2FI9mnat%2BT9S2RRS0IkFfm6yOLSy%2F4bguL4%3D)



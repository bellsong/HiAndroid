## LeakCanary 使用指南

**1. 官方介绍**

[官方地址](https://github.com/square/leakcanary)

**2. Getting started**

- 引入leakcanary工程

		dependencies {
      debugImplementation 'com.squareup.leakcanary:leakcanary-android:1.6.2'
      releaseImplementation 'com.squareup.leakcanary:leakcanary-android-no-op:1.6.2'
      // Optional, if you use support library fragments:
      debugImplementation 'com.squareup.leakcanary:leakcanary-support-fragment:1.6.2'
      }

- Application中初始化Leakcanary类
	
		public class ExampleApplication extends Application {

      @Override public void onCreate() {
      super.onCreate();
        if (LeakCanary.isInAnalyzerProcess(this)) {
      // This process is dedicated to LeakCanary for heap analysis.
      // You should not init your app in  this process.
        return;
      }
        LeakCanary.install(this);
        // Normal app init code...
      }
    }

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


## 参考
[LeakCanary 中文使用说明](https://www.liaohuqiu.net/cn/posts/leak-canary-read-me/)

[LeakCanary](http://www.jianshu.com/p/e9891d7512ff)



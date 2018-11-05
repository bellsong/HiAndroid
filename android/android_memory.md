# 内存优化

## 内存优化指南

### [内存查看和监控](./android_memory_monitor.md)	

* 查看的方法

		[WETEST](https://wetest.qq.com/lab/view/359.html)

		[内存分析命令](http://gityuan.com/2016/01/02/memory-analysis-command/)

	
	* 监控的手段

	[MemoryMonitor](https://www.diycode.cc/projects/cundong/MemoryMonitor)


* 产生的原因

* 优化的方案

1. [内存泄漏](./android_memory_leak.md)

2. [图片优化](./android_memory_bitmap.md)

3. 图片压缩

4. 缓存池大小

5. 内存抖动

6. Android Studio Inspection Code

* 工具使用

1. adb shell dumpsys meminfo packagename -d

1. [LeakCanary](./android_tool_leakcanary.md)

2. [MAT使用指南](./android_tool_mat.md)


### 参考

[Android 内存优化](http://yefangqingchen.com/2017/04/01/Android-%E5%86%85%E5%AD%98%E4%BC%98%E5%8C%96/)

[Andoird内存优化](https://mp.weixin.qq.com/s/2MsEAR9pQfMr1Sfs7cPdWQ)

[使用 Memory Profiler 查看 Java 堆和内存分配](https://developer.android.com/studio/profile/memory-profiler?hl=zh-cn)

[Android 内存优化](http://wuxiaolong.me/2017/04/15/memory/)

[Android性能优化：全面带你了解 内存优化 & 解决方案](https://juejin.im/entry/5aea6d08f265da0b8f62601f)

[Google官方andorid性能优化](https://www.kancloud.cn/alex_wsc/better/202711)

[Android 内存剖析 – 发现潜在问题](http://www.importnew.com/2433.html)

[使用Android studio分析内存泄露](http://www.jianshu.com/p/c49f778e7acf)

[Android 系统不释放内存么？](https://juejin.im/entry/5b9af2de6fb9a05cdd2cf457?utm_source=gold_browser_extension)


[OOM优化](http://rayleeya.iteye.com/blog/1956059)
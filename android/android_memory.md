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

3. [Memory Profiler](https://developer.android.com/studio/profile/memory-profiler?hl=zh-cn)

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

### 参考

[腾讯bugly Andoird内存优化](https://mp.weixin.qq.com/s/2MsEAR9pQfMr1Sfs7cPdWQ)

[Android性能优化：全面带你了解 内存优化 & 解决方案](https://juejin.im/entry/5aea6d08f265da0b8f62601f)

[Google官方andorid性能优化](https://www.kancloud.cn/alex_wsc/better/202711)

[Android 系统不释放内存么？](https://juejin.im/entry/5b9af2de6fb9a05cdd2cf457?utm_source=gold_browser_extension)

[Android 内存泄漏 - 做一个有“洁癖”的开发者](https://www.jianshu.com/p/44d26d355a56)
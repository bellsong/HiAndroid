# Android性能专项

#### Android性能是什么？

    简而言之，整体表现上的性能，多快给到用户想要的结果

具体包括：UI性能（界面卡顿），内存，CPU，电池，磁盘，网络流量，服务器资源

在开发，测试，灰度，发布各个阶段对性能问题的关注和诉求是不一样的，所采取具体措施也有所不同。

例如

开发阶段主要是防止低性能的设计，编码，例如可以通过IDE插件在编码阶段就对低性能的代码进行告警。

测试阶段，可以基于实验室环境，对卡顿，流量，IO，内存，启动速度等等性能数据进行详尽的自动化测试，越详细越好。

灰度阶段是基于线上环境，对一些核心性能点进行监测。

发布后则只针对一些核心指标进行粗粒度的监控。

#### 优化专项

* [启动优化专项](./android_speed_start.md)

* [Size优化专项](./android_optimize_size.md)

* [UI卡顿优化](./android_carton.md)

* [内存优化专项](./android_memory.md)

* [电量优化专项](./android_energy.md)

* [网络速度优化专项]

* [网络流量优化专项](./android_network_flow.md)

* [磁盘空间优化](./android_storge.md)
#### 监控体系

[蜂鸟团队移动端异常监控体系建设](https://juejin.im/post/5b874cbce51d4538b77667e3?utm_source=gold_browser_extension)

[方法执行耗时统计](./android_method_time.md)

#### 检测工具

[Android性能测试工具列表](https://zmywly8866.github.io/2015/09/09/android-performance-tools.html)

[Hujiawei，魅族开发者](https://hujiaweibujidao.github.io/)

#### 参考

[Android性能优化来龙去脉总结](https://juejin.im/entry/5b971f6c6fb9a05d151c883a?utm_source=gold_browser_extension)

https://juejin.im/entry/587c43a3570c352201039814

[Android性能优化典范（一）](http://www.csdn.net/article/2015-01-20/2823621-android-performance-patterns)

[Android 性能优化](http://rayleeya.iteye.com/blog/1961005)

[Android性能调优](http://www.trinea.cn/android/android-performance-demo/)

[Android性能优化典范](http://hukai.me/android-performance-patterns/)

[性能优化](https://www.youtube.com/playlist?list=PLWz5rJ2EKKc9CBxr3BVjPTPoDPLdPIFCE)

[Android性能优化案例研究](http://www.importnew.com/4065.html)

[Android性能优化案例研究](http://codingnow.cn/android/1378.html)

[Android应用性能剖析全攻略](http://toughcoder.net/blog/2015/09/11/android-performance-profiling-made-easy/)

http://toughcoder.net/blog/2015/09/11/android-performance-profiling-made-easy/
http://www.apkbus.com/forum.php?mod=viewthread&tid=121932&extra=page%3D1&page=1
http://blog.csdn.net/lmj623565791/article/details/58626355
http://blog.csdn.net/litton_van/article/details/21872677
http://blog.csdn.net/xjyzxx/article/details/45369069

http://motalks.cn/2016/09/11/Android-WebView-JavaScript-3/
http://www.csdn.net/article/2015-01-20/2823621-android-performance-patterns

Weapons of mass destruction

https://realm.io/cn/news/android-threading-background-tasks/
https://www.diycode.cc/news/2136
http://blog.vunso.com/201307/android%E5%9E%83%E5%9C%BE%E5%9B%9E%E6%94%B6%E6%9C%BA%E5%88%B6%E5%8F%8A%E7%A8%8B%E5%BA%8F%E4%BC%98%E5%8C%96system-gc.htm
http://www.tuicool.com/articles/FvABja
http://www.eoeandroid.com/thread-173838-1-1.html
http://www.eoeandroid.com/thread-153840-1-1.html
http://www.eoeandroid.com/thread-92711-1-1.html
http://cnetwei.iteye.com/blog/716964
http://androidperformance.com/2015/11/18/Android-app-lunch-optimize-delay-load.html
http://mp.weixin.qq.com/s?__biz=MzAwMTYwNzE2Mg==&mid=2651036594&idx=1&sn=b276c0f76cea713e5d568ab51e3f7f13&scene=0#wechat_redirect
https://mp.weixin.qq.com/s?__biz=MzA3MDMyMjkzNg==&mid=2652261762&idx=1&sn=dd95181222f40e6818d9df26ef31fab6&scene=0&pass_ticket=9/MdgGNR+cHQFO4rm/Vr61EvcK4BkZc9Tdb1bNWhlaRbGZLISyBuUC0YH2PdbNHd#rd
http://zhengxiaoyong.me/2016/07/18/Android%E7%AB%AF%E5%BA%94%E7%94%A8%E7%A7%92%E5%BC%80%E4%BC%98%E5%8C%96%E4%BD%93%E9%AA%8C/?hmsr=toutiao.io&utm_medium=toutiao.io&utm_source=toutiao.io
http://androidperformance.com/2015/11/18/Android-app-lunch-optimize-delay-load.html


[Android性能](http://www.jianshu.com/p/9c07323dc7e5?hmsr=toutiao.io&utm_medium=toutiao.io&utm_source=toutiao.io)

[Android App优化之性能分析工具](http://www.jianshu.com/p/da2a4bfcba68)

* 卡顿
* [如何在Android中避免创建不必要的对象](http://droidyue.com/blog/2016/08/01/avoid-creating-unnecesssary-objects-in-android/?hmsr=toutiao.io&utm_medium=toutiao.io&utm_source=toutiao.io)

* [Android性能优化典范](http://www.csdn.net/article/2015-01-20/2823621-android-performance-patterns)

[android app性能优化大汇总（UI渲染性能优化）](https://www.jianshu.com/p/e058f88b305b)


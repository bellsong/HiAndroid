# Android性能专项

#### Android性能是什么？

整体表现上的性能，多快给到用户想要的结果

具体包括：
UI性能（界面卡顿），内存，CPU，电池，磁盘，网络流量，服务器资源

在开发，测试，灰度，发布各个阶段对性能问题的关注和诉求是不一样的，所采取具体措施也有所不同。

例如

开发阶段主要是防止低性能的设计，编码，例如可以通过IDE插件在编码阶段就对低性能的代码进行告警。

测试阶段，可以基于实验室环境，对卡顿，流量，IO，内存，启动速度等等性能数据进行详尽的自动化测试，越详细越好。

灰度阶段是基于线上环境，对一些核心性能点进行监测。

发布后则只针对一些核心指标进行粗粒度的监控。
  
* [启动优化专项](./android_speed_start.md)

* [Size优化专项](./android_optimize_size.md)

* [UI卡顿优化](./android_carton.md)

* [内存优化专项](./android_memory.md)

* [电量优化专项](./android_energy.md)

* [网络速度优化专项]

* [网络流量优化专项](./android_network_flow.md)

#### 检测工具
[Android性能测试工具列表](https://zmywly8866.github.io/2015/09/09/android-performance-tools.html)

[Hujiawei，魅族开发者](https://hujiaweibujidao.github.io/)

[Android性能：远程触发GC](http://www.atatech.org/articles/58479)

[Android性能：Release版如何排查CPU占用率高的问题](http://www.atatech.org/articles/58453)

[天猫Android性能优化2——文件IO如何设置Buffer](http://www.atatech.org/articles/56936)

#### 参考

[Android性能优化来龙去脉总结](https://juejin.im/entry/5b971f6c6fb9a05d151c883a?utm_source=gold_browser_extension)

https://juejin.im/entry/587c43a3570c352201039814

[Android性能优化典范（一）](http://www.csdn.net/article/2015-01-20/2823621-android-performance-patterns)

[Android 性能优化](http://rayleeya.iteye.com/blog/1961005)

[Android性能调优](http://www.trinea.cn/android/android-performance-demo/)

[Android性能优化典范](http://hukai.me/android-performance-patterns/)

[性能优化视频](https://www.youtube.com/playlist?list=PLWz5rJ2EKKc9CBxr3BVjPTPoDPLdPIFCE)

[Android性能优化案例研究](http://www.importnew.com/4065.html)

[Android性能优化案例研究](http://codingnow.cn/android/1378.html)

[Android应用性能剖析全攻略](http://toughcoder.net/blog/2015/09/11/android-performance-profiling-made-easy/)

http://www.atatech.org/articles/55521
http://toughcoder.net/blog/2015/09/11/android-performance-profiling-made-easy/
http://www.apkbus.com/forum.php?mod=viewthread&tid=121932&extra=page%3D1&page=1
http://blog.csdn.net/lmj623565791/article/details/58626355
http://blog.csdn.net/litton_van/article/details/21872677
http://blog.csdn.net/xjyzxx/article/details/45369069
http://www.atatech.org/articles/50851/?frm=mail_daily&uid=130616

http://motalks.cn/2016/09/11/Android-WebView-JavaScript-3/
http://www.csdn.net/article/2015-01-20/2823621-android-performance-patterns

### Android性能优化案例：手机淘宝

* [手淘安卓版性能优化1 - 性能相关概念介绍](http://www.atatech.org/articles/28721)
* [手淘安卓版性能优化2 - TraceView使用经验](http://www.atatech.org/articles/42723)
* [手淘安卓版性能优化3 - 其它性能分析工具的经验](http://www.atatech.org/articles/42862)
* [手淘安卓版性能优化4 - 手淘中存在的主要问题](http://www.atatech.org/articles/43376)
* [手淘安卓版性能优化5 - 手淘遇到的性能问题和优化工具](http://www.atatech.org/articles/43380)
* [手淘安卓版性能优化6 -  Dalvik虚拟机和ART运行时的介绍及对性能的影响](http://www.atatech.org/articles/43598)
* [手淘安卓版性能优化7- 启动阶段的问题和主要优化思路](http://www.atatech.org/articles/44028)
* [手淘安卓版性能优化7- 启动阶段的问题和主要优化思路](http://www.atatech.org/articles/45112)
* [手淘安卓版性能优化9- 其它因素对启动性能的影响过程分析](http://www.atatech.org/articles/45333)
* [手淘安卓版性能优化10- SharedPreference性能分析和使用规范](http://www.atatech.org/articles/45469)
* [手淘安卓版性能优化11 - 界面性能遇到的问题和主要使用的工具](http://www.atatech.org/articles/45552)
* [手淘安卓版性能优化12 - 界面性能之优化过度绘制](http://www.atatech.org/articles/45768)
* [手淘安卓版性能优化13 - 界面性能之布局性能和使用注意事项](http://www.atatech.org/articles/45920)
* [手淘安卓版性能优化14 - 界面性能之加快界面显示](http://www.atatech.org/articles/45999)
* [手淘安卓版性能优化15 - 界面性能之减少不必要的刷新和Layout请求](http://www.atatech.org/articles/46175)
* [手淘安卓版性能优化16 - 界面性能之帧率排查守得云开见月明](http://www.atatech.org/articles/46313)
* [手淘安卓版性能优化17 - 减少GC，提升性能（上）](http://www.atatech.org/articles/46524)
* [手淘安卓版性能优化18 - 减少GC，提升性能（下）](http://www.atatech.org/articles/46717)
* [手淘安卓版性能优化19 - 字符串与格式化的性能及注意事项](http://www.atatech.org/articles/46997)
* [手淘安卓版性能优化20 - 正确使用单例](http://www.atatech.org/articles/47006)
* [手淘安卓版性能优化21 - 避免使用枚举](http://www.atatech.org/articles/47400)
* [手淘安卓版性能优化23 - 深入理解final有关的性能](http://www.atatech.org/articles/48335)
* [手淘安卓版性能优化24 - SysTrace等工具的性能损耗及问题](http://www.atatech.org/articles/48396)
* [手淘安卓版性能优化25 - 减少Dex方法数并提升性能](http://www.atatech.org/articles/48717)

Weapons of mass destruction

https://realm.io/cn/news/android-threading-background-tasks/
https://www.diycode.cc/news/2136
http://blog.vunso.com/201307/android%E5%9E%83%E5%9C%BE%E5%9B%9E%E6%94%B6%E6%9C%BA%E5%88%B6%E5%8F%8A%E7%A8%8B%E5%BA%8F%E4%BC%98%E5%8C%96system-gc.htm
http://www.tuicool.com/articles/FvABja
http://www.eoeandroid.com/thread-173838-1-1.html
http://www.eoeandroid.com/thread-153840-1-1.html
http://www.eoeandroid.com/thread-92711-1-1.html
http://cnetwei.iteye.com/blog/716964
http://www.atatech.org/articles/17189
http://androidperformance.com/2015/11/18/Android-app-lunch-optimize-delay-load.html
http://mp.weixin.qq.com/s?__biz=MzAwMTYwNzE2Mg==&mid=2651036594&idx=1&sn=b276c0f76cea713e5d568ab51e3f7f13&scene=0#wechat_redirect
https://mp.weixin.qq.com/s?__biz=MzA3MDMyMjkzNg==&mid=2652261762&idx=1&sn=dd95181222f40e6818d9df26ef31fab6&scene=0&pass_ticket=9/MdgGNR+cHQFO4rm/Vr61EvcK4BkZc9Tdb1bNWhlaRbGZLISyBuUC0YH2PdbNHd#rd
http://www.atatech.org/articles/57712/?frm=mail_daily&uid=130616
http://zhengxiaoyong.me/2016/07/18/Android%E7%AB%AF%E5%BA%94%E7%94%A8%E7%A7%92%E5%BC%80%E4%BC%98%E5%8C%96%E4%BD%93%E9%AA%8C/?hmsr=toutiao.io&utm_medium=toutiao.io&utm_source=toutiao.io
http://androidperformance.com/2015/11/18/Android-app-lunch-optimize-delay-load.html


[Android性能](http://www.jianshu.com/p/9c07323dc7e5?hmsr=toutiao.io&utm_medium=toutiao.io&utm_source=toutiao.io)

[Android App优化之性能分析工具](http://www.jianshu.com/p/da2a4bfcba68)

* 卡顿
* [如何在Android中避免创建不必要的对象](http://droidyue.com/blog/2016/08/01/avoid-creating-unnecesssary-objects-in-android/?hmsr=toutiao.io&utm_medium=toutiao.io&utm_source=toutiao.io)

* [Android性能优化典范](http://www.csdn.net/article/2015-01-20/2823621-android-performance-patterns)

[android app性能优化大汇总（UI渲染性能优化）](https://www.jianshu.com/p/e058f88b305b)


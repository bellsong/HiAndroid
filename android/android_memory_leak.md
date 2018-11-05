# 内存泄漏

### 什么是内存泄漏

一些对象有着有限的生命周期。当这些对象所要做的事情完成了，我们希望他们会被回收掉。但是如果有一系列对这个对象的引用，那么在我们期待这个对象生命周期结束的时候被收回的时候，它是不会被回收的。它还会占用内存，这就造成了内存泄露。持续累加，内存很快被耗尽。

比如，当 Activity.onDestroy 被调用之后，activity 以及它涉及到的 view 和相关的 bitmap 都应该被回收。但是，如果有一个后台线程持有这个 activity 的引用，那么 activity 对应的内存就不能被回收。这最终将会导致内存耗尽，然后因为 OOM 而 crash。

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


[内存泄露](http://blog.csdn.net/xiaochuanding/article/details/56286074?utm_source=gank.io&utm_medium=email)


内存泄露检测工具
1. [LeakCanary](./android_tool_leakcanary.md)


### 参考

[Andorid 官方内存泄漏例子](https://android-developers.googleblog.com/2009/01/avoiding-memory-leaks.html)

[Android中常见的内存泄漏汇总](https://xiaozhuanlan.com/topic/0583461792)

[Android性能优化-使用MAT分析内存泄漏](https://xiaozhuanlan.com/topic/6852741390)

[MemoryMonitor](https://www.diycode.cc/projects/cundong/MemoryMonitor)

[WETEST](https://wetest.qq.com/lab/view/359.html)
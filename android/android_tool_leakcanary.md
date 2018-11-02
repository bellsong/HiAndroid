## LeakCanary 使用指南

内存泄露检测工具
[LeakCanary](http://www.jianshu.com/p/e9891d7512ff)

[Android中常见的内存泄漏汇总](https://xiaozhuanlan.com/topic/0583461792)
[Android性能优化-使用MAT分析内存泄漏](https://xiaozhuanlan.com/topic/6852741390)

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
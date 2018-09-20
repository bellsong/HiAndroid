# Android Explore

## Y Android

![](./res/android_path.png)

### 基础篇
### 进阶篇
### 高级篇
### 开源项目学习
### 编码规范
### 案例实践
### 源码阅读

### 基础篇

####  java 基础知识

[Java集合---ArrayList的实现原理](http://www.cnblogs.com/ITtangtang/p/3948555.html)

[Java中Vector和ArrayList的区别](http://www.cnblogs.com/wanlipeng/archive/2010/10/21/1857791.html)

[android HashMap源码分析](http://blog.csdn.net/liuluchao0543/article/details/53457721)

（1）ConcurrentHashMap的锁分段技术
（2）ConcurrentHashMap的读是否要加锁，为什么
（3）ConcurrentHashMap的迭代器是强一致性的迭代器还是弱一致性的迭代器

#### [Activity](./android_activity.md)

#### [Service](./android_service.md)

#### [View](./android_view.md)

* [BroadCast](./android_broadcast.md)

#### [Application](./android_application.md)

[Reflection](./android_2_reflection.md)

* [如何理解Android的多进程](http://www.jianshu.com/p/536978a4f4b2)
* [多进程保活](https://juejin.im/post/58cf80abb123db3f6b45525d?utm_source=gold_browser_extension)

### 进阶篇

#### [插件化](./android_pulgin.md)

##### [Android 6.0 动态权限介绍](./android_systempermissions.md)

##### [视频播放相关](https://github.com/danylovolokh/VideoPlayerManager)

[数据格式处理](./android_2_data.md)

[Android混淆](./android_proguard.md)

#### 安全

[Android 逆向](./android_decomplie.md)

### 性能优化
* [内存](./android_memory.md)
  
* [启动专项](./android_speed_start.md)

* [Size专项](./android_optimize_size.md)

* [android优化](./android_optimize.md)

### 高级篇

[Android系统源码分析--Process启动过程](https://juejin.im/post/59ba055ef265da064a0f232b)


[OpenGL](./android_3_opengl.md)

### [Programmer](./programmer.md)

### 其他

#### 黑科技
[Android卸载监听实现](http://www.jianshu.com/p/189e319a5c45)

[AD SDK](./market/android_4_ad_sdk.md)

### Y OPEN SOURCE

[YOkHttp](./y_open_source/y_ok_http.md)

### Public

[发布开源库](http://blog.chengdazhi.com/index.php/217?hmsr=toutiao.io&utm_medium=toutiao.io&utm_source=toutiao.io)

### Q&A

Q1:[如何解决65535天花板？](./android_q1.md)

Q2:[一种动态为apk写入信息的方案](http://pingguohe.net/2016/03/21/Dynimac-write-infomation-into-apk.html?hmsr=toutiao.io&utm_medium=toutiao.io&utm_source=toutiao.io)

Q3:[android 定时任务的解决方案](./android_alarm.md)


![Android Map](./res/android_map.png)

[From @author 张兴业 http://blog.csdn.net/xyz_lmn](http://blog.csdn.net/xyz_lmn/article/details/41411355)


相关下载
[工具下载地址](./tool/)

# Doc
Just something markable.


1、ListView 中局部刷新Item

2、am
> am start -n "com.pp.assistant/.activity.PPMainActivity"  --ei "key_show_fg_index" 2 --ei "key_curr_frame_index" 2 --ez "key_from_jump" true 

3、pm

[国内Top500Android应用分析报告](http://mp.weixin.qq.com/s?__biz=MzA5OTMxMjQzMw==&mid=2648112527&idx=1&sn=b23c1b5f3e32e343ad96d705bd4d63ff&scene=2&srcid=0711GL3B90iyRPmjRKTBN1I0&from=timeline&isappinstalled=0#wechat_redirect)


[图集功能，查看大图，进行手势缩放](https://github.com/crazyandcoder/ImageZoom)

[虎三说Handler](./android_handler.md)

[Android快速集成微信支付](http://sunzq1993.com/2017/02/17/Android%E5%BF%AB%E9%80%9F%E9%9B%86%E6%88%90%E5%BE%AE%E4%BF%A1%E6%94%AF%E4%BB%98/)

[Android逆向之旅---带你爆破一款应用的签名验证问题](http://blog.csdn.net/jiangwei0910410003/article/details/54629728)

[关于Android中为什么主线程不会因为Looper.loop()里的死循环卡死？引发的思考，事实可能不是一个 epoll 那么 简单。](http://www.cnblogs.com/linguanh/p/6412042.html)

[Android中View的绘制流程](https://gold.xitu.io/post/58aaac80b123db00671da58a?utm_source=gold_browser_extension)

[自定义View系列：打造一个显示密码等级的控件](https://philipdroid.github.io/2016/10/26/%E4%B8%80%E4%B8%AA%E6%98%BE%E7%A4%BA%E5%AF%86%E7%A0%81%E7%AD%89%E7%BA%A7%E7%9A%84%E6%8E%A7%E4%BB%B6PasswordLevelView/)

[重要-作为Android开发者必须了解的Gradle知识](http://www.jianshu.com/p/c31513f5f550?utm_campaign=hugo&utm_medium=reader_share&utm_content=note)


[面试感悟：一名3年工作经验的程序员应该具备的技能](http://mp.weixin.qq.com/s?__biz=MzI1MTA3Mzk4Mg==&mid=2651020152&idx=1&sn=31706c082faf35c74a5ac29efbbbc3aa&chksm=f20f7fa9c578f6bfab1d34c8a264541e625b0a0c8ce4a1900549d62e7f0309647eab7168ecd1&mpshare=1&scene=1&srcid=0217VZ9RuHDTbYXKHfJcOELH#rd)

### [Andorid事件分发机制](./android_event.md)

[Android 进程保活](./android_process.md)

[Android 反射机制](./android_reflection.md)

[Android热修复升级探索——追寻极致的代码热替换](http://www.atatech.org/articles/72533)

[Android Classloader热修复技术之百家齐放](http://blog.csdn.net/sbsujjbcy/article/details/51760578)

[Android消息机制—sdk源码解读之旅](https://zhuanlan.zhihu.com/p/25222485?utm_source=gank.io&utm_medium=email&refer=levent-j)

[【FreeBuf年终策划】2017年最好用的Android渗透工具合集](http://www.freebuf.com/sectool/124507.html)

[Android技术周报](http://www.atatech.org/articles/73880/?flag_data_from=mail_daily_recommend&uid=130616)


[jsoup爬虫获取数据](http://blog.csdn.net/qq_30379689/article/details/55005796)

Crash搜集
https://bugly.qq.com
Bugly，腾讯出品的SDK，对Crash搜集的体验非常赞，能搜集到JNI层的奔溃以及监控线上的ANR问题。

https://try.crashlytics.com/
Crashlytics，国外的一个SDK，我自己没用过，但是用过的朋友对它的评价颇高。

https://github.com/ACRA/acra
ARCA，一个开源的崩溃日志搜集器，轻松让你实现客户端的崩溃日志上传到后台，如果你不喜欢接入别人家的SDK，可以使用它。有一个不足之处，就是它搜集不到JNI层的奔溃。

https://github.com/google/android-classyshark
Classyshark，轻松查看apk内部每个包的方法数，用了哪些开源库，同样拿知乎开刀做例子。

https://github.com/JesusFreke/smali/wiki/smalidea
smali代码调试插件，你以为没有拿到安卓Java源码就不能调试了吗？图样图森破了吧。

https://www.hex-rays.com/products/ida/
IDA Pro，逆向大利器，不管你是smali还是so文件，照样动态调试你。

注意，这些用来涨知识就好，别干坏事！

https://github.com/zzz40500/GsonFormat
根据JSON数据快速生成Java实体类，又一波解放生产力。

性能优化
http://hukai.me/
胡凯，腾讯开发者，翻译了一系列的Google Android性能优化典范的文章。

[Android性能优化（一）之启动加速35%](https://juejin.im/post/5874bff0128fe1006b443fa0)

[应用程序启动速度提升60% !](https://juejin.im/entry/5b8134cdf265da434a1fce4b?utm_source=gold_browser_extension)

https://hujiaweibujidao.github.io/
Hujiawei，魅族开发者，博客最近经常更新Android性能数据搜集统计的相关的文章，本人受益匪浅。

[【掌阅出品】android 提升布局加载速度200%（X2C）](https://www.jianshu.com/p/c1b9ce20ceb3)

[Android APK 瘦身 - JOOX Music项目实战](https://mp.weixin.qq.com/s/9IGYG6hNKL1V7N_p16p2Hg)

* [反编译](./android_decomplie.md)

通过Android API Hook技术，即通过动态代理等方法替换关键节点

[Android 插件化系列第（一）篇 ---Hook 技术之 Activity 的启动过程拦截](https://juejin.im/entry/58a15712b123db16a3e0afc1)

[几个不错的Android开源音视频播放器](https://mp.weixin.qq.com/s/-QYABYGPBhPXQu06drmpUA)

[Android逆向从未如此简单](https://juejin.im/post/58cc92a1b123db00532757cc?utm_source=gold_browser_extension)

[Android App 沉浸式状态栏解决方案](http://jaeger.itscoder.com/android/2016/02/15/status-bar-demo.html)

[线程池运行原理分析](http://www.jianshu.com/p/edab547f2710)


[Android卸载监听实现](http://www.jianshu.com/p/189e319a5c45)

[提高代码质量－工具篇](http://www.atatech.org/articles/58486)

[Android性能：远程触发GC](http://www.atatech.org/articles/58479)

[Android性能：Release版如何排查CPU占用率高的问题](http://www.atatech.org/articles/58453)

[Android上如何在发生崩溃时抓取日志](http://www.atatech.org/articles/58418)

在开发，测试，灰度，发布各个阶段对性能问题的关注和诉求是不一样的，所采取具体措施也有所不同。例如开发阶段主要是防止低性能的设计，编码，例如可以通过IDE插件在编码阶段就对低性能的代码进行告警。测试阶段，可以基于实验室环境，对卡顿，流量，IO，内存，启动速度等等性能数据进行详尽的自动化测试，越详细越好。灰度阶段是基于线上环境，对一些核心性能点进行监测。发布后则只针对一些核心指标进行粗粒度的监控。


[Android中杀进程的几种方法 (1) - killBackgroundProcesses](http://www.atatech.org/articles/57816)

[章鱼店长Andfix接入与插件扩展](http://www.atatech.org/articles/57735)

[Facebook Android Dex优化工具ReDex初探](http://www.atatech.org/articles/53348)

[EasyAdapter for Android 高效列表开发解决方案](http://www.atatech.org/articles/57535)

[Android 新权限系统，及使用AOP进行适配](http://www.atatech.org/articles/57176)

[天猫Android性能优化2——文件IO如何设置Buffer](http://www.atatech.org/articles/56936)

[Android解决ImageSpan不居中的问题](http://www.atatech.org/articles/57158)

[Android 流量优化(一)：模块化流量统计](http://www.atatech.org/articles/56540)

[Android上使用SVG矢量图](http://www.atatech.org/articles/56499)

[BlockCanary — 轻松找出Android App界面卡顿元凶](http://www.atatech.org/articles/56493)

[简单的安卓monkey测试脚本](http://www.atatech.org/articles/56363)

[Android 方法数杂谈](http://www.atatech.org/articles/56219)

[优化dex文件方法数超65535的一种思路](http://www.atatech.org/articles/55152)

[Android 流式布局组件 MMCherryUI](http://www.atatech.org/articles/55379)

[进程保活](http://www.atatech.org/articles/55546)

[无痕换肤的实现方案 for Android](http://www.atatech.org/articles/24389)

[浮窗系列之越过授权使用浮窗](http://www.atatech.org/articles/55874)

[浮窗系列之窗口层级](http://www.atatech.org/articles/55780)

[DumpTool----操作Android app 私有文件的利器！](http://www.atatech.org/articles/55926)

[Android Plugin](./android_pulgin.md)

[Android Accessibility](./android_accessibility.md)

[异常处理](http://geek.csdn.net/news/detail/50839)

[高斯模糊](./andorid_blur.md)


### 编码规范
* [编码规范](./android_style.md)

### 工具
* [adb](./android_adb.md)
* [Android 工具](http://www.androiddevtools.cn/)

### android SDK
https://realm.io/cn/news/oredev-ty-smith-building-android-sdks-fabric/

### 伟大的 SDK Fabric SDK
易用、稳定、轻巧、灵活，很好的支持
Easy to Use
Stable
LightWeight
Flexable
WellSupport

[Design of Android application for developers, what is your favorite approach?](https://medium.com/@lolevsky/design-of-android-application-for-developers-what-is-your-favorite-approach-d16e23cf2ce3#.ur3cqnbum)

SuperToasts

#### 问题
Sdk日志跟服务器上不一致

#### 不重复发明轮子
引进一个新的日志收集模块
* 方案一：使用成熟的日志收集模块，PP使用的
* 方案二：重新写一套

考虑因素
* 实现成本（时间和人力）
* 稳定性

决定使用方案一 使用现有的

引入日志模块时候问题：
1、大量使用全局变量 Application获取context，在Library中却无法直接访问Application，为了解耦，应该当成参数传进来

#### 大量使用Application中Context在项目移动，代码复用的时候就是一场灾难，要一直改Context，所以，为了保持代码解耦，通过传参的方式

通过AndroidStudio可以将不同的模块组合使用，尽量保持解耦。


### 如何实现一个日志模块

#### 数据收集
#### 数据保存 内存 本地持久化
#### 数据上传

JSPatch

拥有的不过是岁月留下来的经验，而不是智慧。
使用的场景是什么，如何优化，针对


优化卡顿
初始化启动卡顿 1.92%降低到0.03%
广告请求的卡顿从 1.32%降低到0.02%

### Android Map  

### Android 基础
[Context都没弄明白，还怎么做Android开发？](http://www.jianshu.com/p/94e0f9ab3f1d)
[Android 消息机制学习](http://www.jianshu.com/p/1e5640e6bef9#)

### [View](./android_view.md)

### 多线程
[Android多线程全面解析：IntentService用法&源码](http://www.jianshu.com/p/8a3c44a9173a)

### Android Fragment 
[Android Fragment 的使用，一些你不可不知的注意事项](http://www.jianshu.com/p/3a101ce9e04d)

### 动画
[Android 动画 爱范儿是如何让详情页缩小为横向列表的](https://gold.xitu.io/post/584d8fd38e450a006ac7b0c6)
[android 软软的动画弹出菜单，基于Facebook的Rebuond](https://gold.xitu.io/post/5849658161ff4b006cb86031)
[Android轻松实现RecyclerView悬浮条](http://www.jianshu.com/p/fe69a53502ab)
[模仿荷包启动动画](http://www.jianshu.com/p/50c358e2155a)
[模拟自然动画的精髓——TimeInterpolator与TypeEvaluator](https://blog.csdn.net/eclipsexys/article/details/52693324)

### 动态加载
[利用动态加载实现手机淘宝的节日特效](http://www.jianshu.com/p/195eb1d8d0de)

[Android热补丁动态更新实践](http://www.jianshu.com/p/f8edac2f7487)

[热补丁方案](http://blog.zhaiyifan.cn/2015/11/20/HotPatchCompare/)

[安卓App热补丁动态修复技术介绍](https://mp.weixin.qq.com/s?__biz=MzI1MTA1MzM2Nw==&mid=400118620&idx=1&sn=b4fdd5055731290eef12ad0d17f39d4a)

[美团动态加载框架](http://tech.meituan.com/mt-android-auto-split-dex.html)
[Android热更新方案Robust](http://tech.meituan.com/android_robust.html)

### 拍照
[你需要知道的Android拍照适配方案](http://www.jianshu.com/p/f269bcda335f)

### 分辨率适配
[（全解析）屏幕尺寸，分辨率，像素，PPI之间到底什么关系？](http://www.jianshu.com/p/c3387bcc4f6e)

### 单元测试
[在Android Studio中进行单元测试和UI测试](http://www.jianshu.com/p/03118c11c199)

### 工具
[ADB](./awesome_adb/README.md)

### 反编译
[Android apk 的反编译及保护杂谈](http://www.jianshu.com/p/caee73ca8963)

### 发布
[发布Android studio项目到本地Maven仓库](http://www.jianshu.com/p/8d7d0cc8fcc3)
[Android Studio发布库到Jcenter并推送到Maven Central最新超级详细!!!](http://www.jianshu.com/p/fb1e0fb2d966)
[Android Studio提交库至Bintray jCenter从入门到放弃](http://www.jianshu.com/p/31410d71eaba#)

### 架构
[MVP](http://www.jianshu.com/p/7567ed0d1853)

[MVVM](./Architecture/MVVM.md)

### [组件化](./Architecture/README.md)


### [开源项目](./android_open_source.md)

### 产品
[如何用一周时间开发一款Android APP并在Google Play上线](http://www.jianshu.com/p/b08e3ef22bce)
[从零开始开发一款Android App](http://www.jianshu.com/p/a58d15ef5c8b)
[干货IO](http://www.jianshu.com/p/d92258d76f7e?hmsr=toutiao.io&utm_medium=toutiao.io&utm_source=toutiao.io)
[我的第一款全栈side project](http://www.jianshu.com/p/39dce598faf1)
[开发一个简易的干货客户端](http://www.jianshu.com/p/fbfb666bad31)
[我的吱吱：视频图片新闻应用](http://www.jianshu.com/p/e2a8c34932a6)
[如何在没有官方API的情况下写一个第三方客户端](http://www.jianshu.com/p/b235773063e3)
[[干货]如何在一天之内完成一款具备cool属性的Android产品<简诗>](http://www.jianshu.com/p/cf496fc408b2)
[一个27岁零基础无业游民的第一个开源作品](http://www.jianshu.com/p/aef225ae1502?hmsr=toutiao.io&utm_medium=toutiao.io&utm_source=toutiao.io)
[「daza.io」这将是我独立完成全端开发的项目](http://www.jianshu.com/p/0b6e630e5cf2?hmsr=toutiao.io&utm_medium=toutiao.io&utm_source=toutiao.io)


[2017年初你绝对想尝试的25个新安卓库](http://www.jcodecraeer.com/a/anzhuokaifa/androidkaifa/2017/0216/7122.html)

### 开发资源
[我是如何自学Android，资料分享（2015 版）](http://www.jianshu.com/p/874ff12a4c01)
[Android最全开发资源](http://www.jianshu.com/p/0c36302e0ed0?hmsr=toutiao.io&utm_medium=toutiao.io&utm_source=toutiao.io)
[为开发者准备的最佳 Android 函数库（2016年版）](http://www.jianshu.com/p/3baf4b4f34b6)

[有赞移动技术团队](https://youzanmobile.github.io/)

[底层原理](http://qiangbo.space/)

[Android架构学习](http://qiangbo.space/2016-09-05/AndroidAnatomy_Introduction/)

[深入理解ServiceManager](https://pqpo.me/2017/04/26/learn-servicemanager/)

[深入理解MessageQueue](https://pqpo.me/2017/05/03/learn-messagequeue/)

[Android 模块化探索与实践](http://baronzhang.com/blog/Framework/Android-%E6%A8%A1%E5%9D%97%E5%8C%96%E6%8E%A2%E7%B4%A2%E4%B8%8E%E5%AE%9E%E8%B7%B5/)

### 其他
StackTraceElement
利用线程运行栈StackTraceElement设计Android日志模块

[[Android]动态更换应用Icon](http://www.jianshu.com/p/eecfd9e0b878)

### [异常处理相关](./UncaughtExceptionHandler.md)

### [网络模块](./andorid_internet.md)

### [Resource](./android_resource.md)

Talk is cheap,show me the code.

![](./res/android_tool_2015.jpg)


### Project
[途牛App](https://mp.weixin.qq.com/s/CfPlVKElv2SshAbfzHfRhg)

[美团点评技术团队](http://tech.meituan.com/)

### 总结归纳
[随想录:开发一流Android SDK](http://blog.csdn.net/dd864140130/article/details/53558011)
[android日常开发总结的技术经验60条](http://www.vmatianyu.cn/summarization-of-technical-experience.html)

[设计模式](http://blog.csdn.net/hguisu/article/category/1133340)

错题集

android.os.BadParcelableException: ClassNotFoundException when unmarshalling
http://blog.csdn.net/lincyang/article/details/7095417

[TODO](./todo.md)


[ARouter](https://juejin.im/entry/5b72331e6fb9a009b16d42ae?utm_source=gold_browser_extension)

### 案例实践
[图片颜色提取 Android Palette](./android_case.md)

### 源码阅读
[Android LayoutInflater](https://www.jianshu.com/p/f0f3de2f63e3)
[Android LayoutInflater Factory 源码解析](https://juejin.im/post/5b52ee765188251b176a666d)

[反作弊](./android_anticheat.md)

#### 体系
[蜂鸟团队移动端异常监控体系建设](https://juejin.im/post/5b874cbce51d4538b77667e3?utm_source=gold_browser_extension)


# 主题切换
[Android App切换主题的实现原理剖析](https://blog.csdn.net/watertekhqx/article/details/51320515)
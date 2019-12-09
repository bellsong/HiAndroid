
# KEY WORD

推荐引擎 作用？实现？

## Android 录音机实现

1. 提供提供的接口
2. 需要的权限
3. 注意事项

### 数据收集的工具

* 手机硬件信息

* 用户信息

* 手机其他应用信息

* 行为数据

关于数据收集的SDK分为几类
1、一般信息收集，例如应用安装列表，应用安装卸载等；

2、需要ROOT权限收集的数据

3、需要预研


监听手机上浏览器的App访问的url

监听本地端口，通过抓包的方式获取到手机访问的地址

生态圈中影响的范围，作用，产生的作用

### 工具类
清理工具

通知点击情况、剪切板、URL调用默认浏览器、浏览器内访问哪些URL、访问时长……等等

1. 调研监听并获取用户的粘贴板
2. 调研监听并获取用户的UA Cookies Http网络请求 流量等等
3. 安装途径，渠道号，分析APK等等
4. 多个应用同时植入SDK后，如何只提交1份数据(数据去重)
5. SDK动态添加竞品，行为等

### Api抓取

网络请求 OKHttp
数据转化 GSON
数据存储 SharePreference

UI展示 View

### 面试题整理

Android里面的线程模式

1、一般线程是区分，使用场景

2、线程模型的事件

3、线程的效率

4、性能的监控

单例中成员变量遇上多线程时候

单例中的成员变量被多线程异步修改，可能导致原来的逻辑无法正常执行。

修改的方案：将单例中的成员变量修改成跟随着对象走，成为对象的属性。

当前类中，需要修改的有 mCurrentAdapter mCurrentState
 
多线程的处理问题，并发，异步，同步锁

结构化体系化思维能力，要说明白：问题是什么？我要做什么？解决途径是什么？进度在解决途径中体现出来即可，主要体现我们对用户增长这个事情的理解


### [图片资源](https://zhuanlan.zhihu.com/p/26073777)

### [设计资源](https://zhuanlan.zhihu.com/p/20321959)

[如何评价 Google 的 Fuchsia、Android、iOS 跨平台应用框架 Flutter？](https://www.zhihu.com/question/50156415)

[程序员的书籍资源](https://zhuanlan.zhihu.com/p/23857699?hmsr=toutiao.io&utm_medium=toutiao.io&utm_source=toutiao.io)



名称	项目信息	仓库地址
FFmpeg	FFmpeg是一个自由软件，可以运行音频和视频多种格式的录影、转换、流功能[1]，包含了libavcodec.	https://github.com/FFmpeg/FFmpeg
achartengine	一个图表库,上次更新是10月前,现在用的较多的是----MPAndroidChart	https://github.com/ddanny/achartengine
CircleImageView	一个圆角图片库,比较轻量,就一个类.	https://github.com/hdodenhof/CircleImageView
TheMVP	MVP模式突然很火,可能是代码写习惯了,换换风格,哈哈,如作者所说,一个MVP框架	https://github.com/kymjs/TheMVP
androidquery	3年前#过时 一个轻量级的Android开源框架，可以简化开发Android的一些代码量和工作量.	https://github.com/androidquery/androidquery
gson	Google官方的Json解析,同类型的还有原生Json解析和fastjson	https://github.com/google/gson
DiskLruCache	JakeWharton主导又一热门项目----Android"硬盘"缓存	https://github.com/JakeWharton/DiskLruCache
tagsoup	Html解析	https://github.com/ndmitchell/tagsoup
ViewPagerIndicator	JakeWharton主导的项目,所以不用说了吧,很老的项目了,学习还是很不错的,Github上衍生出了很多,尝试搜索"Indicator"	https://github.com/JakeWharton/ViewPagerIndicator
wire	项目简介: Clean, lightweight protocol buffers for Android and Java.	https://github.com/square/wire
okio	java IO框架 :Okio是一个新的库，补充java.io和java.nio，使它更容易访问，存储和处理您的数据。	https://github.com/square/okio
ijkplayer	Bilibili开源的视频播放库,用过的都说好=.=	https://github.com/Bilibili/ijkplayer
dagger	Square 公司开源的 一种针对Android和Java的快速依赖注入器	https://github.com/square/dagger
jackson-databind	解放双手,减少重复代码编写量	https://github.com/FasterXML/jackson-databind
jackson-core	jackson 的核心支持库	https://github.com/FasterXML/jackson-core
jackson-annotations	一个注解库,不熟悉T_T	https://github.com/FasterXML/jackson-annotations
DanmakuFlameMaster	Bilibili开源的中二病开源弹幕引擎----烈焰弹幕使	https://github.com/Bilibili/DanmakuFlameMaster
android-stackblur	图像处理----高斯模糊	https://github.com/kikoso/android-stackblur
Android-Charts	又一个图表库	https://github.com/limccn/Android-Charts
android-supprt-library	Google支持库,例如:v4,v7,v13,v21...	国内版https://developer.android.google.cn/index.html
NineOldAndroids	动画兼容库,最低支持sdk14	https://github.com/JakeWharton/NineOldAndroids
Leakcanary	Android 内存泄漏检测库	https://github.com/square/leakcanary
EventBus	Android 事件总线,使用方便,但不易维护	https://github.com/greenrobot/EventBus
androidannotations	又一个注解库	https://github.com/androidannotations/androidannotations
fastjson	阿里出品的Json解析库	https://github.com/alibaba/fastjson
okhttp	网络请求框架,目前最好没有之一	https://github.com/square/okhttp
OpenSSL	网络安全	https://github.com/openssl/openssl
duktape	一个轻量级的嵌入式 JavaScript 引擎, 专注于可移植性和低占用率.	https://github.com/svaarala/duktape
ProgressWheel	环形进度的UI库	https://github.com/Todd-Davies/ProgressWheel
android-gif-drawable	让Android 显示 Gif 动图	https://github.com/koral--/android-gif-drawable
zlib	数据压缩用的库	https://github.com/madler/zlib
libyuv	libyuv是Google开源的实现各种YUV与RGB之间相互转换、旋转、缩放的库.	https://github.com/lemenkov/libyuv
css-layout	Facebook开源跨平台前端布局引擎Yoga	https://github.com/facebook/yoga
aspectj	一个面向切面的框架,它扩展了Java语言.	https://github.com/eclipse/org.aspectj
libjpeg-turbo	libjpeg 是一个完全用C语言编写的库，包含了被广泛使用的JPEG解码、JPEG编码和其他的JPEG功能的实现。libjpeg-turbo 是一个专门为 x86 和 x86-64 处理器优化的高速 libjpeg 的改进版本。	https://github.com/libjpeg-turbo/libjpeg-turbo

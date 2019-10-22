# Android日志收集


### 埋点
[JJEvent 一个可靠的Android端数据埋点SDK](https://juejin.im/post/5bbdca89e51d450e92526a3b?utm_source=gold_browser_extension)

[Android无埋点数据收集SDK关键技术](https://www.jianshu.com/p/b5ffe845fe2d)

Android程序log追踪工具_cooker-tracer

[Android打印Trace堆栈](http://gityuan.com/2017/07/09/android_debug/)

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

StackTraceElement
利用线程运行栈StackTraceElement设计Android日志模块

### 如何实现一个日志模块

#### 数据收集
#### 数据保存 本地持久化
#### 数据上传


android:分享 一个很强大的LOG开关---Log.isLoggable

### [Log.isLogggable](https://developer.android.com/reference/android/util/Log.html)

adb shell setprop log.tag.YOUR_LOG_TAG 
adb shell setprop log.tag.UNIONADS D

### 日志记录框架

日志记录无论在服务端开发还是移动端开发，都是一个基础且重要的能力，开发人员在代码调试以及错误定位过程中，大多说都要依赖日志信息，一个简洁灵活的日志记录模块是相当重要的。Logger（https://github.com/orhanobut/logger） 是基于系统Log类基础上进行的封装，但新增了如下超赞的特性。

1. 在Logcat中完美的格式化输出，再也不用担心和手机其他APP或者系统的日志信息相混淆了
2. 包含线程、类、方法信息，可以清楚地看到日志记录的调用堆栈
3. 支持跳转到源码处
4. 支持格式化输出JSON、XML格式信息

### 参考
[android 4.2.1 一种高效log打开方式](http://blog.csdn.net/maybe_windleave/article/details/8742178)


1、API亮点：

此API可以实现不更换APK，在出问题的手机上就直接能抓到有效log，能提升不少工作效率。

2、API介绍

最近在解决短信问题时，看到一个很强大的LOG开关---Log.isLoggable

1. if (Log.isLoggable(LogTag.TRANSACTION, Log.VERBOSE)) {  

2.     Log.v(TAG, "Creating TransactionService");  

3. }  

 进入framework中查看isLoggable方法的定义，发现这是一个用了JNI的方法。不过方法声明上有很多注释。仔细看了下，发现这是android给大家的一个礼物。
原文大致意思：检查当前的tag是否在指定的log级别。一般默认的log级别是INFO，这也就意味着在这之上包括INFO的log都会被输出。你可以通过设置系统属性修改默认的log级别，执行如下命令即可：

1. setprop log.tag.;YOUR_LOG_TAG> ;LEVEL  

你也可以将这句代码写进local.prop文件里面，并且将这个文件放到/data/local.prop。根据上面的指导，试着执行下下面的命令：

1. adb shell setprop log.tag.Mms:transaction VERBOSE  

接着发了一条彩信试验，有效log都输出来了。

       我们在这个基础上在进行一些挖掘，看看google还给我们留了哪些惊喜。从上面第二段代码中的LogTag.TRANSACTION，可以看到Mms的包里面有个LogTag.java文件，发现里面果然声明了不少常量：

1. public static final String TRANSACTION = "Mms:transaction";  

2. public static final String APP = "Mms:app";  

3. public static final String THREAD_CACHE = "Mms:threadcache";  

4. public static final String THUMBNAIL_CACHE = "Mms:thumbnailcache";  

5. public static final String PDU_CACHE = "Mms:pducache";  

6. public static final String WIDGET = "Mms:widget";  

7. public static final String CONTACT = "Mms:contact";  

这一个个都是彩信里面重要log开关。知道了这些以后调试彩信就方便多了，不用换APK，在出问题的手机上就直接能抓到有效log，能提升不少工作效率。我们接着载回去看看isLoggable(Stringtag, int level)在哪里有调用，发现除了framework层和Mms应用，还有Contacts等重要模块有调用，以后调试这些模块都会方便很多。

https://github.com/orhanobut/logger

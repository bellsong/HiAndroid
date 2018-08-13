android:分享 一个很强大的LOG开关---Log.isLoggable

### [Log.isLogggable](https://developer.android.com/reference/android/util/Log.html)

adb shell setprop log.tag.YOUR_LOG_TAG 
adb shell setprop log.tag.UNIONADS D

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


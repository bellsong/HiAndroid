#如何解决65535限制？

##What?

如果你是一名幸运的Android应用开发者，正在开发一个前景广阔的应用，不断地加入新功能、添加新的类库，那么终有一天，你会不幸遇到这个错误：

Conversion to Dalvik format failed: Unable to execute dex: method ID not in [0, 0xffff]: 65536

65535 A DEX LIMIT

congratulations, you have a lot of code.

现象：无法正常安装apk。

##Why？

这个错误是Android应用的方法总数限制造成的。Android平台的Java虚拟机Dalvik在执行DEX格式的Java应用程序时，使用原生类型short来索引DEX文件中的方法。这意味着单个DEX文件可被引用的方法总数被限制为65536。通常APK包含一个classes.dex文件，因此Android应用的方法总数不能超过这个数量，这包括Android框架、类库和你自己开发的代码。

##How?

目前比较常用的方法：

(1) 应用插件化,比如使用我正在参与开发的插件化框架 : https://github.com/singwhatiwanna/dynamic-load-apk ,如果有建议或者相关的问题,欢迎到Github上积极参与. 

(2) 分割Dex,多工程: 把所需要的.class文件或者是Jar文件和一些源码一起编译生成一个Jar文件。然后使用Android SDK提供的dx工具把Jar文件转成Dex文件。(可参考facebook:https://www.facebook.com/notes/facebook-engineering/under-the-hood-dalvik-patch-for-facebook-for-android/10151345597798920,这里边还可以看到在2.3上动态改变LinearAlloc缓冲的解决思路) 

（3）删除无用的代码和第三方库

随着新的MultiDex支持库发布，Google正式为解决此问题提供官方支持。构建超过65K方法数的应用介绍了如何使用Gradle构建多DEX应用。

首先使用Android SDK Manager升级到最新的Android SDK Build Tools和Android Support Library R21。然后进行以下两步操作：

1.修改Gradle配置文件，启用MultiDex并包含MultiDex支持：

```
android { 
		compileSdkVersion 21 buildToolsVersion "21.1.0"
		defaultConfig {
    	...
    	minSdkVersion 14
    	targetSdkVersion 21
    	...

    	// Enabling multidex support.
    	multiDexEnabled true
		}
		...
		}

		dependencies { compile 'com.android.support:multidex:1.0.0' } 
```		
		
2.让应用支持多DEX文件。在MultiDexApplication JavaDoc中描述了三种可选方法：

* 在AndroidManifest.xml的application中声明
* android.support.multidex.MultiDexApplication；
如果你已经有自己的Application类，让其继承MultiDexApplication；

* 如果你的Application类已经继承自其它类，你不想/能修改它，那么可以重写attachBaseContext()方法：

```
@Override 
protected void attachBaseContext(Context base) {
    super.attachBaseContext(base); MultiDex.install(this);
}
```
经过以上步骤，你的应用已经可以实现多个DEX文件了。当应用构建时，构建工具会分析哪些类必须放在第一个DEX文件，哪些类可以放在附加的DEX文件中。当它创建了第一个DEX文件（classes.dex）后，如果有必要会继续创建附加的DEX文件，如classes2.dex, classes3.dex。Multidex的支持类库将被包含在应用的第一个DEX文件中，帮助实现对其它DEX文件的访问。

文中还介绍了在开发多DEX应用时，通过设置productFlavors提高开发效率以及多DEX应用的测试方法。

Android 5.0和更高版本使用名为ART的运行时，它原生支持从APK文件加载多个DEX文件。在应用安装时，它会执行预编译，扫描classes(..N).dex文件然后将其编译成单个.oat文件用于执行。了解更多关于ART的信息。

主要思路：
1、通过将一个DEX文件拆分成多个DEX文件解决；Facebook介绍了为Android应用开发的Dalvik补丁
2、Android Developers博客介绍了通过自定义类加载过程的方法来解决此问题。http://android-developers.blogspot.hk/2011/07/custom-class-loading-in-dalvik.html
But，并不优雅。
SO

## Advice

虽然Google解决了应用总方法数限制的问题，但并不意味着开发者可以任意扩大项目规模。Multidex仍有一些限制：

#### multidex存在的问题

1、应用启动速度会降低，由于用用启动时会加载额外的dex文件，将会造成启动速度降低，而且有可能会造成ANR,所以尽可能的第二个dex不要太大。此时应该使用ProGuard减小DEX文件的大小。

2、由于dalvik LinearAlloc的bug，这可能会导致multidex的应用无法在android4.0之前的手机上运行。

同样因为Dalvik linearAlloc的限制，如果请求大量内存可能导致崩溃。Dalvik linearAlloc是一个固定大小的缓冲区。在应用的安装过程中，系统会运行一个名为dexopt的程序为该应用在当前机型中运行做准备。dexopt使用LinearAlloc来存储应用的方法信息。Android 2.2和2.3的缓冲区只有5MB，Android 4.x提高到了8MB或16MB。当方法数量过多导致超出缓冲区大小时，会造成dexopt崩溃。
Multidex构建工具还不支持指定哪些类必须包含在首个DEX文件中，因此可能会导致某些类库（例如某个类库需要从原生代码访问Java代码）无法使用。

避免应用过大、方法过多仍然是Android开发者要注意的问题。

通常开发者自己的代码很难达到这样的方法数量限制，但随着第三方类库的加入，方法数就会迅速膨胀。因此选择合适的类库对Android开发者来说尤为重要。

举个例子：
开发者应该避免使用Google Guava这样的类库，它包含了13000多个方法。

尽量使用专为移动应用设计的Lite/Android版本类库，或者使用小类库替换大类库，例如用Google-gson替换Jackson JSON。而对于Google Protocol Buffers这样的数据交换格式，其标准实现会自动生成大量的方法。采用Square Wire的实现则可以很好地解决此问题。

##Other

Mihai Parparita的开源项目[dex-method-counts](https://github.com/mihaip/dex-method-counts)可以用于统计APK中每个包的方法数量。

##参考
[MultiDex官方教程](https://developer.android.com/studio/build/multidex.html)

[当Field邂逅65535](http://jiajixin.cn/2015/10/21/field-65535/)

[Android 使用android-support-multidex解决Dex超出方法数的限制问题,让你的应用不再爆棚](http://blog.csdn.net/t12x3456/article/details/40837287)

[](http://www.jianshu.com/p/d2d6e87e5a42)

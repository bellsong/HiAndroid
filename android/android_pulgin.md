#动态加载和插件化

##简介

1. 第一部分前世今生，即插件化的历史。

2. 第二部分是入门知识，即与插件化有关的Android系统底层的实现原理。

3. 第三部分是技术流派，即目前Android插件化多种技术流派及其具体不同的实现方式。

4. 第四部分是周边相关的技术实现。

5. 第五部分是目前国内流行的开源框架，包括各个公司正在使用的框架，还有流传不是很广但意义也很重大的框架。

6. 第六部分是针对Android插件化的一些问题的经验分享。

7. 第七部分是Android插件化的未来——我们是否该放弃这门技术。

引自[包建强：为什么我说Android插件化从入门到放弃？](http://mp.weixin.qq.com/s?__biz=MjM5MDE0Mjc4MA==&mid=2650993300&idx=1&sn=797fa87ef528cff3a50e77806cf9f675&scene=1&srcid=07124TeQfqvSzge8vCmJ66Oi#wechat_redirect)

## What?

#### 第一部分前世今生，即插件化的历史。

* 2012年7月 AndroidDynamicLoader 大众点评 屠毅敏
* 2013年 23Code 自定义控件动态下载
* 2014年初 Altas 阿里伯奎的技术分享
* 2014年底 Dynamic-load-aok 任玉刚
* 2015年4月 OpenAltas/ACDD
* 2015年8月 DroidPlugin 张勇
* 2015年9月 AndFix 阿里 黎三平
* 2015年11月 Nuwa 大众点评 贾吉鑫
* 2015年底 Small 林广亮

**Android插件化的历史，可能是本次演讲最有价值的内容。**

我梳理了三年前到现在Android插件化发展的一些规律。

首先，要记住2012年这个时间点。2012年的时候，就有人做插件化技术，是大众点评的屠毅敏，他推出了AndroidDynamicLoader框架，用Fragment来实现。大众点评是国内做App比较早的公司，他们积累了很多的经验，尤其是插件化技术 。通过动态加载不同的Fragement，把想换的页面都换掉。我们也是在这个项目中第一次看到了如何通过addAssetPath来读取插件中的资源。

2013年，出现了23Code。23Code提供了一个壳，在这个壳里可以动态下载插件，然后动态运行。可以在壳外编写各种各样的控件，放在这个框架下去运行。这就是Android插件化技术。这个项目的作者和开源地址，目前不是很清楚。

2014年初，大家也许看过一个视频，阿里一位员工做了一次技术分享，专门讲淘宝的Altas技术，以及这项技术的大方向。但是很多技术细节没有分享。

然后是任玉刚的里程碑式的项目。2014年底，玉刚发布了一个Android插件化项目，起名为dynamic-load-apk，这跟后续介绍的很多插件化项目都不太一样。它没有Hook太多的系统底层方法，而是从上层，即App应用层解决问题，创建一个继承自Activity的ProxyActivity类，然后让插件中的所有Activity都继承自ProxyActivity，并重写Activity所有的方法。

之所以说这个项目是里程碑式的，是因为在2015年之前业界没有太多资料可以参考。之后曾和玉刚聊天，他十分感慨当年如何举步维艰地开发这个框架。我当时在途牛工作，使用了这个Android插件化框架。

2015年4月，一个新框架推出来，叫OpenAltas，后来改名为ACDD。这个框架参考了淘宝App的很多经验，主要就是Hook的思想，同时，还首次提出来通过扩展AAPT来解决插件与宿主的资源id冲突的问题。

2015年8月，张勇发布DroidPlugin。这是Android插件化中第二个里程碑式的项目，这个项目太牛了，能把任意的App都加载到宿主里。可以基于这个框架写一个宿主App，然后就可以把别人写的App都当作插件来加载。这个框架的功能的确很强大，但强大的代价就是要改写很多Android系统的底层代码，更别提这哥们还比较懒，没有制订任何说明文档，导致技术人员掌握这个框架不太容易。360的田维术曾编写了一系列文章，专门介绍这个框架，后面我会介绍。

再之后就是百花齐放的时代了，GitHub上有很多插件化框架，但这些框架影响都不大，我们这里就略过了。

接下来登场的是热修复技术。2015年5月，iOS推出了JSPatch，JSPatch通过Runtime的机制，能迅速修复线上App任何一个类的任何一个方法。而当时的Android系统没有能迅速替换的方式。于是，在2015年9月，有人找到了实现迅速替换的途径，就是Andfix，后面会讲它的原理。

2015年10月，大众点评的贾吉鑫做了一个项目，起名为Nuwa（女娲），主要思路跟Andfix差不多，都是解决Android的修复问题，能修复线上的任何一个方法。可惜后来没有继续维护。

最后，2015年底，仍然是Android插件化框架，福建的林广亮提出了一个新机制——Small框架，这个机制不太一样的地方就是，通过脚本的方式来解决资源冲突的问题。



#### 第二部分是入门知识，即与插件化有关的Android系统底层的实现原理

* Binder
* App打包流程
* App安装流程
* App启动流程
* 资源加载机制
* Gradle

要想完全明白插件化技术，首先需要了解Android系统的底层实现。

**首先，做Android系统原代码的人应该非常熟悉Binder，如果没有它真的寸步难行。**Binder涉及两层技术。你可以认为它是一个中介者模式，在客户端和服务器端之间，Binder就起到中介的作用，这是我这段时间对Binder的思考。要实现四大组件的插件化，就需要在Binder上做修改。Binder服务端的内容没办法修改，只能改客户端的代码。四大组件每个组件的客户端都不太一样，这个需要大家自己去发现，时间关系，这里就不多说了。

**学习Binder的最好方式就是AIDL。**你可以读到很多关于AIDL的资料，通过制订一个aidl文件自动生成一个Java类，研究一下这个Java类的每个方法和变量，然后再反过来看四大组件，其实都是跟AIDL差不多的方式。

**其次，是App打包的流程。**代码写完了，执行一次打包操作，中途经历了资源打包、dex生成、签名等过程。其中最重要的就是资源的打包，即AAPT这一步，如果宿主和插件的资源id冲突，一种解决办法就是在这里做修改。

**第三，App在手机上的安装流程也很重要。**熟悉安装流程不仅对插件化有帮助，在遇到安装bug的时候也非常重要。手机安装App的时候，经常会有下载异常，提示资源包不能解析，这时需要知道安装App的这段代码在什么地方，这只是第一步。

第二步需要知道，App下载到本地后，具体要做哪些事情。手机有些目录不能访问，App下载到本地之后，放到哪个目录下，然后会生成哪些文件。插件化有个增量更新的概念，如何下载一个增量包，从本地具体哪个位置取出一个包，这个包的具体命名规则是什么，等等。这些细节都必须要清楚明白。

**第四，是App的启动流程。**Activity启动有几种方式？一种是写一个startActivity，第二种是点击手机App，通过手机系统里的Launcher机制，启动App里默认的Activity。通常，App开发人员喜闻乐见的方式是第二种。那么第一种方式的启动原理是什么呢？另外，启动的时候，main函数在哪里？这个main函数的位置很重要，我们可以对它所在的类做修改，从而实现插件化。

**第五点更重要，做Android插件化需要控制两个地方。**首先是插件Dex的加载，如何把插件Dex中的类加载到内存？另外是资源加载的问题。插件可能是apk也可能是so格式，不管哪一种，都不会生成R.id，从而没办法使用。这个问题有好几种解决方案。

一种是是重写Context的getAsset、getResource之类的方法，偷换概念，让插件读取插件里的资源，但缺点就是宿主和插件的资源id会冲突，需要重写AAPT。另一种是重写AMS中保存的插件列表，从而让宿主和插件分别去加载各自的资源而不会冲突。第三种方法，就是打包后，执行一个脚本，修改生成包中资源id。

**第六点，在实施插件化后，如何解决不同插件的开发人员的工作区问题**。比如，插件1和插件2，需要分别下载哪些代码，如何独立运行？就像机票和火车票，如何只运行自己的插件，而不运行别人的插件？这是协同工作的问题。

火车票和机票，这两个Android团队的各自工作区是不一样的，这时候就要用到Gradle脚本了，每个项目分别有各自的仓库，有各自不同的打包脚本，只需要把自己的插件跟宿主项目一起打包运行起来，而不用引入其他插件，还有更厉害的是，也可以把自己的插件当作一个App来打包并运行。

上面介绍了插件化的入门知识，一共六点，每一点都需要花大量时间去理解。否则，在面对插件化项目的时候，很多地方你会一头雾水。而只要理解了这六点核心，一切可迎刃而解。



#### 第三部分是技术流派，

#### 即目前Android插件化多种技术流派及其具体不同的实现方式

* 动态替换 也就是Hook
* 静态代理
* Dex合并

**技术流派目前分三种。**

**第一种是动态替换，也就是Hook。**可以在不同层次进行Hook，从而动态替换也细分为若干小流派。

可以直接在Activity里做Hook，重写getAsset的几个方法，从而使用自己的ResourceManager和AssetPath；也可以在更抽象的层面，也就是在startActivity方法的位置做Hook，涉及的类包括ActivityThread、Instrumentation等；最高层次则是在AMS上做修改，也就是张勇的解决方案，这里需要修改的类非常多，AMS、PMS等都需要改动。总之，在越抽象的层次上做Hook，需要做的改动就越大，但好处就是更加灵活了。没有哪一个方法更好，一切看你自己的选择。

**第二种是静态代理，这是任玉刚的框架采取的思路。**写一个PluginActivity继承自Activity基类，把Activity基类里面涉及生命周期的方法全都重写一遍，插件中的Activity是没有生命周期的，所以要让插件中的Activity都继承自PluginActivity，这样就有生命周期了。

**第三种是Dex合并，Dex合并就是Android热修复的思想。**刚才说到了两个项目——AndFix和Nuwa，它们的思想是相同的。原生Apk自带的Dex是通过PathClassLoader来加载的，而插件Dex则是通过DexClassLoader来加载的。但有一个顺序问题，是由Davlik的机制决定的，如果宿主Dex和插件Dex都有一个相同命名空间的类的方法，那么先加载哪个Dex，哪个Dex中的这个类的方法将会占山为王，后面其他同名方法都替换了。

所以，AndFix热修复就是优先加载插件包中的Dex，从而实现热修复。由于热修复的插件包通常只包括一个类的方法，体量很小，和正常的插件不是一个数量级的，所以只称为热修复补丁包，而不是插件。



#### 第四部分是周边相关的技术实现

* AAPT
* 增量更新
* 插件管理平台

关于技术周边的内容有三个部分。

**首先是AAPT，资源冲突**，就是说默认APP应用，插件里的资源和数据资源冲突，如果不引入这个资源，相安无事。很多时候就算有冲突也无所谓，问题就出在插件引用资源的时候有冲突了，无法解决，怎么办？那就要立刻改写App。有一个关于打包的App，可以加当前的前缀，改成你想要的。比如，火车票和酒店分别取名，这样就可以指定前缀、打包，插进一个模块，资源的前缀都不一样。小米也承认，会占用0x11这个前缀。这是需要关注的一个点。

**第二是增量更新。**360目前最牛逼的地方是，把所有数据跟之前一个版本差，产生增量的数据。他们当然也更新了插件化。360的刘存栋做了一个增量更新的框架。可以在后台服务器把两个版本的Android App做拆分，然后把增量包下载到本地，再跟本地进行合并，提供一个STK，再合在一起，这就是增量更新。

**第三是插件管理平台**，要管理每个版本的差异、每个插件最低数据的版本号。



#### 第五部分是目前国内流行的开源框架，包括各个公司正在使用的框架，还有流传不是很广但意义也很重大的框架

#### 第六部分是针对Android插件化的一些问题的经验分享

接下来是最近两年出现的一些尴尬困境。

**首先，插件化已经沦落为修bug的工具。**这跟插件化的初衷不一样，插件化是实现新功能，而不是修复bug。

新功能发布和热修复是两码事，但是我们把这二者都混在插件化中了。这就有问题了。热修复是很轻量级的东西，完全可以使用AndFix或Nuwa来解决，但是我们现在通常是使用发布新的插件来修复线上bug，一两天一个插件化版本，用户会不停的下载升级插件。

插件化是用来发布新功能的。一般来说，大版本是一个月一次，中途想上一个功能，这时候才是插件化的最好使用场景。所以说，要把新功能发布和热修复拆开成两套机制，而不要混为一谈。

再举个例子，就是用插件化来发布新功能。然后我就发现，在下一次发布大版本之前，只有50%的用户升级了插件并使用的是新功能。这是因为Dalvik的机制导致的，一旦加载了这个插件的原始版本，就会一直使用，即使你下载的新版本插件，也只能在App退出进程后重启才能生效。Android用户一般不会杀App的进程，也就我们这些程序员或发烧友才知道要去什么地方杀进程。为了解决这个尴尬的问题，我们曾经试图把App做成多进程的，每个模块的插件都是一个单独的进程，插件升级会自动杀之前插件的进程，然后再启动新的进程来运行新版本的插件。这就是张勇的DroidPlugin的思想，在他的框架出来之前，国内的插件化机制都是单进程的，都有我刚才说的这个问题。然后就有人给出一种解决方案让App崩溃后重启，虽然也能解决问题，但就会产生一种搞笑的场景，用户正在酒店模块下单，这时因为机票模块的插件升级而重启，这是很让人抓狂的事情。

**其次，插件化现在有一个更好的替代品——RN。**接下来，RN会是真正实现动态化的最佳方式，至少我是这么认为的。

**第三，插件化技术只在中国有市场。**国外的公司根本不看好这项技术，这可能是因为他们用GooglePlay，而谷歌官方不建议用插件化这种方式。国外开发者不敢越雷池半步。大概是老外觉得线上有崩溃或者严重错误，发个新版本让用户更新就是了，不是什么大不了的事情。

**第四，四大组件都需要做插件化吗？**根据我自己的经验，做一款电商或旅游类的App，有一两百个Activity，Service用得很少，ContentProvider和BroadcastReceiver基本不用。所以，这种App实现Activity和Service的插件化就够了。像手机助手这样的App，非常频繁使用四大组件，所以四大组件都必须实现插件化，这也是张勇当年在360开发出DroidPlugin支持四大组件的原因。

- Service这个组件，大都用于在线音乐的App，因为后台也要能播放音乐。此外，不管是哪款App，只要涉及到聊天，都需要Service插件来帮忙修改App图标上的未读消息数。
- Broadcast和ContentProvider什么时候使用？像手机助手、安全卫士这类的App会着重依赖于这些技术，所以你会看到DroidPlugin必须要实现这两个组件的插件化技术，因为作者是在360这家公司，而任玉刚的开源框架（目前途牛在使用），以及携程的框架，他们只支持了Activity和Service就够了，因为大多数电商App有这两个就够了。

#### 第七部分是Android插件化的未来——我们是否该放弃这门技术

**阿里一位技术专家冯森林曾说，插件化最厉害的方向应该是每个Activity都是一个插件。**这个观点在插件化技术交流群里一提出来之后，群里所有人都沉默良久。仔细想想，插件化的未来好像的确是这个发展方向，这样就可以将任何一个出问题的Activity迅速替换。但当RN一经提出，这个观点就慢慢消失了，**RN比插件化更轻量级，越来越多人选择了RN。**

**其次，就是双开技术。**双开技术是现在非常火的一项技术。如果想实现这种机制，一定要实现上面讲的插件化所涉及的内容。宁波一位高中生Lody，他从初三就开始研究这门技术，将很多不错的双开项目放在GitHub上。

**第三，刚才讲的所有数据都增量下载的机制，大家都可以实施一下，虽然做起来很麻烦，但是一旦实现，会让你的App非常快。**比如，你每次进入都需要刷新城市列表吗？大约几百KB，即使你开gzip，刷新速度仍然很慢，这时候增量更新就是一个很好的方式。

**最后是内功的修炼。**

通读一遍上面列出来的基础知识，然后再去做App应用，你会清楚知道静态广播、动态广播具体什么时候用，什么情况下可能出bug。AIDL可能会很少用，但真正做设计和实现的时候，这个基本功就非常重要了。所以，插件化只是一门技术，你最应该关注的是其背后的本质，也就是内功修炼。

##Why?

解决的问题

* 65535方法数限制

  * Android的APK安装包将编译后的字节码放在dex格式的文件中，供Android的JVM加载执行。不幸的是，单个dex文件的方法数被限制在了`65536`之内，这其中除了我们自己实现的方法之外，还包括了我们用到的Android Framework方法、其他library包含的方法。如果我们的方法总数超过了这个限制，那么我们在尝试打包时，会抛出异常

* APK安装失败

  * Android官方推荐了一个叫做[MultiDex](https://developer.android.com/studio/build/multidex.html)的工具，用来在打包时将方法分散放到多个dex内，以此来解决65K方法数的问题。但是，除此之外，方法数过多还会带来dex文件过大的问题。

    在安装APK时，系统会运行一个叫做`dexopt`的程序，dexopt会使用`Dalvik LinearAlloc`缓冲区来存储应用的方法信息。在Android 2.x的系统中，该缓冲区大小仅为5M，当我们的dex文件过大超过该缓冲区大小时，就会遇到APK安装失败的问题。

引自 [Android动态加载插件APK](https://segmentfault.com/a/1190000006132827)

##How?

思路：对于如上的两个问题，有个非常有名的方案，就是采用动态加载插件化APK的方法。

插件化APK的思路为：将部分代码分离出来放在另外的APK中，做成插件APK的形式，在我们的应用程序启动后，在使用时动态加载该插件APK中的内容。

该思路简单来说便是将部分代码放在了另外一个独立的APK中，而不是放在我们自己的dex中。这样一方面减少了我们自己dex中方法总数，另一方面也减小了dex文件的大小，因此可以解决如上两个方面的问题。对于这个插件APK包含的类，我们可以在使用到的时候再加载进来，这便是动态加载的思路。

要实现插件化APK，我们只需要解决如下3个问题：

- 如何生成插件APK
- 如何加载插件APK
- 如何使用插件APK中的内容



### 类加载器

在实现插件化APK之前，我们需要先了解一下Android中的类加载机制，作为实现动态加载的基础。



**动态加载的基础是ClassLoader**，从名字也可以看出，ClassLoader就是专门用来处理类加载工作的，所以这货也叫类加载器，而且一个运行中的APP **不仅只有一个类加载器**。

其实，在Android系统启动的时候会创建一个Boot类型的ClassLoader实例，用于加载一些系统Framework层级需要的类，我们的Android应用里也需要用到一些系统的类，所以APP启动的时候也会把这个Boot类型的ClassLoader传进来。

此外，APP也有自己的类，这些类保存在APK的dex文件里面，所以APP启动的时候，也会创建一个自己的ClassLoader实例，用于加载自己dex文件中的类。验证方法：



```java
  @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        ClassLoader classLoader = getClassLoader();
        if (classLoader != null){
            Log.i(TAG, "[onCreate] classLoader " + i + " : " + classLoader.toString());
            while (classLoader.getParent()!=null){
                classLoader = classLoader.getParent();
                Log.i(TAG,"[onCreate] classLoader " + i + " : " + classLoader.toString());
            }
        }
    }
```



输出结果：

```java
[onCreate] classLoader 1 : dalvik.system.PathClassLoader[DexPathList[[zip file "/data/app/me.kaede.anroidclassloadersample-1/base.apk"],nativeLibraryDirectories=[/vendor/lib, /system/lib]]]

[onCreate] classLoader 2 : java.lang.BootClassLoader@14af4e32
```



可以看见有2个Classloader实例，一个是BootClassLoader（系统启动的时候创建的），另一个是PathClassLoader（应用启动时创建的，用于加载“/data/app/me.kaede.anroidclassloadersample-1/base.apk”里面的类）。由此也可以看出，**一个运行的Android应用至少有2个ClassLoader**。



创建一个ClassLoader实例的时候，需要使用一个现有的ClassLoader实例作为新创建的实例的Parent。这样一来，一个Android应用，甚至整个Android系统里所有的ClassLoader实例都会被一棵树关联起来，这也是ClassLoader的 **双亲代理模型**（Parent-Delegation Model）的特点。



#### ClassLoader双亲代理模型加载类的特点和作用


JVM中ClassLoader通过defineClass方法加载jar里面的Class，而Android中这个方法被弃用了。

```java
  @Deprecated
    protected final Class<?> defineClass(byte[] classRep, int offset, int length)
            throws ClassFormatError {
        throw new UnsupportedOperationException("can't load this type of class file");
    }
```



取而代之的是loadClass方法

```java
public Class<?> loadClass(String className) throws ClassNotFoundException {
        return loadClass(className, false);
    }

    protected Class<?> loadClass(String className, boolean resolve) throws ClassNotFoundException {
        Class<?> clazz = findLoadedClass(className);

        if (clazz == null) {
            ClassNotFoundException suppressed = null;
            try {
                clazz = parent.loadClass(className, false);
            } catch (ClassNotFoundException e) {
                suppressed = e;
            }

            if (clazz == null) {
                try {
                    clazz = findClass(className);
                } catch (ClassNotFoundException e) {
                    e.addSuppressed(suppressed);
                    throw e;
                }
            }
        }

        return clazz;
    }
```



#### 特点

从源码中我们也可以看出，loadClass方法在加载一个类的实例的时候，

1. 会先查询当前ClassLoader实例是否加载过此类，有就返回；
2. 如果没有。查询Parent是否已经加载过此类，如果已经加载过，就直接返回Parent加载的类；
3. 如果继承路线上的ClassLoader都没有加载，才由Child执行类的加载工作；

这样做有个明显的特点，如果一个类被位于树根的ClassLoader加载过，那么在以后整个系统的生命周期内，这个类永远不会被重新加载。

#### 作用

首先是共享功能，一些Framework层级的类一旦被顶层的ClassLoader加载过就缓存在内存里面，以后任何地方用到都不需要重新加载。

除此之外还有隔离功能，不同继承路线上的ClassLoader加载的类肯定不是同一个类，这样的限制避免了用户自己的代码冒充核心类库的类访问核心类库包可见成员的情况。这也好理解，一些系统层级的类会在系统初始化的时候被加载，比如java.lang.String，如果在一个应用里面能够简单地用自定义的String类把这个系统的String类给替换掉，那将会有严重的安全问题。



#### 使用ClassLoader一些需要注意的问题

我们可以通过动态加载获得新的类，从而升级一些代码逻辑，这里有几个问题要注意一下。

如果你希望通过动态加载的方式，加载一个新版本的dex文件，使用里面的新类替换原有的旧类，从而修复原有类的BUG，那么你必须保证在加载新类的时候，旧类还没有被加载，因为如果已经加载过旧类，那么ClassLoader会一直优先使用旧类。

如果旧类总是优先于新类被加载，我们也可以使用一个与加载旧类的ClassLoader没有树的继承关系的另一个ClassLoader来加载新类，因为ClassLoader只会检查其Parent有没有加载过当前要加载的类，如果两个ClassLoader没有继承关系，那么旧类和新类都能被加载。

不过这样一来又有另一个问题了，在Java中，只有当两个实例的类名、包名以及加载其的ClassLoader都相同，才会被认为是同一种类型。上面分别加载的新类和旧类，虽然包名和类名都完全一样，但是由于加载的ClassLoader不同，所以并不是同一种类型，在实际使用中可能会出现类型不符异常。

> 同一个Class = 相同的 ClassName + PackageName + ClassLoader

以上问题在采用动态加载功能的开发中容易出现，请注意。



#### DexClassLoader 和 PathClassLoader

在Android中，ClassLoader是一个抽象类，实际开发过程中，我们一般是使用其具体的子类DexClassLoader、PathClassLoader这些类加载器来加载类的，它们的不同之处是：

- DexClassLoader可以加载jar/apk/dex，可以从SD卡中加载未安装的apk；
- PathClassLoader只能加载系统中已经安装过的apk；

#### 类加载器的初始化

平时开发的时候，使用DexClassLoader就够用了，但是我们不妨挖一下这两者具体细节上的区别。

```java
// DexClassLoader.java
public class DexClassLoader extends BaseDexClassLoader {
    public DexClassLoader(String dexPath, String optimizedDirectory,
            String libraryPath, ClassLoader parent) {
        super(dexPath, new File(optimizedDirectory), libraryPath, parent);
    }
}

// PathClassLoader.java
public class PathClassLoader extends BaseDexClassLoader {
    public PathClassLoader(String dexPath, ClassLoader parent) {
        super(dexPath, null, null, parent);
    }

    public PathClassLoader(String dexPath, String libraryPath,
            ClassLoader parent) {
        super(dexPath, null, libraryPath, parent);
    }
}
```

这两者只是简单的对BaseDexClassLoader做了一下封装，具体的实现还是在父类里。不过这里也可以看出，PathClassLoader的optimizedDirectory只能是null，进去BaseDexClassLoader看看这个参数是干什么的

```java
    public BaseDexClassLoader(String dexPath, File optimizedDirectory, String libraryPath, ClassLoader parent) {
    super(parent);
    this.originalPath = dexPath;
    this.pathList = new DexPathList(this, dexPath, libraryPath, optimizedDirectory);
}
```
这里创建了一个DexPathList实例，进去看看

```java
public DexPathList(ClassLoader definingContext, String dexPath,
            String libraryPath, File optimizedDirectory) {
        ……
        this.dexElements = makeDexElements(splitDexPath(dexPath), optimizedDirectory);
    }

    private static Element[] makeDexElements(ArrayList<File> files,
            File optimizedDirectory) {
        ArrayList<Element> elements = new ArrayList<Element>();
        for (File file : files) {
            ZipFile zip = null;
            DexFile dex = null;
            String name = file.getName();
            if (name.endsWith(DEX_SUFFIX)) {
                dex = loadDexFile(file, optimizedDirectory);
            } else if (name.endsWith(APK_SUFFIX) || name.endsWith(JAR_SUFFIX)
                    || name.endsWith(ZIP_SUFFIX)) {
                zip = new ZipFile(file);
            }
            ……
            if ((zip != null) || (dex != null)) {
                elements.add(new Element(file, zip, dex));
            }
        }
        return elements.toArray(new Element[elements.size()]);
    }

    private static DexFile loadDexFile(File file, File optimizedDirectory)
            throws IOException {
        if (optimizedDirectory == null) {
            return new DexFile(file);
        } else {
            String optimizedPath = optimizedPathFor(file, optimizedDirectory);
            return DexFile.loadDex(file.getPath(), optimizedPath, 0);
        }
    }

    /**
     * Converts a dex/jar file path and an output directory to an
     * output file path for an associated optimized dex file.
     */
    private static String optimizedPathFor(File path,
            File optimizedDirectory) {
        String fileName = path.getName();
        if (!fileName.endsWith(DEX_SUFFIX)) {
            int lastDot = fileName.lastIndexOf(".");
            if (lastDot < 0) {
                fileName += DEX_SUFFIX;
            } else {
                StringBuilder sb = new StringBuilder(lastDot + 4);
                sb.append(fileName, 0, lastDot);
                sb.append(DEX_SUFFIX);
                fileName = sb.toString();
            }
        }
        File result = new File(optimizedDirectory, fileName);
        return result.getPath();
    }
```

看到这里我们明白了，optimizedDirectory是用来缓存我们需要加载的dex文件的，并创建一个DexFile对象，如果它为null，那么会直接使用dex文件原有的路径来创建DexFile
对象。

optimizedDirectory必须是一个内部存储路径，还记得我们之前说过的，无论哪种动态加载，加载的可执行文件一定要存放在内部存储。DexClassLoader可以指定自己的optimizedDirectory，所以它可以加载外部的dex，因为这个dex会被复制到内部路径的optimizedDirectory；而PathClassLoader没有optimizedDirectory，所以它只能加载内部的dex，这些大都是存在系统中已经安装过的apk里面的。





#### 加载类的过程

上面还只是创建了类加载器的实例，其中创建了一个DexFile实例，用来保存dex文件，我们猜想这个实例就是用来加载类的。

Android中，ClassLoader用loadClass方法来加载我们需要的类

```java
 public Class<?> loadClass(String className) throws ClassNotFoundException {
        return loadClass(className, false);
    }

    protected Class<?> loadClass(String className, boolean resolve) throws ClassNotFoundException {
        Class<?> clazz = findLoadedClass(className);
        if (clazz == null) {
            ClassNotFoundException suppressed = null;
            try {
                clazz = parent.loadClass(className, false);
            } catch (ClassNotFoundException e) {
                suppressed = e;
            }

            if (clazz == null) {
                try {
                    clazz = findClass(className);
                } catch (ClassNotFoundException e) {
                    e.addSuppressed(suppressed);
                    throw e;
                }
            }
        }
        return clazz;
    }

```



loadClass方法调用了findClass方法，而BaseDexClassLoader重载了这个方法，得到BaseDexClassLoader看看

```java
@Override
    protected Class<?> findClass(String name) throws ClassNotFoundException {
        Class clazz = pathList.findClass(name);
        if (clazz == null) {
            throw new ClassNotFoundException(name);
        }
        return clazz;
    }

```

结果还是调用了DexPathList的findClass

```java
public Class findClass(String name) {
        for (Element element : dexElements) {
            DexFile dex = element.dexFile;
            if (dex != null) {
                Class clazz = dex.loadClassBinaryName(name, definingContext);
                if (clazz != null) {
                    return clazz;
                }
            }
        }
        return null;
    }
```

这里遍历了之前所有的DexFile实例，其实也就是遍历了所有加载过的dex文件，再调用loadClassBinaryName方法一个个尝试能不能加载想要的类，真是简单粗暴

```java
public Class loadClassBinaryName(String name, ClassLoader loader) {
        return defineClass(name, loader, mCookie);
    }
    private native static Class defineClass(String name, ClassLoader loader, int cookie);
```

看到这里想必大家都明白了，loadClassBinaryName中调用了Native方法defineClass加载类。

至此，ClassLoader的创建和加载类的过程的完成了。有趣的是，标准JVM中，ClassLoader是用defineClass加载类的，而Android中defineClass被弃用了，改用了loadClass方法，而且加载类的过程也挪到了DexFile中，在DexFile中加载类的具体方法也叫defineClass，不知道是Google故意写成这样的还是巧合。

#### 自定义ClassLoader

平时进行动态加载开发的时候，使用DexClassLoader就够了。但我们也可以创建自己的类去继承ClassLoader，需要注意的是loadClass方法并不是final类型的，所以我们可以重载loadClass方法并改写类的加载逻辑。

通过前面我们分析知道，ClassLoader双亲代理的实现很大一部分就是在loadClass方法里，我们可以通过重写loadClass方法避开双亲代理的框架，这样一来就可以在重新加载已经加载过的类，也可以在加载类的时候注入一些代码。这是一种Hack的开发方式，采用这种开发方式的程序稳定性可能比较差，但是却可以实现一些“黑科技”的功能。



#### Android程序比起一般Java程序在使用动态加载时麻烦在哪里

通过上面的分析，我们知道使用ClassLoader动态加载一个外部的类是非常容易的事情，所以很容易就能实现动态加载新的可执行代码的功能，但是比起一般的Java程序，在Android程序中使用动态加载主要有两个麻烦的问题：

1. Android中许多组件类（如Activity、Service等）是需要在Manifest文件里面注册后才能工作的（系统会检查该组件有没有注册），所以即使动态加载了一个新的组件类进来，没有注册的话还是无法工作；
2. Res资源是Android开发中经常用到的，而Android是把这些资源用对应的R.id注册好，运行时通过这些ID从Resource实例中获取对应的资源。如果是运行时动态加载进来的新类，那类里面用到R.id的地方将会抛出找不到资源或者用错资源的异常，因为新类的资源ID根本和现有的Resource实例中保存的资源ID对不上；

说到底，抛开虚拟机的差别不说，**一个Android程序和标准的Java程序最大的区别就在于他们的上下文环境（Context）不同**。Android中，这个环境可以给程序提供组件需要用到的功能，也可以提供一些主题、Res等资源，其实上面说到的两个问题都可以统一说是这个环境的问题，而现在的各种Android动态加载框架中，核心要解决的东西也正是“如何给外部的新类提供上下文环境”的问题。

参考：

[Android动态加载基础 ClassLoader工作机制](https://segmentfault.com/a/1190000004062880)


在Android中，我们通过`ClassLoader`来加载应用程序运行需要的类。`ClassLoader`是一个抽象类，我们需要继承该类来实现具体的类加载器的行为。在Android中，`ClassLoader`的实现类采用了`代理模型(Delegation Model)`来执行类的加载。每一个`ClassLoader`类都有一个与之相关联的父加载器，当一个`ClassLoader`类尝试加载某个类时，首先会委托其父加载器加载该类。如果父加载器成功加载了该类，则不会再由该子加载器进行加载；如果父加载器未能加载成功，则再由子加载器进行类加载的动作。

在Android中，我们一般使用`DexClassLoader`和`PathClassLoader`进行类的加载。

- `DexClassLoader`: 可以从.jar或者.apk文件中加载类；
- `PathClassLoader`: 只能从系统内存中已安装的内容中加载类。

对于我们的插件化APK，显然需要使用`DexClassLoader`进行自定义类加载。我们看一下`DexClassLoader`的构造方法：

```
/**
 * Create DexClassLoader
 * @param dexPath String: the list of jar/apk files containing classes and resources, delimited by File.pathSeparator, which defaults to ":" on Android
 * @param optimizedDirectory String: directory where optimized dex files should be written; must not be null
 * @param librarySearchPath String: the list of directories containing native libraries, delimited by File.pathSeparator; may be null
 * @param parent ClassLoader: the parent class loader
 */
DexClassLoader (String dexPath, 
                String optimizedDirectory, 
                String librarySearchPath, 
                ClassLoader parent)
```

从以上可以看到，该构造方法的入参中除了指定各种加载路径外，还需要指定一个父加载器，以此实现我们以上提到的类加载代理模型。

### 步骤规划

为了让整个coding过程变得简单，我们来实现一个简单得不能再简单的功能：在主Activity上以"年-月-日"的格式显示当前的日期。为了让插件APK的整个思路清晰一点，我们想要实现如下设定：

- 提供一个插件化APK，提供一个生成日期的方法；
- 应用程序主Activity中通过插件APK中的方法获取到该日期，显示在TextView中。

有了如上的铺垫，我们现在可以明确我们的实现步骤：

- 创建我们的Application；
- 创建一个共享接口的library module；

由于我们将一部分方法放到了插件APK里，这也就意味着，我们在自己的`app module`中对这些方法是不可见的，这就需要有一个机制让`app module`中使用这些方法变成可能。

在这里，我们采用一个公共的接口来进行方法的定义。你可以理解为我们在`app`和`插件APK`之间搭了一座桥，我们在`app module`中使用接口定义的这些方法，而方法的具体实现放在了`插件APK`中。

我们创建一个`library module`，命名为`library`。在该`library module`中，我们创建一个`TestInterface`接口，在该接口中定义相关方法。

为了让`插件APK`引用该library定义的接口，我们需要生成一个jar包，首先，在`library module`的`gradle`脚本中增加如下配置：

```
android.libraryVariants.all { variant ->
    def name = variant.buildType.name
    if (name.equals(com.android.builder.core.BuilderConstants.DEBUG)) {
        return; // Skip debug builds.
    }
    def task = project.tasks.create "jar${name.capitalize()}", Jar
    task.dependsOn variant.javaCompile
    task.from variant.javaCompile.destinationDir
    artifacts.add('archives', task);
}
```

在工程根目录执行如下命令：

```
./gradlew :library:jarRelease
```

- 生成插件APK；

在工程中创建一个module，类型选择为application（而不是library），取名为`plugin`。

将上一步中生成的`library.jar`放到该plugin module的`libs`目录下，在gradle脚本中添加

```
provided files('libs/library.jar')
```

便可以引用library中定义的共享接口了。

正如如上所说，我们在该plugin module中做方法的具体实现，因此，我们创建一个`TestUtil`类，实现如上定义的`TestInterface`接口定义的方法



接下来，我们需要生成一个`插件APK`，将该APK放在应用程序app module的SourceSet下，供app module的类加载器进行加载。为此，我们在plugin的gradle脚本中添加如下配置：

```
buildTypes {
        release {
            minifyEnabled false
            proguardFiles getDefaultProguardFile('proguard-android.txt'), 'proguard-rules.pro'

            applicationVariants.all { variant ->
                variant.outputs.each { output ->
                    def apkName = "plugin.apk"
                    output.outputFile = file("$rootProject.projectDir/app/src/main/assets/plugin/" + apkName)
                }
            }
        }
    }
```

该脚本将生成的apk放在app的assets目录下。

最后，在工程根目录执行：

```
./gradlew :plugin:assembleRelease
```

便可以在`/app/src/main/assets/plugin`目录下生成了一个plugin.apk文件。到此为止，我们便生成了我们的`插件APK`。



- 实现自定义类加载器；
  - 将该APK复制到SD卡中；
  - 从SD卡中加载该APK。


- 实现动态加载。

### 源码

该示例工程的源代码我放到了自己的GitHub上：
[Github/Anchorer/PluginApk](https://github.com/Anchorer/PluginApk)

这个工程对代码进行了一定程度的封装：

- `PluginManager`: 该类统一提供了创建类加载器和加载具体类的所有入口；

- `PluginLoader`: 该类具体创建了类加载器，执行具体的加载类的行为；

- `MainActivity`: 主Activity，展示了如何调用插件内的方法。

  ​

上述方式只是能加载不存在的类，让APK增加本来不存在的方法。

但Android中组件如Activity、Service等是需要在AndroidManifest中注册了才可以用。



Activity等组件是需要在Manifest中注册后才能以标准Intent的方式启动的（如果有兴趣强烈推荐你了解下Activity生命周期实现的机制及源码），通过ClassLoader加载并实例化的Activity实例只是一个普通的Java对象，能调用对象的方法，但是它没有生命周期，而且Activity等系统组件是需要Android的上下文环境的（Context等资源），没有这些东西Activity根本无法工作。

使用插件APK里的Activity需要解决**两个问题**：

1. 如何使插件APK里的Activity具有生命周期；
2. 如何使插件APK里的Activity具有上下文环境（使用R资源）；

代理Activity模式为解决这两个问题提供了一种思路。



##### 代理Activity模式

这种模式也是我们项目中，继“简单动态加载模式”之后，第二种投入实际生产项目的开发方式。

其主要特点是：主项目APK注册一个代理Activity（命名为ProxyActivity），ProxyActivity是一个普通的Activity，但只是一个空壳，自身并没有什么业务逻辑。每次打开插件APK里的某一个Activity的时候，都是在主项目里使用标准的方式启动ProxyActivity，再在ProxyActivity的生命周期里同步调用插件中的Activity实例的生命周期方法，从而执行插件APK的业务逻辑。

> ProxyActivity + 没注册的Activity = 标准的Activity



### 处理插件Activity的生命周期

目前还真的没什么办法能够处理这个问题，一个Activity的启动，如果不采用标准的Intent方式，没有经历过Android系统Framework层级的一系列初始化和注册过程，它的生命周期方法是不会被系统调用的（除非你能够修改Android系统的一些代码，而这已经是另一个领域的话题了，这里不展开）。

那把插件APK里所有Activity都注册到主项目的Manifest里，再以标准Intent方式启动。但是事先主项目并不知道插件Activity里会新增哪些Activity，如果每次有新加的Activity都需要升级主项目的版本，那不是本末倒置了，不如把插件的逻辑直接写到主项目里来得方便。

那就绕绕弯吧，生命周期不就是系统对Activity一些特定方法的调用嘛，那我们可以在主项目里创建一个ProxyActivity，再由它去代理调用插件Activity的生命周期方法（这也是代理模式叫法的由来）。用ProxyActivity（一个标准的Activity实例）的生命周期同步控制插件Activity（普通类的实例）的生命周期，同步的方式可以有下面两种：

- 在ProxyActivity生命周期里用反射调用插件Activity相应生命周期的方法，简单粗暴。
- 把插件Activity的生命周期抽象成接口，在ProxyActivity的生命周期里调用。另外，多了这一层接口，也方便主项目控制插件Activity。

这里补充说明下，Fragment自带生命周期，用Fragment来代替Activity开发可以省去大部分生命周期的控制工作，但是会使得界面跳转比较麻烦，而且Honeycomb以前没有Fragment，无法在API11以前的系统使用。



### 在插件Activity里使用R资源

使用代理的方式同步调用生命周期的做法容易理解，也没什么问题，但是要使用插件里面的res资源就有点麻烦了。简单的说，res里的每一个资源都会在R.java里生成一个对应的Integer类型的id，APP启动时会先把R.java注册到当前的上下文环境，我们在代码里以R文件的方式使用资源时正是通过使用这些id访问res资源，然而插件的R.java并没有注册到当前的上下文环境，所以插件的res资源也就无法通过id使用了。

这个问题困扰了我们很久，一开始的项目急于投入生产，所以我们索性抛开res资源，插件里需要用到的新资源都通过纯Java代码的方式创建（包括XML布局、动画、点九图等），蛋疼但有效。知道网上出现了解决这一个问题的有效方法（一开始貌似是在手机QQ项目中出现的，但是没有开源所以不清楚，在这里真的佩服这些对技术这么有追求的开发者）。

记得我们平时怎么使用res资源的吗，就是“getResources().getXXX(resid)”，看看“getResources()”



```java
@Override
    public Resources getResources() {
        if (mResources != null) {
            return mResources;
        }
        if (mOverrideConfiguration == null) {
            mResources = super.getResources();
            return mResources;
        } else {
            Context resc = createConfigurationContext(mOverrideConfiguration);
            mResources = resc.getResources();
            return mResources;
        }
    }
```



看起来像是通过mResources实例获取res资源的，在找找mResources实例是怎么初始化的，看看上面的代码发现是使用了super类ContextThemeWrapper里的“getResources()”方法，看进去

```java
Context mBase;
    public ContextWrapper(Context base) {
        mBase = base;
    }
@Override
    public Resources getResources()
    {
        return mBase.getResources();
    }
```

看样子又调用了Context的“getResources()”方法，看到这里，我们知道Context只是个抽象类，其实际工作都是在ContextImpl完成的，赶紧去ContextImpl里看看“getResources()”方法吧

```java
@Override
    public Resources getResources() {
        return mResources;
    }
```



还是没有mResources的创建过程啊！啊，不对，mResources是ContextImpl的成员变量，可能是在构造方法中创建的，赶紧去看看构造方法（这里只给出关键代码）。

```java
resources = mResourcesManager.getTopLevelResources(packageInfo.getResDir(),
                        packageInfo.getSplitResDirs(), packageInfo.getOverlayDirs(),
                        packageInfo.getApplicationInfo().sharedLibraryFiles, displayId,
                        overrideConfiguration, compatInfo);
mResources = resources;
```

看样子是在ResourcesManager的“getTopLevelResources”方法中创建的，看进去

```java
Resources getTopLevelResources(String resDir, String[] splitResDirs,
            String[] overlayDirs, String[] libDirs, int displayId,
            Configuration overrideConfiguration, CompatibilityInfo compatInfo) {
        Resources r;
        AssetManager assets = new AssetManager();
        if (libDirs != null) {
            for (String libDir : libDirs) {
                if (libDir.endsWith(".apk")) {
                    if (assets.addAssetPath(libDir) == 0) {
                        Log.w(TAG, "Asset path '" + libDir +
                                "' does not exist or contains no resources.");
                    }
                }
            }
        }
        DisplayMetrics dm = getDisplayMetricsLocked(displayId);
        Configuration config ……;
        r = new Resources(assets, dm, config, compatInfo);
        return r;
    }
```

看来这里是关键了，看样子就是通过这些代码从一个APK文件加载res资源并创建Resources实例，经过这些逻辑后就可以使用R文件访问资源了。具体过程是，获取一个AssetManager实例，使用其“addAssetPath”方法加载APK（里的资源），再使用DisplayMetrics、Configuration、CompatibilityInfo实例一起创建我们想要的Resources实例。

最终访问插件APK里res资源的关键代码如下

```java
 try {  
        AssetManager assetManager = AssetManager.class.newInstance();  
        Method addAssetPath = assetManager.getClass().getMethod("addAssetPath", String.class);  
        addAssetPath.invoke(assetManager, mDexPath);  
        mAssetManager = assetManager;  
    } catch (Exception e) {  
        e.printStackTrace();  
    }  
    Resources superRes = super.getResources();  
    mResources = new Resources(mAssetManager, superRes.getDisplayMetrics(),  
            superRes.getConfiguration());
```

注意，有的人担心从插件APK加载进来的res资源的ID可能与主项目里现有的资源ID冲突，其实这种方式加载进来的res资源并不是融入到主项目里面来，主项目里的res资源是保存在ContextImpl里面的Resources实例，整个项目共有，而新加进来的res资源是保存在新创建的Resources实例的，也就是说ProxyActivity其实有两套res资源，并不是把新的res资源和原有的res资源合并了（所以不怕R.id重复），对两个res资源的访问都需要用对应的Resources实例，这也是开发时要处理的问题。（其实应该有3套，Android系统会加载一套framework-res.apk资源，里面存放系统默认Theme等资源）

额外补充下，这里你可能注意到了我们采用了反射的方法调用AssetManager的“addAssetPath”方法，而在上面ResourcesManager中调用AssetManager的“addAssetPath”方法是直接调用的，不用反射啊，而且看看SDK里AssetManager的“addAssetPath”方法的源码（这里也能看到具体APK资源的提取过程是在Native里完成的），发现它也是public类型的，外部可以直接调用，为什么还要用反射呢？



```java
 /**
     * Add an additional set of assets to the asset manager.  This can be
     * either a directory or ZIP file.  Not for use by applications.  Returns
     * the cookie of the added asset, or 0 on failure.
     * {@hide}
     */
    public final int addAssetPath(String path) {
        synchronized (this) {
            int res = addAssetPathNative(path);
            makeStringBlocks(mStringBlocks);
            return res;
        }
    }
```



这里有个误区，SDK的源码只是给我们参考用的，APP实际上运行的代码逻辑在android.jar里面（位于android-sdk\platforms\android-XX），反编译android.jar并找到ResourcesManager类就可以发现这些接口都是对应用层隐藏的。

到此，启动插件里的Activity的两大问题都有解决的方案了。

##### [Android动态加载进阶 代理Activity模式](https://segmentfault.com/a/1190000004062972)

##### [Android动态加载补充 加载SD卡中的SO库](https://segmentfault.com/a/1190000004062899)

##### 代理模式的具体项目 [dynamic-load-apk](https://github.com/singwhatiwanna/dynamic-load-apk)

##### 

##Advice

##Other

##参考
[DL动态加载框架技术文档 任玉刚](http://blog.csdn.net/singwhatiwanna/article/details/40283117)

[Android动态加载插件APK](https://segmentfault.com/a/1190000006132827)

[包建强，插件化从入门到放弃](http://mp.weixin.qq.com/s?__biz=MjM5MDE0Mjc4MA==&mid=2650993300&idx=1&sn=797fa87ef528cff3a50e77806cf9f675&scene=1&srcid=07124TeQfqvSzge8vCmJ66Oi#wechat_redirect)
[专访包建强：为什么我说Android插件化从入门到放弃？](http://www.infoq.com/cn/news/2016/04/baojianqiang-interview)

[Android插件化原理解析——Hook机制之动态代理](http://weishu.me/2016/01/28/understand-plugin-framework-proxy-hook/)

[Android博客周刊专题之＃插件化开发＃](http://www.androidblog.cn/index.php/Index/detail/id/16#)

[Android 使用动态加载框架DL进行插件化开发](http://blog.csdn.net/t12x3456/article/details/39958755)

[Android 插件化的 过去 现在 未来](http://kymjs.com/code/2016/05/04/01)

[Android 插件化 动态升级](http://www.trinea.cn/android/android-plugin/)

[插件化 从放弃到捡起](http://kymjs.com/column/plugin.html)

360的加载框架的地址: [https://github.com/Qihoo360/DroidPlugin](https://github.com/Qihoo360/DroidPlugin)

另外一个阿里同学在开发中的插件框架：[https://github.com/limpoxe/Android-Plugin-Framework](https://github.com/limpoxe/Android-Plugin-Framework) 

整理了下以前star的热修复 插件 和 换肤的东东 

[https://github.com/Qihoo360/DroidPlugin](https://github.com/Qihoo360/DroidPlugin) 

[https://github.com/limpoxe/Android-Plugin-Framework](https://github.com/limpoxe/Android-Plugin-Framework)
[https://github.com/houkx/android-pluginmgr](https://github.com/houkx/android-pluginmgr)
[https://github.com/bunnyblue/DroidFix](https://github.com/bunnyblue/DroidFix)
[https://github.com/dodola/HotFix](https://github.com/dodola/HotFix)
[https://github.com/jasonross/Nuwa](https://github.com/jasonross/Nuwa)
[https://github.com/alibaba/AndFix](https://github.com/alibaba/AndFix)
[https://github.com/hongyangAndroid/AndroidChangeSkin](https://github.com/hongyangAndroid/AndroidChangeSkin)
[https://github.com/fengjundev/Android-Skin-Loader](https://github.com/fengjundev/Android-Skin-Loader)



##### [Android动态加载技术 系列索引](https://segmentfault.com/a/1190000004086213)

##### [Android动态加载技术 简单易懂的介绍方式](https://segmentfault.com/a/1190000004062866)

[插件化框架和热修复技术知识汇总](https://www.figotan.org/2016/08/12/android-plugin-and-hotfix-collections/?hmsr=toutiao.io&utm_medium=toutiao.io&utm_source=toutiao.io)



## 巨人的肩膀

>任玉刚

> 张勇

> 包建强
> ​	《App研发录》一书作者。同时著有《2015年无线技术白皮书》，发表于2016年《程序员》杂志。擅长iOS和Android，对Android插件化、iOS热修复等技术多有涉及。目前从事区块链技术研究工作。

> [张涛](http://kymjs.com/)

> Lody




# I, Programmer.
##Day 1 Android插件化

#### 解决问题：

1、不用重新安装apk即可扩展新功能，动态添加功能；

2、减少apk Size；

#### 技术难点：

	1、一个没有被安装的apk包无法被运行；

	2、apk包的资源无法被引用；

	3、apk包加载后无法跟用户进行交互；

#### 解决方案：

	1、Android中有一个dexClassLoad类加载器，可以通过反射加载一个类来运行；

	2、
		a.通过将插件资源放到sdcard上通过流的方式读取
			缺点:对流的读取会有问题，通配性差。
		b.将插件apk复制一份到当前app内进行加载；
			缺点：每个插件都要复制一份，久而久之，对空间占用高。
	
	3、在github上有一个叫AndroidDynamicLoader的项目，是通过用Fragment做为插件
	的表现形式，由于Fragment特殊性（既可以处理逻辑交互又具备与Activity相同的生命周期）
	可是Fragment限制性太大了，太过碎片化使得使用起来复杂性过高。

#### 最终实现方案：
	通过代理/委托模式设计的Application类去动态的改变一个apk所在的环境，实现动态加载的目的。
	抱着这种思路，我曾想自己去设计一个application类，但是技术有限，太复杂了，于是结合AndroidDynamicLoader的思路与360的思路，
	我自己设计了一个Activity去代理插件的Activity，于是就有了CJFrameForAndroid.

#### 原理描述
首先解释几个名词： APP项目：指要调用插件apk的那个已经安装到用户手机上的应用。
插件项目：指没有被安装且希望借助已经安装到手机上的项目运行的apk。 
插件化：Activity继承自CJActivity，且与APP项目jar包冲突已经解决的插件项目称为已经被插件化。 
Activity事务：在CJFrameForAndroid中，一个Activity的生命周期以及交互事件统称为Activity的事务。 
托管所：指插件中的一个委派/代理Activity，通过这个Activity去处理插件中Activity的全部事务，从而表现为就像插件中的Activity在运行一样。

CJFrameForAndroid的实现原理是通过类加载器，动态加载存在于SD卡上的apk包中的Activity。通过使用一个托管所，插件Activity全部事务(包括声明周期与交互事件)将交由托管所来处理，间接实现插件的运行。
一句话描述：CJFrameForAndroid中的托管所，复制了插件中的Activity，来替代插件中的Activity与用户交互。


### 热修复方案

基于技术解决方案可分为两类：native hook和ClassLoader hook。

[AndFix](https://www.diycode.cc/projects/alibaba/AndFix)
AndFix作为native hook的代表，它的实现优美，使用也比较简洁基于修改的类生产对应的类，非常巧妙，生产的patch包体积也很小。但是由于是对dalivk & ART进行hook，由于hook的native API中可能会经过ROM厂商的修改以及Android版本迭代过程中API的变更，都给兼容性带来了挑战。
比如发布的Android N是不能支持的，需要继续做适配工作

饿了么APP当初采用了AndFix作为第一版的技术方案，在三星和华为等的机器上出现很多Dex格式加载相关的问题

[Tinker](https://www.diycode.cc/projects/Tencent/tinker)

[QZone](https://zhuanlan.zhihu.com/p/20308548)
QQZone作为ClassLoader hook的方案代码，需要对code进行插桩，只可修复Java code，关于资源的加载以及so的修复是不能支持的。后来Tinker把这块补了上来，同时对差分体积做了更深的挖掘，但是相对在打差分包上就有很多的限制，要保留class混淆和res ids的映射文件，比较麻烦。

[Amigo](https://www.diycode.cc/projects/eleme/Amigo)
Amigo 原理与 QQZone 的方案有些类似，QQZone,Tinker,Nuwa这类方案是通过修改PathClassLoader中的dex实现的，Amigo则是釜底抽薪直接替换ClassLoader。同时进一步实现了 so 文件、资源文件、四大组件的修复，可以对APP全面进行修复，不愧 Amigo（朋友）这个称号，能在危急时刻送来全面的帮助。同时Amigo也致力解决使用过程中的带来的束缚，比如对代码进行插桩、打包时保存和指定映射文件等(如果mapping文件丢了后果不堪设想)，所以开发和打包完全无侵入。


[Aceso](https://www.diycode.cc/projects/meili/Aceso)

[Robust]()

对于Android下的冷启动修复，最早的实现方案是QQ空间提出的dex插入方式。该方案的主要思想，就是把插入新dex插入到ClassLoader索引路径的最前面。这样在load一个class时，就会优先找到补丁中的。后来微信的Tinker和手Q的QFix都基于该方案做了改进，而这类插入dex的方案，都会遇到一个主要的问题，就是如何解决Dalvik虚拟机下Pre-verify Class的问题。

如果一个方法中直接引用到的类和该方法所属类都在同一个dex中的话，那么这个方法的所属类就会被打上CLASS_ISPREVERIFIED，具体判定代码可见虚拟机中的verifyAndOptimizeClass函数。

我们先来看看腾讯的三大热修复方案是如何解决这个问题的:

QQ空间的处理方式，是在每个类中插入一个来自其他dex的hack.class，由此让所有类里面都无法满足pre-verified条件。

Tinker的方式，是合成全量的dex文件，这样所有class的都在全量dex中解决，从而消除class重复而带来的冲突。

QFix的方式，是取得虚拟机中的某些底层函数，提前resolve所有补丁类。以此绕过Pre-verify检查。

以上的三种方案里面，QQ空间方案会侵入打包流程，并且为了hack添加一些无用的信息，实现起来很不优雅。而我们一开始采用的QFix的方案，需要获取底层虚拟机的函数，不够稳定可靠，并且有个比较大的问题是无法新增public函数。

现在看来比较好的方式，就是像Tinker那样全量合成完整新dex。他们的合成方案，是从dex的方法和指令维度进行全量合成，虽然可以很大地节省空间，但由于对dex内容的比较粒度过细，实现较为复杂，性能消耗比较严重。实际上，dex的大小占整个apk的比例是比较低的，一个app里面的dex文件大小并不是主要部分，而占空间大的主要还是apk中的资源文件。因此，Tinker方案的时空代价转换的性价比不高。

[阿里Hotfix 2.x](https://mp.weixin.qq.com/s/KyDg09MpWpP7NbGXuFTlvw)

[热修复三大流派](https://baichuan.bbs.taobao.com/detail.html?spm=5176.100239.blogcont65319.11.0Ukaxl&postId=6571722)

[Android热修复升级探索](https://www.atatech.org/articles/72533)

摘自：

[从放弃到捡起 Android插件化开发框架CJFrameForAndroid](http://kymjs.com/column/plugin.html)

[DroidPlugin360](https://github.com/Qihoo360/DroidPlugin)

[Android 插件化之AssetManager](http://www.atatech.org/articles/56034)

[动态加载](https://juejin.im/entry/566109fd00b01b7852c1cc9d)

[Android动态加载](http://www.jianshu.com/p/e9da13f647a9)

[Android动态加载技术 简单易懂的介绍方式](https://segmentfault.com/a/1190000004062866)

[MultiDex工作原理分析和优化方案](https://zhuanlan.zhihu.com/p/24305296)

[ Android中插件开发篇之----类加载器](http://blog.csdn.net/jiangwei0910410003/article/details/41384667)

[怎么将 Android 程序做成插件化的形式？](https://www.zhihu.com/question/19981105)

[Android动态加载jar/dex](http://www.cnblogs.com/over140/archive/2011/11/23/2259367.html)

[Android热更新方案Robust开源，新增自动化补丁工具](http://tech.meituan.com/android_autopatch.html)

[Android 热修复，没你想的那么难](https://kymjs.com/code/2016/05/08/01/)

[热修复入门：Android 中的 ClassLoader](http://jaeger.itscoder.com/android/2016/08/27/android-classloader.html)


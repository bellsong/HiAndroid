# Check List
Android的java运行环境，在4.4以下用的是dalvik虚拟机，而在4.4以上用的是art虚拟机。

dalvik虚拟机和art虚拟机的区别

### What is Dalvik
Dalvik是Google公司自己设计用于Android平台的虚拟机。
Dalvik虚拟机是Google等厂商合作开发的Android移动设备平台的核心组成部分之一。
它可以支持已转换为 .dex格式的Java应用程序的运行，.dex格式是专为Dalvik设计的一种压缩格式，适合内存和处理器速度有限的系统。
Dalvik 经过优化，允许在有限的内存中同时运行多个虚拟机的实例，并且每一个Dalvik 应用作为一个独立的Linux 进程执行。独立的进程可以防止在虚拟机崩溃的时候所有程序都被关闭。


### What is ART
即Android Runtime
ART 的机制与 Dalvik 不同。在Dalvik下，应用每次运行的时候，字节码都需要通过即时编译器（just in time ，JIT）转换为机器码，这会拖慢应用的运行效率，而在ART 环境中，应用在第一次安装的时候，字节码就会预先编译成机器码，使其成为真正的本地应用。这个过程叫做预编译（AOT,Ahead-Of-Time）。这样的话，应用的启动(首次)和执行都会变得更加快速。

### ART有什么优缺点呢？
优点：
1、系统性能的显著提升。
2、应用启动更快、运行更快、体验更流畅、触感反馈更及时。
3、更长的电池续航能力。
4、支持更低的硬件。
缺点：
1.机器码占用的存储空间更大，字节码变为机器码之后，可能会增加10%-20%（不过在应用包中，可执行的代码常常只是一部分。比如最新的 Google+ APK 是 28.3 MB，但是代码只有 6.9 MB。）
2.应用的安装时间会变长。

在Android开发的过程中，有哪些坑是值得你放在checklist中警示自己的？

通常我们会将经常遇到的问题、容易犯错的地方都放在一个checklist中，说说在Android开发过程中有哪些问题值得你放在自己的checklist中。

举例说明：

检查项：是否调用了类的finalize方法作为释放与类相关的资源？

可能存在问题：类的finalize方法的调用时机由系统而定，不能准确知道调用时机，可能会导致程序在使用过程中的异常。

解决方案：将与类释放资源相关的操作封装成一个共有方法供外部使用，而不是调用类的finalize方法释放与类相关的资源。

1、TextView（往往 TextView 派生子类同样适用）调用 setText 方法设置一个 int 型的数据，千万要将该值转为 String，否则在某些设备中它会默认去查询 R 文件中定义的资源。

2、使用友盟分享 SDK，需要执行分享的 Activity 请不要为该 Activity 设置android:process
属性。比如你的 App 运行在 com.codingfish.test 进程，需要产生分享动作的Activity 设置 android:proces=":com.codingfish.hello" ，那么新浪微博就会出现你设置的分享内容没有显示的问题。该 Bug 已经提交给友盟的技术人员，但是 N 久没有得到修复。

3、上线之前一定要使用正式签名打包。某朋友公司 Android 的应用上架之前，负责打包上线的童鞋（新人，老人已离职，只有这一个Android）没有签名的概念，直接将 Debug 签名的 Apk 投放到渠道了，到现在还有一批设备没有替换回来。

4、在 Activity 中尽可能少的创建 Handler 对象，创建一个主线程 Handler，一个后台 HandlerThread 就可以了。

5、使用 BitMap 之后，一定要记得释放。

6、使用线程的地方尽量不要 new Thread，而是使用 AsyncThread 。

7、onCreate(Bundle savedInstanceState)
 切记将super.onCreate(savedInstanceState);
 放在一切业务的前面。

8、创建了四大组件一定记得要在 AndroidManifest 文件中声明（当然 BroadcastReceiver 可以动态注册）。

1. 为Activity声明系统配置变更事件  系统配置变更事件是指转屏，区域语言发生变化，屏幕尺寸发生变化等等，如果Activity没有声明处理这些事件，发生事件时，系统会把Activity杀掉然后重启，并尝试恢复状态，Activity有机会通过onSaveInstanceState()保存一些基本数据到Bundle中，然后此Bundle会在Activity的onCreate()中传递过去。虽然这貌似正常，但是这会引发问题，因为很多其他的东西比如Dialog等是要依赖于具体Activity实例的。所以这种系统默认行为通常都不是我们想要的。为了避免这些系统默认行为，就需要为Activity声明这些配置，如下二个是每个Activity必须声明的：<activity android:configChanges="orientation|keyboardHidden">几乎所有的Activity都要声明如上，为什么Android不把它们变成Default的呢?

2. 尽量使用Android的API 这好像是废话，在Android上面开发不用Android API用什么？因为Android几乎支持Java SE所有的API，所以有很多地方Android API与Java SE的API会有重复的地方，比如说对于文件的操作最好使用Android里面Context封装的API，而不要直接使用File对象：Context.openFileOutput(String); // no File file = new File(String)原因就是API里面会考虑到Android平台本身的特性；再如，少用Thread，而多使用AsyncTask等。

3. 要考虑到Activity和进程被杀掉的情况  如了通常情况退出Activity外，还有Activity因其他原因被杀的情况，比如系统内存过低，系统配置变更，有异常等等，要考虑和测试这种情况，特别是Activity处理重要的数据时，做好的数据的保存。

4. 小心多语言  有些语言真的很啰嗦，中文或英文很简短就能表达的事情到了其他语言就变的死长死长的，所以如果是wrap_content就可能把其他控制挤出可视范围； 如果是指定长度就可能显示不全。也要注意特殊语言比如那些从右向左读的语言。

5. 不要用四大组件去实现接口  一是组件的对象都比较大，实现接口比较浪费，而且让代码更不易读和理解； 另外更重要的是导致多方引用，可能会引发内存泄露。

6. 用getApplication()来取Context当参数 对于需要使用Context对象作为参数的函数，要使用getApplication()获取Context对象当参数，而不要使用this，除非你需要特定的组件实例！getApplication()返回的Context是属于Application的，它会在整个应用的生命周期内存在，远大于某个组件的生命周期，所以即使某个引用长期持有Context对象也不会引发内存泄露。
8、主线程只做UI控制和Frameworks回调相关的事。附属线程只做费时的后台操作。交互只通过Handler。这样就可以避免大量的线程问题。
9、Frameworks的回调不要做太多事情仅做必要的初始化，其他不是很重要的事情可以放到其他线程中去做，或者用Handler Schedule到稍后再做。
10、 要考虑多分辨率  至少为hdpi, mdpi, ldpi准备图片和布局。元素的单位也尽可能的使用dip而不要用px。
11、利用Android手机的硬键  几乎所有的Android手机都有BACK和MENU，它们的作用是返回和弹出菜单，所以就不要再在UI中设计返回按扭和菜单按扭。很多优秀的应用如随手记和微信都有返回键，他们之所以有是因为他们都是从iOS上移植过来的，为了保存体验的一致，所以也有了返回和菜单。但这不够Android化，一个纯正的Android是没有必须重复硬键的功能的。


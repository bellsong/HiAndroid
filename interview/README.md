# Android 面试大全

Android面试相关的总结，包含两部分  

## Android 部分 

### Android基础部分 
1. Handler原理

一个 Activity 绑定 Service 后，在 startService ，然后在 stopService，此时 Service 是否还需要解绑

Webview优化 提前初始化 缓存js 内核做什么，外壳可以做什么，webview内存优化

Binder的大体设计模式

图片压缩不失真的情况下能压缩多少倍？

Handler为什么会发生内存泄漏

2. 
### Android源码分析部分 

## Java部分 

### JAVA基础
1. Java程序运行机制
2. HashMap

    hashmap和hashtable区别  HashMap源码分析  HashMap底层结构 HashMap查找效率 HashMap 的实现过程 HashMap 的实现原理

3. 设计一个线程安全的HashMap
4. 为什么产生死锁

    死锁： 是指在一组进程中的各个进程均占有不会释放的资源，但因互相申请被其他进程所站用不会释放的资源而处于的一种永久等待状态
5. jvm类加载
6. java反射获取私有属性，改变值 反射用途
7. synchronized volatile关键字的用法

    volatile 的作用，为什么会出现变量读取不一致的情况， synchronized 的区别

8. ArrayList的底层实现

    ArrayList 如何删除重复的元素或者指定的元素

12. Java 中内部类为什么可以访问外部类

13. Java内存模块分区和GC机制，GC算法有哪些

### 网络部分

9. 浏览器输入地址到返回结果发生了什么
    
    发起请求 DNS 三次握手 建立连接 数据传输 断开连接

10. TCP

    TCP 三次握手过程  TCP是如何保证可靠性传输的

11. 如何设计在 UDP 上层保证 UDP 的可靠性传输

### 多线程部分 

1. 多核里面进程和线程的表现

1. 线程如何实现

1. Android中线程能开多少个？

1. synchronized 和volatile 关键字的区别？

synchronized 和volatile的作用：
1）保证了不同线程对这个变量进行操作时的可见性，即一个线程修改了某个变量的值，这新值对其他线程来说是立即可见的。
2）禁止进行指令重排序。
volatile 本质是在告诉jvm 当前变量在寄存器（工作内存）中的值是不确定的，需要从主存中读取；synchronized 则是锁定当前变量，只有当前线程可以访问该变量，其他线程被阻塞住。
区别：
1）volatile 仅能使用在变量级别；synchronized 则可以使用在变量、方法、和类级别的
2）volatile 仅能实现变量的修改可见性，并不能保证原子性；synchronized 则可以保证变量的修改可见性和原子性
3）volatile 不会造成线程的阻塞；synchronized 可能会造成线程的阻塞。
4）volatile 标记的变量不会被编译器优化；synchronized 标记的变量可以被编译器优化

### 工程

APK 包含了哪些东西，打包过程是什么；

断点上传如何设计

Android 音频控件如何使用，底层原理

### 设计模式

1. 手写单例模式

    饿汉式单例和懒汉式单例的区别


## 算法部分
### 基础排序算法
1. 冒泡排序
2. 快速排序 

外排序有哪些，与内部排序的区别

### 使用案例

设计移动端的联系人存储与查询的功能，要求快速搜索联系人，可以用到哪些数据结构？（二叉排序树，建立索引）

算法：单链表输出倒数第 k 个元素，冒泡排

100 万个数据找出 100 个最大的 
小顶堆

数组插入

单链表，O（1）复杂度内删除一个结点，写算法

冒泡排序的链表实现

手写堆排序

连续子序列问题，保证 i < j, Ai < Aj 的算法思想

最短路径的算法思想


字符串中查找子字符串一般用什么算法

密码学中两大加密算法是什么

排序中哪个算法复杂度为o(n)


### 排序算法有哪些？

常用的10大排序算法：冒泡排序、选择排序、插入排序、希尔排序、归并排序、快速排序、堆排序、计数排序、桶排序、基数排序。

### 最快的排序算法是哪个？
### 手写一个冒泡排序

见代码：

### 手写快速排序代码

见代码：

### 快速排序的过程、时间复杂度、空间复杂度
### 手写堆排序

见代码：

### 堆排序过程、时间复杂度及空间复杂度
### 写出你所知道的排序算法及时空复杂度，稳定性

一张图清楚地说明这些算法之间的对比：

![](https://github.com/AweiLoveAndroid/CommonDevKnowledge/blob/master/pic/Sorting-algorithm.png?raw=true)


相关术语：
稳定：如果a原本在b前面，而a=b，排序之后a仍然在b的前面；
不稳定：如果a原本在b的前面，而a=b，排序之后a可能会出现在b的后面；
内排序（In-place）：所有排序操作都在内存中完成；
外排序（Out-place）：由于数据太大，因此把数据放在磁盘中，而排序通过磁盘和内存的数据传输才能进行；
时间复杂度： 一个算法执行所耗费的时间。
空间复杂度：运行完一个程序所需内存的大小

具体讲解可以参考博客：[https://www.cnblogs.com/onepixel/articles/7674659.html](https://www.cnblogs.com/onepixel/articles/7674659.html)

### 二叉树给出根节点和目标节点，找出从根节点到目标节点的路径
### 给阿里2万多名员工按年龄排序应该选择哪个算法？
### GC算法(各种算法的优缺点以及应用场景)
### 蚁群算法与蒙特卡洛算法
### 子串包含问题(KMP 算法)写代码实现
### 一个无序，不重复数组，输出N个元素，使得N个元素的和相加为M，给出时间复杂度、空间复杂度。手写算法
### 万亿级别的两个URL文件A和B，如何求出A和B的差集C(提示：Bit映射->hash分组->多文件读写效率->磁盘寻址以及应用层面对寻址的优化)
### 百度POI中如何试下查找最近的商家功能(提示：坐标镜像+R树)。
### 两个不重复的数组集合中，求共同的元素。
### 两个不重复的数组集合中，这两个集合都是海量数据，内存中放不下，怎么求共同的元素？
### 一个文件中有100万个整数，由空格分开，在程序中判断用户输入的整数是否在此文件中。说出最优的方法
### 一张Bitmap所占内存以及内存占用的计算
### 2000万个整数，找出第五十大的数字？
### 烧一根不均匀的绳，从头烧到尾总共需要1个小时。现在有若干条材质相同的绳子，问如何用烧绳的方法来计时一个小时十五分钟呢？
### 求1000以内的水仙花数以及40亿以内的水仙花数
### 5枚硬币，2正3反如何划分为两堆然后通过翻转让两堆中正面向上的硬8币和反面向上的硬币个数相同
### 时针走一圈，时针分针重合几次
### N*N的方格纸,里面有多少个正方形
### x个苹果，一天只能吃一个、两个、或者三个，问多少天可以吃完？

## 面试案例

阿里巴巴面试案例 https://maimai.cn/article/detail?fid=1160837648&efid=Pu_Xg-tnob2owjriFS-ehg

## 面试题

#一、图片

### 图片库的对比

图片库名称|实现原理|优点|缺点
-|-|-|-
Fresco||1.最大的优势便在于5.0以下(最低2.3) bitmap的加载，在5.0以下系统，Fresco将图片放到一个特别的内存区域(Ashmem)，而且图片不显示时，占用的内存会自动被释放，这会使APP更加流畅，减少因图片内存占用而引发的OOM。5.0以后的系统默认存储在Ashmem区了2.支持加载Git动态图和Webp格式的图片3.图片的渐进式呈现，图片先呈现大致的轮廓，然后随着图片下载的继续，逐渐成仙清晰的图片，这对于慢网络对说，用户体验更好。|框架体积比较大，3M左右，会增加APK的大小
Glide||1.图片默认格式为RGB565，而不是ARGB888，内存开销更小2.图片缓存的尺寸是和ImageView一样的，这使得图片加载更快3.支持加载Git动态图4.支持配置网络请求5.Glide的with()方法可以接收activity和fragment，图片的加载会和Activity和Fragment的生命周期保持一致|Glide 显示动画会消耗很多内存
Picasso||1.图片质量高，但加载速度一般2.Picasso体积比起Glide小|只缓存一个全尺寸的图片

具体的可以参考这篇博客[Android的Glide库加载图片的用法及其与Picasso的对比](http://www.jb51.net/article/83152.htm)，虽然有点老了，新版本的api有许多变动，但是还是有参考价值的。（**以后有时间我把最新的Glide 4.+ 和 Picasso最新版  Fresco 做一个对比，写一篇博客介绍一下**）

### 图片库的源码分析

这个网上有很多博客，自己可以去看看，内容太多，不方便在这里写出来了。

### 自己去实现图片库，怎么做？

这里介绍一下思路，详细的可以看我给出的一个示例sample，自己手写的一个类似于Picasso的图片框架

**需要注意的几点：**

* 1.首先要做好缓存，这是必备的：可以使用三级缓存，Android提供了LruCache：硬盘缓存用DiskLruCache，内存缓存用LRUCache。
* 2.图片加载需要压缩，可以考虑BitmapFactory做压缩
* 3.图片加载的来源，是从文件，还是网络
* 4.图片的异步加载和同步加载
* 
* 知识点内存缓存 

* 图片下载时请求转发


IEG Round 1

1. 自我介绍 

2. 项目经验

3. Android 基础

3. 算法

4. Java基础

5. 自己做过东西的亮点

Android基础：

1. 事件分发机制， U字型

2. 动画的实现方式

3. Activity的启动模式 SingTask SingTop SingleInstance Standard

Java

1. 双亲委托模式

2. JVM的内存模型

3. volite字段使用

4. 

算法

1. 输入一个数字，输出读法，例如1234，输出一千二百三十四

2. 


## App的生产周期

持续打包集成接入
性能监控接入

数据结构

    1、栈 Stack 
    2、队列 Queue 
    3、链表 Linked List 
    4、数组 Array 
    5、哈希表 Hash Table 
    6、二叉树 Binary Tree 
    7、堆 Heap 
    8、并查集 Union Find 
    9、字典树 Trie

算法

    1、二分搜索 Binary Search 
    2、分治 Divide Conquer 
    3、宽度优先搜索 Breadth First Search 
    4、深度优先搜索 Depth First Search 
    5、回溯法 Backtracking 
    6、双指针 Two Pointers 
    7、动态规划 Dynamic Programming 
    8、扫描线 Scan-line algorithm 
    9、快排 Quick Sort

计算机网络

    1、简述TCP/IP体系？ 
    2、TCP与UDP区别与应用？ 
    3、GET，POST区别（计算机底层实现的区别）？ 
    4、Https 理论基础及Https在Android中的应用（HTTPS 理论基础及其在 Android 中的最佳实践 ， 浅谈https\ssl\数字证书）

六大原则和设计模式

    1、单一职责原则 
    2、开闭原则 
    3、里氏替换原则 
    4、依赖倒置原则 
    5、接口隔离原则 
    6、迪米特原则

    1、单例模式 
    2、Builder模式 
    3、原型模式 
    4、工厂方法模式 
    5、抽象工厂模式 
    6、策略模式 
    7、状态模式 
    8、责任链模式 
    9、解释器模式 
    10、命令模式 
    11、观察者模式 
    12、备忘录模式 
    13、迭代器模式 
    14、模板方法模式 
    15、访问者模式 
    16、中介者模式 
    17、代理模式 
    18、组合模式 
    19、适配器模式 
    20、装饰漆器模式 
    21、享元模式 
    22、外观模式 
    23、桥接模式 
    24、MVC、MVP、MVVM 模式

Java知识

    1、String、StringBuffer、StringBuilder 的区别？String为什么是不可变的？ 
    2、Vector、ArrayList、LinkedList的原理和区别？ 
    3、HashTable、HashMap、TreeMap原理和区别？ 
    5、常见编码方式？utf-8编码中的中文占几个字节？int型几个字节？ 
    6、JVM(Java虚拟机) 
    7、ThreadLocal 原理 
    8、synchronize和volatile （Java并发编程：volatile关键字解析）手写一个死锁例子
    9、线程及线程池

## Android知识

系统相关

    1、Android系统启动流程 ？ 
    2、Jvm、Art 和 Dalvik对比？ 
    3、点击 Android Studio 的 build 按钮后发生了什么？ 
    4、Android应用安装到手机上时发生了什么？ 
    5、Android应用启动流程？ 
    6、Android进程和 Application 的生命周期及保活方案？ 
    7、Android的 Inter-Process-Communication （IPC）跨进程通信？ 
    8、Binder 机制？ 
    9、Handler 机制？ 
    10、Activity、Service、Fragment的生命周期和启动模式？ 
    11、SP是进程同步的吗?有什么方法做到同步； 
    12、SpareArray原理？

架构相关

    1、组件化 
    2、插件化 
    3、热修复 
    4、换肤原理

View相关

    1、View工作原理？ 
    2、View的事件体系？ 
    3、SurfaceView和TextureView的区别？

图片加载相关

    1、Bitmap的加载原理？ 
    2、Android中的缓存策略？ 
    2、LruCache 底层原理？

优化相关

    1、内存回收机制与GC算法(各种算法的优缺点以及应用场景)； 
    2、内存泄露场景及避免和解决方法 
    3、Android性能优化 
    4、Android网络优化

开源库

    1、Glide源码解析 
    2、Okhttp源码解析


VIVO面试题
主要从项目经验中引申出解决问题的思路

优化启动速度做了什么事情？

优化内存做了什么？

一个应用起来后有什么线程？

Binder传输数据大小限制 1M

Binder并发数优先上限？无

四大组件相关：

1.启动一个Activity，在应用进程至少需要两个Binder线程。

2.启动一个launchMode为singleTask的Activity，它并不一定会运行在新的Activity栈中。

3.两个不同应用的Activity，可以运行在同一个Activity栈中。

4.同一个应用进程中的所有Activity，共享一个WindowSession。

5.弹出一个AlertDialog，不一定需要Activity级别的Context，而且任何地方都有办法弹出一个AlertDialog，只要是在Application的attachBaseContext之后。

6.可以通过设置Activity主题android.R.style.Theme_NoDisplay，来启动一个不显示的Activity，在某些需要过渡的地方很实用。

7.Activity、Service、Receiver在没有配置intent-filter的action属性时，exported默认为false，配置了intent-filter的action属性时，exported默认为true。稍有不慎，很可能埋下越权、Intent攻击等安全隐患。

8.当从最近使用应用列表中移除某个App时，四大组件只有Service拥有神奇的onTaskRemoved回调，但是并不一定回调，还与stopWithTask属性等有关。

9.四大组件都运行在主线程，是因为它们在ActityThread中（或Instrumentation）实例化；它们的生命周期也运行在主线程，是因为通过ActivityThread.H将消息从Binder线程发送到主线程，然后执行回调。

10.TaskStackBuilder的出现基本上解决了所有构造Activity回退栈的问题。

11.ContentProvider的onCreate()方法先于Application的onCreate()方法执行，晚于Application的attachBaseContext()方法，所以在ContentProvider的onCreate()时候也是有办法弹出一个AlertDialog的（参考5）。

12.BroadCastReceiver回调onReceive(Context context,Intent intent)中的context类型各种场景相差很大，静态注册的receiver回调的Context都是ReceiverRestrictedContext，动态注册的receiver有可能是Activity或Application。

13.ServiceRecord和BroadcastRecord自身就是Binder。

14.同一个provider组件名，可能对应多个provider。



Handler、Message相关：

1.MessageQueue.addIdleHandler可以用来在线程空闲的时候，完成某些操作，比较适合那种需要在将来执行操作，却又不知道需要指定多少延迟时间的操作。

2.Message.what尽量不要设置成0，因为postRunnable的方式会生成Message.what为0的消息，如果删除了what为0的Message，也会将runnable方式创建的Message删掉。

3.Handler可以设置同步异步（默认是同步的），他们的区别在于异步不会被Barrier阻塞，而同步会被阻塞。

4.Handler的消息分发流程是如果Message的callback不为空，通过callback处理，如果Handler的mCallback不为空，通过mCallback来处理，如果前两个都为空，才调用handleMessage来处理。在DroidPlugin中，便是利用ActivityThread.H的这一特性，拦截了部分消息，实现Activity的插件化。

5.Java层和Native层Looper、MessageQueue的创建时序，Java层Looper—>Java层MessageQueue—>Native层NativeMessageQueue—>Native层Looper。

6.Java层通过Handler去发送消息，而Native层是通过Looper发消息。


Window、View相关：

1.硬件加速在Window级只能开不能关，View级只能关不能开。

2.自android2.3删除MidWindow后，PhoneWindow成了Window的唯一实现类。

3.WMS管理Window的过程中涉及4个Binder，应用进程只有ViewRootImpl.W一个Binder服务端。

4.MotionEvent、KeyEvent、DragEvent等具有相似的链式缓存，类似Message。

5.在View的状态保存、恢复过程中，ActionBar中所有View共享一个SparseArray容器，ContentView中所有View共享一个SparseArray容器。当前获取焦点的View会额外存储。

6.设置ViewTreeObserver的系列监听方法需要确保View在attachToWindow之后，否则可能因为add监听和remove监听不是作用于同一个对象而引起内存泄漏等。



Binder、IPC、进程等相关

1.可以通过文件锁来实现进程间互斥（参考：RePlugin），在处理某些只需要单进程执行的任务时很实用。

2.Binder设计架构中，只有Binder主线程是由本进程主动创建，Binder普通线程都是由Binder驱动根据IPC通信需求被动创建。

3.oneway与非oneway，都需要等待Binder Driver的回应消息（BR_TRANSACTION_COMPLETE），区别在于oneway不用等待BR_REPLY消息。

4.mediaserver和servicemanager的主线程都是binder线程，但system_server的主线程不是Binder线程，system_server主线程的玩法跟应用进程一样。

5.同一个BpBinder可以注册多个死亡回调，但Kernel只允许注册一次死亡通知。

6.应用进程由Zygote进程孵化而来，在它真正成为应用进程之前，系统通过抛异常的方式来清理栈帧，并反射调用ActivityThread的main方法。

7.在Binder通信的过程中，数据是从发起通信进程的用户空间直接写到目标进程内核空间，内核空间的数据释放是由用户空间控制的。

App进程则是我们常说的应用程序，主线程主要负责Activity/Service等组件的生命周期以及UI相关操作都运行在这个线程； 另外，每个App进程中至少会有两个binder线程 ApplicationThread(简称AT)和ActivityManagerProxy（简称AMP）



一般来说，大部分面试官在面试所耗用的时间都不是固定的，以我平均的面试时间做个标准的话，基本上是在 45 分钟之间，如果碰到比较优秀的 Android 面试者可能会延长面试的时间。

但是如果是我主动延长面试时间的话，那么首先要恭喜这位朋友了我是对他感兴趣的，不出意外的话，基本上这位朋友应该是稳了。

因为面试本来就是一个输入和输出的过程，面试官主要的职责是要引导面试者在短时间展现他最强的实力，如果一位面试官连这点都做不好，我敢断定这决对不是一位合格的面试官。

那对于面试官，该如何完成一次高效的 Android 面试呢？

面试前的准备
1.简历调查

简历到你手上的时候，你要做好充分的调查分析,不仅仅是对公司负责，也是对自己与候选人时间的尊重，明显不“对劲”的简历，就不要抱着“要不喊过来试试看”的想法了，候选人也许很不错，但如果跟你的岗位不匹配,也不要浪费大家时间，你要想清楚现在需要的人是有潜力可以培养的，还是亟需帮忙干活的。另外如果简历里附带了博客链接，GitHub地址， 相关作品的，可以提前去看看，直接看人家多年积累的文章与代码，比这短短一小时的面试来得靠谱的多。

2.准备问题

了解清楚候选人背景后，要根据简历，有针对性的准备问题，可以是他作品或做过项目里的某个技术细节的实现方式，也可以是他声称精通的某些领域的相关问题。总之不要等到面试过程中现想问题，特别是刚开始面试别人的同学，往往经验不足稍带紧张导致大脑短路，其实也是很尴尬的，把要问的问题提前写下来，准备充分。

面试官，你考察几个点?
1.简历是否真实

这其实是面试第一要务, 面试的过程其实就是看简历是否属实的过程，因为能到面试环节,说明这个人是符合要求的，不满足要求的早就被剔除了，如果他真的如简历描述的那样，100%会招过来，如果人人都如此，那就不需要有面试这种过程了。

需要注意的是这里的真实有三层含义:

是他如实描述了自身经历，很多人只在一些大项目里做一个很小的螺丝钉，但简历里往往夸张这段经历。
二是不知道自己不知道，常见于简历里各种“精通”开头的描述，因为知识体系与视野的局限，明明只是了解很浅却夸口精通，很多时候他并不认为自己说的有问题，而是真的以为自己已然精通，有点井底之蛙的感觉。
三是简历里的真实要与你的期望相匹配，一门技术了解到怎样的程度才算精通，很难有定论，所以这里的“真实" 只能是候选人与面试官标准之间的契合，这种有主观运气成分，也许面试官水平不够错误判断了你，也不用感到不爽，面试何尝不是种双向选择呢。
2.技术的深度

技术的深度一向是我最看重的部分，当今任何一个技术领域都非常宽广，一个人要同时掌握那么多知识并且都深入几乎不可能，那都需要拼学习效率与工作年限了。而你曾经做过的东西，正在做的东西，是绝对可以了解得更深入的，一个对技术有好奇心，有技术热情的人，都不会仅仅停留在这个东西挺好用，而是会忍不住去探究它背后的技术原理，即便不是亲自去看源码，也会花点时间了解别人整理过的经验，所以单凭考察技术上的深度，就可以考察-个人是否对技术有热情 ,是否有技术好奇心等等这些很多大牛认为的所谓“优秀程序员的特征”。

之前曾看到过一句话:“一个人对他所做的事情了解得越深，他就能做的越好”。放在这里再合适不过了。

3.技术的广度

深度是有了，还需要广度吗?我个人的理解是:深度是必要条件，广度是加分项。同样的有技术好奇心的优秀程序员，也不会满足于仅仅局限于自己的一亩三分地，工作之余，也会想要尝试一些其它的领域和方向，因为投入问题也许不够深入，但很多领域知识你知道与不知道，对你个人知识体系的形成关系很大。

比如你要实现一个功能， 在你当前熟悉的技术领域上很困难或者效果不佳，在你就要放弃时你的同事告诉你，这用一个简单Sqi语句就可以实现啦,为什么要搞得那么麻烦？这个例子虽然举得很蹩脚，但是我想意思大家应该已经明白了。知识越有厂度，头脑里的技术体系就越备，同样的问题，你就可以想到N个解，思考一下就得出最优解了 。

如果你听都没听过一些东西，就会经常说出“这个好难搞啊”、"这根本就不可能”其实有的时候真是知识的局限问题，所谓的从0到1难，也是这个意思。

4.逻辑思维能力

这也是我比较看重的一点，这里并不是指那些臭名昭彰的脑经急转弯问题，而是通过交流观察,判断一个人表达观点逻辑是否清晰，回答问题是否有章法，这个很难描述，但如果你细心观察,你会发现很容易通过一些简单的交流，就可以看出一一个人是否逻辑清晰。

有时候你会觉得某个人表达沟通很不错，其实不是沟通的问题，是他说出去的话，经过了他大脑的条理清晰的整理，让你很容易就能明白。这种习惯不是一朝一夕就能养成的，所以面试过程中这点装不出来。

另外一个人如果逻辑清晰，而且反应又敏捷，语速很快，那是大大的加分项，恭喜你，碰到一个聪明人了。


那对于面试者，面试官通常会问哪些深度与广度并存的 Android 面试题呢？

因知乎平台的推荐机制，这里只列举我曾经面过的几类很经典的 Android 面试题，不过我已经把这些年来收集所有大厂的 Android 中高级面试题整理成核心知识上传至我的【Github】。
如有面试需要的朋友，可以点击“【Github】”参考复习

【Github】
​
github.com
第一章 Android组件
1. Activity的4大启动模式，与开发中需要注意的问题，如onNewIntent() 的调用；
2. Activity A跳转B，B跳转C，A不能直接跳转到C，A如何传递消息给C？(美团)
3. Activity如何保存状态的？
4. 请描诉Activity的启动流程，从点击图标开始。(B站)
5. APP是怎么启动的？
6. 启动一个Activity的流程分析
7. Service的生命周期是什么样的？Service两种生命周期以及区别
8. 你会在什么情况下使用Service？
9. startServer和bindServier的区别？(美团)
10. Service和Thread的区别？
11. IntentService与Service的区别？
12. ContentProvider如何自定义与使用场景是什么？
13. BroadcastReciver的静态注册与动态注册的区别？
14. 广播的分类与工作原理
15. 可以再onReceive中开启线程么，会有什么问题？
16. 什么是有序广播？
17. Application、Activity、Service中context的区别？能否启动一个activity、dialog?
18. Fragment的生命周期？ （美团）
19. Fragment的构造函数为啥不让传参？（B站）
20. Fragment add与replace的区别，分别对Fragment的生命周期影响（美团）
第二章 View System
1. View绘制流程与自定义View注意点。（东方头条、美团）

Android中的每一个UI控件都是集成自View,然后这些View都具有相同的绘制流程，必须经过measure,layout和draw.
view的绘制流程是在Window添加过程中，ViewRootImpl类的setView方法开始的
2. 在onResume中可以测量宽高么
3. 事件分发机制是什么过程？（东方头条）
4. 事件冲突怎么解决？（东方头条）
5. View分发反向制约的方法？（字节跳动）
6. 自定义Behavior，NestScroll，NestChild。（东方头条）
7. View.inflater过程与异步inflater（东方头条）
8. inflater为什么比自定义View慢？（东方头条）
9. onTouchListener onTouchEvent onClick的执行顺序。（58 京东）
10. 怎么拦截事件 onTouchEvent如果返回false onClick还会执行么？（58 京东）
11. 事件的分发机制，责任链模式的优缺点 （美团）
12. 动画的分类以及区别（车和家）
13. 属性动画与普通的动画有什么区别？（车和家）
14. 插值器 估值器的区别（车和家）
15. RecyclerView与ListView的对比，缓存策略，优缺点。（美团）
16. WebView如何做资源缓存？（字节跳动）
17. WebView和JS交互的几种方式与拦截方法。（字节跳动）
18. 自定义view与viewgroup的区别
19. View的绘制原理
20. View中onTouch，onTouchEvent和onClick的执行顺序
21. View的滑动方式
22. invalidate() 和 postInvalicate() 区别
23. View的绘制流程是从Activity的哪个生命周期方法开始执行的
24. Activity,Window,View三者的联系和区别
25. 如何实现Activity窗口快速变暗
26. ListView卡顿的原因以及优化策略
27. ViewHolder为什么要被声明成静态内部类
28. Android中的动画有哪些? 动画占用大量内存，如何优化
29. 自定义View执行invalidate()方法,为什么有时候不会回调onDraw()
30. DecorView, ViewRootImpl,View之间的关系，ViewGroup.add()会多添加一个ViewrootImpl吗
31. 如何通过WindowManager添加Window(代码实现)？
32. 为什么Dialog不能用Application的Context？
33. WindowMangerService中token到底是什么？有什么区别
34. RecyclerView是什么？如何使用？如何返回不一样的Item
35. RecyclerView的回收复用机制
36. 如何给ListView & RecyclerView加上拉刷新 & 下拉加载更多机制
37. 如何对ListView & RecycleView进行局部刷新的？
38. ScrollView下嵌套一个RecycleView通常会出现什么问题？
39. 一个ListView或者一个RecyclerView在显示新闻数据的时候，出现图片错位，可能的原因有哪些 & 如何解决？
40. Requestlayout，onlayout，onDraw，DrawChild区别与联系
41. 如何优化自定义View
42. Android属性动画实现原理，补间动画实现原理

第三章 Android FrameWork
1. Android中多进程通信的方式有哪些？ 进程通信你用过哪些？原理是什么？（字节跳动、小米）
2. 描述下Binder机制原理？（东方头条）
3. Binder线程池的工作过程是什么样？（东方头条）
4. Handler怎么进行线程通信，原理是什么？（东方头条）
5. Handler如果没有消息处理是阻塞的还是非阻塞的？（字节跳动、小米）
6. handler.post(Runnable) runnable是如何执行的？（字节跳动、小米）
7. handler的Callback和handlemessage都存在，但callback返回true handleMessage还会执行么？（字节跳动、小米）
8. Handler的sendMessage和postDelay的区别？（字节跳动）
9. IdleHandler是什么？怎么使用，能解决什么问题？
10. 为什么Looper.loop不阻塞主线程？Looper无限循环为啥没有ANR（B站）
11. Looper如何在子线程中创建？（字节跳动、小米）
12. Looper、handler、线程间的关系。例如一个线程可以有几个Looper可以对应几个Handler？（字节跳动、小米）
13. 如何更新UI，为什么子线程不能更新UI？(美团)
14. ThreadLocal的原理，以及在Looper是如何应用的？（字节跳动、小米）
15. Android 有哪些存储数据的方式？
16. SharedPreference原理，commit与apply的区别是什么？使用时需要有哪些注意？
17. 如何判断一个 APP 在前台还是后台？
18. 如何做应用保活？
19. 一张图片100x100在内存中的大小？（字节跳动）
20. Intent的原理，作用，可以传递哪些类型的参数?
21. 如果需要在Activity间传递大量的数据怎么办？
22. 打开多个页面，如何实现一键退出?
23. LiveData的生命周期如何监听的?(B站)

第四章 性能优化
1. 内存优化，内存抖动和内存泄漏。 什么时候会发生内存泄漏？举几个例子（美团）
2. Bitmap压缩，质量100%与90%的区别？（东方头条）
3. TraceView的使用，查找CPU占用（东方头条）
4. 内存泄漏查找 （酷我音乐）
5. ANR是什么，怎么解决？ANR查找(美团)
6. CPU波动
7. 当前项目中是如何进行性能优化分析的
8. 冷启动、热启动的概念（酷我音乐）
9. View层次过深怎么优化，选择哪个布局比较好？（美团）
10. 怎样检测函数执行是否卡顿 (字节跳动)

第五章 开源框架
1. LeakCanray 2.0为啥不需要在application里调install？（B站）
2. OkHttp的原理（B站）
3. Glide缓存机制（B站）
4. Android如何发起网络请求，有用过相关框架码？OkHttp框架解决了你什么问题？（美团）

第六章 NDK
1.Jni了解吗？有没有自己使用过,只要编译成功Hello World也算,与JAVA相比效率如何？（美团）
2.C++中引用和指针的区别。


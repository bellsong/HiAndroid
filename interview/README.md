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




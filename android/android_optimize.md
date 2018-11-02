### Android性能专项

#### Android性能是什么？

整体表现上的性能，多快给到用户想要的结果

具体包括：
UI性能（界面卡顿），内存，CPU，电池，磁盘，网络流量，服务器资源

在开发，测试，灰度，发布各个阶段对性能问题的关注和诉求是不一样的，所采取具体措施也有所不同。

例如

开发阶段主要是防止低性能的设计，编码，例如可以通过IDE插件在编码阶段就对低性能的代码进行告警。

测试阶段，可以基于实验室环境，对卡顿，流量，IO，内存，启动速度等等性能数据进行详尽的自动化测试，越详细越好。

灰度阶段是基于线上环境，对一些核心性能点进行监测。

发布后则只针对一些核心指标进行粗粒度的监控。
  
* [启动优化专项](./android_speed_start.md)

* [Size优化专项](./android_optimize_size.md)

* [UI卡顿优化](./android_carton.md)

* [内存优化专项](./android_memory.md)

* [电量优化专项]

* [网络速度优化专项]

* [网络流量优化专项]

    [Android 流量优化(一)：模块化流量统计](http://www.atatech.org/articles/56540)


#### 检测工具
[Android性能测试工具列表](https://zmywly8866.github.io/2015/09/09/android-performance-tools.html)

[Hujiawei，魅族开发者](https://hujiaweibujidao.github.io/)

[Android性能：远程触发GC](http://www.atatech.org/articles/58479)

[Android性能：Release版如何排查CPU占用率高的问题](http://www.atatech.org/articles/58453)

[天猫Android性能优化2——文件IO如何设置Buffer](http://www.atatech.org/articles/56936)

####参考

[Android性能优化来龙去脉总结](https://juejin.im/entry/5b971f6c6fb9a05d151c883a?utm_source=gold_browser_extension)

https://juejin.im/entry/587c43a3570c352201039814

[Android性能优化典范（一）](http://www.csdn.net/article/2015-01-20/2823621-android-performance-patterns)

[Android 性能优化](http://rayleeya.iteye.com/blog/1961005)

[Android性能调优](http://www.trinea.cn/android/android-performance-demo/)

[Android性能优化典范](http://hukai.me/android-performance-patterns/)

[性能优化视频](https://www.youtube.com/playlist?list=PLWz5rJ2EKKc9CBxr3BVjPTPoDPLdPIFCE)

[Android性能优化案例研究](http://www.importnew.com/4065.html)

[Android性能优化案例研究](http://codingnow.cn/android/1378.html)

[Android应用性能剖析全攻略](http://toughcoder.net/blog/2015/09/11/android-performance-profiling-made-easy/)

http://www.atatech.org/articles/55521
http://toughcoder.net/blog/2015/09/11/android-performance-profiling-made-easy/
http://www.apkbus.com/forum.php?mod=viewthread&tid=121932&extra=page%3D1&page=1
http://blog.csdn.net/lmj623565791/article/details/58626355
http://blog.csdn.net/litton_van/article/details/21872677
http://blog.csdn.net/xjyzxx/article/details/45369069
http://www.atatech.org/articles/50851/?frm=mail_daily&uid=130616

http://motalks.cn/2016/09/11/Android-WebView-JavaScript-3/
http://www.csdn.net/article/2015-01-20/2823621-android-performance-patterns

### 卡顿优化点

* 1、图片加载框架线程优先级降低，减少对主线程CPU时间片的抢占
* 2、减少布局过度绘制，降低GPU绘制资源消耗
* 3、优化所有推荐集布局，减少布局深度，减少inflate时候遍历xml树的时间。
* 4、优化列表getView耗时操作，优化列表加载速度
* 5、优化搜索栏滚动字幕，每页减少50个TextView，减少不必要的测量布局和绘制。
* 6、优化首页ViewPager，去掉不可见tab页面的layout、measure。


开发过程关于卡顿的注意事项

* 1、布局时候注意减少布局层次。 Android布局采用XML树方式，每多一层，树的深度加一，inflate遍历时间会成倍增加，同时layout、measure、draw的时间也会相应增加。性能极致追求的做法是使用代码代替xml进行布局，可免去JAVA inflate 反射XML布局文件的时间。
* 2、减少布局的过度绘制。每多一层过度绘制，会多浪费一分GPU绘制资源，增加当前帧的绘制时间
* 3、注意子线程的数量和优先级管理。子线程虽然不会直接的阻塞UI，但是过多的子线程，特别是和UI线程相同优先级的线程，会不断抢占主线程的CPU时间片，造成主线程任务完成不及时，间接造成卡顿。
* 4、ListView getView减少耗时操作。在ListView getView做耗时的操作会直接ListView item的渲染速度，从而影响列表滑动卡顿。如当前PP各种复杂的推荐集，大大的增加列表项渲染时间，造成卡顿。
* 5、尽量维持最少的View个数。没多一个View，父布局在刷新绘制的时候，会多做一分测量、布局、绘制操作。
* 6、谨慎使用透明度动画。透明度动画绘制时候，会创建一个额外的Buffer，并且需要双倍的填充速度，是很耗性能的。
* 7、善用硬件加速。在一些动画效果很频繁的View，可以适当开启硬件加速加快布局渲染速度。
* 8、使用Animator替换Animation。Animator只会绘制当前View，Animation会将parent在内的所有子View都重绘。同一个View有多个Animator，可以使用PropertyValueHolder来合并一些重绘操作。
* 9、ViewPager使用注意页面回收和子页渲染。ViewPager本质上就是一个ViewGroup，当前页面的任务引起ViewGroup重绘的操作，都会引起其他所有子页面的绘制。如果每个子页面很复杂很庞大，会带来大量的卡顿。


###	修改点

* 1、对工程全部图片使用压缩工具进行压缩。
* 2、使用lint进行无用资源的检测并删除
* 3、个别大图转换图片格式
*     4、删除了重复的so库
*     5、删除废弃功能代码资源

     
###  发现的一些问题：
*     1、UI交付的图片有的并没有进行压缩，size较大（以前有跟UI沟通过这个问题并提供压缩工具）
*     2、经过几个版本后残留的资源较多
*     3、jar包较多（目前40个jar包，未打包情况下size为5.1M）
     
###  个人想法和建议：
*     1、研发收到UI交付的图片时也应留意一下图片的size。
*     2、对于不需要透明度，图片较大的图可以考虑转换成jpg，可以减少较可观的size。特别是色彩丰富的图片，收益更多。
*     3、无用的资源和代码需要及时删除掉，如果以后真的还需要这些资源时可以通过git找回。
*     4、对于一些简单的图片可以考虑通过代码去实现。
*     5、定期清理代码和资源，总能发现存在可以清理的。


### 往后优化方向：
*     1、尝试facebook的redex进行字节码优化
*     2、安装包大小监控, 实现了一个安装包检测工具(检测具体的细节并进行报警)
*     3、通过Web页面替代本地页面（一些交互少，功能简单的页面可以考虑）


### Android性能优化案例：手机淘宝

* [手淘安卓版性能优化1 - 性能相关概念介绍](http://www.atatech.org/articles/28721)
* [手淘安卓版性能优化2 - TraceView使用经验](http://www.atatech.org/articles/42723)
* [手淘安卓版性能优化3 - 其它性能分析工具的经验](http://www.atatech.org/articles/42862)
* [手淘安卓版性能优化4 - 手淘中存在的主要问题](http://www.atatech.org/articles/43376)
* [手淘安卓版性能优化5 - 手淘遇到的性能问题和优化工具](http://www.atatech.org/articles/43380)
* [手淘安卓版性能优化6 -  Dalvik虚拟机和ART运行时的介绍及对性能的影响](http://www.atatech.org/articles/43598)
* [手淘安卓版性能优化7- 启动阶段的问题和主要优化思路](http://www.atatech.org/articles/44028)
* [手淘安卓版性能优化7- 启动阶段的问题和主要优化思路](http://www.atatech.org/articles/45112)
* [手淘安卓版性能优化9- 其它因素对启动性能的影响过程分析](http://www.atatech.org/articles/45333)
* [手淘安卓版性能优化10- SharedPreference性能分析和使用规范](http://www.atatech.org/articles/45469)
* [手淘安卓版性能优化11 - 界面性能遇到的问题和主要使用的工具](http://www.atatech.org/articles/45552)
* [手淘安卓版性能优化12 - 界面性能之优化过度绘制](http://www.atatech.org/articles/45768)
* [手淘安卓版性能优化13 - 界面性能之布局性能和使用注意事项](http://www.atatech.org/articles/45920)
* [手淘安卓版性能优化14 - 界面性能之加快界面显示](http://www.atatech.org/articles/45999)
* [手淘安卓版性能优化15 - 界面性能之减少不必要的刷新和Layout请求](http://www.atatech.org/articles/46175)
* [手淘安卓版性能优化16 - 界面性能之帧率排查守得云开见月明](http://www.atatech.org/articles/46313)
* [手淘安卓版性能优化17 - 减少GC，提升性能（上）](http://www.atatech.org/articles/46524)
* [手淘安卓版性能优化18 - 减少GC，提升性能（下）](http://www.atatech.org/articles/46717)
* [手淘安卓版性能优化19 - 字符串与格式化的性能及注意事项](http://www.atatech.org/articles/46997)
* [手淘安卓版性能优化20 - 正确使用单例](http://www.atatech.org/articles/47006)
* [手淘安卓版性能优化21 - 避免使用枚举](http://www.atatech.org/articles/47400)
* [手淘安卓版性能优化23 - 深入理解final有关的性能](http://www.atatech.org/articles/48335)
* [手淘安卓版性能优化24 - SysTrace等工具的性能损耗及问题](http://www.atatech.org/articles/48396)
* [手淘安卓版性能优化25 - 减少Dex方法数并提升性能](http://www.atatech.org/articles/48717)

Weapons of mass destruction

#1、优化dex加载
## 背景
由于app的体量较大，超过了65536的天坑，于是我们的安装包被迫分为2个dex文件，第2个文件在第一次启动时会进行安装。（当然，这里暂不讨论为什么不精减到一个的问题，这是后面努力的方向），然而安装的过程在低端手机上惨不忍睹，普通需要4秒以上。我们的多DEX支持的代码是从Google官方的MultiDex和MultiDexExtractor中取出来的，那么这块是否还有优化空间了呢？
[http://www.atatech.org/articles/56465/?frm=mail_daily&uid=130616](http://www.atatech.org/articles/56465/?frm=mail_daily&uid=130616)

##现状
经过分析，安装过程中比较耗时的有2个过程，一是把classes2.dex从apk中取出来的，然后以zip的形式保存在程序目录下面，这个过程为extract。

private static void extract(ZipFile apk, ZipEntry dexFile, File extractTo,
            String extractedFilePrefix) throws IOException, FileNotFoundException {

        InputStream in = apk.getInputStream(dexFile);
        ZipOutputStream out = null;
        File tmp = File.createTempFile(extractedFilePrefix, EXTRACTED_SUFFIX,
                extractTo.getParentFile());

        TMLog.i(TAG, "Extracting " + tmp.getPath());
        try {
            out = new ZipOutputStream(new BufferedOutputStream(new FileOutputStream(tmp)));
            out.setLevel(Deflater.BEST_SPEED);
            try {
                ZipEntry classesDex = new ZipEntry("classes.dex");
                // keep zip entry time since it is the criteria used by Dalvik
                classesDex.setTime(dexFile.getTime());
                out.putNextEntry(classesDex);

                byte[] buffer = new byte[BUFFER_SIZE];
                int length = in.read(buffer);
                while (length != -1) {
                    out.write(buffer, 0, length);
                    length = in.read(buffer);
                }
                out.closeEntry();
            } finally {
                out.close();
            }
            TMLog.i(TAG, "Renaming to " + extractTo.getPath());
            if (!tmp.renameTo(extractTo)) {
                throw new IOException("Failed to rename \"" + tmp.getAbsolutePath() +
                        "\" to \"" + extractTo.getAbsolutePath() + "\"");
            }
        } finally {
            closeQuietly(in);
            tmp.delete(); // return status ignored
        }
    }



二是对这个dex进行安装（系统优化），这个过程为install。目前的情况，我用两台低端手机各测三次，结果如下：(单位毫秒)

手机1，TCL 
06-14 17:46:48.073 24963-24963/com.tmall.wireless E/TTTT: extract time1873
06-14 17:46:50.553 24963-24963/com.tmall.wireless E/TTTT: install time2477
06-14 17:47:34.193 26517-26517/com.tmall.wireless E/TTTT: extract time1878
06-14 17:47:35.883 26517-26517/com.tmall.wireless E/TTTT: install time1679
06-14 17:48:35.193 28026-28026/com.tmall.wireless E/TTTT: extract time1960
06-14 17:48:36.893 28026-28026/com.tmall.wireless E/TTTT: install time1698

手机2，华为电信赠送机

06-14 17:48:37.698 25220-25220/com.tmall.wireless E/TTTT: extract time2518
06-14 17:48:41.158 25220-25220/com.tmall.wireless E/TTTT: install time3447
06-14 18:02:49.658 10344-10344/com.tmall.wireless E/TTTT: extract time2168
06-14 18:02:52.058 10344-10344/com.tmall.wireless E/TTTT: install time2396
06-14 18:03:20.908 11684-11684/com.tmall.wireless E/TTTT: extract time2207
06-14 18:03:23.138 11684-11684/com.tmall.wireless E/TTTT: install time2224

classes2.dex.zip 文件大小为约为2.1M

尝试一
我们知道，PathClassLoader是可以加载zip, dex等文件，看上述源代码这一行：

new ZipOutputStream(new BufferedOutputStream(new FileOutputStream(tmp)));
它是把文件流取出来又重新压缩，如果不经过压缩这一步，直接取出来就是dex，那是不是会快一点呢？
动手尝试后，发现更慢了。原因在于SD卡的写入速度。原始的dex文件有5M多，而我这手机的SD卡的写入速度了10Mb/S，约为每秒1M的写入速度，实验数据太难看，就不发出来污大家眼了。

尝试二
有了前面的经验，我尝试对压缩的参数进行调整，

            out = new ZipOutputStream(new BufferedOutputStream(new FileOutputStream(tmp)));
            out.setLevel(Deflater.BEST_COMPRESSION);
猜一下结果，

TCL
06-14 17:46:48.073 24963-24963/com.tmall.wireless E/TTTT: extract time2564
06-14 17:46:50.553 24963-24963/com.tmall.wireless E/TTTT: install time2311
06-14 17:47:34.193 26517-26517/com.tmall.wireless E/TTTT: extract time2870
06-14 17:47:35.883 26517-26517/com.tmall.wireless E/TTTT: install time1899
06-14 17:48:35.193 28026-28026/com.tmall.wireless E/TTTT: extract time2509
06-14 17:48:36.893 28026-28026/com.tmall.wireless E/TTTT: install time1769

华为手机
06-14 17:43:28.883 20248-20248/com.tmall.wireless E/TTTT: extract time4015
06-14 17:43:30.983 20248-20248/com.tmall.wireless E/TTTT: install time2099
06-14 17:44:01.123 21681-21681/com.tmall.wireless E/TTTT: extract time3703
06-14 17:44:02.803 21681-21681/com.tmall.wireless E/TTTT: install time1676
06-14 17:44:26.623 22339-22339/com.tmall.wireless E/TTTT: extract time3626
06-14 17:44:28.243 22339-22339/com.tmall.wireless E/TTTT: install time1613

classes2.dex.zip 文件大小为约为2.1M

根本原因是压缩花了大量的时间，文件大小几乎没变！

尝试三
            out = new ZipOutputStream(new BufferedOutputStream(new FileOutputStream(tmp)));
            out.setLevel(Deflater.BEST_SPEED);
时间数据如下：

TCL
06-14 17:30:44.152 12719-12719/com.tmall.wireless E/TTTT: extract time924
06-14 17:30:45.762 12719-12719/com.tmall.wireless E/TTTT: install time1605
06-14 17:31:35.252 14335-14335/com.tmall.wireless E/TTTT: extract time865
06-14 17:31:37.072 14335-14335/com.tmall.wireless E/TTTT: install time1821
06-14 17:32:44.102 16292-16292/com.tmall.wireless E/TTTT: extract time838
06-14 17:32:45.782 16292-16292/com.tmall.wireless E/TTTT: install time1675

华为

06-14 17:33:31.285 10842-10842/com.tmall.wireless E/TTTT: extract time1028
06-14 17:33:34.045 10842-10842/com.tmall.wireless E/TTTT: install time2155
06-14 17:34:07.255 12350-12350/com.tmall.wireless E/TTTT: extract time985
06-14 17:34:09.515 12350-12350/com.tmall.wireless E/TTTT: install time2357
06-14 17:34:46.095 13734-13734/com.tmall.wireless E/TTTT: extract time1020
06-14 17:34:48.425 13734-13734/com.tmall.wireless E/TTTT: install time2030

classes2.dex.zip文件大小约为2.28M

可以看出，在install时间波动不大的情况下，解压的时间缩短了约1s的样子，使得安装第2个dex的整体时间从4到5s降低到3到4s，虽然结果还是很难看，但也算有20%到25%的提升。。。。

为了追求解压速度与SD卡写入速度的一个平衡，要求速度足够快，文件又不能太大。这就需要综合了解了Deflater算法，从网上抠了别人的测试结果。

时间图：


压缩数据图：
7bfc85c0d74ff05806e0b5a0fa0c1df1

由此可以看出，BEST_COMPRESS(值为9）需要的时间最久，效果也最好，BEST_SPEED(1)最快，效果看起来是最差。但是要考虑到IO的吞吐性能与时间的综合因素，压缩参数321的收益是最大的，我考虑到移动处理器较慢的影响，最后使用了BEST_SPEED来作为程序参数。

结论
压缩是程序中经常会用到的算法，包括处理网络协议，图片，文件等，这时我们要选择适当的压缩算法，调整适当的参数，才能让算法的收益达到最大。同时，我们也要对默认参数保持足够多的警戒心。


https://realm.io/cn/news/android-threading-background-tasks/
https://www.diycode.cc/news/2136
http://blog.vunso.com/201307/android%E5%9E%83%E5%9C%BE%E5%9B%9E%E6%94%B6%E6%9C%BA%E5%88%B6%E5%8F%8A%E7%A8%8B%E5%BA%8F%E4%BC%98%E5%8C%96system-gc.htm
http://www.tuicool.com/articles/FvABja
http://www.eoeandroid.com/thread-173838-1-1.html
http://www.eoeandroid.com/thread-153840-1-1.html
http://www.eoeandroid.com/thread-92711-1-1.html
http://cnetwei.iteye.com/blog/716964
http://www.atatech.org/articles/17189
http://androidperformance.com/2015/11/18/Android-app-lunch-optimize-delay-load.html
http://mp.weixin.qq.com/s?__biz=MzAwMTYwNzE2Mg==&mid=2651036594&idx=1&sn=b276c0f76cea713e5d568ab51e3f7f13&scene=0#wechat_redirect
https://mp.weixin.qq.com/s?__biz=MzA3MDMyMjkzNg==&mid=2652261762&idx=1&sn=dd95181222f40e6818d9df26ef31fab6&scene=0&pass_ticket=9/MdgGNR+cHQFO4rm/Vr61EvcK4BkZc9Tdb1bNWhlaRbGZLISyBuUC0YH2PdbNHd#rd
http://www.atatech.org/articles/57712/?frm=mail_daily&uid=130616
http://zhengxiaoyong.me/2016/07/18/Android%E7%AB%AF%E5%BA%94%E7%94%A8%E7%A7%92%E5%BC%80%E4%BC%98%E5%8C%96%E4%BD%93%E9%AA%8C/?hmsr=toutiao.io&utm_medium=toutiao.io&utm_source=toutiao.io
http://androidperformance.com/2015/11/18/Android-app-lunch-optimize-delay-load.html


[Android性能](http://www.jianshu.com/p/9c07323dc7e5?hmsr=toutiao.io&utm_medium=toutiao.io&utm_source=toutiao.io)

[破译Android性能优化中的16ms问题](http://www.jianshu.com/p/a769a6028e51)

[Android App优化之性能分析工具](http://www.jianshu.com/p/da2a4bfcba68)

* 卡顿
* [如何在Android中避免创建不必要的对象](http://droidyue.com/blog/2016/08/01/avoid-creating-unnecesssary-objects-in-android/?hmsr=toutiao.io&utm_medium=toutiao.io&utm_source=toutiao.io)

* [Android性能优化典范](http://www.csdn.net/article/2015-01-20/2823621-android-performance-patterns)

Android应用优化方案

前言：

前面两篇文章主要是讲关于activity、fragment生命周期方面的总结，这篇文章主要是总结在android应用开发过程的优化方案，还有一些常用的优化工具。优化的方向包括：启动速度、界面流畅性、内存使用情况、apk体积、耗电量、流量等方面。

app启动速度

1、通过style 设置一个默认的启动图来过度，从交互体验上来提高启动速度

2、分析application和首屏的业务逻辑异步初始化第三方组件，防止阻塞主线程（或者延迟初始化（用的时候再初始化））

3、闪屏的2秒停顿可以利用起来，把一些耗时操作延迟到这里来初始化

4、同工具DDMS中的TraceView来检测耗时的点在哪里，做针对的处理

5、mainActivity的onCreate流程，特别是UI的布局与渲染操作，如果布局过于复杂很可能导致严重的启动性能问题；（可以考虑先把mainActivity需要的数据请求回来），根据首页的结构可以考虑懒加载。

Android APP启动优化： wuxiaolong.me/2017/03/13/…

App启动速度优化之耗时检测处理： www.jianshu.com/p/a0e242d57…

使用 TraceView 找到卡顿的元凶： blog.csdn.net/u011240877/…

上面的几篇文章基本上描述了应用的启动流程，如何优化白屏，检测耗时以及一些SDK的懒加载等等...

界面流畅性

1、谈到UI流畅度，一般就是不要在主进程去做耗时的操作，提升UI的绘制速度（减少View的布局层级，避免过渡绘制等）

2、merge、include、ViewStub标签的合理使用减少布局层级

3、自定义view的ondraw里面不要做耗时的任务

Android UI性能优化实战 ：blog.csdn.net/lmj62356579…

性能优化之布局优化： www.trinea.cn/android/lay…

当然了对于UI卡顿，不可避免的要引入检测的方案：

方式1：一般有监听Looper的日志

方式2、利用Choreographer

当然也相应的有一些开源工具：

github.com/markzhai/An… [方式1]

github.com/wasabeef/Ta… [方式2]

github.com/friendlyrob… [方式2]


apk体积优化

代码瘦身

● 移除无用代码、功能；

● 移除无用的库、避免功能雷同的库；

● 启用Proguard；

● 缩减方法数；

●第三方开源库的瘦身，仅保留自己需要的部分

资源瘦身

● 移除无用的资源文件；

● Drawable目录只保留一份资源；

● 对图片进行压缩；

● PNG转换JPG；

● 使用矢量图；

● 使用WebP；

● 资源混淆；

● 资源在线化；

● 能不用图片的就不用图片实现，用代码实现

So瘦身

● 在允许的情况下，针对用户机型分布保留特定架构的So；

耗电量

电量是移动设备非常宝贵的资源,作为一名开发者,有必要为用户着想,减少电量的消耗.调查显示通常只有30%左右的电量是被程序核心的功能所消耗,比如界面渲染,剩下的70%则是被上报数据,位置更新,后台通知所消耗.

如何检测？

1、手机选项中通过查看APP的电量消耗的统计数据

2、使用Battery Historian Tool来查看详细的电量消耗

如何优化

●减少唤醒屏幕的次数与持续的时间,正确的使用WakeLock.

●延迟非必须的操作到充电状态时,比如日志上报完全可以在夜间充电时完成,这点可以结合JobScheduler使用

●使用传感器采集数据时,一旦不需要记得取消注册.

●减少网络通信,合并通信.

●合理使用定位功能,减少位置更新频率以及根据实际情况使用不同精度的定位需求

网络优化

现在App几乎都需要联网操作,做好网络优化一方面可以提高体验,另一方面可以减少流量和电量的损耗.另外,无论是对用户还是网络服务提供者,网络同样是一种资源,任何开发者都不应该假设网络资源是无限制的

如何检测

●使用Android Studio里的Network Traffic Tools来查看网络请求

●使用Android Studio中的Monitor，安卓studio3.0新的性能分析工具更方便

●使用Fidder或者Charles等抓包工具分析网络数据包

如何优化

●有必要的时候务必做好缓存,无论是图片还是普通的数据,使用LruCache和DiskLruCache构建自己的缓存系统,并根据实际场景设计缓存策略

●避免过度的网络同步,合并相关的网络请求

●根据实际场景确定请求策略,避免使用固定的间隔频率来进行网络操作.比如连接WiFi并充电的情况下请求频率可以高,第一次网络请求失败后可以使用双倍的时间间隔来进行下一次

●减少数据传输量,对传输的数据做压缩.如果传输的是图片,需要选择合适的图片格式以及根据显示大小请求合适规格的图片.对于普通数据,可以考虑使用ProtocalBuffers来减小传输数据的大小.

[android app性能优化大汇总（UI渲染性能优化）](https://www.jianshu.com/p/e058f88b305b)


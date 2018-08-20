## 启动优化专项

#### 现象
#### 原理
#### 工具
#### 方法
#### 原则

#### 现象
一般Android启动白屏

在手指按下桌面图标的时候，先"静止"一段时间，然后再启动，而且中间一点白色的间隙也没有，又是为什么？

启动分两种：冷启动 热启动
>
1、冷启动：当启动应用时，后台没有该应用的进程，这时系统会重新创建一个新的进程分配给该应用，这个启动方式就是冷启动。

特点：冷启动因为系统会重新创建一个新的进程分配给它，所以会先创建和初始化application类，再创建和初始化MainActivity类（包括一系列的测量、布局、绘制），最后显示在界面上。

2、热启动：当启动应用时，后台已有该应用的进程（例：按back键、home键，应用虽然会退出，但是该应用的进程是依然会保留在后台，可进入任务列表查看），所以在已有进程的情况下，这种启动会从已有的进程中来启动应用，这个方式叫热启动。

特点：热启动因为会从已有的进程中来启动，所以热启动就不会走application这步了，而是直接走MainActivity（包括一系列的测量、布局、绘制），所以热启动的过程只需要创建和初始化一个MainActivity就行了，而不必创建和初始化application，因为一个应用从新进程的创建到进程的销毁，application只会初始化一次。

既然上述问题不是出在application，那么肯定就是在Activity了，我是这么想的，然后我就想着是不是SetContentView的时候花了很多时间呢？然后我又测试了一遍：

---
@Override
protected void onCreate(Bundle savedInstanceState) {
    super.onCreate(savedInstanceState);
    long startTime = System.currentTimeMillis();
    setContentView(R.layout.activity_start);
    Log.d(TAG, "time===" + (System.currentTimeMillis() - startTime));
}


然后打印出来的时间是：
time = 210

果真是setContentView导致的，那就很好解决了,我们不要setContentView就好了，可能还有人要问了，你不要setContentView你咋加载布局呢？别急，别忘了还有theme这个好东西啊！我们可以定义一个theme，然后给theme设置背景就好了：
<style name="StartTheme" parent="AppTheme">
    <item name="android:windowBackground">@mipmap/icon_splash</item>
</style>

注：setContentView的内部原理有兴趣的同学可以自己去百度看看，看看在哪里耗时了

提个建议：当初我也尝试过是这么做的，但有个问题，图片的内存会释放不掉，所以放在activity的super调用前，用流资源方式加载图片，设置到window的背景中去就好了


#### 原理
[Android 官方关于启动说明](https://developer.android.com/topic/performance/vitals/launch-time)

[Android应用启动界面分析（Starting Window）](http://blog.csdn.net/xueshanhaizi/article/details/51262528)
[Android冷启动白屏解析，带你一步步分析和解决问题](http://blog.csdn.net/guolin_blog/article/details/51019856)

[Android应用启动优化:一种DelayLoad的实现和原理(上篇)](http://androidperformance.com/2015/11/18/Android-app-lunch-optimize-delay-load.html)

#### 原则
所以提高app的启动速度，加快Application的执行时间也是一个很重要的方面，这里我暂时总结了几条原则：

* 尽量不将一些业务逻辑放于Application中；

* Application尽量不以静态变量的方式保存应用数据；

* 若App的大小不是特别大无需使用dex分包方案；

* 在Application中关于文件，数据库的操作尽量

### 参考

#### 解决方案
[写启动界面Splash的正确姿势,解决启动白屏](http://www.jianshu.com/p/cd6ef8d3d74d)

[Android开发之解决APP启动白屏或者黑屏闪现的问题](http://blog.csdn.net/minimicall/article/details/39854969)

[Android app启动时黑屏或者白屏的原因及解决办法](http://www.manongjc.com/article/1221.html)

[Android 启动APP时黑屏白屏的三个解决方案](http://www.cnblogs.com/liqw/p/4263418.html)

  * [冷启动秒开](http://www.jianshu.com/p/03c0fd3fc245)



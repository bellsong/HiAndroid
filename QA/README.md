## 基础知识 
1. [简单讲一下AsyncTask的优缺点](./asynctask.md)

2. [如何解决65535天花板？](./android_q1.md)

3. [一种动态为apk写入信息的方案](http://pingguohe.net/2016/03/21/Dynimac-write-infomation-into-apk.html?hmsr=toutiao.io&utm_medium=toutiao.io&utm_source=toutiao.io)

4. [android 定时任务的解决方案](./android_alarm.md)

5. Andorid为什么不能再子线程更新UI？
    https://blog.csdn.net/self_study/article/details/50548894

    首先UI线程（mainThread）并不是线程安全的，这样如果子线程修改UI容易数据错乱，如果做到线程安全的话，这样做是很低效的。
    其次谷歌推荐如果子线程需要修改UI可以使用handler，这样的队列设也是考虑到并发，效率的体现。
    为什么 android 会设计成只有创建 ViewRootImpl 的原始线程才能更改 ui 呢？这就要说到 Android 的单线程模型了，因为如果支持多线程修改 View 的话，由此产生的线程同步和线程安全问题将是非常繁琐的，所以 Android 直接就定死了，View 的操作必须在创建它的 UI 线程，从而简化了系统设计。
    有没有可以在其他非原始线程更新 ui 的情况呢？有，SurfaceView 就可以在其他线程更新，具体的大家可以去网上了解一下相关资料。

## 工具使用
1. Gradle 升级处理

## APM相关
1. [Android ANR日志收集原理](android_anr.md)
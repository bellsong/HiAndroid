### UI卡顿优化

#### 如何检测应用在UI线程的卡顿

利用UI线程Looper打印的日志
利用Choreographer
两种方式都有一些开源项目，例如：

https://github.com/markzhai/AndroidPerformanceMonitor [方式1]
https://github.com/wasabeef/Takt [方式2]
https://github.com/friendlyrobotnyc/TinyDancer [方式2]

一个还比较有意思的方案，该方法的灵感来源于一篇给我微信投稿的文章：

https://github.com/android-notes/Cockroach

[Android UI性能优化 检测应用中的UI卡顿](http://blog.csdn.net/lmj623565791/article/details/58626355)

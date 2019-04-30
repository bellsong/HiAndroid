## UI卡顿优化


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

[破译Android性能优化中的16ms问题](http://www.jianshu.com/p/a769a6028e51)


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


## 参考
[为什么你的 App 会卡顿](https://juejin.im/entry/58f5b6a18d6d81006493bb7e)
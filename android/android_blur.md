### Android 高斯模糊 （Gaussian blur）

自从iOS系统引入了Blur效果，也就是所谓的毛玻璃、模糊化效果，磨砂效果，各大系统就开始竞相模仿

实现Blur效果主要有两种方式，

**一个是通过RenderScript来做**，
RenderScript是API11之后才引入的，所以对版本有限制，而且RenderScript确实挺复杂的，虽然使用他的Blur功能很简单，但是要真正搞懂，不是一天两天的事。

**另一种是通过直接对像素点来进行算法处理**。
本文主要介绍另一种算法来实现Blur，这个算法是目前市面上对Blur效果处理比较好的一种算法了，研究的前沿网址请戳 我是Blur 。

[Android（Android5.0）下毛玻璃（磨砂）效果如何实现？](https://www.zhihu.com/question/27017363)

[开源项目stackblur](https://github.com/kikoso/android-stackblur)

[FastBlur](http://blog.csdn.net/eclipsexys/article/details/39642865)

[Android 实现模糊效果的一些笔记](http://www.blog1980.info/2014/04/android_20.html)

[Android 图片的毛玻璃效果](http://blog.knero.cn/2014/10/26/Android-blur-image.html)

[Android 实现毛玻璃效果](http://blog.hwangjr.com/2015/07/10/Android-%E5%AE%9E%E7%8E%B0%E6%AF%9B%E7%8E%BB%E7%92%83%E6%95%88%E6%9E%9C/)


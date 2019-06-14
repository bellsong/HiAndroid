# ViewPager 

### PagerAdapter FragmentPageAdapter FragmentStatePagerAdapter

使用PageAdapter时，至少需要覆盖以下四个方法

* instantiateItem(ViewGroup, int)
* destroyItem(ViewGroup, int, Object)
* getCount()
* isViewFromObject(View, Object)


使用 FragmentStatePagerAdapter 更省内存，但是销毁后新建也是需要时间的。一般情况下，如果你是制作主页面，就 3、4 个 Tab，那么可以选择使用 FragmentPagerAdapter，如果你是用于 ViewPager 展示数量特别多的条目时，那么建议使用 FragmentStatePagerAdapter。


### 动画
* ViewPagerTransforms

    每当viewpager上一个可见或依附的页面发生了滚动事件就会调用PageTransformer，这让应用可以使用自定义transformation让viewpager某一个页面视图上实现某些特定的动画属性。

    但是这样的属性动画只能支持到android3.0版本或以上，在早期的版本上设置viewpager的PageTransformer会被忽略

    transformPage 应用属性动画到一个指定的页面


## 使用场景

* ViewPager来实现一个程序引导的demo
* 资讯类支持左右滑屏的场景


## 参考 

[ViewPager的简单使用](https://www.runoob.com/w3cnote/android-tutorial-viewpager.html)

[ViewPager使用详解](http://blog.csdn.net/wangjinyu501/article/details/8169924)

[Android中三种超实用的滑屏方式汇总（ViewPager、ViewFlipper、ViewFlow）](http://smallwoniu.blog.51cto.com/3911954/1308959)

[ViewPager 超详解：玩出十八般花样](https://juejin.im/post/5a4c2f496fb9a044fd122631)

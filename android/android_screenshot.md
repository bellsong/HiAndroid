# Android 截屏功能

## 系统组合键截屏
1. Android源码中对按键的捕获位于文件PhoneWindowManager.java（\frameworks\base\policy\src\com\android\internal\policy\impl）

2. interceptScreenshotChord();

3. takeScreenshot();
    启动了一个截图的service（ "com.android.systemui.screenshot.TakeScreenshotService")。android的一切功能都是handle通过sendmessage然后通过service实现的

4. TakeScreenshotService GlobalScreenshot

5. surfacecontrol类，位于/frameworks/base/core/java/android/view 这个类是被google隐藏

## 参考

[Android 截屏方式整理](https://juejin.im/entry/5980419d5188254ae4503c1c)

[Android屏幕截图那些事](https://www.zybuluo.com/olunx/note/18021)

[Android系统截屏的实现 需要ROOT](http://blog.csdn.net/buptgshengod/article/details/39155979)

[【android4.3】记一次完整的android源码截屏事件的捕获（不同于网上的老版本）](http://blog.csdn.net/buptgshengod/article/details/19911909)

[Android 高仿微信头像截取 打造不一样的自定义控件 ](http://tech.techweb.com.cn/thread-638113-1-1.html)

[微信等头像截取的实现](http://my.oschina.net/lifj/blog/68688)

[Android自动化测试中AccessibilityService获取控件信息（2）-三种方式对比](https://blog.csdn.net/itfootball/article/details/22063185)



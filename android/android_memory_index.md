## Android内存优化常规思路
### 背景
App开发中随着业务迭代，功能的增加，内存逐步上涨，为了保证平稳运行，减少内存占用过多衍生的卡顿、响应慢等用户体验问题，甚至内存溢出、ANR等稳定性问题
本文就内存优化实践过程，总结一下常规的优化手段。
                           
### 优化思路
1. 了解现状通过当前压测结果，切入业务进行了解，借助工具分析内存占用情况（MAT或Memory Profiler）注意，一定要运用数据来说明现状问题，后续也通过数据来验证优化效果。
常见内存占用多的情况：内存泄漏、图片（本地和网络）、非常驻业务的对象缓存、WebView

2. 制定方案针
对Launcher承载的业务和内存情况，分别进行内存泄漏排查、本地图片优化、业务解耦及时回收不使用的内存对象，WebView处理等

3. 逐步执行见下面【内存优化手段】

4. 分步验证

每完成一个优化项，进行压测，通过数据对比优化效果。

5. 重复1-4直到达到目标

### 内存优化手段

1. 内存泄漏排查
常规的内存泄漏情况
a.生命周期长的对象一直持有生命周期已经结束的对象，导致一直无法释放，例如单例中传了Activity
b.handler内存泄漏（可以通过声明为静态,软引用方式引入Activity）
c.匿名内部类会引用外部类
d.WebView

2. 本地图片优化

1）利用TinyPng，通过合并图片中相似的颜色，通过将 24 位的 PNG 图片压缩成小得多的 8 位色值的图片，并且去掉了图片中不必要的 metadata（元数据，从 Photoshop 等工具中导出的图片都会带有此类信息），这种方式几乎能完美支持原图片的透明度。将App图片进行一轮优化。

影响点：保证图片在肉眼察觉不出的效果变化前提，减少图片加载时的内存。同时apk Size也能减少。

2）Bitmap处理
展示高分辨率图片的时候，最好先将图片进行压缩。压缩后的图片大小应该和用来展示它的控件大小相近，在一个很小的ImageView上显示一张超大的图片不会带来任何视觉上的好处，但却会占用我们相当多宝贵的内存，而且在性能上还可能会带来负面影响Bitmap是内存消耗的大头，当使用时要及时回收。
另外配置：inSampleSize：缩放比例，图片按需加载，避免不必要的大图载入。decode format：解码格式，选择ARGB_8888/RBG_565/ARGB_4444/ALPHA_8，存在很大差异。

3）纯色图处理技巧：
例如背景图，用代码颜色填充。

3. 网络图片优化方案
基于我们图片加载框架Glide，对于无限制下发的图片进行尺寸限制处理。
影响点：保证图片加载内存不受图片尺寸不规范导致内存增多。

4. 业务模块解耦方案
业务解耦，拆分进程（例如Webview的）

### 内存优化工具

1. LeakCanary
查看内存泄漏情况，详见https://github.com/square/leakcanary

2. AndroidStudio自带的 Android Profiler
比较直观的看到App的内存变化情况，并且能方便的dump操作，查看对象占用内存。MAT内存分析工具，快速查看内存中对象的占用大小，通过GC Root 链能看到哪些对象引用阻止了垃圾收集器的回收工作，并可以通过报表直观的查看到可能造成这种结果的对象

### 注意点
前后压测条件一致
保证验证工具一致

WebView内存问题
webview占用内存比较多，并且存在内存泄漏情况，使用时要多加留意，一般不直接在xml里面声明，直接通过代码创建对象，退出时候及时销毁，甚至可以考虑将webview相关的activity单独进程。

### 小结和后续

结合业务，通过常规的手段对内存进行优化，遵循”用完就放“的原则，优化过程通过数据验证效果。除了代码层面上注意，可考虑建立一套健全的性能监控体系，通过数据及时发现相关的性能问题。
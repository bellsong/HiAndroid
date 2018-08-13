Q1:ViewPager和Fragment中内嵌WebView，因为ViewPager缓存关系（默认是前后一页），当有四个Fragment或以上时，滑动到第三个时就有第一个被回收的问题，再次滑动回到第一个，就展示一个空的webview。

A1：可以通过直接设大ViewPager缓存的方式解决。

PS:

- 为什么Fragment被回收了，就无法再次展示出来？
- ViewPager的缓存机制是怎么样的？read the fucking source


[Android中用ViewPager和Fragment内嵌WebView]()

WebView很好很强大，但是在Android中加载慢。 

在同一个Activity中，用ViewPager可以加载多个Fragment，切换视图比较流畅，但是如果超出了3个Fragment，而且刚好Fragment中有WebView，体验就非常糟糕，页面要好几秒才能显示出来。 

这是因为ViewPager缺省情况下，只把当前页的前一页和后一页放在缓冲区中。如果超出了3个Fragment，那么切换到第4个时，第一个会被销毁，第4个需要重建。内嵌的WebView就要重新被加载。 

如果Frragment不是很多的话，那可以设置ViewPager.setOffscreenPageLimit，增加缓冲页面，避免WebView被重建。例如有4页，可以设置setOffscreenPageLimit(2)，保持当前页的前两页和后两页。 

巧的是，由于ViewPager可以预先加载和缓存fragment，避免了fragment中的WebView被无谓地刷新，体验反而更流畅了。如果WebView不在首页，那和原生开发的视图更没有太大区别。
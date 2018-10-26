# OKHttp 

一个开源框架的诞生解决什么样的问题？

使用场景是什么？

有什么优点和缺点？

技术选型上为何要用到这个？成本？风险？易用性？后期扩展和维护上？



"OkHttp是一款优秀的HTTP框架，它支持get请求和post请求，支持基于Http的文件上传和下载，支持加载图片，支持下载文件透明的GZIP压缩，支持响应缓存避免重复的网络请求，支持使用连接池来降低响应延迟问题。"



### 概览

"OkHttp是一款优秀的HTTP框架，它支持get请求和post请求，支持基于Http的文件上传和下载，支持加载图片，支持下载文件透明的GZIP压缩，支持响应缓存避免重复的网络请求，支持使用连接池来降低响应延迟问题。"



http是现在主流应用使用的网络请求方式, 用来交换数据和内容, 有效的使用HTTP可以使你的APP 变的更快和减少流量的使用

OkHttp 是一个很棒HTTP客户端:

- 支持SPDY, 可以合并多个到同一个主机的请求
-  使用连接池技术减少请求的延迟(如果SPDY是可用的话)
-  使用GZIP压缩减少传输的数据量
-  缓存响应避免重复的网络请求

当你的网络出现拥挤的时候,就是OKHttp 大显身手的时候, 它可以避免常见的网络问题,如果你的服务是部署在不同的IP上面的,如果第一个连接失败, OkHTtp会尝试其他的连接. 这个对现在IPv4+IPv6 中常见的把服务冗余部署在不同的数据中心上.  OkHttp 将使用现在TLS特性(SNI ALPN) 来初始化新的连接. 如果握手失败, 将切换到SLLv3

使用OkHttp很容易,   同时支持 异步阻塞请求和回调.

如果你使用OkHttp ,你不用重写你的代码,   okhttp-urlconnection模块实现了 java.net.HttpURLConnection 中的API,  okhttp-apache模块实现了HttpClient中的API。



[okHttp](https://gold.xitu.io/entry/5728441d128fe1006058b6b9)

[鸿洋](http://blog.csdn.net/lmj623565791/article/details/47911083)

[对OkHttp进行封装，实现了只查询缓存，网络请求失败自动查询本地缓存等功能](http://www.jcodecraeer.com/a/anzhuokaifa/androidkaifa/2015/1208/3760.html)

[Android Https相关完全解析 当OkHttp遇到Https](http://www.jcodecraeer.com/a/anzhuokaifa/androidkaifa/2015/0831/3393.html)

[OKHttp源码解析](http://www.jcodecraeer.com/a/anzhuokaifa/androidkaifa/2015/0326/2643.html)

[如何更高效地使用 OkHttp](http://www.jcodecraeer.com/a/anzhuokaifa/androidkaifa/2016/0222/3988.html)

[Android OkHttp完全解析 是时候来了解OkHttp了](http://www.jcodecraeer.com/a/anzhuokaifa/androidkaifa/2015/0824/3355.html)

[OkHttp的使用简介及封装，实现更简洁的调用](http://www.jcodecraeer.com/a/anzhuokaifa/androidkaifa/2016/0105/3831.html)

[OkHttp使用教程](http://www.jcodecraeer.com/a/anzhuokaifa/androidkaifa/2015/0106/2275.html)

资讯类的客户端

## 组成部分
#### 网络库
    okhttp
#### 数据存储
    greendao
#### 动画库
    oldnineanimation

#### 界面展示
1. Fragment


2. 下拉刷新库
[SVProgressHUD For Android 精仿iOS的提示库 SVProgressHUD，api也几乎一样。](https://github.com/saiwu-bigkoo/Android-SVProgressHUD)

3. Twitter 的点赞动画库
[LikeButton](https://github.com/jd-alexander/LikeButton)
Twitter's heart animation for Android

4. 图片加载库 com.github.bumptech.glide

#### 提高效率
[butterknife](http://jakewharton.github.io/butterknife/)

# 网络库 Y_OK_HTTP

#### 问题

​	一切离不开互联网，联网将一个个孤岛练成彼此沟通的世界，

联网是如何实现的？原理是什么？这一切的一切又是如何发生的？中间经过了什么等等。相关的疑问有待后续逐一发掘，这次讨论的范围，是一种网络请求和响应，一种相互约定好的协议，一种打破孤岛的通信机制。

[带你学开源项目：Meizhi Android 之 RxJava & Retrofit 最佳实践](http://android.jobbole.com/82737/)

[2016年最值得学习的五大开源项目](http://www.jianshu.com/p/8180cc105f01)

[开发第三方库最佳实践](http://www.jianshu.com/p/0aacd419cb7e#)

[Android工程师角度分析App使用的开源框架-1.支付宝](http://yeungeek.com/2017/02/05/Android%E5%B7%A5%E7%A8%8B%E5%B8%88%E8%A7%92%E5%BA%A6%E5%88%86%E6%9E%90App%E4%BD%BF%E7%94%A8%E7%9A%84%E5%BC%80%E6%BA%90%E6%A1%86%E6%9E%B6-1.%E6%94%AF%E4%BB%98%E5%AE%9D/)

[开源项目](http://www.trinea.cn/android/android-open-project-analysis-phase1/)

[20多个可以提高你安卓开发技能的开源app](http://www.jcodecraeer.com/a/anzhuokaifa/androidkaifa/2017/0214/7114.html)

[开源框架Android之史上最全最简单最有用的第三方开源库收集整理，有助于快速开发](http://www.tuicool.com/articles/jyA3MrU/)

[带你学开源项目：OkHttp--自己动手实现okhttp](https://wingjay.com/2016/07/21/%E5%B8%A6%E4%BD%A0%E5%AD%A6%E5%BC%80%E6%BA%90%E9%A1%B9%E7%9B%AE%EF%BC%9AOkHttp-%E8%87%AA%E5%B7%B1%E5%8A%A8%E6%89%8B%E5%AE%9E%E7%8E%B0okhttp/?hmsr=toutiao.io&utm_medium=toutiao.io&utm_source=toutiao.io)



[Fresco](./android_opensource_fresco.md)

[各大公司技术博客](http://asteam.cc/index.php/archives/10/?hmsr=toutiao.io&utm_medium=toutiao.io&utm_source=toutiao.io)


[【Android】Retrofit 2.0 的使用](http://www.jianshu.com/p/7efdc3477269)

[EventBus](./android_eventbus.md)

[开源项目](http://skyseraph.com/2015/11/30/Android/%E5%AE%8C%E6%95%B4%E5%BC%80%E6%BA%90%E9%A1%B9%E7%9B%AE%E8%8D%9F%E8%90%83%EF%BC%88Android%E7%AF%87%EF%BC%89/)

[github最火开源项目](http://www.csdn.net/tag/%E6%9C%80%E5%8F%97%E6%AC%A2%E8%BF%8E%E7%9A%84%E5%BC%80%E6%BA%90%E9%A1%B9%E7%9B%AE/news)



[如果你不知道一个Android的类怎么用，可以在Codota上面快速的找到很多不错的示例代码。](https://www.codota.com/)

[你是否还在为找不到合适的开源库而苦恼，Android Arsenal这个网站已经帮你做了一定的分类，可以帮你提高不少效率。](https://android-arsenal.com/)

[Android源码](https://android.googlesource.com/)

http://androidxref.com/
克隆Android一个模块的代码量是很多的，有时候我只想要几个类的代码怎么办？AndroidXRef这个网站可以让你单独搜索某个类，要哪几个下载哪几个即可。

http://grepcode.com/
除了AndroidXRef可以查看某个类的源代码外，GrepCode同样也能做到。而且GrepCode不限于Android的源码，这里也推荐一下。


源码分析的网站很多，这里举几个比较经典的网站。

http://a.codekk.com/
国内Android源码分析的先驱，由滴滴的技术专家Trinea发起，坦白的讲，这个项目对我的影响很大，我也从这里开始体会源码解读的魅力的。

http://0xcc0xcd.com/p/index.php
老罗，罗升阳的个人博客站点，很多人看过他博客里面是如何分析Android和Chrome的源代码的。非常好的一个网站，以前功力不够没能看懂文章，经过一段时间后再回去翻看一些文章，不得不赞。

http://gityuan.com/
GitYuan，MIUI系统工程师，他的博客经常分享Android系统源码解读的文章，质量很高。而且，更新频率也很高！

https://github.com/LittleFriendsGroup/AndroidSdkSourceAnalysis
CJJ，猪场（网易）的开发者，由他带领发起的Android SDK源码解析，同样推荐。

思考
实践
扩展
总结





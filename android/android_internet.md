
## 浅谈我所知道的Android网络技术

互联网是一个伟大的发明，打破了信息孤岛，让人们可以自由的共享和获取信息。

Android领域里面，如何接入互联网，从而获取到想要的信息呢？

个人认为，主要分为三步

1）链接

2）获取

3）解释

链接，用到的技术是网络传输协议里面的七层；

建立链接后，可以不断的获取想要的信息；

拿到信息后，进行解释转换，成为我们想要的信息；

### 链接
网络传输协议一般都是HTTP

在Android上发送HTTP请求的方式一般有两种，HttpURLConnection和HttpClient。

 * HttpURLConnection/URLConnection

注意： URLConnection和HttpURLConnection使用的都是java.net中的类，属于标准的java接口。HttpURLConnection继承自URLConnection,差别在与HttpURLConnection仅仅针对Http连接。 

这种方式 服务器返回的是网页的HTML代码！

* HttpClient

HttpClient是Apache提供的HTTP网络访问接口，从一开始的时候就被引入到了Android API中。(但是在Android6.0中删除了相关API，被OKHttp代替！) 

如果要使用

android {
    useLibrary 'org.apache.http.legacy'
}


使用步骤： 
1. 创建HttpClient对象。 
2. 创建请求方法的实例，并指定请求URL。如果需要发送GET请求，创建HttpGet对象；如果需要发送POST请求，创建HttpPost对象。 
3. 如果需要发送请求参数，可调用HttpGet、HttpPost共同的setParams(HetpParams params)方法来添加请求参数；对于HttpPost对象而言，也可调用setEntity(HttpEntity entity)方法来设置请求参数。 
4. 调用HttpClient对象的execute(HttpUriRequest request)发送请求，该方法返回一个HttpResponse。 
5. 调用HttpResponse的getAllHeaders()、getHeaders(String name)等方法可获取服务器的响应头；调用HttpResponse的getEntity()方法可获取HttpEntity对象，该对象包装了服务器的响应内容。程序可通过该对象获取服务器的响应内容。 
6. 释放连接。无论执行方法是否成功，都必须释放连接。 

> 问题 ：如果是HTTPS 怎么办？

上面个提及到在Android6.0中删除了HttpClient相关API，被OKHttp代替

OKHttp是什么来头？起初，是一家square公司封装的网络库，后面开源了，然后受到广大开发者欢迎，然后，GOOGLE就把它吸收进官方API了。

所以，本着不重新发明轮子的原则，用经得住考验的代码。


目前已知行业比较出名的网络请求框架

* OKHttp
* Volley

### 获取
    从网络回来的数据一般分为 文本格式 XML格式 还有比较常见的json格式

* 解析XML格式数据
    * Pull解析
    * SAX解析

* 解析JSON
    * 直接解释
    * 使用GSON

### 延伸阅读

[Android网络请求心路历程](http://www.jianshu.com/p/3141d4e46240)

[为什么Android开发者应该使用FlatBuffers替代JSON?](http://blog.chengdazhi.com/index.php/201?hmsr=toutiao.io&utm_medium=toutiao.io&utm_source=toutiao.io)

[android-remote-notifications：从远程JSON文件拉取通知显示在你的应用中](http://hao.jobbole.com/android-remote-notifications/?utm_source=www.jobbole.com&utm_medium=homepage-resources)

[Android Okhttp3+Retrofit2网络加载效率优化](http://itfish.net/article/63134.html)

[okhttp 实现 https 访问，支持 Android 4.X 系统 https 访问原创](http://blog.csdn.net/guiying712/article/details/56301736)

### 扩展 

* 单点下载
* 多点下载


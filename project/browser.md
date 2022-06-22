# Y Browser

## 浏览器业务介绍

大概从互联网诞生开始，浏览器就很快成为必不可少的应用之一。只要上网，一般用户基本都是通过浏览器软件进行。

浏览器新闻网站，看电视，听歌等等，统统可以通过浏览器进行。浏览器成为互联网世界中用户相互连接的重要入口。

## 浏览器功能

1. 网页加载
1. 首页导航

    形态：搜索框、导航栏，内容页，信息流
    
    技术：动态化，配置化

2. 搜索
3. 下载
4. 播放（音视频）

## 浏览器技术

1. 浏览器内核（Chrome内核）

1. Web容器

1. JSBridge 预加载 预渲染


## Android WebView 常用优化方法

1. 打开缓存

    H5的缓存机制有：
    
    1.1 Dom Storage
    
    1.2 Web SQL Database
    
    1.3 浏览器缓存机制

    1.4 Application Cache

    1.5 Indexed Database

    1.6 File System API 

2. 延迟图片加载

    onPageStarted()时view.getSettings().setBlockNetworkImage(true)

    onPageFinished()时view.getSettings().setBlockNetworkImage(false)

2. 常用资源放本地
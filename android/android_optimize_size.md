## Size专项

#### 瘦身目的
* 提高下载转化率
简单的小结便是：安装包越小，用户下载等待的时间越短，对手机存储配置小的设备体验愈佳，应用的下载转化率也就越高。
* 省流量
* 给用户好印象
* 节省空间

记得以前在腾讯大讲堂听微信大牛说过，微信第一个版本只有差不多 400KB ，瞬间膜拜。

#### Apk的组成部分
dex
res
assets
lib
resource.arsc
AndroidManifest.xml
META_INF
其他

#### 资源瘦身
* 尽量保持一份图片资源
* 使用 Drawable XML、Color、.9 PNG 代替 PNG
* 使用 JPG 代替 PNG
* 谨慎使用 WebP 代替 PNG
* 移除无用的资源
* 有损编码格式的音频文件代替无损格式的音频文件

#### Native瘦身

#### 代码瘦身
* 代码混淆
* 废弃功能的代码
* 重复代码
* 功能重叠的框架
* 无用的dependencies和jar包
* 减少对Support兼容包依赖
* 插件化

来源：
[Android 应用瘦身，从 18MB 到 12.5MB](https://www.diycode.cc/topics/586)
[[Android技术专题]APK瘦身看这一篇文章就够了](https://zhuanlan.zhihu.com/p/21962184?refer=zmywly8866)


### 相关工具
[ThinR gradle plugin](https://github.com/meili/ThinRPlugin/blob/master/README.zh-cn.md)
ThinR插件在编译时将除R$styleable.class以外的所有R.class删除掉，并且在引用的地方替换成对应的常量，从而达到缩减包大小和减少dex个数的效果。

该插件已经在蘑菇街app上使用，将包大小降低1mb（原包大小40mb）,dex数量减少3个。

### 相关方案
[微信 安装包立减1M--微信Android资源混淆打包工具:直接处理安装包. 不依赖源码，不依赖编译过程，仅仅输入一个安装包，得到一个混淆包](https://mp.weixin.qq.com/s?__biz=MzA3NTYzODYzMg==&mid=214472913&idx=1&sn=92b54b5fcd9bbab6513e46d92095a07f&scene=1&srcid=0427eTI2x0dnk2EsFnysnjZI#rd)

[美团Android 资源混淆保护实践](http://tech.meituan.com/mt-android-resource-obfuscation.html)

[APK瘦身记](http://www.atatech.org/articles/51081)
[Android客户端方法数精简的尝试](http://www.atatech.org/articles/57128)

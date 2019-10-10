## Size专项

#### 瘦身目的
* 提高下载转化率
简单的小结便是：安装包越小，用户下载等待的时间越短，对手机存储配置小的设备体验愈佳，应用的下载转化率也就越高。
* 省流量
* 给用户好印象
* 节省空间

记得以前在腾讯大讲堂听微信大牛说过，微信第一个版本只有差不多 400KB ，瞬间膜拜。

#### Apk的组成部分

* dex 代码
* res 资源
* assets 资源
* lib
* resource.arsc
* AndroidManifest.xml
* META_INF
* so
* 其他

#### 资源瘦身
* 移除无用的资源文件
* Drawable尽量保持一份图片资源
* 使用 Drawable XML、Color、.9 PNG 代替 PNG
* 使用 JPG 代替 PNG
* 使用矢量图
* 使用WebP
* 谨慎使用 WebP 代替 PNG https://developers.google.com/speed/webp/
* 对图片进行压缩
* 有损编码格式的音频文件代替无损格式的音频文件
* 能不用图片的就不用图片实现，用代码实现

#### Native瘦身

#### 代码瘦身
* 代码混淆 启用Proguard R8压缩(依赖AS 3.3)
* 移除无用代码、功能 静态扫描
* 移除无用的库、避免功能雷同的库
* 重复代码
* 功能重叠的框架
* 无用的dependencies和jar包
* 减少对Support兼容包依赖
* 插件化
* 缩减方法数
* 第三方开源库的瘦身，仅保留自己需要的部分

#### So瘦身
* 在允许的情况下，针对用户机型分布保留特定架构的So
           ndk {
               abiFilters "armeabi", "armeabi-v7a", "arm64-v8a", "arm64-v7a", "x86" // 保留这些架构的CPU
           }


## 实践

* 1、对工程全部图片使用压缩工具进行压缩  [图片压缩工具TinyPng](https://tinypng.com/)
* 2、使用lint进行无用资源的检测并删除
* 3、个别大图转换图片格式
* 4、删除了重复的so库
* 5、删除废弃功能代码资源

### 发现的一些问题：
*     1、UI交付的图片有的并没有进行压缩，size较大（以前有跟UI沟通过这个问题并提供压缩工具）
*     2、经过几个版本后残留的资源较多
*     3、jar包较多（目前40个jar包，未打包情况下size为5.1M）
     
###  个人想法和建议：
*     1、研发收到UI交付的图片时也应留意一下图片的size。
*     2、对于不需要透明度，图片较大的图可以考虑转换成jpg，可以减少较可观的size。特别是色彩丰富的图片，收益更多。
*     3、无用的资源和代码需要及时删除掉，如果以后真的还需要这些资源时可以通过git找回。
*     4、对于一些简单的图片可以考虑通过代码去实现。
*     5、定期清理代码和资源，总能发现存在可以清理的。


### 往后优化方向：
*     1、尝试facebook的redex进行字节码优化
*     2、安装包大小监控, 实现了一个安装包检测工具(检测具体的细节并进行报警)
*     3、通过Web页面替代本地页面（一些交互少，功能简单的页面可以考虑）

### 相关工具
1. [ThinR gradle plugin](https://github.com/meili/ThinRPlugin/blob/master/README.zh-cn.md)
ThinR插件在编译时将除R$styleable.class以外的所有R.class删除掉，并且在引用的地方替换成对应的常量，从而达到缩减包大小和减少dex个数的效果。

该插件已经在蘑菇街app上使用，将包大小降低1mb（原包大小40mb）,dex数量减少3个。

1. 图片压缩工具 [TinyPng](https://tinypng.com/)

1. APK Analyzer

APK Analyzer是AndroidStudio自带的APK文件分析工具，可以检查APK的内容，直观看到每个文件的大小，dex文件的组成，以及比较两个 APK 之间的差异。Raw File Size表示原文件大小，Download Size表示经过Google play处理压缩后的apk大小。我们关注Raw File Size，因为apk预置到系统中真实占用的是Raw File Size，各文件的Raw File size 加起来大于Apk size。

详细的工具说明参考谷歌官方文档：https://developer.android.google.cn/studio/build/apk-analyzer

1.压缩工具R8
Android Studio 3.3版本开始带来的新特性是新一代的代码压缩工具R8，新的R8工具使代码缩减过程更快和更有效率，一步到位地完成了所有的缩减（shrinking），去糖（desugaring）和 转换成 Dalvik 字节码（dexing ）过程。

缩减（shrinking）过程实现以下三个重要的功能：

压缩：从代码中移除无用的类、段、方法等。

优化：使代码在指令级更小，更高效。

混淆：使用简短无意义的名称重命名代码里剩余的类，字段和方法。

R8 和当前的代码缩减解决方案 Proguard 相比，R8 可以更快地缩减代码，同时改善输出大小。

android gradle plugin 3.3.0需在项目的 gradle.properties 里加上：android.enableR8=true，在android gradle plugin 3.4.0开始默认使用R8。

### 参考
[Android 应用瘦身，从 18MB 到 12.5MB](https://www.diycode.cc/topics/586)
[[Android技术专题]APK瘦身看这一篇文章就够了](https://zhuanlan.zhihu.com/p/21962184?refer=zmywly8866)

[微信 安装包立减1M--微信Android资源混淆打包工具:直接处理安装包. 不依赖源码，不依赖编译过程，仅仅输入一个安装包，得到一个混淆包](https://mp.weixin.qq.com/s?__biz=MzA3NTYzODYzMg==&mid=214472913&idx=1&sn=92b54b5fcd9bbab6513e46d92095a07f&scene=1&srcid=0427eTI2x0dnk2EsFnysnjZI#rd)

[美团Android 资源混淆保护实践](http://tech.meituan.com/mt-android-resource-obfuscation.html)

[Android APK 瘦身 - JOOX Music项目实战](https://mp.weixin.qq.com/s/9IGYG6hNKL1V7N_p16p2Hg)


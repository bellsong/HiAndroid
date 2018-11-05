### 打包混淆问题引发的凌晨回公司

最近都在忙着adSdk接入头条的事情，先是CRC32校验问题，具体现象为在通过文件算CRC32的值过程中，会把文件给删除掉，这个目前还没找到原因，于是先用M9加密的方法。



白天头条接入也没反馈什么问题，应该可以正常发包，心里想着终于松一口气。今天晚上9点多下班，却不想快要到12点的时候产品经理来电，说正式发布的包拿不到广告，一直提示说miss Adapter。



跟那边的研发简单沟通后，了解到基本现象是

* 没混淆前的包可以正常拿广告
* 混淆后的包无法正常拿到广告

初步定位是混淆问题，打开电脑，定位到报错的位置，是用了Facebook的NativeAd的类。



梳理一下AdSdk的实现方案，看在哪个环节上出了问题。

宿主会先直接调用AdSdk的外壳接口，外壳里面会去先加载sdk.jar,加载成功后才执行广告逻辑。



#### 问题1：混淆的原理是什么？混淆过程会改变什么？

#### 问题2：现在的情况是，宿主引入了adsdk，还有作为动态加载的sdk.jar,sdk.jar输出时已经进行了混淆打包的时候，宿主引入后再次进行打包，这个过程会不会对作为资源的jar包进行二次混淆？



#### 检验是否混淆的方法：直接通过工具对最终的包进行反编译观察对应类是否被混淆。



通过尝试keep住Facebook的相关类测试，发现正式包也可以正常拿到广告。



所以，结论是通过二次混淆，找不到对应的sdk.jar中的类。



这里面涉及到要了解的东西


1. 混淆的原理
2. 混淆的使用 ：声明、保护代码写法 keep
3. 对于第三方资源的混淆，例如，引入的jar包
4. 对于动态加载的混淆

优化proguard规则，包括：

-optimizationpasses 5    ##提升压缩级别
-dontskipnonpubliclibraryclasses
-dontskipnonpubliclibraryclassmembers
-allowaccessmodification
-mergeinterfacesaggressively ##合并接口
-useuniqueclassmembernames
-dontpreverify #关闭预校验
-optimizations !code/simplification/arithmetic,!field/*,!class/merging/*




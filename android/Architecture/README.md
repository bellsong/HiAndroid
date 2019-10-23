# Android 架构

## 背景
业务快速迭代阶段，一般研发人员都是怎么快怎么来，赶着上线验证效果。但是随着用户体量的增加，当初不假思索留下的代码存在的设计缺陷，性能瓶颈逐一放大。最终导致使用卡顿，响应慢，甚至稳定性受到影响等用户负体验。出来混，始终要还。所以一个好的架构设计能够引领业务稳定向前发展。

个人觉得携程架构分享里面说的比较好的一段：

    架构是非常值得分享和讨论的，好的技术架构能够持续支持伟大的商业梦想。但是无论什么优秀的可扩展性好的技术架构，都不能脱离于业务而存在，最终都会随着业务的不断发展，而同时其架构也在进行不同程度的演进与优化。一个好的架构首先是必须是能解决公司遇到的现实技术问题和符合满足公司目前架构技术现状，其次能带来技术性的革新从而引领业务的发展。

    做架构之前，要想清楚这样设计的目的是什么，通过架构设计使程序模块化，做到模块内部的高聚合和模块之间的低耦合，做到基本符合迪米特、依赖倒置、里氏替换、接口隔离等原则。这样做的好处是使得程序在开发的过程中，开发人员只需要专注于一点，提高程序开发的效率，并且更容易进行后续的测试以及定位问题。但设计不能违背目的，对于不同量级的工程，具体架构的实现方式必然是不同的，切忌为了设计而设计，为了架构而架构。

新的Android体系架构指南（Android Architecture Guide ）定义了一些关键原则，一个优秀的Android应用程序应该符合这些原则，而且该指南为开发者创建优秀的APP提供了安全的路线。但是，该指南明确规定，所提出的路线不是强制性的，采用哪种类型的架构由开发者自己决定。

根据指南，优秀的Android APP应该提供一个牢固的关注点隔离(separation of concerns)，并且在model层驱动UI。 任何不处理UI或与操作系统交互的代码不应该在Activity 或Fragment中，因为保持Activity 和 Fragment尽可能干净，可以避免许多与生命周期相关的问题。 毕竟，系统可以随时销毁掉Activity 或 Fragment。此外，数据应该由与UI隔离和与生命周期隔离的model层来处理。

## 大型Android项目的工程化之路 
1. 项目架构
2. 模块化
3. 插件化
4. 热更新
5. 持续集成
6. 异常采集与分析
7. 编码规范
8. SDK设计
9. VCS工作流

## 框架设计原则

为了解决模块化的问题，我们要遵循以下原则去设计客户端框架：

根据基础技术层级、客户端的业务线等原则，对客户端应用程序进行模块化拆分。
每一个模块由独立的小团队或者个人来进行开发、维护、测试、集成。
模块与模块之间要做到彻底解耦，模块之间可以通过接口进行依赖。
每一个模块可以进行热插拔，单个模块的插拔不影响整体的工程的编译运行。

作者：蚂蚁金服移动开发平台mPaaS
链接：https://juejin.im/post/5bd81cf2f265da0ab674096c
来源：掘金
著作权归作者所有。商业转载请联系作者获得授权，非商业转载请注明出处。

[MVC，MVP 和 MVVM 的图示](http://www.ruanyifeng.com/blog/2015/02/mvcmvp_mvvm.html)

[Android 组件化](./android_component.md)

[Android Clean](./android_clean.md)

[Architecture 实践](https://github.com/googlesamples/android-architecture)

[MVP](http://www.jianshu.com/p/7567ed0d1853)

[MVVM](./MVVM.md)

### 数据处理

[Room Persistence Library](https://developer.android.com/reference/android/arch/persistence/room/package-summary)

## 参考

[Google 官方 MVP Sample 代码解读](https://juejin.im/entry/581067dcc4c9710058a86df5)

[Android 官方 MVP 架构示例项目解析](https://www.infoq.cn/article/android-official-mvp-architecture-sample-project-analysis)

[短视频类APP架构设计 美图](https://blog.csdn.net/dongtinghong/article/details/50766094)

[微信 Android 客户端架构演进之路](https://www.infoq.cn/article/wechat-android-app-architecture)

[微信 Android 模块化架构重构实践](https://juejin.im/entry/5955fb465188250d800799d0)

[支付宝客户端架构解析：Android 容器化框架初探](https://juejin.im/post/5bd81cf2f265da0ab674096c)

[ Android UI 组件开源软件](http://www.oschina.net/project/tag/342/android-ui?lang=0&os=0&sort=view)

[Android 高仿 频道管理----网易、今日头条、腾讯视频 （可以拖动的GridView）附源码DEMO](http://blog.csdn.net/vipzjyno1/article/details/25005851)

[Android应用架构的发展和实践](https://juejin.im/post/5c6656c9e51d451b08296d43)

[优秀架构师必须掌握的架构思维](https://www.infoq.cn/article/architecture-thought)

[安居客 Android App 走向平台化](https://juejin.im/post/5daefe9a51882568691fba2d?utm_source=gold_browser_extension)
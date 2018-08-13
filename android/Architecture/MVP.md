[MVP](http://antonioleiva.com/mvp-android/)

###MVP for Android: how to organize the presentation layer
by Antonio Leiva 

[The Clean Architecture](http://blog.8thlight.com/uncle-bob/2012/08/13/the-clean-architecture.html)

在架构设计过程中，通常还涉及到各种技术选型，这也是为了避免重复造轮子，更好地利用已有成果、提高开发效率。例如，业务层选用MVP还是MVVM架构？在哪些子模块使用？组件层，图像异步加载使用UIL还是Picasso？网络连接管理采用OkHttp还是Volley？诸如此类。

这部分涉及较多的开源项目、相关组件以及第三方服务商的优缺点分析，例如不同组件的性能、sdk文件体积、组件的配套使用等因素，后面继续探讨。

[XDroid 轻量级的Android快速开发框架](https://github.com/limedroid/XDroid/wiki)
XDroid是一个轻量级的Android快速开发框架，由UI、Cache、Event、ImageLoader、Kit、Log、Router、Net等几个部分组成。其设计思想是使用接口对各模块解耦规范化，不强依赖某些明确的三方类库，使得三方类库可自由搭配组装，方便替换。可快速、自由的进行App开发。

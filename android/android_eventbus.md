# EventBus

## What 什么是EventBus
---
EventBus是一个Android端优化的publish/subscribe消息总线，简化了应用程序内各组件间、组件与后台线程间的通信。比如请求网络，等网络返回时通过Handler或Broadcast通知UI，两个Fragment之间需要通过Listener通信，这些需求都可以通过EventBus实现。

    EventBus是一个Android事件发布/订阅的框架，主要目的是通过解耦发布者和订阅者简化Android事件传递。

    传统的事件传递方式包括： Handler、BroadcastReceiver、Interfac回调

    EventBus优点是代码简洁，使用简单，解耦

## Who EventBus框架

    GreenDao
    大家谈到EventBus，总会想到greenrobot的EventBus，但是实际上EventBus是一个通用的叫法，例如Google出品的Guava，Guava是一个庞大的库，EventBus只是它附带的一个小功能，因此实际项目中使用并不多。用的最多的是greenrobot/EventBus，这个库的优点是接口简洁，集成方便，但是限定了方法名，不支持注解。另一个库square/otto修改自 Guava ，用的人也不少。

    Subscriber

    Publisher

    ThreadMode

## When

## Where

## Why

## How 

```
    compile 'org.greenrobot:eventbus:3.0.0'
```


```
    EventBus.getDefault().register(this);
    EventBus.getDefault().unregister(this);
```
```
    @Subscribe(threadMode = ThreadMode.MainThread)
    public void helloEventBus(String message){
        mText.setText(message);
    }
```
```
    String json="";
    EventBus.getDefault().post(json);

```

* MainThread 主线程
* BackgroundThread 后台线程
* Async 后台线程
* PostThread 发送线程（默认）

### processor
按照Markus Junginger的说法（EventBus创作者），在3.0中，如果你想进一步提升你的app的性能，你需要添加：
```
    provided 'org.greenrobot:eventbus-annotation-processor:3.0.1'

```


## Good

## Bad

## Other

使用注解的好处和缺陷？


世界观
方法论

1、解决问题的方法和思路

## EventBus	

### What

​	[EventBus](https://github.com/greenrobot/EventBus)是一个Android端优化的publish/subscribe消息总线，简化了应用程序内各组件间、组件与后台线程间的通信。比如请求网络，等网络返回时通过Handler或Broadcast通知UI，两个Fragment之间需要通过Listener通信，这些需求都可以通过**EventBus**实现。

EventBus是有GreenDao公司提出的一个用于Android平台线程、进程间通信的库，使用方式简单粗暴，而且，库的大小不足50K，成本很低。



##### **缺陷**：

降低模块之间的耦合，处理不好带来降低耦合后混乱和困惑。

###### 嵌套事件

​	不要在事件订阅者里面发布事件，这看起来很容易避免，但是经常会不容易理解。一个订阅者很可能会通过调用其他的方法来间接地抛出其他的事件。这样事件会纠缠成一个复杂地难以置信的球，很难进行调试。

问题的根本在于生产者是否处理好一下两个方面：

- 要求把数据作为依赖。
- 处理好数据未获取到时情况。

在事件处理中组件可以把事件数据作为依赖，仅仅是作为是否可用的依赖。这需要一个随时可用的依赖构造函数或者是通过依赖注入注入数据（这个可以继续深入讨论）。

如果还没有请求到任何数据，组件需要处理好这种还未得到信息的情况（对于UI组件通常是处理等待加载的情况）。

[为什么你应该停止使用EventBus](https://github.com/hehonghui/android-tech-frontier/blob/master/issue-16/%E4%B8%BA%E4%BB%80%E4%B9%88%E4%BD%A0%E5%BA%94%E8%AF%A5%E5%81%9C%E6%AD%A2%E4%BD%BF%E7%94%A8EventBus.md)

EventBus在事件类型以及监听者的实现上考虑到了继承，但即便不考虑，也会有一个比较麻烦的地方——太多的EventBus会导致程序逻辑不清晰。举个栗子，原先Activity和Fragment之间通讯，可能是Fragment直接调用getActivity()后强转类型，然后调用某个方法，现在可能直接post一个事件，两端代码相比较，明显前者更轻易能看出Fragment在和谁、什么方法通信，再者，如果某个onEvent方法是出现在Activity的父类中，整体代码看上去就更加隐晦，可读性变差。

因此使用时注意适量，并适当添加注释



### Who

​	GreenRobert

### When

### Where

### Why

​	Android开发一直在说MVC模式，C这块在客户端App上已经不是很明显，M和V两块单独分离是比较简单的。M层本身也可以比较容易的进行架构组织，但是整体在拆的时候会出现两个问题：

1. V对M的变化监听实现比较复杂。在Android中，要么使用回调(需要声明接口，并添加监听)，要么使用广播(需要声明广播监听器，最重要的是传输的数据只能通过Intent，伴随的是声明一堆static final变量)，两种实现成本都比较高，重复工作很多；
2. V本身的拆分比较困难。许多复杂的页面，不可能将所有的View操作都写在同一个类中，因此会独立出一些Custom View(这个一般比较好处理)和Fragment，而Fragment之间的通讯是一件非常麻烦的事情：需要通过Activity中转(得判断Activity是否为空)或者广播进行。实际上，Activity之间的通讯也存在相似的问题。

这些问题的出现在呼唤一个东西的出现——一种更高效的代码间通讯方式。这就是使用EventBus的最佳理由。

EventBus可以使得在代码上看上去毫无关系(只有需要传送的信息对象)的两个对象之间能够完成信息传递，并且代码简短高效。

EventBus提供的一些高级特性：比如Priority(可以用于事件拦截)、Sticky(想了一下，如果A页面跳转到B页面，A页面同时又在等待B页面的信息，此时应该使用sticky事件，因为A页面可能会因为内存不足而被杀掉，使用Sticky则可以在A页面恢复的时候再度获取到事件)，可以为一些特殊场景提供便利。

### How

在使用方式上，EventBus3.0版本比2.4版本更加自由。

首先，在build.gradle中添加

```
 compile 'org.greenrobot:eventbus:3.0.0'
 
```

其次，之后，在你需要接受发送的消息的类的构造函数/onCreate方法里面添加

```
EventBus.getDefault().register(this);
```

当然，添加了注册，不要忘记反注册

```
 EventBus.getDefault().unregister(this);
```

之后，在这个类里面书写一个任意的public方法，参数就是你要接受的消息类型，

```
public void helloEventBus(String message){}
```

在这个方法上面添加 @Subscribe 注解，同时这里可以添加threadMode参数决定helloEventBus方法执行在什么线程上面，这里的参数分为

| 参数         | 含义                                       |
| ---------- | ---------------------------------------- |
| POSTING    | 与消息发送者在同一线程（默认）                          |
| MAIN       | 执行在主线程                                   |
| BACKGROUND | 执行在一个后台线程，会优先寻找发送者的线程，如果发送者执行在主线程，那么就新建一个后台线程 |
| ASYNC      | 执行在一个新的后台线程中                             |

在这里，同样支持添加优先级 priority 属性来决定在同种ThreadMode下面的处理任务的顺序。

最后，在调用的地方使用

```
EventBus.getDefault().post(message);
```

来发送消息。



观察者模式

Publish/Subscribers



[为什么要用EventBus？它是干什么用的？](http://www.jianshu.com/p/be963f80f309)

[EventBus使用](http://www.jianshu.com/p/be963f80f309)

[EventBus](http://yydcdut.com/2016/03/07/eventbus3-code-analyse/)



类似开源库

[AndroidEventBus](https://github.com/hehonghui/AndroidEventBus/blob/master/README-ch.md)

[如何用RxJava替代EventBus进行事件分发](http://blog.csdn.net/hzl9966/article/details/51280493)

[EventBus介绍](http://www.cnblogs.com/angeldevil/p/3715934.html)

[EventBusFor Android](https://greenrobot.github.io/EventBus/)

http://yydcdut.com/2016/03/07/eventbus3-code-analyse/

[老司机教你 “飙” EventBus 3](http://bugly.qq.com/bbs/forum.php?mod=viewthread&tid=938)



​	
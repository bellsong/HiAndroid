# Android Jobscheduler

#### API 25

#### JobScheduler 的由来
> Android 5.0系统以前，在处理一些特定情况下的任务，或者是为了应用的保活，我们通常是使用了Service常驻后台来满足我们的需求。当达到某个条件时触发该Service来进行相应任务的处理。或者仅仅是为了我们自己的应用不被系统回收销毁。这样做在满足了自己应用的需求的同时也消耗了部分硬件性能。对用户的体验上，和Android系统环境上都有不利的影响。而且在其它地方，大多数开发者都认为在后台永驻进程，是获取用户的隐私，是不合法的。然而在我国也许是因为开发商的需求大，迫切想要达到自己的目标，使用永驻的进程可以完成用户行为分析和推送等其它后台的业务。因此在开发的模式上采取了极端的个体主义思想。

>Android 5.0系统以后，Google为了优化Android系统，提高使用流畅度以及延长电池续航，加入了在应用后台/锁屏时，系统会回收应用，同时自动销毁应用拉起的Service的机制。同时为了满足在特定条件下需要执行某些任务的需求，google在全新一代操作系统上，采取了Job （jobservice & JobInfo）的方式，即每个需要后台的业务处理为一个job，通过系统管理job，来提高资源的利用率，从而提高性能，节省电源。这样又能满足APP开发商的要求，又能满足系统性能的要求。Jobscheduler由此应运而生。

作者：芥末末的沫
链接：https://www.jianshu.com/p/9fb882cae239
來源：简书
简书著作权归作者所有，任何形式的转载都请联系作者获得授权并注明出处。


#### JobScheduler 的特性

1. 支持在一个任务上组合多个条件
2. 内置条件：设备待机、充电和链接网络
3. 支持持续的 Job
    
    意味着设备重启后，之前被中断的 Job 可以继续执行

4. 支持设置 Job 的最后限期

#### JobScheduler 适用场景
1. 可以推迟的、不需要用户参与的任务，例如定期更新数据库数据
2. 插入设备时希望优先执行的任务，例如充电时进行数据备份
3. 需要访问网络或者 WIFI 链接时进行的任务，例如向服务端拉取内置数据
4. 需要作为一个批次定期运行的多任务

#### JobScheduler 的特征

1. Job Scheduler只有在Api21或以上的系统支持。
2. Job Scheduler是将多个任务打包在一个场景下执行。
3. 在系统重启以后，任务会依然保留在Job Scheduler当中，因此不需要监听系统启动状态重复设定。
4. 如果在一定期限内还没有满足特定执行所需情况，Job Scheduler会将这些任务加入队列，并且随后会进行执行。

使用Job Scheduler，应用需要做的事情就是判断哪些任务是不紧急的，可以交给Job Scheduler来处理，Job Scheduler集中处理收到的任务，选择合适的时间，合适的网络，再一起进行执行。把时效性不强的工作丢给它做。

#### JobScheduler 的使用

通俗的来讲，Jobscheduler也是通过Jobservice服务和JobInfo搭配来完成我们特定情况下需要完成的各种工作的一个新组件。和传统的service相比较，（从上边也可以了解到）它具有更优秀的表现，不仅对系统环境的友好，同时在特定的预置条件下进行我们期望的工作也节省了手机的电量，节约了系统资源。下边先了解下Jobservice和JobInfo。之后在完成搭配使用的用例以及应用衍生。

Jobscheduler的使用流程大概分为以下四个部分：

* 派生JobService 子类，定义需要执行的任务（UI线程）
* 从Context 中获取JobScheduler 实例（相当于管理器）
* 构建JobInfo 实例，指定 JobService任务实现类及其执行条件
* 通过JobScheduler 实例加入到任务队列

### 参考
1. [JobScheduler 的使用](https://www.jianshu.com/p/9fb882cae239)

2. [android 性能优化JobScheduler使用及源码分析](https://juejin.im/entry/5a45d979518825256362fcc0)

3. [Android 7.0之JobScheduler 分析](https://blog.csdn.net/u011311586/article/details/78290168)

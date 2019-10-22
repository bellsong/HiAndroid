# Android 多线程


1. Android线程调度原理剖析

线程调度原理：
任一时刻，只有一个线程占用CPU，处于运行状态

多线程并发：
轮流获取CPU使用权

JVM负责线程调度：
按照特定机制分配CPU使用权

线程调度模型：
分时调度模型：轮流获取、均分CPU时间
抢占式调度模型：优先级好的获取，JVM采用这种方式。
nice值、cgroup
nice值：
1）Process中定义
2）值越小，优先级越高
3）默认是THREAD_PRIORITY_DEFAULT,0
cgroup：
更严格的群组调度策略。
保证前台线程可以获取到更多的CPU
注意点：
线程过多会导致CPU频繁切换，降低线程运行效率
正确认识任务重要性决定哪种优先级。 （工作量越大优先级越低）

优先级具有继承性

2. Android异步方式汇总
最简单、常见的异步方式
不易复用，频繁创建及销毁开销大
复杂场景不易使用

HandlerThread
自带消息循环的线程
串行执行
长时执行，需要不断从队列中获取任务

IntentService
继承自Service在内部创建 HandlerThread
异步，不占用主线程
优先级较高，不易被系统Kill

AsyncTask
Android提供的工具类
无需自己处理线程切换
需要注意版本不一致问题

线程池
易复用，减少频繁创建、销毁的时间
功能强大： 定时、任务队列、并发数控制等

RxJava
由强大的Scheduler集合提供
不同类型的区分：IO、Computation
各种异步方式的线程优先级？写Demo

3. Android线程优化实战
线程使用准则：
1）严禁直接 new Thread
2）提供基础线程池供各个业务线使用
3）避免各个业务线各自维护一套线程池，导致线程数过多
4）根据任务类型选择合适的异步方式，优先级低，长时间执行，HandlerThread
5）创建线程必须命名！方便定位线程归属。运行期 Thread.currentThread().setName修改名字
6）关键异步任务监控 异步不等于不耗时。通过AOP的方式来做监控
7）重视优先级设置 Process.setThreadPriority();可以设置多次

4. 如何锁定线程创建者
项目变大后收敛线程
项目源码、三方库、aar中都有线程的创建
避免恶化的监控预防手段

分析：
创建线程的位置获取堆栈
所有异步方式都会走到 new Thread
适合采用 Hook 手段
找Hook点：构造函数或者特定方法
Thread的构造函数

5. 线程收敛优雅实践初步
根据线程创建堆栈考量合理性，使用统一线程库
各业务线下掉自己的线程库

基础库怎么使用线程？
基础库内部暴露API：setExecutor
初始化的时候注入统一的线程库
区分任务类型：IO、CPU密集型
IO密集型任务不消耗CPU，核心池可以很大。
CPU密集型任务：核心池大小和CPU核心数有关。

6. 线程优化问题
1）线程使用为什么会遇到问题？
项目早起阶段堆积功能。每个人使用线程的方式很乱
问题原因和表现形式。
壮大后遇到UI卡顿，异步任务耗时等。
Java线程调度原理引入。IO、CPU也没区分，可能出现UI线程抢不到时间片的问题。

2）怎么在项目中对线程进行优化？
线程数过多的情况做了线程收敛
Hook手段得到堆栈信息。看是否需要单独创建。
尽可能将线程收敛到基础库中。
向外暴露接口提供一个基础库使用的机会。
CPU和IO 密集型任务进行了区分。提高我们的执行效率。决定不同的核心池大小。

重要的异步逻辑进行监控。执行异步任务的时候，优先级和线程名的设置。

## 参考

[android多线程](http://www.eoeandroid.com/forum.php?mod=viewthread&tid=210116&_dsign=c9c8cdb9)

[Android多线程全面解析：IntentService用法&源码](http://www.jianshu.com/p/8a3c44a9173a)

[线程池运行原理分析](http://www.jianshu.com/p/edab547f2710)

[关于Android中为什么主线程不会因为Looper.loop()里的死循环卡死？引发的思考，事实可能不是一个 epoll 那么 简单。](http://www.cnblogs.com/linguanh/p/6412042.html)

[探索 Android 多线程优化方法](https://juejin.im/post/5d45a75de51d4561ee1bdf10)
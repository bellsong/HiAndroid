### AsyncTask

### 基本知识

* AsyncTask,是android提供的轻量级的异步类,可以直接继承AsyncTask,在类中实现异步操作,并提供接口反馈当前异步执行的程度(可以通过接口实现UI进度更新),最后反馈执行的结果给UI主线程。
* 3.0以前支持多个线程同步执行，3.0以后 AsyncTask 实际上是单线程执行。
* 最多执行128个任务，并等待10个，超过限制报异常。

#### 优点
* 简单，快捷
* 过程可控

#### 缺点
 1.生命周期

关于AsyncTask存在一个这样广泛的误解，很多人认为一个在Activity中的AsyncTask会随着Activity的销毁而销毁。然后事实并非如此。AsyncTask会一直执行doInBackground()方法直到方法执行结束。一旦上述方法结束，会依据情况进行不同的操作。
 •如果cancel(boolean)调用了，则执行onCancelled(Result)方法
 •如果cancel(boolean)没有调用，则执行onPostExecute(Result)方法
 
AsyncTask的cancel方法需要一个布尔值的参数，参数名为mayInterruptIfRunning,意思是如果正在执行是否可以打断,如果这个值设置为true，表示这个任务可以被打断，否则，正在执行的程序会继续执行直到完成。如果在doInBackground()方法中有一个循环操作，我们应该在循环中使用isCancelled()来判断，如果返回为true，我们应该避免执行后续无用的循环操作。

总之，我们使用AsyncTask需要确保AsyncTask正确地取消。

2.不好好工作的cancel()

简而言之的答案，有时候起作用。

如果你调用了AsyncTask的cancel(false)，doInBackground()仍然会执行到方法结束，只是不会去调用onPostExecute()方法。但是实际上这是让应用程序执行了没有意义的操作。那么是不是我们调用cancel(true)前面的问题就能解决呢？并非如此。如果mayInterruptIfRunning设置为true，会使任务尽早结束，但是如果的doInBackground()有不可打断的方法会失效，比如这个BitmapFactory.decodeStream() IO操作。但是你可以提前关闭IO流并捕获这样操作抛出的异常。但是这样会使得cancel()方法没有任何意义。

3.内存泄露

还有一种常见的情况就是，在Activity中使用非静态匿名内部AsyncTask类，由于Java内部类的特点，AsyncTask内部类会持有外部类的隐式引用。由于AsyncTask的生命周期可能比Activity的长，当Activity进行销毁AsyncTask还在执行时，由于AsyncTask持有Activity的引用，导致Activity对象无法回收，进而产生内存泄露。

4.结果丢失

另一个问题就是在屏幕旋转等造成Activity重新创建时AsyncTask数据丢失的问题。当Activity销毁并重新创建后，还在运行的AsyncTask会持有一个Activity的非法引用即之前的Activity实例。导致onPostExecute()没有任何作用。

5.串行还是并行

下面的两个任务时同时执行呢，还是AsyncTask1执行结束之后，AsyncTask2才能执行呢？实际上是结果依据API不同而不同。
关于AsyncTask时串行还是并行有很多疑问，这很正常，因为它经过多次的修改。如果你并不明白什么时串行还是并行，可以通过接下来的例子了解，假设我们在一个方法体里面有如下两行代码：

new AsyncTask1.execute();
new AsyncTask2.execute();
在1.6(Donut)之前:
在第一版的AsyncTask，任务是串行调度。一个任务执行完成另一个才能执行。由于串行执行任务，使用多个AsyncTask可能会带来有些问题。所以这并不是一个很好的处理异步（尤其是需要将结果作用于UI试图）操作的方法。

从1.6到2.3(Gingerbread)
后来Android团队决定让AsyncTask并行来解决1.6之前引起的问题，这个问题是解决了，新的问题又出现了。很多开发者实际上依赖于顺序执行的行为。于是很多并发的问题蜂拥而至。

3.0（Honeycomb）到现在
好吧，开发者可能并不喜欢让AsyncTask并行，于是Android团队又把AsyncTask改成了串行。当然这一次的修改并没有完全禁止AsyncTask并行。你可以通过设置executeOnExecutor(Executor)来实现多个AsyncTask并行。关于API文档的描述如下
If we want to make sure we have control over the execution, whether it will run serially or parallel, we can check at runtime with this code to make sure it runs parallel

6.真的需要AsyncTask么

并非如此，使用AsyncTask虽然可以以简短的代码实现异步操作，但是正如本文提到的，你需要让AsyncTask正常工作的话，需要注意很多条条框框。推荐的一种进行异步操作的技术就是使用Loaders。这个方法从Android 3.0 (Honeycomb)开始引入，在android支持包中也有包含。可以通过查看官方的文档来详细了解Loaders。

备注：此时用 Handler 更合适。在Handler 异步实现时,涉及到 Handler, Looper, Message,Thread四个对象，实现异步的流程是主线程启动Thread（子线程）àthread(子线程)运行并生成Message- àLooper获取Message并传递给HandleràHandler逐个获取Looper中的Message，并进行UI变更。

结构清晰，功能定义明确；对于多个后台任务时，简单，清晰

#### 内部线程池

AnsycTask执行任务时，内部会创建一个进程作用域的线程池来管理要运行的任务，也就就是说当你调用了AsyncTask.execute()后，AsyncTask会把任务交给线程池，由线程池来管理创建Thread和运行Therad。对于内部的线程池不同版本的Android的实现方式是不一样的：

3.0之前规定同一时刻能够运行的线程数为5个，线程池总大小为128。也就是说当我们启动了10个任务时，只有5个任务能够立刻执行，另外的5个任务则需要等待，当有一个任务执行完毕后，第6个任务才会启动，以此类推。而线程池中最大能存放的线程数是128个，当我们尝试去添加第129个任务时，程序就会崩溃。

因此在3.0版本中AsyncTask的改动还是挺大的，在3.0之前的AsyncTask可以同时有5个任务在执行，而3.0之后的AsyncTask同时只能有1个任务在执行。为什么升级之后可以同时执行的任务数反而变少了呢？这是因为更新后的AsyncTask已变得更加灵活，如果不想使用默认的线程池，还可以自由地进行配置。比如使用如下的代码来启动任务：

>Executor exec = new ThreadPoolExecutor(15, 200, 10, TimeUnit.SECONDS, new LinkedBlockingQueue<Runnable>());
new DownloadTask().executeOnExecutor(exec);

这样就可以使用我们自定义的一个Executor来执行任务，而不是使用SerialExecutor。上述代码的效果允许在同一时刻有15个任务正在执行，并且最多能够存储200个任务。


参考：

译文：Android中糟糕的AsyncTask - 技术小黑屋
http://droidyue.com/blog/2014/11/08/bad-smell-of-asynctask-in-android/

Android实战技巧：深入解析AsyncTask - 浪人的星空 - 博客频道 - CSDN.NET
http://blog.csdn.net/hitlion2008/article/details/7983449

Android AsyncTask完全解析，带你从源码的角度彻底理解 - 郭霖的专栏 - 博客频道 - CSDN.NET
http://blog.csdn.net/guolin_blog/article/details/11711405

Android7.0 AsyncTask机制
https://blog.csdn.net/fu_kevin0606/article/details/64920412

AsyncTask的原理及优缺点解析
https://www.jianshu.com/p/f6cf01ecac30






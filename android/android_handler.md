### 虎三说Handler

	Android UI操作并不是线程安全的并且这些操作必须在UI线程中执行。Android利用Handler来实现UI线程的更新的。

Handler的定义: 主要接受子线程发送的数据, 并用此数据配合主线程更新UI. 

解释: 当应用程序启动时，Android首先会开启一个主线程 (也就是UI线程) , 主线程为管理界面中的UI控件，进行事件分发, 比如说, 你要是点击一个 Button ,Android会分发事件到Button上，来响应你的操作。  如果此时需要一个耗时的操作，例如: 联网读取数据，或者读取本地较大的一个文件的时候，你不能把这些操作放在主线程中，，如果你放在主线程中的话，界面会出现假死现象,如果5秒钟还没有完成的话，会收到Android系统的一个错误提示 "强制关闭".这个时候我们需要把这些耗时的操作，放在一个子线程中,因为子线程涉及到UI更新，Android主线程是线程不安全的，也就是说，更新UI只能在主线程中更新，子线程中操作是危险的. 这个时候，Handler就出现了.来解决这个复杂的问题,由于Handler运行在主线程中(UI线程中),  它与子线程可以通过Message对象来传递数据, 这个时候，Handler就承担着接受子线程传过来的(子线程用sedMessage()方法传弟)Message对象，(里面包含数据),把这些消息放入主线程队列中，配合主线程进行更新UI。

Handler一些特点 
handler可以分发Message对象和Runnable对象到主线程中, 每个Handler实例,都会绑定到创建他的线程中(一般是位于主线程), 
它有两个作用:
(1)安排消息或Runnable 在某个主线程中某个地方执行
(2)安排一个动作在不同的线程中执行 
Handler中分发消息的一些方法 
post(Runnable) 
postAtTime(Runnable,long) 
postDelayed(Runnable long) 
sendEmptyMessage(int) 
sendMessage(Message) 
sendMessageAtTime(Message,long) 
sendMessageDelayed(Message,long) 
以上post类方法允许你排列一个Runnable对象到主线程队列中, 
sendMessage类方法, 允许你安排一个带数据的Message对象到队列中，等待更新.
 
补充别人总结的：------------------->
1、向哪个Handler 发送消息，就必须在哪个handler 里面接收；
2、直接使用JAVA 的 Thread 是无法更新Android UI的，因为Android View 在设计的时线程是不完全的，不过Android 提供了几种供开发者在线程中更新UI的方法，如下：
runOnUiThread( Runnable )
post( Runnable )
postDelayed( Runnable, long )
3、直接使用hanlder.post 等方法是在当前主线程里面做操作，而不是另外新建线程，建议使用Thread 线程直接新建另外一个线程或者使用HandlerThread类也可以。( 这句话的意思是ui线程是主线程，把一些耗时的操作放入其他线程做，主线程仅仅更新视图)
4、记住消息队列的先进先出原则。 
 
需要注意的：
一. Handler与Thread的区别。
	Handler与调用者处于同一线程，如果Handler里面做耗时的动作，调用者线程会阻塞。Android UI操作不是线程安全的，并且这些操作必须在UI线程中执行。Android提供了几种基本的可以在其他线程中处理UI操作的方案，包括Activity的runOnUiThread(Runnable),View的post以及1.5版本的工具类AsyncTask等方案都采用了Handler，Handler的post对线程的处理也不是真正start一个新的线程，而是直接调用了线程的run方法，这正是google煞费苦心搞一套Handler的用意。
 
二. Handler对于Message的处理不是并发的。
	一个Looper 只有处理完一条Message才会读取下一条，所以消息的处理是阻塞形式的。但是如果用不同的Looper则能达到并发的目的。Service中，onStart的执行也是阻塞的。如果一个startService在onStart执行完成之前，再次条用startService也会阻塞。如果希望能尽快的执行onStart则可以在onStart中使用handler,因为Message的send是非阻塞的。如果要是不同消息的处理也是并发的，则可以用不同的Looper实例化Handler。
 
三. 资源回收
	向Handler对象发送类似new Message ()形式的空Message可以达到清空Message的目的，这种做法与getLooper().quit()的做法是一样的。如果利用的资源较多，应及时清理。



[Handler源码分析 - Java层](http://www.jianshu.com/p/1bd6e015653f)
[Android 消息机制学习](http://www.jianshu.com/p/1e5640e6bef9#)

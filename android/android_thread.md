# Android 多线程

## 多线程的实现方式

### AsyncTask

### Handler & Handler Thread

### Loader

1. AsyncTaskLoader
2. CursorLoader

### IntentService

IntentService是Service的一个实现类。其内部实现包含了一个HandlerThread，当任务提交给IntentService时，会被加到队列并顺序处理。

```java

public class MyIntentService extends IntentService {
     public MyIntentService() {
       super("thread-name");
     }
     protected void onHandleIntent(Intent intent) {
       // executes on the background HandlerThread.
     } 
}

Intent intent = new Intent(context, MyIntentService.class);
intent.setData(uri); intent.putExtra("param", "some value"); 
startService(intent);
```

IntentService返回任务结果给Activity可以有下面几种方法（Service和Activity的通信方法）：

1. Activity启动IntentService时候，传递一个PendingIntent对象给IntentService，IntentService在处理完任何后调用PendingIntent.send()通知Activity，Activity在onActivityResult方法内处理收到的消息。
2. 提交一个系统通知，这样App不在前台，用户也可以知道任务完成，比如下载地图数据。
3. 使用Messenger发送一个消息给Activity里的Handler。
4. 使用广播方式把结果封装在Intent中广播出去， 这样Fragment或Activity都可以通过监听广播获得处理结果。

IntentService可以完全和Activity解耦。适合即使App主界面退出情况下也必须完成的任务，比如后台上传日志，图片之类的任务。

### Service

### AlarmManager

## 参考

[android多线程](http://www.eoeandroid.com/forum.php?mod=viewthread&tid=210116&_dsign=c9c8cdb9)

[Android多线程全面解析：IntentService用法&源码](http://www.jianshu.com/p/8a3c44a9173a)

[线程池运行原理分析](http://www.jianshu.com/p/edab547f2710)

[关于Android中为什么主线程不会因为Looper.loop()里的死循环卡死？引发的思考，事实可能不是一个 epoll 那么 简单。](http://www.cnblogs.com/linguanh/p/6412042.html)
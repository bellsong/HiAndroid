[Android进程保活的一般套路](http://www.jianshu.com/p/1da4541b70ad)

如何保证android进程的存活率，是开发android应用程序的同学都要遇到的问题，常见的方法有setforeground或是采取守护进程。
对于采取守护进程，做起来比麻烦，做法上比较流氓，而采用setforeground，似乎不可避免的会在通知栏显示一个通知。

so, 怎么办？

在介绍具体的方法之前，先科普下android中进程的优先级，已经了解的同学，可以直接跳到后面。

android的后台进程为什么会被杀？是因为存在系统中存在一个杀手，叫Low Memory Killer。这个杀手的任务是保证有足够的内存提供给前台程序，当系统内存低于一些阈值的时候，就是天黑请闭眼的时刻。

在lowmemorykiller.c中，有定义

```
static uint32_t lowmem_debug_level = 1;
static int lowmem_adj[6] = {
    0,
    1,
    6,
    12,
};
static int lowmem_adj_size = 4;
static int lowmem_minfree[6] = {
    3 * 512,    /* 6MB */
    2 * 1024,   /* 8MB */
    4 * 1024,   /* 16MB */
    16 * 1024,  /* 64MB */
};
static int lowmem_minfree_size = 4;
```

lowmem_minfree是杀进程的时机，谁被杀，则取决于lowmem_adj。
那这些0，1，6，12代表什么意思呢，在ProcessList.java，有一些定义

```
// Adjustment used in certain places where we don't know it yet.
    // (Generally this is something that is going to be cached, but we
    // don't know the exact value in the cached range to assign yet.)
    static final int UNKNOWN_ADJ = 16;

    // This is a process only hosting activities that are not visible,
    // so it can be killed without any disruption.
    static final int CACHED_APP_MAX_ADJ = 15;
    static final int CACHED_APP_MIN_ADJ = 9;

    // The B list of SERVICE_ADJ -- these are the old and decrepit
    // services that aren't as shiny and interesting as the ones in the A list.
    static final int SERVICE_B_ADJ = 8;

    // This is the process of the previous application that the user was in.
    // This process is kept above other things, because it is very common to
    // switch back to the previous app.  This is important both for recent
    // task switch (toggling between the two top recent apps) as well as normal
    // UI flow such as clicking on a URI in the e-mail app to view in the browser,
    // and then pressing back to return to e-mail.
    static final int PREVIOUS_APP_ADJ = 7;

    // This is a process holding the home application -- we want to try
    // avoiding killing it, even if it would normally be in the background,
    // because the user interacts with it so much.
    static final int HOME_APP_ADJ = 6;

    // This is a process holding an application service -- killing it will not
    // have much of an impact as far as the user is concerned.
    static final int SERVICE_ADJ = 5;

    // This is a process with a heavy-weight application.  It is in the
    // background, but we want to try to avoid killing it.  Value set in
    // system/rootdir/init.rc on startup.
    static final int HEAVY_WEIGHT_APP_ADJ = 4;

    // This is a process currently hosting a backup operation.  Killing it
    // is not entirely fatal but is generally a bad idea.
    static final int BACKUP_APP_ADJ = 3;

    // This is a process only hosting components that are perceptible to the
    // user, and we really want to avoid killing them, but they are not
    // immediately visible. An example is background music playback.
    static final int PERCEPTIBLE_APP_ADJ = 2;

    // This is a process only hosting activities that are visible to the
    // user, so we'd prefer they don't disappear.
    static final int VISIBLE_APP_ADJ = 1;

    // This is the process running the current foreground app.  We'd really
    // rather not kill it!
    static final int FOREGROUND_APP_ADJ = 0;

    // This is a process that the system or a persistent process has bound to,
    // and indicated it is important.
    static final int PERSISTENT_SERVICE_ADJ = -11;

    // This is a system persistent process, such as telephony.  Definitely
    // don't want to kill it, but doing so is not completely fatal.
    static final int PERSISTENT_PROC_ADJ = -12;

    // The system process runs at the default adjustment.
    static final int SYSTEM_ADJ = -16;

    // Special code for native processes that are not being managed by the system (so
    // don't have an oom adj assigned by the system).
    static final int NATIVE_ADJ = -17;
```

简单的说，数字越小，优先级越高，越不容易被杀死。
这里说几个标准，-16是系统进程，0是前台进程，这里的前台进程是指用户正在使用的Activity所在的进程，用户按Home键之后回到桌面时的优先级是6，普通Service的进程是8.

我们先来看下如何查看一个进程的oom_adj值。

用adb shell登录，比如，我们的进程名是com.tmall.rex.wannakillme，则用以下方法找到进程号PID。

```
ps | grep com.tmall.rex.wannakillme
```
比如进程号是13456
则使用命令
```
cat /proc/13456/oom_adj
```

正常情况下，如果应用程序在前台，这个结果是0，按HOME切后台后，变成6，按BACK后，变成8.

知道了如何检验成效后，我们来上黑科技代码。

假设AlwaysLiveService是需要长驻内存的服务，在其onStartCommend中写入以下方法：

```
@Override
   public int onStartCommand(Intent intent, int flags, int startId) {

       startForeground(R.id.notify, new Notification());
       startService(new Intent(this, TempService.class));
       return super.onStartCommand(intent, flags, startId);


   }
```

其中，TempSerivce为另一个短命的服务, 在这个短命的服务中同样调用startForeground，如下：

```
package com.tmall.rex.wannakillme;

public class TempService extends Service {


    @Nullable
    @Override
    public IBinder onBind(Intent intent) {
        return null;
    }


    @Override
    public int onStartCommand(Intent intent, int flags, int startId) {
        startForeground(R.id.notify, new Notification());
        stopSelf();
        return super.onStartCommand(intent, flags, startId);
    }

    @Override
    public void onDestroy() {
        stopForeground(true);
        super.onDestroy();
    }
}
```

有了这两个服务之后，你会惊奇的发现在后台之后oom_adj的值为1.其优先级则紧次于前台进程，又不会很流氓的霸占内存，又没有额外的守护进程开销，算是一种比较优雅的保活方式之一。

>参考资料：
>
[进程保活](http://www.atatech.org/articles/54730/?frm=mail_week)

> [Demo](.././code/demo_keep_process_alive)

[几种进程通信方式的对比总结](http://blog.csdn.net/u011240877/article/details/72863432)
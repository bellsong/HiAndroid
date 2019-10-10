# Android 异常处理

## Android 捕获异常的原理
    Android 的异常分为两种，Java层的异常和Native层的异常

### Java层异常捕获
    Java层默认通过调用Thread.setDafaultUncaughtException 注册一个UncaughtExceptionHandler来捕获异常，这个Handler可以抓到所有线程的未处理异常。

    ```java
    final Thread.UncaughtExceptionHandler oldHandler = Thread.getDefaultUncaughtExceptionHandler();
		Thread.setDefaultUncaughtExceptionHandler(new Thread.UncaughtExceptionHandler() {
			
			@Override
			public void uncaughtException(Thread thread, Throwable throwable) {
				// 崩溃前的处理
				// ...
				// 既可以收到Exception, 也可以收到Error

                /*调用默认处理，杀死进程*/
				oldHandler.uncaughtException(thread, throwable);
			}
		});

    ```

也可以注册针对指定线程的异常处理Handler，如

```java
    Thread.currentThread().setUncaughtExceptionHandler(new Thread.UncaughtExceptionHandler() {
					
					@Override
					public void uncaughtException(Thread thread, Throwable throwable) {
						// 崩溃前的处理
						// ...
						// 既可以收到Exception, 也可以收到Error
						if( /*这是一个致命错误*/ ) {
							//调用默认处理，杀死进程
							Thread.getDefaultUncaughtExceptionHandler().uncaughtException(thread, throwable);
						} else {
							// 什么也不处理的话， 当前线程被终止，但是整个进程可以继续运行
						}
					}
				});

```

### Native层异常
    Native层异常时通过信号来通知。要捕获native异常，需要注册信号回调。
方法如下：

```C
    static struct sigaction g_oldCSSigAction[8] = {0};
void CrashSaver_handleSignal(int sig, siginfo_t* info, void* context);

// 注册信号函数
void CrashSaver_install()
{
    // 保存信号的默认行为对象
    memset(g_oldCSSigAction, 0, sizeof(g_oldCSSigAction));
    sigaction(SIGTRAP, NULL, &g_oldCSSigAction[0]);
    sigaction(SIGABRT, NULL, &g_oldCSSigAction[1]);
    sigaction(SIGILL, NULL, &g_oldCSSigAction[2]);
    sigaction(SIGSEGV, NULL, &g_oldCSSigAction[3]);
    sigaction(SIGFPE, NULL, &g_oldCSSigAction[4]);
    sigaction(SIGBUS, NULL, &g_oldCSSigAction[5]);
    sigaction(SIGPIPE, NULL, &g_oldCSSigAction[6]);
    sigaction(SIGSYS, NULL, &g_oldCSSigAction[7]);
    
    // 创建 信号行为对象
    struct sigaction newSigAction;       
    sigemptyset(&newSigAction.sa_mask);
    newSigAction.sa_flags = SA_SIGINFO;
    /*设置信号处理函数*/
    newSigAction.sa_sigaction = CrashSaver_handleSignal;
    
    // 注册信号新的行为对象
    sigaction(SIGTRAP, &newSigAction, NULL);
    sigaction(SIGABRT, &newSigAction, NULL);
    sigaction(SIGILL, &newSigAction, NULL);
    sigaction(SIGSEGV, &newSigAction, NULL);
    sigaction(SIGFPE, &newSigAction, NULL);
    sigaction(SIGBUS, &newSigAction, NULL);
    sigaction(SIGPIPE, &newSigAction, NULL);
    sigaction(SIGSYS, &newSigAction, NULL);

}
```

回调处理函数：

```C
   void CrashSaver_handleSignal(int sig, siginfo_t* info, void* context)
{
	// sig  触发的信号ID   如 SIGABRT、SIGSEGV等
    // info  对此信号的描述信息
    // context 信号发生的上下文。比如各种寄存器信息。此结构和具体的CPU平台有关
    
    // 此处增加处理逻辑
    
    CrashSaver_uninstall(); /*反注册*/
    raise(signum); /*调用系统默认信号处理*/
} 
```

反注册函数

```C
    void CrashSaver_uninstall()
{
    sigaction(SIGTRAP, &g_oldCSSigAction[0], NULL);
    sigaction(SIGABRT, &g_oldCSSigAction[1], NULL);
    sigaction(SIGILL , &g_oldCSSigAction[2], NULL);
    sigaction(SIGSEGV, &g_oldCSSigAction[3], NULL);
    sigaction(SIGFPE, &g_oldCSSigAction[4], NULL);
    sigaction(SIGBUS, &g_oldCSSigAction[5], NULL);
    sigaction(SIGPIPE, &g_oldCSSigAction[6], NULL);
    sigaction(SIGSYS, &g_oldCSSigAction[7], NULL);
    memset(g_oldCSSigAction, 0, sizeof(g_oldCSSigAction));

}
```

## Crash SDK 
1. [Bugly](https://bugly.qq.com) 
    腾讯出品的SDK，对Crash搜集的体验非常赞，能搜集到JNI层的奔溃以及监控线上的ANR问题。

2. [Crashlytics](https://try.crashlytics.com/)
    国外的一个SDK，我自己没用过，但是用过的朋友对它的评价颇高。

3. [ARCA](https://github.com/ACRA/acra)
    一个开源的崩溃日志搜集器，轻松让你实现客户端的崩溃日志上传到后台，如果你不喜欢接入别人家的SDK，可以使用它。有一个不足之处，就是它搜集不到JNI层的奔溃。

4. [异常处理](http://geek.csdn.net/news/detail/50839)

## 参考

[android.os.BadParcelableException: ClassNotFoundException when unmarshalling](http://blog.csdn.net/lincyang/article/details/7095417)


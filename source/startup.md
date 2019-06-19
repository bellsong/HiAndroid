# Android 启动流程

1. init进程启动，内核启动之后启动的第一个用户级进程
2. 根据init.rc和init.xxx.rc建立几个基本服务，包括ServiceManager、Zygote
3. 启动Native服务，ServiceManager和Zygot两个是Android的基础，建立Java Runtime，建立虚拟机，进入Zygote服务框架（Zygote建立好了，利用Socket通讯，接收ActivityManangerService 的请求，Fork 应用程）
4. System Server，Android 服务启动：Zygote fork一个进程SystemServer，SystemServer建立Android要用到的服务并用init2建立一个线程
5. Home 启动：Home 就是在ActivityManagerService.systemReady() 通知的过程中建立的

Zygote是一个虚拟机进程，同时也是一个虚拟机实例的孵化器，每当系统要求执行一个Android应用程序，Zygote就会FORK出一个子进程来执行该应用程序。
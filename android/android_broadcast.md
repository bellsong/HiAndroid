[动态注册广播源码分析](http://my.oschina.net/winHerson/blog/90336)

### Android应用在未启动的情况下无法收到指定广播的问题总结

作者：张明云
链接：https://zhuanlan.zhihu.com/p/20933603
来源：知乎
著作权归作者所有。商业转载请联系作者获得授权，非商业转载请注明出处。

最近在做一个需求：程序没有桌面图标，安装后能够自动将其插件添加到Launcher上，也就是程序在未启动的情况下，能够完成一些操作。
能够想到的方案是在AndroidMainifest.xml中静态注册一个广播，监听系统的某些广播达到触发应用完成操作的目的，但现象是：程序安装后，在未启动的情况下无法接收到系统的广播；但在启动一次后，就能够正常收到系统广播。

通过查阅资料发现，这个问题只有在Android 3.1及以上的版本才会出现，我用的是4.2.2的版本测试，自然会有这个问题，具体原因是：从Android3.1开始，新安装的程序会被置于"stopped"状态，并且只有在至少手动启动这个程序一次后该程序才会改变状态，能够正常接收到指定的广播消息。Android这样做的目的是防止广播无意或者不必要地开启未启动的APP后台服务。

也就是说在Android3.1及以上的版本，在未启动的情况下通过应用自身完成一些操作是不可能的，但Android提供了一种借助其它应用发送指定Flag广播的方式，达到应用在未启动的情况下仍然能够收到消息的效果。

从Android 3.1开始，系统给Intent定义了两个新的Flag，分别为FLAG_INCLUDE_STOPPED_PACKAGES（表示包含未启动的App）和FLAG_EXCLUDE_STOPPED_PACKAGES（表示不包含未启动的App），用来控制Intent是否要对处于停止状态的App起作用，具体的操作方式如下：

1.在需要接收广播的应用中静态注册广播，并定义好action，并且需要指定android:exported="true"；

<receiver android:name=".receiver.UpdateWidgetReceiver"
     android:exported="true">
     <intent-filter>
          <action android:name="com.uperone.widget.action"/>
     </intent-filter>
</receiver>
2.在发送广播的应用中添加如下代码段：

Intent intent = new Intent();
intent.setAction("com.uperone.widget.action");
intent.addFlags(Intent.FLAG_INCLUDE_STOPPED_PACKAGES);
sendBroadcast(intent);


参考资料：

[App doesn't auto-start an app when booting the device in Android](http://stackoverflow.com/questions/31353411/app-doesnt-auto-start-an-app-when-booting-the-device-in-android)

# ANR


> 什么是ANR 如何避免它？

在Android 上，如果你的应用程序有一段时间响应不够灵敏，系统会向用户显示一个对话框，这个对话框称作应
用程序无响应（ANR：Application Not Responding）对话框。用户可以选择让程序继续运行，但是，他们在使用你的应用程序时，并不希望每次都要处理这个对话框。因此，在程序里对响应性能的设计很重要，这样，系统不会显示ANR 给用户。
不同的组件发生ANR 的时间不一样，主线程（Activity、Service）是 5 秒，BroadCastReceiver 是 10 秒。

**解决方案：**
将所有耗时操作，比如访问网络，Socket 通信，查询大量SQL 语句，复杂逻辑计算等都放在子线程中去，然后通过handler.sendMessage、runonUITread、AsyncTask 等方式更新UI。无论如何都要确保用户界面操作的流畅度。
如果耗时操作需要让用户等待，那么可以在界面上显示进度条。
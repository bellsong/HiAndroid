# Android SharePreference 了解

## 简介
    Android平台里面一个轻量级的存储库，用来保存应用程序的各种配置信息。本质上是一个以“Key-Value”键值对的方式保存数据的xml文件

    保存地址：/data/data/package name/shared_prefs目录下

## 使用

```java
        //获取得到SharePreference，第一个参数是文件名称，第二个参数是操作模式
        //一般是MODE_PRIVATE模式，指定该SharedPreferences数据只能被本应用程序读、写
        SharedPreferences sharedPreferences = getSharedPreferences("demo_text", MODE_PRIVATE);
        //创建Editor对象
        SharedPreferences.Editor editor = sharedPreferences.edit();
        //保存数据
        editor.putString("name","bell");
        //editor.commit();
        editor.apply();
        //读取数据
        String result=sharedPreferences.getString("name","default");

```

## 进阶

1. SharePreference适用场景，跟数据库有什么区别，有什么优缺点？

2. apply和commit的区别

    * commit: 同步提交,commit将同步数据写入磁盘和内存缓存，并且有返回值
    * apply: 异步提交，会把数据同步写入内存缓存，然后异步保存到磁盘，可能会失败，不会收到错误回调。

    区别：

    * 都是原子性操作，但是原子操作不同。
    * commit从数据提交到内存到磁盘。
    * apply将数据提交到内存就返回了， 再异步写进磁盘

2. commit效率会比apply慢，因为要写入磁盘。同一个进程中，优先考虑apply。 

3. 性能优化

4.  多进程使用场景
    单进程中的SharePreference，读写操作都是从内存的Map中读取（获取sp的时候已经从磁盘加载到内存），不涉及IO操作。多进程场景下内存不共享，这时读写SharePreference就会有问题，会出现数据不一致情况。

    解决方案：可以通过ContentProvider来处理多进程间的文件共享。

ContentProvider的特点：

* ContentProvider内部的同步机制会防止多个进程同时访问，避免数据冲突。
* ContentProvider的数据源，可以选择数据库或者文件，核心操作就在update()和query()，里面操作存取的数据源可以需要替换成文件，也可以换成SharedPreferences。

所以可以使用ContentProvider做中间媒介，让它帮我们实现多进程同步机制，里面操作的数据改成SharedPreferences来实现。这样的话就可以实现了跨进程访问SharePreference。

优化一下的思路：

在ContentProvider里面加一个HashMap<String,SharePreference>进行一下缓存，key值是文件名，value是对应的SharePreference对象，这样不用每次都去加载SharePreference对象了。
    在ContentProvider里面实现回调listener，在key值有变化的时候，进行通知订阅者。

## 源码分析


## 正确使用姿势

1. 不要存放大的key和value在sharepreferences中，否则会一直存储在内存中得不到释放，内存使用过高会频繁引发GC，导致界面丢帧甚至ANR

2. 不相关的配置最好不要放在一起，单个文件越大读取速度越慢

3. 读取频繁的key和不频繁的key尽量不要放在一起

4. 不要每次都edit，因为每次都会创建一个新的EditorImpl对象，最好是批量处理统一提交。否则edit().commit每次创建一个EditorImpl对象并且进行一次IO操作，严重影响性能。

5. commit发生在UI线程中，apply发生在工作线程中，对于数据的提交最好是批量操作统一提交。虽然apply发生在工作线程（不会因为IO阻塞UI线程）但是如果添加任务较多也有可能带来其他严重后果（参照ActivityThread源码中handleStopActivity方法实现

6. 尽量不要存放json和html，这种可以直接文件缓存

7. 不要指望这货能够跨进程通信 Context.PROCESS

8. 最好提前初始化SharedPreferences，避免SharedPreferences第一次创建时读取文件线程未结束而出现等待情况

## 参考

1. [SharePreference源码学习和多进程的场景](https://juejin.im/post/5cc2de52e51d456e2e656db1?utm_source=gold_browser_extension)

2. 微信mmkv 可以替代；用c++实现，速度更快； 支持多进程

3. [SharedPreferences 多进程解决方案](https://juejin.im/entry/590833711b69e60058eb34b9/)

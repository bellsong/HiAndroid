### Monkey 

一、Monkey 是什么？

Monkey测试是Android自动化测试的一种手段。该工具用于进行压力测试，就是模拟用户的按键输入，触摸屏输入，手势输入等，看设备多长时间会出异常。 然后开发人员结合monkey 打印的日志 和系统打印的日志，结局测试中出现的问题。

二、Monkey命令

1）. 标准的monkey 命令 
[adb shell] monkey [options] < eventcount > , 例如：

> adb shell monkey -v 500

产生500次随机事件，作用在系统中所有activity（其实也不是所有的activity，而是包含 Intent.CATEGORY_LAUNCHER 或Intent.CATEGORY_MONKEY 的activity）。 
上面只是一个简单的例子，实际情况中通常会有很多的options 选项.

2）. 四大类

* 常用选项
* 事件选项
* 约束选项
* 调试选项

具体的命令解释可以看这里：android 压力测试命令monkey详解

一个简单的Monkey命令如下：

>adb shell monkey -p com.example.xystudy -s 500 -v 10000

工作中为了保证测试数量的完整进行，我们一般不会在发生错误时立刻退出压力测试。monkey 测试命令如下：

/**
 * monkey 作用的包：com.ckt.android.junit
 * 产生时间序列的种子值：500
 * 忽略程序崩溃、 忽略超时、 监视本地程序崩溃、 详细信息级别为2， 产生10000个事件 。
 */
adb shell monkey -p com.xy.android.junit -s 500 --ignore-crashes
--ignore-timeouts --monitor-native-crashes -v -v 10000 > E:\monkey_log\java_monkey_log.txt


三、强制停止Monkey测试

方法一：
> adb shell ps | awk '/com\.android\.commands\.monkey/ { system("adb shell kill " $2) }'  

方法二：

1. adb shell

2. top | grep monkey

显示如下：

top | grep monkey
 5447  0   1% S    10 262960K  10328K     root     com.android.commands.monkey
 5447  0   0% S    10 262960K  10324K     root     com.android.commands.monkey

找到id为5447，然后再kill掉就OK了

3. adb shell

4. kill -9 5447





上上周在robotium.cn官方群里，聊到了monkey崩溃日志的知识，做了一下笔记，分享出来

android存异常日志的地方：

/data/system/dropbox
/data/dontpanic
/err
/data/tomstone
data/anr
dump&log

 monkey crash 有7种 
* FC ANR WatchDog 属于应用层面的crash
* tombstone c
* c/c++ 属于native层 
* modem 属于cp层
* apanic 属于内核层

前面2种是最常见的
tombstone 分为2种 一种为应用错误 一种为系统错误 需要具体定位分析
monkey测试除了以上几个文件夹需要拷贝
其实还有一个monkeyscreenlog
以及dumpsys.log
还有一个bugreport
这些也需要拷贝出来分析的
FC和ANR的错误可以直接看/data/system/dropbox里面的文件 有对应的关键字 例如 anr 
dropbox里面event_data 就只有 start= XXXXX end=XXXXX

https://nimbledroid.com/

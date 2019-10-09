### Andorid 内存监控


### Linux内存基础

Android 内存是如何统计的？Android系统基于Linux内核，而Linux中内存统计两个重要文件是

1. /proc/meminfo 每个字段的含义可见 https://www.kernel.org/doc/Documentation/filesystems/proc.txt

2. /proc/[pid]/smaps

#### 内存明细

1. /proc/meminfo 每个字段的含义可见 https://www.kernel.org/doc/Documentation/filesystems/proc.txt

```
MemTotal:        1009388 kB 总内存，物理内存减去系统预留内存和内核二进制代码
MemFree:           57936 kB 空闲内存 
MemAvailable:     441184 kB 可用内存
Buffers:           24208 kB 磁盘数据缓存
Cached:           375852 kB 文件数据缓存，一个文件不再跟进程关联后，原来在page cache的页面并不回收，被记入Cached
SwapCached:        20052 kB
Active:           383668 kB 最近被访问过的内存页
Inactive:         283188 kB 长时间未被访问过的内存页
Active(anon):     109180 kB 活跃的anonymous pages（匿名页）
Inactive(anon):   162104 kB 不活跃的anonymous pages
Active(file):     274488 kB 活跃的file-backed pages （与文件对应的内存页）
Inactive(file):   121084 kB 不活跃的file-backed pages（与文件对应的内存页）
Unevictable:        2016 kB 
Mlocked:             332 kB
HighTotal:        498432 kB
HighFree:          18684 kB
LowTotal:         510956 kB
LowFree:           39252 kB
SwapTotal:        403748 kB
SwapFree:         338904 kB
Dirty:                40 kB
Writeback:             0 kB
AnonPages:        267724 kB 用户进程的内存页分两种：file-backed pages(与文件对应的内存页) 和 anonymous pages（匿名页，主要是堆和栈内存）
Mapped:           204736 kB
Shmem:              2804 kB
Slab:              55996 kB
SReclaimable:      23144 kB
SUnreclaim:        32852 kB
KernelStack:        8840 kB
PageTables:        18192 kB
NFS_Unstable:          0 kB
Bounce:                0 kB
WritebackTmp:          0 kB
CommitLimit:      908440 kB
Committed_AS:   18203240 kB
VmallocTotal:     499712 kB
VmallocUsed:           0 kB
VmallocChunk:          0 kB 

```

2. /proc/[pid]/smaps

linux 通过proc文件每一个进程都提供一个smaps文件，里面的每一条记录表示进程虚拟内存空间中一块连续的区域。
从第一行左边到右边意识表示地址范围、权限标识、映射文件偏移，设备号，inode，文件路径。

* Size： 表示该映射区在虚拟内存空间中的大小
* Rss： 表示该映射区当前在物理内存中占用空间大小
* Pss： 表示虚拟内存区域按比例平摊计算后的物理内存大小（有些内存会和其他进程共享，例如mmap），将smaps文件中所有的Pss字段加起来就是这个进程占用总的Pss内存
* Shared_Clean：和其他进程共享的未被改写page的大小
* Shared_Dirty：和其他进程共享的被改写的page的大小，通过copy-on-write的方式，共享的堆栈，例如Zyote堆内存与其fork出来的其他APP进程间共享内存
* Private_Clean:未被改写的私有页面大小
* Private_Dirty:已被改写的私有页面大小

### Android 内存基础

  https://developer.android.com/topic/performance/memory-overview

Android上查看系统内存占用情况

  adb dumpsys meminfo -a 

### 分析工具

1. Java层 AndroidStudio的Memory Profiler
2. Native堆内存分析工具   

  https://source.android.com/devices/tech/debug/native-memory

### 数据收集 
  
  需要关注的内存指标 PSS，系统剩余内存，已用内存



| Name        | Full Name    |  含义  | 等价	|
| --------   | -----:   | :----: |:----:|
| USS        | Unique Set Size      |   物理内存    | 进程独占的内存
| PSS        | Proportional Set Size      |   物理内存    | PSS = USS + 按比例包含共享库
| RSS        | Resident Size      |   物理内存    | RSS = USS + 包含共享库
| VSS	| Virtual | 虚拟内存 | VSS = RSS + 未分配实际物理内存

> 内存大小关系 USS <= PSS <= RSS <= VSS

###  内存的查看方法
1. dumpsys meminfo
2. procrank
3. cat /proc/meminfo
4. free
5. showmap
6. vmstat
7. adb shell kill -QUIT PID 生成anr traces文件,名称一般是trace_xx，在data/anr目录下，包含了GC的统计信息

#### 详细介绍
1. dumpsys meminfo
dumpsys meminfo 输出结果包括以下部分

| NO.        | 划分类型    |  排序  | 说明	|
| --------   | -----:   | :----: |:----:|
| 	1        |process     |   PSS    | 进程的PSS从大到小排序显示，每行显示一个进程
| 2       | OOM adj      |   PSS    | Native/System/Persistent/Foreground/Visible/Perceptible/A Services/Home/B Services/Cached，分别显示每类的进程情况
| 3        | category      |   PSS    | 以Dalvik/Native/.art mmap/.dex map等划分的各类进程的总PSS情况
| 4	| total | - | 总内存、剩余内存、可用内存、其他内存

```  
Total PSS by process: //以process来划分
   167128 kB: com.android.systemui (pid 4395)
   124527 kB: system (pid 1192)
    44213 kB: com.android.settings (pid 29256 / activities)
    41822 kB: surfaceflinger (pid 391)
    ...

Total PSS by OOM adjustment: //以oom来划分，会详细列举所有的类别的进程，此处省略.
   183683 kB: Native
        42024 kB: surfaceflinger (pid 388)
        16740 kB: mediaserver (pid 471)
        16040 kB: zygote (pid 494)
        ...
   124527 kB: System
   344259 kB: Persistent
    69719 kB: Foreground
    49026 kB: Visible
    34005 kB: Perceptible
     7880 kB: A Services
    58689 kB: Home
    98352 kB: B Services
    94888 kB: Cached

Total PSS by category:  // 以category划分
   309449 kB: Dalvik
   230330 kB: Native
   145344 kB: EGL mtrack
   117797 kB: .so mmap
    54389 kB: .art mmap
    44886 kB: .dex mmap
    32428 kB: Dalvik Other
    31083 kB: .oat mmap
    29456 kB: Stack
    21782 kB: Gfx dev
    21733 kB: Unknown
    12695 kB: .apk mmap
     9367 kB: Other mmap
     2169 kB: .ttf mmap
     2062 kB: Other dev
       38 kB: .jar mmap
       12 kB: Ashmem
        8 kB: Cursor
        0 kB: GL mtrack
        0 kB: Other mtrack

//整体情况
Total RAM: 2857032 kB (status moderate)
 Free RAM: 1439488 kB (94888 cached pss + 344620 cached kernel + 999980 free)
 Used RAM: 1280552 kB (970140 used pss + 310412 kernel)
 Lost RAM: 136992 kB
     ZRAM: 4 kB physical used for 0 kB in swap (524284 kB total swap)
   Tuning: 256 (large 512), oom 525000 kB, restore limit 175000 kB (high-end-gfx)
```

另外，可以输出某个pid或者package的进程信息

```
dumpsys meminfo <pid> // 输出指定pid的某一进程

dumpsys meminfo --package <packagename> // 输出指定包名的进程，可能包含多个进程
```

2. procrank

功能： 获取所有进程的内存使用的排行榜，排行是以Pss的大小而排序。procrank命令比dumpsys meminfo命令，能输出更详细的VSS/RSS/PSS/USS内存指标。

最后一行输出下面6个指标：

total	free	buffers	cached	shmem	slab

```
YdeMacBook-Pro:launcher Y$ adb shell procrank
  PID       Vss      Rss      Pss      Uss  cmdline
 4395  2270020K  202312K  136099K  121964K  com.android.systemui
 1192  2280404K  147048K   89883K   84144K  system_server
29256  2145676K   97880K   44328K   40676K  com.android.settings
  501  1458332K   61876K   23609K    9736K  zygote
 4239  2105784K   68056K   21665K   19592K  com.android.phone
  479   164392K   24068K   17970K   15364K  /system/bin/mediaserver
  391   200892K   27272K   15930K   11664K  /system/bin/surfaceflinger
...
RAM: 2857032K total, 998088K free, 78060K buffers, 459780K cached, 312K shmem, 92392K slab

```

3. cat /proc/meminfo

功能：能否显示更加详细的内存信息

输出结果如下(结果内存值不带小数点，此处添加小数点的目的是为了便于比对大小)：

```
YdeMacBook-Pro:launcher Y$ adb shell cat /proc/meminfo
MemTotal:        2857.032 kB  //RAM可用的总大小 (即物理总内存减去系统预留和内核二进制代码大小)
MemFree:         1020.708 kB  //RAM未使用的大小
Buffers:           75.104 kB  //用于文件缓冲
Cached:           448.244 kB  //用于高速缓存
SwapCached:             0 kB  //用于swap缓存

Active:           832.900 kB  //活跃使用状态，记录最近使用过的内存，通常不回收用于其它目的
Inactive:         391.128 kB  //非活跃使用状态，记录最近并没有使用过的内存，能够被回收用于其他目的
Active(anon):     700.744 kB  //Active = Active(anon) + Active(file)
Inactive(anon):       228 kB  //Inactive = Inactive(anon) + Inactive(file)
Active(file):     132.156 kB
Inactive(file):   390.900 kB

Unevictable:            0 kB
Mlocked:                0 kB

SwapTotal:        524.284 kB  //swap总大小
SwapFree:         524.284 kB  //swap可用大小
Dirty:                  0 kB  //等待往磁盘回写的大小
Writeback:              0 kB  //正在往磁盘回写的大小

AnonPages:        700.700 kB  //匿名页，用户空间的页表，没有对应的文件
Mapped:           187.096 kB  //文件通过mmap分配的内存，用于map设备、文件或者库
Shmem:               .312 kB

Slab:              91.276 kB  //kernel数据结构的缓存大小，Slab=SReclaimable+SUnreclaim
SReclaimable:      32.484 kB  //可回收的slab的大小
SUnreclaim:        58.792 kB  //不可回收slab的大小

KernelStack:       25.024 kB
PageTables:        23.752 kB  //以最低的页表级
NFS_Unstable:           0 kB  //不稳定页表的大小
Bounce:                 0 kB
WritebackTmp:           0 kB
CommitLimit:     1952.800 kB
Committed_AS:   82204.348 kB   //评估完成的工作量，代表最糟糕case下的值，该值也包含swap内存

VmallocTotal:  251658.176 kB  //总分配的虚拟地址空间
VmallocUsed:      166.648 kB  //已使用的虚拟地址空间
VmallocChunk:  251398.700 kB  //虚拟地址空间可用的最大连续内存块
```

对于cache和buffer也是系统可以使用的内存。所以系统总的可用内存为 MemFree+Buffers+Cached

4. free

主功能：查看可用内存，缺省单位KB。该命令比较简单、轻量，专注于查看剩余内存情况。数据来源于/proc/meminfo。

```
YdeMacBook-Pro:launcher Y$ adb shell free
		total        used        free      shared     buffers
Mem:       1024610304   978239488    46370816     1167360    21381120
-/+ buffers/cache:      956858368    67751936
Swap:       768450560   163958784   604491776
```

对于Mem行，存在的公式关系： total = used + free;

对于-/+ buffers行： 956858368 = 978239488 - 21381120(buffers); 67751936 = 46370816 + 21381120(buffers);


5. showmap

主要功能：用于查看虚拟地址区域的内存情况

用法：  showmap -a [pid]

该命令的输出每一行代表一个虚拟地址区域(vm area)

```
root@phone:/ # showmap -a 10901
   start    end      virtual                   shared   shared  private  private
    addr     addr     size      RSS      PSS    clean    dirty    clean    dirty object
-------- -------- -------- -------- -------- -------- -------- -------- -------- ------------------------------
f3b87000 f3d85000     2040        4        4        0        0        4        0 /dev/binder
```

start addr和end addr:分别代表进程空间的起止虚拟地址；

virtual size/ RSS /PSS这些前面介绍过；

shared clean：代表多个进程的虚拟地址可指向这块物理空间，即有多少个进程共享这个库；

shared: 共享数据

private: 该进程私有数据

clean: 干净数据，是指该内存数据与disk数据一致，当内存紧张时，可直接释放内存，不需要回写到disk

dirty: 脏数据，与disk数据不一致，需要先回写到disk，才能被释放。

功能与cat /proc/[pid]/maps基本一致

6. vmstat

主功能：不仅可以查看内存情况，还可以查看进程运行队列、系统切换、CPU时间占比等情况，另外该指令还是周期性地动态输出。

用法：

Usage: vmstat [ -n iterations ] [ -d delay ] [ -r header_repeat ]
    -n iterations     数据循环输出的次数
    -d delay          两次数据间的延迟时长(单位：S)
    -r header_repeat  循环多少次，再输出一次头信息行

```
root@phone:/ # vmstat
procs  memory                       system          cpu
 r  b   free  mapped   anon   slab    in   cs  flt  us ni sy id wa ir
 2  0  663436 232836 915192 113960   196  274    0   8  0  2 99  0  0
 0  0  663444 232836 915108 113960   180  260    0   7  0  3 99  0  0
 0  0  663476 232836 915216 113960   154  224    0   2  0  5 99  0  0
 1  0  663132 232836 915304 113960   179  259    0  11  0  3 99  0  0
 2  0  663124 232836 915096 113960   110  175    0   4  0  3 99  0  0
```

参数列总共15个参数，分为4大类：

* procs(进程)

	* r: Running队列中进程数量
	
	* b: IO wait的进程数量

* memory(内存)
	
	* free: 可用内存大小
	
	* mapped：mmap映射的内存大小
	
	* anon: 匿名内存大小
	
	* slab: slab的内存大小

* system(系统)
	
	* in: 每秒的中断次数(包括时钟中断)
	
	* cs: 每秒上下文切换的次数

* cpu(处理器)
	
	* us: user time
	
	* ni: nice time
	
	* sy: system time
	
	* id: idle time
	
	* wa: iowait time
	
	* ir: interrupt time

#### 小结
dumpsys meminfo适用场景： 查看进程的oom adj，或者dalvik/native等区域内存情况，或者某个进程或apk的内存情况，功能非常强大；

procrank适用场景： 查看进程的VSS/RSS/PSS/USS各个内存指标；

cat /proc/meminfo适用场景： 查看系统的详尽内存信息，包含内核情况；

free适用场景： 只查看系统的可用内存；

showmap适用场景： 查看进程的虚拟地址空间的内存分配情况；

vmstat适用场景： 周期性地打印出进程运行队列、系统切换、CPU时间占比等情况；

### 运行时内存

```java
private void getMemoryInfo() {
    ActivityManager activityManager = (ActivityManager) this.getSystemService(ACTIVITY_SERVICE);
    ActivityManager.MemoryInfo memoryInfo = new ActivityManager.MemoryInfo();
    activityManager.getMemoryInfo(memoryInfo);
    LogUtil.d("totalMem=" + memoryInfo.totalMem + ",availMem=" + memoryInfo.availMem);
    if (!memoryInfo.lowMemory) {
        // 运行在低内存环境
    }
}
```

### 参考

[Android内存分析命令](http://gityuan.com/2016/01/02/memory-analysis-command/)
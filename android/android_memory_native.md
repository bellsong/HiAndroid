# Android Native内存检测

## Android Native内存检测方法

1. adb shell dumpsys meminfo 进程名/进程号 -d

** MEMINFO in pid 1043 [xxxxx] **
                   Pss  Private  Private  SwapPss     Heap     Heap     Heap
                 Total    Dirty    Clean    Dirty     Size    Alloc     Free
                ------   ------   ------   ------   ------   ------   ------
  Native Heap    33079    31404     1532    10652    51200    42277     8920
  Dalvik Heap    17068    17020       20      304    17845    11701     6144
 Dalvik Other     7899     7604      292       98
        Stack       36       36        0       12
       Ashmem        2        0        0        0
    Other dev       30        0       20        0
     .so mmap     4000      184     1008      193
    .apk mmap     1126       12        0       32
    .ttf mmap      675        0      644        0
    .dex mmap    14314       36     5380       64
    .oat mmap     1883        0      324        0
    .art mmap    15003    12192     2444      441
   Other mmap     5970       12     5120       20
      Unknown     2076     1904      172     4072
        TOTAL   119049    70404    16956    15888    69045    53978    15064

里面的Native Heap就是Native内存。

2. 

## 参考

[Android Native内存泄漏诊断](https://blog.csdn.net/yellowcath/article/details/78085419)

[性能优化工具（十）- Android内存分析命令](https://www.jianshu.com/p/9edfe9d5eb34)


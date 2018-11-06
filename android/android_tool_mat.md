# MAT 使用指南

### 概述

MAT(Memory Analyzer Tool)，一个基于Eclipse的内存分析工具，是一个快速、功能丰富的JAVA heap分析工具，它可以帮助我们查找内存泄漏和减少内存消耗。使用内存分析工具从众多的对象中进行分析，快速的计算出在内存中对象的占用大小，看看是谁阻止了垃圾收集器的回收工作，并可以通过报表直观的查看到可能造成这种结果的对象。

### 使用方式
1. 下载MAT
2. 从Android Studio dump出来的 hprof 文件，如果要在 MAT 中打开，需要使用Android sdk提供的 hprof-conv 工具对文件进行转换：

		hprof-conv input.hprof out.hprof

然后通过 MAT 导入hprof文件即可

### 参考

[MAT使用指南](http://androidperformance.com/2015/04/11/AndroidMemory-Usage-Of-MAT.html)

[通过MAT查看内存占用](https://blog.csdn.net/xiaanming/article/details/42396507)

[MAT使用指南](http://androidperformance.com/2015/04/11/AndroidMemory-Usage-Of-MAT.html)

[MAT使用指南](http://www.importnew.com/2433.html)

[MAT使用进阶](https://www.androidperformance.com/2015/04/11/AndroidMemory-Usage-Of-MAT-Pro/)

[Android 性能优化必知必会](https://www.androidperformance.com/2018/05/07/Android-performance-optimization-skills-and-tools/)
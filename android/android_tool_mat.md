

[通过MAT查看内存占用](https://blog.csdn.net/xiaanming/article/details/42396507)


**5. 更全面的分析内存泄漏问题**

有时候通过Leakcanary可以方便的找出内存泄漏的嫌疑点，但有可能分析的结果不太好排查内存泄漏问题，这时我们可以使用更高级的工具来分析原始的hprof文件。

MAT [MAT使用指南](http://androidperformance.com/2015/04/11/AndroidMemory-Usage-Of-MAT.html)

MAT(Memory Analyzer Tool)，一个基于Eclipse的内存分析工具，是一个快速、功能丰富的JAVA heap分析工具，它可以帮助我们查找内存泄漏和减少内存消耗。使用内存分析工具从众多的对象中进行分析，快速的计算出在内存中对象的占用大小，看看是谁阻止了垃圾收集器的回收工作，并可以通过报表直观的查看到可能造成这种结果的对象。

前面介绍到的由Android dump出来的 hprof 文件，如果要在 MAT 中打开，需要使用Android sdk提供的 hprof-conv 工具对文件进行转换：

		hprof-conv input.hprof out.hprof

然后通过 MAT 导入hprof文件即可

[MAT使用指南](http://androidperformance.com/2015/04/11/AndroidMemory-Usage-Of-MAT.html)




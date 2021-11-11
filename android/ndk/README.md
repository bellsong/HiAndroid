# Android NDK

## At the Beginning
对于一个新知识，要解决三个问题：是什么，为什么，怎么做。

### 概念
1. JavaVm
JavaVM 是虚拟机在 JNI 层的代表，一个进程只有一个 JavaVM，所有的线程共用一个 JavaVM。

2. JNIEnv
JNIEnv 表示 Java 调用 native 语言的环境，是一个封装了几乎全部 JNI 方法的指针。

JNIEnv 只在创建它的线程生效，不能跨线程传递，不同线程的 JNIEnv 彼此独立。

native 环境中创建的线程，如果需要访问 JNI，必须要调用 AttachCurrentThread 关联，并使用 DetachCurrentThread 解除链接

### 两种代码风格（C/C++）
JavaVM 和 JNIEnv 在 C 语言环境下和 C++ 环境下调用是有区别的，主要表现在：

C风格：(*env)->NewStringUTF(env, “Hellow World!”);

C++风格：env->NewStringUTF(“Hellow World!”);

建议使用 C++ 风格，这也是大部分代码使用的形式。

注意：C++ 风格其实只是封装了 C 风格，使得调用更加简介方便。


## NDK 简介
    Native Development Kit 简称 NDK，是一种基于原生程序接口的软件开发接口。不同于java编写的程序运行在虚拟机上，通过此工具开发的程序可以直接以本地语言运行。因此只有Java等基于虚拟机运行的语言程序才会有原生开发工具包。

### NDK是一系列工具的集合
    NDK提供了一系列的工具，帮助开发者快速开发C（或C++）的动态库，并能自动将so和java应用一起打包成apk。这些工具对开发者的帮助是巨大的.

    NDK集成了交叉编译器，并提供了相应的mk文件隔离CPU、平台、ABI等差异，开发人员只需要简单修改mk文件（指出“哪些文件需要编译”、“编译特性要求”等），就可以创建出so。

    NDK可以自动地将so和Java应用一起打包，极大地减轻了开发人员的打包工作。

## 为什么使用NDK

1. 提高程序执行效率
    将要求高性能的应用逻辑用C开发，从而提高应用程序的执行效率
2. 便于移植
    用C/C++编写的库可以方便在其他嵌入式平台上再次使用
3. 方便的使用现存的开源库
    大部分现存的开源库都是用C/C++编写，可以复用。
4. 代码保护
    由于apk的java层代码容易被反编译，用C/C++库反编译难度较大

## 怎么用NDK进行开发

JNI的话，有一份叫 《The Java Native Interface Programmer's Guide and Specification》的文档非常好，比较清楚的讲解了Java代码如何与C/C++代码相互访问的方法。至于NDK，可以参考NDK自带的文档，或者android aosp项目中来了解如何编写Android.mk。 参考NDK自带的文档来了解如何编译，链接，调试，如何编写Application.mk，如何使用STL等。还可以搜一下如何把NDK集成进自己使用的IDE中，如Eclipse，Android Studio等等。

### 开发环境准备
1. CMake
* CMakeList.txt 是脚本文件, 需要指定包含哪些源代码;
* 可以写一些条件语句, 实现不同的代码包含
* 内部说明:add_library 表示编译一个代码库, 内部包含了代码库的名称, 以及源代码有哪些
2. NDK
* ndk-build 形式; Android Studio 2.2之前的模式
* CMake 形式: CLion C/C++编辑器; AS2.2之后整合了CLion代码, AS就支持了CMake形式的NDK开发

### C代码库生成的名称规则
1. 如果栈顶代码库名称为 "nh" 那么生成的文件必定是libnh.so
命名规则: lib库名.so
2. System.loadLibrary(库名); //此处不能包含前面的lib和后面的.so

### Java 调用C/C++代码
1. 任何一个类的方法, 如果声明了native修饰符, 那么就可以认为是一个C代码;

2. 可以用对象, 类直接调用

3. 创建C/C++文件; 如果一个类中有一个native的方法, 那么对应的C方法: Java_包名类名方法名(JNIEnv *env, ...);

4. 当Java类中包含了native的方法, 那么这个类必须写一个静态初初始化块: System.loadLibrary("库名")

## [练习](./demo/README.md)

1. 环境搭建（NDK BUILD 方式和CMake方式）打印HelloWorld

2. 计算机

## 参考

[官方文档](https://developer.android.com/ndk/guides)

[官方samples](https://developer.android.com/ndk/samples)

[CMake](https://developer.android.google.cn/ndk/guides/cmake)

[向您的项目添加 C 和 C++ 代码](https://developer.android.google.cn/studio/projects/add-native-code.html)

https://www.jianshu.com/p/0261e6cceb3e

[Android NDK 简介 极客学院](http://wiki.jikexueyuan.com/project/jni-ndk-developer-guide/ndkoverview.html)

[NDK开发入门终极教程](https://juejin.im/post/5c3b01016fb9a049f81984bb)

[AndroidStudio下NDK开发流程](http://www.liuhaihua.cn/archives/566540.html)

[Android NDK开发之入门示例 用C++ 写一个能四则运算的计算器](https://yq.aliyun.com/articles/656469/)

[android Ndk JNI 入门](https://cloud.tencent.com/developer/article/1356491)

[Android Studio jni开发入门——看我就够了！](http://www.jcodecraeer.com/a/anzhuokaifa/2017/0401/7769.html)

[JNI学习积累之一 ---- 常用函数大全](https://blog.csdn.net/qinjuning/article/details/7595104)

[JNI相关](./jni.md)

[JNI和NDK编程-使用AndroidStudio进行NDK开发](https://blog.csdn.net/guiying712/article/details/75452193)
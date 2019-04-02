# Android NDK

## At the Beginning
对于一个新知识，要解决三个问题：是什么，为什么，怎么做。

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

## 练习

1. 打印HelloWorld

2. 

## 参考


[官方文档](https://developer.android.com/ndk/guides)

[Android NDK 入门与实践](http://wuxiaolong.me/2017/12/27/AndroidNDK/)

https://www.jianshu.com/p/0261e6cceb3e

https://developer.android.com/ndk/samples
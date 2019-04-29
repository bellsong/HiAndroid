# Android JNI

## 诞生背景

## 解决的问题

## 优点和缺点

## 基础

1. Java如何调用Native代码
2. Native如何调用Java代码

## JNI 与 NDK关系
JNI是Java中的接口，用于java与本地语言(例如C、C++)交互

NDK是Android中工具开发包，用于快速开发C、C++的动态库，并自动将so和应用一起打包成APK

两者之间的关系：JNI是实现的目的，NDK是实现的手段，可以理解成在AndroidStudio中，通过NKD实现JNI的功能

## 将字符转换成byte数组在java层再转成String

JNI代码

```java
    JNIEXPORT jbyteArray JNICALL Java_com_**_getString(JNIEnv *env, jclass clazz){
    char buf[] = “abc”;
    int len = strlen(buf);
    jbyteArray byte_array = env->NewByteArray(len);
    env->SetByteArrayRegion(byte_array, 0, len, (jbyte*)buf);
    return byte_array;
    }
```

Java代码
```java
    new String(getString(),"UTF-8");
```
## 参考

[JNI官方文档](https://docs.oracle.com/javase/7/docs/technotes/guides/jni/spec/jniTOC.html?hl=zh-cn)

[Android JNI编程—JNI基础](https://www.jianshu.com/p/aba734d5b5cd)

[Android JNI出坑指南](https://cloud.tencent.com/developer/article/1071854)

[emoji表情引发的JNI崩溃](http://m.www.cnblogs.com/meteoric_cry/p/4960077.html)

[由Emoji表情发现的JNI GetStringUTFChars()隐藏的问题](https://www.jianshu.com/p/f604a4224098)

[Android：JNI 与 NDK到底是什么？](https://blog.csdn.net/carson_ho/article/details/73250163)

[Android JNI原理分析](http://gityuan.com/2016/05/28/android-jni/)
# Android 方法耗时统计

## 背景

    需求开发或者速度优化过程中，希望打印方法和参数

## 原理

## Hugo统计方法耗时使用

使用方法很简单，Hugo是基于注解被调用的

1. 引入相关依赖

```xml
    buildscript {
        repositories {
            mavenCentral()
        }

        dependencies {
            classpath 'com.jakewharton.hugo:hugo-plugin:1.2.1'
        }
    }

    apply plugin: 'com.android.application'
    apply plugin: 'com.jakewharton.hugo'

```

2. 在方法上加上 @DebugLog 即可, 也可以在类前加上@DebugLog， 对该类的所有方法都可以监控到。

## 参考

[官网地址](https://github.com/JakeWharton/hugo)

[统计方法耗时神器 Hugo](https://yq.aliyun.com/articles/7104)

[便于性能分析的日志框架 hugo](https://xiaozhuanlan.com/topic/1428095376)

[Hugo 应用内方法调用监控实践](https://www.jianshu.com/p/e3d9221f8e37)

## MethodTraceMan

## 参考

[App流畅度优化：利用字节码插桩实现一个快速排查高耗时方法的工具](https://juejin.im/post/5da33dc56fb9a04e35597a47?utm_source=gold_browser_extension)

[MethodTraceMan](https://github.com/zhengcx/MethodTraceMan)

## 方法耗时统计插件Mirror]

[Android魔镜：方法耗时统计插件Mirror-基础篇](https://juejin.im/post/5bce85e9e51d457b7c3e6bed?utm_source=gold_browser_extension)
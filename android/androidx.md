# AndroidX 了解一下

## 什么是AndroidX

AndroidX 是 Android 团队用于在 Jetpack 中开发、测试、打包和发布库以及对其进行版本控制的开源项目。

## 使用AndroidX

如果要在新项目中使用 AndroidX，则需要将编译 SDK 设置为 Android 9.0（API 级别 28）或更高版本，并在 gradle.properties 文件中将以下两个 Android Gradle 插件标记设置为 true。

android.useAndroidX：如果设置为 true，Android 插件会使用相应的 AndroidX 库，而非支持库。如果未指定，则该标记默认为 false。

android.enableJetifier：如果设置为 true，Android 插件会重写其二进制文件，自动迁移现有的第三方库以使用 AndroidX。如果未指定，则该标记默认为 false。


## 参考

[AndroidX 官网](https://developer.android.google.cn/jetpack/androidx)

[Android:你好,androidX！再见,android.support](https://www.jianshu.com/p/41de8689615d)
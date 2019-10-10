# Android 主题切换 Theme换肤

## 原理

早期的换肤都是将所有的皮肤资源打到apk里面，皮肤apk和应用apk采用相同android:sharedUserId，这样应用apk就得到了皮肤apk的访问权限，从尔可以实现替换皮肤的功能。
但是这样实现遇到两个问题：

1.用户必须安装皮肤apk(这个对产品来说基本很难接受，也导致换肤功能形同虚设的主要原因)
2.从开发角度，即使应用apk能获取皮肤apk的访问权限，但是替换已有view的drawable是一个浩大的工程，为了一个换肤的功能侵入了过多的特有业务代码，不仅极难维护也让正常的业务需求变得复杂和不确定。
所以要实现一个可行的换肤功能，必须解决上面两个问题：

1.用户无感知
2.对业务开发者透明
随着动态部署的实现用户无感知已经不是问题了,基本原理就是下载一个皮肤apk(不需要安装)

```java
    mAsset = AssetManager.class.getConstructor().newInstance();
    Method addAssetPathMethod=AssetManager.class.getMethod("addAssetPath", new Class[]{String.class});
    mCookie=(Integer) addAssetPathMethod.invoke(mAsset, apkPath);
```

通过apkPath加载一个带有皮肤资源的AssetManager,然后对AssetManager做一次封装，形成一个皮肤的Resources。换肤的实现就是把这个皮肤的Resources替换应用原生Context里面的mResources,让所有的Resources.getDrawable取到的是皮肤Resources中的资源。

## 参考

[Android App切换主题的实现原理剖析](https://blog.csdn.net/watertekhqx/article/details/51320515)

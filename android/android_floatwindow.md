# Android 悬浮窗

## 原理

* 悬浮窗是如何浮起来的，去了解窗口层级关系
* 悬浮窗有哪些限制，怎么可以越过用户授权
* 悬浮窗是怎么接收事件的，了解窗口和用户的输入系统

## 怎么做

```java

final WindowManager windowManager = getWindowManager(context);
//创建自定义浮窗
 FloatView    hideDialog = new FloatView(context);
WindowManager.LayoutParams  params = new     WindowManager.LayoutParams();
//params.type 窗口类型，主要决定了窗口的层次
params.type = WindowManager.LayoutParams.TYPE_PHONE;
params.format = PixelFormat.RGBA_8888;
//params.flags 描述窗体其他属性的标记位，
//LayoutParams.FLAG_NOT_FOCUSABLE表示不能获取输入法焦点
params.flags = LayoutParams.FLAG_NOT_FOCUSABLE;
params.gravity = Gravity.LEFT | Gravity.TOP;
params.width = LayoutParams.MATCH_PARENT;
params.height = LayoutParams.MATCH_PARENT;
//添加
windowManager.addView(hideDialog, dialogParams);


/**
 * 判断是否开启浮窗权限,api未公开，使用反射调用
 * AppOpsManager是api 19以后引入的，第三方rom可以利用它来管理权限，将某些权限交给用户来定夺，* 例如浮窗。详细参考官方文档：AppOpsManager。
 * @return
 */
private static boolean hasAuthorFloatWin(Context context){

    if (Device.getSystemVersion() < 19){
        return false;
    }
    try {
        AppOpsManager appOps = (AppOpsManager)context.getSystemService(Context.APP_OPS_SERVICE);
        Class c = appOps.getClass();
        Class[] cArg = new Class[3];
        cArg[0] = int.class;
        cArg[1] = int.class;
        cArg[2] = String.class；
        Method lMethod = c.getDeclaredMethod("checkOp", cArg);
        //24是浮窗权限的标记
        return (AppOpsManager.MODE_ALLOWED == (Integer) lMethod.invoke(appOps, 24, Binder.getCallingUid(), context.getPackageName())){

    } catch(Exception e) {
       return false;
    }
}

```

## 参考

[Android仿微信文章悬浮窗效果](https://juejin.im/post/5bbc564df265da0aea69962a?utm_source=gold_browser_extension)

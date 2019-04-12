# AccessibilityService

## 是什么
    官方提供这个服务初衷是为了解决残障人士能够在使用APP时获得更好的体验，具体表现是点击区域时有语音或者触摸提示。开发者只需要通过增加类似contentDescription的属性，就能在不修改代码的情况下，让残障人士能够获得使用体验的优化。

    现在AccessibilityService已经基本偏离了它设计的初衷，至少在国内是这样，越来越多的App借用AccessibilityService来实现了一些其它功能，甚至是灰色产品。

    不知道从什么时候开始，AccessibilityService突然从一个残障人士使用的辅助服务，一跃变成了各种App的黑科技，利用AccessibilityService来做的事情，也越来越偏离了AccessibilityService设计的初衷，各种安全问题也随之暴露出来。

如何理解AccessibilityService

    很多人可能对AccessibilityService了解的不是很深入，所以认为AccessibilityService是在调用一些系统服务来自动执行一些操作，实际上，这个理解不能算错，当然也不全对，我觉得你可以把AccessibilityService理解为——『按键精灵』。相信很多开发者都玩过PC上的这款软件，他的作用，就是将你一次操作的整个记录，录制下来，然后就可以根据这个记录，重复的执行这些操作，例如：先点击某个输入框，再输入XXXX，再输入验证码，最后点击某按钮，这些操作如果需要重复执行，那么显然是一套机械的步骤，那么通过按键精灵，记录下这些操作后，直接通过脚本就可以完成这些操作。其实AccessibilityService跟这个是一样的，我们记录的，实际上就是我们的操作步骤，或者称之为『脚本』，那么系统在监控整个手机的各种AccessibilityService事件时，就会根据我们的逻辑来判断该使用哪一个脚本。

## 可以做什么

## 怎么做

### 获取视图

```java 
    public AccessibilityNodeInfo getSource ()  //Added in API level 14
```

```java
    public List<AccessibilityWindowInfo> getWindows () //Added in API level 21
```
不仅能够更好地表示视图层次，而且还能够公开包含当前输入方法(输入法)的节点。

### 查找目标

* findAccessibilityNodeInfosByViewId(String viewId)
* findAccessibilityNodeInfosByText(String text)

### 触发事件

* 全局系统事件
    // 返回键
    performGlobalAction(AccessibilityService.GLOBAL_ACTION_BACK);
    // HOME键
    performGlobalAction(AccessibilityService.GLOBAL_ACTION_HOME);

* View 事件

   AccessibilityNodeInfo performAction()

## 检测服务是否开启
* 方法一:借助服务管理器AccessibilityManager来判断,但是该方法不能检测app本身开启的服务.
```java
    private boolean enabled(String name) {
        AccessibilityManager am = (AccessibilityManager) getSystemService(Context.ACCESSIBILITY_SERVICE);
        List<AccessibilityServiceInfo> serviceInfos = am.getEnabledAccessibilityServiceList(AccessibilityServiceInfo.FEEDBACK_GENERIC);
        List<AccessibilityServiceInfo> installedAccessibilityServiceList = am.getInstalledAccessibilityServiceList();
        for (AccessibilityServiceInfo info : installedAccessibilityServiceList) {
            Log.d("MainActivity", "all -->" + info.getId());
            if (name.equals(info.getId())) {
                return true;
            }
        }
        return false;
    }
```

* 方法二:我们知道大部分的系统属性都在settings中进行设置,比如wifi,蓝牙状态等,而这些信息主要是存储在settings对应的的数据库中(system表和serure表),这就意味我们可以通过直接读取setting设置来判断相关服务是否开启:

```java
    private boolean checkStealFeature1(String service) {
        int ok = 0;
        try {
            ok = Settings.Secure.getInt(getApplicationContext().getContentResolver(), Settings.Secure.ACCESSIBILITY_ENABLED);
        } catch (Settings.SettingNotFoundException e) {
        }

        TextUtils.SimpleStringSplitter ms = new TextUtils.SimpleStringSplitter(':');
        if (ok == 1) {
            String settingValue = Settings.Secure.getString(getApplicationContext().getContentResolver(), Settings.Secure.ENABLED_ACCESSIBILITY_SERVICES);
            if (settingValue != null) {
                ms.setString(settingValue);
                while (ms.hasNext()) {
                    String accessibilityService = ms.next();
                    if (accessibilityService.equalsIgnoreCase(service)) {
                        return true;
                    }

                }
            }
```


## 性能

## 

## 参考

* [AccessibilityService 官方介绍](https://developer.android.com/reference/android/accessibilityservice/AccessibilityService.html)

* [Android 辅助功能进阶使用](https://www.jianshu.com/p/7ffaf7ca07bc)

* [理解AccessibilityService](http://www.voidcn.com/article/p-qifdyzff-sn.html)

* [辅助模式最终考验的是想象力](https://segmentfault.com/a/1190000015345637)
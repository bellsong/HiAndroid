# Android Permission 权限使用指南

* 权限声明
* 动态权限说明
* 自定义权限

## Android 6.0 动态权限介绍

### 前言

Android自诞生以来，其安全性一直被人们所诟病。加之，国内安卓App生态环境恶劣，只要是个应用，恨不得申请所有权限。谷歌或许也意识到这个问题，因此在Marshmallow包含了期待已久的运行时权限管理。

### M时代之前
在Android 6.0之前，一个应用所需要的权限会在安装的时候列出，如果用户想安装这个应用，只能全盘接受。国内应用一般都是权限毒瘤，这一直以来都是安卓用户的痛中之痛。主要体现在两点：
>
无法动态授权
无法知道应用什么时候使用了权限

如果M之前的代码不加修改，直接跑在M手机上，可能导致应用崩溃。

### M时代之后
Android中的权限分为两大类：普通权限和危险权限，具体可以参考开发文档。在M手机上，对于敏感权限，需要在程序运行时进行动态申请。对于非敏感权限，即Normal Permissions，和M之前的使用相同。

Android6.0发布新特性：
>
- 锁频下语音搜索
- 指纹识别
- 更完整的应用权限管理
- Doze电量管理
- Now on Tap
- App link

上面六个新特性中 **更完整的应用权限管理** 就为如何在Android 6.0上更好的使用Android系统权限提供了一个新的方案。


##更完整的应用权限管理 runtime permissions model
android 6.0开始推行新的权限管理机制——动态权限管理，类似于ios上的权限申请，权限的获取不再是在app安装时进行，而是在运行时申请。

在Android 6.0中谷歌摒弃了之前的
**install time permissions model**
取而代之的是
**runtime permissions model**

先来说说install time permissions model，这个大家不陌生，就是当Android App安装的时候会向用户展示一坨权限，如果此时用户选择安装，则表示用户同意将这些权限赋予App，如果用户不同意那么这个App就会取消安装。

runtime permissions model就牛逼了，在App安装的时候同样会向用户展示所需要的权限，并且在用户选择安装App的时候并不表示用户将这些权限赋予了App，而是需要App在运行阶段主动去申请这些权限。这样做的好处显而易见，App对权限的申请对于用户来说变得更加透明，而且用户对App权限的控制也更加灵活。

###权限的分类

当然并不是所有的权限都需要动态申请，谷歌把权限划分为两大类，普通权限和危险权限。对于普通权限，还是和之前一样在AndroidManifest.xml里申请就行，而对于危险权限，就必须在运行时动态申请，得到用户的授权后才可使用。

Android将系统权限分成了四个保护等级

normal,dangerous,signature,signatureOrSystem,

其中最常见的是normal permission和dangerous permission两类。

normal permission涵盖的一系列权限的共同点是：

App需要访问App运行沙盒以外的数据或资源，但是这些资源对用户的隐私或其他App的危险性较小，下面列举一下这些权限：

* ACCESS_LOCATION_EXTRA_COMMANDS
* ACCESS_NETWORK_STATE
* ACCESS_NOTIFICATION_POLICY
* ACCESS_WIFI_STATE
* BLUETOOTH
* BLUETOOTH_ADMIN
* BROADCAST_STICKY
* CHANGE_NETWORK_STATE
* CHANGE_WIFI_MULTICAST_STATE
* CHANGE_WIFI_STATE
* DISABLE_KEYGUARD
* EXPAND_STATUS_BAR
* FLASHLIGHT
* GET_PACKAGE_SIZE
* INTERNET
* KILL_BACKGROUND_PROCESSES
* MODIFY_AUDIO_SETTINGS
* NFC
* READ_SYNC_SETTINGS
* READ_SYNC_STATS
* RECEIVE_BOOT_COMPLETED
* REORDER_TASKS
* REQUEST_INSTALL_PACKAGES
* SET_TIME_ZONE
* SET_WALLPAPER
* SET_WALLPAPER_HINTS
* TRANSMIT_IR
* USE_FINGERPRINT
* VIBRATE
* WAKE_LOCK
* WRITE_SYNC_SETTINGS
* SET_ALARM
* INSTALL_SHORTCUT
* UNINSTALL_SHORTCUT


以上这些就是Android 6.0中所有的normal permissions了。

dangerous permissions 涵盖的一系列权限的共同点是：这些权限会读写用户的隐私信息，也可能会读写用户存储的数据或影响其他App的正常运行。下面例举出这些权限：

| 权限组 | 权限 |
| ---------- | ---------- |
| CALENDAR | READ_CALENDAR，WRITE_CALENDAR |
| CAMERA | CAMERA |
| CONTACTS | READ_CONTACTS，WRITE_CONTACTS，GET_ACCOUNTS |
| LOCATION | ACCESS_FINE_LOCATION，ACCESS_COARSE_LOCATION |
| MICROPHONE | RECORD_AUDIO |
| PHONE | READ_PHONE_STATE，CALL_PHONE，READ_CALL_LOG，WRITE_CALL_LOG，ADD_VOICEMAIL，USE_SIP，PROCESS_OUTGOING_CALLS |
| SENSORS | BODY_SENSORS |
| SMS | SEND_SMS，RECEIVE_SMS，READ_SMS，RECEIVE_WAP_PUSH，RECEIVE_MMS |
| STORAGE | READ_EXTERNAL_STORAGE，WRITE_EXTERNAL_STORAGE |

以上这些权限就是Android6.0中所有的dangerous permissions。

Runtime Permissions针对的是dangerous permissions，normal permissions还是会在App安装期间被默认赋予。


可以看出普通权限都是和手机本身有关的功能，而用户数据的权限（包括几乎所有app都要申请的读写SD卡得权限）已被划入危险权限中。

API23中新增了几个与权限有关的接口

* 用于检查权限的接口，该接口可以通过ContextCompat调用，也可以在当前的activity里直接调用。

```java
// Assume thisActivity is the current activity
int permissionCheck = ContextCompat.checkSelfPermission(thisActivity,
        Manifest.permission.WRITE_CALENDAR);
```
该接口的返回值代表权限查询的结果，PackageManager.PERMISSION_GRANTED表示用户已经授权，PackageManager.PERMISSION_DENIED表示用户还没有授权，这是需要使用另一个接口进行权限申请。这个方法在使用时不需要再判断系统版本了，因为方法内部会区分版本，如果是API23以下会直接通过检查manifest的方式进行。

* 用于请求权限

```java
public static void requestPermissions (Activity activity, String[] 
                  permissions, int requestCode)
```                  
参数的第一个activity，也就是说权限的申请必须在UI线程去做，第二个参数是要申请的权限数组，第三个参数请求码用于在回调时区分不同的权限申请（比如你可能在同一个activity的两处申请了两个不同的权限）。

通过实现接口

```java 
ActivityCompat.OnRequestPermissionsResultCallback中的
public abstract void onRequestPermissionsResult (int requestCode, String[]
               permissions, int[] grantResults)
```

activity就能接收到请求申请结果。grantResults数组记录了每个权限申请的结果。requestCode就是刚刚申请时的值。

即使为了安全，也不能不考虑用户体验，为了防止用户被过多的打扰，API 23还提出了pemisssion_group权限组概念，即把权限分组，用户只要授权了某个组里的某一个权限，那么该组的其他权限就不需要再次授权了。比如用户已经授权了READ_PHONE_STATE，那么下次再申请CALL_PHONE时就不会再弹窗让用户授权了。


* 用来检查是否之前用户已经拒绝过这个权限了，包括已经勾选了不再提示的，这种情况下，可能需要进行提示，告诉用户这个权限app用来做什么。

```java
ActivityCompat.shouldShowRequestPermissionRationale

public static boolean shouldShowRequestPermissionRationale (Activity
                         activity, String permission)
```


另外有两个特殊的权限，

>
SYSTEM_ALERT_WINDOW 
WRITE_SETTINGS

这两个权限的管理在系统应用管理里，如果需要使用，需要通过intent方式启动该管理页面，用户允许后方可使用，判断是否有这两个权限的方法分别是
>
Settings.canDrawOverlays(Context) //SYSTEM_ALERT_WINDOW
Settings.System.canWrite(Context) //WRITE_SETTINGS

另外可以再intent指定URI，这样在启动应用管理UI后可以直接进入指定app的管理页面，如下的方式。

```java
Intent intent = new Intent(Settings.ACTION_MANAGE_OVERLAY_PERMISSION);
intent.setData(Uri.parse("package:com.app.my"));
intent.setFlags(Intent.FLAG_ACTIVITY_NEW_TASK);
this.startActivity(intent);
```

当然这些变化只有在app改变编译时使用的sdk版本到23时才需要用到，如果编译时sdk版本不是23，那么会以兼容方式运行，系统仍然会通过静态方式进行权限检查。


###举个例子
下面我们就以STORAGE组中的WRITE_EXTERNAL_STORAGE为例子，尝试在Android 6.0中使用runtime permission相关api。
目标：在手机的存储设备上新建一个hello.txt。

1.在AndroidManifest文件中添加如下权限生命

```java
<uses-permission android:name="android.permission.WRITE_EXTERNAL_STORAGE"/>

```

2.在MainActivity中添加如下方法：

    //在sdcard上新建一个名为fileName的文件
    private void createFile(String fileName){
        File sdcard = Environment.getExternalStorageDirectory();
        File newFile = new File(sdcard,"/" + fileName) ;
        if(!newFile.exists()){
            try {
                newFile.createNewFile();
            } catch (IOException e) {
                e.printStackTrace();
            }
        }
    }
    
好的，如果没有runtime permissions这个概念的话，那其实这个功能已经完成了，它在Android 6.0之前的版本上的运行
它提示用户该应用会修改会删除SD卡的内容，如果此时用户选择安装，那么也就是默认将WRITE_EXTERNAL_STORAGE这个权限赋予了该应用。

接下来再在Android6.0的机子上安装这个应用，

安装界面也是向用户展示了App所涉及的权限。

当实际执行时log显示App没有权限在SD卡上创建文件。这里要再讲一下runtime permissions原理

对于权限分类中的dangerous permissions，runtime permissions要求App在运行的时候做权限请求，某则App则无法获得相应请求。

接下来看一下怎么在代码中进行权限的申请：

```java 

public static final int EXTERNAL_STORAGE_REQ_CODE = 10 ;
    
    public void requestPermission(){
        //判断当前Activity是否已经获得了该权限
        if (ContextCompat.checkSelfPermission(this,
                Manifest.permission.WRITE_EXTERNAL_STORAGE)
                != PackageManager.PERMISSION_GRANTED) {

            //如果App的权限申请曾经被用户拒绝过，就需要在这里跟用户做出解释
            if (ActivityCompat.shouldShowRequestPermissionRationale(this,
                    Manifest.permission.WRITE_EXTERNAL_STORAGE)) {
                Toast.makeText(this,"please give me the permission",Toast.LENGTH_SHORT).show();
            } else {
                //进行权限请求
                ActivityCompat.requestPermissions(this,
                        new String[]{Manifest.permission.WRITE_EXTERNAL_STORAGE},
                        EXTERNAL_STORAGE_REQ_CODE);
            }
        }
    }

```
    
当进行权限申请，并且用户做出选择后会回调onRequestPermissionsResult这个方法，在这个方法中做相关处理

```java
@Override
    public void onRequestPermissionsResult(int requestCode,
                                           String permissions[], int[] grantResults) {
        switch (requestCode) {
            case EXTERNAL_STORAGE_REQ_CODE: {
                // 如果请求被拒绝，那么通常grantResults数组为空
                if (grantResults.length > 0
                        && grantResults[0] == PackageManager.PERMISSION_GRANTED) {
                    //申请成功，进行相应操作
                    createFile("hello.txt");
                } else {
                    //申请失败，可以继续向用户解释。
                }
                return;
            }
        }
    }

```
        
到这里runtime permissions的权限申请操作就结束了，允许后会再次调用createFile("hello.txt")方法，文件会被成功创建。

##不重新发明轮子
虽然动态权限的编码逻辑简单，涉及的Api也就几个。但由于申请权限的位置和授权结果回调分别在两个地方，给人的感觉就一个字乱；并且，如果Activity规模较大、需要申请权限较多时，代码就会变得混乱。针对这些，已经有前辈封装了不少 *[动态权限第三方库](https://gist.github.com/dlew/2a21b06ee8715e0f7338)，*

这里拿PermissionsDispatcher进行说明。PermissionsDispatcher具有如下优点

* 采用注解，代码形式简洁；
* PermissionsDispatcher采用编译时生成代理类，让Activity/Fragment调用。因此，在效率上和官方写法没有区别。

###使用
给需要动态权限的Activity或者Fragment加上注解RuntimePermissions：

```
@RuntimePermissions
public class MainActivity extends AppCompatActivity {
  // ...
}
```

给涉及到动态权限的方法加上注解@NeedsPermission：

```
@RuntimePermissions
public class MainActivity extends AppCompatActivity {
    @NeedsPermission(Manifest.permission.CALL_PHONE)
    void callPhone() {
        // Trigger the calling of a number here
    }
}
```
重新编译工程后，将产生一个以PermissionsDispatcher为后缀的代理类，例如这里是MainActivityPermissionsDispatcher。
如果提示找不到MainActivityPermissionsDispatcher类，说明apt代码生成失败，重启Android Studio试试。
代理类生成后，重写权限申请回调，让代理类来处理：

```java
@Override
public void onRequestPermissionsResult(int requestCode, String[] permissions, int[] grantResults) {
    super.onRequestPermissionsResult(requestCode, permissions, grantResults);
    MainActivityPermissionsDispatcher.onRequestPermissionsResult(this, REQUEST_PERMISSION_CALL_PHONE, grantResults);
}
```

4.3 处理授权结果
应用首次申请权限，用户拒绝，使用@OnPermissionDenied标识的方法将作为回调：

```java
@OnPermissionDenied(Manifest.permission.CALL_PHONE)
void showDeniedForCallPhone() {
    Toast.makeText(this, "未授权", Toast.LENGTH_SHORT).show();
}
```

应用首次申请权限被拒绝，再次申请权限时，给出提示信息，使用@OnShowRationale注解标识：

```java
@OnShowRationale(Manifest.permission.CALL_PHONE)
void showRationaleForCallPhone(final PermissionRequest request) {
    new AlertDialog.Builder(this)
            .setTitle("提示")
            .setMessage("需要授权电话权限")
            .setNegativeButton("取消", new DialogInterface.OnClickListener() {
                @Override
                public void onClick(DialogInterface dialog, int which) {
                    request.cancel();
                }
            })
            .setPositiveButton("确定", new DialogInterface.OnClickListener() {
                @Override
                public void onClick(DialogInterface dialog, int which) {
                    request.proceed();
                }
            })
            .create()
            .show();
}
```

应用非首次申请权限时，授权对话框会多出一个复选框不再询问，相应回调方法用@OnNeverAskAgain标识：

```java
@OnNeverAskAgain(Manifest.permission.CALL_PHONE)
void showNeverAskForPhoneCall() {
    Toast.makeText(this, "不再询问", Toast.LENGTH_SHORT).show();
}
```

[PermissionsDispatcher完整示例](https://github.com/AaronChanSunny/AndroidRuntimePermissions/blob/feature/dispatcher/app/src/main/java/com/aaron/androidruntimepermissions/MainActivity.java)


再来一个 [android-permission-manager](https://github.com/buchandersenn/android-permission-manager)

优点：

* 代码简洁，容易理解
* 使用简单

```java
private final PermissionManager permissionManager = PermissionManager.create(this);

private void showCameraPreview() {
    permissionManager.with(Manifest.permission.CAMERA)
            .onPermissionGranted(startPermissionGrantedActivity(this, new Intent(this, CameraPreviewActivity.class)))
            .onPermissionDenied(showPermissionDeniedSnackbar(mLayout, "Camera permission request was denied.", "SETTINGS"))
            .onPermissionShowRationale(showPermissionShowRationaleSnackbar(mLayout, "Camera access is required to display the camera preview.", "OK"))
            .request();
}

@Override
public void onRequestPermissionsResult(int requestCode, @NonNull String[] permissions, @NonNull int[] grantResults) {
    permissionManager.handlePermissionResult(requestCode, grantResults);
}

```

看到一个跟着一个的. 有没有跟构建Notification的builder是曾相识，实现的方式使用了建造者模式。

再看一个 [Grant](https://github.com/anthonycr/Grant)

优点：

* 同样代码简洁，容易理解
* 使用简单

```java
@Override
public void onRequestPermissionsResult(int requestCode, 
                                       @NonNull String[] permissions, 
                                       @NonNull int[] grantResults) {
    PermissionsManager.getInstance().notifyPermissionsChange(permissions, grantResults);
}

```

```java
PermissionsManager.getInstance().requestPermissionsIfNecessaryForResult(this,
    new String[]{Manifest.permission.WRITE_EXTERNAL_STORAGE}, new PermissionsResultAction() {

    @Override
    public void onGranted() {
        writeToStorage();
    }

    @Override
    public void onDenied(String permission) {
        Toast.makeText(MainActivity.this, 
                      "Sorry, we need the Storage Permission to do that", 
                      Toast.LENGTH_SHORT).show();
    }
});

``` 

使用了一个集合维护相关请求权限的动作，将结果通知到调用者，观察者模式。

对于一个需求，实现思路不一样，最终代码的形态就不一样，但只要遵循基本原则方便调用，易于阅读理解，低耦合等，都是好代码，值得学习。

# Libraries

* [Andele](https://github.com/hiqes/andele)
* [android-permissions-gradle-plugin](https://github.com/galex/android-permissions-gradle-plugin)
* [android-permission-manager](https://github.com/buchandersenn/android-permission-manager)
* [AndroidPermissions](https://github.com/ZeroBrain/AndroidPermissions)
* [assent](https://github.com/afollestad/assent)
* [Dexter](https://github.com/Karumi/Dexter)
* [EasyMPermission](https://github.com/mobmead/EasyMPermission)
* [EasyPermissions](https://github.com/googlesamples/easypermissions)
* [Gota](https://github.com/alhazmy13/Gota)
* [Grant](https://github.com/anthonycr/Grant)
* [Let](https://github.com/canelmas/let)
* [Nammu](https://github.com/tajchert/Nammu)
* [Permiso](https://github.com/greysonp/permiso)
* [PermissionsDispatcher](https://github.com/hotchemi/PermissionsDispatcher)
* [PermissionUtil](https://github.com/kayvannj/PermissionUtil)
* [RxPermissions](https://github.com/tbruyelle/RxPermissions)
* [WPAndroidPermissions](https://github.com/webpartners/WPAndroidPermissions)


##参考

[Android 6.0运行时权限完全解析](http://aaronchansunny.github.io/2016/03/05/AndroidRuntimePermissions/)

[Android 6.0 动态权限介绍](http://www.jianshu.com/p/4f587f742d85)

[android 6.0权限动态申请](http://blog.csdn.net/changlei_shennan/article/details/51225229)

[浅谈Android 6.0之Runtime Permissions](http://www.cnblogs.com/zqlxtt/p/4873831.html)

[Android 6.0 动态权限初探](http://www.kyletung.com/android-6-0-dynamic-permission/)

[Android 6.0 动态权限申请注意事项](http://www.it610.com/article/5127590.htm)

[Android 6.0 运行时权限处理](https://www.aswifter.com/2015/11/04/android-6-permission/)

[聊一聊Android 6.0的运行时权限](http://droidyue.com/blog/2016/01/17/understanding-marshmallow-runtime-permission/index.html)

[在Android 6.0 设备上动态获取权限](http://gudong.name/%E6%8A%80%E6%9C%AF/2015/11/10/android_m_permission.html)

[Android M 新的运行时权限开发者需要知道的一切](http://jijiaxin89.com/2015/08/30/Android-s-Runtime-Permission/)

[Android 新权限系统，及使用AOP进行适配](http://www.atatech.org/articles/57176)

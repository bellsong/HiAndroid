#### 获取设备管理权限增加卸载难度
一、背景
渠道会启动线下渠道合作，针对于线下装机（用户可卸载）比例上升，可以通过获取“设备管理器”的权限 来增加用户的卸载难度 从而降低卸载率
当App激活成为“设备管理器”，用户不能直接卸载，需要取消App “设备管理器”权限，增加了卸载难度，可以在一定程度提高App生存能力

二、实现方式

1. 我们的app在第一次运行是注册app成为设备管理器 
2. 渠道那边会通过程序模拟用户 在系统界面点击确认

a. 在程序第一次运行时，注册app成为“设备管理器”

```
private void registerAdmin() {

 DevicePolicyManager mDPM;

        ComponentName mAdminName;

    try {

        // Initiate DevicePolicyManager.

        mDPM = (DevicePolicyManager) getSystemService(Context.DEVICE_POLICY_SERVICE);

        // Set DeviceAdminDemo Receiver for active the component with different option

        mAdminName = new ComponentName(this, DeviceAdmin .class); //根据情况修改

        if (!mDPM.isAdminActive(mAdminName)) {

        // try to become active

        Intent intent = new Intent(DevicePolicyManager.ACTION_ADD_DEVICE_ADMIN);

        intent.putExtra(DevicePolicyManager.EXTRA_DEVICE_ADMIN, mAdminName);

        intent.putExtra(DevicePolicyManager.EXTRA_ADD_EXPLANATION, "Click on Activate button to secure your application.");

        startActivityForResult(intent, 0);

    } else {

    // Already is a device administrator, can do security operations now.

            }

} catch (Exception e) {

            e.printStackTrace();

        }

}


```


b.重写 onActivityResult  函数监听 ：

```
@Override

protected void onActivityResult(int requestCode, int resultCode, Intent data) {

        super.onActivityResult(requestCode, resultCode, data);

 

        if (0 == requestCode) {

if (requestCode == Activity.RESULT_OK) {

    // done with activate to Device Admin

} else {

    // cancle it.

}

}

}


```

c.注册设备管理器接受器，用于监听设备管理器变化。 DeviceAdmin  继承自 DeviceAdminReceiver  ，重写监听动作

```
public class DeviceAdmin extends DeviceAdminReceiver {

 

@Override

public void onReceive(Context context, Intent intent) {

super.onReceive(context, intent);

}

 

public void onEnabled(Context context, Intent intent) {

 

};

 

public void onDisabled(Context context, Intent intent) {

 

};

 

@Override

public CharSequence onDisableRequested(Context context, Intent intent) {

 

return null;

}

 

}

```

d.AndroidManifest.xml  增加配置信息：

```

<receiver

            android:name="com.example.demo.DeviceAdmin"

            android:permission="android.permission.BIND_DEVICE_ADMIN" >

             <meta-data

                android:name="android.app.device_admin"

                android:resource="@xml/my_admin" />

 

            <intent-filter>

                <action android:name="android.app.action.DEVICE_ADMIN_ENABLED" />

            </intent-filter>

 </receiver>
```

e. 创建/res/xml/my_admin

###补充： 

1.经过不完全测试，小米/华为部分机型，可以在Luncher 直接拖动卸载，不会触发“设备管理器”监测，此方式无效。

2.根据已知取消设备管理存在漏洞，可以一定程度 阻止用户取消激活设备管理器 。

参见：[http://blog.csdn.net/androidsecurity/article/details/39962571 ](http://blog.csdn.net/androidsecurity/article/details/39962571 )

以上方式APP一旦注册成功很难卸载，不推荐使用。



### 将App激活成 "设备管理器"，结果以及操作流程如下：
          
1. 需要注册 receiver ，继承自 DeviceAdminReceiver，用来负责监听设备管理器开关以及注册的事件的响应（如：强制锁屏、清除数据，停用相机等等事件）

2. 添加需要管理的用户手机策略事件：（也可以不注册）

```
        <device-admin xmlns:android="http://schemas.android.com/apk/res/android">
                <uses-policies>
                        <limit-password />    设置密码规则
                        <watch-login />        监视屏幕解锁尝试次数
                        <reset-password />   重置密码
                        <force-lock />           强制锁屏
                        <wipe-data />
                        <expire-password />
                        <encrypted-storage />
                        <disable-camera />
                        <disable-keyguard-features />
                </uses-policies>
          </device-admin>
          
```



3.添加Intent，引导用户打开激活设备管理器的界面

```
                Intent intent = new Intent(DevicePolicyManager.ACTION_ADD_DEVICE_ADMIN); 
                intent.putExtra(DevicePolicyManager.EXTRA_DEVICE_ADMIN,  demoDeviceAdmin);
                intent.putExtra(DevicePolicyManager.EXTRA_ADD_EXPLANATION,  "Your boss told you to do this");
                startActivityForResult(intent, ACTIVATION_REQUEST);
                
```
                
会根据你在第2步中注册的事件，显示对应的权限，需要用户主动点击 激活 按钮，才能将App激活成 "设备管理器"

![](./res/activeapp.jpg)
                

4 当点击 激活 以后，就会将该注册为 "设备管理器"，去到系统设置页面：  设置--安全--设备管理器，可以查看所有注册为设备管理器的App

![](./res/activeapp1.jpg)
                    

5 当尝试卸载该App时，系统会弹框提示当前App被激活成了 "设备管理器"，无法直接卸载，需要先去管理设备器页面，取消激活才行

![](./res/activeapp2.jpg)
                

6 点击 管理设备管理器 ，进入相应应用，点击取消激活，再次尝试卸载该应用，既能正常卸载
                
![](./res/activeapp3.jpg)


             
             
###  结论：
 
 要将App激活成 “设备管理器”，只能通过提示用户去激活打开，上述第三步，没办法直接打开激活开关，当App被激活后，在卸载时系统会提示当前App被注册成了 "设备管理器"，需要先取消激活，才能正常卸载
 
###  影响：
 
 1. 要将App激活成 “设备管理器”，只能通过提示用户去激活打开，没办法通过程序直接激活
                          
 2. 一旦被注册成设备管理器，用户卸载时难度会增大，会对我们应用口碑造成影响
                          
 3. 目前安全软件均增加了检测 第三方app 是否注册了该权限，很容易被扫描出来当作 病毒、流氓 软件处理，风险极大
Android 获取应用安装结果
1.背景

一直以来，除了部分使用PC连接助手和root用户外，Android系统应用安装都是交给系统PacakageInstaller。而对于安装结果，我们都是通过注册ACTION_PACKAGE_ADDED来感知的。但是，这就有一个问题，我们只能知道安装成功的消息，却不知道是安装失败还是用户取消。不利于我们对用户行为的分析，以及对我们分发策略的调整。

2.三种应用安装方式

Android 系统的应用安装是由系统pm完成，一般有三种方法可以完成应用的安装，一是root装，直接提权使用系统pm进行安装；二是adb安装，adb的权限比较高，但是需要用户开启开发者模式；第三就是普通的应用安装，应用层的系统安装都会交给PackageInstall去完成。
Pm.png

其中,、**root安装**和**adb安装**是直接使用系统pm进行安装，因此，可以准确获取到安装是成功、失败，还是用户取消，而且原因获取也相对准确，但是由于系统安全性的提升，现在root用户越来越少，使用adb安装的也是极少数.两种安装方式分别为：

Pm install -r $APK_APTH
adb install -r $APK_APTH
普通安装则是委托系统安装器PackageInstaller来完成安装，PackageInstaller是系统应用，需要安装者发送安装事件广播通知系统安装器来安装目标应用。

Intent intent = new Intent(Intent.ACTION_VIEW);
intent.addFlags(Intent.FLAG_ACTIVITY_NEW_TASK);
intent.setDataAndType(Uri.fromFile($APK_PATH),"application/vnd.android.package-archive");
startActivity(intent);
3.获取安装结果

3.1root安装和adb命令安装

这两张都是Pm直接安装，Pm会直接返回安装结果，因此这两种安装结果最准确，但是由于Android权限的收紧，用处越来越小。

优点：返回数据最精准
缺点：用户量极少

3.2startActivityForResult

startActivityForResult是另一种获取安装结果的方法，是具体方法如下：

private void sendInstallEvent() {
  Intent intent = new Intent(Intent.ACTION_VIEW);
  intent.addFlags(Intent.FLAG_ACTIVITY_NEW_TASK);
  intent.putExtra(Intent.EXTRA_RETURN_RESULT, true);
  intent.setDataAndType(Uri.fromFile($APK_PATH),"application/vnd.android.package-archive");
  startActivityForResult(intent, REQUEST_CODE);
}

@Override
protected void onActivityResult(int requestCode, int resultCode, Intent data) {
    super.onActivityResult(requestCode, resultCode, data);
    if (requestCode == REQUEST_CODE) {
    //handle install event
  }
}
接下来，我们来看看Android系统是如何给我们返回安装信息的，由于源码PackageInstaller在几个版本都有部分修改，这里展示的是7.0的源码，当安装广播发送时，首先是唤起PackageInstallActivity安装页，这个时候用户可以选择取消安装，安装结果则会通过setPmResult返回给调用者，如果用户继续安装，则经过一系列操作后，吊起InstallAppProgress展示安装过程。
具体过程我就不一一张开细说，直接上源码，大家有兴趣可以直接看安装源码。
首先是PackageInstallActivity中的setPmResult

void setPmResult(int pmResult) {
  Intent result = new Intent();
  result.putExtra(Intent.EXTRA_INSTALL_RESULT, pmResult);
  setResult(pmResult == PackageManager.INSTALL_SUCCEEDE
         ? RESULT_OK : RESULT_FIRST_USER, result);
}
接着是InstallAppProgress，这个在接受来自PackageManagerService的消息后，通过Handler处理安装消息，如果发起EXTRA_RETURN_RESULT为true，则会返回结果。并且将自己结束掉。

private Handler mHandler = new Handler() {
   void handleMessage(Message msg) {
       switch (msg.what) {
           case INSTALL_COMPLETE:
               if (getIntent().getBooleanExtra(Intent.EXTRA_RETURN_RESULT, false)) {
                   Intent result = new Intent();
                   result.putExtra(Intent.EXTRA_INSTALL_RESULT, msg.arg1);
                   setResult(msg.arg1 == PackageInstaller.STATUS_SUCCESS
                           ? Activity.RESULT_OK : Activity.RESULT_FIRST_USER,
                                   result);
                   clearCachedApkIfNeededAndFinish();
                   return;
               }
               ......
        }
    }
};
通过以上过程，我们可以在发起系统安装时可以获取到安装结果，但是同时也有缺点，就是这种方式是可以获取到安装结果，但是同时也执行了clearCachedApkIfNeededAndFinish(),也就是说，不会有安装结果页，对于一个应用分发应用，没有安装完成页意味着激活量会打打折扣，而且体验不好。所以接下来，需要自定义安装完成页。根据安装返回码，判断成功或者失败，给出相应的安装完成页。
以下给出PP助手的安装完成页：

installfinish.gif

这个方法虽然可以获取安装结果，而且可以通过安装完成页做二次分发，但是由于部分系统修改了PackageInstaller，像华为、OPPO、VIVO等机子，甚至做了安装拦截，其中一些机子的安装拦截是可以绕过的，但是一旦使用安装完成页，这些绕过就会失效。而YUNOS系统则因为之前有bug，导致通过startActivityForResult启动崩溃(YUNOS同学确认该问题已修复)。

优点：可以获取安装结果，而且可以带二次分发。
缺点：由于各家rom定制，需要一定的适配，有些rom不一定返回正确的结果。

3.3 Android 7.0以后系统广播

在Android 7.0之前，InstallAppProgress中是通过binder获取安装结果，所以其他app想要获取到安装结果，只能又InstallAppProgress返回给我们，即上面的方法2，而在7.0上，系统该用广播做通知。我们先来比较一下源码：
6.0的安装消息源码

class PackageInstallObserver extends IPackageInstallObserver.Stub {
    public void packageInstalled(String packageName, int returnCode) {
        Message msg = mHandler.obtainMessage(INSTALL_COMPLETE);
        msg.arg1 = returnCode;
        mHandler.sendMessage(msg);
    }
}
7.0的安装消息源码

private final BroadcastReceiver mBroadcastReceiver = new BroadcastReceiver() {
    @Override
    public void onReceive(Context context, Intent intent) {
        final int statusCode = intent.getIntExtra(
                PackageInstaller.EXTRA_STATUS, PackageInstaller.STATUS_FAILURE);
        if (statusCode == PackageInstaller.STATUS_PENDING_USER_ACTION) {
            context.startActivity((Intent)intent.getParcelableExtra(Intent.EXTRA_INTENT));
        } else {
            onPackageInstalled(statusCode);
        }
    }
};
看完这源码，大家应该都会想到一个问题，那是不是我们将自己注册为广播接受者，也能接收到广播了呢？答案是Yes。可能google意识到太多厂商更改了PackageInstaller，导致应用分发商无法拿到正确的安装事件吧。瞎猜的>_<!!
接下来，我们注册一个接收事件：

private static final String BROADCAST_ACTION = "com.android.packageinstaller.ACTION_INSTALL_COMMIT";
private static final String BROADCAST_SENDER_PERMISSION = "android.permission.INSTALL_PACKAGES";

public InstallStateReceiver(Context context) {
    IntentFilter intentFilter = new IntentFilter();
    intentFilter.addAction(BROADCAST_ACTION);
    context.registerReceiver(
        mBroadcastReceiver, intentFilter, BROADCAST_SENDER_PERMISSION, null /*scheduler*/);
}
如上所述，在7.0以上系统，我们就可以收到应用安装事件了，而且，只要进程一直活在，就可以一直接收安装事件，当然，通过pm安装的除外。

优点：安装结果返回精准，被厂商修改的可能性比较小
缺点：1. 部分机型返回结果不包含包名，因此批量安装结果获取有影响 2.只对7.0以上机型生效

4.总结

安装作为一个应用分发最重要的功能，长期以来都是扔给系统安装器，发起安装后，缺乏对安装过程的跟踪，通过对上述各种方法的综合使用，PP助手已经初步收集了一些用户安装的情况，并且发现了一些安装过程的问题。这些问题都可以反过来初始化我们把安装流程做到更好。同时对安装这件事的认识也更加深刻。

5.备注

关于安装结果的更多问题，可以联系 @善师 @玄五，欢迎一起研究，探讨。
# Android 悬浮窗的实现

桌面悬浮窗任务主要分为两个方面：
在桌面显示悬浮窗
悬浮窗桌面只在桌面应用出现，在其他应用中是隐藏的
一、在桌面上显示
要实现系统级悬浮窗样式，必须不依赖于我们的Activity，要做到这一点，我首先想到的Service 和 WindowManager，大概思路是，在应用启动的时候，创建一个service，在service中用WindowManager创建一个系统级的窗口，让他显示在左面上，service加入保活控制，确保他被销毁后可以立刻重新启动，这样就保证了窗口常驻在桌面上。
接着我们继续想想怎么实现这个功能，首先，我们要了解怎么用WindowManager把窗口搞到桌面上去：
1、首先，我们必须拿到一个系统级的WindowManager
     (WindowManager) getSystemService(WINDOW_SERVICE)
2、往WindowManager中添加一个View，这个view就是我们要显示的弹窗
     WindowManager.addView(FloatView, WindowManager.LayoutParams)
3、在步骤2，我们看到往WindowManager中添加View的时候，需要设置一个WindowManager.LayoutPramas，这个LayoutPramas是一个极其重要的参数，他不仅决定了这个View的位置，大小等，还决定了这个View是在应用层面还是系统层面：
首先来看一下LayoutParmas的各个类型，具体详见Android文档：https://developer.android.com/reference/android/view/WindowManager.LayoutParams.html
WindowManager.LayoutParams.type 弹窗窗口类型分为以下三种主要类型：
•	Applicationwindows：
     取值在 FIRST_APPLICATION_WINDOW 和 LAST_APPLICATION_WINDOW 之间。是通常的、顶层的应用程序窗口。必须将 token 设置成 activity 的 token 。
•	Sub_windows：
     取值在 FIRST_SUB_WINDOW 和 LAST_SUB_WINDOW 之间。与顶层窗口相关联，token 必须设置为它所附着的宿主窗口的 token。
•	Systemwindows：
     取值在 FIRST_SYSTEM_WINDOW 和 LAST_SYSTEM_WINDOW 之间。用于特定的系统功能。它不能用于应用程序，使用时需要特殊权限。
经过上面的分析，可以知道，要让悬浮窗显示在Application以外，那么只能使用SystemWindows模式，加载成系统特定功能，我选出了以下三个：
•	TYPE_SYSTEM_ALERT(系统窗口，需要权限 <uses-permission android:name="android.permission.SYSTEM_ALERT_WINDOW" />)
•	TYPE_TOAST(不需要申请权限)
•	TYPE_SYSTEM_OVERLAY (系统窗口，在锁屏之上，需要权限 <uses-permission android:name="android.permission.SYSTEM_ALERT_WINDOW" />)

通过比较以上三个，优先考虑的是Toast类型的，因为不需要申请额外的权限，打开的时候不收权限控制，是最优选择，接着是TYPE_SYSTEM_ALERT，而TYPE_SYSTEM_OVERLAY 会显示在锁屏之上，而且会影响屏幕解锁，因此不考虑。但是经过测试发现，TYPE_TOAST在API <=18的时候，Toast是不可点击的，于是查看了源码，发现在API <=18的情况下，系统默认将Toast设置为不可点击：
 public void adjustWindowParamsLw(WindowManager.LayoutParams attrs) {
        switch (attrs.type) {
            case TYPE_SYSTEM_OVERLAY:
            case TYPE_SECURE_SYSTEM_OVERLAY:
                // These types of windows can't receive input events.
               attrs.flags |= WindowManager.LayoutParams.FLAG_NOT_FOCUSABLE
                        | WindowManager.LayoutParams.FLAG_NOT_TOUCHABLE;
                attrs.flags &= ~WindowManager.LayoutParams.FLAG_WATCH_OUTSIDE_TOUCH;
                break;
       }
} 

解决方案：
  因此，Toast方案不能使用，必须使用TYPE_SYSTEM_ALERT，但是这个又需要权限，但是这里有一个巧合的是，Android是在5.0，即API = 19 的时候才收紧了权限的限制，也就是说，如果在API <= 18的时候，使用TYPE_SYSTEM_ALERT ，在API > 18的时候，就可以避过需要手动设置权限才能显示的情况。

在桌面应用显示，在其他应用里边关闭

情况分析：
  这个过程就比较复杂了，由于Android系统并没有给我们Android任务栈的变化情况的方法回调，也就是没有人会告诉我们Android任务栈的变化，那如何拿到任务栈变化呢，我们又不是输入法，如果是输入法的话，也是可以通过输入法服务拿到当前所在的应用的。幸运的是，Android并没有把我们的权限给封死，在Android4.4及一下，我们是可以直接通过任务栈的，而在5.0系统上，任务栈已经拿不到了，但是，却有了一个叫 “有权查看使用权限的应用”的东西，就是得到用户的最近使用的任务情况，通过这个东西，我们也可以拿到栈顶应用，只不过这个需要权限，如果用户不给授权，那我们也无法获取到。
可能出现的问题：
假设用户授权，即我们可以拿到栈顶元素了，那先在就有一个问题摆在我们面前了，虽然我们能够拿到栈顶应用，但是这不是系统通知给我们的，而是我们主动去拿的，也就是说，要实现这个功能，我们必须每时每刻的去检查任务栈有没有变化，那等于我们的应用在后台一直在跑东西，会造成耗电量增大。
竞品的解决方案：
  既然这样，那我就去看看竞品是怎么做到，所以反编译了360 和百度手机助手，360手机助手的命名，好吧，找不到关键节点，于是找到了百度手机助手，发现百度手机助手有一个服务叫 FloatService,很显然这就是来跑悬浮窗的，接着继续跟，发现里边有个线程叫FloatViewStateMonitor，还有一个管理列叫FloatManager，这个FloatViewStateMonitor是用来监听任务栈状态的，接着就行找代码，结果发现了,百度也是用一个循环去检测任务栈变化的。具体代码在：
while (true)
{
  if (!bool4)
    this.a.stopService(new Intent(this.a, FloatService.class));
  return;
  bool1 = false;
  break;
  label250: FloatManager.a(this.a).g();
  break label199;
  label263: if (!this.e)
  {
    FloatManager.a(this.a).g();
    FloatManager.a(this.a).l();
    FloatManager.a(this.a).p();
  }
}
解决方案：
在Android 4.4以下下的机子，可以通过ActivityManager类中中getRunningTasks（）获取栈顶元素，Android 5.0及以上的机子，通过有权查看使用权限的应用接口获得最近使用的栈顶元素。
通过主动轮询的方式，去检查用户是否在桌面上，如果是，则显示出来。

存在问题：
因为Android权限的问题，5.0以上如果用户不给授权，则无法判断当前应用，因此在这种情况下取消轮询，直接在桌面上显示，用户开关关闭则不显示。

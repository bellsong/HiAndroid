# Something about Android View

[强烈建议！让你的团队强制推行ConstraintLayout！])(https://cloud.tencent.com/developer/article/1355429)


Android中加载View是从Activity的OnCreate方法调用setContentView开始。那View的具体加载过程是怎么样的？

追踪一下代码：
####Activity：
```java
    /**
     * Set the activity content from a layout resource.  The resource will be
     * inflated, adding all top-level views to the activity.
     *
     * @param layoutResID Resource ID to be inflated.
     *
     * @see #setContentView(android.view.View)
     * @see #setContentView(android.view.View, android.view.ViewGroup.LayoutParams)
     */
    public void setContentView(int layoutResID) {
        getWindow().setContentView(layoutResID);
        initWindowDecorActionBar();
    }
    
        /**
     * Retrieve the current {@link android.view.Window} for the activity.
     * This can be used to directly access parts of the Window API that
     * are not available through Activity/Screen.
     *
     * @return Window The current window, or null if the activity is not
     *         visual.
     */
    public Window getWindow() {
        return mWindow;
    }
    
```

再找一下mWindow是那里初始化的：

```java
    final void attach(Context context, ....) {
        attachBaseContext(context);

        mFragments.attachActivity(this, mContainer, null);

        mWindow = PolicyManager.makeNewWindow(this);
        
    }

```

Activity在调用onCreate()之前会掉用attach()初始化mWindow,目前先不管attach()是谁调用以及怎么调用，只是分析一下View的加载流程。下面是
####PolicyManager

```java
// sPolicy为Policy对象，实现了接口IPolicy  
    public static Window makeNewWindow(Context context) {  
        return sPolicy.makeNewWindow(context);  
    } 
```

再看看Policy类中的代码

```java
// 这里就是返回了一个PhoneWindow对象  
public PhoneWindow makeNewWindow(Context context) {  
        return new PhoneWindow(context);  
    }  
    
```

从而可知 Activity中的setContentView 最终调用的是PhoneWindow类中的 setContentView. 


```java
@Override  
   public void setContentView(int layoutResID) {  
       if (mContentParent == null) {  
           installDecor();  
       } else {  
           mContentParent.removeAllViews();  
       }  
       mLayoutInflater.inflate(layoutResID, mContentParent);  
       final Callback cb = getCallback();  
       if (cb != null && !isDestroyed()) {  
           cb.onContentChanged();  
       }  
   }  


```

installDecor()初始化了DecorView、mContentParent还有title（3.0以后的ActionBar）。DecorView是继承自FrameLayout的PhoneWindow的内部类。
installDecor()中的代码：

```java
if (mContentParent == null) {  
            mContentParent = generateLayout(mDecor); 
```

再看generateLayout：

```java
protected ViewGroup generateLayout(DecorView decor) {  
  
       View in = mLayoutInflater.inflate(layoutResource, null);  
       decor.addView(in, new ViewGroup.LayoutParams(MATCH_PARENT, MATCH_PARENT));  
  
 }  


```

从上面的代码看出，加载的视图添加到了DecorView上，这样Activitty加载视图的过程就完成了。试图加载过程中出现了Activity、Window、View。

>Activity是Android应用程序的载体，允许用户在其上创建一个用户界面，并提供用户处理事件的API，如onKeyEvent, onTouchEvent等， 并维护应用程序的生命周期。
>
>每一个Activity组件都有一个关联的Window对象，用来描述一个应用程序窗口。每一个应用程序窗口内部又包含有一个View（DecorView）对象，用来描述应用程序窗口的视图。
>
>应用程序窗口视图是真正用来实现UI内容和布局的，也就是说，每一个Activity组件的UI内容和布局都是通过与其所关联的一个Window对象的内部的一个View对象来实现的。

![时序图](./res/screenshot 2016-07-15 15.54.42.png)


##Tips
Q：如何getContentView()

```java
private View getContentView(){
        ViewGroup view = (ViewGroup) getWindow().getDecorView();
        FrameLayout content = (FrameLayout) view.getChildAt(0);
        return content.getChildAt(0);
    }
```



### 设置View的id
API 17以上时，可以直接使用 generateViewId() 获得，且可以得到不重复的ID。

当API小于17时：  可以直接使用此方法
```java
    private static final AtomicInteger sNextGeneratedId = new AtomicInteger(1);  
public static int generateViewId() {  
    for (;;) {  
        final int result = sNextGeneratedId.get();  
        // aapt-generated IDs have the high byte nonzero; clamp to the range under that.  
        int newValue = result + 1;  
        if (newValue > 0x00FFFFFF) newValue = 1; // Roll over to 1, not 0.  
        if (sNextGeneratedId.compareAndSet(result, newValue)) {  
            return result;  
        }  
    }  
}  

```

##参考资料
* [张兴业](http://blog.csdn.net/xyz_lmn)

* [View的加载过程](http://www.cnblogs.com/xyzlmn/p/3641702.html)

* [View原理](http://www.jianshu.com/p/17b372ef3f41)
* [教你实现一个具备展开折叠功能的TextView](http://www.jianshu.com/p/317b118dd2d7)

* [把RecyclerView撸成 马 蜂 窝](http://www.jianshu.com/p/6c78a5a07db5)

* [Vector](http://www.jianshu.com/p/e3614e7abc03)

* [屏幕尺寸，分辨率，像素，PPI之间到底什么关系？](http://www.jianshu.com/p/c3387bcc4f6e)

* [自定义View](http://www.jianshu.com/p/c84693096e41)

* [Android 的窗口管理系统 (View, Canvas, WindowManager)](http://www.cnblogs.com/samchen2009/p/3367496.html)

* [Android重写view时onAttachedToWindow () 和 onDetachedFromWindow ()](http://blog.csdn.net/eyu8874521/article/details/8493995)

* [Android 屏幕元素层次结构](http://www.uml.org.cn/jmshj/201303261.asp)

* [Android 分享一个流量显示界面](http://blog.csdn.net/wangjinyu501/article/details/39527021)

* [Android 仿美团网，探索使用ViewPager+GridView实现左右滑动查看更多分类的功能](http://blog.csdn.net/qq_20785431/article/details/52528404)

* [Andorid Drawable玩法](https://blog.csdn.net/lmj623565791/article/details/43752383)

* [Fragment](./android_fragment.md)

* [ListView](./android_listview.md)

* [RecyclerView](http://antonioleiva.com/recyclerview-listener/)

* [Bitmap](./android_view_bitmap.md)

* [ImageView](./android_view_imageview.md)

* [ViewStub](./android_viewstub.md)

* [AndroidCircularSeekBar](https://github.com/RaghavSood/AndroidCircularSeekBar)


* [软键盘](./android_keyboard.md)

* [Toast](./android_toast.md)

* [UI专题](http://dev.10086.cn/cmdn/bbs/viewthread.php?tid=18736&page=1#pid89255)

* TextView

  [获取TextView显示的字符串宽度](http://2kpurple.github.io/2014/11/02/get-text-view-text-width/)

[Android中View的绘制流程](https://gold.xitu.io/post/58aaac80b123db00671da58a?utm_source=gold_browser_extension)

[自定义View系列：打造一个显示密码等级的控件](https://philipdroid.github.io/2016/10/26/%E4%B8%80%E4%B8%AA%E6%98%BE%E7%A4%BA%E5%AF%86%E7%A0%81%E7%AD%89%E7%BA%A7%E7%9A%84%E6%8E%A7%E4%BB%B6PasswordLevelView/)

[图解 View 测量、布局及绘制原理](https://juejin.im/entry/58d37fb861ff4b006cb6670a)
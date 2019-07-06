# Activity的启动模式了解

## Activity四种启动模式

## Activity启动分析

1. 入口。

以前一直都说Activity的人口是onCreate方法。其实Android上一个应用的入口，应该是ActivityThread。和普通的java类一样，入口是一个main方法。

```java
public static final void main(String[] args) {
        SamplingProfilerIntegration.start();
       ……
        Looper.prepareMainLooper();
        if (sMainThreadHandler == null) {
            sMainThreadHandler = new Handler();
        }
        ActivityThread thread = new ActivityThread();
        thread.attach(false);
       ……
        Looper.loop();
       ……
        thread.detach();
        ……
        Slog.i(TAG, "Main thread of " + name + " is now exiting");
    }
```

下面仔细分析一下这个main方法。

2. Looper.prepareMainLooper();

ActivityThread其实就是我们经常说的UI thread，也就是主线程。我们都知道主线程可以使用Handler进行异步通信，因为主线程中已经创建了Looper，而这个Looper就是在这里创建的。如果其他线程需要使用Handler通信，就要自己去创建Looper。


3. sMainThreadHandler = new Handler();

创建一个Handler。


4. ActivityThread thread = new ActivityThread();

创建ActivityThread 对象。

ActivityThread 有几个比较重要的成员变量，会在创建ActivityThread对象时初始化。

（1）final ApplicationThread mAppThread = new ApplicationThread();

ApplicationThread继承自ApplicationThreadNative， 而ApplicationThreadNative又继承自Binder并实现了IApplicationThread接口。IApplicationThread继承自IInterface。这是一个很明显的binder结构，用于于Ams通信。IApplicationThread接口定义了对一个程序（linux的进程）操作的接口。ApplicationThread通过binder与Ams通信，并将Ams的调用，通过下面的H类（也就是Hnalder）将消息发送到消息队列，然后进行相应的操作，入activity的start， stop。
（2）final H mH = new H();
private final class H extends Handler
mH负责处理ApplicationThread发送到消息队列的消息，例如：

```java
public void handleMessage(Message msg) {
            if (DEBUG_MESSAGES) Slog.v(TAG, ">>> handling: " + msg.what);
            switch (msg.what) {
                case LAUNCH_ACTIVITY: {
                    ActivityClientRecord r = (ActivityClientRecord)msg.obj;
r.packageInfo = getPackageInfoNoCheck(
                            r.activityInfo.applicationInfo);
                    handleLaunchActivity(r, null);
                } break;
                
```

5. handleLaunchActivity(r, null);

从名字中就可以看出，这里就将进行启动activity的工作了。

方法中主要调用了这一句：

Activity a = performLaunchActivity(r, customIntent);


6. performLaunchActivity（）

进行了一些初始化和赋值操作后，创建activity。

activity = mInstrumentation.newActivity(
                    cl, component.getClassName(), r.intent);

然后调用：

mInstrumentation.callActivityOnCreate(activity, r.state);

这一句就会调用到acitivity的onCreate方法了，就进入了大多数应用开发的入口了。


##Activity启动
Activity由ActivityThread负责启动。 
    ActivityThread.java

``` java    
    private final Activity performLaunchActivity(ActivityClientRecord r, Intent customIntent) {
        Activity activity = null;
        try {
            java.lang.ClassLoader cl = r.packageInfo.getClassLoader();
            activity = mInstrumentation.newActivity(
                    cl, component.getClassName(), r.intent);
            r.intent.setExtrasClassLoader(cl);
            if (r.state != null) {
                r.state.setClassLoader(cl);
            }
        } catch (Exception e) {
            if (!mInstrumentation.onException(activity, e)) {
                throw new RuntimeException(
                    "Unable to instantiate activity " + component
                    + ": " + e.toString(), e);
            }
        }
		return activity;
    }
    
```

二。调用Activity的attach方法。
    ActivityThread.java
 
 ```java   
    private final Activity performLaunchActivity(ActivityClientRecord r, Intent customIntent) {
        ContextImpl appContext = new ContextImpl();
        appContext.init(r.packageInfo, r.token, this);
        appContext.setOuterContext(activity);
        CharSequence title = r.activityInfo.loadLabel(appContext.getPackageManager());
        Configuration config = new Configuration(mConfiguration);
        if (DEBUG_CONFIGURATION) Slog.v(TAG, "Launching activity "
                + r.activityInfo.name + " with config " + config);
        activity.attach(appContext, this, getInstrumentation(), r.token,
                r.ident, app, r.intent, r.activityInfo, title, r.parent,
                r.embeddedID, r.lastNonConfigurationInstance,
                r.lastNonConfigurationChildInstances, config);
    }
 ```

三。Activity的attach实现
    1.attach的实现
        Activity.java //Activity implement Window.Callback
 
 ```java       
    final void attach(Context context, ActivityThread aThread,
                Instrumentation instr, IBinder token, int ident,
                Application application, Intent intent, ActivityInfo info,
                CharSequence title, Activity parent, String id,
                Object lastNonConfigurationInstance,
                HashMap<String,Object> lastNonConfigurationChildInstances,
                Configuration config) {
            attachBaseContext(context); //ContextThemeWrapper中实现，赋值给mBase
				mWindow = PolicyManager.makeNewWindow(this); //创建window，实际上是一个PhoneWindow对象
            mWindow.setCallback(this); //设置Window.Callback，因为Activity implement Window.Callback    
            mWindow.setWindowManager(null, mToken, mComponent.flattenToString());
            mWindowManager = mWindow.getWindowManager();
        }
```        


2.PolicyManager.makeNewWindow实现    
 (1).PolicyManager.java
    
 ```java       
 private static final String POLICY_IMPL_CLASS_NAME =
            "com.android.internal.policy.impl.Policy";
        private static final IPolicy sPolicy;
        static {
            // Pull in the actual implementation of the policy at run-time
            Class policyClass = Class.forName(POLICY_IMPL_CLASS_NAME);
            sPolicy = (IPolicy)policyClass.newInstance();
        }
        
  public static Window makeNewWindow(Context context) {
            return sPolicy.makeNewWindow(context); 
        }        
        (2).Policy.java //Policy implements IPolicy
        public PhoneWindow makeNewWindow(Context context) {
            return new PhoneWindow(context);
        }
  ```
  
        
3.设置WindowManager
        mWindow.setWindowManager(null, mToken, mComponent.flattenToString()); //为Window中的WindowManager赋值
        mWindowManager = mWindow.getWindowManager(); //为Acitivity中的WindowManager赋值
        
        WindowManager只是一个interface，实现类有两个：Window.LocalWindowManager和WindowManagerImpl。
        (1).Window.LocalWindowManager实现，只是对WindowManagerImpl的封装调用。
        private class LocalWindowManager implements WindowManager {
            private final WindowManager mWindowManager;
            LocalWindowManager(WindowManager wm) {
                mWindowManager = wm;
            }
            public final void addView(View view, ViewGroup.LayoutParams params) {
                //some code to check params here
                mWindowManager.addView(view, params);
            }
            public void updateViewLayout(View view, ViewGroup.LayoutParams params) {
                mWindowManager.updateViewLayout(view, params);
            }
            public final void removeView(View view) {
                mWindowManager.removeView(view);
            }
            public final void removeViewImmediate(View view) {
                mWindowManager.removeViewImmediate(view);
            }
        }
        (2).Window.setWindowManager实现分析
        public void setWindowManager(WindowManager wm, IBinder appToken, String appName) {
            if (wm == null) {
                wm = WindowManagerImpl.getDefault();
            }
            mWindowManager = new LocalWindowManager(wm);
        }

四。回调Activity的onCreate方法。
    (1). ActivityThread.java
    private final Activity performLaunchActivity(ActivityClientRecord r, Intent customIntent) {
        mInstrumentation.callActivityOnCreate(activity, r.state);
    }
    (2). Instrumentation.java
    public void callActivityOnCreate(Activity activity, Bundle icicle) {
        activity.onCreate(icicle);
    }

五。Activity中setContentView(int layout)分析
    Activity.java
    public void setContentView(int layoutResID) {
        getWindow().setContentView(layoutResID); 
    }
    
    public Window getWindow() {
        return mWindow; //mWindow = PolicyManager.makeNewWindow(this);
    }
    
六。Window中setContentView(int layout)分析
    (1)PhoneWindow.java //因为Window是个abstract class， mWindow实际上是一个PhoneWindow对象
    public void setContentView(int layoutResID) {
        if (mContentParent == null) {
            installDecor();
        } else {
            mContentParent.removeAllViews();
        }
        mLayoutInflater.inflate(layoutResID, mContentParent);
        final Callback cb = getCallback();
        if (cb != null) {
            cb.onContentChanged();
        }
    }
    (2) installDecor()分析 //PhoneWindow.java中
    private void installDecor() {
        if (mDecor == null) { 
            mDecor = generateDecor();
        }
        if (mContentParent == null) {
            mContentParent = generateLayout(mDecor);
        }
    }
    mDecor： class DecorView extends FrameLayout
    mContentParent： class ViewGroup
    (3)generateDecor()分析
    protected DecorView generateDecor() {
        return new DecorView(getContext(), -1);
    }
    DecorView:继承自FrameLayout，可以理解成窗口修饰，这个窗口修饰可以有各种style，比如标题栏，显示进度条等。常见的窗口修饰的layout路径为：frameworks/base/core/res/res/layout，比如R.layout.dialog_title_icons, R.layout.screen_title_icons.
    (4)generateLayout()分析
    protected ViewGroup generateLayout(DecorView decor) {
        // Inflate the window decor.
        int layoutResource;
        if ((features & ((1 << FEATURE_LEFT_ICON) | (1 << FEATURE_RIGHT_ICON))) != 0) {
            if (mIsFloating) {
                layoutResource = com.android.internal.R.layout.dialog_title_icons;
            } else {
                layoutResource = com.android.internal.R.layout.screen_title_icons;
            }
        } else if ((features & ((1 << FEATURE_PROGRESS) | (1 << FEATURE_INDETERMINATE_PROGRESS))) != 0) {
            layoutResource = com.android.internal.R.layout.screen_progress;
        } else if ((features & (1 << FEATURE_CUSTOM_TITLE)) != 0) {
            if (mIsFloating) {
                layoutResource = com.android.internal.R.layout.dialog_custom_title;
            } else {
                layoutResource = com.android.internal.R.layout.screen_custom_title;
            }
        } else if ((features & (1 << FEATURE_NO_TITLE)) == 0) {
            if (mIsFloating) {
                layoutResource = com.android.internal.R.layout.dialog_title;
            } else {
                layoutResource = com.android.internal.R.layout.screen_title;
            }
        } else {
            layoutResource = com.android.internal.R.layout.screen_simple;
        }

        View in = mLayoutInflater.inflate(layoutResource, null);
        decor.addView(in, new ViewGroup.LayoutParams(MATCH_PARENT, MATCH_PARENT));

        ViewGroup contentParent = (ViewGroup)findViewById(ID_ANDROID_CONTENT);
        if (contentParent == null) {
            throw new RuntimeException("Window couldn't find content container view");
        }
        return contentParent;
    }
    DecorView的样式的定义：第一种是在Activity中onCreate中调用requestFeature（）。另一种是在AndroidManifest.xml中配置android:theme="xxx".
    DecorView添加View：generateLayout的前部分代码，就是确定DecorView的样式，然后inflate这个layout文件，再调用DecorView.addView()添加到DecorView上。
    mContentParent赋值：窗口修饰的layout中必须有一个FrameLayout，id为ID_ANDROID_CONTENT（实际上id=content），将这个FrameLayout赋值给mContentParent。
    (5)添加窗口内容到mContentParent。
    mLayoutInflater.inflate(layoutResID, mContentParent);
    将activity传入的layoutRes添加到窗口装饰中。
    (6)Window.Callback回调
    final Callback cb = getCallback(); //Activity实现了Window.Callback接口，并且在创建PhoneWindow后，调用mWindow.setCallback(this)
    if (cb != null) {
        cb.onContentChanged();
    }
   
七。通知WmS，显示DecorView
    Activity准备好后会通知AmS，AmS通过一些条件判断，回调Activity的makeVisible().
    (1.) Activity.java
    public void setVisible(boolean visible) {
        if (mVisibleFromClient != visible) {
            mVisibleFromClient = visible;
            if (mVisibleFromServer) {
                if (visible) makeVisible();
                else mDecor.setVisibility(View.INVISIBLE);
            }
        }
    }
    void makeVisible() {
        if (!mWindowAdded) {
            ViewManager wm = getWindowManager();
            wm.addView(mDecor, getWindow().getAttributes()); //通过WindowManager将DecorView添加到window。
            mWindowAdded = true;
        }
        mDecor.setVisibility(View.VISIBLE);
    }
    public Window getWindow() {
        return mWindow;
    }
    (2.) Window.java
    public final WindowManager.LayoutParams getAttributes() {
        return mWindowAttributes;
    }
    private final WindowManager.LayoutParams mWindowAttributes =
        new WindowManager.LayoutParams();
    (3.) WindowManager.LayoutParams
    public LayoutParams() {
        super(LayoutParams.MATCH_PARENT, LayoutParams.MATCH_PARENT);
        type = TYPE_APPLICATION;
        format = PixelFormat.OPAQUE;
    }
    (4.)Window.LocalWindowManager
    wm.addView(mDecor, getWindow().getAttributes())分析
    这个wm是LocalWindowManager，是对WindowManagerImpl的封装，目地是对params进行一些校验后，再调用WindowManagerImpl的addView。
    校验params：
    if (wp.type >= WindowManager.LayoutParams.FIRST_SUB_WINDOW &&
        wp.type <= WindowManager.LayoutParams.LAST_SUB_WINDOW) {
    } else {
    }
    
八。WindowManagerImpl.addView分析
    方法：addView(View view, ViewGroup.LayoutParams params, boolean nest)；
    class WindowManagerImpl extends WindowManager， interface WindowManager extends ViewManager
    3个成员变量：
    private View[] mViews;//每个view对象都将成为WmS所认为的一个窗口
    private ViewRoot[] mRoots;//每个view对应一个ViewRoot
    private WindowManager.LayoutParams[] mParams;//对应mViews的每个view的param
    (1.)检查view是否已经添加过，不允许重复添加
        int index = findViewLocked(view, false);
        if (index >= 0) {
            if (!nest) {
                throw new IllegalStateException("View " + view
                        + " has already been added to the window manager.");
            }
            root = mRoots[index];
            root.mAddNesting++;
            // Update layout parameters.
            view.setLayoutParams(wparams);
            root.setLayoutParams(wparams, true);
            return;
        }
    (2.)检查窗口类型是否为sub window。如果是，则找到它的父窗口，并保存在临时变量panelParentView中，为下面调用ViewRoot的setView使用。
        if (wparams.type >= WindowManager.LayoutParams.FIRST_SUB_WINDOW &&
                wparams.type <= WindowManager.LayoutParams.LAST_SUB_WINDOW) {
            final int count = mViews != null ? mViews.length : 0;
            for (int i=0; i<count; i++) {
                if (mRoots[i].mWindow.asBinder() == wparams.token) {
                    panelParentView = mViews[i];
                }
            }
        }
    (3.)创建一个新的ViewRoot，上文说过每个view都对应一个ViewRoot。
        root = new ViewRoot(view.getContext());
        root.mAddNesting = 1;
        ViewRoot的构造方法：
        public ViewRoot(Context context) {
            super();

            // Initialize the statics when this class is first instantiated. This is
            // done here instead of in the static block because Zygote does not
            // allow the spawning of threads.
            getWindowSession(context.getMainLooper()); //init static IWindowSession sWindowSession;
            
            mThread = Thread.currentThread();
            mLocation = new WindowLeaked(null);
            mLocation.fillInStackTrace();
            mWidth = -1;
            mHeight = -1;
            mDirty = new Rect();
            mTempRect = new Rect();
            mVisRect = new Rect();
            mWinFrame = new Rect();
            mWindow = new W(this, context); // class W extends IWindow.Stub
            mInputMethodCallback = new InputMethodCallback(this);
            mViewVisibility = View.GONE;
            mTransparentRegion = new Region();
            mPreviousTransparentRegion = new Region();
            mFirst = true; // true for the first time the view is added
            mAdded = false;
            mAttachInfo = new View.AttachInfo(sWindowSession, mWindow, this, this);
            mViewConfiguration = ViewConfiguration.get(context);
            mDensity = context.getResources().getDisplayMetrics().densityDpi;
        }
    (4.)将view，root和param添加到上面的3个数组中。
        if (mViews == null) {
            index = 1;
            mViews = new View[1];
            mRoots = new ViewRoot[1];
            mParams = new WindowManager.LayoutParams[1];
        } else {
            index = mViews.length + 1;
            Object[] old = mViews; //保存原来的数组
            mViews = new View[index]; //新建一个数组，长度+1
            System.arraycopy(old, 0, mViews, 0, index-1); //将原来的数组copy到新建的数组中
            old = mRoots;
            mRoots = new ViewRoot[index];
            System.arraycopy(old, 0, mRoots, 0, index-1);
            old = mParams;
            mParams = new WindowManager.LayoutParams[index];
            System.arraycopy(old, 0, mParams, 0, index-1);
        }
        index--;
        mViews[index] = view;
        mRoots[index] = root;
        mParams[index] = wparams;
    (5.)调用ViewRoot的setView方法，完成最后的添加工作。

九。ViewRoot的setView分析
    参数意义：
    view： WindowManagerImpl中mViews数组中的元素，也就是新建的窗口界面
    attrs：窗口参数，描述窗口的风格，大小，位置。attrs中的token变量指明了该窗口和activity的关系。
    panelParentView：view的父窗口
    public void setView(View view, WindowManager.LayoutParams attrs, View panelParentView) {
        synchronized (this) {
            if (mView == null) {
                mView = view;//给成员变量赋值。
                mWindowAttributes.copyFrom(attrs);//给成员变量赋值。
                attrs = mWindowAttributes;//给成员变量赋值。
                ………………………………
                mSoftInputMode = attrs.softInputMode;//给成员变量赋值。
                mWindowAttributesChanged = true;//给成员变量赋值。
                mAttachInfo.mRootView = view;//给成员变量赋值。
                if (panelParentView != null) {
                    mAttachInfo.mPanelParentWindowToken
                            = panelParentView.getApplicationWindowToken();
                }
                ………………………………
                requestLayout(); //发出重绘请求，使该 在相应消息前变的可见
                ………………………………
                try {
                    //通知WmS，添加窗口
                    res = sWindowSession.add(mWindow, mWindowAttributes,
                            getHostVisibility(), mAttachInfo.mContentInsets,
                            mInputChannel);
                } catch (RemoteException e) {
                } 
            }
        }
    }
    
十。sWindowSession.add分析
    （1）IWindowSession是一个aidl接口，实现类是在WmS中：class Session extends IWindowSession.Stub
    （2）sWindowSession的初始化： //static IWindowSession sWindowSession;
        public static IWindowSession getWindowSession(Looper mainLooper) {
            synchronized (mStaticInit) {
                if (!mInitialized) {
                    try {
                        InputMethodManager imm = InputMethodManager.getInstance(mainLooper);
                        sWindowSession = IWindowManager.Stub.asInterface(
                                ServiceManager.getService("window")) //先获得WmS
                                .openSession(imm.getClient(), imm.getInputContext()); //再通过WmS获取分配的IWindowSession
                        mInitialized = true;
                    } catch (RemoteException e) {
                    }
                }
                return sWindowSession;
            }
        }
    （3）sWindowSession为static，WmS为每个进程只分配1个。调用sWindowSession.add是app请求WmS添加窗口的唯一入口。
    
        
总结：至此，从客户端的角度讲，已经完成了窗口创建的全部工作。

## 参考
[Android：launchmode（Activity的四种启动模式介绍）](https://blog.csdn.net/u012538947/article/details/39647119)
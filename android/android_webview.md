Android Webview

[WebView·开车指南](https://jiandanxinli.github.io/2016-08-31.html?hmsr=toutiao.io&utm_medium=toutiao.io&utm_source=toutiao.io)

[android WebView全面总结](http://www.jcodecraeer.com/a/anzhuokaifa/androidkaifa/2013/1010/1569.html)

[Android开发HTML5应用](http://www.cnblogs.com/GhostHorse/archive/2013/01/30/Android_HTML5_WebView.html)

[WebView Cache 缓存清除](http://blog.csdn.net/moubenmao_jun/article/details/17078269)

[Android WebView常见问题及解决方案汇总](http://blog.csdn.net/t12x3456/article/details/13769731)

[android webview中调用了js的时候混淆注意事项](http://blog.csdn.net/minenamewj/article/details/40112335)

[Android中webview和js之间的交互](http://www.cnblogs.com/leizhenzi/archive/2011/06/29/2093636.html)

[Android WebView性能优化](http://motalks.cn/2016/09/11/Android-WebView-JavaScript-3/)

##常见BUG及解决方案

```
    java.lang.IllegalArgumentException: Service not registered: android.speech.tts.TextToSpeech$Connection@41519670
       at android.app.LoadedApk.forgetServiceDispatcher(LoadedApk.java:937)
       at android.app.ContextImpl.unbindService(ContextImpl.java:1521)
       at android.content.ContextWrapper.unbindService(ContextWrapper.java:490)
       at android.speech.tts.TextToSpeech$Connection.disconnect(TextToSpeech.java:1321)
       at android.speech.tts.TextToSpeech$1.run(TextToSpeech.java:661)
       at android.speech.tts.TextToSpeech$1.run(TextToSpeech.java:656)
       at android.speech.tts.TextToSpeech$Connection.runAction(TextToSpeech.java:1340)
       at android.speech.tts.TextToSpeech.runAction(TextToSpeech.java:570)
       at android.speech.tts.TextToSpeech.runActionNoReconnect(TextToSpeech.java:557)
       at android.speech.tts.TextToSpeech.shutdown(TextToSpeech.java:656)
       at android.webkit.AccessibilityInjector$TextToSpeechWrapper.shutdown(AccessibilityInjector.java:748)
       at android.webkit.AccessibilityInjector.destroy(AccessibilityInjector.java:187)
       at android.webkit.WebViewClassic.destroyJava(WebViewClassic.java:2618)
       at android.webkit.WebViewClassic.destroy(WebViewClassic.java:2601)
       at android.webkit.WebView.destroy(WebView.java:665)
       at com.douban.dongxi.app.BrowserPurchaseActvitiy$5.run(BrowserPurchaseActvitiy.java:197)
       at android.os.Handler.handleCallback(Handler.java:725)
       at android.os.Handler.dispatchMessage(Handler.java:92)
       at android.os.Looper.loop(Looper.java:137)
       at android.app.ActivityThread.main(ActivityThread.java:5105)
       at java.lang.reflect.Method.invokeNative(Method.java)
       at java.lang.reflect.Method.invoke(Method.java:511)
       at com.android.internal.os.ZygoteInit$MethodAndArgsCaller.run(ZygoteInit.java:793)
       at com.android.internal.os.ZygoteInit.main(ZygoteInit.java:560)
       at dalvik.system.NativeStart.main(NativeStart.java)

```

One Method from [stackoverflow](http://stackoverflow.com/questions/27740935/service-not-registered-android-speech-tts-texttospeech)


Have to move all webview from the xml file and have added them dynamically and passing application context.


    WebView webView = new WebView(getApplicationContext());
Use following code in OnDestory() method.

```
  @Override
        protected void onDestroy() {
           clearHistory();
             long timeout = ViewConfiguration.getZoomControlsTimeout();
             new Timer().schedule(new TimerTask() {
                     @Override
                      public void run() {
                            destroyWebView(mWebView);
                      }
              }, timeout);
        }

      void clearHistory() {
        if (mWebView != null) {
            runOnUiThread(new Runnable() {
                @Override
                public void run() {
                    mWebView.clearHistory();
                }
            });
        }

      private void destroyWebView(final WebView webView) {
          if (webView != null) {
            try {
                mWebViewContainer.removeView(webView);

                if (webView != null) {
                    runOnUiThread(new Runnable() {
                        @Override
                        public void run() {
                            webView.stopLoading();
                            webView.removeAllViews();
                            webView.clearCache(true);
                            webView.destroyDrawingCache();
                            webView.destroy();
                        }
                    });
                }
            } catch (Exception e) {
                e.printStackTrace();
            }
        }

```


Two things should be noticed :

all operations related with Webview should be on the UIThread
webview-destroy operations should happen after removed it from the container view.



## BUG
android webview ZoomButtonsController 导致android.view.WindowLeaked 问题彻底解决

Activity has leaked window android.widget.ZoomButtonsController that was originally added here android.view.WindowLeaked:

引起这个错误的原因是：

发现是webview的 ZoomButton，也就是那两个放大和缩小的按钮，导致的。如果设置为让他们出现，并且可以自动隐藏，那么，由于他们的自动隐藏是一个渐变的过程，所以在逐渐消失的过程中如果调用了父容器的destroy方法，就会导致Leaked。

所以解决方案是，在destroy之前，先让他俩立马消失。

至于立马消失的方法，网上大多抄来抄去，没有效果。

我的解决办法是，在finish掉此activity时，把子view 全部remove掉。理论上说，只需要remove这个zoom view就可以，但是我没找到获取该view的办法，只好remove掉所有的子view。这样在activity destroy时就不会报 WindowLeaked的错误了。

```

@Override
    public void finish() {
        ViewGroup view = (ViewGroup) getWindow().getDecorView();
        view.removeAllViews();
        super.finish();
    }
```
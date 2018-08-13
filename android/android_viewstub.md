[ViewStub](http://www.cnblogs.com/plokmju/p/android_ViewStub.html)


[布局文件延迟加载控件ViewStub](http://www.blogjava.net/gaolei-xj/archive/2013/02/17/395348.html)

[使用惰性控件ViewStub实现布局动态加载](http://www.cnblogs.com/menlsh/archive/2013/03/17/2965217.html)

ViewStub可以用来实现局部页面跳转的功能，让一些View先隐藏，点击后可显示，view显示后有一些点击操作，我想获得他的ontouch事件进行点击，但直接设置view.setOnClickListener，没反应，经过几番周折发现它有个setOnInflateListener，它是用来监听ViewStub Inflate后的操作，把对ViewStub的操作放到这里面即可，个人判断，若ViewStub里有一些控件需要捕获并设置监听，也是需要在这里写。

```java
viewStub.setOnInflateListener(new OnInflateListener() {  
              
            @Override  
            public void onInflate(ViewStub stub, View inflated) {  
                // TODO Auto-generated method stub  
                Log.v("~~~~viewStub.setOnInflateListener~~~~~~", "zhixing");  
                  
                inflated.setOnTouchListener(new OnTouchListener() {  
                      
                    @Override  
                    public boolean onTouch(View v, MotionEvent event) {  
                        // TODO Auto-generated method stub  
                        Log.v("~~~~viewStub.setOnInflateLi22stener~~~~~~", "zhi22xing:"+event.getY());  
                        return false;  
                    }  
                });       
            }  
        });  

```
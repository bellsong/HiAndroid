[Android 高清加载巨图方案 拒绝压缩图片](http://blog.csdn.net/lmj623565791/article/details/49300989)


解决Android解析图片的OOM问题
用BitmapFactory这里的各种Decode方法，如果图片很小的话，不会出现oom,但是当图片很大的时候
就要用BitmapFactory.Options这个东东了,

options.inJustDecodeBounds = false/true;  
//图片压缩比例.  
options.inSampleSize = ssize;  

[android图像处理系列之七－－图片涂鸦，水印－图片叠加](http://blog.csdn.net/sjf0115/article/details/7267028)

[Android实战经验之图像处理及特效处理的集锦（总结版）](http://www.oschina.net/question/231733_44154)

[BigImageViewer](https://www.diycode.cc/projects/Piasy/BigImageViewer)
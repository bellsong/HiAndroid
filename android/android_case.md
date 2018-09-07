### 图片颜色提取

#### 背景
#### 目标
#### 原理
#### 实践
#### 参考
#### 知识点

#### 背景
    通过筛选图片的主色调来显示如红色系，蓝色系的照片。这里涉及到图片主色调的提取。Android 采用的是 Google 官方推出的 Palette 算法。（其他还有什么算法？）

##### Google Palette 算法
    Palette算法是Android Lillipop中新增的特性。它可以从一张图中提取主要颜色，然后把提取的颜色融入的App的UI之中。现在在很多设计出彩的泛前端展示届非常普遍，如知乎等。Palette算法分析，相比于很多传统的图片提取算法，Palette的特点是不单单是去筛选中出现颜色最多的。而是从使用角度出发，通过六种模式，如活力色彩，柔和色彩等，筛选出更符合人眼筛选视觉焦点的颜色。如夜晚中的霓虹灯，白色背景的产品照。同时，也可以自定义筛选模式，输入自己的筛选规则，得到目标颜色。

作者：唐笛_Dylan
链接：https://www.jianshu.com/p/a67a6d1b1844
來源：简书
简书著作权归作者所有，任何形式的转载都请联系作者获得授权并注明出处。

#### 目标
    实现一个通过摄像头获取主色调的功能。

#### 原理
#### 实践
1. Android Lollipop开始引入，使用时需要添加 Support 库

> compile 'com.android.support:palette-v7:21.0.0'

#### 参考
[android 调用Camera，获取预览帧中的图像](https://blog.csdn.net/u013869488/article/details/49853217)

[Camera API](https://developer.android.com/guide/topics/media/camera#java)

[Android 分别使用 SurfaceView 和 TextureView 来预览 Camera，获取NV21数据](https://rustfisher.github.io/2018/02/26/Android_note/Android-camera_nv21_surfaceview_textureview/)

[Android Camera SurfaceView 预览拍照](https://www.jianshu.com/p/9e0f3fc5a3b4)

[Android 上的调色板 —— Palette](https://www.jianshu.com/p/430274ade74f)

[GlidePalette](https://github.com/florent37/GlidePalette)

[Android 使用Palette获取Gallery图片主色调](https://blog.csdn.net/zhoumushui/article/details/70143462)

[MaterialDesignDemo](https://github.com/loonggg/MaterialDesignDemo)

[Android Lollipop：使用Palette抽取图片主色调](https://www.jianshu.com/p/4dc897dc5354)

[cameraview](https://github.com/google/cameraview)

[Android Camera踩坑详解](https://hakuless.github.io/2017/12/12/Android-Camera%E8%B8%A9%E5%9D%91%E8%AF%A6%E8%A7%A3/)

#### 知识点
1. Camera / android.hardware.camera2(API 21)
2. SurfaceView
3. Palette

[理解JobScheduler机制](http://gityuan.com/2017/03/10/job_scheduler_service/)
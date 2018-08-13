### Android 资源相关

资源 Id简介：
我们平时获取资源是通过 findViewById 方法进行的，比如我们常在onCreate方法中使用这样的语句：
btnChecked=(ImageView)findViewById(R.id.imgCheck);
findViewById是我们获取layout中各种View 对象比如按钮、标签、ListView和ImageView的便利方法。顾名思义，它需要一个int参数：资源id。
资源id非常有用。Android回自动为每个位于res目录下的资源分配id，包括各种图片文件、xml文中的”@+id”对象。res的子目录几乎总是固定的，比如每次都能见到的：drawable-xxxx、layout、values，以及不常见的：anim、xml、row、color。
Android会为res目录下的所有资源分配id，其主要的分配原则是：
drawable中的图片文件总是每个文件一个资源id。
Xml文件中每个使用android:id＝”@+id/xxx”的view都会被分配一个未用的资源id。


Android使用getIdentifier()方法根据资源名来获取资源id

       //获取布局文件资源的ID
        int layoutId = getResources().getIdentifier("activity_main", "layout", getPackageName());
        Log.d(TAG, "----> 获取到的图片资源 drawableId= " + layoutId);

        //获取图片资源的ID
        mImageView = (ImageView) findViewById(R.id.imageView);
        int drawableId = getResources().getIdentifier("oyp", "drawable", getPackageName());
        mImageView.setImageResource(drawableId);
        Log.d(TAG, "----> 获取到的图片资源 drawableId=" + drawableId);


        mipmapImageView = (ImageView) findViewById(R.id.mipmapImageView);
        int mipmapId = getResources().getIdentifier("ic_launcher", "mipmap", getPackageName());
        mipmapImageView.setImageResource(mipmapId);
        Log.d(TAG, "----> 获取到的图片资源 mipmapId=" + mipmapId);

        //获取字符串资源
        mTextView = (TextView) findViewById(R.id.textView);
        int stringId = getResources().getIdentifier("author", "string", getPackageName());
        mTextView.setText(stringId);
        Log.d(TAG, "----> 获取到的字符串资源 stringId=" + stringId);


### 旁门左道
主要由两种方法，个人建议第二种。

1. 不把图片放在res/drawable下，而是存放在src某个package中（如：com.drawable.resource），这种情况下的调用方法为：
String path = "com/drawable/resource/imageName.png";
InputStream is = getClassLoader().getResourceAsStream(path);
Drawable.createFromStream(is, "src");


> 封装类详见 ResourceUtil
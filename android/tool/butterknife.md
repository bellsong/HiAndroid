# Android Butterknife（黄油刀） 使用

## 简介
ButterKnife是一个专注于Android系统的View注入框架,以前总是要写很多findViewById来找到View对象，有了ButterKnife可以很轻松的省去这些步骤。是大神JakeWharton的力作，目前使用很广。最重要的一点，使用ButterKnife对性能基本没有损失，因为ButterKnife用到的注解并不是在运行时反射的，而是在编译的时候生成新的class。项目集成起来也是特别方便，使用起来也是特别简单。

## [ButterKnife项目地址](https://github.com/JakeWharton/butterknife)

## 优势

* 强大的View绑定和Click事件处理，简化代码，提升开发效率
* 运行时不影响APP效率
* 代码清晰，可读性强
* 使用方便

## 使用

1、在Activity 类中绑定 ：ButterKnife.bind(this);必须在setContentView();之后绑定；且父类bind绑定后，子类不需要再bind。

2、在非Activity 类（eg：Fragment、ViewHold）中绑定： ButterKnife.bind(this，view);这里的this不能替换成getActivity（）。

3、在Activity中不需要做解绑操作，在Fragment 中必须在onDestroyView()中做解绑操作。

4、使用ButterKnife修饰的方法和控件，不能用private or static 修饰，否则会报错。错误: @BindView fields must not be private or static. (com.zyj.wifi.ButterknifeActivity.button1)

5、setContentView()不能通过注解实现。（其他的有些注解框架可以）

6、使用Activity为根视图绑定任意对象时，如果你使用类似MVC的设计模式你可以在Activity 调用ButterKnife.bind(this, activity)，来绑定Controller。

7、使用ButterKnife.bind(this，view)绑定一个view的子节点字段。如果你在子View的布局里或者自定义view的构造方法里 使用了inflate,你可以立刻调用此方法。或者，从XML inflate来的自定义view类型可以在onFinishInflate回调方法中使用它。
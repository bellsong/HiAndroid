### 前言
刚要用到反射的知识，来整理一下，留个脚印。

### 内容
* 反射原理是什么？
* 为什么要用到反射？
* 调用反射的优缺点是什么？
* 反射的使用方法
* 反射的局限性


### 反射原理
    Java反射机制是指在运行状态中，对于任意一个类，都能够知道这个类的所有属性和方法；对于任意一个对象，都能够调用它的任意一个方法和属性；这种动态获取的信息以及动态调用对象的方法的功能称为java语言的反射机制。
用一句话总结就是反射可以实现在运行时可以知道任意一个类的属性和方法。

### 为什么要用到反射
    为什么要用反射机制？直接创建对象不就可以了吗，这就涉及到了动态与静态的概念

静态编译：在编译时确定类型，绑定对象,即通过。
动态编译：运行时确定类型，绑定对象。动态编译最大限度发挥了java的灵活性，体现了多态的应用，有以降低类之间的藕合性。

### 调用反射的优缺点
优点
可以实现动态创建对象和编译，体现出很大的灵活性，特别是在J2EE的开发中它的灵活性就表现的十分明显。比如，一个大型的软件，不可能一次就把把它设计的很完美，当这个程序编译后，发布了，当发现需要更新某些功能时，我们不可能要用户把以前的卸载，再重新安装新的版本，假如这样的话，这个软件肯定是没有多少人用的。采用静态的话，需要把整个程序重新编译一次才可以实现功能的更新，而采用反射机制的话，它就可以不用卸载，只需要在运行时才动态的创建和编译，就可以实现该功能。

缺点
对性能有影响。使用反射基本上是一种解释操作，我们可以告诉JVM，我们希望做什么并且它满足我们的要求。这类操作总是慢于只直接执行相同的操作。

### 反射使用方法

获取成员方法Method
获取成员变量Field
获取构造函数Constructor

1、知道需要反射的类名，方法名，参数等；

2、构造反射对象 (无参构造和有参构造方法)
无参构造方法：
> Class clazz = Class.forName("className");
> Object object = clazz.newInstance();

有参构造方法
> Class clazz = Class.forName("className");
> Constructor constructor = clazz.getConstructor(String.class);
> Object object = constructor.newInstance("Y");

3、获取公有字段
> Filed filed = clazz.getField("age");

4、获取私有字段
> Filed filed = clazz.getDecleardField("age");

5、修改私有字段
>            Field field = clazz.getDeclaredField("age");
>            field.setAccessible(true);
>            Constructor constructor = clazz.getConstructor(int.class, String.class);
>            Object object = constructor.newInstance(18, "Y");
>            Object name = field.get(object);
>            field.set(object, "bell");

6、使用反射记得加try catch

```java
        //反射一个public方法
        try {
            String className = "xxxx";
            Class clazz = Class.forName(className);
            Object object = clazz.newInstance();
            Method method = clazz.getMethod("onCreate", new Class[]{Bundle.class});
            method.invoke(object, new Object[]{savedInstanceState});
        } catch (Exception var3) {
            this.finish();
        }

```

### 反射的局限性

### 参考
[谈谈JAVA/Android中的反射原理](http://www.jianshu.com/p/6abb938263fc)
[Android 极简反射教程](http://www.jianshu.com/p/4ef846c0b10d)
[java反射详解](http://www.cnblogs.com/rollenholt/archive/2011/09/02/2163758.html)
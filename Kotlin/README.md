Kotlin

# What Kotilin

Kotlin是什么，它的诞生背景是什么，能解决什么样的问题

关于Kotilin，它是由Jetbrains开发的，Google后来把它作为Android开发的官方语言。

官网最显眼的部分写的是：

“A modern programming language that makes developers happier.”

一种能让开发者更开心的现代编程语言。不是开心，是更开心，听上去是不是有点让人有兴趣，毕竟这个年头让人开心的东西不过了，让人更开心那就凤毛麟角了。继续往下，看看Kotlin是不是让开发者更开心，怎么让开发者更开心。

# Why Kotlin

### Modern，concise and safe programming language

### A productive way to write server‑side applications

### Natural way to share code between mobile platforms （KMM）

### Big, friendly and helpful community
    
    Kotlin生来得到Google爸爸支持，开发者生态社区活跃

# Kotlin有什么用，能解决什么样问题，比起其他语言优缺点

# Kotlin学习路径，基础语法，开发工具

## kotlin语法
### 1. 程序入口
跟Java里面main函数一样，Koltin的入口也是main函数

    fun main() {
        println("Hello world!")
    }

另外一个接收参数的main函数

    fun main() {
        prinln("Hi kotlin")
    }

### 2. 函数声明
跟Java 函数参数里面 先声明类型，再声明变量不一样，Kotlin的函数里面参数是先声明变量，再指定类型
    例如，一个求和函数声明如下：

    fun sum(a:Int, b:Int):Int{
        return a+b
    }

另外，返回值类型也是跟着函数名字后面，用：分隔。还有一个细节不知道注意没有，Kotlin语法里结尾是不需要分号的。爱惜键盘按键。

甚至，可以直接写成

    fun sum(a:Int, b:Int) = a+b

### 3. 没有返回值的函数声明 Unit

    fun printSum(a:Int, b:Int):Unit {
        println("sum of $a and $b is ${a+b}")
    }

### 4.变量声明
常量声明，一旦用了val，就无法修改改变量，类似Java中final

    val a : Int = 1;
    val b = 2
    val c : Int
        c = 3

变量声明 var

    var x = 4
    x = 5
    x += 1



### 5.类和实例
类声明 class
    例如:

        class Rectangle(var height:Double, var width:Double) {
            var perimeter = (height + width) * 2
        }

### 6.继承

一个类可以被继承需要用关键字open 修饰

    open class Shape
    class Rectangle(var height:Double, var width:Double):Shape(){
        var perimeter = (height + width) * 2
    }

### 7.字符串

    fun main() {
        var a = 1
        val S1 = "a is $a"

        a = 2
        val s2 = "${s1.replace("is", "was")}, but now is $a"
        println(s2)
    }

结果：a was 1, but now is 2

### 8.条件语句

    fun maxOf(a:Int, b:Int) {
        if(a > b) {
            return a
        } else {
            return b
        }
    }

或者是

    fun maxOf(a:Int, b:Int) = if (a > b) a else b

    fun main(){
        println("max of 0 and 42 is ${maxOf(0,42)}")
    }

### 9. for 循环

    val items = listOf("apple", "banana", "kiwifruit")
    for (item in items) {
        println(item)
    }

    val items = listOf("apple", "banana", "kiwifruit")
    for (index in items.indices) {
        println("item at $index is ${items[index]}")
    }

### 10，while 循环

    val items = listOf("apple", "banana", "kiwifruit")
    var index = 0
    while (index < items.size) {
        println("item at $index is ${items[index]}")
        index++
    }

### 11.when 
类似于Java里面switch

    fun describe(obj: Any): String =
    when (obj) {
        1          -> "One"
        "Hello"    -> "Greeting"
        is Long    -> "Long"
        !is String -> "Not a string"
        else       -> "Unknown"
    }

### 12.范围Ranges﻿

    val x = 10
    val y = 9
    if (x in 1..y+1) {
        println("fits in range")
    }

### 13.集合

    fun main() {
        val items = listOf("apple", "banana", "kiwifruit")
        for (item in items) {
            println(item)
        }
    }

### 14.Nullable 和空值检测

    fun parseInt(str:String):Int? {
        //...
    }

例如：

    fun printProduct(arg1: String, arg2: String) {
        val x = parseInt(arg1)
        val y = parseInt(arg2)

        // Using `x * y` yields error because they may hold nulls.
        if (x != null && y != null) {
            // x and y are automatically cast to non-nullable after null check
            println(x * y)
        }
        else {
            println("'$arg1' or '$arg2' is not a number")
        }    
    }

### 15.类型检测 is

    fun getStringLength(obj: Any): Int? {
        if (obj is String) {
            // `obj` is automatically cast to `String` in this branch
            return obj.length
        }

        // `obj` is still of type `Any` outside of the type-checked branch
        return null
    }

或者

    fun getStringLength(obj: Any): Int? {
        if (obj !is String) return null

        // `obj` is automatically cast to `String` in this branch
        return obj.length
    }

又或者

    fun getStringLength(obj: Any): Int? {
        // `obj` is automatically cast to `String` on the right-hand side of `&&`
        if (obj is String && obj.length > 0) {
            return obj.length
        }

        return null
    }

# More
[Kotlin官网](https://kotlinlang.org/)

[Kotlin GitHub地址](https://github.com/JetBrains/kotlin)

[The Kotlin Blog](https://blog.jetbrains.com/kotlin/)

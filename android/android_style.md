# 编码注意事项
     
       为了更好地提高项目的编码质量，提升程序的性能，大家可以关注一下以下的这些注意事项，

一、Java相关
1.类中尽量少用全局的static成员变量，它会一直占用内存空间，浪费内存。

2.尽量少用单例模式，除非需要管理和共享的数据涉及到程序全局的，如全局
的监听器、缓存。

3.当要用到字符串连接的情况时，尽量使用StringBuilder来进行处理，避免连接
操作时JVM new一些无用的String对象出来，不仅要花时间生成对象，以后可能
还需花时间对这些对象进行垃圾回收和处理。同时也可以直接指定stringBuilder
的容量，避免在容量不够的时候自动增长，以提高程序性能。


4.在使用完I/O流、cursor、数据库等的时候应该及时关闭，避免内存泄露。
（一般是在try/catch块中的finally里面进行关闭）

5.尽量减少对变量的重复计算、函数的重复调用
如for循环
 
6.慎用异常，抛出异常首先要创建一个新的对象。Throwable接口的构造函数
调用名为fillInStackTrace()的本地（Native）方法，fillInStackTrace()方法检查
堆栈，收集调用跟踪信息。只要有异常被抛出，VM就必须调整调用堆栈，因
为在处理过程中创建了一个新的对象。
 
 
7.可用移位来代替乘除法，提高程序性能。


8.涉及到多线程同步场景时，使用AtomicInteger来进行计数（里面提供了原子性的int类型增减操作）。
 
 
9.避免static final来定义没有被外部调用的常量，只需定义final即可。

 
10.当我们对序列化进行控制时，可能某个特定子对象不想让Java序列化机制自动保存与恢复。如果子对象表示的
     是我们不希望将其序列化的敏感信息（如密码），通常会面临这种情况。即使对象中的这些信息是private属性，
     一经序列化处理，人们就可以通过读取文件或者拦截网络传输的方式来访问到它。
     这个时候，就可以使用transient关键字逐个字段地关闭序列化，transient关键字只能和Serializable对象一起使用。

二、Android相关
1.项目中注册的listener和receiver的，在注册后，需要在页面销毁或者不需要使用的时候把他们给注销掉，避免造成不必要的内存占用和引起一些不可预知的bug。
 
2.耗时的操作尽量都在子线程中进行处理，如文件操作、网络相关操作、Html.fromHtml、数据加密等

3.在程序设计时，可以创建一个manager类来管理共享数据。
 
 
4.使用完bitmap等占用内存比较大的对象时需要及时的释放。
（如在fragment或activity中保存了一个bitmap的引用，在onDestroy（）的时候就需要释放掉）


5.在adapter中，尽量使用自定义viewholder来保存convertview中view的引用，避免多次findviewbyid操作。
 
 
6.项目中，当需要使用到线程的时候，尽量使用线程池。
 
7.使用viewstub进行动态加载，避免在布局中将view先设置为gone再设置为visible。
 
 
8.在使用context时，尽量使用application的context，以免context引用一直无法释放。
 
 
9.尽量避免使用setImageBitmap、setImageResource，因为它们都是通过java层的createBitmap来完成的，需要消耗更多内存，
   改用先通过BitmapFactory.decodeStream方法，创建出一个bitmap，再将其设为ImageView的 source。


10.常用SpannableString，里面提供了许多text相关的效果处理，避免创建一些无谓的view来浪费资源。

三、设计模式相关
1.单例模式
    完整范例：

     a.在写单例模式的时候，必须定义一个私有的构造方法。
 
     b.定义静态的类实例变量的时候，必须加上volatile关键字。
 
volatile关键字：
在并发编程中，我们通常会遇到以下三个问题：原子性问题，可见性问题，有序性问题。
        volatile关键字就是Java用来保证可见性，当一个共享变量被volatile修饰时，它会保证修改的值
会立即被更新到主存，当有其他线程需要读取时，它会去内存（线程工作内存）中读取新值。
一旦一个共享变量（类的成员变量、类的静态成员变量）被volatile修饰之后，那么就具备了两层语义：
　　1）保证了不同线程对这个变量进行操作时的可见性，即一个线程修改了某个变量的值，这新值对其他线程来说是立即可见的。
　　2）禁止进行指令重排序。
（PS：这里说明一下主存和线程工作内存，Java内存模型规定所有的变量都是存在主存当中；每个
线程都有自己的工作内存，线程中执行的写入操作，首先得在自己的工作线程中对变量i所在的缓存
行进行赋值操作，然后再写入主存当中）
相关链接：http://www.cnblogs.com/dolphin0520/p/3920373.html


Android性能优化Tips整理
如何优化
优化本身是一个很大的主题，对于Android平台优化可以为以下几部分：
一是JAVA语法层次通用的优化，如尽量使用局部变量（栈变量），IO缓冲等。
二是通用的Android性能优化，如同步改异步，各种缓存的使用等
三是应用程序内部的性能优化，如内部逻辑、数据插入及查找、数据结构的安排与组织等

一、JAVA语法层次的优化
1. 类和对象使用技巧
1.  尽量少用new生成新对象
2.  使用clone方法生成新对象
3.  尽量使用局部变量栈变量
4.  减少方法调用
5.  使用final类和final/static/private方法
6.  让访问实例内变量的 getter/setter 方法变成final  
7.  避免不需要的 instanceof 操作  
8.  避免不需要的造型操作  
9.  尽量重用对象
10. 不要重复初始化变量  
11. 不要过分创建对象
2. Java IO技巧
1.  使用缓冲提高IO性能
2.  lnputStream比Reader高效，OutputStream比Writer高效
3   在适当的时候用byte替代char
4.  有缓冲的块操作IO要比缓冲的流字符IO快
5.  序列化时使用原子类型
6.  在finally块中关闭stream 
7.  SQL语句
8.  尽早释放资源
3. 异常Exceptions使用技巧
1.  避免使用异常来控制程序流程
2.  尽可能重用异常
3.  将trycatch 块移出循环  
4. 线程使用技巧
1.  在使用大量线程Threading的场合使用线程池管理
2.  防止过多的同步
3.  同步方法而不要同步整个代码段
4.  在追求速度的场合用ArrayList和HashMap代替Vector和Hashtable
5.  使用notify而不是notifyAll
6.  不要在循环中调用 synchronized同步方法   
7.  单线程应尽量使用 HashMap，ArrayList
5. 其它常用技巧
*   使用移位操作替代乘除法操作可以极大地提高性能
*   对Vector中最后位置的添加删除操作要远远快于埘第一个元素的添加删除操作
*   当复制数组时使用System.arraycop方法
*   使用复合赋值运算符
*   用int而不用其它基本类型
*   在进行数据库连接和网络连接时使用连接池
*   用压缩加快网络传输速度一种常用方法是把相关文件打包到一个jar文件中
*   在数据库应用程序中使用批处理功能
*   消除循环体中不必要的代码
*   为vectors 和 hashtables定义初始大小  
*   如果只是查找单个字符的话用charat代替startswith
*   在字符串相加的时候使用 charat()代替startswith() 如果该字符串只有一个字符的话  
*   对于 boolean 值避免不必要的等式判断  
*   对于常量字符串用string 代替 stringbuffer   
*   用stringtokenizer 代替 indexof 和substring  
*   使用条件操作符替代if cond else  结构 
*   不要在循环体中实例化变量  
*   确定 stringbuffer的容量  
*   不要总是使用取反操作符  
*   与一个接口 进行instanceof 操作  
*   采用在需要的时候才开始创建的策略  
*   通过 StringBuffer 的构造函数来设定他的初始化容量可以明显提升性能  
*   合理使用 javautilVector
*   不要将数组声明为public static final
*   HaspMap 的遍历
*   array数组和 ArrayList 的使用  
*   StringBufferStringBuilder 的区别
*   尽量使用基本数据类型代替对象   
*   用简单的数值计算代替复杂的函数计算比如查表方式解决三角函数问题  
*   使用具体类比使用接口效率高但结构弹性降低了但现代 IDE都可以解决这个问题 
*   考虑使用静态方法
*   应尽可能避免使用内在的GET/SET 方法 
*   避免枚举浮点数的使用   
*   二维数组比一维数组占用更多的内存空间大概是 10倍计算 
*   SQLite
*   奇偶判断

二、通用Android性能优化
布局优化
（原文参考：ImprovingLayout Performance）
• 尽量减少Android程序布局中View的层次，View层次越多，效率就越低
• 使用<include/>复用布局
• 使用ViewStub懒加载布局 (TODO:Android布局技巧：使用ViewStub提高UI性能)
• 使用ViewHolder、Thread使ListView滚动更加流畅
其它优化点
• 合理使用异步操作
• 懒加载：当前不需要的数据，不要加载，即按需加载。懒加载的范围是广泛的，可以是数据，可以是View，或者其它
• 使用缓存
• 图片缓存：包括MemoryCache和DiskCache，推荐使用官方DEMO中的Cache
参考：DisplayingBitmaps Efficiently
• 单例数据缓存：建立一个管理数据的类，管理所有数据，当主界面消失后，由于Application本身没有实际退出，因此，数据本身也没有释放掉，下次启动时，省去了加载数据的时间，当然，这并不是一个好的行为。
• 使用ListView、GridView的View缓存
• 使用Message自身的缓存，避免重复创建Message实例
• 线程池
• 数据池（可参考Message Pool的实现方式）
• ……
• 数据库优化
• SQL优化
• 建立索引
• 使用事务
• …...
• 算法优化
• 用快速排序代替冒泡排序
• 用二分查找代替线性查找
• ……
• 数据结构使用
• 不要全部使用ArrayList，合理使用LinkedList等易于插入和删除的集合
• 合理使用HashMap、HashSet来提高查找性能
• 使用SparseArray、SparseIntArray、SparseBooleanArray来替代某些特定的HashMap
• ……
• 其它策略
• 可以考虑延迟处理，避免在同一时间干过多的事情
三、应用程序内部的性能优化
该部分的优化应该是依据程序的不同而不同，没有万般皆准的法则，实际上，上述的性能优化点基本上已经能够解决很多性能问题了。
主要的优化手段有：

• 程序逻辑简化：分析代码，去掉冗余逻辑
• 数据结构的优化：对集合类的灵活使用，特别是HashMap的使用，极大的提高查找性能。
• 批量处理原则：对于需要循环调用地方，采用批量处理

## 参考

* 阿里巴巴编码规范
        
 
 
 

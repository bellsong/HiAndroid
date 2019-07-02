# Java 死锁的理解

## 死锁的定义

    死锁是指两个或者两个以上的进程在执行过程中，由于竞争资源或者由于彼此通信造成的一种阻塞现象，若无外力作用，它们都将无法推进下发。

    竞争的资源可以是：锁，网络连接，通知事件，磁盘，带宽以及一切可以被称作”资源“的东西

## 死锁的例子

public class DeadLockDemo impleement Runnanle {
    public int flag = 1;
    static Object object1 = new Object();
    static Object object2 = new Object();

    public void run(){
        if(flag == 1){
            synchronized(object1){
                try{
                    Thread.sleep(500p);
                } catch(Exception e){
                    e.printtStackTrace();
                }
            }
            synchronized(object2){
                System.out.printlin("1")
            }
        }

        if(flag ==0){
            synchronized(object2){
                try {
                    Thread.sleep(500);
                } catch (InterruptedException e) {
                    e.printStackTrace();
                }
                synchronized (o1){
                    System.out.println("0");
                }
            }
        }
    }

    public static void main(Stringp[] args){
        DesdLockDemo demo1 = new DeadLockDemo();
        DeadLockDemo demo2 = new DeadLockDemo();
        demo1.flag = 1;
        demo2.flag = 0;
        new Thread(t1).start();
        new Thread(t2).start();
    }
}


## 死锁产生的原因

产生死锁的原因主要是：

    系统资源不足
    资源分配不当
    推进顺序不合适

产生死锁的四个必要条件：

    互斥：一个资源每次只能被一个进程使用
    资源保持：一个进程被阻塞时，不释放已有资源
    不可抢占：正在使用的资源不能被剥夺
    循环等待：循环等待未被分配的资源

## 参考

[死锁终极篇](https://juejin.im/post/5aaf6ee76fb9a028d3753534)


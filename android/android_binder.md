# 谈谈Android Binder

一.引言
最近一段时间由于工作，接触到framework部分比较多一点，也难免要和Binder打一些交道，也整理了一些相关知识，但准备写这篇文章时，还是有些慌。而且关于整个Binder机制的复杂程度不是三言两语能描叙清楚的，也害怕自己的理解有些偏差，误导一些朋友（ps:反正也没人看....扎心）所以也参考了很多资料。
本文主要站在Android开发的角度来大致解析下Binder在java层的一些知识原理，给大家脑子形成一个完整的概念，比如AIDL的实现原理，Binder是怎么通信的等等，

二.Binder概述
2.1 Android为什么选择Binder
Android是基于Linux内核的，所以Android要实现进程间的通信，其实大可使用linux原有的一些手段，比如管道，共享内存，socket等方式，但是Android还是采用了Binder作为主要机制，说明Binder具有无可比拟的优势。
其实进程通信大概就两个方面因素，一者性能方面，传输效率问题，传统的管道队列模式采用内存缓冲区的方式，数据先从发送方缓存区拷贝到内核开辟的缓存区中，然后再从内核缓存区拷贝到接收方缓存区，至少有两次拷贝过程，而socket都知道传输效率低，开销大，用于跨网络进程交互比较多，共享内存虽然无需拷贝。
二者这是安全问题，Android作为一个开放式，拥有众多开发者的的平台，应用程序的来源广泛，确保终端安全是非常重要的，传统的IPC通信方式没有任何措施，基本依靠上层协议，其一无法确认对方可靠的身份，Android为每个安装好的应用程序分配了自己的UID，故进程的UID是鉴别进程身份的重要标志，传统的IPC要发送类似的UID也只能放在数据包里，但也容易被拦截，恶意进攻，socket则需要暴露自己的ip和端口，知道这些恶意程序则可以进行任意接入。
综上所述，Android需要一种高效率，安全性高的进程通信方式，也就是Binder，Binder只需要一次拷贝，性能仅次于共享内存，而且采用的传统的C/S结构，稳定性也是没得说，发送添加UID/PID，安全性高。
2.2 Binder实现机制
2.2.1进程隔离
我们知道进程之间是无法直接进行交互的，每个进程独享自己的数据，而且操作系统为了保证自身的安全稳定性，将系统内核空间和用户空间分离开来，保证用户程序进程崩溃时不会影响到整个系统，简单的说就是，内核空间（Kernel）是系统内核运行的空间，用户空间（UserSpace）是用户程序运行的空间。为了保证安全性，它们之间是隔离的，所以用户空间的进程要进行交互需要通过内核空间来驱动整个过程。
2.2.2 C/S结构
Binder是基于C/S机制的，要实现这样的机制，server必须需要有特定的节点来接受到client的请求，也就是入口地址，像输入一个网址，通过DNS解析出对应的ip，然后进行访问，这个就是server提供出来的节点地址，而Binder而言的话，与传统的C/S不太一样，Binder本身来作为Server中提供的节点，client拿到Binder实体对象对应的地址去访问Server，对于client而言，怎么拿到这个地址并建立起整个通道是整个交互的关键所在，而且Binder作为一个Server中的实体，对象提供一系列的方法来实现服务端和客户端之间的请求，只要client拿到这个引用就可以或者一个有着该方法代理对象的引用，就可以进行通信了。
引用Android Bander设计与实现 - 设计篇一段话来总结：
面向对象思想的引入将进程间通信转化为通过对某个Binder对象的引用调用该对象的方法，而其独特之处在于Binder对象是一个可以跨进程引用的对象，它的实体位于一个进程中，而它的引用却遍布于系统的各个进程之中。最诱人的是，这个引用和java里引用一样既可以是强类型，也可以是弱类型，而且可以从一个进程传给其它进程，让大家都能访问同一Server，就象将一个对象或引用赋值给另一个引用一样。Binder模糊了进程边界，淡化了进程间通信过程，整个系统仿佛运行于同一个面向对象的程序之中。形形色色的Binder对象以及星罗棋布的引用仿佛粘接各个应用程序的胶水，这也是Binder在英文里的原意。
2.2.3 Binder通信模型
Binder基于C/S的结构下，定义了4个角色：Server、Client、ServerManager、Binder驱动，其中前三者是在用户空间的，也就是彼此之间无法直接进行交互，Binder驱动是属于内核空间的，属于整个通信的核心，虽然叫驱动，但是实际上和硬件没有太大关系，只是实现的方式和驱动差不多，驱动负责进程之间Binder通信的建立，Binder在进程之间的传递，Binder引用计数管理，数据包在进程之间的传递和交互等一系列底层支持。
ServerManager的作用？
我们知道ServerManager也是属于用户空间的一个进程，主要作用就是作为Server和client的桥梁，client可以ServerManager拿到Server中Binder实体的引用，这么说可能有点模糊，举个简单的例子，我们访问，www.baidu.com，百度首页页面就显示出来了，首先我们知道，这个页面肯定是发布在百度某个服务器上的，DNS通过你这个地址，解析出对应的ip地址，再去访问对应的页面，然后再把数据返回给客户端，完成交互。这个和Binder的C/S非常类似，这里的DNS就是对应的ServerManager，首先，Server中的Binder实体对象，将自己的引用（也就是ip地址）注册到ServerManager，client通过特定的key（也就是百度这个网址）和这个引用进行绑定，ServerManager内部自己维护一个类似MAP的表来一一对应，通过这个key就可以向ServerManager拿到Server中Binder的引用，对应到Android开发中，我们知道很多系统服务都是通过Binder去和AMS进行交互的，比如获取音量服务：
AudioManager am = (AudioManager)context.getSystemService(Context.AUDIO_SERVICE);
细心的朋友应该发现ServerManager和Server也是两个不同的进程呀，Server要向ServerManager去注册不是也要涉及到进程间的通信吗，当前实现进程间通信又要用到进程间的通信，你这不是扯犊子吗....莫急莫急，Binder的巧妙之处在于，当ServerManager作为Serve端的时候，它提供的Binder比较特殊，它没有名字也不需要注册，当一个进程使用BINDER_SET_CONTEXT_MGR命令将自己注册成SMgr时Binder驱动会自动为它创建Binder实体，这个Binder的引用在所有Client中都固定为0而无须通过其它手段获得。也就是说，一个Server若要向ServerManager注册自己Binder就必需通过0这个引用号和ServerManager的Binder通信,有朋友又要问了，server和client属于两个不同的进程，client怎么能拿到server中对象，不妨先看看下面的交互图

从上图很清晰的可以看出来整个的交互过程，原本从SM中拿到binder的引用，通过Binder驱动层的处理之后，返回给了client一个代理对象，实际上如果client和server处于同一个进程，返回的就是当前binder对象，如果client和server不处于同一个进程，返回给client的就是一个代理对象，这一点，有兴趣可以看下源码。
2.2.4 Binder角色的定位
Binder本质上只是提供了一种通信的方式，和我们具体要实现的内容没有关系，为了实现这个服务，我们需要定义一些接口，让client能够远程调用服务，因为是跨进程，这时候就要设计到代理模式，以接口函数位基准，client和server去实现接口函数，Server是服务真正的实现，client作为一个远程的调用。

从Server进程来看，Binder是存在的实体对象，client通过transact()函数，经过Binder驱动，最终回调到Binder实体的onTransact()函数中。
从 Client进程的角度看，Binder 指的是对 Binder 代理对象，是 Binder 实体对象的一个远程代理，通过Binder驱动进行交互

3.手写进程通信
上面说了那么多，大家伙也看累了，下面通过代码的形式让大家对Binder加深点理解，日常开发中，涉及到进程间通信的话，我们首先想到的可能就是AIDL，但不知道有没有和我感觉一样的朋友。。第一次写AIDL是蒙蔽的，通过.aidl文件，编译器自动生成代码，生成一个java文件，里面又有Stub类，里面还有Proxy类，完全不理解里面的机制，确实不便于我们理解学习，为了加深理解，我们抛弃aidl，手写一个通信代码。
首先我们要定义一个接口服务，也就是上述的服务端要具备的能力来提供给客户端,定义一个接口继承IInterface，代表了服务端的能力
public interface PersonManger extends IInterface {
    void addPerson(Person mPerson);
    List<Person> getPersonList();
}
复制代码接下来我们就要定义一个Server中的Binder实体对象了，首先肯定要继承Binder，其次需要实现上面定义好的服务接口
public abstract class BinderObj extends Binder implements PersonManger {
    public static final String DESCRIPTOR = "com.example.taolin.hellobinder";
    public static final int TRANSAVTION_getPerson = IBinder.FIRST_CALL_TRANSACTION;
    public static final int TRANSAVTION_addPerson = IBinder.FIRST_CALL_TRANSACTION + 1;
    public static PersonManger asInterface(IBinder mIBinder){
        IInterface iInterface = mIBinder.queryLocalInterface(DESCRIPTOR);
        if (null!=iInterface&&iInterface instanceof PersonManger){
            return (PersonManger)iInterface;
        }
        return new Proxy(mIBinder);
    }
    @Override
    protected boolean onTransact(int code, @NonNull Parcel data, @Nullable Parcel reply, int flags) throws RemoteException {
        switch (code){
            case INTERFACE_TRANSACTION:
                reply.writeString(DESCRIPTOR);
                return true;

            case TRANSAVTION_getPerson:
                data.enforceInterface(DESCRIPTOR);
                List<Person> result = this.getPersonList();
                reply.writeNoException();
                reply.writeTypedList(result);
                return true;

            case TRANSAVTION_addPerson:
                data.enforceInterface(DESCRIPTOR);
                Person arg0 = null;
                if (data.readInt() != 0) {
                    arg0 = Person.CREATOR.createFromParcel(data);
                }
                this.addPerson(arg0);
                reply.writeNoException();
                return true;
        }
        return super.onTransact(code, data, reply, flags);

    }

    @Override
    public IBinder asBinder() {
        return this;
    }
    
}
复制代码首先我们看asInterface方法，Binder驱动传来的IBinder对象，通过queryLocalInterface方法，查找本地Binder对象，如果返回的就是PersonManger，说明client和server处于同一个进程，直接返回，如果不是，返回给一个代理对象。
当然作为代理对象，也是需要实现服务接口
public class Proxy implements PersonManger {
    private IBinder mIBinder;
    public Proxy(IBinder mIBinder) {
        this.mIBinder =mIBinder;
    }

    @Override
    public void addPerson(Person mPerson) {
        Parcel data = Parcel.obtain();
        Parcel replay = Parcel.obtain();

        try {
            data.writeInterfaceToken(DESCRIPTOR);
            if (mPerson != null) {
                data.writeInt(1);
                mPerson.writeToParcel(data, 0);
            } else {
                data.writeInt(0);
            }
            mIBinder.transact(BinderObj.TRANSAVTION_addPerson, data, replay, 0);
            replay.readException();
        } catch (RemoteException e){
            e.printStackTrace();
        } finally {
            replay.recycle();
            data.recycle();
        }
    }

    @Override
    public List<Person> getPersonList() {
        Parcel data = Parcel.obtain();
        Parcel replay = Parcel.obtain();
        List<Person> result = null;
        try {
            data.writeInterfaceToken(DESCRIPTOR);
            mIBinder.transact(BinderObj.TRANSAVTION_getPerson, data, replay, 0);
            replay.readException();
            result = replay.createTypedArrayList(Person.CREATOR);
        }catch (RemoteException e){
            e.printStackTrace();
        } finally{
            replay.recycle();
            data.recycle();
        }
        return result;
    }

    @Override
    public IBinder asBinder() {
        return null;
    }
}
复制代码这里的代理对象实质就是client最终拿到的代理服务，通过这个就可以和Server进行通信了，首先通过Parcel将数据序列化，然后调用 remote.transact()将方法code，和data传输过去，对应的会回调在在Server中的onTransact()中
然后是我们的Server进程,onBind方法返回mStub对象，也就是Server中的Binder实体对象
public class ServerSevice extends Service {
    private static final String TAG = "ServerSevice";
    private List<Person> mPeople = new ArrayList<>();


    @Override
    public void onCreate() {
        mPeople.add(new Person());
        super.onCreate();
    }

    @Nullable
    @Override
    public IBinder onBind(Intent intent) {
        return mStub;
    }
    private BinderObj mStub = new BinderObj() {
        @Override
        public void addPerson(Person mPerson) {
            if (mPerson==null){
                mPerson = new Person();
                Log.e(TAG,"null obj");
            }
            mPeople.add(mPerson);
            Log.e(TAG,mPeople.size()+"");
        }

        @Override
        public List<Person> getPersonList() {
            return mPeople;
        }
    };
}
复制代码最终我们在客户端进程，bindService传入一个ServiceConnection对象，在与服务端建立连接时，通过我们定义好的BinderObj的asInterface方法返回一个代理对象，再调用方法进行交互
public class MainActivity extends AppCompatActivity {
    private boolean isConnect = false;
    private static final String TAG = "MainActivity";
    private PersonManger personManger;
    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);
        start();
        findViewById(R.id.textView).setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View v) {
                if (personManger==null){
                    Log.e(TAG,"connect error");
                    return;
                }
                personManger.addPerson(new Person());
                Log.e(TAG,personManger.getPersonList().size()+"");
            }
        });
    }

    private void start() {
        Intent intent = new Intent(this, ServerSevice.class);
        bindService(intent,mServiceConnection,Context.BIND_AUTO_CREATE);
    }
    private ServiceConnection mServiceConnection = new ServiceConnection() {
        @Override
        public void onServiceConnected(ComponentName name, IBinder service) {
            Log.e(TAG,"connect success");
            isConnect = true;
            personManger = BinderObj.asInterface(service);
            List<Person> personList = personManger.getPersonList();
            Log.e(TAG,personList.size()+"");
        }

        @Override
        public void onServiceDisconnected(ComponentName name) {
            Log.e(TAG,"connect failed");
            isConnect = false;
        }
    };
}
复制代码这样的话，一次完成的进程间的交互就完成了~是不是感觉没有想象中那么难，最后建议大家在不借助 AIDL 的情况下手写实现 Client 和 Server 进程的通信，加深对 Binder 通信过程的理解。
本文在写作过程中参考了蛮多的文章和源码，感谢大佬们的无私奉献

作者：散人丶
链接：https://juejin.im/post/5c444a67f265da612c5e29e1
来源：掘金
著作权归作者所有。商业转载请联系作者获得授权，非商业转载请注明出处。

## 参考

[3分钟带你看懂android的Binder机制](https://juejin.im/post/5c444a67f265da612c5e29e1)

[Android进阶——Android跨进程通讯机制之Binder、IBinder、Parcel、AIDL](https://blog.csdn.net/qq_30379689/article/details/79451596)
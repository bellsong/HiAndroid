# BLE Mesh

要说清楚 BLE Mesh 首先需要回答几个问题：

1、mesh 是什么？

2、mesh 用来干嘛？

3、mesh 在 BLE 中的位置？


BLE 作为蓝牙发展中的后续产物，现目前支持的应用场景非常有限，在 Connection 状态下的数据传输，也是点对点的数据传输，虽然现在 BLE 能够支持 Multi-Connection，但是其最大连接数和直接的硬件资源强相关，所以无法支持无限个连接，即便是能够支持很多连接，在 BLE5.x时代，引入的多 PHY 规格中，Coded PHY 125kbps 状态下的连接交互长度依然很有限制。

有了各种限制，就直接关系到应用场景的单一化；此刻 Mesh 就应运而生；


1、mesh 是什么？
Mesh 是蓝牙官方组织（SIG）推出的蓝牙 BLE 组网的规范，通过 BLE 作为载体，制作了一套星形网状的拓扑类型的多对多的组织。每一台设备都可以与网络中的其它设备进行通信，设备间的通信以消息的形式传递，一台设备可以将某一台设备发来的消息 中继到另一台设备，这样就可以扩展端到端的通信范围， 这个范围远超过一个单独设备蓝牙无线电所覆盖的范围；

BLE mesh是被设计用于大规模节点互相通信的网络支持的特性的。其应用目标场景是比如楼宇自动化、传感器网络、以及更多的 IoT 应用。

 

2、mesh 用来干嘛？
根据 BLE mesh 组网以及 mesh 本身的规范来说，它可以支持更多的节点通信，更远的消息传播的距离（中继节点），更低功耗的 IoT 节点（低功耗节点），可靠的消息传输（安全加密）；比如在停车场，在楼宇自动化，在室内超市，等等场景，均可以部署 BLE mesh 节点，通过 mesh 本身的特性来达到安全，可靠数据交互的目的；

 

3、mesh 在 BLE 中的位置？
就 mesh 本身而言，他是基于 BLE ADV 的一层应用，可以将其理解为 HOST 层的一个新增特性，他的数据通过 ADV 发送，通过全窗 SCAN 来接收，以 ADV/SCAN 作为载体，定义不同的节点类型以及数据的含义，得以实现 Mesh 网状结构。

4、基本术语

    1、节点（node）

    2、开通配置（provisioning）

    3、元素（element）

    4、消息（message）

    5、地址（Address）

    6、消息的发布/订阅（Publish / Subscribe）

    7、状态与属性（State / Property）

    7.1、状态

    7.1.1 状态的绑定

    7.2、属性

    8、特性

    8.1、中继（Relay）

    8.2、低功耗 & 友节点（Low Power & Friend）

    8.3、代理节点（Proxy）

    9、安全
    
5、Mesh 协议栈架构

(1) Model layer：根据用户的使用场景，定义的标准化的操作；

(2) Foundation Model layer：定义了用于配置和管理mesh网络的状态、消息以及model。

(3) Access layer： 定义应用层的数据格式。应用层数据的加解密功能在这一层完成，并验证接收的数据是否认证通过。

(4) Upper transport layer：对应用数据进行加解密以及鉴权。

(5) Lower transport layer： 主要对upper transportlayer的数据包进行分段和重组。

(6) Network layer： 网络层是MESH网络的关键层。这一层主要负责将传输层的数据包传输给一个或者多个其它节点。数据包是否被拒绝、或者被在本节点做进一步处理、或者数据包将会被前传给其它节点是网络层的核心功能。同时，网络层还对本层消息进行加解密和鉴权。定义了多种输入输出的 Filter；

(7) Bearer layer：定义了网络层数据包如何在节点之间传递。当前协议版本定义了两种承载，一种是广播承载，另一种是GATT承载。

(8) Bluetooth low energy core specification：这一层是在MESH协议发布之前所定义的 BLE Core Specification。

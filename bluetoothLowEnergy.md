# 低功耗蓝牙 Bluetooth Low Energy（BLE）

## 介绍
BLE 的全称叫做 Bluetooth Low Energy，也称之为低功耗蓝牙，属于蓝牙技术中的一种。与之对应的是 Classic Bluetooth，经典蓝牙。当然，经典蓝牙是最先推出来的，后面才有了 LE 的版本。在兼容性上，LE 的蓝牙不兼容 Classic 的版本，可以理解成为独立的一种蓝牙形态。旨在针对低功耗的领域进行的一种无线数据传送的解决方案。

## 频段
“蓝牙低功耗”技术采用与“经典蓝牙”技术相同的工作频率（2.400GHz-2.4835GHz - ISM频带），但使用另一组信道。不同于经典蓝牙的79 1-MHz信道，蓝牙低功耗使用40个 RF Channel，2MHz带宽信道。在一个信道内，数据使用高斯频移调制（GFSK）传输，类似经典蓝牙的基本速率方案；比特率1Mbit/s，最大发射功率10mW。

由于使用了免许可的 2.4GHz 频段，这个频段很多无线产品都在用（WIFI），所以为了稳定性，蓝牙在链接状态下，有一种称之为跳频的技术（Hopping），链接双方在约定好的时间段同时调到另外的频率上进行数据收发，避免了信道上的数据拥堵。

## 应用
手环，门锁，智能家居的节点等等，都能够使用 LE 的方案进行数据传送。

在一颗蓝牙芯片上，既能够支持 Classic 也支持 LE 的，称之为双模。否则，为单模。手机上的蓝牙芯片，绝大多数都是双模的蓝牙芯片。

当然，无线产品还有很多，ZigBee、Wifi、LTE 等等都是属于无线产品，BLE 只是其中的一种。

## 构成
既然是无线芯片呢，其组成由软件和硬件构成。软件部分，为 BLE 协议栈，硬件部分包含 RF PHY（无线收发装置），Modem（调制解调器），以及 Baseband（基带）构成。BLE 的协议规范，全部需要遵循 Core Spec 来进行定制，Core Spec 中包含了 RF 、Modem、Air Interface，数据编码/解码，以及软件的协议规范：
https://www.bluetooth.com/specifications/bluetooth-core-specification/

## 发展
蓝牙技术有一个叫做 SIG 的蓝牙技术联盟，联盟负责推动蓝牙技术的持续发展，包括定制 Spec，组织一些会议等等。

从最初的 BLE4.2 版本，发展到 BLE 5.0 （支持 Extend Advertising，Periodic Advertising，2M/Coded PHY等），到现在的 BLE 5.1 版本（AoA/AoD Antenna矩阵蓝牙定位，以及 ISO 同步传输），可见蓝牙的 LE 技术一直在高速的演进。

## 技术点

### 基本特性（状态、角色、地址、信道）

### 空口数据包组成

### 数据发送接收流程 

### 广播态数据包组成 Advertising Packets PDUs

### 扫描态数据包组成 Scanning Packets PDUs

### 发起态数据包组成 Initiating Packets PDUs

### 连接态数据包组成 Connection Packet PDUs

### Device Filtering

### Privacy

### BLE 架构（分层：HOST/HCI/Controller）

在 BLE 系统中不仅仅是物理层和硬件内容，根据整个 BLE 从上到下，大致分为了 Profile，Host，HCI，Controller 以及 硬件部分的 Baseband 和 RF。

两个 BLE 通信，从逻辑上来看，是每个层的单独通信，从数据的流向来看，发送是自上而下，数据接收是自下而上。

Profile：主要是根据不同的应用场景，制定了最上层的协议交互。比如，心率的 Profile，电池电量检测的 Profile 等。

Host：为上层提供接口支持，自身维护了一些软件层次的协议和加密流程等等控制，纯软件行为。

HCI 的全称是：Host Interface Controller，是 Host 和 Controller 通信之间的接口。那么问题来了，Host 与 app 之间是直接的 Function call，那为啥 Host 与 Controller 之间要专门的定义两层软件之间的通信协议呢？这就要从 BLE 的产品形态说起了。稍后呈现。

Controller：向上负责向 Host 汇报状态，向下，接收 Host 的指令，与 Baseband 交互，协调、控制、管理硬件资源。

Modem：负责调制解调

RF：收发机负责接收/发送空中的数据包。

#### HCI
HCI 的全称是：Host Interface Controller，是 Host 和 Controller 通信之间的接口。存在这个薄薄的一层协议的含义是这样的：

一般的，BLE 是一颗单芯片的解决方案，芯片集成了所有的软硬件资源，对于单独的产品来说，可能出现以下产品形态：

1、Full stack：从上到下的协议栈全部都有，用户简单的写 App 调用接口即可，此刻，HCI 退化成为 Function Call（比如手环产品，节点控制器，单车控制器等等）

2、Only Controller：有的产品，只需要 Controller 不要 Host 和 APP，BLE 芯片作为一个外挂芯片，接到主控芯片上，主控芯片通过标准的 HCI 指令（通常介质为 UART）与 BLE 芯片交互。此刻，HCI 便成为了一种两颗芯片之间的通信协议（比如手机上 AP 和 BLE 就是这样的关系）

3、Only Host：Android 不就是么

在 BLE Core Spec 中，HCI 相关的内容定义在 Vol 4:Host Controller Interface [Transport Layer]

BLE Core Spec 支持的 HCI 种类有 4 中：

UART

USB

SDIO

3-Wire UART

也就是说，当我们要使用 BLE 的时候，如果他能够支持外挂的这种类型，并且要知道他支持的 HCI 的类型，那么我们就可以将其挂接到我们的 AP 或者 MCU 上，通过其总线来进行 BLE 芯片的控制（其实就把 BLE当成一个 MCU 的外设一样就可，一个大一点的 IP 而已）。

一般的，一款这样的 BLE 只支持一种传输的 HCI 协议，通常是第一中类型，也就是 UART。那我们就来针对这个进行分析，其余的也是套路。






## 参考
————————————————
版权声明：本文为CSDN博主「爱洋葱」的原创文章，遵循CC 4.0 BY-SA版权协议，转载请附上原文出处链接及本声明。
原文链接：https://blog.csdn.net/zhoutaopower/article/details/94499166

## 以下是一些蓝牙开发技术的资料推荐：

《蓝牙低功耗：基于ARM Cortex-M0架构》（原书第2版）作者：Robin Heydon。这是一本非常详细的蓝牙低功耗开发指南，适合想要深入了解蓝牙低功耗技术的开发者。

《Bluetooth Essentials for Programmers》作者：Albert S. Huang。这是一本介绍蓝牙核心技术和应用的书籍，适合对蓝牙技术初步了解的开发者。

《Bluetooth Low Energy: The Developer's Handbook》作者：Robin Heydon。这是一本介绍蓝牙低功耗的开发指南，适合初学者和有一定经验的开发者。

Bluetooth SIG官网（https://www.bluetooth.com/）提供了蓝牙开发的最新技术和标准，包括规范文档、开发工具和资源等。

Nordic Semiconductor官网（https://www.nordicsemi.com/）提供了Nordic Semiconductor芯片的蓝牙开发指南、应用案例和开发工具。

Android官网（https://developer.android.com/guide/topics/connectivity/bluetooth）提供了Android设备蓝牙开发的指南和API文档。

Apple官网（https://developer.apple.com/bluetooth/）提供了iOS设备蓝牙开发的指南和API文档。

Bluetooth开发者社区（https://www.bluetooth-dev.com/）提供了蓝牙开发的论坛和博客，可以与其他开发者交流和分享经验。

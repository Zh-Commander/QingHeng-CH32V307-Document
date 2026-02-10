# 第 24 章 控制器局域网（CAN）

本章模块描述适用于CH32F2x、CH32V2x和CH32V3x微控制器系列部分产品。

控制器局域网是一种用于串行数据通信的高性能通信协议。CAN控制器提供了一个完整的 CAN 协议实现方案，支持 CAN 协议 2.0A 和 2.0B。CAN 控制器可以用来构建强大的局域网来实现安全的分布式实时控制，以较小的CPU负荷来处理大量的数据报文，在工业和汽车领域有着广泛的应用。

## 24.1 主要特性

兼容 CAN 规范 2.0A 和 2.0B  
$\bullet$ 可编程的传输速率，最高可达 1Mbit/s  
$\bullet$ 支持时间触发通信功能，避免低优先级消息阻塞  
$\bullet$ 支持三个发送邮箱，发送报文优先级可由报文标识符或发送请求的次序决定，并可记录发送报文SOF时刻的时间戳  
l 支持三级邮箱深度的 2 个接收 FIFO，28 个报文过滤器组可供配置，大容量产品 CAN1 和 CAN2 共用28个过滤器，每个过滤器组可配置成32 或16 位模式，屏蔽位或标识符列表模式，能够尽量减少软件对报文筛选的干预，FIFO溢出处理方式灵活，并可记录接收报文 SOF时刻的时间戳  
占用 4 个中断向量，每个中断源可以独立配置

## 24.2 CAN 控制器工作模式

CAN 控制器可以对寄存器 CAN_CTLR 中的 SLEEP 或 INRQ 位进行操作，实现在初始化模式、睡眠模式和正常模式3个工作模式下切换。

### 24.2.1 初始化模式

在复位后，CAN默认工作在睡眠模式以减低功耗，此时禁止报文收发，TX引脚的内部上拉电阻使能，TX引脚输出隐性位。对寄存器CAN_CTLR中的INRQ 位置 1，请求 CAN控制器进入初始化模式，当寄存器 CAN_STATR 的 INAK 位自动置 1 则成功进入初始化状态。同样对寄存器 CAN_CTLR 中的 INRQ 位清零，请求 CAN 控制器退出初始化模式，当寄存器 CAN_STATR 的 INAK 位自动清 0 则成功退出初始化状态。

对过滤器组进行初始化，可以在非初始化模式下进行，不过必须对寄存器 CAN_FCTLR 的 FINIT位进行置1，此时禁止接收报文。

### 24.2.2 睡眠模式

对寄存器CAN_CTLR中的SLEEP位置1，请求CAN控制器进入睡眠模式，当寄存器CAN_STATR的SNAK位自动置1则CAN成功进入睡眠模式，此时CAN控制器的时钟停止，但邮箱寄存器仍可访问。

由睡眠模式进入初始化模式，必须对CAN_CTLR的SLEEP位清O，INRQ位置1，当寄存器CAN_STATR的位自动置 1 则切换为初始化状态完成。

由睡眠模式进入正常模式，必须对CAN_CTLR的SLEEP位清O，当寄存器CAN_STATR的SNAK位自动清0则进入正常模式。

![](images/f0c487bca015eef32ed2a59f52c865a20e937b1151aa36eded58d2e6cd6e1d31.jpg)  
图 24-1 CAN 工作模式切换

### 24.3 CAN 控制器测试模式

在初始化模式下，对寄存器 CAN_BTIMR 的 SILM 和 LBKM 位进行操作，可以选择一种测试模式，然后通过对寄存器CAN_CTLR的INRQ位清零，退出初始化模式，进入测试模式。测试模式分为静默模式、环回模式和静默环回模式三种。

### 24.3.1 静默模式

对寄存器CAN_BTIMR的SILM位置1，可选择进入静默模式。该模式下，CAN控制器可以接收，不能对外发送报文，对外总是处于隐性位，可以避免对总线产生影响，但是报文能够被所在节点的控制器所接收。通常静默模式被用于CAN总线的状态分析。

### 24.3.2 环回模式

对寄存器CAN_BTIMR的LBKM位置1，可选择进入环回模式。该模式下，CAN控制器可以对外发送报文，不能接收外部报文，但是发送报文能够被所在节点的控制器所接收，接收过滤机制有效。通常环回模式被用于CAN控制器的收发测试。

### 24.3.3 静默环回模式

对寄存器CAN_BTIMR的SILM和LBKM位置1，可选择进入静默环回模式。该模式通常用于 CAN控制器封闭自测试，在该模式下，对 CAN 总线无影响，RX 引脚与总线断开，TX 引脚置隐性位。

![](images/127d8f947e465a7b84e6ec2992d016def587763f2b34487994c05e656a0db656.jpg)  
图24-2 CAN总线的三种测试模式

![](images/7081f5d6a6f1fa4ecadf54306b3a8067abd313e0dd1c0b8f3368a294f864cc38.jpg)

![](images/391a060d97a992266c321232c59e98ea3c15db494ea12d01467acb24e9796f99.jpg)

### 24.4 MCU 处于调试模式下 CAN 控制器的工作状态

当MCU进入调试模式后，内核处于暂停状态，但可以通过调试模块中配置位来决定 CAN控制器是处于正常运行或停止状态。

## 24.5 CAN 控制器功能描述

### 24.5.1 发送处理流程

发送处理流程如下：如果三个发送邮箱中有空置的邮箱，应用层软件仅对空置邮箱的寄存器具有写入权限，对寄存器 CAN_TXMIRx、CAN_TXMDTRx、CAN_TXMDLRx 和 CAN_TXMDHRx 进行操作，可以设置报文标识符、报文长度、时间戳和报文数据等。在数据准备好之后，对寄存器 CAN_TXMIRx 的 TXRQ 位置 1 请求发送，邮箱进入挂号状态，并进行优先级排队；一旦成为最高优先级邮箱，则变为预定发送状态，等待 CAN 总线空闲；当 CAN 总线空闲时，预定发送邮箱的报文立刻进入发送状态；报文发送完毕后，邮箱重新成为空置邮箱，并且寄存器CAN_TSTATR 的RQCP 和 TXOK位置1，来指示发送成功；若发送时仲裁失败，寄存器CAN_TSTATR的ALST位置1，若发送错误，则 TERR位置 1。

### 24.5.2 发送优先级

发送优先级可以由标识符或发送请求先后次序决定，寄存器CAN_CTLR 的 TXFP位置1 按发送请求先后次序发送，按发送请求先后次序主要应用于分段发送；TXFP 位清 0 按标识符优先级决定发送次序，标识符越小则优先级越高，同标识符的情况下，则低编号的邮箱有更高优先级。

### 24.5.3 发送中止处理

若对寄存器CAN_TSTATR的ABRQ位置 1，则可以中止发送请求。当邮箱状态为挂号或预定发送状态时，发送请求直接中止；当邮箱处于发送状态时，中止请求可能会成功（停止发送），也有可能会失败（发送完成），结果可由寄存器 CAN_TSTATR 的 TXOK 位来查询。

### 24.5.4 基于时间触发模式

传统的CAN通信总线繁忙时，容易造成低优先级的消息长时间阻塞，甚至无法满足其时限的要求。为了解决该瓶颈，推出了基于时间触发模式的相关协议，此类协议在工业上有一定规模的应用，基于时间触发模式的功能即为配合此类协议的应用。

在时间触发模式下有两种模式可供选择，使用该模式需关闭自动重传功能，通过配置 CAN_TTCTLR寄存器的MODE位来选择默认模式和增强模式。对寄存器 CAN_CTLR的 TTCM和NART位置 1，使能时间触发模式并禁止自动重传，CAN_TTCTLR 寄存器的 MODE 位默认为 0，此时工作在默认模式，内部定时器被激活用来产生发送和接收邮箱的时间戳，定时器在 CAN位时间累加，内部定时器在接收和发送的

帧起始位的采样点位置被采样并产生时间戳。若使用增强模式，需要将配置CAN_TTCTLR寄存器的MODE位为1，来开启增强模式，使用该模式，在整个 CAN网络中，必须存在三个或三个以上的节点，其中一个节点发送时间基准，其他节点收到该基准节点的时间戳后，通过向 CAN_TTCTLR寄存器的 TIMRST位写1复位内部计数器，将内部计数器进行同步，这样除了发送时间基准的节点之外，其余CAN节点实现了时间同步，之后将待发送的数据写入发送邮箱，依次配置各节点的时间触发计数值（CAN_TTCNT寄存器的 TIMCNT）和内部计数器计数终值（CAN_TTCTLR 寄存器 TIMCMV），时间触发计数值和内部计数器计数终值由 CAN 节点的个数、CAN 通信速率和一帧数据位数决定，配置完成后，各节点等待内部计数器计数到时间触发计数值后，触发发送动作。

### 24.5.5 接收处理流程

CAN 总线报文的接收，由控制器硬件来完成，无需MCU 的干涉，减轻了 MCU的处理负荷。所接收到的报文，根据寄存器 CAN_FAFIFOR 的设置，分别被存储到两个具有 3 级邮箱深度的 FIFO 中，应用层如需获取报文，只能通过接收FIFO邮箱来读取有效接收报文。

初始时，接收 FIFO 为空，接收 FIFO 寄存器 CAN_RFIFOx 的 FMR[1:0]值为二进制 00b，接到一个有效接收报文后，变为挂号 1 状态，控制器自动把接收 FIFO 寄存器 CAN_RFIFOx 的 FMR[1:0]设置二进制 01b；若此时读取邮箱数据寄存器 CAN_RXMDLRx 和 CAN_RXMDHRx，通过对接收 FIFO 寄存器CAN_RFIFOx的RFOM位置1来释放邮箱，接收 FIFO状态又变为空；如果在挂号 1 状态时不释放邮箱，下一个有效接收报文被接到后，接收 FIFO 状态切换为挂号 2 状态，此时接收 FIFO 寄存器 CAN_RFIFOx的FMR[1:0]自动置二进制10b；若读取邮箱数据寄存器并释放邮箱，则状态回到挂号 1；如果在挂号2 状态不释放邮箱，则接收 FIFO 进入挂号 3 状态；同样在挂号 3 状态下读取报文并释放邮箱，则返回挂号 2 状态；若在挂号 3 状态不释放邮箱，则在接收到下一个有效报文时，必然导致报文丢失情况出现。

![](images/d45ce42eac6a011989d38382e4a52d76f40e1694148caaa15cd3f8e176a8569b.jpg)  
图 24-3 接收 FIFO 状态切换图

上文中的报文丢失情况，即接收FIFO为满，报文溢出导致报文丢失，接收 FIFO寄存器 CAN_RFIFOx的 FOVR 位会硬件自动置 1，以供溢出查询。寄存器 CAN_CTLR 的 RFLM 位置 1，则接收 FIFO 锁定功能启用，丢弃的报文为新接收报文；寄存器 CAN_CTLR的 RFLM 位清 0，则接收FIFO 锁定功能停用，接收FIFO的三个原报文中，最后接收的报文会被新报文覆盖。

当寄存器CAN_INTENR相关位置位，可以使接收 FIFO状态切换时产生中断，以便更高效的处理接收报文，详见24.6节CAN中断。

### 24.5.6 接收报文标识符过滤

模块中有着多达 28 个过滤器组，通过设置过滤器组，每个 CAN 节点都可以接收到符合过滤规则的报文，不符合过滤规则的报文被硬件丢弃，无需软件干涉。

每个过滤器组由 2 个 32 位寄存器 CAN_FxR0 和 CAN_FxR1 组成。过滤器组的位宽都可以通过设置寄存器 CAN_FSCFGR 的各个位独立配置成 1 个 32 位过滤器或两个 16 位过滤器。每个过滤器组可通过设置寄存器 CAN_FMCFGR 的各个位配置为屏蔽位或标识符列表模式，各个过滤器组可以通过设置寄存器CAN_FWR的各个位选择启用或禁用。设置寄存器 CAN_FAFIFOR的各个位可以把选择通过过滤器的报文存放到哪个接收FIFO。

如下表 24-1 所示，屏蔽位模式下，两个寄存器分别为标识符寄存器和屏蔽寄存器，两者需要配合使用，标识符寄存器每一位指示相应的位期望值为显性或隐性，屏蔽寄存器每一位指示相应位是否需要对应标识符寄存器位期望值一致。

表 24-1 32 位屏蔽位模式  

<table><tr><td>标识符寄存器</td><td>CAN_FxR1[31:24]</td><td colspan="2">CAN_FxR1[23:16]</td><td>CAN_FxR1[15:8]</td><td colspan="4">CAN_FxR1[7:0]</td></tr><tr><td>屏蔽位寄存器</td><td>CAN_FxR2[31:24]</td><td colspan="2">CAN_FxR2[23:16]</td><td>CAN_FxR2[15:8]</td><td colspan="4">CAN_FxR2[7:0]</td></tr><tr><td>映射</td><td>STID[10:3]</td><td>STID[2:0]</td><td>EXID[17:13]</td><td>EXID[12:5]</td><td>EXID[4:0]</td><td>IDE</td><td>RTR</td><td>0</td></tr></table>

标识符列表模式下，两个寄存器都被用作标识符寄存器，接收报文标识符必须与其中一个寄存器保持一致才能通过筛选。

表24-2 32位标识符列表模式  

<table><tr><td>标识符寄存器</td><td>CAN_FxR1[31:24]</td><td colspan="2">CAN_FxR1[23:16]</td><td>CAN_FxR1[15:8]</td><td colspan="4">CAN_FxR1[7:0]</td></tr><tr><td>屏蔽位寄存器</td><td>CAN_FxR2[31:24]</td><td colspan="2">CAN_FxR2[23:16]</td><td>CAN_FxR2[15:8]</td><td colspan="4">CAN_FxR2[7:0]</td></tr><tr><td>映射</td><td>STID[10:3]</td><td>STID[2:0]</td><td>EXID[17:13]</td><td>EXID[12:5]</td><td>EXID[4:0]</td><td>IDE</td><td>RTR</td><td>0</td></tr></table>

在 16 位模式下，寄存器组被拆分成四个寄存器，屏蔽位模式每组过滤器的屏蔽位模式可以有 2个过滤器，每个过滤器里各包含一个16位标识符寄存器和 16位屏蔽寄存器；标识符列表模式下四个寄存器都用作标识符寄存器。

表 24-3 16 位屏蔽位模式  

<table><tr><td>标识符寄存器n</td><td>CAN_FxR1[15:8]</td><td colspan="4">CAN_FxR1[7:0]</td></tr><tr><td>屏蔽位寄存器n</td><td>CAN_FxR1[31:24]</td><td colspan="4">CAN_FxR1[23:16]</td></tr><tr><td>标识符寄存器n+1</td><td>CAN_FxR2[15:8]</td><td colspan="4">CAN_FxR2[7:0]</td></tr><tr><td>屏蔽位寄存器n+1</td><td>CAN_FxR2[31:24]</td><td colspan="4">CAN_FxR2[23:16]</td></tr><tr><td>映射</td><td>STID[10:3]</td><td>STID[2:0]</td><td>RTR</td><td>IDE</td><td>EXID[17:15]</td></tr></table>

表24-4 16位标识符列表模式  

<table><tr><td>标识符寄存器n</td><td>CAN_FxR1[15:8]</td><td colspan="4">CAN_FxR1[7:0]</td></tr><tr><td>屏蔽位寄存器n</td><td>CAN_FxR1[31:24]</td><td colspan="4">CAN_FxR1[23:16]</td></tr><tr><td>标识符寄存器n+1</td><td>CAN_FxR2[15:8]</td><td colspan="4">CAN_FxR2[7:0]</td></tr><tr><td>屏蔽位寄存器n+1</td><td>CAN_FxR2[31:24]</td><td colspan="4">CAN_FxR2[23:16]</td></tr><tr><td>映射</td><td>STID[10:3]</td><td>STID[2:0]</td><td>RTR</td><td>IDE</td><td>EXID[17:15]</td></tr></table>

报文进入 FIFO 邮箱中，会被应用程序读取并存放，通常应用程序根据报文标识符来区分报文数据。CAN 控制器对接收 FIFO 中通过不同过滤器筛选的报文，提供了过滤器编号，编号被存放在寄存器CAN_RXMDTRx的FMI[7:0]中，编号时不考虑过滤器组是否启用。编号规则详见图 22-4的示例。

当出现某个报文能通过多个过滤器的过滤，则接收邮箱中存放的过滤器编号根据过滤器优先级规则来决定存放哪个过滤器的编号，过滤器优先级规则如下：

所有 32 位的过滤器优先级均高于 16 位的过滤器  
$\bullet$ 对于同样宽度的过滤器，标识符列表的过滤器优先级高于屏蔽位模式的过滤器  
宽度和模式都一致的过滤器，编号小的过滤器优先级更高

如图 22-5 所示：在接收报文时，先把标识符与 32 位标识符列表模式过滤器进行匹配筛选，没有

匹配再与32位屏蔽位模式过滤器进行匹配筛选，没有匹配则继续与 16位标识符列表模式过滤器进行匹配筛选，没有匹配最后与16位屏蔽位模式过滤器进行匹配筛选，最后如果都没有匹配则丢弃报文，出现匹配则报文存入接收FIFO的邮箱，标识符编号存入寄存器 CAN_RXMDTRx的 FMI中。

![](images/0cad574bfd93a75f0695a651ce376282f0f114044aa87de32a6b4160d2efc9ce.jpg)  
图24-4 过滤器编号的示例

![](images/8f1ea5a4a6775986ffe7b3ad321af87c217a559fb87435daf09666ac1c8f955d.jpg)  
图 24-5 过滤器过滤示例

### 24.5.7 出错处理

CAN 控制器依靠状态错误寄存器CAN_ERRSR，对于总线上的出错管理。状态错误寄存器 CAN_ERRSR

里的TEC和REC，分别代表发送和接收错误计数值，根据随着收发错误的增加而增加，收发成功而减小，可以根据它们的值来判断CAN总线的稳定性。

当状态错误寄存器CAN_ERRSR里的TEC和REC小于128时，当前CAN节点处于错误主动状态，可以正常参与总线通信，并且在侦测到错误的时候发出主动错误标志。

当状态错误寄存器 CAN_ERRSR 里的 TEC 和 REC 大于 127 时，当前 CAN 节点处于错误被动状态，并且在侦测到错误的时候不允许发出主动错误标志，只能发出被动错误标志。

当状态错误寄存器 CAN_ERRSR 里的 TEC 大于 255 时，当前 CAN 节点进入离线状态。

当总线监视到 128 次出现 11 个连续的隐性位时，恢复到错误主动状态，该恢复方式受主控制寄存器 CAN_CTLR 里的 ABOM 位影响。若 ABOM 置 1，则硬件自动退出离线状态。若 ABOM 为 0，则需要软件操作 INRQ 位进入初识化模式，随后退出初始化，才能退出离线状态。

![](images/dcf5da02bfe2f99c366d2521d1e561c3e10d2e298331d20c5384525e3622b948.jpg)  
图 24-6 CAN 错误状态切换图

### 24.5.8 位时序

按照 CAN 总线的标准，将每一位时间分为四段：分别为同步段、传播时间段、相位缓冲段 1 和相位缓冲段 2。这些段由最小时间单元 Tq 组成。CAN 控制器通过采样来监测 CAN 总线变化，通过帧起始位的边沿进行同步

CAN 控制器把上述四段重新划分为三段，分别为：

l 同步段(SS)：也就是CAN标准里的同步段，固定为1 个最小时间单元，正常情况下所期望的位跳变发生在本时间段内。  
l 时间段1(BS1)：包含CAN标准里的传播时间段和相位缓冲段 1，可以被设置为包含1到 16最小时间单元，可以被自动延长，用于补偿CAN总线上不同节点频率精度误差带来的相位正向漂移。该时间段结束为采样点位置。  
l 时间段2(BS2)：也就是CAN标准里的相位缓冲段 2，可以被设置为 1到 8个最小时间单元，可以被自动缩短，以补偿CAN总线上不同节点频率精度误差带来的相位负向漂移。

重新同步跳转宽度(SJW)，是每位中可以延长和缩小的最小时间单元数量上限，范围可设置为 1到4个最小时间单元。

上述参数都可以在 CAN 总线时序寄存器 CAN_BTIMR 里配置。

![](images/38f6f52fcdb10a52191bcfd986924adb1a9627c860ef8b7e3f47b718a43c3d1a.jpg)  
图 24-7 跳变出现在 BS1 中

如图24-7，SJW为2，总线电平跳变在时间段 1 被检测到，则需要延长时间段 1 的长度，最大延长SJW，从而延迟采样点的位置。

![](images/da5bf99c40534f08cbe8c078b810edd77788085776c439d4799d6126d0310d5d.jpg)  
图 24-8 跳变出现在 BS2 中

如图24-8，SJW为2，总线电平跳变在时间段 2 被检测到，则需要缩小时间段 2 的长度，最大缩小SJW，从而提前采样点的位置。

CAN波特率计算公式为：

$$
\mathrm {C A N b p s} = \frac {\mathrm {t p c l k 1}}{(\mathrm {T S 1} [ 3 : 0 ] + 1 + \mathrm {T S 2} [ 2 : 0 ] + 1 + 1) \times \mathrm {B R P} [ 9 : 0 ]}
$$

这里 tpclk1 为 APB1 时钟周期，BRP[9:0]、TS1[3:0]、TS2[2:0]为 CANx_BTIMR 寄存器对应位。

## 24.6 CAN 中断

CAN 控制器有四个中断向量，分别为发送中断、FIFO_0 中断、FIFO_1 中断、错误及状态变化中断。

设置 CAN 中断允许寄存器 CAN_INTENR，可以允许或禁用各个中断源。

发送中断由发送邮箱变空事件产生，中断产生后，查询寄存器 CAN_TSTATR 的 RQCP0、RQCP1 和RQCP2位来判断是哪个邮箱变空事件产生。

FIFO0中断由接收新报文、接收邮箱变满和溢出事件产生，中断产生后，查询寄存器 CAN_RFIFO0的FMP0、FULL0和FOVER0位来判断是哪个邮箱变空事件产生。

FIFO1中断由接收新报文、接收邮箱变满和溢出事件产生，中断产生后，查询寄存器 CAN_RFIFO1

的 FMP1、FULL1 和 FOVER1 位来判断是哪个邮箱变空事件产生。

错误及状态变化中断由出错、唤醒和睡眠事件产生。

![](images/9e90d4d0dc2ad4b4aa5b90c38cde521c438e74eb519c540ea7f5445fbbdb3e32.jpg)  
图 24-9 CAN 中断逻辑图

![](images/92d82bdd8d0d2b2b3aff6ffa67a9a2107c75b7485d1c246c62568e016354a32f.jpg)

![](images/39af9d6fff555a48f8da8c28fcf4237a71da7f28ae4fcef42ef5dd8dc5434a80.jpg)

![](images/ed688c13dc15921e7f2d919f0f472408722404115dc847b103375dd60fb5d00e.jpg)

## 24.7 寄存器描述

CAN 控制器相关的寄存器必须用 32 位字的方式来操作。为了避免当前节点对整个 CAN 总线的影响，所以应用软件只能在初始化模式下修改位时序寄存器 CAN_BTIMR。

表 24-5 CAN1 相关寄存器列表  

<table><tr><td>名称</td><td>访问地址</td><td>描述</td><td>复位值</td></tr><tr><td>R32_CAN1_CTLR</td><td>0x40006400</td><td>CAN1 主控制寄存器</td><td>0x00010002</td></tr><tr><td>R32_CAN1_STATR</td><td>0x40006404</td><td>CAN1 主状态寄存器</td><td>0x00000C02</td></tr><tr><td>R32_CAN1_TSTATR</td><td>0x40006408</td><td>CAN1 发送状态寄存器</td><td>0x1C000000</td></tr><tr><td>R32_CAN1_RFIFO0</td><td>0x4000640C</td><td>CAN1 接收 FIF00 控制和状态寄存器</td><td>0x00000000</td></tr><tr><td>R32_CAN1_RFIFO01</td><td>0x40006410</td><td>CAN1 接收 FIF01 控制和状态寄存器</td><td>0x00000000</td></tr><tr><td>R32_CAN1_INTENR</td><td>0x40006414</td><td>CAN1中断使能寄存器</td><td>0x00000000</td></tr><tr><td>R32_CAN1_ERRSR</td><td>0x40006418</td><td>CAN1错误状态寄存器</td><td>0x00000000</td></tr><tr><td>R32_CAN1_BTIMR</td><td>0x4000641C</td><td>CAN1位时序寄存器</td><td>0x01230000</td></tr><tr><td>R32_CAN1_TTCTLR</td><td>0x40006420</td><td>CAN1时间触发控制寄存器</td><td>0x0000FFFF</td></tr><tr><td>R32_CAN1_TTCNT</td><td>0x40006424</td><td>CAN1时间触发计数值寄存器</td><td>0x00000000</td></tr></table>

表 24-6 CAN2 相关寄存器列表  

<table><tr><td>名称</td><td>访问地址</td><td>描述</td><td>复位值</td></tr><tr><td>R32_CAN2_CTLR</td><td>0x40006800</td><td>CAN2 主控制寄存器</td><td>0x00010002</td></tr><tr><td>R32_CAN2_STATR</td><td>0x40006804</td><td>CAN2 主状态寄存器</td><td>0x00000C02</td></tr><tr><td>R32_CAN2_TSTATR</td><td>0x40006808</td><td>CAN2 发送状态寄存器</td><td>0x1C000000</td></tr><tr><td>R32_CAN2_RFIFO0</td><td>0x4000680C</td><td>CAN2 接收 FIFO0 控制和状态寄存器</td><td>0x00000000</td></tr><tr><td>R32_CAN2_RFIFO01</td><td>0x40006810</td><td>CAN2 接收 FIFO01 控制和状态寄存器</td><td>0x00000000</td></tr><tr><td>R32_CAN2_INTENR</td><td>0x40006814</td><td>CAN2 中断使能寄存器</td><td>0x00000000</td></tr><tr><td>R32_CAN2_ERRSR</td><td>0x40006818</td><td>CAN2 错误状态寄存器</td><td>0x00000000</td></tr><tr><td>R32_CAN2_BTIMR</td><td>0x4000681C</td><td>CAN2 位时序寄存器</td><td>0x01230000</td></tr><tr><td>R32_CAN2_TTCTLR</td><td>0x40006820</td><td>CAN2 时间触发控制寄存器</td><td>0x0000FFFF</td></tr><tr><td>R32_CAN2_TTCNT</td><td>0x40006824</td><td>CAN2 时间触发计数值寄存器</td><td>0x00000000</td></tr></table>

表24-7 CAN1邮箱相关寄存器列表  

<table><tr><td>名称</td><td>访问地址</td><td>描述</td><td>复位值</td></tr><tr><td>R32_CAN1_TXMIRO</td><td>0x40006580</td><td>CAN1发送邮箱0标识符寄存器</td><td>X</td></tr><tr><td>R32_CAN1_TXMDTRO</td><td>0x40006584</td><td>CAN1发送邮箱0数据长度和时间戳寄存器</td><td>X</td></tr><tr><td>R32_CAN1_TXMDLRO</td><td>0x40006588</td><td>CAN1发送邮箱0低字节数据寄存器</td><td>X</td></tr><tr><td>R32_CAN1_TXMDHRO</td><td>0x4000658C</td><td>CAN1发送邮箱0高字节数据寄存器</td><td>X</td></tr><tr><td>R32_CAN1_TXMIR1</td><td>0x40006590</td><td>CAN1发送邮箱1标识符寄存器</td><td>X</td></tr><tr><td>R32_CAN1_TXMDTR1</td><td>0x40006594</td><td>CAN1发送邮箱1数据长度和时间戳寄存器</td><td>X</td></tr><tr><td>R32_CAN1_TXMDLR1</td><td>0x40006598</td><td>CAN1发送邮箱1低字节数据寄存器</td><td>X</td></tr><tr><td>R32_CAN1_TXMDHR1</td><td>0x4000659C</td><td>CAN1发送邮箱1高字节数据寄存器</td><td>X</td></tr><tr><td>R32_CAN1_TXMIR2</td><td>0x400065A0</td><td>CAN1发送邮箱2标识符寄存器</td><td>X</td></tr><tr><td>R32_CAN1_TXMDTR2</td><td>0x400065A4</td><td>CAN1发送邮箱2数据长度和时间戳寄存器</td><td>X</td></tr><tr><td>R32_CAN1_TXMDLR2</td><td>0x400065A8</td><td>CAN1发送邮箱2低字节数据寄存器</td><td>X</td></tr><tr><td>R32_CAN1_TXMDHR2</td><td>0x400065AC</td><td>CAN1发送邮箱2高字节数据寄存器</td><td>X</td></tr><tr><td>R32_CAN1_RXMIRO</td><td>0x400065B0</td><td>CAN1接收FIFO00邮箱标识符寄存器</td><td>X</td></tr><tr><td>R32_CAN1_RXMDTRO</td><td>0x400065B4</td><td>CAN1接收FIFO00邮箱数据长度和时间戳寄存器</td><td>X</td></tr><tr><td>R32_CAN1_RXMDLR0</td><td>0x400065B8</td><td>CAN1接收FIFO00邮箱低字节数据寄存器</td><td>X</td></tr><tr><td>R32_CAN1_RXMDHR0</td><td>0x400065BC</td><td>CAN1接收FIFO00邮箱高字节数据寄存器</td><td>X</td></tr><tr><td>R32_CAN1_RXMIR1</td><td>0x400065C0</td><td>CAN1接收FIFO01邮箱标识符寄存器</td><td>X</td></tr><tr><td>R32_CAN1_RXMDTR1</td><td>0x400065C4</td><td>CAN1接收FIFO01邮箱数据长度和时间戳寄存器</td><td>X</td></tr><tr><td>R32_CAN1_RXMDLR1</td><td>0x400065C8</td><td>CAN1接收FIFO01邮箱低字节数据寄存器</td><td>X</td></tr><tr><td>R32_CAN1_RXMDHR1</td><td>0x400065CC</td><td>CAN1接收FIFO01邮箱高字节数据寄存器</td><td>X</td></tr></table>

表24-8 CAN2邮箱相关寄存器列表  

<table><tr><td>名称</td><td>访问地址</td><td>描述</td><td>复位值</td></tr><tr><td>R32_CAN2_TXMIRO</td><td>0x40006980</td><td>CAN2发送邮箱0标识符寄存器</td><td>X</td></tr><tr><td>R32_CAN2_TXMDTRO</td><td>0x40006984</td><td>CAN2发送邮箱0数据长度和时间戳寄存器</td><td>X</td></tr><tr><td>R32_CAN2_TXMDLRO</td><td>0x40006988</td><td>CAN2发送邮箱0低字节数据寄存器</td><td>X</td></tr><tr><td>R32_CAN2_TXMDHRO</td><td>0x4000698C</td><td>CAN2发送邮箱0高字节数据寄存器</td><td>X</td></tr><tr><td>R32_CAN2_TXMIR1</td><td>0x40006990</td><td>CAN2发送邮箱1标识符寄存器</td><td>X</td></tr><tr><td>R32_CAN2_TXMDTR1</td><td>0x40006994</td><td>CAN2发送邮箱1数据长度和时间戳寄存器</td><td>X</td></tr><tr><td>R32_CAN2_TXMDLR1</td><td>0x40006998</td><td>CAN2发送邮箱1低字节数据寄存器</td><td>X</td></tr><tr><td>R32_CAN2_TXMDHR1</td><td>0x4000699C</td><td>CAN2发送邮箱1高字节数据寄存器</td><td>X</td></tr><tr><td>R32_CAN2_TXMIR2</td><td>0x400069A0</td><td>CAN2发送邮箱2标识符寄存器</td><td>X</td></tr><tr><td>R32_CAN2_TXMDTR2</td><td>0x400069A4</td><td>CAN2发送邮箱2数据长度和时间戳寄存器</td><td>X</td></tr><tr><td>R32_CAN2_TXMDLR2</td><td>0x400069A8</td><td>CAN2发送邮箱2低字节数据寄存器</td><td>X</td></tr><tr><td>R32_CAN2_TXMDHR2</td><td>0x400069AC</td><td>CAN2发送邮箱2高字节数据寄存器</td><td>X</td></tr><tr><td>R32_CAN2_RXMIRO</td><td>0x400069B0</td><td>CAN2接收FIFO00邮箱标识符寄存器</td><td>X</td></tr><tr><td>R32_CAN2_RXMDTRO</td><td>0x400069B4</td><td>CAN2接收FIFO00邮箱数据长度和时间戳寄存器</td><td>X</td></tr><tr><td>R32_CAN2_RXMDLR0</td><td>0x400069B8</td><td>CAN2接收FIFO00邮箱低字节数据寄存器</td><td>X</td></tr><tr><td>R32_CAN2_RXMDHRO</td><td>0x400069BC</td><td>CAN2接收FIFO00邮箱高字节数据寄存器</td><td>X</td></tr><tr><td>R32_CAN2_RXMIR1</td><td>0x400069C0</td><td>CAN2接收FIFO01邮箱标识符寄存器</td><td>X</td></tr><tr><td>R32_CAN2_RXMDTR1</td><td>0x400069C4</td><td>CAN2接收FIFO01邮箱数据长度和时间戳寄存器</td><td>X</td></tr><tr><td>R32_CAN2_RXMDLR1</td><td>0x400069C8</td><td>CAN2接收FIFO01邮箱低字节数据寄存器</td><td>X</td></tr><tr><td>R32_CAN2_RXMDHR1</td><td>0x400069CC</td><td>CAN2接收FIFO01邮箱高字节数据寄存器</td><td>X</td></tr></table>

表 24-9 CAN1 过滤器相关寄存器列表  

<table><tr><td>名称</td><td>访问地址</td><td>描述</td><td>复位值</td></tr><tr><td>R32_CAN1_FCTLR</td><td>0x40006600</td><td>CAN1 过滤器主控制寄存器</td><td>0x2A1C0E01</td></tr><tr><td>R32_CAN1_FMCFGR</td><td>0x40006604</td><td>CAN1 过滤器模式寄存器</td><td>0x00000000</td></tr><tr><td>R32_CAN1_FSCFGR</td><td>0x4000660C</td><td>CAN1 过滤器位宽寄存器</td><td>0x00000000</td></tr><tr><td>R32_CAN1_FAFIFOR</td><td>0x40006614</td><td>CAN1 过滤器 FIFO 关联寄存器</td><td>0x00000000</td></tr><tr><td>R32_CAN1_FWR</td><td>0x4000661C</td><td>CAN1 过滤器激活寄存器</td><td>0x00000000</td></tr><tr><td>R32_CAN1_FOR1</td><td>0x40006640</td><td>CAN1 过滤器组0寄存器1</td><td>X</td></tr><tr><td>R32_CAN1_FOR2</td><td>0x40006644</td><td>CAN1 过滤器组0寄存器2</td><td>X</td></tr><tr><td>R32_CAN1_F1R1</td><td>0x40006648</td><td>CAN1 过滤器组1寄存器1</td><td>X</td></tr><tr><td>R32_CAN1_F1R2</td><td>0x4000664C</td><td>CAN1 过滤器组1寄存器2</td><td>X</td></tr><tr><td>R32_CAN1_F2R1</td><td>0x40006650</td><td>CAN1 过滤器组2寄存器1</td><td>X</td></tr><tr><td>R32_CAN1_F2R2</td><td>0x40006654</td><td>CAN1 过滤器组2寄存器2</td><td>X</td></tr><tr><td>R32_CAN1_F3R1</td><td>0x40006658</td><td>CAN1 过滤器组3寄存器1</td><td>X</td></tr><tr><td>R32_CAN1_F3R2</td><td>0x4000665C</td><td>CAN1 过滤器组3寄存器2</td><td>X</td></tr><tr><td>R32_CAN1_F4R1</td><td>0x40006660</td><td>CAN1 过滤器组4寄存器1</td><td>X</td></tr><tr><td>R32_CAN1_F4R2</td><td>0x40006664</td><td>CAN1 过滤器组4寄存器2</td><td>X</td></tr><tr><td>R32_CAN1_F5R1</td><td>0x40006668</td><td>CAN1 过滤器组5寄存器1</td><td>X</td></tr><tr><td>R32_CAN1_F5R2</td><td>0x4000666C</td><td>CAN1 过滤器组 5 寄存器 2</td><td>X</td></tr><tr><td>R32_CAN1_F6R1</td><td>0x40006670</td><td>CAN1 过滤器组 6 寄存器 1</td><td>X</td></tr><tr><td>R32_CAN1_F6R2</td><td>0x40006674</td><td>CAN1 过滤器组 6 寄存器 2</td><td>X</td></tr><tr><td>R32_CAN1_F7R1</td><td>0x40006678</td><td>CAN1 过滤器组 7 寄存器 1</td><td>X</td></tr><tr><td>R32_CAN1_F7R2</td><td>0x4000667C</td><td>CAN1 过滤器组 7 寄存器 2</td><td>X</td></tr><tr><td>R32_CAN1_F8R1</td><td>0x40006680</td><td>CAN1 过滤器组 8 寄存器 1</td><td>X</td></tr><tr><td>R32_CAN1_F8R2</td><td>0x40006684</td><td>CAN1 过滤器组 8 寄存器 2</td><td>X</td></tr><tr><td>R32_CAN1_F9R1</td><td>0x40006688</td><td>CAN1 过滤器组 9 寄存器 1</td><td>X</td></tr><tr><td>R32_CAN1_F9R2</td><td>0x4000668C</td><td>CAN1 过滤器组 9 寄存器 2</td><td>X</td></tr><tr><td>R32_CAN1_F10R1</td><td>0x40006690</td><td>CAN1 过滤器组 10 寄存器 1</td><td>X</td></tr><tr><td>R32_CAN1_F10R2</td><td>0x40006694</td><td>CAN1 过滤器组 10 寄存器 2</td><td>X</td></tr><tr><td>R32_CAN1_F11R1</td><td>0x40006698</td><td>CAN1 过滤器组 11 寄存器 1</td><td>X</td></tr><tr><td>R32_CAN1_F11R2</td><td>0x4000669C</td><td>CAN1 过滤器组 11 寄存器 2</td><td>X</td></tr><tr><td>R32_CAN1_F12R1</td><td>0x400066A0</td><td>CAN1 过滤器组 12 寄存器 1</td><td>X</td></tr><tr><td>R32_CAN1_F12R2</td><td>0x400066A4</td><td>CAN1 过滤器组 12 寄存器 2</td><td>X</td></tr><tr><td>R32_CAN1_F13R1</td><td>0x400066A8</td><td>CAN1 过滤器组 13 寄存器 1</td><td>X</td></tr><tr><td>R32_CAN1_F13R2</td><td>0x400066AC</td><td>CAN1 过滤器组 13 寄存器 2</td><td>X</td></tr><tr><td>R32_CAN1_F14R1</td><td>0x400066B0</td><td>CAN1 过滤器组 14 寄存器 1</td><td>X</td></tr><tr><td>R32_CAN1_F14R2</td><td>0x400066B4</td><td>CAN1 过滤器组 14 寄存器 2</td><td>X</td></tr><tr><td>R32_CAN1_F15R1</td><td>0x400066B8</td><td>CAN1 过滤器组 15 寄存器 1</td><td>X</td></tr><tr><td>R32_CAN1_F15R2</td><td>0x400066BC</td><td>CAN1 过滤器组 15 寄存器 2</td><td>X</td></tr><tr><td>R32_CAN1_F16R1</td><td>0x400066C0</td><td>CAN1 过滤器组 16 寄存器 1</td><td>X</td></tr><tr><td>R32_CAN1_F16R2</td><td>0x400066C4</td><td>CAN1 过滤器组 16 寄存器 2</td><td>X</td></tr><tr><td>R32_CAN1_F17R1</td><td>0x400066C8</td><td>CAN1 过滤器组 17 寄存器 1</td><td>X</td></tr><tr><td>R32_CAN1_F17R2</td><td>0x400066CC</td><td>CAN1 过滤器组 17 寄存器 2</td><td>X</td></tr><tr><td>R32_CAN1_F18R1</td><td>0x400066D0</td><td>CAN1 过滤器组 18 寄存器 1</td><td>X</td></tr><tr><td>R32_CAN1_F18R2</td><td>0x400066D4</td><td>CAN1 过滤器组 18 寄存器 2</td><td>X</td></tr><tr><td>R32_CAN1_F19R1</td><td>0x400066D8</td><td>CAN1 过滤器组 19 寄存器 1</td><td>X</td></tr><tr><td>R32_CAN1_F19R2</td><td>0x400066DC</td><td>CAN1 过滤器组 19 寄存器 2</td><td>X</td></tr><tr><td>R32_CAN1_F20R1</td><td>0x400066E0</td><td>CAN1 过滤器组 20 寄存器 1</td><td>X</td></tr><tr><td>R32_CAN1_F20R2</td><td>0x400066E4</td><td>CAN1 过滤器组 20 寄存器 2</td><td>X</td></tr><tr><td>R32_CAN1_F21R1</td><td>0x400066E8</td><td>CAN1 过滤器组 21 寄存器 1</td><td>X</td></tr><tr><td>R32_CAN1_F21R2</td><td>0x400066EC</td><td>CAN1 过滤器组 21 寄存器 2</td><td>X</td></tr><tr><td>R32_CAN1_F22R1</td><td>0x400066F0</td><td>CAN1 过滤器组 22 寄存器 1</td><td>X</td></tr><tr><td>R32_CAN1_F22R2</td><td>0x400066F4</td><td>CAN1 过滤器组 22 寄存器 2</td><td>X</td></tr><tr><td>R32_CAN1_F23R1</td><td>0x400066F8</td><td>CAN1 过滤器组 23 寄存器 1</td><td>X</td></tr><tr><td>R32_CAN1_F23R2</td><td>0x400066FC</td><td>CAN1 过滤器组 23 寄存器 2</td><td>X</td></tr><tr><td>R32_CAN1_F24R1</td><td>0x40006700</td><td>CAN1 过滤器组 24 寄存器 1</td><td>X</td></tr><tr><td>R32_CAN1_F24R2</td><td>0x40006704</td><td>CAN1 过滤器组 24 寄存器 2</td><td>X</td></tr><tr><td>R32_CAN1_F25R1</td><td>0x40006708</td><td>CAN1 过滤器组 25 寄存器 1</td><td>X</td></tr><tr><td>R32_CAN1_F25R2</td><td>0x4000670C</td><td>CAN1 过滤器组 25 寄存器 2</td><td>X</td></tr><tr><td>R32_CAN1_F26R1</td><td>0x40006710</td><td>CAN1 过滤器组 26 寄存器 1</td><td>X</td></tr><tr><td>R32_CAN1_F26R2</td><td>0x40006714</td><td>CAN1 过滤器组 26 寄存器 2</td><td>X</td></tr><tr><td>R32_CAN1_F27R1</td><td>0x40006718</td><td>CAN1 过滤器组 27 寄存器 1</td><td>X</td></tr><tr><td>R32_CAN1_F27R2</td><td>0x4000671C</td><td>CAN1 过滤器组 27 寄存器 2</td><td>X</td></tr></table>

表24-10 CAN2过滤器相关寄存器列表  

<table><tr><td>名称</td><td>访问地址</td><td>描述</td><td>复位值</td></tr><tr><td>R32_CAN2_FCTLR</td><td>0x40006A00</td><td>CAN2 过滤器主控制寄存器</td><td>0x2A1C0E01</td></tr><tr><td>R32_CAN2_FMCFGR</td><td>0x40006A04</td><td>CAN2 过滤器模式寄存器</td><td>0x00000000</td></tr><tr><td>R32_CAN2_FSCFGR</td><td>0x40006A0C</td><td>CAN2 过滤器位宽寄存器</td><td>0x00000000</td></tr><tr><td>R32_CAN2_FAFIFOR</td><td>0x40006A14</td><td>CAN2 过滤器 FIFO 关联寄存器</td><td>0x00000000</td></tr><tr><td>R32_CAN2_FWR</td><td>0x40006A1C</td><td>CAN2 过滤器激活寄存器</td><td>0x00000000</td></tr><tr><td>R32_CAN2_FOR1</td><td>0x40006A40</td><td>CAN2 过滤器组 0 寄存器 1</td><td>X</td></tr><tr><td>R32_CAN2_FOR2</td><td>0x40006A44</td><td>CAN2 过滤器组 0 寄存器 2</td><td>X</td></tr><tr><td>R32_CAN2_F1R1</td><td>0x40006A48</td><td>CAN2 过滤器组 1 寄存器 1</td><td>X</td></tr><tr><td>R32_CAN2_F1R2</td><td>0x40006A4C</td><td>CAN2 过滤器组 1 寄存器 2</td><td>X</td></tr><tr><td>R32_CAN2_F2R1</td><td>0x40006A50</td><td>CAN2 过滤器组 2 寄存器 1</td><td>X</td></tr><tr><td>R32_CAN2_F2R2</td><td>0x40006A54</td><td>CAN2 过滤器组 2 寄存器 2</td><td>X</td></tr><tr><td>R32_CAN2_F3R1</td><td>0x40006A58</td><td>CAN2 过滤器组 3 寄存器 1</td><td>X</td></tr><tr><td>R32_CAN2_F3R2</td><td>0x40006A5C</td><td>CAN2 过滤器组 3 寄存器 2</td><td>X</td></tr><tr><td>R32_CAN2_F4R1</td><td>0x40006A60</td><td>CAN2 过滤器组 4 寄存器 1</td><td>X</td></tr><tr><td>R32_CAN2_F4R2</td><td>0x40006A64</td><td>CAN2 过滤器组 4 寄存器 2</td><td>X</td></tr><tr><td>R32_CAN2_F5R1</td><td>0x40006A68</td><td>CAN2 过滤器组 5 寄存器 1</td><td>X</td></tr><tr><td>R32_CAN2_F5R2</td><td>0x40006A6C</td><td>CAN2 过滤器组 5 寄存器 2</td><td>X</td></tr><tr><td>R32_CAN2_F6R1</td><td>0x40006A70</td><td>CAN2 过滤器组 6 寄存器 1</td><td>X</td></tr><tr><td>R32_CAN2_F6R2</td><td>0x40006A74</td><td>CAN2 过滤器组 6 寄存器 2</td><td>X</td></tr><tr><td>R32_CAN2_F7R1</td><td>0x40006A78</td><td>CAN2 过滤器组 7 寄存器 1</td><td>X</td></tr><tr><td>R32_CAN2_F7R2</td><td>0x40006A7C</td><td>CAN2 过滤器组 7 寄存器 2</td><td>X</td></tr><tr><td>R32_CAN2_F8R1</td><td>0x40006A80</td><td>CAN2 过滤器组 8 寄存器 1</td><td>X</td></tr><tr><td>R32_CAN2_F8R2</td><td>0x40006A84</td><td>CAN2 过滤器组 8 寄存器 2</td><td>X</td></tr><tr><td>R32_CAN2_F9R1</td><td>0x40006A88</td><td>CAN2 过滤器组 9 寄存器 1</td><td>X</td></tr><tr><td>R32_CAN2_F9R2</td><td>0x40006A8C</td><td>CAN2 过滤器组 9 寄存器 2</td><td>X</td></tr><tr><td>R32_CAN2_F10R1</td><td>0x40006A90</td><td>CAN2 过滤器组 10 寄存器 1</td><td>X</td></tr><tr><td>R32_CAN2_F10R2</td><td>0x40006A94</td><td>CAN2 过滤器组 10 寄存器 2</td><td>X</td></tr><tr><td>R32_CAN2_F11R1</td><td>0x40006A98</td><td>CAN2 过滤器组 11 寄存器 1</td><td>X</td></tr><tr><td>R32_CAN2_F11R2</td><td>0x40006A9C</td><td>CAN2 过滤器组 11 寄存器 2</td><td>X</td></tr><tr><td>R32_CAN2_F12R1</td><td>0x40006AA0</td><td>CAN2 过滤器组 12 寄存器 1</td><td>X</td></tr><tr><td>R32_CAN2_F12R2</td><td>0x40006AA4</td><td>CAN2 过滤器组 12 寄存器 2</td><td>X</td></tr><tr><td>R32_CAN2_F13R1</td><td>0x40006AA8</td><td>CAN2 过滤器组 13 寄存器 1</td><td>X</td></tr><tr><td>R32_CAN2_F13R2</td><td>0x40006AAC</td><td>CAN2 过滤器组 13 寄存器 2</td><td>X</td></tr><tr><td>R32_CAN2_F14R1</td><td>0x40006AB0</td><td>CAN2 过滤器组 14 寄存器 1</td><td>X</td></tr><tr><td>R32_CAN2_F14R2</td><td>0x40006AB4</td><td>CAN2 过滤器组 14 寄存器 2</td><td>X</td></tr><tr><td>R32_CAN2_F15R1</td><td>0x40006AB8</td><td>CAN2 过滤器组 15 寄存器 1</td><td>X</td></tr><tr><td>R32_CAN2_F15R2</td><td>0x40006ABC</td><td>CAN2 过滤器组 15 寄存器 2</td><td>X</td></tr><tr><td>R32_CAN2_F16R1</td><td>0x40006AC0</td><td>CAN2 过滤器组 16 寄存器 1</td><td>X</td></tr><tr><td>R32_CAN2_F16R2</td><td>0x40006AC4</td><td>CAN2 过滤器组 16 寄存器 2</td><td>X</td></tr><tr><td>R32_CAN2_F17R1</td><td>0x40006AC8</td><td>CAN2 过滤器组 17 寄存器 1</td><td>X</td></tr><tr><td>R32_CAN2_F17R2</td><td>0x40006ACC</td><td>CAN2 过滤器组 17 寄存器 2</td><td>X</td></tr><tr><td>R32_CAN2_F18R1</td><td>0x40006AD0</td><td>CAN2 过滤器组 18 寄存器 1</td><td>X</td></tr><tr><td>R32_CAN2_F18R2</td><td>0x40006AD4</td><td>CAN2 过滤器组 18 寄存器 2</td><td>X</td></tr><tr><td>R32_CAN2_F19R1</td><td>0x40006AD8</td><td>CAN2 过滤器组 19 寄存器 1</td><td>X</td></tr><tr><td>R32_CAN2_F19R2</td><td>0x40006ADC</td><td>CAN2 过滤器组 19 寄存器 2</td><td>X</td></tr><tr><td>R32_CAN2_F20R1</td><td>0x40006AEO</td><td>CAN2 过滤器组 20 寄存器 1</td><td>X</td></tr><tr><td>R32_CAN2_F20R2</td><td>0x40006AE4</td><td>CAN2 过滤器组 20 寄存器 2</td><td>X</td></tr><tr><td>R32_CAN2_F21R1</td><td>0x40006AE8</td><td>CAN2 过滤器组 21 寄存器 1</td><td>X</td></tr><tr><td>R32_CAN2_F21R2</td><td>0x40006AEC</td><td>CAN2 过滤器组 21 寄存器 2</td><td>X</td></tr><tr><td>R32_CAN2_F22R1</td><td>0x40006AFO</td><td>CAN2 过滤器组 22 寄存器 1</td><td>X</td></tr><tr><td>R32_CAN2_F22R2</td><td>0x40006AF4</td><td>CAN2 过滤器组 22 寄存器 2</td><td>X</td></tr><tr><td>R32_CAN2_F23R1</td><td>0x40006AF8</td><td>CAN2 过滤器组 23 寄存器 1</td><td>X</td></tr><tr><td>R32_CAN2_F23R2</td><td>0x40006AFC</td><td>CAN2 过滤器组 23 寄存器 2</td><td>X</td></tr><tr><td>R32_CAN2_F24R1</td><td>0x40006B00</td><td>CAN2 过滤器组 24 寄存器 1</td><td>X</td></tr><tr><td>R32_CAN2_F24R2</td><td>0x40006B04</td><td>CAN2 过滤器组 24 寄存器 2</td><td>X</td></tr><tr><td>R32_CAN2_F25R1</td><td>0x40006B08</td><td>CAN2 过滤器组 25 寄存器 1</td><td>X</td></tr><tr><td>R32_CAN2_F25R2</td><td>0x40006B0C</td><td>CAN2 过滤器组 25 寄存器 2</td><td>X</td></tr><tr><td>R32_CAN2_F26R1</td><td>0x40006B10</td><td>CAN2 过滤器组 26 寄存器 1</td><td>X</td></tr><tr><td>R32_CAN2_F26R2</td><td>0x40006B14</td><td>CAN2 过滤器组 26 寄存器 2</td><td>X</td></tr><tr><td>R32_CAN2_F27R1</td><td>0x40006B18</td><td>CAN2 过滤器组 27 寄存器 1</td><td>X</td></tr><tr><td>R32_CAN2_F27R2</td><td>0x40006B1C</td><td>CAN2 过滤器组 27 寄存器 2</td><td>X</td></tr></table>

### 24.7.1 CANx 主控制寄存器（CANx_CTLR）（x=1/2）

偏移地址： $0 \times 0 0$

<table><tr><td>31</td><td>30</td><td>29</td><td>28</td><td>27</td><td>26</td><td>25</td><td>24</td><td>23</td><td>22</td><td>21</td><td>20</td><td>19</td><td>18</td><td>17</td><td>16</td></tr><tr><td colspan="15">Reserved</td><td>DBF</td></tr><tr><td>15</td><td>14</td><td>13</td><td>12</td><td>11</td><td>10</td><td>9</td><td>8</td><td>7</td><td>6</td><td>5</td><td>4</td><td>3</td><td>2</td><td>1</td><td>0</td></tr><tr><td>RST</td><td colspan="7">Reserved</td><td>TTCM</td><td>ABOM</td><td>AWUM</td><td>NART</td><td>RFLM</td><td>TXFP</td><td>SLEEP</td><td>INRQ</td></tr></table>

<table><tr><td>位</td><td>名称</td><td>访问</td><td>描述</td><td>复位值</td></tr><tr><td>[31:17]</td><td>Reserved</td><td>R0</td><td>保留。</td><td>0</td></tr><tr><td>16</td><td>DBF</td><td>RW</td><td>调试是否禁止CAN总线工作1:调试时,CAN的收发被禁止,但是接收FIFO0的控制和读写操作一切正常;0:调试时,CAN控制器正常工作。</td><td>1</td></tr><tr><td>15</td><td>RST</td><td>RW1</td><td>CAN控制器软件复位请求,该位写0无效1:对CAN控制器进行复位,复位后控制器进入睡眠模式,然后硬件自动清0;0:CAN控制器正常状态。</td><td>0</td></tr><tr><td>[14:8]</td><td>Reserved</td><td>R0</td><td>保留</td><td>0</td></tr><tr><td>7</td><td>TTCM</td><td>RW</td><td>是否允许时间触发模式1:使能时间触发模式;0:禁止时间触发模式。时间触发模式主要是配合TTCAN协议使用。</td><td>0</td></tr><tr><td>6</td><td>ABOM</td><td>RW</td><td>离线自动退出控制1:硬件检测到128次连续11个隐性位,自</td><td>0</td></tr><tr><td></td><td></td><td></td><td>动退出离线状态;0:需要软件操作寄存器 CAN_CTLR 的 INRQ 位置 1 然后清 0,当检测到 128 次连续 11 个隐性位后,退出离线状态。</td><td></td></tr><tr><td>5</td><td>AWUM</td><td>RW</td><td>CAN 控制器自动唤醒使能1:当检测到报文时,硬件自动唤醒,寄存器CAN_STATR 的 SLEEP 和 SLAK 位自动清 0;0:需要软件操作寄存器 CAN_CTLR 的 SLEEP 位清 0,唤醒 CAN 控制器。</td><td>0</td></tr><tr><td>4</td><td>NART</td><td>RW</td><td>报文自动重传功能禁止1:无论发送成功与否,报文只能被发送一次;0:CAN 控制器一直重传至发送成功为止。</td><td>0</td></tr><tr><td>3</td><td>RFLM</td><td>RW</td><td>接收 FIFO 报文锁定模式使能1:当接收 FIFO 溢出时,已接收邮箱报文未读出,邮箱未释放时,新接收到的报文被丢弃;0:当接收 FIFO 溢出时,已接收邮箱报文未读出,邮箱未释放时,新接收到的报文会覆盖原有报文。</td><td>0</td></tr><tr><td>2</td><td>TXFP</td><td>RW</td><td>发送邮箱优先级方式选择1:优先级由发送请求的先后顺序决定;0:优先级由报文标识符来决定。</td><td>0</td></tr><tr><td>1</td><td>SLEEP</td><td>RW</td><td>睡眠模式请求位1:置1 请求 CAN 控制器进入睡眠模式,当前活动完成后,控制器进入睡眠模式,若 AWUM 位置 1 ,则在接收到报文时,控制器把 SLEEP 位清 0;0:软件清 0 后,控制器退出睡眠模式。</td><td>1</td></tr><tr><td>0</td><td>INRQ</td><td>RW</td><td>初始化模式请求位1:置1 请求 CAN 控制器进入初始化模式,当前活动完成后,控制器进入初始化模式,硬件对寄存器 CAN_STATR 的 INAK 位置 1 ;0:置0 请求 CAN 控制器退出初始化模式,进入正常模式,硬件对寄存器 CAN_STATR 的 INAK 位清 0。</td><td>0</td></tr></table>

### 24.7.2 CANx 主状态寄存器（CANx_STATR）（x=1/2）

偏移地址： $0 \times 0 4$

<table><tr><td>31</td><td>30</td><td>29</td><td>28</td><td>27</td><td>26</td><td>25</td><td>24</td><td>23</td><td>22</td><td>21</td><td>20</td><td>19</td><td>18</td><td>17</td><td>16</td></tr><tr><td colspan="16">Reserved</td></tr><tr><td>15</td><td>14</td><td>13</td><td>12</td><td>11</td><td>10</td><td>9</td><td>8</td><td>7</td><td>6</td><td>5</td><td>4</td><td>3</td><td>2</td><td>1</td><td>0</td></tr><tr><td colspan="3">Reserved</td><td>RX</td><td>SAMP</td><td>RXM</td><td>TXM</td><td colspan="3">Reserved</td><td>SLAKI</td><td>WKUI</td><td>ERRI</td><td>SLAK</td><td>INAK</td><td></td></tr></table>

<table><tr><td>位</td><td>名称</td><td>访问</td><td>描述</td><td>复位值</td></tr><tr><td>[31:12]</td><td>Reserved</td><td>R0</td><td>保留。</td><td>0</td></tr><tr><td>11</td><td>RX</td><td>R0</td><td>CAN控制器接收引脚RX当前实际电平。</td><td>1</td></tr><tr><td>10</td><td>SAMP</td><td>R0</td><td>CAN控制器接收引脚RX上一个接收位的电平</td><td>1</td></tr><tr><td>9</td><td>RXM</td><td>R0</td><td>接收模式查询位1:当前CAN控制器为接收模式;0:当前CAN控制器非接收模式。</td><td>0</td></tr><tr><td>8</td><td>TXM</td><td>R0</td><td>发送模式查询位1:当前CAN控制器为发送模式;0:当前CAN控制器非发送模式。</td><td>0</td></tr><tr><td>[7:5]</td><td>Reserved</td><td>R0</td><td>保留</td><td>0</td></tr><tr><td>4</td><td>SLAKI</td><td>RW1</td><td>睡眠中断使能时,即寄存器CAN_INTENR的SLKIE位置1时,中断产生标志位,写1清0,写0无效。1:进入睡眠模式时,中断产生,硬件置1;0:退出睡眠模式时,硬件清0也可软件清0。</td><td>0</td></tr><tr><td>3</td><td>WKUI</td><td>RW1</td><td>唤醒中断标志位。当寄存器CAN_INTENR的WKUI位置1时,若CAN控制器处于睡眠模式时,检测到SOF位,则硬件置1。软件置1清0,置0无效。</td><td>0</td></tr><tr><td>2</td><td>ERRI</td><td>RW1</td><td>出错中断状态标志位。当寄存器CAN_INTENR的ERRIE位置1时,产生错误及状态变化中断。该位软件置1清0,置0无效。</td><td>0</td></tr><tr><td>1</td><td>SLAK</td><td>R0</td><td>睡眠模式指示位。1:CAN控制器正处于睡眠模式;0:CAN控制器不在睡眠模式。</td><td>1</td></tr><tr><td>0</td><td>INAK</td><td>R0</td><td>初始化模式指示位。1:CAN控制器正在初始化模式;0:CAN控制器工作在非初始化模式。</td><td>0</td></tr></table>

### 24.7.3 CANx 发送状态寄存器（CANx_TSTATR）（x=1/2）

偏移地址： $0 \times 0 8$

<table><tr><td>31</td><td>30</td><td>29</td><td>28</td><td>27</td><td>26</td><td>25</td><td>24</td><td>23</td><td>22</td><td>21</td><td>20</td><td>19</td><td>18</td><td>17</td><td>16</td></tr><tr><td>LOW2</td><td>LOW1</td><td>LOWO</td><td>TME2</td><td>TME1</td><td>TME0</td><td colspan="2">CODE[1:0]</td><td>ABRQ2</td><td colspan="3">Reserved</td><td>TERR2</td><td>ALST2</td><td>TXOK2</td><td>RQCP2</td></tr><tr><td>15</td><td>14</td><td>13</td><td>12</td><td>11</td><td>10</td><td>9</td><td>8</td><td>7</td><td>6</td><td>5</td><td>4</td><td>3</td><td>2</td><td>1</td><td>0</td></tr><tr><td>ABRQ1</td><td colspan="3">Reserved</td><td>TERR1</td><td>ALST1</td><td>TXOK1</td><td>RQCP1</td><td>ABRQ0</td><td colspan="3">Reserved</td><td>TERRO</td><td>ALST0</td><td>TXOK0</td><td>RQCP0</td></tr></table>

<table><tr><td>位</td><td>名称</td><td>访问</td><td>描述</td><td>复位值</td></tr><tr><td>31</td><td>LOW2</td><td>R0</td><td>发送邮箱2的最低优先级标志位1:表示发送邮箱2的优先级最低;0:表示发送邮箱2的优先级非最低。</td><td>0</td></tr><tr><td>30</td><td>LOW1</td><td>R0</td><td>发送邮箱1的最低优先级标志位1:表示发送邮箱1的优先级最低;0:表示发送邮箱1的优先级非最低。</td><td>0</td></tr><tr><td>29</td><td>LOWO</td><td>R0</td><td>发送邮箱0的最低优先级标志位1:表示发送邮箱0的优先级最低;</td><td>0</td></tr><tr><td></td><td></td><td></td><td>0:表示发送邮箱0的优先级非最低。</td><td></td></tr><tr><td>28</td><td>TME2</td><td>R0</td><td>发送邮箱2的空标志位1:表示发送邮箱2无等待发送报文;0:表示发送邮箱2有等待发送报文。</td><td>1</td></tr><tr><td>27</td><td>TME1</td><td>R0</td><td>发送邮箱1的空标志位1:表示发送邮箱1无等待发送报文;0:表示发送邮箱1有等待发送报文。</td><td>1</td></tr><tr><td>26</td><td>TME0</td><td>R0</td><td>发送邮箱0的空标志位1:表示发送邮箱0无等待发送报文;0:表示发送邮箱0有等待发送报文。</td><td>1</td></tr><tr><td>[25:24]</td><td>CODE[1:0]</td><td>R0</td><td>邮箱编号当有1个以上邮箱为空时,表示下一个为空的邮箱号;当邮箱全空时,表示优先级最低的邮箱号。</td><td>0</td></tr><tr><td>23</td><td>ABRQ2</td><td>RW1</td><td>发送邮箱2的发送中止请求。软件置1,可以中止邮箱2的发送请求,发送报文被清除时硬件清0,若邮箱2清空,软件置1无效。</td><td>0</td></tr><tr><td>[22:20]</td><td>Reserved</td><td>R0</td><td>保留</td><td>0</td></tr><tr><td>19</td><td>TERR2</td><td>RW1</td><td>发送邮箱2发送失败标志位,当发送邮箱2发送失败,该位自动置1。软件置1清0,软件写0无效。</td><td>0</td></tr><tr><td>18</td><td>ALST2</td><td>RW1</td><td>发送邮箱2仲裁失败标志位,当发送邮箱2仲裁优先级低导致发送失败,该位自动置1。软件置1清0,软件写0无效。</td><td>0</td></tr><tr><td>17</td><td>TXOK2</td><td>RW1</td><td>发送邮箱2发送成功标志位1:上次发送成功;0:上次发送失败。软件置1清0,软件写0无效。</td><td>0</td></tr><tr><td>16</td><td>RQCP2</td><td>RW</td><td>发送邮箱2请求完成标志位,当发送邮箱2的发送或中止请求完成时,该位自动置1。软件置1清0,软件写0无效。</td><td>0</td></tr><tr><td>15</td><td>ABRQ1</td><td>RW0</td><td>发送邮箱1的发送中止请求。软件置1,可以中止邮箱1的发送请求,发送报文被清除时硬件清0。软件写0无效。</td><td>0</td></tr><tr><td>[14:12]</td><td>Reserved</td><td>R0</td><td>保留</td><td>0</td></tr><tr><td>11</td><td>TERR1</td><td>RW1</td><td>发送邮箱1发送失败标志位,当发送邮箱1发送失败,该位自动置1。软件置1清0,软件写0无效。</td><td>0</td></tr><tr><td>10</td><td>ALST1</td><td>RW1</td><td>发送邮箱1仲裁失败标志位,当发送邮箱1仲裁优先级低导致发送失败,该位自动置1。</td><td>0</td></tr><tr><td>9</td><td>TXOK1</td><td>RW1</td><td>发送邮箱1发送成功标志位1:上次发送成功;0:上次发送失败。软件置1清0,软件写0无效。</td><td>0</td></tr><tr><td>8</td><td>RQCP1</td><td>RW</td><td>发送邮箱1请求完成标志位,当发送邮箱1</td><td>0</td></tr><tr><td></td><td></td><td></td><td>的发送或中止请求完成时,该位自动置1。软件置1清0,软件写0无效。</td><td></td></tr><tr><td>7</td><td>ABRQ0</td><td>RWO</td><td>发送邮箱0的发送中止请求。软件置1,可以中止邮箱0的发送请求,发送报文被清除时硬件清0。软件写0无效。</td><td>0</td></tr><tr><td>[6:4]</td><td>Reserved</td><td>R0</td><td>保留</td><td>0</td></tr><tr><td>3</td><td>TERRO</td><td>RW1</td><td>发送邮箱0发送失败标志位,当发送邮箱0发送失败,该位自动置1。软件置1清0,软件写0无效。</td><td>0</td></tr><tr><td>2</td><td>ALST0</td><td>RW1</td><td>发送邮箱0仲裁失败标志位,当发送邮箱0仲裁优先级低导致发送失败,该位自动置1。软件置1清0,软件写0无效。</td><td>0</td></tr><tr><td>1</td><td>TXOKO</td><td>RW1</td><td>发送邮箱0发送成功标志位1:上次发送成功;0:上次发送失败。软件置1清0,软件写0无效。</td><td>0</td></tr><tr><td>0</td><td>RQCP0</td><td>RW</td><td>发送邮箱0请求完成标志位,当发送邮箱0的发送或中止请求完成时,该位自动置1。软件置1清0,软件写0无效。</td><td>0</td></tr></table>

### 24.7.4 CANx 接收 FIFO 0 状态寄存器（CANx_RFIFO0）（x=1/2）

偏移地址： $0 \times 0 0$

<table><tr><td>31</td><td>30</td><td>29</td><td>28</td><td>27</td><td>26</td><td>25</td><td>24</td><td>23</td><td>22</td><td>21</td><td>20</td><td>19</td><td>18</td><td>17</td><td>16</td></tr><tr><td colspan="16">Reserved</td></tr><tr><td>15</td><td>14</td><td>13</td><td>12</td><td>11</td><td>10</td><td>9</td><td>8</td><td>7</td><td>6</td><td>5</td><td>4</td><td>3</td><td>2</td><td>1</td><td>0</td></tr><tr><td colspan="10">Reserved</td><td>RFOMO</td><td>FOVRO</td><td>FULLO</td><td colspan="2">Reser ved</td><td>FMP0[1:0]</td></tr></table>

<table><tr><td>位</td><td>名称</td><td>访问</td><td>描述</td><td>复位值</td></tr><tr><td>[31:6]</td><td>Reserved</td><td>R0</td><td>保留。</td><td>0</td></tr><tr><td>5</td><td>RFOMO</td><td>RW1</td><td>软件对该位置1,则释放接收FIFO00的当前邮箱报文,释放完后自动清0,软件写0无效。</td><td>0</td></tr><tr><td>4</td><td>FOVRO</td><td>RW1</td><td>接收FIFO00溢出标志位。当FIFO00中有三个报文时,又接到新报文,硬件置1。该位需要软件置1清0,软件写0无效。</td><td>0</td></tr><tr><td>3</td><td>FULL0</td><td>RW1</td><td>接收FIFO00满标志位。当FIFO00中有三个报文时,硬件置1。该位需要软件置1清0,软件写0无效。</td><td>0</td></tr><tr><td>2</td><td>Reserved</td><td>R0</td><td>保留。</td><td>0</td></tr><tr><td>[1:0]</td><td>FMP0[1:0]</td><td>R0</td><td>接收FIFO00报文数目。</td><td>0</td></tr></table>

### 24.7.5 CANx 接收 FIFO 1 状态寄存器（CANx_RFIFO1）（x=1/2）

偏移地址： $0 \times 1 0$

<table><tr><td>31</td><td>30</td><td>29</td><td>28</td><td>27</td><td>26</td><td>25</td><td>24</td><td>23</td><td>22</td><td>21</td><td>20</td><td>19</td><td>18</td><td>17</td><td>16</td></tr><tr><td colspan="16">Reserved</td></tr><tr><td>15</td><td>14</td><td>13</td><td>12</td><td>11</td><td>10</td><td>9</td><td>8</td><td>7</td><td>6</td><td>5</td><td>4</td><td>3</td><td>2</td><td>1</td><td>0</td></tr><tr><td colspan="10">Reserved</td><td>RF0M1</td><td>FOVR1</td><td>FULL1</td><td colspan="2">Reser ved</td><td>FMP1[1:0]</td></tr></table>

<table><tr><td>位</td><td>名称</td><td>访问</td><td>描述</td><td>复位值</td></tr><tr><td>[31:6]</td><td>Reserved</td><td>R0</td><td>保留。</td><td>0</td></tr><tr><td>5</td><td>RF0M1</td><td>RW1</td><td>软件对该位置1,则释放接收FIFO_1的当前邮箱报文,释放完后自动清0,软件写0无效。</td><td>0</td></tr><tr><td>4</td><td>FOVR1</td><td>RW1</td><td>接收FIFO_1溢出标志位。当FIFO_1中有三个报文时,又接到新报文,硬件置1。该位需要软件置1清0,软件写0无效。</td><td>0</td></tr><tr><td>3</td><td>FULL1</td><td>RW1</td><td>接收FIFO_1满标志位。当FIFO_1中有三个报文时,硬件置1。该位需要软件置1清0,软件写0无效。</td><td>0</td></tr><tr><td>2</td><td>Reserved</td><td>RF</td><td>保留</td><td>0</td></tr><tr><td>[1:0]</td><td>FMP1[1:0]</td><td>R0</td><td>接收FIFO_1报文数目。</td><td>0</td></tr></table>

### 24.7.6 CANx 中断使能寄存器（CANx_INTENR）（x=1/2）

偏移地址：0x14

<table><tr><td>31</td><td>30</td><td>29</td><td>28</td><td>27</td><td>26</td><td>25</td><td>24</td><td>23</td><td>22</td><td>21</td><td>20</td><td>19</td><td>18</td><td>17</td><td>16</td><td></td></tr><tr><td colspan="15">Reserved</td><td>SLKIE</td><td>WKUIE</td></tr><tr><td>15</td><td>14</td><td>13</td><td>12</td><td>11</td><td>10</td><td>9</td><td>8</td><td>7</td><td>6</td><td>5</td><td>4</td><td>3</td><td>2</td><td>1</td><td colspan="2">0</td></tr><tr><td>ERRIE</td><td colspan="2">Reserved</td><td>LECIE</td><td>BOFIE</td><td>EPVIE</td><td>EWGIE</td><td colspan="2">Reser ved</td><td>FOVIE1</td><td>FFIE1</td><td>FMPIE1</td><td>FOVIE0</td><td>FFIE0</td><td>FMPIE0</td><td colspan="2">TMEIE</td></tr></table>

<table><tr><td>位</td><td>名称</td><td>访问</td><td>描述</td><td>复位值</td></tr><tr><td>[31:18]</td><td>Reserved</td><td>R0</td><td>保留。</td><td>0</td></tr><tr><td>17</td><td>SLKIE</td><td>RW</td><td>睡眠中断使能位。
1: 进入睡眠状态时,产生中断;
0: 进入睡眠状态时,不产生中断。</td><td>0</td></tr><tr><td>16</td><td>WKUIE</td><td>RW</td><td>唤醒中断使能位。
1: 当 CAN 控制器被唤醒时,产生中断;
0: 当 CAN 控制器被唤醒时,不产生中断。</td><td>0</td></tr><tr><td>15</td><td>ERRIE</td><td>RW</td><td>错误中断使能位, CAN 错误中断总使能位。
1: 当 CAN 控制器产生错误时,产生中断;
0: 当 CAN 控制器产生错误时,不产生中断。</td><td>0</td></tr><tr><td>[14:12]</td><td>Reserved</td><td>RF</td><td>保留。</td><td>0</td></tr><tr><td>11</td><td>LECIE</td><td>RW</td><td>上次错误号中断使能位。
1: 检测到错误时,硬件更新 LEC[2:0],更新ERRI位为1,触发错误中断;</td><td>0</td></tr><tr><td></td><td></td><td></td><td>0:检测到错误时,硬件更新LEC[2:0],不更新ERRI位,不触发错误中断。</td><td></td></tr><tr><td>10</td><td>BOFIE</td><td>RW</td><td>离线中断使能位。1:进入离线状态时,更新ERRI位为1,触发错误中断;0:进入离线状态时,不更新ERRI位,不触发错误中断。</td><td>0</td></tr><tr><td>9</td><td>EPVIE</td><td>RW</td><td>错误被动中断使能位。1:进入错误被动状态时,更新ERRI位为1,触发错误中断;0:进入错误被动状态时,不更新ERRI位,不触发错误中断。</td><td>0</td></tr><tr><td>8</td><td>EWGIE</td><td>RW</td><td>错误警告中断使能位。1:出错次数达到警告阈值时,更新ERRI位为1,触发错误中断;0:出错次数达到警告阈值时,不更新ERRI位,不触发错误中断。</td><td>0</td></tr><tr><td>7</td><td>Reserved</td><td>RF</td><td>保留。</td><td>0</td></tr><tr><td>6</td><td>FOVIE1</td><td>RW</td><td>接收FIFO_1溢出中断使能位。1:当FIFO_1溢出,触发FIFO_1中断;0:当FIFO_1溢出,不触发FIFO_1中断。</td><td>0</td></tr><tr><td>5</td><td>FFIE1</td><td>RW</td><td>接收FIFO_1满中断使能位。1:当FIFO_1为满,触发FIFO_1中断;0:当FIFO_1为满,不触发FIFO_1中断。</td><td>0</td></tr><tr><td>4</td><td>FMPIE1</td><td>RW</td><td>接收FIFO_1消息挂号中断使能位。1:当FIFO_1更新FMP位,且不为0,触发FIFO_1中断;0:当FIFO_1更新FMP位,且不为0,不触发FIFO_1中断。</td><td>0</td></tr><tr><td>3</td><td>FOVIE0</td><td>RW</td><td>接收FIFO_0溢出中断使能位。1:当FIFO_0溢出,触发FIFO_0中断;0:当FIFO_0溢出,不触发FIFO_0中断。</td><td>0</td></tr><tr><td>2</td><td>FFIE0</td><td>RW</td><td>接收FIFO_0满中断使能位。1:当FIFO_0为满,触发FIFO_0中断;0:当FIFO_0为满,不触发FIFO_0中断。</td><td>0</td></tr><tr><td>1</td><td>FMPIE0</td><td>RW</td><td>接收FIFO_0消息挂号中断使能位。1:当FIFO_0更新FMP位,且不为0,触发FIFO_0中断;0:当FIFO_0更新FMP位,且不为0,不触发FIFO_0中断。</td><td>0</td></tr><tr><td>0</td><td>TMEIE</td><td>RW</td><td>发送邮箱空中断。1:当发送邮箱为空时,产生中断;0:当发送邮箱为空时,不产生中断。</td><td>0</td></tr></table>

### 24.7.7 CANx 错误状态寄存器（CANx_ERRSR）（x=1/2）

偏移地址：0x18

<table><tr><td>31</td><td>30</td><td>29</td><td>28</td><td>27</td><td>26</td><td>25</td><td>24</td><td>23</td><td>22</td><td>21</td><td>20</td><td>19</td><td>18</td><td>17</td><td>16</td></tr><tr><td colspan="8">REC[7:0]</td><td colspan="8">TEC[7:0]</td></tr><tr><td>15</td><td>14</td><td>13</td><td>12</td><td>11</td><td>10</td><td>9</td><td>8</td><td>7</td><td>6</td><td>5</td><td>4</td><td>3</td><td>2</td><td>1</td><td>0</td></tr><tr><td colspan="8">Reserved</td><td colspan="2">LEC[2:0]</td><td colspan="2">Reser ved</td><td>BOFF</td><td colspan="2">EPVF</td><td>EWGF</td></tr></table>

<table><tr><td>位</td><td>名称</td><td>访问</td><td>描述</td><td>复位值</td></tr><tr><td>[31:24]</td><td>REC[7:0]</td><td>R0</td><td>接收错误计数器。当CAN接收出错时,根据出错条件,该计数器加1或8;接收成功后,该计数器减1或设为120(错误计数值大于127)。计数器值超过127时,CAN进入错误被动状态。</td><td>0</td></tr><tr><td>[23:16]</td><td>TEC[7:0]</td><td>R0</td><td>发送错误计数器。当CAN发送出错时,根据出错条件,该计数器加1或8;发送成功后,该计数器减1或设为120(错误计数值大于127)。计数器值超过127时,CAN进入错误被动状态。</td><td>0</td></tr><tr><td>[15:7]</td><td>Reserved</td><td>R0</td><td>保留。</td><td>0</td></tr><tr><td>[6:4]</td><td>LEC[2:0]</td><td>RW</td><td>上次错误代号。检测到CAN总线上发送错误时,控制器根据出错情况设置,当正确收发报文时,置000b。000:无错误;001:位填充错误;010:FORM格式错误;011:ACK确认错误;100:隐性位错误;101:显性位错误;110:CRC错误;111:软件设置。通常应用软件读取到错误时,把代号设置为111b,可以检测到代号更新。</td><td>000b</td></tr><tr><td>3</td><td>Reserved</td><td>R0</td><td>保留。</td><td>0</td></tr><tr><td>2</td><td>BOFF</td><td>R0</td><td>离线状态标志位。当CAN控制器进入离线状态时,硬件自动置1;退出离线状态时,硬件自动清0。</td><td>0</td></tr><tr><td>1</td><td>EPVF</td><td>R0</td><td>错误被动标志位。当收发错误计数器达到错误被动阈值时,即大于127时,硬件置1。</td><td>0</td></tr><tr><td>0</td><td>EWGF</td><td>R0</td><td>错误警告标志位。当收发错误计数器达到警告阈值时,即大于等于96时,硬件置1。</td><td>0</td></tr></table>

### 24.7.8 CANx 位时序寄存器（CANx_BTIMR）（x=1/2）

偏移地址：0x1C

<table><tr><td>31</td><td>30</td><td>29</td><td>28</td><td>27</td><td>26</td><td>25</td><td>24</td><td>23</td><td>22</td><td>21</td><td>20</td><td>19</td><td>18</td><td>17</td><td>16</td></tr><tr><td>SILM</td><td>LBKM</td><td colspan="4">Reserved</td><td colspan="2">SJW[1:0]</td><td>Reserved</td><td colspan="3">TS2[2:0]</td><td colspan="4">TS1[3:0]</td></tr></table>

<table><tr><td>15</td><td>14</td><td>13</td><td>12</td><td>11</td><td>10</td><td>9</td><td>8</td><td>7</td><td>6</td><td>5</td><td>4</td><td>3</td><td>2</td><td>1</td><td>0</td></tr><tr><td colspan="6">Reserved</td><td colspan="10">BRP[9:0]</td></tr></table>

<table><tr><td>位</td><td>名称</td><td>访问</td><td>描述</td><td>复位值</td></tr><tr><td>31</td><td>SILM</td><td>RW</td><td>静默模式设置位。1:进入静默模式;0:退出静默模式。</td><td>0</td></tr><tr><td>30</td><td>LBKM</td><td>RW</td><td>环回模式设置位。1:进入环回模式;0:退出环回模式。</td><td>0</td></tr><tr><td>[29:26]</td><td>Reserved</td><td>R0</td><td>保留。</td><td>0</td></tr><tr><td>[25:24]</td><td>SJW[1:0]</td><td>RW</td><td>定义了重新同步跳转宽度设置值。实现重新同步时,位中可以延长和缩小的最小时间单元数量上限,实际值为(SJW[1:0]+1),范围可设置为1到4个最小时时间单元。</td><td>01b</td></tr><tr><td>23</td><td>Reserved</td><td>R0</td><td>保留。</td><td>0</td></tr><tr><td>[22:20]</td><td>TS2[2:0]</td><td>RW</td><td>时间段2设置值。定义了时间段2占用了多少个最小时间单元,实际值为(TS2[2:0]+1)。</td><td>010b</td></tr><tr><td>[19:16]</td><td>TS1[3:0]</td><td>RW</td><td>时间段1设置值。定义了时间段1占用了多少个最小时间单元,实际值为(TS1[3:0]+1)。</td><td>0011b</td></tr><tr><td>[15:10]</td><td>Reserved</td><td>R0</td><td>保留。</td><td>0</td></tr><tr><td>[9:0]</td><td>BRP[9:0]</td><td>RW</td><td>最小时间单元长度设置值Tq=(BRP[9:0]+1)×tpclk注:CAN波特率计算公式为:CANbps=PCLK1/((TSI[3:0]+1+TS2[2:0]+1+1)*(BPR[9:0]+1))</td><td>0</td></tr></table>

### 24.7.9 CANx 时间触发控制寄存器（CANx_TTCTLR）（x=1/2）

偏移地址： $_ { 0 \times 2 0 }$

<table><tr><td>31</td><td>30</td><td>29</td><td>28</td><td>27</td><td>26</td><td>25</td><td>24</td><td>23</td><td>22</td><td>21</td><td>20</td><td>19</td><td>18</td><td>17</td><td>16</td></tr><tr><td colspan="14">Reserved</td><td>MODE</td><td>TIMRS T</td></tr><tr><td>15</td><td>14</td><td>13</td><td>12</td><td>11</td><td>10</td><td>9</td><td>8</td><td>7</td><td>6</td><td>5</td><td>4</td><td>3</td><td>2</td><td>1</td><td>0</td></tr><tr><td colspan="16">TIMCMV [15:0]</td></tr></table>

<table><tr><td>位</td><td>名称</td><td>访问</td><td>描述</td><td>复位值</td></tr><tr><td>[31:18]</td><td>Reserved</td><td>R0</td><td>保留。</td><td>0</td></tr><tr><td>17</td><td>MODE</td><td>RW</td><td>时间触发模式选择位。
1: 增强模式;
0: 默认模式。</td><td>0</td></tr><tr><td>16</td><td>TIMRST</td><td>WZ</td><td>内部计数器复位控制位。
写1复位内部计数器, 硬件自动清0</td><td>0</td></tr><tr><td>[15:0]</td><td>TIMCMV[15:0]</td><td>RW</td><td>内部计数器计数终值</td><td>fffffh</td></tr></table>

### 24.7.10 CANx 时间触发计数值寄存器（CANx_TTCNT）（x=1/2）

偏移地址： ${ 0 } { \times 2 4 }$

<table><tr><td>31</td><td>30</td><td>29</td><td>28</td><td>27</td><td>26</td><td>25</td><td>24</td><td>23</td><td>22</td><td>21</td><td>20</td><td>19</td><td>18</td><td>17</td><td>16</td></tr><tr><td colspan="16">Reserved</td></tr><tr><td>15</td><td>14</td><td>13</td><td>12</td><td>11</td><td>10</td><td>9</td><td>8</td><td>7</td><td>6</td><td>5</td><td>4</td><td>3</td><td>2</td><td>1</td><td>0</td></tr><tr><td colspan="16">TIMCNT[15:0]</td></tr></table>

<table><tr><td>位</td><td>名称</td><td>访问</td><td>描述</td><td>复位值</td></tr><tr><td>[31:16]</td><td>Reserved</td><td>R0</td><td>保留。</td><td>0</td></tr><tr><td>[15:0]</td><td>TIMCNT[15:0]</td><td>RW</td><td>时间触发计数值</td><td>0</td></tr></table>

### 24.7.11 CANx 发送邮箱标识符寄存器（CANx_TXMIRy）（x=0/1,y=0/1/2）

偏移地址：0x180,0x190,0x1A0

<table><tr><td>31</td><td>30</td><td>29</td><td>28</td><td>27</td><td>26</td><td>25</td><td>24</td><td>23</td><td>22</td><td>21</td><td>20</td><td>19</td><td>18</td><td>17</td><td>16</td></tr><tr><td colspan="11">STID[10:0]/EXID[28:18]</td><td colspan="5">EXID[17:13]</td></tr><tr><td>15</td><td>14</td><td>13</td><td>12</td><td>11</td><td>10</td><td>9</td><td>8</td><td>7</td><td>6</td><td>5</td><td>4</td><td>3</td><td>2</td><td>1</td><td>0</td></tr><tr><td colspan="12">EXID[12:0]</td><td>IDE</td><td>RTR</td><td colspan="2">TXRQ</td></tr></table>

<table><tr><td>位</td><td>名称</td><td>访问</td><td>描述</td><td>复位值</td></tr><tr><td>[31:21]</td><td>STID[10:0] /EXID[28:18]</td><td>RW</td><td>标准标识符或扩展标识符的高11位。</td><td>x</td></tr><tr><td>[20:3]</td><td>EXID[17:0]</td><td>RW</td><td>扩展标识符的低18位。</td><td>x</td></tr><tr><td>2</td><td>IDE</td><td>RW</td><td>标识符选择标志位。1:选用扩展标识符;0:选用标准标识符。</td><td>x</td></tr><tr><td>1</td><td>RTR</td><td>RW</td><td>远程帧(也称遥控帧)选择标志位。1:当前为远程帧;0:当前为数据帧。</td><td>x</td></tr><tr><td>0</td><td>TXRQ</td><td>RW</td><td>数据发送请求标志位。软件置1时,请求发送邮箱里的数据,发送完毕邮箱为空时,硬件清0。</td><td>0</td></tr></table>

### 24.7.12 CANx 发送邮箱数据长度和时间戳寄存器（CANx_TXMDTRy）（x=0/1,y=0/1/2）

偏移地址：0x184,0x194,0x1A4

<table><tr><td>31</td><td>30</td><td>29</td><td>28</td><td>27</td><td>26</td><td>25</td><td>24</td><td>23</td><td>22</td><td>21</td><td>20</td><td>19</td><td>18</td><td>17</td><td>16</td></tr><tr><td colspan="16">TIME[15:0]</td></tr><tr><td>15</td><td>14</td><td>13</td><td>12</td><td>11</td><td>10</td><td>9</td><td>8</td><td>7</td><td>6</td><td>5</td><td>4</td><td>3</td><td>2</td><td>1</td><td>0</td></tr><tr><td colspan="7">Reserved</td><td>TGT</td><td colspan="4">Reserved</td><td colspan="4">DLC[3:0]</td></tr></table>

<table><tr><td>位</td><td>名称</td><td>访问</td><td>描述</td><td>复位值</td></tr><tr><td>[31:16]</td><td>TIME[15:0]</td><td>RW</td><td>用于发送报文SOF时刻的16位定时器值。</td><td>x</td></tr><tr><td>[15:9]</td><td>Reserved</td><td>RO</td><td>保留。</td><td>0</td></tr><tr><td>8</td><td>TGT</td><td>RW</td><td>报文时间戳发送选择标志位。该位在TTCM置1，并报文长度为8时有效。1：发送时间戳，值为TIME[15:0]的即时值，替换了8字节报文的最后两个字节；0：不发送时间戳。</td><td>x</td></tr><tr><td>[7:4]</td><td>Reserved</td><td>RO</td><td>保留。</td><td>0</td></tr><tr><td>[3:0]</td><td>DLC[3:0]</td><td>RW</td><td>数据帧的数据长度或远程帧请求数据长度数据长度可设置范围为0到8。</td><td>0</td></tr></table>

### 24.7.13 CANx 发送邮箱低字节数据寄存器（CANx_TXMDLRy）（x=0/1,y=0/1/2）

偏移地址：0x188,0x198,0x1A8

<table><tr><td>31</td><td>30</td><td>29</td><td>28</td><td>27</td><td>26</td><td>25</td><td>24</td><td>23</td><td>22</td><td>21</td><td>20</td><td>19</td><td>18</td><td>17</td><td>16</td></tr><tr><td colspan="8">DATA3[7:0]</td><td colspan="8">DATA2[7:0]</td></tr><tr><td>15</td><td>14</td><td>13</td><td>12</td><td>11</td><td>10</td><td>9</td><td>8</td><td>7</td><td>6</td><td>5</td><td>4</td><td>3</td><td>2</td><td>1</td><td>0</td></tr><tr><td colspan="8">DATA1[7:0]</td><td colspan="8">DATA0[7:0]</td></tr></table>

<table><tr><td>位</td><td>名称</td><td>访问</td><td>描述</td><td>复位值</td></tr><tr><td>[31:24]</td><td>DATA3[7:0]</td><td>RW</td><td>发送数据字节3的内容。</td><td>xxh</td></tr><tr><td>[23:16]</td><td>DATA2[7:0]</td><td>RW</td><td>发送数据字节2的内容。</td><td>xxh</td></tr><tr><td>[15:8]</td><td>DATA1[7:0]</td><td>RW</td><td>发送数据字节1的内容。</td><td>xxh</td></tr><tr><td>[7:0]</td><td>DATA0[7:0]</td><td>RW</td><td>发送数据字节0的内容。</td><td>xxh</td></tr></table>

### 24.7.14 CANx 发送邮箱高字节数据寄存器（CANx_TXMDHRy）（x=0/1,y=0/1/2）

偏移地址：0x18C,0x19C,0x1AC

<table><tr><td>31</td><td>30</td><td>29</td><td>28</td><td>27</td><td>26</td><td>25</td><td>24</td><td>23</td><td>22</td><td>21</td><td>20</td><td>19</td><td>18</td><td>17</td><td>16</td></tr><tr><td colspan="8">DATA7[7:0]</td><td colspan="8">DATA6[7:0]</td></tr><tr><td>15</td><td>14</td><td>13</td><td>12</td><td>11</td><td>10</td><td>9</td><td>8</td><td>7</td><td>6</td><td>5</td><td>4</td><td>3</td><td>2</td><td>1</td><td>0</td></tr><tr><td colspan="8">DATA5[7:0]</td><td colspan="8">DATA4[7:0]</td></tr></table>

<table><tr><td>位</td><td>名称</td><td>访问</td><td>描述</td><td>复位值</td></tr><tr><td>[31:24]</td><td>DATA7[7:0]</td><td>RW</td><td>发送数据字节7的内容。</td><td>x</td></tr><tr><td>[23:16]</td><td>DATA6[7:0]</td><td>RW</td><td>发送数据字节6的内容。</td><td>x</td></tr><tr><td>[15:8]</td><td>DATA5[7:0]</td><td>RW</td><td>发送数据字节5的内容。</td><td>x</td></tr><tr><td>[7:0]</td><td>DATA4[7:0]</td><td>RW</td><td>发送数据字节4的内容。</td><td>x</td></tr></table>

### 24.7.15 CANx 接收邮箱标识符寄存器（CANx_RXMIRy）（x=0/1,y=0/1）

偏移地址：0x1B0,0x1C0

<table><tr><td>31</td><td>30</td><td>29</td><td>28</td><td>27</td><td>26</td><td>25</td><td>24</td><td>23</td><td>22</td><td>21</td><td>20</td><td>19</td><td>18</td><td>17</td><td>16</td></tr><tr><td colspan="11">STID[10:0]/EXID[28:18]</td><td colspan="5">EXID[17:13]</td></tr><tr><td>15</td><td>14</td><td>13</td><td>12</td><td>11</td><td>10</td><td>9</td><td>8</td><td>7</td><td>6</td><td>5</td><td>4</td><td>3</td><td>2</td><td>1</td><td>0</td></tr><tr><td colspan="12">EXID[12:0]</td><td>IDE</td><td>RTR</td><td colspan="2">TXRQ</td></tr></table>

<table><tr><td>位</td><td>名称</td><td>访问</td><td>描述</td><td>复位值</td></tr><tr><td>[31:21]</td><td>STID[10:0] /EXIDH[28:18]</td><td>R0</td><td>标准标识符或扩展标识符的高11位。</td><td>x</td></tr><tr><td>[20:3]</td><td>EXIDL[17:0]</td><td>R0</td><td>扩展标识符的低18位。</td><td>x</td></tr><tr><td>2</td><td>IDE</td><td>R0</td><td>标识符选择标志位。1:选用扩展标识符;0:选用标准标识符。</td><td>x</td></tr><tr><td>1</td><td>RTR</td><td>R0</td><td>远程帧(也称遥控帧)选择标志位。1:当前为远程帧;0:当前为数据帧。</td><td>x</td></tr><tr><td>0</td><td>Reserved</td><td>R0</td><td>保留。</td><td>x</td></tr></table>

### 24.7.16 CANx 接收邮箱数据长度和时间戳寄存器（CANx_RXMDTRy）（x=0/1,y=0/1）

偏移地址：0x1B4,0x1C4

<table><tr><td>31</td><td>30</td><td>29</td><td>28</td><td>27</td><td>26</td><td>25</td><td>24</td><td>23</td><td>22</td><td>21</td><td>20</td><td>19</td><td>18</td><td>17</td><td>16</td></tr><tr><td colspan="16">TIME[15:0]</td></tr><tr><td>15</td><td>14</td><td>13</td><td>12</td><td>11</td><td>10</td><td>9</td><td>8</td><td>7</td><td>6</td><td>5</td><td>4</td><td>3</td><td>2</td><td>1</td><td>0</td></tr><tr><td colspan="8">FMI[7:0]</td><td colspan="4">Reserved</td><td colspan="4">DLC[3:0]</td></tr></table>

<table><tr><td>位</td><td>名称</td><td>访问</td><td>描述</td><td>复位值</td></tr><tr><td>[31:16]</td><td>TIME[15:0]</td><td>R0</td><td>用于接收报文 SOF 时刻的 16 位定时器值。</td><td>x</td></tr><tr><td>[15:8]</td><td>FMI[7:0]</td><td>R0</td><td>报文所匹配的过滤器编号。</td><td>x</td></tr><tr><td>[7:4]</td><td>Reserved</td><td>R0</td><td>保留。</td><td>0</td></tr><tr><td>[3:0]</td><td>DLC[3:0]</td><td>R0</td><td>接收报文数据长度。
数据帧长度 0 到 8，远程帧为 0。</td><td>x</td></tr></table>

### 24.7.17 CANx 接收邮箱低字节数据寄存器（CANx_RXMDLRy）（x=0/1,y=0/1）

偏移地址：0x1B8,0x1C8

<table><tr><td>31</td><td>30</td><td>29</td><td>28</td><td>27</td><td>26</td><td>25</td><td>24</td><td>23</td><td>22</td><td>21</td><td>20</td><td>19</td><td>18</td><td>17</td><td>16</td></tr><tr><td colspan="8">DATA3[7:0]</td><td colspan="8">DATA2[7:0]</td></tr><tr><td>15</td><td>14</td><td>13</td><td>12</td><td>11</td><td>10</td><td>9</td><td>8</td><td>7</td><td>6</td><td>5</td><td>4</td><td>3</td><td>2</td><td>1</td><td>0</td></tr><tr><td colspan="8">DATA1[7:0]</td><td colspan="8">DATA0[7:0]</td></tr></table>

<table><tr><td>位</td><td>名称</td><td>访问</td><td>描述</td><td>复位值</td></tr><tr><td>[31:24]</td><td>DATA3[7:0]</td><td>R0</td><td>接收报文的数据字节3。</td><td>x</td></tr><tr><td>[23:16]</td><td>DATA2[7:0]</td><td>R0</td><td>接收报文的数据字节2。</td><td>x</td></tr><tr><td>[15:8]</td><td>DATA1[7:0]</td><td>R0</td><td>接收报文的数据字节1。</td><td>x</td></tr><tr><td>[7:0]</td><td>DATA0[7:0]</td><td>R0</td><td>接收报文的数据字节0。</td><td>x</td></tr></table>

### 24.7.18 CANx 接收邮箱高字节数据寄存器（CANx_RXMDHRy）（x=0/1,y=0/1）

偏移地址：0x1BC,0x1CC

<table><tr><td>31</td><td>30</td><td>29</td><td>28</td><td>27</td><td>26</td><td>25</td><td>24</td><td>23</td><td>22</td><td>21</td><td>20</td><td>19</td><td>18</td><td>17</td><td>16</td></tr><tr><td colspan="8">DATA7[7:0]</td><td colspan="8">DATA6[7:0]</td></tr><tr><td>15</td><td>14</td><td>13</td><td>12</td><td>11</td><td>10</td><td>9</td><td>8</td><td>7</td><td>6</td><td>5</td><td>4</td><td>3</td><td>2</td><td>1</td><td>0</td></tr><tr><td colspan="8">DATA5[7:0]</td><td colspan="8">DATA4[7:0]</td></tr></table>

<table><tr><td>位</td><td>名称</td><td>访问</td><td>描述</td><td>复位值</td></tr><tr><td>[31:24]</td><td>DATA7[7:0]</td><td>R0</td><td>接收报文的数据字节7。</td><td>x</td></tr><tr><td>[23:16]</td><td>DATA6[7:0]</td><td>R0</td><td>接收报文的数据字节6。</td><td>x</td></tr><tr><td>[15:8]</td><td>DATA5[7:0]</td><td>R0</td><td>接收报文的数据字节5。</td><td>x</td></tr><tr><td>[7:0]</td><td>DATA4[7:0]</td><td>R0</td><td>接收报文的数据字节4。</td><td>x</td></tr></table>

### 24.7.19 CANx 过滤器主控制寄存器（CANx_FCTLR）（ $\bf { \chi } _ { x = 0 } / 1 )$

偏移地址： $\mathtt { 0 \times 2 0 0 }$

<table><tr><td>31</td><td>30</td><td>29</td><td>28</td><td>27</td><td>26</td><td>25</td><td>24</td><td>23</td><td>22</td><td>21</td><td>20</td><td>19</td><td>18</td><td>17</td><td>16</td></tr><tr><td colspan="16">Reserved</td></tr><tr><td>15</td><td>14</td><td>13</td><td>12</td><td>11</td><td>10</td><td>9</td><td>8</td><td>7</td><td>6</td><td>5</td><td>4</td><td>3</td><td>2</td><td>1</td><td>0</td></tr><tr><td>Reserved</td><td colspan="7">CAN2SB[5:0]</td><td colspan="7">Reserved</td><td>FINIT</td></tr></table>

<table><tr><td>位</td><td>名称</td><td>访问</td><td>描述</td><td>复位值</td></tr><tr><td>[31:14]</td><td>Reserved</td><td>R0</td><td>保留。</td><td>0</td></tr><tr><td>[13:8]</td><td>CAN2SB[5:0]</td><td>RW</td><td>CAN2过滤器开始组（取值范围1-27）。</td><td>01110b</td></tr><tr><td>[7:1]</td><td>Reserved</td><td>R0</td><td>保留。</td><td>0</td></tr><tr><td>0</td><td>FINIT</td><td>RW</td><td>过滤器初始化模式使能标志位。
1：过滤器组为初始化模式；
0：过滤器组为正常模式。</td><td>1</td></tr></table>

### 24.7.20 CANx 过滤器模式寄存器（CANx_FMCFGR）（ $\mathbf { \chi } _ { \mathbf { x } = 0 / 1 }$ ）

偏移地址： $0 \times 2 0 4$

<table><tr><td>31</td><td>30</td><td>29</td><td>28</td><td>27</td><td>26</td><td>25</td><td>24</td><td>23</td><td>22</td><td>21</td><td>20</td><td>19</td><td>18</td><td>17</td><td>16</td></tr><tr><td colspan="3">Reserved</td><td>FBM27</td><td>FBM26</td><td>FBM25</td><td>FBM24</td><td>FBM23</td><td>FBM22</td><td>FBM21</td><td>FBM20</td><td>FBM19</td><td>FBM18</td><td>FBM17</td><td>FBM16</td><td></td></tr><tr><td>15</td><td>14</td><td>13</td><td>12</td><td>11</td><td>10</td><td>9</td><td>8</td><td>7</td><td>6</td><td>5</td><td>4</td><td>3</td><td>2</td><td>1</td><td>0</td></tr><tr><td>FBM15</td><td>FBM14</td><td>FBM13</td><td>FBM12</td><td>FBM11</td><td>FBM10</td><td>FBM9</td><td>FBM8</td><td>FBM7</td><td>FBM6</td><td>FBM5</td><td>FBM4</td><td>FBM3</td><td>FBM2</td><td>FBM1</td><td>FBM0</td></tr></table>

<table><tr><td>位</td><td>名称</td><td>访问</td><td>描述</td><td>复位值</td></tr><tr><td>[31:28]</td><td>Reserved</td><td>R0</td><td>保留。</td><td>0</td></tr><tr><td>[27:0]</td><td>FBMx</td><td>RW</td><td>过滤器组x的工作模式控制位，FINT为1才能写入。
0：过滤器组x的寄存器为屏蔽位模式；
1：过滤器组x的寄存器为标识符列表模式。</td><td>0</td></tr></table>

### 24.7.21 CANx 过滤器位宽寄存器（CANx_FSCFGR）（ $\bf { \chi } _ { x = 0 / 1 }$ ）

偏移地址： $_ { 0 \times 2 0 0 }$

<table><tr><td>31</td><td>30</td><td>29</td><td>28</td><td>27</td><td>26</td><td>25</td><td>24</td><td>23</td><td>22</td><td>21</td><td>20</td><td>19</td><td>18</td><td>17</td><td>16</td></tr><tr><td colspan="4">Reserved</td><td>FSC27</td><td>FSC26</td><td>FSC25</td><td>FSC24</td><td>FSC23</td><td>FSC22</td><td>FSC21</td><td>FSC20</td><td>FSC19</td><td>FSC18</td><td>FSC17</td><td>FSC16</td></tr><tr><td>15</td><td>14</td><td>13</td><td>12</td><td>11</td><td>10</td><td>9</td><td>8</td><td>7</td><td>6</td><td>5</td><td>4</td><td>3</td><td>2</td><td>1</td><td>0</td></tr><tr><td>FSC15</td><td>FSC14</td><td>FSC13</td><td>FSC12</td><td>FSC11</td><td>FSC10</td><td>FSC9</td><td>FSC8</td><td>FSC7</td><td>FSC6</td><td>FSC5</td><td>FSC4</td><td>FSC3</td><td>FSC2</td><td>FSC1</td><td>FSC0</td></tr></table>

<table><tr><td>位</td><td>名称</td><td>访问</td><td>描述</td><td>复位值</td></tr><tr><td>[31:28]</td><td>Reserved</td><td>R0</td><td>保留。</td><td>0</td></tr><tr><td>[27:0]</td><td>FSCx</td><td>RW</td><td>过滤器组x的位宽控制位，FINT为1才能写入。
1：过滤器组x的寄存器为单个32位；
0：过滤器组x的寄存器为2个16位。</td><td>0</td></tr></table>

### 24.7.22 CANx 过滤器 FIFO 关联寄存器（CANx_FAFIFOR）（x=0/1）

偏移地址： $0 \times 2 1 4$

<table><tr><td>31</td><td>30</td><td>29</td><td>28</td><td>27</td><td>26</td><td>25</td><td>24</td><td>23</td><td>22</td><td>21</td><td>20</td><td>19</td><td>18</td><td>17</td><td>16</td></tr><tr><td colspan="4">Reserved</td><td>FFA27</td><td>FFA26</td><td>FFA25</td><td>FFA24</td><td>FFA23</td><td>FFA22</td><td>FFA21</td><td>FFA20</td><td>FFA19</td><td>FFA18</td><td>FFA17</td><td>FFA16</td></tr><tr><td>15</td><td>14</td><td>13</td><td>12</td><td>11</td><td>10</td><td>9</td><td>8</td><td>7</td><td>6</td><td>5</td><td>4</td><td>3</td><td>2</td><td>1</td><td>0</td></tr><tr><td>FFA15</td><td>FFA14</td><td>FFA13</td><td>FFA12</td><td>FFA11</td><td>FFA10</td><td>FFA9</td><td>FFA8</td><td>FFA7</td><td>FFA6</td><td>FFA5</td><td>FFA4</td><td>FFA3</td><td>FFA2</td><td>FFA1</td><td>FFA0</td></tr></table>

<table><tr><td>位</td><td>名称</td><td>访问</td><td>描述</td><td>复位值</td></tr><tr><td>[31:28]</td><td>Reserved</td><td>R0</td><td>保留。</td><td>0</td></tr><tr><td>[27:0]</td><td>FFAx</td><td>RW</td><td>过滤器组x的关联FIFO控制位,FINT为1才能写入。
1:过滤器组x被关联到FIFO_1;
0:过滤器组x被关联到FIFO_0。</td><td>0</td></tr></table>

### 24.7.23 CANx 过滤器激活寄存器（CANx_FWR）（ $\bf { \chi } _ { x = 0 / 1 }$ ）

偏移地址： $_ { 0 \times 2 1 0 }$

<table><tr><td>31</td><td>30</td><td>29</td><td>28</td><td>27</td><td>26</td><td>25</td><td>24</td><td>23</td><td>22</td><td>21</td><td>20</td><td>19</td><td>18</td><td>17</td><td>16</td></tr><tr><td colspan="4">Reserved</td><td>FACT
27</td><td>FACT
26</td><td>FACT
25</td><td>FACT
24</td><td>FACT
23</td><td>FACT
22</td><td>FACT
21</td><td>FACT
20</td><td>FACT
19</td><td>FACT
18</td><td>FACT
17</td><td>FACT
16</td></tr><tr><td>15</td><td>14</td><td>13</td><td>12</td><td>11</td><td>10</td><td>9</td><td>8</td><td>7</td><td>6</td><td>5</td><td>4</td><td>3</td><td>2</td><td>1</td><td>0</td></tr><tr><td>FACT
15</td><td>FACT
14</td><td>FACT
13</td><td>FACT
12</td><td>FACT
11</td><td>FACT
10</td><td>FACT
9</td><td>FACT
8</td><td>FACT
7</td><td>FACT
6</td><td>FACT
5</td><td>FACT
4</td><td>FACT
3</td><td>FACT
2</td><td>FACT
1</td><td>FACT
0</td></tr></table>

<table><tr><td>位</td><td>名称</td><td>访问</td><td>描述</td><td>复位值</td></tr><tr><td>[31:28]</td><td>Reserved</td><td>R0</td><td>保留。</td><td>0</td></tr><tr><td>[27:0]</td><td>FACTx</td><td>RW</td><td>过滤器组x的激活控制位。
1: 过滤器组x激活;
0: 过滤器组x禁用。</td><td>0</td></tr></table>

### 24.7.24 CANx 过滤器组的过滤寄存器（CANx_FiRy）（x=1/2,i=0-27,y=1/2）

偏移地址： $0 { \times } 2 4 0 { - } 0 { \times } 3 1 0$

<table><tr><td>31</td><td>30</td><td>29</td><td>28</td><td>27</td><td>26</td><td>25</td><td>24</td><td>23</td><td>22</td><td>21</td><td>20</td><td>19</td><td>18</td><td>17</td><td>16</td></tr><tr><td>FB31</td><td>FB30</td><td>FB29</td><td>FB28</td><td>FB27</td><td>FB26</td><td>FB25</td><td>FB24</td><td>FB23</td><td>FB22</td><td>FB21</td><td>FB20</td><td>FB19</td><td>FB18</td><td>FB17</td><td>FB16</td></tr><tr><td>15</td><td>14</td><td>13</td><td>12</td><td>11</td><td>10</td><td>9</td><td>8</td><td>7</td><td>6</td><td>5</td><td>4</td><td>3</td><td>2</td><td>1</td><td>0</td></tr><tr><td>FB15</td><td>FB14</td><td>FB13</td><td>FB12</td><td>FB11</td><td>FB10</td><td>FB9</td><td>FB8</td><td>FB7</td><td>FB6</td><td>FB5</td><td>FB4</td><td>FB3</td><td>FB2</td><td>FB1</td><td>FB0</td></tr></table>

<table><tr><td>位</td><td>名称</td><td>访问</td><td>描述</td><td>复位值</td></tr><tr><td>[31:0]</td><td>FB</td><td>RW</td><td>过滤器组中寄存器的标志位，FINT为1才能写入。标识符模式下：1：相应位期望电平为隐性位；0：相应位期望电平为显性位。屏蔽位模式下：1：必须和对应的标识符寄存器位一致；0：不需要和对应的标识符寄存器位一致。</td><td>0</td></tr></table>


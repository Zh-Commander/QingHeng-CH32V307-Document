# 第 19 章 两线通信总线（I2C）

本章模块描述适用于CH32F2x、CH32V2x和CH32V3x微控制器全系列产品。

内部集成电路总线（I2C）广泛用在微控制器和传感器及其他片外模块的通讯上，它本身支持多主多从模式，仅仅使用两根线（SDA和SCL）就能以100KHz（标准）和400KHz（快速）两种速度通讯。I2C总线还兼容SMBus协议，不仅支持I2C的时序，还支持仲裁、定时和DMA，拥有CRC校验功能。

## 19.1 主要特征

支持主模式和从模式  
$\bullet$ 支持 7位或10位地址  
$\bullet$ 从设备支持双 7 位地址  
$\bullet$ 支持两种速度模式：100KHz 和 400KHz  
$\bullet$ 多种状态模式，多种错误标志  
支持加长的时钟功能  
2个中断向量  
支持 DMA  
$\bullet$ 支持 PEC  
兼容 SMBus

## 19.2 概述

I2C 是半双工的总线，它同时只能运行在下列四种模式中之一：主设备发送模式、主设备接收模式、从设备发送模式和从设备接收模式。I2C模块默认工作在从模式，在产生起始条件后，会自动地切换到主模式，当仲裁丢失或产生停止信号后，会切换到从模式。I2C模块支持多主机功能。工作在主模式时，I2C模块会主动发出数据和地址。数据和地址都以 8 位为单位进行传输，高位在前，低位在后，在起始事件后的是一个字节（7位地址模式下）或两个字节（10位地址模式下）地址，主机每发送8位数据或地址，从机需要回复一个应答 ACK，即把 SDA总线拉低，如图 19-1 所示。

![](images/11f8554ecd6d1b55bbdaf3436af135fb6e848c8e240f9598f68922c63693a9a9.jpg)  
图 19-1 I2C 时序图

为了正常使用必须给I2C输入正确的时钟，其中标准模式下，输入时钟最低为 2MHz,在快速模式下，输入时钟最低为4MHz。

图 19-2 是 I2C 模块功能框图。

![](images/c9418045eada5e5debf450f67126537eae76978bd3c35366f031db4a7b9541a0.jpg)  
图 19-2 I2C 功能框图

## 19.3 主模式

主模式时，I2C模块主导数据传输并输出时钟信号，数据传输以开始事件开始，以结束事件结束。使用主模式通讯的步骤为：

在控制寄存器 2（R16_I2Cx_CTLR2）和时钟控制寄存器（R16_I2Cx_CKCFGR）中设置正确的时钟；

在上升沿寄存器（R16_I2Cx_RTR）设置合适的上升沿；

在控制寄存器（R16_I2Cx_CTLR1）中置 PE 位启动外设；

在控制寄存器（R16_I2Cx_CTLR1）中置 START 位，产生起始事件。

在置 START 位后，I2C 模块会自动切换到主模式，MSL 位会置位，产生起始事件，在产生起始事件后，SB 位会置位，如果 ITEVTEN 位（在 R16_I2Cx_CTLR2）被置位，则会产生中断。此时应该读取状态寄存器 1（R16_I2Cx_STAR1），写从地址到数据寄存器后，SB 位会自动清除；

5）如果是使用 10 位地址模式，那么写数据寄存器发送头序列（头序列为 $1 1 1 1 0 \times \times 0 6$ ，其中的 xx 位是10位地址的最高两位）。

在发送完头序列之后，状态寄存器的ADD10 位会被置位，如果ITEVTEN位已经置位，则会产生中断，此时应读取 R16_I2Cx_STAR1 寄存器后，写第二个地址字节到数据寄存器后，清除 ADD10 位。

然后写数据寄存器发送第二个地址字节，在发送完第二个地址字节后，状态寄存器的 ADDR 位会被置位，如果 ITEVTEN 位已经置位，则会产生中断，此时应读取 R16_I2Cx_STAR1 寄存器后再读一次R16_I2Cx_STAR2 寄存器以清除 ADDR 位；

如果使用的是7位地址模式，那么写数据寄存器发送地址字节，在发送完地址字节后，状态寄存器的 ADDR位会被置位，如果ITEVTEN位已经置位，则会产生中断，此时应读取 R16_I2Cx_STAR1寄存器后再读一次 R16_I2Cx_STAR2 寄存器以清除 ADDR 位；

在7位地址模式下，发送的第一个字节为地址字节，头 7位代表的是目标从设备地址，第 8位决定了后续报文的方向，0代表是主设备写入数据到从设备，1代表是主设备向从设备读取信息。

在10位地址模式下，如图18-3所示，在发送地址阶段，第一个字节为 $1 1 1 1 0 \times \times 0$ ，xx 为 10 位地址的最高 2 位，第二个字节为 10 位地址的低 8 位。若后续进入主设备发送模式，则继续发送数据；若后续准备进入主设备接收模式，则需要重新发送一个起始条件，跟随发送一个字节为 $1 1 1 1 0 \times \times 1$ ，然后进入主设备接收模式。

![](images/fc6a38dbb5c163c5261a812e8062fc8b215c2792d32437883db7d4221a2dfef8.jpg)  
图 19-3 10 位地址时主机收发数据示意图

#### 主发送模式：

主设备内部的移位寄存器将数据从数据寄存器发送到 SDA线上，当主设备接收到 ACK时，状态寄存器 1（R16_I2Cx_STAR1）的 TxE 被置位，如果 ITEVTEN 和 ITBUFEN 被置位，还会产生中断。向数据寄存器写入数据将会清除 TxE 位。

如果 TxE 位被置位且上次发送数据之前没有新的数据被写入数据寄存器，那么 BTF 位会被置位,在其被清除之前，SCL将保持低电平，读R16_I2Cx_STAR1后，向数据寄存器写入数据将会清除BTF位。

![](images/65b7e7d92c850983bba90d6a55659a49ac44ee4808983a549dec1cd600a151f4.jpg)  
图 19-4 主发送器传送序列图

#### 主接收模式：

I2C 模块会从SDA线接收数据，通过移位寄存器写进数据寄存器。在每个字节之后，如果 ACK位被置位，那么 I2C 模块将会发出一个应答低电平，同时 RxNE 位会被置位，如果 ITEVTEN 和 ITBUFEN被置位，还会产生中断。如果RxNE被置位且在新的数据被接收前，原有的数据没有被读出，则BTF位将被置位，在清除BTF之前，SCL将保持低电平，读取 R16_I2Cx_STAR1后，再读取数据寄存器将会清除 BTF 位。

图19-5 接收器传送序列图  
![](images/c4a140b054e570bfc4ad20e1c3b6c571415794e0f5d19127c3e739e596428715.jpg)  
说明：S=Start(起始条件)， $\mathsf { s r } =$ 重复的起始条件，P=Stop（停止条件），A=响应，NA=非响应，EVTx=事件（ITEVFEN=1时产生中断）  
EVT5：SB=1，读SR1然后将地址写入DR寄存器将清除该事件。  
EVT6：ADDR=1,读SR1然后读SR2将消除该事件。在10位主接收模式下，该事件后应设置CR2的START=1。  
EVT6_1：没有对应的事件标志，只适于接收1个字节的情况。恰好在EVT6之后（即清除了ADDR之后），要清除响应和停止条件的产生位。  
EVT7：RxNE=1，读DR寄存器清除该事件。   
EVT7_1：RxNE=1，读DR寄存器清除该事件。设置ACK=0和STOP请求。  
EVT9：ADDR10=1，读SR1然后写入DR寄存器将清除该事件。

主设备在结束发送数据时，会主动发一个结束事件，即置 STOP位，I2C将切换至从模式。在接收模式时，主设备需要在最后一个数据位的应答位置NAK，接收到 NACK后，从设备释放对 SCL和 SDA线的控制；主设备就可以发送一个停止/重起始条件。注意，产生停止条件后，I2C模块将会自动切换至从模式。

## 19.4 从模式

从模式时，I2C模块能识别它自己的地址和广播呼叫地址。软件能控制开启或禁止广播呼叫地址的识别。一旦检测到起始事件，I2C模块将SDA的数据通过移位寄存器与自己的地址（位数取决于ENDUAL和ADDMODE）或广播地址（ENGC置位时）相比较，如果不匹配将会忽略，直到产生新的起始事件；如果与头序列相匹配，则会产生一个 ACK信号并等待第二个字节的地址；如果第二字节的地址也匹配或者7位地址情况下全段地址匹配，那么：

首先产生一个 ACK 应答；

ADDR 位被置位，如果 ITEVTEN 位已经置位，那么还会产生相应的中断；

如果使用的是双地址模式（ENDUAL 位被置位），还需要读取 DUALF 位来判断主机唤起的是哪一个地址。

从模式默认是接收模式，在接收的头序列的最后一位为 1，或者 7位地址最后一位为 1后（取决于第一次接收到头序列还是普通的7位地址），当接收到重复的起始条件时，I2C模块将进入到发送器模式，TRA 位将指示当前是接收器还是发送器模式。

#### 从发送模式：

在清除ADDR位后，I2C模块将字节从数据寄存器通过移位寄存器发送到 SDA线上。从设备保持SCL为低电平，直到ADDR位被清除且待发送数据已写入数据寄存器。（见下图中的 EVT1 和EVT3）。在收到一个应答ACK后，TxE位将被置位，如果设置了 ITEVTEN和 ITBUFEN，还会产生一个中断。如果 TxE 被置位但在下一个数据发送结束前没有新的数据被写入数据寄存器时，BTF 位将被置位。在清除BTF前，SCL将保持低电平，读取状态寄存器 1（R16_I2Cx_STAR1）后，再向数据寄存器写入数据将会清除BTF位。

![](images/fdfca52213a189a3738cdd37e08e7ebfe0717deda9a57dd7fcda7de746ca74ec.jpg)  
图19-6 从发送器的传送序列图

#### 从接收模式：

在ADDR被清除后，I2C模块将SDA上的数据通过移位寄存器存进数据寄存器，在每接收到一个字节后，I2C模块都会置一个ACK位，并置RxNE位。如果设置了 ITEVTEN和ITBUFEN，还会产生一个中断。如果RxNE被置位，且在接收到新的数据前旧的数据没有被读出，那么 BTF会被置位。在清除BTF位之前SCL会保持低电平。读取状态寄存器1（R16_I2Cx_STAR1）并读取数据寄存器里的数据会清除BTF位。

![](images/aa5261d5959e8dceff81374367281aa32fef7ef201144176f370b03413af95bc.jpg)  
图 19-7 从接收器的传送序列图

主设备在传输完最后一个数据字节后，将产生一个停止条件，当 I2C模块检测到停止事件时，将置STOPF位，如果设置了ITEVFEN位，还会产生一个中断。用户需要读取状态寄存器（R16_I2Cx_STAR1）再写控制寄存器（比如复位控制字 SWRST）来清除。（见上图中的 EVT4）。

## 19.5 错误

### 19.5.1 总线错误 BERR

在传输地址或数据期间，I2C模块检测到外部的起始或停止事件时，将产生一个总线错误。产生总线错误时，BERR位被置位，如果设置了ITERREN还会产生一个中断。在从模式下，数据被丢弃，硬件释放总线。如果是起始信号，硬件会认为是重启信号，开始等待地址或停止信号；如果是停止信号，则提前按正常的停止条件操作。在主模式下，硬件不会释放总线，同时不影响当前传输，由用户代码决定是否中止传输。

### 19.5.2 应答错误 AF

当I2C模块检测到一个字节后没有应答时，会产生应答错误。产生应答错误时：AF 会被置位，如果设置了 ITERREN 还会产生一个中断；遇到 AF 错误，如果 I2C 模块工作在从模式，硬件必须释放总线，如果处于主模式，软件必须生成一个停止事件。

### 19.5.3 仲裁丢失 ARLO

当I2C模块检测到仲裁丢失时，产生仲裁丢失错误。产生仲裁丢失错误时：ARLO 位被置位，如果设置了ITERREN还会产生一个中断；I2C模块切换到从模式，并不再响应针对它的从地址发起的传输，除非有主机发起新的起始事件；硬件会释放总线。

### 19.5.4 过载/欠载错误 OVR

#### 过载错误：

在从机模式下，如果禁止时钟延长，I2C模块正在接收数据，如果已经接受到一个字节的数据，但是上一次接收到数据还没有被读出，则会产生过载错误。发生过载错误时，最后收到的字节将被丢弃，发送方应当重发最后一次发送的字节。

#### 欠载错误：

在从模式下，如果禁止时钟延长，I2C 模块正在发送数据，如果在下一个字节的时钟到来之前新的数据还没有被写入到数据寄存器，那么将产生欠载错误。在发生欠载错误时，前一次数据寄存器里的数据将被发送两次，如果发生欠载错误，那么接收方应该丢弃重复收到的数据。为了不产生欠载错误，I2C模块应当在下一个字节的第一个上升沿之前将数据写入数据寄存器。

## 19.6 时钟延长

如果禁止时钟延长，那么就存在发生过载/欠载错误的可能。但如果使能了时钟延长：

$\bullet$ 在发送模式下，如果TxE置位且BTF置位，SCL将一直为低，一直等待用户读取状态寄存器，并向数据寄存器写入待发送的数据；  
$\bullet$ 在接收模式下，如果RxNE置位且BTF置位，那么 SCL在接收到数据后将保持低，直到用户读取状态寄存器，并读取数据寄存器；

由此可见，使能时钟延长可以避免出现过载/欠载错误。

## 19.7 SMBus

SMBus也是一种双线接口，它一般应用在系统和电源管理之间。SMBus和 I2C有很多相似的地方，例如 SMBus 使用和 I2C 一样的 7 位地址模式，以下是 SMBus 和 I2C 的共同点：

1） 主从通信模式，主机提供时钟，支持多主多从；  
2） 两线通讯结构，其中 SMBus 可选一个警示线；

3） 都支持 7 位地址格式。

同时SMBus和I2C也存在区别：

1） I2C 支持的速度最高 $4 0 0 \mathsf { K H z }$ ，而 SMBus 支持的最高是 100KHz，且 SMBus 有最小 10KHz 的速度限制；  
2） SMBus 的时钟为低超过 35mS 时，会报超时，但 I2C 无此限制；  
3） SMBus 有固定的逻辑电平，而 I2C 没有，取决于 VDD；  
4） SMBus 有总线协议，而 I2C 没有。

SMBus还包括设备识别、地址解析协议、唯一的设备标识符、SMBus 提醒和各种总线协议，具体请参考SMBus规范2.0版本。当使用SMBus时，只需要置控制寄存器的 SMBus位，按需配置SMBTYPE 位和 ENAARP 位。

## 19.8 中断

每个I2C模块都有两种中断向量，分别是事件中断和错误中断。两种中断支持图 19-4的中断源。

![](images/cf1582c4fd9094a93c35022b99b472f15e1ca02b0eed17370217ea2e8bdda738.jpg)  
图 19-8 I2C 中断请求

## 19.9 DMA

可以使用DMA来进行批量数据的收发。使用 DMA时不能对控制寄存器的 ITBUFEN 位进行置位。

利用 DMA 发送

通过将 CTLR2 寄存器的 DMAEN 位置位可以激活 DMA 模式。只要 TxE 位被置位，数据将由 DMA 从设定的内存装载进I2C的数据寄存器。需要进行以下设定来为 I2C分配通道。

1） 向 DMA_PADDRx 寄存器设置 I2Cx_DATAR 寄存器地址，DMA_MADDRx 寄存器中设置存储器地址，这样在每个 TxE 事件后，数据将从存储器送至 I2Cx_DATAR 寄存器。  
2） 在 DMA_CNTRx 寄存器中设置所需的传输字节数。在每个 TxE 事件后，此值将被递减。  
3） 利用 DMA_CFGRx 寄存器中的 PL[0:1]位配置通道优先级。  
4） 设置DMA_CFGRx寄存器中的DIR位，并根据应用要求可以配置在整个传输完成一半或全部完成时发出中断请求。  
5） 通过设置 DMA_CFGRx 寄存器上的 EN 位激活通道。

当DMA控制器中设置的数据传输字节数目已经完成时，DMA控制器给 I2C接口发送一个传输结束的 EOT/ EOT_1信号。在中断允许的情况下，将产生一个 DMA中断。

#### 利用DMA接收

置位 CTLR2 寄存器的 DMAEN 后即可进行 DMA 接收模式。使用 DMA 接收时，DMA 将数据寄存器里的数据传送到预设的内存区域。需要以下步骤来为 I2C 分配通道。

1） 向 DMA_PADDRx 寄存器设置 $1 2 0 \times$ _DATAR寄存器地址，DMA_MADDRx寄存器中设置存储器地址，这样在每个 RxNE 事件后，数据将从 $1 2 0 \times$ _DATAR 寄存器写入存储器。  
2） 在 DMA_CNTRx 寄存器中设置所需的传输字节数。在每个 RxNE 事件后，此值将被递减。  
3） 用 DMA_CFGRx 寄存器中的 PL[0:1]配置通道优先级。  
4） 清除 DMA_CFGRx 寄存器中的 DIR 位，根据应用要求可以设置在数据传输完成一半或全部完成时发出中断请求。  
5） 设置 DMA_CFGRx 寄存器中的 EN 位激活该通道。

当DMA控制器中设置的数据传输字节数目已经完成时，DMA控制器给 I2C接口发送一个传输结束的 EOT/EOT_1 信号。在中断允许的情况下，将产生一个 DMA 中断。

## 19.10 包校验错误

包错误校验(PEC)是为了提供传输的可靠性而增加一项 CRC8 校验的步骤，使用以下多项式对每一位串行数据进行计算：

$$
\mathrm {C} = \mathrm {X} ^ {8} + \mathrm {X} ^ {2} + \mathrm {X} + 1
$$

PEC计算是由控制寄存器的ENPEC位激活，对所有信息字节进行计算，包括地址和读写位在内。在发送时，启用PEC会在最后一字节数据之后加上一个字节的 CRC8 计算结果；而在接收模式，在最后一字节被认为是 CRC8 校验结果，如果和内部的计算结果不符合，就会回复一个 NAK，如果是主接收器，无论校验结果正确与否，都会回复一个 NAK。

## 19.11 调试模式

当系统进入调试模式之后，可以通过 DEBUG 模块的 DBG_I2Cx_SMBUS_TIMEOUT 位来决定I2CSMBus 的超时控制是继续工作还是停止。

## 19.12 寄存器描述

表 19-1 I2C1 相关寄存器列表  

<table><tr><td>名称</td><td>访问地址</td><td>描述</td><td>复位值</td></tr><tr><td>R16_I2C1_CTLR1</td><td>0x40005400</td><td>I2C1 控制寄存器 1</td><td>0x0000</td></tr><tr><td>R16_I2C1_CTLR2</td><td>0x40005404</td><td>I2C1 控制寄存器 2</td><td>0x0000</td></tr><tr><td>R16_I2C1_OADDR1</td><td>0x40005408</td><td>I2C1 地址寄存器 1</td><td>0x0000</td></tr><tr><td>R16_I2C1_OADDR2</td><td>0x4000540C</td><td>I2C1 地址寄存器 2</td><td>0x0000</td></tr><tr><td>R16_I2C1_DATAR</td><td>0x40005410</td><td>I2C1 数据寄存器</td><td>0x0000</td></tr><tr><td>R16_I2C1_STAR1</td><td>0x40005414</td><td>I2C1 状态寄存器 1</td><td>0x0000</td></tr><tr><td>R16_I2C1_STAR2</td><td>0x40005418</td><td>I2C1 状态寄存器 2</td><td>0x0000</td></tr><tr><td>R16_I2C1_CKCFGR</td><td>0x4000541C</td><td>I2C1 时钟寄存器</td><td>0x0000</td></tr><tr><td>R16_I2C1_RTR</td><td>0x40005420</td><td>I2C1 上升时间寄存器</td><td>0x0002</td></tr></table>

表 19-2 I2C2 相关寄存器列表  

<table><tr><td>名称</td><td>访问地址</td><td>描述</td><td>复位值</td></tr><tr><td>R16_I2C2_CTLR1</td><td>0x40005800</td><td>I2C2 控制寄存器 1</td><td>0x0000</td></tr><tr><td>R16_I2C2_CTLR2</td><td>0x40005804</td><td>I2C2 控制寄存器 2</td><td>0x0000</td></tr><tr><td>R16_I2C2_OADDR1</td><td>0x40005808</td><td>I2C2 地址寄存器 1</td><td>0x0000</td></tr><tr><td>R16_I2C2_OADDR2</td><td>0x4000580C</td><td>I2C2 地址寄存器 2</td><td>0x0000</td></tr><tr><td>R16_I2C2_DATAR</td><td>0x40005810</td><td>I2C2 数据寄存器</td><td>0x0000</td></tr><tr><td>R16_I2C2_STAR1</td><td>0x40005814</td><td>I2C2 状态寄存器 1</td><td>0x0000</td></tr><tr><td>R16_I2C2_STAR2</td><td>0x40005818</td><td>I2C2 状态寄存器 2</td><td>0x0000</td></tr><tr><td>R16_I2C2_CKCFGR</td><td>0x4000581C</td><td>I2C2 时钟寄存器</td><td>0x0000</td></tr><tr><td>R16_I2C2_RTR</td><td>0x40005820</td><td>I2C2 上升时间寄存器</td><td>0x0002</td></tr></table>

### 19.12.1 I2C 控制寄存器（I2Cx_CTLR1）（x=1/2）

偏移地址： $0 \times 0 0$

<table><tr><td>15</td><td>14</td><td>13</td><td>12</td><td>11</td><td>10</td><td>9</td><td>8</td><td>7</td><td>6</td><td>5</td><td>4</td><td>3</td><td>2</td><td>1</td><td>0</td></tr><tr><td>SWRST</td><td>Reserved</td><td>ALERT</td><td>PEC</td><td>POS</td><td>ACK</td><td>STOP</td><td>START</td><td>NOSTRETCH</td><td>ENGC</td><td>ENPEC</td><td>ENARP</td><td>SMBTYPE</td><td>Reser ved</td><td>SMBUS</td><td>PE</td></tr></table>

<table><tr><td>位</td><td>名称</td><td>访问</td><td>描述</td><td>复位值</td></tr><tr><td>15</td><td>SWRST</td><td>RW</td><td>软件重置,用户代码置此位会使I2C外设重置。在复位前确定I2C总线的引脚被释放,总线处于空闲状态。注:该位可以在总线上没有检测到停止条件但是busy位为1时,重置I2C模块。</td><td>0</td></tr><tr><td>14</td><td>Reserved</td><td>RO</td><td>保留。</td><td>0</td></tr><tr><td>13</td><td>ALERT</td><td>RW</td><td>SMBus提醒位,用户代码可以设置此位或清除此位;当PE置位后,此位可以被硬件清除。1:驱动SMBusALERT引脚使其变低,响应地址头应紧跟在ACK信号后面;0:释放SMBusALERT引脚使其变高,响应地址头应紧跟在NACK信号后面。</td><td>0</td></tr><tr><td>12</td><td>PEC</td><td>RW</td><td>数据包出错检测使能位,置此位启用数据包出错检测。用户代码可以对此位置位或清零;当PEC被传输后,产生开始或结束信号,或PE位清0时,硬件清零该位;1:带PEC;0:不带PEC。注:仲裁丢失时,PEC失效。</td><td>0</td></tr><tr><td>11</td><td>POS</td><td>RW</td><td>ACK和PEC位置设置位,该位可以被用户代码置位或清零,在PE被清零后,可以被硬件清除;1:ACK位控制在移位寄存器里接收的下一个字节的ACK或NAK。PEC移位寄存器里接收的下一字节是PEC;</td><td>0</td></tr><tr><td></td><td></td><td></td><td>0: ACK位控制当前移位寄存器内正在接受的字节的 ACK或NAK。PEC位表明当位前移位寄存器的字节是PEC。注:POS位在2字节数据接收中的用法如下:必须在接收之前配置好。为了NACK第2个字节,必须在清除ADDR位后立刻清除ACK位;为了检测第二个字节的PEC,必须在ADDR事件发生后,配置POS位后设置PEC位。</td><td></td></tr><tr><td>10</td><td>ACK</td><td>RW</td><td>应答使能位,该位可以被用户代码置位或清零,当PE位被置位时,该位可以被硬件清除;1:在接收到一个字节后返回一个应答;0:不设应答。</td><td>0</td></tr><tr><td>9</td><td>STOP</td><td>RW</td><td>停止事件产生位,该位可以被用户代码置位或清零,或当检测到停止事件时,由硬件清除,或检测到超时错误时,由硬件将其置位。主模式下:1:在当前字节传输或当前起始条件发出后产生停止事件;0:无停止事件产生。从模式下:1:在当前字节传输后释放SCL和SDA线;0:无停止事件产生。</td><td>0</td></tr><tr><td>8</td><td>START</td><td>RW</td><td>起始事件产生位,该位可以被用户代码置位或清零,当起始条件发出后或PE被清零时,由硬件清零。主模式下:1:重复产生起始事件;0:无起始事件产生。从模式下:1:当总线空闲时,产生起始事件;0:无起始事件产生。</td><td>0</td></tr><tr><td>7</td><td>NOSTRETCH</td><td>RW</td><td>禁止时钟延长位,此位用于在ADDB或BTF标志被置位的情况下,禁止从模式下的时钟延长,直至被软件清零。1:禁止时钟延长;0:允许时钟延长。</td><td>0</td></tr><tr><td>6</td><td>ENGC</td><td>RW</td><td>广播呼叫使能位,置此位使能广播呼叫,应答广播地址00h。</td><td>0</td></tr><tr><td>5</td><td>ENPEC</td><td>RW</td><td>PEC使能位,置此位开启PEC计算。</td><td>0</td></tr><tr><td>4</td><td>ENARP</td><td>RW</td><td>ARP使能位,置此位使能ARP。如果SMBTYPE=0,则使用SMBus设备的默认地址;如果SMBTYPE=1,则使用SMBus的主地址。</td><td>0</td></tr><tr><td>3</td><td>SMBTYPE</td><td>RW</td><td>SMBus设备类型,置1为SMBus主设备,置0为SMBus从设备。</td><td>0</td></tr><tr><td>2</td><td>Reserved</td><td>RO</td><td>保留。</td><td>0</td></tr><tr><td>1</td><td>SMBUS</td><td>RW</td><td>SMBus模式选择位,置1为使用SMBus模式,</td><td>0</td></tr><tr><td></td><td></td><td></td><td>置0为使用I2C模式。</td><td></td></tr><tr><td>0</td><td>PE</td><td>RW</td><td>I2C外设使能位。
1:启用I2C模块;
0:禁用I2C模块。</td><td>0</td></tr></table>

### 19.12.2 I2C 控制寄存器 2（I2Cx_CTLR2）（x=1/2）

偏移地址： $0 \times 0 4$

<table><tr><td>Reserved</td><td>LAST</td><td>DMAEN</td><td>ITBUFEN</td><td>ITEVTEN</td><td>ITERREN</td><td>Reserved</td><td>FREQ[5:0]</td></tr></table>

<table><tr><td>位</td><td>名称</td><td>访问</td><td>描述</td><td>复位值</td></tr><tr><td>[15:13]</td><td>Reserved</td><td>R0</td><td>保留。</td><td>0</td></tr><tr><td>12</td><td>LAST</td><td>RW</td><td>DMA最后一次传输设置位。1:下一次DMA的EOT是最后的传输;0:下一次DMA的EOT不是最后的传输。注:该位在主接收模式使用,可以在最后一次接收数据时产生一个NAK。</td><td>0</td></tr><tr><td>11</td><td>DMAEN</td><td>RW</td><td>DMA请求使能位,置此位在TxE或RxEN被置位时允许DMA请求。</td><td>0</td></tr><tr><td>10</td><td>ITBUFEN</td><td>RW</td><td>缓冲器中断使能位。1:当TxE或RxEN被置位时,产生事件中断;0:当TxE或RxEN被置位时,不产生中断。</td><td>0</td></tr><tr><td>9</td><td>ITEVTEN</td><td>RW</td><td>时间中断使能位,置此位使能事件中断。在下列条件下,将产生此中断:SB=1(主模式);ADDR=1(主从模式);ADDR10=1(主模式);STOPF=1(从模式);BTF=1,但是没有TxE或RxEN事件;如果ITBUFEN=1,TxE事件为1;如果ITBUFEN=1,RxNE事件为1。</td><td>0</td></tr><tr><td>8</td><td>ITERREN</td><td>RW</td><td>出错中断使能位,置位表示允许出错中断。在下列条件下,将产生该中断;BERR=1;ARLO=1;AF=1;OVR=1;PECERR=1;TIMEOUT=1;SMBAlert=1。</td><td>0</td></tr><tr><td>[7:6]</td><td>Reserved</td><td>RO</td><td>保留。</td><td>0</td></tr><tr><td>[5:0]</td><td>FREQ[5:0]</td><td>RW</td><td>12C模块时钟频率域,必须输入正确的时钟频率以产生正确的时序,允许的范围在2-36MHz之间。必须设置在000010b到100100b之间,单位为MHz。</td><td>0</td></tr></table>

### 19.12.3 I2C 地址寄存器 1（I2Cx_OADDR1）（x=1/2）

偏移地址： $0 \times 0 8$

<table><tr><td>15</td><td>14</td><td>13</td><td>12</td><td>11</td><td>10</td><td>9</td><td>8</td><td>7</td><td>6</td><td>5</td><td>4</td><td>3</td><td>2</td><td>1</td><td>0</td></tr></table>

<table><tr><td>ADD MODE</td><td>MUST1</td><td>Reserved</td><td>ADD[9:8]</td><td>ADD[7:1]</td><td>ADD0</td></tr></table>

<table><tr><td>位</td><td>名称</td><td>访问</td><td>描述</td><td>复位值</td></tr><tr><td>15</td><td>ADDMODE</td><td>RW</td><td>地址模式。
1:10位从机地址（不响应7位地址）;
0:7位从机地址（不响应10位地址）。</td><td>0</td></tr><tr><td>14</td><td>MUST1</td><td>RW1</td><td>必须始终由软件写1。</td><td>0</td></tr><tr><td>[13:10]</td><td>Reserved</td><td>RO</td><td>保留。</td><td>0</td></tr><tr><td>[9:8]</td><td>ADD[9:8]</td><td>RW</td><td>接口地址,在使用10位地址时为第9-8位,在使用7位地址时忽略。</td><td>0</td></tr><tr><td>[7:1]</td><td>ADD[7:1]</td><td>RW</td><td>接口地址,第7-1位。</td><td>0</td></tr><tr><td>0</td><td>ADDO</td><td>RW</td><td>接口地址,使用10位地址时为第0位,在使用7位地址时忽略。</td><td>0</td></tr></table>

### 19.12.4 I2C 地址寄存器 2（I2Cx_OADDR2）（x=1/2）

偏移地址： $0 \times 0 0$

<table><tr><td>15</td><td>14</td><td>13</td><td>12</td><td>11</td><td>10</td><td>9</td><td>8</td><td>7</td><td>6</td><td>5</td><td>4</td><td>3</td><td>2</td><td>1</td><td>0</td></tr><tr><td colspan="8">Reserved</td><td colspan="7">ADD2[7:1]</td><td>ENDUAL</td></tr></table>

<table><tr><td>位</td><td>名称</td><td>访问</td><td>描述</td><td>复位值</td></tr><tr><td>[15:8]</td><td>Reserved</td><td>R0</td><td>保留。</td><td>0</td></tr><tr><td>[7:1]</td><td>ADD2[7:1]</td><td>RW</td><td>接口地址，双地址模式下地址的7-1位。</td><td>0</td></tr><tr><td>0</td><td>ENDUAL</td><td>RW</td><td>双地址模式使能位，置此位可以让ADD2也能被识别。</td><td>0</td></tr></table>

### 19.12.5 I2C 数据寄存器（I2Cx_DATAR）（x=1/2）

偏移地址： $0 \times 1 0$

<table><tr><td>15</td><td>14</td><td>13</td><td>12</td><td>11</td><td>10</td><td>9</td><td>8</td><td>7</td><td>6</td><td>5</td><td>4</td><td>3</td><td>2</td><td>1</td><td>0</td></tr><tr><td colspan="8">Reserved</td><td colspan="8">DR[7:0]</td></tr></table>

<table><tr><td>位</td><td>名称</td><td>访问</td><td>描述</td><td>复位值</td></tr><tr><td>[15:8]</td><td>Reserved</td><td>R0</td><td>保留。</td><td>0</td></tr><tr><td>[7:0]</td><td>DR[7:0]</td><td>RW</td><td>数据寄存器，该域用来存放接收到的数据或存放用于发送到总线的数据。</td><td>0</td></tr></table>

### 19.12.6 I2C 状态寄存器 1（I2Cx_STAR1）（x=1/2）

偏移地址： $0 \times 1 4$

<table><tr><td>15</td><td>14</td><td>13</td><td>12</td><td>11</td><td>10</td><td>9</td><td>8</td><td>7</td><td>6</td><td>5</td><td>4</td><td>3</td><td>2</td><td>1</td><td>0</td></tr><tr><td>SMBAL
ERT</td><td>TIME
OUT</td><td>Reserved</td><td>PECER
R</td><td>0VR</td><td>AF</td><td>ARLO</td><td>BERR</td><td>TxE</td><td>RxNE</td><td>Reser
ved</td><td>STOPF</td><td>ADD10</td><td>BTF</td><td>ADDR</td><td>SB</td></tr></table>

<table><tr><td>位</td><td>名称</td><td>访问</td><td>描述</td><td>复位值</td></tr><tr><td>15</td><td>SMBALERT</td><td>RWO</td><td>SMBus警示位,该位可以由用户写0复位,或在PE变低时由硬件复位。在SMBus主机模式下:1:在引脚上产生了SMBus警示;0:无SMBus警示。在SMBus从机模式下:1:收到SMBAlert响应地址头序列直到SMBAlert变低;0:没有收到SMBAlert响应地址头序列。</td><td>0</td></tr><tr><td>14</td><td>TIMEOUT</td><td>RWO</td><td>超时或Tlow错误标志位,该位可以由用户写0复位,或在PE变低时由硬件复位。1:SCL处于低已达到25mS,或主机低电平累计时钟扩招时间超过10mS,或从设备低电平累计时间超过25mS;0:无超时错误。注:在从模式下此位被置位,从设备会复位通讯,硬件会释放总线;在主模式下此位被置位,硬件会发出停止条件。</td><td>0</td></tr><tr><td>13</td><td>Reserved</td><td>R0</td><td>保留。</td><td>0</td></tr><tr><td>12</td><td>PECERR</td><td>RWO</td><td>在接收时发生PEC错误标志位,该位可以由用户写0复位,或在PE变低时由硬件复位。1:有PEC错误,接收到PEC后,返回NAK;0:无PEC错误。</td><td>0</td></tr><tr><td>11</td><td>OVR</td><td>RWO</td><td>过载、欠载标志位。1:有过载、欠载事件发生:当NOSTRETCH=1时,在接收模式中收到一个新的字节时,数据寄存器里的内容还未被读出,则新接收的字节将丢失;在发送模式时,没有新的数据写入数据寄存器,同样的字节将被发送两次;0:无过载、欠载事件。</td><td>0</td></tr><tr><td>10</td><td>AF</td><td>RWO</td><td>应答失败标志位,该位可以由用户写0复位,或在PE变低时由硬件复位。1:应答错误;0:应答正常。</td><td>0</td></tr><tr><td>9</td><td>ARLO</td><td>RWO</td><td>仲裁丢失标志位,该位可以由用户写0复位,或在PE变低时由硬件复位。1:检测到仲裁丢失,模块失去对总线的控制;0:仲裁正常。</td><td>0</td></tr><tr><td>8</td><td>BERR</td><td>RWO</td><td>总线出错标志位,该位可以由用户写0复位,或在PE变低时由硬件复位。1:起始或停止条件出错;0:正常。</td><td>0</td></tr><tr><td>7</td><td>TxE</td><td>R0</td><td>数据寄存器为空标志位,向数据寄存器写数据可以清除,或产生一个起始或停止位后,或当PE为0后,由硬件自动清除。</td><td>0</td></tr><tr><td></td><td></td><td></td><td>1:发送数据时,发送数据寄存器为空;0:数据寄存器非空。</td><td></td></tr><tr><td>6</td><td>RxNE</td><td>R0</td><td>数据寄存器非空标志位,对数据寄存器的读写操作将清除此位,或当PE为0后,由硬件清除此位。1:接收数据时,数据寄存器不为空;0:正常。</td><td>0</td></tr><tr><td>5</td><td>Reserved</td><td>R0</td><td>保留。</td><td>0</td></tr><tr><td>4</td><td>STOPF</td><td>R0</td><td>停止事件标志位,用户读取状态寄存器1之后,对控制寄存器1的写操作将会清除该位,或当PE为0后,由硬件清除此位。1:在应答之后,从设备在总线上检测到停止事件;0:没有检测到停止事件。</td><td>0</td></tr><tr><td>3</td><td>ADD10</td><td>R0</td><td>10位地址头序列发送标志位,用户读取状态寄存器1之后,对控制寄存器1的写操作将会清除该位,或当PE为0后,由硬件清除此位。1:在10位地址模式下,主设备已经将第一个地址字节发送出去;0:无。</td><td>0</td></tr><tr><td>2</td><td>BTF</td><td>R0</td><td>字节发送结束标志位,用户读取状态寄存器1后,对数据寄存器的读写将清除此位;在传输中,发起一个起始或者停止事件后,或当PE为0后,由硬件清除此位。1:字节发送结束。当NOSTRETCH=0时:发送时,当一个新数据被发送且数据寄存器还未被写入新数据;接收时,当接收一个新的字节但是数据寄存器还未被读取;0:无。</td><td>0</td></tr><tr><td>1</td><td>ADDR</td><td>RWO</td><td>地址被发送/地址匹配标志位,用户读取状态寄存器1后,对状态寄存器2的读操作将会清除此位,或当PE为0时,由硬件清除此位。主模式:1:地址发送结束:在10位地址模式下,当收到地址的第二个字节的ACK后改为被置位;在7位地址模式下,当收到地址的ACK后被置位;0:地址发送没有结束。从模式:1:收到的地址匹配;0:地址不匹配或没有收到地址。</td><td>0</td></tr><tr><td>0</td><td>SB</td><td>R0</td><td>起始位发送标志位,读取状态寄存器1后写数据寄存器的操作将清除该位,或当PE为0时,硬件将会清除此位。1:已发送起始位;0:未发送起始位。</td><td>0</td></tr></table>

### 19.12.7 I2C 状态寄存器 2（I2Cx_STAR2）（x=1/2）

偏移地址： $0 \times 1 8$

<table><tr><td>15</td><td>14</td><td>13</td><td>12</td><td>11</td><td>10</td><td>9</td><td>8</td><td>7</td><td>6</td><td>5</td><td>4</td><td>3</td><td>2</td><td>1</td><td>0</td></tr><tr><td colspan="8">PEC[7:0]</td><td>DUALF</td><td>SMBHO ST</td><td>SMBDE FAULT</td><td>GENCA LL</td><td>Reser ved</td><td>TRA</td><td>BUSY</td><td>MSL</td></tr></table>

<table><tr><td>位</td><td>名称</td><td>访问</td><td>描述</td><td>复位值</td></tr><tr><td>[15:8]</td><td>PEC[7:0]</td><td>R0</td><td>包错误检查域,当PEC使能时(ENPEC置位),此域存放PEC的值。</td><td>0</td></tr><tr><td>7</td><td>DUALF</td><td>R0</td><td>匹配检测标志位,在产生停止位或起始位时,或在PE=0时,硬件会将该位清零。1:接收到的地址与OAR2中的内容相符;0:接收到的地址与OAR1中的内容相符。</td><td>0</td></tr><tr><td>6</td><td>SMBHOST</td><td>R0</td><td>SMBus主机头标志位,在产生停止位或起始位时,或在PE=0时,硬件会将该位清零。1:当SMBTYPE=1且ENARP=1时,收到了SMBus主机地址;0:未接收到SMBus主机地址。</td><td>0</td></tr><tr><td>5</td><td>SMBDEFAULT</td><td>R0</td><td>SMBus设备默认地址标志位,在产生停止位或起始位时,或在PE=0时,硬件会将该位清零。1:当ENARP=1,收到SMBus设备的默认地址;0:未收到地址。</td><td>0</td></tr><tr><td>4</td><td>GENCALL</td><td>R0</td><td>广播呼叫地址标志位,在产生停止位或起始位时,或者在PE=0时,硬件会将该位清零。1:当ENG=1时,收到广播呼叫的地址;0:未收到广播呼叫地址。</td><td>0</td></tr><tr><td>3</td><td>Reserved</td><td>R0</td><td>保留。</td><td>0</td></tr><tr><td>2</td><td>TRA</td><td>R0</td><td>发送/接收标志位,在检测到停止事件(STOPF=1),重复的起始条件、总线仲裁丢失(ARLO=1)或PE=0时,硬件会将其清零。1:数据已发送;0:接收到数据。该位根据地址字节的R/W位来决定。</td><td>0</td></tr><tr><td>1</td><td>BUSY</td><td>R0</td><td>总线忙标志位,该位在检测到一个停止位时会被清零。在接口被禁用时(PE=0),该信息仍被更新。1:总线忙:SDA或SCL存在低电平;0:总线空闲无通讯。</td><td>0</td></tr><tr><td>0</td><td>MSL</td><td>R0</td><td>主从模式指示位,当接口处于主模式时(SB=1),硬件将该位置位;当总线检测到一个停止位,仲裁丢失时,或PE=0时,硬件会清除该位。</td><td>0</td></tr></table>

### 19.12.8 I2C 时钟寄存器（I2Cx_CKCFGR）（x=1/2）

偏移地址： ${ 0 } { \times 1 } { 0 }$

<table><tr><td>15</td><td>14</td><td>13</td><td>12</td><td>11</td><td>10</td><td>9</td><td>8</td><td>7</td><td>6</td><td>5</td><td>4</td><td>3</td><td>2</td><td>1</td><td>0</td></tr><tr><td>F/S</td><td>DUTY</td><td colspan="2">Reserved</td><td colspan="12">CCR[11:0]</td></tr></table>

<table><tr><td>位</td><td>名称</td><td>访问</td><td>描述</td><td>复位值</td></tr><tr><td>15</td><td>F/S</td><td>RW</td><td>主模式选择位。1: 快速模式;0: 标准模式。</td><td>0</td></tr><tr><td>14</td><td>DUTY</td><td>RW</td><td>快速模式时的高电平时间比上低电平时间的占空比。1: 36%; 0: 33.3%。</td><td>0</td></tr><tr><td>[13:12]</td><td>Reserved</td><td>RO</td><td>保留。</td><td>0</td></tr><tr><td>[11:0]</td><td>CCR[11:0]</td><td>RW</td><td>时钟分频系数域,决定SCL时钟的频率波形。</td><td>0</td></tr></table>

### 19.12.9 I2C 上升时间寄存器（I2Cx_RTR）（x=1/2）

偏移地址： $_ { 0 \times 2 0 }$

<table><tr><td>15</td><td>14</td><td>13</td><td>12</td><td>11</td><td>10</td><td>9</td><td>8</td><td>7</td><td>6</td><td>5</td><td>4</td><td>3</td><td>2</td><td>1</td><td>0</td></tr><tr><td colspan="10">Reserved</td><td colspan="6">TRISE[5:0]</td></tr></table>

<table><tr><td>位</td><td>名称</td><td>访问</td><td>描述</td><td>复位值</td></tr><tr><td>[15:6]</td><td>Reserved</td><td>R0</td><td>保留。</td><td>0</td></tr><tr><td>[5:0]</td><td>TRISE[5:0]</td><td>RW</td><td>最大上升时间域。这个位设置主模式的SCL的上升时间。最大的上升沿时间等于TRISE-1个时钟周期。此位只能在PE清零下设置。比如如果I2C模块的输入时钟周期为125nS,而TRISE的值为9,那么最大上升沿时间为(9-1)*125nS,即1000nS。</td><td>000010b</td></tr></table>


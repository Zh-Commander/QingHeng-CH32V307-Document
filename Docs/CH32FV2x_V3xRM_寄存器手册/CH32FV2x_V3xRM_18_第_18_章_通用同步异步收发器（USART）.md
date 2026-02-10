# 第 18 章 通用同步异步收发器（USART）

本章模块描述适用于CH32F2x、 $_ { C H 3 2 V 2 x }$ 和 $_ { C H 3 2 V 3 x }$ 微控制器全系列产品。

该模块包含 3个通用同步异步收发器（USART1/2/3）和 5 个通用异步收发器（UART4/5/6/7/8）。注：对于CH32V20x_D6、CH32F20x_D6，串口4为同步异步收发器（USART4）。

## 18.1 主要特征

全双工或半双工的同步或异步通信  
NRZ数据格式  
$\bullet$ 分数波特率发生器，最高 9Mbps  
$\bullet$ 可编程数据长度  
可配置的停止位  
$\bullet$ 支持 LIN，IrDA 编码器，智能卡  
$\bullet$ 支持 DMA  
多种中断源

## 18.2 概述

![](images/964fd43dd65913dbc79ad33d4326591b3dfc99e25cdb10be3b559260a0c891bc.jpg)  
图18-1 通用同步/异步收发器的结构框图

当 TE（发送使能位）置位时，发送移位寄存器里的数据在 TX 引脚上输出，时钟在 CK 引脚上输

出。在发送时，最先移出的是最低有效位，每个数据帧都由一个低电平的起始位开始，然后发送器根据M（字长）位上的设置发送八位或九位的数据字，最后是数目可配置的停止位。如果配有奇偶检验位，数据字的最后一位为校验位。在 TE 置位后会发送一个空闲帧，空闲帧是 10 位或 11 位高电平，包含停止位。断开帧是10位或11位低电平，后跟着停止位。

## 18.3 波特率发生器

收发器的波特率 $=$ FCLK/(16*USARTDIV)，FCLK 是 APBx 的时钟，即 PCLK1 或 PCLK2， USART1 模块使用 PCLK2，其余的使用 PCLK1。USARTDIV 的值是根据 USART_BRR 中的 DIV_M 和 DIV_F 两个域决定的，具体计算的公式为：

$$
U S A R T D I V = D I V _ {M} + (D I V _ {F} / 1 6)
$$

需要注意的是，波特率产生器产生的比特率不一定能刚好生成用户所需要的波特率，这其中可能是存在偏差。除了尽量取接近的值，减小偏差的方法还可以是增大 APBx 的时钟。比如设定波特率为115200bps 的时，USARTDIV 的值设为 39.0625，在最高频率时可以得到刚好 115200bps 的波特率，但是如果你需要 921600bps 的波特率时，计算的 USARTDIV 是 4.88，但是实际上在 USART_BRR 里填入的值最接近只能是 4.875，实际产生的波特率是 923076bps，误差达到 $0 . 1 6 \%$ 。

发送方发出的串口波形传到接收端时，接收方和发送方的波特率是有一定误差的。误差主要来自三个方面：接收方和发送方实际的波特率不一致；接收方和发送方的时钟有误差；波形在线路中产生的变化。外设模块的接收器是有一定接收容差能力的，当以上三个方面产生的总偏差之和小于模块的容差能力极限时，这个总偏差不影响收发。模块的容差能力极限受是否采用分数波特率和 M位（数据域字长）影响，采用分数波特率和使用9位数据域长度会使容差能力极限降低，但不低于 $3 \%$ 。

## 18.4 同步模式

同步模式使得系统在使用USART模块时可以输出时钟信号。在开启同步模式对外发送数据时，CK引脚会同时对外输出时钟。

开启同步模式的方式是对控制寄存器 2（R16_USARTx_CTLR2）的CLKEN 位置位，但同时需要关闭LIN 模式、智能卡模式、红外模式和半双工模式，即保证 SCEN、HDSEL 和 IREN 位处于复位状态，这三位在控制寄存器 3（R16_USARTx_CTLR3）中。

同步模式使用的要点在于时钟的输出控制。有以下几点需要注意：

USART 模块同步模式只工作在主模式，即 CK 引脚只输出时钟，不接收输入；

只在TX引脚输出数据时输出时钟信号；

LBCL 位决定在发送最后一位数据位时是否输出时钟，CPOL 位决定时钟的极性，CPHA 决定时钟的相位，这三个位在控制寄存器 2（R16_USARTx_CTLR2）中，这三个位需要在 TE 和 RE 未被使能的情况下设置，具体区别见图 18-2。

接收器在同步模式下只会在输出时钟时采样，需要从设备保持一定的信号建立时间和保持时间，具体见图18-3。

![](images/2ae2e60c0e82cd2bddcc03c0330e109a740e60ac35673f13a7e215087e686041.jpg)  
图 18-2 USART 时钟时序示例(M=0)

![](images/6215776a2450eac31a7c83e2ac1274449ee22543fd44e279679a68dbe55b42c9.jpg)  
图18-3 数据采样保持时间

## 18.5 单线半双工模式

半双工模式支持使用单个引脚（只使用TX引脚）来接收和发送，TX引脚和 RX引脚在芯片内部连接。

开启半双工模式的方式是对控制寄存器 3（R16_USARTx_CTLR3）的 HDSEL位置位，但同时需要关闭 LIN 模式、智能卡模式、红外模式和同步模式，即保证 SCEN、CLKEN 和 IREN 位处于复位状态，这三位在控制寄存器 2 和 3（R16_USARTx_CTLR2 和 R16_USARTx_CTLR3）中。

设置成半双工模式之后，需要把 TX 的 IO 口设置成浮空输入或开漏输出高模式。在 TE 置位的情况下，只要将数据写到数据寄存器上，就会发送出去。特别要注意的是，半双工模式可能会出现多设备使用单总线收发时的总线冲突，这需要用户用软件自行避免。

## 18.6 智能卡

智能卡模式支持 ISO7816-3 协议访问智能卡控制器。

开启智能卡模式的方式是对控制寄存器 3（R16_USARTx_CTLR3）的 SCEN位置位，但同时需要关闭LIN 模式、半双工模式和红外模式，即保证 LINEN、HDSEL 和 IREN 位处于复位状态，但是可以开启

CLKEN 来输出时钟，这些位在控制寄存器 2 和 3（R16_USARTx_CTLR2 和 R16_USARTx_CTLR3）中。

为了支持智能卡模式，USART应当被置为8 位数据位外加 1位校验位，它的停止位建议配置成发送和接收都为 1.5 位，智能卡模式是一种单线半双工的协议，它使用 TX 线作为数据通讯，应当被配置为开漏输出加上拉。当接收方接收一帧数据检测到奇偶校验错误时，会在停止位时，发出一个 NACK信号，即在停止位期间主动把 TX 拉低一个周期，发送方检测到 NACK 信号后，会产生帧错误，应用程序据此可以重发。图17-4展示了正确情况下和发生奇偶校验错误情况下的 TX引脚上的波形图。USART的 TC 标志（发送完成标志）可以延迟 GT（保护时间）个时钟产生，接收方也不会将自己置的 NACK 信号认成起始位。

![](images/fc35e90bf2dc8ce2ce8510b5d3fe7aabd4406236d161d7c26347129293ec78f0.jpg)  
图 18-4 (未)发生奇偶校验错误示意图

在智能卡模式下，CK引脚使能后输出的波形和通讯无关，它仅仅是给智能卡提供时钟的，它的值是APB时钟再经过五位可设置的时钟分频（分频值为 PSC的两倍，最高 62分频）。

## 18.7 IrDA

USART模块支持控制IrDA红外收发器进行物理层通信。使用 IrDA必须清除 LINEN、STOP、CLKEN、SCEN和HDSEL位。USART模块和SIR物理层（红外收发器）之间使用NRZ（不归零）编码，最高支持到 115200bps 速率。

IrDA是一个半双工的协议，如果UASRT 正在给SIR物理层发数据，那么IrDA解码器将会忽视新发来的红外信号，如果 USART 正在接受从 SIR 发来的数据，那么 SIR 不会接受来自 USART 的信号。USART 发给 SIR 和 SIR 发给 USART 的电平逻辑是不一样的，SIR 接收逻辑中，高电平为 1，低电平为0，但是在 SIR 发送逻辑中，高电平为 0，低电平为 1。

## 18.8 DMA

USART 模块支持 DMA 功能，可以利用 DMA 实现快速连续收发。当启用 DMA 时，TXE 被置位时，DMA就会从设定的内存空间向发送缓冲区写数据。当使用 DMA接收时，每次 RXNE置位后，DMA就会将接收缓冲区里的数据转移到特定的内存空间。

## 18.9 中断

USART模块支持多种中断源，包括发送数据寄存器空（TXE）、CTS、发送完成（TC）、接收数据就绪（TXNE）、数据溢出（ORE）、线路空闲（IDLE）、奇偶校验出错（PE）、断开标志（LBD）、噪声（NE）、多缓冲通信的溢出（ORE）和帧错误（FE）等等。

表 18-1 中断和对应的使能位的关系  

<table><tr><td>中断源</td><td>使能位</td></tr><tr><td>数据寄存器空（TXE）</td><td>TXEIE</td></tr><tr><td>允许发送（CTS）</td><td>CTSIE</td></tr><tr><td>发送完成（TC）</td><td>TCIE</td></tr><tr><td>接收数据就绪（TXNE）</td><td rowspan="2">TXNEIE</td></tr><tr><td>数据溢出（ORE）</td></tr><tr><td>线路空闲（IDLE）</td><td>IDLEIE</td></tr><tr><td>奇偶校验出错（PE）</td><td>PEIE</td></tr><tr><td>断开标志（LBD）</td><td>LBDIE</td></tr><tr><td>噪声（NE）</td><td rowspan="3">EIE</td></tr><tr><td>多缓冲通信的溢出（ORE）</td></tr><tr><td>多缓冲通信的帧错误（FE）</td></tr></table>

## 18.10 寄存器描述

表 18-2 USART1 相关寄存器列表  

<table><tr><td>名称</td><td>访问地址</td><td>描述</td><td>复位值</td></tr><tr><td>R32_USART1_STAT</td><td>0x40013800</td><td>UASRT1 状态寄存器</td><td>0x000000C0</td></tr><tr><td>R32_USART1_DATAR</td><td>0x40013804</td><td>UASRT1 数据寄存器</td><td>0x000000XX</td></tr><tr><td>R32_USART1_BRR</td><td>0x40013808</td><td>UASRT1 波特率寄存器</td><td>0x00000000</td></tr><tr><td>R32_USART1_CTLR1</td><td>0x4001380C</td><td>UASRT1 控制寄存器 1</td><td>0x00000000</td></tr><tr><td>R32_USART1_CTLR2</td><td>0x40013810</td><td>UASRT1 控制寄存器 2</td><td>0x00000000</td></tr><tr><td>R32_USART1_CTLR3</td><td>0x40013814</td><td>UASRT1 控制寄存器 3</td><td>0x00000000</td></tr><tr><td>R32_USART1_GPR</td><td>0x40013818</td><td>UASRT1 保护时间和预分频寄存器</td><td>0x00000000</td></tr></table>

表 18-3 USART2 相关寄存器列表  

<table><tr><td>名称</td><td>访问地址</td><td>描述</td><td>复位值</td></tr><tr><td>R32_USART2_STAT</td><td>0x40004400</td><td>UASRT2 状态寄存器</td><td>0x000000C0</td></tr><tr><td>R32_USART2_DATAR</td><td>0x40004404</td><td>UASRT2 数据寄存器</td><td>0x000000XX</td></tr><tr><td>R32_USART2_BRR</td><td>0x40004408</td><td>UASRT2 波特率寄存器</td><td>0x00000000</td></tr><tr><td>R32_USART2_CTLR1</td><td>0x4000440C</td><td>UASRT2 控制寄存器 1</td><td>0x00000000</td></tr><tr><td>R32_USART2_CTLR2</td><td>0x40004410</td><td>UASRT2 控制寄存器 2</td><td>0x00000000</td></tr><tr><td>R32_USART2_CTLR3</td><td>0x40004414</td><td>UASRT2 控制寄存器 3</td><td>0x00000000</td></tr><tr><td>R32_USART2_GPR</td><td>0x40004418</td><td>UASRT2 保护时间和预分频寄存器</td><td>0x00000000</td></tr></table>

表 18-4 USART3 相关寄存器列表  

<table><tr><td>名称</td><td>访问地址</td><td>描述</td><td>复位值</td></tr><tr><td>R32_USART3_STAT</td><td>0x40004800</td><td>UASRT3 状态寄存器</td><td>0x000000C0</td></tr><tr><td>R32_USART3_DATAR</td><td>0x40004804</td><td>UASRT3 数据寄存器</td><td>0x000000XX</td></tr><tr><td>R32_USART3_BRR</td><td>0x40004808</td><td>UASRT3 波特率寄存器</td><td>0x00000000</td></tr><tr><td>R32_USART3_CTLR1</td><td>0x4000480C</td><td>UASRT3 控制寄存器 1</td><td>0x00000000</td></tr><tr><td>R32_USART3_CTLR2</td><td>0x40004810</td><td>UASRT3 控制寄存器 2</td><td>0x00000000</td></tr><tr><td>R32_USART3_CTLR3</td><td>0x40004814</td><td>UASRT3 控制寄存器 3</td><td>0x00000000</td></tr><tr><td>R32_USART3_GPR</td><td>0x40004818</td><td>UASRT3 保护时间和预分频寄存器</td><td>0x00000000</td></tr></table>

表 18-5 UART4 相关寄存器列表  

<table><tr><td>名称</td><td>访问地址</td><td>描述</td><td>复位值</td></tr><tr><td>R32_UART4_STAT</td><td>0x40004C00</td><td>UART4 状态寄存器</td><td>0x000000C0</td></tr><tr><td>R32_UART4_DATAR</td><td>0x40004C04</td><td>UART4 数据寄存器</td><td>0x000000XX</td></tr><tr><td>R32_UART4_BRR</td><td>0x40004C08</td><td>UART4 波特率寄存器</td><td>0x00000000</td></tr><tr><td>R32_UART4_CTLR1</td><td>0x40004C0C</td><td>UART4 控制寄存器 1</td><td>0x00000000</td></tr><tr><td>R32_UART4_CTLR2</td><td>0x40004C10</td><td>UART4 控制寄存器 2</td><td>0x00000000</td></tr><tr><td>R32_UART4_CTLR3</td><td>0x40004C14</td><td>UART4 控制寄存器 3</td><td>0x00000000</td></tr><tr><td>R32_UART4_GPR</td><td>0x40004C18</td><td>UART4 保护时间和预分频寄存器</td><td>0x00000000</td></tr></table>

表 18-6 UART5 相关寄存器列表  

<table><tr><td>名称</td><td>访问地址</td><td>描述</td><td>复位值</td></tr><tr><td>R32_UART5_STAT</td><td>0x40005000</td><td>UART5状态寄存器</td><td>0x000000C0</td></tr><tr><td>R32_UART5_DATAR</td><td>0x40005004</td><td>UART5数据寄存器</td><td>0x000000XX</td></tr><tr><td>R32_UART5_BRR</td><td>0x40005008</td><td>UART5波特率寄存器</td><td>0x00000000</td></tr><tr><td>R32_UART5_CTLR1</td><td>0x4000500C</td><td>UART5控制寄存器1</td><td>0x00000000</td></tr><tr><td>R32_UART5_CTLR2</td><td>0x40005010</td><td>UART5控制寄存器2</td><td>0x00000000</td></tr><tr><td>R32_UART5_CTLR3</td><td>0x40005014</td><td>UART5控制寄存器3</td><td>0x00000000</td></tr><tr><td>R32_UART5_GPR</td><td>0x40005018</td><td>UART5保护时间和预分频寄存器</td><td>0x00000000</td></tr></table>

表 18-7 UART6 相关寄存器列表  

<table><tr><td>名称</td><td>访问地址</td><td>描述</td><td>复位值</td></tr><tr><td>R32_UART6_STAT</td><td>0x40001800</td><td>UART6状态寄存器</td><td>0x000000C0</td></tr><tr><td>R32_UART6_DATAR</td><td>0x40001804</td><td>UART6数据寄存器</td><td>0x000000XX</td></tr><tr><td>R32_UART6_BRR</td><td>0x40001808</td><td>UART6波特率寄存器</td><td>0x00000000</td></tr><tr><td>R32_UART6_CTLR1</td><td>0x4000180C</td><td>UART6控制寄存器1</td><td>0x00000000</td></tr><tr><td>R32_UART6_CTLR2</td><td>0x40001810</td><td>UART6控制寄存器2</td><td>0x00000000</td></tr><tr><td>R32_UART6_CTLR3</td><td>0x40001814</td><td>UART6控制寄存器3</td><td>0x00000000</td></tr><tr><td>R32_UART6_GPR</td><td>0x40001818</td><td>UART6保护时间和预分频寄存器</td><td>0x00000000</td></tr></table>

表 18-8 UART7 相关寄存器列表  

<table><tr><td>名称</td><td>访问地址</td><td>描述</td><td>复位值</td></tr><tr><td>R32_UART7_STAT</td><td>0x40001C00</td><td>UART7 状态寄存器</td><td>0x000000C0</td></tr><tr><td>R32_UART7_DATAR</td><td>0x40001C04</td><td>UART7 数据寄存器</td><td>0x000000XX</td></tr><tr><td>R32_UART7_BRR</td><td>0x40001C08</td><td>UART7 波特率寄存器</td><td>0x00000000</td></tr><tr><td>R32_UART7_CTLR1</td><td>0x40001C0C</td><td>UART7 控制寄存器 1</td><td>0x00000000</td></tr><tr><td>R32_UART7_CTLR2</td><td>0x40001C10</td><td>UART7 控制寄存器 2</td><td>0x00000000</td></tr><tr><td>R32_UART7_CTLR3</td><td>0x40001C14</td><td>UART7 控制寄存器 3</td><td>0x00000000</td></tr><tr><td>R32_UART7_GPR</td><td>0x40001C18</td><td>UART7 保护时间和预分频寄存器</td><td>0x00000000</td></tr></table>

表 18-9 UART8 相关寄存器列表  

<table><tr><td>名称</td><td>访问地址</td><td>描述</td><td>复位值</td></tr><tr><td>R32_UART8_STAT</td><td>0x40002000</td><td>UART8状态寄存器</td><td>0x000000C0</td></tr><tr><td>R32_UART8_DATAR</td><td>0x40002004</td><td>UART8数据寄存器</td><td>0x000000XX</td></tr><tr><td>R32_UART8_BRR</td><td>0x40002008</td><td>UART8 波特率寄存器</td><td>0x00000000</td></tr><tr><td>R32_UART8_CTLR1</td><td>0x4000200C</td><td>UART8 控制寄存器 1</td><td>0x00000000</td></tr><tr><td>R32_UART8_CTLR2</td><td>0x40002010</td><td>UART8 控制寄存器 2</td><td>0x00000000</td></tr><tr><td>R32_UART8_CTLR3</td><td>0x40002014</td><td>UART8 控制寄存器 3</td><td>0x00000000</td></tr><tr><td>R32_UART8_GPR</td><td>0x40002018</td><td>UART8 保护时间和预分频寄存器</td><td>0x00000000</td></tr></table>

### 18.10.1 USART 状态寄存器（USARTx_STATR）（x=1/2/3/4/5/6/7/8）

偏移地址： $0 \times 0 0$

<table><tr><td>31</td><td>30</td><td>29</td><td>28</td><td>27</td><td>26</td><td>25</td><td>24</td><td>23</td><td>22</td><td>21</td><td>20</td><td>19</td><td>18</td><td>17</td><td>16</td></tr><tr><td colspan="16">Reserved</td></tr><tr><td>15</td><td>14</td><td>13</td><td>12</td><td>11</td><td>10</td><td>9</td><td>8</td><td>7</td><td>6</td><td>5</td><td>4</td><td>3</td><td>2</td><td>1</td><td>0</td></tr><tr><td colspan="5">Reserved</td><td>CTS</td><td>LBD</td><td>TXE</td><td>TC</td><td>RXNE</td><td>IDLE</td><td>ORE</td><td>NE</td><td>FE</td><td>PE</td><td></td></tr></table>

<table><tr><td>位</td><td>名称</td><td>访问</td><td>描述</td><td>复位值</td></tr><tr><td>[31:10]</td><td>Reserved</td><td>R0</td><td>保留。</td><td>0</td></tr><tr><td>9</td><td>CTS</td><td>RWO</td><td>CTS 状态改变标志。如果设置了 CTSE 位,当nCTS 输出状态改变时,该位将由硬件置高。由软件清零。如果 CTSIE 位已经被置位,则会产生中断。1: nCTS 状态线上存在变化;0: nCTS 状态线上没有变化。</td><td>0</td></tr><tr><td>8</td><td>LBD</td><td>RWO</td><td>LIN 断开检测标志。当检测到 LIN 断开时,该位被硬件置位。由软件清零。如果 LBDIE 已经被置位,则将会产生中断。1: 检测到 LIN 断开;0: 没有检测待 LIN 断开。</td><td>0</td></tr><tr><td>7</td><td>TXE</td><td>R0</td><td>发送数据寄存器空标志。当 TDR 寄存器中的的数据被硬件转移到移位寄存器的时候,该位被硬件置位。如果 TXEIE 已经被置位时,就会产生中断,对数据寄存器进行写操作,此位将会被复位。1: 数据已经被转移到移位寄存器;0: 数据还没被转移到移位寄存器。</td><td>1</td></tr><tr><td>6</td><td>TC</td><td>RWO</td><td>发送完成标志。当含有数据的一帧发送完成后,并且 TXE 被置位,则硬件将会此位置位,如果 TCIE 被置位,还会产生对应中断,软件读了此位再写数据寄存器则会清除此位。也可以直接写 0 来清除此位。1: 发送完成;0: 发送还未完成。</td><td>1</td></tr><tr><td>5</td><td>RXNE</td><td>RWO</td><td>读数据寄存器非空标志,当移位寄存器中的数据被转移到数据寄存器中,该位会被硬件置位。如果 RXNEIE 已经被置位,则还会产生对应的中断。对数据寄存器的读操作可以将该位清</td><td>0</td></tr><tr><td></td><td></td><td></td><td>除。也可以直接写0来清除该位。1:数据收到,能够读出;0:数据还没收到。</td><td></td></tr><tr><td>4</td><td>IDLE</td><td>R0</td><td>总线空闲标志。当总线空闲时,该位将会被硬件置位。如果IDLEIE已经被置位,则会产生对应的中断。读状态寄存器再读数据寄存器的操作会清除此位。1:总线正空闲;0:没有检测到总线空闲。注:此位不会被再次置位直到RXNE被置位。</td><td>0</td></tr><tr><td>3</td><td>ORE</td><td>R0</td><td>过载错误标志。当接收移位寄存器存在数据需要转到数据寄存器时,但是数据寄存器的接收域还有数据未读出时,此位将会被置位。如果RXNEIE被置位了,还会产生对应中断。1:发生过载错误;0:没有过载错误。注:发生过载错误时,数据寄存器的值不会丢失,但是移位寄存器的值会被覆盖。如果设置可EIE位,在多缓冲区通讯模式下,ORE标志位置位会产生中断。</td><td>0</td></tr><tr><td>2</td><td>NE</td><td>R0</td><td>噪声错误标志。当检测到噪声错误标志时,由硬件置位。读状态寄存器后,再读数据寄存器的操作会复位此位。1:检测到噪声;0:没有检测到噪声。注:该位不会产生中断。如果设置了EIE位,在多缓冲区通讯模式下,FE标志位置位会产生中断。</td><td>0</td></tr><tr><td>1</td><td>FE</td><td>R0</td><td>帧错误标志。当检测到同步错误,过多的噪声或者断开符,该位将会被硬件置位。读此位再读数据寄存器的操作会复位此位。1:检测到帧错误;0:没有检测到帧错误。注:该位不会产生中断,如果设置了EIE位,在多缓冲区通讯模式下,FE标志位置位会产生中断。</td><td>0</td></tr><tr><td>0</td><td>PE</td><td>R0</td><td>校验错误标志。在接收模式下,如果产生奇偶检验错误,硬件置位此位。读此位再读数据寄存器的操作会复位此位。在清除此位前,软件必须等RXNE标志位被置位。如果PEIE之前已经被置位,那么此位被置位会产生对应的中断。1:出现奇偶校验错误;0:没有检验错误。</td><td>0</td></tr></table>

### 18.10.2 USART 数据寄存器（USARTx_DATAR）（x=1/2/3/4/5/6/7/8）

偏移地址： $0 \times 0 4$

<table><tr><td>31</td><td>30</td><td>29</td><td>28</td><td>27</td><td>26</td><td>25</td><td>24</td><td>23</td><td>22</td><td>21</td><td>20</td><td>19</td><td>18</td><td>17</td><td>16</td></tr><tr><td colspan="16">Reserved</td></tr><tr><td>15</td><td>14</td><td>13</td><td>12</td><td>11</td><td>10</td><td>9</td><td>8</td><td>7</td><td>6</td><td>5</td><td>4</td><td>3</td><td>2</td><td>1</td><td>0</td></tr><tr><td colspan="7">Reserved</td><td colspan="9">DR[8:0]</td></tr></table>

<table><tr><td>位</td><td>名称</td><td>访问</td><td>描述</td><td>复位值</td></tr><tr><td>[31:9]</td><td>Reserved</td><td>R0</td><td>保留。</td><td>0</td></tr><tr><td>[8:0]</td><td>DR[8:0]</td><td>RW</td><td>数据寄存器。这个寄存器实际上是接收数据寄存器（RDR）和发送寄存器（TDR）两个寄存器组成，DR的读写操作起始分别是读接收寄存器（RDR）和写发送寄存器（TDR）。</td><td>X</td></tr></table>

### 18.10.3 USART 波特率寄存器（USARTx_BRR）（x=1/2/3/4/5/6/7/8）

偏移地址： $0 \times 0 8$

<table><tr><td>31</td><td>30</td><td>29</td><td>28</td><td>27</td><td>26</td><td>25</td><td>24</td><td>23</td><td>22</td><td>21</td><td>20</td><td>19</td><td>18</td><td>17</td><td>16</td></tr><tr><td colspan="16">Reserved</td></tr><tr><td>15</td><td>14</td><td>13</td><td>12</td><td>11</td><td>10</td><td>9</td><td>8</td><td>7</td><td>6</td><td>5</td><td>4</td><td>3</td><td>2</td><td>1</td><td>0</td></tr><tr><td colspan="12">DIV_Mantissa[11:0]</td><td colspan="4">DIV_Fraction[3:0]</td></tr></table>

<table><tr><td>位</td><td>名称</td><td>访问</td><td>描述</td><td>复位值</td></tr><tr><td>[31:16]</td><td>Reserved</td><td>R0</td><td>保留。</td><td>0</td></tr><tr><td>[15:4]</td><td>DIV_Mantissa[11:0]</td><td>RW</td><td>这12位定义了分频器除法因子的整数部分。</td><td>0</td></tr><tr><td>[3:0]</td><td>DIV_Fraction[3:0]</td><td>RW</td><td>这4位定义了分频器除法因子的小数部分。</td><td>0</td></tr></table>

### 18.10.4 USART 控制寄存器 1（USARTx_CTLR1）（x=1/2/3/4/5/6/7/8）

偏移地址： $0 \times 0 0$

<table><tr><td colspan="16">Reserved</td></tr><tr><td>15</td><td>14</td><td>13</td><td>12</td><td>11</td><td>10</td><td>9</td><td>8</td><td>7</td><td>6</td><td>5</td><td>4</td><td>3</td><td>2</td><td>1</td><td>0</td></tr><tr><td>Reserved</td><td>UE</td><td>M</td><td>WAKE</td><td>PCE</td><td>PS</td><td>PEIE</td><td>TXEIE</td><td>TCIE</td><td>RXNEIE</td><td>IDLEIE</td><td>TE</td><td>RE</td><td>RWU</td><td>SBK</td><td></td></tr></table>

<table><tr><td>位</td><td>名称</td><td>访问</td><td>描述</td><td>复位值</td></tr><tr><td>[31:14]</td><td>Reserved</td><td>R0</td><td>保留。</td><td>0</td></tr><tr><td>13</td><td>UE</td><td>RW</td><td>USART使能位。当此位被置位后,在当前字节传输完成后,USART的分频器和输出都会停止工作。</td><td>0</td></tr><tr><td>12</td><td>M</td><td>RW</td><td>字长位。1:9个数据位; 0:8个数据位。</td><td>0</td></tr><tr><td>11</td><td>WAKE</td><td>RW</td><td>唤醒位。此位决定了把USART唤醒的方法:1:地址标记; 0:总线空闲。</td><td>0</td></tr><tr><td>10</td><td>PCE</td><td>RW</td><td>校验位使能。对于接收方,就是进行对数据的奇偶校验;对于发送方,就是插入校验位。一旦设置了此位,只有当前字节传输完成后,校验位使能才生效。</td><td>0</td></tr><tr><td>9</td><td>PS</td><td>RW</td><td>奇偶校验选择。0表示偶校验,1表示奇校验。设置了该位后,只有当前字节传输完成后,校验位使能才生效。</td><td>0</td></tr><tr><td>8</td><td>PEIE</td><td>RW</td><td>奇偶检验中断使能位。对此位置位表示允许产生奇偶检验错误中断。</td><td>0</td></tr><tr><td>7</td><td>TXEIE</td><td>RW</td><td>发送缓冲区空中断使能。对此位置位表示允许产生发送缓冲区空中断。</td><td>0</td></tr><tr><td>6</td><td>TCIE</td><td>RW</td><td>发送完成中断使能。对此位置位表示允许产生发送完成中断。</td><td>0</td></tr><tr><td>5</td><td>RXNEIE</td><td>RW</td><td>接收缓冲区非空中断使能。对此位置位表示允许产生接收缓冲区非空中断。</td><td>0</td></tr><tr><td>4</td><td>IDLEIE</td><td>RW</td><td>总线空闲中断使能。对此位置位表示允许产生总线空闲中断。</td><td>0</td></tr><tr><td>3</td><td>TE</td><td>RW</td><td>发送使能。置此位会使能发送器。</td><td>0</td></tr><tr><td>2</td><td>RE</td><td>RW</td><td>接收使能。置此位会使能接收器,接收器开始检测RX引脚上的起始位。</td><td>0</td></tr><tr><td>1</td><td>RWU</td><td>RW</td><td>接收唤醒。该位决定是否把USART置于静默模式:1:接收器处于静默模式;0:接收器处于正常工作模式。注1:置RWU位之前,USART需要先接收一个数据字节,否则在静默模式下,不能被总线空闲唤醒;注2:当配置成地址标记唤醒时,在RXNE被置位时,不能用软件修改RWU位。</td><td>0</td></tr><tr><td>0</td><td>SBK</td><td>RW</td><td>发送帧断开字符控制位。置此位来发送一个帧断开字符。在断开帧的停止位时,由硬件复位。1:发送; 0:不发送。</td><td>0</td></tr></table>

### 18.10.5 USART 控制寄存器 2（USARTx_CTLR2）（x=1/2/3/4/5/6/7/8）

偏移地址： $0 \times 1 0$

<table><tr><td>31</td><td>30</td><td>29</td><td>28</td><td>27</td><td>26</td><td>25</td><td>24</td><td>23</td><td>22</td><td>21</td><td>20</td><td>19</td><td>18</td><td>17</td><td>16</td></tr><tr><td colspan="16">Reserved</td></tr><tr><td>15</td><td>14</td><td>13</td><td>12</td><td>11</td><td>10</td><td>9</td><td>8</td><td>7</td><td>6</td><td>5</td><td>4</td><td>3</td><td>2</td><td>1</td><td>0</td></tr><tr><td>Reserved</td><td>LINEN</td><td>STOP</td><td>CLKEN</td><td>CPOL</td><td>CPHA</td><td>LBCL</td><td>Reser ved</td><td>LBDIE</td><td>LBDL</td><td>Reser ved</td><td></td><td colspan="4">ADD[3:0]</td></tr></table>

<table><tr><td>位</td><td>名称</td><td>访问</td><td>描述</td><td>复位值</td></tr><tr><td>[31:15]</td><td>Reserved</td><td>R0</td><td>保留。</td><td>0</td></tr><tr><td>14</td><td>LINEN</td><td>RW</td><td>LIN模式使能位,置位则使能LIN模式。在LIN模式下,可以使用SBK位发送LIN同步断开符号,以及检测LIN同步断开符。</td><td>0</td></tr><tr><td>[13:12]</td><td>STOP</td><td>RW</td><td>停止位设置域。这两位来设置停止位。00:1个停止位;01:0.5个停止位;10:2个停止位;11:1.5个停止位。</td><td>00b</td></tr><tr><td>11</td><td>CLKEN</td><td>RW</td><td>时钟使能,使能CK引脚。1:使能;0:禁止。</td><td>0</td></tr><tr><td>10</td><td>CPOL</td><td>RW</td><td>时钟极性设置位。在同步模式下,可以用该位选择SLCK引脚上时钟输出的极性,和CPHA一起配合来产生需要的时钟/数据的采样关系。1:总线空闲时CK引脚上保持高电平;0:总线空闲时CK引脚上保持低电平。注:使能发送后此位不可被修改。</td><td>0</td></tr><tr><td>9</td><td>CPHA</td><td>RW</td><td>时钟相位设置位。在同步模式下,可以用该位选择SLCK引脚上的时钟输出的相位,和CPOL位一起配合来产生需要的时钟/数据的采样关系。1:在时钟的第二个边沿进行数据捕获;0:在时钟的第一个边沿进行数据捕获。注:使能发送后此位不可被修改。</td><td>0</td></tr><tr><td>8</td><td>LBCL</td><td>RW</td><td>最后一个时钟脉冲控制位。在同步模式下,使用该位来控制是否在CK引脚上输出最后发送的那个数据字节对应的时钟脉冲;1:最后一位数据的时钟脉冲不从CK输出;0:最后一位数据的时钟脉冲会从CK输出。注:使能发送后此位不可被修改。</td><td>0</td></tr><tr><td>7</td><td>Reserved</td><td>RW</td><td>保留。</td><td>0</td></tr><tr><td>6</td><td>LBDIE</td><td>RW</td><td>LIN断开符检测中断使能,该位置位会使能LBD引起的中断;</td><td>0</td></tr><tr><td>5</td><td>LBDL</td><td>RW</td><td>LIN断开符检测长度,该位用来选择是11位还是10位的断开符检测。1:11位的断开符检测;0:10位的断开符检测。</td><td>0</td></tr><tr><td>4</td><td>Reserved</td><td>RW</td><td>保留。</td><td>0</td></tr><tr><td>[3:0]</td><td>ADD[3:0]</td><td>RW</td><td>地址域,用来设置本设备的USART节点地址。在多处理器通讯下的静默模式中使用的,使用地址标记来唤醒某个USART设备。</td><td>0</td></tr></table>

### 18.10.6 USART 控制寄存器 3（USARTx_CTLR3）（x=1/2/3/4/5/6/7/8）

偏移地址： $0 \times 1 4$

<table><tr><td>31</td><td>30</td><td>29</td><td>28</td><td>27</td><td>26</td><td>25</td><td>24</td><td>23</td><td>22</td><td>21</td><td>20</td><td>19</td><td>18</td><td>17</td><td>16</td></tr><tr><td colspan="16">Reserved</td></tr><tr><td>15</td><td>14</td><td>13</td><td>12</td><td>11</td><td>10</td><td>9</td><td>8</td><td>7</td><td>6</td><td>5</td><td>4</td><td>3</td><td>2</td><td>1</td><td>0</td></tr><tr><td colspan="4">Reserved</td><td>CTSIE</td><td>CTSE</td><td>RTSE</td><td>DMAT</td><td>DMAR</td><td>SCEN</td><td>NACK</td><td>HDSEL</td><td>IRLP</td><td>IREN</td><td>EIE</td><td></td></tr></table>

<table><tr><td>位</td><td>名称</td><td>访问</td><td>描述</td><td>复位值</td></tr><tr><td>[31:11]</td><td>Reserved</td><td>RO</td><td>保留。</td><td>0</td></tr><tr><td>10</td><td>CTSIE</td><td>RW</td><td>CTSIE中断使能位,置此位时在CTS被置位时会产生中断。</td><td>0</td></tr><tr><td>9</td><td>CTSE</td><td>RW</td><td>CTS使能位,置此位会使能CTS流控。</td><td>0</td></tr><tr><td>8</td><td>RTSE</td><td>RW</td><td>RTS使能位,置此位会使能RTS流控。</td><td>0</td></tr><tr><td>7</td><td>DMAT</td><td>RW</td><td>DMA发送使能位。此位置1在发送时使用DMA。</td><td>0</td></tr><tr><td>6</td><td>DMAR</td><td>RW</td><td>DMA接收使能位。此位置1在接收时使用DMA。</td><td>0</td></tr><tr><td>5</td><td>SCEN</td><td>RW</td><td>智能卡模式使能位,置1使能智能卡模式。</td><td>0</td></tr><tr><td>4</td><td>NACK</td><td>RW</td><td>智能卡NACK使能位,置此位在校验错误出现时,发送NACK。</td><td>0</td></tr><tr><td>3</td><td>HDSEL</td><td>RW</td><td>半双工模式选择位,置此位选择半双工模式。</td><td>0</td></tr><tr><td>2</td><td>IRLP</td><td>RW</td><td>红外低功耗选择位,置此位在选择红外线时,启用低功耗模式。</td><td>0</td></tr><tr><td>1</td><td>IREN</td><td>RW</td><td>红外线使能位,置此位使能红外模式。</td><td>0</td></tr><tr><td>0</td><td>EIE</td><td>RW</td><td>错误使能中断位,置此位后,在DMAR被置位的前提下,如果FE、ORE或NE被置位,就会产生中断。</td><td>0</td></tr></table>

### 18.10.7 USART 保护时间和预分频寄存器（USARTx_GPR）（x=1/2/3/4/5/6/7/8）

偏移地址： $0 \times 1 8$

<table><tr><td>31</td><td>30</td><td>29</td><td>28</td><td>27</td><td>26</td><td>25</td><td>24</td><td>23</td><td>22</td><td>21</td><td>20</td><td>19</td><td>18</td><td>17</td><td>16</td></tr><tr><td colspan="16">Reserved</td></tr><tr><td>15</td><td>14</td><td>13</td><td>12</td><td>11</td><td>10</td><td>9</td><td>8</td><td>7</td><td>6</td><td>5</td><td>4</td><td>3</td><td>2</td><td>1</td><td>0</td></tr><tr><td colspan="8">GT[7:0]</td><td colspan="8">PSC[7:0]</td></tr></table>

<table><tr><td>位</td><td>名称</td><td>访问</td><td>描述</td><td>复位值</td></tr><tr><td>[31:16]</td><td>Reserved</td><td>RO</td><td>保留。</td><td>0</td></tr><tr><td>[15:8]</td><td>GT[7:0]</td><td>RW</td><td>保护时间值域。该域规定了以波特率时钟为单位的保护时间。在智能卡模式下,当保护时间过去后,才会设置发送完成标志。</td><td>0</td></tr><tr><td>[7:0]</td><td>PSC[7:0]</td><td>RW</td><td>预分频器值域。在红外低功耗模式下,源时钟被该值(全部8位有效)分频,值为0时表示保留;在红外正常模式下,此位只能被设置为1;在智能卡模式下,源时钟被该值(低5位有效)</td><td>0</td></tr><tr><td></td><td></td><td></td><td>的两倍分频,来给智能卡提供时钟,值为0表示保留。</td><td></td></tr></table>


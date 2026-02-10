# 第 28 章 SDIO 接口（SDIO）

本章模块描述适用于CH32F2x、CH32V2x和CH32V3x微控制器全系列产品。

本章单独所述“SDIO”，是指微控制器上一个为操作 SD 卡等外部存储卡或其他设备而设计的通信接口，是微控制器的一个外设。微控制器的 SDIO 直接挂载在 AHB 总线上，由 HCLK 直接提供时钟，能实现较高的通讯速度，微控制器的 SDIO 用作 SDIO 主机，被控制的设备也被统称 SDIO 设备。应用中一般使用 SDIO 来读写 SD 卡、TF 卡或 eMMC 颗粒，或控制其他使用 SDIO 作为通讯接口的设备，比如 WiFi/4G 模块。

## 28.1 主要特征

### 28.1.1 特征

支持 SD 卡、SDIO 卡和 MMC 卡  
支持 1 位、4 位和 8 位总线  
SDIO 的时钟最快可达到 HCLK 的一半  
兼容 MMC 规范 4.5   
$\bullet$ 兼容 SD 卡规范 2.0，SDIO 卡规范 2.0  
不兼容 SPI 或 QSPI

### 28.1.2 概述

微控制器的 SDIO 支持与 SD 卡或 MMC 卡等存储器通讯，需要明确的是，SDIO 仅仅是提供一组实现SD卡、MMC卡规范单次命令传输所需要的时钟，数据和命令控制时序，各命令间的先后组合需用用户通过程序自行确定。此外，对于各种存储卡而言，SDIO 仅仅只能实现读写功能，文件系统所提供的对文件的功能需要用户自行通过程序构建文件系统而实现。

SDIO 不同于 QSPI 接口，其没有片选引脚，并多出一个 CMD 引脚，CMD 可以认为是一个特殊数据线，专门用来传输命令和响应；SDIO有1 位、4 位和 8 位三种数据线宽度可选；SDIO的时钟在配置时一般工作在 400KHz 以下的频率，当正式进行数据传输时，则可以配成 SDIO 设备所支持的最大时钟，微控制器所支持的最大 SDIO 时钟输出为 HCLK 的一半，按照协议当 SDIO 设备接收的时钟大于某个阈值时，需要降低时钟线、数据线和命令线的波形峰值，以节省波形上升和下降的耗时。

与 SD 卡不同，SDIO 卡经常指代使用 SDIO 接口的 WIFI/蓝牙模块和 4G 模块等非存储设备。如果没有特殊说明，本章叙述的内容一定适用于 SD 卡，只适用于 SDIO 卡或 MMC 卡的内容将会特别指出。本章在叙述时，优先将SD卡视为潜在操作对象，其次为 SDIO卡，最后为 MMC卡。

## 28.2 接口和时钟

SDIO 通过 AHB 总线接收 CPU 对其的控制，SDIO 的寄存器中有一个 FIFO 接口，CPU 或 DMA 通过读写 FIFO 获取或发生数据。SDIO 由 HCLK 直接供给时钟，并拥有一个中断入口，支持多种中断源。由SDIO 直接控制的引脚有 SDIO_CK、SDIO_CMD、SDIO_D[7:0]这十个，通过几个引脚连接到 SDIO 设备上。SDIO 是一个主机主导的通讯接口，所有的传输必须由微控制器发起。

### 28.2.1 外设结构

SDIO 的结构如图 28-1 所示。

![](images/54f3df443c9297c6206a78c14423836ee8654842fc284edbbc66800771c76041.jpg)  
图 28-1 SDIO 的结构图

SDIO 是微控制器作为主机直接操作 SDIO 设备的外设，其大概由 AHB 接口、时钟控制部分、CMD线控制部分，数据线控制部分和控制寄存器部分这五个模块组成，SDIO是一个半双工的外设，CPU 向控制寄存器写入需要发送的命令和数据，命令线和数据线控制模块负责将命令或数据推到 IO 上，并加上 CRC。SDIO 的数据流由通用 DMA 负责完成从 RAM 到 SDIO 的 FIFO 的搬移，SDIO 的 FIFO 有 32 个32位的大小。

#### 28.2.2 引脚及其配置

SDIO 需要配置的引脚及其模式见表 28-1。

表 28-1 SDIO 的引脚配置  

<table><tr><td>GPIO</td><td>SDIO复用功能</td><td>需要配置成的引脚模式</td><td>需要配置成的引脚速度</td></tr><tr><td>PC[8:11]</td><td>SDIO_D[0:3]</td><td>推挽复用输出</td><td>50MHz</td></tr><tr><td>PC12</td><td>SDIO_CK</td><td>推挽复用输出</td><td>50MHz</td></tr><tr><td>PD2</td><td>SDIO_CMD</td><td>推挽复用输出</td><td>50MHz</td></tr><tr><td>PB8</td><td>SDIO_D4</td><td>推挽复用输出</td><td>50MHz</td></tr><tr><td>PB9</td><td>SDIO_D5</td><td>推挽复用输出</td><td>50MHz</td></tr><tr><td>PC6</td><td>SDIO_D6</td><td>推挽复用输出</td><td>50MHz</td></tr><tr><td>PC7</td><td>SDIO_D7</td><td>推挽复用输出</td><td>50MHz</td></tr></table>

#### 28.2.3 时钟

SDIO 的时钟引脚为 SDIO_CK，其输出时钟由 HCLK 分频得到，分频系数可配置为 2-261 之间的任意整数值。SDIO设备在初始化的时候一般只支持最高 $4 0 0 \mathsf { K H z }$ 时钟的单总线模式，在初始化后，主机一般会发起切换至低电压的操作，同时将时钟提升至微控制器和 SDIO 设备双方能接受的最大时钟。不同版本和速度等级的SD卡所支持时钟速度和切换流程有所区别，用户需要自行了解。

### 28.2.4 命令状态机

SDIO的命令工作流程遵循如下图的状态机。

![](images/93471f3dd35c4829759e2e137837d9917f527cc92ea77291250559ee45c0e8c7.jpg)  
图 28-2 命令状态机

### 28.2.5 数据状态机

SDIO 的数据工作流程遵循如下图的状态机。

![](images/95ec72b0a77f4bacaaa405acf2505fe121c47fc008e43c809161db9b5035eccd.jpg)  
图 28-3 数据状态机

## 28.3 SDIO 协议简述

SDIO 上的通讯以传输为最小单位，每个传输总是以主机在 CMD 线上发送命令为起始，有的命令发送之后，SDIO设备同样会在CMD线上发送一段数据回复主机，称为“响应”，有时还会伴随着数据

的传输，数据的传输是在D线上。命令和响应的格式是固定的，各域各位的定义根据不同的命令或响应而确定。响应和数据传输需要在命令或响应结束后规定的时间内发出或停止，否则将产生超时错误。

本小节的目的是以较小的篇幅让用户对使用 SDIO 所必要的一些规范的细节有初步的了解，并不保证详尽和更新及时。微控制器的SDIO也仅仅保证其实现了 SD 2.0，SDIO 2.0及 MMC 4.5规范的硬件操作基础，对于更高版本规范定义的功能，例如双边沿采样等，并不一定支持。用户在做具体开发时应以SD规范，SDIO规范和MMC规范为依据进行SDIO交互编程。

### 28.3.1 总线时序

SD 卡的传输都是以主机发起 CMD 发起的，SD 卡可能不回复响应，可能回复短响应和长响应，有的响应后面还会伴随数据传输。而在SDIO卡或MMC卡的通讯中，还可能会有 SDIO 设备主动报中断的情况。时序如下图组所示。

![](images/c6496e37d3e7ce5b0aef2e8d3d0dac4104860c3c0938f9b1103197b0c109d466.jpg)  
图 28-4 SDIO 的无响应时序和无数据时序（以 SD 卡为例）

![](images/e016ecb41339bf9fc93fef6aedffd037819ea1434f69b077608a0eca551f2a23.jpg)  
图 28-5 SDIO 的多块读时序（以 SD 卡为例）

![](images/6ab608dbd16aa2e0ca3deb90d4b5bb004e9ac3bc9cb2d7d0e5c38dd3eabf6aea.jpg)  
图 28-6 SDIO 的多块写时序（以 SD 卡为例）

![](images/93d1433453c31c5451cac4646b37fc6646c3b8848e7890633574f1d54aa9f085.jpg)  
图 28-7 SDIO 的数据流读时序（以 SDIO 卡为例）

![](images/cad8ebe2a659460df6564abeb05cf64731ad9f0214ba2bb9d93e6acef1f70668.jpg)  
图 28-8 SDIO 的数据流写时序（以 SDIO 卡为例）

### 28.3.2 命令

SDIO传输大部分以命令（CMD）开始，SDIO主机通过命令向设备告知自己的意图。命令的格式如下表。

表 28-2 命令格式  

<table><tr><td>位/域的名称</td><td>起始位</td><td>传输位</td><td>命令索引</td><td>命令参数</td><td>CRC7</td><td>结束位</td></tr><tr><td>位次/宽度</td><td>47</td><td>46</td><td>[45:40] / 6bits</td><td>[39:8] / 32bits</td><td>[7:1] / 7bits</td><td>0</td></tr><tr><td>值</td><td>0b</td><td>1b</td><td>X</td><td>X</td><td>X</td><td>1b</td></tr></table>

命令大致有四种：

1） 广播命令（bc）：发给总线上的所有的卡，没有响应返回；  
2） 带响应的广播命令（bcr）：发给总线上的所有的卡，有响应返回；  
3） 点对点命令（ac）：发给特定的卡，但没有数据传输；  
4） 带数据传输的点对点命令（adtc）：发给特定的卡，并附带数据传输；

下面是一些常用的命令。

#### 28.3.2.1 基础命令

基础命令是 SD 卡所支持的一些较基本的功能。

表 28-3 SD 卡的基础命令  

<table><tr><td>命令索引</td><td>类型</td><td>参数</td><td>回复类型</td><td>简写</td><td>描述</td></tr><tr><td>CMDO</td><td>bc</td><td>[31:0]填充位无意义;</td><td>无</td><td>GO_IDLE_STATE</td><td>复位所有的卡到</td></tr><tr><td></td><td></td><td></td><td></td><td></td><td>IDLE状态</td></tr><tr><td>CMD2</td><td>bcr</td><td>[31:0]填充位无意义;</td><td>R2</td><td>ALL_SEND_CID</td><td>所有卡回复CID</td></tr><tr><td>CMD3</td><td>bcr</td><td>[31:0]填充位无意义;</td><td>R6</td><td>SEND_RELATIVE_ADDR</td><td>回复新的RCA</td></tr><tr><td>CMD7</td><td>ac</td><td>[31:16]RCA;[15:0]填充位无意义;</td><td>R1b选中卡</td><td>SELECT/DESELECT_CARD</td><td>选中或者取消选选中某个卡</td></tr><tr><td>CMD8</td><td>bcr</td><td>[31:12]保留;[11:8]供电;[7:0]校验模式;</td><td>R7</td><td>SEND_IF_COND</td><td>发送SD卡接口条件。</td></tr><tr><td>CMD9</td><td>ac</td><td>[31:16]RCA;[15:0]填充位无意义;</td><td>R2</td><td>SEND_CSD</td><td>要求CSD。</td></tr><tr><td>CMD10</td><td>ac</td><td>[31:16]RCA;[15:0]填充位无意义;</td><td>R2</td><td>SEND_CID</td><td>要求CID。</td></tr><tr><td>CMD11</td><td>ac</td><td>[31:0]填充位无意义;</td><td>R1</td><td>VOLATGE_SWITCH</td><td>切换到1.8V电平。</td></tr><tr><td>CMD12</td><td>ac</td><td>[31:0]填充位无意义;</td><td>R1b</td><td>STOP_TRANSMISSION</td><td>强制卡停止传输;</td></tr><tr><td>CMD13</td><td>ac</td><td>[31:16]RCA;[15]发送任务状态寄存器;[14:0]填充位;</td><td>R1</td><td>SEND_STATUS/SEND_TASK_STATUS</td><td>发送状态或任务状态寄存器</td></tr><tr><td>CMD15</td><td>ac</td><td>[31:16]RCA;[15:0]填充位无意义;</td><td>无</td><td>GO_INACTIVE_STATE</td><td>要求卡进到INACTIVE模式;</td></tr></table>

#### 28.3.2.2 擦除命令

SD 卡也是 FLASH 的结构，写入前也需要擦除 FLASH，但是 SD 卡内部集成了擦除逻辑，在执行写命令前如果发现没有擦除会自动补上擦除操作。在很多情况下特别是大批量写之前，如果主动执行擦除有助于提高效率。

表 28-4 SD 卡的擦除命令[1]  

<table><tr><td>命令索引</td><td>类型</td><td>参数</td><td>回复类型</td><td>简写</td><td>描述</td></tr><tr><td>CMD32</td><td>ac</td><td>[31:0]开始擦除的地址[2]</td><td>R1</td><td>ERASE_WR_BLK_START</td><td>设定擦除的首地址</td></tr><tr><td>CMD33</td><td>ac</td><td>[31:0]结束擦除的地址[2]</td><td>R1</td><td>ERASE_WR_BLK_END</td><td>设定擦除的尾地址</td></tr><tr><td>CMD38</td><td>ac</td><td>[31:0]擦除模式</td><td>R1b</td><td>ERASE</td><td>参数为0即普通擦</td></tr></table>

注1：这里的擦除命令是SD协议规范定义的SD卡的擦除命令，SDIO卡和MMC卡的擦除命令与此有区别；2：目前常用的SDHC/SDXC（2GB到2TB）级别的卡写入的擦除地址必须是块地址，即512字节对齐；

#### 28.3.2.3 块传输的读命令

表 28-5 SD 卡的块读命令[1]  

<table><tr><td>命令索引</td><td>类型</td><td>参数</td><td>回复类型</td><td>简写</td><td>描述</td></tr><tr><td>CMD16</td><td>ac</td><td>[31:0]块长度</td><td>R1</td><td>SET_BLOCKLEN</td><td>写入块长度,512</td></tr><tr><td>CMD17</td><td>adtc</td><td>[31:0]单个块的地址</td><td>R1</td><td>READ_SINGLE_BLOCK</td><td>设定单个读的起</td></tr><tr><td></td><td></td><td>[1]</td><td></td><td></td><td>始地址</td></tr><tr><td>CMD18</td><td>adtc</td><td>[31:0]多个块的地址[1]</td><td>R1</td><td>READ_multiple_BLOCK</td><td>设定单个读的起始地址</td></tr><tr><td>CMD19</td><td>adtc</td><td>[31:0]保留位</td><td>R1</td><td>SEND_TUNING_BLOCK</td><td>发送表示模式变化的64字节序列</td></tr><tr><td>CMD20</td><td>ac</td><td>[31:28]保留位[27:0]速度控制位</td><td>R1b</td><td>SPEED_CLASS_CONTROL</td><td>速度控制控制</td></tr><tr><td>CMD22</td><td>ac</td><td>[31:6]保留位[5:0]扩展地址</td><td>R1</td><td>ADDRESS_EXTENSION</td><td>SDUC才会用用到。</td></tr><tr><td>CMD23</td><td>ac</td><td>[31:0]块计数器</td><td>R1</td><td>SET_BLOCK_COUNT</td><td>块计数器</td></tr></table>

注：1：目前常用的 SDHC/SDXC（2GB到2TB）级别的卡写入的擦除地址必须是块地址，即512字节对齐。

#### 28.3.2.4 块传输的写命令

表 28-6 SD 卡的块读命令[1]  

<table><tr><td>命令索引</td><td>类型</td><td>参数</td><td>回复类型</td><td>简写</td><td>描述</td></tr><tr><td>CMD16</td><td>ac</td><td>[31:0]块长度</td><td>R1</td><td>SET_BLOCKLEN</td><td>写入块长度,512</td></tr><tr><td>CMD24</td><td>adtc</td><td>[31:0]块的地址[1]</td><td>R1</td><td>WRITE_BLOCK</td><td>设定单个块写的起始地址</td></tr><tr><td>CMD25</td><td>adtc</td><td>[31:0]多个块的地址[1]</td><td>R1</td><td>WRITE_multiple_BLOCK</td><td>设定多个块写的起始地址</td></tr><tr><td>CMD27</td><td>adtc</td><td>[31:0]填充位</td><td>R1</td><td>PROGRAM_CSD</td><td>对CSD可编程的字编程</td></tr></table>

注：1：目前常用的 SDHC/SDXC（2GB到2TB）级别的卡写入的擦除地址必须是块地址，即512字节对齐。

### 28.3.3 响应

响应作为 SDIO 设备对主机的必要回复，同样也是在 CMD 线上传输的，且必须在规定的时间内回复。响应传输为高位在前低位在后，响应长度和各比特各域的定义由响应的类型具体决定，但所有的响应都是由一个固定为0的起始位开始，跟着一个为固定 0 的传输方向位[1]。所有的响应最后都有一个停止位，固定为 1[1]。大致有 7 种响应，SD 卡支持 R1 / R1b / R2 / R3 / R6 / R7，SDIO 卡还支持R4 / R5，格式见下文。

#### 28.3.3.1 R1 响应

普通响应，总长 48 位，有 CRC7 校验，卡状态域为 32 位。格式如下表。

表 28-7 R1 的格式  

<table><tr><td>位/域的名称</td><td>起始位</td><td>传输位</td><td>响应索引</td><td>卡状态</td><td>CRC7</td><td>结束位</td></tr><tr><td>位次/宽度</td><td>47</td><td>46</td><td>[45:40] / 6 bits</td><td>[39:8] / 32 bits</td><td>[7:1] / 7 bits</td><td>0</td></tr><tr><td>值</td><td>0b</td><td>0b</td><td>跟随CMD索引</td><td>X</td><td>X</td><td>1b</td></tr></table>

#### 28.3.3.2 R1b 响应

R1b 的格式和 R1 一致，但是可以在响应后添加繁忙信号，即钳住数据线 D2。主机收到繁忙信号（检测到SDIO_D2为低）后需要进行相应处理。

#### 28.3.3.3 R2 响应

应对特定几个命令的响应，总长136位，CRC7校验包含在卡状态域中，卡状态域为 128位。卡状态域存放CID寄存器或CSD寄存器的值，CID寄存器一般作为 CMD2/CMD10 的回复，CSD寄存器一般作为 CMD9 的回复，CID/CSD 寄存器的具体含义见 28.4.2 设备寄存器部分。需要注意的是 R2 只会回复CID/CSD 寄存器的[127:1]段，CID/CSD[0]固定为 1 的保留位[2]被结束位占据，结束位固定也为 1。格式如下表。

表 28-8 R2 的格式  

<table><tr><td>位/域的名称</td><td>起始位</td><td>传输位</td><td>命令索引</td><td>卡状态</td><td>结束位</td></tr><tr><td>位次/宽度</td><td>135</td><td>134</td><td>[133:128] / 6 bits</td><td>[127:1] / 127 bits</td><td>0</td></tr><tr><td>值</td><td>0b</td><td>0b</td><td>111111b</td><td>CID/CSD</td><td>1b</td></tr></table>

#### 28.3.3.4 R3 响应

回复 OCR 寄存器的专用响应，总长 48 位，没有 CRC7 校验。OCR 寄存器一般作为 ACMD41 的回复。格式如下表。

表 28-9 R3 的格式  

<table><tr><td>位/域的名称</td><td>起始位</td><td>传输位</td><td>命令索引</td><td>卡状态</td><td>保留</td><td>结束位</td></tr><tr><td>位次/宽度</td><td>47</td><td>46</td><td>[45:40] / 6bits</td><td>[39:8] / 32bits</td><td>[7:1] / 7bits</td><td>0</td></tr><tr><td>值</td><td>0b</td><td>0b</td><td>111111b</td><td>0CR</td><td>1111111b</td><td>1b</td></tr></table>

#### 28.3.3.5 R4 响应

应对 CMD5 的响应回复 OCR 及相关寄存器的专用响应，总长 48 位，有 CRC7 校验。R4 应用在 SDIO卡中，格式如下表。

表 28-10 R4 的格式  

<table><tr><td>位/域的名称</td><td>位次</td><td>宽度（单位：bit）</td><td>值</td></tr><tr><td>起始位</td><td>47</td><td>1</td><td>0b</td></tr><tr><td>传输位</td><td>46</td><td>1</td><td>0b</td></tr><tr><td>保留</td><td>[45:40]</td><td>6</td><td>111111b</td></tr><tr><td>卡就绪</td><td>39</td><td>1</td><td>X</td></tr><tr><td>IO功能数目</td><td>[38:36]</td><td>3</td><td>X</td></tr><tr><td>当前寄存器</td><td>[35]</td><td>1</td><td>X</td></tr><tr><td>填充位</td><td>[34:33]</td><td>2</td><td>00b</td></tr><tr><td>S18A</td><td>32</td><td>1</td><td>X</td></tr><tr><td>IO OCR</td><td>[31:8]</td><td>24</td><td>OCR</td></tr><tr><td>CRC校验域</td><td>[7:1]</td><td>7</td><td>1111111b</td></tr><tr><td>结束位</td><td>0</td><td>1</td><td>1b</td></tr></table>

注：MMC卡R4的格式和 SDI0卡不同。

#### 28.3.3.6 R5 响应

应对 CMD5 的专用响应，总长 48 位，有 CRC7 校验。R5 应用在 SDIO 卡中，格式如下表。

表 28-11 R5 的格式  

<table><tr><td>位/域的名称</td><td>起始位</td><td>传输位</td><td>命令索引</td><td>填充位</td><td>响应格式</td><td>读写数据</td><td>CRC7</td><td>结束位</td></tr><tr><td>位次</td><td>47</td><td>46</td><td>[45:40]</td><td>[39:24]</td><td>[23:16]</td><td>[15:8]</td><td>[7:1]</td><td>0</td></tr><tr><td>宽度</td><td>1</td><td>1</td><td>6</td><td>16</td><td>8</td><td>8</td><td>7</td><td>1</td></tr><tr><td>值</td><td>0b</td><td>0b</td><td>110100b</td><td>0000h</td><td>X</td><td>X</td><td>X</td><td>1b</td></tr></table>

注：MMC卡R5的格式和 SDI0卡不同。

#### 28.3.3.7 R6 响应

回复 RCA 的专用响应，总长 48 位，有 CRC7 校验，格式如下表。

表 28-12 R6 的格式  

<table><tr><td>位/域的名称</td><td>起始位</td><td>传输位</td><td>命令索引</td><td>卡的RCA</td><td>卡状态位</td><td>CRC7</td><td>结束位</td></tr><tr><td>位次</td><td>47</td><td>46</td><td>[45:40]</td><td>[39:24]</td><td>[23:8]</td><td>[7:1]</td><td>0</td></tr><tr><td>宽度</td><td>1</td><td>1</td><td>6</td><td>16</td><td>16</td><td>7</td><td>1</td></tr><tr><td>值</td><td>0b</td><td>0b</td><td>000011b</td><td>X</td><td>X</td><td>X</td><td>1b</td></tr></table>

#### 28.3.3.8 R7 响应

应对 CMD8 的专用响应，表明支持的电压的信息，总长 48 位，有 CRC7 校验，格式如下表。

表 28-13 R7 的格式  

<table><tr><td>位/域的名称</td><td>起始位</td><td>传输位</td><td>命令索引</td><td>保留位</td><td>PCIe 1V2支持</td><td>PCIe响应</td><td>接受的电压</td><td>检查回馈</td><td>CRC7</td><td>结束位</td></tr><tr><td>位次</td><td>47</td><td>46</td><td>[45:40]</td><td>[39:22]</td><td>21</td><td>20</td><td>[19:16]</td><td>[15:8]</td><td>[7:1]</td><td>0</td></tr><tr><td>宽度</td><td>1</td><td>1</td><td>6</td><td>18</td><td>1</td><td>1</td><td>4</td><td>8</td><td>7</td><td>1</td></tr><tr><td>值</td><td>0b</td><td>0b</td><td>001000b</td><td>00000h</td><td>X</td><td>X</td><td>X</td><td>X</td><td>X</td><td>1b</td></tr></table>

### 28.3.4 数据传输

数据传输在数据线SDIO_D上进行，有 $1 / 4 / 8$ 位三种宽度，数据传输时一般是一个时钟的起始位在最前面，每个字节的高位在前，低位在后。对于 SD 卡仅仅支持的块传输模式，每个块的数据传输结束后还跟着CRC校验。MMC卡还支持数据流传输模式，此时不附带 CRC。下图为数据传输的格式。

![](images/1101d878d6a313c9936cd99d25f10bf87a15b491e7f18805dcea3131c819ca38.jpg)  
图28-9 SD卡单总线数据传输字节的格式

![](images/5ed21d663d7113e3932229ec5e4906f457c41ed3d434730f650445f9c5fcdb84.jpg)  
图 28-10 SD 卡 4 总线数据传输字节的格式

![](images/04e0de5227a3eef7eca0ac68164bc89e0ed362f127c88d6ab3dbfc66510b065d.jpg)  
图 28-11 SD 卡单总线数据传输一个 512 位字的格式

![](images/13a77bd65cd3991d881221ce99385e1000898567df5327bfcbdb1e4269eeda50.jpg)  
图 28-12 SD 卡 4 总线数据传输一个 512 位字的格式

## 28.4 应用

### 28.4.1 设备初始化和设备寄存器

#### 28.4.1.1 OCR 寄存器

操作条件寄存器（Operation Conditions Register）存储了 SD 卡的一些其接收的供电电压的信息和相关的状态位。相关的位定义如下表。

表 28-14 OCR 寄存器位定义  

<table><tr><td>位次</td><td>位定义</td><td>描述</td></tr><tr><td>0-3</td><td>保留。</td><td rowspan="9">支持的VDD电压范围,单位为伏特。</td></tr><tr><td>4</td><td>保留。</td></tr><tr><td>5</td><td>保留。</td></tr><tr><td>6</td><td>保留。</td></tr><tr><td>7</td><td>为低电压范围保留</td></tr><tr><td>8</td><td>保留</td></tr><tr><td>9</td><td>保留</td></tr><tr><td>10</td><td>保留</td></tr><tr><td>11</td><td>保留</td></tr><tr><td>12</td><td>保留</td><td></td></tr><tr><td>13</td><td>保留</td><td></td></tr><tr><td>14</td><td>保留</td><td></td></tr><tr><td>15</td><td>2.7-2.8</td><td></td></tr><tr><td>16</td><td>2.8-2.9</td><td></td></tr><tr><td>17</td><td>2.9-3.0</td><td></td></tr><tr><td>18</td><td>3.0-3.1</td><td></td></tr><tr><td>19</td><td>3.1-3.2</td><td></td></tr><tr><td>20</td><td>3.2-3.3</td><td></td></tr><tr><td>21</td><td>3.3-3.4</td><td></td></tr><tr><td>22</td><td>3.4-3.5</td><td></td></tr><tr><td>23</td><td>3.5-3.6</td><td></td></tr><tr><td>24</td><td>接受切换到1.8V</td><td></td></tr><tr><td>25-26</td><td>保留</td><td></td></tr><tr><td>27</td><td>超过2TB支持状态位</td><td></td></tr><tr><td>28</td><td>保留</td><td></td></tr><tr><td>29</td><td>UHS-II卡状态位</td><td>此位被置位表示此卡支持UHS-II接口。</td></tr><tr><td>30</td><td>卡容量状态(CCS)</td><td>此位被置位表示卡容量大于2GB。</td></tr><tr><td>31</td><td>卡上电状态位(busy)</td><td>此位在卡上电完成后被置位。上电完成后,其他位才有意义。</td></tr></table>

#### 28.4.1.2 CID 寄存器

CID 存储了一些身份识别信息。

表 28-15 CID 寄存器各位各域的定义  

<table><tr><td>位/域名称</td><td>简写</td><td>宽度</td><td>位次</td></tr><tr><td>厂商 ID</td><td>MID</td><td>8</td><td>[127:120]</td></tr><tr><td>应用 ID</td><td>OID</td><td>16</td><td>[119:104]</td></tr><tr><td>产品名称</td><td>PNM</td><td>40</td><td>[103:64]</td></tr><tr><td>产品版本</td><td>PRV</td><td>8</td><td>[63:56]</td></tr><tr><td>产品序列号</td><td>PSN</td><td>32</td><td>[55:24]</td></tr><tr><td>保留</td><td>无</td><td>4</td><td>[23:20]</td></tr><tr><td>生产日期</td><td>MDT</td><td>12</td><td>[19:8]</td></tr><tr><td>CRC7</td><td>CRC</td><td>7</td><td>[7:1]</td></tr><tr><td>固定位, 固定为 1</td><td>无</td><td>1</td><td>[0]</td></tr></table>

#### 28.4.1.3 CSD 寄存器

CSD 寄存器存储了 SD 卡的特征数据。以目前最常用的 SDHC 和 SDXC 卡最常用的第二版的 CSD 为例，各位域的定义如下表。

表 28-16 CSD 寄存器各位域的含义  

<table><tr><td>名称</td><td>简写</td><td>宽度</td><td>值</td><td>读写</td><td>位次</td></tr><tr><td>CSD 版本</td><td>CSDSTRUCTURE</td><td>2</td><td>01b</td><td>R0</td><td>[127:126]</td></tr><tr><td>保留</td><td>无</td><td>6</td><td>00_0000b</td><td>R0</td><td>[125120</td></tr><tr><td>读数访问时 间</td><td>TAAC</td><td>8</td><td>0Eh</td><td>R0</td><td>[119:112]</td></tr><tr><td>用时钟周期</td><td>NSAC</td><td>8</td><td>00h</td><td>R0</td><td>[111:104]</td></tr><tr><td>表示的读数访问时间</td><td></td><td></td><td></td><td></td><td></td></tr><tr><td>最大数据发送速度</td><td>TRAN_SPEED</td><td>8</td><td>32h5Ah0Bh2Bh</td><td>RO</td><td>[103:96]</td></tr><tr><td>卡命令类</td><td>CCC</td><td>12</td><td>X1X1101101X1b</td><td>RO</td><td>[95:84]</td></tr><tr><td>读数据块最大长度</td><td>READ_BL_LEN</td><td>4</td><td>9</td><td>RO</td><td>[83:80]</td></tr><tr><td>允许块部分读</td><td>READ_BL_PARTIAL</td><td>1</td><td>0</td><td>RO</td><td>[79]</td></tr><tr><td>块写不对齐</td><td>WRITE_BLK_MISALIGN</td><td>1</td><td>0</td><td>RO</td><td>[78]</td></tr><tr><td>块读不对齐</td><td>READ_BLK_MISALIGN</td><td>1</td><td>0</td><td>RO</td><td>[77]</td></tr><tr><td>执行的DSR</td><td>DSR_IMP</td><td>1</td><td>X</td><td>RO</td><td>[76]</td></tr><tr><td>保留</td><td>无</td><td>6</td><td>00_0000b</td><td>RO</td><td>[75:70]</td></tr><tr><td>设备大小</td><td>C_SIZE</td><td>22</td><td>XXXXXXXXh</td><td>RO</td><td>[69:48]</td></tr><tr><td>保留</td><td>无</td><td>1</td><td>0</td><td>RO</td><td>[47]</td></tr><tr><td>单块擦使能</td><td>ERASE_BLK_EN</td><td>1</td><td>1</td><td>RO</td><td>[46]</td></tr><tr><td>擦扇区尺寸</td><td>SECTOR_SIZE</td><td>7</td><td>7Fh</td><td>RO</td><td>[45:39]</td></tr><tr><td>写保护组大小</td><td>WP_GRP_SIZE</td><td>7</td><td>0000000b</td><td>RO</td><td>[38:32]</td></tr><tr><td>写保护组使能</td><td>WP_GRP_ENABLE</td><td>1</td><td>0</td><td>RO</td><td>[31]</td></tr><tr><td>保留</td><td>无</td><td>2</td><td>00b</td><td>RO</td><td>[30:29]</td></tr><tr><td>写速度因素</td><td>R2W_FACTOR</td><td>3</td><td>010b</td><td>RO</td><td>[28:26]</td></tr><tr><td>最大写数据块长度</td><td>WRITE_BL_LEN</td><td>4</td><td>9</td><td>RO</td><td>[25:22]</td></tr><tr><td>允许块部分写</td><td>WRITE_BL_PARTIAL</td><td>1</td><td>0</td><td>RO</td><td>[21]</td></tr><tr><td>保留</td><td>无</td><td>5</td><td>00000b</td><td>RO</td><td>[20:16]</td></tr><tr><td>文件格式组</td><td>FILE_FORMAT_GRP</td><td>1</td><td>0</td><td>RO</td><td>[15]</td></tr><tr><td>复制标志</td><td>TMP_WRITE_PROTECT</td><td>1</td><td>X</td><td>RW OTP</td><td>[14]</td></tr><tr><td>永久写保护</td><td>PERM_WRITE_PROTECT</td><td>1</td><td>X</td><td>RW OTP</td><td>[13]</td></tr><tr><td>临时写保护</td><td>TMP_WRITE_PROTECT</td><td>1</td><td>X</td><td>RW</td><td>[12]</td></tr><tr><td>文件格式</td><td>FILE_FOMAT</td><td>2</td><td>00b</td><td>RO</td><td>[11:10]</td></tr><tr><td>保留</td><td>无</td><td>2</td><td>00b</td><td>RO</td><td>[9:8]</td></tr><tr><td>CRC</td><td>CRC</td><td>7</td><td>0000000b</td><td>RW</td><td>[7:1]</td></tr><tr><td>未使用,必须使用1</td><td>无</td><td>1</td><td>1b</td><td>RO</td><td>[0]</td></tr></table>

#### 28.4.1.4 RCA 寄存器

相对卡地址寄存器存储了卡的地址，为 16 位，默认值为 0.

### 28.4.2 电压切换

在 SD 卡初始化后期，需要进行接口电平切换，将 SD 卡的时钟线数据线和命令线的 IO 电平切换到1.8V水平。对于压摆率不是足够优秀的器件，使用更低的电平标准有助于提升频率。但是SD 的供电电压并不一定变化，只在较新版本的协议才出现了低电压供电的 SD卡。

切换电压的步骤如下图。

![](images/2929199b14edf4d7b511f6b80e525be3014a41e44b2ae21cec66af49a3fc841a.jpg)  
图28-13 电压切换序列

### 28.4.3 时钟切换

初始化时 SD 卡的时钟只有 400KHz，在电压完成切换之后可以将时钟提升至较高的水平，例如SDHC 卡 UHS-I 模式第一档速度，总线时钟可以达到 80MHz，鉴于微控制器的 IO 输出能力，应将时钟限制在 50MHz 之内。

## 28.5 中断

### 28.5.1 SDIO 中断

SDIO支持多种中断源，如中断使能寄存器（R32_SDIO_IER）所示的有 24种情况都可以触发中断，用户可酌情自行开启。

### 28.5.2 SDIO 设备中断

需要注意的是不光 SDIO 外设可以向 CPU 报中断，外接 SDIO 卡和 MMC 卡也能向 SDIO 外设报中断。在 4 位总线模式下，中断线是 D1，在 8 位总线模式下，中断线是 D7，低电平有效。如果在空闲状态下 SDIO 检测到 D1 或 D7 为低电平，应读取 SDIO 设备的状态寄存器或中断标志寄存器及时响应中断。CPU 通过 R32_SDIO_SR:22 位可以得到 SDIO 主机是否收到中断。

## 28.6 寄存器描述

表 28-17 SDIO 相关寄存器列表  

<table><tr><td>名称</td><td>访问地址</td><td>描述</td><td>复位值</td></tr><tr><td>R32_SDIO_POWER</td><td>0x40018000</td><td>电源寄存器</td><td>0x00000000</td></tr><tr><td>R32_SDIO_CLKCR</td><td>0x40018004</td><td>时钟寄存器</td><td>0x00000000</td></tr><tr><td>R32_SDIOArg</td><td>0x40018008</td><td>命令参数寄存器</td><td>0x00000000</td></tr><tr><td>R32_SDIO_CMD</td><td>0x4001800C</td><td>命令寄存器</td><td>0x00000000</td></tr><tr><td>R32_SDIO_REQCMD</td><td>0x40018010</td><td>响应寄存器</td><td>0x00000000</td></tr><tr><td>R128_SDIO_REQX</td><td>0x40018014</td><td>响应参数寄存器</td><td>0x00000000</td></tr><tr><td>R32_SDIO_TIMER</td><td>0x40018024</td><td>数据定时寄存器</td><td>0x00000000</td></tr><tr><td>R32_SDIO_DLEN</td><td>0x40018028</td><td>传输长度寄存器</td><td>0x00000000</td></tr><tr><td>R32_SDIO_DCTLR</td><td>0x4001802C</td><td>数据控制寄存器</td><td>0x00000000</td></tr><tr><td>R32_SDIO_DCOUNT</td><td>0x40018030</td><td>传输计数寄存器</td><td>0x00000000</td></tr><tr><td>R32_SDIO_sta</td><td>0x40018034</td><td>状态寄存器</td><td>0x00000000</td></tr><tr><td>R32_SDIO_ICR</td><td>0x40018038</td><td>中断清除寄存器</td><td>0x00000000</td></tr><tr><td>R32_SDIO_MASK</td><td>0x4001803C</td><td>中断使能寄存器</td><td>0x0000_0000</td></tr><tr><td>R32_SDIO_FIFOCNT</td><td>0x40018048</td><td>FIFO计数器</td><td>0x0000_0000</td></tr><tr><td>R32_SDIO_FIFO</td><td>0x40018080</td><td>FIFO寄存器</td><td>0x0000_0000</td></tr></table>

### 28.6.1 电源寄存器（R32_SDIO_POWER）

偏移地址： $0 \times 0 0$

<table><tr><td>31</td><td>30</td><td>29</td><td>28</td><td>27</td><td>26</td><td>25</td><td>24</td><td>23</td><td>22</td><td>21</td><td>20</td><td>19</td><td>18</td><td>17</td><td>16</td></tr><tr><td colspan="16">Reserved</td></tr><tr><td>15</td><td>14</td><td>13</td><td>12</td><td>11</td><td>10</td><td>9</td><td>8</td><td>7</td><td>6</td><td>5</td><td>4</td><td>3</td><td>2</td><td>1</td><td>0</td></tr><tr><td colspan="14">Reserved</td><td colspan="2">PWRCTRL</td></tr></table>

<table><tr><td>位</td><td>名称</td><td>访问</td><td>描述</td><td>复位值</td></tr><tr><td>[31:2]</td><td>Reserved</td><td>R0</td><td>保留。</td><td>0</td></tr><tr><td>[1:0]</td><td>PWRCTRL</td><td>RW</td><td>电源检测位:00:电源关闭,时钟停止;01:保留;10:保留的上电状态;11:上电状态,卡时钟开启;</td><td>00b</td></tr></table>

### 28.6.2 时钟寄存器（R32_SDIO_CLKCR）

偏移地址： $0 \times 0 4$

<table><tr><td>31</td><td>30</td><td>29</td><td>28</td><td>27</td><td>26</td><td>25</td><td>24</td><td>23</td><td>22</td><td>21</td><td>20</td><td>19</td><td>18</td><td>17</td><td>16</td></tr><tr><td colspan="16">Reserved</td></tr><tr><td>15</td><td>14</td><td>13</td><td>12</td><td>11</td><td>10</td><td>9</td><td>8</td><td>7</td><td>6</td><td>5</td><td>4</td><td>3</td><td>2</td><td>1</td><td>0</td></tr><tr><td>Res</td><td>HWFC_EN</td><td>NEGEDGE</td><td colspan="2">WIDBUS</td><td>BYPAS S</td><td>PWRSA V</td><td>CLKEN</td><td colspan="8">CLKDIV</td></tr></table>

<table><tr><td>位</td><td>名称</td><td>访问</td><td>描述</td><td>复位值</td></tr><tr><td>[31:15]</td><td>Reserved</td><td>RO</td><td>保留。</td><td>0</td></tr><tr><td>14</td><td>HWFC_EN</td><td>RW</td><td>硬件流控使能位。此位置位后, TXFIFOE 和 RXFIFOF 信号才起作用。0: 关闭硬件流控;1: 开启硬件流控;</td><td>0</td></tr><tr><td>13</td><td>NEGEDGE</td><td>RW</td><td>SDIO_CK 相位选择位。0: 在 HCLK 的上升沿产生 SDIO_CK;1: 在 HLCK 的下降沿生产 SDIO_CK;</td><td>0</td></tr><tr><td>[12:11]</td><td>WIDBUS</td><td>RW</td><td>总线宽度配置域。00: 1 位总线模式, 使用 SDIO_D0;01: 4 位总线模式, 使用 SDIO_D[3:0];10: 8 位总线模式, 使用 SDIO_D[7:0];11: 未使用;</td><td>00b</td></tr><tr><td>10</td><td>BYPASS</td><td>RW</td><td>时钟旁路使能位。0: SDIO_CK 通过分频器分频得出;1: SDIO_CK 直接接到 HCLK/2 上;</td><td>0</td></tr><tr><td>9</td><td>PWRSAV</td><td>RW</td><td>空闲时时钟状态配置位。此位置位后, 总线空闲时关闭 SDIO_CK 输出以节省电能。0: SDIO_CK 始终输出;</td><td>0</td></tr><tr><td></td><td></td><td></td><td>1: SDIO_CK 只在需要时输出;</td><td></td></tr><tr><td>8</td><td>CLKEN</td><td>RW</td><td>时钟使能位。
0: SDIO_CK 被禁止输出;
1: SDIO_CK 被允许输出;</td><td>0</td></tr><tr><td>[7:0]</td><td>CLKDIV</td><td>RW</td><td>时钟分频系数域,此域表示 SDIO_CK 与 HCLK 的关系。SDIO_CK=HCLK/(CLKDIV+2)。注意在初始化阶段,SDIO_CK 应低于 400KHz。</td><td>0</td></tr></table>

注：时钟配置寄存器用来控制 SDI0_CK相关的参数，需要注意的是本寄存器在读写数据期间到7个周期内不能更改。

### 28.6.3 命令参数寄存器（R32_SDIO_ARG）

偏移地址： $0 \times 0 8$   

<table><tr><td>31</td><td>30</td><td>29</td><td>28</td><td>27</td><td>26</td><td>25</td><td>24</td><td>23</td><td>22</td><td>21</td><td>20</td><td>19</td><td>18</td><td>17</td><td>16</td></tr><tr><td colspan="16">CMDARG</td></tr><tr><td>15</td><td>14</td><td>13</td><td>12</td><td>11</td><td>10</td><td>9</td><td>8</td><td>7</td><td>6</td><td>5</td><td>4</td><td>3</td><td>2</td><td>1</td><td>0</td></tr><tr><td colspan="16">CMDARG</td></tr></table>

<table><tr><td>位</td><td>名称</td><td>访问</td><td>描述</td><td>复位值</td></tr><tr><td>[31:0]</td><td>CMDARG</td><td>RW</td><td>命令的参数域。此域存放的是命令中的参数，会作为命令的一部分一起发生到CMD线上。</td><td>0</td></tr></table>

### 28.6.4 命令寄存器（R32_SDIO_CMD）

偏移地址： $0 \times 0 0$   

<table><tr><td>31</td><td>30</td><td>29</td><td>28</td><td>27</td><td>26</td><td>25</td><td>24</td><td>23</td><td>22</td><td>21</td><td>20</td><td>19</td><td>18</td><td>17</td><td>16</td></tr><tr><td colspan="16">Reserved</td></tr><tr><td>15</td><td>14</td><td>13</td><td>12</td><td>11</td><td>10</td><td>9</td><td>8</td><td>7</td><td>6</td><td>5</td><td>4</td><td>3</td><td>2</td><td>1</td><td>0</td></tr><tr><td>Res</td><td>ATACM D</td><td>NIEN</td><td>ENCMD Comp I</td><td>SDI0S uspen d</td><td>CPSMEN</td><td>WAITP END</td><td>WAITI NT</td><td>WAITRESP</td><td colspan="7">CMDINDEX</td></tr></table>

<table><tr><td>位</td><td>名称</td><td>访问</td><td>描述</td><td>复位值</td></tr><tr><td>[31:15]</td><td>Reserved</td><td>R0</td><td>保留。</td><td>0</td></tr><tr><td>14</td><td>ATACMD</td><td>RW</td><td>进行 CE-ATA 命令。如果置此位,CPSM 将转到CMD61。</td><td>0</td></tr><tr><td>13</td><td>NIEN</td><td>RW</td><td>不使能 CE-ATA 中断设置位。如果置此位,CE-ATA 将不产生中断。</td><td>0</td></tr><tr><td>12</td><td>ENCMDCompl</td><td>RW</td><td>使能 CMD 完成信号使能位。如果置此位,命令完成将产生信号。</td><td>0</td></tr><tr><td>11</td><td>SDIOSuspend</td><td>RW</td><td>暂停命令发生位。如果置此位,将发送一个暂停信号。仅适用于 SDIO 卡。</td><td>0</td></tr><tr><td>10</td><td>CPSMEN</td><td>RW</td><td>CPSM(命令通道状态机)使能位。如果置此位,将使能 CPSM。</td><td>0</td></tr><tr><td>9</td><td>WAITPEND</td><td>RW</td><td>命令等待控制位。如果设置此位,发送命令前,</td><td>0</td></tr><tr><td></td><td></td><td></td><td>CPSM会等待数据传输完成。</td><td></td></tr><tr><td>8</td><td>WAITINT</td><td>RW</td><td>命令等待中断控制位。如果置此位,CPSM会关闭超时控制并等待中断产生。</td><td>0</td></tr><tr><td>[7:6]</td><td>WAITRESP</td><td>RW</td><td>响应类型等位域。此域指示 CPSM期望收到的响应类型。00:无响应,等待CMDSENT标志;01:短响应,等待CMDREND或CCRCFAIL标志;10:无响应,等待CMDSENT标志;11:长响应,等待CMDREND或CCRCFAIL标志;</td><td>00b</td></tr><tr><td>[5:0]</td><td>CMDINDEX</td><td>RW</td><td>命令索引域。此域表明具体的命令值。</td><td>0</td></tr></table>

### 28.6.5 响应寄存器（R32_SDIO_RESPCMD）

偏移地址： $0 \times 1 0$

<table><tr><td>31</td><td>30</td><td>29</td><td>28</td><td>27</td><td>26</td><td>25</td><td>24</td><td>23</td><td>22</td><td>21</td><td>20</td><td>19</td><td>18</td><td>17</td><td>16</td></tr><tr><td colspan="16">Reserved</td></tr><tr><td>15</td><td>14</td><td>13</td><td>12</td><td>11</td><td>10</td><td>9</td><td>8</td><td>7</td><td>6</td><td>5</td><td>4</td><td>3</td><td>2</td><td>1</td><td>0</td></tr><tr><td colspan="10">Reserved</td><td colspan="6">RESPCMD</td></tr></table>

<table><tr><td>位</td><td>名称</td><td>访问</td><td>描述</td><td>复位值</td></tr><tr><td>[31:6]</td><td>Reserved</td><td>R0</td><td>保留。</td><td>0</td></tr><tr><td>[5:0]</td><td>RESPCMD</td><td>R0</td><td>这个域记录了收到的响应的索引值。</td><td>0</td></tr></table>

### 28.6.6 响应参数寄存器（R128_SDIO_RESPX）

#### 28.6.6.1 响应参数寄存器高 32 位（R128_SDIO_RESP1[127:96]）

偏移地址： $0 \times 1 4$

<table><tr><td>31</td><td>30</td><td>29</td><td>28</td><td>27</td><td>26</td><td>25</td><td>24</td><td>23</td><td>22</td><td>21</td><td>20</td><td>19</td><td>18</td><td>17</td><td>16</td></tr><tr><td colspan="16">CARDSTATUS1</td></tr><tr><td>15</td><td>14</td><td>13</td><td>12</td><td>11</td><td>10</td><td>9</td><td>8</td><td>7</td><td>6</td><td>5</td><td>4</td><td>3</td><td>2</td><td>1</td><td>0</td></tr><tr><td colspan="16">CARDSTATUS1</td></tr></table>

#### 28.6.6.2 响应参数寄存器次高 32 位（R128_SDIO_RESP2[95:64]）

偏移地址： $0 \times 1 8$

<table><tr><td>31</td><td>30</td><td>29</td><td>28</td><td>27</td><td>26</td><td>25</td><td>24</td><td>23</td><td>22</td><td>21</td><td>20</td><td>19</td><td>18</td><td>17</td><td>16</td></tr><tr><td colspan="16">CARDSTATUS2</td></tr><tr><td>15</td><td>14</td><td>13</td><td>12</td><td>11</td><td>10</td><td>9</td><td>8</td><td>7</td><td>6</td><td>5</td><td>4</td><td>3</td><td>2</td><td>1</td><td>0</td></tr><tr><td colspan="16">CARDSTATUS2</td></tr></table>

#### 28.6.6.3 响应参数寄存器次低 32 位（R128_SDIO_RESP3[63:32]）

偏移地址： ${ 0 } { \times 1 } { 0 }$

<table><tr><td>31</td><td>30</td><td>29</td><td>28</td><td>27</td><td>26</td><td>25</td><td>24</td><td>23</td><td>22</td><td>21</td><td>20</td><td>19</td><td>18</td><td>17</td><td>16</td></tr><tr><td colspan="16">CARDSTATUS3</td></tr><tr><td>15</td><td>14</td><td>13</td><td>12</td><td>11</td><td>10</td><td>9</td><td>8</td><td>7</td><td>6</td><td>5</td><td>4</td><td>3</td><td>2</td><td>1</td><td>0</td></tr><tr><td colspan="16">CARDSTATUS3</td></tr></table>

#### 28.6.6.4 响应参数寄存器低 32 位（R128_SDIO_RESP4[31:0]）

偏移地址： $_ { 0 \times 2 0 }$

<table><tr><td>31</td><td>30</td><td>29</td><td>28</td><td>27</td><td>26</td><td>25</td><td>24</td><td>23</td><td>22</td><td>21</td><td>20</td><td>19</td><td>18</td><td>17</td><td>16</td></tr><tr><td colspan="16">CARDSTATUS4</td></tr><tr><td>15</td><td>14</td><td>13</td><td>12</td><td>11</td><td>10</td><td>9</td><td>8</td><td>7</td><td>6</td><td>5</td><td>4</td><td>3</td><td>2</td><td>1</td><td>0</td></tr><tr><td colspan="16">CARDSTATUS4</td></tr></table>

<table><tr><td>位</td><td>名称</td><td>访问</td><td>描述</td><td>复位值</td></tr><tr><td>[127:0]</td><td>CARDSTATUSx</td><td>R0</td><td>当响应为长响应时,整128位均表示卡状态;当响应为短响应时,低32位表示卡状态。SDIO外设先收到卡状态的最高位,并从R128_SDIO_RESX最低位开始存储。</td><td>0</td></tr></table>

### 28.6.7 数据定时寄存器（R32_SDIO_DTIMER）

偏移地址： ${ 0 } { \times 2 4 }$

<table><tr><td>31</td><td>30</td><td>29</td><td>28</td><td>27</td><td>26</td><td>25</td><td>24</td><td>23</td><td>22</td><td>21</td><td>20</td><td>19</td><td>18</td><td>17</td><td>16</td></tr><tr><td colspan="16">DATATIME</td></tr><tr><td>15</td><td>14</td><td>13</td><td>12</td><td>11</td><td>10</td><td>9</td><td>8</td><td>7</td><td>6</td><td>5</td><td>4</td><td>3</td><td>2</td><td>1</td><td>0</td></tr><tr><td colspan="16">DATATIME</td></tr></table>

<table><tr><td>位</td><td>名称</td><td>访问</td><td>描述</td><td>复位值</td></tr><tr><td>[31:0]</td><td>DATATIME</td><td>RW</td><td>数据超时时长。以SD10_CK的周期数为单位。</td><td>0</td></tr></table>

### 28.6.8 传输长度寄存器（R32_SDIO_DLEN）

偏移地址： $_ { 0 \times 2 8 }$

<table><tr><td>31</td><td>30</td><td>29</td><td>28</td><td>27</td><td>26</td><td>25</td><td>24</td><td>23</td><td>22</td><td>21</td><td>20</td><td>19</td><td>18</td><td>17</td><td>16</td></tr><tr><td colspan="7">Reserved</td><td colspan="9">DATALENGTH</td></tr><tr><td>15</td><td>14</td><td>13</td><td>12</td><td>11</td><td>10</td><td>9</td><td>8</td><td>7</td><td>6</td><td>5</td><td>4</td><td>3</td><td>2</td><td>1</td><td>0</td></tr><tr><td colspan="16">DATALENGTH</td></tr></table>

<table><tr><td>位</td><td>名称</td><td>访问</td><td>描述</td><td>复位值</td></tr><tr><td>[31:25]</td><td>Reserved</td><td>R0</td><td>保留。</td><td>0</td></tr><tr><td>[24:0]</td><td>DATALENGTH</td><td>RW</td><td>传输数据长度域。此域的值在开启传输时会被加载到传输计数器。对于块传输,此域的值必须是块大小的整数倍,块大小由SDIO设备定义,存储在R32_SDIO_DCTLR[7:4]中,常见值为512B等。</td><td>0</td></tr></table>

### 28.6.9 数据控制寄存器（R32_SDIO_DCTRL）

偏移地址： $\mathtt { 0 } \mathtt { x } 2 0$

<table><tr><td>31</td><td>30</td><td>29</td><td>28</td><td>27</td><td>26</td><td>25</td><td>24</td><td>23</td><td>22</td><td>21</td><td>20</td><td>19</td><td>18</td><td>17</td><td>16</td></tr><tr><td colspan="16">Reserved</td></tr><tr><td>15</td><td>14</td><td>13</td><td>12</td><td>11</td><td>10</td><td>9</td><td>8</td><td>7</td><td>6</td><td>5</td><td>4</td><td>3</td><td>2</td><td>1</td><td>0</td></tr><tr><td colspan="3">Reserved</td><td>SDIOEN</td><td>RWMOD</td><td>RWSTOP</td><td>RWSTART</td><td colspan="4">DBLOCKSIZE</td><td>DMAEN</td><td>DTMODE</td><td>DTDIR</td><td>DTEN</td><td></td></tr></table>

<table><tr><td>位</td><td>名称</td><td>访问</td><td>描述</td><td>复位值</td></tr><tr><td>[31:12]</td><td>Reserved</td><td>RO</td><td>保留。</td><td>0</td></tr><tr><td>11</td><td>SDIOEN</td><td>RW</td><td>SDIO使能位。此位被置位后, DPSM才能进行运转。</td><td>0</td></tr><tr><td>10</td><td>RWMOD</td><td>RW</td><td>读等待模式。0: 停止SDIO_CK控制读等待;1: 使用SDIO_D2控制读等待;</td><td>0</td></tr><tr><td>9</td><td>RWSTOP</td><td>RW</td><td>读等待停止位。如果RWSTART位被置位,那么读等待将被停止。</td><td>0</td></tr><tr><td>8</td><td>RWSTART</td><td>RW</td><td>读等待开始位。置此位将执行读等待操作。</td><td>0</td></tr><tr><td>[7:4]</td><td>DBLOCKSIZE</td><td>RW</td><td>数据块长度域。此域存储了数据块的长度,采用块传输前必须定义块传输长度。此域可以写入的值为0到1110b之间,表示的块传输长度为2BLKLEN,即0到16384字节之间。</td><td>0</td></tr><tr><td>3</td><td>DMAEN</td><td>RW</td><td>DMA使能位。置此位使能DMA。</td><td>0</td></tr><tr><td>2</td><td>DTMODE</td><td>RW</td><td>传输模式设置位。置此位设置传输模式。0: 块传输;1: 流传输;</td><td>0</td></tr><tr><td>1</td><td>DTDIR</td><td>RW</td><td>传输方向设置位。置此位设置传输方向。0: 控制器到卡;1: 卡到控制器;</td><td>0</td></tr><tr><td>0</td><td>DTEN</td><td>RW</td><td>传输使能位。置此位开始数据传输。具体流程为,置此位后,DPSM进入Wait_S或Wait_R的流程(取决于传输方向),</td><td>0</td></tr></table>

### 28.6.10 传输计数寄存器（R32_SDIO_DCOUNT）

偏移地址： $\mathtt { 0 } { \times } 3 0$

<table><tr><td>31</td><td>30</td><td>29</td><td>28</td><td>27</td><td>26</td><td>25</td><td>24</td><td>23</td><td>22</td><td>21</td><td>20</td><td>19</td><td>18</td><td>17</td><td>16</td></tr><tr><td colspan="7">Reserved</td><td colspan="9">DATACOUNT</td></tr><tr><td>15</td><td>14</td><td>13</td><td>12</td><td>11</td><td>10</td><td>9</td><td>8</td><td>7</td><td>6</td><td>5</td><td>4</td><td>3</td><td>2</td><td>1</td><td>0</td></tr><tr><td colspan="16">DATACOUNT</td></tr></table>

<table><tr><td>位</td><td>名称</td><td>访问</td><td>描述</td><td>复位值</td></tr><tr><td>[31:25]</td><td>Reserved</td><td>R0</td><td>保留。</td><td>0</td></tr><tr><td>[24:0]</td><td>DATACOUNT</td><td>R0</td><td>传输数据计数器域。在启动传输时，发送长度</td><td>0</td></tr><tr><td></td><td></td><td></td><td>寄存器的值将会被加载到此计数器中，并随着传输过程递减。</td><td></td></tr></table>

### 28.6.11 状态寄存器（R32_SDIO_STA）

偏移地址： $0 \times 3 4$

<table><tr><td colspan="7">Reserved</td><td>CEAT
AEND</td><td>SDIOI
T</td><td>RXDAV
L</td><td>TXDAV
L</td><td>RXFI
FOE</td><td>TXFIF
OE</td><td>RXFIF
OF</td><td>TXFI
FOF</td><td></td></tr><tr><td>15</td><td>14</td><td>13</td><td>12</td><td>11</td><td>10</td><td>9</td><td>8</td><td>7</td><td>6</td><td>5</td><td>4</td><td>3</td><td>2</td><td>1</td><td>0</td></tr><tr><td>RXFI
FOHF</td><td>TXFI
FOHE</td><td>RXACT</td><td>TXACT</td><td>CMDACT</td><td>DBCK
END</td><td>STBI
TERR</td><td>DATA
END</td><td>CMDS
ENT</td><td>CMDRE
ND</td><td>RXOV
ERR</td><td>TXUND
ERR</td><td>DTIME
OUT</td><td>CTIME
EOUT</td><td>DCRCF
AIL</td><td>CCRCF
AIL</td></tr></table>

<table><tr><td>位</td><td>名称</td><td>访问</td><td>描述</td><td>复位值</td></tr><tr><td>[31:24]</td><td>Reserved</td><td>R0</td><td>保留。</td><td>0</td></tr><tr><td>23</td><td>CEATAEND</td><td>R0</td><td>此位被置位时,CMD61接收到CE-ATA完成信号。</td><td>0</td></tr><tr><td>22</td><td>SDIOIT</td><td>R0</td><td>此位被置位时,SDIO收到设备中断。</td><td>0</td></tr><tr><td>21</td><td>RXDAVL</td><td>R0</td><td>此位被置位时,接收FIFO数据可用。</td><td>0</td></tr><tr><td>20</td><td>TXDAVL</td><td>R0</td><td>此位被置位时,发送FIFO中的数据可用。</td><td>0</td></tr><tr><td>19</td><td>RXFIFOE</td><td>R0</td><td>此位被置位时,接收FIFO空。</td><td>0</td></tr><tr><td>18</td><td>TXFIFOE</td><td>R0</td><td>此位被置位时,发送FIFO空。</td><td>0</td></tr><tr><td>17</td><td>RXFIFOF</td><td>R0</td><td>此位被置位时,接收FIFO满。</td><td>0</td></tr><tr><td>16</td><td>TXFIFOF</td><td>R0</td><td>此位被置位时,发送FIFO满。</td><td>0</td></tr><tr><td>15</td><td>RXFIFOHF</td><td>R0</td><td>此位被置位时,接收FIFO半满。</td><td>0</td></tr><tr><td>14</td><td>TXFIFOHE</td><td>R0</td><td>此位被置位时,发送FIFO半空。</td><td>0</td></tr><tr><td>13</td><td>RXACT</td><td>R0</td><td>此位被置位时,正在接收数据。</td><td>0</td></tr><tr><td>12</td><td>TXACT</td><td>R0</td><td>此位被置位时,正在发送数据。</td><td>0</td></tr><tr><td>11</td><td>CMDACT</td><td>R0</td><td>此位被置位时,正在传输命令。</td><td>0</td></tr><tr><td>10</td><td>DBCKEND</td><td>R0</td><td>此位被置位时,已经发送或者接受了数据块且CRC校验通过。</td><td>0</td></tr><tr><td>9</td><td>STBITERR</td><td>R0</td><td>此位被置位时,在宽总线模式下,所有数据线都没有检测到起始信号。</td><td>0</td></tr><tr><td>8</td><td>DATAEND</td><td>R0</td><td>此位被置位时,数据传输结束(传输计数器为零)。</td><td>0</td></tr><tr><td>7</td><td>CMDSENT</td><td>R0</td><td>此位被置位时,命令已经发送出去。</td><td>0</td></tr><tr><td>6</td><td>CMDREND</td><td>R0</td><td>此位被置位时,已经收到响应,CRC检验成功。</td><td>0</td></tr><tr><td>5</td><td>RXOVERR</td><td>R0</td><td>此位被置位时,接收FIFO上溢。</td><td>0</td></tr><tr><td>4</td><td>TXUNDERR</td><td>R0</td><td>此位被置位时,发送FIFO下溢。</td><td>0</td></tr><tr><td>3</td><td>DTIMEOUT</td><td>R0</td><td>此位被置位时,数据超时。</td><td>0</td></tr><tr><td>2</td><td>CTIMEOUT</td><td>R0</td><td>此位被置位时,命令超时超过了64个SDIO_CK周期。</td><td>0</td></tr><tr><td>1</td><td>DCRCFAIL</td><td>R0</td><td>此位被置位时,已经发送或者接受了数据块但CRC校验失败。</td><td>0</td></tr><tr><td>0</td><td>CCRCFAIL</td><td>R0</td><td>此位被置位时,已经收到响应,但CRC检验失</td><td>0</td></tr><tr><td></td><td></td><td></td><td>败。</td><td></td></tr></table>

### 28.6.12 中断清除寄存器（R32_SDIO_ICR）

偏移地址： ${ 0 } { \times } \ 3 { 8 }$

<table><tr><td colspan="5">Reserved</td><td>CEATA
ENDC</td><td>SDIOI
TC</td><td colspan="7">Reserved</td><td></td><td></td></tr><tr><td>15</td><td>14</td><td>13</td><td>12</td><td>11</td><td>10</td><td>9</td><td>8</td><td>7</td><td>6</td><td>5</td><td>4</td><td>3</td><td>2</td><td>1</td><td>0</td></tr><tr><td>Reserved</td><td>DBCKE
NDC</td><td>STBIT
ERRC</td><td>DATA
ENDC</td><td>CMDSEN
TC</td><td>CMDR
ENDC</td><td>RXOV
ERRC</td><td>TXUND
ERRC</td><td>DTIME
OUTC</td><td>CTIME
OUTC</td><td>DCRCF
AILC</td><td>DCRCF
AILC</td><td>CCRCF
AILC</td><td>CCRCF
AILC</td><td>CCRCF
AILC</td><td></td></tr></table>

<table><tr><td>位</td><td>名称</td><td>访问</td><td>描述</td><td>复位值</td></tr><tr><td>[31:24]</td><td>Reserved</td><td>RO</td><td>保留。</td><td>0</td></tr><tr><td>23</td><td>CEATAENDC</td><td>RW</td><td>置此位清除状态寄存器中的CEATAEND位。</td><td>0</td></tr><tr><td>22</td><td>SDIOITC</td><td>RW</td><td>置此位清除状态寄存器中的SDIOIT位。</td><td>0</td></tr><tr><td>[21:11]</td><td>Reserved</td><td>RW</td><td>保留。</td><td>0</td></tr><tr><td>10</td><td>DBCKENDC</td><td>RW</td><td>置此位清除状态寄存器中的DBCKEND位。</td><td>0</td></tr><tr><td>9</td><td>STBITERRC</td><td>RW</td><td>置此位清除状态寄存器中的STBITERR位。</td><td>0</td></tr><tr><td>8</td><td>DATAENDC</td><td>RW</td><td>置此位清除状态寄存器中的DATAEND位。</td><td>0</td></tr><tr><td>7</td><td>CMDSENTC</td><td>RW</td><td>置此位清除状态寄存器中的CMDSENT位。</td><td>0</td></tr><tr><td>6</td><td>CMDRENDC</td><td>RW</td><td>置此位清除状态寄存器中的CMDREND位。</td><td>0</td></tr><tr><td>5</td><td>RXOVERRC</td><td>RW</td><td>置此位清除状态寄存器中的RXOVERR位。</td><td>0</td></tr><tr><td>4</td><td>TXUNDERRC</td><td>RW</td><td>置此位清除状态寄存器中的TXUNDERR位。</td><td>0</td></tr><tr><td>3</td><td>DTIMEOUTC</td><td>RW</td><td>置此位清除状态寄存器中的DTIMEOUT位。</td><td>0</td></tr><tr><td>2</td><td>CTIMEOUTC</td><td>RW</td><td>置此位清除状态寄存器中的CTIMEOUT位。</td><td>0</td></tr><tr><td>1</td><td>DCRCFAILC</td><td>RW</td><td>置此位清除状态寄存器中的DCRCFAIL位。</td><td>0</td></tr><tr><td>0</td><td>CCRCFAILC</td><td>RW</td><td>置此位清除状态寄存器中的CCRCFAIL位。</td><td>0</td></tr></table>

### 28.6.13 中断使能寄存器（R32_SDIO_MASK）

偏移地址： $\mathtt { 0 } \mathtt { x } \mathtt { 3 0 }$

<table><tr><td colspan="8">Reserved</td><td>CEATAE
NDIE</td><td>SDIOI
TIE</td><td>RXDAV
LIE</td><td>TXDAV
LIE</td><td>RXFI
FOEI
E</td><td>TXFIF
OEIE</td><td>RXFIF
OFIE</td><td>TXFI
FOI
E</td></tr><tr><td>15</td><td>14</td><td>13</td><td>12</td><td>11</td><td>10</td><td>9</td><td>8</td><td>7</td><td>6</td><td>5</td><td>4</td><td>3</td><td>2</td><td>1</td><td>0</td></tr><tr><td>RXFI
FOHF
IE</td><td>TXFI
FOHE
IE</td><td>RXACT
IE</td><td>TXACT
IE</td><td>CMDACT
IE</td><td>DBCK
ENDI
E</td><td>STBI
TERR
IE</td><td>DATA
ENDI
E</td><td>CMDS
ENTI
E</td><td>CMDRE
NDIE</td><td>RXOV
ERRI
E</td><td>TXUND
ERRIE</td><td>DTIME
OUTIE</td><td>CTIME
EOUT
IE</td><td>DCRCF
AILIE</td><td>CCRCF
AILIE</td></tr></table>

<table><tr><td>位</td><td>名称</td><td>访问</td><td>描述</td><td>复位值</td></tr><tr><td>[31:24]</td><td>Reserved</td><td>R0</td><td>保留。</td><td>0</td></tr><tr><td>23</td><td>CEATAENDIE</td><td>RW</td><td>置此位后,状态寄存器在置CEATAEND位时将产生中</td><td>0</td></tr><tr><td></td><td></td><td></td><td>断。</td><td></td></tr><tr><td>22</td><td>SDIOITIE</td><td>RW</td><td>置此位后,状态寄存器在置SDIOIT位时将产生中断。</td><td>0</td></tr><tr><td>21</td><td>RXDAVLIE</td><td>RW</td><td>置此位后,状态寄存器在置RXDAVL位时将产生中断。</td><td>0</td></tr><tr><td>20</td><td>TXDAVLIE</td><td>RW</td><td>置此位后,状态寄存器在置TXDAVL位时将产生中断。</td><td>0</td></tr><tr><td>19</td><td>RXFIFOEIE</td><td>RW</td><td>置此位后,状态寄存器在置RXFIFOE位时将产生中断。</td><td>0</td></tr><tr><td>18</td><td>TXFIFOEIE</td><td>RW</td><td>置此位后,状态寄存器在置TXFIFOE位时将产生中断。</td><td>0</td></tr><tr><td>17</td><td>RXFIFOFIE</td><td>RW</td><td>置此位后,状态寄存器在置RXFIFOF位时将产生中断。</td><td>0</td></tr><tr><td>16</td><td>TXFIFOFIE</td><td>RW</td><td>置此位后,状态寄存器在置TXFIFOF位时将产生中断。</td><td>0</td></tr><tr><td>15</td><td>RXFIFOHFIE</td><td>RW</td><td>置此位后,状态寄存器在置RXFIFOHF位时将产生中断。</td><td>0</td></tr><tr><td>14</td><td>TXFIFOHEIE</td><td>RW</td><td>置此位后,状态寄存器在置TXFIFOHE位时将产生中断。</td><td>0</td></tr><tr><td>13</td><td>RXACTIE</td><td>RW</td><td>置此位后,状态寄存器在置RXACT位时将产生中断。</td><td>0</td></tr><tr><td>12</td><td>TXACTIE</td><td>RW</td><td>置此位后,状态寄存器在置TXACT位时将产生中断。</td><td>0</td></tr><tr><td>11</td><td>CMDACTIE</td><td>RW</td><td>置此位后,状态寄存器在置CMDACT位时将产生中断。</td><td>0</td></tr><tr><td>10</td><td>DBCKENDIE</td><td>RW</td><td>置此位后,状态寄存器在置DBCKEND位时将产生中断。</td><td>0</td></tr><tr><td>9</td><td>STBITERRIE</td><td>RW</td><td>置此位后,状态寄存器在置STBITERR位时将产生中断。</td><td>0</td></tr><tr><td>8</td><td>DATAENDIE</td><td>RW</td><td>置此位后,状态寄存器在置DATAEND位时将产生中断。</td><td>0</td></tr><tr><td>7</td><td>CMDSENTIE</td><td>RW</td><td>置此位后,状态寄存器在置CMDSENT位时将产生中断。</td><td>0</td></tr><tr><td>6</td><td>CMDRENDIE</td><td>RW</td><td>置此位后,状态寄存器在置CMDREND位时将产生中断。</td><td>0</td></tr><tr><td>5</td><td>RXOVERRIE</td><td>RW</td><td>置此位后,状态寄存器在置RXOVERR位时将产生中断。</td><td>0</td></tr><tr><td>4</td><td>TXUNDERRIE</td><td>RW</td><td>置此位后,状态寄存器在置TXUNDERR位时将产生中断。</td><td>0</td></tr><tr><td>3</td><td>DTIMEOUTIE</td><td>RW</td><td>置此位后,状态寄存器在置DTIMEOUT位时将产生中断。</td><td>0</td></tr><tr><td>2</td><td>CTIMEOUTIE</td><td>RW</td><td>置此位后,状态寄存器在置CTIMEOUT位时将产生中断。</td><td>0</td></tr><tr><td>1</td><td>DCRCFAILIE</td><td>RW</td><td>置此位后,状态寄存器在置DCRCFAIL位时将产生中断。</td><td>0</td></tr><tr><td>0</td><td>CCRCFAILIE</td><td>RW</td><td>置此位后,状态寄存器在置CCRCFAIL位时将产生中断。</td><td>0</td></tr></table>

### 28.6.14 FIFO 计数寄存器（R32_SDIO_FIFOCNT）

偏移地址： $0 \times 4 8$

<table><tr><td>31</td><td>30</td><td>29</td><td>28</td><td>27</td><td>26</td><td>25</td><td>24</td><td>23</td><td>22</td><td>21</td><td>20</td><td>19</td><td>18</td><td>17</td><td>16</td></tr><tr><td colspan="16">FIFOCOUNT</td></tr><tr><td>15</td><td>14</td><td>13</td><td>12</td><td>11</td><td>10</td><td>9</td><td>8</td><td>7</td><td>6</td><td>5</td><td>4</td><td>3</td><td>2</td><td>1</td><td>0</td></tr><tr><td colspan="16">FIFOCOUNT</td></tr></table>

<table><tr><td>位</td><td>名称</td><td>访问</td><td>描述</td><td>复位值</td></tr><tr><td>[31:0]</td><td>FIFOCOUNT</td><td>RO</td><td>FIFO0包含还未写入FIFO0或者还未从FIFO0读出的字(32bit)数。在设置R32_SDIO_DCTLR:R32_SDIO_DCTLR时,如果DPSM空闲,FIFO计数器将从R32_SDIO_TLEN中加载传输长度值,如果此值不能被4整除,则最后的1至3个字节会被当做一个字处理。</td><td>0</td></tr></table>

### 28.6.15 FIFO（R32_SDIO_FIFO）

偏移地址： $\mathtt { 0 } \mathtt { x } 8 0$

<table><tr><td>31</td><td>30</td><td>29</td><td>28</td><td>27</td><td>26</td><td>25</td><td>24</td><td>23</td><td>22</td><td>21</td><td>20</td><td>19</td><td>18</td><td>17</td><td>16</td></tr><tr><td colspan="16">FIFODATA</td></tr><tr><td>15</td><td>14</td><td>13</td><td>12</td><td>11</td><td>10</td><td>9</td><td>8</td><td>7</td><td>6</td><td>5</td><td>4</td><td>3</td><td>2</td><td>1</td><td>0</td></tr><tr><td colspan="16">FIFODATA</td></tr></table>

<table><tr><td>位</td><td>名称</td><td>访问</td><td>描述</td><td>复位值</td></tr><tr><td>[31:0]</td><td>FIFODATA</td><td>RW</td><td>FIFO数据域。此域即为FIFO的数据。读写此域将读出接收到的数据或者发送待发的数据。SDIO的FIFO共32个字(一个字为32位)。</td><td>0</td></tr></table>


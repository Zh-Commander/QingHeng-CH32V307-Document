# 第 20 章 串行外设接口（SPI/I2S）

本章模块描述适用于CH32F2x、CH32V2x和CH32V3x微控制器全系列产品。

SPI 支持以三线同步串行模式进行数据交互，加上片选线支持硬件切换主从模式，支持以单根数据线通讯。

I2S 也是三线的同步串行接口通信协议，它支持四种音频标准，包括飞利浦 I2S 标准、MSB 对齐标准、LSB对齐标准和PCM标准。

## 20.1 主要特征

### 20.1.1 SPI 特征

支持全双工同步串行模式  
$\bullet$ 支持单线半双工模式  
$\bullet$ 支持主模式和从模式，多从模式  
$\bullet$ 支持 8 位或 16 位数据结构  
$\bullet$ 最高时钟频率支持到Fpclk的一半  
$\bullet$ 数据顺序支持 MSB 或 LSB 在前  
$\bullet$ 支持硬件或软件控制 NSS 引脚  
$\bullet$ 收发支持硬件CRC校验  
收发缓冲器支持 DMA 传输  
支持修改时钟相位和极性

### 20.1.2 I2S 特征

支持单工通信  
$\bullet$ 支持主模式和从模式  
$\bullet$ 支持 16位、24位和32位数据格式  
$\bullet$ 音频采样频率支持范围 8KHz-562.2KHz  
$\bullet$ 支持时钟极性可编程  
$\bullet$ 支持常用 I2S 协议：飞利浦标准、MSB 对齐标准、MSL 对齐标准和 PCM 标准  
$\bullet$ 收发缓冲器支持DMA传输  
支持主时钟向外部音频设备输出

## 20.2 SPI 功能描述

### 20.2.1 概述

![](images/476574ac4e3558365d2744980c8cb437730a822800b668853bd394334f17eb73.jpg)  
图 20-1 SPI 结构框图

由图 20-1可以看出，与 SPI相关的主要是MISO、MOSI、SCK和 NSS四个引脚。其中 MISO引脚在SPI 模块工作在主模式下时，是数据输入引脚；工作在从模式下时，是数据输出引脚。MOSI 引脚工作在主模式下时，是数据输出引脚；工作在从模式时，是数据输入引脚。SCK是时钟引脚，时钟信号一直由主机输出，从机接收时钟信号并同步数据收发。NSS引脚是片选引脚，有以下用法：

1） NSS由软件控制：此时SSM被置位，内部 NSS信号由 SSI决定输出高还是低，这种情况一般用于SPI 主模式；  
2） NSS 由硬件控制：在 NSS 输出使能时，即 SSOE 置位时，在 SPI 主机向外发送输出时会主动拉低NSS 引脚，如果拉低 NSS 脚，则会产生一个硬件错误；SSOE 不置位，则可以用于多主机模式，如果它被拉低则会强行进入从机模式，MSTR 位会被自动清除。

可以通过 CPHA 和 CPOL 配置 SPI 的工作模式。CPHA 置位表示模块在时钟的第二个边沿进行数据采样，数据被锁存，CPHA 不置位表示 SPI 模块在时钟的第一个边沿进行采样，数据被锁存。CPOL 则表示无数据时时钟保持高电平还是低电平。具体见下图 20-2。

![](images/df8fc581a1ae9a93f1cd71a8790b909e5bba462e1f4b0aee30f493ffbcc01aa0.jpg)  
图 20-2 SPI 模式

主机和设备需要设置为相同的SPI模式，在配置 SPI模式前，需要清除 SPE位。DEF位可以决定SP的单个数据长度是8位还是16位。LSBFIRST 可以控制单个数据字是高位在前还是低位在前。

### 20.2.2 主模式

在SPI模块工作在主模式时，由SCK产生串行时钟。配置成主模式进行以下步骤：

配置控制寄存器的BR[2:0]域来确定时钟；

配置 CPOL 和 CPHA 位来确定 SPI 模式；

配置DEF确定数据字长；

配置 LSBFIRST 确定帧格式；

配置 NSS 引脚，比如置 SSOE 位让硬件去置 NSS。也可以置 SSM 位并把 SSI 位置高；

置 MSTR 位和 SPE 位，需要保证 NSS 此时已经是高。

需要发送数据时只需要向数据寄存器写要发送的数据就行了。SPI会从发送缓冲区并行地把数据送到移位寄存器，然后按照 LSBFIRST 的设置将数据从移位寄存器发出去，当数据已经到了移位寄存器时，TXE 标志会被置位，如果已经置位了 TXEIE，那么会产生中断。如果 TXE 标志位置位需要向数据寄存器里填数据，维持完整的数据流。

当接收器接收数据时，当数据字的最后一个采样时钟沿到来时，数据从移位寄存器并行地转移到接收缓冲区，RXNE位被置位，如果之前置位了 RXNEIE 位，还会产生中断。此时应该尽快读取数据寄

存器取走数据。

### 20.2.3 从模式

当SPI模块工作在从模式时，SCK用于接收主机发来的时钟，自身的波特率设置无效。配置成从模式的步骤如下：

配置 DEF 位设置数据位长度；

配置 CPOL 和 CPHA 位匹配主机模式；

配置 LSBFIRST 匹配主机数据帧格式；

硬件管理模式下，NSS 管脚需要保持为低电平，如果设置 NSS 为软件管理（SSM 置位），那么请保持 SSI 不被置位；

清除 MSTR 位，置 SPE 位，开启 SPI 模式。

在发送时，当SCK出现第一个从机接收采样沿时，从机开始发送。发送的过程就是发送缓冲区的数据移到发送移位寄存器，当发送缓冲区的数据移到了移位寄存器之后，会置位 TXE 标志，如果之前置位了TXEIE位，那么会产生中断。

在接收时，最后一个时钟采样沿之后，RXNE 位被置位，移位寄存器接收到的字节被转移到接收缓冲区，读数据寄存器的读操作可以获得接收缓冲区里的数据。如果在 RXNE 置位之前 RXNEIE已经被置位，那么会产生中断。

### 20.2.4 单工模式

SPI 接口可以工作在半双工模式，即主设备使用 MOSI 引脚，从设备使用 MISO引脚进行通讯。使用半双工通讯时需要把 BIDIMODE 置位，使用 BIDIOE 控制传输方向。

在正常全双工模式下将RXONLY位置位可以将 SPI模块设置为仅仅接收的单工模式，在RXONLY置位之后会释放一个数据脚，主模式和从模式释放的引脚并不相同。也可以不理会接收的数据将 SPI 置成只发送的模式。

### 20.2.5 CRC

SPI 模块使用 CRC 校验保证全双工通信的可靠性，数据收发分别使用单独的 CRC 计算器。CRC 计算的多项式由多项式寄存器决定，对于8位数据宽度和 16位数据宽度，会分别使用不同的计算方法。

设置 CRCEN 位会启用 CRC 校验，同时会使 CRC 计算器复位。在发送完最后一个数据字节后，置CRCNEXT 位会在当前字节发送结束后发送 TXCRCR 计算器的计算结果，同时最后接收到的接收移位寄存器的值如果与本地算出来的RXCRCR的计算值不相符，那么 CRCERR位会被置位。使用 CRC校验需要在配置 SPI 工作模式时设置多项式计算器并置 CRCEN 位，并在最后一个字或半字置 CRCNEXT 位发送CRC 并进行接收 CRC 的校验。注意，收发双方的 CRC 计算多项式应该统一。

### 20.2.6 DMA

SPI 模块支持使用DMA加快数据通讯速度，可以使用 DMA向发送缓冲区填写数据，或使用 DMA从接收缓冲区及时取走数据。DMA会以RXNE和 TXE为信号及时取走或发来数据。DMA也可以工作在单工或加CRC校验的模式。

### 20.2.7 错误

#### 主模式失效错误

当SPI工作在NSS引脚硬件管理模式下，发生了外部拉低 NSS引脚的操作；或在 NSS引脚软件管理模式下，SSI 位被清零；或 SPE 位被清零，导致 SPI 被关闭；或 MSTR 位被清零，SPI 进入从模式。如果 ERRIE位已经被置位，还会产生中断。清除 MODF 位步骤：首先执行一次对 R16_SPI1_STATR的读或写操作，然后写 R16_SPI1_CTLR1。

#### 溢出错误

如果主机发送了数据，而从设备的接收缓冲区中还有未读取的数据，就会发生溢出错误，OVR位被置位，如果ERRIE被置位还会产生中断。发送溢出错误应该重新开始当前传输。读取数据寄存器再读取状态寄存器会消除此位。

#### CRC 错误

当接收到的CRC校验字和RXCRCR的值不匹配时，会产生 CRC校验错误，CRCERR 位会被置位。

### 20.2.8 中断

SPI 模块的中断支持五个中断源，其中发送缓冲区空、接收缓冲区非空这两个事件分别会置位 TXE和RXNE，在分别置位了TXEIE和RXNEIE位的情况下会产生中断。除此之外上面提到的三种错误也会产生中断，分别是MODF、OVR和CRCERR，在使能了ERRIE位之后，这三种错误也会产生错误中断。

## 20.3 I2S 功能描述

### 20.3.1 I2S 概述

![](images/d590c704cde633a24861e6be07e6e49b11dd1e3b8845e14356938200119e0d96.jpg)  
图 20-3 I2S 结构框图

通过将寄存器 I2SCFGR 的 I2SMOD 位置位，使能 I2S 功能。此时，可以把 SPI 模块用作 I2S 音频接口。I2S与SPI共用3个引脚：

● SD：串行数据(映射至 MOSI 引脚)，用来发送和接收 2 路时分复用通道的数据；  
$\bullet$ WS：字选(映射至NSS引脚)，主模式下作为数据控制信号输出，从模式下作为输入；  
● CK：串行时钟(映射至 SCK 引脚)，主模式下作为时钟信号输出，从模式下作为输入。

在某些外部音频设备需要主时钟时，可以另有一个附加引脚输出时钟：

● MCK：主时钟(独立映射)，在I2S配置为主模式，寄存器 I2SPR的 MCKOE 位为 1 时，作为输出额外的时钟信号引脚使用。输出时钟信号的频率预先设置为 $2 5 6 \times \mathsf { F s }$ ，其中 Fs 是音频信号的采样频率。

设置成主模式时，I2S使用自身的时钟发生器来产生通信用的时钟信号。这个时钟发生器也是主时钟输出的时钟源。I2S 模式下有 2 个额外的寄存器，一个是与时钟发生器配置相关的寄存器 I2SPR，另一个是I2S通用配置寄存器I2SCFGR(可设置音频标准、从/主模式、数据格式、数据包帧、时钟极性等参数)。在I2S模式下不使用寄存器CTLR1和所有的 CRCR寄存器。同样，I2S 模式下也不使用寄存器 CTLR2 的 SSOE 位，和寄存器 STATR 的 MODF 位和 CRCERR 位。I2S 使用与 SPI 相同的寄存器 DATAR用作16位宽模式数据传输。

### 20.3.2 支持的音频协议

三线总线支持 2 个声道上音频数据的时分复用：左声道和右声道，但是只有一个 16 位寄存器用作发送或接收。因此，软件必须在对数据寄存器写入数据时，根据当前传输中的声道写入相应的数据；同样，在读取寄存器数据时，通过检查寄存器 STATR的 CHSIDE位来判断接收到的数据属于哪个声道。左声道总是先于右声道发送数据(CHSIDE 位在 PCM 协议下无意义)。有四种可用的数据和包帧组合。可以通过以下四种数据格式发送数据：

● 16 位数据打包进 16 位帧  
$\bullet$ 16 位数据打包进 32 位帧  
● 24 位数据打包进 32 位帧  
● 32 位数据打包进 32 位帧

在使用 16 位数据扩展到 32 位帧时，前 16 位(MSB)是有意义的数据，后 16 位(LSB)被强制为 0，该操作不需要软件干预，也不需要有DMA请求(仅需要一次读或写操作)。24位和32 位数据帧需要 CPU对寄存器 DATAR 进行 2 次读或写操作，在使用 DMA 时，需要 2 次 DMA 传输。对于 24 位数据，扩展到32 位后，最低 8 位由硬件置 0。对于所有的数据格式和通讯标准，总是先发送最高位(MSB)。I2S 接口支持四种音频标准，可以通过设置寄存器I2SCFGR的 I2SSTD[1:0]位和PCMSYNC 位来选择。

#### 20.3.2.1 I2S 飞利浦标准

在此标准下，引脚WS用来指示正在发送的数据属于哪个声道。在发送第一位数据(MSB)前 1个时钟周期，该引脚即为有效。发送方在时钟信号(CK)的下降沿改变数据，接收方在上升沿读取数据。WS信号也在时钟信号的下降沿变化。

![](images/57bbf1b7a7dbf997edb7fd15113377ab11453da3585b30dea28d3c2c38d6d086.jpg)  
图 20-4 飞利浦协议波形（16/32 全精度，CPOL=0）

![](images/b9c486504ca2e1bd9c1d53d1382ca01ad1d3a753c87c8b7e0b7d900162b62f04.jpg)  
图 20-5 飞利浦协议波形（24 位帧，CPOL=0）

此模式需要对寄存器 SPI_DATAR 进行 2 次读或写操作。在发送模式下：如果需要发送 0x8EAA33(24位)：

![](images/b650626dff07f2f9086b5a7683ae2a954d5012ca7463e0f7aa7fc1d30d643967.jpg)

在接收模式下：如果接收 0x8EAA33：

![](images/0907232c400a5a8e80319d3849824b361a97715904069b810b807eef4be56eca.jpg)

![](images/3291b4a9f2317a4ecb856b91900210089747602f031290a553082984c0071b42.jpg)  
图 20-6 飞利浦协议标准波形（16 位扩展至 32 位包帧， $\mathsf { C P O L = 0 } ,$ ）

在I2S配置阶段，如果选择将16位数据扩展到 32位声道帧，只需要访问一次寄存器 DATAR用来扩展到 32 位的低 16 位被硬件置为 $0 \times 0 0 0 0$ 。如果待传输或接收的数据是 $0 \times 7 6 { \tt A } 3$ (扩展到 32 位是$0 \times 7 6 \mathsf { A } 3 0 0 0 0 )$ )，只需要操作一次 DATAR。在发送时需要将 MSB 写入寄存器 DATAR；标志位 TXE 为 1 表示可以写入新的数据，如果允许了相应的中断，则可以产生中断。发送是由硬件完成的，即使还未发送出后 16位的 $0 \times 0 0 0 0$ ，也会设置TXE并产生相应的中断。接收时，每次收到高 16位半字(MSB)后，标志位RXNE置1，如果允许了相应的中断，则可以产生中断。这样，在 2次读和写之间有更多的时间，可以防止下溢或上溢的情况发生。

#### 20.3.2.2 MSB 对齐标准

在此标准下，WS 信号和第一个数据位，即最高位(MSB)同时产生。

![](images/c6a11620da4cc7e0732c16722eaddb88f4d9e40d503163d1716c1aa54637f836.jpg)  
图 20-7 MSB 对齐 16 位或 32 位全精度（ $\mathsf { C P O L } = \mathsf { 0 } ;$ ）

发送方在时钟信号的下降沿改变数据；接收方是在上升沿读取数据。

![](images/cd78a0f2a4e6da0bb4628d74ac563e4009e1b70f35285c823f6a892c2203a249.jpg)  
图 20-8 MSB 对齐 24 位数据， $\mathsf { C P O L } = 0$

![](images/fa3890327993f71de7d5d1c9f103f3840763d71d16b37ae3163f0230e1040935.jpg)  
图 20-9 MSB 对齐 16 位数据扩展到 32 位包帧，CPOL = 0

#### 20.3.2.3 LSB 对齐标准

此标准与 MSB 对齐标准类似(在 16 位或 32 位全精度帧格式下无区别)。

![](images/680c6b81334c1d37fd6bad86a23373b087096a835a8d6437bc75afcc7a1295c3.jpg)  
图 20-10 LSB 对齐 16 或 32 位全精度， $\mathsf { C P O L } = 0$

![](images/9eda589a32cf7c9506b87bf07918ec874149927d8310cc6489e6bd6badc26142.jpg)  
图 20-11 LSB 对齐 24 位数据， $\mathsf { C P O L } = 0$

在发送模式下如果要发送数据0x3478AE，需要通过软件或 DMA对寄存器 DATAR进行 2次写操作。

![](images/13d341ef011df9c3bef58bae99f1022a2a74406fbfc2892589f911c6cfd4f64b.jpg)

在接收模式下如果要接收数据 0x3478AE，需要在 2 个连续的 RXNE 事件发生时，分别对寄存器DATAR 进行 1 次读操作。

![](images/a1b18618f68bac0bcc4efb8a12bd1dfca026f6e09339d4546a23fc8646b3c56a.jpg)

![](images/767298636c7d3877449a1f533220af109fe60bb1446b3c953f86d88b02dce1e5.jpg)  
图 20-12 LSB 对齐 16 位数据扩展到 32 位包帧， CPOL = 0

在I2S配置阶段，如果选择将16位数据扩展到32 声道帧，只需要访问一次寄存器 DATAR。此时，扩展到 32 位后的高半字(16 位 MSB)被硬件置为 $0 \times 0 0 0 0$ 。

如果待传输或接收的数据是 $0 \times 7 6 { \tt A } 3$ (扩展到 32位是 $0 \times 0 0 0 0 7 6 \mathsf { A } 3 )$ ，只需要操作一次 DATAR，在发送时，如果 TXE 为 1，用户需要写入待发送的数据(即 $0 \times 7 6 A 3 )$ )。用来扩展到 32 位的 $0 \times 0 0 0 0$ 部分由硬件首先发送出去，一旦有效数据开始从 SD 引脚送出，即发生下一次 TXE 事件。在接收时，一旦接收

到有效数据(而不是 $0 \times 0 0 0 0$ 部分)，即发生 RXNE 事件。这样，在 2 次读和写之间有更多的时间，可以防止下溢或上溢的情况发生。

#### 20.3.2.4 PCM 标准

在 PCM 标准下，不存在声道选择的信息。PCM 标准有 2 种可用的帧结构，短帧或长帧，可以通过设置寄存器 I2SCFGR 的 PCMSYNC 位来选择。

![](images/13e6f0df173a33bf48e51a1a3c741adace721da959d8e5b0f33037e4953a5030.jpg)  
图 20-13 PCM 标准波形（16 位）

对于长帧，主模式下，用来同步的 WS 信号有效的时间固定为 13 位。对于短帧，用来同步的 WS信号长度只有 1 位。

![](images/873020fbb2da7a5b172b5fcb9fffe5cb9c70a4c9bdc96c7680333e59629fa922.jpg)  
图 20-14 PCM 标准波形（16 位）

无论哪种模式(主或从)、哪种同步方式(短帧或长帧)，连续的 2 帧数据之间和 2 个同步信号之间的时间差，(即使是从模式)需要通过设置I2SCFGR寄存器的 DATLEN位和CHLEN位来确定。

### 20.3.3 时钟发生器

I2S 的比特率即确定了在I2S数据线上的数据流和 I2S的时钟信号频率。I2S比特率 $=$ 每个声道的比特数 $\times$ 声道数目 $\times$ 音频采样频率。

对于一个具有左右声道和 16 位音频信号，I2S 比特率计算如下：

I2S 比特率 $= 1 6 \times 2 \times F s$ s

如果包长为 32 位，I2S 比特率计算如下：

I2S 比特率 $= 3 2 \times 2 \times { \mathsf { F } } { \mathsf { s } }$

![](images/d8fb7d1e72f91add5d7a5ced0ced10a66acd5504d0fa7e2773145be56b3d3c4f.jpg)  
图20-15 音频采样频率定义

在主模式下，为了获得需要的音频频率，需要正确地对线性分频器进行设置。

![](images/268676fdb3e8076bbb092cf2a3e6e21da984b93754f2c7c4932e9c30e5f39749.jpg)  
图 20-16 I2S 时钟发生器结构

图中 $1 2 \mathsf { S x C L K }$ 的时钟源是系统时钟(即驱动 AHB时钟的 HSI、HSE或PLL)。 $1 2 \mathsf { S x C L K }$ 可以来自SYSCLK，或 PLL3 VCO(2xPLL3CLK)时钟，可以通过 RCC_CFGR2 寄存器的 I2S2SRC 和 I2S3SRC 位选择。音频的采样频率可以是 96KHz、48KHz、44.1KHz、32KHz、22.05KHz、16KHz、11.025KHz 或8KHz(或任何此范围内的数值)。为了获得需要的频率，需按照以下公式设置线性分频器：

当需要生成主时钟时(寄存器 SPI_I2SPR 的 MCKOE 位为 1)：

声道的帧长为 16 位时， $\mathsf { F } _ { 5 } = \mathsf { 1 2 S } \times \mathsf { G L K } / \left[ \left( 1 6 ^ { * } 2 \right) \ * \ \left( \left( 2 ^ { * } \mathsf { I } 2 \mathsf { S D } \mathsf { I } \mathsf { V } \right) \ + \ \mathsf { 0 } \mathsf { D } \mathsf { D } \right) { * } 8 \right]$

声道的帧长为 32 位时， Fs = I2SxCLK / [(32*2) * ((2*I2SDIV) + ODD)*4]

当关闭主时钟时(MCKOE 位为’ $0 ^ { \prime }$ ’)：

声道的帧长为 16 位时， $\mathsf { F } \mathsf { s } = \mathsf { 1 2 S x C L K } / \left[ \left( 1 6 * 2 \right) * \left( \left( 2 * | 2 \mathsf { S } \mathsf { D } | \mathsf { V } \right) + \mathsf { 0 } \mathsf { D } \mathsf { D } \right) \right]$

声道的帧长为 32 位时， Fs = I2SxCLK / [(32*2) * ((2*I2SDIV) + ODD)]

### 20.3.4 I2S 主模式

设置 I2S 工作在主模式，串行时钟由引脚 CK 输出，字选信号由引脚 WS 产生。可以通过设置寄存器I2SPR的MCKOE位来选择输出或不输出主时钟(MCK)。

#### 20.3.4.1 配置流程

l 设置寄存器I2SPR的I2SDIV[7:0]定义与音频采样频率相符的串行时钟波特率。同时也要定义寄存器 I2SPR 的 ODD 位。  
设置CKPOL位定义通信时钟在空闲时的电平状态。如果需要向外部的 DAC/ADC音频器件提供主

时钟 MCK，需寄存器 I2SPR 的 MCKOE 位置为。

l 设置寄存器 I2SCFGR 的 I2SMOD 位为 1 激活 I2S 功能，设置 I2SSTD[1:0]和 PCMSYNC 位选择所用的 I2S 标准，设置 CHLEN 选择每个声道的数据位数。还要设置寄存器 SPI_I2SCFGR 的 I2SCFG[1:0]选择 I2S主模式和方向(发送端还是接收端)。  
如果需要，可以通过设置寄存器 CR2 来打开所需的中断功能和 DMA 功能。  
$\bullet$ 必须将寄存器 I2SCFGR 的 I2SE 位置为 1。  
$\bullet$ 引脚 WS 和 CK 需要配置为输出模式。如果寄存器 SPI_I2SPR 的 MCKOE 位为 1，引脚 MCK 也要配置成输出模式。

#### 20.3.4.2 发送流程

当写入1个半字(16位)的数据至发送缓存，发送流程开始。假设第一个写入发送缓存的数据对应的是左声道数据。当数据从发送缓存移到移位寄存器时，标志位 TXE置 1，这时，要把对应右声道的数据写入发送缓存。标志位 CHSIDE 提示了目前待传输的数据对应哪个声道。标志位 CHSIDE 的值在TXE为1时更新，因此它在TXE为1时有意义。在先左声道后右声道的数据都传输完成后，才能被认为是一个完整的数据帧。不可以只传输部分数据帧，如仅有左声道的数据。

当发出第一位数据的同时，半字数据被并行地传送至 16位移位寄存器，然后后面的位依次按高位在先的顺序从引脚MOSI/SD发出。每次数据从发送缓存移至移位寄存器时，标志位 TXE置为 1，如果寄存器CR2的TXEIE位为1，则产生中断。

为了保证连续的音频数据传输，建议在当前传输完成之前，对寄存器 DATAR写入下一个要传输的数据。建议在要关闭 I2S 功能时，等待标志位 TXE=1 及 ${ \tt B S Y } { = } 0$ ，再将 I2SE 位清’0’。

#### 20.3.4.3 接收流程

接收流程的配置步骤除了第3点外，与发送流程的一致(参见前述的”发送流程”)，需要通过配置I2SCFG[1:0]来选择主接收模式。无论何种数据和声道长度，音频数据总是以 16位包的形式接收。即每次填满接收缓存后，标志位RXNE置1，如果寄存器 CR2的 RXNEIE位为 1，则产生中断。根据配置的数据和声道长度，收到左声道或右声道的数据会需要 1 次或 2次把数据传送到接收缓存的过程。对寄存器DATAR进行读操作即可清除 RXNE标志位。每次接收以后即更新 CHSIDE。它的值取决于I2S单元产生的WS信号。如果前一个接收到的数据还没有被读取，又接收到新数据，即发生上溢，标志位OVR被置为1，如果寄存器CR2的 ERRIE位为1，则产生中断，表示发生了错误。若要关闭I2S功能，需要执行特别的操作，以保证 I2S模块可以正常地完成传输周期而不会开始新的数据传输。操作过程与数据配置和通道长度、以及音频协议的模式相关：

● 16 位数据扩展到 32 位通道长度(DATLEN=00 并且 CHLEN=1)，使用 LSB(低位)对齐模式( $\angle 2 \mathsf { S S } \mathsf { T D } = 1 0$ ) a) 等待倒数第二个 $( n - 1 ) \mathsf { R } \mathsf { X N E } { = } 1$ ；

b) 等待 17 个 I2S 时钟周期(使用软件延迟)；  
c) 关闭 I2S( $I 2 S E = 0$ )。

$\bullet$ 16 位数据扩展到 32 位通道长度(DATLEN=00 并且 CHLEN=1)，使用 MSB(高位)对齐、I2S 或 PCM 模式(分别为 $1 2 5 5 \mathsf { T D } { = } 0 0$ 、 $\lvert 2 \mathsf { S S } \mathsf { T D } { = } 0 \rvert$ 或 $1 2 \thinspace \mathrm { S S } \thinspace \mathsf { T D } { = } 1 \thinspace 1$ )

a) 等待最后一个 ${ \sf R X N E } = 1$ ；  
b) 等待 1 个 I2S 时钟周期(使用软件延迟)；  
c) 关闭 I2S( $I 2 S E = 0$ )。

$\bullet$ 所有其它DATLEN和CHLEN的组合，I2SSTD选择的任意音频模式，使用下述方式关闭 I2S：

a) 等待倒数第二个 $( n - 1 ) \mathsf { R } \mathsf { X N E } { = } 1$ ；  
b) 等待一个 I2S 时钟周期(使用软件延迟)；  
c) 关闭 I2S( $I 2 S E = 0$ )。

注： 在传输期间BSY标志始终为低。

### 20.3.5 I2S 从模式

在从模式下，I2S可以设置成发送和接收模式。从模式的配置方式基本遵循和配置主模式一样的流程。在从模式下，不需要I2S接口提供时钟。时钟信号和 WS信号都由外部主 I2S设备提供，连接到相应的引脚上。因此用户无需配置时钟。

配置步骤如下：

l 设置寄存器 I2SCFGR 的 I2SMOD 位激活 I2S 功能；设置 I2SSTD[1:0]来选择所用的 I2S 标准；设置DATLEN[1:0]选择数据的比特数；设置 CHLEN 选择每个声道的数据位数。设置寄存器 I2SCFGR的I2SCFG[1:0]选择I2S从模式的数据方向(发送端还是接收端)。  
根据需要，设置寄存器 CR2 打开所需的中断功能和 DMA 功能。  
必须设置寄存器 I2SCFGR 的 I2SE 位为 1。

#### 20.3.5.1 发送流程

当外部主设备发送时钟信号，并且当 NSS_WS信号请求传输数据时，发送流程开始。必须先使能从设备，并且写入I2S数据寄存器之后，外部主设备才能开始通信。对于 I2S的 MSB对齐和LSB对齐模式，第一个写入数据寄存器的数据项对应左声道的数据。当开始通信时，数据从发送缓冲器传送到移位寄存器，然后标志位TXE置为1；这时，要把对应右声道的数据项写入 I2S数据寄存器。标志位 CHSIDE 提示了目前待传输的数据对应哪个声道。与主模式的发送流程相比，在从模式中，CHSIDE取决于来自外部主I2S的WS信号。这意味着从 I2S在接收到主机生成的时钟信号之前，就要准备好第一个要发送的数据。 WS信号为1表示先发送左声道。

#### 20.3.5.2 接收流程

配置步骤除了第1点外，与发送流程一致。需要通过配置 I2SCFG[1:0]来选择主接收模式。无论何种数据和声道长度，音频数据总是以 16位包的形式接收，即每次填满接收缓存，标志位 RXNE置1，如果寄存器I2S_CTLR2的RXNEIE位为 1，则产生中断。按照不同的数据和声道长度设置，收到左声道或右声道数据会需要1次或2次传输数据至接收缓冲器的过程。每次接收到数据(将要从DATAR读出)以后即更新CHSIDE，它对应I2S 单元产生的 WS信号。读取SPI_DATAR寄存器，将清除RXNE位。在还没有读出前一个接收到的数据，又接收到新数据时，即产生上溢，并设置标志位 OVR为1；如果寄存器I2S_CTLR2的ERRIE位为1，则产生中断，指示发生了错误。要关闭 I2S功能时，需要在接收到最后一次RXNE=1时将I2SE位清0。

### 20.3.6 状态标志位

有 3 个状态标志位供用户监控 I2S 总线的状态。

#### 20.3.6.1 状态标志位（BSY）

BSY 标志由硬件设置与清除(写入此位无效果)，该标志位指示 I2S通信层的状态。该位为 1时表明 I2S 通讯正在进行中，但有一个例外：主接收模式 $( 1 2 \mathsf { S C F G } = 1 1 )$ )下，在接收期间 BSY 标志始终为低。在软件要关闭SPI模块之前，可以使用 BSY标志检测传输是否结束，这样可以避免破坏最后一次传输，因此需要严格按照下述过程执行。当传输开始时， BSY 标志被置为 1，除非 I2S 模块处于主接收模式。当传输结束时或者当关闭I2S模块时，该标志位被清除。当通信是连续的时候在主发送模式时，整个传输期间， BSY 标志始终为高；在从模式时，每个数据项传输之间， BSY 标志在 1个 I2S 时钟周期内变低。

#### 20.3.6.2 发送缓冲标志位（TXE）

该标志位为1表示发送缓冲器为空，可以对发送缓冲器写入新的待发送数据。在发送缓冲器中已有数据时，标志位清0。在I2S被关闭时(I2SE位为 0)，该标志位也为 0。

#### 20.3.6.3 接收缓存非空标志位（RXNE）

该标志位置1表示在接收缓存里有接收到的有效数据。在读取寄存器 DATAR时，该位清 0。

#### 20.3.6.4 声道标志位（CHSIDE）

在发送模式下，该标志位在TXE为高时刷新，指示从 SD引脚上发送的数据所在的声道。如果在从发送模式下发生了下溢错误，该标志位的值无效，在重新开始通讯前需要把 I2S关闭再打开。在接收模式下，该标志位在寄存器 DATAR 接收到数据时刷新，指示接收到的数据所在的声道。注意，如果发生错误，该标志位无意义，需要将 I2S 关闭再打开。在 PCM 标准下，无论短帧格式还是长帧格式，这个标志位都没有意义。如果寄存器 STATR的标志位 OVR或 UDR为 1，且寄存器 CR2的 ERRIE位为1，则会产生中断。(中断源已经被清除后)可以通过读寄存器 STATR来清除中断标志。

### 20.3.7 错误标志位

#### 20.3.7.1 下溢标志位（UDR）

在从发送模式下，如果数据传输的第一个时钟边沿到达时，新的数据仍然没有写入 DATAR 寄存器，该标志位会被置 1。在寄存器 I2SCFGR 的 I2SMOD 位置 1 后，该标志位才有效。如果寄存器 CR2的ERRIE位为1，就会产生中断。通过对寄存器 STATR进行读操作来清除该标志位。

#### 20.3.7.2 上溢标志位（OVR）

如果还没有读出前一个接收到的数据时，又接收到新的数据，即产生上溢，该标志位置 1，如果寄存器 CTLR2 的 ERRIE 位为 1，则产生中断指示发生了错误。这时，接收缓存的内容，不会刷新为从发送设备送来的新数据。对寄存器DATAR的读操作返回最后一个正确接收到的数据。其他所有在上溢发生后由发送设备发出的16位数据都会丢失。通过先读寄存器 DATAR再读寄存器 STATR，来清除该标志位。

### 20.3.8 I2S 中断

I2S 有4个中断源，其中发送缓冲区空，接收缓冲区非空这两个事件分别会置位 TXE和 RXNE，在分别置位了TXEIE和RXNEIE位的情况下会产生中断。如果还没有读出前一个接收到的数据时，又接收到新的数据，即产生上溢，如果置位了 ERRIE会产生上溢中断；在从发送模式下，如果数据传输的第一个时钟边沿到达时，新的数据仍然没有写入 DATAR 寄存器如果置位了 ERRIE 会产生下溢中断。

### 20.3.9 DMA 功能

DMA 的工作方式在 I2S 模式除了 CRC 功能不可用以外，与在 SPI 模式完全相同。因为在 I2S 模式下没有数据传输保护系统。

## 20.4 寄存器描述

表 20-1 SPI1 相关寄存器列表  

<table><tr><td>名称</td><td>访问地址</td><td>描述</td><td>复位值</td></tr><tr><td>R16_SPI1_CTLR1</td><td>0x40013000</td><td>SPI1 控制寄存器 1</td><td>0x0000</td></tr><tr><td>R16_SPI1_CTLR2</td><td>0x40013004</td><td>SPI1 控制寄存器 2</td><td>0x0000</td></tr><tr><td>R16_SPI1_STATR</td><td>0x40013008</td><td>SPI1 状态寄存器</td><td>0x0002</td></tr><tr><td>R16_SPI1_DATAR</td><td>0x4001300C</td><td>SPI1 数据寄存器</td><td>0x0000</td></tr><tr><td>R16_SPI1_CRCR</td><td>0x40013010</td><td>SPI1 多项式寄存器</td><td>0x0007</td></tr><tr><td>R16_SPI1_RCRCR</td><td>0x40013014</td><td>SPI1 接收 CRC 寄存器</td><td>0x0000</td></tr><tr><td>R16_SPI1_TCRCR</td><td>0x40013018</td><td>SPI1 发送 CRC 寄存器</td><td>0x0000</td></tr><tr><td>R16_SPI1_I2S_CFGR</td><td>0x4001301C</td><td>SPI1_I2S配置寄存器</td><td>0x0000</td></tr><tr><td>R16_SPI1_HSCR</td><td>0x40013024</td><td>SPI1高速控制寄存器</td><td>0x0000</td></tr></table>

表 20-2 SPI2 相关寄存器列表  

<table><tr><td>名称</td><td>访问地址</td><td>描述</td><td>复位值</td></tr><tr><td>R16_SPI2_CTLR1</td><td>0x40003800</td><td>SPI2 控制寄存器 1</td><td>0x0000</td></tr><tr><td>R16_SPI2_CTLR2</td><td>0x40003804</td><td>SPI2 控制寄存器 2</td><td>0x0000</td></tr><tr><td>R16_SPI2_STATR</td><td>0x40003808</td><td>SPI2 状态寄存器</td><td>0x0002</td></tr><tr><td>R16_SPI2_DATAR</td><td>0x4000380C</td><td>SPI2 数据寄存器</td><td>0x0000</td></tr><tr><td>R16_SPI2_CRCR</td><td>0x40003810</td><td>SPI2 多项式寄存器</td><td>0x0007</td></tr><tr><td>R16_SPI2_RCRCR</td><td>0x40003814</td><td>SPI2 接收 CRC 寄存器</td><td>0x0000</td></tr><tr><td>R16_SPI2_TCRCR</td><td>0x40003818</td><td>SPI2 发送 CRC 寄存器</td><td>0x0000</td></tr><tr><td>R16_SPI2_I2S_CFGR</td><td>0x4000381C</td><td>SPI2_I2S 配置寄存器</td><td>0x0000</td></tr><tr><td>R16_SPI2_I2SPR</td><td>0x40003820</td><td>SPI2_I2S 预分频寄存器</td><td>0x0000</td></tr><tr><td>R16_SPI2_HSCR</td><td>0x40003824</td><td>SPI2 高速控制寄存器</td><td>0x0000</td></tr></table>

表 20-3 SPI3 相关寄存器列表  

<table><tr><td>名称</td><td>访问地址</td><td>描述</td><td>复位值</td></tr><tr><td>R16_SPI3_CTLR1</td><td>0x40003C00</td><td>SPI3 控制寄存器 1</td><td>0x0000</td></tr><tr><td>R16_SPI3_CTLR2</td><td>0x40003C04</td><td>SPI3 控制寄存器 2</td><td>0x0000</td></tr><tr><td>R16_SPI3_STATR</td><td>0x40003C08</td><td>SPI3 状态寄存器</td><td>0x0002</td></tr><tr><td>R16_SPI3_DATAR</td><td>0x40003C0C</td><td>SPI3 数据寄存器</td><td>0x0000</td></tr><tr><td>R16_SPI3_CRCR</td><td>0x40003C10</td><td>SPI3 多项式寄存器</td><td>0x0007</td></tr><tr><td>R16_SPI3_RCRCR</td><td>0x40003C14</td><td>SPI3 接收 CRC 寄存器</td><td>0x0000</td></tr><tr><td>R16_SPI3_TCRCR</td><td>0x40003C18</td><td>SPI3 发送 CRC 寄存器</td><td>0x0000</td></tr><tr><td>R16_SPI3_I2S_CFGR</td><td>0x40003C1C</td><td>SPI3_I2S 配置寄存器</td><td>0x0000</td></tr><tr><td>R16_SPI3_I2SPR</td><td>0x40003C20</td><td>SPI3_I2S 预分频寄存器</td><td>0x0000</td></tr><tr><td>R16_SPI3_HSCR</td><td>0x40003C24</td><td>SPI3 高速控制寄存器</td><td>0x0000</td></tr></table>

### 20.4.1 SPI 控制寄存器 1（SPIx_CTLR1）（x=1/2/3）

偏移地址： $0 \times 0 0$

<table><tr><td>15</td><td>14</td><td>13</td><td>12</td><td>11</td><td>10</td><td>9</td><td>8</td><td>7</td><td>6</td><td>5</td><td>4</td><td>3</td><td>2</td><td>1</td><td>0</td></tr><tr><td>BIDI
MODE</td><td>BIDI
OE</td><td>CRCEN</td><td>CRC
NEXT</td><td>DFF</td><td>RX
ONLY</td><td>SSM</td><td>SSI</td><td>LSB
FIRST</td><td>SPE</td><td colspan="3">BR[2:0]</td><td>MSTR</td><td>CPOL</td><td>CPHA</td></tr></table>

<table><tr><td>位</td><td>名称</td><td>访问</td><td>描述</td><td>复位值</td></tr><tr><td>15</td><td>BIDIMODE</td><td>RW</td><td>单向数据模式使能位。1:选择单线双向模式;0:选择双线双向模式。</td><td>0</td></tr><tr><td>14</td><td>BIDIOE</td><td>RW</td><td>单线输出使能位,和BIDIMODE配合使用。1:使能输出,仅发送;0:禁止输出,仅接收。</td><td>0</td></tr><tr><td>13</td><td>CRCEN</td><td>RW</td><td>硬件CRC校验使能位,该位只能在SPE为0时写入,该位只能在全双工模式下使用。</td><td>0</td></tr><tr><td></td><td></td><td></td><td>1:启动CRC计算;0:禁止CRC计算。</td><td></td></tr><tr><td>12</td><td>CRCNEXT</td><td>RW</td><td>在接下来的一次数据传输后,发送CRC寄存器的值。这位应该在向数据寄存器写入最后一个数据后立刻置位。1:发送CRC校验结果;0:继续发送数据寄存器的数据。</td><td>0</td></tr><tr><td>11</td><td>DFF</td><td>RW</td><td>数据帧长度位,此位只能在SPE为0时写入。1:使用16位数据长度进行收发;0:使用8位数据长度进行收发。</td><td>0</td></tr><tr><td>10</td><td>RXONLY</td><td>RW</td><td>双线模式下只接收位,该位和BIDIMODE配合使用。置此位可以让设备只接收不发送。1:只接收,单工模式;0:全双工模式。</td><td>0</td></tr><tr><td>9</td><td>SSM</td><td>RW</td><td>片选引脚管理位,此位决定NSS引脚的电平由硬件还是软件控制。1:软件控制NSS引脚;0:硬件控制NSS引脚。</td><td>0</td></tr><tr><td>8</td><td>SSI</td><td>RW</td><td>片选引脚控制位,在SSM置位的情况下,此位决定NSS引脚的电平。1:NSS为高电平;0:NSS为低电平。</td><td>0</td></tr><tr><td>7</td><td>LSBFIRST</td><td>RW</td><td>帧格式控制位。不可以在通讯时修改此位。1:先发送LSB;0:先发送MSB。</td><td>0</td></tr><tr><td>6</td><td>SPE</td><td>RW</td><td>SPI使能位。1:启用SPI;0:禁用SPI。</td><td>0</td></tr><tr><td>[5:3]</td><td>BR[2:0]</td><td>RW</td><td>波特率设置域,在通讯时不可以修改此域。000:FPCLK/2;001:FPCLK/4;010:FPCLK/8;011:FPCLK/16;100:FPCLK/32;101:FPCLK/64;110:FPCLK/128;111:FPCLK/256。</td><td>000b</td></tr><tr><td>2</td><td>MSTR</td><td>RW</td><td>主从设置位,在通讯时不可以修改此位。1:配置为主设备;0:配置为从设备。</td><td>0</td></tr><tr><td>1</td><td>CPOL</td><td>RW</td><td>时钟极性选择位,在通讯时不可以修改此位。1:空闲状态时,SCK保持高电平;0:空闲状态时,SCK保持低电平。</td><td>0</td></tr><tr><td>0</td><td>CPHA</td><td>RW</td><td>时钟相位设置位,在通讯时不可以修改此位。1:数据采样从第二个时钟沿开始;0:数据采样从第一个时钟沿开始。</td><td>0</td></tr></table>

### 20.4.2 SPI 控制寄存器 2（SPIx_CTLR2）（x=1/2/3）

偏移地址： $0 \times 0 4$

<table><tr><td>Reserved</td><td>TXEIE</td><td>RXNEIE</td><td>ERRIE</td><td>Reserved</td><td>SSOE</td><td>TXDMAEN</td><td>RXDMAEN</td></tr></table>

控制寄存器 2  

<table><tr><td>位</td><td>名称</td><td>访问</td><td>描述</td><td>复位值</td></tr><tr><td>[15:8]</td><td>Reserved</td><td>R0</td><td>保留。</td><td>0</td></tr><tr><td>7</td><td>TXEIE</td><td>RW</td><td>发送缓冲区空中断使能位。置此位允许 TXE 被置位时产生中断。</td><td>0</td></tr><tr><td>6</td><td>RXNEIE</td><td>RW</td><td>接收缓冲区非空中断使能位。置此位允许 RXNE 被置位时产生中断。</td><td>0</td></tr><tr><td>5</td><td>ERRIE</td><td>RW</td><td>错误中断使能位。置此位允许在产生错误 (CRCERR, OVR, MODF)时产生中断。</td><td>0</td></tr><tr><td>[4:3]</td><td>Reserved</td><td>R0</td><td>保留。</td><td>0</td></tr><tr><td>2</td><td>SSOE</td><td>RW</td><td>SS 输出使能。禁止 SS 输出可以工作在多主模式下。1: 使能 SS 输出;0: 禁止主模式下的 SS 输出。</td><td>0</td></tr><tr><td>1</td><td>TXDMAEN</td><td>RW</td><td>发送缓冲区 DMA 使能位。1: 启用发送缓冲区 DMA;0: 禁用发送缓冲区 DMA。</td><td>0</td></tr><tr><td>0</td><td>RXDMAEN</td><td>RW</td><td>接收缓冲区 DMA 使能位。1: 启用接收缓冲区 DMA;0: 禁用接收缓冲区 DMA。</td><td>0</td></tr></table>

### 20.4.3 SPI 状态寄存器（SPIx_STATR）（x=1/2/3）

偏移地址： $0 \times 0 8$

<table><tr><td>Reserved</td><td>BSY</td><td>OVR</td><td>MODF</td><td>CRC
ERR</td><td>UDR</td><td>CHS ID
E</td><td>TXE</td><td>RXNE</td></tr></table>

<table><tr><td>位</td><td>名称</td><td>访问</td><td>描述</td><td>复位值</td></tr><tr><td>[15:8]</td><td>Reserved</td><td>R0</td><td>保留。</td><td>0</td></tr><tr><td>7</td><td>BSY</td><td>R0</td><td>忙标志位,该位由硬件置位或复位。1:SPI正在通讯,或发送缓冲区非空;0:SPI不在通讯。</td><td>0</td></tr><tr><td>6</td><td>OVR</td><td>R0</td><td>溢出标志位,该位由硬件置位,软件复位。1:出现溢出错误;0:没有出现溢出错误。</td><td>0</td></tr><tr><td>5</td><td>MODF</td><td>R0</td><td>模式错误标志位,该位由硬件置位,软件复位。1:出现了模式错误;0:没有出现模式错误。</td><td>0</td></tr><tr><td>4</td><td>CRCERR</td><td>RWO</td><td>CRC错误标志位,该位由硬件置位,软件复位。1:收到的CRC值与RCRCR的值不一致;0:收到的CRC值与RCRCR的值一致。</td><td>0</td></tr><tr><td>3</td><td>UDR</td><td>RO</td><td>下溢标志位,该位由硬件置位,软件复位。1:发生下溢;0:未发生下溢。</td><td>0</td></tr><tr><td>2</td><td>CHSIDE</td><td>RO</td><td>声道,该位由硬件置位,软件复位。1:需要传输或接收左声道;0:需要传输或接收右声道。</td><td>0</td></tr><tr><td>1</td><td>TXE</td><td>RO</td><td>发送缓冲区为空标志位。1:发送缓冲区为空;0:发送缓冲区非空。</td><td>1</td></tr><tr><td>0</td><td>RXNE</td><td>RO</td><td>接收缓冲区非空标志位。1:接收缓冲区非空;0:接收缓冲区为空。注:读DATAR,自动清零。</td><td>0</td></tr></table>

### 20.4.4 SPI 数据寄存器（SPIx_DATAR）（x=1/2/3）

偏移地址： $0 \times 0 0$

<table><tr><td>15</td><td>14</td><td>13</td><td>12</td><td>11</td><td>10</td><td>9</td><td>8</td><td>7</td><td>6</td><td>5</td><td>4</td><td>3</td><td>2</td><td>1</td><td>0</td></tr><tr><td colspan="16">DR[15:0]</td></tr></table>

<table><tr><td>位</td><td>名称</td><td>访问</td><td>描述</td><td>复位值</td></tr><tr><td>[15:0]</td><td>DR[15:0]</td><td>RW</td><td>数据寄存器。数据寄存器用于存放接收到的数据或预存将要发送出去的数据,因此数据寄存器的读写实际上是对应操作不同的区域,其中读对应接收缓冲区,写对应发送缓冲区。数据的接收和发送可以是8位或者16位的,需要在传输之前就确定使用多少位的数据。使用8位进行数据传输时,只有数据寄存器的低8位被使用,接收时高8位强制为0。使用16位数据结构则会使全部16位数据寄存器被使用。</td><td>0</td></tr></table>

### 20.4.5 SPI 多项式寄存器（SPIx_CRCR）（x=1/2/3）

偏移地址： $0 \times 1 0$

<table><tr><td>15</td><td>14</td><td>13</td><td>12</td><td>11</td><td>10</td><td>9</td><td>8</td><td>7</td><td>6</td><td>5</td><td>4</td><td>3</td><td>2</td><td>1</td><td>0</td></tr><tr><td colspan="16">CRCPOLY[15:0]</td></tr></table>

<table><tr><td>位</td><td>名称</td><td>访问</td><td>描述</td><td>复位值</td></tr><tr><td>[15:0]</td><td>CRCPOLY[15:0]</td><td>RW</td><td>CRC多项式。此域定义CRC计算用到的多项式。</td><td>0007h</td></tr></table>

### 20.4.6 SPI 接收 CRC 寄存器（SPIx_RCRCR）（x=1/2/3）

偏移地址： $0 \times 1 4$

15 14 13 12 11 10 9 8 7 6 5 4 3 2 1 0

RXCRC[15:0]

<table><tr><td>位</td><td>名称</td><td>访问</td><td>描述</td><td>复位值</td></tr><tr><td>[15:0]</td><td>RXCRC[15:0]</td><td>R0</td><td>接收CRC值。存储着计算出来的接收到的字节的CRC校验的结果。对CRCEN置位会复位该寄存器。计算方法使用CRCPOLY用到的多项式。8位模式下只有低8位参与计算，16位模式下全部16位都会参与计算。需要在BSY为0时去读取这个寄存器。</td><td>0</td></tr></table>

### 20.4.7 发送 CRC 寄存器（SPIx_TCRCR）（x=1/2/3）

偏移地址： $0 \times 1 8$

15 14 13 12 11 10 9 8 7 6 5 4 3 2 1 0

TXCRC[15:0]

<table><tr><td>位</td><td>名称</td><td>访问</td><td>描述</td><td>复位值</td></tr><tr><td>[15:0]</td><td>TXCRC[15:0]</td><td>R0</td><td>发送CRC值。存储着计算出来的已经发送出去的字节的CRC校验的结果。对CRCEN置位会复位该寄存器。计算方法使用CRCPOLY用到的多项式。8位模式下只有低8位参与计算,16位模式下全部16位都会参与计算。需要在BSY为0时去读取这个寄存器。</td><td>0</td></tr></table>

### 20.4.8 SPI_I2S 配置寄存器（SPI_I2S_CFGR）（x=1/2/3）

偏移地址： ${ 0 } { \times 1 } { 0 }$

15 14 13 12 11 10 9 8 7 6 5 4 3 2 1 0

<table><tr><td>Reserved</td><td>12S
MOD</td><td>12SE</td><td>12SCFG
[1:0]</td><td>PCM
SYNC</td><td>Reser
ved</td><td>12SSTD
[1:0]</td><td>CKPOL</td><td>DATLEN
[1:0]</td><td>CHLEN</td></tr></table>

<table><tr><td>位</td><td>名称</td><td>访问</td><td>描述</td><td>复位值</td></tr><tr><td>[15:12]</td><td>Reserved</td><td>R0</td><td>保留</td><td>0</td></tr><tr><td>11</td><td>I2SMOD</td><td>RW</td><td>I2S模式选择,该位只有在关闭了SPI或者I2S时才能设置。1:选择I2S模式;0:选择SPI模式。</td><td>0</td></tr><tr><td>10</td><td>I2SE</td><td>RW</td><td>I2S使能,在SPI模式下不使用。1:I2S使能;0:关闭I2S。</td><td>0</td></tr><tr><td>[9:8]</td><td>I2SCFG[1:0]</td><td>RW</td><td>I2S模式选择,此该位只有在关闭了I2S时才能设置:00:从设备发送;</td><td>00b</td></tr><tr><td></td><td></td><td></td><td>01:从设备接收;10:主设备发送;11:主设备接受。</td><td></td></tr><tr><td>7</td><td>PCMSYNC</td><td>RW</td><td>PCM帧同步。该位只在I2SSTD=11(使用PCM标准)时有意义。1:长帧同步;0:短帧同步。</td><td>0</td></tr><tr><td>6</td><td>Reserved</td><td>RO</td><td>保留</td><td>0</td></tr><tr><td>[5:4]</td><td>I2SSTD[1:0]</td><td>RW</td><td>I2S标准选择,只有在关闭了I2S时才能设置该位。00:I2S飞利浦标准;01:高字节对齐标准(左对齐);10:低字节对齐标准(右对齐);11:PCM标准。</td><td>00b</td></tr><tr><td>3</td><td>CKPOL</td><td>RW</td><td>静止态时钟极性,为了正确操作,该位只有在关闭了I2S时才能设置。1:I2S时钟静止态为高电平;0:I2S时钟静止态为低电平。</td><td>0</td></tr><tr><td>[2:1]</td><td>DATLEN[1:0]</td><td>RW</td><td>待传输数据长度,为了正确操作,该位只有在关闭了I2S时才能设置。00:16位数据长度;01:24位数据长度;10:32位数据长度;11:不允许。</td><td>00b</td></tr><tr><td>0</td><td>CHLEN</td><td>RW</td><td>声道长度,只有在DATLEN=00时该位的写操作才有意义,否则声道长度都由硬件固定为32位。。1:32位宽;0:16位宽。</td><td>0</td></tr></table>

### 20.4.9 SPI_I2S 预分频寄存器（SPIx_I2SPR）（x=2/3）

偏移地址： $_ { 0 \times 2 0 }$

<table><tr><td>Reserved</td><td>MCKOE</td><td>ODD</td><td>I2SDIV[7:0]</td></tr></table>

<table><tr><td>位</td><td>名称</td><td>访问</td><td>描述</td><td>复位值</td></tr><tr><td>[15:10]</td><td>Reserved</td><td>R0</td><td>保留</td><td>0</td></tr><tr><td>9</td><td>MCKOE</td><td>RW</td><td>主设备时钟输出使能,为了正确操作,该位只有在关闭了I2S时才能设置。仅在I2S主设备模式下使用该位。1:主设备时钟输出使能;0:关闭主设备时钟输出。</td><td>0</td></tr><tr><td>8</td><td>ODD</td><td>RW</td><td>奇系数预分频,为了正确操作,该位只有在关闭了I2S时才能设置。仅在I2S主设备模式下使用该位。</td><td>0</td></tr><tr><td></td><td></td><td></td><td>1: 实际分频系数 = (I2SDIV * 2) + 1;0: 实际分频系数 = I2SDIV *2。</td><td></td></tr><tr><td>[7:0]</td><td>I2SDIV[7:0]</td><td>RW</td><td>I2S 线性预分频。为了正确操作,该位只有在关闭了 I2S 时才能设置。仅在 I2S 主设备模式下使用该位。禁止设置 I2SDIV [7:0] = 0 或者 I2SDIV [7:0] = 1参见 19.3.3 节。</td><td>0</td></tr></table>

### 20.4.10 SPI 高速控制寄存器（SPIx_HSCR）（x=1/2/3）

偏移地址： ${ 0 } { \times 2 4 }$

<table><tr><td>15</td><td>14</td><td>13</td><td>12</td><td>11</td><td>10</td><td>9</td><td>8</td><td>7</td><td>6</td><td>5</td><td>4</td><td>3</td><td>2</td><td>1</td><td>0</td></tr><tr><td colspan="15">Reserved</td><td>HSRX
EN</td></tr></table>

<table><tr><td>位</td><td>名称</td><td>访问</td><td>描述</td><td>复位值</td></tr><tr><td>[15:1]</td><td>Reserved</td><td>R0</td><td>保留</td><td>0</td></tr><tr><td>0</td><td>HSRXEN</td><td>WO</td><td>SPI高速模式下(CLK大于等于36MHz)读使能。该模式仅在时钟2分频(即CTLR1寄存器的BR=000)时有效。该位不可读。1:使能高速读模式;0:关闭高速读模式。</td><td>0</td></tr></table>


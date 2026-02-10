## CH230 CH231  
CH233 CH235  
CH236 CH237  
CH238

![](images/3077c9f26da0e485f01df5765ee222bb1381cf047fdd96a15164d2f8fda0fdcf.jpg)  
应用框图 \ Block Diagram

选型指南 \Model Selection Guide  

<table><tr><td>型号</td><td>接口支持</td><td>协议支持</td><td>内置MOS</td><td>其他特点</td><td>反馈</td><td>过流/限流</td><td>封装</td></tr><tr><td>CH230K/A</td><td>单C</td><td>PD+PPS,最高13V</td><td>/</td><td>VBUS余电泄放</td><td>FB</td><td>/</td><td>SOT23-6</td></tr><tr><td>CH231K/A</td><td>单C</td><td>PD+PPS,最高13V</td><td>/</td><td>/</td><td>FB</td><td>/</td><td>SOT23-6</td></tr><tr><td>CH233K/A</td><td>单C</td><td>PD+PPS,最高21V</td><td>/</td><td>/</td><td>FB</td><td>/</td><td>SOT23-6</td></tr><tr><td>CH233P</td><td>单C</td><td>PD+PPS,最高21V</td><td>/</td><td>多芯片组合,智能设备识别与功率分配</td><td>FB</td><td>/</td><td>QFN16</td></tr><tr><td>CH235S</td><td>单C</td><td>PD+PPS,常用A口协议,最高13V</td><td>/</td><td rowspan="4">VBUS检测与放电,可双芯组合降功率或共享5V</td><td>FB</td><td>过流</td><td>ESSOP10</td></tr><tr><td>CH236D</td><td>单C</td><td rowspan="3">PD+PPS,常用A口协议,低压大电流直充,最高20V</td><td>/</td><td>AO</td><td>限流</td><td>QFN20</td></tr><tr><td>CH237D</td><td>A+C</td><td>/</td><td>AO</td><td>限流</td><td>QFN20</td></tr><tr><td>CH238P</td><td>单C</td><td>内置NMOS</td><td>FB/AO</td><td>限流</td><td>QFN16</td></tr></table>

### 其他USB PD协议芯片 \ Others

CH226/5：USB Type-C转音频+快充方案，单芯片内嵌USB PD控制器，实现Type-C耳机接口的手机在充电的同时使用耳机。

### PCIe总线接口芯片

CH368是PCI-Express总线通用接口芯片，CH368将PCIe转换为类似于ISA的32位或8位主动并行接口，用于制作基于PCIe总线的计算机板卡，以及将原先基于ISA总线或者PCI总线的板卡升级到PCIe上。适用于高速实时的I/O控制卡、通讯接口卡、数据采集卡等。

### PCIe总线四串口/双串口

### 及打印口芯片

CH384是PCI-Express总线的四串口及打印口芯片，包含四个兼容16C550/750的异步串口和一个EPP/ECP增强型双向并口，可外加CH438芯片扩展多达28个串口。可用于PCIe总线的RS232串口扩展、带自动硬件速率控制的PCIE高速串口、串口组网、RS485通讯、IrDA通讯、并口/打印口扩展等。

![](images/c286561380aff6ce0b152b93df6dbd11f5a58c11417cfb0bdd6ced61ee91e133.jpg)  
应用框图 \ Block Diagram

#### 产品特点 \ Features

> 支持I/O端口映射、存储器映射、扩展ROM以及中断  
> 基于PCIe总线提供8位或者32位主动并行总线  
> 提供32位被动并行接口，可以挂接到其它CPU或者单片机MCU总线，支持BusMaster/DMA  
> 支持I/O读写，自动分配I/O基址，支持长度达232字节的I/O端口  
> 读写脉冲的宽度从30ns到450ns可选，32位存储器突发块存取的速度可达每秒50MB  
> 支持闪存扩展ROM无硬盘引导，可以提供扩展ROM应用的子程序库BRM  
> 提供高速的3线或者4线SPI串行主机接口  
> 提供两线串行主机接口，可以挂接类似24C0X的串口EEPROM器件用于存储非易失数据

### 其他PCI/PCIe芯片 \ Others

CH364：PCI扩展ROM控制芯片，提供Flash-ROM，用于系统安全控制卡/隔离卡等。

CH365：PCI通用接口芯片，用于I/O控制等PCI Device（Slave），8位并口，直接升级ISA卡。

CH366：PCI-Express扩展ROM控制芯片，提供Flash-ROM，用于系统安全控制卡/隔离卡等。

CH367：PCI-Express通用8位接口芯片，用于PCIe通讯卡/IO控制卡等。

### 典型应用 \Applications

工业控制

信息安全

医疗仪器

仪器仪表

![](images/31e4fcb5d0028c18cecfd4049d96053a09f6a859be822c2c731c4b5a07032ed0.jpg)  
应用框图 \ Block Diagram

#### 产品特点 \ Features

> 同一芯片可配置为PCIe总线的四通道串口加并口/打印口或者四通道串口加扩展多串口  
> 可以挂接串口EEPROM器件并设定PCIe板卡的设备标识（VendorID，DeviceID，ClassCode等）  
> 完全独立的4个异步串口，提供PCIe接口8串口、16串口、28串口等应用方案  
> 串口可编程通讯波特率，支持115200bps以及最高达8Mbps的通讯波特率  
> 串口内置256字节的FIFO先进先出缓冲器，支持4个FIFO触发级  
> 支持全双工和半双工串口通讯，串口0内置SIR红外线编解码器，支持IrDA红外通讯  
> 支持SPP、Nibble、Byte、PS/2、EPP、ECP等IEEE1284并口/打印口工作方式  
> 并口支持双向数据传输，支持最高达1M字节/秒的传输速度

### 其他PCIe芯片 \ Others

CH382：可实现PCI-E总线双串口加一并口/打印口扩展，256字节FIFO。

### 典型应用 \Applications

工业控制

金融设备

一卡通系统

医疗仪器

办公自动化


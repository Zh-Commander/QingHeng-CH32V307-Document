## CH32H417

### 青稞RISC-V双内核

### 超高速USB3.0+百兆以太网互联型MCU

CH32H417是基于青稞V5F和V3F内核的双核微控制器，提供超高速USB3.0和高速USB2.0主机/设备接口并内置收发器PHY、以太网MAC及

100M PHY、SerDes高速隔离收发器，集成SD/EMMC控制器、通用高速接口

UHSIF、数字图像接口DVP、单线协议主接口SWPMI、PIOC、灵活存储控制器

FMC、数字滤波器、LTDC、图形处理硬件加速器等丰富外设资源和高速ADC、

运放、比较器等模拟资源，内置USBPD，支持PDUSB和Type-C功率传输。

### 应用框图 \ Block Diagram

![](images/1a1875334067d82186326dda73cf8e9f2afcc47155d9367d29d886876a4a8eee.jpg)

#### 产品特点 \ Features

> 双核结构：青稞RISC-V5F和RISC-V3F  
V5F最高频率384MHz,V3F最高频率144MHz   
> 896KB SRAM,512KB Flash   
> 系统供电额定3.3V  
> 常规GPIO供电额定3.3V,支持1.8V  
> 高速GPIO供电可选1.2/1.8/2.5/3.3V  
> USB 3.0模块供电额定1.2V   
> 上电/断电复位 (POR/PDR)  
> 2组共16路通用DMA控制器  
> 2组12位模数转换ADC   
> 1组10位高速模数转换HSADC  
> 3组运放OPA/PGA/电压比较器  
> 1组模拟电压比较器CMP   
> 14个定时器  
> 16路TouchKey通道检测   
通用高速接口UHSIF  
> 数字图像接口DVP   
> SD/EMMC控制器(SDMMC)  
> SDIO主机/从机接口

> 单线协议主接口SWPMI  
可编程协议I/O控制器PIOC  
> 以太网控制器MAC及10M/100M PHY  
> 5Gbps超高速USB3.0控制器及PHY  
> 480Mbps高速USB2.0控制器及PHY  
> 远距离SerDes控制器及PHY  
> 8组USART串口、4组I²C接口、1组I³C接口  
> 4组SPI接口、2组QuadSPI接口  
> 3组CAN接口(2.0B主动)  
> 数字滤波器,用于 $\sum \Delta$ 调制器DFSDM  
串行音频接口SAI   
DCT-TFT显示控制器LTDC   
图形处理硬件加速器GPHA  
> 灵活存储控制器FMC  
> 95个10   
> ECDC加密模块   
单线(默认)和双线两种调试模式  
封装形式：QFN68、QFN88、LQFP144

#### 主要资源

#### Main Resource

<table><tr><td colspan="2">典型产品型号</td><td>CH32H417ZET6</td><td>CH32H417MEU6</td><td>CH32H417REU6</td></tr><tr><td colspan="2">内核</td><td colspan="3">RISC-V5F+RISC-V3F</td></tr><tr><td colspan="2">Flash (KB)</td><td colspan="3">512K</td></tr><tr><td colspan="2">SRAM (KB)</td><td colspan="3">896K</td></tr><tr><td colspan="2">GPIO</td><td>95</td><td>65</td><td>50</td></tr><tr><td rowspan="7">定时器</td><td>高级(16位)</td><td colspan="3">2</td></tr><tr><td>通用(16位)</td><td colspan="3">4</td></tr><tr><td>通用(32位)</td><td colspan="3">4</td></tr><tr><td>基本(16位)</td><td colspan="3">2</td></tr><tr><td>LPTIM</td><td colspan="3">2</td></tr><tr><td>WDOG</td><td colspan="3">2</td></tr><tr><td>SysTick</td><td colspan="3">2</td></tr><tr><td colspan="2">RTC</td><td colspan="3">1</td></tr><tr><td colspan="2">ADC/TouchKey(单元/通道数)</td><td>2/16</td><td>2/9</td><td>2/7</td></tr><tr><td colspan="2">HSADC(单元/通道数)</td><td>1/7</td><td>1/4</td><td>1/4</td></tr><tr><td colspan="2">DAC(单元)</td><td>2</td><td>2</td><td>DAC2</td></tr><tr><td colspan="2">OPA</td><td>3</td><td>OPA1, OPA3</td><td>OPA1, OPA3</td></tr><tr><td colspan="2">CMP</td><td colspan="3">1</td></tr><tr><td colspan="2">DFSDM</td><td colspan="3">1</td></tr><tr><td colspan="2">RNG</td><td colspan="3">1</td></tr><tr><td colspan="2">LTDC</td><td colspan="3">1</td></tr><tr><td colspan="2">GPHA</td><td colspan="3">1</td></tr><tr><td colspan="2">SDMMC</td><td colspan="3">1</td></tr><tr><td colspan="2">PIOC</td><td colspan="3">1</td></tr><tr><td rowspan="17">通信接口</td><td>DVP</td><td colspan="3">1</td></tr><tr><td>USART</td><td>8</td><td>8</td><td>7</td></tr><tr><td>SPI/I²S</td><td>4/2</td><td>4/2</td><td>3/2</td></tr><tr><td>QUADSPI</td><td>2</td><td>QUADSPI2</td><td>QUADSPI2</td></tr><tr><td>I²C</td><td colspan="3">4</td></tr><tr><td>I³C</td><td colspan="3">1</td></tr><tr><td>UHSIF</td><td colspan="3">1</td></tr><tr><td>CAN</td><td colspan="3">3</td></tr><tr><td>SDIO</td><td colspan="3">1</td></tr><tr><td>SAI</td><td colspan="3">1</td></tr><tr><td>SWPMI</td><td colspan="3">1</td></tr><tr><td>USBFS/OTG_FS</td><td colspan="2">1</td><td>PDU</td></tr><tr><td>USBHS</td><td colspan="3">1</td></tr><tr><td>USBSS</td><td colspan="3">1</td></tr><tr><td>USB PD和Type-C</td><td colspan="2">Source/Sink/DRP</td><td>-</td></tr><tr><td>SerDes</td><td>1</td><td>1</td><td>-</td></tr><tr><td>Ethernet</td><td colspan="3">MAC+10/100M PHY</td></tr><tr><td rowspan="2">FMC</td><td>FSMC</td><td colspan="3">1</td></tr><tr><td>SDRAM</td><td colspan="3">1</td></tr><tr><td colspan="2">封装形式</td><td>LQFP144</td><td>QFN88</td><td>QFN68</td></tr></table>

注：更多型号请参考MCU选型表


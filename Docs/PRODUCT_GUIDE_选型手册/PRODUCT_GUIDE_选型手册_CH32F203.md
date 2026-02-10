## CH32F203

### 青稞RISC-V/Cortex-M3内核

### 大容量通用型MCU

#### 8个串口, 10组定时器

CH32V303是基于青稞V4F内核的工业级通用微控制器，144MHz主频，内置4组运放比较器可配合ADC和定时器实现信号放大采样及比较输出。芯片提供USB2.0全速主机/设备接口、CAN、SDIO、FSMC等专用接口，可满足工业、医疗、消费类等多场景应用需求。

CH32V203基于青稞V4B内核，提供双USB、CAN、运放、ADC和触摸功能。

### 应用框图 \ Block Diagram

![](images/401441f3ca8b875928f90be2bc787beecc7249ffc59adcb0a9678035d60deee6.jpg)

#### 产品特点 \ Features

> RISC-V4F处理器,最高144MHz系统主频  
支持单周期乘法和硬件除法  
支持硬件浮点运算  
> 64KB SRAM, 256KB Flash   
> GPIO单元独立供电，可不同步系统供电  
> 多种低功耗模式：睡眠/停止/待机  
> 上电/断电复位(POR/PDR)  
> 可编程电压监测器(PVD)  
> 2组18路DMA控制器  
> 4组运放、比较器  
> 1个随机数发生器TRNG   
> 2组12位DAC数模转换   
> 16路TouchKey通道检测

> 16路12位ADC转换通道  
> 10个定时器  
> 1个USB2.0 FS主机/设备接口  
> 1个CAN接口(2.0B主动)  
> SDIO主机接口  
> FSMC存储器接口  
> 2个I²C接口  
> 3个USART和5个UART   
> 3个SPI接口(支持Master和Slave模式)  
> 80个I/O,所有IO都可映射到16个外部中断  
CRC计算单元,96位芯片唯一ID  
> 串行2线调试接口SDI   
封装形式：QFN48、LQFP48、LQFP64M、LQFP100

#### 主要资源 \Main Resource

<table><tr><td colspan="2">典型产品型号</td><td>CH32V303VCT6</td><td>CH32V303RCT6</td><td>CH32F203VCT6</td><td>CH32F203RCT6</td></tr><tr><td colspan="2">内核</td><td colspan="2">RISC-V (FPU)</td><td colspan="2">Cortex-M3</td></tr><tr><td colspan="2">Flash (KB)</td><td colspan="4">256</td></tr><tr><td colspan="2">SRAM (KB)</td><td colspan="4">64</td></tr><tr><td colspan="2">GPIO</td><td>80</td><td>51</td><td>80</td><td>51</td></tr><tr><td rowspan="5">定时器</td><td>高级(16位)</td><td colspan="4">4</td></tr><tr><td>通用(16位)</td><td colspan="4">4</td></tr><tr><td>基本(16位)</td><td colspan="4">2</td></tr><tr><td>WDOG</td><td colspan="4">2</td></tr><tr><td>SysTick</td><td colspan="4">1</td></tr><tr><td colspan="2">RTC</td><td colspan="4">1</td></tr><tr><td colspan="2">ADC/TouchKey (单元/通道数)</td><td colspan="4">2/16</td></tr><tr><td colspan="2">DAC (单元)</td><td colspan="4">2</td></tr><tr><td colspan="2">运放、比较器</td><td colspan="4">4</td></tr><tr><td colspan="2">随机数发生器</td><td colspan="4">1</td></tr><tr><td rowspan="8">通信接口</td><td>U (S) ART</td><td colspan="4">8</td></tr><tr><td>SPI</td><td colspan="4">3</td></tr><tr><td>I²S</td><td colspan="4">2</td></tr><tr><td>I²C</td><td colspan="4">2</td></tr><tr><td>CAN</td><td colspan="4">1</td></tr><tr><td>SDIO</td><td colspan="4">1</td></tr><tr><td>USB (FS)</td><td colspan="2">Host/Device</td><td colspan="2">Device</td></tr><tr><td>FSMC</td><td>1</td><td>-</td><td>1</td><td>-</td></tr><tr><td colspan="2">主频 (MHz)</td><td colspan="4">144</td></tr><tr><td colspan="2">VDD(V)</td><td colspan="4">2.5/3.3</td></tr><tr><td colspan="2">封装</td><td>LQFP100</td><td>LQFP64M</td><td>LQFP100</td><td>LQFP64M</td></tr></table>

注：更多型号请参考MCU选型表

### 典型应用 \Applications

![](images/b04a53edcfb75f8f23c717cf907f5bc0d9671346261cdbc9ac9a4eb5c7d6c194.jpg)  
工业控制

![](images/df9ddf3ae0914ead473659b0ac51a230c0614f27dc50b8b6a551a600240e7209.jpg)  
健康医疗

![](images/b22c7892abbe7ad37a38723d9c60cac7dad1505288c8c6b6a48a86ef53dfd4c5.jpg)  
安防监控

![](images/61ae569e1a9c3e33db8b564fa3f74271b53008acd66e44b12c912fde71ae3ddc.jpg)  
消费电子


## CH32V317  
CH32V307  
CH32V305  
CH32F207  
CH32F205

### 青稞RISC-V/Cortex-M3内核

### 高速互联型MCU

#### 内置高速USB和以太网收发器

CH32V317是基于青稞V4F浮点型内核的工业级通用微控制器，144MHz主频，提供480Mbps高速USB2.0主机/设备接口并集成PHY、USB2.0全速OTG接口、百兆以太网内置PHY、数字图像接口、2组CAN控制器、4组运放比较器等，常规外设数量提升，适合多采集、多通讯方向的综合类应用场景。

#### 应用框图 \ Block Diagram

![](images/82923e8afe337bf017ff5a4f53ff54339b6eabc66ae4d0e81b5f5e03a651fa06.jpg)

#### 产品特点 \ Features

> RISC-V4F处理器,最高144MHz系统主频  
> 支持单周期乘法和硬件除法  
> 支持硬件浮点运算  
> 64KB SRAM,256KB Flash   
> GPIO单元独立供电,可不同步系统供电  
> 多种低功耗模式：睡眠/停止/待机  
> 上电/断电复位(POR/PDR)  
> 2组18路通道DMA控制器  
> 4组运放、比较器  
> 1个随机数发生器TRNG   
> 2组12位DAC数模转换   
> 16路TouchKey通道检测   
> 2单元16路12位ADC转换  
> 10个定时器

> USB2.0全速OTG接口  
> USB2.0高速480Mbps主机/设备接口(内置PHY)   
> 以太网MAC控制器及10/100M PHY  
> 2组CAN接口(2.0B主动)  
> 2个I²C接口  
> 3个USART和5个UART   
> 3个SPI接口(支持Master和Slave模式)  
> SDIO主机接口  
> FSMC存储器接口  
> 数字图像接口DVP   
> 80个I/O,所有IO口都可以映射到16个外部中断  
CRC计算单元,96位芯片唯一ID  
>串行2线调试接口SDI   
封装形式：LQFP64M、LQFP100

#### 主要资源

#### Main Resource

<table><tr><td colspan="2">典型产品型号</td><td>CH32V317VCT6</td><td>CH32V307VCT6</td><td>CH32V305RBT6</td><td>CH32F207VCT6</td><td>CH32F205RBT6</td></tr><tr><td colspan="2">内核</td><td colspan="3">RISC-V (FPU)</td><td colspan="2">Cortex-M3</td></tr><tr><td colspan="2">Flash (KB)</td><td colspan="2">256</td><td>128</td><td>256</td><td>128</td></tr><tr><td colspan="2">SRAM (KB)</td><td colspan="2">64</td><td>32</td><td>64</td><td>32</td></tr><tr><td colspan="2">GPIO</td><td>70</td><td>80</td><td>51</td><td>80</td><td>51</td></tr><tr><td rowspan="5">定时器</td><td>高级(16位)</td><td colspan="5">4</td></tr><tr><td>通用(16位)</td><td colspan="5">4</td></tr><tr><td>基本(16位)</td><td colspan="5">2</td></tr><tr><td>WDOG</td><td colspan="5">2</td></tr><tr><td>SysTick</td><td colspan="5">1</td></tr><tr><td colspan="2">RTC</td><td colspan="5">1</td></tr><tr><td colspan="2">ADC/TouchKey(单元/通道数)</td><td colspan="5">2/16</td></tr><tr><td colspan="2">DAC(单元)</td><td colspan="5">2</td></tr><tr><td colspan="2">运放、比较器</td><td colspan="5">4</td></tr><tr><td colspan="2">随机数发生器</td><td colspan="5">1</td></tr><tr><td rowspan="11">通信接口</td><td>U(S)ART</td><td colspan="2">8</td><td>5</td><td>8</td><td>5</td></tr><tr><td>SPI</td><td colspan="5">3</td></tr><tr><td>I²S</td><td colspan="5">2</td></tr><tr><td>I²C</td><td colspan="5">2</td></tr><tr><td>CAN</td><td colspan="5">2</td></tr><tr><td>SDIO</td><td colspan="5">1</td></tr><tr><td>DVP</td><td colspan="2">1</td><td>-</td><td>1</td><td>-</td></tr><tr><td>USB (FS)</td><td colspan="5">OTG</td></tr><tr><td>USB (HS)</td><td colspan="5">Host/Device (480Mbps)</td></tr><tr><td>Ethernet</td><td>MAC+10/100 PHY</td><td>1G MAC+10M PHY</td><td>-</td><td>1G MAC+10M PHY</td><td>-</td></tr><tr><td>FSMC</td><td>-</td><td>1</td><td>-</td><td>1</td><td>-</td></tr><tr><td colspan="2">主频(MHz)</td><td colspan="5">144</td></tr><tr><td colspan="2">VDD(V)</td><td colspan="5">2.5/3.3</td></tr><tr><td colspan="2">封装</td><td>LQFP100</td><td>LQFP100</td><td>LQFP64M</td><td>LQFP100</td><td>LQFP64M</td></tr></table>

注:更多型号请参考MCU选型表

### 典型应用 \Applications

![](images/9fc7a3e1410ccf909a00c4f31fcc02b283da5816a56c787f6a4731449525da1b.jpg)

工业控制

![](images/13b75c7e26c06549c859dc7d17cd6257918eb373bc2e19ded8062865fce6d7bb.jpg)  
物联网

![](images/906e4c019901ebdaaeb05cc076a93751abc27bfe67074e94b6b464963b8ec4de.jpg)  
健康医疗

![](images/a64a2af3293b3f4290c0bb700e24ea00c1295b967bc8b81f2dfb1b0e697cb12d.jpg)  
消费电子  
工业控制


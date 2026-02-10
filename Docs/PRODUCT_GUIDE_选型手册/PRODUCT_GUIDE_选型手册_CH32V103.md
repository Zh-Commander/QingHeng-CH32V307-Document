## CH32V103  
CH32F103

### 青棵RISC-V/Cortex-M3内核3.3V/5V额定电压通用型MCU

CH32V103是基于青稞V3A内核的工业级通用微控制器，80MHz主频，提供USB2.0主机/设备接口、12位ADC、多通道触摸按键和常规外设。

### 应用框图 \ Block Diagram

![](images/626ee11e45e041e6d768834dfc16138bb2e35751a5f34dcc47b67ccfabc0553c.jpg)

#### 产品特点 \ Features

> RISC-V3A处理器,最高80MHz系统主频  
> 支持单周期乘法和硬件除法  
> 20KB SRAM,64KB Flash   
> 供电范围：2.7V-5.5V，GPIO同步供电电压  
> 多种低功耗模式：睡眠/停止/待机  
> 上电/断电复位(POR/PDR)  
> 可编程电压监测器(PVD)  
> 7通道DMA控制器  
> 16路TouchKey通道检测   
> 16路12位ADC转换通道

> 7个定时器  
> 1个USB2.0主机/设备接口(全速和低速)  
> 2个I²C接口（支持SMBus/PMBus）  
> 3个USART接口  
> 2个SPI接口(支持Master和Slave模式)  
> 51个I/O口,所有的I/O口都可以映射到16个外部中断  
CRC计算单元，96位芯片唯一ID   
串行2线调试接口SDI   
封装形式：LQFP64M、LQFP48、QFN48X7

#### 主要资源 \Main Resource

<table><tr><td colspan="2">典型产品型号</td><td>CH32V103R8T6</td><td>CH32V103C8T6</td><td>CH32F103R8T6</td><td>CH32F103C8T6</td></tr><tr><td colspan="2">内核</td><td colspan="2">RISC-V</td><td colspan="2">Cortex-M3</td></tr><tr><td colspan="2">Flash (KB)</td><td colspan="4">64</td></tr><tr><td colspan="2">SRAM (KB)</td><td colspan="4">20</td></tr><tr><td colspan="2">GPIO</td><td>51</td><td>37</td><td>51</td><td>37</td></tr><tr><td rowspan="4">定时器</td><td>高级(16位)</td><td colspan="4">1</td></tr><tr><td>通用(16位)</td><td colspan="4">3</td></tr><tr><td>WDOG</td><td colspan="4">2</td></tr><tr><td>SysTick</td><td colspan="4">1</td></tr><tr><td colspan="2">RTC</td><td colspan="4">1</td></tr><tr><td colspan="2">ADC/TouchKey(单元/通道数)</td><td>1/16</td><td>1/10</td><td>1/16</td><td>1/10</td></tr><tr><td colspan="2">DAC(单元)</td><td>-</td><td>-</td><td>1</td><td>1</td></tr><tr><td rowspan="5">通信接口</td><td>U(S)ART</td><td colspan="4">3</td></tr><tr><td>SPI</td><td colspan="4">2</td></tr><tr><td>I²C</td><td colspan="4">2</td></tr><tr><td>CAN</td><td>-</td><td>-</td><td>1</td><td>1</td></tr><tr><td>USB (FS)</td><td>Host/Device</td><td>Host/Device</td><td>Device+Host/Device</td><td>Device+Host/Device</td></tr><tr><td colspan="2">主频(MHz)</td><td colspan="2">80</td><td colspan="2">72</td></tr><tr><td colspan="2">VDD(V)</td><td colspan="4">3.3/5.0</td></tr><tr><td colspan="2">封装</td><td>LQFP64M</td><td>LQFP48</td><td>LQFP64M</td><td>LQFP48</td></tr></table>

注：更多型号请参考MCU选型表

### 典型应用 Applications

![](images/7fbbd06c3cd838c2bb9f4699d687c0de6084e2a28bbe65249f09557506001c59.jpg)  
工业控制

![](images/8673ea9cafaade32227cf6be0e8d0333a6e7bbc4eb3ae594b1143e0b570b149a.jpg)  
健康医疗

![](images/375baf260d48068bbe34364a95db2ccde2b2414844cb238f1a08a7631d6d56f7.jpg)  
安防监控

![](images/2563d75a6f19b127a4ed65773817d80241863c349441ef3e6d6415e1dd950e50.jpg)  
消费电子


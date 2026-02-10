## CH32X035

### 青稞RISC-V内核USB通讯和PD电源双功能Type-C接口MCU

CH32X035是基于青棵V4C内核的工业级微控制器，48MHz主频，提供USB2.0全速主机/设备接口，内置PD PHY，支持PDUSB及Type-C功率传输，内置可编程协议I/O控制器，提供多组运放和电压比较器、12位ADC、触摸按键和常规外设。

### 应用框图 \ Block Diagram

![](images/66c1cda54f5cbe9d863497d2e17f34f969d2a7ce1392f9df4d4dec352085219c.jpg)

#### 产品特点 \ Features

> RISC-V4C处理器, 最高48MHz  
> 支持单周期乘法和硬件除法  
> 20KB SRAM, 62KB Flash   
> 多种低功耗模式：睡眠/停止/待机  
> 上/下电复位、可编程电压监测器  
>8路通用DMA控制器  
> 可编程协议I/O控制器PIOC  
>2组运放OPA/PGA/电压比较器  
> 3组模拟电压比较器CMP   
多路外部12位ADC转换通道  
> 多路TouchKey通道检测  
>2个16位高级定时器

> 1个16位通用定时器  
> 2个看门狗定时器(独立和窗口)  
> 1个系统时基定时器  
> 4组USART串口:支持LIN和ISO7816  
> 1个I²C接口：支持SMBus/PMBus  
> 1个SPI接口  
> USB2.0全速控制器及PHY  
> USB PD和Type-C控制器及PHY  
> 快速GPIO端口，支持24个外部中断  
> 96位芯片唯一ID   
> 串行2线调试接口SDI   
封装形式：LQFP64M、LQFP48、QFN28、QSOP28、QFN20、TSSOP20

#### 主要资源 \Main Resource

<table><tr><td colspan="2">典型产品型号</td><td>CH32X035R8T6</td><td>CH32X035C8T6</td><td>CH32X035G8U6</td><td>CH32X035G8R6</td><td>CH32X035F8U6</td><td>CH32X035F7P6</td><td>CH32X033F8P6</td></tr><tr><td colspan="2">内核</td><td colspan="7">RISC-V</td></tr><tr><td colspan="2">Flash (KB)</td><td colspan="7">62</td></tr><tr><td colspan="2">SRAM (KB)</td><td colspan="7">20</td></tr><tr><td colspan="2">GPIO</td><td>60</td><td>46</td><td>27</td><td>26</td><td>19</td><td>18</td><td>18</td></tr><tr><td rowspan="4">定时器</td><td>高级(16位)</td><td colspan="7">2</td></tr><tr><td>通用(16位)</td><td colspan="7">1</td></tr><tr><td>WDOG</td><td colspan="7">2</td></tr><tr><td>SysTick</td><td colspan="7">1</td></tr><tr><td colspan="2">ADC/TouchKey(单元/通道数)</td><td>1/14</td><td>1/10</td><td>1/12</td><td>1/11</td><td>1/10</td><td>1/11</td><td>1/10</td></tr><tr><td colspan="2">OPA运放(组)</td><td colspan="5">2</td><td>1</td><td>2</td></tr><tr><td colspan="2">CMP比较器(组)</td><td>3</td><td>3</td><td>1</td><td>3</td><td>-</td><td>1</td><td>2</td></tr><tr><td colspan="2">PIOC</td><td colspan="7">1</td></tr><tr><td rowspan="5">通信接口</td><td>U(S)ART</td><td colspan="4">4</td><td colspan="2">3</td><td>4</td></tr><tr><td>SPI</td><td colspan="7">1</td></tr><tr><td>I²C</td><td colspan="7">1</td></tr><tr><td>USB(FS)</td><td colspan="4">Host/Device</td><td colspan="2">Device</td><td>PDU</td></tr><tr><td>USB PD和Type-C</td><td colspan="6">Source/Sink/DRP</td><td>-</td></tr><tr><td colspan="2">主频(MHz)</td><td colspan="7">48</td></tr><tr><td colspan="2">VDD(V)</td><td colspan="7">3.3/5.0</td></tr><tr><td colspan="2">封装</td><td>LQFP64M</td><td>LQFP48</td><td>QFN28</td><td>QSOP28</td><td>QFN20</td><td>TSSOP20</td><td>TSSOP20</td></tr></table>

注：更多型号请参考MCU选型表

### 典型应用 \Applications

![](images/f2523825edcc656be9a1da2b2ff5bb7e4369cd5428fd1e7f5c35dfc79463695b.jpg)  
工业设备

![](images/bfbcf366fe53017c92f7dc6d989e08bebb0b9c6b510c96e8988e337d353ce5eb.jpg)  
PD充电

![](images/5ee78d03706a7179c6b10bec8c7bcd5f93ffbc35ce9aabc22a38f0a3eea4cf6d.jpg)  
消费电子

![](images/04097a11acd1fabe7a439736970992184698fb6bf538de0fd8c51dc9bf4db68e.jpg)  
计算机与手机周边


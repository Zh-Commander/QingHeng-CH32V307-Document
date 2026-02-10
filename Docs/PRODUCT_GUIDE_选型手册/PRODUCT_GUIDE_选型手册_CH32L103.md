## CH32L103

### 青稞RISC-V内核

### PDUSB低功耗MCU

CH32L103系列是基于青棵V4C内核的工业级低功耗通用微控制器,96MHz主频,提供USB2.0全速主机/设备接口,内置PDPHY,支持PDUSB及Type-C功率传输,内置低功耗定时器,提供运放、电压比较器、12位ADC、触摸按键、CAN等丰富外设。

### 应用框图 \ Block Diagram

![](images/d5af7d3b2bdca875e4ecf4948a807327336f4ea5f82bf6e86610bfbcf63b17c4.jpg)

#### 产品特点 \ Features

> RISC-V4C处理器,系统主频最高96MHz > 1个32位通用定时器  
> 支持单周期乘法和硬件除法 > 2个看门狗定时器(独立和窗口)  
> 20KB SRAM,64KB Flash > 1个系统时基定时器  
> 多种低功耗模式：睡眠/停止/待机 > 4组USART串口：支持LIN和ISO7816  
> 上/下电复位、可编程电压监测器 > 2个I2C接口：支持SMBus/PMBus  
> 8路通用DMA控制器 > 2个SPI接口  
> 1组运放OPA/PGA/电压比较器  
> 3组模拟电压比较器CMP   
> 10路外部12位ADC转换通道  
> 10路TouchKey通道检测   
> 16位低功耗定时器  
> 1个16位高级定时器  
> 2个16位通用定时器

#### 主要资源 \Main Resource

<table><tr><td colspan="2">典型产品型号</td><td>CH32L103C8T6</td><td>CH32L103K8U6</td><td>CH32L103G8R6</td><td>CH32L103F8U6</td><td>CH32L103F8P6</td></tr><tr><td colspan="2">内核</td><td colspan="5">RISC-V</td></tr><tr><td colspan="2">Flash (KB)</td><td colspan="5">64K</td></tr><tr><td colspan="2">SRAM (KB)</td><td colspan="5">20K</td></tr><tr><td colspan="2">GPIO</td><td>37</td><td>31</td><td>26</td><td>19</td><td>16</td></tr><tr><td rowspan="6">定时器</td><td>高级(16位)</td><td colspan="5">1</td></tr><tr><td>通用(16位)</td><td colspan="5">2</td></tr><tr><td>通用(32位)</td><td colspan="5">1</td></tr><tr><td>低功耗(LPTIM)</td><td colspan="5">1</td></tr><tr><td>WDOG</td><td colspan="5">2</td></tr><tr><td>SysTick</td><td colspan="5">1</td></tr><tr><td colspan="2">RTC</td><td colspan="5">1</td></tr><tr><td colspan="2">ADC/TouchKey(单元/通道数)</td><td colspan="4">1/10</td><td>1/9</td></tr><tr><td colspan="2">OPA</td><td colspan="5">1</td></tr><tr><td colspan="2">CMP</td><td>3</td><td>3</td><td>3</td><td>3</td><td>2</td></tr><tr><td rowspan="6">通信接口</td><td>U(S)ART</td><td colspan="5">4</td></tr><tr><td>SPI</td><td>2</td><td>1</td><td>2</td><td>2</td><td>1</td></tr><tr><td>I²C</td><td>2</td><td>1</td><td>2</td><td>2</td><td>1</td></tr><tr><td>CAN</td><td colspan="4">1</td><td>PDUSB</td></tr><tr><td>USB(FS)</td><td colspan="4">Host/Device</td><td>Device</td></tr><tr><td>USB PD和Type-C</td><td colspan="5">DRP/Source/Sink</td></tr><tr><td colspan="2">主频(MHz)</td><td colspan="5">96</td></tr><tr><td colspan="2">VDD(V)</td><td colspan="5">3.3</td></tr><tr><td colspan="2">封装</td><td>LQFP48</td><td>QFN32</td><td>QSOP28</td><td>QFN20</td><td>TSSOP20</td></tr></table>

注：更多型号请参考MCU选型表

#### 典型应用

#### Applications

![](images/69d0c7303706145424c4e844bd345388dada3644133d00ec40ab18285d7b8e93.jpg)  
工业设备

![](images/63c19e0814698f1cc0c9fda27754be525cdc198c51c2b8e591357899cd64024b.jpg)  
PD充电

![](images/3f31727c45e4c6c5bb75f34853dec30d328a57ce0dc4f73fd064180ba0c52252.jpg)  
消费电子

![](images/b05f528e4fa567e54b2f4ebb2e8995a50165426efef2286c06f2e9f6e361ee30.jpg)  
计算机与手机周边


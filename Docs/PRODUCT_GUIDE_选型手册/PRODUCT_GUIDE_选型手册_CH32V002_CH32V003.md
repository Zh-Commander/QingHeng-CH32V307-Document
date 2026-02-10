## CH32V002 CH32V003

### 青稞RISC-V内核16K闪存2-5V宽电压超值型MCU

CH32V002是基于青棵V2C内核的工业级通用微控制器，48MHz主频，具有2～5V宽电压、低功耗、单线调试等特点。内置12位ADC支持3M采样率；提供DMA、定时器、串口、I²C、SPI等外设资源，封装小至2*2mm。CH32V003基于青棵V2A内核，3.3V/5V电源，支持单线调试。

### 应用框图 \ Block Diagram

![](images/1bd23658e71e9a94dc1bdf9a519fa8736b9e688e9aab32f677908caa40986e95.jpg)

#### 产品特点 \ Features

> 青稞RISC-V2C处理器, 支持2级中断嵌套  
>最高48MHz系统主频  
> 4KB SRAM,16KB Flash   
>供电电压：2.5/3.3/5V  
> 低功耗模式：睡眠、待机  
> 上/下电复位、可编程电压监测器  
>7路通用DMA控制器  
> 12位ADC,8路外部通道,支持3M采样率  
> 1个16位高级定时器、1个16位通用定时器  
> 2个看门狗定时器(独立和窗口)、1个系统时基定时器  
> 1组USART接口、1个I²C接口、1个SPI接口  
> 18个I/O口，映射1个外部中断   
> 芯片唯一ID   
> 串行单线调试接口  
封装形式：TSSOP20、QFN20、SOP16、QFN12、SOP8

#### 主要资源 \Main Resource

<table><tr><td colspan="2">典型产品型号</td><td>CH32V002
F4P6</td><td>CH32V002
F4U6</td><td>CH32V002
A4M6</td><td>CH32V002
D4U6</td><td>CH32V002
J4M6</td></tr><tr><td colspan="2">内核</td><td colspan="5">RISC-V</td></tr><tr><td colspan="2">Flash (KB)</td><td colspan="5">16</td></tr><tr><td colspan="2">SRAM (KB)</td><td colspan="5">20</td></tr><tr><td colspan="2">GPIO</td><td>18</td><td>18</td><td>14</td><td>11</td><td>6</td></tr><tr><td rowspan="4">定时器</td><td>高级</td><td colspan="5">1</td></tr><tr><td>通用</td><td colspan="5">1</td></tr><tr><td>WDOG</td><td colspan="3">2</td><td colspan="2">-</td></tr><tr><td>SysTick</td><td colspan="5">1</td></tr><tr><td colspan="2">ADC/TouchKey
(单元/通道数)</td><td>1/8</td><td>1/8</td><td>1/6</td><td>1/4</td><td>1/6</td></tr><tr><td rowspan="3">通信接口</td><td>U (S) ART</td><td colspan="5">1</td></tr><tr><td>SPI</td><td colspan="3">1</td><td colspan="2">-</td></tr><tr><td>I²C</td><td colspan="5">1</td></tr><tr><td colspan="2">主频 (MHz)</td><td colspan="5">48</td></tr><tr><td colspan="2">VDD(V)</td><td colspan="5">2-5</td></tr><tr><td colspan="2">封装</td><td>TSSOP20</td><td>QFN20</td><td>SOP16</td><td>QSOP12</td><td>SOP8</td></tr></table>

注：更多型号请参考MCU选型表

### 典型应用 \Applications

![](images/d58647bfe2a945e57453dcf3f04cfd88bb90bc05fa2efe7644d2a7365ba2da63.jpg)  
工业控制

![](images/ea64812d5740a6551a4df7725e72423e971179065f45f55358f580d7ef4c2954.jpg)  
健康医疗

![](images/1b2bbbbe1275d07568fa6abc25d95c0025f6253407141044a714e7b5e2468a1a.jpg)  
计算机与手机周边

![](images/587a892751df855304d7aa9916607665139223af7b1b1dc5fc041ac4e227c635.jpg)  
消费电子


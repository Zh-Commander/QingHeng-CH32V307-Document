## CH32V006  
CH32V005

### 青稞RISC-V内核

### 64K闪存2-5V宽电压超值型MCU

CH32V006系列是基于青稞V2C内核的工业级通用微控制器，支持48MHz系统主频，具有宽压、低功耗、单线调试等特点。内置12位ADC支持3M采样率，P端可轮询OPA支持高压摆率高速模式，提供触摸按键等外设资源。

### 应用框图 \ Block Diagram

![](images/65e7a184c4e409336ad2bb5c9abda51ea7e5278c4455371c5821b9702bce3f1c.jpg)

#### 产品特点 \ Features

> 青稞RISC-V2C内核,支持2级中断嵌套  
>最高48MHz系统主频  
>8KBSRAM,62KBFlash   
> 宽电源电压：2~5V  
>低功耗模式：睡眠、待机  
> 上/下电复位、可编程电压监测器  
>7路通用DMA控制器  
> 1组运放，P端支持3通道轮询，支持高速模式，多档增益可选  
> 12位ADC,8路外部通道,支持3M采样率  
> 1个16位高级定时器、1个16位通用定时器、1个16位精简定时器  
> 2个看门狗定时器(独立和窗口)、1个系统时基定时器  
> 2个USART串口、1个I²C接口、1个SPI接口  
>96位芯片唯一ID   
> 支持单线/双线两种调试模式  
封装形式：QFN32、QSOP24、QFN20、TSSOP20、QFN12

#### 主要资源 \Main Resource

<table><tr><td colspan="2">典型产品型号</td><td>CH32V006K8U6</td><td>CH32V006E8R6</td><td>CH32V006F8U6</td><td>CH32V006F8P6</td><td>CH32V006F4U6</td><td>CH32V005E6R6</td><td>CH32V005F6U6</td><td>CH32V005F6P6</td><td>CH32V005D6U6</td></tr><tr><td colspan="2">内核</td><td colspan="9">RISC-V</td></tr><tr><td colspan="2">Flash (KB)</td><td colspan="4">62</td><td>16</td><td colspan="4">32</td></tr><tr><td colspan="2">SRAM (KB)</td><td colspan="4">8</td><td>4</td><td colspan="4">6</td></tr><tr><td colspan="2">GPIO</td><td>31</td><td>22</td><td>18</td><td>18</td><td>18</td><td>22</td><td>18</td><td>11</td><td>11</td></tr><tr><td rowspan="5">定时器</td><td>高级(16位)</td><td colspan="9">1</td></tr><tr><td>通用(16位)</td><td colspan="9">1</td></tr><tr><td>精简(16位)</td><td colspan="5">1</td><td colspan="4">-</td></tr><tr><td>WDOG</td><td colspan="9">2</td></tr><tr><td>SysTick</td><td colspan="9">1</td></tr><tr><td colspan="2">ADC/TouchKey(单元/通道数)</td><td colspan="5">1/8</td><td colspan="3">1/8(无TouchKey)</td><td>1/4(无TouchKey)</td></tr><tr><td colspan="2">OPA运放</td><td colspan="4">1</td><td>-</td><td colspan="4">1</td></tr><tr><td rowspan="3">通信接口</td><td>U (S) ART</td><td colspan="4">2</td><td>1</td><td colspan="4">2</td></tr><tr><td>SPI</td><td colspan="9">1</td></tr><tr><td>I²C</td><td colspan="9">1</td></tr><tr><td colspan="2">主频(MHz)</td><td colspan="9">48</td></tr><tr><td colspan="2">VDD(V)</td><td colspan="9">2~5</td></tr><tr><td colspan="2">封装</td><td>QFN32</td><td>QSOP24</td><td>QFN20</td><td>TSSOP20</td><td>QFN20</td><td>QSOP24</td><td>QFN20</td><td>TSSOP20</td><td>QFN12</td></tr></table>

注：更多型号请参考MCU选型表

### 典型应用 \ Applications

![](images/3ca55262ac576881892b69d3fa46422ce11e88ca02478395e07356b1b7e5e506.jpg)  
工业控制

![](images/e02fb596a47c4ba42013c0c97b0103d7476b1e11f0891fdbbaecdbd7480e824d.jpg)  
健康医疗

![](images/8f0545d4a17184ded93084be672f445f4c061bc1a9380a001362e1b00f4e0f2f.jpg)  
消费电子

![](images/7f0a49fcc39f23fba9acfe56ae24b583e55dd9cc470c9dce3914bc0098a53600.jpg)  
计算机与手机周边


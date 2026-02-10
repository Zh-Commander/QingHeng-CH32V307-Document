## CH32M007 CH32V007

### 青稞RISC-V内核

### 24V PN或48V双N预驱电机控制MCU

CH32M007是基于青棵V2C内核的工业级微控制器，48MHz主频，单线调试。内置12位ADC支持3M采样率，运放支持3通道轮询和高速模式，提供2组电压比较器，可用于BLDC/PMSM、有感/无感、单/双电阻等多种电机控制方案。

CH32M007G8R6内置48V三相双N预驱和自举二极管及高压LDO。

CH32M007E8内置24V三相P+N预驱和高压LDO。

### 应用框图 \ Block Diagram

![](images/8619d5a0792438119d3c3eeef21f041cd7649fdd606c7668f1e47a6726560a9a.jpg)

#### 产品特点 \ Features

> 青稞RISC-V2C处理器,支持2级中断嵌套  
>最高48MHz系统主频  
>8KBSRAM,62KBFlash   
>CH32V007支持额定2.5～5V供电  
> CH32M007G8R6支持额定6～48V供电  
> CH32M007E8支持额定6～24V供电  
>低功耗模式：睡眠/待机  
> 三相半桥驱动器

CH32M007G8R6内置48V双N预驱和二极管

CH32M007E8内置24V三相P+N预驱

内置死区控制，防止高侧低侧功率管直通内置欠压保护

> 上/下电复位、可编程电压监测器  
>7路通用DMA控制器  
> 运放OPA/PGA/电压比较器

多路输入通道,可选多档增益

2路输出通道，可选ADC引脚

支持3通道轮询,支持单或双电阻方案

支持高速模式以提高压摆率

2组模拟电压比较器CMP，支持3路比较器轮询检测定位

> 12位ADC,7路外部通道  
> 1个16位高级定时器  
> 1个16位通用定时器  
> 1个16位的精简定时器  
> 2个看门狗定时器(独立和窗口)  
> 1个系统时基定时器  
> 2组USART串口:支持LIN  
> I²C接口, SPI接口  
> 16个I/O口  
> 96位芯片唯一ID   
单线或双线两种调试模式  
封装形式:QSOP24、QFN26C3、QSOP28

#### 主要资源

#### Main Resource

<table><tr><td colspan="2">典型产品型号</td><td>CH32M007E8R6</td><td>CH32M007E8U6</td><td>CH32M007G8R6</td><td>CH32V007E8R6</td><td>CH32V007K8U6</td></tr><tr><td colspan="2">内核</td><td colspan="5">RISC-V</td></tr><tr><td colspan="2">Flash (KB)</td><td colspan="5">62</td></tr><tr><td colspan="2">SRAM (KB)</td><td colspan="5">8</td></tr><tr><td colspan="2">GPIO</td><td>15</td><td>16</td><td>12</td><td>22</td><td>31</td></tr><tr><td rowspan="3">定时器</td><td>高级</td><td colspan="5">1</td></tr><tr><td>通用</td><td colspan="5">1</td></tr><tr><td>精简</td><td colspan="5">1</td></tr><tr><td colspan="2">WDOG</td><td colspan="5">2</td></tr><tr><td rowspan="2">三相预驱</td><td>电压</td><td>24V</td><td>24V</td><td>48V</td><td>-</td><td>-</td></tr><tr><td>结构</td><td>P+N</td><td>P+N</td><td>N+N</td><td>-</td><td>-</td></tr><tr><td colspan="2">ADC/TouchKey(单元/通道数)</td><td>1/7</td><td>1/7</td><td>1/7</td><td>1/8</td><td>1/8</td></tr><tr><td colspan="2">OPA运放</td><td colspan="5">1</td></tr><tr><td colspan="2">OPA轮询</td><td colspan="5">3</td></tr><tr><td colspan="2">CMP比较器(组)</td><td colspan="5">2</td></tr><tr><td rowspan="3">通信接口</td><td>U(S)ART</td><td colspan="5">2</td></tr><tr><td>SPI</td><td colspan="5">1</td></tr><tr><td>I²C</td><td colspan="5">1</td></tr><tr><td colspan="2">CPU主频(MHz)</td><td colspan="5">48</td></tr><tr><td colspan="2">VDD(V)</td><td colspan="2">6-24</td><td>6-48</td><td colspan="2">2.5-5</td></tr><tr><td colspan="2">封装</td><td>QSOP24</td><td>QFN26C3</td><td>QSOP28</td><td>QSOP24</td><td>QFN32</td></tr></table>

注：更多型号请参考MCU选型表

### 其他内置预驱SoC\Others

CH641：基于青棵V2A内核的PD无线充电和电机控制SoC。

内置12V高压I/O用于驱动MOS，支持USB PD及Type-C功率传输、BC1.2及DCP等多种充电协议，提供ADC、电机PWM、差分输入电流采样和交流小信号放大解码器、过压和过温保护。

具有5~12V宽工作电压、单线调试、低功耗、外围精简等特点。

### 典型应用 \Applications

![](images/0f2c82c6bff84d8902648879f43afe5deb2782bdee1a5f05945f41bfd2b2ecc2.jpg)

电机应用

![](images/d19ebc5d59159a31c3e00d5bf20dff5b83d149117630ffa85257b2f799776e73.jpg)  
工业设备

![](images/d581287a6e8b3e5d424ac1c5bcbd27c8b4516e07d8a7c0a84bee3a2d00426a5a.jpg)  
智能家电

![](images/7522ae1df95cbed4133582c0e059ccaae8b0d388705eb8dd750d2ca5c5820c89.jpg)  
运动出行  
电机应用

工业设备


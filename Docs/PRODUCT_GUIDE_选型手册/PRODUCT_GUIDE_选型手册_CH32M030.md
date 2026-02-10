## CH32M030

### 青稞RISC-V内核

### 28V双N预驱电机控制MCU

CH32M030是扩展工业级的电机微控制器，采用青棵V3B内核且针对电机算法进行指令优化，72MHz主频，内置带ECC校验的64K闪存。芯片增强模拟和电源管理能力，提供4路运放、3路比较器、2路差分电流采样、2路可编程灌电流支持外部DC-DC动态调压，集成4对N管预驱和28V高压LDO，2组Type-C和USB PD控制器及PHY、USB2.0全速主机/设备接口，支持PDUSB和PD PPS，提供高压I/O引脚、过压过温保护，适于电机控制、双向Type-C方案、无线充等应用。

### 应用框图 \ Block Diagram

![](images/852fdde1b49ae473c7cb07842ac85b5bf0c0dcc5e61346fc23733dcb3d0c4295.jpg)

#### 产品特点 \ Features

> 青稞RISC-V3B处理器,特有高速中断响应机制  
> 最高72MHz系统主频  
> 支持RV32IMCB指令集和自扩展指令  
> 12KB SRAM, 64KB Flash   
> 内置高压LDO,VHV支持额定5~28V系统供电  
> 预驱动I/O额定电压：5～10V   
> 多种低功耗模式：睡眠/停止/待机  
> 4个双N型MOSFET半桥驱动器,外部只需电容   
>7路通用DMA控制器  
>20路外部12位ADC转换通道  
> 支持外部延迟触发，支持ADC滑动平均功能  
> 1个16位高级定时器  
> 1个16位通用定时器  
> 1个16位的精简通用定时器  
> 1个窗口看门狗定时器  
> 1个系统时基定时器

> 4组运放OPA、3组模拟电压比较器CMP   
> 支持组合为2组交流小信号放大解码器QII1及QII2和2组差分输入电流采样ISP  
> 2组Type-C和USB PD控制器及PHY  
> 全速USB2.0控制器及PHY  
> 支持BC1.2及多种HVDCP/CDP充电协议  
OTP过温保护和OVP过压保护及欠压复位  
多引脚映射的UART串口、1个I²C接口、1个SPI接口  
> 2组10位可编程灌电流模块   
> 2组源电流模块   
> 36个I/O口,映射16个外部中断   
> 8个MV预驱动引脚,2个HV高压引脚   
> 64位芯片唯一ID   
> 支持单线和双线两种调试模式  
> 全系支持105°C工作温度,个别型号支持125°C  
封装形式：QFN48X7_A、LQFP48、QFN48、QFN32、QSOP28

#### 主要资源

Main Resource

<table><tr><td colspan="2">典型产品型号</td><td>CH32M030C8U3</td><td>CH32M030C8T7</td><td>CH32M030C8U7</td><td>CH32M030K8U7</td><td>CH32M030G8R7</td></tr><tr><td colspan="2">内核</td><td colspan="5">RISC-V</td></tr><tr><td colspan="2">Flash (KB)</td><td colspan="5">64</td></tr><tr><td colspan="2">SRAM (KB)</td><td colspan="5">12</td></tr><tr><td colspan="2">半桥栅极驱动器</td><td colspan="3">4</td><td>2</td><td>3</td></tr><tr><td colspan="2">GPIO</td><td>36</td><td>35</td><td>36</td><td>24</td><td>17</td></tr><tr><td colspan="2">预驱动I/O (MVI/0)</td><td colspan="2">8</td><td>1</td><td colspan="2">6</td></tr><tr><td colspan="2">高压I/O (HVI/0)</td><td>2</td><td>-</td><td>1</td><td>1</td><td>-</td></tr><tr><td></td><td>高级</td><td colspan="5">1</td></tr><tr><td rowspan="2">定时器</td><td>通用</td><td colspan="5">1</td></tr><tr><td>精简</td><td colspan="5">1</td></tr><tr><td></td><td>WWDG</td><td colspan="5">1</td></tr><tr><td></td><td>SysTick</td><td colspan="5">1</td></tr><tr><td colspan="2">ADC/TouchKey(单元/通道数)</td><td>1/20</td><td>1/20</td><td>1/20</td><td>1/16</td><td>1/11</td></tr><tr><td colspan="2">OPA运放(组)</td><td colspan="3">4</td><td colspan="2">3</td></tr><tr><td colspan="2">CMP比较器(组)</td><td colspan="3">3</td><td colspan="2">2</td></tr><tr><td colspan="2">电流采样ISP, ISN</td><td colspan="3">差分*2</td><td>差分*1/单端*1</td><td>差分*2</td></tr><tr><td colspan="2">信号解码QII</td><td colspan="3">2</td><td colspan="2">1</td></tr><tr><td colspan="2">可编程灌电流模块</td><td colspan="3">2</td><td>2</td><td>1</td></tr><tr><td colspan="2">源电流模块</td><td colspan="3">2</td><td>1</td><td>-</td></tr><tr><td rowspan="5">通信接口</td><td>U (S) ART</td><td colspan="5">1</td></tr><tr><td>SPI</td><td colspan="4">1</td><td>-</td></tr><tr><td>I²C</td><td colspan="5">1</td></tr><tr><td>USB (FS)</td><td colspan="5">Host/Device</td></tr><tr><td>USB PD
Type-C</td><td>(CC1R, CC2R)
(CC3, CC4)
内置Rd</td><td>(CC1, CC2)
(CC3, CC4)</td><td colspan="2">(CC1R, CC2R)
(CC3, CC4)
内置Rd</td><td>(CC3, CC4)</td></tr><tr><td colspan="2">CPU主频 (MHz)</td><td colspan="5">72</td></tr><tr><td colspan="2">VDD(V)</td><td colspan="5">3.3</td></tr><tr><td colspan="2">封装</td><td>QFN48X7_A</td><td>LQFP48</td><td>QFN48</td><td>QFN32</td><td>QSOP28</td></tr></table>

注: 更多型号请参考MCU选型表

### 典型应用 \Applications

![](images/18154f1597f6be8a77d81d90376d8c39bd87f6bc5b85838049502ac1ec7cd416.jpg)  
电机应用

![](images/c81a17f134a6accf44fac994df8616d692f746188cee892449436ab7a3f6258d.jpg)  
工业设备

![](images/5fe61698a2b36c83a4b9c496f2b3ff4dacd14a82df8fa37b521d578460430a11.jpg)  
智能家电

![](images/f507f93a51c9fc81bf6ce904dae4bb7030e020c12a326da313f8016d14aec450.jpg)  
运动出行


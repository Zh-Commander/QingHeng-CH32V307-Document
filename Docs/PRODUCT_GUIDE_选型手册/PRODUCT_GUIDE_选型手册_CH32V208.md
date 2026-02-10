## CH32V208  
CH32F208

### 青稞RISC-V/Cortex-M3内核低功耗蓝牙无线型MCU

BLE5.3 10M以太网

CH32V208是基于青稞V4C内核的工业级通用微控制器，144MHz主频，集成低功耗蓝牙BLE通讯模块、以太网控制器及PHY、USB2.0全速设备+主机/设备接口、CAN控制器、运放、ADC和触摸按键等功能。

### 应用框图 \ Block Diagram

![](images/d35f3fc23c743fd936962324921dc26ffc30cc1476f36270fa409fd78a74d608.jpg)

#### 产品特点 \ Features

> RISC-V4C处理器,最高144MHz系统主频   
> 支持单周期乘法和硬件除法  
> 64KB SRAM,128KB Flash   
> 10M以太网控制器ETH(MAC+PHY)  
> 低功耗蓝牙BLE 5.3  
> GPIO单元独立供电,可不同步系统供电  
> 多种低功耗模式：睡眠/停止/待机  
> 上电/断电复位 (POR/PDR)  
> 可编程电压监测器(PVD)  
> 2组运放、比较器OPA   
> 16路TouchKey通道检测

> 16路12位ADC转换通道  
>5个定时器  
> USB2.0全速主机/设备+设备接口  
> 1组CAN接口(2.0B主动)  
> 2个 $\mathsf{I}^2\mathsf{C}$ 接口  
> 4个USART   
> 2个SPI接口(支持Master和Slave模式)  
> 53个I/O   
CRC计算单元,96位芯片唯一ID  
> 串行单线调试接口SDI   
封装形式：LQFP64M、QFN68、QFN48、QFN28

#### 主要资源

Main Resource

<table><tr><td colspan="2">典型产品型号</td><td>CH32V208WBU6</td><td>CH32V208RBT6</td><td>CH32F208WBU6</td><td>CH32F208RBT6</td></tr><tr><td colspan="2">内核</td><td colspan="2">RISC-V</td><td colspan="2">Cortex-M3</td></tr><tr><td colspan="2">Flash (KB)</td><td colspan="4">128</td></tr><tr><td colspan="2">SRAM (KB)</td><td colspan="4">64</td></tr><tr><td colspan="2">GPIO</td><td>53</td><td>49</td><td>53</td><td>49</td></tr><tr><td rowspan="5">定时器</td><td>高级(16位)</td><td colspan="4">1</td></tr><tr><td>通用(16位)</td><td colspan="4">3</td></tr><tr><td>通用(32位)</td><td colspan="4">1</td></tr><tr><td>WDOG</td><td colspan="4">2</td></tr><tr><td>SysTick</td><td colspan="4">1</td></tr><tr><td colspan="2">RTC</td><td colspan="4">1</td></tr><tr><td colspan="2">ADC/TouchKey(单元/通道数)</td><td colspan="4">1/16</td></tr><tr><td colspan="2">运放、比较器</td><td colspan="4">2</td></tr><tr><td rowspan="7">通信接口</td><td>U(S)ART</td><td colspan="4">4</td></tr><tr><td>SPI</td><td colspan="4">2</td></tr><tr><td>I²C</td><td colspan="4">2</td></tr><tr><td>CAN</td><td colspan="4">1</td></tr><tr><td>USB (FS)</td><td colspan="4">Device+Host/Device</td></tr><tr><td>Ethernet</td><td colspan="4">10M MAC+10M PHY</td></tr><tr><td>BLE</td><td colspan="4">5.3</td></tr><tr><td colspan="2">主频(MHz)</td><td colspan="4">144</td></tr><tr><td colspan="2">VDD(V)</td><td colspan="4">2.5/3.3</td></tr><tr><td colspan="2">封装</td><td>QFN68</td><td>LQFP64M</td><td>QFN68</td><td>LQFP64M</td></tr></table>

注：更多型号请参考MCU选型表

#### 典型应用

Applications

![](images/e77b3dcab02ec683f7324c046a3712873cf9aaac5aaa0fb44ed15011a3ad90f0.jpg)  
工业控制

![](images/0c5b15af78be96b0c8844ebcf203b137702f858b75621dce0c7cdd797252b5a9.jpg)  
物联网

![](images/a515357ddae57527038fd176ed288ccf25705638c17ee0c0e54ea49d0be4a612.jpg)  
计算机与手机周边

![](images/fbeff09b8e61271931d90b2980831ff9213dd7e855a932c3df2c7d8bb95aec36.jpg)  
消费电子


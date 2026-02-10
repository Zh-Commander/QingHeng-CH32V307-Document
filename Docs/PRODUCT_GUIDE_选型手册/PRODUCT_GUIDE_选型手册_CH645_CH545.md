## CH645 CH545

### 青稞RISC-V内核USB多主机/设备+双PD+以太网多接口MCU/SoC

CH645内置8组USB高速PHY和2组PD PHY，提供8个USB主机口/4个USB设备口，通过片内4通道包括7端口HUB的USB组合设备控制器可支持最多28个USB设备。芯片支持PDUSB，可单芯片实现高速USB数据传输与Type-C功率传输，内置100M以太网MAC控制器及PHY，提供SDIO、5个I²C等丰富外设资源。为PDHUB、KVM、隔离及远距离USB、Type-C扩展坞等应用提供高集成度的解决方案。

### 应用框图 \ Block Diagram

![](images/4f83ff98a441f14131a0af9891c7cf8bbebb8147abbe7b6fc03e60031280432a.jpg)

![](images/0539b36a11e39f6c3fe0a8d5a32a24e8482e1db389df956d0632fbc8ef339c54.jpg)

#### 产品特点 \ Features

> RISC-V内核，125MHz主频   
> 内置出厂调校的20MHz的RC振荡器  
> 内置4通道带HUB的USB组合设备控制器，支持4端口KVM应用  
> 基于SerDes的远距离USB收发器PHY，支持USB信号隔离和远距离传输  
> USB2.0高速控制器及收发器PHY，支持最多8个USB主机和最多4个USB设备  
> 2组USB PD和Type-C控制器及PHY  
> 以太网控制器MAC及10M/100M PHY  
> SDIO主机/从机接口，支持EMMC/SD/SDIO卡  
> 串行2线调试接口SDI   
封装形式：QFN68、QFN32

#### 选型指南 \ Model Selection Guide

<table><tr><td>Part NO.</td><td>Flash</td><td>RAM</td><td>USB</td><td>USB隔离远传</td><td>Ethernet</td><td>SDIO</td><td>Type-C</td><td>UART</td><td>SPI</td><td>I²C</td><td>I/O</td><td>Timer</td><td>VDD</td><td>Package</td></tr><tr><td>CH645W</td><td rowspan="2">224K</td><td rowspan="2">72-80K</td><td>8*H/28*D(480Mbps)</td><td rowspan="2">√</td><td rowspan="2">100M MAC+PHY</td><td rowspan="2">1</td><td rowspan="2">PD*2</td><td rowspan="2">2</td><td>2</td><td>5</td><td>40</td><td rowspan="2">2*16b</td><td rowspan="2">3.3V</td><td>QFN68</td></tr><tr><td>CH645F</td><td>5*H/4*D(480Mbps)</td><td>1</td><td>4</td><td>13</td><td>QFN32</td></tr><tr><td>CH545</td><td>64K</td><td>8K+256</td><td>4*H/17*D</td><td>-</td><td>-</td><td>-</td><td>-</td><td>2</td><td>2</td><td>5</td><td>58</td><td>3*16b</td><td>3.3V/5V</td><td>LQFP64</td></tr></table>

### 青稞RISC-V内核支持Type-C的全内驱RGB全彩键盘MCU/SoC

CH643支持USB数据通讯和PD功率传输。芯片内置可编程协议I/O控制器，全内置RGB显示驱动支持192组RGB三色LED或576只单色LED，外置PMOS支持288组RGB，适于RGB键盘、RGB面板等应用。

CH555内置了RGB驱动单元，支持128组RGB三色LED或384只单色LED，适于RGB灯光驱动、键盘等应用。


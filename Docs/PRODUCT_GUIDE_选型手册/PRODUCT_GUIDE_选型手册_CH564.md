## CH564

### 应用框图 \ Block Diagram

![](images/ddedefdb719a01e1c025edfbb5de2988ebc5e90832bf7fbd5146f6b699123286.jpg)

#### 产品特点 \ Features

> RISC-V内核,120MHz主频   
> 支持单周期乘法和硬件除法  
> 448KB CodeFlash, 32KB DataFlash   
> 16KB的32位宽SRAM  
> 32/64/96KB可配置的128位宽SRAM   
> 超高速USB3.0 5Gbps、USB2.0高速480Mbps主机和设备控制器及收发器(内置PHY)  
> 内置千兆以太网控制器

> 内置SerDes控制及收发器，网线传输距离达90m  
> 内置数字视频接口DVP  
> 内置高速并行接口HSPI, 最快传输速度约为3.8Gbps  
> 内置EMMC控制器  
支持AES/SM4算法  
> 主动并口:8位数据, 15位地址总线  
> 4组UART,2组SPI接口,3组26位定时器  
> 集成2线调试接口,支持在线仿真

#### 选型指南 \ Model Selection Guide

<table><tr><td>Part NO.</td><td>Freq/Max</td><td>Flash</td><td>RAM</td><td>DataFlash</td><td>USB3.0</td><td>USB2.0</td><td>Ethernet</td><td>SerDes</td><td>HSPI</td><td>DVP</td><td>SDIO</td><td>Encrypt</td><td>UART</td><td>SPI</td><td>Timer</td><td>CAP</td><td>PWM</td><td>GPIO</td><td>VDD</td><td>Package</td></tr><tr><td>CH569W</td><td>96/120MHz</td><td>448K</td><td>48/80/112K</td><td>32K</td><td>OTG</td><td>H/D</td><td>1G MAC</td><td>1.25Gb</td><td>3.8Gb</td><td>-</td><td>1*UHS</td><td>AES/SM4</td><td>4</td><td>2</td><td>3*26b</td><td>3</td><td>7</td><td>49</td><td>3.3</td><td>QFN68</td></tr><tr><td>CH565W</td><td>96/120MHz</td><td>448K</td><td>48/80/112K</td><td>32K</td><td>OTG</td><td>H/D</td><td>1G MAC</td><td>1.25Gb</td><td>-</td><td>96MHz</td><td>1*UHS</td><td>AES/SM4</td><td>4</td><td>2</td><td>3*26b</td><td>3</td><td>7</td><td>49</td><td>3.3</td><td>QFN68</td></tr></table>

### 应用框图 \ Block Diagram

![](images/06ac29e7be21a05c62ae5d985ed522e84aeeb1c296343b46e56eebf300125d63.jpg)

#### 产品特点 \ Features

> RISC-V4J处理器,最高120MHz系统主频  
> 支持单周期乘法和硬件除法  
> 可配64/96/128KB SRAM  
> 448KB CodeFlash, 32KB DataFlash   
> 低功耗模式：睡眠/深度睡眠  
> 480Mbps USB2.0高速接口,支持主机/设备模式  
> 内置高速USB PHY,无需外接PHY收发器  
> USB PD和Type-C控制器及PHY  
> 10M/100M以太网接口, MAC和PHY全集成

> 12位ADC,7路外部通道  
> 4个28位通用定时器  
> 1个系统时基定时器  
> 4组串口、1个I²C、2个SPI  
> 1个8位被动并口、1个外部总线接口  
> 3组GPIO, 77个I/O口, 部分耐受5V  
> 96位芯片唯一ID   
> 支持单线/双线两种调试模式  
> CH564Q/L与CH563Q/L引脚基本兼容，功耗更低

#### 选型指南 \ Model Selection Guide

<table><tr><td rowspan="2">PartNO.</td><td rowspan="2">Freq</td><td rowspan="2">CodeFlash</td><td rowspan="2">DataFlash</td><td rowspan="2">SRAM</td><td rowspan="2">GPIO</td><td rowspan="2">GP Timer</td><td rowspan="2">PWM</td><td rowspan="2">CAP</td><td rowspan="2">ADC</td><td colspan="2">PDUSB</td><td rowspan="2">Ethernet 10/100M MAC+PHY</td><td rowspan="2">SLV</td><td rowspan="2">XBUS</td><td rowspan="2">UART</td><td rowspan="2">I²C</td><td rowspan="2">SPI</td><td rowspan="2">Package</td></tr><tr><td>USB2.0 HS 480Mbps</td><td>Type-C Source Sink DRP</td></tr><tr><td>CH564L</td><td>120MHz</td><td>448K</td><td>32K</td><td>64/96/128K</td><td>77</td><td>4*28bit</td><td>4</td><td>4</td><td>7+2</td><td>H/D</td><td>CC1,CC2</td><td>√</td><td>1</td><td>1</td><td>4</td><td>1</td><td>2</td><td>LQFP128</td></tr><tr><td>CH564Q</td><td>120MHz</td><td>448K</td><td>32K</td><td>64/96/128K</td><td>30</td><td>4*28bit</td><td>4</td><td>4</td><td>6+2</td><td>H/D</td><td>CC1,CC2</td><td>√</td><td>1</td><td>-</td><td>4</td><td>1</td><td>2</td><td>LQFP64M</td></tr><tr><td>CH564F</td><td>120MHz</td><td>448K</td><td>32K</td><td>64/96/128K</td><td>20</td><td>4*28bit</td><td>3</td><td>3</td><td>4+2</td><td>H/D</td><td>CC1,CC2</td><td>√</td><td>1</td><td>-</td><td>4</td><td>1</td><td>2</td><td>QFN28</td></tr><tr><td>CH564C</td><td>120MHz</td><td>192K</td><td>32K</td><td>64/96/128K</td><td>17</td><td>4*28bit</td><td>2</td><td>2</td><td>4+2</td><td>H/D</td><td>CC1,CC2</td><td>-</td><td>1</td><td>-</td><td>4</td><td>1</td><td>2</td><td>QFN26C3</td></tr></table>


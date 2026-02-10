## CH377

![](images/a0e95f47d33ea8b2c8658bcccd80b0aae2615ed0ff53afdffd38de8b1ce769b1.jpg)  
应用框图 \ Block Diagram

#### 产品特点 \ Features

> 4端口USB集线器,提供4个USB2.0高速下行端口,向下兼容低全速  
> 支持高性能MTT模式,为每个端口提供独立TT实现满带宽并发传输,总带宽是STT的4倍  
> 自研专用USB PHY,低功耗技术,相比第一代HUB芯片功耗大幅降低  
> 6KV增强ESD性能, Class 3A   
> 工业级温度范围：-40~85°C  
> 提供QFN28、QFN24、QFN16、QFN12、QSOP16、QSOP28等多种小体积、低成本、易加工的封装形式

选型指南 \ Model Selection Guide  

<table><tr><td>Part NO.</td><td>TT模式</td><td>过流检测</td><td>电源控制</td><td>LED指示灯</td><td>I/0引脚配置供电模式</td><td>外部EEPROM提供配置信息</td><td>定制配置信息</td><td>上行口交换功能</td><td>免晶振应用</td><td>封装</td></tr><tr><td>CH335J</td><td rowspan="7">MTT</td><td>-</td><td>-</td><td>-</td><td>-</td><td>-</td><td rowspan="7">√</td><td>-</td><td>√</td><td>QFN12</td></tr><tr><td>CH334P</td><td>-</td><td>-</td><td>1</td><td>-</td><td>-</td><td>-</td><td>可选</td><td>QFN16</td></tr><tr><td>CH334R</td><td>-</td><td>-</td><td>-</td><td>-</td><td>-</td><td>-</td><td>可选</td><td>QSOP16</td></tr><tr><td>CH334U/F</td><td>GANG</td><td>GANG</td><td>5</td><td>√</td><td>√</td><td>-</td><td>可选</td><td>QSOP28/QFN24</td></tr><tr><td>CH334S/Q</td><td>GANG</td><td>GANG</td><td>1</td><td>√</td><td>√</td><td>-</td><td>可选</td><td>SSOP28/QFN36X6</td></tr><tr><td>CH334H/L</td><td>独立/GANG</td><td>GANG</td><td>1</td><td>√</td><td>√</td><td>-</td><td>可选</td><td>QFN28X5/LQFP48</td></tr><tr><td>CH335F</td><td>独立/GANG</td><td>独立/GANG</td><td>5/9</td><td>√</td><td>√</td><td>√</td><td>可选</td><td>QFN28</td></tr></table>

### 典型应用 \Applications

计算机和工控机主板

计算机和工控机外设

嵌入式系统

![](images/0c5c6d17bb0ba5c431c738668a0814821b3e142da0b843d1faefe0b3e688e662.jpg)  
应用框图 \ Block Diagram

#### 产品特点 \Features

> 480Mbps高速USB设备接口，外围元器件只需晶振和电容  
> 支持SD卡、MMC卡以及SPI接口的FLASH芯片  
> 兼容SD卡规范2.0,兼容MMC规范4.5   
> 单一3.3V供电  
> CH377F支持串口记录仪模式,实时保存串口透传数据  
> CH377F支持FAT文件系统,支持通过配置文件配置参数  
> CH377F支持4路GPIO输入输出功能  
> CH377F串口通讯波特率支持2400bps~3000000bps  
> CH377A支持3端口USB2.0 HUB功能,提供3个USB2.0下行端口,兼容USB1.1规范  
> CH377A的HUB功能支持高性能的MTT模式,为每个端口提供独立TT实现满带宽并发传输  
> CH377A支持双磁盘功能,SD卡或MMC卡对应磁盘1,SPI的FLASH芯片对应磁盘2  
> CH377A支持4线或8线SDIO模式,CH377F仅支持4线SDIO模式  
> 内置EEPROM,可配置芯片VID、PID、最大电流值、厂商和产品信息字符串等参数  
> 提供QFN28无铅封装

### 典型应用 \Applications

计算机和工控机主板

计算机和工控机外设

嵌入式系统

### 高速USB2.0转FIFO/被动SPI/UART芯片

CH346是一款高速USB总线转接芯片，通过USB总线提供高速FIFO并口、被动SPI接口、两路串口等。

### 高速USB2.0转JTAG/主动SPI /I²C/UART/GPIO芯片

CH347是一款高速USB总线转接芯片，通过USB总线提供异步串口、I²C同步串行接口、SPI同步串行接口和JTAG接口等。

![](images/7386469c1f181ec5e72547845f1b998be7318168119901061ae198d2cef30163.jpg)  
应用框图 \ Block Diagram

#### 产品特点 \ Features

> 480Mbps高速USB2.0设备接口  
> 并口FIFO工作在从机模式，传输速度可达30M字节/s  
> SPI接口工作在从机模式，支持模式0/3，传输频率可达36MHz  
> 2通道硬件全双工串口，内置独立的收发缓冲区，通讯波特率支持最高15Mbps  
每个串口内置8192字节的接收FIFO，4096字节的发送FIFO   
> 串口支持半双工，提供串口正在发送状态指示TNOW，可用于控制RS485收发切换  
> I/O独立供电，支持3.3V、2.5V、1.8V和1.2V电源电压，适配不同电压的串口外设和主控  
> 内置EEPROM，可配置工作模式、芯片VID、PID、最大电流值、厂商和产品信息字符串等参数

### 典型应用 \Applications

FPGA/CPU/MCU数据采集

工业控制

编程下载器

![](images/07c7e2cff503bbfc55bed6a264944d928c60eed1534ff95ae9125676d2f1e894.jpg)  
应用框图 \ Block Diagram

#### 产品特点 \ Features

> 480Mbps高速USB设备接口，外围元器件只需晶振和电容  
> 支持JTAG主机接口，支持自定义协议的快速模式和bit-bang模式，传输频率可达30Mbit/s  
> 支持SPI模式0/1/2/3，支持传输频率配置，传输频率可达60MHz  
> 提供I²C主机接口，速度支持20K/100K/400K/750KHz  
> 硬件全双工串口，内置独立的收发缓冲区，通讯波特率支持1200bps～9Mbps  
> 支持半双工，提供串口正在发送状态指示TNOW，可用于控制RS485收发切换  
> I/O独立供电，支持3.3V、2.5V、1.8V和1.2V电源电压  
> 支持最多8路GPIO输入输出功能  
> 内置EEPROM，可配置工作模式、芯片VID、PID、最大电流值、厂商和产品信息字符串等参数

### 典型应用 \ Applications

FPGA/CPU/MCU调试下载

工业控制

编程下载器

### USB转串口芯片

USB高速/全速转串口系列芯片，可实现USB转1/2/4/8路串口，支持串口I/O独立供电，支持VCP/HID/CDC转串口，VCP串口支持硬件流控和高波特率连续通讯，部分型号支持VID/PID/String等内容配置，支持Windows/Linux/Android/macOS等操作系统。

### 应用框图 \ Block Diagram

![](images/5c1c16c6e1a3fc2bd6bf20ea9b4077c54a7ff29c9ab89e9bb3258d5c605320f6.jpg)

#### 产品特点 \ Features

> 单芯片实现高速/全速USB转接1/2/4/8路串口  
> 支持串口I/O独立供电，实现5V/3.3V/2.5V/1.8V等串口通讯  
> 支持高波特率与硬件流控，支持串口波特率自适应  
> 支持串口和其他总线扩展接口：FIFO/SPI/ $\mathrm{I}^2\mathrm{C}$ /JTAG等，共同使用。  
> 支持多种驱动类型,可使用厂商VCP串口驱动或CDC/HID类驱动  
> 内部高度集成,内置时钟/USB终端电阻/上电复位,外围精简  
> 内置UniqueID(USB Serial Number)  
> 内置/外置EEPROM,支持VID/PID/String等内容配置  
> 支持USB/BLE转虚拟串口,实现BLE/串口/USB三向透传  
> 支持免外围电路的串口一键下载功能

#### 产品路线

#### RoadMap

沁恒高速USB转串口系列芯片，可实现最高15Mbps波特率的连续稳定通讯。内部高度集成，晶振/USB终端电阻/EEPROM全内置。部分型号采用双电源设计，支持串口IO独立供电，可支持3.3V/2.5V/1.8V等串口通讯，提供多种封装可选。支持VID/PID/String等USB参数配置。

<table><tr><td>型号</td><td>串口数量</td><td>最高波特率 (Mbps)</td><td>串口I/O 电压支持</td><td>双供电</td><td>全功能 Modem</td><td>封装</td></tr><tr><td>CH9111L</td><td>1</td><td>15</td><td>3.3/2.5/1.8V</td><td>✓</td><td>✓</td><td>LQFP48</td></tr><tr><td>CH346C</td><td>1</td><td>15</td><td>3.3/2.5/1.8V</td><td>✓</td><td>✓</td><td>QFN26C3</td></tr><tr><td>CH347T</td><td>1</td><td>9</td><td>3.3V</td><td>-</td><td>✓</td><td>TSSOP20</td></tr><tr><td>CH346C</td><td>2</td><td>15</td><td>3.3/2.5/1.8V</td><td>✓</td><td>✓</td><td>QFN26C3</td></tr><tr><td>CH347F</td><td>2</td><td>9</td><td>3.3/2.5/1.8V</td><td>✓</td><td>-</td><td>QFN28</td></tr><tr><td>CH347T</td><td>2</td><td>9</td><td>3.3V</td><td>-</td><td>-</td><td>TSSOP20</td></tr><tr><td>CH9114F</td><td>4</td><td>15</td><td>3.3/2.5/1.8V</td><td>✓</td><td>-</td><td>QFN32</td></tr><tr><td>CH9114L</td><td>4</td><td>15</td><td>3.3/2.5/1.8V</td><td>✓</td><td>✓</td><td>LQFP64M</td></tr><tr><td>CH9114W</td><td>4</td><td>15</td><td>3.3/2.5/1.8V</td><td>✓</td><td>✓</td><td>QFN56X8</td></tr><tr><td>CH9344Q</td><td>4</td><td>12</td><td>3.3/2.5/1.8V</td><td>✓</td><td>-</td><td>LQFP48</td></tr><tr><td>CH9344L</td><td>4</td><td>12</td><td>3.3/2.5/1.8V</td><td>✓</td><td>-</td><td>LQFP48</td></tr><tr><td>CH344Q</td><td>4</td><td>6</td><td>3.3V</td><td>-</td><td>✓</td><td>LQFP48</td></tr><tr><td>CH348L</td><td>8</td><td>6</td><td>3.3/2.5/1.8V</td><td>✓</td><td>✓</td><td>LQFP100</td></tr><tr><td>CH348Q</td><td>8</td><td>6</td><td>3.3V</td><td>-</td><td>-</td><td>LQFP48</td></tr></table>

注：上述芯片均支持硬件流控，除CH9344L，其他芯片均支持USB参数配置。

上述芯片均为工业级设计，支持-40°C至85°C温度范围。

#### 选型指南

#### Model Selection Guide

CH9111/CH346/CH9114:480Mbps高速USB转1/2/4路全功能高速异步串口，波特率高达15Mbps，支持高波特率大数据高效持续传输，支持串口硬件流控，串口I/O电压支持3.3V/2.5V/1.8V，支持USB参数配置。CH9111还支持USB转SPI接口，CH346还支持USB转FIFO并口和SPI等通讯接口。  
CH347:480Mbps高速USB转2路全功能高速异步串口，波特率高达9Mbps，支持高波特率大数据高效持续传输，支持串口硬件流控，串口I/O电压支持3.3V/2.5V/1.8V，除串口功能外芯片还支持USB转JTAG/SPI/I²C/GPIO等通讯接口。  
CH348:480Mbps高速USB转八路增强型异步串口，波特率高达6Mbps，串口I/O电压支持3.3V/2.5V/1.8V，提供8路RS485方向控制引脚，48路GPIO等信号。  
CH343：全速USB转一路增强型异步串口，波特率高达6Mbps，支持串口硬件流控和高波特率大数据连续传输，串口I/O电压支持5V/3.3V/2.5V/1.8V，内置时钟，提供QFN小封装。  
CH342：全速USB转两路增强型异步串口，波特率高达3Mbps，支持串口硬件流控和高波特率大数据连续传输，串口I/O电压支持5V/3.3V/2.5V/1.8V，内置时钟，提供QFN小封装。  
CH340/CH341:USB转单串口芯片经典型号,提供免晶振版本,提供多种封装,

CH340K内置三只二极管用于减少独立供电时与MCU的I/O引脚之间的电流倒灌。


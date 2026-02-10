## CH378

### USB2.0高速文件管理控制芯片

CH378是一款高速文件管理控制芯片，用于单片机系统快速读写常用U盘或SD卡中文件。无需了解U盘底层操作，无需了解SD卡底层操作，无需了解FAT文件系统，即可轻松读写U盘或SD卡中的文件。

![](images/cc86e583966aa24fa27312168cf727eaf6e0ffb6db530b92edc3ef692d2c2b29.jpg)  
应用框图 \ Block Diagram

#### 产品特点 \ Features

> 支持常用的USB存储设备：U盘/USB硬盘/USB读卡器等  
支持常用SD卡及协议兼容卡：SD卡/Mini-SD卡/HC-SD卡/MMC卡/TF卡  
> 内置USB2.0协议固件，FAT12/FAT16/FAT32文件系统的管理固件  
> 内置20KB RAM，外部系统只需很少资源  
> 单片机通过简单命令即可实现文件操作（如打开/新建/删除/搜索/枚举等）  
支持长文件名和多级目录操作，支持U盘和SD卡  
> 提供多种MCU接口：8位被动并行接口、异步串口、SPI接口  
> 提供评估板和常用单片机应用例程

选型指南 \ Model Selection Guide

典型应用 \Applications  

<table><tr><td rowspan="2">Part NO.</td><td rowspan="2">USB接口规范</td><td rowspan="2">USB接口功能</td><td rowspan="2">USB HUB</td><td rowspan="2">USB底层固件</td><td rowspan="2">文件系统管理固件</td><td rowspan="2">操作U盘</td><td rowspan="2">操作SD卡</td><td colspan="3">MCU接口</td><td rowspan="2">连接检测与事件通知</td></tr><tr><td>并口</td><td>串口</td><td>SPI</td></tr><tr><td>CH378</td><td>高速/全速/低速</td><td>主机/设备</td><td>-</td><td>内置</td><td>✓</td><td>✓</td><td>✓</td><td>✓</td><td>✓</td><td>✓</td><td>✓</td></tr><tr><td>CH376</td><td>全速/低速</td><td>主机/设备</td><td>-</td><td>内置</td><td>✓</td><td>✓</td><td>✓</td><td>✓</td><td>✓</td><td>✓</td><td>✓</td></tr><tr><td>CH375</td><td>全速/低速</td><td>主机/设备</td><td>-</td><td>内置</td><td>-</td><td>✓</td><td>-</td><td>✓</td><td>✓</td><td>-</td><td>✓</td></tr><tr><td>CH374</td><td>全速/低速</td><td>主机/设备</td><td>3端口根集线器</td><td>-</td><td>-</td><td>✓</td><td>-</td><td>✓</td><td>-</td><td>✓</td><td>✓</td></tr><tr><td>CH372</td><td>全速/低速</td><td>仅设备</td><td>-</td><td>内置</td><td>-</td><td>✓</td><td>-</td><td>✓</td><td>-</td><td>-</td><td>✓</td></tr><tr><td>CH370</td><td>全速/低速</td><td>仅主机</td><td>-</td><td>-</td><td>-</td><td>✓</td><td>-</td><td>✓</td><td>-</td><td>✓</td><td>✓</td></tr></table>

### USB总线通用接口芯片

#### CH376:单片机读写U盘或SD卡文件

> 内置FAT12/FAT16/FAT32文件系统管理固件，支持U盘或SD卡  
支持长文件名, 支持创建多级子目录  
> SPI主机接口支持SD卡及与其协议兼容的MMC卡和TF卡等  
> 提供8位被动并行接口、异步串口、SPI接口等多种MCU接口  
> 自动检测USB设备的连接和断开，提供事件通知  
> 支持USB主机和设备模式,可动态切换

#### CH375:单片机读写U盘文件

> 支持U盘，闪盘以及读卡器等  
> 单片机通过U盘文件系统管理库读写USB存储设备中的文件  
> 自动检测USB设备的连接和断开，提供事件通知  
> 支持USB主机和设备模式,可动态切换

#### CH374:内置HUB同时管理多个USB设备

> 内置3端口USB根集线器Root-HUB,可同时连接和管理3个USB设备  
> 提供8位被动并口和SPI串行接口等多种MCU接口  
> 自动检测USB设备的连接和断开，提供事件通知  
> 支持USB主机和设备模式,可动态切换

#### CH372:USB设备接口, 自动完成枚举过程

> 内置USB底层固件,支持省事的内置固件模式和灵活的外部固件模式  
> 内置固件可自动完成标准的USB枚举配置过程，简化单片机的固件编程  
> 全速USB设备接口，兼容USB2.0，即插即用

#### CH370:USB主机接口, 操作低/全速USB设备

> 提供8位被动并口和SPI串行接口连接MCU  
> 自动检测USB设备的连接和断开，提供中断通知

#### CH377:带3端口HUB功能的USB2.0高速读卡器芯片

> 支持SD卡、MMC卡以及SPI接口的FLASH芯片  
支持3端口USB2.0高速HUB功能  
> 单一3.3V供电；外围元器件只需晶振和电容   
> 支持串口记录仪模式,实时保存串口透传数据

#### CH132:ULPI接口的高速USB收发器芯片

> 兼容USB2.0协议规范和UTMI+Low Pin Interface (ULPI) 1.1协议规范   
> 支持USB2.0高速480Mbps、全速12Mbps和低速1.5Mbps数据发送和接收  
> 可为具有ULPI接口的MCU或FPGA扩展USB主机或设备接口

工业控制

安防监控

汽车电子

物联网

仪器仪表

纺织机械

公共服务终端

金融设备

一卡通系统

智能交通

电力电网

### 双Type-C接口显示器专用SoC芯片

CH9245支持2个Type-C口的PD协议通讯及电源通路管理，内置N型MOSFET栅极升压驱动模块和USB2.0信号二选一，支持双C口数据通信切换，集成双路差分运放，支持双口电流检测，内置高压LDO，静态功耗低，集成度高，可用于便携显示屏、拓展坞及多口线缆应用。

CH9245M支持PD3.2协议的通讯管理，最高支持140W PD协议通讯，可过认证。

### 一进多出快充线缆专用SoC芯片

CH9241支持PD3.2/3.0/2.0协议的通讯转发、功率分配及电源通路的切换管理，内置eMarker功能，支持最高140W PD协议功率输入。内置N型MOSFET栅极升压驱动模块，可直接驱动多路高侧NMOS进行电源通路切换控制，集成双路输出电流检测，可广泛用于各类一进多出快充线缆及相关应用。

#### 应用框图 Bl

#### Block Diagram

![](images/3d5db56893caa4af1aeb34386e8094d2aca6824967306da738c19f71d40dc519.jpg)

#### 产品特点

#### Features

> 支持2个Type-C口的PD协议通讯及电源通路管理  
> 支持PD3.2协议，最高140W通讯  
> 内置高压LDO，VBUS直接供电  
> 内置NMOS栅极升压驱动，多通路高侧NMOS直驱  
> 内置双路差分运放  
> 内置USB2.0模拟开关（CH9245F），支持两个Type-C口数据通信切换  
> 支持Type-C接口固件升级  
封装形式：QFN20、QFN32

### 应用框图 \ Block Diagram

![](images/745ff4cb2bd909c0935dba8abe00aefbbe7a8f16f31fd3a580ccdf764279e5d8.jpg)

#### 产品特点 \ Features

> 支持PD3.2/2.0，智能功率分配及电源通路控制  
> 内置eMarker功能，最高140W PD协议功率输入  
> 内置高压LDO，VBUS直接供电  
> 内置NMOS栅极升压驱动，多通路高侧NMOS直驱  
> 内置双路输出电流检测功能  
> 内置USB2.0模拟开关（CH9241F），支持USB数据通信及D+/D-快充协议  
> 支持基于输入端Type-C接口的固件升级  
封装形式：ESSOP10、QFN32

### 典型应用 \Applications

CH9241K一拖二：单插100W，双插共享5V，带数据切换

CH9241F一拖二：双口盲插，同时快充，智能功率分配，带数据切换

CH9241F—拖三：单DCDC，单插盲插快充；双插/三插单口快充，智能功率分配，带数据切换

CH9241A一拖二：140W双口盲插，智能自适应功率分配，支持数据通讯，外驱NMOS，内置eMarker

<table><tr><td colspan="2">单插</td><td colspan="2">双插</td><td colspan="3">三插</td><td colspan="2">BOM</td><td rowspan="2">芯片型号</td></tr><tr><td>OUT1</td><td>OUT2</td><td>OUT1</td><td>OUT2</td><td>OUT1</td><td>OUT2</td><td>OUT3</td><td>DCDC</td><td>NMOS</td></tr><tr><td>140W</td><td>140W</td><td colspan="2">122W+18W50W+50W82W+18W等智能自适应功率分配</td><td>/</td><td>/</td><td>/</td><td>1</td><td>6</td><td>CH9241A</td></tr><tr><td>100W</td><td>100W</td><td colspan="2">70W+30W</td><td colspan="2">60W+30W</td><td>10W</td><td>1</td><td>6</td><td rowspan="3">CH9241F</td></tr><tr><td>100W</td><td>100W</td><td>85W</td><td>12W</td><td>85W</td><td colspan="2">共享12W</td><td>1</td><td>4</td></tr><tr><td>100W</td><td>100W</td><td colspan="2">共享5V3A</td><td>/</td><td>/</td><td>/</td><td>/</td><td>2</td></tr><tr><td>100W</td><td>100W</td><td colspan="2">共享5V3A</td><td>/</td><td>/</td><td>/</td><td>/</td><td>2</td><td>CH9241K</td></tr><tr><td>100W</td><td>10W</td><td>90W</td><td>10W</td><td>/</td><td>/</td><td>/</td><td>1</td><td>内置</td><td>CH220P</td></tr></table>

CH249

CH248

CH246

CH32X035

### PD及无线充电专用SoC芯片

CH249是一款20V的无线充电发射专用SoC，支持为三个设备同时充电，集成双通道解码电路和双路差分电流放大模块及N管预驱，可实现WPC Qi等桌面、车载无线充电方案，支持PD3.2等快充协议及FB调压。

CH249M用于一芯多充方案,支持耳机、手机和手表同时充电。

CH249F用于单/双线圈和一圈多充方案，可适配Type-C和无线充电宝应用。

### 应用框图 \ Block Diagram

![](images/d407dc0cbe8fdcfec77f2e68819e290d513b1ed40e43d689283577ec49c1c69f.jpg)

#### 产品特点 \ Features

> 支持5V～20V快充协议输入、12V及以内DC输入  
> 支持一芯三充、一圈多充和双线圈  
> 支持PD3.2、BC1.2等多种快充协议  
> 内置NMOS全桥预驱，选型更灵活  
> 内置谐振电容切换控制电路

> 集成电压解码、电流解码和差分电流放大模块   
> 支持过欠压、过流、过温、FOD等保护  
> 芯片睡眠待机功耗低至75uA   
> 支持免拆机升级  
封装形式：QFN32、QFN48

### 其他无线充电芯片 Others

CH246：无线充电管理芯片，单芯片集成无线收发模块及小信号解码电路，外加部分客户自定义软件可轻松实现各类无线充电方案。支持PD、BC1.2等多种协议快充输入，支持5W/7.5W/10W/15W无线充电输出。  
CH248：无线充电发射专用SoC，支持PD3.0、BC1.2等多种快充协议输入，为手表、耳机、手机等设备提供最大15W无线充电功率。提供高压引脚直接驱动MOS、死区可调、支持半桥和全桥切换，灵活度高。  
CH32X035：青稞RISC-V内核，内置USB和PD PHY，支持PDUSB及Type-C快充功能，三充方案集成手表充，USB口升级，支持双线圈，支持充电电源功率监控和Q值检测。  
CH271/CH273/CH275：无线充电发射端全桥功率芯片，内置4个功率开关管和电流采样模块及无线充电反馈信号放大模块，集成过流/过温/过压保护和欠压锁定模块，内置LDO为MCU提供5V或3.3V电源，外围精简。CH271支持5V电压，CH273支持12V电压，CH275支持20V电压。

CH253

CH252

CH254

CH251

### eMarker电子标签芯片

CH253是一款高耐压USB Type-C线缆电子标签芯片，支持USB Type-C 2.1标准及USB PD3.1标准，最高支持USB4协议速率Passive Cable及ActiveCable，采用超小型DFN封装，单芯片集成VCONN二极管、Ra电阻和高压LDO，VCONN引脚耐压51V，CC引脚耐压55V，支持240W(48V5A)功率的各类Type-C线缆，支持配置数据多次更新烧录，具有锁定功能，便于开发的同时保证了数据安全。

### 应用框图 \ Block Diagram

![](images/743bf07f4b82e17343abac74d62c113de11a5f36b94fa70a1438e092afb09c38.jpg)

![](images/f6b8fa0a3a8144c4ff643bb7cbdb0da634cebf0bf99e053e1a19e60a3a4b7bd0.jpg)

#### 产品特点 \Features

> VCONN支持2.8V至30V输入电压  
> 支持USB Type-C 2.1标准及USB PD 3.1标准  
> 集成VCONN二极管和Ra电阻  
> VCONN引脚耐压51V，CC引脚耐压55V   
> 支持配置数据更新烧录  
> 支持EPR Mode

> 支持Discover SVIDs, Discover Modes, Enter Mode,  
> Exit Mode消息   
> 支持Get_Manufacturer_Info消息，厂商字符串可配置  
> 支持Get_Status消息  
> TID: 11163   
封装形式：DFN6

#### 其他电子标签芯片\Others

CH252：常规款，支持240W（48V5A）功率的各类Type-C线缆。

CH254：支持外部NTC多档温度保护及功率控制，支持240W(48V5A)功率各类Type-C线缆。

CH251：简化版本，支持100W(20V5A)或240W(48V5A)功率的Type-C五芯线缆。


## CH634

### 4端口USB3.0超高速HUB控制器芯片

CH634是符合USB3.2 Gen1协议规范的超高速USB HUB控制器芯片，单芯片集成四口USB HUB和USB PD功能，支持Type-C接口，支持上行口交换；内置两组Type-C双通道USB3.0 PHY和双PD PHY，原生支持Type-C正反插自动识别、PDHUB和Type-C电源15W和PD的100W快充(20V*5A)。

应用框图 \ Block Diagram

![](images/910a02394ddc9a81cff95f7ce4b7294e5c908a71188534df293972b919d0b9da.jpg)  
典型应用

#### 产品特点 \ Features

> 一个上行口,支持 USB3.0 超高速 5Gbps、USB2.0 高速 480Mbps 和全速 12Mbps  
> 四个下行口,支持 USB3.0 超高速5Gbps、USB2.0 高速480Mbps、全速12Mbps和低速 1.5Mbps  
> 内置两组自研的Type-C双通道USB3.0 PHY, 原生支持Type-C正插和反插自适应  
> 内置两路USB PD PHY, 原生支持Type-C电源15W和PD的100W快充, 支持PDHUB和扩展坞  
支持高性能MTT模式  
下行口支持BC1.2充电协议和CDP  
> 自研的HUB专用USB PHY,低功耗技术,支持自供电或总线供电  
> 支持SMBus总线,支持主板集成和管理  
> 支持上行口交换功能,便于2个USB主机管理多个USB设备  
> 集成3.3V的LDO调压器和1.2V的DC-DC降压器,支持外部5V电源供电,外围精简  
支持外加Type-C接口芯片CH211实现28V高压PDHUB和扩展坞  
> 提供QFN32、QFN48、QFN64、QFN68等多种封装形式

选型指南

Model Selection Guide

<table><tr><td>型号
功能</td><td>CH634F</td><td>CH634M</td><td>CH634X</td><td>CH634W6G</td></tr><tr><td>USB2端口</td><td>4</td><td>4</td><td>4</td><td>4</td></tr><tr><td>USB3端口</td><td>2</td><td>4</td><td>4+2C</td><td>4</td></tr><tr><td>PD控制器</td><td>×</td><td>1</td><td>2</td><td>1</td></tr><tr><td>上行口交换</td><td>×</td><td>✓</td><td>✓</td><td>×</td></tr><tr><td>MTT模式</td><td>✓</td><td>✓</td><td>✓</td><td>✓</td></tr><tr><td>独立过流检测</td><td>×</td><td>×</td><td>4</td><td>×</td></tr><tr><td>整体过流检测</td><td>✓</td><td>✓</td><td>✓</td><td>✓</td></tr><tr><td>独立电源控制</td><td>×</td><td>×</td><td>4</td><td>×</td></tr><tr><td>整体电源控制</td><td>✓</td><td>✓</td><td>✓</td><td>✓</td></tr><tr><td>I/O配置 整体/独立</td><td>-</td><td>-</td><td>✓</td><td>-</td></tr><tr><td>I/O配置电源控制极性</td><td>-</td><td>-</td><td>-</td><td>✓</td></tr><tr><td>LED指示灯</td><td>×</td><td>1</td><td>1</td><td>1</td></tr><tr><td>内部EEPROM配置信息</td><td>✓</td><td>✓</td><td>✓</td><td>✓</td></tr><tr><td>外部EEPROM配置信息</td><td>×</td><td>×</td><td>×</td><td>×</td></tr><tr><td>外部FLASH配置信息</td><td>×</td><td>×</td><td>×</td><td>✓</td></tr><tr><td>SMBus接口配置信息</td><td>✓</td><td>✓</td><td>✓</td><td>×</td></tr><tr><td>定制配置信息</td><td>✓</td><td>✓</td><td>✓</td><td>✓</td></tr><tr><td>I/O配置 BC充电</td><td>-</td><td>✓</td><td>-</td><td>✓</td></tr><tr><td>Typc-C快充15W</td><td>×</td><td>✓</td><td>✓</td><td>✓</td></tr><tr><td>PDHUB快充100W</td><td>×</td><td>✓</td><td>✓</td><td>✓</td></tr><tr><td>单5V供电</td><td>×</td><td>✓</td><td>✓</td><td>✓</td></tr><tr><td>单3.3V供电</td><td>×</td><td>✓</td><td>✓</td><td>✓</td></tr><tr><td>3.3V+1.2V双供电</td><td>✓</td><td>✓</td><td>✓</td><td>✓</td></tr><tr><td>封装</td><td>QFN32</td><td>QFN48</td><td>QFN68</td><td>QFN64</td></tr><tr><td>塑体尺寸</td><td>4*4</td><td>5*5</td><td>8*8</td><td>8*8</td></tr></table>

注：除上述型号，还提供CH634W5M、CH634W6C、CH634W6T、CH634W7G、CH634W7R、CH634W7S、CH634W7U、CH634W7V、CH634W8G等更多客制引脚型号。

Applications

计算机和工控机主板

计算机和工控机外设

嵌入式系统

USB集线器

拓展坞

### 7端口集成以太网、读卡器、USB PD等多功能高速HUB芯片

CH339符合USB2.0协议规范，单芯片集成7口USB HUB、百兆以太网、高速SD读卡器、USB PD和USB转JTAG/UART/SPI/I2C接口等功能。芯片支持高性能MTT模式，工业级设计，外围精简。部分型号支持上行口交换，非以太网场合可免晶振。

### 7端口工业级USB HUB控制器芯片

CH338符合USB2.0协议规范，支持高性能MTT模式，部分应用场合可免晶振。部分型号支持上行口交换，集成USB PD功能，支持Type-C功率传输。工业级设计，外围精简，适于计算机和工控机主板、外设、嵌入式系统等应用场景。

### 应用框图 \ Block Diagram

![](images/27485f12e2c4de0000cac5ad8e1c5aca5355c48c01337d469a2f690e550cd436.jpg)

#### 产品特点 \ Features

> 7端口USB集线器，上行端口支持USB2.0高速480Mbps和全速12Mbps，下行端口支持USB2.0高速、全速和低速  
非以太网应用场合可支持免晶振模式，节省外置晶体和电容  
> 自研专用USB PHY, 低功耗技术, 支持自供电或总线供电  
> 自研10M/100M以太网MAC+PHY,兼容IEEE 802.3 10BASE-T/100BASE-TX  
> 10M/100M自动协商,支持UTP CAT5E、CAT6双绞线,支持Auto-MDIX,自动识别正负信号线  
支持通过魔术包和网络唤醒包等事件进行远程唤醒  
> 支持IPv4/IPv6封包校验,支持IPv4 TCP/UDP/HEAD和IPv6 TCP/UDP封包校验生成和检查  
> 支持SD卡和MMC卡，可将其转换成标准的USB大容量存储类设备  
> SDIO接口兼容SD卡规范2.0、MMC规范4.5  
> 提供USB转JTAG/UART/SPI/I2C等接口功能  
> 6KV增强ESD性能,Class 3A   
工业级温度范围：-40～85℃  
> 提供QFN68、QFN32等小体积、低成本、易加工的封装形式

### 典型应用 \Applications

计算机和工控机主板

计算机和工控机外设

嵌入式系统

USB HUB

拓展坞

### 应用框图 \ Block Diagram

![](images/8dfd73cd1e1ac49c131f5b80b211843f5cfc45c2232cf3b0f7495865d98725e2.jpg)

#### 产品特点 \ Features

> 7端口USB集线器,上行端口支持USB2.0高速480Mbps和全速12Mbps,下行端口支持USB2.0高速、全速和低速  
> 部分应用场合可支持免晶振模式，节省外置晶体和电容  
> 自研专用USB PHY, 低功耗技术, 支持自供电或总线供电  
> 6KV增强ESD性能, Class 3A   
> 工业级温度范围：-40～85℃  
> 提供QFN64X9、LQFP48、QFN32等多种小体积、低成本、易加工的封装形式

#### 选型指南 \ Model Selection Guide

<table><tr><td>Part NO.</td><td>TT
模式</td><td>过流检测</td><td>电源控制</td><td>LED
指示灯</td><td>I/0引脚
配置
供电模式</td><td>I/0引脚
配置
不可移除设备</td><td>外部/内部EEPROM
SMBus接口
配置信息</td><td>定制
配置信息</td><td>上行口
交换功能</td><td>Type-C
PD</td><td>芯片供电</td><td>封装</td></tr><tr><td>CH338X</td><td rowspan="3">MTT</td><td>独立/GANG</td><td>独立/GANG</td><td>7+4</td><td>√</td><td>√</td><td rowspan="3">√</td><td rowspan="3">√</td><td>-</td><td>-</td><td>单3.3V</td><td>QFN64X9</td></tr><tr><td>CH338L</td><td>GANG</td><td>GANG</td><td>15</td><td>-</td><td>√</td><td>-</td><td>-</td><td>单3.3V/单5V</td><td>LQFP48</td></tr><tr><td>CH338F</td><td>GANG</td><td>GANG</td><td>-</td><td>-</td><td>-</td><td>√</td><td>√</td><td>单3.3V</td><td>QFN32</td></tr></table>

### 典型应用 \Applications

计算机和工控机主板

计算机和工控机外设

嵌入式系统


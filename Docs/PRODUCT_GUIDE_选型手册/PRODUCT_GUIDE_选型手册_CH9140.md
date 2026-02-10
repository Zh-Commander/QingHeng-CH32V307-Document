## CH9140  
CH9141  
CH9142  
CH9143

### BLE转单/双串口、三通芯片

蓝牙、串口和USB之间的数据互传，基于USB和BLE虚拟化串口技术，兼容常规串口应用程序。

CH9141F/K:蓝牙串口透传芯片,支持AT

CH9140:蓝牙转串口芯片

CH9142:蓝牙转双串口芯片

CH9143:BLE/UART/USB三通芯片

![](images/036157e7375e941ad74fd3716b8333bc730b052028051df97bf9e1ca66dbd1bb.jpg)  
应用框图 \ Block Diagram

产品特点 \ Features  

<table><tr><td>型号</td><td>封装</td><td>功能概述</td></tr><tr><td>CH9140</td><td>QFN28</td><td>蓝牙转串口芯片。基于BLE虚拟化串口技术,实现蓝牙和串口之间的数据互传,兼容常规串口应用程序,无需二次开发,即连即用。</td></tr><tr><td>CH9141F</td><td>QFN28</td><td rowspan="2">蓝牙串口透传芯片。实现蓝牙和串口数据之间的透明传输。支持串口AT和蓝牙传输指令配置,MODEM联络信号,并提供通用GPIO、同步GPIO、ADC采集等功能。</td></tr><tr><td>CH9141K</td><td>ESSOP10</td></tr><tr><td>CH9142</td><td>QFN28</td><td>蓝牙转双串口芯片。基本BLE虚拟化串口技术,实现蓝牙和两个串口之间的数据互传,兼容常规串口应用程序,无需二次开发,即连即用。</td></tr><tr><td>CH9143</td><td>QFN28</td><td>BLE/UART/USB三通芯片。基于USB和BLE虚拟化串口技术,实现蓝牙、USB和串口之间数据互传,无需二次开发,即连即用。</td></tr></table>

#### 典型应用 \Applications

智能家居

运动设备

传感检测

车载蓝牙

安防监控

手机连接

#### BLE模块及成品

BLE模块   

<table><tr><td>名称</td><td>说明</td><td>特点</td><td>实物图</td></tr><tr><td>BLE-SER-A-ANT</td><td>蓝牙转串口模块</td><td>板载PCB天线
体积小
内置32M晶体</td><td>10.2mm</td></tr><tr><td>BLE-TPT-A-ANT</td><td rowspan="2">蓝牙串口透传模块</td><td>板载PCB天线
体积小
内置32M晶体</td><td>10.2mm</td></tr><tr><td>BLE-TPT-E-ANT</td><td>板载PCB天线
体积小
功能引脚部分引出</td><td>16mm</td></tr><tr><td>BLE2U-A-ANT</td><td rowspan="2">BLE/UART/USB
三通模块</td><td>板载PCB天线
体积小
内置32M晶体</td><td>10.2mm</td></tr><tr><td>BLE2U-C-ANT</td><td>板载PCB天线
功能引脚全部引出
内置32M和32K晶体</td><td>18.02mm</td></tr></table>

BLE成品  

<table><tr><td>名称</td><td>说明</td><td>特点</td><td>实物图</td></tr><tr><td>CH585D</td><td>高速USB无线接收器</td><td>单芯片接收器,集成自研2.4G和高速USB,体积小巧,即插即用。搭配CH592可实现2~8k高回报率无线鼠标。</td><td>HCH</td></tr><tr><td>BLE232-NEP</td><td>无线RS232免供电转换器</td><td>支持低功耗蓝牙,兼容常规串口应用程序和串口调试工具,无需二次开发,实现无线串口和串口延长功能。</td><td>HCH
HCH</td></tr><tr><td>BLE-Dongle</td><td>无线串口接收器</td><td>支持低功耗蓝牙,兼容常规串口应用程序和串口调试工具,无需二次开发,实现PC USB转蓝牙。</td><td>HCH</td></tr></table>

### BLE Mesh 无线组网

### BLE Mesh无线组网方案

BLE Mesh是蓝牙官方组织推出的组网规范，为星型网状的多对多拓扑结构，网络中的设备可以相互通信。沁恒微BLE Mesh无线组网方案全面支持蓝牙Mesh Profile的各项特性，包括转发、代理、朋友以及低功耗，并通过蓝牙技术联盟官方认证以及阿里天猫精灵生态认证，适用于智能家电、智能照明、智能楼宇、智能机器人、智能穿戴设备等领域。

#### 快速接入互联网, 单芯片, 免编程

蓝牙设备网络快速接入互联网，符合低功耗蓝牙规范，可通过串口、蓝牙或网口配置，使用方便。

### 蓝牙以太网网关模块

![](images/001d8ac4620192155fb55426d9697587d503330524f7cacf5a5478b1350d57bd.jpg)  
应用框图 \ Block Diagram

#### 产品特点 \ Features

> 自发现、自连接、自组网  
>秒级配网，毫秒级控制延时  
> 提供安全、可靠、便捷的BLE Mesh开发包  
> 提供Mesh Model的绝大多数模型  
> 便于客户开发验证，提供以CH59X为主控的BLEMOD模组，该模组已通过SRRC认证以及阿里联盟生态认证

![](images/3b9a4ea30ed4c3666c815ccf5e2d92dec0d787f67f34e632a2d74d633c9a0877.jpg)  
开发套件 \ Development Hardware  
BLEMOD模组

![](images/921b987e7cfda9c5601133c97980139cd917dcad5f4c383515aa7cb26b10e5b1.jpg)  
BLEMOD EVT开发板

![](images/155112073d8cdcce0b602aa963a0e05b0df8b24c47fe22382f86eef5788f704d.jpg)  
应用框图 \ Block Diagram

#### 产品特点 \ Features

单芯片方案,无需编程  
> 符合低功耗蓝牙规范   
> 10M以太网口  
支持连接的蓝牙设备快速接入互联网  
> 支持蓝牙和以太网配置  
支持多路GPIO  
支持一路ADC采集，可以通过蓝牙读取  
支持一路UART,波特率300~921600bps  
支持MQTT等物联协议，支持云平台连接

### 典型应用 \Applications

物联传感器

数据监测

智能家居

智慧农业

工业生产

蓝牙入网

### 10/100M以太网PHY收发器

CH182系列是单芯片、单端口的以太网PHY收发器，支持10Base-T和100Base-TX及自动协商，提供MII和RMII两种接口，支持Auto-MDIX的TX/RX自动交换和正负信号线自动识别，I/O接口支持3.3V、2.5V、1.8V，适配多种电压的主控。CH182系列提供丰富的封装形式和引脚布局，其中CH182D采用QFN20封装，尺寸仅3*3mm。

### 10/100M以太网控制器芯片

#### ## 网口任意扩展

CH390系列是集成10/100M以太网MAC和物理层收发器PHY的工业级以太网控制器芯片，支持10BASE-T的CAT3、4、5和100BASE-TX的CAT5、6连接，支持HP Auto-MDIX，低功耗设计，符合IEEE802.3u规范。CH390内置16K字节SRAM，支持1.8V、2.5V、3.3V并行接口和SPI串行接口，兼容MCU、MPU、DSP等控制器和处理器。

![](images/4b848db45887b1f1a3c5191d8e94b64e2c6063fe5eb702075eb2654a2afe4c74.jpg)  
应用框图 \ Block Diagram

#### 产品特点 \ Features

> 支持100Base-TX和10Base-T  
> 支持自动协商  
> 支持Auto-MDIX  
> 自动识别正负信号线  
> 支持全/半双工操作  
> 支持MII、RMII两种模式  
> 支持网络唤醒(WOL)

> 支持中断功能  
> 支持停机模式  
> 支持两种网络状态LED  
> 内置LDO, 单一3.3V电源  
> 可选支持外部50MHz时钟输入  
> 支持25MHz的外部晶体或振荡器  
> 内置50Ω阻抗匹配电阻和晶体振荡器所需电容

选型指南 \Model Selection Guide  

<table><tr><td>型号</td><td>封装形式</td><td>塑体尺寸</td><td colspan="2">引脚节距</td></tr><tr><td>CH182D</td><td>QFN20</td><td>3*3mm</td><td>0.4mm</td><td>15.7mil</td></tr><tr><td>CH182H1</td><td>QFN32</td><td>4*4mm</td><td>0.4mm</td><td>15.7mil</td></tr><tr><td>CH182H2</td><td>QFN32X5</td><td>5*5mm</td><td>0.5mm</td><td>19.7mil</td></tr></table>

注：CH182D内置唯一MAC地址，建议优选小体积的CH182D或CH182H1。

CH182H2是CH182H（表中未列出）的升级版本，引脚兼容。

除上述型号，还提供CH182H3、CH182H6等客制引脚和封装形式

### 典型应用 \Applications

物联网

工控主板

交通服务

安防监控

![](images/af2aa85bce8377458357c33ab027e9fefca1c637af5cf685902efda26e225cdd.jpg)  
应用框图 \ Block Diagram

#### 产品特点 \ Features

> 内置MAC控制器和10/100M以太网PHY  
> 内置唯一MAC地址，无需另外购买或分配  
> 支持10Base-T和100Base-TX及自动协商  
> 支持Auto-MDIX的TX/RX交换, 自动识别正负信号线  
> 支持样本帧、链路状态变化和魔法包唤醒  
> 支持IEEE802.3x的流量控制  
> 支持IPv4/IPv6的TCP/UDP校验和的生成和检查  
> 内置50Ω阻抗匹配电阻和晶体振荡器所需电容  
> 支持可选的外部EEPROM配置芯片

选型指南 \Model Selection Guide  

<table><tr><td>芯片型号</td><td>接口类型</td><td>I/O独立供电</td><td>接口电压</td><td>封装形式</td><td>塑体尺寸</td><td>MAC地址</td></tr><tr><td>CH390D</td><td>SPI串行接口</td><td>-</td><td>3.3V</td><td>QFN20</td><td>3*3mm</td><td rowspan="4">均内置
唯一MAC地址
无需另购或分配</td></tr><tr><td>CH390F</td><td>8位并行接口</td><td>✓</td><td>1.8/2.5/3.3V</td><td>QFN28</td><td>4*4mm</td></tr><tr><td>CH390H</td><td>SPI串行接口</td><td>✓</td><td>1.8/2.5/3.3V</td><td>QFN32X5</td><td>5*5mm</td></tr><tr><td>CH390L</td><td>8位、16位并行接口</td><td>✓</td><td>2.5/3.3V</td><td>LQFP48</td><td>7*7mm</td></tr></table>

### 典型应用 \Applications

物联网

工控主板

交通服务

安防监控

### TCP/IP网络协议栈芯片

#### 让MCU轻松联网

CH395、CH394系列10/100M以太网协议栈芯片内置TCP/IP协议簇、以太网MAC和PHY,支持IPv4、UDP、TCP等常用以太网协议,MCU只需简单命令即可实现网络通讯,让嵌入式系统轻松联网。

![](images/2598a46def1b4e65a2d7000dbf30f0a1d484dd55996c3fb817107a1b2ead08d8.jpg)  
应用框图 \ Block Diagram

#### 产品特点 \ Features

> 内置TCP/IP协议簇,支持IPv4、ARP、ICMP、UDP、TCP协议  
> CH395支持DHCP自动获取IP地址  
> CH394支持IGMP协议  
> 网络协议命令化,MCU只需简单命令即可实现网络通讯  
> 内置10/100M以太网MAC和物理层收发器PHY  
> 全双工/半双工自适应  
> 支持MDI/MDIX线路自动切换,交叉/直连网线任意连接

> 支持多种MCU接口：8位被动并口、SPI、异步串口  
> I/O口支持1.8V、2.5V、3.3V, 兼容不同电压的主控  
> 提供最高8个独立的Socket对,支持同时数据收发  
> 内置最高32KB RAM,各Socket收发缓冲区可以自由配置  
> 提供评估板和常用MCU应用例程,有效缩短开发时间  
> 可提供TCP/IP协议栈定制服务  
> 支持MQTT等物联协议，支持云平台连接

选型指南 \ Model Selection Guide  

<table><tr><td>芯片型号</td><td>接口类型</td><td>Socket数量</td><td>数据收发RAM缓冲区</td><td>封装形式</td><td>塑体尺寸</td></tr><tr><td>CH395F</td><td>8位并口、串口、SPI</td><td>8</td><td>24KB</td><td>QFN32</td><td>4*4mm</td></tr><tr><td>CH394L</td><td>8位并口、SPI</td><td>4</td><td>16KB</td><td>LQFP48</td><td>7*7mm</td></tr><tr><td>CH394Q</td><td>SPI</td><td>8</td><td>32KB</td><td>LQFP48</td><td>7*7mm</td></tr></table>

注：CH395内置4K EEPROM，支持睡眠模式，支持8路GPIO，串口波特率支持动态调整。  
CH394支持网络唤醒(WOL)、掉电模式和LED状态显示。  
CH392为10M以太网协议栈芯片，支持SPI和串口连接，提供TSSOP20和QFN28封装。

### 典型应用 \ Applications

物联网

办公自动化

公共服务终端

城市交通管理

医疗保健

服务器管理

### 以太网串口透传芯片

#### 串口设备快速联网

CH9121系列以太网串口透传芯片内部集成TCP/IP网络协议栈和串口通讯固件，可轻松实现以太网和串口间的双向、透明数据传输，支持TCP/UDP的Client和Server模式，可大幅降低串口设备联网难度，缩短产品开发周期。

![](images/b99f956861c9dd8b943ddc0c514cbcc96e58887088d184b40911ad4eb76e8d46.jpg)  
应用框图 \ Block Diagram

#### 产品特点 \ Features

> 内置以太网TCP/IP协议栈和串口通讯固件  
> 内置10/100M以太网MAC和物理层收发器PHY  
> 支持10/100M以太网和串口间的双向、透明数据传输  
> 支持TCP/UDP的Client和Server模式  
> 支持KEEPALIVE机制  
> 支持DHCP自动获取IP地址,支持DNS域名访问  
> 支持全双工/半双工自适应，支持MDI/MDIX线路自动切换  
> I/O口支持1.8V、2.5V、3.3V, 兼容不同电压的主控

> 同时支持两路独立串口，独立透传，串口波特率最高可到10Mbps  
支持全双工和半双工串口通讯，支持RS485收发自动切换  
> 支持通过上位机软件、串口命令设置芯片工作模式、端口、IP等网络参数  
> 支持LED显示Link和ACT状态  
> 内置网口上拉电阻、晶振匹配电容，外部电路精简

选型指南 \ Model Selection Guide  

<table><tr><td>芯片型号</td><td>串口速率</td><td>接口电压</td><td>以太网规范</td><td>封装形式</td><td>塑体尺寸</td></tr><tr><td>CH9121T</td><td>最高10Mbps</td><td>1.8/2.5/3.3V</td><td>100M、10M</td><td>TSSOP20</td><td>4.4*6.5mm</td></tr><tr><td>CH9121A</td><td>最高10Mbps</td><td>1.8/2.5/3.3V</td><td>100M、10M</td><td>LQFP64</td><td>10*10mm</td></tr></table>

注：新设计建议使用CH9121T，支持硬件流控，封装更小，外围电路更精简。

### 其他协议栈芯片\Others

CH9126：基于SNTP协议的网络授时芯片。支持SNTP服务器和SNTP客户端模式，可以通过网络和串口配置芯片参数。

芯片内部还有一个独立的数据透传通道，可以实现以太网与串口数据透传。

### 典型应用 \Applications

智能家居

电力仪表

工业自动化

一卡通系统

公共服务终端

交通管理

### 串口转网络模块

### 串口和网络数据双向透明传输

无需修改原有串口设备通讯协议，可以快速实现串口设备联网功能。

### USB2.0百兆网卡芯片

CH397是符合USB2.0协议规范的USB转以太网芯片，内部集成了USB2.0 PHY及符合IEEE802.3协议规范、支持10M/100M网络的以太网MAC+PHY。具有高集成度、低功耗、易于使用等特点。


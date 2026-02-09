# WCH-Link 使用说明

版本：V2.6

https://wch.cn

## 1 WCH-Link

### 1.1 模块简介

WCH-Link 模块可用于沁恒 RISC-V 架构 MCU 在线调试和下载，也可用于带有 SWD/JTAG 接口的 ARM内核 MCU 的在线调试和下载。同时带有一路串口，方便调试输出。目前有 WCH-LinkE、WCH-DAPLink 和WCH-LinkW 三种产品，推荐选用 WCH-LinkE。

![](images/b2cbfcf8546e79c3dee614503b69504e7d6d6caa8333a22f1429ab30ad9e36f9.jpg)

![](images/09199fe5311b7612428df6df78b50e33879f4f00f18749cca7113b048aac21bb.jpg)

![](images/e5ff5564a469c1ae977de49e0f4b52039397e5fdd423986b0941a090b4c0216b.jpg)  
图一 WCH-Link 实物图

![](images/e9eb4e59511e3c796084088491e032c8ff228c1f3fcf964cc71945005381871e.jpg)  
图二 WCH-Link 模式

表一 WCH-Link 模式  

<table><tr><td>模式</td><td>状态指示灯</td><td>IDE</td><td>支持芯片</td></tr><tr><td>RISC-V</td><td>空闲时蓝灯常灭</td><td>MounRiver Studio</td><td>本公司支持单/双线调试的 RISC-V 核芯片</td></tr><tr><td>ARM</td><td>空闲时蓝灯常亮</td><td>Keil/MounRiver Studio</td><td>支持 SWD/JTAG 接口的 ARM 核芯片</td></tr></table>

### 1.2 模式切换

方式一：使用 MounRiver Studio 软件切换 Link 模式（该方式适用于 WCH-Link、WCH-LinkE 和 WCH-LinkW）

$\textcircled{1}$ 点击快捷工具栏中的 箭头，弹出工程下载配置窗口  
$\textcircled{2}$ 点击 Target Mode 右侧 Query,查看当前 Link 模式  
$\textcircled{3}$ 点击 Target Mode 选项框，选择目标 Link 模式，点击 Apply

![](images/8997b539b06ed54b44e41a54fbc54d8615da24d329afd363c31fdc4e7f2d908b.jpg)

方式二：使用 WCH-LinkUtility 工具切换 Link 模式

$\textcircled{1}$ 点击 Active WCH-Link Mode 右侧 Get，查看当前 Link 模式  
$\textcircled{2}$ 点击 Active WCH-Link Mode 选项框，选择目标 Link 模式，点击 Set

![](images/a02d3c38a68cce750dcae75cfed83ca896b035e3002113f7d8fa103115643e73.jpg)

方式三：使用 ModeS 键切换 Link 模式（该方式适用于 WCH-LinkE-R0-1v2、WCH-DAPLink-R0-2v0 和WCH-LinkW-R0-1v1 及以上版本）

$\textcircled{1}$ 长按 ModeS 键后将 Link 上电

注：

（1）下载和调试时，蓝灯闪烁。  
（2）后续使用时，Link保持切换后的模式。  
（3）扫描Link背面二维码，即可打开WCH-Link仿真调试器模块网址。  
(4）WCH-Link 仿真调试器模块网址：https://www.wch.cn/products/WCH-Link.html   
(5）MounRiver Studio 获取网址: https://mounriver.com/   
(6）WCH-LinkUtility 获取网址: https://www.wch.cn/downloads/WCH-LinkUtility_ZIP.html   
(7） WCHlSPStudio 获取网址: https://www.wch.cn/downloads/WCHISPTool_Setup_exe.html   
（8）WCH-Link、WCH-LinkE 和WCH-LinkW支持LinkRV和LinkDAP-WINUSB 模式切换；WCH-DAPLink 支持LinkDAP-WINUSB 和LinKDAP-HID 模式切换。

### 1.3 串口波特率

表二 WCH-Link 串口支持波特率  

<table><tr><td>1200</td><td>2400</td><td>4800</td><td>9600</td><td>14400</td></tr><tr><td>19200</td><td>38400</td><td>57600</td><td>115200</td><td>230400</td></tr></table>

表三 WCH-LinkE/DAPLink/LinkW 串口支持波特率  

<table><tr><td>1200</td><td>2400</td><td>4800</td><td>9600</td><td>14400</td><td>19200</td></tr><tr><td>38400</td><td>57600</td><td>115200</td><td>230400</td><td>460800</td><td>921600</td></tr></table>

注：  
（1）图一中排针RX和TX为串口收发引脚，串口支持波特率见上表。  
（2）Win7下需安装CDC驱动。  
（3）若重新拔插Link，需重新开启串口调试助手。

### 1.4 功能对比

表四 Link 功能和性能对比表  

<table><tr><td>功能项</td><td>WCH-Link-R1-1v1</td><td>WCH-LinkE-R0-1v3</td><td>WCH-DAPLink-R0-2v0</td><td>WCH-LinkW-R0-1v1</td></tr><tr><td>RISC-V模式</td><td>✓</td><td>✓</td><td>×</td><td>✓</td></tr><tr><td>ARM-SWD模式-HID设备</td><td>×</td><td>×</td><td>✓</td><td>×</td></tr><tr><td>ARM-SWD模式-WINUSB设备</td><td>✓</td><td>✓</td><td>✓</td><td>✓</td></tr><tr><td>ARM-JTAG模式-HID设备</td><td>×</td><td>×</td><td>✓</td><td>×</td></tr><tr><td>ARM-JTAG模式-WINUSB设备</td><td>×</td><td>✓</td><td>✓</td><td>✓</td></tr><tr><td>ModeS键切换模式</td><td>×</td><td>✓</td><td>✓</td><td>✓</td></tr><tr><td>两线方式离线升级固件</td><td>×</td><td>✓</td><td>×</td><td>×</td></tr><tr><td>串口离线升级固件</td><td>✓</td><td>×</td><td>×</td><td>×</td></tr><tr><td>USB离线升级固件</td><td>✓</td><td>×</td><td>✓</td><td>✓</td></tr><tr><td>3.3V/5V电源输出可控</td><td>×</td><td>✓</td><td>✓</td><td>✓</td></tr><tr><td>高速USB2.0转JTAG接口</td><td>×</td><td>✓</td><td>×</td><td>×</td></tr><tr><td>无线模式</td><td>×</td><td>×</td><td>×</td><td>✓</td></tr><tr><td>下载工具</td><td>MounRiver Studio
WCH-LinkUtility
Keil uVision5</td><td>MounRiver Studio
WCH-LinkUtility
Keil uVision5</td><td>WCH-LinkUtility
Keil uVision5</td><td>MounRiver Studio
WCH-LinkUtility
Keil uVision5</td></tr><tr><td>Keil支持版本</td><td>Keil V5.25及以上版本</td><td>Keil V5.25及以上版本</td><td>所有版本Keil都支持</td><td>Keil V5.25及以上版本</td></tr></table>

## 2 引脚连接

表五 Link 支持芯片型号  

<table><tr><td>常用芯片型号</td><td>WCH-Link</td><td>WCH-LinkE</td><td>WCH-DAPLink</td><td>WCH-LinkW</td></tr><tr><td>CH643/CH32X035_X033/CH32L103/CH32V003/CH32V002_004_005_006_007/CH32V317/CH32M007</td><td>×</td><td>✓</td><td>×</td><td>✓</td></tr><tr><td>CH32V10x/CH32V20x/CH32V30x</td><td>✓</td><td>✓</td><td>×</td><td>✓</td></tr><tr><td>CH569/CH573/CH583</td><td>✓</td><td>✓</td><td>×</td><td>×</td></tr><tr><td>CH59x/CH641/CH645/CH564/CH584_585_581/CH32M030/CH570_572/CH32H417_416_415/CH32V205_203CC/CH595_596</td><td>×</td><td>✓</td><td>×</td><td>×</td></tr><tr><td>CH32F10x/CH32F20x/CH579/支持 SWD 接口的友商芯片</td><td>✓</td><td>✓</td><td>✓</td><td>✓</td></tr><tr><td>支持 JTAG 接口的友商芯片</td><td>×</td><td>✓</td><td>✓</td><td>✓</td></tr></table>

表六 常用芯片引脚连接  

<table><tr><td>常用芯片型号</td><td>SWD10</td><td>SWCLK</td></tr><tr><td>CH569</td><td>PA11</td><td>PA10</td></tr><tr><td>CH579</td><td>PB16</td><td>PB17</td></tr><tr><td>CH573/CH583/CH59x/CH584_585_581</td><td>PB14</td><td>PB15</td></tr><tr><td>CH643/CH32X035_X033</td><td>PC18</td><td>PC19</td></tr><tr><td>CH32V003</td><td>PD1</td><td>-</td></tr><tr><td>CH641</td><td>PB0</td><td>-</td></tr><tr><td>CH645/CH570_572</td><td>PA0</td><td>PA1</td></tr><tr><td>CH32V10x/CH32V20x/CH32V30x/CH32F10x/CH32F20x</td><td>PA13</td><td>PA14</td></tr><tr><td>/CH32L103/CH32V317/CH32V205_203CC</td><td></td><td></td></tr><tr><td>CH564</td><td>PB10</td><td>PB11</td></tr><tr><td>CH32V002_004_005_006_007/CH32M007</td><td>PD1</td><td>PB3</td></tr><tr><td>CH32M030</td><td>PA3</td><td>PA2</td></tr><tr><td>CH32H417_415_416</td><td>PB9</td><td>PB8</td></tr><tr><td>CH595_596</td><td>PA12</td><td>PA13</td></tr></table>

注：  
（1） 其中CH564、CH32M030、 CH584_585_581、CH570_572、CH32V002_004_005_006_007，CH32V205_203CC,CH32H417_415_416,CH595_596和CH32M007支持单线(SWDI0)和两线(SWDI0-SWCLK）调试接口。  
（2）WCH-Link-R1-1v1已停产，官方依然维护，但不再新增功能。

表七 STM32F10xxx JTAG 接口引脚连接  

<table><tr><td>JTAG接口引脚名称</td><td>JTAG调试接口</td><td>引脚分配</td></tr><tr><td>TMS</td><td>JTAG模式选择</td><td>PA13</td></tr><tr><td>TCK</td><td>JTAG时钟</td><td>PA14</td></tr><tr><td>TDI</td><td>JTAG数据输入</td><td>PA15</td></tr><tr><td>TDO</td><td>JTAG数据输出</td><td>PB3</td></tr></table>

注：  
（1）Link最大支持线长：30cm，如下载过程不稳定，可尝试将下载速度调低。  
（2）JTAG 模式，WCH-LinkE-RO-1v3、WCH-DAPLink-RO-2vO硬件版本开始支持，之前硬件版本不支持。  
（3）WCH-LinkE高速版本仅针对 $C H 3 2 F 2 0 \times \mathrm { \it / C H 3 2 V 2 0 \times \mathrm { \it / C H 3 2 V 3 0 \times } }$ 进行提速。  
（4）CH569、CH579、CH573、CH583、 $\mathtt { C H 5 9 x }$ 、CH584585581，若要使用Link进行下载或调试，需使用官方ISP工具开启两线调试接口，使用时需注意Link模式。步骤如 $\tau$ ：

打开 WCHISPStud io 工具 待测芯片进入 BOOT 模式

需短接 HD0 和 GND， 通过 U 口上电；

·CH573/CH583/CH59x/CH584_585_581需长按Download 键，通过U口上电;

WCHISPStudio工具会自动弹出适配窗口 点击开启两线调试接口

## 3 Keil 下载与调试

### 3.1 设备切换

WCH-DAPLink 支持 ARM 模式-WINUSB 设备和 ARM 模式-HID 设备两种模式，可通过 WCH-LinkUtility工具（或长按 ModeS 键后将 Link 上电）切换两种设备模式。WCH-Link、WCH-LinkE 和 WCH-LinkW 仅支持 ARM 模式-WINUSB 设备模式。

![](images/b675974fcbc258a64bf1266095d2d0ee6526d3bb4dc07c78b0bf1ba04001532e.jpg)

表八 WCH-DAPLink 设备  

<table><tr><td>设备</td><td>支持 Link</td><td>Keil 支持版本</td></tr><tr><td>ARM 模式-WINUSB 设备</td><td>WCH-LinkWCH-LinkEWCH-DAPLINKCH-LinkW</td><td>Keil V5.25 及以上版本ARM-CMSIS V5.3.0 及以上版本</td></tr><tr><td>ARM 模式-HID 设备</td><td>WCH-DAPLINK</td><td>所有版本 Keil 都支持</td></tr></table>

注：

（1）WCH-Link、WCH-LinkE、WCH-DAPLink 和WCH-LinkW出厂默认为WINUSB设备模式。  
（2）WCH-DAPLink-RO0-1vO通过长按IAP键上电切换两种设备模式。  
（3）WCH-LinkUtility工具开启会占用Link设备，导致Kei丨软件无法识别到Link。

### 3.2 下载配置

$\textcircled{1}$ 点击工具栏中的 魔法棒，弹出 Options for Target 对话框，点击 Debug，选择仿真器型号

![](images/89a92caefe47ffb9936c37e957dfdb5998f876851fee284276746491170a31fa.jpg)

$\textcircled{2}$ 点击 Use 选项框，选择 CMSIS-DAP Debugger   
$\textcircled{3}$ 点击 Settings 按钮，弹出 Cortex-M Target Driver Setup 对话框

![](images/6b977e8292186050d2d98727a390607e9b397f5770d95619e203fe9c70780404.jpg)

Serial No：显示正在使用的调试适配器的标识符，当连接多个适配器时，可通过下拉列表来指定适配器。

SW Device：显示连接设备的设备 ID 和名称。

Port：设置内部调试接口 SW 或 JTAG。（WCH-LinkE-R0-1v3、WCH-DAPLink-R0-2v0 和 WCH-LinkW-R0-1v1 两种接口都支持）

Max Clock：设置与目标设备通信的时钟速率。

$\textcircled{4}$ 点击 Flash Download，进行下载配置

![](images/49bd836b3e19dc2bbb3c3ffa935fe2b8c194feb86498afc6e3de6f8efd80eaa4.jpg)

Download Function：配置选项

Erase Full Chip：全片擦除   
Erase Sectors：部分擦除

Do not Erase：不擦除   
Program：编程  
Verify：校验   
Reset and Run：复位后运行

RAM for Algorithm：配置 RAM 空间的起始地址和大小

我司 CH32F103 系列芯片 RAM 空间大小为 $0 \times 1 0 0 0$ ，CH32F20x 系列芯片 RAM 空间大小为 $0 \times 2 8 0 0$ 。

Programming Algorithm：添加算法文件

算法文件在安装芯片器件包之后已经自动添加，点击 OK 即可。

$\textcircled{5}$ 完成上述配置后，点击 OK，关闭对话框。点击工具栏中的 图标，即可进行代码烧录

### 3.3 调试

$\textcircled{1}$ 点击工具栏中的 调试按钮，进入调试页面  
$\textcircled{2}$ 设置断点

![](images/160ba30b8c8c8f904f2137fde6bb608007810a5426674e409b11b3528d13bedb.jpg)

$\textcircled{3}$ 基本调试指令

![](images/fa8127e63fb26a1b26b89df25b624f0550c05ab427dd7993b31f7e88e71f6a29.jpg)

复位：对程序进行复位操作。

![](images/b3314da6ab67d9c9383b3acc5d2f620402fc9452271ab376866fc7043635aefe.jpg)

全速运行：使当前程序开始全速运行，直到程序遇到断点时停止。

![](images/0557dc5e93a4050d3293c62574b0ff689fd66902506663f6ca1d8ebacddb40c1.jpg)

单步跳入调试：执行单条语句，如果遇到函数，则会进入函数内部。

$\{ \} ^ { \downarrow }$ 单步跳过调试：执行单条语句，遇到函数不会进入函数内部，而是全速运行函数，并跳到下一条语句。

![](images/4c2f6aee6e061c02dc5ced75a767f68bd258cc0835380e6a89dd6e7522042d57.jpg)

单步返回调试：全速运行当前函数后面所有内容，直到函数返回上一级。

$\textcircled{4}$ 再次点击工具栏中的 $\alpha$ 调试按钮，退出调试

## 4 MounRiver Studio 下载与调试

### 4.1 下载配置

$\textcircled{1}$ 点击工具栏中的 箭头，弹出工程下载配置窗口  
$\textcircled{2}$ 点击 Disable Read-Protect 按钮，解除芯片读保护

![](images/9eac94d970a07ae1207d0ffeec18e607f10840910c64192bcac0d9ff141d88e8.jpg)

$\textcircled{3}$ 目标配置，主要内容如下：

![](images/ede9aa6263f1da9be09a5491691c86477c95f50b92770d63b2aca586b9731f7d.jpg)

MCU Type：芯片型号

Program Address：编程地址

CLK Speed：两线调试速度

Target File：目标文件

$\textcircled{4}$ 配置选项

![](images/102d71cabb48d00989f3bd9beded446a728298f757de88dd07d75251d5226db9.jpg)

Erase All：全片擦除

Program：编程

Verify：校验

Reset and run：复位后运行

$\textcircled{5}$ 点击 Apply and Close，保存下载配置。点击工具栏中的 图标，即可进行代码烧录，结果显示在 Console 中

### 4.2 调试

$\textcircled{1}$ 进入调试页面

方式一：点击工具栏中的 $^ { * * }$ 调试按钮，直接进入调试页面。

方式二：点击工具栏中的 箭头，选择 Debug Configurations，弹出调试配置页面。双击 GDBOpenOCD MRS Debugging，生成 obj 文件，选择 obj 文件，点击右下角 Debug 按钮，进入调试页面。

![](images/ceed01b6e3f8feff38b56101e44f6e200212e746dc90b4c820439511dffeda12.jpg)

#### $\textcircled{2}$ 设置断点

![](images/267b9f875aee68dfe026c4377165da11392b501132afa9924adf573016d36a92.jpg)

#### $\textcircled{3}$ 基本调试指令

![](images/5996553b7f2088b3edecec0d3151e1c16dfe4eccadc1b427fc65617a1eb68fe3.jpg)

复位：对程序进行复位操作。

![](images/257e195925fdb286df9e393d881760c1d7029eb787973607f933c8781bd59542.jpg)

全速运行：使当前程序开始全速运行，直到程序遇到断点时停止。

![](images/88ca0730742029a3fbcf2d1b5e03a73bf8c6a96f024ba93df059521592ac75aa.jpg)

终止调试：退出调试。

![](images/562482de1095fdf95b990e5594fc4d63acb47241f1c782d6dd0d2e53ca2f706e.jpg)

单步跳入调试：执行单条语句，如果遇到函数，则会进入函数内部。

2 单步跳过调试：执行单条语句，遇到函数不会进入函数内部，而是全速运行函数，并跳到下一条语句。

![](images/c4d3f7a9eaa43a9f003dabb11adab5d66ece9357995fe2837f962fe28f6ce825.jpg)

单步返回调试：全速运行当前函数后面所有内容，直到函数返回上一级。

④ 点击 按钮，退出调试

### 4.3 其他功能

#### 4.3.1 设置芯片读保护

![](images/74027ef4c882f8e228f40e0757ec3c30867c66ac84a7d94b183b5b035d65f331.jpg)

查询芯片读保护状态

![](images/2bb92c862ade026930f7c9351b96504174df0756b92b3e85cf91367b8b09e28f.jpg)

使能芯片读保护状态

![](images/a585a346bc3c7a3cce89c0958fafc4df51110d536d698262a70520f6ad297411.jpg)

解除芯片读保护状态

#### 4.3.2 Code Flash 全擦

MounRiver Studio 可以通过控制硬件复位引脚或重新上电对芯片的用户区进行全部擦除。通过重新上电控制擦除，需使用 Link 为芯片供电；通过硬件复位引脚控制擦除，需连接芯片与 Link 的复位引脚。（仅 WCH-LinkE、WCH-DAPLink 和 WCH-LinkW 支持）

![](images/c0fa24022a0f1905ecf2a4b51b8f557825b64e560ec8bacbfcaa89162c21a19c.jpg)

#### 4.3.3 关闭两线调试接口

针对部分芯片，可通过关闭两线调试接口，开启代码和数据保护。

![](images/a8608ff8d86cfdecfae252ac874250cee5b26e6c45c199bdaf38e21b34ac5b8b.jpg)

禁止两线调试接口

### 4.3.4 芯片内存分配

针对大容量通用型（连接型/互联型/无线型）芯片，可通过 MounRiver Studio 进行内存分配，详情请参考 CH32FV2x_V3xRM 手册的用户选择字章节。

![](images/329347cddb31e12b651e251b087913b0c1bda78dd1b2d031715622e63085d0b1.jpg)

## 5 WCH-LinkUtility 下载

### 5.1 下载配置

$\textcircled{1}$ 点击 图标，连接 Link   
$\textcircled{2}$ 选择芯片型号

![](images/9d04227a613efbaa881239614ae184fef3e8bcc42baf032d812399e04580cdfc.jpg)

$\textcircled{3}$ 配置选项

![](images/558b05964a5ce27ac6f3cedb99ec6d62b1891d3d686316537e7685cc91cb64b3.jpg)

Erase All：全片擦除

Program：编程

Verify：校验

Reset and run：复位后运行

$\textcircled{4}$ 解除芯片读保护，设置两线调试速度

O Enable MCU Code Read-Protect O Disable MCu Code Read-Protect

CLK Speed: High

自$\textcircled{5}$ 点击 图标，添加固件  
$\textcircled{6}$ 点击 图标，执行下载

### 5.2 其他功能

#### 5.2.1 查询芯片信息

点击 图标，查询芯片信息

<table><tr><td>Name</td><td>Value</td></tr><tr><td>MCU UID</td><td>17-9f-ab-cd-7f-b4-bc-48</td></tr><tr><td>Flash Size</td><td>16 KB</td></tr><tr><td>Read-ProTECT</td><td></td></tr><tr><td>Link Version</td><td>V2.8</td></tr><tr><td></td><td></td></tr></table>

MCU UID：芯片 ID

Flash Size：芯片 Flash 大小

Read-Protect：芯片读保护状态

Link Version：Link 固件版本

#### 5.2.2 设置芯片读保护

![](images/7de7d914aa4d8be8ad70c79a2d79db0b19643c0750ed3502c35d011e563269b7.jpg)

查询芯片读保护状态

![](images/bb20939db81f2de990f2cbd149b4ac5db544af4f5620cd3ecb4d51e0a4398011.jpg)

使能芯片读保护状态

![](images/382137f089b269ee36a76457930e317d720847715669bd98afef6233cca9f9fa.jpg)

解除芯片读保护状态

#### 5.2.3 读取芯片 Flash

点击 Flach 图标，读取芯片 Flash

#### 5.2.4 Code Flash 全擦

WCH-LinkUtility 工具可以通过控制硬件复位引脚或重新上电对芯片的用户区进行全部擦除。通过重新上电控制擦除，需使用 Link 为芯片供电；通过硬件复位引脚控制擦除，需连接芯片与 Link 的复位引脚。（仅 WCH-LinkE、WCH-DAPLink 和 WCH-LinkW 支持）

![](images/a19863a76f0551abd7c638007c64bbef55007df21de01e61d37072ee60eeae33.jpg)

#### 5.2.5 电源输出可控

WCH-LinkUtility 工具可控制 Link 电源输出。点击 Target，在下拉列表中可选择开启/关闭电源3.3V/5V 输出。（仅 WCH-LinkE、WCH-DAPLink 和 WCH-LinkW 支持）

![](images/272477f66ca78f1180b88b43cd199f902a344ecb32a825dc3141795d9c67d185.jpg)

#### 5.2.6 自动连续下载

勾选 Auto download when WCH-Link was linked，可实现工程自动连续下载。

![](images/aefa0482b83029a350c92b169fef1e85c5b54cde30ef3148d069d3ec9cc338f3.jpg)

#### 5.2.7 多设备下载

WCH-LinkUtility 工具可以识别多个 Link 设备。当连接多个 Link 时，可通过 Connected WCH-Link List 选项框选择指定 Link 设备进行下载。

![](images/9d59c4e26c5c661a373fd1acc0165ae7a921758fd27107c41c744cfcdcaacb2f.jpg)

#### 5.2.8 关闭两线调试接口

针对部分芯片，可通过关闭两线调试接口，开启代码和数据保护。

![](images/0000ae5a2e8b6e1ae3ef2dfc76e8eaf6f0dc9f917970a997a9a9e316bf2cbbbd.jpg)

#### 5.2.9 用户选择字配置

针对 CH32 系列芯片，可通过 WCH-LinkUtility 工具进行用户选择字配置，详情请参考芯片 RM 手册的用户选择字章节。

![](images/4d423361d0b023d81cb5725fe7abb58e8f20758a6648311468af22af814f495a.jpg)

![](images/e75587049f5bb2738aa1e5429459c852f2e72ef267918b5b52a7ecacde5b9a58.jpg)

#### 5.2.10 BOOT 下载

针对 CH32V003、CH641、CH32V002_004_005_006_007、CH32M007、CH32X035_X033 芯片，可通过WCH-LinkUtility 工具选择程序下载到程序闪存存储区或系统存储区。

![](images/fca9566e375360343f2d3f2fc39c33d698e15d06818d3c72520a74cb48a3ea24.jpg)

### 5.2.11 SDI 虚拟串口功能

该功能使用 SDI 接口，实现芯片打印输出功能，该功能需修改打印函数，具体参考相关 EVT 中SDI_Printf 例程。该功能仅 V1.80 及以上版本支持，使用时需勾选 Enable SDI Printf,且打开 WCH-LinkE 的 COM 口。

![](images/a49a136ec5090caf7f1307e9e7beb3795f240259eed54c1381236d5903ae459d.jpg)

注：

（1）该功能仅V1.80及以上版本支持。  
（2）该功能仅 WCH-LinkE支持。  
(3） 该功能支持芯片包括CH32V003、CH32V103、CH32V20x、CH32V30×、CH32X035 X033、CH32L103、CH641、CH32V002_004_005_006_007、CH32M007、CH32M030、CH32V317，CH32V205_203CC。

## 6 固件更新方式

### 6.1 MounRiver Studio 在线更新

若固件需升级，点击下载按钮时 MounRiver Studio 会有弹窗提醒，点击 Yes 启动更新。

![](images/728189fecedabe65985aedddda0409d1c1d067cdb0bfe3062e25c6ef59c57430.jpg)

### 6.2 WCH-LinkUtility 在线更新

若固件需升级，点击下载按钮时 WCH-LinkUtility 会有弹窗提醒，点击 Yes 启动更新。

![](images/bd0ab50dd8aa192a6e0336ae3d23682697779e4f874e2fdacad2f32fa05de80e.jpg)

注：

（1）WCH-LinkE支持手动在线更新，步骤如下：

长按 IAP键后将Link上电 直至蓝灯闪烁；  
$\bullet$ 点击下载按钮时 MounRiver Stud io/WCH-Li nkUt i l ity 会有弹窗提醒 点击 Yes 启动更新

（2）若Link固件更新异常，请通过离线更新方式更新固件。

### 6.3 WCH-LinkUtility 离线更新（两线方式离线更新）

$\textcircled{1}$ 连接 WCH-LinkE 与待更新 WCH-LinkE

<table><tr><td>WCH-LinkE</td><td>待更新 WCH-LinkE</td></tr><tr><td>3V3</td><td>3V3</td></tr><tr><td>GND</td><td>GND</td></tr><tr><td>SWDIO</td><td>SWDIO</td></tr><tr><td>SWCLK</td><td>SWCLK</td></tr></table>

$\textcircled{2}$ WCH-LinkE 上电，选择待更新 Link 芯片型号（WCH-LinkE 主控芯片为 $\mathsf { C H 3 2 V 3 0 x } .$ ）  
$\textcircled{3}$ 待更新 Link 进入 IAP 模式（长按 IAP 键后将 Link 上电，即通过 USB 口连接电脑上电）  
$\textcircled{4}$ 点击 Target->Clear All Code Flash-By Power off，对芯片的用户区进行全部擦除

![](images/8c490616fe531f6dc3ee039bc793763c7aa6495ec3b13a06ebbd7a38c105ccbb.jpg)

![](images/6b2a3c52f5320ab221e8c701691fcc701c7bbebbc2237b141d984190e14293e6.jpg)

点击 图标，解除芯片读保护

![](images/3b709ae8b60fbc4ce0cb04c9f30af65176b3570879cef8e4b956a4603f7254fb.jpg)

$\textcircled{6}$ 点击 图标，添加 Link 离线升级固件  
$\textcircled{7}$ 配置选项（编程+校验 $^ +$ 复位）

![](images/55c0b7e60eaf4af489afd8acaf603ed06cc303c78c18d17a384c1450a1ed32ee.jpg)

$\textcircled{8}$ 点击 ? 图标，执行下载

注：

（1）两线方式离线更新仅WCH-LinkE支持。  
（2）该方式需要有两个WCH-LinkE。  
（3）Link进入IAP 模式时，蓝灯闪烁。

### 6.4 WCHISPStudio 串口离线更新

$\textcircled{1}$ 连接 WCH-Link 与 USB 转 TTL 模块

<table><tr><td>WCH-Link</td><td>USB转TTL模块</td></tr><tr><td>TX</td><td>RX</td></tr><tr><td>RX</td><td>TX</td></tr><tr><td>GND</td><td>GND</td></tr></table>

$\textcircled{2}$ USB 转 TTL 模块上电，WCH-Link 进入 BOOT 模式（短接图一中 J1 将 Link 上电）  
$\textcircled{3}$ 选择芯片型号：CH549，下载接口：串口，设备列表：选择 USB 转 TTL 模块对应的串口号

![](images/f147d980da1edbfbaf56601a89c5403b85d7a07b4679f626f629b73b5f2f28dd.jpg)

$\textcircled{4}$ 添加 Link 离线升级固件至目标程序文件  
$\textcircled{5}$ 下载配置

$\textcircled{6}$ 点击下载按钮   
$\textcircled{7}$ 点击下载后出现等待设备接入字段，此时将 WCH-Link 插上 USB 接口，ISP 工具自动开始下载注：串口离线更新仅WCH-Link支持。

### 6.5 WCHISPStudio USB 离线更新

$\textcircled{1}$ 待更新 Link 进入 BOOT 模式（短接图一中 J1 或长按 BOOT 键后将 Link 上电）  
$\textcircled{2}$ WCHISPStudio 工具会自动弹出适配窗口  
$\textcircled{3}$ 添加 Link 离线升级固件至目标程序文件  
$\textcircled{4}$ 下载配置

![](images/ad068388a9c899e2e93452c206757999580d03070cca7fc331af3c2c82cd2390.jpg)

![](images/9df2c37183eb0629992bc04f5e7ba250c4b150ebbcf77266d30e9d8f392f82fd.jpg)

![](images/d604f318737131b8bc7c2d7308c3315fc7e32dcfb87e4963b532c1c3f72245e7.jpg)

#### $\textcircled{5}$ 点击下载按钮

### 注：

（1）USB 离线更新仅WCH-Link、WCH-DAPLink 和WCH-LinkW支持。  
（2）WCH-LinkE-R0-1v3和WCH-DAPLink-R0-2v0 仅适用固件版本 ${ \boldsymbol { v } } 2 . 8$ 及以上版本。  
(3）WCH-LinkUtility工具可通过MounRiver Studio软件导出。

![](images/7864f7e1cb929688dbfc1b0c5437294b554e2fcbef5ff5c63b891ebed2b6a70f.jpg)

（4）Link离线升级固件位于MounRiver Studio安装路径和WCH-LinkUtility安装路径。

![](images/786f2923a5b071f2f137866aad14f9a6a67b3c8d3973e59b58b2477ae245bffe.jpg)

$\textcircled{1}$ WCH-DAPLink升级固件  
$\textcircled{2}$ WCH-LinkW升级固件  
$\textcircled{3}$ WCH-LinkE升级固件  
$\textcircled{4}$ WCH-LinkRISC-V模式升级固件  
$\textcircled{5}$ WCH-LinkARM模式升级固件  
$\textcircled{6}$ WCH-DAPLink离线升级固件  
$\textcircled{7}$ WCH-LinkARM模式离线升级固件  
$\textcircled{8}$ WCH-Link RISC-V模式离线升级固件  
$\textcircled{9}$ WCH-LinkE离线升级固件  
$\textcircled{10}$ WCH-LinkW离线升级固件

## 7 WCH-LinkE 高速 JTAG

### 7.1 模块简介

WCH-LinkE-R0-1v3 提供了一个 JTAG 接口，支持四线连接（TMS 线，TCK 线，TDI 线和 TDO 线），用于为计算机扩展 JTAG 接口，操作 CPU、DSP、FPGA 和 CPLD 等器件。

![](images/54542d3e629019147de0a2c3aaf61b9503c3d54741f258d600f23dba10cf1208.jpg)  
图三 WCH-LinkE 高速 JTAG 模式

### 7.2 模块特点

作为 Host/Master 主机模式。  
$\bullet$ JTAG 接口提供 TMS 线、TCK 线、TDI 和 TDO 线。  
$\bullet$ 支持高速 USB 数据传输。  
通过计算机 API 配合，可灵活操作 CPU、DSP、FPGA 和 CPLD 等器件。

### 7.3 模式切换

可通过 WCHLinkEJtagUpdTool 工具将 WCH-LinkE-R0-1v3 升级为高速 JTAG 模式，下载步骤如下：

$\textcircled{1}$ WCH-LinkE-R0-1v3 进入 IAP 模式（长按 IAP 键后将 Link 上电，即通过 USB 口连接电脑上电），此时蓝灯闪烁  
$\textcircled{2}$ 打开 WCHLinkEJtagUpdTool 工具，执行下载（WCH-LinkE 高速 JTAG 升级固件已自动添加）  
$\textcircled{3}$ 固件更新完成后，此时蓝灯常亮

![](images/2ce66b71d597e255ac0620e59182387d58f68dbf7ef8a2283f85589af44a4351.jpg)

注：

(1） WCHLinkEJtagUpdTool 获取网址:https://www.wch.cn/downloads/WCHLinkEJtagUpdTool ZIP.html

(2）可通过WCH-LinkUtility工具离线更新固件，详情请参考手册6.3WCH-LinkUtility离线更新。  
（3）WCH-LinkE高速JTAG离线升级固件位于WCHLinkEJtagUpdTool安装路径。

WCHLinkEJtagUpdTool $\rightharpoondown$ FirmwareList  
名称 $①$ FIMWARE_USB_JTAG_CH32V305.bin $②$ WCH-LinkE-USB-JTAG-APP-IAP.bin

$\textcircled{1}$ WCH-LinkE高速JTAG升级固件  
$\textcircled{2}$ WCH-LinkE高速JTAG离线升级固件

### 7.4 下载流程

$\textcircled{1}$ 在 WCH-LinkE 高速 JTAG 模式下，通过 JTAG 先将 Bit 程序文件下载到 FPGA 中，Bit 文件将操作 FPGA 的 SPI 控制器，将 JTAG 数据转换为 SPI 数据写入 Flash，此步骤即将 BIN 文件写入，从而实现其程序固化过程  
$\textcircled{2}$ 此处选用 FPGA 为 Xilinx 的 xc7a35t。编写 CFG 文件，使用“openocd -f”指定来调用。将CFG 文件命名为 usb20jtag.cfg，保存至 openocd.exe 文件所在位置

# 指定 WCH-LinkE 高速 JTAG 调试器

adapter driver ch347

ch347 vid_pid 0x1a86 0x55dd

# 设置 TCK 时钟频率

adapter speed 10000

# 指定操作的 TARGET，加载 OpenOCD 中 JTAG-SPI 驱动

source [find cpld/xilinx-xc7.cfg]

source [find cpld/jtagspi.cfg]

# 设置 TARGET 的 IR 命令

set XC7_JSHUTDOWN $0 \times 0 { \mathsf { d } }$

set XC7_JPROGRAM $0 \times 0 \mathrm { b }$

set XC7_JSTART 0x0c

set XC7_BYPASS 0x3f

# 下载流程

init

# 先下载 Bit 文件至 TARGET

pld load 0 bscan_spi_xc7a35t.bit

reset halt

# 检测 Flash 信息

flash probe 0

# 下载 Bin 文件至 Flash 中

flash write_image erase test.bin $0 { \times } 0$ bin

# 生效固件操作

irscan xc7.tap $XC7_JSHUTDOWN

```tcl
ircscan xc7.tap $XC7_JPROGRAM  
runtest 60000  
runtest 2000  
ircscan xc7.tap $XC7_BYPASS  
runtest 2000  
exit 
```

$\textcircled{3}$ 在 Windows 终端运行命令：openocd.exe -f usb20jtag.cfg，执行如下

```txt
D:\MounRiver\MounRiver_Studio\toolchain\OpenOCD\bin>openocd.exe -f usb20jtag.cfg   
Open On-Chip Debugger 0.11.0+dev-02415-gfd123a16-dirty (2022-12-13-09:38)   
Licensed under GNU GPL v2   
For bug reports, read http://openocd.org/doc/doxygen/bugs.html   
Info: only one transport option; autoselect 'jtag'   
Info: clock speed 10000 kHz   
Info: JTAG tap: xc7. tap tap/device found: 0x0362d093 (mfg: 0x049 (Xilinx), part: 0x362d, ver: 0x0) [xc7. proxy] Target successfully examined.   
Info: JTAG tap: xc7. tap tap/device found: 0x0362d093 (mfg: 0x049 (Xilinx), part: 0x362d, ver: 0x0)   
Info: Found flash device 'issi is251p128d' (ID 0x18609d)   
Info: sector 0 took 0 ms   
Info: sector 1 took 0 ms   
Info: sector 2 took 0 ms   
Info: sector 3 took 0 ms   
Info: sector 4 took 0 ms   
Info: sector 5 took 0 ms   
Info: sector 6 took 0 ms   
Info: sector 7 took 15 ms   
Info: sector 8 took 0 ms   
Info: sector 9 took 0 ms   
Info: sector 10 took 0 ms   
Info: sector 11 took 0 ms   
Info: sector 12 took 0 ms   
Info: sector 13 took 16 ms   
Info: sector 14 took 0 ms   
Info: sector 15 took 0 ms   
Info: sector 16 took 0 ms   
Info: sector 17 took 0 ms   
Info: sector 18 took 0 ms   
Info: sector 19 took 16 ms   
Info: sector 20 took 0 ms   
Info: sector 21 took 0 ms   
Info: sector 22 took 0 ms   
Info: sector 23 took 0 ms   
Info: sector 24 took 0 ms   
Info: Close the CH347. 
```

$\textcircled{4}$ 下载结束，设备正常运行

注：

(1）转换作用的 Bit 文件，可借助 Github 开源项目：https://github.com/quartiq/bscan_spi_bitstreams  
(2）openocd.exe 文件所在位置:MounRiver\MounRiver_Studio\toolchain\Open0CD\bin

## 8.WCH-LinkW 使用介绍

### 8.1 模块简介

WCH-LinkW 是一款有线/无线 2.4G 双模仿真调试器，可用于沁恒 RISC-V 架构 MCU 在线调试和下载，也可用于带有 SWD/JTAG 接口的 ARM 芯片在线调试和下载。

### 8.2 使用方法

#### 8.2.1 有线模式

有线模式仅需要 1 个 WCH-LinkW，将排针连接 MCU，USB 口连接电脑，即可进行下载调试。

![](images/ecea7f0ec03068a114888f27d38a4f41e176455810a64bcdf08ff9667affb786.jpg)

#### 8.2.2 无线模式

无线模式需要 2 个 WCH-LinkW，分为 WCH-LinkW 主机（连接电脑）和 WCH-LinkW 从机（连接 MCU）。WCH-LinkW 连接电脑枚举成功后，在 2 秒内检测是否有从机匹配，来进行模式切换，若匹配到从机，则切换至无线模式，此时绿灯点亮；反之，则切换至有线模式，绿灯熄灭。

无线模式下载调试需要用到 2 个 WCH-LinkW，使用步骤如下：

$\textcircled{1}$ 从机与 MCU 通过两线方式连接，通过 MCU 的 U 口供电，也可通过充电头或移动电源与从机 USB口连接，对从机和 MCU 进行供电  
$\textcircled{2}$ 待从机上电成功后，主机 USB 口连接电脑。主机设备枚举成功后，2 秒内匹配到从机，则主机和从机绿灯点亮  
$\textcircled{3}$ 无线模式主机和从机匹配成功后，主机和从机都断电则需重复上述步骤，若仅 1 个断电，可重新上电进行自动匹配，不必重复上述流程  
$\textcircled{4}$ 进行 MCU 下载调试

![](images/086485e30a537ee16d9e16b542a0208d9a621829b5271897bbf10b291ec5cac1.jpg)  
注：使用CodeFlash全擦和电源输出可控功能需通过充电头或移动电源与从机USB口连接，对从机和MCU进行供电。

### 8.3 无线模式访问地址匹配

WCH-LinkW 使用无线模式进行下载调试时，需保证主机和从机的无线模式访问地址一致。可通过WCH-LinkUtility 工具设置无线模式访问地址。步骤如下：

![](images/7a53a414c0fe984687aab7e3b47a2eba7cdd719a776db4ec5f5c8f43eeff17d2.jpg)

$\textcircled{1}$ 分别将 WCH-LinkW 无线模式主机和从机连接电脑，点击 GET 按钮，查询当前主机和从机的访问地址是否一致，若一致且不为 $0 { \times } 0 . 3 3 9 5 3 3 9$ ，执行步骤 4，否则执行步骤 2  
$\textcircled{2}$ 点击 CREATE 按钮，创建随机地址  
$\textcircled{3}$ 点击 SET 按钮，分别设置主机和从机的访问地址  
$\textcircled{4}$ 从机连接 MCU 先上电，之后主机连接电脑，绿灯点亮即可正常使用无线模式

### 注：

（1）WCH-LinkW无线模式访问地址出厂默认为OxE339E339。  
（2）无线模式时，同一使用环境下，需保证仅有一对WCH-LinkW无线访问地址一致。若有多对设备，需使用上述步骤设置不同的无线模式访问地址。

## 9 典型问题说明

### 错误提醒

#### 使用 Keil 软件下载

![](images/a1843c7ed28e74e15e493350376175a35cf6af4b69622bcdbca111a939659256.jpg)

### 解决方法

1.请参考手册3.2下载配置完成Keil下载配置。

![](images/8b8549e557b54a4cdafac32b6214a5eec33537aec329b0b2ccd328389f44bb9f.jpg)

#### 使用 Keil 软件下载

![](images/e99ad7577bd46d469759e928b943a2466d700be1a6d7b99f7810cc2cf2cf4b01.jpg)

1.我司 CH32F20x 系列芯片 RAM 空间大小为$0 \times 2 8 0 0$ 。

![](images/2fd2192ffa091048bb8bc1309886439f85f4bc9b63a1a5be9d02917e6d72f6bf.jpg)

#### 使用 MounRiver Studio 软件下载

<table><tr><td>Download Output Console</td></tr><tr><td>14:03:21:759 &gt;&gt; Attempt to open link device and upgrade firmware if necessary...</td></tr><tr><td>14:03:21:896 &gt;&gt; Link Device is CH32V305. Already the latest version v2.8, no need to upgrade</td></tr><tr><td>14:03:21:930 &gt;&gt; Starting to Send Chip Type...</td></tr><tr><td>14:03:21:985 &gt;&gt; Board chip status error</td></tr><tr><td>Board chip status error</td></tr><tr><td>14:03:21:985 &gt;&gt; Starting to Close Link...</td></tr><tr><td>14:03:21:998 &gt;&gt; Close Link Success</td></tr><tr><td>End</td></tr></table>

1.检查芯片两线调试接口与Link连接是否正确；  
2.检查芯片的 Debug 功能是否开启（若未开启，可通过 ISP 工具开启）；  
3.检查芯片内用户程序是否开启睡眠功能，是否有操作 FLASH 相关函数（若开启，可进 BOOT 模式，通过两线下载）；  
4.检查芯片内用户程序的两线调试接口是否复用为普通 GPIO 口（若复用，可进 BOOT 模式，通过两线下载）。

### 注：

（1）对于CH32系列芯片，若出现下载不成功的情况，可进 BOOT模式（BOOTO接VCC、BOOT1接GND)，通过Link进行下载。  
（2）对于3和4的问题，可通过对芯片的用户

<table><tr><td></td><td>区进行全部擦除解决(具体参考手册4.3.2 Code Flash全擦或5.2.4 Code Flash全擦)。</td></tr><tr><td>使用WCH-LinkUtility工具下载</td><td>对芯片的用户区进行全部擦除</td></tr><tr><td>Connected WCH-Link List: RISC-V Link [#1] Refresh Get Set Operation Result: Result Collect: Succ:0 | Total:0 Clear</td><td>File Target View Connect WCH-Link Disconnect Query Chip Info Erase Chip F9 Program F10 Verify F11 Reset F12 Query Chip R-Protect Status Enable Chip R-Protect Disable Chip R-Protect Query Flash QE Status Enable Flash QE Clear All Code Flash-By Pin NRST Clear All Code Flash-By Power off</td></tr><tr><td>14:08:14:292&gt;&gt; Begin to set chip type... 14:08:14:354&gt;&gt; Failed, the chip type is not matched or status of chip is wrong!</td><td rowspan="2">1.分析原因,可能是WCH-LinkE-R0-1v3上的Y1晶振焊接异常,导致晶振无法正常起振。因此需重新焊接下Y1晶振。</td></tr><tr><td>使用WCHLinkEJtagUpdTool工具更新固件,按照手册7.3模式切换下载步骤更新固件后,WCH-LinkE-R0-1v3上蓝灯不亮,设备管理器无法识别设备。</td></tr><tr><td>使用WCH-LinkW无线模式进行下载调试时,绿灯不亮。</td><td>1.请参考手册8.2.2章节进行操作。</td></tr><tr><td>使用WCH-LinkW无线模式进行下载调试时,Code Flash全擦和电源输出可控功能不可用。</td><td>1.使用上述功能需通过充电头或移动电源与从机USB口连接,对从机和MCU进行供电。</td></tr><tr><td>使用WCH-LinkW无线模式进行下载调试时,从机无法在线升级固件。</td><td>1.WCH-LinkW从机在线升级固件需在有线模式下进行。</td></tr></table>

注：

（1）用户程序开启睡眠功能时，不支持调试功能。  
（2）若使用debug功能时异常退出，建议重新拔插Link。  
（3）使用 CH32F103/CH32F203/CH32V103/CH32V203/CH32V307/CH32L103/CH32V317/CH32V205 203CC的下载和调试功能时，B0OTO需接地。  
（4）使用CH569的调试功能时，用户代码必须小于配置的 ROM空间，具体见CH569DS1手册表2-2。  
（5）使用CH32系列芯片的调试功能时，请确保芯片处于读保护关闭状态。  
(6）WCH-Link 典型常见问题可参考:https://www.wch.cn/bbs/thread-100647-1.html

## 10 驱动安装

### 10.1 WCH-Link 驱动

（1）安装 MounRiver Studio 时会自动安装 WCH-Link 驱动，安装成功后设备管理器如下表所示，如果驱动安装失败，请打开 MounRiver Studio 安装路径下的 LinkDrv 文件夹，手动安装 WCHLink 文件夹下的 SETUP.EXE。  
（2）安装 WCH-LinkUtility 工具需手动安装 WCH-Link 驱动，请打开 WCH-LinkUtility 工具文件目录的 Drv_Link 文件夹，手动安装 WCHLinkDrv_WHQL_S.exe。

设备管理器

驱动路径

![](images/8532d0376c7782b87c34795bc78579edcf796cbf69bb04b01ae901c6d662b857.jpg)

![](images/e7080cb2e970693b7f38d30a8036f2aaaf94f7431ea11d15e86f6b1ca3ec953f.jpg)

### 10.2 WCH-LinkE 高速 JTAG 驱动

WCH-LinkE-R0-1v3 升级为高速 JTAG 模式，需手动安装 WCH-LinkE 高速 JTAG 驱动才能正常使用。请打开 WCHLinkEJtagUpdTool 安装路径下的 Drv 文件夹，手动安装 CH341PAR.EXE。

![](images/cff2ce57bd6b82da989bc2b66e74c485d45fda01d590454ac26aa9a1a6182d3f.jpg)

### 10.3 CDC 驱动

WIN7 下 CDC 设备安装问题：

$\textcircled{1}$ 若串口驱动安装成功，则无需以下步骤  
$\textcircled{2}$ 确认路径 B 中是否有 usbser.sys 文件，如果缺失，从路径 A 中将其复制到路径 B  
$\textcircled{3}$ 重新安装 CDC 驱动(驱动路径见上表，请安装对应模式下的 CDC 驱动)

![](images/b0ba4cbe8ae03cdbd813ca1e3e20634d783bc3339cc1b76a69a4ecca90cb1b59.jpg)

注：若上述步骤不能解决问题，请参考下方链接

![](images/ea7847e2cd90cc071e66b630db0f4306b6cc7a68878c4aa40f0cdd0c2d05938e.jpg)

参考资料: https://www.wch. cn/downloads/InstalINote0n64BitWIN7_ZH_PDF.html
# 第 23 章 USB 全速主机/设备控制器（OTG_FS/USBFS）

本章模块描述适用于CH32F2x、CH32V2x和CH32V3x微控制器系列部分产品。

## 23.1 USB 控制器简介

芯片内嵌 USB 控制器及收发器，特性如下：

双重角色设备控制器，支持 USB Host 主机功能和 USB Device 设备功能。  
遵循 On-The-GoSupplement to the USB2.0 规范，主机和设备模式均支持 USB2.0 全速 12Mbps或低速 1.5Mbps。  
支持软件 HNP 和 SRP 协议。  
$\bullet$ 支持 USB 控制传输、批量传输、中断传输、同步/实时传输。  
支持最大 64 字节的数据包，内置 FIFO，支持中断和 DMA。

## 23.2 寄存器描述

USB 相关寄存器分为 3 个部分，部分寄存器是在主机和设备模式下进行复用的。

USB全局寄存器  
$\bullet$ USB设备控制寄存器  
USB主机控制寄存器

### 23.2.1 全局寄存器描述

表 23-1 USBFS 相关寄存器列表  

<table><tr><td>名称</td><td>访问地址</td><td>描述</td><td>复位值</td></tr><tr><td>R8_USB_CTRL</td><td>0x50000000</td><td>USB 控制寄存器</td><td>0x06</td></tr><tr><td>R8_USB_INT_EN</td><td>0x50000002</td><td>USB 中断使能寄存器</td><td>0x00</td></tr><tr><td>R8_USB_DEV_AD</td><td>0x50000003</td><td>USB 设备地址寄存器</td><td>0x00</td></tr><tr><td>R32_USB_STATUS</td><td>0x50000004</td><td>USB 状态寄存器</td><td>0xXX20XXXX</td></tr><tr><td>R8_USB_MIS_ST</td><td>0x50000005</td><td>USB 杂项状态寄存器</td><td>0xXX</td></tr><tr><td>R8_USB_INT_PG</td><td>0x50000006</td><td>USB 中断标志寄存器</td><td>0x20</td></tr><tr><td>R8_USB_INT_ST</td><td>0x50000007</td><td>USB 中断状态寄存器</td><td>0xXX</td></tr><tr><td>R16_USB_RX_LEN</td><td>0x50000008</td><td>USB 接收长度寄存器</td><td>0xXX</td></tr><tr><td>R32_USB_OTG_CR</td><td>0x50000054</td><td>USB OTG 控制寄存器</td><td>0x00</td></tr><tr><td>R32_USB_OTG_SR</td><td>0x50000058</td><td>USB OTG 状态寄存器</td><td>0x00</td></tr></table>

23.2.1.1 USB 控制寄存器（R8_USB_CTRL）  

<table><tr><td>位</td><td>名称</td><td>访问</td><td>描述</td><td>复位值</td></tr><tr><td>7</td><td>RB_UC_HOST_MODE</td><td>RW</td><td>USB工作模式选择位:1:主机模式(HOST);0:设备模式(DEVICE)。</td><td>0</td></tr><tr><td>6</td><td>RB_UC_LOW_SPEED</td><td>RW</td><td>USB总线信号传输速率选择位:1:1.5Mbps;0:12Mbps。</td><td>0</td></tr><tr><td>5</td><td>RB_UC_DEVPU_EN</td><td>RW</td><td>USB设备模式下,USB设备使能和内部上拉电阻控制位,为1则使能USB设备传输并且启用内部上拉电阻。</td><td>0</td></tr><tr><td>[5:4]</td><td>MASK_UC_SYS_CTRL</td><td>RW</td><td>见下表配置USB系统。</td><td>0</td></tr><tr><td>3</td><td>RB_UC_INTBUSY</td><td>RW</td><td>USB 传输完成中断标志未清零前自动暂停使能位:
1: 在中断标志 UIF_TRANSFER未清零前自动暂停, 设备模式下自动应答忙 NAK, 主机模式下自动暂停后续传输;
0: 不暂停。</td><td>0</td></tr><tr><td>2</td><td>RB_UC_RST_SIE</td><td>RW</td><td>USB 协议处理器软件复位控制位:
1: 强制复位 USB 协议处理器 (SIE), 需要软件清零;
0: 不复位。</td><td>1</td></tr><tr><td>1</td><td>RB_UC_CLR_ALL</td><td>RW</td><td>USB 的 FIFO 和中断标志清零:
1: 强制清空和清零; 0: 不清。</td><td>1</td></tr><tr><td>0</td><td>RB_UC_DMA_EN</td><td>RW</td><td>USB 的 DMA 和 DMA 中断控制位:
1: 使能 DMA 功能和 DMA 中断;
0: 关闭 DMA。</td><td>0</td></tr></table>

由 RB_UC_HOST_MODE 和 MASK_UC_SYS_CTRL 组成 USB 系统控制组合：

表 23-2 USB 系统控制组合  

<table><tr><td>RB_UC_HOST_MODE</td><td>MASK_UC_SYS_CTRL</td><td>USB系统控制描述</td></tr><tr><td>0</td><td>00</td><td>禁止USB设备功能,关闭内部上拉电阻。</td></tr><tr><td>0</td><td>01</td><td>使能USB设备功能,关闭内部上拉电阻,需加外部上拉。</td></tr><tr><td>0</td><td>1x</td><td>使能USB设备功能,启用内部1.5K上拉电阻。该上拉电阻优先于下拉电阻,也可用于GPIO模式。</td></tr><tr><td>1</td><td>00</td><td>USB主机模式,正常工作状态。</td></tr><tr><td>1</td><td>01</td><td>USB主机模式,强制DP/DM输出SEO状态。</td></tr><tr><td>1</td><td>10</td><td>USB主机模式,强制DP/DM输出J状态。</td></tr><tr><td>1</td><td>11</td><td>USB主机模式,强制DP/DM输出K状态/唤醒。</td></tr></table>

23.2.1.2 USB 中断使能寄存器（R8_USB_INT_EN）  

<table><tr><td>位</td><td>名称</td><td>访问</td><td>描述</td><td>复位值</td></tr><tr><td>7</td><td>RB_UID_DEV_SOF</td><td>RW</td><td>USB设备模式,接收SOF包中断:1:使能中断;0:禁止中断。</td><td>0</td></tr><tr><td>6</td><td>RB_UID_DEV_NAK</td><td>RW</td><td>USB设备模式,接收到NAK中断:1:使能中断;0:禁止中断。</td><td>0</td></tr><tr><td>5</td><td>Reserved</td><td>RO</td><td>保留。</td><td>0</td></tr><tr><td>4</td><td>RB_UID_FIFO_0V</td><td>RW</td><td>FIFO溢出中断:1:使能中断;0:禁止中断。</td><td>0</td></tr><tr><td>3</td><td>RB_UID_HST_SOF</td><td>RW</td><td>USB主机模式,SOF定时中断:1:使能中断;0:禁止中断。</td><td>0</td></tr><tr><td>2</td><td>RB_UID_SUSPEND</td><td>RW</td><td>USB总线挂起或唤醒事件中断:1:使能中断;0:禁止中断。</td><td>0</td></tr><tr><td>1</td><td>RB_UID_TRANSFER</td><td>RW</td><td>USB传输完成中断:1:使能中断;0:禁止中断。</td><td>0</td></tr><tr><td>0</td><td>RB_UID_DETECT</td><td>RW</td><td>USB主机模式,USB设备连接或断开事件中断:</td><td>0</td></tr><tr><td rowspan="2"></td><td></td><td></td><td>1:使能中断; 0:禁止中断。</td><td></td></tr><tr><td>RB_UID_BUS_RST</td><td>RW</td><td>USB设备模式,USB总线复位事件中断:1:使能中断;0:禁止中断。</td><td>0</td></tr></table>

23.2.1.3 USB 设备地址寄存器（R8_USB_DEV_AD）  

<table><tr><td>位</td><td>名称</td><td>访问</td><td>描述</td><td>复位值</td></tr><tr><td>7</td><td>RB_UDA_GP_BIT</td><td>RW</td><td>USB通用标志位，用户自定义。</td><td>0</td></tr><tr><td>[6:0]</td><td>MASK_USB_ADDR</td><td>RW</td><td>主机模式：当前操作者的USB设备地址；设备模式：该USB自身地址。</td><td>0</td></tr></table>

23.2.1.4 USB 杂项状态寄存器（R8_USB_MIS_ST）  

<table><tr><td>位</td><td>名称</td><td>访问</td><td>描述</td><td>复位值</td></tr><tr><td>7</td><td>RB_UMS_SOF_PRES</td><td>RO</td><td>USB主机模式下SOF包预示状态位:1:将要发送SOF包,此时如有其它USB数据包将被自动延后;0:无SOF包发送。</td><td>x</td></tr><tr><td>6</td><td>RB_UMS_SOF_ACT</td><td>RO</td><td>USB主机模式下SOF包传输状态位:1:正在发出SOF包;0:发送完成或者空闲。</td><td>x</td></tr><tr><td>5</td><td>RB_UMS_SIE-Free</td><td>RO</td><td>USB协议处理器的空闲状态位:1:协议器空闲;0:忙,正在进行USB传输。</td><td>1</td></tr><tr><td>4</td><td>RB_UMS_R_FIFO_RDY</td><td>RO</td><td>USB接收FIFO数据就绪状态位:1:接收FIFO非空;0:接收FIFO为空。</td><td>0</td></tr><tr><td>3</td><td>RB_UMS_BUS_RST</td><td>RO</td><td>USB总线复位状态位:1:当前USB总线处于复位态;0:当前USB总线处于非复位态。</td><td>x</td></tr><tr><td>2</td><td>RB_UMS_SUSPEND</td><td>RO</td><td>USB挂起状态位:1:USB总线处于挂起态,有一段时间没有USB活动;0:USB总线处于非挂起态。</td><td>0</td></tr><tr><td>1</td><td>RB_UMS_DM_LEVEL</td><td>RO</td><td>USB主机模式下,设备刚连入USB端口时DM引脚的电平状态,用于判断速度:1:高电平/低速;0:低电平/全速。</td><td>0</td></tr><tr><td>0</td><td>RB_UMS_DEV_ATTACH</td><td>RO</td><td>USB主机模式下端口的USB设备连接状态位:1:端口已经连接USB设备;0:端口没有USB设备连接。</td><td>0</td></tr></table>

23.2.1.5 USB 中断标志寄存器（R8_USB_INT_FG）  

<table><tr><td>位</td><td>名称</td><td>访问</td><td>描述</td><td>复位值</td></tr><tr><td>7</td><td>RB_U_IS_NAK</td><td>R0</td><td>USB设备模式下，NAK响应状态位：1：当前USB传输过程中回应NAK；</td><td>0</td></tr><tr><td></td><td></td><td></td><td>0:无NAK响应。</td><td></td></tr><tr><td>6</td><td>RB_U_TOG_OK</td><td>RO</td><td>当前USB传输DATA0/1同步标志匹配状态位:1:同步;0:不同步。</td><td>0</td></tr><tr><td>5</td><td>RB_U_SIE-Free</td><td>RO</td><td>USB协议处理器空闲状态位:1:USB空闲;0:忙,正在进行USB传输。</td><td>1</td></tr><tr><td>4</td><td>RB_UID_FIFO_0V</td><td>RW</td><td>USB FIFO溢出中断标志位,写1清零:1: FIFO溢出触发;0:无事件。</td><td>0</td></tr><tr><td>3</td><td>RB_UID_HST_SOF</td><td>RW</td><td>USB主机模式下SOF定时中断标志位,写1清零:1:SOF包传输完成触发;0:无事件。</td><td>0</td></tr><tr><td>2</td><td>RB_UID_SUSPEND</td><td>RW</td><td>USB总线挂起或唤醒事件中断标志位,写1清零:1:USB挂起事件或唤醒事件触发;0:无事件。</td><td>0</td></tr><tr><td>1</td><td>RB_UID_TRANSFER</td><td>RW</td><td>USB传输完成中断标志位,写1清零:1:一个USB传输完成触发;0:无事件。</td><td>0</td></tr><tr><td rowspan="2">0</td><td>RB_UID_DETECT</td><td>RW</td><td>USB主机模式下USB设备连接或断开事件中断标志位,写1清零:1:检测到USB设备连接或断开触发;0:无事件。</td><td>0</td></tr><tr><td>RB_UID_BUS_RST</td><td>RW</td><td>USB设备模式下USB总线复位事件中断标志位,写1清零:1:USB总线复位事件触发;0:无事件。</td><td>0</td></tr></table>

23.2.1.6 USB 中断状态寄存器（R8_USB_INT_ST）  

<table><tr><td>位</td><td>名称</td><td>访问</td><td>描述</td><td>复位值</td></tr><tr><td>7</td><td>RB_UID_IS_NAK</td><td>R0</td><td>USB 设备模式下,NAK 响应状态位,同RB_U_IS_NAK:1:当前USB传输过程中回应NAK;0:无NAK响应。</td><td>0</td></tr><tr><td>6</td><td>RB_UID_TOG_OK</td><td>R0</td><td>当前USB传输DATA0/1同步标志匹配状态位,同RB_U_TOG_OK:1:同步;0:不同步。</td><td>0</td></tr><tr><td>[5:4]</td><td>MASK_UID_TOKEN</td><td>R0</td><td>设备模式下,当前USB传输事务的令牌PID标识。</td><td>x</td></tr><tr><td rowspan="2">[3:0]</td><td>MASK_UID_ENDP</td><td>R0</td><td>设备模式下,当前USB传输事务的端点号。</td><td>x</td></tr><tr><td>MASK_UID_H_RES</td><td>R0</td><td>主机模式下,当前USB传输事务的应答PID标识,0000表示设备无应答或超时;其它值表示应答PID。</td><td>x</td></tr></table>

MASK_UIS_TOKEN 用于 USB 设备模式下标识当前 USB 传输事务的令牌 PID：00 表示 OUT 包；01 表

示 SOF 包；10 表示 IN 包；11 表示 SETUP 包。

MASK_UIS_H_RES 仅在主机模式下有效。在主机模式下，若主机发送 OUT/SETUP 令牌包时，则该PID 是握手包 ACK/NAK/STALL，或是设备无应答/超时。若主机发送 IN 令牌包，则该 PID 是数据包的PID（DATA0/DATA1）或握手包 PID。

23.2.1.7 USB 接收长度寄存器（R16_USB_RX_LEN）  

<table><tr><td>位</td><td>名称</td><td>访问</td><td>描述</td><td>复位值</td></tr><tr><td>[15:10]</td><td>Reserved</td><td>R0</td><td>保留</td><td>0</td></tr><tr><td>[9:0]</td><td>R16_USB_RX_LEN</td><td>R0</td><td>当前USB端点接收的数据字节数</td><td>x</td></tr></table>

23.2.1.8 USB OTG 控制寄存器（R32_USB_OTG_CR）  

<table><tr><td>位</td><td>名称</td><td>访问</td><td>描述</td><td>复位值</td></tr><tr><td>[31:6]</td><td>Reserved</td><td>R0</td><td>保留</td><td>0</td></tr><tr><td>5</td><td>RB_CR_SESS_VTH</td><td>RW</td><td>OTG会话有效阈值电压设置1:SESS_VLD电平为1.4V;0:SESS_VLD电平为0.8V。</td><td>0</td></tr><tr><td>4</td><td>RB_CR_VBUS_VTH</td><td>RW</td><td>OTG VBUS阈值电压设置1: VBUS_VLD电平为4.4V;0: VBUS_VLD电平为4.8V。</td><td>0</td></tr><tr><td>3</td><td>RB_CR_OTG_EN</td><td>RW</td><td>OTG功能使能1:使能;0:禁止</td><td>0</td></tr><tr><td>2</td><td>RB_CR_IDPU</td><td>RW</td><td>USB_OTG_ID引脚上拉使能1:使能;0:禁止</td><td>0</td></tr><tr><td>1</td><td>RB_CR_CHARGE_VBUS</td><td>RW</td><td>OTG VBUS充电使能1:使能;0:禁止</td><td>0</td></tr><tr><td>0</td><td>RB_CR_DISCHAR_VBUS</td><td>RW</td><td>OTG VBUS放电使能1:使能;0:禁止</td><td>0</td></tr></table>

注：此寄存器仅适用于CH32V305、CH32V307、CH32F205和CH32F207

23.2.1.9 USB OTG 控制寄存器（R32_USB_OTG_SR）  

<table><tr><td>位</td><td>名称</td><td>访问</td><td>描述</td><td>复位值</td></tr><tr><td>[31:4]</td><td>Reserved</td><td>R0</td><td>保留</td><td>0</td></tr><tr><td>3</td><td>RB_SR_ID_DIG</td><td>R0</td><td>OTG ID 标志1: B 设备; 0: A 设备。</td><td>0</td></tr><tr><td>2</td><td>RB_SR_SESS_END</td><td>R0</td><td>OTG 会话结束有效标志1: 有效; 0: 无效</td><td>0</td></tr><tr><td>1</td><td>RB_SR_SESS_VLD</td><td>R0</td><td>OTG 会话有效标志1: 有效, 会话有效电平大于阈值电压0: 无效, 会话有效电平小于阈值电压</td><td>0</td></tr><tr><td>0</td><td>RB_SR_VBUS_VLD</td><td>R0</td><td>OTG VBUS 输入电平1: VBUS 电压大于阈值电压;0: VBUS 电压小于阈值电压。</td><td>X</td></tr></table>

注：此寄存器仅适用于CH32V305、CH32V307、CH32F205 和CH32F207

### 23.2.2 设备寄存器描述

USB OTG模块在USB设备模式下，提供了端点号 0-7共 8组双向端点配置寄存器，可映射端点号8-15的配置，除3号端点外所有端点最大包长度为64 字节，3号端点的最大数据包长度都是 1023 字节（同步传输）。

端点 0 是默认端点，支持控制传输，发送和接收共用一个 64 字节数据缓冲区  
端点1-15，可配置独立的64字节发送和接收缓冲区或者双 64字节数据缓冲区，支持批量传输、中断传输和实时/同步传输。

每组端点都具有一个控制寄存器 R8_UEPn_CTRL 和发送长度寄存器 R16_UEPn_T_LEN，用于设定该端点的同步触发位、对OUT事务和IN事务的响应以及发送数据的长度等。

USB OTG模块主机和设备的角色由OTG_FS_ID引脚状态决定，当 OTG_FS_ID引脚悬空，其内置的上拉电阻会使得 USB OTG 状态寄存器 R32_USB_OTG_SR 的 RB_SR_ID_DIG 位置 1，此时控制器应初始化为 B 设备。当 OTG_FS_ID 引脚接地，此时 USB OTG 状态寄存器 R32_USB_OTG_SR 的 RB_SR_ID_DIG 位为0，控制器应初始化为 A 设备。

B 设备在开始会话根据 OTG 状态寄存器 R32_USB_OTG_SR 的 RB_SR_SESS_VLD 来确保 VBUS 的电平低于 VSESS_VLD,min，若该位为 0，则可开始新的会话。若该位为 1，则可将 OTG 控制寄存器 R32_USB_OTG_CR的 RB_CR_DISCHAR_VBUS 位置 1 进行放电，使得 VBUS 小于会话阈值电平。

作为 B 类设备时，需从 VBUS 取电，则外部需一个转换电路，如图 22-1 所示。

![](images/31380bc190bf22c52494d8811acbd5f17a07f391503b62d73ae3d94d788dd731.jpg)  
图 23-1 OTG B 类设备连接图

作为 USB 设备所必要的 USB 总线上拉电阻可以由软件随时设置是否启用，当 USB 控制寄存器R8_USB_CTRL 中的 RB_UC_DEV_PU_EN 置 1 时，控制器根据 RB_UD_LOW_SPEED 的速度设置，在内部为 USB总线的DP/DM引脚连接上拉电阻，并启用 USB设备功能。

当检测到USB总线复位、USB总线挂起或唤醒事件，或当 USB成功处理完数据发送或数据接收后，USB协议处理器都将设置相应的中断标志，如果中断使能打开，还会产生相应的中断请求。应用程序可以直接查询或在 USB 中断服务程序中查询并分析中断标志寄存器 R8_USB_INT_FG，根据RB_UIF_BUS_RST 和 RB_UIF_SUSPEND 进行相应的处理；并且，如果 RB_UIF_TRANSFER 有效，那么还需要继续分析 USB 中断状态寄存器 R8_USB_INT_ST，根据当前端点号 MASK_UIS_ENDP 和当前事务令牌PID 标识 MASK_UIS_TOKEN 进行相应的处理。如果事先设定了各个端点的 OUT 事务的同步触发位RB_UEP_R_TOG，那么可以通过 RB_U_TOG_OK 或 RB_UIS_TOG_OK 判断当前所接收到的数据包的同步触发

位是否与该端点的同步触发位匹配，如果数据同步，则数据有效；如果数据不同步，则数据应该被丢弃。每次处理完USB发送或接收中断后，都应该正确修改相应端点的同步触发位，用于下次所发送的数据包或下次所接收的数据包是否同步检测；另外，设置 RB_UEP_AUTO_TOG 可以实现在发送成功或接收成功后自动翻转相应的同步触发位。

各个端点准备发送的数据在各自的缓冲区中，准备发送的数据长度是独立设定在 R8_UEPn_T_LEN中；各个端点接收到的数据在各自的缓冲区中，但是接收到的数据长度都在 USB 接收长度寄存器R8_USB_RX_LEN 中，可以在 USB 接收中断时根据当前端点号区分。

表 23-3 设备相关寄存器列表  

<table><tr><td>名称</td><td>访问地址</td><td>描述</td><td>复位值</td></tr><tr><td>R8_UDEV_CTRL</td><td>0x5000001</td><td>USB设备物理端口控制寄存器</td><td>0x0</td></tr><tr><td>R8_UEP4_1_MOD</td><td>0x5000000C</td><td>端点1(9)/4(8/12)模式控制寄存器</td><td>0x00</td></tr><tr><td>R8_UEP2_3_MOD</td><td>0x5000000D</td><td>端点2(10)/3(11)模式控制寄存器</td><td>0x00</td></tr><tr><td>R8_UEP5_6_MOD</td><td>0x5000000E</td><td>端点5(13)/6(14)模式控制寄存器</td><td>0x00</td></tr><tr><td>R8_UEP7_MOD</td><td>0x5000000F</td><td>端点7(15)模式控制寄存器</td><td>0x00</td></tr><tr><td>R16_UEPO_DMA</td><td>0x50000010</td><td>端点0缓冲区起始地址</td><td>0xFFFF</td></tr><tr><td>R16_UEP1_DMA</td><td>0x50000014</td><td>端点1(9)缓冲区起始地址</td><td>0xFFFF</td></tr><tr><td>R16_UEP2_DMA</td><td>0x50000018</td><td>端点2(10)缓冲区起始地址</td><td>0xFFFF</td></tr><tr><td>R16_UEP3_DMA</td><td>0x5000001C</td><td>端点3(11)缓冲区起始地址</td><td>0xFFFF</td></tr><tr><td>R16_UEP4_DMA</td><td>0x50000020</td><td>端点4(8/12)缓冲区起始地址</td><td>0xFFFF</td></tr><tr><td>R16_UEP5_DMA</td><td>0x50000024</td><td>端点5(13)缓冲区起始地址</td><td>0xFFFF</td></tr><tr><td>R16_UEP6_DMA</td><td>0x50000028</td><td>端点6(14)缓冲区起始地址</td><td>0xFFFF</td></tr><tr><td>R16_UEP7_DMA</td><td>0x5000002C</td><td>端点7(15)缓冲区起始地址</td><td>0xFFFF</td></tr><tr><td>R8_UEPO_T_LEN</td><td>0x50000030</td><td>端点0发送长度寄存器</td><td>0xX</td></tr><tr><td>R8_UEPO_TX_CTRL</td><td>0x50000032</td><td>端点0发送控制寄存器</td><td>0x00</td></tr><tr><td>R8_UEPO_RX_CTRL</td><td>0x50000033</td><td>端点0接收控制寄存器</td><td>0x00</td></tr><tr><td>R8_UEP1_T_LEN</td><td>0x50000034</td><td>端点1(9)发送长度寄存器</td><td>0xX</td></tr><tr><td>R8_UEP1_TX_CTRL</td><td>0x50000036</td><td>端点1(9)发送控制寄存器</td><td>0x00</td></tr><tr><td>R8_UEP1_RX_CTRL</td><td>0x50000037</td><td>端点1(9)接收控制寄存器</td><td>0x00</td></tr><tr><td>R8_UEP2_T_LEN</td><td>0x50000038</td><td>端点2(10)发送长度寄存器</td><td>0xX</td></tr><tr><td>R8_UEP2_TX_CTRL</td><td>0x5000003A</td><td>端点2(10)发送控制寄存器</td><td>0x00</td></tr><tr><td>R8_UEP2_RX_CTRL</td><td>0x5000003B</td><td>端点2(10)接收控制寄存器</td><td>0x00</td></tr><tr><td>R8_UEP3_T_LEN</td><td>0x5000003C</td><td>端点3(11)发送长度寄存器</td><td>0xX</td></tr><tr><td>R8_UEP3_TX_CTRL</td><td>0x5000003E</td><td>端点3(11)发送控制寄存器</td><td>0x00</td></tr><tr><td>R8_UEP3_RX_CTRL</td><td>0x5000003F</td><td>端点3(11)接收控制寄存器</td><td>0x00</td></tr><tr><td>R8_UEP4_T_LEN</td><td>0x50000040</td><td>端点4(8/12)发送长度寄存器</td><td>0xX</td></tr><tr><td>R8_UEP4_TX_CTRL</td><td>0x50000042</td><td>端点4(8/12)发送控制寄存器</td><td>0x00</td></tr><tr><td>R8_UEP4_RX_CTRL</td><td>0x50000043</td><td>端点4(8/12)接收控制寄存器</td><td>0x00</td></tr><tr><td>R8_UEP5_T_LEN</td><td>0x50000044</td><td>端点5(13)发送长度寄存器</td><td>0xX</td></tr><tr><td>R8_UEP5_TX_CTRL</td><td>0x50000046</td><td>端点5(13)发送控制寄存器</td><td>0x00</td></tr><tr><td>R8_UEP5_RX_CTRL</td><td>0x50000047</td><td>端点5(13)接收控制寄存器</td><td>0x00</td></tr><tr><td>R8_UEP6_T_LEN</td><td>0x50000048</td><td>端点6(14)发送长度寄存器</td><td>0xX</td></tr><tr><td>R8_UEP6_TX_CTRL</td><td>0x5000004A</td><td>端点6(14)发送控制寄存器</td><td>0x00</td></tr><tr><td>R8_UEP6_RX_CTRL</td><td>0x5000004B</td><td>端点6(14)接收控制寄存器</td><td>0x00</td></tr><tr><td>R8_UEP7_T_LEN</td><td>0x5000004C</td><td>端点7(15)发送长度寄存器</td><td>0xX</td></tr><tr><td>R8_UEP7_TX_CTRL</td><td>0x5000004E</td><td>端点7(15)发送控制寄存器</td><td>0x00</td></tr><tr><td>R8_UEP7_RX_CTRL</td><td>0x5000004F</td><td>端点7(15)接收控制寄存器</td><td>0x00</td></tr></table>

23.2.2.1 USB 设备物理端口控制寄存器（R8_UDEV_CTRL）  

<table><tr><td>位</td><td>名称</td><td>访问</td><td>描述</td><td>复位值</td></tr><tr><td>7</td><td>RB_UD_PD_DIS</td><td>RW</td><td>USB 设备端口 UD+/UD-引脚内部下拉电阻控制位:1:禁用内部下拉;0:使能内部下拉。可用于 GPIO 模式提供下拉电阻。</td><td>1</td></tr><tr><td>6</td><td>Reserved</td><td>RO</td><td>保留。</td><td>0</td></tr><tr><td>5</td><td>RB_UD_DP_PIN</td><td>RO</td><td>当前 UD+引脚状态:1:高电平; 0:低电平。</td><td>x</td></tr><tr><td>4</td><td>RB_UD_DM_PIN</td><td>RO</td><td>当前 UD-引脚状态:1:高电平; 0:低电平。</td><td>x</td></tr><tr><td>3</td><td>Reserved</td><td>RO</td><td>保留。</td><td>0</td></tr><tr><td>2</td><td>RB_UD_LOW_SPEED</td><td>RW</td><td>USB 设备物理端口低速模式使能位:1:选择1.5Mbps低速模式;0:选择12Mbps全速模式。</td><td>0</td></tr><tr><td>1</td><td>RB_UD_GP_BIT</td><td>RW</td><td>USB 设备模式通用标志位,用户自定义。</td><td>0</td></tr><tr><td>0</td><td>RB_UD_PORT_EN</td><td>RW</td><td>USB 设备物理端口使能位:1:使能物理端口;0:禁用物理端口。</td><td>0</td></tr></table>

23.2.2.2 端点 1(9)/4(8/12)模式控制寄存器（R8_UEP4_1_MOD）  

<table><tr><td>位</td><td>名称</td><td>访问</td><td>描述</td><td>复位值</td></tr><tr><td>7</td><td>RB_UEP1_RX_EN</td><td>RW</td><td>1:使能端点1(9)接收(OUT);0:禁止端点1(9)接收。</td><td>0</td></tr><tr><td>6</td><td>RB_UEP1_TX_EN</td><td>RW</td><td>1:使能端点1(9)发送(IN);0:禁止端点1(9)发送。</td><td>0</td></tr><tr><td>5</td><td>Reserved</td><td>RO</td><td>保留。</td><td>0</td></tr><tr><td>4</td><td>RB_UEP1BUF_MOD</td><td>RW</td><td>端点1(9)数据缓冲区模式控制位。注:该位为1时,UEP1_RX_EN和UEP1_TX_EN不能同时为1。</td><td>0</td></tr><tr><td>3</td><td>RB_UEP4_RX_EN</td><td>RW</td><td>1:使能端点4(8/12)接收(OUT);0:禁止端点4(8/12)接收。</td><td>0</td></tr><tr><td>2</td><td>RB_UEP4_TX_EN</td><td>RW</td><td>1:使能端点4(8/12)发送(IN);0:禁止端点4(8/12)发送。</td><td>0</td></tr><tr><td>1</td><td>Reserved</td><td>RO</td><td>保留。</td><td>0</td></tr><tr><td>0</td><td>RB_UEP4BUF_MOD</td><td>RW</td><td>端点4(8/12)数据缓冲区模式控制位。注:该位为1时,UEP4_RX_EN和UEP4_TX_EN不能同时为1。注:此位控制只支持CH32V103x系列。</td><td>0</td></tr></table>

注：端点1配置选项映射端点9，端点4配置选项映射端点8和12。

23.2.2.3 端点 2(10)/3(11)模式控制寄存器（R8_UEP2_3_MOD）  

<table><tr><td>位</td><td>名称</td><td>访问</td><td>描述</td><td>复位值</td></tr><tr><td>7</td><td>RB_UEP3_RX_EN</td><td>RW</td><td>1: 使能端点3(11)接收(OUT);</td><td>0</td></tr><tr><td></td><td></td><td></td><td>0:禁止端点3(11)接收。</td><td></td></tr><tr><td>6</td><td>RB_UEP3_TX_EN</td><td>RW</td><td>1:使能端点3(11)发送(IN);0:禁止端点3(11)发送。</td><td>0</td></tr><tr><td>5</td><td>Reserved</td><td>RO</td><td>保留。</td><td>0</td></tr><tr><td>4</td><td>RB_UEP3BUF_MOD</td><td>RW</td><td>端点3(11)数据缓冲区模式控制位。注:该位为1时,UEP3_RX_EN和UEP3_TX_EN不能同时为1。</td><td>0</td></tr><tr><td>3</td><td>RB_UEP2_RX_EN</td><td>RW</td><td>1:使能端点2(10)接收(OUT);0:禁止端点2(10)接收。</td><td>0</td></tr><tr><td>2</td><td>RB_UEP2_TX_EN</td><td>RW</td><td>1:使能端点2(10)发送(IN);0:禁止端点2(10)发送。</td><td>0</td></tr><tr><td>1</td><td>Reserved</td><td>RO</td><td>保留。</td><td>0</td></tr><tr><td>0</td><td>RB_UEP2BUF_MOD</td><td>RW</td><td>端点2(10)数据缓冲区模式控制位。注:该位为1时,UEP2_RX_EN和UEP2_TX_EN不能同时为1。</td><td>0</td></tr></table>

注：端点2配置选项映射端点10，端点3配置选项映射端点11。

23.2.2.4 端点 5(13)/6(14)模式控制寄存器（R8_UEP5_6_MOD）  

<table><tr><td>位</td><td>名称</td><td>访问</td><td>描述</td><td>复位值</td></tr><tr><td>7</td><td>RB_UEP6_RX_EN</td><td>RW</td><td>1:使能端点6(14)接收(OUT);0:禁止端点6(14)接收。</td><td>0</td></tr><tr><td>6</td><td>RB_UEP6_TX_EN</td><td>RW</td><td>1:使能端点6(14)发送(IN);0:禁止端点6(14)发送。</td><td>0</td></tr><tr><td>5</td><td>Reserved</td><td>RO</td><td>保留。</td><td>0</td></tr><tr><td>4</td><td>RB_UEP6BUF_MOD</td><td>RW</td><td>端点6(14)数据缓冲区模式控制位。注:该位为1时,UEP6_RX_EN和UEP6_TX_EN不能同时为1。</td><td>0</td></tr><tr><td>3</td><td>RB_UEP5_RX_EN</td><td>RW</td><td>1:使能端点5(13)接收(OUT);0:禁止端点5(13)接收。</td><td>0</td></tr><tr><td>2</td><td>RB_UEP5_TX_EN</td><td>RW</td><td>1:使能端点5(13)发送(IN);0:禁止端点5(13)发送。</td><td>0</td></tr><tr><td>1</td><td>Reserved</td><td>RO</td><td>保留。</td><td>0</td></tr><tr><td>0</td><td>RB_UEP5BUF_MOD</td><td>RW</td><td>端点5(13)数据缓冲区模式控制位。注:该位为1时,UEP5_RX_EN和UEP5_TX_EN不能同时为1</td><td>0</td></tr></table>

注：端点5配置选项映射端点13，端点6配置选项映射端点14。

23.2.2.5 端点 7(15)模式控制寄存器（R8_UEP7_MOD）  

<table><tr><td>位</td><td>名称</td><td>访问</td><td>描述</td><td>复位值</td></tr><tr><td>[7:4]</td><td>Reserved</td><td>R0</td><td>保留。</td><td>0</td></tr><tr><td>3</td><td>RB_UEP7_RX_EN</td><td>RW</td><td>1:使能端点7(15)接收(OUT);0:禁止端点7(15)接收。</td><td>0</td></tr><tr><td>2</td><td>RB_UEP7_TX_EN</td><td>RW</td><td>1:使能端点7(15)发送(IN);0:禁止端点7(15)发送。</td><td>0</td></tr><tr><td>1</td><td>Reserved</td><td>R0</td><td>保留。</td><td>0</td></tr><tr><td>0</td><td>RB_UEP7BUF_MOD</td><td>RW</td><td>端点7(15)数据缓冲区模式控制位。</td><td>0</td></tr></table>

注：端点7配置选项映射端点15。

由 RB_UEPn_RX_EN 和 RB_UEPn_TX_EN 以及 RB_UEPn_BUF_MOD 组合分别配置 USB 端点 1-15 的数据缓冲区模式，具体参考表 23-4。其中，在双 64 字节缓冲区模式下，USB 数据传输时将根据RB_UEP_*_TOG=0 选择前 64 字节缓冲区，根据 RB_UEP_*_TOG=1 选择后 64 字节缓冲区，设置RB_UEP_AUTO_ $\mathtt { T O G } = 1$ 可实现自动切换。

表 23-4 端点 n 缓冲区模式（ $\therefore n = 1 - 7$ ）  

<table><tr><td>RB_UEPn_RX_EN</td><td>RB_UEPn_TX_EN</td><td>RB_UEPnBUF_MOD</td><td>描述:以R16_UEPn_DMA为起始地址由低向高排列</td></tr><tr><td>0</td><td>0</td><td>X</td><td>端点被禁用,未用到R16_UEPn_DMA缓冲区。</td></tr><tr><td>1</td><td>0</td><td>0</td><td>单64字节接收缓冲区(OUT)。</td></tr><tr><td>1</td><td>0</td><td>1</td><td>双64字节接收缓冲区(OUT),由RB_UEP_R_TOG选择。</td></tr><tr><td>0</td><td>1</td><td>0</td><td>单64字节发送缓冲区(IN)。</td></tr><tr><td>0</td><td>1</td><td>1</td><td>双64字节发送缓冲区(IN),由RB_UEP_T_TOG选择。</td></tr><tr><td>1</td><td>1</td><td>0</td><td>单64字节接收缓冲区(OUT),单64字节发送缓冲区(IN)。</td></tr><tr><td>1</td><td>1</td><td>1</td><td>双64字节接收缓冲区(OUT),通过RB_UEP_R_TOG选择,双64字节发送缓冲区(IN),通过RB_UEP_T_TOG选择。全部256字节排列如下:UEPn_DMA+0地址:RB_UEP_R_TOG=0时端点接收地址;UEPn_DMA+64地址:RB_UEP_R_TOG=1时端点接收地址;UEPn_DMA+128地址:RB_UEP_T_TOG=0时端点发送地址;UEPn_DMA+192地址:RB_UEP_T_TOG=1时端点发送地址。</td></tr></table>

注：表21-4的配置选择支持 $\scriptstyle n = 1 - 7$ ，端点8-15配置映射端点1-7配置。

端点 n 缓冲区起始地址（R8_UEPn_DMA）（ $\scriptstyle n = 0 - 7$ ）  

<table><tr><td>位</td><td>名称</td><td>访问</td><td>描述</td><td>复位值</td></tr><tr><td>[7:0]</td><td>R8_UEPn_DMA</td><td>RW</td><td>端点n缓冲区起始地址。低15位有效，地址必须4字节对齐。</td><td>x</td></tr></table>

注：1.接收数据的缓冲区的长度 $> = m i n$ （可能收到的最大数据包长度＋2字节，64字节）。  
2.端点DMA配置支持0-7端点，可映射配置端点8-15端点。

23.2.2.6 端点 n 发送长度寄存器（R16_UEPn_T_LEN）（n=0-7）  

<table><tr><td>位</td><td>名称</td><td>访问</td><td>描述</td><td>复位值</td></tr><tr><td>[9:0]</td><td>R8_UEP3_T_LEN</td><td>RW</td><td>设置USB端点3准备发送的数据字节数。</td><td>x</td></tr><tr><td>[7:0]</td><td>R8_UEPn_T_LEN</td><td>RW</td><td>设置USB端点n准备发送的数据字节数n=0,1,2,4,5,6,7。</td><td>x</td></tr></table>

注1.端点发送长度配置支持0-7端点，可映射配置8-15端点的发送。  
2.主机发送支持最大1023字节（针对同步端点）

23.2.2.7 端点 n 控制寄存器（R8_UEPn_TX_CTRL）（ $\scriptstyle \mathbf { n = 0 - 7 }$ ）  

<table><tr><td>位</td><td>名称</td><td>访问</td><td>描述</td><td>复位值</td></tr><tr><td>[7:4]</td><td>Reserved</td><td>R0</td><td>保留。</td><td>0</td></tr><tr><td>3</td><td>RB_UEP_T_AUTO_TOG</td><td>RW</td><td>同步触发位自动翻转使能控制位:1:数据发送成功后自动翻转相应的同步</td><td>0</td></tr><tr><td></td><td></td><td></td><td>触发位;0:不自动翻转,可以手动切换。注:端点0此位保留。</td><td></td></tr><tr><td>2</td><td>RB_UEP_T_TOG</td><td>RW</td><td>USB端点n的发送器(处理IN事务)准备的同步触发位:1:发送DATA1; 0:发送DATA0。</td><td>0</td></tr><tr><td>[1:0]</td><td>MASK_UEP_T_RES</td><td>RW</td><td>端点n的发送器对IN事务的响应控制:00:DATA0/DATA1数据就绪并期望ACK;01:应答DATA0/DATA1并期望无响应,用于非端点0的实时/同步传输;10:应答NAK或忙;11:应答STALL或错误。</td><td>00b</td></tr></table>

注：端点配置支持0-7端点，可映射配置端点8-15端点。

#### 23.2.2.8 端点 n 控制寄存器（R8_UEPn_ RX_CTRL）（n=0-7）

<table><tr><td>位</td><td>名称</td><td>访问</td><td>描述</td><td>复位值</td></tr><tr><td>[7:4]</td><td>Reserved</td><td>R0</td><td>保留。</td><td>0</td></tr><tr><td>3</td><td>RB_UEP_R_AUTO_TOG</td><td>RW</td><td>同步触发位自动翻转使能控制位:1:数据接收成功后自动翻转相应的同步触发位;0:不自动翻转,可以手动切换。注:端点0此位保留。</td><td>0</td></tr><tr><td>2</td><td>MASK_UEP_R_TOG</td><td>RW</td><td>USB端点n的接收器(处理OUT事务)期望的同步触发位:1:期望DATA1; 0:期望DATA0。</td><td>0</td></tr><tr><td>[1:0]</td><td>MASK_UEP_R_RES</td><td>RW</td><td>端点n的接收器对OUT事务的响应控制:00:应答ACK;01:超时/无响应,用于非端点0的实时/同步传输;10:应答NAK或忙;11:应答STALL或错误。</td><td>00b</td></tr></table>

注：端点配置支持0-7端点，可映射配置端点8-15端点。

### 22.2.3 USB 主机寄存器

在USB OTG主机模式下，芯片提供了一组双向主机端点，包括一个发送端点 OUT和一个接收端点IN，一个数据包的最大长度是1023字节，支持控制传输、中断传输、批量传输和实时/同步传输。

在USB OTG主机模式下，若控制器不能为VUBS 提供 5V电源，则需外接电荷泵，若应用板能提供 5V电源则可采用模拟开关控制 VBUS 的打开和关断，如图 23-2 所示。

![](images/3c70dcb93bf4f5b90c9e08e0f363fe127abe2d14f999ae595d7a3f8d3a207428.jpg)  
图 23-2 OTG 的 A 类设备连接

主机端点发起的每一个USB事务，在处理结束后总是自动设置 RB_UIF_TRANSFER中断标志。应用程序可以直接查询或在USB中断服务程序中查询并分析中断标志寄存器 R8_USB_INT_FG，根据各中断标志分别进行相应的处理；并且，如果RB_UIF_TRANSFER有效，那么还需要继续分析 USB中断状态寄存器 R8_USB_INT_ST，根据当前 USB 传输事务的应答 PID 标识 MASK_UIS_H_RES 进行相应的处理。

如果事先设定了主机接收端点的 IN 事务的同步触发位（RB_UH_R_TOG），那么可以通过RB_U_TOG_OK 或 RB_UIS_TOG_OK 判断当前所接收到的数据包的同步触发位是否与主机接收端点的同步触发位匹配，如果数据同步，则数据有效；如果数据不同步，则数据应该被丢弃。每次处理完 USB发送或接收中断后，都应该正确修改相应主机端点的同步触发位，用于同步下次所发送的数据包和检测下次所接收的数据包是否同步；另外，通过设置 RB_UH_T_AUTO_TOG 和 RB_UH_R_AUTO_TOG 可以实现在发送成功或接收成功后自动翻转相应的同步触发位。

USB主机令牌设置寄存器R8_UH_EP_PID用于设置被操作的目标设备的端点号和本次 USB传输事务的令牌PID包标识。SETUP令牌和OUT令牌所对应的数据由主机发送端点提供，准备发送的数据在 R16_UH_TX_DMA 缓冲区中，准备发送的数据长度设置在 R16_UH_TX_LEN 中；IN 令牌所对应数据由目标设备返回给主机接收端点，接收到数据存放 R16_UH_RX_DMA 缓冲区中，接收到的数据长度存放在 R16_USB_RX_LEN 中，主机端点可接收的最大包长度需要提前写入到 R16_UH_RX_MAX_LEN 寄存器中。

表23-5 主机相关寄存器列表  

<table><tr><td>名称</td><td>访问地址</td><td>描述</td><td>复位值</td></tr><tr><td>R8_UHOST_CTRL</td><td>0x50000001</td><td>USB 主机物理端口控制寄存器</td><td>0x0</td></tr><tr><td>R32_UH_EP_MOD</td><td>0x5000000D</td><td>USB 主机端点模式控制寄存器</td><td>0x00</td></tr><tr><td>R16_UH_RX_DMA</td><td>0x50000018</td><td>USB 主机接收缓冲区起始地址</td><td>X</td></tr><tr><td>R16_UH_TX_DMA</td><td>0x5000001C</td><td>USB 主机发送缓冲区起始地址</td><td>X</td></tr><tr><td>R16_UH_setup</td><td>0x50000036</td><td>USB 主机辅助设置寄存器</td><td>0x00</td></tr><tr><td>R8_UH_EP_PID</td><td>0x50000038</td><td>USB 主机令牌设置寄存器</td><td>0x00</td></tr><tr><td>R8_UH_RX_CTRL</td><td>0x5000003A</td><td>USB 主机接收端点控制寄存器</td><td>0x00</td></tr><tr><td>R16_UH_TX_LEN</td><td>0x5000003C</td><td>USB 主机发送长度寄存器</td><td>X</td></tr><tr><td>R8_UH_TX_CTRL</td><td>0x5000003E</td><td>USB 主机发送端点控制寄存器</td><td>0x00</td></tr></table>

22.2.3.1 USB 主机物理端口控制寄存器（R8_UHOST_CTRL）  

<table><tr><td>位</td><td>名称</td><td>访问</td><td>描述</td><td>复位值</td></tr><tr><td>7</td><td>RB_UH_PD_DIS</td><td>RW</td><td>USB 主机端口 UD+/UD-引脚内部下拉电阻控制位:1:禁用内部下拉;0:使能内部下拉。可用于 GPIO 模式提供下拉电阻。</td><td>1</td></tr><tr><td>6</td><td>Reserved</td><td>RO</td><td>保留。</td><td>0</td></tr><tr><td>5</td><td>RB_UH_DP_PIN</td><td>RO</td><td>当前 UD+引脚状态:1:高电平; 0:低电平。</td><td>x</td></tr><tr><td>4</td><td>RB_UH_DM_PIN</td><td>RO</td><td>当前 UD-引脚状态:1:高电平; 0:低电平。</td><td>x</td></tr><tr><td>3</td><td>Reserved</td><td>RO</td><td>保留。</td><td>0</td></tr><tr><td>2</td><td>RB_UH_LOW_SPEED</td><td>RW</td><td>USB 主机端口低速模式使能位:1:选择 1.5Mbps 低速模式;0:选择 12Mbps 全速模式。</td><td>0</td></tr><tr><td>1</td><td>RB_UH_BUS_RST</td><td>RW</td><td>USB 主机模式总线复位控制位:1:强制输出 USB 总线复位;0:结束输出。</td><td>0</td></tr><tr><td>0</td><td>RB_UH_PORT_EN</td><td>RW</td><td>USB 主机端口使能位:1:使能主机端口;0:禁用主机端口。当 USB 设备断开连接时,该位自动清 0。</td><td>0</td></tr></table>

22.2.3.2 USB 主机端点模式控制寄存器（R32_UH_EP_MOD）  

<table><tr><td>位</td><td>名称</td><td>访问</td><td>描述</td><td>复位值</td></tr><tr><td>7</td><td>Reserved</td><td>R0</td><td>保留。</td><td>0</td></tr><tr><td>6</td><td>RB_UH_EP_TX_EN</td><td>RW</td><td>主机发送端点发送 (SETUP/OUT) 使能位:1: 使能端点发送;0: 禁止端点发送。</td><td>0</td></tr><tr><td>5</td><td>Reserved</td><td>R0</td><td>保留。</td><td>0</td></tr><tr><td>4</td><td>RB_UH_EP_TBUF_MOD</td><td>RW</td><td>主机发送端点发送数据缓冲区模式控制位。</td><td>0</td></tr><tr><td>3</td><td>RB_UH_EP_RX_EN</td><td>RW</td><td>主机接收端点接收 (IN) 使能位:1: 使能端点接收;0: 禁止端点接收。</td><td>0</td></tr><tr><td>[2:1]</td><td>Reserved</td><td>R0</td><td>保留。</td><td>0</td></tr><tr><td>0</td><td>RB_UH_EP_RBUF_MOD</td><td>RW</td><td>USB 主机接收端点接收数据缓冲区模式控制位。</td><td>0</td></tr></table>

由 RB_UH_EP_TX_EN 和 RB_UH_EP_TBUF_MOD 组合控制主机发送端点数据缓冲区模式，参考下表。

表23-6 主机发送缓冲区模式  

<table><tr><td>RB_UH_EP_TX_EN</td><td>RB_UH_EP_TBUF_MOD</td><td>描述:以R16_UH_TX_DMA为起始地址</td></tr><tr><td>0</td><td>X</td><td>端点被禁用,未用到R16_UH_TX_DMA缓冲区。</td></tr><tr><td>1</td><td>0</td><td>单64字节发送缓冲区(SETUP/OUT)。</td></tr><tr><td>1</td><td>1</td><td>双64字节发送缓冲区,通过RB_UH_T_TOG选择:当RB_UH_T_TOG=0时选择前64字节缓冲区;当RB_UH_T_TOG=1时选择后64字节缓冲区。</td></tr></table>

由 RB_UH_EP_RX_EN 和 RB_UH_EP_RBUF_MOD 组合控制主机接收端点数据缓冲区模式，参考下表。

表23-7 主机接收缓冲区模式  

<table><tr><td>RB_UH_EP_RX_EN</td><td>RB_UH_EP_RBUF_MOD</td><td>结构描述:以R16_UH_TX_DMA为起始地址</td></tr><tr><td>0</td><td>X</td><td>端点被禁用,未用到R16_UH_RX_DMA缓冲区。</td></tr><tr><td>1</td><td>0</td><td>单64字节接收缓冲区(IN)。</td></tr><tr><td>1</td><td>1</td><td>双64字节接收缓冲区,通过RB_UH_R_TOG选择:当RB_UH_R_TOG=0时选择前64字节缓冲区;当RB_UH_R_TOG=1时选择后64字节缓冲区。</td></tr></table>

USB 主机接收缓冲区起始地址（R16_UH_RX_DMA）  

<table><tr><td>位</td><td>名称</td><td>访问</td><td>描述</td><td>复位值</td></tr><tr><td>[15:0]</td><td>R16_UH_RX_DMA</td><td>RW</td><td>主机端点数据接收缓冲区起始地址。低15位有效，地址必须4字节对齐。</td><td>x</td></tr></table>

USB 主机发送缓冲区起始地址（R16_UH_TX_DMA）  

<table><tr><td>位</td><td>名称</td><td>访问</td><td>描述</td><td>复位值</td></tr><tr><td>[15:0]</td><td>R16_UH_TX_DMA</td><td>RW</td><td>主机端点数据发送缓冲区起始地址。低15位有效，地址必须4字节对齐。</td><td>x</td></tr></table>

22.2.3.3 USB 主机辅助设置寄存器（R16_UH_SETUP）  

<table><tr><td>位</td><td>名称</td><td>访问</td><td>描述</td><td>复位值</td></tr><tr><td>[15:11]</td><td>Reserved</td><td>R0</td><td>保留。</td><td>0</td></tr><tr><td>10</td><td>RB_UH_PRE_PID_EN</td><td>RW</td><td>低速前导包 PRE_PID 使能位:1:使能,用于通过外部 HUB 与低速 USB设备通讯。0:禁用低速前导包。</td><td>0</td></tr><tr><td>[9:3]</td><td>Reserved</td><td>R0</td><td>保留。</td><td>0</td></tr><tr><td>2</td><td>RB_UH_SOF_EN</td><td>RW</td><td>自动产生 SOF 包使能位:1:主机自动产生 SOF 包;0:关闭自动 SOF 功能。</td><td>0</td></tr><tr><td>[1:0]</td><td>Reserved</td><td>R0</td><td>保留。</td><td>0</td></tr></table>

22.2.3.4 USB 主机令牌设置寄存器（R8_UH_EP_PID）  

<table><tr><td>位</td><td>名称</td><td>访问</td><td>描述</td><td>复位值</td></tr><tr><td>[7:4]</td><td>MASK_UH_TOKEN</td><td>RW</td><td>设置本次USB传输事务的令牌PID标识。</td><td>0</td></tr><tr><td>[3:0]</td><td>MASK_UH_ENDP</td><td>RW</td><td>设置本次被操作的目标设备的端点号。</td><td>0</td></tr></table>

22.2.3.5 USB 主机接收端点控制寄存器（R8_UH_RX_CTRL）  

<table><tr><td>位</td><td>名称</td><td>访问</td><td>描述</td><td>复位值</td></tr><tr><td>[7:4]</td><td>Reserved</td><td>R0</td><td>保留。</td><td>0</td></tr><tr><td>3</td><td>RB_UH_R_AUTO_TOG</td><td>RW</td><td>同步触发位自动翻转使能控制位:1:数据接收成功后自动翻转相应的期待同步触发位(RB_UH_R_TOG);0:手动控制同步触发位(RB_UH_R_TOG)。</td><td>0</td></tr><tr><td>2</td><td>RB_UH_R_TOG</td><td>RW</td><td>主机接收器(处理IN事务)准备的同步触发位:1:无响应,用于非0端点的实时/同步传输;0:应答ACK。</td><td>0</td></tr><tr><td>1</td><td>Reserved</td><td>R0</td><td>保留。</td><td>0</td></tr><tr><td>0</td><td>RB_UH_R_RES</td><td>R0</td><td>主机接收器对IN事务的响应控制位:1:无响应,用于非0端点的实时/同步传输;0:应答ACK。</td><td>0</td></tr></table>

22.2.3.6 USB 主机发送长度寄存器（R16_UH_TX_LEN）  

<table><tr><td>位</td><td>名称</td><td>访问</td><td>描述</td><td>复位值</td></tr><tr><td>[15:0]</td><td>R16_UH_TX_LEN</td><td>RW</td><td>设置USB主机发送端点准备发送的数据字节数。</td><td>x</td></tr></table>

22.2.3.7 USB 主机发送端点控制寄存器（R8_UH_TX_CTRL）  

<table><tr><td>位</td><td>名称</td><td>访问</td><td>描述</td><td>复位值</td></tr><tr><td>[7:4]</td><td>Reserved</td><td>R0</td><td>保留。</td><td>0</td></tr><tr><td>3</td><td>RB_UH_T_AUTO_TOG</td><td>R0</td><td>同步触发位自动翻转使能控制位:1:数据发送成功后自动翻转相应的同步触发位(RB_UH_T_TOG);0:手动控制同步触发位(RB_UH_T_TOG)。</td><td>0</td></tr><tr><td>2</td><td>RB_UH_T_TOG</td><td>RW</td><td>USB主机发送器(处理SETUP/OUT事务)准备的同步触发位:1:表示发送DATA1;0:表示发送DATA0。</td><td>0</td></tr><tr><td>1</td><td>Reserved</td><td>R0</td><td>保留。</td><td>0</td></tr><tr><td>0</td><td>RB_UH_T_RES</td><td>RW</td><td>USB主机发送器对SETUP/OUT事务的响应控制位:1:期望无响应,用于非0端点的实时/同步传输;0:期望应答ACK。</td><td>0</td></tr></table>


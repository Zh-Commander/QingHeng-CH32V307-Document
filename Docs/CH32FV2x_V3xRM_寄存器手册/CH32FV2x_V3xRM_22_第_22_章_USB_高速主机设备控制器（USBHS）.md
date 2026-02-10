# 第 22 章 USB 高速主机/设备控制器（USBHS）

本章模块描述适用于CH32F2x、CH32V2x和CH32V3x微控制器系列部分产品。

## 22.1 USB 控制器简介

内嵌 USB2.0 控制器和 USB-PHY，具有主机控制器和 USB 设备控制器双重角色。当作为主机控制器时，它可支持低速、全速和高速的 USB 设备/HUB。当作为设备控制器时，可以灵活设置为低速、全速或高速模式以适应各种应用。

USB控制器特性如下：

支持 USB Host 主机功能和 USB Device 设备功能。  
主机模式下支持下行端口连接高速/全速 HUB。  
$\bullet$ 设备模式下支持 USB2.0 高速 480Mbps、全速 12Mbps 或低速 1.5Mbps。  
$\bullet$ 支持 USB 控制传输、批量传输、中断传输和同步/实时传输。  
$\bullet$ 支持 DMA 直接访问各端点缓冲区的数据。  
$\bullet$ 支持挂起，唤醒/远程唤醒。  
端点0支持最大64字节的数据包，除设备端点 0 外，其他端点均支持最大 1024字节的数据包，且均支持双缓冲。

## 22.2 寄存器描述

USB 相关寄存器分为 3 个部分，部分寄存器是在主机和设备模式下进行复用的。

USB 全局寄存器  
$\bullet$ USB 设备控制寄存器  
USB主机控制寄存器

### 22.2.1 全局寄存器描述

表 22-1 USBHS 相关寄存器列表  

<table><tr><td>名称</td><td>访问地址</td><td>描述</td><td>复位值</td></tr><tr><td>R8_USB_CTRL</td><td>0x40023400</td><td>USB 控制寄存器</td><td>0x06</td></tr><tr><td>R8_USB_INT_EN</td><td>0x40023402</td><td>USB 中断使能寄存器</td><td>0</td></tr><tr><td>R8_USB_DEV_AD</td><td>0x40023403</td><td>USB 设备地址寄存器</td><td>0</td></tr><tr><td>R16_USB_FRAME_NO</td><td>0x40023404</td><td>USB 帧号寄存器</td><td>0</td></tr><tr><td>R8_USB_SUSPEND</td><td>0x40023406</td><td>USB 挂起控制寄存器</td><td>0</td></tr><tr><td>R8_USB_SPPED_TYPE</td><td>0x40023408</td><td>USB 当前速度类型寄存器</td><td>0</td></tr><tr><td>R8_USB_MIS_ST</td><td>0x40023409</td><td>USB 杂项状态寄存器</td><td>xx10_1000b</td></tr><tr><td>R8_USB_INT_FG</td><td>0x4002340A</td><td>USB 中断标志寄存器</td><td>0</td></tr><tr><td>R8_USB_INT_ST</td><td>0x4002340B</td><td>USB 中断状态寄存器</td><td>00xx_xxxxxb</td></tr><tr><td>R16_USB_RX_LEN</td><td>0x4002340C</td><td>USB 接收长度寄存器</td><td>xx</td></tr></table>

22.2.1.1 USB 控制寄存器（R8_USB_CTRL）  

<table><tr><td>位</td><td>名称</td><td>访问</td><td>描述</td><td>复位值</td></tr><tr><td>7</td><td>RB_UC_HOST_MODE</td><td>RW</td><td>USB工作模式选择位:1:主机模式(HOST);0:设备模式(DEVICE)。</td><td>0</td></tr><tr><td>[6:5]</td><td>RB_UC_SPEED_TYPE</td><td>RW</td><td>USB总线信号传输速率选择位:</td><td>00b</td></tr><tr><td></td><td></td><td></td><td>00:全速;01:高速;10:低速。</td><td></td></tr><tr><td>4</td><td>RB_UC_DEVPU_EN</td><td>RW</td><td>设备模式下,USB设备使能和内部上拉电阻控制位:1:使能USB设备传输并且启用内部上拉电阻;0:不启用。</td><td>0</td></tr><tr><td>3</td><td>RB_UC_INTBUSY</td><td>RW</td><td>USB传输完成中断标志未清零前自动暂停使能位:1:在中断标志UIF_TRANSFER未清零前自动暂停,设备模式下自动应答忙NAK,主机模式下自动暂停后续传输;0:不暂停。</td><td>0</td></tr><tr><td>2</td><td>RB_UC_RST_SIE</td><td>RW</td><td>USB协议处理器软件复位控制位:1:强制复位USB协议处理器(SIE),需要软件清零;0:不复位。该位清除后,PB6/PB7自动切换为USBIO模式</td><td>1</td></tr><tr><td>1</td><td>RB_UC_CLR_ALL</td><td>RW</td><td>USB的FIFO和中断标志清零:1:清空USB中断标志和FIFO,需要软件清零;0:不清空。</td><td>1</td></tr><tr><td>0</td><td>RB_UC_DMA_EN</td><td>RW</td><td>使能USB的DMA,正常传输模式下该位必须设置为1:1:使能DMA功能和DMA中断;0:关闭DMA。</td><td>0</td></tr></table>

22.2.1.2 USB 中断使能寄存器（R8_USB_INT_EN）  

<table><tr><td>位</td><td>名称</td><td>访问</td><td>描述</td><td>复位值</td></tr><tr><td>7</td><td>RB_UID_DEV_NAK</td><td>RW</td><td>USB设备模式,接收到NAK中断:1:使能相应中断;0:禁止相应中断。</td><td>0</td></tr><tr><td>6</td><td>RB_UID_ISO_ACT</td><td>RW</td><td>同步传输开始发送/接收数据中断:1:使能相应中断;0:禁止相应中断。</td><td>0</td></tr><tr><td>5</td><td>RB_UID_setup_ACT</td><td>RW</td><td>SETUP事务完成中断:1:使能相应中断;0:禁止相应中断。</td><td>0</td></tr><tr><td>4</td><td>RB_UID_FIFO_0V</td><td>RW</td><td>FIFO0溢出中断:1:使能中断;0:禁止中断。</td><td>0</td></tr><tr><td>3</td><td>RB_UID_SOF_ACT</td><td>RW</td><td>USB主机模式,SOF定时中断:1:使能中断;0:禁止中断。USB设备模式,使能后接收SOF包产生传输完成中断</td><td>0</td></tr><tr><td>2</td><td>RB_UID_SUSPEND</td><td>RW</td><td>USB总线挂起或唤醒事件中断:1:使能中断;0:禁止中断。</td><td>0</td></tr><tr><td>1</td><td>RB_UID_TRANSFER</td><td>RW</td><td>USB传输(不包括SETUP事务)完成中断:1:使能中断;0:禁止中断。</td><td>0</td></tr><tr><td rowspan="2">0</td><td>RB_UID_DETECT</td><td>RW</td><td>USB主机模式,USB设备连接或断开事件中断:1:使能中断;0:禁止中断。</td><td>0</td></tr><tr><td>RB_UID_BUS_RST</td><td>RW</td><td>USB设备模式,USB总线复位事件中断:1:使能中断;0:禁止中断。</td><td>0</td></tr></table>

#### 22.2.1.3 USB 设备地址寄存器（R16_USB_DEV_AD）

<table><tr><td>位</td><td>名称</td><td>访问</td><td>描述</td><td>复位值</td></tr><tr><td>7</td><td>Reserved</td><td>R0</td><td>保留。</td><td>0</td></tr><tr><td>[6:0]</td><td>RB_MASK_USB_ADDR</td><td>RW</td><td>主机模式下是当前操作时的USB设备的地址或HUB地址;设备模式:该USB自身地址。</td><td>0</td></tr></table>

#### 22.2.1.4 USB 帧号寄存器（R16_USB_FRAME_NO）

<table><tr><td>位</td><td>名称</td><td>访问</td><td>描述</td><td>复位值</td></tr><tr><td>[15:0]</td><td>USB_FRAME_NO</td><td>R0</td><td>帧号,主机模式下表示即将发送的 SOF包的帧号,设备模式下表示当前接收到的 SOF 包的帧号。其中低 11 位为有效帧号,高 3 位为高速模式的微帧号。</td><td>0</td></tr></table>

注：USB_FRAME_N0是16为寄存器，其中低11位表示SOF包帧号，高3为表示当前属于第几个微帧，可在操作高速HUB下进行中断、同步/实时传输时使用。

#### 22.2.1.5 USB 挂起寄存器（R8_USB_SUSPEND）

<table><tr><td>位</td><td>名称</td><td>访问</td><td>描述</td><td>复位值</td></tr><tr><td>[7:6]</td><td>Reserved</td><td>R0</td><td>保留。</td><td>0</td></tr><tr><td>[5:4]</td><td>RB_USB_LINESTATE</td><td>R0</td><td>PHY的Linestate信号</td><td>X</td></tr><tr><td>3</td><td>Reserved</td><td>R0</td><td>保留。</td><td>0</td></tr><tr><td>2</td><td>RB_USB_WAKEUP_ST</td><td>R0</td><td>在挂起状态下,如果接收到主机的唤醒信号,该位置1,直到退出挂起状态。</td><td>X</td></tr><tr><td>[1:0]</td><td>RB_USB_SYS_MOD</td><td>RW</td><td>主机模式下的测试模式</td><td>0</td></tr></table>

注：需要远程唤醒时，将RB_UH_REMOTE_WKUP位拉高再拉低即可。

#### 22.2.1.6 USB 速度类型寄存器（R8_USB_SPEED_TYPE）

<table><tr><td>位</td><td>名称</td><td>访问</td><td>描述</td><td>复位值</td></tr><tr><td>[7:2]</td><td>Reserved</td><td>R0</td><td>保留。</td><td>0</td></tr><tr><td>[1:0]</td><td>RB_USB_SPEED_TYPE</td><td>R0</td><td>在主机模式下,表示当前连接的设备速度类型,在设备模式下,表示当前设备的速度类型;00:全速;01:高速;10:低速。</td><td>00b</td></tr></table>

注：区别于R8_USB_CTRL寄存器中的RB_UC_SPEED_TYPE，RB_UC_SPEED_TYPE表示期望处于的最高速度，假设在设备模式下，设置RB_UC_SPEED_TYPE为高速，当该设备连接在一个全速主机下，则实际的速度类型就是全速，通过查询R8_USB_SPEED_TYPE寄存器可以获知。在主机模式下，设置RB_UC_SPEED_TYPE为高速，当连接一个全速设备时，则实际通讯速度就是全速，通过查询

R8_USB_SPEED_TYPE寄存器可以获知。

22.2.1.7 USB 杂项状态寄存器（R8_USB_MIS_ST）  

<table><tr><td>位</td><td>名称</td><td>访问</td><td>描述</td><td>复位值</td></tr><tr><td>7</td><td>RB_UMS_SOF_PRES</td><td>RO</td><td>USB主机模式下SOF包预示状态位:1:将要发送SOF包,此时如有其它USB数据包将被自动延后;0:无SOF包发送。</td><td>X</td></tr><tr><td>6</td><td>RB_UMS_SOF_ACT</td><td>RO</td><td>USB主机模式下SOF包传输状态位:1:正在发出SOF包;0:发送完成或空闲。</td><td>X</td></tr><tr><td>5</td><td>RB_UMS_SIE-Free</td><td>RO</td><td>USB协议处理器的空闲状态位:1:协议器空闲;0:忙,正在进行USB传输。</td><td>1</td></tr><tr><td>4</td><td>RB_UMS_R_FIFO_RDY</td><td>RO</td><td>USB接收FIFO数据就绪状态位:1:接收FIFO非空;0:接收FIFO为空。</td><td>0</td></tr><tr><td>3</td><td>RB_UMS_BUS_RST</td><td>RO</td><td>USB总线复位状态位:1:当前USB总线处于复位态;0:当前USB总线处于非复位态。</td><td>X</td></tr><tr><td>2</td><td>RB_UMS_SUSPEND</td><td>RO</td><td>USB挂起状态位:1:USB总线处于挂起态,有一段时间没有USB活动;0:USB总线处于非挂起态。</td><td>0</td></tr><tr><td>1</td><td>RB_UMS_DEV_ATTACH</td><td>RO</td><td>USB主机模式下端口的USB设备连接状态位:1:端口已经连接USB设备;0:端口没有USB设备连接。</td><td>0</td></tr><tr><td>0</td><td>RB_UMS_SPLIT_CAN</td><td>RO</td><td>USB主机模式下,SPLIT包发送允许位:1:允许发送SPLIT包;0:禁止发送</td><td>0</td></tr></table>

22.2.1.8 USB 中断标志寄存器（R8_USB_INT_FG）  

<table><tr><td>位</td><td>名称</td><td>访问</td><td>描述</td><td>复位值</td></tr><tr><td>7</td><td>Reserved</td><td>R0</td><td>保留</td><td>0</td></tr><tr><td>6</td><td>RB_UID_ISO_ACT</td><td>R0</td><td>同步传输开始发送/接收数据中断标志位,写1清零:1:开始发送/接收数据触发;0:无事件。注:对于接收如果接收CRC16错误,将不会产生UID_TRANSFER,否则在事务完成后依然会产生UID_TRANSFER</td><td>0</td></tr><tr><td>5</td><td>RB_UID_setup_ACT</td><td>R0</td><td>SETUP事务完成中断标志位,写1清零:1:SETUP事务完成;0:无事件。</td><td>1</td></tr><tr><td>4</td><td>RB_UID_FIFO0_0V</td><td>RW</td><td>USB FIFO 溢出中断标志位,写1清零:1: FIFO 溢出触发; 0:无事件。</td><td>0</td></tr><tr><td>3</td><td>RB_UID_HST_SOF</td><td>RW</td><td>USB 主机模式下 SOF 定时中断标志位,写1清零:1: SOF 包传输完成触发;0:无事件。</td><td>0</td></tr><tr><td>2</td><td>RB_UID_SUSPEND</td><td>RW</td><td>USB 总线挂起或唤醒事件中断标志位,写1清零:1: USB 挂起事件或唤醒事件触发;0:无事件。</td><td>0</td></tr><tr><td>1</td><td>RB_UID_TRANSFER</td><td>RW</td><td>USB 传输完成中断标志位,写1清零:1: 一个 USB 传输完成触发;0:无事件。</td><td>0</td></tr><tr><td rowspan="2">0</td><td>RB_UID_DETECT</td><td>RW</td><td>USB 主机模式下 USB 设备连接或断开事件中断标志位,写1清零:1: 检测到 USB 设备连接或断开触发;0:无事件。</td><td>0</td></tr><tr><td>RB_UID_BUS_RST</td><td>RW</td><td>USB 设备模式下 USB 总线复位事件中断标志位,写1清零:1: USB 总线复位事件触发;0:无事件。</td><td>0</td></tr></table>

22.2.1.9 USB 中断状态寄存器（R8_USB_INT_ST）  

<table><tr><td>位</td><td>名称</td><td>访问</td><td>描述</td><td>复位值</td></tr><tr><td>7</td><td>RB_UID_IS_NAK</td><td>R0</td><td>USB 设备模式下,NAK 响应状态位,同RB_U_IS_NAK:1:当前USB传输过程中回应NAK;0:无NAK响应。</td><td>0</td></tr><tr><td>6</td><td>RB_UID_TOG_OK</td><td>R0</td><td>USB事务接收完成后,接收到的数据包的 Toggle 与设置的期望值匹配状态位:1:toggle 匹配;0:toggle 不匹配。</td><td>0</td></tr><tr><td>[5:4]</td><td>MASK_UID_TOKEN</td><td>R0</td><td>设备模式下,当前USB传输事务的令牌PID标识。</td><td>X</td></tr><tr><td rowspan="2">[3:0]</td><td>MASK_UID_ENDP</td><td>R0</td><td>设备模式下,当前USB传输事务的端点号。</td><td>X</td></tr><tr><td>MASK_UID_H_RES</td><td>R0</td><td>主机模式下,当前USB传输事务的应答PID标识,0000 表示设备无应答或超时;其它值表示应答PID。</td><td>X</td></tr></table>

注：MASK_UIS_TOKEN用于USB设备模式下标识当前USB传输事务的令牌PID：O0表示OUT包；01表示 SOF包；10表示IN包；11表示SETUP包。

MASK_UIS_H_RES仅在主机模式下有效。在主机模式下，若主机发送OUT/SETUP令牌包时，则该是握手包 ACK/NAK/STALL， 或是设备无应答/超时。 若主机发送 IN 令牌包， 则该 PID 是数据包的PID（DATAO/DATA1）或握手包PID。

#### 22.2.1.10 USB 接收长度寄存器（R16_USB_RX_LEN）

<table><tr><td>位</td><td>名称</td><td>访问</td><td>描述</td><td>复位值</td></tr><tr><td>[15:0]</td><td>R16_USB_RX_LEN</td><td>R0</td><td>当前USB端点接收的数据字节数</td><td>X</td></tr></table>

### 22.2.2 设备寄存器描述

USBHS模块在USB设备模式下，提供了端点 0-15共 16组双向端点，除端点 0 之外的所有端点的最大数据包长度都是 1024 字节，端点 0 的最大数据包长度为 64 字节。

端点0是默认端点，支持控制传输，发送和接收共用一个 64字节数据缓冲区。  
端点1-15各自包括一个发送端点IN和一个接收端点 OUT，发送和接收各有一个独立的数据缓冲区，支持批量传输、中断传输和实时/同步传输。  
端点 0 具有独立的 DMA 地址，收发共用，端点 1~15 的发送和接收各有一个 DMA 地址。通过入R32_UEPn_BUF_MOD 寄存器可以设置数据缓冲区的模式为双缓冲或单缓冲。若使用双缓冲区模式，该端点只能使用单方向传输。  
每组端点都具有收发控制寄存器 R8_UEPn_TX_CTRL、R8_UEPn_RX_CTRL 和发送长度寄存器R16_UEPn_T_LEN 和 R32_UEPn_*_DMA（ $\scriptstyle \mathbf { n = 0 } ^ { \sim } 1 5 ,$ ），用于配置该端点的同步触发位、对 OUT 事务和IN 事务的响应以及发送数据的长度等。

作为 USB 设备所必要的 USB 总线上拉电阻可以由软件随时设置是否启用，当 USB 控制寄存器R8_USB_CTRL 中的 RB_UC_DEV_PU_EN 置 1 时，控制器根据 RB_UC_SPEED_TYPE 的速度设置，在内部为USB总线的DP/DM引脚连接上拉电阻，并启用 USB设备功能。

当检测到USB总线复位、USB总线挂起或唤醒事件，或当 USB成功处理完数据发送或数据接收后，USB协议处理器都将设置相应的中断标志，如果中断使能打开，还会产生相应的中断请求。应用程序可以直接查询或在 USB 中断服务程序中查询并分析中断标志寄存器 R8_USB_INT_FG，根据RB_UIF_BUS_RST 和 RB_UIF_SUSPEND 进行相应的处理；并且，如果 RB_UIF_TRANSFER 有效，那么还需要继续分析 USB 中断状态寄存器 R8_USB_INT_ST，根据当前端点号 MASK_UIS_ENDP 和当前事务令牌PID 标识 MASK_UIS_TOKEN 进行相应的处理。如果事先设定了各个端点的 OUT 事务的同步触发位RB_UEP_R_TOG，那么可以通过 RB_U_TOG_OK 或 RB_UIS_TOG_OK 判断当前所接收到的数据包的同步触发位是否与该端点的同步触发位匹配，如果数据同步，则数据有效；如果数据不同步，则数据应该被丢弃。每次处理完USB发送或接收中断后，都应该正确修改相应端点的同步触发位，用于下次所发送的数据包或下次所接收的数据包是否同步检测；另外，设置 RB_UEP_T_TOG_AUTO 或 RB_UEP_R_TOG_AUTO可以实现在发送成功或接收成功后自动修改相应的同步触发位（翻转或自减）。

各个端点准备发送的数据在各自的缓冲区中，准备发送的数据长度是独立设定在 R16_UEPn_T_LEN中；各个端点接收到的数据在各自的缓冲区中，但是接收到的数据长度都在 USB 接收长度寄存器R16_USB_RX_LEN中，可以在USB接收中断时根据当前端点号区分。

表 22-2 设备相关寄存器列表  

<table><tr><td>名称</td><td>访问地址</td><td>描述</td><td>复位值</td></tr><tr><td>R32_UEP_CONFIG</td><td>0x40023410</td><td>端点使能配置寄存器</td><td>00000000h</td></tr><tr><td>R32_UEP_TYPE</td><td>0x40023414</td><td>端点类型配置寄存器</td><td>00000000h</td></tr><tr><td>R32_UEP_buf_MOD</td><td>0x40023418</td><td>端点缓冲区模式寄存器</td><td>00000000h</td></tr><tr><td>R32_UEPO_DMA</td><td>0x4002341C</td><td>端点0缓冲区的起始地址</td><td>xxxxh</td></tr><tr><td>R32_UEP1_RX_DMA</td><td>0x40023420</td><td>端点1接收缓冲区的起始地址</td><td>xxxxh</td></tr><tr><td>R32_UEP2_RX_DMA</td><td>0x40023424</td><td>端点2接收缓冲区的起始地址</td><td>xxxxh</td></tr><tr><td>R32_UEP3_RX_DMA</td><td>0x40023428</td><td>端点3接收缓冲区的起始地址</td><td>xxxxh</td></tr><tr><td>R32_UEP4_RX_DMA</td><td>0x4002342C</td><td>端点4接收缓冲区的起始地址</td><td>xxxxh</td></tr><tr><td>R32_UEP5_RX_DMA</td><td>0x40023430</td><td>端点5接收缓冲区的起始地址</td><td>xxxxh</td></tr><tr><td>R32_UEP6_RX_DMA</td><td>0x40023434</td><td>端点6接收缓冲区的起始地址</td><td>xxxxh</td></tr><tr><td>R32_UEP7_RX_DMA</td><td>0x40023438</td><td>端点7接收缓冲区的起始地址</td><td>xxxxh</td></tr><tr><td>R32_UEP8_RX_DMA</td><td>0x4002343C</td><td>端点8接收缓冲区的起始地址</td><td>xxxxh</td></tr><tr><td>R32_UEP9_RX_DMA</td><td>0x40023440</td><td>端点9接收缓冲区的起始地址</td><td>xxxxh</td></tr><tr><td>R32_UEP10_RX_DMA</td><td>0x40023444</td><td>端点10接收缓冲区的起始地址</td><td>xxxxh</td></tr><tr><td>R32_UEP11_RX_DMA</td><td>0x40023448</td><td>端点11接收缓冲区的起始地址</td><td>xxxxh</td></tr><tr><td>R32_UEP12_RX_DMA</td><td>0x4002344C</td><td>端点12接收缓冲区的起始地址</td><td>xxxxh</td></tr><tr><td>R32_UEP13_RX_DMA</td><td>0x40023450</td><td>端点13接收缓冲区的起始地址</td><td>xxxxh</td></tr><tr><td>R32_UEP14_RX_DMA</td><td>0x40023454</td><td>端点14接收缓冲区的起始地址</td><td>xxxxh</td></tr><tr><td>R32_UEP15_RX_DMA</td><td>0x40023458</td><td>端点15接收缓冲区的起始地址</td><td>xxxxh</td></tr><tr><td>R32_UEP1_TX_DMA</td><td>0x4002345C</td><td>端点1发送缓冲区的起始地址</td><td>xxxxh</td></tr><tr><td>R32_UEP2_TX_DMA</td><td>0x40023460</td><td>端点2发送缓冲区的起始地址</td><td>xxxxh</td></tr><tr><td>R32_UEP3_TX_DMA</td><td>0x40023464</td><td>端点3发送缓冲区的起始地址</td><td>xxxxh</td></tr><tr><td>R32_UEP4_TX_DMA</td><td>0x40023468</td><td>端点4发送缓冲区的起始地址</td><td>xxxxh</td></tr><tr><td>R32_UEP5_TX_DMA</td><td>0x4002346C</td><td>端点5发送缓冲区的起始地址</td><td>xxxxh</td></tr><tr><td>R32_UEP6_TX_DMA</td><td>0x40023470</td><td>端点6发送缓冲区的起始地址</td><td>xxxxh</td></tr><tr><td>R32_UEP7_TX_DMA</td><td>0x40023474</td><td>端点7发送缓冲区的起始地址</td><td>xxxxh</td></tr><tr><td>R32_UEP8_TX_DMA</td><td>0x40023478</td><td>端点8发送缓冲区的起始地址</td><td>xxxxh</td></tr><tr><td>R32_UEP9_TX_DMA</td><td>0x4002347C</td><td>端点9发送缓冲区的起始地址</td><td>xxxxh</td></tr><tr><td>R32_UEP10_TX_DMA</td><td>0x40023480</td><td>端点10发送缓冲区的起始地址</td><td>xxxxh</td></tr><tr><td>R32_UEP11_TX_DMA</td><td>0x40023484</td><td>端点11发送缓冲区的起始地址</td><td>xxxxh</td></tr><tr><td>R32_UEP12_TX_DMA</td><td>0x40023488</td><td>端点12发送缓冲区的起始地址</td><td>xxxxh</td></tr><tr><td>R32_UEP13_TX_DMA</td><td>0x4002348C</td><td>端点13发送缓冲区的起始地址</td><td>xxxxh</td></tr><tr><td>R32_UEP14_TX_DMA</td><td>0x40023490</td><td>端点14发送缓冲区的起始地址</td><td>xxxxh</td></tr><tr><td>R32_UEP15_TX_DMA</td><td>0x40023494</td><td>端点15发送缓冲区的起始地址</td><td>xxxxh</td></tr><tr><td>R16_UEPO_MAX_LEN</td><td>0x40023498</td><td>端点0最大长度包寄存器</td><td>xxxxh</td></tr><tr><td>R16_UEP1_MAX_LEN</td><td>0x4002349C</td><td>端点1最大长度包寄存器</td><td>xxxxh</td></tr><tr><td>R16_UEP2_MAX_LEN</td><td>0x400234A0</td><td>端点2最大长度包寄存器</td><td>xxxxh</td></tr><tr><td>R16_UEP3_MAX_LEN</td><td>0x400234A4</td><td>端点3最大长度包寄存器</td><td>xxxxh</td></tr><tr><td>R16_UEP4_MAX_LEN</td><td>0x400234A8</td><td>端点4最大长度包寄存器</td><td>xxxxh</td></tr><tr><td>R16_UEP5_MAX_LEN</td><td>0x400234AC</td><td>端点5最大长度包寄存器</td><td>xxxxh</td></tr><tr><td>R16_UEP6_MAX_LEN</td><td>0x400234B0</td><td>端点6最大长度包寄存器</td><td>xxxxh</td></tr><tr><td>R16_UEP7_MAX_LEN</td><td>0x400234B4</td><td>端点7最大长度包寄存器</td><td>xxxxh</td></tr><tr><td>R16_UEP8_MAX_LEN</td><td>0x400234B8</td><td>端点8最大长度包寄存器</td><td>xxxxh</td></tr><tr><td>R16_UEP9_MAX_LEN</td><td>0x400234BC</td><td>端点9最大长度包寄存器</td><td>xxxxh</td></tr><tr><td>R16_UEP10_MAX_LEN</td><td>0x400234C0</td><td>端点10最大长度包寄存器</td><td>xxxxh</td></tr><tr><td>R16_UEP11_MAX_LEN</td><td>0x400234C4</td><td>端点11最大长度包寄存器</td><td>xxxxh</td></tr><tr><td>R16_UEP12_MAX_LEN</td><td>0x400234C8</td><td>端点12最大长度包寄存器</td><td>xxxxh</td></tr><tr><td>R16_UEP13_MAX_LEN</td><td>0x400234CC</td><td>端点13最大长度包寄存器</td><td>xxxxh</td></tr><tr><td>R16_UEP14_MAX_LEN</td><td>0x400234D0</td><td>端点14最大长度包寄存器</td><td>xxxxh</td></tr><tr><td>R16_UEP15_MAX_LEN</td><td>0x400234D4</td><td>端点15最大长度包寄存器</td><td>xxxxh</td></tr><tr><td>R16_UEPO_T_LEN</td><td>0x400234D8</td><td>端点0发送长度寄存器</td><td>xxxxh</td></tr><tr><td>R8_UEPO_TX_CTRL</td><td>0x400234DA</td><td>端点0发送控制寄存器</td><td>00h</td></tr><tr><td>R8_UEPO_RX_CTRL</td><td>0x400234DB</td><td>端点0接收控制寄存器</td><td>00h</td></tr><tr><td>R16_UEP1_T_LEN</td><td>0x400234DC</td><td>端点1发送长度寄存器</td><td>xxxxh</td></tr><tr><td>R8_UEP1_TX_CTRL</td><td>0x400234DE</td><td>端点1发送控制寄存器</td><td>00h</td></tr><tr><td>R8_UEP1_RX_CTRL</td><td>0x400234DF</td><td>端点1接收控制寄存器</td><td>00h</td></tr><tr><td>R16_UEP2_T_LEN</td><td>0x400234E0</td><td>端点2发送长度寄存器</td><td>xxxxh</td></tr><tr><td>R8_UEP2_TX_CTRL</td><td>0x400234E2</td><td>端点2发送控制寄存器</td><td>00h</td></tr><tr><td>R8_UEP2_RX_CTRL</td><td>0x400234E3</td><td>端点2接收控制寄存器</td><td>00h</td></tr><tr><td>R16_UEP3_T_LEN</td><td>0x400234E4</td><td>端点3发送长度寄存器</td><td>xxxxh</td></tr><tr><td>R8_UEP3_TX_CTRL</td><td>0x400234E6</td><td>端点3发送控制寄存器</td><td>00h</td></tr><tr><td>R8_UEP3_RX_CTRL</td><td>0x400234E7</td><td>端点3接收控制寄存器</td><td>00h</td></tr><tr><td>R16_UEP4_T_LEN</td><td>0x400234E8</td><td>端点4发送长度寄存器</td><td>xxxxh</td></tr><tr><td>R8_UEP4_TX_CTRL</td><td>0x400234EA</td><td>端点4发送控制寄存器</td><td>00h</td></tr><tr><td>R8_UEP4_RX_CTRL</td><td>0x400234EB</td><td>端点4接收控制寄存器</td><td>00h</td></tr><tr><td>R16_UEP5_T_LEN</td><td>0x400234EC</td><td>端点5发送长度寄存器</td><td>xxxxh</td></tr><tr><td>R8_UEP5_TX_CTRL</td><td>0x400234EE</td><td>端点5发送控制寄存器</td><td>00h</td></tr><tr><td>R8_UEP5_RX_CTRL</td><td>0x400234EF</td><td>端点5接收控制寄存器</td><td>00h</td></tr><tr><td>R16_UEP6_T_LEN</td><td>0x400234F0</td><td>端点6发送长度寄存器</td><td>xxxxh</td></tr><tr><td>R8_UEP6_TX_CTRL</td><td>0x400234F2</td><td>端点6发送控制寄存器</td><td>00h</td></tr><tr><td>R8_UEP6_RX_CTRL</td><td>0x400234F3</td><td>端点6接收控制寄存器</td><td>00h</td></tr><tr><td>R16_UEP7_T_LEN</td><td>0x400234F4</td><td>端点7发送长度寄存器</td><td>xxxxh</td></tr><tr><td>R8_UEP7_TX_CTRL</td><td>0x400234F6</td><td>端点7发送控制寄存器</td><td>00h</td></tr><tr><td>R8_UEP7_RX_CTRL</td><td>0x400234F7</td><td>端点7接收控制寄存器</td><td>00h</td></tr><tr><td>R16_UEP8_T_LEN</td><td>0x400234F8</td><td>端点8发送长度寄存器</td><td>xxxxh</td></tr><tr><td>R8_UEP8_TX_CTRL</td><td>0x400234FA</td><td>端点8发送控制寄存器</td><td>00h</td></tr><tr><td>R8_UEP8_RX_CTRL</td><td>0x400234FB</td><td>端点8接收控制寄存器</td><td>00h</td></tr><tr><td>R16_UEP9_T_LEN</td><td>0x400234FC</td><td>端点9发送长度寄存器</td><td>xxxxh</td></tr><tr><td>R8_UEP9_TX_CTRL</td><td>0x400234FE</td><td>端点9发送控制寄存器</td><td>00h</td></tr><tr><td>R8_UEP9_RX_CTRL</td><td>0x400234FF</td><td>端点9接收控制寄存器</td><td>00h</td></tr><tr><td>R16_UEP10_T_LEN</td><td>0x40023500</td><td>端点10发送长度寄存器</td><td>xxxxh</td></tr><tr><td>R8_UEP10_TX_CTRL</td><td>0x40023502</td><td>端点10发送控制寄存器</td><td>00h</td></tr><tr><td>R8_UEP10_RX_CTRL</td><td>0x40023503</td><td>端点10接收控制寄存器</td><td>00h</td></tr><tr><td>R16_UEP11_T_LEN</td><td>0x40023504</td><td>端点11发送长度寄存器</td><td>xxxxh</td></tr><tr><td>R8_UEP11_TX_CTRL</td><td>0x40023506</td><td>端点11发送控制寄存器</td><td>00h</td></tr><tr><td>R8_UEP11_RX_CTRL</td><td>0x40023507</td><td>端点11接收控制寄存器</td><td>00h</td></tr><tr><td>R16_UEP12_T_LEN</td><td>0x40023508</td><td>端点12发送长度寄存器</td><td>xxxxh</td></tr><tr><td>R8_UEP12_TX_CTRL</td><td>0x4002350A</td><td>端点12发送控制寄存器</td><td>00h</td></tr><tr><td>R8_UEP12_RX_CTRL</td><td>0x4002350B</td><td>端点12接收控制寄存器</td><td>00h</td></tr><tr><td>R16_UEP13_T_LEN</td><td>0x4002350C</td><td>端点13发送长度寄存器</td><td>xxxxh</td></tr><tr><td>R8_UEP13_TX_CTRL</td><td>0x4002350E</td><td>端点13发送控制寄存器</td><td>00h</td></tr><tr><td>R8_UEP13_RX_CTRL</td><td>0x4002350F</td><td>端点13接收控制寄存器</td><td>00h</td></tr><tr><td>R16_UEP14_T_LEN</td><td>0x40023510</td><td>端点14发送长度寄存器</td><td>xxxxh</td></tr><tr><td>R8_UEP14_TX_CTRL</td><td>0x40023512</td><td>端点14发送控制寄存器</td><td>00h</td></tr><tr><td>R8_UEP14_RX_CTRL</td><td>0x40023513</td><td>端点14接收控制寄存器</td><td>00h</td></tr><tr><td>R16_UEP15_T_LEN</td><td>0x40023514</td><td>端点15发送长度寄存器</td><td>xxxxh</td></tr><tr><td>R8_UEP15_TX_CTRL</td><td>0x40023516</td><td>端点15发送控制寄存器</td><td>00h</td></tr><tr><td>R8_UEP15_RX_CTRL</td><td>0x40023517</td><td>端点15接收控制寄存器</td><td>00h</td></tr></table>

22.2.2.1 USB 端点配置寄存器（R32_UEP_CONFIG）  

<table><tr><td>位</td><td>名称</td><td>访问</td><td>描述</td><td>复位值</td></tr><tr><td>[31:17]</td><td>RB_UEP_R_EN</td><td>RW</td><td>端点1-15接收使能</td><td>0</td></tr><tr><td>16</td><td>Reserved</td><td>R0</td><td>保留</td><td>0</td></tr><tr><td>[15:1]</td><td>RB_UEP_T_EN</td><td>RW</td><td>端点1-15发送使能</td><td>0</td></tr><tr><td>0</td><td>Reserved</td><td>R0</td><td>保留</td><td>0</td></tr></table>

注：端点0的收发使能信号始终有效。

22.2.2.2 USB 端点类型控制寄存器（R32_UEP_TYPE）  

<table><tr><td>位</td><td>名称</td><td>访问</td><td>描述</td><td>复位值</td></tr><tr><td>[31:17]</td><td>RB_UEP_R_TYPE</td><td>RW</td><td>端点1-15,OUT方向传输类型,1表示同步传输,</td><td>0</td></tr><tr><td>16</td><td>Reserved</td><td>R0</td><td>保留</td><td>0</td></tr><tr><td>[15:1]</td><td>RB_UEP_T_TYPE</td><td>RW</td><td>端点1-15,IN方向传输类型,1表示同步传输</td><td>0</td></tr><tr><td>0</td><td>Reserved</td><td>R0</td><td>保留</td><td>0</td></tr></table>

22.2.2.3 USB 端点缓冲区模式控制寄存器（R32_UEP_BUF_MOD）  

<table><tr><td>位</td><td>名称</td><td>访问</td><td>描述</td><td>复位值</td></tr><tr><td>[31:16]</td><td>RB_UEP_ISO BUF_MOD</td><td>RW</td><td>同步端点缓冲区模式控制位,1有效。</td><td>0</td></tr><tr><td>[15:0]</td><td>RB_UEP BUF_MOD</td><td>RW</td><td>端点缓冲区模式控制位</td><td>0</td></tr></table>

注：当RB_UEP_ISO_BUF_MOD为1时，对于同步IN端点，在接收到 SOF包后，硬件会有以下操作：将 $E P x \_ R \_ T O G$ 内容加载到 EPx_T_TOG中；将EPx_MAX_LEN值加载到 EPx_T_LEN中；将UEPn_RX_DMA值加载到UEPn_TX_DMA中。

当RB_UEP_ISO_BUF_MOD为1时，对于同步OUT端点，在接收到 SOF包后，硬件会有以下操作：将 EPx_T_TOG 内容加载到 EPx_R_TOG中；将 UEPn_TX_DMA的值加载到UEPn_RX_DMA中。

表 21-3 端点 n 缓冲区模式（ $\scriptstyle \mathbf { \bar { n } } = 1 - 1 5 )$ ）  

<table><tr><td>UEPn_RX_EN</td><td>UEPn_TX_EN</td><td>UEPnBUF_MOD</td><td>描述:以UEPn_DMA为起始地址由低向高排列</td></tr><tr><td>0</td><td>0</td><td>x</td><td>端点被禁用,未用到UEPn_*DMA缓冲区。</td></tr><tr><td>1</td><td>0</td><td>0</td><td>接收(OUT)缓冲区首地址为UEPn_RX_DMA</td></tr><tr><td>1</td><td>0</td><td>1</td><td>RB_UEPn_RX_TOG[0]=0,使用缓冲区UEPn_RX_DMA
RB_UEPn_RX_TOG[0]=1,使用缓冲区UEPn_TX_DMA</td></tr><tr><td>0</td><td>1</td><td>0</td><td>发送(IN)缓冲区首地址为UEPn_TX_DMA。</td></tr><tr><td>0</td><td>1</td><td>1</td><td>RB_UEPn_RX_TOG[0]=0,使用缓冲区UEPn_TX_DMA
RB_UEPn_RX_TOG[0]=1,使用缓冲区UEPn_RX_DMA</td></tr></table>

端点 n 缓冲区起始地址（R32_UEP0_DMA）  

<table><tr><td>位</td><td>名称</td><td>访问</td><td>描述</td><td>复位值</td></tr><tr><td>[31:17]</td><td>Reserved</td><td>R0</td><td>保留</td><td>0</td></tr><tr><td>[16:0]</td><td>R32_UEPn_DMA</td><td>RW</td><td>端点0缓冲区起始地址。
低16位有效，地址必须4字节对齐。</td><td>X</td></tr></table>

USB 端点 n 发送缓冲区起始地址（R32_UEPn_TX_DMA）（n=1-15）

<table><tr><td>位</td><td>名称</td><td>访问</td><td>描述</td><td>复位值</td></tr><tr><td>[31:17]</td><td>Reserved</td><td>R0</td><td>保留</td><td>0</td></tr><tr><td>[16:0]</td><td>R32_UEPn_TX_DMA</td><td>RW</td><td>端点n发送缓冲区起始地址。低16位有效，地址必须4字节对齐。</td><td>X</td></tr></table>

USB 端点 n 接收缓冲区起始地址（R32_UEPn_RX_DMA）（n=1-15）

<table><tr><td>位</td><td>名称</td><td>访问</td><td>描述</td><td>复位值</td></tr><tr><td>[31:17]</td><td>Reserved</td><td>R0</td><td>保留</td><td>0</td></tr><tr><td>[16:0]</td><td>R32_UEPn_RX_DMA</td><td>RW</td><td>端点n接收缓冲区起始地址。低16位有效，地址必须4字节对齐。</td><td>X</td></tr></table>

22.2.2.4 端点 n 最大长度包寄存器（R16_UEPn_MAX_LEN）（n=0-15）

<table><tr><td>位</td><td>名称</td><td>访问</td><td>描述</td><td>复位值</td></tr><tr><td>[15:11]</td><td>Reserved</td><td>R0</td><td>保留</td><td>0</td></tr><tr><td>[10:0]</td><td>UEPn_MAX_LEN</td><td>RW</td><td>端点n接收数据的最大包长度。</td><td>X</td></tr></table>

注：这个最大包长度决定了端点可接收数据最大长度，超出此长度的数据会被丢弃，不会写入缓冲区。

22.2.2.5 端点 n 发送长度寄存器（R16_UEPn_T_LEN）（n=0-15）

<table><tr><td>位</td><td>名称</td><td>访问</td><td>描述</td><td>复位值</td></tr><tr><td>[10:0]</td><td>UEPn_T_LEN</td><td>RW</td><td>设置USB端点n准备发送的数据字节数,对于控制端点(0),低7位有效。</td><td>X</td></tr></table>

22.2.2.6 端点 n 发送控制寄存器（R8_UEPn_TX_CTRL）（n=0-15）

<table><tr><td>位</td><td>名称</td><td>访问</td><td>描述</td><td>复位值</td></tr><tr><td>[7:6]</td><td>Reserved</td><td>R0</td><td>保留</td><td>0</td></tr><tr><td>5</td><td>RB_UEP_T_TOG_AUTO</td><td>RW</td><td>同步触发位自动翻转使能控制位,软件可修改:1:对于非同步端点,数据发送成功后自动翻转MASK_UEP_T_TOG [0],对于同步端点,数据发送成功后MASK_UEP_T_TOG自动减10:不自动翻转,可以手动切换。注:端点0此位为保留位。</td><td>0</td></tr><tr><td>[4:3]</td><td>MASK_UEP_T_TOG</td><td>RW</td><td>USB端点n的发送器(处理IN事务)准备的同步触发位:00:发送DATA0;01:发送DATA1;10:发送DATA2;11:发送MDATA。</td><td>00b</td></tr><tr><td>2</td><td>Reserved</td><td>R0</td><td>保留</td><td>0</td></tr><tr><td>[1:0]</td><td>MASK_UEP_T_RES</td><td>RW</td><td>端点n的发送器对IN事务的响应控制:00:数据就绪并期望ACK;10:应答NAK或忙;11:应答STALL或错误。</td><td>00b</td></tr></table>

22.2.2.7 端点 n 接收控制寄存器（R8_UEPn_RX_CTRL）（n=0-15）  

<table><tr><td>位</td><td>名称</td><td>访问</td><td>描述</td><td>复位值</td></tr><tr><td>[7:6]</td><td>Reserved</td><td>R0</td><td>保留</td><td>0</td></tr><tr><td>5</td><td>RB_UEP_R_TOG_AUTO</td><td>R0</td><td>同步触发位自动翻转使能控制位:1:数据接收成功后自动翻转MASK_UEP_R_TOG[0]0:不自动翻转,可以手动切换。注:端点0此位为保留位。</td><td>0</td></tr><tr><td>[4:3]</td><td>MASK_UEP_R_TOG</td><td>RW</td><td>USB端点n的接收器(处理OUT事务)准备的同步触发位:00:期望DATA0;01:期望DATA1;10:期望DATA2;11:期望MDATA。对于实时/同步传输无效。</td><td>00b</td></tr><tr><td>2</td><td>Reserved</td><td>R0</td><td>保留</td><td>0</td></tr><tr><td>[1:0]</td><td>MASK_UEP_R_RES</td><td>RW</td><td>端点n的接收器对OUT事务的响应控制:00:数据就绪并期望ACK;10:应答NAK或忙;11:应答STALL或错误;01:应答NYET。对于实时/同步传输无效。</td><td>00b</td></tr></table>

### 22.2.3 USB 主机寄存器

在USB主机模式下，芯片提供了一组双向主机端点，包括一个发送端点 OUT和一个接收端点 IN，一个数据包的最大长度是1024字节（同步传输），支持控制传输、中断传输、批量传输和实时/同步传输。

主机端点发起的每一个USB事务，在处理结束后总是自动设置 RB_UIF_TRANSFER中断标志。应用程序可以直接查询或在USB中断服务程序中查询并分析中断标志寄存器 R8_USB_INT_FG，根据各中断标志分别进行相应的处理；并且，如果RB_UIF_TRANSFER有效，那么还需要继续分析 USB中断状态寄存器 R8_USB_INT_ST，根据当前 USB 传输事务的应答 PID 标识 MASK_UIS_H_RES 进行相应的处理。

如果事先设定了主机接收端点的 IN 事务的同步触发位（RB_UH_R_TOG），那么可以通过RB_U_TOG_OK 或 RB_UIS_TOG_OK 判断当前所接收到的数据包的同步触发位是否与主机接收端点的同步触发位匹配，如果数据同步，则数据有效；如果数据不同步，则数据应该被丢弃。每次处理完 USB发送或接收中断后，都应该正确修改相应主机端点的同步触发位，用于同步下次所发送的数据包和检测下次所接收的数据包是否同步；另外，通过设置 RB_UH_T_AUTO_TOG 和 RB_UH_R_AUTO_TOG 可以实现在发送成功或接收成功后自动翻转相应的同步触发位。

USB 主机令牌设置寄存器 R8_UH_EP_PID 用于设置被操作的目标设备的端点号和本次 USB 传输事务的令牌PID包标识。SETUP令牌和OUT令牌所对应的数据由主机发送端点提供，准备发送的数据在R16_UH_TX_DMA缓冲区中，准备发送的数据长度设置在 R16_UH_TX_LEN中；IN令牌所对应数据由目标设备返回给主机接收端点，接收到数据存放 R16_UH_RX_DMA 缓冲区中，接收到的数据长度存放在

R16_USB_RX_LEN 中。

表22-3 主机相关寄存器列表  

<table><tr><td>名称</td><td>访问地址</td><td>描述</td><td>复位值</td></tr><tr><td>R8_UHOST_CTRL</td><td>0x40023401</td><td>USB 主机控制寄存器</td><td>00h</td></tr><tr><td>R32_UH_CONFIG</td><td>0x40023410</td><td>USB 主机端点配置寄存器</td><td>00000000h</td></tr><tr><td>R32_UH_EP_TYPE</td><td>0x40023414</td><td>USB 主机端点类型寄存器</td><td>00000000h</td></tr><tr><td>R32_UH_RX_DMA</td><td>0x40023424</td><td>USB 主机接收缓冲区起始地址</td><td>16xxxxx</td></tr><tr><td>R32_UH_TX_DMA</td><td>0x40023464</td><td>USB 主机发送缓冲区起始地址</td><td>16xxxxx</td></tr><tr><td>R16_UH_RX_MAX_LEN</td><td>0x400234A0</td><td>USB 主机接收最大长度包寄存器</td><td>16xxxxx</td></tr><tr><td>R8_UH_EP_PID</td><td>0x400234E0</td><td>USB 主机令牌设置寄存器</td><td>8h00</td></tr><tr><td>R8_UH_RX_CTRL</td><td>0x400234E3</td><td>USB 主机接收端点控制寄存器</td><td>8h00</td></tr><tr><td>R16_UH_TX_LEN</td><td>0x400234E4</td><td>USB 主机发送长度寄存器</td><td>16xxxxx</td></tr><tr><td>R8_UH_TX_CTRL</td><td>0x400234E6</td><td>USB 主机发送端点控制寄存器</td><td>8h00</td></tr><tr><td>R16_UH_SPLIT_DATA</td><td>0x400234E8</td><td>USB 主机发送 SPLIT 包的数据</td><td>16xxxxx</td></tr></table>

22.2.3.1 USB 主机控制寄存器（R8_UHOST_CTRL）  

<table><tr><td>位</td><td>名称</td><td>访问</td><td>描述</td><td>复位值</td></tr><tr><td>7</td><td>RB_UH_SOF_EN</td><td>RW</td><td>自动产生SOF包使能控制位:1:主机自动发生SOF包;0:不产生SOF包。该位在从连接状态变为断开状态时由硬件自动清零</td><td>0</td></tr><tr><td>6</td><td>RB_UH_SOF-Free</td><td>RO</td><td>总线空闲</td><td>0</td></tr><tr><td>5</td><td>Reserved</td><td>RO</td><td>保留。</td><td>0</td></tr><tr><td>4</td><td>RB_UH_PHY_SUSPENDM</td><td>RW</td><td>USB-PHY的处于挂起状态,内部USB-PLL将被关闭,低有效</td><td>0</td></tr><tr><td>3</td><td>RB_UH remotework</td><td>RW</td><td>远程唤醒。</td><td>0</td></tr><tr><td>2</td><td>RB_UH_TX_BUS_RESUME</td><td>RW</td><td>主机模式下,表示主机唤醒设备,软件拉高50ns后,硬件自动发送30ms的唤醒信号。</td><td>0</td></tr><tr><td>1</td><td>RB_UH_TX_BUS_SUSPEND</td><td>RW</td><td>USB主机发送挂起信号,需由软件拉高10ms。</td><td>0</td></tr><tr><td>0</td><td>RB_UH_TX_BUS_RST</td><td>RW</td><td>USB主机发送总线复位信号,需由软件拉高10ms。</td><td>0</td></tr></table>

注：复位的时间由RB_UH_TX_BUS_RST的高电平持续时间决定（建议至少10ms，10ms后直接查询速度类型）。如果主机唤醒设备，bUH_TX_BUS_RESUME拉高之后，硬件自动发送30ms的唤醒信号（K），bUH_TX_BUS_RESUME需要手动清除，以免影响下一次主机挂起（bUH_TX_BUS_RESUME高电平至少维持50ns）。

22.2.3.2 USB 主机端点配置控制寄存器（R32_UH_CONFIG）  

<table><tr><td>位</td><td>名称</td><td>访问</td><td>描述</td><td>复位值</td></tr><tr><td>[31:19]</td><td>Reserved</td><td>R0</td><td>保留</td><td>0</td></tr><tr><td>18</td><td>RB_UH_EP_RX_EN</td><td>RW</td><td>主机接收使能</td><td>0</td></tr><tr><td>[17:4]</td><td>Reserved</td><td>R0</td><td>保留。</td><td>0</td></tr><tr><td>3</td><td>RB_UH_EP_TX_EN</td><td>RW</td><td>主机发送使能</td><td>0</td></tr><tr><td>[2:0]</td><td>Reserved</td><td>R0</td><td>保留。</td><td>0</td></tr></table>

22.2.3.3 USB 主机端点类型寄存器（R32_UH_EP_TYPE）  

<table><tr><td>位</td><td>名称</td><td>访问</td><td>描述</td><td>复位值</td></tr><tr><td>[31:19]</td><td>Reserved</td><td>R0</td><td>保留</td><td>0</td></tr><tr><td>18</td><td>RB_UH_EP_RX_TYPE</td><td>RW</td><td>主机接收端点类型,1表示同步传输</td><td>0</td></tr><tr><td>[17:4]</td><td>Reserved</td><td>R0</td><td>保留。</td><td>0</td></tr><tr><td>3</td><td>RB_UH_EP_TX_TYPE</td><td>RW</td><td>主机发送端点类型,1表示同步传输</td><td>0</td></tr><tr><td>[2:0]</td><td>Reserved</td><td>R0</td><td>保留。</td><td>0</td></tr></table>

USB 主机接收缓冲区起始地址（R32_UH_RX_DMA）  

<table><tr><td>位</td><td>名称</td><td>访问</td><td>描述</td><td>复位值</td></tr><tr><td>[31:17]</td><td>Reserved</td><td>R0</td><td>保留</td><td>0</td></tr><tr><td>[16:0]</td><td>R16_UH_RX_DMA</td><td>RW</td><td>主机端点数据接收缓冲区起始地址，最低2位固定为0（4字节对齐）</td><td>X</td></tr></table>

USB 主机发送缓冲区起始地址（R32_UH_TX_DMA）  

<table><tr><td>位</td><td>名称</td><td>访问</td><td>描述</td><td>复位值</td></tr><tr><td>[31:17]</td><td>Reserved</td><td>R0</td><td>保留</td><td>0</td></tr><tr><td>[16:0]</td><td>R16_UH_TX_DMA</td><td>RW</td><td>主机端点数据发送缓冲区起始地址（不需要4字节对齐）</td><td>X</td></tr></table>

22.2.3.4 USB 主机接收最大长度包寄存器（R16_UH_RX_MAX_LEN）  

<table><tr><td>位</td><td>名称</td><td>访问</td><td>描述</td><td>复位值</td></tr><tr><td>[15:11]</td><td>Reserved</td><td>R0</td><td>保留</td><td>0</td></tr><tr><td>[10:0]</td><td>UH_RX_MAX_LEN</td><td>RW</td><td>主机端点接收数据的最大包长度。</td><td>X</td></tr></table>

注：这个最大包大小决定了端点可接收数据最大长度，超出此长度的数据会被丢弃。

22.2.3.5 USB 主机令牌设置寄存器（R8_UH_EP_PID）  

<table><tr><td>位</td><td>名称</td><td>访问</td><td>描述</td><td>复位值</td></tr><tr><td>[7:4]</td><td>MASK_UH_TOKEN</td><td>RW</td><td>设置本次USB传输事务的令牌PID标识。</td><td>0</td></tr><tr><td>[3:0]</td><td>MASK_UH_ENDP</td><td>RW</td><td>设置本次被操作的目标设备的端点号。</td><td>0</td></tr></table>

22.2.3.6 USB 主机接收端点控制寄存器（R8_UH_RX_CTRL）  

<table><tr><td>位</td><td>名称</td><td>访问</td><td>描述</td><td>复位值</td></tr><tr><td>7</td><td>Reserved</td><td>R0</td><td>保留。</td><td>0</td></tr><tr><td>6</td><td>RB_UH_R_DATA_NO</td><td>RW</td><td>1:不期待数据包,用于主机模式下的操作高速 HUB;0:期望数据包(IN)。</td><td>0</td></tr><tr><td>5</td><td>RB_UH_R_AUTO_TOG</td><td>RW</td><td>同步触发位自动翻转使能控制位:1:对于非同步传输,数据接收成功后自动翻转相应MASK_UH_R_TOG[0];对于同步传输,数据接收成功后MASK_UH_R_TOG会自动减1。</td><td>0</td></tr><tr><td></td><td></td><td></td><td>0:不自动翻转,可以手动切换。</td><td></td></tr><tr><td>[4:3]</td><td>MASK_UH_R_TOG</td><td>RW</td><td>主机接收器(处理IN事务)期望的同步触发位,00:期望DATA0;01:期望DATA1;10:期望DATA2;11:期望MDATA。</td><td>00b</td></tr><tr><td>2</td><td>RB_UH_R_RES_NO</td><td>RW</td><td>1:无应答,用于实现非端点0的实时/同步传输。此时忽略MASK_UEP_R_RES;0:接收数据成功后发送应答。</td><td>0</td></tr><tr><td>[1:0]</td><td>MASK_UH_R_RES</td><td>RW</td><td>主机接收器对IN事务的响应控制位:00:应答ACK;对于实时/同步传输无效。</td><td>00b</td></tr></table>

#### 22.2.3.7 USB 主机发送长度寄存器（R16_UH_TX_LEN）

<table><tr><td>位</td><td>名称</td><td>访问</td><td>描述</td><td>复位值</td></tr><tr><td>[15:11]</td><td>Reserved</td><td>R0</td><td>保留</td><td>0</td></tr><tr><td>[10:0]</td><td>R16_UH_TX_LEN</td><td>RW</td><td>设置USB主机发送端点准备发送的数据字节。</td><td>X</td></tr></table>

#### 22.2.3.8 USB 主机发送端点控制寄存器（R8_UH_TX_CTRL）

<table><tr><td>位</td><td>名称</td><td>访问</td><td>描述</td><td>复位值</td></tr><tr><td>7</td><td>Reserved</td><td>R0</td><td>保留。</td><td>0</td></tr><tr><td>6</td><td>RB_UH_T_DATA_NO</td><td>RW</td><td>1:不发送数据包(PING/SPLIT);0:发送数据包(OUT/SETUP)。</td><td>0</td></tr><tr><td>5</td><td>RB_UH_T_AUTO_TOG</td><td>RW</td><td>同步触发位自动翻转使能控制位,可软件修改:1:对于非同步传输,数据发送成功后自动翻转MASK_UH_T_TOG[0]。0:不自动翻转,可以手动切换。</td><td>0</td></tr><tr><td>[4:3]</td><td>MASK_UH_T_TOG</td><td>RW</td><td>USB主机发送器(处理SETUP/OUT事务)准备的同步触发位00表示发送DATA0;01表示发送DATA1;10表示发送DATA2;11表示发送MDATA。</td><td>00b</td></tr><tr><td>2</td><td>RB_UH_T_RES_NO</td><td>RW</td><td>1:无应答,用于实现非端点0的实时/同步传输。此时忽略MASK_UEP_T_RES;0:发送数据成功后期待应答。</td><td>0</td></tr><tr><td>[1:0]</td><td>MASK_UH_T_RES</td><td>RW</td><td>USB主机发送器对SETUP/OUT事务的响应控制位00:期望应答ACK;10:期望应答NAK或忙;11:期望应答STALL或错误;01:期望应答NYET。</td><td>00b</td></tr><tr><td></td><td></td><td></td><td>对于实时/同步传输无效。</td><td></td></tr></table>

22.2.3.9 USB 主机发送 SPLIT 包的数据（R16_UH_SPLIT_DATA）  

<table><tr><td>位</td><td>名称</td><td>访问</td><td>描述</td><td>复位值</td></tr><tr><td>[15:12]</td><td>Reserved</td><td>R0</td><td>保留</td><td>0</td></tr><tr><td>[11:0]</td><td>UH_SPLIT_DATA</td><td>RW</td><td>主机端点发送SPLIT包的数据内容，低12位有效，高4位固定为0</td><td>0xxxh</td></tr></table>


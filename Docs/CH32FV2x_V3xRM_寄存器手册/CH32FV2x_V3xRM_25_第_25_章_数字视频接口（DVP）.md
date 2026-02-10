# 第 25 章 数字视频接口（DVP）

本章模块描述适用于CH32F2x、CH32V2x和CH32V3x微控制器系列部分产品。

数字视频接口（DVP），它支持使用 DVP 接口时序来获取图像数据流，支持按原始的行、帧格式组织的图像数据，如 YUV、RGB 等，也支持如 JPEG 格式的压缩图像数据，能够接收外部 8 位、10 位、12 位的摄像头模块输出的高速并行数据流。

## 25.1 主要特性

可配置8/10/12位数据宽度模式  
支持YUV、RGB格式数据  
$\bullet$ 支持JPEG压缩格式数据  
$\bullet$ 内置FIFO，支持DMA传输  
$\bullet$ 支持双缓冲接收  
$\bullet$ 支持裁剪功能  
支持连续模式和快照模式

## 25.2 功能描述

### 25.2.1 与传感器相连

![](images/9e40754a782aefe45606a4264e94c5278e3e17f523b56bd3ca8dc8743c7db0dd.jpg)  
图 25-1 DVP 接口连接

PLCK（Pixel clk）：像素时钟，每个时钟对应一个像素数据（非压缩数据）。外部 DVP 接口传感器输出的PCLK时钟最大支持96MHz。  
HSYNC（horizonal synchronization）：行同步信号。  
VSYNC（vertical synchronization）：帧同步信号。  
$\bullet$ DATA：像素数据或压缩数据，位宽支持 8/10/12位。  
XCLK：Sensor 的参考时钟，可以由微控制器提供或外部提供，一般使用晶振。  
I2C接口：用来配置sensor的寄存器，可通过控制的普通 GPIO 口模拟I2C时序通讯，亦可使用硬件 I2C接口操作。

表 25-1 DVP 引脚  

<table><tr><td>名称</td><td>信号类型描述</td></tr><tr><td>PLCK</td><td>像素时钟输入</td></tr><tr><td>DATA[11:0]</td><td>像素数据输入</td></tr><tr><td>VSYNC</td><td>帧同步信号输入</td></tr><tr><td>HSYNC</td><td>行同步信号输入</td></tr></table>

### 25.2.2 工作时序

一般使用的 DVP接口传感器模块输出的数字信号数据流和图像大小存在一定的关系，下面以一个图像数据举例。

如图25-2所示，传感器内部是一个 $3 5 7 6 { * } 1 1 4 5$ 大小完整的图像尺寸，经过内部缩放处理，最终从接口输出的图像数据大小为 $1 9 2 0 { ^ { * } } 1 0 8 0$ ，图像刷新率为 25。

![](images/9878c1714077302b314508f3606073eba11995085fd04c352caf6467c90edd2c.jpg)  
图 25-2 时序描述

PCLK是一个像素传输的时间，所以HSYNC是 PCLK的 3576 倍，在3576个像素中，只有 1920 个像素是有效的，剩下的1656个像素点时间内 Sensor 不传输数据。VSYNC 是帧同步信号，所以 VSYNC时间是 PCLK 的 $3 5 7 6 { * } 1 1 4 5$ 倍，同样，只有在 $1 9 2 0 { ^ { * } } 1 0 8 0$ 个有效像素时间内，Sensor在传输数据。如果传感器传输的是JPEG压缩后的数据，可能无需使用 HSYNC信号。

DVP接口信号和图像数据的关系具体以选用的传感器数据手册说明为主。

### 25.2.3 RGB/YUV/JPEG 压缩数据格式说明

RGB

三原色：红色、绿色、蓝色

YUV

亮度信号 Y，色度信号 U 和 V、Pb 和 Pr、Cb 和 Cr。

JPEG（Joint Photographic Experts Group）

有损失压缩，但损失的部分是人视觉不易察觉的部分，利用人眼对计算机色彩中高频信息部分不敏感的特点。去除视觉上多余信息（空间冗余度），去除数据本身的多余信息（结构冗余度）。

## 25.3 数字视频接口的应用

### 25.3.1 数字图像接口配置说明

1） DVP 的数据接收中，每一帧数据由 BUF0 和 BUF1 交替存储，从 BUF0 开始。对于 RGB 和 YUV 数据流，硬件在每次帧信号从无效电平变为有效电平时会复位选择 BUF0 开始，存满一行数据将会切换的 BUF1，实现交替存储；对于 JPEG 压缩数据，硬件会根据设置的 DMA 接收长度设置 BUF0 和BUF1的切换阈值。  
2） 当数据总线宽度为10位或者12位时，每接收一个数据时，系统将自动对数据进行无符号扩展到16 位再进行存储。  
3） R16_DVP_ROW_NUM 和 R16_DVP_COL_NUM 寄存器必须与传感器实际输出的图像大小匹配。  
4） 在视频流 RGB 模式下，R16_DVP_COL_NUM 表示一行数据的有效 PCLK 周期数，R16_DVP_ROW_NUM 表示一帧图像数据内包含的行数；在图像JPEG模式下，R16_DVP_COL_NUM用于配置 DMA长度，此模式下，R16_DVP_ROW_NUM 寄存器不起作用。

### 25.3.2 数字图像接口应用说明

在使用数字图像接口接收图像数据时，须正确配置 DVP相关的控制寄存器，使其与图像传感器的模式相匹配，具体操作步骤如下：

1） 通过 R8_DVP_CR1 寄存器清除 RB_DVP_ALL_CLR 和 RB_DVP_RCV_CLR 字段。  
2） 通过 R8_DVP_CR0 寄存器配置图像模式、数据位宽、PCLK 极性、HSYNC 极性和 VSYNC 极性，使其与 SENSOR 的输出相匹配。  
3） 根据配置后的图像传感器输出的有效图像像素，配置 R16_DVP_ROW_NUM 和 R16_DVP_COL_NUM 寄存器，使其与SENSOR的输出相匹配，在图像 JPEG 模式下，只需配置 R16_DVP_COL_NUM寄存器。  
4） 通过 R32_DVP_DMA_BUF0/1 寄存器配置 DMA 接收地址。  
5） 若使用快照模式，需通过R8_DVP_CR1 寄存器，配置RB_DVP_CM字段，开启快照模式。  
6） 若使用裁剪模式，需通过 R8_DVP_CR1 寄存器，配置 RB_DVP_CROP 和 RB_DVP_FCRC 字段，开启裁剪功能和控制帧捕获率，同时通过配置 R16_DVP_HOFFCNT、R16_DVP_VST、R16_DVP_CAPCNT 和R16_DVP_VLINE 设置裁剪图像的尺寸。  
7） 根据需求，通过 R8_DVP_IER 寄存器使能相应中断，通过中断控制器 NVIC 或 PFIC 配置中断优先级以及使能DVP中断。  
8） 通过 R8_DVP_CR1 寄存器使能 DMA，通过 R8_DVP_CR0 寄存器使能 DVP 接口。  
9） 等待相关接收中断的产生，及时处理接收数据。

### 25.3.3 捕获模式说明

DVP 接口支持两种捕获模式：快照（单帧）模式和连续模式。

#### 25.3.3.1 快照模式

在该模式下，只捕获单帧（R8_DVP_CR1寄存器中 RB_DVP_CM置1），当使能DVP接口后，等待系统检测帧起始，之后开始进行图像数据采样，在接收完整一帧数据后，将会关闭 DVP 接口（R8_DVP_CR0 寄存器的 RB_DVP_ENABLE 字段清 0）。快照模式下的单帧捕获波形如图 25-3 所示。

![](images/201baecc85b6c77b20ed65b1648e989e2b7792ef53bde07a27ea52afd916c34e.jpg)  
图25-3 快照模式下的单帧捕获波形

#### 25.3.3.2 连续模式

在该模式下（R8_DVP_CR1 寄存器中 RB_DVP_CM 清 0），当使能 DVP 接口后，在 R8_DVP_CR0 寄存器的 RB_DVP_ENABLE字段清0前，会持续采样每帧数据。连续模式下的帧捕获波形如图 25-4 所示。

![](images/d5f72d0c7f6d74efd5b8596513319b5380c474570a3f4f157f7c1ba9058cedfe.jpg)  
图25-4 连续模式下帧捕获波形

### 25.3.4 裁剪功能说明

DVP 可以使用裁剪功能从接收到的图像中截取一个矩形窗口，该矩形窗口起始坐标（矩形左上角 X 坐标 R16_DVP_HOFFCNT，Y 坐标 R16_DVP_VST）和窗口大小（R16_DVP_CAPCNT 表示水平尺寸、R16_DVP_VLINE表示垂直尺寸）可配置。裁剪窗口的坐标和大小如图 25-5所示。裁剪窗口数据捕获波形如图25-6所示。

![](images/e17213d1f64073f6490afeaaa23841c685fc7defd7429ce30e5624ddef1ab984.jpg)  
图 25-5 裁剪窗口的坐标和大小

![](images/0a39095c7b05ba6ba2734fd070008148b5fba96c90c2e262230384a20d7a14d7.jpg)  
图25-6 裁剪窗口数据捕获波形

![](images/726f68d1520e9a1da270738db1e66fb8a9ed0011e57faeb99893d6c0a20f8b86.jpg)  
在此阶段未捕获的数据

![](images/2686616cf1905ebeea7a18f85f5acc83cbabbbe901c44a68ff0553610939cec3.jpg)  
在此阶段捕获的数据

## 25.4 寄存器描述

表 25-2 DVP 相关寄存器列表  

<table><tr><td>名称</td><td>访问地址</td><td>描述</td><td>复位值</td></tr><tr><td>R8_DVP_CRO</td><td>0x50050000</td><td>DVP 控制寄存器 0</td><td>0x00</td></tr><tr><td>R8_DVP_CR1</td><td>0x50050001</td><td>DVP 控制寄存器 1</td><td>0x06</td></tr><tr><td>R8_DVP_IER</td><td>0x50050002</td><td>DVP 中断使能寄存器</td><td>0x00</td></tr><tr><td>R16_DVP_ROW_NUM</td><td>0x50050004</td><td>图像行数配置寄存器</td><td>0x0000</td></tr><tr><td>R16_DVP_COL_NUM</td><td>0x50050006</td><td>图像列数配置寄存器</td><td>0x0000</td></tr><tr><td>R32_DVP_DMA BUF0</td><td>0x50050008</td><td>DVP DMA 地址 0 寄存器</td><td>0x00000000</td></tr><tr><td>R32_DVP_DMA BUF1</td><td>0x5005000C</td><td>DVP DMA 地址 1 寄存器</td><td>0x00000000</td></tr><tr><td>R8_DVP_IFR</td><td>0x50050010</td><td>DVP 中断标志寄存器</td><td>0x00</td></tr><tr><td>R8_DVP_STATUS</td><td>0x50050011</td><td>DVP 接收 FIFO 状态寄存器</td><td>0x00</td></tr><tr><td>R16_DVP_ROW_CNT</td><td>0x50050014</td><td>DVP 行计数器寄存器</td><td>0x0000</td></tr><tr><td>R16_DVP_HOFFCNT</td><td>0x50050018</td><td>窗口开始的水平方向位移寄存器</td><td>0x0000</td></tr><tr><td>R16_DVP_VST</td><td>0x5005001A</td><td>窗口开始的行数寄存器</td><td>0x0000</td></tr><tr><td>R16_DVP_CAPCNT</td><td>0x5005001C</td><td>捕获计数寄存器</td><td>0x0000</td></tr><tr><td>R16_DVP_VLINE</td><td>0x5005001E</td><td>垂直行计数寄存器</td><td>0x0000</td></tr><tr><td>R32_DVP_DR</td><td>0x50050020</td><td>数据寄存器</td><td>0x00000000</td></tr></table>

### 25.4.1 DVP 配置寄存器（R8_DVP_CR0）

偏移地址： $0 \times 0 0$   

<table><tr><td>位</td><td>名称</td><td>访问</td><td>描述</td><td>复位值</td></tr><tr><td>6</td><td>RB_DVP_JPEG</td><td>RW</td><td>JPEG模式使能。1:JPEG压缩格式;0:原始数据格式。</td><td>0</td></tr><tr><td>[5:4]</td><td>RB_DVP_MSB_DAT_MOD</td><td>RW</td><td>DVP数据位宽配置。00:8位模式;01:10位模式;1x:12位模式。</td><td>00b</td></tr><tr><td>3</td><td>RB_DVP_P_POLAR</td><td>RW</td><td>PCLK极性配置。</td><td>0</td></tr><tr><td></td><td></td><td></td><td>1: 在 PCLK 下降沿采样数据;0: 在 PCLK 上升沿采样数据。</td><td></td></tr><tr><td>2</td><td>RB_DVP_H_POLAR</td><td>RW</td><td>HSYNC 极性配置。1: HSYNC 低电平数据有效;0: HSYNC 高电平数据有效。</td><td>0</td></tr><tr><td>1</td><td>RB_DVP_V_POLAR</td><td>RW</td><td>VSYNC 极性配置。1: VSYNC 高电平数据有效;0: VSYNC 低电平数据有效。</td><td>0</td></tr><tr><td>0</td><td>RB_DVP_ENABLE</td><td>RW</td><td>DVP 功能使能。1: 使能 DVP;0: 禁用 DVP。</td><td>0</td></tr></table>

### 25.4.2 DVP 配置寄存器（R8_DVP_CR1）

偏移地址： $0 \times 0 1$   

<table><tr><td>位</td><td>名称</td><td>访问</td><td>描述</td><td>复位值</td></tr><tr><td>[7:6]</td><td>RB_DVP_FCRC</td><td>RW</td><td>DVP帧捕获率控制。00:捕获所有帧;01:每隔一帧捕获一次;10:每隔三帧捕获一次。11:保留</td><td>00b</td></tr><tr><td>5</td><td>RB_DVP_CROP</td><td>RW</td><td>裁剪功能控制。0:捕获完整图像;1:仅捕获剪裁寄存器所指定的窗口中的数据。</td><td>0</td></tr><tr><td>4</td><td>RB_DVP_CM</td><td>RW</td><td>捕获模式。0:连续模式;1:快照模式。</td><td>0</td></tr><tr><td>3</td><td>RB_DVPBUF_TOG</td><td>RWT</td><td>缓冲地址标志位。硬件控制翻转,软件置1翻转该位,写0无效。1:数据存储在接收地址1;0:数据存储在接收地址0。</td><td>0</td></tr><tr><td>2</td><td>RB_DVP_RCV_CLR</td><td>RW</td><td>接收逻辑复位控制。1:复位接收逻辑电路;0:取消复位操作。</td><td>1</td></tr><tr><td>1</td><td>RB_DVP_ALL_CLR</td><td>RW</td><td>标志与FIFO清除控制,由软件写1或写0:1:复位标志与FIFO;0:取消复位操作。</td><td>1</td></tr><tr><td>0</td><td>RB_DVP_DMA_ENABLE</td><td>RW</td><td>DMA使能控制位:1:使能DMA;0:禁用DMA。</td><td>0</td></tr></table>

### 25.4.3 DVP 中断使能寄存器（R8_DVP_IER）

偏移地址： ${ 0 } { \times 0 } { 2 }$   

<table><tr><td>位</td><td>名称</td><td>访问</td><td>描述</td><td>复位值</td></tr><tr><td>[7:5]</td><td>Reserved</td><td>R0</td><td>保留。</td><td>0</td></tr><tr><td>4</td><td>RB_DVP_IE_STP_FRM</td><td>RW</td><td>帧结束中断使能。(在 VSYNC 从有效电平变为无效电平时产生中断。)1:使能帧结束中断;0:禁止帧结束中断。</td><td>0</td></tr><tr><td>3</td><td>RB_DVP_IE_FIFO_0V</td><td>RW</td><td>接收 FIFO 溢出中断使能。1:使能 FIFO 溢出中断;0:禁止 FIFO 溢出中断。</td><td>0</td></tr><tr><td>2</td><td>RB_DVP_IE_FRM_DONE</td><td>RW</td><td>帧接收完成中断使能。(计数器达到RAW/COL_NUM 配置值时产生中断,表示最后一个数据已写入 RAM)1:使能帧接收完成中断;0:禁止帧接收完成中断。</td><td>0</td></tr><tr><td>1</td><td>RB_DVP_IE_ROW_DONE</td><td>RW</td><td>行结束中断使能。(计数器达到 COL_NUM 配置值时产生中断)1:使能行结束中断;0:禁止行结束中断。</td><td>0</td></tr><tr><td>0</td><td>RB_DVP_IE_str_FRM</td><td>RW</td><td>新一帧开始中断使能。(在 VSYNC 从无效电平变为有效电平时产生中断,表示新的一帧开始,数据即将到来。)1:使能新一帧开始中断;0:禁止新一帧开始中断。</td><td>0</td></tr></table>

### 25.4.4 DVP 图像有效行数配置寄存器（R16_DVP_ROW_NUM）

偏移地址： $0 \times 0 4$   

<table><tr><td>位</td><td>名称</td><td>访问</td><td>描述</td><td>复位值</td></tr><tr><td>[15:0]</td><td>RB_DVP_ROW_NUM</td><td>RW</td><td>在 RGB 模式下，表示一帧图像数据内包含的行数。在 JPEG 模式下，该寄存器无实际意义。</td><td>0</td></tr></table>

### 25.4.5 DVP 图像有效列配置寄存器（16_DVP_COL_NUM）

偏移地址： $0 \times 0 6$   

<table><tr><td>位</td><td>名称</td><td>访问</td><td>描述</td><td>复位值</td></tr><tr><td>[15:0]</td><td>RB_DVP_COL_NUM</td><td>RW</td><td>在RGB模式下,表示一行数据内包含的PCLK周期数。在JPEG模式下,用于配置DMA接收长度。</td><td>0</td></tr></table>

### 25.4.6 DVP DMA 接收地址 0 寄存器（R32_DVP_DMA_BUF0）

偏移地址： $0 \times 0 8$   

<table><tr><td>位</td><td>名称</td><td>访问</td><td>描述</td><td>复位值</td></tr><tr><td>[31:17]</td><td>Reserved</td><td>R0</td><td>保留。</td><td>0</td></tr><tr><td>[16:0]</td><td>RB_DVP_DMA BUF0</td><td>RW</td><td>DMA 接收地址 0。</td><td>0</td></tr></table>

### 25.4.7 DVP DMA 接收地址 1 寄存器（R32_DVP_DMA_BUF1）

偏移地址： $0 \times 0 0$   

<table><tr><td>位</td><td>名称</td><td>访问</td><td>描述</td><td>复位值</td></tr><tr><td>[31:17]</td><td>Reserved</td><td>R0</td><td>保留。</td><td>0</td></tr><tr><td>[16:0]</td><td>RB_DVP_DMA BUF1</td><td>RW</td><td>DMA 接收地址 1。</td><td>0</td></tr></table>

### 25.4.8 DVP 中断标志寄存器（R8_DVP_IFR）

偏移地址： $0 \times 1 0$   

<table><tr><td>位</td><td>名称</td><td>访问</td><td>描述</td><td>复位值</td></tr><tr><td>[7:5]</td><td>Reserved</td><td>RO</td><td>保留。</td><td>0</td></tr><tr><td>4</td><td>RB_DVP_IF_STP_FRM</td><td>RW</td><td>帧结束中断标志,高有效,写0清除。</td><td>0</td></tr><tr><td>3</td><td>RB_DVP_IF_FIFO_OV</td><td>RW</td><td>接收FIFO溢出中断标志,高有效,写0清除。</td><td>0</td></tr><tr><td>2</td><td>RB_DVP_IF_FRM_DONE</td><td>RW</td><td>帧接收完成中断标志,高有效,写0清除。</td><td>0</td></tr><tr><td>1</td><td>RB_DVP_IF_ROW_DONE</td><td>RW</td><td>行结束中断标志,高有效,写0清除。</td><td>0</td></tr><tr><td>0</td><td>RB_DVP_IFSTR_FRM</td><td>RW</td><td>帧开始中断标志,高有效,写0清除。</td><td>0</td></tr></table>

### 25.4.9 DVP 接收 FIFO 状态寄存器（R8_DVP_STATUS）

偏移地址： $0 \times 1 1$   

<table><tr><td>位</td><td>名称</td><td>访问</td><td>描述</td><td>复位值</td></tr><tr><td>7</td><td>Reserved</td><td>R0</td><td>保留。</td><td>0</td></tr><tr><td>[6:4]</td><td>RB_DVP_FIFO_CNT</td><td>R0</td><td>FIFO计数器。</td><td>0</td></tr><tr><td>3</td><td>Reserved</td><td>R0</td><td>保留。</td><td>0</td></tr><tr><td>2</td><td>RB_DVP_FIFO_0V</td><td>R0</td><td>FIFO溢出状态。1: FIFO溢出; 0: FIFO未溢出。</td><td>0</td></tr><tr><td>1</td><td>RB_DVP_FIFO_FULL</td><td>R0</td><td>FIFO满状态。1: 缓存已满; 0: FIFO未满。</td><td>0</td></tr><tr><td>0</td><td>RB_DVP_FIFO_RDY</td><td>R0</td><td>FIFO就绪状态。1: FIFO中有数据; 0: FIFO中无数据。</td><td>0</td></tr></table>

### 25.4.10 DVP 接收图像行数寄存器（R16_DVP_ROW_CNT）

偏移地址： $0 \times 1 4$   

<table><tr><td>位</td><td>名称</td><td>访问</td><td>描述</td><td>复位值</td></tr><tr><td>[15:0]</td><td>RB_DVP_ROW_CNT</td><td>RO</td><td>实际接收中，一帧图像数据包含的行数，此寄存器在帧结束时更新。在JPEG格式下，该寄存器的值没有意义。</td><td>0</td></tr></table>

### 25.4.11 DVP 窗口开始的水平方向位移寄存器（R16_DVP_HOFFCNT）

偏移地址： $0 \times 1 8$   

<table><tr><td>位</td><td>名称</td><td>访问</td><td>描述</td><td>复位值</td></tr><tr><td>[15:0]</td><td>RB_DVP_HOFFCNT</td><td>RW</td><td>窗口行内,每行在捕获数据前需要空出PCLK周期数。低14位有效。</td><td>0</td></tr></table>

### 25.4.12 DVP 窗口开始的行数寄存器（R16_DVP_VST）

偏移地址： ${ 0 } { \times 1 } { \mathsf A }$   

<table><tr><td>位</td><td>名称</td><td>访问</td><td>描述</td><td>复位值</td></tr><tr><td>[15:0]</td><td>RB_DVP_VST</td><td>RW</td><td>图像开始捕获的行数，此行之前的数据不捕获。低13位有效。</td><td>0</td></tr></table>

### 25.4.13 DVP 捕获计数寄存器（R16_DVP_CAPCNT）

偏移地址： ${ 0 } { \times 1 } { 0 }$   

<table><tr><td>位</td><td>名称</td><td>访问</td><td>描述</td><td>复位值</td></tr><tr><td>[15:0]</td><td>RB_DVP_CAPCNT</td><td>RW</td><td>裁剪窗口内需要捕获的PCLK周期数。低14位有效。</td><td>0</td></tr></table>

### 25.4.14 DVP 垂直行计数寄存器（R16_DVP_VLINE）

偏移地址：0x1E  

<table><tr><td>位</td><td>名称</td><td>访问</td><td>描述</td><td>复位值</td></tr><tr><td>[15:0]</td><td>RB_DVP_VLINE</td><td>RW</td><td>裁剪窗口内需要捕获的行数。低14位有效。</td><td>0</td></tr></table>

### 25.4.15 DVP 数据寄存器（R32_DVP_DR）

偏移地址： $_ { 0 \times 2 0 }$   

<table><tr><td>位</td><td>名称</td><td>访问</td><td>描述</td><td>复位值</td></tr><tr><td>[31:0]</td><td>RB_DVP_DR</td><td>RO</td><td>DVP接口每次接收4字节数据,才会触发一次DMA请求,4字节深度的FIFO可为DMA传输留有充足的时间。可有效防止出现DMA溢出情况。</td><td>0</td></tr></table>


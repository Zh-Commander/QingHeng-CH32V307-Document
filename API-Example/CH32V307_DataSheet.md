# 概述

CH32V 系列是基于青稞 RISC-V 内核设计的工业级通用微控制器，包括 CH32V305 连接型 MCU、CH32V307/CH32V317 互联型 MCU、CH32V208 无线型 MCU 等。 $\mathtt { C H 3 2 V 3 0 x }$ 和 CH32V31x 系列基于青稞 V4F微处理器设计，支持单精度浮点指令和快速中断响应，支持144MHz主频零等待运行，提供8组串口、4组电机PWM高级定时器、SDIO、DVP数字图像接口、4组模拟运放、双ADC单元、双DAC单元，内置USB2.0高速PHY收发器（480Mbps）、千兆以太网MAC控制器及10兆物理层收发器、10/100兆物理层收发器（仅适用于CH32V317）等。

# 产品特性

# 内核 Core：

- 青稞32位RISC-V4F内核，多种指令集组合  
- 快速可编程中断控制器 $+ \cdot$ 硬件中断堆栈  
- 分支预测、冲突处理机制  
- 单周期乘法、硬件除法、硬件浮点  
- 系统主频 144MHz，零等待

# $\bullet$ 存储器：

- 可配最大 128KB 易失数据存储区 SRAM  
- 可配 480KB 程序存储区 CodeFlash（零等待应用区 $^ +$ 非零等待数据区）  
- 28KB 系统引导程序存储区 BootLoader   
- 128B系统非易失配置信息存储区  
- 128B 用户自定义信息存储区

# 电源管理和低功耗：

- 系统供电 $\mathsf { V } _ { \mathsf { D } \mathsf { D } }$ 额定：3.3V  
- GPIO 单元独立供电 $\mathsf { V } _ { 1 0 }$ 额定：3.3V  
- 低功耗模式：睡眠、停止、待机  
- VBAT电源独立为 RTC 和后备寄存器供电

# 系统时钟、复位：

- 内置出厂调校的8MHz的RC振荡器  
- 内置约 $4 0 k \mathsf { H } z$ 的RC振荡器  
- 内置 PLL，可选 CPU 时钟达 144MHz  
- 外部支持 $3 \sim 2 5 \mathsf { M H z }$ 高速振荡器  
- 外部支持 32.768kHz 低速振荡器  
- 上电和掉电复位、可编程电压监测器

# $\bullet$ 实时时钟RTC：32位独立定时器

# 2组18路通用DMA控制器：

- 18个通道，支持环形缓冲区管理  
- 支持 TIMx/ADC/DAC/USART/I2C/SPI/I2S/SDIO

#  4组运放、比较器：连接ADC和TIMx

# 2 组 12 位数模转换 DAC

# 2 组 12 位模数转换 ADC：

- 模拟输入范围： $V _ { S S A } { \sim } V _ { D D A }$   
- 16路外部信号 $^ { + 2 }$ 路内部信号通道  
- 片上温度传感器  
- 双 ADC 转换模式

# $\bullet$ 16 路 TouchKey 通道检测

# $\bullet$ 多组定时器：

- 4 个 16 位高级定时器，支持死区控制和紧急刹车，提供用于电机控制的 PWM 互补输出  
- 4个16位通用定时器，提供输入捕获/输出比较/PWM/脉冲计数及增量编码器输入  
- 2个基本定时器  
- 2个看门狗定时器（独立和窗口型）  
- 系统时基定时器：64位计数器

# 多种通讯接口：

- 8 个 USART 接口（包含 5 个 UART）  
- 2 个 I2C 接口（支持 SMBus/PMBus）  
- 3 个 SPI 接口（SPI2、SPI3 用于 I2S2、I2S3）  
- USB2.0 全速主机/设备接口，内置 PHY  
- USB2.0 全速 OTG 接口  
- USB2.0 高速主机/设备接口，内置 PHY  
- 2 组 CAN 接口（2.0B 主动）  
- SDIO 主机接口（MMC、SD/SDIO 卡及 CE-ATA）  
- FSMC 存储器接口  
- 数字图像接口DVP   
- 千兆以太网 MAC 控制器，10 兆 PHY 收发器  
- 10/100 兆 PHY 收发器（仅 CH32V317）

# 快速GPIO端口：

- 80个I/O口，映射16个外部中断

# $\bullet$ 安全特性：CRC 计算单元，96 位芯片唯一 ID

# $\bullet$ 调试模式：串行2线调试接口

# 封装形式：LQFP、QFN 和 TSSOP

# 第 1 章 系列产品说明

CH32V系列产品是基于32位RISC-V指令集架构设计的工业级通用增强型MCU，按照功能资源划分为通用、连接、无线等类别。它们之间以封装类别、外设资源及数量、引脚数目、器件特性高低上的差异相互延伸，但在软件和功能、硬件引脚配置上保持相互兼容，为用户在产品开发中进行产品迭代及快速应用提供了自由和方便。

有关此系列产品的器件特性请参考数据手册。

有关产品各外设功能描述、使用方法及寄存器配置等详细信息请参考《CH32FV2x_V3xRM》。

数据手册和参考手册均可在沁恒官网下载：https://wch.cn

有关 RISC-V 指令集及架构的相关信息，可在“http://riscv.org”网站下载。

本手册为 CH32V303、CH32V305、CH32V307、CH32V317 系列产品数据手册。V203 系列请参考《CH32V203DS0》、V208 系列请参考《CH32V208DS0》。

表 1-1-1 CH32V303/305/307/317 系列产品概览  

<table><tr><td colspan="2">大容量通用型(V303)</td><td colspan="2">连接型(V305)</td><td>互联型(V307)</td><td>互联型(V317)</td></tr><tr><td colspan="6">青棵V4F</td></tr><tr><td>128K闪存</td><td>256K闪存</td><td>128K闪存</td><td>256K闪存</td><td>256K闪存</td><td>256K闪存</td></tr><tr><td>32K SRAM</td><td>64K SRAM</td><td>32K SRAM</td><td>64K SRAM</td><td>64K SRAM</td><td>64K SRAM</td></tr><tr><td>2*ADC(TKey)2*DACADTM3*GPTM2*BCTMCRC3*USART2*SPI2*I2CUSBFSCANRTC4*OPARGNGSDIOFSMC</td><td>2*ADC(TKey)2*DAC4*ADTM4*GPTM2*BCTMCRC8*USART/UART3*SPI(2*I2S)2*I2CUSBFSCANRTC2*WDG4*OPARGNGSDIOFSMC</td><td>2*ADC(TKey)2*DAC4*ADTM4*GPTM2*BCTMCRC5*USART/UART3*SPI(2*I2S)2*I2COTG_FSUSBHS(+PHY)2*CANRTC2*WDG4*OPARGNGSDIO</td><td>2*ADC(TKey)2*DAC4*ADTM4*GPTM2*BCTMCRC5*USART/UART3*SPI(2*I2S)2*I2COTG_FSUSBHS(+PHY)2*CANRTC2*WDG4*OPARGNGSDIO</td><td>2*ADC(TKey)2*DAC4*ADTM4*GPTM2*BCTMCRC8*USART/UART3*SPI(2*I2S)2*I2COTG_FSUSBHS(+PHY)2*CANRTC2*WDG4*OPARGNGSDIO</td><td>2*ADC(TKey)2*DAC4*ADTM4*GPTM2*BCTM2*BCTMCRC8*USART/UART3*SPI(2*I2S)2*I2COTG_FSUSBHS(+PHY)2*CANRTC2*WDG4*OPARGNGSDIO</td></tr></table>

注：同一类产品的某些外设数量或功能可能受封装限制，选择时请确认产品封装。

表 1-1-2 CH32V203/208 系列产品概览  

<table><tr><td colspan="2">中小容量通用型
(V203)</td><td>无线型
(V208)</td></tr><tr><td colspan="2">青棵 V4B</td><td>青棵 V4C</td></tr><tr><td>32K闪存</td><td>64K闪存</td><td>128K闪存</td></tr><tr><td>10K SRAM</td><td>20K SRAM</td><td>64K SRAM</td></tr><tr><td>2*ADC (TKey)</td><td>2*ADC (TKey)</td><td>ADC (TKey)</td></tr><tr><td>ADTM</td><td>ADTM</td><td>ADTM</td></tr><tr><td>3*GPTM</td><td>3*GPTM</td><td>3*GPTM</td></tr><tr><td>CRC</td><td>CRC</td><td>GPTM (32)</td></tr><tr><td>2*USART</td><td>4*USART</td><td>CRC</td></tr><tr><td>SPI</td><td>2*SPI</td><td>4*USART/UART</td></tr><tr><td>12C</td><td>2*12C</td><td>2*SPI</td></tr><tr><td>USBD</td><td>USBD</td><td>2*12C</td></tr><tr><td>USBFS</td><td>USBFS</td><td>USBD</td></tr><tr><td>CAN</td><td>CAN</td><td>CAN</td></tr><tr><td>RTC</td><td>RTC</td><td>RTC</td></tr><tr><td>2*WDG</td><td>2*WDG</td><td>2*WDG</td></tr><tr><td>2*OPA</td><td>2*OPA</td><td>2*OPA</td></tr><tr><td></td><td></td><td>ETH-10M (+PHY)</td></tr><tr><td></td><td></td><td>BLE5.3</td></tr></table>

注：同一类产品的某些外设数量或功能可能受封装限制，选择时请确认产品封装。

缩写

ADTM：高级定时器

GPTM：通用定时器

GPTM（32）：32位通用定时器

BCTM：基本定时器

OPA：运放、比较器

RNG：随机数发生器

USBD：全速设备控制器

USBFS：全速主机/设备控制器

USBHS：高速主机/设备控制器

表 1-2 MCU 内核对比概览  

<table><tr><td>特点
内核</td><td>指令集</td><td>硬件
堆栈
级数</td><td>中断
嵌套
级数</td><td>快速
中断
通道数</td><td>整数
除法
周期</td><td>向量表
模式</td><td>扩展
指令</td><td>内存
保护</td></tr><tr><td>青稞V4B</td><td>IMAC</td><td>2</td><td>2</td><td>4</td><td>9</td><td>地址或指令</td><td>支持</td><td>无</td></tr><tr><td>青稞V4C</td><td>IMAC</td><td>2</td><td>2</td><td>4</td><td>5</td><td>地址或指令</td><td>支持</td><td>标准</td></tr><tr><td>青稞V4F</td><td>IMAFC</td><td>3</td><td>8</td><td>4</td><td>5</td><td>地址或指令</td><td>支持</td><td>标准</td></tr></table>

注：有关内核的相关信息，可参考青稞QingKeV4微处理器手册《QingKeV4_Processor_Manual》。

# 第 2 章 规格信息

CH32V30x 和 CH32V31x 系列是基于青稞 V4F 微处理器设计的 32 位 RISC-V 内核 MCU，工作频率 144MHz，内置高速存储器，系统结构中多条总线同步工作，提供了丰富的外设功能和增强型I/O端口。本系列产品内置2个12位ADC模块、2个12位DAC模块、多组定时器、多通道触摸按键电容检测（TKey）等功能，还包含了标准和专用通讯接口：I2C、I2S、SPI、USART、SDIO、CAN 控制器、USB2.0 全速主机/设备控制器、USB2.0高速主机/设备控制器（内置480Mbps收发器）、数字图像接口、千兆以太网控制器等。

产品工作额定电压为3.3V，工作温度范围为 $- 4 0 ^ { \circ } \mathsf { C } \sim 8 5 ^ { \circ } \mathsf { C }$ （除 CH32V303RCT7 芯片的工作温度范围为$- 4 0 ^ { \circ } \mathsf { C } \widetilde { \mathbf { \Gamma } } \widetilde { \mathbf { \Gamma } } \widetilde { \mathbf { \Gamma } } \overline { { 1 0 5 ^ { \circ } } } \mathsf { C } .$ ）。支持多种省电工作模式来满足产品低功耗应用要求。系列产品中各型号在资源分配、外设数量、外设功能等方面有所差异，按需选择。

# 2.1 型号对比

表 2-1-1 CH32V303/305/307 产品资源分配  

<table><tr><td colspan="3" rowspan="2">产品型号资源差异</td><td colspan="4">CH32V303</td><td colspan="4">CH32V305 (4)</td><td colspan="3">CH32V307</td></tr><tr><td>CB</td><td>RB</td><td>RC</td><td>VC</td><td>FB</td><td>GB</td><td>CC</td><td>RB</td><td>RC</td><td>WC</td><td>VC</td></tr><tr><td colspan="3">芯片引脚数</td><td>48</td><td>64</td><td>64</td><td>100</td><td>20</td><td>28</td><td>48</td><td>64</td><td>64</td><td>68</td><td>100</td></tr><tr><td colspan="3">Code FLASH(字节)</td><td>480K</td><td>480K</td><td>480K</td><td>480K</td><td>224K</td><td>224K</td><td>480K</td><td>480K</td><td>480K</td><td>480K</td><td>480K</td></tr><tr><td colspan="3">闪存(字节)</td><td>128K</td><td>128K</td><td>256K(1)</td><td>256K(1)</td><td>128K</td><td>128K</td><td>256K(1)</td><td>128K</td><td>256K(1)</td><td>256K(1)</td><td>256K(1)</td></tr><tr><td colspan="3">SRAM(字节)</td><td>32K</td><td>32K</td><td>64K(1)</td><td>64K(1)</td><td>32K</td><td>32K</td><td>64K(1)</td><td>32K</td><td>64K(1)</td><td>64K(1)</td><td>64K(1)</td></tr><tr><td colspan="3">GPIO端口数</td><td>37</td><td>51</td><td>51</td><td>80</td><td>17</td><td>24</td><td>41</td><td>51</td><td>51</td><td>54</td><td>80</td></tr><tr><td colspan="3">GPIO供电</td><td>共用</td><td colspan="3">独立供电V10</td><td>共用</td><td>独立供电V10</td><td>共用</td><td colspan="4">独立供电V10</td></tr><tr><td rowspan="5">定时器</td><td colspan="2">高级(16位) (2)</td><td>1</td><td>1</td><td>4</td><td>4</td><td>4</td><td>4</td><td>4</td><td>4</td><td>4</td><td>4</td><td>4</td></tr><tr><td colspan="2">通用(16位) (2)</td><td>3</td><td>3</td><td>4</td><td>4</td><td>4</td><td>4</td><td>4</td><td>4</td><td>4</td><td>4</td><td>4</td></tr><tr><td colspan="2">基本(16位)</td><td colspan="2">-</td><td>2</td><td>2</td><td>2</td><td>2</td><td>2</td><td>2</td><td>2</td><td>2</td><td>2</td></tr><tr><td colspan="2">看门狗</td><td colspan="11">2 (WWDG + IWDG)</td></tr><tr><td colspan="2">系统时基64位</td><td colspan="11">支持</td></tr><tr><td colspan="3">RTC</td><td colspan="11">支持</td></tr><tr><td colspan="2" rowspan="2">ADC/TKey</td><td>单元数</td><td>2</td><td>2</td><td>2</td><td>2</td><td>2</td><td>2</td><td>2</td><td>2</td><td>2</td><td>2</td><td>2</td></tr><tr><td>通道数</td><td>10</td><td>16</td><td>16</td><td>16</td><td>1</td><td>6</td><td>16</td><td>16</td><td>16</td><td>16</td><td>16</td></tr><tr><td colspan="3">DAC(单元)</td><td>2</td><td>2</td><td>2</td><td>2</td><td>DAC2</td><td>2</td><td>2</td><td>2</td><td>2</td><td>2</td><td>2</td></tr><tr><td colspan="3">运放、比较器</td><td>4</td><td>4</td><td>4</td><td>4</td><td>-</td><td>OPA3</td><td>4</td><td>4</td><td>4</td><td>4</td><td>4</td></tr><tr><td colspan="3">随机数发生器</td><td>-</td><td>-</td><td>1</td><td>1</td><td>1</td><td>1</td><td>1</td><td>1</td><td>1</td><td>1</td><td>1</td></tr><tr><td rowspan="9">通信接口</td><td colspan="2">USART/UART</td><td>3</td><td>3</td><td>8</td><td>8</td><td>USART1USART3</td><td>5</td><td>5</td><td>5</td><td>8</td><td>8</td><td>8</td></tr><tr><td colspan="2">SPI</td><td>2</td><td>2</td><td>3</td><td>3</td><td>SPI2</td><td>3</td><td>3</td><td>3</td><td>3</td><td>3</td><td>3</td></tr><tr><td colspan="2">I2S</td><td>-</td><td>-</td><td>2</td><td>2</td><td>I2S2</td><td>2</td><td>2</td><td>2</td><td>2</td><td>2</td><td>2</td></tr><tr><td colspan="2">I2C</td><td>2</td><td>2</td><td>2</td><td>2</td><td>2</td><td>2</td><td>2</td><td>2</td><td>2</td><td>2</td><td>2</td></tr><tr><td colspan="2">CAN</td><td>1</td><td>1</td><td>1</td><td>1</td><td>CAN2</td><td>CAN2</td><td>2</td><td>2</td><td>2</td><td>2</td><td>2</td></tr><tr><td colspan="2">SDIO</td><td>-</td><td>-</td><td>1</td><td>1</td><td>-</td><td>1</td><td>-</td><td>1</td><td>1</td><td>1</td><td>1</td></tr><tr><td>USB(FS)</td><td>USBHD</td><td>1</td><td>1</td><td>1</td><td>1</td><td>-</td><td>-</td><td>1</td><td>1</td><td>1</td><td>1</td><td>1</td></tr><tr><td colspan="2">USBHS(含PHY)</td><td colspan="4">-</td><td>1</td><td>1</td><td>1</td><td>1</td><td>1</td><td>1</td><td>1</td></tr><tr><td colspan="2">Ethernet</td><td colspan="8">-</td><td colspan="3">1G MAC+10M PHY</td></tr><tr><td colspan="2" rowspan="2">产品型号资源差异</td><td colspan="4">CH32V303</td><td colspan="4">CH32V305 (4)</td><td colspan="4">CH32V307</td></tr><tr><td>CB</td><td>RB</td><td>RC</td><td>VC</td><td>FB</td><td>GB</td><td>CC</td><td>RB</td><td>RC</td><td>WC</td><td colspan="2">VC</td></tr><tr><td rowspan="2"></td><td>DVP</td><td colspan="10">-</td><td colspan="2">1</td></tr><tr><td>FSMC</td><td colspan="3">-</td><td>1</td><td colspan="6">-</td><td colspan="2">1</td></tr><tr><td colspan="2">CPU主频</td><td colspan="12">Max:144MHz</td></tr><tr><td colspan="2">工作温度</td><td colspan="12">-40℃~85℃(3)</td></tr><tr><td colspan="2">封装形式</td><td>LQFP48</td><td colspan="2">LQFP64M</td><td>LQFP100</td><td>TSSOP20</td><td>QFN28</td><td>LQFP48</td><td>LQFP64M</td><td>LQFP64M</td><td>QFN68</td><td colspan="2">LQFP100</td></tr></table>

注：1.256KFLASH+64K SRAM的产品支持用户选择字配置为（192K FLASH+128K SRAM）、（224K FLASH+96KSRAM)、（256K FLASH+64K SRAM)、（288K FLASH+32K SRAM）几种组合中的一种。在此基础上，批号倒数第六位不为0的256KFLASH+64KSRAM产品还增加了一种配置组合：（128KFLASH+192KSRAM）。FLASH闪存表示的是零等待运行区域ROWAIT，256KFLASH+64K SRAM的产品支持（480K-RoWAIT）字节的非零等待区域。  
2.定时器中的PWM、捕捉等涉及引脚信号的功能需要结合实际芯片封装的引脚，有些封装芯片没有引出则此类功能不能使用。  
3.CH32V303RCT7芯片支持的工作温度范围为： $- 4 0 ^ { \circ } C \sim 1 0 5 ^ { \circ } C$ 。  
4.CH32V305芯片不支持UART6～8。

表 2-1-2 CH32V317 产品资源分配  

<table><tr><td colspan="3" rowspan="2">产品型号资源差异</td><td colspan="2">CH32V317</td></tr><tr><td>WC</td><td>VC</td></tr><tr><td colspan="3">芯片引脚数</td><td>68</td><td>100</td></tr><tr><td colspan="3">Code FLASH(字节)</td><td>480K</td><td>480K</td></tr><tr><td colspan="3">闪存(字节)</td><td>256K(1)</td><td>256K(1)</td></tr><tr><td colspan="3">SRAM(字节)</td><td>64K(1)</td><td>64K(1)</td></tr><tr><td colspan="3">GPIO端口数</td><td>48</td><td>70</td></tr><tr><td colspan="3">GPIO供电</td><td colspan="2">独立供电V10</td></tr><tr><td rowspan="5">定时器</td><td colspan="2">高级(16位)(2)</td><td>4</td><td>4</td></tr><tr><td colspan="2">通用(16位)(2)</td><td>4</td><td>4</td></tr><tr><td colspan="2">基本(16位)</td><td>2</td><td>2</td></tr><tr><td colspan="2">看门狗</td><td colspan="2">2 (WWDG + IWDG)</td></tr><tr><td colspan="2">系统时基64位</td><td colspan="2">支持</td></tr><tr><td colspan="3">RTC</td><td colspan="2">支持</td></tr><tr><td colspan="2" rowspan="2">ADC/TKey</td><td>单元数</td><td>2</td><td>2</td></tr><tr><td>通道数</td><td>16</td><td>16</td></tr><tr><td colspan="3">DAC(单元)</td><td>2</td><td>2</td></tr><tr><td colspan="3">运放</td><td>4</td><td>4</td></tr><tr><td colspan="3">随机数发生器</td><td>1</td><td>1</td></tr><tr><td rowspan="7">通信接口</td><td colspan="2">USART/UART</td><td>8</td><td>8</td></tr><tr><td colspan="2">SPI</td><td>3</td><td>3</td></tr><tr><td colspan="2">I2S</td><td>2</td><td>2</td></tr><tr><td colspan="2">I2C</td><td>2</td><td>2</td></tr><tr><td colspan="2">CAN</td><td>2</td><td>2</td></tr><tr><td colspan="2">SDIO</td><td>1</td><td>1</td></tr><tr><td>USB(FS)</td><td>USBHD</td><td>1</td><td>1</td></tr><tr><td colspan="2" rowspan="2">产品型号
资源差异</td><td colspan="3">CH32V317</td></tr><tr><td>WC</td><td colspan="2">VC</td></tr><tr><td rowspan="4"></td><td>USBHS（含PHY）</td><td>1</td><td colspan="2">1</td></tr><tr><td>Ethernet</td><td colspan="3">MAC+10M/100M PHY</td></tr><tr><td>DVP</td><td>-</td><td colspan="2">1</td></tr><tr><td>FSMC</td><td colspan="3">-</td></tr><tr><td colspan="2">CPU主频</td><td colspan="3">Max: 144MHz</td></tr><tr><td colspan="2">工作温度</td><td colspan="3">工业级: -40℃~85℃</td></tr><tr><td colspan="2">封装形式</td><td>QFN68</td><td colspan="2">LQFP100</td></tr></table>

注：1.CH32V317 支持用户选择字配置为（192K FLASH+128K SRAM）、（224K FLASH+96K SRAM）、（256KFLASH+64K SRAM)、（288K FLASH+32K SRAM）、（128K FLASH+192K SRAM）几种组合中的一种。FLASH闪存表示的是零等待运行区域RoWAIT，CH32V317支持（480K-RoWAIT）字节的非零等待区域。  
2.定时器中的PWM、捕捉等涉及引脚信号的功能需要结合实际芯片封装的引脚，有些封装芯片没有引出则此类功能不能使用。

# 2.2 系统架构

微控制器基于 RISC-V 指令集设计，其架构中将内核、仲裁单元、DMA 模块、SRAM 存储等部分通过多组总线实现交互。设计中集成通用DMA控制器以减轻CPU负担、提高访问效率，应用多级时钟管理机制降低了外设的运行功耗，同时兼有数据保护机制，时钟自动切换保护等措施增加了系统稳定性。下图是系列产品内部总体架构框图。

![](images/f895c2c110a2ce37920eb2b29957404cb9c8585b5f78cb6473785d364ea5932f.jpg)  
图 2-1-1 CH32V303/305/307 系统框图

![](images/cffe8be8c482c6f3b57630d175f32c5ec189f036d17aa1b2ea470de440b13fa0.jpg)  
图 2-1-2 CH32V317 系统框图

# 2.3 存储器映射表

![](images/850f5d98e0517b7a3ef10b9ad525f911122f089f950d3a7d01aa242cf50f86ee.jpg)  
图 2-2 存储器地址映射

# 2.4 时钟树

系统中引入4组时钟源：内部高频RC振荡器（HSI）、内部低频RC振荡器（LSI）、外接高频振荡器（HSE）、外接低频振荡器（LSE）。其中，低频时钟源为RTC和独立看门狗提供了时钟基准。高频时钟源直接或者间接通过 PLL 倍频后输出为系统总线时钟（SYSCLK），系统时钟再由各预分频器提供了HB 域、PB1 域、PB2 域外设控制时钟及采样或接口输出时钟，部分模块工作需要由 PLL 时钟直接提供。

![](images/fd44fc685621abff5b314e166fc0e395ea28245e8fb9f81dafcec1b2bfa849ba.jpg)  
图 2-3 CH32V305/307/317 时钟树框图

图 2-4 CH32V303 时钟树框图  
![](images/c5a2c15ff8be3dcb9bcc5b10e5733051434f56d5629f85375b996b271a348feb.jpg)  
注：当使用USB功能时，CPU的频率必须是48MHz或96MHz或144MHz。当系统从停机或待机状态唤醒时，系统会自动切换为HSI做主频。

# 2.5 功能概述

# 2.5.1 RISC-V4F 处理器

RISC-V4F 支持 RISC-V 指令集 IMAFC 子集，增加了单精度浮点运算。处理器内部以模块化管理，包含快速可编程中断控制器（PFIC）、内存保护、分支预测模式、扩展指令支持等单元。对外多组总线与外部单元模块相连，实现外部功能模块和内核的交互。

青稞微处理器以其极简指令集、多种工作模式、模块化定制扩展等特点可以灵活应用不同场景微控制器设计，例如小面积低功耗嵌入式场景、高性能应用操作系统场景等。

支持机器和用户特权模式  
快速可编程中断控制器（PFIC）  
$\bullet$ 多级硬件中断堆栈  
$\bullet$ 串行2线调试接口  
$\bullet$ 标准内存保护设计  
$\bullet$ 静态或动态分支预测、高效跳转、冲突检测机制  
自定义扩展指令

# 2.5.2 片上存储器及自举模式

内置最大128K字节SRAM区，用于存放数据，掉电后数据丢失。具体容量要对应芯片型号。

内置最大480K字节程序闪存存储区（Code FLASH），即用户区，用于用户的应用程序和常量数据存储。其中包括零等待程序运行区域和非零等待区域。区域具体大小对应芯片型号。

内置28K字节系统存储区（System FLASH），即BOOT区，用于系统引导程序存储（厂家固化自举加载程序）。

128字节用于系统非易失配置信息存储区，用于厂商配置字存储，出厂前固化，用户不可修改。

128字节用于用户自定义信息存储区，用于用户选择字存储。

在启动时，通过自举引脚（BOOT0和BOOT1）可以选择三种自举模式中的一种：

从程序闪存存储器自举  
$\bullet$ 从系统存储器自举  
$\bullet$ 从内部SRAM自举

自举加载程序存放于系统存储区，可以通过USART1和USB接口对程序闪存存储区的内容重新编程。

# 2.5.3 供电方案

# （1）CH32V303/305/307

$\mathsf { V } _ { \mathsf { D } 0 } = 2 . 4 \mathrm { \Omega } ^ { ( 1 ) } \sim 3 . 6 \mathsf { V }$ $v _ { \tt D D } = 2$ ：为部分I/O引脚和内部调压器供电，包括内置的USB PHY和以太网PHY。  
 $\mathsf { V } _ { 1 0 } = 2 . 4 \mathrm { \Omega } ^ { ( 1 ) } \sim 3 . 6 \mathsf { V }$ ：为大部分 I/O 引脚供电，决定了引脚输出高压幅值。正常工作时， $\mathsf { V } _ { 1 0 }$ 电压不能高于 $\mathsf { V } _ { \mathsf { D } \mathsf { D } }$ 电压。  
 ${ \mathsf { V } } _ { \tt { D O A } } = 2 . 4 \mathrm { \Omega } ^ { ( 1 ) } \sim 3 . 6 { \mathsf { V } }$ ：为高频 RC 振荡器、ADC、温度传感器、OPA、DAC 及 PLL 的模拟部分供电。 $V _ { \mathsf { D D A } }$ 电压必须和 $\mathsf { V } _ { 1 0 }$ 电压相同（如果 $\mathsf { V } _ { \mathsf { D } \mathsf { D } }$ 掉电， $\mathsf { V } _ { 1 0 }$ 带电，则 $V _ { \mathsf { D D A } }$ 必须带电并且和 $\mathsf { V } _ { 1 0 }$ 一致）。使用 ADC时， $V _ { \mathsf { D D A } }$ 不得小于2.4V。  
 $\mathsf { V } _ { \mathsf { B A T } } = \mathsf { \Omega } 1 . 8 { \sim } 3 . 6 \mathsf { V }$ ：可选的备用电源，当关闭 $\mathsf { V } _ { \mathsf { D } \mathsf { D } }$ 时，（通过内部电源切换器）单独为 RTC、外部低频振荡器和后备寄存器供电。

# （2）CH32V317

） $\mathsf { V } _ { \mathsf { 0 0 } } = 2 . 4 \mathrm { \Omega } ^ { ( 1 ) } \sim 3 . 6 \mathsf { V }$ ：为部分I/O引脚和内部调压器供电，包括内置的USB PHY和以太网10MPHY。  
 $\mathsf { V } _ { 1 0 } = 2 . 4 \mathrm { \Omega } ^ { ( 1 ) } \sim 3 . 6 \mathsf { V }$ ：为大部分 I/O 引脚供电，决定了引脚输出高压幅值。正常工作时， $\mathsf { V } _ { 1 0 }$ 电压不能高于 $\mathsf { V } _ { \mathsf { D } \mathsf { D } }$ 电压。

 ${ \mathsf { V } } _ { \tt { D O A } } = 2 . 4 \mathrm { \Omega } ^ { ( 1 ) } \sim 3 . 6 { \mathsf { V } }$ ：为高频 RC 振荡器、ADC、温度传感器、OPA、DAC 及 PLL 的模拟部分供电。 $V _ { \mathsf { D D A } }$ 电压必须和 $\mathsf { V } _ { 1 0 }$ 电压相同（如果 $\mathsf { V } _ { \mathsf { D } \mathsf { D } }$ 掉电， $\mathsf { V } _ { 1 0 }$ 带电，则 $V _ { \mathsf { D D A } }$ 必须带电并且和 $V _ { 1 0 }$ 一致）。使用 ADC时， $V _ { \mathsf { D D A } }$ 不得小于2.4V。  
 $\mathsf { V } _ { \mathsf { D } \mathsf { D } \mathsf { \Pi } , \mathsf { E } \mathsf { T } \mathsf { H } } = \mathsf { \Omega } 3 . 1 5 \sim 3 . 4 5 \mathsf { V }$ ：为内置的 10/100M 以太网 PHY 供电。建议外接 $1 \mathsf { u F } { \sim } 4 . 7 \mathsf { u F }$ 容量的对地电容，支持10uF但需并联0.1uF。  
$\mathsf { V } _ { \mathsf { D D K } }$ ：内部电源 LDO 退耦端，需外接 1uF 容量的退耦电容。  
 $\mathsf { V } _ { \mathsf { B A T } } = \mathrm { ~ 1 ~ . ~ } 8 { \sim } 3 . 6 \mathsf { V }$ ：可选的备用电源，当关闭 $\mathsf { V } _ { \mathsf { D } \mathsf { D } }$ 时，（通过内部电源切换器）单独为 RTC、外部低频振荡器和后备寄存器供电。

注：1.对于寄存器FEATURE_SIGN的位 $\begin{array} { l } { { V L E V E L } } \end{array} = \begin{array} { l } { { 1 } } \end{array}$ 的芯片， $V _ { D D }$ $V _ { I O }$ $V _ { D D A }$ 支持最低供电电压为2.4V；对于位 ${ \cal W } L E { \cal V } E L = 0$ 的芯片， $V _ { D D }$ $V _ { D D A }$ 支持最低供电电压为1.8V, $V _ { I O }$ 支持最低供电电压1.2V（仅对于批号倒数第五位大于4的产品，无独立供电 $V _ { I O }$ 的产品和CH32V305GBU6除外）或 $1 . 8 V _ { o }$

# 2.5.4 供电监控器

产品内部集成了上电复位（POR）/掉电复位（PDR）电路，该电路始终处于工作状态。当 $\mathsf { V } _ { \mathsf { D } \mathsf { D } }$ 低于设定的阈值（V ）时，置器件于复位状态，而不必使用外部复位电路。

另外系统设有一个可编程的电压监测器（PVD），需要通过软件开启，用于比较 $\mathsf { V } _ { \mathsf { D } \mathsf { D } }$ 供电与设定的阈值 $V _ { P V D }$ 的电压大小。打开PVD相应边沿中断，可在 $\mathsf { V } _ { \mathsf { D } \mathsf { D } }$ 下降到PVD阈值或上升到PVD阈值时，收到中断通知。关于 $\mathsf { V } _ { \mathsf { P O R / P D R } }$ 和 $V _ { \mathsf { P V D } }$ 的值参考第4章。

# 2.5.5 电压调节器

复位后，调节器自动开启，根据应用方式有三个操作模式

$\bullet$ 开启模式：正常的运行操作，提供稳定的内核电源  
$\bullet$ 低功耗模式：当 CPU 进入停止模式后，可选择调节器低功耗运行  
$\bullet$ 关断模式：当CPU进入待机模式后自动切换调节器到此模式，调压器输出为高阻状态，内核电路的供电切断，调压器处于零消耗状态。

该调压器在复位后始终处于开启模式，在待机模式下被关闭处于关断模式，此时是高阻输出。

# 2.5.6 低功耗模式

系统支持三种低功耗模式，可以针对低功耗、短启动时间和多种唤醒事件等条件下选择达到最佳的平衡。

# 睡眠模式

在睡眠模式下，只有CPU时钟停止，但所有外设时钟供电正常，外设处于工作状态。此模式是最浅低功耗模式，但可以达到最快唤醒。

退出条件：任意中断或唤醒事件。

# 停止模式

此模式FLASH进入低功耗模式，PLL、HSI的RC振荡器和HSE晶体振荡器被关闭。在保持SRAM和寄存器内容不丢失的情况下，停止模式可以达到最低的电能消耗。

退出条件：任意外部中断/事件（EXTI信号）、NRST上的外部复位信号、IWDG复位，其中EXTI信号包括16个外部I/O口之一、PVD的输出、RTC闹钟、以太网唤醒信号或USB的唤醒信号。

# 待机模式

此模式下，系统主 LDO 关闭，由低功耗 LDO 给唤醒电路供电，其他数字电路全部断电，且 FLASH处于断电状态。从待机模式唤醒系统会产生复位，同时SBF（PWR_CSR）会置位。唤醒后，查询SBF状态可知唤醒前的低功耗模式，SBF 由 CSBF（PWR_CR）位清除。在待机模式下，32KB 的 SRAM 的内容可以保持（取决于睡前的规划配置），后备寄存器内容保留。

退出条件：任意外部事件（EXTI信号）、NRST上的外部复位信号、IWDG复位、WKUP引脚上的一个上升边沿，其中EXTI信号包括16个外部I/O口之一、RTC闹钟、以太网唤醒信号或USB的唤醒信号。

# 2.5.7 CRC（循环冗余校验）计算单元

CRC（循环冗余校验）计算单元使用一个固定的多项式发生器，从一个32位的数据字产生一个CRC码。在众多的应用中，基于CRC的技术被用于验证数据传输或存储的一致性。在EN/IEC 60335-1标准的范围内，提供了一种检测闪存存储器错误的手段，CRC 计算单元可以用于实时地计算软件的签名，并与在链接和生成该软件时产生的签名对比。

# 2.5.8 快速可编程中断控制器（PFIC）

产品内置快速可编程中断控制器（PFIC），最多支持255个中断向量，以最小的中断延迟提供了灵活的中断管理功能。当前产品管理了8个内核私有中断和88个外设中断管理，其他中断源保留。PFIC的寄存器均可以在用户和机器特权模式下访问。

2个可单独屏蔽中断   
提供一个不可屏蔽中断 NMI  
$\bullet$ 支持硬件中断堆栈（HPE），无需指令开销  
$\bullet$ 提供4路免表中断（VTF）  
$\bullet$ 向量表支持地址或指令模式  
$\bullet$ 中断嵌套深度可配置最高8级  
支持中断尾部链接功能

# 2.5.9 外部中断/事件控制器（EXTI）

外部中断/事件控制器总共包含19个边沿检测器，用于产生中断/事件请求。每个中断线都可以独立地配置其触发事件（上升沿或下降沿或双边沿），并能够单独地被屏蔽；挂起寄存器维持所有中断请求状态。EXTI 可以检测到脉冲宽度小于内部 PB2 的时钟周期。多达 80 个通用 I/O 口都可选择连接到16个外部中断线。

# 2.5.10 通用 DMA 控制器

系统内置了2组通用DMA控制器，总共管理18个通道，灵活处理存储器到存储器、外设到存储器和存储器到外设间的高速数据传输，支持环形缓冲区方式。每个通道都有专门的硬件DMA请求逻辑，支持一个或多个外设对存储器的访问请求，可配置访问优先权、传输长度、传输的源地址和目标地址等。

DMA 用于主要的外设包括：通用/高级/基本定时器 TIMx、ADC、DAC、I2S、USART、I2C、SPI、SDIO。注：DMA1、DMA2和CPU经过仲裁器仲裁之后对系统SRAM进行访问。

# 2.5.11 时钟和启动

系统时钟源 HSI 默认开启，在没有配置时钟或者复位后，内部 8MHz 的 RC 振荡器作为默认的 CPU时钟，随后可以另外选择外部 $3 \sim 2 5 \mathsf { M H z }$ 时钟或PLL时钟。当打开时钟安全模式后，如果HSE用作系统时钟（直接或间接），此时检测到外部时钟失效，系统时钟将自动切换到内部RC振荡器，同时HSE和PLL自动关闭；对于关闭时钟的低功耗模式，唤醒后系统也将自动地切换到内部的RC振荡器。如果使能了时钟中断，软件可以接收到相应的中断。

多个预分频器用于配置HB的频率、高速PB（PB2）和低速PB（PB1）区域提供各外设时钟，最高频率 144MHz，参考图 2-3 的时钟树框图。I2S 单元的时钟来源另一个专用的 PLL（PLL3），这样，I2S主时钟可产生 $8 \mathsf { k H z } \sim 1 9 2 \mathsf { k H z }$ 之间的所有标准的采样频率。

# 2.5.12 RTC（实时时钟）和后备寄存器

RTC 和后备寄存器在系统内部处于后备供电区域，在 $\mathsf { V } _ { \mathsf { D } \mathsf { D } }$ 有效时由 $\mathsf { V } _ { \mathsf { D } \mathsf { D } }$ 供电，在 $\mathsf { V } _ { \mathsf { D } \mathsf { D } }$ 无效时内部自动切换到由 $\mathsf { V } _ { \mathsf { B A T } }$ 引脚供电。

RTC 实时时钟是一组 32 位可编程计数器，时基支持 20 位预分频，用于较长时间段的测量。时钟

基准来源高速的外部时钟128分频（HSE/128）、外部晶体低频振荡器（LSE）或内部低功耗RC振荡器（LSI）。其中LSE也存在后备供电区域，所以，当选择LSE做RTC时基下，系统复位或从待机模式唤醒后，RTC的设置和时间能够保持不变。

后备寄存器最多包含42个16位寄存器，可以用来存储84字节的用户应用数据。此数据在待机唤醒后，或系统复位或电源复位时，都能继续保持。在侵入检测功能开启下，一旦侵入检测信号有效，将被清除后备寄存器中所有内容。

# 2.5.13 ADC（模拟/数字转换器）和触摸按键电容检测（TKey）

产品内置2个12位的模拟/数字转换器（ADC），共用多达16个外部通道和2个内部通道采样，可编程的通道采样时间，可以实现单次、连续、扫描或间断转换，且支持双ADC转换模式。提供模拟看门狗功能允许非常精准地监控一路或多路选中的通道，用于监测通道信号电压。支持外部事件触发转换，触发源包括片上定时器的内部信号和外部引脚。支持使用DMA操作。

ADC 内部通道采样包括一路内置温度传感器采样和一路内部参考电源采样。温度传感器产生一个随温度线性变化的电压。温度传感器在内部被连接到IN16输入通道上，用于将传感器的输出转换到数字数值。

触摸按键电容检测单元，提供了多达 16 个检测通道，复用 ADC 模块的外部通道。检测结果通过ADC模块转换输出结果，通过软件计算识别触摸按键状态。

# 2.5.14 DAC（数字/模拟转换器）

产品内置2个12位电压输出数字/模拟转换器（DAC），转换2路数字信号为2路模拟电压信号并输出，支持双DAC通道独立或同步转换，支持外部事件触发转换，触发源包括片上定时器的内部信号和外部引脚（EXTI线9）。可实现三角波、噪声生成。支持使用DMA操作。

# 2.5.15 定时器及看门狗

系统中的定时器包括高级定时器、通用定时器、基本定时器、看门狗定时器以及系统时基定时器。系列中不同的产品包含的定时器数量有差异，具体参考表2-2。

表 2-2 定时器比较  

<table><tr><td colspan="2">定时器</td><td>分辨率</td><td>计数类型</td><td>时基</td><td>DMA</td><td>功能作用</td></tr><tr><td rowspan="4">高级定时器</td><td>TIM1</td><td rowspan="4">16位</td><td rowspan="4">向上向下向上/下</td><td rowspan="4">PB2时域16位分频器</td><td rowspan="4">支持</td><td rowspan="4">PWM互补输出,单脉冲输出输入捕获输出比较定时计数</td></tr><tr><td>TIM8</td></tr><tr><td>TIM9</td></tr><tr><td>TIM10</td></tr><tr><td rowspan="4">通用定时器</td><td>TIM2</td><td rowspan="4">16位</td><td rowspan="4">向上向下向上/下</td><td rowspan="4">PB1时域16位分频器</td><td rowspan="4">支持</td><td rowspan="4">输入捕获输出比较定时计数</td></tr><tr><td>TIM3</td></tr><tr><td>TIM4</td></tr><tr><td>TIM5</td></tr><tr><td rowspan="2">基本定时器</td><td>TIM6</td><td rowspan="2">16位</td><td rowspan="2">向上</td><td rowspan="2">PB1时域16位分频器</td><td rowspan="2">支持</td><td rowspan="2">定时计数</td></tr><tr><td>TIM7</td></tr><tr><td colspan="2">窗口看门狗</td><td>7位</td><td>向下</td><td>PB1时域4种分频</td><td>不支持</td><td>定时复位系统(正常工作)</td></tr><tr><td colspan="2">独立看门狗</td><td>12位</td><td>向下</td><td>PB1时域7种分频</td><td>不支持</td><td>定时复位系统(正常+低功耗工作)</td></tr><tr><td colspan="2">系统时基定时器</td><td>64位</td><td>向上或下</td><td>SYSCLK或SYSCLK/8</td><td>不支持</td><td>定时</td></tr></table>

# 高级定时器

高级定时器是一个16位的自动装载递加/递减计数器，具有16位可编程的预分频器。除了完整的通用定时器功能外，可以被看成是分配到6个通道的三相PWM发生器，具有带死区插入的互补PWM输出功能，允许在指定数目的计数器周期之后更新定时器进行重复计数周期，刹车功能等。高级定时器的很多功能都与通用定时器相同，内部结构也相同，因此高级定时器可以通过定时器链接功能与其他TIM定时器协同操作，提供同步或事件链接功能。

# 通用定时器

通用定时器是一个 16 位的自动装载递加/递减计数器，具有一个可编程的 16 位预分频器以及 4个独立的通道，每个通道都支持输入捕获、输出比较、PWM 生成和单脉冲模式输出。还能通过定时器链接功能与高级定时器共同工作，提供同步或事件链接功能。在调试模式下，计数器可以被冻结，同时PWM输出被禁止，从而切断由这些输出所控制的开关。任意通用定时器都能用于产生PWM输出。每个定时器都有独立的 DMA 请求机制。这些定时器还能够处理增量编码器的信号，也能处理 1 至 3 个霍尔传感器的数字输出。

# 基本定时器

基本定时器是一个16位自动装载计数器，支持16位可编程预分频器。可以位数模转换（DAC）提供时钟，触发DAC的同步电路。基本定时器之间是互相独立的，互不共享任何资源。

# 独立看门狗

独立看门狗是一个自由运行的12位递减计数器，支持7种分频系数。由一个内部独立的约40kHz的RC振荡器（LSI）提供时钟；因为LSI独立于主时钟，所以可运行于停止和待机模式。IWDG在主程序之外，可以完全独立工作，因此，用于在发生问题时复位整个系统，或作为一个自由定时器为应用程序提供超时管理。通过选项字节可以配置成是软件或硬件启动看门狗。在调试模式下，计数器可以被冻结。

# 窗口看门狗

窗口看门狗是一个7位的递减计数器，并可以设置成自由运行。可以被用于在发生问题时复位整个系统。其由主时钟驱动，具有早期预警中断功能；在调试模式下，计数器可以被冻结。

# 系统时基定时器

青稞微处理器内核自带一个64位可选递增或递减的计数器，用于产生SYSTICK异常（异常号：15），可专用于实时操作系统，为系统提供“心跳”节律，也可当成一个标准的64位计数器。具有自动重加载功能及可编程的时钟源。

# 2.5.16 通用同步/异步串口收发器（USART）

产品提供了3组通用同步/异步串口收发器（USART1、USART2、USART3），以及5组通用异步收发器（UART4、UART5、UART6、UART7、UART8）。支持全双工异步通信、同步单向通信以及半双工单线通信，也支持 LIN（局部互连网），兼容 ISO7816 的智能卡协议和 IrDA SIR ENDEC 传输编解码规范，以及调制解调器（CTS/RTS 硬件流控）操作。还允许多处理器通信。其采用分数波特率发生器系统，并支持DMA操作连续通讯。

# 2.5.17 串行外设接口（SPI）

最高3组串行外设SPI接口，提供主或从操作，动态切换。支持多主模式，全双工或半双工同步传输，支持基本的SD卡和MMC模式。可编程的时钟极性和相位，数据位宽提供8或16位选择，可靠通信的硬件CRC产生/校验，支持DMA操作连续通讯。

# 2.5.18 I2S（音频）接口

最高2组标准的I2S接口（与SPI2和SPI3复用）工作于主或从模式。软件可配置为16/32位数据包传输帧，支持音频采样频率从 8kHz 到 562.2kHz，支持 4 种音频标准。在主模式下，其主时钟可以以固定的256倍音频采样频率输出到外部的DAC或CODEC（解码器），支持DMA。

# 2.5.19 I2C 总线

多达2个I2C总线接口，能够工作于多主机模式或从模式，完成所有I2C总线特定的时序、协议、仲裁等。支持标准和快速两种通讯速度，同时与SMBus2.0兼容。

I2C接口提供7位或10位寻址，并且在7位从模式时支持双从地址寻址。内置了硬件CRC发生器/校验器。可以使用 DMA 操作并支持 SMBus 总线 2.0 版/PMBus 总线。

# 2.5.20 控制器区域网络（CAN）

CAN接口兼容规范2.0A和2.0B（主动），波特率高达1Mbits/s，支持时间触发通信功能。可以接收和发送11位标识符的标准帧，也可以接收和发送29位标识符的扩展帧。具有3个发送邮箱和2个3级深度接收FIFO。

具有2组CAN控制器的产品，共享28个可设置的过滤器和512字节的SRAM存储器资源。

具有1组CAN控制器产品只有14个可设置的过滤器，并和USBD模块共用一个专用的512字节SRAM存储器用于数据的发送和接收，当USBD和CAN同时使用时，为了防止访问SRAM冲突，USBD只能使用低384字节空间。

# 2.5.21 通用串行总线 USB2.0 全速主机/设备控制器（USBFS/OTG_FS）

USB2.0 全速主机控制器和设备控制器（USBFS），遵循 USB2.0 Fullspeed 标准。提供 16 个可配置的 USB 设备端点及一组主机端点。支持控制/批量/同步/中断传输，双缓冲区机制，USB 总线挂起/恢复操作，并提供待机/唤醒功能。USBFS模块专用的48MHz时钟由内部主PLL分频直接产生（PLL必须为 144MHz 或 96MHz 或 48MHz）。

OTG_FS 是双重角色 USB 控制器，支持主机端和设备端的功能，兼容 On-The-Go Supplement to theUSB2.0 规范。同时，该控制器也可配置为仅支持主机端或仅支持设备端功能的控制器，兼容 USB2.0全速规范。控制器使用来自PLL分频得到的48MHz时钟，主要特性包括：

支持在（OTG_FS 控制器的物理层）USB On-The-Go Supplement，Revision1.3 规范中定义为可选项目OTG协议  
通过软件可配置 USB 全速主机、USB 全速/低速设备、USB 双重角色设备  
提供省电功能  
支持控制传输、批量传输、中断传输、实时/同步传输  
提供总线复位、挂起、唤醒和恢复功能

# 2.5.22 通用串行总线 USB2.0 高速主机/设备控制器（USBHS）

USB2.0高速控制器具有主机控制器和设备控制器双重角色，内置480Mbps的USB PHY物理层收发器。当作为主机控制器时，它可支持低速、全速和高速的USB设备。当作为设备控制器时，可以灵活设置为低速、全速或高速模式以适应各种应用。主要特性包括：

支持 USB 2.0、USB 1.1、USB 1.0 协议规范  
$\bullet$ 支持控制传输、批量传输、中断传输、实时/同步传输  
$\bullet$ 提供总线复位、挂起、唤醒和恢复功能  
$\bullet$ 支持高速HUB  
$\bullet$ 设备模式下提供16组上下传输通道，支持配置16个端点号  
$\bullet$ 除设备端点0外，其他端点均支持最大1024字节的数据包，可使用双缓冲功能

# 2.5.23 数字图像接口（DVP）

数字图像接口 DVP（Digital Video Port）用来连接摄像头模块获取图像数据流。提供了 8/10/12bit并行接口方式通讯。支持按原始的行、帧格式组织的图像数据，如YUV、RGB等，也支持如JPEG格式的压缩图像数据流。接收时，主要依靠VSYNC和HSYNC信号同步。支持图像裁剪功能。

# 2.5.24 SDIO 主机控制器

SDIO主机接口提供了多媒体卡（MMC）、SD存储卡、SDIO卡以及CE-ATA设备的操作接口。支持3种不同的数据总线模式：1位（默认）、4位和8位。在8位模式下，该接口可以使数据传输速率达到48MHz。目前该接口全兼容多媒体卡系统规范4.2（向前兼容）、SD I/O卡规范2.0、SD存储卡规范2.0、CE-ATA 数字协议规范 1.1。

# 2.5.25 可配置的静态存储器控制器（FSMC）

FSMC 接口主要提供了同步或异步存储器接口，支持 SRAM、PSRAM、NOR 及 NAND 等器件。内部 HB传输信号被转换成合适的外部通讯协议，允许8/16/32位数据的连续访问。并灵活可配置采样延迟时间以满足不同器件时序。

此外，FSMC 也可用于多数图形 LCD 控制器接口，它支持 Intel 8080 和 Motorola 6800 的模式，很方便地构建简易的图形应用环境，或用于专用加速控制器的高性能方案。

# 2.5.26 以太网控制器（ $M A C + P H Y$ ）

产品提供了符合IEEE 802.3-2002标准的千兆以太网控制器（MAC），充当数据链路层的角色，其Link速率最高支持1Gbps，支持千兆和百兆及速度自适应，提供MII/RMII/RGMII接口连接外置的PHY芯片（例如100Mbps的工业级物理层芯片CH182）。应用时，结合TCP/IP协议栈实现网络产品的开发。

CH32V307 芯片内置 10Mbps 的以太网 PHY 物理层收发器；而 CH32V317 芯片内置 10Mbps/100Mbps以太网PHY物理层收发器。单芯片即可实现以太网通讯。

主要特性包括：

符合 IEEE 802.3 协议规范及设计  
提供 RGMII、RMII、MII 接口，连接外置的以太网 PHY 收发器  
$\bullet$ 支持全双工操作，支持10/100/1000Mbps的数据传输速率  
$\bullet$ 硬件自动完成IPv4和IPv6包完整性校验，IP/ICMP/UDP/TCP包校验和计算机帧长度填充  
$\bullet$ 多种MAC地址过滤模式  
SMI 即可对外置 PHY 进行配置和管理  
 CH32V307芯片应用可选择以太网控制器MAC $^ +$ 内置 10Mbps PHY 或外置 1Gbps PHY  
CH32V317 芯片应用可为以太网控制器 MAC $^ +$ 内置 10Mbps/100Mbps PHY  
CH32V317 芯片支持 Auto-MDIX 交换 RX/TX，自动识别正负信号线。

# 2.5.27 通用输入输出接口（GPIO）

系统提供了5组GPIO端口，共80个GPIO引脚。每个引脚都可以由软件配置成输出（推挽或开漏）、输入（带或不带上拉或下拉）或复用的外设功能端口。多数GPIO引脚都与数字或模拟的复用外设共用。除了具有模拟输入功能的端口，所有的GPIO引脚都有大电流通过能力。提供锁定机制冻结IO配置，以避免意外的写入I/O寄存器。

系统中大部分 IO 引脚电源由 $\mathsf { V } _ { 1 0 }$ 提供，通过改变 $\mathsf { V } _ { 1 0 }$ 供电将改变 IO 引脚输出电平高值来适配外部通讯接口电平。具体引脚请参考引脚描述。

# 2.5.28 随机数发生器（RNG）

产品内置一个随机数发生器，它通过内部的模拟电路提供一个32位的随机数。

# 2.5.29 运放比较器（OPA）

产品内置4组运算放大器，也可用于比较器，内部选择关联到ADC和TIMx外设，其输入和输出均可通过更改配置对多个通道进行选择。支持将外部模拟小信号被放大送入ADC以实现小信号ADC转换，也可以完成信号比较器功能，比较结果由GPIO输出或者直接接入TIMx的输入通道。

# 2.5.30 串行 2 线调试接口（2-wire SDI Serial Debug Interface）

内核自带一个串行2线调试的接口（SDI），包括SWDIO和SWCLK引脚。系统上电或复位后默认调试接口引脚功能开启，主程序运行后可以根据需要关闭SDI。

# 第 3 章 引脚信息

# 3.1 引脚排列

# 3.1.1 互联型 V307

![](images/3a94d2201d39383313872c6f2b645d1b6795f848d76e53261abe9a482da52502.jpg)

![](images/7801b29fa84a520184d48966e067d0d7707ea971efda1731296c79f48a684ac3.jpg)

![](images/fd2020ad967f49fa26425d560f931745f5abf6c09756b8e1d68fe755931c13e9.jpg)

![](images/12d61d01f2034897c15b0fd92510588802e8501f884eae7d480d6f81b50a0fe4.jpg)  
3.1.2 连接型 V305

![](images/56ae40d1960f5526e216f3e16590f7d9fa9b4fecdb3079531f2c09bc6e7ad9d4.jpg)

![](images/873e33a4564c3d9531f0273a552882ec90fb9ccf3a2a51894d91ba83453155e4.jpg)

![](images/f11ad8c9b58138cd88b696530fca3826d8c2bebd3d2cb6c742a068a13065af5e.jpg)  
3.1.3 大容量通用型 V303

![](images/a70934b2b7279a07d1673d9a9249af5cb0e719b37ac7f284f2a0f096e68957cc.jpg)  
CH32V303RxT6/CH32V303RCT7

![](images/70734498f959ae73041d8cd32d9fefe0888d207f6bea8e0f30ae5f784c3dc0a2.jpg)  
CH32V303CBT6

# 3.1.4 互联型 V317

![](images/2e7bd7e5fd21bf941887726dcb97a8096628ab0e05d56d37b8a704460dbef37f.jpg)

![](images/0c49e5a4e860fe95bce3d4e7429fed532040a628a011fa1e6c5568e9eae6c4f4.jpg)  
注：引脚图中复用功能均为缩写。  
示例：USB2DP:USBHS_DP   
USB2DM:USBHS_DM   
USB1DP:OTG_FS_DP  
USB1DM:OTG_FS_DM

# 3.2 引脚定义

注意，下表中的引脚功能描述针对的是所有功能，不涉及具体型号产品。不同型号之间外设资源有差异，查看前请先根据产品型号资源表确认是否有此功能。

表 3-1 CH32V303/305/307 引脚定义  

<table><tr><td colspan="7">引脚编号</td><td rowspan="2">引脚名称</td><td rowspan="2">引脚类型(1)</td><td rowspan="2">主电0/1</td><td rowspan="2">主功能(复位后)</td><td rowspan="2">默认复用功能</td><td rowspan="2">重映射功能(13)</td></tr><tr><td>TSSOP20</td><td>QFN28</td><td>LQFP48(V305CCT6)</td><td>LQFP48(V303CBT6)</td><td>LQFP64M(17)</td><td>QFN68</td><td>LQFP100</td></tr><tr><td rowspan="2">18</td><td>0</td><td rowspan="2">-</td><td rowspan="2">-</td><td rowspan="2">-</td><td rowspan="2">0</td><td rowspan="2">-</td><td rowspan="2">VSS</td><td rowspan="2">P</td><td rowspan="2">-</td><td rowspan="2">VSS</td><td rowspan="2">-</td><td rowspan="2">-</td></tr><tr><td>20</td></tr><tr><td>-</td><td>-</td><td>-</td><td>-</td><td>-</td><td>-</td><td>1</td><td>PE2</td><td>I/0</td><td>FT</td><td>PE2</td><td>FSMC_A23</td><td>TIM10_BKIN_2TIM10_BKIN_3</td></tr><tr><td>-</td><td>-</td><td>-</td><td>-</td><td>-</td><td>-</td><td>2</td><td>PE3</td><td>I/0</td><td>FT</td><td>PE3</td><td>FSMC_A19</td><td>TIM10_CH1N_2TIM10_CH1N_3</td></tr><tr><td>-</td><td>-</td><td>-</td><td>-</td><td>-</td><td>-</td><td>3</td><td>PE4</td><td>I/0</td><td>FT</td><td>PE4</td><td>FSMC_A20</td><td>TIM10_CH2N_2TIM10_CH2N_3</td></tr><tr><td>-</td><td>-</td><td>-</td><td>-</td><td>-</td><td>-</td><td>4</td><td>PE5</td><td>I/0</td><td>FT</td><td>PE5</td><td>FSMC_A21</td><td>TIM10_CH3N_2TIM10_CH3N_3</td></tr><tr><td>-</td><td>-</td><td>-</td><td>-</td><td>-</td><td>-</td><td>5</td><td>PE6</td><td>I/0</td><td>FT</td><td>PE6</td><td>FSMC_A22</td><td></td></tr><tr><td>-</td><td>-</td><td>-</td><td>1</td><td>1</td><td>1</td><td>6</td><td>VBAT</td><td>P</td><td>-</td><td>VBAT</td><td></td><td></td></tr><tr><td>-</td><td>-</td><td>-</td><td>2</td><td>2</td><td>2</td><td>7</td><td>PC13-TAMPER-RTC(2)</td><td>I/0</td><td>-</td><td>PC13(3)</td><td>TAMPER-RTC</td><td>TIM8_CH4_1</td></tr><tr><td>-</td><td>-</td><td>-</td><td>3</td><td>3</td><td>3</td><td>8</td><td>PC14-OSC32_IN(2)</td><td>I/0/A</td><td>-</td><td>PC14(3)</td><td>OSC32_IN</td><td>TIM9_CH4_1</td></tr><tr><td>-</td><td>-</td><td>-</td><td>4</td><td>4</td><td>4</td><td>9</td><td>PC15-OSC32_OUT(2)</td><td>I/0/A</td><td>-</td><td>PC15(3)</td><td>OSC32_OUT</td><td>TIM10_CH4_1</td></tr><tr><td>-</td><td>-</td><td>-</td><td>-</td><td>-</td><td>-</td><td>10</td><td>VSS_5</td><td>P</td><td>-</td><td>VSS_5</td><td></td><td></td></tr><tr><td>-</td><td>-</td><td>-</td><td>-</td><td>-</td><td>-</td><td>11</td><td>VDD_5</td><td>P</td><td>-</td><td>VDD_5</td><td></td><td></td></tr><tr><td>19</td><td>1</td><td>1</td><td>5</td><td>5</td><td>5</td><td>12</td><td>OSC_IN</td><td>I/A</td><td>-</td><td>OSC_IN</td><td></td><td>PD0(4)</td></tr><tr><td>20</td><td>2</td><td>2</td><td>6</td><td>6</td><td>6</td><td>13</td><td>OSC_OUT</td><td>0/A</td><td>-</td><td>OSC_OUT</td><td></td><td>PD1(4)</td></tr><tr><td>1</td><td>3</td><td>3</td><td>7</td><td>7</td><td>7</td><td>14</td><td>NRST</td><td>I</td><td>-</td><td>NRST</td><td></td><td></td></tr><tr><td>-</td><td>-</td><td>4</td><td>-</td><td>8</td><td>8</td><td>15</td><td>PC0</td><td>I/0/A</td><td>-</td><td>PC0</td><td>ADC_IN10TIM9_CH1NUART6_TXETH_RGMII_RXC</td><td></td></tr><tr><td>-</td><td>-</td><td>5</td><td>-</td><td>9</td><td>9</td><td>16</td><td>PC1</td><td>I/0/A</td><td>-</td><td>PC1</td><td>ADC_IN11TIM9_CH2NUART6_RXETH_MII_MDCETH_RMII_MDC</td><td></td></tr><tr><td colspan="7">引脚编号</td><td rowspan="2">引脚名称</td><td rowspan="2">引脚类型(1)</td><td rowspan="2">主申0/1</td><td rowspan="2">主功能(复位后)</td><td rowspan="2">默认复用功能</td><td rowspan="2">重映射功能(13)</td></tr><tr><td>TSSOP20</td><td>QFN28</td><td>LQFP48(V305CCT6)</td><td>LQFP48(V303CBT6)</td><td>LQFP64M(17)</td><td>QFN68</td><td>LQFP100</td></tr><tr><td></td><td></td><td></td><td></td><td></td><td></td><td></td><td></td><td></td><td></td><td></td><td>ETH_RGMII_RXCTL</td><td></td></tr><tr><td>-</td><td>4</td><td>6</td><td>-</td><td>10</td><td>10</td><td>17</td><td>PC2</td><td>I/0/A</td><td>-</td><td>PC2</td><td>ADC_IN12TIM9_CH3NUART7_TXOPA3_CH1NETH_MII_TXD2ETH_RGMII_RXD0</td><td></td></tr><tr><td>-</td><td>5</td><td>7</td><td>-</td><td>11</td><td>11</td><td>18</td><td>PC3</td><td>I/0/A</td><td>-</td><td>PC3</td><td>ADC_IN13TIM10_CH3UART7_RXOPA4_CH1NETH_MII_TX_CLKETH_RGMII_RXD1</td><td></td></tr><tr><td>-</td><td>-</td><td>8</td><td>8</td><td>12</td><td>12</td><td>19</td><td>VSSA</td><td>P</td><td>-</td><td>VSSA</td><td></td><td></td></tr><tr><td>-</td><td>-</td><td>-</td><td>-</td><td>-</td><td>-</td><td>20</td><td>VREF-</td><td>P</td><td>-</td><td>VREF-</td><td></td><td></td></tr><tr><td>-</td><td>-</td><td>-</td><td>-</td><td>-</td><td>-</td><td>21</td><td>VREF+</td><td>P</td><td>-</td><td>VREF+</td><td></td><td></td></tr><tr><td>-</td><td>-</td><td>9</td><td>9</td><td>13</td><td>13</td><td>22</td><td>VDDA</td><td>P</td><td>-</td><td>VDDA</td><td></td><td></td></tr><tr><td>-</td><td>-</td><td>10</td><td>10</td><td>14</td><td>14</td><td>23</td><td>PAO-WKUP</td><td>I/0/A</td><td>-</td><td>PAO</td><td>WKUPUSART2_CTSADC_INOTIM2_CH1(14)TIM2_ETR(14)TIM5_CH1TIM8_ETROPA4_OUTOETH_MII_CRSETH_RGMII_RXD2</td><td>TIM2_CH1_2(14)TIM2_ETR_2(14)TIM8_ETR_1</td></tr><tr><td>2</td><td>7</td><td>11</td><td>11</td><td>15</td><td>15</td><td>24</td><td>PA1(15)</td><td>I/0/A</td><td>-</td><td>PA1</td><td>USART2_RTSADC_IN1TIM5_CH2TIM2_CH2OPA3_OUTOETH_MII_RX_CLKETH_RMII_REF_CLKETH_RGMII_RXD3</td><td>TIM2_CH2_2TIM9_BKIN_1</td></tr><tr><td>-</td><td>-</td><td>12</td><td>12</td><td>16</td><td>16</td><td>25</td><td>PA2</td><td>I/0/A</td><td>-</td><td>PA2</td><td>USART2_TXTIM5_CH3</td><td>TIM2_CH3_1TIM9_CH1_1</td></tr><tr><td colspan="6">引脚编号</td><td>引脚名称</td><td rowspan="2">引脚类型(1)</td><td rowspan="2">主电0/1</td><td rowspan="2">主功能(复位后)</td><td rowspan="2">默认复用功能</td><td rowspan="2">重映射功能(13)</td><td></td></tr><tr><td>TSSOP20</td><td>QFN28</td><td>LQFP48(V305CCT6)</td><td>LQFP48(V303C8BT6)</td><td>LQFP64M(17)</td><td>QFN68</td><td>LQFP100</td><td></td></tr><tr><td></td><td></td><td></td><td></td><td></td><td></td><td></td><td></td><td></td><td></td><td>ADC_IN2TIM2_CH3TIM9_CH1TIM9_ETROPA2_OUTOETH_MII_MDIOETH_RMII_MDIOETH_RGMII GTXC</td><td>TIM9_ETR_1</td><td></td></tr><tr><td>-</td><td>-</td><td>-</td><td>-</td><td>-</td><td>17</td><td>-</td><td>V10_4</td><td>P</td><td>-</td><td>V10_4</td><td></td><td></td></tr><tr><td>-</td><td>-</td><td>13</td><td>13</td><td>17</td><td>19</td><td>26</td><td>PA3</td><td>I/0/A</td><td>-</td><td>PA3</td><td>USART2_RXTIM5_CH4ADC_IN3TIM2_CH4TIM9_CH2OPA1_OUTOETH_MII_COLETH_RGMII_TXEN</td><td>TIM2_CH4_1TIM9_CH2_1</td></tr><tr><td>-</td><td>-</td><td>-</td><td>-</td><td>18</td><td>-</td><td>27</td><td>VSS_4</td><td>P</td><td>-</td><td>VSS_4</td><td></td><td></td></tr><tr><td>-</td><td>-</td><td>-</td><td>-</td><td>19</td><td>-</td><td>28</td><td>VDD_4</td><td>P</td><td>-</td><td>VDD_4</td><td></td><td></td></tr><tr><td>-</td><td>7</td><td>14</td><td>14</td><td>20</td><td>20</td><td>29</td><td>PA4</td><td>I/0/A</td><td>-</td><td>PA4</td><td>SPI1_NSSUSART2_CKADC_IN4DAC1_OUTTIM9_CH3DVP_HSYNC</td><td>SPI3_NSS_1I2S3_WS_1TIM9_CH3_1</td></tr><tr><td>2</td><td>8</td><td>15</td><td>15</td><td>21</td><td>21</td><td>30</td><td>PA5(15)</td><td>I/0/A</td><td>-</td><td>PA5</td><td>SPI1_SCKADC_IN5DAC2_OUTOPA2_CH1NDVP_VSYNC</td><td>TIM10_CH1N_1USART1_CTS_2USART1_CK_3</td></tr><tr><td>-</td><td>9</td><td>16</td><td>16</td><td>22</td><td>22</td><td>31</td><td>PA6</td><td>I/0/A</td><td>-</td><td>PA6</td><td>SPI1_MISO TIM8_BKINADC_IN6TIM3_CH1OPA1_CH1NDVP_PCLK</td><td>TIM1_BKIN_1USART1_TX_3UART7_TX_1TIM10_CH2N_1</td></tr><tr><td>-</td><td>10</td><td>17</td><td>17</td><td>23</td><td>23</td><td>32</td><td>PA7</td><td>I/0/A</td><td>-</td><td>PA7</td><td>SPI1_MOSI</td><td>TIM1_CH1N_1</td></tr><tr><td colspan="6">引脚编号</td><td>引脚名称</td><td rowspan="2">引脚类型(1)</td><td rowspan="2">主申0/1</td><td rowspan="2">主功能(复位后)</td><td rowspan="2">默认复用功能</td><td rowspan="2">重映射功能(13)</td><td></td></tr><tr><td>TSSOP20</td><td>QFN28</td><td>LQFP48(V305CCT6)</td><td>LQFP48(V303C8BT6)</td><td>LQFP64M(17)</td><td>QFN68</td><td>LQFP100</td><td></td></tr><tr><td></td><td></td><td></td><td></td><td></td><td></td><td></td><td></td><td></td><td></td><td>TIM8_CH1NADC_IN7TIM3_CH2OPA2_CH1PETH_MII_RX_DVETH_RMII_CRS_DVETH_RGMII_TXDO</td><td>USART1_RX_3UART7_RX_1TIM10_CH3N_1</td><td></td></tr><tr><td>-</td><td>-</td><td>18</td><td>-</td><td>24</td><td>24</td><td>33</td><td>PC4</td><td>I/0/A</td><td>-</td><td>PC4</td><td>ADC_IN14TIM9_CH4UART8_TXOPA4_CH1PETH_MII_RXDOETH_RMII_RXDOETH_RGMII_TXD1</td><td>USART1_CTS_3</td></tr><tr><td>-</td><td>-</td><td>19</td><td>-</td><td>25</td><td>25</td><td>34</td><td>PC5</td><td>I/0/A</td><td>-</td><td>PC5</td><td>ADC_IN15TIM9_BKINUART8_RXOPA3_CH1PETH_MII_RXD1ETH_RMII_RXD1ETH_RGMII_TXD2</td><td>USART1_RTS_3</td></tr><tr><td>-</td><td>-</td><td>20</td><td>18</td><td>26</td><td>26</td><td>35</td><td>PBO</td><td>I/0/A</td><td>-</td><td>PBO</td><td>ADC_IN8TIM3_CH3TIM8_CH2NOPA1_CH1PETH_MII_RXD2ETH_RGMII_TXD3</td><td>TIM1_CH2N_1TIM3_CH3_2TIM9_CH1N_1UART4_TX_1</td></tr><tr><td>-</td><td>-</td><td>21</td><td>19</td><td>27</td><td>27</td><td>36</td><td>PB1</td><td>I/0/A</td><td>-</td><td>PB1</td><td>ADC_IN9TIM3_CH4TIM8_CH3NOPA4_CHONETH_MII_RXD3ETH_RGMII_125IN</td><td>TIM1_CH3N_1TIM3_CH4_2TIM9_CH2N_1UART4_RX_1</td></tr><tr><td>-</td><td>-</td><td>22</td><td>20</td><td>28</td><td>28</td><td>37</td><td>PB2(5)</td><td>I/0</td><td>FT</td><td>PB2BOOT1(5)</td><td>OPA3_CHON</td><td>TIM9_CH3N_1</td></tr><tr><td>-</td><td>-</td><td>-</td><td>-</td><td>-</td><td>-</td><td>38</td><td>PE7</td><td>I/0/A</td><td>FT</td><td>PE7</td><td>FSMC_D4OPA3_OUT1</td><td>TIM1_ETR_3</td></tr><tr><td>TSSOP20</td><td>QFN28</td><td>LQFP48(V305CCT6)</td><td>LQFP48(V303CBT6)</td><td>LQFP64M(17)</td><td>LQFP100</td><td colspan="7"></td></tr><tr><td>-</td><td>-</td><td>-</td><td>-</td><td>-</td><td>-</td><td>39</td><td>PE8</td><td>I/0/A</td><td>FT</td><td>PE8</td><td>FSMC_D5OPA4_OUT1</td><td>TIM1_CH1N_3UART5_TX_2UART5_TX_3</td></tr><tr><td>-</td><td>-</td><td>-</td><td>-</td><td>-</td><td>-</td><td>40</td><td>PE9</td><td>I/0</td><td>FT</td><td>PE9</td><td>FSMC_D6</td><td>TIM1_CH1_3UART5_RX_2UART5_RX_3</td></tr><tr><td>-</td><td>-</td><td>-</td><td>-</td><td>-</td><td>-</td><td>41</td><td>PE10</td><td>I/0</td><td>FT</td><td>PE10</td><td>FSMC_D7</td><td>TIM1_CH2N_3UART6_TX_2UART6_TX_3</td></tr><tr><td>-</td><td>-</td><td>-</td><td>-</td><td>-</td><td>-</td><td>42</td><td>PE11</td><td>I/0</td><td>FT</td><td>PE11</td><td>FSMC_D8</td><td>TIM1_CH2_3UART6_RX_2UART6_RX_3</td></tr><tr><td>-</td><td>-</td><td>-</td><td>-</td><td>-</td><td>-</td><td>43</td><td>PE12</td><td>I/0</td><td>FT</td><td>PE12</td><td>FSMC_D9</td><td>TIM1_CH3N_3UART7_TX_2UART7_TX_3</td></tr><tr><td>-</td><td>-</td><td>-</td><td>-</td><td>-</td><td>-</td><td>44</td><td>PE13</td><td>I/0</td><td>FT</td><td>PE13</td><td>FSMC_D10</td><td>TIM1_CH3_3UART7_RX_2UART7_RX_3</td></tr><tr><td>-</td><td>-</td><td>-</td><td>-</td><td>-</td><td>-</td><td>45</td><td>PE14</td><td>I/0/A</td><td>FT</td><td>PE14</td><td>FSMC_D11OPA2_OUT1</td><td>TIM1_CH4_3UART8_TX_2UART8_TX_3</td></tr><tr><td>-</td><td>-</td><td>-</td><td>-</td><td>-</td><td>-</td><td>46</td><td>PE15</td><td>I/0/A</td><td>FT</td><td>PE15</td><td>FSMC_D12OPA1_OUT1</td><td>TIM1_BKIN_3UART8_RX_2UART8_RX_3</td></tr><tr><td>3</td><td>11</td><td>23</td><td>21</td><td>29</td><td>29</td><td>47</td><td>PB10</td><td>I/0/A</td><td>FT</td><td>PB10</td><td>I2C2_SCLUSART3_TXOPA2_CHONETH_MII_RX_ER</td><td>TIM2_CH3_2TIM2_CH3_3TIM10_BKIN_1</td></tr><tr><td>4</td><td>12</td><td>24</td><td>22</td><td>30</td><td>30</td><td>48</td><td>PB11</td><td>I/0/A</td><td>FT</td><td>PB11</td><td>I2C2_SDAUSART3_RXOPA1_CHONETH_MII_TX_ENETH_RMII_TX_EN</td><td>TIM2_CH4_2TIM2_CH4_3TIM10_ETR_1</td></tr><tr><td>-</td><td>-</td><td>26</td><td>23</td><td>31</td><td>18</td><td>49</td><td>Vss_1</td><td>P</td><td></td><td>Vss_1</td><td></td><td></td></tr><tr><td>-</td><td>-</td><td>-</td><td>-</td><td>32</td><td>31</td><td>50</td><td>V10_1</td><td>P</td><td></td><td>V10_1</td><td></td><td></td></tr><tr><td>-</td><td>-</td><td>25</td><td>24</td><td>-</td><td>-</td><td>-</td><td>VDD_10_1</td><td>P</td><td></td><td>VDD_10_1</td><td></td><td></td></tr><tr><td>-</td><td>-</td><td>-</td><td>-</td><td>-</td><td>32</td><td>-</td><td>VDD_1</td><td>P</td><td></td><td>VDD_1</td><td></td><td></td></tr><tr><td colspan="7">引脚编号</td><td rowspan="2">引脚名称</td><td rowspan="2">引脚类型(1)</td><td rowspan="2">主申0/1</td><td rowspan="2">主功能(复位后)</td><td rowspan="2">默认复用功能</td><td rowspan="2">重映射功能(13)</td></tr><tr><td>TSSOP20</td><td>QFN28</td><td>LQFP48(V305CCT6)</td><td>LQFP48(V303C8BT6)</td><td>LQFP64M(17)</td><td>QFN68</td><td>LQFP100</td></tr><tr><td>5</td><td>13</td><td>27</td><td>25</td><td>33</td><td>35</td><td>51</td><td>PB12</td><td>I/0/A</td><td>FT</td><td>PB12</td><td>SPI2 NSSI2S2_WSI2G2_SMBAUSART3_CKTIM1_BKINOPA4_CHOPCAN2_RXETH_MII_TXDOETH_RMII_TXDOETH_RGMII_MDC</td><td></td></tr><tr><td>6</td><td>14</td><td>28</td><td>26</td><td>34</td><td>36</td><td>52</td><td>PB13</td><td>I/0/A</td><td>FT</td><td>PB13</td><td>SPI2_SCKI2S2_CKUSART3_CTSTIM1_CH1NOPA3_CHOPCAN2_TXETH_MII_TXD1ETH_RMII_TXD1ETH_RGMII_MDIO</td><td>USART3_CTS_1</td></tr><tr><td>7</td><td>15</td><td>29</td><td>27</td><td>35</td><td>37</td><td>53</td><td>PB14</td><td>I/0/A</td><td>FT</td><td>PB14</td><td>SPI2_MISO TIM1_CH2NUSART3_RTSOPA2_CHOPSDIO_DO(7)</td><td>USART3_RTS_1</td></tr><tr><td>8</td><td>16</td><td>30</td><td>28</td><td>36</td><td>38</td><td>54</td><td>PB15</td><td>I/0/A</td><td>FT</td><td>PB15</td><td>SPI2_MOSI12S2_SD TIM1_CH3NOPA1_CHOPSDIO_D1(7)</td><td>USART1_TX_2</td></tr><tr><td>-</td><td>-</td><td>-</td><td>-</td><td>-</td><td>33</td><td>55</td><td>PD8</td><td>I/0</td><td>FT</td><td>PD8</td><td>FSMC_D13</td><td>USART3_TX_3TIM9_CH1N_2TIM9_CH1N_3ETH_MII_RX_DV_1ETH_RMII_CRS_DV_1</td></tr><tr><td>-</td><td>-</td><td>-</td><td>-</td><td>-</td><td>34</td><td>56</td><td>PD9</td><td>I/0</td><td>FT</td><td>PD9</td><td>FSMC_D14</td><td>USART3_RX_3TIM9_CH1_2</td></tr><tr><td colspan="6">引脚编号</td><td>引脚名称</td><td rowspan="2">引脚类型(1)</td><td rowspan="2">主申0/1</td><td rowspan="2">主功能(复位后)</td><td rowspan="2">默认复用功能</td><td rowspan="2">重映射功能(13)</td><td></td></tr><tr><td>TSSOP20</td><td>QFN28</td><td>LQFP48(V305CCT6)</td><td>LQFP48(V303C8BT6)</td><td>LQFP64M(17)</td><td>QFN68</td><td>LQFP100</td><td></td></tr><tr><td></td><td></td><td></td><td></td><td></td><td></td><td></td><td></td><td></td><td></td><td></td><td>TIM9_ETR_2</td><td></td></tr><tr><td></td><td></td><td></td><td></td><td></td><td></td><td></td><td></td><td></td><td></td><td></td><td>TIM9_CH1_3</td><td></td></tr><tr><td></td><td></td><td></td><td></td><td></td><td></td><td></td><td></td><td></td><td></td><td></td><td>TIM9_ETR_3</td><td></td></tr><tr><td></td><td></td><td></td><td></td><td></td><td></td><td></td><td></td><td></td><td></td><td></td><td>ETH_MII_RXD0_1</td><td></td></tr><tr><td></td><td></td><td></td><td></td><td></td><td></td><td></td><td></td><td></td><td></td><td></td><td>ETH_RMII_RXD0_1</td><td></td></tr><tr><td>-</td><td>-</td><td>-</td><td>-</td><td>-</td><td>-</td><td>57</td><td>PD10</td><td>I/0</td><td>FT</td><td>PD10</td><td>FSMC_D15</td><td>USART3_CK_2</td></tr><tr><td></td><td></td><td></td><td></td><td></td><td></td><td></td><td></td><td></td><td></td><td></td><td></td><td>USART3_CK_3</td></tr><tr><td></td><td></td><td></td><td></td><td></td><td></td><td></td><td></td><td></td><td></td><td></td><td></td><td>TIM9_CH2N_2</td></tr><tr><td></td><td></td><td></td><td></td><td></td><td></td><td></td><td></td><td></td><td></td><td></td><td></td><td>TIM9_CH2N_3</td></tr><tr><td></td><td></td><td></td><td></td><td></td><td></td><td></td><td></td><td></td><td></td><td></td><td></td><td>ETH_MII_RXD1_1</td></tr><tr><td></td><td></td><td></td><td></td><td></td><td></td><td></td><td></td><td></td><td></td><td></td><td></td><td>ETH_RMII_RXD1_1</td></tr><tr><td>-</td><td>-</td><td>-</td><td>-</td><td>-</td><td>-</td><td>58</td><td>PD11</td><td>I/0</td><td>FT</td><td>PD11</td><td>FSMC_A16</td><td>USART3_CTS_2</td></tr><tr><td></td><td></td><td></td><td></td><td></td><td></td><td></td><td></td><td></td><td></td><td></td><td></td><td>USART3_CTS_3</td></tr><tr><td></td><td></td><td></td><td></td><td></td><td></td><td></td><td></td><td></td><td></td><td></td><td></td><td>TIM9_CH2_2</td></tr><tr><td></td><td></td><td></td><td></td><td></td><td></td><td></td><td></td><td></td><td></td><td></td><td></td><td>TIM9_CH2_3</td></tr><tr><td></td><td></td><td></td><td></td><td></td><td></td><td></td><td></td><td></td><td></td><td></td><td></td><td>ETH_MII_RXD2_1</td></tr><tr><td>-</td><td>-</td><td>-</td><td>-</td><td>-</td><td>-</td><td>59</td><td>PD12</td><td>I/0</td><td>FT</td><td>PD12</td><td>FSMC_A17</td><td>TIM4_CH1_1</td></tr><tr><td></td><td></td><td></td><td></td><td></td><td></td><td></td><td></td><td></td><td></td><td></td><td></td><td>TIM9_CH3N_2</td></tr><tr><td></td><td></td><td></td><td></td><td></td><td></td><td></td><td></td><td></td><td></td><td></td><td></td><td>TIM9_CH3N_3</td></tr><tr><td></td><td></td><td></td><td></td><td></td><td></td><td></td><td></td><td></td><td></td><td></td><td></td><td>USART3_RTS_3</td></tr><tr><td></td><td></td><td></td><td></td><td></td><td></td><td></td><td></td><td></td><td></td><td></td><td></td><td>ETH_MII_RXD3</td></tr><tr><td></td><td></td><td></td><td></td><td></td><td></td><td></td><td></td><td></td><td></td><td></td><td></td><td>USART3_RTS_2</td></tr><tr><td>-</td><td>-</td><td>-</td><td>-</td><td>-</td><td>-</td><td>60</td><td>PD13</td><td>I/0</td><td>FT</td><td>PD13</td><td>FSMC_A18</td><td>TIM4_CH2_1</td></tr><tr><td></td><td></td><td></td><td></td><td></td><td></td><td></td><td></td><td></td><td></td><td></td><td></td><td>TIM9_CH3_2</td></tr><tr><td></td><td></td><td></td><td></td><td></td><td></td><td></td><td></td><td></td><td></td><td></td><td></td><td>TIM9_CH3_3</td></tr><tr><td>-</td><td>-</td><td>-</td><td>-</td><td>-</td><td>-</td><td>61</td><td>PD14</td><td>I/0</td><td>FT</td><td>PD14</td><td>FSMC_D0</td><td>TIM4_CH3_1</td></tr><tr><td></td><td></td><td></td><td></td><td></td><td></td><td></td><td></td><td></td><td></td><td></td><td></td><td>TIM9_BKIN_2</td></tr><tr><td></td><td></td><td></td><td></td><td></td><td></td><td></td><td></td><td></td><td></td><td></td><td></td><td>TIM9_BKIN_3</td></tr><tr><td>-</td><td>-</td><td>-</td><td>-</td><td>-</td><td>-</td><td>62</td><td>PD15</td><td>I/0</td><td>FT</td><td>PD15</td><td>FSMC_D1</td><td>TIM4_CH4_1</td></tr><tr><td></td><td></td><td></td><td></td><td></td><td></td><td></td><td></td><td></td><td></td><td></td><td></td><td>TIM9_CH4_2</td></tr><tr><td></td><td></td><td></td><td></td><td></td><td></td><td></td><td></td><td></td><td></td><td></td><td></td><td>TIM9_CH4_3</td></tr><tr><td>9</td><td>-</td><td>31</td><td>-</td><td>37</td><td>39</td><td>63</td><td>PC6</td><td>I/0</td><td>FT</td><td>PC6</td><td>I2S2_MCKTIM8_CH1SD10_D6ETH_RXP</td><td>TIM3_CH1_3</td></tr><tr><td>10</td><td>-</td><td>-</td><td>-</td><td>38</td><td>40</td><td>64</td><td>PC7</td><td>I/0</td><td>FT</td><td>PC7</td><td>I2S3_MCK(11)(12)TIM8_CH2</td><td>TIM3_CH2_3</td></tr><tr><td colspan="7">引脚编号</td><td rowspan="2">引脚名称</td><td rowspan="2">引脚类型(1)</td><td rowspan="2">主申0/1</td><td rowspan="2">主功能(复位后)</td><td rowspan="2">默认复用功能</td><td rowspan="2">重映射功能(13)</td></tr><tr><td>TSSOP20</td><td>QFN28</td><td>LQFP48(V305CCT6)</td><td>LQFP48(V303C8BT6)</td><td>LQFP64M(17)</td><td>QFN68</td><td>LQFP100</td></tr><tr><td></td><td></td><td></td><td></td><td></td><td></td><td></td><td></td><td></td><td></td><td></td><td>SDIO_D7ETH_RXN</td><td></td></tr><tr><td>11</td><td>17</td><td>-</td><td>-</td><td>39</td><td>41</td><td>65</td><td>PC8</td><td>I/0</td><td>FT</td><td>PC8</td><td>TIM8_CH3SDIO_D0(7)ETH_TXPDVP_D2</td><td>TIM3_CH3_3</td></tr><tr><td>12</td><td>18</td><td>-</td><td>-</td><td>40</td><td>42</td><td>66</td><td>PC9(6)</td><td>I/0</td><td>FT</td><td>PC9</td><td>TIM8_CH4SDIO_D1(7)ETH_TXNVDVP_D3</td><td>TIM3_CH4_3</td></tr><tr><td></td><td></td><td>32</td><td>29</td><td>41</td><td>43</td><td>67</td><td>PA8(6)</td><td>I/0</td><td>FT</td><td>PA8</td><td>USART1_CKTIM1_CH1MCO12S3_MCK(11)(12)</td><td>USART1_CK_1USART1_RX_2TIM1_CH1_1</td></tr><tr><td>13</td><td>-</td><td>33</td><td>30</td><td>42</td><td>44</td><td>68</td><td>PA9(16)</td><td>I/0</td><td>FT</td><td>PA9</td><td>USART1_TXTIM1_CH2OTG_FS_VBUSDVP_D012S3_SD(10)(12)</td><td>USART1_RTS_2TIM1_CH2_1</td></tr><tr><td>-</td><td>-</td><td>34</td><td>31</td><td>43</td><td>45</td><td>69</td><td>PA10</td><td>I/0</td><td>FT</td><td>PA10</td><td>USART1_RXTIM1_CH3OTG_FS_IDVDVP_D1</td><td>USART1_CK_2TIM1_CH3_1</td></tr><tr><td>-</td><td>-</td><td>35</td><td>32</td><td>44</td><td>46</td><td>70</td><td>PA11</td><td>I/0/A</td><td>FT</td><td>PA11</td><td>USART1_CTSCAN1_RXTIM1_CH4OTG_FS_DM</td><td>USART1_CTS_1TIM1_CH4_1</td></tr><tr><td>-</td><td>-</td><td>36</td><td>33</td><td>45</td><td>47</td><td>71</td><td>PA12</td><td>I/0/A</td><td>FT</td><td>PA12</td><td>USART1_RTSCAN1_TXTIM1_ETRTIM10_CH1NOTG_FS_DP</td><td>USART1_RTS_1TIM1_ETR_1</td></tr><tr><td>13</td><td>19</td><td>37</td><td>34</td><td>46</td><td>48</td><td>72</td><td>PA13(16)</td><td>I/0</td><td>FT</td><td>SWDIO</td><td>TIM10_CH2N</td><td>PA13TIM8_CH1N_1USART3_TX_2</td></tr><tr><td>-</td><td>-</td><td>-</td><td>-</td><td>-</td><td>-</td><td>73</td><td colspan="6">未使用</td></tr><tr><td>-</td><td>-</td><td>-</td><td>35</td><td>47</td><td>49</td><td>74</td><td>Vss_2</td><td>P</td><td>-</td><td>Vss_2</td><td></td><td></td></tr></table>

<table><tr><td colspan="6">引脚编号</td><td rowspan="2">引脚名称</td><td rowspan="2">引脚类型(1)</td><td rowspan="2">主申0/1</td><td rowspan="2">主功能(复位后)</td><td rowspan="2">默认复用功能</td><td rowspan="2">重映射功能(13)</td></tr><tr><td>TSSOP20</td><td>QFN28</td><td>LQFP48(V305CCT6)</td><td>LQFP48(V303CBT6)</td><td>LQFP64M(17)</td><td>LQFP100</td></tr><tr><td>-</td><td>-</td><td>-</td><td>36</td><td>48</td><td>50</td><td>75</td><td>VDD-2</td><td>P</td><td>-</td><td>VDD-2</td><td></td></tr><tr><td>-</td><td>-</td><td>-</td><td>-</td><td>-</td><td>51</td><td>-</td><td>V10-2</td><td>P</td><td>-</td><td>V10-2</td><td></td></tr><tr><td>15</td><td>22</td><td>38</td><td>37</td><td>49</td><td>52</td><td>76</td><td>PA14</td><td>I/0</td><td>FT</td><td>SWCLK</td><td>TIM8_CH2N_1UART8_TX_1USART3_RX_2</td></tr><tr><td>-</td><td>-</td><td>39</td><td>38</td><td>50</td><td>53</td><td>77</td><td>PA15</td><td>I/0</td><td>FT</td><td>PA15</td><td>TIM2_CH1_1(14)TIM2_ETR_1(14)TIM2_CH1_3(14)TIM2_ETR_3(14)SPI1 NSS(12)I2S3_WS(12)</td></tr><tr><td>-</td><td>23</td><td>-</td><td>-</td><td>51</td><td>54</td><td>78</td><td>PC10</td><td>I/0</td><td>FT</td><td>PC10</td><td>UART4_TXSDIO_D2TIM10_ETDRVP_D8</td></tr><tr><td>-</td><td>24</td><td>-</td><td>-</td><td>52</td><td>55</td><td>79</td><td>PC11</td><td>I/0</td><td>FT</td><td>PC11</td><td>UART4_RXSDIO_D3TIM10_CH4DVP_D4</td></tr><tr><td>-</td><td>25</td><td>-</td><td>-</td><td>53</td><td>56</td><td>80</td><td>PC12</td><td>I/0</td><td>FT</td><td>PC12</td><td>UART5_TXSDIO_CKTIM10_BKINDVP_D9</td></tr><tr><td>-</td><td>-</td><td>-</td><td>-</td><td>-</td><td>-</td><td>81</td><td>PDO</td><td>I/0/A</td><td>FT</td><td>PDO</td><td>FSMC_D2</td></tr><tr><td>-</td><td>-</td><td>-</td><td>-</td><td>-</td><td>-</td><td>82</td><td>PD1</td><td>I/0/A</td><td>FT</td><td>PD1</td><td>FSMC_D3</td></tr><tr><td>-</td><td>26</td><td>-</td><td>-</td><td>54</td><td>57</td><td>83</td><td>PD2</td><td>I/0</td><td>FT</td><td>PD2</td><td>TIM3_ETRUART5_RXSDIO_CMDDVP_D11FSMC_NADV(9)</td></tr><tr><td>-</td><td>-</td><td>-</td><td>-</td><td>-</td><td>-</td><td>84</td><td>PD3</td><td>I/0</td><td>FT</td><td>PD3</td><td>FSMC_CLK</td></tr></table>

<table><tr><td colspan="6">引脚编号</td><td rowspan="2">引脚名称</td><td rowspan="2">引脚类型(1)</td><td rowspan="2">主电0/1</td><td rowspan="2">主功能(复位后)</td><td rowspan="2">默认复用功能</td><td rowspan="2">重映射功能(13)</td><td></td></tr><tr><td>TSSOP20</td><td>QFN28</td><td>LQFP48(V305CCT6)</td><td>LQFP48(V303C8BT6)</td><td>LQFP64M(17)</td><td>LQFP100</td><td></td></tr><tr><td></td><td></td><td></td><td></td><td></td><td></td><td></td><td></td><td></td><td></td><td></td><td>TIM10_CH2_3</td><td></td></tr><tr><td>-</td><td>-</td><td>-</td><td>-</td><td>-</td><td>85</td><td>PD4</td><td>I/0</td><td>FT</td><td>PD4</td><td>FSMC_NOE</td><td>USART2_RTS_1</td><td></td></tr><tr><td>-</td><td>-</td><td>-</td><td>-</td><td>-</td><td>86</td><td>PD5</td><td>I/0</td><td>FT</td><td>PD5</td><td>FSMC_NWE</td><td>USART2_TX_1TIM10_CH3_2TIM10_CH3_3</td><td></td></tr><tr><td>-</td><td>-</td><td>-</td><td>-</td><td>-</td><td>87</td><td>PD6</td><td>I/0</td><td>FT</td><td>PD6</td><td>FSMC_NWAITDVP_D10</td><td>USART2_RX_1</td><td></td></tr><tr><td>-</td><td>-</td><td>-</td><td>-</td><td>-</td><td>88</td><td>PD7</td><td>I/0</td><td>FT</td><td>PD7</td><td>FSMC_NE1FSMC_NCE2</td><td>USART2_CK_1TIM10_CH4_2TIM10_CH4_3</td><td></td></tr><tr><td>-</td><td>-</td><td>40</td><td>39</td><td>55</td><td>58</td><td>89</td><td>PB3</td><td>I/0</td><td>FT</td><td>PB3</td><td>SPI3_SCK12S3_OK(12)DVP_D5(8)</td><td>TIM2_CH2_1TIM2_CH2_3SPI1_SCK_1TIM10_CH1_1</td></tr><tr><td>-</td><td>-</td><td>41</td><td>40</td><td>56</td><td>59</td><td>90</td><td>PB4</td><td>I/0</td><td>FT</td><td>PB4</td><td>SPI3_MISO</td><td>TIM3_CH1_2SPI1_MISO_1UART5_TX_1TIM10_CH2_1</td></tr><tr><td>-</td><td>-</td><td>42</td><td>41</td><td>57</td><td>60</td><td>91</td><td>PB5</td><td>I/0</td><td>FT</td><td>PB5</td><td>I2C1_SMBSPI3_MOSI(12)I2S3_SD(10)(12)ETH_MII_PPS_OUTETH_RMII_PPS_OUT</td><td>TIM3_CH2_2SPI1_MOSI_1CAN2_RX_1TIM10_CH3_1UART5_RX_1</td></tr><tr><td>16</td><td>27</td><td>43</td><td>42</td><td>58</td><td>61</td><td>92</td><td>PB6</td><td>I/0</td><td>FT</td><td>PB6</td><td>I2C1_SCLTIM4_CH1DVP_D5(8)USBHS_DM</td><td>USART1_TX_1CAN2_TX_1TIM8_CH1_1</td></tr><tr><td>17</td><td>28</td><td>44</td><td>43</td><td>59</td><td>62</td><td>93</td><td>PB7</td><td>I/0</td><td>FT</td><td>PB7</td><td>I2C1_SDFSMC_NADVTIM4_CH2USBHS_DP</td><td>USART1_RX_1TIM8_CH2_1</td></tr><tr><td>-</td><td>-</td><td>-</td><td>44</td><td>60</td><td>63</td><td>94</td><td>B00T0(5)</td><td>I</td><td>-</td><td>B00T0(5)</td><td></td><td></td></tr><tr><td>-</td><td>-</td><td>45</td><td>45</td><td>61</td><td>64</td><td>95</td><td>PB8</td><td>I/0/A</td><td>FT</td><td>PB8</td><td>TIM4_CH3SD10_D4TIM10_CH1DVP_D6ETH_MII_TXD3</td><td>I2C1_SCL_1CAN1_RX_2UART6_TX_1TIM8_CH3_1</td></tr><tr><td>TSSOP20</td><td>QFN28</td><td>LQFP48(V305CCT6)</td><td>LQFP48(V303CBT6)</td><td>LQFP64M(17)</td><td>QFN68</td><td>LQFP100</td><td></td></tr><tr><td>-</td><td>-</td><td>46</td><td>46</td><td>62</td><td>65</td><td>96</td><td>PB9</td><td>I/0/A</td><td>FT</td><td>PB9</td><td>TIM4_CH4SDIO_D5TIM10_CH2DVP_D7</td><td>I2C1_SDA_1CAN1_TX_2UART6_RX_1TIM8_BKIN_1</td></tr><tr><td>-</td><td>-</td><td>-</td><td>-</td><td>-</td><td>66</td><td>97</td><td>PEO</td><td>I/0</td><td>FT</td><td>PEO</td><td>TIM4_ETRFSMC_NBLO</td><td>TIM4_ETR_1UART4_TX_2UART4_TX_3</td></tr><tr><td>-</td><td>-</td><td>-</td><td>-</td><td>-</td><td>-</td><td>98</td><td>PE1</td><td>I/0</td><td>FT</td><td>PE1</td><td>FSMC_NBLO1</td><td>UART4_RX_2UART4_RX_3</td></tr><tr><td>-</td><td>-</td><td>47</td><td>47</td><td>63</td><td>-</td><td>99</td><td>VSS_3</td><td>P</td><td>-</td><td>VSS_3</td><td></td><td></td></tr><tr><td>-</td><td>-</td><td>-</td><td>-</td><td>64</td><td>67</td><td>100</td><td>V10_3</td><td>P</td><td>-</td><td>V10_3</td><td></td><td></td></tr><tr><td>-</td><td>-</td><td>-</td><td>-</td><td>-</td><td>68</td><td>-</td><td>VDD_3</td><td>P</td><td>-</td><td>VDD_3</td><td></td><td></td></tr><tr><td>-</td><td>-</td><td>48</td><td>48</td><td>-</td><td></td><td>-</td><td>VDD_10_3</td><td>P</td><td>-</td><td>VDD_10_3</td><td></td><td></td></tr><tr><td rowspan="2">14</td><td>21</td><td>-</td><td>-</td><td>-</td><td>-</td><td>-</td><td>VDD</td><td>P</td><td>-</td><td>VDD</td><td></td><td></td></tr><tr><td>6</td><td>-</td><td>-</td><td>-</td><td>-</td><td>-</td><td>V10</td><td>P</td><td>-</td><td>V10</td><td></td><td></td></tr></table>

注1：表格缩写解释：  
$I \ = \ T T L / C M O S$ 电平斯密特输入； $o =$ CMOS电平三态输出；A=模拟信号输入或输出；  
$\boldsymbol { P } =$ 电源； $F T =$ 耐受5V；ANT $=$ 射频信号输入输出（天线）。  
注2： $V _ { D D }$ 和 $V _ { B A T }$ 均可连接内部模拟开关为备份区域以及PC13、PC14和PC15引脚供电，这个模拟开关只能够通过有限的电流 $( 3 m A )$ 。当由 $V _ { D D }$ 供电时：PC14和PC15可用于GPI0或LSE引脚、PC13可作为通用I/0口、TAMPER引脚、RTC校准时钟、RTC闹钟或秒输出；PC13、PC14和PC15作为GPI0输出脚时只能工作在2MHz模式下，最大驱动负载为 $3 0 p F$ ，并且不能作为电流源(如驱动LED)。而当由 $V _ { B A T }$ 供电时：PC14和PC15只能用于LSE引脚、PC13可作为TAMPER引脚、RTC闹钟或秒输出。  
注3：这些引脚在备份区域第一次上电时处于主功能状态下，之后即使复位，这些引脚的状态由备份区域寄存器控制（这些寄存器不会被主复位系统所复位）。关干如何控制这些10口的具体信息，请参考CH32FV2xV3xRM手册的电池备份区域和BKP寄存器的相关章节。  
注4：LQFP64M封装的引脚5和引脚6在芯片复位后默认配置为0SCIN和0SCOUT功能脚。软件可以重新设置这两个引脚为PDO和PD1功能。但对于LQFP10O封装，由于PDO和PD1为固有的功能引脚，因此没有必要再由软件进行重映射设置。更多详细信息请参考CH32FV2x_V3×RM手册的复用功能I/0章节和调试设置章节。  
注5：B00T0和B00T1/PB2引脚都未引出的芯片，在内部B00T0引脚已短接到GND，B00T1/PB2引脚浮空，低功耗情况下，建议配置PB2为下拉输入模式，以防止产生额外电流。  
B0OT1/PB2引脚引出但B00T0引脚未引出的芯片，在内部B00T0引脚已短接到GND。  
B00T0引脚引出但B00T1/PB2引脚未引出的芯片，在内部B00T1/PB2引脚已短接到GND，低功耗情况下，建议配置PB2下拉输入模式，以防止产生额外电流。  
注6：对于CH32V305FBP6和CH32V305GBU6芯片，PA8和PC9引脚在芯片内部短接合封，禁止将两个I0均配置为输出功能，有功耗要求的注意引脚状态。

注7：SDI0_D0和SDI0_D1默认映射到PC8和PC9。仅对于批号倒数第五位大于1，或批号倒数第六位不为0的产品（除CH32V305GBU6芯片以外），当寄存器RCC_HBPCENR的bit[14]ETHMACEN=1与bit[10]SDI0EN=1时，SDI0_D0和SDI0_D1默认映射自动改到PB14和PB15。

注8：DVP_D5默认映射到PB6。仅对于批号倒数第五位大于1，或批号倒数第六位不为0的产品，当寄存器RCC_HBPCENR的bit[13]DVPEN=1与bit[11]USBHSEN=1且R8_USB_CTRL的bit[2]RB_UC_RST_SIE=0时，DVP_D5默认映射自动改到PB3。

注9：FSMC_NADV默认映射到PB7。仅对于批号倒数第五位大于1，或批号倒数第六位不为0的产品，当寄存器RCC_HBPCENR的bit[8]FSMCEN=1与bit[11]USBHSEN=1且R8_USB_CTRL的bit[2]RB_UC_RST_SIE=0时，FSMC_NADV默认映射自动改到PD2。

注10：I2S3_SD默认映射到PB5。仅对于批号倒数第五位大于2，或批号倒数第六位不为0的产品，如果同时使用10M以太网和I2S3功能，则I2S3_SD默认映射自动改到PA9。  
注11：I2S3_MCK默认映射到PC7。仅对于批号倒数第五位大于2，或批号倒数第六位不为0的产品，如果同时使用10M以太网和I2S3功能，则I2S3_MCK默认映射自动改到PA8。  
注12：SPI3_MOSI默认映射到PB5。仅对于批号倒数第五位为2，且批号倒数第六位为0的产品，当使用以太网时，I2S3默认引脚功能不可用，SPI3默认引脚的片选信号不可用，此时SPI3_MOSI默认映射自动改到PA15。  
注13：重映射功能下划线后的数值表示AFI0寄存器中相对应位的配置值。例如：UART4_RX_3表示AFI0寄存器相应位配置为11b。  
注14：TIM2_CH1和TIM2_ETR共用一个引脚，但不能同时使用。  
注15、注16：对于CH32V305FBP6芯片，PA5和PA1引脚在芯片内部短接合封，禁止将两个I0均配置为输出功能；PA9和PA13引脚在芯片内部短接合封，禁止将两个I0均配置为输出功能；有功耗要求的注意引脚状态。  
注17：CH32V303CBT6和CH32V303RBT6芯片均不支持TIM8。

表 3-2 CH32V317 引脚定义  

<table><tr><td colspan="2">引脚编号</td><td rowspan="2">引脚名称</td><td rowspan="2">引脚类型(1)</td><td rowspan="2">二</td><td rowspan="2">主功能(复位后)</td><td rowspan="2">默认复用功能</td><td rowspan="2">重映射功能(10)</td></tr><tr><td>QFN68</td><td>LQFP100</td></tr><tr><td>0</td><td>-</td><td>VSS</td><td>P</td><td>-</td><td>VSS</td><td></td><td></td></tr><tr><td>-</td><td>1</td><td>PE2</td><td>I/0</td><td>FT</td><td>PE2</td><td></td><td>TIM10_BKIN_2TIM10_BKIN_3</td></tr><tr><td>-</td><td>2</td><td>PE3</td><td>I/0</td><td>FT</td><td>PE3</td><td></td><td>TIM10_CH1N_2TIM10_CH1N_3</td></tr><tr><td>-</td><td>3</td><td>PE4</td><td>I/0</td><td>FT</td><td>PE4</td><td></td><td>TIM10_CH2N_2TIM10_CH2N_3</td></tr><tr><td>-</td><td>4</td><td>PE5</td><td>I/0</td><td>FT</td><td>PE5</td><td></td><td>TIM10_CH3N_2TIM10_CH3N_3</td></tr><tr><td>-</td><td>5</td><td>PE6</td><td>I/0</td><td>FT</td><td>PE6</td><td></td><td></td></tr><tr><td>1</td><td>6</td><td>VBAT</td><td>P</td><td>-</td><td>VBAT</td><td></td><td></td></tr><tr><td>2</td><td>7</td><td>PC13-TAMPER-RTC(2)</td><td>I/0</td><td>-</td><td>PC13(3)</td><td>TAMPER-RTC</td><td>TIM8_CH4_1</td></tr><tr><td>3</td><td>8</td><td>PC14-</td><td>I/0/A</td><td>-</td><td>PC14(3)</td><td>OSC32_IN</td><td>TIM9_CH4_1</td></tr><tr><td colspan="2">引脚编号</td><td rowspan="2">引脚名称</td><td rowspan="2">引脚类型(1)</td><td rowspan="2">I/O</td><td rowspan="2">主功能(复位后)</td><td rowspan="2">默认复用功能</td><td rowspan="2">重映射功能(10)</td></tr><tr><td>QFN68</td><td>LQFP100</td></tr><tr><td></td><td></td><td>OSC32_IN(2)</td><td></td><td></td><td></td><td></td><td></td></tr><tr><td>4</td><td>9</td><td>PC15-OSC32_OUT(2)</td><td>I/O/A</td><td>-</td><td>PC15(3)</td><td>OSC32_OUT</td><td>TIM10_CH4_1</td></tr><tr><td>-</td><td>10</td><td>VSS_5</td><td>P</td><td>-</td><td>VSS_5</td><td></td><td></td></tr><tr><td>-</td><td>11</td><td>VDD_5</td><td>P</td><td>-</td><td>VDD_5</td><td></td><td></td></tr><tr><td>5</td><td>12</td><td>OSC_IN</td><td>I/A</td><td>-</td><td>OSC_IN</td><td></td><td>PD0(4)</td></tr><tr><td>6</td><td>13</td><td>OSC_OUT</td><td>O/A</td><td>-</td><td>OSC_OUT</td><td></td><td>PD1(4)</td></tr><tr><td>7</td><td>14</td><td>NRST</td><td>I</td><td>-</td><td>NRST</td><td></td><td></td></tr><tr><td>8</td><td>15</td><td>PCO</td><td>I/O/A</td><td>-</td><td>PCO</td><td>ADC_IN10TIM9_CH1NUART6_TX</td><td></td></tr><tr><td>9</td><td>16</td><td>PC1</td><td>I/O/A</td><td>-</td><td>PC1</td><td>ADC_IN11TIM9_CH2NUART6_RX</td><td></td></tr><tr><td>10</td><td>17</td><td>PC2</td><td>I/O/A</td><td>-</td><td>PC2</td><td>ADC_IN12TIM9_CH3NUART7_TXOPA3_CH1N</td><td></td></tr><tr><td>11</td><td>18</td><td>PC3</td><td>I/O/A</td><td>-</td><td>PC3</td><td>ADC_IN13TIM10_CH3NUART7_RXOPA4_CH1N</td><td></td></tr><tr><td>12</td><td>19</td><td>VSSA</td><td>P</td><td>-</td><td>VSSA</td><td></td><td></td></tr><tr><td>-</td><td>20</td><td>VREF-</td><td>P</td><td>-</td><td>VREF-</td><td></td><td></td></tr><tr><td>-</td><td>21</td><td>VREF+</td><td>P</td><td>-</td><td>VREF+</td><td></td><td></td></tr><tr><td>13</td><td>22</td><td>VDDA</td><td>P</td><td>-</td><td>VDDA</td><td></td><td></td></tr><tr><td>14</td><td>23</td><td>PAO-WKUP</td><td>I/O/A</td><td>-</td><td>PAO</td><td>WKUPUSART2_CTSADC_IN0TIM2_CH1(11)TIM2_ETR(11)TIM5_CH1TIM8_ETROPA4_OUT0</td><td>TIM2_CH1_2(11)TIM2_ETR_2(11)TIM8_ETR_1</td></tr><tr><td>15</td><td>24</td><td>PA1</td><td>I/O/A</td><td>-</td><td>PA1</td><td>USART2_RTSADC_IN1TIM5_CH2TIM2_CH2OPA3_OUT0</td><td>TIM2_CH2_2TIM9_BKIN_1</td></tr><tr><td colspan="2">引脚编号</td><td rowspan="2">引脚名称</td><td rowspan="2">引脚类型(1)</td><td rowspan="2">引脚0/1</td><td rowspan="2">主功能(复位后)</td><td rowspan="2">默认复用功能</td><td rowspan="2">重映射功能(10)</td></tr><tr><td>QFN68</td><td>LQFP100</td></tr><tr><td>16</td><td>25</td><td>PA2</td><td>I/0/A</td><td>-</td><td>PA2</td><td>USART2_TXTIM5_CH3ADC_IN2TIM2_CH3TIM9_CH1TIM9_ETROPA2_OUT0</td><td>TIM2_CH3_1TIM9_CH1_1TIM9_ETR_1</td></tr><tr><td>17</td><td>26</td><td>PA3</td><td>I/0/A</td><td>-</td><td>PA3</td><td>USART2_RXTIM5_CH4ADC_IN3TIM2_CH4TIM9_CH2OPA1_OUT0</td><td>TIM2_CH4_1TIM9_CH2_1</td></tr><tr><td>-</td><td>27</td><td>VSS_4</td><td>P</td><td>-</td><td>VSS_4</td><td></td><td></td></tr><tr><td>18</td><td>28</td><td>V10_4</td><td>P</td><td>-</td><td>V10_4</td><td></td><td></td></tr><tr><td>19</td><td>29</td><td>PA4</td><td>I/0/A</td><td>-</td><td>PA4</td><td>SPI1 NSSUSART2_CKADC_IN4DAC1_OUTTIM9_CH3DVP_HSYNC</td><td>SPI3 NSS_1I2S3_WS_1TIM9_CH3_1</td></tr><tr><td>20</td><td>30</td><td>PA5</td><td>I/0/A</td><td>-</td><td>PA5</td><td>SPI1_SCKADC_IN5DAC2_OUTOPA2_CH1N DVP_VSYNC</td><td>TIM10_CH1N_1USART1_CTS_2USART1_CK_3</td></tr><tr><td>21</td><td>31</td><td>PA6</td><td>I/0/A</td><td>-</td><td>PA6</td><td>SPI1_MISO TIM8_BKIN ADC_IN6TIM3_CH1OPA1_CH1N DVP_PCLK</td><td>TIM1_BKIN_1USART1_TX_3UART7_TX_1TIM10_CH2N_1</td></tr><tr><td>22</td><td>32</td><td>PA7</td><td>I/0/A</td><td>-</td><td>PA7</td><td>SPI1_MOSI TIM8_CH1N ADC_IN7TIM3_CH2OPA2_CH1P</td><td>TIM1_CH1N_1USART1_RX_3UART7_RX_1TIM10_CH3N_1</td></tr><tr><td>23</td><td>33</td><td>PC4</td><td>I/0/A</td><td>-</td><td>PC4</td><td>ADC_IN14TIM9_CH4 UART8_TX</td><td>USART1_CTS_3</td></tr><tr><td colspan="2">引脚编号</td><td rowspan="2">引脚名称</td><td rowspan="2">引脚类型(1)</td><td rowspan="2">I/O</td><td rowspan="2">主功能(复位后)</td><td rowspan="2">默认复用功能</td><td rowspan="2">重映射功能(10)</td></tr><tr><td>QFN68</td><td>LQFP100</td></tr><tr><td></td><td></td><td></td><td></td><td></td><td></td><td>OPA4_CH1P</td><td></td></tr><tr><td>24</td><td>34</td><td>PC5</td><td>I/0/A</td><td>-</td><td>PC5</td><td>ADC_IN15TIM9_BKUNART8_RXOPA3_CH1P</td><td>USART1_RTS_3</td></tr><tr><td>25</td><td>35</td><td>PBO</td><td>I/0/A</td><td>-</td><td>PBO</td><td>ADC_IN8TIM3_CH3TIM8_CH2NOPA1_CH1P</td><td>TIM1_CH2N_1TIM3_CH3_2TIM9_CH1N_1UART4_TX_1</td></tr><tr><td>26</td><td>36</td><td>PB1</td><td>I/0/A</td><td>-</td><td>PB1</td><td>ADC_IN9TIM3_CH4TIM8_CH3NOPA4_CHON</td><td>TIM1_CH3N_1TIM3_CH4_2TIM9_CH2N_1UART4_RX_1</td></tr><tr><td>27</td><td>37</td><td>PB2(5)</td><td>I/0</td><td>FT</td><td>PB2BOOT1(5)</td><td>OPA3_CHON</td><td>TIM9_CH3N_1</td></tr><tr><td>28</td><td>38</td><td>LEDO</td><td>I/0</td><td>-</td><td>LEDO</td><td></td><td></td></tr><tr><td>29</td><td>39</td><td>LED1</td><td>I/0</td><td>-</td><td>LED1</td><td></td><td></td></tr><tr><td>30</td><td>40</td><td>VDDK</td><td>P</td><td>-</td><td>VDDK</td><td></td><td></td></tr><tr><td>-</td><td>41</td><td>VSS_6</td><td>P</td><td>-</td><td>VSS_6</td><td></td><td></td></tr><tr><td>31</td><td>42</td><td>MDITP(12)</td><td>I/0</td><td>-</td><td>MDITP</td><td></td><td></td></tr><tr><td>32</td><td>43</td><td>MDITN(12)</td><td>I/0</td><td>-</td><td>MDITN</td><td></td><td></td></tr><tr><td>33</td><td>44</td><td>MDIRP(12)</td><td>I/0</td><td>-</td><td>MDIRP</td><td></td><td></td></tr><tr><td>34</td><td>45</td><td>MDIRN(12)</td><td>I/0</td><td>-</td><td>MDIRN</td><td></td><td></td></tr><tr><td>35</td><td>46</td><td>VDD ETH</td><td>P</td><td>-</td><td>VDD ETH</td><td></td><td></td></tr><tr><td>-</td><td>47</td><td>VSS_1</td><td>P</td><td>-</td><td>VSS_1</td><td></td><td></td></tr><tr><td>36</td><td>48</td><td>V10_1</td><td>P</td><td>-</td><td>V10_1</td><td></td><td></td></tr><tr><td>42</td><td>49</td><td>PB10(6)</td><td>I/0/A</td><td>FT</td><td>PB10</td><td>I2C2_SCLUSART3_TXOPA2_CHON</td><td>TIM2_CH3_2TIM2_CH3_3TIM10_BKIN_1</td></tr><tr><td>41</td><td>50</td><td>PB11(7)</td><td>I/0/A</td><td>FT</td><td>PB11</td><td>I2C2_SDAUSART3_RXOPA1_CHON</td><td>TIM2_CH4_2TIM2_CH4_3TIM10_ETR_1</td></tr><tr><td>-</td><td>51</td><td>VDD_1</td><td>P</td><td></td><td>VDD_1</td><td></td><td></td></tr><tr><td>37</td><td>52</td><td>PB12</td><td>I/0/A</td><td>FT</td><td>PB12</td><td>SPI2 NSS12S2_WS12C2_SMBAUSART3_CKTIM1_BKINOPA4_CHOP</td><td></td></tr><tr><td></td><td></td><td></td><td></td><td></td><td></td><td>CAN2_RX</td><td></td></tr><tr><td>38</td><td>53</td><td>PB13</td><td>I/0/A</td><td>FT</td><td>PB13</td><td>SPI2_SCK12S2_CKUSART3_CTSTIM1_CH1NOPA3_CHOPCAN2_TX</td><td>USART3_CTS_1</td></tr><tr><td>39</td><td>54</td><td>PB14</td><td>I/0/A</td><td>FT</td><td>PB14</td><td>SPI2_MISO12S2_SDUSART3_RTSOPA2_CHOPSDIO_D0(8)</td><td>USART3_RTS_1</td></tr><tr><td>40</td><td>55</td><td>PB15</td><td>I/0/A</td><td>FT</td><td>PB15</td><td>SPI2_MOSI12S2_SDUSART1_CH3NOPA1_CHOPSDIO_D1(8)</td><td>USART1_TX_2</td></tr><tr><td>-</td><td>56</td><td>PD9</td><td>I/0</td><td>FT</td><td>PD9</td><td></td><td>USART3_RX_3TIM9_CH1_2TIM9_ETR_2TIM9_CH1_3TIM9_ETR_3</td></tr><tr><td>-</td><td>57</td><td>PD10</td><td>I/0</td><td>FT</td><td>PD10</td><td></td><td>USART3_CK_2USART3_CK_3TIM9_CH2N_2TIM9_CH2N_3</td></tr><tr><td>-</td><td>58</td><td>PD11</td><td>I/0</td><td>FT</td><td>PD11</td><td></td><td>USART3_CTS_2USART3_CTS_3TIM9_CH2_2TIM9_CH2_3</td></tr><tr><td>-</td><td>59</td><td>PD12</td><td>I/0</td><td>FT</td><td>PD12</td><td></td><td>TIM4_CH1_1TIM9_CH3N_2TIM9_CH3N_3USART3_RTS_3USART3_RTS_2</td></tr><tr><td>-</td><td>60</td><td>PD13</td><td>I/0</td><td>FT</td><td>PD13</td><td></td><td>TIM4_CH2_1TIM9_CH3_2TIM9_CH3_3</td></tr><tr><td>41</td><td>61</td><td>PD14(7)</td><td>I/0</td><td>FT</td><td>PD14</td><td></td><td>TIM4_CH3_1TIM9_BKIN_2</td></tr><tr><td colspan="2">引脚编号</td><td rowspan="2">引脚名称</td><td rowspan="2">引脚类型(1)</td><td rowspan="2">引脚长度</td><td rowspan="2">主功能(复位后)</td><td rowspan="2">默认复用功能</td><td rowspan="2">重映射功能(10)</td></tr><tr><td>QFN68</td><td>LQFP100</td></tr><tr><td></td><td></td><td></td><td></td><td></td><td></td><td></td><td>TIM9_BKIN_3</td></tr><tr><td>42</td><td>62</td><td>PD15(6)</td><td>I/0</td><td>FT</td><td>PD15</td><td></td><td>TIM4_CH4_1TIM9_CH4_2TIM9_CH4_3</td></tr><tr><td>-</td><td>63</td><td>PC6</td><td>I/0</td><td>FT</td><td>PC6</td><td>I2S2_MCKTIM8_CH1SDIO_D6</td><td>TIM3_CH1_3</td></tr><tr><td>-</td><td>64</td><td>PC7</td><td>I/0</td><td>FT</td><td>PC7</td><td>I2S3_MCKTIM8_CH2SDIO_D7</td><td>TIM3_CH2_3</td></tr><tr><td>-</td><td>65</td><td>PC8</td><td>I/0</td><td>FT</td><td>PC8</td><td>TIM8_CH3SDIO_D0(8)DVP_D2</td><td>TIM3_CH3_3</td></tr><tr><td>-</td><td>66</td><td>PC9</td><td>I/0</td><td>FT</td><td>PC9</td><td>TIM8_CH4SDIO_D1(8)DVP_D3</td><td>TIM3_CH4_3</td></tr><tr><td>43</td><td>67</td><td>PA8</td><td>I/0</td><td>FT</td><td>PA8</td><td>USART1_CKTIM1_CH1MCO</td><td>USART1_CK_1USART1_RX_2TIM1_CH1_1</td></tr><tr><td>44</td><td>68</td><td>PA9</td><td>I/0</td><td>FT</td><td>PA9</td><td>USART1_TXTIM1_CH2OTG_FS_VBUSDVP_D0</td><td>USART1_RTS_2TIM1_CH2_1</td></tr><tr><td>45</td><td>69</td><td>PA10</td><td>I/0</td><td>FT</td><td>PA10</td><td>USART1_RXTIM1_CH3OTG_FS_IDDVP_D1</td><td>USART1_CK_2TIM1_CH3_1</td></tr><tr><td>46</td><td>70</td><td>PA11</td><td>I/0/A</td><td>FT</td><td>PA11</td><td>USART1_CTSCAN1_RXTIM1_CH4OTG_FS_DM</td><td>USART1_CTS_1TIM1_CH4_1</td></tr><tr><td>47</td><td>71</td><td>PA12</td><td>I/0/A</td><td>FT</td><td>PA12</td><td>USART1_RTSCAN1_TXTIM1_ETRTIM10_CH1NOTG_FS_DP</td><td>USART1_RTS_1TIM1_ETR_1</td></tr><tr><td>48</td><td>72</td><td>PA13</td><td>I/0</td><td>FT</td><td>SWDIO</td><td>TIM10_CH2N</td><td>PA13TIM8_CH1N_1USART3_TX_2</td></tr><tr><td colspan="2">引脚编号</td><td rowspan="2">引脚名称</td><td rowspan="2">引脚类型(1)</td><td rowspan="2">I/O</td><td rowspan="2">主功能(复位后)</td><td rowspan="2">默认复用功能</td><td rowspan="2">重映射功能(10)</td></tr><tr><td>QFN68</td><td>LQFP100</td></tr><tr><td>-</td><td>73</td><td>NC.</td><td></td><td></td><td></td><td></td><td></td></tr><tr><td>49</td><td>74</td><td>Vss_2</td><td>P</td><td>-</td><td>Vss_2</td><td></td><td></td></tr><tr><td>50</td><td>75</td><td>VDD_2</td><td>P</td><td>-</td><td>VDD_2</td><td></td><td></td></tr><tr><td>51</td><td>-</td><td>V10_2</td><td>P</td><td>-</td><td>V10_2</td><td></td><td></td></tr><tr><td>52</td><td>76</td><td>PA14</td><td>I/0</td><td>FT</td><td>SWCLK</td><td>TIM10_CH3N</td><td>TIM8_CH2N_1UART8_TX_1USART3_RX_2</td></tr><tr><td>53</td><td>77</td><td>PA15</td><td>I/0</td><td>FT</td><td>PA15</td><td>SPI3 NSS12S3_WS</td><td>TIM2_CH1_1(11)TIM2_ETR_1(11)TIM2_CH1_3(11)TIM2_ETR_3(11)SPI1 NSS_1TIM8_CH3N_1UART8_RX_1</td></tr><tr><td>54</td><td>78</td><td>PC10</td><td>I/0</td><td>FT</td><td>PC10</td><td>UART4_TXSDIO_D2TIM10_ETR DVP_D8</td><td>USART3_TX_1SPI3_SCK_1I2S3_CK_1</td></tr><tr><td>55</td><td>79</td><td>PC11</td><td>I/0</td><td>FT</td><td>PC11</td><td>UART4_RXSDIO_D3TIM10_CH4 DVP_D4</td><td>USART3_RX_1SPI3_MISO_1</td></tr><tr><td>56</td><td>80</td><td>PC12</td><td>I/0</td><td>FT</td><td>PC12</td><td>UART5_TXSDIO_CKTIM10_BKIN DVP_D9</td><td>USART3_CK_1SPI3_MOSI_1I2S3_SD_1</td></tr><tr><td>-</td><td>81</td><td>PDO</td><td>I/0/A</td><td>FT</td><td>PDO</td><td></td><td>CAN1_RX_3TIM10_ETR_2TIM10_ETR_3</td></tr><tr><td>-</td><td>82</td><td>PD1</td><td>I/0/A</td><td>FT</td><td>PD1</td><td></td><td>CAN1_TX_3TIM10_CH1_2TIM10_CH1_3</td></tr><tr><td>57</td><td>83</td><td>PD2</td><td>I/0</td><td>FT</td><td>PD2</td><td>TIM3_ETRUART5_RXSDIO_CMD DVP_D11</td><td>TIM3_ETR_2TIM3_ETR_3</td></tr><tr><td>-</td><td>84</td><td>PD3</td><td>I/0</td><td>FT</td><td>PD3</td><td></td><td>USART2_CTS_1TIM10_CH2_2TIM10_CH2_3</td></tr><tr><td>-</td><td>85</td><td>PD4</td><td>I/0</td><td>FT</td><td>PD4</td><td></td><td>USART2_RTS_1</td></tr><tr><td>-</td><td>86</td><td>PD5</td><td>I/0</td><td>FT</td><td>PD5</td><td></td><td>USART2_TX_1TIM10_CH3_2TIM10_CH3_3</td></tr><tr><td>-</td><td>87</td><td>PD6</td><td>I/0</td><td>FT</td><td>PD6</td><td>DVP_D10</td><td>USART2_RX_1</td></tr><tr><td>-</td><td>88</td><td>PD7</td><td>I/0</td><td>FT</td><td>PD7</td><td></td><td>USART2_CK_1TIM10_CH4_2TIM10_CH4_3</td></tr><tr><td>58</td><td>89</td><td>PB3</td><td>I/0</td><td>FT</td><td>PB3</td><td>SPI3_SCK12S3_CKDVP_D5(9)</td><td>TIM2_CH2_1TIM2_CH2_3SPI1_SCK_1TIM10_CH1_1</td></tr><tr><td>59</td><td>90</td><td>PB4</td><td>I/0</td><td>FT</td><td>PB4</td><td>SPI3_MISO</td><td>TIM3_CH1_2SPI1_MISO_1UART5_TX_1TIM10_CH2_1</td></tr><tr><td>60</td><td>91</td><td>PB5</td><td>I/0</td><td>FT</td><td>PB5</td><td>I2C1_SMBSPI3_MOSI12S3_SD</td><td>TIM3_CH2_2SPI1_MOSI_1CAN2_RX_1TIM10_CH3_1UART5_RX_1</td></tr><tr><td>61</td><td>92</td><td>PB6</td><td>I/0</td><td>FT</td><td>PB6</td><td>I2C1_SCLTIM4_CH1DVP_D5(9)USBHS_DM</td><td>USART1_TX_1CAN2_TX_1TIM8_CH1_1</td></tr><tr><td>62</td><td>93</td><td>PB7</td><td>I/0</td><td>FT</td><td>PB7</td><td>I2C1_SDATIM4_CH2USBHS_DP</td><td>USART1_RX_1TIM8_CH2_1</td></tr><tr><td>63</td><td>94</td><td>B00TO(5)</td><td>I</td><td>-</td><td>B00TO(5)</td><td></td><td></td></tr><tr><td>64</td><td>95</td><td>PB8</td><td>I/0/A</td><td>FT</td><td>PB8</td><td>TIM4_CH3SDIO_D4TIM10_CH1DVP_D6</td><td>I2C1_SCL_1CAN1_RX_2UART6_TX_1TIM8_CH3_1</td></tr><tr><td>65</td><td>96</td><td>PB9</td><td>I/0/A</td><td>FT</td><td>PB9</td><td>TIM4_CH4SDIO_D5TIM10_CH2DVP_D7</td><td>I2C1_SDA_1CAN1_TX_2UART6_RX_1TIM8_BKIN_1</td></tr><tr><td>66</td><td>97</td><td>PEO</td><td>I/0</td><td>FT</td><td>PEO</td><td>TIM4_ETR</td><td>TIM4_ETR_1UART4_TX_2</td></tr><tr><td colspan="2">引脚编号</td><td rowspan="2">引脚名称</td><td rowspan="2">引脚类型(1)</td><td rowspan="2">二/三卡</td><td rowspan="2">主功能(复位后)</td><td rowspan="2">默认复用功能</td><td rowspan="2">重映射功能(10)</td></tr><tr><td>QFN68</td><td>LQFP100</td></tr><tr><td></td><td></td><td></td><td></td><td></td><td></td><td></td><td>UART4_TX_3</td></tr><tr><td>-</td><td>98</td><td>PE1</td><td>I/0</td><td>FT</td><td>PE1</td><td></td><td>UART4_RX_2
UART4_RX_3</td></tr><tr><td>-</td><td>99</td><td>VSS_3</td><td>P</td><td>-</td><td>VSS_3</td><td></td><td></td></tr><tr><td>67</td><td>100</td><td>V10_3</td><td>P</td><td>-</td><td>V10_3</td><td></td><td></td></tr><tr><td>68</td><td>-</td><td>VDD_3</td><td>P</td><td>-</td><td>VDD_3</td><td></td><td></td></tr></table>

注1：表格缩写解释：  
$I \ = \ T T L / C M O S$ 电平斯密特输入； $o =$ CMOS电平三态输出；A $=$ 模拟信号输入或输出；  
$\boldsymbol { P } =$ 电源； $F T =$ 耐受 $5 V _ { o }$   
注2： $V _ { D D }$ 和 $\mathbb { V } _ { B A T }$ 均可连接内部模拟开关为备份区域以及PC13、PC14和PC15引脚供电，这个模拟开关只能够通过有限的电流 $( 3 m A )$ 。当由 $V _ { D D }$ 供电时：PC14和PC15可用于GPI0或LSE引脚、PC13可作为通用I/0口、JAMPER引脚、RTC校准时钟、RTC闹钟或秒输出；PC13、PC14和PC15作为GPI0输出脚时只能工作在2MHz模式下，最大驱动负载为 $3 0 p F$ ，并且不能作为电流源(如驱动LED)。而当由 $V _ { B A T }$ 供电时：PC14和PC15只能用于LSE引脚、PC13可作为TAMPER引脚、RTC闹钟或秒输出。  
注3：这些引脚在备份区域第一次上电时处于主功能状态下，之后即使复位，这些引脚的状态由备份区域寄存器控制（这些寄存器不会被主复位系统所复位）。关于如何控制这些I0口的具体信息，请参考CH32FV2x_V3xRM手册的电池备份区域和BKP寄存器的相关章节。  
注4：对于CH32V317WCU6芯片，引脚5和引脚6在芯片复位后默认配置为0SC_IN和OSC_OUT功能脚。软件可以重新设置这两个引脚为PD0和PD1功能。但对于CH32V317VCT6芯片，由于PD0和PD1为固有的功能引脚，因此没有必要再由软件进行重映射设置。更多详细信息请参考CH32FV2x_V3×RM手册的复用功能I/0章节和调试设置章节。  
注5：B00T0和B00T1/PB2引脚都未引出的芯片，在内部B00T0引脚已短接到GND，B00T1/PB2引脚浮空，低功耗情况下，建议配置PB2为下拉输入模式，以防止产生额外电流。  
B00T1/PB2引脚引出但B0OT0引脚未引出的芯片，在内部B00T0引脚已短接到GND。  
B00T0引脚引出但B00T1/PB2引脚未引出的芯片，在内部B00T1/PB2引脚已短接到GND，低功耗情况下，建议配置PB2下拉输入模式，以防止产生额外电流。  
注6、注7：对于CH32V317WCU6芯片，PB10和PD15引脚在芯片内部短接合封，禁止将两个I0均配置为输出功能，有功耗要求的注意引脚状态；PB11和PD14引脚在芯片内部短接合封，禁止将两个I0均配置为输出功能，有功耗要求的注意引脚状态。  
注8：SDI0_D0和SDI0_D1默认映射到PC8和PC9。当寄存器RCC_HBPCENR的bit[14]ETHMACEN=1与bit[10]SDI0EN=1时，SDI0_D0和SDI0_D1默认映射自动改到PB14和PB15。  
注9：DVP_D5默认映射到PB6。当寄存器RCC_HBPCENR的bit[13]DVPEN=1与bit[11]USBHSEN=1且R8_USB_CTRL的bit[2]RB_UC_RST_SIE=0时，DVP_D5默认映射自动改到PB3。  
注10：重映射功能下划线后的数值表示AFI0寄存器中相对应位的配置值。例如：UART4RX3表示AFI0寄存器相应位配置为11b。  
注11：TIM2_CH1和TIM2_ETR共用一个引脚，但不能同时使用。  
注12：CH32V317芯片支持以太网引脚RX/TX收发识别和成对交换，支持引脚MDIRP/MDIRN正负识别和交换，支持引脚MDITP/MDITN正负识别和交换。

表 3-3 CH32V317 专有引脚说明  

<table><tr><td>引脚名称</td><td colspan="5">引脚说明</td></tr><tr><td>VDD ETH</td><td colspan="5">为10/100M以太网PHY供电,建议1uF~4.7uF对地电容贴近芯片放置,支持10uF但需并联0.1uF。</td></tr><tr><td>VDDK</td><td colspan="5">外接1uF对地电容贴近芯片放置。</td></tr><tr><td>MDI TP</td><td colspan="5">10BASE-T/100BASE-TX MDI模式下的差分输出;</td></tr><tr><td>MDI TN</td><td colspan="5">10BASE-T/100BASE-TX MDIX模式下的差分输入。</td></tr><tr><td>MDIRP</td><td colspan="5">10BASE-T/100BASE-TX MDI模式下的差分输入;</td></tr><tr><td>MDIRN</td><td colspan="5">10BASE-T/100BASE-TX MDIX模式下的差分输出。</td></tr><tr><td rowspan="3">LEDO</td><td colspan="5">传统LED功能选择,默认LED_SEL为11:</td></tr><tr><td>LED_SEL</td><td>00</td><td>01</td><td>10</td><td>11</td></tr><tr><td>LEDO</td><td>ACTALL</td><td>LINKALL/ACTALL</td><td>LINK10/ACTALL</td><td>LINK10/ACT10</td></tr><tr><td rowspan="3">LED1</td><td colspan="5">传统LED功能选择,默认LED_SEL为11:</td></tr><tr><td>LED_SEL</td><td>00</td><td>01</td><td>10</td><td>11</td></tr><tr><td>LED1</td><td>LINK100</td><td>LINK100</td><td>LINK100</td><td>LINK100/ACT100</td></tr></table>

# 3.3 引脚复用功能

注意，下表中的引脚功能描述针对的是所有功能，不涉及具体型号产品。不同型号之间外设资源有差异，查看前请先根据产品型号资源表确认是否有此功能。表 3-4 引脚复用和重映射功能

<table><tr><td>复用引脚</td><td>ADC DAC</td><td>TIM1 8/9/10</td><td>TIM2 3/4/5</td><td>UARTUSART</td><td>USB</td><td>SYS</td><td>I2G</td><td>SPI 12S</td><td>ETH</td><td>FSMC SDIO</td><td>DVP</td><td>OPA</td><td>CAN</td></tr><tr><td>PA0</td><td>ADC_IN0</td><td>TIM8_ETR TIM8_ETR_1</td><td>TIM2_CH1 TIM2_ETR TIM2_CH1_2 TIM2_ETR_2 TIM5_CH1</td><td>USART2_CTS</td><td></td><td>WKUP</td><td></td><td></td><td>ETH_MII_CRS ETH_RGMII_RXD2</td><td></td><td></td><td>OPA4_OUT0</td><td></td></tr><tr><td>PA1</td><td>ADC_IN1</td><td>TIM9_BKIN_1</td><td>TIM2_CH2 TIM2_CH2_2 TIM5_CH2</td><td>USART2_RTS</td><td></td><td></td><td></td><td></td><td>ETH_MII_RX_CLK ETH_RMI_REF_CLK ETH_RGMII_RXD3</td><td></td><td></td><td>OPA3_OUT0</td><td></td></tr><tr><td>PA2</td><td>ADC_IN2</td><td>TIM9_CH1 TIM9_CH1_1 TIM9_ETR TIM9_ETR_1</td><td>TIM2_CH3 TIM2_CH3_1 TIM5_CH3</td><td>USART2_TX</td><td></td><td></td><td></td><td></td><td>ETH_MII_MDIO ETH_RMI_MDIO ETH_RGMII_GTXC</td><td></td><td></td><td>OPA2_OUT0</td><td></td></tr><tr><td>PA3</td><td>ADC_IN3</td><td>TIM9_CH2 TIM9_CH2_1</td><td>TIM2_CH4 TIM2_CH4_1 TIM5_CH4</td><td>USART2_RX</td><td></td><td></td><td></td><td></td><td>ETH_MII_COL ETH_RGMII_TXEN</td><td></td><td></td><td>OPA1_OUT0</td><td></td></tr><tr><td>PA4</td><td>ADC_IN4 DAC1_OUT</td><td>TIM9_CH3 TIM9_CH3_1</td><td></td><td>USART2_CK</td><td></td><td></td><td></td><td>SPI1 NSS SP13 NSS_1 I253_WS_1</td><td></td><td></td><td>DVP_HSYNC</td><td></td><td></td></tr><tr><td>PA5</td><td>ADC_IN5 DAC2_OUT</td><td>TIM10_CH1N_1</td><td></td><td>USART1_CTS_2USART1_CK_3</td><td></td><td></td><td></td><td>SPI1_SCK</td><td></td><td></td><td>DVP_VSYNC</td><td>OPA2_CH1N</td><td></td></tr><tr><td>PA6</td><td>ADC_IN6</td><td>TIM1_BKIN_1 TIM8_BKIN TIM10_CH2N_1</td><td>TIM3_CH1</td><td>USART1_TX_3 UART7_TX_1</td><td></td><td></td><td></td><td>SPI1_MISO</td><td></td><td></td><td>DVP_PCLK</td><td>OPA1_CH1N</td><td></td></tr><tr><td>PA7</td><td>ADC_IN7</td><td>TIM1_CH1N_1 TIM8_CH1N TIM10_CH3N_1</td><td>TIM3_CH2</td><td>USART1_RX_3 UART7_RX_1</td><td></td><td></td><td></td><td>SPI1_MOSI</td><td>ETH_MII_RX_DV ETH_RMI_CRS_DV ETH_RGMII_TXDO</td><td></td><td></td><td>OPA2_CH1P</td><td></td></tr><tr><td>PA8</td><td></td><td>TIM1_CH1 TIM1_CH1_1</td><td></td><td>USART1_CKUSART1_CK_1USART1_RX_2</td><td></td><td>MCO</td><td></td><td>I2S3_MCK</td><td></td><td></td><td></td><td></td><td></td></tr><tr><td>PA9</td><td></td><td>TIM1_CH2 TIM1_CH2_1</td><td></td><td>USART1_TXUSART1_RTS_2</td><td>OTG_FS_VBUS</td><td></td><td></td><td>I2S3_SD</td><td></td><td></td><td>DVP_D0</td><td></td><td></td></tr><tr><td>PA10</td><td></td><td>TIM1_CH3 TIM1_CH3_1</td><td></td><td>USART1_RXUSART1_CK_2</td><td>OTG_FS_ID</td><td></td><td></td><td></td><td></td><td></td><td>DVP_D1</td><td></td><td></td></tr><tr><td>PA11</td><td></td><td>TIM1_CH4 TIM1_CH4_1</td><td></td><td>USART1_CTSUSART1_CTS_1</td><td>OTG_FS_DM</td><td></td><td></td><td></td><td></td><td></td><td></td><td></td><td>CAN1_RX</td></tr><tr><td>PA12</td><td></td><td>TIM1_ETR TIM1_ETR_1 TIM10_CH1N</td><td></td><td>USART1_RTSUSART1_RTS_1</td><td>OTG_FS_DP</td><td></td><td></td><td></td><td></td><td></td><td></td><td></td><td>CAN1_TX</td></tr><tr><td>PA13</td><td></td><td>TIM8_CH1N_1 TIM10_CH2N</td><td></td><td>USART3_TX_2</td><td></td><td>SWDIO</td><td></td><td></td><td></td><td></td><td></td><td></td><td></td></tr><tr><td>PA14</td><td></td><td>TIM8_CH2N_1 TIM10_CH3N</td><td></td><td>UART8_TX_1USART3_RX_2</td><td></td><td>SWCLK</td><td></td><td></td><td></td><td></td><td></td><td></td><td></td></tr><tr><td>PA15</td><td></td><td>TIM8_CH3N_1</td><td>TIM2_CH1_1TIM2_ETR_1TIM2_CH1_3TIM2_ETR_3</td><td>UART8_RX_1</td><td></td><td></td><td></td><td>SPI1_NSS_1SPI13_MOSI SPI13_NSS 12S3_WS</td><td></td><td></td><td></td><td></td><td></td></tr><tr><td>PB0</td><td>ADC_IN8</td><td>TIM1 CH2N_1 TIM8 CH2N TIM9 CH1N_1</td><td>TIM3_CH3 TIM3 CH3_2</td><td>UART4_TX_1</td><td></td><td></td><td></td><td></td><td>ETH_MII_RXD2 ETH_RGMII_TXD3</td><td></td><td></td><td>OPA1_CH1P</td><td></td></tr><tr><td>PB1</td><td>ADC_IN9</td><td>TIM1 CH3N_1 TIM8 CH3N TIM9 CH2N_1</td><td>TIM3_CH4 TIM3 CH4_2</td><td>UART4_RX_1</td><td></td><td></td><td></td><td></td><td>ETH_MII_RXD3 ETH_RGMII_1251N</td><td></td><td></td><td>OPA4_CHON</td><td></td></tr><tr><td>PB2</td><td></td><td>TIM9_CH3N_1</td><td></td><td></td><td></td><td>BOOT1</td><td></td><td></td><td></td><td></td><td></td><td>OPA3_CHON</td><td></td></tr><tr><td>PB3</td><td></td><td>TIM10_CH1_1</td><td>TIM2_CH2_1TIM2_CH2_3</td><td></td><td></td><td></td><td></td><td>SPI1_SCK_1 SPI13_SCK 12S3_CK</td><td></td><td></td><td>DVP_D5</td><td></td><td></td></tr><tr><td>PB4</td><td></td><td>TIM10_CH2_1</td><td>TIM3_CH1_2</td><td>UART5_TX_1</td><td></td><td></td><td></td><td>SPI1_MISO_1 SPI13_MISO</td><td></td><td></td><td></td><td></td><td></td></tr><tr><td>复用引脚</td><td>ADC DAC</td><td>TIM1 8/9/10</td><td>TIM2 3/4/5</td><td>UART USART</td><td>USB</td><td>SYS</td><td>I2C</td><td>SPI 12S</td><td>ETH</td><td>FSMC SD10</td><td>DVP</td><td>OPA</td><td>CAN</td></tr><tr><td>PB5</td><td></td><td>TIM10_CH3_1</td><td>TIM3_CH2_2</td><td>UART5_RX_1</td><td></td><td></td><td>I2C1_SMBA</td><td>SP11_MOSI_1SP13_MOSI 12S3_SD</td><td>ETH_MII_PPS_OUTETH_RMI_PPS_OUT</td><td></td><td></td><td></td><td>CAN2_RX_1</td></tr><tr><td>PB6</td><td></td><td>TIM8_CH1_1</td><td>TIM4_CH1</td><td>USART1_TX_1</td><td>USBHS_DM</td><td></td><td>I2C1_SCL</td><td></td><td></td><td></td><td>DVP_D5</td><td></td><td>CAN2_TX_1</td></tr><tr><td>PB7</td><td></td><td>TIM8_CH2_1</td><td>TIM4_CH2</td><td>USART1_RX_1</td><td>USBHS_DP</td><td></td><td>I2C1_SDA</td><td></td><td></td><td>FSMC_NADV</td><td></td><td></td><td></td></tr><tr><td>PB8</td><td></td><td>TIM8_CH3_1TIM10_CH1</td><td>TIM4_CH3</td><td>UART6_TX_1</td><td></td><td></td><td>I2C1_SCL_1</td><td></td><td>ETH_MII_TXD3</td><td>SD10_D4</td><td>DVP_D6</td><td></td><td>CAN1_RX_2</td></tr><tr><td>PB9</td><td></td><td>TIM8_BKIN_1TIM10_CH2</td><td>TIM4_CH4</td><td>UART6_RX_1</td><td></td><td></td><td>I2C1_SDA_1</td><td></td><td></td><td>SD10_D5</td><td>DVP_D7</td><td></td><td>CAN1_TX_2</td></tr><tr><td>PB10</td><td></td><td>TIM10_BKIN_1</td><td>TIM2_CH3_2TIM2_CH3_3</td><td>USART3_TX</td><td></td><td></td><td>I2C2_SCL</td><td></td><td>ETH_MII_RX_ER</td><td></td><td></td><td>OPA2_CHON</td><td></td></tr><tr><td>PB11</td><td></td><td>TIM10_ETR_1</td><td>TIM2_CH4_2TIM2_CH4_3</td><td>USART3_RX</td><td></td><td></td><td>I2C2_SDA</td><td></td><td>ETH_MII_TX ENETH_RMI_TX EN</td><td></td><td></td><td>OPA1_CHON</td><td></td></tr><tr><td>PB12</td><td></td><td>TIM1_BKIN</td><td></td><td>USART3_CK</td><td></td><td></td><td>I2C2_SMBA</td><td>SP12 NSS12S2_WS</td><td>ETH_MII_TXD0ETH_RMI_TXD0ETH_RGM1_MDC</td><td></td><td></td><td>OPA4_CHOP</td><td>CAN2_RX</td></tr><tr><td>PB13</td><td></td><td>TIM1_CH1N</td><td></td><td>USART3_CTSUSART3_CTS_1</td><td></td><td></td><td></td><td>SP12_SCK12S2_CK</td><td>ETH_MII_TXD1ETH_RMI_TXD1ETH_RGM1_MD10</td><td></td><td></td><td>OPA3_CHOP</td><td>CAN2_TX</td></tr><tr><td>PB14</td><td></td><td>TIM1_CH2N</td><td></td><td>USART3_RTSUSART3_RTS_1</td><td></td><td></td><td></td><td>SP12_MISO</td><td></td><td>SD10_D0</td><td></td><td>OPA2_CHOP</td><td></td></tr><tr><td>PB15</td><td></td><td>TIM1_CH3N</td><td></td><td>USART1_TX_2</td><td></td><td></td><td></td><td>SP12_MOSI12S2_SD</td><td></td><td>SD10_D1</td><td></td><td>OPA1_CHOP</td><td></td></tr><tr><td>PC0</td><td>ADC_IN10</td><td>TIM9_CH1N</td><td></td><td>UART6_TX</td><td></td><td></td><td></td><td></td><td>ETH_RGM1_RXC</td><td></td><td></td><td></td><td></td></tr><tr><td>PC1</td><td>ADC_IN11</td><td>TIM9_CH2N</td><td></td><td>UART6_RX</td><td></td><td></td><td></td><td></td><td>ETH_MII_MDCETH_RMI_MDCETH_RGM1_RXCTL</td><td></td><td></td><td></td><td></td></tr><tr><td>PC2</td><td>ADC_IN12</td><td>TIM9_CH3N</td><td></td><td>UART7_TX</td><td></td><td></td><td></td><td></td><td>ETH_MII_TXD2ETH_RGM1_RXD0</td><td></td><td></td><td>OPA3_CH1N</td><td></td></tr><tr><td>PC3</td><td>ADC_IN13</td><td>TIM10_CH3</td><td></td><td>UART7_RX</td><td></td><td></td><td></td><td></td><td>ETH_MII_TX_CLKETH_RGM1_RXD1</td><td></td><td></td><td>OPA4_CH1N</td><td></td></tr><tr><td>PC4</td><td>ADC_IN14</td><td>TIM9_CH4</td><td></td><td>USART1_CTS_3UART8_TX</td><td></td><td></td><td></td><td></td><td>ETH_MII_RXD0ETH_RMI_TXD0ETH_RGM1_TXD1</td><td></td><td></td><td>OPA4_CH1P</td><td></td></tr><tr><td>PC5</td><td>ADC_IN15</td><td>TIM9_BKIN</td><td></td><td>USART1_RTS_3UART8_RX</td><td></td><td></td><td></td><td></td><td>ETH_MII_RXD1ETH_RMI_TXD1ETH_RGM1_TXD2</td><td></td><td></td><td>OPA3_CH1P</td><td></td></tr><tr><td>PC6</td><td></td><td>TIM8_CH1</td><td>TIM3_CH1_3</td><td></td><td></td><td></td><td></td><td>I2S2_MCK</td><td>ETH_RXP</td><td>SD10_D6</td><td></td><td></td><td></td></tr><tr><td>PC7</td><td></td><td>TIM8_CH2</td><td>TIM3_CH2_3</td><td></td><td></td><td></td><td></td><td>I2S3_MCK</td><td>ETH_RXN</td><td>SD10_D7</td><td></td><td></td><td></td></tr><tr><td>PC8</td><td></td><td>TIM8_CH3</td><td>TIM3_CH3_3</td><td></td><td></td><td></td><td></td><td></td><td>ETH_TXP</td><td>SD10_D0</td><td>DVP_D2</td><td></td><td></td></tr><tr><td>PC9</td><td></td><td>TIM8_CH4</td><td>TIM3_CH4_3</td><td></td><td></td><td></td><td></td><td></td><td>ETH_TXN</td><td>SD10_D1</td><td>DVP_D3</td><td></td><td></td></tr><tr><td>PC10</td><td></td><td>TIM10_ETR</td><td></td><td>USART3_TX_1UART4_TX</td><td></td><td></td><td></td><td>SP13_SCK_112S3_CK_1</td><td></td><td>SD10_D2</td><td>DVP_D8</td><td></td><td></td></tr><tr><td>PC11</td><td></td><td>TIM10_CH4</td><td></td><td>USART3_RX_1UART4_RX</td><td></td><td></td><td></td><td>SP13_MISO_112S3_SD_1</td><td></td><td>SD10_D3</td><td>DVP_D4</td><td></td><td></td></tr><tr><td>PC12</td><td></td><td>TIM10_BKIN</td><td></td><td>USART3_CK_1UART5_TX</td><td></td><td></td><td></td><td>SP13_MOSI_112S3_SD_1</td><td></td><td>SD10_CK</td><td>DVP_D9</td><td></td><td></td></tr><tr><td>PC13</td><td></td><td>TIM8_CH4_1</td><td></td><td></td><td></td><td>TAMPER-RTC</td><td></td><td></td><td></td><td></td><td></td><td></td><td></td></tr><tr><td>PC14</td><td></td><td>TIM9_CH4_1</td><td></td><td></td><td></td><td>OSC32_IN</td><td></td><td></td><td></td><td></td><td></td><td></td><td></td></tr><tr><td>PC15</td><td></td><td>TIM10_CH4_1</td><td></td><td></td><td></td><td>OSC32_OUT</td><td></td><td></td><td></td><td></td><td></td><td></td><td></td></tr><tr><td>PD0</td><td></td><td>TIM10_ETR_2TIM10_ETR_3</td><td></td><td></td><td></td><td>OSC_IN</td><td></td><td></td><td></td><td>FSMC_D2</td><td></td><td></td><td>CAN1_RX_3</td></tr><tr><td>PD1</td><td></td><td>TIM10_CH1_2TIM10_CH1_3</td><td></td><td></td><td></td><td>OSC_OUT</td><td></td><td></td><td></td><td>FSMC_D3</td><td></td><td></td><td>CAN1_TX_3</td></tr><tr><td>PD2</td><td></td><td></td><td>TIM3_ETRTIM3_ETR_2TIM3_ETR_3</td><td>UART5_RX</td><td></td><td></td><td></td><td></td><td></td><td>SD10_CMDFSMC_NADV</td><td>DVP_D11</td><td></td><td></td></tr><tr><td>复用引脚</td><td>ADC DAC</td><td>TIM1 8/9/10</td><td>TIM2 3/4/5</td><td>UARTUSART</td><td>USB</td><td>SYS</td><td>I2C</td><td>SPI I2S</td><td>ETH</td><td>FSMC SD10</td><td>DVP</td><td>OPA</td><td>CAN</td></tr><tr><td>PD3</td><td></td><td>TIM10_CH2_2TIM10_CH2_3</td><td></td><td>USART2_CTS_1</td><td></td><td></td><td></td><td></td><td></td><td>FSMC_CLK</td><td></td><td></td><td></td></tr><tr><td>PD4</td><td></td><td></td><td></td><td>USART2_RTS_1</td><td></td><td></td><td></td><td></td><td></td><td>FSMC_NOE</td><td></td><td></td><td></td></tr><tr><td>PD5</td><td></td><td>TIM10_CH3_2TIM10_CH3_3</td><td></td><td>USART2_TX_1</td><td></td><td></td><td></td><td></td><td></td><td>FSMC_NWE</td><td></td><td></td><td></td></tr><tr><td>PD6</td><td></td><td></td><td></td><td>USART2_RX_1</td><td></td><td></td><td></td><td></td><td></td><td>FSMC_NWAIT</td><td>DVP_D10</td><td></td><td></td></tr><tr><td>PD7</td><td></td><td>TIM10_CH4_2TIM10_CH4_3</td><td></td><td>USART2_CK_1</td><td></td><td></td><td></td><td></td><td></td><td>FSMC_NE1FSMC_NCE2</td><td></td><td></td><td></td></tr><tr><td>PD8</td><td></td><td>TIM9_CH1N_2TIM9_CH1N_3</td><td></td><td>USART3_TX_3</td><td></td><td></td><td></td><td></td><td>ETH_MII_RX_DV_1ETH_RMII_CRS_DV_1</td><td>FSMC_D13</td><td></td><td></td><td></td></tr><tr><td>PD9</td><td></td><td>TIM9_CH1_2TIM9_ETR_2TIM9_ETR_3TIM9_ETR_3</td><td></td><td>USART3_RX_3</td><td></td><td></td><td></td><td></td><td>ETH_MII_RXDO_1ETH_RMII_RXDO_1</td><td>FSMC_D14</td><td></td><td></td><td></td></tr><tr><td>PD10</td><td></td><td>TIM9_CH2N_2TIM9_CH2N_3</td><td></td><td>USART3_CK_3USART3_CK_2</td><td></td><td></td><td></td><td></td><td>ETH_MII_RXD1_1ETH_RMII_RXD1_1</td><td>FSMC_D15</td><td></td><td></td><td></td></tr><tr><td>PD11</td><td></td><td>TIM9_CH2_2TIM9_CH2_3</td><td></td><td>USART3_CTS_3USART3_CTS_2</td><td></td><td></td><td></td><td></td><td>ETH_MII_RXD2_1</td><td>FSMC_A16</td><td></td><td></td><td></td></tr><tr><td>PD12</td><td></td><td>TIM9_CH3N_2TIM9_CH3N_3</td><td>TIM4_CH1_1</td><td>USART3_RTS_3USART3_RTS_2</td><td></td><td></td><td></td><td></td><td>ETH_MII_RXD3</td><td>FSMC_A17</td><td></td><td></td><td></td></tr><tr><td>PD13</td><td></td><td>TIM9_CH3_2TIM9_CH3_3</td><td>TIM4_CH2_1</td><td></td><td></td><td></td><td></td><td></td><td></td><td>FSMC_A18</td><td></td><td></td><td></td></tr><tr><td>PD14</td><td></td><td>TIM9_BKIN_2TIM9_BKIN_3</td><td>TIM4_CH3_1</td><td></td><td></td><td></td><td></td><td></td><td></td><td>FSMC_D0</td><td></td><td></td><td></td></tr><tr><td>PD15</td><td></td><td>TIM9_CH4_2TIM9_CH4_3</td><td>TIM4_CH4_1</td><td></td><td></td><td></td><td></td><td></td><td></td><td>FSMC_D1</td><td></td><td></td><td></td></tr><tr><td>PE0</td><td></td><td></td><td>TIM4_ETR TIM4_ETR_1</td><td>UART4_TX_2UART4_TX_3</td><td></td><td></td><td></td><td></td><td></td><td>FSMC_NBL0</td><td></td><td></td><td></td></tr><tr><td>PE1</td><td></td><td></td><td></td><td>UART4_RX_2UART4_RX_3</td><td></td><td></td><td></td><td></td><td></td><td>FSMC_NBL1</td><td></td><td></td><td></td></tr><tr><td>PE2</td><td></td><td>TIM10_BKIN_2TIM10_BKIN_3</td><td></td><td></td><td></td><td></td><td></td><td></td><td></td><td>FSMC_A23</td><td></td><td></td><td></td></tr><tr><td>PE3</td><td></td><td>TIM10_CH1N_2TIM10_CH1N_3</td><td></td><td></td><td></td><td></td><td></td><td></td><td></td><td>FSMC_A19</td><td></td><td></td><td></td></tr><tr><td>PE4</td><td></td><td>TIM10_CH2N_2TIM10_CH2N_3</td><td></td><td></td><td></td><td></td><td></td><td></td><td></td><td>FSMC_A20</td><td></td><td></td><td></td></tr><tr><td>PE5</td><td></td><td>TIM10_CH3N_2TIM10_CH3N_3</td><td></td><td></td><td></td><td></td><td></td><td></td><td></td><td>FSMC_A21</td><td></td><td></td><td></td></tr><tr><td>PE6</td><td></td><td></td><td></td><td></td><td></td><td></td><td></td><td></td><td></td><td>FSMC_A22</td><td></td><td></td><td></td></tr><tr><td>PE7</td><td></td><td>TIM1_ETR_3</td><td></td><td></td><td></td><td></td><td></td><td></td><td></td><td>FSMC_D4</td><td></td><td>OPA3_OUT1</td><td></td></tr><tr><td>PE8</td><td></td><td>TIM1_CHIN_3</td><td></td><td>UART5_TX_2UART5_TX_3</td><td></td><td></td><td></td><td></td><td></td><td>FSMC_D5</td><td></td><td>OPA4_OUT1</td><td></td></tr><tr><td>PE9</td><td></td><td>TIM1_CH1_3</td><td></td><td>UART5_RX_2UART5_RX_3</td><td></td><td></td><td></td><td></td><td></td><td>FSMC_D6</td><td></td><td></td><td></td></tr><tr><td>PE10</td><td></td><td>TIM1_CH2N_3</td><td></td><td>UART6_TX_2UART6_TX_3</td><td></td><td></td><td></td><td></td><td></td><td>FSMC_D7</td><td></td><td></td><td></td></tr><tr><td>PE11</td><td></td><td>TIM1_CH2_3</td><td></td><td>UART6_RX_2UART6_RX_3</td><td></td><td></td><td></td><td></td><td></td><td>FSMC_D8</td><td></td><td></td><td></td></tr><tr><td>PE12</td><td></td><td>TIM1_CH3N_3</td><td></td><td>UART7_TX_2UART7_TX_3</td><td></td><td></td><td></td><td></td><td></td><td>FSMC_D9</td><td></td><td></td><td></td></tr><tr><td>PE13</td><td></td><td>TIM1_CH3_3</td><td></td><td>UART7_RX_2UART7_RX_3</td><td></td><td></td><td></td><td></td><td></td><td>FSMC_D10</td><td></td><td></td><td></td></tr><tr><td>PE14</td><td></td><td>TIM1_CH4_3</td><td></td><td>UART8_TX_2UART8_TX_3</td><td></td><td></td><td></td><td></td><td></td><td>FSMC_D11</td><td></td><td>OPA2_OUT1</td><td></td></tr><tr><td>PE15</td><td></td><td>TIM1_BKIN_3</td><td></td><td>UART8_RX_2UART8_RX_3</td><td></td><td></td><td></td><td></td><td></td><td>FSMC_D12</td><td></td><td>OPA1_OUT1</td><td></td></tr></table>

# 第 4 章 电气特性

# 4.1 测试条件

除非特殊说明和标注，所有电压都以 $\mathsf { V } _ { \mathbb S }$ 为基准。

所有最小值和最大值将在最坏的环境温度、供电电压和时钟频率条件下得到保证。

CH32V303/305/307 的典型数值是基于常温 $2 5 \mathrm { ^ \circ C }$ 和 $\mathsf { V } _ { \mathsf { D D } } = 3 . 3 \mathsf { V }$ 环境下用于设计指导。

CH32V317 的典型数值是基于常温 $2 5 \mathrm { ^ \circ C }$ 、 $\mathsf { V } _ { \mathsf { D D } } = 3 . 3 \mathsf { V }$ 、 $V _ { \tt D D \_ E T H } = 3 . 3 V$ 环境下用于设计指导。

对于通过综合评估、设计模拟或工艺特性得到的数据，不会在生产线进行测试。在综合评估的基础上，最小和最大值是通过样本测试后统计得到。除非特殊说明为实测值，否则特性参数以综合评估或设计保证。

供电方案：

![](images/c335e2a3d6ba925ad963c2ca2c5f71de1c498d7dcfa6945da97d3fcbb94d0070.jpg)  
图 4-1-1 CH32V303/305/307 常规供电典型电路

![](images/0b145afc7321a86374de5caa304a349d47806abb04d3ae8a5ddb7a60b48ce788.jpg)  
图 4-1-2 CH32V317 常规供电典型电路

# 4.2 绝对最大值

临界或者超过绝对最大值将可能导致芯片工作不正常甚至损坏。

表4-1 绝对最大值参数表  

<table><tr><td>符号</td><td colspan="2">描述</td><td>最小值</td><td>最大值</td><td>单位</td></tr><tr><td rowspan="2">TA</td><td rowspan="2">工作时的环境温度</td><td>除CH32V303RCT7</td><td>-40</td><td>85</td><td>°C</td></tr><tr><td>CH32V303RCT7</td><td>-40</td><td>105</td><td>°C</td></tr><tr><td>TS</td><td colspan="2">存储时的环境温度</td><td>-40</td><td>125</td><td>°C</td></tr><tr><td>VDD-VSS</td><td colspan="2">外部主供电电压（包含VDDA和VDD）</td><td>-0.3</td><td>4.0</td><td>V</td></tr><tr><td>V10-VSS</td><td colspan="2">10供电电压</td><td>-0.3</td><td>4.0</td><td>V</td></tr><tr><td>VDD ETH-VSS</td><td>内部10/100M以太网PHY供电电压</td><td>CH32V317</td><td>-0.3</td><td>4.0</td><td>V</td></tr><tr><td>VDDK</td><td>内部电源LDO退耦端的电压</td><td>CH32V317</td><td>-0.2</td><td>1.5</td><td>V</td></tr><tr><td rowspan="4">VIN</td><td colspan="2">FT(耐受5V)引脚上的输入电压</td><td>Vss-0.3</td><td>5.5</td><td>V</td></tr><tr><td>10/100M以太网PHY差分引脚</td><td>CH32V317</td><td>Vss-0.3</td><td>VDD ETH+0.3</td><td>V</td></tr><tr><td colspan="2">USB和10M以太网PHY引脚上的输入电压</td><td>Vss-0.3</td><td>VDD+0.3</td><td>V</td></tr><tr><td colspan="2">其他引脚上的输入电压</td><td>Vss-0.3</td><td>V10+0.3</td><td>V</td></tr><tr><td>|ΔVDDx|</td><td colspan="2">不同主供电引脚之间的电压差</td><td></td><td>50</td><td>mV</td></tr><tr><td>|ΔV10x|</td><td colspan="2">不同10端供电引脚之间的电压差</td><td></td><td>50</td><td>mV</td></tr><tr><td>|ΔVSSx|</td><td colspan="2">不同接地引脚之间的电压差</td><td></td><td>50</td><td>mV</td></tr><tr><td rowspan="2">VESD(HBM)</td><td colspan="2">ESD静电放电电压(HBM人体模型,非接触式)</td><td colspan="2">4K</td><td>V</td></tr><tr><td colspan="2">USB引脚(PA11、PA12)</td><td colspan="2">3K</td><td>V</td></tr><tr><td>IVDD</td><td colspan="2">经过VDD/VDDA/V10电源线的总电流(供应电流)</td><td></td><td>150</td><td rowspan="8">mA</td></tr><tr><td>IVss</td><td colspan="2">经过Vss地线的总电流(流出电流)</td><td></td><td>150</td></tr><tr><td rowspan="2">I10</td><td colspan="2">任意I/0和控制引脚上的灌电流</td><td></td><td>25</td></tr><tr><td colspan="2">任意I/0和控制引脚上的源电流</td><td></td><td>-25</td></tr><tr><td rowspan="3">INJ(PIN)</td><td colspan="2">NRST引脚注入电流</td><td></td><td>+/-5</td></tr><tr><td colspan="2">HSE的OSC_IN引脚和LSE的OSC_IN引脚注入电流</td><td></td><td>+/-5</td></tr><tr><td colspan="2">其他引脚的注入电流</td><td></td><td>+/-5</td></tr><tr><td>ΣINJ(PIN)</td><td colspan="2">所有10和控制引脚的总注入电流</td><td></td><td>+/-25</td></tr></table>

# 4.3 电气参数

# 4.3.1 工作条件

表4-2 通用工作条件  

<table><tr><td>符号</td><td colspan="3">参数</td><td>条件</td><td>最小值</td><td>最大值</td><td>单位</td></tr><tr><td>\(F_{HCLK}\)</td><td colspan="3">内部 HB 时钟频率</td><td></td><td></td><td>144</td><td>MHz</td></tr><tr><td>\(F_{PCLK1}\)</td><td colspan="3">内部 PB1 时钟频率</td><td></td><td></td><td>144</td><td>MHz</td></tr><tr><td>\(F_{PCLK2}\)</td><td colspan="3">内部 PB2 时钟频率</td><td></td><td></td><td>144</td><td>MHz</td></tr><tr><td rowspan="2">\(V_{DD}\)</td><td rowspan="2" colspan="3">标准工作电压</td><td>未使用 USB 或 ETH</td><td>2.4(2)</td><td>3.6</td><td rowspan="2">V</td></tr><tr><td>使用 USB 或 ETH</td><td>3.0</td><td>3.6</td></tr><tr><td>\(V_{10}\)</td><td colspan="3">大部分 I0 引脚输出电压</td><td>\(V_{10}\) 不能高于 \(V_{DD}\)</td><td>2.4(2)</td><td>3.6</td><td>V</td></tr><tr><td rowspan="2">\(V_{DDA}\)</td><td colspan="3">模拟部分工作电压(未使用 ADC)</td><td>\(V_{DDA}\) 必须与 \(V_{10}\) 相同,\(V_{REF+}\)</td><td>2.4(2)</td><td>3.6</td><td rowspan="2">V</td></tr><tr><td colspan="3">模拟部分工作电压(使用 ADC)</td><td>不能高于 \(V_{DDA}\), \(V_{REF-}\)等于 \(V_{SS}\)</td><td>2.4</td><td>3.6</td></tr><tr><td>\(V_{DD\_ETH}\)</td><td colspan="2">内部 10/100M 以太网 PHY 电源电压</td><td>CH32V317</td><td></td><td>3.15</td><td>3.45</td><td>V</td></tr><tr><td>\(V_{BAT}^{(1)}\)</td><td colspan="3">备份单元工作电压</td><td></td><td>1.8</td><td>3.6</td><td>V</td></tr><tr><td rowspan="2">\(T_A\)</td><td rowspan="2">环境温度</td><td colspan="2">除 CH32V303RCT7</td><td></td><td>-40</td><td>85</td><td>°C</td></tr><tr><td colspan="2">CH32V303RCT7</td><td></td><td>-40</td><td>105</td><td>°C</td></tr><tr><td rowspan="2">\(T_J\)</td><td rowspan="2">结温度范围</td><td colspan="2">除 CH32V303RCT7</td><td></td><td>-40</td><td>105</td><td>°C</td></tr><tr><td colspan="2">CH32V303RCT7</td><td></td><td>-40</td><td>115</td><td>°C</td></tr></table>

注：1.电池到 $V _ { B A T }$ 连线要尽可能的短。  
2.对于位 $\begin{array} { l } { { \forall L E V E L ~ = ~ 1 } } \end{array}$ 的芯片， $V _ { D D }$ $V _ { I O }$ $V _ { D D A }$ 支持最低供电电压为2.4V；对于位 ${ \cal V } { \cal L } E { \cal V } E L = 0$ 的芯片， $V _ { D D }$ $V _ { D D A }$ 支持最低供电电压为1.8V， $V _ { I O }$ 支持最低供电电压1.2V（仅对于批号倒数第五位大于4的产品，无独立供电 $V _ { I O }$ 的产品和CH32V305GBU6 除外）或1.8V；使用ADC，DAC与OPA外设时，VDD、Vio、$V _ { D D A }$ 支持最低供电压仍为 $2 . 4 V _ { o }$

表4-3 上电和掉电条件  

<table><tr><td>符号</td><td>参数</td><td>条件</td><td>最小值</td><td>最大值</td><td>单位</td></tr><tr><td rowspan="2">tVDD</td><td>VDD上升速率</td><td></td><td>0</td><td>∞</td><td rowspan="2">us/V</td></tr><tr><td>VDD下降速率</td><td></td><td>20</td><td>∞</td></tr><tr><td rowspan="2">tVDDA</td><td>VDDA上升速率</td><td rowspan="2">VDD有电</td><td>0</td><td>10000</td><td rowspan="2">us/V</td></tr><tr><td>VDDA下降速率</td><td>20</td><td>∞</td></tr></table>

# 4.3.2 内置复位和电源控制模块特性

表 4-4-1 复位及电压监测（适用于位 VLEVEL = 1 的芯片）  

<table><tr><td>符号</td><td>参数</td><td>条件</td><td>最小值</td><td>典型值</td><td>最大值</td><td>单位</td></tr><tr><td rowspan="16">VPVD(1)</td><td rowspan="16">可编程电压检测器的电平选择(2)</td><td>PLS[2:0] = 000(上升沿)</td><td></td><td>2.39</td><td></td><td>V</td></tr><tr><td>PLS[2:0] = 000(下降沿)</td><td></td><td>2.31</td><td></td><td>V</td></tr><tr><td>PLS[2:0] = 001(上升沿)</td><td></td><td>2.56</td><td></td><td>V</td></tr><tr><td>PLS[2:0] = 001(下降沿)</td><td></td><td>2.48</td><td></td><td>V</td></tr><tr><td>PLS[2:0] = 010(上升沿)</td><td></td><td>2.65</td><td></td><td>V</td></tr><tr><td>PLS[2:0] = 010(下降沿)</td><td></td><td>2.57</td><td></td><td>V</td></tr><tr><td>PLS[2:0] = 011(上升沿)</td><td></td><td>2.78</td><td></td><td>V</td></tr><tr><td>PLS[2:0] = 011(下降沿)</td><td></td><td>2.69</td><td></td><td>V</td></tr><tr><td>PLS[2:0] = 100(上升沿)</td><td></td><td>2.89</td><td></td><td>V</td></tr><tr><td>PLS[2:0] = 100(下降沿)</td><td></td><td>2.81</td><td></td><td>V</td></tr><tr><td>PLS[2:0] = 101(上升沿)</td><td></td><td>3.05</td><td></td><td>V</td></tr><tr><td>PLS[2:0] = 101(下降沿)</td><td></td><td>2.96</td><td></td><td>V</td></tr><tr><td>PLS[2:0] = 110(上升沿)</td><td></td><td>3.17</td><td></td><td>V</td></tr><tr><td>PLS[2:0] = 110(下降沿)</td><td></td><td>3.08</td><td></td><td>V</td></tr><tr><td>PLS[2:0] = 111(上升沿)</td><td></td><td>3.31</td><td></td><td>V</td></tr><tr><td>PLS[2:0] = 111(下降沿)</td><td></td><td>3.21</td><td></td><td>V</td></tr><tr><td>VPVDhyst</td><td>PVD迟滞</td><td></td><td></td><td>0.08</td><td></td><td>V</td></tr><tr><td rowspan="2">VPOR/PDR</td><td rowspan="2">上电/掉电复位阈值</td><td>上升沿</td><td></td><td>2.2</td><td></td><td>V</td></tr><tr><td>下降沿</td><td></td><td>2.2</td><td></td><td>V</td></tr><tr><td>VPDRhyst</td><td>PDR迟滞</td><td></td><td></td><td>20</td><td></td><td>mV</td></tr><tr><td rowspan="2">tRSTTEMP</td><td>上电复位</td><td></td><td>16</td><td>28</td><td>30</td><td rowspan="2">mS</td></tr><tr><td>其他复位</td><td></td><td>2</td><td>10</td><td>30</td></tr></table>

注：1.常温测试值。  
2.对于CH32V317芯片，建议PLS[2:0]选择配置为110或111。

表4-4-2 复位及电压监测（适用于位VLEVEL = 0的芯片）  

<table><tr><td>符号</td><td>参数</td><td>条件</td><td>最小值</td><td>典型值</td><td>最大值</td><td>单位</td></tr><tr><td rowspan="5">VPVD(1)</td><td rowspan="5">可编程电压检测器的电平选择</td><td>PLS[2:0] = 000(上升沿)</td><td></td><td>2.19</td><td></td><td>V</td></tr><tr><td>PLS[2:0] = 000(下降沿)</td><td></td><td>2.13</td><td></td><td>V</td></tr><tr><td>PLS[2:0] = 001(上升沿)</td><td></td><td>2.33</td><td></td><td>V</td></tr><tr><td>PLS[2:0] = 001(下降沿)</td><td></td><td>2.25</td><td></td><td>V</td></tr><tr><td>PLS[2:0] = 010(上升沿)</td><td></td><td>2.39</td><td></td><td>V</td></tr><tr><td rowspan="11"></td><td rowspan="11"></td><td>PLS[2:0] = 010 (下降沿)</td><td></td><td>2.32</td><td></td><td>V</td></tr><tr><td>PLS[2:0] = 011 (上升沿)</td><td></td><td>2.48</td><td></td><td>V</td></tr><tr><td>PLS[2:0] = 011 (下降沿)</td><td></td><td>2.42</td><td></td><td>V</td></tr><tr><td>PLS[2:0] = 100 (上升沿)</td><td></td><td>2.57</td><td></td><td>V</td></tr><tr><td>PLS[2:0] = 100 (下降沿)</td><td></td><td>2.51</td><td></td><td>V</td></tr><tr><td>PLS[2:0] = 101 (上升沿)</td><td></td><td>2.69</td><td></td><td>V</td></tr><tr><td>PLS[2:0] = 101 (下降沿)</td><td></td><td>2.61</td><td></td><td>V</td></tr><tr><td>PLS[2:0] = 110 (上升沿)</td><td></td><td>2.78</td><td></td><td>V</td></tr><tr><td>PLS[2:0] = 110 (下降沿)</td><td></td><td>2.69</td><td></td><td>V</td></tr><tr><td>PLS[2:0] = 111 (上升沿)</td><td></td><td>2.88</td><td></td><td>V</td></tr><tr><td>PLS[2:0] = 111 (下降沿)</td><td></td><td>2.79</td><td></td><td>V</td></tr><tr><td>VPVDhyst</td><td>PVD 迟滞</td><td></td><td></td><td>0.08</td><td></td><td>V</td></tr><tr><td rowspan="2">VPOR/PDR</td><td rowspan="2">上电/掉电复位阈值</td><td>上升沿</td><td></td><td>1.59</td><td></td><td>V</td></tr><tr><td>下降沿</td><td></td><td>1.57</td><td></td><td>V</td></tr><tr><td>VPDRhyst</td><td>PDR 迟滞</td><td></td><td></td><td>20</td><td></td><td>mV</td></tr><tr><td rowspan="2">tRSTTEMP0</td><td>上电复位</td><td></td><td>16</td><td>28</td><td>30</td><td rowspan="2">mS</td></tr><tr><td>其他复位</td><td></td><td>2</td><td>10</td><td>30</td></tr></table>

注：1.常温测试值。

# 4.3.3 内置的参考电压

表4-5 内置参考电压  

<table><tr><td>符号</td><td>参数</td><td>条件</td><td>最小值</td><td>典型值</td><td>最大值</td><td>单位</td></tr><tr><td>VREFINT</td><td>内置参考电压</td><td>TA=-40°C~105°C</td><td>1.17</td><td>1.2</td><td>1.23</td><td>V</td></tr><tr><td>TS_vrefint</td><td>当读出内部参考电压时, ADC的采样时间</td><td></td><td></td><td></td><td>17.1</td><td>us</td></tr></table>

# 4.3.4 供电电流特性

电流消耗是多种参数和因素的综合指标，这些参数和因素包括工作电压、环境温度、I/O 引脚的负载、产品的软件配置、工作频率、I/O 脚的翻转速率、程序在存储器中的位置以及执行的代码等。电流消耗测量方法如下图：

![](images/ed2d1f89872b469bf3fd3da2866a3d26a22b922b6e9f3e6af3d8df1d1c92a997.jpg)  
图 4-2-1 CH32V303/305/307 电流消耗测量

![](images/db981db2f7c7bd127490de873d19524f423f8f77ee3ec434904971f028868920.jpg)  
图 4-2-2 CH32V317 电流消耗测量

CH32V303/305/307 处于下列条件：

常温 $\mathsf { V } _ { \mathsf { D } \mathsf { D } } = 3 . \ 3 \mathsf { V }$ 情况下，测试时：所有IO端口配置下拉输入，HSE或HSI只开1个， $H S E = 8 M$ ，HSI=8M（已校准）， $F _ { P L G K 1 } = F _ { H G L K } / 2$ ， $F _ { P l C K 2 } = F _ { H C L K }$ ，当 $F _ { { \sf H C L K } } > 8 M H z$ 时，PLL 打开。使能或关闭所有外设时钟的功耗。

CH32V317 处于下列条件：

常温 $\mathsf { V } _ { \mathsf { D D } } = 3 . 3 \mathsf { V }$ 、 $V _ { \tt D 0 \_ E T H } = 3 . 3 V$ 情况下，测试时：所有 IO 端口配置下拉输入，HSE 或 HSI 只开 1个， $H S E = 8 M$ ， $A S 1 = 8 M$ （已校准）， $F _ { P L G K 1 } = F _ { H G L K } / 2$ ， $F _ { P l C K 2 } = F _ { H C L K }$ ，当 $F _ { \mathsf { H C L K } } { > } 8 M H z$ 时，PLL 打开。使能或关闭所有外设时钟的功耗。

表 $_ { 4 - 6 }$ 运行模式下典型的电流消耗，数据处理代码从内部闪存中运行  

<table><tr><td rowspan="2">符号</td><td rowspan="2">参数</td><td colspan="2" rowspan="2">条件</td><td colspan="2">典型值</td><td rowspan="2">单位</td></tr><tr><td>使能所有外设</td><td>关闭所有外设</td></tr><tr><td rowspan="15">IDD(1)</td><td rowspan="15">运行模式下的供应电流</td><td rowspan="9">外部时钟</td><td>FHCLK=144MHz</td><td>22.4</td><td>12.4</td><td rowspan="15">mA</td></tr><tr><td>FHCLK=72MHz</td><td>11.5</td><td>6.5</td></tr><tr><td>FHCLK=48MHz</td><td>8.0</td><td>4.6</td></tr><tr><td>FHCLK=36MHz</td><td>6.4</td><td>3.8</td></tr><tr><td>FHCLK=24MHz</td><td>4.4</td><td>2.7</td></tr><tr><td>FHCLK=16MHz</td><td>3.5</td><td>2.3</td></tr><tr><td>FHCLK=8MHz</td><td>1.8</td><td>1.3</td></tr><tr><td>FHCLK=4MHz</td><td>1.3</td><td>1.0</td></tr><tr><td>FHCLK=500kHz</td><td>0.8</td><td>0.7</td></tr><tr><td rowspan="6">运行于高速内部RC振荡器(HSI),使用HB预分频以减低频率</td><td>FHCLK=144MHz</td><td>22.1</td><td>12.2</td></tr><tr><td>FHCLK=72MHz</td><td>11.3</td><td>6.3</td></tr><tr><td>FHCLK=48MHz</td><td>7.7</td><td>4.3</td></tr><tr><td>FHCLK=36MHz</td><td>5.8</td><td>3.3</td></tr><tr><td>FHCLK=24MHz</td><td>4.1</td><td>2.4</td></tr><tr><td>FHCLK=16MHz</td><td>3.0</td><td>1.8</td></tr><tr><td rowspan="3"></td><td rowspan="3"></td><td rowspan="3"></td><td>FHCLK=8MHz</td><td>1.5</td><td>1.0</td><td rowspan="3"></td></tr><tr><td>FHCLK=4MHz</td><td>1.0</td><td>0.7</td></tr><tr><td>FHCLK=500kHz</td><td>0.4</td><td>0.4</td></tr></table>

注：以上为实测参数。

表 4-7 睡眠模式下典型的电流消耗，数据处理代码从内部闪存或 SRAM 中运行  

<table><tr><td rowspan="2">符号</td><td rowspan="2">参数</td><td rowspan="2" colspan="2">条件</td><td colspan="2">典型值</td><td rowspan="2">单位</td></tr><tr><td>使能所有外设</td><td>关闭所有外设</td></tr><tr><td rowspan="18">IDD(1)</td><td rowspan="18">睡眠模式下的供应电流(此时外设供电和时钟保持)</td><td rowspan="9">外部时钟</td><td>FHCLK=144MHz</td><td>13.7</td><td>3.8</td><td rowspan="18">mA</td></tr><tr><td>FHCLK=72MHz</td><td>7.2</td><td>2.3</td></tr><tr><td>FHCLK=48MHz</td><td>5.1</td><td>1.8</td></tr><tr><td>FHCLK=36MHz</td><td>4.0</td><td>1.5</td></tr><tr><td>FHCLK=24MHz</td><td>2.9</td><td>1.3</td></tr><tr><td>FHCLK=16MHz</td><td>2.2</td><td>1.1</td></tr><tr><td>FHCLK=8MHz</td><td>1.4</td><td>0.8</td></tr><tr><td>FHCLK=4MHz</td><td>1.0</td><td>0.8</td></tr><tr><td>FHCLK=500kHz</td><td>0.7</td><td>0.7</td></tr><tr><td rowspan="9">运行于高速内部RC振荡器(HSI),使用HB预分频以减低频率</td><td>FHCLK=144MHz</td><td>13.4</td><td>3.5</td></tr><tr><td>FHCLK=72MHz</td><td>6.9</td><td>1.9</td></tr><tr><td>FHCLK=48MHz</td><td>4.7</td><td>1.4</td></tr><tr><td>FHCLK=36MHz</td><td>3.6</td><td>1.2</td></tr><tr><td>FHCLK=24MHz</td><td>2.6</td><td>0.9</td></tr><tr><td>FHCLK=16MHz</td><td>1.9</td><td>0.7</td></tr><tr><td>FHCLK=8MHz</td><td>1.0</td><td>0.5</td></tr><tr><td>FHCLK=4MHz</td><td>0.7</td><td>0.4</td></tr><tr><td>FHCLK=500kHz</td><td>0.4</td><td>0.3</td></tr></table>

注：以上为实测参数。

表4-8 停止和待机模式下典型的电流消耗  

<table><tr><td>符号</td><td>参数</td><td>条件</td><td>典型值</td><td>单位</td></tr><tr><td rowspan="6">IDD(1)</td><td rowspan="2">停止模式下的供应电流</td><td>调压器处于运行模式,低速和高速内部RC振荡器及外部振荡器都处于关闭状态(没有独立看门狗)</td><td>110</td><td rowspan="6">uA</td></tr><tr><td>调压器处于低功耗模式,低速和高速内部RC振荡器及外部振荡器都处于关闭状态(没有独立看门狗,PVD关闭),RAM进入低功耗模式</td><td>30</td></tr><tr><td rowspan="4">待机模式下的供应电流</td><td>低速内部RC振荡器和独立看门狗处于开启状态,所有RAM不带电</td><td>1.8</td></tr><tr><td>低速内部RC振荡器处于开启状态,独立看门狗关闭状态,所有RAM不带电</td><td>1.8</td></tr><tr><td>LSI/LSE/RTC/IWDG关闭,32K_RAM带电并处于低功耗状态</td><td>2.5</td></tr><tr><td>LSI/LSE/RTC/IWDG关闭,</td><td>1.2</td></tr><tr><td rowspan="2"></td><td rowspan="2"></td><td>2K_RAM带电并处于低功耗状态</td><td></td><td rowspan="3"></td></tr><tr><td>LSI/LSE/RTC/IWDG关闭,所有RAM不带电</td><td>1</td></tr><tr><td>IDDVBAT(1)</td><td>备份区域的供应电流(移除VDD和VDDA,只使用VBAT供电)</td><td>低速外部振荡器和RTC处于开启状态</td><td>1.8</td></tr></table>

注：以上为实测参数。

CH32V317 芯片内置 10/100Mbps 以太网 PHY 物理层收发器，该模块的电流消耗如下表 4-9 所示。

表 4-9 10/100Mbps 以太网 PHY 模块的电流消耗（仅针对 CH32V317 芯片）  

<table><tr><td>符号</td><td>参数</td><td>条件</td><td>典型值</td><td>单位</td></tr><tr><td rowspan="6">IDD_10/100M_PHY</td><td rowspan="2">传输状态下的供应电流</td><td>100BASE-TX 通路链接成功并且在收发通道上有数据包</td><td>60.4</td><td rowspan="6">mA</td></tr><tr><td>10BASE-TX 通路链接成功并且在收发通道上有数据包</td><td>34.2</td></tr><tr><td rowspan="2">空闲状态下的供应电流</td><td>100BASE-TX 通路链接成功并且在收发通道上无任何数据包</td><td>61.4</td></tr><tr><td>10BASE-TX 通路链接成功并且在收发通道上无任何数据包</td><td>28.1</td></tr><tr><td>断开状态下的供应电流</td><td>100BASE-TX 和 10BASE-TX 通路均未链接成功且 PHY 处于自动协商状态。</td><td>38.4</td></tr><tr><td>停机状态下的供应电流</td><td>仅 SMI 接口处于工作状态</td><td>0.2</td></tr></table>

# 4.3.5 外部时钟源特性

表4-10 来自外部高速时钟  

<table><tr><td>符号</td><td>参数</td><td>条件</td><td>最小值</td><td>典型值</td><td>最大值</td><td>单位</td></tr><tr><td>FHSE_ext</td><td>外部时钟频率</td><td></td><td>3</td><td>8</td><td>25</td><td>MHz</td></tr><tr><td>VHSEH (1)</td><td>OSC_IN 输入引脚高电平电压</td><td></td><td>0.8V10</td><td></td><td>V10</td><td>V</td></tr><tr><td>VHSEL (1)</td><td>OSC_IN 输入引脚低电平电压</td><td></td><td>0</td><td></td><td>0.2V10</td><td>V</td></tr><tr><td>Cin (HSE)</td><td>OSC_IN 输入电容</td><td></td><td></td><td>5</td><td></td><td>pF</td></tr><tr><td>DuCyHSE</td><td>占空比 (Duty cycle)</td><td></td><td></td><td>50</td><td></td><td>%</td></tr><tr><td>IL</td><td>OSC_IN 输入漏电流</td><td></td><td></td><td></td><td>±1</td><td>uA</td></tr></table>

注：不满足此条件可能会引起电平识别错误。

![](images/1a82a6e3d550d939348647eabff4b5536d1c3c7f8610d0e80cac6babead19c74.jpg)  
图4-3 外部提供高频时钟源电路

表4-11 来自外部低速时钟  

<table><tr><td>符号</td><td>参数</td><td>条件</td><td>最小值</td><td>典型值</td><td>最大值</td><td>单位</td></tr><tr><td>F LSE_ext</td><td>用户外部时钟频率</td><td></td><td></td><td>32.768</td><td>1000</td><td>kHz</td></tr><tr><td>V LSEH</td><td>OSC32_IN输入引脚高电平电压</td><td></td><td>0.8VDD</td><td></td><td>VDD</td><td>V</td></tr><tr><td>V LSEL</td><td>OSC32_IN输入引脚低电平电压</td><td></td><td>0</td><td></td><td>0.2VDD</td><td>V</td></tr><tr><td>C in (LSE)</td><td>OSC32_IN输入电容</td><td></td><td></td><td>5</td><td></td><td>pF</td></tr><tr><td>DuCyLSE</td><td>占空比(Duty cycle)</td><td></td><td></td><td>50</td><td></td><td>%</td></tr><tr><td>IL</td><td>OSC32_IN输入漏电流</td><td></td><td></td><td></td><td>±1</td><td>uA</td></tr></table>

![](images/818cca3d3b7da15d05d62971b9f6961e0c7c93206d2424787d78dd5fff800588.jpg)  
图4-4 外部提供低频时钟源电路

表4-12 使用一个晶体/陶瓷谐振器产生的高速外部时钟  

<table><tr><td>符号</td><td>参数</td><td>条件</td><td>最小值</td><td>典型值</td><td>最大值</td><td>单位</td></tr><tr><td>Fosc_IN</td><td>谐振器频率</td><td></td><td>3</td><td>8</td><td>25</td><td>MHz</td></tr><tr><td>RF</td><td>反馈电阻</td><td></td><td></td><td>250</td><td></td><td>kΩ</td></tr><tr><td>C</td><td>建议的负载电容与对应晶体串行阻抗Rs</td><td>Rs=60Ω(1)</td><td></td><td>20</td><td></td><td>pF</td></tr><tr><td>I2</td><td>HSE驱动电流</td><td></td><td></td><td>0.53</td><td></td><td>mA</td></tr><tr><td>gm</td><td>振荡器的跨导</td><td>启动</td><td></td><td>17</td><td></td><td>mA/V</td></tr><tr><td>tsu(HSE)</td><td>启动时间</td><td>VDD稳定,8M晶体</td><td></td><td>1.5(2)</td><td>4</td><td>ms</td></tr></table>

注：1.建议晶体 ESR不超过80欧姆，优先选择ESR较小的晶体，负载电容参考晶体手册要求。  
2.启动时间指从HSEON开启到HSERDY被置位的时间差。  
3.对于CH32V317芯片，晶振频率偏差建议在±40ppm以内。

电路参考设计及要求：

晶体的负载电容以晶体厂商建议为准，通常情况 $\mathtt { C } _ { 1 } \mathtt { \Pi } _ { 1 } = \mathtt { C } _ { 1 } \mathtt { \Pi } _ { 2 }$ 。

![](images/e70c142757d4eb78fdbb7826bfd46636d05102bbcb3a550123bb6c76bf54a441.jpg)  
图4-5 外接8M晶体典型电路

表4-13 使用一个晶体/陶瓷谐振器产生的低速外部时钟（fLSE=32.768kHz）  

<table><tr><td>符号</td><td>参数</td><td>条件</td><td>最小值</td><td>典型值</td><td>最大值</td><td>单位</td></tr><tr><td>RF</td><td>反馈电阻</td><td></td><td></td><td>5</td><td></td><td>MΩ</td></tr><tr><td>C</td><td>建议的负载电容与对应晶体串行阻抗 Rs</td><td>Rs&lt;70kΩ</td><td></td><td></td><td>15</td><td>pF</td></tr><tr><td>i2</td><td>LSE驱动电流</td><td>VDD = 3.3V</td><td></td><td>0.35</td><td></td><td>uA</td></tr><tr><td>gm</td><td>振荡器的跨导</td><td>启动</td><td></td><td>25.3</td><td></td><td>uA/V</td></tr><tr><td>tsu(LSE)</td><td>启动时间</td><td>VDD是稳定的</td><td></td><td>800</td><td></td><td>mS</td></tr></table>

电路参考设计及要求：

晶体的负载电容以晶体厂商建议为准，通常情况 $\mathtt { C } _ { 1 } \mathtt { \Pi } _ { 1 } = \mathtt { C } _ { 1 } \mathtt { \Pi } _ { 2 }$ ，可选 12pF 左右。

图 4-6 外接 32.768K 晶体典型电路  
![](images/3f80289ed3ca4de7f63d92dfd20149a6b2536b6a60f4d0913b6f23412225111e.jpg)  
注：负载电容 $\pmb { C } _ { L }$ 由下式计算： $G _ { L } = G _ { L 1 } \times G _ { L 2 } \times ( G _ { L 1 } + G _ { L 2 } ) + G _ { s t r a y }$ ，其中 $ { C _ { s t r a y } }$ 是引脚的电容和PCB板或PCB相关的电容，它的典型值是介于 $2 p F$ 至 $7 p F$ 之间。

# 4.3.6 内部时钟源特性

表4-14 内部高速（HSI）RC振荡器特性  

<table><tr><td>符号</td><td>参数</td><td>条件</td><td>最小值</td><td>典型值</td><td>最大值</td><td>单位</td></tr><tr><td>FHSI</td><td>频率(校准后)</td><td></td><td></td><td>8</td><td></td><td>MHz</td></tr><tr><td>DuCyHSI</td><td>占空比(Duty cycle)</td><td></td><td>45</td><td>50</td><td>55</td><td>%</td></tr><tr><td rowspan="2">ACCHSI</td><td rowspan="2">HSI振荡器的精度(校准后)</td><td>TA=0°C~70°C</td><td>-1.6</td><td></td><td>1.6</td><td>%</td></tr><tr><td>TA=表4-2所示工作环境温度范围</td><td>-2.2</td><td></td><td>2.2</td><td>%</td></tr><tr><td>tsu(HSI)</td><td>HSI振荡器启动稳定时间(1)</td><td></td><td></td><td></td><td>8</td><td>us</td></tr><tr><td>IDD(HSI)</td><td>HSI振荡器功耗</td><td></td><td>120</td><td>180</td><td>270</td><td>uA</td></tr></table>

注：1.寄存器RCC_CTLRHSION位置1，等待HSIRDY置1。

表4-15 内部低速（LSI）RC振荡器特性  

<table><tr><td>符号</td><td>参数</td><td>条件</td><td>最小值</td><td>典型值</td><td>最大值</td><td>单位</td></tr><tr><td>FLSI</td><td>频率</td><td></td><td>25</td><td>39</td><td>60</td><td>kHz</td></tr><tr><td>DuCyLSI</td><td>占空比(Duty cycle)</td><td></td><td>45</td><td>50</td><td>55</td><td>%</td></tr><tr><td rowspan="2">tsu(LSI)</td><td rowspan="2">LSI振荡器启动稳定时间(1)</td><td>LSE开启</td><td></td><td>230</td><td></td><td>us</td></tr><tr><td>LSE关闭</td><td></td><td>5</td><td></td><td>ms</td></tr><tr><td>IDD(LSI)</td><td>LSI振荡器功耗</td><td></td><td></td><td>0.6</td><td></td><td>uA</td></tr></table>

注：1.寄存器RCC_RSTSCKRLSION位置1，等待LSIRDY置1。

# 4.3.7 PLL 特性

表 4-16 PLL 特性  

<table><tr><td>符号</td><td>参数</td><td>条件</td><td>最小值</td><td>典型值</td><td>最大值</td><td>单位</td></tr><tr><td rowspan="2">\(F_{PLL\_IN}\)</td><td>PLL 输入时钟</td><td></td><td>3</td><td>8</td><td>25</td><td>MHz</td></tr><tr><td>PLL 输入时钟占空比</td><td></td><td>40</td><td></td><td>60</td><td>%</td></tr><tr><td>\(F_{PLL\_OUT}\)</td><td>PLL 倍频输出时钟</td><td></td><td>18</td><td></td><td>\(144^{(1)}\)</td><td>MHz</td></tr><tr><td>\(t_{LOCK}\)</td><td>PLL 锁定时间</td><td></td><td></td><td>80</td><td>200</td><td>us</td></tr></table>

注：须选择合适倍频，满足PLL输出频率范围。

表 4-17 PLL2 特性  

<table><tr><td>符号</td><td>参数</td><td>条件</td><td>最小值</td><td>典型值</td><td>最大值</td><td>单位</td></tr><tr><td rowspan="2">\(F_{PLL\_IN}\)</td><td>PLL 输入时钟</td><td></td><td>3</td><td></td><td>25</td><td>MHz</td></tr><tr><td>PLL 输入时钟占空比</td><td></td><td>40</td><td></td><td>60</td><td>%</td></tr><tr><td>\(F_{PLL\_OUT}\)</td><td>PLL 倍频输出时钟</td><td></td><td>30</td><td></td><td>\(75^{(1)}\)</td><td>MHz</td></tr><tr><td>\(F_{VCO}\)</td><td>VCO 输出时钟</td><td></td><td>60</td><td></td><td>150</td><td>MHz</td></tr><tr><td>\(t_{LOCK1}\)</td><td>PLL 锁定时间</td><td></td><td></td><td>80</td><td>200</td><td>us</td></tr></table>

注：1.须选择合适倍频，满足PLL输出频率范围。

表 4-18 PLL3 特性  

<table><tr><td>符号</td><td>参数</td><td>条件</td><td>最小值</td><td>典型值</td><td>最大值</td><td>单位</td></tr><tr><td rowspan="2">\(F_{PLL\_IN}\)</td><td>PLL 输入时钟</td><td></td><td>3</td><td></td><td>25</td><td>MHz</td></tr><tr><td>PLL 输入时钟占空比</td><td></td><td>40</td><td></td><td>60</td><td>%</td></tr><tr><td>\(F_{PLL\_OUT}\)</td><td>PLL 倍频输出时钟</td><td></td><td>30</td><td></td><td>\(100^{(1)}\)</td><td>MHz</td></tr><tr><td>\(F_{VCO}\)</td><td>VCO 输出时钟</td><td></td><td>60</td><td></td><td>200</td><td>MHz</td></tr><tr><td>\(t_{LOCK1}\)</td><td>PLL 锁定时间</td><td></td><td></td><td>80</td><td>200</td><td>us</td></tr></table>

注：1.须选择合适倍频，满足PLL输出频率范围。

# 4.3.8 从低功耗模式唤醒的时间

表4-19 低功耗模式唤醒的时间 （1）  

<table><tr><td>符号</td><td>参数</td><td>条件</td><td>典型值</td><td>单位</td></tr><tr><td>twsleep</td><td>从睡眠模式唤醒</td><td>使用HSI RC时钟唤醒</td><td>2.4</td><td>us</td></tr><tr><td rowspan="2">twstop</td><td>从停止模式唤醒(调压器处于运行模式)</td><td>HSI RC时钟唤醒</td><td>23.1</td><td>us</td></tr><tr><td>从停止模式唤醒(调压器为低功耗模式)</td><td>调压器从低功耗模式唤醒时间+HSI RC时钟唤醒</td><td>76.7</td><td>us</td></tr><tr><td>twustDBY</td><td>从待机模式唤醒</td><td>LDO稳定时间+HSI RC时钟唤醒+代码加载时间(2)(举例256K)</td><td>8.9</td><td>ms</td></tr></table>

注：1.以上为实测参数；  
2.代码加载时间以当前芯片配置0等待运行区域容量和加载配置时钟大小计算可得。

# 4.3.9 存储器特性

表4-20 闪存存储器特性  

<table><tr><td>符号</td><td>参数</td><td>条件</td><td>最小值</td><td>典型值</td><td>最大值</td><td>单位</td></tr><tr><td>\(F_{prog}\)</td><td>操作频率\(^{(1)}\)</td><td>\(T_A\)= -40°C~85°C</td><td></td><td></td><td>60</td><td>MHz</td></tr><tr><td>\(t_{prog\_page}\)</td><td>页(256字节)编程时间</td><td>\(T_A\)= -40°C~85°C</td><td></td><td>2</td><td>2.5</td><td>ms</td></tr><tr><td>\(t_{erase\_page}\)</td><td>页(256字节)擦除时间</td><td>\(T_A\)= -40°C~85°C</td><td></td><td>16</td><td>20</td><td>ms</td></tr><tr><td>\(t_{erase\_sec}\)</td><td>扇区(4K字节)擦除时间</td><td>\(T_A\)= -40°C~85°C</td><td></td><td>16</td><td>20</td><td>ms</td></tr><tr><td>\(V_{prog}\)</td><td>编程电压</td><td></td><td>2.4</td><td></td><td>3.6</td><td>V</td></tr></table>

注：flash的操作频率包括读、编程、擦除，时钟来自于HCLK。

表 4-21 闪存存储器寿命和数据保存期限  

<table><tr><td>符号</td><td>参数</td><td>条件</td><td>最小值</td><td>典型值</td><td>最大值</td><td>单位</td></tr><tr><td>NEND</td><td>擦写次数</td><td>TA=25°C</td><td>10K</td><td>80K(1)</td><td></td><td>次</td></tr><tr><td>tRET</td><td>数据保存期限</td><td></td><td>20</td><td></td><td></td><td>年</td></tr></table>

注：实测操作擦写次数，非担保。

# 4.3.10 I/O 端口特性

表 4-22 通用 I/O 静态特性  

<table><tr><td>符号</td><td>参数</td><td>条件</td><td>最小值</td><td>典型值</td><td>最大值</td><td>单位</td></tr><tr><td rowspan="2">\(V_{IH}\)</td><td>标准 I/0 脚,输入高电平电压</td><td></td><td>0.41*(V10-1.8)+1.3</td><td></td><td>V10+0.3</td><td>V</td></tr><tr><td>FT 10 引脚,输入高电平电压</td><td></td><td>0.42*(V10-1.8)+1</td><td></td><td>5.5</td><td>V</td></tr><tr><td rowspan="2">\(V_{IL}\)</td><td>标准 I/0 脚,输入低电平电压</td><td rowspan="2"></td><td>-0.3</td><td></td><td>0.28*(V10-1.8)+0.6</td><td>V</td></tr><tr><td>FT 10 引脚,输入低电平电压</td><td>-0.3</td><td></td><td>0.32*(V10-1.8)+0.55</td><td>V</td></tr><tr><td rowspan="2">\(V_{hys}\)</td><td>标准 I/0 脚施密特触发器电压迟滞</td><td rowspan="2"></td><td>150</td><td></td><td></td><td rowspan="2">mV</td></tr><tr><td>FT 10 引脚施密特触发器电压迟滞</td><td>90</td><td></td><td></td></tr><tr><td rowspan="2">\(I_{1kg}\)</td><td rowspan="2">输入漏电流</td><td>标准 I0 端口</td><td></td><td></td><td>1</td><td rowspan="2">uA</td></tr><tr><td>FT 10 端口</td><td></td><td></td><td>3</td></tr><tr><td>\(R_{PU}\)</td><td>上拉等效电阻</td><td></td><td>30</td><td>40</td><td>50</td><td>kΩ</td></tr><tr><td>\(R_{PD}\)</td><td>下拉等效电阻</td><td></td><td>30</td><td>40</td><td>50</td><td>kΩ</td></tr><tr><td>\(C_{10}\)</td><td>I/0 引脚电容</td><td></td><td></td><td>5</td><td></td><td>pF</td></tr></table>

# 输出驱动电流特性

GPIO（通用输入/输出端口）可以吸收或输出多达±8mA电流，并且吸收或输出±20mA电流（不严格达到 $V _ { \mathfrak { a } } / \mathsf { V } _ { \mathfrak { d } \sharp } ,$ ）。在用户应用中，所有IO引脚驱动总电流不能超过4.2节给出的绝对最大额定值：

表4-23 输出电压特性  

<table><tr><td>符号</td><td>参数</td><td>条件</td><td>最小值</td><td>最大值</td><td>单位</td></tr><tr><td>\(V_{OL}\)</td><td>输出低电平,8个引脚灌电流</td><td rowspan="2">TTL端口,\(I_{10}\)=8mA2.7V\(V_{DD}\)&lt;3.6V</td><td></td><td>0.4</td><td rowspan="2">V</td></tr><tr><td>\(V_{OH}\)</td><td>输出高电平,8个引脚源电流</td><td>\(V_{DD}-0.4\)</td><td></td></tr><tr><td>\(V_{OL}\)</td><td>输出低电平,8个引脚灌电流</td><td rowspan="2">CMOS端口,\(I_{10}\)=8mA2.7V\(V_{DD}\)&lt;3.6V</td><td></td><td>0.4</td><td rowspan="2">V</td></tr><tr><td>\(V_{OH}\)</td><td>输出高电平,8个引脚源电流</td><td>\(V_{DD}-0.4\)</td><td></td></tr><tr><td>\(V_{OL}\)</td><td>输出低电平,8个引脚灌电流</td><td rowspan="2">\(I_{10}\)=20mA2.7V\(V_{DD}\)&lt;3.6V</td><td></td><td>1.0</td><td rowspan="2">V</td></tr><tr><td>\(V_{OH}\)</td><td>输出高电平,8个引脚源电流</td><td>\(V_{DD}-1.2\)</td><td></td></tr><tr><td>\(V_{OL}\)</td><td>输出低电平,8个引脚灌电流</td><td rowspan="2">\(I_{10}\)=6mA2.4V\(V_{DD}\)&lt;2.7V</td><td></td><td>0.4</td><td rowspan="2">V</td></tr><tr><td>\(V_{OH}\)</td><td>输出高电平,8个引脚源电流</td><td>\(V_{DD}-0.6\)</td><td></td></tr></table>

注：以上条件中如果多个10引脚同时驱动，电流总和不能超过表4.2节给出的绝对最大额定值。另外多个10引脚同时驱动时，电源/地线点上的电流很大，会导致压降使内部I0的电压达不到表中电源电压，从而导致驱动电流小于标称值。

表4-24 输入输出交流特性  

<table><tr><td>MODEx[1:0]配置</td><td>符号</td><td>参数</td><td>条件</td><td>最小值</td><td>最大值</td><td>单位</td></tr><tr><td rowspan="3">10(2MHz)</td><td>Fmax(10)out</td><td>最大频率</td><td>CL=50pF, VDD=2.7-3.6V</td><td></td><td>2</td><td>MHz</td></tr><tr><td>tf(10)out</td><td>输出高至低电平的下降时间</td><td rowspan="2">CL=50pF, VDD=2.7-3.6V</td><td></td><td>125</td><td>ns</td></tr><tr><td>tr(10)out</td><td>输出低至高电平的上升时间</td><td></td><td>125</td><td>ns</td></tr><tr><td rowspan="3">01(10MHz)</td><td>Fmax(10)out</td><td>最大频率</td><td>CL=50pF, VDD=2.7-3.6V</td><td></td><td>10</td><td>MHz</td></tr><tr><td>tf(10)out</td><td>输出高至低电平的下降时间</td><td rowspan="2">CL=50pF, VDD=2.7-3.6V</td><td></td><td>25</td><td>ns</td></tr><tr><td>tr(10)out</td><td>输出低至高电平的上升时间</td><td></td><td>25</td><td>ns</td></tr><tr><td rowspan="6">11(50MHz)</td><td rowspan="2">Fmax(10)out</td><td rowspan="2">最大频率</td><td>CL=30pF, VDD=2.7-3.6V</td><td></td><td>50</td><td>MHz</td></tr><tr><td>CL=50pF, VDD=2.7-3.6V</td><td></td><td>30</td><td>MHz</td></tr><tr><td rowspan="2">tf(10)out</td><td rowspan="2">输出高至低电平的下降时间</td><td>CL=30pF, VDD=2.7-3.6V</td><td></td><td>5</td><td>ns</td></tr><tr><td>CL=50pF, VDD=2.7-3.6V</td><td></td><td>8</td><td>ns</td></tr><tr><td rowspan="2">tr(10)out</td><td rowspan="2">输出低至高电平的上升时间</td><td>CL=30pF, VDD=2.7-3.6V</td><td></td><td>5</td><td>ns</td></tr><tr><td>CL=50pF, VDD=2.7-3.6V</td><td></td><td>8</td><td>ns</td></tr><tr><td></td><td>tEXT1pw</td><td>EXT1 控制器检测到外部信号的脉冲宽度</td><td></td><td>10</td><td></td><td>ns</td></tr></table>

# 4.3.11 NRST 引脚特性

表4-25 外部复位引脚特性  

<table><tr><td>符号</td><td>参数</td><td>条件</td><td>最小值</td><td>典型值</td><td>最大值</td><td>单位</td></tr><tr><td>VIL (NRST)</td><td>NRST输入低电平电压</td><td></td><td>-0.3</td><td></td><td>0.28*(VDD-1.8)+0.6</td><td>V</td></tr><tr><td>VIH (NRST)</td><td>NRST输入高电平电压</td><td></td><td>0.41*(VDD-1.8)+1.3</td><td></td><td>VDD+0.3</td><td>V</td></tr><tr><td>Vhys (NRST)</td><td>NRST施密特触发器电压迟滞</td><td></td><td>150</td><td></td><td></td><td>mV</td></tr><tr><td>RPU (1)</td><td>上拉等效电阻</td><td></td><td>30</td><td>40</td><td>50</td><td>kΩ</td></tr><tr><td>VF (NRST)</td><td>NRST输入可被滤波脉宽</td><td></td><td></td><td></td><td>100</td><td>ns</td></tr><tr><td>VNF (NRST)</td><td>NRST输入无法滤波脉宽</td><td></td><td>300</td><td></td><td></td><td>ns</td></tr></table>

注：上拉电阻是一个真正的电阻串联一个可开关的PMOS实现。这个PMOS开关的电阻很小（约占 $10 \%$ ）。

电路参考设计及要求：

![](images/7fd494c9a86306839506ec216a1f3f6e98916852b717b7cc2543409a70622039.jpg)  
图4-7 外部复位引脚典型电路

# 4.3.12 TIM 定时器特性

表 4-26 TIMx 特性  

<table><tr><td>符号</td><td>参数</td><td>条件</td><td>最小值</td><td>最大值</td><td>单位</td></tr><tr><td>tres (TIM)</td><td>定时器基准时钟</td><td></td><td>1</td><td></td><td>tTIMxCLK</td></tr><tr><td></td><td></td><td>fTIMxCLK=72MHz</td><td>13.9</td><td></td><td>ns</td></tr><tr><td rowspan="2">FEXT</td><td rowspan="2">CH1至CH4的定时器外部时钟频率</td><td></td><td>0</td><td>fTIMxCLK/2</td><td>MHz</td></tr><tr><td>fTIMxCLK=72MHz</td><td>0</td><td>36</td><td>MHz</td></tr><tr><td>ResTIM</td><td>定时器分辨率</td><td></td><td></td><td>16</td><td>位</td></tr><tr><td rowspan="2">tcounter</td><td rowspan="2">当选择了内部时钟时,16位计数器时钟周期</td><td></td><td>1</td><td>65536</td><td>tTIMxCLK</td></tr><tr><td>fTIMxCLK=72MHz</td><td>0.0139</td><td>910</td><td>us</td></tr><tr><td rowspan="2">tMAX_COUNT</td><td rowspan="2">最大可能的计数</td><td></td><td></td><td>65536</td><td>tTIMxCLK</td></tr><tr><td>fTIMxCLK=72MHz</td><td></td><td>59.6</td><td>s</td></tr></table>

# 4.3.13 I2C 接口特性

![](images/a47e61ba39e14821ccb5b43e0ac850fb8aaac48c9e8c9dee74ca3d5420768743.jpg)  
图 4-8 I2C 总线时序图

表 4-27 I2C 接口特性  

<table><tr><td rowspan="2">符号</td><td rowspan="2">参数</td><td colspan="2">标准 I2C</td><td colspan="2">快速 I2C</td><td rowspan="2">单位</td></tr><tr><td>最小值</td><td>最大值</td><td>最小值</td><td>最大值</td></tr><tr><td>tw(SCKL)</td><td>SCL 时钟低电平时间</td><td>4.7</td><td></td><td>1.2</td><td></td><td>us</td></tr><tr><td>tw(SCKH)</td><td>SCL 时钟高电平时间</td><td>4.0</td><td></td><td>0.6</td><td></td><td>us</td></tr><tr><td>tsu(SDA)</td><td>SDA 数据建立时间</td><td>250</td><td></td><td>100</td><td></td><td>ns</td></tr><tr><td>th(SDA)</td><td>SDA 数据保持时间</td><td>0</td><td></td><td>0</td><td>900</td><td>ns</td></tr><tr><td>tr(SDA)/tr(SCL)</td><td>SDA 和 SCL 上升时间</td><td></td><td>1000</td><td>20</td><td></td><td>ns</td></tr><tr><td>tf(SDA)/tf(SCL)</td><td>SDA 和 SCL 下降时间</td><td></td><td>300</td><td></td><td></td><td>ns</td></tr><tr><td>th(STA)</td><td>开始条件保持时间</td><td>4.0</td><td></td><td>0.6</td><td></td><td>us</td></tr><tr><td>tsu(STA)</td><td>重复的开始条件建立时间</td><td>4.7</td><td></td><td>0.6</td><td></td><td>us</td></tr><tr><td>tsu(STO)</td><td>停止条件建立时间</td><td>4.0</td><td></td><td>0.6</td><td></td><td>us</td></tr><tr><td>tw(STO:STA)</td><td>停止条件至开始条件的时间(总线空闲)</td><td>4.7</td><td></td><td>1.2</td><td></td><td>us</td></tr><tr><td>Cb</td><td>每条总线的容性负载</td><td></td><td>400</td><td></td><td>400</td><td>pF</td></tr></table>

# 4.3.14 SPI 接口特性

![](images/c73fdb7b50c68492e96490d66f7b8f12d68654f81ddf10ebbbef59424d107e7b.jpg)  
图 4-9 SPI 主模式时序图

![](images/b5d8579df1694f9f8e17970ddfbc09d9e7da47671128cd9bc659b0c4e92892c9.jpg)  
图 4-10 SPI 从模式时序图（CPHA=0）

![](images/d1b74ce2befebcb7395e0c5c0919edf7420c9edc3bd5dbc7a89e86f86b781806.jpg)  
图 4-11 SPI 从模式时序图（CPHA=1）

表 4-28 SPI 接口特性  

<table><tr><td>符号</td><td>参数</td><td colspan="2">条件</td><td>最小值</td><td>最大值</td><td>单位</td></tr><tr><td rowspan="2">fsck/tsck</td><td rowspan="2">SPI时钟频率</td><td colspan="2">主模式</td><td></td><td>72</td><td>MHz</td></tr><tr><td colspan="2">从模式</td><td></td><td>72</td><td>MHz</td></tr><tr><td>tr(SCK)/tf(SCK)</td><td>SPI时钟上升和下降时间</td><td colspan="2">负载电容:C=30pF</td><td></td><td>8</td><td>ns</td></tr><tr><td>tsu(NSS)</td><td>NSS建立时间</td><td colspan="2">从模式</td><td>2*tPCLK</td><td></td><td>ns</td></tr><tr><td>th(NSS)</td><td>NSS保持时间</td><td colspan="2">从模式</td><td>2*tPCLK</td><td></td><td>ns</td></tr><tr><td>tw(SCKH)/tw(SCKL)</td><td>SCK高电平和低电平时间</td><td colspan="2">主模式,fPCLK=24MHz,预分频系数=4</td><td>70</td><td>97</td><td>ns</td></tr><tr><td rowspan="2">tsu(MI)</td><td rowspan="3">数据输入建立时间</td><td rowspan="2">主模式</td><td>HSRXEN=0</td><td>10</td><td></td><td>ns</td></tr><tr><td>HSRXEN=1</td><td>10-0.5*tSCK</td><td></td><td>ns</td></tr><tr><td>tsu(SI)</td><td colspan="2">从模式</td><td>4</td><td></td><td>ns</td></tr><tr><td rowspan="2">th(MI)</td><td rowspan="3">数据输入保持时间</td><td rowspan="2">主模式</td><td>HSRXEN=0</td><td>-4</td><td></td><td>ns</td></tr><tr><td>HSRXEN=1</td><td>0.5*tSCK-4</td><td></td><td>ns</td></tr><tr><td>th(SI)</td><td colspan="2">从模式</td><td>4</td><td></td><td>ns</td></tr><tr><td>ta(SO)</td><td>数据输出访问时间</td><td colspan="2">从模式,fPCLK=20MHz</td><td>0</td><td>1*tPCLK</td><td>ns</td></tr><tr><td>tdis(SO)</td><td>数据输出禁止时间</td><td colspan="2">从模式</td><td>0</td><td>10</td><td>ns</td></tr><tr><td>tv(SO)</td><td rowspan="2">数据输出有效时间</td><td colspan="2">从模式(使能边沿之后)</td><td></td><td>10</td><td>ns</td></tr><tr><td>tv(MO)</td><td colspan="2">主模式(使能边沿之后)</td><td></td><td>5</td><td>ns</td></tr><tr><td>th(SO)</td><td rowspan="2">数据输出保持时间</td><td colspan="2">从模式(使能边沿之后)</td><td>8</td><td></td><td>ns</td></tr><tr><td>th(MO)</td><td colspan="2">主模式(使能边沿之后)</td><td>0</td><td></td><td>ns</td></tr></table>

# 4.3.15 I2S 接口特性

![](images/9af9acb801d18042fceb24efd2e40ef6f7414fb7574b970990a429856b629e51.jpg)  
图4-12 I2S总线主模式时序图

![](images/1dc22b7ea0800bbc6a2f730a0d4c90cd14d81eb8d968663ff4402e253c76a333.jpg)  
图4-13 I2S总线从模式时序图

表 4-29 I2S 接口特性  

<table><tr><td>符号</td><td>参数</td><td>条件</td><td>最小值</td><td>最大值</td><td>单位</td></tr><tr><td rowspan="2">fCK/tCK</td><td rowspan="2">I2S时钟频率</td><td>主模式</td><td></td><td>8</td><td>MHz</td></tr><tr><td>从模式</td><td></td><td>8</td><td>MHz</td></tr><tr><td>tr(CK)/tf(CK)</td><td>I2S时钟上升和下降时间</td><td>负载电容: C = 30pF</td><td></td><td>20</td><td>ns</td></tr><tr><td>tV(WS)</td><td>WS有效时间</td><td>主模式</td><td></td><td>5</td><td>ns</td></tr><tr><td>tsU(WS)</td><td>WS建立时间</td><td>从模式</td><td>10</td><td></td><td>ns</td></tr><tr><td rowspan="2">th(WS)</td><td rowspan="2">WS保持时间</td><td>主模式</td><td>0</td><td></td><td>ns</td></tr><tr><td>从模式</td><td>0</td><td></td><td>ns</td></tr><tr><td>tw(CKH)/tw(CKL)</td><td>SCK高电平和低电平时间</td><td>主模式, fPCLK = 36MHz预分频系数 = 4</td><td>40</td><td>60</td><td>%</td></tr><tr><td>tsU(SD_MR)</td><td>数据输入建立时间</td><td>主模式</td><td>8</td><td></td><td>ns</td></tr><tr><td>tsu(SD_SR)</td><td></td><td>从模式</td><td>8</td><td></td><td>ns</td></tr><tr><td>th(SD_MR)</td><td rowspan="2">数据输入保持时间</td><td>主模式</td><td>5</td><td></td><td>ns</td></tr><tr><td>th(SD_SR)</td><td>从模式</td><td>4</td><td></td><td>ns</td></tr><tr><td>th(SD_MT)</td><td rowspan="2">数据输出保持时间</td><td>主模式（使能边沿之后）</td><td></td><td>5</td><td>ns</td></tr><tr><td>th(SD_ST)</td><td>从模式（使能边沿之后）</td><td></td><td>5</td><td>ns</td></tr><tr><td>tv(SD_MT)</td><td rowspan="2">数据输出有效时间</td><td>主模式（使能边沿之后）</td><td></td><td>5</td><td>ns</td></tr><tr><td>tv(SD_ST)</td><td>从模式（使能边沿之后）</td><td></td><td>4</td><td>ns</td></tr></table>

# 4.3.16 USB 接口特性

表 4-30 USB 模块特性  

<table><tr><td>符号</td><td>参数</td><td>条件</td><td>最小值</td><td>最大值</td><td>单位</td></tr><tr><td>VDD</td><td>USB操作电压</td><td></td><td>3.0</td><td>3.6</td><td>V</td></tr><tr><td>VSE</td><td>单端接收器阈值</td><td>VDD=3.3V</td><td>1.2</td><td>1.9</td><td>V</td></tr><tr><td>VOL</td><td>静态输出低电平</td><td></td><td></td><td>0.3</td><td>V</td></tr><tr><td>VOH</td><td>静态输出高电平</td><td></td><td>2.8</td><td>3.6</td><td>V</td></tr><tr><td>VHSSQ</td><td>高速压制信息检测阈值</td><td></td><td>100</td><td>150</td><td>mV</td></tr><tr><td>VHSDSC</td><td>高速断开连接检测阈值</td><td></td><td>500</td><td>625</td><td>mV</td></tr><tr><td>VHSO1</td><td>高速空闲电平</td><td></td><td>-10</td><td>10</td><td>mV</td></tr><tr><td>VHSOH</td><td>高速数据高电平</td><td></td><td>360</td><td>440</td><td>mV</td></tr><tr><td>VHSOL</td><td>高速数据低电平</td><td></td><td>-10</td><td>10</td><td>mV</td></tr></table>

# 4.3.17 SD/MMC 接口特性

![](images/b8a771269fac5b59f1dfb57b98f183a53e59bd22baedf3fb6bc63fbc8eac9986.jpg)  
图4-14 SD高速模式时序图

![](images/f5aba34168917c5cadd103a913ed746a54cb9fff8dc1d4427d31b78ea0c7b6ed.jpg)  
图 4-15 SD 默认模式时序图

表 4-31 SD/MMC 接口特性  

<table><tr><td>符号</td><td>参数</td><td colspan="2">条件</td><td>最小值</td><td>最大值</td><td>单位</td></tr><tr><td>fCK/tCK</td><td>数据传输模式下的时钟频率</td><td colspan="2">CL≤30pF</td><td></td><td>48</td><td>MHz</td></tr><tr><td>tw(CKL)</td><td>时钟低电平时间</td><td colspan="2">CL≤30pF</td><td>6</td><td></td><td rowspan="4">ns</td></tr><tr><td>tw(CKH)</td><td>时钟高电平时间</td><td colspan="2">CL≤30pF</td><td>6</td><td></td></tr><tr><td>tr(CK)</td><td>上升时间</td><td colspan="2">CL≤30pF</td><td></td><td>4</td></tr><tr><td>tf(CK)</td><td>下降时间</td><td colspan="2">CL≤30pF</td><td></td><td>4</td></tr><tr><td colspan="7">CMD/DAT输入(参考CK)</td></tr><tr><td>tISU</td><td>输入建立时间</td><td colspan="2">CL≤30pF</td><td>7</td><td></td><td rowspan="2">ns</td></tr><tr><td>tIH</td><td>输入保持时间</td><td colspan="2">CL≤30pF</td><td>2</td><td></td></tr><tr><td colspan="7">在高速模式下,CMD/DAT输出(参考CK)</td></tr><tr><td rowspan="2">tov</td><td rowspan="2">输出有效时间</td><td rowspan="2">CL≤30pF</td><td>主模式</td><td></td><td>5</td><td rowspan="3">ns</td></tr><tr><td>从模式</td><td></td><td>9</td></tr><tr><td>tOH</td><td>输出保持时间</td><td colspan="2">CL≤30pF</td><td>20</td><td></td></tr><tr><td colspan="7">在默认模式下,CMD/DAT输出(参考CK)</td></tr><tr><td rowspan="2">tOVD</td><td rowspan="2">输出有效默认时间</td><td rowspan="2">CL≤30pF</td><td>主模式</td><td></td><td>8</td><td rowspan="3">ns</td></tr><tr><td>从模式</td><td></td><td>14</td></tr><tr><td>tOHD</td><td>输出保持默认时间</td><td colspan="2">CL≤30pF</td><td>20</td><td></td></tr></table>

# 4.3.18 FSMC 特性

![](images/bff74a16afb386446625e6a47e681710a8693aae33ea9b2e188de15dfb9dac7e.jpg)  
图 4-16 异步总线复用 PSRAM/NOR 读操作波形

表 4-32 异步总线复用的 PSRAM/NOR 读操作时序  

<table><tr><td>符号</td><td>参数</td><td>最小值</td><td>最大值</td><td>单位</td></tr><tr><td>\(t_{W(NE)}\)</td><td>FSMC_NE 低电平时间</td><td>\(7t_{HCLK}\)</td><td></td><td rowspan="15">ns</td></tr><tr><td>\(t_{V(NOE_NE)}\)</td><td>FSMC_NE 低至 FSMC_NOE 低</td><td>0</td><td></td></tr><tr><td>\(t_{W(NOE)}\)</td><td>FSMC_NOE 低时间</td><td>\(7t_{HCLK}\)</td><td></td></tr><tr><td>\(t_{h(NE_NOE)}\)</td><td>FSMC_NOE 高至 FSMC_NE 高保持时间</td><td>0</td><td></td></tr><tr><td>\(t_{V(A_NE)}\)</td><td>FSMC_NE 低至 FSMC_A 有效</td><td>0</td><td>5</td></tr><tr><td>\(t_{V(NADV_NE)}\)</td><td>FSMC_NE 低至 FSMC_NADV 低</td><td>0</td><td>5</td></tr><tr><td>\(t_{W(NADV)}\)</td><td>FSMC_NADV 低时间</td><td>\(t_{HCLK}\)</td><td></td></tr><tr><td>\(t_{h(AD_NADV)}\)</td><td>FSMC_NADV 高之后 FSMC_AD(地址)有效保持时间</td><td>\(2t_{HCLK}\)</td><td></td></tr><tr><td>\(t_{h(A_NOE)}\)</td><td>FSMC_NOE 高之后的地址保持时间</td><td>0</td><td></td></tr><tr><td>\(t_{h(BL_NOE)}\)</td><td>FSMC_NOE 高之后的 FSMC_BL 保持时间</td><td>0</td><td></td></tr><tr><td>\(t_{V(BL_NE)}\)</td><td>FSMC_NE 低至 FSMC_BL 有效</td><td>0</td><td>5</td></tr><tr><td>\(t_{SU(DATA_NE)}\)</td><td>数据至 FSMC_NE 高的建立时间</td><td>\(3t_{HCLK}\)</td><td></td></tr><tr><td>\(t_{SU(DATA_NOE)}\)</td><td>数据至 FSMC_NOE 高的建立时间</td><td>\(3t_{HCLK}\)</td><td></td></tr><tr><td>\(t_{h(DATA_NE)}\)</td><td>FSMC_NE 高之后的数据保持时间</td><td>0</td><td></td></tr><tr><td>\(t_{h(DATA_NOE)}\)</td><td>FSMC_NOE 高之后的数据保持时间</td><td>0</td><td></td></tr></table>

![](images/5122274f9719beb08fb1dcbfd012e598dfe997321d3d8556a92ce717665dcb90.jpg)  
图 4-17 异步总线复用 PSRAM/NOR 写操作波形

表 4-33 异步总线复用 PSRAM/NOR 写操作时序  

<table><tr><td>符号</td><td>参数</td><td>最小值</td><td>最大值</td><td>单位</td></tr><tr><td>tw(NE)</td><td>FSMC_NE低电平时间</td><td>5thCLK</td><td></td><td rowspan="13">ns</td></tr><tr><td>tv(NEW_NE)</td><td>FSMC_NE低至FSMC_NWE低</td><td>3thCLK</td><td></td></tr><tr><td>tw(NWE)</td><td>FSMC_NWE低时间</td><td>2thCLK</td><td></td></tr><tr><td>th(NE_NWE)</td><td>FSMC_NWE高至FSMC_NE高保持时间</td><td>thCLK</td><td></td></tr><tr><td>tv(A_NE)</td><td>FSMC_NE低至FSMC_A有效</td><td>0</td><td>5</td></tr><tr><td>tv(NADV_NE)</td><td>FSMC_NE低至FSMC_NADV低</td><td>0</td><td>5</td></tr><tr><td>tw(NADV)</td><td>FSMC_NADV低时间</td><td>thCLK</td><td></td></tr><tr><td>th(AD_NADV)</td><td>FSMC_NADV高之后FSMC_AD(地址)有效保持时间</td><td>2thCLK</td><td></td></tr><tr><td>th(A_NWE)</td><td>FSMC_NWE高之后的地址保持时间</td><td>thCLK</td><td></td></tr><tr><td>tv(BL_NE)</td><td>FSMC_NE低至FSMC_BL有效</td><td>0</td><td>5</td></tr><tr><td>th(BL_NWE)</td><td>FSMC_NWE高之后的FSMC_BL保持时间</td><td>thCLK</td><td></td></tr><tr><td>tv(DATA_NADV)</td><td>FSMC_NADV高至数据保持时间</td><td>2thCLK</td><td></td></tr><tr><td>th(DATA_NWE)</td><td>FSMC_NWE高之后的数据保持时间</td><td>thCLK</td><td></td></tr></table>

![](images/95373f5fbda36cd875aaa1395b6a04f77ba8d5328824e6acf9ae88a16e07091d.jpg)  
图 4-18 同步总线复用 NOR/PSRAM 读波形

表 4-34 同步总线复用 NOR/PSRAM 读时序  

<table><tr><td>符号</td><td>参数</td><td>最小值</td><td>最大值</td><td>单位</td></tr><tr><td>\(t_{W(CLK)}\)</td><td>FSMC_CLK 周期</td><td>\(2t_{HCLK}\)</td><td></td><td rowspan="15">ns</td></tr><tr><td>\(t_{d(CLKL_NEL)}\)</td><td>FSMC_CLK低至FSMC_NE低</td><td>0</td><td>5</td></tr><tr><td>\(t_{d(CLKH_NEH)}\)</td><td>FSMC_CLK高至FSMC_NE高</td><td>0.5\(t_{HCLK}\)</td><td>0.5\(t_{HCLK}\)</td></tr><tr><td>\(t_{d(CLKL_NADVVL)}\)</td><td>FSMC_CLK低至FSMC_NADV低</td><td>0</td><td>5</td></tr><tr><td>\(t_{d(CLKL_NADVH)}\)</td><td>FSMC_CLK低至FSMC_NADV高</td><td>0</td><td>5</td></tr><tr><td>\(t_{d(CLKL_AV)}\)</td><td>FSMC_CLK低至FSMC_Ax有效 (x=16...23)</td><td>0</td><td>5</td></tr><tr><td>\(t_{d(CLKH_AIV)}\)</td><td>FSMC_CLK高至FSMC_Ax无效 (x=16...23)</td><td>0</td><td>5</td></tr><tr><td>\(t_{d(CLKL_NOEL)}\)</td><td>FSMC_CLK低至FSMC_NOE低</td><td>\(2t_{HCLK}\)</td><td></td></tr><tr><td>\(t_{d(CLKH_NOEH)}\)</td><td>FSMC_CLK高至FSMC_NOE高</td><td>\(t_{HCLK}\)</td><td></td></tr><tr><td>\(t_{d(CLKL_ADV)}\)</td><td>FSMC_CLK低至FSMC_AD[15:0]有效</td><td>0</td><td>5</td></tr><tr><td>\(t_{d(CLKL_ADV)}\)</td><td>FSMC_CLK低至FSMC_AD[15:0]无效</td><td>0</td><td>5</td></tr><tr><td>\(t_{SU(ADV_CLKH)}\)</td><td>FSMC_CLK高之前FSMC_AD[15:0]有效数据</td><td>8</td><td></td></tr><tr><td>\(t_{h(CLKH_ADV)}\)</td><td>FSMC_CLK高之后FSMC_AD[15:0]有效数据</td><td>8</td><td></td></tr><tr><td>\(t_{SU(NWAITV_CLKH)}\)</td><td>FSMC_CLK高之前FSMC_NWAIT有效</td><td>6</td><td></td></tr><tr><td>\(t_{h(CLKH_NWAITV)}\)</td><td>FSMC_CLK高之后FSMC_NWAIT有效</td><td>2</td><td></td></tr></table>

![](images/45912a97771c7a5244be10f7c20b5adafe6fec5b1c637c056bf6ac2ec50d7777.jpg)  
图 4-19 同步总线复用 PSRAM 写波形

表 4-35 同步总线复用 PSRAM 写时序  

<table><tr><td>符号</td><td>参数</td><td>最小值</td><td>最大值</td><td>单位</td></tr><tr><td>tw(CLK)</td><td>FSMC_CLK 周期</td><td>2thCLK</td><td></td><td rowspan="15">ns</td></tr><tr><td>td(CLK_NEL)</td><td>FSMC_CLK低至FSMC_NE低</td><td>0</td><td>5</td></tr><tr><td>td(CLKH_NEH)</td><td>FSMC_CLK高至FSMC_NE高</td><td>0.5thCLK</td><td>0.5thCLK</td></tr><tr><td>td(CLKL_NADVL)</td><td>FSMC_CLK低至FSMC_NADV低</td><td>0</td><td>5</td></tr><tr><td>td(CLKL_NADVH)</td><td>FSMC_CLK低至FSMC_NADV高</td><td>0</td><td>5</td></tr><tr><td>td(CLKL_AV)</td><td>FSMC_CLK低至FSMC_Ax有效 (x=16...23)</td><td>0</td><td>5</td></tr><tr><td>td(CLKH_AIV)</td><td>FSMC_CLK高至FSMC_Ax无效 (x=16...23)</td><td>0</td><td>5</td></tr><tr><td>td(CLKL_NWEL)</td><td>FSMC_CLK低至FSMC_NWE低</td><td>0</td><td></td></tr><tr><td>td(CLKH_NWEH)</td><td>FSMC_CLK高至FSMC_NWE高</td><td>0</td><td></td></tr><tr><td>td(CLKL_ADV)</td><td>FSMC_CLK低至FSMC_AD[15:0]有效</td><td>0</td><td>5</td></tr><tr><td>td(CLKL_ADV)</td><td>FSMC_CLK低至FSMC_AD[15:0]无效</td><td>0</td><td>5</td></tr><tr><td>td(CLKL_DATA)</td><td>FSMC_CLK低之后FSMC_AD[15:0]有效</td><td>2</td><td></td></tr><tr><td>tsu(NWAITV_CLKH)</td><td>FSMC_CLK高之前FSMC_NWAIT有效</td><td>6</td><td></td></tr><tr><td>th(CLKH_NWAITV)</td><td>FSMC_CLK高之后FSMC_NWAIT有效</td><td>2</td><td></td></tr><tr><td>td(CLKL_NBLH)</td><td>FSMC_CLK低至FSMC_NBL高</td><td>2</td><td></td></tr></table>

# NAND控制器波形和时序

测试条件：NAND操作区域，选择16位数据宽度，使能ECC计算电路，512字节页面大小，其他时序配置为设置寄存器 $\mathsf { F S M C \_ P C R 2 = 0 \times 0 0 0 2 0 0 5 E }$ ，FSMC_PMEM2=0x01020301，FSMC_PATT2=0x01020301。

![](images/edb0bab3d0ef95a37813e7e0b9d2117a819282cad2ae1d2c10c2dcbeeb442978.jpg)  
图 4-20 NAND 控制器读操作波形

![](images/16bb96bf98ef12fdc58ef2270ff23315cebcac9b96d4752d6ef20cde1b9b2087.jpg)  
图 4-21 NAND 控制器写操作波形

![](images/d628b3ba944eb5da3e60519d116dc7f89a373c4317effeea6b1001d7907c393c.jpg)  
图4-22 NAND控制器在通用存储空间的读操作波形

![](images/9af6ba1d49d30aa0cb7530a7768e5eaa5c73dab901479de013e65265131842cd.jpg)  
图 4-23 NAND 控制器在通用存储空间的写操作波形

表4-36 NAND闪存读写周期的时序特性  

<table><tr><td>符号</td><td>参数</td><td>最小值</td><td>最大值</td><td>单位</td></tr><tr><td>td(D-NWE)</td><td>FSMC_NWE高之前至FSMC_D[15:0]数据有效</td><td>4thCLK</td><td></td><td rowspan="11">ns</td></tr><tr><td>tw(NOE)</td><td>FSMC_NOE低时间</td><td>4thCLK</td><td></td></tr><tr><td>tsu(D-NOE)</td><td>FSMC_NOE高之前至FSMC_D[15:0]数据有效</td><td>20</td><td></td></tr><tr><td>th(NOE-D)</td><td>FSMC_NOE高之后至FSMC_D[15:0]数据有效</td><td>15</td><td></td></tr><tr><td>tw(NWE)</td><td>FSMC_NWE低时间</td><td>4thCLK</td><td></td></tr><tr><td>tv(NWE-D)</td><td>FSMC_NWE低至FSMC_D[15:0]数据有效</td><td>0</td><td></td></tr><tr><td>th(NWE-D)</td><td>FSMC_NWE高至FSMC_D[15:0]数据无效</td><td>2thCLK</td><td></td></tr><tr><td>td(ALE-NWE)</td><td>FSMC_NWE低之前至FSMC_ALE有效</td><td>2thCLK</td><td></td></tr><tr><td>th(NWE-ALE)</td><td>FSMC_NWE高至FSMC_ALE无效</td><td>2thCLK</td><td></td></tr><tr><td>td(ALE-NOE)</td><td>FSMC_NOE低之前至FSMC_ALE有效</td><td>2thCLK</td><td></td></tr><tr><td>th(NOE-ALE)</td><td>FSMC_NOE高至FSMC_ALE无效</td><td>4thCLK</td><td></td></tr></table>

# 4.3.19 DVP 接口特性

![](images/d50c6912a6ab3932b889d9812e5f4adaee132d862a72cf7d1d3237bb18cc00fd.jpg)  
图 4-24 DVP 时序波形

表 4-37 DVP 接口特性  

<table><tr><td>符号</td><td>参数及描述</td><td>最小值</td><td>最大值</td><td>单位</td></tr><tr><td>fPixCLK/tPixCLK</td><td>像素时钟输入频率</td><td></td><td>144</td><td>MHz</td></tr><tr><td>Duty (PixCLK)</td><td>像素时钟的占空比</td><td>15</td><td></td><td>%</td></tr><tr><td>tsu (DATA)</td><td>数据建立时间</td><td>2.5</td><td></td><td rowspan="4">ns</td></tr><tr><td>th (DATA)</td><td>数据保持时间</td><td>1</td><td></td></tr><tr><td>tsu (HSYNC) / tsu
(VSYNC)</td><td>HSYNC/VSYNC信号输入建立时间</td><td>2.5</td><td></td></tr><tr><td>th (HSYNC) / th
(VSYNC)</td><td>HSYNC/VSYNC信号输入保持时间</td><td>1</td><td></td></tr></table>

# 4.3.20 千兆以太网接口特性

![](images/734bdabd66b75c20321dd93162d287b51153bd23976632ec3ced6b45d8818158.jpg)  
图 4-25 ETH-SMI 时序波形

表 4-38 以太网 MAC 的 SMI 信号特性  

<table><tr><td>符号</td><td>参数及描述</td><td>最小值</td><td>典型值</td><td>最大值</td><td>单位</td></tr><tr><td>fMDC/tMDC</td><td>MDC时钟频率</td><td></td><td></td><td>2.5</td><td>MHz</td></tr><tr><td>td(MDIO)</td><td>MDIO写数据的有效时间</td><td>0</td><td></td><td>300</td><td rowspan="3">ns</td></tr><tr><td>tsu(MDIO)</td><td>读数据建立时间</td><td>10</td><td></td><td></td></tr><tr><td>th(MDIO)</td><td>读数据保持时间</td><td>10</td><td></td><td></td></tr></table>

![](images/1c1000b76e5574a90b9278739928aa57945ed6d302391558a9b9377b3cdf0662.jpg)  
图 4-26 ETH-RMII 信号时序波形

表 4-39 以太网 MAC 信号 RMII 信号特性  

<table><tr><td>符号</td><td>参数及描述</td><td>最小值</td><td>典型值</td><td>最大值</td><td>单位</td></tr><tr><td>tsu(RXD)</td><td>接收数据的建立时间</td><td>4</td><td></td><td></td><td rowspan="6">ns</td></tr><tr><td>tih(RXD)</td><td>接收数据的保持时间</td><td>2</td><td></td><td></td></tr><tr><td>tsu(CRS_DV)</td><td>载波侦测信号建立时间</td><td>4</td><td></td><td></td></tr><tr><td>tih(CRS_DV)</td><td>载波侦测信号保持时间</td><td>2</td><td></td><td></td></tr><tr><td>td(TXEN)</td><td>传输使能有效延迟时间</td><td></td><td></td><td>16</td></tr><tr><td>td(TXD)</td><td>数据传输有效延迟时间</td><td></td><td></td><td>16</td></tr></table>

![](images/a0c1bdc0c279193eaa92630e1f5c45d3af9b61f520f2a3019b40d4f3c105ac46.jpg)  
图 4-27 ETH-MII 信号时序波形

表 4-40 以太网 MAC 信号 MII 信号特性  

<table><tr><td>符号</td><td>参数及描述</td><td>最小值</td><td>典型值</td><td>最大值</td><td>单位</td></tr><tr><td>tsu(RXD)</td><td>接收数据的建立时间</td><td>10</td><td></td><td></td><td rowspan="8">ns</td></tr><tr><td>tih(RXD)</td><td>接收数据的保持时间</td><td>10</td><td></td><td></td></tr><tr><td>tsu(DV)</td><td>数据有效信号建立时间</td><td>10</td><td></td><td></td></tr><tr><td>tih(DV)</td><td>数据有效信号保持时间</td><td>10</td><td></td><td></td></tr><tr><td>tsu(ER)</td><td>错误信号建立时间</td><td>10</td><td></td><td></td></tr><tr><td>tih(ER)</td><td>错误信号保持时间</td><td>10</td><td></td><td></td></tr><tr><td>td(TXEN)</td><td>传输使能有效延迟时间</td><td></td><td></td><td>16</td></tr><tr><td>td(TXD)</td><td>数据传输有效延迟时间</td><td></td><td></td><td>16</td></tr></table>

![](images/fe2683b189ab1b4bdbfba6a9c739ff29e1060fdd9ab37238b1936ad753d927b4.jpg)  
图 4-28 ETH-RGMII 信号时序波形

表 4-41 以太网 MAC 信号 RGMII 信号特性  

<table><tr><td>符号</td><td>参数及描述</td><td>最小值</td><td>典型值</td><td>最大值</td><td>单位</td></tr><tr><td>fTXC/tTXC</td><td>TXC/RXC时钟频率</td><td>7.2</td><td>8</td><td>8.8</td><td rowspan="7">ns</td></tr><tr><td>tR</td><td>TXC/RXC上升时间</td><td></td><td></td><td>2.0</td></tr><tr><td>tF</td><td>TXC/RXC下降时间</td><td></td><td></td><td>2.0</td></tr><tr><td>tsu(TDATA)</td><td>发送数据建立时间</td><td>1.2</td><td>2.0</td><td></td></tr><tr><td>th(TDATA)</td><td>发送数据保持时间</td><td>1.2</td><td>2.0</td><td></td></tr><tr><td>tsu(RDATA)</td><td>输入数据建立时间</td><td>1.2</td><td>2.0</td><td></td></tr><tr><td>th(RDATA)</td><td>输入数据保持时间</td><td>1.2</td><td>2.0</td><td></td></tr></table>

# 4.3.21 12 位 ADC 特性

表 4-42 ADC 特性  

<table><tr><td>符号</td><td>参数</td><td>条件</td><td>最小值</td><td>典型值</td><td>最大值</td><td>单位</td></tr><tr><td>VDDA</td><td>供电电压</td><td></td><td>2.4</td><td></td><td>3.6</td><td>V</td></tr><tr><td>VREF+</td><td>正参考电压</td><td>VREF+不能高于VDDA</td><td>2.4</td><td></td><td>VDDA</td><td>V</td></tr><tr><td>IVREF</td><td>参考电流</td><td></td><td></td><td>160</td><td>220</td><td>uA</td></tr><tr><td>IDDA</td><td>供电电流</td><td></td><td></td><td>480</td><td>530</td><td>uA</td></tr><tr><td>fADC</td><td>ADC时钟频率</td><td></td><td></td><td></td><td>14</td><td>MHz</td></tr><tr><td>fS</td><td>采样速率</td><td></td><td>0.05</td><td></td><td>1</td><td>MHz</td></tr><tr><td>fTRIG</td><td>外部触发频率</td><td></td><td></td><td></td><td>16</td><td>1/fADC</td></tr><tr><td>VAIN</td><td>转换电压范围</td><td></td><td>0</td><td></td><td>VREF+</td><td>V</td></tr><tr><td>RAIN</td><td>外部输入阻抗</td><td></td><td></td><td></td><td>50</td><td>kΩ</td></tr><tr><td>RADC</td><td>采样开关电阻</td><td></td><td></td><td>0.6</td><td>1.5</td><td>kΩ</td></tr><tr><td>GADC</td><td>内部采样和保持电容</td><td></td><td></td><td>8</td><td></td><td>pF</td></tr><tr><td>tCAL</td><td>校准时间</td><td></td><td></td><td>40</td><td></td><td>1/fADC</td></tr><tr><td>tlat</td><td>注入触发转换时延</td><td></td><td></td><td></td><td>2</td><td>1/fADC</td></tr><tr><td>tlatr</td><td>常规触发转换时延</td><td></td><td></td><td></td><td>2</td><td>1/fADC</td></tr><tr><td>ts</td><td>采样时间</td><td></td><td>1.5</td><td></td><td>239.5</td><td>1/fADC</td></tr><tr><td>tSTAB</td><td>上电时间</td><td></td><td></td><td></td><td>1</td><td>us</td></tr><tr><td>tCONV</td><td>总的转换时间（包括采样时间）</td><td></td><td>14</td><td></td><td>252</td><td>1/fADC</td></tr></table>

注：以上均为设计参数保证。

公式：最大 $\mathsf { R } _ { \mathsf { A l N } }$

$$
\mathrm {R _ {A I N} <   \frac {T _ {S}}{f _ {A D C} \times C _ {A D C} \times \ln 2 ^ {N + 2}} - R _ {A D C}}
$$

上述公式用于决定最大的外部阻抗，使得误差可以小于1/4 LSB。其中 $N = 1 2$ （表示12位分辨率）。

表 $4 { - } 4 3 \neq _ { \tt A D C } = 1 4 \mathsf { M H } z$ 时的最大 $\mathsf { R } _ { \mathsf { A l N } }$   

<table><tr><td>Ts(周期)</td><td>ts (us)</td><td>最大RAIN (kΩ)</td></tr><tr><td>1.5</td><td>0.11</td><td>0.4</td></tr><tr><td>7.5</td><td>0.54</td><td>5.9</td></tr><tr><td>13.5</td><td>0.96</td><td>11.4</td></tr><tr><td>28.5</td><td>2.04</td><td>25.2</td></tr><tr><td>41.5</td><td>2.96</td><td>37.2</td></tr><tr><td>55.5</td><td>3.96</td><td>50</td></tr><tr><td>71.5</td><td>5.11</td><td>无效</td></tr><tr><td>239.5</td><td>17.1</td><td>无效</td></tr></table>

表 4-44 ADC 误差  

<table><tr><td>符号</td><td>参数</td><td>条件</td><td>最小值</td><td>典型值</td><td>最大值</td><td>单位</td></tr><tr><td>E0</td><td>偏移误差</td><td rowspan="3">fPCLK2=56MHz,fADC=14MHz,RAIN&lt;10kΩ, VDDA=3.3V</td><td></td><td>±4</td><td></td><td rowspan="3">LSB</td></tr><tr><td>ED</td><td>微分非线性误差</td><td></td><td>±0.5</td><td>±3</td></tr><tr><td>EL</td><td>积分非线性误差</td><td></td><td>±1</td><td>±4</td></tr></table>

$\complement _ { \mathfrak { p } }$ 表示PCB与焊盘上的寄生电容（大约5pF），可能与焊盘和PCB布局质量有关。较大的 $\complement _ { \mathfrak { p } }$ 数值将降低转换精度，解决办法是降低fADC值。

![](images/c928be7dfd664c165502fc67684bbdf14160a7ce04d866673d2b64f59eef5d90.jpg)  
图 4-29 ADC 典型连接图

![](images/708e181227a994056df8a89563772bb7b9a18f1b14d19c38cc6da2dcec5ba197.jpg)  
图4-30 模拟电源及退耦电路参考

# 4.3.22 温度传感器特性

表4-45 温度传感器特性  

<table><tr><td>符号</td><td>参数</td><td>条件</td><td>最小值</td><td>典型值</td><td>最大值</td><td>单位</td></tr><tr><td>RTS</td><td>温度传感器测量范围</td><td></td><td>-40</td><td></td><td>85</td><td>°C</td></tr><tr><td>ATSC</td><td>温度传感器的测量误差</td><td></td><td></td><td>±12</td><td></td><td>°C</td></tr><tr><td>Avg_Slope</td><td>平均斜率(负温度系数)</td><td></td><td>3.8</td><td>4.3</td><td>4.8</td><td>mV/°C</td></tr><tr><td>V25</td><td>在25°C时的电压</td><td></td><td>1.34</td><td>1.40</td><td>1.46</td><td>V</td></tr><tr><td>TS_temp</td><td>当读取温度时,ADC采样时间</td><td>fADC=14MHz</td><td></td><td></td><td>17.1</td><td>us</td></tr></table>

# 4.3.23 DAC 特性

表 4-46 DAC 特性  

<table><tr><td>符号</td><td>参数</td><td>条件</td><td>最小值</td><td>典型值</td><td>最大值</td><td>单位</td></tr><tr><td>VDDA</td><td>供电电压</td><td></td><td>2.4</td><td>3.3</td><td>3.6</td><td>V</td></tr><tr><td>VREF+</td><td>正参考电压</td><td>VREF+不能高于VDDA</td><td>2.4</td><td>3.3</td><td>3.6</td><td>V</td></tr><tr><td rowspan="2">RL(1)</td><td>缓冲器打开时的负载电阻</td><td></td><td>5</td><td></td><td></td><td>kΩ</td></tr><tr><td>缓冲器关闭时的负载电阻</td><td></td><td>15</td><td></td><td></td><td>kΩ</td></tr><tr><td>CL(1)</td><td>缓冲器打开时负载电容</td><td></td><td></td><td></td><td>50</td><td>pF</td></tr><tr><td>VOUT_MIN(1)</td><td rowspan="2">缓冲器打开,12位DAC转换</td><td></td><td>0</td><td></td><td>8</td><td>mV</td></tr><tr><td>VOUT_MAX(1)</td><td>VREF+ = 3.3V</td><td>3.29</td><td></td><td>3.3</td><td>V</td></tr><tr><td>VOUT_MIN(1)</td><td rowspan="2">缓冲器关闭,12位DAC转换</td><td></td><td>0</td><td></td><td>3</td><td>mV</td></tr><tr><td>VOUT_MAX(1)</td><td>VREF+ = 3.3V</td><td>3.295</td><td></td><td>3.3</td><td>V</td></tr><tr><td rowspan="3">IVREF+</td><td colspan="2">无负载,输入值0x800</td><td></td><td>58</td><td></td><td rowspan="3">uA</td></tr><tr><td colspan="2">无负载,VREF+ = 3.6V时,输入值0xF1C</td><td></td><td>194</td><td></td></tr><tr><td colspan="2">无负载,VREF+ = 3.6V时,输入值0xF555(最差)</td><td></td><td>331</td><td></td></tr><tr><td rowspan="3">IDDA</td><td colspan="2">缓冲器打开无负载,输入值0x800</td><td></td><td>170</td><td></td><td rowspan="3">uA</td></tr><tr><td colspan="2">缓冲器打开无负载,VREF+ = 3.6V,输入值0xF1C</td><td></td><td>150</td><td></td></tr><tr><td colspan="2">缓冲器打开无负载,VREF+ = 3.6V,输入值0x555(最差)</td><td></td><td>170</td><td></td></tr><tr><td>DNL</td><td>微分非线性误差</td><td></td><td></td><td>±2</td><td></td><td>LSB</td></tr><tr><td>INL</td><td>积分非线性误差</td><td>经过失调误差和增益误差校正后</td><td></td><td>±4</td><td></td><td>LSB</td></tr><tr><td rowspan="2">失调</td><td rowspan="2">偏移误差</td><td></td><td></td><td>±3</td><td>±12</td><td>mV</td></tr><tr><td>VREF+ = 3.6V</td><td></td><td></td><td>±10</td><td>LSB</td></tr><tr><td>增益误差</td><td></td><td>DAC配置为12位</td><td></td><td>±0.4</td><td></td><td>%</td></tr><tr><td>放大器增益(1)</td><td>开环时放大器的增益</td><td>5kΩ的负载(最大)</td><td>80</td><td>85</td><td></td><td>dB</td></tr><tr><td>tSETTLING</td><td>设置时间(全范围:输入代码</td><td>CLOAD≤50pF</td><td></td><td>3</td><td>4</td><td>us</td></tr><tr><td></td><td>从最小值转变为最大值, DAC_OUT达到其终值的±1 LSB)</td><td>\( R_{LOAD} \geq 5k\Omega \)</td><td></td><td></td><td></td><td></td></tr><tr><td>更新速率</td><td>当输入代码为较小变化时(从数值i变到i+1LSB),得到正确 DAC_OUT的最大频率</td><td>\( C_{LOAD} \leq 50pF \)\( R_{LOAD} \geq 5k\Omega \)</td><td></td><td></td><td>1</td><td>MS/s</td></tr><tr><td>\( t_{WAKEUP} \)</td><td>从关闭状态唤醒的时间(DAC的使能位ENx从0变为1)</td><td>\( C_{LOAD} \leq 50pF \),\( R_{LOAD} \geq 5k\Omega \),输入代码介于最小和最大可能数值之间</td><td></td><td>6.5</td><td>10</td><td>us</td></tr><tr><td>\( PSRR^{+(1)} \)</td><td>供电抑制比(相对于\( V_{DDA} \))(静态直流测量)</td><td>没有\( R_{LOAD} \),\( C_{LOAD} \leq 50pF \)</td><td></td><td>-100</td><td>-75</td><td>dB</td></tr></table>

注：来源设计或仿真非实测。

# 4.3.24 OPA 特性

表 4-47-1 OPA 特性  

<table><tr><td>符号</td><td>参数</td><td>条件</td><td>最小值</td><td>典型值</td><td>最大值</td><td>单位</td></tr><tr><td>VDDA</td><td>供电电压</td><td></td><td>2.4</td><td>3.3</td><td>3.6</td><td>V</td></tr><tr><td>CMIR</td><td>共模输入电压</td><td></td><td>0</td><td></td><td>VDDA</td><td>V</td></tr><tr><td>OSW(3)</td><td>输出摆幅</td><td></td><td>0</td><td></td><td>VDDA</td><td>V</td></tr><tr><td>VOFFSET</td><td>输入失调电压</td><td></td><td></td><td>±2.5</td><td>±10</td><td>mV</td></tr><tr><td>ILOAD</td><td>驱动电流</td><td></td><td></td><td></td><td>600</td><td>uA</td></tr><tr><td>IDDOPAMP</td><td>消耗电流</td><td>无负载,静态模式</td><td></td><td>195</td><td></td><td>uA</td></tr><tr><td>CMRR(1)</td><td>共模抑制比</td><td>@1kHz</td><td></td><td>96</td><td></td><td>dB</td></tr><tr><td>PSRR(1)</td><td>电源抑制比</td><td>@1kHz</td><td></td><td>86</td><td></td><td>dB</td></tr><tr><td>AV(1)</td><td>开环增益</td><td>CLOAD=5pF</td><td></td><td>136</td><td></td><td>dB</td></tr><tr><td>GBW(1)</td><td>单位增益带宽</td><td>CLOAD=5pF</td><td></td><td>19</td><td></td><td>MHz</td></tr><tr><td>PM(1)</td><td>相位裕度</td><td>CLOAD=5pF</td><td></td><td>93</td><td></td><td>°</td></tr><tr><td>SR(1)</td><td>压摆率</td><td>CLOAD=5pF</td><td></td><td>8</td><td></td><td>V/us</td></tr><tr><td>tWAKUP(1)</td><td>关闭到唤醒建立时间,0.1%</td><td>输入VDDA/2, CLOAD=50pF, RLOAD=4kΩ</td><td></td><td></td><td>0.5</td><td>us</td></tr><tr><td>RLOAD</td><td>电阻性负载</td><td></td><td>4</td><td></td><td></td><td>kΩ</td></tr><tr><td>CLOAD</td><td>电容性负载</td><td></td><td></td><td></td><td>50</td><td>pF</td></tr><tr><td rowspan="2">VOHSAT(2)</td><td rowspan="2">高饱和输出电压</td><td>RLOAD=4kΩ,输入VDDA</td><td>VDDA-300</td><td>VDDA-150</td><td></td><td rowspan="2">mV</td></tr><tr><td>RLOAD=20kΩ,输入VDDA</td><td>VDDA-50</td><td>VDDA-30</td><td></td></tr><tr><td rowspan="2">VOLSAT(2)</td><td rowspan="2">低饱和输出电压</td><td>RLOAD=4kΩ,输入0</td><td></td><td>3</td><td>10</td><td rowspan="2">mV</td></tr><tr><td>RLOAD=20kΩ,输入0</td><td></td><td>3</td><td>10</td></tr><tr><td rowspan="2">EN(1)</td><td rowspan="2">等效输入电压噪声</td><td>RLOAD=4kΩ, @1kHz</td><td></td><td>83</td><td></td><td rowspan="2">nV/√Hz</td></tr><tr><td>RLOAD=4kΩ, @10kHz</td><td></td><td>42</td><td></td></tr></table>

注：1.来源仿真非实测。  
2.负载电流会限制饱和输出电压。  
3.当PE7、PE8、PE14、PE15引脚用于运放输出时，其输出摆幅受限： $V _ { D D A } = 3 . \ 3 V$ 时， $0 { \leqslant } 0 _ { s w } { \leqslant } 2 V _ { o }$

表 4-47-2 OPA 特性（高速模式）  

<table><tr><td>符号</td><td>参数</td><td>条件</td><td>最小值</td><td>典型值</td><td>最大值</td><td>单位</td></tr><tr><td>VDDA</td><td>供电电压</td><td></td><td>2.4</td><td>3.3</td><td>3.6</td><td>V</td></tr><tr><td>CMIR</td><td>共模输入电压</td><td></td><td>0</td><td></td><td>VDDA</td><td>V</td></tr><tr><td>OSW(3)</td><td>输出摆幅</td><td></td><td>0</td><td></td><td>VDDA</td><td>V</td></tr><tr><td>VIOFFSET</td><td>输入失调电压</td><td></td><td></td><td>±2.5</td><td>±10</td><td>mV</td></tr><tr><td>ILOAD</td><td>驱动电流</td><td></td><td></td><td></td><td>600</td><td>uA</td></tr><tr><td>IDDOPAMP</td><td>消耗电流</td><td>无负载,静态模式</td><td></td><td>770</td><td></td><td>uA</td></tr><tr><td>CMRR(1)</td><td>共模抑制比</td><td>@1kHz</td><td></td><td>96</td><td></td><td>dB</td></tr><tr><td>PSRR(1)</td><td>电源抑制比</td><td>@1kHz</td><td></td><td>86</td><td></td><td>dB</td></tr><tr><td>AV(1)</td><td>开环增益</td><td>CLOAD=5pF</td><td></td><td>136</td><td></td><td>dB</td></tr><tr><td>GBW(1)</td><td>单位增益带宽</td><td>CLOAD=5pF</td><td></td><td>53</td><td></td><td>MHz</td></tr><tr><td>PM(1)</td><td>相位裕度</td><td>CLOAD=5pF</td><td></td><td>93</td><td></td><td>°</td></tr><tr><td>SR(1)</td><td>压摆率</td><td>CLOAD=5pF</td><td></td><td>16</td><td></td><td>V/us</td></tr><tr><td>tWAKUP(1)</td><td>关闭到唤醒建立时间,0.1%</td><td>输入VDDA/2, CLOAD=50pF, RLOAD=4kΩ</td><td></td><td></td><td>0.5</td><td>us</td></tr><tr><td>RLOAD</td><td>电阻性负载</td><td></td><td>4</td><td></td><td></td><td>kΩ</td></tr><tr><td>CLOAD</td><td>电容性负载</td><td></td><td></td><td></td><td>30</td><td>pF</td></tr><tr><td rowspan="2">VOHSAT(2)</td><td rowspan="2">高饱和输出电压</td><td>RLOAD=4kΩ,输入VDDA</td><td>VDDA-300</td><td>VDDA-150</td><td></td><td rowspan="2">mV</td></tr><tr><td>RLOAD=20kΩ,输入VDDA</td><td>VDDA-50</td><td>VDDA-30</td><td></td></tr><tr><td rowspan="2">VOLSAT(2)</td><td rowspan="2">低饱和输出电压</td><td>RLOAD=4kΩ,输入0</td><td></td><td>3</td><td>10</td><td rowspan="2">mV</td></tr><tr><td>RLOAD=20kΩ,输入0</td><td></td><td>3</td><td>10</td></tr><tr><td rowspan="2">EN(1)</td><td rowspan="2">等效输入电压噪声</td><td>RLOAD=4kΩ, @1kHz</td><td></td><td>83</td><td></td><td rowspan="2">nV/√Hz</td></tr><tr><td>RLOAD=4kΩ, @10kHz</td><td></td><td>42</td><td></td></tr></table>

注：1.来源仿真非实测。  
2.负载电流会限制饱和输出电压。  
3.当PE7、PE8、PE14、PE15引脚用于运放输出时，其输出摆幅受限： $V _ { D D A } = 3 . \ 3 V$ 时， $0 { \leqslant } 0 _ { s w } { \leqslant } 2 V _ { o }$

# 第 5章 封装及订货信息

芯片封装  

<table><tr><td>封装形式</td><td>塑体尺寸</td><td colspan="2">引脚节距</td><td>封装说明</td><td>订货型号</td></tr><tr><td>LQFP48</td><td>7*7mm</td><td>0.5mm</td><td>19.7mil</td><td>LQFP48(7*7)贴片</td><td>CH32V303CBT6</td></tr><tr><td>LQFP64M</td><td>10*10mm</td><td>0.5mm</td><td>19.7mil</td><td>LQFP64M(10*10)贴片</td><td>CH32V303RBT6</td></tr><tr><td>LQFP64M</td><td>10*10mm</td><td>0.5mm</td><td>19.7mil</td><td>LQFP64M(10*10)贴片</td><td>CH32V303RCT6</td></tr><tr><td>LQFP64M</td><td>10*10mm</td><td>0.5mm</td><td>19.7mil</td><td>LQFP64M(10*10)贴片</td><td>CH32V303RCT7</td></tr><tr><td>LQFP100</td><td>14*14mm</td><td>0.5mm</td><td>19.7mil</td><td>LQFP100(14*14)贴片</td><td>CH32V303VCT6</td></tr><tr><td>TSSOP20</td><td>4.4*6.5mm</td><td>0.65mm</td><td>25.6mil</td><td>薄小型的20脚贴片</td><td>CH32V305FBP6</td></tr><tr><td>QFN28</td><td>4*4mm</td><td>0.4mm</td><td>15.7mil</td><td>四边无引线28脚</td><td>CH32V305GBU6</td></tr><tr><td>LQFP48</td><td>7*7mm</td><td>0.5mm</td><td>19.7mil</td><td>LQFP48(7*7)贴片</td><td>CH32V305CCT6</td></tr><tr><td>LQFP64M</td><td>10*10mm</td><td>0.5mm</td><td>19.7mil</td><td>LQFP64M(10*10)贴片</td><td>CH32V305RBT6</td></tr><tr><td>LQFP64M</td><td>10*10mm</td><td>0.5mm</td><td>19.7mil</td><td>LQFP64M(10*10)贴片</td><td>CH32V307RCT6</td></tr><tr><td>QFN68</td><td>8*8mm</td><td>0.4mm</td><td>15.7mil</td><td>四边无引线68脚</td><td>CH32V307WCU6</td></tr><tr><td>LQFP100</td><td>14*14mm</td><td>0.5mm</td><td>19.7mil</td><td>LQFP100(14*14)贴片</td><td>CH32V307VCT6</td></tr><tr><td>QFN68</td><td>8*8mm</td><td>0.4mm</td><td>15.7mil</td><td>四边无引线68脚</td><td>CH32V317WCU6</td></tr><tr><td>LQFP100</td><td>14*14mm</td><td>0.5mm</td><td>19.7mil</td><td>LQFP100(14*14)贴片</td><td>CH32V317VCT6</td></tr></table>

说明：尺寸标注的单位是 mm（毫米），引脚中心间距总是标称值，没有误差，除此之外的尺寸误差不大于 $\pm 0 . 2 \mathsf { m m }$ 或者± $10 \%$ 两者中的较大值。

# 5.1 TSSOP20 封装

![](images/b0d0d6e5f0b777e4514d79c2f1e59093ef076798be7d989fb81abe66c0e83683.jpg)

![](images/b5ee21941e5876975555df8e91f65c041f9fecc2e712d0f6c7531a66e8f965a4.jpg)

![](images/acbc446e9b600a0dd2ccfe362a2df32cae1484912bd8e37d33aeb5598913e24e.jpg)

# 5.2 QFN28 封装

![](images/37809d3ebbc24738f6f3d9390ffb7bdd3dc815098e8455a9004b1a7680837fb6.jpg)

![](images/4af62b91c8936f0f63639d402725d47062663c20c4d45fbd6ad9be514947fbd1.jpg)

![](images/206b8639c01364a4c08b9dfc449f87617555c0b2522217334ed9f331d0a3e453.jpg)

# 5.3 LQFP48 封装

![](images/aa13824dde527eec2a02a3ec817315a7c5011cc4d10295a182e4a517a71b5b27.jpg)

# 5.4 LQFP64M 封装

![](images/1feb2bbcf06d036dbb58e4c336112ad7cc0077e2116f423f0534fd8429653326.jpg)

![](images/cc0c4ff41d0f484bc8560bbe7291f0c5be62708e9a2f1451da94aa03fd116f79.jpg)

# 5.5 QFN68 封装

![](images/9ba955e1753ed77da99376f971b239b9dc531a798049f71f62c5d228103bcd3f.jpg)

![](images/6fddf110d3eb237109b3c8b3ea68ed413aa4f9e13fa1f8d4c0fa67cf37c37f75.jpg)

![](images/243447ed8d3dd4b7df66e4c9432b26675fc58c1c3833938e0b10334152f8d2bb.jpg)

# 5.6 LQFP100 封装

![](images/88712b2b540735a2000c64298c4beca3b8287d4d6b938e7e07dd138c8310fc22.jpg)

![](images/071a1cfa02c035e122f20139337c89180d7959ff1e805ec7bb4718db665663e0.jpg)

# 系列产品命名规则

<table><tr><td>举例: CH32 V 303 R 8 T 6</td></tr><tr><td>产品系列</td></tr><tr><td>F = Arm 内核,通用MCU</td></tr><tr><td>V = 青稞RISC-V内核,通用MCU</td></tr><tr><td>L = 青稞RISC-V内核,低功耗MCU</td></tr><tr><td>X = 青稞RISC-V内核,专用或特殊外设MCU</td></tr><tr><td>M = 青稞RISC-V内核,内置预驱的电机MCU</td></tr><tr><td>产品类型(*) +产品子系列(**)</td></tr></table>

<table><tr><td>产品类型</td><td>产品子系列</td></tr><tr><td>0 = 青稞 V2/V4 内核,超值版,主频&lt;=48M</td><td>03 = 16K 闪存基础通用型,OPA06 = 64K 闪存多能通用型,OPA、双串口、TKey35 = 连接型,USB、USB PD/Type-C</td></tr><tr><td>1 = M3/青稞 V3/V4 内核,基本版,主频&lt;=96M</td><td>03 = 连接型,USB05 = 连接型,USB HS、SDIO、CAN</td></tr><tr><td>2 = M3/青稞 V4 非浮点内核,增强版,主频&lt;=144M</td><td>07 = 互联型,USB HS、CAN、以太网、SDIO、FSMC08 = 无线型,BLE5.x、CAN、USB、以太网</td></tr><tr><td>3 = 青稞 V4F 浮点内核,增强版,主频&lt;=144M</td><td>17 = 互联型,USB HS、CAN、以太网(10/100M PHY)、SDIO</td></tr></table>

# 引脚数目

<table><tr><td>J = 8 脚</td><td>D = 12 脚</td><td>A = 16 脚</td><td>F = 20 脚</td><td>E = 24 脚</td></tr><tr><td>G = 28 脚</td><td>K = 32 脚</td><td>T = 36 脚</td><td>S = 44&amp;46 脚</td><td>C = 48 脚</td></tr><tr><td>R = 64 脚</td><td>W = 68 脚</td><td>V = 100 脚</td><td>Z = 144 脚</td><td></td></tr></table>

# 闪存存储容量

<table><tr><td>4 = 16K 闪存存储器</td><td>6 = 32K 闪存存储器</td><td>7 = 48K 闪存存储器</td></tr><tr><td>8 = 64K 闪存存储器</td><td>B = 128K 闪存存储器</td><td>C = 256K 闪存存储器</td></tr></table>

# 封装

<table><tr><td>T = LQFP</td><td>U = QFN</td><td>R = QSOP</td><td>P = TSSOP</td><td>M = SOP</td></tr></table>

# 温度范围

$$
6 = - 4 0 ^ {\circ} \mathrm {C} \sim 8 5 ^ {\circ} \mathrm {C} \quad 7 = - 4 0 ^ {\circ} \mathrm {C} \sim 1 0 5 ^ {\circ} \mathrm {C}
$$

$$
3 = - 4 0 ^ {\circ} \mathrm {C} \sim 1 2 5 ^ {\circ} \mathrm {C} \quad \mathrm {D} = - 4 0 ^ {\circ} \mathrm {C} \sim 1 5 0 ^ {\circ} \mathrm {C}
$$
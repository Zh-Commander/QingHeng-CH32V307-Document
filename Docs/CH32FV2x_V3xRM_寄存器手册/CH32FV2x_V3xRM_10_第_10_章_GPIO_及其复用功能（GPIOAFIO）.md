# 第 10 章 GPIO 及其复用功能（GPIO/AFIO）

本章模块描述适用于CH32F2x、CH32V2x和CH32V3x微控制器全系列产品。

GPIO口可以配置成多种输入或输出模式，内置可关闭的上拉或下拉电阻，可以配置成推挽或开漏功能。GPIO口还可以复用成其他功能。

## 10.1 主要特征

端口的每个引脚都可以配置成以下的多种模式之一：

浮空输入  
开漏输出  
上拉输入  
推挽输出  
下拉输入  
复用功能的输入和输出  
模拟输入

许多引脚拥有复用功能，很多其他的外设把自己的输出和输入通道映射到这些引脚上，这些复用引脚具体用法需要参照各个外设，而对这些引脚是否复用和是否重映射的内容由本章说明。

## 10.2 功能描述

### 10.2.1 概述

![](images/48d34e319722daec5af78aeac02f72cd53f5e87e05343d4579211a0fe9e877c4.jpg)  
图 10-1 GPIO 模块基本结构框图

如图 10-1 所示 IO 口结构，每个引脚在芯片内部都有两只保护二极管，IO 口内部可分为输入和输出驱动模块。其中输入驱动有弱上下拉电阻可选，可连接到 AD 等模拟输入的外设；如果输入到数字外设，就需要经过一个 TTL 施密特触发器，再连接到 GPIO 输入寄存器或其他复用外设。而输出驱动有一对 MOS 管，可通过配置上下的 MOS 管是否使能来将 IO 口配置成开漏或推挽输出；输出驱动内部也可以配置成由GPIO控制输出还是由复用的其他外设控制输出。

### 10.2.2 GPIO 的初始化功能

刚复位后，GPIO 口运行在初始状态，这时大多数 IO 口都是运行在浮空输入状态，但也有 HSE 等外设相关的引脚是运行在外设复用的功能上。具体的初始化功能请参照引脚描述相关的章节。

### 10.2.3 外部中断

所有的 GPIO 口都可以被配置外部中断输入通道，但一个外部中断输入通道最多只能映射到一个GPIO 引脚上，且外部中断通道的序号必须和 GPIO 端口的位号一致，比如 PA1（或 PB1、PC1、PD1、PE1 等）只能映射到 EXTI1 上，且 EXTI1 只能接受 PA1、PB1、PC1、PD1 或 PE1 等其中之一的映射，两方都是一对一的关系。

### 10.2.4 复用功能

使用复用功能必须要注意：

l 使用输入方向的复用功能，端口必须配置成复用输入模式，上下拉设置可根据实际需要来设置  
$\bullet$ 使用输出方向的复用功能，端口必须配置成复用输出模式，推挽或开漏可根据实际情况设置  
l 对于双向的复用功能，端口必须配置成复用输出模式，这时驱动器被配置成浮空输入模式

同一个 IO 口可能有多个外设复用到此管脚，因此为了使各个外设都有最大的发挥空间，外设的复用引脚除了默认复用引脚，还可以进行重映射，重映射到其他的引脚，避开被占用的引脚。

### 10.2.5 锁定机制

锁定机制可以锁定IO口的配置。经过特定的一个写序列后，选定的IO 引脚配置将被锁定，在下一个复位前无法更改。

### 10.2.6 输入配置

![](images/f2cd2077e1a8d94d39272a904c4f7f8e5302f0e4a51810139c1d03195fbff23a.jpg)  
图 10-2 GPIO 模块输入配置结构框图

当 IO 口配置成输入模式时，输出驱动断开，输入上下拉可选，不连接复用功能和模拟输入。在每个IO口上的数据在每个APB2时钟被采样到输入数据寄存器，读取输入数据寄存器对应位即获取了对应引脚的电平状态。

### 10.2.7 输出配置

![](images/e6246b52d49b45893198530532eb7102aa6b2894c7cc0751d47065c74f61a770.jpg)  
图10-3 GPIO模块输出配置结构框图

当 IO 口配置成输出模式时，输出驱动器中的一对 MOS 可根据需要被配置成推挽或开漏模式，不使用复用功能。输入驱动的上下拉电阻被禁用，TTL施密特触发器被激活，出现在 IO引脚上的电平将会在每个 APB2 时钟被采样到输入数据寄存器，所以读取输入数据寄存器将会得到 IO 状态，在推挽输出模式时，对输出数据寄存器的访问就会得到最后一次写入的值。

### 10.2.8 复用功能配置

![](images/30dd67e11fb95020f4c29777eb1378fc9a0ca3185d76a02e75615ef8f3f633c9.jpg)  
图10-4 GPIO模块被其他外设复用时的结构框图

在启用复用功能时，输出驱动器被使能，可以按需要被配置成开漏或推挽模式，施密特触发器也被打开，复用功能的输入和输出线都被连接，但是输出数据寄存器被断开，出现在 IO 引脚上的电平将会在每个 APB2 时钟被采样到输入数据寄存器，在开漏模式下，读取输入数据寄存器将会得到 IO 口当前状态；在推挽模式下，读取输出数据寄存器将会得到最后一次写入的值。

### 10.2.9 模拟输入配置

![](images/7ab8f4450cb3c79d9554cc219543996f8e4f372f298899d00456ca92e4b5a0fe.jpg)  
图10-5 GPIO模块作为模拟输入时的配置结构框图

在启用模拟输入时，输出缓冲器被断开，输入驱动中施密特触发器的输入被禁止以防止产生 IO口上的消耗，上下拉电阻被禁止，读取输入数据寄存器将一直为 0。

### 10.2.10 外设的 GPIO 设置

下列表格推荐了各个外设的引脚相应的 GPIO 口配置。

表 10-1 高级定时器（TIM1/8/9/10）  

<table><tr><td>TIM1/8/9/10</td><td>配置</td><td>GPIO配置</td></tr><tr><td rowspan="2">TIM1/8/9/10_CHx</td><td>输入捕获通道x</td><td>浮空输入</td></tr><tr><td>输出比较通道x</td><td>推挽复用输出</td></tr><tr><td>TIM1/8/9/10_CHxN</td><td>互补输出通道x</td><td>推挽复用输出</td></tr><tr><td>TIM1/8/9/10_BKIN</td><td>刹车输入</td><td>浮空输入</td></tr><tr><td>TIM1/8/9/10_ETR</td><td>外部触发时钟输入</td><td>浮空输入</td></tr></table>

表 10-2 通用定时器（TIM2/3/4/5）  

<table><tr><td>TIM2/3/4/5 引脚</td><td>配置</td><td>GPIO配置</td></tr><tr><td rowspan="2">TIM2/3/4/5_CHx</td><td>输入捕获通道x</td><td>浮空输入</td></tr><tr><td>输出比较通道x</td><td>推挽复用输出</td></tr><tr><td>TIM2/3/4/5_ETR</td><td>外部触发时钟输入</td><td>浮空输入</td></tr></table>

表 10-3 通用同步异步串行收发器（USART）  

<table><tr><td>USART引脚</td><td>配置</td><td>GPIO配置</td></tr><tr><td rowspan="2">USARTx_TX</td><td>全双工模式</td><td>推挽复用输出</td></tr><tr><td>半双工同步模式</td><td>推挽复用输出</td></tr><tr><td rowspan="2">USARTx_RX</td><td>全双工模式</td><td>浮空输入或带上拉输入</td></tr><tr><td>半双工同步模式</td><td>未使用</td></tr><tr><td>USARTx_CK</td><td>同步模式</td><td>推挽复用输出</td></tr><tr><td>USARTx_RTS</td><td>硬件流量控制</td><td>推挽复用输出</td></tr><tr><td>USARTx_CTS</td><td>硬件流量控制</td><td>浮空输入或带上拉输入</td></tr></table>

表 10-4 串行外设接口（SPI）模块  

<table><tr><td>SPI引脚</td><td>配置</td><td>GPIO配置</td></tr><tr><td rowspan="2">SPIx_SCK</td><td>主模式</td><td>推挽复用输出</td></tr><tr><td>从模式</td><td>浮空输入</td></tr><tr><td rowspan="4">SPIx_MOSI</td><td>全双工主模式</td><td>推挽复用输出</td></tr><tr><td>全双工从模式</td><td>浮空输入或带上拉输入</td></tr><tr><td>简单的双向数据线/主模式</td><td>推挽复用输出</td></tr><tr><td>简单的双向数据线/从模式</td><td>未使用</td></tr><tr><td rowspan="4">SPIx_MISO</td><td>全双工主模式</td><td>浮空输入或带上拉输入</td></tr><tr><td>全双工从模式</td><td>推挽复用输出</td></tr><tr><td>简单的双向数据线/主模式</td><td>未使用</td></tr><tr><td>简单的双向数据线/从模式</td><td>推挽复用输出</td></tr><tr><td rowspan="3">SPIx_NSS</td><td>硬件主或从模式</td><td>浮空、上拉或下拉输入</td></tr><tr><td>硬件主模式</td><td>推挽复用输出</td></tr><tr><td>软件模式</td><td>未使用</td></tr></table>

表 10-5 内置音频总线（I2S）模块  

<table><tr><td>12S引脚</td><td>配置</td><td>GPIO配置</td></tr><tr><td rowspan="2">12Sx_WS</td><td>主模式</td><td>推挽复用输出</td></tr><tr><td>从模式</td><td>浮空输入</td></tr><tr><td rowspan="2">12Sx_CK</td><td>主模式</td><td>推挽复用输出</td></tr><tr><td>从模式</td><td>浮空输入</td></tr><tr><td rowspan="2">12Sx_SD</td><td>发送器</td><td>推挽复用输出</td></tr><tr><td>接收器</td><td>浮空、上拉或下拉输入</td></tr><tr><td rowspan="2">12Sx_MCK</td><td>主模式</td><td>推挽复用输出</td></tr><tr><td>从模式</td><td>未使用</td></tr></table>

表 10-6 内部集成总线（I2C）模块  

<table><tr><td>I²C引脚</td><td>配置</td><td>GPIO配置</td></tr><tr><td>I²C_SCL</td><td>I²C时钟</td><td>开漏复用输出</td></tr><tr><td>I²C_SDA</td><td>I²C数据</td><td>开漏复用输出</td></tr></table>

表 10-7 控制器局域网（CAN）模块  

<table><tr><td>CAN引脚</td><td>GPIO配置</td></tr><tr><td>CANx_TX</td><td>推挽复用输出</td></tr><tr><td>CANx_RX</td><td>浮空输入或上拉输入</td></tr></table>

表 10-8 USB 全速设备（USBD）控制器  

<table><tr><td>USBD 引脚</td><td>GPIO 配置</td></tr><tr><td>USBD_DM/USBD_DP</td><td>使能了 USB 模块之后，复用 I0 口会自动连接到内部 USBD 收发器</td></tr></table>

表 10-9 USB 主机设备（USBFS）控制器  

<table><tr><td>USBFS 引脚</td><td>GPIO 配置</td></tr><tr><td>USBFS_DM/USBFS_DP</td><td>使能了 USB 模块之后，复用 I0 口会自动连接到内部 USBFS 收发</td></tr><tr><td></td><td>器</td></tr></table>

表 10-10 USB OTG_FS 控制器  

<table><tr><td>USB OTG_FS 引脚</td><td>GPIO 配置</td></tr><tr><td>OTG_FS_VBUS</td><td>模拟输入</td></tr><tr><td>OTG_FS_ID</td><td>上拉输入</td></tr><tr><td>OTG_FS_DM</td><td>由 USB 断电自动控制</td></tr><tr><td>OTG_FS_DP</td><td>由 USB 断电自动控制</td></tr></table>

表 10-11 安全数字输入输出（SDIO）模块  

<table><tr><td>SD10 引脚</td><td>配置</td><td>GPIO 配置</td></tr><tr><td>SD10_CK</td><td>时钟</td><td>推挽复用输出</td></tr><tr><td>SD10_CMD</td><td>命令</td><td>推挽复用输出</td></tr><tr><td>SD10[D7:D0]</td><td>数据</td><td>推挽复用输出</td></tr></table>

表10-12 直接存储器访问控制（FSMC）控制器  

<table><tr><td>FSMC引脚</td><td>GPIO配置</td></tr><tr><td>FSMC_A[23:16]</td><td rowspan="2">推挽复用输出</td></tr><tr><td>FSMC_D[15:0]</td></tr><tr><td>FSMC_CK</td><td>推挽复用输出</td></tr><tr><td>FSMC_NOE</td><td rowspan="2">推挽复用输出</td></tr><tr><td>FSMC_NWE</td></tr><tr><td>FSMC_NE1</td><td rowspan="2">推挽复用输出</td></tr><tr><td>FSMC_NCE2</td></tr><tr><td>FSMC_NWAIT</td><td>浮空输入或带上拉输入</td></tr><tr><td>FSMC_NBL[1:0]</td><td>推挽复用输出</td></tr></table>

表 10-13 模拟转数字转换器（ADC）及数字转模拟转换器（DAC）  

<table><tr><td>ADC/DAC引脚</td><td>GPIO配置</td></tr><tr><td>ADC/DAC</td><td>模拟输入</td></tr></table>

表 10-14 其他的 IO 功能设置  

<table><tr><td>引脚</td><td>配置功能</td><td>GPIO配置</td></tr><tr><td rowspan="2">TAMPER_RTC</td><td>RTC输出</td><td rowspan="2">硬件自动设置</td></tr><tr><td>侵入事件输入</td></tr><tr><td>MCO</td><td>时钟输出</td><td>推挽复用输出</td></tr><tr><td>EXT1</td><td>外部中断输入</td><td>浮空、上拉或下拉输入</td></tr></table>

### 10.2.11 复用功能重映射 GPIO 设置

#### 10.2.11.1 OSC32_IN/OSC32_OUT 作为 GPIO 端口 PC14/PC15

当 LSEON=0 时，LSE 振荡器引脚 OSC32_IN/OSC32_OUT 可以分别用做 GPIO 的 PC14/PC15。

当 $\mathsf { L S E O N } = 1$ 时，作为 LSE 引脚。

#### 10.2.11.2 OSC_IN/OSC_OUT 引脚作为 GPIO 端口 PD0/PD1

OSC_IN/OSC_OUT 可以用做 GPIO 的 PD0/PD1，通过设置重映射寄存器 1（AFIO_PCFR1）实现。

这个重映射只适用于 20、32、48 和 64 脚的封装,但 CH32V203RBT6 只有 OSC_IN 和 OSC_OUT 功能脚,不支持映射（对于 LQFP100 封装，由于 PD0 和 PD1 为固有的功能引脚，因此没有必要 再由软件进行重映像设置。）

#### 10.2.11.3 定时器复用功能重映射

表 10-15 TIM1 复用功能重映射  

<table><tr><td>复用功能</td><td>TIM1_RM=00
默认映射</td><td>TIM1_RM=01
部分映射</td><td>TIM1_RM=11
完全映射(1)</td></tr><tr><td>TIM1_ETR</td><td>PA12</td><td>PA12</td><td>PE7</td></tr><tr><td>TIM1_CH1</td><td>PA8</td><td>PA8</td><td>PE9</td></tr><tr><td>TIM1_CH2</td><td>PA9</td><td>PA9</td><td>PE11</td></tr><tr><td>TIM1_CH3</td><td>PA10</td><td>PA10</td><td>PE13</td></tr><tr><td>TIM1_CH4</td><td>PA11</td><td>PA11</td><td>PE14</td></tr><tr><td>TIM1_BKIN</td><td>PB12</td><td>PA6</td><td>PE15</td></tr><tr><td>TIM1_CH1N</td><td>PB13</td><td>PA7</td><td>PE8</td></tr><tr><td>TIM1_CH2N</td><td>PB14</td><td>PBO</td><td>PE10</td></tr><tr><td>TIM1_CH3N</td><td>PB15</td><td>PB1</td><td>PE12</td></tr></table>

注：（1）仅LQFP100封装，支持该位重映射功能。

表 10-16 TIM2 复用功能重映射  

<table><tr><td>复用功能</td><td>TIM2_RM=00
默认映射</td><td>TIM2_RM=01
部分映射</td><td>TIM2_RM=10
部分映射</td><td>TIM2_RM=11
完全映射</td></tr><tr><td>TIM2_ETR</td><td>PA0</td><td>PA15</td><td>PA0</td><td>PA15</td></tr><tr><td>TIM2_CH1</td><td>PA0</td><td>PA15</td><td>PA0</td><td>PA15</td></tr><tr><td>TIM2_CH2</td><td>PA1</td><td>PB3</td><td>PA1</td><td>PB3</td></tr><tr><td>TIM2_CH3</td><td>PA2</td><td>PA2</td><td>PB10</td><td>PB10</td></tr><tr><td>TIM2_CH4</td><td>PA3</td><td>PA3</td><td>PB11</td><td>PB11</td></tr></table>

表 10-17 TIM3 复用功能重映射  

<table><tr><td>复用功能</td><td>TIM3_RM=00
默认映射</td><td>TIM3_RM=10
部分映射</td><td>TIM3_RM=11
完全映射(1)</td></tr><tr><td>TIM3_CH1</td><td>PA6</td><td>PB4</td><td>PC6</td></tr><tr><td>TIM3_CH2</td><td>PA7</td><td>PB5</td><td>PC7</td></tr><tr><td>TIM3_CH3</td><td>PBO</td><td>PBO</td><td>PC8</td></tr><tr><td>TIM3_CH4</td><td>PB1</td><td>PB1</td><td>PC9</td></tr></table>

注：（1）64脚以下封装不支持该位映射。

表 10-18 TIM4 复用功能重映射  

<table><tr><td>复用功能</td><td>TIM4_RM=0
默认映射</td><td>TIM4_RM=1
重映射(1)</td></tr><tr><td>TIM4_CH1</td><td>PB6</td><td>PD12</td></tr><tr><td>TIM4_CH2</td><td>PB7</td><td>PD13</td></tr><tr><td>TIM4_CH3</td><td>PB8</td><td>PD14</td></tr><tr><td>TIM4_CH4</td><td>PB9</td><td>PD15</td></tr></table>

注：（1）仅LQFP100封装，支持该位重映射功能。

表 10-19 TIM5 复用功能重映射  

<table><tr><td>复用功能</td><td>TIM5CH4_RM=0
默认映射</td><td>TIM5CH4_RM=1
重映射</td></tr><tr><td>TIM5_CH4</td><td>PA3</td><td>LSI 内部时钟</td></tr></table>

表 10-20 TIM8 复用功能重映射（1）  

<table><tr><td>复用功能</td><td>TIM8_RM=0
默认映射</td><td>TIM8_RM=1
重映射</td></tr><tr><td>TIM8_ETR</td><td>PA0</td><td>PA0</td></tr><tr><td>TIM8_CH1</td><td>PC6</td><td>PB6</td></tr><tr><td>TIM8_CH2</td><td>PC7</td><td>PB7</td></tr><tr><td>TIM8_CH3</td><td>PC8</td><td>PB8</td></tr><tr><td>TIM8_CH4</td><td>PC9</td><td>PC13</td></tr><tr><td>TIM8_BKIN</td><td>PA6</td><td>PB9</td></tr><tr><td>TIM8_CH1N</td><td>PA7</td><td>PA13</td></tr><tr><td>TIM8_CH2N</td><td>PBO</td><td>PA14</td></tr><tr><td>TIM8_CH3N</td><td>PB1</td><td>PA15</td></tr></table>

表 10-21 TIM9 复用功能重映射  

<table><tr><td>复用功能</td><td>TIM9_RM=00
默认映射</td><td>TIM9_RM=01
部分映射</td><td>TIM9_RM=1x
完全映射(1)</td></tr><tr><td>TIM9_ETR</td><td>PA2</td><td>PA2</td><td>PD9</td></tr><tr><td>TIM9_CH1</td><td>PA2</td><td>PA2</td><td>PD9</td></tr><tr><td>TIM9_CH2</td><td>PA3</td><td>PA3</td><td>PD11</td></tr><tr><td>TIM9_CH3</td><td>PA4</td><td>PA4</td><td>PD13</td></tr><tr><td>TIM9_CH4</td><td>PC4</td><td>PC14</td><td>PD15</td></tr><tr><td>TIM9_BKIN</td><td>PC5</td><td>PA1</td><td>PD14</td></tr><tr><td>TIM9_CH1N</td><td>PC0</td><td>PBO</td><td>PD8</td></tr><tr><td>TIM9_CH2N</td><td>PC1</td><td>PB1</td><td>PD10</td></tr><tr><td>TIM9_CH3N</td><td>PC2</td><td>PB2</td><td>PD12</td></tr></table>

注：（1）仅LQFP100封装，支持该位重映射功能。

表 10-22 TIM10 复用功能重映射  

<table><tr><td>复用功能</td><td>TIM10_RM=00
默认映射</td><td>TIM10_RM=01
部分映射</td><td>TIM10_RM=1x
完全映射(1)</td></tr><tr><td>TIM10_ETR</td><td>PC10</td><td>PB11</td><td>PD0</td></tr><tr><td>TIM10_CH1</td><td>PB8</td><td>PB3</td><td>PD1</td></tr><tr><td>TIM10_CH2</td><td>PB9</td><td>PB4</td><td>PD3</td></tr><tr><td>TIM10_CH3</td><td>PC3</td><td>PB5</td><td>PD5</td></tr><tr><td>TIM10_CH4</td><td>PC11</td><td>PC15</td><td>PD7</td></tr><tr><td>TIM10_BKIN</td><td>PC12</td><td>PB10</td><td>PE2</td></tr><tr><td>TIM10_CH1N</td><td>PA12</td><td>PA5</td><td>PE3</td></tr><tr><td>TIM10_CH2N</td><td>PA13</td><td>PA6</td><td>PE4</td></tr><tr><td>TIM10_CH3N</td><td>PA14</td><td>PA7</td><td>PE5</td></tr></table>

注：（1）仅LQFP100封装，支持该位重映射功能。

#### 10.2.11.4 USART、UART 复用功能重映射

表 10-23 USART1 复用功能重映射  

<table><tr><td rowspan="3">复用功能</td><td>USART1_RM1=0 (1)</td><td>USART1_RM1=1 (1)</td><td>USART1_RM1=0 (1)</td><td>USART1_RM1=1 (1)</td></tr><tr><td>USART1_RM2=0 (2)</td><td>USART1_RM2=0 (2)</td><td>USART1_RM2=1 (2)</td><td>USART1_RM2=1 (2)</td></tr><tr><td>默认映射</td><td>重映射</td><td>重映射</td><td>重映射</td></tr><tr><td>USART1_CK</td><td>PA8</td><td>PA8</td><td>PA10</td><td>PA5</td></tr><tr><td>USART1_TX</td><td>PA9</td><td>PB6</td><td>PB15</td><td>PA6</td></tr><tr><td>USART1_RX</td><td>PA10</td><td>PB7</td><td>PA8</td><td>PA7</td></tr><tr><td>USART1_CTS</td><td>PA11</td><td>PA11</td><td>PA5</td><td>PC4</td></tr><tr><td>USART1_RTS</td><td>PA12</td><td>PA12</td><td>PA9</td><td>PC5</td></tr></table>

注：（1）USART1_RM1为AFI0_PCFR1寄存器bit2，为映射配置低位。  
（2）USART1_RM2为AFI0_PCFR2寄存器 bit26，为映射配置高位，CH32V20x_D6、CH32F20x_D6不支持该位映射。

表 10-24 USART2 复用功能重映射  

<table><tr><td>复用功能</td><td>USART2_RM=0
默认映射</td><td>USART2_RM=1
重映射(1)</td></tr><tr><td>USART2_CTS</td><td>PA0</td><td>PD3</td></tr><tr><td>USART2_RTS</td><td>PA1</td><td>PD4</td></tr><tr><td>USART2_TX</td><td>PA2</td><td>PD5</td></tr><tr><td>USART2_RX</td><td>PA3</td><td>PD6</td></tr><tr><td>USART2_CK</td><td>PA4</td><td>PD7</td></tr></table>

表 10-25 USART3 复用功能重映射  

<table><tr><td>复用功能</td><td>USART3_RM=00
默认映射</td><td>USART3_RM=01
部分映射(1)</td><td>USART3_RM=10
部分映射(2)</td><td>USART3_RM=11
完全映射(3)</td></tr><tr><td>USART3_TX</td><td>PB10</td><td>PC10</td><td>PA13</td><td>PD8</td></tr><tr><td>USART3_RX</td><td>PB11</td><td>PC11</td><td>PA14</td><td>PD9</td></tr><tr><td>USART3_CK</td><td>PB12</td><td>PC12</td><td>PD10</td><td>PD10</td></tr><tr><td>USART3_CTS</td><td>PB13</td><td>PB13</td><td>PD11</td><td>PD11</td></tr><tr><td>USART3_RTS</td><td>PB14</td><td>PB14</td><td>PD12</td><td>PD12</td></tr></table>

注：（1）48脚以下封装不支持该位映射。  
（2）批号第五位小于2的不支持该功能。  
（3）仅LQFP100封装，支持该位重映射功能。

表 10-26 UART4 复用功能重映射  

<table><tr><td>复用功能(1)</td><td>UART4_RM=00
默认映射</td><td>UART4_RM=01
重映射</td><td>UART4_RM=1x
重映射(2)</td></tr><tr><td>UART4_TX</td><td>PC10</td><td>PBO</td><td>PE0</td></tr><tr><td>UART4_RX</td><td>PC11</td><td>PB1</td><td>PE1</td></tr></table>

注：（1）适用于 CH32V30x_D8C、CH32V30x_D8、CH32V30x_D8W、CH32V20x_D8、CH32F20x_D8C、CH32F20x_D8、CH32F20x_D8W。

（2）仅LQFP100封装，支持该位重映射功能。

表 10-27 USART4 复用功能重映射（ 2）  

<table><tr><td>复用功能</td><td>USART4_RM=x0
默认映射</td><td>USART4_RM=x1
重映射</td></tr><tr><td>USART4_CK</td><td>PB2</td><td>PA6</td></tr><tr><td>USART4_TX</td><td>PBO</td><td>PA5</td></tr><tr><td>USART4_RX</td><td>PB1</td><td>PB5</td></tr><tr><td>USART4_CTS</td><td>PB3</td><td>PA7</td></tr><tr><td>USART4_RTS</td><td>PB4</td><td>PA15</td></tr></table>

注：（1）仅LQFP100封装，支持该位重映射功能。  
（2）该功能不支持64引脚以下产品。

表 10-28 UART5 复用功能重映射（2）  

<table><tr><td>复用功能</td><td>UART5_RM=00
默认映射</td><td>UART5_RM=01
重映射</td><td>UART5_RM=1x
重映射(1)</td></tr><tr><td>UART5_TX</td><td>PC12</td><td>PB4</td><td>PE8</td></tr><tr><td>UART5_RX</td><td>PD2</td><td>PB5</td><td>PE9</td></tr></table>

注：（1）仅LQFP100封装，支持该位重映射功能。  
（2）该功能不支持64引脚以下产品。

表 10-29 UART6 复用功能重映射（2）  

<table><tr><td>复用功能</td><td>UART6_RM=00
默认映射(2)</td><td>UART6_RM=01
重映射</td><td>UART6_RM=1x
重映射(1)</td></tr><tr><td>UART6_TX</td><td>PC0</td><td>PB8</td><td>PE10</td></tr><tr><td>UART6_RX</td><td>PC1</td><td>PB9</td><td>PE11</td></tr></table>

注：（1）仅LQFP100封装，支持该位重映射功能。  
（2）该功能不支持64引脚以下产品。

表 10-30 UART7 复用功能重映射 （2）  

<table><tr><td>复用功能</td><td>UART7_RM=00
默认映射</td><td>UART7_RM=01
重映射</td><td>UART7_RM=1x
重映射(1)</td></tr><tr><td>UART7_TX</td><td>PC2</td><td>PA6</td><td>PE12</td></tr><tr><td>UART7_RX</td><td>PC3</td><td>PA7</td><td>PE13</td></tr></table>

注：（1）仅LQFP100封装，支持该位重映射功能。  
（2）该功能不支持64引脚以下产品。

表 10-31 UART8 复用功能重映射（2）  

<table><tr><td>复用功能</td><td>UART8_RM=00
默认映射</td><td>UART8_RM=01
重映射</td><td>UART8_RM=1x
重映射(1)</td></tr><tr><td>UART8_TX</td><td>PC4</td><td>PA14</td><td>PE14</td></tr><tr><td>UART8_RX</td><td>PC5</td><td>PA15</td><td>PE15</td></tr></table>

注：（1）仅LQFP100封装，支持该位重映射功能。  
（2）该功能不支持64引脚以下产品。

#### 10.2.11.5 SPI 复用功能重映射

表 10-32 SPI1 复用功能重映射  

<table><tr><td>复用功能</td><td>SPI1_RM=0
默认映射</td><td>SPI1_RM=1
重映射</td></tr><tr><td>SPI1_NSS</td><td>PA4</td><td>PA15</td></tr><tr><td>SPI1_SCK</td><td>PA5</td><td>PB3</td></tr><tr><td>SPI1_MISO</td><td>PA6</td><td>PB4</td></tr><tr><td>SPI1_MOSI</td><td>PA7</td><td>PB5</td></tr></table>

表 10-33 SPI3 复用功能重映射  

<table><tr><td>复用功能(1)</td><td>SPI1_RM=0
默认映射</td><td>SPI1_RM=1
重映射</td></tr><tr><td>SPI3_NSS</td><td>PA15</td><td>PA4</td></tr><tr><td>SPI3_SCK</td><td>PB3</td><td>PC10</td></tr><tr><td>SPI3_MISO</td><td>PB4</td><td>PC11</td></tr><tr><td>SPI3_MOSI</td><td>PB5</td><td>PC12</td></tr></table>

#### 10.2.11.6 I2C 复用功能重映射

表 10-34 I2C1 复用功能重映射  

<table><tr><td>复用功能</td><td>I2C1_RM=0
默认映射</td><td>I2C1_RM=1
重映射</td></tr><tr><td>I2C1_SCL</td><td>PB6</td><td>PB8</td></tr><tr><td>I2C1_SDA</td><td>PB7</td><td>PB9</td></tr></table>

#### 10.2.11.7 CAN 复用功能重映射

表 10-35 CAN1 复用功能重映射  

<table><tr><td>复用功能</td><td>CAN1_RM=00
默认映射</td><td>CAN1_RM=10
重映射(1)</td><td>CAN1_RM=11
重映射(2)</td></tr><tr><td>CAN1_RX</td><td>PA11</td><td>PB8</td><td>PDO</td></tr><tr><td>CAN1_TX</td><td>PA12</td><td>PB9</td><td>PD1</td></tr></table>

注：（1）重映射不适用于48脚以下封装的芯片。  
（2）当PDO和PD1没有被重映射到OSC_IN和OSC_OUT时，重映射功能只适用于100脚的封装上。

表 10-36 CAN2 复用功能重映射  

<table><tr><td>复用功能</td><td>CAN2_RM=0
默认映射</td><td>CAN2_RM=1
重映射</td></tr><tr><td>CAN2_RX</td><td>PB12</td><td>PB5</td></tr><tr><td>CAN2_TX</td><td>PB13</td><td>PB6</td></tr></table>

#### 10.2.11.8 ADC 复用功能重映射

表10-37 ADC1外部触发注入转换复用功能重映射  

<table><tr><td>复用功能</td><td>ADC1_ETRGINJ_RM=0
默认映射</td><td>ADC1_ETRGINJ_RM=1
重映射</td></tr><tr><td>ADC1外部触发注入转换</td><td>ADC1外部触发注入转换与EXT115相连</td><td>ADC1外部触发注入转换与TIM8_CH4相连</td></tr></table>

表 10-38 ADC1 外部触发规则转换复用功能重映射  

<table><tr><td>复用功能</td><td>ADC1_ETRGREG_RM=0
默认映射</td><td>ADC1_ETRGREG_RM=1
重映射</td></tr><tr><td>ADC1外部触发规则转换</td><td>ADC1外部触发规则转换与EXT111相连</td><td>ADC1外部触发规则转换与TIM8_TRGO相连</td></tr></table>

表10-39 ADC2外部触发注入转换复用功能重映射  

<table><tr><td>复用功能</td><td>ADC2_ETRGINJ_RM=0
默认映射</td><td>ADC2_ETRGINJ_RM=1
重映射</td></tr><tr><td>ADC2外部触发注入转换</td><td>ADC2外部触发注入转换与EXT115相连</td><td>ADC2外部触发注入转换与TIM8_CH4相连</td></tr></table>

表 10-40 ADC2 外部触发规则转换复用功能重映射  

<table><tr><td>复用功能</td><td>ADC2_ETRGREG_RM=0
默认映射</td><td>ADC2_ETRGREG_RM=1
重映射</td></tr><tr><td>ADC2外部触发规则转换</td><td>ADC2外部触发规则转换与EXT111相连</td><td>ADC2外部触发规则转换与TIM8_TRGO相连</td></tr></table>

注：ADC1重映射只支持存在TIM8的产品，详细信息见相关数据手册。

#### 10.2.11.9 ETH 复用功能重映射

表 10-41 ETH 复用功能重映射  

<table><tr><td>复用功能(1)</td><td>ETH_RM=0
默认映射</td><td>ETH_RM=1
重映射(1)</td></tr><tr><td>ETH_RX_DV</td><td>PA7</td><td>PD8</td></tr><tr><td>ETH_CRS_DV</td><td>PA7</td><td>PD8</td></tr><tr><td>ETH_RXD0</td><td>PC4</td><td>PD9</td></tr><tr><td>ETH_RXD1</td><td>PC5</td><td>PD10</td></tr><tr><td>ETH_RXD2</td><td>PBO</td><td>PD11</td></tr><tr><td>ETH_RXD3</td><td>PB1</td><td>PD12</td></tr></table>

注：（1）仅LQFP100封装互联型，支持该位重映射功能。

#### 10.2.11.10 FSMC_NADV 复用功能重映射（2）

表 10-42 FSMC_NADV 复用功能重映射  

<table><tr><td>复用功能(1)</td><td>FSMCEN=1
默认映射</td><td>FSMCEN=1&amp;USBHSEN=1
&amp;R_BUS_RST_SIE=0</td></tr><tr><td>FSMC_NADV</td><td>PB7</td><td>PD2</td></tr></table>

注：（1)当FSMC_NADV=1时禁止FSMC_NADV输出。  
(2)批号第五位小于2的不支持该功能。

#### 10.2.11.11 DVP 复用功能重映射 （1）

表 10-43 DVP 复用功能重映射  

<table><tr><td>复用功能</td><td>DVPEN=1
默认映射</td><td>DVPEN=1&amp;USBHSEN=1
&amp;R_B_UC_RST_SIE=0</td></tr><tr><td>DVP_D5</td><td>PB6</td><td>PB3</td></tr></table>

注：（1）批号第五位小于2的不支持该功能。

#### 10.2.11.12 SDIO 复用功能重映射（1）

表 10-44 SDIO 复用功能重映射  

<table><tr><td>复用功能</td><td>SDIOEN=1</td><td>SDIOEN=1&amp;ETHMACEN=1</td></tr><tr><td>SDCK</td><td>PC12</td><td>PC12</td></tr><tr><td>CMD</td><td>PD2</td><td>PD2</td></tr><tr><td>SD0</td><td>PC8</td><td>PB14</td></tr><tr><td>SD1</td><td>PC9</td><td>PB15</td></tr><tr><td>SD2</td><td>PC10</td><td>PC10</td></tr><tr><td>SD3</td><td>PC11</td><td>PC11</td></tr><tr><td>SD4</td><td>PB8</td><td>PB8</td></tr><tr><td>SD5</td><td>PB9</td><td>PB9</td></tr><tr><td>SD6</td><td>PC6</td><td>PC6</td></tr><tr><td>SD7</td><td>PC7</td><td>PC7</td></tr></table>

注：（1）批号第五位小于2的不支持该功能。

## 10.3 寄存器描述

### 10.3.1 GPIO 的寄存器描述

除非特殊说明，GPIO 的寄存器必须以字的方式操作（以 32 位来操作这些寄存器）。

表 10-45 GPIO 相关寄存器列表  

<table><tr><td>名称</td><td>访问地址</td><td>描述</td><td>复位值</td></tr><tr><td>R32_GPI0A_CFGLR</td><td>0x40010800</td><td>PA端口配置寄存器低位</td><td>0x44444444</td></tr><tr><td>R32_GPI0B_CFGLR</td><td>0x40010C00</td><td>PB端口配置寄存器低位</td><td>0x44444444</td></tr><tr><td>R32_GPI0C_CFGLR</td><td>0x40011000</td><td>PC端口配置寄存器低位</td><td>0x44444444</td></tr><tr><td>R32_GPI0D_CFGLR</td><td>0x40011400</td><td>PD端口配置寄存器低位</td><td>0x44444444</td></tr><tr><td>R32_GPI0E_CFGLR</td><td>0x40011800</td><td>PE端口配置寄存器低位</td><td>0x44444444</td></tr><tr><td>R32_GPI0A_CFGHR</td><td>0x40010804</td><td>PA端口配置寄存器高位</td><td>0x44444444</td></tr><tr><td>R32_GPI0B_CFGHR</td><td>0x40010C04</td><td>PB端口配置寄存器高位</td><td>0x44444444</td></tr><tr><td>R32_GPI0C_CFGHR</td><td>0x40011004</td><td>PC端口配置寄存器高位</td><td>0x44444444</td></tr><tr><td>R32_GPI0D_CFGHR</td><td>0x40011404</td><td>PD端口配置寄存器高位</td><td>0x44444444</td></tr><tr><td>R32_GPI0E_CFGHR</td><td>0x40011804</td><td>PE端口配置寄存器高位</td><td>0x44444444</td></tr><tr><td>R32_GPI0A_INDR</td><td>0x40010808</td><td>PA端口输入数据寄存器</td><td>0x0000XXX</td></tr><tr><td>R32_GPIOB_INDR</td><td>0x40010C08</td><td>PB端口输入数据寄存器</td><td>0x0000XXX</td></tr><tr><td>R32_GPIOC_INDR</td><td>0x40011008</td><td>PC端口输入数据寄存器</td><td>0x0000XXX</td></tr><tr><td>R32_GPIOD_INDR</td><td>0x40011408</td><td>PD端口输入数据寄存器</td><td>0x0000XXX</td></tr><tr><td>R32_GPIOE_INDR</td><td>0x40011808</td><td>PE端口输入数据寄存器</td><td>0x0000XXX</td></tr><tr><td>R32_GPIOA_OUTDR</td><td>0x4001080C</td><td>PA端口输出数据寄存器</td><td>0x0000000</td></tr><tr><td>R32_GPIOB_OUTDR</td><td>0x40010C0C</td><td>PB端口输出数据寄存器</td><td>0x0000000</td></tr><tr><td>R32_GPIOC_OUTDR</td><td>0x4001100C</td><td>PC端口输出数据寄存器</td><td>0x0000000</td></tr><tr><td>R32_GPIOD_OUTDR</td><td>0x4001140C</td><td>PD端口输出数据寄存器</td><td>0x0000000</td></tr><tr><td>R32_GPIOE_OUTDR</td><td>0x4001180C</td><td>PE端口输出数据寄存器</td><td>0x0000000</td></tr><tr><td>R32_GPIOA_BSHR</td><td>0x40010810</td><td>PA端口置位/复位寄存器</td><td>0x0000000</td></tr><tr><td>R32_GPIOB_BSHR</td><td>0x40010C10</td><td>PB端口置位/复位寄存器</td><td>0x0000000</td></tr><tr><td>R32_GPIOC_BSHR</td><td>0x40011010</td><td>PC端口置位/复位寄存器</td><td>0x0000000</td></tr><tr><td>R32_GPIOD_BSHR</td><td>0x40011410</td><td>PD端口置位/复位寄存器</td><td>0x0000000</td></tr><tr><td>R32_GPIOE_BSHR</td><td>0x40011810</td><td>PE端口置位/复位寄存器</td><td>0x0000000</td></tr><tr><td>R32_GPIOA_BCR</td><td>0x40010814</td><td>PA端口复位寄存器</td><td>0x0000000</td></tr><tr><td>R32_GPIOB_BCR</td><td>0x40010C14</td><td>PB端口复位寄存器</td><td>0x0000000</td></tr><tr><td>R32_GPIOC_BCR</td><td>0x40011014</td><td>PC端口复位寄存器</td><td>0x0000000</td></tr><tr><td>R32_GPIOD_BCR</td><td>0x40011414</td><td>PD端口复位寄存器</td><td>0x0000000</td></tr><tr><td>R32_GPIOE_BCR</td><td>0x40011814</td><td>PE端口复位寄存器</td><td>0x0000000</td></tr><tr><td>R32_GPIOA_LCKR</td><td>0x40010818</td><td>PA端口锁定配置寄存器</td><td>0x0000000</td></tr><tr><td>R32_GPIOB_LCKR</td><td>0x40010C18</td><td>PB端口锁定配置寄存器</td><td>0x0000000</td></tr><tr><td>R32_GPIOC_LCKR</td><td>0x40011018</td><td>PC端口锁定配置寄存器</td><td>0x0000000</td></tr><tr><td>R32_GPIOD_LCKR</td><td>0x40011418</td><td>PD端口锁定配置寄存器</td><td>0x0000000</td></tr><tr><td>R32_GPIOE_LCKR</td><td>0x40011818</td><td>PE端口锁定配置寄存器</td><td>0x0000000</td></tr></table>

#### 10.3.1.1 GPIO 配置寄存器低位（GPIOx_CFGLR）（x=A/B/C/D/E）

偏移地址： $0 \times 0 0$

<table><tr><td>31</td><td>30</td><td>29</td><td>28</td><td>27</td><td>26</td><td>25</td><td>24</td><td>23</td><td>22</td><td>21</td><td>20</td><td>19</td><td>18</td><td>17</td><td>16</td></tr><tr><td colspan="2">CNF7[1:0]</td><td colspan="2">MODE7[1:0]</td><td colspan="2">CNF6[1:0]</td><td colspan="2">MODE6[1:0]</td><td colspan="2">CNF5[1:0]</td><td colspan="2">MODE5[1:0]</td><td colspan="2">CNF4[1:0]</td><td colspan="2">MODE4[1:0]</td></tr><tr><td>15</td><td>14</td><td>13</td><td>12</td><td>11</td><td>10</td><td>9</td><td>8</td><td>7</td><td>6</td><td>5</td><td>4</td><td>3</td><td>2</td><td>1</td><td>0</td></tr><tr><td colspan="2">CNF3[1:0]</td><td colspan="2">MODE3[1:0]</td><td colspan="2">CNF2[1:0]</td><td colspan="2">MODE2[1:0]</td><td colspan="2">CNF1[1:0]</td><td colspan="2">MODE1[1:0]</td><td colspan="2">CNF0[1:0]</td><td colspan="2">MODE0[1:0]</td></tr></table>

<table><tr><td>位</td><td>名称</td><td>访问</td><td>描述</td><td>复位值</td></tr><tr><td>[31:30]</td><td rowspan="8">CNFy[1:0]</td><td rowspan="8">RW</td><td rowspan="8">(y=0-7),端口x的配置位,通过这些位配置相应的端口。在输入模式时(MODE=00b):00:模拟输入模式;01:浮空输入模式;10:带有上下拉模式。11:保留。在输出模式(MODE&gt;00b):00:通用推挽输出模式;01:通用开漏输出模式;</td><td rowspan="8">01b</td></tr><tr><td>[27:26]</td></tr><tr><td>[23:22]</td></tr><tr><td>[19:18]</td></tr><tr><td>[15:14]</td></tr><tr><td>[11:10]</td></tr><tr><td>[7:6]</td></tr><tr><td>[3:2]</td></tr><tr><td></td><td></td><td></td><td>10: 复用功能推挽输出模式;11: 复用功能开漏输出模式。</td><td></td></tr><tr><td>[29:28] [25:24] [21:20] [17:16] [13:12] [9:8] [5:4] [1:0]</td><td>MODEy[1:0]</td><td>RW</td><td>(y=0-7),端口x模式选择,通过这些位配置相应的端口。00:输入模式;01:输出模式,最大速度10MHz;10:输出模式,最大速度2MHz;11:输出模式,最大速度50MHz。</td><td>00b</td></tr></table>

#### 10.3.1.2 GPIO 配置寄存器高位（GPIOx_CFGHR）（x=A/B/C/D/E）

偏移地址： $0 \times 0 4$

<table><tr><td>31</td><td>30</td><td>29</td><td>28</td><td>27</td><td>26</td><td>25</td><td>24</td><td>23</td><td>22</td><td>21</td><td>20</td><td>19</td><td>18</td><td>17</td><td>16</td></tr><tr><td colspan="2">CNF15[1:0]</td><td colspan="2">MODE15[1:0]</td><td colspan="2">CNF14[1:0]</td><td colspan="2">MODE14[1:0]</td><td colspan="2">CNF13[1:0]</td><td colspan="2">MODE13[1:0]</td><td colspan="2">CNF12[1:0]</td><td colspan="2">MODE12[1:0]</td></tr><tr><td>15</td><td>14</td><td>13</td><td>12</td><td>11</td><td>10</td><td>9</td><td>8</td><td>7</td><td>6</td><td>5</td><td>4</td><td>3</td><td>2</td><td>1</td><td>0</td></tr><tr><td colspan="2">CNF11[1:0]</td><td colspan="2">MODE11[1:0]</td><td colspan="2">CNF10[1:0]</td><td colspan="2">MODE10[1:0]</td><td colspan="2">CNF9[1:0]</td><td colspan="2">MODE9[1:0]</td><td colspan="2">CNF8[1:0]</td><td colspan="2">MODE8[1:0]</td></tr></table>

<table><tr><td>位</td><td>名称</td><td>访问</td><td>描述</td><td>复位值</td></tr><tr><td>[31:30]</td><td rowspan="8">CNFy[1:0]</td><td rowspan="8">RW</td><td rowspan="8">(y=8-15),端口x的配置位,通过这些位配置相应的端口。在输入模式时(MODE=00b):00:模拟输入模式;01:浮空输入模式;10:带有上下拉模式。11:保留。在输出模式(MODE&gt;00b):00:通用推挽输出模式;01:通用开漏输出模式;10:复用功能推挽输出模式;11:复用功能开漏输出模式。</td><td rowspan="8">01b</td></tr><tr><td>[27:26]</td></tr><tr><td>[23:22]</td></tr><tr><td>[19:18]</td></tr><tr><td>[15:14]</td></tr><tr><td>[11:10]</td></tr><tr><td>[7:6]</td></tr><tr><td>[3:2]</td></tr><tr><td>[29:28]</td><td rowspan="8">MODEy[1:0]</td><td rowspan="8">RW</td><td rowspan="8">(y=8-15),端口x的模式位,通过这些位配置相应的端口。00:输入模式;01:输出模式,最大速度10MHz;10:输出模式,最大速度2MHz;11:输出模式,最大速度50MHz。</td><td rowspan="8">00b</td></tr><tr><td>[25:24]</td></tr><tr><td>[21:20]</td></tr><tr><td>[17:16]</td></tr><tr><td>[13:12]</td></tr><tr><td>[9:8]</td></tr><tr><td>[5:4]</td></tr><tr><td>[1:0]</td></tr></table>

#### 10.3.1.3 端口输入寄存器（GPIOx_INDR）（x=A/B/C/D/E）

偏移地址： $0 \times 0 8$

<table><tr><td>31</td><td>30</td><td>29</td><td>28</td><td>27</td><td>26</td><td>25</td><td>24</td><td>23</td><td>22</td><td>21</td><td>20</td><td>19</td><td>18</td><td>17</td><td>16</td></tr><tr><td colspan="16">Reserved</td></tr><tr><td>15</td><td>14</td><td>13</td><td>12</td><td>11</td><td>10</td><td>9</td><td>8</td><td>7</td><td>6</td><td>5</td><td>4</td><td>3</td><td>2</td><td>1</td><td>0</td></tr><tr><td>IDR15</td><td>IDR14</td><td>IDR13</td><td>IDR12</td><td>IDR11</td><td>IDR10</td><td>IDR9</td><td>IDR8</td><td>IDR7</td><td>IDR6</td><td>IDR5</td><td>IDR4</td><td>IDR3</td><td>IDR2</td><td>IDR1</td><td>IDR0</td></tr></table>

<table><tr><td>位</td><td>名称</td><td>访问</td><td>描述</td><td>复位值</td></tr><tr><td>[31:16]</td><td>Reserved</td><td>R0</td><td>保留。</td><td>0</td></tr><tr><td>[15:0]</td><td>IDRy</td><td>R0</td><td>(y=0-15)，端口输入数据。这些位只读并只能以16位形式读出。读出的值就是对应位的高低状态。</td><td>X</td></tr></table>

#### 10.3.1.4 端口输出寄存器（GPIOx_OUTDR）（x=A/B/C/D/E）

偏移地址： $0 \times 0 0$

<table><tr><td>31</td><td>30</td><td>29</td><td>28</td><td>27</td><td>26</td><td>25</td><td>24</td><td>23</td><td>22</td><td>21</td><td>20</td><td>19</td><td>18</td><td>17</td><td>16</td></tr><tr><td colspan="16">Reserved</td></tr><tr><td>15</td><td>14</td><td>13</td><td>12</td><td>11</td><td>10</td><td>9</td><td>8</td><td>7</td><td>6</td><td>5</td><td>4</td><td>3</td><td>2</td><td>1</td><td>0</td></tr><tr><td>ODR15</td><td>ODR14</td><td>ODR13</td><td>ODR12</td><td>ODR11</td><td>ODR10</td><td>ODR9</td><td>ODR8</td><td>ODR7</td><td>ODR6</td><td>ODR5</td><td>ODR4</td><td>ODR3</td><td>ODR2</td><td>ODR1</td><td>ODR0</td></tr></table>

<table><tr><td>位</td><td>名称</td><td>访问</td><td>描述</td><td>复位值</td></tr><tr><td>[31:16]</td><td>Reserved</td><td>R0</td><td>保留。</td><td>0</td></tr><tr><td>[15:0]</td><td>ODRy</td><td>RW</td><td>对于输出模式:(y=0-15),端口输出的数据。这些数据只能以16位的形式操作。10口对外输出这些寄存器的值。对于带有上下拉的输入模式:0:下拉输入;1:上拉输入。</td><td>0</td></tr></table>

#### 10.3.1.5 端口复位/置位寄存器（GPIOx_BSHR）（x=A/B/C/D/E）

偏移地址： $0 \times 1 0$

<table><tr><td>31</td><td>30</td><td>29</td><td>28</td><td>27</td><td>26</td><td>25</td><td>24</td><td>23</td><td>22</td><td>21</td><td>20</td><td>19</td><td>18</td><td>17</td><td>16</td></tr><tr><td>BR15</td><td>BR14</td><td>BR13</td><td>BR12</td><td>BR11</td><td>BR10</td><td>BR9</td><td>BR8</td><td>BR7</td><td>BR6</td><td>BR5</td><td>BR4</td><td>BR3</td><td>BR2</td><td>BR1</td><td>BR0</td></tr><tr><td>15</td><td>14</td><td>13</td><td>12</td><td>11</td><td>10</td><td>9</td><td>8</td><td>7</td><td>6</td><td>5</td><td>4</td><td>3</td><td>2</td><td>1</td><td>0</td></tr><tr><td>BS15</td><td>BS14</td><td>BS13</td><td>BS12</td><td>BS11</td><td>BS10</td><td>BS9</td><td>BS8</td><td>BS7</td><td>BS6</td><td>BS5</td><td>BS4</td><td>BS3</td><td>BS2</td><td>BS1</td><td>BS0</td></tr></table>

<table><tr><td>位</td><td>名称</td><td>访问</td><td>描述</td><td>复位值</td></tr><tr><td>[31:16]</td><td>BRy</td><td>WO</td><td>(y=0-15)，对这些位置位会清除对应的 OUTDR 位，写0不产生影响。这些位只能以16位的形式访问。如果同时设置了BR和BS位，则BS位起作用。</td><td>0</td></tr><tr><td>[15:0]</td><td>BSy</td><td>WO</td><td>(y=0-15)，对这些位置位会使对应的 OUTDR 位置位，写0不产生影响。这些位只能以16位的形式访问。如果同时设置了BR和BS位，则BS位起作用。</td><td>0</td></tr></table>

#### 10.3.1.6 端口复位寄存器（GPIOx_BCR）（x=A/B/C/D/E）

偏移地址： $0 \times 1 4$

<table><tr><td>31</td><td>30</td><td>29</td><td>28</td><td>27</td><td>26</td><td>25</td><td>24</td><td>23</td><td>22</td><td>21</td><td>20</td><td>19</td><td>18</td><td>17</td><td>16</td></tr><tr><td>15</td><td>14</td><td>13</td><td>12</td><td>11</td><td>10</td><td>9</td><td>8</td><td>7</td><td>6</td><td>5</td><td>4</td><td>3</td><td>2</td><td>1</td><td>0</td></tr><tr><td>BR15</td><td>BR14</td><td>BR13</td><td>BR12</td><td>BR11</td><td>BR10</td><td>BR9</td><td>BR8</td><td>BR7</td><td>BR6</td><td>BR5</td><td>BR4</td><td>BR3</td><td>BR2</td><td>BR1</td><td>BR0</td></tr></table>

<table><tr><td>位</td><td>名称</td><td>访问</td><td>描述</td><td>复位值</td></tr><tr><td>[31:16]</td><td>Reserved</td><td>R0</td><td>保留。</td><td>0</td></tr><tr><td>[15:0]</td><td>BRy</td><td>WO</td><td>(y=0-15)，对这些位置位会清除对应的 OUTDR 位，写0不产生影响。这些位只能以16位的形式访问。</td><td>0</td></tr></table>

#### 10.3.1.7 配置锁定寄存器（GPIOx_LCKR）（x=A/B/C/D/E）

偏移地址： $0 \times 1 8$

<table><tr><td>31</td><td>30</td><td>29</td><td>28</td><td>27</td><td>26</td><td>25</td><td>24</td><td>23</td><td>22</td><td>21</td><td>20</td><td>19</td><td>18</td><td>17</td><td>16</td></tr><tr><td colspan="15">Reserved</td><td>LCKK</td></tr><tr><td>15</td><td>14</td><td>13</td><td>12</td><td>11</td><td>10</td><td>9</td><td>8</td><td>7</td><td>6</td><td>5</td><td>4</td><td>3</td><td>2</td><td>1</td><td>0</td></tr><tr><td>LCK15</td><td>LCK14</td><td>LCK13</td><td>LCK12</td><td>LCK11</td><td>LCK10</td><td>LCK9</td><td>LCK8</td><td>LCK7</td><td>LCK6</td><td>LCK5</td><td>LCK4</td><td>LCK3</td><td>LCK2</td><td>LCK1</td><td>LCK0</td></tr></table>

<table><tr><td>位</td><td>名称</td><td>访问</td><td>描述</td><td>复位值</td></tr><tr><td>[31:17]</td><td>Reserved</td><td>R0</td><td>保留</td><td>0</td></tr><tr><td>16</td><td>LCKK</td><td>RW</td><td>锁定键,它可以通过特定的序列写入实现锁定,但它可以随时读出。它读出为0时表示未锁定生效,读出1时表示锁定生效。锁定键的写入序列为:写1-写0-写1-读0-读1,最后一步非必要,但是可以用以确认锁定键已经激活。在写入序列时任何错误都不会使激活锁定,且在写入序列时,不能更改LCK[15:0]的值。锁定生效后,只有在下次复位后才能更改端口的配置。</td><td>0</td></tr><tr><td>[15:0]</td><td>LCKy</td><td>RW</td><td>(y=0-15),这些位为1时表示锁定对应端口的配置。只能在LCKK未锁定前改变这些位。锁定的配置指的是配置寄存器GPIOx_CFGLR和GPIOx_CFGHR。</td><td>0</td></tr></table>

注：当对相应的端口位执行了LOCK序列后，在下次系统复位之前将不能再更改端口位的配置。

### 10.3.2 AFIO 寄存器

除非特殊说明，AFIO的寄存器必须以字的方式操作（以 32位来操作这些寄存器）。

表 10-46 AFIO 相关寄存器列表  

<table><tr><td>名称</td><td>访问地址</td><td>描述</td><td>复位值</td></tr><tr><td>R32_AFIO_ECR</td><td>0x40010000</td><td>事件控制寄存器</td><td>0x00000000</td></tr><tr><td>R32_AFIO_PCFR1</td><td>0x40010004</td><td>重映射寄存器1</td><td>0x00000000</td></tr><tr><td>R32_AFIO_EXTCR1</td><td>0x40010008</td><td>外部中断配置寄存器1</td><td>0x00000000</td></tr><tr><td>R32_AFIO_EXTCR2</td><td>0x4001000C</td><td>外部中断配置寄存器2</td><td>0x00000000</td></tr><tr><td>R32_AFIO_EXTCR3</td><td>0x40010010</td><td>外部中断配置寄存器3</td><td>0x00000000</td></tr><tr><td>R32_AFIO_EXTCR4</td><td>0x40010014</td><td>外部中断配置寄存器4</td><td>0x00000000</td></tr><tr><td>R32_AFIO_PCFR2</td><td>0x4001001C</td><td>重映射寄存器2</td><td>0x00000000</td></tr></table>

#### 10.3.2.1 事件控制寄存器（AFIO_ECR）

偏移地址： $0 \times 0 0$

<table><tr><td>31</td><td>30</td><td>29</td><td>28</td><td>27</td><td>26</td><td>25</td><td>24</td><td>23</td><td>22</td><td>21</td><td>20</td><td>19</td><td>18</td><td>17</td><td>16</td></tr><tr><td colspan="16">Reserved</td></tr><tr><td>15</td><td>14</td><td>13</td><td>12</td><td>11</td><td>10</td><td>9</td><td>8</td><td>7</td><td>6</td><td>5</td><td>4</td><td>3</td><td>2</td><td>1</td><td>0</td></tr><tr><td colspan="8">Reserved</td><td>EVOE</td><td colspan="3">PORT[2:0]</td><td colspan="4">PIN[3:0]</td></tr></table>

<table><tr><td>位</td><td>名称</td><td>访问</td><td>描述</td><td>复位值</td></tr><tr><td>[31:8]</td><td>Reserved</td><td>R0</td><td>保留。</td><td>0</td></tr><tr><td>7</td><td>EVOE</td><td>RW</td><td>允许事件输出位,对该位置位会使内核的EVENTOUT连接到PORT和PIN选定的I0口。</td><td>0</td></tr><tr><td>[6:4]</td><td>PORT[2:0]</td><td>RW</td><td>用于选择内核输出EVENTOUT的端口:000:选择PA口; 001:选择PB口;010:选择PC口; 011:选择PD口;其他:保留。</td><td>000b</td></tr><tr><td>[3:0]</td><td>PIN[3:0]</td><td>RW</td><td>此位的值用来确定选择内核输出EVENTOUT到端口的具体引脚号,值0-15分别对应PORT中选定的Px的第0-15号引脚。</td><td>0</td></tr></table>

#### 10.3.2.2 重映射寄存器 1（AFIO_PCFR1）

偏移地址： $0 \times 0 4$

<table><tr><td>31</td><td>30</td><td>29</td><td>28</td><td>27</td><td>26</td><td>25</td><td>24</td><td>23</td><td>22</td><td>21</td><td>20</td><td>19</td><td>18</td><td>17</td><td>16</td></tr><tr><td>Reserved</td><td>PTP_PPS_RM</td><td>TIM2_ITR1_RM</td><td>SPI3_RM</td><td>Reserved</td><td colspan="3">SW_CFG[2:0]</td><td>MII_RMII_SEL</td><td>CAN2_RM</td><td>ETHRM</td><td>ADC2_ETRGREG_RM</td><td>ADC2_ETRGRIGNJ_RM</td><td>ADC1_ETRGREGRM</td><td>ADC1_ETRGRIGNJ</td><td>TIM5C H4_RM</td></tr><tr><td>15</td><td>14</td><td>13</td><td>12</td><td>11</td><td>10</td><td>9</td><td>8</td><td>7</td><td>6</td><td>5</td><td>4</td><td>3</td><td>2</td><td>1</td><td>0</td></tr><tr><td>PDO1_RM</td><td colspan="2">CAN_RM[1:0]</td><td>TIM4_RM</td><td colspan="2">TIM3_RM[1:0]</td><td colspan="2">TIM2_RM[1:0]</td><td colspan="2">TIM1_RM[1:0]</td><td colspan="2">USART3_RM[1:0]</td><td>USART2_RM</td><td>USART1_RM</td><td>12C1_RM</td><td>SPI1_RM</td></tr></table>

<table><tr><td>位</td><td>名称</td><td>访问</td><td>描述</td><td>复位值</td></tr><tr><td>31</td><td>Reserved</td><td>R0</td><td>保留。</td><td>0</td></tr><tr><td>30</td><td>PTP_PPS_RM</td><td>RW</td><td>以太网的PTP PPS重映射。0:PTP PPS不输出到PB5引脚;1:PTP PPS输出到PB5引脚。</td><td>0</td></tr><tr><td>29</td><td>TIM2ITR1_RM</td><td>RW</td><td>TIM2内部触发1重映射。0:在内部连接TIM2_ITR1至以太网的PTP输出;1:在内部连接TIM2_ITR1至全速USBOTG的SOF输出。</td><td>0</td></tr><tr><td>28</td><td>SPI3_RM</td><td>RW</td><td>SPI3重映射。0:默认映射(NSS/PA15、SCK/PB3、MISO/PB4、MOSI/PB5);1:重映射(NSS/PA4、SCK/PC10、MISO/PC11、MOSI/PC12)。</td><td>0</td></tr><tr><td>27</td><td>Reserved</td><td>R0</td><td>保留。</td><td>0</td></tr><tr><td>[26:24]</td><td>SW_CFG[2:0]</td><td>WO</td><td>这些位用以配置SW功能和跟踪功能的IO口。SWD(SDI)是访问内核的调试接口。系统复位后总是作为SWD端口。0xx:启用SWD(SDI);100:关闭SWD(SDI),作为GPIO功能;其他:无效。</td><td>000b</td></tr><tr><td>23</td><td>MII_RMII_SEL</td><td>RW</td><td>MII或RMII选择。配置内部的以太网MAC适用外部的MII接口还是RMII接口的收发器(PHY)。0:配置以太网的MAC使用外部MII接口的收发器(PHY);1:配置以太网的MAC使用外部RMII接口的收发器(PHY);</td><td>0</td></tr><tr><td>22</td><td>CAN2_RM</td><td>RW</td><td>CAN2重映射位。0:默认映射(CAN2_RX/PB12,CAN2_TX/PB13);1:重映射(CAN2_RX/PB5,CAN2_TX/PB6)。</td><td>0</td></tr><tr><td>21</td><td>ETH_RM</td><td>RW</td><td>以太网的重映射位。0:默认映射(RX_DV-CRS_DV/PA7,RXD0/PC4,RXD1/PC5,RXD2/PB0,RXD3/PB1);</td><td>0</td></tr><tr><td></td><td></td><td></td><td>1:重映射 ( RX_DV-CRS_DV/PD8, RXD0/PD9, RXD1/PD10, RXD2/PD11, RXD3/PD12)</td><td></td></tr><tr><td>20</td><td>ADC2_ETRGREG_RM</td><td>RW</td><td>ADC2外部触发规则转换的重映射位。0:ADC1外部触发规则转换与EXT111相连;1:ADC1外部触发规则转换与TIM8_TRGO相连;</td><td>0</td></tr><tr><td>19</td><td>ADC2_ETRGINJ_RM</td><td>RW</td><td>ADC2外部触发注入转换的重映射位。0:ADC2外部触发注入转换与EXT115相连;1:ADC2外部触发注入转换与TIM8_CH4相连;</td><td>0</td></tr><tr><td>18</td><td>ADC1_ETRGREG_RM</td><td>RW</td><td>ADC1外部触发规则转换的重映射位。0:ADC1外部触发规则转换与EXT111相连;1:ADC1外部触发规则转换与TIM8_TRGO相连;</td><td>0</td></tr><tr><td>17</td><td>ADC1_ETRGINJ_RM</td><td>RW</td><td>ADC1外部触发注入转换的重映射位。0:ADC1外部触发注入转换与EXT115相连;1:ADC1外部触发注入转换与TIM8_CH4相连;</td><td>0</td></tr><tr><td>16</td><td>TIM5CH4_RM</td><td>RW</td><td>定时器5通道的重映射。0:默认映射,定时器5通道4的重映射;1:重映射,定时器5通道4映射至LSI内部时钟。</td><td>0</td></tr><tr><td>15</td><td>PD01_RM</td><td>RW</td><td>引脚PD0&amp;PD1重映射位,该位可由用户读写。它控制PD0和PD1的GPIO功能是否进行重映射,即PD0&amp;PD1映射到OSC_IN&amp;OSC_OUT。0:引脚作为晶振引脚使用;1:引脚作为GPIO口使用;</td><td>0</td></tr><tr><td>[14:13]</td><td>CAN1_RM[1:0]</td><td>RW</td><td>CAN1复用功能重映射位,这些位可由用户读写。控制CAN_RX和CAN_TX的重映射:00:CAN1_RX映射到PA11, CAN1_TX映射到PA12;10:CAN1_RX映射到PB8, CAN1_TX映射到PB9;01:保留;11:CAN1_RX映射到PD0, CAN1_TX映射到PD1;</td><td>00b</td></tr><tr><td>12</td><td>TIM4_RM</td><td>RW</td><td>定时器4的重映射位,该位可由用户读写。它控制定时器4的通道1至4在GPIO端口的重映射:0:默认映射 (CH1/PB6, CH2/PB7, CH3/PB8, CH4/PB9);1:重映射 (CH1/PD12, CH2/PD13, CH3/PD14, CH4/PD15)。</td><td>0</td></tr><tr><td>[11:10]</td><td>TIM3_RM[1:0]</td><td>RW</td><td>定时器3的重映射位,这些位可由用户读写。它控制定时器3的通道1至4在GPIO端口的重映射:00:默认映射 (CH1/PA6, CH2/PA7, CH3/PBO, CH4/PB1);01:保留;10:部分映射 (CH1/PB4, CH2/PB5, CH3/PBO, CH4/PB1);11:完全映射 (CH1/PC6, CH2/PC7, CH3/PC8,</td><td>00b</td></tr><tr><td></td><td></td><td></td><td>CH4/PC9);注:重映射不影响在PD2上的TIM3_ETR。</td><td></td></tr><tr><td>[9:8]</td><td>TIM2_RM[1:0]</td><td>RW</td><td>定时器2的重映射位。这些位可由用户读写。它控制定时器2的通道1至4和外部触发(ETR)在GPIO端口的映射:00:默认映射(ETR/PA0,CH1/PA0,CH2/PA1,CH3/PA2,CH4/PA3);01:部分映射(ETR/PA15,CH1/PA15,CH2/PB3,CH3/PA2,CH4/PA3);10:部分映射(ETR/PA0,CH1/PA0,CH2/PA1,CH3/PB10,CH4/PB11);11:完全映射(ETR/PA15,CH1/PA15,CH2/PB3,CH3/PB10,CH4/PB11)。</td><td>00b</td></tr><tr><td>[7:6]</td><td>TIM1_RM[1:0]</td><td>RW</td><td>定时器1的重映射位。这些位可由用户读写。它控制定时器1的通道1至4、1N至3N、外部触发(ETR)和刹车输入(BKIN)在GPIO端口的映射:00:默认映射(ETR/PA12,CH1/PA8,CH2/PA9,CH3/PA10,CH4/PA11,BKIN/PB12,CH1N/PB13,CH2N/PB14,CH3N/PB15);01:部分映射(ETR/PA12,CH1/PA8,CH2/PA9,CH3/PA10,CH4/PA11,BKIN/PA6,CH1N/PA7,CH2N/PB0,CH3N/PB1);10:保留;11:完全映射(ETR/PE7,CH1/PE9,CH2/PE11,CH3/PE13,CH4/PE14,BKIN/PE15,CH1N/PE8,CH2N/PE10,CH3N/PE12)。</td><td>00b</td></tr><tr><td>[5:4]</td><td>USART3_RM[1:0]</td><td>RW</td><td>USART3的重映射位,这些位可由用户读写。它控制USART3的CTS、RTS、CK、TX和RX复用功能在GPIO端口的映射:00:默认映射(TX/PB10,RX/PB11,CK/PB12,CTS/PB13,RTS/PB14);01:部分重映射(TX/PC10,RX/PC11,CK/PC12,CTS/PB13,RTS/PB14);10:部分重映射(TX/PA13,RX/PA14,CK/PD10,CTS/PD11,RTS/PD12);11:完全重映射(TX/PD8,RX/PD9,CK/PD10,CTS/PD11,RTS/PD12)。注:CH32V20x_D6、CH32F20x_D6只存在默认映射(00b)。</td><td>00b</td></tr><tr><td>3</td><td>USART2_RM</td><td>RW</td><td>USART2的重映射位。该位可由用户读写。它控制USART2的CTS、RTS、CK、TX和RX复用功能在GPIO端口的映射:0:默认映射(CTS/PA0,RTS/PA1, TX/PA2,RX/PA3,CK/PA4);1:重映射(CTS/PD3,RTS/PD4, TX/PD5,RX/PD6,</td><td>0</td></tr><tr><td></td><td></td><td></td><td>CK/PD7)。注:CH32V20x_D6、CH32F20x_D6只存在默认映射(Ob)。</td><td></td></tr><tr><td>2</td><td>USART1_RM</td><td>RW</td><td>USART1映射配置低位(配合AF10_PCFR2寄存器bit26 USART1_RM1使用)。00:默认映射(CK/PA8, TX/PA9, RX/PA10,CTS/PA11, RTS/PA12);01:重映射(CK/PA8, TX/PB6, RX/PB7,CTS/PA11, RTS/PA12);10:重映射(CK/PA10, TX/PB15, RX/PA8,CTS/PA5, RTS/PA9);11:重映射(CK/PA5, TX/PA6, RX/PA7,CTS/PC4,RTS/PC5)。注:CH32V20x_D6、CH32F20x_D6只存在默认映射(00b)、重映射(01b)。</td><td>0</td></tr><tr><td>1</td><td>I2C1_RM</td><td>RW</td><td>I2C1的重映射。该位可由用户读写。它控制I2C1的SCL和SDA复用功能在GPIO端口的映射:0:默认映射(SCL/PB6,SDA/PB7);1:重映射(SCL/PB8,SDA/PB9)。</td><td>0</td></tr><tr><td>0</td><td>SPI1_RM</td><td>RW</td><td>SPI1的重映射。该位可由用户读写。它控制SPI1的NSS、SCK、MISO和MOSI复用功能在GPIO端口的映射:0:默认映射(NSS/PA4, SCK/PA5, MISO/PA6,MOSI/PA7);1:重映射(NSS/PA15, SCK/PB3, MISO/PB4,MOSI/PB5)。</td><td>0</td></tr></table>

#### 10.3.2.3 外部中断配置寄存器 1（AFIO_EXTICR1）

偏移地址： $0 \times 0 8$

<table><tr><td>31</td><td>30</td><td>29</td><td>28</td><td>27</td><td>26</td><td>25</td><td>24</td><td>23</td><td>22</td><td>21</td><td>20</td><td>19</td><td>18</td><td>17</td><td>16</td></tr><tr><td colspan="16">Reserved</td></tr><tr><td>15</td><td>14</td><td>13</td><td>12</td><td>11</td><td>10</td><td>9</td><td>8</td><td>7</td><td>6</td><td>5</td><td>4</td><td>3</td><td>2</td><td>1</td><td>0</td></tr><tr><td colspan="4">EXT13[3:0]</td><td colspan="4">EXT12[3:0]</td><td colspan="4">EXT11[3:0]</td><td colspan="4">EXT10[3:0]</td></tr></table>

<table><tr><td>位</td><td>名称</td><td>访问</td><td>描述</td><td>复位值</td></tr><tr><td>[31:16]</td><td>Reserved</td><td>R0</td><td>保留。</td><td>0</td></tr><tr><td>[15:12]</td><td rowspan="4">EXT1x[3:0]</td><td rowspan="4">RW</td><td rowspan="4">(x=0-3),外部中断输入引脚配置位。用以决定外部中断引脚映射到哪个端口的引脚上:0000:PA引脚的第x个引脚;0001:PB引脚的第x个引脚;0010:PC引脚的第x个引脚;0011:PD引脚的第x个引脚;0100:PE引脚的第x个引脚;</td><td rowspan="4">0000b</td></tr><tr><td>[11:8]</td></tr><tr><td>[7:4]</td></tr><tr><td>[3:0]</td></tr><tr><td></td><td></td><td></td><td>其他:保留。</td><td></td></tr></table>

#### 10.3.2.4 外部中断配置寄存器 2（AFIO_EXTICR2）

偏移地址： $0 \times 0 0$

<table><tr><td>31</td><td>30</td><td>29</td><td>28</td><td>27</td><td>26</td><td>25</td><td>24</td><td>23</td><td>22</td><td>21</td><td>20</td><td>19</td><td>18</td><td>17</td><td>16</td></tr><tr><td colspan="16">Reserved</td></tr><tr><td>15</td><td>14</td><td>13</td><td>12</td><td>11</td><td>10</td><td>9</td><td>8</td><td>7</td><td>6</td><td>5</td><td>4</td><td>3</td><td>2</td><td>1</td><td>0</td></tr><tr><td colspan="4">EXT17[3:0]</td><td colspan="4">EXT16[3:0]</td><td colspan="4">EXT15[3:0]</td><td colspan="4">EXT14[3:0]</td></tr></table>

<table><tr><td>位</td><td>名称</td><td>访问</td><td>描述</td><td>复位值</td></tr><tr><td>[31:16]</td><td>Reserved</td><td>R0</td><td>保留。</td><td>0</td></tr><tr><td>[15:12]</td><td rowspan="4">EXT1x[3:0]</td><td rowspan="4">RW</td><td rowspan="4">(x=4-7),外部中断输入引脚配置位。用以决定外部中断引脚映射到哪个端口的引脚上:0000:PA引脚的第x个引脚;0001:PB引脚的第x个引脚;0010:PC引脚的第x个引脚;0011:PD引脚的第x个引脚;0100:PE引脚的第x个引脚;其他:保留。</td><td rowspan="4">0000b</td></tr><tr><td>[11:8]</td></tr><tr><td>[7:4]</td></tr><tr><td>[3:0]</td></tr></table>

#### 10.3.2.5 外部中断配置寄存器 3（AFIO_EXTICR3）

偏移地址： $0 \times 1 0$

<table><tr><td>31</td><td>30</td><td>29</td><td>28</td><td>27</td><td>26</td><td>25</td><td>24</td><td>23</td><td>22</td><td>21</td><td>20</td><td>19</td><td>18</td><td>17</td><td>16</td></tr><tr><td colspan="16">Reserved</td></tr><tr><td>15</td><td>14</td><td>13</td><td>12</td><td>11</td><td>10</td><td>9</td><td>8</td><td>7</td><td>6</td><td>5</td><td>4</td><td>3</td><td>2</td><td>1</td><td>0</td></tr><tr><td colspan="4">EXT111[3:0]</td><td colspan="4">EXT110[3:0]</td><td colspan="4">EXT19[3:0]</td><td colspan="4">EXT18[3:0]</td></tr></table>

<table><tr><td>位</td><td>名称</td><td>访问</td><td>描述</td><td>复位值</td></tr><tr><td>[31:16]</td><td>Reserved</td><td>R0</td><td>保留。</td><td>0</td></tr><tr><td>[15:12]</td><td rowspan="4">EXT1x[3:0]</td><td rowspan="4">RW</td><td rowspan="4">(x=8-11)，外部中断输入引脚配置位。用以决定外部中断引脚映射到哪个端口的引脚上:0000:PA引脚的第x个引脚;0001:PB引脚的第x个引脚;0010:PC引脚的第x个引脚;0011:PD引脚的第x个引脚;0100:PE引脚的第x个引脚;其他:保留。</td><td rowspan="4">0000b</td></tr><tr><td>[11:8]</td></tr><tr><td>[7:4]</td></tr><tr><td>[3:0]</td></tr></table>

#### 10.3.2.6 外部中断配置寄存器 4（AFIO_EXTICR4）

偏移地址：0x14

<table><tr><td>31</td><td>30</td><td>29</td><td>28</td><td>27</td><td>26</td><td>25</td><td>24</td><td>23</td><td>22</td><td>21</td><td>20</td><td>19</td><td>18</td><td>17</td><td>16</td></tr><tr><td colspan="16">Reserved</td></tr><tr><td colspan="16">15 14 13 12 11 10 9 8 7 6 5 4 3 2 1 0</td></tr><tr><td>EXT115[3:0]</td><td>EXT114[3:0]</td><td>EXT113[3:0]</td><td colspan="13">EXT112[3:0]</td></tr></table>

<table><tr><td>位</td><td>名称</td><td>访问</td><td>描述</td><td>复位值</td></tr><tr><td>[31:16]</td><td>Reserved</td><td>R0</td><td>保留。</td><td>0</td></tr><tr><td>[15:12]</td><td rowspan="4">EXT1x[3:0]</td><td rowspan="4">RW</td><td rowspan="4">(x=12-15),外部中断输入引脚配置位。用以决定外部中断引脚映射到哪个端口的引脚上:0000:PA引脚的第x个引脚;0001:PB引脚的第x个引脚;0010:PC引脚的第x个引脚;0011:PD引脚的第x个引脚;0100:PE引脚的第x个引脚;其他:保留。</td><td rowspan="4">0000b</td></tr><tr><td>[11:8]</td></tr><tr><td>[7:4]</td></tr><tr><td>[3:0]</td></tr></table>

#### 10.3.2.7 重映射寄存器 2（AFIO_PCFR2）

偏移地址：0x1C

<table><tr><td>31</td><td>30</td><td>29</td><td>28</td><td>27</td><td>26</td><td>25</td><td>24</td><td>23</td><td>22</td><td>21</td><td>20</td><td>19</td><td>18</td><td>17</td><td>16</td></tr><tr><td colspan="4">Reserved</td><td>USART1_RM1</td><td colspan="2">UART8_RM[1:0]</td><td colspan="2">UART7_RM[1:0]</td><td colspan="2">UART6_RM[1:0]</td><td colspan="2">UART5_RM[1:0]</td><td colspan="2">UART4_RM[1:0]</td><td></td></tr><tr><td>15</td><td>14</td><td>13</td><td>12</td><td>11</td><td>10</td><td>9</td><td>8</td><td>7</td><td>6</td><td>5</td><td>4</td><td>3</td><td>2</td><td>1</td><td>0</td></tr><tr><td colspan="4">Reserved</td><td>FSMC_NADV</td><td colspan="3">Reserved</td><td colspan="2">TIM10_RM[1:0]</td><td colspan="2">TIM9_RM[1:0]</td><td colspan="2">TIM8_RM</td><td colspan="2">Reserved</td></tr></table>

<table><tr><td>位</td><td>名称</td><td>访问</td><td>描述</td><td>复位值</td></tr><tr><td>[31:27]</td><td>Reserved</td><td>R0</td><td>保留。</td><td>0</td></tr><tr><td>26</td><td>USART1_RM1</td><td>RW</td><td>USART1 映射配置高位（配合 AF10_PCFR1 寄存器 bit2 USART1_RM 使用）。00：默认映射(CK/PA8, TX/PA9, RX/PA10,CTS/PA11, RTS/PA12)；01：重映射(CK/PA8, TX/PB6, RX/PB7,CTS/PA11, RTS/PA12)；10：重映射(CK/PA10, TX/PB15, RX/PA8,CTS/PA5, RTS/PA9)；11：重映射(CK/PA5, TX/PA6, RX/PA7,CTS/PC4,RTS/PC5)。注：CH32V20x_D6、CH32F20x_D6 只存在默认映射(00b)、重映射(01b)。</td><td>0</td></tr><tr><td>[25:24]</td><td>UART8_RM[1:0]</td><td>RW</td><td>UART8 重映射。00：默认映射(TX/PC4, RX/PC5)；01：重映射(TX/PA14, RX/PA15)；1x：重映射(TX/PE14, RX/PE15)。</td><td>00b</td></tr><tr><td>[23:22]</td><td>UART7_RM[1:0]</td><td>RW</td><td>UART7 重映射。00：默认映射(TX/PC2, RX/PC3)；01：重映射(TX/PA6, RX/PA7)；</td><td>00b</td></tr><tr><td></td><td></td><td></td><td>1x:重映射(TX/PE12,RX/PE13)。</td><td></td></tr><tr><td>[21:20]</td><td>UART6_RM[1:0]</td><td>RW</td><td>UART6重映射。00:默认映射(TX/PC0,RX/PC1);01:重映射(TX/PB8,RX/PB9);1x:重映射(TX/PE10,RX/PE11)。</td><td>00b</td></tr><tr><td>[19:18]</td><td>UART5_RM[1:0]</td><td>RW</td><td>UART5重映射。00:默认映射(TX/PC12,RX/PD2);01:重映射(TX/PB4,RX/PB5);1x:重映射(TX/PE8,RX/PE9)。</td><td>00b</td></tr><tr><td>[17:16]</td><td>UART4_RM[1:0]</td><td>RW</td><td>UART4重映射。00:默认映射(TX/PC10,RX/PC11);01:重映射(TX/PB0,RX/PB1);1x:重映射(TX/PE0,RX/PE1)。注:适用于CH32V30x_D8C、CH32V30x_D8、CH32V30x_D8W、CH32V20x_D8、CH32F20x_D8C、CH32F20x_D8、CH32F20x_D8W。x0:默认映射(CK/PB2,TX/PB0,RX/PB1,CTS/PB3,RTS/PB4);x1:重映射(CK/PA6,TX/PA5,RX/PB5,CTS/PA7,RTS/PA15)。注:适用于CH32V20x_D6、CH32F20x_D6。</td><td>00b</td></tr><tr><td>[15:11]</td><td>Reserved</td><td>RW</td><td>保留。</td><td>0</td></tr><tr><td>10</td><td>FSMC_NADV</td><td>RW</td><td>FSMC_NADV重映射。0:FSMC NADV映射到PB7;1:禁止FSMC NADV输出。</td><td>0</td></tr><tr><td>[9:7]</td><td>Reserved</td><td>RO</td><td>保留。</td><td>0</td></tr><tr><td>[6:5]</td><td>TIM10_RM[1:0]</td><td>RW</td><td>TIM10的重映射位。00:默认映射(ETR/PC10,CH1/PB8,CH2/PB9,CH3/PC3,CH4/PC11,BKIN/PC12,CH1N/PA12,CH2N/PA13,CH3N/PA14);01:部分映射(ETR/PB11,CH1/PB3,CH2/PB4,CH3/PB5,CH4/PC15,BKIN/PB10,CH1N/PA5,CH2N/PA6,CH3N/PA7);1x:完全映射(ETR/PD0,CH1/PD1,CH2/PD3,CH3/PD5,CH4/PD7,BKIN/PE2,CH1N/PE3,CH2N/PE4,CH3N/PE5)。</td><td>00b</td></tr><tr><td>[4:3]</td><td>TIM9_RM[1:0]</td><td>RW</td><td>TIM9的重映射位。00:默认映射(ETR/PA2,CH1/PA2,CH2/PA3,CH3/PA4,CH4/PC4,BKIN/PC5,CH1N/PC0,CH2N/PC1,CH3N/PC2);01:部分映射(ETR/PA2,CH1/PA2,CH2/PA3,CH3/PA4,CH4/PC14,BKIN/PA1,CH1N/PB0,CH2N/PB1,CH3N/PB2);1x:完全映射(ETR/PD9,CH1/PD9,CH2/PD11,CH3/PD13,CH4/PD15,BKIN/PD14,CH1N/PD8,CH2N/PD10,CH3N/PD12)。</td><td>00b</td></tr><tr><td>2</td><td>TIM8_RM</td><td>RW</td><td>TIM8的重映射位。
0: 默认映射(ETR/PA0, CH1/PC6, CH2/PC7, CH3/PC8, CH4/PC9, BKIN/PA6, CH1N/PA7, CH2N/PB0, CH3N/PB1);
1: 重映射(ETR/PA0, CH1/PB6, CH2/PB7, CH3/PB8, CH4/PC13, BKIN/PB9, CH1N/PA13, CH2N/PA14, CH3N/PA15);</td><td>0</td></tr><tr><td>[1:0]</td><td>Reserved</td><td>RW</td><td>保留。</td><td>0</td></tr></table>


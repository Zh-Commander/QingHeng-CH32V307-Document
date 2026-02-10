# 第 11 章 直接存储器访问控制（DMA）

本章模块描述适用于CH32F2x、CH32V2x和CH32V3x微控制器全系列产品。

直接存储器访问控制器（DMA）提供在外设和存储器之间或存储器和存储器之间的高速数据传输方式，无须 CPU 干预，数据可以通过 DMA 快速地移动，以节省 CPU 的资源来做其他操作。

DMA 控制器每个通道专门用来管理来自于一个或多个外设对存储器访问的请求。还有一个仲裁器来协调各通道之间的优先级。

## 11.1 主要特性

多个独立可配置通道  
每个通道都直接连接专用的硬件 DMA 请求，并支持软件触发  
$\bullet$ 支持循环的缓冲器管理  
多个通道之间的请求优先权可以通过软件编程设置(最高、高、中和低)，优先权设置相等时由通道号决定（通道号越低优先级越高）  
支持外设到存储器、存储器到外设、存储器到存储器之间的传输  
$\bullet$ 闪存、SRAM、外设的 SRAM、APB1、APB2 和 AHB 外设均可作为访问的源和目标  
可编程的数据传输字节数目：最大为 65535

## 11.2 功能描述

### 11.2.1 DMA 通道处理

#### 1）仲裁优先级

多个独立的通道产生的DMA请求通过逻辑或结构输入到 DMA控制器，当前只会有一个通道的请求得到响应。模块内部的仲裁器根据通道请求的优先级来选择要启动的外设/存储器的访问。

软件管理中，应用程序通过对DMA_CFGRx寄存器的PL[1:0]位设置，可以为每个通道独立配置优先等级，包括最高、高、中、低4个等级。当通道间的软件设置等级一致时，模块会按固定的硬件优先级选择，通道编号偏低的要比偏高的有较高优先权。

#### 2）DMA 配置

当DMA控制器收到一个请求信号时，会访问发出请求的外设或存储器，建立外设或存储器和存储器之间的数据传输。主要包括下面3个操作步骤：

1） 从外设数据寄存器或当前外设/存储器地址寄存器指示的存储器地址取数据，第一次传输时的开始地址是 DMA_PADDRx 或 DMA_MADDRx 寄存器指定的外设基地址或存储器地址。  
2）存数据到外设数据寄存器或当前外设/存储器地址寄存器指示的存储器地址，第一次传输时的开始地址是DMA_PADDRx或DMA_MADDRx寄存器指定的外设基地址或存储器地址。  
3）执行一次DMA_CNTRx寄存器中数值的递减操作，该寄存器指示当前未完成转移的操作数目。

每个通道包括3种DMA数据转移方式：

外设到存储器（MEM2MEM=0，DIR=0）  
$\bullet$ 存储器到外设（MEM2MEM=0， $\mathsf { D } | \mathsf { R } { = } 1$ ）  
$\bullet$ 存储器到存储器（MEM2MEM=1）

注：存储器到存储器方式无需外设请求信号，配置为此模式后（MEM2MEM=1），通道开启（EN=1）即可启动数据传输。此方式不支持循环模式。

配置过程如下：

1）在 DMA_PADDRx 寄存器中设置外设寄存器的首地址或存储器到存储器方式（MEM2MEM=1）下存储器数据地址。发生DMA请求时，这个地址将是数据传输的源或目标地址。  
2） 在 DMA_MADDRx 寄存器中设置存储器数据地址。发生 DMA 请求时，传输的数据将从这个地址读出或写入这个地址。  
3） 在 DMA_CNTRx 寄存器中设置要传输的数据数量。在每个数据传输后，这个数值递减。  
4） 在 DMA_CFGRx 寄存器的 PL[1:0]位中设置通道的优先级。  
5）在 DMA_CFGRx寄存器中设置数据传输的方向、循环模式、外设和存储器的增量模式、外设和存储器的数据宽度、传输过半、传输完成、传输错误中断使能位，  
6） 设置 DMA_CCRx 寄存器的 ENABLE 位，启动通道 x。

注：DMA_PADDRx/DMA_MADDRx/DMA_CNTRx寄存器以及DMA_CFGRx寄存器中的数据传输的方向（DIR）、循环模式（位置）、外设和存储器的增量模式（MINC/PINC）等控制位只有在DMA 通道被关闭下才可以配置写入。

#### 3）循环模式

设置DMA_CFGRx寄存器的CIRC位置1，可以启用通道数据传输的循环模式功能。循环模式下，当数据传输的数目变为0时，DMA_CNTRx寄存器的内容会自动被重新加载为其初始数值，内部的外设和存储器地址寄存器也被重新加载为 DMA_PADDRx 和 DMA_MADDRx 寄存器设定的初始地址值，DMA 操作将继续进行，直到通道被关闭或关闭DMA模式。

#### 4）DMA 处理状态

l 传输过半：对应DMA_INTFR寄存器中的 HTIFx位硬件置位。当DMA的传输字节数目减至初始设定值一半以下将会产生DMA传输过半标志，如果在 DMA_CCRx寄存器中置位了 HTIE，则将产生中断。硬件通过此标志提醒应用程序，可以为新一轮数据传输做准备。  
传输完成：对应DMA_INTFR寄存器中的 ${ \mathsf { T C l } } \mathsf { F x }$ 位硬件置位。当DMA的传输字节数目减至 0 将会产生 DMA 传输完成标志，如果在 DMA_CCRx 寄存器中置位了 TCIE，则将产生中断。  
l 传输错误：对应DMA_INTFR寄存器中的 TEIFx位硬件置位。读写一个保留的地址区域，将会产生DMA传输错误。同时模块硬件会自动清0发生错误的通道所对应的 DMA_CCRx寄存器的 EN位，该通道被关闭。如果在DMA_CCRx寄存器中置位了 TEIE，则将产生中断。

应用程序在查询 DMA 通道状态时，可以先访问 DMA_INTFR 寄存器的 GIFx 位，判断出当前哪个通道发生了 DMA 事件，进而处理该通道的具体 DAM 事件内容。

### 11.2.2 可编程的数据传输总大小/数据位宽/对齐方式

DMA 每个通道一轮传输的数据量总大小可编程，最大 65535 次。DMA_CNTRx 寄存器中指示待传输字节数目。在 EN=0 时，写入设置值，在 EN=1 开启 DMA 传输通道后，此寄存器变为只读属性，在每次传输后数值递减。

外设和存储器的传输数据取值支持地址指针自动递增功能，指针增量可编程。它们访问的第一个传输的数据地址存放在 DMA_PADDRx 和 DMA_MADDRx 寄存器中，通过设置 DMA_CFGRx 寄存器的 PINC 位或 MINC 位置 1，可以分别开启外设地址自增模式或存储器地址自增模式，PSIZE[1:0]设置外设地址取数据大小及地址自增大小，MSIZE[1:0]设置存储器地址取数据大小及地址自增大小，包括 3 种选择：8 位、16 位、32 位。具体数据转移方式如下表：

表 11-1 不同数据位宽下 DMA 转移（PINC=MINC=1）  

<table><tr><td>源端位宽</td><td>目标位宽</td><td>传输数目</td><td>源:地址/数据</td><td>目标:地址/数据</td><td>传输操作</td></tr><tr><td rowspan="3">8</td><td rowspan="3">8</td><td rowspan="3">4</td><td>0x00/B0</td><td>0x00/B0</td><td rowspan="3">●源端地址递增量与源端设置的数据位宽对齐,取值大小等于源端数据位宽</td></tr><tr><td>0x01/B1</td><td>0x01/B1</td></tr><tr><td>0x02/B2</td><td>0x02/B2</td></tr><tr><td></td><td></td><td></td><td>0x03/B3</td><td>0x03/B3</td><td>• 目标地址递增量与目标设置数据的位宽对齐,取值大小等于目标数据位宽DMA转移送入目标端的数据依据原则:数据大小不足高位补0,数据大小溢出高位去掉• 存储数据方式:小端模式,低地址存放低字节,高地址存放高字节</td></tr><tr><td>8</td><td>16</td><td>4</td><td>0x00/B00x01/B10x02/B20x03/B3</td><td>0x00/00B00x02/00B10x04/00B20x06/00B3</td><td>• 目标地址递增量与目标设置数据的位宽对齐,取值大小等于目标数据位宽DMA转移送入目标端的数据依据原则:数据大小不足高位补0,数据大小溢出高位去掉• 存储数据方式:小端模式,低地址存放低字节,高地址存放高字节</td></tr><tr><td>8</td><td>32</td><td>4</td><td>0x00/B00x01/B10x02/B20x03/B3</td><td>0x00/000000B00x04/000000B10x08/000000B20x0C/000000B3</td><td>• 目标地址递增量与目标设置数据的位宽对齐,取值大小等于目标数据位宽DMA转移送入目标端的数据依据原则:数据大小不足高位补0,数据大小溢出高位去掉• 存储数据方式:小端模式,低地址存放低字节,高地址存放高字节</td></tr><tr><td>16</td><td>8</td><td>4</td><td>0x00/B1B00x02/B3B20x04/B5B40x06/B7B6</td><td>0x00/B00x01/B20x02/B40x03/B6</td><td>• 目标地址递增量与目标设置数据的位宽对齐,取值大小等于目标数据位宽DMA转移送入目标端的数据依据原则:数据大小不足高位补0,数据大小溢出高位去掉</td></tr><tr><td>16</td><td>16</td><td>4</td><td>0x00/B1B00x02/B3B20x04/B5B40x06/B7B6</td><td>0x00/B1B00x02/B3B20x04/B5B40x06/B7B6</td><td>• 目标地址递增量与目标设置数据的位宽对齐,取值大小等于目标数据位宽DMA转移送入目标端的数据依据原则:数据大小不足高位补0,数据大小溢出高位去掉</td></tr><tr><td>16</td><td>32</td><td>4</td><td>0x00/B1B00x02/B3B20x04/B5B40x06/B7B6</td><td>0x00/0000B1B00x04/0000B3B20x08/0000B5B40x0C/0000B7B6</td><td>• 目标地址递增量与目标设置数据的位宽对齐,取值大小等于目标数据位宽DMA转移送入目标端的数据依据原则:数据大小不足高位补0,数据大小溢出高位去掉</td></tr><tr><td>32</td><td>8</td><td>4</td><td>0x00/B3B2B1B00x04/B7B6B5B40x08/BBBAB9B80x0C/BFBEBDBC</td><td>0x00/B00x01/B40x02/B80x03/BC</td><td>• 目标地址递增量与目标设置数据的位宽对齐,取值大小等于目标数据位宽DMA转移送入目标端的数据依据原则:数据大小不足高位补0,数据大小溢出高位去掉</td></tr><tr><td>32</td><td>16</td><td>4</td><td>0x00/B3B2B1B00x04/B7B6B5B40x08/BBBAB9B80x0C/BFBEBDBC</td><td>0x00/B1B00x02/B5B40x04/B9B80x06/BDBC</td><td>• 目标地址递增量与目标设置数据的位宽对齐,取值大小等于目标数据位宽DMA转移送入目标端的数据依据原则:数据大小不足高位补0,数据大小溢出高位去掉</td></tr><tr><td>32</td><td>32</td><td>4</td><td>0x00/B3B2B1B00x04/B7B6B5B40x08/BBBAB9B80x0C/BFBEBDBC</td><td>0x00/B3B2B1B00x04/B7B6B5B40x08/BBBAB9B80x0C/BFBEBDBC</td><td>• 目标地址递增量与目标设置数据的位宽对齐,取值大小等于目标数据位宽DMA转移送入目标端的数据依据原则:数据大小不足高位补0,数据大小溢出高位去掉</td></tr></table>

### 11.2.3 DMA 请求映射

青稞 V4F MCU（包括 CH32V30x_D8C、CH32V30x_D8）以及 ARM○R CortexTM-M3 MCU(包括 CH32F20x_D8C、CH32F20x_D8），DMA控制器提供18个通道，其中 DMA1 包含 7个通道，DMA2包含 11个通道，每个通道对应多个外设请求，通过设置相应外设寄存器中对应 DMA 控制位，可以独立的开启或关闭各个外设的 DMA 功能，具体对应关系如下。

![](images/0159f50eec506a61590c13962a071543f9d44370d3d980e7cd6b8f6f0562b7f8.jpg)  
图 11-1 DMA1 请求映像

![](images/258d2d7306c403ed2f3c9710eeeaaada0fcfce1eecc2b9979e40a907526c49ee.jpg)  
图 11-2 DMA2 请求映像

表 11-2 DMA1 各通道外设映射表  

<table><tr><td>外设</td><td>通道1</td><td>通道2</td><td>通道3</td><td>通道4</td><td>通道5</td><td>通道6</td><td>通道7</td></tr><tr><td>ADC1</td><td>ADC1</td><td></td><td></td><td></td><td></td><td></td><td></td></tr><tr><td>SPI1</td><td></td><td>SPI1_RX</td><td>SPI1_TX</td><td></td><td></td><td></td><td></td></tr><tr><td>SPI/12S2</td><td></td><td></td><td></td><td>SPI/12S2_RX</td><td>SPI/12S2_TX</td><td></td><td></td></tr><tr><td>USART1</td><td></td><td></td><td></td><td>USART1_TX</td><td>USART1_RX</td><td></td><td></td></tr><tr><td>USART2</td><td></td><td></td><td></td><td></td><td></td><td>USART2_RX</td><td>USART2_TX</td></tr><tr><td>USART3</td><td></td><td>USART3_TX</td><td>USART3_RX</td><td></td><td></td><td></td><td></td></tr><tr><td>I2C1</td><td></td><td></td><td></td><td></td><td></td><td>I2C1_TX</td><td>I2C1_RX</td></tr><tr><td>I2C2</td><td></td><td></td><td></td><td>I2C2_TX</td><td>I2C2_RX</td><td></td><td></td></tr><tr><td>TIM1</td><td></td><td>TIM1_CH1</td><td>TIM1_CH2</td><td>TIM1_CH4
TIM1_TRIG
TIM1_COM</td><td>TIM1_UP</td><td>TIM1_CH3</td><td></td></tr><tr><td>TIM2</td><td>TIM2_CH3</td><td>TIM2_UP</td><td></td><td></td><td>TIM2_CH1</td><td></td><td>TIM2_CH2
TIM2_CH4</td></tr><tr><td>TIM3</td><td></td><td>TIM3_CH3</td><td>TIM3_CH4
TIM3_UP</td><td></td><td></td><td>TIM3_CH1
TIM3_TRIG</td><td></td></tr><tr><td>TIM4</td><td>TIM4_CH1</td><td></td><td></td><td>TIM4_CH2</td><td>TIM4_CH3</td><td></td><td>TIM4_UP</td></tr></table>

表11-3 DMA2各通道外设映射表一  

<table><tr><td>外设</td><td>通道1</td><td>通道2</td><td>通道3</td><td>通道4</td><td>通道5</td><td>通道6</td><td>通道7</td></tr><tr><td rowspan="2">TIM5</td><td>TIM5_CH4</td><td>TIM5_CH3</td><td></td><td>TIM5_CH2</td><td>TIM5_CH1</td><td></td><td></td></tr><tr><td>TIM5_TRIG</td><td>TIM5_UP</td><td></td><td></td><td></td><td></td><td></td></tr><tr><td>TIM6</td><td></td><td></td><td>TIM6_UP</td><td></td><td></td><td></td><td></td></tr><tr><td>TIM7</td><td></td><td></td><td></td><td>TIM7_UP</td><td></td><td></td><td></td></tr><tr><td rowspan="3">TIM8</td><td>TIM8_CH3</td><td>TIM8_CH4</td><td>TIM8_CH1</td><td></td><td>TIM8_CH2</td><td></td><td></td></tr><tr><td>TIM8_UP</td><td>TIM8_TRIG</td><td></td><td></td><td></td><td></td><td></td></tr><tr><td>TIM8_COM</td><td>TIM8_COM</td><td></td><td></td><td></td><td></td><td></td></tr><tr><td>TIM9</td><td></td><td></td><td></td><td></td><td></td><td>TIM9_UP</td><td>TIM9_CH1</td></tr><tr><td rowspan="2">TIM10</td><td></td><td></td><td></td><td></td><td></td><td>TIM10_CH4</td><td>TIM10_TRIG</td></tr><tr><td></td><td></td><td></td><td></td><td></td><td></td><td>TIM10_COM</td></tr><tr><td>UART4</td><td></td><td></td><td>USART4_RX</td><td></td><td>UART4_TX</td><td></td><td></td></tr><tr><td>UART5</td><td></td><td>USART5_RX</td><td></td><td>UART5_TX</td><td></td><td></td><td></td></tr><tr><td>UART6</td><td></td><td></td><td></td><td></td><td></td><td>UART6_TX</td><td>UART6_RX</td></tr><tr><td>UART7</td><td></td><td></td><td></td><td></td><td></td><td></td><td></td></tr><tr><td>UART8</td><td></td><td></td><td></td><td></td><td></td><td></td><td></td></tr><tr><td>SPI/12S3</td><td>SPI/12S3_RX</td><td>SPI/12S3_TX</td><td></td><td></td><td></td><td></td><td></td></tr><tr><td>SDIO</td><td></td><td></td><td></td><td>SDIO</td><td></td><td></td><td></td></tr><tr><td>DAC1</td><td></td><td></td><td>DAC1</td><td></td><td></td><td></td><td></td></tr><tr><td>DAC2</td><td></td><td></td><td></td><td>DAC2</td><td></td><td></td><td></td></tr></table>

表11-4 DMA2各通道外设映射表二  

<table><tr><td>外设</td><td>通道8</td><td>通道9</td><td>通道10</td><td>通道11</td></tr><tr><td>TIM5</td><td></td><td></td><td></td><td></td></tr><tr><td>TIM6</td><td></td><td></td><td></td><td></td></tr><tr><td>TIM7</td><td></td><td></td><td></td><td></td></tr><tr><td>TIM8</td><td></td><td></td><td></td><td></td></tr><tr><td rowspan="2">TIM9</td><td rowspan="2">TIM9_CH4</td><td rowspan="2">TIM9_CH2</td><td>TIM9_TRIG</td><td rowspan="2">TIM9_CH3</td></tr><tr><td>TIM9_COM</td></tr><tr><td>TIM10</td><td>TIM10_CH1</td><td>TIM10_CH3</td><td>TIM10_CH2</td><td>TIM10_UP</td></tr><tr><td>UART4</td><td></td><td></td><td></td><td></td></tr><tr><td>UART5</td><td></td><td></td><td></td><td></td></tr><tr><td>UART6</td><td></td><td></td><td></td><td></td></tr><tr><td>UART7</td><td>UART7_TX</td><td>UART7_RX</td><td></td><td></td></tr><tr><td>UART8</td><td></td><td></td><td>UART8_TX</td><td>UART8_RX</td></tr><tr><td>SPI/12S3</td><td></td><td></td><td></td><td></td></tr><tr><td>SD10</td><td></td><td></td><td></td><td></td></tr><tr><td>DAC1</td><td></td><td></td><td></td><td></td></tr><tr><td>DAC2</td><td></td><td></td><td></td><td></td></tr></table>

青稞 V4B MCU（包括 $\mathsf { C H } 3 2 \mathsf { V } 2 0 \mathsf { x } \_ \mathsf { D } 6 .$ ）以及 ARM○R CortexTM-M3 MCU(包括 CH32F20x_D6），DMA 控制器提供 8 个通道，每个通道对应多个外设请求，通过设置相应外设寄存器中对应 DMA 控制位，可以独立的开启或关闭各个外设的DMA功能，具体对应关系如下。

<table><tr><td>外设</td><td>通道1</td><td>通道2</td><td>通道3</td><td>通道4</td><td>通道5</td><td>通道6</td><td>通道7</td><td>通道8</td></tr><tr><td>ADC1</td><td>ADC1</td><td></td><td></td><td></td><td></td><td></td><td></td><td></td></tr><tr><td>SPI1</td><td></td><td>SPI1_RX</td><td>SPI1_TX</td><td></td><td></td><td></td><td></td><td></td></tr><tr><td>SPI2</td><td></td><td></td><td></td><td>SPI2_RX</td><td>SPI2_TX</td><td></td><td></td><td></td></tr><tr><td>USART1</td><td></td><td></td><td></td><td>USART1_TX</td><td>USART1_RX</td><td></td><td></td><td></td></tr><tr><td>USART2</td><td></td><td></td><td></td><td></td><td></td><td>USART2_RX</td><td>USART2_TX</td><td></td></tr><tr><td>USART3</td><td></td><td>USART3_TX</td><td>USART3_RX</td><td></td><td></td><td></td><td></td><td></td></tr><tr><td>USART4</td><td>USART4_TX</td><td></td><td></td><td></td><td></td><td></td><td></td><td>USART4_RX</td></tr><tr><td>I2C1</td><td></td><td></td><td></td><td></td><td></td><td>I2C1_TX</td><td>I2C1_RX</td><td></td></tr><tr><td>I2C2</td><td></td><td></td><td></td><td>I2C2_TX</td><td>I2C2_RX</td><td></td><td></td><td></td></tr><tr><td>TIM1</td><td></td><td>TIM1_CH1</td><td>TIM1_CH2</td><td>TIM1_CH4TIM1_TRIGTIM1_COM</td><td>TIM1_UP</td><td>TIM1_CH3</td><td></td><td></td></tr><tr><td>TIM2</td><td>TIM2_CH3</td><td>TIM2_UP</td><td></td><td></td><td>TIM2_CH1</td><td></td><td>TIM2_CH2TIM2_CH4</td><td></td></tr><tr><td>TIM3</td><td></td><td>TIM3_CH3</td><td>TIM3_CH4TIM3_UP</td><td></td><td></td><td>TIM3_CH1TIM3_TRIG</td><td></td><td></td></tr><tr><td>TIM4</td><td>TIM4_CH1</td><td></td><td></td><td>TIM4_CH2</td><td>TIM4_CH3</td><td></td><td>TIM4_UP</td><td></td></tr></table>

青稞 V4C MCU（包括 CH32V20x_D8W、CH32V20x_D8）以及 ARM○R CortexTM-M3 MCU(包括 CH32F20x_D8W），DMA控制器提供8个通道，每个通道对应多个外设请求，通过设置相应外设寄存器中对应DMA控制位，可以独立的开启或关闭各个外设的 DMA 功能，具体对应关系如下。

<table><tr><td>外设</td><td>通道1</td><td>通道2</td><td>通道3</td><td>通道4</td><td>通道5</td><td>通道6</td><td>通道7</td><td>通道8</td></tr><tr><td>ADC1</td><td>ADC1</td><td></td><td></td><td></td><td></td><td></td><td></td><td></td></tr><tr><td>SPI1</td><td></td><td>SPI1_RX</td><td>SPI1_TX</td><td></td><td></td><td></td><td></td><td></td></tr><tr><td>SPI2</td><td></td><td></td><td></td><td>SPI2_RX</td><td>SPI2_TX</td><td></td><td></td><td></td></tr><tr><td>USART1</td><td></td><td></td><td></td><td>USART1_TX</td><td>USART1_RX</td><td></td><td></td><td></td></tr><tr><td>USART2</td><td></td><td></td><td></td><td></td><td></td><td>USART2_RX</td><td>USART2_TX</td><td></td></tr><tr><td>USART3</td><td></td><td>USART3_TX</td><td>USART3_RX</td><td></td><td></td><td></td><td></td><td></td></tr><tr><td>USART4</td><td>USART4_TX</td><td></td><td></td><td></td><td></td><td></td><td></td><td>USART4_RX</td></tr><tr><td>I2C1</td><td></td><td></td><td></td><td></td><td></td><td>I2C1_TX</td><td>I2C1_RX</td><td></td></tr><tr><td>I2C2</td><td></td><td></td><td></td><td>I2C2_TX</td><td>I2C2_RX</td><td></td><td></td><td></td></tr><tr><td rowspan="2">TIM1</td><td rowspan="2"></td><td rowspan="2">TIM1_CH1</td><td rowspan="2">TIM1_CH2</td><td>TIM1_CH4</td><td rowspan="2">TIM1_TRIG</td><td rowspan="2">TIM1_UP</td><td rowspan="2">TIM1_CH3</td><td rowspan="2"></td></tr><tr><td>TIM1_COM</td></tr><tr><td rowspan="2">TIM2</td><td rowspan="2">TIM2_CH3</td><td rowspan="2">TIM2_UP</td><td rowspan="2"></td><td rowspan="2"></td><td rowspan="2">TIM2_CH1</td><td rowspan="2"></td><td>TIM2_CH2</td><td rowspan="2"></td></tr><tr><td>TIM2_CH4</td></tr><tr><td>TIM3</td><td></td><td>TIM3_CH3</td><td>TIM3_CH4TIM3_UP</td><td></td><td></td><td>TIM3_CH1TIM3_TRIG</td><td></td><td></td></tr><tr><td>TIM4</td><td>TIM4_CH1</td><td></td><td></td><td>TIM4_CH2</td><td>TIM4_CH3</td><td></td><td>TIM4_UP</td><td></td></tr><tr><td>TIM5</td><td>TIM5_CH2</td><td></td><td>TIM5_CH3</td><td></td><td></td><td>TIM5_CH4</td><td>TIM5_CH1TIM5_TRIG</td><td>TIM5_UP</td></tr></table>

## 11.3 寄存器描述

表 11-5 DMA1 相关寄存器列表  

<table><tr><td>名称</td><td>访问地址</td><td>描述</td><td>复位值</td></tr><tr><td>R32_DMA1_INTFR</td><td>0x40020000</td><td>DMA1中断状态寄存器</td><td>0x00000000</td></tr><tr><td>R32_DMA1_INTFCR</td><td>0x40020004</td><td>DMA1中断标志清除寄存器</td><td>0x00000000</td></tr><tr><td>R32_DMA1_CFGR1</td><td>0x40020008</td><td>DMA1通道1配置寄存器</td><td>0x00000000</td></tr><tr><td>R32_DMA1_CNTR1</td><td>0x4002000C</td><td>DMA1通道1传输数据数目寄存器</td><td>0x00000000</td></tr><tr><td>R32_DMA1_PADDR1</td><td>0x40020010</td><td>DMA1通道1外设地址寄存器</td><td>0x00000000</td></tr><tr><td>R32_DMA1_MADDR1</td><td>0x40020014</td><td>DMA1通道1存储器地址寄存器</td><td>0x00000000</td></tr><tr><td>R32_DMA1_CFGR2</td><td>0x4002001C</td><td>DMA1通道2配置寄存器</td><td>0x00000000</td></tr><tr><td>R32_DMA1_CNTR2</td><td>0x40020020</td><td>DMA1通道2传输数据数目寄存器</td><td>0x00000000</td></tr><tr><td>R32_DMA1_PADDR2</td><td>0x40020024</td><td>DMA1通道2外设地址寄存器</td><td>0x00000000</td></tr><tr><td>R32_DMA1_MADDR2</td><td>0x40020028</td><td>DMA1通道2存储器地址寄存器</td><td>0x00000000</td></tr><tr><td>R32_DMA1_CFGR3</td><td>0x40020030</td><td>DMA1通道3配置寄存器</td><td>0x00000000</td></tr><tr><td>R32_DMA1_CNTR3</td><td>0x40020034</td><td>DMA1通道3传输数据数目寄存器</td><td>0x00000000</td></tr><tr><td>R32_DMA1_PADDR3</td><td>0x40020038</td><td>DMA1通道3外设地址寄存器</td><td>0x00000000</td></tr><tr><td>R32_DMA1_MADDR3</td><td>0x4002003C</td><td>DMA1通道3存储器地址寄存器</td><td>0x00000000</td></tr><tr><td>R32_DMA1_CFGR4</td><td>0x40020044</td><td>DMA1通道4配置寄存器</td><td>0x00000000</td></tr><tr><td>R32_DMA1_CNTR4</td><td>0x40020048</td><td>DMA1通道4传输数据数目寄存器</td><td>0x00000000</td></tr><tr><td>R32_DMA1_PADDR4</td><td>0x4002004C</td><td>DMA1通道4外设地址寄存器</td><td>0x00000000</td></tr><tr><td>R32_DMA1_MADDR4</td><td>0x40020050</td><td>DMA1通道4存储器地址寄存器</td><td>0x00000000</td></tr><tr><td>R32_DMA1_CFGR5</td><td>0x40020058</td><td>DMA1通道5配置寄存器</td><td>0x00000000</td></tr><tr><td>R32_DMA1_CNTR5</td><td>0x4002005C</td><td>DMA1通道5传输数据数目寄存器</td><td>0x00000000</td></tr><tr><td>R32_DMA1_PADDR5</td><td>0x40020060</td><td>DMA1通道5外设地址寄存器</td><td>0x00000000</td></tr><tr><td>R32_DMA1_MADDR5</td><td>0x40020064</td><td>DMA1通道5存储器地址寄存器</td><td>0x00000000</td></tr><tr><td>R32_DMA1_CFGR6</td><td>0x4002006C</td><td>DMA1通道6配置寄存器</td><td>0x00000000</td></tr><tr><td>R32_DMA1_CNTR6</td><td>0x40020070</td><td>DMA1通道6传输数据数目寄存器</td><td>0x00000000</td></tr><tr><td>R32_DMA1_PADDR6</td><td>0x40020074</td><td>DMA1通道6外设地址寄存器</td><td>0x00000000</td></tr><tr><td>R32_DMA1_MADDR6</td><td>0x40020078</td><td>DMA1通道6存储器地址寄存器</td><td>0x00000000</td></tr><tr><td>R32_DMA1_CFGR7</td><td>0x40020080</td><td>DMA1通道7配置寄存器</td><td>0x00000000</td></tr><tr><td>R32_DMA1_CNTR7</td><td>0x40020084</td><td>DMA1通道7传输数据数目寄存器</td><td>0x00000000</td></tr><tr><td>R32_DMA1_PADDR7</td><td>0x40020088</td><td>DMA1通道7外设地址寄存器</td><td>0x00000000</td></tr><tr><td>R32_DMA1_MADDR7</td><td>0x4002008C</td><td>DMA1通道7存储器地址寄存器</td><td>0x00000000</td></tr><tr><td>R32_DMA1_CFGR8</td><td>0x40020094</td><td>DMA1通道8配置寄存器</td><td>0x00000000</td></tr><tr><td>R32_DMA1_CNTR8</td><td>0x40020098</td><td>DMA1通道8传输数据数目寄存器</td><td>0x00000000</td></tr><tr><td>R32_DMA1_PADDR8</td><td>0x4002009C</td><td>DMA1通道8外设地址寄存器</td><td>0x00000000</td></tr><tr><td>R32_DMA1_MADDR8</td><td>0x400200AO</td><td>DMA1通道8存储器地址寄存器</td><td>0x00000000</td></tr></table>

表 11-6 DMA2 相关寄存器列表  

<table><tr><td>名称</td><td>访问地址</td><td>描述</td><td>复位值</td></tr><tr><td>R32_DMA2_INTFR</td><td>0x40020400</td><td>DMA2中断状态寄存器</td><td>0x00000000</td></tr><tr><td>R32_DMA2_INTFCR</td><td>0x40020404</td><td>DMA2中断标志清除寄存器</td><td>0x00000000</td></tr><tr><td>R32_DMA2_CFGR1</td><td>0x40020408</td><td>DMA2通道1配置寄存器</td><td>0x00000000</td></tr><tr><td>R32_DMA2_CNTR1</td><td>0x4002040C</td><td>DMA2通道1传输数据数目寄存器</td><td>0x00000000</td></tr><tr><td>R32_DMA2_PADDR1</td><td>0x40020410</td><td>DMA2通道1外设地址寄存器</td><td>0x00000000</td></tr><tr><td>R32_DMA2_MADDR1</td><td>0x40020414</td><td>DMA2通道1存储器地址寄存器</td><td>0x00000000</td></tr><tr><td>R32_DMA2_CFGR2</td><td>0x4002041C</td><td>DMA2通道2配置寄存器</td><td>0x00000000</td></tr><tr><td>R32_DMA2_CNTR2</td><td>0x40020420</td><td>DMA2通道2传输数据数目寄存器</td><td>0x00000000</td></tr><tr><td>R32_DMA2_PADDR2</td><td>0x40020424</td><td>DMA2通道2外设地址寄存器</td><td>0x00000000</td></tr><tr><td>R32_DMA2_MADDR2</td><td>0x40020428</td><td>DMA2通道2存储器地址寄存器</td><td>0x00000000</td></tr><tr><td>R32_DMA2_CFGR3</td><td>0x40020430</td><td>DMA2通道3配置寄存器</td><td>0x00000000</td></tr><tr><td>R32_DMA2_CNTR3</td><td>0x40020434</td><td>DMA2通道3传输数据数目寄存器</td><td>0x00000000</td></tr><tr><td>R32_DMA2_PADDR3</td><td>0x40020438</td><td>DMA2通道3外设地址寄存器</td><td>0x00000000</td></tr><tr><td>R32_DMA2_MADDR3</td><td>0x4002043C</td><td>DMA2通道3存储器地址寄存器</td><td>0x00000000</td></tr><tr><td>R32_DMA2_CFGR4</td><td>0x40020444</td><td>DMA2通道4配置寄存器</td><td>0x00000000</td></tr><tr><td>R32_DMA2_CNTR4</td><td>0x40020448</td><td>DMA2通道4传输数据数目寄存器</td><td>0x00000000</td></tr><tr><td>R32_DMA2_PADDR4</td><td>0x4002044C</td><td>DMA2通道4外设地址寄存器</td><td>0x00000000</td></tr><tr><td>R32_DMA2_MADDR4</td><td>0x40020450</td><td>DMA2通道4存储器地址寄存器</td><td>0x00000000</td></tr><tr><td>R32_DMA2_CFGR5</td><td>0x40020458</td><td>DMA2通道5配置寄存器</td><td>0x00000000</td></tr><tr><td>R32_DMA2_CNTR5</td><td>0x4002045C</td><td>DMA2通道5传输数据数目寄存器</td><td>0x00000000</td></tr><tr><td>R32_DMA2_PADDR5</td><td>0x40020460</td><td>DMA2通道5外设地址寄存器</td><td>0x00000000</td></tr><tr><td>R32_DMA2_MADDR5</td><td>0x40020464</td><td>DMA2通道5存储器地址寄存器</td><td>0x00000000</td></tr><tr><td>R32_DMA2_CFGR6</td><td>0x4002046C</td><td>DMA2通道6配置寄存器</td><td>0x00000000</td></tr><tr><td>R32_DMA2_CNTR6</td><td>0x40020470</td><td>DMA2通道6传输数据数目寄存器</td><td>0x00000000</td></tr><tr><td>R32_DMA2_PADDR6</td><td>0x40020474</td><td>DMA2通道6外设地址寄存器</td><td>0x00000000</td></tr><tr><td>R32_DMA2_MADDR6</td><td>0x40020478</td><td>DMA2通道6存储器地址寄存器</td><td>0x00000000</td></tr><tr><td>R32_DMA2_CFGR7</td><td>0x40020480</td><td>DMA2通道7配置寄存器</td><td>0x00000000</td></tr><tr><td>R32_DMA2_CNTR7</td><td>0x40020484</td><td>DMA2通道7传输数据数目寄存器</td><td>0x00000000</td></tr><tr><td>R32_DMA2_PADDR7</td><td>0x40020488</td><td>DMA2通道7外设地址寄存器</td><td>0x00000000</td></tr><tr><td>R32_DMA2_MADDR7</td><td>0x4002048C</td><td>DMA2通道7存储器地址寄存器</td><td>0x00000000</td></tr><tr><td>R32_DMA2_CFGR8</td><td>0x40020490</td><td>DMA2通道8配置寄存器</td><td>0x00000000</td></tr><tr><td>R32_DMA2_CNTR8</td><td>0x40020494</td><td>DMA2通道8传输数据数目寄存器</td><td>0x00000000</td></tr><tr><td>R32_DMA2_PADDR8</td><td>0x40020498</td><td>DMA2通道8外设地址寄存器</td><td>0x00000000</td></tr><tr><td>R32_DMA2_MADDR8</td><td>0x4002049C</td><td>DMA2通道8存储器地址寄存器</td><td>0x00000000</td></tr><tr><td>R32_DMA2_CFGR9</td><td>0x400204A0</td><td>DMA2通道9配置寄存器</td><td>0x00000000</td></tr><tr><td>R32_DMA2_CNTR9</td><td>0x400204A4</td><td>DMA2通道9传输数据数目寄存器</td><td>0x00000000</td></tr><tr><td>R32_DMA2_PADDR9</td><td>0x400204A8</td><td>DMA2通道9外设地址寄存器</td><td>0x00000000</td></tr><tr><td>R32_DMA2_MADDR9</td><td>0x400204AC</td><td>DMA2通道9存储器地址寄存器</td><td>0x00000000</td></tr><tr><td>R32_DMA2_CFGR10</td><td>0x400204B0</td><td>DMA2通道10配置寄存器</td><td>0x00000000</td></tr><tr><td>R32_DMA2_CNTR10</td><td>0x400204B4</td><td>DMA2通道10传输数据数目寄存器</td><td>0x00000000</td></tr><tr><td>R32_DMA2_PADDR10</td><td>0x400204B8</td><td>DMA2通道10外设地址寄存器</td><td>0x00000000</td></tr><tr><td>R32_DMA2_MADDR10</td><td>0x400204BC</td><td>DMA2通道10存储器地址寄存器</td><td>0x00000000</td></tr><tr><td>R32_DMA2_CFGR11</td><td>0x400204C0</td><td>DMA2通道11配置寄存器</td><td>0x00000000</td></tr><tr><td>R32_DMA2_CNTR11</td><td>0x400204C4</td><td>DMA2 通道 11 传输数据数目寄存器</td><td>0x00000000</td></tr><tr><td>R32_DMA2_PADDR11</td><td>0x400204C8</td><td>DMA2 通道 11 外设地址寄存器</td><td>0x00000000</td></tr><tr><td>R32_DMA2_MADDR11</td><td>0x400204CC</td><td>DMA2 通道 11 存储器地址寄存器</td><td>0x00000000</td></tr><tr><td>R32_DMA2_EXTEM_INTFR</td><td>0x400204D0</td><td>DMA2 扩展中断状态寄存器</td><td>0x00000000</td></tr><tr><td>R32_DMA2_EXTEM_INTFCR</td><td>0x400204D4</td><td>DMA2 扩展中断标志清除寄存器</td><td>0x00000000</td></tr></table>

### 11.3.1 DMAx 中断状态寄存器（DMAx_INTFR）（ $( x = 1 / 2 )$

偏移地址： $0 \times 0 0 + ( \times - 1 ) \times 0 \times 4 0 0$

<table><tr><td>31</td><td>30</td><td>29</td><td>28</td><td>27</td><td>26</td><td>25</td><td>24</td><td>23</td><td>22</td><td>21</td><td>20</td><td>19</td><td>18</td><td>17</td><td>16</td></tr><tr><td>TEIF8</td><td>HTIF8</td><td>TCIF8</td><td>GIF8</td><td>TEIF7</td><td>HTIF7</td><td>TCIF7</td><td>GIF7</td><td>TEIF6</td><td>HTIF6</td><td>TCIF6</td><td>GIF6</td><td>TEIF5</td><td>HTIF5</td><td>TCIF5</td><td>GIF5</td></tr><tr><td>15</td><td>14</td><td>13</td><td>12</td><td>11</td><td>10</td><td>9</td><td>8</td><td>7</td><td>6</td><td>5</td><td>4</td><td>3</td><td>2</td><td>1</td><td>0</td></tr><tr><td>TEIF4</td><td>HTIF4</td><td>TCIF4</td><td>GIF4</td><td>TEIF3</td><td>HTIF3</td><td>TCIF3</td><td>GIF3</td><td>TEIF2</td><td>HTIF2</td><td>TCIF2</td><td>GIF2</td><td>TEIF1</td><td>HTIF1</td><td>TCIF1</td><td>GIF1</td></tr></table>

<table><tr><td>位</td><td>名称</td><td>访问</td><td>描述</td><td>复位值</td></tr><tr><td>31/27/23/19/15/11/7/3</td><td>TEIFx</td><td>RO</td><td>通道x的传输错误标志(x=1/2/3/4/5/6/7/8):1:在通道x上发生了传输错误;0:在通道x上没有传输错误。硬件置位,软件写CTEIFx位清除此标志。</td><td>0</td></tr><tr><td>30/26/22/18/14/10/6/2</td><td>HTIFx</td><td>RO</td><td>通道x的传输过半标志(x=1/2/3/4/5/6/7/8):1:在通道x上产生了传输过半事件;0:在通道x上没有传输过半。硬件置位,软件写CHTIFx位清除此标志。</td><td>0</td></tr><tr><td>29/25/21/17/13/9/5/1</td><td>TCIFx</td><td>RO</td><td>通道x的传输完成标志(x=1/2/3/4/5/6/7/8):1:在通道x上产生了传输完成事件;0:在通道x上没有传输完成事件。硬件置位,软件写CTCIFx位清除此标志。</td><td>0</td></tr><tr><td>28/24/20/16/12/8/4/0</td><td>GIFx</td><td>RO</td><td>通道x的全局中断标志(x=1/2/3/4/5/6/7/8):1:在通道x上产生了TEIFx或HTIFx或TCIFx;0:在通道x上没有发生TEIFx或HTIFx或TCIFx。硬件置位,软件写CGIFx位清除此标志。</td><td>0</td></tr></table>

注：通道 $^ { 8 }$ 适用于CH32V20x_D8、CH32V20x_D8W、CH32F20x_D8W、CH32F20x_D6、CH32V20x_D6。

### 11.3.2 DMAx 中断标志清除寄存器（DMAx_INTFCR）（ $\bf { \sigma } _ { x = 1 / 2 ) }$ ）

偏移地址： $0 \times 0 . 4 + ( \times - 1 ) \ast 0 \times 4 0 0$

<table><tr><td>31</td><td>30</td><td>29</td><td>28</td><td>27</td><td>26</td><td>25</td><td>24</td><td>23</td><td>22</td><td>21</td><td>20</td><td>19</td><td>18</td><td>17</td><td>16</td></tr><tr><td>CTEIF8</td><td>CHTIF8</td><td>CTCIF8</td><td>CGIF8</td><td>CTEIF7</td><td>CHTIF7</td><td>CTCIF7</td><td>CGIF7</td><td>CTEIF6</td><td>CHTIF6</td><td>CTCIF6</td><td>CGIF6</td><td>CTEIF5</td><td>CHTIF5</td><td>CTCIF5</td><td>CGIF5</td></tr><tr><td>15</td><td>14</td><td>13</td><td>12</td><td>11</td><td>10</td><td>9</td><td>8</td><td>7</td><td>6</td><td>5</td><td>4</td><td>3</td><td>2</td><td>1</td><td>0</td></tr><tr><td>CTEIF4</td><td>CHTIF4</td><td>CTCIF4</td><td>CGIF4</td><td>CTEIF3</td><td>CHTIF3</td><td>CTCIF3</td><td>CGIF3</td><td>CTEIF2</td><td>CHTIF2</td><td>CTCIF2</td><td>CGIF2</td><td>CTEIF1</td><td>CHTIF1</td><td>CTCIF1</td><td>CGIF1</td></tr></table>

<table><tr><td>位</td><td>名称</td><td>访问</td><td>描述</td><td>复位值</td></tr><tr><td>31/27/23/19/15/11/7/3</td><td>CTEIFx</td><td>WO</td><td>清除通道x的传输错误标志（x=1/2/3/4/5/6/8）：1：清除DMA_INTF寄存器中的TEIFx标志；0：无作用。</td><td>0</td></tr><tr><td>30/26/22/18/14/10/6/2</td><td>CHTIFx</td><td>WO</td><td>清除通道x的传输过半标志(x=1/2/3/4/5/6/8):1:清除DMA_INTFRC寄存器中的HTIFx标志;0:无作用。</td><td>0</td></tr><tr><td>29/25/21/17/13/9/5/1</td><td>CTCIFx</td><td>WO</td><td>清除通道x的传输完成标志(x=1/2/3/4/5/6/7/8):1:清除DMA_INTFRC寄存器中的TCIFx标志;0:无作用。</td><td>0</td></tr><tr><td>28/24/20/16/12/8/4/0</td><td>CGIFx</td><td>WO</td><td>清除通道x的全局中断标志(x=1/2/3/4/5/6/7/8):1:清除DMA_INTFRC寄存器中的TEIFx/HTIFx/TCIFx/GIFx标志;0:无作用。</td><td>0</td></tr></table>

注：通道 $^ { 8 }$ 适用于CH32V20x_D8、CH32V20x_D8W、CH32F20x_D8W、CH32F20x_D6、CH32V20x_D6。

### 11.3.3 DMAy 通道 $\pmb { \times }$ 配置寄存器（DMAy_CFGRx）（x=1/2/3/4/5/6/7/8，y=0/1）

偏移地址： $0 \times 0 8 + ( \times - 1 ) * 2 0 + ( \times - 1 ) * 0 \times 4 0 0$

<table><tr><td>31</td><td>30</td><td>29</td><td>28</td><td>27</td><td>26</td><td>25</td><td>24</td><td>23</td><td>22</td><td>21</td><td>20</td><td>19</td><td>18</td><td>17</td><td>16</td></tr><tr><td colspan="16">Reserved</td></tr><tr><td>15</td><td>14</td><td>13</td><td>12</td><td>11</td><td>10</td><td>9</td><td>8</td><td>7</td><td>6</td><td>5</td><td>4</td><td>3</td><td>2</td><td>1</td><td>0</td></tr><tr><td>Reserved</td><td>MEM2
MEM</td><td colspan="2">PL[1:0]</td><td colspan="2">MSIZE[1:0]</td><td colspan="2">PSIZE[1:0]</td><td>MINC</td><td>PINC</td><td>CIRC</td><td>DIR</td><td>TEIE</td><td>HTIE</td><td>TCIE</td><td>EN</td></tr></table>

<table><tr><td>位</td><td>名称</td><td>访问</td><td>描述</td><td>复位值</td></tr><tr><td>[31:15]</td><td>Reserved</td><td>R0</td><td>保留。</td><td>0</td></tr><tr><td>14</td><td>MEM2MEM</td><td>RW</td><td>存储器到存储器模式使能:1:使能存储器到存储器数据传输模式;0:非存储器到存储器数据传输。</td><td>0</td></tr><tr><td>[13:12]</td><td>PL[1:0]</td><td>RW</td><td>通道优先级设置:00:低; 01:中;10:高; 11:最高。</td><td>00b</td></tr><tr><td>[11:10]</td><td>MSIZE[1:0]</td><td>RW</td><td>存储器地址数据宽度设置:00:8位; 01:16位;10:32位; 11:保留。</td><td>00b</td></tr><tr><td>[9:8]</td><td>PSIZE[1:0]</td><td>RW</td><td>外设地址数据宽度设置:00:8位; 01:16位;10:32位; 11:保留。</td><td>00b</td></tr><tr><td>7</td><td>MINC</td><td>RW</td><td>存储器地址增量递增模式使能:1:使能存储器地址增量递增操作;0:存储器地址保持不变操作。</td><td>0</td></tr><tr><td>6</td><td>PINC</td><td>RW</td><td>外设地址增量递增模式使能:1:使能外设地址增量递增操作;0:外设地址保持不变操作。</td><td>0</td></tr><tr><td>5</td><td>CIRC</td><td>RW</td><td>DMA通道循环模式使能:1:使能循环操作;0:执行单次操作。</td><td>0</td></tr><tr><td>4</td><td>DIR</td><td>RW</td><td>数据传输方向:</td><td>0</td></tr><tr><td></td><td></td><td></td><td>1:从存储器读;0:从外设读。</td><td></td></tr><tr><td>3</td><td>TEIE</td><td>RW</td><td>传输错误中断使能控制:1:使能传输错误中断;0:禁止传输错误中断。</td><td>0</td></tr><tr><td>2</td><td>HTIE</td><td>RW</td><td>传输过半中断使能控制:1:使能传输过半中断;0:禁止传输过半中断。</td><td>0</td></tr><tr><td>1</td><td>TCIE</td><td>RW</td><td>传输完成中断使能控制:1:使能传输完成中断;0:禁止传输完成中断。</td><td>0</td></tr><tr><td>0</td><td>EN</td><td>RW</td><td>通道使能控制:1:通道开启;0:通道关闭。发生DMA传输错误时,硬件自动将此位清0,关闭通道。</td><td>0</td></tr></table>

注：通道 $^ { 8 }$ 适用于CH32V20x_D8、CH32V20x_D8W、CH32F20x_D8W、CH32F20x_D6、CH32V20x_D6。

### 11.3.4 DMAy 通道 $\pmb { \times }$ 传输数据数目寄存器（DMAy_CNTRx）（x=1/2/3/4/5/6/7/8，y=0/1）

偏移地址： $0 \times 0 0 + ( x - 1 ) \times 2 0 + ( y - 1 ) \ast 0 \times 4 0 0$

<table><tr><td>31</td><td>30</td><td>29</td><td>28</td><td>27</td><td>26</td><td>25</td><td>24</td><td>23</td><td>22</td><td>21</td><td>20</td><td>19</td><td>18</td><td>17</td><td>16</td></tr><tr><td colspan="16">Reserved</td></tr><tr><td>15</td><td>14</td><td>13</td><td>12</td><td>11</td><td>10</td><td>9</td><td>8</td><td>7</td><td>6</td><td>5</td><td>4</td><td>3</td><td>2</td><td>1</td><td>0</td></tr><tr><td colspan="16">NDT[15:0]</td></tr></table>

<table><tr><td>位</td><td>名称</td><td>访问</td><td>描述</td><td>复位值</td></tr><tr><td>[31:16]</td><td>Reserved</td><td>R0</td><td>保留。</td><td>0</td></tr><tr><td>[15:0]</td><td>NDT[15:0]</td><td>RW</td><td>数据传输数量,范围0-65535。这个寄存器只能在通道不工作(DMA_CFGRx的EN=0)时写入。通道开启后该寄存器变为只读,指示剩余的待传输字节数目(寄存器内容在每次DMA传输后递减)。在通道为循环模式下,寄存器的内容将被自动重新加载为之前配置的数值。</td><td>0</td></tr></table>

注：此寄存器只能在EN=0时更改；EN=1时，为只读寄存器，表示当前待传输字节数目。当寄存器内容为0时，无论通道是否开启，都不会发生任何数据传输。通道8适用于CH32V20x_D8、CH32V20x_D8W、CH32F20x_D8W、CH32F20x_D6、CH32V20x_D6。

### 11.3.5 DMAy 通道 $\pmb { \times }$ 外设地址寄存器（DMAy_PADDRx）（x=1/2/3/4/5/6/7/8）

偏移地址： $0 \times 1 0 + ( \times - 1 ) * 2 0 + ( \times - 1 ) * 0 \times 4 0 0$

31 30 29 28 27 26 25 24 23 22 21 20 19 18 17 16 15 14 13 12 11 10 9 8 7 6 5 4 3 2 1 0

PA[31:0]

<table><tr><td>位</td><td>名称</td><td>访问</td><td>描述</td><td>复位值</td></tr><tr><td>[31:0]</td><td>PA[31:0]</td><td>RW</td><td>外设基地址，作为外设数据传输的源或目标地址。</td><td>0</td></tr><tr><td></td><td></td><td></td><td>当PSIZE[1:0]='01' (16位),模块自动忽略bit0,操作地址自动2字节对齐;当PSIZE[1:0]='10' (32位),模块自动忽略bit[1:0],操作地址自动4字节对齐。</td><td></td></tr></table>

注：此寄存器只能在 $E N { = } 0$ 时更改， $\bar { E N } { = } 1$ 时不可写。通道8适用于CH32V20x_D8、CH32V20x_D8W、CH32F20x_D8W、CH32F20x_D6、CH32V20x_D6。

### 11.3.6 DMAy 通道 $\pmb { \times }$ 存储器地址寄存器（DMAy_MADDRx）（x=1/2/3/4/5/6/7/8）

偏移地址： $0 \times 1 4 + ( x - 1 ) \ast 2 0 + ( y - 1 ) \ast 0 \times 4 0 0$

31 30 29 28 27 26 25 24 23 22 21 20 19 18 17 16 15 14 13 12 11 10 9 8 7 6 5 4 3 2 1 0

MA[31:0]

<table><tr><td>位</td><td>名称</td><td>访问</td><td>描述</td><td>复位值</td></tr><tr><td>[31:0]</td><td>MA[31:0]</td><td>RW</td><td>存储器数据地址,作为数据传输的源或目标地址。当MSIZE[1:0]=&#x27;01&#x27;(16位),模块自动忽略bit0,操作地址自动2字节对齐;当MSIZE[1:0]=&#x27;10&#x27;(32位),模块自动忽略bit[1:0],操作地址自动4字节对齐。</td><td>0</td></tr></table>

注：此寄存器只能在 $E N { = } 0$ 时更改，EN=1时不可写。通道8适用于CH32V20x_D8、CH32V20x_D8W、CH32F20x_D8W、CH32F20x_D6、CH32V20x_D6。

### 11.3.7 DMA2 通道 $\pmb { \times }$ 配置寄存器（DMA2_CFGRx）（x=8/9/10/11）

偏移地址： $0 \times 4 9 0 + ( \times - 8 ) \times 1 6$

<table><tr><td colspan="16">Reserved</td></tr><tr><td>15</td><td>14</td><td>13</td><td>12</td><td>11</td><td>10</td><td>9</td><td>8</td><td>7</td><td>6</td><td>5</td><td>4</td><td>3</td><td>2</td><td>1</td><td>0</td></tr><tr><td>Reserved</td><td>MEM2
MEM</td><td colspan="2">PL[1:0]</td><td colspan="2">MSIZE[1:0]</td><td colspan="2">PSIZE[1:0]</td><td>MINC</td><td>PINC</td><td>CIRC</td><td>DIR</td><td>TEIE</td><td>HTIE</td><td>TCIE</td><td>EN</td></tr></table>

<table><tr><td>位</td><td>名称</td><td>访问</td><td>描述</td><td>复位值</td></tr><tr><td>[31:15]</td><td>Reserved</td><td>R0</td><td>保留。</td><td>0</td></tr><tr><td>14</td><td>MEM2MEM</td><td>RW</td><td>存储器到存储器模式使能:1:使能存储器到存储器数据传输模式;0:非存储器到存储器数据传输。</td><td>0</td></tr><tr><td>[13:12]</td><td>PL[1:0]</td><td>RW</td><td>通道优先级设置:00:低; 01:中;10:高; 11:最高。</td><td>00b</td></tr><tr><td>[11:10]</td><td>MSIZE[1:0]</td><td>RW</td><td>存储器地址数据宽度设置:00:8位; 01:16位;10:32位; 11:保留。</td><td>00b</td></tr><tr><td>[9:8]</td><td>PSIZE[1:0]</td><td>RW</td><td>外设地址数据宽度设置:00:8位; 01:16位;10:32位; 11:保留。</td><td>00b</td></tr><tr><td>7</td><td>MINC</td><td>RW</td><td>存储器地址增量递增模式使能:</td><td>0</td></tr><tr><td></td><td></td><td></td><td>1:使能存储器地址增量递增操作;0:存储器地址保持不变操作。</td><td></td></tr><tr><td>6</td><td>PINC</td><td>RW</td><td>外设地址增量递增模式使能:1:使能外设地址增量递增操作;0:外设地址保持不变操作。</td><td>0</td></tr><tr><td>5</td><td>CIRC</td><td>RW</td><td>DMA通道循环模式使能:1:使能循环操作;0:执行单次操作。</td><td>0</td></tr><tr><td>4</td><td>DIR</td><td>RW</td><td>数据传输方向:1:从存储器读;0:从外设读。</td><td>0</td></tr><tr><td>3</td><td>TEIE</td><td>RW</td><td>传输错误中断使能控制:1:使能传输错误中断;0:禁止传输错误中断。</td><td>0</td></tr><tr><td>2</td><td>HTIE</td><td>RW</td><td>传输过半中断使能控制:1:使能传输过半中断;0:禁止传输过半中断。</td><td>0</td></tr><tr><td>1</td><td>TCIE</td><td>RW</td><td>传输完成中断使能控制:1:使能传输完成中断;0:禁止传输完成中断。</td><td>0</td></tr><tr><td>0</td><td>EN</td><td>RW</td><td>通道使能控制:1:通道开启;0:通道关闭。发生DMA传输错误时,硬件自动将此位清0,关闭通道。</td><td>0</td></tr></table>

注：适用于CH32F20x_D8、CH32F20x_D8C、CH32V30x_D8、CH32V30x_D8C。

### 11.3.8 DMA2 通道 $\pmb { \times }$ 传输数据数目寄存器（DMA2_CNTRx）（x=8/9/10/11）

偏移地址： $0 { \times } 4 9 4 + ( { \times } - 8 ) { \ast } 1 6$

<table><tr><td>31</td><td>30</td><td>29</td><td>28</td><td>27</td><td>26</td><td>25</td><td>24</td><td>23</td><td>22</td><td>21</td><td>20</td><td>19</td><td>18</td><td>17</td><td>16</td></tr><tr><td colspan="16">Reserved</td></tr><tr><td>15</td><td>14</td><td>13</td><td>12</td><td>11</td><td>10</td><td>9</td><td>8</td><td>7</td><td>6</td><td>5</td><td>4</td><td>3</td><td>2</td><td>1</td><td>0</td></tr><tr><td colspan="16">NDT[15:0]</td></tr></table>

<table><tr><td>位</td><td>名称</td><td>访问</td><td>描述</td><td>复位值</td></tr><tr><td>[31:16]</td><td>Reserved</td><td>R0</td><td>保留。</td><td>0</td></tr><tr><td>[15:0]</td><td>NDT[15:0]</td><td>RW</td><td>数据传输数量,范围0-65535。这个寄存器只能在通道不工作(DMA_CFGRx的EN=0)时写入。通道开启后该寄存器变为只读,指示剩余的待传输字节数目(寄存器内容在每次DMA传输后递减)。在通道为循环模式下,寄存器的内容将被自动重新加载为之前配置的数值。</td><td>0</td></tr></table>

注：此寄存器只能在EN=0时更改；EN=1时，为只读寄存器，表示当前待传输字节数目。当寄存器内容为0时，无论通道是否开启，都不会发生任何数据传输，适用于CH32F20x_D8、CH32F20x_D8C、CH32V30x_D8、CH32V30x_D8C。

### 11.3.9 DMA2 通道 $\pmb { \times }$ 外设地址寄存器（DMA2_PADDRx）（x=8/9/10/11）

偏移地址： $0 \times 4 9 8 + ( \times - 8 ) \times 1 6$

31 30 29 28 27 26 25 24 23 22 21 20 19 18 17 16 15 14 13 12 11 10 9 8 7 6 5 4 3 2 1 0

PA[31:0]   

<table><tr><td>位</td><td>名称</td><td>访问</td><td>描述</td><td>复位值</td></tr><tr><td>[31:0]</td><td>PA[31:0]</td><td>RW</td><td>外设基地址，作为外设数据传输的源或目标地址。当PSIZE[1:0]=&#x27;01&#x27;（16位），模块自动忽略bit0，操作地址自动2字节对齐；当PSIZE[1:0]=&#x27;10&#x27;（32位），模块自动忽略bit[1:0]，操作地址自动4字节对齐。</td><td>0</td></tr></table>

注：此寄存器只能在EN=0时更改，EN=1时不可写，适用于CH32F20x_D8、CH32F20x_D8C、CH32V30x_D8、CH32V30x_D8C。

### 11.3.10 DMA2 通道 $\pmb { \times }$ 存储器地址寄存器（DMA2_MADDRx）（x=8/9/10/11）

偏移地址： $0 \times 4 9 0 + ( x - 8 ) \times 1 6$

31 30 29 28 27 26 25 24 23 22 21 20 19 18 17 16 15 14 13 12 11 10 9 8 7 6 5 4 3 2 1 0

MA[31:0]   

<table><tr><td>位</td><td>名称</td><td>访问</td><td>描述</td><td>复位值</td></tr><tr><td>[31:0]</td><td>MA[31:0]</td><td>RW</td><td>存储器数据地址,作为数据传输的源或目标地址。当MSIZE[1:0]=&#x27;01&#x27;(16位),模块自动忽略bit0,操作地址自动2字节对齐;当MSIZE[1:0]=&#x27;10&#x27;(32位),模块自动忽略bit[1:0],操作地址自动4字节对齐。</td><td>0</td></tr></table>

注：此寄存器只能在EN=0时更改，EN=1时不可写，适用于CH32F20×_D8、CH32F20x_D8C、CH32V30x_D8、CH32V30x_D8C。

### 11.3.11 DMA2 扩展中断状态寄存器（DMA2_EXTEM_INTFR）

偏移地址： $0 \times 4 0 0$

<table><tr><td>31</td><td>30</td><td>29</td><td>28</td><td>27</td><td>26</td><td>25</td><td>24</td><td>23</td><td>22</td><td>21</td><td>20</td><td>19</td><td>18</td><td>17</td><td>16</td></tr><tr><td colspan="16">Reserved</td></tr><tr><td>15</td><td>14</td><td>13</td><td>12</td><td>11</td><td>10</td><td>9</td><td>8</td><td>7</td><td>6</td><td>5</td><td>4</td><td>3</td><td>2</td><td>1</td><td>0</td></tr><tr><td>TEIF 11</td><td>HTIF 11</td><td>TCIF 11</td><td>GIF 11</td><td>TEIF 10</td><td>HTIF 10</td><td>TCIF 10</td><td>GIF 10</td><td>TEIF 9</td><td>HTIF 9</td><td>TCIF 9</td><td>GIF 9</td><td>TEIF 8</td><td>HTIF 8</td><td>TCIF 8</td><td>GIF 8</td></tr></table>

<table><tr><td>位</td><td>名称</td><td>访问</td><td>描述</td><td>复位值</td></tr><tr><td>[31:16]</td><td>Reserved</td><td>R0</td><td>保留。</td><td>0</td></tr><tr><td>15/11/7/3</td><td>TEIFx</td><td>R0</td><td>通道x的传输错误标志（x=8/9/10/11）：1：在通道x上发生了传输错误；0：在通道x上没有传输错误。硬件置位，软件写CTEIFx位清除此标志。</td><td>0</td></tr><tr><td>14/10/6/2</td><td>HTIFx</td><td>R0</td><td>通道x的传输过半标志（x=8/9/10/11）：</td><td>0</td></tr><tr><td></td><td></td><td></td><td>1: 在通道 x 上产生了传输过半事件;0: 在通道 x 上没有传输过半。硬件置位,软件写 CHTIFx 位清除此标志。</td><td></td></tr><tr><td>13/9/5/1</td><td>TCIFx</td><td>RO</td><td>通道 x 的传输完成标志 (x=8/9/10/11):1: 在通道 x 上产生了传输完成事件;0: 在通道 x 上没有传输完成事件。硬件置位,软件写 CTCIFx 位清除此标志。</td><td>0</td></tr><tr><td>12/8/4/0</td><td>GIFx</td><td>RO</td><td>通道 x 的全局中断标志 (x=8/9/10/11):1: 在通道 x 上产生了 TEIFx 或 HTIFx 或 TCIFx;0: 在通道 x 上没有发生 TEIFx 或 HTIFx 或 TCIFx。硬件置位,软件写 CGIFx 位清除此标志。</td><td>0</td></tr></table>

注：适用于CH32F20x_D8、CH32F20x_D8C、CH32V30x_D8、CH32V30x_D8C。

### 11.3.12 DMA2 扩展中断标志清除寄存器（DMA2_EXTEM_INTFCR）

偏移地址： $0 \times 4 0 4$

<table><tr><td colspan="17">Reserved</td></tr><tr><td>15</td><td>14</td><td>13</td><td>12</td><td>11</td><td>10</td><td>9</td><td>8</td><td>7</td><td>6</td><td>5</td><td>4</td><td>3</td><td>2</td><td>1</td><td>0</td><td></td></tr><tr><td>CTEIF11</td><td>CHTIF11</td><td>CTCIF11</td><td>CGIF11</td><td>CTEIF10</td><td>CHTI F10</td><td>CTCIF10</td><td>CGIF10</td><td>CTEIF9</td><td>CHTI F9</td><td>CTCIF9</td><td>CGIF9</td><td>CTEIF9</td><td>CTCIF8</td><td>CHTIF8</td><td>CTCIF8</td><td>CGIF8</td></tr></table>

<table><tr><td>位</td><td>名称</td><td>访问</td><td>描述</td><td>复位值</td></tr><tr><td>[31:16]</td><td>Reserved</td><td>RO</td><td>保留。</td><td>0</td></tr><tr><td>15/11/7/3</td><td>CTEIFx</td><td>WO</td><td>清除通道x的传输错误标志(x=8/9/10/11):1:清除DMA_INTFRC寄存器中的TEIFx标志;0:无作用。</td><td>0</td></tr><tr><td>14/10/6/2</td><td>CHTIFx</td><td>WO</td><td>清除通道x的传输过半标志(x=8/9/10/11):1:清除DMA_INTFRC寄存器中的HTIFx标志;0:无作用。</td><td>0</td></tr><tr><td>13/9/5/1</td><td>CTCIFx</td><td>WO</td><td>清除通道x的传输完成标志(x=8/9/10/11):1:清除DMA_INTFRC寄存器中的TCIFx标志;0:无作用。</td><td>0</td></tr><tr><td>12/8/4/0</td><td>CGIFx</td><td>WO</td><td>清除通道x的全局中断标志(x=8/9/10/11):1:清除DMA_INTFRC寄存器中的TEIFx/HTIFx/TCIFx/GIFx标志;0:无作用。</td><td>0</td></tr></table>

注：适用于CH32F20x_D8、CH32F20x_D8C、CH32V30x_D8、CH32V30x_D8C。


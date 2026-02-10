# 第 16 章 基本定时器（BCTM）

本章模块描述适用于CH32F2x、CH32V2x和CH32V3x微控制器全系列产品。

基本定时器模块包含一个16位可自动重装的定时器（TIM6和 TIM7），用于计数和在更新新事件产生中断或DMA请求。

## 16.1 主要特征

通用定时器的主要特征包括：

$\bullet$ 16 位自动重装计数器，支持增计数模式  
$\bullet$ 16 位预分频器，分频系数从 $1 \sim 6 5 5 3 6$ 之间动态可调  
$\bullet$ 触发 DAC 同步电路  
在更新事件时产生中断或 DMA 请求

## 16.2 原理和结构

![](images/d50401d691f969bdfa33805629d5acbe09dd8fa0c7a2e3e4f216b3ec7b6bff5a.jpg)  
图16-1 基本定时器的结构框图

### 16.2.1 概述

如图15-1所示，通用定时器的结构大致可以分为两部分，即输入时钟部分和核心计数器部分。

基本定时器的时钟来自于 AHB 总线时钟（CK_INT）。这些输入的时钟信号经过各种设定的滤波分频等操作后成为 CK_PSC 时钟，输出给核心计数器部分。另外，这些复杂的时钟来源还可以作为 TRGO输出至DAC外设。

基本定时器的核心是一个16位计数器（CNT）。CK_PSC 经过预分频器（PSC）分频后，成为 CK_CNT再最终输给CNT，CNT支持增计数模式，并有一个自动重装值寄存器（ATRLR）在每个计数周期结束后为CNT重装载初始化值。

### 16.2.2 基本定时器和通用定时器的区别

与通用定时器相比，基本定时器缺少以下功能：

1） 基本定时器缺少减计数模式和增减计数模式。  
2） 基本定时器缺少四路独立的比较捕获通道。  
3） 基本定时器不支持外部信号控制定时器。  
4） 基本定时器不支持增量式编码，定时器之间的级联和同步。

### 16.2.3 时钟输入

基本定时器的时钟由内部时钟 CK_INT 提供。

### 16.2.4 计数器和周边

CK_PSC输入给预分频器（PSC）进行分频。PSC是 16位的，实际的分频系数相当于 R16_TIMx_PSC的值 $+ 1$ 。CK_PSC 经过 PSC 会成为 CK_INT。更改 R16_TIM1_PSC 的值并不会实时生效，而会在更新事件后更新给PSC。更新事件包括UG位清零和复位。

### 16.3 调试模式

当系统进入调试模式时，根据 DBG 模块的设置可以控制定时器继续运转或者停止。

## 16.4 寄存器描述

表 16-1 TIM6 相关寄存器列表  

<table><tr><td>名称</td><td>偏移地址</td><td>描述</td><td>复位值</td></tr><tr><td>R16_TIM6_CTLR1</td><td>0x40001000</td><td>TIM6 控制寄存器1</td><td>0x0000</td></tr><tr><td>R16_TIM6_CTLR2</td><td>0x40001004</td><td>TIM6 控制寄存器2</td><td>0x0000</td></tr><tr><td>R16_TIM6_DMAINTENR</td><td>0x4000100C</td><td>TIM6 DMA/中断使能寄存器</td><td>0x0000</td></tr><tr><td>R16_TIM6_INTFR</td><td>0x40001010</td><td>TIM6 中断状态寄存器</td><td>0x0000</td></tr><tr><td>R16_TIM6_SWEVGR</td><td>0x40001014</td><td>TIM6 事件产生寄存器</td><td>0x0000</td></tr><tr><td>R16_TIM6_CNT</td><td>0x40001024</td><td>TIM6 计数器</td><td>0x0000</td></tr><tr><td>R16_TIM6_PSC</td><td>0x40001028</td><td>TIM6 计数时钟预分频器</td><td>0x0000</td></tr><tr><td>R16_TIM6_ATRLR</td><td>0x4000102C</td><td>TIM6 自动重装值寄存器</td><td>0x0000</td></tr></table>

表 16-2 TIM7 相关寄存器列表  

<table><tr><td>名称</td><td>偏移地址</td><td>描述</td><td>复位值</td></tr><tr><td>R16_TIM7_CTLR1</td><td>0x40001400</td><td>TIM7 控制寄存器1</td><td>0x0000</td></tr><tr><td>R16_TIM7_CTLR2</td><td>0x40001404</td><td>TIM7 控制寄存器2</td><td>0x0000</td></tr><tr><td>R16_TIM7_DMAINTENR</td><td>0x4000140C</td><td>TIM7 DMA/中断使能寄存器</td><td>0x0000</td></tr><tr><td>R16_TIM7_INTFR</td><td>0x40001410</td><td>TIM7 中断状态寄存器</td><td>0x0000</td></tr><tr><td>R16_TIM7_CNT</td><td>0x40001424</td><td>TIM7 计数器</td><td>0x0000</td></tr><tr><td>R16_TIM7_PSC</td><td>0x40001428</td><td>TIM7 计数时钟预分频器</td><td>0x0000</td></tr><tr><td>R16_TIM7_ATRLR</td><td>0x4000142C</td><td>TIM7 自动重装值寄存器</td><td>0x0000</td></tr></table>

### 16.4.1 控制寄存器 1（TIMx_CTLR1）（x=6/7）

偏移地址： $0 \times 0 0$

<table><tr><td>15</td><td>14</td><td>13</td><td>12</td><td>11</td><td>10</td><td>9</td><td>8</td><td>7</td><td>6</td><td>5</td><td>4</td><td>3</td><td>2</td><td>1</td><td>0</td></tr><tr><td colspan="8">Reserved</td><td>ARPE</td><td colspan="2">Reserved</td><td>OPM</td><td colspan="2">URS</td><td>UDIS</td><td>CEN</td></tr></table>

<table><tr><td>位</td><td>名称</td><td>访问</td><td>描述</td><td>复位值</td></tr><tr><td>[15:8]</td><td>Reserved</td><td>R0</td><td>保留。</td><td>0</td></tr><tr><td>7</td><td>ARPE</td><td>RW</td><td>自动重装预装使能位:1:使能自动重装值寄存器(ATRLR);0:禁止自动重装值寄存器(ATRLR)。</td><td>0</td></tr><tr><td>[6:4]</td><td>Reserved</td><td>R0</td><td>保留。</td><td>0</td></tr><tr><td>3</td><td>OPM</td><td>RW</td><td>单脉冲模式。1:在发生下一次更新事件(清除CEN位)时,计数器停止;0:在发生下一次更新事件时,计数器不停止。</td><td>0</td></tr><tr><td>2</td><td>URS</td><td>RW</td><td>更新请求源,软件通过该位选择UEV事件的源。1:如果使能了更新中断或DMA请求,则只有计数器溢出/下溢才产生更新中断或DMA请求;0:如果使能了更新中断或DMA请求,则下述任一事件产生更新中断或DMA请求:-计数器溢出/下溢-设置UG位-从模式控制器产生的更新</td><td>0</td></tr><tr><td>1</td><td>UDIS</td><td>RW</td><td>禁止更新,软件通过该位允许/禁止UEV事件的产生。1:禁止UEV。不产生更新事件,各寄存器(ATRLR、PSC、CHCTLRx)保持它们的值。如果设置了UG位或从模式控制器发出了一个硬件复位,则计数器和预分频器被重新初始化。0:允许UEV。更新(UEV)事件由下述任一事件产生:-计数器溢出/下溢-设置UG位-从模式控制器产生的更新具有缓存的寄存器被装入它们的预装载值。</td><td>0</td></tr><tr><td>0</td><td>CEN</td><td>RW</td><td>使能计数器(Counter enable)。1:使能计数器;0:禁止计数器。</td><td>0</td></tr></table>

### 16.4.2 控制寄存器 2（TIMx_CTLR2）（ $\bf { \chi } _ { x = 6 / 7 ) }$

偏移地址： $0 \times 0 4$

<table><tr><td>15</td><td>14</td><td>13</td><td>12</td><td>11</td><td>10</td><td>9</td><td>8</td><td>7</td><td>6</td><td>5</td><td>4</td><td>3</td><td>2</td><td>1</td><td>0</td></tr><tr><td colspan="9">Reserved</td><td colspan="3">MMS[2:0]</td><td colspan="4">Reserved</td></tr></table>

<table><tr><td>位</td><td>名称</td><td>访问</td><td>描述</td><td>复位值</td></tr><tr><td>[15:7]</td><td>Reserved</td><td>R0</td><td>保留。</td><td>0</td></tr><tr><td>[6:4]</td><td>MMS[2:0]</td><td>RW</td><td>主模式选择:这3位用于选择在主模式下送到从定时器的同步信息(TRGO)。可能的组合如下:000:复位-UG位被用于作为触发输出(TRGO)。如果是触发输入产生的复位(从模式控制器处于复位模式),则TRGO上的信号相对实际的复位会有一个延迟;001:使能-计数器使能信号CNT_EN被用于作为触发输出(TRGO)。有时需要在同一时间启动多个定时器或控制在一段时间内使能从定时器。计数器使能信号是通过CEN控制位和门控模式下的触发输入信号的逻辑或产生。当计数器使能信号受控于触发输入时,TRGO上会有一个延迟,除非选择了主/从模式(见TIMx_SMCFGR寄存器中MSM位的描述);010:更新事件被选为触发输入(TRGO)。例如,一个主定时器的时钟可以被用作一个从定时器的预分频器。</td><td>000b</td></tr><tr><td>[3:0]</td><td>Reserved</td><td>R0</td><td>保留。</td><td>0</td></tr></table>

### 16.4.3 DMA/中断使能寄存器（TIMx_DMAINTENR）（x=6/7）

偏移地址： $0 \times 0 0$

<table><tr><td>Reserved</td><td>UDE</td><td>Reserved</td><td>UIE</td></tr></table>

<table><tr><td>位</td><td>名称</td><td>访问</td><td>描述</td><td>复位值</td></tr><tr><td>[15:9]</td><td>Reserved</td><td>RO</td><td>保留。</td><td>0</td></tr><tr><td>8</td><td>UDE</td><td>RW</td><td>更新的 DMA 请求使能位。
1: 允许更新的 DMA 请求;
0: 禁止更新的 DMA 请求。</td><td>0</td></tr><tr><td>[7:1]</td><td>Reserved</td><td>RO</td><td>保留。</td><td>0</td></tr><tr><td>0</td><td>UIE</td><td>RW</td><td>更新中断使能位。
1: 允许更新中断;
0: 禁止更新中断。</td><td>0</td></tr></table>

### 16.4.4 中断状态寄存器（R16_TIMx_INTFR）（ $\bf { x = 6 / 7 ) }$

偏移地址： $0 \times 1 0$

<table><tr><td>15</td><td>14</td><td>13</td><td>12</td><td>11</td><td>10</td><td>9</td><td>8</td><td>7</td><td>6</td><td>5</td><td>4</td><td>3</td><td>2</td><td>1</td><td>0</td></tr><tr><td colspan="15">Reserved</td><td>UIF</td></tr></table>

<table><tr><td>位</td><td>名称</td><td>访问</td><td>描述</td><td>复位值</td></tr><tr><td>[15:1]</td><td>Reserved</td><td>R0</td><td>保留。</td><td>0</td></tr><tr><td>0</td><td>UIF</td><td>RWO</td><td>更新中断标志位，当产生更新事件时该位由硬件置</td><td>0</td></tr><tr><td></td><td></td><td></td><td>位,由软件清零。
1: 更新中断产生;
0: 无更新事件产生。
以下情形会产生更新事件:
若 UDIS=0, 当重复计数器数值上溢或下溢时;
若 URS=0、UDIS=0, 当置 UG 位时, 或当通过软件对计数器核心计数器重新初始化时;</td><td></td></tr></table>

### 16.4.5 事件产生寄存器（TIMx_SWEVGR）（ $\bf { x = 6 / 7 ) }$ ）

偏移地址：0x14

<table><tr><td>15</td><td>14</td><td>13</td><td>12</td><td>11</td><td>10</td><td>9</td><td>8</td><td>7</td><td>6</td><td>5</td><td>4</td><td>3</td><td>2</td><td>1</td><td>0</td></tr><tr><td colspan="15">Reserved</td><td>UG</td></tr></table>

<table><tr><td>位</td><td>名称</td><td>访问</td><td>描述</td><td>复位值</td></tr><tr><td>[15:1]</td><td>Reserved</td><td>R0</td><td>保留。</td><td>0</td></tr><tr><td>0</td><td>UG</td><td>WO</td><td>更新事件产生位,产生更新事件。该位由软件置位,由硬件自动清零。1:初始化计数器,并产生一个更新事件;0:无动作。注:预分频器的计数器也被清零,但是预分频系数不变。若在中心对称模式下或增计数模式下则核心计数器被清零;若减计数模式下则核心计数器取重装值寄存器的值。</td><td>0</td></tr></table>

### 16.4.6 通用定时器的计数器（TIMx_CNT）（x=6/7）

偏移地址： ${ 0 } { \times 2 4 }$

<table><tr><td>15</td><td>14</td><td>13</td><td>12</td><td>11</td><td>10</td><td>9</td><td>8</td><td>7</td><td>6</td><td>5</td><td>4</td><td>3</td><td>2</td><td>1</td><td>0</td></tr><tr><td colspan="16">CNT[15:0]</td></tr></table>

<table><tr><td>位</td><td>名称</td><td>访问</td><td>描述</td><td>复位值</td></tr><tr><td>[15:0]</td><td>CNT[15:0]</td><td>RW</td><td>定时器的计数器的实时值。</td><td>0</td></tr></table>

### 16.4.7 计数时钟预分频器（TIMx_PSC）（ $\bf { x } = 6 / 7 )$

偏移地址： $_ { 0 \times 2 8 }$

<table><tr><td>15</td><td>14</td><td>13</td><td>12</td><td>11</td><td>10</td><td>9</td><td>8</td><td>7</td><td>6</td><td>5</td><td>4</td><td>3</td><td>2</td><td>1</td><td>0</td></tr><tr><td colspan="16">PSC[15:0]</td></tr></table>

<table><tr><td>位</td><td>名称</td><td>访问</td><td>描述</td><td>复位值</td></tr><tr><td>[15:0]</td><td>PSC[15:0]</td><td>RW</td><td>定时器的预分频器的分频系数；计数器的时钟频率等于分频器的输入频率/(PSC+1)。</td><td>0</td></tr></table>

### 16.4.8 自动重装值寄存器（TIMx_ATRLR）（x=6/7）

偏移地址： $\mathtt { 0 } \mathtt { x } 2 0$

<table><tr><td>15</td><td>14</td><td>13</td><td>12</td><td>11</td><td>10</td><td>9</td><td>8</td><td>7</td><td>6</td><td>5</td><td>4</td><td>3</td><td>2</td><td>1</td><td>0</td></tr><tr><td colspan="16">ARR[15:0]</td></tr></table>

<table><tr><td>位</td><td>名称</td><td>访问</td><td>描述</td><td>复位值</td></tr><tr><td>[15:0]</td><td>ARR[15:0]</td><td>RW</td><td>ATRLR[15:0]的值将会被装入计数器，ATRLR何时动作和更新请阅读14.2.4节；ATRLR为空时，计数器停止。</td><td>0</td></tr></table>


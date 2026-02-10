# 第 12 章 模拟/数字转换（ADC）

本章模块描述适用于CH32F2x、CH32V2x和CH32V3x微控制器全系列产品。

ADC 模块包含2个12位的逐次逼近型的模拟数字转换器，最高 14MHz 的输入时钟。支持16个外部通道和2个内部信号源采样源。可完成通道的单次转换、连续转换，通道间自动扫描模式、间断模式、外部触发模式、双重采样等功能。可以通过模拟看门狗功能监测通道电压是否在阈值范围内。

## 12.1 主要特性

12 位分辨率  
支持 16 个外部通道和 2 个内部信号源采样  
$\bullet$ 多通道的多种采样转换方式：单次、连续、扫描、触发、间断等  
$\bullet$ 数据对齐模式：左对齐、右对齐  
$\bullet$ 采样时间可按通道分别编程  
$\bullet$ 规则转换和注入转换均支持外部触发  
$\bullet$ 模拟看门狗监测通道电压，自校准功能  
$\bullet$ 双重模式  
$\bullet$ ADC 通道输入范围： $0 \leqslant V _ { \mathsf { I N } } \leqslant V _ { \mathsf { D O A } }$   
输入增益可调，可实现小信号放大采样

## 12.2 功能描述

### 12.2.1 模块结构

![](images/c6d7da1fd51bfacc9b96f600f5f3063165830b18336559d977be35e66f5dfedb.jpg)  
图 12-1 ADC 模块框图

### 12.2.2 ADC 配置

#### 1）模块上电

ADC_CTLR2 寄存器的 ADON 位为 1 表示 ADC 模块上电。当 ADC 模块从断电模式（ADON=0）下进入上电状态（ADON=1）后，需要延迟一段时间 tSTAB用于模块稳定时间。之后再次写入 ADON 位为 1，用于作为软件启动ADC转换的启动信号。通过清除 ADON位为 0，可以终止当前转换并将 ADC模块置于断电模式，这个状态下，ADC几乎不耗电。

#### 2）采样时钟

模块的寄存器操作基于PCLK2（APB2总线）时钟，其转换单元的时钟基准 ADCCLK与 PCLK2同步，由 RCC_CFGR0 寄存器的 ADCPRE[1:0]域配置分频，最大不能超过 14MHz。

#### 3）通道配置

ADC 模块提供了18个通道采样源，包括 16个外部通道和 2个内部通道。它们可以配置到两种转换组中：规则组和注入组。以实现任意多个通道上以任意顺序进行一系列转换构成的组转换。

#### 转换组：

规则组：由多达16个转换组成。规则通道和它们的转换顺序在 ADC_RSQRx 寄存器中设置。规则组中转换的总数量应写入ADC_RSQR1寄存器的RLEN[3:0]中。  
注入组：由多达 4 个转换组成。注入通道和它们的转换顺序在 ADC_ISQR 寄存器中设置。注入组里的转换总数量应写入ADC_ISQR寄存器的 ILEN[1:0]中。

注：如果ADC_RSQRx或ADC_ISQR寄存器在转换期间被更改，当前的转换被终止，一个新的启动信号将发送到ADC以转换新选择的组。

#### 2 个内部通道：

$\bullet$ 温度传感器：连接ADC_IN16通道，用来测量器件周围的温度(TA)。  
VREFINT 内部参考电压：连接 ADC_IN17 通道。

#### 4）校准

ADC 有一个内置自校准模式。经过校准环节可大幅减小因内部电容器组的变化而造成的精准度误差。在校准期间，在每个电容器上都会计算出一个误差修正码，用于消除在随后的转换中每个电容器上产生的误差。

通过写ADC_CTLR2寄存器的RSTCAL位置1 初始化校准寄存器，等待 RSTCAL硬件清 0表示初始化完成。置位 CAL 位，启动校准功能，一旦校准结束，硬件会自动清除 CAL 位，将校准码存储到ADC_RDATAR中。之后可以开始正常的转换功能。建议在 ADC模块上电时执行一次 ADC校准。

注：启动校准前，必须保证ADC模块处于上电状态(ADON=1)超过至少两个ADC时钟周期。

#### 5）可编程采样时间

ADC 使用若干个ADCCLK周期对输入电压采样，通道的采样周期数目可以通过ADC_SAMPTR1 和ADC_SAMPTR2 寄存器中的 $\mathsf { S M P } \times [ 2 : 0 ]$ 位更改。每个通道可以分别使用不同的时间采样。

总转换时间如下计算：

TCONV $=$ 采样时间 $+ 1 2 . 5 \mathsf { T } _ { \mathsf { A D C C l K } }$

ADC的规则通道转换支持DMA功能。规则通道转换的值储存在一个仅有的数据寄存器 ADC_RDATAR中，为防止连续转换多个规则通道时，没有及时取走 ADC_RDATAR 寄存器中的数据，可以开启 ADC 的DMA功能。硬件会在规则通道的转换结束时（EOC置位）产生 DMA请求，并将转换的数据从 ADC_RDATAR寄存器传输到用户指定的目的地址。

对 DMA 控制器模块的通道配置完成后，写 ADC_CTLR2 寄存器的 DMA 位置 1，开启 ADC 的 DMA 功能。注：注入组转换不支持DMA功能。

#### 6）数据对齐

ADC_CTLR2寄存器中的ALIGN位选择ADC转换后的数据存储对齐方式。12 位数据支持左对齐和右对齐模式。

规则组通道的数据寄存器ADC_RDATAR保存的是实际转换的 12位数字值；而注入组通道的数据寄存器ADC_IDATARx是实际转换的数据减去 ADC_IOFRx寄存器的定义的偏移量后写入的值，会存在正负情况，所以有符号位（SIGNB）。

图 12-2 数据左对齐  
规则组数据寄存器  

<table><tr><td>D11</td><td>D10</td><td>D9</td><td>D8</td><td>D7</td><td>D6</td><td>D5</td><td>D4</td><td>D4</td><td>D2</td><td>D1</td><td>D0</td><td>0</td><td>0</td><td>0</td><td>0</td></tr></table>

注入组数据寄存器  

<table><tr><td>SIGNB</td><td>D11</td><td>D10</td><td>D9</td><td>D8</td><td>D7</td><td>D6</td><td>D5</td><td>D4</td><td>D3</td><td>D2</td><td>D1</td><td>D0</td><td>0</td><td>0</td><td>0</td></tr></table>

图 12-3 数据右对齐

规则组数据寄存器  

<table><tr><td>0</td><td>0</td><td>0</td><td>0</td><td>D11</td><td>D10</td><td>D9</td><td>D8</td><td>D7</td><td>D6</td><td>D5</td><td>D4</td><td>D3</td><td>D2</td><td>D1</td><td>D0</td></tr></table>

注入组数据寄存器  

<table><tr><td>SIGNB</td><td>SIGNB</td><td>SIGNB</td><td>SIGNB</td><td>D11</td><td>D10</td><td>D9</td><td>D8</td><td>D7</td><td>D6</td><td>D5</td><td>D4</td><td>D3</td><td>D2</td><td>D1</td><td>D0</td></tr></table>

### 12.2.3 外部触发源

ADC 转换的启动事件可以由外部事件触发。如果设置了 ADC_CTLR2 寄存器的 EXTTRIG 或 JEXTTRIG位，则可分别通过外部事件触发规则组或注入组通道的转换。此时，EXTSEL[2:0]和 JEXTSEL[2:0]位的配置决定规则组和注入组的外部事件源。

注：当外部触发信号被选为ADC规则或注入转换时，只有它的上升沿可以启动转换。

表 12-1 规则组通道的外部触发源  

<table><tr><td>EXTSEL[2:0]</td><td>触发源</td><td>类型</td></tr><tr><td>000</td><td>定时器1的CC1事件</td><td rowspan="6">来自片上定时器的内部信号</td></tr><tr><td>001</td><td>定时器1的CC2事件</td></tr><tr><td>010</td><td>定时器1的CC3事件</td></tr><tr><td>011</td><td>定时器2的CC2事件</td></tr><tr><td>100</td><td>定时器3的TRGO事件</td></tr><tr><td>101</td><td>定时器4的CC4事件</td></tr><tr><td>110</td><td>EXTI线11/TIM8_TRGO</td><td>来自外部引脚/内部定时器信号</td></tr><tr><td>111</td><td>SWSTART位置1软件触发</td><td>软件控制位</td></tr></table>

表12-2 注入组通道的外部触发源  

<table><tr><td>JEXTSEL[2:0]</td><td>触发源</td><td>类型</td></tr><tr><td>000</td><td>定时器1的TRGO事件</td><td rowspan="6">来自片上定时器的内部信号</td></tr><tr><td>001</td><td>定时器1的CC4事件</td></tr><tr><td>010</td><td>定时器2的TRGO事件</td></tr><tr><td>011</td><td>定时器2的CC1事件</td></tr><tr><td>100</td><td>定时器3的CC4事件</td></tr><tr><td>101</td><td>定时器4的TRGO事件</td></tr><tr><td>110</td><td>EXTI线15/TIM8_CC4</td><td>来自外部引脚/内部定时器信号</td></tr><tr><td>111</td><td>JSWSTART位置1软件触发</td><td>软件控制位</td></tr></table>

### 12.2.4 转换模式

表12-3 转换模式组合  

<table><tr><td colspan="5">ADC_CTLR1和ADC_CTLR2寄存器控制位</td><td rowspan="2">ADC转换模式</td></tr><tr><td>CONT</td><td>SCAN</td><td>RDISCEN/IDISCEN</td><td>IAUTO</td><td>启动事件</td></tr><tr><td rowspan="3">0</td><td rowspan="2">0</td><td rowspan="2">0</td><td rowspan="2">0</td><td>ADON位置1</td><td>单次单通道模式:某一规则通道执行单次转换。</td></tr><tr><td>外部触发方式</td><td>单次单通道模式:规则通道或注入通道的某一通道执行单次转换。</td></tr><tr><td>1</td><td>0</td><td>0</td><td>ADON位置</td><td>单次扫描模式:按顺序对选中的所有规则组</td></tr><tr><td rowspan="5"></td><td rowspan="2"></td><td rowspan="2"></td><td></td><td>1或外部触发方式</td><td>通道(ADC_RSQRx)或所有注入组通道(ADC_ISQR)逐个执行单次转换。触发注入方式:当规则组通道转换过程中可以插入注入组通道所有转换,之后再继续规则组通道转换;但转换注入组通道时不会插入规则组通道转换。</td></tr><tr><td>1</td><td>ADON位置1或外部触发方式</td><td>单次扫描模式:按顺序对选中的所有规则组通道(ADC_RSQRx)或所有注入组通道(ADC_ISQR)逐个执行单次转换。自动注入方式:在规则组通道转换完之后,注入组通道被自动转换。注:转换过程中不允许出现注入通道的外部触发信号。</td></tr><tr><td rowspan="2">0</td><td rowspan="2">1(RDISCEN和IDISCEN不能同时为1)</td><td>0</td><td>外部触发方式</td><td>单次间断模式:每次启动事件,执行一个短序列(DISCNUM[2:0]定义数量)的通道数量转换,直到所有选中通道转换完成才能重头开始。注:规则组和注入组选中此模式控制位分别为IDISCEN和RDISCEN,不能同时为规则组和注入组配置间断模式,间断模式只能用于一组转换。</td></tr><tr><td>1</td><td>-</td><td>禁止此模式。</td></tr><tr><td>1</td><td>1</td><td>X</td><td>-</td><td>无此模式。</td></tr><tr><td rowspan="3">1</td><td>0</td><td>0</td><td>0</td><td rowspan="3">ADON位置1或外部触发方式</td><td rowspan="3">连续单通道/扫描模式:每轮结束后重复新一轮的转换,直到CONT清0才能终止。</td></tr><tr><td rowspan="2">1</td><td rowspan="2">0</td><td>0</td></tr><tr><td>1</td></tr></table>

注：规则组和注入组的外部触发事件是不一样的，而且‘ACON’位只能启动规则组通道转换，所以规则组和注入组通道转换的启动事件独立。

#### 1）单次单通道转换模式

此模式下，对当前 1 个通道只执行一次转换。该模式对规则组或注入组中排序第 1 的通道执行转换，其中通过设置 ADC_CTLR2 寄存器的 ADON 位置 1(只适用于规则通道)启动也可通过外部触发启动(适用于规则通道或注入通道)。一旦选择通道的转换完成将：

如果转换的是规则组通道，则转换数据被储存在 16 位 ADC_RDATAR 寄存器中，EOC 标志被置位，如果设置了EOCIE位，将触发ADC中断。

如果转换的是注入组通道，则转换数据被储存在 16 位 ADC_IDATAR1 寄存器中，EOC 和 JEOC 标志被置位，如果设置了 JEOCIE 或 EOCIE 位，将触发 ADC 中断。

#### 2）单次扫描模式转换

通过设置 ADC_CTLR1 寄存器的 SCAN 位为 1 进入 ADC 扫描模式。此模式用来扫描一组模拟通道，对被 ADC_RSQRx 寄存器(对规则通道)或 ADC_ISQR(对注入通道)选中的所有通道逐个执行单次转换，当前通道转换结束时，同一组的下一个通道被自动转换。

在扫描模式里，根据 IAUTO 位的状态，又分为触发注入方式和自动注入方式。

#### 触发注入

IAUTO 位为 0，当在扫描规则组通道过程中，发生了注入组通道转换的触发事件，当前转换被复位，注入通道的序列被以单次扫描方式进行，在所有选中的注入组通道扫描转换结束后，恢复上次被

中断的规则组通道转换。

如果当前在扫描注入组通道序列时，发生了规则通道的启动事件，注入组转换不会被中断，而是在注入序列转换完成后再执行规则序列的转换。

注：使用触发的注入转换时，必须保证触发事件的间隔长于注入序列。例如，完成注入序列的转换总体时间需要28个ADCCLK，那么触发注入通道的事件间隔时间最小值为29个ADCCLK。

#### 自动注入

IAUTO 位为 1，在扫描完规则组选中的所有通道转换后，自动进行注入组选中通道的转换。这种方式可以用来转换ADC_RSQRx和ADC_ISQR寄存器中多达 20个转换序列。

此模式里，必须禁止注入通道的外部触发（JEXTTRIG=0）。

注：对于ADC时钟预分频系数（ADCPRE[1:0]）为4至8时，当从规则转换切换到注入序列或从注入转换切换到规则序列时，会自动插入1个ADCCLK间隔；当ADC时钟预分频系数为2时，则有2个间隔的延迟。

#### 3）单次间断模式转换

通过设置 ADC_CTLR1 寄存器的 RDISCEN 或 IDISCEN 位为 1 进入规则组或注入组的间断模式。此模式区别扫描模式中扫描完整的一组通道，而是将一组通道分为多个短序列，每次外部触发事件将执行一个短序列的扫描转换。

短序列的长度n（ $n < = 8 1$ ）定义在 ADC_CTLR1 寄存器的 DISCNUM[2:0]中，当 RDISCEN 为 1，则是规则组的间断模式，待转换总长度定义在 ADC_RSQR1 寄存器的 RLEN[3:0]中；当 IDISCEN 为 1，则是注入组的间断模式，待转换总长度定义在 ADC_ISQR 寄存器的 ILEN[1:0]中。不能同时将规则组和注入组设置为间断模式。

规则组间断模式举例：

RDISCEN=1，DISCNUM[2:0]=3，RLEN[3:0]=8，待转换通道 $\cdot = 1$ ，3，2，5，8，4，10，6

第 1 次外部触发：转换序列为：1，3，2

第 2 次外部触发：转换序列为：5，8，4

第 3 次外部触发：转换序列为：10，6，同时产生 EOC 事件

第 4 次外部触发：转换序列为：1，3，2

注入组间断模式举例：

IDISCEN=1，ILEN[1:0]=3，待转换通道=1，3，2

第 1 次外部触发：转换序列为：1

第2次外部触发：转换序列为：3

第 3 次外部触发：转换序列为：2，同时产生 EOC 和 JEOC 事件

第 4 次外部触发：转换序列为：1

注：1.当以间断模式转换一个规则组或注入组时，转换序列结束后不自动从头开始。当所有子组被转换完成，下一次触发事件启动第一个子组的转换。

2.不能同时使用自动注入（ $1 A U T O = 1 .$ ）和间断模式。

3.不能同时为规则组和注入组设置间断模式，间断模式只能用于一组转换。

注入组的间断模式下， 外部触发后要转换的注入通道数目为 1。

#### 4）连续转换

通过设置 ADC_CTLR2 寄存器的 CONT 位为 1，进入 ADC 的连续转换模式。此模式在前面 ADC 转换一结束马上就启动另一次转换，转换不会在选择组的最后一个通道上停止，而是再次从选择组的第一个通道继续转换。

启动事件包括外部触发事件和 ADON 位置 1。结合前面的单次模式中的几种转换方式，也包括连续单通道转换、连续扫描模式（触发注入或自动注入）转换。

### 12.2.5 模拟看门狗

如果被 ADC转换的模拟电压低于低阀值或高于高阀值，AWD模拟看门狗状态位被设置。阀值设置位于 ADC_WDHTR 和 ADC_WDLTR 寄存器的最低 12 个有效位中。通过设置 ADC_CTLR1 寄存器的 AWDIE位以允许产生相应中断。

![](images/e6546e2567d8b223b610f4646c46b3c7fc447d2803362bae6f6946b99e30b520.jpg)  
图 12-4 模拟看门狗阈值区

配置 ADC_CTLR1 寄存器的 AWDSGL、RAWDEN、IAWDEN 及 AWDCH[4:0]位选择模拟看门狗警戒的通道，具体关系见下表：

表12-4 模拟看门狗通道选择  

<table><tr><td rowspan="2">模拟看门狗警戒通道</td><td colspan="4">ADC_CTLR1 寄存器控制位</td></tr><tr><td>AWDSGL</td><td>RAWDEN</td><td>IAWDEN</td><td>AWDCH[4:0]</td></tr><tr><td>不警戒</td><td>忽略</td><td>0</td><td>0</td><td>忽略</td></tr><tr><td>所有注入通道</td><td>0</td><td>0</td><td>1</td><td>忽略</td></tr><tr><td>所有规则通道</td><td>0</td><td>1</td><td>0</td><td>忽略</td></tr><tr><td>所有注入和规则通道</td><td>0</td><td>1</td><td>1</td><td>忽略</td></tr><tr><td>单一注入通道</td><td>1</td><td>0</td><td>1</td><td>决定通道编号</td></tr><tr><td>单一规则通道</td><td>1</td><td>1</td><td>0</td><td>决定通道编号</td></tr><tr><td>单一注入和规则通道</td><td>1</td><td>1</td><td>1</td><td>决定通道编号</td></tr></table>

### 12.2.6 温度传感器

模块内置温度传感器，连接ADC_INT16通道，通过ADC 将传感器输出的电压转换成数字值来反馈器件周围温度，推荐设置采样时间是17.1us。温度传感器输出的电压随温度线性变化，由于生产差异，其线性变化的曲线斜率和偏移有所不同，所以内部温度传感器更适合于检测温度的变化，而不是测量绝对的温度。如果需要测量精确的温度，应该使用一个外置的温度传感器。

通过设置ADC_CTLR2寄存器的TSVREFE位置 1，唤醒 ADC内部采样通道，软件启动或外部触发启动ADC的温度传感器通道转换，读取数据结果（mV）。其中，数字值和温度 ${ \mathcal { C } } )$ 换算公式如下：

温度 $( ^ { \circ } C ) \ = \ ( \ ( \lor _ { { \tt S E N S E } } - \lor _ { { \tt 2 5 } } ) / \mathsf { A v g \_ S l o p e } ) + 2 5$ $^ { + 2 5 }$

V25：温度传感器在 $2 5 \mathrm { ^ \circ C }$ 下的电压值

Avg_Slope：温度与 VSENSE 曲线的平均斜率 $\mathsf { \Omega } ( \mathsf { m } \mathsf { V } / ^ { \circ } \mathsf { C } $ ）

参考数据手册电气特性章节中 $\mathsf { V } _ { 2 5 }$ 和 Avg_Slope 的实际值。

注：内部温度传感器上电（TSVREFE位从0改为1）需要一个建立时间，而ADC模块上电也需要一个建立时间（ADON位从 $o$ 改为1），所以为了缩短等待时间，可以同时设置ADON和TSVREFE位。

### 12.2.7 双 ADC 模式

在有 2个的ADC模块产品中，可以使2个 ADC配合使用，实现双 ADC模式。在双 ADC模式中，ADC1 为主 ADC，ADC2 为从 ADC。通过配置 ADC1_CTLR1 中 DAULMODE[3:0]以选择模式，实现 ADC1 和ADC2 交替触发或同步触发转换。

注：双ADC模式中，选择外部触发事件触发时，用户必须使能主从ADC的外部触发使能且需要将主

设为相应的触发 从ADC设置为软件触发 防止不必要的触发使从ADC进行转换。

通过配置可以实现以下几种可能的模式：

独立模式  
同步注入模式  
$\bullet$ 同步规则模式  
$\bullet$ 快速交替模式  
$\bullet$ 慢速交替模式  
$\bullet$ 交替触发模式  
$\bullet$ 同步规则模式 $^ +$ 同步注入模式  
$\bullet$ 同步规则模式 $^ +$ 交替触发模式  
$\bullet$ 同步注入模式 $^ { + }$ 快速交替模式  
$\bullet$ 同步注入模式 $^ +$ 慢速交替模式

注：1.双ADC模式中，为了主数据寄存器能够读取从ADC的转换数据，需要使能DMA位。

2.只有ADC1拥有DMA功能。而ADC2转化的数据只可以通过双ADC模式，利用ADC1的 DMA功能传输。

![](images/5d279e6bcf66f391e5e8aa6473d505e7e63669bbd08ffbb3751a41f020106f14.jpg)  
图 12-5 双 ADC 框图

#### 1）独立模式

此模式下，双 ADC 不同步工作，相互之间独立工作。

#### 2）同步注入模式

此模式下用于转换一个注入通道组，设置 ADC1_CTLR2中 JEXTSEL[2:0]，以选择触发源,同时它也将用于同步触发ADC2。转换完成时，转换后的数据分别存储在 $\mathsf { A D C x }$ 的 ADC_IDATARx 中，且若使能任一ADC中断，转换结束后将产生JEOC中断。

![](images/feac0f027f963a63076a674bbe0085b3c4d3dde81728a72123211685b9846351.jpg)  
图12-6 4通道同步注入转换

注：1.同一时刻 ADC1和ADC2的转换通道不应重合。  
2.同步模式下，ADC1和ADC2应有相同时间长度的转换序列或两者之中较长的转换序列的时长小于触发时间间隔，以保证每次触发两个序列均能转换完成。

#### 3）同步规则模式

此模式下用于转换规则通道序列，设置 ADC1_CTLR2中 EXTSEL[2:0]，以选择触发源，同时它也将用于同步触发ADC2。转换完成时，将产生一个 32位 DMA传输请求，将数据寄存器 ADC1_RDATAR的内容传输到 SRAM 中，高 16 位包含 ADC2 转换数据，低 16 位包含 ADC1 转换数据。若使能任一 ADC中断，将产生EOC中断。

![](images/6799a21698f605937424a51484046e3d9c9485a757f6988bb4b52debe23e43ad.jpg)  
图12-7 16通道同步规则转换

注：1.同一时刻ADC1和ADC2的转换通道不应重合；  
2.同步模式下，ADC1和ADC2应有相同时间长度的转换序列或两者之中较长的转换序列的时长小于触发时间间隔，以保证每次触发两个序列均能转换完成。

#### 4）快速交替模式

此模式仅适用于规则通道（往往只有一个通道），设置 ADC1_CTLR2 中 EXTSEL[2:0]，以选择触发源，当触发产生后 ADC2 将立即启动转换，ADC1 在延迟 7 个 ADC 时钟周期后启动转换。如果均开启连续模式（CONT 被置位），两个 ADC 将对规则通道连续交替转换。如果使能中断，ADC1 将产生EOC 中断，若同时使能 DMA，则将产生一个 32 位的 DMA 传输请求，将数据寄存器 ADC1_RDATAR 的内容传输到 SRAM 中，高 16 位包含 ADC2 转换数据，低 16 位包含 ADC1 转换数据。

![](images/fcb122cf89db8854b35fb4e9ffe0a51454f51904be31655bfb7bc5aa02d7fdd1.jpg)  
图12-8 单通道快速交替连续转换

![](images/4d2f9b634f78ee64e4b974c6437debc057e55f6359d21ce698e12ce46fb78743.jpg)  
注：采样时间应小于7个ADC时钟周期，以避免ADC1和ADC2在采样同一个通道时，出现采样周期重合的问题。

#### 5）慢速交替模式

此模式仅适用于规则通道且只能一个通道。设置 ADC1_CTLR2中 EXTSEL[2:0]，以选择触发源，当触发产生后ADC2将立即启动转换，ADC1 在延迟 14个 ADC时钟周期后启动转换，再次延时 14个ADC时钟周期后ADC2再次启动，如此往复循环。若使能中断，ADC1 将产生 EOC中断，若同时使能DMA，则将产生一个 32 位的 DMA 传输请求，将数据寄存器 ADC1_RDATAR 的内容传输到 SRAM 中，高16位包含 ADC2转换数据，低16位包含ADC1转换数据。

![](images/8a51c6a875100e15b7671d3fc7151cb948dee0432d0a0200863acd68cb81d9e0.jpg)  
图12-9 单通道慢速交替转换

![](images/ea1435d5c6026e04572051f9e28650d849ead4dcc59773878274ee936214ff7f.jpg)  
注：1.采样时间应小于14个ADC时钟周期，以避免和下一次采样周期重合；

个 ADC 时钟周期后自动启动新的 ADC2 转换；  
3.不需要设置CONT位。

#### 6）交替触发模式

此模式仅适用于注入通道组，设置ADC1_CTLR2中JEXTSEL[2:0]，以选择触发源。当第一次触发事件发生时，ADC1 上所有的注入通道被转换，当第二次触发事件发生时，ADC2 上所有的注入通道被转换。依次循环往复，若使能中断，则当 ADC1 所有注入通道转换完成后产生 JEOC 中断，ADC2 所有注入通道转换完成后产生JEOC中断。

![](images/22c4f49274a58d0ef194c3dc3641c7e3f5f0b3bd811c5d7e161c70ec34e79940.jpg)  
图 12-10 每个 ADC 注入通道组交替触发转换

若同时使用注入间断模式，则当第一次触发事件发生时，ADC1 上的第一个注入通道被转换，当第二次触发事件发生时，ADC2上第一个注入通道被转换。以此类推，往复循环。同时若使能中断，则当 ADC1所有注入通道转换完成后，产生 JEOC 中断，当 ADC2所有注入通道转换完成后产生 JEOC中断。

![](images/5c99619f1e7cbd67c9de92067cb492cc2df3fc8e77248a9f5cfbf157b2e9f2f2.jpg)  
图 12-11 间断模式下每个 ADC 注入通道交替触发转换

#### 7）同步规则模式+同步注入模式

此模式下规则组的同步转换可以打断，以启动注入组的同步转换。同时此模式下需要保准转换具有相同时间长度的序列，或保证触发时间间隔比两个序列中较长序列的时间长。

#### 8）同步规则模式+交替触发模式

此模式下规则组的同步转换可以被打断，以启动注入组的交替触发转换。当发生注入事件时，交替触发转换立即启动。如果同步规则正在转换，则所有 ADC的规则转换被停止，在注入转换结束后同步恢复。

图 12-12 同步规则模式下交替触发注入通道转换  
![](images/1ee6060673d8954169f401208b845e9245e1a50348e31a4ef2bd4f373d9bc99f.jpg)  
注：此模式下需要保准转换具有相同时间长度的序列，或者保证触发时间间隔比两个序列中较长序列的时间长。

如果注入触发事件发生在中断了规则转换的注入转换期间，则这个触发事件将会被忽略，例如下图描述的第 2 次触发的情况。

![](images/cb490bc4435ded2ff2e092a53d88dbc8d70f52f678fa4c197cf3893e26196a9e.jpg)  
图12-13 触发事件发生在注入转换期间

#### 9）同步注入模式+交替模式

此模式下，注入转换可以打断交替转换。发生注入事件时，交替转换被打断，注入转换启动，在注入转换结束后，交替转换被恢复。

![](images/0205b9ee7fc7a21ab9aa0d349783f34d89b44dc0a8bc0495b5c42be05c251167.jpg)  
图 12-14 交替转换下触发注入组转换

注：当ADC的预分频系数为4时，交替转换恢复后采样间隔不再是均匀的7个ADC时钟，改为8个和6个时钟周期交替。

## 12.3 寄存器描述

表 12-5 ADC1 相关寄存器列表  

<table><tr><td>名称</td><td>访问地址</td><td>描述</td><td>复位值</td></tr><tr><td>R32_ADC1_STAT</td><td>0x40012400</td><td>ADC1 状态寄存器</td><td>0x00000000</td></tr><tr><td>R32_ADC1_CTLR1</td><td>0x40012404</td><td>ADC1 控制寄存器 1</td><td>0x00000000</td></tr><tr><td>R32_ADC1_CTLR2</td><td>0x40012408</td><td>ADC1 控制寄存器 2</td><td>0x00000000</td></tr><tr><td>R32_ADC1_SAMPTR1</td><td>0x4001240C</td><td>ADC1 采样时间配置寄存器 1</td><td>0x00000000</td></tr><tr><td>R32_ADC1_SAMPTR2</td><td>0x40012410</td><td>ADC1 采样时间配置寄存器 2</td><td>0x00000000</td></tr><tr><td>R32_ADC1_IOFR1</td><td>0x40012414</td><td>ADC1 注入通道数据偏移寄存器 1</td><td>0x00000000</td></tr><tr><td>R32_ADC1_IOFR2</td><td>0x40012418</td><td>ADC1 注入通道数据偏移寄存器 2</td><td>0x00000000</td></tr><tr><td>R32_ADC1_IOFR3</td><td>0x4001241C</td><td>ADC1 注入通道数据偏移寄存器 3</td><td>0x00000000</td></tr><tr><td>R32_ADC1_IOFR4</td><td>0x40012420</td><td>ADC1 注入通道数据偏移寄存器 4</td><td>0x00000000</td></tr><tr><td>R32_ADC1_WDHTR</td><td>0x40012424</td><td>ADC1 看门狗高阈值寄存器</td><td>0x00000000</td></tr><tr><td>R32_ADC1_WDLTR</td><td>0x40012428</td><td>ADC1 看门狗低阈值寄存器</td><td>0x00000000</td></tr><tr><td>R32_ADC1_RSQR1</td><td>0x4001242C</td><td>ADC1 规则通道序列寄存器 1</td><td>0x00000000</td></tr><tr><td>R32_ADC1_RSQR2</td><td>0x40012430</td><td>ADC1 规则通道序列寄存器 2</td><td>0x00000000</td></tr><tr><td>R32_ADC1_RSQR3</td><td>0x40012434</td><td>ADC1 规则通道序列寄存器 3</td><td>0x00000000</td></tr><tr><td>R32_ADC1_ISQR</td><td>0x40012438</td><td>ADC1 注入通道序列寄存器</td><td>0x00000000</td></tr><tr><td>R32_ADC1_IDATAR1</td><td>0x4001243C</td><td>ADC1 注入数据寄存器 1</td><td>0x00000000</td></tr><tr><td>R32_ADC1_IDATAR2</td><td>0x40012440</td><td>ADC1 注入数据寄存器 2</td><td>0x00000000</td></tr><tr><td>R32_ADC1_IDATAR3</td><td>0x40012444</td><td>ADC1 注入数据寄存器 3</td><td>0x00000000</td></tr><tr><td>R32_ADC1_IDATAR4</td><td>0x40012448</td><td>ADC1 注入数据寄存器 4</td><td>0x00000000</td></tr><tr><td>R32_ADC1_RDATAR</td><td>0x4001244C</td><td>ADC1 规则数据寄存器</td><td>0x00000000</td></tr></table>

表 12-6 ADC2 相关寄存器列表  

<table><tr><td>名称</td><td>访问地址</td><td>描述</td><td>复位值</td></tr><tr><td>R32_ADC2_STAT</td><td>0x40012800</td><td>ADC2状态寄存器</td><td>0x00000000</td></tr><tr><td>R32_ADC2_CTLR1</td><td>0x40012804</td><td>ADC2控制寄存器1</td><td>0x00000000</td></tr><tr><td>R32_ADC2_CTLR2</td><td>0x40012808</td><td>ADC2控制寄存器2</td><td>0x00000000</td></tr><tr><td>R32_ADC2_SAMPTR1</td><td>0x4001280C</td><td>ADC2采样时间配置寄存器1</td><td>0x00000000</td></tr><tr><td>R32_ADC2_SAMPTR2</td><td>0x40012810</td><td>ADC2采样时间配置寄存器2</td><td>0x00000000</td></tr><tr><td>R32_ADC2_IOFR1</td><td>0x40012814</td><td>ADC2注入通道数据偏移寄存器1</td><td>0x00000000</td></tr><tr><td>R32_ADC2_IOFR2</td><td>0x40012818</td><td>ADC2注入通道数据偏移寄存器2</td><td>0x00000000</td></tr><tr><td>R32_ADC2_IOFR3</td><td>0x4001281C</td><td>ADC2注入通道数据偏移寄存器3</td><td>0x00000000</td></tr><tr><td>R32_ADC2_IOFR4</td><td>0x40012820</td><td>ADC2注入通道数据偏移寄存器4</td><td>0x00000000</td></tr><tr><td>R32_ADC2_WDHTR</td><td>0x40012824</td><td>ADC2看门狗高阈值寄存器</td><td>0x00000000</td></tr><tr><td>R32_ADC2_WDLTR</td><td>0x40012828</td><td>ADC2看门狗低阈值寄存器</td><td>0x00000000</td></tr><tr><td>R32_ADC2_RSQR1</td><td>0x4001282C</td><td>ADC2规则通道序列寄存器1</td><td>0x00000000</td></tr><tr><td>R32_ADC2_RSQR2</td><td>0x40012830</td><td>ADC2规则通道序列寄存器2</td><td>0x00000000</td></tr><tr><td>R32_ADC2_RSQR3</td><td>0x40012834</td><td>ADC2规则通道序列寄存器3</td><td>0x00000000</td></tr><tr><td>R32_ADC2_ISQR</td><td>0x40012838</td><td>ADC2注入通道序列寄存器</td><td>0x00000000</td></tr><tr><td>R32_ADC2_IDATAR1</td><td>0x4001283C</td><td>ADC2注入数据寄存器1</td><td>0x00000000</td></tr><tr><td>R32_ADC2_IDATAR2</td><td>0x40012840</td><td>ADC2注入数据寄存器2</td><td>0x00000000</td></tr><tr><td>R32_ADC2_IDATAR3</td><td>0x40012844</td><td>ADC2注入数据寄存器3</td><td>0x00000000</td></tr><tr><td>R32_ADC2_IDATAR4</td><td>0x40012848</td><td>ADC2注入数据寄存器4</td><td>0x00000000</td></tr><tr><td>R32_ADC2_RDATAR</td><td>0x4001284C</td><td>ADC2规则数据寄存器</td><td>0x00000000</td></tr></table>

### 12.3.1 ADCx 状态寄存器（ADCx_STATR）（x=1/2）

偏移地址： $0 \times 0 0$

<table><tr><td>31</td><td>30</td><td>29</td><td>28</td><td>27</td><td>26</td><td>25</td><td>24</td><td>23</td><td>22</td><td>21</td><td>20</td><td>19</td><td>18</td><td>17</td><td>16</td></tr><tr><td colspan="16">Reserved</td></tr><tr><td>15</td><td>14</td><td>13</td><td>12</td><td>11</td><td>10</td><td>9</td><td>8</td><td>7</td><td>6</td><td>5</td><td>4</td><td>3</td><td>2</td><td>1</td><td>0</td></tr><tr><td colspan="11">Reserved</td><td>STRT</td><td>JSTRT</td><td>JEOC</td><td>EOC</td><td>AWD</td></tr></table>

<table><tr><td>位</td><td>名称</td><td>访问</td><td>描述</td><td>复位值</td></tr><tr><td>[31:5]</td><td>Reserved</td><td>R0</td><td>保留。</td><td>0</td></tr><tr><td>4</td><td>STRT</td><td>RWO</td><td>规则通道转换开始状态:1:规则通道转换已开始;0:规则通道转换未开始。该位由硬件置1,由软件清0(写1无效)。</td><td>0</td></tr><tr><td>3</td><td>JSTRT</td><td>RWO</td><td>注入通道转换开始状态:</td><td>0</td></tr><tr><td></td><td></td><td></td><td>1: 注入通道转换已开始;0: 注入通道转换未开始。该位由硬件置1,由软件清0(写1无效)。</td><td></td></tr><tr><td>2</td><td>JEOC</td><td>RWO</td><td>注入通道组转换结束状态:1: 转换完成;0: 转换未完成。该位由硬件置1(所有注入通道转换完),由软件清0(写1无效)。</td><td>0</td></tr><tr><td>1</td><td>EOC</td><td>RWO</td><td>转换结束状态:1: 转换完成;0: 转换未完成。该位由硬件置1(规则或注入通道组转换结束),由软件清0(写1无效)或读ADC_RDATAR时清除。</td><td>0</td></tr><tr><td>0</td><td>AWD</td><td>RWO</td><td>模拟看门狗标志位:1: 发生模拟看门狗事件;0: 没有发生模拟看门狗事件。该位由硬件置1(转换值超出ADC_WDHTR和ADC_WDLTR寄存器范围),由软件清0(写1无效)。</td><td>0</td></tr></table>

### 12.3.2 ADCx 控制寄存器 1（ADCx_CTLR1）（x=1/2）

偏移地址： $0 \times 0 4$

<table><tr><td>Reserved</td><td colspan="2">PGA[1:0]</td><td>BUF
EN</td><td>TKI
TUNE</td><td>TKENABLE</td><td>AWDEN</td><td>JAWDEN</td><td colspan="2">Reserved</td><td colspan="2">DUALMOD[3:0]</td></tr><tr><td colspan="12">15 14 13 12 11 10 9 8 7 6 5 4 3 2 1 0</td></tr><tr><td>DISCNUM[2:0]</td><td>JDISC
EN</td><td>DISC
EN</td><td>JAUTO</td><td>AWD
SGL</td><td>SCAN</td><td>JEOC
IE</td><td>AWDIE</td><td>EOC
IE</td><td colspan="3">AWDCH[4:0]</td></tr></table>

<table><tr><td>位</td><td>名称</td><td>访问</td><td>描述</td><td>复位值</td></tr><tr><td>[31:29]</td><td>Reserved</td><td>R0</td><td>保留。</td><td>0</td></tr><tr><td>[28:27]</td><td>PGA[1:0]</td><td>RW</td><td>ADC通道增益配置00: x101: x410: x1611: x64注:输入增益可调,可实现小信号放大采样。使用此功能需开启ADC Buffer。</td><td>00b</td></tr><tr><td>26</td><td>BUFEN</td><td>RW</td><td>ADC BUFFER使能0: 关闭输入Buffer1: 使能输入Buffer注:需要开启buffer的情况:1. 当TKENABLE位或TSVREFE位置1, buffer默认开启不可关闭(故在这两种情况下,ADC校准需在TKENABLE位或TSVREFE位置1之前,且buffer需要关闭)。</td><td>0</td></tr><tr><td></td><td></td><td></td><td>2.当外部输入阻抗大于最大输入阻抗要求,可开启buffer以改善ADC采集数据(此时ADC的采样时间不建议小于7.5T),外部输入阻抗详见CH32F203DS0/CH32F208DS0/CH32V203DS0/CH32V208DS0数据手册表4-28 CH32V307DS0/CH32F207DS0数据手册表4-41。具体操作可参考EVT相关例程。</td><td></td></tr><tr><td>25</td><td>TKITUNE</td><td>RW</td><td>TKEY模块充电电流配置0:充电电流为35uA1:充电电流减半</td><td>0</td></tr><tr><td>24</td><td>TKENABLE</td><td>RW</td><td>TKEY模块使能控制,包括TKEY_F和TKEY_V单元:1:开启TKEY模块;0:关闭TKEY模块。</td><td>0</td></tr><tr><td>23</td><td>AWDEN</td><td>RW</td><td>在规则通道上模拟看门狗功能使能位:1:规则通道上使能模拟看门狗;0:规则通道上关闭模拟看门狗。</td><td>0</td></tr><tr><td>22</td><td>JAWDEN</td><td>RW</td><td>在注入通道上模拟看门狗功能使能位:1:注入通道上使能模拟看门狗;0:注入通道上关闭模拟看门狗。</td><td>0</td></tr><tr><td>[21:20]</td><td>Reserved</td><td>RO</td><td>保留。</td><td>0</td></tr><tr><td>[19:16]</td><td>DUALMOD[3:0]</td><td>RW</td><td>双重模式选择。0000:独立模式0001:同步规则+同步注入模式0010:同步规则+交替触发模式0011:同步注入+快速交替模式0100:同步注入+慢速交替模式0101:同步注入模式0110:同步规则模式0111:快速交替模式1000:慢速交替模式1001:交替触发模式注:ADC2中这些位为保留位,任何配置位的修改应在双重模式关闭的情况下进行。</td><td>0000b</td></tr><tr><td>[15:13]</td><td>DISCNUM[2:0]</td><td>RW</td><td>间断模式下,外部触发后要转换的规则通道数目:000:1个通道;...111:8个通道。</td><td>000b</td></tr><tr><td>12</td><td>JDISCEN</td><td>RW</td><td>注入通道上的间断模式使能位:1:使能注入通道上的间断模式;0:关闭注入通道上的间断模式。</td><td>0</td></tr><tr><td>11</td><td>DISCEN</td><td>RW</td><td>规则通道上的间断模式使能位:1:使能规则通道上的间断模式;0:关闭规则通道上的间断模式。</td><td>0</td></tr><tr><td>10</td><td>JAUTO</td><td>RW</td><td>开启规则通道完成后,自动转换注入通道组使能位:1:使能自动的注入通道组转换;0:关闭自动的注入通道组转换。注:此模式需要禁止注入通道的外部触发功能。</td><td>0</td></tr><tr><td>9</td><td>AWDSGL</td><td>RW</td><td>扫描模式下,在单一通道上使用模拟看门狗使能位:1:在单一通道上使用模拟看门狗(AWDCH[4:0]选择);0:在所有通道上使用模拟看门狗。</td><td>0</td></tr><tr><td>8</td><td>SCAN</td><td>RW</td><td>扫描模式使能位:1:使能扫描模式(连续转换ADC_IORx和ADC_RSQRx选择的所有通道);0:关闭扫描模式。</td><td>0</td></tr><tr><td>7</td><td>JEOCIE</td><td>RW</td><td>注入通道组转换结束中断使能位:1:使能注入通道组转换完成中断(JEOC标志);0:关闭注入通道组转换完成中断。</td><td>0</td></tr><tr><td>6</td><td>AWDIE</td><td>RW</td><td>模拟看门狗中断使能位:1:使能模拟看门狗中断;0:关闭模拟看门狗中断。注:在扫描模式下,如果发生此中断将中止扫描。</td><td>0</td></tr><tr><td>5</td><td>EOCIE</td><td>RW</td><td>转换结束(规则或注入通道组)中断使能位:1:使能转换结束中断(EOC标志);0:关闭转换结束中断。</td><td>0</td></tr><tr><td>[4:0]</td><td>AWDCH[4:0]</td><td>RW</td><td>模拟看门狗通道选择位:00000:模拟输入通道0;00001:模拟输入通道1;...10001:模拟输入通道17。</td><td>00000b</td></tr></table>

### 12.3.3 ADCx 控制寄存器 2（ADCx_CTLR2）（x=1/2）

偏移地址： $0 \times 0 8$

<table><tr><td>31</td><td>30</td><td>29</td><td>28</td><td>27</td><td>26</td><td>25</td><td>24</td><td>23</td><td>22</td><td>21</td><td>20</td><td>19</td><td>18</td><td>17</td><td>16</td></tr><tr><td colspan="8">Reserved</td><td>TS
VREFE</td><td>SW
START</td><td>JSW
START</td><td>EXT
TRIG</td><td colspan="3">EXTSEL[2:0]</td><td>Reser
ved</td></tr><tr><td>15</td><td>14</td><td>13</td><td>12</td><td>11</td><td>10</td><td>9</td><td>8</td><td>7</td><td>6</td><td>5</td><td>4</td><td>3</td><td>2</td><td>1</td><td>0</td></tr><tr><td>JEXT
TRIG</td><td colspan="2">JEXTSEL[2:0]</td><td>ALIGN</td><td>Reserved</td><td colspan="2">DMA</td><td colspan="5">Reserved</td><td>RST
CAL</td><td>CAL</td><td>CONT</td><td>ADON</td></tr></table>

<table><tr><td>位</td><td>名称</td><td>访问</td><td>描述</td><td>复位值</td></tr><tr><td>[31:24]</td><td>Reserved</td><td>RO</td><td>保留。</td><td>0</td></tr><tr><td>23</td><td>TSVREFE</td><td>RW</td><td>温度传感器和内部电压（VREFINT）通道使能位：1：使能温度传感器和VREFINT通道；0：禁止温度传感器和VREFINT通道。注：该位仅适用于ADC1。</td><td>0</td></tr><tr><td>22</td><td>SWSTART</td><td>RW</td><td>启动一个规则通道转换，需要设置软件触发：1：启动规则通道转换；0：复位状态。此位由软件置位，转换开始后硬件清0。</td><td>0</td></tr><tr><td>21</td><td>JSWSTART</td><td>RW</td><td>启动一个注入通道转换，需要设置软件触发：1：启动注入通道转换；</td><td>0</td></tr><tr><td></td><td></td><td></td><td>0:复位状态。此位由软件置位,转换开始后硬件清0或者软件清0。</td><td></td></tr><tr><td>20</td><td>EXTTRIG</td><td>RW</td><td>规则通道的外部触发转换模式使能:1:使用外部事件启动转换;0:关闭外部事件启动功能。</td><td>0</td></tr><tr><td>[19:17]</td><td>EXTSEL[2:0]</td><td>RW</td><td>启动规则通道转换的外部触发事件选择:000:定时器1的CC1事件;001:定时器1的CC2事件;010:定时器1的CC3事件;011:定时器2的CC2事件;100:定时器3的TRGO事件;101:定时器4的CC4事件;110:EXTI线11/定时器8的TRGO事件;111:SWSTART软件触发。注:仅大容量产品中具有定时器8的TRGO事件</td><td>000b</td></tr><tr><td>16</td><td>Reserved</td><td>RO</td><td>保留。</td><td>0</td></tr><tr><td>15</td><td>JEXTTRIG</td><td>RW</td><td>注入通道的外部触发转换模式使能:1:使用外部事件启动转换;0:关闭外部事件启动功能。</td><td>0</td></tr><tr><td>[14:12]</td><td>JEXTSEL[2:0]</td><td>RW</td><td>启动注入通道转换的外部触发事件选择:000:定时器1的TRGO事件;001:定时器1的CC4事件;010:定时器2的TRGO事件;011:定时器2的CC1事件;100:定时器3的CC4事件;101:定时器4的TRGO事件;110:EXTI线15/定时器8的CC4事件;111:JSWSTART软件触发。注:仅大容量产品中具有定时器8的CC4事件</td><td>000b</td></tr><tr><td>11</td><td>ALIGN</td><td>RW</td><td>数据对齐方式:1:左对齐;0:右对齐。</td><td>0</td></tr><tr><td>[10:9]</td><td>Reserved</td><td>RO</td><td>保留。</td><td>0</td></tr><tr><td>8</td><td>DMA</td><td>RW</td><td>直接存储访问(DMA)模式使能:1:使能DMA模式;0:关闭DMA模式。</td><td>0</td></tr><tr><td>[7:4]</td><td>Reserved</td><td>RO</td><td>保留。</td><td>0</td></tr><tr><td>3</td><td>RSTCAL</td><td>RW</td><td>复位校准,此位由软件置位,复位完成后由硬件清0:1:初始化校准寄存器;0:校准寄存器已初始化。注:如果正在进行转换时设置RSTCAL,清除校准寄存器需要额外的周期。</td><td>0</td></tr><tr><td>2</td><td>CAL</td><td>RW</td><td>A/D校准,该位由软件置位,校准结束时由硬件清0。1:开始校准;0:校准完成。</td><td>0</td></tr><tr><td>1</td><td>CONT</td><td>RW</td><td>连续转换使能:1:连续转换模式;</td><td>0</td></tr><tr><td></td><td></td><td></td><td>0:单次转换模式。
如果设置了此位,则转换将连续进行直到该位被清除。</td><td></td></tr><tr><td>0</td><td>ADON</td><td>RW</td><td>开/关A/D转换器
当该位为0时,写入1将把ADC从断电模式下唤醒;
当该位为1时,写入1将启动转换。
1:开启ADC并启动转换;
0:关闭ADC转换/校准,并进入断电模式。
注:当寄存器只有ADON改变时,才会启动一次转换,如果还有其他任意位发送变化,则不会启动新的转换。</td><td>0</td></tr></table>

### 12.3.4 ADCx 采样时间配置寄存器 1（ADCx_SAMPTR1）（x=1/2）

偏移地址： $0 \times 0 0$

<table><tr><td>31</td><td>30</td><td>29</td><td>28</td><td>27</td><td>26</td><td>25</td><td>24</td><td>23</td><td>22</td><td>21</td><td>20</td><td>19</td><td>18</td><td>17</td><td>16</td></tr><tr><td colspan="8">Reserved</td><td colspan="3">SMP17[2:0]</td><td colspan="3">SMP16[2:0]</td><td colspan="2">SMP15[2:1]</td></tr><tr><td>15</td><td>14</td><td>13</td><td>12</td><td>11</td><td>10</td><td>9</td><td>8</td><td>7</td><td>6</td><td>5</td><td>4</td><td>3</td><td>2</td><td>1</td><td>0</td></tr><tr><td>SMP15[0]</td><td colspan="3">SMP14[2:0]</td><td colspan="3">SMP13[2:0]</td><td colspan="3">SMP12[2:0]</td><td colspan="3">SMP11[2:0]</td><td colspan="3">SMP10[2:0]</td></tr></table>

<table><tr><td>位</td><td>名称</td><td>访问</td><td>描述</td><td>复位值</td></tr><tr><td>[31:24]</td><td>Reserved</td><td>R0</td><td>保留。</td><td>0</td></tr><tr><td>[23:0]</td><td>SMPx[2:0]</td><td>RW</td><td>SMPx[2:0]:通道x的采样时间配置:000:1.5周期; 001:7.5周期;010:13.5周期; 011:28.5周期;100:41.5周期; 101:55.5周期;110:71.5周期; 111:239.5周期;这些位用于独立地选择每个通道的采样时间,在采样周期中通道配置值必须保持不变。</td><td>000b</td></tr></table>

### 12.3.5 ADCx 采样时间配置寄存器 2（ADCx_SAMPTR2）（x=1/2）

偏移地址： $0 \times 1 0$

<table><tr><td>31</td><td>30</td><td>29</td><td>28</td><td>27</td><td>26</td><td>25</td><td>24</td><td>23</td><td>22</td><td>21</td><td>20</td><td>19</td><td>18</td><td>17</td><td>16</td></tr><tr><td colspan="2">Reserved</td><td colspan="3">SMP9[2:0]</td><td colspan="3">SMP8[2:0]</td><td colspan="3">SMP7[2:0]</td><td colspan="3">SMP6[2:0]</td><td colspan="2">SMP5[2:1]</td></tr><tr><td>15</td><td>14</td><td>13</td><td>12</td><td>11</td><td>10</td><td>9</td><td>8</td><td>7</td><td>6</td><td>5</td><td>4</td><td>3</td><td>2</td><td>1</td><td>0</td></tr><tr><td>SMP5[0]</td><td colspan="3">SMP4[2:0]</td><td colspan="3">SMP3[2:0]</td><td colspan="3">SMP2[2:0]</td><td colspan="3">SMP1[2:0]</td><td colspan="3">SMP0[2:0]</td></tr></table>

<table><tr><td>位</td><td>名称</td><td>访问</td><td>描述</td><td>复位值</td></tr><tr><td>[31:30]</td><td>Reserved</td><td>R0</td><td>保留。</td><td>0</td></tr><tr><td>[29:0]</td><td>SMPx[2:0]</td><td>RW</td><td>SMPx[2:0]:通道x的采样时间配置:000:1.5周期; 001:7.5周期;010:13.5周期; 011:28.5周期;100:41.5周期; 101:55.5周期;110:71.5周期; 111:239.5周期;这些位用于独立地选择每个通道的采样时间,在采样周期中通道配置值必须保持不变。</td><td>000b</td></tr></table>

### 12.3.6 ADCy 注入通道数据偏移寄存器 $\pmb { \times }$ （ADCy_IOFRx）(y=1/2；x=1/2/3/4)

偏移地址： $0 \times 1 4 + ( \times - 1 ) \times 4$

<table><tr><td>31</td><td>30</td><td>29</td><td>28</td><td>27</td><td>26</td><td>25</td><td>24</td><td>23</td><td>22</td><td>21</td><td>20</td><td>19</td><td>18</td><td>17</td><td>16</td></tr><tr><td colspan="16">Reserved</td></tr><tr><td>15</td><td>14</td><td>13</td><td>12</td><td>11</td><td>10</td><td>9</td><td>8</td><td>7</td><td>6</td><td>5</td><td>4</td><td>3</td><td>2</td><td>1</td><td>0</td></tr><tr><td colspan="4">Reserved</td><td colspan="12">J0FFSETx[11:0]</td></tr></table>

<table><tr><td>位</td><td>名称</td><td>访问</td><td>描述</td><td>复位值</td></tr><tr><td>[31:12]</td><td>Reserved</td><td>R0</td><td>保留。</td><td>0</td></tr><tr><td>[11:0]</td><td>J0FFSETx[11:0]</td><td>RW</td><td>注入通道x的数据偏移值。转换注入通道时，这个值定义了用于从原始转换数据中减去的数值。转换的结果可以在ADC_IDATARx寄存器中读出。</td><td>0</td></tr></table>

### 12.3.7 ADCx 看门狗高阈值寄存器（ADCx_WDHTR）（x=1/2）

偏移地址： ${ 0 } { \times 2 4 }$

<table><tr><td>31</td><td>30</td><td>29</td><td>28</td><td>27</td><td>26</td><td>25</td><td>24</td><td>23</td><td>22</td><td>21</td><td>20</td><td>19</td><td>18</td><td>17</td><td>16</td></tr><tr><td colspan="16">Reserved</td></tr><tr><td>15</td><td>14</td><td>13</td><td>12</td><td>11</td><td>10</td><td>9</td><td>8</td><td>7</td><td>6</td><td>5</td><td>4</td><td>3</td><td>2</td><td>1</td><td>0</td></tr><tr><td colspan="4">Reserved</td><td colspan="12">HT[11:0]</td></tr></table>

<table><tr><td>位</td><td>名称</td><td>访问</td><td>描述</td><td>复位值</td></tr><tr><td>[31:12]</td><td>Reserved</td><td>R0</td><td>保留。</td><td>0</td></tr><tr><td>[11:0]</td><td>HT[11:0]</td><td>RW</td><td>模拟看门狗高阈值设置值。</td><td>0</td></tr></table>

注：可以在转换过程中更改WDHTR和WDLTR的值，但它们将在下次转换时生效。

### 12.3.8 ADCx 看门狗低阈值寄存器（ADCx_WDLTR）（x=1/2）

偏移地址： $_ { 0 \times 2 8 }$

<table><tr><td>31</td><td>30</td><td>29</td><td>28</td><td>27</td><td>26</td><td>25</td><td>24</td><td>23</td><td>22</td><td>21</td><td>20</td><td>19</td><td>18</td><td>17</td><td>16</td></tr><tr><td colspan="16">Reserved</td></tr><tr><td>15</td><td>14</td><td>13</td><td>12</td><td>11</td><td>10</td><td>9</td><td>8</td><td>7</td><td>6</td><td>5</td><td>4</td><td>3</td><td>2</td><td>1</td><td>0</td></tr><tr><td colspan="4">Reserved</td><td colspan="12">LT[11:0]</td></tr></table>

<table><tr><td>位</td><td>名称</td><td>访问</td><td>描述</td><td>复位值</td></tr><tr><td>[31:12]</td><td>Reserved</td><td>R0</td><td>保留。</td><td>0</td></tr><tr><td>[11:0]</td><td>LT[11:0]</td><td>RW</td><td>模拟看门狗低阈值设置值。</td><td>0</td></tr></table>

注：可以在转换过程中更改WDHTR和WDLTR的值，但它们将在下次转换时生效。

### 12.3.9 ADCx 规则通道序列寄存器 1（ADCx_RSQR1）（x=1/2）

偏移地址： $\mathtt { 0 } \mathtt { x } 2 0$

<table><tr><td>31</td><td>30</td><td>29</td><td>28</td><td>27</td><td>26</td><td>25</td><td>24</td><td>23</td><td>22</td><td>21</td><td>20</td><td>19</td><td>18</td><td>17</td><td>16</td></tr><tr><td colspan="8">Reserved</td><td colspan="4">L[3:0]</td><td colspan="4">RSQ16[4:1]</td></tr><tr><td>15</td><td>14</td><td>13</td><td>12</td><td>11</td><td>10</td><td>9</td><td>8</td><td>7</td><td>6</td><td>5</td><td>4</td><td>3</td><td>2</td><td>1</td><td>0</td></tr><tr><td>SQ16[0]</td><td colspan="5">SQ15[4:0]</td><td colspan="5">SQ14[4:0]</td><td colspan="5">SQ13[4:0]</td></tr></table>

<table><tr><td>位</td><td>名称</td><td>访问</td><td>描述</td><td>复位值</td></tr><tr><td>[31:24]</td><td>Reserved</td><td>R0</td><td>保留。</td><td>0</td></tr><tr><td>[23:20]</td><td>L[3:0]</td><td>RW</td><td>规则通道转换序列中需要转换的通道数目:0000-1111:1-16个转换。</td><td>0</td></tr><tr><td>[19:15]</td><td>SQ16[4:0]</td><td>RW</td><td>规则序列中的第16个转换通道的编号(0-17)。</td><td>0</td></tr><tr><td>[14:10]</td><td>SQ15[4:0]</td><td>RW</td><td>规则序列中的第15个转换通道的编号(0-17)。</td><td>0</td></tr><tr><td>[9:5]</td><td>SQ14[4:0]</td><td>RW</td><td>规则序列中的第14个转换通道的编号(0-17)。</td><td>0</td></tr><tr><td>[4:0]</td><td>SQ13[4:0]</td><td>RW</td><td>规则序列中的第13个转换通道的编号(0-17)。</td><td>0</td></tr></table>

### 12.3.10 $\pmb { \mathsf { A D C x } }$ 规则通道序列寄存器 2（ADCx_RSQR2）（x=1/2）

偏移地址： $\mathtt { 0 } { \times } 3 0$

<table><tr><td>31</td><td>30</td><td>29</td><td>28</td><td>27</td><td>26</td><td>25</td><td>24</td><td>23</td><td>22</td><td>21</td><td>20</td><td>19</td><td>18</td><td>17</td><td>16</td></tr><tr><td colspan="2">Reserved</td><td colspan="5">SQ12[4:0]</td><td colspan="5">SQ11[4:0]</td><td colspan="4">SQ10[4:1]</td></tr><tr><td>15</td><td>14</td><td>13</td><td>12</td><td>11</td><td>10</td><td>9</td><td>8</td><td>7</td><td>6</td><td>5</td><td>4</td><td>3</td><td>2</td><td>1</td><td>0</td></tr><tr><td>SQ10[0]</td><td colspan="5">SQ9[4:0]</td><td colspan="5">SQ8[4:0]</td><td colspan="5">SQ7[4:0]</td></tr></table>

<table><tr><td>位</td><td>名称</td><td>访问</td><td>描述</td><td>复位值</td></tr><tr><td>[31:30]</td><td>Reserved</td><td>R0</td><td>保留。</td><td>0</td></tr><tr><td>[29:25]</td><td>SQ12[4:0]</td><td>RW</td><td>规则序列中的第12个转换通道的编号（0-17）。</td><td>0</td></tr><tr><td>[24:20]</td><td>SQ11[4:0]</td><td>RW</td><td>规则序列中的第11个转换通道的编号（0-17）。</td><td>0</td></tr><tr><td>[19:15]</td><td>SQ10[4:0]</td><td>RW</td><td>规则序列中的第10个转换通道的编号（0-17）。</td><td>0</td></tr><tr><td>[14:10]</td><td>SQ9[4:0]</td><td>RW</td><td>规则序列中的第9个转换通道的编号（0-17）。</td><td>0</td></tr><tr><td>[9:5]</td><td>SQ8[4:0]</td><td>RW</td><td>规则序列中的第8个转换通道的编号（0-17）。</td><td>0</td></tr><tr><td>[4:0]</td><td>SQ7[4:0]</td><td>RW</td><td>规则序列中的第7个转换通道的编号（0-17）。</td><td>0</td></tr></table>

### 12.3.11 ADCx 规则通道序列寄存器 3（ADCx_RSQR3）（x=1/2）

偏移地址： $0 \times 3 4$

<table><tr><td>31</td><td>30</td><td>29</td><td>28</td><td>27</td><td>26</td><td>25</td><td>24</td><td>23</td><td>22</td><td>21</td><td>20</td><td>19</td><td>18</td><td>17</td><td>16</td></tr><tr><td colspan="2">Reserved</td><td colspan="5">SQ6[4:0]</td><td colspan="5">SQ5[4:0]</td><td colspan="4">SQ4[4:1]</td></tr><tr><td>15</td><td>14</td><td>13</td><td>12</td><td>11</td><td>10</td><td>9</td><td>8</td><td>7</td><td>6</td><td>5</td><td>4</td><td>3</td><td>2</td><td>1</td><td>0</td></tr><tr><td>SQ4[0]</td><td colspan="5">SQ3[4:0]</td><td colspan="5">SQ2[4:0]</td><td colspan="5">SQ1[4:0]</td></tr></table>

<table><tr><td>位</td><td>名称</td><td>访问</td><td>描述</td><td>复位值</td></tr><tr><td>[31:30]</td><td>Reserved</td><td>R0</td><td>保留。</td><td>0</td></tr><tr><td>[29:25]</td><td>SQ6[4:0]</td><td>RW</td><td>规则序列中的第6个转换通道的编号（0-17）。</td><td>0</td></tr><tr><td>[24:20]</td><td>SQ5[4:0]</td><td>RW</td><td>规则序列中的第5个转换通道的编号（0-17）。</td><td>0</td></tr><tr><td>[19:15]</td><td>SQ4[4:0]</td><td>RW</td><td>规则序列中的第4个转换通道的编号（0-17）。</td><td>0</td></tr><tr><td>[14:10]</td><td>SQ3[4:0]</td><td>RW</td><td>规则序列中的第3个转换通道的编号（0-17）。</td><td>0</td></tr><tr><td>[9:5]</td><td>SQ2[4:0]</td><td>RW</td><td>规则序列中的第2个转换通道的编号（0-17）。</td><td>0</td></tr><tr><td>[4:0]</td><td>SQ1[4:0]</td><td>RW</td><td>规则序列中的第1个转换通道的编号（0-17）。</td><td>0</td></tr></table>

### 12.3.12 ADCx 注入通道序列寄存器（ADCx_ISQR）（x=1/2）

偏移地址： ${ 0 } { \times } \ 3 { 8 }$

<table><tr><td>31</td><td>30</td><td>29</td><td>28</td><td>27</td><td>26</td><td>25</td><td>24</td><td>23</td><td>22</td><td>21</td><td>20</td><td>19</td><td>18</td><td>17</td><td>16</td></tr><tr><td colspan="10">Reserved</td><td colspan="2">JL[1:0]</td><td colspan="4">JSQ4[4:1]</td></tr><tr><td>15</td><td>14</td><td>13</td><td>12</td><td>11</td><td>10</td><td>9</td><td>8</td><td>7</td><td>6</td><td>5</td><td>4</td><td>3</td><td>2</td><td>1</td><td>0</td></tr><tr><td>JSQ4[0]</td><td colspan="5">JSQ3[4:0]</td><td colspan="5">JSQ2[4:0]</td><td colspan="5">JSQ1[4:0]</td></tr></table>

<table><tr><td>位</td><td>名称</td><td>访问</td><td>描述</td><td>复位值</td></tr><tr><td>[31:22]</td><td>Reserved</td><td>R0</td><td>保留。</td><td>0</td></tr><tr><td>[21:20]</td><td>JL[1:0]</td><td>RW</td><td>注入通道转换序列中需要转换的通道数目:00-11:1-4个转换。</td><td>0</td></tr><tr><td>[19:15]</td><td>JSQ4[4:0]</td><td>RW</td><td>注入序列中的第4个转换通道的编号(0-17)。</td><td>0</td></tr><tr><td>[14:10]</td><td>JSQ3[4:0]</td><td>RW</td><td>注入序列中的第3个转换通道的编号(0-17)。</td><td>0</td></tr><tr><td>[9:5]</td><td>JSQ2[4:0]</td><td>RW</td><td>注入序列中的第2个转换通道的编号(0-17)。</td><td>0</td></tr><tr><td>[4:0]</td><td>JSQ1[4:0]</td><td>RW</td><td>注入序列中的第1个转换通道的编号(0-17)。</td><td>0</td></tr></table>

注：不同于规则转换序列，如果ILEN[1:0]的长度小于4，则转换的序列顺序是从(4-ILEN)开始。

### 12.3.13 ADCy 注入数据寄存器 $\pmb { \times }$ （ADCy_IDATARx）（y=1/2；x=1/2/3/4）

偏移地址： $0 \times 3 0 + ( x - 1 ) \times 4$

<table><tr><td>31</td><td>30</td><td>29</td><td>28</td><td>27</td><td>26</td><td>25</td><td>24</td><td>23</td><td>22</td><td>21</td><td>20</td><td>19</td><td>18</td><td>17</td><td>16</td></tr><tr><td colspan="16">Reserved</td></tr><tr><td>15</td><td>14</td><td>13</td><td>12</td><td>11</td><td>10</td><td>9</td><td>8</td><td>7</td><td>6</td><td>5</td><td>4</td><td>3</td><td>2</td><td>1</td><td>0</td></tr><tr><td colspan="16">JDATA[15:0]</td></tr></table>

<table><tr><td>位</td><td>名称</td><td>访问</td><td>描述</td><td>复位值</td></tr><tr><td>[31:16]</td><td>Reserved</td><td>R0</td><td>保留。</td><td>0</td></tr><tr><td>[15:0]</td><td>JDATA[15:0]</td><td>R0</td><td>注入通道转换数据（数据左对齐或右对齐）。</td><td>0</td></tr></table>

### 12.3.14 $\pmb { \mathsf { A D C x } }$ 规则数据寄存器（ADCx_RDATAR）（x=1/2）

偏移地址： $\mathtt { 0 } \mathtt { x } 4 0$

<table><tr><td>31</td><td>30</td><td>29</td><td>28</td><td>27</td><td>26</td><td>25</td><td>24</td><td>23</td><td>22</td><td>21</td><td>20</td><td>19</td><td>18</td><td>17</td><td>16</td></tr><tr><td colspan="16">ADC2DATA[15:0]</td></tr><tr><td>15</td><td>14</td><td>13</td><td>12</td><td>11</td><td>10</td><td>9</td><td>8</td><td>7</td><td>6</td><td>5</td><td>4</td><td>3</td><td>2</td><td>1</td><td>0</td></tr><tr><td colspan="16">DATA[4:0]</td></tr></table>

<table><tr><td>位</td><td>名称</td><td>访问</td><td>描述</td><td>复位值</td></tr><tr><td>[31:16]</td><td>ADC2DATA[15:0]</td><td>R0</td><td>ADC2转换的数据:在ADC1中:双模式下,这些位包含了ADC2转换的规则通道数据。在ADC2中:不使用这些位。</td><td>0</td></tr><tr><td>[15:0]</td><td>DATA[4:0]</td><td>R0</td><td>规则通道转换数据(数据左对齐或右对齐)。</td><td>0</td></tr></table>


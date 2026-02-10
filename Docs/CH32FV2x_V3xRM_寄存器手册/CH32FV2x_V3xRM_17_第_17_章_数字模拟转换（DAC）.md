# 第 17 章 数字/模拟转换（DAC）

本章模块描述适用于CH32F2x、CH32V2x和 $_ { C H 3 2 V 3 x }$ 微控制器部分产品。

数字/模拟转换模块（DAC），包含2个可配置8/12 位数字输入转换 2路模拟电压输出的转换器。内置三角波、噪声波形发生器，支持多种事件触发转换，DMA 功能等。

## 17.1 主要特性

2 个 DAC 转换器，每个转换器对应 1 个输出通道  
$\bullet$ 三角波、噪声波形发生器  
$\bullet$ 可配置 8 位或 12 位输出  
12位数据左对齐或右对齐  
$\bullet$ 双 DAC 同时或分别转换  
$\bullet$ 支持 DMA功能  
多种触发事件

## 17.2 功能描述

### 17.2.1 DAC 模块结构

![](images/6e507ff55b60c035f614d898cd7d549e40c748843456bf3525b73db661d38147.jpg)  
图 17-1 DAC 模块框图

### 17.2.2 DAC 通道配置

#### 17.2.2.1 开启 DAC 功能：

将DAC_CTLR寄存器的ENx位置1，即可打开对DAC通道 x的模拟供电。经过一段启动时间，DAC 通道 $\mathsf { x }$ 即被使能。DAC 包含 2 个模拟输出通道，可同时或分别独立输出。

注：为了避免寄生的干扰和额外的功耗，DAC通道对应的引脚需提前设置成模拟输入(AIN)模式。

#### 17.2.2.2 打开输出缓冲：

DAC 集成了输出缓冲，可以用来减少输出阻抗，增加驱动能力直接驱动外部负载。每个 DAC通道输出缓存可以通过设置DAC_CTLR寄存器的 BOFFx位来使能或关闭。

#### 17.2.2.3 数据格式：

单DAC通道模式下，包括8位数据右对齐、12位数据左对齐、12位数据右对齐。写入数据到DAC_R8BDHRx[7:0]，模块将加载（1个APB1时钟周期后）其左移数据到数据输出寄存器DAC_DORx[11:0]，写入数据到 DAC_R12BDHRx[11:0]，模块将加载（1 个 APB1 时钟周期后）右对齐数

据到数据输出寄存器 DAC_DORx[11:0]；写入数据到 DAC_L12BDHRx[15:4]，模块经过相应的移位后，将加载（1个APB1时钟周期后）左对齐数据到数据输出寄存器 DAC_DORx[11:0]。双 DAC通道模式下，同样包括8位数据右对齐、12数据左对齐、12位数据右对齐三种方式。写入数据到DAC_RD8BDHR[7:0]，模块将加载（1 个 APB1 时钟周期后）位[7:0]移位后到 DAC_DOR1[11:0]，位[15:8]移位后到 DAC_DOR2[11:0]；写入数据到 DAC_LD12BDHR[31:0]，模块将加载（1 个 APB 时钟周期后）位[15:4]数据移位后到 DAC_DOR1[11:4]，位[31:20]数据移位后到 DAC_DOR2[11:4]；写入数据到 DAC_RD12BDHR[31:0]，模块将加载（1 个 APB 时钟周期后）位[11:0]数据到 DAC_DOR1[11:0]，位[27:16]数据到 DAC_DOR2[11:0]。

![](images/aa307d0c4dac9361bcf0580712d804f9c95ef61f64559062dee6f7296eabca77.jpg)  
图17-2 单通道数据格式

![](images/1c7e5f6504512555f7a040e66be34bb7431aeec73eb450756b83378ac2d3e8f3.jpg)  
图17-3 双通道数据格式

#### 17.2.2.4 DMA 功能：

DAC 通道具有DMA功能。设置DAC_CTLR寄存器的DMAENx位为1，开启对应通道的 DMA功能。当有触发事件(不包括软件触发)发生，则产生一个 DMA请求，然后 DAC_DORx寄存器的数据将被更新。

#### 17.2.2.5 触发事件选择：

DAC 转换可以由以下事件触发进行转换。当配置 DAC_CTLR寄存器的 TENx 位为 1，配置TSELx[2:0]控制位选择某个触发事件触发 DAC转换。

表 17-1 触发事件  

<table><tr><td>触发源</td><td>类型</td><td>TSELx[2:0]</td></tr><tr><td>定时器6 TRGO事件</td><td rowspan="6">来自片上定时器内部信号</td><td>000</td></tr><tr><td>定时器8 TRGO事件</td><td>001</td></tr><tr><td>定时器7 TRGO事件</td><td>010</td></tr><tr><td>定时器5 TRGO事件</td><td>011</td></tr><tr><td>定时器2 TRGO事件</td><td>100</td></tr><tr><td>定时器4 TRGO事件</td><td>101</td></tr><tr><td>EXTI 线路9</td><td>外部引脚</td><td>110</td></tr><tr><td>SWTRIG(软件触发)</td><td>软件控制位</td><td>111</td></tr></table>

DAC 接口会监测到来自选中的定时器 TRGO 输出或外部中断线 9 的上升沿，在触发后的 3 个 APB1时钟周期后，将寄存器DAC_DORx更新为新值。

如果配置的是软件触发方式，SWTRIG位一旦置 1，将会启动一次转换，在触发后的 1个 APB1 时钟周期后，将寄存器DAC_DORx更新为新值，并且硬件对 SWTRIG 位自动清 0。

注：不能在ENx为1时改变 $T S E L x [ 2 ; 0 ]$ 位。

### 17.2.3 DAC 转换

DAC 通道的数据来自DAC_DORx寄存器，但不能直接对寄存器 DAC_DORx写入数据，任何输出到DAC 通道 $\mathsf { x }$ 的数据都必须写入 DAC_R12BDHR1、DAC_L12BDHR1、DAC_R12BDHR2、DAC_L12BDHR2、DAC_RD12BDHR、DAC_LD12BDHR、DAC_RD8BDHR 寄存器中。由系统内部的保持寄存器 DAC_DHRx 会获取上述寄存器值将其经过相应时间送入DAC_DORx寄存器。

非触发方式下，写入寄存器 DAC_xDHRx 的数据会在 1 个 APB1 时钟周期后移入 DAC_DORx 寄存器。

软件触发下，事件触发上升沿后 1 个 APB1 时钟周期后自动更新 DAC_DORx 寄存器。

硬件触发（定时器 TRGO 事件或者外部中断线 9 上升沿）下，触发事件后 3 个 APB1 时钟周期后自动更新 DAC_DORx 寄存器。

装入 DAC_DORx 寄存器数据，在经过时间 tSETTLING之后，输出即有效，这段时间的长短依电源电压和模拟输出负载的不同会有所变化。

数字输入经过 DAC 被线性地转换为模拟电压输出，其范围为 0 到 $V _ { \mathsf { D D A } }$ 。任一 DAC 通道引脚上的输出电压满足下面的关系：

DAC 输出电压 $= \mathsf { V } _ { \mathsf { D D A } } \ast$ (DAC_DORx/ 4096)。

### 17.2.4 DAC 三角波生成器

模块内置了一个三角波生成器，可以在基准信号上加上一个小幅度的三角波。设置 WAVEx[1:0]位为‘10’选择DAC的三角波生成功能。设置 DAC_CTLR寄存器的 MAMPx[3:0]位来选择三角波的幅度。

系统内部包含一个从0开始的三角波计数器，在每次触发事件后 3个 APB1时钟周期后累加 1。计数器的值与DAC_DHRx寄存器的数值相加并丢弃溢出位后写入 DAC_DORx 寄存器。在传入 DAC_DORx寄存器的数值小于 $\mathsf { M A M P } \times \left[ 3 : 0 \right]$ 位定义的最大幅度时，三角波计数器逐步累加，一旦达到设置的最大幅度，则计数器开始递减，达到0后再开始累加，周而复始。将 WAVEx[1:0]位置‘00’，可以复位三角波的生成。

注：1.为了产生三角波，必须使能 DAC 触发，即设 DAC_CTLR寄存器的 TENx位为1。

2.MAMPx[3:0]位必须在使能DAC之前设置，否则其值不能修改。

![](images/f2311923ead2271032471b3a6b0602e5b005fde06fa6eb07a55d4662f716eea6.jpg)  
图 17-4 三角波生成

### 17.2.5 DAC 噪声生成器

模块内置了一个噪声生成器，是利用线性反馈移位寄存器(Linear Feedback Shift RegisterLFSR)产生幅度变化的伪噪声。设置WAVE[1:0]位为‘01’选择 DAC噪声生成功能。设置 DAC_CTLR寄存器的MAMPx[3:0]位来选择屏蔽部分LFSR的数据。

寄存器LFSR的预装入值为0xAAA。按照特定算法，在每次触发事件后 3个APB1时钟周期之后更新该寄存器的值。设置DAC_CR寄存器的 MAMPx[3:0]位可以屏蔽部分或者全部 LFSR 的数据，这样的得到的 LSFR 值与 DAC_DHRx 的数值相加，去掉溢出位之后即被写入 DAC_DORx 寄存器。如果寄存器LFSR 值为 $0 \times 0 0 0$ ，则会注入‘1’(防锁定机制)。将 WAVEx[1:0]位置‘00’，可以复位 LFSR 波形的生成算法。

注：为了产生噪声，必须使能 DAC 触发，即设DAC_CTLR寄存器的 TENx位为1。

![](images/19cf372b06640b7c498a7197be77ed41d85ab1f729f3534a5e303ed2b7ed78b1.jpg)  
图 17-5 LFSR 寄存器算法

## 17.3 双 DAC 转换

当需要2个DAC同时转换的情况下，为了更加便捷和高效的操作 DAC模块，模块集成 3个双DAC 模式下的数据寄存器 DAC_RD8BDHR、DAC_LD12BDHR、DAC_RD12BDHR。只需操作其中之一寄存器即可更新 2 个 DAC 的转换值。

针对双DAC的转换可配合模块的其他寄存器，可实现 11种不同组合的转换模式，两个通道待转换值需写入上述 3 个双通道数据寄存器之一。

### 17.3.1 不同触发下使用相同 LFSR

设置 TENx 置位，TSELx 为不同值，WAVEx 为 0b01，MAMPx 为相同的 LFSR 屏蔽值。当通道 1 触发事件发生时，将通道1数据寄存器DAC_DHR1值加上带相同屏蔽的 LFSR1计数值，延迟 3个 APB1时钟后送给DAC_DOR1，用于转换，并更新LFSR1；当通道 2触发事件发生时，将通道 2 数据寄存器DAC_DHR2值加上带相同屏蔽的LFSR2计数值，延迟3 个 APB1时钟后送给 DAC_DOR2，用于转换，并更新 LFSR2。

### 17.3.2 不同触发下使用不同 LFSR

设置 TENx 置位，TSELx 为不同值，WAVEx 为 0b01，MAMPx 为不同的 LFSR 屏蔽值。当通道 1 触发事件发生时，将通道1数据寄存器DAC_DHR1值加上按 MAMP1[3:0］所设屏蔽的 LFSR1 计数值，延迟3个APB1时钟后送给DAC_DOR1，用于转换，并更新LFSR1；当通道2 触发事件发生时，将通道 2 数据寄存器 DAC_DHR2 值加上按 MAMP2[3:0］所设屏蔽的 LFSR2 计数值，延迟 3 个 APB1 时钟后送给DAC_DOR2，用于转换，并更新 LFSR2。

### 17.3.3 不同触发下产生相同三角波

设置 TENx 置位，TSELx 为不同值，WAVEx 为 0b1x， ${ \mathsf { M A M P x } }$ 设为相同三角波幅值。当通道 1触发事件发生时，将通道 1 数据寄存器 DAC_DHR1 值加上 MAMPx 所设的相同幅值的三角波计数器值，延迟3个APB1时钟后送给DAC_DOR1，用于转换，并更新通道 1三角波计数器；当通道 2触发事件发生时，将通道2数据寄存器DAC_DHR2值加上 MAMPx所设的相同幅值的三角波计数器值，延迟 3个APB1 时钟后送给 DAC_DOR2，用于转换，并更新通道 2 三角波计数器。

### 17.3.4 不同触发下产生不同三角波

设置 TENx 置位，TSELx 为不同值，WAVEx 为 0b1x，MAMPx 设为不同的三角波幅值。当通道 1 触发事件发生时，将通道 1 数据寄存器 DAC_DHR1 值加上 MAMP1 所设幅值的三角波计数器值，延迟 3 个APB1时钟后送给DAC_DOR1，用于转换，并更新通道1 三角波计数器；当通道2 触发事件发生时，将通道2数据寄存器DAC_DHR2值加上MAMP2所设幅值的三角波计数器值，延迟3 个 APB1时钟后送给DAC_DOR2，用于转换，并更新通道 2 三角波计数器。

### 17.3.5 不同触发下不使用波形发生器

设置TENx置位，TSELx为不同值选择不同触发源。当通道 1 触发事件发生时，将通道 1数据寄

存器DAC_DHR1值延迟3个APB1时钟后送给DAC_DOR1，用于转换；当通道2触发事件发生时，将通道 2 数据寄存器 DAC_DHR2 值延迟 3 个 APB1 时钟后送给 DAC_DOR2，用于转换。

### 17.3.6 均使用软件触发

在此配置下，双通道数据寄存器写入需要转换值，1 个 APB1时钟周期后，DAC_DHR1和DAC_DHR2 的数据被分别送到 DAC_DOR1 和 DAC_DOR2 用于转换。

### 17.3.7 相同触发下使用相同 LFSR

设置 TENx 置位，TSELx 为相同值，WAVEx 为 0b01，MAMPx 为相同的 LFSR 屏蔽值。当触发事件发生后，寄存器 DAC_DHR1 的值加上带相同屏蔽的 LFSR1 计数值，延迟 3 个 APB1 时钟后送给DAC_DOR1，用于转换，并更新LFSR1，同时寄存器DAC_DHR2的值加上带相同屏蔽的 LFSR2 计数值，延迟 3 个 APB1 时钟后送给 DAC_DOR2，用于转换，并更新 LFSR2。

### 17.3.8 相同触发下使用不同 LFSR

设置 TENx 置位，TSELx 为相同值，WAVEx 为 0b01，MAMPx 为不同的 LFSR 屏蔽值。当触发事件发生后，寄存器DAC_DHR1的值加上带不同屏蔽值的 LFSR1计数值，延迟 3个 APB1时钟后送给DAC_DOR1，用于转换，并更新 LFSR1，同时寄存器 DAC_DHR2 的值加上带不同屏蔽值的 LFSR2 计数值，延迟 3 个 APB1 时钟后送给 DAC_DOR2，用于转换，并更新 LFSR2。

### 17.3.9 相同触发下产生相同三角波

设置 TENx 置位，TSELx 为相同值，WAVEx 为 0b1x，MAMPx 为相同的三角波幅值。当触发事件发生后，寄存器DAC_DHR1的值加上相同三角波幅值的计数器值，延迟 3个 APB1时钟后送给DAC_DOR1，用于转换，并更新通道1三角波计数器，同时寄存器 DAC_DHR2的值加上相同三角波幅值的计数器值，延迟 3 个 APB1 时钟后送给 DAC_DOR2，用于转换，并更新通道 2 三角波计数器值。

### 17.3.10 相同触发下产生不同三角波

设置TENx置位，TSELx为相同值，WAVEx为 0b1x，MAMPx为不同的三角波幅值。当触发事件发生后，寄存器DAC_DHR1的值加上MAMP1[3:0]所设的三角波幅值计数器值，延迟 3个 APB1 时钟后送给DAC_DOR1，用于转换，并更新通道1三角波计数器，同时寄存器 DAC_DHR2的值加上 MAMP2[3:0]所设的三角波幅值计数器值，延迟 3 个 APB1 时钟后送给 DAC_DOR2，用于转换，并更新通道 2 三角波计数器值。

### 17.3.11 相同触发下不使用波形发生器

设置 TENx 置位，TSELx 为相同值。该配置下，当触发事件发生后，寄存器 DAC_DHR1 和DAC_DHR2 的值在延迟 3 个 APB1 时钟后分别送给 DAC_DOR1 和 DAC_DOR2 用于 DAC 转换。

## 17.4 寄存器描述

表 17-2 DAC 相关寄存器列表  

<table><tr><td>名称</td><td>访问地址</td><td>描述</td><td>复位值</td></tr><tr><td>R32_DAC_CTLR</td><td>0x40007400</td><td>DAC配置寄存器</td><td>0x00000000</td></tr><tr><td>R32_DAC_SWTR</td><td>0x40007404</td><td>DAC软件触发寄存器</td><td>0x00000000</td></tr><tr><td>R32_DAC_R12BDHR1</td><td>0x40007408</td><td>DAC通道1右对齐12位数据保存寄存器</td><td>0x00000000</td></tr><tr><td>R32_DAC_L12BDHR1</td><td>0x4000740C</td><td>DAC通道1左对齐12位数据保存寄存器</td><td>0x00000000</td></tr><tr><td>R32_DAC_R8BDHR1</td><td>0x40007410</td><td>DAC通道1右对齐8位数据保存寄存器</td><td>0x00000000</td></tr><tr><td>R32_DAC_R12BDHR2</td><td>0x40007414</td><td>DAC通道2右对齐12位数据保存寄存器</td><td>0x00000000</td></tr><tr><td>R32_DAC_L12BDHR2</td><td>0x40007418</td><td>DAC通道2左对齐12位数据保存寄存器</td><td>0x00000000</td></tr><tr><td>R32_DAC_R8BDHR2</td><td>0x4000741C</td><td>DAC通道2右对齐8位数据保存寄存器</td><td>0x00000000</td></tr><tr><td>R32_DAC_RD12BDHR</td><td>0x40007420</td><td>双通道右对齐12位数据保存寄存器</td><td>0x00000000</td></tr><tr><td>R32_DAC_LD12BDHR</td><td>0x40007424</td><td>双通道左对齐12位数据保存寄存器</td><td>0x00000000</td></tr><tr><td>R32_DAC_RD8BDHR</td><td>0x40007428</td><td>双通道右对齐8位数据保存寄存器</td><td>0x00000000</td></tr><tr><td>R32_DAC_DOR1</td><td>0x4000742C</td><td>DAC通道1数据输出寄存器</td><td>0x00000000</td></tr><tr><td>R32_DAC_DOR2</td><td>0x40007430</td><td>DAC通道2数据输出寄存器</td><td>0x00000000</td></tr></table>

### 17.4.1 DAC 配置寄存器（DAC_CTLR）

偏移地址： $0 \times 0 0$

<table><tr><td>31</td><td>30</td><td>29</td><td>28</td><td>27</td><td>26</td><td>25</td><td>24</td><td>23</td><td>22</td><td>21</td><td>20</td><td>19</td><td>18</td><td>17</td><td>16</td></tr><tr><td>Reserved</td><td colspan="2">DMAEN2</td><td colspan="5">MAMP2[3:0]</td><td colspan="2">WAVE2[2:0]</td><td colspan="3">TSEL2[2:0]</td><td>TEN2</td><td>BOFF2</td><td>EN2</td></tr><tr><td>15</td><td>14</td><td>13</td><td>12</td><td>11</td><td>10</td><td>9</td><td>8</td><td>7</td><td>6</td><td>5</td><td>4</td><td>3</td><td>2</td><td>1</td><td>0</td></tr><tr><td>Reserved</td><td colspan="2">DMAEN1</td><td colspan="5">MAMP1[3:0]</td><td colspan="2">WAVE1[2:0]</td><td colspan="3">TSEL1[2:0]</td><td>TEN1</td><td>BOFF1</td><td>EN1</td></tr></table>

<table><tr><td>位</td><td>名称</td><td>访问</td><td>描述</td><td>复位值</td></tr><tr><td>[31:29]</td><td>Reserved</td><td>R0</td><td>保留。</td><td>0</td></tr><tr><td>28</td><td>DMAEN2</td><td>RW</td><td>DAC通道2的DMA使能:1:使能DAC通道2DMA功能;0:关闭DAC通道2DMA功能。</td><td>0</td></tr><tr><td>[27:24]</td><td>MAMP2[3:0]</td><td>RW</td><td>DAC通道2屏蔽/幅值设置,软件设置这个区域用来在噪声生成模式下选择LFSR数据屏蔽位,在三角波形生成模式下选择波形的幅值:0000:不屏蔽LFSR位0/三角波幅值为1;0001:不屏蔽LFSR位[1:0]/三角波幅值为3;0010:不屏蔽LFSR位[2:0]/三角波幅值为7;0011:不屏蔽LFSR位[3:0]/三角波幅值为15;0100:不屏蔽LFSR位[4:0]/三角波幅值为31;0101:不屏蔽LFSR位[5:0]/三角波幅值为63;0110:不屏蔽LFSR位[6:0]/三角波幅值为127;0111:不屏蔽LFSR位[7:0]/三角波幅值为255;1000:不屏蔽LFSR位[8:0]/三角波幅值为511;1001:不屏蔽LFSR位[9:0]/三角波幅值为1023;1010:不屏蔽LFSR位[10:0]/三角波幅值为2047;≥1011:不屏蔽LFSR位[11:0]/三角波幅值为4095。</td><td>0000b</td></tr><tr><td>[23:22]</td><td>WAVE2[1:0]</td><td>RW</td><td>DAC通道2的噪声/三角波生成使能00:关闭波形发生器;01:使能噪声波形发生器;1x:使能三角波波形发生器。</td><td>00b</td></tr><tr><td>[21:19]</td><td>TSEL2[2:0]</td><td>RW</td><td>DAC通道2触发事件选择设置:</td><td>000b</td></tr><tr><td></td><td></td><td></td><td>000: TIM6 TRGO事件;001: TIM8 TRGO事件;010: TIM7 TRGO事件011: TIM5 TRGO事件100: TIM2 TRGO事件;101: TIM4 TRGO事件;110: 外部中断线9;111: 软件触发;其他:保留。</td><td></td></tr><tr><td>18</td><td>TEN2</td><td>RW</td><td>DAC通道2外部触发模式使能:1:使能DAC通道2触发功能,写入DAC_xDHR寄存器的数据在3个APB1时钟周期后送入DAC_DOR2寄存器。0:关闭DAC通道2触发功能,写入DAC_xDHR寄存器的数据在1个APB1时钟周期后送入DAC_DOR2寄存器。注:如果选择软件触发,DAC_xDHR中的数据只需1个APB1时钟周期后送入DAC_DOR2寄存器。</td><td>0</td></tr><tr><td>17</td><td>BOFF2</td><td>RW</td><td>DAC通道2输出缓冲关闭控制(建议打开):1:关闭DAC通道2输出缓存;0:打开DAC通道2输出缓存。</td><td>0</td></tr><tr><td>16</td><td>EN2</td><td>RW</td><td>DAC通道2使能:1:使能DAC通道2;0:关闭DAC通道2。</td><td>0</td></tr><tr><td>[15:13]</td><td>Reserved</td><td>RO</td><td>保留。</td><td>0</td></tr><tr><td>12</td><td>DMAEN1</td><td>RW</td><td>DAC通道1的DMA使能:1:使能DAC通道1DMA功能;0:关闭DAC通道1DMA功能。</td><td>0</td></tr><tr><td>[11:8]</td><td>MAMP1[3:0]</td><td>RW</td><td>DAC通道1屏蔽/幅值设置,软件设置这个区域用来在噪声生成模式下选择LFSR数据屏蔽位,在三角波形生成模式下选择波形的幅值:0000:不屏蔽LFSR位0/三角波幅值为1;0001:不屏蔽LFSR位[1:0]/三角波幅值为3;0010:不屏蔽LFSR位[2:0]/三角波幅值为7;0011:不屏蔽LFSR位[3:0]/三角波幅值为15;0100:不屏蔽LFSR位[4:0]/三角波幅值为31;0101:不屏蔽LFSR位[5:0]/三角波幅值为63;0110:不屏蔽LFSR位[6:0]/三角波幅值为127;0111:不屏蔽LFSR位[7:0]/三角波幅值为255;1000:不屏蔽LFSR位[8:0]/三角波幅值为511;1001:不屏蔽LFSR位[9:0]/三角波幅值为1023;1010:不屏蔽LFSR位[10:0]/三角波幅值为2047;≥1011:不屏蔽LFSR位[11:0]/三角波幅值为4095。</td><td>0000b</td></tr><tr><td>[7:6]</td><td>WAVE1[1:0]</td><td>RW</td><td>DAC通道1的噪声/三角波生成使能。00:关闭波形发生器;01:使能噪声波形发生器;1x:使能三角波波形发生器。</td><td>00b</td></tr><tr><td>[5:3]</td><td>TSEL1[2:0]</td><td>RW</td><td>DAC通道1触发事件选择设置:</td><td>000b</td></tr><tr><td>2</td><td>TEN1</td><td>RW</td><td>DAC通道1外部触发模式使能:1: 使能 DAC通道1触发功能,写入 DAC_xDHR 寄存器的数据在3个APB1时钟周期后送入 DAC_DOR1 寄存器。0: 关闭 DAC通道1触发功能,写入 DAC_xDHR 寄存器的数据在1个APB1时钟周期后送入 DAC_DOR1 寄存器。注:如果选择软件触发,DAC_xDHR中的数据只需1个APB1时钟周期后送入 DAC_DOR1 寄存器。</td><td>0</td></tr><tr><td>1</td><td>BOFF1</td><td>RW</td><td>DAC通道1输出缓冲关闭控制(建议打开):1: 关闭 DAC通道1输出缓存;0: 打开 DAC通道1输出缓存。</td><td>0</td></tr><tr><td>0</td><td>EN1</td><td>RW</td><td>DAC通道1使能:1: 使能 DAC通道1;0: 关闭 DAC通道1。</td><td>0</td></tr></table>

注：配置寄存器包括通道1和通道2的配置，当同时使能（EN×位为‘1’）2个通道的输出，将以通道1的配置输出相同波形到2个硬件通道上；当只使能1号通道的输出，将以1号通道配置输出波形到1号硬件通道上，2号通道不输出；当只使能2号通道的输出，将以2号通道配置输出波形到2号硬件通道上，1号通道不输出。

### 17.4.2 DAC 软件触发寄存器（DAC_SWTR）

偏移地址： $0 \times 0 4$

<table><tr><td>31</td><td>30</td><td>29</td><td>28</td><td>27</td><td>26</td><td>25</td><td>24</td><td>23</td><td>22</td><td>21</td><td>20</td><td>19</td><td>18</td><td>17</td><td>16</td></tr><tr><td colspan="16">Reserved</td></tr><tr><td>15</td><td>14</td><td>13</td><td>12</td><td>11</td><td>10</td><td>9</td><td>8</td><td>7</td><td>6</td><td>5</td><td>4</td><td>3</td><td>2</td><td>1</td><td>0</td></tr><tr><td colspan="14">Reserved</td><td>SWTRIG2</td><td>SWTRIG1</td></tr></table>

<table><tr><td>位</td><td>名称</td><td>访问</td><td>描述</td><td>复位值</td></tr><tr><td>[31:2]</td><td>Reserved</td><td>R0</td><td>保留。</td><td>0</td></tr><tr><td>1</td><td>SWTRIG2</td><td>W0</td><td>DAC通道2软件触发控制位:1:使能DAC通道2软件触发;0:关闭DAC通道2软件触发。注:一旦DACxDHR中的数据(1个APB1时钟周期后)送入DAC_DOR2寄存器,该位将硬件清0。</td><td>0</td></tr><tr><td>0</td><td>SWTRIG1</td><td>W0</td><td>DAC通道1软件触发控制位:1:使能DAC通道1软件触发;</td><td>0</td></tr><tr><td></td><td></td><td></td><td>0: 关闭 DAC 通道 1 软件触发。注: 一旦 DAC_xDHR 中的数据 (1 个 APB1 时钟周期后) 送入 DAC_DOR1 寄存器, 该位将硬件清 0。</td><td></td></tr></table>

### 17.4.3 DAC 通道 1 右对齐 12 位数据保存寄存器（DAC_R12BDHR1）

偏移地址： $0 \times 0 8$

<table><tr><td>31</td><td>30</td><td>29</td><td>28</td><td>27</td><td>26</td><td>25</td><td>24</td><td>23</td><td>22</td><td>21</td><td>20</td><td>19</td><td>18</td><td>17</td><td>16</td></tr><tr><td colspan="16">Reserved</td></tr><tr><td>15</td><td>14</td><td>13</td><td>12</td><td>11</td><td>10</td><td>9</td><td>8</td><td>7</td><td>6</td><td>5</td><td>4</td><td>3</td><td>2</td><td>1</td><td>0</td></tr><tr><td colspan="4">Reserved</td><td colspan="12">DACC1DHR[11:0]</td></tr></table>

<table><tr><td>位</td><td>名称</td><td>访问</td><td>描述</td><td>复位值</td></tr><tr><td>[31:12]</td><td>Reserved</td><td>R0</td><td>保留。</td><td>0</td></tr><tr><td>[11:0]</td><td>DACC1DHR[11:0]</td><td>RW</td><td>DAC通道1的12位右对齐数据。</td><td>0</td></tr></table>

### 17.4.4 DAC 通道 1 左对齐 12 位数据保存寄存器（DAC_L12BDHR1）

偏移地址： $0 \times 0 0$

<table><tr><td>31</td><td>30</td><td>29</td><td>28</td><td>27</td><td>26</td><td>25</td><td>24</td><td>23</td><td>22</td><td>21</td><td>20</td><td>19</td><td>18</td><td>17</td><td>16</td></tr><tr><td colspan="16">Reserved</td></tr><tr><td>15</td><td>14</td><td>13</td><td>12</td><td>11</td><td>10</td><td>9</td><td>8</td><td>7</td><td>6</td><td>5</td><td>4</td><td>3</td><td>2</td><td>1</td><td>0</td></tr><tr><td colspan="12">DACC1DHR[11:0]</td><td colspan="4">Reserved</td></tr></table>

<table><tr><td>位</td><td>名称</td><td>访问</td><td>描述</td><td>复位值</td></tr><tr><td>[31:16]</td><td>Reserved</td><td>R0</td><td>保留。</td><td>0</td></tr><tr><td>[15:4]</td><td>DACC1DHR[11:0]</td><td>RW</td><td>DAC通道1的12位左对齐数据。</td><td>0</td></tr><tr><td>[3:0]</td><td>Reserved</td><td>R0</td><td>保留。</td><td>0</td></tr></table>

### 17.4.5 DAC 通道 1 右对齐 8 位数据保存寄存器（DAC_R8BDHR1）

偏移地址： $0 \times 1 0$

<table><tr><td>31</td><td>30</td><td>29</td><td>28</td><td>27</td><td>26</td><td>25</td><td>24</td><td>23</td><td>22</td><td>21</td><td>20</td><td>19</td><td>18</td><td>17</td><td>16</td></tr><tr><td colspan="16">Reserved</td></tr><tr><td>15</td><td>14</td><td>13</td><td>12</td><td>11</td><td>10</td><td>9</td><td>8</td><td>7</td><td>6</td><td>5</td><td>4</td><td>3</td><td>2</td><td>1</td><td>0</td></tr><tr><td colspan="8">Reserved</td><td colspan="8">DACC1DHR[7:0]</td></tr></table>

<table><tr><td>位</td><td>名称</td><td>访问</td><td>描述</td><td>复位值</td></tr><tr><td>[31:8]</td><td>Reserved</td><td>R0</td><td>保留。</td><td>0</td></tr><tr><td>[7:0]</td><td>DACC1DHR[7:0]</td><td>RW</td><td>DAC通道1的8位右对齐数据。</td><td>0</td></tr></table>

### 17.4.6 DAC 通道 2 右对齐 12 位数据保存寄存器（DAC_R12BDHR2）

偏移地址： $0 \times 1 4$

<table><tr><td>31</td><td>30</td><td>29</td><td>28</td><td>27</td><td>26</td><td>25</td><td>24</td><td>23</td><td>22</td><td>21</td><td>20</td><td>19</td><td>18</td><td>17</td><td>16</td></tr><tr><td colspan="16">Reserved</td></tr><tr><td>15</td><td>14</td><td>13</td><td>12</td><td>11</td><td>10</td><td>9</td><td>8</td><td>7</td><td>6</td><td>5</td><td>4</td><td>3</td><td>2</td><td>1</td><td>0</td></tr><tr><td colspan="4">Reserved</td><td colspan="12">DACC2DHR[11:0]</td></tr></table>

<table><tr><td>位</td><td>名称</td><td>访问</td><td>描述</td><td>复位值</td></tr><tr><td>[31:12]</td><td>Reserved</td><td>R0</td><td>保留。</td><td>0</td></tr><tr><td>[11:0]</td><td>DACC2DHR[11:0]</td><td>RW</td><td>DAC通道2的12位右对齐数据。</td><td>0</td></tr></table>

### 17.4.7 DAC 通道 2 左对齐 12 位数据保存寄存器（DAC_L12BDHR2）

偏移地址： $0 \times 1 8$

<table><tr><td>31</td><td>30</td><td>29</td><td>28</td><td>27</td><td>26</td><td>25</td><td>24</td><td>23</td><td>22</td><td>21</td><td>20</td><td>19</td><td>18</td><td>17</td><td>16</td></tr><tr><td colspan="16">Reserved</td></tr><tr><td>15</td><td>14</td><td>13</td><td>12</td><td>11</td><td>10</td><td>9</td><td>8</td><td>7</td><td>6</td><td>5</td><td>4</td><td>3</td><td>2</td><td>1</td><td>0</td></tr><tr><td colspan="12">DACC2DHR[11:0]</td><td colspan="4">Reserved</td></tr></table>

<table><tr><td>位</td><td>名称</td><td>访问</td><td>描述</td><td>复位值</td></tr><tr><td>[31:16]</td><td>Reserved</td><td>R0</td><td>保留。</td><td>0</td></tr><tr><td>[15:4]</td><td>DACC2DHR[11:0]</td><td>RW</td><td>DAC通道2的12位左对齐数据。</td><td>0</td></tr><tr><td>[3:0]</td><td>Reserved</td><td>R0</td><td>保留。</td><td>0</td></tr></table>

### 17.4.8 DAC 通道 2 右对齐 8 位数据保存寄存器（DAC_R8BDHR2）

偏移地址： ${ 0 } { \times 1 } { 0 }$

<table><tr><td>31</td><td>30</td><td>29</td><td>28</td><td>27</td><td>26</td><td>25</td><td>24</td><td>23</td><td>22</td><td>21</td><td>20</td><td>19</td><td>18</td><td>17</td><td>16</td></tr><tr><td colspan="16">Reserved</td></tr><tr><td>15</td><td>14</td><td>13</td><td>12</td><td>11</td><td>10</td><td>9</td><td>8</td><td>7</td><td>6</td><td>5</td><td>4</td><td>3</td><td>2</td><td>1</td><td>0</td></tr><tr><td colspan="8">Reserved</td><td colspan="8">DACC2DHR[7:0]</td></tr></table>

<table><tr><td>位</td><td>名称</td><td>访问</td><td>描述</td><td>复位值</td></tr><tr><td>[31:8]</td><td>Reserved</td><td>R0</td><td>保留。</td><td>0</td></tr><tr><td>[7:0]</td><td>DACC2DHR[7:0]</td><td>RW</td><td>DAC通道2的8位右对齐数据。</td><td>0</td></tr></table>

### 17.4.9 DAC 双通道右对齐 12 位数据保存寄存器（DAC_RD12BDHR）

偏移地址： $_ { 0 \times 2 0 }$

<table><tr><td>31</td><td>30</td><td>29</td><td>28</td><td>27</td><td>26</td><td>25</td><td>24</td><td>23</td><td>22</td><td>21</td><td>20</td><td>19</td><td>18</td><td>17</td><td>16</td></tr><tr><td colspan="4">Reserved</td><td colspan="12">DACC2DHR[11:0]</td></tr><tr><td>15</td><td>14</td><td>13</td><td>12</td><td>11</td><td>10</td><td>9</td><td>8</td><td>7</td><td>6</td><td>5</td><td>4</td><td>3</td><td>2</td><td>1</td><td>0</td></tr><tr><td colspan="4">Reserved</td><td colspan="12">DACC1DHR[11:0]</td></tr></table>

<table><tr><td>位</td><td>名称</td><td>访问</td><td>描述</td><td>复位值</td></tr><tr><td>[31:28]</td><td>Reserved</td><td>R0</td><td>保留。</td><td>0</td></tr><tr><td>[27:16]</td><td>DACC2DHR[11:0]</td><td>RW</td><td>DAC通道2的12位右对齐数据。</td><td>0</td></tr><tr><td>[15:12]</td><td>Reserved</td><td>R0</td><td>保留。</td><td>0</td></tr><tr><td>[11:0]</td><td>DACC1DHR[11:0]</td><td>RW</td><td>DAC通道1的12位右对齐数据。</td><td>0</td></tr></table>

### 17.4.10 DAC 双通道左对齐 12 位数据保存寄存器（DAC_LD12BDHR）

偏移地址： ${ 0 } { \times 2 4 }$

<table><tr><td>31</td><td>30</td><td>29</td><td>28</td><td>27</td><td>26</td><td>25</td><td>24</td><td>23</td><td>22</td><td>21</td><td>20</td><td>19</td><td>18</td><td>17</td><td>16</td></tr><tr><td colspan="12">DACC2DHR[11:0]</td><td colspan="4">Reserved</td></tr><tr><td>15</td><td>14</td><td>13</td><td>12</td><td>11</td><td>10</td><td>9</td><td>8</td><td>7</td><td>6</td><td>5</td><td>4</td><td>3</td><td>2</td><td>1</td><td>0</td></tr><tr><td colspan="12">DACC1DHR[11:0]</td><td colspan="4">Reserved</td></tr></table>

<table><tr><td>位</td><td>名称</td><td>访问</td><td>描述</td><td>复位值</td></tr><tr><td>[31:20]</td><td>DACC2DHR[11:0]</td><td>RW</td><td>DAC通道2的12位左对齐数据。</td><td>0</td></tr><tr><td>[19:16]</td><td>Reserved</td><td>RO</td><td>保留。</td><td>0</td></tr><tr><td>[15:4]</td><td>DACC1DHR[11:0]</td><td>RW</td><td>DAC通道1的12位左对齐数据。</td><td>0</td></tr><tr><td>[3:0]</td><td>Reserved</td><td>RO</td><td>保留。</td><td>0</td></tr></table>

### 17.4.11 DAC 双通道右对齐 8 位数据保存寄存器（DAC_RD8BDHR）

偏移地址： $_ { 0 \times 2 8 }$

<table><tr><td>31</td><td>30</td><td>29</td><td>28</td><td>27</td><td>26</td><td>25</td><td>24</td><td>23</td><td>22</td><td>21</td><td>20</td><td>19</td><td>18</td><td>17</td><td>16</td></tr><tr><td colspan="16">Reserved</td></tr><tr><td>15</td><td>14</td><td>13</td><td>12</td><td>11</td><td>10</td><td>9</td><td>8</td><td>7</td><td>6</td><td>5</td><td>4</td><td>3</td><td>2</td><td>1</td><td>0</td></tr><tr><td colspan="8">DACC2DHR[7:0]</td><td colspan="8">DACC1DHR[7:0]</td></tr></table>

<table><tr><td>位</td><td>名称</td><td>访问</td><td>描述</td><td>复位值</td></tr><tr><td>[31:16]</td><td>Reserved</td><td>R0</td><td>保留。</td><td>0</td></tr><tr><td>[15:8]</td><td>DACC2DHR[7:0]</td><td>RW</td><td>DAC通道2的8位右对齐数据。</td><td>0</td></tr><tr><td>[7:0]</td><td>DACC1DHR[7:0]</td><td>RW</td><td>DAC通道1的8位右对齐数据。</td><td>0</td></tr></table>

### 17.4.12 DAC 通道 1 数据输出寄存器（DAC_DOR1）

偏移地址： $\mathtt { 0 } \mathtt { x } 2 0$

<table><tr><td>31</td><td>30</td><td>29</td><td>28</td><td>27</td><td>26</td><td>25</td><td>24</td><td>23</td><td>22</td><td>21</td><td>20</td><td>19</td><td>18</td><td>17</td><td>16</td></tr><tr><td colspan="16">Reserved</td></tr><tr><td>15</td><td>14</td><td>13</td><td>12</td><td>11</td><td>10</td><td>9</td><td>8</td><td>7</td><td>6</td><td>5</td><td>4</td><td>3</td><td>2</td><td>1</td><td>0</td></tr><tr><td colspan="4">Reserved</td><td colspan="12">DACC1DOR[11:0]</td></tr></table>

<table><tr><td>位</td><td>名称</td><td>访问</td><td>描述</td><td>复位值</td></tr><tr><td>[31:12]</td><td>Reserved</td><td>R0</td><td>保留。</td><td>0</td></tr><tr><td>[11:0]</td><td>DACC1DOR[11:0]</td><td>R0</td><td>DAC通道1输出数据。</td><td>0</td></tr></table>

### 17.4.13 DAC 通道 2 数据输出寄存器（DAC_DOR2）

偏移地址： $\mathtt { 0 } { \times } 3 0$

<table><tr><td>31</td><td>30</td><td>29</td><td>28</td><td>27</td><td>26</td><td>25</td><td>24</td><td>23</td><td>22</td><td>21</td><td>20</td><td>19</td><td>18</td><td>17</td><td>16</td></tr><tr><td colspan="16">Reserved</td></tr><tr><td>15</td><td>14</td><td>13</td><td>12</td><td>11</td><td>10</td><td>9</td><td>8</td><td>7</td><td>6</td><td>5</td><td>4</td><td>3</td><td>2</td><td>1</td><td>0</td></tr><tr><td colspan="4">Reserved</td><td colspan="12">DACC2DOR[11:0]</td></tr></table>

<table><tr><td>位</td><td>名称</td><td>访问</td><td>描述</td><td>复位值</td></tr><tr><td>[31:12]</td><td>Reserved</td><td>R0</td><td>保留。</td><td>0</td></tr><tr><td>[11:0]</td><td>DACC2DOR[11:0]</td><td>R0</td><td>DAC通道2输出数据。</td><td>0</td></tr></table>


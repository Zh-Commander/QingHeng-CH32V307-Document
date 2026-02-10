# 第 14 章 高级定时器（ADTM）

本章模块描述适用于CH32F2x、CH32V2x和CH32V3x微控制器全系列产品。

高级定时器模块包含一个功能强大的 16 位自动重装定时器（TIM1、TIM8、TIM9 和 TIM10），可用于测量脉冲宽度或产生脉冲、PWM波等。用于电机控制、电源等领域。

## 14.1 主要特征

高级定时器（TIM1/8/9/10）的主要特征包括：

16 位自动重装计数器，支持增计数模式，减计数模式和增减计数模式；  
16 位预分频器，分频系数从 $1 \sim 6 5 5 3 6$ 之间动态可调；  
支持四路独立的比较捕获通道；  
$\bullet$ 每路比较捕获通道支持多种工作模式，比如：输入捕获，输出比较，PWM 生成和单脉冲输出；  
$\bullet$ 支持可编程死区时间的互补输出；  
$\bullet$ 支持外部信号控制定时器；  
$\bullet$ 支持使用重复计数器在确定周期后更新定时器；  
$\bullet$ 支持使用刹车信号将定时器复位或置其于确定状态；  
$\bullet$ 支持在多种模式下使用 DMA；  
$\bullet$ 支持增量式编码器；  
支持定时器之间的级联和同步

## 14.2 原理和结构

本节主要论述高级定时器的内部构造。

### 14.2.1 概述

如图14-1，高级定时器的结构大致可以分为三部分，即输入时钟部分，核心计数器部分和比较捕获通道部分。

高级定时器的时钟可以来自于APB总线时钟（CK_INT），可以来自外部时钟输入引脚（TIMx_ETR），亦可以来自于其他具有时钟输出功能的定时器（ITRx），还可以来自于比较捕获通道的输入端（TIMx_CHx）。这些输入的时钟信号经过各种设定的滤波分频等操作后成为 CK_PSC 时钟，输出给核心计数器部分。另外，这些复杂的时钟来源还可以作为 TRGO 输出给其他的定时器、ADC 和 DAC 等外设。

高级定时器的核心是一个16位计数器（CNT）。CK_PSC经过预分频器（PSC）分频后，成为 CK_CNT并输出给CNT，CNT支持增计数模式、减计数模式和增减计数模式，并有一个自动重装值寄存器（ATRLR）在每个计数周期结束后为CNT重装载初始值。另外还有个辅助计数器在一旁计数 ATRLR为 CNT重装载初值的次数，当次数达到重复计数值寄存器（RPTCR）里设置的次数时，可以产生特定事件。

高级定时器拥有四组比较捕获通道，每组比较捕获通道都可以从专属的引脚上输入脉冲，也可以向引脚输出波形，即比较捕获通道支持输入和输出模式。比较捕获寄存器每个通道的输入都支持滤波、分频和边沿检测等操作，并支持通道间的互触发，还能为核心计数器 CNT提供时钟。每个比较捕获通道都拥有一组比较捕获寄存器（CHxCVR），支持与主计数器（CNT）进行比较而输出脉冲。

![](images/9d7c7d99488e1c445ddafa64fbc03d19a01032056c6e4890c8e928c238377f77.jpg)  
图14-1 高级定时器的结构框图

### 14.2.2 时钟输入

![](images/74826bc4cdf2506a552e0bc40d28584b06784bf48b83eb6450a5d687c630d937.jpg)  
图 14-2 高级定时器的 CK_PSC 来源框图

高级定时器 CK_PSC 的时钟来源很多，可以分为 4 类：

1） 外部时钟引脚（ETR）输入时钟的路线：ETR ETRP→ETRF；  
2） 内部 APB 时钟输入路线：CK_INT；  
3） 来自比较捕获通道引脚（TIMx_CHx）的路线：TIMx_CHx→TIx→TIxFPx，此路线也用于编码器模式；  
4） 来自内部其他定时器的输入：ITRx；

通过决定 CK_PSC 来源的 SMS 的输入脉冲选择可以将实际的操作分为 4 类：

1） 选择内部时钟源（CK_INT）；  
2） 外部时钟源模式1；  
3） 外部时钟源模式 2；  
4） 编码器模式；

上文提到的 4 种时钟源来源都可通过这 4 种操作选定。

#### 14.2.2.1 内部时钟源（CK_INT）

如果将 SMS 域保持 000b 时启动高级定时器，那么就是选定内部时钟源（CK_INT）为时钟。此时CK_INT 就是 CK_PSC。

#### 14.2.2.2 外部时钟源模式 1

如果将 SMS 域设置为 111b 时，就会启用外部时钟源模式 1。启用外部时钟源 1 时，TRGI 被选定为CK_PSC的来源，值得注意的，还需要通过配置 TS域来选择 TRGI的来源。TS域可选择以下几种脉冲作为时钟来源：

1） 内部触发（ITRx，x 为 0,1,2,3）；  
2） 比较捕获通道 1 经过边缘检测器后的信号（TI1F_ED）；  
3） 比较捕获通道的信号 TI1FP1、TI2FP2；  
4） 来自外部时钟引脚输入的信号 ETRF。

#### 14.2.2.3 外部时钟源模式 2

使用外部触发模式2能在外部时钟引脚输入的每一个上升沿或下降沿计数。将 ECE位置位时，将使用外部时钟源模式2。使用外部时钟源模式 2时，ETRF 被选定为 CK_PSC。ETR引脚经过可选的反相

器（ETP），分频器（ETPS）后成为 ETRP，再经过滤波器（ETF）后即成为 ETRF。

在 ECE 位置位且将 SMS 设为 111b 时，相当于 TS 选择 ETRF 为输入。

#### 14.2.2.4 编码器模式

将 SMS 置为 001b，010b，011b 将会启用编码器模式。启用编码器模式可以选择在 TI1FP1 和 TI2FP2中某一个特定的电平下以另一个跳变沿作为信号进行信号输出。此模式用于外接编码器使用的情况下。具体功能参考14.3.9节。

### 14.2.3 计数器和周边

CK_PSC输入给预分频器（PSC）进行分频。PSC是 16位的，实际的分频系数相当于 R16_TIMx_PSC的值 $+ 1$ 。CK_PSC 经过 PSC 会成为 CK_INT。更改 R16_TIM1_PSC 的值并不会实时生效，而会在更新事件后更新给PSC。更新事件包括UG位清零和复位。定时器的核心是一个 16位计数器（CNT），CK_CNT最终会输入给 CNT，CNT 支持增计数模式、减计数模式和增减计数模式，并有一个自动重装值寄存器（ATRLR）在每个计数周期结束后为 CNT 重新装载初始值。另外还有个辅助计数器在一旁记录 ATRLR为 CNT 重新装载初值的次数，当达到重复计数值寄存器（RPTCR）里设置的次数时，可以产生特定事件。

### 14.2.4 比较捕获通道和周边

比较捕获通道是定时器实现复杂功能的主要组件，它的核心是比较捕获寄存器，辅以外围输入部分的数字滤波，分频和通道间复用、输出部分的比较器和输出控制组成。

![](images/fb11e2fc5d1b95207bf7c2106698c08b5ff41962496e279561d009aff757f7d5.jpg)  
图14-3 比较捕获通道的结构框图

比较捕获通道的结构框图如图14-3所示。信号从通道 x 引脚输入进来后可选做为 TIx（TI1的来源可以不只是CH1，见定时器的结构框图 14-1），TI1经过滤波器（ICF[3:0]）生成TI1F，再经过边沿检测器分成 TI1F_Rising 和 TI1F_Falling，这两个信号经过选择（CC1P）生成 TI1FP1，TI1FP1 和来自通道2的TI2FP1一起送给CC1S选择成为IC1，经过 ICPS分频后送给比较捕获寄存器。

比较捕获寄存器由一个预装载寄存器和一个影子寄存器组成，读写过程仅操作预装载寄存器。在捕获模式下，捕获发生在影子寄存器上，然后复制到预装载寄存器；在比较模式下，预装载寄存器的内容被复制到影子寄存器中，然后影子寄存器的内容与核心计数器（CNT）进行比较。

## 14.3 功能和实现

高级定时器复杂功能的实现都是对定时器的比较捕获通道、时钟输入电路和计数器及周边部分的操作实现的。定时器的时钟输入可以来自于包括比较捕获通道的输入在内的多个时钟源。对比较捕获通道和时钟源选择的操作直接决定其功能。比较捕获通道是双向的，可以工作在输入和输出模式。

### 14.3.1 输入捕获模式

输入捕获模式是定时器的基本功能之一。输入捕获模式的原理是，当检测到 ${ \mathsf { I C X P S } }$ 信号上确定的边沿后，则发生捕获事件，计数器当前的值会被锁存到比较捕获寄存器（R16_TIMx_CHCTLRx）中。发生捕获事件时， ${ \tt C C } \times { \tt I F }$ （在 R16_TIMx_INTFR 中）被置位，如果使能了中断或 DMA，还会产生相应中断或DMA。如果发生捕获事件时， ${ \mathsf { C C } } \times { \mathsf { I F } }$ 已经被置位了，那么 ${ \tt C C } \tt { x O F }$ 位会被置位。CCxIF可由软件清除，也可以通过读取比较捕获寄存器由硬件清除。 $\mathtt { C C } \mathtt { x O F }$ 由软件清除。

举个通道1的例子来说明使用输入捕获模式的步骤，如下：

1） 配置 $\mathtt { C C } \mathtt { x } \mathtt { S }$ 域，选择 $1 0 \times$ 信号的来源。比如设为 10b，选择 TI1FP1 作为 IC1的来源，而不可以使用默认设置， $\mathtt { C O x S }$ 域默认是使比较捕获模块作为输出通道；  
2） 配置 ${ | 0 \times | }$ 域，设定 TI 信号的数字滤波器。数字滤波器会以确定的频率，采样确定的次数，再输出一个跳变。这个采样频率和次数是通过 ${ | 0 \times | }$ 来确定的；  
3） 配置 $\mathtt { C C } \mathtt { x } \mathtt { P }$ 位，设定 TIxFPx 的极性。比如保持 CC1P 位为低，选择上升沿跳变；  
4） 配置 ${ \mathsf { I C X P S } }$ 域，设定 $1 0 \times$ 信号成为 ${ \mathsf { I C X P S } }$ 之间的分频系数。比如保持 ${ \mathsf { I C X P S } }$ 为 00b，不分频；  
5） 配置 $\mathtt { C C } \mathtt { x } \mathtt { E }$ 位，允许捕获核心计数器（CNT）的值到比较捕获寄存器中。置 CC1E位；  
6） 根据需要配置 $\mathsf { C C } \mathsf { \times } { \mathsf { I E } }$ 和 $\complement 0 \times \mathsf { D E }$ 位，决定是否允许使能中断或 DMA。

至此已经将比较捕获通道配置完成。

当TI1输入了一个被捕获的脉冲时，核心计数器（CNT）的值会被记录到比较捕获寄存器中，CC1IF被置位，当CC1IF在之前就已经被置位时，CCIOF位也会被置位。如果 CC1IE位，那么会产生一个中断；如果 CC1DE 被置位，会产生一个 DMA 请求。可以通过写事件产生寄存器（TIMx_SWEVGR）的方式由软件产生一个输入捕获事件。

### 14.3.2 比较输出模式

比较输出模式是定时器的基本功能之一。比较输出模式的原理是在核心计数器（CNT）的值与比较捕获寄存器的值一致时，输出特定的变化或波形。 $0 0 \times 1 1$ 域（在 R16_TIMx_CHCTLRx 中）和 $\complement 0 \times \emptyset$ 位（在 R16_TIMx_CCER 中）决定输出的是确定的高低电平还是电平翻转。产生比较一致事件时还会置CCxIF位，如果预先置了 $\mathsf { C C } \mathsf { \times } { \mathsf { I E } }$ 位，则会产生一个中断；如果预先设置了 $\complement 0 \times \mathsf { D E }$ 位，则会产生一个 DMA请求。

配置为比较输出模式的步骤为下：

1） 配置核心计数器（CNT）的时钟源和自动重装值；  
2） 设置需要对比的计数值到比较捕获寄存器（R16_TIMx_CHxCVR）中；  
3） 如果需要产生中断，置 $\complement 0 \times 1 \mathsf { E }$ 位；  
4） 保持 ${ \tt O C } \tt { x P E }$ 为 0，禁用比较寄存器的预装载寄存器；  
5） 设定输出模式，设置 $0 0 \times 1 1$ 域和 $\complement 0 \times \emptyset$ 位；  
6） 使能输出，置 $\mathtt { C C } \mathtt { x } \mathtt { E }$ 位；  
7） 置 CEN 位启动定时器。

### 14.3.3 强制输出模式

定时器的比较捕获通道的输出模式可以由软件强制输出确定的电平，而不依赖比较捕获寄存器的影子寄存器和核心计数器的比较。

具体的做法是将 $0 0 \times 1 1$ 置为100b，即为强制将OCxREF 置为低；或者将 $0 0 \times 1 1$ 置为 101b，即为强制

将 ${ 0 0 } \times { \mathsf { R E F } }$ 置为高。

需要注意的是，将 $0 0 \times 1 1$ 强制置为 100b 或者 101b，内部核心计数器和比较捕获寄存器的比较过程还在进行，相应的标志位还在置位，中断和 DMA请求还在产生。

### 14.3.4 PWM 输入模式

PWM 输入模式是用来测量 PWM 的占空比和频率的，是输入捕获模式的一种特殊情况。除下列区别外，操作和输入捕获模式相同：PWM占用两个比较捕获通道，且两个通道的输入极性设为相反，其中一个信号被设为触发输入，SMS设为复位模式。

例如，测量从 TI1 输入的 PWM 波的周期和频率，需要进行以下操作：

1） 将 TI1(TI1FP1)设为 IC1 信号的输入。将 CC1S 置为 01b；  
2） 将 TI1FP1 置为上升沿有效。将 CC1P 保持为 0；  
3） 将 TI1(TI1FP2)置为 IC2 信号的输入。将 CC2S 置为 10b；  
4） 选 TI1FP2 置为下降沿有效。将 CC2P 置为 1；  
5） 时钟源的来源选择 TI1FP1。将 TS 设为 101b；  
6） 将 SMS 设为复位模式，即 100b；  
7） 使能输入捕获。CC1E 和 CC2E 置位；

这样比较捕获寄存器 1 的值就是 PWM 的周期，而比较捕获寄存器 2 的值就是其占空比。

### 14.3.5 PWM 输出模式

PWM 输出模式是定时器的基本功能之一。PWM 输出模式最常见的是使用重装值确定 PWM 频率，使用捕获比较寄存器确定占空比的方法。将 $0 0 \times 1 1$ 域中置 110b 或 111b 使用 PWM 模式 1 或模式 2，置OCxPE 位使能预装载寄存器，最后置 ARPE 位使能预装载寄存器的自动重装载。由于在发生一个更新事件时，预装载寄存器的值才能被送到影子寄存器，所以在核心计数器开始计数之前，需要置 UG 位来初始化所有寄存器。在 PWM 模式下，核心计数器和比较捕获寄存器一直在进行比较，根据 CMS 位，定时器能够输出边沿对齐或中央对齐的PWM信号。

#### 边沿对齐

使用边沿对齐时，核心计数器增计数或减计数，在PWM模式 1的情景下，在核心计数器的值大于比较捕获寄存器时，OCxREF为高；当核心计数器的值小于比较捕获寄存器时（比如核心计数器增长到R16_TIMx_ATRLR 的值而恢复成全 0 时），OCxREF 为低。

#### 中央对齐

使用中央对齐模式时，核心计数器运行在增计数和减计数交替进行的模式下，OCxREF 在核心计数器和比较捕获寄存器的值一致时进行上升和下降的跳变。但比较标志在三种中央对齐模式下，置位的时机有所不同。在使用中央对齐模式时，最好在启动核心计数器之前产生一个软件更新标志（置 UG位）。

### 14.3.6 互补输出和死区

比较捕获通道一般有两个输出引脚（比较捕获通道4 只有一个输出引脚），能输出两个互补的信号（ $\mathtt { 0 0 x }$ 和 ${ \tt O C } \times { \tt N } ,$ ）， $\mathtt { 0 0 x }$ 和 $0 0 \times 1 1$ 可以通过 $\mathtt { C C } \mathtt { x } \mathtt { P }$ 和 ${ \mathsf { C C } } { \mathsf { x N P } }$ 位独立地设置极性，通过 $\mathtt { C C } \mathtt { x } \mathtt { E }$ 和 ${ \tt C C } \times { \tt N E }$ 独立地设置输出使能，通过 MOE、OIS、OISN、OSSI、OSSR 位进行死区和其他的控制。同时使能 $\mathtt { 0 0 x }$ 和$\mathtt { 0 0 x N }$ 输出将插入死区，每个通道都有一个 10 位的死区发生器。如果存在刹车电路则还要设置 MOE 位。$\mathtt { 0 0 x }$ 和 $0 0 \times 1 1$ 由 ${ 0 0 } \times { \mathsf { R E F } }$ 关联产生，如果 $\mathtt { 0 0 x }$ 和 $0 0 \times 1 1$ 都是高有效，那么 $\mathtt { O C x }$ 与 ${ 0 0 } \times { \mathsf { R E F } }$ 相同，只是 $\mathtt { 0 0 x }$ 的上升沿相当于 OCxREF 有一个延迟， $\mathtt { 0 0 x N }$ 与 $\mathtt { O C x }$ REF 相反，它的上升沿相对参考信号的下降沿会有一个延迟，如果延迟大于有效输出宽度，则不会产生相应的脉冲。

如图 14-4 展示了 $\mathtt { 0 0 x }$ 和 $0 0 \times 1 1$ 与 ${ 0 0 } \times { \mathsf { R E F } }$ 的关系，并展示出死区。

![](images/21bf9e687300b7ab4d10354fb8f56cb28601deddf5e54ee22a453127d52cc5ca.jpg)  
图14-4 互补输出和死区

### 14.3.7 刹车信号

当产生刹车信号时，输出使能信号和无效电平都会根据 MOE、OIS、OISN、OSSI 和 OSSR等位进行修改。但 $\mathtt { 0 0 x }$ 和 $0 0 \times 1 1$ 不会在任何时间都处在有效电平。刹车事件源可以来自于刹车输入引脚，也可以是一个时钟失败事件，而时钟失败事件由 CSS（时钟安全系统）产生。

在系统复位后，刹车功能被默认禁止（MOE位为低），置 BKE位可以使能刹车功能，输入的刹车信号的极性可以通过设置 BKP 设置，BKE 和 BKP 信号可以被同时写入，在真正写入之前会有一个 APB时钟的延迟，因此需要等一个APB周期才能正确读出写入值。

在刹车引脚出现选定的电平系统将产生如下动作：

1） MOE 位被异步清零，根据 SOOI 位的设置将输出置为无效状态、空闲状态或复位状态；  
2） 在 MOE 被清零后，每一个输出通道输出由 OISx 确定的电平；  
3） 当使用互补输出时：输出被置于无效状态，具体取决于极性；  
4） 如果 BIE 被置位，当 BIF 置位，会产生一个中断；如果设置了 BDE 位，则会产生一个 DMA 请求；  
5） 如果 AOE 被置位，在下一个更新事件 UEV 时，MOE 位被自动置位。

### 14.3.8 单脉冲模式

单脉冲模式可以用于让微控制器响应一个特定的事件，使之在一个延迟之后产生一个脉冲，延迟和脉冲的宽度可编程。置 OPM 位可以使核心计数器在产生下一个更新事件 UEV 时（计数器翻转到 0）停止。

如图 14-5，需要在 TI2 输入引脚上检测到一个上升沿开始，延迟 Tdelay 之后，在 OC1 上产生一个长度为Tpulse的正脉冲：

![](images/bf0c0f74ac10446bde3f378fb6e1ea5ce75076a357f7e578a8ce973207bb65cd.jpg)  
图 14-5 单脉冲的产生

1） 设定 TI2 为触发。置 CC2S 域为 01b，把 TI2FP2 映射到 TI2；置 CC2P 位为 0b，TI2FP2 设为上升沿检测；置 TS 域为 110b，TI2FP2 设为触发源；置 SMS 域为 110b，TI2FP2 被用来启动计数器；  
2） Tdelay 由比较捕获寄存器的的值确定，Tpulse 由自动重装值寄存器的值和比较捕获寄存器的值

确定。

### 14.3.9 编码器模式

编码器模式是定时器的一个典型应用，可以用来接入编码器的双相输出，核心计数器的计数方向和编码器的转轴方向同步，编码器每输出一个脉冲就会使核心计数器加一或减一。使用编码器的步骤为：将 SMS 域置为 001b（只在 TI2 边沿计数）、010b（只在 TI1 边沿计数）或 011b（在 TI1 和 TI2 双边沿计数），将编码器接到比较捕获通道 1、2 的输入端，给重装值寄存器设一个值，这个值可以设的大一点。在编码器模式时，定时器内部的比较捕获寄存器，预分频器，重复计数寄存器等都正常工作。下表表明了计数方向和编码器信号的关系。

表 14-1 定时器编码器模式的计数方向和编码器信号之间的关系  

<table><tr><td rowspan="2">计数有效边沿</td><td rowspan="2">相对信号的电平</td><td colspan="2">TI1FP1 信号边沿</td><td colspan="2">TI2FP2 信号</td></tr><tr><td>上升沿</td><td>下降沿</td><td>上升沿</td><td>下降沿</td></tr><tr><td rowspan="2">仅在 TI1 边沿计数</td><td>高</td><td>向下计数</td><td>向上计数</td><td rowspan="2" colspan="2">不计数</td></tr><tr><td>低</td><td>向上计数</td><td>向下计数</td></tr><tr><td rowspan="2">仅在 TI2 边沿计数</td><td>高</td><td rowspan="2" colspan="2">不计数</td><td>向上计数</td><td>向下计数</td></tr><tr><td>低</td><td>向下计数</td><td>向上计数</td></tr><tr><td rowspan="2">在 TI1 和 TI2 双边沿计数</td><td>高</td><td>向下计数</td><td>向上计数</td><td>向上计数</td><td>向下计数</td></tr><tr><td>低</td><td>向上计数</td><td>向下计数</td><td>向下计数</td><td>向上计数</td></tr></table>

### 14.3.10 定时器同步模式

定时器能够输出时钟脉冲（TRGO），也能接收其他定时器的输入（ITRx）。不同的定时器的 ITRx的来源（别的定时器的TRGO）是不一样的。定时器内部触发连接如表 14-2所示。

表 14-2 TIMx 内部触发连接  

<table><tr><td>从定时器</td><td>ITRO (TS=000)</td><td>ITR1 (TS=001)</td><td>ITR2 (TS=010)</td><td>ITR3 (TS=011)</td></tr><tr><td>TIM1</td><td>TIM5</td><td>TIM2</td><td>TIM3</td><td>TIM4</td></tr><tr><td>TIM8</td><td>TIM1</td><td>TIM2</td><td>TIM4</td><td>TIM5</td></tr><tr><td>TIM9</td><td>TIM10</td><td>TIM5</td><td>TIM6</td><td>TIM7</td></tr><tr><td>TIM10</td><td>TIM9</td><td>TIM2</td><td>TIM4</td><td>TIM5</td></tr></table>

### 14.3.11 调试模式

当系统进入调试模式时，定时器根据 DBG模块的设置继续运转或停止。

## 14.4 寄存器描述

表 14-3 TIM1 相关寄存器列表  

<table><tr><td>名称</td><td>访问地址</td><td>描述</td><td>复位值</td></tr><tr><td>R16_TIM1_CTLR1</td><td>0x40012C00</td><td>控制寄存器1</td><td>0x0000</td></tr><tr><td>R16_TIM1_CTLR2</td><td>0x40012C04</td><td>控制寄存器2</td><td>0x0000</td></tr><tr><td>R16_TIM1_SMCFGR</td><td>0x40012C08</td><td>从模式控制寄存器</td><td>0x0000</td></tr><tr><td>R16_TIM1_DMAINTENR</td><td>0x40012C0C</td><td>DMA/中断使能寄存器</td><td>0x0000</td></tr><tr><td>R16_TIM1_INTFR</td><td>0x40012C10</td><td>中断状态寄存器</td><td>0x0000</td></tr><tr><td>R16_TIM1_SWEVGR</td><td>0x40012C14</td><td>事件产生寄存器</td><td>0x0000</td></tr><tr><td>R16_TIM1_CHCTLR1</td><td>0x40012C18</td><td>比较/捕获控制寄存器1</td><td>0x0000</td></tr><tr><td>R16_TIM1_CHCTLR2</td><td>0x40012C1C</td><td>比较/捕获控制寄存器2</td><td>0x0000</td></tr><tr><td>R16_TIM1_CCER</td><td>0x40012C20</td><td>比较/捕获使能寄存器</td><td>0x0000</td></tr><tr><td>R16_TIM1_CNT</td><td>0x40012C24</td><td>计数器</td><td>0x0000</td></tr><tr><td>R16_TIM1_PSC</td><td>0x40012C28</td><td>计数时钟预分频器</td><td>0x0000</td></tr><tr><td>R16_TIM1_ATRLR</td><td>0x40012C2C</td><td>自动重装值寄存器</td><td>0x0000</td></tr><tr><td>R16_TIM1_RPTCR</td><td>0x40012C30</td><td>重复计数值寄存器</td><td>0x0000</td></tr><tr><td>R16_TIM1_CH1CVR</td><td>0x40012C34</td><td>比较/捕获寄存器1</td><td>0x0000</td></tr><tr><td>R16_TIM1_CH2CVR</td><td>0x40012C38</td><td>比较/捕获寄存器2</td><td>0x0000</td></tr><tr><td>R16_TIM1_CH3CVR</td><td>0x40012C3C</td><td>比较/捕获寄存器3</td><td>0x0000</td></tr><tr><td>R16_TIM1_CH4CVR</td><td>0x40012C40</td><td>比较/捕获寄存器4</td><td>0x0000</td></tr><tr><td>R16_TIM1_BDTR</td><td>0x40012C44</td><td>刹车和死区寄存器</td><td>0x0000</td></tr><tr><td>R16_TIM1_DMACFGR</td><td>0x40012C48</td><td>DMA控制寄存器</td><td>0x0000</td></tr><tr><td>R16_TIM1_DMAADR</td><td>0x40012C4C</td><td>连续模式的DMA地址寄存器</td><td>0x0000</td></tr></table>

表 14-4 TIM8 相关寄存器列表  

<table><tr><td>名称</td><td>访问地址</td><td>描述</td><td>复位值</td></tr><tr><td>R16_TIM8_CTLR1</td><td>0x40013400</td><td>控制寄存器1</td><td>0x0000</td></tr><tr><td>R16_TIM8_CTLR2</td><td>0x40013404</td><td>控制寄存器2</td><td>0x0000</td></tr><tr><td>R16_TIM8_SMCFGR</td><td>0x40013408</td><td>从模式控制寄存器</td><td>0x0000</td></tr><tr><td>R16_TIM8_DMAINTENR</td><td>0x4001340C</td><td>DMA/中断使能寄存器</td><td>0x0000</td></tr><tr><td>R16_TIM8_INTERR</td><td>0x40013410</td><td>中断状态寄存器</td><td>0x0000</td></tr><tr><td>R16_TIM8_SWEVGR</td><td>0x40013414</td><td>事件产生寄存器</td><td>0x0000</td></tr><tr><td>R16_TIM8_CHCTLR1</td><td>0x40013418</td><td>比较/捕获控制寄存器1</td><td>0x0000</td></tr><tr><td>R16_TIM8_CHCTLR2</td><td>0x4001341C</td><td>比较/捕获控制寄存器2</td><td>0x0000</td></tr><tr><td>R16_TIM8_CCER</td><td>0x40013420</td><td>比较/捕获使能寄存器</td><td>0x0000</td></tr><tr><td>R16_TIM8_CNT</td><td>0x40013424</td><td>计数器</td><td>0x0000</td></tr><tr><td>R16_TIM8_PSC</td><td>0x40013428</td><td>计数时钟预分频器</td><td>0x0000</td></tr><tr><td>R16_TIM8_ATRLR</td><td>0x4001342C</td><td>自动重装值寄存器</td><td>0x0000</td></tr><tr><td>R16_TIM8_RPTCR</td><td>0x40013430</td><td>重复计数值寄存器</td><td>0x0000</td></tr><tr><td>R16_TIM8_CH1CVR</td><td>0x40013434</td><td>比较/捕获寄存器1</td><td>0x0000</td></tr><tr><td>R16_TIM8_CH2CVR</td><td>0x40013438</td><td>比较/捕获寄存器2</td><td>0x0000</td></tr><tr><td>R16_TIM8_CH3CVR</td><td>0x4001343C</td><td>比较/捕获寄存器3</td><td>0x0000</td></tr><tr><td>R16_TIM8_CH4CVR</td><td>0x40013440</td><td>比较/捕获寄存器4</td><td>0x0000</td></tr><tr><td>R16_TIM8_BDTR</td><td>0x40013444</td><td>刹车和死区寄存器</td><td>0x0000</td></tr><tr><td>R16_TIM8_DMACFGR</td><td>0x40013448</td><td>DMA控制寄存器</td><td>0x0000</td></tr><tr><td>R16_TIM8_DMAADR</td><td>0x4001344C</td><td>连续模式的DMA地址寄存器</td><td>0x0000</td></tr></table>

表 14-5 TIM9 相关寄存器列表  

<table><tr><td>名称</td><td>访问地址</td><td>描述</td><td>复位值</td></tr><tr><td>R16_TIM9_CTLR1</td><td>0x40014C00</td><td>控制寄存器1</td><td>0x0000</td></tr><tr><td>R16_TIM9_CTLR2</td><td>0x40014C04</td><td>控制寄存器2</td><td>0x0000</td></tr><tr><td>R16_TIM9_SMCFGR</td><td>0x40014C08</td><td>从模式控制寄存器</td><td>0x0000</td></tr><tr><td>R16_TIM9_DMAINTENR</td><td>0x40014C0C</td><td>DMA/中断使能寄存器</td><td>0x0000</td></tr><tr><td>R16_TIM9_INTFR</td><td>0x40014C10</td><td>中断状态寄存器</td><td>0x0000</td></tr><tr><td>R16_TIM9_SWEVGR</td><td>0x40014C14</td><td>事件产生寄存器</td><td>0x0000</td></tr><tr><td>R16_TIM9_CHCTLR1</td><td>0x40014C18</td><td>比较/捕获控制寄存器1</td><td>0x0000</td></tr><tr><td>R16_TIM9_CHCTLR2</td><td>0x40014C1C</td><td>比较/捕获控制寄存器2</td><td>0x0000</td></tr><tr><td>R16_TIM9_CCER</td><td>0x40014C20</td><td>比较/捕获使能寄存器</td><td>0x0000</td></tr><tr><td>R16_TIM9_CNT</td><td>0x40014C24</td><td>计数器</td><td>0x0000</td></tr><tr><td>R16_TIM9_PSC</td><td>0x40014C28</td><td>计数时钟预分频器</td><td>0x0000</td></tr><tr><td>R16_TIM9_ATRLR</td><td>0x40014C2C</td><td>自动重装值寄存器</td><td>0x0000</td></tr><tr><td>R16_TIM9_RPTCR</td><td>0x40014C30</td><td>重复计数值寄存器</td><td>0x0000</td></tr><tr><td>R16_TIM9_CH1CVR</td><td>0x40014C34</td><td>比较/捕获寄存器1</td><td>0x0000</td></tr><tr><td>R16_TIM9_CH2CVR</td><td>0x40014C38</td><td>比较/捕获寄存器2</td><td>0x0000</td></tr><tr><td>R16_TIM9_CH3CVR</td><td>0x40014C3C</td><td>比较/捕获寄存器3</td><td>0x0000</td></tr><tr><td>R16_TIM9_CH4CVR</td><td>0x40014C40</td><td>比较/捕获寄存器4</td><td>0x0000</td></tr><tr><td>R16_TIM9_BDTR</td><td>0x40014C44</td><td>刹车和死区寄存器</td><td>0x0000</td></tr><tr><td>R16_TIM9_DMACFGR</td><td>0x40014C48</td><td>DMA控制寄存器</td><td>0x0000</td></tr><tr><td>R16_TIM9_DMAADR</td><td>0x40014C4C</td><td>连续模式的DMA地址寄存器</td><td>0x0000</td></tr></table>

表 14-6 TIM10 相关寄存器列表  

<table><tr><td>名称</td><td>访问地址</td><td>描述</td><td>复位值</td></tr><tr><td>R16_TIM10_CTLR1</td><td>0x40015000</td><td>控制寄存器1</td><td>0x0000</td></tr><tr><td>R16_TIM10_CTLR2</td><td>0x40015004</td><td>控制寄存器2</td><td>0x0000</td></tr><tr><td>R16_TIM10_SMCFGR</td><td>0x40015008</td><td>从模式控制寄存器</td><td>0x0000</td></tr><tr><td>R16_TIM10_DMAINTENR</td><td>0x4001500C</td><td>DMA/中断使能寄存器</td><td>0x0000</td></tr><tr><td>R16_TIM10_INTFR</td><td>0x40015010</td><td>中断状态寄存器</td><td>0x0000</td></tr><tr><td>R16_TIM10_SWEVGR</td><td>0x40015014</td><td>事件产生寄存器</td><td>0x0000</td></tr><tr><td>R16_TIM10_CHCTLR1</td><td>0x40015018</td><td>比较/捕获控制寄存器1</td><td>0x0000</td></tr><tr><td>R16_TIM10_CHCTLR2</td><td>0x4001501C</td><td>比较/捕获控制寄存器2</td><td>0x0000</td></tr><tr><td>R16_TIM10_CCER</td><td>0x40015020</td><td>比较/捕获使能寄存器</td><td>0x0000</td></tr><tr><td>R16_TIM10_CNT</td><td>0x40015024</td><td>计数器</td><td>0x0000</td></tr><tr><td>R16_TIM10_PSC</td><td>0x40015028</td><td>计数时钟预分频器</td><td>0x0000</td></tr><tr><td>R16_TIM10_ATRLR</td><td>0x4001502C</td><td>自动重装值寄存器</td><td>0x0000</td></tr><tr><td>R16_TIM10_RPTCR</td><td>0x40015030</td><td>重复计数值寄存器</td><td>0x0000</td></tr><tr><td>R16_TIM10_CH1CVR</td><td>0x40015034</td><td>比较/捕获寄存器1</td><td>0x0000</td></tr><tr><td>R16_TIM10_CH2CVR</td><td>0x40015038</td><td>比较/捕获寄存器2</td><td>0x0000</td></tr><tr><td>R16_TIM10_CH3CVR</td><td>0x4001503C</td><td>比较/捕获寄存器3</td><td>0x0000</td></tr><tr><td>R16_TIM10_CH4CVR</td><td>0x40015040</td><td>比较/捕获寄存器4</td><td>0x0000</td></tr><tr><td>R16_TIM10_BDTR</td><td>0x40015044</td><td>刹车和死区寄存器</td><td>0x0000</td></tr><tr><td>R16_TIM10_DMACFGR</td><td>0x40015048</td><td>DMA控制寄存器</td><td>0x0000</td></tr><tr><td>R16_TIM10_DMAADR</td><td>0x4001504C</td><td>连续模式的DMA地址寄存器</td><td>0x0000</td></tr></table>

### 14.4.1 控制寄存器 1（TIMx_CTLR1）（x=1/8/9/10）

偏移地址： $0 \times 0 0$

<table><tr><td>Reserved</td><td>CKD[1:0]</td><td>ARPE</td><td>CMS[1:0]</td><td>DIR</td><td>OPM</td><td>URS</td><td>UDIS</td><td>CEN</td></tr></table>

<table><tr><td>位</td><td>名称</td><td>访问</td><td>描述</td><td>复位值</td></tr><tr><td>[15:10]</td><td>Reserved</td><td>R0</td><td>保留。</td><td>0</td></tr><tr><td>[9:8]</td><td>CKD[1:0]</td><td>RW</td><td>这2位定义在定时器时钟(CK_INT)频率、死区时间和由死区发生器与数字滤波器(ETR, T1x)所用的采样时钟之间的分频比例:00: Tdts=Tck_int01: Tdts=2xTck_int10: Tdts=4xTck_int11: 保留。</td><td>00b</td></tr><tr><td>7</td><td>ARPE</td><td>RW</td><td>自动重装预装使能位:1: 使能自动重装值寄存器(ATRLR);0: 禁止自动重装值寄存器(ATRLR)。</td><td>0</td></tr><tr><td>[6:5]</td><td>CMS[1:0]</td><td>RW</td><td>中央对齐模式选择:00: 边沿对齐模式。计数器依据方向位(DIR)向上或向下计数。01: 中央对齐模式1。计数器交替地向上和向下计数。配置为输出的通道(CHCTLRx 寄存器中CCxS=00)的输出比较中断标志位,只在计数器向下计数时被设置。10: 中央对齐模式2。计数器交替地向上和向下计数。配置为输出的通道(CHCTLRx 寄存器中CCxS=00)的输出比较中断标志位,只在计数器向上计数时被设置。11: 中央对齐模式3。计数器交替地向上和向下计数。配置为输出的通道(CHCTLRx 寄存器中CCxS=00)的输出比较中断标志位,在计数器向上和向下计数时均被设置。注:在计数器使能时(CEN=1),不允许从边沿对齐模式转换到中央对齐模式。</td><td>00b</td></tr><tr><td>4</td><td>DIR</td><td>RW</td><td>计数器方向:0: 计数器的计数模式为增计数;1: 计数器的计数模式为减计数。注:当计数器配置为中央对齐模式或编码器模式时,该位无效。</td><td>0</td></tr><tr><td>3</td><td>OPM</td><td>RW</td><td>单脉冲模式:1: 在发生下一次更新事件(清除CEN位)时,计数器停止。0: 在发生下一次更新事件时,计数器不停止。</td><td>0</td></tr><tr><td>2</td><td>URS</td><td>RW</td><td>更新请求源,软件通过该位选择UEV事件的源。1: 如果使能了更新中断或DMA请求,则只有计数器溢出/下溢才产生更新中断或DMA请求;0: 如果使能了更新中断或DMA请求,则下述任一事件产生更新中断或DMA请求。-计数器溢出/下溢</td><td>0</td></tr><tr><td></td><td></td><td></td><td>-设置UG位
-从模式控制器产生的更新</td><td></td></tr><tr><td>1</td><td>UDIS</td><td>RW</td><td>禁止更新，软件通过该位允许/禁止UEV事件的产生。
1:禁止UEV。不产生更新事件,各寄存器(ARR、PSC、CCRx)保持它们的值。如果设置了UG位或从模式控制器发出了一个硬件复位,则计数器和预分频器被重新初始化;
0:允许UEV。更新(UEV)事件由下述任一事件产生:
-计数器溢出/下溢
-设置UG位
-从模式控制器产生的更新
具有缓存的寄存器被装入它们的预装载值。</td><td>0</td></tr><tr><td>0</td><td>CEN</td><td>RW</td><td>使能计数器。
1:使能计数器;
0:禁止计数器。
注:在软件设置了CEN位后,外部时钟、门控模式和编码器模式才能工作。触发模式可以自动地通过硬件设置CEN位。</td><td>0</td></tr></table>

### 14.4.2 控制寄存器 2（TIMx_CTLR2）（x=1/8/9/10）

偏移地址： $0 \times 0 4$

<table><tr><td>15</td><td>14</td><td>13</td><td>12</td><td>11</td><td>10</td><td>9</td><td>8</td><td>7</td><td>6</td><td>5</td><td>4</td><td>3</td><td>2</td><td>1</td><td>0</td></tr><tr><td>Reserved</td><td>01S4</td><td>01S3N</td><td>01S3</td><td>01S2N</td><td>01S2</td><td>01S2</td><td>01S1N</td><td>01S1</td><td>TI1S</td><td colspan="2">MMS[2:0]</td><td>CCDS</td><td>CCUS</td><td>Reserved</td><td>CCPC</td></tr></table>

<table><tr><td>位</td><td>名称</td><td>访问</td><td>描述</td><td>复位值</td></tr><tr><td>15</td><td>Reserved</td><td>R0</td><td>保留。</td><td>0</td></tr><tr><td>14</td><td>01S4</td><td>RW</td><td>输出空闲状态4:1:当MOE=0时,如果实施了0C4N,则死区后0C1=1;0:当MOE=0时,如果实施了0C4N,则死区后0C1=0。注:已经设置了LOCK(TIMx_BDTR寄存器)级别1、2或3后,该位不能被修改。</td><td>0</td></tr><tr><td>13</td><td>01S3N</td><td>RW</td><td>输出空闲状态3:1:当MOE=0时,死区后0C1N=1;0:当MOE=0时,死区后0C1N=0。注:已经设置了LOCK(TIMx_BDTR寄存器)级别1、2或3后,该位不能被修改。</td><td>0</td></tr><tr><td>12</td><td>01S3</td><td>RW</td><td>输出空闲状态3,参见01S4。</td><td>0</td></tr><tr><td>11</td><td>01S2N</td><td>RW</td><td>输出空闲状态2,参见01S3N。</td><td>0</td></tr><tr><td>10</td><td>01S2</td><td>RW</td><td>输出空闲状态2,参见01S4。</td><td>0</td></tr><tr><td>9</td><td>01S1N</td><td>RW</td><td>输出空闲状态1,参见01S3N。</td><td>0</td></tr><tr><td>8</td><td>01S1</td><td>RW</td><td>输出空闲状态1,参见01S4。</td><td>0</td></tr><tr><td>7</td><td>TI1S</td><td>RW</td><td>TI1 选择:1:TIMx_CH1、TIMx_CH2和TIMx_CH3引脚经异或后连到TI1输入;0:TIMx_CH1引脚直连到TI1输入。</td><td>0</td></tr><tr><td>[6:4]</td><td>MMS[2:0]</td><td>RW</td><td>主模式选择:这3位用于选择在主模式下送到从定时器的同步信息(TRGO)。可能的组合如下:000:复位-TIMx_EGR寄存器的UG位被用于作为触发输出(TRGO)。如果是触发输入产生的复位(从模式控制器处于复位模式),则TRGO上的信号相对实际的复位会有一个延迟;001:使能-计数器使能信号CNT_EN被用于作为触发输出(TRGO)。有时需要在同一时间启动多个定时器或控制在一段时间内使能从定时器。计数器使能信号是通过CEN控制位和门控模式下的触发输入信号的逻辑或产生。当计数器使能信号受控于触发输入时,TRGO上会有一个延迟,除非选择了主/从模式(见TIMx_SMCR寄存器中MSM位的描述);010:更新-更新事件被选为触发输入(TRGO)。例如,一个主定时器的时钟可以被用作一个从定时器的预分频器;011:比较脉冲-在发生一次捕获或一次比较成功时,当要设置CC1IF标志时(即使它已经为高),触发输出送出一个正脉冲(TRGO);100:比较-0C1REF信号被用于作为触发输出(TRGO);101:比较-0C2REF信号被用于作为触发输出(TRGO);110:比较-0C3REF信号被用于作为触发输出(TRGO);111:比较-0C4REF信号被用于作为触发输出(TRGO)。</td><td>000b</td></tr><tr><td>3</td><td>CCDS</td><td>RW</td><td>捕获比较的DMA选择。1:当发生更新事件时,送出CHxCVR的DMA请求;0:当发生CHxCVR时,产生CHxCVR的DMA请求。</td><td>0</td></tr><tr><td>2</td><td>CCUS</td><td>RW</td><td>比较捕获控制更新选择位。1:如果CCPC置位,可以通过设置COM位或TRGI上的一个上升沿更新它们;0:如果CCPC置位,只能通过设置COM位更新它们。注:该位只对具有互补输出的通道起作用。</td><td>0</td></tr><tr><td>1</td><td>Reserved</td><td>RO</td><td>保留。</td><td>0</td></tr><tr><td>0</td><td>CCPC</td><td>RW</td><td>比较捕获预装载控制位。1:CCxE,CCxNE和OCxM位是预装载的,设置该位后,它们只在设置了COM位后被更新;</td><td>0</td></tr><tr><td></td><td></td><td></td><td>0: CCxE, CCxNE 和 OCxM 位不是预装载的。
注: 该位只对具有互补输出的通道起作用。</td><td></td></tr></table>

### 14.4.3 从模式控制寄存器（TIMx_SMCFGR）（x=1/8/9/10）

偏移地址： $0 \times 0 8$

15 14 13 12 11 10 9 8 7 6 5 4 3 2 1 0

<table><tr><td>ETP</td><td>ECE</td><td>ETPS[1:0]</td><td>ETF[3:0]</td><td>MSM</td><td>TS[2:0]</td><td>Reserved</td><td>SMS[2:0]</td></tr></table>

<table><tr><td>位</td><td>名称</td><td>访问</td><td>描述</td><td>复位值</td></tr><tr><td>15</td><td>ETP</td><td>R0</td><td>ETR 触发极性选择,该位选择是直接输入 ETR 还是输入 ETR 的反相。1:将 ETR 反相,低电平或下降沿有效;0:ETR,高电平或上升沿有效。</td><td>0</td></tr><tr><td>14</td><td>ECE</td><td>RW</td><td>外部时钟模式 2 启用选择:1:使能外部时钟模式 2;0:禁用外部时钟模式 2。注 1:从模式可以与外部时钟模式 2 同时使用:复位模式,门控模式和触发模式;但是,这时TRGI不能连到 ETRF (TS 位不能是‘111’)。注 2:外部时钟模式 1 和外部时钟模式 2 同时被使能时,外部时钟的输入是 ETRF。</td><td>0</td></tr><tr><td>[13:12]</td><td>ETPS[1:0]</td><td>RW</td><td>外部触发信号 (ETRP) 分频,这个信号频率最大不能超过 TIMxCLK 频率的 1/4,可以通过这个域来降频:00:关闭预分频;01:ETRP 频率除以 2;10:ETRP 频率除以 4;11:ETRP 频率除以 8。</td><td>00b</td></tr><tr><td>[11:8]</td><td>ETF[3:0]</td><td>RW</td><td>外部触发滤波,实际上,数字滤波器是一个事件计数器,它使用一定的采样的频率,记录到 N 个事件后会产生一个输出的跳变。0000:无滤波器,以 Fdt's 采样;0001:采样频率 Fsampling=Fck_int,N=2;0010:采样频率 Fsampling=Fck_int,N=4;0011:采样频率 Fsampling=Fck_int,N=8;0100:采样频率 Fsampling=Fdt's/2,N=6;0101:采样频率 Fsampling=Fdt's/2,N=8;0110:采样频率 Fsampling=Fdt's/4,N=6;0111:采样频率 Fsampling=Fdt's/4,N=8;1000:采样频率 Fsampling=Fdt's/8,N=6;1001:采样频率 Fsampling=Fdt's/8,N=8;1010:采样频率 Fsampling=Fdt's/16,N=5;1011:采样频率 Fsampling=Fdt's/16,N=6;1100:采样频率 F sampling=Fdt's/16,N=8;1101:采样频率 F sampling=Fdt's/32,N=5;1110:采样频率 F sampling=Fdt's/32,N=6;</td><td>0000b</td></tr><tr><td></td><td></td><td></td><td>1111:采样频率Fsampling=Edts/32,N=8。</td><td></td></tr><tr><td>7</td><td>MSM</td><td>RW</td><td>主/从模式选择:1:触发输入(TRGI)上的事件被延迟了,以允许在当前定时器(通过TRGO)与它的从定时器间的完美同步。这对要求把几个定时器同步到一个单一的外部事件时是非常有用的;0:不发挥作用。</td><td>0</td></tr><tr><td>[6:4]</td><td>TS[2:0]</td><td>RW</td><td>触发选择域,这3位选择用于同步计数器的触发输入源:000:内部触发0(ITRO);001:内部触发1(ITR1);010:内部触发2(ITR2);011:内部触发3(ITR3);100:TI1的边沿检测器(TI1F_ED);101:滤波后的定时器输入1(TI1FP1);110:滤波后的定时器输入2(TI2FP2);111:外部触发输入(ETRF);以上只有在SMS为0时改变。注:具体见表14-2。</td><td>000b</td></tr><tr><td>3</td><td>Reserved</td><td>RO</td><td>保留。</td><td>0</td></tr><tr><td>[2:0]</td><td>SMS[2:0]</td><td>RW</td><td>输入模式选择域。选择核心计数器的时钟和触发模式。000:由内部时钟CK_INT驱动;001:编码器模式1,根据TI1FP1的电平,核心计数器在TI2FP2的边沿增减计数;010:编码器模式2,根据TI2FP2的电平,核心计数器在TI1FP1的边沿增减计数;011:编码器模式3,根据另一个信号的输入电平,核心计数器在TI1FP1和TI2FP2的边沿增减计数;100:复位模式,触发输入(TRGI)的上升沿将初始化计数器,并且产生一个更新寄存器的信号;101:门控模式,当触发输入(TRGI)为高时,计数器的时钟开启;在触发输入变为低,计数器停止,计数器的启停都是受控的;110:触发模式,计数器在触发输入TRGI的上升沿启动,只有计数器的启动是受控的;111:外部时钟模式1,选中的触发输入(TRGI)的上升沿驱动计数器。</td><td>000b</td></tr></table>

### 14.4.4 DMA/中断使能寄存器（TIMx_DMAINTENR）（x=1/8/9/10）

偏移地址： $0 \times 0 0$

<table><tr><td>15</td><td>14</td><td>13</td><td>12</td><td>11</td><td>10</td><td>9</td><td>8</td><td>7</td><td>6</td><td>5</td><td>4</td><td>3</td><td>2</td><td>1</td><td>0</td></tr><tr><td>Reserved</td><td>TDE</td><td>COMDE</td><td>CC4DE</td><td>CC3DE</td><td>CC2DE</td><td>CC1DE</td><td>UDE</td><td>BIE</td><td>TIE</td><td>COMIE</td><td>CC4IE</td><td>CC3IE</td><td>CC2IE</td><td>CC1IE</td><td>UIE</td></tr></table>

<table><tr><td>位</td><td>名称</td><td>访问</td><td>描述</td><td>复位值</td></tr><tr><td>15</td><td>Reserved</td><td>RO</td><td>保留。</td><td>0</td></tr><tr><td>14</td><td>TDE</td><td>RW</td><td>触发DMA请求使能位。1:允许触发DMA请求;0:禁止触发DMA请求。</td><td>0</td></tr><tr><td>13</td><td>COMDE</td><td>RW</td><td>COM的DMA请求使能位。1:允许COM的DMA请求;0:禁止COM的DMA请求。</td><td>0</td></tr><tr><td>12</td><td>CC4DE</td><td>RW</td><td>比较捕获通道4的DMA请求使能位。1:允许比较捕获通道4的DMA请求;0:禁止比较捕获通道4的DMA请求。</td><td>0</td></tr><tr><td>11</td><td>CC3DE</td><td>RW</td><td>比较捕获通道3的DMA请求使能位。1:允许比较捕获通道3的DMA请求;0:禁止比较捕获通道3的DMA请求。</td><td>0</td></tr><tr><td>10</td><td>CC2DE</td><td>RW</td><td>比较捕获通道2的DMA请求使能位。1:允许比较捕获通道2的DMA请求;0:禁止比较捕获通道2的DMA请求。</td><td>0</td></tr><tr><td>9</td><td>CC1DE</td><td>RW</td><td>比较捕获通道1的DMA请求使能位。1:允许比较捕获通道1的DMA请求;0:禁止比较捕获通道1的DMA请求。</td><td>0</td></tr><tr><td>8</td><td>UDE</td><td>RW</td><td>更新的DMA请求使能位。1:允许更新的DMA请求;0:禁止更新的DMA请求。</td><td>0</td></tr><tr><td>7</td><td>BIE</td><td>RW</td><td>刹车中断使能位。1:允许刹车中断;0:禁止刹车中断。</td><td>0</td></tr><tr><td>6</td><td>TIE</td><td>RW</td><td>触发中断使能位。1:使能触发中断;0:禁止触发中断。</td><td>0</td></tr><tr><td>5</td><td>COMIE</td><td>RW</td><td>COM中断允许位。1:允许COM中断;0:禁止COM中断。</td><td>0</td></tr><tr><td>4</td><td>CC4IE</td><td>RW</td><td>比较捕获通道4中断使能位。1:允许比较捕获通道4中断;0:禁止比较捕获通道4中断。</td><td>0</td></tr><tr><td>3</td><td>CC3IE</td><td>RW</td><td>比较捕获通道3中断使能位。1:允许比较捕获通道3中断;0:禁止比较捕获通道3中断。</td><td>0</td></tr><tr><td>2</td><td>CC2IE</td><td>RW</td><td>比较捕获通道2中断使能位。1:允许比较捕获通道2中断;0:禁止比较捕获通道2中断。</td><td>0</td></tr><tr><td>1</td><td>CC1IE</td><td>RW</td><td>比较捕获通道1中断使能位。1:允许比较捕获通道1中断;0:禁止比较捕获通道1中断。</td><td>0</td></tr><tr><td>0</td><td>UIE</td><td>RW</td><td>更新中断使能位。</td><td>0</td></tr><tr><td></td><td></td><td></td><td>1: 允许更新中断;
0: 禁止更新中断。</td><td></td></tr></table>

### 14.4.5 中断状态寄存器（TIMx_INTFR）（x=1/8/9/10）

偏移地址： $0 \times 1 0$

15 14 13 12 11 10 9 8 7 6 5 4 3 2 1 0

<table><tr><td>Reserved</td><td>CC40F</td><td>CC30F</td><td>CC20F</td><td>CC10F</td><td>Reserved</td><td>BIF</td><td>TIF</td><td>COMIF</td><td>CC4IF</td><td>CC3IF</td><td>CC2IF</td><td>CC1IF</td><td>UIF</td></tr></table>

<table><tr><td>位</td><td>名称</td><td>访问</td><td>描述</td><td>复位值</td></tr><tr><td>[15:13]</td><td>Reserved</td><td>R0</td><td>保留。</td><td>0</td></tr><tr><td>12</td><td>CC40F</td><td>RWO</td><td>比较捕获通道4重复捕获标志位。</td><td>0</td></tr><tr><td>11</td><td>CC30F</td><td>RWO</td><td>比较捕获通道3重复捕获标志位。</td><td>0</td></tr><tr><td>10</td><td>CC20F</td><td>RWO</td><td>比较捕获通道2重复捕获标志位。</td><td>0</td></tr><tr><td>9</td><td>CC10F</td><td>RWO</td><td>比较捕获通道1重复捕获标志位,仅用于比较捕获通道被配置为输入捕获模式时。该标记由硬件置位,软件写0可清除此位。1:计数器的值被捕获到捕获比较寄存器时,CC1IF的状态已经被置位;0:无重复捕获产生。</td><td>0</td></tr><tr><td>8</td><td>Reserved</td><td>R0</td><td>保留。</td><td>0</td></tr><tr><td>7</td><td>BIF</td><td>RWO</td><td>刹车中断标志位,一旦刹车输入有效,由硬件对该位置位,可由软件清零。1:刹车引脚输入上检测到设定的有效电平;0:无刹车事件产生。</td><td>0</td></tr><tr><td>6</td><td>TIF</td><td>RWO</td><td>触发器中断标志位,当发生触发事件时由硬件对该位置位,由软件清零。触发事件包括从除门控模式外的其它模式时,在TRGI输入端检测到有效边沿,或门控模式下的任一边沿。1:触发器事件产生;0:无触发器事件产生。</td><td>0</td></tr><tr><td>5</td><td>COMIF</td><td>RWO</td><td>COM中断标志位,一旦产生COM事件,该位由硬件置位,由软件清零。COM事件包括CCxE、CCxNE、OCxM被更新。1:COM事件产生;0:无COM事件产生。</td><td>0</td></tr><tr><td>4</td><td>CC4IF</td><td>RWO</td><td>比较捕获通道4中断标志位。</td><td>0</td></tr><tr><td>3</td><td>CC3IF</td><td>RWO</td><td>比较捕获通道3中断标志位。</td><td>0</td></tr><tr><td>2</td><td>CC2IF</td><td>RWO</td><td>比较捕获通道2中断标志位。</td><td>0</td></tr><tr><td>1</td><td>CC1IF</td><td>RWO</td><td>比较捕获通道1中断标志位。如果比较捕获通道配置为输出模式:当计数器值与比较值匹配时该位由硬件置位,但在中心对称模式下除外。该位由软件清零。1:核心计数器的值与比较捕获寄存器1的值匹配;0:无匹配发生。如果比较捕获通道1配置为输入模式:</td><td>0</td></tr><tr><td></td><td></td><td></td><td>当捕获事件发生时该位由硬件置位,它由软件清零或通过读比较捕获寄存器清零。1:计数器值已被捕获比较捕获寄存器1;0:无输入捕获产生。</td><td></td></tr><tr><td>0</td><td>UIF</td><td>RWO</td><td>更新中断标志位,当产生更新事件时该位由硬件置位,由软件清零。1:更新中断产生;0:无更新事件产生。以下情形会产生更新事件:若UDIS=0,当重复计数器数值上溢或下溢时;若URS=0、UDIS=0,当置UG位时,或当通过软件对计数器核心计数器重新初始化时;若URS=0、UDIS=0,当计数器CNT被触发事件重新初始化时;</td><td>0</td></tr></table>

### 14.4.6 事件产生寄存器（TIMx_SWEVGR）（x=1/8/9/10）

偏移地址：0x14

<table><tr><td>15</td><td>14</td><td>13</td><td>12</td><td>11</td><td>10</td><td>9</td><td>8</td><td>7</td><td>6</td><td>5</td><td>4</td><td>3</td><td>2</td><td>1</td><td>0</td></tr><tr><td colspan="8">Reserved</td><td>BG</td><td>TG</td><td>COMG</td><td>CC4G</td><td>CC3G</td><td>CC2G</td><td>CC1G</td><td>UG</td></tr></table>

<table><tr><td>位</td><td>名称</td><td>访问</td><td>描述</td><td>复位值</td></tr><tr><td>[15:8]</td><td>Reserved</td><td>RO</td><td>保留。</td><td>0</td></tr><tr><td>7</td><td>BG</td><td>WO</td><td>刹车事件产生位,此位由软件置位和清零,用来产生一个刹车事件。1:产生一个刹车事件。此时MOE=0、BIF=1,若使能对应的中断和DMA,则产生相应的中断和DMA;0:无动作。</td><td>0</td></tr><tr><td>6</td><td>TG</td><td>WO</td><td>触发事件产生位,该位由软件置位,硬件清零,用于产生一个触发事件。1:产生一个触发事件,TIF被置位,若使能对应的中断和DMA,则产生相应的中断和DMA;0:无动作。</td><td>0</td></tr><tr><td>5</td><td>COMG</td><td>WO</td><td>比较捕获控制更新产生位。产生比较捕获控制更新事件。该位由软件置位,由硬件自动清零。1:当CCPC=1,允许更新CCxE、CCxNE、OCxM位;0:无动作。注:该位只对拥有互补输出的通道(通道1,2,3)有效。</td><td>0</td></tr><tr><td>4</td><td>CC4G</td><td>WO</td><td>比较捕获事件产生位4。产生比较捕获事件4。</td><td>0</td></tr><tr><td>3</td><td>CC3G</td><td>WO</td><td>比较捕获事件产生位3。产生比较捕获事件3。</td><td>0</td></tr><tr><td>2</td><td>CC2G</td><td>WO</td><td>比较捕获事件产生位2。产生比较捕获事件2。</td><td>0</td></tr><tr><td>1</td><td>CC1G</td><td>WO</td><td>比较捕获事件产生位1,产生比较捕获事件1。该位由软件置位,由硬件清零。用于产生一个比较捕获事件。1:在比较捕获通道1上产生一个比较捕获事件:</td><td>0</td></tr><tr><td></td><td></td><td></td><td>若比较捕获通道1配置为输出:置CC1IF位。若使能对应的中断和DMA,则产生相应的中断和DMA;若比较捕获通道1配置为输入:当前核心计数器的值被捕获至比较捕获寄存器1;置CC1IF位,若使能了对应的中断和DMA,则产生相应的中断和DMA。若CC1IF已经置位,则置CC1OF位。0:无动作。</td><td></td></tr><tr><td>0</td><td>UG</td><td>WO</td><td>更新事件产生位,产生更新事件。该位由软件置位,由硬件自动清零。1:初始化计数器,并产生一个更新事件;0:无动作。注:预分频器的计数器也被清零,但是预分频系数不变。若在中心对称模式下或增计数模式下则核心计数器被清零;若减计数模式下则核心计数器取重装值寄存器的值。</td><td>0</td></tr></table>

### 14.4.7 比较/捕获控制寄存器 1（TIMx_CHCTLR1）（x=1/8/9/10）

偏移地址： $0 \times 1 8$

通道可用于输入(捕获模式)或输出(比较模式)，通道的方向由相应的 $\mathtt { C C } \mathtt { x } \mathtt { S }$ 位定义。该寄存器其它位的作用在输入和输出模式下不同。 ${ 0 0 } \times \times$ 描述了通道在输出模式下的功能， $1 0 \times \mathsf { x }$ 描述了通道在输入模式下的功能。

<table><tr><td>15</td><td>14</td><td>13</td><td>12</td><td>11</td><td>10</td><td>9</td><td>8</td><td>7</td><td>6</td><td>5</td><td>4</td><td>3</td><td>2</td><td>1</td><td>0</td></tr><tr><td>OC2CE</td><td colspan="3">OC2M[2:0]</td><td>OC2PE</td><td>OC2FE</td><td rowspan="2" colspan="2">CC2S[1:0]</td><td>OC1CE</td><td colspan="3">OC1M[2:0]</td><td>OC1PE</td><td>OC1FE</td><td rowspan="2" colspan="2">CC1S[1:0]</td></tr><tr><td colspan="4">IC2F[3:0]</td><td colspan="2">IC2PSC[1:0]</td><td colspan="4">IC1F[3:0]</td><td colspan="2">IC1PSC[1:0]</td></tr></table>

比较模式（引脚方向为输出）：

<table><tr><td>位</td><td>名称</td><td>访问</td><td>描述</td><td>复位值</td></tr><tr><td>15</td><td>OC2CE</td><td>RW</td><td>比较捕获通道2清零使能位。1:一旦检测到ETRF输入高电平,清除OC2REF位零;0:OC2REF不受ETRF输入的影响。</td><td>0</td></tr><tr><td>[14:12]</td><td>OC2M[2:0]</td><td>RW</td><td>比较捕获通道2模式设置域。该3位定义了输出参考信号OC2REF的动作,而OC2REF决定了OC2、OC2N的值。OC2REF是高电平有效,而OC2和OC2N的有效电平取决于CC2P、CC2NP位。000:冻结。比较捕获寄存器的值与核心计数器间的比较值对OC1REF不起作用;001:强制设为有效电平。当核心计数器与比较捕获寄存器1的值相同时,强制OC1REF为高;010:强制设为无效电平。当核心计数器的值与比较捕获寄存器1相同时,强制OC1REF为低;011:翻转。当核心计数器与比较捕获寄存器1的值相同时,翻转OC1REF的电平。</td><td>000b</td></tr><tr><td></td><td></td><td></td><td>100:强制为无效电平。强制OC1REF为低。101:强制为有效电平。强制OC1REF为高。110:PWM模式1:在向上计数时,一旦核心计数器大于比较捕获寄存器的值时通道1为无效电平,否则为有效电平;在向下计数时,一旦核心计数器大于比较捕获寄存器的值时通道1为有效电平,否则为无效电平。111:PWM模式2:在向上计数时,一旦核心计数器大于比较捕获寄存器的值时,通道1为有效电平,否则为无效电平;在向下计数时,一旦核心计数器大于比较捕获寄存器的值时,通道1为无效电平,否则为有效电平(OC1REF=1)。注:一旦LOCK级别设为3并且CC1S=00b则该位不能被修改。在PWM模式1或PWM模式2中,只有当比较结果改变了或在输出比较模式中从冻结模式切换到PWM模式时,OC1REF电平才改变。</td><td></td></tr><tr><td>11</td><td>OC2PE</td><td>RW</td><td>比较捕获寄存器2预装载使能位。1:开启比较捕获寄存器2的预装载功能,读写操作仅对预装载寄存器操作,比较捕获寄存器2的预装载值在更新事件到来时被加载至当前影子寄存器中;0:禁止比较捕获寄存器2的预装载功能,可随时写入比较捕获寄存器2,并且新写入的数值立即起作用。注:一旦LOCK级别设为3并且CC1S=00,则该位不能被修改;仅仅在单脉冲模式下(OPM=1)可以在未确认预装载寄存器情况下使用PWM模式,否则其动作不确定。</td><td>0</td></tr><tr><td>10</td><td>OC2FE</td><td>RW</td><td>比较捕获通道2快速使能位,该位用于加快比较捕获通道输出对触发输入事件的响应。1:输入到触发器的有效沿的作用就像发生了一次比较匹配。因此,OC被设置为比较电平而与比较结果无关。采样触发器的有效沿和比较捕获通道2输出间的延时被缩短为3个时钟周期;0:根据计数器与比较捕获寄存器2的值,比较捕获通道2正常操作,即使触发器是打开的。当触发器的输入有一个有效沿时,激活比较捕获通道2输出的最小延时为5个时钟周期。OC2FE只在通道被配置成PWM1或PWM2模式时起作用。</td><td>0</td></tr><tr><td>[9:8]</td><td>CC2S[1:0]</td><td>RW</td><td>比较捕获通道2输入选择域。00:比较捕获通道2被配置为输出;01:比较捕获通道2被配置为输入,IC2映射在TI2上;10:比较捕获通道2被配置为输入,IC2映射在TI1上;</td><td>00b</td></tr><tr><td></td><td></td><td></td><td>11:比较捕获通道2被配置为输入,IC2映射在TRC上。此模式仅工作在内部触发器输入被选中时(由TS位选择)。注:比较捕获通道2仅在通道关闭时(CC2E为零时)才是可写的。</td><td></td></tr><tr><td>7</td><td>OC1CE</td><td>RW</td><td>比较捕获通道1清零使能位。</td><td>0</td></tr><tr><td>[6:4]</td><td>OC1M[2:0]</td><td>RW</td><td>比较捕获通道1模式设置域。</td><td>0</td></tr><tr><td>3</td><td>OC1PE</td><td>RW</td><td>比较捕获寄存器1预装载使能位。</td><td>0</td></tr><tr><td>2</td><td>OC1FE</td><td>RW</td><td>比较捕获通道1快速使能位。</td><td>0</td></tr><tr><td>[1:0]</td><td>CC1S[1:0]</td><td>RW</td><td>比较捕获通道1输入选择域。</td><td>0</td></tr></table>

捕获模式（引脚方向为输入）：  

<table><tr><td>位</td><td>名称</td><td>访问</td><td>描述</td><td>复位值</td></tr><tr><td>[15:12]</td><td>IC2F[3:0]</td><td>RW</td><td>输入捕获滤波器2配置域,这几位设置了TI1输入的采样频率及数字滤波器长度。数字滤波器由一个事件计数器组成,它记录到N个事件后会产生一个输出的跳变。0000:无滤波器,以fDTS采样;1000:采样频率Fsampling=Fdts/8,N=6;0001:采样频率Fsampling=Fck_int,N=2;1001:采样频率Fsampling=Fdts/8,N=8;0010:采样频率Fsampling=Fck_int,N=4;1010:采样频率Fsampling=Fdts/16,N=5;0011:采样频率Fsampling=f=Fck_int,N=8;1011:采样频率Fsampling=Fdts/16,N=6;0100:采样频率Fsampling=Fdts/2,N=6;1100:采样频率Fsampling=Fdts/16,N=8;0101:采样频率Fsampling=Fdts/2,N=8;1101:采样频率Fsampling=Fdts/32,N=5;0110:采样频率Fsampling=Fdts/4,N=6;1110:采样频率Fsampling=Fdts/32,N=6;0111:采样频率Fsampling=Fdts/4,N=8;1111:采样频率Fsampling=Fdts/32,N=8。</td><td>0000b</td></tr><tr><td>[11:10]</td><td>IC2PSC[1:0]</td><td>RW</td><td>比较捕获通道2预分频配置域,这2位定义了比较捕获通道2的预分频系数。一旦CC1E=0,则预分频器复位。00:无预分频器,捕获输入口上检测到的每一个边沿都触发一次捕获;01:每2个事件触发一次捕获;10:每4个事件触发一次捕获;11:每8个事件触发一次捕获。</td><td>00b</td></tr><tr><td>[9:8]</td><td>CC2S[1:0]</td><td>RW</td><td>比较捕获通道2输入选择域,这2位定义通道的方向(输入/输出),及输入脚的选择。00:比较捕获通道1通道被配置为输出;01:比较捕获通道1通道被配置为输入,IC1映射</td><td>00b</td></tr><tr><td></td><td></td><td></td><td>在 TI1 上;10: 比较捕获通道 1 通道被配置为输入, IC1 映射在 TI2 上;11: 比较捕获通道 1 通道被配置为输入, IC1 映射在 TRC 上。此模式仅工作在内部触发器输入被选中时(由 TS 位选择)。注: CC1S 仅在通道关闭时 (CC1E 为 0) 才是可写的。</td><td></td></tr><tr><td>[7:4]</td><td>IC1F[3:0]</td><td>RW</td><td>输入捕获滤波器 1 配置域。</td><td>0</td></tr><tr><td>[3:2]</td><td>IC1PSC[1:0]</td><td>RW</td><td>比较捕获通道 1 预分频配置域。</td><td>0</td></tr><tr><td>[1:0]</td><td>CC1S[1:0]</td><td>RW</td><td>比较捕获通道 1 输入选择域。</td><td>0</td></tr></table>

### 14.4.8 比较/捕获控制寄存器 2（TIMx_CHCTLR2）（x=1/8/9/10）

偏移地址： ${ 0 } { \times 1 } { 0 }$

通道可用于输入(捕获模式)或输出(比较模式)，通道的方向由相应的 $\mathtt { C C } \mathtt { x } \mathtt { S }$ 位定义。该寄存器其它位的作用在输入和输出模式下不同。 ${ 0 0 } \times \times$ 描述了通道在输出模式下的功能， $1 0 \times \mathsf { x }$ 描述了通道在输入模式下的功能。

<table><tr><td>15</td><td>14</td><td>13</td><td>12</td><td>11</td><td>10</td><td>9</td><td>8</td><td>7</td><td>6</td><td>5</td><td>4</td><td>3</td><td>2</td><td>1</td><td>0</td></tr><tr><td>OC4CE</td><td colspan="2">OC4M[2:0]</td><td>OC4PE</td><td>OC4FE</td><td rowspan="2" colspan="2">CC4S[1:0]</td><td>OC3CE</td><td colspan="2">OC3M[2:0]</td><td>OC3PE</td><td colspan="2">OC3FE</td><td rowspan="2" colspan="2">CC3S[1:0]</td><td></td></tr><tr><td colspan="3">IC4F[3:0]</td><td colspan="2">IC4PSC[1:0]</td><td colspan="3">IC3F[3:0]</td><td colspan="3">IC3PSC[1:0]</td><td></td></tr></table>

比较模式（引脚方向为输出）：

<table><tr><td>位</td><td>名称</td><td>访问</td><td>描述</td><td>复位值</td></tr><tr><td>15</td><td>OC4CE</td><td>RW</td><td>比较捕获通道4清零使能位。</td><td>0</td></tr><tr><td>[14:12]</td><td>OC4M[2:0]</td><td>RW</td><td>比较捕获通道4模式设置域。</td><td>0</td></tr><tr><td>11</td><td>OC4PE</td><td>RW</td><td>比较捕获寄存器4预装载使能位。</td><td>0</td></tr><tr><td>10</td><td>OC4FE</td><td>RW</td><td>比较捕获通道4快速使能位。</td><td>0</td></tr><tr><td>[9:8]</td><td>CC4S[1:0]</td><td>RW</td><td>比较捕获通道4输入选择域。</td><td>0</td></tr><tr><td>7</td><td>OC3CE</td><td>RW</td><td>比较捕获通道3清零使能位。</td><td>0</td></tr><tr><td>[6:4]</td><td>OC3M[2:0]</td><td>RW</td><td>比较捕获通道3模式设置域。</td><td>0</td></tr><tr><td>3</td><td>OC3PE</td><td>RW</td><td>比较捕获寄存器3预装载使能位。</td><td>0</td></tr><tr><td>2</td><td>OC3FE</td><td>RW</td><td>比较捕获通道3快速使能位。</td><td>0</td></tr><tr><td>[1:0]</td><td>CC3S[1:0]</td><td>RW</td><td>比较捕获通道3输入选择域。</td><td>0</td></tr></table>

捕获模式（引脚方向为输入）：

<table><tr><td>位</td><td>名称</td><td>访问</td><td>描述</td><td>复位值</td></tr><tr><td>[15:12]</td><td>IC4F[3:0]</td><td>RW</td><td>输入捕获滤波器4配置域。</td><td>0</td></tr><tr><td>[11:10]</td><td>IC4PSC[1:0]</td><td>RW</td><td>比较捕获通道4预分频配置域。</td><td>0</td></tr><tr><td>[9:8]</td><td>CC4S[1:0]</td><td>RW</td><td>比较捕获通道4输入选择域。</td><td>0</td></tr><tr><td>[7:4]</td><td>IC3F[3:0]</td><td>RW</td><td>输入捕获滤波器3配置域。</td><td>0</td></tr><tr><td>[3:2]</td><td>IC3PSC[1:0]</td><td>RW</td><td>比较捕获通道3预分频配置域。</td><td>0</td></tr><tr><td>[1:0]</td><td>CC3S[1:0]</td><td>RW</td><td>比较捕获通道3输入选择域。</td><td>0</td></tr></table>

### 14.4.9 比较/捕获使能寄存器（TIMx_CCER）（x=1/8/9/10）

偏移地址： $_ { 0 \times 2 0 }$

<table><tr><td>Reserved</td><td>CC4P</td><td>CC4E</td><td>CC3NP</td><td>CC3NE</td><td>CC3P</td><td>CC3E</td><td>CC2NP</td><td>CC2NE</td><td>CC2P</td><td>CC2E</td><td>CC1NP</td><td>CC1NE</td><td>CC1P</td><td>CC1E</td></tr></table>

<table><tr><td>位</td><td>名称</td><td>访问</td><td>描述</td><td>复位值</td></tr><tr><td>[15:14]</td><td>Reserved</td><td>R0</td><td>保留。</td><td>0</td></tr><tr><td>13</td><td>CC4P</td><td>RW</td><td>比较捕获通道4输出极性设置位。</td><td>0</td></tr><tr><td>12</td><td>CC4E</td><td>RW</td><td>比较捕获通道4输出使能位。</td><td>0</td></tr><tr><td>11</td><td>CC3NP</td><td>RW</td><td>比较捕获通道3互补输出极性设置位。</td><td>0</td></tr><tr><td>10</td><td>CC3NE</td><td>RW</td><td>比较捕获通道3互补输出使能位。</td><td>0</td></tr><tr><td>9</td><td>CC3P</td><td>RW</td><td>比较捕获通道3输出极性设置位。</td><td>0</td></tr><tr><td>8</td><td>CC3E</td><td>RW</td><td>比较捕获通道3输出使能位。</td><td>0</td></tr><tr><td>7</td><td>CC2NP</td><td>RW</td><td>比较捕获通道2互补输出极性设置位。</td><td>0</td></tr><tr><td>6</td><td>CC2NE</td><td>RW</td><td>比较捕获通道2互补输出使能位。</td><td>0</td></tr><tr><td>5</td><td>CC2P</td><td>RW</td><td>比较捕获通道2输出极性设置位。</td><td>0</td></tr><tr><td>4</td><td>CC2E</td><td>RW</td><td>比较捕获通道2输出使能位。</td><td>0</td></tr><tr><td>3</td><td>CC1NP</td><td>RW</td><td>比较捕获通道1互补输出极性设置位。</td><td>0</td></tr><tr><td>2</td><td>CC1NE</td><td>RW</td><td>比较捕获通道1互补输出使能位。</td><td>0</td></tr><tr><td>1</td><td>CC1P</td><td>RW</td><td>比较捕获通道1输出极性设置位。CC1通道配置为输出:1:0C1低电平有效;0:0C1高电平有效。CC1通道配置为输入:该位选择是IC1还是IC1的反相信号作为触发或捕获信号。1:反相:捕获发生在IC1的下降沿;当用作外部触发器时,IC1反相。0:不反相:捕获发生在IC1的上升沿;当用作外部触发器时,IC1不反相。注:一旦LOCK级别(TIMx_BDTR寄存器中的LOCK位)设为3或2,则该位不能被修改。</td><td>0</td></tr><tr><td>0</td><td>CC1E</td><td>RW</td><td>比较捕获通道1输出使能位。CC1通道配置为输出:1:开启。0C1信号输出到对应的输出引脚,其输出电平依赖于MOE、OSSI、OSSR、OIS1、OIS1N和CC1NE位的值。0:关闭。0C1禁止输出,因此0C1的输出电平依赖于MOE、OSSI、OSSR、OIS1、OIS1N和CC1NE位的值。CC1通道配置为输入:该位决定了计数器的值是否能捕获入TIMx_CCR1寄存器。1:捕获使能;0:捕获禁止。</td><td>0</td></tr></table>

### 14.4.10 高级定时器的计数器（TIMx_CNT）（x=1/8/9/10）

偏移地址： ${ 0 } { \times 2 4 }$

<table><tr><td>15</td><td>14</td><td>13</td><td>12</td><td>11</td><td>10</td><td>9</td><td>8</td><td>7</td><td>6</td><td>5</td><td>4</td><td>3</td><td>2</td><td>1</td><td>0</td></tr><tr><td colspan="16">CNT[15:0]</td></tr></table>

<table><tr><td>位</td><td>名称</td><td>访问</td><td>描述</td><td>复位值</td></tr><tr><td>[15:0]</td><td>CNT[15:0]</td><td>RW</td><td>定时器的计数器的实时值。</td><td>0</td></tr></table>

### 14.4.11 计数时钟预分频器（TIMx_PSC）（x=1/8/9/10）

偏移地址： $_ { 0 \times 2 8 }$

<table><tr><td>15</td><td>14</td><td>13</td><td>12</td><td>11</td><td>10</td><td>9</td><td>8</td><td>7</td><td>6</td><td>5</td><td>4</td><td>3</td><td>2</td><td>1</td><td>0</td></tr><tr><td colspan="16">PSC[15:0]</td></tr></table>

<table><tr><td>位</td><td>名称</td><td>访问</td><td>描述</td><td>复位值</td></tr><tr><td>[15:0]</td><td>PSC[15:0]</td><td>RW</td><td>定时器的预分频器的分频系数；计数器的时钟频率等于分频器的输入频率/(PSC+1)。</td><td>0</td></tr></table>

### 14.4.12 自动重装值寄存器（TIMx_ATRLR）（x=1/8/9/10）

偏移地址： $\mathtt { 0 } \mathtt { x } 2 0$

<table><tr><td>15</td><td>14</td><td>13</td><td>12</td><td>11</td><td>10</td><td>9</td><td>8</td><td>7</td><td>6</td><td>5</td><td>4</td><td>3</td><td>2</td><td>1</td><td>0</td></tr><tr><td colspan="16">ARR[15:0]</td></tr></table>

<table><tr><td>位</td><td>名称</td><td>访问</td><td>描述</td><td>复位值</td></tr><tr><td>[15:0]</td><td>ARR[15:0]</td><td>RW</td><td>此域的值将会被装入计数器，ATRLR何时动作和更新见14.2.3章节；ATRLR为空时，计数器停止。</td><td>0</td></tr></table>

### 14.4.13 重复计数值寄存器（TIMx_RPTCR）（x=1/8/9/10）

偏移地址： $\mathtt { 0 } { \times } 3 0$

<table><tr><td>15</td><td>14</td><td>13</td><td>12</td><td>11</td><td>10</td><td>9</td><td>8</td><td>7</td><td>6</td><td>5</td><td>4</td><td>3</td><td>2</td><td>1</td><td>0</td></tr><tr><td colspan="8">Reserved</td><td colspan="8">REP[7:0]</td></tr></table>

<table><tr><td>位</td><td>名称</td><td>访问</td><td>描述</td><td>复位值</td></tr><tr><td>[15:8]</td><td>Reserved</td><td>R0</td><td>保留。</td><td>0</td></tr><tr><td>[7:0]</td><td>REP[7:0]</td><td>RW</td><td>重复计数器的值。</td><td>0</td></tr></table>

### 14.4.14 比较/捕获寄存器 1（TIMx_CH1CVR）（x=1/8/9/10）

偏移地址： $0 \times 3 4$

<table><tr><td>15</td><td>14</td><td>13</td><td>12</td><td>11</td><td>10</td><td>9</td><td>8</td><td>7</td><td>6</td><td>5</td><td>4</td><td>3</td><td>2</td><td>1</td><td>0</td></tr><tr><td colspan="16">CCR1[15:0]</td></tr></table>

<table><tr><td>位</td><td>名称</td><td>访问</td><td>描述</td><td>复位值</td></tr><tr><td>[15:0]</td><td>CCR1[15:0]</td><td>RW</td><td>比较捕获寄存器通道1的值。</td><td>0</td></tr></table>

### 14.4.15 比较/捕获寄存器 2（TIMx_CH2CVR）（x=1/8/9/10）

偏移地址： ${ 0 } { \times } \ 3 { 8 }$

<table><tr><td>15</td><td>14</td><td>13</td><td>12</td><td>11</td><td>10</td><td>9</td><td>8</td><td>7</td><td>6</td><td>5</td><td>4</td><td>3</td><td>2</td><td>1</td><td>0</td></tr><tr><td colspan="16">CCR2[15:0]</td></tr></table>

<table><tr><td>位</td><td>名称</td><td>访问</td><td>描述</td><td>复位值</td></tr><tr><td>[15:0]</td><td>CCR2[15:0]</td><td>RW</td><td>比较捕获寄存器通道2的值。</td><td>0</td></tr></table>

### 14.4.16 比较/捕获寄存器 3（TIMx_CH3CVR）（x=1/8/9/10）

偏移地址： $\mathtt { 0 } \mathtt { x } \mathtt { 3 0 }$

<table><tr><td>15</td><td>14</td><td>13</td><td>12</td><td>11</td><td>10</td><td>9</td><td>8</td><td>7</td><td>6</td><td>5</td><td>4</td><td>3</td><td>2</td><td>1</td><td>0</td></tr><tr><td colspan="16">CCR3[15:0]</td></tr></table>

<table><tr><td>位</td><td>名称</td><td>访问</td><td>描述</td><td>复位值</td></tr><tr><td>[15:0]</td><td>CCR3[15:0]</td><td>RW</td><td>比较捕获寄存器通道3的值。</td><td>0</td></tr></table>

### 14.4.17 比较/捕获寄存器 4（TIMx_CH4CVR）（x=1/8/9/10）

偏移地址： $0 \times 4 0$

<table><tr><td>15</td><td>14</td><td>13</td><td>12</td><td>11</td><td>10</td><td>9</td><td>8</td><td>7</td><td>6</td><td>5</td><td>4</td><td>3</td><td>2</td><td>1</td><td>0</td></tr><tr><td colspan="16">CCR4[15:0]</td></tr></table>

<table><tr><td>位</td><td>名称</td><td>访问</td><td>描述</td><td>复位值</td></tr><tr><td>[15:0]</td><td>CCR4[15:0]</td><td>RW</td><td>比较捕获寄存器通道4的值。</td><td>0</td></tr></table>

### 14.4.18 刹车和死区寄存器（TIMx_BDTR）（x=1/8/9/10）

偏移地址： $0 \times 4 4$

<table><tr><td>15</td><td>14</td><td>13</td><td>12</td><td>11</td><td>10</td><td>9</td><td>8</td><td>7</td><td>6</td><td>5</td><td>4</td><td>3</td><td>2</td><td>1</td><td>0</td></tr><tr><td>MOE</td><td>AOE</td><td>BKP</td><td>BKE</td><td>OSSR</td><td>OSSI</td><td colspan="2">LOCK[1:0]</td><td colspan="8">DTG[7:0]</td></tr></table>

<table><tr><td>位</td><td>名称</td><td>访问</td><td>描述</td><td>复位值</td></tr><tr><td>15</td><td>MOE</td><td>RW</td><td>主输出使能位。一旦刹车信号有效,将被异步清零。1:允许OCx和OCxN设为输出;0:禁止OCx和OCxN的输出或者强制为空闲状态。</td><td>0</td></tr><tr><td>14</td><td>AOE</td><td>RW</td><td>自动输出使能。1:MOE可以被软件置位或者在下一个更新事件中被置位;0:MOE只能被软件置位。</td><td>0</td></tr><tr><td>13</td><td>BKP</td><td>RW</td><td>刹车输入极性设置位。</td><td>0</td></tr><tr><td></td><td></td><td></td><td>1:刹车输入高电平有效;0:刹车输入低电平有效。注:当设置了 LOCK 级别 1 后,该位不能被修改。对该位的写需要一个 APB 时钟以后才能生效。</td><td></td></tr><tr><td>12</td><td>BKE</td><td>RW</td><td>刹车功能使能位。1:开启刹车输入;0:禁止刹车输入。注:当设置了 LOCK 级别 1 后,该位不能被修改。对该位的写需要一个 APB 时钟以后才能生效。</td><td>0</td></tr><tr><td>11</td><td>OSSR</td><td>RW</td><td>1:当定时器不工作时,一旦 CCxE=1 或 CCxNE=1,首先开启 OC/OCN 并输出无效电平,然后置 OCx、OCxN 使能输出信号=1;0:当定时器不工作时,禁止 OC/OCN 输出。注:当设置了 LOCK 级别 1 后,该位不能被修改。</td><td>0</td></tr><tr><td>10</td><td>OSSI</td><td>RW</td><td>1:当定时器不工作时,一旦 CCxE=1 或 CCxNE=1,OC/OCN 首先输出其空闲电平,然后 OCx、OCxN 使能输出信号=1;0:当定时器不工作时,禁止 OC/OCN 输出。注:当设置了 LOCK 级别 1 后,该位不能被修改。</td><td>0</td></tr><tr><td>[9:8]</td><td>LOCK[1:0]</td><td>RW</td><td>锁定功能设置域。00:关闭锁定功能;01:锁定级别 1,不能写 DTG、BKE、BKP、AOE、OISx和 OISxN 位;10:锁定级别 2,不能写入锁定级别 1 中的各位,也不能写入 CC 极性位以及 OSSR 和 OSSI 位;11:锁定级别 3,不能写入锁定级别 2 中的各位,也不能写入 CC 控制位。注:在系统复位后,只能写一次 LOCK 位,无法再次修改直到复位。</td><td>00b</td></tr><tr><td>[7:0]</td><td>DTG[7:0]</td><td>RW</td><td>死区设置位,这些位定义了互补输出之间的死区持续时间。假设 DT 表示其持续时间:DTG[7:5]=0xx=&gt;DT=DTG[7:0]*Tdtg,Tdtg=TDDS;DTG[7:5]=10x=&gt;DT=(64+DTG[5:0])*Tdtg,Tdtg=2*TDDS;DTG[7:5]=110=&gt;DT=(32+DTG[4:0])*Tdtg,Tdtg=8×TDDS;DTG[7:5]=111=&gt;DT=(32+DTG[4:0])*Tdtg,Tdtg=16*TDDS。</td><td>0</td></tr></table>

### 14.4.19 DMA 控制寄存器（TIMx_DMACFGR）（x=1/8/9/10）

偏移地址： $0 \times 4 8$

<table><tr><td>Reserved</td><td>DBL[4:0]</td><td>Reserved</td><td>DBA[4:0]</td></tr></table>

<table><tr><td>位</td><td>名称</td><td>访问</td><td>描述</td><td>复位值</td></tr><tr><td>[15:13]</td><td>Reserved</td><td>R0</td><td>保留。</td><td>0</td></tr><tr><td>[12:8]</td><td>DBL[4:0]</td><td>RW</td><td>DMA连续传送的长度,实际值为此域的值+1。</td><td>0</td></tr><tr><td>[7:5]</td><td>Reserved</td><td>R0</td><td>保留。</td><td>0</td></tr><tr><td>[4:0]</td><td>DBA[4:0]</td><td>RW</td><td>这些位定义了DMA在连续模式下从控制寄存器1所在地址的偏移量。</td><td>0</td></tr></table>

### 14.4.20 连续模式的 DMA 地址寄存器（TIMx_DMAADR）（x=1/8/9/10）

偏移地址： $\mathtt { 0 } \mathtt { x } 4 0$

<table><tr><td>15</td><td>14</td><td>13</td><td>12</td><td>11</td><td>10</td><td>9</td><td>8</td><td>7</td><td>6</td><td>5</td><td>4</td><td>3</td><td>2</td><td>1</td><td>0</td></tr><tr><td colspan="16">DMAB[15:0]</td></tr></table>

<table><tr><td>位</td><td>名称</td><td>访问</td><td>描述</td><td>复位值</td></tr><tr><td>[15:0]</td><td>DMAB[15:0]</td><td>RW</td><td>连续模式下，DMA的地址。</td><td>0</td></tr></table>


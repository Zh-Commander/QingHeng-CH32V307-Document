# 第 3 章 复位和时钟控制（RCC）

本章模块描述适用于CH32F2x、CH32V2x和 $_ { C H 3 2 V 3 x }$ 微控制器全系列产品。

控制器根据电源区域的划分以及应用中的外设功耗管理考虑，提供了不同的复位形式以及可配置的时钟树结构。此章节描述了系统中各个时钟的作用域。

## 3.1 主要特性

多种复位形式  
$\bullet$ 多路时钟源，总线时钟管理  
$\bullet$ 内置外部晶体振荡监测和时钟安全系统  
$\bullet$ 各外设时钟独立管理：复位、开启、关闭  
支持内部时钟输出

## 3.2 复位

控制器提供了 3 种复位形式：电源复位、系统复位和后备区域复位。

### 3.2.1 电源复位

电源复位发生时，将复位除了后备区域外的所有寄存器（后备区域由 $\mathsf { V } _ { \mathsf { B A T } }$ 供电）。

其产生条件包括：

上电/掉电复位(POR/PDR 复位)  
从待机模式下唤醒

### 3.2.2 系统复位

系统复位发生时，将复位除了控制/状态寄存器 RCC_RSTSCKR 中的复位标志和后备区域外的所有寄存器。通过查看RCC_RSTSCKR寄存器中的复位状态标志位识别复位事件来源。

其产生条件包括：

NRST引脚上的低电平信号（外部复位）  
$\bullet$ 窗口看门狗计数终止(WWDG 复位)   
$\bullet$ 独立看门狗计数终止(IWDG 复位)  
软件复位(SW 复位)  
低功耗管理复位

窗口/独立看门狗复位：由窗口/独立看门狗外设定时器计数周期溢出触发产生，详细描述看其相应章节。

软件复位：CH32F2x 产品通过内核寄存器 AIRCR 中的 bit2 置 1 复位系统，具体操作请参考 Cortex-M3 内核手册获得更详细信息。CH32V2x 和 CH32V3x 产品通过可编程中断控制器 PFIC 中的中断配置寄存器 PFIC_CFGR 的 SYSRST 位置 1 复位系统或配置寄存器 PFIC_SCTLR 的 SYSRST 位置 1 复位系统，具体参考对应章节。

低功耗管理复位：通过将用户选择字节中的 STANDY_RST位置 0，将启用待机模式复位。这时执行了进入待机模式的过程后，将执行系统复位而不是进入待机模式。通过将用户选择字节中的 STOP_RST位置0，将启用停机模式复位。这时执行了进入停机模式的过程后，将执行系统复位而不是进入停机模式。

![](images/a78bef7b6b0c8c78fcf548cac76d105fd3101c08bf5ffc891c1af2477bd0eeb3.jpg)  
图 3-1系统复位结构

### 3.2.3 后备区域复位

后备区域复位发生时，只会复位后备区域寄存器，包括后备寄存器、RCC_BDCTLR 寄存器（RTC 使能和 LSE振荡器）。其产生条件包括：

$\bullet$ 在 $\mathsf { V } _ { \mathsf { D } \mathsf { D } }$ 和 $V _ { \mathsf { B A T } }$ 都掉电的前提下，由 $\mathsf { V } _ { \mathsf { D } \mathsf { D } }$ 或 $V _ { \mathsf { B A T } }$ 上电引起  
$\bullet$ RCC_BDCTLR 寄存器的 BDRST 位置 1  
RCC_APB1PRSTR 寄存器的 BKPRST 位置 1

## 3.3 时钟

### 3.3.1 系统时钟结构

图 3-2 CH32V305/307 和 CH32F205/207 时钟树框图  
![](images/c91960c5d87c364122ab44a041e6782099f71ed61085a2cd50acf395e0e48a7b.jpg)  
注：本时钟树适用于CH32F20xD8C和CH32V30xD8C。当使用USB功能时，CPU的频率必须是48MHz、96MHz或144MHz。使用USB高速功能时，USBHSPLL的时钟源只能为HSE。当系统从睡眠状态唤醒时系统会自动切换为HSI做主频。

图 3-3 CH32FV203/V303 时钟树框图  
![](images/8693b8502f18603f9e7597c70e4c306ff6950d317975ce47952f2b15b5c5f25d.jpg)  
注：本时钟树适用于CH32F20x_D6、CH32F20x_D8、CH32V20x_D6和CH32V30x_D8。

图 3-4 CH32V203RB 时钟树结构  
![](images/9aeb054c83cc8dcf410c871c0b95cef1853f669759a7f31dab349fc7e9209678.jpg)  
注：CH32V203RB产品外接晶体或时钟（HSE）为32M，使用外置晶体时无需负载电容已内置。

图 3-5 CH32FV208 时钟树结构  
![](images/2c0ba877f9d783be55537269b419c58dda5a74e9c4254a59847c33c690f46b63.jpg)  
注：本时钟树适用于CH32F20x_D8W、CH32V20x_D8和CH32V20x_D8W。若同时使用USB和ETH功能，需将USBPRE[1:0]置为11b。产品外接晶体或时钟（HSE）为32M，使用外置晶体时无需负载电容已内置。

### 3.3.2 高速时钟（HSI/HSE）

HSI 是系统内部 8MHz 的 RC 振荡器产生的高速时钟信号。HSI RC 振荡器能够在不需要任何外部器件的条件下提供系统时钟。它的启动时间很短但时钟频率精度较差。HSI 通过设置 RCC_CTLR 寄存器中的 HSION 位被启动和关闭，HSIRDY 位指示 HSI RC 振荡器是否稳定。系统默认 HSION 和 HSIRDY 置 1（建议不要关闭）。如果设置了RCC_INTR寄存器的 HSIRDYIE 位，将产生相应中断。

l 出厂校准：制造工艺的差异会导致每个芯片的 RC 振荡频率不同，所以在芯片出厂前，会为每颗

芯片进行HSI校准。系统复位后，工厂校准值被装载到 RCC_CTLR寄存器的HSICAL[7:0]中。

用户调整：基于不同的电压或环境温度，应用程序可以通过 RCC_CTLR寄存器里的 HSITRIM[4:0]位来调整HSI频率。

注：如果HSE晶体振荡器失效，HSI时钟会被作为备用时钟源（时钟安全系统）。

HSE 是外部的高速时钟信号，包括外部晶体/陶瓷谐振器产生或者外部高速时钟送入。

l 外部晶体/陶瓷谐振器（HSE晶体）：外接 3-25MHz 外部振荡器为系统提供更为精确的时钟源。进一步信息可参考数据手册的电气特性部分。HSE晶体可以通过设置RCC_CTLR寄存器中的HSEON位被启动和关闭，HSERDY位指示HSE晶体振荡是否稳定，硬件在HSERDY 位置 1后才将时钟送入系统。如果设置了RCC_INTR寄存器的HSERDYIE位，将产生相应中断。

图3-6 高速外部晶体电路  
![](images/4cadef2d18625327bde392668f21a18e5d913dbe58bc05bbf3649147e4ce29ef.jpg)  
注：负载电容需要尽可能地靠近振荡器引脚，并根据晶体厂家参数选择容值。

l 外部高速时钟源（HSE旁路）：此模式从外部直接送入时钟源到 OSC_IN 引脚，OSC_OUT引脚悬空。最高支持25MHz频率。应用程序需在HSEON 位为 0情况下，置位HSEBYP位，打开 HSE旁路功能，然后再置位HSEON位。

![](images/9fe818b3a416fd0e3585bfd8a9d001d68be46bab812fb8822d99809ac3689bb1.jpg)  
图3-7 高速时钟源电路

### 3.3.3 低速时钟（LSI/LSE）

LSI 是系统内部约 40KHz 的 RC 振荡器产生的低速时钟信号。它可以在停机和待机模式下保持运行，为RTC时钟、独立看门狗和唤醒单元提供时钟基准。进一步信息可参考数据手册的电气特性部分。LSI 可以通过设置 RCC_RSTSCKR 寄存器中的 LSION 位被启动和关闭，然后通过查询 LSIRDY 位检测 LSIRC 振荡是否稳定，硬件在 LSIRDY 位置 1 后才将时钟送入。如果设置了 RCC_INTR 寄存器的 LSIRDYIE位，将产生相应中断。

LSE是外部的低速时钟信号，包括外部晶体/陶瓷谐振器产生或者外部低速时钟送入。它为 RTC时钟或者其他定时功能提供一个低功耗且精确的时钟源。

l 外部晶体/陶瓷谐振器（LSE晶体）：外接 32.768KHz的外部低速振荡器。LSE通过设置 RCC_BDCTLR寄存器中的LSEON位被启动和关闭，LSERDY 位指示LSE 晶体振荡是否稳定，硬件在 LSERDY位置1后才将时钟送入系统。如果设置了RCC_INTR 寄存器的 LSERDYIE位，将产生相应中断。

![](images/6d3e22ee7046be55b83d770ee955b33756ceadb94652a6abeb47fc95a7c841b0.jpg)  
图3-8 低速外部晶体电路

l 外部低速时钟源（LSE旁路）：此模式从外部直接送入时钟源到 OSC32_IN 引脚，OSC32_OUT 引脚悬空。应用程序需在 LSEON 位为 0 情况下，置位 LSEBYP 位，打开 LSE 旁路功能，然后再置位 LSEON位。

![](images/c2a4f4143c40538744ffd88a2cfe207f2d473b4ab25810be00afb6571aa93b6a.jpg)  
图 3-9 低速时钟源电路

### 3.3.4 PLL 时钟

通过配置RCC_CFGR0寄存器和扩展寄存器 EXTEN_CTR，内部PLL时钟可以选择 3 种时钟来源和倍频系数，这些设置必须在每个 PLL 被开启前完成，一旦 PLL 被启动，这些参数就不能被改动。设置RCC_CTLR 寄存器中的 PLLON 位被启动和关闭，PLLRDY 位指示 PLL 时钟是否稳定，硬件在 PLLRDY 位置1后才将时钟送入系统。设置RCC_CTLR寄存器中的PLLON2位被启动和关闭，PLLRDY2位指示 PLL2时钟是否稳定，硬件在PLLRDY2位置1后才将时钟送入系统。设置RCC_CTLR寄存器中的 PLLON3位被启动和关闭，PLLRDY3 位指示 PLL3 时钟是否稳定，硬件在 PLLRDY3 位置 1 后才将时钟送入系统。如果设置了 RCC_INTR 寄存器的 PLLRDYIE 位、PLL2RDYIE 位或 PLL3RDYIE 位，将产生相应中断。

PLL 时钟来源：

HSI时钟送入  
HSI 经过 2 分频送入  
HSE 时钟或通过一个可配置的分频器的 PLL2 时钟

PLL2 和 PLL3 由 HSE 通过一个可配置的分频器（PREDIV2）2 提供时钟

### 3.3.5 总线/外设时钟

#### 3.3.5.1 系统时钟（SYSCLK）

通过配置RCC_CFGR0寄存器SW[1:0]位配置系统时钟来源，SWS[1:0]指示当前的系统时钟源。

HSI作为系统时钟  
HSE作为系统时钟  
PLL时钟作为系统时钟

控制器复位后，默认 HSI 时钟被选为系统时钟源。时钟源之间的切换必须在目标时钟源准备就绪后才会发生。

#### 3.3.5.2 AHB/APB1/APB2 总线外设时钟（HCLK/PCLK1/PCLK2）

通过配置 RCC_CFGR0 寄存器的 HPRE[3:0]、PPRE1[2:0]、PPRE2[2:0]位，可以分别配置 AHB、APB1、APB2 总线的时钟。这些总线时钟决定了挂载在其下面的外设接口访问时钟基准。应用程序可以调整不同的数值，来降低部分外设工作时的功耗。

通过 RCC_AHBRSTR、RCC_APB1PRSTR、RCC_APB2PRSTR 寄存器中各个位可以复位不同的外设模块，

将其恢复到初始状态。

通过 RCC_AHBPCENR、RCC_APB1PCENR、RCC_APB2PCENR 寄存器中各个位可以单独开启或关闭不同外设模块通讯时钟接口。使用某个外设时，首先需要开启其时钟使能位，才能访问其寄存器。

#### 3.3.5.3 RTC 时钟（RTCCLK）

通过设置 RCC_BDCTLR 寄存器的 RTCSEL[1:0]位，RTCCLK 时钟源可以由 HSE/128、LSE 或 LSI 时钟提供。修改此位前要保证电源控制寄存器(PWR_CR)中的 DBP 位置 1，只有后备区域复位，才能复位此位。

LSE 作为 RTC 时钟：由于 LSE 处于后备域由 $V _ { \mathsf { B A T } }$ 供电，只要 $V _ { \mathsf { B A T } }$ 维持供电，尽管 $\mathsf { V } _ { \mathsf { D } \mathsf { D } }$ 供电被切断，RTC 仍继续工作。  
LSI 作为 RTC 时钟：如果 $\mathsf { V } _ { \mathsf { D } \mathsf { D } }$ 供电被切断，RTC 自动唤醒不能保证。  
HSE/128 作为 RTC 时钟：如果 VDD 供电被切断或内部电压调压器被关闭(1.8V 域的供电被切断)，则 RTC 状态不确定。

#### 3.3.5.4 独立看门狗时钟

如果独立看门狗已经由硬件配置设置或软件启动，LSI 振荡器将被强制打开，并且不能被关闭。在LSI振荡器稳定后，时钟供应给IWDG。

#### 3.3.5.5 时钟输出（MCO）

微控制器允许输出时钟信号到 MCO 引脚。在相应的 GPIO 端口寄存器配置复用推挽输出模式，通过配置RCC_CFGR0寄存器MCO[3:0]位，可以选择以下 8 个时钟信号作为 MCO时钟输出：

系统时钟(SYSCLK)输出  
HSI 时钟输出  
$\bullet$ HSE 时钟输出  
$\bullet$ PLL时钟经过2分频输出  
$\bullet$ PLL2 时钟输出  
PLL3 时钟输出  
PLL3 时钟经过 2 分频输出  
XT1外部3-25MHz振荡器（用于以太网）

#### 3.3.5.6 USB 时钟

USBD 48MHz 时钟源来自通过一个可配置的分频器的 PLL 时钟，此时 PLL 支持三种时钟配置，包括 48MHz、96MHz 和 144MHz，通过配置寄存器 RCC_CFGR0 的 USBPRE[1:0]位输出 48MHz 时钟到 USBD。

OTG_FS/USBFS 48MHz 时钟源来自通过一个可配置的分频器的 PLL 时钟或者 USBHSPLL 时钟，可通过配置寄存器 RCC_CFGR2 的 USBHSSRC 位来选择。若时钟源选择通过一个可配置的分频器的 PLL 时钟作为时钟源时，则配置步骤可参考 USBD。若时钟源选择 USBHSPLL 时钟作为时钟源时，通过配置寄存器 RCC_CFGR2 的 USBHSCLK [1:0]位，选择 USBHS PLL 参考时钟频率（参考时钟频率必须和 USBHS PLL输入时钟保持一致）。

USBHS 时钟源来自 USBHSPLL 时钟，通过配置寄存器 RCC_CFGR2 的 USBHSCLK [1:0]位，选择 USBHSPLL 参考时钟频率（参考时钟频率必须和 USBHS PLL 输入时钟保持一致），通过配置寄存器 RCC_CFGR2的 USBHSPLL 位，使能 USB PHY 内部 PLL。

#### 3.3.5.7 ETH 时钟

ETH 时钟配置参考 27.1.4.5 章节。

#### 3.3.5.8 I2S 和 RNG 时钟

I2S 和 RNG 的时钟源来自 PLL3VCO 或系统时钟（SYSCLK），I2S2、I2S3 和 TRNG 可分别通过配置寄存器 RCC_CFGR2 的 I2S2SRC 位、I2S3SRC 位和 RNGSRC 位选择时钟源。

### 3.3.6 时钟安全系统

时钟安全系统是控制器的一种运行保护机制，它可以在 HSE 时钟发送故障的情况下，切换到 HSI时钟下，并产生中断通知，允许应用程序软件完成营救操作。

通过设置RCC_CTLR寄存器的CSSON位置 1，激活时钟安全系统。此时，时钟监测器将在 HSE振荡器启动（HSERDY=1）延迟后被使能，并在HSE时钟关闭后关闭。一旦系统运行过程中 HSE时钟发生故障，HSE 振荡器将被关闭，时钟失效事件将被送到高级定时器(TIM1 和 TIM8)的刹车输入端，并产生时钟安全中断，CSSF位置1，并且应用程序进入 NMI不可屏蔽中断，通过置位CSSC位，可以清除 CSSF位标志，可撤销NMI中断挂起位。

如果当前 HSE 作为系统时钟，或者当前 HSE 作为 PLL 输入时钟，PLL 作为系统时钟，时钟安全系统将在HSE故障时自动将系统时钟切换到 HSI振荡器，并关闭 HSE振荡器和PLL。

## 3.4 寄存器描述

表3-1 RCC相关寄存器列表  

<table><tr><td>名称</td><td>访问地址</td><td>描述</td><td>复位值</td></tr><tr><td>R32_RCC_CTLR</td><td>0x40021000</td><td>时钟控制寄存器</td><td>0x0000xx83</td></tr><tr><td>R32_RCC_CFGRO</td><td>0x40021004</td><td>时钟配置寄存器0</td><td>0x00000000</td></tr><tr><td>R32_RCC_INTR</td><td>0x40021008</td><td>时钟中断寄存器</td><td>0x00000000</td></tr><tr><td>R32_RCC_APB2PRSTR</td><td>0x4002100C</td><td>APB2外设复位寄存器</td><td>0x00000000</td></tr><tr><td>R32_RCC_APB1PRSTR</td><td>0x40021010</td><td>APB1外设复位寄存器</td><td>0x00000000</td></tr><tr><td>R32_RCC_AHBPCENR</td><td>0x40021014</td><td>AHB外设时钟使能寄存器</td><td>0x00000014</td></tr><tr><td>R32_RCC_APB2PCENR</td><td>0x40021018</td><td>APB2外设时钟使能寄存器</td><td>0x00000000</td></tr><tr><td>R32_RCC_APB1PCENR</td><td>0x4002101C</td><td>APB1外设时钟使能寄存器</td><td>0x00000000</td></tr><tr><td>R32_RCC_BDCTLR</td><td>0x40021020</td><td>后备域控制寄存器</td><td>0x00000000</td></tr><tr><td>R32_RCC_RSTSCKR</td><td>0x40021024</td><td>控制/状态寄存器</td><td>0x0C000000</td></tr><tr><td>R32_RCC_AHBRSTR</td><td>0x40021028</td><td>AHB外设复位寄存器</td><td>0x00000000</td></tr><tr><td>R32_RCC_CFGR2</td><td>0x4002102C</td><td>时钟配置寄存器2</td><td>0x00000000</td></tr></table>

表 3-2 OSC 相关寄存器列表  

<table><tr><td>名称</td><td>访问地址</td><td>描述</td><td>复位值</td></tr><tr><td>R32_HSE_CAL_CTRL</td><td>0x4002202C</td><td>外部晶振校准控制寄存器</td><td>0x09000000</td></tr><tr><td>R16_LSI32K_TUNE</td><td>0x40022036</td><td>内部低速晶振校准调节寄存器</td><td>0x1011</td></tr><tr><td>R8_LSI32K_CAL_CFG</td><td>0x40022049</td><td>内部低速晶振校准配置寄存器</td><td>0x01</td></tr><tr><td>R16_LSI32K_CAL_STAT</td><td>0x4002204C</td><td>内部低速晶振校准状态寄存器</td><td>0x0000</td></tr><tr><td>R8_LSI32K_CAL_OV_CNT</td><td>0x4002204E</td><td>内部低速晶振校准次数计数器</td><td>0x00</td></tr><tr><td>R8_LSI32K_CAL_CTRL</td><td>0x4002204F</td><td>内部低速晶振校准控制寄存器</td><td>0x80</td></tr></table>

注：适用于CH32V20x_D8W、CH32F20x_D8W。

### 3.4.1 时钟控制寄存器（RCC_CTLR）

偏移地址： $0 \times 0 0$   

<table><tr><td>31</td><td>30</td><td>29</td><td>28</td><td>27</td><td>26</td><td>25</td><td>24</td><td>23</td><td>22</td><td>21</td><td>20</td><td>19</td><td>18</td><td>17</td><td>16</td></tr><tr><td>Reserved</td><td>PLL3 RDY</td><td>PLL3 ON</td><td>PLL2 RDY</td><td>PLL2 ON</td><td>PLL RDY</td><td>PLL ON</td><td colspan="5">Reserved</td><td>CSSON</td><td>HSE BYP</td><td>HSE RDY</td><td>HSEON</td></tr><tr><td>15</td><td>14</td><td>13</td><td>12</td><td>11</td><td>10</td><td>9</td><td>8</td><td>7</td><td>6</td><td>5</td><td>4</td><td>3</td><td>2</td><td>1</td><td>0</td></tr><tr><td colspan="7">HSICAL[7:0]</td><td colspan="5">HSITRIM[4:0]</td><td>Reser ved</td><td>HSI RDY</td><td colspan="2">HSION</td></tr></table>

<table><tr><td>位</td><td>名称</td><td>访问</td><td>描述</td><td>复位值</td></tr><tr><td>[31:30]</td><td>Reserved</td><td>R0</td><td>保留。</td><td>0</td></tr><tr><td>29</td><td>PLL3RDY</td><td>R0</td><td>PLL3时钟就绪锁定标志位(由硬件置位):1:PLL3时钟锁定;0:PLL3时钟未锁定。注:适用于CH32F20x_D8C、CH32V30x_D8C。</td><td>0</td></tr><tr><td>28</td><td>PLL3ON</td><td>RW</td><td>PLL3时钟使能控制位:1:使能PLL3时钟;0:关闭PLL3时钟。注:进入停止或待机低功耗模式后,此位由硬件清0。适用于CH32F20x_D8C、CH32V30x_D8C。</td><td>0</td></tr><tr><td>27</td><td>PLL2RDY</td><td>R0</td><td>PLL2时钟就绪锁定标志位(由硬件置位):1:PLL时钟锁定;0:PLL时钟未锁定。注:适用于CH32F20x_D8C、CH32V30x_D8C。</td><td>0</td></tr><tr><td>26</td><td>PLL2ON</td><td>RW</td><td>PLL2时钟使能控制位:1:使能PLL时钟;0:关闭PLL时钟。注:进入停止或待机低功耗模式后,此位由硬件清0。适用于CH32F20x_D8C、CH32V30x_D8C。</td><td>0</td></tr><tr><td>25</td><td>PLLRDY</td><td>R0</td><td>PLL时钟就绪锁定标志位(由硬件置位):1:PLL时钟锁定;0:PLL时钟未锁定。</td><td>0</td></tr><tr><td>24</td><td>PLLON</td><td>RW</td><td>PLL时钟使能控制位:1:使能PLL时钟;0:关闭PLL时钟。注:进入停止或待机低功耗模式后,此位由硬件清0。</td><td>0</td></tr><tr><td>[23:20]</td><td>Reserved</td><td>R0</td><td>保留。</td><td>0</td></tr><tr><td>19</td><td>CSSON</td><td>RW</td><td>时钟安全系统使能控制位:1:使能时钟安全系统。当HSE准备好(HSERDY置1),硬件开启对HSE的时钟监测功能,发现HSE异常触发CSSF标志及NMI中断;当HSE没有准备好,硬件关闭对HSE的时钟监测功能。0:关闭时钟安全系统。</td><td>0</td></tr><tr><td>18</td><td>HSEBYP</td><td>RW</td><td>外部高速晶体旁路控制位:1:旁路外部高速晶体/陶瓷谐振器(使用外部时钟源);</td><td>0</td></tr><tr><td></td><td></td><td></td><td>0:不旁路高速外部晶体/陶瓷谐振器。注:此位需在HSEON为0下写入。</td><td></td></tr><tr><td>17</td><td>HSERDY</td><td>RO</td><td>外部高速晶体振荡稳定就绪标志位(由硬件置位):1:外部高速晶体振荡稳定;0:外部高速晶体振荡没有稳定。注:在HSEON位清0后,该位需要6个HSE周期清0。</td><td>0</td></tr><tr><td>16</td><td>HSEON</td><td>RW</td><td>外部高速晶体振荡使能控制位:1:使能HSE振荡器;0:关闭HSE振荡器。注:进入停止或待机低功耗模式后,此位由硬件清0。</td><td>0</td></tr><tr><td>[15:8]</td><td>HSICAL[7:0]</td><td>RO</td><td>内部高速时钟校准值,在系统启动时被自动初始化。</td><td>xxh</td></tr><tr><td>[7:3]</td><td>HSITRIM[4:0]</td><td>RW</td><td>内部高速时钟调整值:用户可以输入一个调整值叠加到HSICAL[7:0]数值上,根据电压和温度的变化调整内部HSI RC振荡器的频率。默认值为16,可以把HSI调整到8MHz±1%;每步HSICAL的变化调整约40KHz。</td><td>10000b</td></tr><tr><td>2</td><td>Reserved</td><td>RO</td><td>保留。</td><td>0</td></tr><tr><td>1</td><td>HSIRDY</td><td>RO</td><td>内部高速时钟(8MHz)稳定就绪标志位(由硬件置位):1:内部高速时钟(8MHz)稳定;0:内部高速时钟(8MHz)没有稳定。注:在HSION位清0后,该位需要6个HSI周期清0。</td><td>1</td></tr><tr><td>0</td><td>HSION</td><td>RW</td><td>内部高速时钟(8MHz)使能控制位:1:使能HSI振荡器;0:关闭HSI振荡器。注:当从待机和停止模式返回或用作系统时钟的外部振荡器HSE发生故障时,该位由硬件置1来启动内部8MHz的RC振荡器。</td><td>1</td></tr></table>

### 3.4.2 时钟配置寄存器 0（RCC_CFGR0）

偏移地址： $0 \times 0 4$

<table><tr><td>31</td><td>30</td><td>29</td><td>28</td><td>27</td><td>26</td><td>25</td><td>24</td><td>23</td><td>22</td><td>21</td><td>20</td><td>19</td><td>18</td><td>17</td><td>16</td></tr><tr><td>ADCDUTY</td><td colspan="2">Reserved</td><td colspan="2">ETHP
RE</td><td colspan="3">MCO[3:0]</td><td colspan="2">USBPRE
[1:0]</td><td colspan="4">PLLMUL[3:0]</td><td>PLL
XTPRE</td><td>PLL
SRC</td></tr><tr><td>15</td><td>14</td><td>13</td><td>12</td><td>11</td><td>10</td><td>9</td><td>8</td><td>7</td><td>6</td><td>5</td><td>4</td><td>3</td><td>2</td><td>1</td><td>0</td></tr><tr><td colspan="2">ADCPRE[1:0]</td><td colspan="3">PPRE2[2:0]</td><td colspan="3">PPRE1[2:0]</td><td colspan="3">HPRE[3:0]</td><td colspan="3">SWS[1:0]</td><td colspan="2">SW[1:0]</td></tr></table>

<table><tr><td>位</td><td>名称</td><td>访问</td><td>描述</td><td>复位值</td></tr><tr><td>31</td><td>ADCDUTY</td><td>RW</td><td>ADC时钟占空比调整:1:ADC时钟低电平时间更长;0:ADC时钟占空比为50%。</td><td>0</td></tr><tr><td>[30:29]</td><td>Reserved</td><td>RO</td><td>保留。</td><td>0</td></tr><tr><td>28</td><td>ETHPRE</td><td>RW</td><td>以太网时钟来源预分频控制:0:不分频;</td><td>0</td></tr><tr><td></td><td></td><td></td><td>1:2分频;注:适用于CH32V20x_D8W、CH32V20x_D8、CH32F20x_D8W。</td><td></td></tr><tr><td>[27:24]</td><td>MCO[3:0]</td><td>RW</td><td>微控制器MCO引脚时钟输出控制:00xx:没有时钟输出;0100:系统时钟(SYSCLK)输出;0101:内部8MHz的RC振荡器时钟(HSI)输出;0110:外部振荡器时钟(HSE)输出;0111:PLL时钟2分频后输出;1000:PLL2时钟输出;1001:PLL3时钟2分频后输出;1010:XT1外部震荡器时钟输出;1011:PLL3时钟输出。注:在启动或切换MCO时钟时,可能有几个周期的时钟丢失。其中1000——1011适用于适用于CH32F20x_D8C、CH32V30x_D8C。</td><td>0000b</td></tr><tr><td>[23:22]</td><td>USBPRE[1:0]</td><td>RW</td><td>USBFS/USBOTG时钟分频配置:00:1分频(适用于PLLCLK=48MHz);01:2分频(适用于PLLCLK=96MHz);10:3分频(适用于PLLCLK=144MHz);11:5分频,且PLL的源为HSE二分频(适用于PLLCLK=240MHz,仅适用于CH32V20x_D8W/CH32F20x_D8W)。注:CH32V20x_D8W、CH32F20x_D8W具有11b选项,其余型号该选项保留。</td><td>00b</td></tr><tr><td>[21:18]</td><td>PLLMUL[3:0]</td><td>RW</td><td>PLL时钟倍频系数(在PLL关闭才可写入):对于CH32V20x_D8、CH32V20x_D8W、CH32F20x_D8W、CH32V30x_D8、CH32F20x_D8、CH32V20x_D8W、CH32F20x_D8W:0000:PLL2倍频输出;0001:PLL3倍频输出;0010:PLL4倍频输出;0011:PLL5倍频输出;0100:PLL6倍频输出;0101:PLL7倍频输出;0110:PLL8倍频输出;0111:PLL9倍频输出;1000:PLL10倍频输出;1001:PLL11倍频输出;1010:PLL12倍频输出;1011:PLL13倍频输出;1100:PLL14倍频输出;1101:PLL15倍频输出;1110:PLL16倍频输出;1111:PLL18倍频输出。对于CH32F20x_D8C、CH32V30x_D8C:0000:PLL18倍频输出;0001:PLL3倍频输出;0010:PLL4倍频输出;0011:PLL5倍频输出;0100:PLL6倍频输出;0101:PLL7倍频输出;0110:PLL8倍频输出;0111:PLL9倍频输出;1000:PLL10倍频输出;1001:PLL 11倍频输出;1010:PLL12倍频输出;1011:PLL13倍频输出;1100:PLL14倍频输出;1101:PLL6.5倍频输出;1110:PLL15倍频输出;1111:PLL16倍频输出。</td><td>0000b</td></tr><tr><td>17</td><td>PLLXTPRE</td><td>RW</td><td>HSE分频送入PLL控制(在PLL关闭才可写入):对于CH32F20x_D6、CH32F20x_D8、CH32F20x_D8C、CH32V20x_D6、CH32V30x_D8、CH32V30x_D8C:1:HSE2分频送入PLL;0:HSE不分频送入PLL。对于CH32F20x_D8W、CH32V20x_D8、CH32V20x_D8W:1:HSE8分频送入PLL;0:HSE4分频送入PLL。</td><td>0</td></tr><tr><td>16</td><td>PLLSRC</td><td>RW</td><td>PLL的输入时钟源(在PLL关闭才可写入):1:HSE不分频或2分频送入PLL;0:HSI不分频或2分频送入PLL。</td><td>0</td></tr><tr><td>[15:14]</td><td>ADCPRE[1:0]</td><td>RW</td><td>ADC时钟来源预分频控制:00:PCLK22分频后作为ADC时钟;01:PCLK24分频后作为ADC时钟;10:PCLK26分频后作为ADC时钟;11:PCLK28分频后作为ADC时钟。注:ADC时钟最高不要超过14MHz。</td><td>00b</td></tr><tr><td>[13:11]</td><td>PPRE2[2:0]</td><td>RW</td><td>APB2时钟来源预分频控制:0xx:HCLK不分频;100:HCLK2分频;101:HCLK4分频;110:HCLK8分频;111:HCLK16分频</td><td>000b</td></tr><tr><td>[10:8]</td><td>PPRE1[2:0]</td><td>RW</td><td>APB1时钟来源预分频控制:0xx:HCLK不分频;100:HCLK2分频;101:HCLK4分频;110:HCLK8分频;111:HCLK16分频</td><td>000b</td></tr><tr><td>[7:4]</td><td>HPRE[3:0]</td><td>RW</td><td>AHB时钟来源预分频控制:0xxx:SYSCLK不分频;1000:SYSCLK2分频;1001:SYSCLK4分频;1010:SYSCLK8分频;1011:SYSCLK16分频;1100:SYSCLK64分频;1101:SYSCLK128分频;1110:SYSCLK256分频;1111:SYSCLK512分频。注:当AHB时钟来源的预分频系数大于1时,必须开启预取缓冲器。</td><td>0000b</td></tr><tr><td>[3:2]</td><td>SWS[1:0]</td><td>RO</td><td>系统时钟(SYSCLK)状态(硬件置位):00:系统时钟源是HSI;01:系统时钟源是HSE;10:系统时钟源是PLL;</td><td>00b</td></tr><tr><td></td><td></td><td></td><td>11:不可用。</td><td></td></tr><tr><td>[1:0]</td><td>SW[1:0]</td><td>RW</td><td>选择系统时钟来源:00:HSI作为系统时钟;01:HSE作为系统时钟;10:PLL输出作为系统时钟;11:不可用。注:在使能了时钟安全系统下(CSSON=1),当从待机和停止模式返回或用作系统时钟的外部振荡器HSE发生故障时,由硬件强制选择HSI作为系统时钟。</td><td>00b</td></tr></table>

### 3.4.3 时钟中断寄存器（RCC_INTR）

偏移地址： $0 \times 0 8$

<table><tr><td colspan="9">Reserved</td><td>CSSC</td><td>PLL3 RDYC</td><td>PLL2 RDYC</td><td>PLL RDYC</td><td>HSE RDYC</td><td>HSI RDYC</td><td>LSE RDYC</td><td>LSI RDYC</td></tr><tr><td>15</td><td>14</td><td>13</td><td>12</td><td>11</td><td>10</td><td>9</td><td>8</td><td>7</td><td>6</td><td>5</td><td>4</td><td>3</td><td>2</td><td>1</td><td>0</td><td></td></tr><tr><td>Reserved</td><td>PLL3 RDYIE</td><td>PLL2 RDYIE</td><td>PLL RDYIE</td><td>HSE RDYIE</td><td>HSI RDYIE</td><td>LSE RDYIE</td><td>LSI RDYIE</td><td>CSSF</td><td>PLL3 RDYF</td><td>PLL2 RDYF</td><td>PLL RDYF</td><td>HSE RDYF</td><td>HSI RDYF</td><td>LSE RDYF</td><td>LSI RDYF</td><td>LSI RDYF</td></tr></table>

<table><tr><td>位</td><td>名称</td><td>访问</td><td>描述</td><td>复位值</td></tr><tr><td>[31:24]</td><td>Reserved</td><td>RO</td><td>保留。</td><td>0</td></tr><tr><td>23</td><td>CSSC</td><td>WO</td><td>清除时钟安全系统中断标志位（CSSF）：1：清除CSSF中断标志；0：无动作。</td><td>0</td></tr><tr><td>22</td><td>PLL3RDYC</td><td>WO</td><td>清除PLL3就绪中断标志位：1：清除PLL3RDYF中断标志；0：无动作。注：适用于CH32F20x_D8C、CH32V30x_D8C。</td><td>0</td></tr><tr><td>21</td><td>PLL2RDYC</td><td>WO</td><td>清除PLL2就绪中断标志位：1：清除PLL2RDYF中断标志；0：无动作。注：适用于CH32F20x_D8C、CH32V30x_D8C。</td><td>0</td></tr><tr><td>20</td><td>PLLRDYC</td><td>WO</td><td>清除PLL就绪中断标志位：1：清除PLLRDYF中断标志；0：无动作。</td><td>0</td></tr><tr><td>19</td><td>HSERDYC</td><td>WO</td><td>清除HSE振荡器就绪中断标志位：1：清除HSERDYF中断标志；0：无动作。</td><td>0</td></tr><tr><td>18</td><td>HSIRDYC</td><td>WO</td><td>清除HSI振荡器就绪中断标志位：1：清除HSIRDYF中断标志；0：无动作。</td><td>0</td></tr><tr><td>17</td><td>LSERDYC</td><td>WO</td><td>清除LSE振荡器就绪中断标志位：1：清除LSERDYF中断标志；0：无动作。</td><td>0</td></tr><tr><td>16</td><td>LSIRDYC</td><td>WO</td><td>清除LSI振荡器就绪中断标志位:1:清除LSIRDYF中断标志;0:无动作。</td><td>0</td></tr><tr><td>15</td><td>Reserved</td><td>RO</td><td>保留。</td><td>0</td></tr><tr><td>14</td><td>PLL3RDYIE</td><td>RW</td><td>PLL3就绪中断使能位:1:使能PLL3就绪中断;0:关闭PLL3就绪中断。注:适用于CH32F20x_D8C、CH32V30x_D8C。</td><td>0</td></tr><tr><td>13</td><td>PLL2RDYIE</td><td>RW</td><td>PLL2就绪中断使能位:1:使能PLL2就绪中断;0:关闭PLL2就绪中断。注:适用于CH32F20x_D8C、CH32V30x_D8C。</td><td>0</td></tr><tr><td>12</td><td>PLLRDYIE</td><td>RW</td><td>PLL就绪中断使能位:1:使能PLL就绪中断;0:关闭PLL就绪中断。</td><td>0</td></tr><tr><td>11</td><td>HSERDYIE</td><td>RW</td><td>HSE就绪中断使能位:1:使能HSE就绪中断;0:关闭HSE就绪中断。</td><td>0</td></tr><tr><td>10</td><td>HSIRDYIE</td><td>RW</td><td>HSI就绪中断使能位:1:使能HSI就绪中断;0:关闭HSI就绪中断。</td><td>0</td></tr><tr><td>9</td><td>LSERDYIE</td><td>RW</td><td>LSE就绪中断使能位:1:使能LSE就绪中断;0:关闭LSE就绪中断。</td><td>0</td></tr><tr><td>8</td><td>LSIRDYIE</td><td>RW</td><td>LSI就绪中断使能位:1:使能LSI就绪中断;0:关闭LSI就绪中断。</td><td>0</td></tr><tr><td>7</td><td>CSSF</td><td>RO</td><td>时钟安全系统中断标志位:1:HSE时钟失效,产生了时钟安全中断CSSI;0:无时钟安全系统中断。硬件置位,软件写CSSC位1清除。</td><td>0</td></tr><tr><td>6</td><td>PLL3RDYF</td><td>RO</td><td>PLL3时钟就绪锁定中断标志:1:PLL3时钟锁定产生中断;0:无PLL3时钟锁定中断。硬件置位,软件写PLL3RDYC位1清除。注:适用于CH32F20x_D8C、CH32V30x_D8C。</td><td>0</td></tr><tr><td>5</td><td>PLL2RDYF</td><td>RO</td><td>PLL2时钟就绪锁定中断标志:1:PLL2时钟锁定产生中断;0:无PLL2时钟锁定中断。硬件置位,软件写PLL2RDYC位1清除。注:适用于CH32F20x_D8C、CH32V30x_D8C。</td><td>0</td></tr><tr><td>4</td><td>PLLRDYF</td><td>RO</td><td>PLL时钟就绪锁定中断标志:1:PLL时钟锁定产生中断;0:无PLL时钟锁定中断。硬件置位,软件写PLLRDYC位1清除。</td><td>0</td></tr><tr><td>3</td><td>HSERDYF</td><td>RO</td><td>HSE时钟就绪中断标志:</td><td>0</td></tr><tr><td></td><td></td><td></td><td>1: HSE 时钟就绪产生中断;0: 无 HSE 时钟就绪中断。硬件置位,软件写 HSERDYC 位 1 清除。</td><td></td></tr><tr><td>2</td><td>HSIRDYF</td><td>RO</td><td>HSI 时钟就绪中断标志:1: HSI 时钟就绪产生中断;0: 无 HSI 时钟就绪中断。硬件置位,软件写 HSIRDYC 位 1 清除。</td><td>0</td></tr><tr><td>1</td><td>LSERDYF</td><td>RO</td><td>LSE 时钟就绪中断标志:1: LSE 时钟就绪产生中断;0: 无 LSE 时钟就绪中断。硬件置位,软件写 LSERDYC 位 1 清除。</td><td>0</td></tr><tr><td>0</td><td>LSIRDYF</td><td>RO</td><td>LSI 时钟就绪中断标志:1: LSI 时钟就绪产生中断;0: 无 LSI 时钟就绪中断。硬件置位,软件写 LSIRDYC 位 1 清除。</td><td>0</td></tr></table>

### 3.4.4 APB2 外设复位寄存器（RCC_APB2PRSTR）

偏移地址： $0 \times 0 0$

<table><tr><td colspan="11">Reserved</td><td>TIM10
RST</td><td>TIM9
RST</td><td colspan="4">Reserved</td></tr><tr><td>15</td><td>14</td><td>13</td><td>12</td><td>11</td><td>10</td><td>9</td><td>8</td><td>7</td><td>6</td><td>5</td><td>4</td><td>3</td><td>2</td><td>1</td><td>0</td><td></td></tr><tr><td>Reserved</td><td>USART1
RST</td><td>TIM8
RST</td><td>SPI1
RST</td><td>TIM1
RST</td><td>ADC2
RST</td><td>ADC1
RST</td><td>Reserved</td><td>IOPE
RST</td><td>IOPD
RST</td><td>IOPC
RST</td><td>IOPB
RST</td><td>IOPA
RST</td><td>Reserved</td><td>AFIO
RST</td><td></td><td></td></tr></table>

<table><tr><td>位</td><td>名称</td><td>访问</td><td>描述</td><td>复位值</td></tr><tr><td>[31:21]</td><td>Reserved</td><td>R0</td><td>保留。</td><td>0</td></tr><tr><td>20</td><td>TIM10RST</td><td>RW</td><td>TIM10模块复位控制:1:复位模块; 0:无作用。</td><td>0</td></tr><tr><td>19</td><td>TIM9RST</td><td>RW</td><td>TIM9模块复位控制:1:复位模块; 0:无作用。</td><td>0</td></tr><tr><td>[18:15]</td><td>Reserved</td><td>R0</td><td>保留。</td><td>0</td></tr><tr><td>14</td><td>USART1RST</td><td>RW</td><td>USART1接口复位控制:1:复位模块; 0:无作用。</td><td>0</td></tr><tr><td>13</td><td>TIM8RST</td><td>RW</td><td>TIM8模块复位控制:1:复位模块; 0:无作用。</td><td>0</td></tr><tr><td>12</td><td>SPI1RST</td><td>RW</td><td>SPI1接口复位控制:1:复位模块; 0:无作用。</td><td>0</td></tr><tr><td>11</td><td>TIM1RST</td><td>RW</td><td>TIM1模块复位控制:1:复位模块; 0:无作用。</td><td>0</td></tr><tr><td>10</td><td>ADC2RST</td><td>RW</td><td>ADC2模块复位控制:1:复位模块; 0:无作用。</td><td>0</td></tr><tr><td>9</td><td>ADC1RST</td><td>RW</td><td>ADC1模块复位控制:1:复位模块; 0:无作用。</td><td>0</td></tr><tr><td>[8:7]</td><td>Reserved</td><td>RO</td><td>保留。</td><td>0</td></tr><tr><td>6</td><td>I0PERST</td><td>RW</td><td>IO的PE端口模块复位控制:1:复位模块; 0:无作用。</td><td>0</td></tr><tr><td>5</td><td>I0PDRST</td><td>RW</td><td>IO的PD端口模块复位控制:1:复位模块; 0:无作用。</td><td>0</td></tr><tr><td>4</td><td>I0PCRST</td><td>RW</td><td>IO的PC端口模块复位控制:1:复位模块; 0:无作用。</td><td>0</td></tr><tr><td>3</td><td>I0PBRST</td><td>RW</td><td>IO的PB端口模块复位控制:1:复位模块; 0:无作用。</td><td>0</td></tr><tr><td>2</td><td>I0PARST</td><td>RW</td><td>IO的PA端口模块复位控制:1:复位模块; 0:无作用。</td><td>0</td></tr><tr><td>1</td><td>Reserved</td><td>RO</td><td>保留。</td><td>0</td></tr><tr><td>0</td><td>AFIORST</td><td>RW</td><td>IO辅助功能模块复位控制:1:复位模块; 0:无作用。</td><td>0</td></tr></table>

### 3.4.5 APB1 外设复位寄存器（RCC_APB1PRSTR）

偏移地址： $0 \times 1 0$

<table><tr><td>31</td><td>30</td><td>29</td><td>28</td><td>27</td><td>26</td><td>25</td><td>24</td><td>23</td><td>22</td><td>21</td><td>20</td><td>19</td><td>18</td><td>17</td><td>16</td></tr><tr><td colspan="2">Reserved</td><td>DAC
RST</td><td>PWR
RST</td><td>BKP
RST</td><td>CAN2
RST</td><td>CAN1
RST</td><td>Reserved</td><td>USBD
RST</td><td>I2C2
RST</td><td>I2C1
RST</td><td>UART
5
RST</td><td>UART
4
RST</td><td>USART3
RST</td><td>USART2
RST</td><td>Reserved</td></tr><tr><td>15</td><td>14</td><td>13</td><td>12</td><td>11</td><td>10</td><td>9</td><td>8</td><td>7</td><td>6</td><td>5</td><td>4</td><td>3</td><td>2</td><td>1</td><td>0</td></tr><tr><td>SPI3
RST</td><td>SPI2
RST</td><td colspan="2">Reserved</td><td>WWDG
RST</td><td colspan="2">Reserved</td><td>UART
8RST</td><td>UART
7RST</td><td>UART
6RST</td><td>TIM7
RST</td><td>TIM6
RST</td><td>TIM5
RST</td><td>TIM4
RST</td><td>TIM3
RST</td><td>TIM2
RST</td></tr></table>

<table><tr><td>位</td><td>名称</td><td>访问</td><td>描述</td><td>复位值</td></tr><tr><td>[31:30]</td><td>Reserved</td><td>RO</td><td>保留。</td><td>0</td></tr><tr><td>29</td><td>DACRST</td><td>RW</td><td>DAC模块复位控制:1:复位模块; 0:无作用。</td><td>0</td></tr><tr><td>28</td><td>PWRRST</td><td>RW</td><td>电源接口模块复位控制:1:复位模块; 0:无作用。</td><td>0</td></tr><tr><td>27</td><td>BKPRST</td><td>RW</td><td>后备单元复位控制:1:复位模块; 0:无作用。</td><td>0</td></tr><tr><td>26</td><td>CAN2RST</td><td>RW</td><td>CAN2模块复位控制:1:复位模块; 0:无作用。</td><td>0</td></tr><tr><td>25</td><td>CAN1RST</td><td>RW</td><td>CAN1模块复位控制:1:复位模块; 0:无作用。</td><td>0</td></tr><tr><td>24</td><td>Reserved</td><td>RO</td><td>保留。</td><td>0</td></tr><tr><td>23</td><td>USBDRST</td><td>RW</td><td>USBD模块复位控制:1:复位模块; 0:无作用。</td><td>0</td></tr><tr><td>22</td><td>I2C2RST</td><td>RW</td><td>I2C2接口复位控制:1:复位模块; 0:无作用。</td><td>0</td></tr><tr><td>21</td><td>I2C1RST</td><td>RW</td><td>I2C1接口复位控制:1:复位模块; 0:无作用。</td><td>0</td></tr><tr><td>20</td><td>UART5RST</td><td>RW</td><td>UART5接口复位控制:1:复位模块; 0:无作用。</td><td>0</td></tr><tr><td>19</td><td>UART4RST</td><td>RW</td><td>UART4接口复位控制:1:复位模块; 0:无作用。</td><td>0</td></tr><tr><td>18</td><td>USART3RST</td><td>RW</td><td>USART3接口复位控制:1:复位模块; 0:无作用。</td><td>0</td></tr><tr><td>17</td><td>USART2RST</td><td>RW</td><td>USART2接口复位控制:1:复位模块; 0:无作用。</td><td>0</td></tr><tr><td>16</td><td>Reserved</td><td>RO</td><td>保留。</td><td>0</td></tr><tr><td>15</td><td>SPI3RST</td><td>RW</td><td>SPI3接口复位控制:1:复位模块; 0:无作用。</td><td>0</td></tr><tr><td>14</td><td>SPI2RST</td><td>RW</td><td>SPI2接口复位控制:1:复位模块; 0:无作用。</td><td>0</td></tr><tr><td>[13:12]</td><td>Reserved</td><td>RO</td><td>保留。</td><td>0</td></tr><tr><td>11</td><td>WWDGRST</td><td>RW</td><td>窗口看门狗复位控制:1:复位模块; 0:无作用。</td><td>0</td></tr><tr><td>[10:9]</td><td>Reserved</td><td>RO</td><td>保留。</td><td>0</td></tr><tr><td>8</td><td>UART8RST</td><td>RW</td><td>UART8接口复位控制:1:复位模块; 0:无作用。</td><td>0</td></tr><tr><td>7</td><td>UART7RST</td><td>RW</td><td>UART7接口复位控制:1:复位模块; 0:无作用。</td><td>0</td></tr><tr><td>6</td><td>UART6RST</td><td>RW</td><td>UART6接口复位控制:1:复位模块; 0:无作用。</td><td>0</td></tr><tr><td>5</td><td>TIM7RST</td><td>RW</td><td>定时器7模块复位控制:1:复位模块; 0:无作用。</td><td>0</td></tr><tr><td>4</td><td>TIM6RST</td><td>RW</td><td>定时器6模块复位控制:1:复位模块; 0:无作用。</td><td>0</td></tr><tr><td>3</td><td>TIM5RST</td><td>RW</td><td>定时器5模块复位控制:1:复位模块; 0:无作用。</td><td>0</td></tr><tr><td>2</td><td>TIM4RST</td><td>RW</td><td>定时器4模块复位控制:1:复位模块; 0:无作用。</td><td>0</td></tr><tr><td>1</td><td>TIM3RST</td><td>RW</td><td>定时器3模块复位控制:1:复位模块; 0:无作用。</td><td>0</td></tr><tr><td>0</td><td>TIM2RST</td><td>RW</td><td>定时器2模块复位控制:1:复位模块; 0:无作用。</td><td>0</td></tr></table>

### 3.4.6 AHB 外设时钟使能寄存器（RCC_AHBPCENR）

偏移地址： $0 \times 1 4$

<table><tr><td>31</td><td>30</td><td>29</td><td>28</td><td>27</td><td>26</td><td>25</td><td>24</td><td>23</td><td>22</td><td>21</td><td>20</td><td>19</td><td>18</td><td>17</td><td>16</td></tr><tr><td colspan="14">Reserved</td><td>BLES</td><td>ETHMACRXEN/BLEC</td></tr><tr><td>15</td><td>14</td><td>13</td><td>12</td><td>11</td><td>10</td><td>9</td><td>8</td><td>7</td><td>6</td><td>5</td><td>4</td><td>3</td><td>2</td><td>1</td><td>0</td></tr><tr><td>ETHMACXEN</td><td>ETHMACEN</td><td>DVPEN</td><td>OTGFSEN</td><td>USBHSEN</td><td>SDIOEN</td><td>RNGEN</td><td>FSMCEEN</td><td>Reserved</td><td>CRCEN</td><td colspan="3">Reserved</td><td>SRAMEN</td><td>DMA2EN</td><td>DMA1EN</td></tr></table>

<table><tr><td>位</td><td>名称</td><td>访问</td><td>描述</td><td>复位值</td></tr><tr><td>[31:18]</td><td>Reserved</td><td>R0</td><td>保留。</td><td>0</td></tr><tr><td>17</td><td>BLES</td><td>RW</td><td>BLES时钟使能:1: BLES时钟开启;0: BLES时钟关闭。注:适用于CH32V20x_D8W、CH32F20x_D8W。</td><td>1</td></tr><tr><td>16</td><td>BLEC</td><td>RW</td><td>BLEC时钟使能:1: BLEC时钟开启;0: BLEC时钟关闭。注:适用于CH32V20x_D8W、CH32F20x_D8W。</td><td>1</td></tr><tr><td>16</td><td>ETHMACRXEN</td><td>RW</td><td>以太网MAC接收时钟使能:1: 以太网MAC接收时钟开启;0: 以太网MAC接收时钟关闭。注:适用于CH32V30x_D8C、CH32F20x_D8C百兆千兆外置PHY。</td><td>0</td></tr><tr><td>15</td><td>ETHMACTXEN</td><td>RW</td><td>以太网MAC发送时钟使能:1: 以太网MAC发送时钟开启;0: 以太网MAC发送时钟关闭。注:适用于CH32V30x_D8C、CH32F20x_D8C百兆千兆外置PHY。</td><td>0</td></tr><tr><td>14</td><td>ETHMACEN</td><td>RW</td><td>以太网MAC时钟使能:1: 以太网MAC时钟开启;0: 以太网MAC时钟关闭。注:适用于CH32V30x_D8C、CH32F20x_D8C百兆千兆外置PHY。</td><td>0</td></tr><tr><td>13</td><td>DVPEN</td><td>RW</td><td>DVP模块时钟使能位:1: 模块时钟开启;0: 模块时钟关闭。</td><td>0</td></tr><tr><td>12</td><td>OTGFSEN</td><td>RW</td><td>USBOTG_FS模块时钟使能位:1: 模块时钟开启;0: 模块时钟关闭。</td><td>0</td></tr><tr><td>11</td><td>USBHSEN</td><td>RW</td><td>USBHS模块时钟使能位:1: 模块时钟开启;0: 模块时钟关闭。</td><td>0</td></tr><tr><td>10</td><td>SDIOEN</td><td>RW</td><td>SDIO模块时钟使能位:1: 模块时钟开启;0: 模块时钟关闭。</td><td>0</td></tr><tr><td>9</td><td>RNGEN</td><td>RW</td><td>RNG模块时钟使能位:1: 模块时钟开启;0: 模块时钟关闭。</td><td>0</td></tr><tr><td>8</td><td>FSMCEN</td><td>RW</td><td>FSMCEN模块时钟使能位:1: 模块时钟开启; 0: 模块时钟关闭。</td><td>0</td></tr><tr><td>7</td><td>Reserved</td><td>RO</td><td>保留。</td><td>0</td></tr><tr><td>6</td><td>CRCEN</td><td>RW</td><td>CRC 模块时钟使能位:1: 模块时钟开启; 0: 模块时钟关闭。</td><td>0</td></tr><tr><td>[5:3]</td><td>Reserved</td><td>RO</td><td>保留。</td><td>0</td></tr><tr><td>2</td><td>SRAMEN</td><td>RW</td><td>SRAM 接口模块时钟使能位:1: 睡眠模式时, SRAM 接口模块时钟开启;0: 睡眠模式时, SRAM 接口模块时钟关闭。</td><td>1</td></tr><tr><td>1</td><td>DMA2EN</td><td>RW</td><td>DMA2 模块时钟使能位:1: 模块时钟开启; 0: 模块时钟关闭。</td><td>0</td></tr><tr><td>0</td><td>DMA1EN</td><td>RW</td><td>DMA1 模块时钟使能位:1: 模块时钟开启; 0: 模块时钟关闭。</td><td>0</td></tr></table>

注：当外设时钟没有启用时，软件不能读出外设寄存器数值，返回的数值始终为0。

### 3.4.7 APB2 外设时钟使能寄存器（RCC_APB2PCENR）

偏移地址：0x18

<table><tr><td colspan="11">Reserved</td><td>TIM10
EN</td><td>TIM9
EN</td><td colspan="4">Reserved</td></tr><tr><td>15</td><td>14</td><td>13</td><td>12</td><td>11</td><td>10</td><td>9</td><td>8</td><td>7</td><td>6</td><td>5</td><td>4</td><td>3</td><td>2</td><td>1</td><td>0</td><td></td></tr><tr><td>Reserved</td><td>USART1
EN</td><td>TIM8
EN</td><td>SPI1
EN</td><td>TIM1
EN</td><td>ADC2
EN</td><td>ADC1
EN</td><td>Reserved</td><td>IOPE
EN</td><td>IOPD
EN</td><td>IOPC
EN</td><td>IOPB
EN</td><td>IOPA
EN</td><td>Reser
ved</td><td>AFIO
EN</td><td></td><td></td></tr></table>

<table><tr><td>位</td><td>名称</td><td>访问</td><td>描述</td><td>复位值</td></tr><tr><td>[31:21]</td><td>Reserved</td><td>RO</td><td>保留。</td><td>0</td></tr><tr><td>20</td><td>TIM10EN</td><td>RW</td><td>TIM10接口时钟使能位:1:模块时钟开启; 0:模块时钟关闭。</td><td>0</td></tr><tr><td>19</td><td>TIM9EN</td><td>RW</td><td>TIM9接口时钟使能位:1:模块时钟开启; 0:模块时钟关闭。</td><td>0</td></tr><tr><td>[18:15]</td><td>Reserved</td><td>RO</td><td>保留。</td><td>0</td></tr><tr><td>14</td><td>USART1EN</td><td>RW</td><td>USART1接口时钟使能位:1:模块时钟开启; 0:模块时钟关闭。</td><td>0</td></tr><tr><td>13</td><td>TIM8EN</td><td>RW</td><td>TIM8模块时钟使能位:1:模块时钟开启; 0:模块时钟关闭。</td><td>0</td></tr><tr><td>12</td><td>SPI1EN</td><td>RW</td><td>SPI1接口时钟使能位:1:模块时钟开启; 0:模块时钟关闭。</td><td>0</td></tr><tr><td>11</td><td>TIM1EN</td><td>RW</td><td>TIM1模块时钟使能位:1:模块时钟开启; 0:模块时钟关闭。</td><td>0</td></tr><tr><td>10</td><td>ADC2EN</td><td>RW</td><td>ADC2模块时钟使能位:1:模块时钟开启; 0:模块时钟关闭。</td><td>0</td></tr><tr><td>9</td><td>ADC1EN</td><td>RW</td><td>ADC1模块时钟使能位:1:模块时钟开启; 0:模块时钟关闭。</td><td>0</td></tr><tr><td>[8:7]</td><td>Reserved</td><td>RO</td><td>保留。</td><td>0</td></tr><tr><td>6</td><td>IOPEEN</td><td>RW</td><td>IO的PE端口模块时钟使能位:1:模块时钟开启; 0:模块时钟关闭。</td><td>0</td></tr><tr><td>5</td><td>IOPDEN</td><td>RW</td><td>IO的PD端口模块时钟使能位:1:模块时钟开启; 0:模块时钟关闭。</td><td>0</td></tr><tr><td>4</td><td>IOPCEN</td><td>RW</td><td>IO的PC端口模块时钟使能位:1:模块时钟开启; 0:模块时钟关闭。</td><td>0</td></tr><tr><td>3</td><td>IOPBEN</td><td>RW</td><td>IO的PB端口模块时钟使能位:1:模块时钟开启; 0:模块时钟关闭。</td><td>0</td></tr><tr><td>2</td><td>IOPAEN</td><td>RW</td><td>IO的PA端口模块时钟使能位:1:模块时钟开启; 0:模块时钟关闭。</td><td>0</td></tr><tr><td>1</td><td>Reserved</td><td>RO</td><td>保留。</td><td>0</td></tr><tr><td>0</td><td>AFIOEN</td><td>RW</td><td>IO辅助功能模块时钟使能位:1:模块时钟开启; 0:模块时钟关闭。</td><td>0</td></tr></table>

注：当外设时钟没有启用时，软件不能读出外设寄存器数值，返回的数值始终为0。

### 3.4.8 APB1 外设时钟使能寄存器（RCC_APB1PCENR）

偏移地址：0x1C

<table><tr><td>31</td><td>30</td><td>29</td><td>28</td><td>27</td><td>26</td><td>25</td><td>24</td><td>23</td><td>22</td><td>21</td><td>20</td><td>19</td><td>18</td><td>17</td><td>16</td></tr><tr><td colspan="2">Reserved</td><td>DAC
EN</td><td>PWR
EN</td><td>BKP
EN</td><td>CAN2
EN</td><td>CAN1
EN</td><td>Reserved</td><td>USBD
EN</td><td>I2C2
EN</td><td>I2C1
EN</td><td>UART5
EN</td><td>UART4
EN</td><td>USART3
EN</td><td>USART2
EN</td><td>Reserved</td></tr><tr><td>15</td><td>14</td><td>13</td><td>12</td><td>11</td><td>10</td><td>9</td><td>8</td><td>7</td><td>6</td><td>5</td><td>4</td><td>3</td><td>2</td><td>1</td><td>0</td></tr><tr><td>SPI3
EN</td><td>SPI2
EN</td><td colspan="2">Reserved</td><td>WWDG
EN</td><td colspan="2">Reserved</td><td>UART8
EN</td><td>UART7
EN</td><td>UART6
EN</td><td>TIM7
EN</td><td>TIM6
EN</td><td>TIM5
EN</td><td>TIM4
EN</td><td>TIM3
EN</td><td>TIM2
EN</td></tr></table>

<table><tr><td>位</td><td>名称</td><td>访问</td><td>描述</td><td>复位值</td></tr><tr><td>[31:30]</td><td>Reserved</td><td>RO</td><td>保留。</td><td>0</td></tr><tr><td>29</td><td>DACEN</td><td>RW</td><td>DAC模块时钟使能位:1:模块时钟开启; 0:模块时钟关闭。</td><td>0</td></tr><tr><td>28</td><td>PWREN</td><td>RW</td><td>电源接口模块时钟使能位:1:模块时钟开启; 0:模块时钟关闭。</td><td>0</td></tr><tr><td>27</td><td>BKPEN</td><td>RW</td><td>后备单元时钟使能位:1:模块时钟开启; 0:模块时钟关闭。</td><td>0</td></tr><tr><td>26</td><td>CAN2EN</td><td>RW</td><td>CAN2模块时钟使能位:1:模块时钟开启; 0:模块时钟关闭。</td><td>0</td></tr><tr><td>25</td><td>CAN1EN</td><td>RW</td><td>CAN1模块时钟使能位:1:模块时钟开启; 0:模块时钟关闭。</td><td>0</td></tr><tr><td>24</td><td>Reserved</td><td>RO</td><td>保留。</td><td>0</td></tr><tr><td>23</td><td>USBDEN</td><td>RW</td><td>USBD模块时钟使能位:1:模块时钟开启; 0:模块时钟关闭。</td><td>0</td></tr><tr><td>22</td><td>I2C2EN</td><td>RW</td><td>I2C2接口时钟使能位:1:模块时钟开启; 0:模块时钟关闭。</td><td>0</td></tr><tr><td>21</td><td>I2C1EN</td><td>RW</td><td>I2C1接口时钟使能位:1:模块时钟开启; 0:模块时钟关闭。</td><td>0</td></tr><tr><td>20</td><td>UART5EN</td><td>RW</td><td>UART5接口时钟使能位:</td><td>0</td></tr><tr><td></td><td></td><td></td><td>1:模块时钟开启; 0:模块时钟关闭。</td><td></td></tr><tr><td>19</td><td>UART4EN</td><td>RW</td><td>UART4接口时钟使能位:1:模块时钟开启; 0:模块时钟关闭。</td><td>0</td></tr><tr><td>18</td><td>USART3EN</td><td>RW</td><td>USART3接口时钟使能位:1:模块时钟开启; 0:模块时钟关闭。</td><td>0</td></tr><tr><td>17</td><td>USART2EN</td><td>RW</td><td>USART2接口时钟使能位:1:模块时钟开启; 0:模块时钟关闭。</td><td>0</td></tr><tr><td>16</td><td>Reserved</td><td>RO</td><td>保留。</td><td>0</td></tr><tr><td>15</td><td>SPI3EN</td><td>RW</td><td>SPI3接口时钟使能位:1:模块时钟开启; 0:模块时钟关闭。</td><td>0</td></tr><tr><td>14</td><td>SPI2EN</td><td>RW</td><td>SPI2接口时钟使能位:1:模块时钟开启; 0:模块时钟关闭。</td><td>0</td></tr><tr><td>[13:12]</td><td>Reserved</td><td>RO</td><td>保留。</td><td>0</td></tr><tr><td>11</td><td>WWDGEN</td><td>RW</td><td>窗口看门狗时钟使能位:1:模块时钟开启; 0:模块时钟关闭。</td><td>0</td></tr><tr><td>[10:9]</td><td>Reserved</td><td>RO</td><td>保留。</td><td>0</td></tr><tr><td>8</td><td>UART8EN</td><td>RW</td><td>UART8使能位:1:模块时钟开启; 0:模块时钟关闭。</td><td>0</td></tr><tr><td>7</td><td>UART7EN</td><td>RW</td><td>UART7使能位:1:模块时钟开启; 0:模块时钟关闭。</td><td>0</td></tr><tr><td>6</td><td>UART6EN</td><td>RW</td><td>UART6使能位:1:模块时钟开启; 0:模块时钟关闭。</td><td>0</td></tr><tr><td>5</td><td>TIM7EN</td><td>RW</td><td>定时器7模块时钟使能位:1:模块时钟开启; 0:模块时钟关闭。</td><td>0</td></tr><tr><td>4</td><td>TIM6EN</td><td>RW</td><td>定时器6模块时钟使能位:1:模块时钟开启; 0:模块时钟关闭。</td><td>0</td></tr><tr><td>3</td><td>TIM5EN</td><td>RW</td><td>定时器5模块时钟使能位:1:模块时钟开启; 0:模块时钟关闭。</td><td>0</td></tr><tr><td>2</td><td>TIM4EN</td><td>RW</td><td>定时器4模块时钟使能位:1:模块时钟开启; 0:模块时钟关闭。</td><td>0</td></tr><tr><td>1</td><td>TIM3EN</td><td>RW</td><td>定时器3模块时钟使能位:1:模块时钟开启; 0:模块时钟关闭。</td><td>0</td></tr><tr><td>0</td><td>TIM2EN</td><td>RW</td><td>定时器2模块时钟使能位:1:模块时钟开启; 0:模块时钟关闭。</td><td>0</td></tr></table>

注：当外设时钟没有启用时，软件不能读出外设寄存器数值，返回的数值始终为0。

### 3.4.9 后备域控制寄存器（RCC_BDCTLR）

偏移地址： $_ { 0 \times 2 0 }$

<table><tr><td>31</td><td>30</td><td>29</td><td>28</td><td>27</td><td>26</td><td>25</td><td>24</td><td>23</td><td>22</td><td>21</td><td>20</td><td>19</td><td>18</td><td>17</td><td>16</td></tr><tr><td colspan="15">Reserved</td><td>BDRST</td></tr><tr><td>15</td><td>14</td><td>13</td><td>12</td><td>11</td><td>10</td><td>9</td><td>8</td><td>7</td><td>6</td><td>5</td><td>4</td><td>3</td><td>2</td><td>1</td><td>0</td></tr><tr><td>RTCEN</td><td colspan="3">Reserved</td><td colspan="3">RTCSEL[1:0]</td><td colspan="5">Reserved</td><td>LSE BYP</td><td>LSE RDY</td><td colspan="2">LSEON</td></tr></table>

<table><tr><td>位</td><td>名称</td><td>访问</td><td>描述</td><td>复位值</td></tr><tr><td>[31:17]</td><td>Reserved</td><td>R0</td><td>保留。</td><td>0</td></tr><tr><td>16</td><td>BDRST</td><td>RW</td><td>后备域软件复位控制:1:复位整个后备域。0:撤销复位。</td><td>0</td></tr><tr><td>15</td><td>RTCEN</td><td>RW</td><td>RTC时钟使能控制:1:使能RTC时钟;0:关闭RTC时钟。注:RTCSEL!=0的条件下才可以使能RTC时钟,否则硬件强制为0。</td><td>0</td></tr><tr><td>[14:10]</td><td>Reserved</td><td>R0</td><td>保留。</td><td>0</td></tr><tr><td>[9:8]</td><td>RTCSEL[1:0]</td><td>RW</td><td>RTC时钟源选择:00:无时钟;01:LSE振荡器作为RTC时钟;10:LSI振荡器作为RTC时钟;11:HSE振荡器经128分频后作为RTC时钟。注:一旦RTC时钟源被选定(RTCEN=1),直到下次后备域被复位,它不能再被改变。可通过设置BDRST位来恢复默认。</td><td>0</td></tr><tr><td>[7:3]</td><td>Reserved</td><td>R0</td><td>保留。</td><td>0</td></tr><tr><td>2</td><td>LSEBYP</td><td>RW</td><td>外部低速晶体(LSE)旁路控制位:1:旁路外部低速晶体/陶瓷谐振器(使用外部时钟源);0:不旁路低速外部晶体/陶瓷谐振器。注:此位需在LSEON为0下写入。</td><td>0</td></tr><tr><td>1</td><td>LSERDY</td><td>RO</td><td>外部低速晶体振荡稳定就绪标志位(由硬件置位):1:外部低速晶体振荡稳定;0:外部低速晶体振荡没有稳定。注:在LSEON位清0后,该位需要6个LSE周期清0。</td><td>0</td></tr><tr><td>0</td><td>LSEON</td><td>RW</td><td>外部低速晶体振荡使能控制位:1:使能LSE振荡器;0:关闭LSE振荡器。</td><td>0</td></tr></table>

注：后备域控制寄存器中(RCC_BDCTLR)的LSEON、LSEBYP、RTCSEL和RTCEN位处于后备域。因此，这些位在复位后处于写保护状态，只有在电源控制寄存器(PWR_CR)中的DBP位置1后，才能对这些位进行改动。这些位只能由后备域复位清除。任何内部或外部复位都不会影响这些位。

### 3.4.10 控制/状态寄存器（RCC_RSTSCKR）

偏移地址： ${ 0 } { \times 2 4 }$

<table><tr><td>31</td><td>30</td><td>29</td><td>28</td><td>27</td><td>26</td><td>25</td><td>24</td><td>23</td><td>22</td><td>21</td><td>20</td><td>19</td><td>18</td><td>17</td><td>16</td></tr><tr><td>LPWR
RSTF</td><td>WWDG
RSTF</td><td>IWDG
RSTF</td><td>SFT
RSTF</td><td>POR
RSTF</td><td>PIN
RSTF</td><td>Reser
ved</td><td>RMVF</td><td colspan="8">Reserved</td></tr><tr><td>15</td><td>14</td><td>13</td><td>12</td><td>11</td><td>10</td><td>9</td><td>8</td><td>7</td><td>6</td><td>5</td><td>4</td><td>3</td><td>2</td><td>1</td><td>0</td></tr><tr><td colspan="14">Reserved</td><td>LSI
RDY</td><td>LSION</td></tr></table>

<table><tr><td>位</td><td>名称</td><td>访问</td><td>描述</td><td>复位值</td></tr><tr><td>31</td><td>LPWRSTF</td><td>RW</td><td>低功耗复位标志:1:发生低功耗复位;0:无低功耗复位发生。发生低功耗管理复位时由硬件置1;软件写RMVF位清除。</td><td>0</td></tr><tr><td>30</td><td>WWDGRSTF</td><td>RW</td><td>窗口看门狗复位标志:1:发生窗口看门狗复位;0:无窗口看门狗复位发生。发生窗口看门狗复位时由硬件置1;软件写RMVF位清除。</td><td>0</td></tr><tr><td>29</td><td>IWDGRSTF</td><td>RW</td><td>独立看门狗复位标志:1:发生独立看门狗复位;0:无独立看门狗复位发生。发生独立看门狗复位时由硬件置1;软件写RMVF位清除。</td><td>0</td></tr><tr><td>28</td><td>SFTRSTF</td><td>RW</td><td>软件复位标志:1:发生软件复位;0:无软件复位发生。发生软件复位时由硬件置1;软件写RMVF位清除。</td><td>0</td></tr><tr><td>27</td><td>PORRSTF</td><td>RW</td><td>上电/掉电复位标志:1:发生上电/掉电复位;0:无上电/掉电复位发生。发生上电/掉电复位时由硬件置1;软件写RMVF位清除。</td><td>1</td></tr><tr><td>26</td><td>PINRSTF</td><td>RW</td><td>外部手动复位(NRST引脚)标志:1:发生NRST引脚复位;0:无NRST引脚复位发生。在NRST引脚复位发生时由硬件置1;软件写RMVF位清除。</td><td>0</td></tr><tr><td>25</td><td>Reserved</td><td>RO</td><td>保留。</td><td>0</td></tr><tr><td>24</td><td>RMVF</td><td>RW</td><td>清除复位标志控制:1:清除复位标志;0:无作用。</td><td>0</td></tr><tr><td>[23:2]</td><td>Reserved</td><td>RO</td><td>保留。</td><td>0</td></tr><tr><td>1</td><td>LSIRDY</td><td>RO</td><td>内部低速时钟(LSI)稳定就绪标志位(由硬件置位):1:内部低速时钟(40KHz)稳定;0:内部低速时钟(40KHz)没有稳定。注:在LSION位清0后,该位需要3个LSI周期清0。</td><td>0</td></tr><tr><td>0</td><td>LSION</td><td>RW</td><td>内部低速时钟(LSI)使能控制位:1:使能LSI(40KHz)振荡器;0:关闭LSI(40KHz)振荡器。</td><td>0</td></tr></table>

注：除复位标志只能由上电复位清除，其他由系统复位清除。

### 3.4.11 AHB 外设复位寄存器（RCC_AHBRSTR）

偏移地址： $_ { 0 \times 2 8 }$

<table><tr><td>31</td><td>30</td><td>29</td><td>28</td><td>27</td><td>26</td><td>25</td><td>24</td><td>23</td><td>22</td><td>21</td><td>20</td><td>19</td><td>18</td><td>17</td><td>16</td></tr><tr><td colspan="16">Reserved</td></tr><tr><td>15</td><td>14</td><td>13</td><td>12</td><td>11</td><td>10</td><td>9</td><td>8</td><td>7</td><td>6</td><td>5</td><td>4</td><td>3</td><td>2</td><td>1</td><td>0</td></tr><tr><td>Reserved</td><td>ETHMARCST</td><td>DVPRST</td><td>OTGFSRST</td><td colspan="12">Reserved</td></tr></table>

<table><tr><td>位</td><td>名称</td><td>访问</td><td>描述</td><td>复位值</td></tr><tr><td>[31:15]</td><td>Reserved</td><td>R0</td><td>保留。</td><td>0</td></tr><tr><td>14</td><td>ETHMACRST</td><td>RW</td><td>以太网 MAC 复位控制:1: 复位模块; 0: 无作用。</td><td>0</td></tr><tr><td>13</td><td>DVPRST</td><td>R0</td><td>DVP 复位控制:1: 复位模块; 0: 无作用。</td><td>0</td></tr><tr><td>12</td><td>OTGFSRST</td><td>RW</td><td>USBOTG_FS 模块复位控制:1: 复位模块; 0: 无作用。</td><td>0</td></tr><tr><td>[11:0]</td><td>Reserved</td><td>R0</td><td>保留。</td><td>0</td></tr></table>

### 3.4.12 时钟配置寄存器 2（RCC_CFGR2）

偏移地址： $\mathtt { 0 } \mathtt { x } 2 0$

<table><tr><td>31</td><td>30</td><td>29</td><td>28</td><td>27</td><td>26</td><td>25</td><td>24</td><td>23</td><td>22</td><td>21</td><td>20</td><td>19</td><td>18</td><td>17</td><td>16</td></tr><tr><td>USBHS
SRC</td><td>USBHS
PLL</td><td>USBHSCLK
[1:0]</td><td>USBHS
PLLSR
C</td><td colspan="2">USBHSDIV[2:0]</td><td>Reser
ved</td><td>ETH1G
EN</td><td colspan="2">ETH1GSRC
[1:0]</td><td>RNGSR
C</td><td>I2S3S
RC</td><td>I2S2S
RC</td><td colspan="2">PREDI
V1SRC</td><td></td></tr><tr><td>15</td><td>14</td><td>13</td><td>12</td><td>11</td><td>10</td><td>9</td><td>8</td><td>7</td><td>6</td><td>5</td><td>4</td><td>3</td><td>2</td><td>1</td><td>0</td></tr><tr><td colspan="4">PLL3MUL[3:0]</td><td colspan="4">PLL2MUL[3:0]</td><td colspan="4">PREDIV2[3:0]</td><td colspan="4">PREDIV1[3:0]</td></tr></table>

<table><tr><td>位</td><td>名称</td><td>访问</td><td>描述</td><td>复位值</td></tr><tr><td>31</td><td>USBHSSRC</td><td>RW</td><td>USBHS 48MHz时钟源选择:1:USB PHY 0:PLL CLK</td><td>0</td></tr><tr><td>30</td><td>USBHSPLL</td><td>RW</td><td>USBHS PHY 内部PLL 控制位:1:USB PHY 内部PLL 使能0:USB PHY 内部PLL 关闭</td><td>0</td></tr><tr><td>[29:28]</td><td>USBHSCLK[1:0]</td><td>RW</td><td>USBHS PLL 参考时钟频率选择(USBHSPLLSRC/USBHSDIV):00:3MHz01:4MHz10:8MHz11:5MHz</td><td>00b</td></tr><tr><td>27</td><td>USBHSPLLSRC</td><td>RW</td><td>USBHS PLL 参考源选择:1:HSI 0:HSE</td><td>0</td></tr><tr><td>[26:24]</td><td>USBHSDIV[2:0]</td><td>RW</td><td>USBHS PLL 参考源分频:000:1分频 001:2分频</td><td>000b</td></tr><tr><td></td><td></td><td></td><td>010:3分频 011:4分频100:5分频 101:6分频110:7分频 111:8分频</td><td></td></tr><tr><td>23</td><td>Reserved</td><td>R0</td><td>保留。</td><td>0</td></tr><tr><td>22</td><td>ETH1GEN</td><td>RW</td><td>千兆以太网125M时钟控制位:1:开启 0:关闭</td><td>0</td></tr><tr><td>[21:20]</td><td>ETH1GSRC[1:0]</td><td>RW</td><td>千兆以太网125M时钟选择:00:PLL2 VCO01:PLL3 VCO1x:外部PB1引脚输入</td><td>00b</td></tr><tr><td>19</td><td>RNGSRC</td><td>RW</td><td>RNG时钟源选择:1:PLL3 VCO 0:系统时钟</td><td>0</td></tr><tr><td>18</td><td>I2S3SRC</td><td>RW</td><td>I2S3时钟源:1:PLL3 VCO;0:系统时钟(SYSCLK)。</td><td>0</td></tr><tr><td>17</td><td>I2S2SRC</td><td>RW</td><td>I2S2时钟源:1:PLL3 VCO;0:系统时钟(SYSCLK)。</td><td>0</td></tr><tr><td>16</td><td>PREDIV1SRC</td><td>RW</td><td>PREDIV1时钟源:1:PLL2;0:HSE。</td><td>0</td></tr><tr><td>[15:12]</td><td>PLL3MUL[3:0]</td><td>RW</td><td>PLL3倍频因子(在PLL3关闭才可写入)。0000:PLL3 2.5倍频输出 0001:PLL3 12.5倍频输出;0010:PLL3 4倍频输出;0011:PLL3 5倍频输出;0100:PLL3 6倍频输出;0101:PLL3 7倍频输出;0110:PLL3 8倍频输出;0111:PLL3 9倍频输出;1000:PLL3 10倍频输出;1001:PLL3 11倍频输出;1010:PLL3 12倍频输出;1011:PLL3 13倍频输出;1100:PLL3 14倍频输出;1101:PLL3 15倍频输出;1110:PLL3 16倍频输出;1111:PLL3 20倍频输出</td><td>0000b</td></tr><tr><td>[11:8]</td><td>PLL2MUL[3:0]</td><td>RW</td><td>PLL2倍频因子(在PLL2关闭才可写入)。0000:PLL2 2.5倍频输出 0001:PLL2 12.5倍频输出;0010:PLL2 4倍频输出;0011:PLL2 5倍频输出;0100:PLL2 6倍频输出;0101:PLL2 7倍频输出;0110:PLL2 8倍频输出;0111:PLL2 9倍频输出;1000:PLL2 10倍频输出;1001:PLL2 11倍频输出;1010:PLL2 12倍频输出;1011:PLL2 13倍频输出;1100:PLL2 14倍频输出;1101:PLL2 15倍频输出;1110:PLL2 16倍频输出;1111:PLL2 20倍频输出</td><td>0000b</td></tr><tr><td>[7:4]</td><td>PREDIV2[3:0]</td><td>RW</td><td>PREDIV2分频因子(在PLL2和PLL3关闭才可写入)0000:PREDIV2不对输入时钟分频;0001:PREDIV2对输入时钟2分频;0010:PREDIV2对输入时钟3分频;0011:PREDIV2对输入时钟4分频;0100:PREDIV2对输入时钟5分频;0101:PREDIV2对输入时钟6分频;</td><td>0000b</td></tr><tr><td></td><td></td><td></td><td>0110: PREDIV2 对输入时钟 7 分频;0111: PREDIV2 对输入时钟 8 分频;1000: PREDIV2 对输入时钟 9 分频;1001: PREDIV2 对输入时钟 10 分频;1010: PREDIV2 对输入时钟 11 分频;1011: PREDIV2 对输入时钟 12 分频;1100: PREDIV2 对输入时钟 13 分频;1101: PREDIV2 对输入时钟 14 分频;1110: PREDIV2 对输入时钟 15 分频;1111: PREDIV2 对输入时钟 16 分频;</td><td></td></tr><tr><td>[3:0]</td><td>PREDIV1 [3:0]</td><td>RW</td><td>PREDIV1 分频因子(在 PLL 关闭才可写入)0000: PREDIV1 不对输入时钟分频;0001: PREDIV1 对输入时钟 2 分频;0010: PREDIV1 对输入时钟 3 分频;0011: PREDIV1 对输入时钟 4 分频;0100: PREDIV1 对输入时钟 5 分频;0101: PREDIV1 对输入时钟 6 分频;0110: PREDIV1 对输入时钟 7 分频;0111: PREDIV1 对输入时钟 8 分频;1000: PREDIV1 对输入时钟 9 分频;1001: PREDIV1 对输入时钟 10 分频;1010: PREDIV1 对输入时钟 11 分频;1011: PREDIV1 对输入时钟 12 分频;1100: PREDIV1 对输入时钟 13 分频;1101: PREDIV1 对输入时钟 14 分频;1110: PREDIV1 对输入时钟 15 分频;1111: PREDIV1 对输入时钟 16 分频;注:bit0 和 RCC_CFGRO 的 bit17 相同,修改 RCC_CFGRO 的 bit17 会同时改变该寄存器的 bit0。</td><td>0000b</td></tr></table>

注：适用于CH32F20x_D8C、CH32V30x_D8C。

### 3.4.13 外部晶振校准控制寄存器（HSE_CAL_CTRL）

偏移地址： $0 \times 0 0$   

<table><tr><td>31</td><td>30</td><td>29</td><td>28</td><td>27</td><td>26</td><td>25</td><td>24</td><td>23</td><td>22</td><td>21</td><td>20</td><td>19</td><td>18</td><td>17</td><td>16</td></tr><tr><td colspan="4">HSEC[3:0]</td><td>HSE
FAULT</td><td>Reser
ved</td><td colspan="2">HSEITRIM
[1:0]</td><td colspan="8">Reserved</td></tr><tr><td>15</td><td>14</td><td>13</td><td>12</td><td>11</td><td>10</td><td>9</td><td>8</td><td>7</td><td>6</td><td>5</td><td>4</td><td>3</td><td>2</td><td>1</td><td>0</td></tr><tr><td colspan="16">Reserved</td></tr></table>

<table><tr><td>位</td><td>名称</td><td>访问</td><td>描述</td><td>复位值</td></tr><tr><td>[31:28]</td><td>HSEC[3:0]</td><td>RW</td><td>HSE内置匹配电容调节位:111:22pF110:20pF101:18pF</td><td>000b</td></tr><tr><td></td><td></td><td></td><td>100:16pF011:14pF010:12pF001:10pF000:8pF</td><td></td></tr><tr><td>27</td><td>HSEFAULT</td><td>RW</td><td>HSE 失效检测禁用控制位:1: 忽略模拟输入的 HSE 失效检测信号0: 使用模拟输入的 HSE 失效检测信号</td><td>0</td></tr><tr><td>26</td><td>Reserved</td><td>RO</td><td>保留。</td><td>0</td></tr><tr><td>[25:24]</td><td>HSEITRIM[1:0]</td><td>RW</td><td>HSE 起振电流调节位。</td><td>01b</td></tr><tr><td>[23:0]</td><td>Reserved</td><td>RO</td><td>保留。</td><td>0</td></tr></table>

### 3.4.14 内部低速晶振校准调节寄存器（LSI32K_TUNE）

偏移地址： $0 \times 0 \mathsf { A }$

<table><tr><td>Reserved</td><td>HTUNE[7:0]</td><td>LTUNE[4:0]</td></tr></table>

<table><tr><td>位</td><td>名称</td><td>访问</td><td>描述</td><td>复位值</td></tr><tr><td>[15:13]</td><td>Reserved</td><td>RO</td><td>保留。</td><td>0</td></tr><tr><td>[12:5]</td><td>HTUNE[7:0]</td><td>RW</td><td>LSI32K细调配置位</td><td>80h</td></tr><tr><td>[4:0]</td><td>LTUNE[4:0]</td><td>RW</td><td>LSI32K粗调配置位</td><td>11h</td></tr></table>

### 3.4.15 内部低速晶振校准配置寄存器（LSI32K_CAL_CFG）

偏移地址： ${ 0 } \times 1 { 0 }$

<table><tr><td>7</td><td>6</td><td>5</td><td>4</td><td>3</td><td>2</td><td>1</td><td>0</td></tr><tr><td>Reserved</td><td>LPEN</td><td>WKUPEN</td><td>HALTMD</td><td colspan="4">CNTVLU[3:0]</td></tr></table>

<table><tr><td>位</td><td>名称</td><td>访问</td><td>描述</td><td>复位值</td></tr><tr><td>7</td><td>Reserved</td><td>RO</td><td>保留。</td><td>0</td></tr><tr><td>6</td><td>LPEN</td><td>RW</td><td>低功耗模式下校准使能位:0:在低功耗模式下关闭校准功能1:在低功耗模式下使能校准功能注:此功能必须要配合EXTEN中的RB_HSE_KEEP_LP使用</td><td>0</td></tr><tr><td>5</td><td>WKUPEN</td><td>RW</td><td>LS132K唤醒中断使能:0:禁止唤醒中断1:使能唤醒中断</td><td>0</td></tr><tr><td>4</td><td>HALTMD</td><td>RW</td><td>LS132K校准计数暂停时长配置位:0:计数暂停维持1个CK32K周期1:计数暂停维持3个CK32K周期</td><td>0</td></tr><tr><td>[3:0]</td><td>CNTVLU[3:0]</td><td>RW</td><td>LS132K校准采样时长配置位:0000:2个CK32K周期0001:4个CK32K周期0010:32个CK32K周期</td><td>0001b</td></tr><tr><td></td><td></td><td></td><td>0011:64个CK32K周期0100:128个CK32K周期0101:256个CK32K周期0110:512个CK32K周期0111:1024个CK32K周期1000:1088个CK32K周期1001:1152个CK32K周期1010:1216个CK32K周期1011:1280个CK32K周期1100:2000个CK32K周期注:其他的配置值对应2个CK32K周期</td><td></td></tr></table>

### 3.4.16 内部低速晶振校准状态寄存器（LSI32K_CAL_STATR）

偏移地址： $_ { 0 \times 2 0 }$

<table><tr><td>15</td><td>14</td><td>13</td><td>12</td><td>11</td><td>10</td><td>9</td><td>8</td><td>7</td><td>6</td><td>5</td><td>4</td><td>3</td><td>2</td><td>1</td><td>0</td></tr><tr><td>IFEND</td><td>CNTOV</td><td colspan="14">CNT[13:0]</td></tr></table>

<table><tr><td>位</td><td>名称</td><td>访问</td><td>描述</td><td>复位值</td></tr><tr><td>15</td><td>IFEND</td><td>RW1</td><td>LS132K校准计数结束中断标志位:0:采样计数中,无标志1:采样计数结束,标志置位</td><td>0</td></tr><tr><td>14</td><td>CNTOV</td><td>RW1</td><td>LS132K采样计数器溢出标志位:0:未发生溢出1:发生溢出</td><td>0</td></tr><tr><td>[13:0]</td><td>CNT[13:0]</td><td>R0</td><td>对若干个CK32K周期基于系统主频的计数值。注:具体的CK32K周期数可配置。</td><td>0</td></tr></table>

### 3.4.17 内部低速晶振校准次数计数器（LSI32K_CAL_OV_CNT）

偏移地址： ${ 0 } \times 2 2$

<table><tr><td>7</td><td>6</td><td>5</td><td>4</td><td>3</td><td>2</td><td>1</td><td>0</td></tr><tr><td colspan="8">OVCNT[7:0]</td></tr></table>

<table><tr><td>位</td><td>名称</td><td>访问</td><td>描述</td><td>复位值</td></tr><tr><td>[7:0]</td><td>OVCNT[7:0]</td><td>R0</td><td>LS132K采样计数器溢出次数。
注:清除溢出标志操作会清除此计数器。</td><td>0</td></tr></table>

### 3.4.18 内部低速晶振校准控制寄存器（LSI32K_CAL_CTRL）

偏移地址： ${ 0 } \times { 2 } { 3 }$

<table><tr><td>7</td><td>6</td><td>5</td><td>4</td><td>3</td><td>2</td><td>1</td><td>0</td></tr><tr><td>HALT</td><td colspan="5">Reserved</td><td>CALEN</td><td>CAL INTEN</td></tr></table>

<table><tr><td>位</td><td>名称</td><td>访问</td><td>描述</td><td>复位值</td></tr><tr><td>7</td><td>HALT</td><td>R0</td><td>LS132K校准计数状态位:</td><td>1</td></tr><tr><td></td><td></td><td></td><td>0:计数中,计数值不可用
1:计数暂停,可获取计数值</td><td></td></tr><tr><td>[6:2]</td><td>Reserved</td><td>R0</td><td>保留。</td><td>0</td></tr><tr><td>1</td><td>CALEN</td><td>RW</td><td>LS132K校准使能</td><td>0</td></tr><tr><td>0</td><td>CAL INTEN</td><td>RW</td><td>LS132K校准中断使能</td><td>0</td></tr></table>

注：适用于CH32F20x_D8C、CH32V30×_D8C。


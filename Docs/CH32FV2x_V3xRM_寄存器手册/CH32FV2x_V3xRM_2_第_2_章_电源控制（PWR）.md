# 第 2 章 电源控制（PWR）

本章模块描述适用于CH32F2x、CH32V2x和CH32V3x微控制器全系列产品。

## 2.1 概述

系统工作电压 $\mathsf { V } _ { \mathsf { D } \mathsf { D } }$ 范围为 $2 . 4 { \sim } 3 . 6 \mathsf { V }$ ，内置电压调节器提供内核所需的 1.5V 电源。当主电源 $\mathsf { V } _ { \mathsf { D } \mathsf { D } }$ 掉电后，电池等后备电源可通过 $V _ { \mathsf { B A T } }$ 引脚为实时时钟(RTC)和后备寄存器提供电源，如果无需后备电源，建议将 $\mathsf { V } _ { \mathsf { D } \mathsf { D } }$ 直接连接到 $V _ { \mathsf { B A T } }$ 引脚上。

$V _ { \mathsf { D D A } }$ 和 $\mathsf { V } _ { \mathsf { S S A } }$ 引脚专门为系统中模拟相关电路供电，包括 ADC、DAC、温度传感器等。 $V _ { R E F + }$ 和 $V _ { R E F }$ -作为一些模拟电路的参考点，在芯片内部等于 $V _ { \tt D D A }$ 及 $\mathsf { V } _ { \mathsf { S S A } }$ 。实际应用中 $V _ { \mathsf { D D A } }$ 和 $\mathsf { V } _ { \mathsf { S S A } }$ 必须连接到 $\mathsf { V } _ { \mathsf { D } \mathsf { D } }$ 和 $\mathsf { V } _ { \mathbb S }$ 端。

![](images/7965a92aa198089b99f6f4e835b717d9f9217c33a27807937cbe0977eb697f68.jpg)  
图 2-1 电源结构框图

在主电源 $\mathsf { V } _ { \mathsf { D } \mathsf { D } }$ 掉电后，模拟开关切换至 $V _ { \mathsf { B A T } }$ ，后备区域由 $V _ { \mathsf { B A T } }$ 引脚供电，此时 $\mathsf { P C } 1 3 \sim 1 5$ 无法作为GPIO，仅可使用如下功能：

PC13 可以作为 TAMPER 引脚、RTC 闹钟或秒输出。  
PC14 和 PC15 只能用作 LSE 引脚。

当主电源 $\mathsf { V } _ { \mathsf { D } \mathsf { D } }$ 上电稳定后，系统自动切换后备区域由 $\mathsf { V } _ { \mathsf { D } \mathsf { D } }$ 供电， $\mathsf { P C } 1 3 \sim 1 5$ 可以用作 GPIO 功能。

当 $\mathsf { P C } 1 3 \sim 1 5$ 引脚作为 GPIO 输出时，速度必须限制在 2MHz 以下，最大负载电容为 30pF，并且禁止用在持续输出和吸入电流的场合，比如 LED 驱动。

注：在主电源 $V _ { D D }$ 恢复供电过程中，内部 $V _ { B A T }$ 电源仍然通过对应的 $V _ { B A T }$ 引脚连在外部备用电源上，若 $V _ { D D }$ 在小于复位滞后时间tRSTTEMPo内就达到稳定，并且高于 $V _ { B A T }$ 的值0.6V以 $\perp$ ，则有可能存在较短瞬间，电流通过 $V _ { D D }$ 与 $V _ { B A T }$ 之间的二极管灌入 $V _ { B A T }$ ，进而通过 $V _ { B A T }$ 引脚注入电池等后备电源，如果后备电源无法承受这样瞬时注入电流，建议在后备电源和 $V _ { B A T }$ 引脚之间加一只正向导通低压降二极管。

## 2.2 电源管理

### 2.2.1 上电复位和掉电复位

系统内部集成了上电复位POR和掉电复位PDR电路，当芯片供电电压 $\mathsf { V } _ { \mathsf { D } \mathsf { D } }$ 和 $V _ { \mathsf { D D A } }$ 低于对应门限电压

时，系统被相关电路复位，无需外置额外的复位电路。上电门限电压 $V _ { P O R }$ 和掉电门限电压 $V _ { P D R }$ 的参数请参考对应的数据手册。

![](images/8badda17f71fe8cea24ac32314527af9e2d5507ee93eeb9a5d81ffef02af9c7a.jpg)  
图 2-2 POR 和 PDR 的工作示意图

### 2.2.2 可编程电压监视器

可编程电压监视器 PVD，主要被用于监控系统主电源的变化，与电源控制寄存器 PWR_CTLR 的PLS[2:0]所设置的门槛电压相比较，配合外部中断寄存器（EXTI）设置，可产生相关中断，以便及时通知系统进行数据保存等掉电前操作。

具体配置如下：

1）设置PWR_CTLR寄存器的PLS[2:0]域，选择要监控电压阈值。  
2）可选的中断处理。PVD功能内部连接EXTI模块的第16线的上升/下降边沿触发设置，开启此中断（配置EXTI），当 $\mathsf { V } _ { \mathsf { D } \mathsf { D } }$ 下降到PVD阀值以下或上升到PVD 阀值之上时就会产生PVD中断。  
3） 设置 PWR_CTLR 寄存器的 PVDE 位来开启 PVD 功能。  
4） 读取 PWR_CSR 状态寄存器的 PVD0 位可获取当前系统主电源与 PLS[2:0]设置阈值关系，执行相应软处理。

![](images/ba13c29584b6d9468beb73e2361d613f8eff21baa6008b24e9fde5e60225f4d2.jpg)  
图 2-3 PVD 的工作示意图

## 2.3 低功耗模式

在系统复位后，微控制器处于正常工作状态（运行模式），此时可以通过降低系统主频或者关闭不用外设时钟或者降低工作外设时钟来节省系统功耗。如果系统不需要工作，可设置系统进入低功耗模式，并通过特定事件让系统跳出此状态。

微控制器目前提供了 3 种低功耗模式，从处理器、外设、电压调节器等的工作差异上分为：

睡眠模式：内核停止运行，所有外设（包含内核私有外设）仍在运行。

停止模式：停止所有时钟，唤醒后系统继续运行。  
待机模式：停止所有时钟，唤醒后微控制器复位（电源复位）。

表 2-1 低功耗模式一览  

<table><tr><td>模式</td><td>进入</td><td>唤醒源</td><td>对时钟的影响</td><td>电压调节器</td></tr><tr><td rowspan="2">睡眠</td><td>WFI</td><td>任意中断唤醒</td><td rowspan="2">内核时钟关闭,其他时钟无影响</td><td rowspan="2">正常</td></tr><tr><td>WFE</td><td>唤醒事件唤醒</td></tr><tr><td>停止</td><td>SLEEPDEEP 置 1PDDS 清 0WFI 或 WFE</td><td>任一外部中断/事件(在外部中断寄存器中设置)、WKUP 引脚上升沿</td><td>关闭 HSE、HSI、PLL 和外设时钟</td><td>正常: LPDS=0 或低功耗: LPDS=1</td></tr><tr><td>待机</td><td>SLEEPDEEP 置 1PDDS 置 1WFI 或 WFE</td><td>WKUP 引脚上升沿、RTC 闹钟事件、NRST 引脚复位、IWDG 复位。注: 任意事件也可以唤醒系统,但唤醒后系统复位。</td><td>关闭 HSE、HSI、PLL 和外设时钟</td><td>关闭</td></tr></table>

注：SLEEPDEEP位属于内核私有外设控制位，CH32F2x产品参考Cortex-M3内核手册，CH32V2×和产品参考 PFIC SCTLR 寄存器。

### 2.3.1 低功耗配置选项

#### WFI 和 WFE 方式

WFI：微控制器被具有中断控制器响应的中断源唤醒，系统唤醒后，将最先执行中断服务函数（微控制器复位除外）。

WFE：唤醒事件触发微控制器将退出低功耗模式。唤醒事件包括：

1）配置一个外部或内部的EXTI线为事件模式，此时无需配置中断控制器；  
2）或者配置某个中断源，等效为WFI唤醒，系统优先执行中断服务函数；  
3）或者配置SLEEPONPEN位，开启外设中断使能，但不开启中断控制器中的中断使能，系统唤醒后需要清除中断挂起位。

#### l SLEEPONEXIT

启用：执行WFI或WFE指令后，微控制器确保所有待处理的中断服务退出后进入低功耗模式。不启用：执行WFI或WFE指令后，微控制器立即进入低功耗模式。

#### SEVONPEND

启用：所有中断或者唤醒事件都可以唤醒通过执行 WFE 进入的低功耗。

不启用：只有在中断控制器中使能的中断或者唤醒事件可以唤醒通过执行 WFE进入的低功耗。

### 2.3.2 睡眠模式

此模式下，所有的 IO 引脚都保持他们运行模式下的状态，所有的外设时钟都正常，所以进入睡眠模式前，尽量关闭无用的外设时钟，以减低功耗。该模式唤醒所需时间最短。

进入：配置内核寄存器控制位 SLEEPDEEP=0，电源控制寄存器 $\mathsf { P D D S = } 0$ ，LPDS 决定内部调压器状态，执行 WFI 或 WFE，可选 SEVONPEND 和 SLEEPONEXIT。

退出：任意中断或者唤醒事件。

### 2.3.3 停止模式

停止模式是在内核的深睡眠模式（SLEEPDEEP）基础上结合了外设的时钟控制机制，并让电压调节器的运行处于更低功耗的状态。此模式高频时钟（HSE/HSI/PLL）域被关闭，SRAM 和寄存器内容保持，IO引脚状态保持。该模式唤醒后系统可以继续运行，HSI为默认系统时钟。

如果正在进行闪存编程，直到对内存访问完成，系统才进入停止模式；如果正在进行对 APB 的访问，直到对APB访问完成，系统才进入停止模式。

停止模式下可工作模块：独立看门狗（IWDG）、实时时钟（RTC）、低频时钟（LSI/LSE）。

进入：配置内核寄存器控制位 SLEEPDEEP=1，电源控制寄存器的 $\mathsf { P D D S = } 0$ ，可选 LPDS 位，执行 WFI

或 WFE，可选 SEVONPEND 和 SLEEPONEXIT。

退出：任一外部中断/事件(在外部中断寄存器中设置)。

在停止模式下，可选 LPDS 位， $\mathsf { L P D S = } 0$ ，电压调节器工作在正常模式； $\mathsf { L P D S } = 1$ ，电压调节器工作在低功耗模式。在低功耗模式下，可以通过配置 PWR_CTLR寄存器的RAMLV=1，使能 RAM低电压模式，功耗达到最低。

### 2.3.4 待机模式

待机模式对比停止模式，唯一的差别在于：在某些指定的唤醒条件下退出后，微控制器将被复位，并且执行的是电源复位。

待机模式下可工作模块：独立看门狗（IWDG）、实时时钟（RTC）、低频时钟（LSI/LSE）。

进入：配置内核寄存器控制位 SLEEPDEEP=1，电源控制寄存器的 $\mathsf { P D D S } = 1$ ，执行 WFI 或 WFE，可选SEVONPEND 和 SLEEPONEXIT。

退出：

1）任一事件(在外部中断寄存器中设置)，此唤醒后微控制器执行电源复位。  
2）WKUP引脚的上升沿、RTC闹钟事件的上升沿、NRST引脚上外部复位、IWDG 复位，此唤醒后微控制器执行电源复位。

在待机模式下，当正常供电时，通过配置 PWR_CTLR寄存器的 $R 2 { \sf K S } { \sf T } { \sf Y } = 1$ 控制 2K字节 RAM不掉电，${ \sf R } 3 0 { \sf K } { \sf S } { \sf T } { \sf Y } = 1$ 控制 30K 字节 RAM 不掉电；当使用 VBAT 供电时，通过配置 PWR_CTLR 寄存器的 R2KVBAT=1控制 2K 字节 RAM 不掉电，R32K_VBATEN $= 1$ 控制 30K字节 RAM不掉电。在该基础之上，可以通过配置PWR_CTLR 寄存器的 RAMLV=1，使能 RAM 低电压模式，功耗达到最低。

注：调试模式下，使微处理器进入停止或待机模式，将失去调试连接。

$R 2 K S T Y = 1$ 控制 2K 字节 RAM 的地址范围： 0x20000000—0x20000000+2K

R30KSTY=1控制30K字节RAM的地址范围： $0 { x } 2 0 0 0 0 0 0 0 { + } 2 { \cal K } .$ 0x20000000+2K+30K

### 2.3.5 RTC 自动唤醒

RTC 可以实现无需外部中断的情况下自动唤醒。通过对时间基数进行编程，可周期性地从停止或待机模式下唤醒。

可选择精准的外部低频 32.768KHz 晶振 LSE 作为 RTC 时钟源，也可以选择内部 LSI 振荡器作为RTC时钟源，LSI的精度和功耗指标要差于 LSE。

RTC 闹钟事件能够把 MCU 从停机模式下唤醒，为了实现此功能，需要把外部中断线 17 配置为上升沿中断，并且把RTC设置成可产生闹钟事件。而从待机模式下唤醒，仅需把 RTC设置成可产生闹钟事件。

## 2.4 寄存器描述

表 2-2 PWR 相关寄存器列表  

<table><tr><td>名称</td><td>访问地址</td><td>描述</td><td>复位值</td></tr><tr><td>R32_PWR_CTLR</td><td>0x40007000</td><td>电源控制寄存器</td><td>0x00000000</td></tr><tr><td>R32_PWR_CSR</td><td>0x40007004</td><td>电源控制/状态寄存器</td><td>0x00000000</td></tr></table>

### 2.4.1 电源控制寄存器（PWR_CTLR）

偏移地址： $0 \times 0 0$

<table><tr><td>Reserved</td><td>RAM
LV</td><td>R30K
VBAT</td><td>R2K
VBAT</td><td>R30K
STY</td><td>R2K
STY</td></tr></table>

<table><tr><td>15</td><td>14</td><td>13</td><td>12</td><td>11</td><td>10</td><td>9</td><td>8</td><td>7</td><td>6</td><td>5</td><td>4</td><td>3</td><td>2</td><td>1</td><td>0</td></tr><tr><td colspan="6">Reserved</td><td>DBP</td><td colspan="2">PLS[2:0]</td><td>PVDE</td><td>CSBF</td><td>CWUF</td><td>PDDS</td><td colspan="2">LPDS</td><td></td></tr></table>

<table><tr><td>位</td><td>名称</td><td>访问</td><td>描述</td><td>复位值</td></tr><tr><td>[31:21]</td><td>Reserved</td><td>R0</td><td>保留。</td><td>0</td></tr><tr><td>20</td><td>RAMLV</td><td>RW</td><td>RAM工作在低电压模式使能控制位(功耗相对更低):1:开启;0:关闭。注:在PWR_CTLR寄存器的LPDS位为1时有效。</td><td>0</td></tr><tr><td>19</td><td>R30KVBAT</td><td>RW</td><td>VBAT供电时,Standby模式下30K RAM是否带电控制位:1:带电;0:不带电。注:适用于CH32F20x_D8、CH32F20x_D8C、CH32V30x_D8、CH32V30x_D8C、CH32V20x_D8、CH32V20x_D8W、CH32F20x_D8。</td><td>0</td></tr><tr><td>18</td><td>R2KVBAT</td><td>RW</td><td>VBAT供电时,Standby模式下2K RAM是否带电控制位:1:带电;0:不带电。</td><td>0</td></tr><tr><td>17</td><td>R30KSTY</td><td>RW</td><td>Standby模式下30K RAM是否带电控制位:1:带电;0:不带电。注:适用于CH32F20x_D8、CH32F20x_D8C、CH32V30x_D8、CH32V30x_D8C、CH32V20x_D8、CH32V20x_D8W、CH32F20x_D9。</td><td>0</td></tr><tr><td>16</td><td>R2KSTY</td><td>RW</td><td>Standby模式下2K RAM是否带电控制位:1:带电;0:不带电。</td><td>0</td></tr><tr><td>[15:9]</td><td>Reserved</td><td>R0</td><td>保留。</td><td>0</td></tr><tr><td>8</td><td>DBP</td><td>RW</td><td>后备区域的写使能。当RTC时钟为外部时钟的128分频时,该位必须设置为1。1:允许写RTC和后备寄存器;0:禁止写RTC和后备寄存器。</td><td>0</td></tr><tr><td>[7:5]</td><td>PLS[2:0]</td><td>RW</td><td>PVD电压监视阈值设置。详细说明见数据手册中电气特性部分。000:上升沿2.37V/下降沿2.29V;001:上升沿2.55V/下降沿2.46V;010:上升沿2.63V/下降沿2.55V;011:上升沿2.76V/下降沿2.67V;100:上升沿2.87V/下降沿2.78V;101:上升沿3.03V/下降沿2.93V;110:上升沿3.18V/下降沿3.06V;111:上升沿3.29V/下降沿3.19V。</td><td>000b</td></tr><tr><td>4</td><td>PVDE</td><td>RW</td><td>电源电压监视功能使能标志位:1:开启电源电压监视功能;0:禁止电源电压监视功能。</td><td>0</td></tr><tr><td>3</td><td>CSBF</td><td>RW1</td><td>清除待机状态标志位,读出始终为0。1:置1清除SBF待机状态标志位;0:清0无效。</td><td>0</td></tr><tr><td>2</td><td>CWUF</td><td>RW1</td><td>清除唤醒状态标志位,读出始终为0。1:置1后2个系统时钟周期后清除WUF标志位;0:清0无效。</td><td>0</td></tr><tr><td>1</td><td>PDDS</td><td>RW</td><td>掉电深睡眠情景下,待机/停机模式选择位。1:进入待机模式;0:进入停机模式,电压调节器状态由LPDS控制。</td><td>0</td></tr><tr><td>0</td><td>LPDS</td><td>RW</td><td>停机模式下,电压调节器工作模式选择位。PDDS=0,该位有效。1:电压调节器工作在低功耗模式;0:电压调节器工作在正常模式。</td><td>0</td></tr></table>

注：此寄存器从待机模式唤醒时复位。

### 2.4.2 电源控制/状态寄存器（PWR_CSR）

偏移地址： $0 \times 0 4$

<table><tr><td>31</td><td>30</td><td>29</td><td>28</td><td>27</td><td>26</td><td>25</td><td>24</td><td>23</td><td>22</td><td>21</td><td>20</td><td>19</td><td>18</td><td>17</td><td>16</td></tr><tr><td colspan="16">Reserved</td></tr><tr><td>15</td><td>14</td><td>13</td><td>12</td><td>11</td><td>10</td><td>9</td><td>8</td><td>7</td><td>6</td><td>5</td><td>4</td><td>3</td><td>2</td><td>1</td><td>0</td></tr><tr><td colspan="7">Reserved</td><td>EWUP</td><td colspan="5">Reserved</td><td>PVDO</td><td>SBF</td><td>WUF</td></tr></table>

<table><tr><td>位</td><td>名称</td><td>访问</td><td>描述</td><td>复位值</td></tr><tr><td>[31:9]</td><td>Reserved</td><td>R0</td><td>保留。</td><td>0</td></tr><tr><td>8</td><td>EWUP</td><td>RW</td><td>WKUP引脚使能位:1:WKUP强制配置为输入下拉状态,用于把MCU从待机状态下唤醒;0:WKUP引脚可用于通用IO,无待机唤醒功能。</td><td>0</td></tr><tr><td>[7:3]</td><td>Reserved</td><td>R0</td><td>保留。</td><td>0</td></tr><tr><td>2</td><td>PVDO</td><td>R0</td><td>PVD输出状态标志位。当PWR_CTLR寄存器的PVDE=1时,该位有效。1:VDD和VDDA低于PLS[2:0]设定的PVD阈值;0:VDD和VDDA高于PLS[2:0]设定的PVD阈值。</td><td>0</td></tr><tr><td>1</td><td>SBF</td><td>R0</td><td>待机状态标志位,可通过CSBF位置1清除。1:MCU进入待机模式;0:MCU不在待机模式。</td><td>0</td></tr><tr><td>0</td><td>WUF</td><td>R0</td><td>唤醒事件状态标志位,可通过CWUF位置1清除。1:在WKUP引脚检测到唤醒事件或RTC闹钟事件;0:没有唤醒事件发生。</td><td>0</td></tr></table>

注：此寄存器从待机模式唤醒后保持不变。


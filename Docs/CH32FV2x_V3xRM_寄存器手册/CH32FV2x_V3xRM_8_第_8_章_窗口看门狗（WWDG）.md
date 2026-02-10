# 第 8 章 窗口看门狗（WWDG）

本章模块描述适用于CH32F2x、CH32V2x和CH32V3x微控制器全系列产品。

窗口看门狗一般用来监测系统运行的软件故障，例如外部干扰、不可预见的逻辑错误等情况。它需要在一个特定的窗口时间（有上下限）内进行计数器刷新（喂狗），否则早于或者晚于这个窗口时间看门狗电路都会产生系统复位。

## 8.1 主要特征

可编程的 7 位自减型计数器  
$\bullet$ 双条件复位：当前计数器值小于 $0 \times 4 0$ ，或者计数器值在窗口时间外被重装载  
唤醒提前通知功能（EWI），用于及时喂狗动作防止系统复位

## 8.2 功能说明

### 8.2.1 原理和用法

窗口看门狗运行基于一个 7 位的递减计数器，其挂载在 APB1 总线下，计数时基 WWDG_CLK 来源（PCLK1/4096）时钟的分频，分频系数在配置寄存器 WWDG_CFGR 中的 WDGTB[1:0]域设置。递减计数器处于自由运行状态，无论看门狗功能是否开启，计数器一直循环递减计数。如图 8-1所示，窗口看门狗内部结构框图。

![](images/baff27dc9557e943569713b5c4632826621eaa0523c48876e4ae56a2d584f082.jpg)  
图 8-1 窗口看门狗结构框图

#### 启动窗口看门狗

系统复位后，看门狗处于关闭状态，设置 WWDG_CTLR 寄存器的 WDGA 位能够开启看门狗，随后它不能再被关闭，除非发生复位。

注：可以通过设置RCC_APB1PCENR寄存器关闭WWDG的时钟来源，暂停 WWDG_CLK计数，间接停止看门狗功能，或者通过设置RCC_APB1PRSTR寄存器复位WWDG模块，等效为复位的作用。

#### 看门狗配置

看门狗内部是一个不断循环递减运行的 7位计数器，支持读写访问。使用看门狗复位功能，需要执行下面几点操作：

1） 计数时基：通过 WWDG_CFGR 寄存器的 WDGTB[1:0]位域，注意要开启 RCC 单元的 WWDG 模块时钟。  
2） 窗口计数器：设置 WWDG_CFGR 寄存器的 W[6:0]位域，此计数器由硬件用作和当前计数器比较使用，数值由用户软件配置，不会改变。作为窗口时间的上限值。  
3） 看门狗使能：WWDG_CTLR 寄存器 WDGA 位软件置 1，开启看门狗功能，可以系统复位。

4） 喂狗：即刷新当前计数器值，配置WWDG_CTLR寄存器的 T[6:0]位域。此动作需要在看门狗功能开启后，在周期性的窗口时间内执行，否则会出现看门狗复位动作。

#### 喂狗窗口时间

如图 8-2 所示，灰色区域为窗口看门狗的监测窗口区域，其上限时间 t2 对应当前计数器值达到窗口值W[6:0]的时间点；其下限时间t3对应当前计数器值达到 ${ 0 } \times { 3 } { \mathsf F }$ 的时间点。此区域时间内 $\pm 2 < \mathrm { t } < \mathrm { t } 3$ 可以进行喂狗操作（写T[6:0]），刷新当前计数器的数值。

![](images/da380698c6bc3527e8c434bc1a7f073a79df5a927133618f4f7c673cd4f3a714.jpg)  
图8-2 窗口看门狗的计数模式

#### 看门狗复位：

1） 当没有及时喂狗操作，导致 T[6:0]计数器的值由 $0 \times 4 0$ 变成 ${ 0 } \times { 3 } { \mathsf F }$ ，将出现“窗口看门狗复位”，产生系统复位。即T6-bit被硬件检测为 0，将出现系统复位。

注：应用程序可以通过软件写T6-bit为0，实现系统复位，等效软件复位功能。

2） 当在不允许喂狗时间内执行计数器刷新动作，即在 $\mathtt { t } 1 \leqslant \mathtt { t } \leqslant \mathtt { t } 2$ 时间内操作写 T[6:0]位域，将出现“窗口看门狗复位”，产生系统复位。

#### 提前唤醒

为了防止没有及时刷新计数器导致系统复位，看门狗模块提供了早期唤醒中断（EWI）通知。当计数器自减到 $0 \times 4 0$ 时，产生提前唤醒信号，EWIF 标志置 1，如果置位了 EWI 位，会同时触发窗口看门狗中断。此时距离硬件复位有1个计数器时钟周期（自减为 ${ 0 } \times { 3 } { \mathsf F }$ ），应用程序可在此时间内即时进行喂狗操作。

### 8.2.2 调试模式

系统进入调试模式时，可以由调试模块寄存器配置WWDG的计数器继续工作或停止。

## 8.3 寄存器描述

表 8-1 WWDG 相关寄存器列表  

<table><tr><td>名称</td><td>访问地址</td><td>描述</td><td>复位值</td></tr><tr><td>R16_WWDG_CTLR</td><td>0x40002C00</td><td>控制寄存器</td><td>0x007F</td></tr><tr><td>R16_WWDG_CFGR</td><td>0x40002C04</td><td>配置寄存器</td><td>0x007F</td></tr><tr><td>R16_WWDG_STAT</td><td>0x40002C08</td><td>状态寄存器</td><td>0x0000</td></tr></table>

### 8.3.1 WWDG 控制寄存器（WWDG_CTLR）

偏移地址： $0 \times 0 0$

<table><tr><td>15</td><td>14</td><td>13</td><td>12</td><td>11</td><td>10</td><td>9</td><td>8</td><td>7</td><td>6</td><td>5</td><td>4</td><td>3</td><td>2</td><td>1</td><td>0</td></tr><tr><td colspan="8">Reserved</td><td>WDGA</td><td colspan="7">T[6:0]</td></tr></table>

<table><tr><td>位</td><td>名称</td><td>访问</td><td>描述</td><td>复位值</td></tr><tr><td>[15:8]</td><td>Reserved</td><td>R0</td><td>保留。</td><td>0</td></tr><tr><td>7</td><td>WDGA</td><td>RW1</td><td>窗口看门狗复位使能位。
1: 开启看门狗功能（可产生复位信号）;
0: 禁止看门狗功能。
软件写1开启,但是只允许复位后硬件清0。</td><td>0</td></tr><tr><td>[6:0]</td><td>T[6:0]</td><td>RW</td><td>7位自减计数器,每4096*2WDGTB个PCLK1周期自减1。当计数器从0x40自减到0x3F时,即T6跳变为0时,产生看门狗复位。</td><td>7Fh</td></tr></table>

### 8.3.2 WWDG 配置寄存器（WWDG_CFGR）

偏移地址： $0 \times 0 4$

<table><tr><td>15</td><td>14</td><td>13</td><td>12</td><td>11</td><td>10</td><td>9</td><td>8</td><td>7</td><td>6</td><td>5</td><td>4</td><td>3</td><td>2</td><td>1</td><td>0</td></tr><tr><td colspan="6">Reserved</td><td>EWI</td><td colspan="2">WDGTB[1:0]</td><td colspan="7">W[6:0]</td></tr></table>

<table><tr><td>位</td><td>名称</td><td>访问</td><td>描述</td><td>复位值</td></tr><tr><td>[15:10]</td><td>Reserved</td><td>R0</td><td>保留。</td><td>0</td></tr><tr><td>9</td><td>EWI</td><td>RW1</td><td>提前唤醒中断使能位。若此位置1,则在计数器的值达到0x40时产生中断。此位只能在复位后由硬件请0。</td><td>0</td></tr><tr><td>[8:7]</td><td>WDGTB[1:0]</td><td>RW</td><td>窗口看门狗时钟分频选择:00:1分频,计数时基 = PCLK1/4096;01:2分频,计数时基 = PCLK1/4096/2;10:4分频,计数时基 = PCLK1/4096/4;11:8分频,计数时基 = PCLK1/4096/8。</td><td>00b</td></tr><tr><td>[6:0]</td><td>W[6:0]</td><td>RW</td><td>窗口看门狗7位窗口值。用来与计数器的值做比较。喂狗操作只能在计数器的值小于窗口值且大于0x3F时进行。</td><td>7Fh</td></tr></table>

### 8.3.3 WWDG 状态寄存器（WWDG_STATR）

偏移地址： $0 \times 0 8$

<table><tr><td>15</td><td>14</td><td>13</td><td>12</td><td>11</td><td>10</td><td>9</td><td>8</td><td>7</td><td>6</td><td>5</td><td>4</td><td>3</td><td>2</td><td>1</td><td>0</td></tr><tr><td colspan="15">Reserved</td><td>EWIF</td></tr></table>

<table><tr><td>位</td><td>名称</td><td>访问</td><td>描述</td><td>复位值</td></tr><tr><td>[15:1]</td><td>Reserved</td><td>W0</td><td>保留。</td><td>0</td></tr><tr><td>0</td><td>EWIF</td><td>RWO</td><td>提前唤醒中断标志位。
当计数器到达0x40时，此位会被硬件置位，必须通过软件清0，用户置位是无效的。即使EWI未被置位，此位在事件发生时仍会照常被置位。</td><td>0</td></tr></table>


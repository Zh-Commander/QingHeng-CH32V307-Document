# 第 7 章 独立看门狗（IWDG）

本章模块描述适用于CH32F2x、CH32V2x和 $_ { C H 3 2 V 3 x }$ 微控制器全系列产品。

系统设有独立看门狗（IWDG）用来检测逻辑错误和外部环境干扰引起的软件故障。IWDG时钟源来自于 LSI，可独立于主程序之外运行，适用于对精度要求低的场合。

## 7.1 主要特征

12 位自减型计数器  
$\bullet$ 时钟来源 LSI 分频，可以在低功耗模式下运行  
复位条件：计数器值减到0

## 7.2 功能说明

### 7.2.1 原理和用法

独立看门狗的时钟来源LSI时钟分频，其功能在停机和待机模式时仍能正常工作。当看门狗计数器自减到0时，将会产生系统复位，所以超时时间为（重装载值 $\cdot ^ { + 1 }$ ）个时钟。

![](images/fc2b2dd3c8ff6f58bfa3c4d24e2b8e92f24223338c05f5de7d34ed4187908192.jpg)  
图7-1 独立看门狗的结构框图

#### 启动独立看门狗

系统复位后，看门狗处于关闭状态，向 IWDG_CTLR 寄存器写 $0 \times 0 0 0 0$ 开启看门狗，随后它不能再被关闭，除非发生复位。

如果在用户选择字开启了硬件独立看门狗使能位（IWDG_SW），在微控制器复位后将固定开启 IWDG。

#### 看门狗配置

看门狗内部是一个递减运行的12位计数器，当计数器的值减为0 时，将发生系统复位。开启 IWDG功能，需要执行下面几点操作：

1) 计数时基：IWDG 时钟来源 LSI，通过 IWDG_PSCR 寄存器设置 LSI 分频值时钟作为 IWDG 的计数时基。操作方法先向IWDG_CTLR寄存器写 $0 \times 5 5 5 5$ ，再修改 IWDG_PSCR 寄存器中的分频值。IWDG_STATR寄存器中的 PVU 位指示了分频值更新状态，在更新完成的情况下才可以进行分频值的修改和读出。  
2) 重装载值：用于更新独立看门狗中计数器当前值，并且计数器由此值进行递减。操作方法先向IWDG_CTLR 寄存器写 $0 \times 5 5 5 5$ ，再修改 IWDG_RLDR 寄存器设置目标重装载值。IWDG_STATR 寄存器中的 RVU位指示了重装载值更新状态，在更新完成的情况下才可以进行 IWDG_RLDR寄存器的修改和读出。  
3) 看门狗使能：向 IWDG_CTLR 寄存器写 $0 \times 0 0 0 0$ ，即可开启看门狗功能。  
4) 喂狗：即在看门狗计数器递减到 0 前刷新当前计数器值防止发生系统复位。向 IWDG_CTLR 寄存器写0xAAAA，让硬件将IWDG_RLDR寄存值更新到看门狗计数器中。此动作需要在看门狗功能开启后

定时执行，否则会出现看门狗复位动作。

### 7.2.2 调试模式

系统进入调试模式时，可以由调试模块寄存器配置IWDG的计数器继续工作或停止。

## 7.3 寄存器描述

表 7-1 IWDG 相关寄存器列表  

<table><tr><td>名称</td><td>访问地址</td><td>描述</td><td>复位值</td></tr><tr><td>R16_IWDG_CTLR</td><td>0x40003000</td><td>控制寄存器</td><td>0x0000</td></tr><tr><td>R16_IWDG_PSCR</td><td>0x40003004</td><td>分频因子寄存器</td><td>0x0000</td></tr><tr><td>R16_IWDG_RLDR</td><td>0x40003008</td><td>重装载值寄存器</td><td>0x0FFF</td></tr><tr><td>R16_IWDG_STAT</td><td>0x4000300C</td><td>状态寄存器</td><td>0x0000</td></tr></table>

### 7.3.1 IWDG 控制寄存器（IWDG_CTLR）

偏移地址： $0 \times 0 0$

15 14 13 12 11 10 9 8 7 6 5 4 3 2 1 0

KEY[15:0]

<table><tr><td>位</td><td>名称</td><td>访问</td><td>描述</td><td>复位值</td></tr><tr><td>[15:0]</td><td>KEY[15:0]</td><td>WO</td><td>操作键值锁。0xAAAA:喂狗。加载IWDG_RLDR寄存器值到独立看门狗计数器中;0x5555:允许修改R16_IWDG_PSCR和R16_IWDG_RLDR寄存器;0xCC:启动看门狗,如果启用了硬件看门狗(用户选择字配置)则不受这个限制。</td><td>0</td></tr></table>

### 7.3.2 分频因子寄存器（IWDG_PSCR）

偏移地址： $0 \times 0 4$

15 14 13 12 11 10 9 8 7 6 5 4 3 2 1 0

<table><tr><td>Reserved</td><td>PR[2:0]</td></tr></table>

<table><tr><td>位</td><td>名称</td><td>访问</td><td>描述</td><td>复位值</td></tr><tr><td>[15:3]</td><td>Reserved</td><td>R0</td><td>保留。</td><td>0</td></tr><tr><td>[2:0]</td><td>PR[2:0]</td><td>RW</td><td>IWDG时钟分频系数,修改此域前要向KEY中写0x5555。000:4分频; 001:8分频;010:16分频; 011:32分频;100:64分频; 101:128分频;110:256分频; 111:256分频。IWDG计数时基=LSI/分频系数。注:读该域值前,要确保IWDG_STAT寄存器中的PVU位为0,否则读出值无效。</td><td>000b</td></tr></table>

### 7.3.3 重装载值寄存器（IWDG_RLDR）

偏移地址： $0 \times 0 8$

<table><tr><td>15</td><td>14</td><td>13</td><td>12</td><td>11</td><td>10</td><td>9</td><td>8</td><td>7</td><td>6</td><td>5</td><td>4</td><td>3</td><td>2</td><td>1</td><td>0</td></tr><tr><td colspan="4">Reserved</td><td colspan="12">RL[11:0]</td></tr></table>

<table><tr><td>位</td><td>名称</td><td>访问</td><td>描述</td><td>复位值</td></tr><tr><td>[15:12]</td><td>Reserved</td><td>R0</td><td>保留。</td><td>0</td></tr><tr><td>[11:0]</td><td>RL[11:0]</td><td>RW</td><td>计数器重装载值。修改此域前要向KEY中写0x5555。当向KEY中写0xAAAA后,此域的值将会被硬件装载到计数器中,随后计数器从这个值开始递减计数。注:读写该域值前,要确保IWDG_STAT寄存器中的RVU位为0,否则读写此域无效。</td><td>FFFh</td></tr></table>

注：此寄存器在待机模式下会被复位。

### 7.3.4 状态寄存器（IWDG_STATR）

偏移地址： $0 \times 0 0$

<table><tr><td>15</td><td>14</td><td>13</td><td>12</td><td>11</td><td>10</td><td>9</td><td>8</td><td>7</td><td>6</td><td>5</td><td>4</td><td>3</td><td>2</td><td>1</td><td>0</td></tr><tr><td colspan="14">Reserved</td><td>RVU</td><td>PVU</td></tr></table>

<table><tr><td>位</td><td>名称</td><td>访问</td><td>描述</td><td>复位值</td></tr><tr><td>[15:2]</td><td>Reserved</td><td>R0</td><td>保留。</td><td>0</td></tr><tr><td>1</td><td>RVU</td><td>R0</td><td>重装值更新标志位。硬件置位或清0。1:重装载值更新正在进行中;0:重装载更新结束(最多5个LSI周期)。注:重装载值寄存器IWDG_RLDR只有在RVU位被清0后才可读写访问。</td><td>0</td></tr><tr><td>0</td><td>PVU</td><td>R0</td><td>时钟分频系数更新标志位。硬件置位或清0。1:时钟分频值更新正在进行中;0:时钟分频值更新结束(最多5个LSI周期)。注:分频因子寄存器IWDG_PSCR只有在PVU位被清0后才可读写访问。</td><td>0</td></tr></table>

注：在预分频或重装值更新后，不必等待RVU或PVU复位，可继续执行下面的代码。（即使在低功耗模式下，此写操作仍会被继续执行完成。）


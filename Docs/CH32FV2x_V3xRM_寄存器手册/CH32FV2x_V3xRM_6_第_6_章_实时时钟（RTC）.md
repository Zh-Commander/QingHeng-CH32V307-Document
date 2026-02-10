# 第 6 章 实时时钟（RTC）

本章模块描述适用于CH32F2x、CH32V2x和CH32V3x微控制器全系列产品。

实时时钟（RTC）是一个独立的定时器模块，其可编程计数器最大可达到 32位，配合软件即可以实现实时时钟功能，并且可以修改计数器的值来重新配置系统的当前时间和日期。RTC模块在后备供电区域，系统复位和待机模式唤醒对其不造成影响。

## 6.1 主要特征

$\bullet$ 最高为 $2 ^ { 2 0 }$ 的预分频系数  
$\bullet$ 32位可编程计数器  
$\bullet$ 多种时钟源，中断  
$\bullet$ 独立复位

## 6.2 功能描述

### 6.2.1 概述

![](images/aac205240ce62937478e1599087cd2236a3d2e9b224f5dbf552209956083aefc.jpg)  
图 6-1 RTC 结构框图

由图 6-1 所示，RTC 模块主要是 APB1 总线接口、分频器和计数器、控制和状态寄存器三部分组成，其中分频器和计数器部分在后备区域，可由 $V _ { \mathsf { B A T } }$ 供电。RTCCLK输入分频器（RTC_DIV）之后，被分频成TR_CLK。值得注意的是，分频器（RTC_DIV）的内部是一个自减计数器，自减到溢出就会输出一个TR_CLK，然后从重装值寄存器（RTC_PSCR）里取出预设值重装到分频器里，读分频器实际上是读取它的实时值（read only），写分频系数应该写到重装值寄存器（RTC_PSCR）里。一般 TR_CLK 的周期被设置为 1 秒，TR_CLK 会触发秒事件，同时会使主计数器（RTC_CNT）自增 1；当主计数器增加到和

闹钟寄存器的值一致时，会触发闹钟事件；当主计数器自增到溢出时，会触发溢出事件。以上三种事件都可以触发中断，并对应相应中断使能位控制。

### 6.2.2 复位

由于实时时钟的特殊用途，其处于后备域的四组寄存器：预分频，预分频重装值，主计数器和闹钟，只能通过后备域的复位信号复位，参照 RCC 的后备域复位章节。实时时钟的控制寄存器受系统复位或电源复位控制。

### 6.2.3 较特别的读写寄存器操作

由于实时时钟的特殊用处，RTC 和 APB1 总线是独立的，APB1 对 RTC 的读取不一定是实时的，通过APB1读取RTC的寄存器必须在APB1启动后并经过了一个 RTC上升沿，这种情形可能出现在系统复位和电源复位之后、从待机或者停机模式唤醒后。方便的做法是等待控制寄存器（CTLR）的 RSF位被置高。对 RTC 的写操作器必须等上一个写操作结束，且必须进入配置模式，具体的步骤为：

1） 查询 RTOFF 位，直到其变为 1；  
2） 置 CNF 位，进入配置模式；  
3） 对一个或者多个 RTC 寄存器进行写操作；  
4） 清 CNF 位，退出配置模式，APB1 接口开始对 RTC 寄存器进行写入；  
5） 查询 RTOFF 位，直到其变为 1 即为写完；

## 6.3 寄存器描述

表 6-1 RTC 相关寄存器列表  

<table><tr><td>名称</td><td>访问地址</td><td>描述</td><td>复位值</td></tr><tr><td>R16_RTC_CTLRH</td><td>0x40002800</td><td>RTC控制寄存器高位</td><td>0x0000</td></tr><tr><td>R16_RTC_CTLRL</td><td>0x40002804</td><td>RTC控制寄存器低位</td><td>0x0000</td></tr><tr><td>R16_RTC_PSCRH</td><td>0x40002808</td><td>预分频器重装值寄存器高位</td><td>0x0000</td></tr><tr><td>R16_RTC_PSCRL</td><td>0x4000280C</td><td>预分频器重装值寄存器低位</td><td>0x0000</td></tr><tr><td>R16_RTC_DIVH</td><td>0x40002810</td><td>分频器寄存器高位</td><td>0x0000</td></tr><tr><td>R16_RTC_DIVL</td><td>0x40002814</td><td>分频器寄存器低位</td><td>0x0000</td></tr><tr><td>R16_RTC_CNTH</td><td>0x40002818</td><td>RTC计数器高位</td><td>0x0000</td></tr><tr><td>R16_RTC_CNTL</td><td>0x4000281C</td><td>RTC计数器低位</td><td>0x0000</td></tr><tr><td>R16_RTC_ALRMH</td><td>0x40002820</td><td>闹钟寄存器高位</td><td>0xFFFF</td></tr><tr><td>R16_RTC_ALRML</td><td>0x40002824</td><td>闹钟寄存器低位</td><td>0xFFFF</td></tr></table>

### 6.3.1 RTC 控制寄存器高位（RTC_CTLRH）

偏移地址： $0 \times 0 0$

15 14 13 12 11 10 9 8 7 6 5 4 3 2 1 0

<table><tr><td>Reserved</td><td>OWIE</td><td>ALRIE</td><td>SECIE</td></tr></table>

<table><tr><td>位</td><td>名称</td><td>访问</td><td>描述</td><td>复位值</td></tr><tr><td>[15:3]</td><td>Reserved</td><td>R0</td><td>保留。</td><td>0</td></tr><tr><td>2</td><td>OWIE</td><td>RW</td><td>溢出中断使能位。</td><td>0</td></tr><tr><td>1</td><td>ALRIE</td><td>RW</td><td>闹钟中断使能位。</td><td>0</td></tr><tr><td>0</td><td>SECIE</td><td>RW</td><td>秒中断使能位。</td><td>0</td></tr></table>

### 6.3.2 RTC 控制寄存器低位（RTC_CTLRL）

偏移地址： $0 \times 0 4$

<table><tr><td>Reserved</td><td>RTOFF</td><td>CNF</td><td>RSF</td><td>OWF</td><td>ALRF</td><td>SECF</td></tr></table>

<table><tr><td>位</td><td>名称</td><td>访问</td><td>描述</td><td>复位值</td></tr><tr><td>[15:6]</td><td>Reserved</td><td>R0</td><td>保留。</td><td>0</td></tr><tr><td>5</td><td>RTOFF</td><td>R0</td><td>RTC操作状态指示位,表示对RTC的最后一次操作的执行状态,对RTC的操作必须等待此位为1。1:上一次对RTC的操作已经完成;0:上一次对RTC的操作还在进行中。</td><td>1</td></tr><tr><td>4</td><td>CNF</td><td>RW</td><td>配置标志位,将此位写1进入配置模式,从而允许向计数器(R16_RTC_CNTx)、闹钟寄存器(R16_RTC_ALRMx)和预分频器重装值寄存器(R16_RTC_PSCRx)写入值.只有将该位写1并重新被软件清0后才会执行写的操作:1:进入配置模式;0:退出配置模式,开始更新RTC寄存器。</td><td>0</td></tr><tr><td>3</td><td>RSF</td><td>RWO</td><td>寄存器同步标志位,在对RTC模块的预分频(PSCRx)、闹钟(ALRMx)、计数器(CNTx)这些寄存器进行读写前,都要先保证这个位已经被硬件置位,以确定这些寄存器已经被同步;在进行读写这些寄存器时,或者APB1复位或APB1时钟停止后,第一步应该将此位复位。1:寄存器已被同步;0:寄存器未被同步。</td><td>0</td></tr><tr><td>2</td><td>OWF</td><td>RWO</td><td>计数器溢出标志,当32位计数器溢出时,此位由硬件置位。如果置位了OWIE位,还会产生一个溢出中断。此位只能由软件清零,不能被软件置位。</td><td>0</td></tr><tr><td>1</td><td>ALRF</td><td>RWO</td><td>闹钟标志,当计数器的值达到闹钟寄存器(ALRMx)的值,此位会被硬件置位,如果闹钟中断使能位(ALRIE)置位,还会产生一个闹钟中断。此位只能由软件清零,不能被软件置位。</td><td>0</td></tr><tr><td>0</td><td>SECF</td><td>RWO</td><td>秒事件标志,当时钟经过预分频器分频后每产生一个下降沿,就会使计数器自增一,同时产生一个秒事件,此位会被置位,如果秒中断被使能(SECIE被置位),同时还会产生一个秒中断。此位只能由软件清零,不能被软件置位。</td><td>0</td></tr></table>

### 6.3.3 预分频器重装值寄存器高位（RTC_PSCRH）

偏移地址： $0 \times 0 8$

<table><tr><td>15</td><td>14</td><td>13</td><td>12</td><td>11</td><td>10</td><td>9</td><td>8</td><td>7</td><td>6</td><td>5</td><td>4</td><td>3</td><td>2</td><td>1</td><td>0</td></tr><tr><td colspan="12">Reserved</td><td colspan="4">PRL [19:16]</td></tr></table>

<table><tr><td>位</td><td>名称</td><td>访问</td><td>描述</td><td>复位值</td></tr><tr><td>[15:4]</td><td>Reserved</td><td>R0</td><td>保留。</td><td>0</td></tr><tr><td>[3:0]</td><td>PRL[19:16]</td><td>W0</td><td>重装值高位。</td><td>0</td></tr></table>

### 6.3.4 预分频器重装值寄存器低位（RTC_PSCRL）

偏移地址： $0 \times 0 0$

<table><tr><td>15</td><td>14</td><td>13</td><td>12</td><td>11</td><td>10</td><td>9</td><td>8</td><td>7</td><td>6</td><td>5</td><td>4</td><td>3</td><td>2</td><td>1</td><td>0</td></tr><tr><td colspan="16">PRL[15:0]</td></tr></table>

<table><tr><td>位</td><td>名称</td><td>访问</td><td>描述</td><td>复位值</td></tr><tr><td>[15:0]</td><td>PRL[15:0]</td><td>WO</td><td>重装值低位。实际的分频系数就是(PRL[19:0]+1),比如如果RTC输入频率为32768Hz,那么这个值设为0x7fff就可以分频出1秒周期的信号。</td><td>8000h</td></tr></table>

### 6.3.5 分频器寄存器高位（RTC_DIVH）

偏移地址：0x10

<table><tr><td>15</td><td>14</td><td>13</td><td>12</td><td>11</td><td>10</td><td>9</td><td>8</td><td>7</td><td>6</td><td>5</td><td>4</td><td>3</td><td>2</td><td>1</td><td>0</td></tr><tr><td colspan="12">Reserved</td><td colspan="4">DIV[19:16]</td></tr></table>

<table><tr><td>位</td><td>名称</td><td>访问</td><td>描述</td><td>复位值</td></tr><tr><td>[15:4]</td><td>Reserved</td><td>R0</td><td>保留。</td><td>0</td></tr><tr><td>[3:0]</td><td>DIV[19:16]</td><td>R0</td><td>分频器寄存器高位。</td><td>0</td></tr></table>

### 6.3.6 分频器寄存器低位（RTC_DIVL）

偏移地址： $0 \times 1 4$

<table><tr><td>15</td><td>14</td><td>13</td><td>12</td><td>11</td><td>10</td><td>9</td><td>8</td><td>7</td><td>6</td><td>5</td><td>4</td><td>3</td><td>2</td><td>1</td><td>0</td></tr><tr><td colspan="16">DIV[15:0]</td></tr></table>

<table><tr><td>位</td><td>名称</td><td>访问</td><td>描述</td><td>复位值</td></tr><tr><td>[15:0]</td><td>DIV[15:0]</td><td>RO</td><td>分频器寄存器低位。DIV实际上是一个自减计数器，RTC_CLK每来一个时钟DIV计数器就会减1，溢出后就会输出一个TR_CLK，同时从PSCR中重装载值。DIV只能读取，读出的是当前分频器的计数器的剩余值。</td><td>8000h</td></tr></table>

### 6.3.7 RTC 计数器高位（RTC_CNTH）

偏移地址： $0 \times 1 8$

<table><tr><td>15</td><td>14</td><td>13</td><td>12</td><td>11</td><td>10</td><td>9</td><td>8</td><td>7</td><td>6</td><td>5</td><td>4</td><td>3</td><td>2</td><td>1</td><td>0</td></tr><tr><td colspan="16">CNT[31:16]</td></tr></table>

<table><tr><td>位</td><td>名称</td><td>访问</td><td>描述</td><td>复位值</td></tr><tr><td>[15:0]</td><td>CNT[31:16]</td><td>RW</td><td>计数器高位。</td><td>0</td></tr></table>

### 6.3.8 RTC 计数器低位（RTC_CNTL）

偏移地址： ${ 0 } { \times 1 } { 0 }$

<table><tr><td>15</td><td>14</td><td>13</td><td>12</td><td>11</td><td>10</td><td>9</td><td>8</td><td>7</td><td>6</td><td>5</td><td>4</td><td>3</td><td>2</td><td>1</td><td>0</td></tr><tr><td colspan="16">CNT[15:0]</td></tr></table>

<table><tr><td>位</td><td>名称</td><td>访问</td><td>描述</td><td>复位值</td></tr><tr><td>[15:0]</td><td>CNT[15:0]</td><td>RW</td><td>计数器低位，RTC定时器的核心器件，由TRCLK（周期一般设为1秒）提供时钟。通过读取CNT[31:0]来计算出当前的时间。写这个值需要进入配置模式。</td><td>0</td></tr></table>

### 6.3.9 闹钟寄存器高位（RTC_ALRMH）

偏移地址： $_ { 0 \times 2 0 }$

<table><tr><td>15</td><td>14</td><td>13</td><td>12</td><td>11</td><td>10</td><td>9</td><td>8</td><td>7</td><td>6</td><td>5</td><td>4</td><td>3</td><td>2</td><td>1</td><td>0</td></tr><tr><td colspan="16">ALR[31:16]</td></tr></table>

<table><tr><td>位</td><td>名称</td><td>访问</td><td>描述</td><td>复位值</td></tr><tr><td>[15:0]</td><td>ALR[31:16]</td><td>WO</td><td>闹钟寄存器高位。</td><td>FFFFh</td></tr></table>

### 6.3.10 闹钟寄存器低位（RTC_ALRML）

偏移地址： ${ 0 } { \times 2 4 }$

<table><tr><td>15</td><td>14</td><td>13</td><td>12</td><td>11</td><td>10</td><td>9</td><td>8</td><td>7</td><td>6</td><td>5</td><td>4</td><td>3</td><td>2</td><td>1</td><td>0</td></tr><tr><td colspan="16">ALR[15:0]</td></tr></table>

<table><tr><td>位</td><td>名称</td><td>访问</td><td>描述</td><td>复位值</td></tr><tr><td>[15:0]</td><td>ALR[15:0]</td><td>WO</td><td>闹钟寄存器低位。当闹钟寄存器ALRM[31:0]的值和计数器CNT[31:0]的值一致时会产生一个闹钟事件。更改这个值需要进入配置模式。</td><td>FFFFh</td></tr></table>


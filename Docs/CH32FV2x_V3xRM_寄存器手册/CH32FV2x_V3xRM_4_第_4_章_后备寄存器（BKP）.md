# 第 4 章 后备寄存器（BKP）

本章模块描述适用于CH32F2x、 $_ { C H 3 2 V 2 x }$ 和 $_ { C H 3 2 V 3 x }$ 微控制器全系列产品。

后备寄存器（BKP）提供了最大42个16 位的后备数据寄存器，最大可以用来存储 84字节的用户数据。在主电源（ $( \mathsf { V } _ { \mathsf { D } \mathsf { D } } )$ ）掉电后，这些数据仍可以由 $V _ { \mathsf { B A T } }$ 供电而保持，不受待机状态、系统复位或电源复位的影响。此外BKP单元还提供了侵入检测管理、RTC时钟校准及脉冲输出功能。

## 4.1 主要特征

$\bullet$ 侵入检测（TAMPER）功能  
$\bullet$ RTC时钟校准功能  
在 PC13 引脚上输出 RTC 时钟 64 分频，闹钟脉冲或者秒脉冲

## 4.2 功能说明

微控制器复位后对后备寄存器和 RTC 的访问被禁止，需通过以下操作开启对后备寄存器的访问：

1）置寄存器RCC_APB1PCENR的PWREN位和 BKPEN位来打开电源和后备接口的操作时钟；  
2）置电源控制寄存器 PWR_CTLR 的 DBP 位，使能对后备寄存器和 RTC 寄存器的访问。

### 4.2.1 后备数据寄存器

后备数据寄存器可以作为通用数据缓存使用，由于其在 $\mathsf { V } _ { \mathsf { D } \mathsf { D } }$ 掉电下靠 $V _ { \mathsf { B A T } }$ 电源保存数据的特性，可以用来存一些重要的或敏感的数据。但这些数据在产生侵入事件后会被全部清除。

### 4.2.2 侵入检测

侵入检测就是当外界提供了一个信号（上升沿或下降沿）时，表示有“侵入事件”，硬件将自动清除当前系统中保留的重要信息。这种方式可以增加系统信息的安全性。

当侵入检测引脚上出现跳变沿（取决于 TPAL 位）时会产生一个侵入事件，如果使能了侵入检测中断，还会同时产生一个侵入检测中断。只要出现了侵入事件，后备数据寄存器就会被全部清除。此外，硬件检测采用记忆方式，即使侵入检测功能未开启（ $\mathsf { T P E } = 0$ ），系统也会采样是否有跳变沿，并在满足 TPAL 位选择情况下，提前锁定侵入事件，并在 TPE 位置 1 下，触发侵入事件。

例如：当 TPAL $\mathtt { = 0 }$ 时，如果TPE=0未开启功能，但 TAMPER 引脚已经为高电平，一旦 ${ \mathsf { T P E } } { = } 1$ 后，则会产生一个额外的侵入事件（系统提前锁定了上升沿）。当 TPAL=1 时，如果 $\mathtt { T P E = 0 }$ 未开启功能，但TAMPER 引脚已经为低电平，一旦 ${ \mathsf { T P E } } { = } 1$ 后，则会产生一个额外的侵入事件（系统提前锁定了下降沿）。

所以为了防止发生不必要的侵入事件，导致清除了后备寄存器，建议：在希望硬件检测侵入引脚的开始时刻，通过写 BKP_TPCSR 寄存器 CTE 位置 1，先清除硬件可能记忆过的侵入事件，并确保当前侵入检测引脚状态是无效的。

注：当 $V _ { D D }$ 电源断开时，侵入检测功能仍然有效。为了避免不必要的复位数据后备寄存器，TAMPER引脚应该在片外连接到正确的电平。

### 4.2.3 RTC 校准

此功能必须配置侵入检测引脚作为普通 IO口使用。配置 BKP_TPCTLR 寄存器TPE位清 0。

脉冲输出

配置 BKP_OCTLR 寄存器的 ASOE 位，开启 RTC 脉冲输出，设置 ASOS 位，选择秒脉冲输出还是闹钟脉冲输出。

RTC 校准

配置BKP_OCTLR寄存器的CCO位后，内部的RTC时钟将经过64分频后输出到侵入检测引脚（TAMPER）

上。通过实际测试，软件配合修改 CAL[6:0]位来调整时钟对 RTC 进行校准。

### 4.2.4 BKP 接口复位

BKP 区域可以在 $\mathsf { V } _ { \mathsf { D } \mathsf { D } }$ 主电源掉电下，由 $V _ { \mathsf { B A T } }$ 独立供电。应用代码控制 BKP区域寄存器复位中，后备数据寄存器 BKP_DATAR1-10、ASOS 位、ASOE 位在软件配置 RCC_BDCTLR 寄存器的 BDRST 位下复位，不受 RCC 外设接口控制 BKPRST 位影响。

## 4.3 寄存器描述

表 4-1 BKP 相关寄存器列表  

<table><tr><td>名称</td><td>访问地址</td><td>描述</td><td>复位值</td></tr><tr><td>R16_BKP_DATAR1</td><td>0x40006C04</td><td>后备数据寄存器1</td><td>0x0000</td></tr><tr><td>R16_BKP_DATAR2</td><td>0x40006C08</td><td>后备数据寄存器2</td><td>0x0000</td></tr><tr><td>R16_BKP_DATAR3</td><td>0x40006C0C</td><td>后备数据寄存器3</td><td>0x0000</td></tr><tr><td>R16_BKP_DATAR4</td><td>0x40006C10</td><td>后备数据寄存器4</td><td>0x0000</td></tr><tr><td>R16_BKP_DATAR5</td><td>0x40006C14</td><td>后备数据寄存器5</td><td>0x0000</td></tr><tr><td>R16_BKP_DATAR6</td><td>0x40006C18</td><td>后备数据寄存器6</td><td>0x0000</td></tr><tr><td>R16_BKP_DATAR7</td><td>0x40006C1C</td><td>后备数据寄存器7</td><td>0x0000</td></tr><tr><td>R16_BKP_DATAR8</td><td>0x40006C20</td><td>后备数据寄存器8</td><td>0x0000</td></tr><tr><td>R16_BKP_DATAR9</td><td>0x40006C24</td><td>后备数据寄存器9</td><td>0x0000</td></tr><tr><td>R16_BKP_DATAR10</td><td>0x40006C28</td><td>后备数据寄存器10</td><td>0x0000</td></tr><tr><td>R16_BKP_OCTLR</td><td>0x40006C2C</td><td>RTC校准寄存器</td><td>0x0000</td></tr><tr><td>R16_BKP_TPCTLR</td><td>0x40006C30</td><td>侵入检测控制寄存器</td><td>0x0000</td></tr><tr><td>R16_BKPTPCSR</td><td>0x40006C34</td><td>侵入检测状态寄存器</td><td>0x0000</td></tr><tr><td>R16_BKP_DATAR11</td><td>0x40006C40</td><td>后备数据寄存器11</td><td>0x0000</td></tr><tr><td>R16_BKP_DATAR12</td><td>0x40006C44</td><td>后备数据寄存器12</td><td>0x0000</td></tr><tr><td>R16_BKP_DATAR13</td><td>0x40006C48</td><td>后备数据寄存器13</td><td>0x0000</td></tr><tr><td>R16_BKP_DATAR14</td><td>0x40006C4C</td><td>后备数据寄存器14</td><td>0x0000</td></tr><tr><td>R16_BKP_DATAR15</td><td>0x40006C50</td><td>后备数据寄存器15</td><td>0x0000</td></tr><tr><td>R16_BKP_DATAR16</td><td>0x40006C54</td><td>后备数据寄存器16</td><td>0x0000</td></tr><tr><td>R16_BKP_DATAR17</td><td>0x40006C58</td><td>后备数据寄存器17</td><td>0x0000</td></tr><tr><td>R16_BKP_DATAR18</td><td>0x40006C5C</td><td>后备数据寄存器18</td><td>0x0000</td></tr><tr><td>R16_BKP_DATAR19</td><td>0x40006C60</td><td>后备数据寄存器19</td><td>0x0000</td></tr><tr><td>R16_BKP_DATAR20</td><td>0x40006C64</td><td>后备数据寄存器20</td><td>0x0000</td></tr><tr><td>R16_BKP_DATAR21</td><td>0x40006C68</td><td>后备数据寄存器21</td><td>0x0000</td></tr><tr><td>R16_BKP_DATAR22</td><td>0x40006C6C</td><td>后备数据寄存器22</td><td>0x0000</td></tr><tr><td>R16_BKP_DATAR23</td><td>0x40006C70</td><td>后备数据寄存器23</td><td>0x0000</td></tr><tr><td>R16_BKP_DATAR24</td><td>0x40006C74</td><td>后备数据寄存器24</td><td>0x0000</td></tr><tr><td>R16_BKP_DATAR25</td><td>0x40006C78</td><td>后备数据寄存器25</td><td>0x0000</td></tr><tr><td>R16_BKP_DATAR26</td><td>0x40006C7C</td><td>后备数据寄存器26</td><td>0x0000</td></tr><tr><td>R16_BKP_DATAR27</td><td>0x40006C80</td><td>后备数据寄存器27</td><td>0x0000</td></tr><tr><td>R16_BKP_DATAR28</td><td>0x40006C84</td><td>后备数据寄存器28</td><td>0x0000</td></tr><tr><td>R16_BKP_DATAR29</td><td>0x40006C88</td><td>后备数据寄存器29</td><td>0x0000</td></tr><tr><td>R16_BKP_DATAR30</td><td>0x40006C8C</td><td>后备数据寄存器30</td><td>0x0000</td></tr><tr><td>R16_BKP_DATAR31</td><td>0x40006C90</td><td>后备数据寄存器31</td><td>0x0000</td></tr><tr><td>R16_BKP_DATAR32</td><td>0x40006C94</td><td>后备数据寄存器32</td><td>0x0000</td></tr><tr><td>R16_BKP_DATAR33</td><td>0x40006C98</td><td>后备数据寄存器33</td><td>0x0000</td></tr><tr><td>R16_BKP_DATAR34</td><td>0x40006C9C</td><td>后备数据寄存器34</td><td>0x0000</td></tr><tr><td>R16_BKP_DATAR35</td><td>0x40006CA0</td><td>后备数据寄存器35</td><td>0x0000</td></tr><tr><td>R16_BKP_DATAR36</td><td>0x40006CA4</td><td>后备数据寄存器36</td><td>0x0000</td></tr><tr><td>R16_BKP_DATAR37</td><td>0x40006CA8</td><td>后备数据寄存器37</td><td>0x0000</td></tr><tr><td>R16_BKP_DATAR38</td><td>0x40006CAC</td><td>后备数据寄存器38</td><td>0x0000</td></tr><tr><td>R16_BKP_DATAR39</td><td>0x40006CB0</td><td>后备数据寄存器39</td><td>0x0000</td></tr><tr><td>R16_BKP_DATAR40</td><td>0x40006CB4</td><td>后备数据寄存器40</td><td>0x0000</td></tr><tr><td>R16_BKP_DATAR41</td><td>0x40006CB8</td><td>后备数据寄存器41</td><td>0x0000</td></tr><tr><td>R16_BKP_DATAR42</td><td>0x40006CBC</td><td>后备数据寄存器42</td><td>0x0000</td></tr></table>

注：后备数据寄存器（BKP_DATARx）（x=11-42）适用于CH32F20x_D8、CH32F20x_D8C、CH32F20×_D8W、CH32V20x_D8、CH32V20x_D8W、CH32V30x_D8、CH32V30x_D8C。

### 4.3.1 后备数据寄存器（BKP_DATARx）（ $\mathbf { x } = 1 - 4 2 )$

偏移地址： $\phantom { - } 0 { \times } 0 4 \ – 0 { \times } 2 8$ ， $0 { \times } 4 0 { - } 0 { \times } \mathsf { B C }$

<table><tr><td>15</td><td>14</td><td>13</td><td>12</td><td>11</td><td>10</td><td>9</td><td>8</td><td>7</td><td>6</td><td>5</td><td>4</td><td>3</td><td>2</td><td>1</td><td>0</td></tr><tr><td colspan="16">D[15:0]</td></tr></table>

<table><tr><td>位</td><td>名称</td><td>访问</td><td>描述</td><td>复位值</td></tr><tr><td>[15:0]</td><td>D[15:0]</td><td>RW</td><td>后备数据，可以被用户程序调用。
注：它们仅由后备域复位来复位（BDRST）或（如果侵入检测引脚TAMPER功能被开启时）由侵入引脚事件复位。</td><td>0</td></tr></table>

### 4.3.2 RTC 校准寄存器（BKP_OCTLR）

偏移地址： $\mathtt { 0 } \mathtt { x } 2 0$

<table><tr><td>15</td><td>14</td><td>13</td><td>12</td><td>11</td><td>10</td><td>9</td><td>8</td><td>7</td><td>6</td><td>5</td><td>4</td><td>3</td><td>2</td><td>1</td><td>0</td></tr><tr><td colspan="4">Reserved</td><td>ASOS</td><td>ASOE</td><td colspan="2">CCO</td><td colspan="8">CAL[6:0]</td></tr></table>

<table><tr><td>位</td><td>名称</td><td>访问</td><td>描述</td><td>复位值</td></tr><tr><td>[15:10]</td><td>Reserved</td><td>R0</td><td>保留。</td><td>0</td></tr><tr><td>9</td><td>ASOS</td><td>RW</td><td>TAMPER引脚闹钟/秒脉冲输出选择。1:输出秒脉冲;0:输出闹钟脉冲。注:此位只会由后备域复位(BDRST)来复位。</td><td>0</td></tr><tr><td>8</td><td>ASOE</td><td>RW</td><td>TAMPER引脚使能脉冲输出位0:禁止输出闹钟脉冲或者秒脉冲;1:使能输出闹钟脉冲或者秒脉冲。注:此位只会由后备域复位(BDRST)来复位。</td><td>0</td></tr><tr><td>7</td><td>CCO</td><td>RW</td><td>校准时钟输出选择位1:TEMPER引脚输出经64分频的RTC时钟;0:不输出校准时钟。注1:开启此功能必须关闭侵入检测功能。</td><td>0</td></tr><tr><td></td><td></td><td></td><td>注2:当VDD供电断开时,该位被清除。</td><td></td></tr><tr><td>[6:0]</td><td>CAL[6:0]</td><td>RW</td><td>校准值寄存器,这个寄存器的值表示在每220个时钟脉冲中有多少个被跳过。这个功能用来校准RTC时钟。RTC时钟可以被减慢0~121ppm。</td><td>0</td></tr></table>

### 4.3.3 侵入检测控制寄存器（BKP_TPCTLR）

偏移地址： $\mathtt { 0 } { \times } 3 0$

<table><tr><td>15</td><td>14</td><td>13</td><td>12</td><td>11</td><td>10</td><td>9</td><td>8</td><td>7</td><td>6</td><td>5</td><td>4</td><td>3</td><td>2</td><td>1</td><td>0</td></tr><tr><td colspan="14">Reserved</td><td>TPAL</td><td>TPE</td></tr></table>

<table><tr><td>位</td><td>名称</td><td>访问</td><td>描述</td><td>复位值</td></tr><tr><td>[15:2]</td><td>Reserved</td><td>R0</td><td>保留。</td><td>0</td></tr><tr><td>1</td><td>TPAL</td><td>RW</td><td>侵入检测引脚（TEMPER引脚）有效电平设置0：侵入检测引脚上的高电平会清除所有后备数据寄存器（硬件锁定上升沿）；1：侵入检测引脚上的低电平会清除所有后备数据寄存器（硬件锁定下降沿）。</td><td>0</td></tr><tr><td>0</td><td>TPE</td><td>RW</td><td>侵入检测引脚使能位0：TEMPER引脚做普通I0口用；1：TEMPER引脚做侵入检测用。</td><td>0</td></tr></table>

注：同时将TPAL和TPE位清除会产生一个假的侵入事件，推荐只在TPE为O时才改变TPAL位的状态。

### 4.3.4 侵入检测状态寄存器（BKP_TPCSR）

偏移地址： $0 \times 3 4$

<table><tr><td>15</td><td>14</td><td>13</td><td>12</td><td>11</td><td>10</td><td>9</td><td>8</td><td>7</td><td>6</td><td>5</td><td>4</td><td>3</td><td>2</td><td>1</td><td>0</td></tr><tr><td colspan="4">Reserved</td><td>TIF</td><td>TEF</td><td colspan="6">Reserved</td><td>TPIE</td><td>CTI</td><td>CTE</td><td></td></tr></table>

<table><tr><td>位</td><td>名称</td><td>访问</td><td>描述</td><td>复位值</td></tr><tr><td>[15:10]</td><td>Reserved</td><td>R0</td><td>保留。</td><td>0</td></tr><tr><td>9</td><td>TIF</td><td>R0</td><td>侵入中断标志位,当检测到侵入事件且TPIE位置1时,此位会被置位。通过向CTI位写1来清除此标志位。如果TPIE位被复位,那么此位同时也会被复位。注:仅当系统复位或由待机模式唤醒后才复位该位。</td><td>0</td></tr><tr><td>8</td><td>TEF</td><td>R0</td><td>侵入事件标志位,当检测到侵入事件时,此位会被置位。通过向CTE位写1会清除此位。注:当此位为1时,所有的BKP_DATARx寄存器的值会被清除,且在此位不复位前,所有对BKP_DATARx寄存器的写入操作都是无效的。</td><td>0</td></tr><tr><td>[7:3]</td><td>Reserved</td><td>R0</td><td>保留。</td><td>0</td></tr><tr><td>2</td><td>TPIE</td><td>RW</td><td>产生侵入中断使能位:0:禁止侵入检测中断;</td><td>0</td></tr><tr><td></td><td></td><td></td><td>1:使能侵入检测中断(TPE需置1)。注1:侵入中断无法将内核从低功耗模式唤醒。注2:仅当系统复位或由待机模式唤醒后才复位该位。</td><td></td></tr><tr><td>1</td><td>CTI</td><td>WO</td><td>侵入检测中断清除位,写1清除,读取无效。</td><td>0</td></tr><tr><td>0</td><td>CTE</td><td>WO</td><td>侵入检测事件清除位,写1清除,读取无效。</td><td>0</td></tr></table>


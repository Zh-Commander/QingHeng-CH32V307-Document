# 第 32 章 闪存及用户选择字（FLASH）

本章模块描述适用于CH32F2x、CH32V2x和CH32V3x微控制器全系列产品。

## 32.1 闪存组织

芯片内部闪存组织结构如下（以 xVCT6 为例）：

表 32-1 闪存组织结构  

<table><tr><td>块</td><td>名称</td><td>地址范围</td><td>大小(字节)</td></tr><tr><td rowspan="10">主存储器</td><td>页0</td><td>0x08000000 - 0x080000FF</td><td>256</td></tr><tr><td>页1</td><td>0x08000100 - 0x080001FF</td><td>256</td></tr><tr><td>页2</td><td>0x08000200 - 0x080002FF</td><td>256</td></tr><tr><td>页3</td><td>0x08000300 - 0x080003FF</td><td>256</td></tr><tr><td>页4</td><td>0x08000400 - 0x080004FF</td><td>256</td></tr><tr><td>页5</td><td>0x08000500 - 0x080005FF</td><td>256</td></tr><tr><td>页6</td><td>0x08000600 - 0x080006FF</td><td>256</td></tr><tr><td>页7</td><td>0x08000700 - 0x080007FF</td><td>256</td></tr><tr><td>...</td><td>...</td><td>...</td></tr><tr><td>页1919</td><td>0x08077EFF - 0x08077FFF</td><td>256</td></tr><tr><td rowspan="2">信息块</td><td>系统引导代码存储</td><td>0x1FFF8000 - 0x1FFFEFFF</td><td>28K</td></tr><tr><td>用户选择字</td><td>0x1FFFF800 - 0x1FFFF87F</td><td>128</td></tr></table>

注：

1）上述主存储器区域用于用户的应用程序存储，以4K字节（16页）单位进行写保护划分；除了“厂商配置字”区域出厂锁定，用户不可访问，其他区域在一定条件下用户可操作。  
2）在进行FLASH相关操作时，强烈建议系统主频不大于120M。

若实际应用一定要求使用系统主频大于120M，需注意：

在进行非零等待区域FLASH 和零等待区域FLASH、用户字读写以及厂商配置字和Boot区域读时，需做以下操作，首先将HCLK进行2分频（相关外设时钟也同时分频，影响需评估），FLASH操作完成后再恢复，保证FLASH访问时钟频率不超过6OMhz（FLASH_CTLR寄存器的bit[25]-可配置 FLASH 访问时钟频率为系统时钟或系统时钟的一半 该bit 默认配置为系统时钟的一半）。

## 32.2 闪存编程及安全性

### 32.2.1 两种编程/擦除方式

l 标准编程：此方式是默认编程方式（兼容方式）。这种模式下 CPU 以单次 2 字节方式执行编程，单次4K字节执行擦除及整片擦除操作。  
l 快速编程：此方式采用页操作方式（推荐）。经过特定序列解锁后，执行单次 256 字节的编程及256字节擦除、32K字节擦除、64K字节擦除及整片擦除。

### 32.2.2 安全性-防止非法访问（读、写、擦）

$\bullet$ 页写入保护  
读保护

芯片处于读保护状态下时：

1） 主存储器 0-15 页（4K 字节）自动写保护状态，不受 FLASH_WPR 寄存器控制；解除读保护状态，所有主存储页都由 FLASH_WPR 寄存器控制。  
2） 系统引导代码区、SWD 或 SDI 模式、RAM 区域都不可对主存储器进行擦除或编程，整片擦除除外。

可擦除或编程用户选择字区域。如果试图解除读保护（编程用户字），芯片将自动擦除整片用户区。

注：进行闪存的编程/擦除操作时，必须打开内部RC振荡器（HSI）。

## 32.3 FLASH 增强读模式

FLASH 增强读模式适用于 用户程序运行在 FLASH 中（用户代码空间超过用户选择 字RAM_CODE_MOD[1:0]位配置的CODE大小空间），开启该模式，可提高FLASH 访问效率。开启该模式需将 FLASH_CTLR 寄存器的 EHMOD 位置 1，关闭该模式需先将 EHMOD 位清 0，再将 RSENACT 置 1。同时可通过配置FLASH_CTLR寄存器的SCKMOD位选择访问时钟频率。

注：在使用FLASH增强读模式，需注意以下几点：

1）在对FLASH进行任何模式的擦除或编程（包括解除读保护等用户字编程）等操作之前，须先退出增强读模式，否者会导致擦除和编程操作失败；  
2）在进入停止模式之前，须先退出增强读模式，否者可能导致停止模式异常；  
3）在电源复位和系统复位结束后，芯片由硬件控制自动退出增强读模式。

## 32.4 寄存器描述

表 32-2 FLASH 相关寄存器列表  

<table><tr><td>名称</td><td>访问地址</td><td>描述</td><td>复位值</td></tr><tr><td>R32_FLASH_KEYR</td><td>0x40022004</td><td>FPEC键寄存器</td><td>X</td></tr><tr><td>R32_FLASH_OBKEYR</td><td>0x40022008</td><td>OBKEY寄存器</td><td>X</td></tr><tr><td>R32_FLASH_STATR</td><td>0x4002200C</td><td>状态寄存器</td><td>0x00000000</td></tr><tr><td>R32_FLASH_CTLR</td><td>0x40022010</td><td>配置寄存器</td><td>0x00000080</td></tr><tr><td>R32_FLASH_ADDR</td><td>0x40022014</td><td>地址寄存器</td><td>0x00000000</td></tr><tr><td>R32_FLASH_OBR</td><td>0x4002201C</td><td>选择字寄存器</td><td>0x03FFFFFFC</td></tr><tr><td>R32_FLASH_WPR</td><td>0x40022020</td><td>写保护寄存器</td><td>0xFFFFFF</td></tr><tr><td>R32_FLASH_MODEKEYR</td><td>0x40022024</td><td>扩展键寄存器</td><td>X</td></tr></table>

### 32.4.1 FPEC 键寄存器（FLASH_KEYR）

偏移地址： $0 \times 0 4$

<table><tr><td>31</td><td>30</td><td>29</td><td>28</td><td>27</td><td>26</td><td>25</td><td>24</td><td>23</td><td>22</td><td>21</td><td>20</td><td>19</td><td>18</td><td>17</td><td>16</td></tr><tr><td colspan="16">KEYR[31:16]</td></tr><tr><td>15</td><td>14</td><td>13</td><td>12</td><td>11</td><td>10</td><td>9</td><td>8</td><td>7</td><td>6</td><td>5</td><td>4</td><td>3</td><td>2</td><td>1</td><td>0</td></tr><tr><td colspan="16">KEYR[15:0]</td></tr></table>

<table><tr><td>位</td><td>名称</td><td>访问</td><td>描述</td><td>复位值</td></tr><tr><td>[31:0]</td><td>KEYR[31:0]</td><td>WO</td><td>FPEC键，用于输入FPEC的解锁键包括：RDPRT键=0x000000A5;KEY1=0x45670123;KEY2=0xCDEF89AB。</td><td>x</td></tr></table>

### 32.4.2 OBKEY 寄存器（FLASH_OBKEYR）

偏移地址： $0 \times 0 8$

<table><tr><td>31</td><td>30</td><td>29</td><td>28</td><td>27</td><td>26</td><td>25</td><td>24</td><td>23</td><td>22</td><td>21</td><td>20</td><td>19</td><td>18</td><td>17</td><td>16</td></tr><tr><td colspan="16">OBKEYR[31:16]</td></tr><tr><td>15</td><td>14</td><td>13</td><td>12</td><td>11</td><td>10</td><td>9</td><td>8</td><td>7</td><td>6</td><td>5</td><td>4</td><td>3</td><td>2</td><td>1</td><td>0</td></tr><tr><td colspan="16">OBKEYR[15:0]</td></tr></table>

<table><tr><td>位</td><td>名称</td><td>访问</td><td>描述</td><td>复位值</td></tr><tr><td>[31:0]</td><td>OBKEYR[31:0]</td><td>W0</td><td>选择字键，用于输入选择字键解除OPTWRE。</td><td>x</td></tr></table>

### 32.4.3 状态寄存器（FLASH_STATR）

偏移地址： $0 \times 0 0$

<table><tr><td>31</td><td>30</td><td>29</td><td>28</td><td>27</td><td>26</td><td>25</td><td>24</td><td>23</td><td>22</td><td>21</td><td>20</td><td>19</td><td>18</td><td>17</td><td>16</td></tr><tr><td colspan="16">Reserved</td></tr><tr><td>15</td><td>14</td><td>13</td><td>12</td><td>11</td><td>10</td><td>9</td><td>8</td><td>7</td><td>6</td><td>5</td><td>4</td><td>3</td><td>2</td><td>1</td><td>0</td></tr><tr><td colspan="6">Reserved</td><td colspan="2">EHMODS</td><td>Reser ved</td><td>EOP</td><td>WRPRT ERR</td><td colspan="2">Reserved</td><td colspan="2">WRBSY</td><td>BSY</td></tr></table>

<table><tr><td>位</td><td>名称</td><td>访问</td><td>描述</td><td>复位值</td></tr><tr><td>[31:8]</td><td>Reserved</td><td>R0</td><td>保留。</td><td>0</td></tr><tr><td>7</td><td>EHMODS</td><td>R0</td><td>FLASH 增强读模式是否开启位:1: FLASH 增强读模式开启0: FLASH 增强读模式关闭</td><td>0</td></tr><tr><td>6</td><td>Reserved</td><td>R0</td><td>保留。</td><td>0</td></tr><tr><td>5</td><td>EOP</td><td>RW1</td><td>指示操作结束,写1清零。每次成功擦除或编程时,硬件会置位。</td><td>0</td></tr><tr><td>4</td><td>WRPRTERR</td><td>RW1</td><td>指示写保护错误,写1清零。如果对写保护的地址编程时,硬件会置位。</td><td>0</td></tr><tr><td>[3:2]</td><td>Reserved</td><td>R0</td><td>保留。</td><td>0</td></tr><tr><td>1</td><td>WRBSY</td><td>R0</td><td>该位在快速页编程时使用,指示编程数据正在写入。在页编程时,当写入数据,该位被设置‘1’,硬件自动清‘0’;如果该位为‘0’,表示允许写入下个数据。</td><td>0</td></tr><tr><td>0</td><td>BSY</td><td>R0</td><td>指示忙状态:1: 表示闪存操作正在进行;0: 操作结束。</td><td>0</td></tr></table>

注：进行编程操作时，需要确定FLASH_CTLR寄存器的STRT位为0。

### 32.4.4 配置寄存器（FLASH_CTLR）

偏移地址： $0 \times 1 0$

<table><tr><td colspan="5">Reserved</td><td>SCKM
OD</td><td>EHMO
D</td><td>Reserved</td><td>RSEN
ACT</td><td>PGSTR
T</td><td>Reserved</td><td>BER
64</td><td>BER32</td><td>FTER</td><td>FTPG</td><td></td></tr><tr><td>15</td><td>14</td><td>13</td><td>12</td><td>11</td><td>10</td><td>9</td><td>8</td><td>7</td><td>6</td><td>5</td><td>4</td><td>3</td><td>2</td><td>1</td><td>0</td></tr><tr><td>FLOCK</td><td>Reserved</td><td>E0PIE</td><td>Reserved</td><td>ERRIE</td><td>OBWRE</td><td>Reserved</td><td>LOCK</td><td>STRT</td><td>OBER</td><td>OBPG</td><td>Reserved</td><td>MER</td><td>SER</td><td>PG</td><td></td></tr></table>

<table><tr><td>位</td><td>名称</td><td>访问</td><td>描述</td><td>复位值</td></tr><tr><td>[31:26]</td><td>Reserved</td><td>R0</td><td>保留。</td><td>0</td></tr><tr><td>25</td><td>SCKMOD</td><td>RW</td><td>FLASH 访问时钟配置:1:FLASH 访问时钟频率=系统时钟(SYSCLK);0:FLASH 访问时钟频率=系统时钟一半(SYSCLK/2);注:FLASH 访问时钟频率不超过72MHZ。</td><td>0</td></tr><tr><td>24</td><td>EHMOD</td><td>RW</td><td>FLASH 增强读模式:该模式,在程序运行在 FLASH 时,可提高访问效率。1:使能 FLASH 增强读模式;0:关闭 FLASH 增强读模式,需配合 RSENACT位一起操作,退出步骤先将 EHMOD 位清0,再将 RSENACT 置1。</td><td>0</td></tr><tr><td>23</td><td>Reserved</td><td>R0</td><td>保留。</td><td>0</td></tr><tr><td>22</td><td>RSENACT</td><td>WO</td><td>退出增强读模式,硬件自动清除,需配合EHMOD 位一起操作,退出步骤先将 EHMOD 位清0,再将 RSENACT 置1。</td><td>0</td></tr><tr><td>21</td><td>PGSTRT</td><td>RWO</td><td>开始。置1启动一次页编程,硬件自动清除。</td><td>0</td></tr><tr><td>20</td><td>Reserved</td><td>R0</td><td>保留。</td><td>0</td></tr><tr><td>19</td><td>BER64</td><td>RW</td><td>执行64KB擦除。</td><td>0</td></tr><tr><td>18</td><td>BER32</td><td>RW</td><td>执行32KB擦除。</td><td>0</td></tr><tr><td>17</td><td>FTER</td><td>RW</td><td>执行快速页(256Byte)擦除操作。</td><td>0</td></tr><tr><td>16</td><td>FTPG</td><td>RW</td><td>执行快速页编程操作。</td><td>0</td></tr><tr><td>15</td><td>FLOCK</td><td>RW1</td><td>快速编程锁。只能写‘1’。当该位为‘1’时表示快速编程/擦除模式不可用。在检测到正确的解锁序列后,硬件清除此位为‘0’。软件置1,重新加锁。</td><td>1</td></tr><tr><td>[14:13]</td><td>Reserved</td><td>R0</td><td>保留。</td><td>0</td></tr><tr><td>12</td><td>EOPIE</td><td>RW</td><td>操作完成中断控制(FLASH_STAT 储存器中EOP 置位):1:允许产生中断;0:禁止产生中断。</td><td>0</td></tr><tr><td>11</td><td>Reserved</td><td>R0</td><td>保留。</td><td>0</td></tr><tr><td>10</td><td>ERRIE</td><td>RW</td><td>错误状态中断控制(FLASH_STAT 寄存器中</td><td>0</td></tr><tr><td></td><td></td><td></td><td>PGERR/WRPRTERR 置位):1:允许产生中断;0:禁止产生中断。</td><td></td></tr><tr><td>9</td><td>OBWRE</td><td>RWO</td><td>用户选择字锁,软件清0:1:表示可以对用户选择字进行编程操作。需要在FLASH_OBKEYR 寄存器中写入正确序列后由硬件置位。0:软件清零后重新加锁用户选择字。</td><td>0</td></tr><tr><td>8</td><td>Reserved</td><td>R0</td><td>保留。</td><td>0</td></tr><tr><td>7</td><td>LOCK</td><td>RW1</td><td>锁。只能写‘1’。当该位为‘1’时表示FPEC和FLASH_CTLR 被锁住不可写。在检测到正确的解锁序列后,硬件清除此位为‘0’。在一次不成功的解锁操作后,直到下次系统复位前,该位不会再改变。</td><td>1</td></tr><tr><td>6</td><td>STRT</td><td>RW1</td><td>开始。置1启动一次擦除动作,硬件自动清0 (BSY 变‘0’)。</td><td>0</td></tr><tr><td>5</td><td>OBER</td><td>RW</td><td>执行用户选择字擦除</td><td>0</td></tr><tr><td>4</td><td>OBPG</td><td>RW</td><td>执行用户选择字编程</td><td>0</td></tr><tr><td>3</td><td>Reserved</td><td>R0</td><td>保留。</td><td>0</td></tr><tr><td>2</td><td>MER</td><td>RW</td><td>执行全擦除操作(擦除整个用户区)。</td><td>0</td></tr><tr><td>1</td><td>PER</td><td>RW</td><td>执行标准页(4KB)擦除操作。</td><td>0</td></tr><tr><td>0</td><td>PG</td><td>RW</td><td>执行标准编程操作。</td><td>0</td></tr></table>

### 32.4.5 地址寄存器（FLASH_ADDR）

偏移地址：0x14

<table><tr><td>31</td><td>30</td><td>29</td><td>28</td><td>27</td><td>26</td><td>25</td><td>24</td><td>23</td><td>22</td><td>21</td><td>20</td><td>19</td><td>18</td><td>17</td><td>16</td></tr><tr><td colspan="16">FAR[31:16]</td></tr><tr><td>15</td><td>14</td><td>13</td><td>12</td><td>11</td><td>10</td><td>9</td><td>8</td><td>7</td><td>6</td><td>5</td><td>4</td><td>3</td><td>2</td><td>1</td><td>0</td></tr><tr><td colspan="16">FAR[15:0]</td></tr></table>

<table><tr><td>位</td><td>名称</td><td>访问</td><td>描述</td><td>复位值</td></tr><tr><td>[31:0]</td><td>FAR[31:0]</td><td>WO</td><td>闪存地址，进行编程时为编程的地址，进行擦除时为擦除的起始地址。当FLASH_SR寄存器中的BSY位为‘1’时，不能写此寄存器。</td><td>0</td></tr></table>

### 32.4.6 选择字寄存器（FLASH_OBR）

偏移地址： ${ 0 } { \times 1 } { 0 }$

<table><tr><td>31</td><td>30</td><td>29</td><td>28</td><td>27</td><td>26</td><td>25</td><td>24</td><td>23</td><td>22</td><td>21</td><td>20</td><td>19</td><td>18</td><td>17</td><td>16</td></tr><tr><td colspan="16">Reserved</td></tr><tr><td>15</td><td>14</td><td>13</td><td>12</td><td>11</td><td>10</td><td>9</td><td>8</td><td>7</td><td>6</td><td>5</td><td>4</td><td>3</td><td>2</td><td>1</td><td>0</td></tr><tr><td colspan="5">Reserved</td><td>USERReserved</td><td>PORCTR</td><td>USBDPU</td><td>USBDMODE</td><td>STANDYRST</td><td>STOPRST</td><td>IWDGSW</td><td>RDPRT</td><td colspan="2">OBERR</td><td></td></tr></table>

<table><tr><td>位</td><td colspan="2">名称</td><td>访问</td><td>描述</td><td>复位值</td></tr><tr><td>[31:10]</td><td colspan="2">Reserved</td><td>R0</td><td>保留。</td><td>0</td></tr><tr><td>[9:8]</td><td rowspan="4">USER</td><td>RAM_CODE_MOD</td><td>R0</td><td>00: CODE-192KB + RAM-128KB01: CODE-224KB + RAM-96KB10: CODE-256KB + RAM-64KB11: CODE-288KB + RAM-32KB注:适用于CH32V303RC、CH32V303VC、CH32V307RC、CH32V307WC、CH32V307VC、CH32F203RC、CH32F203VC、CH32F207VC。00: CODE-128KB + RAM-64KB01: CODE-144KB + RAM-48KB1x: CODE-160KB + RAM-32KB注:适用于CH32V20x_D8W、CH32V20x_D8、CH32F20x_D8W。</td><td>xxb</td></tr><tr><td>[7:5]</td><td>Reserved</td><td>R0</td><td>保留。</td><td>xxb</td></tr><tr><td>4</td><td>STANDYRST</td><td>R0</td><td>待机模式下系统复位控制。</td><td>x</td></tr><tr><td>3</td><td>STOPRST</td><td>R0</td><td>停止模式下系统复位控制。</td><td>x</td></tr><tr><td>2</td><td></td><td>IWDGSW</td><td>R0</td><td>独立看门狗(IWDG)硬件使能位。</td><td>1</td></tr><tr><td>1</td><td colspan="2">RDPRT</td><td>R0</td><td>读保护状态。1:表示闪存当前读保护有效。</td><td>1</td></tr><tr><td>0</td><td colspan="2">OBERR</td><td>R0</td><td>选择字错误。1:表示选择字和它的反码不匹配。</td><td>0</td></tr></table>

注：USER和RDPRT在系统复位后从用户选择字区域加载。

### 32.4.7 写保护寄存器（FLASH_WPR）

偏移地址： $_ { 0 \times 2 0 }$

<table><tr><td>31</td><td>30</td><td>29</td><td>28</td><td>27</td><td>26</td><td>25</td><td>24</td><td>23</td><td>22</td><td>21</td><td>20</td><td>19</td><td>18</td><td>17</td><td>16</td></tr><tr><td colspan="16">WRP[31:16]</td></tr><tr><td>15</td><td>14</td><td>13</td><td>12</td><td>11</td><td>10</td><td>9</td><td>8</td><td>7</td><td>6</td><td>5</td><td>4</td><td>3</td><td>2</td><td>1</td><td>0</td></tr><tr><td colspan="16">WRP[15:0]</td></tr></table>

<table><tr><td>位</td><td>名称</td><td>访问</td><td>描述</td><td>复位值</td></tr><tr><td>[31:0]</td><td>WRP[31:0]</td><td>R0</td><td>闪存写保护状态。</td><td>x</td></tr><tr><td></td><td></td><td></td><td>1:写保护失效;
0:写保护有效。
每个比特位代表4K字节(16页)存储写保护状态。</td><td></td></tr></table>

注：WPR在系统复位后从用户选择字区域加载。

### 32.4.8 扩展键寄存器（FLASH_MODEKEYR）

偏移地址： ${ 0 } { \times 2 4 }$

<table><tr><td>31</td><td>30</td><td>29</td><td>28</td><td>27</td><td>26</td><td>25</td><td>24</td><td>23</td><td>22</td><td>21</td><td>20</td><td>19</td><td>18</td><td>17</td><td>16</td></tr><tr><td colspan="16">MODEKEYR[31:16]</td></tr><tr><td>15</td><td>14</td><td>13</td><td>12</td><td>11</td><td>10</td><td>9</td><td>8</td><td>7</td><td>6</td><td>5</td><td>4</td><td>3</td><td>2</td><td>1</td><td>0</td></tr><tr><td colspan="16">MODEKEYR[15:0]</td></tr></table>

<table><tr><td>位</td><td>名称</td><td>访问</td><td>描述</td><td>复位值</td></tr><tr><td>[31:0]</td><td>MODEKEYR[31:0]</td><td>W0</td><td>输入下面序列解锁快速编程/擦除模式:KEY1 = 0x45670123;KEY2 = 0xCDEF89AB。</td><td>x</td></tr></table>

## 32.5 闪存操作流程

### 32.5.1 读操作

在通用地址空间内进行直接寻址，任何8/16/32位数据的读操作都能访问闪存模块的内容并得到相应的数据。

### 32.5.2 解除闪存锁

系统复位后，闪存控制器（FPEC）和 FLASH_CTLR 寄存器是被锁定的，不可访问。通过写入序列到FLASH_KEYR寄存器可解锁闪存控制器模块。

解锁序列：

1） 向 FLASH_KEYR 寄存器写入 $\mathsf { K E Y 1 } ~ = ~ 0 \times 4 5 6 7 0 1 2 3$ （第 1 步必须是 KEY1）；  
2） 向 FLASH_KEYR 寄存器写入 ${ \sf K E Y 2 } = 0 { \sf { x C D E F 8 9 A B } }$ （第 2 步必须是 KEY2）。

上述操作必须按序并连续执行，否则属于错误操作，会锁死 FPEC 模块和FLASH_CTLR 寄存器并产生总线错误，直到下次系统复位。

闪存控制器（FPEC）和 FLASH_CTLR 寄存器可以通过将 FLASH_CTLR 寄存器的“LOCK”位，置 1 来再次锁定。

### 32.5.3 主存储器标准编程

标准编程每次可以写入 2 字节。当 FLASH_CTLR 寄存器的 PG 位为‘1’时，每次向闪存地址写入半字（2 字节）将启动一次编程，写入任何非半字数据，FPEC 都会产生总线错误。编程过程中，BSY位为‘1’，编程结束，BSY位为‘0’，EOP 位为‘1’。

注：当BSY位为‘1’时，将禁止对任何寄存器执行写操作。

![](images/f570d1bffea4e0ca4f24c7bcbe4677a3b70241ebba2d71bd3918ce4a7e18b45a.jpg)  
图 32-1 FLASH 编程

1）检查 FLASH_CTLR 寄存器 LOCK，如果为 1，需要执行“解除闪存锁”操作。  
2）设置 FLASH_CTLR 寄存器的 PG 位为‘1’，开启标准编程模式。  
3）向指定闪存地址（偶地址）写入要编程的半字。  
4）等待 BSY 位变为‘0’或 FLASH_STATR 寄存器的 EOP 位为‘1’表示编程结束，将 EOP 位清 0。  
5）查询 FLASH_STATR 寄存器看是否有错误，或者读编程地址数据校验。  
6）继续编程可以重复 3-5 步骤，结束编程将 PG 位清 0。

### 32.5.4 主存储器标准擦除

闪存可以按标准页（4K 字节）擦除，也可以整片擦除。

![](images/e8b4c6a43d2e0329204f1d8b7a7c24fd3bcf0e5a7fe4cfb3036465813b5b1d2f.jpg)  
图 32-2 FLASH 页擦除

1）检查 FLASH_CTLR 寄存器 LOCK 位，如果为 1，需要执行“解除闪存锁”操作。  
2）设置 FLASH_CTLR 寄存器的 PER 位为‘1’，开启标准页擦除模式。  
3）向 FLASH_ADDR 寄存器写入选择擦除的页首地址。  
4）设置 FLASH_CTLR 寄存器的 STAT 位为‘1’，启动一次擦除动作。  
5）等待 BYS 位变为‘0’或 FLASH_STATR 寄存器的 EOP 位为‘1’表示擦除结束，将 EOP 位清 0。

6）读擦除页的数据进行校验。  
7）继续标准页擦除可以重复 3-5 步骤，结束擦除将 PEG 位清 0。

注：擦除成功后，字读- $0 x e 3 3 9 e 3 3 9$ ，半字读-0xe339，偶地址字节读- $0 { x } 3 9$ ，奇地址读0xe3。

![](images/5110966697f080dc24cbc3a9590cfe248e75b0dbe62ab419d6be84656c014833.jpg)  
图 32-3 FLASH 整片擦除

1）检查 FLASH_CTLR 寄存器 LOCK 位，如果为 1，需要执行“解除闪存锁”操作。  
2）设置FLASH_CTLR寄存器的MER位为‘1’，开启整片擦除模式。  
3）设置 FLASH_CTLR 寄存器的 STAT 位为‘1’，启动擦除动作。  
4）等待 BYS 位变为‘0’或 FLASH_STATR 寄存器的 EOP 位为‘1’表示擦除结束，将 EOP 位清 0。  
5）读擦除页的数据进行校验。  
6）将 MER 位清 0。

### 32.5.5 快速编程模式解锁

通过写入序列到 FLASH_MODEKEYR 寄存器可解锁快速编程模式操作。解锁后，FLASH_CTLR 寄存器的 FLOCK 位将清 0，表示可以进行快速擦除和编程操作。通过将 FLASH_CTLR 寄存器的“FLOCK”位软件置1来再次锁定。

解锁序列：

1）向 FLASH_MODEKEYR 寄存器写入 $\mathsf { K E Y 1 } ~ = ~ 0 \times 4 5 6 7 0 1 2 3$ ；  
2）向 FLASH_MODEKEYR 寄存器写入 $\mathsf { K E Y 2 } \mathrm { ~ = ~ } 0 \times \mathsf { C D E F 8 9 A B } \mathrm { ~ . ~ }$ 。

上述操作必须按序并连续执行，否则属于错误操作会锁定，直到下次系统复位才能重新解锁。

注：快速编程操作需要解除“LOCK”和“FLOCK”两层锁定。

### 32.5.6 主存储器快速编程

快速编程按页（256 字节）进行编程。

1）检查 FLASH_CTLR 寄存器 LOCK 位，如果为‘1’，需要执行“解除闪存锁”操作。  
2）检查 FLASH_CTLR 寄存器 FLOCK 位，如果为‘1’，需要执行“快速编程模式解锁”操作。  
3）检查 FLASH_STATR 寄存器的 BSY 位，以确认没有其他正在进行的编程操作。  
4）设置 FLASH_CTLR 寄存器的 FTPG 位为‘1’，使能快速页编程模式。  
5）使用 32 位方式向 FLASH 地址写入数据，例如

*（uint32_t*） $0 \times 8 0 0 0 0 0 0 0 = \mathrm { } 0 \times 1 2 3 4 5 6 7 8 { \mathrm { ; } }$

6）等待 FLASH_STATR 寄存器的 WR_BSY 为 $" 0 "$ ’，写入下个数据。  
7）重复步骤 5-6 共 64 次。  
8）设置 FLASH_CTLR 寄存器的 PGSTRT 位为‘1’，启动快速页编程。

9）等待 BSY 位变为‘0’或 FLASH_STATR 寄存器的 EOP 位为‘1’表示一次快速页编程完成，将 EOP 位清 0。  
10）查询 FLASH_STATR 寄存器看是否有错误，或者读编程地址数据校验。  
11）继续快速页编程可以重复 5-10 步骤，结束编程将 FTPG 位清 0。

### 32.5.7 主存储器快速擦除

快速擦除按页（256字节）进行擦除。

1）检查 FLASH_CTLR 寄存器 LOCK 位，如果为 1，需要执行“解除闪存锁”操作。  
2）检查 FLASH_CTLR 寄存器 FLOCK 位，如果为 1，需要执行“快速编程模式解锁”操作。  
3）检查 FLASH_STATR 寄存器的 BSY 位，以确认没有其他正在进行的编程操作。  
4）设置 FLASH_CTLR 寄存器的 FTER 位为‘1’，开启快速页擦除（256 字节）模式功能。  
5）向 FLASH_ADDR 寄存器写入快速擦除页的首地址。  
6）设置 FLASH_CTLR 寄存器的 STAT 位为‘1’，启动一次快速页擦除（256 字节）动作。  
7）等待 BSY 位变为‘0’或 FLASH_STATR 寄存器的 EOP 位为‘1’表示擦除结束，将 EOP 位清 0。  
8）查询 FLASH_STATR 寄存器看是否有错误，或者读擦除页地址数据校验。  
9）继续快速页擦除可以重复 5-8 步骤，结束擦除将 FTER 位清 0。

注：擦除成功后，字读-0xe339e339，半字读-0xe339，偶地址字节读- $0 { x } 3 9$ ，奇地址读0xe3。

快速擦除按块（32K字节）进行擦除。

1）检查FLASH_CTLR寄存器LOCK位，如果为1，需要执行“解除闪存锁”操作。  
2）检查 FLASH_CTLR 寄存器 FLOCK 位，如果为 1，需要执行“快速编程模式解锁”操作。  
3）检查 FLASH_STATR 寄存器的 BSY 位，以确认没有其他正在进行的编程操作。  
4）设置 FLASH_CTLR 寄存器的 BER32 位为‘1’，开启快速块擦除（32K 字节）模式功能。  
5）向 FLASH_ADDR 寄存器写入快速擦除块的首地址。  
6）设置 FLASH_CTLR 寄存器的 STAT 位为‘1’，启动一次快速块擦除（32K 字节）动作。  
7）等待 BYS 位变为‘0’或 FLASH_STATR 寄存器的 EOP 位为‘1’表示擦除结束，将 EOP 位清 0。  
8）查询FLASH_STATR寄存器看是否有错误，或者读擦除页地址数据校验。  
9）继续快速页擦除可以重复 5-8 步骤，结束擦除将 BER32 位清 0。

注：擦除成功后，字读- $0 x e 3 3 9 e 3 3 9$ ，半字读-0xe339，偶地址字节读- $0 { x } 3 9$ ，奇地址读0xe3。

快速擦除按块（64K字节）进行擦除。

1）检查FLASH_CTLR寄存器LOCK位，如果为1，需要执行“解除闪存锁”操作。  
2）检查 FLASH_CTLR 寄存器 FLOCK 位，如果为 1，需要执行“快速编程模式解锁”操作。  
3）检查 FLASH_STATR 寄存器的 BSY 位，以确认没有其他正在进行的编程操作。  
4）设置 FLASH_CTLR 寄存器的 BER64 位为‘1’，开启快速块擦除（64K 字节）模式功能。  
5）向 FLASH_ADDR 寄存器写入快速擦除块的首地址。  
6）设置 FLASH_CTLR 寄存器的 STAT 位为‘1’，启动一次快速块擦除（64K 字节）动作。  
7）等待 BYS 位变为‘0’或 FLASH_STATR 寄存器的 EOP 位为‘1’表示擦除结束，将 EOP 位清 0。  
8）查询 FLASH_STATR 寄存器看是否有错误，或者读擦除页地址数据校验。  
9）继续快速页擦除可以重复 5-8 步骤，结束擦除将 BER64 位清 0。

注：擦除成功后，字读- $0 x e 3 3 9 e 3 3 9$ ，半字读-0xe339，偶地址字节读- $0 { x } 3 9$ ，奇地址读0xe3。

## 32.6 用户选择字

用户选择字固化在FLASH中，在系统复位后会被重新装载到相应寄存器，用户可以任意的进行擦除和编程。用户选择字信息块总共有8个字节（4 个字节为写保护，1个字节为读保护，1 个字节为配

置选项，2个字节存储用户数据），每个位都有其反码位用于装载过程中的校验。下面描述了选择字信息结构和意义。

表 32-3 32 位选择字格式划分  

<table><tr><td>[31:24]</td><td>[23:16]</td><td>[15:8]</td><td>[7:0]</td></tr><tr><td>选择字字节1反码</td><td>选择字字节1</td><td>选择字字节0反码</td><td>选择字字节0</td></tr></table>

表 32-4 用户选择字信息结构  

<table><tr><td>地址
位</td><td>[31:24]</td><td>[23:16]</td><td>[15:8]</td><td>[7:0]</td></tr><tr><td>0x1FFF800</td><td>nUSER</td><td>USER</td><td>nRDPR</td><td>RDPR</td></tr><tr><td>0x1FFF804</td><td>nData1</td><td>Data1</td><td>nData0</td><td>Data0</td></tr><tr><td>0x1FFF808</td><td>nWRPR1</td><td>WRPR1</td><td>nWRPRO</td><td>WRPRO</td></tr><tr><td>0x1FFF80C</td><td>nWRPR3</td><td>WRPR3</td><td>nWRPR2</td><td>WRPR2</td></tr></table>

<table><tr><td colspan="3">名称/字节</td><td>描述</td><td>复位值</td></tr><tr><td colspan="3">RDPR</td><td>读保护控制位,配置是否可以读出闪存中的代码。0xA5:若此字节为0xA5(nRDP必须为0x5A),表示当前代码处于非读保护状态,可以读出;其他值:表示代码读保护状态,不可读,0-31页(4K)将自动写保护,不受WRPRO控制。</td><td>0x01</td></tr><tr><td rowspan="5">USER</td><td>[7:6]</td><td>RAM_CODE_MOD</td><td>00:CODE-192KB + RAM-128KB01:CODE-224KB + RAM-96KB10:CODE-256KB + RAM-64KB11:CODE-288KB + RAM-32KB注:适用于CH32V30x_D8C、CH32V30x_D8、CH32F20x_D8C、CH32F20x_D8。00:CODE-128KB + RAM-64KB01:CODE-144KB + RAM-48KB1x:CODE-160KB + RAM-32KB注:适用于CH32V20x_D8W、CH32V20x_D8、CH32F20x_D8W。</td><td>xxb</td></tr><tr><td>[5:3]</td><td>Reserved</td><td>保留。</td><td>1</td></tr><tr><td>2</td><td>STANDYRST</td><td>待机模式下系统复位控制:1:不启用,进入待机模式系统不复位;0:启用,进入待机模式产生系统复位。</td><td>1</td></tr><tr><td>1</td><td>STOPRST</td><td>停止模式下系统复位控制:1:不启用,进入停止模式不复位系统;0:启用,进入停止模式产生系统复位。</td><td>1</td></tr><tr><td>0</td><td>IWDGSW</td><td>独立看门狗(IWDG)硬件使能位:1:IWDG功能由软件开启,禁止硬件开启;0:IWDG功能由硬件开启(随LSI时钟决定)。</td><td>1</td></tr><tr><td colspan="3">Data0 - Data1</td><td>存储用户数据2字节。</td><td>FFFFh</td></tr><tr><td colspan="3">WRPRO - WRPR3</td><td>写保护控制位。每个比特位用于控制主存储器中1个扇区(4K字节/扇区)的写保护状态:</td><td>FFFFFFh</td></tr><tr><td colspan="3"></td><td>1:关闭写保护;0:启用写保护。4个字节用于保护总共512K字节的主存储器。WRPR0:第0-7扇区存储写保护控制;WRPR1:第8-15扇区存储写保护控制;WRPR2:第16-23扇区存储写保护控制;WRPR3:位0-6提供第24-30扇区的写保护;位7提供第31-127扇区的写保护。</td><td></td></tr></table>

### 32.6.1 用户选择字解锁

通过写入序列到 FLASH_OBKEYR 寄存器可解锁用户选择字操作。解锁后，FLASH_CTLR 寄存器的OBWRE位将置1，表示可以进行用户选择字的擦除和编程。通过将 FLASH_CTLR寄存器的“OBWRE”位，软件清 0 来再次锁定。

解锁序列：

1）向 FLASH_OBKEYR 寄存器写入 $\mathsf { K E Y 1 } = 0 \times 4 5 6 7 0 1 2 3 ;$ ；  
2）向 FLASH_OBKEYR 寄存器写入 KEY2 = 0xCDEF89AB。

注：用户选择字操作需要解除“LOCK”和“OBWRE”两层锁定。

### 32.6.2 用户选择字编程

只支持标准编程方式，一次写入半字（2 字节）。实际过程中，对用户选择字进行编程时，FPEC只使用半字中的低字节，并自动计算出高字节（高字节为低字节的反码），然后开始编程操作，这将保证用户选择字中的字节和它的反码始终是正确的。

1）检查 FLASH_CTLR 寄存器 LOCK 位，如果为 1，需要执行“解除闪存锁”操作。  
2）检查 FLASH_STATR 寄存器的 BSY 位，以确认没有其他正在进行的编程操作。  
3）检查FLASH_CTLR寄存器OBWRE位，如果为 0，需要执行“用户选择字解锁”操作。  
4）设置 FLASH_CTLR 寄存器的 OBPG 位为‘1’，之后设置 FLASH_CTLR 寄存器的 STAT 位为‘1’，开启用户选择字编程。  
5）写入要编程的半字（2 字节）到指定地址。  
6）等待 BYS 位变为‘0’或 FLASH_STATR 寄存器的 EOP 位为‘1’表示编程结束，将 EOP 位清 0。  
7）读编程地址数据校验。  
8）继续编程可以重复 5-7 步骤，结束编程将 OBPG 位清 0。

注：当修改选择字中的“读保护”变成“非保护”状态时，会自动执行一次整片擦除主存储区操作。如果修改“读保护”之外的选型，则不会出现整片擦除的操作。

### 32.6.3 用户选择字擦除

直接擦除整个 128 字节用户选择字区域。

1）检查FLASH_CTLR寄存器LOCK位，如果为1，需要执行“解除闪存锁”操作。  
2）检查 FLASH_STATR 寄存器的 BSY 位，以确认没有正在进行的编程操作。  
3）检查FLASH_CTLR寄存器OBWRE位，如果为 0，需要执行“用户选择字解锁”操作。  
4）设置 FLASH_CTLR 寄存器的 OBER 位为‘1’，之后设置 FLASH_CTLR 寄存器的 STAT 位为‘1’，开启用户选择字擦除。  
5）等待 BYS 位变为‘0’或 FLASH_STATR 寄存器的 EOP 位为‘1’表示擦除结束，将 EOP 位清 0  
6）读擦除地址数据校验。  
7）结束将 OBER 位清 0。

注：擦除成功后，字读-0xe339e339，半字读-0xe339，字节读-0x39。

### 32.6.4 解除读保护

闪存是否读保护，由用户选择字决定。读取 FLASH_OBR 寄存器，当 RDPRT 位为‘1’表示当前闪存处于读保护状态，闪存操作上受到读保护状态的一系列安全防护。解除读保护过程如下：

1）擦除整个用户选择字区域，此时读保护字段 RDPR，此时读保护仍然有效。  
2）用户选择字编程，写入正确的 RDPR 代码 0xA5 以解除闪存的读保护。（此步骤首先将导致系统自动对闪存执行整片擦除操作）  
3）进行上电复位以重新加载选择字节（包括新的 RDPR码），此时读保护被解除。

### 32.6.5 解除写保护

闪存是否写保护，由用户选择字决定。读取 FLASH_WPR 寄存器，每个比特位代表 4K 字节闪存空间，当比特位为‘1’表示非写保护状态，为 $" 0 "$ ’表示写保护。解除写保护过程如下：

1）擦除整个用户选择字区域。  
2）写入正确的 RDPR 码 0xA5，允许读访问；  
3）进行系统复位，重新加载选择字节（包括新的 WRPR[3:0]字节），写保护被解除。


# 第 13 章 触摸按键检测（TKEY）

本章模块描述适用于CH32F2x、CH32V2x和 $_ { C H 3 2 V 3 x }$ 微控制器全系列产品。

触摸检测控制（TKEY）单元，借助ADC 模块的电压转换功能，通过将电容量转换为电压量进行采样，实现触摸按键检测功能。检测通道复用 ADC 的 16 个外部通道，通过 ADC 模块的单次转换模式实现触摸按键检测。

## 13.1 TKEY 功能描述

### TKEY 开启

TKEY检测过程需要ADC模块配合进行，所以使用 TKEY功能时，需要保证 ADC模块处于上电状态（ADON=1），然后将 ADC_CTLR1 寄存器的 TKENABLE 位置 1，打开 TKEY 单元功能，且可以通过TKITUNE 位调整 TKEY 模块的充电电流。

TKEY只支持单次单通道转换模式，将待转换的通道配置到 ADC模块的规则组序列第一个，软件启动转换（写 TKEY_ACT_DCG 寄存器）。

注：不进行TKEY转换时，仍然可以保留ADC通道配置转换功能。

![](images/5164d296b307ebcf9065bf0129bfd32f83157fc4507356465b3b28e89b22e39a.jpg)  
图 13-1 TKEY 工作时序图

### 可编程采样时间

TKEY 单元转换需要先使用若干个 ADCCLK 时钟周期（tDISCHG）进行放电，然后再通过若干个ADCCLK 周期（tCHG）对通道进行充电进行电压采样，充电周期数为 TKEY_CHARGE1 和 TKEY_CHARGE2寄存器中的 $\operatorname { T K C G } \times [ 2 : 0 ]$ 配置值加上TKEY_CHGOFFSET偏移量之和，每个通道可以分别用不同的充电周期来调整采样电压。

## 13.2 TKEY 操作步骤

TKEY检测属于ADC模块下的扩展功能，其工作原理是通过“触摸”和“非触摸”方式让硬件通道感知的电容量发生变化，进而通过可设置的充放电周期数将电容量的变化转换为电压的变化，最后通过ADC模块转换为数字值。

采样时，需要将 ADC 配置为单次单通道工作模式，由 TKEY_ACT 寄存器的“写操作”启动一次转换，具体流程如下：

1） 初始化 ADC 功能，配置 ADC 模块为单次转换模块，置 ACON 位为 1，唤醒 ADC 模块。将 ADC_CTLR1

寄存器的 TKENABLE 位置 1，打开 TKEY 单元。

2） 设置要转换的通道，将通道号写入ADC规则组序列中第一个转换位置（ADC_RSQR3[4:0]），设置RLEN[3:0]为 1。  
3） 设置通道的充电采样时间，写 TKEY_CHARGEx 寄存器，可为每个通道配置不同的充电时间。  
4） 写 TKEY_CHGOFFSET 寄存器，设置通道的充电时间偏移量（低八位有效），以调整充电时间。  
5） 写 TKEY_ACT_DCG 寄存器，设置放电时间（低八位有效），并启动一次 TKEY 的采样和转换。  
6） 等待ADC状态寄存器的EOC转换结束标志位置1，读取 ADC_DR寄存器得到此次转换值。  
7） 如果需要进行下次转换，重复 2-6 步骤。如果不需修改通道充电采样时间，可省略步骤 3 或 4。

## 13.3 TKEY 寄存器描述

表 13-1 TKEY1 相关寄存器列表  

<table><tr><td>名称</td><td>访问地址</td><td>描述</td><td>复位值</td></tr><tr><td>R32_TKEY1_CHARGE1</td><td>0x4001240C</td><td>TKEY 充电采样时间寄存器 1</td><td>0x00000000</td></tr><tr><td>R32_TKEY1_CHARGE2</td><td>0x40012410</td><td>TKEY 充电采样时间寄存器 2</td><td>0x00000000</td></tr><tr><td>R32_TKEY1_CHGOFFSET</td><td>0x4001243C</td><td>TKEY 充电时间偏移量寄存器</td><td>0x00000000</td></tr><tr><td>R32_TKEY1_ACT_DCG</td><td>0x4001244C</td><td>TKEY 启动和放电时间寄存器</td><td>0x00000000</td></tr><tr><td>R32_TKEY1_DR</td><td>0x4001244C</td><td>TKEY 数据寄存器</td><td>0x00000000</td></tr></table>

表 13-2 TKEY2 相关寄存器列表  

<table><tr><td>名称</td><td>访问地址</td><td>描述</td><td>复位值</td></tr><tr><td>R32_TKEY2_CHARGE1</td><td>0x4001280C</td><td>TKEY 充电采样时间寄存器 1</td><td>0x00000000</td></tr><tr><td>R32_TKEY2_CHARGE2</td><td>0x40012810</td><td>TKEY 充电采样时间寄存器 2</td><td>0x00000000</td></tr><tr><td>R32_TKEY2_CHGOFFSET</td><td>0x4001283C</td><td>TKEY 充电时间偏移量寄存器</td><td>0x00000000</td></tr><tr><td>R32_TKEY2_ACT_DCG</td><td>0x4001284C</td><td>TKEY 启动和放电时间寄存器</td><td>0x00000000</td></tr><tr><td>R32_TKEY2_DR</td><td>0x4001284C</td><td>TKEY 数据寄存器</td><td>0x00000000</td></tr></table>

### 13.3.1 TKEYx 充电采样时间寄存器 1（TKEYx_CHARGE1）（x=1/2）

偏移地址： $0 \times 0 0$   

<table><tr><td colspan="4">Reserved</td><td colspan="2">TKCG17[2:0]</td><td colspan="2">TKCG16[2:0]</td><td>TKCG15[2:1]</td></tr><tr><td>15</td><td>14</td><td>13</td><td>12</td><td>11</td><td>10</td><td>9</td><td>8</td><td>7</td></tr><tr><td>TKCG15</td><td colspan="2">TKCG14[2:0]</td><td>TKCG13[2:0]</td><td colspan="2">TKCG12[2:0]</td><td colspan="2">TKCG11[2:0]</td><td>TKCG10[2:0]</td></tr></table>

<table><tr><td>位</td><td>名称</td><td>访问</td><td>描述</td><td>复位值</td></tr><tr><td>[31:24]</td><td>Reserved</td><td>R0</td><td>保留。</td><td>0</td></tr><tr><td>[23:0]</td><td>TKCGx[2:0]</td><td>RW</td><td>TKCGx[2:0] (x=10-17): 选择通道x的充电采样时间这些为用于独立地选择每个通道的充电时间。000: 1.5周期 100: 41.5周期001: 7.5周期 101: 55.5周期010: 13.5周期 110: 71.5周期011: 28.5周期 111: 239.5周期时间基准: ADC时钟。</td><td>000b</td></tr></table>

注：此寄存器映射ADC模块的采样时间寄存器1（ADC_SAMPTR1）。配置ADC功能时，为通道的采用时间；配置TKEY功能时，为通道充电时间。

### 13.3.2 TKEYx 充电采样时间寄存器 2（TKEYx_CHARGE2）（x=1/2）

偏移地址： $0 \times 1 0$

<table><tr><td>31</td><td>30</td><td>29</td><td>28</td><td>27</td><td>26</td><td>25</td><td>24</td><td>23</td><td>22</td><td>21</td><td>20</td><td>19</td><td>18</td><td>17</td><td>16</td></tr><tr><td colspan="2">Reserved</td><td colspan="3">TKCG9[2:0]</td><td colspan="3">TKCG8[2:0]</td><td colspan="3">TKCG7[2:0]</td><td colspan="3">TKCG6[2:0]</td><td colspan="2">TKCG5[2:1]</td></tr><tr><td>15</td><td>14</td><td>13</td><td>12</td><td>11</td><td>10</td><td>9</td><td>8</td><td>7</td><td>6</td><td>5</td><td>4</td><td>3</td><td>2</td><td>1</td><td>0</td></tr><tr><td>TKCG5</td><td colspan="3">TKCG4[2:0]</td><td colspan="3">TKCG3[2:0]</td><td colspan="3">TKCG2[2:0]</td><td colspan="3">TKCG1[2:0]</td><td colspan="3">TKCG0[2:0]</td></tr></table>

<table><tr><td>位</td><td>名称</td><td>访问</td><td>描述</td><td>复位值</td></tr><tr><td>[31:30]</td><td>Reserved</td><td>R0</td><td>保留。</td><td>0</td></tr><tr><td>[29:0]</td><td>TKCGx[2:0]</td><td>RW</td><td>TKCGx[2:0] (x=0-9): 选择通道x的充电采样时间这些为用于独立地选择每个通道的充电时间。000: 1.5周期 100: 41.5周期001: 7.5周期 101: 55.5周期010: 13.5周期 110: 71.5周期011: 28.5周期 111: 239.5周期时间基准: ADC时钟。</td><td>000b</td></tr></table>

注：此寄存器映射ADC模块的采样时间寄存器1（ADC SAMPTR2）。配置ADC功能时，为通道的采用时间；配置TKEY功能时，为通道充电时间。

### 13.3.3 TKEYx 充电时间偏移量寄存器（TKEYx_CHGOFFSET）（x=1/2）

偏移地址： $\mathtt { 0 } \mathtt { x } \mathtt { 3 0 }$

<table><tr><td>31</td><td>30</td><td>29</td><td>28</td><td>27</td><td>26</td><td>25</td><td>24</td><td>23</td><td>22</td><td>21</td><td>20</td><td>19</td><td>18</td><td>17</td><td>16</td></tr><tr><td colspan="16">Reserved</td></tr><tr><td>15</td><td>14</td><td>13</td><td>12</td><td>11</td><td>10</td><td>9</td><td>8</td><td>7</td><td>6</td><td>5</td><td>4</td><td>3</td><td>2</td><td>1</td><td>0</td></tr><tr><td colspan="8">Reserved</td><td colspan="8">TKCGOFFSET[7:0]</td></tr></table>

<table><tr><td>位</td><td>名称</td><td>访问</td><td>描述</td><td>复位值</td></tr><tr><td>[31:8]</td><td>Reserved</td><td>R0</td><td>保留。</td><td>0</td></tr><tr><td>[7:0]</td><td>TKCGOFFSET[7:0]</td><td>WO</td><td>TKEY充电时间偏移量配置值。总充电时间TCHG=TKCGOFFSET+TKCGx</td><td>0</td></tr></table>

注：此寄存器映射ADC模块的注入数据寄存器1（ADC_IDATAR1）。因此当该地址寄存器进行“写操作”时，作为TKEY充电时间偏移量（TKEY_CHGOFFSET）执行；进行“读操作”时，作为ADC模块的注入数据寄存器1（ADC_IDATAR1）执行。

### 13.3.4 TKEYx 启动和放电时间寄存器（TKEYx_ACT_DCG）（x=1/2）

偏移地址： $\mathtt { 0 } \mathtt { x } 4 0$

<table><tr><td>31</td><td>30</td><td>29</td><td>28</td><td>27</td><td>26</td><td>25</td><td>24</td><td>23</td><td>22</td><td>21</td><td>20</td><td>19</td><td>18</td><td>17</td><td>16</td></tr><tr><td colspan="16">Reserved</td></tr><tr><td>15</td><td>14</td><td>13</td><td>12</td><td>11</td><td>10</td><td>9</td><td>8</td><td>7</td><td>6</td><td>5</td><td>4</td><td>3</td><td>2</td><td>1</td><td>0</td></tr><tr><td colspan="8">Reserved</td><td colspan="8">TKACT_DCG[7:0]</td></tr></table>

<table><tr><td>位</td><td>名称</td><td>访问</td><td>描述</td><td>复位值</td></tr><tr><td>[31:8]</td><td>Reserved</td><td>R0</td><td>保留。</td><td>0</td></tr><tr><td>[7:0]</td><td>TKACT_DCG[7:0]</td><td>W0</td><td>写放电时间并启动一次TKEY通道检测。</td><td>0</td></tr></table>

注：此寄存器映射ADC模块的规则数据寄存器（ADC_RDATAR）。

### 13.3.5 TKEYx 数据寄存器（TKEYx_DR）（x=1/2）

偏移地址： $\mathtt { 0 } \mathtt { x } 4 0$

<table><tr><td>31</td><td>30</td><td>29</td><td>28</td><td>27</td><td>26</td><td>25</td><td>24</td><td>23</td><td>22</td><td>21</td><td>20</td><td>19</td><td>18</td><td>17</td><td>16</td></tr><tr><td colspan="16">Reserved</td></tr><tr><td>15</td><td>14</td><td>13</td><td>12</td><td>11</td><td>10</td><td>9</td><td>8</td><td>7</td><td>6</td><td>5</td><td>4</td><td>3</td><td>2</td><td>1</td><td>0</td></tr><tr><td colspan="16">DATA[15:0]</td></tr></table>

<table><tr><td>位</td><td>名称</td><td>访问</td><td>描述</td><td>复位值</td></tr><tr><td>[31:16]</td><td>Reserved</td><td>R0</td><td>保留。</td><td>0</td></tr><tr><td>[15:0]</td><td>DATA[15:0]</td><td>R0</td><td>转换的数据。</td><td>0</td></tr></table>

注：此寄存器映射ADC模块的规则数据寄存器（ADC_RDATAR）。


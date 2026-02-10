# 第 5 章 循环冗余校验（CRC）

本章模块描述适用于CH32F2x、CH32V2x和CH32V3x微控制器全系列产品。

循环冗余校验(CRC)计算单元是根据固定的生成多项式得到任一 32位数据的CRC计算结果。一般用于数据存储和数据通讯领域用来核实数据的正确性。系统提供硬件 CRC 计算单元可以大大节省 CPU和 RAM 资源提高效率。

![](images/7601f93c539102d343b7a1201327ffc31c7ded5e6617d32eac1e2ea3f668b41f.jpg)  
图 5-1 CRC 结构框图

## 5.1 主要特征

l 使用 CRC32 多项式（0x4C11DB7）： $X ^ { 3 2 } + X ^ { 2 6 } + X ^ { 2 3 } + X ^ { 2 2 } + X ^ { 1 6 } + X ^ { 1 2 } + X ^ { 1 1 } + X ^ { 1 0 } + X ^ { 8 } + X ^ { 7 } + X ^ { 5 } + X ^ { 4 } + X ^ { 2 } + X + 1 $   
$\bullet$ 同一个 32 位寄存器作为数据的输入和 CRC32 计算输出  
单次转换时间：4 个 AHB 时钟周期（HCLK）

## 5.2 功能描述

### CRC 单元复位

如果要开始一次新数据组的 CRC 计算，需要复位 CRC 计算单元。向控制寄存器 CRC_CTLR 的 RST位写 1，硬件将复位数据寄存器，恢复初始值 0xFFFFFFFF。

### CRC 计算

CRC 单元的计算是前一次 CRC 计算结果和新参与的数据的 CRC 结果。CRC_DATAR 数据寄存器，对其执行写操作将送入新数据到硬件计算单元；执行读取操作，将得到最新一轮的 CRC计算值。硬件计算时会中断系统的写操作，因此可以连续写入新的值。

注：CRC单元是对整个32位数据进行计算，而不是逐字节计算。

### 独立数据缓冲区

CRC 单元提供了一个 8 位独立数据寄存器 CRC_IDATAR，用于应用代码临时存放 1 字节的数据，不受 CRC 单元复位影响。

## 5.3 寄存器描述

表5-1 CRC相关寄存器列表  

<table><tr><td>名称</td><td>访问地址</td><td>描述</td><td>复位值</td></tr><tr><td>R32_CRC_DATAR</td><td>0x40023000</td><td>数据寄存器</td><td>0xFFFFFFF</td></tr><tr><td>R8_CRC_IDATAR</td><td>0x40023004</td><td>独立数据缓冲</td><td>0x00</td></tr><tr><td>R32_CRC_CTLR</td><td>0x40023008</td><td>控制寄存器</td><td>0x00000000</td></tr></table>

### 5.3.1 数据寄存器（CRC_DATAR）

偏移地址： $0 \times 0 0$

<table><tr><td>31</td><td>30</td><td>29</td><td>28</td><td>27</td><td>26</td><td>25</td><td>24</td><td>23</td><td>22</td><td>21</td><td>20</td><td>19</td><td>18</td><td>17</td><td>16</td></tr><tr><td colspan="16">DR[31:16]</td></tr><tr><td>15</td><td>14</td><td>13</td><td>12</td><td>11</td><td>10</td><td>9</td><td>8</td><td>7</td><td>6</td><td>5</td><td>4</td><td>3</td><td>2</td><td>1</td><td>0</td></tr><tr><td colspan="16">DR[15:0]</td></tr></table>

<table><tr><td>位</td><td>名称</td><td>访问</td><td>描述</td><td>复位值</td></tr><tr><td>[31:0]</td><td>DR[31:0]</td><td>RW</td><td>写入原始数据；读出计算结果。</td><td>0xFFFFFF</td></tr></table>

### 5.3.2 独立数据缓冲（CRC_IDATAR）

偏移地址： $0 \times 0 4$

<table><tr><td>Reserved</td><td>IDR[7:0]</td></tr></table>

<table><tr><td>位</td><td>名称</td><td>访问</td><td>描述</td><td>复位值</td></tr><tr><td>[7:0]</td><td>IDR[7:0]</td><td>RW</td><td>8位通用寄存器，可以用作数据缓存，这个寄存器不受控制寄存器的RST域影响。</td><td>0</td></tr></table>

### 5.3.3 控制寄存器（CRC_CTLR）

偏移地址： $0 \times 0 8$

<table><tr><td>31</td><td>30</td><td>29</td><td>28</td><td>27</td><td>26</td><td>25</td><td>24</td><td>23</td><td>22</td><td>21</td><td>20</td><td>19</td><td>18</td><td>17</td><td>16</td></tr><tr><td colspan="16">Reserved</td></tr><tr><td>15</td><td>14</td><td>13</td><td>12</td><td>11</td><td>10</td><td>9</td><td>8</td><td>7</td><td>6</td><td>5</td><td>4</td><td>3</td><td>2</td><td>1</td><td>0</td></tr><tr><td colspan="15">Reserved</td><td>RST</td></tr></table>

<table><tr><td>位</td><td>名称</td><td>访问</td><td>描述</td><td>复位值</td></tr><tr><td>[31:1]</td><td>Reserved</td><td>R0</td><td>保留。</td><td>0</td></tr><tr><td>0</td><td>RST</td><td>WO</td><td>CRC计算单元复位控制，写1执行，硬件自动清零，执行完后，数据寄存器为0xFFFFFFF。</td><td>0</td></tr></table>


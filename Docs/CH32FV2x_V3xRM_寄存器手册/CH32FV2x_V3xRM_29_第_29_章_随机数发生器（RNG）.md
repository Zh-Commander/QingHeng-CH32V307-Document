# 第 29 章 随机数发生器（RNG）

本章模块描述适用于 $G H 3 2 F 2 x$ 和CH32V3x微控制器系列部分产品。

随机数发生器（RNG），以连续模拟噪声为基础，在主机读取数据时提供 32bit的随机数。

## 29.1 主要特性

可产生 32bit 随机数  
可实现错误管理  
可被单独禁止，降低功耗

![](images/ef7e67d37ce4e899d8093cd378f1b2f05a53ffff6b4d64c07e7bb822c4d4d016.jpg)  
图 29-1 RNG 模块框图

## 29.2 功能描述

随机发送器采用模拟电路实现，该电路产生线性反馈移位寄存器（RNG_LFSR）的种子，用于生成32位随机数。RNG_LFSR由专用时钟（PLL48CLK）按照恒定频率提供时钟信息，故随机数质量与HCLK时钟有关。当有大量种子引入RNG_LFSR后，RNG_LFSR的内容会传入数据寄存器（RNG_DR）。

### 29.2.1 RNG 操作

RNG 具体操作步骤如下：

1） 若使能中断，需通过将RNG_CR寄存器中的 IE位置1（当准备好随机数或出现错误时产生该中断）。  
2） 通过配置 RNG_CR 寄存器的 RNGEN 位使能随机数产生，同时激活模拟部分、RNG_LFSR 和错误检测器。  
3） 若使能中断，每次产生中断时，通过查询 RNG_SR寄存器中的 SEIS和 CEIS 位为 0 确定未出现错误且DRDY为为1确定随机数已准备就绪。之后即可读取 RNG_DR 寄存器中的内容。

### 29.2.2 错误管理

RNG 出错包括时钟错误和种子错误。当出现时钟错误时，RNG无法再产生随机数，此时需检查时钟控制器是否正确配置，是否可提供RNG时钟，然后将 CEIS位清零。当 CECS位为0 时，RNG可正常工作。当产生时钟错误时，对上一个随机数没有影响，可正常使用。当出现种子错误时，此时RNG_DR 寄存器中的值不可使用该随机数，若用重新使用 RNG，需要先将 SEIS 位清零，然后将 RNGEN位清零并置1，重新初始化和重新启动RNG。

## 29.3 寄存器描述

表 29-1 OPA 相关寄存器列表  

<table><tr><td>名称</td><td>访问地址</td><td>描述</td><td>复位值</td></tr><tr><td>R32_RNG_CR</td><td>0x40023C00</td><td>RNG 控制寄存器</td><td>0x00000000</td></tr><tr><td>R32_RNG_SR</td><td>0x40023C04</td><td>RNG 状态寄存器</td><td>0x00000000</td></tr><tr><td>R32_RNG_DR</td><td>0x40023C08</td><td>RNG 数据寄存器</td><td>0x00000000</td></tr></table>

### 29.3.1 RNG 控制寄存器（RNG_CR）

偏移地址： $0 \times 0 0$   

<table><tr><td>31</td><td>30</td><td>29</td><td>28</td><td>27</td><td>26</td><td>25</td><td>24</td><td>23</td><td>22</td><td>21</td><td>20</td><td>19</td><td>18</td><td>17</td><td>16</td></tr><tr><td colspan="16">Reserved</td></tr><tr><td>15</td><td>14</td><td>13</td><td>12</td><td>11</td><td>10</td><td>9</td><td>8</td><td>7</td><td>6</td><td>5</td><td>4</td><td>3</td><td>2</td><td>1</td><td>0</td></tr><tr><td colspan="12">Reserved</td><td>IE</td><td>RNGEN</td><td colspan="2">Reserved</td></tr></table>

<table><tr><td>位</td><td>名称</td><td>访问</td><td>描述</td><td>复位值</td></tr><tr><td>[31:4]</td><td>Reserved</td><td>RO</td><td>保留。</td><td>0</td></tr><tr><td>3</td><td>IE</td><td>RW</td><td>中断使能控制0:禁止RNG中断1:使能RNG中断</td><td>0</td></tr><tr><td>2</td><td>RNGEN</td><td>RW</td><td>随机数发生器使能0:禁止随机数发生器1:使能随机数发生器</td><td>0</td></tr><tr><td>[1:0]</td><td>Reserved</td><td>RW</td><td>保留。</td><td>0</td></tr></table>

### 29.3.2 RNG 状态寄存器（RNG_SR）

偏移地址： $0 \times 0 4$   

<table><tr><td>31</td><td>30</td><td>29</td><td>28</td><td>27</td><td>26</td><td>25</td><td>24</td><td>23</td><td>22</td><td>21</td><td>20</td><td>19</td><td>18</td><td>17</td><td>16</td></tr><tr><td colspan="16">Reserved</td></tr><tr><td>15</td><td>14</td><td>13</td><td>12</td><td>11</td><td>10</td><td>9</td><td>8</td><td>7</td><td>6</td><td>5</td><td>4</td><td>3</td><td>2</td><td>1</td><td>0</td></tr><tr><td colspan="9">Reserved</td><td>SEIS</td><td>CEIS</td><td>Reserved</td><td>SECS</td><td>CECS</td><td colspan="2">DRDY</td></tr></table>

<table><tr><td>位</td><td>名称</td><td>访问</td><td>描述</td><td>复位值</td></tr><tr><td>[31:7]</td><td>Reserved</td><td>R0</td><td>保留。</td><td>0</td></tr><tr><td>6</td><td>SEIS</td><td>RW</td><td>种子错误中断状态(此位与SECS同时设置)0:未检测倒错误序列1:检测到以下错误序列之一:-超过64个相同连续位;-超过32个连续交替的0和1;</td><td>0</td></tr><tr><td>5</td><td>CEIS</td><td>RW</td><td>时钟错误中断状态(此位与CECS同时设置)0:正确检测到PLL48CLK时钟1:未正确检测到PLL48CLK时钟</td><td>0</td></tr><tr><td>[4:3]</td><td>Reserved</td><td>R0</td><td>保留。</td><td>0</td></tr><tr><td>2</td><td>SECS</td><td>R0</td><td>种子错误当前状态0:未检测出错误序列1:检测到以下错误序列之一:-超过64个相同连续位;-超过32个连续交替的0和1;</td><td>0</td></tr><tr><td>1</td><td>CECS</td><td>R0</td><td>时钟错误当前状态0:正确检测到PLL48CLK时钟1:未正确检测到PLL48CLK时钟</td><td>0</td></tr><tr><td>0</td><td>DRDY</td><td>R0</td><td>数据就绪(读取RNG_DR寄存器后,该位清0)0:RNG_DR寄存器无效,此随机数不可用1:RNG_DR寄存器有效,此随机数可用</td><td>0</td></tr></table>

### 29.3.3 RNG 数据寄存器（RNG_DR）

偏移地址： $0 \times 0 8$

<table><tr><td>31</td><td>30</td><td>29</td><td>28</td><td>27</td><td>26</td><td>25</td><td>24</td><td>23</td><td>22</td><td>21</td><td>20</td><td>19</td><td>18</td><td>17</td><td>16</td></tr><tr><td colspan="16">RNDATA[31:16]</td></tr><tr><td>15</td><td>14</td><td>13</td><td>12</td><td>11</td><td>10</td><td>9</td><td>8</td><td>7</td><td>6</td><td>5</td><td>4</td><td>3</td><td>2</td><td>1</td><td>0</td></tr><tr><td colspan="16">RNDATA[15:0]</td></tr></table>

<table><tr><td>位</td><td>名称</td><td>访问</td><td>描述</td><td>复位值</td></tr><tr><td>[31:0]</td><td>RNDATA</td><td>R0</td><td>32位随机数</td><td>0</td></tr></table>


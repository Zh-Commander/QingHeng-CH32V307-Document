# 第 30 章 运算放大器（OPA）

本章模块描述适用于CH32F2x、CH32V2x和CH32V3x微控制器系列部分产品。

运算放大器模块（OPA），包含4个可独立配置的运算放大器。每个运算放大器的输入和输出均连接至 I/O 口，且输入引脚可选择，输出引脚可选择配置到通用 I/O 口或复用为 ADC 采样通道的I/O。

## 30.1 主要特性

输入引脚可选择   
输出引脚可选择通用 I/O 口或 ADC 采样通道

## 30.2 功能描述

置位 OPAx_EN，即可使能对应的 OPAx，配置 OPAx_MODE 可选择 OPAx 的输出通道为 ADC 采样通道或者普通 I/O 口，配置 OPAx_PSEL，可选择 OPAx 的正向输入引脚，配置 OPAx_NSEL，可选择 OPAx 的负向输入引脚。

注：各OPA详细的输入输出引脚，参考数据手册中引脚说明。

## 30.3 寄存器描述

表 30-1 OPA 相关寄存器列表  

<table><tr><td>名称</td><td>访问地址</td><td>描述</td><td>复位值</td></tr><tr><td>R32_OPA_CTLR</td><td>0x40023804</td><td>OPA配置寄存器</td><td>0x00000000</td></tr></table>

### 30.3.1 OPA 配置寄存器（OPA_CTLR）

偏移地址： $0 \times 0 0$

<table><tr><td>31</td><td>30</td><td>29</td><td>28</td><td>27</td><td>26</td><td>25</td><td>24</td><td>23</td><td>22</td><td>21</td><td>20</td><td>19</td><td>18</td><td>17</td><td>16</td></tr><tr><td colspan="16">Reserved</td></tr><tr><td>15</td><td>14</td><td>13</td><td>12</td><td>11</td><td>10</td><td>9</td><td>8</td><td>7</td><td>6</td><td>5</td><td>4</td><td>3</td><td>2</td><td>1</td><td>0</td></tr><tr><td>PSEL4</td><td>NSEL4</td><td>MODE4</td><td>EN4</td><td>PSEL3</td><td>NSEL3</td><td>MODE3</td><td>EN3</td><td>PSEL2</td><td>NSEL2</td><td>MODE2</td><td>EN2</td><td>PSEL1</td><td>NSEL1</td><td>MODE1</td><td>EN1</td></tr></table>

<table><tr><td>位</td><td>名称</td><td>访问</td><td>描述</td><td>复位值</td></tr><tr><td>[31:16]</td><td>Reserved</td><td>R0</td><td>保留。</td><td>0</td></tr><tr><td>15</td><td>PSEL4</td><td>RW</td><td>OPA4正向输入端选择0:CHP01:CHP1注:适用于CH32F20x_D8、CH32F20x_D8C、CH32V30x_D8、CH32V30x_D8C。</td><td>0</td></tr><tr><td>14</td><td>NSEL4</td><td>RW</td><td>OPA4负向输入端选择0:CHN01:CHN1注:适用于CH32F20x_D8、CH32F20x_D8C、CH32V30x_D8、CH32V30x_D8C。</td><td>0</td></tr><tr><td>13</td><td>MODE4</td><td>RW</td><td>OPA4输出通道选择0:输出通道为OPA4_OUT01:输出通道为OPA4_OUT1注:适用于CH32F20x_D8、CH32F20x_D8C、CH32V30x_D8、CH32V30x_D8C。</td><td>0</td></tr><tr><td>12</td><td>EN4</td><td>RW</td><td>OPA4使能0:禁止OPA41:使能OPA4注:适用于CH32F20x_D8、CH32F20x_D8C、CH32V30x_D8、CH32V30x_D8C。</td><td>0</td></tr><tr><td>11</td><td>PSEL3</td><td>RW</td><td>OPA3正向输入端选择0:CHP01:CHP1注:适用于CH32F20x_D8、CH32F20x_D8C、CH32V30x_D8、CH32V30x_D8C。</td><td>0</td></tr><tr><td>10</td><td>NSEL3</td><td>RW</td><td>OPA3负向输入端选择0:CHN01:CHN1注:适用于CH32F20x_D8、CH32F20x_D8C、CH32V30x_D8、CH32V30x_D8C。</td><td>0</td></tr><tr><td>9</td><td>MODE3</td><td>RW</td><td>OPA3输出通道选择0:输出通道为OPA3_OUT01:输出通道为OPA3_OUT1注:适用于CH32F20x_D8、CH32F20x_D8C、CH32V30x_D8、CH32V30x_D8C。</td><td>0</td></tr><tr><td>8</td><td>EN3</td><td>RW</td><td>OPA3使能0:禁止OPA31:使能OPA3注:适用于CH32F20x_D8、CH32F20x_D8C、CH32V30x_D8、CH32V30x_D8C。</td><td>0</td></tr><tr><td>7</td><td>PSEL2</td><td>RW</td><td>OPA2正向输入端选择0:CHP01:CHP1</td><td>0</td></tr><tr><td>6</td><td>NSEL2</td><td>RW</td><td>OPA2负向输入端选择0:CHN01:CHN1</td><td>0</td></tr><tr><td>5</td><td>MODE2</td><td>RW</td><td>OPA2输出通道选择0:输出通道为OPA2_OUT01:输出通道为OPA2_OUT1</td><td>0</td></tr><tr><td>4</td><td>EN2</td><td>RW</td><td>OPA2使能0:禁止OPA21:使能OPA2</td><td>0</td></tr><tr><td>3</td><td>PSEL1</td><td>RW</td><td>OPA1正向输入端选择0:CHP01:CHP1</td><td>0</td></tr><tr><td>2</td><td>NSEL1</td><td>RW</td><td>OPA1负向输入端选择</td><td>0</td></tr><tr><td></td><td></td><td></td><td>0:CHNO
1:CHN1</td><td></td></tr><tr><td>1</td><td>MODE1</td><td>RW</td><td>OPA1输出通道选择
0:输出通道为OPA1_OUT0
1:输出通道为OPA1_OUT1</td><td>0</td></tr><tr><td>0</td><td>EN1</td><td>RW</td><td>OPA1使能
0:禁止OPA1
1:使能OPA1</td><td>0</td></tr></table>


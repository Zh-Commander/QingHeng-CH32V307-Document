## CH591

### 应用框图 \ Block Diagram

![](images/09fa2016eaa69f80bf6e916ed9bea0f7b73cbdfa506cd459ad0a2abc4677fbed.jpg)

#### 产品特点 \ Features

> 青稞32位RISC-V3C内核  
> 支持RV32IMBC指令集和自扩展指令，特有高速中断响应机制

> 128KB SRAM,512KB Flash   
> 支持BLE5.4,内置2.4GHz RF收发器  
> 2.4G模式下最高8kHz上报率  
> 接收灵敏度-95dBm@1Mbps, 可编程+4.5dBm发送功率  
> 提供协议栈和应用层API  
> 480Mbps高速/全速USB2.0控制器及PHY  
> 近场通信无线接口NFC

段式LCD接口，支持112个点 $(28^{*}4)$ LCD面板  
> LED点阵屏接口,支持1/2/4/8路数据线  
> 14通道触摸按键   
> 14通道12位ADC  
> 4组UART, 2组SPI, 12路PWM, 1路I²C   
> 40个GPIO

> 最低支持1.7V电源电压  
> 内置温度传感器  
> 内置AES-128加解密单元,芯片唯一ID  
封装：QFN48T、QFN32、QFN26C3

#### 选型指南 \ Model Selection Guide

<table><tr><td>Part NO.</td><td>Core</td><td>Freq</td><td>Flash</td><td>SRAM</td><td>Data Flash</td><td>BLE</td><td>NFC</td><td>USB FS</td><td>USB HS</td><td>ADC(12bit) Unit/ Channel</td><td>Touch Key</td><td>LEDC</td><td>LCD</td><td>Timer (26bit)</td><td>PWM</td><td>UART</td><td>SPI</td><td>I²C</td><td>DC-DC</td><td>RTC</td><td>WDOG</td><td>GPIO</td><td>VDD</td><td>Package</td></tr><tr><td>CH585M</td><td>RISC-V</td><td>78MHz</td><td>448K</td><td>128K</td><td>32K</td><td>5.4</td><td>✓</td><td>1</td><td>1</td><td>1/14</td><td>14</td><td>1/2/4/8</td><td>28*4</td><td>4</td><td>12</td><td>4</td><td>2</td><td>1</td><td>✓</td><td>✓</td><td>✓</td><td>40</td><td>1.7/3.3</td><td>QFN48</td></tr><tr><td>CH585F</td><td>RISC-V</td><td>78MHz</td><td>448K</td><td>128K</td><td>32K</td><td>5.4</td><td>✓</td><td>1</td><td>1</td><td>1/7</td><td>7</td><td>-</td><td>14*4</td><td>4</td><td>11</td><td>4</td><td>1</td><td>1</td><td>✓</td><td>✓</td><td>✓</td><td>24</td><td>1.7/3.3</td><td>QFN32</td></tr><tr><td>CH585C</td><td>RISC-V</td><td>78MHz</td><td>448K</td><td>128K</td><td>32K</td><td>5.4</td><td>✓</td><td>1</td><td>1</td><td>1/4</td><td>-</td><td>-</td><td>-</td><td>4</td><td>9</td><td>2</td><td>1</td><td>-</td><td>✓</td><td>✓</td><td>✓</td><td>17</td><td>1.7/3.3</td><td>QFN26C3</td></tr><tr><td>CH585D</td><td>RISC-V</td><td>78MHz</td><td>448K</td><td>128K</td><td>32K</td><td>5.4</td><td>✓</td><td>1</td><td>1</td><td>1/4</td><td>-</td><td>-</td><td>-</td><td>4</td><td>7</td><td>2</td><td>1</td><td>-</td><td>✓</td><td>✓</td><td>✓</td><td>12</td><td>1.7/3.3</td><td>QFN20</td></tr><tr><td>CH584M</td><td>RISC-V</td><td>78MHz</td><td>448K</td><td>96K</td><td>32K</td><td>5.4</td><td>✓</td><td>1</td><td>-</td><td>1/14</td><td>14</td><td>1/2/4/8</td><td>28*4</td><td>4</td><td>12</td><td>4</td><td>1</td><td>1</td><td>✓</td><td>✓</td><td>✓</td><td>40</td><td>1.7/3.3</td><td>QFN48</td></tr><tr><td>CH584F</td><td>RISC-V</td><td>78MHz</td><td>448K</td><td>96K</td><td>32K</td><td>5.4</td><td>✓</td><td>1</td><td>-</td><td>1/7</td><td>7</td><td>-</td><td>14*4</td><td>4</td><td>11</td><td>4</td><td>1</td><td>1</td><td>✓</td><td>✓</td><td>✓</td><td>24</td><td>1.7/3.3</td><td>QFN32</td></tr></table>

### 应用框图 \ Block Diagram

![](images/eac788c0d1d2bb97ff4233581833105876f398e473adf485317b5ce9774cd010.jpg)

#### 产品特点 \ Features

>青稞32位RISC-V4C内核  
> 支持RV32IMAC指令集和自扩展指令  
> 支持单周期乘法和硬件除法  
> 26KB SRAM, 512KB FLASH   
> 支持BLE5.4,内置2.4GHz RF收发器  
> 接受灵敏度-95dBm,可编程+4.5dBm发送功率  
> 提供优化的协议栈和应用层API，支持组网  
> 主从一体, 支持多主多从  
> 内置温度传感器  
> 段式LCD, 支持80点 $(20^{*}4)$ LCD面板

选型指南 \Model Selection Guide  

<table><tr><td>Part NO.</td><td>Core</td><td>Freq</td><td>Flash</td><td>SRAM</td><td>Data Flash</td><td>BLE</td><td>USB2.0 FS</td><td>ADC/TS</td><td>TouchKey</td><td>Timer</td><td>PWM</td><td>UART</td><td>SPI</td><td>I²C</td><td>DC-DC</td><td>RTC</td><td>WDOG</td><td>GPIO</td><td>VDD</td><td>Package</td></tr><tr><td>CH592X</td><td>RISC-V</td><td>20MHz</td><td>448K</td><td>26K</td><td>32K</td><td>5.4</td><td>1*H/D</td><td>12/1</td><td>12</td><td>4</td><td>4+8</td><td>4</td><td>1</td><td>1</td><td>✓</td><td>✓</td><td>✓</td><td>24</td><td>1.7/3.3</td><td>QFN32</td></tr><tr><td>CH592F</td><td>RISC-V</td><td>20MHz</td><td>448K</td><td>26K</td><td>32K</td><td>5.4</td><td>1*H/D</td><td>8/1</td><td>8</td><td>4</td><td>4+6</td><td>4</td><td>1</td><td>1</td><td>✓</td><td>✓</td><td>✓</td><td>20</td><td>1.7/3.3</td><td>QFN28</td></tr><tr><td>CH592A</td><td>RISC-V</td><td>20MHz</td><td>448K</td><td>26K</td><td>32K</td><td>5.4</td><td>1*H/D</td><td>8/1</td><td>8</td><td>4</td><td>4+6</td><td>4</td><td>1</td><td>1</td><td>✓</td><td>✓</td><td>✓</td><td>20</td><td>2.3/3.3</td><td>QFN28</td></tr><tr><td>CH592D</td><td>RISC-V</td><td>20MHz</td><td>448K</td><td>26K</td><td>32K</td><td>5.4</td><td>1*H/D</td><td>4/1</td><td>4</td><td>2</td><td>2+3</td><td>2</td><td>1</td><td>1</td><td>✓</td><td>✓</td><td>✓</td><td>12</td><td>1.7/3.3</td><td>QFN20</td></tr><tr><td>CH591F</td><td>RISC-V</td><td>20MHz</td><td>192K</td><td>26K</td><td>32K</td><td>5.4</td><td>1*D</td><td>6/1</td><td>-</td><td>4</td><td>4+6</td><td>2</td><td>1</td><td>-</td><td>✓</td><td>✓</td><td>✓</td><td>20</td><td>2.3/3.3</td><td>QFN28</td></tr><tr><td>CH591D</td><td>RISC-V</td><td>20MHz</td><td>192K</td><td>26K</td><td>32K</td><td>5.4</td><td>1*D</td><td>4/1</td><td>-</td><td>3</td><td>3+4</td><td>2</td><td>1</td><td>-</td><td>✓</td><td>✓</td><td>✓</td><td>12</td><td>2.3/3.3</td><td>QFN20</td></tr><tr><td>CH591R</td><td>RISC-V</td><td>20MHz</td><td>192K</td><td>26K</td><td>32K</td><td>5.4</td><td>1*D</td><td>4/1</td><td>-</td><td>4</td><td>4+3</td><td>2</td><td>1</td><td>-</td><td>-</td><td>✓</td><td>✓</td><td>10</td><td>2.3/3.3</td><td>TSSOP16</td></tr></table>

### 其他低功耗蓝牙芯片\Others

CH579/8：BLE4.2，内置以太网控制器及收发器、全速USB主机和设备控制器及收发器、段式LCD、ADC、触摸按键

等外设资源。

### 青稞RISC-V内核

### USB和2.4G射频传输无线MCU/SoC

CH570是基于青棵V3C内核的2.4G无线通讯MCU/SoC。相比8位MCU资源更多，提供240K闪存、12K RAM、全速USB、比较器、按键检测及常规外设，支持高性能2.4G无线通讯协议，支持单线调试，适于2.4G无线通讯和Dongle应用。

CH572集成2Mbps低功耗蓝牙BLE通讯模块，支持BLE5.0。

### 应用框图 \Blc

#### Block Diagram

![](images/d226acd541e4aead8ab459ea37ce7acac4c7e107f15389cdbb64b09ea2945a32.jpg)

#### 产品特点 \ Features

> 青稞RISC-V3C内核  
> 支持RV32IMBC指令集和自扩展指令  
> 12KB SRAM, 256KB Flash   
> 5V转3.3V调压器LDO5V   
> 2.4GHz RF收发器，支持BLE5.0  
> 2.4G模式下最高8kHz上报率  
> 接收灵敏度-95dBm,可编程+7.5dBm发送功率   
> 提供协议栈和应用层API

> 支持20路按键检测，10路矩阵区按键、10路独立区按键  
> 全速USB2.0控制器及PHY  
> 模拟电压比较器CMP, 16档参考电压, 等效4位ADC  
> 1组UART, 1组SPI, 6路PWM, 1路I²C   
> 12个GPIO   
> 内置AES-128加解密单元,芯片唯一ID  
封装：QFN20、DFN10X3、TSSOP16、SOP8

#### 选型指南 \ Model Selection Guide

<table><tr><td>PartNO.</td><td>Core</td><td>Freq</td><td>Flash</td><td>SRAM</td><td>BLE</td><td>2.4G</td><td>USB</td><td>Key detection</td><td>Timer (26bit)</td><td>PWM</td><td>UART</td><td>SPI</td><td>I²C</td><td>CMP</td><td>GPIO</td><td>VDD</td><td>Package</td></tr><tr><td>CH572D</td><td>RISC-V</td><td>100MHz</td><td>240K</td><td>12K</td><td>5.0</td><td>✓</td><td>1*H/D</td><td>20</td><td>1</td><td>6</td><td>1</td><td>1</td><td>1</td><td>1</td><td>12</td><td>2.0/3.3 or 4.5/5.0</td><td>QFN20</td></tr><tr><td>CH572Q</td><td>RISC-V</td><td>100MHz</td><td>240K</td><td>12K</td><td>5.0</td><td>✓</td><td>1*H/D</td><td>5</td><td>1</td><td>4</td><td>1</td><td>-</td><td>1</td><td>1</td><td>5</td><td>2.0/3.3 or 4.5/5.0</td><td>DFN10X3</td></tr><tr><td>CH572R</td><td>RISC-V</td><td>100MHz</td><td>240K</td><td>12K</td><td>5.0</td><td>✓</td><td>1*H/D</td><td>9</td><td>1</td><td>6</td><td>1</td><td>1</td><td>1</td><td>1</td><td>10</td><td>2.0/3.3</td><td>TSSOP16</td></tr><tr><td>CH570D</td><td>RISC-V</td><td>100MHz</td><td>240K</td><td>12K</td><td>-</td><td>✓</td><td>1*H/D</td><td>20</td><td>1</td><td>6</td><td>1</td><td>1</td><td>1</td><td>1</td><td>12</td><td>2.0/3.3 or 4.5/5.0</td><td>QFN20</td></tr><tr><td>CH570Q</td><td>RISC-V</td><td>100MHz</td><td>240K</td><td>12K</td><td>-</td><td>✓</td><td>1*D</td><td>5</td><td>1</td><td>1</td><td>1</td><td>-</td><td>1</td><td>-</td><td>5</td><td>2.0/3.3 or 4.5/5.0</td><td>DFN10X3</td></tr><tr><td>CH570G</td><td>RISC-V</td><td>100MHz</td><td>240K</td><td>12K</td><td>-</td><td>✓</td><td>1*H/D</td><td>9</td><td>1</td><td>6</td><td>1</td><td>1</td><td>1</td><td>1</td><td>10</td><td>2.0/3.3 or 4.5/5.0</td><td>SOP16</td></tr><tr><td>CH570E</td><td>RISC-V</td><td>100MHz</td><td>240K</td><td>12K</td><td>-</td><td>✓</td><td>-</td><td>-</td><td>1</td><td>1</td><td>1</td><td>-</td><td>1</td><td>-</td><td>3</td><td>2.0/3.3</td><td>SOP8</td></tr></table>

### 青稞RISC-V内核

### 低功耗蓝牙BLE 5.3无线MCU/SoC

CH583是基于青稞V4A内核的无线MCU/SoC，支持BLE5.3，集成2个全速USB主机和设备控制器及收发器、ADC、触摸按键、4个串口等外设资源。

### 应用框图 \ Block Diagram

![](images/709c8929726b29ed65be26a65cbeb7ec2b25487405385d4214b18e35d39392b6.jpg)

#### 产品特点 \ Features

> 青稞RISC-V4A内核  
> 支持RV32IMAC指令集,支持硬件乘法和除法  
> 32KB SRAM, 512KB Flash   
> 支持BLE 5.3,内置2.4GHz RF收发器  
> 接受灵敏度-98dBm,可编程+6dBm发送功率  
> 提供协议栈和应用层API  
> 提供Mesh协议栈接口  
> 主从一体, 支持多主多从  
> 内置温度传感器

> 2组USB2.0全速Host/Device  
> 14通道触摸按键   
> 14通道12位ADC  
> 4组UART, 2组SPI, 12路PWM, 1路I²C   
> 40个GPIO   
> 最低支持1.7V电源电压  
> 内置AES-128加解密单元,芯片唯一ID  
封装：QFN48、QFN28

#### 选型指南 \ Model Selection Guide

<table><tr><td>Part NO.</td><td>Core</td><td>Freq</td><td>Flash</td><td>SRAM</td><td>Data Flash</td><td>BLE</td><td>USB2.0 FS</td><td>ADC(12bit) Unit/Channel</td><td>TouchKey</td><td>Timer (26bit)</td><td>PWM</td><td>UART</td><td>SPI</td><td>I²C</td><td>RTC</td><td>WDOG</td><td>GPIO</td><td>VDD</td><td>Package</td></tr><tr><td>CH583M</td><td>RISC-V</td><td>20MHz</td><td>448K</td><td>32K</td><td>32K</td><td>5.3</td><td>2*H/D</td><td>1/14</td><td>14</td><td>4</td><td>12</td><td>4</td><td>2</td><td>1</td><td>✓</td><td>✓</td><td>40</td><td>1.7/3.3</td><td>QFN48</td></tr><tr><td>CH582M</td><td>RISC-V</td><td>20MHz</td><td>448K</td><td>32K</td><td>32K</td><td>5.3</td><td>2*H/D</td><td>1/14</td><td>14</td><td>4</td><td>12</td><td>4</td><td>1</td><td>1</td><td>✓</td><td>✓</td><td>40</td><td>2.3/3.3</td><td>QFN48</td></tr><tr><td>CH582F</td><td>RISC-V</td><td>20MHz</td><td>448K</td><td>32K</td><td>32K</td><td>5.3</td><td>2*H/D</td><td>1/8</td><td>8</td><td>4</td><td>10</td><td>4</td><td>1</td><td>1</td><td>✓</td><td>✓</td><td>20</td><td>2.3/3.3</td><td>QFN28</td></tr></table>

#### 其他低功耗蓝牙芯片

Others

CH573/1:32位RISC-V内核低功耗蓝牙BLE4.2无线MCU/SoC。


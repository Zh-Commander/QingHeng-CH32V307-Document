## 青稞RISC-V MCU产品一览

### 自研连接技术交叉组合的跨接口芯片

沁恒自研的连接技术打通了USB/蓝牙/以太网的垂直层次，基于专业接口的交叉组合，提供跨接口的解决方案。

CH32V317/CH32V307

高速USB+百兆以太网内置PHY

CH397

百兆USB网卡，内置PHY

CH645

USB多主机/设备+双USBPD+以太网

CH569

5Gbps超高速USB+千兆以太网+SerDes

CH32V208 全能小网关

以太网

+

USE

+

蓝牙

CH585

BLE5.4+高性能自定义2.4G+高速USB+NFC

CH592

BLE5.4+USB+段式LCD

CH582

BLE5.3+双USB

CH570/2

BLE5.0+泛用型2.4G+USB

### 沁恒微电子USB转串口产品选型

USB转单串口芯片  

<table><tr><td>型号</td><td>USB</td><td>驱动类型①</td><td>峰值最高
波特率</td><td>流控连续
波特率</td><td>硬件
流控</td><td>自动控制
RS485</td><td>USB配置</td><td>IO电压
(MCU电压)</td><td>省电双供电
IO防倒灌</td><td>MODEM信号
(兼GPIO和其他接口)②</td><td>时钟</td><td>温度范围</td><td>封装</td><td>内核</td></tr><tr><td>CH346C</td><td>480Mbps
高速</td><td>VCP/CDC</td><td>15Mbps</td><td>15Mbps</td><td>√</td><td>√</td><td>内置</td><td>3.3V/2.5V/1.8V</td><td>√</td><td>RTS/CTS/DTR/DSR/DCD/RI
FIFO/SPI/GPIO*12</td><td>内置
外置</td><td>-40~+85°C</td><td>QFN26C3</td><td>高速USB
+多接口</td></tr><tr><td>CH9111L</td><td>480Mbps
高速</td><td>VCP/CDC</td><td>15Mbps</td><td>15Mbps</td><td>√</td><td>√</td><td>内置</td><td>3.3V/2.5V/1.8V</td><td>√</td><td>RTS/CTS/DTR/DSR/DCD/RI
SPI/GPIO*14</td><td>内置
外置</td><td>-40~+85°C</td><td>LQFP48</td><td>高速USB
+多接口</td></tr><tr><td>CH347T</td><td>480Mbps
高速</td><td>VCP/CDC
HID</td><td>9Mbps</td><td>9Mbps</td><td>√</td><td>√</td><td>内置</td><td>3.3V</td><td>-</td><td>RTS/CTS/DTR/DSR/DCD/RI
JTAG/SWD/SPI/IC/GPIO*8</td><td>外置</td><td>-40~+85°C</td><td>TSSOP20</td><td>高速USB
+多接口</td></tr><tr><td>CH343P</td><td>全速</td><td>VCP/CDC</td><td>6Mbps</td><td>6Mbps</td><td>√</td><td>√</td><td>内置</td><td>5V/3.3V/2.5V/1.8V</td><td>√</td><td>RTS/CTS/DTR/DSR/DCD/RI</td><td>内置</td><td>-40~+85°C</td><td>QFN16</td><td>第3代</td></tr><tr><td>CH343G</td><td>全速</td><td>VCP/CDC</td><td>6Mbps</td><td>6Mbps</td><td>√</td><td>√</td><td>批量定制</td><td>5V/3.3V/2.5V/1.8V</td><td>√</td><td>RTS/CTS/DTR/DSR/DCD/RI</td><td>内置</td><td>-40~+85°C</td><td>SOP16</td><td>第3代</td></tr><tr><td>CH343K</td><td>全速</td><td>VCP/CDC</td><td>6Mbps</td><td>6Mbps</td><td>√</td><td>-</td><td>批量定制</td><td>5V/3.3V/2.5V/1.8V</td><td>√</td><td>RTS/CTS/DTR</td><td>内置</td><td>-40~+85°C</td><td>ESSOP10</td><td>第3代</td></tr><tr><td>CH9102F</td><td>全速</td><td>VCP/CDC</td><td>4Mbps</td><td>4Mbps</td><td>√</td><td>√</td><td>内置</td><td>5V/3.3V/2.5V/1.8V</td><td>√</td><td>RTS/CTS/DTR/DSR/DCD/RI/GPIO*5</td><td>内置</td><td>-40~+85°C</td><td>QFN24</td><td>第3代</td></tr><tr><td>CH9102X</td><td>全速</td><td>VCP/CDC</td><td>4Mbps</td><td>4Mbps</td><td>√</td><td>√</td><td>批量定制</td><td>3.3V</td><td>-</td><td>RTS/CTS/DTR/DSR/DCD/RI/GPIO*6</td><td>内置</td><td>-40~+85°C</td><td>QFN28X5</td><td>第3代</td></tr><tr><td>CH9101U</td><td>全速</td><td>VCP/CDC</td><td>3Mbps</td><td>3Mbps</td><td>√</td><td>√</td><td>内置</td><td>5V/3.3V/2.5V/1.8V</td><td>√</td><td>RTS/CTS/DTR/DSR/DCD/RI/GPIO*6</td><td>内置</td><td>-40~+85°C</td><td>SSOP28</td><td>第3代</td></tr><tr><td>CH341F</td><td>全速</td><td>VCP</td><td>2Mbps</td><td>2Mbps</td><td>√</td><td>√</td><td>外置或
批量定制</td><td>5V/3.3V</td><td>-</td><td>RTS/CTS/DTR/DSR/DCD/RI
FIFO/SPI/FC</td><td>内置
外置</td><td>-20~+70°C
-40~+85°C</td><td>QFN28</td><td>第2代</td></tr><tr><td>CH341B</td><td>全速</td><td>VCP</td><td>2Mbps</td><td>2Mbps</td><td>√</td><td>√</td><td>外置或
批量定制</td><td>5V/3.3V</td><td>-</td><td>RTS/CTS/DTR/DSR/DCD/RI
FIFO/SPI/FC</td><td>内置
外置</td><td>-20~+70°C
-40~+85°C</td><td>SOP28</td><td>第2代</td></tr><tr><td>CH340B</td><td>全速</td><td>VCP</td><td>2Mbps</td><td>460800bps</td><td>-</td><td>√</td><td>内置</td><td>5V/3.3V</td><td>-</td><td>RTS/CTS/DSR/DCD/RI</td><td>内置</td><td>-20~+70°C</td><td>SOP16</td><td>第2代</td></tr><tr><td>CH340K</td><td>全速</td><td>VCP</td><td>230400bps</td><td>230400bps</td><td>-</td><td>-</td><td>-</td><td>5V/3.3V/2.5V/1.8V</td><td>仅防倒灌</td><td>DTR/RTS/CTS</td><td>内置</td><td>-20~+70°C</td><td>ESSOP10</td><td>经典优化版</td></tr><tr><td>CH340N</td><td>全速</td><td>VCP</td><td>2Mbps</td><td>460800bps</td><td>-</td><td>-</td><td>-</td><td>5V/3.3V</td><td>-</td><td>RTS</td><td>内置</td><td>-20~+70°C</td><td>SOP8</td><td>经典版+</td></tr><tr><td>CH340E</td><td>全速</td><td>VCP</td><td>2Mbps</td><td>460800bps</td><td>-</td><td>√</td><td>-</td><td>5V/3.3V</td><td>-</td><td>RTS/CTS</td><td>内置</td><td>-20~+70°C</td><td>MSOP10</td><td>经典版+</td></tr><tr><td>CH340C</td><td>全速</td><td>VCP</td><td>2Mbps</td><td>460800bps</td><td>-</td><td>-</td><td>-</td><td>5V/3.3V</td><td>-</td><td>RTS/CTS/DTR/DSR/DCD/RI/OUT</td><td>内置</td><td>-20~+70°C</td><td>SOP16</td><td>经典版+</td></tr><tr><td>CH341A</td><td>全速</td><td>VCP</td><td>2Mbps</td><td>2Mbps</td><td>√</td><td>√</td><td>外置</td><td>5V/3.3V</td><td>-</td><td>RTS/CTS/DTR/DSR/DCD/RI
FIFO/SPI/FC</td><td>外置</td><td>-40~+85°C</td><td>SOP28</td><td>经典增强版</td></tr><tr><td>CH341T</td><td>全速</td><td>VCP</td><td>2Mbps</td><td>2Mbps</td><td>√</td><td>√</td><td>外置</td><td>5V/3.3V</td><td>-</td><td>SCL/SDA</td><td>外置</td><td>-40~+85°C</td><td>SSOP20</td><td>经典增强版</td></tr><tr><td>CH340G</td><td>全速</td><td>VCP</td><td>2Mbps</td><td>460800bps</td><td>-</td><td>-</td><td>-</td><td>5V/3.3V</td><td>-</td><td>RTS/CTS/DTR/DSR/DCD/RI</td><td>外置</td><td>-40~+85°C</td><td>SOP16</td><td>经典版</td></tr><tr><td>CH340T</td><td>全速</td><td>VCP</td><td>2Mbps</td><td>460800bps</td><td>-</td><td>√</td><td>-</td><td>5V/3.3V</td><td>-</td><td>RTS/CTS/DTR/DSR/DCD/RI</td><td>外置</td><td>-40~+85°C</td><td>SSOP20</td><td>经典版</td></tr><tr><td>CH9329</td><td>全速</td><td>HID</td><td>115200bps</td><td>115200bps</td><td>-</td><td>-</td><td>内置</td><td>5V/3.3V</td><td>-</td><td>-</td><td>内置</td><td>-40~+85°C</td><td>SOP16</td><td>-</td></tr><tr><td>CH9143</td><td>全速</td><td>VCP/CDC</td><td>1Mbps</td><td>230400bps</td><td>√</td><td>-</td><td>批量定制</td><td>3.3V/2.5V</td><td>-</td><td>RTS/CTS/DTR/DSR/DCD/RI
蓝牙无线传输</td><td>外置</td><td>-40~+85°C</td><td>QFN28</td><td>BLE+USB</td></tr></table>

USB转多串口芯片  

<table><tr><td>型号</td><td>串口数</td><td>USB</td><td>驱动类型①</td><td>峰值最高波特率</td><td>流控连续波特率</td><td>硬件流控</td><td>自动控制RS485</td><td>USB配置</td><td>IO电压(MCU电压)</td><td>省电双供电IO防倒灌</td><td>MODEM信号(兼GPIO和其他接口)②</td><td>时钟</td><td>温度范围</td><td>封装</td><td>内核</td></tr><tr><td>CH346C</td><td>2</td><td>480Mbps高速</td><td>VCP/CDC</td><td>15Mbps</td><td>15Mbps</td><td>√</td><td>√</td><td>内置</td><td>3.3V/2.5V/1.8V</td><td>√</td><td>RTS/CTS/DTR/DSR/DCD/RI FIFO/SPI/GPIO*12</td><td>内置/外置</td><td>-40~+85°C</td><td>QFN26C3</td><td>高速USB+多接口</td></tr><tr><td>CH347F</td><td>2</td><td>480Mbps高速</td><td>VCP/CDC/HID</td><td>9Mbps</td><td>9Mbps</td><td>√</td><td>√</td><td>内置</td><td>3.3V/2.5V/1.8V</td><td>√</td><td>RTS/CTS/DTR JTAG/SWD/SPI/I2C/GPIO*8</td><td>外置</td><td>-40~+85°C</td><td>QFN28</td><td>高速USB+多接口</td></tr><tr><td>CH347T</td><td>2</td><td>480Mbps高速</td><td>VCP/CDC/HID</td><td>9Mbps</td><td>9Mbps</td><td>√</td><td>√</td><td>内置</td><td>3.3V</td><td>-</td><td>RTS/CTS/DTR/DSR/DCD/RI JTAG/SWD/SPI/I2C/GPIO*8</td><td>外置</td><td>-40~+85°C</td><td>TSSOP20</td><td>高速USB+多接口</td></tr><tr><td>CH342F</td><td>2</td><td>全速</td><td>VCP/CDC</td><td>3Mbps</td><td>3Mbps</td><td>√</td><td>√</td><td>内置</td><td>5V/3.3V/2.5V/1.8V</td><td>√</td><td>RTS/CTS/DTR/DSR/DCD/RI</td><td>内置</td><td>-40~+85°C</td><td>QFN24</td><td>-</td></tr><tr><td>CH342K</td><td>2</td><td>全速</td><td>VCP/CDC</td><td>3Mbps</td><td>3Mbps</td><td>√</td><td>√</td><td>批量定制</td><td>5V/3.3V/2.5V/1.8V</td><td>√</td><td>-</td><td>内置</td><td>-40~+85°C</td><td>ESSOP10</td><td>-</td></tr><tr><td>CH344L</td><td>4</td><td>全速</td><td>VCP/CDC</td><td>2Mbps</td><td>2Mbps</td><td>√</td><td>√</td><td>内置</td><td>3.3V</td><td>-</td><td>RTS/CTS/GPIO*12</td><td>内置/外置</td><td>-40~+85°C</td><td>LQFP48</td><td>-</td></tr><tr><td>CH344Q</td><td>4</td><td>480Mbps高速</td><td>VCP/CDC</td><td>6Mbps</td><td>6Mbps</td><td>√</td><td>√</td><td>内置</td><td>3.3V</td><td>-</td><td>RTS/CTS/DTR/DSR/DCD/RI/GPIO*16</td><td>外置</td><td>-40~+85°C</td><td>LQFP48</td><td>-</td></tr><tr><td>CH9114L</td><td>4</td><td>480Mbps高速</td><td>VCP/CDC</td><td>15Mbps</td><td>15Mbps</td><td>√</td><td>√</td><td>内置</td><td>3.3V/2.5V/1.8V</td><td>√</td><td>RTS/CTS/DTR/DSR/DCD/RI/GPIO*24</td><td>内置/外置</td><td>-40~+85°C</td><td>LQFP64M</td><td>-</td></tr><tr><td>CH9114W</td><td>4</td><td>480Mbps高速</td><td>VCP/CDC</td><td>15Mbps</td><td>15Mbps</td><td>√</td><td>√</td><td>内置</td><td>3.3V/2.5V/1.8V</td><td>√</td><td>RTS/CTS/DTR/DSR/DCD/RI/GPIO*24</td><td>内置/外置</td><td>-40~+85°C</td><td>QFN56x8</td><td>-</td></tr><tr><td>CH9114F</td><td>4</td><td>480Mbps高速</td><td>VCP/CDC</td><td>15Mbps</td><td>15Mbps</td><td>√</td><td>√</td><td>内置</td><td>3.3V/2.5V/1.8V</td><td>√</td><td>RTS/CTS/DTR/RI/GPIO*12</td><td>内置/外置</td><td>-40~+85°C</td><td>QFN32</td><td>-</td></tr><tr><td>CH9344Q</td><td>4</td><td>480Mbps高速</td><td>VCP</td><td>12Mbps</td><td>12Mbps</td><td>√</td><td>√</td><td>内置</td><td>3.3V/2.5V/1.8V</td><td>√</td><td>RTS/CTS/DTR/DSR/DCD/RI/GPIO*12</td><td>内置/外置</td><td>-40~+85°C</td><td>LQFP48</td><td>-</td></tr><tr><td>CH348L</td><td>8</td><td>480Mbps高速</td><td>VCP</td><td>6Mbps</td><td>6Mbps</td><td>√</td><td>√</td><td>内置</td><td>3.3V/2.5V/1.8V</td><td>√</td><td>RTS/CTS/DTR/DSR/DCD/RI/GPIO*48</td><td>外置</td><td>-40~+85°C</td><td>LQFP100</td><td>-</td></tr><tr><td>CH348Q</td><td>8</td><td>480Mbps高速</td><td>VCP</td><td>6Mbps</td><td>6Mbps</td><td>√</td><td>√</td><td>内置</td><td>3.3V</td><td>√</td><td>RTS/CTS/GPIO*12</td><td>外置</td><td>-40~+85°C</td><td>LQFP48</td><td>-</td></tr></table>

①沁恒提供多种USB串口驱动程序,支持Windows/Linux/Android/macOS等操作系统。驱动类型说明:

VCP：厂商提供仿真串口驱动，支持各操作系统，功能多，效率高，支持高波特率通讯、硬件流控、GPIO等功能。驱动只需安装一次，也可以联网自动安装。

HID：所有操作系统已内置此类驱动程序，用户无需安装驱动；缺点是通讯效率低，无法仿真串口和使用常规串口应用软件。

CDC:低于Windows10的操作系统版本,需安装驱动。因CDC类协议和类驱动的原因,CDC串口功能没有VCP完整,使用上也存在一些差异,具体见如下方案说明。

②部分芯片型号除串口功能外，还提供FIFO并口、JTAG/SWD接口、SPI接口、I²C等硬件接口，用于满足其他场景的数据传输需求。具体参见各芯片数据手册。

③USB转串口芯片CH910X/CH911X系列：CH9101U、CH9101H、CH9101Y、CH9101R、CH9101N、CH9102F、CH9102X、CH9111L、CH9103M、CH9104L、CH9114L、CH9114W等型号是为了满足用户的国产化替代需求而推出的引脚兼容型号。


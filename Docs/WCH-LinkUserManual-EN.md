# WCH-LinkUserManual

Version: V2.6

https://wch-ic.com

## 1 WCH-Link

### 1.1 Module Introduction

WCH-Link module can be used for online debugging and downloading of WCH RISC-V MCU, and also for online debugging and downloading of ARM MCU with SWD/JTAG interface. It also comes with a serial port for easy debugging output. There are currently 3 WCH-Links including WCH-LinkE, WCH-DAPLink, and WCH-LinkW, and WCH-LinkE is recommended.

![](images/81f295f95668cc1873ec798a1ee061aa87cff15be3d1001183a0e9632e6dd2cc.jpg)  
Figure 1 WCH-Link physical diagram

![](images/35e0b0aa28800c94153ac7120845d3946d16b36e667943326bafe5c474afdc41.jpg)

![](images/da10b77f4d1adb6855793cdef1db96a2b4a0cef89c0e0fc5c09e7442291cd55d.jpg)

![](images/fb84ea0b3670673017d812b0221b232100e90d8165918d65c3f33044b928f880.jpg)  
Figure 2 WCH-Link mode

Table 1 WCH-Link mode   

<table><tr><td>Mode</td><td>Status LED</td><td>IDE</td><td>Support chip</td></tr><tr><td>RISC-V</td><td>Blue LED is always off when idle</td><td>MounRiver Studio</td><td>WCH RISC-V core chips that support single/dual line debugging</td></tr><tr><td>ARM</td><td>Blue LED is always on when idle</td><td>Keil/MounRiver Studio</td><td>ARM core chips that support SWD/JTAG protocol</td></tr></table>

### 1.2 Mode Switching

Way 1: Use MounRiver Studio software to switch Link mode. (This method is applicable to WCH-Link, WCH-LinkE and WCH-LinkW)

$\textcircled{1}$ Click arrow in the shortcut toolbar to bring up the project download configuration window   
$\textcircled{2}$ Click Query on the right side of Target Mode to view the current Link mode   
$\textcircled{3}$ Click Target Mode option box, select the target Link mode, click Apply

![](images/866736181d9732b8504147b6540749ada18a17c92d2c9cd0bfe901c192e0300d.jpg)

Way 2: Use WCH-LinkUtility tool to switch Link mode.

$\textcircled{1}$ Click Get on the right side of Active WCH-Link mode to view the current Link mode   
$\textcircled{2}$ Click Active WCH-Link mode option box, select the target Link mode, click Set

![](images/31dd138d055408a233d7186c5de77545a656caab063860c6e7ea53483f6db831.jpg)

Way 3: Use ModeS key to switch Link mode. (This method is applicable to WCH-LinkE-R0-1v2, WCH-DAPLink-R0-2v0 and WCH-LinkW-R0-1v1 and above)

① Press and hold the ModeS key to power up the Link

#### Notes:

(1) The blue LED flashes when downloading and debugging.   
(2) The Link maintains the switched mode for subsequent use.   
(3) WCH-Link simulation debugger module URL: https://www.wch-ic.com/products/WCH-Link.html   
(4) MounRiver Studio Access URL: http://mounriver.com/   
(5) WCH-LinkUtility Access URL: https://www.wch.cn/downloads/WCH-LinkUtility_ZIP.html   
(6) WCHISPTool Access URL: https://www.wch.cn/downloads/WCHISPTool_Setup_exe.html   
(7) WCH-Link, WCH-LinkE and WCH-LinkW support LinkRV and LinkDAP-WINUSB mode switching; WCH-DAPLink supports LinkDAP-WINUSB and LinKDAP-HID mode switching.

### 1.3 Serial Port Baud Rate

Table 2 WCH-Link serial port supports baud rate   

<table><tr><td>1200</td><td>2400</td><td>4800</td><td>9600</td><td>14400</td></tr><tr><td>19200</td><td>38400</td><td>57600</td><td>115200</td><td>230400</td></tr></table>

Table 3 WCH-LinkE/DAPLink/LinkW serial port supports baud rate   

<table><tr><td>1200</td><td>2400</td><td>4800</td><td>9600</td><td>14400</td><td>19200</td></tr><tr><td>38400</td><td>57600</td><td>115200</td><td>230400</td><td>460800</td><td>921600</td></tr></table>

#### Notes:

(1) Figure 1 in the row of pins RX and TX for the serial port transceiver pins, serial port support baud rate is shown in the table above.   
(2) CDC driver needs to be installed under Win7.   
(3) If you re-unplug Link, please re-open the serial debugging assistant.

### 1.4 Function Comparison

Table 4 Link functions and performance comparison table   

<table><tr><td>Function items</td><td>WCH-Link-R1-1v1</td><td>WCH-LinkE-R0-1v3</td><td>WCH-DAPLink-R0-2v0</td><td>WCH-LinkW-R0-1v1</td></tr><tr><td>RISC-V mode</td><td>✓</td><td>✓</td><td>×</td><td>✓</td></tr><tr><td>ARM-SWD mode-HID device</td><td>×</td><td>×</td><td>✓</td><td>×</td></tr><tr><td>ARM-SWD mode-WINUSB device</td><td>✓</td><td>✓</td><td>✓</td><td>✓</td></tr><tr><td>ARM-JTAG mode-HID device</td><td>×</td><td>×</td><td>✓</td><td>×</td></tr><tr><td>ARM-JTAG mode-WINUSB device</td><td>×</td><td>✓</td><td>✓</td><td>✓</td></tr><tr><td>ModeS key to switch mode</td><td>×</td><td>✓</td><td>✓</td><td>✓</td></tr><tr><td>2-wire way upgrade firmware offline</td><td>×</td><td>✓</td><td>×</td><td>×</td></tr><tr><td>Serial port upgrade firmware offline</td><td>✓</td><td>×</td><td>×</td><td>×</td></tr><tr><td>USB upgrade firmware offline</td><td>✓</td><td>×</td><td>✓</td><td>✓</td></tr><tr><td>Controllable 3.3V/5V power output</td><td>×</td><td>✓</td><td>✓</td><td>✓</td></tr><tr><td>High-speed USB2.0 to JTAG interface</td><td>×</td><td>✓</td><td>×</td><td>×</td></tr><tr><td>Wireless mode</td><td>×</td><td>×</td><td>×</td><td>✓</td></tr><tr><td>Download tools</td><td>MounRiver Studio
WCH-LinkUtility
Keil uVision5</td><td>MounRiver Studio
WCH-LinkUtility
Keil uVision5</td><td>WCH-LinkUtility
Keil uVision5</td><td>MounRiver Studio
WCH-LinkUtility
Keil uVision5</td></tr><tr><td>Keil supported versions</td><td>Keil V5.25 and above</td><td>Keil V5.25 and above</td><td>Supported in all versions of Keil</td><td>Keil V5.25 and above</td></tr></table>

## 2 Pin Connections

Table 5 Link supported chip model   

<table><tr><td>Common chip models</td><td>WCH-Link</td><td>WCH-LinkE</td><td>WCH-DAPLink</td><td>WCH-LinkW</td></tr><tr><td>CH643/CH32X035_X033/CH32L103/CH32V003/CH32V002_004_005_006_007/CH32V317/CH32M007</td><td>x</td><td>✓</td><td>x</td><td>✓</td></tr><tr><td>CH32V10x/CH32V20x/CH32V30x</td><td>✓</td><td>✓</td><td>x</td><td>✓</td></tr><tr><td>CH569/CH573/CH583</td><td>✓</td><td>✓</td><td>x</td><td>x</td></tr><tr><td>CH59x/CH641/CH645/CH564/CH584_585_581/CH32M030/CH570_CH572/CH32H417_415_416/CH32V205_V203CC/CH595_596</td><td>x</td><td>✓</td><td>x</td><td>x</td></tr><tr><td>CH32F10x/CH32F20x/CH579/friendly chips that support SWD interface</td><td>✓</td><td>✓</td><td>✓</td><td>✓</td></tr><tr><td>friendly chips that support JTAG interface</td><td>x</td><td>✓</td><td>✓</td><td>✓</td></tr></table>

Table 6 Common chip pin connections   

<table><tr><td>Common chip models</td><td>SWDIO</td><td>SWCLK</td></tr><tr><td>CH569</td><td>PA11</td><td>PA10</td></tr><tr><td>CH579</td><td>PB16</td><td>PB17</td></tr><tr><td>CH573/CH583/CH59x/CH584_585_581</td><td>PB14</td><td>PB15</td></tr><tr><td>CH643/CH32X035_X033</td><td>PC18</td><td>PC19</td></tr><tr><td>CH32V003</td><td>PD1</td><td>-</td></tr><tr><td>CH641</td><td>PB0</td><td>-</td></tr><tr><td>CH645/CH570_CH572</td><td>PA0</td><td>PA1</td></tr><tr><td>CH32V10x/CH32V20x/CH32V30x/CH32F10x/CH32F20x/CH32L103/CH32V317/CH32V205_203CC</td><td>PA13</td><td>PA14</td></tr><tr><td>CH564</td><td>PB10</td><td>PB11</td></tr><tr><td>CH32V002_004_005_006_007/CH32M007</td><td>PD1</td><td>PB3</td></tr><tr><td>CH32M030</td><td>PA3</td><td>PA2</td></tr><tr><td>CH32H417_415_416</td><td>PB9</td><td>PB8</td></tr><tr><td>CH595_596</td><td>PA12</td><td>PA13</td></tr></table>

Note:   
(1) Among them, CH564, CH32M030, CH584_585_581, CH570_572, CH32V205_203CC, CH32H417_415_416, CH32V002_004_005_006_007, CH595_596 and CH32M007 support both 1-wire (SWDIO) and 2-wire (SWDIO-SWCLK) debug interfaces.   
(2) WCH-Link-R1-1v1 has been discontinued and is still officially maintained, but no new features are added.

Table 7 STM32F10xxx JTAG interface pinout   

<table><tr><td>JTAG interface pin name</td><td>JTAG debug interface</td><td>Pinout</td></tr><tr><td>TMS</td><td>JTAG mode selection</td><td>PA13</td></tr><tr><td>TCK</td><td>JTAG clock</td><td>PA14</td></tr><tr><td>TDI</td><td>JTAG data input</td><td>PA15</td></tr><tr><td>TDO</td><td>JTAG data output</td><td>PB3</td></tr></table>

### Notes:

(1) Link maximum supported line length: 30cm, if the download process is unstable, try to turn down the download speed.   
(2) JTAG mode, WCH-LinkE-R0-1v3, WCH-DAPLink-R0-2v0 hardware version began to support, the previous hardware version does not support.   
(3) WCH-LinkE high-speed version is only for CH32F20x/CH32V20x/CH32V30x to speed up.   
(4) CH569、 CH579、 CH573、 CH583、 CH59x、 CH584_585_581, if you want to use Link for downloading or debugging, you need to use the official ISP tool to open the 2-wire debug interface, and you need to pay attention to Link mode when using it. The steps are as follows:

Open WCHISPStudio tool, the chip to be tested enters BOOT mode

-CH569 needs to short HD0 and GND to power on through U-port;

-CH573/CH583/CH59x/ CH584_585_581 need to press and hold the Download button to power on through the U-port;

WCHISPStudio tool will automatically pop up the adaptation window, click to open the two-wire debug interface

## 3 Keil Download and Debug

### 3.1 Device Switching

WCH-DAPLink supports two modes, ARM mode-WINUSB device and ARM mode-HID device, and you can switch between the two device modes with the WCH-LinkUtility tool (or by powering up the Link after long pressing the ModeS key.) WCH-Link, WCH-LinkE and WCH-LinkW only support ARM mode-WINUSB device mode.

![](images/aebd1f44e18a9a5d337f254f04484285e72ae31921eb4715abe702371a02bced.jpg)

Table 8 WCH-DAPLink device   

<table><tr><td>Device</td><td>Support Link</td><td>Keil supported versions</td></tr><tr><td>ARM mode-WINUSB device</td><td>WCH-LinkWCH-LinkEWCH-DAPLinkWCH-LinkW</td><td>Keil V5.25 and aboveARM-CMSIS V5.3.0 and above</td></tr><tr><td>ARM mode-HID device</td><td>WCH-DAPLink</td><td>Supported in all versions of Keil</td></tr></table>

Note:   
(1) WCH-Link, WCH-LinkE, WCH-DAPLink and WCH-LinkW are factory defaulted to WINUSB device mode.   
(2) WCH-DAPLink-R0-1v0 switches between two device modes by long pressing the IAP key to power up.   
(3) WCH-LinkUtility tool on will occupy the Link device and cause Keil software can not recognize the Link.

### 3.2 Download Configuration

① Click the magic wand in the toolbar to bring up the Options for Target dialog box, click Debug and select the emulator model

![](images/b7fdfe78a1f0627086f548af20eae17a024afbf46be40844e2a5ab173d81a002.jpg)

$\textcircled{2}$ Click the Use option box and select CMSIS-DAP Debugger   
$\textcircled{3}$ Click the Settings button to bring up the Cortex-M Target Driver Setup dialog box

![](images/24ccc6759ce602fadf92581cff3bc730ef1b831463780fcecf1e7932448cd7f1.jpg)

Serial No: Display the identifier of the debug adapter being used. When multiple adapters are connected, you can specify the adapter by using the drop-down list.

SW Device: Show the device ID and name of the connected device.

Port: Set the internal debug interface SW or JTAG. (Both interfaces are supported by WCH-LinkE-R0-1v3, WCH-DAPLink-R0-2v0 and WCH-LinkW-R0-1v1)

Max Clock: Set the clock rate to communicate with the target device.

④ Click Flash Download for download configuration

![](images/3f71fa02ec6d2d3407a672babc712973c692350ca6c9b959de226d615bbe5cdf.jpg)

Download Function: Configuration options

RAM for Algorithm: Configure the starting address and size of RAM space

Our CH32F103 series chip RAM space size is 0x1000, CH32F20x series chip RAM space size is 0x2800.

Programming Algorithm: Add algorithm file

The algorithm file has been added automatically after installing the chip device package, click OK.

$\textcircled{5}$ After completing the above configuration, click OK to close the dialog box. Click the icon in the toolbar to burn in the code.

### 3.3 Debug

$\textcircled{1}$ Click the Debug button $\Subset$ in the toolbar to enter the debug page   
$\textcircled{2}$ Set breakpoints

![](images/2074eb5d802e21b2bb175569baa314ba7136c49a39f6fa86627dffec907689ca.jpg)

③ Basic debug commands

![](images/36d69d52ff7958c1e9e8d28a19e2a788731dd6d9272d40fc2746d9e116031b2c.jpg)

Reset: Perform a reset operation on the program.

Run: Cause the current program to start running at full speed until the program stops when it encounters a breakpoint.   
Step: Execute a single statement and if a function is encountered, it will go inside the function.   
Step Over: Execute a single statement that does not go inside the function if it encounters a function, but runs the function at full speed and jumps to the next statement.   
$\{ \mathfrak { F }$ Step Out: Run all the contents after the current function at full-speed until the function returns to the previous level.

$\textcircled{4}$ Click the Debug button $\Subset$ in the toolbar again to exit debug.

## 4 MounRiver Studio Download and Debug

### 4.1 Download Configuration

① Click the arrow in the toolbar to bring up the project download configuration window   
$\textcircled{2}$ Click the Disable Read-Protect button to disable the chip read protection

![](images/84f6cec921565c66c394981ca1596085bda90e9cb3b310b598e7fb1bfab1c69a.jpg)

③ Target configuration, the main elements are as follows.

![](images/de361f812542f8820d7f0d0ccc415083d2667c908859a5fdceed9efa581896c6.jpg)

④ Configuration Options

![](images/2210fe11119d5a1b19d9145a729673d6bbf2ffc56c389eb6d81cf8cad994b113.jpg)

⑤ Click Apply and Close to save the download configuration. Click on the icon in the toolbar to burn the code, and the result will be displayed in the Console.

### 4.2 Debug

① Enter the debugging page

Way 1: Click the Debug button in the toolbar to enter the debug page directly.

Way 2: Click the arrow in the toolbar and select Debug Configurations to pop up the debug configuration page. Double-click GDB OpenOCD MRS Debugging to generate the obj file, select the obj file and click the Debug button at the bottom right corner to enter the debugging page.

![](images/11ac2ca33fa41209dba48e98832c847bb8ad8c4f9fe8c16745dbf377e7237c80.jpg)

#### ② Set breakpoints

![](images/7ff4d34ca8aff8cba57ebe5ac9215732ca61982922f3b5eccfbe13a597af13a9.jpg)

#### ③ Basic debug commands

![](images/f2056d5815e0d254c5f1a53f5c8d36899123f77650299c6703ec93be0c246169.jpg)

Reset: Perform a reset operation on the program.

Run: Make the current program start running at full speed until the program stops when it meets a breakpoint.

![](images/a2db0a989feed50991ff5f53db1036701c3c8a12aa43a9b7c9f026b599df2de7.jpg)

Terminate: Exit debugging.

$\Bumpeq$ Step Into: Execute a single statement, and if a function is encountered, it will go inside the function.   
Step Over: Execute a single statement, and if it encounters a function, it will not go inside the function, but run the function at full speed and skip to the next statement.   
Step Return: Run all contents after the current function at full speed until the function returns to the previous level.

Click button, exit the debug.

### 4.3 Other Functions

#### 4.3.1 Set Chip Read-Protect

![](images/b6a90b4d2bb5420efe6679e763d353878da1a3947597923150c5b8bd7ea912af.jpg)

Query chip read-protect status

![](images/a501cd5c23680b949bdb8fcd0a2b8fcd0b5715eea3bca40864cf19a498940cbf.jpg)

Enable chip read-protect status

![](images/45395111aec19bb1b715a86f381095d208a2f25813eb7a31a69041a2fb6b0612.jpg)

Disable chip read-protect status

#### 4.3.2 Code Flash Full Erase

MounRiver Studio can erase all the user areas of the chip by controlling the hardware reset pin or by re-powering the chip. To control erase by re-powering, Link is required to power the chip; to control erase by hardware reset pin, the reset pins of the chip and Link need to be connected. (Supported by WCH-LinkE, WCH-DAPLink and WCH-LinkW only)

![](images/175dbf85cf30556d5b98c8468bf8ceb1a9be3a4f15216515ef5d085d1070756e.jpg)

#### 4.3.3 Disable 2-wire SDI

For partial chips, code and data protection can be enabled by disabling the 2-wire SDI.

![](images/aa3a79b146b5aef2f3fd1bb13a43062de0a305e43b8ac85155c45d16634c1fe8.jpg)

Disable the 2-wire SDI

#### 4.3.4 Chip Memory Allocation

For high-capacity general-purpose (connected/interconnected/wireless) chips, memory allocation is available through MounRiver Studio; please refer to the User Selection Word section of the CH32FV2x_V3xRM manual for details.

![](images/a880712d261151308721c408b1caf8f0e727aa6a38175ff5759001b44778337d.jpg)

## 5 WCH-LinkUtility Download

### 5.1 Download Configuration

$\textcircled{1}$ Click the icon connect to Link   
$\textcircled{2}$ Select the chip model

![](images/4951fb80b8c98335608372dda5bff88599a6e62bf5b1ede1756ea3f0974f8a23.jpg)

$\textcircled{3}$ Configuration options

![](images/88dfeab490298793a898564bc4159d52a4c7f5f98acce7b6372808cddb025fd2.jpg)

④ Release chip read protection and set two-line debug speed

![](images/7e3dbc2fdb3542cb349383416237f649d2bc9577ff203c53ce084f59e120351d.jpg)

⑤ Click icon to add firmware

⑥ Click icon to execute download

### 5.2 Other Functions

#### 5.2.1 Query Chip Information

Click icon to query chip information

<table><tr><td>Name</td><td>Value</td></tr><tr><td>MCU UID</td><td>17-9f-ab-cd-7f-b4-bc-48</td></tr><tr><td>Flash Size</td><td>16 KB</td></tr><tr><td>Read-ProTECT</td><td></td></tr><tr><td>Link Version</td><td>V2.8</td></tr><tr><td></td><td></td></tr></table>

#### 5.2.2 Set Chip Read-Protect

![](images/653e6b2eba00696e9dde42a3913c14628ce0d965c3d7509a03e26395ea2fc9cb.jpg)

Query chip read-protect status

![](images/3a40b86d89a72e98c49567bdd97281409f17ef4816ed8fe1ab7b69727cd1ab27.jpg)

Enable chip read-protect status

![](images/14e8661dc74247dadb4fbdd228809e55430311d69fadc34c2bdbbd92ae47af24.jpg)

Disable chip read-protect status

#### 5.2.3 Read Chip Flash

Click icon Flach to read chip Flash

![](images/68762ac92fead8e7caae4aadb23688dbef2fa1d40c44eda044c6c43cbdd99c10.jpg)

#### 5.2.4 Code Flash Full Erase

The WCH-LinkUtility tool can erase all user areas of the chip by controlling the hardware reset pin or by re-powering the chip. To control erase by re-powering, Link is required to power the chip; to control erase by hardware reset pin, the reset pins of the chip and Link are required to be connected. (Supported by WCH-LinkE, WCH-DAPLink and WCH-LinkW only)

![](images/c132370c579dc06acf56022c277b48c0724725d329002e5c78b484644b42211c.jpg)

#### 5.2.5 Power Output Controllable

WCH-LinkUtility tool can control Link power output. Click on Target and choose to turn on/off the power supply 3.3V/5V output in the drop-down list. (Supported by WCH-LinkE, WCH-DAPLink and WCH-LinkW only)

![](images/209aec8bc6d9d9f7a0c5c16ccc5d4d9b8a9febfe857d444e796eeb7b9a1b72b1.jpg)

#### 5.2.6 Automatic Continuous Download

Tick Auto download when WCH-Link was linked to enable automatic continuous download of the project.

![](images/5920c0ae23c54d8735fddc12e435ce910d5005afa844fa5b7f24e5b888e6a818.jpg)

#### 5.2.7 Multi-Device Download

The WCH-LinkUtility tool can recognize multiple Link devices. When multiple Links are connected, the Connected WCH-Link List option box allows you to select a specific Link device for downloading.

![](images/0eece69c15edd3e993250b6f3a5d13f62c9dd5f2c934cc7ddb77844611f5ebdf.jpg)

#### 5.2.8 Turn off the 2-wire Debug Interface

For partial chips, code and data protection can be enabled by closing the two-wire debug interface.

![](images/525beae0b6086b8825a2da6f85441816181912659541b662b2d1a2b7798d4ae4.jpg)

#### 5.2.9 User Select Word Configuration

For CH32 series chips, user selectable word configuration can be done through the WCH-LinkUtility tool. For details, please refer to the user selectable word section in the RM manual.

![](images/6f141b0986099c9384e72b83a7277a3a3993788c57fae0f5b5f26348637473ce.jpg)

![](images/224ba4595b127c2a28654b98f9b301f3f5c50bd608d1ca2992a8f3047b0620ba.jpg)

#### 5.2.10 BOOT Download

For CH32V003, CH641, CH32V002_004_005_006_007, CH32M007, CH32X035_X033 chip, you can select program download to program flash memory storage or system storage by WCH-LinkUtility tool.

![](images/a7b9dec74188bbaed809bb53827897b4e83fe6de2d1525600258000ffdb52ce3.jpg)

#### 5.2.11 SDI Virtual Serial Port Function

This function uses the SDI interface to realize the chip printout function, which needs to modify the printing function. Refer to the SDI_Printf routine in the relevant EVT. This function is only supported in V1.80 and above. You need to check EnableSDIPrintf and open the COM port of WCH-LinkE.

![](images/e127c5dfca4d0d5d98eedb017274c84aebc6bf903312b59be6b55c76beab2957.jpg)

Note:

(1) This feature is only supported in V1.80 and above.   
(2) This feature is only supported by WCH-LinkE.   
(3) The supporting chip includes CH32V003, CH32V103, CH32V20x, CH32V30x, CH32X035_X033, CH32L103, CH641, CH32V002_004_005_006_007, CH32M007, CH32M030, CH32V317, CH32V205_203CC.

## 6 Firmware Update Methods

### 6.1 MounRiver Studio Online Update

If the firmware needs to be updated, MounRiver Studio will have a pop-up window to remind you when you click the download button, click Yes to start the update.

![](images/e7b58e173c7d03e996c0a7e545495aec6ed37b3b7e17e701b8d1d231eeb0aa2b.jpg)

### 6.2 WCH-LinkUtility Online Update

If the firmware needs to be updated, WCH-LinkUtility will have a pop-up window to remind you when you click the download button, click Yes to start the update.

![](images/a5c652d5ef44905d852e1e49ab2fa76aabad6cb4d80f08f1a3d3a3c74f2f3138.jpg)

#### Notes:

(1) WCH-LinkE supports manual online update, the steps are as follows.   
Power up the Link after long press the IAP button until the blue LED blinks.   
MounRiver Studio/WCH-LinkUtility will have a pop-up window to remind you when you click the download button, click Yes to start the update.   
(2) If the Link firmware update is abnormal, please update the firmware by offline update.

### 6.3 WCH-LinkUtility Offline Update (2-wire Approach to Offline Update)

① Connect WCH-LinkE with WCH-LinkE to be updated

<table><tr><td>WCH-LinkE</td><td>Link to be updated</td></tr><tr><td>3V3</td><td>3V3</td></tr><tr><td>GND</td><td>GND</td></tr><tr><td>SWDIO</td><td>SWDIO</td></tr><tr><td>SWCLK</td><td>SWCLK</td></tr></table>

② WCH-LinkE power on, select the Link chip model to be updated (WCH-LinkE main control chip is CH32V30x)   
$\textcircled{3}$ To be updated Link into IAP mode (long press the IAP button to power up the Link, that is, through the USB port connected to the computer to power up)   
④ Click Target->Clear All Code Flash-By Power off to erase all the user area of the chip

![](images/000a9c423d223be58206d1751210a815f9fc3c6db67fee7c0f91ed4d97e844a9.jpg)

⑤ Click icon , diaable chip read-protect

![](images/9ba5fb3ed50f2335f7751a33fc2bc50218ace63c613276c84be0ff788863fc58.jpg)

$\textcircled{6}$ Click icon , add Link offline updated firmware   
$\textcircled{7}$ Configuration options (Program $^ +$ Verify $^ +$ Reset and Run)

![](images/22688d2046f13396195ff7999efa2ac8e6fc90a3ffe946050674704f77b1eaee.jpg)

⑧ Click icon to execute download

#### Notes:

(1) The Link to be updated is limited to WCH-LinkE.   
(2) Two WCH-LinkE are required for this method.   
(3) When Link enters IAP mode, the blue LED flashes.

### 6.4 WCHISPStudio Serial Port Offline Update

① Connect WCH-Link with USB to TTL module

<table><tr><td>WCH-Link</td><td>USB to TTL module</td></tr><tr><td>TX</td><td>RX</td></tr><tr><td>RX</td><td>TX</td></tr><tr><td>GND</td><td>GND</td></tr></table>

$\textcircled{2}$ USB to TTL module power on, WCH-Link into BOOT mode (short connection J1 in Figure 1 will Link power on)   
$\textcircled{3}$ Select chip model: CH549, download interface: serial port, device list: select the serial port number corresponding to the USB to TTL module

![](images/b308d001d6d684b834b6f6a389c0478adb5df13b5f34d6a847cf8b74a7081d21.jpg)

$\textcircled{4}$ Add Link offline updated firmware to target program file   
$\textcircled{5}$ Download configuration

![](images/4dd8ef8510fee1a08d87769b09b57bf4770e70c918ec9e6f924a44e382211893.jpg)

$\textcircled{6}$ Click the download button   
$\textcircled{7}$ Click on the download and wait for the device to access the field, then plug the WCH-Link into the USB port, the ISP tool automatically began to download

Note: Serial port offline update is only supported by WCH-Link.

### 6.5 WCHISPStudio USB Offline Update

$\textcircled{1}$ To update the Link into BOOT mode (short connect J1 in Figure 1 or long press BOOT key and then power up the Link)   
$\textcircled{2}$ WCHISPStudio tool will automatically pop up the adaptation window   
$\textcircled{3}$ Add Link offline upgrade firmware to the target program file   
$\textcircled{4}$ Download configuration

![](images/4490bc2b01b96c6fabdadb6b5413ff573108068002a726759cd17c3685038707.jpg)

⑤ Click the download button

Notes:

(1) USB offline update is only supported by WCH-Link, WCH-DAPLink and WCH-LinkW.   
(2) WCH-LinkE-R0-1v3 and WCH-DAPLink-R0-2v0 are only available for firmware version $\nu 2 . 8$ and above.   
(3) WCH-LinkUtility tool can be exported through MounRiver Studio software.

![](images/6ab4e08ac65622986d3a97e26c9bd2b07ab824dd43b5776be8282f80dea86d23.jpg)

(4) Link offline upgrade firmware is located in the MounRiver Studio installation path and WCH-LinkUtility installation path.

![](images/bd79a217c8a72850aa4890b030f0015b7ff5379d851b98ad7e8789559b337391.jpg)

①WCH-DAPLink upgrade firmware   
@WCH-LinkW upgrade firmware   
③WCH-LinkE upgrade firmware   
④WCH-Link RISC-V mode upgrade firmware   
③WCH-Link ARM mode upgrade firmware   
$\textcircled{6}$ WCH-DAPLink offline upgrade firmware   
$\textcircled{7}$ WCH-Link ARM mode offline upgrade firmware   
$\textcircled{8}$ WCH-Link RISC-V mode offline upgrade firmware   
$\textcircled{9}$ WCH-LinkE offline upgrade firmware   
@WCH-LinkW offline upgrade firmware

## 7 WCH-LinkE High-speed JTAG

### 7.1 Module Overview

The WCH-LinkE-R0-1v3 provides a JTAG interface that supports 4-wire connections (TMS, TCK, TDI and TDO wires) for extending the JTAG interface for computers to operate CPUs, DSPs, FPGAs, CPLDs and other devices.

![](images/357ef0303e4c493be45e4431392182cf139fca121bd3c44544cbe1f4669cfd2d.jpg)  
Figure 3 WCH-LinkE high-speed JTAG mode

### 7.2 Module Features

As Host/Master host mode.   
JTAG interface provides TMS wire, TCK wire, TDI wire and TDO wire.   
Support high-speed USB data transfer.   
 Flexible operation of CPU, DSP, FPGA and CPLD devices through computer API cooperation.

### 7.3 Module Switching

The WCH-LinkE-R0-1v3 can be upgraded to high-speed JTAG mode via the WCHLinkEJtagUpdTool tool, download the steps as follows.

① WCH-LinkE-R0-1v3 into IAP mode (long press the IAP button to power up the Link, i.e., connect to the computer through the USB port to power up), at this time the blue LED flashes.   
$\textcircled{2}$ Open WCHLinkEJtagUpdTool tool, execute the download (WCH-LinkE high-speed JTAG upgrade firmware has been automatically added).   
$\textcircled{3}$ Firmware update is complete, at this time the blue LED is always on.

![](images/e357819d2c72d348c917309f2bca83d3afe89ed42344f7dc528a19425bf4ae13.jpg)

#### Notes.

(1) WCHLinkEJtagUpdTool get URL: https://www.wch.cn/downloads/WCHLinkEJtagUpdTool_ZIP.html   
(2) The firmware can be updated offline by WCH-LinkUtility tool, please refer to manual 6.3 WCH-LinkUtility Offline Update for details.   
(3) WCH-LinkE high-speed JTAG offline update firmware is located in the WCHLinkEJtagUpdTool installation path.

WCHLinkEJtagUpdTool 》FirmwareList

名称

① FIRMWARE_USB_JTAG_CH32V305.bin   
② WCH-LinkE-USB-JTAG-APP-IAP.bin

①WCH-LinkE high-speed JTAG upgrade firmware   
$\textcircled{2}$ WCH-LinkE high-speed JTAG offline upgrade firmware

### 7.4 Download Process

$\textcircled{1}$ In WCH-LinkE high-speed JTAG mode, the Bit program file is first downloaded to the FPGA via JTAG, and the Bit file will operate the SPI controller of the FPGA to convert the JTAG data to SPI data for writing to Flash, and this step is to write the BIN file to realize its program curing process.   
$\textcircled{2}$ Here the FPGA is Xilinx xc7a35t. Write the CFG file and use "openocd -f" to call it. Name the CFG file as usb20jtag.cfg and save it to the location of the openocd.exe file.

# Specify WCH-LinkE high-speed JTAG debugger

adapter driver ch347

ch347 vid_pid 0x1a86 0x55dd

# Set TCK clock frequency

adapter speed 10000

# Specify TARGET, loading the JTAG-SPI driver in OpenOCD

source [find cpld/xilinx-xc7.cfg]

source [find cpld/jtagspi.cfg]

# Set IR command of TARGET

set XC7_JSHUTDOWN 0x0d

set XC7_JPROGRAM 0x0b

set XC7_JSTART 0x0c

set XC7_BYPASS 0x3f

# Download process

Init

# First download the Bit file to TARGET

pld load 0 bscan_spi_xc7a35t.bit

reset halt

# Detect Flash information

flash probe 0

# Download Bin file to Flash

flash write_image erase test.bin 0x0 bin

# Effective firmware operation

irscan xc7.tap $XC7_JSHUTDOWN

irscan xc7.tap $XC7_JPROGRAM

runtest 60000

runtest 2000

irscan xc7.tap $XC7_BYPASS

runtest 2000

exit

③ Run the command: openocd.exe -f usb20jtag.cfg in Windows terminal and execute it as follows.

```txt
D:\MounRiver\MounRiver_Studio\toolchain\OpenOCD\bin>openocd.exe -f usb20jtag.cfg   
Open On-Chip Debugger 0.11.0+dev-02415-gfad123a16-dirty (2022-12-13-09:38)   
Licensed under GNU GPL v2   
For bug reports, read http://openocd.org/doc/doxygen/bugs.html   
Info: only one transport option; autoselect 'jtag'   
Info: clock speed 10000 kHz   
Info: JTAG tap: xc7. tap tap/device found: 0x0362d093 (mfg: 0x049 (Xi1inx), part: 0x362d, ver: 0x0)   
Ixc7. proxy1 Target successfully examined.   
Info: JTAG tap: xc7. tap tap/device found: 0x0362d093 (mfg: 0x049 (Xi1inx), part: 0x362d, ver: 0x0)   
Info: Found flash device 'issi is251p128d' (ID 0x18609d)   
Info: sector 0 took 0 ms   
Info: sector 1 took 0 ms   
Info: sector 2 took 0 ms   
Info: sector 3 took 0 ms   
Info: sector 4 took 0 ms   
Info: sector 5 took 0 ms   
Info: sector 6 took 0 ms   
Info: sector 7 took 15 ms   
Info: sector 8 took 0 ms   
Info: sector 9 took 0 ms   
Info: sector 10 took 0 ms   
Info: sector 11 took 0 ms   
Info: sector 12 took 0 ms   
Info: sector 13 took 16 ms   
Info: sector 14 took 0 ms   
Info: sector 15 took 0 ms   
Info: sector 16 took 0 ms   
Info: sector 17 took 0 ms   
Info: sector 18 took 0 ms   
Info: sector 19 took 16 ms   
Info: sector 20 took 0 ms   
Info: sector 21 took 0 ms   
Info: sector 22 took 0 ms   
Info: sector 23 took 0 ms   
Info: sector 24 took 0 ms   
Info: Close the CH347. 
```

④ The download is over and the device is running normally.

#### Notes.

(1) conversion role of the Bit file, with the help of Github open source project: https://github.com/quartiq/bscan_spi_bitstreams   
(2) openocd.exe file location: MounRiver\MounRiver_Studio\toolchain\OpenOCD\bin

## 8 WCH-LinkW Use Instruction

### 8.1 Module Overview

WCH-LinkW is a wired/wireless 2.4G dual imitation real debugger, which can be used for online debugging and downloading of WCH RISC-V architecture MCU and also for online debugging and downloading of ARM chips with SWD/JTAG interface.

### 8.2 Use Methods

#### 8.2.1 Wired Mode

Wired mode only needs 1 WCH-LinkW, connect the row of pins to MCU and USB port to PC for downloading and debugging.

![](images/0eda77f952dd2e54e4d0be24c2bae09f3755cefe2b6769d557e42db6f7e5073e.jpg)

#### 8.2.2 Wireless Mode

Wireless mode requires 2 WCH-LinkW, divided into WCH-LinkW master (connected to computer) and WCH-LinkW slave (connected to MCU.) After WCH-LinkW is successfully enumerated to computer, it will detect whether there is a slave match within 2 seconds to switch mode, if there is a slave match, it will switch to wireless mode, and the green light will be on; otherwise, it will switch to wired mode, and the green light goes off.

Wireless mode download debugging requires the use of two WCH-LinkW, the use of the following steps:

$\textcircled{1}$ The slave and MCU are connected by two wires, and the power is supplied through the U-port of MCU, or the slave and MCU can be powered by charging head or mobile power connected to the USB port of the slave   
$\textcircled{2}$ After the slave is successfully powered up, the host USB port is connected to the computer. After the host device is successfully enumerated and matched to the slave within 2 seconds, the green light of the host and the slave will be lit   
$\textcircled{3}$ After the successful matching of wireless mode host and slave, the host and slave are powered off, then repeat the above steps, if only one is powered off, it can be re-powered for automatic matching without repeating the above process   
$\textcircled{4}$ Download and debug MCU

![](images/8fabc95aefd49ce6d356c33d3d002ef47751aaff1bb09e9725f920929046efec.jpg)

Note: Using Code Flash full erase and power output controllable function requires connecting to the slave USB port via charging head or mobile power to power the slave and MCU.

### 8.3 Wireless Mode Access Address Match

When WCH-LinkW uses wireless mode for downloading and debugging, it is necessary to ensure that the wireless mode access addresses of the host and slave are the same. You can set the wireless mode access address through the WCH-LinkUtility tool. The steps are as follows:

![](images/2bdd41100fda5f2d2fb36bf8b2311341c793db6fcbc658404e99e5754d626b75.jpg)

$\textcircled{1}$ Connect WCH-LinkW wireless mode host and slave to computer respectively, click GET button, check whether the current host and slave access addresses are the same, if they are the same and not 0xE339E339, execute step 4, otherwise execute step 2   
$\textcircled{2}$ Click CREATE button to create a random address   
$\textcircled{3}$ Click SET button to set the access address of host and slave respectively   
$\textcircled{4}$ The slave connects to the MCU to power on first, then the host connects to the computer, and the green light will light up to use the wireless mode normally

#### Notes:

(1) The factory default WCH-LinkW wireless mode access address is 0xE339E339.   
(2) When wireless mode, only one pair of WCH-LinkW wireless access address should be guaranteed to be the same in the same usage environment. If there are more than one pair of devices, you need to use the above steps to set different wireless mode access addresses.

## 9 Typical Problem Statement

<table><tr><td>Error Alert</td><td colspan="2">Solution</td></tr><tr><td>Use Keil software to download</td><td colspan="2">1. Please refer to manual 3.2 Download configuration to complete Keil download configuration.</td></tr><tr><td>Main.c
1 / / File Name : Main.c
3 / Author : WCH
4 / Version : V1.0.0
5 / Date : 2019/10/18
6 / Description : Main program body
7 / / No UUNLK2/ME Device found
8 / Copyright (c) 2021 Nanjing Ouheng Microelectronics Co., Ltd.
9 / attention: This software (modified or not) and binary are used for microcontrollers manufactured by Nanjing Ouheng Microelectronics Co.
10 / / Microcontrollers manufactured by Nanjing Ouheng Microelectronics Co.
11 / / UUNLK2/ME Cortex-M Error X
12 / / Main program body.
13 / / No UUNLK2/ME Device found
14 / / Main program body.
15 / / No UUNLK2/ME Device found
16 / / No UUNLK2/ME Device found
17 / / Main program body.
18 / / No UUNLK2/ME Device found
19 / / No UUNLK2/ME Device found
20 / / No UUNLK2/ME Device found
21 / / No UUNLK2/ME Device found
22 / / No Global Variable +/-
23 / / Global Variable +/-
24 / u6 TaBuf[1024];
25 / u6 Calibration_Val = 0;
26 / / Main program body.
27 / / Main program body.</td><td colspan="2"></td></tr><tr><td>Use Keil software to download</td><td colspan="2">1. The RAM space size of our CH32F20x series chips is 0x2800.</td></tr><tr><td>Main.c
1 / / File Name : Main.c
3 / Author : WCH
4 / Version : V1.0.0
5 / Date : 2021/08/08
6 / Description : Main program body.
7 / / No UUNLK2/ME Device found
8 / Copyright (c) 2021 Nanjing Ouheng Microelectronics Co., Ltd.
9 / attention: This software (modified or not) and binary are used for microcontrollers manufactured by Nanjing Ouheng Microelectronics Co.
10 / / No UUNLK2/ME Device found
11 / / No Global Variable +/-
12 / / No Global Variable +/-
13 / / No Global Variable +/-
14 / / No Global Variable +/-
15 / / No Global Variable +/-
16 / / No Global Variable +/-
17 / / No Global Variable +/-
18 / / No Global Variable +/-
19 / / No Global Variable +/-
20 / / No Global Variable +/-
21 / / No Global Variable +/-
22 / / No Global Variable +/-
23 / / No Global Variable +/-
24 / u6 TaBuf[1024];
25 / u6 Calibration_Val = 0;
26 / / Main program body.
27 / / Main program body.</td><td colspan="2"></td></tr><tr><td>Use MounRiver Studio software to download</td><td colspan="2">1. Check whether the chip's two-wire debug interface is correctly connected to Link.
2. Check whether the Debug function of the chip is turned on (if not, it can be turned on through the ISP tool).
3. Check whether the user program inside the chip is open to sleep function and whether there is an operation of FLASHrelated functions (if open, you can enter BOOT mode and download through two lines).
4. Check whether the two-wire debug interface of the user program inside the chip is multiplexed as a common GPIO port (if multiplexed, you can enter BOOT mode and download through two wires).
Note:
(1) For CH32 series chips, if the download is not successful, you can enter BOOT mode (BOOT0 to VCC, BOOT1 to GND) and download through Link.
(2) For 3 and 4, the problem can be solved by</td></tr><tr><td></td><td colspan="2">WCH-LinkUtility tool to erase all the user area of the chip (refer to Chapter 5of the manual for WCH-LinkUtility download).</td></tr><tr><td>Use the WCH-LinkUtility tool to download</td><td colspan="2">Erase all user areas of the</td></tr><tr><td>Connected WCH-Link List: RISC-V Link [#1] Refresh WCH-LinkRV Get Set</td><td colspan="2">File Target View</td></tr><tr><td>Active WCH-Link Mode: Result Collect: Succ0 | Total0 Clear</td><td colspan="2">Connect WCH-Link Disconnect</td></tr><tr><td>14:08:14:292&gt;&gt; Begin to set chip type... 14:08:14:354&gt;&gt; Failed, the chip type is not matched or status of chip is wrong!</td><td rowspan="2">Query Chip Info Erase Chip F9 Program F10 Verify F11 Reset F12 Query Chip R-Protect Status Enable Chip R-Protect Disable Chip R-Protect Query Flash QE Status Enable Flash QE Clear All Code Flash-By Pin NRST Clear All Code Flash-By Power off</td><td rowspan="2">1. Analysis of the cause, may be the WCH-LinkE-R0-1v3 on the Y1 crystal soldering abnormalities, resulting in the crystal cannot properly start vibration. Therefore, you need to re-solder the Y1 crystal.</td></tr><tr><td>Update firmware using WCHLinkEJtagUpdTool tool After updating the firmware according to manual 7.3 Mode Switching Download Procedure, the blue LED on the WCH-LinkE-R0-1v3 does not light up and the Device Manager cannot recognize the device.</td></tr><tr><td>When using WCH-LinkW wireless mode for downloading and debugging, the green light does not turn on.</td><td colspan="2">1. Please refer to section 8.2.2 of the manual for operation.</td></tr><tr><td>When using WCH-LinkW wireless mode for downloading and debugging, Code Flash full erase and power output controllable functions are not available.</td><td colspan="2">1. To use the above function, you need to connect to the slave USB port through the charging head or mobile power to power the slave and MCU.</td></tr><tr><td>When using WCH-LinkW wireless mode for downloading and debugging, the slave cannot upgrade the firmware online.</td><td colspan="2">1.WCH-LinkW slave online firmware upgrade needs to be done in wired mode.</td></tr></table>

Notes:   
(1) The debugging function is not supported when the user program turns on the sleep function.   
(2) If you exit abnormally when using the debug function, it is recommended to re-plug the Link.   
(3) When using the download and debug functions of CH32F103/CH32F203/CH32V103/CH32V203/ CH32V307/CH32L103/CH32V317/CH32V205_203CC, BOOT0 is grounded.   
(4) When using the debug function of CH569, the user code must be smaller than the configured ROM space, as shown in Table 2-2 of CH569SD1.   
(5) When using the debug function of CH32 series chip, please make sure the chip is in the read protection off state.   
(6) Typical WCH-Link FAQs can be found at: https://www.wch.cn/bbs/thread-100647-1.html

## 10 Driver Installation

### 10.1 WCH-Link Driver

(1) The WCH-Link driver will be installed automatically when MounRiver Studio is installed, and the device manager will be shown in the table below after successful installation. If the driver installation fails, please open the LinkDrv folder under the installation path of MounRiver Studio and manually install SETUP.EXE under the WCHLink folder.   
(2) To install the WCH-LinkUtility tool, you need to manually install the WCH-Link driver. Please open the Drv_Link folder in the WCH-LinkUtility tool file directory and manually install WCHLinkDrv_WHQL_S.exe.

<table><tr><td colspan="2">Device manager</td><td colspan="2">Drive path</td></tr><tr><td rowspan="2">Device Manager
File Action View Help</td><td rowspan="2">Device Manager
File Action View Help</td><td colspan="2">MounRiver &gt; MounRiver_Studio &gt; LinkDrv &gt;</td></tr><tr><td colspan="2">名称
WCHLink -WCHLink Drive
WCHLinkSER -CDC Drive</td></tr><tr><td rowspan="2">Storage controllers
System devices
UCM Client
Universal Serial Bus controllers
External Interface
WCH-LinkRV</td><td rowspan="2">Storage controllers
System devices
UCM Client
Universal Serial Bus controllers
External Interface
WCH CMSIS-DAP</td><td colspan="2">WCH-LinkUtility &gt; Drv_Link</td></tr><tr><td colspan="2">名称
CH372DRV_S.exe -CH372 Drive
WCHLinkDrv_WHQL_S.exe -WCHLink Drive</td></tr></table>

### 10.2 WCH-LinkE High-speed JTAG Driver

WCH-LinkE-R0-1v3 is upgraded to high-speed JTAG mode, you need to manually install the WCH-LinkE high-speed JTAG driver to use it properly. Please open the Drv folder under the installation path of WCHLinkEJtagUpdTool and install CH341PAR.EXE manually.

<table><tr><td>Device manager</td><td>Drive path</td></tr><tr><td>Device Manager
File Action View Help</td><td rowspan="2">WCHLinkEJtagUpdTool &gt; Drv
名称
CH341PAR.EXE</td></tr><tr><td>Storage controllers
System devices
UCM Client
Universal Serial Bus controllers
External Interface
USB HighSpeed-JTAG/12C... CH347</td></tr></table>

### 10.3 CDC Driver

CDC device installation problems under WIN7.

$\textcircled{1}$ If the serial port driver is successfully installed, the following steps are not required.   
$\textcircled{2}$ Confirm that the usbser.sys file is present in path B. If it is missing, copy it from path A to path B.   
$\textcircled{3}$ Reinstall the CDC driver. (See the above table for the driver path, please install the CDC driver in the corresponding mode)

![](images/c72ce26e7c10c55d02f5dcb44d0d11375943685d9cba4fe8058466d1e406d5ab.jpg)

Note: If the above steps do not solve the problem, please refer to the link below

![](images/f982961374e195e61b9683dd1c3b56fdd7a0583d331d3f4d32136e6aa864458b.jpg)

Reference: http://www.wch.cn/downloads/InstallNoteOn64BitWIN7_ZH_PDF.html
# WCHNET IAP 升级方案使用教程

## 1. 内存分配

见 ETH_IAP 工程中“IAP_Task.h”文件中的配置参数。

## 2. 操作说明

### 2.1 硬件连接

$\textcircled{1}$ 将 WCH-Link 的电源线、下载线、串口线和评估板（以 CH32V307 为例）连接；

![](images/652845fd55ca25bf7f1fbe66b4b8d50d194f391f7eec5cee0be9061a4fa9072b.jpg)

$\textcircled{2}$ 将 PA0 连接 KEY；

![](images/13f36013c87e51f5048fbdc7b7f584513d295e2641e3c1f61eb23fb0ba18e84a.jpg)

$\textcircled{3}$ 将 WCH-Link 连接 PC，评估板通过网线连接 PC。

### 2.2 配置网络

$\textcircled{1}$ 打开 TCP 调试工具“TcpIpDebug”；

![](images/0c6000b8544d2486d51bb4ea01f4e828655ee5d7a809df6555e78b2089c15ad9.jpg)

$\textcircled{2}$ 新建服务器，确认本机 IP 和本机端口，启动服务器；

![](images/cd3bd9db1707ea8881e0db4ed91c0901c8764673a522f1e3a8f2f92ddb0e97d5.jpg)

$\textcircled{3}$ 打开ETH_IAP工程，选中“main.c”文件，修改IP 地址、网关、子网掩码、服务器IP。

main.c   
10   
19 ETH IAP例程，演示通过TCP数据传输，进行IAP。   
20 \*/   
21   
22 u8 MACAddr[6]; //MAC address   
23 u8 IPAddr[4] $=$ {192,168,1,10}; //IP address   
24 u8 GWIPAddr[4] $=$ {192,168,1,1}; //Gateway IP address   
25 u8 IPMask[4] $=$ {255,255,255,0}; // subnet mask   
26 u8 DESIP[4] $=$ {192,168,1,100}; //destination IP addr   
27 u16 desport $=$ 1000; //destination port   
28 u16 srcport $=$ 1000; //source port

### 2.3 下载 IAP 程序

$\textcircled{1}$ 打开串口调试助手，选中串口号，波特率 115200，打开串口。  
$\textcircled{2}$ 编译工程并通过“WCH-Link”下载 IAP 程序；

![](images/fc0e3aa3ca997452e51c7e999a9ca0e5491afad39ca1c7cda600593ad3def0f8.jpg)

$\textcircled{3}$ 串口调试助手打印信息如下所示，由于尚未下载 APP 程序，所以提示 HardFault_Handler，表明 IAP 程序下载完成。

![](images/1af2334cc2f5f51f77968b1d2b50e4b1f3f1cc0debf40f72a97460dcd839a038.jpg)

### 2.4 生成 APP 程序 Bin 文件

$\textcircled{1}$ 打开用于生成升级文件的工程，选中“Link.ld”，修改 FLASH 和 RAM 如下所示；

![](images/4075765eab15f494448623df80706c94b6aef1619da4f8b46bef566db40ba84d.jpg)

$\textcircled{2}$ 点击工具栏的属性图标，进入属性配置界面。再点击 ${ } ^ { 6 6 } \complement / \complement + +$ 构建->设置->GNU RISC-V CrossCreate Flash Image->General->Output file format”选择 Raw binary，应用并关闭；

![](images/6397a00116fc654d40156a88e560a49d1a677116b116913a032104143c2b7328.jpg)

![](images/0a22879cea42b99f60cf6e19f826a99eaf1e4790544bf8bb149119f6abc620b8.jpg)

$\textcircled{3}$ 编译工程；

![](images/89acb107ff58dd10810bdaca599d93045fb8fcae449d669f1c82708b3a31d58d.jpg)

④在 obj 文件夹中找到刚刚编译生成的 bin 文件。

### 2.5 添加 bin 文件升级信息

将生成的 bin 文件通过 VerifyBinTool_WCHNET.exe 工具添加升级信息，将新生成的 bin 文件作为最终的升级文件。

![](images/e24cb60ae628503a5966796062be58b7b27b9dfffb4f954f510975260663938a.jpg)

### 2.6 网络 IAP 升级

$\textcircled{1}$ 按住评估板 KEY 同时复位单片机，串口调试助手打印 TCP Connect Success，同时 TcpIpDebug连接成功；

![](images/659addca096300ddffe9b7c272de802752222173469711e7c0c2599e2cbd9c26.jpg)

$\textcircled{2}$ 勾选发送文件，选择需要发送的 bin 文件，点击发送；

![](images/91b076114c1b7b4a9345bc5d8181f8d909e5a68448ee0fb4144cb0dfd8f287af.jpg)

$\textcircled{3}$ 串口助手打印如图所示则表明升级成功，升级成功后会自动运行 APP 程序。

![](images/4f71c368178250002502b5118b63f7b386902f105c24b13d2cdd3b6cc67ef76c.jpg)

④如果升级失败，应该重点检查一下 ld文件的配置是否正确，是否添加过升级信息等。

注：实际的升级空间大小应该在“IAP_Task.h”中的配置值上减少 256B，该 256B 用于存储升级标志等信息。
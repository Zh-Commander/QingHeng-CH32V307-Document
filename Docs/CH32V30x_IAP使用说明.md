# CH32V30x_IAP 使用说明

版本：V1.1

https://wch.cn

1． 将 IAP 下载到 CH32V30x

2． App 工程

```txt
1 ENTRY(_start)  
2  
3_stack_size = 2048;  
4  
5 PROVIDE(_stack_size = _stack_size);  
6  
7  
8 MEMORY  
9{  
10 FLASH (rx) : ORIGIN = 0x00005000, LENGTH = 228K  
11 RAM (xrw) : ORIGIN = 0x20000000, LENGTH = 32K  
12}  
13  
14  
15 SECTIONS  
16{  
17  
18 .init : 
```

修改 LD 起始地址从 0x00005000 开始，可根据需求修改。

3． 生成.hex 或者.bin 文件。软件版本 (V1.50)同时支持.hex 和.bin 下载文件方式。

![](images/675543a05ddf0c197a5d3f4c1cc3473c369c5b8c7f560e741a1cf30c9bbb415a.jpg)

4．用 IAP 下载工具下载。当前芯片支持两种跳转模式以及两种下载模式（USB\UART）

跳转方式 1：下载成功后自动跳转到 APP 运行，并且可以在 APP 程序中通过指令跳转到 IAP，等待再下载。

跳转方式 2：GPIO 引脚（PA0 默认上拉输入），下载 APP 后复位，检测到低电平后跳转到 APP。APP中同样可以通过指令跳转到 IAP 等待再下载

USB 下载：

![](images/6258a846fc47cb25f7d3c8469fad728a4bbb43ecd786ba81ee117450a9f39aa9.jpg)

在 APP 中运行时，可以通过“进入 IAP”指令跳转至 IAP 等待再下载。

![](images/f2865d6e6dbd3046486ce8befeb4dadf5c2a2f26353b591ae387a61e35da3636.jpg)

UART 下载：

选择对应串口，设置波特率 460800（可根据需求修改）

![](images/d7c7cc5592b567443a75f50f67742b6abf6af202651ac339d1d532d0e583c8e4.jpg)

在 APP 中运行时，可以通过“进入 IAP”指令跳转至 IAP 等待再下载。

![](images/19204fd3e0e5a1f1d8add5e0eee95d963e157121c8c6e3027301e00d35652c55.jpg)
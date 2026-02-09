# 说明

${ \tt C H 3 2 F 2 x }$ 系列产品是基于 ARM○RCortexTM-M3 内核设计的通用微控制器，与大部分 ARM 工具和软件兼容。提供了丰富的通讯接口和控制单元，适用于大部分控制、连接、综合等嵌入式领域。

${ \mathsf { C H 3 2 V 2 x } }$ 和 ${ \mathsf { C H } } 3 2 { \mathsf { V } } 3 \times$ 系列是基于32位 RISC-V指令集及架构设计的工业级通用微控制器。依据产品性能和资源差异搭配了不同的内核型号。 $\mathtt { C H 3 2 V 2 0 3 x }$ 采用青稞V4B内核，支持硬件中断堆栈，提升中断响应效率； $\mathtt { C H 3 2 V 2 0 8 x }$ 采用青稞V4C内核，进一步加快硬件除法运算速度，并增加了内存保护功能； $\mathtt { C H 3 2 V 3 0 3 \times } / 3 0 5 \times / 3 0 7 \times$ 采用青稞 V4F 内核，进一步支持了硬件浮点运算。该系列产品挂载了丰富的外设接口和功能模块。其内部组织架构满足低成本低功耗嵌入式应用场景。

本手册针对用户的应用开发，提供了 ${ \tt C H 3 2 F 2 x }$ 系列、CH32V2x 系列和 CH32V3x 系列产品的详细使用信息，适用于系列中不同存储器容量、功能资源、封装的产品，若有差异将在对应的功能章节做特殊说明。

有关此系列产品的器件特性请参考以下数据手册。

CH32F20x_D6：《CH32F203DS0》

CH32F20x_D8、CH32F20x_D8C：《CH32F207DS0》。

CH32F20x_D8W：《CH32F208DS0》。

CH32V20x_D6、CH32V20x_D8：《CH32V203DS0》。

CH32V20x_D8W：《CH32V208DS0》。

CH32V30x_D8、CH32V30x_D8C：《CH32V307DS0》。

有关 ARM○R CortexTM-M3 内核的相关信息，请参考《ARM® Cortex®-M3 Processor Technical ReferenceManual Revision r2p1》，可在 ARM 公司网站下载。

有关 RISC-V 内核的相关信息，可参考 QingKeV4 微处理手册：《QingKeV4_Processor_Manual》。

CH32F2x 系列产品概览  

<table><tr><td colspan="2">中小容量通用型(F203)</td><td colspan="2">大容量通用型(F203)</td><td>连接型(F205)</td><td>互联型(F207)</td><td>无线型(F208)</td></tr><tr><td>32K闪存</td><td>64K闪存</td><td>128K闪存</td><td>256K闪存</td><td>128K闪存</td><td>256K闪存</td><td>128K闪存</td></tr><tr><td>10K SRAM</td><td>20K SRAM</td><td>32K SRAM</td><td>64K SRAM</td><td>32K SRAM</td><td>64K SRAM</td><td>64K SRAM</td></tr><tr><td>2*ADC(TKey)</td><td>2*ADC(TKey)</td><td>2*ADC(TKey)</td><td>2*ADC(TKey)</td><td>2*ADC(TKey)</td><td>2*ADC(TKey)</td><td>ADC(TKey)</td></tr><tr><td>ADTM</td><td>ADTM</td><td>2*DAC</td><td>2*DAC</td><td>2*DAC</td><td>2*DAC</td><td>ADTM</td></tr><tr><td>2*GPTM</td><td>3*GPTM</td><td>ADTM</td><td>4*ADTM</td><td>4*ADTM</td><td>4*ADTM</td><td>3*GPTM</td></tr><tr><td>2*USART</td><td>4*U(S)ART</td><td>3*GPTM</td><td>4*GPTM</td><td>4*GPTM</td><td>4*GPTM</td><td>GPTM(32)</td></tr><tr><td>SPI</td><td>2*SPI</td><td>3*USART</td><td>2*BCTM</td><td>2*BCTM</td><td>2*BCTM</td><td>4*U(S)ART</td></tr><tr><td>12C</td><td>2*12C</td><td>2*SPI</td><td>8*U(S)ART</td><td>5*U(S)ART</td><td>8*U(S)ART</td><td>2*SPI</td></tr><tr><td>USBD</td><td>USBD</td><td>2*12C</td><td>3*SPI (2*12S)</td><td>3*SPI (2*12S)</td><td>3*SPI (2*12S)</td><td>2*12C</td></tr><tr><td>USBFS</td><td>USBFS</td><td>USBD</td><td>2*12C</td><td>2*12C</td><td>2*12C</td><td>USBD</td></tr><tr><td>CAN</td><td>CAN</td><td>CAN</td><td>USBD</td><td>OTG_FS</td><td>OTG_FS</td><td>USBFS</td></tr><tr><td>RTC</td><td>RTC</td><td>RTC</td><td>CAN</td><td>USBHS (+PHY)</td><td>USBHS (+PHY)</td><td>CAN</td></tr><tr><td>2*WDG</td><td>2*WDG</td><td>2*WDG</td><td>RTC</td><td>2*CAN</td><td>2*CAN</td><td>RTC</td></tr><tr><td>2*OPA</td><td>2*OPA</td><td></td><td>2*WDG</td><td>RTC</td><td>RTC</td><td>2*WDG</td></tr><tr><td></td><td></td><td></td><td>4*OPA</td><td>2*WDG</td><td>2*WDG</td><td>2*OPA</td></tr><tr><td></td><td></td><td></td><td>RNG</td><td>4*OPA</td><td>4*OPA</td><td>ETH-</td></tr><tr><td></td><td></td><td></td><td>SDIO</td><td>RNG</td><td>RNG</td><td>10M (+PHY)</td></tr><tr><td></td><td></td><td></td><td>FSMC</td><td>SDIO</td><td>SDIO</td><td>BLE5.3</td></tr><tr><td></td><td></td><td></td><td></td><td></td><td>FSMC</td><td></td></tr><tr><td></td><td></td><td></td><td></td><td></td><td>DVP</td><td></td></tr><tr><td></td><td></td><td></td><td></td><td></td><td>ETH-1000MAC</td><td></td></tr><tr><td></td><td></td><td></td><td></td><td></td><td>10M-PHY</td><td></td></tr></table>

注：同一类产品的某些外设数量或功能可能受封装限制，选择时请确认产品封装。

CH32V2x 和 CH32V3x 系列产品概览  

<table><tr><td colspan="2">中小容量通用型(V203)</td><td colspan="2">大容量通用型(V303)</td><td>连接型(V305)</td><td>互联型(V307)</td><td>无线型(V208)</td></tr><tr><td colspan="2">青棵V4B内核</td><td colspan="4">青棵V4F内核</td><td>青棵V4C内核</td></tr><tr><td>32K闪存</td><td>64K闪存</td><td>128K闪存</td><td>256K闪存</td><td>128K闪存</td><td>256K闪存</td><td>128K闪存</td></tr><tr><td>10K SRAM</td><td>20K SRAM</td><td>32K SRAM</td><td>64K SRAM</td><td>32K SRAM</td><td>64K SRAM</td><td>64K SRAM</td></tr><tr><td>2*ADC(TKey)ADTM3*GPTM2*USARTSPI12CUSBDUSBFSCANRTC2*WDG2*OPA</td><td>2*ADC(TKey)ADTM3*GPTM4*U(S)ART2*SPI2*12CUSBDUSBFSCANRTC2*WDG2*OPA</td><td>2*ADC(TKey)2*DACADTM3*GPTM3*USART2*SPI2*12CUSBFSCANRTC2*WDG4*OPA</td><td>2*ADC(TKey)2*DAC4*ADTM4*GPTM2*BCTM8*U(S)ART3*SPI(2*12S)2*12CUSBFSCANRTC2*WDG4*OPARNGSDIOFSMC</td><td>2*ADC(TKey)2*DAC4*ADTM4*GPTM2*BCTM5*U(S)ART3*SPI(2*12S)2*12COTG_FSUSBHS(+PHY)2*CANRTC2*WDG4*OPARNGSDIO</td><td>2*ADC(TKey)2*DAC4*ADTM4*GPTM2*BCTM8*U(S)ART3*SPI(2*12S)2*12COTG_FSUSBHS(+PHY)2*CANRTC2*WDG4*OPARNGSDIOFSMC</td><td>ADC(TKey)ADTM3*GPTMGPTM(32)4*U(S)ART2*SPI2*12CUSBDSUBFSCANRTC2*WDG4*OPAR2*OPAETH-10M(+PHY)BLE5.3</td></tr></table>

注：同一类产品的某些外设数量或功能可能受封装限制，选择时请确认产品封装。

RISC-V 内核版本对比概览  

<table><tr><td>特点
内核
版本</td><td>指令集</td><td>硬件
堆栈
级数</td><td>中断
嵌套
级数</td><td>快速
中断
通道数</td><td>整数
除法
周期</td><td>向量表模式</td><td>扩展
指令</td><td>内存
保护</td></tr><tr><td>青棵V4B</td><td>IMAC</td><td>2</td><td>2</td><td>4</td><td>9</td><td>地址或指令</td><td>支持</td><td>无</td></tr><tr><td>青棵V4C</td><td>IMAC</td><td>2</td><td>2</td><td>4</td><td>5</td><td>地址或指令</td><td>支持</td><td>标准</td></tr><tr><td>青棵V4F</td><td>IMAFC</td><td>3</td><td>8</td><td>4</td><td>5</td><td>地址或指令</td><td>支持</td><td>标准</td></tr></table>

寄存器中位属性缩写描述：  

<table><tr><td>寄存器位属性</td><td>属性描述</td></tr><tr><td>RF</td><td>只读属性,读出固定值。</td></tr><tr><td>RO</td><td>只读属性,由硬件改变。</td></tr><tr><td>RZ</td><td>只读属性,读操作后自动位清0</td></tr><tr><td>WO</td><td>只写属性(不可读,读值不确定)</td></tr><tr><td>WA</td><td>只写属性,安全模式下可写入。</td></tr><tr><td>WZ</td><td>只写属性,写操作后自动位清0</td></tr><tr><td>RW</td><td>可读,可写。</td></tr><tr><td>RWA</td><td>可读,安全模式下可写入。</td></tr><tr><td>RW1</td><td>可读,写1有效,写0无效。</td></tr><tr><td>RW0</td><td>可读,写0有效,写1无效。</td></tr><tr><td>RW1T</td><td>可读,写0无效,写1翻转。</td></tr><tr><td>RW1Z</td><td>可读,写1清除此位,写0无效。</td></tr><tr><td>SC</td><td>自动清除。</td></tr></table>

CH32 系列 MCU 分类描述：  

<table><tr><td>分类缩写</td><td>FLASH 大小</td><td>产品分类</td></tr><tr><td>D6</td><td>32KB 或 64KB</td><td>中小容量通用型</td></tr><tr><td>D8</td><td>128KB 或 256KB</td><td>大容量通用型</td></tr><tr><td>D8C</td><td>128KB 或 256KB</td><td>连接型或互联型</td></tr><tr><td>D8W</td><td>128KB 或 256KB</td><td>无线型</td></tr></table>

注：D(Density）、6(2^6)、8(2^8)  
具体分类缩写：  
CH32F20x_D6 包括：CH32F203K8、CH32F203C6、CH32F203C8；  
CH32F20x_D8 包括：CH32F203CB、CH32F203RC、CH32F203VC、CH32F203RB；  
CH32F20x_D8C 包括：CH32F205RB、CH32F207VC；  
CH32F20x_D8W 包括：CH32F208RB、CH32F208WB；  
CH32V20x_D6 包括：CH32V203F6、CH32V203G6、CH32V203K6、CH32V203F8、CH32V203G8、CH32V203K8、  
CH32V203C6、CH32V203C8；  
CH32V20x_D8 包括：CH32V203RB；  
CH32V20x_D8W 包括：CH32V208GB、CH32V208CB、CH32V208RB、CH32V208WB；  
CH32V30x_D8 包括：CH32V303CB、CH32V303RB、CH32V303RC、CH32V303VC；  
CH32V30x_D8C 包括：CH32V305FB 、CH32V305RB、CH32V307RC 、CH32V307WC 、CH32V307VC；

# 第 1 章 存储器和总线架构

## 1.1 总线架构

CH32F2x 系列是基于 ARM○R CortexTM-M3 内核设计的微控制器，其构架中的内核、仲裁单元、DMA 模块和 SRAM 存储等部分通过多组总线实现交互。

CH32V2x、CH32V3x 系列产品是基于 RISC-V 指令集设计的通用微控制器，其架构中的内核、仲裁单元、DMA模块、SRAM存储等部分通过多组总线实现交互。

![](images/510019e907c744aa299ed5403502bfdc8ada560cba201e7dbbdaa974a959fefb.jpg)  
图 1-1 CH32F203（中小容量通用型）系统框图

![](images/fad4b6d8c7576e8e985071755c9194ed52fbf2a9ff2e49c7108c1b13a73aabcd.jpg)  
图 1-2 CH32F2x（连接/互联/大容量通用型）系统框图

![](images/4551c8137c0a13c443c683353d60a005067d9f0220ee542d6264d12284b1d736.jpg)  
图 1-3 CH32F208（无线型）系统框图

![](images/b3044530fc384b8223fa9b4fc7a0f28728158cdc33cc8d9f267f4fab8bcfeeb9.jpg)  
图 1-4 CH32V203 系统框图

![](images/7b1869bacaf860244394f1409a98039183b21c3457020dda84204e2903427bed.jpg)  
图 1-5 CH32V208 系统框图

![](images/e3bf224d4d8a0e0bd161b8e1db98eaa46415fcdfc8e8393a0a7c0d5940560bff.jpg)  
图 1-6 CH32V3x 系统框图

系统中设有：Flash 访问预取机制用以加快代码执行速度；通用 DMA 控制器用以减轻 CPU 负担、提高效率；时钟树分级管理用以降低了外设总的运行功耗，同时还兼有数据保护机制，时钟安全系统保护机制等措施来增加系统稳定性。

$\bullet$ 指令总线(I-Code)将内核和 FLASH 指令接口相连，预取指在此总线上完成。  
$\bullet$ 数据总线(D-Code)将内核和 FLASH 数据接口相连，用于常量加载和调试。  
$\bullet$ 系统总线将内核和总线矩阵相连，用于协调内核、DMA、SRAM 和外设的访问。  
$\bullet$ DMA总线负责DMA的AHB主控接口与总线矩阵相连，该总线访问对象是 FLASH数据、SRAM 和外设。

总线矩阵负责的是系统总线、数据总线、DMA 总线、SRAM 和 AHB/APB 桥之间的访问协调。  
$\bullet$ AHB/APB 桥，为AHB总线和两个APB总线提供同步连接。不同的外设挂在不同的 APB总线下，可以按实际需求配置不同总线时钟，优化性能。

## 1.2 存储器映像

CH32F2x、CH32V2x 和 ${ \mathsf { C H } } 3 2 { \mathsf { V } } 3 \times$ 系列产品都包含了程序存储器、数据存储器、内核寄存器和外设寄存器等等，它们都在一个4GB的线性空间寻址。

系统存储以小端格式存放数据，即低字节存放在低地址，高字节存放在高地址。

![](images/8637a2ef3aeb2424eeabbea5ec7fc9e8111a83c8bb1045d97c14eda426e45978.jpg)  
图 1-7 CH32F203（中小容量通用型）存储映像

![](images/f3bf9c5bb4bd23fbb008a563c0ed9c3c78a096ffaa87cf860ce60a0863feb0ab.jpg)  
图 1-8 CH32F2x（连接/互联/大容量通用型）存储映像

![](images/a2f1bd053497a70783c75b2aacccaa30c926ee543564ac390acd4e7bcfb44f97.jpg)  
图 1-9 CH32F208（无线型）存储映像

![](images/3229399a9b3b3a1e587909dec237d53900759c4bfa9a701e0a2e6a224c7c210a.jpg)  
图 1-10 CH32V203 存储映像

![](images/9eb88719945b1f82da71ffcdf76eb0095a6cce925da373db43b0528f39b176d7.jpg)  
图 1-11 CH32V208 存储映像

![](images/42846b5d482f2dc0bb8b5c7df334776e6cf41218fc28521f1056075792e99a66.jpg)  
图 1-12 CH32V3x 存储映像

### 1.2.1 位段访问

位操作就是单独读写一个比特位的操作。CH32F2x产品中通过映射的处理方式提供了对外设寄存器和 SRAM区内容的位操作读写。具体方法：

1）通过对映射地址区域32位的数据进行读操作，读出的值为 0 或非 0，获取目标位域值是 0或 1；  
2）通过对映射地址区域 32 位的数据进行写操作，写入 0 或 1，修改目标位域值为 0 或 1。

地址映射：

目标位域：基地址(BEaddr) $+$ 偏移地址(Ofaddr) $^ +$ 位号(BitN)

映射地址：Mapaddr

$$
\text {M a p a d d r} = \text {B E a d d r} + 0 \times 2 0 0 0 0 0 0 + (0 \text {f a d d r} \times 3 2) + (\text {B i t N} \times 4)
$$

举例 1：对 SRAM 区的 0x20000100 地址字节中的 bit3 目标位域进行操作：

$$
\text {M a p a d d r} = 0 \times 2 0 0 0 0 0 0 0 + 0 \times 2 0 0 0 0 0 0 + (0 \times 1 0 0 * 3 2) + (3 * 4) = 0 \times 2 2 0 0 2 0 0 0
$$

则读取 $0 { \times } 2 2 0 0 2 0 0 0$ 地址的 4 字节数据内容可知 $0 { \times } 2 0 0 0 0 1 0 0$ 地址字节中的 bit3 是 0 还是 1；对$0 { \times } 2 2 0 0 2 0 0 0$ 地址执行写0或1操作，可以修改 $0 { \times } 2 0 0 0 0 1 0 0$ 地址字节中的 bit3 为 0还是1。

举例 2：对外设区域的 $0 { \times } 4 0 0 2 1 0 0 0$ 地址中的 bit24 进行操作：

$$
\text {M a p a d d r} = 0 \times 2 0 0 0 0 0 0 0 + 0 \times 2 0 0 0 0 0 0 + (0 \times 2 1 0 0 0 * 3 2) + (2 4 * 4) = 0 \times 2 2 4 2 0 0 6 0
$$

则读取 $0 { \times } 2 2 4 2 0 0 6 0$ 地址的4字节数据内容可知 $0 { \times } 4 0 0 2 1 0 0 0$ 外设地址中的 bit24是0 还是 1；对$0 { \times } 2 2 4 2 0 0 6 0$ 地址执行写0或1操作，可以修改 $0 { \times } 4 0 0 2 1 0 0 0$ 外设地址中的 bit24为 0还是1。

注：CH32V2x和CH32V3x产品不支持位段映射访问方式。

### 1.2.2 存储器分配

内置最大 128K 字节的 SRAM，起始地址 0x20000000，支持字节、半字(2 字节)、全字(4 字节)访问。

内置最大 480K 字节的程序闪存存储区(CodeFlash)，用于存储用户应用程序。

内置 28K 字节的系统存储器(bootloader)，用于存储系统引导程序（厂家固化自举加载程序）。内置 128 字节空间用于厂商配置字存储，出厂前固化，用户不可修改。

内置 128 字节空间用于用户选择字存储。

注；储存器分配各型号不同，具体参考芯片对应数据手册。

## 1.3 启动配置

系统可以通过 BOOT0 和 BOOT1 引脚来选择三种不同的启动模式。

表 1-1 启动模式  

<table><tr><td>B00T0</td><td>B00T1</td><td>启动模式</td></tr><tr><td>0</td><td>X</td><td>从程序闪存存储器启动</td></tr><tr><td>1</td><td>0</td><td>从系统存储器启动</td></tr><tr><td>1</td><td>1</td><td>从内部 SRAM 启动</td></tr></table>

用户通过设置 BOOT 引脚的状态值来选择复位后的启动模式。系统复位后或者电源复位都会导致BOOT引脚的值被重新锁存。

启动模式不同，程序闪存存储器、系统存储器和内部SRAM 有着不同的访问方式：

l 从程序闪存存储器启动时，程序闪存存储器地址被映射到 $0 \times 0 0 0 0 0 0 0 0$ 地址区域，同时也能够在原地址区域 $0 \times 0 8 0 0 0 0 0 0$ 访问。  
$\bullet$ 从系统存储器启动时，系统存储器地址被映射到 $0 \times 0 0 0 0 0 0 0 0$ 地址区域，同时也能够在原地址区域 0x1FFFF000 访问。  
$\bullet$ 从内部 SRAM 启动，只能够从 $0 { \times } 2 0 0 0 0 0 0 0$ 地址区域访问。对于 CH32F2x 系列产品，在此区域启动时，需要通过 NVIC 控制器设置向量表偏移寄存器，重映射向量表到 SRAM 中。对于 CH32V2x 和 ${ \mathsf { C H } } 3 2 { \mathsf { V } } 3 \times$ 系列产品无需此动作。

# 第 2 章 电源控制（PWR）

本章模块描述适用于CH32F2x、CH32V2x和CH32V3x微控制器全系列产品。

## 2.1 概述

系统工作电压 $\mathsf { V } _ { \mathsf { D } \mathsf { D } }$ 范围为 $2 . 4 { \sim } 3 . 6 \mathsf { V }$ ，内置电压调节器提供内核所需的 1.5V 电源。当主电源 $\mathsf { V } _ { \mathsf { D } \mathsf { D } }$ 掉电后，电池等后备电源可通过 $V _ { \mathsf { B A T } }$ 引脚为实时时钟(RTC)和后备寄存器提供电源，如果无需后备电源，建议将 $\mathsf { V } _ { \mathsf { D } \mathsf { D } }$ 直接连接到 $V _ { \mathsf { B A T } }$ 引脚上。

$V _ { \mathsf { D D A } }$ 和 $\mathsf { V } _ { \mathsf { S S A } }$ 引脚专门为系统中模拟相关电路供电，包括 ADC、DAC、温度传感器等。 $V _ { R E F + }$ 和 $V _ { R E F }$ -作为一些模拟电路的参考点，在芯片内部等于 $V _ { \tt D D A }$ 及 $\mathsf { V } _ { \mathsf { S S A } }$ 。实际应用中 $V _ { \mathsf { D D A } }$ 和 $\mathsf { V } _ { \mathsf { S S A } }$ 必须连接到 $\mathsf { V } _ { \mathsf { D } \mathsf { D } }$ 和 $\mathsf { V } _ { \mathbb S }$ 端。

![](images/7965a92aa198089b99f6f4e835b717d9f9217c33a27807937cbe0977eb697f68.jpg)  
图 2-1 电源结构框图

在主电源 $\mathsf { V } _ { \mathsf { D } \mathsf { D } }$ 掉电后，模拟开关切换至 $V _ { \mathsf { B A T } }$ ，后备区域由 $V _ { \mathsf { B A T } }$ 引脚供电，此时 $\mathsf { P C } 1 3 \sim 1 5$ 无法作为GPIO，仅可使用如下功能：

PC13 可以作为 TAMPER 引脚、RTC 闹钟或秒输出。  
PC14 和 PC15 只能用作 LSE 引脚。

当主电源 $\mathsf { V } _ { \mathsf { D } \mathsf { D } }$ 上电稳定后，系统自动切换后备区域由 $\mathsf { V } _ { \mathsf { D } \mathsf { D } }$ 供电， $\mathsf { P C } 1 3 \sim 1 5$ 可以用作 GPIO 功能。

当 $\mathsf { P C } 1 3 \sim 1 5$ 引脚作为 GPIO 输出时，速度必须限制在 2MHz 以下，最大负载电容为 30pF，并且禁止用在持续输出和吸入电流的场合，比如 LED 驱动。

注：在主电源 $V _ { D D }$ 恢复供电过程中，内部 $V _ { B A T }$ 电源仍然通过对应的 $V _ { B A T }$ 引脚连在外部备用电源上，若 $V _ { D D }$ 在小于复位滞后时间tRSTTEMPo内就达到稳定，并且高于 $V _ { B A T }$ 的值0.6V以 $\perp$ ，则有可能存在较短瞬间，电流通过 $V _ { D D }$ 与 $V _ { B A T }$ 之间的二极管灌入 $V _ { B A T }$ ，进而通过 $V _ { B A T }$ 引脚注入电池等后备电源，如果后备电源无法承受这样瞬时注入电流，建议在后备电源和 $V _ { B A T }$ 引脚之间加一只正向导通低压降二极管。

## 2.2 电源管理

### 2.2.1 上电复位和掉电复位

系统内部集成了上电复位POR和掉电复位PDR电路，当芯片供电电压 $\mathsf { V } _ { \mathsf { D } \mathsf { D } }$ 和 $V _ { \mathsf { D D A } }$ 低于对应门限电压

时，系统被相关电路复位，无需外置额外的复位电路。上电门限电压 $V _ { P O R }$ 和掉电门限电压 $V _ { P D R }$ 的参数请参考对应的数据手册。

![](images/8badda17f71fe8cea24ac32314527af9e2d5507ee93eeb9a5d81ffef02af9c7a.jpg)  
图 2-2 POR 和 PDR 的工作示意图

### 2.2.2 可编程电压监视器

可编程电压监视器 PVD，主要被用于监控系统主电源的变化，与电源控制寄存器 PWR_CTLR 的PLS[2:0]所设置的门槛电压相比较，配合外部中断寄存器（EXTI）设置，可产生相关中断，以便及时通知系统进行数据保存等掉电前操作。

具体配置如下：

1）设置PWR_CTLR寄存器的PLS[2:0]域，选择要监控电压阈值。  
2）可选的中断处理。PVD功能内部连接EXTI模块的第16线的上升/下降边沿触发设置，开启此中断（配置EXTI），当 $\mathsf { V } _ { \mathsf { D } \mathsf { D } }$ 下降到PVD阀值以下或上升到PVD 阀值之上时就会产生PVD中断。  
3） 设置 PWR_CTLR 寄存器的 PVDE 位来开启 PVD 功能。  
4） 读取 PWR_CSR 状态寄存器的 PVD0 位可获取当前系统主电源与 PLS[2:0]设置阈值关系，执行相应软处理。

![](images/ba13c29584b6d9468beb73e2361d613f8eff21baa6008b24e9fde5e60225f4d2.jpg)  
图 2-3 PVD 的工作示意图

## 2.3 低功耗模式

在系统复位后，微控制器处于正常工作状态（运行模式），此时可以通过降低系统主频或者关闭不用外设时钟或者降低工作外设时钟来节省系统功耗。如果系统不需要工作，可设置系统进入低功耗模式，并通过特定事件让系统跳出此状态。

微控制器目前提供了 3 种低功耗模式，从处理器、外设、电压调节器等的工作差异上分为：

睡眠模式：内核停止运行，所有外设（包含内核私有外设）仍在运行。

停止模式：停止所有时钟，唤醒后系统继续运行。  
待机模式：停止所有时钟，唤醒后微控制器复位（电源复位）。

表 2-1 低功耗模式一览  

<table><tr><td>模式</td><td>进入</td><td>唤醒源</td><td>对时钟的影响</td><td>电压调节器</td></tr><tr><td rowspan="2">睡眠</td><td>WFI</td><td>任意中断唤醒</td><td rowspan="2">内核时钟关闭,其他时钟无影响</td><td rowspan="2">正常</td></tr><tr><td>WFE</td><td>唤醒事件唤醒</td></tr><tr><td>停止</td><td>SLEEPDEEP 置 1PDDS 清 0WFI 或 WFE</td><td>任一外部中断/事件(在外部中断寄存器中设置)、WKUP 引脚上升沿</td><td>关闭 HSE、HSI、PLL 和外设时钟</td><td>正常: LPDS=0 或低功耗: LPDS=1</td></tr><tr><td>待机</td><td>SLEEPDEEP 置 1PDDS 置 1WFI 或 WFE</td><td>WKUP 引脚上升沿、RTC 闹钟事件、NRST 引脚复位、IWDG 复位。注: 任意事件也可以唤醒系统,但唤醒后系统复位。</td><td>关闭 HSE、HSI、PLL 和外设时钟</td><td>关闭</td></tr></table>

注：SLEEPDEEP位属于内核私有外设控制位，CH32F2x产品参考Cortex-M3内核手册，CH32V2×和产品参考 PFIC SCTLR 寄存器。

### 2.3.1 低功耗配置选项

#### WFI 和 WFE 方式

WFI：微控制器被具有中断控制器响应的中断源唤醒，系统唤醒后，将最先执行中断服务函数（微控制器复位除外）。

WFE：唤醒事件触发微控制器将退出低功耗模式。唤醒事件包括：

1）配置一个外部或内部的EXTI线为事件模式，此时无需配置中断控制器；  
2）或者配置某个中断源，等效为WFI唤醒，系统优先执行中断服务函数；  
3）或者配置SLEEPONPEN位，开启外设中断使能，但不开启中断控制器中的中断使能，系统唤醒后需要清除中断挂起位。

#### l SLEEPONEXIT

启用：执行WFI或WFE指令后，微控制器确保所有待处理的中断服务退出后进入低功耗模式。不启用：执行WFI或WFE指令后，微控制器立即进入低功耗模式。

#### SEVONPEND

启用：所有中断或者唤醒事件都可以唤醒通过执行 WFE 进入的低功耗。

不启用：只有在中断控制器中使能的中断或者唤醒事件可以唤醒通过执行 WFE进入的低功耗。

### 2.3.2 睡眠模式

此模式下，所有的 IO 引脚都保持他们运行模式下的状态，所有的外设时钟都正常，所以进入睡眠模式前，尽量关闭无用的外设时钟，以减低功耗。该模式唤醒所需时间最短。

进入：配置内核寄存器控制位 SLEEPDEEP=0，电源控制寄存器 $\mathsf { P D D S = } 0$ ，LPDS 决定内部调压器状态，执行 WFI 或 WFE，可选 SEVONPEND 和 SLEEPONEXIT。

退出：任意中断或者唤醒事件。

### 2.3.3 停止模式

停止模式是在内核的深睡眠模式（SLEEPDEEP）基础上结合了外设的时钟控制机制，并让电压调节器的运行处于更低功耗的状态。此模式高频时钟（HSE/HSI/PLL）域被关闭，SRAM 和寄存器内容保持，IO引脚状态保持。该模式唤醒后系统可以继续运行，HSI为默认系统时钟。

如果正在进行闪存编程，直到对内存访问完成，系统才进入停止模式；如果正在进行对 APB 的访问，直到对APB访问完成，系统才进入停止模式。

停止模式下可工作模块：独立看门狗（IWDG）、实时时钟（RTC）、低频时钟（LSI/LSE）。

进入：配置内核寄存器控制位 SLEEPDEEP=1，电源控制寄存器的 $\mathsf { P D D S = } 0$ ，可选 LPDS 位，执行 WFI

或 WFE，可选 SEVONPEND 和 SLEEPONEXIT。

退出：任一外部中断/事件(在外部中断寄存器中设置)。

在停止模式下，可选 LPDS 位， $\mathsf { L P D S = } 0$ ，电压调节器工作在正常模式； $\mathsf { L P D S } = 1$ ，电压调节器工作在低功耗模式。在低功耗模式下，可以通过配置 PWR_CTLR寄存器的RAMLV=1，使能 RAM低电压模式，功耗达到最低。

### 2.3.4 待机模式

待机模式对比停止模式，唯一的差别在于：在某些指定的唤醒条件下退出后，微控制器将被复位，并且执行的是电源复位。

待机模式下可工作模块：独立看门狗（IWDG）、实时时钟（RTC）、低频时钟（LSI/LSE）。

进入：配置内核寄存器控制位 SLEEPDEEP=1，电源控制寄存器的 $\mathsf { P D D S } = 1$ ，执行 WFI 或 WFE，可选SEVONPEND 和 SLEEPONEXIT。

退出：

1）任一事件(在外部中断寄存器中设置)，此唤醒后微控制器执行电源复位。  
2）WKUP引脚的上升沿、RTC闹钟事件的上升沿、NRST引脚上外部复位、IWDG 复位，此唤醒后微控制器执行电源复位。

在待机模式下，当正常供电时，通过配置 PWR_CTLR寄存器的 $R 2 { \sf K S } { \sf T } { \sf Y } = 1$ 控制 2K字节 RAM不掉电，${ \sf R } 3 0 { \sf K } { \sf S } { \sf T } { \sf Y } = 1$ 控制 30K 字节 RAM 不掉电；当使用 VBAT 供电时，通过配置 PWR_CTLR 寄存器的 R2KVBAT=1控制 2K 字节 RAM 不掉电，R32K_VBATEN $= 1$ 控制 30K字节 RAM不掉电。在该基础之上，可以通过配置PWR_CTLR 寄存器的 RAMLV=1，使能 RAM 低电压模式，功耗达到最低。

注：调试模式下，使微处理器进入停止或待机模式，将失去调试连接。

$R 2 K S T Y = 1$ 控制 2K 字节 RAM 的地址范围： 0x20000000—0x20000000+2K

R30KSTY=1控制30K字节RAM的地址范围： $0 { x } 2 0 0 0 0 0 0 0 { + } 2 { \cal K } .$ 0x20000000+2K+30K

### 2.3.5 RTC 自动唤醒

RTC 可以实现无需外部中断的情况下自动唤醒。通过对时间基数进行编程，可周期性地从停止或待机模式下唤醒。

可选择精准的外部低频 32.768KHz 晶振 LSE 作为 RTC 时钟源，也可以选择内部 LSI 振荡器作为RTC时钟源，LSI的精度和功耗指标要差于 LSE。

RTC 闹钟事件能够把 MCU 从停机模式下唤醒，为了实现此功能，需要把外部中断线 17 配置为上升沿中断，并且把RTC设置成可产生闹钟事件。而从待机模式下唤醒，仅需把 RTC设置成可产生闹钟事件。

## 2.4 寄存器描述

表 2-2 PWR 相关寄存器列表  

<table><tr><td>名称</td><td>访问地址</td><td>描述</td><td>复位值</td></tr><tr><td>R32_PWR_CTLR</td><td>0x40007000</td><td>电源控制寄存器</td><td>0x00000000</td></tr><tr><td>R32_PWR_CSR</td><td>0x40007004</td><td>电源控制/状态寄存器</td><td>0x00000000</td></tr></table>

### 2.4.1 电源控制寄存器（PWR_CTLR）

偏移地址： $0 \times 0 0$

<table><tr><td>Reserved</td><td>RAM
LV</td><td>R30K
VBAT</td><td>R2K
VBAT</td><td>R30K
STY</td><td>R2K
STY</td></tr></table>

<table><tr><td>15</td><td>14</td><td>13</td><td>12</td><td>11</td><td>10</td><td>9</td><td>8</td><td>7</td><td>6</td><td>5</td><td>4</td><td>3</td><td>2</td><td>1</td><td>0</td></tr><tr><td colspan="6">Reserved</td><td>DBP</td><td colspan="2">PLS[2:0]</td><td>PVDE</td><td>CSBF</td><td>CWUF</td><td>PDDS</td><td colspan="2">LPDS</td><td></td></tr></table>

<table><tr><td>位</td><td>名称</td><td>访问</td><td>描述</td><td>复位值</td></tr><tr><td>[31:21]</td><td>Reserved</td><td>R0</td><td>保留。</td><td>0</td></tr><tr><td>20</td><td>RAMLV</td><td>RW</td><td>RAM工作在低电压模式使能控制位(功耗相对更低):1:开启;0:关闭。注:在PWR_CTLR寄存器的LPDS位为1时有效。</td><td>0</td></tr><tr><td>19</td><td>R30KVBAT</td><td>RW</td><td>VBAT供电时,Standby模式下30K RAM是否带电控制位:1:带电;0:不带电。注:适用于CH32F20x_D8、CH32F20x_D8C、CH32V30x_D8、CH32V30x_D8C、CH32V20x_D8、CH32V20x_D8W、CH32F20x_D8。</td><td>0</td></tr><tr><td>18</td><td>R2KVBAT</td><td>RW</td><td>VBAT供电时,Standby模式下2K RAM是否带电控制位:1:带电;0:不带电。</td><td>0</td></tr><tr><td>17</td><td>R30KSTY</td><td>RW</td><td>Standby模式下30K RAM是否带电控制位:1:带电;0:不带电。注:适用于CH32F20x_D8、CH32F20x_D8C、CH32V30x_D8、CH32V30x_D8C、CH32V20x_D8、CH32V20x_D8W、CH32F20x_D9。</td><td>0</td></tr><tr><td>16</td><td>R2KSTY</td><td>RW</td><td>Standby模式下2K RAM是否带电控制位:1:带电;0:不带电。</td><td>0</td></tr><tr><td>[15:9]</td><td>Reserved</td><td>R0</td><td>保留。</td><td>0</td></tr><tr><td>8</td><td>DBP</td><td>RW</td><td>后备区域的写使能。当RTC时钟为外部时钟的128分频时,该位必须设置为1。1:允许写RTC和后备寄存器;0:禁止写RTC和后备寄存器。</td><td>0</td></tr><tr><td>[7:5]</td><td>PLS[2:0]</td><td>RW</td><td>PVD电压监视阈值设置。详细说明见数据手册中电气特性部分。000:上升沿2.37V/下降沿2.29V;001:上升沿2.55V/下降沿2.46V;010:上升沿2.63V/下降沿2.55V;011:上升沿2.76V/下降沿2.67V;100:上升沿2.87V/下降沿2.78V;101:上升沿3.03V/下降沿2.93V;110:上升沿3.18V/下降沿3.06V;111:上升沿3.29V/下降沿3.19V。</td><td>000b</td></tr><tr><td>4</td><td>PVDE</td><td>RW</td><td>电源电压监视功能使能标志位:1:开启电源电压监视功能;0:禁止电源电压监视功能。</td><td>0</td></tr><tr><td>3</td><td>CSBF</td><td>RW1</td><td>清除待机状态标志位,读出始终为0。1:置1清除SBF待机状态标志位;0:清0无效。</td><td>0</td></tr><tr><td>2</td><td>CWUF</td><td>RW1</td><td>清除唤醒状态标志位,读出始终为0。1:置1后2个系统时钟周期后清除WUF标志位;0:清0无效。</td><td>0</td></tr><tr><td>1</td><td>PDDS</td><td>RW</td><td>掉电深睡眠情景下,待机/停机模式选择位。1:进入待机模式;0:进入停机模式,电压调节器状态由LPDS控制。</td><td>0</td></tr><tr><td>0</td><td>LPDS</td><td>RW</td><td>停机模式下,电压调节器工作模式选择位。PDDS=0,该位有效。1:电压调节器工作在低功耗模式;0:电压调节器工作在正常模式。</td><td>0</td></tr></table>

注：此寄存器从待机模式唤醒时复位。

### 2.4.2 电源控制/状态寄存器（PWR_CSR）

偏移地址： $0 \times 0 4$

<table><tr><td>31</td><td>30</td><td>29</td><td>28</td><td>27</td><td>26</td><td>25</td><td>24</td><td>23</td><td>22</td><td>21</td><td>20</td><td>19</td><td>18</td><td>17</td><td>16</td></tr><tr><td colspan="16">Reserved</td></tr><tr><td>15</td><td>14</td><td>13</td><td>12</td><td>11</td><td>10</td><td>9</td><td>8</td><td>7</td><td>6</td><td>5</td><td>4</td><td>3</td><td>2</td><td>1</td><td>0</td></tr><tr><td colspan="7">Reserved</td><td>EWUP</td><td colspan="5">Reserved</td><td>PVDO</td><td>SBF</td><td>WUF</td></tr></table>

<table><tr><td>位</td><td>名称</td><td>访问</td><td>描述</td><td>复位值</td></tr><tr><td>[31:9]</td><td>Reserved</td><td>R0</td><td>保留。</td><td>0</td></tr><tr><td>8</td><td>EWUP</td><td>RW</td><td>WKUP引脚使能位:1:WKUP强制配置为输入下拉状态,用于把MCU从待机状态下唤醒;0:WKUP引脚可用于通用IO,无待机唤醒功能。</td><td>0</td></tr><tr><td>[7:3]</td><td>Reserved</td><td>R0</td><td>保留。</td><td>0</td></tr><tr><td>2</td><td>PVDO</td><td>R0</td><td>PVD输出状态标志位。当PWR_CTLR寄存器的PVDE=1时,该位有效。1:VDD和VDDA低于PLS[2:0]设定的PVD阈值;0:VDD和VDDA高于PLS[2:0]设定的PVD阈值。</td><td>0</td></tr><tr><td>1</td><td>SBF</td><td>R0</td><td>待机状态标志位,可通过CSBF位置1清除。1:MCU进入待机模式;0:MCU不在待机模式。</td><td>0</td></tr><tr><td>0</td><td>WUF</td><td>R0</td><td>唤醒事件状态标志位,可通过CWUF位置1清除。1:在WKUP引脚检测到唤醒事件或RTC闹钟事件;0:没有唤醒事件发生。</td><td>0</td></tr></table>

注：此寄存器从待机模式唤醒后保持不变。

# 第 3 章 复位和时钟控制（RCC）

本章模块描述适用于CH32F2x、CH32V2x和 $_ { C H 3 2 V 3 x }$ 微控制器全系列产品。

控制器根据电源区域的划分以及应用中的外设功耗管理考虑，提供了不同的复位形式以及可配置的时钟树结构。此章节描述了系统中各个时钟的作用域。

## 3.1 主要特性

多种复位形式  
$\bullet$ 多路时钟源，总线时钟管理  
$\bullet$ 内置外部晶体振荡监测和时钟安全系统  
$\bullet$ 各外设时钟独立管理：复位、开启、关闭  
支持内部时钟输出

## 3.2 复位

控制器提供了 3 种复位形式：电源复位、系统复位和后备区域复位。

### 3.2.1 电源复位

电源复位发生时，将复位除了后备区域外的所有寄存器（后备区域由 $\mathsf { V } _ { \mathsf { B A T } }$ 供电）。

其产生条件包括：

上电/掉电复位(POR/PDR 复位)  
从待机模式下唤醒

### 3.2.2 系统复位

系统复位发生时，将复位除了控制/状态寄存器 RCC_RSTSCKR 中的复位标志和后备区域外的所有寄存器。通过查看RCC_RSTSCKR寄存器中的复位状态标志位识别复位事件来源。

其产生条件包括：

NRST引脚上的低电平信号（外部复位）  
$\bullet$ 窗口看门狗计数终止(WWDG 复位)   
$\bullet$ 独立看门狗计数终止(IWDG 复位)  
软件复位(SW 复位)  
低功耗管理复位

窗口/独立看门狗复位：由窗口/独立看门狗外设定时器计数周期溢出触发产生，详细描述看其相应章节。

软件复位：CH32F2x 产品通过内核寄存器 AIRCR 中的 bit2 置 1 复位系统，具体操作请参考 Cortex-M3 内核手册获得更详细信息。CH32V2x 和 CH32V3x 产品通过可编程中断控制器 PFIC 中的中断配置寄存器 PFIC_CFGR 的 SYSRST 位置 1 复位系统或配置寄存器 PFIC_SCTLR 的 SYSRST 位置 1 复位系统，具体参考对应章节。

低功耗管理复位：通过将用户选择字节中的 STANDY_RST位置 0，将启用待机模式复位。这时执行了进入待机模式的过程后，将执行系统复位而不是进入待机模式。通过将用户选择字节中的 STOP_RST位置0，将启用停机模式复位。这时执行了进入停机模式的过程后，将执行系统复位而不是进入停机模式。

![](images/a78bef7b6b0c8c78fcf548cac76d105fd3101c08bf5ffc891c1af2477bd0eeb3.jpg)  
图 3-1系统复位结构

### 3.2.3 后备区域复位

后备区域复位发生时，只会复位后备区域寄存器，包括后备寄存器、RCC_BDCTLR 寄存器（RTC 使能和 LSE振荡器）。其产生条件包括：

$\bullet$ 在 $\mathsf { V } _ { \mathsf { D } \mathsf { D } }$ 和 $V _ { \mathsf { B A T } }$ 都掉电的前提下，由 $\mathsf { V } _ { \mathsf { D } \mathsf { D } }$ 或 $V _ { \mathsf { B A T } }$ 上电引起  
$\bullet$ RCC_BDCTLR 寄存器的 BDRST 位置 1  
RCC_APB1PRSTR 寄存器的 BKPRST 位置 1

## 3.3 时钟

### 3.3.1 系统时钟结构

图 3-2 CH32V305/307 和 CH32F205/207 时钟树框图  
![](images/c91960c5d87c364122ab44a041e6782099f71ed61085a2cd50acf395e0e48a7b.jpg)  
注：本时钟树适用于CH32F20xD8C和CH32V30xD8C。当使用USB功能时，CPU的频率必须是48MHz、96MHz或144MHz。使用USB高速功能时，USBHSPLL的时钟源只能为HSE。当系统从睡眠状态唤醒时系统会自动切换为HSI做主频。

图 3-3 CH32FV203/V303 时钟树框图  
![](images/8693b8502f18603f9e7597c70e4c306ff6950d317975ce47952f2b15b5c5f25d.jpg)  
注：本时钟树适用于CH32F20x_D6、CH32F20x_D8、CH32V20x_D6和CH32V30x_D8。

图 3-4 CH32V203RB 时钟树结构  
![](images/9aeb054c83cc8dcf410c871c0b95cef1853f669759a7f31dab349fc7e9209678.jpg)  
注：CH32V203RB产品外接晶体或时钟（HSE）为32M，使用外置晶体时无需负载电容已内置。

图 3-5 CH32FV208 时钟树结构  
![](images/2c0ba877f9d783be55537269b419c58dda5a74e9c4254a59847c33c690f46b63.jpg)  
注：本时钟树适用于CH32F20x_D8W、CH32V20x_D8和CH32V20x_D8W。若同时使用USB和ETH功能，需将USBPRE[1:0]置为11b。产品外接晶体或时钟（HSE）为32M，使用外置晶体时无需负载电容已内置。

### 3.3.2 高速时钟（HSI/HSE）

HSI 是系统内部 8MHz 的 RC 振荡器产生的高速时钟信号。HSI RC 振荡器能够在不需要任何外部器件的条件下提供系统时钟。它的启动时间很短但时钟频率精度较差。HSI 通过设置 RCC_CTLR 寄存器中的 HSION 位被启动和关闭，HSIRDY 位指示 HSI RC 振荡器是否稳定。系统默认 HSION 和 HSIRDY 置 1（建议不要关闭）。如果设置了RCC_INTR寄存器的 HSIRDYIE 位，将产生相应中断。

l 出厂校准：制造工艺的差异会导致每个芯片的 RC 振荡频率不同，所以在芯片出厂前，会为每颗

芯片进行HSI校准。系统复位后，工厂校准值被装载到 RCC_CTLR寄存器的HSICAL[7:0]中。

用户调整：基于不同的电压或环境温度，应用程序可以通过 RCC_CTLR寄存器里的 HSITRIM[4:0]位来调整HSI频率。

注：如果HSE晶体振荡器失效，HSI时钟会被作为备用时钟源（时钟安全系统）。

HSE 是外部的高速时钟信号，包括外部晶体/陶瓷谐振器产生或者外部高速时钟送入。

l 外部晶体/陶瓷谐振器（HSE晶体）：外接 3-25MHz 外部振荡器为系统提供更为精确的时钟源。进一步信息可参考数据手册的电气特性部分。HSE晶体可以通过设置RCC_CTLR寄存器中的HSEON位被启动和关闭，HSERDY位指示HSE晶体振荡是否稳定，硬件在HSERDY 位置 1后才将时钟送入系统。如果设置了RCC_INTR寄存器的HSERDYIE位，将产生相应中断。

图3-6 高速外部晶体电路  
![](images/4cadef2d18625327bde392668f21a18e5d913dbe58bc05bbf3649147e4ce29ef.jpg)  
注：负载电容需要尽可能地靠近振荡器引脚，并根据晶体厂家参数选择容值。

l 外部高速时钟源（HSE旁路）：此模式从外部直接送入时钟源到 OSC_IN 引脚，OSC_OUT引脚悬空。最高支持25MHz频率。应用程序需在HSEON 位为 0情况下，置位HSEBYP位，打开 HSE旁路功能，然后再置位HSEON位。

![](images/9fe818b3a416fd0e3585bfd8a9d001d68be46bab812fb8822d99809ac3689bb1.jpg)  
图3-7 高速时钟源电路

### 3.3.3 低速时钟（LSI/LSE）

LSI 是系统内部约 40KHz 的 RC 振荡器产生的低速时钟信号。它可以在停机和待机模式下保持运行，为RTC时钟、独立看门狗和唤醒单元提供时钟基准。进一步信息可参考数据手册的电气特性部分。LSI 可以通过设置 RCC_RSTSCKR 寄存器中的 LSION 位被启动和关闭，然后通过查询 LSIRDY 位检测 LSIRC 振荡是否稳定，硬件在 LSIRDY 位置 1 后才将时钟送入。如果设置了 RCC_INTR 寄存器的 LSIRDYIE位，将产生相应中断。

LSE是外部的低速时钟信号，包括外部晶体/陶瓷谐振器产生或者外部低速时钟送入。它为 RTC时钟或者其他定时功能提供一个低功耗且精确的时钟源。

l 外部晶体/陶瓷谐振器（LSE晶体）：外接 32.768KHz的外部低速振荡器。LSE通过设置 RCC_BDCTLR寄存器中的LSEON位被启动和关闭，LSERDY 位指示LSE 晶体振荡是否稳定，硬件在 LSERDY位置1后才将时钟送入系统。如果设置了RCC_INTR 寄存器的 LSERDYIE位，将产生相应中断。

![](images/6d3e22ee7046be55b83d770ee955b33756ceadb94652a6abeb47fc95a7c841b0.jpg)  
图3-8 低速外部晶体电路

l 外部低速时钟源（LSE旁路）：此模式从外部直接送入时钟源到 OSC32_IN 引脚，OSC32_OUT 引脚悬空。应用程序需在 LSEON 位为 0 情况下，置位 LSEBYP 位，打开 LSE 旁路功能，然后再置位 LSEON位。

![](images/c2a4f4143c40538744ffd88a2cfe207f2d473b4ab25810be00afb6571aa93b6a.jpg)  
图 3-9 低速时钟源电路

### 3.3.4 PLL 时钟

通过配置RCC_CFGR0寄存器和扩展寄存器 EXTEN_CTR，内部PLL时钟可以选择 3 种时钟来源和倍频系数，这些设置必须在每个 PLL 被开启前完成，一旦 PLL 被启动，这些参数就不能被改动。设置RCC_CTLR 寄存器中的 PLLON 位被启动和关闭，PLLRDY 位指示 PLL 时钟是否稳定，硬件在 PLLRDY 位置1后才将时钟送入系统。设置RCC_CTLR寄存器中的PLLON2位被启动和关闭，PLLRDY2位指示 PLL2时钟是否稳定，硬件在PLLRDY2位置1后才将时钟送入系统。设置RCC_CTLR寄存器中的 PLLON3位被启动和关闭，PLLRDY3 位指示 PLL3 时钟是否稳定，硬件在 PLLRDY3 位置 1 后才将时钟送入系统。如果设置了 RCC_INTR 寄存器的 PLLRDYIE 位、PLL2RDYIE 位或 PLL3RDYIE 位，将产生相应中断。

PLL 时钟来源：

HSI时钟送入  
HSI 经过 2 分频送入  
HSE 时钟或通过一个可配置的分频器的 PLL2 时钟

PLL2 和 PLL3 由 HSE 通过一个可配置的分频器（PREDIV2）2 提供时钟

### 3.3.5 总线/外设时钟

#### 3.3.5.1 系统时钟（SYSCLK）

通过配置RCC_CFGR0寄存器SW[1:0]位配置系统时钟来源，SWS[1:0]指示当前的系统时钟源。

HSI作为系统时钟  
HSE作为系统时钟  
PLL时钟作为系统时钟

控制器复位后，默认 HSI 时钟被选为系统时钟源。时钟源之间的切换必须在目标时钟源准备就绪后才会发生。

#### 3.3.5.2 AHB/APB1/APB2 总线外设时钟（HCLK/PCLK1/PCLK2）

通过配置 RCC_CFGR0 寄存器的 HPRE[3:0]、PPRE1[2:0]、PPRE2[2:0]位，可以分别配置 AHB、APB1、APB2 总线的时钟。这些总线时钟决定了挂载在其下面的外设接口访问时钟基准。应用程序可以调整不同的数值，来降低部分外设工作时的功耗。

通过 RCC_AHBRSTR、RCC_APB1PRSTR、RCC_APB2PRSTR 寄存器中各个位可以复位不同的外设模块，

将其恢复到初始状态。

通过 RCC_AHBPCENR、RCC_APB1PCENR、RCC_APB2PCENR 寄存器中各个位可以单独开启或关闭不同外设模块通讯时钟接口。使用某个外设时，首先需要开启其时钟使能位，才能访问其寄存器。

#### 3.3.5.3 RTC 时钟（RTCCLK）

通过设置 RCC_BDCTLR 寄存器的 RTCSEL[1:0]位，RTCCLK 时钟源可以由 HSE/128、LSE 或 LSI 时钟提供。修改此位前要保证电源控制寄存器(PWR_CR)中的 DBP 位置 1，只有后备区域复位，才能复位此位。

LSE 作为 RTC 时钟：由于 LSE 处于后备域由 $V _ { \mathsf { B A T } }$ 供电，只要 $V _ { \mathsf { B A T } }$ 维持供电，尽管 $\mathsf { V } _ { \mathsf { D } \mathsf { D } }$ 供电被切断，RTC 仍继续工作。  
LSI 作为 RTC 时钟：如果 $\mathsf { V } _ { \mathsf { D } \mathsf { D } }$ 供电被切断，RTC 自动唤醒不能保证。  
HSE/128 作为 RTC 时钟：如果 VDD 供电被切断或内部电压调压器被关闭(1.8V 域的供电被切断)，则 RTC 状态不确定。

#### 3.3.5.4 独立看门狗时钟

如果独立看门狗已经由硬件配置设置或软件启动，LSI 振荡器将被强制打开，并且不能被关闭。在LSI振荡器稳定后，时钟供应给IWDG。

#### 3.3.5.5 时钟输出（MCO）

微控制器允许输出时钟信号到 MCO 引脚。在相应的 GPIO 端口寄存器配置复用推挽输出模式，通过配置RCC_CFGR0寄存器MCO[3:0]位，可以选择以下 8 个时钟信号作为 MCO时钟输出：

系统时钟(SYSCLK)输出  
HSI 时钟输出  
$\bullet$ HSE 时钟输出  
$\bullet$ PLL时钟经过2分频输出  
$\bullet$ PLL2 时钟输出  
PLL3 时钟输出  
PLL3 时钟经过 2 分频输出  
XT1外部3-25MHz振荡器（用于以太网）

#### 3.3.5.6 USB 时钟

USBD 48MHz 时钟源来自通过一个可配置的分频器的 PLL 时钟，此时 PLL 支持三种时钟配置，包括 48MHz、96MHz 和 144MHz，通过配置寄存器 RCC_CFGR0 的 USBPRE[1:0]位输出 48MHz 时钟到 USBD。

OTG_FS/USBFS 48MHz 时钟源来自通过一个可配置的分频器的 PLL 时钟或者 USBHSPLL 时钟，可通过配置寄存器 RCC_CFGR2 的 USBHSSRC 位来选择。若时钟源选择通过一个可配置的分频器的 PLL 时钟作为时钟源时，则配置步骤可参考 USBD。若时钟源选择 USBHSPLL 时钟作为时钟源时，通过配置寄存器 RCC_CFGR2 的 USBHSCLK [1:0]位，选择 USBHS PLL 参考时钟频率（参考时钟频率必须和 USBHS PLL输入时钟保持一致）。

USBHS 时钟源来自 USBHSPLL 时钟，通过配置寄存器 RCC_CFGR2 的 USBHSCLK [1:0]位，选择 USBHSPLL 参考时钟频率（参考时钟频率必须和 USBHS PLL 输入时钟保持一致），通过配置寄存器 RCC_CFGR2的 USBHSPLL 位，使能 USB PHY 内部 PLL。

#### 3.3.5.7 ETH 时钟

ETH 时钟配置参考 27.1.4.5 章节。

#### 3.3.5.8 I2S 和 RNG 时钟

I2S 和 RNG 的时钟源来自 PLL3VCO 或系统时钟（SYSCLK），I2S2、I2S3 和 TRNG 可分别通过配置寄存器 RCC_CFGR2 的 I2S2SRC 位、I2S3SRC 位和 RNGSRC 位选择时钟源。

### 3.3.6 时钟安全系统

时钟安全系统是控制器的一种运行保护机制，它可以在 HSE 时钟发送故障的情况下，切换到 HSI时钟下，并产生中断通知，允许应用程序软件完成营救操作。

通过设置RCC_CTLR寄存器的CSSON位置 1，激活时钟安全系统。此时，时钟监测器将在 HSE振荡器启动（HSERDY=1）延迟后被使能，并在HSE时钟关闭后关闭。一旦系统运行过程中 HSE时钟发生故障，HSE 振荡器将被关闭，时钟失效事件将被送到高级定时器(TIM1 和 TIM8)的刹车输入端，并产生时钟安全中断，CSSF位置1，并且应用程序进入 NMI不可屏蔽中断，通过置位CSSC位，可以清除 CSSF位标志，可撤销NMI中断挂起位。

如果当前 HSE 作为系统时钟，或者当前 HSE 作为 PLL 输入时钟，PLL 作为系统时钟，时钟安全系统将在HSE故障时自动将系统时钟切换到 HSI振荡器，并关闭 HSE振荡器和PLL。

## 3.4 寄存器描述

表3-1 RCC相关寄存器列表  

<table><tr><td>名称</td><td>访问地址</td><td>描述</td><td>复位值</td></tr><tr><td>R32_RCC_CTLR</td><td>0x40021000</td><td>时钟控制寄存器</td><td>0x0000xx83</td></tr><tr><td>R32_RCC_CFGRO</td><td>0x40021004</td><td>时钟配置寄存器0</td><td>0x00000000</td></tr><tr><td>R32_RCC_INTR</td><td>0x40021008</td><td>时钟中断寄存器</td><td>0x00000000</td></tr><tr><td>R32_RCC_APB2PRSTR</td><td>0x4002100C</td><td>APB2外设复位寄存器</td><td>0x00000000</td></tr><tr><td>R32_RCC_APB1PRSTR</td><td>0x40021010</td><td>APB1外设复位寄存器</td><td>0x00000000</td></tr><tr><td>R32_RCC_AHBPCENR</td><td>0x40021014</td><td>AHB外设时钟使能寄存器</td><td>0x00000014</td></tr><tr><td>R32_RCC_APB2PCENR</td><td>0x40021018</td><td>APB2外设时钟使能寄存器</td><td>0x00000000</td></tr><tr><td>R32_RCC_APB1PCENR</td><td>0x4002101C</td><td>APB1外设时钟使能寄存器</td><td>0x00000000</td></tr><tr><td>R32_RCC_BDCTLR</td><td>0x40021020</td><td>后备域控制寄存器</td><td>0x00000000</td></tr><tr><td>R32_RCC_RSTSCKR</td><td>0x40021024</td><td>控制/状态寄存器</td><td>0x0C000000</td></tr><tr><td>R32_RCC_AHBRSTR</td><td>0x40021028</td><td>AHB外设复位寄存器</td><td>0x00000000</td></tr><tr><td>R32_RCC_CFGR2</td><td>0x4002102C</td><td>时钟配置寄存器2</td><td>0x00000000</td></tr></table>

表 3-2 OSC 相关寄存器列表  

<table><tr><td>名称</td><td>访问地址</td><td>描述</td><td>复位值</td></tr><tr><td>R32_HSE_CAL_CTRL</td><td>0x4002202C</td><td>外部晶振校准控制寄存器</td><td>0x09000000</td></tr><tr><td>R16_LSI32K_TUNE</td><td>0x40022036</td><td>内部低速晶振校准调节寄存器</td><td>0x1011</td></tr><tr><td>R8_LSI32K_CAL_CFG</td><td>0x40022049</td><td>内部低速晶振校准配置寄存器</td><td>0x01</td></tr><tr><td>R16_LSI32K_CAL_STAT</td><td>0x4002204C</td><td>内部低速晶振校准状态寄存器</td><td>0x0000</td></tr><tr><td>R8_LSI32K_CAL_OV_CNT</td><td>0x4002204E</td><td>内部低速晶振校准次数计数器</td><td>0x00</td></tr><tr><td>R8_LSI32K_CAL_CTRL</td><td>0x4002204F</td><td>内部低速晶振校准控制寄存器</td><td>0x80</td></tr></table>

注：适用于CH32V20x_D8W、CH32F20x_D8W。

### 3.4.1 时钟控制寄存器（RCC_CTLR）

偏移地址： $0 \times 0 0$   

<table><tr><td>31</td><td>30</td><td>29</td><td>28</td><td>27</td><td>26</td><td>25</td><td>24</td><td>23</td><td>22</td><td>21</td><td>20</td><td>19</td><td>18</td><td>17</td><td>16</td></tr><tr><td>Reserved</td><td>PLL3 RDY</td><td>PLL3 ON</td><td>PLL2 RDY</td><td>PLL2 ON</td><td>PLL RDY</td><td>PLL ON</td><td colspan="5">Reserved</td><td>CSSON</td><td>HSE BYP</td><td>HSE RDY</td><td>HSEON</td></tr><tr><td>15</td><td>14</td><td>13</td><td>12</td><td>11</td><td>10</td><td>9</td><td>8</td><td>7</td><td>6</td><td>5</td><td>4</td><td>3</td><td>2</td><td>1</td><td>0</td></tr><tr><td colspan="7">HSICAL[7:0]</td><td colspan="5">HSITRIM[4:0]</td><td>Reser ved</td><td>HSI RDY</td><td colspan="2">HSION</td></tr></table>

<table><tr><td>位</td><td>名称</td><td>访问</td><td>描述</td><td>复位值</td></tr><tr><td>[31:30]</td><td>Reserved</td><td>R0</td><td>保留。</td><td>0</td></tr><tr><td>29</td><td>PLL3RDY</td><td>R0</td><td>PLL3时钟就绪锁定标志位(由硬件置位):1:PLL3时钟锁定;0:PLL3时钟未锁定。注:适用于CH32F20x_D8C、CH32V30x_D8C。</td><td>0</td></tr><tr><td>28</td><td>PLL3ON</td><td>RW</td><td>PLL3时钟使能控制位:1:使能PLL3时钟;0:关闭PLL3时钟。注:进入停止或待机低功耗模式后,此位由硬件清0。适用于CH32F20x_D8C、CH32V30x_D8C。</td><td>0</td></tr><tr><td>27</td><td>PLL2RDY</td><td>R0</td><td>PLL2时钟就绪锁定标志位(由硬件置位):1:PLL时钟锁定;0:PLL时钟未锁定。注:适用于CH32F20x_D8C、CH32V30x_D8C。</td><td>0</td></tr><tr><td>26</td><td>PLL2ON</td><td>RW</td><td>PLL2时钟使能控制位:1:使能PLL时钟;0:关闭PLL时钟。注:进入停止或待机低功耗模式后,此位由硬件清0。适用于CH32F20x_D8C、CH32V30x_D8C。</td><td>0</td></tr><tr><td>25</td><td>PLLRDY</td><td>R0</td><td>PLL时钟就绪锁定标志位(由硬件置位):1:PLL时钟锁定;0:PLL时钟未锁定。</td><td>0</td></tr><tr><td>24</td><td>PLLON</td><td>RW</td><td>PLL时钟使能控制位:1:使能PLL时钟;0:关闭PLL时钟。注:进入停止或待机低功耗模式后,此位由硬件清0。</td><td>0</td></tr><tr><td>[23:20]</td><td>Reserved</td><td>R0</td><td>保留。</td><td>0</td></tr><tr><td>19</td><td>CSSON</td><td>RW</td><td>时钟安全系统使能控制位:1:使能时钟安全系统。当HSE准备好(HSERDY置1),硬件开启对HSE的时钟监测功能,发现HSE异常触发CSSF标志及NMI中断;当HSE没有准备好,硬件关闭对HSE的时钟监测功能。0:关闭时钟安全系统。</td><td>0</td></tr><tr><td>18</td><td>HSEBYP</td><td>RW</td><td>外部高速晶体旁路控制位:1:旁路外部高速晶体/陶瓷谐振器(使用外部时钟源);</td><td>0</td></tr><tr><td></td><td></td><td></td><td>0:不旁路高速外部晶体/陶瓷谐振器。注:此位需在HSEON为0下写入。</td><td></td></tr><tr><td>17</td><td>HSERDY</td><td>RO</td><td>外部高速晶体振荡稳定就绪标志位(由硬件置位):1:外部高速晶体振荡稳定;0:外部高速晶体振荡没有稳定。注:在HSEON位清0后,该位需要6个HSE周期清0。</td><td>0</td></tr><tr><td>16</td><td>HSEON</td><td>RW</td><td>外部高速晶体振荡使能控制位:1:使能HSE振荡器;0:关闭HSE振荡器。注:进入停止或待机低功耗模式后,此位由硬件清0。</td><td>0</td></tr><tr><td>[15:8]</td><td>HSICAL[7:0]</td><td>RO</td><td>内部高速时钟校准值,在系统启动时被自动初始化。</td><td>xxh</td></tr><tr><td>[7:3]</td><td>HSITRIM[4:0]</td><td>RW</td><td>内部高速时钟调整值:用户可以输入一个调整值叠加到HSICAL[7:0]数值上,根据电压和温度的变化调整内部HSI RC振荡器的频率。默认值为16,可以把HSI调整到8MHz±1%;每步HSICAL的变化调整约40KHz。</td><td>10000b</td></tr><tr><td>2</td><td>Reserved</td><td>RO</td><td>保留。</td><td>0</td></tr><tr><td>1</td><td>HSIRDY</td><td>RO</td><td>内部高速时钟(8MHz)稳定就绪标志位(由硬件置位):1:内部高速时钟(8MHz)稳定;0:内部高速时钟(8MHz)没有稳定。注:在HSION位清0后,该位需要6个HSI周期清0。</td><td>1</td></tr><tr><td>0</td><td>HSION</td><td>RW</td><td>内部高速时钟(8MHz)使能控制位:1:使能HSI振荡器;0:关闭HSI振荡器。注:当从待机和停止模式返回或用作系统时钟的外部振荡器HSE发生故障时,该位由硬件置1来启动内部8MHz的RC振荡器。</td><td>1</td></tr></table>

### 3.4.2 时钟配置寄存器 0（RCC_CFGR0）

偏移地址： $0 \times 0 4$

<table><tr><td>31</td><td>30</td><td>29</td><td>28</td><td>27</td><td>26</td><td>25</td><td>24</td><td>23</td><td>22</td><td>21</td><td>20</td><td>19</td><td>18</td><td>17</td><td>16</td></tr><tr><td>ADCDUTY</td><td colspan="2">Reserved</td><td colspan="2">ETHP
RE</td><td colspan="3">MCO[3:0]</td><td colspan="2">USBPRE
[1:0]</td><td colspan="4">PLLMUL[3:0]</td><td>PLL
XTPRE</td><td>PLL
SRC</td></tr><tr><td>15</td><td>14</td><td>13</td><td>12</td><td>11</td><td>10</td><td>9</td><td>8</td><td>7</td><td>6</td><td>5</td><td>4</td><td>3</td><td>2</td><td>1</td><td>0</td></tr><tr><td colspan="2">ADCPRE[1:0]</td><td colspan="3">PPRE2[2:0]</td><td colspan="3">PPRE1[2:0]</td><td colspan="3">HPRE[3:0]</td><td colspan="3">SWS[1:0]</td><td colspan="2">SW[1:0]</td></tr></table>

<table><tr><td>位</td><td>名称</td><td>访问</td><td>描述</td><td>复位值</td></tr><tr><td>31</td><td>ADCDUTY</td><td>RW</td><td>ADC时钟占空比调整:1:ADC时钟低电平时间更长;0:ADC时钟占空比为50%。</td><td>0</td></tr><tr><td>[30:29]</td><td>Reserved</td><td>RO</td><td>保留。</td><td>0</td></tr><tr><td>28</td><td>ETHPRE</td><td>RW</td><td>以太网时钟来源预分频控制:0:不分频;</td><td>0</td></tr><tr><td></td><td></td><td></td><td>1:2分频;注:适用于CH32V20x_D8W、CH32V20x_D8、CH32F20x_D8W。</td><td></td></tr><tr><td>[27:24]</td><td>MCO[3:0]</td><td>RW</td><td>微控制器MCO引脚时钟输出控制:00xx:没有时钟输出;0100:系统时钟(SYSCLK)输出;0101:内部8MHz的RC振荡器时钟(HSI)输出;0110:外部振荡器时钟(HSE)输出;0111:PLL时钟2分频后输出;1000:PLL2时钟输出;1001:PLL3时钟2分频后输出;1010:XT1外部震荡器时钟输出;1011:PLL3时钟输出。注:在启动或切换MCO时钟时,可能有几个周期的时钟丢失。其中1000——1011适用于适用于CH32F20x_D8C、CH32V30x_D8C。</td><td>0000b</td></tr><tr><td>[23:22]</td><td>USBPRE[1:0]</td><td>RW</td><td>USBFS/USBOTG时钟分频配置:00:1分频(适用于PLLCLK=48MHz);01:2分频(适用于PLLCLK=96MHz);10:3分频(适用于PLLCLK=144MHz);11:5分频,且PLL的源为HSE二分频(适用于PLLCLK=240MHz,仅适用于CH32V20x_D8W/CH32F20x_D8W)。注:CH32V20x_D8W、CH32F20x_D8W具有11b选项,其余型号该选项保留。</td><td>00b</td></tr><tr><td>[21:18]</td><td>PLLMUL[3:0]</td><td>RW</td><td>PLL时钟倍频系数(在PLL关闭才可写入):对于CH32V20x_D8、CH32V20x_D8W、CH32F20x_D8W、CH32V30x_D8、CH32F20x_D8、CH32V20x_D8W、CH32F20x_D8W:0000:PLL2倍频输出;0001:PLL3倍频输出;0010:PLL4倍频输出;0011:PLL5倍频输出;0100:PLL6倍频输出;0101:PLL7倍频输出;0110:PLL8倍频输出;0111:PLL9倍频输出;1000:PLL10倍频输出;1001:PLL11倍频输出;1010:PLL12倍频输出;1011:PLL13倍频输出;1100:PLL14倍频输出;1101:PLL15倍频输出;1110:PLL16倍频输出;1111:PLL18倍频输出。对于CH32F20x_D8C、CH32V30x_D8C:0000:PLL18倍频输出;0001:PLL3倍频输出;0010:PLL4倍频输出;0011:PLL5倍频输出;0100:PLL6倍频输出;0101:PLL7倍频输出;0110:PLL8倍频输出;0111:PLL9倍频输出;1000:PLL10倍频输出;1001:PLL 11倍频输出;1010:PLL12倍频输出;1011:PLL13倍频输出;1100:PLL14倍频输出;1101:PLL6.5倍频输出;1110:PLL15倍频输出;1111:PLL16倍频输出。</td><td>0000b</td></tr><tr><td>17</td><td>PLLXTPRE</td><td>RW</td><td>HSE分频送入PLL控制(在PLL关闭才可写入):对于CH32F20x_D6、CH32F20x_D8、CH32F20x_D8C、CH32V20x_D6、CH32V30x_D8、CH32V30x_D8C:1:HSE2分频送入PLL;0:HSE不分频送入PLL。对于CH32F20x_D8W、CH32V20x_D8、CH32V20x_D8W:1:HSE8分频送入PLL;0:HSE4分频送入PLL。</td><td>0</td></tr><tr><td>16</td><td>PLLSRC</td><td>RW</td><td>PLL的输入时钟源(在PLL关闭才可写入):1:HSE不分频或2分频送入PLL;0:HSI不分频或2分频送入PLL。</td><td>0</td></tr><tr><td>[15:14]</td><td>ADCPRE[1:0]</td><td>RW</td><td>ADC时钟来源预分频控制:00:PCLK22分频后作为ADC时钟;01:PCLK24分频后作为ADC时钟;10:PCLK26分频后作为ADC时钟;11:PCLK28分频后作为ADC时钟。注:ADC时钟最高不要超过14MHz。</td><td>00b</td></tr><tr><td>[13:11]</td><td>PPRE2[2:0]</td><td>RW</td><td>APB2时钟来源预分频控制:0xx:HCLK不分频;100:HCLK2分频;101:HCLK4分频;110:HCLK8分频;111:HCLK16分频</td><td>000b</td></tr><tr><td>[10:8]</td><td>PPRE1[2:0]</td><td>RW</td><td>APB1时钟来源预分频控制:0xx:HCLK不分频;100:HCLK2分频;101:HCLK4分频;110:HCLK8分频;111:HCLK16分频</td><td>000b</td></tr><tr><td>[7:4]</td><td>HPRE[3:0]</td><td>RW</td><td>AHB时钟来源预分频控制:0xxx:SYSCLK不分频;1000:SYSCLK2分频;1001:SYSCLK4分频;1010:SYSCLK8分频;1011:SYSCLK16分频;1100:SYSCLK64分频;1101:SYSCLK128分频;1110:SYSCLK256分频;1111:SYSCLK512分频。注:当AHB时钟来源的预分频系数大于1时,必须开启预取缓冲器。</td><td>0000b</td></tr><tr><td>[3:2]</td><td>SWS[1:0]</td><td>RO</td><td>系统时钟(SYSCLK)状态(硬件置位):00:系统时钟源是HSI;01:系统时钟源是HSE;10:系统时钟源是PLL;</td><td>00b</td></tr><tr><td></td><td></td><td></td><td>11:不可用。</td><td></td></tr><tr><td>[1:0]</td><td>SW[1:0]</td><td>RW</td><td>选择系统时钟来源:00:HSI作为系统时钟;01:HSE作为系统时钟;10:PLL输出作为系统时钟;11:不可用。注:在使能了时钟安全系统下(CSSON=1),当从待机和停止模式返回或用作系统时钟的外部振荡器HSE发生故障时,由硬件强制选择HSI作为系统时钟。</td><td>00b</td></tr></table>

### 3.4.3 时钟中断寄存器（RCC_INTR）

偏移地址： $0 \times 0 8$

<table><tr><td colspan="9">Reserved</td><td>CSSC</td><td>PLL3 RDYC</td><td>PLL2 RDYC</td><td>PLL RDYC</td><td>HSE RDYC</td><td>HSI RDYC</td><td>LSE RDYC</td><td>LSI RDYC</td></tr><tr><td>15</td><td>14</td><td>13</td><td>12</td><td>11</td><td>10</td><td>9</td><td>8</td><td>7</td><td>6</td><td>5</td><td>4</td><td>3</td><td>2</td><td>1</td><td>0</td><td></td></tr><tr><td>Reserved</td><td>PLL3 RDYIE</td><td>PLL2 RDYIE</td><td>PLL RDYIE</td><td>HSE RDYIE</td><td>HSI RDYIE</td><td>LSE RDYIE</td><td>LSI RDYIE</td><td>CSSF</td><td>PLL3 RDYF</td><td>PLL2 RDYF</td><td>PLL RDYF</td><td>HSE RDYF</td><td>HSI RDYF</td><td>LSE RDYF</td><td>LSI RDYF</td><td>LSI RDYF</td></tr></table>

<table><tr><td>位</td><td>名称</td><td>访问</td><td>描述</td><td>复位值</td></tr><tr><td>[31:24]</td><td>Reserved</td><td>RO</td><td>保留。</td><td>0</td></tr><tr><td>23</td><td>CSSC</td><td>WO</td><td>清除时钟安全系统中断标志位（CSSF）：1：清除CSSF中断标志；0：无动作。</td><td>0</td></tr><tr><td>22</td><td>PLL3RDYC</td><td>WO</td><td>清除PLL3就绪中断标志位：1：清除PLL3RDYF中断标志；0：无动作。注：适用于CH32F20x_D8C、CH32V30x_D8C。</td><td>0</td></tr><tr><td>21</td><td>PLL2RDYC</td><td>WO</td><td>清除PLL2就绪中断标志位：1：清除PLL2RDYF中断标志；0：无动作。注：适用于CH32F20x_D8C、CH32V30x_D8C。</td><td>0</td></tr><tr><td>20</td><td>PLLRDYC</td><td>WO</td><td>清除PLL就绪中断标志位：1：清除PLLRDYF中断标志；0：无动作。</td><td>0</td></tr><tr><td>19</td><td>HSERDYC</td><td>WO</td><td>清除HSE振荡器就绪中断标志位：1：清除HSERDYF中断标志；0：无动作。</td><td>0</td></tr><tr><td>18</td><td>HSIRDYC</td><td>WO</td><td>清除HSI振荡器就绪中断标志位：1：清除HSIRDYF中断标志；0：无动作。</td><td>0</td></tr><tr><td>17</td><td>LSERDYC</td><td>WO</td><td>清除LSE振荡器就绪中断标志位：1：清除LSERDYF中断标志；0：无动作。</td><td>0</td></tr><tr><td>16</td><td>LSIRDYC</td><td>WO</td><td>清除LSI振荡器就绪中断标志位:1:清除LSIRDYF中断标志;0:无动作。</td><td>0</td></tr><tr><td>15</td><td>Reserved</td><td>RO</td><td>保留。</td><td>0</td></tr><tr><td>14</td><td>PLL3RDYIE</td><td>RW</td><td>PLL3就绪中断使能位:1:使能PLL3就绪中断;0:关闭PLL3就绪中断。注:适用于CH32F20x_D8C、CH32V30x_D8C。</td><td>0</td></tr><tr><td>13</td><td>PLL2RDYIE</td><td>RW</td><td>PLL2就绪中断使能位:1:使能PLL2就绪中断;0:关闭PLL2就绪中断。注:适用于CH32F20x_D8C、CH32V30x_D8C。</td><td>0</td></tr><tr><td>12</td><td>PLLRDYIE</td><td>RW</td><td>PLL就绪中断使能位:1:使能PLL就绪中断;0:关闭PLL就绪中断。</td><td>0</td></tr><tr><td>11</td><td>HSERDYIE</td><td>RW</td><td>HSE就绪中断使能位:1:使能HSE就绪中断;0:关闭HSE就绪中断。</td><td>0</td></tr><tr><td>10</td><td>HSIRDYIE</td><td>RW</td><td>HSI就绪中断使能位:1:使能HSI就绪中断;0:关闭HSI就绪中断。</td><td>0</td></tr><tr><td>9</td><td>LSERDYIE</td><td>RW</td><td>LSE就绪中断使能位:1:使能LSE就绪中断;0:关闭LSE就绪中断。</td><td>0</td></tr><tr><td>8</td><td>LSIRDYIE</td><td>RW</td><td>LSI就绪中断使能位:1:使能LSI就绪中断;0:关闭LSI就绪中断。</td><td>0</td></tr><tr><td>7</td><td>CSSF</td><td>RO</td><td>时钟安全系统中断标志位:1:HSE时钟失效,产生了时钟安全中断CSSI;0:无时钟安全系统中断。硬件置位,软件写CSSC位1清除。</td><td>0</td></tr><tr><td>6</td><td>PLL3RDYF</td><td>RO</td><td>PLL3时钟就绪锁定中断标志:1:PLL3时钟锁定产生中断;0:无PLL3时钟锁定中断。硬件置位,软件写PLL3RDYC位1清除。注:适用于CH32F20x_D8C、CH32V30x_D8C。</td><td>0</td></tr><tr><td>5</td><td>PLL2RDYF</td><td>RO</td><td>PLL2时钟就绪锁定中断标志:1:PLL2时钟锁定产生中断;0:无PLL2时钟锁定中断。硬件置位,软件写PLL2RDYC位1清除。注:适用于CH32F20x_D8C、CH32V30x_D8C。</td><td>0</td></tr><tr><td>4</td><td>PLLRDYF</td><td>RO</td><td>PLL时钟就绪锁定中断标志:1:PLL时钟锁定产生中断;0:无PLL时钟锁定中断。硬件置位,软件写PLLRDYC位1清除。</td><td>0</td></tr><tr><td>3</td><td>HSERDYF</td><td>RO</td><td>HSE时钟就绪中断标志:</td><td>0</td></tr><tr><td></td><td></td><td></td><td>1: HSE 时钟就绪产生中断;0: 无 HSE 时钟就绪中断。硬件置位,软件写 HSERDYC 位 1 清除。</td><td></td></tr><tr><td>2</td><td>HSIRDYF</td><td>RO</td><td>HSI 时钟就绪中断标志:1: HSI 时钟就绪产生中断;0: 无 HSI 时钟就绪中断。硬件置位,软件写 HSIRDYC 位 1 清除。</td><td>0</td></tr><tr><td>1</td><td>LSERDYF</td><td>RO</td><td>LSE 时钟就绪中断标志:1: LSE 时钟就绪产生中断;0: 无 LSE 时钟就绪中断。硬件置位,软件写 LSERDYC 位 1 清除。</td><td>0</td></tr><tr><td>0</td><td>LSIRDYF</td><td>RO</td><td>LSI 时钟就绪中断标志:1: LSI 时钟就绪产生中断;0: 无 LSI 时钟就绪中断。硬件置位,软件写 LSIRDYC 位 1 清除。</td><td>0</td></tr></table>

### 3.4.4 APB2 外设复位寄存器（RCC_APB2PRSTR）

偏移地址： $0 \times 0 0$

<table><tr><td colspan="11">Reserved</td><td>TIM10
RST</td><td>TIM9
RST</td><td colspan="4">Reserved</td></tr><tr><td>15</td><td>14</td><td>13</td><td>12</td><td>11</td><td>10</td><td>9</td><td>8</td><td>7</td><td>6</td><td>5</td><td>4</td><td>3</td><td>2</td><td>1</td><td>0</td><td></td></tr><tr><td>Reserved</td><td>USART1
RST</td><td>TIM8
RST</td><td>SPI1
RST</td><td>TIM1
RST</td><td>ADC2
RST</td><td>ADC1
RST</td><td>Reserved</td><td>IOPE
RST</td><td>IOPD
RST</td><td>IOPC
RST</td><td>IOPB
RST</td><td>IOPA
RST</td><td>Reserved</td><td>AFIO
RST</td><td></td><td></td></tr></table>

<table><tr><td>位</td><td>名称</td><td>访问</td><td>描述</td><td>复位值</td></tr><tr><td>[31:21]</td><td>Reserved</td><td>R0</td><td>保留。</td><td>0</td></tr><tr><td>20</td><td>TIM10RST</td><td>RW</td><td>TIM10模块复位控制:1:复位模块; 0:无作用。</td><td>0</td></tr><tr><td>19</td><td>TIM9RST</td><td>RW</td><td>TIM9模块复位控制:1:复位模块; 0:无作用。</td><td>0</td></tr><tr><td>[18:15]</td><td>Reserved</td><td>R0</td><td>保留。</td><td>0</td></tr><tr><td>14</td><td>USART1RST</td><td>RW</td><td>USART1接口复位控制:1:复位模块; 0:无作用。</td><td>0</td></tr><tr><td>13</td><td>TIM8RST</td><td>RW</td><td>TIM8模块复位控制:1:复位模块; 0:无作用。</td><td>0</td></tr><tr><td>12</td><td>SPI1RST</td><td>RW</td><td>SPI1接口复位控制:1:复位模块; 0:无作用。</td><td>0</td></tr><tr><td>11</td><td>TIM1RST</td><td>RW</td><td>TIM1模块复位控制:1:复位模块; 0:无作用。</td><td>0</td></tr><tr><td>10</td><td>ADC2RST</td><td>RW</td><td>ADC2模块复位控制:1:复位模块; 0:无作用。</td><td>0</td></tr><tr><td>9</td><td>ADC1RST</td><td>RW</td><td>ADC1模块复位控制:1:复位模块; 0:无作用。</td><td>0</td></tr><tr><td>[8:7]</td><td>Reserved</td><td>RO</td><td>保留。</td><td>0</td></tr><tr><td>6</td><td>I0PERST</td><td>RW</td><td>IO的PE端口模块复位控制:1:复位模块; 0:无作用。</td><td>0</td></tr><tr><td>5</td><td>I0PDRST</td><td>RW</td><td>IO的PD端口模块复位控制:1:复位模块; 0:无作用。</td><td>0</td></tr><tr><td>4</td><td>I0PCRST</td><td>RW</td><td>IO的PC端口模块复位控制:1:复位模块; 0:无作用。</td><td>0</td></tr><tr><td>3</td><td>I0PBRST</td><td>RW</td><td>IO的PB端口模块复位控制:1:复位模块; 0:无作用。</td><td>0</td></tr><tr><td>2</td><td>I0PARST</td><td>RW</td><td>IO的PA端口模块复位控制:1:复位模块; 0:无作用。</td><td>0</td></tr><tr><td>1</td><td>Reserved</td><td>RO</td><td>保留。</td><td>0</td></tr><tr><td>0</td><td>AFIORST</td><td>RW</td><td>IO辅助功能模块复位控制:1:复位模块; 0:无作用。</td><td>0</td></tr></table>

### 3.4.5 APB1 外设复位寄存器（RCC_APB1PRSTR）

偏移地址： $0 \times 1 0$

<table><tr><td>31</td><td>30</td><td>29</td><td>28</td><td>27</td><td>26</td><td>25</td><td>24</td><td>23</td><td>22</td><td>21</td><td>20</td><td>19</td><td>18</td><td>17</td><td>16</td></tr><tr><td colspan="2">Reserved</td><td>DAC
RST</td><td>PWR
RST</td><td>BKP
RST</td><td>CAN2
RST</td><td>CAN1
RST</td><td>Reserved</td><td>USBD
RST</td><td>I2C2
RST</td><td>I2C1
RST</td><td>UART
5
RST</td><td>UART
4
RST</td><td>USART3
RST</td><td>USART2
RST</td><td>Reserved</td></tr><tr><td>15</td><td>14</td><td>13</td><td>12</td><td>11</td><td>10</td><td>9</td><td>8</td><td>7</td><td>6</td><td>5</td><td>4</td><td>3</td><td>2</td><td>1</td><td>0</td></tr><tr><td>SPI3
RST</td><td>SPI2
RST</td><td colspan="2">Reserved</td><td>WWDG
RST</td><td colspan="2">Reserved</td><td>UART
8RST</td><td>UART
7RST</td><td>UART
6RST</td><td>TIM7
RST</td><td>TIM6
RST</td><td>TIM5
RST</td><td>TIM4
RST</td><td>TIM3
RST</td><td>TIM2
RST</td></tr></table>

<table><tr><td>位</td><td>名称</td><td>访问</td><td>描述</td><td>复位值</td></tr><tr><td>[31:30]</td><td>Reserved</td><td>RO</td><td>保留。</td><td>0</td></tr><tr><td>29</td><td>DACRST</td><td>RW</td><td>DAC模块复位控制:1:复位模块; 0:无作用。</td><td>0</td></tr><tr><td>28</td><td>PWRRST</td><td>RW</td><td>电源接口模块复位控制:1:复位模块; 0:无作用。</td><td>0</td></tr><tr><td>27</td><td>BKPRST</td><td>RW</td><td>后备单元复位控制:1:复位模块; 0:无作用。</td><td>0</td></tr><tr><td>26</td><td>CAN2RST</td><td>RW</td><td>CAN2模块复位控制:1:复位模块; 0:无作用。</td><td>0</td></tr><tr><td>25</td><td>CAN1RST</td><td>RW</td><td>CAN1模块复位控制:1:复位模块; 0:无作用。</td><td>0</td></tr><tr><td>24</td><td>Reserved</td><td>RO</td><td>保留。</td><td>0</td></tr><tr><td>23</td><td>USBDRST</td><td>RW</td><td>USBD模块复位控制:1:复位模块; 0:无作用。</td><td>0</td></tr><tr><td>22</td><td>I2C2RST</td><td>RW</td><td>I2C2接口复位控制:1:复位模块; 0:无作用。</td><td>0</td></tr><tr><td>21</td><td>I2C1RST</td><td>RW</td><td>I2C1接口复位控制:1:复位模块; 0:无作用。</td><td>0</td></tr><tr><td>20</td><td>UART5RST</td><td>RW</td><td>UART5接口复位控制:1:复位模块; 0:无作用。</td><td>0</td></tr><tr><td>19</td><td>UART4RST</td><td>RW</td><td>UART4接口复位控制:1:复位模块; 0:无作用。</td><td>0</td></tr><tr><td>18</td><td>USART3RST</td><td>RW</td><td>USART3接口复位控制:1:复位模块; 0:无作用。</td><td>0</td></tr><tr><td>17</td><td>USART2RST</td><td>RW</td><td>USART2接口复位控制:1:复位模块; 0:无作用。</td><td>0</td></tr><tr><td>16</td><td>Reserved</td><td>RO</td><td>保留。</td><td>0</td></tr><tr><td>15</td><td>SPI3RST</td><td>RW</td><td>SPI3接口复位控制:1:复位模块; 0:无作用。</td><td>0</td></tr><tr><td>14</td><td>SPI2RST</td><td>RW</td><td>SPI2接口复位控制:1:复位模块; 0:无作用。</td><td>0</td></tr><tr><td>[13:12]</td><td>Reserved</td><td>RO</td><td>保留。</td><td>0</td></tr><tr><td>11</td><td>WWDGRST</td><td>RW</td><td>窗口看门狗复位控制:1:复位模块; 0:无作用。</td><td>0</td></tr><tr><td>[10:9]</td><td>Reserved</td><td>RO</td><td>保留。</td><td>0</td></tr><tr><td>8</td><td>UART8RST</td><td>RW</td><td>UART8接口复位控制:1:复位模块; 0:无作用。</td><td>0</td></tr><tr><td>7</td><td>UART7RST</td><td>RW</td><td>UART7接口复位控制:1:复位模块; 0:无作用。</td><td>0</td></tr><tr><td>6</td><td>UART6RST</td><td>RW</td><td>UART6接口复位控制:1:复位模块; 0:无作用。</td><td>0</td></tr><tr><td>5</td><td>TIM7RST</td><td>RW</td><td>定时器7模块复位控制:1:复位模块; 0:无作用。</td><td>0</td></tr><tr><td>4</td><td>TIM6RST</td><td>RW</td><td>定时器6模块复位控制:1:复位模块; 0:无作用。</td><td>0</td></tr><tr><td>3</td><td>TIM5RST</td><td>RW</td><td>定时器5模块复位控制:1:复位模块; 0:无作用。</td><td>0</td></tr><tr><td>2</td><td>TIM4RST</td><td>RW</td><td>定时器4模块复位控制:1:复位模块; 0:无作用。</td><td>0</td></tr><tr><td>1</td><td>TIM3RST</td><td>RW</td><td>定时器3模块复位控制:1:复位模块; 0:无作用。</td><td>0</td></tr><tr><td>0</td><td>TIM2RST</td><td>RW</td><td>定时器2模块复位控制:1:复位模块; 0:无作用。</td><td>0</td></tr></table>

### 3.4.6 AHB 外设时钟使能寄存器（RCC_AHBPCENR）

偏移地址： $0 \times 1 4$

<table><tr><td>31</td><td>30</td><td>29</td><td>28</td><td>27</td><td>26</td><td>25</td><td>24</td><td>23</td><td>22</td><td>21</td><td>20</td><td>19</td><td>18</td><td>17</td><td>16</td></tr><tr><td colspan="14">Reserved</td><td>BLES</td><td>ETHMACRXEN/BLEC</td></tr><tr><td>15</td><td>14</td><td>13</td><td>12</td><td>11</td><td>10</td><td>9</td><td>8</td><td>7</td><td>6</td><td>5</td><td>4</td><td>3</td><td>2</td><td>1</td><td>0</td></tr><tr><td>ETHMACXEN</td><td>ETHMACEN</td><td>DVPEN</td><td>OTGFSEN</td><td>USBHSEN</td><td>SDIOEN</td><td>RNGEN</td><td>FSMCEEN</td><td>Reserved</td><td>CRCEN</td><td colspan="3">Reserved</td><td>SRAMEN</td><td>DMA2EN</td><td>DMA1EN</td></tr></table>

<table><tr><td>位</td><td>名称</td><td>访问</td><td>描述</td><td>复位值</td></tr><tr><td>[31:18]</td><td>Reserved</td><td>R0</td><td>保留。</td><td>0</td></tr><tr><td>17</td><td>BLES</td><td>RW</td><td>BLES时钟使能:1: BLES时钟开启;0: BLES时钟关闭。注:适用于CH32V20x_D8W、CH32F20x_D8W。</td><td>1</td></tr><tr><td>16</td><td>BLEC</td><td>RW</td><td>BLEC时钟使能:1: BLEC时钟开启;0: BLEC时钟关闭。注:适用于CH32V20x_D8W、CH32F20x_D8W。</td><td>1</td></tr><tr><td>16</td><td>ETHMACRXEN</td><td>RW</td><td>以太网MAC接收时钟使能:1: 以太网MAC接收时钟开启;0: 以太网MAC接收时钟关闭。注:适用于CH32V30x_D8C、CH32F20x_D8C百兆千兆外置PHY。</td><td>0</td></tr><tr><td>15</td><td>ETHMACTXEN</td><td>RW</td><td>以太网MAC发送时钟使能:1: 以太网MAC发送时钟开启;0: 以太网MAC发送时钟关闭。注:适用于CH32V30x_D8C、CH32F20x_D8C百兆千兆外置PHY。</td><td>0</td></tr><tr><td>14</td><td>ETHMACEN</td><td>RW</td><td>以太网MAC时钟使能:1: 以太网MAC时钟开启;0: 以太网MAC时钟关闭。注:适用于CH32V30x_D8C、CH32F20x_D8C百兆千兆外置PHY。</td><td>0</td></tr><tr><td>13</td><td>DVPEN</td><td>RW</td><td>DVP模块时钟使能位:1: 模块时钟开启;0: 模块时钟关闭。</td><td>0</td></tr><tr><td>12</td><td>OTGFSEN</td><td>RW</td><td>USBOTG_FS模块时钟使能位:1: 模块时钟开启;0: 模块时钟关闭。</td><td>0</td></tr><tr><td>11</td><td>USBHSEN</td><td>RW</td><td>USBHS模块时钟使能位:1: 模块时钟开启;0: 模块时钟关闭。</td><td>0</td></tr><tr><td>10</td><td>SDIOEN</td><td>RW</td><td>SDIO模块时钟使能位:1: 模块时钟开启;0: 模块时钟关闭。</td><td>0</td></tr><tr><td>9</td><td>RNGEN</td><td>RW</td><td>RNG模块时钟使能位:1: 模块时钟开启;0: 模块时钟关闭。</td><td>0</td></tr><tr><td>8</td><td>FSMCEN</td><td>RW</td><td>FSMCEN模块时钟使能位:1: 模块时钟开启; 0: 模块时钟关闭。</td><td>0</td></tr><tr><td>7</td><td>Reserved</td><td>RO</td><td>保留。</td><td>0</td></tr><tr><td>6</td><td>CRCEN</td><td>RW</td><td>CRC 模块时钟使能位:1: 模块时钟开启; 0: 模块时钟关闭。</td><td>0</td></tr><tr><td>[5:3]</td><td>Reserved</td><td>RO</td><td>保留。</td><td>0</td></tr><tr><td>2</td><td>SRAMEN</td><td>RW</td><td>SRAM 接口模块时钟使能位:1: 睡眠模式时, SRAM 接口模块时钟开启;0: 睡眠模式时, SRAM 接口模块时钟关闭。</td><td>1</td></tr><tr><td>1</td><td>DMA2EN</td><td>RW</td><td>DMA2 模块时钟使能位:1: 模块时钟开启; 0: 模块时钟关闭。</td><td>0</td></tr><tr><td>0</td><td>DMA1EN</td><td>RW</td><td>DMA1 模块时钟使能位:1: 模块时钟开启; 0: 模块时钟关闭。</td><td>0</td></tr></table>

注：当外设时钟没有启用时，软件不能读出外设寄存器数值，返回的数值始终为0。

### 3.4.7 APB2 外设时钟使能寄存器（RCC_APB2PCENR）

偏移地址：0x18

<table><tr><td colspan="11">Reserved</td><td>TIM10
EN</td><td>TIM9
EN</td><td colspan="4">Reserved</td></tr><tr><td>15</td><td>14</td><td>13</td><td>12</td><td>11</td><td>10</td><td>9</td><td>8</td><td>7</td><td>6</td><td>5</td><td>4</td><td>3</td><td>2</td><td>1</td><td>0</td><td></td></tr><tr><td>Reserved</td><td>USART1
EN</td><td>TIM8
EN</td><td>SPI1
EN</td><td>TIM1
EN</td><td>ADC2
EN</td><td>ADC1
EN</td><td>Reserved</td><td>IOPE
EN</td><td>IOPD
EN</td><td>IOPC
EN</td><td>IOPB
EN</td><td>IOPA
EN</td><td>Reser
ved</td><td>AFIO
EN</td><td></td><td></td></tr></table>

<table><tr><td>位</td><td>名称</td><td>访问</td><td>描述</td><td>复位值</td></tr><tr><td>[31:21]</td><td>Reserved</td><td>RO</td><td>保留。</td><td>0</td></tr><tr><td>20</td><td>TIM10EN</td><td>RW</td><td>TIM10接口时钟使能位:1:模块时钟开启; 0:模块时钟关闭。</td><td>0</td></tr><tr><td>19</td><td>TIM9EN</td><td>RW</td><td>TIM9接口时钟使能位:1:模块时钟开启; 0:模块时钟关闭。</td><td>0</td></tr><tr><td>[18:15]</td><td>Reserved</td><td>RO</td><td>保留。</td><td>0</td></tr><tr><td>14</td><td>USART1EN</td><td>RW</td><td>USART1接口时钟使能位:1:模块时钟开启; 0:模块时钟关闭。</td><td>0</td></tr><tr><td>13</td><td>TIM8EN</td><td>RW</td><td>TIM8模块时钟使能位:1:模块时钟开启; 0:模块时钟关闭。</td><td>0</td></tr><tr><td>12</td><td>SPI1EN</td><td>RW</td><td>SPI1接口时钟使能位:1:模块时钟开启; 0:模块时钟关闭。</td><td>0</td></tr><tr><td>11</td><td>TIM1EN</td><td>RW</td><td>TIM1模块时钟使能位:1:模块时钟开启; 0:模块时钟关闭。</td><td>0</td></tr><tr><td>10</td><td>ADC2EN</td><td>RW</td><td>ADC2模块时钟使能位:1:模块时钟开启; 0:模块时钟关闭。</td><td>0</td></tr><tr><td>9</td><td>ADC1EN</td><td>RW</td><td>ADC1模块时钟使能位:1:模块时钟开启; 0:模块时钟关闭。</td><td>0</td></tr><tr><td>[8:7]</td><td>Reserved</td><td>RO</td><td>保留。</td><td>0</td></tr><tr><td>6</td><td>IOPEEN</td><td>RW</td><td>IO的PE端口模块时钟使能位:1:模块时钟开启; 0:模块时钟关闭。</td><td>0</td></tr><tr><td>5</td><td>IOPDEN</td><td>RW</td><td>IO的PD端口模块时钟使能位:1:模块时钟开启; 0:模块时钟关闭。</td><td>0</td></tr><tr><td>4</td><td>IOPCEN</td><td>RW</td><td>IO的PC端口模块时钟使能位:1:模块时钟开启; 0:模块时钟关闭。</td><td>0</td></tr><tr><td>3</td><td>IOPBEN</td><td>RW</td><td>IO的PB端口模块时钟使能位:1:模块时钟开启; 0:模块时钟关闭。</td><td>0</td></tr><tr><td>2</td><td>IOPAEN</td><td>RW</td><td>IO的PA端口模块时钟使能位:1:模块时钟开启; 0:模块时钟关闭。</td><td>0</td></tr><tr><td>1</td><td>Reserved</td><td>RO</td><td>保留。</td><td>0</td></tr><tr><td>0</td><td>AFIOEN</td><td>RW</td><td>IO辅助功能模块时钟使能位:1:模块时钟开启; 0:模块时钟关闭。</td><td>0</td></tr></table>

注：当外设时钟没有启用时，软件不能读出外设寄存器数值，返回的数值始终为0。

### 3.4.8 APB1 外设时钟使能寄存器（RCC_APB1PCENR）

偏移地址：0x1C

<table><tr><td>31</td><td>30</td><td>29</td><td>28</td><td>27</td><td>26</td><td>25</td><td>24</td><td>23</td><td>22</td><td>21</td><td>20</td><td>19</td><td>18</td><td>17</td><td>16</td></tr><tr><td colspan="2">Reserved</td><td>DAC
EN</td><td>PWR
EN</td><td>BKP
EN</td><td>CAN2
EN</td><td>CAN1
EN</td><td>Reserved</td><td>USBD
EN</td><td>I2C2
EN</td><td>I2C1
EN</td><td>UART5
EN</td><td>UART4
EN</td><td>USART3
EN</td><td>USART2
EN</td><td>Reserved</td></tr><tr><td>15</td><td>14</td><td>13</td><td>12</td><td>11</td><td>10</td><td>9</td><td>8</td><td>7</td><td>6</td><td>5</td><td>4</td><td>3</td><td>2</td><td>1</td><td>0</td></tr><tr><td>SPI3
EN</td><td>SPI2
EN</td><td colspan="2">Reserved</td><td>WWDG
EN</td><td colspan="2">Reserved</td><td>UART8
EN</td><td>UART7
EN</td><td>UART6
EN</td><td>TIM7
EN</td><td>TIM6
EN</td><td>TIM5
EN</td><td>TIM4
EN</td><td>TIM3
EN</td><td>TIM2
EN</td></tr></table>

<table><tr><td>位</td><td>名称</td><td>访问</td><td>描述</td><td>复位值</td></tr><tr><td>[31:30]</td><td>Reserved</td><td>RO</td><td>保留。</td><td>0</td></tr><tr><td>29</td><td>DACEN</td><td>RW</td><td>DAC模块时钟使能位:1:模块时钟开启; 0:模块时钟关闭。</td><td>0</td></tr><tr><td>28</td><td>PWREN</td><td>RW</td><td>电源接口模块时钟使能位:1:模块时钟开启; 0:模块时钟关闭。</td><td>0</td></tr><tr><td>27</td><td>BKPEN</td><td>RW</td><td>后备单元时钟使能位:1:模块时钟开启; 0:模块时钟关闭。</td><td>0</td></tr><tr><td>26</td><td>CAN2EN</td><td>RW</td><td>CAN2模块时钟使能位:1:模块时钟开启; 0:模块时钟关闭。</td><td>0</td></tr><tr><td>25</td><td>CAN1EN</td><td>RW</td><td>CAN1模块时钟使能位:1:模块时钟开启; 0:模块时钟关闭。</td><td>0</td></tr><tr><td>24</td><td>Reserved</td><td>RO</td><td>保留。</td><td>0</td></tr><tr><td>23</td><td>USBDEN</td><td>RW</td><td>USBD模块时钟使能位:1:模块时钟开启; 0:模块时钟关闭。</td><td>0</td></tr><tr><td>22</td><td>I2C2EN</td><td>RW</td><td>I2C2接口时钟使能位:1:模块时钟开启; 0:模块时钟关闭。</td><td>0</td></tr><tr><td>21</td><td>I2C1EN</td><td>RW</td><td>I2C1接口时钟使能位:1:模块时钟开启; 0:模块时钟关闭。</td><td>0</td></tr><tr><td>20</td><td>UART5EN</td><td>RW</td><td>UART5接口时钟使能位:</td><td>0</td></tr><tr><td></td><td></td><td></td><td>1:模块时钟开启; 0:模块时钟关闭。</td><td></td></tr><tr><td>19</td><td>UART4EN</td><td>RW</td><td>UART4接口时钟使能位:1:模块时钟开启; 0:模块时钟关闭。</td><td>0</td></tr><tr><td>18</td><td>USART3EN</td><td>RW</td><td>USART3接口时钟使能位:1:模块时钟开启; 0:模块时钟关闭。</td><td>0</td></tr><tr><td>17</td><td>USART2EN</td><td>RW</td><td>USART2接口时钟使能位:1:模块时钟开启; 0:模块时钟关闭。</td><td>0</td></tr><tr><td>16</td><td>Reserved</td><td>RO</td><td>保留。</td><td>0</td></tr><tr><td>15</td><td>SPI3EN</td><td>RW</td><td>SPI3接口时钟使能位:1:模块时钟开启; 0:模块时钟关闭。</td><td>0</td></tr><tr><td>14</td><td>SPI2EN</td><td>RW</td><td>SPI2接口时钟使能位:1:模块时钟开启; 0:模块时钟关闭。</td><td>0</td></tr><tr><td>[13:12]</td><td>Reserved</td><td>RO</td><td>保留。</td><td>0</td></tr><tr><td>11</td><td>WWDGEN</td><td>RW</td><td>窗口看门狗时钟使能位:1:模块时钟开启; 0:模块时钟关闭。</td><td>0</td></tr><tr><td>[10:9]</td><td>Reserved</td><td>RO</td><td>保留。</td><td>0</td></tr><tr><td>8</td><td>UART8EN</td><td>RW</td><td>UART8使能位:1:模块时钟开启; 0:模块时钟关闭。</td><td>0</td></tr><tr><td>7</td><td>UART7EN</td><td>RW</td><td>UART7使能位:1:模块时钟开启; 0:模块时钟关闭。</td><td>0</td></tr><tr><td>6</td><td>UART6EN</td><td>RW</td><td>UART6使能位:1:模块时钟开启; 0:模块时钟关闭。</td><td>0</td></tr><tr><td>5</td><td>TIM7EN</td><td>RW</td><td>定时器7模块时钟使能位:1:模块时钟开启; 0:模块时钟关闭。</td><td>0</td></tr><tr><td>4</td><td>TIM6EN</td><td>RW</td><td>定时器6模块时钟使能位:1:模块时钟开启; 0:模块时钟关闭。</td><td>0</td></tr><tr><td>3</td><td>TIM5EN</td><td>RW</td><td>定时器5模块时钟使能位:1:模块时钟开启; 0:模块时钟关闭。</td><td>0</td></tr><tr><td>2</td><td>TIM4EN</td><td>RW</td><td>定时器4模块时钟使能位:1:模块时钟开启; 0:模块时钟关闭。</td><td>0</td></tr><tr><td>1</td><td>TIM3EN</td><td>RW</td><td>定时器3模块时钟使能位:1:模块时钟开启; 0:模块时钟关闭。</td><td>0</td></tr><tr><td>0</td><td>TIM2EN</td><td>RW</td><td>定时器2模块时钟使能位:1:模块时钟开启; 0:模块时钟关闭。</td><td>0</td></tr></table>

注：当外设时钟没有启用时，软件不能读出外设寄存器数值，返回的数值始终为0。

### 3.4.9 后备域控制寄存器（RCC_BDCTLR）

偏移地址： $_ { 0 \times 2 0 }$

<table><tr><td>31</td><td>30</td><td>29</td><td>28</td><td>27</td><td>26</td><td>25</td><td>24</td><td>23</td><td>22</td><td>21</td><td>20</td><td>19</td><td>18</td><td>17</td><td>16</td></tr><tr><td colspan="15">Reserved</td><td>BDRST</td></tr><tr><td>15</td><td>14</td><td>13</td><td>12</td><td>11</td><td>10</td><td>9</td><td>8</td><td>7</td><td>6</td><td>5</td><td>4</td><td>3</td><td>2</td><td>1</td><td>0</td></tr><tr><td>RTCEN</td><td colspan="3">Reserved</td><td colspan="3">RTCSEL[1:0]</td><td colspan="5">Reserved</td><td>LSE BYP</td><td>LSE RDY</td><td colspan="2">LSEON</td></tr></table>

<table><tr><td>位</td><td>名称</td><td>访问</td><td>描述</td><td>复位值</td></tr><tr><td>[31:17]</td><td>Reserved</td><td>R0</td><td>保留。</td><td>0</td></tr><tr><td>16</td><td>BDRST</td><td>RW</td><td>后备域软件复位控制:1:复位整个后备域。0:撤销复位。</td><td>0</td></tr><tr><td>15</td><td>RTCEN</td><td>RW</td><td>RTC时钟使能控制:1:使能RTC时钟;0:关闭RTC时钟。注:RTCSEL!=0的条件下才可以使能RTC时钟,否则硬件强制为0。</td><td>0</td></tr><tr><td>[14:10]</td><td>Reserved</td><td>R0</td><td>保留。</td><td>0</td></tr><tr><td>[9:8]</td><td>RTCSEL[1:0]</td><td>RW</td><td>RTC时钟源选择:00:无时钟;01:LSE振荡器作为RTC时钟;10:LSI振荡器作为RTC时钟;11:HSE振荡器经128分频后作为RTC时钟。注:一旦RTC时钟源被选定(RTCEN=1),直到下次后备域被复位,它不能再被改变。可通过设置BDRST位来恢复默认。</td><td>0</td></tr><tr><td>[7:3]</td><td>Reserved</td><td>R0</td><td>保留。</td><td>0</td></tr><tr><td>2</td><td>LSEBYP</td><td>RW</td><td>外部低速晶体(LSE)旁路控制位:1:旁路外部低速晶体/陶瓷谐振器(使用外部时钟源);0:不旁路低速外部晶体/陶瓷谐振器。注:此位需在LSEON为0下写入。</td><td>0</td></tr><tr><td>1</td><td>LSERDY</td><td>RO</td><td>外部低速晶体振荡稳定就绪标志位(由硬件置位):1:外部低速晶体振荡稳定;0:外部低速晶体振荡没有稳定。注:在LSEON位清0后,该位需要6个LSE周期清0。</td><td>0</td></tr><tr><td>0</td><td>LSEON</td><td>RW</td><td>外部低速晶体振荡使能控制位:1:使能LSE振荡器;0:关闭LSE振荡器。</td><td>0</td></tr></table>

注：后备域控制寄存器中(RCC_BDCTLR)的LSEON、LSEBYP、RTCSEL和RTCEN位处于后备域。因此，这些位在复位后处于写保护状态，只有在电源控制寄存器(PWR_CR)中的DBP位置1后，才能对这些位进行改动。这些位只能由后备域复位清除。任何内部或外部复位都不会影响这些位。

### 3.4.10 控制/状态寄存器（RCC_RSTSCKR）

偏移地址： ${ 0 } { \times 2 4 }$

<table><tr><td>31</td><td>30</td><td>29</td><td>28</td><td>27</td><td>26</td><td>25</td><td>24</td><td>23</td><td>22</td><td>21</td><td>20</td><td>19</td><td>18</td><td>17</td><td>16</td></tr><tr><td>LPWR
RSTF</td><td>WWDG
RSTF</td><td>IWDG
RSTF</td><td>SFT
RSTF</td><td>POR
RSTF</td><td>PIN
RSTF</td><td>Reser
ved</td><td>RMVF</td><td colspan="8">Reserved</td></tr><tr><td>15</td><td>14</td><td>13</td><td>12</td><td>11</td><td>10</td><td>9</td><td>8</td><td>7</td><td>6</td><td>5</td><td>4</td><td>3</td><td>2</td><td>1</td><td>0</td></tr><tr><td colspan="14">Reserved</td><td>LSI
RDY</td><td>LSION</td></tr></table>

<table><tr><td>位</td><td>名称</td><td>访问</td><td>描述</td><td>复位值</td></tr><tr><td>31</td><td>LPWRSTF</td><td>RW</td><td>低功耗复位标志:1:发生低功耗复位;0:无低功耗复位发生。发生低功耗管理复位时由硬件置1;软件写RMVF位清除。</td><td>0</td></tr><tr><td>30</td><td>WWDGRSTF</td><td>RW</td><td>窗口看门狗复位标志:1:发生窗口看门狗复位;0:无窗口看门狗复位发生。发生窗口看门狗复位时由硬件置1;软件写RMVF位清除。</td><td>0</td></tr><tr><td>29</td><td>IWDGRSTF</td><td>RW</td><td>独立看门狗复位标志:1:发生独立看门狗复位;0:无独立看门狗复位发生。发生独立看门狗复位时由硬件置1;软件写RMVF位清除。</td><td>0</td></tr><tr><td>28</td><td>SFTRSTF</td><td>RW</td><td>软件复位标志:1:发生软件复位;0:无软件复位发生。发生软件复位时由硬件置1;软件写RMVF位清除。</td><td>0</td></tr><tr><td>27</td><td>PORRSTF</td><td>RW</td><td>上电/掉电复位标志:1:发生上电/掉电复位;0:无上电/掉电复位发生。发生上电/掉电复位时由硬件置1;软件写RMVF位清除。</td><td>1</td></tr><tr><td>26</td><td>PINRSTF</td><td>RW</td><td>外部手动复位(NRST引脚)标志:1:发生NRST引脚复位;0:无NRST引脚复位发生。在NRST引脚复位发生时由硬件置1;软件写RMVF位清除。</td><td>0</td></tr><tr><td>25</td><td>Reserved</td><td>RO</td><td>保留。</td><td>0</td></tr><tr><td>24</td><td>RMVF</td><td>RW</td><td>清除复位标志控制:1:清除复位标志;0:无作用。</td><td>0</td></tr><tr><td>[23:2]</td><td>Reserved</td><td>RO</td><td>保留。</td><td>0</td></tr><tr><td>1</td><td>LSIRDY</td><td>RO</td><td>内部低速时钟(LSI)稳定就绪标志位(由硬件置位):1:内部低速时钟(40KHz)稳定;0:内部低速时钟(40KHz)没有稳定。注:在LSION位清0后,该位需要3个LSI周期清0。</td><td>0</td></tr><tr><td>0</td><td>LSION</td><td>RW</td><td>内部低速时钟(LSI)使能控制位:1:使能LSI(40KHz)振荡器;0:关闭LSI(40KHz)振荡器。</td><td>0</td></tr></table>

注：除复位标志只能由上电复位清除，其他由系统复位清除。

### 3.4.11 AHB 外设复位寄存器（RCC_AHBRSTR）

偏移地址： $_ { 0 \times 2 8 }$

<table><tr><td>31</td><td>30</td><td>29</td><td>28</td><td>27</td><td>26</td><td>25</td><td>24</td><td>23</td><td>22</td><td>21</td><td>20</td><td>19</td><td>18</td><td>17</td><td>16</td></tr><tr><td colspan="16">Reserved</td></tr><tr><td>15</td><td>14</td><td>13</td><td>12</td><td>11</td><td>10</td><td>9</td><td>8</td><td>7</td><td>6</td><td>5</td><td>4</td><td>3</td><td>2</td><td>1</td><td>0</td></tr><tr><td>Reserved</td><td>ETHMARCST</td><td>DVPRST</td><td>OTGFSRST</td><td colspan="12">Reserved</td></tr></table>

<table><tr><td>位</td><td>名称</td><td>访问</td><td>描述</td><td>复位值</td></tr><tr><td>[31:15]</td><td>Reserved</td><td>R0</td><td>保留。</td><td>0</td></tr><tr><td>14</td><td>ETHMACRST</td><td>RW</td><td>以太网 MAC 复位控制:1: 复位模块; 0: 无作用。</td><td>0</td></tr><tr><td>13</td><td>DVPRST</td><td>R0</td><td>DVP 复位控制:1: 复位模块; 0: 无作用。</td><td>0</td></tr><tr><td>12</td><td>OTGFSRST</td><td>RW</td><td>USBOTG_FS 模块复位控制:1: 复位模块; 0: 无作用。</td><td>0</td></tr><tr><td>[11:0]</td><td>Reserved</td><td>R0</td><td>保留。</td><td>0</td></tr></table>

### 3.4.12 时钟配置寄存器 2（RCC_CFGR2）

偏移地址： $\mathtt { 0 } \mathtt { x } 2 0$

<table><tr><td>31</td><td>30</td><td>29</td><td>28</td><td>27</td><td>26</td><td>25</td><td>24</td><td>23</td><td>22</td><td>21</td><td>20</td><td>19</td><td>18</td><td>17</td><td>16</td></tr><tr><td>USBHS
SRC</td><td>USBHS
PLL</td><td>USBHSCLK
[1:0]</td><td>USBHS
PLLSR
C</td><td colspan="2">USBHSDIV[2:0]</td><td>Reser
ved</td><td>ETH1G
EN</td><td colspan="2">ETH1GSRC
[1:0]</td><td>RNGSR
C</td><td>I2S3S
RC</td><td>I2S2S
RC</td><td colspan="2">PREDI
V1SRC</td><td></td></tr><tr><td>15</td><td>14</td><td>13</td><td>12</td><td>11</td><td>10</td><td>9</td><td>8</td><td>7</td><td>6</td><td>5</td><td>4</td><td>3</td><td>2</td><td>1</td><td>0</td></tr><tr><td colspan="4">PLL3MUL[3:0]</td><td colspan="4">PLL2MUL[3:0]</td><td colspan="4">PREDIV2[3:0]</td><td colspan="4">PREDIV1[3:0]</td></tr></table>

<table><tr><td>位</td><td>名称</td><td>访问</td><td>描述</td><td>复位值</td></tr><tr><td>31</td><td>USBHSSRC</td><td>RW</td><td>USBHS 48MHz时钟源选择:1:USB PHY 0:PLL CLK</td><td>0</td></tr><tr><td>30</td><td>USBHSPLL</td><td>RW</td><td>USBHS PHY 内部PLL 控制位:1:USB PHY 内部PLL 使能0:USB PHY 内部PLL 关闭</td><td>0</td></tr><tr><td>[29:28]</td><td>USBHSCLK[1:0]</td><td>RW</td><td>USBHS PLL 参考时钟频率选择(USBHSPLLSRC/USBHSDIV):00:3MHz01:4MHz10:8MHz11:5MHz</td><td>00b</td></tr><tr><td>27</td><td>USBHSPLLSRC</td><td>RW</td><td>USBHS PLL 参考源选择:1:HSI 0:HSE</td><td>0</td></tr><tr><td>[26:24]</td><td>USBHSDIV[2:0]</td><td>RW</td><td>USBHS PLL 参考源分频:000:1分频 001:2分频</td><td>000b</td></tr><tr><td></td><td></td><td></td><td>010:3分频 011:4分频100:5分频 101:6分频110:7分频 111:8分频</td><td></td></tr><tr><td>23</td><td>Reserved</td><td>R0</td><td>保留。</td><td>0</td></tr><tr><td>22</td><td>ETH1GEN</td><td>RW</td><td>千兆以太网125M时钟控制位:1:开启 0:关闭</td><td>0</td></tr><tr><td>[21:20]</td><td>ETH1GSRC[1:0]</td><td>RW</td><td>千兆以太网125M时钟选择:00:PLL2 VCO01:PLL3 VCO1x:外部PB1引脚输入</td><td>00b</td></tr><tr><td>19</td><td>RNGSRC</td><td>RW</td><td>RNG时钟源选择:1:PLL3 VCO 0:系统时钟</td><td>0</td></tr><tr><td>18</td><td>I2S3SRC</td><td>RW</td><td>I2S3时钟源:1:PLL3 VCO;0:系统时钟(SYSCLK)。</td><td>0</td></tr><tr><td>17</td><td>I2S2SRC</td><td>RW</td><td>I2S2时钟源:1:PLL3 VCO;0:系统时钟(SYSCLK)。</td><td>0</td></tr><tr><td>16</td><td>PREDIV1SRC</td><td>RW</td><td>PREDIV1时钟源:1:PLL2;0:HSE。</td><td>0</td></tr><tr><td>[15:12]</td><td>PLL3MUL[3:0]</td><td>RW</td><td>PLL3倍频因子(在PLL3关闭才可写入)。0000:PLL3 2.5倍频输出 0001:PLL3 12.5倍频输出;0010:PLL3 4倍频输出;0011:PLL3 5倍频输出;0100:PLL3 6倍频输出;0101:PLL3 7倍频输出;0110:PLL3 8倍频输出;0111:PLL3 9倍频输出;1000:PLL3 10倍频输出;1001:PLL3 11倍频输出;1010:PLL3 12倍频输出;1011:PLL3 13倍频输出;1100:PLL3 14倍频输出;1101:PLL3 15倍频输出;1110:PLL3 16倍频输出;1111:PLL3 20倍频输出</td><td>0000b</td></tr><tr><td>[11:8]</td><td>PLL2MUL[3:0]</td><td>RW</td><td>PLL2倍频因子(在PLL2关闭才可写入)。0000:PLL2 2.5倍频输出 0001:PLL2 12.5倍频输出;0010:PLL2 4倍频输出;0011:PLL2 5倍频输出;0100:PLL2 6倍频输出;0101:PLL2 7倍频输出;0110:PLL2 8倍频输出;0111:PLL2 9倍频输出;1000:PLL2 10倍频输出;1001:PLL2 11倍频输出;1010:PLL2 12倍频输出;1011:PLL2 13倍频输出;1100:PLL2 14倍频输出;1101:PLL2 15倍频输出;1110:PLL2 16倍频输出;1111:PLL2 20倍频输出</td><td>0000b</td></tr><tr><td>[7:4]</td><td>PREDIV2[3:0]</td><td>RW</td><td>PREDIV2分频因子(在PLL2和PLL3关闭才可写入)0000:PREDIV2不对输入时钟分频;0001:PREDIV2对输入时钟2分频;0010:PREDIV2对输入时钟3分频;0011:PREDIV2对输入时钟4分频;0100:PREDIV2对输入时钟5分频;0101:PREDIV2对输入时钟6分频;</td><td>0000b</td></tr><tr><td></td><td></td><td></td><td>0110: PREDIV2 对输入时钟 7 分频;0111: PREDIV2 对输入时钟 8 分频;1000: PREDIV2 对输入时钟 9 分频;1001: PREDIV2 对输入时钟 10 分频;1010: PREDIV2 对输入时钟 11 分频;1011: PREDIV2 对输入时钟 12 分频;1100: PREDIV2 对输入时钟 13 分频;1101: PREDIV2 对输入时钟 14 分频;1110: PREDIV2 对输入时钟 15 分频;1111: PREDIV2 对输入时钟 16 分频;</td><td></td></tr><tr><td>[3:0]</td><td>PREDIV1 [3:0]</td><td>RW</td><td>PREDIV1 分频因子(在 PLL 关闭才可写入)0000: PREDIV1 不对输入时钟分频;0001: PREDIV1 对输入时钟 2 分频;0010: PREDIV1 对输入时钟 3 分频;0011: PREDIV1 对输入时钟 4 分频;0100: PREDIV1 对输入时钟 5 分频;0101: PREDIV1 对输入时钟 6 分频;0110: PREDIV1 对输入时钟 7 分频;0111: PREDIV1 对输入时钟 8 分频;1000: PREDIV1 对输入时钟 9 分频;1001: PREDIV1 对输入时钟 10 分频;1010: PREDIV1 对输入时钟 11 分频;1011: PREDIV1 对输入时钟 12 分频;1100: PREDIV1 对输入时钟 13 分频;1101: PREDIV1 对输入时钟 14 分频;1110: PREDIV1 对输入时钟 15 分频;1111: PREDIV1 对输入时钟 16 分频;注:bit0 和 RCC_CFGRO 的 bit17 相同,修改 RCC_CFGRO 的 bit17 会同时改变该寄存器的 bit0。</td><td>0000b</td></tr></table>

注：适用于CH32F20x_D8C、CH32V30x_D8C。

### 3.4.13 外部晶振校准控制寄存器（HSE_CAL_CTRL）

偏移地址： $0 \times 0 0$   

<table><tr><td>31</td><td>30</td><td>29</td><td>28</td><td>27</td><td>26</td><td>25</td><td>24</td><td>23</td><td>22</td><td>21</td><td>20</td><td>19</td><td>18</td><td>17</td><td>16</td></tr><tr><td colspan="4">HSEC[3:0]</td><td>HSE
FAULT</td><td>Reser
ved</td><td colspan="2">HSEITRIM
[1:0]</td><td colspan="8">Reserved</td></tr><tr><td>15</td><td>14</td><td>13</td><td>12</td><td>11</td><td>10</td><td>9</td><td>8</td><td>7</td><td>6</td><td>5</td><td>4</td><td>3</td><td>2</td><td>1</td><td>0</td></tr><tr><td colspan="16">Reserved</td></tr></table>

<table><tr><td>位</td><td>名称</td><td>访问</td><td>描述</td><td>复位值</td></tr><tr><td>[31:28]</td><td>HSEC[3:0]</td><td>RW</td><td>HSE内置匹配电容调节位:111:22pF110:20pF101:18pF</td><td>000b</td></tr><tr><td></td><td></td><td></td><td>100:16pF011:14pF010:12pF001:10pF000:8pF</td><td></td></tr><tr><td>27</td><td>HSEFAULT</td><td>RW</td><td>HSE 失效检测禁用控制位:1: 忽略模拟输入的 HSE 失效检测信号0: 使用模拟输入的 HSE 失效检测信号</td><td>0</td></tr><tr><td>26</td><td>Reserved</td><td>RO</td><td>保留。</td><td>0</td></tr><tr><td>[25:24]</td><td>HSEITRIM[1:0]</td><td>RW</td><td>HSE 起振电流调节位。</td><td>01b</td></tr><tr><td>[23:0]</td><td>Reserved</td><td>RO</td><td>保留。</td><td>0</td></tr></table>

### 3.4.14 内部低速晶振校准调节寄存器（LSI32K_TUNE）

偏移地址： $0 \times 0 \mathsf { A }$

<table><tr><td>Reserved</td><td>HTUNE[7:0]</td><td>LTUNE[4:0]</td></tr></table>

<table><tr><td>位</td><td>名称</td><td>访问</td><td>描述</td><td>复位值</td></tr><tr><td>[15:13]</td><td>Reserved</td><td>RO</td><td>保留。</td><td>0</td></tr><tr><td>[12:5]</td><td>HTUNE[7:0]</td><td>RW</td><td>LSI32K细调配置位</td><td>80h</td></tr><tr><td>[4:0]</td><td>LTUNE[4:0]</td><td>RW</td><td>LSI32K粗调配置位</td><td>11h</td></tr></table>

### 3.4.15 内部低速晶振校准配置寄存器（LSI32K_CAL_CFG）

偏移地址： ${ 0 } \times 1 { 0 }$

<table><tr><td>7</td><td>6</td><td>5</td><td>4</td><td>3</td><td>2</td><td>1</td><td>0</td></tr><tr><td>Reserved</td><td>LPEN</td><td>WKUPEN</td><td>HALTMD</td><td colspan="4">CNTVLU[3:0]</td></tr></table>

<table><tr><td>位</td><td>名称</td><td>访问</td><td>描述</td><td>复位值</td></tr><tr><td>7</td><td>Reserved</td><td>RO</td><td>保留。</td><td>0</td></tr><tr><td>6</td><td>LPEN</td><td>RW</td><td>低功耗模式下校准使能位:0:在低功耗模式下关闭校准功能1:在低功耗模式下使能校准功能注:此功能必须要配合EXTEN中的RB_HSE_KEEP_LP使用</td><td>0</td></tr><tr><td>5</td><td>WKUPEN</td><td>RW</td><td>LS132K唤醒中断使能:0:禁止唤醒中断1:使能唤醒中断</td><td>0</td></tr><tr><td>4</td><td>HALTMD</td><td>RW</td><td>LS132K校准计数暂停时长配置位:0:计数暂停维持1个CK32K周期1:计数暂停维持3个CK32K周期</td><td>0</td></tr><tr><td>[3:0]</td><td>CNTVLU[3:0]</td><td>RW</td><td>LS132K校准采样时长配置位:0000:2个CK32K周期0001:4个CK32K周期0010:32个CK32K周期</td><td>0001b</td></tr><tr><td></td><td></td><td></td><td>0011:64个CK32K周期0100:128个CK32K周期0101:256个CK32K周期0110:512个CK32K周期0111:1024个CK32K周期1000:1088个CK32K周期1001:1152个CK32K周期1010:1216个CK32K周期1011:1280个CK32K周期1100:2000个CK32K周期注:其他的配置值对应2个CK32K周期</td><td></td></tr></table>

### 3.4.16 内部低速晶振校准状态寄存器（LSI32K_CAL_STATR）

偏移地址： $_ { 0 \times 2 0 }$

<table><tr><td>15</td><td>14</td><td>13</td><td>12</td><td>11</td><td>10</td><td>9</td><td>8</td><td>7</td><td>6</td><td>5</td><td>4</td><td>3</td><td>2</td><td>1</td><td>0</td></tr><tr><td>IFEND</td><td>CNTOV</td><td colspan="14">CNT[13:0]</td></tr></table>

<table><tr><td>位</td><td>名称</td><td>访问</td><td>描述</td><td>复位值</td></tr><tr><td>15</td><td>IFEND</td><td>RW1</td><td>LS132K校准计数结束中断标志位:0:采样计数中,无标志1:采样计数结束,标志置位</td><td>0</td></tr><tr><td>14</td><td>CNTOV</td><td>RW1</td><td>LS132K采样计数器溢出标志位:0:未发生溢出1:发生溢出</td><td>0</td></tr><tr><td>[13:0]</td><td>CNT[13:0]</td><td>R0</td><td>对若干个CK32K周期基于系统主频的计数值。注:具体的CK32K周期数可配置。</td><td>0</td></tr></table>

### 3.4.17 内部低速晶振校准次数计数器（LSI32K_CAL_OV_CNT）

偏移地址： ${ 0 } \times 2 2$

<table><tr><td>7</td><td>6</td><td>5</td><td>4</td><td>3</td><td>2</td><td>1</td><td>0</td></tr><tr><td colspan="8">OVCNT[7:0]</td></tr></table>

<table><tr><td>位</td><td>名称</td><td>访问</td><td>描述</td><td>复位值</td></tr><tr><td>[7:0]</td><td>OVCNT[7:0]</td><td>R0</td><td>LS132K采样计数器溢出次数。
注:清除溢出标志操作会清除此计数器。</td><td>0</td></tr></table>

### 3.4.18 内部低速晶振校准控制寄存器（LSI32K_CAL_CTRL）

偏移地址： ${ 0 } \times { 2 } { 3 }$

<table><tr><td>7</td><td>6</td><td>5</td><td>4</td><td>3</td><td>2</td><td>1</td><td>0</td></tr><tr><td>HALT</td><td colspan="5">Reserved</td><td>CALEN</td><td>CAL INTEN</td></tr></table>

<table><tr><td>位</td><td>名称</td><td>访问</td><td>描述</td><td>复位值</td></tr><tr><td>7</td><td>HALT</td><td>R0</td><td>LS132K校准计数状态位:</td><td>1</td></tr><tr><td></td><td></td><td></td><td>0:计数中,计数值不可用
1:计数暂停,可获取计数值</td><td></td></tr><tr><td>[6:2]</td><td>Reserved</td><td>R0</td><td>保留。</td><td>0</td></tr><tr><td>1</td><td>CALEN</td><td>RW</td><td>LS132K校准使能</td><td>0</td></tr><tr><td>0</td><td>CAL INTEN</td><td>RW</td><td>LS132K校准中断使能</td><td>0</td></tr></table>

注：适用于CH32F20x_D8C、CH32V30×_D8C。

# 第 4 章 后备寄存器（BKP）

本章模块描述适用于CH32F2x、 $_ { C H 3 2 V 2 x }$ 和 $_ { C H 3 2 V 3 x }$ 微控制器全系列产品。

后备寄存器（BKP）提供了最大42个16 位的后备数据寄存器，最大可以用来存储 84字节的用户数据。在主电源（ $( \mathsf { V } _ { \mathsf { D } \mathsf { D } } )$ ）掉电后，这些数据仍可以由 $V _ { \mathsf { B A T } }$ 供电而保持，不受待机状态、系统复位或电源复位的影响。此外BKP单元还提供了侵入检测管理、RTC时钟校准及脉冲输出功能。

## 4.1 主要特征

$\bullet$ 侵入检测（TAMPER）功能  
$\bullet$ RTC时钟校准功能  
在 PC13 引脚上输出 RTC 时钟 64 分频，闹钟脉冲或者秒脉冲

## 4.2 功能说明

微控制器复位后对后备寄存器和 RTC 的访问被禁止，需通过以下操作开启对后备寄存器的访问：

1）置寄存器RCC_APB1PCENR的PWREN位和 BKPEN位来打开电源和后备接口的操作时钟；  
2）置电源控制寄存器 PWR_CTLR 的 DBP 位，使能对后备寄存器和 RTC 寄存器的访问。

### 4.2.1 后备数据寄存器

后备数据寄存器可以作为通用数据缓存使用，由于其在 $\mathsf { V } _ { \mathsf { D } \mathsf { D } }$ 掉电下靠 $V _ { \mathsf { B A T } }$ 电源保存数据的特性，可以用来存一些重要的或敏感的数据。但这些数据在产生侵入事件后会被全部清除。

### 4.2.2 侵入检测

侵入检测就是当外界提供了一个信号（上升沿或下降沿）时，表示有“侵入事件”，硬件将自动清除当前系统中保留的重要信息。这种方式可以增加系统信息的安全性。

当侵入检测引脚上出现跳变沿（取决于 TPAL 位）时会产生一个侵入事件，如果使能了侵入检测中断，还会同时产生一个侵入检测中断。只要出现了侵入事件，后备数据寄存器就会被全部清除。此外，硬件检测采用记忆方式，即使侵入检测功能未开启（ $\mathsf { T P E } = 0$ ），系统也会采样是否有跳变沿，并在满足 TPAL 位选择情况下，提前锁定侵入事件，并在 TPE 位置 1 下，触发侵入事件。

例如：当 TPAL $\mathtt { = 0 }$ 时，如果TPE=0未开启功能，但 TAMPER 引脚已经为高电平，一旦 ${ \mathsf { T P E } } { = } 1$ 后，则会产生一个额外的侵入事件（系统提前锁定了上升沿）。当 TPAL=1 时，如果 $\mathtt { T P E = 0 }$ 未开启功能，但TAMPER 引脚已经为低电平，一旦 ${ \mathsf { T P E } } { = } 1$ 后，则会产生一个额外的侵入事件（系统提前锁定了下降沿）。

所以为了防止发生不必要的侵入事件，导致清除了后备寄存器，建议：在希望硬件检测侵入引脚的开始时刻，通过写 BKP_TPCSR 寄存器 CTE 位置 1，先清除硬件可能记忆过的侵入事件，并确保当前侵入检测引脚状态是无效的。

注：当 $V _ { D D }$ 电源断开时，侵入检测功能仍然有效。为了避免不必要的复位数据后备寄存器，TAMPER引脚应该在片外连接到正确的电平。

### 4.2.3 RTC 校准

此功能必须配置侵入检测引脚作为普通 IO口使用。配置 BKP_TPCTLR 寄存器TPE位清 0。

脉冲输出

配置 BKP_OCTLR 寄存器的 ASOE 位，开启 RTC 脉冲输出，设置 ASOS 位，选择秒脉冲输出还是闹钟脉冲输出。

RTC 校准

配置BKP_OCTLR寄存器的CCO位后，内部的RTC时钟将经过64分频后输出到侵入检测引脚（TAMPER）

上。通过实际测试，软件配合修改 CAL[6:0]位来调整时钟对 RTC 进行校准。

### 4.2.4 BKP 接口复位

BKP 区域可以在 $\mathsf { V } _ { \mathsf { D } \mathsf { D } }$ 主电源掉电下，由 $V _ { \mathsf { B A T } }$ 独立供电。应用代码控制 BKP区域寄存器复位中，后备数据寄存器 BKP_DATAR1-10、ASOS 位、ASOE 位在软件配置 RCC_BDCTLR 寄存器的 BDRST 位下复位，不受 RCC 外设接口控制 BKPRST 位影响。

## 4.3 寄存器描述

表 4-1 BKP 相关寄存器列表  

<table><tr><td>名称</td><td>访问地址</td><td>描述</td><td>复位值</td></tr><tr><td>R16_BKP_DATAR1</td><td>0x40006C04</td><td>后备数据寄存器1</td><td>0x0000</td></tr><tr><td>R16_BKP_DATAR2</td><td>0x40006C08</td><td>后备数据寄存器2</td><td>0x0000</td></tr><tr><td>R16_BKP_DATAR3</td><td>0x40006C0C</td><td>后备数据寄存器3</td><td>0x0000</td></tr><tr><td>R16_BKP_DATAR4</td><td>0x40006C10</td><td>后备数据寄存器4</td><td>0x0000</td></tr><tr><td>R16_BKP_DATAR5</td><td>0x40006C14</td><td>后备数据寄存器5</td><td>0x0000</td></tr><tr><td>R16_BKP_DATAR6</td><td>0x40006C18</td><td>后备数据寄存器6</td><td>0x0000</td></tr><tr><td>R16_BKP_DATAR7</td><td>0x40006C1C</td><td>后备数据寄存器7</td><td>0x0000</td></tr><tr><td>R16_BKP_DATAR8</td><td>0x40006C20</td><td>后备数据寄存器8</td><td>0x0000</td></tr><tr><td>R16_BKP_DATAR9</td><td>0x40006C24</td><td>后备数据寄存器9</td><td>0x0000</td></tr><tr><td>R16_BKP_DATAR10</td><td>0x40006C28</td><td>后备数据寄存器10</td><td>0x0000</td></tr><tr><td>R16_BKP_OCTLR</td><td>0x40006C2C</td><td>RTC校准寄存器</td><td>0x0000</td></tr><tr><td>R16_BKP_TPCTLR</td><td>0x40006C30</td><td>侵入检测控制寄存器</td><td>0x0000</td></tr><tr><td>R16_BKPTPCSR</td><td>0x40006C34</td><td>侵入检测状态寄存器</td><td>0x0000</td></tr><tr><td>R16_BKP_DATAR11</td><td>0x40006C40</td><td>后备数据寄存器11</td><td>0x0000</td></tr><tr><td>R16_BKP_DATAR12</td><td>0x40006C44</td><td>后备数据寄存器12</td><td>0x0000</td></tr><tr><td>R16_BKP_DATAR13</td><td>0x40006C48</td><td>后备数据寄存器13</td><td>0x0000</td></tr><tr><td>R16_BKP_DATAR14</td><td>0x40006C4C</td><td>后备数据寄存器14</td><td>0x0000</td></tr><tr><td>R16_BKP_DATAR15</td><td>0x40006C50</td><td>后备数据寄存器15</td><td>0x0000</td></tr><tr><td>R16_BKP_DATAR16</td><td>0x40006C54</td><td>后备数据寄存器16</td><td>0x0000</td></tr><tr><td>R16_BKP_DATAR17</td><td>0x40006C58</td><td>后备数据寄存器17</td><td>0x0000</td></tr><tr><td>R16_BKP_DATAR18</td><td>0x40006C5C</td><td>后备数据寄存器18</td><td>0x0000</td></tr><tr><td>R16_BKP_DATAR19</td><td>0x40006C60</td><td>后备数据寄存器19</td><td>0x0000</td></tr><tr><td>R16_BKP_DATAR20</td><td>0x40006C64</td><td>后备数据寄存器20</td><td>0x0000</td></tr><tr><td>R16_BKP_DATAR21</td><td>0x40006C68</td><td>后备数据寄存器21</td><td>0x0000</td></tr><tr><td>R16_BKP_DATAR22</td><td>0x40006C6C</td><td>后备数据寄存器22</td><td>0x0000</td></tr><tr><td>R16_BKP_DATAR23</td><td>0x40006C70</td><td>后备数据寄存器23</td><td>0x0000</td></tr><tr><td>R16_BKP_DATAR24</td><td>0x40006C74</td><td>后备数据寄存器24</td><td>0x0000</td></tr><tr><td>R16_BKP_DATAR25</td><td>0x40006C78</td><td>后备数据寄存器25</td><td>0x0000</td></tr><tr><td>R16_BKP_DATAR26</td><td>0x40006C7C</td><td>后备数据寄存器26</td><td>0x0000</td></tr><tr><td>R16_BKP_DATAR27</td><td>0x40006C80</td><td>后备数据寄存器27</td><td>0x0000</td></tr><tr><td>R16_BKP_DATAR28</td><td>0x40006C84</td><td>后备数据寄存器28</td><td>0x0000</td></tr><tr><td>R16_BKP_DATAR29</td><td>0x40006C88</td><td>后备数据寄存器29</td><td>0x0000</td></tr><tr><td>R16_BKP_DATAR30</td><td>0x40006C8C</td><td>后备数据寄存器30</td><td>0x0000</td></tr><tr><td>R16_BKP_DATAR31</td><td>0x40006C90</td><td>后备数据寄存器31</td><td>0x0000</td></tr><tr><td>R16_BKP_DATAR32</td><td>0x40006C94</td><td>后备数据寄存器32</td><td>0x0000</td></tr><tr><td>R16_BKP_DATAR33</td><td>0x40006C98</td><td>后备数据寄存器33</td><td>0x0000</td></tr><tr><td>R16_BKP_DATAR34</td><td>0x40006C9C</td><td>后备数据寄存器34</td><td>0x0000</td></tr><tr><td>R16_BKP_DATAR35</td><td>0x40006CA0</td><td>后备数据寄存器35</td><td>0x0000</td></tr><tr><td>R16_BKP_DATAR36</td><td>0x40006CA4</td><td>后备数据寄存器36</td><td>0x0000</td></tr><tr><td>R16_BKP_DATAR37</td><td>0x40006CA8</td><td>后备数据寄存器37</td><td>0x0000</td></tr><tr><td>R16_BKP_DATAR38</td><td>0x40006CAC</td><td>后备数据寄存器38</td><td>0x0000</td></tr><tr><td>R16_BKP_DATAR39</td><td>0x40006CB0</td><td>后备数据寄存器39</td><td>0x0000</td></tr><tr><td>R16_BKP_DATAR40</td><td>0x40006CB4</td><td>后备数据寄存器40</td><td>0x0000</td></tr><tr><td>R16_BKP_DATAR41</td><td>0x40006CB8</td><td>后备数据寄存器41</td><td>0x0000</td></tr><tr><td>R16_BKP_DATAR42</td><td>0x40006CBC</td><td>后备数据寄存器42</td><td>0x0000</td></tr></table>

注：后备数据寄存器（BKP_DATARx）（x=11-42）适用于CH32F20x_D8、CH32F20x_D8C、CH32F20×_D8W、CH32V20x_D8、CH32V20x_D8W、CH32V30x_D8、CH32V30x_D8C。

### 4.3.1 后备数据寄存器（BKP_DATARx）（ $\mathbf { x } = 1 - 4 2 )$

偏移地址： $\phantom { - } 0 { \times } 0 4 \ – 0 { \times } 2 8$ ， $0 { \times } 4 0 { - } 0 { \times } \mathsf { B C }$

<table><tr><td>15</td><td>14</td><td>13</td><td>12</td><td>11</td><td>10</td><td>9</td><td>8</td><td>7</td><td>6</td><td>5</td><td>4</td><td>3</td><td>2</td><td>1</td><td>0</td></tr><tr><td colspan="16">D[15:0]</td></tr></table>

<table><tr><td>位</td><td>名称</td><td>访问</td><td>描述</td><td>复位值</td></tr><tr><td>[15:0]</td><td>D[15:0]</td><td>RW</td><td>后备数据，可以被用户程序调用。
注：它们仅由后备域复位来复位（BDRST）或（如果侵入检测引脚TAMPER功能被开启时）由侵入引脚事件复位。</td><td>0</td></tr></table>

### 4.3.2 RTC 校准寄存器（BKP_OCTLR）

偏移地址： $\mathtt { 0 } \mathtt { x } 2 0$

<table><tr><td>15</td><td>14</td><td>13</td><td>12</td><td>11</td><td>10</td><td>9</td><td>8</td><td>7</td><td>6</td><td>5</td><td>4</td><td>3</td><td>2</td><td>1</td><td>0</td></tr><tr><td colspan="4">Reserved</td><td>ASOS</td><td>ASOE</td><td colspan="2">CCO</td><td colspan="8">CAL[6:0]</td></tr></table>

<table><tr><td>位</td><td>名称</td><td>访问</td><td>描述</td><td>复位值</td></tr><tr><td>[15:10]</td><td>Reserved</td><td>R0</td><td>保留。</td><td>0</td></tr><tr><td>9</td><td>ASOS</td><td>RW</td><td>TAMPER引脚闹钟/秒脉冲输出选择。1:输出秒脉冲;0:输出闹钟脉冲。注:此位只会由后备域复位(BDRST)来复位。</td><td>0</td></tr><tr><td>8</td><td>ASOE</td><td>RW</td><td>TAMPER引脚使能脉冲输出位0:禁止输出闹钟脉冲或者秒脉冲;1:使能输出闹钟脉冲或者秒脉冲。注:此位只会由后备域复位(BDRST)来复位。</td><td>0</td></tr><tr><td>7</td><td>CCO</td><td>RW</td><td>校准时钟输出选择位1:TEMPER引脚输出经64分频的RTC时钟;0:不输出校准时钟。注1:开启此功能必须关闭侵入检测功能。</td><td>0</td></tr><tr><td></td><td></td><td></td><td>注2:当VDD供电断开时,该位被清除。</td><td></td></tr><tr><td>[6:0]</td><td>CAL[6:0]</td><td>RW</td><td>校准值寄存器,这个寄存器的值表示在每220个时钟脉冲中有多少个被跳过。这个功能用来校准RTC时钟。RTC时钟可以被减慢0~121ppm。</td><td>0</td></tr></table>

### 4.3.3 侵入检测控制寄存器（BKP_TPCTLR）

偏移地址： $\mathtt { 0 } { \times } 3 0$

<table><tr><td>15</td><td>14</td><td>13</td><td>12</td><td>11</td><td>10</td><td>9</td><td>8</td><td>7</td><td>6</td><td>5</td><td>4</td><td>3</td><td>2</td><td>1</td><td>0</td></tr><tr><td colspan="14">Reserved</td><td>TPAL</td><td>TPE</td></tr></table>

<table><tr><td>位</td><td>名称</td><td>访问</td><td>描述</td><td>复位值</td></tr><tr><td>[15:2]</td><td>Reserved</td><td>R0</td><td>保留。</td><td>0</td></tr><tr><td>1</td><td>TPAL</td><td>RW</td><td>侵入检测引脚（TEMPER引脚）有效电平设置0：侵入检测引脚上的高电平会清除所有后备数据寄存器（硬件锁定上升沿）；1：侵入检测引脚上的低电平会清除所有后备数据寄存器（硬件锁定下降沿）。</td><td>0</td></tr><tr><td>0</td><td>TPE</td><td>RW</td><td>侵入检测引脚使能位0：TEMPER引脚做普通I0口用；1：TEMPER引脚做侵入检测用。</td><td>0</td></tr></table>

注：同时将TPAL和TPE位清除会产生一个假的侵入事件，推荐只在TPE为O时才改变TPAL位的状态。

### 4.3.4 侵入检测状态寄存器（BKP_TPCSR）

偏移地址： $0 \times 3 4$

<table><tr><td>15</td><td>14</td><td>13</td><td>12</td><td>11</td><td>10</td><td>9</td><td>8</td><td>7</td><td>6</td><td>5</td><td>4</td><td>3</td><td>2</td><td>1</td><td>0</td></tr><tr><td colspan="4">Reserved</td><td>TIF</td><td>TEF</td><td colspan="6">Reserved</td><td>TPIE</td><td>CTI</td><td>CTE</td><td></td></tr></table>

<table><tr><td>位</td><td>名称</td><td>访问</td><td>描述</td><td>复位值</td></tr><tr><td>[15:10]</td><td>Reserved</td><td>R0</td><td>保留。</td><td>0</td></tr><tr><td>9</td><td>TIF</td><td>R0</td><td>侵入中断标志位,当检测到侵入事件且TPIE位置1时,此位会被置位。通过向CTI位写1来清除此标志位。如果TPIE位被复位,那么此位同时也会被复位。注:仅当系统复位或由待机模式唤醒后才复位该位。</td><td>0</td></tr><tr><td>8</td><td>TEF</td><td>R0</td><td>侵入事件标志位,当检测到侵入事件时,此位会被置位。通过向CTE位写1会清除此位。注:当此位为1时,所有的BKP_DATARx寄存器的值会被清除,且在此位不复位前,所有对BKP_DATARx寄存器的写入操作都是无效的。</td><td>0</td></tr><tr><td>[7:3]</td><td>Reserved</td><td>R0</td><td>保留。</td><td>0</td></tr><tr><td>2</td><td>TPIE</td><td>RW</td><td>产生侵入中断使能位:0:禁止侵入检测中断;</td><td>0</td></tr><tr><td></td><td></td><td></td><td>1:使能侵入检测中断(TPE需置1)。注1:侵入中断无法将内核从低功耗模式唤醒。注2:仅当系统复位或由待机模式唤醒后才复位该位。</td><td></td></tr><tr><td>1</td><td>CTI</td><td>WO</td><td>侵入检测中断清除位,写1清除,读取无效。</td><td>0</td></tr><tr><td>0</td><td>CTE</td><td>WO</td><td>侵入检测事件清除位,写1清除,读取无效。</td><td>0</td></tr></table>

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

# 第 6 章 实时时钟（RTC）

本章模块描述适用于CH32F2x、CH32V2x和CH32V3x微控制器全系列产品。

实时时钟（RTC）是一个独立的定时器模块，其可编程计数器最大可达到 32位，配合软件即可以实现实时时钟功能，并且可以修改计数器的值来重新配置系统的当前时间和日期。RTC模块在后备供电区域，系统复位和待机模式唤醒对其不造成影响。

## 6.1 主要特征

$\bullet$ 最高为 $2 ^ { 2 0 }$ 的预分频系数  
$\bullet$ 32位可编程计数器  
$\bullet$ 多种时钟源，中断  
$\bullet$ 独立复位

## 6.2 功能描述

### 6.2.1 概述

![](images/aac205240ce62937478e1599087cd2236a3d2e9b224f5dbf552209956083aefc.jpg)  
图 6-1 RTC 结构框图

由图 6-1 所示，RTC 模块主要是 APB1 总线接口、分频器和计数器、控制和状态寄存器三部分组成，其中分频器和计数器部分在后备区域，可由 $V _ { \mathsf { B A T } }$ 供电。RTCCLK输入分频器（RTC_DIV）之后，被分频成TR_CLK。值得注意的是，分频器（RTC_DIV）的内部是一个自减计数器，自减到溢出就会输出一个TR_CLK，然后从重装值寄存器（RTC_PSCR）里取出预设值重装到分频器里，读分频器实际上是读取它的实时值（read only），写分频系数应该写到重装值寄存器（RTC_PSCR）里。一般 TR_CLK 的周期被设置为 1 秒，TR_CLK 会触发秒事件，同时会使主计数器（RTC_CNT）自增 1；当主计数器增加到和

闹钟寄存器的值一致时，会触发闹钟事件；当主计数器自增到溢出时，会触发溢出事件。以上三种事件都可以触发中断，并对应相应中断使能位控制。

### 6.2.2 复位

由于实时时钟的特殊用途，其处于后备域的四组寄存器：预分频，预分频重装值，主计数器和闹钟，只能通过后备域的复位信号复位，参照 RCC 的后备域复位章节。实时时钟的控制寄存器受系统复位或电源复位控制。

### 6.2.3 较特别的读写寄存器操作

由于实时时钟的特殊用处，RTC 和 APB1 总线是独立的，APB1 对 RTC 的读取不一定是实时的，通过APB1读取RTC的寄存器必须在APB1启动后并经过了一个 RTC上升沿，这种情形可能出现在系统复位和电源复位之后、从待机或者停机模式唤醒后。方便的做法是等待控制寄存器（CTLR）的 RSF位被置高。对 RTC 的写操作器必须等上一个写操作结束，且必须进入配置模式，具体的步骤为：

1） 查询 RTOFF 位，直到其变为 1；  
2） 置 CNF 位，进入配置模式；  
3） 对一个或者多个 RTC 寄存器进行写操作；  
4） 清 CNF 位，退出配置模式，APB1 接口开始对 RTC 寄存器进行写入；  
5） 查询 RTOFF 位，直到其变为 1 即为写完；

## 6.3 寄存器描述

表 6-1 RTC 相关寄存器列表  

<table><tr><td>名称</td><td>访问地址</td><td>描述</td><td>复位值</td></tr><tr><td>R16_RTC_CTLRH</td><td>0x40002800</td><td>RTC控制寄存器高位</td><td>0x0000</td></tr><tr><td>R16_RTC_CTLRL</td><td>0x40002804</td><td>RTC控制寄存器低位</td><td>0x0000</td></tr><tr><td>R16_RTC_PSCRH</td><td>0x40002808</td><td>预分频器重装值寄存器高位</td><td>0x0000</td></tr><tr><td>R16_RTC_PSCRL</td><td>0x4000280C</td><td>预分频器重装值寄存器低位</td><td>0x0000</td></tr><tr><td>R16_RTC_DIVH</td><td>0x40002810</td><td>分频器寄存器高位</td><td>0x0000</td></tr><tr><td>R16_RTC_DIVL</td><td>0x40002814</td><td>分频器寄存器低位</td><td>0x0000</td></tr><tr><td>R16_RTC_CNTH</td><td>0x40002818</td><td>RTC计数器高位</td><td>0x0000</td></tr><tr><td>R16_RTC_CNTL</td><td>0x4000281C</td><td>RTC计数器低位</td><td>0x0000</td></tr><tr><td>R16_RTC_ALRMH</td><td>0x40002820</td><td>闹钟寄存器高位</td><td>0xFFFF</td></tr><tr><td>R16_RTC_ALRML</td><td>0x40002824</td><td>闹钟寄存器低位</td><td>0xFFFF</td></tr></table>

### 6.3.1 RTC 控制寄存器高位（RTC_CTLRH）

偏移地址： $0 \times 0 0$

15 14 13 12 11 10 9 8 7 6 5 4 3 2 1 0

<table><tr><td>Reserved</td><td>OWIE</td><td>ALRIE</td><td>SECIE</td></tr></table>

<table><tr><td>位</td><td>名称</td><td>访问</td><td>描述</td><td>复位值</td></tr><tr><td>[15:3]</td><td>Reserved</td><td>R0</td><td>保留。</td><td>0</td></tr><tr><td>2</td><td>OWIE</td><td>RW</td><td>溢出中断使能位。</td><td>0</td></tr><tr><td>1</td><td>ALRIE</td><td>RW</td><td>闹钟中断使能位。</td><td>0</td></tr><tr><td>0</td><td>SECIE</td><td>RW</td><td>秒中断使能位。</td><td>0</td></tr></table>

### 6.3.2 RTC 控制寄存器低位（RTC_CTLRL）

偏移地址： $0 \times 0 4$

<table><tr><td>Reserved</td><td>RTOFF</td><td>CNF</td><td>RSF</td><td>OWF</td><td>ALRF</td><td>SECF</td></tr></table>

<table><tr><td>位</td><td>名称</td><td>访问</td><td>描述</td><td>复位值</td></tr><tr><td>[15:6]</td><td>Reserved</td><td>R0</td><td>保留。</td><td>0</td></tr><tr><td>5</td><td>RTOFF</td><td>R0</td><td>RTC操作状态指示位,表示对RTC的最后一次操作的执行状态,对RTC的操作必须等待此位为1。1:上一次对RTC的操作已经完成;0:上一次对RTC的操作还在进行中。</td><td>1</td></tr><tr><td>4</td><td>CNF</td><td>RW</td><td>配置标志位,将此位写1进入配置模式,从而允许向计数器(R16_RTC_CNTx)、闹钟寄存器(R16_RTC_ALRMx)和预分频器重装值寄存器(R16_RTC_PSCRx)写入值.只有将该位写1并重新被软件清0后才会执行写的操作:1:进入配置模式;0:退出配置模式,开始更新RTC寄存器。</td><td>0</td></tr><tr><td>3</td><td>RSF</td><td>RWO</td><td>寄存器同步标志位,在对RTC模块的预分频(PSCRx)、闹钟(ALRMx)、计数器(CNTx)这些寄存器进行读写前,都要先保证这个位已经被硬件置位,以确定这些寄存器已经被同步;在进行读写这些寄存器时,或者APB1复位或APB1时钟停止后,第一步应该将此位复位。1:寄存器已被同步;0:寄存器未被同步。</td><td>0</td></tr><tr><td>2</td><td>OWF</td><td>RWO</td><td>计数器溢出标志,当32位计数器溢出时,此位由硬件置位。如果置位了OWIE位,还会产生一个溢出中断。此位只能由软件清零,不能被软件置位。</td><td>0</td></tr><tr><td>1</td><td>ALRF</td><td>RWO</td><td>闹钟标志,当计数器的值达到闹钟寄存器(ALRMx)的值,此位会被硬件置位,如果闹钟中断使能位(ALRIE)置位,还会产生一个闹钟中断。此位只能由软件清零,不能被软件置位。</td><td>0</td></tr><tr><td>0</td><td>SECF</td><td>RWO</td><td>秒事件标志,当时钟经过预分频器分频后每产生一个下降沿,就会使计数器自增一,同时产生一个秒事件,此位会被置位,如果秒中断被使能(SECIE被置位),同时还会产生一个秒中断。此位只能由软件清零,不能被软件置位。</td><td>0</td></tr></table>

### 6.3.3 预分频器重装值寄存器高位（RTC_PSCRH）

偏移地址： $0 \times 0 8$

<table><tr><td>15</td><td>14</td><td>13</td><td>12</td><td>11</td><td>10</td><td>9</td><td>8</td><td>7</td><td>6</td><td>5</td><td>4</td><td>3</td><td>2</td><td>1</td><td>0</td></tr><tr><td colspan="12">Reserved</td><td colspan="4">PRL [19:16]</td></tr></table>

<table><tr><td>位</td><td>名称</td><td>访问</td><td>描述</td><td>复位值</td></tr><tr><td>[15:4]</td><td>Reserved</td><td>R0</td><td>保留。</td><td>0</td></tr><tr><td>[3:0]</td><td>PRL[19:16]</td><td>W0</td><td>重装值高位。</td><td>0</td></tr></table>

### 6.3.4 预分频器重装值寄存器低位（RTC_PSCRL）

偏移地址： $0 \times 0 0$

<table><tr><td>15</td><td>14</td><td>13</td><td>12</td><td>11</td><td>10</td><td>9</td><td>8</td><td>7</td><td>6</td><td>5</td><td>4</td><td>3</td><td>2</td><td>1</td><td>0</td></tr><tr><td colspan="16">PRL[15:0]</td></tr></table>

<table><tr><td>位</td><td>名称</td><td>访问</td><td>描述</td><td>复位值</td></tr><tr><td>[15:0]</td><td>PRL[15:0]</td><td>WO</td><td>重装值低位。实际的分频系数就是(PRL[19:0]+1),比如如果RTC输入频率为32768Hz,那么这个值设为0x7fff就可以分频出1秒周期的信号。</td><td>8000h</td></tr></table>

### 6.3.5 分频器寄存器高位（RTC_DIVH）

偏移地址：0x10

<table><tr><td>15</td><td>14</td><td>13</td><td>12</td><td>11</td><td>10</td><td>9</td><td>8</td><td>7</td><td>6</td><td>5</td><td>4</td><td>3</td><td>2</td><td>1</td><td>0</td></tr><tr><td colspan="12">Reserved</td><td colspan="4">DIV[19:16]</td></tr></table>

<table><tr><td>位</td><td>名称</td><td>访问</td><td>描述</td><td>复位值</td></tr><tr><td>[15:4]</td><td>Reserved</td><td>R0</td><td>保留。</td><td>0</td></tr><tr><td>[3:0]</td><td>DIV[19:16]</td><td>R0</td><td>分频器寄存器高位。</td><td>0</td></tr></table>

### 6.3.6 分频器寄存器低位（RTC_DIVL）

偏移地址： $0 \times 1 4$

<table><tr><td>15</td><td>14</td><td>13</td><td>12</td><td>11</td><td>10</td><td>9</td><td>8</td><td>7</td><td>6</td><td>5</td><td>4</td><td>3</td><td>2</td><td>1</td><td>0</td></tr><tr><td colspan="16">DIV[15:0]</td></tr></table>

<table><tr><td>位</td><td>名称</td><td>访问</td><td>描述</td><td>复位值</td></tr><tr><td>[15:0]</td><td>DIV[15:0]</td><td>RO</td><td>分频器寄存器低位。DIV实际上是一个自减计数器，RTC_CLK每来一个时钟DIV计数器就会减1，溢出后就会输出一个TR_CLK，同时从PSCR中重装载值。DIV只能读取，读出的是当前分频器的计数器的剩余值。</td><td>8000h</td></tr></table>

### 6.3.7 RTC 计数器高位（RTC_CNTH）

偏移地址： $0 \times 1 8$

<table><tr><td>15</td><td>14</td><td>13</td><td>12</td><td>11</td><td>10</td><td>9</td><td>8</td><td>7</td><td>6</td><td>5</td><td>4</td><td>3</td><td>2</td><td>1</td><td>0</td></tr><tr><td colspan="16">CNT[31:16]</td></tr></table>

<table><tr><td>位</td><td>名称</td><td>访问</td><td>描述</td><td>复位值</td></tr><tr><td>[15:0]</td><td>CNT[31:16]</td><td>RW</td><td>计数器高位。</td><td>0</td></tr></table>

### 6.3.8 RTC 计数器低位（RTC_CNTL）

偏移地址： ${ 0 } { \times 1 } { 0 }$

<table><tr><td>15</td><td>14</td><td>13</td><td>12</td><td>11</td><td>10</td><td>9</td><td>8</td><td>7</td><td>6</td><td>5</td><td>4</td><td>3</td><td>2</td><td>1</td><td>0</td></tr><tr><td colspan="16">CNT[15:0]</td></tr></table>

<table><tr><td>位</td><td>名称</td><td>访问</td><td>描述</td><td>复位值</td></tr><tr><td>[15:0]</td><td>CNT[15:0]</td><td>RW</td><td>计数器低位，RTC定时器的核心器件，由TRCLK（周期一般设为1秒）提供时钟。通过读取CNT[31:0]来计算出当前的时间。写这个值需要进入配置模式。</td><td>0</td></tr></table>

### 6.3.9 闹钟寄存器高位（RTC_ALRMH）

偏移地址： $_ { 0 \times 2 0 }$

<table><tr><td>15</td><td>14</td><td>13</td><td>12</td><td>11</td><td>10</td><td>9</td><td>8</td><td>7</td><td>6</td><td>5</td><td>4</td><td>3</td><td>2</td><td>1</td><td>0</td></tr><tr><td colspan="16">ALR[31:16]</td></tr></table>

<table><tr><td>位</td><td>名称</td><td>访问</td><td>描述</td><td>复位值</td></tr><tr><td>[15:0]</td><td>ALR[31:16]</td><td>WO</td><td>闹钟寄存器高位。</td><td>FFFFh</td></tr></table>

### 6.3.10 闹钟寄存器低位（RTC_ALRML）

偏移地址： ${ 0 } { \times 2 4 }$

<table><tr><td>15</td><td>14</td><td>13</td><td>12</td><td>11</td><td>10</td><td>9</td><td>8</td><td>7</td><td>6</td><td>5</td><td>4</td><td>3</td><td>2</td><td>1</td><td>0</td></tr><tr><td colspan="16">ALR[15:0]</td></tr></table>

<table><tr><td>位</td><td>名称</td><td>访问</td><td>描述</td><td>复位值</td></tr><tr><td>[15:0]</td><td>ALR[15:0]</td><td>WO</td><td>闹钟寄存器低位。当闹钟寄存器ALRM[31:0]的值和计数器CNT[31:0]的值一致时会产生一个闹钟事件。更改这个值需要进入配置模式。</td><td>FFFFh</td></tr></table>

# 第 7 章 独立看门狗（IWDG）

本章模块描述适用于CH32F2x、CH32V2x和 $_ { C H 3 2 V 3 x }$ 微控制器全系列产品。

系统设有独立看门狗（IWDG）用来检测逻辑错误和外部环境干扰引起的软件故障。IWDG时钟源来自于 LSI，可独立于主程序之外运行，适用于对精度要求低的场合。

## 7.1 主要特征

12 位自减型计数器  
$\bullet$ 时钟来源 LSI 分频，可以在低功耗模式下运行  
复位条件：计数器值减到0

## 7.2 功能说明

### 7.2.1 原理和用法

独立看门狗的时钟来源LSI时钟分频，其功能在停机和待机模式时仍能正常工作。当看门狗计数器自减到0时，将会产生系统复位，所以超时时间为（重装载值 $\cdot ^ { + 1 }$ ）个时钟。

![](images/fc2b2dd3c8ff6f58bfa3c4d24e2b8e92f24223338c05f5de7d34ed4187908192.jpg)  
图7-1 独立看门狗的结构框图

#### 启动独立看门狗

系统复位后，看门狗处于关闭状态，向 IWDG_CTLR 寄存器写 $0 \times 0 0 0 0$ 开启看门狗，随后它不能再被关闭，除非发生复位。

如果在用户选择字开启了硬件独立看门狗使能位（IWDG_SW），在微控制器复位后将固定开启 IWDG。

#### 看门狗配置

看门狗内部是一个递减运行的12位计数器，当计数器的值减为0 时，将发生系统复位。开启 IWDG功能，需要执行下面几点操作：

1) 计数时基：IWDG 时钟来源 LSI，通过 IWDG_PSCR 寄存器设置 LSI 分频值时钟作为 IWDG 的计数时基。操作方法先向IWDG_CTLR寄存器写 $0 \times 5 5 5 5$ ，再修改 IWDG_PSCR 寄存器中的分频值。IWDG_STATR寄存器中的 PVU 位指示了分频值更新状态，在更新完成的情况下才可以进行分频值的修改和读出。  
2) 重装载值：用于更新独立看门狗中计数器当前值，并且计数器由此值进行递减。操作方法先向IWDG_CTLR 寄存器写 $0 \times 5 5 5 5$ ，再修改 IWDG_RLDR 寄存器设置目标重装载值。IWDG_STATR 寄存器中的 RVU位指示了重装载值更新状态，在更新完成的情况下才可以进行 IWDG_RLDR寄存器的修改和读出。  
3) 看门狗使能：向 IWDG_CTLR 寄存器写 $0 \times 0 0 0 0$ ，即可开启看门狗功能。  
4) 喂狗：即在看门狗计数器递减到 0 前刷新当前计数器值防止发生系统复位。向 IWDG_CTLR 寄存器写0xAAAA，让硬件将IWDG_RLDR寄存值更新到看门狗计数器中。此动作需要在看门狗功能开启后

定时执行，否则会出现看门狗复位动作。

### 7.2.2 调试模式

系统进入调试模式时，可以由调试模块寄存器配置IWDG的计数器继续工作或停止。

## 7.3 寄存器描述

表 7-1 IWDG 相关寄存器列表  

<table><tr><td>名称</td><td>访问地址</td><td>描述</td><td>复位值</td></tr><tr><td>R16_IWDG_CTLR</td><td>0x40003000</td><td>控制寄存器</td><td>0x0000</td></tr><tr><td>R16_IWDG_PSCR</td><td>0x40003004</td><td>分频因子寄存器</td><td>0x0000</td></tr><tr><td>R16_IWDG_RLDR</td><td>0x40003008</td><td>重装载值寄存器</td><td>0x0FFF</td></tr><tr><td>R16_IWDG_STAT</td><td>0x4000300C</td><td>状态寄存器</td><td>0x0000</td></tr></table>

### 7.3.1 IWDG 控制寄存器（IWDG_CTLR）

偏移地址： $0 \times 0 0$

15 14 13 12 11 10 9 8 7 6 5 4 3 2 1 0

KEY[15:0]

<table><tr><td>位</td><td>名称</td><td>访问</td><td>描述</td><td>复位值</td></tr><tr><td>[15:0]</td><td>KEY[15:0]</td><td>WO</td><td>操作键值锁。0xAAAA:喂狗。加载IWDG_RLDR寄存器值到独立看门狗计数器中;0x5555:允许修改R16_IWDG_PSCR和R16_IWDG_RLDR寄存器;0xCC:启动看门狗,如果启用了硬件看门狗(用户选择字配置)则不受这个限制。</td><td>0</td></tr></table>

### 7.3.2 分频因子寄存器（IWDG_PSCR）

偏移地址： $0 \times 0 4$

15 14 13 12 11 10 9 8 7 6 5 4 3 2 1 0

<table><tr><td>Reserved</td><td>PR[2:0]</td></tr></table>

<table><tr><td>位</td><td>名称</td><td>访问</td><td>描述</td><td>复位值</td></tr><tr><td>[15:3]</td><td>Reserved</td><td>R0</td><td>保留。</td><td>0</td></tr><tr><td>[2:0]</td><td>PR[2:0]</td><td>RW</td><td>IWDG时钟分频系数,修改此域前要向KEY中写0x5555。000:4分频; 001:8分频;010:16分频; 011:32分频;100:64分频; 101:128分频;110:256分频; 111:256分频。IWDG计数时基=LSI/分频系数。注:读该域值前,要确保IWDG_STAT寄存器中的PVU位为0,否则读出值无效。</td><td>000b</td></tr></table>

### 7.3.3 重装载值寄存器（IWDG_RLDR）

偏移地址： $0 \times 0 8$

<table><tr><td>15</td><td>14</td><td>13</td><td>12</td><td>11</td><td>10</td><td>9</td><td>8</td><td>7</td><td>6</td><td>5</td><td>4</td><td>3</td><td>2</td><td>1</td><td>0</td></tr><tr><td colspan="4">Reserved</td><td colspan="12">RL[11:0]</td></tr></table>

<table><tr><td>位</td><td>名称</td><td>访问</td><td>描述</td><td>复位值</td></tr><tr><td>[15:12]</td><td>Reserved</td><td>R0</td><td>保留。</td><td>0</td></tr><tr><td>[11:0]</td><td>RL[11:0]</td><td>RW</td><td>计数器重装载值。修改此域前要向KEY中写0x5555。当向KEY中写0xAAAA后,此域的值将会被硬件装载到计数器中,随后计数器从这个值开始递减计数。注:读写该域值前,要确保IWDG_STAT寄存器中的RVU位为0,否则读写此域无效。</td><td>FFFh</td></tr></table>

注：此寄存器在待机模式下会被复位。

### 7.3.4 状态寄存器（IWDG_STATR）

偏移地址： $0 \times 0 0$

<table><tr><td>15</td><td>14</td><td>13</td><td>12</td><td>11</td><td>10</td><td>9</td><td>8</td><td>7</td><td>6</td><td>5</td><td>4</td><td>3</td><td>2</td><td>1</td><td>0</td></tr><tr><td colspan="14">Reserved</td><td>RVU</td><td>PVU</td></tr></table>

<table><tr><td>位</td><td>名称</td><td>访问</td><td>描述</td><td>复位值</td></tr><tr><td>[15:2]</td><td>Reserved</td><td>R0</td><td>保留。</td><td>0</td></tr><tr><td>1</td><td>RVU</td><td>R0</td><td>重装值更新标志位。硬件置位或清0。1:重装载值更新正在进行中;0:重装载更新结束(最多5个LSI周期)。注:重装载值寄存器IWDG_RLDR只有在RVU位被清0后才可读写访问。</td><td>0</td></tr><tr><td>0</td><td>PVU</td><td>R0</td><td>时钟分频系数更新标志位。硬件置位或清0。1:时钟分频值更新正在进行中;0:时钟分频值更新结束(最多5个LSI周期)。注:分频因子寄存器IWDG_PSCR只有在PVU位被清0后才可读写访问。</td><td>0</td></tr></table>

注：在预分频或重装值更新后，不必等待RVU或PVU复位，可继续执行下面的代码。（即使在低功耗模式下，此写操作仍会被继续执行完成。）

# 第 8 章 窗口看门狗（WWDG）

本章模块描述适用于CH32F2x、CH32V2x和CH32V3x微控制器全系列产品。

窗口看门狗一般用来监测系统运行的软件故障，例如外部干扰、不可预见的逻辑错误等情况。它需要在一个特定的窗口时间（有上下限）内进行计数器刷新（喂狗），否则早于或者晚于这个窗口时间看门狗电路都会产生系统复位。

## 8.1 主要特征

可编程的 7 位自减型计数器  
$\bullet$ 双条件复位：当前计数器值小于 $0 \times 4 0$ ，或者计数器值在窗口时间外被重装载  
唤醒提前通知功能（EWI），用于及时喂狗动作防止系统复位

## 8.2 功能说明

### 8.2.1 原理和用法

窗口看门狗运行基于一个 7 位的递减计数器，其挂载在 APB1 总线下，计数时基 WWDG_CLK 来源（PCLK1/4096）时钟的分频，分频系数在配置寄存器 WWDG_CFGR 中的 WDGTB[1:0]域设置。递减计数器处于自由运行状态，无论看门狗功能是否开启，计数器一直循环递减计数。如图 8-1所示，窗口看门狗内部结构框图。

![](images/baff27dc9557e943569713b5c4632826621eaa0523c48876e4ae56a2d584f082.jpg)  
图 8-1 窗口看门狗结构框图

#### 启动窗口看门狗

系统复位后，看门狗处于关闭状态，设置 WWDG_CTLR 寄存器的 WDGA 位能够开启看门狗，随后它不能再被关闭，除非发生复位。

注：可以通过设置RCC_APB1PCENR寄存器关闭WWDG的时钟来源，暂停 WWDG_CLK计数，间接停止看门狗功能，或者通过设置RCC_APB1PRSTR寄存器复位WWDG模块，等效为复位的作用。

#### 看门狗配置

看门狗内部是一个不断循环递减运行的 7位计数器，支持读写访问。使用看门狗复位功能，需要执行下面几点操作：

1） 计数时基：通过 WWDG_CFGR 寄存器的 WDGTB[1:0]位域，注意要开启 RCC 单元的 WWDG 模块时钟。  
2） 窗口计数器：设置 WWDG_CFGR 寄存器的 W[6:0]位域，此计数器由硬件用作和当前计数器比较使用，数值由用户软件配置，不会改变。作为窗口时间的上限值。  
3） 看门狗使能：WWDG_CTLR 寄存器 WDGA 位软件置 1，开启看门狗功能，可以系统复位。

4） 喂狗：即刷新当前计数器值，配置WWDG_CTLR寄存器的 T[6:0]位域。此动作需要在看门狗功能开启后，在周期性的窗口时间内执行，否则会出现看门狗复位动作。

#### 喂狗窗口时间

如图 8-2 所示，灰色区域为窗口看门狗的监测窗口区域，其上限时间 t2 对应当前计数器值达到窗口值W[6:0]的时间点；其下限时间t3对应当前计数器值达到 ${ 0 } \times { 3 } { \mathsf F }$ 的时间点。此区域时间内 $\pm 2 < \mathrm { t } < \mathrm { t } 3$ 可以进行喂狗操作（写T[6:0]），刷新当前计数器的数值。

![](images/da380698c6bc3527e8c434bc1a7f073a79df5a927133618f4f7c673cd4f3a714.jpg)  
图8-2 窗口看门狗的计数模式

#### 看门狗复位：

1） 当没有及时喂狗操作，导致 T[6:0]计数器的值由 $0 \times 4 0$ 变成 ${ 0 } \times { 3 } { \mathsf F }$ ，将出现“窗口看门狗复位”，产生系统复位。即T6-bit被硬件检测为 0，将出现系统复位。

注：应用程序可以通过软件写T6-bit为0，实现系统复位，等效软件复位功能。

2） 当在不允许喂狗时间内执行计数器刷新动作，即在 $\mathtt { t } 1 \leqslant \mathtt { t } \leqslant \mathtt { t } 2$ 时间内操作写 T[6:0]位域，将出现“窗口看门狗复位”，产生系统复位。

#### 提前唤醒

为了防止没有及时刷新计数器导致系统复位，看门狗模块提供了早期唤醒中断（EWI）通知。当计数器自减到 $0 \times 4 0$ 时，产生提前唤醒信号，EWIF 标志置 1，如果置位了 EWI 位，会同时触发窗口看门狗中断。此时距离硬件复位有1个计数器时钟周期（自减为 ${ 0 } \times { 3 } { \mathsf F }$ ），应用程序可在此时间内即时进行喂狗操作。

### 8.2.2 调试模式

系统进入调试模式时，可以由调试模块寄存器配置WWDG的计数器继续工作或停止。

## 8.3 寄存器描述

表 8-1 WWDG 相关寄存器列表  

<table><tr><td>名称</td><td>访问地址</td><td>描述</td><td>复位值</td></tr><tr><td>R16_WWDG_CTLR</td><td>0x40002C00</td><td>控制寄存器</td><td>0x007F</td></tr><tr><td>R16_WWDG_CFGR</td><td>0x40002C04</td><td>配置寄存器</td><td>0x007F</td></tr><tr><td>R16_WWDG_STAT</td><td>0x40002C08</td><td>状态寄存器</td><td>0x0000</td></tr></table>

### 8.3.1 WWDG 控制寄存器（WWDG_CTLR）

偏移地址： $0 \times 0 0$

<table><tr><td>15</td><td>14</td><td>13</td><td>12</td><td>11</td><td>10</td><td>9</td><td>8</td><td>7</td><td>6</td><td>5</td><td>4</td><td>3</td><td>2</td><td>1</td><td>0</td></tr><tr><td colspan="8">Reserved</td><td>WDGA</td><td colspan="7">T[6:0]</td></tr></table>

<table><tr><td>位</td><td>名称</td><td>访问</td><td>描述</td><td>复位值</td></tr><tr><td>[15:8]</td><td>Reserved</td><td>R0</td><td>保留。</td><td>0</td></tr><tr><td>7</td><td>WDGA</td><td>RW1</td><td>窗口看门狗复位使能位。
1: 开启看门狗功能（可产生复位信号）;
0: 禁止看门狗功能。
软件写1开启,但是只允许复位后硬件清0。</td><td>0</td></tr><tr><td>[6:0]</td><td>T[6:0]</td><td>RW</td><td>7位自减计数器,每4096*2WDGTB个PCLK1周期自减1。当计数器从0x40自减到0x3F时,即T6跳变为0时,产生看门狗复位。</td><td>7Fh</td></tr></table>

### 8.3.2 WWDG 配置寄存器（WWDG_CFGR）

偏移地址： $0 \times 0 4$

<table><tr><td>15</td><td>14</td><td>13</td><td>12</td><td>11</td><td>10</td><td>9</td><td>8</td><td>7</td><td>6</td><td>5</td><td>4</td><td>3</td><td>2</td><td>1</td><td>0</td></tr><tr><td colspan="6">Reserved</td><td>EWI</td><td colspan="2">WDGTB[1:0]</td><td colspan="7">W[6:0]</td></tr></table>

<table><tr><td>位</td><td>名称</td><td>访问</td><td>描述</td><td>复位值</td></tr><tr><td>[15:10]</td><td>Reserved</td><td>R0</td><td>保留。</td><td>0</td></tr><tr><td>9</td><td>EWI</td><td>RW1</td><td>提前唤醒中断使能位。若此位置1,则在计数器的值达到0x40时产生中断。此位只能在复位后由硬件请0。</td><td>0</td></tr><tr><td>[8:7]</td><td>WDGTB[1:0]</td><td>RW</td><td>窗口看门狗时钟分频选择:00:1分频,计数时基 = PCLK1/4096;01:2分频,计数时基 = PCLK1/4096/2;10:4分频,计数时基 = PCLK1/4096/4;11:8分频,计数时基 = PCLK1/4096/8。</td><td>00b</td></tr><tr><td>[6:0]</td><td>W[6:0]</td><td>RW</td><td>窗口看门狗7位窗口值。用来与计数器的值做比较。喂狗操作只能在计数器的值小于窗口值且大于0x3F时进行。</td><td>7Fh</td></tr></table>

### 8.3.3 WWDG 状态寄存器（WWDG_STATR）

偏移地址： $0 \times 0 8$

<table><tr><td>15</td><td>14</td><td>13</td><td>12</td><td>11</td><td>10</td><td>9</td><td>8</td><td>7</td><td>6</td><td>5</td><td>4</td><td>3</td><td>2</td><td>1</td><td>0</td></tr><tr><td colspan="15">Reserved</td><td>EWIF</td></tr></table>

<table><tr><td>位</td><td>名称</td><td>访问</td><td>描述</td><td>复位值</td></tr><tr><td>[15:1]</td><td>Reserved</td><td>W0</td><td>保留。</td><td>0</td></tr><tr><td>0</td><td>EWIF</td><td>RWO</td><td>提前唤醒中断标志位。
当计数器到达0x40时，此位会被硬件置位，必须通过软件清0，用户置位是无效的。即使EWI未被置位，此位在事件发生时仍会照常被置位。</td><td>0</td></tr></table>

# 第 9 章 中断和事件（NVIC/PFIC）

本章模块描述适用于CH32F2x、CH32V2x和CH32V3x微控制器全系列产品。

${ \tt C H 3 2 F 2 x }$ 系列产品采用 Cortex-M3 内核，内置嵌套向量中断控制器(NVIC– Nested VectoredInterrupt Controller)，管理了 88 个可屏蔽外部中断通道和 10 个内核中断通道，其他中断源保留。中断控制器与内核接口紧密相连，以最小的中断延迟提供了灵活的中断管理功能。具体关于 NVIC 控制器的使用说明请参考Cortex-M3相关文档说明。

CH32V2x 和 CH32V3x 系列内置可编程快速中断控制器（PFIC– Programmable Fast InterruptController），最多支持 255 个中断向量。当前系统管理了 88 个外设中断通道和 8 个内核中断通道，其他保留。

## 9.1 主要特征

### 9.1.1 NVIC 控制器

$\bullet$ 88个可屏蔽的中断通道  
$\bullet$ 提供不可屏蔽中断第一时间响应  
$\bullet$ 向量化的中断设计实现向量入口地址直接进入内核  
$\bullet$ 中断进入和退出时自动压栈和恢复，无需额外指令开销  
16级嵌套，优先级动态修改

### 9.1.2 PFIC 控制器

88个外设中断，每个中断请求都有独立的触发和屏蔽控制位，有专用的状态位  
$\bullet$ 可编程多级中断嵌套，最大嵌套深度8级，硬件压栈深度3级  
$\bullet$ 特有快速中断进出机制，硬件自动压栈和恢复，无需指令开销   
$\bullet$ 特有免表VTF（Vector Table Free）中断响应机制，4路可编程直达中断向量地址

## 9.2 系统定时器

CH32F2x 系列产品

Cortex-M3 内核自带了一个 24 位自减型计数器（SysTick timer）。支持 HCLK 或 HCLK/8 作为时基，具有较高优先级别（6）。一般可用于操作系统的时基。具体请参考Cortex-M3相关文档说明。

CH32V3x 系列产品

内核自带了一个 64 位加减计数器（SysTick），支持 HCLK 或者 HCLK/8 作为时基，具有较高优先级，校准后可用于时间基准。

## 9.3 中断和异常的向量表

表 9-1 CH32F2x 系列产品向量表  

<table><tr><td>位置</td><td>优先级</td><td>优先级类型</td><td>名称</td><td>说明</td><td>绝对地址</td></tr><tr><td></td><td>-</td><td>-</td><td>-</td><td>保留</td><td>0x00000000</td></tr><tr><td></td><td>-3</td><td>固定</td><td>Reset</td><td>复位</td><td>0x00000004</td></tr><tr><td></td><td>-2</td><td>固定</td><td>NMI</td><td>不可屏蔽中断</td><td>0x00000008</td></tr><tr><td></td><td>-1</td><td>固定</td><td>HardFault</td><td>所有类型的失效</td><td>0x0000000C</td></tr><tr><td></td><td>0</td><td>可设置</td><td>MemManage</td><td>存储器管理</td><td>0x00000010</td></tr><tr><td></td><td>1</td><td>可设置</td><td>BusFault</td><td>预取指失败,存储器访问失败</td><td>0x00000014</td></tr><tr><td></td><td>2</td><td>可设置</td><td>UsageFault</td><td>未定义的指令或非法状态</td><td>0x00000018</td></tr><tr><td></td><td>-</td><td>-</td><td>-</td><td>保留</td><td>0x0000001C-0x0000002B</td></tr><tr><td></td><td>3</td><td>可设置</td><td>SVCall</td><td>通过SWI指令的系统服务调用</td><td>0x0000002C</td></tr><tr><td></td><td>4</td><td>可设置</td><td>Debug Monitor</td><td>调试监控器</td><td>0x00000030</td></tr><tr><td></td><td>-</td><td>-</td><td>-</td><td>保留</td><td>0x00000034</td></tr><tr><td></td><td>5</td><td>可设置</td><td>PendSV</td><td>可挂起的系统服务</td><td>0x00000038</td></tr><tr><td></td><td>6</td><td>可设置</td><td>SysTick</td><td>系统滴答定时器</td><td>0x0000003C</td></tr><tr><td>0</td><td>7</td><td>可编程</td><td>WWDG</td><td>窗口定时器中断</td><td>0x00000040</td></tr><tr><td>1</td><td>8</td><td>可编程</td><td>PVD</td><td>电源电压检测中断(EXT1)</td><td>0x00000044</td></tr><tr><td>2</td><td>9</td><td>可编程</td><td>TAMPER</td><td>侵入检测中断</td><td>0x00000048</td></tr><tr><td>3</td><td>10</td><td>可编程</td><td>RTC</td><td>实时时钟中断</td><td>0x0000004C</td></tr><tr><td>4</td><td>11</td><td>可编程</td><td>FLASH</td><td>闪存全局中断</td><td>0x00000050</td></tr><tr><td>5</td><td>12</td><td>可编程</td><td>RCC</td><td>复位和时钟控制中断</td><td>0x00000054</td></tr><tr><td>6</td><td>13</td><td>可编程</td><td>EXT10</td><td>EXTI线0中断</td><td>0x00000058</td></tr><tr><td>7</td><td>14</td><td>可编程</td><td>EXT11</td><td>EXTI线1中断</td><td>0x0000005C</td></tr><tr><td>8</td><td>15</td><td>可编程</td><td>EXT12</td><td>EXTI线2中断</td><td>0x00000060</td></tr><tr><td>9</td><td>16</td><td>可编程</td><td>EXT13</td><td>EXTI线3中断</td><td>0x00000064</td></tr><tr><td>10</td><td>17</td><td>可编程</td><td>EXT14</td><td>EXTI线4中断</td><td>0x00000068</td></tr><tr><td>11</td><td>18</td><td>可编程</td><td>DMA1_CH1</td><td>DMA1通道1全局中断</td><td>0x0000006C</td></tr><tr><td>12</td><td>19</td><td>可编程</td><td>DMA1_CH2</td><td>DMA1通道2全局中断</td><td>0x00000070</td></tr><tr><td>13</td><td>20</td><td>可编程</td><td>DMA1_CH3</td><td>DMA1通道3全局中断</td><td>0x00000074</td></tr><tr><td>14</td><td>21</td><td>可编程</td><td>DMA1_CH4</td><td>DMA1通道4全局中断</td><td>0x00000078</td></tr><tr><td>15</td><td>22</td><td>可编程</td><td>DMA1_CH5</td><td>DMA1通道5全局中断</td><td>0x0000007C</td></tr><tr><td>16</td><td>23</td><td>可编程</td><td>DMA1_CH6</td><td>DMA1通道6全局中断</td><td>0x00000080</td></tr><tr><td>17</td><td>24</td><td>可编程</td><td>DMA1_CH7</td><td>DMA1通道7全局中断</td><td>0x00000084</td></tr><tr><td>18</td><td>25</td><td>可编程</td><td>ADC1_2</td><td>ADC1和ADC2全局中断</td><td>0x00000088</td></tr><tr><td>19</td><td>26</td><td>可编程</td><td>USB_HP或</td><td>USB_HP或CAN1_TX全局中断</td><td>0x0000008C</td></tr><tr><td>20</td><td>27</td><td>可编程</td><td>USB_LP或</td><td>USB_LP或CAN1_RX0全局中断</td><td>0x00000090</td></tr><tr><td>21</td><td>28</td><td>可编程</td><td>CAN1_RX1</td><td>CAN1_RX1全局中断</td><td>0x00000094</td></tr><tr><td>22</td><td>29</td><td>可编程</td><td>CAN1_SCE</td><td>CAN1_SCE全局中断</td><td>0x00000098</td></tr><tr><td>23</td><td>30</td><td>可编程</td><td>EXT19_5</td><td>EXTI线[9:5]中断</td><td>0x0000009C</td></tr><tr><td>24</td><td>31</td><td>可编程</td><td>TIM1_BRK</td><td>TIM1刹车中断</td><td>0x000000A0</td></tr><tr><td>25</td><td>32</td><td>可编程</td><td>TIM1_UP</td><td>TIM1更新中断</td><td>0x000000A4</td></tr><tr><td>26</td><td>33</td><td>可编程</td><td>TIM1_TRG_COM</td><td>TIM1触发和通信中断</td><td>0x000000A8</td></tr><tr><td>27</td><td>34</td><td>可编程</td><td>TIM1_CC</td><td>TIM1捕获比较中断</td><td>0x000000AC</td></tr><tr><td>28</td><td>35</td><td>可编程</td><td>TIM2</td><td>TIM2全局中断</td><td>0x000000B0</td></tr><tr><td>29</td><td>36</td><td>可编程</td><td>TIM3</td><td>TIM3全局中断</td><td>0x000000B4</td></tr><tr><td>30</td><td>37</td><td>可编程</td><td>TIM4</td><td>TIM4全局中断</td><td>0x000000B8</td></tr><tr><td>31</td><td>38</td><td>可编程</td><td>I2C1_ER</td><td>I²C1事件中断</td><td>0x000000BC</td></tr><tr><td>32</td><td>39</td><td>可编程</td><td>I2C1_ER</td><td>I²C1错误中断</td><td>0x000000C0</td></tr><tr><td>33</td><td>40</td><td>可编程</td><td>I2C2_ER</td><td>I²C2事件中断</td><td>0x000000C4</td></tr><tr><td>34</td><td>41</td><td>可编程</td><td>I2C2_ER</td><td>I²C2错误中断</td><td>0x000000C8</td></tr><tr><td>35</td><td>42</td><td>可编程</td><td>SPI1</td><td>SPI1全局中断</td><td>0x000000CC</td></tr><tr><td>36</td><td>43</td><td>可编程</td><td>SPI2</td><td>SPI2全局中断</td><td>0x000000D0</td></tr><tr><td>37</td><td>44</td><td>可编程</td><td>USART1</td><td>USART1 全局中断</td><td>0x000000D4</td></tr><tr><td>38</td><td>45</td><td>可编程</td><td>USART2</td><td>USART2 全局中断</td><td>0x000000D8</td></tr><tr><td>39</td><td>46</td><td>可编程</td><td>USART3</td><td>USART3 全局中断</td><td>0x000000DC</td></tr><tr><td>40</td><td>47</td><td>可编程</td><td>EXT115_10</td><td>EXT1 线[15:10]中断</td><td>0x000000E0</td></tr><tr><td>41</td><td>48</td><td>可编程</td><td>RTCA1arm</td><td>RTC 闹钟中断 (EXT1)</td><td>0x000000E4</td></tr><tr><td>42</td><td>49</td><td>可编程</td><td>USBWakeUp</td><td>USB 唤醒中断 (EXT1)</td><td>0x000000E8</td></tr><tr><td>43</td><td>50</td><td>可编程</td><td>TIM8_BRK</td><td>TIM8 刹车中断</td><td>0x000000EC</td></tr><tr><td>44</td><td>51</td><td>可编程</td><td>TIM8_UP</td><td>TIM8 更新中断</td><td>0x000000F0</td></tr><tr><td>45</td><td>52</td><td>可编程</td><td>TIM8_TRG_COM</td><td>TIM8 触发和通信中断</td><td>0x000000F4</td></tr><tr><td>46</td><td>53</td><td>可编程</td><td>TIM8_CC</td><td>TIM8 捕获比较中断</td><td>0x000000F8</td></tr><tr><td>47</td><td>54</td><td>可编程</td><td>RNG</td><td>RNG 全局中断</td><td>0x000000FC</td></tr><tr><td>48</td><td>55</td><td>可编程</td><td>FSMC</td><td>FSMC 全局中断</td><td>0x00000100</td></tr><tr><td>49</td><td>56</td><td>可编程</td><td>SD10</td><td>SD10 全局中断</td><td>0x00000104</td></tr><tr><td>50</td><td>57</td><td>可编程</td><td>TIM5</td><td>TIM5 全局中断</td><td>0x00000108</td></tr><tr><td>51</td><td>58</td><td>可编程</td><td>SPI3</td><td>SPI3 全局中断</td><td>0x0000010C</td></tr><tr><td>52</td><td>59</td><td>可编程</td><td>UART4</td><td>UART4 全局中断</td><td>0x00000110</td></tr><tr><td>53</td><td>60</td><td>可编程</td><td>UART5</td><td>UART5 全局中断</td><td>0x00000114</td></tr><tr><td>54</td><td>61</td><td>可编程</td><td>TIM6</td><td>TIM6 全局中断</td><td>0x00000118</td></tr><tr><td>55</td><td>62</td><td>可编程</td><td>TIM7</td><td>TIM7 全局中断</td><td>0x0000011C</td></tr><tr><td>56</td><td>63</td><td>可编程</td><td>DMA2_CH1</td><td>DMA2 通道 1 全局中断</td><td>0x00000120</td></tr><tr><td>57</td><td>64</td><td>可编程</td><td>DMA2_CH2</td><td>DMA2 通道 2 全局中断</td><td>0x00000124</td></tr><tr><td>58</td><td>65</td><td>可编程</td><td>DMA2_CH3</td><td>DMA2 通道 3 全局中断</td><td>0x00000128</td></tr><tr><td>59</td><td>66</td><td>可编程</td><td>DMA2_CH4</td><td>DMA2 通道 4 全局中断</td><td>0x0000012C</td></tr><tr><td>60</td><td>67</td><td>可编程</td><td>DMA2_CH5</td><td>DMA2 通道 5 全局中断</td><td>0x00000130</td></tr><tr><td>61</td><td>68</td><td>可编程</td><td>ETH</td><td>ETH 全局中断</td><td>0x00000134</td></tr><tr><td>62</td><td>69</td><td>可编程</td><td>ETH_WKUP</td><td>ETH 唤醒中断</td><td>0x00000138</td></tr><tr><td>63</td><td>70</td><td>可编程</td><td>CAN2_TX</td><td>CAN2_TX 全局中断</td><td>0x0000013C</td></tr><tr><td>64</td><td>71</td><td>可编程</td><td>CAN2_RX0</td><td>CAN2_RX0 全局中断</td><td>0x00000140</td></tr><tr><td>65</td><td>72</td><td>可编程</td><td>CAN2_RX1</td><td>CAN2_RX1 全局中断</td><td>0x00000144</td></tr><tr><td>66</td><td>73</td><td>可编程</td><td>CAN2_SCE</td><td>CAN2_SCE 全局中断</td><td>0x00000148</td></tr><tr><td>67</td><td>74</td><td>可编程</td><td>OTG_FS</td><td>全速 0TG 中断</td><td>0x0000014C</td></tr><tr><td>68</td><td>75</td><td>可编程</td><td>USBHSWakeUp</td><td>高速 USB 唤醒中断</td><td>0x00000150</td></tr><tr><td>69</td><td>76</td><td>可编程</td><td>USBHS</td><td>高速 USB 全局中断</td><td>0x00000154</td></tr><tr><td>70</td><td>77</td><td>可编程</td><td>DVP</td><td>DVP 全局中断</td><td>0x00000158</td></tr><tr><td>71</td><td>78</td><td>可编程</td><td>UART6</td><td>UART6 全局中断</td><td>0x0000015C</td></tr><tr><td>72</td><td>79</td><td>可编程</td><td>UART7</td><td>UART7 全局中断</td><td>0x00000160</td></tr><tr><td>73</td><td>80</td><td>可编程</td><td>UART8</td><td>UART8 全局中断</td><td>0x00000164</td></tr><tr><td>74</td><td>81</td><td>可编程</td><td>TIM9_BRK</td><td>TIM9 刹车中断</td><td>0x00000168</td></tr><tr><td>75</td><td>82</td><td>可编程</td><td>TIM9_UP</td><td>TIM9 更新中断</td><td>0x0000016C</td></tr><tr><td>76</td><td>83</td><td>可编程</td><td>TIM9_TRG_COM</td><td>TIM9 触发和通信中断</td><td>0x00000170</td></tr><tr><td>77</td><td>84</td><td>可编程</td><td>TIM9_CC</td><td>TIM9 捕获比较中断</td><td>0x00000174</td></tr><tr><td>78</td><td>85</td><td>可编程</td><td>TIM10_BRK</td><td>TIM10 刹车中断</td><td>0x00000178</td></tr><tr><td>79</td><td>86</td><td>可编程</td><td>TIM10_UP</td><td>TIM10 更新中断</td><td>0x0000017C</td></tr><tr><td>80</td><td>87</td><td>可编程</td><td>TIM10_TRG_COM</td><td>TIM10 触发和通信中断</td><td>0x00000180</td></tr><tr><td>81</td><td>88</td><td>可编程</td><td>TIM10_CC</td><td>TIM10 捕获比较中断</td><td>0x00000184</td></tr><tr><td>82</td><td>89</td><td>可编程</td><td>DMA2_CH6</td><td>DMA2 通道 6 全局中断</td><td>0x00000188</td></tr><tr><td>83</td><td>90</td><td>可编程</td><td>DMA2_CH7</td><td>DMA2 通道 7 全局中断</td><td>0x0000018C</td></tr><tr><td>84</td><td>91</td><td>可编程</td><td>DMA2_CH8</td><td>DMA2 通道 8 全局中断</td><td>0x00000190</td></tr><tr><td>85</td><td>92</td><td>可编程</td><td>DMA2_CH9</td><td>DMA2 通道 9 全局中断</td><td>0x00000194</td></tr><tr><td>86</td><td>93</td><td>可编程</td><td>DMA2_CH10</td><td>DMA2 通道 10 全局中断</td><td>0x00000198</td></tr><tr><td>87</td><td>94</td><td>可编程</td><td>DMA2_CH11</td><td>DMA2 通道 11 全局中断</td><td>0x0000019C</td></tr></table>

表 9-2 CH32V2x 和 CH32V3x 系列产品向量表  

<table><tr><td>编号</td><td>优先级</td><td>类型</td><td>名称</td><td>描述</td><td>入口地址</td></tr><tr><td>0</td><td>-</td><td>-</td><td>-</td><td>-</td><td>0x00000000</td></tr><tr><td>1</td><td>-</td><td>-</td><td>-</td><td>-</td><td>0x00000004</td></tr><tr><td>2</td><td>-5</td><td>固定</td><td>NMI</td><td>不可屏蔽中断</td><td>0x00000008</td></tr><tr><td>3</td><td>-4</td><td>固定</td><td>HardFault</td><td>异常中断</td><td>0x0000000C</td></tr><tr><td>4</td><td>-</td><td>-</td><td>-</td><td>保留</td><td>0x00000010</td></tr><tr><td>5</td><td>-3</td><td>固定</td><td>Ecall-M</td><td>机器模式回调中断</td><td>0x00000014</td></tr><tr><td>6-7</td><td>-</td><td>-</td><td>-</td><td>保留</td><td>0x00000018-0x0000001C</td></tr><tr><td>8</td><td>-2</td><td>固定</td><td>Ecall-U</td><td>用户模式回调中断</td><td>0x00000020</td></tr><tr><td>9</td><td>-1</td><td>固定</td><td>BreakPoint</td><td>断点回调中断</td><td>0x00000024</td></tr><tr><td>10-11</td><td>-</td><td>-</td><td>-</td><td>保留</td><td>0x00000028-0x0000002C</td></tr><tr><td>12</td><td>0</td><td>可编程</td><td>SysTick</td><td>系统定时器中断</td><td>0x00000030</td></tr><tr><td>13</td><td>-</td><td>-</td><td>-</td><td>保留</td><td>0x00000034</td></tr><tr><td>14</td><td>1</td><td>可编程</td><td>SW</td><td>软件中断</td><td>0x00000038</td></tr><tr><td>15</td><td>-</td><td>-</td><td>-</td><td>保留</td><td>0x0000003C</td></tr><tr><td>16</td><td>2</td><td>可编程</td><td>WWDG</td><td>窗口定时器中断</td><td>0x00000040</td></tr><tr><td>17</td><td>3</td><td>可编程</td><td>PVD</td><td>电源电压检测中断(EXTI)</td><td>0x00000044</td></tr><tr><td>18</td><td>4</td><td>可编程</td><td>TAMPER</td><td>侵入检测中断</td><td>0x00000048</td></tr><tr><td>19</td><td>5</td><td>可编程</td><td>RTC</td><td>实时时钟中断</td><td>0x0000004C</td></tr><tr><td>20</td><td>6</td><td>可编程</td><td>FLASH</td><td>闪存全局中断</td><td>0x00000050</td></tr><tr><td>21</td><td>7</td><td>可编程</td><td>RCC</td><td>复位和时钟控制中断</td><td>0x00000054</td></tr><tr><td>22</td><td>8</td><td>可编程</td><td>EXTI0</td><td>EXTI线0中断</td><td>0x00000058</td></tr><tr><td>23</td><td>9</td><td>可编程</td><td>EXTI1</td><td>EXTI线1中断</td><td>0x0000005C</td></tr><tr><td>24</td><td>10</td><td>可编程</td><td>EXTI2</td><td>EXTI线2中断</td><td>0x00000060</td></tr><tr><td>25</td><td>11</td><td>可编程</td><td>EXTI3</td><td>EXTI线3中断</td><td>0x00000064</td></tr><tr><td>26</td><td>12</td><td>可编程</td><td>EXTI4</td><td>EXTI线4中断</td><td>0x00000068</td></tr><tr><td>27</td><td>13</td><td>可编程</td><td>DMA1_CH1</td><td>DMA1通道1全局中断</td><td>0x0000006C</td></tr><tr><td>28</td><td>14</td><td>可编程</td><td>DMA1_CH2</td><td>DMA1通道2全局中断</td><td>0x00000070</td></tr><tr><td>29</td><td>15</td><td>可编程</td><td>DMA1_CH3</td><td>DMA1通道3全局中断</td><td>0x00000074</td></tr><tr><td>30</td><td>16</td><td>可编程</td><td>DMA1_CH4</td><td>DMA1通道4全局中断</td><td>0x00000078</td></tr><tr><td>31</td><td>17</td><td>可编程</td><td>DMA1_CH5</td><td>DMA1通道5全局中断</td><td>0x0000007C</td></tr><tr><td>32</td><td>18</td><td>可编程</td><td>DMA1_CH6</td><td>DMA1通道6全局中断</td><td>0x00000080</td></tr><tr><td>33</td><td>19</td><td>可编程</td><td>DMA1_CH7</td><td>DMA1通道7全局中断</td><td>0x00000084</td></tr><tr><td>34</td><td>20</td><td>可编程</td><td>ADC1_2</td><td>ADC1 和 ADC2 全局中断</td><td>0x00000088</td></tr><tr><td>35</td><td>21</td><td>可编程</td><td>USB_HP 或 CAN1_TX</td><td>USB_HP 或 CAN1_TX 全局中断</td><td>0x0000008C</td></tr><tr><td>36</td><td>22</td><td>可编程</td><td>USB_LP 或 CAN1_RX0</td><td>USB_LP 或 CAN1_RX0 全局中断</td><td>0x00000090</td></tr><tr><td>37</td><td>23</td><td>可编程</td><td>CAN1_RX1</td><td>CAN1_RX1 全局中断</td><td>0x00000094</td></tr><tr><td>38</td><td>24</td><td>可编程</td><td>CAN1_SCE</td><td>CAN1_SCE 全局中断</td><td>0x00000098</td></tr><tr><td>39</td><td>25</td><td>可编程</td><td>EXT19_5</td><td>EXT1 线[9:5]中断</td><td>0x0000009C</td></tr><tr><td>40</td><td>26</td><td>可编程</td><td>TIM1_BRK</td><td>TIM1 刹车中断</td><td>0x000000A0</td></tr><tr><td>41</td><td>27</td><td>可编程</td><td>TIM1_UP</td><td>TIM1 更新中断</td><td>0x000000A4</td></tr><tr><td>42</td><td>28</td><td>可编程</td><td>TIM1_TRG_COM</td><td>TIM1 触发和通信中断</td><td>0x000000A8</td></tr><tr><td>43</td><td>29</td><td>可编程</td><td>TIM1_CC</td><td>TIM1 捕获比较中断</td><td>0x000000AC</td></tr><tr><td>44</td><td>30</td><td>可编程</td><td>TIM2</td><td>TIM2 全局中断</td><td>0x000000B0</td></tr><tr><td>45</td><td>31</td><td>可编程</td><td>TIM3</td><td>TIM3 全局中断</td><td>0x000000B4</td></tr><tr><td>46</td><td>32</td><td>可编程</td><td>TIM4</td><td>TIM4 全局中断</td><td>0x000000B8</td></tr><tr><td>47</td><td>33</td><td>可编程</td><td>I2C1EV</td><td>I²C1 事件中断</td><td>0x000000BC</td></tr><tr><td>48</td><td>34</td><td>可编程</td><td>I2C1_ER</td><td>I²C1 错误中断</td><td>0x000000C0</td></tr><tr><td>49</td><td>35</td><td>可编程</td><td>I2C2EV</td><td>I²C2 事件中断</td><td>0x000000C4</td></tr><tr><td>50</td><td>36</td><td>可编程</td><td>I2C2_ER</td><td>I²C2 错误中断</td><td>0x000000C8</td></tr><tr><td>51</td><td>37</td><td>可编程</td><td>SPI1</td><td>SPI1 全局中断</td><td>0x000000CC</td></tr><tr><td>52</td><td>38</td><td>可编程</td><td>SPI2</td><td>SPI2 全局中断</td><td>0x000000D0</td></tr><tr><td>53</td><td>39</td><td>可编程</td><td>USART1</td><td>USART1 全局中断</td><td>0x000000D4</td></tr><tr><td>54</td><td>40</td><td>可编程</td><td>USART2</td><td>USART2 全局中断</td><td>0x000000D8</td></tr><tr><td>55</td><td>41</td><td>可编程</td><td>USART3</td><td>USART3 全局中断</td><td>0x000000DC</td></tr><tr><td>56</td><td>42</td><td>可编程</td><td>EXT115_10</td><td>EXT1 线[15:10]中断</td><td>0x000000E0</td></tr><tr><td>57</td><td>43</td><td>可编程</td><td>RTCA1arm</td><td>RTC 闹钟中断 (EXT1)</td><td>0x000000E4</td></tr><tr><td>58</td><td>44</td><td>可编程</td><td>USBWakeUp</td><td>USB 唤醒中断 (EXT1)</td><td>0x000000E8</td></tr><tr><td>59</td><td>45</td><td>可编程</td><td>TIM8_BRK</td><td>TIM8 刹车中断</td><td>0x000000EC</td></tr><tr><td>60</td><td>46</td><td>可编程</td><td>TIM8_UP</td><td>TIM8 更新中断</td><td>0x000000F0</td></tr><tr><td>61</td><td>47</td><td>可编程</td><td>TIM8_TRG_COM</td><td>TIM8 触发和通信中断</td><td>0x000000F4</td></tr><tr><td>62</td><td>48</td><td>可编程</td><td>TIM8_CC</td><td>TIM8 捕获比较中断</td><td>0x000000F8</td></tr><tr><td>63</td><td>49</td><td>可编程</td><td>RNG</td><td>RNG 全局中断</td><td>0x000000FC</td></tr><tr><td>64</td><td>50</td><td>可编程</td><td>FSMC</td><td>FSMC 全局中断</td><td>0x00000100</td></tr><tr><td>65</td><td>51</td><td>可编程</td><td>SD10</td><td>SD10 全局中断</td><td>0x00000104</td></tr><tr><td>66</td><td>52</td><td>可编程</td><td>TIM5</td><td>TIM5 全局中断</td><td>0x00000108</td></tr><tr><td>67</td><td>53</td><td>可编程</td><td>SPI3</td><td>SPI3 全局中断</td><td>0x0000010C</td></tr><tr><td>68</td><td>54</td><td>可编程</td><td>UART4</td><td>UART4 全局中断</td><td>0x00000110</td></tr><tr><td>69</td><td>55</td><td>可编程</td><td>UART5</td><td>UART5 全局中断</td><td>0x00000114</td></tr><tr><td>70</td><td>56</td><td>可编程</td><td>TIM6</td><td>TIM6 全局中断</td><td>0x00000118</td></tr><tr><td>71</td><td>57</td><td>可编程</td><td>TIM7</td><td>TIM7 全局中断</td><td>0x0000011C</td></tr><tr><td>72</td><td>58</td><td>可编程</td><td>DMA2_CH1</td><td>DMA2 通道 1 全局中断</td><td>0x00000120</td></tr><tr><td>73</td><td>59</td><td>可编程</td><td>DMA2_CH2</td><td>DMA2 通道 2 全局中断</td><td>0x00000124</td></tr><tr><td>74</td><td>60</td><td>可编程</td><td>DMA2_CH3</td><td>DMA2 通道 3 全局中断</td><td>0x00000128</td></tr><tr><td>75</td><td>61</td><td>可编程</td><td>DMA2_CH4</td><td>DMA2 通道 4 全局中断</td><td>0x0000012C</td></tr><tr><td>76</td><td>62</td><td>可编程</td><td>DMA2_CH5</td><td>DMA2 通道 5 全局中断</td><td>0x00000130</td></tr><tr><td>77</td><td>63</td><td>可编程</td><td>ETH</td><td>ETH 全局中断</td><td>0x00000134</td></tr><tr><td>78</td><td>64</td><td>可编程</td><td>ETH_WKUP</td><td>ETH 唤醒中断</td><td>0x00000138</td></tr><tr><td>79</td><td>65</td><td>可编程</td><td>CAN2_TX</td><td>CAN2_TX全局中断</td><td>0x0000013C</td></tr><tr><td>80</td><td>66</td><td>可编程</td><td>CAN2_RX0</td><td>CAN2_RX0全局中断</td><td>0x00000140</td></tr><tr><td>81</td><td>67</td><td>可编程</td><td>CAN2_RX1</td><td>CAN2_RX1全局中断</td><td>0x00000144</td></tr><tr><td>82</td><td>68</td><td>可编程</td><td>CAN2_SCE</td><td>CAN2_SCE全局中断</td><td>0x00000148</td></tr><tr><td>83</td><td>69</td><td>可编程</td><td>OTG_FS</td><td>全速OTG中断</td><td>0x0000014C</td></tr><tr><td>84</td><td>70</td><td>可编程</td><td>USBHSWakeUp</td><td>高速USB唤醒中断</td><td>0x00000150</td></tr><tr><td>85</td><td>71</td><td>可编程</td><td>USBHS</td><td>高速USB全局中断</td><td>0x00000154</td></tr><tr><td>86</td><td>72</td><td>可编程</td><td>DVP</td><td>DVP全局中断</td><td>0x00000158</td></tr><tr><td>87</td><td>73</td><td>可编程</td><td>UART6</td><td>UART6全局中断</td><td>0x0000015C</td></tr><tr><td>88</td><td>74</td><td>可编程</td><td>UART7</td><td>UART7全局中断</td><td>0x00000160</td></tr><tr><td>89</td><td>75</td><td>可编程</td><td>UART8</td><td>UART8全局中断</td><td>0x00000164</td></tr><tr><td>90</td><td>76</td><td>可编程</td><td>TIM9_BRK</td><td>TIM9刹车中断</td><td>0x00000168</td></tr><tr><td>91</td><td>77</td><td>可编程</td><td>TIM9_UP</td><td>TIM9更新中断</td><td>0x0000016C</td></tr><tr><td>92</td><td>78</td><td>可编程</td><td>TIM9_TRG_COM</td><td>TIM9触发和通信中断</td><td>0x00000170</td></tr><tr><td>93</td><td>79</td><td>可编程</td><td>TIM9_CC</td><td>TIM9捕获比较中断</td><td>0x00000174</td></tr><tr><td>94</td><td>80</td><td>可编程</td><td>TIM10_BRK</td><td>TIM10刹车中断</td><td>0x00000178</td></tr><tr><td>95</td><td>81</td><td>可编程</td><td>TIM10_UP</td><td>TIM10更新中断</td><td>0x0000017C</td></tr><tr><td>96</td><td>82</td><td>可编程</td><td>TIM10_TRG_COM</td><td>TIM10触发和通信中断</td><td>0x00000180</td></tr><tr><td>97</td><td>83</td><td>可编程</td><td>TIM10_CC</td><td>TIM10捕获比较中断</td><td>0x00000184</td></tr><tr><td>98</td><td>84</td><td>可编程</td><td>DMA2_CH6</td><td>DMA2通道6全局中断</td><td>0x00000188</td></tr><tr><td>99</td><td>85</td><td>可编程</td><td>DMA2_CH7</td><td>DMA2通道7全局中断</td><td>0x0000018C</td></tr><tr><td>100</td><td>86</td><td>可编程</td><td>DMA2_CH8</td><td>DMA2通道8全局中断</td><td>0x00000190</td></tr><tr><td>101</td><td>87</td><td>可编程</td><td>DMA2_CH9</td><td>DMA2通道9全局中断</td><td>0x00000194</td></tr><tr><td>102</td><td>88</td><td>可编程</td><td>DMA2_CH10</td><td>DMA2通道10全局中断</td><td>0x00000198</td></tr><tr><td>103</td><td>89</td><td>可编程</td><td>DMA2_CH11</td><td>DMA2通道11全局中断</td><td>0x0000019C</td></tr></table>

## 9.4 外部中断和事件控制器（EXTI）

### 9.4.1 概述

![](images/878273753a7d35dbeb5fc0f1cc7cd148e104a616c1f7b66ddf0cd11c029ef224.jpg)  
图 9-1 外部中断(EXTI)接口框图

由图 9-1可以看出，外部中断的触发源既可以是软件中断（SWIEVR）也可以是实际的外部中断通道，外部中断通道的信号会先经过边沿检测电路（edge detect circuit）的筛选。只要产生软件中断或外部中断信号其一，就会通过图中的或门电路输出给事件使能和中断使能两个与门电路，只要有中断被使能或事件被使能，就会产生中断或事件。EXTI 的六个寄存器由处理器通过 APB2 接口访问。

### 9.4.2 唤醒事件说明

系统可以通过唤醒事件来唤醒由WFE指令引起的睡眠模式。唤醒事件通过以下两种配置产生：

l 在外设的寄存器里使能一个中断，但不在内核的 NVIC 或 PFIC里使能这个中断，同时在内核里使能 SEVONPEND 位。体现在 EXTI 中，就是使能 EXTI 中断，但不在 NVIC 或 PFIC 中使能 EXTI 中断，同时使能 SEVONPEND 位。当 CPU 从 WFE 中唤醒后，需要清除 EXTI 的中断标志位和 NVIC 或 PFIC挂起位。  
使能一个 EXTI 通道为事件通道，CPU 从 WFE 唤醒后无需清除中断标志位和 NVIC 或 PFIC 挂起位的操作。

### 9.4.3 说明

使用外部中断需要配置相应外部中断通道，即选择相应触发沿，使能相应中断。当外部中断通道上出现了设定的触发沿时，将产生一个中断请求，对应的中断标志位也会被置位。对标志位写 1 可以清除该标志位。

使用外部硬件中断步骤：

1） 配置 GPIO 操作；

2） 配置对应的外部中断通道的中断使能位（EXTI_INTENR）；  
3） 配置触发沿（EXTI_RTENR 或 EXTI_FTENR），选择上升沿触发、下降沿触发或双边沿触发；  
4） 在内核的 NVIC/PFIC 中配置 EXTI 中断，以保证其可以正确响应。

使用外部硬件事件步骤：

1） 配置 GPIO 操作；  
2） 配置对应的外部中断通道的事件使能位（EXTI_EVENR）；  
3） 配置触发沿（EXTI_RTENR 或 EXTI_FTENR），选择上升沿触发、下降沿触发或双边沿触发。

使用软件中断/事件步骤：

1） 使能外部中断（EXTI_INTENR）或外部事件（EXTI_EVENR）；  
2） 如果使用中断服务函数，需要设置内核的 NVIC 或 PFIC 里 EXTI 中断；  
3） 设置软件中断触发（EXTI_SWIEVR），即会产生中断。

### 9.4.4 外部事件映射

表 9-3 EXTI 中断映射  

<table><tr><td>外部中断/事件线路</td><td>映射事件描述</td></tr><tr><td>EXT10~EXT115</td><td>Px0~Px15(x=A/B/C/D/E),任何一个I0口都可以启用外部中断/事件功能,由AF10_EXTICRx寄存器配置。</td></tr><tr><td>EXT116</td><td>PVD事件:超出电压监控阀值</td></tr><tr><td>EXT117</td><td>RTC闹钟事件</td></tr><tr><td>EXT118</td><td>USBD/USBFSOTG唤醒事件(适用CH32F20x_D8、CH32F20x_D8C、CH32V30x_D8、CH32V30x_D8C)USBD唤醒事件(其余芯片型号)</td></tr><tr><td>EXT119</td><td>ETH唤醒事件</td></tr><tr><td>EXT120</td><td>USBHS唤醒事件(适用CH32F20x_D8C、CH32V30x_D8C)USBFS唤醒事件(适用其余芯片型号)</td></tr><tr><td>EXT121</td><td>内部32K校准唤醒事件(适用于CH32V20x_D8、CH32V20x_D8W、CH32F20x_D8W)</td></tr></table>

## 9.5 寄存器描述

### 9.5.1 EXTI 寄存器描述

表 9-4 EXTI 相关寄存器列表  

<table><tr><td>名称</td><td>访问地址</td><td>描述</td><td>复位值</td></tr><tr><td>R32_EXTI_INTENR</td><td>0x40010400</td><td>中断使能寄存器</td><td>0x00000000</td></tr><tr><td>R32_EXTI_EVENR</td><td>0x40010404</td><td>事件使能寄存器</td><td>0x00000000</td></tr><tr><td>R32_EXTI_RTENR</td><td>0x40010408</td><td>上升沿触发使能寄存器</td><td>0x00000000</td></tr><tr><td>R32_EXTI_FTENR</td><td>0x4001040C</td><td>下降沿触发使能寄存器</td><td>0x00000000</td></tr><tr><td>R32_EXTI_SWIEVR</td><td>0x40010410</td><td>软中断事件寄存器</td><td>0x00000000</td></tr><tr><td>R32_EXTI_INTFR</td><td>0x40010414</td><td>中断标志位寄存器</td><td>0x0000XXXX</td></tr></table>

#### 9.5.1.1 中断使能寄存器（EXTI_INTENR）

偏移地址： $0 \times 0 0$

<table><tr><td colspan="11">Reserved</td><td>MR20</td><td>MR19</td><td>MR18</td><td>MR17</td><td>MR16</td></tr><tr><td>15</td><td>14</td><td>13</td><td>12</td><td>11</td><td>10</td><td>9</td><td>8</td><td>7</td><td>6</td><td>5</td><td>4</td><td>3</td><td>2</td><td>1</td><td>0</td></tr><tr><td>MR15</td><td>MR14</td><td>MR13</td><td>MR12</td><td>MR11</td><td>MR10</td><td>MR9</td><td>MR8</td><td>MR7</td><td>MR6</td><td>MR5</td><td>MR4</td><td>MR3</td><td>MR2</td><td>MR1</td><td>MRO</td></tr></table>

<table><tr><td>位</td><td>名称</td><td>访问</td><td>描述</td><td>复位值</td></tr><tr><td>[31:21]</td><td>Reserved</td><td>R0</td><td>保留。</td><td>0</td></tr><tr><td>[20:0]</td><td>MRx</td><td>RW</td><td>使能外部中断通道x的中断请求信号:1:使能此通道的中断;0:屏蔽此通道的中断。</td><td>0</td></tr></table>

#### 9.5.1.2 事件使能寄存器（EXTI_EVENR）

偏移地址： $0 \times 0 4$

<table><tr><td colspan="11">Reserved</td><td>MR20</td><td>MR19</td><td>MR18</td><td>MR17</td><td>MR16</td></tr><tr><td>15</td><td>14</td><td>13</td><td>12</td><td>11</td><td>10</td><td>9</td><td>8</td><td>7</td><td>6</td><td>5</td><td>4</td><td>3</td><td>2</td><td>1</td><td>0</td></tr><tr><td>MR15</td><td>MR14</td><td>MR13</td><td>MR12</td><td>MR11</td><td>MR10</td><td>MR9</td><td>MR8</td><td>MR7</td><td>MR6</td><td>MR5</td><td>MR4</td><td>MR3</td><td>MR2</td><td>MR1</td><td>MRO</td></tr></table>

<table><tr><td>位</td><td>名称</td><td>访问</td><td>描述</td><td>复位值</td></tr><tr><td>[31:21]</td><td>Reserved</td><td>R0</td><td>保留。</td><td>0</td></tr><tr><td>[20:0]</td><td>MRx</td><td>RW</td><td>使能外部中断通道x的事件请求信号:
1:使能此通道的事件;
0:屏蔽此通道的事件。</td><td>0</td></tr></table>

#### 9.5.1.3 上升沿触发使能寄存器（EXTI_RTENR）

偏移地址： $0 \times 0 8$

<table><tr><td>31</td><td>30</td><td>29</td><td>28</td><td>27</td><td>26</td><td>25</td><td>24</td><td>23</td><td>22</td><td>21</td><td>20</td><td>19</td><td>18</td><td>17</td><td>16</td></tr><tr><td colspan="11">Reserved</td><td>TR20</td><td>TR19</td><td>TR18</td><td>TR17</td><td>TR16</td></tr><tr><td>15</td><td>14</td><td>13</td><td>12</td><td>11</td><td>10</td><td>9</td><td>8</td><td>7</td><td>6</td><td>5</td><td>4</td><td>3</td><td>2</td><td>1</td><td>0</td></tr><tr><td>TR15</td><td>TR14</td><td>TR13</td><td>TR12</td><td>TR11</td><td>TR10</td><td>TR9</td><td>TR8</td><td>TR7</td><td>TR6</td><td>TR5</td><td>TR4</td><td>TR3</td><td>TR2</td><td>TR1</td><td>TR0</td></tr></table>

<table><tr><td>位</td><td>名称</td><td>访问</td><td>描述</td><td>复位值</td></tr><tr><td>[31:21]</td><td>Reserved</td><td>R0</td><td>保留。</td><td>0</td></tr><tr><td>[20:0]</td><td>TRx</td><td>RW</td><td>使能外部中断通道x的上升沿触发:1:使能此通道的上升沿触发;0:禁止此通道的上升沿触发。</td><td>0</td></tr></table>

#### 9.5.1.4 下降沿触发使能寄存器（EXTI_FTENR）

偏移地址： $0 \times 0 0$

<table><tr><td colspan="11">Reserved</td><td>TR20</td><td>TR19</td><td>TR18</td><td>TR17</td><td>TR16</td></tr><tr><td>15</td><td>14</td><td>13</td><td>12</td><td>11</td><td>10</td><td>9</td><td>8</td><td>7</td><td>6</td><td>5</td><td>4</td><td>3</td><td>2</td><td>1</td><td>0</td></tr><tr><td>TR15</td><td>TR14</td><td>TR13</td><td>TR12</td><td>TR11</td><td>TR10</td><td>TR9</td><td>TR8</td><td>TR7</td><td>TR6</td><td>TR5</td><td>TR4</td><td>TR3</td><td>TR2</td><td>TR1</td><td>TR0</td></tr></table>

<table><tr><td>位</td><td>名称</td><td>访问</td><td>描述</td><td>复位值</td></tr><tr><td>[31:21]</td><td>Reserved</td><td>R0</td><td>保留</td><td>0</td></tr><tr><td>[20:0]</td><td>TRx</td><td>RW</td><td>使能外部中断通道x的下降沿触发:0:禁止此通道的下降沿触发;1:使能此通道的下降沿触发。</td><td>0</td></tr></table>

#### 9.5.1.5 软中断事件寄存器（EXTI_SWIEVR）

偏移地址： $0 \times 1 0$

<table><tr><td colspan="11">Reserved</td><td>SWIER20</td><td>SWIER19</td><td>SWIER18</td><td>SWIER17</td><td>SWIER16</td></tr><tr><td>15</td><td>14</td><td>13</td><td>12</td><td>11</td><td>10</td><td>9</td><td>8</td><td>7</td><td>6</td><td>5</td><td>4</td><td>3</td><td>2</td><td>1</td><td>0</td></tr><tr><td>SWIER15</td><td>SWIER14</td><td>SWIER13</td><td>SWIER12</td><td>SWIER11</td><td>SWIER10</td><td>SWIER9</td><td>SWIER8</td><td>SWIER7</td><td>SWIER6</td><td>SWIER5</td><td>SWIER4</td><td>SWIER3</td><td>SWIER2</td><td>SWIER1</td><td>SWIER0</td></tr></table>

<table><tr><td>位</td><td>名称</td><td>访问</td><td>描述</td><td>复位值</td></tr><tr><td>[31:21]</td><td>Reserved</td><td>R0</td><td>保留。</td><td>0</td></tr><tr><td>[20:0]</td><td>SWIERx</td><td>RW</td><td>在相对应的外部触发中断通道上设置一个软件中断。这里置位会使中断标志位(EXTI_INTFR)对应位置位,如果中断使能(EXTI_INTENR)或事件使能(EXTI_EVENR)开启,那么就会产生中断或事件。</td><td>0</td></tr></table>

#### 9.5.1.6 中断标志位寄存器（EXTI_INTFR）

偏移地址： $0 \times 1 4$

<table><tr><td colspan="11">Reserved</td><td>IF20</td><td>IF19</td><td>IF18</td><td>IF17</td><td>IF16</td></tr><tr><td>15</td><td>14</td><td>13</td><td>12</td><td>11</td><td>10</td><td>9</td><td>8</td><td>7</td><td>6</td><td>5</td><td>4</td><td>3</td><td>2</td><td>1</td><td>0</td></tr><tr><td>IF15</td><td>IF14</td><td>IF13</td><td>IF12</td><td>IF11</td><td>IF10</td><td>IF9</td><td>IF8</td><td>IF7</td><td>IF6</td><td>IF5</td><td>IF4</td><td>IF3</td><td>IF2</td><td>IF1</td><td>IF0</td></tr></table>

<table><tr><td>位</td><td>名称</td><td>访问</td><td>描述</td><td>复位值</td></tr><tr><td>[31:21]</td><td>Reserved</td><td>R0</td><td>保留。</td><td>0</td></tr><tr><td>[20:0]</td><td>IFx</td><td>W1</td><td>中断标志位,该位置位标志表示发生了对应的外部中断。写1可以清除此位。</td><td>0</td></tr></table>

### 9.5.2 PFIC 寄存器描述

表 9-5 PFIC 相关寄存器列表  

<table><tr><td>名称</td><td>访问地址</td><td>描述</td><td>复位值</td></tr><tr><td>R32_PF_ISR1</td><td>0xE000E000</td><td>PFIC中断使能状态寄存器1</td><td>0x0000000C</td></tr><tr><td>R32_PF_ISR2</td><td>0xE000E004</td><td>PFIC中断使能状态寄存器2</td><td>0x0000000</td></tr><tr><td>R32_PF_ISR3</td><td>0xE000E008</td><td>PFIC中断使能状态寄存器3</td><td>0x0000000</td></tr><tr><td>R32_PF_ISR4</td><td>0xE000E00C</td><td>PFIC中断使能状态寄存器4</td><td>0x0000000</td></tr><tr><td>R32_PF_IPR1</td><td>0xE000E020</td><td>PFIC中断挂起状态寄存器1</td><td>0x0000000</td></tr><tr><td>R32_PF_IPR2</td><td>0xE000E024</td><td>PFIC中断挂起状态寄存器2</td><td>0x0000000</td></tr><tr><td>R32_PF_IPR3</td><td>0xE000E028</td><td>PFIC中断挂起状态寄存器3</td><td>0x0000000</td></tr><tr><td>R32_PF_IPR4</td><td>0xE000E02C</td><td>PFIC中断挂起状态寄存器4</td><td>0x0000000</td></tr><tr><td>R32_PF_ITHRESDR</td><td>0xE000E040</td><td>PFIC中断优先级阈值配置寄存器</td><td>0x0000000</td></tr><tr><td>R32_PF_CFGR</td><td>0xE000E048</td><td>PFIC中断配置寄存器</td><td>0x0000000</td></tr><tr><td>R32_PF_GISR</td><td>0xE000E04C</td><td>PFIC中断全局状态寄存器</td><td>0x0000000</td></tr><tr><td>R32_PF_VTFIDR</td><td>0xE000E050</td><td>PFIC VTF中断ID配置寄存器</td><td>0x0000000</td></tr><tr><td>R32_PF_VTFADDRR0</td><td>0xE000E060</td><td>PFIC VTF中断0偏移地址寄存器</td><td>0x0000000</td></tr><tr><td>R32_PF_VTFADDRR1</td><td>0xE000E064</td><td>PFIC VTF中断1偏移地址寄存器</td><td>0x0000000</td></tr><tr><td>R32_PF_VTFADDRR2</td><td>0xE000E068</td><td>PFIC VTF中断2偏移地址寄存器</td><td>0x0000000</td></tr><tr><td>R32_PF_VTFADDRR3</td><td>0xE000E06C</td><td>PFIC VTF中断3偏移地址寄存器</td><td>0x0000000</td></tr><tr><td>R32_PF_IENR1</td><td>0xE000E100</td><td>PFIC中断使能设置寄存器1</td><td>0x0000000</td></tr><tr><td>R32_PF_IENR2</td><td>0xE000E104</td><td>PFIC中断使能设置寄存器2</td><td>0x0000000</td></tr><tr><td>R32_PF_IENR3</td><td>0xE000E108</td><td>PFIC中断使能设置寄存器3</td><td>0x0000000</td></tr><tr><td>R32_PF_IENR4</td><td>0xE000E10C</td><td>PFIC中断使能设置寄存器4</td><td>0x0000000</td></tr><tr><td>R32_PF_IRER1</td><td>0xE000E180</td><td>PFIC中断使能清除寄存器1</td><td>0x0000000</td></tr><tr><td>R32_PF_IRER2</td><td>0xE000E184</td><td>PFIC中断使能清除寄存器2</td><td>0x0000000</td></tr><tr><td>R32_PF_IRER3</td><td>0xE000E188</td><td>PFIC中断使能清除寄存器3</td><td>0x0000000</td></tr><tr><td>R32_PF_IRER4</td><td>0xE000E18C</td><td>PFIC中断使能清除寄存器4</td><td>0x0000000</td></tr><tr><td>R32_PF_IPSR1</td><td>0xE000E200</td><td>PFIC中断挂起设置寄存器1</td><td>0x0000000</td></tr><tr><td>R32_PF_IPSR2</td><td>0xE000E204</td><td>PFIC中断挂起设置寄存器2</td><td>0x0000000</td></tr><tr><td>R32_PF_IPSR3</td><td>0xE000E208</td><td>PFIC中断挂起设置寄存器3</td><td>0x0000000</td></tr><tr><td>R32_PF_IPSR4</td><td>0xE000E20C</td><td>PFIC中断挂起设置寄存器4</td><td>0x0000000</td></tr><tr><td>R32_PF_IPRR1</td><td>0xE000E280</td><td>PFIC中断挂起清除寄存器1</td><td>0x0000000</td></tr><tr><td>R32_PF_IPRR2</td><td>0xE000E284</td><td>PFIC中断挂起清除寄存器2</td><td>0x0000000</td></tr><tr><td>R32_PF_IPRR3</td><td>0xE000E288</td><td>PFIC中断挂起清除寄存器3</td><td>0x0000000</td></tr><tr><td>R32_PF_IPRR4</td><td>0xE000E28C</td><td>PFIC中断挂起清除寄存器4</td><td>0x0000000</td></tr><tr><td>R32_PF_IACTR1</td><td>0xE000E300</td><td>PFIC中断激活状态寄存器1</td><td>0x0000000</td></tr><tr><td>R32_PF_IACTR2</td><td>0xE000E304</td><td>PFIC中断激活状态寄存器2</td><td>0x0000000</td></tr><tr><td>R32_PF_IACTR3</td><td>0xE000E308</td><td>PFIC中断激活状态寄存器3</td><td>0x0000000</td></tr><tr><td>R32_PF_IACTR4</td><td>0xE000E30C</td><td>PFIC中断激活状态寄存器4</td><td>0x0000000</td></tr><tr><td>R32_PF_IPRIORx</td><td>0xE000E400</td><td>PFIC中断优先级配置寄存器</td><td>0x0000000</td></tr><tr><td>R32_PF_SCTLR</td><td>0xE000ED10</td><td>PFIC系统控制寄存器</td><td>0x0000000</td></tr></table>

注：1.NMI、EXC、ECALL-M、ECALL-U、BREAKPOINT中断默认总是使能。  
2.ECALL-M、ECALL-U、BREAKPOINT均为 EXC的一种情况，状态由 EXC的状态位bit3表示。  
3.NMI、EXC支持中断挂起清除和设置操作，不支持中断使能清除和设置操作。  
4.ECALL-M、ECALL-U、BREAKPOINT不支持中断挂起清除和设置、中断使能清除和设置操作。

#### 9.5.2.1 PFIC 中断使能状态寄存器 1（PFIC_ISR1）

偏移地址： $0 \times 0 0$

<table><tr><td>31</td><td>30</td><td>29</td><td>28</td><td>27</td><td>26</td><td>25</td><td>24</td><td>23</td><td>22</td><td>21</td><td>20</td><td>19</td><td>18</td><td>17</td><td>16</td></tr><tr><td colspan="16">INTENSTA[31:16]</td></tr><tr><td>15</td><td>14</td><td>13</td><td>12</td><td>11</td><td>10</td><td>9</td><td>8</td><td>7</td><td>6</td><td>5</td><td>4</td><td>3</td><td>2</td><td>1</td><td>0</td></tr><tr><td>INTEN
STA15</td><td>INTEN
STA14</td><td>INTEN
STA13</td><td>INTEN
STA12</td><td colspan="8">Reserved</td><td>INTEN
STA3</td><td>INTEN
STA2</td><td colspan="2">Reserved</td></tr></table>

<table><tr><td>位</td><td>名称</td><td>访问</td><td>描述</td><td>复位值</td></tr><tr><td>[31:12]</td><td>INTENSTA</td><td>RO</td><td>12#-31#中断当前使能状态。1:当前编号中断已使能;0:当前编号中断未启用。</td><td>0</td></tr><tr><td>[11:4]</td><td>Reserved</td><td>RO</td><td>保留</td><td>0</td></tr><tr><td>[3:2]</td><td>INTENSTA</td><td>RO</td><td>2#-3#中断当前使能状态。1:当前编号中断已使能;0:当前编号中断未启用。</td><td>0</td></tr><tr><td>[1:0]</td><td>Reserved</td><td>RO</td><td>保留</td><td>0</td></tr></table>

#### 9.5.2.2 PFIC 中断使能状态寄存器 2（PFIC_ISR2）

偏移地址： $0 \times 0 4$

<table><tr><td>31</td><td>30</td><td>29</td><td>28</td><td>27</td><td>26</td><td>25</td><td>24</td><td>23</td><td>22</td><td>21</td><td>20</td><td>19</td><td>18</td><td>17</td><td>16</td></tr><tr><td colspan="16">INTENSTA[63:48]</td></tr><tr><td>15</td><td>14</td><td>13</td><td>12</td><td>11</td><td>10</td><td>9</td><td>8</td><td>7</td><td>6</td><td>5</td><td>4</td><td>3</td><td>2</td><td>1</td><td>0</td></tr><tr><td colspan="16">INTENSTA[47:32]</td></tr></table>

<table><tr><td>位</td><td>名称</td><td>访问</td><td>描述</td><td>复位值</td></tr><tr><td>[31:0]</td><td>INTENSTA</td><td>RO</td><td>32#-63#中断当前使能状态。
1: 当前编号中断已使能;
0: 当前编号中断未启用。</td><td>0</td></tr></table>

#### 9.5.2.3 PFIC 中断使能状态寄存器 3（PFIC_ISR3）

偏移地址： $0 \times 0 8$

<table><tr><td>31</td><td>30</td><td>29</td><td>28</td><td>27</td><td>26</td><td>25</td><td>24</td><td>23</td><td>22</td><td>21</td><td>20</td><td>19</td><td>18</td><td>17</td><td>16</td></tr><tr><td colspan="16">INTENSTA[95:80]</td></tr><tr><td>15</td><td>14</td><td>13</td><td>12</td><td>11</td><td>10</td><td>9</td><td>8</td><td>7</td><td>6</td><td>5</td><td>4</td><td>3</td><td>2</td><td>1</td><td>0</td></tr><tr><td colspan="16">INTENSTA[79:64]</td></tr></table>

<table><tr><td>位</td><td>名称</td><td>访问</td><td>描述</td><td>复位值</td></tr><tr><td>[31:0]</td><td>INTENSTA</td><td>RO</td><td>64#--95#中断当前使能状态。
1: 当前编号中断已使能;
0: 当前编号中断未启用。</td><td>0</td></tr></table>

#### 9.5.2.4 PFIC 中断使能状态寄存器 4（PFIC_ISR4）

偏移地址： $0 \times 0 0$

<table><tr><td>31</td><td>30</td><td>29</td><td>28</td><td>27</td><td>26</td><td>25</td><td>24</td><td>23</td><td>22</td><td>21</td><td>20</td><td>19</td><td>18</td><td>17</td><td>16</td></tr><tr><td colspan="16">Reserved</td></tr><tr><td>15</td><td>14</td><td>13</td><td>12</td><td>11</td><td>10</td><td>9</td><td>8</td><td>7</td><td>6</td><td>5</td><td>4</td><td>3</td><td>2</td><td>1</td><td>0</td></tr><tr><td colspan="8">Reserved</td><td colspan="8">INTENSTA[103:96]</td></tr></table>

<table><tr><td>位</td><td>名称</td><td>访问</td><td>描述</td><td>复位值</td></tr><tr><td>[31:8]</td><td>Reserved</td><td>R0</td><td>保留</td><td>0</td></tr><tr><td>[7:0]</td><td>INTENSTA</td><td>R0</td><td>96#--103#中断当前使能状态。
1: 当前编号中断已使能;
0: 当前编号中断未启用。</td><td>0</td></tr></table>

#### 9.5.2.5 PFIC 中断挂起状态寄存器 1（PFIC_IPR1）

偏移地址： $_ { 0 \times 2 0 }$

<table><tr><td>31</td><td>30</td><td>29</td><td>28</td><td>27</td><td>26</td><td>25</td><td>24</td><td>23</td><td>22</td><td>21</td><td>20</td><td>19</td><td>18</td><td>17</td><td>16</td></tr><tr><td colspan="16">PENDSTA[31:16]</td></tr><tr><td>15</td><td>14</td><td>13</td><td>12</td><td>11</td><td>10</td><td>9</td><td>8</td><td>7</td><td>6</td><td>5</td><td>4</td><td>3</td><td>2</td><td>1</td><td>0</td></tr><tr><td>PENDSTATA15</td><td>PENDSTATA14</td><td>PENDSTATA13</td><td>PENDSTATA12</td><td colspan="8">Reserved</td><td>PENDSTATA3</td><td>PENDSTATA2</td><td colspan="2">Reserved</td></tr></table>

<table><tr><td>位</td><td>名称</td><td>访问</td><td>描述</td><td>复位值</td></tr><tr><td>[31:12]</td><td>PENDSTA</td><td>RO</td><td>12#-31#中断当前挂起状态。1:当前编号中断已挂起;0:当前编号中断未挂起。</td><td>0</td></tr><tr><td>[11:4]</td><td>Reserved</td><td>RO</td><td>保留。</td><td>0</td></tr><tr><td>[3:2]</td><td>PENDSTA</td><td>RO</td><td>2#-3#中断当前挂起状态。1:当前编号中断已挂起;0:当前编号中断未挂起。</td><td>0</td></tr><tr><td>[1:0]</td><td>Reserved</td><td>RO</td><td>保留。</td><td>0</td></tr></table>

#### 9.5.2.6 PFIC 中断挂起状态寄存器 2（PFIC_IPR2）

偏移地址： ${ 0 } { \times 2 4 }$

<table><tr><td>31</td><td>30</td><td>29</td><td>28</td><td>27</td><td>26</td><td>25</td><td>24</td><td>23</td><td>22</td><td>21</td><td>20</td><td>19</td><td>18</td><td>17</td><td>16</td></tr><tr><td colspan="16">PENDSTA[63:48]</td></tr><tr><td>15</td><td>14</td><td>13</td><td>12</td><td>11</td><td>10</td><td>9</td><td>8</td><td>7</td><td>6</td><td>5</td><td>4</td><td>3</td><td>2</td><td>1</td><td>0</td></tr><tr><td colspan="16">PENDSTA[47:32]</td></tr></table>

<table><tr><td>位</td><td>名称</td><td>访问</td><td>描述</td><td>复位值</td></tr><tr><td>[31:0]</td><td>PENDSTA</td><td>RO</td><td>32#-63#中断当前挂起状态。</td><td>0</td></tr><tr><td></td><td></td><td></td><td>1: 当前编号中断已挂起;0: 当前编号中断未挂起。</td><td></td></tr></table>

#### 9.5.2.7 PFIC 中断挂起状态寄存器 3（PFIC_IPR3）

偏移地址： $_ { 0 \times 2 8 }$

<table><tr><td>31</td><td>30</td><td>29</td><td>28</td><td>27</td><td>26</td><td>25</td><td>24</td><td>23</td><td>22</td><td>21</td><td>20</td><td>19</td><td>18</td><td>17</td><td>16</td></tr><tr><td colspan="16">PENDSTA[95:80]</td></tr><tr><td>15</td><td>14</td><td>13</td><td>12</td><td>11</td><td>10</td><td>9</td><td>8</td><td>7</td><td>6</td><td>5</td><td>4</td><td>3</td><td>2</td><td>1</td><td>0</td></tr><tr><td colspan="16">PENDSTA[79:64]</td></tr></table>

<table><tr><td>位</td><td>名称</td><td>访问</td><td>描述</td><td>复位值</td></tr><tr><td>[31:0]</td><td>PENDSTA</td><td>RO</td><td>64#--95#中断当前挂起状态。
1: 当前编号中断已挂起;
0: 当前编号中断未挂起。</td><td>0</td></tr></table>

#### 9.5.2.8 PFIC 中断挂起状态寄存器 4（PFIC_IPR4）

偏移地址： $\mathtt { 0 } \mathtt { x } 2 0$

<table><tr><td>31</td><td>30</td><td>29</td><td>28</td><td>27</td><td>26</td><td>25</td><td>24</td><td>23</td><td>22</td><td>21</td><td>20</td><td>19</td><td>18</td><td>17</td><td>16</td></tr><tr><td colspan="16">Reserved</td></tr><tr><td>15</td><td>14</td><td>13</td><td>12</td><td>11</td><td>10</td><td>9</td><td>8</td><td>7</td><td>6</td><td>5</td><td>4</td><td>3</td><td>2</td><td>1</td><td>0</td></tr><tr><td colspan="8">Reserved</td><td colspan="8">PENDSTA[103:96]</td></tr></table>

<table><tr><td>位</td><td>名称</td><td>访问</td><td>描述</td><td>复位值</td></tr><tr><td>[31:8]</td><td>Reserved</td><td>R0</td><td>保留。</td><td>0</td></tr><tr><td>[7:0]</td><td>PENDSTA</td><td>R0</td><td>96#--103#中断当前挂起状态。
1: 当前编号中断已挂起;
0: 当前编号中断未挂起。</td><td>0</td></tr></table>

#### 9.5.2.9 PFIC 中断优先级阈值配置寄存器（PFIC_ITHRESDR）

偏移地址： $0 \times 4 0$

31 30 29 28 27 26 25 24 23 22 21 20 19 18 17 16 15 14 13 12 11 10 9 8 7 6 5 4 3 2 1 0

<table><tr><td>Reserved</td><td>THRESHOLD[7:0]</td></tr></table>

<table><tr><td>位</td><td>名称</td><td>访问</td><td>描述</td><td>复位值</td></tr><tr><td>[31:8]</td><td>Reserved</td><td>R0</td><td>保留。</td><td>0</td></tr><tr><td>[7:0]</td><td>THRESHOLD</td><td>RW</td><td>中断优先级阈值设置值。低于当前设置值的中断优先级值，当挂起时不执行中断服务；此寄存器为0时表示阈值寄存器功能无效。[7:4]：优先级阈值。[3:0]：保留，固定为0，写无效。</td><td>0</td></tr></table>

#### 9.5.2.10 PFIC 中断配置寄存器（PFIC_CFGR）

偏移地址： $0 \times 4 8$

<table><tr><td>31</td><td>30</td><td>29</td><td>28</td><td>27</td><td>26</td><td>25</td><td>24</td><td>23</td><td>22</td><td>21</td><td>20</td><td>19</td><td>18</td><td>17</td><td>16</td></tr><tr><td colspan="16">KEYCODE[15:0]</td></tr><tr><td>15</td><td>14</td><td>13</td><td>12</td><td>11</td><td>10</td><td>9</td><td>8</td><td>7</td><td>6</td><td>5</td><td>4</td><td>3</td><td>2</td><td>1</td><td>0</td></tr><tr><td colspan="8">Reserved</td><td>RSTSY</td><td colspan="7">Reserved</td></tr></table>

<table><tr><td>位</td><td>名称</td><td>访问</td><td>描述</td><td>复位值</td></tr><tr><td>[31:16]</td><td>KEYCODE</td><td>WO</td><td>对应不同的目标控制位,需要同步写入相应的安全访问标识数据才能修改,读出数据固定为0。KEY1 = 0xFE05;KEY2 = 0xBCAF;KEY3 = 0xBEEF。</td><td>0</td></tr><tr><td>[15:8]</td><td>Reserved</td><td>RO</td><td>保留。</td><td>0</td></tr><tr><td>7</td><td>RSTSYS</td><td>WO</td><td>系统复位(同步写入KEY3)。自动清0。写1有效,写0无效。注:与PFIC_SCTLR寄存器SYSRST位作用相同。</td><td>0</td></tr><tr><td>[6:0]</td><td>Reserved</td><td>RO</td><td>保留。</td><td>0</td></tr></table>

#### 9.5.2.11 PFIC 中断全局状态寄存器（PFIC_GISR）

偏移地址： $\mathtt { 0 } \mathtt { x } 4 0$

<table><tr><td>31</td><td>30</td><td>29</td><td>28</td><td>27</td><td>26</td><td>25</td><td>24</td><td>23</td><td>22</td><td>21</td><td>20</td><td>19</td><td>18</td><td>17</td><td>16</td></tr><tr><td colspan="16">Reserved</td></tr><tr><td>15</td><td>14</td><td>13</td><td>12</td><td>11</td><td>10</td><td>9</td><td>8</td><td>7</td><td>6</td><td>5</td><td>4</td><td>3</td><td>2</td><td>1</td><td>0</td></tr><tr><td colspan="6">Reserved</td><td>GPEND STA</td><td>GACT STA</td><td colspan="8">NESTSTA[7:0]</td></tr></table>

<table><tr><td>位</td><td>名称</td><td>访问</td><td>描述</td><td>复位值</td></tr><tr><td>[31:10]</td><td>Reserved</td><td>R0</td><td>保留。</td><td>0</td></tr><tr><td>9</td><td>GPENDSTA</td><td>R0</td><td>当前是否有中断处于挂起:1:有; 0:没有。</td><td>0</td></tr><tr><td>8</td><td>GACTSTA</td><td>R0</td><td>当前是否有中断被执行:1:有; 0:没有。</td><td>0</td></tr><tr><td>[7:0]</td><td>NESTSTA</td><td>R0</td><td>当前中断嵌套状态,目前最大支持8级嵌套,硬件压栈深度最大为3级,若设置嵌套深度大于3级,则应配置低三级中断为硬件压栈,其余高优先级使用软件压栈。0xFF:第8级中断中;0x7F:第7级中断中;0x3F:第6级中断中;0x1F:第5级中断中;0x0F:第4级中断中;</td><td>0x00</td></tr><tr><td></td><td></td><td></td><td>0x07:第3级中断中;0x03:第2级中断中;0x01:第1级中断中;0x00:没有中断发生;其他:不可能情况。注:适用于青棵V4F内核:CH32V30x_D8、CH32V30x_D8C。当前中断嵌套状态,目前最大支持2级嵌套,硬件压栈深度最大为2级。0x03:第2级中断中;0x01:第1级中断中;0x00:没有中断发生;其他:不可能情况。注:适用于青棵V4B、V4C内核:CH32V20x_D6、CH32V20x_D8、CH32V20x_D8W。</td><td></td></tr></table>

#### 9.5.2.12 PFIC VTF 中断 ID 配置寄存器（PFIC_VTFIDR）

偏移地址： $_ { 0 \times 5 0 }$

<table><tr><td>31</td><td>30</td><td>29</td><td>28</td><td>27</td><td>26</td><td>25</td><td>24</td><td>23</td><td>22</td><td>21</td><td>20</td><td>19</td><td>18</td><td>17</td><td>16</td></tr><tr><td colspan="8">VTFID3</td><td colspan="8">VTFID2</td></tr><tr><td>15</td><td>14</td><td>13</td><td>12</td><td>11</td><td>10</td><td>9</td><td>8</td><td>7</td><td>6</td><td>5</td><td>4</td><td>3</td><td>2</td><td>1</td><td>0</td></tr><tr><td colspan="8">VTFID1</td><td colspan="8">VTFID0</td></tr></table>

<table><tr><td>位</td><td>名称</td><td>访问</td><td>描述</td><td>复位值</td></tr><tr><td>[31:24]</td><td>VTF ID3</td><td>RW</td><td>配置VTF中断3的中断编号。</td><td>0</td></tr><tr><td>[23:16]</td><td>VTF ID2</td><td>RW</td><td>配置VTF中断2的中断编号。</td><td>0</td></tr><tr><td>[15:8]</td><td>VTF ID1</td><td>RW</td><td>配置VTF中断1的中断编号。</td><td>0</td></tr><tr><td>[7:0]</td><td>VTF ID0</td><td>RW</td><td>配置VTF中断0的中断编号。</td><td>0</td></tr></table>

#### 9.5.2.13 PFIC VTF 中断 0 地址寄存器（PFIC_VTFADDRR0）

偏移地址： $0 \times 6 0$

<table><tr><td>31</td><td>30</td><td>29</td><td>28</td><td>27</td><td>26</td><td>25</td><td>24</td><td>23</td><td>22</td><td>21</td><td>20</td><td>19</td><td>18</td><td>17</td><td>16</td></tr><tr><td colspan="16">ADDR0[31:16]</td></tr><tr><td>15</td><td>14</td><td>13</td><td>12</td><td>11</td><td>10</td><td>9</td><td>8</td><td>7</td><td>6</td><td>5</td><td>4</td><td>3</td><td>2</td><td>1</td><td>0</td></tr><tr><td colspan="15">ADDR0[15:1]</td><td>VTF0EN</td></tr></table>

<table><tr><td>位</td><td>名称</td><td>访问</td><td>描述</td><td>复位值</td></tr><tr><td>[31:1]</td><td>ADDR0</td><td>RW</td><td>VTF中断0服务程序地址bit[31:1],bit0为0。</td><td>0</td></tr><tr><td>0</td><td>VTF0EN</td><td>RW</td><td>VTF中断0使能位:1:启用VTF中断0通道;0:关闭。</td><td>0</td></tr></table>

#### 9.5.2.14 PFIC VTF 中断 1 地址寄存器（PFIC_VTFADDRR1）

偏移地址： $0 \times 6 4$

<table><tr><td>31</td><td>30</td><td>29</td><td>28</td><td>27</td><td>26</td><td>25</td><td>24</td><td>23</td><td>22</td><td>21</td><td>20</td><td>19</td><td>18</td><td>17</td><td>16</td></tr><tr><td colspan="16">ADDR1 [31:16]</td></tr><tr><td>15</td><td>14</td><td>13</td><td>12</td><td>11</td><td>10</td><td>9</td><td>8</td><td>7</td><td>6</td><td>5</td><td>4</td><td>3</td><td>2</td><td>1</td><td>0</td></tr><tr><td colspan="15">ADDR1 [15:1]</td><td>VTF1EN</td></tr></table>

<table><tr><td>位</td><td>名称</td><td>访问</td><td>描述</td><td>复位值</td></tr><tr><td>[31:1]</td><td>ADDR1</td><td>RW</td><td>VTF中断1服务程序地址bit[31:1],bit0为0。</td><td>0</td></tr><tr><td>0</td><td>VTF1EN</td><td>RW</td><td>VTF中断1使能位:1:启用VTF中断1通道;0:关闭。</td><td>0</td></tr></table>

#### 9.5.2.15 PFIC VTF 中断 2 地址寄存器（PFIC_VTFADDRR2）

偏移地址： $0 \times 6 8$

<table><tr><td>31</td><td>30</td><td>29</td><td>28</td><td>27</td><td>26</td><td>25</td><td>24</td><td>23</td><td>22</td><td>21</td><td>20</td><td>19</td><td>18</td><td>17</td><td>16</td></tr><tr><td colspan="16">ADDR2[31:16]</td></tr><tr><td>15</td><td>14</td><td>13</td><td>12</td><td>11</td><td>10</td><td>9</td><td>8</td><td>7</td><td>6</td><td>5</td><td>4</td><td>3</td><td>2</td><td>1</td><td>0</td></tr><tr><td colspan="15">ADDR2[15:1]</td><td>VTF2EN</td></tr></table>

<table><tr><td>位</td><td>名称</td><td>访问</td><td>描述</td><td>复位值</td></tr><tr><td>[31:1]</td><td>ADDR2</td><td>RW</td><td>VTF中断2服务程序地址bit[31:1],bit0为0。</td><td>0</td></tr><tr><td>0</td><td>VTF2EN</td><td>RW</td><td>VTF中断2使能位:1:启用VTF中断2通道;0:关闭。</td><td>0</td></tr></table>

#### 9.5.2.16 PFIC VTF 中断 3 地址寄存器（PFIC_VTFADDRR3）

偏移地址： $_ { 0 \times 6 0 }$

<table><tr><td>31</td><td>30</td><td>29</td><td>28</td><td>27</td><td>26</td><td>25</td><td>24</td><td>23</td><td>22</td><td>21</td><td>20</td><td>19</td><td>18</td><td>17</td><td>16</td></tr><tr><td colspan="16">ADDR3[31:16]</td></tr><tr><td>15</td><td>14</td><td>13</td><td>12</td><td>11</td><td>10</td><td>9</td><td>8</td><td>7</td><td>6</td><td>5</td><td>4</td><td>3</td><td>2</td><td>1</td><td>0</td></tr><tr><td colspan="15">ADDR3[15:1]</td><td>VTF3EN</td></tr></table>

<table><tr><td>位</td><td>名称</td><td>访问</td><td>描述</td><td>复位值</td></tr><tr><td>[31:1]</td><td>ADDR3</td><td>RW</td><td>VTF中断3服务程序地址bit[31:1],bit0为0。</td><td>0</td></tr><tr><td>0</td><td>VTF3EN</td><td>RW</td><td>VTF中断3使能位:1:启用VTF中断3通道;0:关闭。</td><td>0</td></tr></table>

#### 9.5.2.17 PFIC 中断使能设置寄存器 1（PFIC_IENR1）

偏移地址： $0 \times 1 0 0$

<table><tr><td>31</td><td>30</td><td>29</td><td>28</td><td>27</td><td>26</td><td>25</td><td>24</td><td>23</td><td>22</td><td>21</td><td>20</td><td>19</td><td>18</td><td>17</td><td>16</td></tr><tr><td colspan="16">INTEN[31:16]</td></tr><tr><td>15</td><td>14</td><td>13</td><td>12</td><td>11</td><td>10</td><td>9</td><td>8</td><td>7</td><td>6</td><td>5</td><td>4</td><td>3</td><td>2</td><td>1</td><td>0</td></tr><tr><td>INTEN15</td><td>INTEN14</td><td>INTEN13</td><td>INTEN12</td><td colspan="12">Reserved</td></tr></table>

<table><tr><td>位</td><td>名称</td><td>访问</td><td>描述</td><td>复位值</td></tr><tr><td>[31:12]</td><td>INTEN</td><td>WO</td><td>12#-31#中断使能控制。
1: 当前编号中断使能;
0: 无影响。</td><td>0</td></tr><tr><td>[11:0]</td><td>Reserved</td><td>RO</td><td>保留。</td><td>0</td></tr></table>

#### 9.5.2.18 PFIC 中断使能设置寄存器 2（PFIC_IENR2）

偏移地址： $0 \times 1 0 4$

<table><tr><td>31</td><td>30</td><td>29</td><td>28</td><td>27</td><td>26</td><td>25</td><td>24</td><td>23</td><td>22</td><td>21</td><td>20</td><td>19</td><td>18</td><td>17</td><td>16</td></tr><tr><td colspan="16">INTEN[63:48]</td></tr><tr><td>15</td><td>14</td><td>13</td><td>12</td><td>11</td><td>10</td><td>9</td><td>8</td><td>7</td><td>6</td><td>5</td><td>4</td><td>3</td><td>2</td><td>1</td><td>0</td></tr><tr><td colspan="16">INTEN[47:32]</td></tr></table>

<table><tr><td>位</td><td>名称</td><td>访问</td><td>描述</td><td>复位值</td></tr><tr><td>[31:0]</td><td>INTEN</td><td>WO</td><td>32#-63#中断使能控制。
1: 当前编号中断使能;
0: 无影响。</td><td>0</td></tr></table>

#### 9.5.2.19 PFIC 中断使能设置寄存器 3（PFIC_IENR3）

偏移地址： $0 \times 1 0 8$

<table><tr><td>31</td><td>30</td><td>29</td><td>28</td><td>27</td><td>26</td><td>25</td><td>24</td><td>23</td><td>22</td><td>21</td><td>20</td><td>19</td><td>18</td><td>17</td><td>16</td></tr><tr><td colspan="16">INTEN[95:80]</td></tr><tr><td>15</td><td>14</td><td>13</td><td>12</td><td>11</td><td>10</td><td>9</td><td>8</td><td>7</td><td>6</td><td>5</td><td>4</td><td>3</td><td>2</td><td>1</td><td>0</td></tr><tr><td colspan="16">INTEN[79:64]</td></tr></table>

<table><tr><td>位</td><td>名称</td><td>访问</td><td>描述</td><td>复位值</td></tr><tr><td>[31:0]</td><td>INTEN</td><td>WO</td><td>64#--95#中断使能控制。
1: 当前编号中断使能;
0: 无影响。</td><td>0</td></tr></table>

#### 9.5.2.20 PFIC 中断使能设置寄存器 4（PFIC_IENR4）

偏移地址： $0 \times 1 0 0$

<table><tr><td>31</td><td>30</td><td>29</td><td>28</td><td>27</td><td>26</td><td>25</td><td>24</td><td>23</td><td>22</td><td>21</td><td>20</td><td>19</td><td>18</td><td>17</td><td>16</td></tr><tr><td colspan="16">Reserved</td></tr><tr><td>15</td><td>14</td><td>13</td><td>12</td><td>11</td><td>10</td><td>9</td><td>8</td><td>7</td><td>6</td><td>5</td><td>4</td><td>3</td><td>2</td><td>1</td><td>0</td></tr><tr><td colspan="8">Reserved</td><td colspan="8">INTEN[103:96]</td></tr></table>

<table><tr><td>位</td><td>名称</td><td>访问</td><td>描述</td><td>复位值</td></tr><tr><td>[31:8]</td><td>Reserved</td><td>R0</td><td>保留。</td><td>0</td></tr><tr><td>[7:0]</td><td>INTEN</td><td>WO</td><td>96#--103#中断使能控制。
1: 当前编号中断使能;
0: 无影响。</td><td>0</td></tr></table>

#### 9.5.2.21 PFIC 中断使能清除寄存器 1（PFIC_IRER1）

偏移地址： $\mathtt { 0 } \times 1 8 0$

<table><tr><td>31</td><td>30</td><td>29</td><td>28</td><td>27</td><td>26</td><td>25</td><td>24</td><td>23</td><td>22</td><td>21</td><td>20</td><td>19</td><td>18</td><td>17</td><td>16</td></tr><tr><td colspan="16">INTRST[31:16]</td></tr><tr><td>15</td><td>14</td><td>13</td><td>12</td><td>11</td><td>10</td><td>9</td><td>8</td><td>7</td><td>6</td><td>5</td><td>4</td><td>3</td><td>2</td><td>1</td><td>0</td></tr><tr><td>INTRSET15</td><td>INTRSET14</td><td>INTRSET13</td><td>INTRSET12</td><td colspan="12">Reserved</td></tr></table>

<table><tr><td>位</td><td>名称</td><td>访问</td><td>描述</td><td>复位值</td></tr><tr><td>[31:12]</td><td>INTRSET</td><td>WO</td><td>12#-31#中断关闭控制。1:当前编号中断关闭;0:无影响。</td><td>0</td></tr><tr><td>[11:0]</td><td>Reserved</td><td>RO</td><td>保留。</td><td>0</td></tr></table>

#### 9.5.2.22 PFIC 中断使能清除寄存器 2（PFIC_IRER2）

偏移地址： $0 \times 1 8 4$

<table><tr><td>31</td><td>30</td><td>29</td><td>28</td><td>27</td><td>26</td><td>25</td><td>24</td><td>23</td><td>22</td><td>21</td><td>20</td><td>19</td><td>18</td><td>17</td><td>16</td></tr><tr><td colspan="16">INTRSET[63:48]</td></tr><tr><td>15</td><td>14</td><td>13</td><td>12</td><td>11</td><td>10</td><td>9</td><td>8</td><td>7</td><td>6</td><td>5</td><td>4</td><td>3</td><td>2</td><td>1</td><td>0</td></tr><tr><td colspan="16">INTRSET[47:32]</td></tr></table>

<table><tr><td>位</td><td>名称</td><td>访问</td><td>描述</td><td>复位值</td></tr><tr><td>[31:0]</td><td>INTRSET</td><td>WO</td><td>32#-63#中断关闭控制。1:当前编号中断关闭;0:无影响。</td><td>0</td></tr></table>

#### 9.5.2.23 PFIC 中断使能清除寄存器 3（PFIC_IRER3）

偏移地址： $0 \times 1 8 8$

<table><tr><td>31</td><td>30</td><td>29</td><td>28</td><td>27</td><td>26</td><td>25</td><td>24</td><td>23</td><td>22</td><td>21</td><td>20</td><td>19</td><td>18</td><td>17</td><td>16</td></tr><tr><td colspan="16">INTRSET[95:80]</td></tr><tr><td>15</td><td>14</td><td>13</td><td>12</td><td>11</td><td>10</td><td>9</td><td>8</td><td>7</td><td>6</td><td>5</td><td>4</td><td>3</td><td>2</td><td>1</td><td>0</td></tr><tr><td colspan="16">INTRSET[79:64]</td></tr></table>

<table><tr><td>位</td><td>名称</td><td>访问</td><td>描述</td><td>复位值</td></tr><tr><td>[31:0]</td><td>INTRSET</td><td>WO</td><td>64#-95#中断关闭控制。
1: 当前编号中断关闭;
0: 无影响。</td><td>0</td></tr></table>

#### 9.5.2.24 PFIC 中断使能清除寄存器 4（PFIC_IRER4）

偏移地址： $\mathtt { 0 } \mathtt { x } 1 8 0$

<table><tr><td>31</td><td>30</td><td>29</td><td>28</td><td>27</td><td>26</td><td>25</td><td>24</td><td>23</td><td>22</td><td>21</td><td>20</td><td>19</td><td>18</td><td>17</td><td>16</td></tr><tr><td colspan="16">Reserved</td></tr><tr><td>15</td><td>14</td><td>13</td><td>12</td><td>11</td><td>10</td><td>9</td><td>8</td><td>7</td><td>6</td><td>5</td><td>4</td><td>3</td><td>2</td><td>1</td><td>0</td></tr><tr><td colspan="8">Reserved</td><td colspan="8">INTRST [103:96]</td></tr></table>

<table><tr><td>位</td><td>名称</td><td>访问</td><td>描述</td><td>复位值</td></tr><tr><td>[31:8]</td><td>Reserved</td><td>R0</td><td>保留。</td><td>0</td></tr><tr><td>[7:0]</td><td>INTRST</td><td>WO</td><td>96#--103#中断关闭控制。
1: 当前编号中断关闭;
0: 无影响。</td><td>0</td></tr></table>

#### 9.5.2.25 PFIC 中断挂起设置寄存器 1（PFIC_IPSR1）

偏移地址： $\mathtt { 0 \times 2 0 0 }$

<table><tr><td>31</td><td>30</td><td>29</td><td>28</td><td>27</td><td>26</td><td>25</td><td>24</td><td>23</td><td>22</td><td>21</td><td>20</td><td>19</td><td>18</td><td>17</td><td>16</td></tr><tr><td colspan="16">PENDSET[31:16]</td></tr><tr><td>15</td><td>14</td><td>13</td><td>12</td><td>11</td><td>10</td><td>9</td><td>8</td><td>7</td><td>6</td><td>5</td><td>4</td><td>3</td><td>2</td><td>1</td><td>0</td></tr><tr><td>PENDSET15</td><td>PENDSET14</td><td>PENDSET13</td><td>PENDSET12</td><td colspan="8">Reserved</td><td>PENDSET3</td><td>PENDSET2</td><td colspan="2">Reserved</td></tr></table>

<table><tr><td>位</td><td>名称</td><td>访问</td><td>描述</td><td>复位值</td></tr><tr><td>[31:12]</td><td>PENDSET</td><td>WO</td><td>12#-31#中断挂起设置,13#和15#保留。1:当前编号中断挂起;0:无影响。</td><td>0</td></tr><tr><td>[11:4]</td><td>Reserved</td><td>RO</td><td>保留。</td><td>0</td></tr><tr><td>[3:2]</td><td>PENDSET</td><td>WO</td><td>2#-3#中断挂起设置。1:当前编号中断挂起;0:无影响。</td><td>0</td></tr><tr><td>[1:0]</td><td>Reserved</td><td>R0</td><td>保留。</td><td>0</td></tr></table>

#### 9.5.2.26 PFIC 中断挂起设置寄存器 2（PFIC_IPSR2）

偏移地址： $0 \times 2 0 4$

<table><tr><td>31</td><td>30</td><td>29</td><td>28</td><td>27</td><td>26</td><td>25</td><td>24</td><td>23</td><td>22</td><td>21</td><td>20</td><td>19</td><td>18</td><td>17</td><td>16</td></tr><tr><td colspan="16">PENDSET[63:48]</td></tr><tr><td>15</td><td>14</td><td>13</td><td>12</td><td>11</td><td>10</td><td>9</td><td>8</td><td>7</td><td>6</td><td>5</td><td>4</td><td>3</td><td>2</td><td>1</td><td>0</td></tr><tr><td colspan="16">PENDSET[47:32]</td></tr></table>

<table><tr><td>位</td><td>名称</td><td>访问</td><td>描述</td><td>复位值</td></tr><tr><td>[31:0]</td><td>PENDSET</td><td>WO</td><td>32#-63#中断挂起设置。
1: 当前编号中断挂起;
0: 无影响。</td><td>0</td></tr></table>

#### 9.5.2.27 PFIC 中断挂起设置寄存器 3（PFIC_IPSR3）

偏移地址： $_ { 0 \times 2 0 8 }$

<table><tr><td>31</td><td>30</td><td>29</td><td>28</td><td>27</td><td>26</td><td>25</td><td>24</td><td>23</td><td>22</td><td>21</td><td>20</td><td>19</td><td>18</td><td>17</td><td>16</td></tr><tr><td colspan="16">PENDSET[95:80]</td></tr><tr><td>15</td><td>14</td><td>13</td><td>12</td><td>11</td><td>10</td><td>9</td><td>8</td><td>7</td><td>6</td><td>5</td><td>4</td><td>3</td><td>2</td><td>1</td><td>0</td></tr><tr><td colspan="16">PENDSET[79:64]</td></tr></table>

<table><tr><td>位</td><td>名称</td><td>访问</td><td>描述</td><td>复位值</td></tr><tr><td>[31:0]</td><td>PENDSET</td><td>WO</td><td>64#-95#中断挂起设置。
1: 当前编号中断挂起;
0: 无影响。</td><td>0</td></tr></table>

#### 9.5.2.28 PFIC 中断挂起设置寄存器 4（PFIC_IPSR4）

偏移地址： $_ { 0 \times 2 0 0 }$

<table><tr><td>31</td><td>30</td><td>29</td><td>28</td><td>27</td><td>26</td><td>25</td><td>24</td><td>23</td><td>22</td><td>21</td><td>20</td><td>19</td><td>18</td><td>17</td><td>16</td></tr><tr><td colspan="16">Reserved</td></tr><tr><td>15</td><td>14</td><td>13</td><td>12</td><td>11</td><td>10</td><td>9</td><td>8</td><td>7</td><td>6</td><td>5</td><td>4</td><td>3</td><td>2</td><td>1</td><td>0</td></tr><tr><td colspan="8">Reserved</td><td colspan="8">PENDSET [103:96]</td></tr></table>

<table><tr><td>位</td><td>名称</td><td>访问</td><td>描述</td><td>复位值</td></tr><tr><td>[31:8]</td><td>Reserved</td><td>R0</td><td>保留。</td><td>0</td></tr><tr><td>[7:0]</td><td>PENDSET</td><td>WO</td><td>96#--103#中断挂起设置。
1: 当前编号中断挂起;
0: 无影响。</td><td>0</td></tr></table>

#### 9.5.2.29 PFIC 中断挂起清除寄存器 1（PFIC_IPRR1）

偏移地址： $\mathtt { 0 } \mathtt { x } 2 8 0$

<table><tr><td>31</td><td>30</td><td>29</td><td>28</td><td>27</td><td>26</td><td>25</td><td>24</td><td>23</td><td>22</td><td>21</td><td>20</td><td>19</td><td>18</td><td>17</td><td>16</td></tr><tr><td colspan="16">PENDRST[31:16]</td></tr><tr><td>15</td><td>14</td><td>13</td><td>12</td><td>11</td><td>10</td><td>9</td><td>8</td><td>7</td><td>6</td><td>5</td><td>4</td><td>3</td><td>2</td><td>1</td><td>0</td></tr><tr><td>PENDRST15</td><td>PENDRST14</td><td>PENDRST13</td><td>PENDRST12</td><td colspan="8">Reserved</td><td>PENDRST3</td><td>PENDRST2</td><td colspan="2">Reserved</td></tr></table>

<table><tr><td>位</td><td>名称</td><td>访问</td><td>描述</td><td>复位值</td></tr><tr><td>[31:12]</td><td>PENDRST</td><td>WO</td><td>12#-31#中断挂起清除,13#和15#保留。1:当前编号中断清除挂起状态;0:无影响。</td><td>0</td></tr><tr><td>[11:4]</td><td>Reserved</td><td>RO</td><td>保留。</td><td>0</td></tr><tr><td>[3:2]</td><td>PENDRST</td><td>WO</td><td>2#-3#中断挂起清除。1:当前编号中断清除挂起状态;0:无影响。</td><td>0</td></tr><tr><td>[1:0]</td><td>Reserved</td><td>RO</td><td>保留。</td><td>0</td></tr></table>

#### 9.5.2.30 PFIC 中断挂起清除寄存器 2（PFIC_IPRR2）

偏移地址： $0 \times 2 8 4$

<table><tr><td>31</td><td>30</td><td>29</td><td>28</td><td>27</td><td>26</td><td>25</td><td>24</td><td>23</td><td>22</td><td>21</td><td>20</td><td>19</td><td>18</td><td>17</td><td>16</td></tr><tr><td colspan="16">PENDRST[63:48]</td></tr><tr><td>15</td><td>14</td><td>13</td><td>12</td><td>11</td><td>10</td><td>9</td><td>8</td><td>7</td><td>6</td><td>5</td><td>4</td><td>3</td><td>2</td><td>1</td><td>0</td></tr><tr><td colspan="16">PENDRST[47:32]</td></tr></table>

<table><tr><td>位</td><td>名称</td><td>访问</td><td>描述</td><td>复位值</td></tr><tr><td>[31:0]</td><td>PENDRST</td><td>WO</td><td>32#-63#中断挂起清除。
1: 当前编号中断清除挂起状态;
0: 无影响。</td><td>0</td></tr></table>

#### 9.5.2.31 PFIC 中断挂起清除寄存器 3（PFIC_IPRR3）

偏移地址： $_ { 0 \times 2 8 8 }$

<table><tr><td>31</td><td>30</td><td>29</td><td>28</td><td>27</td><td>26</td><td>25</td><td>24</td><td>23</td><td>22</td><td>21</td><td>20</td><td>19</td><td>18</td><td>17</td><td>16</td></tr><tr><td colspan="16">PENDRST[95:80]</td></tr><tr><td>15</td><td>14</td><td>13</td><td>12</td><td>11</td><td>10</td><td>9</td><td>8</td><td>7</td><td>6</td><td>5</td><td>4</td><td>3</td><td>2</td><td>1</td><td>0</td></tr><tr><td colspan="16">PENDRST[79:64]</td></tr></table>

<table><tr><td>位</td><td>名称</td><td>访问</td><td>描述</td><td>复位值</td></tr><tr><td>[31:0]</td><td>PENDRST</td><td>WO</td><td>64#--95#中断挂起清除。
1: 当前编号中断清除挂起状态;
0: 无影响。</td><td>0</td></tr></table>

#### 9.5.2.32 PFIC 中断挂起清除寄存器 4（PFIC_IPRR4）

偏移地址： $\mathtt { 0 \times 2 8 0 }$

<table><tr><td>31</td><td>30</td><td>29</td><td>28</td><td>27</td><td>26</td><td>25</td><td>24</td><td>23</td><td>22</td><td>21</td><td>20</td><td>19</td><td>18</td><td>17</td><td>16</td></tr><tr><td colspan="16">Reserved</td></tr><tr><td>15</td><td>14</td><td>13</td><td>12</td><td>11</td><td>10</td><td>9</td><td>8</td><td>7</td><td>6</td><td>5</td><td>4</td><td>3</td><td>2</td><td>1</td><td>0</td></tr><tr><td colspan="8">Reserved</td><td colspan="8">PENDRST [103:96]</td></tr></table>

<table><tr><td>位</td><td>名称</td><td>访问</td><td>描述</td><td>复位值</td></tr><tr><td>[31:8]</td><td>Reserved</td><td>R0</td><td>保留。</td><td>0</td></tr><tr><td>[7:0]</td><td>PENSET</td><td>WO</td><td>96#--103#中断挂起清除。
1: 当前编号中断清除挂起状态;
0: 无影响</td><td>0</td></tr></table>

#### 9.5.2.33 PFIC 中断激活状态寄存器 1（PFIC_IACTR1）

偏移地址： $0 \times 3 0 0$

<table><tr><td>31</td><td>30</td><td>29</td><td>28</td><td>27</td><td>26</td><td>25</td><td>24</td><td>23</td><td>22</td><td>21</td><td>20</td><td>19</td><td>18</td><td>17</td><td>16</td></tr><tr><td colspan="16">IACTS [31:16]</td></tr><tr><td>15</td><td>14</td><td>13</td><td>12</td><td>11</td><td>10</td><td>9</td><td>8</td><td>7</td><td>6</td><td>5</td><td>4</td><td>3</td><td>2</td><td>1</td><td>0</td></tr><tr><td>IACTS15</td><td>IACTS14</td><td>IACTS13</td><td>IACTS12</td><td colspan="8">Reserved</td><td>IACTS3</td><td>IACTS2</td><td colspan="2">Reserved</td></tr></table>

<table><tr><td>位</td><td>名称</td><td>访问</td><td>描述</td><td>复位值</td></tr><tr><td>[31:12]</td><td>IACTS</td><td>RO</td><td>12#-31#中断执行状态，13#和15#保留。1：当前编号中断执行中；0：当前编号中断没执行。</td><td>0</td></tr><tr><td>[11:4]</td><td>Reserved</td><td>RO</td><td>保留。</td><td>0</td></tr><tr><td>[3:2]</td><td>IACTS</td><td>RO</td><td>2#-3#中断执行状态。1：当前编号中断执行中；0：当前编号中断没执行。</td><td>0</td></tr><tr><td>[1:0]</td><td>Reserved</td><td>RO</td><td>保留。</td><td>0</td></tr></table>

#### 9.5.2.34 PFIC 中断激活状态寄存器 2（PFIC_IACTR2）

偏移地址： $0 \times 3 0 4$

<table><tr><td>31</td><td>30</td><td>29</td><td>28</td><td>27</td><td>26</td><td>25</td><td>24</td><td>23</td><td>22</td><td>21</td><td>20</td><td>19</td><td>18</td><td>17</td><td>16</td></tr><tr><td colspan="16">IACTS[63:48]</td></tr><tr><td>15</td><td>14</td><td>13</td><td>12</td><td>11</td><td>10</td><td>9</td><td>8</td><td>7</td><td>6</td><td>5</td><td>4</td><td>3</td><td>2</td><td>1</td><td>0</td></tr><tr><td colspan="16">IACTS[47:32]</td></tr></table>

<table><tr><td>位</td><td>名称</td><td>访问</td><td>描述</td><td>复位值</td></tr><tr><td>[31:0]</td><td>IACTS</td><td>RO</td><td>32#-63#中断执行状态。</td><td>0</td></tr><tr><td></td><td></td><td></td><td>1: 当前编号中断执行中;
0: 当前编号中断没执行。</td><td></td></tr></table>

#### 9.5.2.35 PFIC 中断激活状态寄存器 3（PFIC_IACTR3）

偏移地址： $0 \times 3 0 8$

<table><tr><td>31</td><td>30</td><td>29</td><td>28</td><td>27</td><td>26</td><td>25</td><td>24</td><td>23</td><td>22</td><td>21</td><td>20</td><td>19</td><td>18</td><td>17</td><td>16</td></tr><tr><td colspan="16">I ACTS[95:80]</td></tr><tr><td>15</td><td>14</td><td>13</td><td>12</td><td>11</td><td>10</td><td>9</td><td>8</td><td>7</td><td>6</td><td>5</td><td>4</td><td>3</td><td>2</td><td>1</td><td>0</td></tr><tr><td colspan="16">I ACTS[79:64]</td></tr></table>

<table><tr><td>位</td><td>名称</td><td>访问</td><td>描述</td><td>复位值</td></tr><tr><td>[31:0]</td><td>IACTS</td><td>RO</td><td>64#--95#中断执行状态。
1: 当前编号中断执行中;
0: 当前编号中断没执行。</td><td>0</td></tr></table>

#### 9.5.2.36 PFIC 中断激活状态寄存器 4（PFIC_IACTR4）

偏移地址： $\mathtt { 0 } \mathtt { x } \mathtt { 3 0 0 }$

<table><tr><td>31</td><td>30</td><td>29</td><td>28</td><td>27</td><td>26</td><td>25</td><td>24</td><td>23</td><td>22</td><td>21</td><td>20</td><td>19</td><td>18</td><td>17</td><td>16</td></tr><tr><td colspan="16">Reserved</td></tr><tr><td>15</td><td>14</td><td>13</td><td>12</td><td>11</td><td>10</td><td>9</td><td>8</td><td>7</td><td>6</td><td>5</td><td>4</td><td>3</td><td>2</td><td>1</td><td>0</td></tr><tr><td colspan="8">Reserved</td><td colspan="8">IACTS [103:96]</td></tr></table>

<table><tr><td>位</td><td>名称</td><td>访问</td><td>描述</td><td>复位值</td></tr><tr><td>[31:8]</td><td>Reserved</td><td>R0</td><td>保留。</td><td>0</td></tr><tr><td>[7:0]</td><td>IACTS</td><td>WO</td><td>96#--103#中断执行状态。
1: 当前编号中断执行中;
0: 当前编号中断没执行</td><td>0</td></tr></table>

#### 9.5.2.37 PFIC 中断优先级配置寄存器（PFIC_IPRIORx）（x=0-63）

偏移地址： $0 { \times } 4 0 0 - 0 { \times } 4 { \sf F F }$

控制器支持 256 个中断（0-255），每个中断使用 8bit 来设置控制优先级。

<table><tr><td rowspan="2">IPRIOR63</td><td>31</td><td>24</td><td>23</td><td>16</td><td>15</td><td>8</td><td>7</td><td>0</td></tr><tr><td colspan="2">PRI0_255</td><td colspan="2">PRI0_254</td><td colspan="2">PRI0_253</td><td colspan="2">PRI0_252</td></tr><tr><td rowspan="2">IPRIORx</td><td>...</td><td>...</td><td>...</td><td>...</td><td>...</td><td>...</td><td colspan="2">...</td></tr><tr><td colspan="2">PRI0_(4x+3)</td><td colspan="2">PRI0_(4x+2)</td><td colspan="2">PRI0_(4x+1)</td><td colspan="2">PRI0_(4x)</td></tr><tr><td rowspan="2">IPRIOR0</td><td>...</td><td>...</td><td>...</td><td>...</td><td>...</td><td>...</td><td colspan="2">...</td></tr><tr><td colspan="2">PRI0_3</td><td colspan="2">PRI0_2</td><td colspan="2">PRI0_1</td><td colspan="2">PRI0_0</td></tr></table>

<table><tr><td>位</td><td>名称</td><td>访问</td><td>描述</td><td>复位值</td></tr><tr><td>[2047:2040]</td><td>IP_255</td><td>RW</td><td>同IP_0描述。</td><td>0</td></tr><tr><td>...</td><td>...</td><td>...</td><td>...</td><td>...</td></tr><tr><td>[31:24]</td><td>IP_3</td><td>RW</td><td>同IP_0描述。</td><td>0</td></tr><tr><td>[23:16]</td><td>IP_2</td><td>RW</td><td>同IP_0描述。</td><td>0</td></tr><tr><td>[15:8]</td><td>IP_1</td><td>RW</td><td>同IP_0描述。</td><td>0</td></tr><tr><td>[7:0]</td><td>IP_0</td><td>RW</td><td>编号0中断优先级配置:[7:4]:优先级控制位。若配置无嵌套,无抢占位;若配置2级嵌套,bit7为抢占位;若配置4级嵌套,bit7-bit6为抢占位;若配置8级嵌套,bit7-bit5为抢占位;优先级数值越小则优先级越高,同一抢占优先级中断若同时挂起,优先执行优先级高的中断。[3:0]:保留,固定为0,写无效。注:适用于青棵V4F内核:CH32V30xD8、CH32V30xD8C。编号0中断优先级配置:[7]:优先级控制位。若配置无嵌套,无抢占位;若配置2级嵌套,bit7为抢占位;优先级数值越小则优先级越高,同一抢占优先级中断若同时挂起,优先执行优先级高的中断。[6:0]:保留,固定为0,写无效。注:适用于青棵V4B、V4C内核:CH32V20xD6、CH32V20xD8、CH32V20xD8W。</td><td>0</td></tr></table>

#### 9.5.2.38 PFIC 系统控制寄存器（PFIC_SCTLR）

偏移地址： $0 \times 0 1 0$

<table><tr><td>31</td><td>30</td><td>29</td><td>28</td><td>27</td><td>26</td><td>25</td><td>24</td><td>23</td><td>22</td><td>21</td><td>20</td><td>19</td><td>18</td><td>17</td><td>16</td></tr><tr><td>SYS
RST</td><td colspan="15">Reserved</td></tr><tr><td>15</td><td>14</td><td>13</td><td>12</td><td>11</td><td>10</td><td>9</td><td>8</td><td>7</td><td>6</td><td>5</td><td>4</td><td>3</td><td>2</td><td>1</td><td>0</td></tr><tr><td colspan="10">Reserved</td><td>SET
EVENT</td><td>SEV
ONPEND</td><td>WF I TO
WFE</td><td>SLEEP
DEEP</td><td>SLEEP
ONEXIT</td><td>Reser
ved</td></tr></table>

<table><tr><td>位</td><td>名称</td><td>访问</td><td>描述</td><td>复位值</td></tr><tr><td>31</td><td>SYSRST</td><td>WO</td><td>系统复位,自动清0。写1有效,写0无效,与PFIC_CFGR寄存器相同效果</td><td>0</td></tr><tr><td>[30:6]</td><td>Reserved</td><td>RO</td><td>保留。</td><td>0</td></tr><tr><td>5</td><td>SETEVENT</td><td>WO</td><td>设置事件,可以唤醒 WFE 的情况。</td><td>0</td></tr><tr><td>4</td><td>SEVONPEND</td><td>RW</td><td>当发生事件或者中断挂起状态时,可以从 WFE 指令后唤醒系统,如果未执行 WFE 指令,将在下次执行该指令后立即唤醒系统。
1: 启用的事件和所有中断(包括未开启中断)都能唤醒系统;
0: 只有启用的事件和启用的中断可以唤醒系统。</td><td>0</td></tr><tr><td>3</td><td>WFITOWFE</td><td>RW</td><td>将 WFI 指令当成是 WFE 执行。
1: 将之后的 WFI 指令当做 WFE 指令;
0: 无作用。</td><td>0</td></tr><tr><td>2</td><td>SLEEPDEEP</td><td>RW</td><td>控制系统的低功耗模式:
1: deepsleep 0: sleep</td><td>0</td></tr><tr><td>1</td><td>SLEEPONEXIT</td><td>RW</td><td>控制离开中断服务程序后,系统状态:
1: 系统进入低功耗模式;
0: 系统进入主程序。</td><td>0</td></tr><tr><td>0</td><td>Reserved</td><td>RO</td><td>保留。</td><td>0</td></tr></table>

### 9.5.3 专用 CSR 寄存器

RISC-V 架构中定义了一些控制和状态寄存器（Control and Status Register,CSR），用于配置或标识或记录运行状态。CSR 寄存器属于内核内部的寄存器，使用专用的 12 位地址空间。 $\mathtt { C H 3 2 V 2 0 x }$ 和 $\mathtt { C H 3 2 V 3 0 x }$ 系列芯片除了 RISC-V 特权架构文档中定义的标准寄存器外，还增加了一些厂商自定义寄存器，需要使用 csr 指令进行访问。

注：此类寄存器标注为“MRW，MRO,MRW1”属性的需要系统在机器模式下才能访问。

#### 9.5.3.1 中断系统控制寄存器（INTSYSCR）

CSR 地址： $0 \times 8 0 4$   

<table><tr><td>31</td><td>30</td><td>29</td><td>28</td><td>27</td><td>26</td><td>25</td><td>24</td><td>23</td><td>22</td><td>21</td><td>20</td><td>19</td><td>18</td><td>17</td><td>16</td></tr><tr><td colspan="16">Reserved</td></tr><tr><td>15</td><td>14</td><td>13</td><td>12</td><td>11</td><td>10</td><td>9</td><td>8</td><td>7</td><td>6</td><td>5</td><td>4</td><td>3</td><td>2</td><td>1</td><td>0</td></tr><tr><td colspan="7">PMTSTA</td><td>Reserved</td><td>GIHWSTKNEN</td><td>HWSTKOVEN</td><td colspan="3">PMTCFG</td><td>INESTEN</td><td>HWSTKEN</td><td></td></tr></table>

<table><tr><td>位</td><td>名称</td><td>访问</td><td>描述</td><td>复位值</td></tr><tr><td>[31:16]</td><td>Reserved</td><td>MRO</td><td>保留。</td><td>0</td></tr><tr><td>[15:8]</td><td>PMTSTA</td><td>MRO</td><td>抢占位状态指示:0x00:优先级配置位中无抢占位,不发生中断嵌套;0x80:优先级配置位中最高位为抢占位,2级中断嵌套;0xC0:优先级配置位中高2位为抢占位,4级中断嵌套;0xE0:优先级配置位中高3位为抢占位,</td><td>0x00</td></tr><tr><td></td><td></td><td></td><td>8级中断嵌套。注:适用于青棵V4F内核:CH32V30x_D8、CH32V30x_D8C。抢占位状态指示:0x00:优先级配置位中无抢占位,不发生中断嵌套;0x80:优先级配置位中最高位为抢占位,2级中断嵌套;注:适用于青棵V4B、V4C内核:CH32V20x_D6、CH32V20x_D8、CH32V20x_D8W。</td><td></td></tr><tr><td>[7:6]</td><td>Reserved</td><td>MRO</td><td>保留。</td><td>0</td></tr><tr><td>5</td><td>GIHWSTKNEN</td><td>MRW1</td><td>全局中断和硬件压栈关闭使能。注:该位常使用于实时操作系统中,中断切换上下文时,置位该位,可关闭全局中断和硬件压栈出栈,当上下文切换完成,执行完中断返回后,硬件自动清除该位。</td><td>0</td></tr><tr><td>4</td><td>HWSTKOVEN</td><td>MRW</td><td>硬件压栈溢出后中断使能:0:硬件压栈溢出后,关闭全局中断;1:硬件压栈溢出后,中断仍可执行。注:CH32V30x_D8、CH32V30x_D8C硬件压栈深度为3级,当配置嵌套等级大于3级,若该位设置1,需要将低优先级的三级中断配置为硬件压栈,高优先级配置为软件压栈。CH32V20x_D6、CH32V20x_D8、CH32V20x_D8W硬件压栈深度为2级。</td><td>0</td></tr><tr><td>[3:2]</td><td>PMTCFG[1:0]</td><td>MRW</td><td>中断嵌套深度配置:00:无嵌套,抢占位个数为0;01:2级嵌套,抢占位个数为1;10:4级嵌套,抢占位个数为2;11:8级嵌套,抢占位个数为3。注:适用于青棵V4F内核:CH32V30x_D8、CH32V30x_D8C。中断嵌套深度配置:00:无嵌套,抢占位个数为0;01:2级嵌套,抢占位个数为1;注:适用于青棵V4B、V4C内核:CH32V20x_D6、CH32V20x_D8、CH32V20x_D8W。</td><td>00b</td></tr><tr><td>1</td><td>INESTEN</td><td>MRW</td><td>中断嵌套使能:0:中断嵌套功能关闭;1:中断嵌套功能使能。</td><td>0</td></tr><tr><td>0</td><td>HWSTKEN</td><td>MRW</td><td>硬件压栈使能:
0: 硬件压栈功能关闭;
1: 硬件压栈功能使能。</td><td>0</td></tr></table>

#### 9.5.3.2 异常入口基地址寄存器（MTVEC）

CSR 地址： $0 \times 3 0 5$

<table><tr><td>31</td><td>30</td><td>29</td><td>28</td><td>27</td><td>26</td><td>25</td><td>24</td><td>23</td><td>22</td><td>21</td><td>20</td><td>19</td><td>18</td><td>17</td><td>16</td></tr><tr><td colspan="16">BASEADDR[31:16]</td></tr><tr><td>15</td><td>14</td><td>13</td><td>12</td><td>11</td><td>10</td><td>9</td><td>8</td><td>7</td><td>6</td><td>5</td><td>4</td><td>3</td><td>2</td><td>1</td><td>0</td></tr><tr><td colspan="14">BASEADDR[15:2]</td><td>MODE1</td><td>MODE0</td></tr></table>

<table><tr><td>位</td><td>名称</td><td>访问</td><td>描述</td><td>复位值</td></tr><tr><td>[31:2]</td><td>BASEADDR[31:2]</td><td>MRW</td><td>中断向量表基地址。</td><td>0</td></tr><tr><td>1</td><td>MODE1</td><td>MRW</td><td>中断向量表识别模式:0:按跳转指令识别,有限范围,支持非跳指令;1:按绝对地址识别,支持全范围,但必须跳转。</td><td>0</td></tr><tr><td>0</td><td>MODE0</td><td>MRW</td><td>中断或异常入口地址模式选择:0:使用统一入口地址;1:根据中断编号*4进行地址偏移。</td><td>0</td></tr></table>

### 9.5.4 物理内存保护单元（PMP）

为了提高系统安全，RISC-V的架构中定义了一套物理地址访问限制，可以为区域内物理内存设置其读、写、执行属性，区域长度最小4字节保护。PMP单元在用户模式下一直生效，在机器模式下可选生效，如果违背了当前内存限制，将会产生系统异常中断（EXC）。

PMP 单元包含 4 组 8-bit 的配置寄存器（32bit）和 4 组地址寄存器，需要使用 csr 指令进行访问，并且在机器模式下进行。

#### 9.5.4.1 PMP 配置寄存器（PMPCFG0）

CSR 地址： $0 \times 3 { \tt A } 0$

<table><tr><td>31</td><td>30</td><td>29</td><td>28</td><td>27</td><td>26</td><td>25</td><td>24</td><td>23</td><td>22</td><td>21</td><td>20</td><td>19</td><td>18</td><td>17</td><td>16</td></tr><tr><td colspan="8">pmp3cfg</td><td colspan="8">pmp2cfg</td></tr><tr><td>15</td><td>14</td><td>13</td><td>12</td><td>11</td><td>10</td><td>9</td><td>8</td><td>7</td><td>6</td><td>5</td><td>4</td><td>3</td><td>2</td><td>1</td><td>0</td></tr><tr><td colspan="8">pmp1cfg</td><td colspan="8">pmp0cfg</td></tr></table>

<table><tr><td>位</td><td>名称</td><td>访问</td><td>描述</td><td>复位值</td></tr><tr><td>[31:24]</td><td>pmp3cfg</td><td>MRW</td><td>见pmp0cfg。</td><td>0</td></tr><tr><td>[23:16]</td><td>pmp2cfg</td><td>MRW</td><td>见pmp0cfg。</td><td>0</td></tr><tr><td>[15:8]</td><td>pmp1cfg</td><td>MRW</td><td>见pmp0cfg。</td><td>0</td></tr></table>

<table><tr><td rowspan="7">[7:0]</td><td rowspan="7">pmp0cfg</td><td rowspan="7">MRW</td><td>位</td><td>名称</td><td>描述</td><td rowspan="7">0</td></tr><tr><td>7</td><td>L</td><td>锁定使能,机器模式下可解锁0:不锁定;1:锁定相关寄存器。</td></tr><tr><td>[6:5]</td><td>-</td><td>保留。</td></tr><tr><td>[4:3]</td><td>A</td><td>地址对齐及保护区域范围选择。</td></tr><tr><td>2</td><td>X</td><td>可执行属性。</td></tr><tr><td>1</td><td>W</td><td>可写入属性。</td></tr><tr><td>0</td><td>R</td><td>可读出属性。</td></tr></table>

其中，地址对齐及保护区域范围选择，对于 A_ADDR≤region＜B_ADDR 区域进行内存保护（要求A_ADDR 和 B_ADDR 均为 4 字节对齐）：

1、如果 B_ADDR – A_ADDR == 22，则采用 NA4 方式；  
2、如果 B_ADDR – $\mathsf { A \_ A D D R } \mathsf { \Lambda } = \mathsf { \Lambda } 2 \mathsf { \Lambda } ^ { ( 6 + 2 ) }$ ， $\mathtt { G } \geqslant 1$ ，且 A_ADDR 为 2（G+2）对齐则采用 NAPOT 方式；  
3、否则采用 TOR 方式。

<table><tr><td>A值</td><td>名称</td><td>描述</td></tr><tr><td>00b</td><td>OFF</td><td>没有区域要保护</td></tr><tr><td>01b</td><td>TOR</td><td>顶端对齐区域保护:
pmp0cfg下,0≤region &lt;pmpaddr0;
pmp1cfg下,pmpaddr0≤region &lt;pmpaddr1;
pmp2cfg下,pmpaddr1≤region &lt;pmpaddr2;
pmp3cfg下,pmpaddr2≤region &lt;pmpaddr3。
pmpaddri-1=A_ADDR&gt;&gt;2;
pmpaddri=B_ADDR&gt;&gt;2。</td></tr><tr><td>10b</td><td>NA4</td><td>固定4字节区域保护。
pmp0cfg~pmp3cfg对应pmpaddr0~pmpaddr3作为起始地址。
pmpaddri=A_ADDR&gt;&gt;2。</td></tr><tr><td>11b</td><td>NAPOT</td><td>保护2(G+2)区域,G≥1,此时A_ADDR为2(G+2)对齐。
pmpaddri= ((A_ADDR| (2(G+2)-1)) &amp; ~ (1&lt;&lt;G+1)) &gt;&gt; 2。</td></tr></table>

#### 9.5.4.2 PMP 地址 0 寄存器（PMPADDR0）

CSR 地址： $\mathtt { 0 } \mathtt { x } \mathtt { 3 } \mathtt { B 0 }$

<table><tr><td>31</td><td>30</td><td>29</td><td>28</td><td>27</td><td>26</td><td>25</td><td>24</td><td>23</td><td>22</td><td>21</td><td>20</td><td>19</td><td>18</td><td>17</td><td>16</td></tr><tr><td colspan="16">ADDR0[33:18]</td></tr><tr><td>15</td><td>14</td><td>13</td><td>12</td><td>11</td><td>10</td><td>9</td><td>8</td><td>7</td><td>6</td><td>5</td><td>4</td><td>3</td><td>2</td><td>1</td><td>0</td></tr><tr><td colspan="16">ADDR0[17:2]</td></tr></table>

<table><tr><td>位</td><td>名称</td><td>访问</td><td>描述</td><td>复位值</td></tr><tr><td>[31:0]</td><td>ADDR0</td><td>MRW</td><td>PMP设置地址0的bit[33:2],实际高2位未用。</td><td>0</td></tr></table>

#### 9.5.4.3 PMP 地址 1 寄存器（PMPADDR1）

CSR 地址： $0 \times 3 { \tt B } 1$

<table><tr><td>31</td><td>30</td><td>29</td><td>28</td><td>27</td><td>26</td><td>25</td><td>24</td><td>23</td><td>22</td><td>21</td><td>20</td><td>19</td><td>18</td><td>17</td><td>16</td></tr><tr><td colspan="16">ADDR1 [33:18]</td></tr><tr><td>15</td><td>14</td><td>13</td><td>12</td><td>11</td><td>10</td><td>9</td><td>8</td><td>7</td><td>6</td><td>5</td><td>4</td><td>3</td><td>2</td><td>1</td><td>0</td></tr><tr><td colspan="16">ADDR1 [17:2]</td></tr></table>

<table><tr><td>位</td><td>名称</td><td>访问</td><td>描述</td><td>复位值</td></tr><tr><td>[31:0]</td><td>ADDR1</td><td>MRW</td><td>PMP设置地址1的bit[33:2],实际高2位未用。</td><td>0</td></tr></table>

#### 9.5.4.4 PMP 地址 2 寄存器（PMPADDR2）

CSR 地址： $0 \times 3 8 2$

<table><tr><td>31</td><td>30</td><td>29</td><td>28</td><td>27</td><td>26</td><td>25</td><td>24</td><td>23</td><td>22</td><td>21</td><td>20</td><td>19</td><td>18</td><td>17</td><td>16</td></tr><tr><td colspan="16">ADDR2[33:18]</td></tr><tr><td>15</td><td>14</td><td>13</td><td>12</td><td>11</td><td>10</td><td>9</td><td>8</td><td>7</td><td>6</td><td>5</td><td>4</td><td>3</td><td>2</td><td>1</td><td>0</td></tr><tr><td colspan="16">ADDR2[17:2]</td></tr></table>

<table><tr><td>位</td><td>名称</td><td>访问</td><td>描述</td><td>复位值</td></tr><tr><td>[31:0]</td><td>ADDR2</td><td>MRW</td><td>PMP设置地址2的bit[33:2],实际高2位未用。</td><td>0</td></tr></table>

#### 9.5.4.5 PMP 地址 3 寄存器（PMPADDR3）

CSR 地址： $0 \times 3 8 3$

<table><tr><td>31</td><td>30</td><td>29</td><td>28</td><td>27</td><td>26</td><td>25</td><td>24</td><td>23</td><td>22</td><td>21</td><td>20</td><td>19</td><td>18</td><td>17</td><td>16</td></tr><tr><td colspan="16">ADDR3[33:18]</td></tr><tr><td>15</td><td>14</td><td>13</td><td>12</td><td>11</td><td>10</td><td>9</td><td>8</td><td>7</td><td>6</td><td>5</td><td>4</td><td>3</td><td>2</td><td>1</td><td>0</td></tr><tr><td colspan="16">ADDR3[17:2]</td></tr></table>

<table><tr><td>位</td><td>名称</td><td>访问</td><td>描述</td><td>复位值</td></tr><tr><td>[31:0]</td><td>ADDR3</td><td>MRW</td><td>PMP设置地址3的bit[33:2],实际高2位未用。</td><td>0</td></tr></table>

### 9.5.5 RISC-V-SysTick 寄存器描述

表9-6 STK相关寄存器列表  

<table><tr><td>名称</td><td>访问地址</td><td>描述</td><td>复位值</td></tr><tr><td>R32_STK_CTLR</td><td>0xE000F000</td><td>系统计数控制寄存器</td><td>0x00000000</td></tr><tr><td>R32_STK_SR</td><td>0xE000F004</td><td>系统计数状态寄存器</td><td>0x00000000</td></tr><tr><td>R32_STK_CNTL</td><td>0xE000F008</td><td>系统计数器低位寄存器</td><td>0x00000000</td></tr><tr><td>R32_STK_CNTH</td><td>0xE000F00C</td><td>系统计数器高位寄存器</td><td>0x00000000</td></tr><tr><td>R32_STK_CMPLR</td><td>0xE000F010</td><td>计数比较低位寄存器</td><td>0x00000000</td></tr><tr><td>R32_STK_CMPHR</td><td>0xE000F014</td><td>计数比较高位寄存器</td><td>0x00000000</td></tr></table>

注：适用于基于32位RISC-V指令集及架构设计的通用微控制器。

#### 9.5.5.1 系统计数控制寄存器（STK_CTLR）

偏移地址： $0 \times 0 0$

<table><tr><td>31</td><td>30</td><td>29</td><td>28</td><td>27</td><td>26</td><td>25</td><td>24</td><td>23</td><td>22</td><td>21</td><td>20</td><td>19</td><td>18</td><td>17</td><td>16</td></tr><tr><td>SWIE</td><td colspan="15">Reserved</td></tr><tr><td>15</td><td>14</td><td>13</td><td>12</td><td>11</td><td>10</td><td>9</td><td>8</td><td>7</td><td>6</td><td>5</td><td>4</td><td>3</td><td>2</td><td>1</td><td>0</td></tr><tr><td colspan="10">Reserved</td><td>INIT</td><td>MODE</td><td>STRE</td><td>STCLK</td><td>STIE</td><td>STE</td></tr></table>

<table><tr><td>位</td><td>名称</td><td>访问</td><td>描述</td><td>复位值</td></tr><tr><td>31</td><td>SWIE</td><td>RW</td><td>软件中断触发使能(SWI):1:触发软件中断;0:关闭触发。进入软件中断后,需软件清0,否则持续触发。</td><td>0</td></tr><tr><td>[30:6]</td><td>Reserved</td><td>RO</td><td>保留。</td><td>0</td></tr><tr><td>5</td><td>INIT</td><td>W1</td><td>计数器初始值更新:1:向上计数时更新为0,向下计数时更新为比较值;0:无效。</td><td>0</td></tr><tr><td>4</td><td>MODE</td><td>RW</td><td>计数模式:1:向下计数;0:向上计数。</td><td>0</td></tr><tr><td>3</td><td>STRE</td><td>RW</td><td>自动重装载计数使能位:1:向上计数到比较值后重新从0开始计数,向下计数到0后,重新从比较值开始计数;0:向上计数到比较值后继续向上计数,向下计数到0后,重新从最大值开始向下计数。</td><td>0</td></tr><tr><td>2</td><td>STCLK</td><td>RW</td><td>计数器时钟源选择位:1: HCLK做时基;0: HCLK/8做时基;</td><td>0</td></tr><tr><td>1</td><td>STIE</td><td>RW</td><td>计数器中断使能控制位:1:使能计数器中断;0:关闭计数器中断。</td><td>0</td></tr><tr><td>0</td><td>STE</td><td>RW</td><td>系统计数器使能控制位:1:启动系统计数器STK;0:关闭系统计数器STK,计数器停止计数。</td><td>0</td></tr></table>

#### 9.5.5.2 系统计数状态寄存器（STK_SR）

偏移地址： $0 \times 0 4$

<table><tr><td>31</td><td>30</td><td>29</td><td>28</td><td>27</td><td>26</td><td>25</td><td>24</td><td>23</td><td>22</td><td>21</td><td>20</td><td>19</td><td>18</td><td>17</td><td>16</td></tr><tr><td colspan="16">Reserved</td></tr><tr><td>15</td><td>14</td><td>13</td><td>12</td><td>11</td><td>10</td><td>9</td><td>8</td><td>7</td><td>6</td><td>5</td><td>4</td><td>3</td><td>2</td><td>1</td><td>0</td></tr><tr><td colspan="15">Reserved</td><td>CNTIF</td></tr></table>

<table><tr><td>位</td><td>名称</td><td>访问</td><td>描述</td><td>复位值</td></tr><tr><td>[31:1]</td><td>Reserved</td><td>R0</td><td>保留</td><td>0</td></tr><tr><td>0</td><td>CNTIF</td><td>RWO</td><td>计数值比较标志，写0清除，写1无效：
1：向上计数达到比较值，向下计数到0；
0：未达到比较值。</td><td>0</td></tr></table>

#### 9.5.5.3 系统计数器低位寄存器（STK_CNTL）

偏移地址： $0 \times 0 8$

<table><tr><td>31</td><td>30</td><td>29</td><td>28</td><td>27</td><td>26</td><td>25</td><td>24</td><td>23</td><td>22</td><td>21</td><td>20</td><td>19</td><td>18</td><td>17</td><td>16</td></tr><tr><td colspan="16">CNT[31:16]</td></tr><tr><td>15</td><td>14</td><td>13</td><td>12</td><td>11</td><td>10</td><td>9</td><td>8</td><td>7</td><td>6</td><td>5</td><td>4</td><td>3</td><td>2</td><td>1</td><td>0</td></tr><tr><td colspan="16">CNT[15:0]</td></tr></table>

<table><tr><td>位</td><td>名称</td><td>访问</td><td>描述</td><td>复位值</td></tr><tr><td>[31:0]</td><td>CNT[31:0]</td><td>RW</td><td>当前计数器计数值低32位。</td><td>0</td></tr></table>

注：寄存器 STK_CNTL和寄存器STK_CNTH共同构成了64位系统计数器。

#### 9.5.5.4 系统计数器高位寄存器（STK_CNTH）

偏移地址： $0 \times 0 0$

<table><tr><td>31</td><td>30</td><td>29</td><td>28</td><td>27</td><td>26</td><td>25</td><td>24</td><td>23</td><td>22</td><td>21</td><td>20</td><td>19</td><td>18</td><td>17</td><td>16</td></tr><tr><td colspan="16">CNT[63:48]</td></tr><tr><td>15</td><td>14</td><td>13</td><td>12</td><td>11</td><td>10</td><td>9</td><td>8</td><td>7</td><td>6</td><td>5</td><td>4</td><td>3</td><td>2</td><td>1</td><td>0</td></tr><tr><td colspan="16">CNT[47:32]</td></tr></table>

<table><tr><td>位</td><td>名称</td><td>访问</td><td>描述</td><td>复位值</td></tr><tr><td>[31:0]</td><td>CNT[63:32]</td><td>RW</td><td>当前计数器计数值高32位。</td><td>0</td></tr></table>

注：寄存器STK_CNTL和寄存器STK_CNTH共同构成了64位系统计数器。

#### 9.5.5.5 计数比较低位寄存器（STK_CMPLR）

偏移地址： $0 \times 1 0$

<table><tr><td>31</td><td>30</td><td>29</td><td>28</td><td>27</td><td>26</td><td>25</td><td>24</td><td>23</td><td>22</td><td>21</td><td>20</td><td>19</td><td>18</td><td>17</td><td>16</td></tr><tr><td colspan="16">CMP[31:16]</td></tr><tr><td>15</td><td>14</td><td>13</td><td>12</td><td>11</td><td>10</td><td>9</td><td>8</td><td>7</td><td>6</td><td>5</td><td>4</td><td>3</td><td>2</td><td>1</td><td>0</td></tr><tr><td colspan="16">CMP[15:0]</td></tr></table>

<table><tr><td>位</td><td>名称</td><td>访问</td><td>描述</td><td>复位值</td></tr><tr><td>[31:0]</td><td>CMP[31:0]</td><td>RW</td><td>设置比较计数器值低32位。</td><td>0</td></tr></table>

注：寄存器STK_CMPLR和寄存器STK_CMPHR共同构成了64位计数器比较值。

#### 9.5.5.6 计数比较高位寄存器（STK_CMPHR）

偏移地址： $0 \times 1 4$

<table><tr><td>31</td><td>30</td><td>29</td><td>28</td><td>27</td><td>26</td><td>25</td><td>24</td><td>23</td><td>22</td><td>21</td><td>20</td><td>19</td><td>18</td><td>17</td><td>16</td></tr><tr><td colspan="16">CMP[63:48]</td></tr><tr><td>15</td><td>14</td><td>13</td><td>12</td><td>11</td><td>10</td><td>9</td><td>8</td><td>7</td><td>6</td><td>5</td><td>4</td><td>3</td><td>2</td><td>1</td><td>0</td></tr><tr><td colspan="16">CMP[47:32]</td></tr></table>

<table><tr><td>位</td><td>名称</td><td>访问</td><td>描述</td><td>复位值</td></tr><tr><td>[31:0]</td><td>CMP[63:32]</td><td>RW</td><td>设置比较计数器值高32位。</td><td>0</td></tr></table>

注：寄存器STK_CMPLR和寄存器STK_CMPHR共同构成了64位计数器比较值。

### 9.5.6 ARM-SysTick 寄存器描述

表 9-7 SysTick 相关寄存器列表  

<table><tr><td>名称</td><td>访问地址</td><td>描述</td><td>复位值</td></tr><tr><td>R32_STK_CTRL</td><td>0xE000E010</td><td>SysTick 控制及状态寄存器</td><td>0x00000000</td></tr><tr><td>R32_STK_LOAD</td><td>0xE000E014</td><td>SysTick 重装载数值寄存器</td><td>0x00000000</td></tr><tr><td>R32_STK_VAL</td><td>0xE000E018</td><td>SysTick 当前数值寄存器</td><td>0x00000000</td></tr><tr><td>R32_STK_CALIB</td><td>0xE000E01C</td><td>SysTick 校准数值寄存器</td><td>0x00000000</td></tr></table>

内核设计的通用微控制器

#### 9.5.6.1 SysTick 控制及状态寄存器（STK_CTRL）

偏移地址： $0 \times 0 0$

<table><tr><td>31</td><td>30</td><td>29</td><td>28</td><td>27</td><td>26</td><td>25</td><td>24</td><td>23</td><td>22</td><td>21</td><td>20</td><td>19</td><td>18</td><td>17</td><td>16</td></tr><tr><td colspan="15">Reserved</td><td>COUNT
FLAG</td></tr><tr><td>15</td><td>14</td><td>13</td><td>12</td><td>11</td><td>10</td><td>9</td><td>8</td><td>7</td><td>6</td><td>5</td><td>4</td><td>3</td><td>2</td><td>1</td><td>0</td></tr><tr><td colspan="12">Reserved</td><td>CLKSO
URCE</td><td>TICKI
NT</td><td>ENABL
E</td><td></td></tr></table>

<table><tr><td>位</td><td>名称</td><td>访问</td><td>描述</td><td>复位值</td></tr><tr><td>[31:17]</td><td>Reserved</td><td>R0</td><td>保留。</td><td>0</td></tr><tr><td>16</td><td>COUNTFLAG</td><td>R0</td><td>如果在上次读取本寄存器后，SysTick 已经数到了 0，则该位为 1。如果读取该位，该位将自动清零。</td><td>0</td></tr><tr><td>[15:3]</td><td>Reserved</td><td>R0</td><td>保留。</td><td>0</td></tr><tr><td>2</td><td>CLKSOURCE</td><td>RW</td><td>0=外部时钟源(STCLK)
1=内部时钟(FCLK)</td><td>0</td></tr><tr><td>1</td><td>TICKINT</td><td>RW</td><td>1=SysTick 倒数到 0 时产生 SysTick 异常请求
0=数到 0 时无动作</td><td>0</td></tr><tr><td>0</td><td>ENABLE</td><td>RW</td><td>SysTick 定时器的使能位</td><td>0</td></tr></table>

#### 9.5.6.2 SysTick 重装载数值寄存器（STK_LOAD）

偏移地址： $0 \times 0 4$

<table><tr><td>31</td><td>30</td><td>29</td><td>28</td><td>27</td><td>26</td><td>25</td><td>24</td><td>23</td><td>22</td><td>21</td><td>20</td><td>19</td><td>18</td><td>17</td><td>16</td></tr><tr><td colspan="8">Reserved</td><td colspan="8">RELOAD[23:16]</td></tr><tr><td>15</td><td>14</td><td>13</td><td>12</td><td>11</td><td>10</td><td>9</td><td>8</td><td>7</td><td>6</td><td>5</td><td>4</td><td>3</td><td>2</td><td>1</td><td>0</td></tr><tr><td colspan="16">RELOAD[15:0]</td></tr></table>

<table><tr><td>位</td><td>名称</td><td>访问</td><td>描述</td><td>复位值</td></tr><tr><td>[31:24]</td><td>Reserved</td><td>R0</td><td>保留。</td><td>0</td></tr><tr><td>[23:0]</td><td>RELOAD</td><td>RW</td><td>当倒数至零时，将被重装载的值</td><td>0</td></tr></table>

#### 9.5.6.3 SysTick 当前数值寄存器（STK_VAL）

偏移地址： $0 \times 0 8$

<table><tr><td>31</td><td>30</td><td>29</td><td>28</td><td>27</td><td>26</td><td>25</td><td>24</td><td>23</td><td>22</td><td>21</td><td>20</td><td>19</td><td>18</td><td>17</td><td>16</td></tr><tr><td colspan="8">Reserved</td><td colspan="8">CURRENT[23:16]</td></tr><tr><td>15</td><td>14</td><td>13</td><td>12</td><td>11</td><td>10</td><td>9</td><td>8</td><td>7</td><td>6</td><td>5</td><td>4</td><td>3</td><td>2</td><td>1</td><td>0</td></tr><tr><td colspan="16">CURRENT[15:0]</td></tr></table>

<table><tr><td>位</td><td>名称</td><td>访问</td><td>描述</td><td>复位值</td></tr><tr><td>[31:24]</td><td>Reserved</td><td>R0</td><td>保留。</td><td>0</td></tr><tr><td>[23:0]</td><td>CURRENT</td><td>RW</td><td>读取时返回当前倒计数的值，写它则使之清零，同时还会清除在 SysTick 控制及状态寄存器中的 COUNTFLAG 标志。</td><td>0</td></tr></table>

#### 9.5.6.4 SysTick 校准数值寄存器（STK_CALIB）

偏移地址： $0 \times 0 0$

<table><tr><td>31</td><td>30</td><td>29</td><td>28</td><td>27</td><td>26</td><td>25</td><td>24</td><td>23</td><td>22</td><td>21</td><td>20</td><td>19</td><td>18</td><td>17</td><td>16</td></tr><tr><td>NOREF</td><td>SKEW</td><td></td><td colspan="5">Reserved</td><td colspan="8">TENMS[23:16]</td></tr><tr><td>15</td><td>14</td><td>13</td><td>12</td><td>11</td><td>10</td><td>9</td><td>8</td><td>7</td><td>6</td><td>5</td><td>4</td><td>3</td><td>2</td><td>1</td><td>0</td></tr><tr><td colspan="16">TENMS[15:0]</td></tr></table>

<table><tr><td>位</td><td>名称</td><td>访问</td><td>描述</td><td>复位值</td></tr><tr><td>31</td><td>NOREF</td><td>RO</td><td>1=没有外部参考时钟(STCLK不可用)0=外部参考时钟可用</td><td>0</td></tr><tr><td>30</td><td>SKEW</td><td>RO</td><td>1=校准值不是准确的10ms0=校准值是准确的10ms</td><td>0</td></tr><tr><td>[29:24]</td><td>Reserved</td><td>RO</td><td>保留。</td><td>0</td></tr><tr><td>[23:0]</td><td>TENMS</td><td>RW</td><td>10ms的时间内倒计数的格数。芯片设计者应该通过Cortex-M3的输入信号提供该数值。若该数值读回零,则表示无法使用校准功能。</td><td>0</td></tr></table>

# 第 10 章 GPIO 及其复用功能（GPIO/AFIO）

本章模块描述适用于CH32F2x、CH32V2x和CH32V3x微控制器全系列产品。

GPIO口可以配置成多种输入或输出模式，内置可关闭的上拉或下拉电阻，可以配置成推挽或开漏功能。GPIO口还可以复用成其他功能。

## 10.1 主要特征

端口的每个引脚都可以配置成以下的多种模式之一：

浮空输入  
开漏输出  
上拉输入  
推挽输出  
下拉输入  
复用功能的输入和输出  
模拟输入

许多引脚拥有复用功能，很多其他的外设把自己的输出和输入通道映射到这些引脚上，这些复用引脚具体用法需要参照各个外设，而对这些引脚是否复用和是否重映射的内容由本章说明。

## 10.2 功能描述

### 10.2.1 概述

![](images/48d34e319722daec5af78aeac02f72cd53f5e87e05343d4579211a0fe9e877c4.jpg)  
图 10-1 GPIO 模块基本结构框图

如图 10-1 所示 IO 口结构，每个引脚在芯片内部都有两只保护二极管，IO 口内部可分为输入和输出驱动模块。其中输入驱动有弱上下拉电阻可选，可连接到 AD 等模拟输入的外设；如果输入到数字外设，就需要经过一个 TTL 施密特触发器，再连接到 GPIO 输入寄存器或其他复用外设。而输出驱动有一对 MOS 管，可通过配置上下的 MOS 管是否使能来将 IO 口配置成开漏或推挽输出；输出驱动内部也可以配置成由GPIO控制输出还是由复用的其他外设控制输出。

### 10.2.2 GPIO 的初始化功能

刚复位后，GPIO 口运行在初始状态，这时大多数 IO 口都是运行在浮空输入状态，但也有 HSE 等外设相关的引脚是运行在外设复用的功能上。具体的初始化功能请参照引脚描述相关的章节。

### 10.2.3 外部中断

所有的 GPIO 口都可以被配置外部中断输入通道，但一个外部中断输入通道最多只能映射到一个GPIO 引脚上，且外部中断通道的序号必须和 GPIO 端口的位号一致，比如 PA1（或 PB1、PC1、PD1、PE1 等）只能映射到 EXTI1 上，且 EXTI1 只能接受 PA1、PB1、PC1、PD1 或 PE1 等其中之一的映射，两方都是一对一的关系。

### 10.2.4 复用功能

使用复用功能必须要注意：

l 使用输入方向的复用功能，端口必须配置成复用输入模式，上下拉设置可根据实际需要来设置  
$\bullet$ 使用输出方向的复用功能，端口必须配置成复用输出模式，推挽或开漏可根据实际情况设置  
l 对于双向的复用功能，端口必须配置成复用输出模式，这时驱动器被配置成浮空输入模式

同一个 IO 口可能有多个外设复用到此管脚，因此为了使各个外设都有最大的发挥空间，外设的复用引脚除了默认复用引脚，还可以进行重映射，重映射到其他的引脚，避开被占用的引脚。

### 10.2.5 锁定机制

锁定机制可以锁定IO口的配置。经过特定的一个写序列后，选定的IO 引脚配置将被锁定，在下一个复位前无法更改。

### 10.2.6 输入配置

![](images/f2cd2077e1a8d94d39272a904c4f7f8e5302f0e4a51810139c1d03195fbff23a.jpg)  
图 10-2 GPIO 模块输入配置结构框图

当 IO 口配置成输入模式时，输出驱动断开，输入上下拉可选，不连接复用功能和模拟输入。在每个IO口上的数据在每个APB2时钟被采样到输入数据寄存器，读取输入数据寄存器对应位即获取了对应引脚的电平状态。

### 10.2.7 输出配置

![](images/e6246b52d49b45893198530532eb7102aa6b2894c7cc0751d47065c74f61a770.jpg)  
图10-3 GPIO模块输出配置结构框图

当 IO 口配置成输出模式时，输出驱动器中的一对 MOS 可根据需要被配置成推挽或开漏模式，不使用复用功能。输入驱动的上下拉电阻被禁用，TTL施密特触发器被激活，出现在 IO引脚上的电平将会在每个 APB2 时钟被采样到输入数据寄存器，所以读取输入数据寄存器将会得到 IO 状态，在推挽输出模式时，对输出数据寄存器的访问就会得到最后一次写入的值。

### 10.2.8 复用功能配置

![](images/30dd67e11fb95020f4c29777eb1378fc9a0ca3185d76a02e75615ef8f3f633c9.jpg)  
图10-4 GPIO模块被其他外设复用时的结构框图

在启用复用功能时，输出驱动器被使能，可以按需要被配置成开漏或推挽模式，施密特触发器也被打开，复用功能的输入和输出线都被连接，但是输出数据寄存器被断开，出现在 IO 引脚上的电平将会在每个 APB2 时钟被采样到输入数据寄存器，在开漏模式下，读取输入数据寄存器将会得到 IO 口当前状态；在推挽模式下，读取输出数据寄存器将会得到最后一次写入的值。

### 10.2.9 模拟输入配置

![](images/7ab8f4450cb3c79d9554cc219543996f8e4f372f298899d00456ca92e4b5a0fe.jpg)  
图10-5 GPIO模块作为模拟输入时的配置结构框图

在启用模拟输入时，输出缓冲器被断开，输入驱动中施密特触发器的输入被禁止以防止产生 IO口上的消耗，上下拉电阻被禁止，读取输入数据寄存器将一直为 0。

### 10.2.10 外设的 GPIO 设置

下列表格推荐了各个外设的引脚相应的 GPIO 口配置。

表 10-1 高级定时器（TIM1/8/9/10）  

<table><tr><td>TIM1/8/9/10</td><td>配置</td><td>GPIO配置</td></tr><tr><td rowspan="2">TIM1/8/9/10_CHx</td><td>输入捕获通道x</td><td>浮空输入</td></tr><tr><td>输出比较通道x</td><td>推挽复用输出</td></tr><tr><td>TIM1/8/9/10_CHxN</td><td>互补输出通道x</td><td>推挽复用输出</td></tr><tr><td>TIM1/8/9/10_BKIN</td><td>刹车输入</td><td>浮空输入</td></tr><tr><td>TIM1/8/9/10_ETR</td><td>外部触发时钟输入</td><td>浮空输入</td></tr></table>

表 10-2 通用定时器（TIM2/3/4/5）  

<table><tr><td>TIM2/3/4/5 引脚</td><td>配置</td><td>GPIO配置</td></tr><tr><td rowspan="2">TIM2/3/4/5_CHx</td><td>输入捕获通道x</td><td>浮空输入</td></tr><tr><td>输出比较通道x</td><td>推挽复用输出</td></tr><tr><td>TIM2/3/4/5_ETR</td><td>外部触发时钟输入</td><td>浮空输入</td></tr></table>

表 10-3 通用同步异步串行收发器（USART）  

<table><tr><td>USART引脚</td><td>配置</td><td>GPIO配置</td></tr><tr><td rowspan="2">USARTx_TX</td><td>全双工模式</td><td>推挽复用输出</td></tr><tr><td>半双工同步模式</td><td>推挽复用输出</td></tr><tr><td rowspan="2">USARTx_RX</td><td>全双工模式</td><td>浮空输入或带上拉输入</td></tr><tr><td>半双工同步模式</td><td>未使用</td></tr><tr><td>USARTx_CK</td><td>同步模式</td><td>推挽复用输出</td></tr><tr><td>USARTx_RTS</td><td>硬件流量控制</td><td>推挽复用输出</td></tr><tr><td>USARTx_CTS</td><td>硬件流量控制</td><td>浮空输入或带上拉输入</td></tr></table>

表 10-4 串行外设接口（SPI）模块  

<table><tr><td>SPI引脚</td><td>配置</td><td>GPIO配置</td></tr><tr><td rowspan="2">SPIx_SCK</td><td>主模式</td><td>推挽复用输出</td></tr><tr><td>从模式</td><td>浮空输入</td></tr><tr><td rowspan="4">SPIx_MOSI</td><td>全双工主模式</td><td>推挽复用输出</td></tr><tr><td>全双工从模式</td><td>浮空输入或带上拉输入</td></tr><tr><td>简单的双向数据线/主模式</td><td>推挽复用输出</td></tr><tr><td>简单的双向数据线/从模式</td><td>未使用</td></tr><tr><td rowspan="4">SPIx_MISO</td><td>全双工主模式</td><td>浮空输入或带上拉输入</td></tr><tr><td>全双工从模式</td><td>推挽复用输出</td></tr><tr><td>简单的双向数据线/主模式</td><td>未使用</td></tr><tr><td>简单的双向数据线/从模式</td><td>推挽复用输出</td></tr><tr><td rowspan="3">SPIx_NSS</td><td>硬件主或从模式</td><td>浮空、上拉或下拉输入</td></tr><tr><td>硬件主模式</td><td>推挽复用输出</td></tr><tr><td>软件模式</td><td>未使用</td></tr></table>

表 10-5 内置音频总线（I2S）模块  

<table><tr><td>12S引脚</td><td>配置</td><td>GPIO配置</td></tr><tr><td rowspan="2">12Sx_WS</td><td>主模式</td><td>推挽复用输出</td></tr><tr><td>从模式</td><td>浮空输入</td></tr><tr><td rowspan="2">12Sx_CK</td><td>主模式</td><td>推挽复用输出</td></tr><tr><td>从模式</td><td>浮空输入</td></tr><tr><td rowspan="2">12Sx_SD</td><td>发送器</td><td>推挽复用输出</td></tr><tr><td>接收器</td><td>浮空、上拉或下拉输入</td></tr><tr><td rowspan="2">12Sx_MCK</td><td>主模式</td><td>推挽复用输出</td></tr><tr><td>从模式</td><td>未使用</td></tr></table>

表 10-6 内部集成总线（I2C）模块  

<table><tr><td>I²C引脚</td><td>配置</td><td>GPIO配置</td></tr><tr><td>I²C_SCL</td><td>I²C时钟</td><td>开漏复用输出</td></tr><tr><td>I²C_SDA</td><td>I²C数据</td><td>开漏复用输出</td></tr></table>

表 10-7 控制器局域网（CAN）模块  

<table><tr><td>CAN引脚</td><td>GPIO配置</td></tr><tr><td>CANx_TX</td><td>推挽复用输出</td></tr><tr><td>CANx_RX</td><td>浮空输入或上拉输入</td></tr></table>

表 10-8 USB 全速设备（USBD）控制器  

<table><tr><td>USBD 引脚</td><td>GPIO 配置</td></tr><tr><td>USBD_DM/USBD_DP</td><td>使能了 USB 模块之后，复用 I0 口会自动连接到内部 USBD 收发器</td></tr></table>

表 10-9 USB 主机设备（USBFS）控制器  

<table><tr><td>USBFS 引脚</td><td>GPIO 配置</td></tr><tr><td>USBFS_DM/USBFS_DP</td><td>使能了 USB 模块之后，复用 I0 口会自动连接到内部 USBFS 收发</td></tr><tr><td></td><td>器</td></tr></table>

表 10-10 USB OTG_FS 控制器  

<table><tr><td>USB OTG_FS 引脚</td><td>GPIO 配置</td></tr><tr><td>OTG_FS_VBUS</td><td>模拟输入</td></tr><tr><td>OTG_FS_ID</td><td>上拉输入</td></tr><tr><td>OTG_FS_DM</td><td>由 USB 断电自动控制</td></tr><tr><td>OTG_FS_DP</td><td>由 USB 断电自动控制</td></tr></table>

表 10-11 安全数字输入输出（SDIO）模块  

<table><tr><td>SD10 引脚</td><td>配置</td><td>GPIO 配置</td></tr><tr><td>SD10_CK</td><td>时钟</td><td>推挽复用输出</td></tr><tr><td>SD10_CMD</td><td>命令</td><td>推挽复用输出</td></tr><tr><td>SD10[D7:D0]</td><td>数据</td><td>推挽复用输出</td></tr></table>

表10-12 直接存储器访问控制（FSMC）控制器  

<table><tr><td>FSMC引脚</td><td>GPIO配置</td></tr><tr><td>FSMC_A[23:16]</td><td rowspan="2">推挽复用输出</td></tr><tr><td>FSMC_D[15:0]</td></tr><tr><td>FSMC_CK</td><td>推挽复用输出</td></tr><tr><td>FSMC_NOE</td><td rowspan="2">推挽复用输出</td></tr><tr><td>FSMC_NWE</td></tr><tr><td>FSMC_NE1</td><td rowspan="2">推挽复用输出</td></tr><tr><td>FSMC_NCE2</td></tr><tr><td>FSMC_NWAIT</td><td>浮空输入或带上拉输入</td></tr><tr><td>FSMC_NBL[1:0]</td><td>推挽复用输出</td></tr></table>

表 10-13 模拟转数字转换器（ADC）及数字转模拟转换器（DAC）  

<table><tr><td>ADC/DAC引脚</td><td>GPIO配置</td></tr><tr><td>ADC/DAC</td><td>模拟输入</td></tr></table>

表 10-14 其他的 IO 功能设置  

<table><tr><td>引脚</td><td>配置功能</td><td>GPIO配置</td></tr><tr><td rowspan="2">TAMPER_RTC</td><td>RTC输出</td><td rowspan="2">硬件自动设置</td></tr><tr><td>侵入事件输入</td></tr><tr><td>MCO</td><td>时钟输出</td><td>推挽复用输出</td></tr><tr><td>EXT1</td><td>外部中断输入</td><td>浮空、上拉或下拉输入</td></tr></table>

### 10.2.11 复用功能重映射 GPIO 设置

#### 10.2.11.1 OSC32_IN/OSC32_OUT 作为 GPIO 端口 PC14/PC15

当 LSEON=0 时，LSE 振荡器引脚 OSC32_IN/OSC32_OUT 可以分别用做 GPIO 的 PC14/PC15。

当 $\mathsf { L S E O N } = 1$ 时，作为 LSE 引脚。

#### 10.2.11.2 OSC_IN/OSC_OUT 引脚作为 GPIO 端口 PD0/PD1

OSC_IN/OSC_OUT 可以用做 GPIO 的 PD0/PD1，通过设置重映射寄存器 1（AFIO_PCFR1）实现。

这个重映射只适用于 20、32、48 和 64 脚的封装,但 CH32V203RBT6 只有 OSC_IN 和 OSC_OUT 功能脚,不支持映射（对于 LQFP100 封装，由于 PD0 和 PD1 为固有的功能引脚，因此没有必要 再由软件进行重映像设置。）

#### 10.2.11.3 定时器复用功能重映射

表 10-15 TIM1 复用功能重映射  

<table><tr><td>复用功能</td><td>TIM1_RM=00
默认映射</td><td>TIM1_RM=01
部分映射</td><td>TIM1_RM=11
完全映射(1)</td></tr><tr><td>TIM1_ETR</td><td>PA12</td><td>PA12</td><td>PE7</td></tr><tr><td>TIM1_CH1</td><td>PA8</td><td>PA8</td><td>PE9</td></tr><tr><td>TIM1_CH2</td><td>PA9</td><td>PA9</td><td>PE11</td></tr><tr><td>TIM1_CH3</td><td>PA10</td><td>PA10</td><td>PE13</td></tr><tr><td>TIM1_CH4</td><td>PA11</td><td>PA11</td><td>PE14</td></tr><tr><td>TIM1_BKIN</td><td>PB12</td><td>PA6</td><td>PE15</td></tr><tr><td>TIM1_CH1N</td><td>PB13</td><td>PA7</td><td>PE8</td></tr><tr><td>TIM1_CH2N</td><td>PB14</td><td>PBO</td><td>PE10</td></tr><tr><td>TIM1_CH3N</td><td>PB15</td><td>PB1</td><td>PE12</td></tr></table>

注：（1）仅LQFP100封装，支持该位重映射功能。

表 10-16 TIM2 复用功能重映射  

<table><tr><td>复用功能</td><td>TIM2_RM=00
默认映射</td><td>TIM2_RM=01
部分映射</td><td>TIM2_RM=10
部分映射</td><td>TIM2_RM=11
完全映射</td></tr><tr><td>TIM2_ETR</td><td>PA0</td><td>PA15</td><td>PA0</td><td>PA15</td></tr><tr><td>TIM2_CH1</td><td>PA0</td><td>PA15</td><td>PA0</td><td>PA15</td></tr><tr><td>TIM2_CH2</td><td>PA1</td><td>PB3</td><td>PA1</td><td>PB3</td></tr><tr><td>TIM2_CH3</td><td>PA2</td><td>PA2</td><td>PB10</td><td>PB10</td></tr><tr><td>TIM2_CH4</td><td>PA3</td><td>PA3</td><td>PB11</td><td>PB11</td></tr></table>

表 10-17 TIM3 复用功能重映射  

<table><tr><td>复用功能</td><td>TIM3_RM=00
默认映射</td><td>TIM3_RM=10
部分映射</td><td>TIM3_RM=11
完全映射(1)</td></tr><tr><td>TIM3_CH1</td><td>PA6</td><td>PB4</td><td>PC6</td></tr><tr><td>TIM3_CH2</td><td>PA7</td><td>PB5</td><td>PC7</td></tr><tr><td>TIM3_CH3</td><td>PBO</td><td>PBO</td><td>PC8</td></tr><tr><td>TIM3_CH4</td><td>PB1</td><td>PB1</td><td>PC9</td></tr></table>

注：（1）64脚以下封装不支持该位映射。

表 10-18 TIM4 复用功能重映射  

<table><tr><td>复用功能</td><td>TIM4_RM=0
默认映射</td><td>TIM4_RM=1
重映射(1)</td></tr><tr><td>TIM4_CH1</td><td>PB6</td><td>PD12</td></tr><tr><td>TIM4_CH2</td><td>PB7</td><td>PD13</td></tr><tr><td>TIM4_CH3</td><td>PB8</td><td>PD14</td></tr><tr><td>TIM4_CH4</td><td>PB9</td><td>PD15</td></tr></table>

注：（1）仅LQFP100封装，支持该位重映射功能。

表 10-19 TIM5 复用功能重映射  

<table><tr><td>复用功能</td><td>TIM5CH4_RM=0
默认映射</td><td>TIM5CH4_RM=1
重映射</td></tr><tr><td>TIM5_CH4</td><td>PA3</td><td>LSI 内部时钟</td></tr></table>

表 10-20 TIM8 复用功能重映射（1）  

<table><tr><td>复用功能</td><td>TIM8_RM=0
默认映射</td><td>TIM8_RM=1
重映射</td></tr><tr><td>TIM8_ETR</td><td>PA0</td><td>PA0</td></tr><tr><td>TIM8_CH1</td><td>PC6</td><td>PB6</td></tr><tr><td>TIM8_CH2</td><td>PC7</td><td>PB7</td></tr><tr><td>TIM8_CH3</td><td>PC8</td><td>PB8</td></tr><tr><td>TIM8_CH4</td><td>PC9</td><td>PC13</td></tr><tr><td>TIM8_BKIN</td><td>PA6</td><td>PB9</td></tr><tr><td>TIM8_CH1N</td><td>PA7</td><td>PA13</td></tr><tr><td>TIM8_CH2N</td><td>PBO</td><td>PA14</td></tr><tr><td>TIM8_CH3N</td><td>PB1</td><td>PA15</td></tr></table>

表 10-21 TIM9 复用功能重映射  

<table><tr><td>复用功能</td><td>TIM9_RM=00
默认映射</td><td>TIM9_RM=01
部分映射</td><td>TIM9_RM=1x
完全映射(1)</td></tr><tr><td>TIM9_ETR</td><td>PA2</td><td>PA2</td><td>PD9</td></tr><tr><td>TIM9_CH1</td><td>PA2</td><td>PA2</td><td>PD9</td></tr><tr><td>TIM9_CH2</td><td>PA3</td><td>PA3</td><td>PD11</td></tr><tr><td>TIM9_CH3</td><td>PA4</td><td>PA4</td><td>PD13</td></tr><tr><td>TIM9_CH4</td><td>PC4</td><td>PC14</td><td>PD15</td></tr><tr><td>TIM9_BKIN</td><td>PC5</td><td>PA1</td><td>PD14</td></tr><tr><td>TIM9_CH1N</td><td>PC0</td><td>PBO</td><td>PD8</td></tr><tr><td>TIM9_CH2N</td><td>PC1</td><td>PB1</td><td>PD10</td></tr><tr><td>TIM9_CH3N</td><td>PC2</td><td>PB2</td><td>PD12</td></tr></table>

注：（1）仅LQFP100封装，支持该位重映射功能。

表 10-22 TIM10 复用功能重映射  

<table><tr><td>复用功能</td><td>TIM10_RM=00
默认映射</td><td>TIM10_RM=01
部分映射</td><td>TIM10_RM=1x
完全映射(1)</td></tr><tr><td>TIM10_ETR</td><td>PC10</td><td>PB11</td><td>PD0</td></tr><tr><td>TIM10_CH1</td><td>PB8</td><td>PB3</td><td>PD1</td></tr><tr><td>TIM10_CH2</td><td>PB9</td><td>PB4</td><td>PD3</td></tr><tr><td>TIM10_CH3</td><td>PC3</td><td>PB5</td><td>PD5</td></tr><tr><td>TIM10_CH4</td><td>PC11</td><td>PC15</td><td>PD7</td></tr><tr><td>TIM10_BKIN</td><td>PC12</td><td>PB10</td><td>PE2</td></tr><tr><td>TIM10_CH1N</td><td>PA12</td><td>PA5</td><td>PE3</td></tr><tr><td>TIM10_CH2N</td><td>PA13</td><td>PA6</td><td>PE4</td></tr><tr><td>TIM10_CH3N</td><td>PA14</td><td>PA7</td><td>PE5</td></tr></table>

注：（1）仅LQFP100封装，支持该位重映射功能。

#### 10.2.11.4 USART、UART 复用功能重映射

表 10-23 USART1 复用功能重映射  

<table><tr><td rowspan="3">复用功能</td><td>USART1_RM1=0 (1)</td><td>USART1_RM1=1 (1)</td><td>USART1_RM1=0 (1)</td><td>USART1_RM1=1 (1)</td></tr><tr><td>USART1_RM2=0 (2)</td><td>USART1_RM2=0 (2)</td><td>USART1_RM2=1 (2)</td><td>USART1_RM2=1 (2)</td></tr><tr><td>默认映射</td><td>重映射</td><td>重映射</td><td>重映射</td></tr><tr><td>USART1_CK</td><td>PA8</td><td>PA8</td><td>PA10</td><td>PA5</td></tr><tr><td>USART1_TX</td><td>PA9</td><td>PB6</td><td>PB15</td><td>PA6</td></tr><tr><td>USART1_RX</td><td>PA10</td><td>PB7</td><td>PA8</td><td>PA7</td></tr><tr><td>USART1_CTS</td><td>PA11</td><td>PA11</td><td>PA5</td><td>PC4</td></tr><tr><td>USART1_RTS</td><td>PA12</td><td>PA12</td><td>PA9</td><td>PC5</td></tr></table>

注：（1）USART1_RM1为AFI0_PCFR1寄存器bit2，为映射配置低位。  
（2）USART1_RM2为AFI0_PCFR2寄存器 bit26，为映射配置高位，CH32V20x_D6、CH32F20x_D6不支持该位映射。

表 10-24 USART2 复用功能重映射  

<table><tr><td>复用功能</td><td>USART2_RM=0
默认映射</td><td>USART2_RM=1
重映射(1)</td></tr><tr><td>USART2_CTS</td><td>PA0</td><td>PD3</td></tr><tr><td>USART2_RTS</td><td>PA1</td><td>PD4</td></tr><tr><td>USART2_TX</td><td>PA2</td><td>PD5</td></tr><tr><td>USART2_RX</td><td>PA3</td><td>PD6</td></tr><tr><td>USART2_CK</td><td>PA4</td><td>PD7</td></tr></table>

表 10-25 USART3 复用功能重映射  

<table><tr><td>复用功能</td><td>USART3_RM=00
默认映射</td><td>USART3_RM=01
部分映射(1)</td><td>USART3_RM=10
部分映射(2)</td><td>USART3_RM=11
完全映射(3)</td></tr><tr><td>USART3_TX</td><td>PB10</td><td>PC10</td><td>PA13</td><td>PD8</td></tr><tr><td>USART3_RX</td><td>PB11</td><td>PC11</td><td>PA14</td><td>PD9</td></tr><tr><td>USART3_CK</td><td>PB12</td><td>PC12</td><td>PD10</td><td>PD10</td></tr><tr><td>USART3_CTS</td><td>PB13</td><td>PB13</td><td>PD11</td><td>PD11</td></tr><tr><td>USART3_RTS</td><td>PB14</td><td>PB14</td><td>PD12</td><td>PD12</td></tr></table>

注：（1）48脚以下封装不支持该位映射。  
（2）批号第五位小于2的不支持该功能。  
（3）仅LQFP100封装，支持该位重映射功能。

表 10-26 UART4 复用功能重映射  

<table><tr><td>复用功能(1)</td><td>UART4_RM=00
默认映射</td><td>UART4_RM=01
重映射</td><td>UART4_RM=1x
重映射(2)</td></tr><tr><td>UART4_TX</td><td>PC10</td><td>PBO</td><td>PE0</td></tr><tr><td>UART4_RX</td><td>PC11</td><td>PB1</td><td>PE1</td></tr></table>

注：（1）适用于 CH32V30x_D8C、CH32V30x_D8、CH32V30x_D8W、CH32V20x_D8、CH32F20x_D8C、CH32F20x_D8、CH32F20x_D8W。

（2）仅LQFP100封装，支持该位重映射功能。

表 10-27 USART4 复用功能重映射（ 2）  

<table><tr><td>复用功能</td><td>USART4_RM=x0
默认映射</td><td>USART4_RM=x1
重映射</td></tr><tr><td>USART4_CK</td><td>PB2</td><td>PA6</td></tr><tr><td>USART4_TX</td><td>PBO</td><td>PA5</td></tr><tr><td>USART4_RX</td><td>PB1</td><td>PB5</td></tr><tr><td>USART4_CTS</td><td>PB3</td><td>PA7</td></tr><tr><td>USART4_RTS</td><td>PB4</td><td>PA15</td></tr></table>

注：（1）仅LQFP100封装，支持该位重映射功能。  
（2）该功能不支持64引脚以下产品。

表 10-28 UART5 复用功能重映射（2）  

<table><tr><td>复用功能</td><td>UART5_RM=00
默认映射</td><td>UART5_RM=01
重映射</td><td>UART5_RM=1x
重映射(1)</td></tr><tr><td>UART5_TX</td><td>PC12</td><td>PB4</td><td>PE8</td></tr><tr><td>UART5_RX</td><td>PD2</td><td>PB5</td><td>PE9</td></tr></table>

注：（1）仅LQFP100封装，支持该位重映射功能。  
（2）该功能不支持64引脚以下产品。

表 10-29 UART6 复用功能重映射（2）  

<table><tr><td>复用功能</td><td>UART6_RM=00
默认映射(2)</td><td>UART6_RM=01
重映射</td><td>UART6_RM=1x
重映射(1)</td></tr><tr><td>UART6_TX</td><td>PC0</td><td>PB8</td><td>PE10</td></tr><tr><td>UART6_RX</td><td>PC1</td><td>PB9</td><td>PE11</td></tr></table>

注：（1）仅LQFP100封装，支持该位重映射功能。  
（2）该功能不支持64引脚以下产品。

表 10-30 UART7 复用功能重映射 （2）  

<table><tr><td>复用功能</td><td>UART7_RM=00
默认映射</td><td>UART7_RM=01
重映射</td><td>UART7_RM=1x
重映射(1)</td></tr><tr><td>UART7_TX</td><td>PC2</td><td>PA6</td><td>PE12</td></tr><tr><td>UART7_RX</td><td>PC3</td><td>PA7</td><td>PE13</td></tr></table>

注：（1）仅LQFP100封装，支持该位重映射功能。  
（2）该功能不支持64引脚以下产品。

表 10-31 UART8 复用功能重映射（2）  

<table><tr><td>复用功能</td><td>UART8_RM=00
默认映射</td><td>UART8_RM=01
重映射</td><td>UART8_RM=1x
重映射(1)</td></tr><tr><td>UART8_TX</td><td>PC4</td><td>PA14</td><td>PE14</td></tr><tr><td>UART8_RX</td><td>PC5</td><td>PA15</td><td>PE15</td></tr></table>

注：（1）仅LQFP100封装，支持该位重映射功能。  
（2）该功能不支持64引脚以下产品。

#### 10.2.11.5 SPI 复用功能重映射

表 10-32 SPI1 复用功能重映射  

<table><tr><td>复用功能</td><td>SPI1_RM=0
默认映射</td><td>SPI1_RM=1
重映射</td></tr><tr><td>SPI1_NSS</td><td>PA4</td><td>PA15</td></tr><tr><td>SPI1_SCK</td><td>PA5</td><td>PB3</td></tr><tr><td>SPI1_MISO</td><td>PA6</td><td>PB4</td></tr><tr><td>SPI1_MOSI</td><td>PA7</td><td>PB5</td></tr></table>

表 10-33 SPI3 复用功能重映射  

<table><tr><td>复用功能(1)</td><td>SPI1_RM=0
默认映射</td><td>SPI1_RM=1
重映射</td></tr><tr><td>SPI3_NSS</td><td>PA15</td><td>PA4</td></tr><tr><td>SPI3_SCK</td><td>PB3</td><td>PC10</td></tr><tr><td>SPI3_MISO</td><td>PB4</td><td>PC11</td></tr><tr><td>SPI3_MOSI</td><td>PB5</td><td>PC12</td></tr></table>

#### 10.2.11.6 I2C 复用功能重映射

表 10-34 I2C1 复用功能重映射  

<table><tr><td>复用功能</td><td>I2C1_RM=0
默认映射</td><td>I2C1_RM=1
重映射</td></tr><tr><td>I2C1_SCL</td><td>PB6</td><td>PB8</td></tr><tr><td>I2C1_SDA</td><td>PB7</td><td>PB9</td></tr></table>

#### 10.2.11.7 CAN 复用功能重映射

表 10-35 CAN1 复用功能重映射  

<table><tr><td>复用功能</td><td>CAN1_RM=00
默认映射</td><td>CAN1_RM=10
重映射(1)</td><td>CAN1_RM=11
重映射(2)</td></tr><tr><td>CAN1_RX</td><td>PA11</td><td>PB8</td><td>PDO</td></tr><tr><td>CAN1_TX</td><td>PA12</td><td>PB9</td><td>PD1</td></tr></table>

注：（1）重映射不适用于48脚以下封装的芯片。  
（2）当PDO和PD1没有被重映射到OSC_IN和OSC_OUT时，重映射功能只适用于100脚的封装上。

表 10-36 CAN2 复用功能重映射  

<table><tr><td>复用功能</td><td>CAN2_RM=0
默认映射</td><td>CAN2_RM=1
重映射</td></tr><tr><td>CAN2_RX</td><td>PB12</td><td>PB5</td></tr><tr><td>CAN2_TX</td><td>PB13</td><td>PB6</td></tr></table>

#### 10.2.11.8 ADC 复用功能重映射

表10-37 ADC1外部触发注入转换复用功能重映射  

<table><tr><td>复用功能</td><td>ADC1_ETRGINJ_RM=0
默认映射</td><td>ADC1_ETRGINJ_RM=1
重映射</td></tr><tr><td>ADC1外部触发注入转换</td><td>ADC1外部触发注入转换与EXT115相连</td><td>ADC1外部触发注入转换与TIM8_CH4相连</td></tr></table>

表 10-38 ADC1 外部触发规则转换复用功能重映射  

<table><tr><td>复用功能</td><td>ADC1_ETRGREG_RM=0
默认映射</td><td>ADC1_ETRGREG_RM=1
重映射</td></tr><tr><td>ADC1外部触发规则转换</td><td>ADC1外部触发规则转换与EXT111相连</td><td>ADC1外部触发规则转换与TIM8_TRGO相连</td></tr></table>

表10-39 ADC2外部触发注入转换复用功能重映射  

<table><tr><td>复用功能</td><td>ADC2_ETRGINJ_RM=0
默认映射</td><td>ADC2_ETRGINJ_RM=1
重映射</td></tr><tr><td>ADC2外部触发注入转换</td><td>ADC2外部触发注入转换与EXT115相连</td><td>ADC2外部触发注入转换与TIM8_CH4相连</td></tr></table>

表 10-40 ADC2 外部触发规则转换复用功能重映射  

<table><tr><td>复用功能</td><td>ADC2_ETRGREG_RM=0
默认映射</td><td>ADC2_ETRGREG_RM=1
重映射</td></tr><tr><td>ADC2外部触发规则转换</td><td>ADC2外部触发规则转换与EXT111相连</td><td>ADC2外部触发规则转换与TIM8_TRGO相连</td></tr></table>

注：ADC1重映射只支持存在TIM8的产品，详细信息见相关数据手册。

#### 10.2.11.9 ETH 复用功能重映射

表 10-41 ETH 复用功能重映射  

<table><tr><td>复用功能(1)</td><td>ETH_RM=0
默认映射</td><td>ETH_RM=1
重映射(1)</td></tr><tr><td>ETH_RX_DV</td><td>PA7</td><td>PD8</td></tr><tr><td>ETH_CRS_DV</td><td>PA7</td><td>PD8</td></tr><tr><td>ETH_RXD0</td><td>PC4</td><td>PD9</td></tr><tr><td>ETH_RXD1</td><td>PC5</td><td>PD10</td></tr><tr><td>ETH_RXD2</td><td>PBO</td><td>PD11</td></tr><tr><td>ETH_RXD3</td><td>PB1</td><td>PD12</td></tr></table>

注：（1）仅LQFP100封装互联型，支持该位重映射功能。

#### 10.2.11.10 FSMC_NADV 复用功能重映射（2）

表 10-42 FSMC_NADV 复用功能重映射  

<table><tr><td>复用功能(1)</td><td>FSMCEN=1
默认映射</td><td>FSMCEN=1&amp;USBHSEN=1
&amp;R_BUS_RST_SIE=0</td></tr><tr><td>FSMC_NADV</td><td>PB7</td><td>PD2</td></tr></table>

注：（1)当FSMC_NADV=1时禁止FSMC_NADV输出。  
(2)批号第五位小于2的不支持该功能。

#### 10.2.11.11 DVP 复用功能重映射 （1）

表 10-43 DVP 复用功能重映射  

<table><tr><td>复用功能</td><td>DVPEN=1
默认映射</td><td>DVPEN=1&amp;USBHSEN=1
&amp;R_B_UC_RST_SIE=0</td></tr><tr><td>DVP_D5</td><td>PB6</td><td>PB3</td></tr></table>

注：（1）批号第五位小于2的不支持该功能。

#### 10.2.11.12 SDIO 复用功能重映射（1）

表 10-44 SDIO 复用功能重映射  

<table><tr><td>复用功能</td><td>SDIOEN=1</td><td>SDIOEN=1&amp;ETHMACEN=1</td></tr><tr><td>SDCK</td><td>PC12</td><td>PC12</td></tr><tr><td>CMD</td><td>PD2</td><td>PD2</td></tr><tr><td>SD0</td><td>PC8</td><td>PB14</td></tr><tr><td>SD1</td><td>PC9</td><td>PB15</td></tr><tr><td>SD2</td><td>PC10</td><td>PC10</td></tr><tr><td>SD3</td><td>PC11</td><td>PC11</td></tr><tr><td>SD4</td><td>PB8</td><td>PB8</td></tr><tr><td>SD5</td><td>PB9</td><td>PB9</td></tr><tr><td>SD6</td><td>PC6</td><td>PC6</td></tr><tr><td>SD7</td><td>PC7</td><td>PC7</td></tr></table>

注：（1）批号第五位小于2的不支持该功能。

## 10.3 寄存器描述

### 10.3.1 GPIO 的寄存器描述

除非特殊说明，GPIO 的寄存器必须以字的方式操作（以 32 位来操作这些寄存器）。

表 10-45 GPIO 相关寄存器列表  

<table><tr><td>名称</td><td>访问地址</td><td>描述</td><td>复位值</td></tr><tr><td>R32_GPI0A_CFGLR</td><td>0x40010800</td><td>PA端口配置寄存器低位</td><td>0x44444444</td></tr><tr><td>R32_GPI0B_CFGLR</td><td>0x40010C00</td><td>PB端口配置寄存器低位</td><td>0x44444444</td></tr><tr><td>R32_GPI0C_CFGLR</td><td>0x40011000</td><td>PC端口配置寄存器低位</td><td>0x44444444</td></tr><tr><td>R32_GPI0D_CFGLR</td><td>0x40011400</td><td>PD端口配置寄存器低位</td><td>0x44444444</td></tr><tr><td>R32_GPI0E_CFGLR</td><td>0x40011800</td><td>PE端口配置寄存器低位</td><td>0x44444444</td></tr><tr><td>R32_GPI0A_CFGHR</td><td>0x40010804</td><td>PA端口配置寄存器高位</td><td>0x44444444</td></tr><tr><td>R32_GPI0B_CFGHR</td><td>0x40010C04</td><td>PB端口配置寄存器高位</td><td>0x44444444</td></tr><tr><td>R32_GPI0C_CFGHR</td><td>0x40011004</td><td>PC端口配置寄存器高位</td><td>0x44444444</td></tr><tr><td>R32_GPI0D_CFGHR</td><td>0x40011404</td><td>PD端口配置寄存器高位</td><td>0x44444444</td></tr><tr><td>R32_GPI0E_CFGHR</td><td>0x40011804</td><td>PE端口配置寄存器高位</td><td>0x44444444</td></tr><tr><td>R32_GPI0A_INDR</td><td>0x40010808</td><td>PA端口输入数据寄存器</td><td>0x0000XXX</td></tr><tr><td>R32_GPIOB_INDR</td><td>0x40010C08</td><td>PB端口输入数据寄存器</td><td>0x0000XXX</td></tr><tr><td>R32_GPIOC_INDR</td><td>0x40011008</td><td>PC端口输入数据寄存器</td><td>0x0000XXX</td></tr><tr><td>R32_GPIOD_INDR</td><td>0x40011408</td><td>PD端口输入数据寄存器</td><td>0x0000XXX</td></tr><tr><td>R32_GPIOE_INDR</td><td>0x40011808</td><td>PE端口输入数据寄存器</td><td>0x0000XXX</td></tr><tr><td>R32_GPIOA_OUTDR</td><td>0x4001080C</td><td>PA端口输出数据寄存器</td><td>0x0000000</td></tr><tr><td>R32_GPIOB_OUTDR</td><td>0x40010C0C</td><td>PB端口输出数据寄存器</td><td>0x0000000</td></tr><tr><td>R32_GPIOC_OUTDR</td><td>0x4001100C</td><td>PC端口输出数据寄存器</td><td>0x0000000</td></tr><tr><td>R32_GPIOD_OUTDR</td><td>0x4001140C</td><td>PD端口输出数据寄存器</td><td>0x0000000</td></tr><tr><td>R32_GPIOE_OUTDR</td><td>0x4001180C</td><td>PE端口输出数据寄存器</td><td>0x0000000</td></tr><tr><td>R32_GPIOA_BSHR</td><td>0x40010810</td><td>PA端口置位/复位寄存器</td><td>0x0000000</td></tr><tr><td>R32_GPIOB_BSHR</td><td>0x40010C10</td><td>PB端口置位/复位寄存器</td><td>0x0000000</td></tr><tr><td>R32_GPIOC_BSHR</td><td>0x40011010</td><td>PC端口置位/复位寄存器</td><td>0x0000000</td></tr><tr><td>R32_GPIOD_BSHR</td><td>0x40011410</td><td>PD端口置位/复位寄存器</td><td>0x0000000</td></tr><tr><td>R32_GPIOE_BSHR</td><td>0x40011810</td><td>PE端口置位/复位寄存器</td><td>0x0000000</td></tr><tr><td>R32_GPIOA_BCR</td><td>0x40010814</td><td>PA端口复位寄存器</td><td>0x0000000</td></tr><tr><td>R32_GPIOB_BCR</td><td>0x40010C14</td><td>PB端口复位寄存器</td><td>0x0000000</td></tr><tr><td>R32_GPIOC_BCR</td><td>0x40011014</td><td>PC端口复位寄存器</td><td>0x0000000</td></tr><tr><td>R32_GPIOD_BCR</td><td>0x40011414</td><td>PD端口复位寄存器</td><td>0x0000000</td></tr><tr><td>R32_GPIOE_BCR</td><td>0x40011814</td><td>PE端口复位寄存器</td><td>0x0000000</td></tr><tr><td>R32_GPIOA_LCKR</td><td>0x40010818</td><td>PA端口锁定配置寄存器</td><td>0x0000000</td></tr><tr><td>R32_GPIOB_LCKR</td><td>0x40010C18</td><td>PB端口锁定配置寄存器</td><td>0x0000000</td></tr><tr><td>R32_GPIOC_LCKR</td><td>0x40011018</td><td>PC端口锁定配置寄存器</td><td>0x0000000</td></tr><tr><td>R32_GPIOD_LCKR</td><td>0x40011418</td><td>PD端口锁定配置寄存器</td><td>0x0000000</td></tr><tr><td>R32_GPIOE_LCKR</td><td>0x40011818</td><td>PE端口锁定配置寄存器</td><td>0x0000000</td></tr></table>

#### 10.3.1.1 GPIO 配置寄存器低位（GPIOx_CFGLR）（x=A/B/C/D/E）

偏移地址： $0 \times 0 0$

<table><tr><td>31</td><td>30</td><td>29</td><td>28</td><td>27</td><td>26</td><td>25</td><td>24</td><td>23</td><td>22</td><td>21</td><td>20</td><td>19</td><td>18</td><td>17</td><td>16</td></tr><tr><td colspan="2">CNF7[1:0]</td><td colspan="2">MODE7[1:0]</td><td colspan="2">CNF6[1:0]</td><td colspan="2">MODE6[1:0]</td><td colspan="2">CNF5[1:0]</td><td colspan="2">MODE5[1:0]</td><td colspan="2">CNF4[1:0]</td><td colspan="2">MODE4[1:0]</td></tr><tr><td>15</td><td>14</td><td>13</td><td>12</td><td>11</td><td>10</td><td>9</td><td>8</td><td>7</td><td>6</td><td>5</td><td>4</td><td>3</td><td>2</td><td>1</td><td>0</td></tr><tr><td colspan="2">CNF3[1:0]</td><td colspan="2">MODE3[1:0]</td><td colspan="2">CNF2[1:0]</td><td colspan="2">MODE2[1:0]</td><td colspan="2">CNF1[1:0]</td><td colspan="2">MODE1[1:0]</td><td colspan="2">CNF0[1:0]</td><td colspan="2">MODE0[1:0]</td></tr></table>

<table><tr><td>位</td><td>名称</td><td>访问</td><td>描述</td><td>复位值</td></tr><tr><td>[31:30]</td><td rowspan="8">CNFy[1:0]</td><td rowspan="8">RW</td><td rowspan="8">(y=0-7),端口x的配置位,通过这些位配置相应的端口。在输入模式时(MODE=00b):00:模拟输入模式;01:浮空输入模式;10:带有上下拉模式。11:保留。在输出模式(MODE&gt;00b):00:通用推挽输出模式;01:通用开漏输出模式;</td><td rowspan="8">01b</td></tr><tr><td>[27:26]</td></tr><tr><td>[23:22]</td></tr><tr><td>[19:18]</td></tr><tr><td>[15:14]</td></tr><tr><td>[11:10]</td></tr><tr><td>[7:6]</td></tr><tr><td>[3:2]</td></tr><tr><td></td><td></td><td></td><td>10: 复用功能推挽输出模式;11: 复用功能开漏输出模式。</td><td></td></tr><tr><td>[29:28] [25:24] [21:20] [17:16] [13:12] [9:8] [5:4] [1:0]</td><td>MODEy[1:0]</td><td>RW</td><td>(y=0-7),端口x模式选择,通过这些位配置相应的端口。00:输入模式;01:输出模式,最大速度10MHz;10:输出模式,最大速度2MHz;11:输出模式,最大速度50MHz。</td><td>00b</td></tr></table>

#### 10.3.1.2 GPIO 配置寄存器高位（GPIOx_CFGHR）（x=A/B/C/D/E）

偏移地址： $0 \times 0 4$

<table><tr><td>31</td><td>30</td><td>29</td><td>28</td><td>27</td><td>26</td><td>25</td><td>24</td><td>23</td><td>22</td><td>21</td><td>20</td><td>19</td><td>18</td><td>17</td><td>16</td></tr><tr><td colspan="2">CNF15[1:0]</td><td colspan="2">MODE15[1:0]</td><td colspan="2">CNF14[1:0]</td><td colspan="2">MODE14[1:0]</td><td colspan="2">CNF13[1:0]</td><td colspan="2">MODE13[1:0]</td><td colspan="2">CNF12[1:0]</td><td colspan="2">MODE12[1:0]</td></tr><tr><td>15</td><td>14</td><td>13</td><td>12</td><td>11</td><td>10</td><td>9</td><td>8</td><td>7</td><td>6</td><td>5</td><td>4</td><td>3</td><td>2</td><td>1</td><td>0</td></tr><tr><td colspan="2">CNF11[1:0]</td><td colspan="2">MODE11[1:0]</td><td colspan="2">CNF10[1:0]</td><td colspan="2">MODE10[1:0]</td><td colspan="2">CNF9[1:0]</td><td colspan="2">MODE9[1:0]</td><td colspan="2">CNF8[1:0]</td><td colspan="2">MODE8[1:0]</td></tr></table>

<table><tr><td>位</td><td>名称</td><td>访问</td><td>描述</td><td>复位值</td></tr><tr><td>[31:30]</td><td rowspan="8">CNFy[1:0]</td><td rowspan="8">RW</td><td rowspan="8">(y=8-15),端口x的配置位,通过这些位配置相应的端口。在输入模式时(MODE=00b):00:模拟输入模式;01:浮空输入模式;10:带有上下拉模式。11:保留。在输出模式(MODE&gt;00b):00:通用推挽输出模式;01:通用开漏输出模式;10:复用功能推挽输出模式;11:复用功能开漏输出模式。</td><td rowspan="8">01b</td></tr><tr><td>[27:26]</td></tr><tr><td>[23:22]</td></tr><tr><td>[19:18]</td></tr><tr><td>[15:14]</td></tr><tr><td>[11:10]</td></tr><tr><td>[7:6]</td></tr><tr><td>[3:2]</td></tr><tr><td>[29:28]</td><td rowspan="8">MODEy[1:0]</td><td rowspan="8">RW</td><td rowspan="8">(y=8-15),端口x的模式位,通过这些位配置相应的端口。00:输入模式;01:输出模式,最大速度10MHz;10:输出模式,最大速度2MHz;11:输出模式,最大速度50MHz。</td><td rowspan="8">00b</td></tr><tr><td>[25:24]</td></tr><tr><td>[21:20]</td></tr><tr><td>[17:16]</td></tr><tr><td>[13:12]</td></tr><tr><td>[9:8]</td></tr><tr><td>[5:4]</td></tr><tr><td>[1:0]</td></tr></table>

#### 10.3.1.3 端口输入寄存器（GPIOx_INDR）（x=A/B/C/D/E）

偏移地址： $0 \times 0 8$

<table><tr><td>31</td><td>30</td><td>29</td><td>28</td><td>27</td><td>26</td><td>25</td><td>24</td><td>23</td><td>22</td><td>21</td><td>20</td><td>19</td><td>18</td><td>17</td><td>16</td></tr><tr><td colspan="16">Reserved</td></tr><tr><td>15</td><td>14</td><td>13</td><td>12</td><td>11</td><td>10</td><td>9</td><td>8</td><td>7</td><td>6</td><td>5</td><td>4</td><td>3</td><td>2</td><td>1</td><td>0</td></tr><tr><td>IDR15</td><td>IDR14</td><td>IDR13</td><td>IDR12</td><td>IDR11</td><td>IDR10</td><td>IDR9</td><td>IDR8</td><td>IDR7</td><td>IDR6</td><td>IDR5</td><td>IDR4</td><td>IDR3</td><td>IDR2</td><td>IDR1</td><td>IDR0</td></tr></table>

<table><tr><td>位</td><td>名称</td><td>访问</td><td>描述</td><td>复位值</td></tr><tr><td>[31:16]</td><td>Reserved</td><td>R0</td><td>保留。</td><td>0</td></tr><tr><td>[15:0]</td><td>IDRy</td><td>R0</td><td>(y=0-15)，端口输入数据。这些位只读并只能以16位形式读出。读出的值就是对应位的高低状态。</td><td>X</td></tr></table>

#### 10.3.1.4 端口输出寄存器（GPIOx_OUTDR）（x=A/B/C/D/E）

偏移地址： $0 \times 0 0$

<table><tr><td>31</td><td>30</td><td>29</td><td>28</td><td>27</td><td>26</td><td>25</td><td>24</td><td>23</td><td>22</td><td>21</td><td>20</td><td>19</td><td>18</td><td>17</td><td>16</td></tr><tr><td colspan="16">Reserved</td></tr><tr><td>15</td><td>14</td><td>13</td><td>12</td><td>11</td><td>10</td><td>9</td><td>8</td><td>7</td><td>6</td><td>5</td><td>4</td><td>3</td><td>2</td><td>1</td><td>0</td></tr><tr><td>ODR15</td><td>ODR14</td><td>ODR13</td><td>ODR12</td><td>ODR11</td><td>ODR10</td><td>ODR9</td><td>ODR8</td><td>ODR7</td><td>ODR6</td><td>ODR5</td><td>ODR4</td><td>ODR3</td><td>ODR2</td><td>ODR1</td><td>ODR0</td></tr></table>

<table><tr><td>位</td><td>名称</td><td>访问</td><td>描述</td><td>复位值</td></tr><tr><td>[31:16]</td><td>Reserved</td><td>R0</td><td>保留。</td><td>0</td></tr><tr><td>[15:0]</td><td>ODRy</td><td>RW</td><td>对于输出模式:(y=0-15),端口输出的数据。这些数据只能以16位的形式操作。10口对外输出这些寄存器的值。对于带有上下拉的输入模式:0:下拉输入;1:上拉输入。</td><td>0</td></tr></table>

#### 10.3.1.5 端口复位/置位寄存器（GPIOx_BSHR）（x=A/B/C/D/E）

偏移地址： $0 \times 1 0$

<table><tr><td>31</td><td>30</td><td>29</td><td>28</td><td>27</td><td>26</td><td>25</td><td>24</td><td>23</td><td>22</td><td>21</td><td>20</td><td>19</td><td>18</td><td>17</td><td>16</td></tr><tr><td>BR15</td><td>BR14</td><td>BR13</td><td>BR12</td><td>BR11</td><td>BR10</td><td>BR9</td><td>BR8</td><td>BR7</td><td>BR6</td><td>BR5</td><td>BR4</td><td>BR3</td><td>BR2</td><td>BR1</td><td>BR0</td></tr><tr><td>15</td><td>14</td><td>13</td><td>12</td><td>11</td><td>10</td><td>9</td><td>8</td><td>7</td><td>6</td><td>5</td><td>4</td><td>3</td><td>2</td><td>1</td><td>0</td></tr><tr><td>BS15</td><td>BS14</td><td>BS13</td><td>BS12</td><td>BS11</td><td>BS10</td><td>BS9</td><td>BS8</td><td>BS7</td><td>BS6</td><td>BS5</td><td>BS4</td><td>BS3</td><td>BS2</td><td>BS1</td><td>BS0</td></tr></table>

<table><tr><td>位</td><td>名称</td><td>访问</td><td>描述</td><td>复位值</td></tr><tr><td>[31:16]</td><td>BRy</td><td>WO</td><td>(y=0-15)，对这些位置位会清除对应的 OUTDR 位，写0不产生影响。这些位只能以16位的形式访问。如果同时设置了BR和BS位，则BS位起作用。</td><td>0</td></tr><tr><td>[15:0]</td><td>BSy</td><td>WO</td><td>(y=0-15)，对这些位置位会使对应的 OUTDR 位置位，写0不产生影响。这些位只能以16位的形式访问。如果同时设置了BR和BS位，则BS位起作用。</td><td>0</td></tr></table>

#### 10.3.1.6 端口复位寄存器（GPIOx_BCR）（x=A/B/C/D/E）

偏移地址： $0 \times 1 4$

<table><tr><td>31</td><td>30</td><td>29</td><td>28</td><td>27</td><td>26</td><td>25</td><td>24</td><td>23</td><td>22</td><td>21</td><td>20</td><td>19</td><td>18</td><td>17</td><td>16</td></tr><tr><td>15</td><td>14</td><td>13</td><td>12</td><td>11</td><td>10</td><td>9</td><td>8</td><td>7</td><td>6</td><td>5</td><td>4</td><td>3</td><td>2</td><td>1</td><td>0</td></tr><tr><td>BR15</td><td>BR14</td><td>BR13</td><td>BR12</td><td>BR11</td><td>BR10</td><td>BR9</td><td>BR8</td><td>BR7</td><td>BR6</td><td>BR5</td><td>BR4</td><td>BR3</td><td>BR2</td><td>BR1</td><td>BR0</td></tr></table>

<table><tr><td>位</td><td>名称</td><td>访问</td><td>描述</td><td>复位值</td></tr><tr><td>[31:16]</td><td>Reserved</td><td>R0</td><td>保留。</td><td>0</td></tr><tr><td>[15:0]</td><td>BRy</td><td>WO</td><td>(y=0-15)，对这些位置位会清除对应的 OUTDR 位，写0不产生影响。这些位只能以16位的形式访问。</td><td>0</td></tr></table>

#### 10.3.1.7 配置锁定寄存器（GPIOx_LCKR）（x=A/B/C/D/E）

偏移地址： $0 \times 1 8$

<table><tr><td>31</td><td>30</td><td>29</td><td>28</td><td>27</td><td>26</td><td>25</td><td>24</td><td>23</td><td>22</td><td>21</td><td>20</td><td>19</td><td>18</td><td>17</td><td>16</td></tr><tr><td colspan="15">Reserved</td><td>LCKK</td></tr><tr><td>15</td><td>14</td><td>13</td><td>12</td><td>11</td><td>10</td><td>9</td><td>8</td><td>7</td><td>6</td><td>5</td><td>4</td><td>3</td><td>2</td><td>1</td><td>0</td></tr><tr><td>LCK15</td><td>LCK14</td><td>LCK13</td><td>LCK12</td><td>LCK11</td><td>LCK10</td><td>LCK9</td><td>LCK8</td><td>LCK7</td><td>LCK6</td><td>LCK5</td><td>LCK4</td><td>LCK3</td><td>LCK2</td><td>LCK1</td><td>LCK0</td></tr></table>

<table><tr><td>位</td><td>名称</td><td>访问</td><td>描述</td><td>复位值</td></tr><tr><td>[31:17]</td><td>Reserved</td><td>R0</td><td>保留</td><td>0</td></tr><tr><td>16</td><td>LCKK</td><td>RW</td><td>锁定键,它可以通过特定的序列写入实现锁定,但它可以随时读出。它读出为0时表示未锁定生效,读出1时表示锁定生效。锁定键的写入序列为:写1-写0-写1-读0-读1,最后一步非必要,但是可以用以确认锁定键已经激活。在写入序列时任何错误都不会使激活锁定,且在写入序列时,不能更改LCK[15:0]的值。锁定生效后,只有在下次复位后才能更改端口的配置。</td><td>0</td></tr><tr><td>[15:0]</td><td>LCKy</td><td>RW</td><td>(y=0-15),这些位为1时表示锁定对应端口的配置。只能在LCKK未锁定前改变这些位。锁定的配置指的是配置寄存器GPIOx_CFGLR和GPIOx_CFGHR。</td><td>0</td></tr></table>

注：当对相应的端口位执行了LOCK序列后，在下次系统复位之前将不能再更改端口位的配置。

### 10.3.2 AFIO 寄存器

除非特殊说明，AFIO的寄存器必须以字的方式操作（以 32位来操作这些寄存器）。

表 10-46 AFIO 相关寄存器列表  

<table><tr><td>名称</td><td>访问地址</td><td>描述</td><td>复位值</td></tr><tr><td>R32_AFIO_ECR</td><td>0x40010000</td><td>事件控制寄存器</td><td>0x00000000</td></tr><tr><td>R32_AFIO_PCFR1</td><td>0x40010004</td><td>重映射寄存器1</td><td>0x00000000</td></tr><tr><td>R32_AFIO_EXTCR1</td><td>0x40010008</td><td>外部中断配置寄存器1</td><td>0x00000000</td></tr><tr><td>R32_AFIO_EXTCR2</td><td>0x4001000C</td><td>外部中断配置寄存器2</td><td>0x00000000</td></tr><tr><td>R32_AFIO_EXTCR3</td><td>0x40010010</td><td>外部中断配置寄存器3</td><td>0x00000000</td></tr><tr><td>R32_AFIO_EXTCR4</td><td>0x40010014</td><td>外部中断配置寄存器4</td><td>0x00000000</td></tr><tr><td>R32_AFIO_PCFR2</td><td>0x4001001C</td><td>重映射寄存器2</td><td>0x00000000</td></tr></table>

#### 10.3.2.1 事件控制寄存器（AFIO_ECR）

偏移地址： $0 \times 0 0$

<table><tr><td>31</td><td>30</td><td>29</td><td>28</td><td>27</td><td>26</td><td>25</td><td>24</td><td>23</td><td>22</td><td>21</td><td>20</td><td>19</td><td>18</td><td>17</td><td>16</td></tr><tr><td colspan="16">Reserved</td></tr><tr><td>15</td><td>14</td><td>13</td><td>12</td><td>11</td><td>10</td><td>9</td><td>8</td><td>7</td><td>6</td><td>5</td><td>4</td><td>3</td><td>2</td><td>1</td><td>0</td></tr><tr><td colspan="8">Reserved</td><td>EVOE</td><td colspan="3">PORT[2:0]</td><td colspan="4">PIN[3:0]</td></tr></table>

<table><tr><td>位</td><td>名称</td><td>访问</td><td>描述</td><td>复位值</td></tr><tr><td>[31:8]</td><td>Reserved</td><td>R0</td><td>保留。</td><td>0</td></tr><tr><td>7</td><td>EVOE</td><td>RW</td><td>允许事件输出位,对该位置位会使内核的EVENTOUT连接到PORT和PIN选定的I0口。</td><td>0</td></tr><tr><td>[6:4]</td><td>PORT[2:0]</td><td>RW</td><td>用于选择内核输出EVENTOUT的端口:000:选择PA口; 001:选择PB口;010:选择PC口; 011:选择PD口;其他:保留。</td><td>000b</td></tr><tr><td>[3:0]</td><td>PIN[3:0]</td><td>RW</td><td>此位的值用来确定选择内核输出EVENTOUT到端口的具体引脚号,值0-15分别对应PORT中选定的Px的第0-15号引脚。</td><td>0</td></tr></table>

#### 10.3.2.2 重映射寄存器 1（AFIO_PCFR1）

偏移地址： $0 \times 0 4$

<table><tr><td>31</td><td>30</td><td>29</td><td>28</td><td>27</td><td>26</td><td>25</td><td>24</td><td>23</td><td>22</td><td>21</td><td>20</td><td>19</td><td>18</td><td>17</td><td>16</td></tr><tr><td>Reserved</td><td>PTP_PPS_RM</td><td>TIM2_ITR1_RM</td><td>SPI3_RM</td><td>Reserved</td><td colspan="3">SW_CFG[2:0]</td><td>MII_RMII_SEL</td><td>CAN2_RM</td><td>ETHRM</td><td>ADC2_ETRGREG_RM</td><td>ADC2_ETRGRIGNJ_RM</td><td>ADC1_ETRGREGRM</td><td>ADC1_ETRGRIGNJ</td><td>TIM5C H4_RM</td></tr><tr><td>15</td><td>14</td><td>13</td><td>12</td><td>11</td><td>10</td><td>9</td><td>8</td><td>7</td><td>6</td><td>5</td><td>4</td><td>3</td><td>2</td><td>1</td><td>0</td></tr><tr><td>PDO1_RM</td><td colspan="2">CAN_RM[1:0]</td><td>TIM4_RM</td><td colspan="2">TIM3_RM[1:0]</td><td colspan="2">TIM2_RM[1:0]</td><td colspan="2">TIM1_RM[1:0]</td><td colspan="2">USART3_RM[1:0]</td><td>USART2_RM</td><td>USART1_RM</td><td>12C1_RM</td><td>SPI1_RM</td></tr></table>

<table><tr><td>位</td><td>名称</td><td>访问</td><td>描述</td><td>复位值</td></tr><tr><td>31</td><td>Reserved</td><td>R0</td><td>保留。</td><td>0</td></tr><tr><td>30</td><td>PTP_PPS_RM</td><td>RW</td><td>以太网的PTP PPS重映射。0:PTP PPS不输出到PB5引脚;1:PTP PPS输出到PB5引脚。</td><td>0</td></tr><tr><td>29</td><td>TIM2ITR1_RM</td><td>RW</td><td>TIM2内部触发1重映射。0:在内部连接TIM2_ITR1至以太网的PTP输出;1:在内部连接TIM2_ITR1至全速USBOTG的SOF输出。</td><td>0</td></tr><tr><td>28</td><td>SPI3_RM</td><td>RW</td><td>SPI3重映射。0:默认映射(NSS/PA15、SCK/PB3、MISO/PB4、MOSI/PB5);1:重映射(NSS/PA4、SCK/PC10、MISO/PC11、MOSI/PC12)。</td><td>0</td></tr><tr><td>27</td><td>Reserved</td><td>R0</td><td>保留。</td><td>0</td></tr><tr><td>[26:24]</td><td>SW_CFG[2:0]</td><td>WO</td><td>这些位用以配置SW功能和跟踪功能的IO口。SWD(SDI)是访问内核的调试接口。系统复位后总是作为SWD端口。0xx:启用SWD(SDI);100:关闭SWD(SDI),作为GPIO功能;其他:无效。</td><td>000b</td></tr><tr><td>23</td><td>MII_RMII_SEL</td><td>RW</td><td>MII或RMII选择。配置内部的以太网MAC适用外部的MII接口还是RMII接口的收发器(PHY)。0:配置以太网的MAC使用外部MII接口的收发器(PHY);1:配置以太网的MAC使用外部RMII接口的收发器(PHY);</td><td>0</td></tr><tr><td>22</td><td>CAN2_RM</td><td>RW</td><td>CAN2重映射位。0:默认映射(CAN2_RX/PB12,CAN2_TX/PB13);1:重映射(CAN2_RX/PB5,CAN2_TX/PB6)。</td><td>0</td></tr><tr><td>21</td><td>ETH_RM</td><td>RW</td><td>以太网的重映射位。0:默认映射(RX_DV-CRS_DV/PA7,RXD0/PC4,RXD1/PC5,RXD2/PB0,RXD3/PB1);</td><td>0</td></tr><tr><td></td><td></td><td></td><td>1:重映射 ( RX_DV-CRS_DV/PD8, RXD0/PD9, RXD1/PD10, RXD2/PD11, RXD3/PD12)</td><td></td></tr><tr><td>20</td><td>ADC2_ETRGREG_RM</td><td>RW</td><td>ADC2外部触发规则转换的重映射位。0:ADC1外部触发规则转换与EXT111相连;1:ADC1外部触发规则转换与TIM8_TRGO相连;</td><td>0</td></tr><tr><td>19</td><td>ADC2_ETRGINJ_RM</td><td>RW</td><td>ADC2外部触发注入转换的重映射位。0:ADC2外部触发注入转换与EXT115相连;1:ADC2外部触发注入转换与TIM8_CH4相连;</td><td>0</td></tr><tr><td>18</td><td>ADC1_ETRGREG_RM</td><td>RW</td><td>ADC1外部触发规则转换的重映射位。0:ADC1外部触发规则转换与EXT111相连;1:ADC1外部触发规则转换与TIM8_TRGO相连;</td><td>0</td></tr><tr><td>17</td><td>ADC1_ETRGINJ_RM</td><td>RW</td><td>ADC1外部触发注入转换的重映射位。0:ADC1外部触发注入转换与EXT115相连;1:ADC1外部触发注入转换与TIM8_CH4相连;</td><td>0</td></tr><tr><td>16</td><td>TIM5CH4_RM</td><td>RW</td><td>定时器5通道的重映射。0:默认映射,定时器5通道4的重映射;1:重映射,定时器5通道4映射至LSI内部时钟。</td><td>0</td></tr><tr><td>15</td><td>PD01_RM</td><td>RW</td><td>引脚PD0&amp;PD1重映射位,该位可由用户读写。它控制PD0和PD1的GPIO功能是否进行重映射,即PD0&amp;PD1映射到OSC_IN&amp;OSC_OUT。0:引脚作为晶振引脚使用;1:引脚作为GPIO口使用;</td><td>0</td></tr><tr><td>[14:13]</td><td>CAN1_RM[1:0]</td><td>RW</td><td>CAN1复用功能重映射位,这些位可由用户读写。控制CAN_RX和CAN_TX的重映射:00:CAN1_RX映射到PA11, CAN1_TX映射到PA12;10:CAN1_RX映射到PB8, CAN1_TX映射到PB9;01:保留;11:CAN1_RX映射到PD0, CAN1_TX映射到PD1;</td><td>00b</td></tr><tr><td>12</td><td>TIM4_RM</td><td>RW</td><td>定时器4的重映射位,该位可由用户读写。它控制定时器4的通道1至4在GPIO端口的重映射:0:默认映射 (CH1/PB6, CH2/PB7, CH3/PB8, CH4/PB9);1:重映射 (CH1/PD12, CH2/PD13, CH3/PD14, CH4/PD15)。</td><td>0</td></tr><tr><td>[11:10]</td><td>TIM3_RM[1:0]</td><td>RW</td><td>定时器3的重映射位,这些位可由用户读写。它控制定时器3的通道1至4在GPIO端口的重映射:00:默认映射 (CH1/PA6, CH2/PA7, CH3/PBO, CH4/PB1);01:保留;10:部分映射 (CH1/PB4, CH2/PB5, CH3/PBO, CH4/PB1);11:完全映射 (CH1/PC6, CH2/PC7, CH3/PC8,</td><td>00b</td></tr><tr><td></td><td></td><td></td><td>CH4/PC9);注:重映射不影响在PD2上的TIM3_ETR。</td><td></td></tr><tr><td>[9:8]</td><td>TIM2_RM[1:0]</td><td>RW</td><td>定时器2的重映射位。这些位可由用户读写。它控制定时器2的通道1至4和外部触发(ETR)在GPIO端口的映射:00:默认映射(ETR/PA0,CH1/PA0,CH2/PA1,CH3/PA2,CH4/PA3);01:部分映射(ETR/PA15,CH1/PA15,CH2/PB3,CH3/PA2,CH4/PA3);10:部分映射(ETR/PA0,CH1/PA0,CH2/PA1,CH3/PB10,CH4/PB11);11:完全映射(ETR/PA15,CH1/PA15,CH2/PB3,CH3/PB10,CH4/PB11)。</td><td>00b</td></tr><tr><td>[7:6]</td><td>TIM1_RM[1:0]</td><td>RW</td><td>定时器1的重映射位。这些位可由用户读写。它控制定时器1的通道1至4、1N至3N、外部触发(ETR)和刹车输入(BKIN)在GPIO端口的映射:00:默认映射(ETR/PA12,CH1/PA8,CH2/PA9,CH3/PA10,CH4/PA11,BKIN/PB12,CH1N/PB13,CH2N/PB14,CH3N/PB15);01:部分映射(ETR/PA12,CH1/PA8,CH2/PA9,CH3/PA10,CH4/PA11,BKIN/PA6,CH1N/PA7,CH2N/PB0,CH3N/PB1);10:保留;11:完全映射(ETR/PE7,CH1/PE9,CH2/PE11,CH3/PE13,CH4/PE14,BKIN/PE15,CH1N/PE8,CH2N/PE10,CH3N/PE12)。</td><td>00b</td></tr><tr><td>[5:4]</td><td>USART3_RM[1:0]</td><td>RW</td><td>USART3的重映射位,这些位可由用户读写。它控制USART3的CTS、RTS、CK、TX和RX复用功能在GPIO端口的映射:00:默认映射(TX/PB10,RX/PB11,CK/PB12,CTS/PB13,RTS/PB14);01:部分重映射(TX/PC10,RX/PC11,CK/PC12,CTS/PB13,RTS/PB14);10:部分重映射(TX/PA13,RX/PA14,CK/PD10,CTS/PD11,RTS/PD12);11:完全重映射(TX/PD8,RX/PD9,CK/PD10,CTS/PD11,RTS/PD12)。注:CH32V20x_D6、CH32F20x_D6只存在默认映射(00b)。</td><td>00b</td></tr><tr><td>3</td><td>USART2_RM</td><td>RW</td><td>USART2的重映射位。该位可由用户读写。它控制USART2的CTS、RTS、CK、TX和RX复用功能在GPIO端口的映射:0:默认映射(CTS/PA0,RTS/PA1, TX/PA2,RX/PA3,CK/PA4);1:重映射(CTS/PD3,RTS/PD4, TX/PD5,RX/PD6,</td><td>0</td></tr><tr><td></td><td></td><td></td><td>CK/PD7)。注:CH32V20x_D6、CH32F20x_D6只存在默认映射(Ob)。</td><td></td></tr><tr><td>2</td><td>USART1_RM</td><td>RW</td><td>USART1映射配置低位(配合AF10_PCFR2寄存器bit26 USART1_RM1使用)。00:默认映射(CK/PA8, TX/PA9, RX/PA10,CTS/PA11, RTS/PA12);01:重映射(CK/PA8, TX/PB6, RX/PB7,CTS/PA11, RTS/PA12);10:重映射(CK/PA10, TX/PB15, RX/PA8,CTS/PA5, RTS/PA9);11:重映射(CK/PA5, TX/PA6, RX/PA7,CTS/PC4,RTS/PC5)。注:CH32V20x_D6、CH32F20x_D6只存在默认映射(00b)、重映射(01b)。</td><td>0</td></tr><tr><td>1</td><td>I2C1_RM</td><td>RW</td><td>I2C1的重映射。该位可由用户读写。它控制I2C1的SCL和SDA复用功能在GPIO端口的映射:0:默认映射(SCL/PB6,SDA/PB7);1:重映射(SCL/PB8,SDA/PB9)。</td><td>0</td></tr><tr><td>0</td><td>SPI1_RM</td><td>RW</td><td>SPI1的重映射。该位可由用户读写。它控制SPI1的NSS、SCK、MISO和MOSI复用功能在GPIO端口的映射:0:默认映射(NSS/PA4, SCK/PA5, MISO/PA6,MOSI/PA7);1:重映射(NSS/PA15, SCK/PB3, MISO/PB4,MOSI/PB5)。</td><td>0</td></tr></table>

#### 10.3.2.3 外部中断配置寄存器 1（AFIO_EXTICR1）

偏移地址： $0 \times 0 8$

<table><tr><td>31</td><td>30</td><td>29</td><td>28</td><td>27</td><td>26</td><td>25</td><td>24</td><td>23</td><td>22</td><td>21</td><td>20</td><td>19</td><td>18</td><td>17</td><td>16</td></tr><tr><td colspan="16">Reserved</td></tr><tr><td>15</td><td>14</td><td>13</td><td>12</td><td>11</td><td>10</td><td>9</td><td>8</td><td>7</td><td>6</td><td>5</td><td>4</td><td>3</td><td>2</td><td>1</td><td>0</td></tr><tr><td colspan="4">EXT13[3:0]</td><td colspan="4">EXT12[3:0]</td><td colspan="4">EXT11[3:0]</td><td colspan="4">EXT10[3:0]</td></tr></table>

<table><tr><td>位</td><td>名称</td><td>访问</td><td>描述</td><td>复位值</td></tr><tr><td>[31:16]</td><td>Reserved</td><td>R0</td><td>保留。</td><td>0</td></tr><tr><td>[15:12]</td><td rowspan="4">EXT1x[3:0]</td><td rowspan="4">RW</td><td rowspan="4">(x=0-3),外部中断输入引脚配置位。用以决定外部中断引脚映射到哪个端口的引脚上:0000:PA引脚的第x个引脚;0001:PB引脚的第x个引脚;0010:PC引脚的第x个引脚;0011:PD引脚的第x个引脚;0100:PE引脚的第x个引脚;</td><td rowspan="4">0000b</td></tr><tr><td>[11:8]</td></tr><tr><td>[7:4]</td></tr><tr><td>[3:0]</td></tr><tr><td></td><td></td><td></td><td>其他:保留。</td><td></td></tr></table>

#### 10.3.2.4 外部中断配置寄存器 2（AFIO_EXTICR2）

偏移地址： $0 \times 0 0$

<table><tr><td>31</td><td>30</td><td>29</td><td>28</td><td>27</td><td>26</td><td>25</td><td>24</td><td>23</td><td>22</td><td>21</td><td>20</td><td>19</td><td>18</td><td>17</td><td>16</td></tr><tr><td colspan="16">Reserved</td></tr><tr><td>15</td><td>14</td><td>13</td><td>12</td><td>11</td><td>10</td><td>9</td><td>8</td><td>7</td><td>6</td><td>5</td><td>4</td><td>3</td><td>2</td><td>1</td><td>0</td></tr><tr><td colspan="4">EXT17[3:0]</td><td colspan="4">EXT16[3:0]</td><td colspan="4">EXT15[3:0]</td><td colspan="4">EXT14[3:0]</td></tr></table>

<table><tr><td>位</td><td>名称</td><td>访问</td><td>描述</td><td>复位值</td></tr><tr><td>[31:16]</td><td>Reserved</td><td>R0</td><td>保留。</td><td>0</td></tr><tr><td>[15:12]</td><td rowspan="4">EXT1x[3:0]</td><td rowspan="4">RW</td><td rowspan="4">(x=4-7),外部中断输入引脚配置位。用以决定外部中断引脚映射到哪个端口的引脚上:0000:PA引脚的第x个引脚;0001:PB引脚的第x个引脚;0010:PC引脚的第x个引脚;0011:PD引脚的第x个引脚;0100:PE引脚的第x个引脚;其他:保留。</td><td rowspan="4">0000b</td></tr><tr><td>[11:8]</td></tr><tr><td>[7:4]</td></tr><tr><td>[3:0]</td></tr></table>

#### 10.3.2.5 外部中断配置寄存器 3（AFIO_EXTICR3）

偏移地址： $0 \times 1 0$

<table><tr><td>31</td><td>30</td><td>29</td><td>28</td><td>27</td><td>26</td><td>25</td><td>24</td><td>23</td><td>22</td><td>21</td><td>20</td><td>19</td><td>18</td><td>17</td><td>16</td></tr><tr><td colspan="16">Reserved</td></tr><tr><td>15</td><td>14</td><td>13</td><td>12</td><td>11</td><td>10</td><td>9</td><td>8</td><td>7</td><td>6</td><td>5</td><td>4</td><td>3</td><td>2</td><td>1</td><td>0</td></tr><tr><td colspan="4">EXT111[3:0]</td><td colspan="4">EXT110[3:0]</td><td colspan="4">EXT19[3:0]</td><td colspan="4">EXT18[3:0]</td></tr></table>

<table><tr><td>位</td><td>名称</td><td>访问</td><td>描述</td><td>复位值</td></tr><tr><td>[31:16]</td><td>Reserved</td><td>R0</td><td>保留。</td><td>0</td></tr><tr><td>[15:12]</td><td rowspan="4">EXT1x[3:0]</td><td rowspan="4">RW</td><td rowspan="4">(x=8-11)，外部中断输入引脚配置位。用以决定外部中断引脚映射到哪个端口的引脚上:0000:PA引脚的第x个引脚;0001:PB引脚的第x个引脚;0010:PC引脚的第x个引脚;0011:PD引脚的第x个引脚;0100:PE引脚的第x个引脚;其他:保留。</td><td rowspan="4">0000b</td></tr><tr><td>[11:8]</td></tr><tr><td>[7:4]</td></tr><tr><td>[3:0]</td></tr></table>

#### 10.3.2.6 外部中断配置寄存器 4（AFIO_EXTICR4）

偏移地址：0x14

<table><tr><td>31</td><td>30</td><td>29</td><td>28</td><td>27</td><td>26</td><td>25</td><td>24</td><td>23</td><td>22</td><td>21</td><td>20</td><td>19</td><td>18</td><td>17</td><td>16</td></tr><tr><td colspan="16">Reserved</td></tr><tr><td colspan="16">15 14 13 12 11 10 9 8 7 6 5 4 3 2 1 0</td></tr><tr><td>EXT115[3:0]</td><td>EXT114[3:0]</td><td>EXT113[3:0]</td><td colspan="13">EXT112[3:0]</td></tr></table>

<table><tr><td>位</td><td>名称</td><td>访问</td><td>描述</td><td>复位值</td></tr><tr><td>[31:16]</td><td>Reserved</td><td>R0</td><td>保留。</td><td>0</td></tr><tr><td>[15:12]</td><td rowspan="4">EXT1x[3:0]</td><td rowspan="4">RW</td><td rowspan="4">(x=12-15),外部中断输入引脚配置位。用以决定外部中断引脚映射到哪个端口的引脚上:0000:PA引脚的第x个引脚;0001:PB引脚的第x个引脚;0010:PC引脚的第x个引脚;0011:PD引脚的第x个引脚;0100:PE引脚的第x个引脚;其他:保留。</td><td rowspan="4">0000b</td></tr><tr><td>[11:8]</td></tr><tr><td>[7:4]</td></tr><tr><td>[3:0]</td></tr></table>

#### 10.3.2.7 重映射寄存器 2（AFIO_PCFR2）

偏移地址：0x1C

<table><tr><td>31</td><td>30</td><td>29</td><td>28</td><td>27</td><td>26</td><td>25</td><td>24</td><td>23</td><td>22</td><td>21</td><td>20</td><td>19</td><td>18</td><td>17</td><td>16</td></tr><tr><td colspan="4">Reserved</td><td>USART1_RM1</td><td colspan="2">UART8_RM[1:0]</td><td colspan="2">UART7_RM[1:0]</td><td colspan="2">UART6_RM[1:0]</td><td colspan="2">UART5_RM[1:0]</td><td colspan="2">UART4_RM[1:0]</td><td></td></tr><tr><td>15</td><td>14</td><td>13</td><td>12</td><td>11</td><td>10</td><td>9</td><td>8</td><td>7</td><td>6</td><td>5</td><td>4</td><td>3</td><td>2</td><td>1</td><td>0</td></tr><tr><td colspan="4">Reserved</td><td>FSMC_NADV</td><td colspan="3">Reserved</td><td colspan="2">TIM10_RM[1:0]</td><td colspan="2">TIM9_RM[1:0]</td><td colspan="2">TIM8_RM</td><td colspan="2">Reserved</td></tr></table>

<table><tr><td>位</td><td>名称</td><td>访问</td><td>描述</td><td>复位值</td></tr><tr><td>[31:27]</td><td>Reserved</td><td>R0</td><td>保留。</td><td>0</td></tr><tr><td>26</td><td>USART1_RM1</td><td>RW</td><td>USART1 映射配置高位（配合 AF10_PCFR1 寄存器 bit2 USART1_RM 使用）。00：默认映射(CK/PA8, TX/PA9, RX/PA10,CTS/PA11, RTS/PA12)；01：重映射(CK/PA8, TX/PB6, RX/PB7,CTS/PA11, RTS/PA12)；10：重映射(CK/PA10, TX/PB15, RX/PA8,CTS/PA5, RTS/PA9)；11：重映射(CK/PA5, TX/PA6, RX/PA7,CTS/PC4,RTS/PC5)。注：CH32V20x_D6、CH32F20x_D6 只存在默认映射(00b)、重映射(01b)。</td><td>0</td></tr><tr><td>[25:24]</td><td>UART8_RM[1:0]</td><td>RW</td><td>UART8 重映射。00：默认映射(TX/PC4, RX/PC5)；01：重映射(TX/PA14, RX/PA15)；1x：重映射(TX/PE14, RX/PE15)。</td><td>00b</td></tr><tr><td>[23:22]</td><td>UART7_RM[1:0]</td><td>RW</td><td>UART7 重映射。00：默认映射(TX/PC2, RX/PC3)；01：重映射(TX/PA6, RX/PA7)；</td><td>00b</td></tr><tr><td></td><td></td><td></td><td>1x:重映射(TX/PE12,RX/PE13)。</td><td></td></tr><tr><td>[21:20]</td><td>UART6_RM[1:0]</td><td>RW</td><td>UART6重映射。00:默认映射(TX/PC0,RX/PC1);01:重映射(TX/PB8,RX/PB9);1x:重映射(TX/PE10,RX/PE11)。</td><td>00b</td></tr><tr><td>[19:18]</td><td>UART5_RM[1:0]</td><td>RW</td><td>UART5重映射。00:默认映射(TX/PC12,RX/PD2);01:重映射(TX/PB4,RX/PB5);1x:重映射(TX/PE8,RX/PE9)。</td><td>00b</td></tr><tr><td>[17:16]</td><td>UART4_RM[1:0]</td><td>RW</td><td>UART4重映射。00:默认映射(TX/PC10,RX/PC11);01:重映射(TX/PB0,RX/PB1);1x:重映射(TX/PE0,RX/PE1)。注:适用于CH32V30x_D8C、CH32V30x_D8、CH32V30x_D8W、CH32V20x_D8、CH32F20x_D8C、CH32F20x_D8、CH32F20x_D8W。x0:默认映射(CK/PB2,TX/PB0,RX/PB1,CTS/PB3,RTS/PB4);x1:重映射(CK/PA6,TX/PA5,RX/PB5,CTS/PA7,RTS/PA15)。注:适用于CH32V20x_D6、CH32F20x_D6。</td><td>00b</td></tr><tr><td>[15:11]</td><td>Reserved</td><td>RW</td><td>保留。</td><td>0</td></tr><tr><td>10</td><td>FSMC_NADV</td><td>RW</td><td>FSMC_NADV重映射。0:FSMC NADV映射到PB7;1:禁止FSMC NADV输出。</td><td>0</td></tr><tr><td>[9:7]</td><td>Reserved</td><td>RO</td><td>保留。</td><td>0</td></tr><tr><td>[6:5]</td><td>TIM10_RM[1:0]</td><td>RW</td><td>TIM10的重映射位。00:默认映射(ETR/PC10,CH1/PB8,CH2/PB9,CH3/PC3,CH4/PC11,BKIN/PC12,CH1N/PA12,CH2N/PA13,CH3N/PA14);01:部分映射(ETR/PB11,CH1/PB3,CH2/PB4,CH3/PB5,CH4/PC15,BKIN/PB10,CH1N/PA5,CH2N/PA6,CH3N/PA7);1x:完全映射(ETR/PD0,CH1/PD1,CH2/PD3,CH3/PD5,CH4/PD7,BKIN/PE2,CH1N/PE3,CH2N/PE4,CH3N/PE5)。</td><td>00b</td></tr><tr><td>[4:3]</td><td>TIM9_RM[1:0]</td><td>RW</td><td>TIM9的重映射位。00:默认映射(ETR/PA2,CH1/PA2,CH2/PA3,CH3/PA4,CH4/PC4,BKIN/PC5,CH1N/PC0,CH2N/PC1,CH3N/PC2);01:部分映射(ETR/PA2,CH1/PA2,CH2/PA3,CH3/PA4,CH4/PC14,BKIN/PA1,CH1N/PB0,CH2N/PB1,CH3N/PB2);1x:完全映射(ETR/PD9,CH1/PD9,CH2/PD11,CH3/PD13,CH4/PD15,BKIN/PD14,CH1N/PD8,CH2N/PD10,CH3N/PD12)。</td><td>00b</td></tr><tr><td>2</td><td>TIM8_RM</td><td>RW</td><td>TIM8的重映射位。
0: 默认映射(ETR/PA0, CH1/PC6, CH2/PC7, CH3/PC8, CH4/PC9, BKIN/PA6, CH1N/PA7, CH2N/PB0, CH3N/PB1);
1: 重映射(ETR/PA0, CH1/PB6, CH2/PB7, CH3/PB8, CH4/PC13, BKIN/PB9, CH1N/PA13, CH2N/PA14, CH3N/PA15);</td><td>0</td></tr><tr><td>[1:0]</td><td>Reserved</td><td>RW</td><td>保留。</td><td>0</td></tr></table>

# 第 11 章 直接存储器访问控制（DMA）

本章模块描述适用于CH32F2x、CH32V2x和CH32V3x微控制器全系列产品。

直接存储器访问控制器（DMA）提供在外设和存储器之间或存储器和存储器之间的高速数据传输方式，无须 CPU 干预，数据可以通过 DMA 快速地移动，以节省 CPU 的资源来做其他操作。

DMA 控制器每个通道专门用来管理来自于一个或多个外设对存储器访问的请求。还有一个仲裁器来协调各通道之间的优先级。

## 11.1 主要特性

多个独立可配置通道  
每个通道都直接连接专用的硬件 DMA 请求，并支持软件触发  
$\bullet$ 支持循环的缓冲器管理  
多个通道之间的请求优先权可以通过软件编程设置(最高、高、中和低)，优先权设置相等时由通道号决定（通道号越低优先级越高）  
支持外设到存储器、存储器到外设、存储器到存储器之间的传输  
$\bullet$ 闪存、SRAM、外设的 SRAM、APB1、APB2 和 AHB 外设均可作为访问的源和目标  
可编程的数据传输字节数目：最大为 65535

## 11.2 功能描述

### 11.2.1 DMA 通道处理

#### 1）仲裁优先级

多个独立的通道产生的DMA请求通过逻辑或结构输入到 DMA控制器，当前只会有一个通道的请求得到响应。模块内部的仲裁器根据通道请求的优先级来选择要启动的外设/存储器的访问。

软件管理中，应用程序通过对DMA_CFGRx寄存器的PL[1:0]位设置，可以为每个通道独立配置优先等级，包括最高、高、中、低4个等级。当通道间的软件设置等级一致时，模块会按固定的硬件优先级选择，通道编号偏低的要比偏高的有较高优先权。

#### 2）DMA 配置

当DMA控制器收到一个请求信号时，会访问发出请求的外设或存储器，建立外设或存储器和存储器之间的数据传输。主要包括下面3个操作步骤：

1） 从外设数据寄存器或当前外设/存储器地址寄存器指示的存储器地址取数据，第一次传输时的开始地址是 DMA_PADDRx 或 DMA_MADDRx 寄存器指定的外设基地址或存储器地址。  
2）存数据到外设数据寄存器或当前外设/存储器地址寄存器指示的存储器地址，第一次传输时的开始地址是DMA_PADDRx或DMA_MADDRx寄存器指定的外设基地址或存储器地址。  
3）执行一次DMA_CNTRx寄存器中数值的递减操作，该寄存器指示当前未完成转移的操作数目。

每个通道包括3种DMA数据转移方式：

外设到存储器（MEM2MEM=0，DIR=0）  
$\bullet$ 存储器到外设（MEM2MEM=0， $\mathsf { D } | \mathsf { R } { = } 1$ ）  
$\bullet$ 存储器到存储器（MEM2MEM=1）

注：存储器到存储器方式无需外设请求信号，配置为此模式后（MEM2MEM=1），通道开启（EN=1）即可启动数据传输。此方式不支持循环模式。

配置过程如下：

1）在 DMA_PADDRx 寄存器中设置外设寄存器的首地址或存储器到存储器方式（MEM2MEM=1）下存储器数据地址。发生DMA请求时，这个地址将是数据传输的源或目标地址。  
2） 在 DMA_MADDRx 寄存器中设置存储器数据地址。发生 DMA 请求时，传输的数据将从这个地址读出或写入这个地址。  
3） 在 DMA_CNTRx 寄存器中设置要传输的数据数量。在每个数据传输后，这个数值递减。  
4） 在 DMA_CFGRx 寄存器的 PL[1:0]位中设置通道的优先级。  
5）在 DMA_CFGRx寄存器中设置数据传输的方向、循环模式、外设和存储器的增量模式、外设和存储器的数据宽度、传输过半、传输完成、传输错误中断使能位，  
6） 设置 DMA_CCRx 寄存器的 ENABLE 位，启动通道 x。

注：DMA_PADDRx/DMA_MADDRx/DMA_CNTRx寄存器以及DMA_CFGRx寄存器中的数据传输的方向（DIR）、循环模式（位置）、外设和存储器的增量模式（MINC/PINC）等控制位只有在DMA 通道被关闭下才可以配置写入。

#### 3）循环模式

设置DMA_CFGRx寄存器的CIRC位置1，可以启用通道数据传输的循环模式功能。循环模式下，当数据传输的数目变为0时，DMA_CNTRx寄存器的内容会自动被重新加载为其初始数值，内部的外设和存储器地址寄存器也被重新加载为 DMA_PADDRx 和 DMA_MADDRx 寄存器设定的初始地址值，DMA 操作将继续进行，直到通道被关闭或关闭DMA模式。

#### 4）DMA 处理状态

l 传输过半：对应DMA_INTFR寄存器中的 HTIFx位硬件置位。当DMA的传输字节数目减至初始设定值一半以下将会产生DMA传输过半标志，如果在 DMA_CCRx寄存器中置位了 HTIE，则将产生中断。硬件通过此标志提醒应用程序，可以为新一轮数据传输做准备。  
传输完成：对应DMA_INTFR寄存器中的 ${ \mathsf { T C l } } \mathsf { F x }$ 位硬件置位。当DMA的传输字节数目减至 0 将会产生 DMA 传输完成标志，如果在 DMA_CCRx 寄存器中置位了 TCIE，则将产生中断。  
l 传输错误：对应DMA_INTFR寄存器中的 TEIFx位硬件置位。读写一个保留的地址区域，将会产生DMA传输错误。同时模块硬件会自动清0发生错误的通道所对应的 DMA_CCRx寄存器的 EN位，该通道被关闭。如果在DMA_CCRx寄存器中置位了 TEIE，则将产生中断。

应用程序在查询 DMA 通道状态时，可以先访问 DMA_INTFR 寄存器的 GIFx 位，判断出当前哪个通道发生了 DMA 事件，进而处理该通道的具体 DAM 事件内容。

### 11.2.2 可编程的数据传输总大小/数据位宽/对齐方式

DMA 每个通道一轮传输的数据量总大小可编程，最大 65535 次。DMA_CNTRx 寄存器中指示待传输字节数目。在 EN=0 时，写入设置值，在 EN=1 开启 DMA 传输通道后，此寄存器变为只读属性，在每次传输后数值递减。

外设和存储器的传输数据取值支持地址指针自动递增功能，指针增量可编程。它们访问的第一个传输的数据地址存放在 DMA_PADDRx 和 DMA_MADDRx 寄存器中，通过设置 DMA_CFGRx 寄存器的 PINC 位或 MINC 位置 1，可以分别开启外设地址自增模式或存储器地址自增模式，PSIZE[1:0]设置外设地址取数据大小及地址自增大小，MSIZE[1:0]设置存储器地址取数据大小及地址自增大小，包括 3 种选择：8 位、16 位、32 位。具体数据转移方式如下表：

表 11-1 不同数据位宽下 DMA 转移（PINC=MINC=1）  

<table><tr><td>源端位宽</td><td>目标位宽</td><td>传输数目</td><td>源:地址/数据</td><td>目标:地址/数据</td><td>传输操作</td></tr><tr><td rowspan="3">8</td><td rowspan="3">8</td><td rowspan="3">4</td><td>0x00/B0</td><td>0x00/B0</td><td rowspan="3">●源端地址递增量与源端设置的数据位宽对齐,取值大小等于源端数据位宽</td></tr><tr><td>0x01/B1</td><td>0x01/B1</td></tr><tr><td>0x02/B2</td><td>0x02/B2</td></tr><tr><td></td><td></td><td></td><td>0x03/B3</td><td>0x03/B3</td><td>• 目标地址递增量与目标设置数据的位宽对齐,取值大小等于目标数据位宽DMA转移送入目标端的数据依据原则:数据大小不足高位补0,数据大小溢出高位去掉• 存储数据方式:小端模式,低地址存放低字节,高地址存放高字节</td></tr><tr><td>8</td><td>16</td><td>4</td><td>0x00/B00x01/B10x02/B20x03/B3</td><td>0x00/00B00x02/00B10x04/00B20x06/00B3</td><td>• 目标地址递增量与目标设置数据的位宽对齐,取值大小等于目标数据位宽DMA转移送入目标端的数据依据原则:数据大小不足高位补0,数据大小溢出高位去掉• 存储数据方式:小端模式,低地址存放低字节,高地址存放高字节</td></tr><tr><td>8</td><td>32</td><td>4</td><td>0x00/B00x01/B10x02/B20x03/B3</td><td>0x00/000000B00x04/000000B10x08/000000B20x0C/000000B3</td><td>• 目标地址递增量与目标设置数据的位宽对齐,取值大小等于目标数据位宽DMA转移送入目标端的数据依据原则:数据大小不足高位补0,数据大小溢出高位去掉• 存储数据方式:小端模式,低地址存放低字节,高地址存放高字节</td></tr><tr><td>16</td><td>8</td><td>4</td><td>0x00/B1B00x02/B3B20x04/B5B40x06/B7B6</td><td>0x00/B00x01/B20x02/B40x03/B6</td><td>• 目标地址递增量与目标设置数据的位宽对齐,取值大小等于目标数据位宽DMA转移送入目标端的数据依据原则:数据大小不足高位补0,数据大小溢出高位去掉</td></tr><tr><td>16</td><td>16</td><td>4</td><td>0x00/B1B00x02/B3B20x04/B5B40x06/B7B6</td><td>0x00/B1B00x02/B3B20x04/B5B40x06/B7B6</td><td>• 目标地址递增量与目标设置数据的位宽对齐,取值大小等于目标数据位宽DMA转移送入目标端的数据依据原则:数据大小不足高位补0,数据大小溢出高位去掉</td></tr><tr><td>16</td><td>32</td><td>4</td><td>0x00/B1B00x02/B3B20x04/B5B40x06/B7B6</td><td>0x00/0000B1B00x04/0000B3B20x08/0000B5B40x0C/0000B7B6</td><td>• 目标地址递增量与目标设置数据的位宽对齐,取值大小等于目标数据位宽DMA转移送入目标端的数据依据原则:数据大小不足高位补0,数据大小溢出高位去掉</td></tr><tr><td>32</td><td>8</td><td>4</td><td>0x00/B3B2B1B00x04/B7B6B5B40x08/BBBAB9B80x0C/BFBEBDBC</td><td>0x00/B00x01/B40x02/B80x03/BC</td><td>• 目标地址递增量与目标设置数据的位宽对齐,取值大小等于目标数据位宽DMA转移送入目标端的数据依据原则:数据大小不足高位补0,数据大小溢出高位去掉</td></tr><tr><td>32</td><td>16</td><td>4</td><td>0x00/B3B2B1B00x04/B7B6B5B40x08/BBBAB9B80x0C/BFBEBDBC</td><td>0x00/B1B00x02/B5B40x04/B9B80x06/BDBC</td><td>• 目标地址递增量与目标设置数据的位宽对齐,取值大小等于目标数据位宽DMA转移送入目标端的数据依据原则:数据大小不足高位补0,数据大小溢出高位去掉</td></tr><tr><td>32</td><td>32</td><td>4</td><td>0x00/B3B2B1B00x04/B7B6B5B40x08/BBBAB9B80x0C/BFBEBDBC</td><td>0x00/B3B2B1B00x04/B7B6B5B40x08/BBBAB9B80x0C/BFBEBDBC</td><td>• 目标地址递增量与目标设置数据的位宽对齐,取值大小等于目标数据位宽DMA转移送入目标端的数据依据原则:数据大小不足高位补0,数据大小溢出高位去掉</td></tr></table>

### 11.2.3 DMA 请求映射

青稞 V4F MCU（包括 CH32V30x_D8C、CH32V30x_D8）以及 ARM○R CortexTM-M3 MCU(包括 CH32F20x_D8C、CH32F20x_D8），DMA控制器提供18个通道，其中 DMA1 包含 7个通道，DMA2包含 11个通道，每个通道对应多个外设请求，通过设置相应外设寄存器中对应 DMA 控制位，可以独立的开启或关闭各个外设的 DMA 功能，具体对应关系如下。

![](images/0159f50eec506a61590c13962a071543f9d44370d3d980e7cd6b8f6f0562b7f8.jpg)  
图 11-1 DMA1 请求映像

![](images/258d2d7306c403ed2f3c9710eeeaaada0fcfce1eecc2b9979e40a907526c49ee.jpg)  
图 11-2 DMA2 请求映像

表 11-2 DMA1 各通道外设映射表  

<table><tr><td>外设</td><td>通道1</td><td>通道2</td><td>通道3</td><td>通道4</td><td>通道5</td><td>通道6</td><td>通道7</td></tr><tr><td>ADC1</td><td>ADC1</td><td></td><td></td><td></td><td></td><td></td><td></td></tr><tr><td>SPI1</td><td></td><td>SPI1_RX</td><td>SPI1_TX</td><td></td><td></td><td></td><td></td></tr><tr><td>SPI/12S2</td><td></td><td></td><td></td><td>SPI/12S2_RX</td><td>SPI/12S2_TX</td><td></td><td></td></tr><tr><td>USART1</td><td></td><td></td><td></td><td>USART1_TX</td><td>USART1_RX</td><td></td><td></td></tr><tr><td>USART2</td><td></td><td></td><td></td><td></td><td></td><td>USART2_RX</td><td>USART2_TX</td></tr><tr><td>USART3</td><td></td><td>USART3_TX</td><td>USART3_RX</td><td></td><td></td><td></td><td></td></tr><tr><td>I2C1</td><td></td><td></td><td></td><td></td><td></td><td>I2C1_TX</td><td>I2C1_RX</td></tr><tr><td>I2C2</td><td></td><td></td><td></td><td>I2C2_TX</td><td>I2C2_RX</td><td></td><td></td></tr><tr><td>TIM1</td><td></td><td>TIM1_CH1</td><td>TIM1_CH2</td><td>TIM1_CH4
TIM1_TRIG
TIM1_COM</td><td>TIM1_UP</td><td>TIM1_CH3</td><td></td></tr><tr><td>TIM2</td><td>TIM2_CH3</td><td>TIM2_UP</td><td></td><td></td><td>TIM2_CH1</td><td></td><td>TIM2_CH2
TIM2_CH4</td></tr><tr><td>TIM3</td><td></td><td>TIM3_CH3</td><td>TIM3_CH4
TIM3_UP</td><td></td><td></td><td>TIM3_CH1
TIM3_TRIG</td><td></td></tr><tr><td>TIM4</td><td>TIM4_CH1</td><td></td><td></td><td>TIM4_CH2</td><td>TIM4_CH3</td><td></td><td>TIM4_UP</td></tr></table>

表11-3 DMA2各通道外设映射表一  

<table><tr><td>外设</td><td>通道1</td><td>通道2</td><td>通道3</td><td>通道4</td><td>通道5</td><td>通道6</td><td>通道7</td></tr><tr><td rowspan="2">TIM5</td><td>TIM5_CH4</td><td>TIM5_CH3</td><td></td><td>TIM5_CH2</td><td>TIM5_CH1</td><td></td><td></td></tr><tr><td>TIM5_TRIG</td><td>TIM5_UP</td><td></td><td></td><td></td><td></td><td></td></tr><tr><td>TIM6</td><td></td><td></td><td>TIM6_UP</td><td></td><td></td><td></td><td></td></tr><tr><td>TIM7</td><td></td><td></td><td></td><td>TIM7_UP</td><td></td><td></td><td></td></tr><tr><td rowspan="3">TIM8</td><td>TIM8_CH3</td><td>TIM8_CH4</td><td>TIM8_CH1</td><td></td><td>TIM8_CH2</td><td></td><td></td></tr><tr><td>TIM8_UP</td><td>TIM8_TRIG</td><td></td><td></td><td></td><td></td><td></td></tr><tr><td>TIM8_COM</td><td>TIM8_COM</td><td></td><td></td><td></td><td></td><td></td></tr><tr><td>TIM9</td><td></td><td></td><td></td><td></td><td></td><td>TIM9_UP</td><td>TIM9_CH1</td></tr><tr><td rowspan="2">TIM10</td><td></td><td></td><td></td><td></td><td></td><td>TIM10_CH4</td><td>TIM10_TRIG</td></tr><tr><td></td><td></td><td></td><td></td><td></td><td></td><td>TIM10_COM</td></tr><tr><td>UART4</td><td></td><td></td><td>USART4_RX</td><td></td><td>UART4_TX</td><td></td><td></td></tr><tr><td>UART5</td><td></td><td>USART5_RX</td><td></td><td>UART5_TX</td><td></td><td></td><td></td></tr><tr><td>UART6</td><td></td><td></td><td></td><td></td><td></td><td>UART6_TX</td><td>UART6_RX</td></tr><tr><td>UART7</td><td></td><td></td><td></td><td></td><td></td><td></td><td></td></tr><tr><td>UART8</td><td></td><td></td><td></td><td></td><td></td><td></td><td></td></tr><tr><td>SPI/12S3</td><td>SPI/12S3_RX</td><td>SPI/12S3_TX</td><td></td><td></td><td></td><td></td><td></td></tr><tr><td>SDIO</td><td></td><td></td><td></td><td>SDIO</td><td></td><td></td><td></td></tr><tr><td>DAC1</td><td></td><td></td><td>DAC1</td><td></td><td></td><td></td><td></td></tr><tr><td>DAC2</td><td></td><td></td><td></td><td>DAC2</td><td></td><td></td><td></td></tr></table>

表11-4 DMA2各通道外设映射表二  

<table><tr><td>外设</td><td>通道8</td><td>通道9</td><td>通道10</td><td>通道11</td></tr><tr><td>TIM5</td><td></td><td></td><td></td><td></td></tr><tr><td>TIM6</td><td></td><td></td><td></td><td></td></tr><tr><td>TIM7</td><td></td><td></td><td></td><td></td></tr><tr><td>TIM8</td><td></td><td></td><td></td><td></td></tr><tr><td rowspan="2">TIM9</td><td rowspan="2">TIM9_CH4</td><td rowspan="2">TIM9_CH2</td><td>TIM9_TRIG</td><td rowspan="2">TIM9_CH3</td></tr><tr><td>TIM9_COM</td></tr><tr><td>TIM10</td><td>TIM10_CH1</td><td>TIM10_CH3</td><td>TIM10_CH2</td><td>TIM10_UP</td></tr><tr><td>UART4</td><td></td><td></td><td></td><td></td></tr><tr><td>UART5</td><td></td><td></td><td></td><td></td></tr><tr><td>UART6</td><td></td><td></td><td></td><td></td></tr><tr><td>UART7</td><td>UART7_TX</td><td>UART7_RX</td><td></td><td></td></tr><tr><td>UART8</td><td></td><td></td><td>UART8_TX</td><td>UART8_RX</td></tr><tr><td>SPI/12S3</td><td></td><td></td><td></td><td></td></tr><tr><td>SD10</td><td></td><td></td><td></td><td></td></tr><tr><td>DAC1</td><td></td><td></td><td></td><td></td></tr><tr><td>DAC2</td><td></td><td></td><td></td><td></td></tr></table>

青稞 V4B MCU（包括 $\mathsf { C H } 3 2 \mathsf { V } 2 0 \mathsf { x } \_ \mathsf { D } 6 .$ ）以及 ARM○R CortexTM-M3 MCU(包括 CH32F20x_D6），DMA 控制器提供 8 个通道，每个通道对应多个外设请求，通过设置相应外设寄存器中对应 DMA 控制位，可以独立的开启或关闭各个外设的DMA功能，具体对应关系如下。

<table><tr><td>外设</td><td>通道1</td><td>通道2</td><td>通道3</td><td>通道4</td><td>通道5</td><td>通道6</td><td>通道7</td><td>通道8</td></tr><tr><td>ADC1</td><td>ADC1</td><td></td><td></td><td></td><td></td><td></td><td></td><td></td></tr><tr><td>SPI1</td><td></td><td>SPI1_RX</td><td>SPI1_TX</td><td></td><td></td><td></td><td></td><td></td></tr><tr><td>SPI2</td><td></td><td></td><td></td><td>SPI2_RX</td><td>SPI2_TX</td><td></td><td></td><td></td></tr><tr><td>USART1</td><td></td><td></td><td></td><td>USART1_TX</td><td>USART1_RX</td><td></td><td></td><td></td></tr><tr><td>USART2</td><td></td><td></td><td></td><td></td><td></td><td>USART2_RX</td><td>USART2_TX</td><td></td></tr><tr><td>USART3</td><td></td><td>USART3_TX</td><td>USART3_RX</td><td></td><td></td><td></td><td></td><td></td></tr><tr><td>USART4</td><td>USART4_TX</td><td></td><td></td><td></td><td></td><td></td><td></td><td>USART4_RX</td></tr><tr><td>I2C1</td><td></td><td></td><td></td><td></td><td></td><td>I2C1_TX</td><td>I2C1_RX</td><td></td></tr><tr><td>I2C2</td><td></td><td></td><td></td><td>I2C2_TX</td><td>I2C2_RX</td><td></td><td></td><td></td></tr><tr><td>TIM1</td><td></td><td>TIM1_CH1</td><td>TIM1_CH2</td><td>TIM1_CH4TIM1_TRIGTIM1_COM</td><td>TIM1_UP</td><td>TIM1_CH3</td><td></td><td></td></tr><tr><td>TIM2</td><td>TIM2_CH3</td><td>TIM2_UP</td><td></td><td></td><td>TIM2_CH1</td><td></td><td>TIM2_CH2TIM2_CH4</td><td></td></tr><tr><td>TIM3</td><td></td><td>TIM3_CH3</td><td>TIM3_CH4TIM3_UP</td><td></td><td></td><td>TIM3_CH1TIM3_TRIG</td><td></td><td></td></tr><tr><td>TIM4</td><td>TIM4_CH1</td><td></td><td></td><td>TIM4_CH2</td><td>TIM4_CH3</td><td></td><td>TIM4_UP</td><td></td></tr></table>

青稞 V4C MCU（包括 CH32V20x_D8W、CH32V20x_D8）以及 ARM○R CortexTM-M3 MCU(包括 CH32F20x_D8W），DMA控制器提供8个通道，每个通道对应多个外设请求，通过设置相应外设寄存器中对应DMA控制位，可以独立的开启或关闭各个外设的 DMA 功能，具体对应关系如下。

<table><tr><td>外设</td><td>通道1</td><td>通道2</td><td>通道3</td><td>通道4</td><td>通道5</td><td>通道6</td><td>通道7</td><td>通道8</td></tr><tr><td>ADC1</td><td>ADC1</td><td></td><td></td><td></td><td></td><td></td><td></td><td></td></tr><tr><td>SPI1</td><td></td><td>SPI1_RX</td><td>SPI1_TX</td><td></td><td></td><td></td><td></td><td></td></tr><tr><td>SPI2</td><td></td><td></td><td></td><td>SPI2_RX</td><td>SPI2_TX</td><td></td><td></td><td></td></tr><tr><td>USART1</td><td></td><td></td><td></td><td>USART1_TX</td><td>USART1_RX</td><td></td><td></td><td></td></tr><tr><td>USART2</td><td></td><td></td><td></td><td></td><td></td><td>USART2_RX</td><td>USART2_TX</td><td></td></tr><tr><td>USART3</td><td></td><td>USART3_TX</td><td>USART3_RX</td><td></td><td></td><td></td><td></td><td></td></tr><tr><td>USART4</td><td>USART4_TX</td><td></td><td></td><td></td><td></td><td></td><td></td><td>USART4_RX</td></tr><tr><td>I2C1</td><td></td><td></td><td></td><td></td><td></td><td>I2C1_TX</td><td>I2C1_RX</td><td></td></tr><tr><td>I2C2</td><td></td><td></td><td></td><td>I2C2_TX</td><td>I2C2_RX</td><td></td><td></td><td></td></tr><tr><td rowspan="2">TIM1</td><td rowspan="2"></td><td rowspan="2">TIM1_CH1</td><td rowspan="2">TIM1_CH2</td><td>TIM1_CH4</td><td rowspan="2">TIM1_TRIG</td><td rowspan="2">TIM1_UP</td><td rowspan="2">TIM1_CH3</td><td rowspan="2"></td></tr><tr><td>TIM1_COM</td></tr><tr><td rowspan="2">TIM2</td><td rowspan="2">TIM2_CH3</td><td rowspan="2">TIM2_UP</td><td rowspan="2"></td><td rowspan="2"></td><td rowspan="2">TIM2_CH1</td><td rowspan="2"></td><td>TIM2_CH2</td><td rowspan="2"></td></tr><tr><td>TIM2_CH4</td></tr><tr><td>TIM3</td><td></td><td>TIM3_CH3</td><td>TIM3_CH4TIM3_UP</td><td></td><td></td><td>TIM3_CH1TIM3_TRIG</td><td></td><td></td></tr><tr><td>TIM4</td><td>TIM4_CH1</td><td></td><td></td><td>TIM4_CH2</td><td>TIM4_CH3</td><td></td><td>TIM4_UP</td><td></td></tr><tr><td>TIM5</td><td>TIM5_CH2</td><td></td><td>TIM5_CH3</td><td></td><td></td><td>TIM5_CH4</td><td>TIM5_CH1TIM5_TRIG</td><td>TIM5_UP</td></tr></table>

## 11.3 寄存器描述

表 11-5 DMA1 相关寄存器列表  

<table><tr><td>名称</td><td>访问地址</td><td>描述</td><td>复位值</td></tr><tr><td>R32_DMA1_INTFR</td><td>0x40020000</td><td>DMA1中断状态寄存器</td><td>0x00000000</td></tr><tr><td>R32_DMA1_INTFCR</td><td>0x40020004</td><td>DMA1中断标志清除寄存器</td><td>0x00000000</td></tr><tr><td>R32_DMA1_CFGR1</td><td>0x40020008</td><td>DMA1通道1配置寄存器</td><td>0x00000000</td></tr><tr><td>R32_DMA1_CNTR1</td><td>0x4002000C</td><td>DMA1通道1传输数据数目寄存器</td><td>0x00000000</td></tr><tr><td>R32_DMA1_PADDR1</td><td>0x40020010</td><td>DMA1通道1外设地址寄存器</td><td>0x00000000</td></tr><tr><td>R32_DMA1_MADDR1</td><td>0x40020014</td><td>DMA1通道1存储器地址寄存器</td><td>0x00000000</td></tr><tr><td>R32_DMA1_CFGR2</td><td>0x4002001C</td><td>DMA1通道2配置寄存器</td><td>0x00000000</td></tr><tr><td>R32_DMA1_CNTR2</td><td>0x40020020</td><td>DMA1通道2传输数据数目寄存器</td><td>0x00000000</td></tr><tr><td>R32_DMA1_PADDR2</td><td>0x40020024</td><td>DMA1通道2外设地址寄存器</td><td>0x00000000</td></tr><tr><td>R32_DMA1_MADDR2</td><td>0x40020028</td><td>DMA1通道2存储器地址寄存器</td><td>0x00000000</td></tr><tr><td>R32_DMA1_CFGR3</td><td>0x40020030</td><td>DMA1通道3配置寄存器</td><td>0x00000000</td></tr><tr><td>R32_DMA1_CNTR3</td><td>0x40020034</td><td>DMA1通道3传输数据数目寄存器</td><td>0x00000000</td></tr><tr><td>R32_DMA1_PADDR3</td><td>0x40020038</td><td>DMA1通道3外设地址寄存器</td><td>0x00000000</td></tr><tr><td>R32_DMA1_MADDR3</td><td>0x4002003C</td><td>DMA1通道3存储器地址寄存器</td><td>0x00000000</td></tr><tr><td>R32_DMA1_CFGR4</td><td>0x40020044</td><td>DMA1通道4配置寄存器</td><td>0x00000000</td></tr><tr><td>R32_DMA1_CNTR4</td><td>0x40020048</td><td>DMA1通道4传输数据数目寄存器</td><td>0x00000000</td></tr><tr><td>R32_DMA1_PADDR4</td><td>0x4002004C</td><td>DMA1通道4外设地址寄存器</td><td>0x00000000</td></tr><tr><td>R32_DMA1_MADDR4</td><td>0x40020050</td><td>DMA1通道4存储器地址寄存器</td><td>0x00000000</td></tr><tr><td>R32_DMA1_CFGR5</td><td>0x40020058</td><td>DMA1通道5配置寄存器</td><td>0x00000000</td></tr><tr><td>R32_DMA1_CNTR5</td><td>0x4002005C</td><td>DMA1通道5传输数据数目寄存器</td><td>0x00000000</td></tr><tr><td>R32_DMA1_PADDR5</td><td>0x40020060</td><td>DMA1通道5外设地址寄存器</td><td>0x00000000</td></tr><tr><td>R32_DMA1_MADDR5</td><td>0x40020064</td><td>DMA1通道5存储器地址寄存器</td><td>0x00000000</td></tr><tr><td>R32_DMA1_CFGR6</td><td>0x4002006C</td><td>DMA1通道6配置寄存器</td><td>0x00000000</td></tr><tr><td>R32_DMA1_CNTR6</td><td>0x40020070</td><td>DMA1通道6传输数据数目寄存器</td><td>0x00000000</td></tr><tr><td>R32_DMA1_PADDR6</td><td>0x40020074</td><td>DMA1通道6外设地址寄存器</td><td>0x00000000</td></tr><tr><td>R32_DMA1_MADDR6</td><td>0x40020078</td><td>DMA1通道6存储器地址寄存器</td><td>0x00000000</td></tr><tr><td>R32_DMA1_CFGR7</td><td>0x40020080</td><td>DMA1通道7配置寄存器</td><td>0x00000000</td></tr><tr><td>R32_DMA1_CNTR7</td><td>0x40020084</td><td>DMA1通道7传输数据数目寄存器</td><td>0x00000000</td></tr><tr><td>R32_DMA1_PADDR7</td><td>0x40020088</td><td>DMA1通道7外设地址寄存器</td><td>0x00000000</td></tr><tr><td>R32_DMA1_MADDR7</td><td>0x4002008C</td><td>DMA1通道7存储器地址寄存器</td><td>0x00000000</td></tr><tr><td>R32_DMA1_CFGR8</td><td>0x40020094</td><td>DMA1通道8配置寄存器</td><td>0x00000000</td></tr><tr><td>R32_DMA1_CNTR8</td><td>0x40020098</td><td>DMA1通道8传输数据数目寄存器</td><td>0x00000000</td></tr><tr><td>R32_DMA1_PADDR8</td><td>0x4002009C</td><td>DMA1通道8外设地址寄存器</td><td>0x00000000</td></tr><tr><td>R32_DMA1_MADDR8</td><td>0x400200AO</td><td>DMA1通道8存储器地址寄存器</td><td>0x00000000</td></tr></table>

表 11-6 DMA2 相关寄存器列表  

<table><tr><td>名称</td><td>访问地址</td><td>描述</td><td>复位值</td></tr><tr><td>R32_DMA2_INTFR</td><td>0x40020400</td><td>DMA2中断状态寄存器</td><td>0x00000000</td></tr><tr><td>R32_DMA2_INTFCR</td><td>0x40020404</td><td>DMA2中断标志清除寄存器</td><td>0x00000000</td></tr><tr><td>R32_DMA2_CFGR1</td><td>0x40020408</td><td>DMA2通道1配置寄存器</td><td>0x00000000</td></tr><tr><td>R32_DMA2_CNTR1</td><td>0x4002040C</td><td>DMA2通道1传输数据数目寄存器</td><td>0x00000000</td></tr><tr><td>R32_DMA2_PADDR1</td><td>0x40020410</td><td>DMA2通道1外设地址寄存器</td><td>0x00000000</td></tr><tr><td>R32_DMA2_MADDR1</td><td>0x40020414</td><td>DMA2通道1存储器地址寄存器</td><td>0x00000000</td></tr><tr><td>R32_DMA2_CFGR2</td><td>0x4002041C</td><td>DMA2通道2配置寄存器</td><td>0x00000000</td></tr><tr><td>R32_DMA2_CNTR2</td><td>0x40020420</td><td>DMA2通道2传输数据数目寄存器</td><td>0x00000000</td></tr><tr><td>R32_DMA2_PADDR2</td><td>0x40020424</td><td>DMA2通道2外设地址寄存器</td><td>0x00000000</td></tr><tr><td>R32_DMA2_MADDR2</td><td>0x40020428</td><td>DMA2通道2存储器地址寄存器</td><td>0x00000000</td></tr><tr><td>R32_DMA2_CFGR3</td><td>0x40020430</td><td>DMA2通道3配置寄存器</td><td>0x00000000</td></tr><tr><td>R32_DMA2_CNTR3</td><td>0x40020434</td><td>DMA2通道3传输数据数目寄存器</td><td>0x00000000</td></tr><tr><td>R32_DMA2_PADDR3</td><td>0x40020438</td><td>DMA2通道3外设地址寄存器</td><td>0x00000000</td></tr><tr><td>R32_DMA2_MADDR3</td><td>0x4002043C</td><td>DMA2通道3存储器地址寄存器</td><td>0x00000000</td></tr><tr><td>R32_DMA2_CFGR4</td><td>0x40020444</td><td>DMA2通道4配置寄存器</td><td>0x00000000</td></tr><tr><td>R32_DMA2_CNTR4</td><td>0x40020448</td><td>DMA2通道4传输数据数目寄存器</td><td>0x00000000</td></tr><tr><td>R32_DMA2_PADDR4</td><td>0x4002044C</td><td>DMA2通道4外设地址寄存器</td><td>0x00000000</td></tr><tr><td>R32_DMA2_MADDR4</td><td>0x40020450</td><td>DMA2通道4存储器地址寄存器</td><td>0x00000000</td></tr><tr><td>R32_DMA2_CFGR5</td><td>0x40020458</td><td>DMA2通道5配置寄存器</td><td>0x00000000</td></tr><tr><td>R32_DMA2_CNTR5</td><td>0x4002045C</td><td>DMA2通道5传输数据数目寄存器</td><td>0x00000000</td></tr><tr><td>R32_DMA2_PADDR5</td><td>0x40020460</td><td>DMA2通道5外设地址寄存器</td><td>0x00000000</td></tr><tr><td>R32_DMA2_MADDR5</td><td>0x40020464</td><td>DMA2通道5存储器地址寄存器</td><td>0x00000000</td></tr><tr><td>R32_DMA2_CFGR6</td><td>0x4002046C</td><td>DMA2通道6配置寄存器</td><td>0x00000000</td></tr><tr><td>R32_DMA2_CNTR6</td><td>0x40020470</td><td>DMA2通道6传输数据数目寄存器</td><td>0x00000000</td></tr><tr><td>R32_DMA2_PADDR6</td><td>0x40020474</td><td>DMA2通道6外设地址寄存器</td><td>0x00000000</td></tr><tr><td>R32_DMA2_MADDR6</td><td>0x40020478</td><td>DMA2通道6存储器地址寄存器</td><td>0x00000000</td></tr><tr><td>R32_DMA2_CFGR7</td><td>0x40020480</td><td>DMA2通道7配置寄存器</td><td>0x00000000</td></tr><tr><td>R32_DMA2_CNTR7</td><td>0x40020484</td><td>DMA2通道7传输数据数目寄存器</td><td>0x00000000</td></tr><tr><td>R32_DMA2_PADDR7</td><td>0x40020488</td><td>DMA2通道7外设地址寄存器</td><td>0x00000000</td></tr><tr><td>R32_DMA2_MADDR7</td><td>0x4002048C</td><td>DMA2通道7存储器地址寄存器</td><td>0x00000000</td></tr><tr><td>R32_DMA2_CFGR8</td><td>0x40020490</td><td>DMA2通道8配置寄存器</td><td>0x00000000</td></tr><tr><td>R32_DMA2_CNTR8</td><td>0x40020494</td><td>DMA2通道8传输数据数目寄存器</td><td>0x00000000</td></tr><tr><td>R32_DMA2_PADDR8</td><td>0x40020498</td><td>DMA2通道8外设地址寄存器</td><td>0x00000000</td></tr><tr><td>R32_DMA2_MADDR8</td><td>0x4002049C</td><td>DMA2通道8存储器地址寄存器</td><td>0x00000000</td></tr><tr><td>R32_DMA2_CFGR9</td><td>0x400204A0</td><td>DMA2通道9配置寄存器</td><td>0x00000000</td></tr><tr><td>R32_DMA2_CNTR9</td><td>0x400204A4</td><td>DMA2通道9传输数据数目寄存器</td><td>0x00000000</td></tr><tr><td>R32_DMA2_PADDR9</td><td>0x400204A8</td><td>DMA2通道9外设地址寄存器</td><td>0x00000000</td></tr><tr><td>R32_DMA2_MADDR9</td><td>0x400204AC</td><td>DMA2通道9存储器地址寄存器</td><td>0x00000000</td></tr><tr><td>R32_DMA2_CFGR10</td><td>0x400204B0</td><td>DMA2通道10配置寄存器</td><td>0x00000000</td></tr><tr><td>R32_DMA2_CNTR10</td><td>0x400204B4</td><td>DMA2通道10传输数据数目寄存器</td><td>0x00000000</td></tr><tr><td>R32_DMA2_PADDR10</td><td>0x400204B8</td><td>DMA2通道10外设地址寄存器</td><td>0x00000000</td></tr><tr><td>R32_DMA2_MADDR10</td><td>0x400204BC</td><td>DMA2通道10存储器地址寄存器</td><td>0x00000000</td></tr><tr><td>R32_DMA2_CFGR11</td><td>0x400204C0</td><td>DMA2通道11配置寄存器</td><td>0x00000000</td></tr><tr><td>R32_DMA2_CNTR11</td><td>0x400204C4</td><td>DMA2 通道 11 传输数据数目寄存器</td><td>0x00000000</td></tr><tr><td>R32_DMA2_PADDR11</td><td>0x400204C8</td><td>DMA2 通道 11 外设地址寄存器</td><td>0x00000000</td></tr><tr><td>R32_DMA2_MADDR11</td><td>0x400204CC</td><td>DMA2 通道 11 存储器地址寄存器</td><td>0x00000000</td></tr><tr><td>R32_DMA2_EXTEM_INTFR</td><td>0x400204D0</td><td>DMA2 扩展中断状态寄存器</td><td>0x00000000</td></tr><tr><td>R32_DMA2_EXTEM_INTFCR</td><td>0x400204D4</td><td>DMA2 扩展中断标志清除寄存器</td><td>0x00000000</td></tr></table>

### 11.3.1 DMAx 中断状态寄存器（DMAx_INTFR）（ $( x = 1 / 2 )$

偏移地址： $0 \times 0 0 + ( \times - 1 ) \times 0 \times 4 0 0$

<table><tr><td>31</td><td>30</td><td>29</td><td>28</td><td>27</td><td>26</td><td>25</td><td>24</td><td>23</td><td>22</td><td>21</td><td>20</td><td>19</td><td>18</td><td>17</td><td>16</td></tr><tr><td>TEIF8</td><td>HTIF8</td><td>TCIF8</td><td>GIF8</td><td>TEIF7</td><td>HTIF7</td><td>TCIF7</td><td>GIF7</td><td>TEIF6</td><td>HTIF6</td><td>TCIF6</td><td>GIF6</td><td>TEIF5</td><td>HTIF5</td><td>TCIF5</td><td>GIF5</td></tr><tr><td>15</td><td>14</td><td>13</td><td>12</td><td>11</td><td>10</td><td>9</td><td>8</td><td>7</td><td>6</td><td>5</td><td>4</td><td>3</td><td>2</td><td>1</td><td>0</td></tr><tr><td>TEIF4</td><td>HTIF4</td><td>TCIF4</td><td>GIF4</td><td>TEIF3</td><td>HTIF3</td><td>TCIF3</td><td>GIF3</td><td>TEIF2</td><td>HTIF2</td><td>TCIF2</td><td>GIF2</td><td>TEIF1</td><td>HTIF1</td><td>TCIF1</td><td>GIF1</td></tr></table>

<table><tr><td>位</td><td>名称</td><td>访问</td><td>描述</td><td>复位值</td></tr><tr><td>31/27/23/19/15/11/7/3</td><td>TEIFx</td><td>RO</td><td>通道x的传输错误标志(x=1/2/3/4/5/6/7/8):1:在通道x上发生了传输错误;0:在通道x上没有传输错误。硬件置位,软件写CTEIFx位清除此标志。</td><td>0</td></tr><tr><td>30/26/22/18/14/10/6/2</td><td>HTIFx</td><td>RO</td><td>通道x的传输过半标志(x=1/2/3/4/5/6/7/8):1:在通道x上产生了传输过半事件;0:在通道x上没有传输过半。硬件置位,软件写CHTIFx位清除此标志。</td><td>0</td></tr><tr><td>29/25/21/17/13/9/5/1</td><td>TCIFx</td><td>RO</td><td>通道x的传输完成标志(x=1/2/3/4/5/6/7/8):1:在通道x上产生了传输完成事件;0:在通道x上没有传输完成事件。硬件置位,软件写CTCIFx位清除此标志。</td><td>0</td></tr><tr><td>28/24/20/16/12/8/4/0</td><td>GIFx</td><td>RO</td><td>通道x的全局中断标志(x=1/2/3/4/5/6/7/8):1:在通道x上产生了TEIFx或HTIFx或TCIFx;0:在通道x上没有发生TEIFx或HTIFx或TCIFx。硬件置位,软件写CGIFx位清除此标志。</td><td>0</td></tr></table>

注：通道 $^ { 8 }$ 适用于CH32V20x_D8、CH32V20x_D8W、CH32F20x_D8W、CH32F20x_D6、CH32V20x_D6。

### 11.3.2 DMAx 中断标志清除寄存器（DMAx_INTFCR）（ $\bf { \sigma } _ { x = 1 / 2 ) }$ ）

偏移地址： $0 \times 0 . 4 + ( \times - 1 ) \ast 0 \times 4 0 0$

<table><tr><td>31</td><td>30</td><td>29</td><td>28</td><td>27</td><td>26</td><td>25</td><td>24</td><td>23</td><td>22</td><td>21</td><td>20</td><td>19</td><td>18</td><td>17</td><td>16</td></tr><tr><td>CTEIF8</td><td>CHTIF8</td><td>CTCIF8</td><td>CGIF8</td><td>CTEIF7</td><td>CHTIF7</td><td>CTCIF7</td><td>CGIF7</td><td>CTEIF6</td><td>CHTIF6</td><td>CTCIF6</td><td>CGIF6</td><td>CTEIF5</td><td>CHTIF5</td><td>CTCIF5</td><td>CGIF5</td></tr><tr><td>15</td><td>14</td><td>13</td><td>12</td><td>11</td><td>10</td><td>9</td><td>8</td><td>7</td><td>6</td><td>5</td><td>4</td><td>3</td><td>2</td><td>1</td><td>0</td></tr><tr><td>CTEIF4</td><td>CHTIF4</td><td>CTCIF4</td><td>CGIF4</td><td>CTEIF3</td><td>CHTIF3</td><td>CTCIF3</td><td>CGIF3</td><td>CTEIF2</td><td>CHTIF2</td><td>CTCIF2</td><td>CGIF2</td><td>CTEIF1</td><td>CHTIF1</td><td>CTCIF1</td><td>CGIF1</td></tr></table>

<table><tr><td>位</td><td>名称</td><td>访问</td><td>描述</td><td>复位值</td></tr><tr><td>31/27/23/19/15/11/7/3</td><td>CTEIFx</td><td>WO</td><td>清除通道x的传输错误标志（x=1/2/3/4/5/6/8）：1：清除DMA_INTF寄存器中的TEIFx标志；0：无作用。</td><td>0</td></tr><tr><td>30/26/22/18/14/10/6/2</td><td>CHTIFx</td><td>WO</td><td>清除通道x的传输过半标志(x=1/2/3/4/5/6/8):1:清除DMA_INTFRC寄存器中的HTIFx标志;0:无作用。</td><td>0</td></tr><tr><td>29/25/21/17/13/9/5/1</td><td>CTCIFx</td><td>WO</td><td>清除通道x的传输完成标志(x=1/2/3/4/5/6/7/8):1:清除DMA_INTFRC寄存器中的TCIFx标志;0:无作用。</td><td>0</td></tr><tr><td>28/24/20/16/12/8/4/0</td><td>CGIFx</td><td>WO</td><td>清除通道x的全局中断标志(x=1/2/3/4/5/6/7/8):1:清除DMA_INTFRC寄存器中的TEIFx/HTIFx/TCIFx/GIFx标志;0:无作用。</td><td>0</td></tr></table>

注：通道 $^ { 8 }$ 适用于CH32V20x_D8、CH32V20x_D8W、CH32F20x_D8W、CH32F20x_D6、CH32V20x_D6。

### 11.3.3 DMAy 通道 $\pmb { \times }$ 配置寄存器（DMAy_CFGRx）（x=1/2/3/4/5/6/7/8，y=0/1）

偏移地址： $0 \times 0 8 + ( \times - 1 ) * 2 0 + ( \times - 1 ) * 0 \times 4 0 0$

<table><tr><td>31</td><td>30</td><td>29</td><td>28</td><td>27</td><td>26</td><td>25</td><td>24</td><td>23</td><td>22</td><td>21</td><td>20</td><td>19</td><td>18</td><td>17</td><td>16</td></tr><tr><td colspan="16">Reserved</td></tr><tr><td>15</td><td>14</td><td>13</td><td>12</td><td>11</td><td>10</td><td>9</td><td>8</td><td>7</td><td>6</td><td>5</td><td>4</td><td>3</td><td>2</td><td>1</td><td>0</td></tr><tr><td>Reserved</td><td>MEM2
MEM</td><td colspan="2">PL[1:0]</td><td colspan="2">MSIZE[1:0]</td><td colspan="2">PSIZE[1:0]</td><td>MINC</td><td>PINC</td><td>CIRC</td><td>DIR</td><td>TEIE</td><td>HTIE</td><td>TCIE</td><td>EN</td></tr></table>

<table><tr><td>位</td><td>名称</td><td>访问</td><td>描述</td><td>复位值</td></tr><tr><td>[31:15]</td><td>Reserved</td><td>R0</td><td>保留。</td><td>0</td></tr><tr><td>14</td><td>MEM2MEM</td><td>RW</td><td>存储器到存储器模式使能:1:使能存储器到存储器数据传输模式;0:非存储器到存储器数据传输。</td><td>0</td></tr><tr><td>[13:12]</td><td>PL[1:0]</td><td>RW</td><td>通道优先级设置:00:低; 01:中;10:高; 11:最高。</td><td>00b</td></tr><tr><td>[11:10]</td><td>MSIZE[1:0]</td><td>RW</td><td>存储器地址数据宽度设置:00:8位; 01:16位;10:32位; 11:保留。</td><td>00b</td></tr><tr><td>[9:8]</td><td>PSIZE[1:0]</td><td>RW</td><td>外设地址数据宽度设置:00:8位; 01:16位;10:32位; 11:保留。</td><td>00b</td></tr><tr><td>7</td><td>MINC</td><td>RW</td><td>存储器地址增量递增模式使能:1:使能存储器地址增量递增操作;0:存储器地址保持不变操作。</td><td>0</td></tr><tr><td>6</td><td>PINC</td><td>RW</td><td>外设地址增量递增模式使能:1:使能外设地址增量递增操作;0:外设地址保持不变操作。</td><td>0</td></tr><tr><td>5</td><td>CIRC</td><td>RW</td><td>DMA通道循环模式使能:1:使能循环操作;0:执行单次操作。</td><td>0</td></tr><tr><td>4</td><td>DIR</td><td>RW</td><td>数据传输方向:</td><td>0</td></tr><tr><td></td><td></td><td></td><td>1:从存储器读;0:从外设读。</td><td></td></tr><tr><td>3</td><td>TEIE</td><td>RW</td><td>传输错误中断使能控制:1:使能传输错误中断;0:禁止传输错误中断。</td><td>0</td></tr><tr><td>2</td><td>HTIE</td><td>RW</td><td>传输过半中断使能控制:1:使能传输过半中断;0:禁止传输过半中断。</td><td>0</td></tr><tr><td>1</td><td>TCIE</td><td>RW</td><td>传输完成中断使能控制:1:使能传输完成中断;0:禁止传输完成中断。</td><td>0</td></tr><tr><td>0</td><td>EN</td><td>RW</td><td>通道使能控制:1:通道开启;0:通道关闭。发生DMA传输错误时,硬件自动将此位清0,关闭通道。</td><td>0</td></tr></table>

注：通道 $^ { 8 }$ 适用于CH32V20x_D8、CH32V20x_D8W、CH32F20x_D8W、CH32F20x_D6、CH32V20x_D6。

### 11.3.4 DMAy 通道 $\pmb { \times }$ 传输数据数目寄存器（DMAy_CNTRx）（x=1/2/3/4/5/6/7/8，y=0/1）

偏移地址： $0 \times 0 0 + ( x - 1 ) \times 2 0 + ( y - 1 ) \ast 0 \times 4 0 0$

<table><tr><td>31</td><td>30</td><td>29</td><td>28</td><td>27</td><td>26</td><td>25</td><td>24</td><td>23</td><td>22</td><td>21</td><td>20</td><td>19</td><td>18</td><td>17</td><td>16</td></tr><tr><td colspan="16">Reserved</td></tr><tr><td>15</td><td>14</td><td>13</td><td>12</td><td>11</td><td>10</td><td>9</td><td>8</td><td>7</td><td>6</td><td>5</td><td>4</td><td>3</td><td>2</td><td>1</td><td>0</td></tr><tr><td colspan="16">NDT[15:0]</td></tr></table>

<table><tr><td>位</td><td>名称</td><td>访问</td><td>描述</td><td>复位值</td></tr><tr><td>[31:16]</td><td>Reserved</td><td>R0</td><td>保留。</td><td>0</td></tr><tr><td>[15:0]</td><td>NDT[15:0]</td><td>RW</td><td>数据传输数量,范围0-65535。这个寄存器只能在通道不工作(DMA_CFGRx的EN=0)时写入。通道开启后该寄存器变为只读,指示剩余的待传输字节数目(寄存器内容在每次DMA传输后递减)。在通道为循环模式下,寄存器的内容将被自动重新加载为之前配置的数值。</td><td>0</td></tr></table>

注：此寄存器只能在EN=0时更改；EN=1时，为只读寄存器，表示当前待传输字节数目。当寄存器内容为0时，无论通道是否开启，都不会发生任何数据传输。通道8适用于CH32V20x_D8、CH32V20x_D8W、CH32F20x_D8W、CH32F20x_D6、CH32V20x_D6。

### 11.3.5 DMAy 通道 $\pmb { \times }$ 外设地址寄存器（DMAy_PADDRx）（x=1/2/3/4/5/6/7/8）

偏移地址： $0 \times 1 0 + ( \times - 1 ) * 2 0 + ( \times - 1 ) * 0 \times 4 0 0$

31 30 29 28 27 26 25 24 23 22 21 20 19 18 17 16 15 14 13 12 11 10 9 8 7 6 5 4 3 2 1 0

PA[31:0]

<table><tr><td>位</td><td>名称</td><td>访问</td><td>描述</td><td>复位值</td></tr><tr><td>[31:0]</td><td>PA[31:0]</td><td>RW</td><td>外设基地址，作为外设数据传输的源或目标地址。</td><td>0</td></tr><tr><td></td><td></td><td></td><td>当PSIZE[1:0]='01' (16位),模块自动忽略bit0,操作地址自动2字节对齐;当PSIZE[1:0]='10' (32位),模块自动忽略bit[1:0],操作地址自动4字节对齐。</td><td></td></tr></table>

注：此寄存器只能在 $E N { = } 0$ 时更改， $\bar { E N } { = } 1$ 时不可写。通道8适用于CH32V20x_D8、CH32V20x_D8W、CH32F20x_D8W、CH32F20x_D6、CH32V20x_D6。

### 11.3.6 DMAy 通道 $\pmb { \times }$ 存储器地址寄存器（DMAy_MADDRx）（x=1/2/3/4/5/6/7/8）

偏移地址： $0 \times 1 4 + ( x - 1 ) \ast 2 0 + ( y - 1 ) \ast 0 \times 4 0 0$

31 30 29 28 27 26 25 24 23 22 21 20 19 18 17 16 15 14 13 12 11 10 9 8 7 6 5 4 3 2 1 0

MA[31:0]

<table><tr><td>位</td><td>名称</td><td>访问</td><td>描述</td><td>复位值</td></tr><tr><td>[31:0]</td><td>MA[31:0]</td><td>RW</td><td>存储器数据地址,作为数据传输的源或目标地址。当MSIZE[1:0]=&#x27;01&#x27;(16位),模块自动忽略bit0,操作地址自动2字节对齐;当MSIZE[1:0]=&#x27;10&#x27;(32位),模块自动忽略bit[1:0],操作地址自动4字节对齐。</td><td>0</td></tr></table>

注：此寄存器只能在 $E N { = } 0$ 时更改，EN=1时不可写。通道8适用于CH32V20x_D8、CH32V20x_D8W、CH32F20x_D8W、CH32F20x_D6、CH32V20x_D6。

### 11.3.7 DMA2 通道 $\pmb { \times }$ 配置寄存器（DMA2_CFGRx）（x=8/9/10/11）

偏移地址： $0 \times 4 9 0 + ( \times - 8 ) \times 1 6$

<table><tr><td colspan="16">Reserved</td></tr><tr><td>15</td><td>14</td><td>13</td><td>12</td><td>11</td><td>10</td><td>9</td><td>8</td><td>7</td><td>6</td><td>5</td><td>4</td><td>3</td><td>2</td><td>1</td><td>0</td></tr><tr><td>Reserved</td><td>MEM2
MEM</td><td colspan="2">PL[1:0]</td><td colspan="2">MSIZE[1:0]</td><td colspan="2">PSIZE[1:0]</td><td>MINC</td><td>PINC</td><td>CIRC</td><td>DIR</td><td>TEIE</td><td>HTIE</td><td>TCIE</td><td>EN</td></tr></table>

<table><tr><td>位</td><td>名称</td><td>访问</td><td>描述</td><td>复位值</td></tr><tr><td>[31:15]</td><td>Reserved</td><td>R0</td><td>保留。</td><td>0</td></tr><tr><td>14</td><td>MEM2MEM</td><td>RW</td><td>存储器到存储器模式使能:1:使能存储器到存储器数据传输模式;0:非存储器到存储器数据传输。</td><td>0</td></tr><tr><td>[13:12]</td><td>PL[1:0]</td><td>RW</td><td>通道优先级设置:00:低; 01:中;10:高; 11:最高。</td><td>00b</td></tr><tr><td>[11:10]</td><td>MSIZE[1:0]</td><td>RW</td><td>存储器地址数据宽度设置:00:8位; 01:16位;10:32位; 11:保留。</td><td>00b</td></tr><tr><td>[9:8]</td><td>PSIZE[1:0]</td><td>RW</td><td>外设地址数据宽度设置:00:8位; 01:16位;10:32位; 11:保留。</td><td>00b</td></tr><tr><td>7</td><td>MINC</td><td>RW</td><td>存储器地址增量递增模式使能:</td><td>0</td></tr><tr><td></td><td></td><td></td><td>1:使能存储器地址增量递增操作;0:存储器地址保持不变操作。</td><td></td></tr><tr><td>6</td><td>PINC</td><td>RW</td><td>外设地址增量递增模式使能:1:使能外设地址增量递增操作;0:外设地址保持不变操作。</td><td>0</td></tr><tr><td>5</td><td>CIRC</td><td>RW</td><td>DMA通道循环模式使能:1:使能循环操作;0:执行单次操作。</td><td>0</td></tr><tr><td>4</td><td>DIR</td><td>RW</td><td>数据传输方向:1:从存储器读;0:从外设读。</td><td>0</td></tr><tr><td>3</td><td>TEIE</td><td>RW</td><td>传输错误中断使能控制:1:使能传输错误中断;0:禁止传输错误中断。</td><td>0</td></tr><tr><td>2</td><td>HTIE</td><td>RW</td><td>传输过半中断使能控制:1:使能传输过半中断;0:禁止传输过半中断。</td><td>0</td></tr><tr><td>1</td><td>TCIE</td><td>RW</td><td>传输完成中断使能控制:1:使能传输完成中断;0:禁止传输完成中断。</td><td>0</td></tr><tr><td>0</td><td>EN</td><td>RW</td><td>通道使能控制:1:通道开启;0:通道关闭。发生DMA传输错误时,硬件自动将此位清0,关闭通道。</td><td>0</td></tr></table>

注：适用于CH32F20x_D8、CH32F20x_D8C、CH32V30x_D8、CH32V30x_D8C。

### 11.3.8 DMA2 通道 $\pmb { \times }$ 传输数据数目寄存器（DMA2_CNTRx）（x=8/9/10/11）

偏移地址： $0 { \times } 4 9 4 + ( { \times } - 8 ) { \ast } 1 6$

<table><tr><td>31</td><td>30</td><td>29</td><td>28</td><td>27</td><td>26</td><td>25</td><td>24</td><td>23</td><td>22</td><td>21</td><td>20</td><td>19</td><td>18</td><td>17</td><td>16</td></tr><tr><td colspan="16">Reserved</td></tr><tr><td>15</td><td>14</td><td>13</td><td>12</td><td>11</td><td>10</td><td>9</td><td>8</td><td>7</td><td>6</td><td>5</td><td>4</td><td>3</td><td>2</td><td>1</td><td>0</td></tr><tr><td colspan="16">NDT[15:0]</td></tr></table>

<table><tr><td>位</td><td>名称</td><td>访问</td><td>描述</td><td>复位值</td></tr><tr><td>[31:16]</td><td>Reserved</td><td>R0</td><td>保留。</td><td>0</td></tr><tr><td>[15:0]</td><td>NDT[15:0]</td><td>RW</td><td>数据传输数量,范围0-65535。这个寄存器只能在通道不工作(DMA_CFGRx的EN=0)时写入。通道开启后该寄存器变为只读,指示剩余的待传输字节数目(寄存器内容在每次DMA传输后递减)。在通道为循环模式下,寄存器的内容将被自动重新加载为之前配置的数值。</td><td>0</td></tr></table>

注：此寄存器只能在EN=0时更改；EN=1时，为只读寄存器，表示当前待传输字节数目。当寄存器内容为0时，无论通道是否开启，都不会发生任何数据传输，适用于CH32F20x_D8、CH32F20x_D8C、CH32V30x_D8、CH32V30x_D8C。

### 11.3.9 DMA2 通道 $\pmb { \times }$ 外设地址寄存器（DMA2_PADDRx）（x=8/9/10/11）

偏移地址： $0 \times 4 9 8 + ( \times - 8 ) \times 1 6$

31 30 29 28 27 26 25 24 23 22 21 20 19 18 17 16 15 14 13 12 11 10 9 8 7 6 5 4 3 2 1 0

PA[31:0]   

<table><tr><td>位</td><td>名称</td><td>访问</td><td>描述</td><td>复位值</td></tr><tr><td>[31:0]</td><td>PA[31:0]</td><td>RW</td><td>外设基地址，作为外设数据传输的源或目标地址。当PSIZE[1:0]=&#x27;01&#x27;（16位），模块自动忽略bit0，操作地址自动2字节对齐；当PSIZE[1:0]=&#x27;10&#x27;（32位），模块自动忽略bit[1:0]，操作地址自动4字节对齐。</td><td>0</td></tr></table>

注：此寄存器只能在EN=0时更改，EN=1时不可写，适用于CH32F20x_D8、CH32F20x_D8C、CH32V30x_D8、CH32V30x_D8C。

### 11.3.10 DMA2 通道 $\pmb { \times }$ 存储器地址寄存器（DMA2_MADDRx）（x=8/9/10/11）

偏移地址： $0 \times 4 9 0 + ( x - 8 ) \times 1 6$

31 30 29 28 27 26 25 24 23 22 21 20 19 18 17 16 15 14 13 12 11 10 9 8 7 6 5 4 3 2 1 0

MA[31:0]   

<table><tr><td>位</td><td>名称</td><td>访问</td><td>描述</td><td>复位值</td></tr><tr><td>[31:0]</td><td>MA[31:0]</td><td>RW</td><td>存储器数据地址,作为数据传输的源或目标地址。当MSIZE[1:0]=&#x27;01&#x27;(16位),模块自动忽略bit0,操作地址自动2字节对齐;当MSIZE[1:0]=&#x27;10&#x27;(32位),模块自动忽略bit[1:0],操作地址自动4字节对齐。</td><td>0</td></tr></table>

注：此寄存器只能在EN=0时更改，EN=1时不可写，适用于CH32F20×_D8、CH32F20x_D8C、CH32V30x_D8、CH32V30x_D8C。

### 11.3.11 DMA2 扩展中断状态寄存器（DMA2_EXTEM_INTFR）

偏移地址： $0 \times 4 0 0$

<table><tr><td>31</td><td>30</td><td>29</td><td>28</td><td>27</td><td>26</td><td>25</td><td>24</td><td>23</td><td>22</td><td>21</td><td>20</td><td>19</td><td>18</td><td>17</td><td>16</td></tr><tr><td colspan="16">Reserved</td></tr><tr><td>15</td><td>14</td><td>13</td><td>12</td><td>11</td><td>10</td><td>9</td><td>8</td><td>7</td><td>6</td><td>5</td><td>4</td><td>3</td><td>2</td><td>1</td><td>0</td></tr><tr><td>TEIF 11</td><td>HTIF 11</td><td>TCIF 11</td><td>GIF 11</td><td>TEIF 10</td><td>HTIF 10</td><td>TCIF 10</td><td>GIF 10</td><td>TEIF 9</td><td>HTIF 9</td><td>TCIF 9</td><td>GIF 9</td><td>TEIF 8</td><td>HTIF 8</td><td>TCIF 8</td><td>GIF 8</td></tr></table>

<table><tr><td>位</td><td>名称</td><td>访问</td><td>描述</td><td>复位值</td></tr><tr><td>[31:16]</td><td>Reserved</td><td>R0</td><td>保留。</td><td>0</td></tr><tr><td>15/11/7/3</td><td>TEIFx</td><td>R0</td><td>通道x的传输错误标志（x=8/9/10/11）：1：在通道x上发生了传输错误；0：在通道x上没有传输错误。硬件置位，软件写CTEIFx位清除此标志。</td><td>0</td></tr><tr><td>14/10/6/2</td><td>HTIFx</td><td>R0</td><td>通道x的传输过半标志（x=8/9/10/11）：</td><td>0</td></tr><tr><td></td><td></td><td></td><td>1: 在通道 x 上产生了传输过半事件;0: 在通道 x 上没有传输过半。硬件置位,软件写 CHTIFx 位清除此标志。</td><td></td></tr><tr><td>13/9/5/1</td><td>TCIFx</td><td>RO</td><td>通道 x 的传输完成标志 (x=8/9/10/11):1: 在通道 x 上产生了传输完成事件;0: 在通道 x 上没有传输完成事件。硬件置位,软件写 CTCIFx 位清除此标志。</td><td>0</td></tr><tr><td>12/8/4/0</td><td>GIFx</td><td>RO</td><td>通道 x 的全局中断标志 (x=8/9/10/11):1: 在通道 x 上产生了 TEIFx 或 HTIFx 或 TCIFx;0: 在通道 x 上没有发生 TEIFx 或 HTIFx 或 TCIFx。硬件置位,软件写 CGIFx 位清除此标志。</td><td>0</td></tr></table>

注：适用于CH32F20x_D8、CH32F20x_D8C、CH32V30x_D8、CH32V30x_D8C。

### 11.3.12 DMA2 扩展中断标志清除寄存器（DMA2_EXTEM_INTFCR）

偏移地址： $0 \times 4 0 4$

<table><tr><td colspan="17">Reserved</td></tr><tr><td>15</td><td>14</td><td>13</td><td>12</td><td>11</td><td>10</td><td>9</td><td>8</td><td>7</td><td>6</td><td>5</td><td>4</td><td>3</td><td>2</td><td>1</td><td>0</td><td></td></tr><tr><td>CTEIF11</td><td>CHTIF11</td><td>CTCIF11</td><td>CGIF11</td><td>CTEIF10</td><td>CHTI F10</td><td>CTCIF10</td><td>CGIF10</td><td>CTEIF9</td><td>CHTI F9</td><td>CTCIF9</td><td>CGIF9</td><td>CTEIF9</td><td>CTCIF8</td><td>CHTIF8</td><td>CTCIF8</td><td>CGIF8</td></tr></table>

<table><tr><td>位</td><td>名称</td><td>访问</td><td>描述</td><td>复位值</td></tr><tr><td>[31:16]</td><td>Reserved</td><td>RO</td><td>保留。</td><td>0</td></tr><tr><td>15/11/7/3</td><td>CTEIFx</td><td>WO</td><td>清除通道x的传输错误标志(x=8/9/10/11):1:清除DMA_INTFRC寄存器中的TEIFx标志;0:无作用。</td><td>0</td></tr><tr><td>14/10/6/2</td><td>CHTIFx</td><td>WO</td><td>清除通道x的传输过半标志(x=8/9/10/11):1:清除DMA_INTFRC寄存器中的HTIFx标志;0:无作用。</td><td>0</td></tr><tr><td>13/9/5/1</td><td>CTCIFx</td><td>WO</td><td>清除通道x的传输完成标志(x=8/9/10/11):1:清除DMA_INTFRC寄存器中的TCIFx标志;0:无作用。</td><td>0</td></tr><tr><td>12/8/4/0</td><td>CGIFx</td><td>WO</td><td>清除通道x的全局中断标志(x=8/9/10/11):1:清除DMA_INTFRC寄存器中的TEIFx/HTIFx/TCIFx/GIFx标志;0:无作用。</td><td>0</td></tr></table>

注：适用于CH32F20x_D8、CH32F20x_D8C、CH32V30x_D8、CH32V30x_D8C。

# 第 12 章 模拟/数字转换（ADC）

本章模块描述适用于CH32F2x、CH32V2x和CH32V3x微控制器全系列产品。

ADC 模块包含2个12位的逐次逼近型的模拟数字转换器，最高 14MHz 的输入时钟。支持16个外部通道和2个内部信号源采样源。可完成通道的单次转换、连续转换，通道间自动扫描模式、间断模式、外部触发模式、双重采样等功能。可以通过模拟看门狗功能监测通道电压是否在阈值范围内。

## 12.1 主要特性

12 位分辨率  
支持 16 个外部通道和 2 个内部信号源采样  
$\bullet$ 多通道的多种采样转换方式：单次、连续、扫描、触发、间断等  
$\bullet$ 数据对齐模式：左对齐、右对齐  
$\bullet$ 采样时间可按通道分别编程  
$\bullet$ 规则转换和注入转换均支持外部触发  
$\bullet$ 模拟看门狗监测通道电压，自校准功能  
$\bullet$ 双重模式  
$\bullet$ ADC 通道输入范围： $0 \leqslant V _ { \mathsf { I N } } \leqslant V _ { \mathsf { D O A } }$   
输入增益可调，可实现小信号放大采样

## 12.2 功能描述

### 12.2.1 模块结构

![](images/c6d7da1fd51bfacc9b96f600f5f3063165830b18336559d977be35e66f5dfedb.jpg)  
图 12-1 ADC 模块框图

### 12.2.2 ADC 配置

#### 1）模块上电

ADC_CTLR2 寄存器的 ADON 位为 1 表示 ADC 模块上电。当 ADC 模块从断电模式（ADON=0）下进入上电状态（ADON=1）后，需要延迟一段时间 tSTAB用于模块稳定时间。之后再次写入 ADON 位为 1，用于作为软件启动ADC转换的启动信号。通过清除 ADON位为 0，可以终止当前转换并将 ADC模块置于断电模式，这个状态下，ADC几乎不耗电。

#### 2）采样时钟

模块的寄存器操作基于PCLK2（APB2总线）时钟，其转换单元的时钟基准 ADCCLK与 PCLK2同步，由 RCC_CFGR0 寄存器的 ADCPRE[1:0]域配置分频，最大不能超过 14MHz。

#### 3）通道配置

ADC 模块提供了18个通道采样源，包括 16个外部通道和 2个内部通道。它们可以配置到两种转换组中：规则组和注入组。以实现任意多个通道上以任意顺序进行一系列转换构成的组转换。

#### 转换组：

规则组：由多达16个转换组成。规则通道和它们的转换顺序在 ADC_RSQRx 寄存器中设置。规则组中转换的总数量应写入ADC_RSQR1寄存器的RLEN[3:0]中。  
注入组：由多达 4 个转换组成。注入通道和它们的转换顺序在 ADC_ISQR 寄存器中设置。注入组里的转换总数量应写入ADC_ISQR寄存器的 ILEN[1:0]中。

注：如果ADC_RSQRx或ADC_ISQR寄存器在转换期间被更改，当前的转换被终止，一个新的启动信号将发送到ADC以转换新选择的组。

#### 2 个内部通道：

$\bullet$ 温度传感器：连接ADC_IN16通道，用来测量器件周围的温度(TA)。  
VREFINT 内部参考电压：连接 ADC_IN17 通道。

#### 4）校准

ADC 有一个内置自校准模式。经过校准环节可大幅减小因内部电容器组的变化而造成的精准度误差。在校准期间，在每个电容器上都会计算出一个误差修正码，用于消除在随后的转换中每个电容器上产生的误差。

通过写ADC_CTLR2寄存器的RSTCAL位置1 初始化校准寄存器，等待 RSTCAL硬件清 0表示初始化完成。置位 CAL 位，启动校准功能，一旦校准结束，硬件会自动清除 CAL 位，将校准码存储到ADC_RDATAR中。之后可以开始正常的转换功能。建议在 ADC模块上电时执行一次 ADC校准。

注：启动校准前，必须保证ADC模块处于上电状态(ADON=1)超过至少两个ADC时钟周期。

#### 5）可编程采样时间

ADC 使用若干个ADCCLK周期对输入电压采样，通道的采样周期数目可以通过ADC_SAMPTR1 和ADC_SAMPTR2 寄存器中的 $\mathsf { S M P } \times [ 2 : 0 ]$ 位更改。每个通道可以分别使用不同的时间采样。

总转换时间如下计算：

TCONV $=$ 采样时间 $+ 1 2 . 5 \mathsf { T } _ { \mathsf { A D C C l K } }$

ADC的规则通道转换支持DMA功能。规则通道转换的值储存在一个仅有的数据寄存器 ADC_RDATAR中，为防止连续转换多个规则通道时，没有及时取走 ADC_RDATAR 寄存器中的数据，可以开启 ADC 的DMA功能。硬件会在规则通道的转换结束时（EOC置位）产生 DMA请求，并将转换的数据从 ADC_RDATAR寄存器传输到用户指定的目的地址。

对 DMA 控制器模块的通道配置完成后，写 ADC_CTLR2 寄存器的 DMA 位置 1，开启 ADC 的 DMA 功能。注：注入组转换不支持DMA功能。

#### 6）数据对齐

ADC_CTLR2寄存器中的ALIGN位选择ADC转换后的数据存储对齐方式。12 位数据支持左对齐和右对齐模式。

规则组通道的数据寄存器ADC_RDATAR保存的是实际转换的 12位数字值；而注入组通道的数据寄存器ADC_IDATARx是实际转换的数据减去 ADC_IOFRx寄存器的定义的偏移量后写入的值，会存在正负情况，所以有符号位（SIGNB）。

图 12-2 数据左对齐  
规则组数据寄存器  

<table><tr><td>D11</td><td>D10</td><td>D9</td><td>D8</td><td>D7</td><td>D6</td><td>D5</td><td>D4</td><td>D4</td><td>D2</td><td>D1</td><td>D0</td><td>0</td><td>0</td><td>0</td><td>0</td></tr></table>

注入组数据寄存器  

<table><tr><td>SIGNB</td><td>D11</td><td>D10</td><td>D9</td><td>D8</td><td>D7</td><td>D6</td><td>D5</td><td>D4</td><td>D3</td><td>D2</td><td>D1</td><td>D0</td><td>0</td><td>0</td><td>0</td></tr></table>

图 12-3 数据右对齐

规则组数据寄存器  

<table><tr><td>0</td><td>0</td><td>0</td><td>0</td><td>D11</td><td>D10</td><td>D9</td><td>D8</td><td>D7</td><td>D6</td><td>D5</td><td>D4</td><td>D3</td><td>D2</td><td>D1</td><td>D0</td></tr></table>

注入组数据寄存器  

<table><tr><td>SIGNB</td><td>SIGNB</td><td>SIGNB</td><td>SIGNB</td><td>D11</td><td>D10</td><td>D9</td><td>D8</td><td>D7</td><td>D6</td><td>D5</td><td>D4</td><td>D3</td><td>D2</td><td>D1</td><td>D0</td></tr></table>

### 12.2.3 外部触发源

ADC 转换的启动事件可以由外部事件触发。如果设置了 ADC_CTLR2 寄存器的 EXTTRIG 或 JEXTTRIG位，则可分别通过外部事件触发规则组或注入组通道的转换。此时，EXTSEL[2:0]和 JEXTSEL[2:0]位的配置决定规则组和注入组的外部事件源。

注：当外部触发信号被选为ADC规则或注入转换时，只有它的上升沿可以启动转换。

表 12-1 规则组通道的外部触发源  

<table><tr><td>EXTSEL[2:0]</td><td>触发源</td><td>类型</td></tr><tr><td>000</td><td>定时器1的CC1事件</td><td rowspan="6">来自片上定时器的内部信号</td></tr><tr><td>001</td><td>定时器1的CC2事件</td></tr><tr><td>010</td><td>定时器1的CC3事件</td></tr><tr><td>011</td><td>定时器2的CC2事件</td></tr><tr><td>100</td><td>定时器3的TRGO事件</td></tr><tr><td>101</td><td>定时器4的CC4事件</td></tr><tr><td>110</td><td>EXTI线11/TIM8_TRGO</td><td>来自外部引脚/内部定时器信号</td></tr><tr><td>111</td><td>SWSTART位置1软件触发</td><td>软件控制位</td></tr></table>

表12-2 注入组通道的外部触发源  

<table><tr><td>JEXTSEL[2:0]</td><td>触发源</td><td>类型</td></tr><tr><td>000</td><td>定时器1的TRGO事件</td><td rowspan="6">来自片上定时器的内部信号</td></tr><tr><td>001</td><td>定时器1的CC4事件</td></tr><tr><td>010</td><td>定时器2的TRGO事件</td></tr><tr><td>011</td><td>定时器2的CC1事件</td></tr><tr><td>100</td><td>定时器3的CC4事件</td></tr><tr><td>101</td><td>定时器4的TRGO事件</td></tr><tr><td>110</td><td>EXTI线15/TIM8_CC4</td><td>来自外部引脚/内部定时器信号</td></tr><tr><td>111</td><td>JSWSTART位置1软件触发</td><td>软件控制位</td></tr></table>

### 12.2.4 转换模式

表12-3 转换模式组合  

<table><tr><td colspan="5">ADC_CTLR1和ADC_CTLR2寄存器控制位</td><td rowspan="2">ADC转换模式</td></tr><tr><td>CONT</td><td>SCAN</td><td>RDISCEN/IDISCEN</td><td>IAUTO</td><td>启动事件</td></tr><tr><td rowspan="3">0</td><td rowspan="2">0</td><td rowspan="2">0</td><td rowspan="2">0</td><td>ADON位置1</td><td>单次单通道模式:某一规则通道执行单次转换。</td></tr><tr><td>外部触发方式</td><td>单次单通道模式:规则通道或注入通道的某一通道执行单次转换。</td></tr><tr><td>1</td><td>0</td><td>0</td><td>ADON位置</td><td>单次扫描模式:按顺序对选中的所有规则组</td></tr><tr><td rowspan="5"></td><td rowspan="2"></td><td rowspan="2"></td><td></td><td>1或外部触发方式</td><td>通道(ADC_RSQRx)或所有注入组通道(ADC_ISQR)逐个执行单次转换。触发注入方式:当规则组通道转换过程中可以插入注入组通道所有转换,之后再继续规则组通道转换;但转换注入组通道时不会插入规则组通道转换。</td></tr><tr><td>1</td><td>ADON位置1或外部触发方式</td><td>单次扫描模式:按顺序对选中的所有规则组通道(ADC_RSQRx)或所有注入组通道(ADC_ISQR)逐个执行单次转换。自动注入方式:在规则组通道转换完之后,注入组通道被自动转换。注:转换过程中不允许出现注入通道的外部触发信号。</td></tr><tr><td rowspan="2">0</td><td rowspan="2">1(RDISCEN和IDISCEN不能同时为1)</td><td>0</td><td>外部触发方式</td><td>单次间断模式:每次启动事件,执行一个短序列(DISCNUM[2:0]定义数量)的通道数量转换,直到所有选中通道转换完成才能重头开始。注:规则组和注入组选中此模式控制位分别为IDISCEN和RDISCEN,不能同时为规则组和注入组配置间断模式,间断模式只能用于一组转换。</td></tr><tr><td>1</td><td>-</td><td>禁止此模式。</td></tr><tr><td>1</td><td>1</td><td>X</td><td>-</td><td>无此模式。</td></tr><tr><td rowspan="3">1</td><td>0</td><td>0</td><td>0</td><td rowspan="3">ADON位置1或外部触发方式</td><td rowspan="3">连续单通道/扫描模式:每轮结束后重复新一轮的转换,直到CONT清0才能终止。</td></tr><tr><td rowspan="2">1</td><td rowspan="2">0</td><td>0</td></tr><tr><td>1</td></tr></table>

注：规则组和注入组的外部触发事件是不一样的，而且‘ACON’位只能启动规则组通道转换，所以规则组和注入组通道转换的启动事件独立。

#### 1）单次单通道转换模式

此模式下，对当前 1 个通道只执行一次转换。该模式对规则组或注入组中排序第 1 的通道执行转换，其中通过设置 ADC_CTLR2 寄存器的 ADON 位置 1(只适用于规则通道)启动也可通过外部触发启动(适用于规则通道或注入通道)。一旦选择通道的转换完成将：

如果转换的是规则组通道，则转换数据被储存在 16 位 ADC_RDATAR 寄存器中，EOC 标志被置位，如果设置了EOCIE位，将触发ADC中断。

如果转换的是注入组通道，则转换数据被储存在 16 位 ADC_IDATAR1 寄存器中，EOC 和 JEOC 标志被置位，如果设置了 JEOCIE 或 EOCIE 位，将触发 ADC 中断。

#### 2）单次扫描模式转换

通过设置 ADC_CTLR1 寄存器的 SCAN 位为 1 进入 ADC 扫描模式。此模式用来扫描一组模拟通道，对被 ADC_RSQRx 寄存器(对规则通道)或 ADC_ISQR(对注入通道)选中的所有通道逐个执行单次转换，当前通道转换结束时，同一组的下一个通道被自动转换。

在扫描模式里，根据 IAUTO 位的状态，又分为触发注入方式和自动注入方式。

#### 触发注入

IAUTO 位为 0，当在扫描规则组通道过程中，发生了注入组通道转换的触发事件，当前转换被复位，注入通道的序列被以单次扫描方式进行，在所有选中的注入组通道扫描转换结束后，恢复上次被

中断的规则组通道转换。

如果当前在扫描注入组通道序列时，发生了规则通道的启动事件，注入组转换不会被中断，而是在注入序列转换完成后再执行规则序列的转换。

注：使用触发的注入转换时，必须保证触发事件的间隔长于注入序列。例如，完成注入序列的转换总体时间需要28个ADCCLK，那么触发注入通道的事件间隔时间最小值为29个ADCCLK。

#### 自动注入

IAUTO 位为 1，在扫描完规则组选中的所有通道转换后，自动进行注入组选中通道的转换。这种方式可以用来转换ADC_RSQRx和ADC_ISQR寄存器中多达 20个转换序列。

此模式里，必须禁止注入通道的外部触发（JEXTTRIG=0）。

注：对于ADC时钟预分频系数（ADCPRE[1:0]）为4至8时，当从规则转换切换到注入序列或从注入转换切换到规则序列时，会自动插入1个ADCCLK间隔；当ADC时钟预分频系数为2时，则有2个间隔的延迟。

#### 3）单次间断模式转换

通过设置 ADC_CTLR1 寄存器的 RDISCEN 或 IDISCEN 位为 1 进入规则组或注入组的间断模式。此模式区别扫描模式中扫描完整的一组通道，而是将一组通道分为多个短序列，每次外部触发事件将执行一个短序列的扫描转换。

短序列的长度n（ $n < = 8 1$ ）定义在 ADC_CTLR1 寄存器的 DISCNUM[2:0]中，当 RDISCEN 为 1，则是规则组的间断模式，待转换总长度定义在 ADC_RSQR1 寄存器的 RLEN[3:0]中；当 IDISCEN 为 1，则是注入组的间断模式，待转换总长度定义在 ADC_ISQR 寄存器的 ILEN[1:0]中。不能同时将规则组和注入组设置为间断模式。

规则组间断模式举例：

RDISCEN=1，DISCNUM[2:0]=3，RLEN[3:0]=8，待转换通道 $\cdot = 1$ ，3，2，5，8，4，10，6

第 1 次外部触发：转换序列为：1，3，2

第 2 次外部触发：转换序列为：5，8，4

第 3 次外部触发：转换序列为：10，6，同时产生 EOC 事件

第 4 次外部触发：转换序列为：1，3，2

注入组间断模式举例：

IDISCEN=1，ILEN[1:0]=3，待转换通道=1，3，2

第 1 次外部触发：转换序列为：1

第2次外部触发：转换序列为：3

第 3 次外部触发：转换序列为：2，同时产生 EOC 和 JEOC 事件

第 4 次外部触发：转换序列为：1

注：1.当以间断模式转换一个规则组或注入组时，转换序列结束后不自动从头开始。当所有子组被转换完成，下一次触发事件启动第一个子组的转换。

2.不能同时使用自动注入（ $1 A U T O = 1 .$ ）和间断模式。

3.不能同时为规则组和注入组设置间断模式，间断模式只能用于一组转换。

注入组的间断模式下， 外部触发后要转换的注入通道数目为 1。

#### 4）连续转换

通过设置 ADC_CTLR2 寄存器的 CONT 位为 1，进入 ADC 的连续转换模式。此模式在前面 ADC 转换一结束马上就启动另一次转换，转换不会在选择组的最后一个通道上停止，而是再次从选择组的第一个通道继续转换。

启动事件包括外部触发事件和 ADON 位置 1。结合前面的单次模式中的几种转换方式，也包括连续单通道转换、连续扫描模式（触发注入或自动注入）转换。

### 12.2.5 模拟看门狗

如果被 ADC转换的模拟电压低于低阀值或高于高阀值，AWD模拟看门狗状态位被设置。阀值设置位于 ADC_WDHTR 和 ADC_WDLTR 寄存器的最低 12 个有效位中。通过设置 ADC_CTLR1 寄存器的 AWDIE位以允许产生相应中断。

![](images/e6546e2567d8b223b610f4646c46b3c7fc447d2803362bae6f6946b99e30b520.jpg)  
图 12-4 模拟看门狗阈值区

配置 ADC_CTLR1 寄存器的 AWDSGL、RAWDEN、IAWDEN 及 AWDCH[4:0]位选择模拟看门狗警戒的通道，具体关系见下表：

表12-4 模拟看门狗通道选择  

<table><tr><td rowspan="2">模拟看门狗警戒通道</td><td colspan="4">ADC_CTLR1 寄存器控制位</td></tr><tr><td>AWDSGL</td><td>RAWDEN</td><td>IAWDEN</td><td>AWDCH[4:0]</td></tr><tr><td>不警戒</td><td>忽略</td><td>0</td><td>0</td><td>忽略</td></tr><tr><td>所有注入通道</td><td>0</td><td>0</td><td>1</td><td>忽略</td></tr><tr><td>所有规则通道</td><td>0</td><td>1</td><td>0</td><td>忽略</td></tr><tr><td>所有注入和规则通道</td><td>0</td><td>1</td><td>1</td><td>忽略</td></tr><tr><td>单一注入通道</td><td>1</td><td>0</td><td>1</td><td>决定通道编号</td></tr><tr><td>单一规则通道</td><td>1</td><td>1</td><td>0</td><td>决定通道编号</td></tr><tr><td>单一注入和规则通道</td><td>1</td><td>1</td><td>1</td><td>决定通道编号</td></tr></table>

### 12.2.6 温度传感器

模块内置温度传感器，连接ADC_INT16通道，通过ADC 将传感器输出的电压转换成数字值来反馈器件周围温度，推荐设置采样时间是17.1us。温度传感器输出的电压随温度线性变化，由于生产差异，其线性变化的曲线斜率和偏移有所不同，所以内部温度传感器更适合于检测温度的变化，而不是测量绝对的温度。如果需要测量精确的温度，应该使用一个外置的温度传感器。

通过设置ADC_CTLR2寄存器的TSVREFE位置 1，唤醒 ADC内部采样通道，软件启动或外部触发启动ADC的温度传感器通道转换，读取数据结果（mV）。其中，数字值和温度 ${ \mathcal { C } } )$ 换算公式如下：

温度 $( ^ { \circ } C ) \ = \ ( \ ( \lor _ { { \tt S E N S E } } - \lor _ { { \tt 2 5 } } ) / \mathsf { A v g \_ S l o p e } ) + 2 5$ $^ { + 2 5 }$

V25：温度传感器在 $2 5 \mathrm { ^ \circ C }$ 下的电压值

Avg_Slope：温度与 VSENSE 曲线的平均斜率 $\mathsf { \Omega } ( \mathsf { m } \mathsf { V } / ^ { \circ } \mathsf { C } $ ）

参考数据手册电气特性章节中 $\mathsf { V } _ { 2 5 }$ 和 Avg_Slope 的实际值。

注：内部温度传感器上电（TSVREFE位从0改为1）需要一个建立时间，而ADC模块上电也需要一个建立时间（ADON位从 $o$ 改为1），所以为了缩短等待时间，可以同时设置ADON和TSVREFE位。

### 12.2.7 双 ADC 模式

在有 2个的ADC模块产品中，可以使2个 ADC配合使用，实现双 ADC模式。在双 ADC模式中，ADC1 为主 ADC，ADC2 为从 ADC。通过配置 ADC1_CTLR1 中 DAULMODE[3:0]以选择模式，实现 ADC1 和ADC2 交替触发或同步触发转换。

注：双ADC模式中，选择外部触发事件触发时，用户必须使能主从ADC的外部触发使能且需要将主

设为相应的触发 从ADC设置为软件触发 防止不必要的触发使从ADC进行转换。

通过配置可以实现以下几种可能的模式：

独立模式  
同步注入模式  
$\bullet$ 同步规则模式  
$\bullet$ 快速交替模式  
$\bullet$ 慢速交替模式  
$\bullet$ 交替触发模式  
$\bullet$ 同步规则模式 $^ +$ 同步注入模式  
$\bullet$ 同步规则模式 $^ +$ 交替触发模式  
$\bullet$ 同步注入模式 $^ { + }$ 快速交替模式  
$\bullet$ 同步注入模式 $^ +$ 慢速交替模式

注：1.双ADC模式中，为了主数据寄存器能够读取从ADC的转换数据，需要使能DMA位。

2.只有ADC1拥有DMA功能。而ADC2转化的数据只可以通过双ADC模式，利用ADC1的 DMA功能传输。

![](images/5d279e6bcf66f391e5e8aa6473d505e7e63669bbd08ffbb3751a41f020106f14.jpg)  
图 12-5 双 ADC 框图

#### 1）独立模式

此模式下，双 ADC 不同步工作，相互之间独立工作。

#### 2）同步注入模式

此模式下用于转换一个注入通道组，设置 ADC1_CTLR2中 JEXTSEL[2:0]，以选择触发源,同时它也将用于同步触发ADC2。转换完成时，转换后的数据分别存储在 $\mathsf { A D C x }$ 的 ADC_IDATARx 中，且若使能任一ADC中断，转换结束后将产生JEOC中断。

![](images/feac0f027f963a63076a674bbe0085b3c4d3dde81728a72123211685b9846351.jpg)  
图12-6 4通道同步注入转换

注：1.同一时刻 ADC1和ADC2的转换通道不应重合。  
2.同步模式下，ADC1和ADC2应有相同时间长度的转换序列或两者之中较长的转换序列的时长小于触发时间间隔，以保证每次触发两个序列均能转换完成。

#### 3）同步规则模式

此模式下用于转换规则通道序列，设置 ADC1_CTLR2中 EXTSEL[2:0]，以选择触发源，同时它也将用于同步触发ADC2。转换完成时，将产生一个 32位 DMA传输请求，将数据寄存器 ADC1_RDATAR的内容传输到 SRAM 中，高 16 位包含 ADC2 转换数据，低 16 位包含 ADC1 转换数据。若使能任一 ADC中断，将产生EOC中断。

![](images/6799a21698f605937424a51484046e3d9c9485a757f6988bb4b52debe23e43ad.jpg)  
图12-7 16通道同步规则转换

注：1.同一时刻ADC1和ADC2的转换通道不应重合；  
2.同步模式下，ADC1和ADC2应有相同时间长度的转换序列或两者之中较长的转换序列的时长小于触发时间间隔，以保证每次触发两个序列均能转换完成。

#### 4）快速交替模式

此模式仅适用于规则通道（往往只有一个通道），设置 ADC1_CTLR2 中 EXTSEL[2:0]，以选择触发源，当触发产生后 ADC2 将立即启动转换，ADC1 在延迟 7 个 ADC 时钟周期后启动转换。如果均开启连续模式（CONT 被置位），两个 ADC 将对规则通道连续交替转换。如果使能中断，ADC1 将产生EOC 中断，若同时使能 DMA，则将产生一个 32 位的 DMA 传输请求，将数据寄存器 ADC1_RDATAR 的内容传输到 SRAM 中，高 16 位包含 ADC2 转换数据，低 16 位包含 ADC1 转换数据。

![](images/fcb122cf89db8854b35fb4e9ffe0a51454f51904be31655bfb7bc5aa02d7fdd1.jpg)  
图12-8 单通道快速交替连续转换

![](images/4d2f9b634f78ee64e4b974c6437debc057e55f6359d21ce698e12ce46fb78743.jpg)  
注：采样时间应小于7个ADC时钟周期，以避免ADC1和ADC2在采样同一个通道时，出现采样周期重合的问题。

#### 5）慢速交替模式

此模式仅适用于规则通道且只能一个通道。设置 ADC1_CTLR2中 EXTSEL[2:0]，以选择触发源，当触发产生后ADC2将立即启动转换，ADC1 在延迟 14个 ADC时钟周期后启动转换，再次延时 14个ADC时钟周期后ADC2再次启动，如此往复循环。若使能中断，ADC1 将产生 EOC中断，若同时使能DMA，则将产生一个 32 位的 DMA 传输请求，将数据寄存器 ADC1_RDATAR 的内容传输到 SRAM 中，高16位包含 ADC2转换数据，低16位包含ADC1转换数据。

![](images/8a51c6a875100e15b7671d3fc7151cb948dee0432d0a0200863acd68cb81d9e0.jpg)  
图12-9 单通道慢速交替转换

![](images/ea1435d5c6026e04572051f9e28650d849ead4dcc59773878274ee936214ff7f.jpg)  
注：1.采样时间应小于14个ADC时钟周期，以避免和下一次采样周期重合；

个 ADC 时钟周期后自动启动新的 ADC2 转换；  
3.不需要设置CONT位。

#### 6）交替触发模式

此模式仅适用于注入通道组，设置ADC1_CTLR2中JEXTSEL[2:0]，以选择触发源。当第一次触发事件发生时，ADC1 上所有的注入通道被转换，当第二次触发事件发生时，ADC2 上所有的注入通道被转换。依次循环往复，若使能中断，则当 ADC1 所有注入通道转换完成后产生 JEOC 中断，ADC2 所有注入通道转换完成后产生JEOC中断。

![](images/22c4f49274a58d0ef194c3dc3641c7e3f5f0b3bd811c5d7e161c70ec34e79940.jpg)  
图 12-10 每个 ADC 注入通道组交替触发转换

若同时使用注入间断模式，则当第一次触发事件发生时，ADC1 上的第一个注入通道被转换，当第二次触发事件发生时，ADC2上第一个注入通道被转换。以此类推，往复循环。同时若使能中断，则当 ADC1所有注入通道转换完成后，产生 JEOC 中断，当 ADC2所有注入通道转换完成后产生 JEOC中断。

![](images/5c99619f1e7cbd67c9de92067cb492cc2df3fc8e77248a9f5cfbf157b2e9f2f2.jpg)  
图 12-11 间断模式下每个 ADC 注入通道交替触发转换

#### 7）同步规则模式+同步注入模式

此模式下规则组的同步转换可以打断，以启动注入组的同步转换。同时此模式下需要保准转换具有相同时间长度的序列，或保证触发时间间隔比两个序列中较长序列的时间长。

#### 8）同步规则模式+交替触发模式

此模式下规则组的同步转换可以被打断，以启动注入组的交替触发转换。当发生注入事件时，交替触发转换立即启动。如果同步规则正在转换，则所有 ADC的规则转换被停止，在注入转换结束后同步恢复。

图 12-12 同步规则模式下交替触发注入通道转换  
![](images/1ee6060673d8954169f401208b845e9245e1a50348e31a4ef2bd4f373d9bc99f.jpg)  
注：此模式下需要保准转换具有相同时间长度的序列，或者保证触发时间间隔比两个序列中较长序列的时间长。

如果注入触发事件发生在中断了规则转换的注入转换期间，则这个触发事件将会被忽略，例如下图描述的第 2 次触发的情况。

![](images/cb490bc4435ded2ff2e092a53d88dbc8d70f52f678fa4c197cf3893e26196a9e.jpg)  
图12-13 触发事件发生在注入转换期间

#### 9）同步注入模式+交替模式

此模式下，注入转换可以打断交替转换。发生注入事件时，交替转换被打断，注入转换启动，在注入转换结束后，交替转换被恢复。

![](images/0205b9ee7fc7a21ab9aa0d349783f34d89b44dc0a8bc0495b5c42be05c251167.jpg)  
图 12-14 交替转换下触发注入组转换

注：当ADC的预分频系数为4时，交替转换恢复后采样间隔不再是均匀的7个ADC时钟，改为8个和6个时钟周期交替。

## 12.3 寄存器描述

表 12-5 ADC1 相关寄存器列表  

<table><tr><td>名称</td><td>访问地址</td><td>描述</td><td>复位值</td></tr><tr><td>R32_ADC1_STAT</td><td>0x40012400</td><td>ADC1 状态寄存器</td><td>0x00000000</td></tr><tr><td>R32_ADC1_CTLR1</td><td>0x40012404</td><td>ADC1 控制寄存器 1</td><td>0x00000000</td></tr><tr><td>R32_ADC1_CTLR2</td><td>0x40012408</td><td>ADC1 控制寄存器 2</td><td>0x00000000</td></tr><tr><td>R32_ADC1_SAMPTR1</td><td>0x4001240C</td><td>ADC1 采样时间配置寄存器 1</td><td>0x00000000</td></tr><tr><td>R32_ADC1_SAMPTR2</td><td>0x40012410</td><td>ADC1 采样时间配置寄存器 2</td><td>0x00000000</td></tr><tr><td>R32_ADC1_IOFR1</td><td>0x40012414</td><td>ADC1 注入通道数据偏移寄存器 1</td><td>0x00000000</td></tr><tr><td>R32_ADC1_IOFR2</td><td>0x40012418</td><td>ADC1 注入通道数据偏移寄存器 2</td><td>0x00000000</td></tr><tr><td>R32_ADC1_IOFR3</td><td>0x4001241C</td><td>ADC1 注入通道数据偏移寄存器 3</td><td>0x00000000</td></tr><tr><td>R32_ADC1_IOFR4</td><td>0x40012420</td><td>ADC1 注入通道数据偏移寄存器 4</td><td>0x00000000</td></tr><tr><td>R32_ADC1_WDHTR</td><td>0x40012424</td><td>ADC1 看门狗高阈值寄存器</td><td>0x00000000</td></tr><tr><td>R32_ADC1_WDLTR</td><td>0x40012428</td><td>ADC1 看门狗低阈值寄存器</td><td>0x00000000</td></tr><tr><td>R32_ADC1_RSQR1</td><td>0x4001242C</td><td>ADC1 规则通道序列寄存器 1</td><td>0x00000000</td></tr><tr><td>R32_ADC1_RSQR2</td><td>0x40012430</td><td>ADC1 规则通道序列寄存器 2</td><td>0x00000000</td></tr><tr><td>R32_ADC1_RSQR3</td><td>0x40012434</td><td>ADC1 规则通道序列寄存器 3</td><td>0x00000000</td></tr><tr><td>R32_ADC1_ISQR</td><td>0x40012438</td><td>ADC1 注入通道序列寄存器</td><td>0x00000000</td></tr><tr><td>R32_ADC1_IDATAR1</td><td>0x4001243C</td><td>ADC1 注入数据寄存器 1</td><td>0x00000000</td></tr><tr><td>R32_ADC1_IDATAR2</td><td>0x40012440</td><td>ADC1 注入数据寄存器 2</td><td>0x00000000</td></tr><tr><td>R32_ADC1_IDATAR3</td><td>0x40012444</td><td>ADC1 注入数据寄存器 3</td><td>0x00000000</td></tr><tr><td>R32_ADC1_IDATAR4</td><td>0x40012448</td><td>ADC1 注入数据寄存器 4</td><td>0x00000000</td></tr><tr><td>R32_ADC1_RDATAR</td><td>0x4001244C</td><td>ADC1 规则数据寄存器</td><td>0x00000000</td></tr></table>

表 12-6 ADC2 相关寄存器列表  

<table><tr><td>名称</td><td>访问地址</td><td>描述</td><td>复位值</td></tr><tr><td>R32_ADC2_STAT</td><td>0x40012800</td><td>ADC2状态寄存器</td><td>0x00000000</td></tr><tr><td>R32_ADC2_CTLR1</td><td>0x40012804</td><td>ADC2控制寄存器1</td><td>0x00000000</td></tr><tr><td>R32_ADC2_CTLR2</td><td>0x40012808</td><td>ADC2控制寄存器2</td><td>0x00000000</td></tr><tr><td>R32_ADC2_SAMPTR1</td><td>0x4001280C</td><td>ADC2采样时间配置寄存器1</td><td>0x00000000</td></tr><tr><td>R32_ADC2_SAMPTR2</td><td>0x40012810</td><td>ADC2采样时间配置寄存器2</td><td>0x00000000</td></tr><tr><td>R32_ADC2_IOFR1</td><td>0x40012814</td><td>ADC2注入通道数据偏移寄存器1</td><td>0x00000000</td></tr><tr><td>R32_ADC2_IOFR2</td><td>0x40012818</td><td>ADC2注入通道数据偏移寄存器2</td><td>0x00000000</td></tr><tr><td>R32_ADC2_IOFR3</td><td>0x4001281C</td><td>ADC2注入通道数据偏移寄存器3</td><td>0x00000000</td></tr><tr><td>R32_ADC2_IOFR4</td><td>0x40012820</td><td>ADC2注入通道数据偏移寄存器4</td><td>0x00000000</td></tr><tr><td>R32_ADC2_WDHTR</td><td>0x40012824</td><td>ADC2看门狗高阈值寄存器</td><td>0x00000000</td></tr><tr><td>R32_ADC2_WDLTR</td><td>0x40012828</td><td>ADC2看门狗低阈值寄存器</td><td>0x00000000</td></tr><tr><td>R32_ADC2_RSQR1</td><td>0x4001282C</td><td>ADC2规则通道序列寄存器1</td><td>0x00000000</td></tr><tr><td>R32_ADC2_RSQR2</td><td>0x40012830</td><td>ADC2规则通道序列寄存器2</td><td>0x00000000</td></tr><tr><td>R32_ADC2_RSQR3</td><td>0x40012834</td><td>ADC2规则通道序列寄存器3</td><td>0x00000000</td></tr><tr><td>R32_ADC2_ISQR</td><td>0x40012838</td><td>ADC2注入通道序列寄存器</td><td>0x00000000</td></tr><tr><td>R32_ADC2_IDATAR1</td><td>0x4001283C</td><td>ADC2注入数据寄存器1</td><td>0x00000000</td></tr><tr><td>R32_ADC2_IDATAR2</td><td>0x40012840</td><td>ADC2注入数据寄存器2</td><td>0x00000000</td></tr><tr><td>R32_ADC2_IDATAR3</td><td>0x40012844</td><td>ADC2注入数据寄存器3</td><td>0x00000000</td></tr><tr><td>R32_ADC2_IDATAR4</td><td>0x40012848</td><td>ADC2注入数据寄存器4</td><td>0x00000000</td></tr><tr><td>R32_ADC2_RDATAR</td><td>0x4001284C</td><td>ADC2规则数据寄存器</td><td>0x00000000</td></tr></table>

### 12.3.1 ADCx 状态寄存器（ADCx_STATR）（x=1/2）

偏移地址： $0 \times 0 0$

<table><tr><td>31</td><td>30</td><td>29</td><td>28</td><td>27</td><td>26</td><td>25</td><td>24</td><td>23</td><td>22</td><td>21</td><td>20</td><td>19</td><td>18</td><td>17</td><td>16</td></tr><tr><td colspan="16">Reserved</td></tr><tr><td>15</td><td>14</td><td>13</td><td>12</td><td>11</td><td>10</td><td>9</td><td>8</td><td>7</td><td>6</td><td>5</td><td>4</td><td>3</td><td>2</td><td>1</td><td>0</td></tr><tr><td colspan="11">Reserved</td><td>STRT</td><td>JSTRT</td><td>JEOC</td><td>EOC</td><td>AWD</td></tr></table>

<table><tr><td>位</td><td>名称</td><td>访问</td><td>描述</td><td>复位值</td></tr><tr><td>[31:5]</td><td>Reserved</td><td>R0</td><td>保留。</td><td>0</td></tr><tr><td>4</td><td>STRT</td><td>RWO</td><td>规则通道转换开始状态:1:规则通道转换已开始;0:规则通道转换未开始。该位由硬件置1,由软件清0(写1无效)。</td><td>0</td></tr><tr><td>3</td><td>JSTRT</td><td>RWO</td><td>注入通道转换开始状态:</td><td>0</td></tr><tr><td></td><td></td><td></td><td>1: 注入通道转换已开始;0: 注入通道转换未开始。该位由硬件置1,由软件清0(写1无效)。</td><td></td></tr><tr><td>2</td><td>JEOC</td><td>RWO</td><td>注入通道组转换结束状态:1: 转换完成;0: 转换未完成。该位由硬件置1(所有注入通道转换完),由软件清0(写1无效)。</td><td>0</td></tr><tr><td>1</td><td>EOC</td><td>RWO</td><td>转换结束状态:1: 转换完成;0: 转换未完成。该位由硬件置1(规则或注入通道组转换结束),由软件清0(写1无效)或读ADC_RDATAR时清除。</td><td>0</td></tr><tr><td>0</td><td>AWD</td><td>RWO</td><td>模拟看门狗标志位:1: 发生模拟看门狗事件;0: 没有发生模拟看门狗事件。该位由硬件置1(转换值超出ADC_WDHTR和ADC_WDLTR寄存器范围),由软件清0(写1无效)。</td><td>0</td></tr></table>

### 12.3.2 ADCx 控制寄存器 1（ADCx_CTLR1）（x=1/2）

偏移地址： $0 \times 0 4$

<table><tr><td>Reserved</td><td colspan="2">PGA[1:0]</td><td>BUF
EN</td><td>TKI
TUNE</td><td>TKENABLE</td><td>AWDEN</td><td>JAWDEN</td><td colspan="2">Reserved</td><td colspan="2">DUALMOD[3:0]</td></tr><tr><td colspan="12">15 14 13 12 11 10 9 8 7 6 5 4 3 2 1 0</td></tr><tr><td>DISCNUM[2:0]</td><td>JDISC
EN</td><td>DISC
EN</td><td>JAUTO</td><td>AWD
SGL</td><td>SCAN</td><td>JEOC
IE</td><td>AWDIE</td><td>EOC
IE</td><td colspan="3">AWDCH[4:0]</td></tr></table>

<table><tr><td>位</td><td>名称</td><td>访问</td><td>描述</td><td>复位值</td></tr><tr><td>[31:29]</td><td>Reserved</td><td>R0</td><td>保留。</td><td>0</td></tr><tr><td>[28:27]</td><td>PGA[1:0]</td><td>RW</td><td>ADC通道增益配置00: x101: x410: x1611: x64注:输入增益可调,可实现小信号放大采样。使用此功能需开启ADC Buffer。</td><td>00b</td></tr><tr><td>26</td><td>BUFEN</td><td>RW</td><td>ADC BUFFER使能0: 关闭输入Buffer1: 使能输入Buffer注:需要开启buffer的情况:1. 当TKENABLE位或TSVREFE位置1, buffer默认开启不可关闭(故在这两种情况下,ADC校准需在TKENABLE位或TSVREFE位置1之前,且buffer需要关闭)。</td><td>0</td></tr><tr><td></td><td></td><td></td><td>2.当外部输入阻抗大于最大输入阻抗要求,可开启buffer以改善ADC采集数据(此时ADC的采样时间不建议小于7.5T),外部输入阻抗详见CH32F203DS0/CH32F208DS0/CH32V203DS0/CH32V208DS0数据手册表4-28 CH32V307DS0/CH32F207DS0数据手册表4-41。具体操作可参考EVT相关例程。</td><td></td></tr><tr><td>25</td><td>TKITUNE</td><td>RW</td><td>TKEY模块充电电流配置0:充电电流为35uA1:充电电流减半</td><td>0</td></tr><tr><td>24</td><td>TKENABLE</td><td>RW</td><td>TKEY模块使能控制,包括TKEY_F和TKEY_V单元:1:开启TKEY模块;0:关闭TKEY模块。</td><td>0</td></tr><tr><td>23</td><td>AWDEN</td><td>RW</td><td>在规则通道上模拟看门狗功能使能位:1:规则通道上使能模拟看门狗;0:规则通道上关闭模拟看门狗。</td><td>0</td></tr><tr><td>22</td><td>JAWDEN</td><td>RW</td><td>在注入通道上模拟看门狗功能使能位:1:注入通道上使能模拟看门狗;0:注入通道上关闭模拟看门狗。</td><td>0</td></tr><tr><td>[21:20]</td><td>Reserved</td><td>RO</td><td>保留。</td><td>0</td></tr><tr><td>[19:16]</td><td>DUALMOD[3:0]</td><td>RW</td><td>双重模式选择。0000:独立模式0001:同步规则+同步注入模式0010:同步规则+交替触发模式0011:同步注入+快速交替模式0100:同步注入+慢速交替模式0101:同步注入模式0110:同步规则模式0111:快速交替模式1000:慢速交替模式1001:交替触发模式注:ADC2中这些位为保留位,任何配置位的修改应在双重模式关闭的情况下进行。</td><td>0000b</td></tr><tr><td>[15:13]</td><td>DISCNUM[2:0]</td><td>RW</td><td>间断模式下,外部触发后要转换的规则通道数目:000:1个通道;...111:8个通道。</td><td>000b</td></tr><tr><td>12</td><td>JDISCEN</td><td>RW</td><td>注入通道上的间断模式使能位:1:使能注入通道上的间断模式;0:关闭注入通道上的间断模式。</td><td>0</td></tr><tr><td>11</td><td>DISCEN</td><td>RW</td><td>规则通道上的间断模式使能位:1:使能规则通道上的间断模式;0:关闭规则通道上的间断模式。</td><td>0</td></tr><tr><td>10</td><td>JAUTO</td><td>RW</td><td>开启规则通道完成后,自动转换注入通道组使能位:1:使能自动的注入通道组转换;0:关闭自动的注入通道组转换。注:此模式需要禁止注入通道的外部触发功能。</td><td>0</td></tr><tr><td>9</td><td>AWDSGL</td><td>RW</td><td>扫描模式下,在单一通道上使用模拟看门狗使能位:1:在单一通道上使用模拟看门狗(AWDCH[4:0]选择);0:在所有通道上使用模拟看门狗。</td><td>0</td></tr><tr><td>8</td><td>SCAN</td><td>RW</td><td>扫描模式使能位:1:使能扫描模式(连续转换ADC_IORx和ADC_RSQRx选择的所有通道);0:关闭扫描模式。</td><td>0</td></tr><tr><td>7</td><td>JEOCIE</td><td>RW</td><td>注入通道组转换结束中断使能位:1:使能注入通道组转换完成中断(JEOC标志);0:关闭注入通道组转换完成中断。</td><td>0</td></tr><tr><td>6</td><td>AWDIE</td><td>RW</td><td>模拟看门狗中断使能位:1:使能模拟看门狗中断;0:关闭模拟看门狗中断。注:在扫描模式下,如果发生此中断将中止扫描。</td><td>0</td></tr><tr><td>5</td><td>EOCIE</td><td>RW</td><td>转换结束(规则或注入通道组)中断使能位:1:使能转换结束中断(EOC标志);0:关闭转换结束中断。</td><td>0</td></tr><tr><td>[4:0]</td><td>AWDCH[4:0]</td><td>RW</td><td>模拟看门狗通道选择位:00000:模拟输入通道0;00001:模拟输入通道1;...10001:模拟输入通道17。</td><td>00000b</td></tr></table>

### 12.3.3 ADCx 控制寄存器 2（ADCx_CTLR2）（x=1/2）

偏移地址： $0 \times 0 8$

<table><tr><td>31</td><td>30</td><td>29</td><td>28</td><td>27</td><td>26</td><td>25</td><td>24</td><td>23</td><td>22</td><td>21</td><td>20</td><td>19</td><td>18</td><td>17</td><td>16</td></tr><tr><td colspan="8">Reserved</td><td>TS
VREFE</td><td>SW
START</td><td>JSW
START</td><td>EXT
TRIG</td><td colspan="3">EXTSEL[2:0]</td><td>Reser
ved</td></tr><tr><td>15</td><td>14</td><td>13</td><td>12</td><td>11</td><td>10</td><td>9</td><td>8</td><td>7</td><td>6</td><td>5</td><td>4</td><td>3</td><td>2</td><td>1</td><td>0</td></tr><tr><td>JEXT
TRIG</td><td colspan="2">JEXTSEL[2:0]</td><td>ALIGN</td><td>Reserved</td><td colspan="2">DMA</td><td colspan="5">Reserved</td><td>RST
CAL</td><td>CAL</td><td>CONT</td><td>ADON</td></tr></table>

<table><tr><td>位</td><td>名称</td><td>访问</td><td>描述</td><td>复位值</td></tr><tr><td>[31:24]</td><td>Reserved</td><td>RO</td><td>保留。</td><td>0</td></tr><tr><td>23</td><td>TSVREFE</td><td>RW</td><td>温度传感器和内部电压（VREFINT）通道使能位：1：使能温度传感器和VREFINT通道；0：禁止温度传感器和VREFINT通道。注：该位仅适用于ADC1。</td><td>0</td></tr><tr><td>22</td><td>SWSTART</td><td>RW</td><td>启动一个规则通道转换，需要设置软件触发：1：启动规则通道转换；0：复位状态。此位由软件置位，转换开始后硬件清0。</td><td>0</td></tr><tr><td>21</td><td>JSWSTART</td><td>RW</td><td>启动一个注入通道转换，需要设置软件触发：1：启动注入通道转换；</td><td>0</td></tr><tr><td></td><td></td><td></td><td>0:复位状态。此位由软件置位,转换开始后硬件清0或者软件清0。</td><td></td></tr><tr><td>20</td><td>EXTTRIG</td><td>RW</td><td>规则通道的外部触发转换模式使能:1:使用外部事件启动转换;0:关闭外部事件启动功能。</td><td>0</td></tr><tr><td>[19:17]</td><td>EXTSEL[2:0]</td><td>RW</td><td>启动规则通道转换的外部触发事件选择:000:定时器1的CC1事件;001:定时器1的CC2事件;010:定时器1的CC3事件;011:定时器2的CC2事件;100:定时器3的TRGO事件;101:定时器4的CC4事件;110:EXTI线11/定时器8的TRGO事件;111:SWSTART软件触发。注:仅大容量产品中具有定时器8的TRGO事件</td><td>000b</td></tr><tr><td>16</td><td>Reserved</td><td>RO</td><td>保留。</td><td>0</td></tr><tr><td>15</td><td>JEXTTRIG</td><td>RW</td><td>注入通道的外部触发转换模式使能:1:使用外部事件启动转换;0:关闭外部事件启动功能。</td><td>0</td></tr><tr><td>[14:12]</td><td>JEXTSEL[2:0]</td><td>RW</td><td>启动注入通道转换的外部触发事件选择:000:定时器1的TRGO事件;001:定时器1的CC4事件;010:定时器2的TRGO事件;011:定时器2的CC1事件;100:定时器3的CC4事件;101:定时器4的TRGO事件;110:EXTI线15/定时器8的CC4事件;111:JSWSTART软件触发。注:仅大容量产品中具有定时器8的CC4事件</td><td>000b</td></tr><tr><td>11</td><td>ALIGN</td><td>RW</td><td>数据对齐方式:1:左对齐;0:右对齐。</td><td>0</td></tr><tr><td>[10:9]</td><td>Reserved</td><td>RO</td><td>保留。</td><td>0</td></tr><tr><td>8</td><td>DMA</td><td>RW</td><td>直接存储访问(DMA)模式使能:1:使能DMA模式;0:关闭DMA模式。</td><td>0</td></tr><tr><td>[7:4]</td><td>Reserved</td><td>RO</td><td>保留。</td><td>0</td></tr><tr><td>3</td><td>RSTCAL</td><td>RW</td><td>复位校准,此位由软件置位,复位完成后由硬件清0:1:初始化校准寄存器;0:校准寄存器已初始化。注:如果正在进行转换时设置RSTCAL,清除校准寄存器需要额外的周期。</td><td>0</td></tr><tr><td>2</td><td>CAL</td><td>RW</td><td>A/D校准,该位由软件置位,校准结束时由硬件清0。1:开始校准;0:校准完成。</td><td>0</td></tr><tr><td>1</td><td>CONT</td><td>RW</td><td>连续转换使能:1:连续转换模式;</td><td>0</td></tr><tr><td></td><td></td><td></td><td>0:单次转换模式。
如果设置了此位,则转换将连续进行直到该位被清除。</td><td></td></tr><tr><td>0</td><td>ADON</td><td>RW</td><td>开/关A/D转换器
当该位为0时,写入1将把ADC从断电模式下唤醒;
当该位为1时,写入1将启动转换。
1:开启ADC并启动转换;
0:关闭ADC转换/校准,并进入断电模式。
注:当寄存器只有ADON改变时,才会启动一次转换,如果还有其他任意位发送变化,则不会启动新的转换。</td><td>0</td></tr></table>

### 12.3.4 ADCx 采样时间配置寄存器 1（ADCx_SAMPTR1）（x=1/2）

偏移地址： $0 \times 0 0$

<table><tr><td>31</td><td>30</td><td>29</td><td>28</td><td>27</td><td>26</td><td>25</td><td>24</td><td>23</td><td>22</td><td>21</td><td>20</td><td>19</td><td>18</td><td>17</td><td>16</td></tr><tr><td colspan="8">Reserved</td><td colspan="3">SMP17[2:0]</td><td colspan="3">SMP16[2:0]</td><td colspan="2">SMP15[2:1]</td></tr><tr><td>15</td><td>14</td><td>13</td><td>12</td><td>11</td><td>10</td><td>9</td><td>8</td><td>7</td><td>6</td><td>5</td><td>4</td><td>3</td><td>2</td><td>1</td><td>0</td></tr><tr><td>SMP15[0]</td><td colspan="3">SMP14[2:0]</td><td colspan="3">SMP13[2:0]</td><td colspan="3">SMP12[2:0]</td><td colspan="3">SMP11[2:0]</td><td colspan="3">SMP10[2:0]</td></tr></table>

<table><tr><td>位</td><td>名称</td><td>访问</td><td>描述</td><td>复位值</td></tr><tr><td>[31:24]</td><td>Reserved</td><td>R0</td><td>保留。</td><td>0</td></tr><tr><td>[23:0]</td><td>SMPx[2:0]</td><td>RW</td><td>SMPx[2:0]:通道x的采样时间配置:000:1.5周期; 001:7.5周期;010:13.5周期; 011:28.5周期;100:41.5周期; 101:55.5周期;110:71.5周期; 111:239.5周期;这些位用于独立地选择每个通道的采样时间,在采样周期中通道配置值必须保持不变。</td><td>000b</td></tr></table>

### 12.3.5 ADCx 采样时间配置寄存器 2（ADCx_SAMPTR2）（x=1/2）

偏移地址： $0 \times 1 0$

<table><tr><td>31</td><td>30</td><td>29</td><td>28</td><td>27</td><td>26</td><td>25</td><td>24</td><td>23</td><td>22</td><td>21</td><td>20</td><td>19</td><td>18</td><td>17</td><td>16</td></tr><tr><td colspan="2">Reserved</td><td colspan="3">SMP9[2:0]</td><td colspan="3">SMP8[2:0]</td><td colspan="3">SMP7[2:0]</td><td colspan="3">SMP6[2:0]</td><td colspan="2">SMP5[2:1]</td></tr><tr><td>15</td><td>14</td><td>13</td><td>12</td><td>11</td><td>10</td><td>9</td><td>8</td><td>7</td><td>6</td><td>5</td><td>4</td><td>3</td><td>2</td><td>1</td><td>0</td></tr><tr><td>SMP5[0]</td><td colspan="3">SMP4[2:0]</td><td colspan="3">SMP3[2:0]</td><td colspan="3">SMP2[2:0]</td><td colspan="3">SMP1[2:0]</td><td colspan="3">SMP0[2:0]</td></tr></table>

<table><tr><td>位</td><td>名称</td><td>访问</td><td>描述</td><td>复位值</td></tr><tr><td>[31:30]</td><td>Reserved</td><td>R0</td><td>保留。</td><td>0</td></tr><tr><td>[29:0]</td><td>SMPx[2:0]</td><td>RW</td><td>SMPx[2:0]:通道x的采样时间配置:000:1.5周期; 001:7.5周期;010:13.5周期; 011:28.5周期;100:41.5周期; 101:55.5周期;110:71.5周期; 111:239.5周期;这些位用于独立地选择每个通道的采样时间,在采样周期中通道配置值必须保持不变。</td><td>000b</td></tr></table>

### 12.3.6 ADCy 注入通道数据偏移寄存器 $\pmb { \times }$ （ADCy_IOFRx）(y=1/2；x=1/2/3/4)

偏移地址： $0 \times 1 4 + ( \times - 1 ) \times 4$

<table><tr><td>31</td><td>30</td><td>29</td><td>28</td><td>27</td><td>26</td><td>25</td><td>24</td><td>23</td><td>22</td><td>21</td><td>20</td><td>19</td><td>18</td><td>17</td><td>16</td></tr><tr><td colspan="16">Reserved</td></tr><tr><td>15</td><td>14</td><td>13</td><td>12</td><td>11</td><td>10</td><td>9</td><td>8</td><td>7</td><td>6</td><td>5</td><td>4</td><td>3</td><td>2</td><td>1</td><td>0</td></tr><tr><td colspan="4">Reserved</td><td colspan="12">J0FFSETx[11:0]</td></tr></table>

<table><tr><td>位</td><td>名称</td><td>访问</td><td>描述</td><td>复位值</td></tr><tr><td>[31:12]</td><td>Reserved</td><td>R0</td><td>保留。</td><td>0</td></tr><tr><td>[11:0]</td><td>J0FFSETx[11:0]</td><td>RW</td><td>注入通道x的数据偏移值。转换注入通道时，这个值定义了用于从原始转换数据中减去的数值。转换的结果可以在ADC_IDATARx寄存器中读出。</td><td>0</td></tr></table>

### 12.3.7 ADCx 看门狗高阈值寄存器（ADCx_WDHTR）（x=1/2）

偏移地址： ${ 0 } { \times 2 4 }$

<table><tr><td>31</td><td>30</td><td>29</td><td>28</td><td>27</td><td>26</td><td>25</td><td>24</td><td>23</td><td>22</td><td>21</td><td>20</td><td>19</td><td>18</td><td>17</td><td>16</td></tr><tr><td colspan="16">Reserved</td></tr><tr><td>15</td><td>14</td><td>13</td><td>12</td><td>11</td><td>10</td><td>9</td><td>8</td><td>7</td><td>6</td><td>5</td><td>4</td><td>3</td><td>2</td><td>1</td><td>0</td></tr><tr><td colspan="4">Reserved</td><td colspan="12">HT[11:0]</td></tr></table>

<table><tr><td>位</td><td>名称</td><td>访问</td><td>描述</td><td>复位值</td></tr><tr><td>[31:12]</td><td>Reserved</td><td>R0</td><td>保留。</td><td>0</td></tr><tr><td>[11:0]</td><td>HT[11:0]</td><td>RW</td><td>模拟看门狗高阈值设置值。</td><td>0</td></tr></table>

注：可以在转换过程中更改WDHTR和WDLTR的值，但它们将在下次转换时生效。

### 12.3.8 ADCx 看门狗低阈值寄存器（ADCx_WDLTR）（x=1/2）

偏移地址： $_ { 0 \times 2 8 }$

<table><tr><td>31</td><td>30</td><td>29</td><td>28</td><td>27</td><td>26</td><td>25</td><td>24</td><td>23</td><td>22</td><td>21</td><td>20</td><td>19</td><td>18</td><td>17</td><td>16</td></tr><tr><td colspan="16">Reserved</td></tr><tr><td>15</td><td>14</td><td>13</td><td>12</td><td>11</td><td>10</td><td>9</td><td>8</td><td>7</td><td>6</td><td>5</td><td>4</td><td>3</td><td>2</td><td>1</td><td>0</td></tr><tr><td colspan="4">Reserved</td><td colspan="12">LT[11:0]</td></tr></table>

<table><tr><td>位</td><td>名称</td><td>访问</td><td>描述</td><td>复位值</td></tr><tr><td>[31:12]</td><td>Reserved</td><td>R0</td><td>保留。</td><td>0</td></tr><tr><td>[11:0]</td><td>LT[11:0]</td><td>RW</td><td>模拟看门狗低阈值设置值。</td><td>0</td></tr></table>

注：可以在转换过程中更改WDHTR和WDLTR的值，但它们将在下次转换时生效。

### 12.3.9 ADCx 规则通道序列寄存器 1（ADCx_RSQR1）（x=1/2）

偏移地址： $\mathtt { 0 } \mathtt { x } 2 0$

<table><tr><td>31</td><td>30</td><td>29</td><td>28</td><td>27</td><td>26</td><td>25</td><td>24</td><td>23</td><td>22</td><td>21</td><td>20</td><td>19</td><td>18</td><td>17</td><td>16</td></tr><tr><td colspan="8">Reserved</td><td colspan="4">L[3:0]</td><td colspan="4">RSQ16[4:1]</td></tr><tr><td>15</td><td>14</td><td>13</td><td>12</td><td>11</td><td>10</td><td>9</td><td>8</td><td>7</td><td>6</td><td>5</td><td>4</td><td>3</td><td>2</td><td>1</td><td>0</td></tr><tr><td>SQ16[0]</td><td colspan="5">SQ15[4:0]</td><td colspan="5">SQ14[4:0]</td><td colspan="5">SQ13[4:0]</td></tr></table>

<table><tr><td>位</td><td>名称</td><td>访问</td><td>描述</td><td>复位值</td></tr><tr><td>[31:24]</td><td>Reserved</td><td>R0</td><td>保留。</td><td>0</td></tr><tr><td>[23:20]</td><td>L[3:0]</td><td>RW</td><td>规则通道转换序列中需要转换的通道数目:0000-1111:1-16个转换。</td><td>0</td></tr><tr><td>[19:15]</td><td>SQ16[4:0]</td><td>RW</td><td>规则序列中的第16个转换通道的编号(0-17)。</td><td>0</td></tr><tr><td>[14:10]</td><td>SQ15[4:0]</td><td>RW</td><td>规则序列中的第15个转换通道的编号(0-17)。</td><td>0</td></tr><tr><td>[9:5]</td><td>SQ14[4:0]</td><td>RW</td><td>规则序列中的第14个转换通道的编号(0-17)。</td><td>0</td></tr><tr><td>[4:0]</td><td>SQ13[4:0]</td><td>RW</td><td>规则序列中的第13个转换通道的编号(0-17)。</td><td>0</td></tr></table>

### 12.3.10 $\pmb { \mathsf { A D C x } }$ 规则通道序列寄存器 2（ADCx_RSQR2）（x=1/2）

偏移地址： $\mathtt { 0 } { \times } 3 0$

<table><tr><td>31</td><td>30</td><td>29</td><td>28</td><td>27</td><td>26</td><td>25</td><td>24</td><td>23</td><td>22</td><td>21</td><td>20</td><td>19</td><td>18</td><td>17</td><td>16</td></tr><tr><td colspan="2">Reserved</td><td colspan="5">SQ12[4:0]</td><td colspan="5">SQ11[4:0]</td><td colspan="4">SQ10[4:1]</td></tr><tr><td>15</td><td>14</td><td>13</td><td>12</td><td>11</td><td>10</td><td>9</td><td>8</td><td>7</td><td>6</td><td>5</td><td>4</td><td>3</td><td>2</td><td>1</td><td>0</td></tr><tr><td>SQ10[0]</td><td colspan="5">SQ9[4:0]</td><td colspan="5">SQ8[4:0]</td><td colspan="5">SQ7[4:0]</td></tr></table>

<table><tr><td>位</td><td>名称</td><td>访问</td><td>描述</td><td>复位值</td></tr><tr><td>[31:30]</td><td>Reserved</td><td>R0</td><td>保留。</td><td>0</td></tr><tr><td>[29:25]</td><td>SQ12[4:0]</td><td>RW</td><td>规则序列中的第12个转换通道的编号（0-17）。</td><td>0</td></tr><tr><td>[24:20]</td><td>SQ11[4:0]</td><td>RW</td><td>规则序列中的第11个转换通道的编号（0-17）。</td><td>0</td></tr><tr><td>[19:15]</td><td>SQ10[4:0]</td><td>RW</td><td>规则序列中的第10个转换通道的编号（0-17）。</td><td>0</td></tr><tr><td>[14:10]</td><td>SQ9[4:0]</td><td>RW</td><td>规则序列中的第9个转换通道的编号（0-17）。</td><td>0</td></tr><tr><td>[9:5]</td><td>SQ8[4:0]</td><td>RW</td><td>规则序列中的第8个转换通道的编号（0-17）。</td><td>0</td></tr><tr><td>[4:0]</td><td>SQ7[4:0]</td><td>RW</td><td>规则序列中的第7个转换通道的编号（0-17）。</td><td>0</td></tr></table>

### 12.3.11 ADCx 规则通道序列寄存器 3（ADCx_RSQR3）（x=1/2）

偏移地址： $0 \times 3 4$

<table><tr><td>31</td><td>30</td><td>29</td><td>28</td><td>27</td><td>26</td><td>25</td><td>24</td><td>23</td><td>22</td><td>21</td><td>20</td><td>19</td><td>18</td><td>17</td><td>16</td></tr><tr><td colspan="2">Reserved</td><td colspan="5">SQ6[4:0]</td><td colspan="5">SQ5[4:0]</td><td colspan="4">SQ4[4:1]</td></tr><tr><td>15</td><td>14</td><td>13</td><td>12</td><td>11</td><td>10</td><td>9</td><td>8</td><td>7</td><td>6</td><td>5</td><td>4</td><td>3</td><td>2</td><td>1</td><td>0</td></tr><tr><td>SQ4[0]</td><td colspan="5">SQ3[4:0]</td><td colspan="5">SQ2[4:0]</td><td colspan="5">SQ1[4:0]</td></tr></table>

<table><tr><td>位</td><td>名称</td><td>访问</td><td>描述</td><td>复位值</td></tr><tr><td>[31:30]</td><td>Reserved</td><td>R0</td><td>保留。</td><td>0</td></tr><tr><td>[29:25]</td><td>SQ6[4:0]</td><td>RW</td><td>规则序列中的第6个转换通道的编号（0-17）。</td><td>0</td></tr><tr><td>[24:20]</td><td>SQ5[4:0]</td><td>RW</td><td>规则序列中的第5个转换通道的编号（0-17）。</td><td>0</td></tr><tr><td>[19:15]</td><td>SQ4[4:0]</td><td>RW</td><td>规则序列中的第4个转换通道的编号（0-17）。</td><td>0</td></tr><tr><td>[14:10]</td><td>SQ3[4:0]</td><td>RW</td><td>规则序列中的第3个转换通道的编号（0-17）。</td><td>0</td></tr><tr><td>[9:5]</td><td>SQ2[4:0]</td><td>RW</td><td>规则序列中的第2个转换通道的编号（0-17）。</td><td>0</td></tr><tr><td>[4:0]</td><td>SQ1[4:0]</td><td>RW</td><td>规则序列中的第1个转换通道的编号（0-17）。</td><td>0</td></tr></table>

### 12.3.12 ADCx 注入通道序列寄存器（ADCx_ISQR）（x=1/2）

偏移地址： ${ 0 } { \times } \ 3 { 8 }$

<table><tr><td>31</td><td>30</td><td>29</td><td>28</td><td>27</td><td>26</td><td>25</td><td>24</td><td>23</td><td>22</td><td>21</td><td>20</td><td>19</td><td>18</td><td>17</td><td>16</td></tr><tr><td colspan="10">Reserved</td><td colspan="2">JL[1:0]</td><td colspan="4">JSQ4[4:1]</td></tr><tr><td>15</td><td>14</td><td>13</td><td>12</td><td>11</td><td>10</td><td>9</td><td>8</td><td>7</td><td>6</td><td>5</td><td>4</td><td>3</td><td>2</td><td>1</td><td>0</td></tr><tr><td>JSQ4[0]</td><td colspan="5">JSQ3[4:0]</td><td colspan="5">JSQ2[4:0]</td><td colspan="5">JSQ1[4:0]</td></tr></table>

<table><tr><td>位</td><td>名称</td><td>访问</td><td>描述</td><td>复位值</td></tr><tr><td>[31:22]</td><td>Reserved</td><td>R0</td><td>保留。</td><td>0</td></tr><tr><td>[21:20]</td><td>JL[1:0]</td><td>RW</td><td>注入通道转换序列中需要转换的通道数目:00-11:1-4个转换。</td><td>0</td></tr><tr><td>[19:15]</td><td>JSQ4[4:0]</td><td>RW</td><td>注入序列中的第4个转换通道的编号(0-17)。</td><td>0</td></tr><tr><td>[14:10]</td><td>JSQ3[4:0]</td><td>RW</td><td>注入序列中的第3个转换通道的编号(0-17)。</td><td>0</td></tr><tr><td>[9:5]</td><td>JSQ2[4:0]</td><td>RW</td><td>注入序列中的第2个转换通道的编号(0-17)。</td><td>0</td></tr><tr><td>[4:0]</td><td>JSQ1[4:0]</td><td>RW</td><td>注入序列中的第1个转换通道的编号(0-17)。</td><td>0</td></tr></table>

注：不同于规则转换序列，如果ILEN[1:0]的长度小于4，则转换的序列顺序是从(4-ILEN)开始。

### 12.3.13 ADCy 注入数据寄存器 $\pmb { \times }$ （ADCy_IDATARx）（y=1/2；x=1/2/3/4）

偏移地址： $0 \times 3 0 + ( x - 1 ) \times 4$

<table><tr><td>31</td><td>30</td><td>29</td><td>28</td><td>27</td><td>26</td><td>25</td><td>24</td><td>23</td><td>22</td><td>21</td><td>20</td><td>19</td><td>18</td><td>17</td><td>16</td></tr><tr><td colspan="16">Reserved</td></tr><tr><td>15</td><td>14</td><td>13</td><td>12</td><td>11</td><td>10</td><td>9</td><td>8</td><td>7</td><td>6</td><td>5</td><td>4</td><td>3</td><td>2</td><td>1</td><td>0</td></tr><tr><td colspan="16">JDATA[15:0]</td></tr></table>

<table><tr><td>位</td><td>名称</td><td>访问</td><td>描述</td><td>复位值</td></tr><tr><td>[31:16]</td><td>Reserved</td><td>R0</td><td>保留。</td><td>0</td></tr><tr><td>[15:0]</td><td>JDATA[15:0]</td><td>R0</td><td>注入通道转换数据（数据左对齐或右对齐）。</td><td>0</td></tr></table>

### 12.3.14 $\pmb { \mathsf { A D C x } }$ 规则数据寄存器（ADCx_RDATAR）（x=1/2）

偏移地址： $\mathtt { 0 } \mathtt { x } 4 0$

<table><tr><td>31</td><td>30</td><td>29</td><td>28</td><td>27</td><td>26</td><td>25</td><td>24</td><td>23</td><td>22</td><td>21</td><td>20</td><td>19</td><td>18</td><td>17</td><td>16</td></tr><tr><td colspan="16">ADC2DATA[15:0]</td></tr><tr><td>15</td><td>14</td><td>13</td><td>12</td><td>11</td><td>10</td><td>9</td><td>8</td><td>7</td><td>6</td><td>5</td><td>4</td><td>3</td><td>2</td><td>1</td><td>0</td></tr><tr><td colspan="16">DATA[4:0]</td></tr></table>

<table><tr><td>位</td><td>名称</td><td>访问</td><td>描述</td><td>复位值</td></tr><tr><td>[31:16]</td><td>ADC2DATA[15:0]</td><td>R0</td><td>ADC2转换的数据:在ADC1中:双模式下,这些位包含了ADC2转换的规则通道数据。在ADC2中:不使用这些位。</td><td>0</td></tr><tr><td>[15:0]</td><td>DATA[4:0]</td><td>R0</td><td>规则通道转换数据(数据左对齐或右对齐)。</td><td>0</td></tr></table>

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

# 第 14 章 高级定时器（ADTM）

本章模块描述适用于CH32F2x、CH32V2x和CH32V3x微控制器全系列产品。

高级定时器模块包含一个功能强大的 16 位自动重装定时器（TIM1、TIM8、TIM9 和 TIM10），可用于测量脉冲宽度或产生脉冲、PWM波等。用于电机控制、电源等领域。

## 14.1 主要特征

高级定时器（TIM1/8/9/10）的主要特征包括：

16 位自动重装计数器，支持增计数模式，减计数模式和增减计数模式；  
16 位预分频器，分频系数从 $1 \sim 6 5 5 3 6$ 之间动态可调；  
支持四路独立的比较捕获通道；  
$\bullet$ 每路比较捕获通道支持多种工作模式，比如：输入捕获，输出比较，PWM 生成和单脉冲输出；  
$\bullet$ 支持可编程死区时间的互补输出；  
$\bullet$ 支持外部信号控制定时器；  
$\bullet$ 支持使用重复计数器在确定周期后更新定时器；  
$\bullet$ 支持使用刹车信号将定时器复位或置其于确定状态；  
$\bullet$ 支持在多种模式下使用 DMA；  
$\bullet$ 支持增量式编码器；  
支持定时器之间的级联和同步

## 14.2 原理和结构

本节主要论述高级定时器的内部构造。

### 14.2.1 概述

如图14-1，高级定时器的结构大致可以分为三部分，即输入时钟部分，核心计数器部分和比较捕获通道部分。

高级定时器的时钟可以来自于APB总线时钟（CK_INT），可以来自外部时钟输入引脚（TIMx_ETR），亦可以来自于其他具有时钟输出功能的定时器（ITRx），还可以来自于比较捕获通道的输入端（TIMx_CHx）。这些输入的时钟信号经过各种设定的滤波分频等操作后成为 CK_PSC 时钟，输出给核心计数器部分。另外，这些复杂的时钟来源还可以作为 TRGO 输出给其他的定时器、ADC 和 DAC 等外设。

高级定时器的核心是一个16位计数器（CNT）。CK_PSC经过预分频器（PSC）分频后，成为 CK_CNT并输出给CNT，CNT支持增计数模式、减计数模式和增减计数模式，并有一个自动重装值寄存器（ATRLR）在每个计数周期结束后为CNT重装载初始值。另外还有个辅助计数器在一旁计数 ATRLR为 CNT重装载初值的次数，当次数达到重复计数值寄存器（RPTCR）里设置的次数时，可以产生特定事件。

高级定时器拥有四组比较捕获通道，每组比较捕获通道都可以从专属的引脚上输入脉冲，也可以向引脚输出波形，即比较捕获通道支持输入和输出模式。比较捕获寄存器每个通道的输入都支持滤波、分频和边沿检测等操作，并支持通道间的互触发，还能为核心计数器 CNT提供时钟。每个比较捕获通道都拥有一组比较捕获寄存器（CHxCVR），支持与主计数器（CNT）进行比较而输出脉冲。

![](images/9d7c7d99488e1c445ddafa64fbc03d19a01032056c6e4890c8e928c238377f77.jpg)  
图14-1 高级定时器的结构框图

### 14.2.2 时钟输入

![](images/74826bc4cdf2506a552e0bc40d28584b06784bf48b83eb6450a5d687c630d937.jpg)  
图 14-2 高级定时器的 CK_PSC 来源框图

高级定时器 CK_PSC 的时钟来源很多，可以分为 4 类：

1） 外部时钟引脚（ETR）输入时钟的路线：ETR ETRP→ETRF；  
2） 内部 APB 时钟输入路线：CK_INT；  
3） 来自比较捕获通道引脚（TIMx_CHx）的路线：TIMx_CHx→TIx→TIxFPx，此路线也用于编码器模式；  
4） 来自内部其他定时器的输入：ITRx；

通过决定 CK_PSC 来源的 SMS 的输入脉冲选择可以将实际的操作分为 4 类：

1） 选择内部时钟源（CK_INT）；  
2） 外部时钟源模式1；  
3） 外部时钟源模式 2；  
4） 编码器模式；

上文提到的 4 种时钟源来源都可通过这 4 种操作选定。

#### 14.2.2.1 内部时钟源（CK_INT）

如果将 SMS 域保持 000b 时启动高级定时器，那么就是选定内部时钟源（CK_INT）为时钟。此时CK_INT 就是 CK_PSC。

#### 14.2.2.2 外部时钟源模式 1

如果将 SMS 域设置为 111b 时，就会启用外部时钟源模式 1。启用外部时钟源 1 时，TRGI 被选定为CK_PSC的来源，值得注意的，还需要通过配置 TS域来选择 TRGI的来源。TS域可选择以下几种脉冲作为时钟来源：

1） 内部触发（ITRx，x 为 0,1,2,3）；  
2） 比较捕获通道 1 经过边缘检测器后的信号（TI1F_ED）；  
3） 比较捕获通道的信号 TI1FP1、TI2FP2；  
4） 来自外部时钟引脚输入的信号 ETRF。

#### 14.2.2.3 外部时钟源模式 2

使用外部触发模式2能在外部时钟引脚输入的每一个上升沿或下降沿计数。将 ECE位置位时，将使用外部时钟源模式2。使用外部时钟源模式 2时，ETRF 被选定为 CK_PSC。ETR引脚经过可选的反相

器（ETP），分频器（ETPS）后成为 ETRP，再经过滤波器（ETF）后即成为 ETRF。

在 ECE 位置位且将 SMS 设为 111b 时，相当于 TS 选择 ETRF 为输入。

#### 14.2.2.4 编码器模式

将 SMS 置为 001b，010b，011b 将会启用编码器模式。启用编码器模式可以选择在 TI1FP1 和 TI2FP2中某一个特定的电平下以另一个跳变沿作为信号进行信号输出。此模式用于外接编码器使用的情况下。具体功能参考14.3.9节。

### 14.2.3 计数器和周边

CK_PSC输入给预分频器（PSC）进行分频。PSC是 16位的，实际的分频系数相当于 R16_TIMx_PSC的值 $+ 1$ 。CK_PSC 经过 PSC 会成为 CK_INT。更改 R16_TIM1_PSC 的值并不会实时生效，而会在更新事件后更新给PSC。更新事件包括UG位清零和复位。定时器的核心是一个 16位计数器（CNT），CK_CNT最终会输入给 CNT，CNT 支持增计数模式、减计数模式和增减计数模式，并有一个自动重装值寄存器（ATRLR）在每个计数周期结束后为 CNT 重新装载初始值。另外还有个辅助计数器在一旁记录 ATRLR为 CNT 重新装载初值的次数，当达到重复计数值寄存器（RPTCR）里设置的次数时，可以产生特定事件。

### 14.2.4 比较捕获通道和周边

比较捕获通道是定时器实现复杂功能的主要组件，它的核心是比较捕获寄存器，辅以外围输入部分的数字滤波，分频和通道间复用、输出部分的比较器和输出控制组成。

![](images/fb11e2fc5d1b95207bf7c2106698c08b5ff41962496e279561d009aff757f7d5.jpg)  
图14-3 比较捕获通道的结构框图

比较捕获通道的结构框图如图14-3所示。信号从通道 x 引脚输入进来后可选做为 TIx（TI1的来源可以不只是CH1，见定时器的结构框图 14-1），TI1经过滤波器（ICF[3:0]）生成TI1F，再经过边沿检测器分成 TI1F_Rising 和 TI1F_Falling，这两个信号经过选择（CC1P）生成 TI1FP1，TI1FP1 和来自通道2的TI2FP1一起送给CC1S选择成为IC1，经过 ICPS分频后送给比较捕获寄存器。

比较捕获寄存器由一个预装载寄存器和一个影子寄存器组成，读写过程仅操作预装载寄存器。在捕获模式下，捕获发生在影子寄存器上，然后复制到预装载寄存器；在比较模式下，预装载寄存器的内容被复制到影子寄存器中，然后影子寄存器的内容与核心计数器（CNT）进行比较。

## 14.3 功能和实现

高级定时器复杂功能的实现都是对定时器的比较捕获通道、时钟输入电路和计数器及周边部分的操作实现的。定时器的时钟输入可以来自于包括比较捕获通道的输入在内的多个时钟源。对比较捕获通道和时钟源选择的操作直接决定其功能。比较捕获通道是双向的，可以工作在输入和输出模式。

### 14.3.1 输入捕获模式

输入捕获模式是定时器的基本功能之一。输入捕获模式的原理是，当检测到 ${ \mathsf { I C X P S } }$ 信号上确定的边沿后，则发生捕获事件，计数器当前的值会被锁存到比较捕获寄存器（R16_TIMx_CHCTLRx）中。发生捕获事件时， ${ \tt C C } \times { \tt I F }$ （在 R16_TIMx_INTFR 中）被置位，如果使能了中断或 DMA，还会产生相应中断或DMA。如果发生捕获事件时， ${ \mathsf { C C } } \times { \mathsf { I F } }$ 已经被置位了，那么 ${ \tt C C } \tt { x O F }$ 位会被置位。CCxIF可由软件清除，也可以通过读取比较捕获寄存器由硬件清除。 $\mathtt { C C } \mathtt { x O F }$ 由软件清除。

举个通道1的例子来说明使用输入捕获模式的步骤，如下：

1） 配置 $\mathtt { C C } \mathtt { x } \mathtt { S }$ 域，选择 $1 0 \times$ 信号的来源。比如设为 10b，选择 TI1FP1 作为 IC1的来源，而不可以使用默认设置， $\mathtt { C O x S }$ 域默认是使比较捕获模块作为输出通道；  
2） 配置 ${ | 0 \times | }$ 域，设定 TI 信号的数字滤波器。数字滤波器会以确定的频率，采样确定的次数，再输出一个跳变。这个采样频率和次数是通过 ${ | 0 \times | }$ 来确定的；  
3） 配置 $\mathtt { C C } \mathtt { x } \mathtt { P }$ 位，设定 TIxFPx 的极性。比如保持 CC1P 位为低，选择上升沿跳变；  
4） 配置 ${ \mathsf { I C X P S } }$ 域，设定 $1 0 \times$ 信号成为 ${ \mathsf { I C X P S } }$ 之间的分频系数。比如保持 ${ \mathsf { I C X P S } }$ 为 00b，不分频；  
5） 配置 $\mathtt { C C } \mathtt { x } \mathtt { E }$ 位，允许捕获核心计数器（CNT）的值到比较捕获寄存器中。置 CC1E位；  
6） 根据需要配置 $\mathsf { C C } \mathsf { \times } { \mathsf { I E } }$ 和 $\complement 0 \times \mathsf { D E }$ 位，决定是否允许使能中断或 DMA。

至此已经将比较捕获通道配置完成。

当TI1输入了一个被捕获的脉冲时，核心计数器（CNT）的值会被记录到比较捕获寄存器中，CC1IF被置位，当CC1IF在之前就已经被置位时，CCIOF位也会被置位。如果 CC1IE位，那么会产生一个中断；如果 CC1DE 被置位，会产生一个 DMA 请求。可以通过写事件产生寄存器（TIMx_SWEVGR）的方式由软件产生一个输入捕获事件。

### 14.3.2 比较输出模式

比较输出模式是定时器的基本功能之一。比较输出模式的原理是在核心计数器（CNT）的值与比较捕获寄存器的值一致时，输出特定的变化或波形。 $0 0 \times 1 1$ 域（在 R16_TIMx_CHCTLRx 中）和 $\complement 0 \times \emptyset$ 位（在 R16_TIMx_CCER 中）决定输出的是确定的高低电平还是电平翻转。产生比较一致事件时还会置CCxIF位，如果预先置了 $\mathsf { C C } \mathsf { \times } { \mathsf { I E } }$ 位，则会产生一个中断；如果预先设置了 $\complement 0 \times \mathsf { D E }$ 位，则会产生一个 DMA请求。

配置为比较输出模式的步骤为下：

1） 配置核心计数器（CNT）的时钟源和自动重装值；  
2） 设置需要对比的计数值到比较捕获寄存器（R16_TIMx_CHxCVR）中；  
3） 如果需要产生中断，置 $\complement 0 \times 1 \mathsf { E }$ 位；  
4） 保持 ${ \tt O C } \tt { x P E }$ 为 0，禁用比较寄存器的预装载寄存器；  
5） 设定输出模式，设置 $0 0 \times 1 1$ 域和 $\complement 0 \times \emptyset$ 位；  
6） 使能输出，置 $\mathtt { C C } \mathtt { x } \mathtt { E }$ 位；  
7） 置 CEN 位启动定时器。

### 14.3.3 强制输出模式

定时器的比较捕获通道的输出模式可以由软件强制输出确定的电平，而不依赖比较捕获寄存器的影子寄存器和核心计数器的比较。

具体的做法是将 $0 0 \times 1 1$ 置为100b，即为强制将OCxREF 置为低；或者将 $0 0 \times 1 1$ 置为 101b，即为强制

将 ${ 0 0 } \times { \mathsf { R E F } }$ 置为高。

需要注意的是，将 $0 0 \times 1 1$ 强制置为 100b 或者 101b，内部核心计数器和比较捕获寄存器的比较过程还在进行，相应的标志位还在置位，中断和 DMA请求还在产生。

### 14.3.4 PWM 输入模式

PWM 输入模式是用来测量 PWM 的占空比和频率的，是输入捕获模式的一种特殊情况。除下列区别外，操作和输入捕获模式相同：PWM占用两个比较捕获通道，且两个通道的输入极性设为相反，其中一个信号被设为触发输入，SMS设为复位模式。

例如，测量从 TI1 输入的 PWM 波的周期和频率，需要进行以下操作：

1） 将 TI1(TI1FP1)设为 IC1 信号的输入。将 CC1S 置为 01b；  
2） 将 TI1FP1 置为上升沿有效。将 CC1P 保持为 0；  
3） 将 TI1(TI1FP2)置为 IC2 信号的输入。将 CC2S 置为 10b；  
4） 选 TI1FP2 置为下降沿有效。将 CC2P 置为 1；  
5） 时钟源的来源选择 TI1FP1。将 TS 设为 101b；  
6） 将 SMS 设为复位模式，即 100b；  
7） 使能输入捕获。CC1E 和 CC2E 置位；

这样比较捕获寄存器 1 的值就是 PWM 的周期，而比较捕获寄存器 2 的值就是其占空比。

### 14.3.5 PWM 输出模式

PWM 输出模式是定时器的基本功能之一。PWM 输出模式最常见的是使用重装值确定 PWM 频率，使用捕获比较寄存器确定占空比的方法。将 $0 0 \times 1 1$ 域中置 110b 或 111b 使用 PWM 模式 1 或模式 2，置OCxPE 位使能预装载寄存器，最后置 ARPE 位使能预装载寄存器的自动重装载。由于在发生一个更新事件时，预装载寄存器的值才能被送到影子寄存器，所以在核心计数器开始计数之前，需要置 UG 位来初始化所有寄存器。在 PWM 模式下，核心计数器和比较捕获寄存器一直在进行比较，根据 CMS 位，定时器能够输出边沿对齐或中央对齐的PWM信号。

#### 边沿对齐

使用边沿对齐时，核心计数器增计数或减计数，在PWM模式 1的情景下，在核心计数器的值大于比较捕获寄存器时，OCxREF为高；当核心计数器的值小于比较捕获寄存器时（比如核心计数器增长到R16_TIMx_ATRLR 的值而恢复成全 0 时），OCxREF 为低。

#### 中央对齐

使用中央对齐模式时，核心计数器运行在增计数和减计数交替进行的模式下，OCxREF 在核心计数器和比较捕获寄存器的值一致时进行上升和下降的跳变。但比较标志在三种中央对齐模式下，置位的时机有所不同。在使用中央对齐模式时，最好在启动核心计数器之前产生一个软件更新标志（置 UG位）。

### 14.3.6 互补输出和死区

比较捕获通道一般有两个输出引脚（比较捕获通道4 只有一个输出引脚），能输出两个互补的信号（ $\mathtt { 0 0 x }$ 和 ${ \tt O C } \times { \tt N } ,$ ）， $\mathtt { 0 0 x }$ 和 $0 0 \times 1 1$ 可以通过 $\mathtt { C C } \mathtt { x } \mathtt { P }$ 和 ${ \mathsf { C C } } { \mathsf { x N P } }$ 位独立地设置极性，通过 $\mathtt { C C } \mathtt { x } \mathtt { E }$ 和 ${ \tt C C } \times { \tt N E }$ 独立地设置输出使能，通过 MOE、OIS、OISN、OSSI、OSSR 位进行死区和其他的控制。同时使能 $\mathtt { 0 0 x }$ 和$\mathtt { 0 0 x N }$ 输出将插入死区，每个通道都有一个 10 位的死区发生器。如果存在刹车电路则还要设置 MOE 位。$\mathtt { 0 0 x }$ 和 $0 0 \times 1 1$ 由 ${ 0 0 } \times { \mathsf { R E F } }$ 关联产生，如果 $\mathtt { 0 0 x }$ 和 $0 0 \times 1 1$ 都是高有效，那么 $\mathtt { O C x }$ 与 ${ 0 0 } \times { \mathsf { R E F } }$ 相同，只是 $\mathtt { 0 0 x }$ 的上升沿相当于 OCxREF 有一个延迟， $\mathtt { 0 0 x N }$ 与 $\mathtt { O C x }$ REF 相反，它的上升沿相对参考信号的下降沿会有一个延迟，如果延迟大于有效输出宽度，则不会产生相应的脉冲。

如图 14-4 展示了 $\mathtt { 0 0 x }$ 和 $0 0 \times 1 1$ 与 ${ 0 0 } \times { \mathsf { R E F } }$ 的关系，并展示出死区。

![](images/21bf9e687300b7ab4d10354fb8f56cb28601deddf5e54ee22a453127d52cc5ca.jpg)  
图14-4 互补输出和死区

### 14.3.7 刹车信号

当产生刹车信号时，输出使能信号和无效电平都会根据 MOE、OIS、OISN、OSSI 和 OSSR等位进行修改。但 $\mathtt { 0 0 x }$ 和 $0 0 \times 1 1$ 不会在任何时间都处在有效电平。刹车事件源可以来自于刹车输入引脚，也可以是一个时钟失败事件，而时钟失败事件由 CSS（时钟安全系统）产生。

在系统复位后，刹车功能被默认禁止（MOE位为低），置 BKE位可以使能刹车功能，输入的刹车信号的极性可以通过设置 BKP 设置，BKE 和 BKP 信号可以被同时写入，在真正写入之前会有一个 APB时钟的延迟，因此需要等一个APB周期才能正确读出写入值。

在刹车引脚出现选定的电平系统将产生如下动作：

1） MOE 位被异步清零，根据 SOOI 位的设置将输出置为无效状态、空闲状态或复位状态；  
2） 在 MOE 被清零后，每一个输出通道输出由 OISx 确定的电平；  
3） 当使用互补输出时：输出被置于无效状态，具体取决于极性；  
4） 如果 BIE 被置位，当 BIF 置位，会产生一个中断；如果设置了 BDE 位，则会产生一个 DMA 请求；  
5） 如果 AOE 被置位，在下一个更新事件 UEV 时，MOE 位被自动置位。

### 14.3.8 单脉冲模式

单脉冲模式可以用于让微控制器响应一个特定的事件，使之在一个延迟之后产生一个脉冲，延迟和脉冲的宽度可编程。置 OPM 位可以使核心计数器在产生下一个更新事件 UEV 时（计数器翻转到 0）停止。

如图 14-5，需要在 TI2 输入引脚上检测到一个上升沿开始，延迟 Tdelay 之后，在 OC1 上产生一个长度为Tpulse的正脉冲：

![](images/bf0c0f74ac10446bde3f378fb6e1ea5ce75076a357f7e578a8ce973207bb65cd.jpg)  
图 14-5 单脉冲的产生

1） 设定 TI2 为触发。置 CC2S 域为 01b，把 TI2FP2 映射到 TI2；置 CC2P 位为 0b，TI2FP2 设为上升沿检测；置 TS 域为 110b，TI2FP2 设为触发源；置 SMS 域为 110b，TI2FP2 被用来启动计数器；  
2） Tdelay 由比较捕获寄存器的的值确定，Tpulse 由自动重装值寄存器的值和比较捕获寄存器的值

确定。

### 14.3.9 编码器模式

编码器模式是定时器的一个典型应用，可以用来接入编码器的双相输出，核心计数器的计数方向和编码器的转轴方向同步，编码器每输出一个脉冲就会使核心计数器加一或减一。使用编码器的步骤为：将 SMS 域置为 001b（只在 TI2 边沿计数）、010b（只在 TI1 边沿计数）或 011b（在 TI1 和 TI2 双边沿计数），将编码器接到比较捕获通道 1、2 的输入端，给重装值寄存器设一个值，这个值可以设的大一点。在编码器模式时，定时器内部的比较捕获寄存器，预分频器，重复计数寄存器等都正常工作。下表表明了计数方向和编码器信号的关系。

表 14-1 定时器编码器模式的计数方向和编码器信号之间的关系  

<table><tr><td rowspan="2">计数有效边沿</td><td rowspan="2">相对信号的电平</td><td colspan="2">TI1FP1 信号边沿</td><td colspan="2">TI2FP2 信号</td></tr><tr><td>上升沿</td><td>下降沿</td><td>上升沿</td><td>下降沿</td></tr><tr><td rowspan="2">仅在 TI1 边沿计数</td><td>高</td><td>向下计数</td><td>向上计数</td><td rowspan="2" colspan="2">不计数</td></tr><tr><td>低</td><td>向上计数</td><td>向下计数</td></tr><tr><td rowspan="2">仅在 TI2 边沿计数</td><td>高</td><td rowspan="2" colspan="2">不计数</td><td>向上计数</td><td>向下计数</td></tr><tr><td>低</td><td>向下计数</td><td>向上计数</td></tr><tr><td rowspan="2">在 TI1 和 TI2 双边沿计数</td><td>高</td><td>向下计数</td><td>向上计数</td><td>向上计数</td><td>向下计数</td></tr><tr><td>低</td><td>向上计数</td><td>向下计数</td><td>向下计数</td><td>向上计数</td></tr></table>

### 14.3.10 定时器同步模式

定时器能够输出时钟脉冲（TRGO），也能接收其他定时器的输入（ITRx）。不同的定时器的 ITRx的来源（别的定时器的TRGO）是不一样的。定时器内部触发连接如表 14-2所示。

表 14-2 TIMx 内部触发连接  

<table><tr><td>从定时器</td><td>ITRO (TS=000)</td><td>ITR1 (TS=001)</td><td>ITR2 (TS=010)</td><td>ITR3 (TS=011)</td></tr><tr><td>TIM1</td><td>TIM5</td><td>TIM2</td><td>TIM3</td><td>TIM4</td></tr><tr><td>TIM8</td><td>TIM1</td><td>TIM2</td><td>TIM4</td><td>TIM5</td></tr><tr><td>TIM9</td><td>TIM10</td><td>TIM5</td><td>TIM6</td><td>TIM7</td></tr><tr><td>TIM10</td><td>TIM9</td><td>TIM2</td><td>TIM4</td><td>TIM5</td></tr></table>

### 14.3.11 调试模式

当系统进入调试模式时，定时器根据 DBG模块的设置继续运转或停止。

## 14.4 寄存器描述

表 14-3 TIM1 相关寄存器列表  

<table><tr><td>名称</td><td>访问地址</td><td>描述</td><td>复位值</td></tr><tr><td>R16_TIM1_CTLR1</td><td>0x40012C00</td><td>控制寄存器1</td><td>0x0000</td></tr><tr><td>R16_TIM1_CTLR2</td><td>0x40012C04</td><td>控制寄存器2</td><td>0x0000</td></tr><tr><td>R16_TIM1_SMCFGR</td><td>0x40012C08</td><td>从模式控制寄存器</td><td>0x0000</td></tr><tr><td>R16_TIM1_DMAINTENR</td><td>0x40012C0C</td><td>DMA/中断使能寄存器</td><td>0x0000</td></tr><tr><td>R16_TIM1_INTFR</td><td>0x40012C10</td><td>中断状态寄存器</td><td>0x0000</td></tr><tr><td>R16_TIM1_SWEVGR</td><td>0x40012C14</td><td>事件产生寄存器</td><td>0x0000</td></tr><tr><td>R16_TIM1_CHCTLR1</td><td>0x40012C18</td><td>比较/捕获控制寄存器1</td><td>0x0000</td></tr><tr><td>R16_TIM1_CHCTLR2</td><td>0x40012C1C</td><td>比较/捕获控制寄存器2</td><td>0x0000</td></tr><tr><td>R16_TIM1_CCER</td><td>0x40012C20</td><td>比较/捕获使能寄存器</td><td>0x0000</td></tr><tr><td>R16_TIM1_CNT</td><td>0x40012C24</td><td>计数器</td><td>0x0000</td></tr><tr><td>R16_TIM1_PSC</td><td>0x40012C28</td><td>计数时钟预分频器</td><td>0x0000</td></tr><tr><td>R16_TIM1_ATRLR</td><td>0x40012C2C</td><td>自动重装值寄存器</td><td>0x0000</td></tr><tr><td>R16_TIM1_RPTCR</td><td>0x40012C30</td><td>重复计数值寄存器</td><td>0x0000</td></tr><tr><td>R16_TIM1_CH1CVR</td><td>0x40012C34</td><td>比较/捕获寄存器1</td><td>0x0000</td></tr><tr><td>R16_TIM1_CH2CVR</td><td>0x40012C38</td><td>比较/捕获寄存器2</td><td>0x0000</td></tr><tr><td>R16_TIM1_CH3CVR</td><td>0x40012C3C</td><td>比较/捕获寄存器3</td><td>0x0000</td></tr><tr><td>R16_TIM1_CH4CVR</td><td>0x40012C40</td><td>比较/捕获寄存器4</td><td>0x0000</td></tr><tr><td>R16_TIM1_BDTR</td><td>0x40012C44</td><td>刹车和死区寄存器</td><td>0x0000</td></tr><tr><td>R16_TIM1_DMACFGR</td><td>0x40012C48</td><td>DMA控制寄存器</td><td>0x0000</td></tr><tr><td>R16_TIM1_DMAADR</td><td>0x40012C4C</td><td>连续模式的DMA地址寄存器</td><td>0x0000</td></tr></table>

表 14-4 TIM8 相关寄存器列表  

<table><tr><td>名称</td><td>访问地址</td><td>描述</td><td>复位值</td></tr><tr><td>R16_TIM8_CTLR1</td><td>0x40013400</td><td>控制寄存器1</td><td>0x0000</td></tr><tr><td>R16_TIM8_CTLR2</td><td>0x40013404</td><td>控制寄存器2</td><td>0x0000</td></tr><tr><td>R16_TIM8_SMCFGR</td><td>0x40013408</td><td>从模式控制寄存器</td><td>0x0000</td></tr><tr><td>R16_TIM8_DMAINTENR</td><td>0x4001340C</td><td>DMA/中断使能寄存器</td><td>0x0000</td></tr><tr><td>R16_TIM8_INTERR</td><td>0x40013410</td><td>中断状态寄存器</td><td>0x0000</td></tr><tr><td>R16_TIM8_SWEVGR</td><td>0x40013414</td><td>事件产生寄存器</td><td>0x0000</td></tr><tr><td>R16_TIM8_CHCTLR1</td><td>0x40013418</td><td>比较/捕获控制寄存器1</td><td>0x0000</td></tr><tr><td>R16_TIM8_CHCTLR2</td><td>0x4001341C</td><td>比较/捕获控制寄存器2</td><td>0x0000</td></tr><tr><td>R16_TIM8_CCER</td><td>0x40013420</td><td>比较/捕获使能寄存器</td><td>0x0000</td></tr><tr><td>R16_TIM8_CNT</td><td>0x40013424</td><td>计数器</td><td>0x0000</td></tr><tr><td>R16_TIM8_PSC</td><td>0x40013428</td><td>计数时钟预分频器</td><td>0x0000</td></tr><tr><td>R16_TIM8_ATRLR</td><td>0x4001342C</td><td>自动重装值寄存器</td><td>0x0000</td></tr><tr><td>R16_TIM8_RPTCR</td><td>0x40013430</td><td>重复计数值寄存器</td><td>0x0000</td></tr><tr><td>R16_TIM8_CH1CVR</td><td>0x40013434</td><td>比较/捕获寄存器1</td><td>0x0000</td></tr><tr><td>R16_TIM8_CH2CVR</td><td>0x40013438</td><td>比较/捕获寄存器2</td><td>0x0000</td></tr><tr><td>R16_TIM8_CH3CVR</td><td>0x4001343C</td><td>比较/捕获寄存器3</td><td>0x0000</td></tr><tr><td>R16_TIM8_CH4CVR</td><td>0x40013440</td><td>比较/捕获寄存器4</td><td>0x0000</td></tr><tr><td>R16_TIM8_BDTR</td><td>0x40013444</td><td>刹车和死区寄存器</td><td>0x0000</td></tr><tr><td>R16_TIM8_DMACFGR</td><td>0x40013448</td><td>DMA控制寄存器</td><td>0x0000</td></tr><tr><td>R16_TIM8_DMAADR</td><td>0x4001344C</td><td>连续模式的DMA地址寄存器</td><td>0x0000</td></tr></table>

表 14-5 TIM9 相关寄存器列表  

<table><tr><td>名称</td><td>访问地址</td><td>描述</td><td>复位值</td></tr><tr><td>R16_TIM9_CTLR1</td><td>0x40014C00</td><td>控制寄存器1</td><td>0x0000</td></tr><tr><td>R16_TIM9_CTLR2</td><td>0x40014C04</td><td>控制寄存器2</td><td>0x0000</td></tr><tr><td>R16_TIM9_SMCFGR</td><td>0x40014C08</td><td>从模式控制寄存器</td><td>0x0000</td></tr><tr><td>R16_TIM9_DMAINTENR</td><td>0x40014C0C</td><td>DMA/中断使能寄存器</td><td>0x0000</td></tr><tr><td>R16_TIM9_INTFR</td><td>0x40014C10</td><td>中断状态寄存器</td><td>0x0000</td></tr><tr><td>R16_TIM9_SWEVGR</td><td>0x40014C14</td><td>事件产生寄存器</td><td>0x0000</td></tr><tr><td>R16_TIM9_CHCTLR1</td><td>0x40014C18</td><td>比较/捕获控制寄存器1</td><td>0x0000</td></tr><tr><td>R16_TIM9_CHCTLR2</td><td>0x40014C1C</td><td>比较/捕获控制寄存器2</td><td>0x0000</td></tr><tr><td>R16_TIM9_CCER</td><td>0x40014C20</td><td>比较/捕获使能寄存器</td><td>0x0000</td></tr><tr><td>R16_TIM9_CNT</td><td>0x40014C24</td><td>计数器</td><td>0x0000</td></tr><tr><td>R16_TIM9_PSC</td><td>0x40014C28</td><td>计数时钟预分频器</td><td>0x0000</td></tr><tr><td>R16_TIM9_ATRLR</td><td>0x40014C2C</td><td>自动重装值寄存器</td><td>0x0000</td></tr><tr><td>R16_TIM9_RPTCR</td><td>0x40014C30</td><td>重复计数值寄存器</td><td>0x0000</td></tr><tr><td>R16_TIM9_CH1CVR</td><td>0x40014C34</td><td>比较/捕获寄存器1</td><td>0x0000</td></tr><tr><td>R16_TIM9_CH2CVR</td><td>0x40014C38</td><td>比较/捕获寄存器2</td><td>0x0000</td></tr><tr><td>R16_TIM9_CH3CVR</td><td>0x40014C3C</td><td>比较/捕获寄存器3</td><td>0x0000</td></tr><tr><td>R16_TIM9_CH4CVR</td><td>0x40014C40</td><td>比较/捕获寄存器4</td><td>0x0000</td></tr><tr><td>R16_TIM9_BDTR</td><td>0x40014C44</td><td>刹车和死区寄存器</td><td>0x0000</td></tr><tr><td>R16_TIM9_DMACFGR</td><td>0x40014C48</td><td>DMA控制寄存器</td><td>0x0000</td></tr><tr><td>R16_TIM9_DMAADR</td><td>0x40014C4C</td><td>连续模式的DMA地址寄存器</td><td>0x0000</td></tr></table>

表 14-6 TIM10 相关寄存器列表  

<table><tr><td>名称</td><td>访问地址</td><td>描述</td><td>复位值</td></tr><tr><td>R16_TIM10_CTLR1</td><td>0x40015000</td><td>控制寄存器1</td><td>0x0000</td></tr><tr><td>R16_TIM10_CTLR2</td><td>0x40015004</td><td>控制寄存器2</td><td>0x0000</td></tr><tr><td>R16_TIM10_SMCFGR</td><td>0x40015008</td><td>从模式控制寄存器</td><td>0x0000</td></tr><tr><td>R16_TIM10_DMAINTENR</td><td>0x4001500C</td><td>DMA/中断使能寄存器</td><td>0x0000</td></tr><tr><td>R16_TIM10_INTFR</td><td>0x40015010</td><td>中断状态寄存器</td><td>0x0000</td></tr><tr><td>R16_TIM10_SWEVGR</td><td>0x40015014</td><td>事件产生寄存器</td><td>0x0000</td></tr><tr><td>R16_TIM10_CHCTLR1</td><td>0x40015018</td><td>比较/捕获控制寄存器1</td><td>0x0000</td></tr><tr><td>R16_TIM10_CHCTLR2</td><td>0x4001501C</td><td>比较/捕获控制寄存器2</td><td>0x0000</td></tr><tr><td>R16_TIM10_CCER</td><td>0x40015020</td><td>比较/捕获使能寄存器</td><td>0x0000</td></tr><tr><td>R16_TIM10_CNT</td><td>0x40015024</td><td>计数器</td><td>0x0000</td></tr><tr><td>R16_TIM10_PSC</td><td>0x40015028</td><td>计数时钟预分频器</td><td>0x0000</td></tr><tr><td>R16_TIM10_ATRLR</td><td>0x4001502C</td><td>自动重装值寄存器</td><td>0x0000</td></tr><tr><td>R16_TIM10_RPTCR</td><td>0x40015030</td><td>重复计数值寄存器</td><td>0x0000</td></tr><tr><td>R16_TIM10_CH1CVR</td><td>0x40015034</td><td>比较/捕获寄存器1</td><td>0x0000</td></tr><tr><td>R16_TIM10_CH2CVR</td><td>0x40015038</td><td>比较/捕获寄存器2</td><td>0x0000</td></tr><tr><td>R16_TIM10_CH3CVR</td><td>0x4001503C</td><td>比较/捕获寄存器3</td><td>0x0000</td></tr><tr><td>R16_TIM10_CH4CVR</td><td>0x40015040</td><td>比较/捕获寄存器4</td><td>0x0000</td></tr><tr><td>R16_TIM10_BDTR</td><td>0x40015044</td><td>刹车和死区寄存器</td><td>0x0000</td></tr><tr><td>R16_TIM10_DMACFGR</td><td>0x40015048</td><td>DMA控制寄存器</td><td>0x0000</td></tr><tr><td>R16_TIM10_DMAADR</td><td>0x4001504C</td><td>连续模式的DMA地址寄存器</td><td>0x0000</td></tr></table>

### 14.4.1 控制寄存器 1（TIMx_CTLR1）（x=1/8/9/10）

偏移地址： $0 \times 0 0$

<table><tr><td>Reserved</td><td>CKD[1:0]</td><td>ARPE</td><td>CMS[1:0]</td><td>DIR</td><td>OPM</td><td>URS</td><td>UDIS</td><td>CEN</td></tr></table>

<table><tr><td>位</td><td>名称</td><td>访问</td><td>描述</td><td>复位值</td></tr><tr><td>[15:10]</td><td>Reserved</td><td>R0</td><td>保留。</td><td>0</td></tr><tr><td>[9:8]</td><td>CKD[1:0]</td><td>RW</td><td>这2位定义在定时器时钟(CK_INT)频率、死区时间和由死区发生器与数字滤波器(ETR, T1x)所用的采样时钟之间的分频比例:00: Tdts=Tck_int01: Tdts=2xTck_int10: Tdts=4xTck_int11: 保留。</td><td>00b</td></tr><tr><td>7</td><td>ARPE</td><td>RW</td><td>自动重装预装使能位:1: 使能自动重装值寄存器(ATRLR);0: 禁止自动重装值寄存器(ATRLR)。</td><td>0</td></tr><tr><td>[6:5]</td><td>CMS[1:0]</td><td>RW</td><td>中央对齐模式选择:00: 边沿对齐模式。计数器依据方向位(DIR)向上或向下计数。01: 中央对齐模式1。计数器交替地向上和向下计数。配置为输出的通道(CHCTLRx 寄存器中CCxS=00)的输出比较中断标志位,只在计数器向下计数时被设置。10: 中央对齐模式2。计数器交替地向上和向下计数。配置为输出的通道(CHCTLRx 寄存器中CCxS=00)的输出比较中断标志位,只在计数器向上计数时被设置。11: 中央对齐模式3。计数器交替地向上和向下计数。配置为输出的通道(CHCTLRx 寄存器中CCxS=00)的输出比较中断标志位,在计数器向上和向下计数时均被设置。注:在计数器使能时(CEN=1),不允许从边沿对齐模式转换到中央对齐模式。</td><td>00b</td></tr><tr><td>4</td><td>DIR</td><td>RW</td><td>计数器方向:0: 计数器的计数模式为增计数;1: 计数器的计数模式为减计数。注:当计数器配置为中央对齐模式或编码器模式时,该位无效。</td><td>0</td></tr><tr><td>3</td><td>OPM</td><td>RW</td><td>单脉冲模式:1: 在发生下一次更新事件(清除CEN位)时,计数器停止。0: 在发生下一次更新事件时,计数器不停止。</td><td>0</td></tr><tr><td>2</td><td>URS</td><td>RW</td><td>更新请求源,软件通过该位选择UEV事件的源。1: 如果使能了更新中断或DMA请求,则只有计数器溢出/下溢才产生更新中断或DMA请求;0: 如果使能了更新中断或DMA请求,则下述任一事件产生更新中断或DMA请求。-计数器溢出/下溢</td><td>0</td></tr><tr><td></td><td></td><td></td><td>-设置UG位
-从模式控制器产生的更新</td><td></td></tr><tr><td>1</td><td>UDIS</td><td>RW</td><td>禁止更新，软件通过该位允许/禁止UEV事件的产生。
1:禁止UEV。不产生更新事件,各寄存器(ARR、PSC、CCRx)保持它们的值。如果设置了UG位或从模式控制器发出了一个硬件复位,则计数器和预分频器被重新初始化;
0:允许UEV。更新(UEV)事件由下述任一事件产生:
-计数器溢出/下溢
-设置UG位
-从模式控制器产生的更新
具有缓存的寄存器被装入它们的预装载值。</td><td>0</td></tr><tr><td>0</td><td>CEN</td><td>RW</td><td>使能计数器。
1:使能计数器;
0:禁止计数器。
注:在软件设置了CEN位后,外部时钟、门控模式和编码器模式才能工作。触发模式可以自动地通过硬件设置CEN位。</td><td>0</td></tr></table>

### 14.4.2 控制寄存器 2（TIMx_CTLR2）（x=1/8/9/10）

偏移地址： $0 \times 0 4$

<table><tr><td>15</td><td>14</td><td>13</td><td>12</td><td>11</td><td>10</td><td>9</td><td>8</td><td>7</td><td>6</td><td>5</td><td>4</td><td>3</td><td>2</td><td>1</td><td>0</td></tr><tr><td>Reserved</td><td>01S4</td><td>01S3N</td><td>01S3</td><td>01S2N</td><td>01S2</td><td>01S2</td><td>01S1N</td><td>01S1</td><td>TI1S</td><td colspan="2">MMS[2:0]</td><td>CCDS</td><td>CCUS</td><td>Reserved</td><td>CCPC</td></tr></table>

<table><tr><td>位</td><td>名称</td><td>访问</td><td>描述</td><td>复位值</td></tr><tr><td>15</td><td>Reserved</td><td>R0</td><td>保留。</td><td>0</td></tr><tr><td>14</td><td>01S4</td><td>RW</td><td>输出空闲状态4:1:当MOE=0时,如果实施了0C4N,则死区后0C1=1;0:当MOE=0时,如果实施了0C4N,则死区后0C1=0。注:已经设置了LOCK(TIMx_BDTR寄存器)级别1、2或3后,该位不能被修改。</td><td>0</td></tr><tr><td>13</td><td>01S3N</td><td>RW</td><td>输出空闲状态3:1:当MOE=0时,死区后0C1N=1;0:当MOE=0时,死区后0C1N=0。注:已经设置了LOCK(TIMx_BDTR寄存器)级别1、2或3后,该位不能被修改。</td><td>0</td></tr><tr><td>12</td><td>01S3</td><td>RW</td><td>输出空闲状态3,参见01S4。</td><td>0</td></tr><tr><td>11</td><td>01S2N</td><td>RW</td><td>输出空闲状态2,参见01S3N。</td><td>0</td></tr><tr><td>10</td><td>01S2</td><td>RW</td><td>输出空闲状态2,参见01S4。</td><td>0</td></tr><tr><td>9</td><td>01S1N</td><td>RW</td><td>输出空闲状态1,参见01S3N。</td><td>0</td></tr><tr><td>8</td><td>01S1</td><td>RW</td><td>输出空闲状态1,参见01S4。</td><td>0</td></tr><tr><td>7</td><td>TI1S</td><td>RW</td><td>TI1 选择:1:TIMx_CH1、TIMx_CH2和TIMx_CH3引脚经异或后连到TI1输入;0:TIMx_CH1引脚直连到TI1输入。</td><td>0</td></tr><tr><td>[6:4]</td><td>MMS[2:0]</td><td>RW</td><td>主模式选择:这3位用于选择在主模式下送到从定时器的同步信息(TRGO)。可能的组合如下:000:复位-TIMx_EGR寄存器的UG位被用于作为触发输出(TRGO)。如果是触发输入产生的复位(从模式控制器处于复位模式),则TRGO上的信号相对实际的复位会有一个延迟;001:使能-计数器使能信号CNT_EN被用于作为触发输出(TRGO)。有时需要在同一时间启动多个定时器或控制在一段时间内使能从定时器。计数器使能信号是通过CEN控制位和门控模式下的触发输入信号的逻辑或产生。当计数器使能信号受控于触发输入时,TRGO上会有一个延迟,除非选择了主/从模式(见TIMx_SMCR寄存器中MSM位的描述);010:更新-更新事件被选为触发输入(TRGO)。例如,一个主定时器的时钟可以被用作一个从定时器的预分频器;011:比较脉冲-在发生一次捕获或一次比较成功时,当要设置CC1IF标志时(即使它已经为高),触发输出送出一个正脉冲(TRGO);100:比较-0C1REF信号被用于作为触发输出(TRGO);101:比较-0C2REF信号被用于作为触发输出(TRGO);110:比较-0C3REF信号被用于作为触发输出(TRGO);111:比较-0C4REF信号被用于作为触发输出(TRGO)。</td><td>000b</td></tr><tr><td>3</td><td>CCDS</td><td>RW</td><td>捕获比较的DMA选择。1:当发生更新事件时,送出CHxCVR的DMA请求;0:当发生CHxCVR时,产生CHxCVR的DMA请求。</td><td>0</td></tr><tr><td>2</td><td>CCUS</td><td>RW</td><td>比较捕获控制更新选择位。1:如果CCPC置位,可以通过设置COM位或TRGI上的一个上升沿更新它们;0:如果CCPC置位,只能通过设置COM位更新它们。注:该位只对具有互补输出的通道起作用。</td><td>0</td></tr><tr><td>1</td><td>Reserved</td><td>RO</td><td>保留。</td><td>0</td></tr><tr><td>0</td><td>CCPC</td><td>RW</td><td>比较捕获预装载控制位。1:CCxE,CCxNE和OCxM位是预装载的,设置该位后,它们只在设置了COM位后被更新;</td><td>0</td></tr><tr><td></td><td></td><td></td><td>0: CCxE, CCxNE 和 OCxM 位不是预装载的。
注: 该位只对具有互补输出的通道起作用。</td><td></td></tr></table>

### 14.4.3 从模式控制寄存器（TIMx_SMCFGR）（x=1/8/9/10）

偏移地址： $0 \times 0 8$

15 14 13 12 11 10 9 8 7 6 5 4 3 2 1 0

<table><tr><td>ETP</td><td>ECE</td><td>ETPS[1:0]</td><td>ETF[3:0]</td><td>MSM</td><td>TS[2:0]</td><td>Reserved</td><td>SMS[2:0]</td></tr></table>

<table><tr><td>位</td><td>名称</td><td>访问</td><td>描述</td><td>复位值</td></tr><tr><td>15</td><td>ETP</td><td>R0</td><td>ETR 触发极性选择,该位选择是直接输入 ETR 还是输入 ETR 的反相。1:将 ETR 反相,低电平或下降沿有效;0:ETR,高电平或上升沿有效。</td><td>0</td></tr><tr><td>14</td><td>ECE</td><td>RW</td><td>外部时钟模式 2 启用选择:1:使能外部时钟模式 2;0:禁用外部时钟模式 2。注 1:从模式可以与外部时钟模式 2 同时使用:复位模式,门控模式和触发模式;但是,这时TRGI不能连到 ETRF (TS 位不能是‘111’)。注 2:外部时钟模式 1 和外部时钟模式 2 同时被使能时,外部时钟的输入是 ETRF。</td><td>0</td></tr><tr><td>[13:12]</td><td>ETPS[1:0]</td><td>RW</td><td>外部触发信号 (ETRP) 分频,这个信号频率最大不能超过 TIMxCLK 频率的 1/4,可以通过这个域来降频:00:关闭预分频;01:ETRP 频率除以 2;10:ETRP 频率除以 4;11:ETRP 频率除以 8。</td><td>00b</td></tr><tr><td>[11:8]</td><td>ETF[3:0]</td><td>RW</td><td>外部触发滤波,实际上,数字滤波器是一个事件计数器,它使用一定的采样的频率,记录到 N 个事件后会产生一个输出的跳变。0000:无滤波器,以 Fdt's 采样;0001:采样频率 Fsampling=Fck_int,N=2;0010:采样频率 Fsampling=Fck_int,N=4;0011:采样频率 Fsampling=Fck_int,N=8;0100:采样频率 Fsampling=Fdt's/2,N=6;0101:采样频率 Fsampling=Fdt's/2,N=8;0110:采样频率 Fsampling=Fdt's/4,N=6;0111:采样频率 Fsampling=Fdt's/4,N=8;1000:采样频率 Fsampling=Fdt's/8,N=6;1001:采样频率 Fsampling=Fdt's/8,N=8;1010:采样频率 Fsampling=Fdt's/16,N=5;1011:采样频率 Fsampling=Fdt's/16,N=6;1100:采样频率 F sampling=Fdt's/16,N=8;1101:采样频率 F sampling=Fdt's/32,N=5;1110:采样频率 F sampling=Fdt's/32,N=6;</td><td>0000b</td></tr><tr><td></td><td></td><td></td><td>1111:采样频率Fsampling=Edts/32,N=8。</td><td></td></tr><tr><td>7</td><td>MSM</td><td>RW</td><td>主/从模式选择:1:触发输入(TRGI)上的事件被延迟了,以允许在当前定时器(通过TRGO)与它的从定时器间的完美同步。这对要求把几个定时器同步到一个单一的外部事件时是非常有用的;0:不发挥作用。</td><td>0</td></tr><tr><td>[6:4]</td><td>TS[2:0]</td><td>RW</td><td>触发选择域,这3位选择用于同步计数器的触发输入源:000:内部触发0(ITRO);001:内部触发1(ITR1);010:内部触发2(ITR2);011:内部触发3(ITR3);100:TI1的边沿检测器(TI1F_ED);101:滤波后的定时器输入1(TI1FP1);110:滤波后的定时器输入2(TI2FP2);111:外部触发输入(ETRF);以上只有在SMS为0时改变。注:具体见表14-2。</td><td>000b</td></tr><tr><td>3</td><td>Reserved</td><td>RO</td><td>保留。</td><td>0</td></tr><tr><td>[2:0]</td><td>SMS[2:0]</td><td>RW</td><td>输入模式选择域。选择核心计数器的时钟和触发模式。000:由内部时钟CK_INT驱动;001:编码器模式1,根据TI1FP1的电平,核心计数器在TI2FP2的边沿增减计数;010:编码器模式2,根据TI2FP2的电平,核心计数器在TI1FP1的边沿增减计数;011:编码器模式3,根据另一个信号的输入电平,核心计数器在TI1FP1和TI2FP2的边沿增减计数;100:复位模式,触发输入(TRGI)的上升沿将初始化计数器,并且产生一个更新寄存器的信号;101:门控模式,当触发输入(TRGI)为高时,计数器的时钟开启;在触发输入变为低,计数器停止,计数器的启停都是受控的;110:触发模式,计数器在触发输入TRGI的上升沿启动,只有计数器的启动是受控的;111:外部时钟模式1,选中的触发输入(TRGI)的上升沿驱动计数器。</td><td>000b</td></tr></table>

### 14.4.4 DMA/中断使能寄存器（TIMx_DMAINTENR）（x=1/8/9/10）

偏移地址： $0 \times 0 0$

<table><tr><td>15</td><td>14</td><td>13</td><td>12</td><td>11</td><td>10</td><td>9</td><td>8</td><td>7</td><td>6</td><td>5</td><td>4</td><td>3</td><td>2</td><td>1</td><td>0</td></tr><tr><td>Reserved</td><td>TDE</td><td>COMDE</td><td>CC4DE</td><td>CC3DE</td><td>CC2DE</td><td>CC1DE</td><td>UDE</td><td>BIE</td><td>TIE</td><td>COMIE</td><td>CC4IE</td><td>CC3IE</td><td>CC2IE</td><td>CC1IE</td><td>UIE</td></tr></table>

<table><tr><td>位</td><td>名称</td><td>访问</td><td>描述</td><td>复位值</td></tr><tr><td>15</td><td>Reserved</td><td>RO</td><td>保留。</td><td>0</td></tr><tr><td>14</td><td>TDE</td><td>RW</td><td>触发DMA请求使能位。1:允许触发DMA请求;0:禁止触发DMA请求。</td><td>0</td></tr><tr><td>13</td><td>COMDE</td><td>RW</td><td>COM的DMA请求使能位。1:允许COM的DMA请求;0:禁止COM的DMA请求。</td><td>0</td></tr><tr><td>12</td><td>CC4DE</td><td>RW</td><td>比较捕获通道4的DMA请求使能位。1:允许比较捕获通道4的DMA请求;0:禁止比较捕获通道4的DMA请求。</td><td>0</td></tr><tr><td>11</td><td>CC3DE</td><td>RW</td><td>比较捕获通道3的DMA请求使能位。1:允许比较捕获通道3的DMA请求;0:禁止比较捕获通道3的DMA请求。</td><td>0</td></tr><tr><td>10</td><td>CC2DE</td><td>RW</td><td>比较捕获通道2的DMA请求使能位。1:允许比较捕获通道2的DMA请求;0:禁止比较捕获通道2的DMA请求。</td><td>0</td></tr><tr><td>9</td><td>CC1DE</td><td>RW</td><td>比较捕获通道1的DMA请求使能位。1:允许比较捕获通道1的DMA请求;0:禁止比较捕获通道1的DMA请求。</td><td>0</td></tr><tr><td>8</td><td>UDE</td><td>RW</td><td>更新的DMA请求使能位。1:允许更新的DMA请求;0:禁止更新的DMA请求。</td><td>0</td></tr><tr><td>7</td><td>BIE</td><td>RW</td><td>刹车中断使能位。1:允许刹车中断;0:禁止刹车中断。</td><td>0</td></tr><tr><td>6</td><td>TIE</td><td>RW</td><td>触发中断使能位。1:使能触发中断;0:禁止触发中断。</td><td>0</td></tr><tr><td>5</td><td>COMIE</td><td>RW</td><td>COM中断允许位。1:允许COM中断;0:禁止COM中断。</td><td>0</td></tr><tr><td>4</td><td>CC4IE</td><td>RW</td><td>比较捕获通道4中断使能位。1:允许比较捕获通道4中断;0:禁止比较捕获通道4中断。</td><td>0</td></tr><tr><td>3</td><td>CC3IE</td><td>RW</td><td>比较捕获通道3中断使能位。1:允许比较捕获通道3中断;0:禁止比较捕获通道3中断。</td><td>0</td></tr><tr><td>2</td><td>CC2IE</td><td>RW</td><td>比较捕获通道2中断使能位。1:允许比较捕获通道2中断;0:禁止比较捕获通道2中断。</td><td>0</td></tr><tr><td>1</td><td>CC1IE</td><td>RW</td><td>比较捕获通道1中断使能位。1:允许比较捕获通道1中断;0:禁止比较捕获通道1中断。</td><td>0</td></tr><tr><td>0</td><td>UIE</td><td>RW</td><td>更新中断使能位。</td><td>0</td></tr><tr><td></td><td></td><td></td><td>1: 允许更新中断;
0: 禁止更新中断。</td><td></td></tr></table>

### 14.4.5 中断状态寄存器（TIMx_INTFR）（x=1/8/9/10）

偏移地址： $0 \times 1 0$

15 14 13 12 11 10 9 8 7 6 5 4 3 2 1 0

<table><tr><td>Reserved</td><td>CC40F</td><td>CC30F</td><td>CC20F</td><td>CC10F</td><td>Reserved</td><td>BIF</td><td>TIF</td><td>COMIF</td><td>CC4IF</td><td>CC3IF</td><td>CC2IF</td><td>CC1IF</td><td>UIF</td></tr></table>

<table><tr><td>位</td><td>名称</td><td>访问</td><td>描述</td><td>复位值</td></tr><tr><td>[15:13]</td><td>Reserved</td><td>R0</td><td>保留。</td><td>0</td></tr><tr><td>12</td><td>CC40F</td><td>RWO</td><td>比较捕获通道4重复捕获标志位。</td><td>0</td></tr><tr><td>11</td><td>CC30F</td><td>RWO</td><td>比较捕获通道3重复捕获标志位。</td><td>0</td></tr><tr><td>10</td><td>CC20F</td><td>RWO</td><td>比较捕获通道2重复捕获标志位。</td><td>0</td></tr><tr><td>9</td><td>CC10F</td><td>RWO</td><td>比较捕获通道1重复捕获标志位,仅用于比较捕获通道被配置为输入捕获模式时。该标记由硬件置位,软件写0可清除此位。1:计数器的值被捕获到捕获比较寄存器时,CC1IF的状态已经被置位;0:无重复捕获产生。</td><td>0</td></tr><tr><td>8</td><td>Reserved</td><td>R0</td><td>保留。</td><td>0</td></tr><tr><td>7</td><td>BIF</td><td>RWO</td><td>刹车中断标志位,一旦刹车输入有效,由硬件对该位置位,可由软件清零。1:刹车引脚输入上检测到设定的有效电平;0:无刹车事件产生。</td><td>0</td></tr><tr><td>6</td><td>TIF</td><td>RWO</td><td>触发器中断标志位,当发生触发事件时由硬件对该位置位,由软件清零。触发事件包括从除门控模式外的其它模式时,在TRGI输入端检测到有效边沿,或门控模式下的任一边沿。1:触发器事件产生;0:无触发器事件产生。</td><td>0</td></tr><tr><td>5</td><td>COMIF</td><td>RWO</td><td>COM中断标志位,一旦产生COM事件,该位由硬件置位,由软件清零。COM事件包括CCxE、CCxNE、OCxM被更新。1:COM事件产生;0:无COM事件产生。</td><td>0</td></tr><tr><td>4</td><td>CC4IF</td><td>RWO</td><td>比较捕获通道4中断标志位。</td><td>0</td></tr><tr><td>3</td><td>CC3IF</td><td>RWO</td><td>比较捕获通道3中断标志位。</td><td>0</td></tr><tr><td>2</td><td>CC2IF</td><td>RWO</td><td>比较捕获通道2中断标志位。</td><td>0</td></tr><tr><td>1</td><td>CC1IF</td><td>RWO</td><td>比较捕获通道1中断标志位。如果比较捕获通道配置为输出模式:当计数器值与比较值匹配时该位由硬件置位,但在中心对称模式下除外。该位由软件清零。1:核心计数器的值与比较捕获寄存器1的值匹配;0:无匹配发生。如果比较捕获通道1配置为输入模式:</td><td>0</td></tr><tr><td></td><td></td><td></td><td>当捕获事件发生时该位由硬件置位,它由软件清零或通过读比较捕获寄存器清零。1:计数器值已被捕获比较捕获寄存器1;0:无输入捕获产生。</td><td></td></tr><tr><td>0</td><td>UIF</td><td>RWO</td><td>更新中断标志位,当产生更新事件时该位由硬件置位,由软件清零。1:更新中断产生;0:无更新事件产生。以下情形会产生更新事件:若UDIS=0,当重复计数器数值上溢或下溢时;若URS=0、UDIS=0,当置UG位时,或当通过软件对计数器核心计数器重新初始化时;若URS=0、UDIS=0,当计数器CNT被触发事件重新初始化时;</td><td>0</td></tr></table>

### 14.4.6 事件产生寄存器（TIMx_SWEVGR）（x=1/8/9/10）

偏移地址：0x14

<table><tr><td>15</td><td>14</td><td>13</td><td>12</td><td>11</td><td>10</td><td>9</td><td>8</td><td>7</td><td>6</td><td>5</td><td>4</td><td>3</td><td>2</td><td>1</td><td>0</td></tr><tr><td colspan="8">Reserved</td><td>BG</td><td>TG</td><td>COMG</td><td>CC4G</td><td>CC3G</td><td>CC2G</td><td>CC1G</td><td>UG</td></tr></table>

<table><tr><td>位</td><td>名称</td><td>访问</td><td>描述</td><td>复位值</td></tr><tr><td>[15:8]</td><td>Reserved</td><td>RO</td><td>保留。</td><td>0</td></tr><tr><td>7</td><td>BG</td><td>WO</td><td>刹车事件产生位,此位由软件置位和清零,用来产生一个刹车事件。1:产生一个刹车事件。此时MOE=0、BIF=1,若使能对应的中断和DMA,则产生相应的中断和DMA;0:无动作。</td><td>0</td></tr><tr><td>6</td><td>TG</td><td>WO</td><td>触发事件产生位,该位由软件置位,硬件清零,用于产生一个触发事件。1:产生一个触发事件,TIF被置位,若使能对应的中断和DMA,则产生相应的中断和DMA;0:无动作。</td><td>0</td></tr><tr><td>5</td><td>COMG</td><td>WO</td><td>比较捕获控制更新产生位。产生比较捕获控制更新事件。该位由软件置位,由硬件自动清零。1:当CCPC=1,允许更新CCxE、CCxNE、OCxM位;0:无动作。注:该位只对拥有互补输出的通道(通道1,2,3)有效。</td><td>0</td></tr><tr><td>4</td><td>CC4G</td><td>WO</td><td>比较捕获事件产生位4。产生比较捕获事件4。</td><td>0</td></tr><tr><td>3</td><td>CC3G</td><td>WO</td><td>比较捕获事件产生位3。产生比较捕获事件3。</td><td>0</td></tr><tr><td>2</td><td>CC2G</td><td>WO</td><td>比较捕获事件产生位2。产生比较捕获事件2。</td><td>0</td></tr><tr><td>1</td><td>CC1G</td><td>WO</td><td>比较捕获事件产生位1,产生比较捕获事件1。该位由软件置位,由硬件清零。用于产生一个比较捕获事件。1:在比较捕获通道1上产生一个比较捕获事件:</td><td>0</td></tr><tr><td></td><td></td><td></td><td>若比较捕获通道1配置为输出:置CC1IF位。若使能对应的中断和DMA,则产生相应的中断和DMA;若比较捕获通道1配置为输入:当前核心计数器的值被捕获至比较捕获寄存器1;置CC1IF位,若使能了对应的中断和DMA,则产生相应的中断和DMA。若CC1IF已经置位,则置CC1OF位。0:无动作。</td><td></td></tr><tr><td>0</td><td>UG</td><td>WO</td><td>更新事件产生位,产生更新事件。该位由软件置位,由硬件自动清零。1:初始化计数器,并产生一个更新事件;0:无动作。注:预分频器的计数器也被清零,但是预分频系数不变。若在中心对称模式下或增计数模式下则核心计数器被清零;若减计数模式下则核心计数器取重装值寄存器的值。</td><td>0</td></tr></table>

### 14.4.7 比较/捕获控制寄存器 1（TIMx_CHCTLR1）（x=1/8/9/10）

偏移地址： $0 \times 1 8$

通道可用于输入(捕获模式)或输出(比较模式)，通道的方向由相应的 $\mathtt { C C } \mathtt { x } \mathtt { S }$ 位定义。该寄存器其它位的作用在输入和输出模式下不同。 ${ 0 0 } \times \times$ 描述了通道在输出模式下的功能， $1 0 \times \mathsf { x }$ 描述了通道在输入模式下的功能。

<table><tr><td>15</td><td>14</td><td>13</td><td>12</td><td>11</td><td>10</td><td>9</td><td>8</td><td>7</td><td>6</td><td>5</td><td>4</td><td>3</td><td>2</td><td>1</td><td>0</td></tr><tr><td>OC2CE</td><td colspan="3">OC2M[2:0]</td><td>OC2PE</td><td>OC2FE</td><td rowspan="2" colspan="2">CC2S[1:0]</td><td>OC1CE</td><td colspan="3">OC1M[2:0]</td><td>OC1PE</td><td>OC1FE</td><td rowspan="2" colspan="2">CC1S[1:0]</td></tr><tr><td colspan="4">IC2F[3:0]</td><td colspan="2">IC2PSC[1:0]</td><td colspan="4">IC1F[3:0]</td><td colspan="2">IC1PSC[1:0]</td></tr></table>

比较模式（引脚方向为输出）：

<table><tr><td>位</td><td>名称</td><td>访问</td><td>描述</td><td>复位值</td></tr><tr><td>15</td><td>OC2CE</td><td>RW</td><td>比较捕获通道2清零使能位。1:一旦检测到ETRF输入高电平,清除OC2REF位零;0:OC2REF不受ETRF输入的影响。</td><td>0</td></tr><tr><td>[14:12]</td><td>OC2M[2:0]</td><td>RW</td><td>比较捕获通道2模式设置域。该3位定义了输出参考信号OC2REF的动作,而OC2REF决定了OC2、OC2N的值。OC2REF是高电平有效,而OC2和OC2N的有效电平取决于CC2P、CC2NP位。000:冻结。比较捕获寄存器的值与核心计数器间的比较值对OC1REF不起作用;001:强制设为有效电平。当核心计数器与比较捕获寄存器1的值相同时,强制OC1REF为高;010:强制设为无效电平。当核心计数器的值与比较捕获寄存器1相同时,强制OC1REF为低;011:翻转。当核心计数器与比较捕获寄存器1的值相同时,翻转OC1REF的电平。</td><td>000b</td></tr><tr><td></td><td></td><td></td><td>100:强制为无效电平。强制OC1REF为低。101:强制为有效电平。强制OC1REF为高。110:PWM模式1:在向上计数时,一旦核心计数器大于比较捕获寄存器的值时通道1为无效电平,否则为有效电平;在向下计数时,一旦核心计数器大于比较捕获寄存器的值时通道1为有效电平,否则为无效电平。111:PWM模式2:在向上计数时,一旦核心计数器大于比较捕获寄存器的值时,通道1为有效电平,否则为无效电平;在向下计数时,一旦核心计数器大于比较捕获寄存器的值时,通道1为无效电平,否则为有效电平(OC1REF=1)。注:一旦LOCK级别设为3并且CC1S=00b则该位不能被修改。在PWM模式1或PWM模式2中,只有当比较结果改变了或在输出比较模式中从冻结模式切换到PWM模式时,OC1REF电平才改变。</td><td></td></tr><tr><td>11</td><td>OC2PE</td><td>RW</td><td>比较捕获寄存器2预装载使能位。1:开启比较捕获寄存器2的预装载功能,读写操作仅对预装载寄存器操作,比较捕获寄存器2的预装载值在更新事件到来时被加载至当前影子寄存器中;0:禁止比较捕获寄存器2的预装载功能,可随时写入比较捕获寄存器2,并且新写入的数值立即起作用。注:一旦LOCK级别设为3并且CC1S=00,则该位不能被修改;仅仅在单脉冲模式下(OPM=1)可以在未确认预装载寄存器情况下使用PWM模式,否则其动作不确定。</td><td>0</td></tr><tr><td>10</td><td>OC2FE</td><td>RW</td><td>比较捕获通道2快速使能位,该位用于加快比较捕获通道输出对触发输入事件的响应。1:输入到触发器的有效沿的作用就像发生了一次比较匹配。因此,OC被设置为比较电平而与比较结果无关。采样触发器的有效沿和比较捕获通道2输出间的延时被缩短为3个时钟周期;0:根据计数器与比较捕获寄存器2的值,比较捕获通道2正常操作,即使触发器是打开的。当触发器的输入有一个有效沿时,激活比较捕获通道2输出的最小延时为5个时钟周期。OC2FE只在通道被配置成PWM1或PWM2模式时起作用。</td><td>0</td></tr><tr><td>[9:8]</td><td>CC2S[1:0]</td><td>RW</td><td>比较捕获通道2输入选择域。00:比较捕获通道2被配置为输出;01:比较捕获通道2被配置为输入,IC2映射在TI2上;10:比较捕获通道2被配置为输入,IC2映射在TI1上;</td><td>00b</td></tr><tr><td></td><td></td><td></td><td>11:比较捕获通道2被配置为输入,IC2映射在TRC上。此模式仅工作在内部触发器输入被选中时(由TS位选择)。注:比较捕获通道2仅在通道关闭时(CC2E为零时)才是可写的。</td><td></td></tr><tr><td>7</td><td>OC1CE</td><td>RW</td><td>比较捕获通道1清零使能位。</td><td>0</td></tr><tr><td>[6:4]</td><td>OC1M[2:0]</td><td>RW</td><td>比较捕获通道1模式设置域。</td><td>0</td></tr><tr><td>3</td><td>OC1PE</td><td>RW</td><td>比较捕获寄存器1预装载使能位。</td><td>0</td></tr><tr><td>2</td><td>OC1FE</td><td>RW</td><td>比较捕获通道1快速使能位。</td><td>0</td></tr><tr><td>[1:0]</td><td>CC1S[1:0]</td><td>RW</td><td>比较捕获通道1输入选择域。</td><td>0</td></tr></table>

捕获模式（引脚方向为输入）：  

<table><tr><td>位</td><td>名称</td><td>访问</td><td>描述</td><td>复位值</td></tr><tr><td>[15:12]</td><td>IC2F[3:0]</td><td>RW</td><td>输入捕获滤波器2配置域,这几位设置了TI1输入的采样频率及数字滤波器长度。数字滤波器由一个事件计数器组成,它记录到N个事件后会产生一个输出的跳变。0000:无滤波器,以fDTS采样;1000:采样频率Fsampling=Fdts/8,N=6;0001:采样频率Fsampling=Fck_int,N=2;1001:采样频率Fsampling=Fdts/8,N=8;0010:采样频率Fsampling=Fck_int,N=4;1010:采样频率Fsampling=Fdts/16,N=5;0011:采样频率Fsampling=f=Fck_int,N=8;1011:采样频率Fsampling=Fdts/16,N=6;0100:采样频率Fsampling=Fdts/2,N=6;1100:采样频率Fsampling=Fdts/16,N=8;0101:采样频率Fsampling=Fdts/2,N=8;1101:采样频率Fsampling=Fdts/32,N=5;0110:采样频率Fsampling=Fdts/4,N=6;1110:采样频率Fsampling=Fdts/32,N=6;0111:采样频率Fsampling=Fdts/4,N=8;1111:采样频率Fsampling=Fdts/32,N=8。</td><td>0000b</td></tr><tr><td>[11:10]</td><td>IC2PSC[1:0]</td><td>RW</td><td>比较捕获通道2预分频配置域,这2位定义了比较捕获通道2的预分频系数。一旦CC1E=0,则预分频器复位。00:无预分频器,捕获输入口上检测到的每一个边沿都触发一次捕获;01:每2个事件触发一次捕获;10:每4个事件触发一次捕获;11:每8个事件触发一次捕获。</td><td>00b</td></tr><tr><td>[9:8]</td><td>CC2S[1:0]</td><td>RW</td><td>比较捕获通道2输入选择域,这2位定义通道的方向(输入/输出),及输入脚的选择。00:比较捕获通道1通道被配置为输出;01:比较捕获通道1通道被配置为输入,IC1映射</td><td>00b</td></tr><tr><td></td><td></td><td></td><td>在 TI1 上;10: 比较捕获通道 1 通道被配置为输入, IC1 映射在 TI2 上;11: 比较捕获通道 1 通道被配置为输入, IC1 映射在 TRC 上。此模式仅工作在内部触发器输入被选中时(由 TS 位选择)。注: CC1S 仅在通道关闭时 (CC1E 为 0) 才是可写的。</td><td></td></tr><tr><td>[7:4]</td><td>IC1F[3:0]</td><td>RW</td><td>输入捕获滤波器 1 配置域。</td><td>0</td></tr><tr><td>[3:2]</td><td>IC1PSC[1:0]</td><td>RW</td><td>比较捕获通道 1 预分频配置域。</td><td>0</td></tr><tr><td>[1:0]</td><td>CC1S[1:0]</td><td>RW</td><td>比较捕获通道 1 输入选择域。</td><td>0</td></tr></table>

### 14.4.8 比较/捕获控制寄存器 2（TIMx_CHCTLR2）（x=1/8/9/10）

偏移地址： ${ 0 } { \times 1 } { 0 }$

通道可用于输入(捕获模式)或输出(比较模式)，通道的方向由相应的 $\mathtt { C C } \mathtt { x } \mathtt { S }$ 位定义。该寄存器其它位的作用在输入和输出模式下不同。 ${ 0 0 } \times \times$ 描述了通道在输出模式下的功能， $1 0 \times \mathsf { x }$ 描述了通道在输入模式下的功能。

<table><tr><td>15</td><td>14</td><td>13</td><td>12</td><td>11</td><td>10</td><td>9</td><td>8</td><td>7</td><td>6</td><td>5</td><td>4</td><td>3</td><td>2</td><td>1</td><td>0</td></tr><tr><td>OC4CE</td><td colspan="2">OC4M[2:0]</td><td>OC4PE</td><td>OC4FE</td><td rowspan="2" colspan="2">CC4S[1:0]</td><td>OC3CE</td><td colspan="2">OC3M[2:0]</td><td>OC3PE</td><td colspan="2">OC3FE</td><td rowspan="2" colspan="2">CC3S[1:0]</td><td></td></tr><tr><td colspan="3">IC4F[3:0]</td><td colspan="2">IC4PSC[1:0]</td><td colspan="3">IC3F[3:0]</td><td colspan="3">IC3PSC[1:0]</td><td></td></tr></table>

比较模式（引脚方向为输出）：

<table><tr><td>位</td><td>名称</td><td>访问</td><td>描述</td><td>复位值</td></tr><tr><td>15</td><td>OC4CE</td><td>RW</td><td>比较捕获通道4清零使能位。</td><td>0</td></tr><tr><td>[14:12]</td><td>OC4M[2:0]</td><td>RW</td><td>比较捕获通道4模式设置域。</td><td>0</td></tr><tr><td>11</td><td>OC4PE</td><td>RW</td><td>比较捕获寄存器4预装载使能位。</td><td>0</td></tr><tr><td>10</td><td>OC4FE</td><td>RW</td><td>比较捕获通道4快速使能位。</td><td>0</td></tr><tr><td>[9:8]</td><td>CC4S[1:0]</td><td>RW</td><td>比较捕获通道4输入选择域。</td><td>0</td></tr><tr><td>7</td><td>OC3CE</td><td>RW</td><td>比较捕获通道3清零使能位。</td><td>0</td></tr><tr><td>[6:4]</td><td>OC3M[2:0]</td><td>RW</td><td>比较捕获通道3模式设置域。</td><td>0</td></tr><tr><td>3</td><td>OC3PE</td><td>RW</td><td>比较捕获寄存器3预装载使能位。</td><td>0</td></tr><tr><td>2</td><td>OC3FE</td><td>RW</td><td>比较捕获通道3快速使能位。</td><td>0</td></tr><tr><td>[1:0]</td><td>CC3S[1:0]</td><td>RW</td><td>比较捕获通道3输入选择域。</td><td>0</td></tr></table>

捕获模式（引脚方向为输入）：

<table><tr><td>位</td><td>名称</td><td>访问</td><td>描述</td><td>复位值</td></tr><tr><td>[15:12]</td><td>IC4F[3:0]</td><td>RW</td><td>输入捕获滤波器4配置域。</td><td>0</td></tr><tr><td>[11:10]</td><td>IC4PSC[1:0]</td><td>RW</td><td>比较捕获通道4预分频配置域。</td><td>0</td></tr><tr><td>[9:8]</td><td>CC4S[1:0]</td><td>RW</td><td>比较捕获通道4输入选择域。</td><td>0</td></tr><tr><td>[7:4]</td><td>IC3F[3:0]</td><td>RW</td><td>输入捕获滤波器3配置域。</td><td>0</td></tr><tr><td>[3:2]</td><td>IC3PSC[1:0]</td><td>RW</td><td>比较捕获通道3预分频配置域。</td><td>0</td></tr><tr><td>[1:0]</td><td>CC3S[1:0]</td><td>RW</td><td>比较捕获通道3输入选择域。</td><td>0</td></tr></table>

### 14.4.9 比较/捕获使能寄存器（TIMx_CCER）（x=1/8/9/10）

偏移地址： $_ { 0 \times 2 0 }$

<table><tr><td>Reserved</td><td>CC4P</td><td>CC4E</td><td>CC3NP</td><td>CC3NE</td><td>CC3P</td><td>CC3E</td><td>CC2NP</td><td>CC2NE</td><td>CC2P</td><td>CC2E</td><td>CC1NP</td><td>CC1NE</td><td>CC1P</td><td>CC1E</td></tr></table>

<table><tr><td>位</td><td>名称</td><td>访问</td><td>描述</td><td>复位值</td></tr><tr><td>[15:14]</td><td>Reserved</td><td>R0</td><td>保留。</td><td>0</td></tr><tr><td>13</td><td>CC4P</td><td>RW</td><td>比较捕获通道4输出极性设置位。</td><td>0</td></tr><tr><td>12</td><td>CC4E</td><td>RW</td><td>比较捕获通道4输出使能位。</td><td>0</td></tr><tr><td>11</td><td>CC3NP</td><td>RW</td><td>比较捕获通道3互补输出极性设置位。</td><td>0</td></tr><tr><td>10</td><td>CC3NE</td><td>RW</td><td>比较捕获通道3互补输出使能位。</td><td>0</td></tr><tr><td>9</td><td>CC3P</td><td>RW</td><td>比较捕获通道3输出极性设置位。</td><td>0</td></tr><tr><td>8</td><td>CC3E</td><td>RW</td><td>比较捕获通道3输出使能位。</td><td>0</td></tr><tr><td>7</td><td>CC2NP</td><td>RW</td><td>比较捕获通道2互补输出极性设置位。</td><td>0</td></tr><tr><td>6</td><td>CC2NE</td><td>RW</td><td>比较捕获通道2互补输出使能位。</td><td>0</td></tr><tr><td>5</td><td>CC2P</td><td>RW</td><td>比较捕获通道2输出极性设置位。</td><td>0</td></tr><tr><td>4</td><td>CC2E</td><td>RW</td><td>比较捕获通道2输出使能位。</td><td>0</td></tr><tr><td>3</td><td>CC1NP</td><td>RW</td><td>比较捕获通道1互补输出极性设置位。</td><td>0</td></tr><tr><td>2</td><td>CC1NE</td><td>RW</td><td>比较捕获通道1互补输出使能位。</td><td>0</td></tr><tr><td>1</td><td>CC1P</td><td>RW</td><td>比较捕获通道1输出极性设置位。CC1通道配置为输出:1:0C1低电平有效;0:0C1高电平有效。CC1通道配置为输入:该位选择是IC1还是IC1的反相信号作为触发或捕获信号。1:反相:捕获发生在IC1的下降沿;当用作外部触发器时,IC1反相。0:不反相:捕获发生在IC1的上升沿;当用作外部触发器时,IC1不反相。注:一旦LOCK级别(TIMx_BDTR寄存器中的LOCK位)设为3或2,则该位不能被修改。</td><td>0</td></tr><tr><td>0</td><td>CC1E</td><td>RW</td><td>比较捕获通道1输出使能位。CC1通道配置为输出:1:开启。0C1信号输出到对应的输出引脚,其输出电平依赖于MOE、OSSI、OSSR、OIS1、OIS1N和CC1NE位的值。0:关闭。0C1禁止输出,因此0C1的输出电平依赖于MOE、OSSI、OSSR、OIS1、OIS1N和CC1NE位的值。CC1通道配置为输入:该位决定了计数器的值是否能捕获入TIMx_CCR1寄存器。1:捕获使能;0:捕获禁止。</td><td>0</td></tr></table>

### 14.4.10 高级定时器的计数器（TIMx_CNT）（x=1/8/9/10）

偏移地址： ${ 0 } { \times 2 4 }$

<table><tr><td>15</td><td>14</td><td>13</td><td>12</td><td>11</td><td>10</td><td>9</td><td>8</td><td>7</td><td>6</td><td>5</td><td>4</td><td>3</td><td>2</td><td>1</td><td>0</td></tr><tr><td colspan="16">CNT[15:0]</td></tr></table>

<table><tr><td>位</td><td>名称</td><td>访问</td><td>描述</td><td>复位值</td></tr><tr><td>[15:0]</td><td>CNT[15:0]</td><td>RW</td><td>定时器的计数器的实时值。</td><td>0</td></tr></table>

### 14.4.11 计数时钟预分频器（TIMx_PSC）（x=1/8/9/10）

偏移地址： $_ { 0 \times 2 8 }$

<table><tr><td>15</td><td>14</td><td>13</td><td>12</td><td>11</td><td>10</td><td>9</td><td>8</td><td>7</td><td>6</td><td>5</td><td>4</td><td>3</td><td>2</td><td>1</td><td>0</td></tr><tr><td colspan="16">PSC[15:0]</td></tr></table>

<table><tr><td>位</td><td>名称</td><td>访问</td><td>描述</td><td>复位值</td></tr><tr><td>[15:0]</td><td>PSC[15:0]</td><td>RW</td><td>定时器的预分频器的分频系数；计数器的时钟频率等于分频器的输入频率/(PSC+1)。</td><td>0</td></tr></table>

### 14.4.12 自动重装值寄存器（TIMx_ATRLR）（x=1/8/9/10）

偏移地址： $\mathtt { 0 } \mathtt { x } 2 0$

<table><tr><td>15</td><td>14</td><td>13</td><td>12</td><td>11</td><td>10</td><td>9</td><td>8</td><td>7</td><td>6</td><td>5</td><td>4</td><td>3</td><td>2</td><td>1</td><td>0</td></tr><tr><td colspan="16">ARR[15:0]</td></tr></table>

<table><tr><td>位</td><td>名称</td><td>访问</td><td>描述</td><td>复位值</td></tr><tr><td>[15:0]</td><td>ARR[15:0]</td><td>RW</td><td>此域的值将会被装入计数器，ATRLR何时动作和更新见14.2.3章节；ATRLR为空时，计数器停止。</td><td>0</td></tr></table>

### 14.4.13 重复计数值寄存器（TIMx_RPTCR）（x=1/8/9/10）

偏移地址： $\mathtt { 0 } { \times } 3 0$

<table><tr><td>15</td><td>14</td><td>13</td><td>12</td><td>11</td><td>10</td><td>9</td><td>8</td><td>7</td><td>6</td><td>5</td><td>4</td><td>3</td><td>2</td><td>1</td><td>0</td></tr><tr><td colspan="8">Reserved</td><td colspan="8">REP[7:0]</td></tr></table>

<table><tr><td>位</td><td>名称</td><td>访问</td><td>描述</td><td>复位值</td></tr><tr><td>[15:8]</td><td>Reserved</td><td>R0</td><td>保留。</td><td>0</td></tr><tr><td>[7:0]</td><td>REP[7:0]</td><td>RW</td><td>重复计数器的值。</td><td>0</td></tr></table>

### 14.4.14 比较/捕获寄存器 1（TIMx_CH1CVR）（x=1/8/9/10）

偏移地址： $0 \times 3 4$

<table><tr><td>15</td><td>14</td><td>13</td><td>12</td><td>11</td><td>10</td><td>9</td><td>8</td><td>7</td><td>6</td><td>5</td><td>4</td><td>3</td><td>2</td><td>1</td><td>0</td></tr><tr><td colspan="16">CCR1[15:0]</td></tr></table>

<table><tr><td>位</td><td>名称</td><td>访问</td><td>描述</td><td>复位值</td></tr><tr><td>[15:0]</td><td>CCR1[15:0]</td><td>RW</td><td>比较捕获寄存器通道1的值。</td><td>0</td></tr></table>

### 14.4.15 比较/捕获寄存器 2（TIMx_CH2CVR）（x=1/8/9/10）

偏移地址： ${ 0 } { \times } \ 3 { 8 }$

<table><tr><td>15</td><td>14</td><td>13</td><td>12</td><td>11</td><td>10</td><td>9</td><td>8</td><td>7</td><td>6</td><td>5</td><td>4</td><td>3</td><td>2</td><td>1</td><td>0</td></tr><tr><td colspan="16">CCR2[15:0]</td></tr></table>

<table><tr><td>位</td><td>名称</td><td>访问</td><td>描述</td><td>复位值</td></tr><tr><td>[15:0]</td><td>CCR2[15:0]</td><td>RW</td><td>比较捕获寄存器通道2的值。</td><td>0</td></tr></table>

### 14.4.16 比较/捕获寄存器 3（TIMx_CH3CVR）（x=1/8/9/10）

偏移地址： $\mathtt { 0 } \mathtt { x } \mathtt { 3 0 }$

<table><tr><td>15</td><td>14</td><td>13</td><td>12</td><td>11</td><td>10</td><td>9</td><td>8</td><td>7</td><td>6</td><td>5</td><td>4</td><td>3</td><td>2</td><td>1</td><td>0</td></tr><tr><td colspan="16">CCR3[15:0]</td></tr></table>

<table><tr><td>位</td><td>名称</td><td>访问</td><td>描述</td><td>复位值</td></tr><tr><td>[15:0]</td><td>CCR3[15:0]</td><td>RW</td><td>比较捕获寄存器通道3的值。</td><td>0</td></tr></table>

### 14.4.17 比较/捕获寄存器 4（TIMx_CH4CVR）（x=1/8/9/10）

偏移地址： $0 \times 4 0$

<table><tr><td>15</td><td>14</td><td>13</td><td>12</td><td>11</td><td>10</td><td>9</td><td>8</td><td>7</td><td>6</td><td>5</td><td>4</td><td>3</td><td>2</td><td>1</td><td>0</td></tr><tr><td colspan="16">CCR4[15:0]</td></tr></table>

<table><tr><td>位</td><td>名称</td><td>访问</td><td>描述</td><td>复位值</td></tr><tr><td>[15:0]</td><td>CCR4[15:0]</td><td>RW</td><td>比较捕获寄存器通道4的值。</td><td>0</td></tr></table>

### 14.4.18 刹车和死区寄存器（TIMx_BDTR）（x=1/8/9/10）

偏移地址： $0 \times 4 4$

<table><tr><td>15</td><td>14</td><td>13</td><td>12</td><td>11</td><td>10</td><td>9</td><td>8</td><td>7</td><td>6</td><td>5</td><td>4</td><td>3</td><td>2</td><td>1</td><td>0</td></tr><tr><td>MOE</td><td>AOE</td><td>BKP</td><td>BKE</td><td>OSSR</td><td>OSSI</td><td colspan="2">LOCK[1:0]</td><td colspan="8">DTG[7:0]</td></tr></table>

<table><tr><td>位</td><td>名称</td><td>访问</td><td>描述</td><td>复位值</td></tr><tr><td>15</td><td>MOE</td><td>RW</td><td>主输出使能位。一旦刹车信号有效,将被异步清零。1:允许OCx和OCxN设为输出;0:禁止OCx和OCxN的输出或者强制为空闲状态。</td><td>0</td></tr><tr><td>14</td><td>AOE</td><td>RW</td><td>自动输出使能。1:MOE可以被软件置位或者在下一个更新事件中被置位;0:MOE只能被软件置位。</td><td>0</td></tr><tr><td>13</td><td>BKP</td><td>RW</td><td>刹车输入极性设置位。</td><td>0</td></tr><tr><td></td><td></td><td></td><td>1:刹车输入高电平有效;0:刹车输入低电平有效。注:当设置了 LOCK 级别 1 后,该位不能被修改。对该位的写需要一个 APB 时钟以后才能生效。</td><td></td></tr><tr><td>12</td><td>BKE</td><td>RW</td><td>刹车功能使能位。1:开启刹车输入;0:禁止刹车输入。注:当设置了 LOCK 级别 1 后,该位不能被修改。对该位的写需要一个 APB 时钟以后才能生效。</td><td>0</td></tr><tr><td>11</td><td>OSSR</td><td>RW</td><td>1:当定时器不工作时,一旦 CCxE=1 或 CCxNE=1,首先开启 OC/OCN 并输出无效电平,然后置 OCx、OCxN 使能输出信号=1;0:当定时器不工作时,禁止 OC/OCN 输出。注:当设置了 LOCK 级别 1 后,该位不能被修改。</td><td>0</td></tr><tr><td>10</td><td>OSSI</td><td>RW</td><td>1:当定时器不工作时,一旦 CCxE=1 或 CCxNE=1,OC/OCN 首先输出其空闲电平,然后 OCx、OCxN 使能输出信号=1;0:当定时器不工作时,禁止 OC/OCN 输出。注:当设置了 LOCK 级别 1 后,该位不能被修改。</td><td>0</td></tr><tr><td>[9:8]</td><td>LOCK[1:0]</td><td>RW</td><td>锁定功能设置域。00:关闭锁定功能;01:锁定级别 1,不能写 DTG、BKE、BKP、AOE、OISx和 OISxN 位;10:锁定级别 2,不能写入锁定级别 1 中的各位,也不能写入 CC 极性位以及 OSSR 和 OSSI 位;11:锁定级别 3,不能写入锁定级别 2 中的各位,也不能写入 CC 控制位。注:在系统复位后,只能写一次 LOCK 位,无法再次修改直到复位。</td><td>00b</td></tr><tr><td>[7:0]</td><td>DTG[7:0]</td><td>RW</td><td>死区设置位,这些位定义了互补输出之间的死区持续时间。假设 DT 表示其持续时间:DTG[7:5]=0xx=&gt;DT=DTG[7:0]*Tdtg,Tdtg=TDDS;DTG[7:5]=10x=&gt;DT=(64+DTG[5:0])*Tdtg,Tdtg=2*TDDS;DTG[7:5]=110=&gt;DT=(32+DTG[4:0])*Tdtg,Tdtg=8×TDDS;DTG[7:5]=111=&gt;DT=(32+DTG[4:0])*Tdtg,Tdtg=16*TDDS。</td><td>0</td></tr></table>

### 14.4.19 DMA 控制寄存器（TIMx_DMACFGR）（x=1/8/9/10）

偏移地址： $0 \times 4 8$

<table><tr><td>Reserved</td><td>DBL[4:0]</td><td>Reserved</td><td>DBA[4:0]</td></tr></table>

<table><tr><td>位</td><td>名称</td><td>访问</td><td>描述</td><td>复位值</td></tr><tr><td>[15:13]</td><td>Reserved</td><td>R0</td><td>保留。</td><td>0</td></tr><tr><td>[12:8]</td><td>DBL[4:0]</td><td>RW</td><td>DMA连续传送的长度,实际值为此域的值+1。</td><td>0</td></tr><tr><td>[7:5]</td><td>Reserved</td><td>R0</td><td>保留。</td><td>0</td></tr><tr><td>[4:0]</td><td>DBA[4:0]</td><td>RW</td><td>这些位定义了DMA在连续模式下从控制寄存器1所在地址的偏移量。</td><td>0</td></tr></table>

### 14.4.20 连续模式的 DMA 地址寄存器（TIMx_DMAADR）（x=1/8/9/10）

偏移地址： $\mathtt { 0 } \mathtt { x } 4 0$

<table><tr><td>15</td><td>14</td><td>13</td><td>12</td><td>11</td><td>10</td><td>9</td><td>8</td><td>7</td><td>6</td><td>5</td><td>4</td><td>3</td><td>2</td><td>1</td><td>0</td></tr><tr><td colspan="16">DMAB[15:0]</td></tr></table>

<table><tr><td>位</td><td>名称</td><td>访问</td><td>描述</td><td>复位值</td></tr><tr><td>[15:0]</td><td>DMAB[15:0]</td><td>RW</td><td>连续模式下，DMA的地址。</td><td>0</td></tr></table>

# 第 15 章 通用定时器（GPTM）

本章模块描述适用于CH32F2x、CH32V2x和CH32V3x微控制器全系列产品。

通用定时器模块包含一个16位可自动重装的定时器（TIM2、TIM3、TIM4 和 TIM5），用于测量脉冲宽度或者产生特定频率的脉冲、PWM波等。可用于自动化控制、电源等领域。

## 15.1 主要特征

通用定时器的主要特征包括：

16 位自动重装计数器，支持增计数模式，减计数模式和增减计数模式  
$\bullet$ 16 位预分频器，分频系数从 $1 \sim 6 5 5 3 6$ 之间动态可调  
$\bullet$ 支持四路独立的比较捕获通道  
$\bullet$ 每路比较捕获通道支持多种工作模式，比如：输入捕获、输出比较、PWM 生成和单脉冲输出  
$\bullet$ 支持外部信号控制定时器  
$\bullet$ 支持在多种模式下使用DMA  
支持增量式编码，定时器之间的级联和同步

## 15.2 原理和结构

![](images/c76fee5c37b0fea124fa4351acc960a1b1f32411a8cb0606cf0b0b467b321dbf.jpg)  
图 15-1 通用定时器的结构框图

### 15.2.1 概述

如图 15-1 所示，通用定时器的结构大致可以分为三部分，即输入时钟部分，核心计数器部分和比较捕获通道部分。

通用定时器的时钟可以来自于AHB总线时钟（CK_INT），可以来自外部时钟输入引脚（TIMx_ETR），可以来自于其他具有时钟输出功能的定时器（ITRx），还可以来自于比较捕获通道的输入端（TIMx_CHx）。这些输入的时钟信号经过各种设定的滤波分频等操作后成为 CK_PSC 时钟，输出给核心计数器部分。另外，这些复杂的时钟来源还可以作为TRGO输出给其他的定时器、ADC和 DAC等外设。

通用定时器的核心是一个 16 位计数器（CNT）。CK_PSC 经过预分频器（PSC）分频后，成为 CK_CNT

再最终输给 CNT，CNT 支持增计数模式、减计数模式和增减计数模式，并有一个自动重装值寄存器（ATRLR）在每个计数周期结束后为CNT重装载初始化值。

通用定时器拥有四组比较捕获通道，每组比较捕获通道都可以从专属的引脚上输入脉冲，也可以向引脚输出波形，即比较捕获通道支持输入和输出模式。比较捕获寄存器每个通道的输入都支持滤波、分频、边沿检测等操作，并支持通道间的互触发，还能为核心计数器 CNT提供时钟。每个比较捕获通道都拥有一组比较捕获寄存器（CHxCVR），支持与主计数器（CNT）进行比较而输出脉冲。

### 15.2.2 通用定时器和高级定时器的区别

与高级定时器相比，通用定时器缺少以下功能：

1） 通用定时器缺少对核心计数器的计数周期进行计数的重复计数寄存器。  
2） 通用定时器的比较捕获通道缺少死区产生，没有互补输出。  
3） 通用定时器没有刹车信号机制。  
4） 通用定时器的默认时钟 CK_INT 都来自 APB1，而高级定时器的 CK_INT 都来自 APB2。

### 15.2.3 时钟输入

本节论述 CK_PSC 的来源。此处截取通用定时器的整体结构框图的时钟源部分。

![](images/903a0dc5ed8ba185f42d542e0903661d28bed1d89a4783c9ed1ca5d86c8d5cd1.jpg)  
图 15-2 通用定时器 CK_PSC 来源框图

可选的输入时钟可以分为 4 类：

1) 外部时钟引脚（ETR）输入的路线：ETR→ETRP→ETRF；  
2) 内部 APB 时钟输入路线：CK_INT；  
3) 来自比较捕获通道引脚（TIMx_CHx）的路线：TIMx_CHx→TIx→TIxFPx，此路线也用于编码器模式；  
4) 来自内部其他定时器的输入：ITRx。

通过决定 CK_PSC 来源的 SMS 的输入脉冲选择可以将实际的操作分为三类：

1) 选择内部时钟源（CK_INT）；  
2) 外部时钟源模式 1；  
3) 外部时钟源模式 2；  
4) 编码器模式。

上文提到的 4 种时钟源来源都可通过这 4 种操作选定。

#### 15.2.3.1 内部时钟源（CK_INT）

如果将 SMS 域保持为 000b 时启动通用定时器，那么就是选定内部时钟源（CK_INT）为时钟。此时 CK_INT 就是 CK_PSC。

#### 15.2.3.2 外部时钟源模式 1

如果将 SMS 域设置为 111b 时，就会启用外部时钟源模式 1。启用外部时钟源 1 时，TRGI 被选定为 CK_PSC 的来源，值得注意的，用户还需要通过配置 TS 域来选择 TRGI 的来源。TS 域可选择以下几种脉冲作为时钟来源：

1） 内部触发（ITRx，x 为 0,1,2,3）；  
2） 比较捕获通道 1 经过边缘检测器后的信号（TI1F_ED）；  
3） 比较捕获通道的信号 TI1FP1、TI2FP2；  
4） 来自外部时钟引脚输入的信号 ETRF。

#### 15.2.3.3 外部时钟源模式 2

使用外部触发模式2能在外部时钟引脚输入的每一个上升沿或下降沿计数。将 ECE位置位时，将使用外部时钟源模式2。使用外部时钟源模式 2时，ETRF 被选定为 CK_PSC。ETR引脚经过可选的反相器（ETP），分频器（ETPS）后成为ETRP，再经过滤波器（ETF）后即成为 ETRF。

在 ECE 位置位且将 SMS 设为 111b 时，那么，相当于 TS 选择 ETRF 为输入。

#### 15.2.3.4 编码器模式

将 SMS 置为 001b，010b，011b 将会启用编码器模式。启用编码器模式可以选择在 TI1FP1 和 TI2FP2中某一个特定的电平下以另一个跳变沿作为信号进行信号输出。此模式用于外接编码器使用的情况下。具体功能参考14.3.9节。

### 15.2.4 计数器和周边

CK_PSC输入给预分频器（PSC）进行分频。PSC是 16位的，实际的分频系数相当于 R16_TIMx_PSC的值 $+ 1$ 。CK_PSC 经过 PSC 会成为 CK_INT。更改 R16_TIM1_PSC 的值并不会实时生效，而会在更新事件后更新给PSC。更新事件包括UG位清零和复位。

### 15.2.5 比较捕获通道

比较捕获通道是定时器实现复杂功能的核心，它的核心是比较捕获寄存器，辅以外围输入部分的数字滤波，分频和通道间复用，输出部分的比较器和输出控制组成。比较捕获通道的结构框图如图 15-3所示。

![](images/197b8a63ba3d27039e91342f5110cf7c167ac828e21d5ebf6ef7bd22f9be56cf.jpg)  
图15-3 比较捕获通道的结构框图

![](images/fc6bd6cb4e4fb8a849a2735360564190b06c49ff80b05b6e6f994e4bf2bfc6ee.jpg)

![](images/ee7e2214a0631081d845884b7dcf1d2c6103f9af3c335947100073bdd2be79dd.jpg)

信号从通道 $\mathsf { x }$ 引脚输入进来后可选做为 TIx（TI1 的来源可以不只是 CH1，见定时器的框图 14-1），TI1 经过滤波器（ICF[3:0]）生成 TI1F，再经过边沿检测器分成 TI1F_Rising 和 TI1F_Falling，这两个信号经过选择（CC1P）生成 TI1FP1，TI1FP1 和来自通道 2 的 TI2FP1 一起送给 CC1S 选择成为IC1，经过ICPS分频后送给比较捕获寄存器。

比较捕获寄存器由一个预装载寄存器和一个影子寄存器组成，读写过程仅操作预装载寄存器。在捕获模式下，捕获发生在影子寄存器上，然后复制到预装载寄存器；在比较模式下，预装载寄存器的内容被复制到影子寄存器中，然后影子寄存器的内容与核心计数器（CNT）进行比较。

## 15.3 功能和实现

通用定时器复杂功能的实现都是对定时器的比较捕获通道、时钟输入电路和计数器及周边组件进行操作实现的。定时器的时钟输入可以来自于包括比较捕获通道的输入在内的多个时钟源。对比较捕

获寄存通道和时钟源选择的操作直接决定其功能。比较捕获通道是双向的，可以工作在输入和输出模式。

### 15.3.1 输入捕获模式

输入捕获模式是定时器的基本功能之一。输入捕获模式的原理是，当检测到 ${ \mathsf { I C X P S } }$ 信号上确定的边沿后，则产生捕获事件，计数器当前的值会被锁存到比较捕获寄存器（R16_TIMx_CHCTLRx）中。发生捕获事件时， ${ \tt C C } \times { \tt I F }$ （在R16_TIMx_INTFR中）被置位，如果使能了中断或者 DMA，还会产生相应中断或者DMA。如果发生捕获事件时， ${ \mathsf { C C } } \times { \mathsf { I F } }$ 已经被置位了，那么 $\mathtt { C C } \mathtt { x 0 F }$ 位会被置位。CCxIF 可由软件清除，也可以通过读取比较捕获寄存器由硬件清除。 ${ \tt C C } \tt { x O F }$ 由软件清除。

举个通道 1 的例子来说明使用输入捕获模式的步骤，如下：

1） 配置 $\mathtt { C C } \mathtt { x } \mathtt { S }$ 域，选择 $1 0 \times$ 信号的来源。比如设为 10b，选择 TI1FP1 作为 IC1的来源，不可以使用默认设置， $\mathtt { C C } \mathtt { x } \mathtt { S }$ 域默认是使比较捕获模块作为输出通道；  
2） 配置 ${ | 0 \times | }$ 域，设定 TI 信号的数字滤波器。数字滤波器会以确定的频率，采样确定的次数，再输出一个跳变。这个采样频率和次数是通过 ${ | 0 \times | }$ 来确定的；  
3） 配置 $\mathtt { C C } \mathtt { x } \mathtt { P }$ 位，设定 TIxFPx 的极性。比如保持 CC1P 位为低，选择上升沿跳变；  
4） 配置 ${ \mathsf { I C X P S } }$ 域，设定 $1 0 \times$ 信号成为 ${ \mathsf { I C X P S } }$ 之间的分频系数。比如保持 ${ \mathsf { I C X P S } }$ 为 00b，不分频；  
5） 配置 $\mathtt { C C } \mathtt { x } \mathtt { E }$ 位，允许捕获核心计数器（CNT）的值到比较捕获寄存器中。置 CC1E位；  
6） 根据需要配置 $\mathsf { C C } \mathsf { \times } { \mathsf { I E } }$ 和 $\complement 0 \times \mathsf { D E }$ 位，决定是否允许使能中断或者 DMA。

至此已经将比较捕获通道配置完成。

当 TI1 输入了一个被捕获的脉冲时，核心计数器（CNT）的值会被记录到比较捕获寄存器中，CC1IF被置位，当CC1IF在之前就已经被置位时，CCIOF位也会被置位。如果 CC1IE位，那么会产生一个中断；如果CC1DE被置位，会产生一个DMA请求。可以通过写事件产生寄存器的方式（R16_TIMx_SWEVGR）的方式由软件产生一个输入捕获事件。

### 15.3.2 比较输出模式

比较输出模式是定时器的基本功能之一。比较输出模式的原理是在核心计数器（CNT）的值与比较捕获寄存器的值一致时，输出特定的变化或波形。 $0 0 \times 1 1$ 域（在 R16_TIMx_CHCTLRx 中）和 $\complement 0 \times \emptyset$ 位（在 R16_TIMx_CCER 中）决定输出的是确定的高低电平还是电平翻转。产生比较一致事件时还会置CCxIF位，如果预先置了 $\mathsf { C C } \mathsf { \times } { \mathsf { I E } }$ 位，则会产生一个中断；如果预先设置了 $\complement 0 \times \mathsf { D E }$ 位，则会产生一个 DMA请求。

配置为比较输出模式的步骤为下：

1） 配置核心计数器（CNT）的时钟源和自动重装值；  
2） 设置好需要对比的计数值到比较捕获寄存器（R16_TIMx_CHxCVR）中；  
3） 如果需要产生中断，置 $\complement 0 \times 1 \mathsf { E }$ 位；  
4） 保持 ${ \tt O C } \tt { x P E }$ 为 0，禁用比较捕获寄存器的预装载寄存器；  
5） 设定输出模式，设置 $0 0 \times 1 1$ 域和 $\complement 0 \times \emptyset$ 位；  
6） 使能输出，置 $\mathtt { C C } \mathtt { x } \mathtt { E }$ 位；  
7） 置 CEN 位启动定时器；

### 15.3.3 强制输出模式

定时器的比较捕获通道的输出模式可以由软件强制输出确定的电平，而不依赖比较捕获寄存器的影子寄存器和核心计数器的比较。

具体的做法是将 $0 0 \times 1 1$ 置为 100b，即为强制将 OCxREF 置为低；或者将 $0 0 \times 1 1$ 置为 101b，即为强制将 ${ 0 0 } \times { \mathsf { R E F } }$ 置为高。

需要注意的是，将 $0 0 \times 1 1$ 强制置为 100b 或者 101b，内部主计数器和比较捕获寄存器的比较过程还在进行，相应的标志位还在置位，中断和 DMA请求还在产生。

### 15.3.4 PWM 输入模式

PWM 输入模式是用来测量PWM的占空比和频率的，是输入捕获模式的一种特殊情况。除下列区别外，操作和输入捕获模式相同：PWM占用两个比较捕获通道，且两个通道的输入极性设为相反，其中一个信号被设为触发输入，SMS设为复位模式。

例如，测量从 TI1 输入的 PWM 波的周期和频率，需要进行以下操作：

1） 将 TI1(TI1FP1)设为 IC1 信号的输入。将 CC1S 置为 01b；  
2） 将 TI1FP1 置为上升沿有效。将 CC1P 保持为 0；  
3） 将 TI1(TI1FP2)置为 IC2 信号的输入。将 CC2S 置为 10b；  
4） 选 TI1FP2 置为下降沿有效。将 CC2P 置为 1；  
5） 时钟源的来源选择 TI1FP1。将 TS 设为 101b；  
6） 将 SMS 设为复位模式，即 100b；  
7） 使能输入捕获。CC1E 和 CC2E 置位。

### 15.3.5 PWM 输出模式

PWM 输出模式是定时器的基本功能之一。PWM 输出模式最常见的是使用重装值确定 PWM 频率，使用捕获比较寄存器确定占空比的方法。将 $0 0 \times 1 1$ 域中置 110b 或者 111b 使用 PWM 模式 1 或者模式 2，置 ${ 0 0 } \times { \mathsf E }$ 位使能预装载寄存器，最后置 ARPE 位使能预装载寄存器的自动重装载。在发生一个更新事件时，预装载寄存器的值才能被送到影子寄存器，所以在核心计数器开始计数之前，需要置 UG 位来初始化所有寄存器。在PWM模式下，核心计数器和比较捕获寄存器一直在进行比较，根据CMS位，定时器能够输出边沿对齐或者中央对齐的PWM信号。

#### 边沿对齐

使用边沿对齐时，核心计数器增计数或者减计数，在 PWM模式 1 的情景下，在核心计数器的值大于比较捕获寄存器时，OCxREF 上升为高；当核心计数器的值小于比较捕获寄存器时（比如核心计数器增长到 R16_TIMx_ATRLR 的值而恢复成全 0 时），OCxREF 下降为低。

#### 中央对齐

使用中央对齐模式时，核心计数器运行在增计数和减计数交替进行的模式下，OCxREF 在核心计数器和比较捕获寄存器的值一致时进行上升和下降的跳变。但比较标志在三种中央对齐模式下，置位的时机有所不同。在使用中央对齐模式时，最好在启动核心计数器之前产生一个软件更新标志（置 UG位）。

### 15.3.6 单脉冲模式

单脉冲模式可以响应一个特定的事件，在一个延迟之后产生一个脉冲，延迟和脉冲的宽度可编程。置 OPM 位可以使核心计数器在产生下一个更新事件 UEV 时（计数器翻转到 0）停止。

![](images/b536465a6571cf7893df95bf77e45a0369ff37918493ef88bbc4a07823cc34e7.jpg)  
图15-4 事件产生和脉冲响应

如图 15-4 所示，需要在 TI2 输入引脚上检测到一个上升沿开始，延迟 Tdelay 之后，在 OC1 上产生一个长度为 Tpulse 的正脉冲：

1） 设定 TI2 为触发。置 CC2S 域为 01b，把 TI2FP2 映射到 TI2；置 CC2P 位为 0b，TI2FP2 设为上升沿检测；置TS域为110b，TI2FP2设为触发源；置SMS 域为 110b，TI2FP2被用来启动计数器；  
2） Tdelay 由比较捕获寄存器定义，Tpulse 由自动重装值寄存器的值和比较捕获寄存器的值确定。

#### 15.3.7 编码器模式

编码器模式是定时器的一个典型应用，可以用来接入编码器的双相输出，核心计数器的计数方向和编码器的转轴方向同步，编码器每输出一个脉冲就会使核心计数器加一或减一。使用编码器的步骤为：将SMS域置为001b（只在TI2边沿计数）、010b（只在TI1边沿计数）或者 011b（在 TI1和 TI2双边沿计数），将编码器接到比较捕获通道 1、2 的输入端，设一个重装值计数器的值，这个值可以设的大一点。在编码器模式时，定时器内部的比较捕获寄存器，预分频器，重复计数寄存器等都正常工作。下表表明了计数方向和编码器信号的关系。

表 15-1 定时器编码器模式的计数方向和编码器信号之间的关系  

<table><tr><td rowspan="2">计数有效边沿</td><td rowspan="2">相对信号的电平</td><td colspan="2">TI1FP1 信号边沿</td><td colspan="2">TI2FP2 信号</td></tr><tr><td>上升沿</td><td>下降沿</td><td>上升沿</td><td>下降沿</td></tr><tr><td rowspan="2">仅在 TI1 边沿计数</td><td>高</td><td>向下计数</td><td>向上计数</td><td rowspan="2" colspan="2">不计数</td></tr><tr><td>低</td><td>向上计数</td><td>向下计数</td></tr><tr><td rowspan="2">仅在 TI2 边沿计数</td><td>高</td><td rowspan="2" colspan="2">不计数</td><td>向上计数</td><td>向下计数</td></tr><tr><td>低</td><td>向下计数</td><td>向上计数</td></tr><tr><td rowspan="2">在 TI1 和 TI2 双边沿计数</td><td>高</td><td>向下计数</td><td>向上计数</td><td>向上计数</td><td>向下计数</td></tr><tr><td>低</td><td>向上计数</td><td>向下计数</td><td>向下计数</td><td>向上计数</td></tr></table>

### 15.3.8 定时器同步模式

定时器能够输出时钟脉冲（TRGO），也能接收其他定时器的输入（ITRx）。不同的定时器的 ITRx的来源（别的定时器的TRGO）是不一样的。定时器内部触发连接如表 12-2所示。

表 15-2 GTPM 内部触发连接  

<table><tr><td>从定时器</td><td>ITR0 (TS=000)</td><td>ITR1 (TS=001)</td><td>ITR2 (TS=010)</td><td>ITR3 (TS=011)</td></tr><tr><td>TIM2</td><td>TIM1</td><td>TIM8/USB/ETH</td><td>TIM3</td><td>TIM4</td></tr><tr><td>TIM3</td><td>TIM1</td><td>TIM2</td><td>TIM5</td><td>TIM4</td></tr><tr><td>TIM4</td><td>TIM1</td><td>TIM2</td><td>TIM3</td><td>TIM8</td></tr><tr><td>TIM5</td><td>TIM2</td><td>TIM3</td><td>TIM4</td><td>TIM8</td></tr></table>

### 15.3.9 调试模式

当系统进入调试模式时，根据DBG模块的设置可以控制定时器继续运转或者停止。

## 15.4 寄存器描述

表 15-3 TIM2 相关寄存器列表  

<table><tr><td>名称</td><td>偏移地址</td><td>描述</td><td>复位值</td></tr><tr><td>R16_TIM2_CTLR1</td><td>0x40000000</td><td>TIM2 控制寄存器1</td><td>0x0000</td></tr><tr><td>R16_TIM2_CTLR2</td><td>0x40000004</td><td>TIM2 控制寄存器2</td><td>0x0000</td></tr><tr><td>R16_TIM2_SMCFGR</td><td>0x40000008</td><td>TIM2 从模式控制寄存器</td><td>0x0000</td></tr><tr><td>R16_TIM2_DMAINTENR</td><td>0x4000000C</td><td>TIM2 DMA/中断使能寄存器</td><td>0x0000</td></tr><tr><td>R16_TIM2_INTERR</td><td>0x40000010</td><td>TIM2 中断状态寄存器</td><td>0x0000</td></tr><tr><td>R16_TIM2_SWEVGR</td><td>0x40000014</td><td>TIM2 事件产生寄存器</td><td>0x0000</td></tr><tr><td>R16_TIM2_CHCTLR1</td><td>0x40000018</td><td>TIM2 比较/捕获控制寄存器1</td><td>0x0000</td></tr><tr><td>R16_TIM2_CHCTLR2</td><td>0x4000001C</td><td>TIM2 比较/捕获控制寄存器2</td><td>0x0000</td></tr><tr><td>R16_TIM2_CCER</td><td>0x40000020</td><td>TIM2 比较/捕获使能寄存器</td><td>0x0000</td></tr><tr><td>R16_TIM2_CNT</td><td>0x40000024</td><td>TIM2 计数器</td><td>0x0000</td></tr><tr><td>R16_TIM2_PSC</td><td>0x40000028</td><td>TIM2 计数时钟预分频器</td><td>0x0000</td></tr><tr><td>R16_TIM2_ATRLR</td><td>0x4000002C</td><td>TIM2 自动重装值寄存器</td><td>0x0000</td></tr><tr><td>R16_TIM2_CH1CVR</td><td>0x40000034</td><td>TIM2 比较/捕获寄存器1</td><td>0x0000</td></tr><tr><td>R16_TIM2_CH2CVR</td><td>0x40000038</td><td>TIM2 比较/捕获寄存器2</td><td>0x0000</td></tr><tr><td>R16_TIM2_CH3CVR</td><td>0x4000003C</td><td>TIM2 比较/捕获寄存器3</td><td>0x0000</td></tr><tr><td>R16_TIM2_CH4CVR</td><td>0x40000040</td><td>TIM2 比较/捕获寄存器4</td><td>0x0000</td></tr><tr><td>R16_TIM2_DMACFGR</td><td>0x40000048</td><td>TIM2 DMA 控制寄存器</td><td>0x0000</td></tr><tr><td>R16_TIM2_DMAADR</td><td>0x4000004C</td><td>TIM2 连续模式的 DMA 地址寄存器</td><td>0x0000</td></tr></table>

表 15-4 TIM3 相关寄存器列表  

<table><tr><td>名称</td><td>偏移地址</td><td>描述</td><td>复位值</td></tr><tr><td>R16_TIM3_CTLR1</td><td>0x40000400</td><td>TIM3 控制寄存器 1</td><td>0x0000</td></tr><tr><td>R16_TIM3_CTLR2</td><td>0x40000404</td><td>TIM3 控制寄存器 2</td><td>0x0000</td></tr><tr><td>R16_TIM3_SMCFGR</td><td>0x40000408</td><td>TIM3 从模式控制寄存器</td><td>0x0000</td></tr><tr><td>R16_TIM3_DMAINTENR</td><td>0x4000040C</td><td>TIM3 DMA/中断使能寄存器</td><td>0x0000</td></tr><tr><td>R16_TIM3_INTFR</td><td>0x40000410</td><td>TIM3 中断状态寄存器</td><td>0x0000</td></tr><tr><td>R16_TIM3_SWEVGR</td><td>0x40000414</td><td>TIM3 事件产生寄存器</td><td>0x0000</td></tr><tr><td>R16_TIM3_CHCTLR1</td><td>0x40000418</td><td>TIM3 比较/捕获控制寄存器 1</td><td>0x0000</td></tr><tr><td>R16_TIM3_CHCTLR2</td><td>0x4000041C</td><td>TIM3 比较/捕获控制寄存器 2</td><td>0x0000</td></tr><tr><td>R16_TIM3_CCER</td><td>0x40000420</td><td>TIM3 比较/捕获使能寄存器</td><td>0x0000</td></tr><tr><td>R16_TIM3_CNT</td><td>0x40000424</td><td>TIM3 计数器</td><td>0x0000</td></tr><tr><td>R16_TIM3_PSC</td><td>0x40000428</td><td>TIM3 计数时钟预分频器</td><td>0x0000</td></tr><tr><td>R16_TIM3_ATRLR</td><td>0x4000042C</td><td>TIM3 自动重装值寄存器</td><td>0x0000</td></tr><tr><td>R16_TIM3_CH1CVR</td><td>0x40000434</td><td>TIM3 比较/捕获寄存器 1</td><td>0x0000</td></tr><tr><td>R16_TIM3_CH2CVR</td><td>0x40000438</td><td>TIM3 比较/捕获寄存器 2</td><td>0x0000</td></tr><tr><td>R16_TIM3_CH3CVR</td><td>0x4000043C</td><td>TIM3 比较/捕获寄存器 3</td><td>0x0000</td></tr><tr><td>R16_TIM3_CH4CVR</td><td>0x40000440</td><td>TIM3 比较/捕获寄存器 4</td><td>0x0000</td></tr><tr><td>R16_TIM3_DMACFGR</td><td>0x40000448</td><td>TIM3 DMA 控制寄存器</td><td>0x0000</td></tr><tr><td>R16_TIM3_DMAADR</td><td>0x4000044C</td><td>TIM3 连续模式的 DMA 地址寄存器</td><td>0x0000</td></tr></table>

表 15-5 TIM4 相关寄存器列表  

<table><tr><td>名称</td><td>偏移地址</td><td>描述</td><td>复位值</td></tr><tr><td>R16_TIM4_CTLR1</td><td>0x40000800</td><td>TIM4 控制寄存器1</td><td>0x0000</td></tr><tr><td>R16_TIM4_CTLR2</td><td>0x40000804</td><td>TIM4 控制寄存器2</td><td>0x0000</td></tr><tr><td>R16_TIM4_SMCFGR</td><td>0x40000808</td><td>TIM4 从模式控制寄存器</td><td>0x0000</td></tr><tr><td>R16_TIM4_DMAINTENR</td><td>0x4000080C</td><td>TIM4 DMA/中断使能寄存器</td><td>0x0000</td></tr><tr><td>R16_TIM4_INTFR</td><td>0x40000810</td><td>TIM4 中断状态寄存器</td><td>0x0000</td></tr><tr><td>R16_TIM4_SWEVGR</td><td>0x40000814</td><td>TIM4 事件产生寄存器</td><td>0x0000</td></tr><tr><td>R16_TIM4_CHCTLR1</td><td>0x40000818</td><td>TIM4 比较/捕获控制寄存器1</td><td>0x0000</td></tr><tr><td>R16_TIM4_CHCTLR2</td><td>0x4000081C</td><td>TIM4 比较/捕获控制寄存器2</td><td>0x0000</td></tr><tr><td>R16_TIM4_CCER</td><td>0x40000820</td><td>TIM4 比较/捕获使能寄存器</td><td>0x0000</td></tr><tr><td>R16_TIM4_CNT</td><td>0x40000824</td><td>TIM4 计数器</td><td>0x0000</td></tr><tr><td>R16_TIM4_PSC</td><td>0x40000828</td><td>TIM4 计数时钟预分频器</td><td>0x0000</td></tr><tr><td>R16_TIM4_ATRLR</td><td>0x4000082C</td><td>TIM4 自动重装值寄存器</td><td>0x0000</td></tr><tr><td>R16_TIM4_CH1CVR</td><td>0x40000834</td><td>TIM4 比较/捕获寄存器1</td><td>0x0000</td></tr><tr><td>R16_TIM4_CH2CVR</td><td>0x40000838</td><td>TIM4 比较/捕获寄存器2</td><td>0x0000</td></tr><tr><td>R16_TIM4_CH3CVR</td><td>0x4000083C</td><td>TIM4 比较/捕获寄存器3</td><td>0x0000</td></tr><tr><td>R16_TIM4_CH4CVR</td><td>0x40000840</td><td>TIM4 比较/捕获寄存器4</td><td>0x0000</td></tr><tr><td>R16_TIM4_DMACFGR</td><td>0x40000848</td><td>TIM4 DMA 控制寄存器</td><td>0x0000</td></tr><tr><td>R16_TIM4_DMAADR</td><td>0x4000084C</td><td>TIM4 连续模式的 DMA 地址寄存器</td><td>0x0000</td></tr></table>

表 15-6 TIM5 相关寄存器列表  

<table><tr><td>名称</td><td>偏移地址</td><td>描述</td><td>复位值</td></tr><tr><td>R16_TIM5_CTLR1</td><td>0x40000C00</td><td>TIM5 控制寄存器1</td><td>0x0000</td></tr><tr><td>R16_TIM5_CTLR2</td><td>0x40000C04</td><td>TIM5 控制寄存器2</td><td>0x0000</td></tr><tr><td>R16_TIM5_SMCFGR</td><td>0x40000C08</td><td>TIM5 从模式控制寄存器</td><td>0x0000</td></tr><tr><td>R16_TIM5_DMAINTENR</td><td>0x40000C0C</td><td>TIM5 DMA/中断使能寄存器</td><td>0x0000</td></tr><tr><td>R16_TIM5_INTFR</td><td>0x40000C10</td><td>TIM5 中断状态寄存器</td><td>0x0000</td></tr><tr><td>R16_TIM5_SWEVGR</td><td>0x40000C14</td><td>TIM5 事件产生寄存器</td><td>0x0000</td></tr><tr><td>R16_TIM5_CHCTLR1</td><td>0x40000C18</td><td>TIM5 比较/捕获控制寄存器1</td><td>0x0000</td></tr><tr><td>R16_TIM5_CHCTLR2</td><td>0x40000C1C</td><td>TIM5 比较/捕获控制寄存器2</td><td>0x0000</td></tr><tr><td>R16_TIM5_CGER</td><td>0x40000C20</td><td>TIM5 比较/捕获使能寄存器</td><td>0x0000</td></tr><tr><td>R16_TIM5_CNT</td><td>0x40000C24</td><td>TIM5 计数器</td><td>0x0000</td></tr><tr><td>R16_TIM5_PSC</td><td>0x40000C28</td><td>TIM5 计数时钟预分频器</td><td>0x0000</td></tr><tr><td>R16_TIM5_ATRLR</td><td>0x40000C2C</td><td>TIM5 自动重装值寄存器</td><td>0x0000</td></tr><tr><td>R16_TIM5_CH1CVR</td><td>0x40000C34</td><td>TIM5 比较/捕获寄存器1</td><td>0x0000</td></tr><tr><td>R16_TIM5_CH2CVR</td><td>0x40000C38</td><td>TIM5 比较/捕获寄存器2</td><td>0x0000</td></tr><tr><td>R16_TIM5_CH3CVR</td><td>0x40000C3C</td><td>TIM5 比较/捕获寄存器 3</td><td>0x0000</td></tr><tr><td>R16_TIM5_CH4CVR</td><td>0x40000C40</td><td>TIM5 比较/捕获寄存器 4</td><td>0x0000</td></tr><tr><td>R16_TIM5_DMACFGR</td><td>0x40000C48</td><td>TIM5 DMA 控制寄存器</td><td>0x0000</td></tr><tr><td>R16_TIM5_DMAADR</td><td>0x40000C4C</td><td>TIM5 连续模式的 DMA 地址寄存器</td><td>0x0000</td></tr></table>

### 15.4.1 控制寄存器 1（TIMx_CTLR1）（x=2/3/4/5）

偏移地址： $0 \times 0 0$

<table><tr><td>Reserved</td><td>CKD[1:0]</td><td>ARPE</td><td>CMS[1:0]</td><td>DIR</td><td>OPM</td><td>URS</td><td>UDIS</td><td>CEN</td></tr></table>

<table><tr><td>位</td><td>名称</td><td>访问</td><td>描述</td><td>复位值</td></tr><tr><td>[15:10]</td><td>Reserved</td><td>R0</td><td>保留。</td><td>0</td></tr><tr><td>[9:8]</td><td>CKD[1:0]</td><td>RW</td><td>这2位定义在定时器时钟(CK_INT)频率、数字滤波器所用的采样时钟之间的分频比例:00: Tds=Tck_int;01: Tds=2xTck_int;10: Tds=4xTck_int;11: 保留。</td><td>00b</td></tr><tr><td>7</td><td>ARPE</td><td>RW</td><td>自动重装预装使能位:1: 使能自动重装值寄存器(ATRLR);0: 禁止自动重装值寄存器(ATRLR)。</td><td>0</td></tr><tr><td>[6:5]</td><td>CMS[1:0]</td><td>RW</td><td>中央对齐模式选择:00: 边沿对齐模式。计数器依据方向位(DIR)向上或向下计数。01: 中央对齐模式1。计数器交替地向上和向下计数。配置为输出的通道(CHCTLRx 寄存器中CCxS=00)的输出比较中断标志位,只在计数器向下计数时被设置。10: 中央对齐模式2。计数器交替地向上和向下计数。配置为输出的通道(CHCTLRx 寄存器中CCxS=00)的输出比较中断标志位,只在计数器向上计数时被设置。11: 中央对齐模式3。计数器交替地向上和向下计数。配置为输出的通道(CHCTLRx 寄存器中CCxS=00)的输出比较中断标志位,在计数器向上和向下计数时均被设置。注:在计数器使能时(CEN=1),不允许从边沿对齐模式转换到中央对齐模式。</td><td>00b</td></tr><tr><td>4</td><td>DIR</td><td>RW</td><td>计数器方向:0: 计数器的计数模式为增计数;1: 计数器的计数模式为减计数。注:当计数器配置为中央对齐模式或编码器模式时,该位无效。</td><td>0</td></tr><tr><td>3</td><td>OPM</td><td>RW</td><td>单脉冲模式。1: 在发生下一次更新事件(清除CEN位)时,计数器</td><td>0</td></tr><tr><td></td><td></td><td></td><td>停止;0:在发生下一次更新事件时,计数器不停止。</td><td></td></tr><tr><td>2</td><td>URS</td><td>RW</td><td>更新请求源,软件通过该位选择UEV事件的源。1:如果使能了更新中断或DMA请求,则只有计数器溢出/下溢才产生更新中断或DMA请求;0:如果使能了更新中断或DMA请求,则下述任一事件产生更新中断或DMA请求:-计数器溢出/下溢-设置UG位-从模式控制器产生的更新</td><td>0</td></tr><tr><td>1</td><td>UDIS</td><td>RW</td><td>禁止更新,软件通过该位允许/禁止UEV事件的产生。1:禁止UEV。不产生更新事件,各寄存器(ATRLR、PSC、CHCTLRx)保持它们的值。如果设置了UG位或从模式控制器发出了一个硬件复位,则计数器和预分频器被重新初始化。0:允许UEV。更新(UEV)事件由下述任一事件产生:-计数器溢出/下溢-设置UG位-从模式控制器产生的更新具有缓存的寄存器被装入它们的预装载值。</td><td>0</td></tr><tr><td>0</td><td>CEN</td><td>RW</td><td>使能计数器(Counter enable)。1:使能计数器;0:禁止计数器。注:在软件设置了CEN位后,外部时钟、门控模式和编码器模式才能工作。触发模式可以自动地通过硬件设置CEN位。</td><td>0</td></tr></table>

### 15.4.2 控制寄存器 2（TIMx_CTLR2）（x=2/3/4/5）

偏移地址： $0 \times 0 4$

<table><tr><td>Reserved</td><td>TI1S</td><td>MMS[2:0]</td><td>CCDS</td><td>CCUS</td><td>Reserved</td><td>CCPC</td></tr></table>

<table><tr><td>位</td><td>名称</td><td>访问</td><td>描述</td><td>复位值</td></tr><tr><td>[15:8]</td><td>Reserved</td><td>R0</td><td>保留。</td><td>0</td></tr><tr><td>7</td><td>TI1S</td><td>RW</td><td>TI1 选择:1: TIMx_CH1、TIMx_CH2 和 TIMx_CH3 引脚经异或后连到 TI1 输入;0: TIMx_CH1 引脚直连到 TI1 输入。</td><td>0</td></tr><tr><td>[6:4]</td><td>MMS[2:0]</td><td>RW</td><td>主模式选择:这3位用于选择在主模式下送到从定时器的同步信息(TRGO)。可能的组合如下:000:复位-UG位被用于作为触发输出(TRGO)。如果是触发输入产生的复位(从模式控制器处于复位模式),则TRGO上的信号相对实际的复位会有一个延迟;001:使能-计数器使能信号CNT_EN被用于作为触发输出(TRGO)。有时需要在同一时间启动多个定时器或控制在一段时间内使能从定时器。计数器使能信号是通过CEN控制位和门控模式下的触发输入信号的逻辑或产生。当计数器使能信号受控于触发输入时,TRGO上会有一个延迟,除非选择了主/从模式(见TIMx_SMCFGR寄存器中MSM位的描述);010:更新事件被选为触发输入(TRGO)。例如,一个主定时器的时钟可以被用作一个从定时器的预分频器;011:比较脉冲,在发生一次捕获或一次比较成功时,当要设置CC1IF标志时(即使它已经为高),触发输出送出一个正脉冲(TRGO);100:OC1REF信号被用于作为触发输出(TRGO);101:OC2REF信号被用于作为触发输出(TRGO);110:OC3REF信号被用于作为触发输出(TRGO);111:OC4REF信号被用于作为触发输出(TRGO)。</td><td>000b</td></tr><tr><td>3</td><td>CCDS</td><td>RW</td><td>1:当发生更新事件时,送出CHxCVR的DMA请求;0:当发生CHxCVR时,产生CHxCVR的DMA请求。</td><td>0</td></tr><tr><td>2</td><td>CCUS</td><td>RW</td><td>比较捕获控制更新选择位。1:如果CCPC置位,可以通过设置COM位或TRGI上的一个上升沿更新它们;0:如果CCPC置位,只能通过设置COM位更新它们。注:该位只对具有互补输出的通道起作用。</td><td>0</td></tr><tr><td>1</td><td>Reserved</td><td>RO</td><td>保留。</td><td>0</td></tr><tr><td>0</td><td>CCPC</td><td>RW</td><td>比较捕获预装载控制位。1:CCxE,CCxNE和OCxM位是预装载的,设置该位后,它们只在设置了COM位后被更新;0:CCxE,CCxNE和OCxM位不是预装载的。注:该位只对具有互补输出的通道起作用。</td><td>0</td></tr></table>

### 15.4.3 从模式控制寄存器（TIMx_SMCFGR）（x=2/3/4/5）

偏移地址： $0 \times 0 8$

<table><tr><td>15</td><td>14</td><td>13</td><td>12</td><td>11</td><td>10</td><td>9</td><td>8</td><td>7</td><td>6</td><td>5</td><td>4</td><td>3</td><td>2</td><td>1</td><td>0</td></tr><tr><td>ETP</td><td>ECE</td><td colspan="2">ETPS[1:0]</td><td colspan="3">ETF[3:0]</td><td>MSM</td><td colspan="2">TS[2:0]</td><td colspan="2">Reserved</td><td colspan="4">SMS[2:0]</td></tr></table>

<table><tr><td>位</td><td>名称</td><td>访问</td><td>描述</td><td>复位值</td></tr><tr><td>15</td><td>ETP</td><td>R0</td><td>ETR 触发极性选择，该位选择是直接输入 ETR 还是输入 ETR 的反相。</td><td>0</td></tr><tr><td></td><td></td><td></td><td>1:将ETR反相,低电平或下降沿有效;0:ETR,高电平或上升沿有效。</td><td></td></tr><tr><td>14</td><td>ECE</td><td>RW</td><td>外部时钟模式2启用选择。1:使能外部时钟模式2;0:禁用外部时钟模式2。注1:从模式可以与外部时钟模式2同时使用:复位模式,门控模式和触发模式;但是,这时TRGI不能连到ETRF(TS位不能是111b)。注2:外部时钟模式1和外部时钟模式2同时被使能时,外部时钟的输入是ETRF。</td><td>0</td></tr><tr><td>[13:12]</td><td>ETPS[1:0]</td><td>RW</td><td>外部触发信号(ETRP)分频,这个信号频率最大不能超过是TIMxCLK频率的1/4,可以通过这个域来降频。00:关闭预分频;01:ETRP频率除以2;10:ETRP频率除以4;11:ETRP频率除以8。</td><td>00b</td></tr><tr><td>[11:8]</td><td>ETF[3:0]</td><td>RW</td><td>外部触发滤波,实际上,数字滤波器是一个事件计数器,它使用一定的采样的频率,记录到N个事件后会产生一个输出的跳变。0000:无滤波器,以Fdt采样;0001:采样频率Fsampling=Fck_int,N=2;0010:采样频率Fsampling=Fck_int,N=4;0011:采样频率Fsampling=Fck_int,N=8;0100:采样频率FsamplingFdts/2,N=6;0101:采样频率FsamplingFdts/2,N=8;0110:采样频率FsamplingFdts/4,N=6;0111:采样频率FsamplingFdts/4,N=8;1000:采样频率FsamplingFdts/8,N=6;1001:采样频率FsamplingFdts/8,N=8;1010:采样频率FsamplingFdts/16,N=5;1011:采样频率FsamplingFdts/16,N=6;1100:采样频率FsamplingFdts/16,N=8;1101:采样频率FsamplingFdts/32,N=5;1110:采样频率FsamplingFdts/32,N=6;1111:采样频率FsamplingFdts/32,N=8。</td><td>0000b</td></tr><tr><td>7</td><td>MSM</td><td>RW</td><td>主/从模式选择:1:触发输入(TRGI)上的事件被延迟了,以允许在当前定时器(通过TRGO)与它的从定时器间的完美同步。这对要求把几个定时器同步到一个单一的外部事件时是非常有用的;0:不发挥作用。</td><td>0</td></tr><tr><td>[6:4]</td><td>TS[2:0]</td><td>RW</td><td>触发选择域,这3位选择用于同步计数器的触发输入源。000:内部触发0(ITRO);100:TI1的边沿检测器(TI1F_ED);</td><td>000b</td></tr><tr><td></td><td></td><td></td><td>001:内部触发1(ITR1);101:滤波后的定时器输入1(TI1FP1);010:内部触发2(ITR2);110:滤波后的定时器输入2(TI2FP2);011:内部触发3(ITR3);111:外部触发输入(ETRF);以上只有在SMS为0时改变。</td><td></td></tr><tr><td>3</td><td>Reserved</td><td>R0</td><td>保留。</td><td>0</td></tr><tr><td>[2:0]</td><td>SMS[2:0]</td><td>RW</td><td>输入模式选择域。选择核心计数器的时钟和触发模式。000:由内部时钟CK_INT驱动;001:编码器模式1,根据TI1FP1的电平,核心计数器在TI2FP2的边沿增减计数;010:编码器模式2,根据TI2FP2的电平,核心计数器在TI1FP1的边沿增减计数;011:编码器模式3,根据另一个信号的输入电平,核心计数器在TI1FP1和TI2FP2的边沿增减计数;100:复位模式,触发输入(TRGI)的上升沿将初始化计数器,并且产生一个更新寄存器的信号;101:门控模式,当触发输入(TRGI)为高时,计数器的时钟开启;在触发输入变为低,计数器停止,计数器的启停都是受控的;110:触发模式,计数器在触发输入TRGI的上升沿启动,只有计数器的启动是受控的;111:外部时钟模式1,选中的触发输入(TRGI)的上升沿驱动计数器。</td><td>000b</td></tr></table>

### 15.4.4 DMA/中断使能寄存器（TIMx_DMAINTENR）（x=2/3/4/5）

偏移地址： $0 \times 0 0$

<table><tr><td>Reserved</td><td>TDE</td><td>COMDE</td><td>CC4DE</td><td>CC3DE</td><td>CC2DE</td><td>CC1DE</td><td>UDE</td><td>Reserved</td><td>TIE</td><td>保留</td><td>CC4IE</td><td>CC3IE</td><td>CC2IE</td><td>CC1IE</td><td>UIE</td></tr></table>

<table><tr><td>位</td><td>名称</td><td>访问</td><td>描述</td><td>复位值</td></tr><tr><td>15</td><td>Reserved</td><td>R0</td><td>保留。</td><td>0</td></tr><tr><td>14</td><td>TDE</td><td>RW</td><td>触发DMA请求使能位。1:允许触发DMA请求;0:禁止触发DMA请求。</td><td>0</td></tr><tr><td>13</td><td>COMDE</td><td>RW</td><td>COM的DMA请求使能位。1:允许COM的DMA请求;0:禁止COM的DMA请求。</td><td>0</td></tr><tr><td>12</td><td>CC4DE</td><td>RW</td><td>比较捕获通道4的DMA请求使能位。1:允许比较捕获通道4的DMA请求;0:禁止比较捕获通道4的DMA请求。</td><td>0</td></tr><tr><td>11</td><td>CC3DE</td><td>RW</td><td>比较捕获通道3的DMA请求使能位。</td><td>0</td></tr><tr><td></td><td></td><td></td><td>1:允许比较捕获通道3的DMA请求;0:禁止比较捕获通道3的DMA请求。</td><td></td></tr><tr><td>10</td><td>CC2DE</td><td>RW</td><td>比较捕获通道2的DMA请求使能位。1:允许比较捕获通道2的DMA请求;0:禁止比较捕获通道2的DMA请求。</td><td>0</td></tr><tr><td>9</td><td>CC1DE</td><td>RW</td><td>比较捕获通道1的DMA请求使能位。1:允许比较捕获通道1的DMA请求;0:禁止比较捕获通道1的DMA请求。</td><td>0</td></tr><tr><td>8</td><td>UDE</td><td>RW</td><td>更新的DMA请求使能位。1:允许更新的DMA请求;0:禁止更新的DMA请求。</td><td>0</td></tr><tr><td>7</td><td>Reserved</td><td>RO</td><td>保留。</td><td>0</td></tr><tr><td>6</td><td>TIE</td><td>RW</td><td>触发中断使能位。1:使能触发中断;0:禁止触发中断。</td><td>0</td></tr><tr><td>5</td><td>Reserved</td><td>RO</td><td>保留。</td><td>0</td></tr><tr><td>4</td><td>CC4IE</td><td>RW</td><td>比较捕获通道4中断使能位。1:允许比较捕获通道4中断;0:禁止比较捕获通道4中断。</td><td>0</td></tr><tr><td>3</td><td>CC3IE</td><td>RW</td><td>比较捕获通道3中断使能位。1:允许比较捕获通道3中断;0:禁止比较捕获通道3中断。</td><td>0</td></tr><tr><td>2</td><td>CC2IE</td><td>RW</td><td>比较捕获通道2中断使能位。1:允许比较捕获通道2中断;0:禁止比较捕获通道2中断。</td><td>0</td></tr><tr><td>1</td><td>CC1IE</td><td>RW</td><td>比较捕获通道1中断使能位。1:允许比较捕获通道1中断;0:禁止比较捕获通道1中断。</td><td>0</td></tr><tr><td>0</td><td>UIE</td><td>RW</td><td>更新中断使能位。1:允许更新中断;0:禁止更新中断。</td><td>0</td></tr></table>

### 15.4.5 中断状态寄存器（R16_TIMx_INTFR）（x=2/3/4/5）

偏移地址： $0 \times 1 0$

<table><tr><td>Reserved</td><td>CC40F</td><td>CC30F</td><td>CC20F</td><td>CC10F</td><td>Reserved</td><td>TIF</td><td>Reserved</td><td>CC4IF</td><td>CC3IF</td><td>CC2IF</td><td>CC1IF</td><td>UIF</td></tr></table>

<table><tr><td>位</td><td>名称</td><td>访问</td><td>描述</td><td>复位值</td></tr><tr><td>[15:13]</td><td>Reserved</td><td>R0</td><td>保留。</td><td>0</td></tr><tr><td>12</td><td>CC40F</td><td>W0</td><td>比较捕获通道4重复捕获标志位。</td><td>0</td></tr><tr><td>11</td><td>CC30F</td><td>W0</td><td>比较捕获通道3重复捕获标志位。</td><td>0</td></tr><tr><td>10</td><td>CC20F</td><td>W0</td><td>比较捕获通道2重复捕获标志位。</td><td>0</td></tr><tr><td>9</td><td>CC10F</td><td>W0</td><td>比较捕获通道1重复捕获标志位,仅用于比较捕获通道被配置为输入捕获模式时。该标记由硬件置</td><td>0</td></tr><tr><td></td><td></td><td></td><td>位,软件写0可清除此位。1:计数器的值被捕获到捕获比较寄存器时,CC1IF的状态已经被置位;0:无重复捕获产生。</td><td></td></tr><tr><td>[8:7]</td><td>Reserved</td><td>R0</td><td>保留。</td><td>0</td></tr><tr><td>6</td><td>TIF</td><td>WO</td><td>触发器中断标志位,当发生触发事件时由硬件对该位置位,由软件清零。触发事件包括从除门控模式外的其它模式时,在TRGI输入端检测到有效边沿,或门控模式下的任一边沿。1:触发器事件产生;0:无触发器事件产生。</td><td>0</td></tr><tr><td>5</td><td>Reserved</td><td>R0</td><td>保留。</td><td>0</td></tr><tr><td>4</td><td>CC4IF</td><td>WO</td><td>比较捕获通道4中断标志位。</td><td>0</td></tr><tr><td>3</td><td>CC3IF</td><td>WO</td><td>比较捕获通道3中断标志位。</td><td>0</td></tr><tr><td>2</td><td>CC2IF</td><td>WO</td><td>比较捕获通道2中断标志位。</td><td>0</td></tr><tr><td>1</td><td>CC1IF</td><td>WO</td><td>比较捕获通道1中断标志位。如果比较捕获通道配置为输出模式,当计数器值与比较值匹配时该位由硬件置位,但在中心对称模式下除外。该位由软件清零。1:核心计数器的值与比较捕获寄存器1的值匹配;0:无匹配发生。如果比较捕获通道1配置为输入模式,当捕获事件发生时该位由硬件置位,它由软件清零或通过读比较捕获寄存器清零。1:计数器值已被捕获比较捕获寄存器1;0:无输入捕获产生。</td><td>0</td></tr><tr><td>0</td><td>UIF</td><td>WO</td><td>更新中断标志位,当产生更新事件时该位由硬件置位,由软件清零。1:更新中断产生;0:无更新事件产生。以下情形会产生更新事件:若UDIS=0,当重复计数器数值上溢或下溢时;若URS=0、UDIS=0,当置UG位时,或当通过软件对计数器核心计数器重新初始化时;若URS=0、UDIS=0,当计数器CNT被触发事件重新初始化时;</td><td>0</td></tr></table>

### 15.4.6 事件产生寄存器（TIMx_SWEVGR）（x=2/3/4/5）

偏移地址：0x14

<table><tr><td>Reserved</td><td>BG</td><td>TG</td><td>COMG</td><td>CC4G</td><td>CC3G</td><td>CC2G</td><td>CC1G</td><td>UG</td></tr></table>

<table><tr><td>位</td><td>名称</td><td>访问</td><td>描述</td><td>复位值</td></tr><tr><td>[15:8]</td><td>Reserved</td><td>R0</td><td>保留。</td><td>0</td></tr><tr><td>7</td><td>BG</td><td>WO</td><td>刹车事件产生位,此位由软件置位和清零,用来产生一个刹车事件。1:产生一个刹车事件。此时MOE=0、BIF=1,若使能对应的中断和DMA,则产生相应的中断和DMA;0:无动作。</td><td>0</td></tr><tr><td>6</td><td>TG</td><td>WO</td><td>触发事件产生位,该位由软件置位,硬件清零,用于产生一个触发事件。1:产生一个触发事件,TIF被置位,若使能对应的中断和DMA,则产生相应的中断和DMA;0:无动作。</td><td>0</td></tr><tr><td>5</td><td>COMG</td><td>WO</td><td>比较捕获控制更新产生位。产生比较捕获控制更新事件。该位由软件置位,由硬件自动清零。1:当CCPC=1,允许更新CCxE、CCxNE、OCxM位;0:无动作。注:该位只对拥有互补输出的通道(通道1,2,3)有效。</td><td>0</td></tr><tr><td>4</td><td>CC4G</td><td>WO</td><td>比较捕获事件产生位4。产生比较捕获事件4。</td><td>0</td></tr><tr><td>3</td><td>CC3G</td><td>WO</td><td>比较捕获事件产生位3。产生比较捕获事件3。</td><td>0</td></tr><tr><td>2</td><td>CC2G</td><td>WO</td><td>比较捕获事件产生位2。产生比较捕获事件2。</td><td>0</td></tr><tr><td>1</td><td>CC1G</td><td>WO</td><td>比较捕获事件产生位1,产生比较捕获事件1。该位由软件置位,由硬件清零。用于产生一个比较捕获事件。1:在比较捕获通道1上产生一个比较捕获事件:若比较捕获通道1配置为输出:置CC1IF位。若使能对应的中断和DMA,则产生相应的中断和DMA;若比较捕获通道1配置为输入:当前核心计数器的值被捕获至比较捕获寄存器1;置CC1IF位,若使能了对应的中断和DMA,则产生相应的中断和DMA。若CC1IF已经置位,则置CC1OF位。0:无动作。</td><td>0</td></tr><tr><td>0</td><td>UG</td><td>WO</td><td>更新事件产生位,产生更新事件。该位由软件置位,由硬件自动清零。1:初始化计数器,并产生一个更新事件;0:无动作。注:预分频器的计数器也被清零,但是预分频系数不变。若在中心对称模式下或增计数模式下则核心计数器被清零;若减计数模式下则核心计数器取重装值寄存器的值。</td><td>0</td></tr></table>

### 15.4.7 比较/捕获控制寄存器 1（TIMx_CHCTLR1）（x=2/3/4/5）

偏移地址： $0 \times 1 8$

通道可用于输入(捕获模式)或输出(比较模式)，通道的方向由相应的 $\mathtt { C C } \mathtt { x } \mathtt { S }$ 位定义。该寄存器其它位的作用在输入和输出模式下不同。 ${ 0 0 } \times \times$ 描述了通道在输出模式下的功能， $1 0 \times \mathsf { x }$ 描述了通道在输入模式下的功能。

<table><tr><td>15</td><td>14</td><td>13</td><td>12</td><td>11</td><td>10</td><td>9</td><td>8</td><td>7</td><td>6</td><td>5</td><td>4</td><td>3</td><td>2</td><td>1</td><td>0</td></tr><tr><td>OC2CE</td><td colspan="2">OC2M[2:0]</td><td>OC2PE</td><td>OC2FE</td><td rowspan="2" colspan="2">CC2S[1:0]</td><td>OC1CE</td><td colspan="2">OC1M[2:0]</td><td>OC1PE</td><td colspan="2">OC1FE</td><td rowspan="2" colspan="2">CC1S[1:0]</td><td></td></tr><tr><td colspan="3">IC2F[3:0]</td><td colspan="2">IC2PSC[1:0]</td><td colspan="3">IC1F[3:0]</td><td colspan="3">IC1PSC[1:0]</td><td></td></tr></table>

比较模式（引脚方向为输出）：  

<table><tr><td>位</td><td>名称</td><td>访问</td><td>描述</td><td>复位值</td></tr><tr><td>15</td><td>OC2CE</td><td>RW</td><td>比较捕获通道2清零使能位。1:一旦检测到ETRF输入高电平,清除OC2REF位零;0:OC2REF不受ETRF输入的影响。</td><td>0</td></tr><tr><td>[14:12]</td><td>OC2M[2:0]</td><td>RW</td><td>比较捕获通道2模式设置域。该3位定义了输出参考信号OC2REF的动作,而OC2REF决定了OC2、OC2N的值。OC2REF是高电平有效,而OC2和OC2N的有效电平取决于CC2P、CC2NP位。000:冻结。比较捕获寄存器的值与核心计数器间的比较值对OC1REF不起作用;001:强制设为有效电平。当核心计数器与比较捕获寄存器1的值相同时,强制OC1REF为高;010:强制设为无效电平。当核心计数器的值与比较捕获寄存器1相同时,强制OC1REF为低;011:翻转。当核心计数器与比较捕获寄存器1的值相同时,翻转OC1REF的电平。100:强制为无效电平。强制OC1REF为低。101:强制为有效电平。强制OC1REF为高。110:PWM模式1:在向上计数时,一旦核心计数器大于比较捕获寄存器的值时通道1为无效电平,否则为有效电平;在向下计数时,一旦核心计数器大于比较捕获寄存器的值时通道1为有效电平,否则为无效电平。111:PWM模式2:在向上计数时,一旦核心计数器大于比较捕获寄存器的值时,通道1为有效电平,否则为无效电平;在向下计数时,一旦核心计数器大于比较捕获寄存器的值时,通道1为无效电平,否则为有效电平(OC1REF=1)。注:一旦LOCK级别设为3并且CC1S=00b则该位不能被修改。在PWM模式1或PWM模式2中,只有当比较结果改变了或在输出比较模式中从冻结模式切换到PWM模式时,OC1REF电平才改变。</td><td>000b</td></tr><tr><td>11</td><td>OC2PE</td><td>RW</td><td>比较捕获寄存器2预装载使能位。1:开启比较捕获寄存器的预装载功能,读写操作仅对预装载寄存器操作,比较捕获寄存器1的预装载值在更新事件到来时被加载至当前影子寄存器中;0:禁止比较捕获寄存器2的预装载功能,可随时写</td><td>0</td></tr><tr><td></td><td></td><td></td><td>入比较捕获寄存器2,并且新写入的数值立即起作用。注:一旦LOCK级别设为3并且CC1S=00,则该位不能被修改。仅仅在单脉冲模式下(OPM=1)可以在未确认预装载寄存器情况下使用PWM模式,否则其动作不确定。</td><td></td></tr><tr><td>10</td><td>OC2FE</td><td>RW</td><td>比较捕获通道2快速使能位,该位用于加快比较捕获通道输出对触发输入事件的响应。1:输入到触发器的有效沿的作用就像发生了一次比较匹配。因此,OC被设置为比较电平而与比较结果无关。采样触发器的有效沿和比较捕获通道2输出间的延时被缩短为3个时钟周期;0:根据计数器与比较捕获寄存器2的值,比较捕获通道2正常操作,即使触发器是打开的。当触发器的输入有一个有效沿时,激活比较捕获通道2输出的最小延时为5个时钟周期。OC2FE只在通道被配置成PWM1或PWM2模式时起作用;</td><td>0</td></tr><tr><td>[9:8]</td><td>CC2S[1:0]</td><td>RW</td><td>比较捕获通道2输入选择域。00:比较捕获通道2被配置为输出;01:比较捕获通道2被配置为输入,IC2映射在TI2上;10:比较捕获通道2被配置为输入,IC2映射在TI1上;11:比较捕获通道2被配置为输入,IC2映射在TRC上。此模式仅工作在内部触发器输入被选中时(由TS位选择)。注:比较捕获通道2仅在通道关闭时(CC2E为零时)才是可写的。</td><td>00b</td></tr><tr><td>7</td><td>OC1CE</td><td>RW</td><td>比较捕获通道1清零使能位。</td><td>0</td></tr><tr><td>[6:4]</td><td>OC1M[2:0]</td><td>RW</td><td>比较捕获通道1模式设置域。</td><td>0</td></tr><tr><td>3</td><td>OC1PE</td><td>RW</td><td>比较捕获寄存器1预装载使能位。</td><td>0</td></tr><tr><td>2</td><td>OC1FE</td><td>RW</td><td>比较捕获通道1快速使能位。</td><td>0</td></tr><tr><td>[1:0]</td><td>CC1S[1:0]</td><td>RW</td><td>比较捕获通道1输入选择域。</td><td>0</td></tr></table>

捕获模式（引脚方向为输入）：  

<table><tr><td>位</td><td>名称</td><td>访问</td><td>描述</td><td>复位值</td></tr><tr><td>[15:12]</td><td>IC2F[3:0]</td><td>RW</td><td>输入捕获滤波器2配置域，这几位设置了TI1输入的采样频率及数字滤波器长度。数字滤波器由一个事件计数器组成，它记录到N个事件后会产生一个输出的跳变。0000：无滤波器，以fDTS采样；1000：采样频率Fsampling=Fdts/8，N=6；0001：采样频率Fsampling=Fck_int，N=2；1001：采样频率Fsampling=Fdts/8，N=8；</td><td>0000b</td></tr><tr><td></td><td></td><td></td><td>0010:采样频率 F sampling = Fck_int, N=4;1010:采样频率 F sampling = Fdt's/16, N=5;0011:采样频率 F sampling = f = Fck_int, N=8;1011:采样频率 F sampling = Fdt's/16, N=6;0100:采样频率 F sampling = Fdt's/2, N=6;1100:采样频率 F sampling = Fdt's/16, N=8;0101:采样频率 F sampling = Fdt's/2, N=8;1101:采样频率 F sampling = Fdt's/32, N=5;0110:采样频率 F sampling = Fdt's/4, N=6;1110:采样频率 F sampling = Fdt's/32, N=6;0111:采样频率 F sampling = Fdt's/4, N=8;1111:采样频率 F sampling = Fdt's/32, N=8。</td><td></td></tr><tr><td>[11:10]</td><td>IC2PSC[1:0]</td><td>RW</td><td>比较捕获通道2预分频配置域,这2位定义了比较捕获通道2的预分频系数。一旦CC1E=0,则预分频器复位。00:无预分频器,捕获输入口上检测到的每一个边沿都触发一次捕获;01:每2个事件触发一次捕获;10:每4个事件触发一次捕获;11:每8个事件触发一次捕获。</td><td>00b</td></tr><tr><td>[9:8]</td><td>CC2S[1:0]</td><td>RW</td><td>比较捕获通道2输入选择域,这2位定义通道的方向(输入/输出),及输入脚的选择。00:比较捕获通道1通道被配置为输出;01:比较捕获通道1通道被配置为输入,IC1映射在TI1上;10:比较捕获通道1通道被配置为输入,IC1映射在TI2上;11:比较捕获通道1通道被配置为输入,IC1映射在TRC上。此模式仅工作在内部触发器输入被选中时(由TS位选择)。注:CC1S仅在通道关闭时(CC1E为0)才是可写的。</td><td>00b</td></tr><tr><td>[7:4]</td><td>IC1F[3:0]</td><td>RW</td><td>输入捕获滤波器1配置域。</td><td>0</td></tr><tr><td>[3:2]</td><td>IC1PSC[1:0]</td><td>RW</td><td>比较捕获通道1预分频配置域。</td><td>0</td></tr><tr><td>[1:0]</td><td>CC1S[1:0]</td><td>RW</td><td>比较捕获通道1输入选择域。</td><td>0</td></tr></table>

### 15.4.8 比较/捕获控制寄存器 2（TIMx_CHCTLR2）（x=2/3/4/5）

偏移地址： ${ 0 } { \times 1 } { 0 }$

通道可用于输入(捕获模式)或输出(比较模式)，通道的方向由相应的 $\mathtt { C C } \mathtt { x } \mathtt { S }$ 位定义。该寄存器其它位的作用在输入和输出模式下不同。 ${ 0 0 } \times \times$ 描述了通道在输出模式下的功能， $1 0 \times \mathsf { x }$ 描述了通道在输入模式下的功能。

<table><tr><td>15</td><td>14</td><td>13</td><td>12</td><td>11</td><td>10</td><td>9</td><td>8</td><td>7</td><td>6</td><td>5</td><td>4</td><td>3</td><td>2</td><td>1</td><td>0</td></tr><tr><td>OC4CE</td><td colspan="2">OC4M[2:0]</td><td>OC4PE</td><td>OC4FE</td><td rowspan="2" colspan="2">CC4S[1:0]</td><td>OC3CE</td><td colspan="2">OC3M[2:0]</td><td>OC3PE</td><td>OC3FE</td><td rowspan="2" colspan="4">CC3S[1:0]</td></tr><tr><td colspan="3">IC4F[3:0]</td><td colspan="2">IC4PSC[1:0]</td><td colspan="3">IC3F[3:0]</td><td colspan="2">IC3PSC[1:0]</td></tr></table>

比较模式（引脚方向为输出）：

<table><tr><td>位</td><td>名称</td><td>访问</td><td>描述</td><td>复位值</td></tr><tr><td>15</td><td>OC4CE</td><td>RW</td><td>比较捕获通道4清零使能位。</td><td>0</td></tr><tr><td>[14:12]</td><td>OC4M[2:0]</td><td>RW</td><td>比较捕获通道4模式设置域。</td><td>0</td></tr><tr><td>11</td><td>OC4PE</td><td>RW</td><td>比较捕获寄存器4预装载使能位。</td><td>0</td></tr><tr><td>10</td><td>OC4FE</td><td>RW</td><td>比较捕获通道4快速使能位。</td><td>0</td></tr><tr><td>[9:8]</td><td>CC4S[1:0]</td><td>RW</td><td>比较捕获通道4输入选择域。</td><td>0</td></tr><tr><td>7</td><td>OC3CE</td><td>RW</td><td>比较捕获通道3清零使能位。</td><td>0</td></tr><tr><td>[6:4]</td><td>OC3M[2:0]</td><td>RW</td><td>比较捕获通道3模式设置域。</td><td>0</td></tr><tr><td>3</td><td>OC3PE</td><td>RW</td><td>比较捕获寄存器3预装载使能位。</td><td>0</td></tr><tr><td>2</td><td>OC3FE</td><td>RW</td><td>比较捕获通道3快速使能位。</td><td>0</td></tr><tr><td>[1:0]</td><td>CC3S[1:0]</td><td>RW</td><td>比较捕获通道3输入选择域。</td><td>0</td></tr></table>

捕获模式（引脚方向为输入）：

<table><tr><td>位</td><td>名称</td><td>访问</td><td>描述</td><td>复位值</td></tr><tr><td>[15:12]</td><td>IC4F[3:0]</td><td>RW</td><td>输入捕获滤波器4配置域。</td><td>0</td></tr><tr><td>[11:10]</td><td>IC4PSC[1:0]</td><td>RW</td><td>比较捕获通道4预分频配置域。</td><td>0</td></tr><tr><td>[9:8]</td><td>CC4S[1:0]</td><td>RW</td><td>比较捕获通道4输入选择域。</td><td>0</td></tr><tr><td>[7:4]</td><td>IC3F[3:0]</td><td>RW</td><td>输入捕获滤波器3配置域。</td><td>0</td></tr><tr><td>[3:2]</td><td>IC3PSC[1:0]</td><td>RW</td><td>比较捕获通道3预分频配置域。</td><td>0</td></tr><tr><td>[1:0]</td><td>CC3S[1:0]</td><td>RW</td><td>比较捕获通道3输入选择域。</td><td>0</td></tr></table>

### 15.4.9 比较/捕获使能寄存器（TIMx_CCER）（x=2/3/4/5）

偏移地址： $_ { 0 \times 2 0 }$

<table><tr><td>Reserved</td><td>CC4P</td><td>CC4E</td><td>Reserved</td><td>CC3P</td><td>CC3E</td><td>Reserved</td><td>CC2P</td><td>CC2E</td><td>Reserved</td><td>CC1P</td><td>CC1E</td></tr></table>

<table><tr><td>位</td><td>名称</td><td>访问</td><td>描述</td><td>复位值</td></tr><tr><td>[15:14]</td><td>Reserved</td><td>R0</td><td>保留。</td><td>0</td></tr><tr><td>13</td><td>CC4P</td><td>RW</td><td>比较捕获通道4输出极性设置位。</td><td>0</td></tr><tr><td>12</td><td>CC4E</td><td>RW</td><td>比较捕获通道4输出使能位。</td><td>0</td></tr><tr><td>[11:10]</td><td>Reserved</td><td>R0</td><td>保留。</td><td>0</td></tr><tr><td>9</td><td>CC3P</td><td>RW</td><td>比较捕获通道3输出极性设置位。</td><td>0</td></tr><tr><td>8</td><td>CC3E</td><td>RW</td><td>比较捕获通道3输出使能位。</td><td>0</td></tr><tr><td>[7:6]</td><td>Reserved</td><td>R0</td><td>保留。</td><td>0</td></tr><tr><td>5</td><td>CC2P</td><td>RW</td><td>比较捕获通道2输出极性设置位。</td><td>0</td></tr><tr><td>4</td><td>CC2E</td><td>RW</td><td>比较捕获通道2输出使能位。</td><td>0</td></tr><tr><td>[3:2]</td><td>Reserved</td><td>R0</td><td>保留。</td><td>0</td></tr><tr><td>1</td><td>CC1P</td><td>RW</td><td>比较捕获通道1输出极性设置位。CC1通道配置为输出:1:0C1低电平有效;0:0C1高电平有效。CC1通道配置为输入:该位选择是IC1还是IC1的反相信号作为触发或捕</td><td>0</td></tr><tr><td></td><td></td><td></td><td>获信号。
1: 反相: 捕获发生在 IC1 的下降沿; 当用作外部触发器时, IC1 反相。
0: 不反相: 捕获发生在 IC1 的上升沿; 当用作外部触发器时, IC1 不反相。</td><td></td></tr><tr><td>0</td><td>CC1E</td><td>RW</td><td>比较捕获通道 1 输出使能位。
CC1 通道配置为输出:
1: 开启: OC1 信号输出到对应的输出引脚。
0: 关闭: OC1 禁止输出。
CC1 通道配置为输入:
该位决定了计数器的值是否能捕获入 TIMx_CCR1 寄存器。
1: 捕获使能;
0: 捕获禁止。</td><td>0</td></tr></table>

### 15.4.10 通用定时器的计数器（TIMx_CNT）（x=2/3/4/5）

偏移地址： ${ 0 } { \times 2 4 }$

<table><tr><td>15</td><td>14</td><td>13</td><td>12</td><td>11</td><td>10</td><td>9</td><td>8</td><td>7</td><td>6</td><td>5</td><td>4</td><td>3</td><td>2</td><td>1</td><td>0</td></tr><tr><td colspan="16">CNT[15:0]</td></tr></table>

<table><tr><td>位</td><td>名称</td><td>访问</td><td>描述</td><td>复位值</td></tr><tr><td>[15:0]</td><td>CNT[15:0]</td><td>RW</td><td>定时器的计数器的实时值。</td><td>0</td></tr></table>

### 15.4.11 计数时钟预分频器（TIMx_PSC）（x=2/3/4/5）

偏移地址： $_ { 0 \times 2 8 }$

<table><tr><td>15</td><td>14</td><td>13</td><td>12</td><td>11</td><td>10</td><td>9</td><td>8</td><td>7</td><td>6</td><td>5</td><td>4</td><td>3</td><td>2</td><td>1</td><td>0</td></tr><tr><td colspan="16">PSC[15:0]</td></tr></table>

<table><tr><td>位</td><td>名称</td><td>访问</td><td>描述</td><td>复位值</td></tr><tr><td>[15:0]</td><td>PSC[15:0]</td><td>RW</td><td>定时器的预分频器的分频系数；计数器的时钟频率等于分频器的输入频率/(PSC+1)。</td><td>0</td></tr></table>

### 15.4.12 自动重装值寄存器（TIMx_ATRLR）（x=2/3/4/5）

偏移地址： $\mathtt { 0 } \mathtt { x } 2 0$

<table><tr><td>15</td><td>14</td><td>13</td><td>12</td><td>11</td><td>10</td><td>9</td><td>8</td><td>7</td><td>6</td><td>5</td><td>4</td><td>3</td><td>2</td><td>1</td><td>0</td></tr><tr><td colspan="16">ARR[15:0]</td></tr></table>

<table><tr><td>位</td><td>名称</td><td>访问</td><td>描述</td><td>复位值</td></tr><tr><td>[15:0]</td><td>ARR[15:0]</td><td>RW</td><td>ATRLR[15:0]的值将会被装入计数器，ATRLR何时动作和更新请阅读14.2.4节；ATRLR为空时，计数器停止。</td><td>0</td></tr></table>

### 15.4.13 比较/捕获寄存器 1（TIMx_CH1CVR）（x=2/3/4/5）

偏移地址： $0 \times 3 4$

<table><tr><td>15</td><td>14</td><td>13</td><td>12</td><td>11</td><td>10</td><td>9</td><td>8</td><td>7</td><td>6</td><td>5</td><td>4</td><td>3</td><td>2</td><td>1</td><td>0</td></tr><tr><td colspan="16">CCR1[15:0]</td></tr></table>

<table><tr><td>位</td><td>名称</td><td>访问</td><td>描述</td><td>复位值</td></tr><tr><td>[15:0]</td><td>CCR1[15:0]</td><td>RW</td><td>比较捕获寄存器通道1的值。</td><td>0</td></tr></table>

### 15.4.14 比较/捕获寄存器 2（TIMx_CH2CVR）（x=2/3/4/5）

偏移地址： ${ 0 } { \times } \ 3 { 8 }$

<table><tr><td>15</td><td>14</td><td>13</td><td>12</td><td>11</td><td>10</td><td>9</td><td>8</td><td>7</td><td>6</td><td>5</td><td>4</td><td>3</td><td>2</td><td>1</td><td>0</td></tr><tr><td colspan="16">CCR2[15:0]</td></tr></table>

<table><tr><td>位</td><td>名称</td><td>访问</td><td>描述</td><td>复位值</td></tr><tr><td>[15:0]</td><td>CCR2[15:0]</td><td>RW</td><td>比较捕获寄存器通道2的值。</td><td>0</td></tr></table>

### 15.4.15 比较/捕获寄存器 3（TIMx_CH3CVR）（x=2/3/4/5）

偏移地址： $\mathtt { 0 } \mathtt { x } \mathtt { 3 0 }$

<table><tr><td>15</td><td>14</td><td>13</td><td>12</td><td>11</td><td>10</td><td>9</td><td>8</td><td>7</td><td>6</td><td>5</td><td>4</td><td>3</td><td>2</td><td>1</td><td>0</td></tr><tr><td colspan="16">CCR3[15:0]</td></tr></table>

<table><tr><td>位</td><td>名称</td><td>访问</td><td>描述</td><td>复位值</td></tr><tr><td>[15:0]</td><td>CCR3[15:0]</td><td>RW</td><td>比较捕获寄存器通道3的值。</td><td>0</td></tr></table>

### 15.4.16 比较/捕获寄存器 4（TIMx_CH4CVR）（x=2/3/4/5）

偏移地址： $0 \times 4 0$

<table><tr><td>15</td><td>14</td><td>13</td><td>12</td><td>11</td><td>10</td><td>9</td><td>8</td><td>7</td><td>6</td><td>5</td><td>4</td><td>3</td><td>2</td><td>1</td><td>0</td></tr><tr><td colspan="16">CCR4[15:0]</td></tr></table>

<table><tr><td>位</td><td>名称</td><td>访问</td><td>描述</td><td>复位值</td></tr><tr><td>[15:0]</td><td>CCR4[15:0]</td><td>RW</td><td>比较捕获寄存器通道4的值。</td><td>0</td></tr></table>

### 15.4.17 DMA 控制寄存器（TIMx_DMACFGR）（x=2/3/4/5）

偏移地址： $0 \times 4 8$

<table><tr><td>Reserved</td><td>DBL[4:0]</td><td>Reserved</td><td>DBA[4:0]</td></tr></table>

<table><tr><td>位</td><td>名称</td><td>访问</td><td>描述</td><td>复位值</td></tr><tr><td>[15:13]</td><td>Reserved</td><td>R0</td><td>保留。</td><td>0</td></tr><tr><td>[12:8]</td><td>DBL[4:0]</td><td>RW</td><td>DMA连续传送的长度，实际值为此域的值+1。</td><td>0</td></tr><tr><td>[7:5]</td><td>Reserved</td><td>RO</td><td>保留。</td><td>0</td></tr><tr><td>[4:0]</td><td>DBA[4:0]</td><td>RW</td><td>这些位定义了DMA在连续模式下从控制寄存器1所在地址的偏移量。</td><td>0</td></tr></table>

### 15.4.18 连续模式的 DMA 地址寄存器（TIMx_DMAADR）（x=2/3/4/5）

偏移地址： $\mathtt { 0 } \mathtt { x } 4 0$

<table><tr><td colspan="17">DMAB[15:0]</td></tr><tr><td>15</td><td>14</td><td>13</td><td>12</td><td>11</td><td>10</td><td>9</td><td>8</td><td>7</td><td>6</td><td>5</td><td>4</td><td>3</td><td>2</td><td>1</td><td>0</td><td></td></tr></table>

<table><tr><td>位</td><td>名称</td><td>访问</td><td>描述</td><td>复位值</td></tr><tr><td>[15:0]</td><td>DMAB[15:0]</td><td>RW</td><td>连续模式下，DMA的地址。</td><td>0</td></tr></table>

# 第 16 章 基本定时器（BCTM）

本章模块描述适用于CH32F2x、CH32V2x和CH32V3x微控制器全系列产品。

基本定时器模块包含一个16位可自动重装的定时器（TIM6和 TIM7），用于计数和在更新新事件产生中断或DMA请求。

## 16.1 主要特征

通用定时器的主要特征包括：

$\bullet$ 16 位自动重装计数器，支持增计数模式  
$\bullet$ 16 位预分频器，分频系数从 $1 \sim 6 5 5 3 6$ 之间动态可调  
$\bullet$ 触发 DAC 同步电路  
在更新事件时产生中断或 DMA 请求

## 16.2 原理和结构

![](images/d50401d691f969bdfa33805629d5acbe09dd8fa0c7a2e3e4f216b3ec7b6bff5a.jpg)  
图16-1 基本定时器的结构框图

### 16.2.1 概述

如图15-1所示，通用定时器的结构大致可以分为两部分，即输入时钟部分和核心计数器部分。

基本定时器的时钟来自于 AHB 总线时钟（CK_INT）。这些输入的时钟信号经过各种设定的滤波分频等操作后成为 CK_PSC 时钟，输出给核心计数器部分。另外，这些复杂的时钟来源还可以作为 TRGO输出至DAC外设。

基本定时器的核心是一个16位计数器（CNT）。CK_PSC 经过预分频器（PSC）分频后，成为 CK_CNT再最终输给CNT，CNT支持增计数模式，并有一个自动重装值寄存器（ATRLR）在每个计数周期结束后为CNT重装载初始化值。

### 16.2.2 基本定时器和通用定时器的区别

与通用定时器相比，基本定时器缺少以下功能：

1） 基本定时器缺少减计数模式和增减计数模式。  
2） 基本定时器缺少四路独立的比较捕获通道。  
3） 基本定时器不支持外部信号控制定时器。  
4） 基本定时器不支持增量式编码，定时器之间的级联和同步。

### 16.2.3 时钟输入

基本定时器的时钟由内部时钟 CK_INT 提供。

### 16.2.4 计数器和周边

CK_PSC输入给预分频器（PSC）进行分频。PSC是 16位的，实际的分频系数相当于 R16_TIMx_PSC的值 $+ 1$ 。CK_PSC 经过 PSC 会成为 CK_INT。更改 R16_TIM1_PSC 的值并不会实时生效，而会在更新事件后更新给PSC。更新事件包括UG位清零和复位。

### 16.3 调试模式

当系统进入调试模式时，根据 DBG 模块的设置可以控制定时器继续运转或者停止。

## 16.4 寄存器描述

表 16-1 TIM6 相关寄存器列表  

<table><tr><td>名称</td><td>偏移地址</td><td>描述</td><td>复位值</td></tr><tr><td>R16_TIM6_CTLR1</td><td>0x40001000</td><td>TIM6 控制寄存器1</td><td>0x0000</td></tr><tr><td>R16_TIM6_CTLR2</td><td>0x40001004</td><td>TIM6 控制寄存器2</td><td>0x0000</td></tr><tr><td>R16_TIM6_DMAINTENR</td><td>0x4000100C</td><td>TIM6 DMA/中断使能寄存器</td><td>0x0000</td></tr><tr><td>R16_TIM6_INTFR</td><td>0x40001010</td><td>TIM6 中断状态寄存器</td><td>0x0000</td></tr><tr><td>R16_TIM6_SWEVGR</td><td>0x40001014</td><td>TIM6 事件产生寄存器</td><td>0x0000</td></tr><tr><td>R16_TIM6_CNT</td><td>0x40001024</td><td>TIM6 计数器</td><td>0x0000</td></tr><tr><td>R16_TIM6_PSC</td><td>0x40001028</td><td>TIM6 计数时钟预分频器</td><td>0x0000</td></tr><tr><td>R16_TIM6_ATRLR</td><td>0x4000102C</td><td>TIM6 自动重装值寄存器</td><td>0x0000</td></tr></table>

表 16-2 TIM7 相关寄存器列表  

<table><tr><td>名称</td><td>偏移地址</td><td>描述</td><td>复位值</td></tr><tr><td>R16_TIM7_CTLR1</td><td>0x40001400</td><td>TIM7 控制寄存器1</td><td>0x0000</td></tr><tr><td>R16_TIM7_CTLR2</td><td>0x40001404</td><td>TIM7 控制寄存器2</td><td>0x0000</td></tr><tr><td>R16_TIM7_DMAINTENR</td><td>0x4000140C</td><td>TIM7 DMA/中断使能寄存器</td><td>0x0000</td></tr><tr><td>R16_TIM7_INTFR</td><td>0x40001410</td><td>TIM7 中断状态寄存器</td><td>0x0000</td></tr><tr><td>R16_TIM7_CNT</td><td>0x40001424</td><td>TIM7 计数器</td><td>0x0000</td></tr><tr><td>R16_TIM7_PSC</td><td>0x40001428</td><td>TIM7 计数时钟预分频器</td><td>0x0000</td></tr><tr><td>R16_TIM7_ATRLR</td><td>0x4000142C</td><td>TIM7 自动重装值寄存器</td><td>0x0000</td></tr></table>

### 16.4.1 控制寄存器 1（TIMx_CTLR1）（x=6/7）

偏移地址： $0 \times 0 0$

<table><tr><td>15</td><td>14</td><td>13</td><td>12</td><td>11</td><td>10</td><td>9</td><td>8</td><td>7</td><td>6</td><td>5</td><td>4</td><td>3</td><td>2</td><td>1</td><td>0</td></tr><tr><td colspan="8">Reserved</td><td>ARPE</td><td colspan="2">Reserved</td><td>OPM</td><td colspan="2">URS</td><td>UDIS</td><td>CEN</td></tr></table>

<table><tr><td>位</td><td>名称</td><td>访问</td><td>描述</td><td>复位值</td></tr><tr><td>[15:8]</td><td>Reserved</td><td>R0</td><td>保留。</td><td>0</td></tr><tr><td>7</td><td>ARPE</td><td>RW</td><td>自动重装预装使能位:1:使能自动重装值寄存器(ATRLR);0:禁止自动重装值寄存器(ATRLR)。</td><td>0</td></tr><tr><td>[6:4]</td><td>Reserved</td><td>R0</td><td>保留。</td><td>0</td></tr><tr><td>3</td><td>OPM</td><td>RW</td><td>单脉冲模式。1:在发生下一次更新事件(清除CEN位)时,计数器停止;0:在发生下一次更新事件时,计数器不停止。</td><td>0</td></tr><tr><td>2</td><td>URS</td><td>RW</td><td>更新请求源,软件通过该位选择UEV事件的源。1:如果使能了更新中断或DMA请求,则只有计数器溢出/下溢才产生更新中断或DMA请求;0:如果使能了更新中断或DMA请求,则下述任一事件产生更新中断或DMA请求:-计数器溢出/下溢-设置UG位-从模式控制器产生的更新</td><td>0</td></tr><tr><td>1</td><td>UDIS</td><td>RW</td><td>禁止更新,软件通过该位允许/禁止UEV事件的产生。1:禁止UEV。不产生更新事件,各寄存器(ATRLR、PSC、CHCTLRx)保持它们的值。如果设置了UG位或从模式控制器发出了一个硬件复位,则计数器和预分频器被重新初始化。0:允许UEV。更新(UEV)事件由下述任一事件产生:-计数器溢出/下溢-设置UG位-从模式控制器产生的更新具有缓存的寄存器被装入它们的预装载值。</td><td>0</td></tr><tr><td>0</td><td>CEN</td><td>RW</td><td>使能计数器(Counter enable)。1:使能计数器;0:禁止计数器。</td><td>0</td></tr></table>

### 16.4.2 控制寄存器 2（TIMx_CTLR2）（ $\bf { \chi } _ { x = 6 / 7 ) }$

偏移地址： $0 \times 0 4$

<table><tr><td>15</td><td>14</td><td>13</td><td>12</td><td>11</td><td>10</td><td>9</td><td>8</td><td>7</td><td>6</td><td>5</td><td>4</td><td>3</td><td>2</td><td>1</td><td>0</td></tr><tr><td colspan="9">Reserved</td><td colspan="3">MMS[2:0]</td><td colspan="4">Reserved</td></tr></table>

<table><tr><td>位</td><td>名称</td><td>访问</td><td>描述</td><td>复位值</td></tr><tr><td>[15:7]</td><td>Reserved</td><td>R0</td><td>保留。</td><td>0</td></tr><tr><td>[6:4]</td><td>MMS[2:0]</td><td>RW</td><td>主模式选择:这3位用于选择在主模式下送到从定时器的同步信息(TRGO)。可能的组合如下:000:复位-UG位被用于作为触发输出(TRGO)。如果是触发输入产生的复位(从模式控制器处于复位模式),则TRGO上的信号相对实际的复位会有一个延迟;001:使能-计数器使能信号CNT_EN被用于作为触发输出(TRGO)。有时需要在同一时间启动多个定时器或控制在一段时间内使能从定时器。计数器使能信号是通过CEN控制位和门控模式下的触发输入信号的逻辑或产生。当计数器使能信号受控于触发输入时,TRGO上会有一个延迟,除非选择了主/从模式(见TIMx_SMCFGR寄存器中MSM位的描述);010:更新事件被选为触发输入(TRGO)。例如,一个主定时器的时钟可以被用作一个从定时器的预分频器。</td><td>000b</td></tr><tr><td>[3:0]</td><td>Reserved</td><td>R0</td><td>保留。</td><td>0</td></tr></table>

### 16.4.3 DMA/中断使能寄存器（TIMx_DMAINTENR）（x=6/7）

偏移地址： $0 \times 0 0$

<table><tr><td>Reserved</td><td>UDE</td><td>Reserved</td><td>UIE</td></tr></table>

<table><tr><td>位</td><td>名称</td><td>访问</td><td>描述</td><td>复位值</td></tr><tr><td>[15:9]</td><td>Reserved</td><td>RO</td><td>保留。</td><td>0</td></tr><tr><td>8</td><td>UDE</td><td>RW</td><td>更新的 DMA 请求使能位。
1: 允许更新的 DMA 请求;
0: 禁止更新的 DMA 请求。</td><td>0</td></tr><tr><td>[7:1]</td><td>Reserved</td><td>RO</td><td>保留。</td><td>0</td></tr><tr><td>0</td><td>UIE</td><td>RW</td><td>更新中断使能位。
1: 允许更新中断;
0: 禁止更新中断。</td><td>0</td></tr></table>

### 16.4.4 中断状态寄存器（R16_TIMx_INTFR）（ $\bf { x = 6 / 7 ) }$

偏移地址： $0 \times 1 0$

<table><tr><td>15</td><td>14</td><td>13</td><td>12</td><td>11</td><td>10</td><td>9</td><td>8</td><td>7</td><td>6</td><td>5</td><td>4</td><td>3</td><td>2</td><td>1</td><td>0</td></tr><tr><td colspan="15">Reserved</td><td>UIF</td></tr></table>

<table><tr><td>位</td><td>名称</td><td>访问</td><td>描述</td><td>复位值</td></tr><tr><td>[15:1]</td><td>Reserved</td><td>R0</td><td>保留。</td><td>0</td></tr><tr><td>0</td><td>UIF</td><td>RWO</td><td>更新中断标志位，当产生更新事件时该位由硬件置</td><td>0</td></tr><tr><td></td><td></td><td></td><td>位,由软件清零。
1: 更新中断产生;
0: 无更新事件产生。
以下情形会产生更新事件:
若 UDIS=0, 当重复计数器数值上溢或下溢时;
若 URS=0、UDIS=0, 当置 UG 位时, 或当通过软件对计数器核心计数器重新初始化时;</td><td></td></tr></table>

### 16.4.5 事件产生寄存器（TIMx_SWEVGR）（ $\bf { x = 6 / 7 ) }$ ）

偏移地址：0x14

<table><tr><td>15</td><td>14</td><td>13</td><td>12</td><td>11</td><td>10</td><td>9</td><td>8</td><td>7</td><td>6</td><td>5</td><td>4</td><td>3</td><td>2</td><td>1</td><td>0</td></tr><tr><td colspan="15">Reserved</td><td>UG</td></tr></table>

<table><tr><td>位</td><td>名称</td><td>访问</td><td>描述</td><td>复位值</td></tr><tr><td>[15:1]</td><td>Reserved</td><td>R0</td><td>保留。</td><td>0</td></tr><tr><td>0</td><td>UG</td><td>WO</td><td>更新事件产生位,产生更新事件。该位由软件置位,由硬件自动清零。1:初始化计数器,并产生一个更新事件;0:无动作。注:预分频器的计数器也被清零,但是预分频系数不变。若在中心对称模式下或增计数模式下则核心计数器被清零;若减计数模式下则核心计数器取重装值寄存器的值。</td><td>0</td></tr></table>

### 16.4.6 通用定时器的计数器（TIMx_CNT）（x=6/7）

偏移地址： ${ 0 } { \times 2 4 }$

<table><tr><td>15</td><td>14</td><td>13</td><td>12</td><td>11</td><td>10</td><td>9</td><td>8</td><td>7</td><td>6</td><td>5</td><td>4</td><td>3</td><td>2</td><td>1</td><td>0</td></tr><tr><td colspan="16">CNT[15:0]</td></tr></table>

<table><tr><td>位</td><td>名称</td><td>访问</td><td>描述</td><td>复位值</td></tr><tr><td>[15:0]</td><td>CNT[15:0]</td><td>RW</td><td>定时器的计数器的实时值。</td><td>0</td></tr></table>

### 16.4.7 计数时钟预分频器（TIMx_PSC）（ $\bf { x } = 6 / 7 )$

偏移地址： $_ { 0 \times 2 8 }$

<table><tr><td>15</td><td>14</td><td>13</td><td>12</td><td>11</td><td>10</td><td>9</td><td>8</td><td>7</td><td>6</td><td>5</td><td>4</td><td>3</td><td>2</td><td>1</td><td>0</td></tr><tr><td colspan="16">PSC[15:0]</td></tr></table>

<table><tr><td>位</td><td>名称</td><td>访问</td><td>描述</td><td>复位值</td></tr><tr><td>[15:0]</td><td>PSC[15:0]</td><td>RW</td><td>定时器的预分频器的分频系数；计数器的时钟频率等于分频器的输入频率/(PSC+1)。</td><td>0</td></tr></table>

### 16.4.8 自动重装值寄存器（TIMx_ATRLR）（x=6/7）

偏移地址： $\mathtt { 0 } \mathtt { x } 2 0$

<table><tr><td>15</td><td>14</td><td>13</td><td>12</td><td>11</td><td>10</td><td>9</td><td>8</td><td>7</td><td>6</td><td>5</td><td>4</td><td>3</td><td>2</td><td>1</td><td>0</td></tr><tr><td colspan="16">ARR[15:0]</td></tr></table>

<table><tr><td>位</td><td>名称</td><td>访问</td><td>描述</td><td>复位值</td></tr><tr><td>[15:0]</td><td>ARR[15:0]</td><td>RW</td><td>ATRLR[15:0]的值将会被装入计数器，ATRLR何时动作和更新请阅读14.2.4节；ATRLR为空时，计数器停止。</td><td>0</td></tr></table>

# 第 17 章 数字/模拟转换（DAC）

本章模块描述适用于CH32F2x、CH32V2x和 $_ { C H 3 2 V 3 x }$ 微控制器部分产品。

数字/模拟转换模块（DAC），包含2个可配置8/12 位数字输入转换 2路模拟电压输出的转换器。内置三角波、噪声波形发生器，支持多种事件触发转换，DMA 功能等。

## 17.1 主要特性

2 个 DAC 转换器，每个转换器对应 1 个输出通道  
$\bullet$ 三角波、噪声波形发生器  
$\bullet$ 可配置 8 位或 12 位输出  
12位数据左对齐或右对齐  
$\bullet$ 双 DAC 同时或分别转换  
$\bullet$ 支持 DMA功能  
多种触发事件

## 17.2 功能描述

### 17.2.1 DAC 模块结构

![](images/6e507ff55b60c035f614d898cd7d549e40c748843456bf3525b73db661d38147.jpg)  
图 17-1 DAC 模块框图

### 17.2.2 DAC 通道配置

#### 17.2.2.1 开启 DAC 功能：

将DAC_CTLR寄存器的ENx位置1，即可打开对DAC通道 x的模拟供电。经过一段启动时间，DAC 通道 $\mathsf { x }$ 即被使能。DAC 包含 2 个模拟输出通道，可同时或分别独立输出。

注：为了避免寄生的干扰和额外的功耗，DAC通道对应的引脚需提前设置成模拟输入(AIN)模式。

#### 17.2.2.2 打开输出缓冲：

DAC 集成了输出缓冲，可以用来减少输出阻抗，增加驱动能力直接驱动外部负载。每个 DAC通道输出缓存可以通过设置DAC_CTLR寄存器的 BOFFx位来使能或关闭。

#### 17.2.2.3 数据格式：

单DAC通道模式下，包括8位数据右对齐、12位数据左对齐、12位数据右对齐。写入数据到DAC_R8BDHRx[7:0]，模块将加载（1个APB1时钟周期后）其左移数据到数据输出寄存器DAC_DORx[11:0]，写入数据到 DAC_R12BDHRx[11:0]，模块将加载（1 个 APB1 时钟周期后）右对齐数

据到数据输出寄存器 DAC_DORx[11:0]；写入数据到 DAC_L12BDHRx[15:4]，模块经过相应的移位后，将加载（1个APB1时钟周期后）左对齐数据到数据输出寄存器 DAC_DORx[11:0]。双 DAC通道模式下，同样包括8位数据右对齐、12数据左对齐、12位数据右对齐三种方式。写入数据到DAC_RD8BDHR[7:0]，模块将加载（1 个 APB1 时钟周期后）位[7:0]移位后到 DAC_DOR1[11:0]，位[15:8]移位后到 DAC_DOR2[11:0]；写入数据到 DAC_LD12BDHR[31:0]，模块将加载（1 个 APB 时钟周期后）位[15:4]数据移位后到 DAC_DOR1[11:4]，位[31:20]数据移位后到 DAC_DOR2[11:4]；写入数据到 DAC_RD12BDHR[31:0]，模块将加载（1 个 APB 时钟周期后）位[11:0]数据到 DAC_DOR1[11:0]，位[27:16]数据到 DAC_DOR2[11:0]。

![](images/aa307d0c4dac9361bcf0580712d804f9c95ef61f64559062dee6f7296eabca77.jpg)  
图17-2 单通道数据格式

![](images/1c7e5f6504512555f7a040e66be34bb7431aeec73eb450756b83378ac2d3e8f3.jpg)  
图17-3 双通道数据格式

#### 17.2.2.4 DMA 功能：

DAC 通道具有DMA功能。设置DAC_CTLR寄存器的DMAENx位为1，开启对应通道的 DMA功能。当有触发事件(不包括软件触发)发生，则产生一个 DMA请求，然后 DAC_DORx寄存器的数据将被更新。

#### 17.2.2.5 触发事件选择：

DAC 转换可以由以下事件触发进行转换。当配置 DAC_CTLR寄存器的 TENx 位为 1，配置TSELx[2:0]控制位选择某个触发事件触发 DAC转换。

表 17-1 触发事件  

<table><tr><td>触发源</td><td>类型</td><td>TSELx[2:0]</td></tr><tr><td>定时器6 TRGO事件</td><td rowspan="6">来自片上定时器内部信号</td><td>000</td></tr><tr><td>定时器8 TRGO事件</td><td>001</td></tr><tr><td>定时器7 TRGO事件</td><td>010</td></tr><tr><td>定时器5 TRGO事件</td><td>011</td></tr><tr><td>定时器2 TRGO事件</td><td>100</td></tr><tr><td>定时器4 TRGO事件</td><td>101</td></tr><tr><td>EXTI 线路9</td><td>外部引脚</td><td>110</td></tr><tr><td>SWTRIG(软件触发)</td><td>软件控制位</td><td>111</td></tr></table>

DAC 接口会监测到来自选中的定时器 TRGO 输出或外部中断线 9 的上升沿，在触发后的 3 个 APB1时钟周期后，将寄存器DAC_DORx更新为新值。

如果配置的是软件触发方式，SWTRIG位一旦置 1，将会启动一次转换，在触发后的 1个 APB1 时钟周期后，将寄存器DAC_DORx更新为新值，并且硬件对 SWTRIG 位自动清 0。

注：不能在ENx为1时改变 $T S E L x [ 2 ; 0 ]$ 位。

### 17.2.3 DAC 转换

DAC 通道的数据来自DAC_DORx寄存器，但不能直接对寄存器 DAC_DORx写入数据，任何输出到DAC 通道 $\mathsf { x }$ 的数据都必须写入 DAC_R12BDHR1、DAC_L12BDHR1、DAC_R12BDHR2、DAC_L12BDHR2、DAC_RD12BDHR、DAC_LD12BDHR、DAC_RD8BDHR 寄存器中。由系统内部的保持寄存器 DAC_DHRx 会获取上述寄存器值将其经过相应时间送入DAC_DORx寄存器。

非触发方式下，写入寄存器 DAC_xDHRx 的数据会在 1 个 APB1 时钟周期后移入 DAC_DORx 寄存器。

软件触发下，事件触发上升沿后 1 个 APB1 时钟周期后自动更新 DAC_DORx 寄存器。

硬件触发（定时器 TRGO 事件或者外部中断线 9 上升沿）下，触发事件后 3 个 APB1 时钟周期后自动更新 DAC_DORx 寄存器。

装入 DAC_DORx 寄存器数据，在经过时间 tSETTLING之后，输出即有效，这段时间的长短依电源电压和模拟输出负载的不同会有所变化。

数字输入经过 DAC 被线性地转换为模拟电压输出，其范围为 0 到 $V _ { \mathsf { D D A } }$ 。任一 DAC 通道引脚上的输出电压满足下面的关系：

DAC 输出电压 $= \mathsf { V } _ { \mathsf { D D A } } \ast$ (DAC_DORx/ 4096)。

### 17.2.4 DAC 三角波生成器

模块内置了一个三角波生成器，可以在基准信号上加上一个小幅度的三角波。设置 WAVEx[1:0]位为‘10’选择DAC的三角波生成功能。设置 DAC_CTLR寄存器的 MAMPx[3:0]位来选择三角波的幅度。

系统内部包含一个从0开始的三角波计数器，在每次触发事件后 3个 APB1时钟周期后累加 1。计数器的值与DAC_DHRx寄存器的数值相加并丢弃溢出位后写入 DAC_DORx 寄存器。在传入 DAC_DORx寄存器的数值小于 $\mathsf { M A M P } \times \left[ 3 : 0 \right]$ 位定义的最大幅度时，三角波计数器逐步累加，一旦达到设置的最大幅度，则计数器开始递减，达到0后再开始累加，周而复始。将 WAVEx[1:0]位置‘00’，可以复位三角波的生成。

注：1.为了产生三角波，必须使能 DAC 触发，即设 DAC_CTLR寄存器的 TENx位为1。

2.MAMPx[3:0]位必须在使能DAC之前设置，否则其值不能修改。

![](images/f2311923ead2271032471b3a6b0602e5b005fde06fa6eb07a55d4662f716eea6.jpg)  
图 17-4 三角波生成

### 17.2.5 DAC 噪声生成器

模块内置了一个噪声生成器，是利用线性反馈移位寄存器(Linear Feedback Shift RegisterLFSR)产生幅度变化的伪噪声。设置WAVE[1:0]位为‘01’选择 DAC噪声生成功能。设置 DAC_CTLR寄存器的MAMPx[3:0]位来选择屏蔽部分LFSR的数据。

寄存器LFSR的预装入值为0xAAA。按照特定算法，在每次触发事件后 3个APB1时钟周期之后更新该寄存器的值。设置DAC_CR寄存器的 MAMPx[3:0]位可以屏蔽部分或者全部 LFSR 的数据，这样的得到的 LSFR 值与 DAC_DHRx 的数值相加，去掉溢出位之后即被写入 DAC_DORx 寄存器。如果寄存器LFSR 值为 $0 \times 0 0 0$ ，则会注入‘1’(防锁定机制)。将 WAVEx[1:0]位置‘00’，可以复位 LFSR 波形的生成算法。

注：为了产生噪声，必须使能 DAC 触发，即设DAC_CTLR寄存器的 TENx位为1。

![](images/19cf372b06640b7c498a7197be77ed41d85ab1f729f3534a5e303ed2b7ed78b1.jpg)  
图 17-5 LFSR 寄存器算法

## 17.3 双 DAC 转换

当需要2个DAC同时转换的情况下，为了更加便捷和高效的操作 DAC模块，模块集成 3个双DAC 模式下的数据寄存器 DAC_RD8BDHR、DAC_LD12BDHR、DAC_RD12BDHR。只需操作其中之一寄存器即可更新 2 个 DAC 的转换值。

针对双DAC的转换可配合模块的其他寄存器，可实现 11种不同组合的转换模式，两个通道待转换值需写入上述 3 个双通道数据寄存器之一。

### 17.3.1 不同触发下使用相同 LFSR

设置 TENx 置位，TSELx 为不同值，WAVEx 为 0b01，MAMPx 为相同的 LFSR 屏蔽值。当通道 1 触发事件发生时，将通道1数据寄存器DAC_DHR1值加上带相同屏蔽的 LFSR1计数值，延迟 3个 APB1时钟后送给DAC_DOR1，用于转换，并更新LFSR1；当通道 2触发事件发生时，将通道 2 数据寄存器DAC_DHR2值加上带相同屏蔽的LFSR2计数值，延迟3 个 APB1时钟后送给 DAC_DOR2，用于转换，并更新 LFSR2。

### 17.3.2 不同触发下使用不同 LFSR

设置 TENx 置位，TSELx 为不同值，WAVEx 为 0b01，MAMPx 为不同的 LFSR 屏蔽值。当通道 1 触发事件发生时，将通道1数据寄存器DAC_DHR1值加上按 MAMP1[3:0］所设屏蔽的 LFSR1 计数值，延迟3个APB1时钟后送给DAC_DOR1，用于转换，并更新LFSR1；当通道2 触发事件发生时，将通道 2 数据寄存器 DAC_DHR2 值加上按 MAMP2[3:0］所设屏蔽的 LFSR2 计数值，延迟 3 个 APB1 时钟后送给DAC_DOR2，用于转换，并更新 LFSR2。

### 17.3.3 不同触发下产生相同三角波

设置 TENx 置位，TSELx 为不同值，WAVEx 为 0b1x， ${ \mathsf { M A M P x } }$ 设为相同三角波幅值。当通道 1触发事件发生时，将通道 1 数据寄存器 DAC_DHR1 值加上 MAMPx 所设的相同幅值的三角波计数器值，延迟3个APB1时钟后送给DAC_DOR1，用于转换，并更新通道 1三角波计数器；当通道 2触发事件发生时，将通道2数据寄存器DAC_DHR2值加上 MAMPx所设的相同幅值的三角波计数器值，延迟 3个APB1 时钟后送给 DAC_DOR2，用于转换，并更新通道 2 三角波计数器。

### 17.3.4 不同触发下产生不同三角波

设置 TENx 置位，TSELx 为不同值，WAVEx 为 0b1x，MAMPx 设为不同的三角波幅值。当通道 1 触发事件发生时，将通道 1 数据寄存器 DAC_DHR1 值加上 MAMP1 所设幅值的三角波计数器值，延迟 3 个APB1时钟后送给DAC_DOR1，用于转换，并更新通道1 三角波计数器；当通道2 触发事件发生时，将通道2数据寄存器DAC_DHR2值加上MAMP2所设幅值的三角波计数器值，延迟3 个 APB1时钟后送给DAC_DOR2，用于转换，并更新通道 2 三角波计数器。

### 17.3.5 不同触发下不使用波形发生器

设置TENx置位，TSELx为不同值选择不同触发源。当通道 1 触发事件发生时，将通道 1数据寄

存器DAC_DHR1值延迟3个APB1时钟后送给DAC_DOR1，用于转换；当通道2触发事件发生时，将通道 2 数据寄存器 DAC_DHR2 值延迟 3 个 APB1 时钟后送给 DAC_DOR2，用于转换。

### 17.3.6 均使用软件触发

在此配置下，双通道数据寄存器写入需要转换值，1 个 APB1时钟周期后，DAC_DHR1和DAC_DHR2 的数据被分别送到 DAC_DOR1 和 DAC_DOR2 用于转换。

### 17.3.7 相同触发下使用相同 LFSR

设置 TENx 置位，TSELx 为相同值，WAVEx 为 0b01，MAMPx 为相同的 LFSR 屏蔽值。当触发事件发生后，寄存器 DAC_DHR1 的值加上带相同屏蔽的 LFSR1 计数值，延迟 3 个 APB1 时钟后送给DAC_DOR1，用于转换，并更新LFSR1，同时寄存器DAC_DHR2的值加上带相同屏蔽的 LFSR2 计数值，延迟 3 个 APB1 时钟后送给 DAC_DOR2，用于转换，并更新 LFSR2。

### 17.3.8 相同触发下使用不同 LFSR

设置 TENx 置位，TSELx 为相同值，WAVEx 为 0b01，MAMPx 为不同的 LFSR 屏蔽值。当触发事件发生后，寄存器DAC_DHR1的值加上带不同屏蔽值的 LFSR1计数值，延迟 3个 APB1时钟后送给DAC_DOR1，用于转换，并更新 LFSR1，同时寄存器 DAC_DHR2 的值加上带不同屏蔽值的 LFSR2 计数值，延迟 3 个 APB1 时钟后送给 DAC_DOR2，用于转换，并更新 LFSR2。

### 17.3.9 相同触发下产生相同三角波

设置 TENx 置位，TSELx 为相同值，WAVEx 为 0b1x，MAMPx 为相同的三角波幅值。当触发事件发生后，寄存器DAC_DHR1的值加上相同三角波幅值的计数器值，延迟 3个 APB1时钟后送给DAC_DOR1，用于转换，并更新通道1三角波计数器，同时寄存器 DAC_DHR2的值加上相同三角波幅值的计数器值，延迟 3 个 APB1 时钟后送给 DAC_DOR2，用于转换，并更新通道 2 三角波计数器值。

### 17.3.10 相同触发下产生不同三角波

设置TENx置位，TSELx为相同值，WAVEx为 0b1x，MAMPx为不同的三角波幅值。当触发事件发生后，寄存器DAC_DHR1的值加上MAMP1[3:0]所设的三角波幅值计数器值，延迟 3个 APB1 时钟后送给DAC_DOR1，用于转换，并更新通道1三角波计数器，同时寄存器 DAC_DHR2的值加上 MAMP2[3:0]所设的三角波幅值计数器值，延迟 3 个 APB1 时钟后送给 DAC_DOR2，用于转换，并更新通道 2 三角波计数器值。

### 17.3.11 相同触发下不使用波形发生器

设置 TENx 置位，TSELx 为相同值。该配置下，当触发事件发生后，寄存器 DAC_DHR1 和DAC_DHR2 的值在延迟 3 个 APB1 时钟后分别送给 DAC_DOR1 和 DAC_DOR2 用于 DAC 转换。

## 17.4 寄存器描述

表 17-2 DAC 相关寄存器列表  

<table><tr><td>名称</td><td>访问地址</td><td>描述</td><td>复位值</td></tr><tr><td>R32_DAC_CTLR</td><td>0x40007400</td><td>DAC配置寄存器</td><td>0x00000000</td></tr><tr><td>R32_DAC_SWTR</td><td>0x40007404</td><td>DAC软件触发寄存器</td><td>0x00000000</td></tr><tr><td>R32_DAC_R12BDHR1</td><td>0x40007408</td><td>DAC通道1右对齐12位数据保存寄存器</td><td>0x00000000</td></tr><tr><td>R32_DAC_L12BDHR1</td><td>0x4000740C</td><td>DAC通道1左对齐12位数据保存寄存器</td><td>0x00000000</td></tr><tr><td>R32_DAC_R8BDHR1</td><td>0x40007410</td><td>DAC通道1右对齐8位数据保存寄存器</td><td>0x00000000</td></tr><tr><td>R32_DAC_R12BDHR2</td><td>0x40007414</td><td>DAC通道2右对齐12位数据保存寄存器</td><td>0x00000000</td></tr><tr><td>R32_DAC_L12BDHR2</td><td>0x40007418</td><td>DAC通道2左对齐12位数据保存寄存器</td><td>0x00000000</td></tr><tr><td>R32_DAC_R8BDHR2</td><td>0x4000741C</td><td>DAC通道2右对齐8位数据保存寄存器</td><td>0x00000000</td></tr><tr><td>R32_DAC_RD12BDHR</td><td>0x40007420</td><td>双通道右对齐12位数据保存寄存器</td><td>0x00000000</td></tr><tr><td>R32_DAC_LD12BDHR</td><td>0x40007424</td><td>双通道左对齐12位数据保存寄存器</td><td>0x00000000</td></tr><tr><td>R32_DAC_RD8BDHR</td><td>0x40007428</td><td>双通道右对齐8位数据保存寄存器</td><td>0x00000000</td></tr><tr><td>R32_DAC_DOR1</td><td>0x4000742C</td><td>DAC通道1数据输出寄存器</td><td>0x00000000</td></tr><tr><td>R32_DAC_DOR2</td><td>0x40007430</td><td>DAC通道2数据输出寄存器</td><td>0x00000000</td></tr></table>

### 17.4.1 DAC 配置寄存器（DAC_CTLR）

偏移地址： $0 \times 0 0$

<table><tr><td>31</td><td>30</td><td>29</td><td>28</td><td>27</td><td>26</td><td>25</td><td>24</td><td>23</td><td>22</td><td>21</td><td>20</td><td>19</td><td>18</td><td>17</td><td>16</td></tr><tr><td>Reserved</td><td colspan="2">DMAEN2</td><td colspan="5">MAMP2[3:0]</td><td colspan="2">WAVE2[2:0]</td><td colspan="3">TSEL2[2:0]</td><td>TEN2</td><td>BOFF2</td><td>EN2</td></tr><tr><td>15</td><td>14</td><td>13</td><td>12</td><td>11</td><td>10</td><td>9</td><td>8</td><td>7</td><td>6</td><td>5</td><td>4</td><td>3</td><td>2</td><td>1</td><td>0</td></tr><tr><td>Reserved</td><td colspan="2">DMAEN1</td><td colspan="5">MAMP1[3:0]</td><td colspan="2">WAVE1[2:0]</td><td colspan="3">TSEL1[2:0]</td><td>TEN1</td><td>BOFF1</td><td>EN1</td></tr></table>

<table><tr><td>位</td><td>名称</td><td>访问</td><td>描述</td><td>复位值</td></tr><tr><td>[31:29]</td><td>Reserved</td><td>R0</td><td>保留。</td><td>0</td></tr><tr><td>28</td><td>DMAEN2</td><td>RW</td><td>DAC通道2的DMA使能:1:使能DAC通道2DMA功能;0:关闭DAC通道2DMA功能。</td><td>0</td></tr><tr><td>[27:24]</td><td>MAMP2[3:0]</td><td>RW</td><td>DAC通道2屏蔽/幅值设置,软件设置这个区域用来在噪声生成模式下选择LFSR数据屏蔽位,在三角波形生成模式下选择波形的幅值:0000:不屏蔽LFSR位0/三角波幅值为1;0001:不屏蔽LFSR位[1:0]/三角波幅值为3;0010:不屏蔽LFSR位[2:0]/三角波幅值为7;0011:不屏蔽LFSR位[3:0]/三角波幅值为15;0100:不屏蔽LFSR位[4:0]/三角波幅值为31;0101:不屏蔽LFSR位[5:0]/三角波幅值为63;0110:不屏蔽LFSR位[6:0]/三角波幅值为127;0111:不屏蔽LFSR位[7:0]/三角波幅值为255;1000:不屏蔽LFSR位[8:0]/三角波幅值为511;1001:不屏蔽LFSR位[9:0]/三角波幅值为1023;1010:不屏蔽LFSR位[10:0]/三角波幅值为2047;≥1011:不屏蔽LFSR位[11:0]/三角波幅值为4095。</td><td>0000b</td></tr><tr><td>[23:22]</td><td>WAVE2[1:0]</td><td>RW</td><td>DAC通道2的噪声/三角波生成使能00:关闭波形发生器;01:使能噪声波形发生器;1x:使能三角波波形发生器。</td><td>00b</td></tr><tr><td>[21:19]</td><td>TSEL2[2:0]</td><td>RW</td><td>DAC通道2触发事件选择设置:</td><td>000b</td></tr><tr><td></td><td></td><td></td><td>000: TIM6 TRGO事件;001: TIM8 TRGO事件;010: TIM7 TRGO事件011: TIM5 TRGO事件100: TIM2 TRGO事件;101: TIM4 TRGO事件;110: 外部中断线9;111: 软件触发;其他:保留。</td><td></td></tr><tr><td>18</td><td>TEN2</td><td>RW</td><td>DAC通道2外部触发模式使能:1:使能DAC通道2触发功能,写入DAC_xDHR寄存器的数据在3个APB1时钟周期后送入DAC_DOR2寄存器。0:关闭DAC通道2触发功能,写入DAC_xDHR寄存器的数据在1个APB1时钟周期后送入DAC_DOR2寄存器。注:如果选择软件触发,DAC_xDHR中的数据只需1个APB1时钟周期后送入DAC_DOR2寄存器。</td><td>0</td></tr><tr><td>17</td><td>BOFF2</td><td>RW</td><td>DAC通道2输出缓冲关闭控制(建议打开):1:关闭DAC通道2输出缓存;0:打开DAC通道2输出缓存。</td><td>0</td></tr><tr><td>16</td><td>EN2</td><td>RW</td><td>DAC通道2使能:1:使能DAC通道2;0:关闭DAC通道2。</td><td>0</td></tr><tr><td>[15:13]</td><td>Reserved</td><td>RO</td><td>保留。</td><td>0</td></tr><tr><td>12</td><td>DMAEN1</td><td>RW</td><td>DAC通道1的DMA使能:1:使能DAC通道1DMA功能;0:关闭DAC通道1DMA功能。</td><td>0</td></tr><tr><td>[11:8]</td><td>MAMP1[3:0]</td><td>RW</td><td>DAC通道1屏蔽/幅值设置,软件设置这个区域用来在噪声生成模式下选择LFSR数据屏蔽位,在三角波形生成模式下选择波形的幅值:0000:不屏蔽LFSR位0/三角波幅值为1;0001:不屏蔽LFSR位[1:0]/三角波幅值为3;0010:不屏蔽LFSR位[2:0]/三角波幅值为7;0011:不屏蔽LFSR位[3:0]/三角波幅值为15;0100:不屏蔽LFSR位[4:0]/三角波幅值为31;0101:不屏蔽LFSR位[5:0]/三角波幅值为63;0110:不屏蔽LFSR位[6:0]/三角波幅值为127;0111:不屏蔽LFSR位[7:0]/三角波幅值为255;1000:不屏蔽LFSR位[8:0]/三角波幅值为511;1001:不屏蔽LFSR位[9:0]/三角波幅值为1023;1010:不屏蔽LFSR位[10:0]/三角波幅值为2047;≥1011:不屏蔽LFSR位[11:0]/三角波幅值为4095。</td><td>0000b</td></tr><tr><td>[7:6]</td><td>WAVE1[1:0]</td><td>RW</td><td>DAC通道1的噪声/三角波生成使能。00:关闭波形发生器;01:使能噪声波形发生器;1x:使能三角波波形发生器。</td><td>00b</td></tr><tr><td>[5:3]</td><td>TSEL1[2:0]</td><td>RW</td><td>DAC通道1触发事件选择设置:</td><td>000b</td></tr><tr><td>2</td><td>TEN1</td><td>RW</td><td>DAC通道1外部触发模式使能:1: 使能 DAC通道1触发功能,写入 DAC_xDHR 寄存器的数据在3个APB1时钟周期后送入 DAC_DOR1 寄存器。0: 关闭 DAC通道1触发功能,写入 DAC_xDHR 寄存器的数据在1个APB1时钟周期后送入 DAC_DOR1 寄存器。注:如果选择软件触发,DAC_xDHR中的数据只需1个APB1时钟周期后送入 DAC_DOR1 寄存器。</td><td>0</td></tr><tr><td>1</td><td>BOFF1</td><td>RW</td><td>DAC通道1输出缓冲关闭控制(建议打开):1: 关闭 DAC通道1输出缓存;0: 打开 DAC通道1输出缓存。</td><td>0</td></tr><tr><td>0</td><td>EN1</td><td>RW</td><td>DAC通道1使能:1: 使能 DAC通道1;0: 关闭 DAC通道1。</td><td>0</td></tr></table>

注：配置寄存器包括通道1和通道2的配置，当同时使能（EN×位为‘1’）2个通道的输出，将以通道1的配置输出相同波形到2个硬件通道上；当只使能1号通道的输出，将以1号通道配置输出波形到1号硬件通道上，2号通道不输出；当只使能2号通道的输出，将以2号通道配置输出波形到2号硬件通道上，1号通道不输出。

### 17.4.2 DAC 软件触发寄存器（DAC_SWTR）

偏移地址： $0 \times 0 4$

<table><tr><td>31</td><td>30</td><td>29</td><td>28</td><td>27</td><td>26</td><td>25</td><td>24</td><td>23</td><td>22</td><td>21</td><td>20</td><td>19</td><td>18</td><td>17</td><td>16</td></tr><tr><td colspan="16">Reserved</td></tr><tr><td>15</td><td>14</td><td>13</td><td>12</td><td>11</td><td>10</td><td>9</td><td>8</td><td>7</td><td>6</td><td>5</td><td>4</td><td>3</td><td>2</td><td>1</td><td>0</td></tr><tr><td colspan="14">Reserved</td><td>SWTRIG2</td><td>SWTRIG1</td></tr></table>

<table><tr><td>位</td><td>名称</td><td>访问</td><td>描述</td><td>复位值</td></tr><tr><td>[31:2]</td><td>Reserved</td><td>R0</td><td>保留。</td><td>0</td></tr><tr><td>1</td><td>SWTRIG2</td><td>W0</td><td>DAC通道2软件触发控制位:1:使能DAC通道2软件触发;0:关闭DAC通道2软件触发。注:一旦DACxDHR中的数据(1个APB1时钟周期后)送入DAC_DOR2寄存器,该位将硬件清0。</td><td>0</td></tr><tr><td>0</td><td>SWTRIG1</td><td>W0</td><td>DAC通道1软件触发控制位:1:使能DAC通道1软件触发;</td><td>0</td></tr><tr><td></td><td></td><td></td><td>0: 关闭 DAC 通道 1 软件触发。注: 一旦 DAC_xDHR 中的数据 (1 个 APB1 时钟周期后) 送入 DAC_DOR1 寄存器, 该位将硬件清 0。</td><td></td></tr></table>

### 17.4.3 DAC 通道 1 右对齐 12 位数据保存寄存器（DAC_R12BDHR1）

偏移地址： $0 \times 0 8$

<table><tr><td>31</td><td>30</td><td>29</td><td>28</td><td>27</td><td>26</td><td>25</td><td>24</td><td>23</td><td>22</td><td>21</td><td>20</td><td>19</td><td>18</td><td>17</td><td>16</td></tr><tr><td colspan="16">Reserved</td></tr><tr><td>15</td><td>14</td><td>13</td><td>12</td><td>11</td><td>10</td><td>9</td><td>8</td><td>7</td><td>6</td><td>5</td><td>4</td><td>3</td><td>2</td><td>1</td><td>0</td></tr><tr><td colspan="4">Reserved</td><td colspan="12">DACC1DHR[11:0]</td></tr></table>

<table><tr><td>位</td><td>名称</td><td>访问</td><td>描述</td><td>复位值</td></tr><tr><td>[31:12]</td><td>Reserved</td><td>R0</td><td>保留。</td><td>0</td></tr><tr><td>[11:0]</td><td>DACC1DHR[11:0]</td><td>RW</td><td>DAC通道1的12位右对齐数据。</td><td>0</td></tr></table>

### 17.4.4 DAC 通道 1 左对齐 12 位数据保存寄存器（DAC_L12BDHR1）

偏移地址： $0 \times 0 0$

<table><tr><td>31</td><td>30</td><td>29</td><td>28</td><td>27</td><td>26</td><td>25</td><td>24</td><td>23</td><td>22</td><td>21</td><td>20</td><td>19</td><td>18</td><td>17</td><td>16</td></tr><tr><td colspan="16">Reserved</td></tr><tr><td>15</td><td>14</td><td>13</td><td>12</td><td>11</td><td>10</td><td>9</td><td>8</td><td>7</td><td>6</td><td>5</td><td>4</td><td>3</td><td>2</td><td>1</td><td>0</td></tr><tr><td colspan="12">DACC1DHR[11:0]</td><td colspan="4">Reserved</td></tr></table>

<table><tr><td>位</td><td>名称</td><td>访问</td><td>描述</td><td>复位值</td></tr><tr><td>[31:16]</td><td>Reserved</td><td>R0</td><td>保留。</td><td>0</td></tr><tr><td>[15:4]</td><td>DACC1DHR[11:0]</td><td>RW</td><td>DAC通道1的12位左对齐数据。</td><td>0</td></tr><tr><td>[3:0]</td><td>Reserved</td><td>R0</td><td>保留。</td><td>0</td></tr></table>

### 17.4.5 DAC 通道 1 右对齐 8 位数据保存寄存器（DAC_R8BDHR1）

偏移地址： $0 \times 1 0$

<table><tr><td>31</td><td>30</td><td>29</td><td>28</td><td>27</td><td>26</td><td>25</td><td>24</td><td>23</td><td>22</td><td>21</td><td>20</td><td>19</td><td>18</td><td>17</td><td>16</td></tr><tr><td colspan="16">Reserved</td></tr><tr><td>15</td><td>14</td><td>13</td><td>12</td><td>11</td><td>10</td><td>9</td><td>8</td><td>7</td><td>6</td><td>5</td><td>4</td><td>3</td><td>2</td><td>1</td><td>0</td></tr><tr><td colspan="8">Reserved</td><td colspan="8">DACC1DHR[7:0]</td></tr></table>

<table><tr><td>位</td><td>名称</td><td>访问</td><td>描述</td><td>复位值</td></tr><tr><td>[31:8]</td><td>Reserved</td><td>R0</td><td>保留。</td><td>0</td></tr><tr><td>[7:0]</td><td>DACC1DHR[7:0]</td><td>RW</td><td>DAC通道1的8位右对齐数据。</td><td>0</td></tr></table>

### 17.4.6 DAC 通道 2 右对齐 12 位数据保存寄存器（DAC_R12BDHR2）

偏移地址： $0 \times 1 4$

<table><tr><td>31</td><td>30</td><td>29</td><td>28</td><td>27</td><td>26</td><td>25</td><td>24</td><td>23</td><td>22</td><td>21</td><td>20</td><td>19</td><td>18</td><td>17</td><td>16</td></tr><tr><td colspan="16">Reserved</td></tr><tr><td>15</td><td>14</td><td>13</td><td>12</td><td>11</td><td>10</td><td>9</td><td>8</td><td>7</td><td>6</td><td>5</td><td>4</td><td>3</td><td>2</td><td>1</td><td>0</td></tr><tr><td colspan="4">Reserved</td><td colspan="12">DACC2DHR[11:0]</td></tr></table>

<table><tr><td>位</td><td>名称</td><td>访问</td><td>描述</td><td>复位值</td></tr><tr><td>[31:12]</td><td>Reserved</td><td>R0</td><td>保留。</td><td>0</td></tr><tr><td>[11:0]</td><td>DACC2DHR[11:0]</td><td>RW</td><td>DAC通道2的12位右对齐数据。</td><td>0</td></tr></table>

### 17.4.7 DAC 通道 2 左对齐 12 位数据保存寄存器（DAC_L12BDHR2）

偏移地址： $0 \times 1 8$

<table><tr><td>31</td><td>30</td><td>29</td><td>28</td><td>27</td><td>26</td><td>25</td><td>24</td><td>23</td><td>22</td><td>21</td><td>20</td><td>19</td><td>18</td><td>17</td><td>16</td></tr><tr><td colspan="16">Reserved</td></tr><tr><td>15</td><td>14</td><td>13</td><td>12</td><td>11</td><td>10</td><td>9</td><td>8</td><td>7</td><td>6</td><td>5</td><td>4</td><td>3</td><td>2</td><td>1</td><td>0</td></tr><tr><td colspan="12">DACC2DHR[11:0]</td><td colspan="4">Reserved</td></tr></table>

<table><tr><td>位</td><td>名称</td><td>访问</td><td>描述</td><td>复位值</td></tr><tr><td>[31:16]</td><td>Reserved</td><td>R0</td><td>保留。</td><td>0</td></tr><tr><td>[15:4]</td><td>DACC2DHR[11:0]</td><td>RW</td><td>DAC通道2的12位左对齐数据。</td><td>0</td></tr><tr><td>[3:0]</td><td>Reserved</td><td>R0</td><td>保留。</td><td>0</td></tr></table>

### 17.4.8 DAC 通道 2 右对齐 8 位数据保存寄存器（DAC_R8BDHR2）

偏移地址： ${ 0 } { \times 1 } { 0 }$

<table><tr><td>31</td><td>30</td><td>29</td><td>28</td><td>27</td><td>26</td><td>25</td><td>24</td><td>23</td><td>22</td><td>21</td><td>20</td><td>19</td><td>18</td><td>17</td><td>16</td></tr><tr><td colspan="16">Reserved</td></tr><tr><td>15</td><td>14</td><td>13</td><td>12</td><td>11</td><td>10</td><td>9</td><td>8</td><td>7</td><td>6</td><td>5</td><td>4</td><td>3</td><td>2</td><td>1</td><td>0</td></tr><tr><td colspan="8">Reserved</td><td colspan="8">DACC2DHR[7:0]</td></tr></table>

<table><tr><td>位</td><td>名称</td><td>访问</td><td>描述</td><td>复位值</td></tr><tr><td>[31:8]</td><td>Reserved</td><td>R0</td><td>保留。</td><td>0</td></tr><tr><td>[7:0]</td><td>DACC2DHR[7:0]</td><td>RW</td><td>DAC通道2的8位右对齐数据。</td><td>0</td></tr></table>

### 17.4.9 DAC 双通道右对齐 12 位数据保存寄存器（DAC_RD12BDHR）

偏移地址： $_ { 0 \times 2 0 }$

<table><tr><td>31</td><td>30</td><td>29</td><td>28</td><td>27</td><td>26</td><td>25</td><td>24</td><td>23</td><td>22</td><td>21</td><td>20</td><td>19</td><td>18</td><td>17</td><td>16</td></tr><tr><td colspan="4">Reserved</td><td colspan="12">DACC2DHR[11:0]</td></tr><tr><td>15</td><td>14</td><td>13</td><td>12</td><td>11</td><td>10</td><td>9</td><td>8</td><td>7</td><td>6</td><td>5</td><td>4</td><td>3</td><td>2</td><td>1</td><td>0</td></tr><tr><td colspan="4">Reserved</td><td colspan="12">DACC1DHR[11:0]</td></tr></table>

<table><tr><td>位</td><td>名称</td><td>访问</td><td>描述</td><td>复位值</td></tr><tr><td>[31:28]</td><td>Reserved</td><td>R0</td><td>保留。</td><td>0</td></tr><tr><td>[27:16]</td><td>DACC2DHR[11:0]</td><td>RW</td><td>DAC通道2的12位右对齐数据。</td><td>0</td></tr><tr><td>[15:12]</td><td>Reserved</td><td>R0</td><td>保留。</td><td>0</td></tr><tr><td>[11:0]</td><td>DACC1DHR[11:0]</td><td>RW</td><td>DAC通道1的12位右对齐数据。</td><td>0</td></tr></table>

### 17.4.10 DAC 双通道左对齐 12 位数据保存寄存器（DAC_LD12BDHR）

偏移地址： ${ 0 } { \times 2 4 }$

<table><tr><td>31</td><td>30</td><td>29</td><td>28</td><td>27</td><td>26</td><td>25</td><td>24</td><td>23</td><td>22</td><td>21</td><td>20</td><td>19</td><td>18</td><td>17</td><td>16</td></tr><tr><td colspan="12">DACC2DHR[11:0]</td><td colspan="4">Reserved</td></tr><tr><td>15</td><td>14</td><td>13</td><td>12</td><td>11</td><td>10</td><td>9</td><td>8</td><td>7</td><td>6</td><td>5</td><td>4</td><td>3</td><td>2</td><td>1</td><td>0</td></tr><tr><td colspan="12">DACC1DHR[11:0]</td><td colspan="4">Reserved</td></tr></table>

<table><tr><td>位</td><td>名称</td><td>访问</td><td>描述</td><td>复位值</td></tr><tr><td>[31:20]</td><td>DACC2DHR[11:0]</td><td>RW</td><td>DAC通道2的12位左对齐数据。</td><td>0</td></tr><tr><td>[19:16]</td><td>Reserved</td><td>RO</td><td>保留。</td><td>0</td></tr><tr><td>[15:4]</td><td>DACC1DHR[11:0]</td><td>RW</td><td>DAC通道1的12位左对齐数据。</td><td>0</td></tr><tr><td>[3:0]</td><td>Reserved</td><td>RO</td><td>保留。</td><td>0</td></tr></table>

### 17.4.11 DAC 双通道右对齐 8 位数据保存寄存器（DAC_RD8BDHR）

偏移地址： $_ { 0 \times 2 8 }$

<table><tr><td>31</td><td>30</td><td>29</td><td>28</td><td>27</td><td>26</td><td>25</td><td>24</td><td>23</td><td>22</td><td>21</td><td>20</td><td>19</td><td>18</td><td>17</td><td>16</td></tr><tr><td colspan="16">Reserved</td></tr><tr><td>15</td><td>14</td><td>13</td><td>12</td><td>11</td><td>10</td><td>9</td><td>8</td><td>7</td><td>6</td><td>5</td><td>4</td><td>3</td><td>2</td><td>1</td><td>0</td></tr><tr><td colspan="8">DACC2DHR[7:0]</td><td colspan="8">DACC1DHR[7:0]</td></tr></table>

<table><tr><td>位</td><td>名称</td><td>访问</td><td>描述</td><td>复位值</td></tr><tr><td>[31:16]</td><td>Reserved</td><td>R0</td><td>保留。</td><td>0</td></tr><tr><td>[15:8]</td><td>DACC2DHR[7:0]</td><td>RW</td><td>DAC通道2的8位右对齐数据。</td><td>0</td></tr><tr><td>[7:0]</td><td>DACC1DHR[7:0]</td><td>RW</td><td>DAC通道1的8位右对齐数据。</td><td>0</td></tr></table>

### 17.4.12 DAC 通道 1 数据输出寄存器（DAC_DOR1）

偏移地址： $\mathtt { 0 } \mathtt { x } 2 0$

<table><tr><td>31</td><td>30</td><td>29</td><td>28</td><td>27</td><td>26</td><td>25</td><td>24</td><td>23</td><td>22</td><td>21</td><td>20</td><td>19</td><td>18</td><td>17</td><td>16</td></tr><tr><td colspan="16">Reserved</td></tr><tr><td>15</td><td>14</td><td>13</td><td>12</td><td>11</td><td>10</td><td>9</td><td>8</td><td>7</td><td>6</td><td>5</td><td>4</td><td>3</td><td>2</td><td>1</td><td>0</td></tr><tr><td colspan="4">Reserved</td><td colspan="12">DACC1DOR[11:0]</td></tr></table>

<table><tr><td>位</td><td>名称</td><td>访问</td><td>描述</td><td>复位值</td></tr><tr><td>[31:12]</td><td>Reserved</td><td>R0</td><td>保留。</td><td>0</td></tr><tr><td>[11:0]</td><td>DACC1DOR[11:0]</td><td>R0</td><td>DAC通道1输出数据。</td><td>0</td></tr></table>

### 17.4.13 DAC 通道 2 数据输出寄存器（DAC_DOR2）

偏移地址： $\mathtt { 0 } { \times } 3 0$

<table><tr><td>31</td><td>30</td><td>29</td><td>28</td><td>27</td><td>26</td><td>25</td><td>24</td><td>23</td><td>22</td><td>21</td><td>20</td><td>19</td><td>18</td><td>17</td><td>16</td></tr><tr><td colspan="16">Reserved</td></tr><tr><td>15</td><td>14</td><td>13</td><td>12</td><td>11</td><td>10</td><td>9</td><td>8</td><td>7</td><td>6</td><td>5</td><td>4</td><td>3</td><td>2</td><td>1</td><td>0</td></tr><tr><td colspan="4">Reserved</td><td colspan="12">DACC2DOR[11:0]</td></tr></table>

<table><tr><td>位</td><td>名称</td><td>访问</td><td>描述</td><td>复位值</td></tr><tr><td>[31:12]</td><td>Reserved</td><td>R0</td><td>保留。</td><td>0</td></tr><tr><td>[11:0]</td><td>DACC2DOR[11:0]</td><td>R0</td><td>DAC通道2输出数据。</td><td>0</td></tr></table>

# 第 18 章 通用同步异步收发器（USART）

本章模块描述适用于CH32F2x、 $_ { C H 3 2 V 2 x }$ 和 $_ { C H 3 2 V 3 x }$ 微控制器全系列产品。

该模块包含 3个通用同步异步收发器（USART1/2/3）和 5 个通用异步收发器（UART4/5/6/7/8）。注：对于CH32V20x_D6、CH32F20x_D6，串口4为同步异步收发器（USART4）。

## 18.1 主要特征

全双工或半双工的同步或异步通信  
NRZ数据格式  
$\bullet$ 分数波特率发生器，最高 9Mbps  
$\bullet$ 可编程数据长度  
可配置的停止位  
$\bullet$ 支持 LIN，IrDA 编码器，智能卡  
$\bullet$ 支持 DMA  
多种中断源

## 18.2 概述

![](images/964fd43dd65913dbc79ad33d4326591b3dfc99e25cdb10be3b559260a0c891bc.jpg)  
图18-1 通用同步/异步收发器的结构框图

当 TE（发送使能位）置位时，发送移位寄存器里的数据在 TX 引脚上输出，时钟在 CK 引脚上输

出。在发送时，最先移出的是最低有效位，每个数据帧都由一个低电平的起始位开始，然后发送器根据M（字长）位上的设置发送八位或九位的数据字，最后是数目可配置的停止位。如果配有奇偶检验位，数据字的最后一位为校验位。在 TE 置位后会发送一个空闲帧，空闲帧是 10 位或 11 位高电平，包含停止位。断开帧是10位或11位低电平，后跟着停止位。

## 18.3 波特率发生器

收发器的波特率 $=$ FCLK/(16*USARTDIV)，FCLK 是 APBx 的时钟，即 PCLK1 或 PCLK2， USART1 模块使用 PCLK2，其余的使用 PCLK1。USARTDIV 的值是根据 USART_BRR 中的 DIV_M 和 DIV_F 两个域决定的，具体计算的公式为：

$$
U S A R T D I V = D I V _ {M} + (D I V _ {F} / 1 6)
$$

需要注意的是，波特率产生器产生的比特率不一定能刚好生成用户所需要的波特率，这其中可能是存在偏差。除了尽量取接近的值，减小偏差的方法还可以是增大 APBx 的时钟。比如设定波特率为115200bps 的时，USARTDIV 的值设为 39.0625，在最高频率时可以得到刚好 115200bps 的波特率，但是如果你需要 921600bps 的波特率时，计算的 USARTDIV 是 4.88，但是实际上在 USART_BRR 里填入的值最接近只能是 4.875，实际产生的波特率是 923076bps，误差达到 $0 . 1 6 \%$ 。

发送方发出的串口波形传到接收端时，接收方和发送方的波特率是有一定误差的。误差主要来自三个方面：接收方和发送方实际的波特率不一致；接收方和发送方的时钟有误差；波形在线路中产生的变化。外设模块的接收器是有一定接收容差能力的，当以上三个方面产生的总偏差之和小于模块的容差能力极限时，这个总偏差不影响收发。模块的容差能力极限受是否采用分数波特率和 M位（数据域字长）影响，采用分数波特率和使用9位数据域长度会使容差能力极限降低，但不低于 $3 \%$ 。

## 18.4 同步模式

同步模式使得系统在使用USART模块时可以输出时钟信号。在开启同步模式对外发送数据时，CK引脚会同时对外输出时钟。

开启同步模式的方式是对控制寄存器 2（R16_USARTx_CTLR2）的CLKEN 位置位，但同时需要关闭LIN 模式、智能卡模式、红外模式和半双工模式，即保证 SCEN、HDSEL 和 IREN 位处于复位状态，这三位在控制寄存器 3（R16_USARTx_CTLR3）中。

同步模式使用的要点在于时钟的输出控制。有以下几点需要注意：

USART 模块同步模式只工作在主模式，即 CK 引脚只输出时钟，不接收输入；

只在TX引脚输出数据时输出时钟信号；

LBCL 位决定在发送最后一位数据位时是否输出时钟，CPOL 位决定时钟的极性，CPHA 决定时钟的相位，这三个位在控制寄存器 2（R16_USARTx_CTLR2）中，这三个位需要在 TE 和 RE 未被使能的情况下设置，具体区别见图 18-2。

接收器在同步模式下只会在输出时钟时采样，需要从设备保持一定的信号建立时间和保持时间，具体见图18-3。

![](images/2ae2e60c0e82cd2bddcc03c0330e109a740e60ac35673f13a7e215087e686041.jpg)  
图 18-2 USART 时钟时序示例(M=0)

![](images/6215776a2450eac31a7c83e2ac1274449ee22543fd44e279679a68dbe55b42c9.jpg)  
图18-3 数据采样保持时间

## 18.5 单线半双工模式

半双工模式支持使用单个引脚（只使用TX引脚）来接收和发送，TX引脚和 RX引脚在芯片内部连接。

开启半双工模式的方式是对控制寄存器 3（R16_USARTx_CTLR3）的 HDSEL位置位，但同时需要关闭 LIN 模式、智能卡模式、红外模式和同步模式，即保证 SCEN、CLKEN 和 IREN 位处于复位状态，这三位在控制寄存器 2 和 3（R16_USARTx_CTLR2 和 R16_USARTx_CTLR3）中。

设置成半双工模式之后，需要把 TX 的 IO 口设置成浮空输入或开漏输出高模式。在 TE 置位的情况下，只要将数据写到数据寄存器上，就会发送出去。特别要注意的是，半双工模式可能会出现多设备使用单总线收发时的总线冲突，这需要用户用软件自行避免。

## 18.6 智能卡

智能卡模式支持 ISO7816-3 协议访问智能卡控制器。

开启智能卡模式的方式是对控制寄存器 3（R16_USARTx_CTLR3）的 SCEN位置位，但同时需要关闭LIN 模式、半双工模式和红外模式，即保证 LINEN、HDSEL 和 IREN 位处于复位状态，但是可以开启

CLKEN 来输出时钟，这些位在控制寄存器 2 和 3（R16_USARTx_CTLR2 和 R16_USARTx_CTLR3）中。

为了支持智能卡模式，USART应当被置为8 位数据位外加 1位校验位，它的停止位建议配置成发送和接收都为 1.5 位，智能卡模式是一种单线半双工的协议，它使用 TX 线作为数据通讯，应当被配置为开漏输出加上拉。当接收方接收一帧数据检测到奇偶校验错误时，会在停止位时，发出一个 NACK信号，即在停止位期间主动把 TX 拉低一个周期，发送方检测到 NACK 信号后，会产生帧错误，应用程序据此可以重发。图17-4展示了正确情况下和发生奇偶校验错误情况下的 TX引脚上的波形图。USART的 TC 标志（发送完成标志）可以延迟 GT（保护时间）个时钟产生，接收方也不会将自己置的 NACK 信号认成起始位。

![](images/fc35e90bf2dc8ce2ce8510b5d3fe7aabd4406236d161d7c26347129293ec78f0.jpg)  
图 18-4 (未)发生奇偶校验错误示意图

在智能卡模式下，CK引脚使能后输出的波形和通讯无关，它仅仅是给智能卡提供时钟的，它的值是APB时钟再经过五位可设置的时钟分频（分频值为 PSC的两倍，最高 62分频）。

## 18.7 IrDA

USART模块支持控制IrDA红外收发器进行物理层通信。使用 IrDA必须清除 LINEN、STOP、CLKEN、SCEN和HDSEL位。USART模块和SIR物理层（红外收发器）之间使用NRZ（不归零）编码，最高支持到 115200bps 速率。

IrDA是一个半双工的协议，如果UASRT 正在给SIR物理层发数据，那么IrDA解码器将会忽视新发来的红外信号，如果 USART 正在接受从 SIR 发来的数据，那么 SIR 不会接受来自 USART 的信号。USART 发给 SIR 和 SIR 发给 USART 的电平逻辑是不一样的，SIR 接收逻辑中，高电平为 1，低电平为0，但是在 SIR 发送逻辑中，高电平为 0，低电平为 1。

## 18.8 DMA

USART 模块支持 DMA 功能，可以利用 DMA 实现快速连续收发。当启用 DMA 时，TXE 被置位时，DMA就会从设定的内存空间向发送缓冲区写数据。当使用 DMA接收时，每次 RXNE置位后，DMA就会将接收缓冲区里的数据转移到特定的内存空间。

## 18.9 中断

USART模块支持多种中断源，包括发送数据寄存器空（TXE）、CTS、发送完成（TC）、接收数据就绪（TXNE）、数据溢出（ORE）、线路空闲（IDLE）、奇偶校验出错（PE）、断开标志（LBD）、噪声（NE）、多缓冲通信的溢出（ORE）和帧错误（FE）等等。

表 18-1 中断和对应的使能位的关系  

<table><tr><td>中断源</td><td>使能位</td></tr><tr><td>数据寄存器空（TXE）</td><td>TXEIE</td></tr><tr><td>允许发送（CTS）</td><td>CTSIE</td></tr><tr><td>发送完成（TC）</td><td>TCIE</td></tr><tr><td>接收数据就绪（TXNE）</td><td rowspan="2">TXNEIE</td></tr><tr><td>数据溢出（ORE）</td></tr><tr><td>线路空闲（IDLE）</td><td>IDLEIE</td></tr><tr><td>奇偶校验出错（PE）</td><td>PEIE</td></tr><tr><td>断开标志（LBD）</td><td>LBDIE</td></tr><tr><td>噪声（NE）</td><td rowspan="3">EIE</td></tr><tr><td>多缓冲通信的溢出（ORE）</td></tr><tr><td>多缓冲通信的帧错误（FE）</td></tr></table>

## 18.10 寄存器描述

表 18-2 USART1 相关寄存器列表  

<table><tr><td>名称</td><td>访问地址</td><td>描述</td><td>复位值</td></tr><tr><td>R32_USART1_STAT</td><td>0x40013800</td><td>UASRT1 状态寄存器</td><td>0x000000C0</td></tr><tr><td>R32_USART1_DATAR</td><td>0x40013804</td><td>UASRT1 数据寄存器</td><td>0x000000XX</td></tr><tr><td>R32_USART1_BRR</td><td>0x40013808</td><td>UASRT1 波特率寄存器</td><td>0x00000000</td></tr><tr><td>R32_USART1_CTLR1</td><td>0x4001380C</td><td>UASRT1 控制寄存器 1</td><td>0x00000000</td></tr><tr><td>R32_USART1_CTLR2</td><td>0x40013810</td><td>UASRT1 控制寄存器 2</td><td>0x00000000</td></tr><tr><td>R32_USART1_CTLR3</td><td>0x40013814</td><td>UASRT1 控制寄存器 3</td><td>0x00000000</td></tr><tr><td>R32_USART1_GPR</td><td>0x40013818</td><td>UASRT1 保护时间和预分频寄存器</td><td>0x00000000</td></tr></table>

表 18-3 USART2 相关寄存器列表  

<table><tr><td>名称</td><td>访问地址</td><td>描述</td><td>复位值</td></tr><tr><td>R32_USART2_STAT</td><td>0x40004400</td><td>UASRT2 状态寄存器</td><td>0x000000C0</td></tr><tr><td>R32_USART2_DATAR</td><td>0x40004404</td><td>UASRT2 数据寄存器</td><td>0x000000XX</td></tr><tr><td>R32_USART2_BRR</td><td>0x40004408</td><td>UASRT2 波特率寄存器</td><td>0x00000000</td></tr><tr><td>R32_USART2_CTLR1</td><td>0x4000440C</td><td>UASRT2 控制寄存器 1</td><td>0x00000000</td></tr><tr><td>R32_USART2_CTLR2</td><td>0x40004410</td><td>UASRT2 控制寄存器 2</td><td>0x00000000</td></tr><tr><td>R32_USART2_CTLR3</td><td>0x40004414</td><td>UASRT2 控制寄存器 3</td><td>0x00000000</td></tr><tr><td>R32_USART2_GPR</td><td>0x40004418</td><td>UASRT2 保护时间和预分频寄存器</td><td>0x00000000</td></tr></table>

表 18-4 USART3 相关寄存器列表  

<table><tr><td>名称</td><td>访问地址</td><td>描述</td><td>复位值</td></tr><tr><td>R32_USART3_STAT</td><td>0x40004800</td><td>UASRT3 状态寄存器</td><td>0x000000C0</td></tr><tr><td>R32_USART3_DATAR</td><td>0x40004804</td><td>UASRT3 数据寄存器</td><td>0x000000XX</td></tr><tr><td>R32_USART3_BRR</td><td>0x40004808</td><td>UASRT3 波特率寄存器</td><td>0x00000000</td></tr><tr><td>R32_USART3_CTLR1</td><td>0x4000480C</td><td>UASRT3 控制寄存器 1</td><td>0x00000000</td></tr><tr><td>R32_USART3_CTLR2</td><td>0x40004810</td><td>UASRT3 控制寄存器 2</td><td>0x00000000</td></tr><tr><td>R32_USART3_CTLR3</td><td>0x40004814</td><td>UASRT3 控制寄存器 3</td><td>0x00000000</td></tr><tr><td>R32_USART3_GPR</td><td>0x40004818</td><td>UASRT3 保护时间和预分频寄存器</td><td>0x00000000</td></tr></table>

表 18-5 UART4 相关寄存器列表  

<table><tr><td>名称</td><td>访问地址</td><td>描述</td><td>复位值</td></tr><tr><td>R32_UART4_STAT</td><td>0x40004C00</td><td>UART4 状态寄存器</td><td>0x000000C0</td></tr><tr><td>R32_UART4_DATAR</td><td>0x40004C04</td><td>UART4 数据寄存器</td><td>0x000000XX</td></tr><tr><td>R32_UART4_BRR</td><td>0x40004C08</td><td>UART4 波特率寄存器</td><td>0x00000000</td></tr><tr><td>R32_UART4_CTLR1</td><td>0x40004C0C</td><td>UART4 控制寄存器 1</td><td>0x00000000</td></tr><tr><td>R32_UART4_CTLR2</td><td>0x40004C10</td><td>UART4 控制寄存器 2</td><td>0x00000000</td></tr><tr><td>R32_UART4_CTLR3</td><td>0x40004C14</td><td>UART4 控制寄存器 3</td><td>0x00000000</td></tr><tr><td>R32_UART4_GPR</td><td>0x40004C18</td><td>UART4 保护时间和预分频寄存器</td><td>0x00000000</td></tr></table>

表 18-6 UART5 相关寄存器列表  

<table><tr><td>名称</td><td>访问地址</td><td>描述</td><td>复位值</td></tr><tr><td>R32_UART5_STAT</td><td>0x40005000</td><td>UART5状态寄存器</td><td>0x000000C0</td></tr><tr><td>R32_UART5_DATAR</td><td>0x40005004</td><td>UART5数据寄存器</td><td>0x000000XX</td></tr><tr><td>R32_UART5_BRR</td><td>0x40005008</td><td>UART5波特率寄存器</td><td>0x00000000</td></tr><tr><td>R32_UART5_CTLR1</td><td>0x4000500C</td><td>UART5控制寄存器1</td><td>0x00000000</td></tr><tr><td>R32_UART5_CTLR2</td><td>0x40005010</td><td>UART5控制寄存器2</td><td>0x00000000</td></tr><tr><td>R32_UART5_CTLR3</td><td>0x40005014</td><td>UART5控制寄存器3</td><td>0x00000000</td></tr><tr><td>R32_UART5_GPR</td><td>0x40005018</td><td>UART5保护时间和预分频寄存器</td><td>0x00000000</td></tr></table>

表 18-7 UART6 相关寄存器列表  

<table><tr><td>名称</td><td>访问地址</td><td>描述</td><td>复位值</td></tr><tr><td>R32_UART6_STAT</td><td>0x40001800</td><td>UART6状态寄存器</td><td>0x000000C0</td></tr><tr><td>R32_UART6_DATAR</td><td>0x40001804</td><td>UART6数据寄存器</td><td>0x000000XX</td></tr><tr><td>R32_UART6_BRR</td><td>0x40001808</td><td>UART6波特率寄存器</td><td>0x00000000</td></tr><tr><td>R32_UART6_CTLR1</td><td>0x4000180C</td><td>UART6控制寄存器1</td><td>0x00000000</td></tr><tr><td>R32_UART6_CTLR2</td><td>0x40001810</td><td>UART6控制寄存器2</td><td>0x00000000</td></tr><tr><td>R32_UART6_CTLR3</td><td>0x40001814</td><td>UART6控制寄存器3</td><td>0x00000000</td></tr><tr><td>R32_UART6_GPR</td><td>0x40001818</td><td>UART6保护时间和预分频寄存器</td><td>0x00000000</td></tr></table>

表 18-8 UART7 相关寄存器列表  

<table><tr><td>名称</td><td>访问地址</td><td>描述</td><td>复位值</td></tr><tr><td>R32_UART7_STAT</td><td>0x40001C00</td><td>UART7 状态寄存器</td><td>0x000000C0</td></tr><tr><td>R32_UART7_DATAR</td><td>0x40001C04</td><td>UART7 数据寄存器</td><td>0x000000XX</td></tr><tr><td>R32_UART7_BRR</td><td>0x40001C08</td><td>UART7 波特率寄存器</td><td>0x00000000</td></tr><tr><td>R32_UART7_CTLR1</td><td>0x40001C0C</td><td>UART7 控制寄存器 1</td><td>0x00000000</td></tr><tr><td>R32_UART7_CTLR2</td><td>0x40001C10</td><td>UART7 控制寄存器 2</td><td>0x00000000</td></tr><tr><td>R32_UART7_CTLR3</td><td>0x40001C14</td><td>UART7 控制寄存器 3</td><td>0x00000000</td></tr><tr><td>R32_UART7_GPR</td><td>0x40001C18</td><td>UART7 保护时间和预分频寄存器</td><td>0x00000000</td></tr></table>

表 18-9 UART8 相关寄存器列表  

<table><tr><td>名称</td><td>访问地址</td><td>描述</td><td>复位值</td></tr><tr><td>R32_UART8_STAT</td><td>0x40002000</td><td>UART8状态寄存器</td><td>0x000000C0</td></tr><tr><td>R32_UART8_DATAR</td><td>0x40002004</td><td>UART8数据寄存器</td><td>0x000000XX</td></tr><tr><td>R32_UART8_BRR</td><td>0x40002008</td><td>UART8 波特率寄存器</td><td>0x00000000</td></tr><tr><td>R32_UART8_CTLR1</td><td>0x4000200C</td><td>UART8 控制寄存器 1</td><td>0x00000000</td></tr><tr><td>R32_UART8_CTLR2</td><td>0x40002010</td><td>UART8 控制寄存器 2</td><td>0x00000000</td></tr><tr><td>R32_UART8_CTLR3</td><td>0x40002014</td><td>UART8 控制寄存器 3</td><td>0x00000000</td></tr><tr><td>R32_UART8_GPR</td><td>0x40002018</td><td>UART8 保护时间和预分频寄存器</td><td>0x00000000</td></tr></table>

### 18.10.1 USART 状态寄存器（USARTx_STATR）（x=1/2/3/4/5/6/7/8）

偏移地址： $0 \times 0 0$

<table><tr><td>31</td><td>30</td><td>29</td><td>28</td><td>27</td><td>26</td><td>25</td><td>24</td><td>23</td><td>22</td><td>21</td><td>20</td><td>19</td><td>18</td><td>17</td><td>16</td></tr><tr><td colspan="16">Reserved</td></tr><tr><td>15</td><td>14</td><td>13</td><td>12</td><td>11</td><td>10</td><td>9</td><td>8</td><td>7</td><td>6</td><td>5</td><td>4</td><td>3</td><td>2</td><td>1</td><td>0</td></tr><tr><td colspan="5">Reserved</td><td>CTS</td><td>LBD</td><td>TXE</td><td>TC</td><td>RXNE</td><td>IDLE</td><td>ORE</td><td>NE</td><td>FE</td><td>PE</td><td></td></tr></table>

<table><tr><td>位</td><td>名称</td><td>访问</td><td>描述</td><td>复位值</td></tr><tr><td>[31:10]</td><td>Reserved</td><td>R0</td><td>保留。</td><td>0</td></tr><tr><td>9</td><td>CTS</td><td>RWO</td><td>CTS 状态改变标志。如果设置了 CTSE 位,当nCTS 输出状态改变时,该位将由硬件置高。由软件清零。如果 CTSIE 位已经被置位,则会产生中断。1: nCTS 状态线上存在变化;0: nCTS 状态线上没有变化。</td><td>0</td></tr><tr><td>8</td><td>LBD</td><td>RWO</td><td>LIN 断开检测标志。当检测到 LIN 断开时,该位被硬件置位。由软件清零。如果 LBDIE 已经被置位,则将会产生中断。1: 检测到 LIN 断开;0: 没有检测待 LIN 断开。</td><td>0</td></tr><tr><td>7</td><td>TXE</td><td>R0</td><td>发送数据寄存器空标志。当 TDR 寄存器中的的数据被硬件转移到移位寄存器的时候,该位被硬件置位。如果 TXEIE 已经被置位时,就会产生中断,对数据寄存器进行写操作,此位将会被复位。1: 数据已经被转移到移位寄存器;0: 数据还没被转移到移位寄存器。</td><td>1</td></tr><tr><td>6</td><td>TC</td><td>RWO</td><td>发送完成标志。当含有数据的一帧发送完成后,并且 TXE 被置位,则硬件将会此位置位,如果 TCIE 被置位,还会产生对应中断,软件读了此位再写数据寄存器则会清除此位。也可以直接写 0 来清除此位。1: 发送完成;0: 发送还未完成。</td><td>1</td></tr><tr><td>5</td><td>RXNE</td><td>RWO</td><td>读数据寄存器非空标志,当移位寄存器中的数据被转移到数据寄存器中,该位会被硬件置位。如果 RXNEIE 已经被置位,则还会产生对应的中断。对数据寄存器的读操作可以将该位清</td><td>0</td></tr><tr><td></td><td></td><td></td><td>除。也可以直接写0来清除该位。1:数据收到,能够读出;0:数据还没收到。</td><td></td></tr><tr><td>4</td><td>IDLE</td><td>R0</td><td>总线空闲标志。当总线空闲时,该位将会被硬件置位。如果IDLEIE已经被置位,则会产生对应的中断。读状态寄存器再读数据寄存器的操作会清除此位。1:总线正空闲;0:没有检测到总线空闲。注:此位不会被再次置位直到RXNE被置位。</td><td>0</td></tr><tr><td>3</td><td>ORE</td><td>R0</td><td>过载错误标志。当接收移位寄存器存在数据需要转到数据寄存器时,但是数据寄存器的接收域还有数据未读出时,此位将会被置位。如果RXNEIE被置位了,还会产生对应中断。1:发生过载错误;0:没有过载错误。注:发生过载错误时,数据寄存器的值不会丢失,但是移位寄存器的值会被覆盖。如果设置可EIE位,在多缓冲区通讯模式下,ORE标志位置位会产生中断。</td><td>0</td></tr><tr><td>2</td><td>NE</td><td>R0</td><td>噪声错误标志。当检测到噪声错误标志时,由硬件置位。读状态寄存器后,再读数据寄存器的操作会复位此位。1:检测到噪声;0:没有检测到噪声。注:该位不会产生中断。如果设置了EIE位,在多缓冲区通讯模式下,FE标志位置位会产生中断。</td><td>0</td></tr><tr><td>1</td><td>FE</td><td>R0</td><td>帧错误标志。当检测到同步错误,过多的噪声或者断开符,该位将会被硬件置位。读此位再读数据寄存器的操作会复位此位。1:检测到帧错误;0:没有检测到帧错误。注:该位不会产生中断,如果设置了EIE位,在多缓冲区通讯模式下,FE标志位置位会产生中断。</td><td>0</td></tr><tr><td>0</td><td>PE</td><td>R0</td><td>校验错误标志。在接收模式下,如果产生奇偶检验错误,硬件置位此位。读此位再读数据寄存器的操作会复位此位。在清除此位前,软件必须等RXNE标志位被置位。如果PEIE之前已经被置位,那么此位被置位会产生对应的中断。1:出现奇偶校验错误;0:没有检验错误。</td><td>0</td></tr></table>

### 18.10.2 USART 数据寄存器（USARTx_DATAR）（x=1/2/3/4/5/6/7/8）

偏移地址： $0 \times 0 4$

<table><tr><td>31</td><td>30</td><td>29</td><td>28</td><td>27</td><td>26</td><td>25</td><td>24</td><td>23</td><td>22</td><td>21</td><td>20</td><td>19</td><td>18</td><td>17</td><td>16</td></tr><tr><td colspan="16">Reserved</td></tr><tr><td>15</td><td>14</td><td>13</td><td>12</td><td>11</td><td>10</td><td>9</td><td>8</td><td>7</td><td>6</td><td>5</td><td>4</td><td>3</td><td>2</td><td>1</td><td>0</td></tr><tr><td colspan="7">Reserved</td><td colspan="9">DR[8:0]</td></tr></table>

<table><tr><td>位</td><td>名称</td><td>访问</td><td>描述</td><td>复位值</td></tr><tr><td>[31:9]</td><td>Reserved</td><td>R0</td><td>保留。</td><td>0</td></tr><tr><td>[8:0]</td><td>DR[8:0]</td><td>RW</td><td>数据寄存器。这个寄存器实际上是接收数据寄存器（RDR）和发送寄存器（TDR）两个寄存器组成，DR的读写操作起始分别是读接收寄存器（RDR）和写发送寄存器（TDR）。</td><td>X</td></tr></table>

### 18.10.3 USART 波特率寄存器（USARTx_BRR）（x=1/2/3/4/5/6/7/8）

偏移地址： $0 \times 0 8$

<table><tr><td>31</td><td>30</td><td>29</td><td>28</td><td>27</td><td>26</td><td>25</td><td>24</td><td>23</td><td>22</td><td>21</td><td>20</td><td>19</td><td>18</td><td>17</td><td>16</td></tr><tr><td colspan="16">Reserved</td></tr><tr><td>15</td><td>14</td><td>13</td><td>12</td><td>11</td><td>10</td><td>9</td><td>8</td><td>7</td><td>6</td><td>5</td><td>4</td><td>3</td><td>2</td><td>1</td><td>0</td></tr><tr><td colspan="12">DIV_Mantissa[11:0]</td><td colspan="4">DIV_Fraction[3:0]</td></tr></table>

<table><tr><td>位</td><td>名称</td><td>访问</td><td>描述</td><td>复位值</td></tr><tr><td>[31:16]</td><td>Reserved</td><td>R0</td><td>保留。</td><td>0</td></tr><tr><td>[15:4]</td><td>DIV_Mantissa[11:0]</td><td>RW</td><td>这12位定义了分频器除法因子的整数部分。</td><td>0</td></tr><tr><td>[3:0]</td><td>DIV_Fraction[3:0]</td><td>RW</td><td>这4位定义了分频器除法因子的小数部分。</td><td>0</td></tr></table>

### 18.10.4 USART 控制寄存器 1（USARTx_CTLR1）（x=1/2/3/4/5/6/7/8）

偏移地址： $0 \times 0 0$

<table><tr><td colspan="16">Reserved</td></tr><tr><td>15</td><td>14</td><td>13</td><td>12</td><td>11</td><td>10</td><td>9</td><td>8</td><td>7</td><td>6</td><td>5</td><td>4</td><td>3</td><td>2</td><td>1</td><td>0</td></tr><tr><td>Reserved</td><td>UE</td><td>M</td><td>WAKE</td><td>PCE</td><td>PS</td><td>PEIE</td><td>TXEIE</td><td>TCIE</td><td>RXNEIE</td><td>IDLEIE</td><td>TE</td><td>RE</td><td>RWU</td><td>SBK</td><td></td></tr></table>

<table><tr><td>位</td><td>名称</td><td>访问</td><td>描述</td><td>复位值</td></tr><tr><td>[31:14]</td><td>Reserved</td><td>R0</td><td>保留。</td><td>0</td></tr><tr><td>13</td><td>UE</td><td>RW</td><td>USART使能位。当此位被置位后,在当前字节传输完成后,USART的分频器和输出都会停止工作。</td><td>0</td></tr><tr><td>12</td><td>M</td><td>RW</td><td>字长位。1:9个数据位; 0:8个数据位。</td><td>0</td></tr><tr><td>11</td><td>WAKE</td><td>RW</td><td>唤醒位。此位决定了把USART唤醒的方法:1:地址标记; 0:总线空闲。</td><td>0</td></tr><tr><td>10</td><td>PCE</td><td>RW</td><td>校验位使能。对于接收方,就是进行对数据的奇偶校验;对于发送方,就是插入校验位。一旦设置了此位,只有当前字节传输完成后,校验位使能才生效。</td><td>0</td></tr><tr><td>9</td><td>PS</td><td>RW</td><td>奇偶校验选择。0表示偶校验,1表示奇校验。设置了该位后,只有当前字节传输完成后,校验位使能才生效。</td><td>0</td></tr><tr><td>8</td><td>PEIE</td><td>RW</td><td>奇偶检验中断使能位。对此位置位表示允许产生奇偶检验错误中断。</td><td>0</td></tr><tr><td>7</td><td>TXEIE</td><td>RW</td><td>发送缓冲区空中断使能。对此位置位表示允许产生发送缓冲区空中断。</td><td>0</td></tr><tr><td>6</td><td>TCIE</td><td>RW</td><td>发送完成中断使能。对此位置位表示允许产生发送完成中断。</td><td>0</td></tr><tr><td>5</td><td>RXNEIE</td><td>RW</td><td>接收缓冲区非空中断使能。对此位置位表示允许产生接收缓冲区非空中断。</td><td>0</td></tr><tr><td>4</td><td>IDLEIE</td><td>RW</td><td>总线空闲中断使能。对此位置位表示允许产生总线空闲中断。</td><td>0</td></tr><tr><td>3</td><td>TE</td><td>RW</td><td>发送使能。置此位会使能发送器。</td><td>0</td></tr><tr><td>2</td><td>RE</td><td>RW</td><td>接收使能。置此位会使能接收器,接收器开始检测RX引脚上的起始位。</td><td>0</td></tr><tr><td>1</td><td>RWU</td><td>RW</td><td>接收唤醒。该位决定是否把USART置于静默模式:1:接收器处于静默模式;0:接收器处于正常工作模式。注1:置RWU位之前,USART需要先接收一个数据字节,否则在静默模式下,不能被总线空闲唤醒;注2:当配置成地址标记唤醒时,在RXNE被置位时,不能用软件修改RWU位。</td><td>0</td></tr><tr><td>0</td><td>SBK</td><td>RW</td><td>发送帧断开字符控制位。置此位来发送一个帧断开字符。在断开帧的停止位时,由硬件复位。1:发送; 0:不发送。</td><td>0</td></tr></table>

### 18.10.5 USART 控制寄存器 2（USARTx_CTLR2）（x=1/2/3/4/5/6/7/8）

偏移地址： $0 \times 1 0$

<table><tr><td>31</td><td>30</td><td>29</td><td>28</td><td>27</td><td>26</td><td>25</td><td>24</td><td>23</td><td>22</td><td>21</td><td>20</td><td>19</td><td>18</td><td>17</td><td>16</td></tr><tr><td colspan="16">Reserved</td></tr><tr><td>15</td><td>14</td><td>13</td><td>12</td><td>11</td><td>10</td><td>9</td><td>8</td><td>7</td><td>6</td><td>5</td><td>4</td><td>3</td><td>2</td><td>1</td><td>0</td></tr><tr><td>Reserved</td><td>LINEN</td><td>STOP</td><td>CLKEN</td><td>CPOL</td><td>CPHA</td><td>LBCL</td><td>Reser ved</td><td>LBDIE</td><td>LBDL</td><td>Reser ved</td><td></td><td colspan="4">ADD[3:0]</td></tr></table>

<table><tr><td>位</td><td>名称</td><td>访问</td><td>描述</td><td>复位值</td></tr><tr><td>[31:15]</td><td>Reserved</td><td>R0</td><td>保留。</td><td>0</td></tr><tr><td>14</td><td>LINEN</td><td>RW</td><td>LIN模式使能位,置位则使能LIN模式。在LIN模式下,可以使用SBK位发送LIN同步断开符号,以及检测LIN同步断开符。</td><td>0</td></tr><tr><td>[13:12]</td><td>STOP</td><td>RW</td><td>停止位设置域。这两位来设置停止位。00:1个停止位;01:0.5个停止位;10:2个停止位;11:1.5个停止位。</td><td>00b</td></tr><tr><td>11</td><td>CLKEN</td><td>RW</td><td>时钟使能,使能CK引脚。1:使能;0:禁止。</td><td>0</td></tr><tr><td>10</td><td>CPOL</td><td>RW</td><td>时钟极性设置位。在同步模式下,可以用该位选择SLCK引脚上时钟输出的极性,和CPHA一起配合来产生需要的时钟/数据的采样关系。1:总线空闲时CK引脚上保持高电平;0:总线空闲时CK引脚上保持低电平。注:使能发送后此位不可被修改。</td><td>0</td></tr><tr><td>9</td><td>CPHA</td><td>RW</td><td>时钟相位设置位。在同步模式下,可以用该位选择SLCK引脚上的时钟输出的相位,和CPOL位一起配合来产生需要的时钟/数据的采样关系。1:在时钟的第二个边沿进行数据捕获;0:在时钟的第一个边沿进行数据捕获。注:使能发送后此位不可被修改。</td><td>0</td></tr><tr><td>8</td><td>LBCL</td><td>RW</td><td>最后一个时钟脉冲控制位。在同步模式下,使用该位来控制是否在CK引脚上输出最后发送的那个数据字节对应的时钟脉冲;1:最后一位数据的时钟脉冲不从CK输出;0:最后一位数据的时钟脉冲会从CK输出。注:使能发送后此位不可被修改。</td><td>0</td></tr><tr><td>7</td><td>Reserved</td><td>RW</td><td>保留。</td><td>0</td></tr><tr><td>6</td><td>LBDIE</td><td>RW</td><td>LIN断开符检测中断使能,该位置位会使能LBD引起的中断;</td><td>0</td></tr><tr><td>5</td><td>LBDL</td><td>RW</td><td>LIN断开符检测长度,该位用来选择是11位还是10位的断开符检测。1:11位的断开符检测;0:10位的断开符检测。</td><td>0</td></tr><tr><td>4</td><td>Reserved</td><td>RW</td><td>保留。</td><td>0</td></tr><tr><td>[3:0]</td><td>ADD[3:0]</td><td>RW</td><td>地址域,用来设置本设备的USART节点地址。在多处理器通讯下的静默模式中使用的,使用地址标记来唤醒某个USART设备。</td><td>0</td></tr></table>

### 18.10.6 USART 控制寄存器 3（USARTx_CTLR3）（x=1/2/3/4/5/6/7/8）

偏移地址： $0 \times 1 4$

<table><tr><td>31</td><td>30</td><td>29</td><td>28</td><td>27</td><td>26</td><td>25</td><td>24</td><td>23</td><td>22</td><td>21</td><td>20</td><td>19</td><td>18</td><td>17</td><td>16</td></tr><tr><td colspan="16">Reserved</td></tr><tr><td>15</td><td>14</td><td>13</td><td>12</td><td>11</td><td>10</td><td>9</td><td>8</td><td>7</td><td>6</td><td>5</td><td>4</td><td>3</td><td>2</td><td>1</td><td>0</td></tr><tr><td colspan="4">Reserved</td><td>CTSIE</td><td>CTSE</td><td>RTSE</td><td>DMAT</td><td>DMAR</td><td>SCEN</td><td>NACK</td><td>HDSEL</td><td>IRLP</td><td>IREN</td><td>EIE</td><td></td></tr></table>

<table><tr><td>位</td><td>名称</td><td>访问</td><td>描述</td><td>复位值</td></tr><tr><td>[31:11]</td><td>Reserved</td><td>RO</td><td>保留。</td><td>0</td></tr><tr><td>10</td><td>CTSIE</td><td>RW</td><td>CTSIE中断使能位,置此位时在CTS被置位时会产生中断。</td><td>0</td></tr><tr><td>9</td><td>CTSE</td><td>RW</td><td>CTS使能位,置此位会使能CTS流控。</td><td>0</td></tr><tr><td>8</td><td>RTSE</td><td>RW</td><td>RTS使能位,置此位会使能RTS流控。</td><td>0</td></tr><tr><td>7</td><td>DMAT</td><td>RW</td><td>DMA发送使能位。此位置1在发送时使用DMA。</td><td>0</td></tr><tr><td>6</td><td>DMAR</td><td>RW</td><td>DMA接收使能位。此位置1在接收时使用DMA。</td><td>0</td></tr><tr><td>5</td><td>SCEN</td><td>RW</td><td>智能卡模式使能位,置1使能智能卡模式。</td><td>0</td></tr><tr><td>4</td><td>NACK</td><td>RW</td><td>智能卡NACK使能位,置此位在校验错误出现时,发送NACK。</td><td>0</td></tr><tr><td>3</td><td>HDSEL</td><td>RW</td><td>半双工模式选择位,置此位选择半双工模式。</td><td>0</td></tr><tr><td>2</td><td>IRLP</td><td>RW</td><td>红外低功耗选择位,置此位在选择红外线时,启用低功耗模式。</td><td>0</td></tr><tr><td>1</td><td>IREN</td><td>RW</td><td>红外线使能位,置此位使能红外模式。</td><td>0</td></tr><tr><td>0</td><td>EIE</td><td>RW</td><td>错误使能中断位,置此位后,在DMAR被置位的前提下,如果FE、ORE或NE被置位,就会产生中断。</td><td>0</td></tr></table>

### 18.10.7 USART 保护时间和预分频寄存器（USARTx_GPR）（x=1/2/3/4/5/6/7/8）

偏移地址： $0 \times 1 8$

<table><tr><td>31</td><td>30</td><td>29</td><td>28</td><td>27</td><td>26</td><td>25</td><td>24</td><td>23</td><td>22</td><td>21</td><td>20</td><td>19</td><td>18</td><td>17</td><td>16</td></tr><tr><td colspan="16">Reserved</td></tr><tr><td>15</td><td>14</td><td>13</td><td>12</td><td>11</td><td>10</td><td>9</td><td>8</td><td>7</td><td>6</td><td>5</td><td>4</td><td>3</td><td>2</td><td>1</td><td>0</td></tr><tr><td colspan="8">GT[7:0]</td><td colspan="8">PSC[7:0]</td></tr></table>

<table><tr><td>位</td><td>名称</td><td>访问</td><td>描述</td><td>复位值</td></tr><tr><td>[31:16]</td><td>Reserved</td><td>RO</td><td>保留。</td><td>0</td></tr><tr><td>[15:8]</td><td>GT[7:0]</td><td>RW</td><td>保护时间值域。该域规定了以波特率时钟为单位的保护时间。在智能卡模式下,当保护时间过去后,才会设置发送完成标志。</td><td>0</td></tr><tr><td>[7:0]</td><td>PSC[7:0]</td><td>RW</td><td>预分频器值域。在红外低功耗模式下,源时钟被该值(全部8位有效)分频,值为0时表示保留;在红外正常模式下,此位只能被设置为1;在智能卡模式下,源时钟被该值(低5位有效)</td><td>0</td></tr><tr><td></td><td></td><td></td><td>的两倍分频,来给智能卡提供时钟,值为0表示保留。</td><td></td></tr></table>

# 第 19 章 两线通信总线（I2C）

本章模块描述适用于CH32F2x、CH32V2x和CH32V3x微控制器全系列产品。

内部集成电路总线（I2C）广泛用在微控制器和传感器及其他片外模块的通讯上，它本身支持多主多从模式，仅仅使用两根线（SDA和SCL）就能以100KHz（标准）和400KHz（快速）两种速度通讯。I2C总线还兼容SMBus协议，不仅支持I2C的时序，还支持仲裁、定时和DMA，拥有CRC校验功能。

## 19.1 主要特征

支持主模式和从模式  
$\bullet$ 支持 7位或10位地址  
$\bullet$ 从设备支持双 7 位地址  
$\bullet$ 支持两种速度模式：100KHz 和 400KHz  
$\bullet$ 多种状态模式，多种错误标志  
支持加长的时钟功能  
2个中断向量  
支持 DMA  
$\bullet$ 支持 PEC  
兼容 SMBus

## 19.2 概述

I2C 是半双工的总线，它同时只能运行在下列四种模式中之一：主设备发送模式、主设备接收模式、从设备发送模式和从设备接收模式。I2C模块默认工作在从模式，在产生起始条件后，会自动地切换到主模式，当仲裁丢失或产生停止信号后，会切换到从模式。I2C模块支持多主机功能。工作在主模式时，I2C模块会主动发出数据和地址。数据和地址都以 8 位为单位进行传输，高位在前，低位在后，在起始事件后的是一个字节（7位地址模式下）或两个字节（10位地址模式下）地址，主机每发送8位数据或地址，从机需要回复一个应答 ACK，即把 SDA总线拉低，如图 19-1 所示。

![](images/11f8554ecd6d1b55bbdaf3436af135fb6e848c8e240f9598f68922c63693a9a9.jpg)  
图 19-1 I2C 时序图

为了正常使用必须给I2C输入正确的时钟，其中标准模式下，输入时钟最低为 2MHz,在快速模式下，输入时钟最低为4MHz。

图 19-2 是 I2C 模块功能框图。

![](images/c9418045eada5e5debf450f67126537eae76978bd3c35366f031db4a7b9541a0.jpg)  
图 19-2 I2C 功能框图

## 19.3 主模式

主模式时，I2C模块主导数据传输并输出时钟信号，数据传输以开始事件开始，以结束事件结束。使用主模式通讯的步骤为：

在控制寄存器 2（R16_I2Cx_CTLR2）和时钟控制寄存器（R16_I2Cx_CKCFGR）中设置正确的时钟；

在上升沿寄存器（R16_I2Cx_RTR）设置合适的上升沿；

在控制寄存器（R16_I2Cx_CTLR1）中置 PE 位启动外设；

在控制寄存器（R16_I2Cx_CTLR1）中置 START 位，产生起始事件。

在置 START 位后，I2C 模块会自动切换到主模式，MSL 位会置位，产生起始事件，在产生起始事件后，SB 位会置位，如果 ITEVTEN 位（在 R16_I2Cx_CTLR2）被置位，则会产生中断。此时应该读取状态寄存器 1（R16_I2Cx_STAR1），写从地址到数据寄存器后，SB 位会自动清除；

5）如果是使用 10 位地址模式，那么写数据寄存器发送头序列（头序列为 $1 1 1 1 0 \times \times 0 6$ ，其中的 xx 位是10位地址的最高两位）。

在发送完头序列之后，状态寄存器的ADD10 位会被置位，如果ITEVTEN位已经置位，则会产生中断，此时应读取 R16_I2Cx_STAR1 寄存器后，写第二个地址字节到数据寄存器后，清除 ADD10 位。

然后写数据寄存器发送第二个地址字节，在发送完第二个地址字节后，状态寄存器的 ADDR 位会被置位，如果 ITEVTEN 位已经置位，则会产生中断，此时应读取 R16_I2Cx_STAR1 寄存器后再读一次R16_I2Cx_STAR2 寄存器以清除 ADDR 位；

如果使用的是7位地址模式，那么写数据寄存器发送地址字节，在发送完地址字节后，状态寄存器的 ADDR位会被置位，如果ITEVTEN位已经置位，则会产生中断，此时应读取 R16_I2Cx_STAR1寄存器后再读一次 R16_I2Cx_STAR2 寄存器以清除 ADDR 位；

在7位地址模式下，发送的第一个字节为地址字节，头 7位代表的是目标从设备地址，第 8位决定了后续报文的方向，0代表是主设备写入数据到从设备，1代表是主设备向从设备读取信息。

在10位地址模式下，如图18-3所示，在发送地址阶段，第一个字节为 $1 1 1 1 0 \times \times 0$ ，xx 为 10 位地址的最高 2 位，第二个字节为 10 位地址的低 8 位。若后续进入主设备发送模式，则继续发送数据；若后续准备进入主设备接收模式，则需要重新发送一个起始条件，跟随发送一个字节为 $1 1 1 1 0 \times \times 1$ ，然后进入主设备接收模式。

![](images/fc6a38dbb5c163c5261a812e8062fc8b215c2792d32437883db7d4221a2dfef8.jpg)  
图 19-3 10 位地址时主机收发数据示意图

#### 主发送模式：

主设备内部的移位寄存器将数据从数据寄存器发送到 SDA线上，当主设备接收到 ACK时，状态寄存器 1（R16_I2Cx_STAR1）的 TxE 被置位，如果 ITEVTEN 和 ITBUFEN 被置位，还会产生中断。向数据寄存器写入数据将会清除 TxE 位。

如果 TxE 位被置位且上次发送数据之前没有新的数据被写入数据寄存器，那么 BTF 位会被置位,在其被清除之前，SCL将保持低电平，读R16_I2Cx_STAR1后，向数据寄存器写入数据将会清除BTF位。

![](images/65b7e7d92c850983bba90d6a55659a49ac44ee4808983a549dec1cd600a151f4.jpg)  
图 19-4 主发送器传送序列图

#### 主接收模式：

I2C 模块会从SDA线接收数据，通过移位寄存器写进数据寄存器。在每个字节之后，如果 ACK位被置位，那么 I2C 模块将会发出一个应答低电平，同时 RxNE 位会被置位，如果 ITEVTEN 和 ITBUFEN被置位，还会产生中断。如果RxNE被置位且在新的数据被接收前，原有的数据没有被读出，则BTF位将被置位，在清除BTF之前，SCL将保持低电平，读取 R16_I2Cx_STAR1后，再读取数据寄存器将会清除 BTF 位。

图19-5 接收器传送序列图  
![](images/c4a140b054e570bfc4ad20e1c3b6c571415794e0f5d19127c3e739e596428715.jpg)  
说明：S=Start(起始条件)， $\mathsf { s r } =$ 重复的起始条件，P=Stop（停止条件），A=响应，NA=非响应，EVTx=事件（ITEVFEN=1时产生中断）  
EVT5：SB=1，读SR1然后将地址写入DR寄存器将清除该事件。  
EVT6：ADDR=1,读SR1然后读SR2将消除该事件。在10位主接收模式下，该事件后应设置CR2的START=1。  
EVT6_1：没有对应的事件标志，只适于接收1个字节的情况。恰好在EVT6之后（即清除了ADDR之后），要清除响应和停止条件的产生位。  
EVT7：RxNE=1，读DR寄存器清除该事件。   
EVT7_1：RxNE=1，读DR寄存器清除该事件。设置ACK=0和STOP请求。  
EVT9：ADDR10=1，读SR1然后写入DR寄存器将清除该事件。

主设备在结束发送数据时，会主动发一个结束事件，即置 STOP位，I2C将切换至从模式。在接收模式时，主设备需要在最后一个数据位的应答位置NAK，接收到 NACK后，从设备释放对 SCL和 SDA线的控制；主设备就可以发送一个停止/重起始条件。注意，产生停止条件后，I2C模块将会自动切换至从模式。

## 19.4 从模式

从模式时，I2C模块能识别它自己的地址和广播呼叫地址。软件能控制开启或禁止广播呼叫地址的识别。一旦检测到起始事件，I2C模块将SDA的数据通过移位寄存器与自己的地址（位数取决于ENDUAL和ADDMODE）或广播地址（ENGC置位时）相比较，如果不匹配将会忽略，直到产生新的起始事件；如果与头序列相匹配，则会产生一个 ACK信号并等待第二个字节的地址；如果第二字节的地址也匹配或者7位地址情况下全段地址匹配，那么：

首先产生一个 ACK 应答；

ADDR 位被置位，如果 ITEVTEN 位已经置位，那么还会产生相应的中断；

如果使用的是双地址模式（ENDUAL 位被置位），还需要读取 DUALF 位来判断主机唤起的是哪一个地址。

从模式默认是接收模式，在接收的头序列的最后一位为 1，或者 7位地址最后一位为 1后（取决于第一次接收到头序列还是普通的7位地址），当接收到重复的起始条件时，I2C模块将进入到发送器模式，TRA 位将指示当前是接收器还是发送器模式。

#### 从发送模式：

在清除ADDR位后，I2C模块将字节从数据寄存器通过移位寄存器发送到 SDA线上。从设备保持SCL为低电平，直到ADDR位被清除且待发送数据已写入数据寄存器。（见下图中的 EVT1 和EVT3）。在收到一个应答ACK后，TxE位将被置位，如果设置了 ITEVTEN和 ITBUFEN，还会产生一个中断。如果 TxE 被置位但在下一个数据发送结束前没有新的数据被写入数据寄存器时，BTF 位将被置位。在清除BTF前，SCL将保持低电平，读取状态寄存器 1（R16_I2Cx_STAR1）后，再向数据寄存器写入数据将会清除BTF位。

![](images/fdfca52213a189a3738cdd37e08e7ebfe0717deda9a57dd7fcda7de746ca74ec.jpg)  
图19-6 从发送器的传送序列图

#### 从接收模式：

在ADDR被清除后，I2C模块将SDA上的数据通过移位寄存器存进数据寄存器，在每接收到一个字节后，I2C模块都会置一个ACK位，并置RxNE位。如果设置了 ITEVTEN和ITBUFEN，还会产生一个中断。如果RxNE被置位，且在接收到新的数据前旧的数据没有被读出，那么 BTF会被置位。在清除BTF位之前SCL会保持低电平。读取状态寄存器1（R16_I2Cx_STAR1）并读取数据寄存器里的数据会清除BTF位。

![](images/aa5261d5959e8dceff81374367281aa32fef7ef201144176f370b03413af95bc.jpg)  
图 19-7 从接收器的传送序列图

主设备在传输完最后一个数据字节后，将产生一个停止条件，当 I2C模块检测到停止事件时，将置STOPF位，如果设置了ITEVFEN位，还会产生一个中断。用户需要读取状态寄存器（R16_I2Cx_STAR1）再写控制寄存器（比如复位控制字 SWRST）来清除。（见上图中的 EVT4）。

## 19.5 错误

### 19.5.1 总线错误 BERR

在传输地址或数据期间，I2C模块检测到外部的起始或停止事件时，将产生一个总线错误。产生总线错误时，BERR位被置位，如果设置了ITERREN还会产生一个中断。在从模式下，数据被丢弃，硬件释放总线。如果是起始信号，硬件会认为是重启信号，开始等待地址或停止信号；如果是停止信号，则提前按正常的停止条件操作。在主模式下，硬件不会释放总线，同时不影响当前传输，由用户代码决定是否中止传输。

### 19.5.2 应答错误 AF

当I2C模块检测到一个字节后没有应答时，会产生应答错误。产生应答错误时：AF 会被置位，如果设置了 ITERREN 还会产生一个中断；遇到 AF 错误，如果 I2C 模块工作在从模式，硬件必须释放总线，如果处于主模式，软件必须生成一个停止事件。

### 19.5.3 仲裁丢失 ARLO

当I2C模块检测到仲裁丢失时，产生仲裁丢失错误。产生仲裁丢失错误时：ARLO 位被置位，如果设置了ITERREN还会产生一个中断；I2C模块切换到从模式，并不再响应针对它的从地址发起的传输，除非有主机发起新的起始事件；硬件会释放总线。

### 19.5.4 过载/欠载错误 OVR

#### 过载错误：

在从机模式下，如果禁止时钟延长，I2C模块正在接收数据，如果已经接受到一个字节的数据，但是上一次接收到数据还没有被读出，则会产生过载错误。发生过载错误时，最后收到的字节将被丢弃，发送方应当重发最后一次发送的字节。

#### 欠载错误：

在从模式下，如果禁止时钟延长，I2C 模块正在发送数据，如果在下一个字节的时钟到来之前新的数据还没有被写入到数据寄存器，那么将产生欠载错误。在发生欠载错误时，前一次数据寄存器里的数据将被发送两次，如果发生欠载错误，那么接收方应该丢弃重复收到的数据。为了不产生欠载错误，I2C模块应当在下一个字节的第一个上升沿之前将数据写入数据寄存器。

## 19.6 时钟延长

如果禁止时钟延长，那么就存在发生过载/欠载错误的可能。但如果使能了时钟延长：

$\bullet$ 在发送模式下，如果TxE置位且BTF置位，SCL将一直为低，一直等待用户读取状态寄存器，并向数据寄存器写入待发送的数据；  
$\bullet$ 在接收模式下，如果RxNE置位且BTF置位，那么 SCL在接收到数据后将保持低，直到用户读取状态寄存器，并读取数据寄存器；

由此可见，使能时钟延长可以避免出现过载/欠载错误。

## 19.7 SMBus

SMBus也是一种双线接口，它一般应用在系统和电源管理之间。SMBus和 I2C有很多相似的地方，例如 SMBus 使用和 I2C 一样的 7 位地址模式，以下是 SMBus 和 I2C 的共同点：

1） 主从通信模式，主机提供时钟，支持多主多从；  
2） 两线通讯结构，其中 SMBus 可选一个警示线；

3） 都支持 7 位地址格式。

同时SMBus和I2C也存在区别：

1） I2C 支持的速度最高 $4 0 0 \mathsf { K H z }$ ，而 SMBus 支持的最高是 100KHz，且 SMBus 有最小 10KHz 的速度限制；  
2） SMBus 的时钟为低超过 35mS 时，会报超时，但 I2C 无此限制；  
3） SMBus 有固定的逻辑电平，而 I2C 没有，取决于 VDD；  
4） SMBus 有总线协议，而 I2C 没有。

SMBus还包括设备识别、地址解析协议、唯一的设备标识符、SMBus 提醒和各种总线协议，具体请参考SMBus规范2.0版本。当使用SMBus时，只需要置控制寄存器的 SMBus位，按需配置SMBTYPE 位和 ENAARP 位。

## 19.8 中断

每个I2C模块都有两种中断向量，分别是事件中断和错误中断。两种中断支持图 19-4的中断源。

![](images/cf1582c4fd9094a93c35022b99b472f15e1ca02b0eed17370217ea2e8bdda738.jpg)  
图 19-8 I2C 中断请求

## 19.9 DMA

可以使用DMA来进行批量数据的收发。使用 DMA时不能对控制寄存器的 ITBUFEN 位进行置位。

利用 DMA 发送

通过将 CTLR2 寄存器的 DMAEN 位置位可以激活 DMA 模式。只要 TxE 位被置位，数据将由 DMA 从设定的内存装载进I2C的数据寄存器。需要进行以下设定来为 I2C分配通道。

1） 向 DMA_PADDRx 寄存器设置 I2Cx_DATAR 寄存器地址，DMA_MADDRx 寄存器中设置存储器地址，这样在每个 TxE 事件后，数据将从存储器送至 I2Cx_DATAR 寄存器。  
2） 在 DMA_CNTRx 寄存器中设置所需的传输字节数。在每个 TxE 事件后，此值将被递减。  
3） 利用 DMA_CFGRx 寄存器中的 PL[0:1]位配置通道优先级。  
4） 设置DMA_CFGRx寄存器中的DIR位，并根据应用要求可以配置在整个传输完成一半或全部完成时发出中断请求。  
5） 通过设置 DMA_CFGRx 寄存器上的 EN 位激活通道。

当DMA控制器中设置的数据传输字节数目已经完成时，DMA控制器给 I2C接口发送一个传输结束的 EOT/ EOT_1信号。在中断允许的情况下，将产生一个 DMA中断。

#### 利用DMA接收

置位 CTLR2 寄存器的 DMAEN 后即可进行 DMA 接收模式。使用 DMA 接收时，DMA 将数据寄存器里的数据传送到预设的内存区域。需要以下步骤来为 I2C 分配通道。

1） 向 DMA_PADDRx 寄存器设置 $1 2 0 \times$ _DATAR寄存器地址，DMA_MADDRx寄存器中设置存储器地址，这样在每个 RxNE 事件后，数据将从 $1 2 0 \times$ _DATAR 寄存器写入存储器。  
2） 在 DMA_CNTRx 寄存器中设置所需的传输字节数。在每个 RxNE 事件后，此值将被递减。  
3） 用 DMA_CFGRx 寄存器中的 PL[0:1]配置通道优先级。  
4） 清除 DMA_CFGRx 寄存器中的 DIR 位，根据应用要求可以设置在数据传输完成一半或全部完成时发出中断请求。  
5） 设置 DMA_CFGRx 寄存器中的 EN 位激活该通道。

当DMA控制器中设置的数据传输字节数目已经完成时，DMA控制器给 I2C接口发送一个传输结束的 EOT/EOT_1 信号。在中断允许的情况下，将产生一个 DMA 中断。

## 19.10 包校验错误

包错误校验(PEC)是为了提供传输的可靠性而增加一项 CRC8 校验的步骤，使用以下多项式对每一位串行数据进行计算：

$$
\mathrm {C} = \mathrm {X} ^ {8} + \mathrm {X} ^ {2} + \mathrm {X} + 1
$$

PEC计算是由控制寄存器的ENPEC位激活，对所有信息字节进行计算，包括地址和读写位在内。在发送时，启用PEC会在最后一字节数据之后加上一个字节的 CRC8 计算结果；而在接收模式，在最后一字节被认为是 CRC8 校验结果，如果和内部的计算结果不符合，就会回复一个 NAK，如果是主接收器，无论校验结果正确与否，都会回复一个 NAK。

## 19.11 调试模式

当系统进入调试模式之后，可以通过 DEBUG 模块的 DBG_I2Cx_SMBUS_TIMEOUT 位来决定I2CSMBus 的超时控制是继续工作还是停止。

## 19.12 寄存器描述

表 19-1 I2C1 相关寄存器列表  

<table><tr><td>名称</td><td>访问地址</td><td>描述</td><td>复位值</td></tr><tr><td>R16_I2C1_CTLR1</td><td>0x40005400</td><td>I2C1 控制寄存器 1</td><td>0x0000</td></tr><tr><td>R16_I2C1_CTLR2</td><td>0x40005404</td><td>I2C1 控制寄存器 2</td><td>0x0000</td></tr><tr><td>R16_I2C1_OADDR1</td><td>0x40005408</td><td>I2C1 地址寄存器 1</td><td>0x0000</td></tr><tr><td>R16_I2C1_OADDR2</td><td>0x4000540C</td><td>I2C1 地址寄存器 2</td><td>0x0000</td></tr><tr><td>R16_I2C1_DATAR</td><td>0x40005410</td><td>I2C1 数据寄存器</td><td>0x0000</td></tr><tr><td>R16_I2C1_STAR1</td><td>0x40005414</td><td>I2C1 状态寄存器 1</td><td>0x0000</td></tr><tr><td>R16_I2C1_STAR2</td><td>0x40005418</td><td>I2C1 状态寄存器 2</td><td>0x0000</td></tr><tr><td>R16_I2C1_CKCFGR</td><td>0x4000541C</td><td>I2C1 时钟寄存器</td><td>0x0000</td></tr><tr><td>R16_I2C1_RTR</td><td>0x40005420</td><td>I2C1 上升时间寄存器</td><td>0x0002</td></tr></table>

表 19-2 I2C2 相关寄存器列表  

<table><tr><td>名称</td><td>访问地址</td><td>描述</td><td>复位值</td></tr><tr><td>R16_I2C2_CTLR1</td><td>0x40005800</td><td>I2C2 控制寄存器 1</td><td>0x0000</td></tr><tr><td>R16_I2C2_CTLR2</td><td>0x40005804</td><td>I2C2 控制寄存器 2</td><td>0x0000</td></tr><tr><td>R16_I2C2_OADDR1</td><td>0x40005808</td><td>I2C2 地址寄存器 1</td><td>0x0000</td></tr><tr><td>R16_I2C2_OADDR2</td><td>0x4000580C</td><td>I2C2 地址寄存器 2</td><td>0x0000</td></tr><tr><td>R16_I2C2_DATAR</td><td>0x40005810</td><td>I2C2 数据寄存器</td><td>0x0000</td></tr><tr><td>R16_I2C2_STAR1</td><td>0x40005814</td><td>I2C2 状态寄存器 1</td><td>0x0000</td></tr><tr><td>R16_I2C2_STAR2</td><td>0x40005818</td><td>I2C2 状态寄存器 2</td><td>0x0000</td></tr><tr><td>R16_I2C2_CKCFGR</td><td>0x4000581C</td><td>I2C2 时钟寄存器</td><td>0x0000</td></tr><tr><td>R16_I2C2_RTR</td><td>0x40005820</td><td>I2C2 上升时间寄存器</td><td>0x0002</td></tr></table>

### 19.12.1 I2C 控制寄存器（I2Cx_CTLR1）（x=1/2）

偏移地址： $0 \times 0 0$

<table><tr><td>15</td><td>14</td><td>13</td><td>12</td><td>11</td><td>10</td><td>9</td><td>8</td><td>7</td><td>6</td><td>5</td><td>4</td><td>3</td><td>2</td><td>1</td><td>0</td></tr><tr><td>SWRST</td><td>Reserved</td><td>ALERT</td><td>PEC</td><td>POS</td><td>ACK</td><td>STOP</td><td>START</td><td>NOSTRETCH</td><td>ENGC</td><td>ENPEC</td><td>ENARP</td><td>SMBTYPE</td><td>Reser ved</td><td>SMBUS</td><td>PE</td></tr></table>

<table><tr><td>位</td><td>名称</td><td>访问</td><td>描述</td><td>复位值</td></tr><tr><td>15</td><td>SWRST</td><td>RW</td><td>软件重置,用户代码置此位会使I2C外设重置。在复位前确定I2C总线的引脚被释放,总线处于空闲状态。注:该位可以在总线上没有检测到停止条件但是busy位为1时,重置I2C模块。</td><td>0</td></tr><tr><td>14</td><td>Reserved</td><td>RO</td><td>保留。</td><td>0</td></tr><tr><td>13</td><td>ALERT</td><td>RW</td><td>SMBus提醒位,用户代码可以设置此位或清除此位;当PE置位后,此位可以被硬件清除。1:驱动SMBusALERT引脚使其变低,响应地址头应紧跟在ACK信号后面;0:释放SMBusALERT引脚使其变高,响应地址头应紧跟在NACK信号后面。</td><td>0</td></tr><tr><td>12</td><td>PEC</td><td>RW</td><td>数据包出错检测使能位,置此位启用数据包出错检测。用户代码可以对此位置位或清零;当PEC被传输后,产生开始或结束信号,或PE位清0时,硬件清零该位;1:带PEC;0:不带PEC。注:仲裁丢失时,PEC失效。</td><td>0</td></tr><tr><td>11</td><td>POS</td><td>RW</td><td>ACK和PEC位置设置位,该位可以被用户代码置位或清零,在PE被清零后,可以被硬件清除;1:ACK位控制在移位寄存器里接收的下一个字节的ACK或NAK。PEC移位寄存器里接收的下一字节是PEC;</td><td>0</td></tr><tr><td></td><td></td><td></td><td>0: ACK位控制当前移位寄存器内正在接受的字节的 ACK或NAK。PEC位表明当位前移位寄存器的字节是PEC。注:POS位在2字节数据接收中的用法如下:必须在接收之前配置好。为了NACK第2个字节,必须在清除ADDR位后立刻清除ACK位;为了检测第二个字节的PEC,必须在ADDR事件发生后,配置POS位后设置PEC位。</td><td></td></tr><tr><td>10</td><td>ACK</td><td>RW</td><td>应答使能位,该位可以被用户代码置位或清零,当PE位被置位时,该位可以被硬件清除;1:在接收到一个字节后返回一个应答;0:不设应答。</td><td>0</td></tr><tr><td>9</td><td>STOP</td><td>RW</td><td>停止事件产生位,该位可以被用户代码置位或清零,或当检测到停止事件时,由硬件清除,或检测到超时错误时,由硬件将其置位。主模式下:1:在当前字节传输或当前起始条件发出后产生停止事件;0:无停止事件产生。从模式下:1:在当前字节传输后释放SCL和SDA线;0:无停止事件产生。</td><td>0</td></tr><tr><td>8</td><td>START</td><td>RW</td><td>起始事件产生位,该位可以被用户代码置位或清零,当起始条件发出后或PE被清零时,由硬件清零。主模式下:1:重复产生起始事件;0:无起始事件产生。从模式下:1:当总线空闲时,产生起始事件;0:无起始事件产生。</td><td>0</td></tr><tr><td>7</td><td>NOSTRETCH</td><td>RW</td><td>禁止时钟延长位,此位用于在ADDB或BTF标志被置位的情况下,禁止从模式下的时钟延长,直至被软件清零。1:禁止时钟延长;0:允许时钟延长。</td><td>0</td></tr><tr><td>6</td><td>ENGC</td><td>RW</td><td>广播呼叫使能位,置此位使能广播呼叫,应答广播地址00h。</td><td>0</td></tr><tr><td>5</td><td>ENPEC</td><td>RW</td><td>PEC使能位,置此位开启PEC计算。</td><td>0</td></tr><tr><td>4</td><td>ENARP</td><td>RW</td><td>ARP使能位,置此位使能ARP。如果SMBTYPE=0,则使用SMBus设备的默认地址;如果SMBTYPE=1,则使用SMBus的主地址。</td><td>0</td></tr><tr><td>3</td><td>SMBTYPE</td><td>RW</td><td>SMBus设备类型,置1为SMBus主设备,置0为SMBus从设备。</td><td>0</td></tr><tr><td>2</td><td>Reserved</td><td>RO</td><td>保留。</td><td>0</td></tr><tr><td>1</td><td>SMBUS</td><td>RW</td><td>SMBus模式选择位,置1为使用SMBus模式,</td><td>0</td></tr><tr><td></td><td></td><td></td><td>置0为使用I2C模式。</td><td></td></tr><tr><td>0</td><td>PE</td><td>RW</td><td>I2C外设使能位。
1:启用I2C模块;
0:禁用I2C模块。</td><td>0</td></tr></table>

### 19.12.2 I2C 控制寄存器 2（I2Cx_CTLR2）（x=1/2）

偏移地址： $0 \times 0 4$

<table><tr><td>Reserved</td><td>LAST</td><td>DMAEN</td><td>ITBUFEN</td><td>ITEVTEN</td><td>ITERREN</td><td>Reserved</td><td>FREQ[5:0]</td></tr></table>

<table><tr><td>位</td><td>名称</td><td>访问</td><td>描述</td><td>复位值</td></tr><tr><td>[15:13]</td><td>Reserved</td><td>R0</td><td>保留。</td><td>0</td></tr><tr><td>12</td><td>LAST</td><td>RW</td><td>DMA最后一次传输设置位。1:下一次DMA的EOT是最后的传输;0:下一次DMA的EOT不是最后的传输。注:该位在主接收模式使用,可以在最后一次接收数据时产生一个NAK。</td><td>0</td></tr><tr><td>11</td><td>DMAEN</td><td>RW</td><td>DMA请求使能位,置此位在TxE或RxEN被置位时允许DMA请求。</td><td>0</td></tr><tr><td>10</td><td>ITBUFEN</td><td>RW</td><td>缓冲器中断使能位。1:当TxE或RxEN被置位时,产生事件中断;0:当TxE或RxEN被置位时,不产生中断。</td><td>0</td></tr><tr><td>9</td><td>ITEVTEN</td><td>RW</td><td>时间中断使能位,置此位使能事件中断。在下列条件下,将产生此中断:SB=1(主模式);ADDR=1(主从模式);ADDR10=1(主模式);STOPF=1(从模式);BTF=1,但是没有TxE或RxEN事件;如果ITBUFEN=1,TxE事件为1;如果ITBUFEN=1,RxNE事件为1。</td><td>0</td></tr><tr><td>8</td><td>ITERREN</td><td>RW</td><td>出错中断使能位,置位表示允许出错中断。在下列条件下,将产生该中断;BERR=1;ARLO=1;AF=1;OVR=1;PECERR=1;TIMEOUT=1;SMBAlert=1。</td><td>0</td></tr><tr><td>[7:6]</td><td>Reserved</td><td>RO</td><td>保留。</td><td>0</td></tr><tr><td>[5:0]</td><td>FREQ[5:0]</td><td>RW</td><td>12C模块时钟频率域,必须输入正确的时钟频率以产生正确的时序,允许的范围在2-36MHz之间。必须设置在000010b到100100b之间,单位为MHz。</td><td>0</td></tr></table>

### 19.12.3 I2C 地址寄存器 1（I2Cx_OADDR1）（x=1/2）

偏移地址： $0 \times 0 8$

<table><tr><td>15</td><td>14</td><td>13</td><td>12</td><td>11</td><td>10</td><td>9</td><td>8</td><td>7</td><td>6</td><td>5</td><td>4</td><td>3</td><td>2</td><td>1</td><td>0</td></tr></table>

<table><tr><td>ADD MODE</td><td>MUST1</td><td>Reserved</td><td>ADD[9:8]</td><td>ADD[7:1]</td><td>ADD0</td></tr></table>

<table><tr><td>位</td><td>名称</td><td>访问</td><td>描述</td><td>复位值</td></tr><tr><td>15</td><td>ADDMODE</td><td>RW</td><td>地址模式。
1:10位从机地址（不响应7位地址）;
0:7位从机地址（不响应10位地址）。</td><td>0</td></tr><tr><td>14</td><td>MUST1</td><td>RW1</td><td>必须始终由软件写1。</td><td>0</td></tr><tr><td>[13:10]</td><td>Reserved</td><td>RO</td><td>保留。</td><td>0</td></tr><tr><td>[9:8]</td><td>ADD[9:8]</td><td>RW</td><td>接口地址,在使用10位地址时为第9-8位,在使用7位地址时忽略。</td><td>0</td></tr><tr><td>[7:1]</td><td>ADD[7:1]</td><td>RW</td><td>接口地址,第7-1位。</td><td>0</td></tr><tr><td>0</td><td>ADDO</td><td>RW</td><td>接口地址,使用10位地址时为第0位,在使用7位地址时忽略。</td><td>0</td></tr></table>

### 19.12.4 I2C 地址寄存器 2（I2Cx_OADDR2）（x=1/2）

偏移地址： $0 \times 0 0$

<table><tr><td>15</td><td>14</td><td>13</td><td>12</td><td>11</td><td>10</td><td>9</td><td>8</td><td>7</td><td>6</td><td>5</td><td>4</td><td>3</td><td>2</td><td>1</td><td>0</td></tr><tr><td colspan="8">Reserved</td><td colspan="7">ADD2[7:1]</td><td>ENDUAL</td></tr></table>

<table><tr><td>位</td><td>名称</td><td>访问</td><td>描述</td><td>复位值</td></tr><tr><td>[15:8]</td><td>Reserved</td><td>R0</td><td>保留。</td><td>0</td></tr><tr><td>[7:1]</td><td>ADD2[7:1]</td><td>RW</td><td>接口地址，双地址模式下地址的7-1位。</td><td>0</td></tr><tr><td>0</td><td>ENDUAL</td><td>RW</td><td>双地址模式使能位，置此位可以让ADD2也能被识别。</td><td>0</td></tr></table>

### 19.12.5 I2C 数据寄存器（I2Cx_DATAR）（x=1/2）

偏移地址： $0 \times 1 0$

<table><tr><td>15</td><td>14</td><td>13</td><td>12</td><td>11</td><td>10</td><td>9</td><td>8</td><td>7</td><td>6</td><td>5</td><td>4</td><td>3</td><td>2</td><td>1</td><td>0</td></tr><tr><td colspan="8">Reserved</td><td colspan="8">DR[7:0]</td></tr></table>

<table><tr><td>位</td><td>名称</td><td>访问</td><td>描述</td><td>复位值</td></tr><tr><td>[15:8]</td><td>Reserved</td><td>R0</td><td>保留。</td><td>0</td></tr><tr><td>[7:0]</td><td>DR[7:0]</td><td>RW</td><td>数据寄存器，该域用来存放接收到的数据或存放用于发送到总线的数据。</td><td>0</td></tr></table>

### 19.12.6 I2C 状态寄存器 1（I2Cx_STAR1）（x=1/2）

偏移地址： $0 \times 1 4$

<table><tr><td>15</td><td>14</td><td>13</td><td>12</td><td>11</td><td>10</td><td>9</td><td>8</td><td>7</td><td>6</td><td>5</td><td>4</td><td>3</td><td>2</td><td>1</td><td>0</td></tr><tr><td>SMBAL
ERT</td><td>TIME
OUT</td><td>Reserved</td><td>PECER
R</td><td>0VR</td><td>AF</td><td>ARLO</td><td>BERR</td><td>TxE</td><td>RxNE</td><td>Reser
ved</td><td>STOPF</td><td>ADD10</td><td>BTF</td><td>ADDR</td><td>SB</td></tr></table>

<table><tr><td>位</td><td>名称</td><td>访问</td><td>描述</td><td>复位值</td></tr><tr><td>15</td><td>SMBALERT</td><td>RWO</td><td>SMBus警示位,该位可以由用户写0复位,或在PE变低时由硬件复位。在SMBus主机模式下:1:在引脚上产生了SMBus警示;0:无SMBus警示。在SMBus从机模式下:1:收到SMBAlert响应地址头序列直到SMBAlert变低;0:没有收到SMBAlert响应地址头序列。</td><td>0</td></tr><tr><td>14</td><td>TIMEOUT</td><td>RWO</td><td>超时或Tlow错误标志位,该位可以由用户写0复位,或在PE变低时由硬件复位。1:SCL处于低已达到25mS,或主机低电平累计时钟扩招时间超过10mS,或从设备低电平累计时间超过25mS;0:无超时错误。注:在从模式下此位被置位,从设备会复位通讯,硬件会释放总线;在主模式下此位被置位,硬件会发出停止条件。</td><td>0</td></tr><tr><td>13</td><td>Reserved</td><td>R0</td><td>保留。</td><td>0</td></tr><tr><td>12</td><td>PECERR</td><td>RWO</td><td>在接收时发生PEC错误标志位,该位可以由用户写0复位,或在PE变低时由硬件复位。1:有PEC错误,接收到PEC后,返回NAK;0:无PEC错误。</td><td>0</td></tr><tr><td>11</td><td>OVR</td><td>RWO</td><td>过载、欠载标志位。1:有过载、欠载事件发生:当NOSTRETCH=1时,在接收模式中收到一个新的字节时,数据寄存器里的内容还未被读出,则新接收的字节将丢失;在发送模式时,没有新的数据写入数据寄存器,同样的字节将被发送两次;0:无过载、欠载事件。</td><td>0</td></tr><tr><td>10</td><td>AF</td><td>RWO</td><td>应答失败标志位,该位可以由用户写0复位,或在PE变低时由硬件复位。1:应答错误;0:应答正常。</td><td>0</td></tr><tr><td>9</td><td>ARLO</td><td>RWO</td><td>仲裁丢失标志位,该位可以由用户写0复位,或在PE变低时由硬件复位。1:检测到仲裁丢失,模块失去对总线的控制;0:仲裁正常。</td><td>0</td></tr><tr><td>8</td><td>BERR</td><td>RWO</td><td>总线出错标志位,该位可以由用户写0复位,或在PE变低时由硬件复位。1:起始或停止条件出错;0:正常。</td><td>0</td></tr><tr><td>7</td><td>TxE</td><td>R0</td><td>数据寄存器为空标志位,向数据寄存器写数据可以清除,或产生一个起始或停止位后,或当PE为0后,由硬件自动清除。</td><td>0</td></tr><tr><td></td><td></td><td></td><td>1:发送数据时,发送数据寄存器为空;0:数据寄存器非空。</td><td></td></tr><tr><td>6</td><td>RxNE</td><td>R0</td><td>数据寄存器非空标志位,对数据寄存器的读写操作将清除此位,或当PE为0后,由硬件清除此位。1:接收数据时,数据寄存器不为空;0:正常。</td><td>0</td></tr><tr><td>5</td><td>Reserved</td><td>R0</td><td>保留。</td><td>0</td></tr><tr><td>4</td><td>STOPF</td><td>R0</td><td>停止事件标志位,用户读取状态寄存器1之后,对控制寄存器1的写操作将会清除该位,或当PE为0后,由硬件清除此位。1:在应答之后,从设备在总线上检测到停止事件;0:没有检测到停止事件。</td><td>0</td></tr><tr><td>3</td><td>ADD10</td><td>R0</td><td>10位地址头序列发送标志位,用户读取状态寄存器1之后,对控制寄存器1的写操作将会清除该位,或当PE为0后,由硬件清除此位。1:在10位地址模式下,主设备已经将第一个地址字节发送出去;0:无。</td><td>0</td></tr><tr><td>2</td><td>BTF</td><td>R0</td><td>字节发送结束标志位,用户读取状态寄存器1后,对数据寄存器的读写将清除此位;在传输中,发起一个起始或者停止事件后,或当PE为0后,由硬件清除此位。1:字节发送结束。当NOSTRETCH=0时:发送时,当一个新数据被发送且数据寄存器还未被写入新数据;接收时,当接收一个新的字节但是数据寄存器还未被读取;0:无。</td><td>0</td></tr><tr><td>1</td><td>ADDR</td><td>RWO</td><td>地址被发送/地址匹配标志位,用户读取状态寄存器1后,对状态寄存器2的读操作将会清除此位,或当PE为0时,由硬件清除此位。主模式:1:地址发送结束:在10位地址模式下,当收到地址的第二个字节的ACK后改为被置位;在7位地址模式下,当收到地址的ACK后被置位;0:地址发送没有结束。从模式:1:收到的地址匹配;0:地址不匹配或没有收到地址。</td><td>0</td></tr><tr><td>0</td><td>SB</td><td>R0</td><td>起始位发送标志位,读取状态寄存器1后写数据寄存器的操作将清除该位,或当PE为0时,硬件将会清除此位。1:已发送起始位;0:未发送起始位。</td><td>0</td></tr></table>

### 19.12.7 I2C 状态寄存器 2（I2Cx_STAR2）（x=1/2）

偏移地址： $0 \times 1 8$

<table><tr><td>15</td><td>14</td><td>13</td><td>12</td><td>11</td><td>10</td><td>9</td><td>8</td><td>7</td><td>6</td><td>5</td><td>4</td><td>3</td><td>2</td><td>1</td><td>0</td></tr><tr><td colspan="8">PEC[7:0]</td><td>DUALF</td><td>SMBHO ST</td><td>SMBDE FAULT</td><td>GENCA LL</td><td>Reser ved</td><td>TRA</td><td>BUSY</td><td>MSL</td></tr></table>

<table><tr><td>位</td><td>名称</td><td>访问</td><td>描述</td><td>复位值</td></tr><tr><td>[15:8]</td><td>PEC[7:0]</td><td>R0</td><td>包错误检查域,当PEC使能时(ENPEC置位),此域存放PEC的值。</td><td>0</td></tr><tr><td>7</td><td>DUALF</td><td>R0</td><td>匹配检测标志位,在产生停止位或起始位时,或在PE=0时,硬件会将该位清零。1:接收到的地址与OAR2中的内容相符;0:接收到的地址与OAR1中的内容相符。</td><td>0</td></tr><tr><td>6</td><td>SMBHOST</td><td>R0</td><td>SMBus主机头标志位,在产生停止位或起始位时,或在PE=0时,硬件会将该位清零。1:当SMBTYPE=1且ENARP=1时,收到了SMBus主机地址;0:未接收到SMBus主机地址。</td><td>0</td></tr><tr><td>5</td><td>SMBDEFAULT</td><td>R0</td><td>SMBus设备默认地址标志位,在产生停止位或起始位时,或在PE=0时,硬件会将该位清零。1:当ENARP=1,收到SMBus设备的默认地址;0:未收到地址。</td><td>0</td></tr><tr><td>4</td><td>GENCALL</td><td>R0</td><td>广播呼叫地址标志位,在产生停止位或起始位时,或者在PE=0时,硬件会将该位清零。1:当ENG=1时,收到广播呼叫的地址;0:未收到广播呼叫地址。</td><td>0</td></tr><tr><td>3</td><td>Reserved</td><td>R0</td><td>保留。</td><td>0</td></tr><tr><td>2</td><td>TRA</td><td>R0</td><td>发送/接收标志位,在检测到停止事件(STOPF=1),重复的起始条件、总线仲裁丢失(ARLO=1)或PE=0时,硬件会将其清零。1:数据已发送;0:接收到数据。该位根据地址字节的R/W位来决定。</td><td>0</td></tr><tr><td>1</td><td>BUSY</td><td>R0</td><td>总线忙标志位,该位在检测到一个停止位时会被清零。在接口被禁用时(PE=0),该信息仍被更新。1:总线忙:SDA或SCL存在低电平;0:总线空闲无通讯。</td><td>0</td></tr><tr><td>0</td><td>MSL</td><td>R0</td><td>主从模式指示位,当接口处于主模式时(SB=1),硬件将该位置位;当总线检测到一个停止位,仲裁丢失时,或PE=0时,硬件会清除该位。</td><td>0</td></tr></table>

### 19.12.8 I2C 时钟寄存器（I2Cx_CKCFGR）（x=1/2）

偏移地址： ${ 0 } { \times 1 } { 0 }$

<table><tr><td>15</td><td>14</td><td>13</td><td>12</td><td>11</td><td>10</td><td>9</td><td>8</td><td>7</td><td>6</td><td>5</td><td>4</td><td>3</td><td>2</td><td>1</td><td>0</td></tr><tr><td>F/S</td><td>DUTY</td><td colspan="2">Reserved</td><td colspan="12">CCR[11:0]</td></tr></table>

<table><tr><td>位</td><td>名称</td><td>访问</td><td>描述</td><td>复位值</td></tr><tr><td>15</td><td>F/S</td><td>RW</td><td>主模式选择位。1: 快速模式;0: 标准模式。</td><td>0</td></tr><tr><td>14</td><td>DUTY</td><td>RW</td><td>快速模式时的高电平时间比上低电平时间的占空比。1: 36%; 0: 33.3%。</td><td>0</td></tr><tr><td>[13:12]</td><td>Reserved</td><td>RO</td><td>保留。</td><td>0</td></tr><tr><td>[11:0]</td><td>CCR[11:0]</td><td>RW</td><td>时钟分频系数域,决定SCL时钟的频率波形。</td><td>0</td></tr></table>

### 19.12.9 I2C 上升时间寄存器（I2Cx_RTR）（x=1/2）

偏移地址： $_ { 0 \times 2 0 }$

<table><tr><td>15</td><td>14</td><td>13</td><td>12</td><td>11</td><td>10</td><td>9</td><td>8</td><td>7</td><td>6</td><td>5</td><td>4</td><td>3</td><td>2</td><td>1</td><td>0</td></tr><tr><td colspan="10">Reserved</td><td colspan="6">TRISE[5:0]</td></tr></table>

<table><tr><td>位</td><td>名称</td><td>访问</td><td>描述</td><td>复位值</td></tr><tr><td>[15:6]</td><td>Reserved</td><td>R0</td><td>保留。</td><td>0</td></tr><tr><td>[5:0]</td><td>TRISE[5:0]</td><td>RW</td><td>最大上升时间域。这个位设置主模式的SCL的上升时间。最大的上升沿时间等于TRISE-1个时钟周期。此位只能在PE清零下设置。比如如果I2C模块的输入时钟周期为125nS,而TRISE的值为9,那么最大上升沿时间为(9-1)*125nS,即1000nS。</td><td>000010b</td></tr></table>

# 第 20 章 串行外设接口（SPI/I2S）

本章模块描述适用于CH32F2x、CH32V2x和CH32V3x微控制器全系列产品。

SPI 支持以三线同步串行模式进行数据交互，加上片选线支持硬件切换主从模式，支持以单根数据线通讯。

I2S 也是三线的同步串行接口通信协议，它支持四种音频标准，包括飞利浦 I2S 标准、MSB 对齐标准、LSB对齐标准和PCM标准。

## 20.1 主要特征

### 20.1.1 SPI 特征

支持全双工同步串行模式  
$\bullet$ 支持单线半双工模式  
$\bullet$ 支持主模式和从模式，多从模式  
$\bullet$ 支持 8 位或 16 位数据结构  
$\bullet$ 最高时钟频率支持到Fpclk的一半  
$\bullet$ 数据顺序支持 MSB 或 LSB 在前  
$\bullet$ 支持硬件或软件控制 NSS 引脚  
$\bullet$ 收发支持硬件CRC校验  
收发缓冲器支持 DMA 传输  
支持修改时钟相位和极性

### 20.1.2 I2S 特征

支持单工通信  
$\bullet$ 支持主模式和从模式  
$\bullet$ 支持 16位、24位和32位数据格式  
$\bullet$ 音频采样频率支持范围 8KHz-562.2KHz  
$\bullet$ 支持时钟极性可编程  
$\bullet$ 支持常用 I2S 协议：飞利浦标准、MSB 对齐标准、MSL 对齐标准和 PCM 标准  
$\bullet$ 收发缓冲器支持DMA传输  
支持主时钟向外部音频设备输出

## 20.2 SPI 功能描述

### 20.2.1 概述

![](images/476574ac4e3558365d2744980c8cb437730a822800b668853bd394334f17eb73.jpg)  
图 20-1 SPI 结构框图

由图 20-1可以看出，与 SPI相关的主要是MISO、MOSI、SCK和 NSS四个引脚。其中 MISO引脚在SPI 模块工作在主模式下时，是数据输入引脚；工作在从模式下时，是数据输出引脚。MOSI 引脚工作在主模式下时，是数据输出引脚；工作在从模式时，是数据输入引脚。SCK是时钟引脚，时钟信号一直由主机输出，从机接收时钟信号并同步数据收发。NSS引脚是片选引脚，有以下用法：

1） NSS由软件控制：此时SSM被置位，内部 NSS信号由 SSI决定输出高还是低，这种情况一般用于SPI 主模式；  
2） NSS 由硬件控制：在 NSS 输出使能时，即 SSOE 置位时，在 SPI 主机向外发送输出时会主动拉低NSS 引脚，如果拉低 NSS 脚，则会产生一个硬件错误；SSOE 不置位，则可以用于多主机模式，如果它被拉低则会强行进入从机模式，MSTR 位会被自动清除。

可以通过 CPHA 和 CPOL 配置 SPI 的工作模式。CPHA 置位表示模块在时钟的第二个边沿进行数据采样，数据被锁存，CPHA 不置位表示 SPI 模块在时钟的第一个边沿进行采样，数据被锁存。CPOL 则表示无数据时时钟保持高电平还是低电平。具体见下图 20-2。

![](images/df8fc581a1ae9a93f1cd71a8790b909e5bba462e1f4b0aee30f493ffbcc01aa0.jpg)  
图 20-2 SPI 模式

主机和设备需要设置为相同的SPI模式，在配置 SPI模式前，需要清除 SPE位。DEF位可以决定SP的单个数据长度是8位还是16位。LSBFIRST 可以控制单个数据字是高位在前还是低位在前。

### 20.2.2 主模式

在SPI模块工作在主模式时，由SCK产生串行时钟。配置成主模式进行以下步骤：

配置控制寄存器的BR[2:0]域来确定时钟；

配置 CPOL 和 CPHA 位来确定 SPI 模式；

配置DEF确定数据字长；

配置 LSBFIRST 确定帧格式；

配置 NSS 引脚，比如置 SSOE 位让硬件去置 NSS。也可以置 SSM 位并把 SSI 位置高；

置 MSTR 位和 SPE 位，需要保证 NSS 此时已经是高。

需要发送数据时只需要向数据寄存器写要发送的数据就行了。SPI会从发送缓冲区并行地把数据送到移位寄存器，然后按照 LSBFIRST 的设置将数据从移位寄存器发出去，当数据已经到了移位寄存器时，TXE 标志会被置位，如果已经置位了 TXEIE，那么会产生中断。如果 TXE 标志位置位需要向数据寄存器里填数据，维持完整的数据流。

当接收器接收数据时，当数据字的最后一个采样时钟沿到来时，数据从移位寄存器并行地转移到接收缓冲区，RXNE位被置位，如果之前置位了 RXNEIE 位，还会产生中断。此时应该尽快读取数据寄

存器取走数据。

### 20.2.3 从模式

当SPI模块工作在从模式时，SCK用于接收主机发来的时钟，自身的波特率设置无效。配置成从模式的步骤如下：

配置 DEF 位设置数据位长度；

配置 CPOL 和 CPHA 位匹配主机模式；

配置 LSBFIRST 匹配主机数据帧格式；

硬件管理模式下，NSS 管脚需要保持为低电平，如果设置 NSS 为软件管理（SSM 置位），那么请保持 SSI 不被置位；

清除 MSTR 位，置 SPE 位，开启 SPI 模式。

在发送时，当SCK出现第一个从机接收采样沿时，从机开始发送。发送的过程就是发送缓冲区的数据移到发送移位寄存器，当发送缓冲区的数据移到了移位寄存器之后，会置位 TXE 标志，如果之前置位了TXEIE位，那么会产生中断。

在接收时，最后一个时钟采样沿之后，RXNE 位被置位，移位寄存器接收到的字节被转移到接收缓冲区，读数据寄存器的读操作可以获得接收缓冲区里的数据。如果在 RXNE 置位之前 RXNEIE已经被置位，那么会产生中断。

### 20.2.4 单工模式

SPI 接口可以工作在半双工模式，即主设备使用 MOSI 引脚，从设备使用 MISO引脚进行通讯。使用半双工通讯时需要把 BIDIMODE 置位，使用 BIDIOE 控制传输方向。

在正常全双工模式下将RXONLY位置位可以将 SPI模块设置为仅仅接收的单工模式，在RXONLY置位之后会释放一个数据脚，主模式和从模式释放的引脚并不相同。也可以不理会接收的数据将 SPI 置成只发送的模式。

### 20.2.5 CRC

SPI 模块使用 CRC 校验保证全双工通信的可靠性，数据收发分别使用单独的 CRC 计算器。CRC 计算的多项式由多项式寄存器决定，对于8位数据宽度和 16位数据宽度，会分别使用不同的计算方法。

设置 CRCEN 位会启用 CRC 校验，同时会使 CRC 计算器复位。在发送完最后一个数据字节后，置CRCNEXT 位会在当前字节发送结束后发送 TXCRCR 计算器的计算结果，同时最后接收到的接收移位寄存器的值如果与本地算出来的RXCRCR的计算值不相符，那么 CRCERR位会被置位。使用 CRC校验需要在配置 SPI 工作模式时设置多项式计算器并置 CRCEN 位，并在最后一个字或半字置 CRCNEXT 位发送CRC 并进行接收 CRC 的校验。注意，收发双方的 CRC 计算多项式应该统一。

### 20.2.6 DMA

SPI 模块支持使用DMA加快数据通讯速度，可以使用 DMA向发送缓冲区填写数据，或使用 DMA从接收缓冲区及时取走数据。DMA会以RXNE和 TXE为信号及时取走或发来数据。DMA也可以工作在单工或加CRC校验的模式。

### 20.2.7 错误

#### 主模式失效错误

当SPI工作在NSS引脚硬件管理模式下，发生了外部拉低 NSS引脚的操作；或在 NSS引脚软件管理模式下，SSI 位被清零；或 SPE 位被清零，导致 SPI 被关闭；或 MSTR 位被清零，SPI 进入从模式。如果 ERRIE位已经被置位，还会产生中断。清除 MODF 位步骤：首先执行一次对 R16_SPI1_STATR的读或写操作，然后写 R16_SPI1_CTLR1。

#### 溢出错误

如果主机发送了数据，而从设备的接收缓冲区中还有未读取的数据，就会发生溢出错误，OVR位被置位，如果ERRIE被置位还会产生中断。发送溢出错误应该重新开始当前传输。读取数据寄存器再读取状态寄存器会消除此位。

#### CRC 错误

当接收到的CRC校验字和RXCRCR的值不匹配时，会产生 CRC校验错误，CRCERR 位会被置位。

### 20.2.8 中断

SPI 模块的中断支持五个中断源，其中发送缓冲区空、接收缓冲区非空这两个事件分别会置位 TXE和RXNE，在分别置位了TXEIE和RXNEIE位的情况下会产生中断。除此之外上面提到的三种错误也会产生中断，分别是MODF、OVR和CRCERR，在使能了ERRIE位之后，这三种错误也会产生错误中断。

## 20.3 I2S 功能描述

### 20.3.1 I2S 概述

![](images/d590c704cde633a24861e6be07e6e49b11dd1e3b8845e14356938200119e0d96.jpg)  
图 20-3 I2S 结构框图

通过将寄存器 I2SCFGR 的 I2SMOD 位置位，使能 I2S 功能。此时，可以把 SPI 模块用作 I2S 音频接口。I2S与SPI共用3个引脚：

● SD：串行数据(映射至 MOSI 引脚)，用来发送和接收 2 路时分复用通道的数据；  
$\bullet$ WS：字选(映射至NSS引脚)，主模式下作为数据控制信号输出，从模式下作为输入；  
● CK：串行时钟(映射至 SCK 引脚)，主模式下作为时钟信号输出，从模式下作为输入。

在某些外部音频设备需要主时钟时，可以另有一个附加引脚输出时钟：

● MCK：主时钟(独立映射)，在I2S配置为主模式，寄存器 I2SPR的 MCKOE 位为 1 时，作为输出额外的时钟信号引脚使用。输出时钟信号的频率预先设置为 $2 5 6 \times \mathsf { F s }$ ，其中 Fs 是音频信号的采样频率。

设置成主模式时，I2S使用自身的时钟发生器来产生通信用的时钟信号。这个时钟发生器也是主时钟输出的时钟源。I2S 模式下有 2 个额外的寄存器，一个是与时钟发生器配置相关的寄存器 I2SPR，另一个是I2S通用配置寄存器I2SCFGR(可设置音频标准、从/主模式、数据格式、数据包帧、时钟极性等参数)。在I2S模式下不使用寄存器CTLR1和所有的 CRCR寄存器。同样，I2S 模式下也不使用寄存器 CTLR2 的 SSOE 位，和寄存器 STATR 的 MODF 位和 CRCERR 位。I2S 使用与 SPI 相同的寄存器 DATAR用作16位宽模式数据传输。

### 20.3.2 支持的音频协议

三线总线支持 2 个声道上音频数据的时分复用：左声道和右声道，但是只有一个 16 位寄存器用作发送或接收。因此，软件必须在对数据寄存器写入数据时，根据当前传输中的声道写入相应的数据；同样，在读取寄存器数据时，通过检查寄存器 STATR的 CHSIDE位来判断接收到的数据属于哪个声道。左声道总是先于右声道发送数据(CHSIDE 位在 PCM 协议下无意义)。有四种可用的数据和包帧组合。可以通过以下四种数据格式发送数据：

● 16 位数据打包进 16 位帧  
$\bullet$ 16 位数据打包进 32 位帧  
● 24 位数据打包进 32 位帧  
● 32 位数据打包进 32 位帧

在使用 16 位数据扩展到 32 位帧时，前 16 位(MSB)是有意义的数据，后 16 位(LSB)被强制为 0，该操作不需要软件干预，也不需要有DMA请求(仅需要一次读或写操作)。24位和32 位数据帧需要 CPU对寄存器 DATAR 进行 2 次读或写操作，在使用 DMA 时，需要 2 次 DMA 传输。对于 24 位数据，扩展到32 位后，最低 8 位由硬件置 0。对于所有的数据格式和通讯标准，总是先发送最高位(MSB)。I2S 接口支持四种音频标准，可以通过设置寄存器I2SCFGR的 I2SSTD[1:0]位和PCMSYNC 位来选择。

#### 20.3.2.1 I2S 飞利浦标准

在此标准下，引脚WS用来指示正在发送的数据属于哪个声道。在发送第一位数据(MSB)前 1个时钟周期，该引脚即为有效。发送方在时钟信号(CK)的下降沿改变数据，接收方在上升沿读取数据。WS信号也在时钟信号的下降沿变化。

![](images/57bbf1b7a7dbf997edb7fd15113377ab11453da3585b30dea28d3c2c38d6d086.jpg)  
图 20-4 飞利浦协议波形（16/32 全精度，CPOL=0）

![](images/b9c486504ca2e1bd9c1d53d1382ca01ad1d3a753c87c8b7e0b7d900162b62f04.jpg)  
图 20-5 飞利浦协议波形（24 位帧，CPOL=0）

此模式需要对寄存器 SPI_DATAR 进行 2 次读或写操作。在发送模式下：如果需要发送 0x8EAA33(24位)：

![](images/b650626dff07f2f9086b5a7683ae2a954d5012ca7463e0f7aa7fc1d30d643967.jpg)

在接收模式下：如果接收 0x8EAA33：

![](images/0907232c400a5a8e80319d3849824b361a97715904069b810b807eef4be56eca.jpg)

![](images/3291b4a9f2317a4ecb856b91900210089747602f031290a553082984c0071b42.jpg)  
图 20-6 飞利浦协议标准波形（16 位扩展至 32 位包帧， $\mathsf { C P O L = 0 } ,$ ）

在I2S配置阶段，如果选择将16位数据扩展到 32位声道帧，只需要访问一次寄存器 DATAR用来扩展到 32 位的低 16 位被硬件置为 $0 \times 0 0 0 0$ 。如果待传输或接收的数据是 $0 \times 7 6 { \tt A } 3$ (扩展到 32 位是$0 \times 7 6 \mathsf { A } 3 0 0 0 0 )$ )，只需要操作一次 DATAR。在发送时需要将 MSB 写入寄存器 DATAR；标志位 TXE 为 1 表示可以写入新的数据，如果允许了相应的中断，则可以产生中断。发送是由硬件完成的，即使还未发送出后 16位的 $0 \times 0 0 0 0$ ，也会设置TXE并产生相应的中断。接收时，每次收到高 16位半字(MSB)后，标志位RXNE置1，如果允许了相应的中断，则可以产生中断。这样，在 2次读和写之间有更多的时间，可以防止下溢或上溢的情况发生。

#### 20.3.2.2 MSB 对齐标准

在此标准下，WS 信号和第一个数据位，即最高位(MSB)同时产生。

![](images/c6a11620da4cc7e0732c16722eaddb88f4d9e40d503163d1716c1aa54637f836.jpg)  
图 20-7 MSB 对齐 16 位或 32 位全精度（ $\mathsf { C P O L } = \mathsf { 0 } ;$ ）

发送方在时钟信号的下降沿改变数据；接收方是在上升沿读取数据。

![](images/cd78a0f2a4e6da0bb4628d74ac563e4009e1b70f35285c823f6a892c2203a249.jpg)  
图 20-8 MSB 对齐 24 位数据， $\mathsf { C P O L } = 0$

![](images/fa3890327993f71de7d5d1c9f103f3840763d71d16b37ae3163f0230e1040935.jpg)  
图 20-9 MSB 对齐 16 位数据扩展到 32 位包帧，CPOL = 0

#### 20.3.2.3 LSB 对齐标准

此标准与 MSB 对齐标准类似(在 16 位或 32 位全精度帧格式下无区别)。

![](images/680c6b81334c1d37fd6bad86a23373b087096a835a8d6437bc75afcc7a1295c3.jpg)  
图 20-10 LSB 对齐 16 或 32 位全精度， $\mathsf { C P O L } = 0$

![](images/9eda589a32cf7c9506b87bf07918ec874149927d8310cc6489e6bd6badc26142.jpg)  
图 20-11 LSB 对齐 24 位数据， $\mathsf { C P O L } = 0$

在发送模式下如果要发送数据0x3478AE，需要通过软件或 DMA对寄存器 DATAR进行 2次写操作。

![](images/13d341ef011df9c3bef58bae99f1022a2a74406fbfc2892589f911c6cfd4f64b.jpg)

在接收模式下如果要接收数据 0x3478AE，需要在 2 个连续的 RXNE 事件发生时，分别对寄存器DATAR 进行 1 次读操作。

![](images/a1b18618f68bac0bcc4efb8a12bd1dfca026f6e09339d4546a23fc8646b3c56a.jpg)

![](images/767298636c7d3877449a1f533220af109fe60bb1446b3c953f86d88b02dce1e5.jpg)  
图 20-12 LSB 对齐 16 位数据扩展到 32 位包帧， CPOL = 0

在I2S配置阶段，如果选择将16位数据扩展到32 声道帧，只需要访问一次寄存器 DATAR。此时，扩展到 32 位后的高半字(16 位 MSB)被硬件置为 $0 \times 0 0 0 0$ 。

如果待传输或接收的数据是 $0 \times 7 6 { \tt A } 3$ (扩展到 32位是 $0 \times 0 0 0 0 7 6 \mathsf { A } 3 )$ ，只需要操作一次 DATAR，在发送时，如果 TXE 为 1，用户需要写入待发送的数据(即 $0 \times 7 6 A 3 )$ )。用来扩展到 32 位的 $0 \times 0 0 0 0$ 部分由硬件首先发送出去，一旦有效数据开始从 SD 引脚送出，即发生下一次 TXE 事件。在接收时，一旦接收

到有效数据(而不是 $0 \times 0 0 0 0$ 部分)，即发生 RXNE 事件。这样，在 2 次读和写之间有更多的时间，可以防止下溢或上溢的情况发生。

#### 20.3.2.4 PCM 标准

在 PCM 标准下，不存在声道选择的信息。PCM 标准有 2 种可用的帧结构，短帧或长帧，可以通过设置寄存器 I2SCFGR 的 PCMSYNC 位来选择。

![](images/13e6f0df173a33bf48e51a1a3c741adace721da959d8e5b0f33037e4953a5030.jpg)  
图 20-13 PCM 标准波形（16 位）

对于长帧，主模式下，用来同步的 WS 信号有效的时间固定为 13 位。对于短帧，用来同步的 WS信号长度只有 1 位。

![](images/873020fbb2da7a5b172b5fcb9fffe5cb9c70a4c9bdc96c7680333e59629fa922.jpg)  
图 20-14 PCM 标准波形（16 位）

无论哪种模式(主或从)、哪种同步方式(短帧或长帧)，连续的 2 帧数据之间和 2 个同步信号之间的时间差，(即使是从模式)需要通过设置I2SCFGR寄存器的 DATLEN位和CHLEN位来确定。

### 20.3.3 时钟发生器

I2S 的比特率即确定了在I2S数据线上的数据流和 I2S的时钟信号频率。I2S比特率 $=$ 每个声道的比特数 $\times$ 声道数目 $\times$ 音频采样频率。

对于一个具有左右声道和 16 位音频信号，I2S 比特率计算如下：

I2S 比特率 $= 1 6 \times 2 \times F s$ s

如果包长为 32 位，I2S 比特率计算如下：

I2S 比特率 $= 3 2 \times 2 \times { \mathsf { F } } { \mathsf { s } }$

![](images/d8fb7d1e72f91add5d7a5ced0ced10a66acd5504d0fa7e2773145be56b3d3c4f.jpg)  
图20-15 音频采样频率定义

在主模式下，为了获得需要的音频频率，需要正确地对线性分频器进行设置。

![](images/268676fdb3e8076bbb092cf2a3e6e21da984b93754f2c7c4932e9c30e5f39749.jpg)  
图 20-16 I2S 时钟发生器结构

图中 $1 2 \mathsf { S x C L K }$ 的时钟源是系统时钟(即驱动 AHB时钟的 HSI、HSE或PLL)。 $1 2 \mathsf { S x C L K }$ 可以来自SYSCLK，或 PLL3 VCO(2xPLL3CLK)时钟，可以通过 RCC_CFGR2 寄存器的 I2S2SRC 和 I2S3SRC 位选择。音频的采样频率可以是 96KHz、48KHz、44.1KHz、32KHz、22.05KHz、16KHz、11.025KHz 或8KHz(或任何此范围内的数值)。为了获得需要的频率，需按照以下公式设置线性分频器：

当需要生成主时钟时(寄存器 SPI_I2SPR 的 MCKOE 位为 1)：

声道的帧长为 16 位时， $\mathsf { F } _ { 5 } = \mathsf { 1 2 S } \times \mathsf { G L K } / \left[ \left( 1 6 ^ { * } 2 \right) \ * \ \left( \left( 2 ^ { * } \mathsf { I } 2 \mathsf { S D } \mathsf { I } \mathsf { V } \right) \ + \ \mathsf { 0 } \mathsf { D } \mathsf { D } \right) { * } 8 \right]$

声道的帧长为 32 位时， Fs = I2SxCLK / [(32*2) * ((2*I2SDIV) + ODD)*4]

当关闭主时钟时(MCKOE 位为’ $0 ^ { \prime }$ ’)：

声道的帧长为 16 位时， $\mathsf { F } \mathsf { s } = \mathsf { 1 2 S x C L K } / \left[ \left( 1 6 * 2 \right) * \left( \left( 2 * | 2 \mathsf { S } \mathsf { D } | \mathsf { V } \right) + \mathsf { 0 } \mathsf { D } \mathsf { D } \right) \right]$

声道的帧长为 32 位时， Fs = I2SxCLK / [(32*2) * ((2*I2SDIV) + ODD)]

### 20.3.4 I2S 主模式

设置 I2S 工作在主模式，串行时钟由引脚 CK 输出，字选信号由引脚 WS 产生。可以通过设置寄存器I2SPR的MCKOE位来选择输出或不输出主时钟(MCK)。

#### 20.3.4.1 配置流程

l 设置寄存器I2SPR的I2SDIV[7:0]定义与音频采样频率相符的串行时钟波特率。同时也要定义寄存器 I2SPR 的 ODD 位。  
设置CKPOL位定义通信时钟在空闲时的电平状态。如果需要向外部的 DAC/ADC音频器件提供主

时钟 MCK，需寄存器 I2SPR 的 MCKOE 位置为。

l 设置寄存器 I2SCFGR 的 I2SMOD 位为 1 激活 I2S 功能，设置 I2SSTD[1:0]和 PCMSYNC 位选择所用的 I2S 标准，设置 CHLEN 选择每个声道的数据位数。还要设置寄存器 SPI_I2SCFGR 的 I2SCFG[1:0]选择 I2S主模式和方向(发送端还是接收端)。  
如果需要，可以通过设置寄存器 CR2 来打开所需的中断功能和 DMA 功能。  
$\bullet$ 必须将寄存器 I2SCFGR 的 I2SE 位置为 1。  
$\bullet$ 引脚 WS 和 CK 需要配置为输出模式。如果寄存器 SPI_I2SPR 的 MCKOE 位为 1，引脚 MCK 也要配置成输出模式。

#### 20.3.4.2 发送流程

当写入1个半字(16位)的数据至发送缓存，发送流程开始。假设第一个写入发送缓存的数据对应的是左声道数据。当数据从发送缓存移到移位寄存器时，标志位 TXE置 1，这时，要把对应右声道的数据写入发送缓存。标志位 CHSIDE 提示了目前待传输的数据对应哪个声道。标志位 CHSIDE 的值在TXE为1时更新，因此它在TXE为1时有意义。在先左声道后右声道的数据都传输完成后，才能被认为是一个完整的数据帧。不可以只传输部分数据帧，如仅有左声道的数据。

当发出第一位数据的同时，半字数据被并行地传送至 16位移位寄存器，然后后面的位依次按高位在先的顺序从引脚MOSI/SD发出。每次数据从发送缓存移至移位寄存器时，标志位 TXE置为 1，如果寄存器CR2的TXEIE位为1，则产生中断。

为了保证连续的音频数据传输，建议在当前传输完成之前，对寄存器 DATAR写入下一个要传输的数据。建议在要关闭 I2S 功能时，等待标志位 TXE=1 及 ${ \tt B S Y } { = } 0$ ，再将 I2SE 位清’0’。

#### 20.3.4.3 接收流程

接收流程的配置步骤除了第3点外，与发送流程的一致(参见前述的”发送流程”)，需要通过配置I2SCFG[1:0]来选择主接收模式。无论何种数据和声道长度，音频数据总是以 16位包的形式接收。即每次填满接收缓存后，标志位RXNE置1，如果寄存器 CR2的 RXNEIE位为 1，则产生中断。根据配置的数据和声道长度，收到左声道或右声道的数据会需要 1 次或 2次把数据传送到接收缓存的过程。对寄存器DATAR进行读操作即可清除 RXNE标志位。每次接收以后即更新 CHSIDE。它的值取决于I2S单元产生的WS信号。如果前一个接收到的数据还没有被读取，又接收到新数据，即发生上溢，标志位OVR被置为1，如果寄存器CR2的 ERRIE位为1，则产生中断，表示发生了错误。若要关闭I2S功能，需要执行特别的操作，以保证 I2S模块可以正常地完成传输周期而不会开始新的数据传输。操作过程与数据配置和通道长度、以及音频协议的模式相关：

● 16 位数据扩展到 32 位通道长度(DATLEN=00 并且 CHLEN=1)，使用 LSB(低位)对齐模式( $\angle 2 \mathsf { S S } \mathsf { T D } = 1 0$ ) a) 等待倒数第二个 $( n - 1 ) \mathsf { R } \mathsf { X N E } { = } 1$ ；

b) 等待 17 个 I2S 时钟周期(使用软件延迟)；  
c) 关闭 I2S( $I 2 S E = 0$ )。

$\bullet$ 16 位数据扩展到 32 位通道长度(DATLEN=00 并且 CHLEN=1)，使用 MSB(高位)对齐、I2S 或 PCM 模式(分别为 $1 2 5 5 \mathsf { T D } { = } 0 0$ 、 $\lvert 2 \mathsf { S S } \mathsf { T D } { = } 0 \rvert$ 或 $1 2 \thinspace \mathrm { S S } \thinspace \mathsf { T D } { = } 1 \thinspace 1$ )

a) 等待最后一个 ${ \sf R X N E } = 1$ ；  
b) 等待 1 个 I2S 时钟周期(使用软件延迟)；  
c) 关闭 I2S( $I 2 S E = 0$ )。

$\bullet$ 所有其它DATLEN和CHLEN的组合，I2SSTD选择的任意音频模式，使用下述方式关闭 I2S：

a) 等待倒数第二个 $( n - 1 ) \mathsf { R } \mathsf { X N E } { = } 1$ ；  
b) 等待一个 I2S 时钟周期(使用软件延迟)；  
c) 关闭 I2S( $I 2 S E = 0$ )。

注： 在传输期间BSY标志始终为低。

### 20.3.5 I2S 从模式

在从模式下，I2S可以设置成发送和接收模式。从模式的配置方式基本遵循和配置主模式一样的流程。在从模式下，不需要I2S接口提供时钟。时钟信号和 WS信号都由外部主 I2S设备提供，连接到相应的引脚上。因此用户无需配置时钟。

配置步骤如下：

l 设置寄存器 I2SCFGR 的 I2SMOD 位激活 I2S 功能；设置 I2SSTD[1:0]来选择所用的 I2S 标准；设置DATLEN[1:0]选择数据的比特数；设置 CHLEN 选择每个声道的数据位数。设置寄存器 I2SCFGR的I2SCFG[1:0]选择I2S从模式的数据方向(发送端还是接收端)。  
根据需要，设置寄存器 CR2 打开所需的中断功能和 DMA 功能。  
必须设置寄存器 I2SCFGR 的 I2SE 位为 1。

#### 20.3.5.1 发送流程

当外部主设备发送时钟信号，并且当 NSS_WS信号请求传输数据时，发送流程开始。必须先使能从设备，并且写入I2S数据寄存器之后，外部主设备才能开始通信。对于 I2S的 MSB对齐和LSB对齐模式，第一个写入数据寄存器的数据项对应左声道的数据。当开始通信时，数据从发送缓冲器传送到移位寄存器，然后标志位TXE置为1；这时，要把对应右声道的数据项写入 I2S数据寄存器。标志位 CHSIDE 提示了目前待传输的数据对应哪个声道。与主模式的发送流程相比，在从模式中，CHSIDE取决于来自外部主I2S的WS信号。这意味着从 I2S在接收到主机生成的时钟信号之前，就要准备好第一个要发送的数据。 WS信号为1表示先发送左声道。

#### 20.3.5.2 接收流程

配置步骤除了第1点外，与发送流程一致。需要通过配置 I2SCFG[1:0]来选择主接收模式。无论何种数据和声道长度，音频数据总是以 16位包的形式接收，即每次填满接收缓存，标志位 RXNE置1，如果寄存器I2S_CTLR2的RXNEIE位为 1，则产生中断。按照不同的数据和声道长度设置，收到左声道或右声道数据会需要1次或2次传输数据至接收缓冲器的过程。每次接收到数据(将要从DATAR读出)以后即更新CHSIDE，它对应I2S 单元产生的 WS信号。读取SPI_DATAR寄存器，将清除RXNE位。在还没有读出前一个接收到的数据，又接收到新数据时，即产生上溢，并设置标志位 OVR为1；如果寄存器I2S_CTLR2的ERRIE位为1，则产生中断，指示发生了错误。要关闭 I2S功能时，需要在接收到最后一次RXNE=1时将I2SE位清0。

### 20.3.6 状态标志位

有 3 个状态标志位供用户监控 I2S 总线的状态。

#### 20.3.6.1 状态标志位（BSY）

BSY 标志由硬件设置与清除(写入此位无效果)，该标志位指示 I2S通信层的状态。该位为 1时表明 I2S 通讯正在进行中，但有一个例外：主接收模式 $( 1 2 \mathsf { S C F G } = 1 1 )$ )下，在接收期间 BSY 标志始终为低。在软件要关闭SPI模块之前，可以使用 BSY标志检测传输是否结束，这样可以避免破坏最后一次传输，因此需要严格按照下述过程执行。当传输开始时， BSY 标志被置为 1，除非 I2S 模块处于主接收模式。当传输结束时或者当关闭I2S模块时，该标志位被清除。当通信是连续的时候在主发送模式时，整个传输期间， BSY 标志始终为高；在从模式时，每个数据项传输之间， BSY 标志在 1个 I2S 时钟周期内变低。

#### 20.3.6.2 发送缓冲标志位（TXE）

该标志位为1表示发送缓冲器为空，可以对发送缓冲器写入新的待发送数据。在发送缓冲器中已有数据时，标志位清0。在I2S被关闭时(I2SE位为 0)，该标志位也为 0。

#### 20.3.6.3 接收缓存非空标志位（RXNE）

该标志位置1表示在接收缓存里有接收到的有效数据。在读取寄存器 DATAR时，该位清 0。

#### 20.3.6.4 声道标志位（CHSIDE）

在发送模式下，该标志位在TXE为高时刷新，指示从 SD引脚上发送的数据所在的声道。如果在从发送模式下发生了下溢错误，该标志位的值无效，在重新开始通讯前需要把 I2S关闭再打开。在接收模式下，该标志位在寄存器 DATAR 接收到数据时刷新，指示接收到的数据所在的声道。注意，如果发生错误，该标志位无意义，需要将 I2S 关闭再打开。在 PCM 标准下，无论短帧格式还是长帧格式，这个标志位都没有意义。如果寄存器 STATR的标志位 OVR或 UDR为 1，且寄存器 CR2的 ERRIE位为1，则会产生中断。(中断源已经被清除后)可以通过读寄存器 STATR来清除中断标志。

### 20.3.7 错误标志位

#### 20.3.7.1 下溢标志位（UDR）

在从发送模式下，如果数据传输的第一个时钟边沿到达时，新的数据仍然没有写入 DATAR 寄存器，该标志位会被置 1。在寄存器 I2SCFGR 的 I2SMOD 位置 1 后，该标志位才有效。如果寄存器 CR2的ERRIE位为1，就会产生中断。通过对寄存器 STATR进行读操作来清除该标志位。

#### 20.3.7.2 上溢标志位（OVR）

如果还没有读出前一个接收到的数据时，又接收到新的数据，即产生上溢，该标志位置 1，如果寄存器 CTLR2 的 ERRIE 位为 1，则产生中断指示发生了错误。这时，接收缓存的内容，不会刷新为从发送设备送来的新数据。对寄存器DATAR的读操作返回最后一个正确接收到的数据。其他所有在上溢发生后由发送设备发出的16位数据都会丢失。通过先读寄存器 DATAR再读寄存器 STATR，来清除该标志位。

### 20.3.8 I2S 中断

I2S 有4个中断源，其中发送缓冲区空，接收缓冲区非空这两个事件分别会置位 TXE和 RXNE，在分别置位了TXEIE和RXNEIE位的情况下会产生中断。如果还没有读出前一个接收到的数据时，又接收到新的数据，即产生上溢，如果置位了 ERRIE会产生上溢中断；在从发送模式下，如果数据传输的第一个时钟边沿到达时，新的数据仍然没有写入 DATAR 寄存器如果置位了 ERRIE 会产生下溢中断。

### 20.3.9 DMA 功能

DMA 的工作方式在 I2S 模式除了 CRC 功能不可用以外，与在 SPI 模式完全相同。因为在 I2S 模式下没有数据传输保护系统。

## 20.4 寄存器描述

表 20-1 SPI1 相关寄存器列表  

<table><tr><td>名称</td><td>访问地址</td><td>描述</td><td>复位值</td></tr><tr><td>R16_SPI1_CTLR1</td><td>0x40013000</td><td>SPI1 控制寄存器 1</td><td>0x0000</td></tr><tr><td>R16_SPI1_CTLR2</td><td>0x40013004</td><td>SPI1 控制寄存器 2</td><td>0x0000</td></tr><tr><td>R16_SPI1_STATR</td><td>0x40013008</td><td>SPI1 状态寄存器</td><td>0x0002</td></tr><tr><td>R16_SPI1_DATAR</td><td>0x4001300C</td><td>SPI1 数据寄存器</td><td>0x0000</td></tr><tr><td>R16_SPI1_CRCR</td><td>0x40013010</td><td>SPI1 多项式寄存器</td><td>0x0007</td></tr><tr><td>R16_SPI1_RCRCR</td><td>0x40013014</td><td>SPI1 接收 CRC 寄存器</td><td>0x0000</td></tr><tr><td>R16_SPI1_TCRCR</td><td>0x40013018</td><td>SPI1 发送 CRC 寄存器</td><td>0x0000</td></tr><tr><td>R16_SPI1_I2S_CFGR</td><td>0x4001301C</td><td>SPI1_I2S配置寄存器</td><td>0x0000</td></tr><tr><td>R16_SPI1_HSCR</td><td>0x40013024</td><td>SPI1高速控制寄存器</td><td>0x0000</td></tr></table>

表 20-2 SPI2 相关寄存器列表  

<table><tr><td>名称</td><td>访问地址</td><td>描述</td><td>复位值</td></tr><tr><td>R16_SPI2_CTLR1</td><td>0x40003800</td><td>SPI2 控制寄存器 1</td><td>0x0000</td></tr><tr><td>R16_SPI2_CTLR2</td><td>0x40003804</td><td>SPI2 控制寄存器 2</td><td>0x0000</td></tr><tr><td>R16_SPI2_STATR</td><td>0x40003808</td><td>SPI2 状态寄存器</td><td>0x0002</td></tr><tr><td>R16_SPI2_DATAR</td><td>0x4000380C</td><td>SPI2 数据寄存器</td><td>0x0000</td></tr><tr><td>R16_SPI2_CRCR</td><td>0x40003810</td><td>SPI2 多项式寄存器</td><td>0x0007</td></tr><tr><td>R16_SPI2_RCRCR</td><td>0x40003814</td><td>SPI2 接收 CRC 寄存器</td><td>0x0000</td></tr><tr><td>R16_SPI2_TCRCR</td><td>0x40003818</td><td>SPI2 发送 CRC 寄存器</td><td>0x0000</td></tr><tr><td>R16_SPI2_I2S_CFGR</td><td>0x4000381C</td><td>SPI2_I2S 配置寄存器</td><td>0x0000</td></tr><tr><td>R16_SPI2_I2SPR</td><td>0x40003820</td><td>SPI2_I2S 预分频寄存器</td><td>0x0000</td></tr><tr><td>R16_SPI2_HSCR</td><td>0x40003824</td><td>SPI2 高速控制寄存器</td><td>0x0000</td></tr></table>

表 20-3 SPI3 相关寄存器列表  

<table><tr><td>名称</td><td>访问地址</td><td>描述</td><td>复位值</td></tr><tr><td>R16_SPI3_CTLR1</td><td>0x40003C00</td><td>SPI3 控制寄存器 1</td><td>0x0000</td></tr><tr><td>R16_SPI3_CTLR2</td><td>0x40003C04</td><td>SPI3 控制寄存器 2</td><td>0x0000</td></tr><tr><td>R16_SPI3_STATR</td><td>0x40003C08</td><td>SPI3 状态寄存器</td><td>0x0002</td></tr><tr><td>R16_SPI3_DATAR</td><td>0x40003C0C</td><td>SPI3 数据寄存器</td><td>0x0000</td></tr><tr><td>R16_SPI3_CRCR</td><td>0x40003C10</td><td>SPI3 多项式寄存器</td><td>0x0007</td></tr><tr><td>R16_SPI3_RCRCR</td><td>0x40003C14</td><td>SPI3 接收 CRC 寄存器</td><td>0x0000</td></tr><tr><td>R16_SPI3_TCRCR</td><td>0x40003C18</td><td>SPI3 发送 CRC 寄存器</td><td>0x0000</td></tr><tr><td>R16_SPI3_I2S_CFGR</td><td>0x40003C1C</td><td>SPI3_I2S 配置寄存器</td><td>0x0000</td></tr><tr><td>R16_SPI3_I2SPR</td><td>0x40003C20</td><td>SPI3_I2S 预分频寄存器</td><td>0x0000</td></tr><tr><td>R16_SPI3_HSCR</td><td>0x40003C24</td><td>SPI3 高速控制寄存器</td><td>0x0000</td></tr></table>

### 20.4.1 SPI 控制寄存器 1（SPIx_CTLR1）（x=1/2/3）

偏移地址： $0 \times 0 0$

<table><tr><td>15</td><td>14</td><td>13</td><td>12</td><td>11</td><td>10</td><td>9</td><td>8</td><td>7</td><td>6</td><td>5</td><td>4</td><td>3</td><td>2</td><td>1</td><td>0</td></tr><tr><td>BIDI
MODE</td><td>BIDI
OE</td><td>CRCEN</td><td>CRC
NEXT</td><td>DFF</td><td>RX
ONLY</td><td>SSM</td><td>SSI</td><td>LSB
FIRST</td><td>SPE</td><td colspan="3">BR[2:0]</td><td>MSTR</td><td>CPOL</td><td>CPHA</td></tr></table>

<table><tr><td>位</td><td>名称</td><td>访问</td><td>描述</td><td>复位值</td></tr><tr><td>15</td><td>BIDIMODE</td><td>RW</td><td>单向数据模式使能位。1:选择单线双向模式;0:选择双线双向模式。</td><td>0</td></tr><tr><td>14</td><td>BIDIOE</td><td>RW</td><td>单线输出使能位,和BIDIMODE配合使用。1:使能输出,仅发送;0:禁止输出,仅接收。</td><td>0</td></tr><tr><td>13</td><td>CRCEN</td><td>RW</td><td>硬件CRC校验使能位,该位只能在SPE为0时写入,该位只能在全双工模式下使用。</td><td>0</td></tr><tr><td></td><td></td><td></td><td>1:启动CRC计算;0:禁止CRC计算。</td><td></td></tr><tr><td>12</td><td>CRCNEXT</td><td>RW</td><td>在接下来的一次数据传输后,发送CRC寄存器的值。这位应该在向数据寄存器写入最后一个数据后立刻置位。1:发送CRC校验结果;0:继续发送数据寄存器的数据。</td><td>0</td></tr><tr><td>11</td><td>DFF</td><td>RW</td><td>数据帧长度位,此位只能在SPE为0时写入。1:使用16位数据长度进行收发;0:使用8位数据长度进行收发。</td><td>0</td></tr><tr><td>10</td><td>RXONLY</td><td>RW</td><td>双线模式下只接收位,该位和BIDIMODE配合使用。置此位可以让设备只接收不发送。1:只接收,单工模式;0:全双工模式。</td><td>0</td></tr><tr><td>9</td><td>SSM</td><td>RW</td><td>片选引脚管理位,此位决定NSS引脚的电平由硬件还是软件控制。1:软件控制NSS引脚;0:硬件控制NSS引脚。</td><td>0</td></tr><tr><td>8</td><td>SSI</td><td>RW</td><td>片选引脚控制位,在SSM置位的情况下,此位决定NSS引脚的电平。1:NSS为高电平;0:NSS为低电平。</td><td>0</td></tr><tr><td>7</td><td>LSBFIRST</td><td>RW</td><td>帧格式控制位。不可以在通讯时修改此位。1:先发送LSB;0:先发送MSB。</td><td>0</td></tr><tr><td>6</td><td>SPE</td><td>RW</td><td>SPI使能位。1:启用SPI;0:禁用SPI。</td><td>0</td></tr><tr><td>[5:3]</td><td>BR[2:0]</td><td>RW</td><td>波特率设置域,在通讯时不可以修改此域。000:FPCLK/2;001:FPCLK/4;010:FPCLK/8;011:FPCLK/16;100:FPCLK/32;101:FPCLK/64;110:FPCLK/128;111:FPCLK/256。</td><td>000b</td></tr><tr><td>2</td><td>MSTR</td><td>RW</td><td>主从设置位,在通讯时不可以修改此位。1:配置为主设备;0:配置为从设备。</td><td>0</td></tr><tr><td>1</td><td>CPOL</td><td>RW</td><td>时钟极性选择位,在通讯时不可以修改此位。1:空闲状态时,SCK保持高电平;0:空闲状态时,SCK保持低电平。</td><td>0</td></tr><tr><td>0</td><td>CPHA</td><td>RW</td><td>时钟相位设置位,在通讯时不可以修改此位。1:数据采样从第二个时钟沿开始;0:数据采样从第一个时钟沿开始。</td><td>0</td></tr></table>

### 20.4.2 SPI 控制寄存器 2（SPIx_CTLR2）（x=1/2/3）

偏移地址： $0 \times 0 4$

<table><tr><td>Reserved</td><td>TXEIE</td><td>RXNEIE</td><td>ERRIE</td><td>Reserved</td><td>SSOE</td><td>TXDMAEN</td><td>RXDMAEN</td></tr></table>

控制寄存器 2  

<table><tr><td>位</td><td>名称</td><td>访问</td><td>描述</td><td>复位值</td></tr><tr><td>[15:8]</td><td>Reserved</td><td>R0</td><td>保留。</td><td>0</td></tr><tr><td>7</td><td>TXEIE</td><td>RW</td><td>发送缓冲区空中断使能位。置此位允许 TXE 被置位时产生中断。</td><td>0</td></tr><tr><td>6</td><td>RXNEIE</td><td>RW</td><td>接收缓冲区非空中断使能位。置此位允许 RXNE 被置位时产生中断。</td><td>0</td></tr><tr><td>5</td><td>ERRIE</td><td>RW</td><td>错误中断使能位。置此位允许在产生错误 (CRCERR, OVR, MODF)时产生中断。</td><td>0</td></tr><tr><td>[4:3]</td><td>Reserved</td><td>R0</td><td>保留。</td><td>0</td></tr><tr><td>2</td><td>SSOE</td><td>RW</td><td>SS 输出使能。禁止 SS 输出可以工作在多主模式下。1: 使能 SS 输出;0: 禁止主模式下的 SS 输出。</td><td>0</td></tr><tr><td>1</td><td>TXDMAEN</td><td>RW</td><td>发送缓冲区 DMA 使能位。1: 启用发送缓冲区 DMA;0: 禁用发送缓冲区 DMA。</td><td>0</td></tr><tr><td>0</td><td>RXDMAEN</td><td>RW</td><td>接收缓冲区 DMA 使能位。1: 启用接收缓冲区 DMA;0: 禁用接收缓冲区 DMA。</td><td>0</td></tr></table>

### 20.4.3 SPI 状态寄存器（SPIx_STATR）（x=1/2/3）

偏移地址： $0 \times 0 8$

<table><tr><td>Reserved</td><td>BSY</td><td>OVR</td><td>MODF</td><td>CRC
ERR</td><td>UDR</td><td>CHS ID
E</td><td>TXE</td><td>RXNE</td></tr></table>

<table><tr><td>位</td><td>名称</td><td>访问</td><td>描述</td><td>复位值</td></tr><tr><td>[15:8]</td><td>Reserved</td><td>R0</td><td>保留。</td><td>0</td></tr><tr><td>7</td><td>BSY</td><td>R0</td><td>忙标志位,该位由硬件置位或复位。1:SPI正在通讯,或发送缓冲区非空;0:SPI不在通讯。</td><td>0</td></tr><tr><td>6</td><td>OVR</td><td>R0</td><td>溢出标志位,该位由硬件置位,软件复位。1:出现溢出错误;0:没有出现溢出错误。</td><td>0</td></tr><tr><td>5</td><td>MODF</td><td>R0</td><td>模式错误标志位,该位由硬件置位,软件复位。1:出现了模式错误;0:没有出现模式错误。</td><td>0</td></tr><tr><td>4</td><td>CRCERR</td><td>RWO</td><td>CRC错误标志位,该位由硬件置位,软件复位。1:收到的CRC值与RCRCR的值不一致;0:收到的CRC值与RCRCR的值一致。</td><td>0</td></tr><tr><td>3</td><td>UDR</td><td>RO</td><td>下溢标志位,该位由硬件置位,软件复位。1:发生下溢;0:未发生下溢。</td><td>0</td></tr><tr><td>2</td><td>CHSIDE</td><td>RO</td><td>声道,该位由硬件置位,软件复位。1:需要传输或接收左声道;0:需要传输或接收右声道。</td><td>0</td></tr><tr><td>1</td><td>TXE</td><td>RO</td><td>发送缓冲区为空标志位。1:发送缓冲区为空;0:发送缓冲区非空。</td><td>1</td></tr><tr><td>0</td><td>RXNE</td><td>RO</td><td>接收缓冲区非空标志位。1:接收缓冲区非空;0:接收缓冲区为空。注:读DATAR,自动清零。</td><td>0</td></tr></table>

### 20.4.4 SPI 数据寄存器（SPIx_DATAR）（x=1/2/3）

偏移地址： $0 \times 0 0$

<table><tr><td>15</td><td>14</td><td>13</td><td>12</td><td>11</td><td>10</td><td>9</td><td>8</td><td>7</td><td>6</td><td>5</td><td>4</td><td>3</td><td>2</td><td>1</td><td>0</td></tr><tr><td colspan="16">DR[15:0]</td></tr></table>

<table><tr><td>位</td><td>名称</td><td>访问</td><td>描述</td><td>复位值</td></tr><tr><td>[15:0]</td><td>DR[15:0]</td><td>RW</td><td>数据寄存器。数据寄存器用于存放接收到的数据或预存将要发送出去的数据,因此数据寄存器的读写实际上是对应操作不同的区域,其中读对应接收缓冲区,写对应发送缓冲区。数据的接收和发送可以是8位或者16位的,需要在传输之前就确定使用多少位的数据。使用8位进行数据传输时,只有数据寄存器的低8位被使用,接收时高8位强制为0。使用16位数据结构则会使全部16位数据寄存器被使用。</td><td>0</td></tr></table>

### 20.4.5 SPI 多项式寄存器（SPIx_CRCR）（x=1/2/3）

偏移地址： $0 \times 1 0$

<table><tr><td>15</td><td>14</td><td>13</td><td>12</td><td>11</td><td>10</td><td>9</td><td>8</td><td>7</td><td>6</td><td>5</td><td>4</td><td>3</td><td>2</td><td>1</td><td>0</td></tr><tr><td colspan="16">CRCPOLY[15:0]</td></tr></table>

<table><tr><td>位</td><td>名称</td><td>访问</td><td>描述</td><td>复位值</td></tr><tr><td>[15:0]</td><td>CRCPOLY[15:0]</td><td>RW</td><td>CRC多项式。此域定义CRC计算用到的多项式。</td><td>0007h</td></tr></table>

### 20.4.6 SPI 接收 CRC 寄存器（SPIx_RCRCR）（x=1/2/3）

偏移地址： $0 \times 1 4$

15 14 13 12 11 10 9 8 7 6 5 4 3 2 1 0

RXCRC[15:0]

<table><tr><td>位</td><td>名称</td><td>访问</td><td>描述</td><td>复位值</td></tr><tr><td>[15:0]</td><td>RXCRC[15:0]</td><td>R0</td><td>接收CRC值。存储着计算出来的接收到的字节的CRC校验的结果。对CRCEN置位会复位该寄存器。计算方法使用CRCPOLY用到的多项式。8位模式下只有低8位参与计算，16位模式下全部16位都会参与计算。需要在BSY为0时去读取这个寄存器。</td><td>0</td></tr></table>

### 20.4.7 发送 CRC 寄存器（SPIx_TCRCR）（x=1/2/3）

偏移地址： $0 \times 1 8$

15 14 13 12 11 10 9 8 7 6 5 4 3 2 1 0

TXCRC[15:0]

<table><tr><td>位</td><td>名称</td><td>访问</td><td>描述</td><td>复位值</td></tr><tr><td>[15:0]</td><td>TXCRC[15:0]</td><td>R0</td><td>发送CRC值。存储着计算出来的已经发送出去的字节的CRC校验的结果。对CRCEN置位会复位该寄存器。计算方法使用CRCPOLY用到的多项式。8位模式下只有低8位参与计算,16位模式下全部16位都会参与计算。需要在BSY为0时去读取这个寄存器。</td><td>0</td></tr></table>

### 20.4.8 SPI_I2S 配置寄存器（SPI_I2S_CFGR）（x=1/2/3）

偏移地址： ${ 0 } { \times 1 } { 0 }$

15 14 13 12 11 10 9 8 7 6 5 4 3 2 1 0

<table><tr><td>Reserved</td><td>12S
MOD</td><td>12SE</td><td>12SCFG
[1:0]</td><td>PCM
SYNC</td><td>Reser
ved</td><td>12SSTD
[1:0]</td><td>CKPOL</td><td>DATLEN
[1:0]</td><td>CHLEN</td></tr></table>

<table><tr><td>位</td><td>名称</td><td>访问</td><td>描述</td><td>复位值</td></tr><tr><td>[15:12]</td><td>Reserved</td><td>R0</td><td>保留</td><td>0</td></tr><tr><td>11</td><td>I2SMOD</td><td>RW</td><td>I2S模式选择,该位只有在关闭了SPI或者I2S时才能设置。1:选择I2S模式;0:选择SPI模式。</td><td>0</td></tr><tr><td>10</td><td>I2SE</td><td>RW</td><td>I2S使能,在SPI模式下不使用。1:I2S使能;0:关闭I2S。</td><td>0</td></tr><tr><td>[9:8]</td><td>I2SCFG[1:0]</td><td>RW</td><td>I2S模式选择,此该位只有在关闭了I2S时才能设置:00:从设备发送;</td><td>00b</td></tr><tr><td></td><td></td><td></td><td>01:从设备接收;10:主设备发送;11:主设备接受。</td><td></td></tr><tr><td>7</td><td>PCMSYNC</td><td>RW</td><td>PCM帧同步。该位只在I2SSTD=11(使用PCM标准)时有意义。1:长帧同步;0:短帧同步。</td><td>0</td></tr><tr><td>6</td><td>Reserved</td><td>RO</td><td>保留</td><td>0</td></tr><tr><td>[5:4]</td><td>I2SSTD[1:0]</td><td>RW</td><td>I2S标准选择,只有在关闭了I2S时才能设置该位。00:I2S飞利浦标准;01:高字节对齐标准(左对齐);10:低字节对齐标准(右对齐);11:PCM标准。</td><td>00b</td></tr><tr><td>3</td><td>CKPOL</td><td>RW</td><td>静止态时钟极性,为了正确操作,该位只有在关闭了I2S时才能设置。1:I2S时钟静止态为高电平;0:I2S时钟静止态为低电平。</td><td>0</td></tr><tr><td>[2:1]</td><td>DATLEN[1:0]</td><td>RW</td><td>待传输数据长度,为了正确操作,该位只有在关闭了I2S时才能设置。00:16位数据长度;01:24位数据长度;10:32位数据长度;11:不允许。</td><td>00b</td></tr><tr><td>0</td><td>CHLEN</td><td>RW</td><td>声道长度,只有在DATLEN=00时该位的写操作才有意义,否则声道长度都由硬件固定为32位。。1:32位宽;0:16位宽。</td><td>0</td></tr></table>

### 20.4.9 SPI_I2S 预分频寄存器（SPIx_I2SPR）（x=2/3）

偏移地址： $_ { 0 \times 2 0 }$

<table><tr><td>Reserved</td><td>MCKOE</td><td>ODD</td><td>I2SDIV[7:0]</td></tr></table>

<table><tr><td>位</td><td>名称</td><td>访问</td><td>描述</td><td>复位值</td></tr><tr><td>[15:10]</td><td>Reserved</td><td>R0</td><td>保留</td><td>0</td></tr><tr><td>9</td><td>MCKOE</td><td>RW</td><td>主设备时钟输出使能,为了正确操作,该位只有在关闭了I2S时才能设置。仅在I2S主设备模式下使用该位。1:主设备时钟输出使能;0:关闭主设备时钟输出。</td><td>0</td></tr><tr><td>8</td><td>ODD</td><td>RW</td><td>奇系数预分频,为了正确操作,该位只有在关闭了I2S时才能设置。仅在I2S主设备模式下使用该位。</td><td>0</td></tr><tr><td></td><td></td><td></td><td>1: 实际分频系数 = (I2SDIV * 2) + 1;0: 实际分频系数 = I2SDIV *2。</td><td></td></tr><tr><td>[7:0]</td><td>I2SDIV[7:0]</td><td>RW</td><td>I2S 线性预分频。为了正确操作,该位只有在关闭了 I2S 时才能设置。仅在 I2S 主设备模式下使用该位。禁止设置 I2SDIV [7:0] = 0 或者 I2SDIV [7:0] = 1参见 19.3.3 节。</td><td>0</td></tr></table>

### 20.4.10 SPI 高速控制寄存器（SPIx_HSCR）（x=1/2/3）

偏移地址： ${ 0 } { \times 2 4 }$

<table><tr><td>15</td><td>14</td><td>13</td><td>12</td><td>11</td><td>10</td><td>9</td><td>8</td><td>7</td><td>6</td><td>5</td><td>4</td><td>3</td><td>2</td><td>1</td><td>0</td></tr><tr><td colspan="15">Reserved</td><td>HSRX
EN</td></tr></table>

<table><tr><td>位</td><td>名称</td><td>访问</td><td>描述</td><td>复位值</td></tr><tr><td>[15:1]</td><td>Reserved</td><td>R0</td><td>保留</td><td>0</td></tr><tr><td>0</td><td>HSRXEN</td><td>WO</td><td>SPI高速模式下(CLK大于等于36MHz)读使能。该模式仅在时钟2分频(即CTLR1寄存器的BR=000)时有效。该位不可读。1:使能高速读模式;0:关闭高速读模式。</td><td>0</td></tr></table>

# 第 21 章 USB 全速设备控制器（USBD）

本章模块描述适用于CH32F2x、CH32V2x和CH32V3x微控制器系列部分产品。

USBD模块是基于USB2.0全速设备技术规范，设计的USB全速、低速协议通讯控制器。内置硬件自动处理物理信号的反向不归零（NRZI)编码/解码、位填充。控制可驱动出 USB 总线多种状态、协议包收发，并提供自动应答进行流控保证应用程序处理时间等功能。

## 21.1 主要特性

符合 USB2.0 全速设备技术规范  
支持 USB 全速 12Mbps、低速 1.5Mbps 模式  
支持配置16个传输通道  
$\bullet$ 支持端点地址范围0-15  
$\bullet$ 支持控制、中断、批量、同步传输  
$\bullet$ 支持批量/同步端点的双缓冲机制  
USB 挂起、唤醒、恢复操作  
$\bullet$ 硬件自动进行数据PID翻转、传输流控  
$\bullet$ 帧锁定时钟脉冲生成

注：USBD和CAN控制器在设计中共享了一个专用的512字节SRAM区域用于数据的发送和接收，因此同时使用USBD和CAN功能时，需要合理分配此共享区域，防止出现数据冲突。

## 21.2 功能描述

### 21.2.1 功能介绍

USBD 模块为 USB 主机（一般是 PC）和微控制器之间的数据通讯提供了一条符合 USB 规范的通信连接，使用时由应用程序和模块硬件配合完成。模块中包含一块共享的 512 字节专用 SRAM 区域作为USB收发数据缓冲区，由配置的端点数目和每个端点最大数据包长度决定实际使用范围。最多可用于16个单向或8个双向端点。

USBD 模块功能包括：

物理信号编码/解码：根据 USB 规范实现令牌包、数据包、握手包的 PID 检测，包括位填充、CRC的生成和校验、帧头同步识别等。  
事务处理：判断正确传输和错误状态，提供各自标志状态及中断通知。  
总线挂起/复位/唤醒状态识别通知。  
自动数据包 PID：根据协议，对非同步端点、同步端点的收发数据包 PID 进行硬件翻转或锁定，减少应用程序工作。  
自动应答包 PID：根据协议，完成一次 USB 事务后，对非同步端点会自动修改应答包状态来为应用程序提供足够的处理和准备时间，但不影响 USB总线上的物理收发。  
l 管理数据收发：定位端点配置及缓冲区描述区域，检测缓冲区边界防溢出。单缓冲/双缓冲区域管理、按端点类型中断上报优先级管理等。  
提供通用类、端点类、缓冲区描述类寄存器配置。

应用程序可以：

获取基于 USB 协议的帧间隔时间点，总线状态：挂起、复位。  
$\bullet$ 自定义端点数目、端点类型、端点大小。自定义传输数据缓冲区域。  
$\bullet$ 获取当前或已挂起端点的服务进行处理。  
$\bullet$ 获取如位填充、格式、CRC、协议、缺失 ACK、缓冲区溢出/缓冲区未满等错误状态。  
$\bullet$ 驱动模块进入低功耗模式。

USBD模块提USB事件映射到3个不同的 NVIC或 PFIC请求线上（使用了3 个中断号）：

1） USB高优先级中断：仅能由同步和双缓冲批量传输的正确传输事件触发，目的是保证最大的传输速率。  
2） USB 低优先级中断：可由所有 USB 事件触发(正确传输，USB 复位等)。固件在处理中断前应当首先确定中断源。  
3） USB 唤醒中断：由 USB 挂起模式的唤醒事件触发。

### 21.2.2 功能配置

#### 1）GPIO 端口

一旦使能了 USBD模块，作为UDP和UPM的 GPIO 口会自动连接到内部 USB收发器，而断开其 GPIO外设的端口设置。所以推荐GPIO口配置为推挽方式输出低电平，防止在未开启 USBD 功能前，出现端口不确定状态或连接PC主机时，提前通知有 USB设备接入。

USBD模块内置USB设备模式的1.5K上拉电阻，无需外接上拉电阻。具体配置请参考配置扩展控制寄存器（EXTEN_CTR）说明。

#### 2）模块初始化

首先，USB收发器相关的模拟部分需要标准的48MHz时钟作为基准时钟，此时钟来源于 AHB总线。应用程序需要先通过配置时钟管理逻辑的相应控制位（RCC_CFGR0寄存器）保证当前USB时钟是48MHz，再使能USB接口时钟，使程序可以访问USBD模块的寄存器。

其次，在模块强制复位时（USBD_CNTR 寄存器上的FRES 位默认为 1），应用程序应该初始化所需要的寄存器和分组缓冲区描述表。包括：分组缓冲区描述表地址寄存器（USBD_BTABLE）、端点配置寄存器（USBD_EPRx）和分组缓冲区描述表寄存器。配置 USBD_DADDR 寄存器 ADD[6:0]域为 0（USB 协议默认地址），置位EF位使能端点传输功能。

最后，启用内部1.5K上拉电阻和设置速度模式（EXTEN_CTR寄存器），然后，清除 USBD_CNTR寄存器上的 FRES 位，撤销 USBD 模块强制复位状态来使能 USBD 模块，清除 USBD_ISTR 寄存器的各种状态标志，以便在使能其他任何单元的操作之前清除未处理的假中断标志。开启 USBD_CNTR 寄存器中需要的中断控制位。

#### 3）USB 复位

USB 复位包括：USBD模块强制复位和 USB总线复位（协议复位）。两者皆会产生 USBD_ISTR寄存器的 RST 标志。发生 USB 复位时，所有端点的通信都被禁止(USBD 模块不会响应任何包传输)。在 USB复位后，USBD 模块被使能，同时 USB 端点也需要被使能以便可以响应 USB 主机（USBD_DADDR 寄存器的 EF 位为 1）。在 USB 设备的枚举阶段，主机将分配给设备一个唯一的地址，这个地址必须写入USBD_DADDR 寄存器的 ADD[6:0]位中。

注：RST标志来源USBD模块强制复位控制位（FRES）的状态和USB总线复位信号起始。

#### 4）端点配置及缓冲区描述表

每个端点配置寄存器可以配置一个双向端点单缓冲属性，也可以配置一个单向端点双缓冲属性。例如：配置双向端点单缓冲属性，端点配置寄存器 3（USBD_EPRx），EA[3:0]为 2，那么可以在 USB 传输上存在端点 2 上传通道和端点 2 下传通道（具体由描述符信息决定）；配置单向端点双缓冲属性（只针对批量端点和同步端点），端点配置寄存器 3（USBD_EPRx），EA[3:0]为2，端点类型（EPTYPE）为同步或批量端点，EP_KIND位置1，那么可以在USB传输上存在端点 2 上传通道或端点 2下传通道，2选1，收发速度上与单缓冲相比更快，微控制器处理和 USBD模块物理收发可以同步进行，降低等待时间。

注：USBD模块内置冲突仲裁机制，使得微控制器和USBD模块对分组缓冲区的访问如同对一个双端口

的访问， 即使微控制器连续访问缓冲区， 也不会产生访问冲突。

每个端点配置寄存器对应一组缓冲区描述类寄存器（描述表）及相应数据收发缓冲区域，他们都位于共享的 512 字节专用 SRAM 区域内（基地址 $0 \times 4 0 0 0 6 0 0 0 ^ { \cdot }$ ）。其中，USBD_BTABLE 寄存器定义缓冲区描述表在 SRAM 区域内的起始地址，而数据收发缓冲区域可以位于整个专用 SRAM 区域内的任意位置，因为它们的地址和长度都定义在对应的缓冲区描述表中，注意分配冲突问题。

注：当使用CAN时，CAN过滤器表使用共享的512字节专用SRAM区域中的高256字节，USB使用低字节。

![](images/d58696d7c41a3065caa064599be0829cedc3a052daa76ef14f1f8709d9217c91.jpg)  
图21-1 缓冲区描述表结构

不论接收或发送，分组缓冲区都是从底部开始使用的。USBD 模块不会改变超出当前分配到的缓冲区区域以外的其他缓冲区的内容。如果缓冲区收到一个比自己大的数据分组，它只会接收最大为自身大小的数据，其他的丢掉，即发生了所谓的缓冲区溢出异常。

#### 1）端点初始化

初始化端点的第一步是把适当的值写到 USBD_ADDRx_TX 或 USBD_ADDRx_RX 寄存器中，以便 USBD模块能找到要传输的数据或准备好接收数据的缓冲区。USBD_EPRx 寄存器的 EPTYPE[1:0]位确定端点的基本类型，EP_KIND 位确定端点的特殊特性。作为发送方，需要设置 USBD_EPRx 寄存器的 STAT_TX位来使能端点，并配置 COUNTx_TX 位决定发送长度。作为接收方，需要设置 STAT_RX 位来使能端点，并且设置BL_SIZE和NUM_BLOCK位，确定接收缓冲区的大小，以检测缓冲区溢出的异常。对于非同步非双缓冲批量传输的单向端点，只需要设置一个传输方向上的寄存器。一旦端点被使能，应用程序就不 能 再 修 改 USBD_EPRx 寄 存 器 的 值 和 USBD_ADDRx_TX/USBD_ADDRx_RX 、 USBD_COUNTx_TX/USBD_COUNTx_RX寄存器所在的位置，因为这些值会被硬件实时修改。当数据传输完成时，CTR中断会产生，此时上述寄存器可以被访问，并重新使能新的传输。

#### 2）IN 事务（进行数据发送）

当接收到 IN 令牌包时，如果接收到的地址和一个配置好的端点地址相符合，并且此时寄存器USBD_EPRx上的STAT_TX位表示可发送的话，USBD模块将会根据缓冲区描述表的内容及DTOG_TX位进行组包编码发出数据包。如果收到的令牌包所对应的端点是无效的，将根据 USBD_EPRx 寄存器上的

STAT_TX 位发送 NAK 或 STALL 握手包而不发送数据包。

在接收到主机响应的 ACK 握手包后，USBD_EPRx 寄存器的值有以下更新：DTOG_TX 位被翻转，STAT_TX位为‘10’（NAK状态），使端点无效，CTR_TX 位被置位。应用程序需要通过 USBD_ISTR寄存器的 EP_ID 和 DIR 位识别产生中断的 USB 端点。CTR_TX 事件的中断服务程序需要首先清除中断标志位，如果要继续发送数据（可以在任何需要发送数据时执行），需要准备好需要发送的数据缓冲区，更新 COUNTx_TX 为下次需要传输的字节数，最后再设置 STAT_TX 位为‘11’(ACK，端点有效)，再次使能数据传输。当 STAT_TX 位为‘10’时(NAK 状态)，任何发送到该端点的 IN 请求都会被 NAK，USB主机会重发IN请求直到该端点确认请求有效。

#### 3）OUT 事务和 SETUP 事务（进行数据接收）

USBD 模块对这两种事务的处理方式基本相同；当接收到一个 OUT 或 SETUP 包时，如果接收到的地址和一个配置好的端点地址相符合，并且此时寄存器 USBD_EPRx 上的 STAT_RX 位表示可接收的话，USBD 模块根据 DTOG_RX 位判断接收数据是否 PID 匹配，如果匹配将访问缓冲区描述表，找到与该端点相关的 ADDRx_RX 和 COUNTx_RX 寄存器，将接收的数据包(先收到的为低字节)保存到 ADDRx_RX 定义的地址空间内并根据 BL_SIZE 和 NUM_BLOCK 的值检测接收是否溢出缓冲区。如果传输中没有任何错误发生，则发送ACK握手包到主机。即使发生 CRC错误或其他类型的错误(位填充，帧错误等)，数据还是会被保存到分组缓冲区中，至少会保存到发生错误的数据点，只是不会发送 ACK 握手包，并且USBD_ISTR寄存器的ERR位将会置位。在这种情况下，应用程序通常不需要干涉处理，USBD模块将从传输错误中自动恢复，并为下一次传输做好准备。如果收到的包所对应的端点没有准备好，USBD 模块将根据 USBD_EPRx 寄存器的 STAT_RX 位发送 NAK 或 STALL 握手包，数据将不会被写入接收缓冲区。

ADDRx_RX的值决定接收缓冲区的起始地址，COUNTx_RX 决定接收缓冲区大小（期望有效数据长度$^ { + 2 }$ 字节 CRC）。如果接收到的数据包长度超出了缓冲区的范围，超过范围的数据不会被写入缓冲区，USBD模块将报告缓冲区发生溢出，并向主机发送 STALL 握手包，并置位分组缓冲区溢出标志 PMAOVR。

如果传输正确完成，USBD 模块将发送 ACK 握手包，并将实际接收数据包中的有效数据字节数写入 COUNTx_RX 寄存器中。USBD_EPRx 寄存器的值有以下更新：DTOG_RX 位翻转，STAT_RX 位为‘10（NAK状态）使端点无效，CTR_RX位被置位。应用程序需要通过 USBD_ISTR 寄存器的 EP_ID和 DIR位识别产生中断的 USB 端点。CTR_RX 事件的中断服务程序首先要根据 SETUP 位确定传输的类型，同时清除中断标志位，然后读相关的缓冲区描述表表项指向的 COUNTx_RX 寄存器，获得此次传输的总字节数，处理接收数据。处理完后，应用程序需要将 USBD_EPRx中的 STAT_RX位置成‘11’（ACK状态），使能下一次的传输。当 STAT_RX 位为‘10’时(NAK 状态)，任何一个发送到端点上的 OUT 请求都会被NAK，SETUP 请求除外（协议规定 SETUP 请求必须以 ACK 握手包接收）。PC 主机将不断重发被 NAK 的OUT 事务包，直到收到端点的 ACK 握手包。

#### 4）控制传输

控制（SETUP）传输一定发生在端点 0 上，所以也称端点 0 位控制端点。控制传输由 3 个阶段组成，首先是主机发送SETUP事务的SETUP阶段，然后是主机发送零个或多个数据（IN/OUT 事务）的数据阶段，最后是状态阶段，由与数据阶段方向相反的数据事务构成。

SETUP 事务非常类似于 OUT 事务的传输过程，所以控制端点在每次发生 CTR_RX 中断时，都必须检查USBD_EPRx寄存器的SETUP位，以识别是普通的 OUT事务还是 SETUP 事务。当主机发送 SETUP 事务下来，USBD模块会固定回复ACK握手包接收下来，而忽略判断 STAT_RX和 DTOG_RX 的内容。然后强制将 DTOG_RX 和 DTOG_TX 设置为 DATA1 状态，并设置 STAT_RX 和 STAT_TX 为‘10’（NAK），保证应用程序可以根据 SETUP 事务中的相应数据决定后面的传输是 IN 还是 OUT。如果拒绝后续数据传输或出现错误，应用程序可以设置STAT_RX或STAT_TX为‘01’，应答 STALL 握手包。如果应用程序收到一个 SETUP 事务并处理时，此时 CTR_RX 仍然保持置位，又收到一个 SETUP 包，USBD 模块会丢掉此SETUP包，并不给于任何握手包应答，以此来模拟一个接收错误，迫使主机再次发送 SETUP包，这样做是为了避免丢失紧随一次CTR_RX中断之后的又一个 SETUP 事务传输。

在控制传输的状态阶段，如果执行的是由主机发送给设备的 OUT 事务，那么 STATUS_OUT 位(USBD_EPRx 寄存器中的 EP_KIND)应该被置位，只有这样，在状态阶段传输过程中收到了非零长度的数据分组，才会产生传输错误。在完成状态阶段传输后，应用程序应该清除 STATUS_OUT 位，并且将STAT_RX 设为 ACK 表示已准备好接收一个新的命令请求，将 STAT_TX 则设为 NAK，不接受任何数据上传的请求。

### 21.2.3 双缓冲机制

在USB协议标准里，对不同的数据传输方式进行了应用描述。其中，批量传输适用于 USB主机和设备间进行大批量的数据传输，主机在帧时间内利用尽可能多的带宽执行批量传输。但这种传输需要保证数据的正确性和完整性，所以传输中包含令牌包、数据包、握手包的顺序进行。同步传输适用于对数据要求恒定速率传送，但对错误有一定容忍性，认为传输一般都可以成功，主机在每个帧时间内有固定的带宽来执行同步传输以此保证传输速率，所以传输中包含令牌包、数据包的顺序进行，没有握手包来确认传输状态和终止传输。

#### 21.2.3.1 单向双缓冲批量端点

批量传输，在单缓冲方式下，当应用程序处理批量端点的前一次的数据传输时，又收到新的数据包，USBD模块将回应NAK握手包，使PC主机不断重发同样的数据包，直到应用程序重新设置 ACK握手包。这样的重传占用了很多带宽，影响了批量传输的速率。因此对批量端点引入双缓冲机制来提高数据传输率。在双缓冲方式下，单向批量端点有 2个数据缓冲区，即该端点的接收和发送两块数据缓冲区。数据翻转位（DTOG_RX 或 DTOG_TX）用来选择当前使用到两块缓冲区中的哪一块，使应用程序可以在 USBD 模块访问其中一块缓冲区的同时，对另一块缓冲区进行操作。例如，对一个双缓冲批量端点进行 OUT 事务传输时，USBD 模块将来自 PC 主机的数据保存到一个缓冲区，同时应用程序可以对另一个缓冲区中的数据进行处理(对于IN事务来说，情况是一样的)。这样利用 USBD 模块的接收或发送数据的时间完成应用程序的数据处理，提高了 USB收发效率。因为对于一个传输方向需要 2个缓冲区，所以配置双向缓冲区的批量端点必须配置为单向端点，其USBD_EPRx 寄存器只需设定 STAT_RX 位(作为双缓冲批量接收端点)或 STAT_TX 位(作为双缓冲批量发送端点)。为尽可能利用双缓冲的优势，达到较高的传输速率，USBD模块处理双缓冲批量端点的流控与其他端点的稍有不同。它只在缓冲区发生访问冲突时才会设置端点为 NAK 状态，而不是在每次传输成功后都将端点设为 NAK 状态。

USBD_EPRx 寄存器中的 DTOG_xx 位用来标识 USBD 模块和应用程序当前分别使用的存储缓存区，以避免发生访问冲突。当配置为单向发送双缓冲区端点时，DTOG_TX标识 USBD 模块当前使用缓冲区，而DTOG_RX标识应用程序当前使用缓冲区；当配置为单向接收双缓冲区端点时，DTOG_RX标识 USBD 模块当前使用缓冲区，而 DTOG_TX 标识应用程序当前使用缓冲区。我们命名 USBD 模块使用缓冲区标识为DTOG，应用程序使用缓冲区标识为SW_BUF。所以双缓冲单向批量端点标识定义如下：

表 21-1 缓冲区标识  

<table><tr><td>缓冲区标识位</td><td>发送端点</td><td>接收端点</td></tr><tr><td>DTOG</td><td>DTOG_TX (USBD_EPRx 寄存器 bit6)</td><td>DTOG_RX (USBD_EPRx 寄存器 bit14)</td></tr><tr><td>SWBUF</td><td>DTOG_RX (USBD_EPRx 寄存器 bit14)</td><td>DTOG_TX (USBD_EPRx 寄存器 bit6)</td></tr></table>

表21-2 双缓冲批量端点缓冲区  

<table><tr><td>端点类型</td><td>DTOG</td><td>SW BUF</td><td>USBD 模块使用缓冲区</td><td>应用程序使用缓冲区</td></tr><tr><td rowspan="4">IN 端点</td><td>0</td><td>1</td><td>ADDRx_TX_0/COUNTx_TX_0</td><td>ADDRx_TX_1/COUNTx_TX_1</td></tr><tr><td>1</td><td>0</td><td>ADDRx_TX_1/COUNTx_TX_1</td><td>ADDRx_TX_0/COUNTx_TX_0</td></tr><tr><td>0</td><td>0</td><td>设置端点处于 NAK 状态</td><td>ADDRx_TX_0/COUNTx_TX_0</td></tr><tr><td>1</td><td>1</td><td>设置端点处于 NAK 状态</td><td>ADDRx_TX_1/COUNTx_TX_1</td></tr><tr><td>OUT 端点</td><td>0</td><td>1</td><td>ADDRx_RX_0/COUNTx_RX_0</td><td>ADDRx_RX_1/COUNTx_RX_1</td></tr><tr><td rowspan="3"></td><td>1</td><td>0</td><td>ADDRx_RX_1/COUNTx_RX_1</td><td>ADDRx_RX_0/COUNTx_RX_0</td></tr><tr><td>0</td><td>0</td><td>设置端点处于 NAK 状态</td><td>ADDRx_RX_0/COUNTx_RX_0</td></tr><tr><td>1</td><td>1</td><td>设置端点处于 NAK 状态</td><td>ADDRx_RX_1/COUNTx_RX_1</td></tr></table>

应用程序配置一个双缓冲批量端点，需要设置 USBD_EPRx 寄存器的 EPTYPE[1:0]为‘00’，EP_KIND位为‘1’。根据传输开始时用到的缓冲区来初始化DTOG和 SW_BUF位。每成功完成一次传输后，USBD模块将根据双缓冲批量端点的流量控制操作，并且持续到 EP_KIND变为无效为止。每次传输结束，根据端点的传输方向，CTR_RX位或CTR_TX位将会置位。与此同时，硬件将设置相应的 DTOG_xx位（翻转），并实现缓冲区交换，如果没有发生 USBD模块和应用程序的缓冲区访问冲突(即 DTOG和 SW_BUF为相同的值，见表154)，则保持STAT_xx位的状态值，否则将会被置为‘10’（NAK状态）。所以应用程序访问缓冲区之后，需要及时翻转SW_BUF位，以通知 USB模块该块缓冲区已变为可用状态。

#### 21.2.3.2 同步端点

同步传输一般用于传输音频流、压缩的视频流等对数据传输率有严格要求的数据。执行同步传输的端点即为同步端点。USB 主机会在每个帧时间内分配固定的带宽给同步端点进行 IN 事务或 OUT 事务传输，并且没有重传机制，无握手协议，同时传输的数据包 PID固定为 DATA0，不会出现 DATA0和DATA1 数据翻转机制（控制/批量/中断传输中出现）。

因为同步传输中没有握手机制，USBD_EPRx 寄存器的STAT_RX位和 STAT_TX位分别只能设成‘00(禁止传输)和‘11’(运行传输)两种状态。同步传输使用双缓冲机制来简化软件流程，它同样使用两个缓冲区，以确保在USB模块使用其中一块缓冲区时，应用程序可以访问另外一块缓冲区。不同于单向批量端点的双缓冲机制，同步端点由于在 USB 标准中传输有固定的时间间隔，及容错能力，所以USBD 模块不判断与应用程序缓冲区冲突情况，只使用 DTOG 位来标识自身当前使用的缓冲区（USBD_EPRx 寄存器中的 DTOG_RX 位用来标识接收同步端点，DTOG_TX 位用来标识发送同步端点）。

表21-3 同步端点缓冲区标识  

<table><tr><td>端点类型</td><td>DTOG</td><td>USBD模块使用缓冲区</td><td>应用程序使用缓冲区</td></tr><tr><td rowspan="2">IN端点</td><td>0</td><td>ADDRx_TX_0/COUNTx_TX_0</td><td>ADDRx_TX_1/COUNTx_TX_1</td></tr><tr><td>1</td><td>ADDRx_TX_1/COUNTx_TX_1</td><td>ADDRx_TX_0/COUNTx_TX_0</td></tr><tr><td rowspan="2">OUT端点</td><td>0</td><td>ADDRx_RX_0/COUNTx_RX_0</td><td>ADDRx_RX_1/COUNTx_RX_1</td></tr><tr><td>1</td><td>ADDRx_RX_1/COUNTx_RX_1</td><td>ADDRx_RX_0/COUNTx_RX_0</td></tr></table>

应用程序配置一个同步端点，需要设置USBD_EPRx寄存器的 EPTYPE[1:0]为‘10’。根据传输开始时用到的缓冲区来初始化 DTOG 位。每成功完成一次传输后，根据端点的传输方向，CTR_RX 位或CTR_TX 位将会置位。与此同时，硬件将设置相应的 DTOG_xx 位（翻转）实现缓冲区交换，但不会改变期望或发送的数据包 PID（固定为 DATA0）。STAT_RX 或 STAT_TX 位不会发生变化。同步传输中，即使OUT 事务发生 CRC 错误或者缓冲区溢出，本次传输仍被看作是正确的，并且可以触发 CTR_RX 中断事件，但是，发生 CRC 错误时硬件会设置 USB_ISTR 寄存器的 ERR 位，提醒应用程序数据可能损坏。

### 21.2.4 挂起/唤醒流程

USB 标准中定义了一种总线状态——总线挂起，如果 USB总线在 3ms内没有任何活动就进入挂起状态。在这种状态下USB总线上提供电流会降低（低速设备一般不超过 500uA，高速或支持远程唤醒功能设备一般不超过2.5mA）。这种电流限制对于由总线供电的 USB设备至关重要，而自供电的设备则不需要严格遵守这样的电流消耗限制。

正常工作状态下，USB 主机会以 1ms 间隔时间发送 SOF 包，所以如果 USBD 模块检测到 3 个连续的 SOF 包丢失事件即可判断主机发出了挂起请求，此时，它会置位 USBD_ISTR 寄存器的 SUSP 位，如果使能了中断还会触发挂起中断。USBD 模块会不断检测总线的挂起状态，并更新 SUSP 位（一直处于总线挂起状态下清除 SUSP 位标志仍然会由硬件再次置位）。所以当应用程序收到 USB 总线挂起事件

#### 后，需要执行以下流程：

将USBD_CNTR寄存器的FSUSP位置1，屏蔽硬件的挂起状态检测，防止不断触发挂起事件。

消除或减少 USBD 模块以外其他模块的静态电流消耗。

将USBD_CNTR寄存器的LPMODE位置1，让USBD模块处于低功耗运行状态，但仍可检测总线唤醒信号。

可选择关闭外部振荡器和 PLL，以停止设备的任何活动。

处于挂起状态的 USB 设备或主机，将由“唤醒”序列唤醒。所谓的“唤醒”序列可以由 USB 主机发起唤醒挂起的USB设备，也可以由USB设备触发唤醒挂起的 USB主机，但最终由 USB主机结束“唤醒”序列。此外，作为挂起的USB设备，还需能够检测 RST信号（总线复位）的功能，并将其当作一次正常的复位操作来执行。

挂起的USBD模块收到唤醒信号后会触发一个 WKUP中断事件（通道 42），并将 USBD_ISTR寄存器的WKUP位置1，自动清除LPMODE位。当应用程序收到 USB唤醒事件后，需要执行以下流程：

清除 USBD_CNTR 寄存器的 FSUSP 位，重新开启 USB 总线的挂起状态检测功能；

可选择启动外部振荡器和 PLL。

查询 USBD_FNR 寄存器的 RXDP 和 RXDM 位判断是什么触发了唤醒事件，并执行相应软件操作。

USBD 模块可以发出唤醒序列唤醒被挂起的 USB 主机。在这种情况下，先将 USBD_CNTR 寄存器的RESUME位置1，然后在1ms-15ms之间再把它清为0可以启动唤醒序列。RESUME 位被清零后，唤醒过程将由主机 PC完成（USB主机唤醒后会继续执行此序列以唤醒其他挂载的 USB设备）。应用程序可以查询 USBD_FNR寄存器的RXDP和RXDM位来判断唤醒是否完成。

注：只有在USBD 模块被设置为挂起状态时(设置USB_CNTR寄存器的FSUSP位为‘1’），才可以设置位。

表 21-4 USB 总线状态  

<table><tr><td>RXDP</td><td>RXDM</td><td>条件</td><td>USB总线状态</td></tr><tr><td>0</td><td>0</td><td>&gt;10ms</td><td>总线复位</td></tr><tr><td rowspan="2">0</td><td rowspan="2">1</td><td>&gt;1ms（全速设备）</td><td>唤醒序列开始</td></tr><tr><td>&gt;3ms（低速设备）</td><td>挂起状态</td></tr><tr><td rowspan="2">1</td><td rowspan="2">0</td><td>&gt;3ms（全速设备）</td><td>挂起状态</td></tr><tr><td>&gt;1ms（低速设备）</td><td>唤醒序列开始</td></tr><tr><td>1</td><td>1</td><td>-</td><td>总线错误（或干扰）</td></tr></table>

## 21.3 寄存器描述

USBD模块有以下 3类寄存器：

通用类寄存器：USBD 模块控制、中断相关，基地址 $0 { \times } 4 0 0 0 5 0 0 0$ 。  
$\bullet$ 端点类寄存器：端点配置、收发状态相关，基地址 $0 { \times } 4 0 0 0 5 0 0 0$ 。  
缓冲区描述类寄存器：数据收发缓冲区相关，基地址 $0 \times 4 0 0 0 6 0 0 0$

表 21-5 USBD 通用类寄存器列表  

<table><tr><td>名称</td><td>访问地址</td><td>描述</td><td>复位值</td></tr><tr><td>R16_USBD_CNTR</td><td>0x40005C40</td><td>USB 控制寄存器</td><td>0x0003</td></tr><tr><td>R16_USBD_ISTR</td><td>0x40005C44</td><td>USB 中断状态寄存器</td><td>0x0000</td></tr><tr><td>R16_USBD_FNR</td><td>0x40005C48</td><td>USB 帧编号寄存器</td><td>0x0XXX</td></tr><tr><td>R16_USBD_DADDR</td><td>0x40005C4C</td><td>USB 设备地址寄存器</td><td>0x0000</td></tr><tr><td>R16_USBD_BTABLE</td><td>0x40005C50</td><td>USB 分组缓冲区描述表地址寄存器</td><td>0x0000</td></tr></table>

表 21-6 USBD 端点类寄存器列表  

<table><tr><td>名称</td><td>访问地址</td><td>描述</td><td>复位值</td></tr><tr><td>R16_USBD_EPR0</td><td>0x40005C00</td><td>USB端点配置寄存器0</td><td>0x0000</td></tr><tr><td>R16_USBD_EPR1</td><td>0x40005C04</td><td>USB端点配置寄存器1</td><td>0x0000</td></tr><tr><td>R16_USBD_EPR2</td><td>0x40005C08</td><td>USB端点配置寄存器2</td><td>0x0000</td></tr><tr><td>R16_USBD_EPR3</td><td>0x40005C0C</td><td>USB端点配置寄存器3</td><td>0x0000</td></tr><tr><td>R16_USBD_EPR4</td><td>0x40005C10</td><td>USB端点配置寄存器4</td><td>0x0000</td></tr><tr><td>R16_USBD_EPR5</td><td>0x40005C14</td><td>USB端点配置寄存器5</td><td>0x0000</td></tr><tr><td>R16_USBD_EPR6</td><td>0x40005C18</td><td>USB端点配置寄存器6</td><td>0x0000</td></tr><tr><td>R16_USBD_EPR7</td><td>0x40005C1C</td><td>USB端点配置寄存器7</td><td>0x0000</td></tr></table>

表 21-7 USBD 缓冲区描述类寄存器列表  

<table><tr><td>名称</td><td>访问地址</td><td>描述</td><td>复位值</td></tr><tr><td>R16_USBD_ADDR0_TX</td><td>0x40006000+ [USBD_BTABLE]</td><td>端点发送缓存区地址寄存器0</td><td>0x0000</td></tr><tr><td>R16_USBD_COUNT0_TX</td><td>0x40006004+ [USBD_BTABLE]</td><td>端点发送数据字节数寄存器0</td><td>0x0000</td></tr><tr><td>R16_USBD_ADDR0_RX</td><td>0x40006008+ [USBD_BTABLE]</td><td>端点接收缓存区地址寄存器0</td><td>0x0000</td></tr><tr><td>R16_USBD_COUNT0_RX</td><td>0x4000600C+ [USBD_BTABLE]</td><td>端点接收数据字节数寄存器0</td><td>0x0000</td></tr><tr><td>R16_USBD_ADDR1_TX</td><td>0x40006010+ [USBD_BTABLE]</td><td>端点发送缓存区地址寄存器1</td><td>0x0000</td></tr><tr><td>R16_USBD_COUNT1_TX</td><td>0x40006014+ [USBD_BTABLE]</td><td>端点发送数据字节数寄存器1</td><td>0x0000</td></tr><tr><td>R16_USBD_ADDR1_RX</td><td>0x40006018+ [USBD_BTABLE]</td><td>端点接收缓存区地址寄存器1</td><td>0x0000</td></tr><tr><td>R16_USBD_COUNT1_RX</td><td>0x4000601C+ [USBD_BTABLE]</td><td>端点接收数据字节数寄存器1</td><td>0x0000</td></tr><tr><td>R16_USBD_ADDR2_TX</td><td>0x40006020+ [USBD_BTABLE]</td><td>端点发送缓存区地址寄存器2</td><td>0x0000</td></tr><tr><td>R16_USBD_COUNT2_TX</td><td>0x40006024+ [USBD_BTABLE]</td><td>端点发送数据字节数寄存器2</td><td>0x0000</td></tr><tr><td>R16_USBD_ADDR2_RX</td><td>0x40006028+ [USBD_BTABLE]</td><td>端点接收缓存区地址寄存器2</td><td>0x0000</td></tr><tr><td>R16_USBD_COUNT2_RX</td><td>0x4000602C+ [USBD_BTABLE]</td><td>端点接收数据字节数寄存器2</td><td>0x0000</td></tr><tr><td>R16_USBD_ADDR3_TX</td><td>0x40006030+ [USBD_BTABLE]</td><td>端点发送缓存区地址寄存器3</td><td>0x0000</td></tr><tr><td>R16_USBD_COUNT3_TX</td><td>0x40006034+ [USBD_BTABLE]</td><td>端点发送数据字节数寄存器3</td><td>0x0000</td></tr><tr><td>R16_USBD_ADDR3_RX</td><td>0x40006038+ [USBD_BTABLE]</td><td>端点接收缓存区地址寄存器3</td><td>0x0000</td></tr><tr><td>R16_USBD_COUNT3_RX</td><td>0x4000603C+ [USBD_BTABLE]</td><td>端点接收数据字节数寄存器3</td><td>0x0000</td></tr><tr><td>R16_USBD_ADDR4_TX</td><td>0x40006040+ [USBD_BTABLE]</td><td>端点发送缓存区地址寄存器4</td><td>0x0000</td></tr><tr><td>R16_USBD_COUNT4_TX</td><td>0x40006044+ [USBD_BTABLE]</td><td>端点发送数据字节数寄存器4</td><td>0x0000</td></tr><tr><td>R16_USBD_ADDR4_RX</td><td>0x40006048+ [USBD_BTABLE]</td><td>端点接收缓存区地址寄存器4</td><td>0x0000</td></tr><tr><td>R16_USBD_COUNT4_RX</td><td>0x4000604C+ [USBD_BTABLE]</td><td>端点接收数据字节数寄存器4</td><td>0x0000</td></tr><tr><td>R16_USBD_ADDR5_TX</td><td>0x40006050+ [USBD_BTABLE]</td><td>端点发送缓存区地址寄存器5</td><td>0x0000</td></tr><tr><td>R16_USBD_COUNT5_TX</td><td>0x40006054+ [USBD_BTABLE]</td><td>端点发送数据字节数寄存器5</td><td>0x0000</td></tr><tr><td>R16_USBD_ADDR5_RX</td><td>0x40006058+ [USBD_BTABLE]</td><td>端点接收缓存区地址寄存器5</td><td>0x0000</td></tr><tr><td>R16_USBD_COUNT5_RX</td><td>0x4000605C+ [USBD_BTABLE]</td><td>端点接收数据字节数寄存器5</td><td>0x0000</td></tr><tr><td>R16_USBD_ADDR6_TX</td><td>0x40006060+ [USBD_BTABLE]</td><td>端点发送缓存区地址寄存器6</td><td>0x0000</td></tr><tr><td>R16_USBD_COUNT6_TX</td><td>0x40006064+ [USBD_BTABLE]</td><td>端点发送数据字节数寄存器6</td><td>0x0000</td></tr><tr><td>R16_USBD_ADDR6_RX</td><td>0x40006068+ [USBD_BTABLE]</td><td>端点接收缓存区地址寄存器6</td><td>0x0000</td></tr><tr><td>R16_USBD_COUNT6_RX</td><td>0x4000606C+ [USBD_BTABLE]</td><td>端点接收数据字节数寄存器6</td><td>0x0000</td></tr><tr><td>R16_USBD_ADDR7_TX</td><td>0x40006070+ [USBD_BTABLE]</td><td>端点发送缓存区地址寄存器7</td><td>0x0000</td></tr><tr><td>R16_USBD_COUNT7_TX</td><td>0x40006074+ [USBD_BTABLE]</td><td>端点发送数据字节数寄存器7</td><td>0x0000</td></tr><tr><td>R16_USBD_ADDR7_RX</td><td>0x40006078+[USBD_BTABLE]</td><td>端点接收缓存区地址寄存器7</td><td>0x0000</td></tr><tr><td>R16_USBD_COUNT7_RX</td><td>0x4000607C+[USBD_BTABLE]</td><td>端点接收数据字节数寄存器7</td><td>0x0000</td></tr></table>

注：以上缓冲区描述类寄存器和端点配置寄存器使用相对应。例如：USB端点配置寄存器0对应端点发送缓存区地址寄存器0、端点发送数据字节数寄存器0、端点接收缓存区地址寄存器0、端点接收数据字节数寄存器0。

### 21.3.1 USB 控制寄存器（USBD_CNTR）

偏移地址： $0 \times 4 0$

<table><tr><td>15</td><td>14</td><td>13</td><td>12</td><td>11</td><td>10</td><td>9</td><td>8</td><td>7</td><td>6</td><td>5</td><td>4</td><td>3</td><td>2</td><td>1</td><td>0</td></tr><tr><td>CTRM</td><td>PMA
OVRM</td><td>ERRM</td><td>WKUPM</td><td>SUSPM</td><td>RSTM</td><td>SOFM</td><td>ESOFM</td><td>Reserved</td><td>RESUME</td><td>FSUSP</td><td>LP
MODE</td><td>PDWN</td><td>FRES</td><td></td><td></td></tr></table>

<table><tr><td>位</td><td>名称</td><td>访问</td><td>描述</td><td>复位值</td></tr><tr><td>15</td><td>CTRM</td><td>RW</td><td>正确传输中断使能位:1:使能正确传输(CTR)中断,在中断寄存器的相应位被置1时产生中断;0:禁止正确传输(CTR)中断。</td><td>0</td></tr><tr><td>14</td><td>PMAOVRM</td><td>RW</td><td>分组缓冲区溢出中断使能位:1:使能PMAOVR中断,在中断寄存器的相应位被置1时产生中断;0:禁止PMAOVR中断。</td><td>0</td></tr><tr><td>13</td><td>ERRM</td><td>RW</td><td>出错中断使能位:1:使能出错中断,在中断寄存器的相应位被置1时产生中断;0:禁止出错中断。</td><td>0</td></tr><tr><td>12</td><td>WKUPM</td><td>RW</td><td>唤醒中断使能位:1:使能唤醒中断,在中断寄存器的相应位被置1时产生中断;0:禁止唤醒中断。</td><td>0</td></tr><tr><td>11</td><td>SUSPM</td><td>RW</td><td>挂起中断使能位:1:使能挂起(SUSP)中断,在中断寄存器的相应位被置1时产生中断;0:禁止挂起(SUSP)中断。</td><td>0</td></tr><tr><td>10</td><td>RSTM</td><td>RW</td><td>USB复位(总线复位或强制复位)中断使能位:1:使能USB复位中断,在中断寄存器的相应位被置1时产生中断;0:禁止USB复位中断。</td><td>0</td></tr><tr><td>9</td><td>SOFM</td><td>RW</td><td>帧首(SOF)中断使能位:1:使能SOF中断,在中断寄存器的相应位被置1时产生中断;0:禁止SOF中断。</td><td>0</td></tr><tr><td>8</td><td>ESOFM</td><td>RW</td><td>定时帧首丢失中断使能位:1:使能ESOF中断,在中断寄存器的相应位被置1时产生中断;0:禁止ESOF中断。</td><td>0</td></tr><tr><td>[7:5]</td><td>Reserved</td><td>R0</td><td>保留。</td><td>0</td></tr><tr><td>4</td><td>RESUME</td><td>RW</td><td>唤醒请求控制位:1:输出唤醒信号;0:空闲状态。根据USB协议,如果此位在1ms到15ms内保持有效,主机将对USBD模块实行唤醒操作。注:只有在FSUSP位为1时,才可以设置此位。</td><td>0</td></tr><tr><td>3</td><td>FSUSP</td><td>RW</td><td>屏蔽挂起检测控制位:1:屏蔽总线挂起状态检测。此时USB模拟收发器的时钟和静态功耗仍然保持。如果需要进入低功耗状态(总线供电类的设备),需要先置位FSUSP再置位LPMODE。0:打开总线挂起状态检测。注:当USB总线上保持3ms没有数据通信(包括SOF)时,SUSP中断会被触发,此时软件必需设置此位,否则会一直触发SUSP中断。</td><td>0</td></tr><tr><td>2</td><td>LPMODE</td><td>RW</td><td>低功耗模式控制位:此模式用于在USB挂起状态下降低功耗。在此模式下,除了外接上拉电阻的供电,其他的静态功耗都被关闭,系统时钟将会停止或者降低到一定的频率来减少耗电。USB总线上的活动(唤醒事件)将会清除此位(软件也可以清0)。1:低功耗模式;0:非低功耗模式。</td><td>0</td></tr><tr><td>1</td><td>PDWN</td><td>RW</td><td>断电模式(Power down)此模式用于彻底关闭USB模块。当此位被置位时,不能使用USB模块。0:退出断电模式1:进入断电模式</td><td>1</td></tr><tr><td>0</td><td>FRES</td><td>RW</td><td>强制USB复位控制位:1:对USBD模块强制复位。USBD模块将一直保持在复位状态下直到软件清除此位。如果USB复位中断被使能,将产生复位中断;0:清除USB复位。</td><td>1</td></tr></table>

### 21.3.2 USB 中断状态寄存器（USBD_ISTR）

偏移地址： $0 \times 4 4$

<table><tr><td>15</td><td>14</td><td>13</td><td>12</td><td>11</td><td>10</td><td>9</td><td>8</td><td>7</td><td>6</td><td>5</td><td>4</td><td>3</td><td>2</td><td>1</td><td>0</td></tr><tr><td>CTR</td><td>PMA0VR</td><td>ERR</td><td>WKUP</td><td>SUSP</td><td>RST</td><td>SOF</td><td>ESOF</td><td colspan="2">Reserved</td><td colspan="2">DIR</td><td colspan="4">EP_ID[3:0]</td></tr></table>

<table><tr><td>位</td><td>名称</td><td>访问</td><td>描述</td><td>复位值</td></tr><tr><td>15</td><td>CTR</td><td>RO</td><td>正确的传输状态指示。此位在端点正确完成一次数据传输后由硬件置位。应用程序可以通过DIR和EP_ID位来识别是哪个端点完成了正确的数据传输。</td><td>0</td></tr><tr><td>14</td><td>PMAOVR</td><td>RWO</td><td>分组缓冲区溢出标志。此位在微控制器长时间没有响应一个访问USB分组缓</td><td>0</td></tr><tr><td></td><td></td><td></td><td>冲区请求时由硬件置位。USBD模块通常在以下情况时置位该位:在接收过程中一个ACK握手分组没有被发送,或在发送过程中发生了比特填充错误,在以上两种情况下主机都会要求数据重传。在正常的数据传输中不会产生PMAOVR中断。由于失败的传输都将由主机发起重传,应用程序就可以在这个中断的服务程序中加速设备的其他操作,并准备重传。但这个中断不会在同步传输中产生(同步传输不支持重传)因此数据可能会丢失。此位可读,写0清除,写1无效。</td><td></td></tr><tr><td>13</td><td>ERR</td><td>RWO</td><td>出错标志,下列错误发生时硬件会置位此位:NANS:无应答。主机的应答超时CRC:校验错误。USB的包中的CRC校验出错BST:位填充错误。USB数据位中检测出位填充错误。FVIO:帧格式错误。收到非标准帧(如EOP出现在错误的时刻,错误的令牌等)。USB应用程序通常可以忽略这些错误,因为USBD模块和主机在发生错误时都会启动重传机制。此位产生的中断可以用于应用程序的开发阶段,可以用来监测USB总线的传输质量,标识用户可能发生的错误(连接线松,环境干扰严重,USB线损坏等)。此位可读,写0清除,写1无效。</td><td>0</td></tr><tr><td>12</td><td>WKUP</td><td>RWO</td><td>唤醒信号标志:当USBD模块处于挂起状态时,如果检测到唤醒信号,此位将由硬件置1。此时CTLR寄存器的LP_MODE位将被清0,FSUSP位需要软件清0开启挂起检测。同时USB_WAKEUP被激活,通知设备的其他部分(如唤醒单元)将开始唤醒过程。此位可读,写0清除,写1无效。</td><td>0</td></tr><tr><td>11</td><td>SUSP</td><td>RWO</td><td>总线挂起标志:此位在USB线上超过3ms没有信号传输时由硬件置位。USB复位(总线复位或强制复位)撤销后,硬件立即使能对挂起信号的检测,但在挂起模式下(FUSP=1)硬件不会再检测挂起信号直到唤醒过程结束。此位可读,写0清除,写1无效。</td><td>0</td></tr><tr><td>10</td><td>RST</td><td>RWO</td><td>USB复位(总线复位或者强制复位)标志:此位在USBD模块检测到USB总线复位信号边沿或强制复位状态时由硬件置位。此时USBD模块将复位内部协议状态机,并在中断使能的情况下触发复位中断来响应。USBD模块的发送和接收部分将被禁止,直到此位被清除。所有的配置寄存器不会被复位,除非应用程序对他们清零。这用来保证在复位撤销后USB传输还可以立即正确执行。但设备的地址和端点寄存器会被USB复位所复位。此位可读,写0清除,写1无效。</td><td>0</td></tr><tr><td>9</td><td>SOF</td><td>RWO</td><td>帧首(SOF)标志:</td><td>0</td></tr><tr><td></td><td></td><td></td><td>此位在USBD模块检测到总线上的SOF包时由硬件置位。中断服务程序可以通过检测SOF事件来完成与主机的1ms同步,并正确读出寄存器在收到SOF时的更新内容(此功能在同步传输时非常有意义)。此位可读,写0清除,写1无效。</td><td></td></tr><tr><td>8</td><td>ESOF</td><td>RWO</td><td>定时帧首(ESOF)丢失标志:此位在USBD模块未按时收到SOF包时由硬件置位。主机应该每毫秒都发送SOF包,但如果USBD模块没有收到,挂起定时器将触发此中断。如果连续发生3次ESOF中断,也就是连续3次未收到SOF包,将产生SUSP中断。此位可读,写0清除,写1无效。</td><td>0</td></tr><tr><td>[7:5]</td><td>Reserved</td><td>R0</td><td>保留。</td><td>0</td></tr><tr><td>4</td><td>DIR</td><td>R0</td><td>事务数据传输方向。此位在完成数据传输产生中断后由硬件根据传输方向写入。如果DIR=0,相应端点的CTR_TX位被置位,标志一个IN事务(数据从USBD模块传输到PC主机)的传输完成。如果DIR=1,相应端点的CTR_RX位被置位,标志一个OUT事务(数据从PC主机传输到USBD模块)的传输完成。如果CTR_TX位同时也被置位,就标志同时存在挂起的OUT事务和IN事务。应用程序可以利用该信息访问USBD_EPnR位对应的操作,它表示挂起中断传输方向的信息。</td><td>0</td></tr><tr><td>[3:0]</td><td>EP_ID[3:0]</td><td>R0</td><td>端点号。此位在USBD模块完成数据传输产生中断后由硬件根据请求中断的端点号写入。如果同时有多个端点的请求中断,硬件写入优先级最高的端点号。端点的优先级按以下方法定义:同步端点和双缓冲批量端点具有高优先级,其他的端点为低优先级。如果多个同优先级的端点请求中断,则根据端点号来确定优先级,即端点0具有最高优先级,端点号越小,优先级越高。应用程序可以通过上述的优先级策略顺序处理端点的中断请求。</td><td>0</td></tr></table>

### 21.3.3 USB 帧编号寄存器（USBD_FNR）

偏移地址： $0 \times 4 8$

15 14 13 12 11 10 9 8 7 6 5 4 3 2 1 0

<table><tr><td>RXDP</td><td>RXDM</td><td>LCK</td><td>LSOF [1:0]</td><td>FN[10:0]</td></tr></table>

<table><tr><td>位</td><td>名称</td><td>访问</td><td>描述</td><td>复位值</td></tr><tr><td>15</td><td>RXDP</td><td>RO</td><td>D+数据线电平状态。</td><td>0</td></tr><tr><td>14</td><td>RXDM</td><td>RO</td><td>D-数据线电平状态。</td><td>0</td></tr><tr><td>13</td><td>LCK</td><td>RO</td><td>SOF包计数停止锁定位。USBD模块在复位或唤醒序列结束后会检测SOF包,如</td><td>0</td></tr><tr><td></td><td></td><td></td><td>果连续检测到至少2个SOF包,则硬件会置位此位。此位一旦锁定,帧计数器将停止计数,一直等到USBD模块复位或总线挂起时再恢复计数。</td><td></td></tr><tr><td>[12:11]</td><td>LSOF[1:0]</td><td>RO</td><td>帧首丢失标志位。当ESOF事件发生时,硬件会将丢失的SOF包的数目写入此域。如果再次收到SOF包,此域被清除。</td><td>X</td></tr><tr><td>[10:0]</td><td>FN[10:0]</td><td>RO</td><td>帧编号。此域为最新收到的SOF包中的11位帧编号。主机每发送一个帧,帧编号都会自加,这对于同步传输非常有意义。此域在发生SOF中断时更新。</td><td>X</td></tr></table>

### 21.3.4 USB 设备地址寄存器（USBD_DADDR）

偏移地址： $\mathtt { 0 } \mathtt { x } 4 0$

<table><tr><td>15</td><td>14</td><td>13</td><td>12</td><td>11</td><td>10</td><td>9</td><td>8</td><td>7</td><td>6</td><td>5</td><td>4</td><td>3</td><td>2</td><td>1</td><td>0</td></tr><tr><td colspan="8">Reserved</td><td>EF</td><td colspan="7">ADD[6:0]</td></tr></table>

<table><tr><td>位</td><td>名称</td><td>访问</td><td>描述</td><td>复位值</td></tr><tr><td>[15:8]</td><td>Reserved</td><td>RO</td><td>保留。</td><td>0</td></tr><tr><td>7</td><td>EF</td><td>RW</td><td>USB功能使能位。此位在需要使能USB设备功能时由应用程序置位。如果此位为0,USBD模块将停止工作,忽略所有寄存器的设置,不响应任何USB通信。1:使能USB设备功能;0:停止USB设备功能。</td><td>0</td></tr><tr><td>[6:0]</td><td>ADD[6:0]</td><td>RW</td><td>USB设备地址。此域是USB主机在枚举过程中为USB设备分配的地址值。该地址值和EA位必需和USB令牌包中的地址信息匹配,才能在指定的端点进行正确的USB传输。</td><td>0</td></tr></table>

### 21.3.5 USB 分组缓冲区描述表地址寄存器（USBD_BTABLE）

偏移地址： $_ { 0 \times 5 0 }$

<table><tr><td>15</td><td>14</td><td>13</td><td>12</td><td>11</td><td>10</td><td>9</td><td>8</td><td>7</td><td>6</td><td>5</td><td>4</td><td>3</td><td>2</td><td>1</td><td>0</td></tr><tr><td colspan="13">BTABLE[15:3]</td><td colspan="3">Reserved</td></tr></table>

<table><tr><td>位</td><td>名称</td><td>访问</td><td>描述</td><td>复位值</td></tr><tr><td>[15:3]</td><td>BTABLE[15:3]</td><td>RW</td><td>缓冲表。此域是分组缓冲区描述表的基地址。分组缓冲区描述表用来指示每个端点的分组缓冲区地址和大小,按8字节对齐(即最低3位为000)。每次传输开始时,USBD模块读取相应端点所对应的分组缓冲区描述表获得缓冲区地址和大小信息。</td><td>0</td></tr><tr><td>[2:0]</td><td>Reserved</td><td>RO</td><td>保留。</td><td>0</td></tr></table>

### 21.3.6 USB 端点配置寄存器 $\pmb { \times }$ （USBD_EPRx）（x=0/1/2/3/4/5/6/7）

偏移地址： $0 { \times } 0 0 { - } 0 { \times } 1 0$

<table><tr><td>15</td><td>14</td><td>13</td><td>12</td><td>11</td><td>10</td><td>9</td><td>8</td><td>7</td><td>6</td><td>5</td><td>4</td><td>3</td><td>2</td><td>1</td><td>0</td></tr><tr><td>CTR_RX</td><td>DTOG_RX</td><td colspan="2">STAT_RX[1:0]</td><td>SETUP</td><td colspan="2">EPT_TYPE[1:0]</td><td>EP_KIND</td><td>CTR_TX</td><td>DTOG_TX</td><td colspan="2">STAT_TX[1:0]</td><td colspan="4">EA[3:0]</td></tr></table>

<table><tr><td>位</td><td>名称</td><td>访问</td><td>描述</td><td>复位值</td></tr><tr><td>15</td><td>CTR_RX</td><td>RWO</td><td>正确接收标志位(OUT/SETUP)。此位在正确接收到OUT或SETUP事务(发送ACK应答)时由硬件置位。如果CTRM位已置位,相应的中断会产生,应用程序需要在处理完该事件后清除此位。收到的是OUT事务还是SETUP事务可以通过下面的SETUP位确定。此位可读,写0清除,写1无效。注:以NAK或STALL应答的事务或出错的传输此位不会置位。</td><td>0</td></tr><tr><td>14</td><td>DTOG_RX</td><td>RW1T</td><td>期望下次接收的数据包PID(OUT/SETUP),硬件设置:1:期望DATA1;0:期望DATA0。对于非同步端点,在接收正确的PID数据包后,USBD模块发送ACK握手包,硬件自动翻转此位。对于控制端点,硬件在收到正确的SETUP包后置位(DATA1)。对于有双缓冲属性的端点,硬件除了自动翻转此位表示期望数据包PID外,还根据此位标识来支持双缓冲区的交换(请参考双缓冲机制中描述)。对于同步端点,硬件不判断数据包PID,仅通过此位标识支持双缓冲区的交换。此位可读,写0无效,写1翻转。注:应用程序可以对此位进行初值设定,或者翻转此位用于特殊用途。</td><td>0</td></tr><tr><td>[13:12]</td><td>STAT_RX[1:0]</td><td>RW1T</td><td>表示数据接收的状态位(OUT/SETUP事务中):00:DISABLED,端点忽略所有的接收请求,不应答;01:STALL,端点以STALL包响应接收请求;10:NAK,端点以NAK包响应接收请求;11:ACK,端点以ACK包响应接收请求。当一次正确的OUT或SETUP数据传输完成后(CTR_RX=1),硬件会自动设置此位为NAK状态,使应用程序有足够的时间处理并响应下一个事务。对于双缓冲批量端点,由于使用特殊的传输流量控制策略,会根据使用的缓冲区状态控制传输状态(请参考双缓冲端点)。对于同步端点,由于端点状态只能是有效或禁用,因此硬件不会在正确的传输之后设置此位。如果将此域设为STALL或NAK,USBD模块响应的操作</td><td>00b</td></tr><tr><td></td><td></td><td></td><td>是未定义的。此域可读,位写0无效,写1翻转。注:应用程序可以对域位进行初值设定。</td><td></td></tr><tr><td>11</td><td>SETUP</td><td>R0</td><td>SETUP事务传输完成标志位:1:是SETUP事务,并正确接收(发送ACK应答);0:非SETUP事务。注意:硬件会在CTR_RX=0条件时,才可能修改此位。</td><td>0</td></tr><tr><td>[10:9]</td><td>EP_TYPE[1:0]</td><td>RW</td><td>传输端点类型:00:BULK,批量端点;01:CONTROL,控制端点;10:ISO,同步端点;11:INTERRUPT,中断端点。只有控制端点才会有SETUP传输,其他类型的端点无视此类传输。SETUP传输不能以NAK或STALL包响应,如果控制端点在收到SETUP包时处于NAK状态,USBD模块将不响应请求,就会出现接收错误。如果控制端点处于STALL状态,SETUP包会被正确接收,数据会被正确传输,并产生一个正确传输完成的中断。控制端点的OUT包按普通端点的方式处理。批量端点和中断端点的处理方式非常类似,仅在对EP_KIND位的处理上有差别。</td><td>00b</td></tr><tr><td rowspan="6">8</td><td rowspan="6">EP_KIND</td><td rowspan="6">RW</td><td>端点特殊类型控制位(配合EP_TYPE使用):EPTYPE[1:0]</td><td>EP_KIND</td></tr><tr><td>BULK</td><td>DBLBuf:开启双缓冲。</td></tr><tr><td>CONTROL</td><td>STATUS_OUT:控制传输状态阶段数据包长度判断。</td></tr><tr><td>ISO</td><td>未使用。</td></tr><tr><td>INTERRUPT</td><td>未使用。</td></tr><tr><td>DBLBuf:设置此位使能批量端点的双缓冲功能。STATUS_OUT:设置此位表示USB设备期望主机发送控制传输中的状态阶段事务,此时,设备对于任何长度不为0的数据包都响应STALL握手包。(此功能仅用于控制端点,有利于提供对于协议层错误的检测。)如果STATUS_OUT位被清除,处于状态阶段的OUT事务可以包含任意长度的数据。</td><td>0</td></tr><tr><td>7</td><td>CTR_TX</td><td>RWO</td><td>正确发送标志位(IN):此位在正确的IN事务(收到ACK应答)完成时由硬件置位。如果CTRM位已被置位,会产生相应的中断,应用程序需要在处理完该事件后清除此位。在IN分组结束时,如果主机响应NAK或STALL则此位不会被置位,因为数据传输没有成功。此位可读,写0清除,写1无效。注:如果主机以NAK或STALL响应则此位不会置位。</td><td>0</td></tr><tr><td>6</td><td>DTOG_TX</td><td>RW1T</td><td>要发送的数据包PID(IN),硬件设置:1:发送DATA1;</td><td>0</td></tr><tr><td></td><td></td><td></td><td>0:发送 DATA0。对于非同步端点,在发送正确的 PID 数据包后,如果USBD 模块收到主机的 ACK 握手包,硬件自动翻转此位。对于控制端点,硬件在收到正确的 SETUP 包后置位(DATA1)。对于有双缓冲属性的端点,硬件除了自动翻转此位表示发送数据包 PID 外,还根据此位标识来支持双缓冲区的交换(请参考双缓冲机制中描述)。对于同步端点,硬件强制发送数据包 DATA0,并且通过此位标识支持双缓冲区的交换。此位可读,写 0 无效,写 1 翻转。注:应用程序可以对此位进行初值设定,或翻转此位用于特殊用途。</td><td></td></tr><tr><td>[5:4]</td><td>STAT_TX[1:0]</td><td>RW1T</td><td>表示发送数据的状态位(IN事务中):00: DISABLED,端点忽略所有的发送请求,不应答;01:STALL,端点以STALL 包响应主机 IN 请求;10:NAK,端点以NAK 包响应主机 IN 请求;11:ACK,端点可以发送数据。当正确完成一次IN事务数据传输完成后(CTR_TX=1),硬件会自动设置此位为NAK 状态,以保证应用程序有足够的时间处理并响应下一个事务传输。对于双缓冲批量端点,由于使用特殊的传输流量控制策略,会根据使用的缓冲区状态控制传输状态(请参考双缓冲端点)。对于同步端点,由于端点状态只能是有效或禁用,因此硬件不会在正确的传输之后设置此位。如果将此域设为STALL 或者NAK,USBD 模块响应的操作是未定义的。此域可读,位写 0 无效,写 1 翻转。注:应用程序可以对域位进行初值设定。</td><td>00b</td></tr><tr><td>[3:0]</td><td>EA[3:0]</td><td>RW</td><td>端点地址域(设置端点号):应用程序要为此端点配置寄存器设置一个端点地址。</td><td>0</td></tr></table>

### 21.3.7 端点发送缓存区地址寄存器 $\pmb { \times }$ （USBD_ADDRx_TX）（x=0/1/2/3/4/5/6/7）

偏移地址：[USBD_BTABLE] $^ +$ x*16

<table><tr><td>15</td><td>14</td><td>13</td><td>12</td><td>11</td><td>10</td><td>9</td><td>8</td><td>7</td><td>6</td><td>5</td><td>4</td><td>3</td><td>2</td><td>1</td><td>0</td></tr><tr><td colspan="15">ADDRx_TX[15:1]</td><td>-</td></tr></table>

<table><tr><td>位</td><td>名称</td><td>访问</td><td>描述</td><td>复位值</td></tr><tr><td>[15:1]</td><td>ADDRx_TX[15:1]</td><td>RW</td><td>待发送数据缓冲区起始地址（IN事务中）。</td><td>0</td></tr><tr><td>0</td><td>-</td><td>RZ</td><td>缓冲区的地址必须按2字节对齐，所以此位必须为0。</td><td>0</td></tr></table>

### 21.3.8 端点发送数据字节数寄存器 $\pmb { \times }$ （USBD_COUNTx_TX）（x=0/1/2/3/4/5/6/7）

偏移地址：[USBD_BTABLE] $+ \times * 1 6 + 4$

<table><tr><td>Reserved</td><td>COUNTx_TX[9:0]</td></tr></table>

<table><tr><td>位</td><td>名称</td><td>访问</td><td>描述</td><td>复位值</td></tr><tr><td>[15:10]</td><td>Reserved</td><td>R0</td><td>保留。</td><td>0</td></tr><tr><td>[9:0]</td><td>COUNTx_TX[9:0]</td><td>RW</td><td>待发送数据长度字节数（IN事务中）。</td><td>0</td></tr></table>

注：双缓冲区和同步IN端点有2个USBD_ADDRx_TX寄存器和2个USB_COUNTx_TX寄存器：分别为USBD_ADDRx_TX_1和USBD_ADDRx_TX_0，USB_COUNTx_TX_1和USB_COUNTx_TX_0，内容如下：

USBD_ADDRx_TX映射为USBD_ADDRx_TX_0

USBD_ADDRx_RX映射为 USBD_ADDRx_TX_1

USBD_COUNTx_TX映射为 USB_COUNTx_TX_0

USBD_COUNTx_RX映射为USB_COUNTx_TX_1

### 21.3.9 端点接收缓存区地址寄存器 $\pmb { \times }$ （USBD_ADDRx_RX）（x=0/1/2/3/4/5/6/7）

偏移地址：[USBD_BTABLE] $^ +$ $+ \times + 1 6 + 8$

<table><tr><td>15</td><td>14</td><td>13</td><td>12</td><td>11</td><td>10</td><td>9</td><td>8</td><td>7</td><td>6</td><td>5</td><td>4</td><td>3</td><td>2</td><td>1</td><td>0</td></tr><tr><td colspan="15">ADDRx_RX[15:1]</td><td>-</td></tr></table>

<table><tr><td>位</td><td>名称</td><td>访问</td><td>描述</td><td>复位值</td></tr><tr><td>[15:1]</td><td>ADDRx_RX[15:1]</td><td>RW</td><td>待接收数据缓冲区起始地址（OUT或SETUP事务中）。</td><td>0</td></tr><tr><td>0</td><td>-</td><td>RZ</td><td>缓冲区的地址必须按2字节对齐，所以此位必须为0。</td><td>0</td></tr></table>

### 21.3.10 端点接收数据字节数寄存器 $\pmb { \times }$ （USBD_COUNTx_RX）（x=0/1/2/3/4/5/6/7）

偏移地址：[USBD_BTABLE] $+ \times + 1 6 + 1 2$

<table><tr><td>15</td><td>14</td><td>13</td><td>12</td><td>11</td><td>10</td><td>9</td><td>8</td><td>7</td><td>6</td><td>5</td><td>4</td><td>3</td><td>2</td><td>1</td><td>0</td></tr><tr><td>BSIZE</td><td colspan="5">NUM_BLOCK[4:0]</td><td colspan="10">COUNTx_RX[9:0]</td></tr></table>

<table><tr><td>位</td><td>名称</td><td>访问</td><td>描述</td><td>复位值</td></tr><tr><td>15</td><td>BL_SIZE</td><td>RW</td><td>存储区块大小:0:块大小2字节,配合NUM_BLOCK使用,可分配接收缓冲区范围2-62字节;1:块大小32字节,配合NUM_BLOCK使用,可分配接收缓冲区范围32-512字节。</td><td>0</td></tr><tr><td>[14:10]</td><td>NUM_BLOCK[4:0]</td><td>RW</td><td>存储区块数目。</td><td>0</td></tr><tr><td>[9:0]</td><td>COUNTx_RX[9:0]</td><td>RO</td><td>端点实际接收数据长度字节数(OUT或SETUP事务中)。</td><td>X</td></tr></table>

注：双缓冲区和同步IN端点有2个USBD_ADDRx_RX寄存器和2个USB_COUNTx_RX寄存器：分别为USBD_ADDRx_RX_1和USBD_ADDRx_RX_0，USB_COUNTx_RX_1和 USB_COUNTx_RX_0，内容如下：

USBD_ADDRx_TX映射为USBD_ADDRx_RX_0

USBD_ADDRx_RX映射为 USBD_ADDRx_RX_1

USBD_COUNTx_TX映射为 USB_COUNTx_RX_0

USBD_COUNTx_RX映射为 USB_COUNTx_RX_1

USBD_COUNTx_RX 寄存器的高 6 位定义了接收分组缓冲区的大小，以便 USBD 模块可以检测缓冲区

的溢出边界。缓冲区的大小可以依据设备枚举过程中的端点描述符中参数 maxPacketSize 表述。

表20-8 缓冲区大小定义  

<table><tr><td rowspan="2">NUM_BLOCK[4:0]</td><td colspan="2">接收缓冲区限制大小</td></tr><tr><td>BFSIZE = 0</td><td>BFSIZE = 1</td></tr><tr><td>00000</td><td>不允许使用</td><td>32字节</td></tr><tr><td>00001</td><td>2字节</td><td>64字节</td></tr><tr><td>00010</td><td>4字节</td><td>96字节</td></tr><tr><td>00011</td><td>6字节</td><td>128字节</td></tr><tr><td>...</td><td>...</td><td>...</td></tr><tr><td>01111</td><td>30字节</td><td>512字节</td></tr><tr><td>10000</td><td>32字节</td><td>保留</td></tr><tr><td>...</td><td>...</td><td>...</td></tr><tr><td>11110</td><td>60字节</td><td>保留</td></tr><tr><td>11111</td><td>62字节</td><td>保留</td></tr></table>

# 第 22 章 USB 高速主机/设备控制器（USBHS）

本章模块描述适用于CH32F2x、CH32V2x和CH32V3x微控制器系列部分产品。

## 22.1 USB 控制器简介

内嵌 USB2.0 控制器和 USB-PHY，具有主机控制器和 USB 设备控制器双重角色。当作为主机控制器时，它可支持低速、全速和高速的 USB 设备/HUB。当作为设备控制器时，可以灵活设置为低速、全速或高速模式以适应各种应用。

USB控制器特性如下：

支持 USB Host 主机功能和 USB Device 设备功能。  
主机模式下支持下行端口连接高速/全速 HUB。  
$\bullet$ 设备模式下支持 USB2.0 高速 480Mbps、全速 12Mbps 或低速 1.5Mbps。  
$\bullet$ 支持 USB 控制传输、批量传输、中断传输和同步/实时传输。  
$\bullet$ 支持 DMA 直接访问各端点缓冲区的数据。  
$\bullet$ 支持挂起，唤醒/远程唤醒。  
端点0支持最大64字节的数据包，除设备端点 0 外，其他端点均支持最大 1024字节的数据包，且均支持双缓冲。

## 22.2 寄存器描述

USB 相关寄存器分为 3 个部分，部分寄存器是在主机和设备模式下进行复用的。

USB 全局寄存器  
$\bullet$ USB 设备控制寄存器  
USB主机控制寄存器

### 22.2.1 全局寄存器描述

表 22-1 USBHS 相关寄存器列表  

<table><tr><td>名称</td><td>访问地址</td><td>描述</td><td>复位值</td></tr><tr><td>R8_USB_CTRL</td><td>0x40023400</td><td>USB 控制寄存器</td><td>0x06</td></tr><tr><td>R8_USB_INT_EN</td><td>0x40023402</td><td>USB 中断使能寄存器</td><td>0</td></tr><tr><td>R8_USB_DEV_AD</td><td>0x40023403</td><td>USB 设备地址寄存器</td><td>0</td></tr><tr><td>R16_USB_FRAME_NO</td><td>0x40023404</td><td>USB 帧号寄存器</td><td>0</td></tr><tr><td>R8_USB_SUSPEND</td><td>0x40023406</td><td>USB 挂起控制寄存器</td><td>0</td></tr><tr><td>R8_USB_SPPED_TYPE</td><td>0x40023408</td><td>USB 当前速度类型寄存器</td><td>0</td></tr><tr><td>R8_USB_MIS_ST</td><td>0x40023409</td><td>USB 杂项状态寄存器</td><td>xx10_1000b</td></tr><tr><td>R8_USB_INT_FG</td><td>0x4002340A</td><td>USB 中断标志寄存器</td><td>0</td></tr><tr><td>R8_USB_INT_ST</td><td>0x4002340B</td><td>USB 中断状态寄存器</td><td>00xx_xxxxxb</td></tr><tr><td>R16_USB_RX_LEN</td><td>0x4002340C</td><td>USB 接收长度寄存器</td><td>xx</td></tr></table>

22.2.1.1 USB 控制寄存器（R8_USB_CTRL）  

<table><tr><td>位</td><td>名称</td><td>访问</td><td>描述</td><td>复位值</td></tr><tr><td>7</td><td>RB_UC_HOST_MODE</td><td>RW</td><td>USB工作模式选择位:1:主机模式(HOST);0:设备模式(DEVICE)。</td><td>0</td></tr><tr><td>[6:5]</td><td>RB_UC_SPEED_TYPE</td><td>RW</td><td>USB总线信号传输速率选择位:</td><td>00b</td></tr><tr><td></td><td></td><td></td><td>00:全速;01:高速;10:低速。</td><td></td></tr><tr><td>4</td><td>RB_UC_DEVPU_EN</td><td>RW</td><td>设备模式下,USB设备使能和内部上拉电阻控制位:1:使能USB设备传输并且启用内部上拉电阻;0:不启用。</td><td>0</td></tr><tr><td>3</td><td>RB_UC_INTBUSY</td><td>RW</td><td>USB传输完成中断标志未清零前自动暂停使能位:1:在中断标志UIF_TRANSFER未清零前自动暂停,设备模式下自动应答忙NAK,主机模式下自动暂停后续传输;0:不暂停。</td><td>0</td></tr><tr><td>2</td><td>RB_UC_RST_SIE</td><td>RW</td><td>USB协议处理器软件复位控制位:1:强制复位USB协议处理器(SIE),需要软件清零;0:不复位。该位清除后,PB6/PB7自动切换为USBIO模式</td><td>1</td></tr><tr><td>1</td><td>RB_UC_CLR_ALL</td><td>RW</td><td>USB的FIFO和中断标志清零:1:清空USB中断标志和FIFO,需要软件清零;0:不清空。</td><td>1</td></tr><tr><td>0</td><td>RB_UC_DMA_EN</td><td>RW</td><td>使能USB的DMA,正常传输模式下该位必须设置为1:1:使能DMA功能和DMA中断;0:关闭DMA。</td><td>0</td></tr></table>

22.2.1.2 USB 中断使能寄存器（R8_USB_INT_EN）  

<table><tr><td>位</td><td>名称</td><td>访问</td><td>描述</td><td>复位值</td></tr><tr><td>7</td><td>RB_UID_DEV_NAK</td><td>RW</td><td>USB设备模式,接收到NAK中断:1:使能相应中断;0:禁止相应中断。</td><td>0</td></tr><tr><td>6</td><td>RB_UID_ISO_ACT</td><td>RW</td><td>同步传输开始发送/接收数据中断:1:使能相应中断;0:禁止相应中断。</td><td>0</td></tr><tr><td>5</td><td>RB_UID_setup_ACT</td><td>RW</td><td>SETUP事务完成中断:1:使能相应中断;0:禁止相应中断。</td><td>0</td></tr><tr><td>4</td><td>RB_UID_FIFO_0V</td><td>RW</td><td>FIFO0溢出中断:1:使能中断;0:禁止中断。</td><td>0</td></tr><tr><td>3</td><td>RB_UID_SOF_ACT</td><td>RW</td><td>USB主机模式,SOF定时中断:1:使能中断;0:禁止中断。USB设备模式,使能后接收SOF包产生传输完成中断</td><td>0</td></tr><tr><td>2</td><td>RB_UID_SUSPEND</td><td>RW</td><td>USB总线挂起或唤醒事件中断:1:使能中断;0:禁止中断。</td><td>0</td></tr><tr><td>1</td><td>RB_UID_TRANSFER</td><td>RW</td><td>USB传输(不包括SETUP事务)完成中断:1:使能中断;0:禁止中断。</td><td>0</td></tr><tr><td rowspan="2">0</td><td>RB_UID_DETECT</td><td>RW</td><td>USB主机模式,USB设备连接或断开事件中断:1:使能中断;0:禁止中断。</td><td>0</td></tr><tr><td>RB_UID_BUS_RST</td><td>RW</td><td>USB设备模式,USB总线复位事件中断:1:使能中断;0:禁止中断。</td><td>0</td></tr></table>

#### 22.2.1.3 USB 设备地址寄存器（R16_USB_DEV_AD）

<table><tr><td>位</td><td>名称</td><td>访问</td><td>描述</td><td>复位值</td></tr><tr><td>7</td><td>Reserved</td><td>R0</td><td>保留。</td><td>0</td></tr><tr><td>[6:0]</td><td>RB_MASK_USB_ADDR</td><td>RW</td><td>主机模式下是当前操作时的USB设备的地址或HUB地址;设备模式:该USB自身地址。</td><td>0</td></tr></table>

#### 22.2.1.4 USB 帧号寄存器（R16_USB_FRAME_NO）

<table><tr><td>位</td><td>名称</td><td>访问</td><td>描述</td><td>复位值</td></tr><tr><td>[15:0]</td><td>USB_FRAME_NO</td><td>R0</td><td>帧号,主机模式下表示即将发送的 SOF包的帧号,设备模式下表示当前接收到的 SOF 包的帧号。其中低 11 位为有效帧号,高 3 位为高速模式的微帧号。</td><td>0</td></tr></table>

注：USB_FRAME_N0是16为寄存器，其中低11位表示SOF包帧号，高3为表示当前属于第几个微帧，可在操作高速HUB下进行中断、同步/实时传输时使用。

#### 22.2.1.5 USB 挂起寄存器（R8_USB_SUSPEND）

<table><tr><td>位</td><td>名称</td><td>访问</td><td>描述</td><td>复位值</td></tr><tr><td>[7:6]</td><td>Reserved</td><td>R0</td><td>保留。</td><td>0</td></tr><tr><td>[5:4]</td><td>RB_USB_LINESTATE</td><td>R0</td><td>PHY的Linestate信号</td><td>X</td></tr><tr><td>3</td><td>Reserved</td><td>R0</td><td>保留。</td><td>0</td></tr><tr><td>2</td><td>RB_USB_WAKEUP_ST</td><td>R0</td><td>在挂起状态下,如果接收到主机的唤醒信号,该位置1,直到退出挂起状态。</td><td>X</td></tr><tr><td>[1:0]</td><td>RB_USB_SYS_MOD</td><td>RW</td><td>主机模式下的测试模式</td><td>0</td></tr></table>

注：需要远程唤醒时，将RB_UH_REMOTE_WKUP位拉高再拉低即可。

#### 22.2.1.6 USB 速度类型寄存器（R8_USB_SPEED_TYPE）

<table><tr><td>位</td><td>名称</td><td>访问</td><td>描述</td><td>复位值</td></tr><tr><td>[7:2]</td><td>Reserved</td><td>R0</td><td>保留。</td><td>0</td></tr><tr><td>[1:0]</td><td>RB_USB_SPEED_TYPE</td><td>R0</td><td>在主机模式下,表示当前连接的设备速度类型,在设备模式下,表示当前设备的速度类型;00:全速;01:高速;10:低速。</td><td>00b</td></tr></table>

注：区别于R8_USB_CTRL寄存器中的RB_UC_SPEED_TYPE，RB_UC_SPEED_TYPE表示期望处于的最高速度，假设在设备模式下，设置RB_UC_SPEED_TYPE为高速，当该设备连接在一个全速主机下，则实际的速度类型就是全速，通过查询R8_USB_SPEED_TYPE寄存器可以获知。在主机模式下，设置RB_UC_SPEED_TYPE为高速，当连接一个全速设备时，则实际通讯速度就是全速，通过查询

R8_USB_SPEED_TYPE寄存器可以获知。

22.2.1.7 USB 杂项状态寄存器（R8_USB_MIS_ST）  

<table><tr><td>位</td><td>名称</td><td>访问</td><td>描述</td><td>复位值</td></tr><tr><td>7</td><td>RB_UMS_SOF_PRES</td><td>RO</td><td>USB主机模式下SOF包预示状态位:1:将要发送SOF包,此时如有其它USB数据包将被自动延后;0:无SOF包发送。</td><td>X</td></tr><tr><td>6</td><td>RB_UMS_SOF_ACT</td><td>RO</td><td>USB主机模式下SOF包传输状态位:1:正在发出SOF包;0:发送完成或空闲。</td><td>X</td></tr><tr><td>5</td><td>RB_UMS_SIE-Free</td><td>RO</td><td>USB协议处理器的空闲状态位:1:协议器空闲;0:忙,正在进行USB传输。</td><td>1</td></tr><tr><td>4</td><td>RB_UMS_R_FIFO_RDY</td><td>RO</td><td>USB接收FIFO数据就绪状态位:1:接收FIFO非空;0:接收FIFO为空。</td><td>0</td></tr><tr><td>3</td><td>RB_UMS_BUS_RST</td><td>RO</td><td>USB总线复位状态位:1:当前USB总线处于复位态;0:当前USB总线处于非复位态。</td><td>X</td></tr><tr><td>2</td><td>RB_UMS_SUSPEND</td><td>RO</td><td>USB挂起状态位:1:USB总线处于挂起态,有一段时间没有USB活动;0:USB总线处于非挂起态。</td><td>0</td></tr><tr><td>1</td><td>RB_UMS_DEV_ATTACH</td><td>RO</td><td>USB主机模式下端口的USB设备连接状态位:1:端口已经连接USB设备;0:端口没有USB设备连接。</td><td>0</td></tr><tr><td>0</td><td>RB_UMS_SPLIT_CAN</td><td>RO</td><td>USB主机模式下,SPLIT包发送允许位:1:允许发送SPLIT包;0:禁止发送</td><td>0</td></tr></table>

22.2.1.8 USB 中断标志寄存器（R8_USB_INT_FG）  

<table><tr><td>位</td><td>名称</td><td>访问</td><td>描述</td><td>复位值</td></tr><tr><td>7</td><td>Reserved</td><td>R0</td><td>保留</td><td>0</td></tr><tr><td>6</td><td>RB_UID_ISO_ACT</td><td>R0</td><td>同步传输开始发送/接收数据中断标志位,写1清零:1:开始发送/接收数据触发;0:无事件。注:对于接收如果接收CRC16错误,将不会产生UID_TRANSFER,否则在事务完成后依然会产生UID_TRANSFER</td><td>0</td></tr><tr><td>5</td><td>RB_UID_setup_ACT</td><td>R0</td><td>SETUP事务完成中断标志位,写1清零:1:SETUP事务完成;0:无事件。</td><td>1</td></tr><tr><td>4</td><td>RB_UID_FIFO0_0V</td><td>RW</td><td>USB FIFO 溢出中断标志位,写1清零:1: FIFO 溢出触发; 0:无事件。</td><td>0</td></tr><tr><td>3</td><td>RB_UID_HST_SOF</td><td>RW</td><td>USB 主机模式下 SOF 定时中断标志位,写1清零:1: SOF 包传输完成触发;0:无事件。</td><td>0</td></tr><tr><td>2</td><td>RB_UID_SUSPEND</td><td>RW</td><td>USB 总线挂起或唤醒事件中断标志位,写1清零:1: USB 挂起事件或唤醒事件触发;0:无事件。</td><td>0</td></tr><tr><td>1</td><td>RB_UID_TRANSFER</td><td>RW</td><td>USB 传输完成中断标志位,写1清零:1: 一个 USB 传输完成触发;0:无事件。</td><td>0</td></tr><tr><td rowspan="2">0</td><td>RB_UID_DETECT</td><td>RW</td><td>USB 主机模式下 USB 设备连接或断开事件中断标志位,写1清零:1: 检测到 USB 设备连接或断开触发;0:无事件。</td><td>0</td></tr><tr><td>RB_UID_BUS_RST</td><td>RW</td><td>USB 设备模式下 USB 总线复位事件中断标志位,写1清零:1: USB 总线复位事件触发;0:无事件。</td><td>0</td></tr></table>

22.2.1.9 USB 中断状态寄存器（R8_USB_INT_ST）  

<table><tr><td>位</td><td>名称</td><td>访问</td><td>描述</td><td>复位值</td></tr><tr><td>7</td><td>RB_UID_IS_NAK</td><td>R0</td><td>USB 设备模式下,NAK 响应状态位,同RB_U_IS_NAK:1:当前USB传输过程中回应NAK;0:无NAK响应。</td><td>0</td></tr><tr><td>6</td><td>RB_UID_TOG_OK</td><td>R0</td><td>USB事务接收完成后,接收到的数据包的 Toggle 与设置的期望值匹配状态位:1:toggle 匹配;0:toggle 不匹配。</td><td>0</td></tr><tr><td>[5:4]</td><td>MASK_UID_TOKEN</td><td>R0</td><td>设备模式下,当前USB传输事务的令牌PID标识。</td><td>X</td></tr><tr><td rowspan="2">[3:0]</td><td>MASK_UID_ENDP</td><td>R0</td><td>设备模式下,当前USB传输事务的端点号。</td><td>X</td></tr><tr><td>MASK_UID_H_RES</td><td>R0</td><td>主机模式下,当前USB传输事务的应答PID标识,0000 表示设备无应答或超时;其它值表示应答PID。</td><td>X</td></tr></table>

注：MASK_UIS_TOKEN用于USB设备模式下标识当前USB传输事务的令牌PID：O0表示OUT包；01表示 SOF包；10表示IN包；11表示SETUP包。

MASK_UIS_H_RES仅在主机模式下有效。在主机模式下，若主机发送OUT/SETUP令牌包时，则该是握手包 ACK/NAK/STALL， 或是设备无应答/超时。 若主机发送 IN 令牌包， 则该 PID 是数据包的PID（DATAO/DATA1）或握手包PID。

#### 22.2.1.10 USB 接收长度寄存器（R16_USB_RX_LEN）

<table><tr><td>位</td><td>名称</td><td>访问</td><td>描述</td><td>复位值</td></tr><tr><td>[15:0]</td><td>R16_USB_RX_LEN</td><td>R0</td><td>当前USB端点接收的数据字节数</td><td>X</td></tr></table>

### 22.2.2 设备寄存器描述

USBHS模块在USB设备模式下，提供了端点 0-15共 16组双向端点，除端点 0 之外的所有端点的最大数据包长度都是 1024 字节，端点 0 的最大数据包长度为 64 字节。

端点0是默认端点，支持控制传输，发送和接收共用一个 64字节数据缓冲区。  
端点1-15各自包括一个发送端点IN和一个接收端点 OUT，发送和接收各有一个独立的数据缓冲区，支持批量传输、中断传输和实时/同步传输。  
端点 0 具有独立的 DMA 地址，收发共用，端点 1~15 的发送和接收各有一个 DMA 地址。通过入R32_UEPn_BUF_MOD 寄存器可以设置数据缓冲区的模式为双缓冲或单缓冲。若使用双缓冲区模式，该端点只能使用单方向传输。  
每组端点都具有收发控制寄存器 R8_UEPn_TX_CTRL、R8_UEPn_RX_CTRL 和发送长度寄存器R16_UEPn_T_LEN 和 R32_UEPn_*_DMA（ $\scriptstyle \mathbf { n = 0 } ^ { \sim } 1 5 ,$ ），用于配置该端点的同步触发位、对 OUT 事务和IN 事务的响应以及发送数据的长度等。

作为 USB 设备所必要的 USB 总线上拉电阻可以由软件随时设置是否启用，当 USB 控制寄存器R8_USB_CTRL 中的 RB_UC_DEV_PU_EN 置 1 时，控制器根据 RB_UC_SPEED_TYPE 的速度设置，在内部为USB总线的DP/DM引脚连接上拉电阻，并启用 USB设备功能。

当检测到USB总线复位、USB总线挂起或唤醒事件，或当 USB成功处理完数据发送或数据接收后，USB协议处理器都将设置相应的中断标志，如果中断使能打开，还会产生相应的中断请求。应用程序可以直接查询或在 USB 中断服务程序中查询并分析中断标志寄存器 R8_USB_INT_FG，根据RB_UIF_BUS_RST 和 RB_UIF_SUSPEND 进行相应的处理；并且，如果 RB_UIF_TRANSFER 有效，那么还需要继续分析 USB 中断状态寄存器 R8_USB_INT_ST，根据当前端点号 MASK_UIS_ENDP 和当前事务令牌PID 标识 MASK_UIS_TOKEN 进行相应的处理。如果事先设定了各个端点的 OUT 事务的同步触发位RB_UEP_R_TOG，那么可以通过 RB_U_TOG_OK 或 RB_UIS_TOG_OK 判断当前所接收到的数据包的同步触发位是否与该端点的同步触发位匹配，如果数据同步，则数据有效；如果数据不同步，则数据应该被丢弃。每次处理完USB发送或接收中断后，都应该正确修改相应端点的同步触发位，用于下次所发送的数据包或下次所接收的数据包是否同步检测；另外，设置 RB_UEP_T_TOG_AUTO 或 RB_UEP_R_TOG_AUTO可以实现在发送成功或接收成功后自动修改相应的同步触发位（翻转或自减）。

各个端点准备发送的数据在各自的缓冲区中，准备发送的数据长度是独立设定在 R16_UEPn_T_LEN中；各个端点接收到的数据在各自的缓冲区中，但是接收到的数据长度都在 USB 接收长度寄存器R16_USB_RX_LEN中，可以在USB接收中断时根据当前端点号区分。

表 22-2 设备相关寄存器列表  

<table><tr><td>名称</td><td>访问地址</td><td>描述</td><td>复位值</td></tr><tr><td>R32_UEP_CONFIG</td><td>0x40023410</td><td>端点使能配置寄存器</td><td>00000000h</td></tr><tr><td>R32_UEP_TYPE</td><td>0x40023414</td><td>端点类型配置寄存器</td><td>00000000h</td></tr><tr><td>R32_UEP_buf_MOD</td><td>0x40023418</td><td>端点缓冲区模式寄存器</td><td>00000000h</td></tr><tr><td>R32_UEPO_DMA</td><td>0x4002341C</td><td>端点0缓冲区的起始地址</td><td>xxxxh</td></tr><tr><td>R32_UEP1_RX_DMA</td><td>0x40023420</td><td>端点1接收缓冲区的起始地址</td><td>xxxxh</td></tr><tr><td>R32_UEP2_RX_DMA</td><td>0x40023424</td><td>端点2接收缓冲区的起始地址</td><td>xxxxh</td></tr><tr><td>R32_UEP3_RX_DMA</td><td>0x40023428</td><td>端点3接收缓冲区的起始地址</td><td>xxxxh</td></tr><tr><td>R32_UEP4_RX_DMA</td><td>0x4002342C</td><td>端点4接收缓冲区的起始地址</td><td>xxxxh</td></tr><tr><td>R32_UEP5_RX_DMA</td><td>0x40023430</td><td>端点5接收缓冲区的起始地址</td><td>xxxxh</td></tr><tr><td>R32_UEP6_RX_DMA</td><td>0x40023434</td><td>端点6接收缓冲区的起始地址</td><td>xxxxh</td></tr><tr><td>R32_UEP7_RX_DMA</td><td>0x40023438</td><td>端点7接收缓冲区的起始地址</td><td>xxxxh</td></tr><tr><td>R32_UEP8_RX_DMA</td><td>0x4002343C</td><td>端点8接收缓冲区的起始地址</td><td>xxxxh</td></tr><tr><td>R32_UEP9_RX_DMA</td><td>0x40023440</td><td>端点9接收缓冲区的起始地址</td><td>xxxxh</td></tr><tr><td>R32_UEP10_RX_DMA</td><td>0x40023444</td><td>端点10接收缓冲区的起始地址</td><td>xxxxh</td></tr><tr><td>R32_UEP11_RX_DMA</td><td>0x40023448</td><td>端点11接收缓冲区的起始地址</td><td>xxxxh</td></tr><tr><td>R32_UEP12_RX_DMA</td><td>0x4002344C</td><td>端点12接收缓冲区的起始地址</td><td>xxxxh</td></tr><tr><td>R32_UEP13_RX_DMA</td><td>0x40023450</td><td>端点13接收缓冲区的起始地址</td><td>xxxxh</td></tr><tr><td>R32_UEP14_RX_DMA</td><td>0x40023454</td><td>端点14接收缓冲区的起始地址</td><td>xxxxh</td></tr><tr><td>R32_UEP15_RX_DMA</td><td>0x40023458</td><td>端点15接收缓冲区的起始地址</td><td>xxxxh</td></tr><tr><td>R32_UEP1_TX_DMA</td><td>0x4002345C</td><td>端点1发送缓冲区的起始地址</td><td>xxxxh</td></tr><tr><td>R32_UEP2_TX_DMA</td><td>0x40023460</td><td>端点2发送缓冲区的起始地址</td><td>xxxxh</td></tr><tr><td>R32_UEP3_TX_DMA</td><td>0x40023464</td><td>端点3发送缓冲区的起始地址</td><td>xxxxh</td></tr><tr><td>R32_UEP4_TX_DMA</td><td>0x40023468</td><td>端点4发送缓冲区的起始地址</td><td>xxxxh</td></tr><tr><td>R32_UEP5_TX_DMA</td><td>0x4002346C</td><td>端点5发送缓冲区的起始地址</td><td>xxxxh</td></tr><tr><td>R32_UEP6_TX_DMA</td><td>0x40023470</td><td>端点6发送缓冲区的起始地址</td><td>xxxxh</td></tr><tr><td>R32_UEP7_TX_DMA</td><td>0x40023474</td><td>端点7发送缓冲区的起始地址</td><td>xxxxh</td></tr><tr><td>R32_UEP8_TX_DMA</td><td>0x40023478</td><td>端点8发送缓冲区的起始地址</td><td>xxxxh</td></tr><tr><td>R32_UEP9_TX_DMA</td><td>0x4002347C</td><td>端点9发送缓冲区的起始地址</td><td>xxxxh</td></tr><tr><td>R32_UEP10_TX_DMA</td><td>0x40023480</td><td>端点10发送缓冲区的起始地址</td><td>xxxxh</td></tr><tr><td>R32_UEP11_TX_DMA</td><td>0x40023484</td><td>端点11发送缓冲区的起始地址</td><td>xxxxh</td></tr><tr><td>R32_UEP12_TX_DMA</td><td>0x40023488</td><td>端点12发送缓冲区的起始地址</td><td>xxxxh</td></tr><tr><td>R32_UEP13_TX_DMA</td><td>0x4002348C</td><td>端点13发送缓冲区的起始地址</td><td>xxxxh</td></tr><tr><td>R32_UEP14_TX_DMA</td><td>0x40023490</td><td>端点14发送缓冲区的起始地址</td><td>xxxxh</td></tr><tr><td>R32_UEP15_TX_DMA</td><td>0x40023494</td><td>端点15发送缓冲区的起始地址</td><td>xxxxh</td></tr><tr><td>R16_UEPO_MAX_LEN</td><td>0x40023498</td><td>端点0最大长度包寄存器</td><td>xxxxh</td></tr><tr><td>R16_UEP1_MAX_LEN</td><td>0x4002349C</td><td>端点1最大长度包寄存器</td><td>xxxxh</td></tr><tr><td>R16_UEP2_MAX_LEN</td><td>0x400234A0</td><td>端点2最大长度包寄存器</td><td>xxxxh</td></tr><tr><td>R16_UEP3_MAX_LEN</td><td>0x400234A4</td><td>端点3最大长度包寄存器</td><td>xxxxh</td></tr><tr><td>R16_UEP4_MAX_LEN</td><td>0x400234A8</td><td>端点4最大长度包寄存器</td><td>xxxxh</td></tr><tr><td>R16_UEP5_MAX_LEN</td><td>0x400234AC</td><td>端点5最大长度包寄存器</td><td>xxxxh</td></tr><tr><td>R16_UEP6_MAX_LEN</td><td>0x400234B0</td><td>端点6最大长度包寄存器</td><td>xxxxh</td></tr><tr><td>R16_UEP7_MAX_LEN</td><td>0x400234B4</td><td>端点7最大长度包寄存器</td><td>xxxxh</td></tr><tr><td>R16_UEP8_MAX_LEN</td><td>0x400234B8</td><td>端点8最大长度包寄存器</td><td>xxxxh</td></tr><tr><td>R16_UEP9_MAX_LEN</td><td>0x400234BC</td><td>端点9最大长度包寄存器</td><td>xxxxh</td></tr><tr><td>R16_UEP10_MAX_LEN</td><td>0x400234C0</td><td>端点10最大长度包寄存器</td><td>xxxxh</td></tr><tr><td>R16_UEP11_MAX_LEN</td><td>0x400234C4</td><td>端点11最大长度包寄存器</td><td>xxxxh</td></tr><tr><td>R16_UEP12_MAX_LEN</td><td>0x400234C8</td><td>端点12最大长度包寄存器</td><td>xxxxh</td></tr><tr><td>R16_UEP13_MAX_LEN</td><td>0x400234CC</td><td>端点13最大长度包寄存器</td><td>xxxxh</td></tr><tr><td>R16_UEP14_MAX_LEN</td><td>0x400234D0</td><td>端点14最大长度包寄存器</td><td>xxxxh</td></tr><tr><td>R16_UEP15_MAX_LEN</td><td>0x400234D4</td><td>端点15最大长度包寄存器</td><td>xxxxh</td></tr><tr><td>R16_UEPO_T_LEN</td><td>0x400234D8</td><td>端点0发送长度寄存器</td><td>xxxxh</td></tr><tr><td>R8_UEPO_TX_CTRL</td><td>0x400234DA</td><td>端点0发送控制寄存器</td><td>00h</td></tr><tr><td>R8_UEPO_RX_CTRL</td><td>0x400234DB</td><td>端点0接收控制寄存器</td><td>00h</td></tr><tr><td>R16_UEP1_T_LEN</td><td>0x400234DC</td><td>端点1发送长度寄存器</td><td>xxxxh</td></tr><tr><td>R8_UEP1_TX_CTRL</td><td>0x400234DE</td><td>端点1发送控制寄存器</td><td>00h</td></tr><tr><td>R8_UEP1_RX_CTRL</td><td>0x400234DF</td><td>端点1接收控制寄存器</td><td>00h</td></tr><tr><td>R16_UEP2_T_LEN</td><td>0x400234E0</td><td>端点2发送长度寄存器</td><td>xxxxh</td></tr><tr><td>R8_UEP2_TX_CTRL</td><td>0x400234E2</td><td>端点2发送控制寄存器</td><td>00h</td></tr><tr><td>R8_UEP2_RX_CTRL</td><td>0x400234E3</td><td>端点2接收控制寄存器</td><td>00h</td></tr><tr><td>R16_UEP3_T_LEN</td><td>0x400234E4</td><td>端点3发送长度寄存器</td><td>xxxxh</td></tr><tr><td>R8_UEP3_TX_CTRL</td><td>0x400234E6</td><td>端点3发送控制寄存器</td><td>00h</td></tr><tr><td>R8_UEP3_RX_CTRL</td><td>0x400234E7</td><td>端点3接收控制寄存器</td><td>00h</td></tr><tr><td>R16_UEP4_T_LEN</td><td>0x400234E8</td><td>端点4发送长度寄存器</td><td>xxxxh</td></tr><tr><td>R8_UEP4_TX_CTRL</td><td>0x400234EA</td><td>端点4发送控制寄存器</td><td>00h</td></tr><tr><td>R8_UEP4_RX_CTRL</td><td>0x400234EB</td><td>端点4接收控制寄存器</td><td>00h</td></tr><tr><td>R16_UEP5_T_LEN</td><td>0x400234EC</td><td>端点5发送长度寄存器</td><td>xxxxh</td></tr><tr><td>R8_UEP5_TX_CTRL</td><td>0x400234EE</td><td>端点5发送控制寄存器</td><td>00h</td></tr><tr><td>R8_UEP5_RX_CTRL</td><td>0x400234EF</td><td>端点5接收控制寄存器</td><td>00h</td></tr><tr><td>R16_UEP6_T_LEN</td><td>0x400234F0</td><td>端点6发送长度寄存器</td><td>xxxxh</td></tr><tr><td>R8_UEP6_TX_CTRL</td><td>0x400234F2</td><td>端点6发送控制寄存器</td><td>00h</td></tr><tr><td>R8_UEP6_RX_CTRL</td><td>0x400234F3</td><td>端点6接收控制寄存器</td><td>00h</td></tr><tr><td>R16_UEP7_T_LEN</td><td>0x400234F4</td><td>端点7发送长度寄存器</td><td>xxxxh</td></tr><tr><td>R8_UEP7_TX_CTRL</td><td>0x400234F6</td><td>端点7发送控制寄存器</td><td>00h</td></tr><tr><td>R8_UEP7_RX_CTRL</td><td>0x400234F7</td><td>端点7接收控制寄存器</td><td>00h</td></tr><tr><td>R16_UEP8_T_LEN</td><td>0x400234F8</td><td>端点8发送长度寄存器</td><td>xxxxh</td></tr><tr><td>R8_UEP8_TX_CTRL</td><td>0x400234FA</td><td>端点8发送控制寄存器</td><td>00h</td></tr><tr><td>R8_UEP8_RX_CTRL</td><td>0x400234FB</td><td>端点8接收控制寄存器</td><td>00h</td></tr><tr><td>R16_UEP9_T_LEN</td><td>0x400234FC</td><td>端点9发送长度寄存器</td><td>xxxxh</td></tr><tr><td>R8_UEP9_TX_CTRL</td><td>0x400234FE</td><td>端点9发送控制寄存器</td><td>00h</td></tr><tr><td>R8_UEP9_RX_CTRL</td><td>0x400234FF</td><td>端点9接收控制寄存器</td><td>00h</td></tr><tr><td>R16_UEP10_T_LEN</td><td>0x40023500</td><td>端点10发送长度寄存器</td><td>xxxxh</td></tr><tr><td>R8_UEP10_TX_CTRL</td><td>0x40023502</td><td>端点10发送控制寄存器</td><td>00h</td></tr><tr><td>R8_UEP10_RX_CTRL</td><td>0x40023503</td><td>端点10接收控制寄存器</td><td>00h</td></tr><tr><td>R16_UEP11_T_LEN</td><td>0x40023504</td><td>端点11发送长度寄存器</td><td>xxxxh</td></tr><tr><td>R8_UEP11_TX_CTRL</td><td>0x40023506</td><td>端点11发送控制寄存器</td><td>00h</td></tr><tr><td>R8_UEP11_RX_CTRL</td><td>0x40023507</td><td>端点11接收控制寄存器</td><td>00h</td></tr><tr><td>R16_UEP12_T_LEN</td><td>0x40023508</td><td>端点12发送长度寄存器</td><td>xxxxh</td></tr><tr><td>R8_UEP12_TX_CTRL</td><td>0x4002350A</td><td>端点12发送控制寄存器</td><td>00h</td></tr><tr><td>R8_UEP12_RX_CTRL</td><td>0x4002350B</td><td>端点12接收控制寄存器</td><td>00h</td></tr><tr><td>R16_UEP13_T_LEN</td><td>0x4002350C</td><td>端点13发送长度寄存器</td><td>xxxxh</td></tr><tr><td>R8_UEP13_TX_CTRL</td><td>0x4002350E</td><td>端点13发送控制寄存器</td><td>00h</td></tr><tr><td>R8_UEP13_RX_CTRL</td><td>0x4002350F</td><td>端点13接收控制寄存器</td><td>00h</td></tr><tr><td>R16_UEP14_T_LEN</td><td>0x40023510</td><td>端点14发送长度寄存器</td><td>xxxxh</td></tr><tr><td>R8_UEP14_TX_CTRL</td><td>0x40023512</td><td>端点14发送控制寄存器</td><td>00h</td></tr><tr><td>R8_UEP14_RX_CTRL</td><td>0x40023513</td><td>端点14接收控制寄存器</td><td>00h</td></tr><tr><td>R16_UEP15_T_LEN</td><td>0x40023514</td><td>端点15发送长度寄存器</td><td>xxxxh</td></tr><tr><td>R8_UEP15_TX_CTRL</td><td>0x40023516</td><td>端点15发送控制寄存器</td><td>00h</td></tr><tr><td>R8_UEP15_RX_CTRL</td><td>0x40023517</td><td>端点15接收控制寄存器</td><td>00h</td></tr></table>

22.2.2.1 USB 端点配置寄存器（R32_UEP_CONFIG）  

<table><tr><td>位</td><td>名称</td><td>访问</td><td>描述</td><td>复位值</td></tr><tr><td>[31:17]</td><td>RB_UEP_R_EN</td><td>RW</td><td>端点1-15接收使能</td><td>0</td></tr><tr><td>16</td><td>Reserved</td><td>R0</td><td>保留</td><td>0</td></tr><tr><td>[15:1]</td><td>RB_UEP_T_EN</td><td>RW</td><td>端点1-15发送使能</td><td>0</td></tr><tr><td>0</td><td>Reserved</td><td>R0</td><td>保留</td><td>0</td></tr></table>

注：端点0的收发使能信号始终有效。

22.2.2.2 USB 端点类型控制寄存器（R32_UEP_TYPE）  

<table><tr><td>位</td><td>名称</td><td>访问</td><td>描述</td><td>复位值</td></tr><tr><td>[31:17]</td><td>RB_UEP_R_TYPE</td><td>RW</td><td>端点1-15,OUT方向传输类型,1表示同步传输,</td><td>0</td></tr><tr><td>16</td><td>Reserved</td><td>R0</td><td>保留</td><td>0</td></tr><tr><td>[15:1]</td><td>RB_UEP_T_TYPE</td><td>RW</td><td>端点1-15,IN方向传输类型,1表示同步传输</td><td>0</td></tr><tr><td>0</td><td>Reserved</td><td>R0</td><td>保留</td><td>0</td></tr></table>

22.2.2.3 USB 端点缓冲区模式控制寄存器（R32_UEP_BUF_MOD）  

<table><tr><td>位</td><td>名称</td><td>访问</td><td>描述</td><td>复位值</td></tr><tr><td>[31:16]</td><td>RB_UEP_ISO BUF_MOD</td><td>RW</td><td>同步端点缓冲区模式控制位,1有效。</td><td>0</td></tr><tr><td>[15:0]</td><td>RB_UEP BUF_MOD</td><td>RW</td><td>端点缓冲区模式控制位</td><td>0</td></tr></table>

注：当RB_UEP_ISO_BUF_MOD为1时，对于同步IN端点，在接收到 SOF包后，硬件会有以下操作：将 $E P x \_ R \_ T O G$ 内容加载到 EPx_T_TOG中；将EPx_MAX_LEN值加载到 EPx_T_LEN中；将UEPn_RX_DMA值加载到UEPn_TX_DMA中。

当RB_UEP_ISO_BUF_MOD为1时，对于同步OUT端点，在接收到 SOF包后，硬件会有以下操作：将 EPx_T_TOG 内容加载到 EPx_R_TOG中；将 UEPn_TX_DMA的值加载到UEPn_RX_DMA中。

表 21-3 端点 n 缓冲区模式（ $\scriptstyle \mathbf { \bar { n } } = 1 - 1 5 )$ ）  

<table><tr><td>UEPn_RX_EN</td><td>UEPn_TX_EN</td><td>UEPnBUF_MOD</td><td>描述:以UEPn_DMA为起始地址由低向高排列</td></tr><tr><td>0</td><td>0</td><td>x</td><td>端点被禁用,未用到UEPn_*DMA缓冲区。</td></tr><tr><td>1</td><td>0</td><td>0</td><td>接收(OUT)缓冲区首地址为UEPn_RX_DMA</td></tr><tr><td>1</td><td>0</td><td>1</td><td>RB_UEPn_RX_TOG[0]=0,使用缓冲区UEPn_RX_DMA
RB_UEPn_RX_TOG[0]=1,使用缓冲区UEPn_TX_DMA</td></tr><tr><td>0</td><td>1</td><td>0</td><td>发送(IN)缓冲区首地址为UEPn_TX_DMA。</td></tr><tr><td>0</td><td>1</td><td>1</td><td>RB_UEPn_RX_TOG[0]=0,使用缓冲区UEPn_TX_DMA
RB_UEPn_RX_TOG[0]=1,使用缓冲区UEPn_RX_DMA</td></tr></table>

端点 n 缓冲区起始地址（R32_UEP0_DMA）  

<table><tr><td>位</td><td>名称</td><td>访问</td><td>描述</td><td>复位值</td></tr><tr><td>[31:17]</td><td>Reserved</td><td>R0</td><td>保留</td><td>0</td></tr><tr><td>[16:0]</td><td>R32_UEPn_DMA</td><td>RW</td><td>端点0缓冲区起始地址。
低16位有效，地址必须4字节对齐。</td><td>X</td></tr></table>

USB 端点 n 发送缓冲区起始地址（R32_UEPn_TX_DMA）（n=1-15）

<table><tr><td>位</td><td>名称</td><td>访问</td><td>描述</td><td>复位值</td></tr><tr><td>[31:17]</td><td>Reserved</td><td>R0</td><td>保留</td><td>0</td></tr><tr><td>[16:0]</td><td>R32_UEPn_TX_DMA</td><td>RW</td><td>端点n发送缓冲区起始地址。低16位有效，地址必须4字节对齐。</td><td>X</td></tr></table>

USB 端点 n 接收缓冲区起始地址（R32_UEPn_RX_DMA）（n=1-15）

<table><tr><td>位</td><td>名称</td><td>访问</td><td>描述</td><td>复位值</td></tr><tr><td>[31:17]</td><td>Reserved</td><td>R0</td><td>保留</td><td>0</td></tr><tr><td>[16:0]</td><td>R32_UEPn_RX_DMA</td><td>RW</td><td>端点n接收缓冲区起始地址。低16位有效，地址必须4字节对齐。</td><td>X</td></tr></table>

22.2.2.4 端点 n 最大长度包寄存器（R16_UEPn_MAX_LEN）（n=0-15）

<table><tr><td>位</td><td>名称</td><td>访问</td><td>描述</td><td>复位值</td></tr><tr><td>[15:11]</td><td>Reserved</td><td>R0</td><td>保留</td><td>0</td></tr><tr><td>[10:0]</td><td>UEPn_MAX_LEN</td><td>RW</td><td>端点n接收数据的最大包长度。</td><td>X</td></tr></table>

注：这个最大包长度决定了端点可接收数据最大长度，超出此长度的数据会被丢弃，不会写入缓冲区。

22.2.2.5 端点 n 发送长度寄存器（R16_UEPn_T_LEN）（n=0-15）

<table><tr><td>位</td><td>名称</td><td>访问</td><td>描述</td><td>复位值</td></tr><tr><td>[10:0]</td><td>UEPn_T_LEN</td><td>RW</td><td>设置USB端点n准备发送的数据字节数,对于控制端点(0),低7位有效。</td><td>X</td></tr></table>

22.2.2.6 端点 n 发送控制寄存器（R8_UEPn_TX_CTRL）（n=0-15）

<table><tr><td>位</td><td>名称</td><td>访问</td><td>描述</td><td>复位值</td></tr><tr><td>[7:6]</td><td>Reserved</td><td>R0</td><td>保留</td><td>0</td></tr><tr><td>5</td><td>RB_UEP_T_TOG_AUTO</td><td>RW</td><td>同步触发位自动翻转使能控制位,软件可修改:1:对于非同步端点,数据发送成功后自动翻转MASK_UEP_T_TOG [0],对于同步端点,数据发送成功后MASK_UEP_T_TOG自动减10:不自动翻转,可以手动切换。注:端点0此位为保留位。</td><td>0</td></tr><tr><td>[4:3]</td><td>MASK_UEP_T_TOG</td><td>RW</td><td>USB端点n的发送器(处理IN事务)准备的同步触发位:00:发送DATA0;01:发送DATA1;10:发送DATA2;11:发送MDATA。</td><td>00b</td></tr><tr><td>2</td><td>Reserved</td><td>R0</td><td>保留</td><td>0</td></tr><tr><td>[1:0]</td><td>MASK_UEP_T_RES</td><td>RW</td><td>端点n的发送器对IN事务的响应控制:00:数据就绪并期望ACK;10:应答NAK或忙;11:应答STALL或错误。</td><td>00b</td></tr></table>

22.2.2.7 端点 n 接收控制寄存器（R8_UEPn_RX_CTRL）（n=0-15）  

<table><tr><td>位</td><td>名称</td><td>访问</td><td>描述</td><td>复位值</td></tr><tr><td>[7:6]</td><td>Reserved</td><td>R0</td><td>保留</td><td>0</td></tr><tr><td>5</td><td>RB_UEP_R_TOG_AUTO</td><td>R0</td><td>同步触发位自动翻转使能控制位:1:数据接收成功后自动翻转MASK_UEP_R_TOG[0]0:不自动翻转,可以手动切换。注:端点0此位为保留位。</td><td>0</td></tr><tr><td>[4:3]</td><td>MASK_UEP_R_TOG</td><td>RW</td><td>USB端点n的接收器(处理OUT事务)准备的同步触发位:00:期望DATA0;01:期望DATA1;10:期望DATA2;11:期望MDATA。对于实时/同步传输无效。</td><td>00b</td></tr><tr><td>2</td><td>Reserved</td><td>R0</td><td>保留</td><td>0</td></tr><tr><td>[1:0]</td><td>MASK_UEP_R_RES</td><td>RW</td><td>端点n的接收器对OUT事务的响应控制:00:数据就绪并期望ACK;10:应答NAK或忙;11:应答STALL或错误;01:应答NYET。对于实时/同步传输无效。</td><td>00b</td></tr></table>

### 22.2.3 USB 主机寄存器

在USB主机模式下，芯片提供了一组双向主机端点，包括一个发送端点 OUT和一个接收端点 IN，一个数据包的最大长度是1024字节（同步传输），支持控制传输、中断传输、批量传输和实时/同步传输。

主机端点发起的每一个USB事务，在处理结束后总是自动设置 RB_UIF_TRANSFER中断标志。应用程序可以直接查询或在USB中断服务程序中查询并分析中断标志寄存器 R8_USB_INT_FG，根据各中断标志分别进行相应的处理；并且，如果RB_UIF_TRANSFER有效，那么还需要继续分析 USB中断状态寄存器 R8_USB_INT_ST，根据当前 USB 传输事务的应答 PID 标识 MASK_UIS_H_RES 进行相应的处理。

如果事先设定了主机接收端点的 IN 事务的同步触发位（RB_UH_R_TOG），那么可以通过RB_U_TOG_OK 或 RB_UIS_TOG_OK 判断当前所接收到的数据包的同步触发位是否与主机接收端点的同步触发位匹配，如果数据同步，则数据有效；如果数据不同步，则数据应该被丢弃。每次处理完 USB发送或接收中断后，都应该正确修改相应主机端点的同步触发位，用于同步下次所发送的数据包和检测下次所接收的数据包是否同步；另外，通过设置 RB_UH_T_AUTO_TOG 和 RB_UH_R_AUTO_TOG 可以实现在发送成功或接收成功后自动翻转相应的同步触发位。

USB 主机令牌设置寄存器 R8_UH_EP_PID 用于设置被操作的目标设备的端点号和本次 USB 传输事务的令牌PID包标识。SETUP令牌和OUT令牌所对应的数据由主机发送端点提供，准备发送的数据在R16_UH_TX_DMA缓冲区中，准备发送的数据长度设置在 R16_UH_TX_LEN中；IN令牌所对应数据由目标设备返回给主机接收端点，接收到数据存放 R16_UH_RX_DMA 缓冲区中，接收到的数据长度存放在

R16_USB_RX_LEN 中。

表22-3 主机相关寄存器列表  

<table><tr><td>名称</td><td>访问地址</td><td>描述</td><td>复位值</td></tr><tr><td>R8_UHOST_CTRL</td><td>0x40023401</td><td>USB 主机控制寄存器</td><td>00h</td></tr><tr><td>R32_UH_CONFIG</td><td>0x40023410</td><td>USB 主机端点配置寄存器</td><td>00000000h</td></tr><tr><td>R32_UH_EP_TYPE</td><td>0x40023414</td><td>USB 主机端点类型寄存器</td><td>00000000h</td></tr><tr><td>R32_UH_RX_DMA</td><td>0x40023424</td><td>USB 主机接收缓冲区起始地址</td><td>16xxxxx</td></tr><tr><td>R32_UH_TX_DMA</td><td>0x40023464</td><td>USB 主机发送缓冲区起始地址</td><td>16xxxxx</td></tr><tr><td>R16_UH_RX_MAX_LEN</td><td>0x400234A0</td><td>USB 主机接收最大长度包寄存器</td><td>16xxxxx</td></tr><tr><td>R8_UH_EP_PID</td><td>0x400234E0</td><td>USB 主机令牌设置寄存器</td><td>8h00</td></tr><tr><td>R8_UH_RX_CTRL</td><td>0x400234E3</td><td>USB 主机接收端点控制寄存器</td><td>8h00</td></tr><tr><td>R16_UH_TX_LEN</td><td>0x400234E4</td><td>USB 主机发送长度寄存器</td><td>16xxxxx</td></tr><tr><td>R8_UH_TX_CTRL</td><td>0x400234E6</td><td>USB 主机发送端点控制寄存器</td><td>8h00</td></tr><tr><td>R16_UH_SPLIT_DATA</td><td>0x400234E8</td><td>USB 主机发送 SPLIT 包的数据</td><td>16xxxxx</td></tr></table>

22.2.3.1 USB 主机控制寄存器（R8_UHOST_CTRL）  

<table><tr><td>位</td><td>名称</td><td>访问</td><td>描述</td><td>复位值</td></tr><tr><td>7</td><td>RB_UH_SOF_EN</td><td>RW</td><td>自动产生SOF包使能控制位:1:主机自动发生SOF包;0:不产生SOF包。该位在从连接状态变为断开状态时由硬件自动清零</td><td>0</td></tr><tr><td>6</td><td>RB_UH_SOF-Free</td><td>RO</td><td>总线空闲</td><td>0</td></tr><tr><td>5</td><td>Reserved</td><td>RO</td><td>保留。</td><td>0</td></tr><tr><td>4</td><td>RB_UH_PHY_SUSPENDM</td><td>RW</td><td>USB-PHY的处于挂起状态,内部USB-PLL将被关闭,低有效</td><td>0</td></tr><tr><td>3</td><td>RB_UH remotework</td><td>RW</td><td>远程唤醒。</td><td>0</td></tr><tr><td>2</td><td>RB_UH_TX_BUS_RESUME</td><td>RW</td><td>主机模式下,表示主机唤醒设备,软件拉高50ns后,硬件自动发送30ms的唤醒信号。</td><td>0</td></tr><tr><td>1</td><td>RB_UH_TX_BUS_SUSPEND</td><td>RW</td><td>USB主机发送挂起信号,需由软件拉高10ms。</td><td>0</td></tr><tr><td>0</td><td>RB_UH_TX_BUS_RST</td><td>RW</td><td>USB主机发送总线复位信号,需由软件拉高10ms。</td><td>0</td></tr></table>

注：复位的时间由RB_UH_TX_BUS_RST的高电平持续时间决定（建议至少10ms，10ms后直接查询速度类型）。如果主机唤醒设备，bUH_TX_BUS_RESUME拉高之后，硬件自动发送30ms的唤醒信号（K），bUH_TX_BUS_RESUME需要手动清除，以免影响下一次主机挂起（bUH_TX_BUS_RESUME高电平至少维持50ns）。

22.2.3.2 USB 主机端点配置控制寄存器（R32_UH_CONFIG）  

<table><tr><td>位</td><td>名称</td><td>访问</td><td>描述</td><td>复位值</td></tr><tr><td>[31:19]</td><td>Reserved</td><td>R0</td><td>保留</td><td>0</td></tr><tr><td>18</td><td>RB_UH_EP_RX_EN</td><td>RW</td><td>主机接收使能</td><td>0</td></tr><tr><td>[17:4]</td><td>Reserved</td><td>R0</td><td>保留。</td><td>0</td></tr><tr><td>3</td><td>RB_UH_EP_TX_EN</td><td>RW</td><td>主机发送使能</td><td>0</td></tr><tr><td>[2:0]</td><td>Reserved</td><td>R0</td><td>保留。</td><td>0</td></tr></table>

22.2.3.3 USB 主机端点类型寄存器（R32_UH_EP_TYPE）  

<table><tr><td>位</td><td>名称</td><td>访问</td><td>描述</td><td>复位值</td></tr><tr><td>[31:19]</td><td>Reserved</td><td>R0</td><td>保留</td><td>0</td></tr><tr><td>18</td><td>RB_UH_EP_RX_TYPE</td><td>RW</td><td>主机接收端点类型,1表示同步传输</td><td>0</td></tr><tr><td>[17:4]</td><td>Reserved</td><td>R0</td><td>保留。</td><td>0</td></tr><tr><td>3</td><td>RB_UH_EP_TX_TYPE</td><td>RW</td><td>主机发送端点类型,1表示同步传输</td><td>0</td></tr><tr><td>[2:0]</td><td>Reserved</td><td>R0</td><td>保留。</td><td>0</td></tr></table>

USB 主机接收缓冲区起始地址（R32_UH_RX_DMA）  

<table><tr><td>位</td><td>名称</td><td>访问</td><td>描述</td><td>复位值</td></tr><tr><td>[31:17]</td><td>Reserved</td><td>R0</td><td>保留</td><td>0</td></tr><tr><td>[16:0]</td><td>R16_UH_RX_DMA</td><td>RW</td><td>主机端点数据接收缓冲区起始地址，最低2位固定为0（4字节对齐）</td><td>X</td></tr></table>

USB 主机发送缓冲区起始地址（R32_UH_TX_DMA）  

<table><tr><td>位</td><td>名称</td><td>访问</td><td>描述</td><td>复位值</td></tr><tr><td>[31:17]</td><td>Reserved</td><td>R0</td><td>保留</td><td>0</td></tr><tr><td>[16:0]</td><td>R16_UH_TX_DMA</td><td>RW</td><td>主机端点数据发送缓冲区起始地址（不需要4字节对齐）</td><td>X</td></tr></table>

22.2.3.4 USB 主机接收最大长度包寄存器（R16_UH_RX_MAX_LEN）  

<table><tr><td>位</td><td>名称</td><td>访问</td><td>描述</td><td>复位值</td></tr><tr><td>[15:11]</td><td>Reserved</td><td>R0</td><td>保留</td><td>0</td></tr><tr><td>[10:0]</td><td>UH_RX_MAX_LEN</td><td>RW</td><td>主机端点接收数据的最大包长度。</td><td>X</td></tr></table>

注：这个最大包大小决定了端点可接收数据最大长度，超出此长度的数据会被丢弃。

22.2.3.5 USB 主机令牌设置寄存器（R8_UH_EP_PID）  

<table><tr><td>位</td><td>名称</td><td>访问</td><td>描述</td><td>复位值</td></tr><tr><td>[7:4]</td><td>MASK_UH_TOKEN</td><td>RW</td><td>设置本次USB传输事务的令牌PID标识。</td><td>0</td></tr><tr><td>[3:0]</td><td>MASK_UH_ENDP</td><td>RW</td><td>设置本次被操作的目标设备的端点号。</td><td>0</td></tr></table>

22.2.3.6 USB 主机接收端点控制寄存器（R8_UH_RX_CTRL）  

<table><tr><td>位</td><td>名称</td><td>访问</td><td>描述</td><td>复位值</td></tr><tr><td>7</td><td>Reserved</td><td>R0</td><td>保留。</td><td>0</td></tr><tr><td>6</td><td>RB_UH_R_DATA_NO</td><td>RW</td><td>1:不期待数据包,用于主机模式下的操作高速 HUB;0:期望数据包(IN)。</td><td>0</td></tr><tr><td>5</td><td>RB_UH_R_AUTO_TOG</td><td>RW</td><td>同步触发位自动翻转使能控制位:1:对于非同步传输,数据接收成功后自动翻转相应MASK_UH_R_TOG[0];对于同步传输,数据接收成功后MASK_UH_R_TOG会自动减1。</td><td>0</td></tr><tr><td></td><td></td><td></td><td>0:不自动翻转,可以手动切换。</td><td></td></tr><tr><td>[4:3]</td><td>MASK_UH_R_TOG</td><td>RW</td><td>主机接收器(处理IN事务)期望的同步触发位,00:期望DATA0;01:期望DATA1;10:期望DATA2;11:期望MDATA。</td><td>00b</td></tr><tr><td>2</td><td>RB_UH_R_RES_NO</td><td>RW</td><td>1:无应答,用于实现非端点0的实时/同步传输。此时忽略MASK_UEP_R_RES;0:接收数据成功后发送应答。</td><td>0</td></tr><tr><td>[1:0]</td><td>MASK_UH_R_RES</td><td>RW</td><td>主机接收器对IN事务的响应控制位:00:应答ACK;对于实时/同步传输无效。</td><td>00b</td></tr></table>

#### 22.2.3.7 USB 主机发送长度寄存器（R16_UH_TX_LEN）

<table><tr><td>位</td><td>名称</td><td>访问</td><td>描述</td><td>复位值</td></tr><tr><td>[15:11]</td><td>Reserved</td><td>R0</td><td>保留</td><td>0</td></tr><tr><td>[10:0]</td><td>R16_UH_TX_LEN</td><td>RW</td><td>设置USB主机发送端点准备发送的数据字节。</td><td>X</td></tr></table>

#### 22.2.3.8 USB 主机发送端点控制寄存器（R8_UH_TX_CTRL）

<table><tr><td>位</td><td>名称</td><td>访问</td><td>描述</td><td>复位值</td></tr><tr><td>7</td><td>Reserved</td><td>R0</td><td>保留。</td><td>0</td></tr><tr><td>6</td><td>RB_UH_T_DATA_NO</td><td>RW</td><td>1:不发送数据包(PING/SPLIT);0:发送数据包(OUT/SETUP)。</td><td>0</td></tr><tr><td>5</td><td>RB_UH_T_AUTO_TOG</td><td>RW</td><td>同步触发位自动翻转使能控制位,可软件修改:1:对于非同步传输,数据发送成功后自动翻转MASK_UH_T_TOG[0]。0:不自动翻转,可以手动切换。</td><td>0</td></tr><tr><td>[4:3]</td><td>MASK_UH_T_TOG</td><td>RW</td><td>USB主机发送器(处理SETUP/OUT事务)准备的同步触发位00表示发送DATA0;01表示发送DATA1;10表示发送DATA2;11表示发送MDATA。</td><td>00b</td></tr><tr><td>2</td><td>RB_UH_T_RES_NO</td><td>RW</td><td>1:无应答,用于实现非端点0的实时/同步传输。此时忽略MASK_UEP_T_RES;0:发送数据成功后期待应答。</td><td>0</td></tr><tr><td>[1:0]</td><td>MASK_UH_T_RES</td><td>RW</td><td>USB主机发送器对SETUP/OUT事务的响应控制位00:期望应答ACK;10:期望应答NAK或忙;11:期望应答STALL或错误;01:期望应答NYET。</td><td>00b</td></tr><tr><td></td><td></td><td></td><td>对于实时/同步传输无效。</td><td></td></tr></table>

22.2.3.9 USB 主机发送 SPLIT 包的数据（R16_UH_SPLIT_DATA）  

<table><tr><td>位</td><td>名称</td><td>访问</td><td>描述</td><td>复位值</td></tr><tr><td>[15:12]</td><td>Reserved</td><td>R0</td><td>保留</td><td>0</td></tr><tr><td>[11:0]</td><td>UH_SPLIT_DATA</td><td>RW</td><td>主机端点发送SPLIT包的数据内容，低12位有效，高4位固定为0</td><td>0xxxh</td></tr></table>

# 第 23 章 USB 全速主机/设备控制器（OTG_FS/USBFS）

本章模块描述适用于CH32F2x、CH32V2x和CH32V3x微控制器系列部分产品。

## 23.1 USB 控制器简介

芯片内嵌 USB 控制器及收发器，特性如下：

双重角色设备控制器，支持 USB Host 主机功能和 USB Device 设备功能。  
遵循 On-The-GoSupplement to the USB2.0 规范，主机和设备模式均支持 USB2.0 全速 12Mbps或低速 1.5Mbps。  
支持软件 HNP 和 SRP 协议。  
$\bullet$ 支持 USB 控制传输、批量传输、中断传输、同步/实时传输。  
支持最大 64 字节的数据包，内置 FIFO，支持中断和 DMA。

## 23.2 寄存器描述

USB 相关寄存器分为 3 个部分，部分寄存器是在主机和设备模式下进行复用的。

USB全局寄存器  
$\bullet$ USB设备控制寄存器  
USB主机控制寄存器

### 23.2.1 全局寄存器描述

表 23-1 USBFS 相关寄存器列表  

<table><tr><td>名称</td><td>访问地址</td><td>描述</td><td>复位值</td></tr><tr><td>R8_USB_CTRL</td><td>0x50000000</td><td>USB 控制寄存器</td><td>0x06</td></tr><tr><td>R8_USB_INT_EN</td><td>0x50000002</td><td>USB 中断使能寄存器</td><td>0x00</td></tr><tr><td>R8_USB_DEV_AD</td><td>0x50000003</td><td>USB 设备地址寄存器</td><td>0x00</td></tr><tr><td>R32_USB_STATUS</td><td>0x50000004</td><td>USB 状态寄存器</td><td>0xXX20XXXX</td></tr><tr><td>R8_USB_MIS_ST</td><td>0x50000005</td><td>USB 杂项状态寄存器</td><td>0xXX</td></tr><tr><td>R8_USB_INT_PG</td><td>0x50000006</td><td>USB 中断标志寄存器</td><td>0x20</td></tr><tr><td>R8_USB_INT_ST</td><td>0x50000007</td><td>USB 中断状态寄存器</td><td>0xXX</td></tr><tr><td>R16_USB_RX_LEN</td><td>0x50000008</td><td>USB 接收长度寄存器</td><td>0xXX</td></tr><tr><td>R32_USB_OTG_CR</td><td>0x50000054</td><td>USB OTG 控制寄存器</td><td>0x00</td></tr><tr><td>R32_USB_OTG_SR</td><td>0x50000058</td><td>USB OTG 状态寄存器</td><td>0x00</td></tr></table>

23.2.1.1 USB 控制寄存器（R8_USB_CTRL）  

<table><tr><td>位</td><td>名称</td><td>访问</td><td>描述</td><td>复位值</td></tr><tr><td>7</td><td>RB_UC_HOST_MODE</td><td>RW</td><td>USB工作模式选择位:1:主机模式(HOST);0:设备模式(DEVICE)。</td><td>0</td></tr><tr><td>6</td><td>RB_UC_LOW_SPEED</td><td>RW</td><td>USB总线信号传输速率选择位:1:1.5Mbps;0:12Mbps。</td><td>0</td></tr><tr><td>5</td><td>RB_UC_DEVPU_EN</td><td>RW</td><td>USB设备模式下,USB设备使能和内部上拉电阻控制位,为1则使能USB设备传输并且启用内部上拉电阻。</td><td>0</td></tr><tr><td>[5:4]</td><td>MASK_UC_SYS_CTRL</td><td>RW</td><td>见下表配置USB系统。</td><td>0</td></tr><tr><td>3</td><td>RB_UC_INTBUSY</td><td>RW</td><td>USB 传输完成中断标志未清零前自动暂停使能位:
1: 在中断标志 UIF_TRANSFER未清零前自动暂停, 设备模式下自动应答忙 NAK, 主机模式下自动暂停后续传输;
0: 不暂停。</td><td>0</td></tr><tr><td>2</td><td>RB_UC_RST_SIE</td><td>RW</td><td>USB 协议处理器软件复位控制位:
1: 强制复位 USB 协议处理器 (SIE), 需要软件清零;
0: 不复位。</td><td>1</td></tr><tr><td>1</td><td>RB_UC_CLR_ALL</td><td>RW</td><td>USB 的 FIFO 和中断标志清零:
1: 强制清空和清零; 0: 不清。</td><td>1</td></tr><tr><td>0</td><td>RB_UC_DMA_EN</td><td>RW</td><td>USB 的 DMA 和 DMA 中断控制位:
1: 使能 DMA 功能和 DMA 中断;
0: 关闭 DMA。</td><td>0</td></tr></table>

由 RB_UC_HOST_MODE 和 MASK_UC_SYS_CTRL 组成 USB 系统控制组合：

表 23-2 USB 系统控制组合  

<table><tr><td>RB_UC_HOST_MODE</td><td>MASK_UC_SYS_CTRL</td><td>USB系统控制描述</td></tr><tr><td>0</td><td>00</td><td>禁止USB设备功能,关闭内部上拉电阻。</td></tr><tr><td>0</td><td>01</td><td>使能USB设备功能,关闭内部上拉电阻,需加外部上拉。</td></tr><tr><td>0</td><td>1x</td><td>使能USB设备功能,启用内部1.5K上拉电阻。该上拉电阻优先于下拉电阻,也可用于GPIO模式。</td></tr><tr><td>1</td><td>00</td><td>USB主机模式,正常工作状态。</td></tr><tr><td>1</td><td>01</td><td>USB主机模式,强制DP/DM输出SEO状态。</td></tr><tr><td>1</td><td>10</td><td>USB主机模式,强制DP/DM输出J状态。</td></tr><tr><td>1</td><td>11</td><td>USB主机模式,强制DP/DM输出K状态/唤醒。</td></tr></table>

23.2.1.2 USB 中断使能寄存器（R8_USB_INT_EN）  

<table><tr><td>位</td><td>名称</td><td>访问</td><td>描述</td><td>复位值</td></tr><tr><td>7</td><td>RB_UID_DEV_SOF</td><td>RW</td><td>USB设备模式,接收SOF包中断:1:使能中断;0:禁止中断。</td><td>0</td></tr><tr><td>6</td><td>RB_UID_DEV_NAK</td><td>RW</td><td>USB设备模式,接收到NAK中断:1:使能中断;0:禁止中断。</td><td>0</td></tr><tr><td>5</td><td>Reserved</td><td>RO</td><td>保留。</td><td>0</td></tr><tr><td>4</td><td>RB_UID_FIFO_0V</td><td>RW</td><td>FIFO溢出中断:1:使能中断;0:禁止中断。</td><td>0</td></tr><tr><td>3</td><td>RB_UID_HST_SOF</td><td>RW</td><td>USB主机模式,SOF定时中断:1:使能中断;0:禁止中断。</td><td>0</td></tr><tr><td>2</td><td>RB_UID_SUSPEND</td><td>RW</td><td>USB总线挂起或唤醒事件中断:1:使能中断;0:禁止中断。</td><td>0</td></tr><tr><td>1</td><td>RB_UID_TRANSFER</td><td>RW</td><td>USB传输完成中断:1:使能中断;0:禁止中断。</td><td>0</td></tr><tr><td>0</td><td>RB_UID_DETECT</td><td>RW</td><td>USB主机模式,USB设备连接或断开事件中断:</td><td>0</td></tr><tr><td rowspan="2"></td><td></td><td></td><td>1:使能中断; 0:禁止中断。</td><td></td></tr><tr><td>RB_UID_BUS_RST</td><td>RW</td><td>USB设备模式,USB总线复位事件中断:1:使能中断;0:禁止中断。</td><td>0</td></tr></table>

23.2.1.3 USB 设备地址寄存器（R8_USB_DEV_AD）  

<table><tr><td>位</td><td>名称</td><td>访问</td><td>描述</td><td>复位值</td></tr><tr><td>7</td><td>RB_UDA_GP_BIT</td><td>RW</td><td>USB通用标志位，用户自定义。</td><td>0</td></tr><tr><td>[6:0]</td><td>MASK_USB_ADDR</td><td>RW</td><td>主机模式：当前操作者的USB设备地址；设备模式：该USB自身地址。</td><td>0</td></tr></table>

23.2.1.4 USB 杂项状态寄存器（R8_USB_MIS_ST）  

<table><tr><td>位</td><td>名称</td><td>访问</td><td>描述</td><td>复位值</td></tr><tr><td>7</td><td>RB_UMS_SOF_PRES</td><td>RO</td><td>USB主机模式下SOF包预示状态位:1:将要发送SOF包,此时如有其它USB数据包将被自动延后;0:无SOF包发送。</td><td>x</td></tr><tr><td>6</td><td>RB_UMS_SOF_ACT</td><td>RO</td><td>USB主机模式下SOF包传输状态位:1:正在发出SOF包;0:发送完成或者空闲。</td><td>x</td></tr><tr><td>5</td><td>RB_UMS_SIE-Free</td><td>RO</td><td>USB协议处理器的空闲状态位:1:协议器空闲;0:忙,正在进行USB传输。</td><td>1</td></tr><tr><td>4</td><td>RB_UMS_R_FIFO_RDY</td><td>RO</td><td>USB接收FIFO数据就绪状态位:1:接收FIFO非空;0:接收FIFO为空。</td><td>0</td></tr><tr><td>3</td><td>RB_UMS_BUS_RST</td><td>RO</td><td>USB总线复位状态位:1:当前USB总线处于复位态;0:当前USB总线处于非复位态。</td><td>x</td></tr><tr><td>2</td><td>RB_UMS_SUSPEND</td><td>RO</td><td>USB挂起状态位:1:USB总线处于挂起态,有一段时间没有USB活动;0:USB总线处于非挂起态。</td><td>0</td></tr><tr><td>1</td><td>RB_UMS_DM_LEVEL</td><td>RO</td><td>USB主机模式下,设备刚连入USB端口时DM引脚的电平状态,用于判断速度:1:高电平/低速;0:低电平/全速。</td><td>0</td></tr><tr><td>0</td><td>RB_UMS_DEV_ATTACH</td><td>RO</td><td>USB主机模式下端口的USB设备连接状态位:1:端口已经连接USB设备;0:端口没有USB设备连接。</td><td>0</td></tr></table>

23.2.1.5 USB 中断标志寄存器（R8_USB_INT_FG）  

<table><tr><td>位</td><td>名称</td><td>访问</td><td>描述</td><td>复位值</td></tr><tr><td>7</td><td>RB_U_IS_NAK</td><td>R0</td><td>USB设备模式下，NAK响应状态位：1：当前USB传输过程中回应NAK；</td><td>0</td></tr><tr><td></td><td></td><td></td><td>0:无NAK响应。</td><td></td></tr><tr><td>6</td><td>RB_U_TOG_OK</td><td>RO</td><td>当前USB传输DATA0/1同步标志匹配状态位:1:同步;0:不同步。</td><td>0</td></tr><tr><td>5</td><td>RB_U_SIE-Free</td><td>RO</td><td>USB协议处理器空闲状态位:1:USB空闲;0:忙,正在进行USB传输。</td><td>1</td></tr><tr><td>4</td><td>RB_UID_FIFO_0V</td><td>RW</td><td>USB FIFO溢出中断标志位,写1清零:1: FIFO溢出触发;0:无事件。</td><td>0</td></tr><tr><td>3</td><td>RB_UID_HST_SOF</td><td>RW</td><td>USB主机模式下SOF定时中断标志位,写1清零:1:SOF包传输完成触发;0:无事件。</td><td>0</td></tr><tr><td>2</td><td>RB_UID_SUSPEND</td><td>RW</td><td>USB总线挂起或唤醒事件中断标志位,写1清零:1:USB挂起事件或唤醒事件触发;0:无事件。</td><td>0</td></tr><tr><td>1</td><td>RB_UID_TRANSFER</td><td>RW</td><td>USB传输完成中断标志位,写1清零:1:一个USB传输完成触发;0:无事件。</td><td>0</td></tr><tr><td rowspan="2">0</td><td>RB_UID_DETECT</td><td>RW</td><td>USB主机模式下USB设备连接或断开事件中断标志位,写1清零:1:检测到USB设备连接或断开触发;0:无事件。</td><td>0</td></tr><tr><td>RB_UID_BUS_RST</td><td>RW</td><td>USB设备模式下USB总线复位事件中断标志位,写1清零:1:USB总线复位事件触发;0:无事件。</td><td>0</td></tr></table>

23.2.1.6 USB 中断状态寄存器（R8_USB_INT_ST）  

<table><tr><td>位</td><td>名称</td><td>访问</td><td>描述</td><td>复位值</td></tr><tr><td>7</td><td>RB_UID_IS_NAK</td><td>R0</td><td>USB 设备模式下,NAK 响应状态位,同RB_U_IS_NAK:1:当前USB传输过程中回应NAK;0:无NAK响应。</td><td>0</td></tr><tr><td>6</td><td>RB_UID_TOG_OK</td><td>R0</td><td>当前USB传输DATA0/1同步标志匹配状态位,同RB_U_TOG_OK:1:同步;0:不同步。</td><td>0</td></tr><tr><td>[5:4]</td><td>MASK_UID_TOKEN</td><td>R0</td><td>设备模式下,当前USB传输事务的令牌PID标识。</td><td>x</td></tr><tr><td rowspan="2">[3:0]</td><td>MASK_UID_ENDP</td><td>R0</td><td>设备模式下,当前USB传输事务的端点号。</td><td>x</td></tr><tr><td>MASK_UID_H_RES</td><td>R0</td><td>主机模式下,当前USB传输事务的应答PID标识,0000表示设备无应答或超时;其它值表示应答PID。</td><td>x</td></tr></table>

MASK_UIS_TOKEN 用于 USB 设备模式下标识当前 USB 传输事务的令牌 PID：00 表示 OUT 包；01 表

示 SOF 包；10 表示 IN 包；11 表示 SETUP 包。

MASK_UIS_H_RES 仅在主机模式下有效。在主机模式下，若主机发送 OUT/SETUP 令牌包时，则该PID 是握手包 ACK/NAK/STALL，或是设备无应答/超时。若主机发送 IN 令牌包，则该 PID 是数据包的PID（DATA0/DATA1）或握手包 PID。

23.2.1.7 USB 接收长度寄存器（R16_USB_RX_LEN）  

<table><tr><td>位</td><td>名称</td><td>访问</td><td>描述</td><td>复位值</td></tr><tr><td>[15:10]</td><td>Reserved</td><td>R0</td><td>保留</td><td>0</td></tr><tr><td>[9:0]</td><td>R16_USB_RX_LEN</td><td>R0</td><td>当前USB端点接收的数据字节数</td><td>x</td></tr></table>

23.2.1.8 USB OTG 控制寄存器（R32_USB_OTG_CR）  

<table><tr><td>位</td><td>名称</td><td>访问</td><td>描述</td><td>复位值</td></tr><tr><td>[31:6]</td><td>Reserved</td><td>R0</td><td>保留</td><td>0</td></tr><tr><td>5</td><td>RB_CR_SESS_VTH</td><td>RW</td><td>OTG会话有效阈值电压设置1:SESS_VLD电平为1.4V;0:SESS_VLD电平为0.8V。</td><td>0</td></tr><tr><td>4</td><td>RB_CR_VBUS_VTH</td><td>RW</td><td>OTG VBUS阈值电压设置1: VBUS_VLD电平为4.4V;0: VBUS_VLD电平为4.8V。</td><td>0</td></tr><tr><td>3</td><td>RB_CR_OTG_EN</td><td>RW</td><td>OTG功能使能1:使能;0:禁止</td><td>0</td></tr><tr><td>2</td><td>RB_CR_IDPU</td><td>RW</td><td>USB_OTG_ID引脚上拉使能1:使能;0:禁止</td><td>0</td></tr><tr><td>1</td><td>RB_CR_CHARGE_VBUS</td><td>RW</td><td>OTG VBUS充电使能1:使能;0:禁止</td><td>0</td></tr><tr><td>0</td><td>RB_CR_DISCHAR_VBUS</td><td>RW</td><td>OTG VBUS放电使能1:使能;0:禁止</td><td>0</td></tr></table>

注：此寄存器仅适用于CH32V305、CH32V307、CH32F205和CH32F207

23.2.1.9 USB OTG 控制寄存器（R32_USB_OTG_SR）  

<table><tr><td>位</td><td>名称</td><td>访问</td><td>描述</td><td>复位值</td></tr><tr><td>[31:4]</td><td>Reserved</td><td>R0</td><td>保留</td><td>0</td></tr><tr><td>3</td><td>RB_SR_ID_DIG</td><td>R0</td><td>OTG ID 标志1: B 设备; 0: A 设备。</td><td>0</td></tr><tr><td>2</td><td>RB_SR_SESS_END</td><td>R0</td><td>OTG 会话结束有效标志1: 有效; 0: 无效</td><td>0</td></tr><tr><td>1</td><td>RB_SR_SESS_VLD</td><td>R0</td><td>OTG 会话有效标志1: 有效, 会话有效电平大于阈值电压0: 无效, 会话有效电平小于阈值电压</td><td>0</td></tr><tr><td>0</td><td>RB_SR_VBUS_VLD</td><td>R0</td><td>OTG VBUS 输入电平1: VBUS 电压大于阈值电压;0: VBUS 电压小于阈值电压。</td><td>X</td></tr></table>

注：此寄存器仅适用于CH32V305、CH32V307、CH32F205 和CH32F207

### 23.2.2 设备寄存器描述

USB OTG模块在USB设备模式下，提供了端点号 0-7共 8组双向端点配置寄存器，可映射端点号8-15的配置，除3号端点外所有端点最大包长度为64 字节，3号端点的最大数据包长度都是 1023 字节（同步传输）。

端点 0 是默认端点，支持控制传输，发送和接收共用一个 64 字节数据缓冲区  
端点1-15，可配置独立的64字节发送和接收缓冲区或者双 64字节数据缓冲区，支持批量传输、中断传输和实时/同步传输。

每组端点都具有一个控制寄存器 R8_UEPn_CTRL 和发送长度寄存器 R16_UEPn_T_LEN，用于设定该端点的同步触发位、对OUT事务和IN事务的响应以及发送数据的长度等。

USB OTG模块主机和设备的角色由OTG_FS_ID引脚状态决定，当 OTG_FS_ID引脚悬空，其内置的上拉电阻会使得 USB OTG 状态寄存器 R32_USB_OTG_SR 的 RB_SR_ID_DIG 位置 1，此时控制器应初始化为 B 设备。当 OTG_FS_ID 引脚接地，此时 USB OTG 状态寄存器 R32_USB_OTG_SR 的 RB_SR_ID_DIG 位为0，控制器应初始化为 A 设备。

B 设备在开始会话根据 OTG 状态寄存器 R32_USB_OTG_SR 的 RB_SR_SESS_VLD 来确保 VBUS 的电平低于 VSESS_VLD,min，若该位为 0，则可开始新的会话。若该位为 1，则可将 OTG 控制寄存器 R32_USB_OTG_CR的 RB_CR_DISCHAR_VBUS 位置 1 进行放电，使得 VBUS 小于会话阈值电平。

作为 B 类设备时，需从 VBUS 取电，则外部需一个转换电路，如图 22-1 所示。

![](images/31380bc190bf22c52494d8811acbd5f17a07f391503b62d73ae3d94d788dd731.jpg)  
图 23-1 OTG B 类设备连接图

作为 USB 设备所必要的 USB 总线上拉电阻可以由软件随时设置是否启用，当 USB 控制寄存器R8_USB_CTRL 中的 RB_UC_DEV_PU_EN 置 1 时，控制器根据 RB_UD_LOW_SPEED 的速度设置，在内部为 USB总线的DP/DM引脚连接上拉电阻，并启用 USB设备功能。

当检测到USB总线复位、USB总线挂起或唤醒事件，或当 USB成功处理完数据发送或数据接收后，USB协议处理器都将设置相应的中断标志，如果中断使能打开，还会产生相应的中断请求。应用程序可以直接查询或在 USB 中断服务程序中查询并分析中断标志寄存器 R8_USB_INT_FG，根据RB_UIF_BUS_RST 和 RB_UIF_SUSPEND 进行相应的处理；并且，如果 RB_UIF_TRANSFER 有效，那么还需要继续分析 USB 中断状态寄存器 R8_USB_INT_ST，根据当前端点号 MASK_UIS_ENDP 和当前事务令牌PID 标识 MASK_UIS_TOKEN 进行相应的处理。如果事先设定了各个端点的 OUT 事务的同步触发位RB_UEP_R_TOG，那么可以通过 RB_U_TOG_OK 或 RB_UIS_TOG_OK 判断当前所接收到的数据包的同步触发

位是否与该端点的同步触发位匹配，如果数据同步，则数据有效；如果数据不同步，则数据应该被丢弃。每次处理完USB发送或接收中断后，都应该正确修改相应端点的同步触发位，用于下次所发送的数据包或下次所接收的数据包是否同步检测；另外，设置 RB_UEP_AUTO_TOG 可以实现在发送成功或接收成功后自动翻转相应的同步触发位。

各个端点准备发送的数据在各自的缓冲区中，准备发送的数据长度是独立设定在 R8_UEPn_T_LEN中；各个端点接收到的数据在各自的缓冲区中，但是接收到的数据长度都在 USB 接收长度寄存器R8_USB_RX_LEN 中，可以在 USB 接收中断时根据当前端点号区分。

表 23-3 设备相关寄存器列表  

<table><tr><td>名称</td><td>访问地址</td><td>描述</td><td>复位值</td></tr><tr><td>R8_UDEV_CTRL</td><td>0x5000001</td><td>USB设备物理端口控制寄存器</td><td>0x0</td></tr><tr><td>R8_UEP4_1_MOD</td><td>0x5000000C</td><td>端点1(9)/4(8/12)模式控制寄存器</td><td>0x00</td></tr><tr><td>R8_UEP2_3_MOD</td><td>0x5000000D</td><td>端点2(10)/3(11)模式控制寄存器</td><td>0x00</td></tr><tr><td>R8_UEP5_6_MOD</td><td>0x5000000E</td><td>端点5(13)/6(14)模式控制寄存器</td><td>0x00</td></tr><tr><td>R8_UEP7_MOD</td><td>0x5000000F</td><td>端点7(15)模式控制寄存器</td><td>0x00</td></tr><tr><td>R16_UEPO_DMA</td><td>0x50000010</td><td>端点0缓冲区起始地址</td><td>0xFFFF</td></tr><tr><td>R16_UEP1_DMA</td><td>0x50000014</td><td>端点1(9)缓冲区起始地址</td><td>0xFFFF</td></tr><tr><td>R16_UEP2_DMA</td><td>0x50000018</td><td>端点2(10)缓冲区起始地址</td><td>0xFFFF</td></tr><tr><td>R16_UEP3_DMA</td><td>0x5000001C</td><td>端点3(11)缓冲区起始地址</td><td>0xFFFF</td></tr><tr><td>R16_UEP4_DMA</td><td>0x50000020</td><td>端点4(8/12)缓冲区起始地址</td><td>0xFFFF</td></tr><tr><td>R16_UEP5_DMA</td><td>0x50000024</td><td>端点5(13)缓冲区起始地址</td><td>0xFFFF</td></tr><tr><td>R16_UEP6_DMA</td><td>0x50000028</td><td>端点6(14)缓冲区起始地址</td><td>0xFFFF</td></tr><tr><td>R16_UEP7_DMA</td><td>0x5000002C</td><td>端点7(15)缓冲区起始地址</td><td>0xFFFF</td></tr><tr><td>R8_UEPO_T_LEN</td><td>0x50000030</td><td>端点0发送长度寄存器</td><td>0xX</td></tr><tr><td>R8_UEPO_TX_CTRL</td><td>0x50000032</td><td>端点0发送控制寄存器</td><td>0x00</td></tr><tr><td>R8_UEPO_RX_CTRL</td><td>0x50000033</td><td>端点0接收控制寄存器</td><td>0x00</td></tr><tr><td>R8_UEP1_T_LEN</td><td>0x50000034</td><td>端点1(9)发送长度寄存器</td><td>0xX</td></tr><tr><td>R8_UEP1_TX_CTRL</td><td>0x50000036</td><td>端点1(9)发送控制寄存器</td><td>0x00</td></tr><tr><td>R8_UEP1_RX_CTRL</td><td>0x50000037</td><td>端点1(9)接收控制寄存器</td><td>0x00</td></tr><tr><td>R8_UEP2_T_LEN</td><td>0x50000038</td><td>端点2(10)发送长度寄存器</td><td>0xX</td></tr><tr><td>R8_UEP2_TX_CTRL</td><td>0x5000003A</td><td>端点2(10)发送控制寄存器</td><td>0x00</td></tr><tr><td>R8_UEP2_RX_CTRL</td><td>0x5000003B</td><td>端点2(10)接收控制寄存器</td><td>0x00</td></tr><tr><td>R8_UEP3_T_LEN</td><td>0x5000003C</td><td>端点3(11)发送长度寄存器</td><td>0xX</td></tr><tr><td>R8_UEP3_TX_CTRL</td><td>0x5000003E</td><td>端点3(11)发送控制寄存器</td><td>0x00</td></tr><tr><td>R8_UEP3_RX_CTRL</td><td>0x5000003F</td><td>端点3(11)接收控制寄存器</td><td>0x00</td></tr><tr><td>R8_UEP4_T_LEN</td><td>0x50000040</td><td>端点4(8/12)发送长度寄存器</td><td>0xX</td></tr><tr><td>R8_UEP4_TX_CTRL</td><td>0x50000042</td><td>端点4(8/12)发送控制寄存器</td><td>0x00</td></tr><tr><td>R8_UEP4_RX_CTRL</td><td>0x50000043</td><td>端点4(8/12)接收控制寄存器</td><td>0x00</td></tr><tr><td>R8_UEP5_T_LEN</td><td>0x50000044</td><td>端点5(13)发送长度寄存器</td><td>0xX</td></tr><tr><td>R8_UEP5_TX_CTRL</td><td>0x50000046</td><td>端点5(13)发送控制寄存器</td><td>0x00</td></tr><tr><td>R8_UEP5_RX_CTRL</td><td>0x50000047</td><td>端点5(13)接收控制寄存器</td><td>0x00</td></tr><tr><td>R8_UEP6_T_LEN</td><td>0x50000048</td><td>端点6(14)发送长度寄存器</td><td>0xX</td></tr><tr><td>R8_UEP6_TX_CTRL</td><td>0x5000004A</td><td>端点6(14)发送控制寄存器</td><td>0x00</td></tr><tr><td>R8_UEP6_RX_CTRL</td><td>0x5000004B</td><td>端点6(14)接收控制寄存器</td><td>0x00</td></tr><tr><td>R8_UEP7_T_LEN</td><td>0x5000004C</td><td>端点7(15)发送长度寄存器</td><td>0xX</td></tr><tr><td>R8_UEP7_TX_CTRL</td><td>0x5000004E</td><td>端点7(15)发送控制寄存器</td><td>0x00</td></tr><tr><td>R8_UEP7_RX_CTRL</td><td>0x5000004F</td><td>端点7(15)接收控制寄存器</td><td>0x00</td></tr></table>

23.2.2.1 USB 设备物理端口控制寄存器（R8_UDEV_CTRL）  

<table><tr><td>位</td><td>名称</td><td>访问</td><td>描述</td><td>复位值</td></tr><tr><td>7</td><td>RB_UD_PD_DIS</td><td>RW</td><td>USB 设备端口 UD+/UD-引脚内部下拉电阻控制位:1:禁用内部下拉;0:使能内部下拉。可用于 GPIO 模式提供下拉电阻。</td><td>1</td></tr><tr><td>6</td><td>Reserved</td><td>RO</td><td>保留。</td><td>0</td></tr><tr><td>5</td><td>RB_UD_DP_PIN</td><td>RO</td><td>当前 UD+引脚状态:1:高电平; 0:低电平。</td><td>x</td></tr><tr><td>4</td><td>RB_UD_DM_PIN</td><td>RO</td><td>当前 UD-引脚状态:1:高电平; 0:低电平。</td><td>x</td></tr><tr><td>3</td><td>Reserved</td><td>RO</td><td>保留。</td><td>0</td></tr><tr><td>2</td><td>RB_UD_LOW_SPEED</td><td>RW</td><td>USB 设备物理端口低速模式使能位:1:选择1.5Mbps低速模式;0:选择12Mbps全速模式。</td><td>0</td></tr><tr><td>1</td><td>RB_UD_GP_BIT</td><td>RW</td><td>USB 设备模式通用标志位,用户自定义。</td><td>0</td></tr><tr><td>0</td><td>RB_UD_PORT_EN</td><td>RW</td><td>USB 设备物理端口使能位:1:使能物理端口;0:禁用物理端口。</td><td>0</td></tr></table>

23.2.2.2 端点 1(9)/4(8/12)模式控制寄存器（R8_UEP4_1_MOD）  

<table><tr><td>位</td><td>名称</td><td>访问</td><td>描述</td><td>复位值</td></tr><tr><td>7</td><td>RB_UEP1_RX_EN</td><td>RW</td><td>1:使能端点1(9)接收(OUT);0:禁止端点1(9)接收。</td><td>0</td></tr><tr><td>6</td><td>RB_UEP1_TX_EN</td><td>RW</td><td>1:使能端点1(9)发送(IN);0:禁止端点1(9)发送。</td><td>0</td></tr><tr><td>5</td><td>Reserved</td><td>RO</td><td>保留。</td><td>0</td></tr><tr><td>4</td><td>RB_UEP1BUF_MOD</td><td>RW</td><td>端点1(9)数据缓冲区模式控制位。注:该位为1时,UEP1_RX_EN和UEP1_TX_EN不能同时为1。</td><td>0</td></tr><tr><td>3</td><td>RB_UEP4_RX_EN</td><td>RW</td><td>1:使能端点4(8/12)接收(OUT);0:禁止端点4(8/12)接收。</td><td>0</td></tr><tr><td>2</td><td>RB_UEP4_TX_EN</td><td>RW</td><td>1:使能端点4(8/12)发送(IN);0:禁止端点4(8/12)发送。</td><td>0</td></tr><tr><td>1</td><td>Reserved</td><td>RO</td><td>保留。</td><td>0</td></tr><tr><td>0</td><td>RB_UEP4BUF_MOD</td><td>RW</td><td>端点4(8/12)数据缓冲区模式控制位。注:该位为1时,UEP4_RX_EN和UEP4_TX_EN不能同时为1。注:此位控制只支持CH32V103x系列。</td><td>0</td></tr></table>

注：端点1配置选项映射端点9，端点4配置选项映射端点8和12。

23.2.2.3 端点 2(10)/3(11)模式控制寄存器（R8_UEP2_3_MOD）  

<table><tr><td>位</td><td>名称</td><td>访问</td><td>描述</td><td>复位值</td></tr><tr><td>7</td><td>RB_UEP3_RX_EN</td><td>RW</td><td>1: 使能端点3(11)接收(OUT);</td><td>0</td></tr><tr><td></td><td></td><td></td><td>0:禁止端点3(11)接收。</td><td></td></tr><tr><td>6</td><td>RB_UEP3_TX_EN</td><td>RW</td><td>1:使能端点3(11)发送(IN);0:禁止端点3(11)发送。</td><td>0</td></tr><tr><td>5</td><td>Reserved</td><td>RO</td><td>保留。</td><td>0</td></tr><tr><td>4</td><td>RB_UEP3BUF_MOD</td><td>RW</td><td>端点3(11)数据缓冲区模式控制位。注:该位为1时,UEP3_RX_EN和UEP3_TX_EN不能同时为1。</td><td>0</td></tr><tr><td>3</td><td>RB_UEP2_RX_EN</td><td>RW</td><td>1:使能端点2(10)接收(OUT);0:禁止端点2(10)接收。</td><td>0</td></tr><tr><td>2</td><td>RB_UEP2_TX_EN</td><td>RW</td><td>1:使能端点2(10)发送(IN);0:禁止端点2(10)发送。</td><td>0</td></tr><tr><td>1</td><td>Reserved</td><td>RO</td><td>保留。</td><td>0</td></tr><tr><td>0</td><td>RB_UEP2BUF_MOD</td><td>RW</td><td>端点2(10)数据缓冲区模式控制位。注:该位为1时,UEP2_RX_EN和UEP2_TX_EN不能同时为1。</td><td>0</td></tr></table>

注：端点2配置选项映射端点10，端点3配置选项映射端点11。

23.2.2.4 端点 5(13)/6(14)模式控制寄存器（R8_UEP5_6_MOD）  

<table><tr><td>位</td><td>名称</td><td>访问</td><td>描述</td><td>复位值</td></tr><tr><td>7</td><td>RB_UEP6_RX_EN</td><td>RW</td><td>1:使能端点6(14)接收(OUT);0:禁止端点6(14)接收。</td><td>0</td></tr><tr><td>6</td><td>RB_UEP6_TX_EN</td><td>RW</td><td>1:使能端点6(14)发送(IN);0:禁止端点6(14)发送。</td><td>0</td></tr><tr><td>5</td><td>Reserved</td><td>RO</td><td>保留。</td><td>0</td></tr><tr><td>4</td><td>RB_UEP6BUF_MOD</td><td>RW</td><td>端点6(14)数据缓冲区模式控制位。注:该位为1时,UEP6_RX_EN和UEP6_TX_EN不能同时为1。</td><td>0</td></tr><tr><td>3</td><td>RB_UEP5_RX_EN</td><td>RW</td><td>1:使能端点5(13)接收(OUT);0:禁止端点5(13)接收。</td><td>0</td></tr><tr><td>2</td><td>RB_UEP5_TX_EN</td><td>RW</td><td>1:使能端点5(13)发送(IN);0:禁止端点5(13)发送。</td><td>0</td></tr><tr><td>1</td><td>Reserved</td><td>RO</td><td>保留。</td><td>0</td></tr><tr><td>0</td><td>RB_UEP5BUF_MOD</td><td>RW</td><td>端点5(13)数据缓冲区模式控制位。注:该位为1时,UEP5_RX_EN和UEP5_TX_EN不能同时为1</td><td>0</td></tr></table>

注：端点5配置选项映射端点13，端点6配置选项映射端点14。

23.2.2.5 端点 7(15)模式控制寄存器（R8_UEP7_MOD）  

<table><tr><td>位</td><td>名称</td><td>访问</td><td>描述</td><td>复位值</td></tr><tr><td>[7:4]</td><td>Reserved</td><td>R0</td><td>保留。</td><td>0</td></tr><tr><td>3</td><td>RB_UEP7_RX_EN</td><td>RW</td><td>1:使能端点7(15)接收(OUT);0:禁止端点7(15)接收。</td><td>0</td></tr><tr><td>2</td><td>RB_UEP7_TX_EN</td><td>RW</td><td>1:使能端点7(15)发送(IN);0:禁止端点7(15)发送。</td><td>0</td></tr><tr><td>1</td><td>Reserved</td><td>R0</td><td>保留。</td><td>0</td></tr><tr><td>0</td><td>RB_UEP7BUF_MOD</td><td>RW</td><td>端点7(15)数据缓冲区模式控制位。</td><td>0</td></tr></table>

注：端点7配置选项映射端点15。

由 RB_UEPn_RX_EN 和 RB_UEPn_TX_EN 以及 RB_UEPn_BUF_MOD 组合分别配置 USB 端点 1-15 的数据缓冲区模式，具体参考表 23-4。其中，在双 64 字节缓冲区模式下，USB 数据传输时将根据RB_UEP_*_TOG=0 选择前 64 字节缓冲区，根据 RB_UEP_*_TOG=1 选择后 64 字节缓冲区，设置RB_UEP_AUTO_ $\mathtt { T O G } = 1$ 可实现自动切换。

表 23-4 端点 n 缓冲区模式（ $\therefore n = 1 - 7$ ）  

<table><tr><td>RB_UEPn_RX_EN</td><td>RB_UEPn_TX_EN</td><td>RB_UEPnBUF_MOD</td><td>描述:以R16_UEPn_DMA为起始地址由低向高排列</td></tr><tr><td>0</td><td>0</td><td>X</td><td>端点被禁用,未用到R16_UEPn_DMA缓冲区。</td></tr><tr><td>1</td><td>0</td><td>0</td><td>单64字节接收缓冲区(OUT)。</td></tr><tr><td>1</td><td>0</td><td>1</td><td>双64字节接收缓冲区(OUT),由RB_UEP_R_TOG选择。</td></tr><tr><td>0</td><td>1</td><td>0</td><td>单64字节发送缓冲区(IN)。</td></tr><tr><td>0</td><td>1</td><td>1</td><td>双64字节发送缓冲区(IN),由RB_UEP_T_TOG选择。</td></tr><tr><td>1</td><td>1</td><td>0</td><td>单64字节接收缓冲区(OUT),单64字节发送缓冲区(IN)。</td></tr><tr><td>1</td><td>1</td><td>1</td><td>双64字节接收缓冲区(OUT),通过RB_UEP_R_TOG选择,双64字节发送缓冲区(IN),通过RB_UEP_T_TOG选择。全部256字节排列如下:UEPn_DMA+0地址:RB_UEP_R_TOG=0时端点接收地址;UEPn_DMA+64地址:RB_UEP_R_TOG=1时端点接收地址;UEPn_DMA+128地址:RB_UEP_T_TOG=0时端点发送地址;UEPn_DMA+192地址:RB_UEP_T_TOG=1时端点发送地址。</td></tr></table>

注：表21-4的配置选择支持 $\scriptstyle n = 1 - 7$ ，端点8-15配置映射端点1-7配置。

端点 n 缓冲区起始地址（R8_UEPn_DMA）（ $\scriptstyle n = 0 - 7$ ）  

<table><tr><td>位</td><td>名称</td><td>访问</td><td>描述</td><td>复位值</td></tr><tr><td>[7:0]</td><td>R8_UEPn_DMA</td><td>RW</td><td>端点n缓冲区起始地址。低15位有效，地址必须4字节对齐。</td><td>x</td></tr></table>

注：1.接收数据的缓冲区的长度 $> = m i n$ （可能收到的最大数据包长度＋2字节，64字节）。  
2.端点DMA配置支持0-7端点，可映射配置端点8-15端点。

23.2.2.6 端点 n 发送长度寄存器（R16_UEPn_T_LEN）（n=0-7）  

<table><tr><td>位</td><td>名称</td><td>访问</td><td>描述</td><td>复位值</td></tr><tr><td>[9:0]</td><td>R8_UEP3_T_LEN</td><td>RW</td><td>设置USB端点3准备发送的数据字节数。</td><td>x</td></tr><tr><td>[7:0]</td><td>R8_UEPn_T_LEN</td><td>RW</td><td>设置USB端点n准备发送的数据字节数n=0,1,2,4,5,6,7。</td><td>x</td></tr></table>

注1.端点发送长度配置支持0-7端点，可映射配置8-15端点的发送。  
2.主机发送支持最大1023字节（针对同步端点）

23.2.2.7 端点 n 控制寄存器（R8_UEPn_TX_CTRL）（ $\scriptstyle \mathbf { n = 0 - 7 }$ ）  

<table><tr><td>位</td><td>名称</td><td>访问</td><td>描述</td><td>复位值</td></tr><tr><td>[7:4]</td><td>Reserved</td><td>R0</td><td>保留。</td><td>0</td></tr><tr><td>3</td><td>RB_UEP_T_AUTO_TOG</td><td>RW</td><td>同步触发位自动翻转使能控制位:1:数据发送成功后自动翻转相应的同步</td><td>0</td></tr><tr><td></td><td></td><td></td><td>触发位;0:不自动翻转,可以手动切换。注:端点0此位保留。</td><td></td></tr><tr><td>2</td><td>RB_UEP_T_TOG</td><td>RW</td><td>USB端点n的发送器(处理IN事务)准备的同步触发位:1:发送DATA1; 0:发送DATA0。</td><td>0</td></tr><tr><td>[1:0]</td><td>MASK_UEP_T_RES</td><td>RW</td><td>端点n的发送器对IN事务的响应控制:00:DATA0/DATA1数据就绪并期望ACK;01:应答DATA0/DATA1并期望无响应,用于非端点0的实时/同步传输;10:应答NAK或忙;11:应答STALL或错误。</td><td>00b</td></tr></table>

注：端点配置支持0-7端点，可映射配置端点8-15端点。

#### 23.2.2.8 端点 n 控制寄存器（R8_UEPn_ RX_CTRL）（n=0-7）

<table><tr><td>位</td><td>名称</td><td>访问</td><td>描述</td><td>复位值</td></tr><tr><td>[7:4]</td><td>Reserved</td><td>R0</td><td>保留。</td><td>0</td></tr><tr><td>3</td><td>RB_UEP_R_AUTO_TOG</td><td>RW</td><td>同步触发位自动翻转使能控制位:1:数据接收成功后自动翻转相应的同步触发位;0:不自动翻转,可以手动切换。注:端点0此位保留。</td><td>0</td></tr><tr><td>2</td><td>MASK_UEP_R_TOG</td><td>RW</td><td>USB端点n的接收器(处理OUT事务)期望的同步触发位:1:期望DATA1; 0:期望DATA0。</td><td>0</td></tr><tr><td>[1:0]</td><td>MASK_UEP_R_RES</td><td>RW</td><td>端点n的接收器对OUT事务的响应控制:00:应答ACK;01:超时/无响应,用于非端点0的实时/同步传输;10:应答NAK或忙;11:应答STALL或错误。</td><td>00b</td></tr></table>

注：端点配置支持0-7端点，可映射配置端点8-15端点。

### 22.2.3 USB 主机寄存器

在USB OTG主机模式下，芯片提供了一组双向主机端点，包括一个发送端点 OUT和一个接收端点IN，一个数据包的最大长度是1023字节，支持控制传输、中断传输、批量传输和实时/同步传输。

在USB OTG主机模式下，若控制器不能为VUBS 提供 5V电源，则需外接电荷泵，若应用板能提供 5V电源则可采用模拟开关控制 VBUS 的打开和关断，如图 23-2 所示。

![](images/3c70dcb93bf4f5b90c9e08e0f363fe127abe2d14f999ae595d7a3f8d3a207428.jpg)  
图 23-2 OTG 的 A 类设备连接

主机端点发起的每一个USB事务，在处理结束后总是自动设置 RB_UIF_TRANSFER中断标志。应用程序可以直接查询或在USB中断服务程序中查询并分析中断标志寄存器 R8_USB_INT_FG，根据各中断标志分别进行相应的处理；并且，如果RB_UIF_TRANSFER有效，那么还需要继续分析 USB中断状态寄存器 R8_USB_INT_ST，根据当前 USB 传输事务的应答 PID 标识 MASK_UIS_H_RES 进行相应的处理。

如果事先设定了主机接收端点的 IN 事务的同步触发位（RB_UH_R_TOG），那么可以通过RB_U_TOG_OK 或 RB_UIS_TOG_OK 判断当前所接收到的数据包的同步触发位是否与主机接收端点的同步触发位匹配，如果数据同步，则数据有效；如果数据不同步，则数据应该被丢弃。每次处理完 USB发送或接收中断后，都应该正确修改相应主机端点的同步触发位，用于同步下次所发送的数据包和检测下次所接收的数据包是否同步；另外，通过设置 RB_UH_T_AUTO_TOG 和 RB_UH_R_AUTO_TOG 可以实现在发送成功或接收成功后自动翻转相应的同步触发位。

USB主机令牌设置寄存器R8_UH_EP_PID用于设置被操作的目标设备的端点号和本次 USB传输事务的令牌PID包标识。SETUP令牌和OUT令牌所对应的数据由主机发送端点提供，准备发送的数据在 R16_UH_TX_DMA 缓冲区中，准备发送的数据长度设置在 R16_UH_TX_LEN 中；IN 令牌所对应数据由目标设备返回给主机接收端点，接收到数据存放 R16_UH_RX_DMA 缓冲区中，接收到的数据长度存放在 R16_USB_RX_LEN 中，主机端点可接收的最大包长度需要提前写入到 R16_UH_RX_MAX_LEN 寄存器中。

表23-5 主机相关寄存器列表  

<table><tr><td>名称</td><td>访问地址</td><td>描述</td><td>复位值</td></tr><tr><td>R8_UHOST_CTRL</td><td>0x50000001</td><td>USB 主机物理端口控制寄存器</td><td>0x0</td></tr><tr><td>R32_UH_EP_MOD</td><td>0x5000000D</td><td>USB 主机端点模式控制寄存器</td><td>0x00</td></tr><tr><td>R16_UH_RX_DMA</td><td>0x50000018</td><td>USB 主机接收缓冲区起始地址</td><td>X</td></tr><tr><td>R16_UH_TX_DMA</td><td>0x5000001C</td><td>USB 主机发送缓冲区起始地址</td><td>X</td></tr><tr><td>R16_UH_setup</td><td>0x50000036</td><td>USB 主机辅助设置寄存器</td><td>0x00</td></tr><tr><td>R8_UH_EP_PID</td><td>0x50000038</td><td>USB 主机令牌设置寄存器</td><td>0x00</td></tr><tr><td>R8_UH_RX_CTRL</td><td>0x5000003A</td><td>USB 主机接收端点控制寄存器</td><td>0x00</td></tr><tr><td>R16_UH_TX_LEN</td><td>0x5000003C</td><td>USB 主机发送长度寄存器</td><td>X</td></tr><tr><td>R8_UH_TX_CTRL</td><td>0x5000003E</td><td>USB 主机发送端点控制寄存器</td><td>0x00</td></tr></table>

22.2.3.1 USB 主机物理端口控制寄存器（R8_UHOST_CTRL）  

<table><tr><td>位</td><td>名称</td><td>访问</td><td>描述</td><td>复位值</td></tr><tr><td>7</td><td>RB_UH_PD_DIS</td><td>RW</td><td>USB 主机端口 UD+/UD-引脚内部下拉电阻控制位:1:禁用内部下拉;0:使能内部下拉。可用于 GPIO 模式提供下拉电阻。</td><td>1</td></tr><tr><td>6</td><td>Reserved</td><td>RO</td><td>保留。</td><td>0</td></tr><tr><td>5</td><td>RB_UH_DP_PIN</td><td>RO</td><td>当前 UD+引脚状态:1:高电平; 0:低电平。</td><td>x</td></tr><tr><td>4</td><td>RB_UH_DM_PIN</td><td>RO</td><td>当前 UD-引脚状态:1:高电平; 0:低电平。</td><td>x</td></tr><tr><td>3</td><td>Reserved</td><td>RO</td><td>保留。</td><td>0</td></tr><tr><td>2</td><td>RB_UH_LOW_SPEED</td><td>RW</td><td>USB 主机端口低速模式使能位:1:选择 1.5Mbps 低速模式;0:选择 12Mbps 全速模式。</td><td>0</td></tr><tr><td>1</td><td>RB_UH_BUS_RST</td><td>RW</td><td>USB 主机模式总线复位控制位:1:强制输出 USB 总线复位;0:结束输出。</td><td>0</td></tr><tr><td>0</td><td>RB_UH_PORT_EN</td><td>RW</td><td>USB 主机端口使能位:1:使能主机端口;0:禁用主机端口。当 USB 设备断开连接时,该位自动清 0。</td><td>0</td></tr></table>

22.2.3.2 USB 主机端点模式控制寄存器（R32_UH_EP_MOD）  

<table><tr><td>位</td><td>名称</td><td>访问</td><td>描述</td><td>复位值</td></tr><tr><td>7</td><td>Reserved</td><td>R0</td><td>保留。</td><td>0</td></tr><tr><td>6</td><td>RB_UH_EP_TX_EN</td><td>RW</td><td>主机发送端点发送 (SETUP/OUT) 使能位:1: 使能端点发送;0: 禁止端点发送。</td><td>0</td></tr><tr><td>5</td><td>Reserved</td><td>R0</td><td>保留。</td><td>0</td></tr><tr><td>4</td><td>RB_UH_EP_TBUF_MOD</td><td>RW</td><td>主机发送端点发送数据缓冲区模式控制位。</td><td>0</td></tr><tr><td>3</td><td>RB_UH_EP_RX_EN</td><td>RW</td><td>主机接收端点接收 (IN) 使能位:1: 使能端点接收;0: 禁止端点接收。</td><td>0</td></tr><tr><td>[2:1]</td><td>Reserved</td><td>R0</td><td>保留。</td><td>0</td></tr><tr><td>0</td><td>RB_UH_EP_RBUF_MOD</td><td>RW</td><td>USB 主机接收端点接收数据缓冲区模式控制位。</td><td>0</td></tr></table>

由 RB_UH_EP_TX_EN 和 RB_UH_EP_TBUF_MOD 组合控制主机发送端点数据缓冲区模式，参考下表。

表23-6 主机发送缓冲区模式  

<table><tr><td>RB_UH_EP_TX_EN</td><td>RB_UH_EP_TBUF_MOD</td><td>描述:以R16_UH_TX_DMA为起始地址</td></tr><tr><td>0</td><td>X</td><td>端点被禁用,未用到R16_UH_TX_DMA缓冲区。</td></tr><tr><td>1</td><td>0</td><td>单64字节发送缓冲区(SETUP/OUT)。</td></tr><tr><td>1</td><td>1</td><td>双64字节发送缓冲区,通过RB_UH_T_TOG选择:当RB_UH_T_TOG=0时选择前64字节缓冲区;当RB_UH_T_TOG=1时选择后64字节缓冲区。</td></tr></table>

由 RB_UH_EP_RX_EN 和 RB_UH_EP_RBUF_MOD 组合控制主机接收端点数据缓冲区模式，参考下表。

表23-7 主机接收缓冲区模式  

<table><tr><td>RB_UH_EP_RX_EN</td><td>RB_UH_EP_RBUF_MOD</td><td>结构描述:以R16_UH_TX_DMA为起始地址</td></tr><tr><td>0</td><td>X</td><td>端点被禁用,未用到R16_UH_RX_DMA缓冲区。</td></tr><tr><td>1</td><td>0</td><td>单64字节接收缓冲区(IN)。</td></tr><tr><td>1</td><td>1</td><td>双64字节接收缓冲区,通过RB_UH_R_TOG选择:当RB_UH_R_TOG=0时选择前64字节缓冲区;当RB_UH_R_TOG=1时选择后64字节缓冲区。</td></tr></table>

USB 主机接收缓冲区起始地址（R16_UH_RX_DMA）  

<table><tr><td>位</td><td>名称</td><td>访问</td><td>描述</td><td>复位值</td></tr><tr><td>[15:0]</td><td>R16_UH_RX_DMA</td><td>RW</td><td>主机端点数据接收缓冲区起始地址。低15位有效，地址必须4字节对齐。</td><td>x</td></tr></table>

USB 主机发送缓冲区起始地址（R16_UH_TX_DMA）  

<table><tr><td>位</td><td>名称</td><td>访问</td><td>描述</td><td>复位值</td></tr><tr><td>[15:0]</td><td>R16_UH_TX_DMA</td><td>RW</td><td>主机端点数据发送缓冲区起始地址。低15位有效，地址必须4字节对齐。</td><td>x</td></tr></table>

22.2.3.3 USB 主机辅助设置寄存器（R16_UH_SETUP）  

<table><tr><td>位</td><td>名称</td><td>访问</td><td>描述</td><td>复位值</td></tr><tr><td>[15:11]</td><td>Reserved</td><td>R0</td><td>保留。</td><td>0</td></tr><tr><td>10</td><td>RB_UH_PRE_PID_EN</td><td>RW</td><td>低速前导包 PRE_PID 使能位:1:使能,用于通过外部 HUB 与低速 USB设备通讯。0:禁用低速前导包。</td><td>0</td></tr><tr><td>[9:3]</td><td>Reserved</td><td>R0</td><td>保留。</td><td>0</td></tr><tr><td>2</td><td>RB_UH_SOF_EN</td><td>RW</td><td>自动产生 SOF 包使能位:1:主机自动产生 SOF 包;0:关闭自动 SOF 功能。</td><td>0</td></tr><tr><td>[1:0]</td><td>Reserved</td><td>R0</td><td>保留。</td><td>0</td></tr></table>

22.2.3.4 USB 主机令牌设置寄存器（R8_UH_EP_PID）  

<table><tr><td>位</td><td>名称</td><td>访问</td><td>描述</td><td>复位值</td></tr><tr><td>[7:4]</td><td>MASK_UH_TOKEN</td><td>RW</td><td>设置本次USB传输事务的令牌PID标识。</td><td>0</td></tr><tr><td>[3:0]</td><td>MASK_UH_ENDP</td><td>RW</td><td>设置本次被操作的目标设备的端点号。</td><td>0</td></tr></table>

22.2.3.5 USB 主机接收端点控制寄存器（R8_UH_RX_CTRL）  

<table><tr><td>位</td><td>名称</td><td>访问</td><td>描述</td><td>复位值</td></tr><tr><td>[7:4]</td><td>Reserved</td><td>R0</td><td>保留。</td><td>0</td></tr><tr><td>3</td><td>RB_UH_R_AUTO_TOG</td><td>RW</td><td>同步触发位自动翻转使能控制位:1:数据接收成功后自动翻转相应的期待同步触发位(RB_UH_R_TOG);0:手动控制同步触发位(RB_UH_R_TOG)。</td><td>0</td></tr><tr><td>2</td><td>RB_UH_R_TOG</td><td>RW</td><td>主机接收器(处理IN事务)准备的同步触发位:1:无响应,用于非0端点的实时/同步传输;0:应答ACK。</td><td>0</td></tr><tr><td>1</td><td>Reserved</td><td>R0</td><td>保留。</td><td>0</td></tr><tr><td>0</td><td>RB_UH_R_RES</td><td>R0</td><td>主机接收器对IN事务的响应控制位:1:无响应,用于非0端点的实时/同步传输;0:应答ACK。</td><td>0</td></tr></table>

22.2.3.6 USB 主机发送长度寄存器（R16_UH_TX_LEN）  

<table><tr><td>位</td><td>名称</td><td>访问</td><td>描述</td><td>复位值</td></tr><tr><td>[15:0]</td><td>R16_UH_TX_LEN</td><td>RW</td><td>设置USB主机发送端点准备发送的数据字节数。</td><td>x</td></tr></table>

22.2.3.7 USB 主机发送端点控制寄存器（R8_UH_TX_CTRL）  

<table><tr><td>位</td><td>名称</td><td>访问</td><td>描述</td><td>复位值</td></tr><tr><td>[7:4]</td><td>Reserved</td><td>R0</td><td>保留。</td><td>0</td></tr><tr><td>3</td><td>RB_UH_T_AUTO_TOG</td><td>R0</td><td>同步触发位自动翻转使能控制位:1:数据发送成功后自动翻转相应的同步触发位(RB_UH_T_TOG);0:手动控制同步触发位(RB_UH_T_TOG)。</td><td>0</td></tr><tr><td>2</td><td>RB_UH_T_TOG</td><td>RW</td><td>USB主机发送器(处理SETUP/OUT事务)准备的同步触发位:1:表示发送DATA1;0:表示发送DATA0。</td><td>0</td></tr><tr><td>1</td><td>Reserved</td><td>R0</td><td>保留。</td><td>0</td></tr><tr><td>0</td><td>RB_UH_T_RES</td><td>RW</td><td>USB主机发送器对SETUP/OUT事务的响应控制位:1:期望无响应,用于非0端点的实时/同步传输;0:期望应答ACK。</td><td>0</td></tr></table>

# 第 24 章 控制器局域网（CAN）

本章模块描述适用于CH32F2x、CH32V2x和CH32V3x微控制器系列部分产品。

控制器局域网是一种用于串行数据通信的高性能通信协议。CAN控制器提供了一个完整的 CAN 协议实现方案，支持 CAN 协议 2.0A 和 2.0B。CAN 控制器可以用来构建强大的局域网来实现安全的分布式实时控制，以较小的CPU负荷来处理大量的数据报文，在工业和汽车领域有着广泛的应用。

## 24.1 主要特性

兼容 CAN 规范 2.0A 和 2.0B  
$\bullet$ 可编程的传输速率，最高可达 1Mbit/s  
$\bullet$ 支持时间触发通信功能，避免低优先级消息阻塞  
$\bullet$ 支持三个发送邮箱，发送报文优先级可由报文标识符或发送请求的次序决定，并可记录发送报文SOF时刻的时间戳  
l 支持三级邮箱深度的 2 个接收 FIFO，28 个报文过滤器组可供配置，大容量产品 CAN1 和 CAN2 共用28个过滤器，每个过滤器组可配置成32 或16 位模式，屏蔽位或标识符列表模式，能够尽量减少软件对报文筛选的干预，FIFO溢出处理方式灵活，并可记录接收报文 SOF时刻的时间戳  
占用 4 个中断向量，每个中断源可以独立配置

## 24.2 CAN 控制器工作模式

CAN 控制器可以对寄存器 CAN_CTLR 中的 SLEEP 或 INRQ 位进行操作，实现在初始化模式、睡眠模式和正常模式3个工作模式下切换。

### 24.2.1 初始化模式

在复位后，CAN默认工作在睡眠模式以减低功耗，此时禁止报文收发，TX引脚的内部上拉电阻使能，TX引脚输出隐性位。对寄存器CAN_CTLR中的INRQ 位置 1，请求 CAN控制器进入初始化模式，当寄存器 CAN_STATR 的 INAK 位自动置 1 则成功进入初始化状态。同样对寄存器 CAN_CTLR 中的 INRQ 位清零，请求 CAN 控制器退出初始化模式，当寄存器 CAN_STATR 的 INAK 位自动清 0 则成功退出初始化状态。

对过滤器组进行初始化，可以在非初始化模式下进行，不过必须对寄存器 CAN_FCTLR 的 FINIT位进行置1，此时禁止接收报文。

### 24.2.2 睡眠模式

对寄存器CAN_CTLR中的SLEEP位置1，请求CAN控制器进入睡眠模式，当寄存器CAN_STATR的SNAK位自动置1则CAN成功进入睡眠模式，此时CAN控制器的时钟停止，但邮箱寄存器仍可访问。

由睡眠模式进入初始化模式，必须对CAN_CTLR的SLEEP位清O，INRQ位置1，当寄存器CAN_STATR的位自动置 1 则切换为初始化状态完成。

由睡眠模式进入正常模式，必须对CAN_CTLR的SLEEP位清O，当寄存器CAN_STATR的SNAK位自动清0则进入正常模式。

![](images/f0c487bca015eef32ed2a59f52c865a20e937b1151aa36eded58d2e6cd6e1d31.jpg)  
图 24-1 CAN 工作模式切换

### 24.3 CAN 控制器测试模式

在初始化模式下，对寄存器 CAN_BTIMR 的 SILM 和 LBKM 位进行操作，可以选择一种测试模式，然后通过对寄存器CAN_CTLR的INRQ位清零，退出初始化模式，进入测试模式。测试模式分为静默模式、环回模式和静默环回模式三种。

### 24.3.1 静默模式

对寄存器CAN_BTIMR的SILM位置1，可选择进入静默模式。该模式下，CAN控制器可以接收，不能对外发送报文，对外总是处于隐性位，可以避免对总线产生影响，但是报文能够被所在节点的控制器所接收。通常静默模式被用于CAN总线的状态分析。

### 24.3.2 环回模式

对寄存器CAN_BTIMR的LBKM位置1，可选择进入环回模式。该模式下，CAN控制器可以对外发送报文，不能接收外部报文，但是发送报文能够被所在节点的控制器所接收，接收过滤机制有效。通常环回模式被用于CAN控制器的收发测试。

### 24.3.3 静默环回模式

对寄存器CAN_BTIMR的SILM和LBKM位置1，可选择进入静默环回模式。该模式通常用于 CAN控制器封闭自测试，在该模式下，对 CAN 总线无影响，RX 引脚与总线断开，TX 引脚置隐性位。

![](images/127d8f947e465a7b84e6ec2992d016def587763f2b34487994c05e656a0db656.jpg)  
图24-2 CAN总线的三种测试模式

![](images/7081f5d6a6f1fa4ecadf54306b3a8067abd313e0dd1c0b8f3368a294f864cc38.jpg)

![](images/391a060d97a992266c321232c59e98ea3c15db494ea12d01467acb24e9796f99.jpg)

### 24.4 MCU 处于调试模式下 CAN 控制器的工作状态

当MCU进入调试模式后，内核处于暂停状态，但可以通过调试模块中配置位来决定 CAN控制器是处于正常运行或停止状态。

## 24.5 CAN 控制器功能描述

### 24.5.1 发送处理流程

发送处理流程如下：如果三个发送邮箱中有空置的邮箱，应用层软件仅对空置邮箱的寄存器具有写入权限，对寄存器 CAN_TXMIRx、CAN_TXMDTRx、CAN_TXMDLRx 和 CAN_TXMDHRx 进行操作，可以设置报文标识符、报文长度、时间戳和报文数据等。在数据准备好之后，对寄存器 CAN_TXMIRx 的 TXRQ 位置 1 请求发送，邮箱进入挂号状态，并进行优先级排队；一旦成为最高优先级邮箱，则变为预定发送状态，等待 CAN 总线空闲；当 CAN 总线空闲时，预定发送邮箱的报文立刻进入发送状态；报文发送完毕后，邮箱重新成为空置邮箱，并且寄存器CAN_TSTATR 的RQCP 和 TXOK位置1，来指示发送成功；若发送时仲裁失败，寄存器CAN_TSTATR的ALST位置1，若发送错误，则 TERR位置 1。

### 24.5.2 发送优先级

发送优先级可以由标识符或发送请求先后次序决定，寄存器CAN_CTLR 的 TXFP位置1 按发送请求先后次序发送，按发送请求先后次序主要应用于分段发送；TXFP 位清 0 按标识符优先级决定发送次序，标识符越小则优先级越高，同标识符的情况下，则低编号的邮箱有更高优先级。

### 24.5.3 发送中止处理

若对寄存器CAN_TSTATR的ABRQ位置 1，则可以中止发送请求。当邮箱状态为挂号或预定发送状态时，发送请求直接中止；当邮箱处于发送状态时，中止请求可能会成功（停止发送），也有可能会失败（发送完成），结果可由寄存器 CAN_TSTATR 的 TXOK 位来查询。

### 24.5.4 基于时间触发模式

传统的CAN通信总线繁忙时，容易造成低优先级的消息长时间阻塞，甚至无法满足其时限的要求。为了解决该瓶颈，推出了基于时间触发模式的相关协议，此类协议在工业上有一定规模的应用，基于时间触发模式的功能即为配合此类协议的应用。

在时间触发模式下有两种模式可供选择，使用该模式需关闭自动重传功能，通过配置 CAN_TTCTLR寄存器的MODE位来选择默认模式和增强模式。对寄存器 CAN_CTLR的 TTCM和NART位置 1，使能时间触发模式并禁止自动重传，CAN_TTCTLR 寄存器的 MODE 位默认为 0，此时工作在默认模式，内部定时器被激活用来产生发送和接收邮箱的时间戳，定时器在 CAN位时间累加，内部定时器在接收和发送的

帧起始位的采样点位置被采样并产生时间戳。若使用增强模式，需要将配置CAN_TTCTLR寄存器的MODE位为1，来开启增强模式，使用该模式，在整个 CAN网络中，必须存在三个或三个以上的节点，其中一个节点发送时间基准，其他节点收到该基准节点的时间戳后，通过向 CAN_TTCTLR寄存器的 TIMRST位写1复位内部计数器，将内部计数器进行同步，这样除了发送时间基准的节点之外，其余CAN节点实现了时间同步，之后将待发送的数据写入发送邮箱，依次配置各节点的时间触发计数值（CAN_TTCNT寄存器的 TIMCNT）和内部计数器计数终值（CAN_TTCTLR 寄存器 TIMCMV），时间触发计数值和内部计数器计数终值由 CAN 节点的个数、CAN 通信速率和一帧数据位数决定，配置完成后，各节点等待内部计数器计数到时间触发计数值后，触发发送动作。

### 24.5.5 接收处理流程

CAN 总线报文的接收，由控制器硬件来完成，无需MCU 的干涉，减轻了 MCU的处理负荷。所接收到的报文，根据寄存器 CAN_FAFIFOR 的设置，分别被存储到两个具有 3 级邮箱深度的 FIFO 中，应用层如需获取报文，只能通过接收FIFO邮箱来读取有效接收报文。

初始时，接收 FIFO 为空，接收 FIFO 寄存器 CAN_RFIFOx 的 FMR[1:0]值为二进制 00b，接到一个有效接收报文后，变为挂号 1 状态，控制器自动把接收 FIFO 寄存器 CAN_RFIFOx 的 FMR[1:0]设置二进制 01b；若此时读取邮箱数据寄存器 CAN_RXMDLRx 和 CAN_RXMDHRx，通过对接收 FIFO 寄存器CAN_RFIFOx的RFOM位置1来释放邮箱，接收 FIFO状态又变为空；如果在挂号 1 状态时不释放邮箱，下一个有效接收报文被接到后，接收 FIFO 状态切换为挂号 2 状态，此时接收 FIFO 寄存器 CAN_RFIFOx的FMR[1:0]自动置二进制10b；若读取邮箱数据寄存器并释放邮箱，则状态回到挂号 1；如果在挂号2 状态不释放邮箱，则接收 FIFO 进入挂号 3 状态；同样在挂号 3 状态下读取报文并释放邮箱，则返回挂号 2 状态；若在挂号 3 状态不释放邮箱，则在接收到下一个有效报文时，必然导致报文丢失情况出现。

![](images/d45ce42eac6a011989d38382e4a52d76f40e1694148caaa15cd3f8e176a8569b.jpg)  
图 24-3 接收 FIFO 状态切换图

上文中的报文丢失情况，即接收FIFO为满，报文溢出导致报文丢失，接收 FIFO寄存器 CAN_RFIFOx的 FOVR 位会硬件自动置 1，以供溢出查询。寄存器 CAN_CTLR 的 RFLM 位置 1，则接收 FIFO 锁定功能启用，丢弃的报文为新接收报文；寄存器 CAN_CTLR的 RFLM 位清 0，则接收FIFO 锁定功能停用，接收FIFO的三个原报文中，最后接收的报文会被新报文覆盖。

当寄存器CAN_INTENR相关位置位，可以使接收 FIFO状态切换时产生中断，以便更高效的处理接收报文，详见24.6节CAN中断。

### 24.5.6 接收报文标识符过滤

模块中有着多达 28 个过滤器组，通过设置过滤器组，每个 CAN 节点都可以接收到符合过滤规则的报文，不符合过滤规则的报文被硬件丢弃，无需软件干涉。

每个过滤器组由 2 个 32 位寄存器 CAN_FxR0 和 CAN_FxR1 组成。过滤器组的位宽都可以通过设置寄存器 CAN_FSCFGR 的各个位独立配置成 1 个 32 位过滤器或两个 16 位过滤器。每个过滤器组可通过设置寄存器 CAN_FMCFGR 的各个位配置为屏蔽位或标识符列表模式，各个过滤器组可以通过设置寄存器CAN_FWR的各个位选择启用或禁用。设置寄存器 CAN_FAFIFOR的各个位可以把选择通过过滤器的报文存放到哪个接收FIFO。

如下表 24-1 所示，屏蔽位模式下，两个寄存器分别为标识符寄存器和屏蔽寄存器，两者需要配合使用，标识符寄存器每一位指示相应的位期望值为显性或隐性，屏蔽寄存器每一位指示相应位是否需要对应标识符寄存器位期望值一致。

表 24-1 32 位屏蔽位模式  

<table><tr><td>标识符寄存器</td><td>CAN_FxR1[31:24]</td><td colspan="2">CAN_FxR1[23:16]</td><td>CAN_FxR1[15:8]</td><td colspan="4">CAN_FxR1[7:0]</td></tr><tr><td>屏蔽位寄存器</td><td>CAN_FxR2[31:24]</td><td colspan="2">CAN_FxR2[23:16]</td><td>CAN_FxR2[15:8]</td><td colspan="4">CAN_FxR2[7:0]</td></tr><tr><td>映射</td><td>STID[10:3]</td><td>STID[2:0]</td><td>EXID[17:13]</td><td>EXID[12:5]</td><td>EXID[4:0]</td><td>IDE</td><td>RTR</td><td>0</td></tr></table>

标识符列表模式下，两个寄存器都被用作标识符寄存器，接收报文标识符必须与其中一个寄存器保持一致才能通过筛选。

表24-2 32位标识符列表模式  

<table><tr><td>标识符寄存器</td><td>CAN_FxR1[31:24]</td><td colspan="2">CAN_FxR1[23:16]</td><td>CAN_FxR1[15:8]</td><td colspan="4">CAN_FxR1[7:0]</td></tr><tr><td>屏蔽位寄存器</td><td>CAN_FxR2[31:24]</td><td colspan="2">CAN_FxR2[23:16]</td><td>CAN_FxR2[15:8]</td><td colspan="4">CAN_FxR2[7:0]</td></tr><tr><td>映射</td><td>STID[10:3]</td><td>STID[2:0]</td><td>EXID[17:13]</td><td>EXID[12:5]</td><td>EXID[4:0]</td><td>IDE</td><td>RTR</td><td>0</td></tr></table>

在 16 位模式下，寄存器组被拆分成四个寄存器，屏蔽位模式每组过滤器的屏蔽位模式可以有 2个过滤器，每个过滤器里各包含一个16位标识符寄存器和 16位屏蔽寄存器；标识符列表模式下四个寄存器都用作标识符寄存器。

表 24-3 16 位屏蔽位模式  

<table><tr><td>标识符寄存器n</td><td>CAN_FxR1[15:8]</td><td colspan="4">CAN_FxR1[7:0]</td></tr><tr><td>屏蔽位寄存器n</td><td>CAN_FxR1[31:24]</td><td colspan="4">CAN_FxR1[23:16]</td></tr><tr><td>标识符寄存器n+1</td><td>CAN_FxR2[15:8]</td><td colspan="4">CAN_FxR2[7:0]</td></tr><tr><td>屏蔽位寄存器n+1</td><td>CAN_FxR2[31:24]</td><td colspan="4">CAN_FxR2[23:16]</td></tr><tr><td>映射</td><td>STID[10:3]</td><td>STID[2:0]</td><td>RTR</td><td>IDE</td><td>EXID[17:15]</td></tr></table>

表24-4 16位标识符列表模式  

<table><tr><td>标识符寄存器n</td><td>CAN_FxR1[15:8]</td><td colspan="4">CAN_FxR1[7:0]</td></tr><tr><td>屏蔽位寄存器n</td><td>CAN_FxR1[31:24]</td><td colspan="4">CAN_FxR1[23:16]</td></tr><tr><td>标识符寄存器n+1</td><td>CAN_FxR2[15:8]</td><td colspan="4">CAN_FxR2[7:0]</td></tr><tr><td>屏蔽位寄存器n+1</td><td>CAN_FxR2[31:24]</td><td colspan="4">CAN_FxR2[23:16]</td></tr><tr><td>映射</td><td>STID[10:3]</td><td>STID[2:0]</td><td>RTR</td><td>IDE</td><td>EXID[17:15]</td></tr></table>

报文进入 FIFO 邮箱中，会被应用程序读取并存放，通常应用程序根据报文标识符来区分报文数据。CAN 控制器对接收 FIFO 中通过不同过滤器筛选的报文，提供了过滤器编号，编号被存放在寄存器CAN_RXMDTRx的FMI[7:0]中，编号时不考虑过滤器组是否启用。编号规则详见图 22-4的示例。

当出现某个报文能通过多个过滤器的过滤，则接收邮箱中存放的过滤器编号根据过滤器优先级规则来决定存放哪个过滤器的编号，过滤器优先级规则如下：

所有 32 位的过滤器优先级均高于 16 位的过滤器  
$\bullet$ 对于同样宽度的过滤器，标识符列表的过滤器优先级高于屏蔽位模式的过滤器  
宽度和模式都一致的过滤器，编号小的过滤器优先级更高

如图 22-5 所示：在接收报文时，先把标识符与 32 位标识符列表模式过滤器进行匹配筛选，没有

匹配再与32位屏蔽位模式过滤器进行匹配筛选，没有匹配则继续与 16位标识符列表模式过滤器进行匹配筛选，没有匹配最后与16位屏蔽位模式过滤器进行匹配筛选，最后如果都没有匹配则丢弃报文，出现匹配则报文存入接收FIFO的邮箱，标识符编号存入寄存器 CAN_RXMDTRx的 FMI中。

![](images/0cad574bfd93a75f0695a651ce376282f0f114044aa87de32a6b4160d2efc9ce.jpg)  
图24-4 过滤器编号的示例

![](images/8f1ea5a4a6775986ffe7b3ad321af87c217a559fb87435daf09666ac1c8f955d.jpg)  
图 24-5 过滤器过滤示例

### 24.5.7 出错处理

CAN 控制器依靠状态错误寄存器CAN_ERRSR，对于总线上的出错管理。状态错误寄存器 CAN_ERRSR

里的TEC和REC，分别代表发送和接收错误计数值，根据随着收发错误的增加而增加，收发成功而减小，可以根据它们的值来判断CAN总线的稳定性。

当状态错误寄存器CAN_ERRSR里的TEC和REC小于128时，当前CAN节点处于错误主动状态，可以正常参与总线通信，并且在侦测到错误的时候发出主动错误标志。

当状态错误寄存器 CAN_ERRSR 里的 TEC 和 REC 大于 127 时，当前 CAN 节点处于错误被动状态，并且在侦测到错误的时候不允许发出主动错误标志，只能发出被动错误标志。

当状态错误寄存器 CAN_ERRSR 里的 TEC 大于 255 时，当前 CAN 节点进入离线状态。

当总线监视到 128 次出现 11 个连续的隐性位时，恢复到错误主动状态，该恢复方式受主控制寄存器 CAN_CTLR 里的 ABOM 位影响。若 ABOM 置 1，则硬件自动退出离线状态。若 ABOM 为 0，则需要软件操作 INRQ 位进入初识化模式，随后退出初始化，才能退出离线状态。

![](images/dcf5da02bfe2f99c366d2521d1e561c3e10d2e298331d20c5384525e3622b948.jpg)  
图 24-6 CAN 错误状态切换图

### 24.5.8 位时序

按照 CAN 总线的标准，将每一位时间分为四段：分别为同步段、传播时间段、相位缓冲段 1 和相位缓冲段 2。这些段由最小时间单元 Tq 组成。CAN 控制器通过采样来监测 CAN 总线变化，通过帧起始位的边沿进行同步

CAN 控制器把上述四段重新划分为三段，分别为：

l 同步段(SS)：也就是CAN标准里的同步段，固定为1 个最小时间单元，正常情况下所期望的位跳变发生在本时间段内。  
l 时间段1(BS1)：包含CAN标准里的传播时间段和相位缓冲段 1，可以被设置为包含1到 16最小时间单元，可以被自动延长，用于补偿CAN总线上不同节点频率精度误差带来的相位正向漂移。该时间段结束为采样点位置。  
l 时间段2(BS2)：也就是CAN标准里的相位缓冲段 2，可以被设置为 1到 8个最小时间单元，可以被自动缩短，以补偿CAN总线上不同节点频率精度误差带来的相位负向漂移。

重新同步跳转宽度(SJW)，是每位中可以延长和缩小的最小时间单元数量上限，范围可设置为 1到4个最小时间单元。

上述参数都可以在 CAN 总线时序寄存器 CAN_BTIMR 里配置。

![](images/38f6f52fcdb10a52191bcfd986924adb1a9627c860ef8b7e3f47b718a43c3d1a.jpg)  
图 24-7 跳变出现在 BS1 中

如图24-7，SJW为2，总线电平跳变在时间段 1 被检测到，则需要延长时间段 1 的长度，最大延长SJW，从而延迟采样点的位置。

![](images/da5bf99c40534f08cbe8c078b810edd77788085776c439d4799d6126d0310d5d.jpg)  
图 24-8 跳变出现在 BS2 中

如图24-8，SJW为2，总线电平跳变在时间段 2 被检测到，则需要缩小时间段 2 的长度，最大缩小SJW，从而提前采样点的位置。

CAN波特率计算公式为：

$$
\mathrm {C A N b p s} = \frac {\mathrm {t p c l k 1}}{(\mathrm {T S 1} [ 3 : 0 ] + 1 + \mathrm {T S 2} [ 2 : 0 ] + 1 + 1) \times \mathrm {B R P} [ 9 : 0 ]}
$$

这里 tpclk1 为 APB1 时钟周期，BRP[9:0]、TS1[3:0]、TS2[2:0]为 CANx_BTIMR 寄存器对应位。

## 24.6 CAN 中断

CAN 控制器有四个中断向量，分别为发送中断、FIFO_0 中断、FIFO_1 中断、错误及状态变化中断。

设置 CAN 中断允许寄存器 CAN_INTENR，可以允许或禁用各个中断源。

发送中断由发送邮箱变空事件产生，中断产生后，查询寄存器 CAN_TSTATR 的 RQCP0、RQCP1 和RQCP2位来判断是哪个邮箱变空事件产生。

FIFO0中断由接收新报文、接收邮箱变满和溢出事件产生，中断产生后，查询寄存器 CAN_RFIFO0的FMP0、FULL0和FOVER0位来判断是哪个邮箱变空事件产生。

FIFO1中断由接收新报文、接收邮箱变满和溢出事件产生，中断产生后，查询寄存器 CAN_RFIFO1

的 FMP1、FULL1 和 FOVER1 位来判断是哪个邮箱变空事件产生。

错误及状态变化中断由出错、唤醒和睡眠事件产生。

![](images/9e90d4d0dc2ad4b4aa5b90c38cde521c438e74eb519c540ea7f5445fbbdb3e32.jpg)  
图 24-9 CAN 中断逻辑图

![](images/92d82bdd8d0d2b2b3aff6ffa67a9a2107c75b7485d1c246c62568e016354a32f.jpg)

![](images/39af9d6fff555a48f8da8c28fcf4237a71da7f28ae4fcef42ef5dd8dc5434a80.jpg)

![](images/ed688c13dc15921e7f2d919f0f472408722404115dc847b103375dd60fb5d00e.jpg)

## 24.7 寄存器描述

CAN 控制器相关的寄存器必须用 32 位字的方式来操作。为了避免当前节点对整个 CAN 总线的影响，所以应用软件只能在初始化模式下修改位时序寄存器 CAN_BTIMR。

表 24-5 CAN1 相关寄存器列表  

<table><tr><td>名称</td><td>访问地址</td><td>描述</td><td>复位值</td></tr><tr><td>R32_CAN1_CTLR</td><td>0x40006400</td><td>CAN1 主控制寄存器</td><td>0x00010002</td></tr><tr><td>R32_CAN1_STATR</td><td>0x40006404</td><td>CAN1 主状态寄存器</td><td>0x00000C02</td></tr><tr><td>R32_CAN1_TSTATR</td><td>0x40006408</td><td>CAN1 发送状态寄存器</td><td>0x1C000000</td></tr><tr><td>R32_CAN1_RFIFO0</td><td>0x4000640C</td><td>CAN1 接收 FIF00 控制和状态寄存器</td><td>0x00000000</td></tr><tr><td>R32_CAN1_RFIFO01</td><td>0x40006410</td><td>CAN1 接收 FIF01 控制和状态寄存器</td><td>0x00000000</td></tr><tr><td>R32_CAN1_INTENR</td><td>0x40006414</td><td>CAN1中断使能寄存器</td><td>0x00000000</td></tr><tr><td>R32_CAN1_ERRSR</td><td>0x40006418</td><td>CAN1错误状态寄存器</td><td>0x00000000</td></tr><tr><td>R32_CAN1_BTIMR</td><td>0x4000641C</td><td>CAN1位时序寄存器</td><td>0x01230000</td></tr><tr><td>R32_CAN1_TTCTLR</td><td>0x40006420</td><td>CAN1时间触发控制寄存器</td><td>0x0000FFFF</td></tr><tr><td>R32_CAN1_TTCNT</td><td>0x40006424</td><td>CAN1时间触发计数值寄存器</td><td>0x00000000</td></tr></table>

表 24-6 CAN2 相关寄存器列表  

<table><tr><td>名称</td><td>访问地址</td><td>描述</td><td>复位值</td></tr><tr><td>R32_CAN2_CTLR</td><td>0x40006800</td><td>CAN2 主控制寄存器</td><td>0x00010002</td></tr><tr><td>R32_CAN2_STATR</td><td>0x40006804</td><td>CAN2 主状态寄存器</td><td>0x00000C02</td></tr><tr><td>R32_CAN2_TSTATR</td><td>0x40006808</td><td>CAN2 发送状态寄存器</td><td>0x1C000000</td></tr><tr><td>R32_CAN2_RFIFO0</td><td>0x4000680C</td><td>CAN2 接收 FIFO0 控制和状态寄存器</td><td>0x00000000</td></tr><tr><td>R32_CAN2_RFIFO01</td><td>0x40006810</td><td>CAN2 接收 FIFO01 控制和状态寄存器</td><td>0x00000000</td></tr><tr><td>R32_CAN2_INTENR</td><td>0x40006814</td><td>CAN2 中断使能寄存器</td><td>0x00000000</td></tr><tr><td>R32_CAN2_ERRSR</td><td>0x40006818</td><td>CAN2 错误状态寄存器</td><td>0x00000000</td></tr><tr><td>R32_CAN2_BTIMR</td><td>0x4000681C</td><td>CAN2 位时序寄存器</td><td>0x01230000</td></tr><tr><td>R32_CAN2_TTCTLR</td><td>0x40006820</td><td>CAN2 时间触发控制寄存器</td><td>0x0000FFFF</td></tr><tr><td>R32_CAN2_TTCNT</td><td>0x40006824</td><td>CAN2 时间触发计数值寄存器</td><td>0x00000000</td></tr></table>

表24-7 CAN1邮箱相关寄存器列表  

<table><tr><td>名称</td><td>访问地址</td><td>描述</td><td>复位值</td></tr><tr><td>R32_CAN1_TXMIRO</td><td>0x40006580</td><td>CAN1发送邮箱0标识符寄存器</td><td>X</td></tr><tr><td>R32_CAN1_TXMDTRO</td><td>0x40006584</td><td>CAN1发送邮箱0数据长度和时间戳寄存器</td><td>X</td></tr><tr><td>R32_CAN1_TXMDLRO</td><td>0x40006588</td><td>CAN1发送邮箱0低字节数据寄存器</td><td>X</td></tr><tr><td>R32_CAN1_TXMDHRO</td><td>0x4000658C</td><td>CAN1发送邮箱0高字节数据寄存器</td><td>X</td></tr><tr><td>R32_CAN1_TXMIR1</td><td>0x40006590</td><td>CAN1发送邮箱1标识符寄存器</td><td>X</td></tr><tr><td>R32_CAN1_TXMDTR1</td><td>0x40006594</td><td>CAN1发送邮箱1数据长度和时间戳寄存器</td><td>X</td></tr><tr><td>R32_CAN1_TXMDLR1</td><td>0x40006598</td><td>CAN1发送邮箱1低字节数据寄存器</td><td>X</td></tr><tr><td>R32_CAN1_TXMDHR1</td><td>0x4000659C</td><td>CAN1发送邮箱1高字节数据寄存器</td><td>X</td></tr><tr><td>R32_CAN1_TXMIR2</td><td>0x400065A0</td><td>CAN1发送邮箱2标识符寄存器</td><td>X</td></tr><tr><td>R32_CAN1_TXMDTR2</td><td>0x400065A4</td><td>CAN1发送邮箱2数据长度和时间戳寄存器</td><td>X</td></tr><tr><td>R32_CAN1_TXMDLR2</td><td>0x400065A8</td><td>CAN1发送邮箱2低字节数据寄存器</td><td>X</td></tr><tr><td>R32_CAN1_TXMDHR2</td><td>0x400065AC</td><td>CAN1发送邮箱2高字节数据寄存器</td><td>X</td></tr><tr><td>R32_CAN1_RXMIRO</td><td>0x400065B0</td><td>CAN1接收FIFO00邮箱标识符寄存器</td><td>X</td></tr><tr><td>R32_CAN1_RXMDTRO</td><td>0x400065B4</td><td>CAN1接收FIFO00邮箱数据长度和时间戳寄存器</td><td>X</td></tr><tr><td>R32_CAN1_RXMDLR0</td><td>0x400065B8</td><td>CAN1接收FIFO00邮箱低字节数据寄存器</td><td>X</td></tr><tr><td>R32_CAN1_RXMDHR0</td><td>0x400065BC</td><td>CAN1接收FIFO00邮箱高字节数据寄存器</td><td>X</td></tr><tr><td>R32_CAN1_RXMIR1</td><td>0x400065C0</td><td>CAN1接收FIFO01邮箱标识符寄存器</td><td>X</td></tr><tr><td>R32_CAN1_RXMDTR1</td><td>0x400065C4</td><td>CAN1接收FIFO01邮箱数据长度和时间戳寄存器</td><td>X</td></tr><tr><td>R32_CAN1_RXMDLR1</td><td>0x400065C8</td><td>CAN1接收FIFO01邮箱低字节数据寄存器</td><td>X</td></tr><tr><td>R32_CAN1_RXMDHR1</td><td>0x400065CC</td><td>CAN1接收FIFO01邮箱高字节数据寄存器</td><td>X</td></tr></table>

表24-8 CAN2邮箱相关寄存器列表  

<table><tr><td>名称</td><td>访问地址</td><td>描述</td><td>复位值</td></tr><tr><td>R32_CAN2_TXMIRO</td><td>0x40006980</td><td>CAN2发送邮箱0标识符寄存器</td><td>X</td></tr><tr><td>R32_CAN2_TXMDTRO</td><td>0x40006984</td><td>CAN2发送邮箱0数据长度和时间戳寄存器</td><td>X</td></tr><tr><td>R32_CAN2_TXMDLRO</td><td>0x40006988</td><td>CAN2发送邮箱0低字节数据寄存器</td><td>X</td></tr><tr><td>R32_CAN2_TXMDHRO</td><td>0x4000698C</td><td>CAN2发送邮箱0高字节数据寄存器</td><td>X</td></tr><tr><td>R32_CAN2_TXMIR1</td><td>0x40006990</td><td>CAN2发送邮箱1标识符寄存器</td><td>X</td></tr><tr><td>R32_CAN2_TXMDTR1</td><td>0x40006994</td><td>CAN2发送邮箱1数据长度和时间戳寄存器</td><td>X</td></tr><tr><td>R32_CAN2_TXMDLR1</td><td>0x40006998</td><td>CAN2发送邮箱1低字节数据寄存器</td><td>X</td></tr><tr><td>R32_CAN2_TXMDHR1</td><td>0x4000699C</td><td>CAN2发送邮箱1高字节数据寄存器</td><td>X</td></tr><tr><td>R32_CAN2_TXMIR2</td><td>0x400069A0</td><td>CAN2发送邮箱2标识符寄存器</td><td>X</td></tr><tr><td>R32_CAN2_TXMDTR2</td><td>0x400069A4</td><td>CAN2发送邮箱2数据长度和时间戳寄存器</td><td>X</td></tr><tr><td>R32_CAN2_TXMDLR2</td><td>0x400069A8</td><td>CAN2发送邮箱2低字节数据寄存器</td><td>X</td></tr><tr><td>R32_CAN2_TXMDHR2</td><td>0x400069AC</td><td>CAN2发送邮箱2高字节数据寄存器</td><td>X</td></tr><tr><td>R32_CAN2_RXMIRO</td><td>0x400069B0</td><td>CAN2接收FIFO00邮箱标识符寄存器</td><td>X</td></tr><tr><td>R32_CAN2_RXMDTRO</td><td>0x400069B4</td><td>CAN2接收FIFO00邮箱数据长度和时间戳寄存器</td><td>X</td></tr><tr><td>R32_CAN2_RXMDLR0</td><td>0x400069B8</td><td>CAN2接收FIFO00邮箱低字节数据寄存器</td><td>X</td></tr><tr><td>R32_CAN2_RXMDHRO</td><td>0x400069BC</td><td>CAN2接收FIFO00邮箱高字节数据寄存器</td><td>X</td></tr><tr><td>R32_CAN2_RXMIR1</td><td>0x400069C0</td><td>CAN2接收FIFO01邮箱标识符寄存器</td><td>X</td></tr><tr><td>R32_CAN2_RXMDTR1</td><td>0x400069C4</td><td>CAN2接收FIFO01邮箱数据长度和时间戳寄存器</td><td>X</td></tr><tr><td>R32_CAN2_RXMDLR1</td><td>0x400069C8</td><td>CAN2接收FIFO01邮箱低字节数据寄存器</td><td>X</td></tr><tr><td>R32_CAN2_RXMDHR1</td><td>0x400069CC</td><td>CAN2接收FIFO01邮箱高字节数据寄存器</td><td>X</td></tr></table>

表 24-9 CAN1 过滤器相关寄存器列表  

<table><tr><td>名称</td><td>访问地址</td><td>描述</td><td>复位值</td></tr><tr><td>R32_CAN1_FCTLR</td><td>0x40006600</td><td>CAN1 过滤器主控制寄存器</td><td>0x2A1C0E01</td></tr><tr><td>R32_CAN1_FMCFGR</td><td>0x40006604</td><td>CAN1 过滤器模式寄存器</td><td>0x00000000</td></tr><tr><td>R32_CAN1_FSCFGR</td><td>0x4000660C</td><td>CAN1 过滤器位宽寄存器</td><td>0x00000000</td></tr><tr><td>R32_CAN1_FAFIFOR</td><td>0x40006614</td><td>CAN1 过滤器 FIFO 关联寄存器</td><td>0x00000000</td></tr><tr><td>R32_CAN1_FWR</td><td>0x4000661C</td><td>CAN1 过滤器激活寄存器</td><td>0x00000000</td></tr><tr><td>R32_CAN1_FOR1</td><td>0x40006640</td><td>CAN1 过滤器组0寄存器1</td><td>X</td></tr><tr><td>R32_CAN1_FOR2</td><td>0x40006644</td><td>CAN1 过滤器组0寄存器2</td><td>X</td></tr><tr><td>R32_CAN1_F1R1</td><td>0x40006648</td><td>CAN1 过滤器组1寄存器1</td><td>X</td></tr><tr><td>R32_CAN1_F1R2</td><td>0x4000664C</td><td>CAN1 过滤器组1寄存器2</td><td>X</td></tr><tr><td>R32_CAN1_F2R1</td><td>0x40006650</td><td>CAN1 过滤器组2寄存器1</td><td>X</td></tr><tr><td>R32_CAN1_F2R2</td><td>0x40006654</td><td>CAN1 过滤器组2寄存器2</td><td>X</td></tr><tr><td>R32_CAN1_F3R1</td><td>0x40006658</td><td>CAN1 过滤器组3寄存器1</td><td>X</td></tr><tr><td>R32_CAN1_F3R2</td><td>0x4000665C</td><td>CAN1 过滤器组3寄存器2</td><td>X</td></tr><tr><td>R32_CAN1_F4R1</td><td>0x40006660</td><td>CAN1 过滤器组4寄存器1</td><td>X</td></tr><tr><td>R32_CAN1_F4R2</td><td>0x40006664</td><td>CAN1 过滤器组4寄存器2</td><td>X</td></tr><tr><td>R32_CAN1_F5R1</td><td>0x40006668</td><td>CAN1 过滤器组5寄存器1</td><td>X</td></tr><tr><td>R32_CAN1_F5R2</td><td>0x4000666C</td><td>CAN1 过滤器组 5 寄存器 2</td><td>X</td></tr><tr><td>R32_CAN1_F6R1</td><td>0x40006670</td><td>CAN1 过滤器组 6 寄存器 1</td><td>X</td></tr><tr><td>R32_CAN1_F6R2</td><td>0x40006674</td><td>CAN1 过滤器组 6 寄存器 2</td><td>X</td></tr><tr><td>R32_CAN1_F7R1</td><td>0x40006678</td><td>CAN1 过滤器组 7 寄存器 1</td><td>X</td></tr><tr><td>R32_CAN1_F7R2</td><td>0x4000667C</td><td>CAN1 过滤器组 7 寄存器 2</td><td>X</td></tr><tr><td>R32_CAN1_F8R1</td><td>0x40006680</td><td>CAN1 过滤器组 8 寄存器 1</td><td>X</td></tr><tr><td>R32_CAN1_F8R2</td><td>0x40006684</td><td>CAN1 过滤器组 8 寄存器 2</td><td>X</td></tr><tr><td>R32_CAN1_F9R1</td><td>0x40006688</td><td>CAN1 过滤器组 9 寄存器 1</td><td>X</td></tr><tr><td>R32_CAN1_F9R2</td><td>0x4000668C</td><td>CAN1 过滤器组 9 寄存器 2</td><td>X</td></tr><tr><td>R32_CAN1_F10R1</td><td>0x40006690</td><td>CAN1 过滤器组 10 寄存器 1</td><td>X</td></tr><tr><td>R32_CAN1_F10R2</td><td>0x40006694</td><td>CAN1 过滤器组 10 寄存器 2</td><td>X</td></tr><tr><td>R32_CAN1_F11R1</td><td>0x40006698</td><td>CAN1 过滤器组 11 寄存器 1</td><td>X</td></tr><tr><td>R32_CAN1_F11R2</td><td>0x4000669C</td><td>CAN1 过滤器组 11 寄存器 2</td><td>X</td></tr><tr><td>R32_CAN1_F12R1</td><td>0x400066A0</td><td>CAN1 过滤器组 12 寄存器 1</td><td>X</td></tr><tr><td>R32_CAN1_F12R2</td><td>0x400066A4</td><td>CAN1 过滤器组 12 寄存器 2</td><td>X</td></tr><tr><td>R32_CAN1_F13R1</td><td>0x400066A8</td><td>CAN1 过滤器组 13 寄存器 1</td><td>X</td></tr><tr><td>R32_CAN1_F13R2</td><td>0x400066AC</td><td>CAN1 过滤器组 13 寄存器 2</td><td>X</td></tr><tr><td>R32_CAN1_F14R1</td><td>0x400066B0</td><td>CAN1 过滤器组 14 寄存器 1</td><td>X</td></tr><tr><td>R32_CAN1_F14R2</td><td>0x400066B4</td><td>CAN1 过滤器组 14 寄存器 2</td><td>X</td></tr><tr><td>R32_CAN1_F15R1</td><td>0x400066B8</td><td>CAN1 过滤器组 15 寄存器 1</td><td>X</td></tr><tr><td>R32_CAN1_F15R2</td><td>0x400066BC</td><td>CAN1 过滤器组 15 寄存器 2</td><td>X</td></tr><tr><td>R32_CAN1_F16R1</td><td>0x400066C0</td><td>CAN1 过滤器组 16 寄存器 1</td><td>X</td></tr><tr><td>R32_CAN1_F16R2</td><td>0x400066C4</td><td>CAN1 过滤器组 16 寄存器 2</td><td>X</td></tr><tr><td>R32_CAN1_F17R1</td><td>0x400066C8</td><td>CAN1 过滤器组 17 寄存器 1</td><td>X</td></tr><tr><td>R32_CAN1_F17R2</td><td>0x400066CC</td><td>CAN1 过滤器组 17 寄存器 2</td><td>X</td></tr><tr><td>R32_CAN1_F18R1</td><td>0x400066D0</td><td>CAN1 过滤器组 18 寄存器 1</td><td>X</td></tr><tr><td>R32_CAN1_F18R2</td><td>0x400066D4</td><td>CAN1 过滤器组 18 寄存器 2</td><td>X</td></tr><tr><td>R32_CAN1_F19R1</td><td>0x400066D8</td><td>CAN1 过滤器组 19 寄存器 1</td><td>X</td></tr><tr><td>R32_CAN1_F19R2</td><td>0x400066DC</td><td>CAN1 过滤器组 19 寄存器 2</td><td>X</td></tr><tr><td>R32_CAN1_F20R1</td><td>0x400066E0</td><td>CAN1 过滤器组 20 寄存器 1</td><td>X</td></tr><tr><td>R32_CAN1_F20R2</td><td>0x400066E4</td><td>CAN1 过滤器组 20 寄存器 2</td><td>X</td></tr><tr><td>R32_CAN1_F21R1</td><td>0x400066E8</td><td>CAN1 过滤器组 21 寄存器 1</td><td>X</td></tr><tr><td>R32_CAN1_F21R2</td><td>0x400066EC</td><td>CAN1 过滤器组 21 寄存器 2</td><td>X</td></tr><tr><td>R32_CAN1_F22R1</td><td>0x400066F0</td><td>CAN1 过滤器组 22 寄存器 1</td><td>X</td></tr><tr><td>R32_CAN1_F22R2</td><td>0x400066F4</td><td>CAN1 过滤器组 22 寄存器 2</td><td>X</td></tr><tr><td>R32_CAN1_F23R1</td><td>0x400066F8</td><td>CAN1 过滤器组 23 寄存器 1</td><td>X</td></tr><tr><td>R32_CAN1_F23R2</td><td>0x400066FC</td><td>CAN1 过滤器组 23 寄存器 2</td><td>X</td></tr><tr><td>R32_CAN1_F24R1</td><td>0x40006700</td><td>CAN1 过滤器组 24 寄存器 1</td><td>X</td></tr><tr><td>R32_CAN1_F24R2</td><td>0x40006704</td><td>CAN1 过滤器组 24 寄存器 2</td><td>X</td></tr><tr><td>R32_CAN1_F25R1</td><td>0x40006708</td><td>CAN1 过滤器组 25 寄存器 1</td><td>X</td></tr><tr><td>R32_CAN1_F25R2</td><td>0x4000670C</td><td>CAN1 过滤器组 25 寄存器 2</td><td>X</td></tr><tr><td>R32_CAN1_F26R1</td><td>0x40006710</td><td>CAN1 过滤器组 26 寄存器 1</td><td>X</td></tr><tr><td>R32_CAN1_F26R2</td><td>0x40006714</td><td>CAN1 过滤器组 26 寄存器 2</td><td>X</td></tr><tr><td>R32_CAN1_F27R1</td><td>0x40006718</td><td>CAN1 过滤器组 27 寄存器 1</td><td>X</td></tr><tr><td>R32_CAN1_F27R2</td><td>0x4000671C</td><td>CAN1 过滤器组 27 寄存器 2</td><td>X</td></tr></table>

表24-10 CAN2过滤器相关寄存器列表  

<table><tr><td>名称</td><td>访问地址</td><td>描述</td><td>复位值</td></tr><tr><td>R32_CAN2_FCTLR</td><td>0x40006A00</td><td>CAN2 过滤器主控制寄存器</td><td>0x2A1C0E01</td></tr><tr><td>R32_CAN2_FMCFGR</td><td>0x40006A04</td><td>CAN2 过滤器模式寄存器</td><td>0x00000000</td></tr><tr><td>R32_CAN2_FSCFGR</td><td>0x40006A0C</td><td>CAN2 过滤器位宽寄存器</td><td>0x00000000</td></tr><tr><td>R32_CAN2_FAFIFOR</td><td>0x40006A14</td><td>CAN2 过滤器 FIFO 关联寄存器</td><td>0x00000000</td></tr><tr><td>R32_CAN2_FWR</td><td>0x40006A1C</td><td>CAN2 过滤器激活寄存器</td><td>0x00000000</td></tr><tr><td>R32_CAN2_FOR1</td><td>0x40006A40</td><td>CAN2 过滤器组 0 寄存器 1</td><td>X</td></tr><tr><td>R32_CAN2_FOR2</td><td>0x40006A44</td><td>CAN2 过滤器组 0 寄存器 2</td><td>X</td></tr><tr><td>R32_CAN2_F1R1</td><td>0x40006A48</td><td>CAN2 过滤器组 1 寄存器 1</td><td>X</td></tr><tr><td>R32_CAN2_F1R2</td><td>0x40006A4C</td><td>CAN2 过滤器组 1 寄存器 2</td><td>X</td></tr><tr><td>R32_CAN2_F2R1</td><td>0x40006A50</td><td>CAN2 过滤器组 2 寄存器 1</td><td>X</td></tr><tr><td>R32_CAN2_F2R2</td><td>0x40006A54</td><td>CAN2 过滤器组 2 寄存器 2</td><td>X</td></tr><tr><td>R32_CAN2_F3R1</td><td>0x40006A58</td><td>CAN2 过滤器组 3 寄存器 1</td><td>X</td></tr><tr><td>R32_CAN2_F3R2</td><td>0x40006A5C</td><td>CAN2 过滤器组 3 寄存器 2</td><td>X</td></tr><tr><td>R32_CAN2_F4R1</td><td>0x40006A60</td><td>CAN2 过滤器组 4 寄存器 1</td><td>X</td></tr><tr><td>R32_CAN2_F4R2</td><td>0x40006A64</td><td>CAN2 过滤器组 4 寄存器 2</td><td>X</td></tr><tr><td>R32_CAN2_F5R1</td><td>0x40006A68</td><td>CAN2 过滤器组 5 寄存器 1</td><td>X</td></tr><tr><td>R32_CAN2_F5R2</td><td>0x40006A6C</td><td>CAN2 过滤器组 5 寄存器 2</td><td>X</td></tr><tr><td>R32_CAN2_F6R1</td><td>0x40006A70</td><td>CAN2 过滤器组 6 寄存器 1</td><td>X</td></tr><tr><td>R32_CAN2_F6R2</td><td>0x40006A74</td><td>CAN2 过滤器组 6 寄存器 2</td><td>X</td></tr><tr><td>R32_CAN2_F7R1</td><td>0x40006A78</td><td>CAN2 过滤器组 7 寄存器 1</td><td>X</td></tr><tr><td>R32_CAN2_F7R2</td><td>0x40006A7C</td><td>CAN2 过滤器组 7 寄存器 2</td><td>X</td></tr><tr><td>R32_CAN2_F8R1</td><td>0x40006A80</td><td>CAN2 过滤器组 8 寄存器 1</td><td>X</td></tr><tr><td>R32_CAN2_F8R2</td><td>0x40006A84</td><td>CAN2 过滤器组 8 寄存器 2</td><td>X</td></tr><tr><td>R32_CAN2_F9R1</td><td>0x40006A88</td><td>CAN2 过滤器组 9 寄存器 1</td><td>X</td></tr><tr><td>R32_CAN2_F9R2</td><td>0x40006A8C</td><td>CAN2 过滤器组 9 寄存器 2</td><td>X</td></tr><tr><td>R32_CAN2_F10R1</td><td>0x40006A90</td><td>CAN2 过滤器组 10 寄存器 1</td><td>X</td></tr><tr><td>R32_CAN2_F10R2</td><td>0x40006A94</td><td>CAN2 过滤器组 10 寄存器 2</td><td>X</td></tr><tr><td>R32_CAN2_F11R1</td><td>0x40006A98</td><td>CAN2 过滤器组 11 寄存器 1</td><td>X</td></tr><tr><td>R32_CAN2_F11R2</td><td>0x40006A9C</td><td>CAN2 过滤器组 11 寄存器 2</td><td>X</td></tr><tr><td>R32_CAN2_F12R1</td><td>0x40006AA0</td><td>CAN2 过滤器组 12 寄存器 1</td><td>X</td></tr><tr><td>R32_CAN2_F12R2</td><td>0x40006AA4</td><td>CAN2 过滤器组 12 寄存器 2</td><td>X</td></tr><tr><td>R32_CAN2_F13R1</td><td>0x40006AA8</td><td>CAN2 过滤器组 13 寄存器 1</td><td>X</td></tr><tr><td>R32_CAN2_F13R2</td><td>0x40006AAC</td><td>CAN2 过滤器组 13 寄存器 2</td><td>X</td></tr><tr><td>R32_CAN2_F14R1</td><td>0x40006AB0</td><td>CAN2 过滤器组 14 寄存器 1</td><td>X</td></tr><tr><td>R32_CAN2_F14R2</td><td>0x40006AB4</td><td>CAN2 过滤器组 14 寄存器 2</td><td>X</td></tr><tr><td>R32_CAN2_F15R1</td><td>0x40006AB8</td><td>CAN2 过滤器组 15 寄存器 1</td><td>X</td></tr><tr><td>R32_CAN2_F15R2</td><td>0x40006ABC</td><td>CAN2 过滤器组 15 寄存器 2</td><td>X</td></tr><tr><td>R32_CAN2_F16R1</td><td>0x40006AC0</td><td>CAN2 过滤器组 16 寄存器 1</td><td>X</td></tr><tr><td>R32_CAN2_F16R2</td><td>0x40006AC4</td><td>CAN2 过滤器组 16 寄存器 2</td><td>X</td></tr><tr><td>R32_CAN2_F17R1</td><td>0x40006AC8</td><td>CAN2 过滤器组 17 寄存器 1</td><td>X</td></tr><tr><td>R32_CAN2_F17R2</td><td>0x40006ACC</td><td>CAN2 过滤器组 17 寄存器 2</td><td>X</td></tr><tr><td>R32_CAN2_F18R1</td><td>0x40006AD0</td><td>CAN2 过滤器组 18 寄存器 1</td><td>X</td></tr><tr><td>R32_CAN2_F18R2</td><td>0x40006AD4</td><td>CAN2 过滤器组 18 寄存器 2</td><td>X</td></tr><tr><td>R32_CAN2_F19R1</td><td>0x40006AD8</td><td>CAN2 过滤器组 19 寄存器 1</td><td>X</td></tr><tr><td>R32_CAN2_F19R2</td><td>0x40006ADC</td><td>CAN2 过滤器组 19 寄存器 2</td><td>X</td></tr><tr><td>R32_CAN2_F20R1</td><td>0x40006AEO</td><td>CAN2 过滤器组 20 寄存器 1</td><td>X</td></tr><tr><td>R32_CAN2_F20R2</td><td>0x40006AE4</td><td>CAN2 过滤器组 20 寄存器 2</td><td>X</td></tr><tr><td>R32_CAN2_F21R1</td><td>0x40006AE8</td><td>CAN2 过滤器组 21 寄存器 1</td><td>X</td></tr><tr><td>R32_CAN2_F21R2</td><td>0x40006AEC</td><td>CAN2 过滤器组 21 寄存器 2</td><td>X</td></tr><tr><td>R32_CAN2_F22R1</td><td>0x40006AFO</td><td>CAN2 过滤器组 22 寄存器 1</td><td>X</td></tr><tr><td>R32_CAN2_F22R2</td><td>0x40006AF4</td><td>CAN2 过滤器组 22 寄存器 2</td><td>X</td></tr><tr><td>R32_CAN2_F23R1</td><td>0x40006AF8</td><td>CAN2 过滤器组 23 寄存器 1</td><td>X</td></tr><tr><td>R32_CAN2_F23R2</td><td>0x40006AFC</td><td>CAN2 过滤器组 23 寄存器 2</td><td>X</td></tr><tr><td>R32_CAN2_F24R1</td><td>0x40006B00</td><td>CAN2 过滤器组 24 寄存器 1</td><td>X</td></tr><tr><td>R32_CAN2_F24R2</td><td>0x40006B04</td><td>CAN2 过滤器组 24 寄存器 2</td><td>X</td></tr><tr><td>R32_CAN2_F25R1</td><td>0x40006B08</td><td>CAN2 过滤器组 25 寄存器 1</td><td>X</td></tr><tr><td>R32_CAN2_F25R2</td><td>0x40006B0C</td><td>CAN2 过滤器组 25 寄存器 2</td><td>X</td></tr><tr><td>R32_CAN2_F26R1</td><td>0x40006B10</td><td>CAN2 过滤器组 26 寄存器 1</td><td>X</td></tr><tr><td>R32_CAN2_F26R2</td><td>0x40006B14</td><td>CAN2 过滤器组 26 寄存器 2</td><td>X</td></tr><tr><td>R32_CAN2_F27R1</td><td>0x40006B18</td><td>CAN2 过滤器组 27 寄存器 1</td><td>X</td></tr><tr><td>R32_CAN2_F27R2</td><td>0x40006B1C</td><td>CAN2 过滤器组 27 寄存器 2</td><td>X</td></tr></table>

### 24.7.1 CANx 主控制寄存器（CANx_CTLR）（x=1/2）

偏移地址： $0 \times 0 0$

<table><tr><td>31</td><td>30</td><td>29</td><td>28</td><td>27</td><td>26</td><td>25</td><td>24</td><td>23</td><td>22</td><td>21</td><td>20</td><td>19</td><td>18</td><td>17</td><td>16</td></tr><tr><td colspan="15">Reserved</td><td>DBF</td></tr><tr><td>15</td><td>14</td><td>13</td><td>12</td><td>11</td><td>10</td><td>9</td><td>8</td><td>7</td><td>6</td><td>5</td><td>4</td><td>3</td><td>2</td><td>1</td><td>0</td></tr><tr><td>RST</td><td colspan="7">Reserved</td><td>TTCM</td><td>ABOM</td><td>AWUM</td><td>NART</td><td>RFLM</td><td>TXFP</td><td>SLEEP</td><td>INRQ</td></tr></table>

<table><tr><td>位</td><td>名称</td><td>访问</td><td>描述</td><td>复位值</td></tr><tr><td>[31:17]</td><td>Reserved</td><td>R0</td><td>保留。</td><td>0</td></tr><tr><td>16</td><td>DBF</td><td>RW</td><td>调试是否禁止CAN总线工作1:调试时,CAN的收发被禁止,但是接收FIFO0的控制和读写操作一切正常;0:调试时,CAN控制器正常工作。</td><td>1</td></tr><tr><td>15</td><td>RST</td><td>RW1</td><td>CAN控制器软件复位请求,该位写0无效1:对CAN控制器进行复位,复位后控制器进入睡眠模式,然后硬件自动清0;0:CAN控制器正常状态。</td><td>0</td></tr><tr><td>[14:8]</td><td>Reserved</td><td>R0</td><td>保留</td><td>0</td></tr><tr><td>7</td><td>TTCM</td><td>RW</td><td>是否允许时间触发模式1:使能时间触发模式;0:禁止时间触发模式。时间触发模式主要是配合TTCAN协议使用。</td><td>0</td></tr><tr><td>6</td><td>ABOM</td><td>RW</td><td>离线自动退出控制1:硬件检测到128次连续11个隐性位,自</td><td>0</td></tr><tr><td></td><td></td><td></td><td>动退出离线状态;0:需要软件操作寄存器 CAN_CTLR 的 INRQ 位置 1 然后清 0,当检测到 128 次连续 11 个隐性位后,退出离线状态。</td><td></td></tr><tr><td>5</td><td>AWUM</td><td>RW</td><td>CAN 控制器自动唤醒使能1:当检测到报文时,硬件自动唤醒,寄存器CAN_STATR 的 SLEEP 和 SLAK 位自动清 0;0:需要软件操作寄存器 CAN_CTLR 的 SLEEP 位清 0,唤醒 CAN 控制器。</td><td>0</td></tr><tr><td>4</td><td>NART</td><td>RW</td><td>报文自动重传功能禁止1:无论发送成功与否,报文只能被发送一次;0:CAN 控制器一直重传至发送成功为止。</td><td>0</td></tr><tr><td>3</td><td>RFLM</td><td>RW</td><td>接收 FIFO 报文锁定模式使能1:当接收 FIFO 溢出时,已接收邮箱报文未读出,邮箱未释放时,新接收到的报文被丢弃;0:当接收 FIFO 溢出时,已接收邮箱报文未读出,邮箱未释放时,新接收到的报文会覆盖原有报文。</td><td>0</td></tr><tr><td>2</td><td>TXFP</td><td>RW</td><td>发送邮箱优先级方式选择1:优先级由发送请求的先后顺序决定;0:优先级由报文标识符来决定。</td><td>0</td></tr><tr><td>1</td><td>SLEEP</td><td>RW</td><td>睡眠模式请求位1:置1 请求 CAN 控制器进入睡眠模式,当前活动完成后,控制器进入睡眠模式,若 AWUM 位置 1 ,则在接收到报文时,控制器把 SLEEP 位清 0;0:软件清 0 后,控制器退出睡眠模式。</td><td>1</td></tr><tr><td>0</td><td>INRQ</td><td>RW</td><td>初始化模式请求位1:置1 请求 CAN 控制器进入初始化模式,当前活动完成后,控制器进入初始化模式,硬件对寄存器 CAN_STATR 的 INAK 位置 1 ;0:置0 请求 CAN 控制器退出初始化模式,进入正常模式,硬件对寄存器 CAN_STATR 的 INAK 位清 0。</td><td>0</td></tr></table>

### 24.7.2 CANx 主状态寄存器（CANx_STATR）（x=1/2）

偏移地址： $0 \times 0 4$

<table><tr><td>31</td><td>30</td><td>29</td><td>28</td><td>27</td><td>26</td><td>25</td><td>24</td><td>23</td><td>22</td><td>21</td><td>20</td><td>19</td><td>18</td><td>17</td><td>16</td></tr><tr><td colspan="16">Reserved</td></tr><tr><td>15</td><td>14</td><td>13</td><td>12</td><td>11</td><td>10</td><td>9</td><td>8</td><td>7</td><td>6</td><td>5</td><td>4</td><td>3</td><td>2</td><td>1</td><td>0</td></tr><tr><td colspan="3">Reserved</td><td>RX</td><td>SAMP</td><td>RXM</td><td>TXM</td><td colspan="3">Reserved</td><td>SLAKI</td><td>WKUI</td><td>ERRI</td><td>SLAK</td><td>INAK</td><td></td></tr></table>

<table><tr><td>位</td><td>名称</td><td>访问</td><td>描述</td><td>复位值</td></tr><tr><td>[31:12]</td><td>Reserved</td><td>R0</td><td>保留。</td><td>0</td></tr><tr><td>11</td><td>RX</td><td>R0</td><td>CAN控制器接收引脚RX当前实际电平。</td><td>1</td></tr><tr><td>10</td><td>SAMP</td><td>R0</td><td>CAN控制器接收引脚RX上一个接收位的电平</td><td>1</td></tr><tr><td>9</td><td>RXM</td><td>R0</td><td>接收模式查询位1:当前CAN控制器为接收模式;0:当前CAN控制器非接收模式。</td><td>0</td></tr><tr><td>8</td><td>TXM</td><td>R0</td><td>发送模式查询位1:当前CAN控制器为发送模式;0:当前CAN控制器非发送模式。</td><td>0</td></tr><tr><td>[7:5]</td><td>Reserved</td><td>R0</td><td>保留</td><td>0</td></tr><tr><td>4</td><td>SLAKI</td><td>RW1</td><td>睡眠中断使能时,即寄存器CAN_INTENR的SLKIE位置1时,中断产生标志位,写1清0,写0无效。1:进入睡眠模式时,中断产生,硬件置1;0:退出睡眠模式时,硬件清0也可软件清0。</td><td>0</td></tr><tr><td>3</td><td>WKUI</td><td>RW1</td><td>唤醒中断标志位。当寄存器CAN_INTENR的WKUI位置1时,若CAN控制器处于睡眠模式时,检测到SOF位,则硬件置1。软件置1清0,置0无效。</td><td>0</td></tr><tr><td>2</td><td>ERRI</td><td>RW1</td><td>出错中断状态标志位。当寄存器CAN_INTENR的ERRIE位置1时,产生错误及状态变化中断。该位软件置1清0,置0无效。</td><td>0</td></tr><tr><td>1</td><td>SLAK</td><td>R0</td><td>睡眠模式指示位。1:CAN控制器正处于睡眠模式;0:CAN控制器不在睡眠模式。</td><td>1</td></tr><tr><td>0</td><td>INAK</td><td>R0</td><td>初始化模式指示位。1:CAN控制器正在初始化模式;0:CAN控制器工作在非初始化模式。</td><td>0</td></tr></table>

### 24.7.3 CANx 发送状态寄存器（CANx_TSTATR）（x=1/2）

偏移地址： $0 \times 0 8$

<table><tr><td>31</td><td>30</td><td>29</td><td>28</td><td>27</td><td>26</td><td>25</td><td>24</td><td>23</td><td>22</td><td>21</td><td>20</td><td>19</td><td>18</td><td>17</td><td>16</td></tr><tr><td>LOW2</td><td>LOW1</td><td>LOWO</td><td>TME2</td><td>TME1</td><td>TME0</td><td colspan="2">CODE[1:0]</td><td>ABRQ2</td><td colspan="3">Reserved</td><td>TERR2</td><td>ALST2</td><td>TXOK2</td><td>RQCP2</td></tr><tr><td>15</td><td>14</td><td>13</td><td>12</td><td>11</td><td>10</td><td>9</td><td>8</td><td>7</td><td>6</td><td>5</td><td>4</td><td>3</td><td>2</td><td>1</td><td>0</td></tr><tr><td>ABRQ1</td><td colspan="3">Reserved</td><td>TERR1</td><td>ALST1</td><td>TXOK1</td><td>RQCP1</td><td>ABRQ0</td><td colspan="3">Reserved</td><td>TERRO</td><td>ALST0</td><td>TXOK0</td><td>RQCP0</td></tr></table>

<table><tr><td>位</td><td>名称</td><td>访问</td><td>描述</td><td>复位值</td></tr><tr><td>31</td><td>LOW2</td><td>R0</td><td>发送邮箱2的最低优先级标志位1:表示发送邮箱2的优先级最低;0:表示发送邮箱2的优先级非最低。</td><td>0</td></tr><tr><td>30</td><td>LOW1</td><td>R0</td><td>发送邮箱1的最低优先级标志位1:表示发送邮箱1的优先级最低;0:表示发送邮箱1的优先级非最低。</td><td>0</td></tr><tr><td>29</td><td>LOWO</td><td>R0</td><td>发送邮箱0的最低优先级标志位1:表示发送邮箱0的优先级最低;</td><td>0</td></tr><tr><td></td><td></td><td></td><td>0:表示发送邮箱0的优先级非最低。</td><td></td></tr><tr><td>28</td><td>TME2</td><td>R0</td><td>发送邮箱2的空标志位1:表示发送邮箱2无等待发送报文;0:表示发送邮箱2有等待发送报文。</td><td>1</td></tr><tr><td>27</td><td>TME1</td><td>R0</td><td>发送邮箱1的空标志位1:表示发送邮箱1无等待发送报文;0:表示发送邮箱1有等待发送报文。</td><td>1</td></tr><tr><td>26</td><td>TME0</td><td>R0</td><td>发送邮箱0的空标志位1:表示发送邮箱0无等待发送报文;0:表示发送邮箱0有等待发送报文。</td><td>1</td></tr><tr><td>[25:24]</td><td>CODE[1:0]</td><td>R0</td><td>邮箱编号当有1个以上邮箱为空时,表示下一个为空的邮箱号;当邮箱全空时,表示优先级最低的邮箱号。</td><td>0</td></tr><tr><td>23</td><td>ABRQ2</td><td>RW1</td><td>发送邮箱2的发送中止请求。软件置1,可以中止邮箱2的发送请求,发送报文被清除时硬件清0,若邮箱2清空,软件置1无效。</td><td>0</td></tr><tr><td>[22:20]</td><td>Reserved</td><td>R0</td><td>保留</td><td>0</td></tr><tr><td>19</td><td>TERR2</td><td>RW1</td><td>发送邮箱2发送失败标志位,当发送邮箱2发送失败,该位自动置1。软件置1清0,软件写0无效。</td><td>0</td></tr><tr><td>18</td><td>ALST2</td><td>RW1</td><td>发送邮箱2仲裁失败标志位,当发送邮箱2仲裁优先级低导致发送失败,该位自动置1。软件置1清0,软件写0无效。</td><td>0</td></tr><tr><td>17</td><td>TXOK2</td><td>RW1</td><td>发送邮箱2发送成功标志位1:上次发送成功;0:上次发送失败。软件置1清0,软件写0无效。</td><td>0</td></tr><tr><td>16</td><td>RQCP2</td><td>RW</td><td>发送邮箱2请求完成标志位,当发送邮箱2的发送或中止请求完成时,该位自动置1。软件置1清0,软件写0无效。</td><td>0</td></tr><tr><td>15</td><td>ABRQ1</td><td>RW0</td><td>发送邮箱1的发送中止请求。软件置1,可以中止邮箱1的发送请求,发送报文被清除时硬件清0。软件写0无效。</td><td>0</td></tr><tr><td>[14:12]</td><td>Reserved</td><td>R0</td><td>保留</td><td>0</td></tr><tr><td>11</td><td>TERR1</td><td>RW1</td><td>发送邮箱1发送失败标志位,当发送邮箱1发送失败,该位自动置1。软件置1清0,软件写0无效。</td><td>0</td></tr><tr><td>10</td><td>ALST1</td><td>RW1</td><td>发送邮箱1仲裁失败标志位,当发送邮箱1仲裁优先级低导致发送失败,该位自动置1。</td><td>0</td></tr><tr><td>9</td><td>TXOK1</td><td>RW1</td><td>发送邮箱1发送成功标志位1:上次发送成功;0:上次发送失败。软件置1清0,软件写0无效。</td><td>0</td></tr><tr><td>8</td><td>RQCP1</td><td>RW</td><td>发送邮箱1请求完成标志位,当发送邮箱1</td><td>0</td></tr><tr><td></td><td></td><td></td><td>的发送或中止请求完成时,该位自动置1。软件置1清0,软件写0无效。</td><td></td></tr><tr><td>7</td><td>ABRQ0</td><td>RWO</td><td>发送邮箱0的发送中止请求。软件置1,可以中止邮箱0的发送请求,发送报文被清除时硬件清0。软件写0无效。</td><td>0</td></tr><tr><td>[6:4]</td><td>Reserved</td><td>R0</td><td>保留</td><td>0</td></tr><tr><td>3</td><td>TERRO</td><td>RW1</td><td>发送邮箱0发送失败标志位,当发送邮箱0发送失败,该位自动置1。软件置1清0,软件写0无效。</td><td>0</td></tr><tr><td>2</td><td>ALST0</td><td>RW1</td><td>发送邮箱0仲裁失败标志位,当发送邮箱0仲裁优先级低导致发送失败,该位自动置1。软件置1清0,软件写0无效。</td><td>0</td></tr><tr><td>1</td><td>TXOKO</td><td>RW1</td><td>发送邮箱0发送成功标志位1:上次发送成功;0:上次发送失败。软件置1清0,软件写0无效。</td><td>0</td></tr><tr><td>0</td><td>RQCP0</td><td>RW</td><td>发送邮箱0请求完成标志位,当发送邮箱0的发送或中止请求完成时,该位自动置1。软件置1清0,软件写0无效。</td><td>0</td></tr></table>

### 24.7.4 CANx 接收 FIFO 0 状态寄存器（CANx_RFIFO0）（x=1/2）

偏移地址： $0 \times 0 0$

<table><tr><td>31</td><td>30</td><td>29</td><td>28</td><td>27</td><td>26</td><td>25</td><td>24</td><td>23</td><td>22</td><td>21</td><td>20</td><td>19</td><td>18</td><td>17</td><td>16</td></tr><tr><td colspan="16">Reserved</td></tr><tr><td>15</td><td>14</td><td>13</td><td>12</td><td>11</td><td>10</td><td>9</td><td>8</td><td>7</td><td>6</td><td>5</td><td>4</td><td>3</td><td>2</td><td>1</td><td>0</td></tr><tr><td colspan="10">Reserved</td><td>RFOMO</td><td>FOVRO</td><td>FULLO</td><td colspan="2">Reser ved</td><td>FMP0[1:0]</td></tr></table>

<table><tr><td>位</td><td>名称</td><td>访问</td><td>描述</td><td>复位值</td></tr><tr><td>[31:6]</td><td>Reserved</td><td>R0</td><td>保留。</td><td>0</td></tr><tr><td>5</td><td>RFOMO</td><td>RW1</td><td>软件对该位置1,则释放接收FIFO00的当前邮箱报文,释放完后自动清0,软件写0无效。</td><td>0</td></tr><tr><td>4</td><td>FOVRO</td><td>RW1</td><td>接收FIFO00溢出标志位。当FIFO00中有三个报文时,又接到新报文,硬件置1。该位需要软件置1清0,软件写0无效。</td><td>0</td></tr><tr><td>3</td><td>FULL0</td><td>RW1</td><td>接收FIFO00满标志位。当FIFO00中有三个报文时,硬件置1。该位需要软件置1清0,软件写0无效。</td><td>0</td></tr><tr><td>2</td><td>Reserved</td><td>R0</td><td>保留。</td><td>0</td></tr><tr><td>[1:0]</td><td>FMP0[1:0]</td><td>R0</td><td>接收FIFO00报文数目。</td><td>0</td></tr></table>

### 24.7.5 CANx 接收 FIFO 1 状态寄存器（CANx_RFIFO1）（x=1/2）

偏移地址： $0 \times 1 0$

<table><tr><td>31</td><td>30</td><td>29</td><td>28</td><td>27</td><td>26</td><td>25</td><td>24</td><td>23</td><td>22</td><td>21</td><td>20</td><td>19</td><td>18</td><td>17</td><td>16</td></tr><tr><td colspan="16">Reserved</td></tr><tr><td>15</td><td>14</td><td>13</td><td>12</td><td>11</td><td>10</td><td>9</td><td>8</td><td>7</td><td>6</td><td>5</td><td>4</td><td>3</td><td>2</td><td>1</td><td>0</td></tr><tr><td colspan="10">Reserved</td><td>RF0M1</td><td>FOVR1</td><td>FULL1</td><td colspan="2">Reser ved</td><td>FMP1[1:0]</td></tr></table>

<table><tr><td>位</td><td>名称</td><td>访问</td><td>描述</td><td>复位值</td></tr><tr><td>[31:6]</td><td>Reserved</td><td>R0</td><td>保留。</td><td>0</td></tr><tr><td>5</td><td>RF0M1</td><td>RW1</td><td>软件对该位置1,则释放接收FIFO_1的当前邮箱报文,释放完后自动清0,软件写0无效。</td><td>0</td></tr><tr><td>4</td><td>FOVR1</td><td>RW1</td><td>接收FIFO_1溢出标志位。当FIFO_1中有三个报文时,又接到新报文,硬件置1。该位需要软件置1清0,软件写0无效。</td><td>0</td></tr><tr><td>3</td><td>FULL1</td><td>RW1</td><td>接收FIFO_1满标志位。当FIFO_1中有三个报文时,硬件置1。该位需要软件置1清0,软件写0无效。</td><td>0</td></tr><tr><td>2</td><td>Reserved</td><td>RF</td><td>保留</td><td>0</td></tr><tr><td>[1:0]</td><td>FMP1[1:0]</td><td>R0</td><td>接收FIFO_1报文数目。</td><td>0</td></tr></table>

### 24.7.6 CANx 中断使能寄存器（CANx_INTENR）（x=1/2）

偏移地址：0x14

<table><tr><td>31</td><td>30</td><td>29</td><td>28</td><td>27</td><td>26</td><td>25</td><td>24</td><td>23</td><td>22</td><td>21</td><td>20</td><td>19</td><td>18</td><td>17</td><td>16</td><td></td></tr><tr><td colspan="15">Reserved</td><td>SLKIE</td><td>WKUIE</td></tr><tr><td>15</td><td>14</td><td>13</td><td>12</td><td>11</td><td>10</td><td>9</td><td>8</td><td>7</td><td>6</td><td>5</td><td>4</td><td>3</td><td>2</td><td>1</td><td colspan="2">0</td></tr><tr><td>ERRIE</td><td colspan="2">Reserved</td><td>LECIE</td><td>BOFIE</td><td>EPVIE</td><td>EWGIE</td><td colspan="2">Reser ved</td><td>FOVIE1</td><td>FFIE1</td><td>FMPIE1</td><td>FOVIE0</td><td>FFIE0</td><td>FMPIE0</td><td colspan="2">TMEIE</td></tr></table>

<table><tr><td>位</td><td>名称</td><td>访问</td><td>描述</td><td>复位值</td></tr><tr><td>[31:18]</td><td>Reserved</td><td>R0</td><td>保留。</td><td>0</td></tr><tr><td>17</td><td>SLKIE</td><td>RW</td><td>睡眠中断使能位。
1: 进入睡眠状态时,产生中断;
0: 进入睡眠状态时,不产生中断。</td><td>0</td></tr><tr><td>16</td><td>WKUIE</td><td>RW</td><td>唤醒中断使能位。
1: 当 CAN 控制器被唤醒时,产生中断;
0: 当 CAN 控制器被唤醒时,不产生中断。</td><td>0</td></tr><tr><td>15</td><td>ERRIE</td><td>RW</td><td>错误中断使能位, CAN 错误中断总使能位。
1: 当 CAN 控制器产生错误时,产生中断;
0: 当 CAN 控制器产生错误时,不产生中断。</td><td>0</td></tr><tr><td>[14:12]</td><td>Reserved</td><td>RF</td><td>保留。</td><td>0</td></tr><tr><td>11</td><td>LECIE</td><td>RW</td><td>上次错误号中断使能位。
1: 检测到错误时,硬件更新 LEC[2:0],更新ERRI位为1,触发错误中断;</td><td>0</td></tr><tr><td></td><td></td><td></td><td>0:检测到错误时,硬件更新LEC[2:0],不更新ERRI位,不触发错误中断。</td><td></td></tr><tr><td>10</td><td>BOFIE</td><td>RW</td><td>离线中断使能位。1:进入离线状态时,更新ERRI位为1,触发错误中断;0:进入离线状态时,不更新ERRI位,不触发错误中断。</td><td>0</td></tr><tr><td>9</td><td>EPVIE</td><td>RW</td><td>错误被动中断使能位。1:进入错误被动状态时,更新ERRI位为1,触发错误中断;0:进入错误被动状态时,不更新ERRI位,不触发错误中断。</td><td>0</td></tr><tr><td>8</td><td>EWGIE</td><td>RW</td><td>错误警告中断使能位。1:出错次数达到警告阈值时,更新ERRI位为1,触发错误中断;0:出错次数达到警告阈值时,不更新ERRI位,不触发错误中断。</td><td>0</td></tr><tr><td>7</td><td>Reserved</td><td>RF</td><td>保留。</td><td>0</td></tr><tr><td>6</td><td>FOVIE1</td><td>RW</td><td>接收FIFO_1溢出中断使能位。1:当FIFO_1溢出,触发FIFO_1中断;0:当FIFO_1溢出,不触发FIFO_1中断。</td><td>0</td></tr><tr><td>5</td><td>FFIE1</td><td>RW</td><td>接收FIFO_1满中断使能位。1:当FIFO_1为满,触发FIFO_1中断;0:当FIFO_1为满,不触发FIFO_1中断。</td><td>0</td></tr><tr><td>4</td><td>FMPIE1</td><td>RW</td><td>接收FIFO_1消息挂号中断使能位。1:当FIFO_1更新FMP位,且不为0,触发FIFO_1中断;0:当FIFO_1更新FMP位,且不为0,不触发FIFO_1中断。</td><td>0</td></tr><tr><td>3</td><td>FOVIE0</td><td>RW</td><td>接收FIFO_0溢出中断使能位。1:当FIFO_0溢出,触发FIFO_0中断;0:当FIFO_0溢出,不触发FIFO_0中断。</td><td>0</td></tr><tr><td>2</td><td>FFIE0</td><td>RW</td><td>接收FIFO_0满中断使能位。1:当FIFO_0为满,触发FIFO_0中断;0:当FIFO_0为满,不触发FIFO_0中断。</td><td>0</td></tr><tr><td>1</td><td>FMPIE0</td><td>RW</td><td>接收FIFO_0消息挂号中断使能位。1:当FIFO_0更新FMP位,且不为0,触发FIFO_0中断;0:当FIFO_0更新FMP位,且不为0,不触发FIFO_0中断。</td><td>0</td></tr><tr><td>0</td><td>TMEIE</td><td>RW</td><td>发送邮箱空中断。1:当发送邮箱为空时,产生中断;0:当发送邮箱为空时,不产生中断。</td><td>0</td></tr></table>

### 24.7.7 CANx 错误状态寄存器（CANx_ERRSR）（x=1/2）

偏移地址：0x18

<table><tr><td>31</td><td>30</td><td>29</td><td>28</td><td>27</td><td>26</td><td>25</td><td>24</td><td>23</td><td>22</td><td>21</td><td>20</td><td>19</td><td>18</td><td>17</td><td>16</td></tr><tr><td colspan="8">REC[7:0]</td><td colspan="8">TEC[7:0]</td></tr><tr><td>15</td><td>14</td><td>13</td><td>12</td><td>11</td><td>10</td><td>9</td><td>8</td><td>7</td><td>6</td><td>5</td><td>4</td><td>3</td><td>2</td><td>1</td><td>0</td></tr><tr><td colspan="8">Reserved</td><td colspan="2">LEC[2:0]</td><td colspan="2">Reser ved</td><td>BOFF</td><td colspan="2">EPVF</td><td>EWGF</td></tr></table>

<table><tr><td>位</td><td>名称</td><td>访问</td><td>描述</td><td>复位值</td></tr><tr><td>[31:24]</td><td>REC[7:0]</td><td>R0</td><td>接收错误计数器。当CAN接收出错时,根据出错条件,该计数器加1或8;接收成功后,该计数器减1或设为120(错误计数值大于127)。计数器值超过127时,CAN进入错误被动状态。</td><td>0</td></tr><tr><td>[23:16]</td><td>TEC[7:0]</td><td>R0</td><td>发送错误计数器。当CAN发送出错时,根据出错条件,该计数器加1或8;发送成功后,该计数器减1或设为120(错误计数值大于127)。计数器值超过127时,CAN进入错误被动状态。</td><td>0</td></tr><tr><td>[15:7]</td><td>Reserved</td><td>R0</td><td>保留。</td><td>0</td></tr><tr><td>[6:4]</td><td>LEC[2:0]</td><td>RW</td><td>上次错误代号。检测到CAN总线上发送错误时,控制器根据出错情况设置,当正确收发报文时,置000b。000:无错误;001:位填充错误;010:FORM格式错误;011:ACK确认错误;100:隐性位错误;101:显性位错误;110:CRC错误;111:软件设置。通常应用软件读取到错误时,把代号设置为111b,可以检测到代号更新。</td><td>000b</td></tr><tr><td>3</td><td>Reserved</td><td>R0</td><td>保留。</td><td>0</td></tr><tr><td>2</td><td>BOFF</td><td>R0</td><td>离线状态标志位。当CAN控制器进入离线状态时,硬件自动置1;退出离线状态时,硬件自动清0。</td><td>0</td></tr><tr><td>1</td><td>EPVF</td><td>R0</td><td>错误被动标志位。当收发错误计数器达到错误被动阈值时,即大于127时,硬件置1。</td><td>0</td></tr><tr><td>0</td><td>EWGF</td><td>R0</td><td>错误警告标志位。当收发错误计数器达到警告阈值时,即大于等于96时,硬件置1。</td><td>0</td></tr></table>

### 24.7.8 CANx 位时序寄存器（CANx_BTIMR）（x=1/2）

偏移地址：0x1C

<table><tr><td>31</td><td>30</td><td>29</td><td>28</td><td>27</td><td>26</td><td>25</td><td>24</td><td>23</td><td>22</td><td>21</td><td>20</td><td>19</td><td>18</td><td>17</td><td>16</td></tr><tr><td>SILM</td><td>LBKM</td><td colspan="4">Reserved</td><td colspan="2">SJW[1:0]</td><td>Reserved</td><td colspan="3">TS2[2:0]</td><td colspan="4">TS1[3:0]</td></tr></table>

<table><tr><td>15</td><td>14</td><td>13</td><td>12</td><td>11</td><td>10</td><td>9</td><td>8</td><td>7</td><td>6</td><td>5</td><td>4</td><td>3</td><td>2</td><td>1</td><td>0</td></tr><tr><td colspan="6">Reserved</td><td colspan="10">BRP[9:0]</td></tr></table>

<table><tr><td>位</td><td>名称</td><td>访问</td><td>描述</td><td>复位值</td></tr><tr><td>31</td><td>SILM</td><td>RW</td><td>静默模式设置位。1:进入静默模式;0:退出静默模式。</td><td>0</td></tr><tr><td>30</td><td>LBKM</td><td>RW</td><td>环回模式设置位。1:进入环回模式;0:退出环回模式。</td><td>0</td></tr><tr><td>[29:26]</td><td>Reserved</td><td>R0</td><td>保留。</td><td>0</td></tr><tr><td>[25:24]</td><td>SJW[1:0]</td><td>RW</td><td>定义了重新同步跳转宽度设置值。实现重新同步时,位中可以延长和缩小的最小时间单元数量上限,实际值为(SJW[1:0]+1),范围可设置为1到4个最小时时间单元。</td><td>01b</td></tr><tr><td>23</td><td>Reserved</td><td>R0</td><td>保留。</td><td>0</td></tr><tr><td>[22:20]</td><td>TS2[2:0]</td><td>RW</td><td>时间段2设置值。定义了时间段2占用了多少个最小时间单元,实际值为(TS2[2:0]+1)。</td><td>010b</td></tr><tr><td>[19:16]</td><td>TS1[3:0]</td><td>RW</td><td>时间段1设置值。定义了时间段1占用了多少个最小时间单元,实际值为(TS1[3:0]+1)。</td><td>0011b</td></tr><tr><td>[15:10]</td><td>Reserved</td><td>R0</td><td>保留。</td><td>0</td></tr><tr><td>[9:0]</td><td>BRP[9:0]</td><td>RW</td><td>最小时间单元长度设置值Tq=(BRP[9:0]+1)×tpclk注:CAN波特率计算公式为:CANbps=PCLK1/((TSI[3:0]+1+TS2[2:0]+1+1)*(BPR[9:0]+1))</td><td>0</td></tr></table>

### 24.7.9 CANx 时间触发控制寄存器（CANx_TTCTLR）（x=1/2）

偏移地址： $_ { 0 \times 2 0 }$

<table><tr><td>31</td><td>30</td><td>29</td><td>28</td><td>27</td><td>26</td><td>25</td><td>24</td><td>23</td><td>22</td><td>21</td><td>20</td><td>19</td><td>18</td><td>17</td><td>16</td></tr><tr><td colspan="14">Reserved</td><td>MODE</td><td>TIMRS T</td></tr><tr><td>15</td><td>14</td><td>13</td><td>12</td><td>11</td><td>10</td><td>9</td><td>8</td><td>7</td><td>6</td><td>5</td><td>4</td><td>3</td><td>2</td><td>1</td><td>0</td></tr><tr><td colspan="16">TIMCMV [15:0]</td></tr></table>

<table><tr><td>位</td><td>名称</td><td>访问</td><td>描述</td><td>复位值</td></tr><tr><td>[31:18]</td><td>Reserved</td><td>R0</td><td>保留。</td><td>0</td></tr><tr><td>17</td><td>MODE</td><td>RW</td><td>时间触发模式选择位。
1: 增强模式;
0: 默认模式。</td><td>0</td></tr><tr><td>16</td><td>TIMRST</td><td>WZ</td><td>内部计数器复位控制位。
写1复位内部计数器, 硬件自动清0</td><td>0</td></tr><tr><td>[15:0]</td><td>TIMCMV[15:0]</td><td>RW</td><td>内部计数器计数终值</td><td>fffffh</td></tr></table>

### 24.7.10 CANx 时间触发计数值寄存器（CANx_TTCNT）（x=1/2）

偏移地址： ${ 0 } { \times 2 4 }$

<table><tr><td>31</td><td>30</td><td>29</td><td>28</td><td>27</td><td>26</td><td>25</td><td>24</td><td>23</td><td>22</td><td>21</td><td>20</td><td>19</td><td>18</td><td>17</td><td>16</td></tr><tr><td colspan="16">Reserved</td></tr><tr><td>15</td><td>14</td><td>13</td><td>12</td><td>11</td><td>10</td><td>9</td><td>8</td><td>7</td><td>6</td><td>5</td><td>4</td><td>3</td><td>2</td><td>1</td><td>0</td></tr><tr><td colspan="16">TIMCNT[15:0]</td></tr></table>

<table><tr><td>位</td><td>名称</td><td>访问</td><td>描述</td><td>复位值</td></tr><tr><td>[31:16]</td><td>Reserved</td><td>R0</td><td>保留。</td><td>0</td></tr><tr><td>[15:0]</td><td>TIMCNT[15:0]</td><td>RW</td><td>时间触发计数值</td><td>0</td></tr></table>

### 24.7.11 CANx 发送邮箱标识符寄存器（CANx_TXMIRy）（x=0/1,y=0/1/2）

偏移地址：0x180,0x190,0x1A0

<table><tr><td>31</td><td>30</td><td>29</td><td>28</td><td>27</td><td>26</td><td>25</td><td>24</td><td>23</td><td>22</td><td>21</td><td>20</td><td>19</td><td>18</td><td>17</td><td>16</td></tr><tr><td colspan="11">STID[10:0]/EXID[28:18]</td><td colspan="5">EXID[17:13]</td></tr><tr><td>15</td><td>14</td><td>13</td><td>12</td><td>11</td><td>10</td><td>9</td><td>8</td><td>7</td><td>6</td><td>5</td><td>4</td><td>3</td><td>2</td><td>1</td><td>0</td></tr><tr><td colspan="12">EXID[12:0]</td><td>IDE</td><td>RTR</td><td colspan="2">TXRQ</td></tr></table>

<table><tr><td>位</td><td>名称</td><td>访问</td><td>描述</td><td>复位值</td></tr><tr><td>[31:21]</td><td>STID[10:0] /EXID[28:18]</td><td>RW</td><td>标准标识符或扩展标识符的高11位。</td><td>x</td></tr><tr><td>[20:3]</td><td>EXID[17:0]</td><td>RW</td><td>扩展标识符的低18位。</td><td>x</td></tr><tr><td>2</td><td>IDE</td><td>RW</td><td>标识符选择标志位。1:选用扩展标识符;0:选用标准标识符。</td><td>x</td></tr><tr><td>1</td><td>RTR</td><td>RW</td><td>远程帧(也称遥控帧)选择标志位。1:当前为远程帧;0:当前为数据帧。</td><td>x</td></tr><tr><td>0</td><td>TXRQ</td><td>RW</td><td>数据发送请求标志位。软件置1时,请求发送邮箱里的数据,发送完毕邮箱为空时,硬件清0。</td><td>0</td></tr></table>

### 24.7.12 CANx 发送邮箱数据长度和时间戳寄存器（CANx_TXMDTRy）（x=0/1,y=0/1/2）

偏移地址：0x184,0x194,0x1A4

<table><tr><td>31</td><td>30</td><td>29</td><td>28</td><td>27</td><td>26</td><td>25</td><td>24</td><td>23</td><td>22</td><td>21</td><td>20</td><td>19</td><td>18</td><td>17</td><td>16</td></tr><tr><td colspan="16">TIME[15:0]</td></tr><tr><td>15</td><td>14</td><td>13</td><td>12</td><td>11</td><td>10</td><td>9</td><td>8</td><td>7</td><td>6</td><td>5</td><td>4</td><td>3</td><td>2</td><td>1</td><td>0</td></tr><tr><td colspan="7">Reserved</td><td>TGT</td><td colspan="4">Reserved</td><td colspan="4">DLC[3:0]</td></tr></table>

<table><tr><td>位</td><td>名称</td><td>访问</td><td>描述</td><td>复位值</td></tr><tr><td>[31:16]</td><td>TIME[15:0]</td><td>RW</td><td>用于发送报文SOF时刻的16位定时器值。</td><td>x</td></tr><tr><td>[15:9]</td><td>Reserved</td><td>RO</td><td>保留。</td><td>0</td></tr><tr><td>8</td><td>TGT</td><td>RW</td><td>报文时间戳发送选择标志位。该位在TTCM置1，并报文长度为8时有效。1：发送时间戳，值为TIME[15:0]的即时值，替换了8字节报文的最后两个字节；0：不发送时间戳。</td><td>x</td></tr><tr><td>[7:4]</td><td>Reserved</td><td>RO</td><td>保留。</td><td>0</td></tr><tr><td>[3:0]</td><td>DLC[3:0]</td><td>RW</td><td>数据帧的数据长度或远程帧请求数据长度数据长度可设置范围为0到8。</td><td>0</td></tr></table>

### 24.7.13 CANx 发送邮箱低字节数据寄存器（CANx_TXMDLRy）（x=0/1,y=0/1/2）

偏移地址：0x188,0x198,0x1A8

<table><tr><td>31</td><td>30</td><td>29</td><td>28</td><td>27</td><td>26</td><td>25</td><td>24</td><td>23</td><td>22</td><td>21</td><td>20</td><td>19</td><td>18</td><td>17</td><td>16</td></tr><tr><td colspan="8">DATA3[7:0]</td><td colspan="8">DATA2[7:0]</td></tr><tr><td>15</td><td>14</td><td>13</td><td>12</td><td>11</td><td>10</td><td>9</td><td>8</td><td>7</td><td>6</td><td>5</td><td>4</td><td>3</td><td>2</td><td>1</td><td>0</td></tr><tr><td colspan="8">DATA1[7:0]</td><td colspan="8">DATA0[7:0]</td></tr></table>

<table><tr><td>位</td><td>名称</td><td>访问</td><td>描述</td><td>复位值</td></tr><tr><td>[31:24]</td><td>DATA3[7:0]</td><td>RW</td><td>发送数据字节3的内容。</td><td>xxh</td></tr><tr><td>[23:16]</td><td>DATA2[7:0]</td><td>RW</td><td>发送数据字节2的内容。</td><td>xxh</td></tr><tr><td>[15:8]</td><td>DATA1[7:0]</td><td>RW</td><td>发送数据字节1的内容。</td><td>xxh</td></tr><tr><td>[7:0]</td><td>DATA0[7:0]</td><td>RW</td><td>发送数据字节0的内容。</td><td>xxh</td></tr></table>

### 24.7.14 CANx 发送邮箱高字节数据寄存器（CANx_TXMDHRy）（x=0/1,y=0/1/2）

偏移地址：0x18C,0x19C,0x1AC

<table><tr><td>31</td><td>30</td><td>29</td><td>28</td><td>27</td><td>26</td><td>25</td><td>24</td><td>23</td><td>22</td><td>21</td><td>20</td><td>19</td><td>18</td><td>17</td><td>16</td></tr><tr><td colspan="8">DATA7[7:0]</td><td colspan="8">DATA6[7:0]</td></tr><tr><td>15</td><td>14</td><td>13</td><td>12</td><td>11</td><td>10</td><td>9</td><td>8</td><td>7</td><td>6</td><td>5</td><td>4</td><td>3</td><td>2</td><td>1</td><td>0</td></tr><tr><td colspan="8">DATA5[7:0]</td><td colspan="8">DATA4[7:0]</td></tr></table>

<table><tr><td>位</td><td>名称</td><td>访问</td><td>描述</td><td>复位值</td></tr><tr><td>[31:24]</td><td>DATA7[7:0]</td><td>RW</td><td>发送数据字节7的内容。</td><td>x</td></tr><tr><td>[23:16]</td><td>DATA6[7:0]</td><td>RW</td><td>发送数据字节6的内容。</td><td>x</td></tr><tr><td>[15:8]</td><td>DATA5[7:0]</td><td>RW</td><td>发送数据字节5的内容。</td><td>x</td></tr><tr><td>[7:0]</td><td>DATA4[7:0]</td><td>RW</td><td>发送数据字节4的内容。</td><td>x</td></tr></table>

### 24.7.15 CANx 接收邮箱标识符寄存器（CANx_RXMIRy）（x=0/1,y=0/1）

偏移地址：0x1B0,0x1C0

<table><tr><td>31</td><td>30</td><td>29</td><td>28</td><td>27</td><td>26</td><td>25</td><td>24</td><td>23</td><td>22</td><td>21</td><td>20</td><td>19</td><td>18</td><td>17</td><td>16</td></tr><tr><td colspan="11">STID[10:0]/EXID[28:18]</td><td colspan="5">EXID[17:13]</td></tr><tr><td>15</td><td>14</td><td>13</td><td>12</td><td>11</td><td>10</td><td>9</td><td>8</td><td>7</td><td>6</td><td>5</td><td>4</td><td>3</td><td>2</td><td>1</td><td>0</td></tr><tr><td colspan="12">EXID[12:0]</td><td>IDE</td><td>RTR</td><td colspan="2">TXRQ</td></tr></table>

<table><tr><td>位</td><td>名称</td><td>访问</td><td>描述</td><td>复位值</td></tr><tr><td>[31:21]</td><td>STID[10:0] /EXIDH[28:18]</td><td>R0</td><td>标准标识符或扩展标识符的高11位。</td><td>x</td></tr><tr><td>[20:3]</td><td>EXIDL[17:0]</td><td>R0</td><td>扩展标识符的低18位。</td><td>x</td></tr><tr><td>2</td><td>IDE</td><td>R0</td><td>标识符选择标志位。1:选用扩展标识符;0:选用标准标识符。</td><td>x</td></tr><tr><td>1</td><td>RTR</td><td>R0</td><td>远程帧(也称遥控帧)选择标志位。1:当前为远程帧;0:当前为数据帧。</td><td>x</td></tr><tr><td>0</td><td>Reserved</td><td>R0</td><td>保留。</td><td>x</td></tr></table>

### 24.7.16 CANx 接收邮箱数据长度和时间戳寄存器（CANx_RXMDTRy）（x=0/1,y=0/1）

偏移地址：0x1B4,0x1C4

<table><tr><td>31</td><td>30</td><td>29</td><td>28</td><td>27</td><td>26</td><td>25</td><td>24</td><td>23</td><td>22</td><td>21</td><td>20</td><td>19</td><td>18</td><td>17</td><td>16</td></tr><tr><td colspan="16">TIME[15:0]</td></tr><tr><td>15</td><td>14</td><td>13</td><td>12</td><td>11</td><td>10</td><td>9</td><td>8</td><td>7</td><td>6</td><td>5</td><td>4</td><td>3</td><td>2</td><td>1</td><td>0</td></tr><tr><td colspan="8">FMI[7:0]</td><td colspan="4">Reserved</td><td colspan="4">DLC[3:0]</td></tr></table>

<table><tr><td>位</td><td>名称</td><td>访问</td><td>描述</td><td>复位值</td></tr><tr><td>[31:16]</td><td>TIME[15:0]</td><td>R0</td><td>用于接收报文 SOF 时刻的 16 位定时器值。</td><td>x</td></tr><tr><td>[15:8]</td><td>FMI[7:0]</td><td>R0</td><td>报文所匹配的过滤器编号。</td><td>x</td></tr><tr><td>[7:4]</td><td>Reserved</td><td>R0</td><td>保留。</td><td>0</td></tr><tr><td>[3:0]</td><td>DLC[3:0]</td><td>R0</td><td>接收报文数据长度。
数据帧长度 0 到 8，远程帧为 0。</td><td>x</td></tr></table>

### 24.7.17 CANx 接收邮箱低字节数据寄存器（CANx_RXMDLRy）（x=0/1,y=0/1）

偏移地址：0x1B8,0x1C8

<table><tr><td>31</td><td>30</td><td>29</td><td>28</td><td>27</td><td>26</td><td>25</td><td>24</td><td>23</td><td>22</td><td>21</td><td>20</td><td>19</td><td>18</td><td>17</td><td>16</td></tr><tr><td colspan="8">DATA3[7:0]</td><td colspan="8">DATA2[7:0]</td></tr><tr><td>15</td><td>14</td><td>13</td><td>12</td><td>11</td><td>10</td><td>9</td><td>8</td><td>7</td><td>6</td><td>5</td><td>4</td><td>3</td><td>2</td><td>1</td><td>0</td></tr><tr><td colspan="8">DATA1[7:0]</td><td colspan="8">DATA0[7:0]</td></tr></table>

<table><tr><td>位</td><td>名称</td><td>访问</td><td>描述</td><td>复位值</td></tr><tr><td>[31:24]</td><td>DATA3[7:0]</td><td>R0</td><td>接收报文的数据字节3。</td><td>x</td></tr><tr><td>[23:16]</td><td>DATA2[7:0]</td><td>R0</td><td>接收报文的数据字节2。</td><td>x</td></tr><tr><td>[15:8]</td><td>DATA1[7:0]</td><td>R0</td><td>接收报文的数据字节1。</td><td>x</td></tr><tr><td>[7:0]</td><td>DATA0[7:0]</td><td>R0</td><td>接收报文的数据字节0。</td><td>x</td></tr></table>

### 24.7.18 CANx 接收邮箱高字节数据寄存器（CANx_RXMDHRy）（x=0/1,y=0/1）

偏移地址：0x1BC,0x1CC

<table><tr><td>31</td><td>30</td><td>29</td><td>28</td><td>27</td><td>26</td><td>25</td><td>24</td><td>23</td><td>22</td><td>21</td><td>20</td><td>19</td><td>18</td><td>17</td><td>16</td></tr><tr><td colspan="8">DATA7[7:0]</td><td colspan="8">DATA6[7:0]</td></tr><tr><td>15</td><td>14</td><td>13</td><td>12</td><td>11</td><td>10</td><td>9</td><td>8</td><td>7</td><td>6</td><td>5</td><td>4</td><td>3</td><td>2</td><td>1</td><td>0</td></tr><tr><td colspan="8">DATA5[7:0]</td><td colspan="8">DATA4[7:0]</td></tr></table>

<table><tr><td>位</td><td>名称</td><td>访问</td><td>描述</td><td>复位值</td></tr><tr><td>[31:24]</td><td>DATA7[7:0]</td><td>R0</td><td>接收报文的数据字节7。</td><td>x</td></tr><tr><td>[23:16]</td><td>DATA6[7:0]</td><td>R0</td><td>接收报文的数据字节6。</td><td>x</td></tr><tr><td>[15:8]</td><td>DATA5[7:0]</td><td>R0</td><td>接收报文的数据字节5。</td><td>x</td></tr><tr><td>[7:0]</td><td>DATA4[7:0]</td><td>R0</td><td>接收报文的数据字节4。</td><td>x</td></tr></table>

### 24.7.19 CANx 过滤器主控制寄存器（CANx_FCTLR）（ $\bf { \chi } _ { x = 0 } / 1 )$

偏移地址： $\mathtt { 0 \times 2 0 0 }$

<table><tr><td>31</td><td>30</td><td>29</td><td>28</td><td>27</td><td>26</td><td>25</td><td>24</td><td>23</td><td>22</td><td>21</td><td>20</td><td>19</td><td>18</td><td>17</td><td>16</td></tr><tr><td colspan="16">Reserved</td></tr><tr><td>15</td><td>14</td><td>13</td><td>12</td><td>11</td><td>10</td><td>9</td><td>8</td><td>7</td><td>6</td><td>5</td><td>4</td><td>3</td><td>2</td><td>1</td><td>0</td></tr><tr><td>Reserved</td><td colspan="7">CAN2SB[5:0]</td><td colspan="7">Reserved</td><td>FINIT</td></tr></table>

<table><tr><td>位</td><td>名称</td><td>访问</td><td>描述</td><td>复位值</td></tr><tr><td>[31:14]</td><td>Reserved</td><td>R0</td><td>保留。</td><td>0</td></tr><tr><td>[13:8]</td><td>CAN2SB[5:0]</td><td>RW</td><td>CAN2过滤器开始组（取值范围1-27）。</td><td>01110b</td></tr><tr><td>[7:1]</td><td>Reserved</td><td>R0</td><td>保留。</td><td>0</td></tr><tr><td>0</td><td>FINIT</td><td>RW</td><td>过滤器初始化模式使能标志位。
1：过滤器组为初始化模式；
0：过滤器组为正常模式。</td><td>1</td></tr></table>

### 24.7.20 CANx 过滤器模式寄存器（CANx_FMCFGR）（ $\mathbf { \chi } _ { \mathbf { x } = 0 / 1 }$ ）

偏移地址： $0 \times 2 0 4$

<table><tr><td>31</td><td>30</td><td>29</td><td>28</td><td>27</td><td>26</td><td>25</td><td>24</td><td>23</td><td>22</td><td>21</td><td>20</td><td>19</td><td>18</td><td>17</td><td>16</td></tr><tr><td colspan="3">Reserved</td><td>FBM27</td><td>FBM26</td><td>FBM25</td><td>FBM24</td><td>FBM23</td><td>FBM22</td><td>FBM21</td><td>FBM20</td><td>FBM19</td><td>FBM18</td><td>FBM17</td><td>FBM16</td><td></td></tr><tr><td>15</td><td>14</td><td>13</td><td>12</td><td>11</td><td>10</td><td>9</td><td>8</td><td>7</td><td>6</td><td>5</td><td>4</td><td>3</td><td>2</td><td>1</td><td>0</td></tr><tr><td>FBM15</td><td>FBM14</td><td>FBM13</td><td>FBM12</td><td>FBM11</td><td>FBM10</td><td>FBM9</td><td>FBM8</td><td>FBM7</td><td>FBM6</td><td>FBM5</td><td>FBM4</td><td>FBM3</td><td>FBM2</td><td>FBM1</td><td>FBM0</td></tr></table>

<table><tr><td>位</td><td>名称</td><td>访问</td><td>描述</td><td>复位值</td></tr><tr><td>[31:28]</td><td>Reserved</td><td>R0</td><td>保留。</td><td>0</td></tr><tr><td>[27:0]</td><td>FBMx</td><td>RW</td><td>过滤器组x的工作模式控制位，FINT为1才能写入。
0：过滤器组x的寄存器为屏蔽位模式；
1：过滤器组x的寄存器为标识符列表模式。</td><td>0</td></tr></table>

### 24.7.21 CANx 过滤器位宽寄存器（CANx_FSCFGR）（ $\bf { \chi } _ { x = 0 / 1 }$ ）

偏移地址： $_ { 0 \times 2 0 0 }$

<table><tr><td>31</td><td>30</td><td>29</td><td>28</td><td>27</td><td>26</td><td>25</td><td>24</td><td>23</td><td>22</td><td>21</td><td>20</td><td>19</td><td>18</td><td>17</td><td>16</td></tr><tr><td colspan="4">Reserved</td><td>FSC27</td><td>FSC26</td><td>FSC25</td><td>FSC24</td><td>FSC23</td><td>FSC22</td><td>FSC21</td><td>FSC20</td><td>FSC19</td><td>FSC18</td><td>FSC17</td><td>FSC16</td></tr><tr><td>15</td><td>14</td><td>13</td><td>12</td><td>11</td><td>10</td><td>9</td><td>8</td><td>7</td><td>6</td><td>5</td><td>4</td><td>3</td><td>2</td><td>1</td><td>0</td></tr><tr><td>FSC15</td><td>FSC14</td><td>FSC13</td><td>FSC12</td><td>FSC11</td><td>FSC10</td><td>FSC9</td><td>FSC8</td><td>FSC7</td><td>FSC6</td><td>FSC5</td><td>FSC4</td><td>FSC3</td><td>FSC2</td><td>FSC1</td><td>FSC0</td></tr></table>

<table><tr><td>位</td><td>名称</td><td>访问</td><td>描述</td><td>复位值</td></tr><tr><td>[31:28]</td><td>Reserved</td><td>R0</td><td>保留。</td><td>0</td></tr><tr><td>[27:0]</td><td>FSCx</td><td>RW</td><td>过滤器组x的位宽控制位，FINT为1才能写入。
1：过滤器组x的寄存器为单个32位；
0：过滤器组x的寄存器为2个16位。</td><td>0</td></tr></table>

### 24.7.22 CANx 过滤器 FIFO 关联寄存器（CANx_FAFIFOR）（x=0/1）

偏移地址： $0 \times 2 1 4$

<table><tr><td>31</td><td>30</td><td>29</td><td>28</td><td>27</td><td>26</td><td>25</td><td>24</td><td>23</td><td>22</td><td>21</td><td>20</td><td>19</td><td>18</td><td>17</td><td>16</td></tr><tr><td colspan="4">Reserved</td><td>FFA27</td><td>FFA26</td><td>FFA25</td><td>FFA24</td><td>FFA23</td><td>FFA22</td><td>FFA21</td><td>FFA20</td><td>FFA19</td><td>FFA18</td><td>FFA17</td><td>FFA16</td></tr><tr><td>15</td><td>14</td><td>13</td><td>12</td><td>11</td><td>10</td><td>9</td><td>8</td><td>7</td><td>6</td><td>5</td><td>4</td><td>3</td><td>2</td><td>1</td><td>0</td></tr><tr><td>FFA15</td><td>FFA14</td><td>FFA13</td><td>FFA12</td><td>FFA11</td><td>FFA10</td><td>FFA9</td><td>FFA8</td><td>FFA7</td><td>FFA6</td><td>FFA5</td><td>FFA4</td><td>FFA3</td><td>FFA2</td><td>FFA1</td><td>FFA0</td></tr></table>

<table><tr><td>位</td><td>名称</td><td>访问</td><td>描述</td><td>复位值</td></tr><tr><td>[31:28]</td><td>Reserved</td><td>R0</td><td>保留。</td><td>0</td></tr><tr><td>[27:0]</td><td>FFAx</td><td>RW</td><td>过滤器组x的关联FIFO控制位,FINT为1才能写入。
1:过滤器组x被关联到FIFO_1;
0:过滤器组x被关联到FIFO_0。</td><td>0</td></tr></table>

### 24.7.23 CANx 过滤器激活寄存器（CANx_FWR）（ $\bf { \chi } _ { x = 0 / 1 }$ ）

偏移地址： $_ { 0 \times 2 1 0 }$

<table><tr><td>31</td><td>30</td><td>29</td><td>28</td><td>27</td><td>26</td><td>25</td><td>24</td><td>23</td><td>22</td><td>21</td><td>20</td><td>19</td><td>18</td><td>17</td><td>16</td></tr><tr><td colspan="4">Reserved</td><td>FACT
27</td><td>FACT
26</td><td>FACT
25</td><td>FACT
24</td><td>FACT
23</td><td>FACT
22</td><td>FACT
21</td><td>FACT
20</td><td>FACT
19</td><td>FACT
18</td><td>FACT
17</td><td>FACT
16</td></tr><tr><td>15</td><td>14</td><td>13</td><td>12</td><td>11</td><td>10</td><td>9</td><td>8</td><td>7</td><td>6</td><td>5</td><td>4</td><td>3</td><td>2</td><td>1</td><td>0</td></tr><tr><td>FACT
15</td><td>FACT
14</td><td>FACT
13</td><td>FACT
12</td><td>FACT
11</td><td>FACT
10</td><td>FACT
9</td><td>FACT
8</td><td>FACT
7</td><td>FACT
6</td><td>FACT
5</td><td>FACT
4</td><td>FACT
3</td><td>FACT
2</td><td>FACT
1</td><td>FACT
0</td></tr></table>

<table><tr><td>位</td><td>名称</td><td>访问</td><td>描述</td><td>复位值</td></tr><tr><td>[31:28]</td><td>Reserved</td><td>R0</td><td>保留。</td><td>0</td></tr><tr><td>[27:0]</td><td>FACTx</td><td>RW</td><td>过滤器组x的激活控制位。
1: 过滤器组x激活;
0: 过滤器组x禁用。</td><td>0</td></tr></table>

### 24.7.24 CANx 过滤器组的过滤寄存器（CANx_FiRy）（x=1/2,i=0-27,y=1/2）

偏移地址： $0 { \times } 2 4 0 { - } 0 { \times } 3 1 0$

<table><tr><td>31</td><td>30</td><td>29</td><td>28</td><td>27</td><td>26</td><td>25</td><td>24</td><td>23</td><td>22</td><td>21</td><td>20</td><td>19</td><td>18</td><td>17</td><td>16</td></tr><tr><td>FB31</td><td>FB30</td><td>FB29</td><td>FB28</td><td>FB27</td><td>FB26</td><td>FB25</td><td>FB24</td><td>FB23</td><td>FB22</td><td>FB21</td><td>FB20</td><td>FB19</td><td>FB18</td><td>FB17</td><td>FB16</td></tr><tr><td>15</td><td>14</td><td>13</td><td>12</td><td>11</td><td>10</td><td>9</td><td>8</td><td>7</td><td>6</td><td>5</td><td>4</td><td>3</td><td>2</td><td>1</td><td>0</td></tr><tr><td>FB15</td><td>FB14</td><td>FB13</td><td>FB12</td><td>FB11</td><td>FB10</td><td>FB9</td><td>FB8</td><td>FB7</td><td>FB6</td><td>FB5</td><td>FB4</td><td>FB3</td><td>FB2</td><td>FB1</td><td>FB0</td></tr></table>

<table><tr><td>位</td><td>名称</td><td>访问</td><td>描述</td><td>复位值</td></tr><tr><td>[31:0]</td><td>FB</td><td>RW</td><td>过滤器组中寄存器的标志位，FINT为1才能写入。标识符模式下：1：相应位期望电平为隐性位；0：相应位期望电平为显性位。屏蔽位模式下：1：必须和对应的标识符寄存器位一致；0：不需要和对应的标识符寄存器位一致。</td><td>0</td></tr></table>

# 第 25 章 数字视频接口（DVP）

本章模块描述适用于CH32F2x、CH32V2x和CH32V3x微控制器系列部分产品。

数字视频接口（DVP），它支持使用 DVP 接口时序来获取图像数据流，支持按原始的行、帧格式组织的图像数据，如 YUV、RGB 等，也支持如 JPEG 格式的压缩图像数据，能够接收外部 8 位、10 位、12 位的摄像头模块输出的高速并行数据流。

## 25.1 主要特性

可配置8/10/12位数据宽度模式  
支持YUV、RGB格式数据  
$\bullet$ 支持JPEG压缩格式数据  
$\bullet$ 内置FIFO，支持DMA传输  
$\bullet$ 支持双缓冲接收  
$\bullet$ 支持裁剪功能  
支持连续模式和快照模式

## 25.2 功能描述

### 25.2.1 与传感器相连

![](images/9e40754a782aefe45606a4264e94c5278e3e17f523b56bd3ca8dc8743c7db0dd.jpg)  
图 25-1 DVP 接口连接

PLCK（Pixel clk）：像素时钟，每个时钟对应一个像素数据（非压缩数据）。外部 DVP 接口传感器输出的PCLK时钟最大支持96MHz。  
HSYNC（horizonal synchronization）：行同步信号。  
VSYNC（vertical synchronization）：帧同步信号。  
$\bullet$ DATA：像素数据或压缩数据，位宽支持 8/10/12位。  
XCLK：Sensor 的参考时钟，可以由微控制器提供或外部提供，一般使用晶振。  
I2C接口：用来配置sensor的寄存器，可通过控制的普通 GPIO 口模拟I2C时序通讯，亦可使用硬件 I2C接口操作。

表 25-1 DVP 引脚  

<table><tr><td>名称</td><td>信号类型描述</td></tr><tr><td>PLCK</td><td>像素时钟输入</td></tr><tr><td>DATA[11:0]</td><td>像素数据输入</td></tr><tr><td>VSYNC</td><td>帧同步信号输入</td></tr><tr><td>HSYNC</td><td>行同步信号输入</td></tr></table>

### 25.2.2 工作时序

一般使用的 DVP接口传感器模块输出的数字信号数据流和图像大小存在一定的关系，下面以一个图像数据举例。

如图25-2所示，传感器内部是一个 $3 5 7 6 { * } 1 1 4 5$ 大小完整的图像尺寸，经过内部缩放处理，最终从接口输出的图像数据大小为 $1 9 2 0 { ^ { * } } 1 0 8 0$ ，图像刷新率为 25。

![](images/9878c1714077302b314508f3606073eba11995085fd04c352caf6467c90edd2c.jpg)  
图 25-2 时序描述

PCLK是一个像素传输的时间，所以HSYNC是 PCLK的 3576 倍，在3576个像素中，只有 1920 个像素是有效的，剩下的1656个像素点时间内 Sensor 不传输数据。VSYNC 是帧同步信号，所以 VSYNC时间是 PCLK 的 $3 5 7 6 { * } 1 1 4 5$ 倍，同样，只有在 $1 9 2 0 { ^ { * } } 1 0 8 0$ 个有效像素时间内，Sensor在传输数据。如果传感器传输的是JPEG压缩后的数据，可能无需使用 HSYNC信号。

DVP接口信号和图像数据的关系具体以选用的传感器数据手册说明为主。

### 25.2.3 RGB/YUV/JPEG 压缩数据格式说明

RGB

三原色：红色、绿色、蓝色

YUV

亮度信号 Y，色度信号 U 和 V、Pb 和 Pr、Cb 和 Cr。

JPEG（Joint Photographic Experts Group）

有损失压缩，但损失的部分是人视觉不易察觉的部分，利用人眼对计算机色彩中高频信息部分不敏感的特点。去除视觉上多余信息（空间冗余度），去除数据本身的多余信息（结构冗余度）。

## 25.3 数字视频接口的应用

### 25.3.1 数字图像接口配置说明

1） DVP 的数据接收中，每一帧数据由 BUF0 和 BUF1 交替存储，从 BUF0 开始。对于 RGB 和 YUV 数据流，硬件在每次帧信号从无效电平变为有效电平时会复位选择 BUF0 开始，存满一行数据将会切换的 BUF1，实现交替存储；对于 JPEG 压缩数据，硬件会根据设置的 DMA 接收长度设置 BUF0 和BUF1的切换阈值。  
2） 当数据总线宽度为10位或者12位时，每接收一个数据时，系统将自动对数据进行无符号扩展到16 位再进行存储。  
3） R16_DVP_ROW_NUM 和 R16_DVP_COL_NUM 寄存器必须与传感器实际输出的图像大小匹配。  
4） 在视频流 RGB 模式下，R16_DVP_COL_NUM 表示一行数据的有效 PCLK 周期数，R16_DVP_ROW_NUM 表示一帧图像数据内包含的行数；在图像JPEG模式下，R16_DVP_COL_NUM用于配置 DMA长度，此模式下，R16_DVP_ROW_NUM 寄存器不起作用。

### 25.3.2 数字图像接口应用说明

在使用数字图像接口接收图像数据时，须正确配置 DVP相关的控制寄存器，使其与图像传感器的模式相匹配，具体操作步骤如下：

1） 通过 R8_DVP_CR1 寄存器清除 RB_DVP_ALL_CLR 和 RB_DVP_RCV_CLR 字段。  
2） 通过 R8_DVP_CR0 寄存器配置图像模式、数据位宽、PCLK 极性、HSYNC 极性和 VSYNC 极性，使其与 SENSOR 的输出相匹配。  
3） 根据配置后的图像传感器输出的有效图像像素，配置 R16_DVP_ROW_NUM 和 R16_DVP_COL_NUM 寄存器，使其与SENSOR的输出相匹配，在图像 JPEG 模式下，只需配置 R16_DVP_COL_NUM寄存器。  
4） 通过 R32_DVP_DMA_BUF0/1 寄存器配置 DMA 接收地址。  
5） 若使用快照模式，需通过R8_DVP_CR1 寄存器，配置RB_DVP_CM字段，开启快照模式。  
6） 若使用裁剪模式，需通过 R8_DVP_CR1 寄存器，配置 RB_DVP_CROP 和 RB_DVP_FCRC 字段，开启裁剪功能和控制帧捕获率，同时通过配置 R16_DVP_HOFFCNT、R16_DVP_VST、R16_DVP_CAPCNT 和R16_DVP_VLINE 设置裁剪图像的尺寸。  
7） 根据需求，通过 R8_DVP_IER 寄存器使能相应中断，通过中断控制器 NVIC 或 PFIC 配置中断优先级以及使能DVP中断。  
8） 通过 R8_DVP_CR1 寄存器使能 DMA，通过 R8_DVP_CR0 寄存器使能 DVP 接口。  
9） 等待相关接收中断的产生，及时处理接收数据。

### 25.3.3 捕获模式说明

DVP 接口支持两种捕获模式：快照（单帧）模式和连续模式。

#### 25.3.3.1 快照模式

在该模式下，只捕获单帧（R8_DVP_CR1寄存器中 RB_DVP_CM置1），当使能DVP接口后，等待系统检测帧起始，之后开始进行图像数据采样，在接收完整一帧数据后，将会关闭 DVP 接口（R8_DVP_CR0 寄存器的 RB_DVP_ENABLE 字段清 0）。快照模式下的单帧捕获波形如图 25-3 所示。

![](images/201baecc85b6c77b20ed65b1648e989e2b7792ef53bde07a27ea52afd916c34e.jpg)  
图25-3 快照模式下的单帧捕获波形

#### 25.3.3.2 连续模式

在该模式下（R8_DVP_CR1 寄存器中 RB_DVP_CM 清 0），当使能 DVP 接口后，在 R8_DVP_CR0 寄存器的 RB_DVP_ENABLE字段清0前，会持续采样每帧数据。连续模式下的帧捕获波形如图 25-4 所示。

![](images/d5f72d0c7f6d74efd5b8596513319b5380c474570a3f4f157f7c1ba9058cedfe.jpg)  
图25-4 连续模式下帧捕获波形

### 25.3.4 裁剪功能说明

DVP 可以使用裁剪功能从接收到的图像中截取一个矩形窗口，该矩形窗口起始坐标（矩形左上角 X 坐标 R16_DVP_HOFFCNT，Y 坐标 R16_DVP_VST）和窗口大小（R16_DVP_CAPCNT 表示水平尺寸、R16_DVP_VLINE表示垂直尺寸）可配置。裁剪窗口的坐标和大小如图 25-5所示。裁剪窗口数据捕获波形如图25-6所示。

![](images/e17213d1f64073f6490afeaaa23841c685fc7defd7429ce30e5624ddef1ab984.jpg)  
图 25-5 裁剪窗口的坐标和大小

![](images/0a39095c7b05ba6ba2734fd070008148b5fba96c90c2e262230384a20d7a14d7.jpg)  
图25-6 裁剪窗口数据捕获波形

![](images/726f68d1520e9a1da270738db1e66fb8a9ed0011e57faeb99893d6c0a20f8b86.jpg)  
在此阶段未捕获的数据

![](images/2686616cf1905ebeea7a18f85f5acc83cbabbbe901c44a68ff0553610939cec3.jpg)  
在此阶段捕获的数据

## 25.4 寄存器描述

表 25-2 DVP 相关寄存器列表  

<table><tr><td>名称</td><td>访问地址</td><td>描述</td><td>复位值</td></tr><tr><td>R8_DVP_CRO</td><td>0x50050000</td><td>DVP 控制寄存器 0</td><td>0x00</td></tr><tr><td>R8_DVP_CR1</td><td>0x50050001</td><td>DVP 控制寄存器 1</td><td>0x06</td></tr><tr><td>R8_DVP_IER</td><td>0x50050002</td><td>DVP 中断使能寄存器</td><td>0x00</td></tr><tr><td>R16_DVP_ROW_NUM</td><td>0x50050004</td><td>图像行数配置寄存器</td><td>0x0000</td></tr><tr><td>R16_DVP_COL_NUM</td><td>0x50050006</td><td>图像列数配置寄存器</td><td>0x0000</td></tr><tr><td>R32_DVP_DMA BUF0</td><td>0x50050008</td><td>DVP DMA 地址 0 寄存器</td><td>0x00000000</td></tr><tr><td>R32_DVP_DMA BUF1</td><td>0x5005000C</td><td>DVP DMA 地址 1 寄存器</td><td>0x00000000</td></tr><tr><td>R8_DVP_IFR</td><td>0x50050010</td><td>DVP 中断标志寄存器</td><td>0x00</td></tr><tr><td>R8_DVP_STATUS</td><td>0x50050011</td><td>DVP 接收 FIFO 状态寄存器</td><td>0x00</td></tr><tr><td>R16_DVP_ROW_CNT</td><td>0x50050014</td><td>DVP 行计数器寄存器</td><td>0x0000</td></tr><tr><td>R16_DVP_HOFFCNT</td><td>0x50050018</td><td>窗口开始的水平方向位移寄存器</td><td>0x0000</td></tr><tr><td>R16_DVP_VST</td><td>0x5005001A</td><td>窗口开始的行数寄存器</td><td>0x0000</td></tr><tr><td>R16_DVP_CAPCNT</td><td>0x5005001C</td><td>捕获计数寄存器</td><td>0x0000</td></tr><tr><td>R16_DVP_VLINE</td><td>0x5005001E</td><td>垂直行计数寄存器</td><td>0x0000</td></tr><tr><td>R32_DVP_DR</td><td>0x50050020</td><td>数据寄存器</td><td>0x00000000</td></tr></table>

### 25.4.1 DVP 配置寄存器（R8_DVP_CR0）

偏移地址： $0 \times 0 0$   

<table><tr><td>位</td><td>名称</td><td>访问</td><td>描述</td><td>复位值</td></tr><tr><td>6</td><td>RB_DVP_JPEG</td><td>RW</td><td>JPEG模式使能。1:JPEG压缩格式;0:原始数据格式。</td><td>0</td></tr><tr><td>[5:4]</td><td>RB_DVP_MSB_DAT_MOD</td><td>RW</td><td>DVP数据位宽配置。00:8位模式;01:10位模式;1x:12位模式。</td><td>00b</td></tr><tr><td>3</td><td>RB_DVP_P_POLAR</td><td>RW</td><td>PCLK极性配置。</td><td>0</td></tr><tr><td></td><td></td><td></td><td>1: 在 PCLK 下降沿采样数据;0: 在 PCLK 上升沿采样数据。</td><td></td></tr><tr><td>2</td><td>RB_DVP_H_POLAR</td><td>RW</td><td>HSYNC 极性配置。1: HSYNC 低电平数据有效;0: HSYNC 高电平数据有效。</td><td>0</td></tr><tr><td>1</td><td>RB_DVP_V_POLAR</td><td>RW</td><td>VSYNC 极性配置。1: VSYNC 高电平数据有效;0: VSYNC 低电平数据有效。</td><td>0</td></tr><tr><td>0</td><td>RB_DVP_ENABLE</td><td>RW</td><td>DVP 功能使能。1: 使能 DVP;0: 禁用 DVP。</td><td>0</td></tr></table>

### 25.4.2 DVP 配置寄存器（R8_DVP_CR1）

偏移地址： $0 \times 0 1$   

<table><tr><td>位</td><td>名称</td><td>访问</td><td>描述</td><td>复位值</td></tr><tr><td>[7:6]</td><td>RB_DVP_FCRC</td><td>RW</td><td>DVP帧捕获率控制。00:捕获所有帧;01:每隔一帧捕获一次;10:每隔三帧捕获一次。11:保留</td><td>00b</td></tr><tr><td>5</td><td>RB_DVP_CROP</td><td>RW</td><td>裁剪功能控制。0:捕获完整图像;1:仅捕获剪裁寄存器所指定的窗口中的数据。</td><td>0</td></tr><tr><td>4</td><td>RB_DVP_CM</td><td>RW</td><td>捕获模式。0:连续模式;1:快照模式。</td><td>0</td></tr><tr><td>3</td><td>RB_DVPBUF_TOG</td><td>RWT</td><td>缓冲地址标志位。硬件控制翻转,软件置1翻转该位,写0无效。1:数据存储在接收地址1;0:数据存储在接收地址0。</td><td>0</td></tr><tr><td>2</td><td>RB_DVP_RCV_CLR</td><td>RW</td><td>接收逻辑复位控制。1:复位接收逻辑电路;0:取消复位操作。</td><td>1</td></tr><tr><td>1</td><td>RB_DVP_ALL_CLR</td><td>RW</td><td>标志与FIFO清除控制,由软件写1或写0:1:复位标志与FIFO;0:取消复位操作。</td><td>1</td></tr><tr><td>0</td><td>RB_DVP_DMA_ENABLE</td><td>RW</td><td>DMA使能控制位:1:使能DMA;0:禁用DMA。</td><td>0</td></tr></table>

### 25.4.3 DVP 中断使能寄存器（R8_DVP_IER）

偏移地址： ${ 0 } { \times 0 } { 2 }$   

<table><tr><td>位</td><td>名称</td><td>访问</td><td>描述</td><td>复位值</td></tr><tr><td>[7:5]</td><td>Reserved</td><td>R0</td><td>保留。</td><td>0</td></tr><tr><td>4</td><td>RB_DVP_IE_STP_FRM</td><td>RW</td><td>帧结束中断使能。(在 VSYNC 从有效电平变为无效电平时产生中断。)1:使能帧结束中断;0:禁止帧结束中断。</td><td>0</td></tr><tr><td>3</td><td>RB_DVP_IE_FIFO_0V</td><td>RW</td><td>接收 FIFO 溢出中断使能。1:使能 FIFO 溢出中断;0:禁止 FIFO 溢出中断。</td><td>0</td></tr><tr><td>2</td><td>RB_DVP_IE_FRM_DONE</td><td>RW</td><td>帧接收完成中断使能。(计数器达到RAW/COL_NUM 配置值时产生中断,表示最后一个数据已写入 RAM)1:使能帧接收完成中断;0:禁止帧接收完成中断。</td><td>0</td></tr><tr><td>1</td><td>RB_DVP_IE_ROW_DONE</td><td>RW</td><td>行结束中断使能。(计数器达到 COL_NUM 配置值时产生中断)1:使能行结束中断;0:禁止行结束中断。</td><td>0</td></tr><tr><td>0</td><td>RB_DVP_IE_str_FRM</td><td>RW</td><td>新一帧开始中断使能。(在 VSYNC 从无效电平变为有效电平时产生中断,表示新的一帧开始,数据即将到来。)1:使能新一帧开始中断;0:禁止新一帧开始中断。</td><td>0</td></tr></table>

### 25.4.4 DVP 图像有效行数配置寄存器（R16_DVP_ROW_NUM）

偏移地址： $0 \times 0 4$   

<table><tr><td>位</td><td>名称</td><td>访问</td><td>描述</td><td>复位值</td></tr><tr><td>[15:0]</td><td>RB_DVP_ROW_NUM</td><td>RW</td><td>在 RGB 模式下，表示一帧图像数据内包含的行数。在 JPEG 模式下，该寄存器无实际意义。</td><td>0</td></tr></table>

### 25.4.5 DVP 图像有效列配置寄存器（16_DVP_COL_NUM）

偏移地址： $0 \times 0 6$   

<table><tr><td>位</td><td>名称</td><td>访问</td><td>描述</td><td>复位值</td></tr><tr><td>[15:0]</td><td>RB_DVP_COL_NUM</td><td>RW</td><td>在RGB模式下,表示一行数据内包含的PCLK周期数。在JPEG模式下,用于配置DMA接收长度。</td><td>0</td></tr></table>

### 25.4.6 DVP DMA 接收地址 0 寄存器（R32_DVP_DMA_BUF0）

偏移地址： $0 \times 0 8$   

<table><tr><td>位</td><td>名称</td><td>访问</td><td>描述</td><td>复位值</td></tr><tr><td>[31:17]</td><td>Reserved</td><td>R0</td><td>保留。</td><td>0</td></tr><tr><td>[16:0]</td><td>RB_DVP_DMA BUF0</td><td>RW</td><td>DMA 接收地址 0。</td><td>0</td></tr></table>

### 25.4.7 DVP DMA 接收地址 1 寄存器（R32_DVP_DMA_BUF1）

偏移地址： $0 \times 0 0$   

<table><tr><td>位</td><td>名称</td><td>访问</td><td>描述</td><td>复位值</td></tr><tr><td>[31:17]</td><td>Reserved</td><td>R0</td><td>保留。</td><td>0</td></tr><tr><td>[16:0]</td><td>RB_DVP_DMA BUF1</td><td>RW</td><td>DMA 接收地址 1。</td><td>0</td></tr></table>

### 25.4.8 DVP 中断标志寄存器（R8_DVP_IFR）

偏移地址： $0 \times 1 0$   

<table><tr><td>位</td><td>名称</td><td>访问</td><td>描述</td><td>复位值</td></tr><tr><td>[7:5]</td><td>Reserved</td><td>RO</td><td>保留。</td><td>0</td></tr><tr><td>4</td><td>RB_DVP_IF_STP_FRM</td><td>RW</td><td>帧结束中断标志,高有效,写0清除。</td><td>0</td></tr><tr><td>3</td><td>RB_DVP_IF_FIFO_OV</td><td>RW</td><td>接收FIFO溢出中断标志,高有效,写0清除。</td><td>0</td></tr><tr><td>2</td><td>RB_DVP_IF_FRM_DONE</td><td>RW</td><td>帧接收完成中断标志,高有效,写0清除。</td><td>0</td></tr><tr><td>1</td><td>RB_DVP_IF_ROW_DONE</td><td>RW</td><td>行结束中断标志,高有效,写0清除。</td><td>0</td></tr><tr><td>0</td><td>RB_DVP_IFSTR_FRM</td><td>RW</td><td>帧开始中断标志,高有效,写0清除。</td><td>0</td></tr></table>

### 25.4.9 DVP 接收 FIFO 状态寄存器（R8_DVP_STATUS）

偏移地址： $0 \times 1 1$   

<table><tr><td>位</td><td>名称</td><td>访问</td><td>描述</td><td>复位值</td></tr><tr><td>7</td><td>Reserved</td><td>R0</td><td>保留。</td><td>0</td></tr><tr><td>[6:4]</td><td>RB_DVP_FIFO_CNT</td><td>R0</td><td>FIFO计数器。</td><td>0</td></tr><tr><td>3</td><td>Reserved</td><td>R0</td><td>保留。</td><td>0</td></tr><tr><td>2</td><td>RB_DVP_FIFO_0V</td><td>R0</td><td>FIFO溢出状态。1: FIFO溢出; 0: FIFO未溢出。</td><td>0</td></tr><tr><td>1</td><td>RB_DVP_FIFO_FULL</td><td>R0</td><td>FIFO满状态。1: 缓存已满; 0: FIFO未满。</td><td>0</td></tr><tr><td>0</td><td>RB_DVP_FIFO_RDY</td><td>R0</td><td>FIFO就绪状态。1: FIFO中有数据; 0: FIFO中无数据。</td><td>0</td></tr></table>

### 25.4.10 DVP 接收图像行数寄存器（R16_DVP_ROW_CNT）

偏移地址： $0 \times 1 4$   

<table><tr><td>位</td><td>名称</td><td>访问</td><td>描述</td><td>复位值</td></tr><tr><td>[15:0]</td><td>RB_DVP_ROW_CNT</td><td>RO</td><td>实际接收中，一帧图像数据包含的行数，此寄存器在帧结束时更新。在JPEG格式下，该寄存器的值没有意义。</td><td>0</td></tr></table>

### 25.4.11 DVP 窗口开始的水平方向位移寄存器（R16_DVP_HOFFCNT）

偏移地址： $0 \times 1 8$   

<table><tr><td>位</td><td>名称</td><td>访问</td><td>描述</td><td>复位值</td></tr><tr><td>[15:0]</td><td>RB_DVP_HOFFCNT</td><td>RW</td><td>窗口行内,每行在捕获数据前需要空出PCLK周期数。低14位有效。</td><td>0</td></tr></table>

### 25.4.12 DVP 窗口开始的行数寄存器（R16_DVP_VST）

偏移地址： ${ 0 } { \times 1 } { \mathsf A }$   

<table><tr><td>位</td><td>名称</td><td>访问</td><td>描述</td><td>复位值</td></tr><tr><td>[15:0]</td><td>RB_DVP_VST</td><td>RW</td><td>图像开始捕获的行数，此行之前的数据不捕获。低13位有效。</td><td>0</td></tr></table>

### 25.4.13 DVP 捕获计数寄存器（R16_DVP_CAPCNT）

偏移地址： ${ 0 } { \times 1 } { 0 }$   

<table><tr><td>位</td><td>名称</td><td>访问</td><td>描述</td><td>复位值</td></tr><tr><td>[15:0]</td><td>RB_DVP_CAPCNT</td><td>RW</td><td>裁剪窗口内需要捕获的PCLK周期数。低14位有效。</td><td>0</td></tr></table>

### 25.4.14 DVP 垂直行计数寄存器（R16_DVP_VLINE）

偏移地址：0x1E  

<table><tr><td>位</td><td>名称</td><td>访问</td><td>描述</td><td>复位值</td></tr><tr><td>[15:0]</td><td>RB_DVP_VLINE</td><td>RW</td><td>裁剪窗口内需要捕获的行数。低14位有效。</td><td>0</td></tr></table>

### 25.4.15 DVP 数据寄存器（R32_DVP_DR）

偏移地址： $_ { 0 \times 2 0 }$   

<table><tr><td>位</td><td>名称</td><td>访问</td><td>描述</td><td>复位值</td></tr><tr><td>[31:0]</td><td>RB_DVP_DR</td><td>RO</td><td>DVP接口每次接收4字节数据,才会触发一次DMA请求,4字节深度的FIFO可为DMA传输留有充足的时间。可有效防止出现DMA溢出情况。</td><td>0</td></tr></table>

# 第 26 章 可变静态存储控制器（FSMC）

本章模块描述适用于CH32F2x、CH32V2x和CH32V3x微控制器系列部分产品。

可变静态存储控制器（FSMC），支持多种静态存储器类型以及丰富的存储操作方法，可根据系统应用需要，对不同类型大容量静态存储器进行扩展。

## 26.1 主要特性

支持操作SRAM、ROM、NOR FLASH和PSRAM  
$\bullet$ 支持操作NAND FLASH，并且内置硬件ECC，最多可检测8KByte数据  
$\bullet$ 支持对同步器件操作，如PSRAM  
支持8bit或16bit数据总线宽度  
时序信号可软件编程

## 26.2 功能描述

### 26.2.1 模块结构

FSMC 主要包括 AHB 总线接口、NOR 存储控制器、NAND 存储控制器和外部设备接口。

![](images/6467b4925735705a9d919f5b202aba62b0e8e4d930691641ba34f8840e4e49e9.jpg)  
图 26-1 FSMC 模块框图

### 26.2.2 外部设备地址映射

FSMC根据地址线将存储块分为固定大小 16MByte的两个存储块，见图 26-2。

![](images/4efaeb7dedf90fe5c563255580b808ff2362935fe364f720f5d1b826b33b4150.jpg)  
图 26-2 FSMC 存储块

#### 26.2.2.1 NOR/PSRAM 地址映射

表26-1 外部存储器地址  

<table><tr><td>数据宽度</td><td>配连接到存储器的地址线</td><td>最大访问存储器空间(bit)</td></tr><tr><td>8bit</td><td>HADDR[23:0]与FSMC_A[23:0]对应相连</td><td>16MBx8=128Mbit</td></tr><tr><td>16bit</td><td>HADDR[23:1]与FSMC_A[22:0]对应相连，HADDR[0]未接</td><td>16MB/2x16=128Mbit</td></tr></table>

注：100脚芯片，支持FSMC，并且仅支持地址和数据线复用模式。

NOR FLASH和PSRAM支持非对齐访问。对于异步模式，每次操作需要准确的地址；对于同步模式，只需发出一次地址信号，之后批量的数据通过 CLK 顺序进行。对于支持非对齐批量访问的 NORFLASH，可以将存储器的非对齐访问模式设置为和 AHB相同的模式，若不能设置，则禁用非对齐访问模式，并把非对齐的访问请求分开成两个连续的访问操作。

#### 26.2.2.2 NAND 地址映射

NAND FLASH 映像和时序寄存器，见表 26-2。

表26-2 存储器映像和时序寄存器  

<table><tr><td>起始地址</td><td>结束地址</td><td>FSMC 存储块</td><td>存储空间</td><td>时序寄存器</td></tr><tr><td>0x78000000</td><td>0x78FFFFFF</td><td rowspan="2">块 2-NAND FLASH</td><td>属性</td><td>FSMC_PATT2(0x6C)</td></tr><tr><td>0x70000000</td><td>0x70FFFFFF</td><td>通用</td><td>FSMC_PRMEM2(0x68)</td></tr></table>

通用和属性空间可以在低256KB划分为地址区（第二个 128KB区域）、命令区（第二个 64KB区域）和数据区（前64KB区域），见表26-3。

软件通过操作这 3 个区域访问 NAND FLASH 的具体流程：

向 NAND FLASH 发送读写等操作命令：软件可以操作命令区任何地址发送命令；  
$\bullet$ 向 NAND FLASH 发送将要操作的地址：软件可以操作地址区任何地址发送命令；  
从 NAND FLASH 读写数据：软件可以操作数据区任何地址写入或读出数据；

表 26-3 NAND 存储块选择  

<table><tr><td>区域名称</td><td>HADDR[17:16]</td><td>地址范围</td></tr><tr><td>地址区</td><td>1X</td><td>0x020000-0x03FFFF</td></tr><tr><td>命令区</td><td>01</td><td>0x010000-0x01FFFF</td></tr><tr><td>数据区</td><td>00</td><td>0x000000-0x00FFFF</td></tr></table>

### 26.2.3 NOR/PSRAM 控制器

FSMC 支持 8bit、16bit 和 32bit 异步操作 SRAM 和 ROM，支持异步模式和突发模式操作 PSRAM，支持异步模式和突发模式操作 NOR FLASH。所有控制器的输出信号在内部时钟 HCLK 的上升沿改变，对于同步写模式（PSRAM）下，输出的数据在存储器时钟（CLK）的下降沿改变，具体参照同步传输和异步传输时序图。存储器的读写参数可软件配置，见表 26-4。

表 26-4 软件可控的 NOR/PSRAM 读写参数  

<table><tr><td>参数</td><td>读写方式</td><td>参数取值范围</td></tr><tr><td>地址建立时间</td><td>异步</td><td>1&lt;&lt; T &lt;&lt;16 (AHB HCLK)</td></tr><tr><td>地址保持时间</td><td>异步</td><td>1&lt;&lt; T &lt;&lt;16 (AHB HCLK)</td></tr><tr><td>数据建立时间</td><td>异步</td><td>2&lt;&lt; T &lt;&lt;256 (AHB HCLK)</td></tr><tr><td>总线恢复时间</td><td>异步或同步读</td><td>1&lt;&lt; T &lt;&lt;16 (AHB HCLK)</td></tr><tr><td>时钟分频因子</td><td>同步</td><td>2&lt;&lt; T &lt;&lt;16 (AHB HCLK)</td></tr><tr><td>数据产生时间</td><td>同步</td><td>2&lt;&lt; T &lt;&lt;17 (Memory CLK)</td></tr></table>

#### 26.2.3.1 外部存储器复用接口信号

NOR FLASH 和 PSRAM 接口，见表 26-5、表 26-6。

表 26-5 复用 NOR FLASH 接口  

<table><tr><td>FSMC引脚</td><td>方向</td><td>描述</td></tr><tr><td>CLK</td><td>输出</td><td>时钟线（仅用于同步突发模式）</td></tr><tr><td>A[23:16]</td><td>输出</td><td>地址线</td></tr><tr><td>AD[15:0]</td><td>输入/输出</td><td>16bit地址/数据线（复用）</td></tr><tr><td>NE[1]</td><td>输出</td><td>片选线</td></tr><tr><td>NOE</td><td>输出</td><td>输出使能</td></tr><tr><td>NWE</td><td>输出</td><td>写使能</td></tr><tr><td>NL(NADV)</td><td>输出</td><td>锁存使能</td></tr><tr><td>NWAIT</td><td>输入</td><td>等待信号线（仅用于NOR FLASH）</td></tr><tr><td>NBL[1]</td><td>输出</td><td>高字节使能（NUB）</td></tr><tr><td>NBL[0]</td><td>输出</td><td>低字节使能（NLB）</td></tr></table>

注：前缀“N”的信号，表示低电平有效。

表 26-6 复用 PSRAM 接口  

<table><tr><td>FSMC引脚</td><td>方向</td><td>描述</td></tr><tr><td>CLK</td><td>输出</td><td>时钟线（仅用于同步突发模式）</td></tr><tr><td>A[23:16]</td><td>输出</td><td>地址线</td></tr><tr><td>AD[15:0]</td><td>输入/输出</td><td>16bit地址/数据线（复用）</td></tr><tr><td>NE[1]</td><td>输出</td><td>片选线</td></tr><tr><td>NOE</td><td>输出</td><td>输出使能</td></tr><tr><td>NWE</td><td>输出</td><td>写使能</td></tr><tr><td>NL (NADV)</td><td>输出</td><td>锁存使能</td></tr><tr><td>NWAIT</td><td>输入</td><td>等待信号线（仅用于 NOR FLASH）</td></tr></table>

#### 26.2.3.2 支持的存储器以及操作方式

支持的存储器以及操作方式见表 26-7。

表26-7 支持存储器和操作方式  

<table><tr><td>存储器</td><td>模式</td><td>AHB数据宽度</td><td>存储器宽度</td><td>描述</td></tr><tr><td rowspan="7">NOR FLASH</td><td>异步读</td><td>8</td><td>16</td><td></td></tr><tr><td>异步读</td><td>16</td><td>16</td><td></td></tr><tr><td>异步写</td><td>16</td><td>16</td><td></td></tr><tr><td>异步读</td><td>32</td><td>16</td><td>分成2次FSMC访问</td></tr><tr><td>异步写</td><td>32</td><td>16</td><td>分成2次FSMC访问</td></tr><tr><td>同步读</td><td>16</td><td>16</td><td></td></tr><tr><td>同步读</td><td>32</td><td>16</td><td>分成2次FSMC访问</td></tr><tr><td rowspan="10">PSRAM</td><td>异步读</td><td>8</td><td>16</td><td></td></tr><tr><td>异步写</td><td>8</td><td>16</td><td>使用字节信NBL[1:0]</td></tr><tr><td>异步读</td><td>16</td><td>16</td><td></td></tr><tr><td>异步写</td><td>16</td><td>16</td><td></td></tr><tr><td>异步读</td><td>32</td><td>16</td><td>分成2次FSMC访问</td></tr><tr><td>异步写</td><td>32</td><td>16</td><td>分成2次FSMC访问</td></tr><tr><td>同步读</td><td>16</td><td>16</td><td></td></tr><tr><td>同步读</td><td>32</td><td>16</td><td>分成2次FSMC 访问</td></tr><tr><td>同步写</td><td>8</td><td>16</td><td>使用字节信NBL[1:0]</td></tr><tr><td>同步写</td><td>16/32</td><td>16</td><td></td></tr><tr><td rowspan="2">SRAM和ROM</td><td>异步读</td><td>8/16/32</td><td>8/16</td><td>使用字节信NBL[1:0]</td></tr><tr><td>异步写</td><td>8/16/32</td><td>8/16</td><td>使用字节信NBL[1:0]</td></tr></table>

#### 26.2.3.3 NOR/PSRAM 异步传输地址/数据复用的时序图

对于异步静态存储器（NOR FLASH 和 PSRAM）操作需注意如下：

当启用扩展模式时，可混合使用模式 A、B、C 和 D 对存储器的读写操作。

![](images/b326a7ddb79f80aba1eff7a2232c24e795544bda3e8781148504373c9c92d8ef.jpg)  
图26-3 模式 1读操作（复用）

![](images/228bfe231ac580e0f8d61815e29be420d8611104038866d47606373ffa26b2b0.jpg)  
图 26-4 模式 1 写操作（复用）

表 26-8 模式 1 FSMC_BCR1 位域  

<table><tr><td>Bitx</td><td>名称</td><td>配置值</td></tr><tr><td>[31:20]</td><td>Reserved</td><td>0x000</td></tr><tr><td>19</td><td>CBURSTRW</td><td>0x0</td></tr><tr><td>[18:16]</td><td>Reserved</td><td>0x0</td></tr><tr><td>15</td><td>ASYNCWAIT</td><td>如果存储器支持该功能则为1,否则为0</td></tr><tr><td>14</td><td>EXTMOD</td><td>0x0</td></tr><tr><td>13</td><td>WAITEN</td><td>0x0</td></tr><tr><td>12</td><td>WREN</td><td>根据需要设置</td></tr><tr><td>11</td><td>WAITCFG</td><td>该位不起作用</td></tr><tr><td>10</td><td>WRAPMOD</td><td>0x0</td></tr><tr><td>9</td><td>WAITPOL</td><td>当bit15为1时,有意义</td></tr><tr><td>8</td><td>BURSTEN</td><td>0x0</td></tr><tr><td>7</td><td>Reserved</td><td>0x1</td></tr><tr><td>6</td><td>FACCEN</td><td>该位不起作用</td></tr><tr><td>[5:4]</td><td>MWID</td><td>根据需要设置</td></tr><tr><td>[3:2]</td><td>MTYP</td><td>根据需要设置,不包含0x2(NOR FLASH)</td></tr><tr><td>1</td><td>MUXEN</td><td>0x1</td></tr><tr><td>0</td><td>MBKEN</td><td>0x1</td></tr></table>

表 26-9 模式 1 FSMC_BTR1 位域  

<table><tr><td>Bitx</td><td>名称</td><td>配置值</td></tr><tr><td>[31:30]</td><td>Reserved</td><td>0x0</td></tr><tr><td>[29:28]</td><td>ACCMOD</td><td>该位不起作用</td></tr><tr><td>[27:24]</td><td>DATLAT</td><td>该位不起作用</td></tr><tr><td>[23:20]</td><td>CLKDIV</td><td>该位不起作用</td></tr><tr><td>[19:16]</td><td>BUSTURN</td><td>0x0</td></tr><tr><td>[15:8]</td><td>DATAST</td><td>数据建立时间。写操作为（DATAST+1 HCLK），读操作为（DATAST+3 HCLK）。该位最小为1。</td></tr><tr><td>[7:4]</td><td>ADDHLD</td><td>地址保持时间。（ADDHLD+1 HCLK）</td></tr><tr><td>[3:0]</td><td>ADDSET</td><td>地址建立时间。（ADDSET+1 HCLK）。</td></tr></table>

![](images/6f28b23c07423ad679dc90e1e3d90370067a50ed3159c239f55ff35f8e1e6198.jpg)  
图26-5 模式 A读操作（复用）

![](images/c9e27519711cb0d19b77fe0def2d888cb61df04f26547205df58910722437f4a.jpg)  
图26-6 模式 A写操作（复用）

表 26-10 模式 A FSMC_BCR1 位域  

<table><tr><td>Bitx</td><td>名称</td><td>配置值</td></tr><tr><td>[31:20]</td><td>Reserved</td><td>0x000</td></tr><tr><td>19</td><td>CBURSTRW</td><td>0x0</td></tr><tr><td>[18:16]</td><td>Reserved</td><td>0x0</td></tr><tr><td>15</td><td>ASYNCWAIT</td><td>如果存储器支持该功能则为1,否则为0</td></tr><tr><td>14</td><td>EXTMOD</td><td>0x1</td></tr><tr><td>13</td><td>WAITEN</td><td>0x0</td></tr><tr><td>12</td><td>WREN</td><td>根据需要设置</td></tr><tr><td>11</td><td>WAITCFG</td><td>该位不起作用</td></tr><tr><td>10</td><td>WRAPMOD</td><td>0x0</td></tr><tr><td>9</td><td>WAITPOL</td><td>当bit15为1时,有意义</td></tr><tr><td>8</td><td>BURSTEN</td><td>0x0</td></tr><tr><td>7</td><td>Reserved</td><td>0x1</td></tr><tr><td>6</td><td>FACCEN</td><td>该位不起作用</td></tr><tr><td>[5:4]</td><td>MWID</td><td>根据需要设置</td></tr><tr><td>[3:2]</td><td>MTYP</td><td>根据需要设置,不包含0x2(NOR FLASH)</td></tr><tr><td>1</td><td>MUXEN</td><td>0x1</td></tr><tr><td>0</td><td>MBKEN</td><td>0x1</td></tr></table>

表 26-11 模式 A FSMC_BTR1 位域  

<table><tr><td>Bitx</td><td>名称</td><td>配置值</td></tr><tr><td>[31:30]</td><td>Reserved</td><td>0x0</td></tr><tr><td>[29:28]</td><td>ACCMOD</td><td>0x0</td></tr><tr><td>[27:24]</td><td>DATLAT</td><td>该位不起作用</td></tr><tr><td>[23:20]</td><td>CLKDIV</td><td>该位不起作用</td></tr><tr><td>[19:16]</td><td>BUSTYRN</td><td>0x0</td></tr><tr><td>[15:8]</td><td>DATAST</td><td>数据建立时间。写操作为（DATAST+1HCLK），读操作为（DATAST+3HCLK）。该位最小为1。</td></tr><tr><td>[7:4]</td><td>ADDHLD</td><td>该位不起作用</td></tr><tr><td>[3:0]</td><td>ADDSET</td><td>地址建立时间。读操作为（ADDSET+1HCLK）。</td></tr></table>

表 26-12 模式 A FSMC_BWTR1 位域  

<table><tr><td>Bitx</td><td>名称</td><td>配置值</td></tr><tr><td>[31:30]</td><td>Reserved</td><td>0x0</td></tr><tr><td>[29:28]</td><td>ACCMOD</td><td>0x0</td></tr><tr><td>[27:24]</td><td>DATLAT</td><td>该位不起作用</td></tr><tr><td>[23:20]</td><td>CLKDIV</td><td>该位不起作用</td></tr><tr><td>[19:16]</td><td>BUSTURN</td><td>0x0</td></tr><tr><td>[15:8]</td><td>DATAST</td><td>数据建立时间。写操作为（DATAST+1</td></tr><tr><td></td><td></td><td>HCLK),读操作为(DATAST+3 HCLK)。该位最小为1。</td></tr><tr><td>[7:4]</td><td>ADDHLD</td><td>地址保持时间。(ADDHLD+1 HCLK)</td></tr><tr><td>[3:0]</td><td>ADDSET</td><td>地址建立时间。写操作为(ADDSET+1 HCLK)。</td></tr></table>

![](images/4113ee691a8e4e512aeacbebce4960b85abd37a19734783ab656db0cbb21b1e9.jpg)  
图26-7 模式 B读操作（复用）

![](images/ad054713386d468b877b5808ad80fb8c2f143b36dfbc6a4f0b74c81861c92892.jpg)  
图26-8 模式 B写操作（复用）

表 26-13 模式 B FSMC_BCR1 位域  

<table><tr><td>Bitx</td><td>名称</td><td>配置值</td></tr><tr><td>[31:20]</td><td>Reserved</td><td>0x000</td></tr><tr><td>19</td><td>CBURSTRW</td><td>0x0</td></tr><tr><td>[18:16]</td><td>Reserved</td><td>0x0</td></tr><tr><td>15</td><td>ASYNCWAIT</td><td>如果存储器支持该功能则为1,否则为0</td></tr><tr><td>14</td><td>EXTMOD</td><td>0x1</td></tr><tr><td>13</td><td>WAITEN</td><td>0x0</td></tr><tr><td>12</td><td>WREN</td><td>根据需要设置</td></tr><tr><td>11</td><td>WAITCFG</td><td>该位不起作用</td></tr><tr><td>10</td><td>WRAPMOD</td><td>0x0</td></tr><tr><td>9</td><td>WAITPOL</td><td>当bit15为1时,有意义。</td></tr><tr><td>8</td><td>BURSTEN</td><td>0x0</td></tr><tr><td>7</td><td>Reserved</td><td>0x1</td></tr><tr><td>6</td><td>FACCEN</td><td>0x1</td></tr><tr><td>[5:4]</td><td>MWID</td><td>根据需要设置</td></tr><tr><td>[3:2]</td><td>MTYP</td><td>0x2(NOR FLASH)</td></tr><tr><td>1</td><td>MUXEN</td><td>0x1</td></tr><tr><td>0</td><td>MBKEN</td><td>0x1</td></tr></table>

表 26-14 模式 B FSMC_BTR1 位域  

<table><tr><td>Bitx</td><td>名称</td><td>配置值</td></tr><tr><td>[31:30]</td><td>Reserved</td><td>0x0</td></tr><tr><td>[29:28]</td><td>ACCMOD</td><td>0x1</td></tr><tr><td>[27:24]</td><td>DATLAT</td><td>该位不起作用</td></tr><tr><td>[23:20]</td><td>CLKDIV</td><td>该位不起作用</td></tr><tr><td>[19:16]</td><td>BUSTURN</td><td>0x0</td></tr><tr><td>[15:8]</td><td>DATAST</td><td>数据建立时间。写操作为（DATAST+1 HCLK），读操作为（DATAST+3 HCLK）。该位最小为1。</td></tr><tr><td>[7:4]</td><td>ADDHLD</td><td>地址保持时间。（ADDHLD+1 HCLK）</td></tr><tr><td>[3:0]</td><td>ADDSET</td><td>地址建立时间。读操作为（ADDSET+1 HCLK）。</td></tr></table>

表 26-15 模式 B FSMC_BWTR1 位域  

<table><tr><td>Bitx</td><td>名称</td><td>配置值</td></tr><tr><td>[31:30]</td><td>Reserved</td><td>0x0</td></tr><tr><td>[29:28]</td><td>ACCMOD</td><td>0x1</td></tr><tr><td>[27:24]</td><td>DATLAT</td><td>该位不起作用</td></tr><tr><td>[23:20]</td><td>CLKDIV</td><td>该位不起作用</td></tr><tr><td>[19:16]</td><td>BUSTURN</td><td>0x0</td></tr><tr><td>[15:8]</td><td>DATAST</td><td>数据建立时间。写操作为（DATAST+1 HCLK），读操作为（DATAST+3 HCLK）。该位最小为1。</td></tr><tr><td>[7:4]</td><td>ADDHLD</td><td>地址保持时间。（ADDHLD+1 HCLK）</td></tr><tr><td>[3:0]</td><td>ADDSET</td><td>地址建立时间。写操作为（ADDSET+1 HCLK）。</td></tr></table>

![](images/09d2136474f5e255b2efbf895633b15161879ce12696d04daa98d24d6fc186a0.jpg)  
图26-9 模式 C读操作（复用）

![](images/2bb0d89d071f9003e87f6ca76c58ae243760f2d76855bbbcabec8696d4b8fb8e.jpg)  
图 26-10 模式 C 写操作（复用）

表 26-16 模式 C FSMC_BCR1 位域  

<table><tr><td>Bitx</td><td>名称</td><td>配置值</td></tr><tr><td>[31:20]</td><td>Reserved</td><td>0x000</td></tr><tr><td>19</td><td>CBURSTRW</td><td>0x0</td></tr><tr><td>[18:16]</td><td>Reserved</td><td>0x0</td></tr><tr><td>15</td><td>ASYNCWAIT</td><td>如果存储器支持该功能则为1，否则为0</td></tr><tr><td>14</td><td>EXTMOD</td><td>0x1</td></tr><tr><td>13</td><td>WAITEN</td><td>0x0</td></tr><tr><td>12</td><td>WREN</td><td>根据需要设置</td></tr><tr><td>11</td><td>WAITCFG</td><td>该位不起作用</td></tr><tr><td>10</td><td>WRAPMOD</td><td>0x0</td></tr><tr><td>9</td><td>WAITPOL</td><td>当bit15为1时，有意义。</td></tr><tr><td>8</td><td>BURSTEN</td><td>0x0</td></tr><tr><td>7</td><td>Reserved</td><td>0x1</td></tr><tr><td>6</td><td>FACCEN</td><td>0x1</td></tr><tr><td>[5:4]</td><td>MWID</td><td>根据需要设置</td></tr><tr><td>[3:2]</td><td>MTYP</td><td>0x2 (NOR FLASH)</td></tr><tr><td>1</td><td>MUXEN</td><td>0x1</td></tr><tr><td>0</td><td>MBKEN</td><td>0x1</td></tr></table>

表 26-17 模式 C FSMC_BTR1 位域  

<table><tr><td>Bitx</td><td>名称</td><td>配置值</td></tr><tr><td>[31:30]</td><td>Reserved</td><td>0x0</td></tr><tr><td>[29:28]</td><td>ACCMOD</td><td>0x2</td></tr><tr><td>[27:24]</td><td>DATLAT</td><td>该位不起作用</td></tr><tr><td>[23:20]</td><td>CLKDIV</td><td>该位不起作用</td></tr><tr><td>[19:16]</td><td>BUSTURN</td><td>0x0</td></tr><tr><td>[15:8]</td><td>DATAST</td><td>数据建立时间。写操作为（DATAST+1 HCLK），读操作为（DATAST+3 HCLK）。该位最小为1。</td></tr><tr><td>[7:4]</td><td>ADDHLD</td><td>地址保持时间。（ADDHLD+1 HCLK）</td></tr><tr><td>[3:0]</td><td>ADDSET</td><td>地址建立时间。读操作为（ADDSET+1 HCLK）。</td></tr></table>

表 26-18 模式 C FSMC_BWTR1 位域  

<table><tr><td>Bitx</td><td>名称</td><td>配置值</td></tr><tr><td>[31:30]</td><td>Reserved</td><td>0x0</td></tr><tr><td>[29:28]</td><td>ACCMOD</td><td>0x2</td></tr><tr><td>[27:24]</td><td>DATLAT</td><td>该位不起作用</td></tr><tr><td>[23:20]</td><td>CLKDIV</td><td>该位不起作用</td></tr><tr><td>[19:16]</td><td>BUSTURN</td><td>0x0</td></tr><tr><td>[15:8]</td><td>DATAST</td><td>数据建立时间。写操作为（DATAST+1 HCLK），读操作为（DATAST+3 HCLK）。该位最小为1。</td></tr><tr><td>[7:4]</td><td>ADDHLD</td><td>地址保持时间。（ADDHLD+1 HCLK）</td></tr><tr><td>[3:0]</td><td>ADDSET</td><td>地址建立时间。写操作为（ADDSET+1</td></tr></table>

![](images/46dc7d805f5af405146df23f65fb703674f96bf06a813b75deeb57577152d21c.jpg)

![](images/f7b93130ff32c82f43181087bb15e3ca24cf9055d87cf6f7eb3eb64efb88e0f2.jpg)  
图 26-11 模式 D 读操作（复用）

![](images/9c6283a7b3c76fcee77f1edbc4161f5c8897aa2b8fc6c6fc48c59331f1a4ea55.jpg)  
图 26-12 模式 D 写操作（复用）

表 26-19 模式 D FSMC_BCR1 位域  

<table><tr><td>Bitx</td><td>名称</td><td>配置值</td></tr><tr><td>[31:20]</td><td>Reserved</td><td>0x000</td></tr><tr><td>19</td><td>CBURSTRW</td><td>0x0</td></tr><tr><td>[18:16]</td><td>Reserved</td><td>0x0</td></tr><tr><td>15</td><td>ASYNCWAIT</td><td>如果存储器支持该功能则为1，否则为0</td></tr><tr><td>14</td><td>EXTMOD</td><td>0x1</td></tr><tr><td>13</td><td>WAITEN</td><td>0x0</td></tr><tr><td>12</td><td>WREN</td><td>根据需要设置</td></tr><tr><td>11</td><td>WAITCFG</td><td>该位不起作用</td></tr><tr><td>10</td><td>WRAPMOD</td><td>0x0</td></tr><tr><td>9</td><td>WAITPOL</td><td>当bit15为1时,有意义。</td></tr><tr><td>8</td><td>BURSTEN</td><td>0x0</td></tr><tr><td>7</td><td>Reserved</td><td>0x1</td></tr><tr><td>6</td><td>FACCEN</td><td>根据需要设置</td></tr><tr><td>[5:4]</td><td>MWID</td><td>根据需要设置</td></tr><tr><td>[3:2]</td><td>MTYP</td><td>根据需要设置</td></tr><tr><td>1</td><td>MUXEN</td><td>0x1</td></tr><tr><td>0</td><td>MBKEN</td><td>0x1</td></tr></table>

表 26-20 模式 D FSMC_BTR1 位域  

<table><tr><td>Bitx</td><td>名称</td><td>配置值</td></tr><tr><td>[31:30]</td><td>Reserved</td><td>0x0</td></tr><tr><td>[29:28]</td><td>ACCMOD</td><td>0x3</td></tr><tr><td>[27:24]</td><td>DATLAT</td><td>该位不起作用</td></tr><tr><td>[23:20]</td><td>CLKDIV</td><td>该位不起作用</td></tr><tr><td>[19:16]</td><td>BUSTURN</td><td>0x0</td></tr><tr><td>[15:8]</td><td>DATAST</td><td>数据建立时间。写操作为（DATAST+1 HCLK），读操作为（DATAST+3 HCLK）。该位最小为1。</td></tr><tr><td>[7:4]</td><td>ADDHLD</td><td>地址保持时间。（ADDHLD+1 HCLK）。</td></tr><tr><td>[3:0]</td><td>ADDSET</td><td>地址建立时间。读操作为（ADDSET+1 HCLK）。</td></tr></table>

表 26-21 模式 D FSMC_BWTR1 位域  

<table><tr><td>Bitx</td><td>名称</td><td>配置值</td></tr><tr><td>[31:30]</td><td>Reserved</td><td>0x0</td></tr><tr><td>[29:28]</td><td>ACCMOD</td><td>0x3</td></tr><tr><td>[27:24]</td><td>DATLAT</td><td>0x0</td></tr><tr><td>[23:20]</td><td>CLKDIV</td><td>0x0</td></tr><tr><td>[19:16]</td><td>BUSTYRN</td><td>0x0</td></tr><tr><td>[15:8]</td><td>DATAST</td><td>数据建立时间。写操作为（DATAST+1 HCLK），读操作为（DATAST+3 HCLK）。该位最小为1。</td></tr><tr><td>[7:4]</td><td>ADDHLD</td><td>地址保持时间。（ADDHLD+1 HCLK）。</td></tr><tr><td>[3:0]</td><td>ADDSET</td><td>地址建立时间。写操作为（ADDSET+1 HCLK）。</td></tr></table>

#### 26.2.3.4 NOR/PSRAM 同步传输地址/数据复用的时序图

数据延迟与NOR FLASH的延时需注意，DATALAT数值必须与 NOR FLASH配置寄存器中的定义一致。NADV 信号为低时的时钟周期不计算在延迟参数中。FSMC 的 DATLAT 参数可以为 $\mathsf { D A T A L A T } + 2$ 或为DATALAT+3，对于特别的存储器会在数据保持时间阶段产生 NWAIT 信号，DATLAT 需要设置为最小值，对于另外一些存储器不会在数据保持时间阶段产生 NWAIT 信号，FSMC 和存储器的数据保持时间

必须设置一致。

对于单次批量传输需注意，存储器配置位同步批量模式，若传输 16bit数据，FSMC 会执行1 次长度为1的批量传输，若传输32bit数据，FSMC分为 2次 16bit 传输，执行1 次长度为2 的批量传输。

NOR FLASH同步批量模式访问时，在保持时间（DATLAT+1 CLK）之后，若检测 NWAIT 信号为低电平时，在NWAIT变成高电平之前FSMC需要插入等待周期，当 NWAIT 变为高电平时，FSMC认为数据有效。在NWAIT信号控制的等待状态插入期间，控制器会持续向存储器发送时钟脉冲、保持片选信号和输出有效信号，同时忽略无效的数据信号。在批量传输模式下，NOR FLASH 的 NWAIT 信号通过配置WAITCFG位选择两种时序配置。

![](images/e238195dee8e0da378896b7d47a893a5ceda23edcd04e89f2e67e22b5efe8fa0.jpg)  
图26-13 同步总线复用读操作（复用）

表 26-22 模式 D FSMC_BCR1 位域  

<table><tr><td>Bitx</td><td>名称</td><td>配置值</td></tr><tr><td>[31:20]</td><td>Reserved</td><td>0x000</td></tr><tr><td>19</td><td>CBURSTRW</td><td>该位不起作用</td></tr><tr><td>[18:16]</td><td>Reserved</td><td>0x0</td></tr><tr><td>15</td><td>ASYNCWAIT</td><td>0x0</td></tr><tr><td>14</td><td>EXTMOD</td><td>0x0</td></tr><tr><td>13</td><td>WAITEN</td><td>0x0</td></tr><tr><td>12</td><td>WREN</td><td>该位不起作用</td></tr><tr><td>11</td><td>WAITCFG</td><td>根据需要设置</td></tr><tr><td>10</td><td>WRAPMOD</td><td>0x0</td></tr><tr><td>9</td><td>WAITPOL</td><td>根据需要设置</td></tr><tr><td>8</td><td>BURSTEN</td><td>0x1</td></tr><tr><td>7</td><td>Reserved</td><td>0x1</td></tr><tr><td>6</td><td>FACCEN</td><td>根据需要设置</td></tr><tr><td>[5:4]</td><td>MWID</td><td>根据需要设置</td></tr><tr><td>[3:2]</td><td>MTYP</td><td>0x1或0x2</td></tr><tr><td>1</td><td>MUXEN</td><td>0x1</td></tr><tr><td>0</td><td>MBKEN</td><td>0x1</td></tr></table>

表 26-23 模式 D FSMC_BTR1 位域  

<table><tr><td>Bitx</td><td>名称</td><td>配置值</td></tr><tr><td>[31:30]</td><td>Reserved</td><td>0x0</td></tr><tr><td>[29:28]</td><td>ACCMOD</td><td>0x0</td></tr><tr><td>[27:24]</td><td>DATLAT</td><td>数据保持时间</td></tr><tr><td>[23:20]</td><td>CLKDIV</td><td>0x0 - CLK=1 HCLK
0x1 - CLK=2 HCLK</td></tr><tr><td>[19:16]</td><td>BUSTURN</td><td>该位不起作用</td></tr><tr><td>[15:8]</td><td>DATAST</td><td>该位不起作用</td></tr><tr><td>[7:4]</td><td>ADDHLD</td><td>该位不起作用</td></tr><tr><td>[3:0]</td><td>ADDSET</td><td>该位不起作用</td></tr></table>

![](images/49c118d4a0985da4b2e186f02e81d54d53f9c728e080a59ef23a3ca8e6f1b39a.jpg)  
图 26-14 同步总线复用写操作（复用）

表 26-24 模式 D FSMC_BCR1 位域  

<table><tr><td>Bitx</td><td>名称</td><td>配置值</td></tr><tr><td>[31:20]</td><td>Reserved</td><td>0x000</td></tr><tr><td>19</td><td>CBURSTRW</td><td>0x1</td></tr><tr><td>[18:16]</td><td>Reserved</td><td>0x0</td></tr><tr><td>15</td><td>ASYNCWAIT</td><td>0x0</td></tr><tr><td>14</td><td>EXTMOD</td><td>0x0</td></tr><tr><td>13</td><td>WAITEN</td><td>如果存储器支持该功能则为1,否则为0</td></tr><tr><td>12</td><td>WREN</td><td>0x1</td></tr><tr><td>11</td><td>WAITCFG</td><td>0x0</td></tr><tr><td>10</td><td>WRAPMOD</td><td>0x0</td></tr><tr><td>9</td><td>WAITPOL</td><td>根据需要设置</td></tr><tr><td>8</td><td>BURSTEN</td><td>该位不起作用</td></tr><tr><td>7</td><td>Reserved</td><td>0x1</td></tr><tr><td>6</td><td>FACCEN</td><td>根据需要设置</td></tr><tr><td>[5:4]</td><td>MWID</td><td>根据需要设置</td></tr><tr><td>[3:2]</td><td>MTYP</td><td>0x1</td></tr><tr><td>1</td><td>MUXEN</td><td>0x1</td></tr><tr><td>0</td><td>MBKEN</td><td>0x1</td></tr></table>

表 26-25 模式 D FSMC_BTR1 位域  

<table><tr><td>Bitx</td><td>名称</td><td>配置值</td></tr><tr><td>[31:30]</td><td>Reserved</td><td>0x0</td></tr><tr><td>[29:28]</td><td>ACCMOD</td><td>0x0</td></tr><tr><td>[27:24]</td><td>DATLAT</td><td>数据保持时间</td></tr><tr><td>[23:20]</td><td>CLKDIV</td><td>0x0 - CLK=1 HCLK
0x1 - CLK=2 HCLK</td></tr><tr><td>[19:16]</td><td>BUSTURN</td><td>该位不起作用</td></tr><tr><td>[15:8]</td><td>DATAST</td><td>该位不起作用</td></tr><tr><td>[7:4]</td><td>ADDHLD</td><td>该位不起作用</td></tr><tr><td>[3:0]</td><td>ADDSET</td><td>该位不起作用</td></tr></table>

### 26.2.4 NAND FLASH 控制器

FSMC 支持 8bit、16bit 操作 NAND FLASH，FSMC 可以根据需要产生多个地址周期，理论上 FSMC对 NAND FLASH 的容量没有限制。NAND FLASH 的读写参数可软件配置，见表 26-26。

表 26-26 软件可控的 NAND 读写参数  

<table><tr><td>参数</td><td>功能</td><td>读写方式</td><td>参数取值范围</td></tr><tr><td>存储器建立时间</td><td>发出命令之前建立地址的时间</td><td>读/写</td><td>1&lt;&lt; T &lt;&lt;256 (AHB HCLK)</td></tr><tr><td>存储器等待时间</td><td>发出命令的最短持续时间</td><td>读/写</td><td>1&lt;&lt; T &lt;&lt;256 (AHB HCLK)</td></tr><tr><td>存储器保持时间</td><td>在发送命令结束后保持地址的时间,写操作时也是数据的保持时间</td><td>读/写</td><td>1&lt;&lt; T &lt;&lt;255 (AHB HCLK)</td></tr><tr><td>存储器数据总线高阻时间</td><td>启动写操作之后数据总线持续为高阻态时间</td><td>写</td><td>0&lt;&lt; T &lt;&lt;255 (AHB HCLK)</td></tr></table>

#### 26.2.4.1 外部存储器接口信号

8bit 和 16bit NAND FLASH 接口，见表 26-27、表 26-28。

表 26-27 8bit NAND FLASH  

<table><tr><td>FSMC引脚</td><td>方向</td><td>描述</td></tr><tr><td>A[17]</td><td>输出</td><td>地址锁存允许信号（ALE）</td></tr><tr><td>A[16]</td><td>输出</td><td>命令锁存允许信号（CLE）</td></tr><tr><td>D[7:0]</td><td>输入/输出</td><td>8bit双向地址/数据复用总线</td></tr><tr><td>NCE[2]</td><td>输出</td><td>片选线</td></tr><tr><td>NOE</td><td>输出</td><td>输出使能</td></tr><tr><td>NWE</td><td>输出</td><td>写使能</td></tr><tr><td>NWAIT</td><td>输入</td><td>等待信号线</td></tr></table>

表 26-28 16bit NAND FLASH  

<table><tr><td>FSMC引脚</td><td>方向</td><td>描述</td></tr><tr><td>A[17]</td><td>输出</td><td>地址锁存允许信号（ALE）</td></tr><tr><td>A[16]</td><td>输出</td><td>命令锁存允许信号（CLE）</td></tr><tr><td>D[15:0]</td><td>输入/输出</td><td>16bit双向地址/数据复用总线</td></tr><tr><td>NCE[2]</td><td>输出</td><td>片选线</td></tr><tr><td>NOE</td><td>输出</td><td>输出使能</td></tr><tr><td>NWE</td><td>输出</td><td>写使能</td></tr><tr><td>NWAIT</td><td>输入</td><td>等待信号线</td></tr></table>

#### 26.2.4.2 支持的存储器以及操作方式

支持的存储器以及操作方式见表 26-29。

表26-29 支持存储器和操作方式  

<table><tr><td>存储器</td><td>模式</td><td>AHB数据宽度</td><td>存储器宽度</td><td>描述</td></tr><tr><td rowspan="6">8bit NAND</td><td>异步读</td><td>8</td><td>8</td><td></td></tr><tr><td>异步写</td><td>8</td><td>8</td><td></td></tr><tr><td>异步读</td><td>16</td><td>8</td><td>分成2次FSMC访问</td></tr><tr><td>异步写</td><td>16</td><td>8</td><td>分成2次FSMC访问</td></tr><tr><td>异步读</td><td>32</td><td>8</td><td>分成4次FSMC访问</td></tr><tr><td>异步写</td><td>32</td><td>8</td><td>分成4次FSMC访问</td></tr><tr><td rowspan="5">16bit NAND</td><td>异步读</td><td>8</td><td>16</td><td></td></tr><tr><td>异步读</td><td>16</td><td>16</td><td></td></tr><tr><td>异步写</td><td>16</td><td>16</td><td></td></tr><tr><td>异步读</td><td>32</td><td>16</td><td>分成2次FSMC访问</td></tr><tr><td>异步写</td><td>32</td><td>16</td><td>分成2次FSMC访问</td></tr></table>

#### 26.2.4.3 NAND FLASH 时序图 （包括预等待功能）

NAND FLASH 操作时序控制，涉及 FSMC_PMEM2 和 FSMC_PATT2 时序寄存器，每一个寄存器包含 4个参数。MEMHOLD、MEMWAIT 和 ATTSET 三个参数对应操作 NAND FLASH 的三个阶段的 HCLK 周期数，MEMHIZ参数对应FSMC开始驱动数据总线的时机。NAND FLASH控制器通用存储空间访问时序，见图26-15。

![](images/1a89ea84112a35d7f9028d33f1e60fe9eb57d0a9817e7611a4ac83cf3cb113f4.jpg)  
图 26-15 NAND FLASH 控制器通用存储空间的访问时序

#### 26.2.4.4 NAND FLASH 操作流程

NAND FLASH的命令锁存使能信号（CLE）和地址锁存使能信号（ALE）分别由FSMC 地址线A16和A17驱动，故在对NAND FLASH操作发送命令或地址时，需要对存储空间的特定地址执行写操作。对 NAND FLASH 的读操作过程如下：

1） 根据即将操作的 NAND FLASH 特性，配置 FSMC_PCR2、FSMC_PMEM2 和 FSMC_PATT2 寄存器的PWID、PTYP、PWAITEN 和 PBKEN。  
2） 在通用存储空间写入命令字节，在NWE为低电平期间，CLE输出高电平，此时该命令字节被NAND FLASH 识别为一个命令，并锁存该命令，随后的页读操作不需要重复发送该命令。  
3） 之后通过向存储器空间写入4个字节，作为读操作的起始地址。在 NWE为低电平期间，ALE输出高电平，此时4个字节被NAND FLASH识别为读操作的起始地址。在操作属性存储空间时，可以使 FSMC产生不同的时序，实现某些 NAND FLASH预等待功能。  
4） FSMC 控制器开始新的操作前需要等待 NAND FLASH 的 R/NB 信号变为高电平，在等待期间 FSMC控制器需要保持NCE信号为低电平。  
5） FSMC 控制器可以通过操作通用存储空间，可以从 NAND FLASH 逐字节的读出存储页。

#### 26.2.4.5 NAND FLASH 预等待功能

某些特殊的NAND FLASH需要在输入最后一个地址字节后，R/NB 信号变为低电平，见图 26-16。图中（1）为CPU写字节命令 $0 \times 0 0$ 到 $0 \times 7 0 0 1 0 0 0 0$ ；图中（2）为 CPU 写 NAND FLASH 的地址 A7-A0 至$0 \times 7 0 0 2 0 0 0 0$ ；图中（3）为 CPU 写 NAND FLASH 的地址 A15-A8 至 $0 \times 7 0 0 2 0 0 0 0$ ；图中（4）为CPU写NAND FLASH 的地址 A23-A16 至 $0 \times 7 0 0 2 0 0 0 0$ ；图中（5）为 CPU 写 NAND FLASH 的地址 A31-A24 至0x70020000；

![](images/e19c3ea82c5cd8446f9a6805e436679b444ebf22b631246ec96945001e9dec26.jpg)  
图 26-16 操作特殊型 NAND FLASH

使用该功能时，通过配置FSMC_PMEM2寄存器的 MEMHOLD位来保证 tWB的时序，之后对 NANDFLASH的读写操作，FSMC控制器都会在NEW信号的上升沿至下一次操作之间插入（MEMHOLD+1）HCLK保持延时。为了解决该问题，这里使用属性空间配置 ATTHOLD数值使之符合tWB 的时序，同时需要保持 MEMHOLD 为最小值。此时，只有在写入 NAND FLASH 地址的最后一个字节时，FSMC 控制器需要写入属性存储空间，其余NAND FLASH的读写操作使用通用存储空间。

#### 26.2.4.6 NAND FLASH ECC 功能

FSMC的NAND FLASH控制器包含1个纠错码计算硬件模块，可有效减少 CPU在处理纠错码时的软件工作量。ECC 模块在读写 NAND FLASH 时，支持每 256、512、1024、2048、4096 或 8192 个字节中，矫正1个bit错误并且检测出2个bit错误。页大小对应 ECC结果有效位，见表 26-30。在ECC计算电路使能后，ECC 模块监测 NAND FLASH 的数据总线和读/写信号（NCE 和 NWE）。ECC 使用时需注意如下：

在访问 NAND FLASH 时，出现在 D[15:0]总线上的数据被锁存并用于 ECC 计算。  
当规定数目的字节已经被写入 NAND FLASH 或从 NAND FLASH 读出后，软件从 FSMC_ECCR2 寄存器读出 ECC 值。若需再次计算 ECC，先将 ECCEN 清 0，再写 1 重新使能 ECC 计算。

表 26-30 页大小对应 ECC 结果有效位  

<table><tr><td>ECCPS[2:0]</td><td>页大小(字节)</td><td>ECC有效位</td></tr><tr><td>000</td><td>256</td><td>ECC[21:0]</td></tr><tr><td>001</td><td>512</td><td>ECC[23:0]</td></tr><tr><td>010</td><td>1024</td><td>ECC[25:0]</td></tr><tr><td>011</td><td>2048</td><td>ECC[27:0]</td></tr><tr><td>100</td><td>4096</td><td>ECC[29:0]</td></tr><tr><td>101</td><td>8192</td><td>ECC[31:0]</td></tr></table>

## 26.3 寄存器描述

表 26-31 FSMC 相关寄存器列表  

<table><tr><td>名称</td><td>访问地址</td><td>描述</td><td>复位值</td></tr><tr><td>R32_FSMC_BCR1</td><td>0xA0000000</td><td>SRAM/NOR-Flash 片选控制寄存器1</td><td>0x000030DX</td></tr><tr><td>R32_FSMC_BTR1</td><td>0xA0000004</td><td>SRAM/NOR-Flash 片选时序寄存器1</td><td>0xFFFFFFF</td></tr><tr><td>R32_FSMC_PCR2</td><td>0xA0000060</td><td>NAND-Flash 控制寄存器2</td><td>0x0000018</td></tr><tr><td>R32_FSMC_SR2</td><td>0xA0000064</td><td>FIFO 状态和中断寄存器2</td><td>0x0000040</td></tr><tr><td>R32_FSMC_PRMEM2</td><td>0xA0000068</td><td>通用存储空间时序寄存器2</td><td>0xFCFCFCFC</td></tr><tr><td>R32_FSMC_PATT2</td><td>0xA000006C</td><td>属性存储空间时序寄存器2</td><td>0xFCFCFCFC</td></tr><tr><td>R32_FSMC_ECCR2</td><td>0xA0000074</td><td>ECC 结果寄存器2</td><td>0x0000000</td></tr><tr><td>R32_FSMC_BWTR1</td><td>0xA0000104</td><td>SRAM/NOR-Flash 写时序寄存器1</td><td>0xFFFFFFF</td></tr></table>

### 26.3.1 SRAM/NOR-Flash 片选控制寄存器 1（FSMC_BCR1）

偏移地址： $0 \times 0 0$   

<table><tr><td colspan="12">Reserved</td><td>CBURS TRW</td><td colspan="3">Reserved</td></tr><tr><td>15</td><td>14</td><td>13</td><td>12</td><td>11</td><td>10</td><td>9</td><td>8</td><td>7</td><td>6</td><td>5</td><td>4</td><td>3</td><td>2</td><td>1</td><td>0</td></tr><tr><td>ASYNCWAIT</td><td>EXTM OD</td><td>WAIT EN</td><td>WREN</td><td>WAIT CFG</td><td>WRAP MOD</td><td>WAIT POL</td><td>BURST EN</td><td>Reser ved</td><td>FACC EN</td><td colspan="2">MWID[1:0]</td><td colspan="2">MTYP[1:0]</td><td>MUXEN</td><td>MBKEN</td></tr></table>

<table><tr><td>位</td><td>名称</td><td>访问</td><td>描述</td><td>复位值</td></tr><tr><td>[31:20]</td><td>Reserved</td><td>R0</td><td>保留。</td><td>0</td></tr><tr><td>19</td><td>CBURSTRW</td><td>RW</td><td>批量写使能。1:写操作处于同步模式;0:写操作处于异步模式。</td><td>0</td></tr><tr><td>[18:16]</td><td>Reserved</td><td>RO</td><td>保留。</td><td>0</td></tr><tr><td>15</td><td>ASYNCWAIT</td><td>RW</td><td>异步传输期间的等待信号。1:启用异步传输期间等待信号;0:禁用异步传输期间等待信号。</td><td>0</td></tr><tr><td>14</td><td>EXTMOD</td><td>RW</td><td>扩展模式使能。1:允许写使用FSMC_BWTR寄存器;0:不允许写使用FSMC_BWTR寄存器。</td><td>0</td></tr><tr><td>13</td><td>WAITEN</td><td>RW</td><td>等待使能。当闪存存储器处于批量传输模式时,该位控制是否根据NWAIT信号插入等待信号。1:使用NWAIT信号;0:禁用NWAIT信号。</td><td>1</td></tr><tr><td>12</td><td>WREN</td><td>RW</td><td>写操作使能。1:允许FSMC对存储器进行写操作;0:禁止FSMC对存储器进行写操作。</td><td>1</td></tr><tr><td>11</td><td>WAITCFG</td><td>RW</td><td>等待时序配置。1:NWAIT信号在等待状态期间有效;0:NWAIT信号在等待状态前的一个数据周期有效。</td><td>0</td></tr><tr><td>10</td><td>WRAPMOD</td><td>RW</td><td>批量模式支持非对齐使能。1:支持直接的非对齐批量操作;0:不支持直接的非对齐批量操作。</td><td>0</td></tr><tr><td>9</td><td>WAITPOL</td><td>RW</td><td>等待信号极性。1:NWAIT等待信号为高有效;0:NWAIT等待信号为低有效。</td><td>0</td></tr><tr><td>8</td><td>BURSTEN</td><td>RW</td><td>批量模式使能。1:使能批量模式;0:不使能批量模式。</td><td>0</td></tr><tr><td>7</td><td>Reserved</td><td>RO</td><td>保留。</td><td>1</td></tr><tr><td>6</td><td>FACCEN</td><td>RW</td><td>NOR FLASH访问使能。1:允许对NOR FLASH访问;0:禁止对NOR FLASH访问。</td><td>1</td></tr><tr><td>[5:4]</td><td>MWID[1:0]</td><td>RW</td><td>数据总线宽度。11:保留;10:保留;01:16位;00:8位;</td><td>01b</td></tr><tr><td>[3:2]</td><td>MTYP[1:0]</td><td>RW</td><td>储存器类型。11:保留;10:NOR FLASH;01:PSRAM;00:SRAM、ROM;</td><td>xxb</td></tr><tr><td>1</td><td>MUXEN</td><td>RW</td><td>地址/数据复用使能。</td><td>x</td></tr><tr><td></td><td></td><td></td><td>1: 地址/数据复用;
0: 地址/数据不复用。</td><td></td></tr><tr><td>0</td><td>MBKEN</td><td>RW</td><td>存储器块使能。
1: 启用对应的存储器块;
0: 禁用对应的存储器块。</td><td>x</td></tr></table>

### 26.3.2 SRAM/NOR-Flash 片选时序寄存器 1（FSMC_BTR1）

偏移地址： $0 \times 0 4$

<table><tr><td>31</td><td>30</td><td>29</td><td>28</td><td>27</td><td>26</td><td>25</td><td>24</td><td>23</td><td>22</td><td>21</td><td>20</td><td>19</td><td>18</td><td>17</td><td>16</td></tr><tr><td>Reserved</td><td colspan="3">ACCMOD[1:0]</td><td colspan="4">DATLAT[3:0]</td><td colspan="4">CLKDIV[3:0]</td><td colspan="4">BUSTURN[3:0]</td></tr><tr><td>15</td><td>14</td><td>13</td><td>12</td><td>11</td><td>10</td><td>9</td><td>8</td><td>7</td><td>6</td><td>5</td><td>4</td><td>3</td><td>2</td><td>1</td><td>0</td></tr><tr><td colspan="8">DATAST[7:0]</td><td colspan="4">ADDHLD[3:0]</td><td colspan="4">ADDSET[3:0]</td></tr></table>

<table><tr><td>位</td><td>名称</td><td>访问</td><td>描述</td><td>复位值</td></tr><tr><td>[31:30]</td><td>Reserved</td><td>R0</td><td>保留。</td><td>0</td></tr><tr><td>[29:28]</td><td>ACCMOD[1:0]</td><td>RW</td><td>异步访问模式。11:模式D;10:模式C;01:模式B;00:模式A。</td><td>00b</td></tr><tr><td>[27:24]</td><td>DATLAT[3:0]</td><td>RW</td><td>数据保持时间。用于同步批量模式(仅适用于NOR FLASH)1111:第一个数据保持时间为17个CLK时钟;1110:第一个数据保持时间为16个CLK时钟;......0000:第一个数据保持时间为2个CLK时钟;</td><td>1111b</td></tr><tr><td>[23:20]</td><td>CLKDIV[3:0]</td><td>RW</td><td>时钟分频比(CLK信号)。访问异步NOR FLASH、SRAM和ROM时,该参数不起作用。1111:1个CLK周期=16个HCLK周期;1110:1个CLK周期=15个HCLK周期;......0001:1个CLK周期=2个HCLK周期;0000:保留。</td><td>1111b</td></tr><tr><td>[19:16]</td><td>BUSTURN[3:0]</td><td>RW</td><td>总线恢复时间。(仅适用于NOR FLASH)1111:总线恢复时间=16个HCLK周期;1110:总线恢复时间=15个HCLK周期;......0000:总线恢复时间=1个HCLK周期;</td><td>1111b</td></tr><tr><td>[15:8]</td><td>DATAST[7:0]</td><td>RW</td><td>数据保持时间。1111111:数据保持时间=256个HCLK周期;11111110:数据保持时间=255个HCLK周期;......</td><td>ffh</td></tr><tr><td></td><td></td><td></td><td>00000001:数据保持时间=2个HCLK周期;00000000:保留。</td><td></td></tr><tr><td>[7:4]</td><td>ADDHLD[3:0]</td><td>RW</td><td>地址保持时间。(仅适用于异步操作)1111:地址保持时间=16个HCLK周期;1110:地址保持时间=15个HCLK周期;......0000:地址保持时间=1个HCLK周期;</td><td>1111b</td></tr><tr><td>[3:0]</td><td>ADDSET[3:0]</td><td>RW</td><td>地址建立时间。(仅适用于异步操作)1111:地址建立时间=16个HCLK周期;1110:地址建立时间=15个HCLK周期;......0000:地址建立时间=1个HCLK周期;</td><td>1111b</td></tr></table>

### 26.3.3 NAND-Flash 控制寄存器 2（FSMC_PCR2）

偏移地址： $0 \times 6 0$

<table><tr><td>31</td><td>30</td><td>29</td><td>28</td><td>27</td><td>26</td><td>25</td><td>24</td><td>23</td><td>22</td><td>21</td><td>20</td><td>19</td><td>18</td><td>17</td><td>16</td></tr><tr><td colspan="12">Reserved</td><td colspan="3">ECCPS[2:0]</td><td>TAR[3]</td></tr><tr><td>15</td><td>14</td><td>13</td><td>12</td><td>11</td><td>10</td><td>9</td><td>8</td><td>7</td><td>6</td><td>5</td><td>4</td><td>3</td><td>2</td><td>1</td><td>0</td></tr><tr><td colspan="2">TAR[2:0]</td><td colspan="3">TCLR[3:0]</td><td colspan="2">Reserved</td><td colspan="2">ECCEN</td><td colspan="3">PWID[1:0]</td><td>PTYP</td><td>PBKEN</td><td>PWAI TEN</td><td>Reser ved</td></tr></table>

<table><tr><td>位</td><td>名称</td><td>访问</td><td>描述</td><td>复位值</td></tr><tr><td>[31:20]</td><td>Reserved</td><td>R0</td><td>保留。</td><td>0</td></tr><tr><td>[19:17]</td><td>ECCPS[2:0]</td><td>RW</td><td>ECC页面大小111:保留。101:8192Byte100:4096Byte011:2048Byte100:1024Byte100:512Byte100:256Byte</td><td>000b</td></tr><tr><td>[16:13]</td><td>TAR[3:0]</td><td>RW</td><td>ALE至RE的延迟。1111:16个HCLK周期;1110:15个HCLK周期;......0000:1个HCLK周期。</td><td>0000b</td></tr><tr><td>[12:9]</td><td>TCLR[3:0]</td><td>RW</td><td>CLE至RE的延迟。1111:16个HCLK周期;1110:15个HCLK周期;......0000:1个HCLK周期。</td><td>0000b</td></tr><tr><td>[8:7]</td><td>Reserved</td><td>R0</td><td>保留。</td><td>0</td></tr><tr><td>6</td><td>ECCEN</td><td>RW</td><td>ECC使能。1:使能ECC;</td><td>0</td></tr><tr><td></td><td></td><td></td><td>0:禁用 ECC。</td><td></td></tr><tr><td>[5:4]</td><td>PWID[1:0]</td><td>RW</td><td>数据总线宽度。11:保留;10:保留;01:16位;00:8位。</td><td>01b</td></tr><tr><td>3</td><td>PTYP</td><td>RW</td><td>存储器类型。1:NAND闪存;0:保留。</td><td>1</td></tr><tr><td>2</td><td>PBKEN</td><td>RW</td><td>NAND存储器使能。1:使能对应的存储器块;0:禁用对应的存储器块。</td><td>0</td></tr><tr><td>1</td><td>PWAITEN</td><td>RW</td><td>等待功能使能。1:使能;0:禁用。</td><td>0</td></tr><tr><td>0</td><td>Reserved</td><td>RO</td><td>保留。</td><td>0</td></tr></table>

### 26.3.4 FIFO 状态和中断寄存器 2（FSMC_SR2）

偏移地址： $0 \times 6 4$

<table><tr><td>31</td><td>30</td><td>29</td><td>28</td><td>27</td><td>26</td><td>25</td><td>24</td><td>23</td><td>22</td><td>21</td><td>20</td><td>19</td><td>18</td><td>17</td><td>16</td></tr><tr><td colspan="16">Reserved</td></tr><tr><td>15</td><td>14</td><td>13</td><td>12</td><td>11</td><td>10</td><td>9</td><td>8</td><td>7</td><td>6</td><td>5</td><td>4</td><td>3</td><td>2</td><td>1</td><td>0</td></tr><tr><td colspan="9">Reserved</td><td>FEMPT</td><td>IFEN</td><td>ILEN</td><td>IREN</td><td>IFS</td><td>ILS</td><td>IRS</td></tr></table>

<table><tr><td>位</td><td>名称</td><td>访问</td><td>描述</td><td>复位值</td></tr><tr><td>[31:7]</td><td>Reserved</td><td>R0</td><td>保留。</td><td>0</td></tr><tr><td>6</td><td>FEMPT</td><td>R0</td><td>FIFO0空标志。1: FIFO0空;0: FIFO0不空。</td><td>1</td></tr><tr><td>5</td><td>IFEN</td><td>RW</td><td>中断下降沿检测使能。1:使能;0:禁用。</td><td>0</td></tr><tr><td>4</td><td>ILEN</td><td>RW</td><td>中断高电平检测使能。1:使能;0:禁用。</td><td>0</td></tr><tr><td>3</td><td>IREN</td><td>RW</td><td>上升沿中断检测使能。1:使能;0:禁用。</td><td>0</td></tr><tr><td>2</td><td>IFS</td><td>RW</td><td>中断下降沿状态。1:产生中断下降沿;0:没有产生下降沿。</td><td>0</td></tr><tr><td>1</td><td>ILS</td><td>RW</td><td>中断高电平状态。1:产生中断高电平;0:没有产生中断高电平。</td><td>0</td></tr><tr><td>0</td><td>IRS</td><td>RW</td><td>中断上升沿状态。1:没有产生中断上升沿;0:产生了中断上升沿。</td><td>0</td></tr></table>

### 26.3.5 通用存储空间时序寄存器 2（FSMC_PMEM2）

偏移地址： $0 \times 6 8$

<table><tr><td>31</td><td>30</td><td>29</td><td>28</td><td>27</td><td>26</td><td>25</td><td>24</td><td>23</td><td>22</td><td>21</td><td>20</td><td>19</td><td>18</td><td>17</td><td>16</td></tr><tr><td colspan="8">MEMHIZx</td><td colspan="8">MEMHOLDx</td></tr><tr><td>15</td><td>14</td><td>13</td><td>12</td><td>11</td><td>10</td><td>9</td><td>8</td><td>7</td><td>6</td><td>5</td><td>4</td><td>3</td><td>2</td><td>1</td><td>0</td></tr><tr><td colspan="8">MEMWAITx</td><td colspan="8">MEMSETx</td></tr></table>

<table><tr><td>位</td><td>名称</td><td>访问</td><td>描述</td><td>复位值</td></tr><tr><td>[31:24]</td><td>MEMHIZx</td><td>RW</td><td>通用空间数据总线的高阻时间。1111111: NAND FLASH为255个HCLK周期;11111110: NAND FLASH为254个HCLK周期;......</td><td>fch</td></tr><tr><td></td><td></td><td></td><td>00000000: NAND FLASH 为 0 个 HCLK 周期。</td><td></td></tr><tr><td>[23:16]</td><td>MEMHOLDx</td><td>RW</td><td>通用空间保持时间。11111111: 255 个 HCLK 周期;11111110: 254 个 HCLK 周期;......00000001: 1 个 HCLK 周期;00000000: 保留。</td><td>fch</td></tr><tr><td>[15:8]</td><td>MEMWAITx</td><td>RW</td><td>通用空间等待时间。(需要加上由 NWAIT 信号变低引入的等待周期)11111111: 256 个 HCLK 周期;11111110: 255 个 HCLK 周期;......00000001: 2 个 HCLK 周期;00000000: 保留。</td><td>fch</td></tr><tr><td>[7:0]</td><td>MEMSETx</td><td>RW</td><td>通用空间建立时间。11111111: NAND FLASH 257 个 HCLK 周期;11111110: NAND FLASH 256 个 HCLK 周期;......00000000: NAND FLASH 2 个 HCLK 周期。</td><td>fch</td></tr></table>

### 26.3.6 属性存储空间时序寄存器 2（FSMC_PATT2）

偏移地址： $_ { 0 \times 6 0 }$

<table><tr><td>31</td><td>30</td><td>29</td><td>28</td><td>27</td><td>26</td><td>25</td><td>24</td><td>23</td><td>22</td><td>21</td><td>20</td><td>19</td><td>18</td><td>17</td><td>16</td></tr><tr><td colspan="8">ATTHIZx</td><td colspan="8">ATTHOLDx</td></tr><tr><td>15</td><td>14</td><td>13</td><td>12</td><td>11</td><td>10</td><td>9</td><td>8</td><td>7</td><td>6</td><td>5</td><td>4</td><td>3</td><td>2</td><td>1</td><td>0</td></tr><tr><td colspan="8">ATTWAITx</td><td colspan="8">ATTSETx</td></tr></table>

<table><tr><td>位</td><td>名称</td><td>访问</td><td>描述</td><td>复位值</td></tr><tr><td>[31:24]</td><td>ATTHIZx</td><td>RW</td><td>属性空间数据总线的高阻时间。11111111:255个HCLK周期;11111110:254个HCLK周期;......00000000:0个HCLK周期。</td><td>fch</td></tr><tr><td>[23:16]</td><td>ATTHOLDx</td><td>RW</td><td>属性空间保持时间。11111111:255个HCLK周期;11111110:254个HCLK周期;......00000001:1个HCLK周期;00000000:保留。</td><td>fch</td></tr><tr><td>[15:8]</td><td>ATTWAITx</td><td>RW</td><td>属性空间等待时间。(需要加上由NWAIT信号变低引入的等待周期)11111111:256个HCLK周期;11111110:255个HCLK周期;......</td><td>fch</td></tr><tr><td></td><td></td><td></td><td>00000001:2个HCLK周期;00000000:1个HCLK周期。</td><td></td></tr><tr><td>[7:0]</td><td>ATTSETx</td><td>RW</td><td>属性空间建立时间。1111111:256个HCLK周期;11111110:255个HCLK周期;......00000000:1个HCLK周期。</td><td>fch</td></tr></table>

### 26.3.7 ECC 结果寄存器 2（FSMC_ ECCR2）

偏移地址： $0 \times 7 4$

<table><tr><td>31</td><td>30</td><td>29</td><td>28</td><td>27</td><td>26</td><td>25</td><td>24</td><td>23</td><td>22</td><td>21</td><td>20</td><td>19</td><td>18</td><td>17</td><td>16</td></tr><tr><td colspan="16">ECC[31:16]</td></tr><tr><td>15</td><td>14</td><td>13</td><td>12</td><td>11</td><td>10</td><td>9</td><td>8</td><td>7</td><td>6</td><td>5</td><td>4</td><td>3</td><td>2</td><td>1</td><td>0</td></tr><tr><td colspan="16">ECC[15:0]</td></tr></table>

<table><tr><td>位</td><td>名称</td><td>访问</td><td>描述</td><td>复位值</td></tr><tr><td>[31:0]</td><td>ECC[31:0]</td><td>R0</td><td>ECC 计算结果</td><td>0</td></tr></table>

### 26.3.8 SRAM/NOR-Flash 写时序寄存器 1（FSMC_BWTR1）

偏移地址： $0 \times 1 0 4$

<table><tr><td>31</td><td>30</td><td>29</td><td>28</td><td>27</td><td>26</td><td>25</td><td>24</td><td>23</td><td>22</td><td>21</td><td>20</td><td>19</td><td>18</td><td>17</td><td>16</td></tr><tr><td>Reserved</td><td colspan="2">ACCMOD
[1:0]</td><td colspan="5">DATLAT[3:0]</td><td colspan="4">CLKDIV[3:0]</td><td colspan="4">BUSTYRN[3:0]</td></tr><tr><td>15</td><td>14</td><td>13</td><td>12</td><td>11</td><td>10</td><td>9</td><td>8</td><td>7</td><td>6</td><td>5</td><td>4</td><td>3</td><td>2</td><td>1</td><td>0</td></tr><tr><td colspan="8">DATAST[7:0]</td><td colspan="4">ADDHLD[3:0]</td><td colspan="4">ADDSET[3:0]</td></tr></table>

<table><tr><td>位</td><td>名称</td><td>访问</td><td>描述</td><td>复位值</td></tr><tr><td>[31:30]</td><td>Reserved</td><td>R0</td><td>保留。</td><td>0</td></tr><tr><td>[29:28]</td><td>ACCMOD[1:0]</td><td>RW</td><td>访问模式。(该位在EXTMOD为1时有效)00:访问模式A;01:访问模式B;10:访问模式C;11:访问模式D。</td><td>00b</td></tr><tr><td>[27:24]</td><td>DATLAT[3:0]</td><td>RW</td><td>数据保持时间。(NOR FLASH同步批量模式)CLK为闪存时钟。1111:17个CLK周期;1110:16个CLK周期;......0000:1个HCLK周期。</td><td>1111b</td></tr><tr><td>[23:20]</td><td>CLKDIV[3:0]</td><td>RW</td><td>时钟分频比(CLK)。1111:1个CLK周期=16个HCLK周期;1110:1个CLK周期=15个HCLK周期;......</td><td>1111b</td></tr><tr><td></td><td></td><td></td><td>0001:1个CLK周期=2个HCLK周期;0000:保留。</td><td></td></tr><tr><td>[19:16]</td><td>BUSTYRN[3:0]</td><td>RW</td><td>总线恢复时间。1111:16个HCLK周期;1110:15个HCLK周期;......0001:2个HCLK周期;0000:1个HCLK周期。</td><td>1111b</td></tr><tr><td>[15:8]</td><td>DATAST[7:0]</td><td>RW</td><td>数据保持时间。1111111:256个HCLK周期;11111110:255个HCLK周期;......00000001:2个HCLK周期;00000000:保留。</td><td>ffh</td></tr><tr><td>[7:4]</td><td>ADDHLD[3:0]</td><td>RW</td><td>地址保持时间。1111:16个HCLK周期;1110:15个HCLK周期;......0001:2个HCLK周期;0000:保留。</td><td>1111b</td></tr><tr><td>[3:0]</td><td>ADDSET[3:0]</td><td>RW</td><td>地址建立时间。1111:16个HCLK周期;1110:15个HCLK周期;......0001:2个HCLK周期;0000:1个HCLK周期。</td><td>1111b</td></tr></table>

# 第 27 章 以太网收发器（ETH）

本章模块描述适用于 $\pmb { C H 3 2 V 3 x }$ 和CH32F20x微控制器系列部分产品。

本章提到的“以太网收发器”是一个专有名词，意为微控制器内部的以太网（Ethernet）数据链路层收发器，通讯速率最高为千兆位每秒（1Gbps），是一个通讯外设。本章提到的“MAC”指的是以太网收发器在数据链路层的角色名称，是以太网收发器的组成部分。

以太网收发器（Ethernet Transceiver MAC）是微控制器一个重要的高速通讯部件，它允许微控制器以千兆位（Gigabit）的连接速度接入以太网，实现极快的数据沟通。

## 27.1 CH32F20x_D8C、CH32V30x_D8C 系列

### 27.1.1 主要特征

微控制器的以太网收发器是微控制器的一个重要高速通讯外设，它集成千兆MAC（媒体访问控制器）、32位宽的DMA控制器、管理计数器（MMC）、精确时间协议控制器（PTP）和一个十兆速度的以太网物理层（10BASE-T PHY）。在外接千兆以太网物理层（PHY）后，它能以千兆位（1Gbps）的速度接入以太网进行数据收发。以太网收发器工作在数据链路层，需要通过软件实现 TCP/IP 协议栈和接口。以太网收发器支持标准MII、RMII和 RGMII三种 MII接口连接 PHY，如果用户要达到千兆的接入速度，必须使用 RGMII 接口；以太网收发器通过 SMI 接口控制 PHY，接口的时序由 MAC 自动实现，无需用户通过软件生成。RGMII接口支持发送时钟相位翻转和相对数据延迟，最大延迟 4 纳秒。以太网收发器的 MAC 支持标准 IEEE802.3 协议的以太网，支持魔法帧和特定唤醒帧。以太网收发器搭配的DMA 控制器以描述符的形式进行数据的收发管理和内存搬运，描述符的数量由用户根据沟通密集程度自行确定，DMA 控制器能以 32 位宽的速度向描述符指定的内存空间写入接收到的数据或从取出将发送的数据。此外，以太网收发器的MAC还支持 IEEE1588精确时间协议。

#### 27.1.1.1 MAC 特征

MAC 挂载在 AHB 总线上  
$\bullet$ 支持 10M/100M/1000M 以太网  
支持 MII/RMII/RGMII 接口  
RGMII支持发送时钟延迟和翻转  
$\bullet$ 支持全双工与半双工  
支持自动插入帧头序列和 SFD  
支持自动插入和校验CRC  
支持自动计算和校验IP/ICMP/TCP/UDP协议数据包的检验和  
支持发送帧长控制  
支持发送间隙调节  
支持 VLAN 帧  
支持帧接收地址的完美地址过滤、HASH 过滤  
支持帧发送地址过滤  
支持多播广播帧接收控制  
支持混杂模式  
$\bullet$ 支持 SMI 管理接口（RGMII 与 MII 各一套）  
$\bullet$ 支持魔法帧和自定义的唤醒帧唤醒微控制器  
$\bullet$ 支持专门的以太网唤醒中断入口  
支持链路层的数据回环

#### 27.1.1.2 DMA 特征

32 位宽的 MAC 专享 DMA

最大程度减小CPU的操作  
$\bullet$ DMA 挂载在 AHB 总线上，也同时做 AHB 总线的主设备直接访问 RAM  
$\bullet$ 支持以字节对齐方式访问RAM  
$\bullet$ 以描述符的方式管理收发缓冲区  
$\bullet$ 接收发送的一部分状态反馈在描述符中  
$\bullet$ 可以动态修改非正在使用的描述符和缓冲区  
$\bullet$ 支持手动停止或启动  
$\bullet$ 支持链式或环式的形式连接描述符  
支持收发完成中断等多种中断源

#### 27.1.1.3 MMC 模块特征

支持手动复位、停止或冻结  
$\bullet$ 多个支持发送和接收的多种计数模式的计数器  
支持多种中断

#### 27.1.1.4 PTP 模块特征

支持 IEEE 1588 协议  
$\bullet$ 支持自动在收发时保存当前的时刻  
$\bullet$ 自带一个 32 位的秒计时器和一个 32 位有符号的亚秒计时器  
支持以精调和粗调两种方式调节时间  
支持一个中断源  
支持 PPS 输出

#### 27.1.1.5 内部 10BASE-T 物理层特征

支持 10BASE-T  
$\bullet$ 通过 SMI 接口管理  
支持 MDIX 自动翻转  
支持 MDI接口的差分对 ${ \mathsf { p } } / { \mathsf { n } }$ 极性翻转  
$\bullet$ 支持手动复位  
$\bullet$ 支持物理层的数据回环  
支持半双工

### 27.1.2 概述

以太网收发器工作在 OSI 七层模型中的数据链路层和物理层。如果需要实现大于 10Mbps 的数据传输速率，需要外接百兆或千兆物理层芯片（PHY）实现物理连接。为了能在广泛使用的以太网中建立 IP、TCP 和 UDP 等协议的通讯，用户还需要用软件实现 TCP/IP 协议栈。以太网收发器由媒体访问控制层（MAC）、搭配的DMA及二者的控制寄存器及十兆物理层组成。

![](images/c92f5817876a258b21cd4175acea5107247ecaf7908f765aa3cd27cad0d64f24.jpg)  
图 27-1 以太网收发器在 OSI 模型和 TCP/IP 模型中的位置

以太网收发器的 MAC 按照 IEEE802.3 协议的规范设计，搭配 32 位宽的 DMA，保证数据能快速地从网线上转发到微控制器的内存中。以太网收发器拥有强大完整的 DMA 控制寄存器、MAC 控制寄存器和模式控制寄存器，微控制器的 CPU 通过 AHB 总线操作以太网收发器的寄存器。以太网收发器通过RGMII 接口和千兆以太网物理层连接，如果只需要百兆以太网的速度，则可以通过 MII 或 RMII 接口和百兆以太网物理层连接。以太网收发器的 MII、RMII 和 RGMII 接口的管脚是复用的，具体参看 27.3节。以太网收发器通过SMI接口管理以太网物理层。使用以太网收发器时，AHB总线的时钟不能低于50MHz。

![](images/0db63181ec9d8e4195e22d9000f638363f6b92586ef1fc878782aa9c111b6edc.jpg)  
图27-2 以太网收发器的结构框图

此外，以太网收发器还支持IEEE1588 精确时间协议（PTP），为微控制器系统或片外提供精确的时间数据。

### 27.1.3 以太网收发器使用引脚的分布和配置

微控制器支持三种MII接口，即标准MII、RMII和 RGMII。MII/RMII接口专用于十兆和百兆以太网物理层，RGMII接口可用于十兆、百兆和千兆以太网。下表显示了标准MII，RMII和 RGMII接口及内部物理层在封装引脚上的分布。

表 27-1 微控制器的媒体独立接口、内置物理层的介质相关接口  
及其他相关的引脚分布和需要进行的配置  

<table><tr><td>引脚</td><td>RGMII</td><td>RGMII的引脚配置</td><td>MII</td><td>RMII</td><td>MII/RMII的引脚配置</td></tr><tr><td>PA2</td><td>TXCLK</td><td></td><td colspan="2">MDIO</td><td>推挽复用输出</td></tr><tr><td>PC1</td><td>RXCTL</td><td>浮空输入</td><td colspan="2">MDC</td><td>推挽复用输出</td></tr><tr><td>PA1</td><td>RXD3</td><td>浮空输入</td><td>RX_CLK</td><td>REF_CLK</td><td>浮空输入</td></tr><tr><td>PA7</td><td>TXD0</td><td>推挽复用输出</td><td>RX_DV</td><td>CRS_DV</td><td>浮空输入</td></tr><tr><td>PC4</td><td>TXD1</td><td>推挽复用输出</td><td>RXD0</td><td>RXD0</td><td>浮空输入</td></tr><tr><td>PC5</td><td>TXD2</td><td>推挽复用输出</td><td>RXD1</td><td>RXD1</td><td>浮空输入</td></tr><tr><td>PB0</td><td>TXD3</td><td>推挽复用输出</td><td>RXD2</td><td></td><td>浮空输入</td></tr><tr><td>PB1</td><td>125MHz_IN</td><td>浮空输入</td><td>RXD3</td><td></td><td>浮空输入</td></tr><tr><td>PB10</td><td></td><td></td><td>RXER</td><td></td><td>浮空输入</td></tr><tr><td>PC3</td><td>RXD1</td><td>浮空输入</td><td>TX_CLK</td><td></td><td>浮空输入</td></tr><tr><td>PB11</td><td></td><td></td><td>TX_EN</td><td>TX_EN</td><td>推挽复用输出</td></tr><tr><td>PB12</td><td>MDC</td><td>推挽复用输出</td><td>TXD0</td><td>TXD0</td><td>推挽复用输出</td></tr><tr><td>PB13</td><td>MDIO</td><td>推挽复用输出</td><td>TXD1</td><td>TXD1</td><td>推挽复用输出</td></tr><tr><td>PC2</td><td>RXD0</td><td>浮空输入</td><td>TXD2</td><td></td><td>推挽复用输出</td></tr><tr><td>PB8</td><td></td><td></td><td>TXD3</td><td></td><td>推挽复用输出</td></tr><tr><td>PA0</td><td>RXD2</td><td>浮空输入</td><td>CRS</td><td></td><td>浮空输入</td></tr><tr><td>PA3</td><td>TXCTL</td><td>推挽复用输出</td><td>COL</td><td></td><td>浮空输入</td></tr><tr><td>PB5</td><td colspan="5">PPS_OUT(推挽复用输出)</td></tr><tr><td>PC6</td><td colspan="5">10BASE-T_TX_p(无需IO配置)</td></tr><tr><td>PC7</td><td colspan="5">10BASE-T_TX_n(无需IO配置)</td></tr><tr><td>PC8</td><td colspan="5">10BASE-T_RX_p(无需IO配置)</td></tr><tr><td>PC9</td><td colspan="5">10BASE-T_RX_n(无需IO配置)</td></tr></table>

### 27.1.4 物理层（PHY）管理和数据交互

以太网收发器的 MAC 通过站点管理接口（SMI 接口）对 PHY 进行管理，使用媒体独立接口（MII接口）与 PHY 进行数据交互。微控制器支持的 MII 接口包括标准 MII（一般就写为 MII）、精简 MII（RMII）和精简的千兆MII（RGMII）。微控制器在MII/RMII模式下和 RGMII模式下使用不同管脚引出SMI接口。

#### 27.1.4.1 SMI 接口

SMI 接口是一种串行通讯接口，使用MDC（时钟线）和MDIO（数据线）两线来访问 PHY的寄存器实现对PHY的管理，最多可以管理32个PHY芯片。其中 MDC是时钟线，空闲时保持低，MDIO 是数据线。SMI的读写操作和帧的组成都是由MAC主导的，用户只需要写入地址和数据。相关的寄存器分别为 MII 地址寄存器（R32_ETH_MACMIIAR）和 MII 数据寄存器（R32_ETH_MACMIIDR）。

#### 27.1.4.1.1 帧格式

SMI 接口管理帧的格式如下表 27-2 所示。

表 27-2 SMI 帧格式  

<table><tr><td></td><td>Preamble</td><td>STR</td><td>OP</td><td>PHY ADR</td><td>REG ADR</td><td>T</td><td>DATA</td><td>P</td></tr><tr><td>读</td><td>32个“1”</td><td>01</td><td>10</td><td>PPPPP</td><td>RRRRR</td><td>Z0</td><td>DCCCCCCCCCCCCCCCC</td><td>Z</td></tr><tr><td>写</td><td>32个“1”</td><td>01</td><td>01</td><td>PPPPP</td><td>RRRRR</td><td>10</td><td>DCCCCCCCCCCCCCCCC</td><td>Z</td></tr></table>

管理帧各域的定义如下：  
1） Preamble：先导符，由 32 个“1”组成，用于 MAC 和 PHY 同步；  
2） STR：起始符，固定为“01”；

3） OP：操作符，读为“10”，写为“01”；  
4） PHY ADR：物理层地址，5 位；  
5） REG ADR：寄存器地址，5 位；  
6） T：转换符，两位，用来切换MAC和PHY对 MDIO 线的控制权。在读操作时，MAC保持对 MDIO 线的高阻，PHY 对第一位保持高阻，对第二位下拉，并获取 MDIO 的控制权；在写操作时，MAC 对 MDIO线先置高再下拉，PHY对MDIO保持高阻状态，MAC保持对 MDIO的控制权；  
7） DATA：数据域，16 位，MAC 对 PHY 读写的数据，MSB 在前；  
8） P：MAC 和 PHY 都对 MDIO 保持高阻状态，但 PHY 的上拉电阻会把 MDIO 拉高。

#### 27.1.4.1.2 读写时序

写PHY寄存器操作如下：

当用户设置了 MII 写位（ETH_MACMIIAR:MW）和忙位(ETH_MACMIIAR:MB)，SMI 接口会向 PHY 发送PHY地址和寄存器地址，然后发送数据(ETH_MACMIIDR)。在 SMI接口发送数据的过程中，不能修改MII地址寄存器和 MII 数据寄存器的值；在此过程中,忙位保持为高，对 MII 地址寄存器或 MII 数据寄存器的写操作将被忽视，并且不对整个传输造成影响。当完成写操作时，SMI接口将清除忙位，用户可以根据忙位判断写操作结束。如图27-3的写部分。

读PHY寄存器操作如下：

当用户把以太网 MAC 的 MII 地址寄存器(ETH_MACMIIAR)的 MII 忙位置位，而保持 MII 写位复位时，SMI接口则发送PHY地址和寄存器地址，执行读 PHY寄存器的操作。在整个传输过程中，用户不能修改 MII 地址寄存器和 MII 数据寄存器的内容。在传输过程中，忙位保持为高，对 MII 地址寄存器或MII数据寄存器的写操作将被忽视，并且不影响整个传输的正确完成。在读操作完成后，SMI接口将清除忙位，并把从 PHY 读回的数据写入 MII 数据寄存器。如图 27-3 的读部分。

![](images/9e2643bf1d952f0d0e2acd24efd0c667d0d0607c2acfe8c4e04d71a7bc8da032.jpg)  
图 27-3 SMI 接口读写时速图

#### 27.1.4.1.3 SMI 时钟

一般来说 SMI 时钟需要保持在一个固定的范围，以保证实际上 SMI 的时钟需要被 PHY 接收。具体参考用户选用的 PHY芯片的手册。

SMI 时钟频率通过 MII 地址寄存器（ETH_MACMIIAR）的 CR 域从 AHB 时钟分频得到。下表显示和SMI时钟和AHB时钟的分频关系。默认选择42分频。

表 27-3 SMI 时钟和 AHB 时钟的分频关系  

<table><tr><td></td><td>适合搭配的 AHB 时钟的范围</td><td>SMI 时钟</td></tr><tr><td>0000b</td><td>60MHz 以上</td><td>AHB 时钟/42</td></tr><tr><td>0010b</td><td>20-35MHz</td><td>AHB 时钟/16</td></tr><tr><td>0011b</td><td>35-60MHz</td><td>AHB 时钟/26</td></tr></table>

<table><tr><td>其它值</td><td>无意义</td></tr></table>

#### 27.1.4.2 MII/RMII 接口

#### 27.1.4.2.1 概述

媒体独立接口（MII）是MAC和PHY进行数据沟通的接口，根据速度和引线数量区分，有标准MII、RMII、GMII、RGMII和SGMII等。媒体独立接口一般是有接收、发送各一组，每组由时钟线、若干数据线和辅助线组成。支持的MII接口为：标准 MII/RMII支持十兆和百兆物理层，RGMII 支持十兆、百兆和千兆物理层。由于市场上常用的物理层一般是 10M/100M 自动协商物理层、10M/100M/1000M 自动协商物理层，所以用户在使用 10M/100M 自动协商物理层时应该使用标准 MII/RMII 接口，在使用10M/100M/1000M 物理层时应该使用 RGMII 接口。

一般情况下，媒体独立接口发送方向（TX 开头）的时钟和数据是由 MAC发出的，接收方向（RX开头）的时钟和数据是由 PHY 发出的,但是 RMII 的时钟是公用的。同方向的引脚走线应注意等长布线，具体布线要点参看用户选用的物理层芯片的 Layout手册或本手册 27.1.4.2.2章节。

#### 27.1.4.2.2 引脚

MII 接口的引脚和功能如下：

表27-4 MII 接口的引脚和功能  

<table><tr><td>引脚</td><td>功能</td></tr><tr><td>TXC</td><td>发送时钟(2.5MHz或25MHz)</td></tr><tr><td>TXEN</td><td>发送数据使能</td></tr><tr><td>TXD0</td><td rowspan="4">发送数据线[0:3]</td></tr><tr><td>TXD1</td></tr><tr><td>TXD2</td></tr><tr><td>TXD3</td></tr><tr><td>RXC</td><td>接收时钟(2.5MHz或25MHz)</td></tr><tr><td>RXDV</td><td>指示接收数据有效</td></tr><tr><td>RXER</td><td>接收数据错误</td></tr><tr><td>RXD0</td><td rowspan="4">接收数据线[0:3]</td></tr><tr><td>RXD1</td></tr><tr><td>RXD2</td></tr><tr><td>RXD3</td></tr><tr><td>COL</td><td>冲突检测</td></tr><tr><td>CRS</td><td>载波检测</td></tr></table>

图 27-4 显示了使用 MII 接口的定义时，MAC 和 PHY 如何连接。

![](images/c8769c9661344b5a159bee999005bce151f15e9d9806523d9dee1d162d13d0c8.jpg)  
图 27-4 以太网收发器在使用 MII 规范时，MAC 和 PHY 如何接线

RMII接口的引脚和功能如下：

表 27-5 RMII 接口的引脚和功能  

<table><tr><td>引脚</td><td>功能</td></tr><tr><td>CLK_REF</td><td>收发时钟(50MHz)</td></tr><tr><td>TXEN</td><td>发送数据使能</td></tr><tr><td>TXDO</td><td rowspan="2">发送数据线[0:1]</td></tr><tr><td>TXD1</td></tr><tr><td>CRSDV</td><td>接收数据有效</td></tr><tr><td>RXD0</td><td rowspan="2">接收数据线[0:1]</td></tr><tr><td>RXD1</td></tr></table>

使用 RMII 接口时，收发时钟共用同一根线传输，时钟频率固定为 50MHz。当 RMII 使用在 10Mbps的速率时，RX 和 TX 每隔 10 个周期采样一个数据，数据线需要将数据保留 10 个周期；MAC 和 PHY 需要使用同一个时钟源。可以将 MAC 的 MCO 输出接到 PHY 的外部时钟源输入引脚上。图 27-5 显示了使用RMII接口时，MAC和PHY如何接线。

图 27-5 使用 RMII 接口时，MAC 和 PHY 的接线示意图  
![](images/f56e0f3169289883f05db550d7acc6e25b5d10f399c610ea082a0f7c7df0f3e9.jpg)  
注：以太网收发器需要从RXC引脚输入外界提供的REF_CLK 50MHz时钟频率。

#### 27.1.4.2.3 时序

RMII 和 MII 的时序的区别在于：

$\bullet$ RMII 的时钟固定为 $5 0 M H z$ ，而 MII 的收发时钟随着实际连接速度的不同而工作在 2.5MHz 或 25MHz；  
RMII 收发共用同一个时钟线；

RMII 是两位数据线，而 MII 是四位数据线。

如图 27-6 显示了 RMII 和 MII 的时序。

![](images/1483d6434c610af9b1ce4a9c791fdd3f1ff47181ce1a48e78f357efd94260f3c.jpg)  
图 27-6 MII 和 RMII 的时序图

#### 27.1.4.3 RGMII 接口

#### 27.1.4.3.1 引脚

RGMII 的引脚和功能如下表

表 27-6 RGMII 的引脚和功能  

<table><tr><td>引脚</td><td>功能</td></tr><tr><td>TXC</td><td>发送时钟</td></tr><tr><td>TXCTL</td><td>发送数据控制</td></tr><tr><td>TXD0</td><td rowspan="4">发送数据线[0:3]</td></tr><tr><td>TXD1</td></tr><tr><td>TXD2</td></tr><tr><td>TXD3</td></tr><tr><td>RXC</td><td>接收时钟</td></tr><tr><td>RXCTL</td><td>接收数据控制</td></tr><tr><td>RXD0</td><td rowspan="4">接收数据线[0:3]</td></tr><tr><td>RXD1</td></tr><tr><td>RXD2</td></tr><tr><td>RXD3</td></tr></table>

注：1.TXCTL/RXCTL在本微控制器的以太网收发器中用作收发数据有效信号来使用；  
有独立的 SMI 接口 。

#### 27.1.4.3.2 时序

RGMII工作在十兆和百兆速率的模式下，时序和MII类似；工作在千兆时，RGMII的时钟为125MHz，采用双边沿采样的方式。RGMII 不支持半双工。

由于 RGMII 使用双边沿采样且工作在 125MHz 的时钟频率下，因此电路设计人员应注意信号完整性问题。

RGMII的接收方接收到的时钟应相对数据滞后 $9 0 ^ { \circ }$ 以保证正确采样。以太网收发器设计了发送时钟延迟输出和相位翻转的功能。

![](images/848d61dfa81007f4bb54c0bb292c166925899e6c1dd54b6375d0ee7baa1533ed.jpg)  
图 27-7 RGMII 的时序示意图

#### 27.1.4.4 内部 10M 物理层使用时的注意事项

在置位扩展寄存器的内部物理层使能位时，所有的 MII相关的配置都将被无视，对 SMI接口的读写将直接映射到内部物理层，且忽略SMI接口写入的物理层地址。用户可以对内部物理层的寄存器进行访问以获取物理层连接状态，详见27.1.8.5章节。

#### 27.1.4.5 时钟产生和输出

#### 27.1.4.5.1 外设时钟源配置

![](images/1a84594ec30bb4174fc5854f1e5392f145c8afc5f708f4a95ce029f4e3cb0748.jpg)  
图 27-8 以太网外设时钟树

由上图可以看出，内部 10M 以太网物理层的时钟由 PLL3 提供，且必须为 60MHz。使用内部物理层时，需要把扩展寄存器的第2位置位，置位后，MII/RMII/RGMII 相关的设置均无效。

在使用MII时，收发时钟都由外部的物理层提供，为 2.5MHz或 25MHz。在使用 RMII 时，REF_CLK是唯一的时钟，从RXC引脚输入微控制器，固定为50MHz，使用RMII时需要将 AFIO 重映射寄存器中的第23位置位。

在使用 RGMII 时，MAC 需要频率为 125MHz 的时钟，由内部 PLL2/PLL3 的 VCO 产生(VCO 输出是 PLL输出频率的两倍)或外部输入，通过 EXT_125M 引脚输入 125MHz 时钟。RGMII 的发送时钟由 MAC 产生，

接收时钟由 PHY产生。开启RGMII需要在扩展寄存器中将第 3位置位，并在 RCC的第二个配置寄存器中将第22位置位，同时在[21:20]中选择合适的 125MHz 时钟源，详见官网 EVT例程。

#### 27.1.4.5.2 MCO 输出

已知 MCO 支持 HSE、HSI、系统主频、PLLCLK/2、PLL2CLK、PLL3CLK、PLL3CLK/2 和 XTI 这八种时钟的输出，用户可以利用 MCO 输出应用中所需要的频率，比如常见物理层芯片所需要的 25MHz 时钟，并以此节省一颗晶体。MCO 的输出频率不应大于 100MHz。

### 27.1.5 IEEE802.3 和 IEEE1588

IEEE802.3协议及其补充协议组成目前以太网的官方标准，它详细定义了目前使用的以太网的方方面面。我们可以认为以太网处于OSI模型中的物理层和数据链路层的一部分。本节在应用的角度上讨论IEEE802.3协议中用户需要用到的部分，即帧格式和 MAC地址相关的内容。同时，微控制器的以太网收发器还支持IEEE1588精确时间协议，本文还将讨论 IEEE1588的部分内容。

IEEE802.3协议构建的以太网模型中，数据传输单元是以太网帧。以太网帧在不同的介质上以不同的速率传输时有不同的编码形式，接收后经过物理层解码，通过 MII 接口发给 MAC，MAC 校验和过滤通过后，由TCP/IP协议栈提取出应用信息发给不同的应用进程。

#### 27.1.5.1 帧格式

以太网帧的帧格式如表 27-7 所示。

表 27-7 普通以太网帧格式  

<table><tr><td>前同步码</td><td>SFD</td><td>目标地址</td><td>源地址</td><td>长度或类型</td><td>数据域</td><td>CRC</td></tr><tr><td>7 Bytes</td><td>1Byte</td><td>6 Bytes</td><td>6 Bytes</td><td>2 Bytes</td><td>46-1500 Bytes</td><td>4 Bytes</td></tr><tr><td colspan="7">总长度：64至1518字节加上8字节的物理层首部（前同步码和SFD）</td></tr></table>

前同步码：56 位（7 字节）交替的低电平和高电平跳跃，值固定，用十六进制表示即为 $0 { \times } \tt A A { - } 0 { \times } \tt A \tt A { - }$ $0 \times A A - 0 \times A A - 0 \times A A - 0 \times A A - 0 \times A A = 1 1 A$ 。此域用来进行时钟同步。该域由硬件自动添加/去除，用户不需要理会。

SFD：帧首定界符，8 位（1 字节），值为 10101011b，SFD 用来提醒接收方这是最后一次进行时钟同步的机会，其后为目标地址。该字节由硬件自动添加/去除，用户不需要理会。

目标地址：发送这个帧的设备希望接收这个帧的设备地址，这里指的是硬件地址，又称 MAC地址，由 IEEE 为生产商分配，全球唯一，6 字节 48 位。本公司（WCH）的微控制器的 MAC 地址出厂时已烧录在芯片内部。地址的发送遵循低有效位在前的原则。

源地址：发送这个帧的设备的硬件地址。源地址必须为单播地址。

长度或类型：2字节，以太网一般用作类型，IEEE802.3标准中也用作长度，以1536（即 $0 \times 0 6 0 0 ^ { \cdot }$ ）分界，1536 以上表示协议，表示数据部分是根据何种上层协议组织起来的，例如 $0 \times 0 8 0 6$ 表示 ARP，$0 \times 0 8 0 0$ 表示 ${ \mathsf { I P v 4 } }$ ， $0 \times 8 6 { \mathsf { d d } }$ 表示IPv6；1536以下表示数据长度。

数据域：最小 46 字节，最大 1500 字节。当数据域不足 46 字节需要加填充增至 46 字节，超过1500 字节请另组一帧。数据域中装载着需要实际发送的数据。

CRC：循环冗余校验，此处用的是 CRC32 校验。

#### 27.1.5.2 帧发送

以太网收发器发送数据帧的时候，其专有的 DMA控制器从发送描述符（trans descript 在14.1.1节）指定的RAM中取出要发送的数据通过专用数据总线压进 MAC。FIFO的填充程度会返回到 DMA控制器中，当要发送的数据全部发送完，DMA 控制器会向 MAC 发送一个数据开始信号，MAC 会启动发送；从FIFO中读取数据，向MII接口（RMII/RGMII）发送先导、SFD，发送实际数据；在 DMA控制器向 MAC发送数据结束信号后，MAC加上自动计算的CRC。

MAC 会按照配置自动设备发送帧的类型，并为其中的检验和域计算校验和，并取代原来校验和域的值。此功能可以关闭。

MAC会自动向填充后面加上CRC32校验，也可以设置成不加上 CRC校验位。按照 IEEE802.3协议的规定，数据域长度不低于46字节，当MAC检测到将要发送的数据域短于 46字节时，会自动在数字域后加上填充，如果选择自动填充，则必然会加上 CRC32 校验，无视寄存器配置。MAC 采用的 CRC32检验公式如下：

$$
\operatorname {G} (x) = x ^ {3 2} + x ^ {2 6} + x ^ {2 3} + x ^ {2 2} + x ^ {1 6} + x ^ {1 2} + x ^ {1 1} + x ^ {1 0} + x ^ {8} + x ^ {7} + x ^ {5} + x ^ {4} + x ^ {2} + x + 1
$$

在使能了 IEEE 1588(PTP)功能的情况下，MAC 在发送以太网帧时，将保存当前时间戳在描述符中，但同时会覆盖掉部分信息，用户可以在接收中断中及时读取时间戳并补全描述符。

#### 27.1.5.3 帧接收

以太网收发器在接收以太网帧的时候，以太网帧从MII、RMII 或RGMII 进入到 MAC，在 MAC中首先进入到FIFO，然后被DMA转发到RAM中的缓冲区。MAC 会对以太网帧进行过滤和检查，过滤包括完美地址过滤和HASH过滤，检查则一般是对帧进行帧长度，IP/ICMP/TCP/UDP的检验和 CRC检查，未通过过滤或检查的帧会被描述符标记出来或被丢弃，超过长度的包可能会被掐断。

在使能了IEEE 1588(PTP)功能的情况下，接收帧时，MAC会保存当前时间戳在描述符中，但同时会覆盖掉部分信息，用户可以在接收中断中及时读取时间戳并补全描述符。

#### 27.1.5.4 帧过滤

使用 MAC 帧过滤器可以对接收的帧的目标 MAC 地址和源 MAC 地址进行完美过滤或 HASH 过滤，并可以设置使不通过过滤的帧被丢弃、被接收、或是被在接收描述符中标记出来。此功能请详细阅读 MAC帧过滤寄存器（R32_ETH_MACFFR）的描述。MAC 内置了四个 MAC 地址寄存器，其中 MAC 地址寄存器 0被默认用作存储自身的 MAC 地址，其余三个 MAC 地址寄存器可以被用作完美过滤，与接收到帧的源地址或目标地址对比。在置位 R32_ETH_MACFFR：RA 后，所有接收到的帧都将会被收到，但相应的状态会在之后描述符的第一个字的状态域中被标记出来，如果置位 R32_ETH_MACFFR：PM 会有类似的效果，但是状态不会被标记。在置位R32_ETH_MACFFR：DAIF/SAIF后，会有结果翻转的效果，原来通过过滤的将会被丢弃或标记，未通过过滤的反而会被转给RAM 中的缓冲区。

#### 27.1.5.4.1 单播过滤

MAC 通过 HPF 位和 HU 位来确定单播帧进行 HASH 过滤还是完美地址过滤。

#### 27.1.5.4.2 多播过滤

MAC 通过HPF位和HM位来确定单播帧进行 HASH 过滤还是完美地址过滤。当 PAM位置位后，所有的多播包都能通过过滤器。

#### 27.1.5.4.3 广播过滤

通过置位BFD位，MAC可以阻断所有的广播包。

#### 27.1.5.4.4 源地址过滤选择

通过设置MAC地址寄存器中的AE位,可以启用该 MAC地址寄存器，而设置 SA位可以决定将该 MAC地址寄存器作为源地址样本还是目标地址样本来进行对比。

#### 27.1.5.4.5 小结

对目标地址和源地址的过滤设置分别如表 27-8 和表 27-9。

表 27-8 R32_ETH_MACFFR 各个位的设置对接收到帧的目的 MAC 地址的接受程度  

<table><tr><td>帧类型</td><td>PM</td><td>HPF</td><td>HU</td><td>DAIF</td><td>HM</td><td>PAM</td><td>效果</td></tr><tr><td rowspan="2">广播帧</td><td>1</td><td>-</td><td>-</td><td>-</td><td>-</td><td>-</td><td>通过</td></tr><tr><td>0</td><td>-</td><td>-</td><td>-</td><td>-</td><td>-</td><td>不通过(BFD置位)</td></tr><tr><td rowspan="7">单播帧</td><td>1</td><td>-</td><td>-</td><td>-</td><td>-</td><td>-</td><td>所有帧通过</td></tr><tr><td>0</td><td>-</td><td>0</td><td>0</td><td>-</td><td>-</td><td>完美滤波匹配时通过</td></tr><tr><td>0</td><td>-</td><td>0</td><td>1</td><td>-</td><td>-</td><td>完美滤波匹配时不通过</td></tr><tr><td>0</td><td>0</td><td>1</td><td>0</td><td>-</td><td>-</td><td>HASH滤波匹配时通过</td></tr><tr><td>0</td><td>0</td><td>1</td><td>1</td><td>-</td><td>-</td><td>HASH滤波匹配时不通过</td></tr><tr><td>0</td><td>1</td><td>1</td><td>0</td><td>-</td><td>-</td><td>完美滤波或HASH滤波匹配时通过</td></tr><tr><td>0</td><td>1</td><td>1</td><td>1</td><td>-</td><td>-</td><td>完美滤波或HASH滤波匹配时不通过</td></tr><tr><td rowspan="8">多播帧</td><td>1</td><td>-</td><td>-</td><td>-</td><td>-</td><td>-</td><td>通过</td></tr><tr><td>-</td><td>-</td><td>-</td><td>-</td><td>-</td><td>1</td><td>通过</td></tr><tr><td>0</td><td>-</td><td>-</td><td>0</td><td>0</td><td>0</td><td>完美滤波匹配时通过</td></tr><tr><td>0</td><td>0</td><td>-</td><td>0</td><td>1</td><td>0</td><td>HASH滤波匹配时通过</td></tr><tr><td>0</td><td>1</td><td>-</td><td>0</td><td>1</td><td>0</td><td>完美滤波或HASH滤波匹配时通过</td></tr><tr><td>0</td><td>-</td><td>-</td><td>1</td><td>0</td><td>0</td><td>完美滤波匹配时不通过</td></tr><tr><td>0</td><td>0</td><td>-</td><td>1</td><td>1</td><td>0</td><td>HASH滤波匹配时不通过</td></tr><tr><td>0</td><td>1</td><td>-</td><td>1</td><td>1</td><td>0</td><td>完美滤波或HASH滤波匹配时不通过</td></tr></table>

表 27-9 R32_ETH_MACFFR 各个位的设置对接收到帧的源 MAC 地址的接受程度  

<table><tr><td>帧类型</td><td>RA</td><td>SAIF</td><td>SAF</td><td>效果</td></tr><tr><td rowspan="5">单播帧</td><td>1</td><td>-</td><td>-</td><td>所有帧通过</td></tr><tr><td>0</td><td>0</td><td>0</td><td>完美过滤器匹配时通过,标记但不丢弃未通过的帧</td></tr><tr><td>0</td><td>1</td><td>0</td><td>完美过滤器匹配时不通过,标记但不丢弃未通过的帧</td></tr><tr><td>0</td><td>0</td><td>1</td><td>完美过滤器匹配时通过,丢弃未通过的帧</td></tr><tr><td>0</td><td>1</td><td>1</td><td>完美过滤器匹配时不通过,丢弃未通过的帧</td></tr></table>

注：“-”表示不关心该位的设置。

#### 27.1.5.5 MMC

管理计数器（MMC）的作用主要是对各种指示帧接收发送状态和 MAC 运转状态进行计数。它可以产生设置并产生中断。一般情况下，我们可以通过接收“好”帧计数器（MMCRGUFCR）获取当前接收到的完好的帧的数量，通过发送“好”帧计数器（MMCTGFCR）来获取成功发送出去的帧的数量，通过接收 CRC 校验错误的帧计数器（MMCRFCECR）查看有无接收 CRC 错误的帧，一般情况下，如果数据时正确的但是 CRC 错误，可以认为 RGMII 线 layout 方面有问题。

用户需要明晰一个概念：什么样的帧是“好”的帧。正常情况下，只要一个帧启动发送，它的帧长符合要求（开启了自动填充），并且开启了自动计算 CRC，那么它就会是一个“好”帧。而一个帧被接收到，只要它CRC正确，帧长在以太网帧长度范围内，帧长和长度域的值一致（如果长度类型域表示的是长度），或没有发生不对齐，那么就会被认为是一个“好”的接收帧。

#### 27.1.5.6 PMT

#### 27.1.5.6.1 概述

PMT（power management）部分的功能主要是通过以太网使微控制器从低功耗模式唤醒。千兆以太网控制器支持两种帧使系统唤醒，即魔法帧和（远程）唤醒帧。当以太网收到这两种帧时，由于微控制器处于低功耗模式，因此并不一定会产生接收中断，即使其合法，但是如果其通过 MAC的魔法帧和唤醒帧识别，就会产生PMT中断，PMT中断是和以太网中断独立的中断，通过查询 PMT控制和状态寄存器就能查出是哪种以太网帧产生了中断。

#### 27.1.5.6.2 魔法帧

AMD 公司定义的魔法帧（magic package，也有称幻数据包）是常用的一种唤醒微控制器的以太网帧，它有固定而特殊的帧格式，即通过帧过滤能让目标网卡接收到，可以封装成广播帧，帧中有连续6个字节的全高（OXFF）,之后紧跟16次重复的目标网卡的 MAC地址。这个组合可以存在于帧的任何位置，因此魔法帧可以封装成 Mac 帧或 IP 包甚至 UDP 包。以下是魔法帧的格式。

xx xx xx xx xx xx xx xx(之前的数据无限制,甚至可以没有之前的数据)—ff ff ff ff ff ff84 c2 e4 01 02 02 84 c2 e4 01 02 02 84 c2 e4 01 02 02 84 c2 e4 01 02 02 84 c2 e4 01 0202 84 c2 e4 01 02 02 84 c2 e4 01 02 02 84 c2 e4 01 02 02 84 c2 e4 01 02 02 84 c2 e4 0102 02 84 c2 e4 01 02 02 84 c2 e4 01 02 02 84 c2 e4 01 02 02 84 c2 e4 01 02 02 84 c2 e401 02 02 84 c2 e4 01 02 02 xx xx xx xx xx xx（有的网卡要求魔法帧最后附带上密码）。

#### 27.1.5.6.3 唤醒帧

由于魔法帧的格式存在限制，（远程）唤醒帧的格式可以由用户自行定义。只要是通过远程唤醒帧过滤寄存器（ETH_MACRWUFFR）组的以太网帧都被认为是唤醒帧，用户通过设置远程唤醒帧过滤寄存器组来定义满足自己需要的唤醒帧。需要注意的是，远程唤醒帧过滤寄存器组只有一个地址入口，用户可以通过连续的八次写操作向其中写数据，再通过连续的八次读数据即可读出原来写进远程唤醒帧过滤寄存器组的数据。

远程唤醒帧过滤寄存器组的结构如下表：

表 27-10 远程唤醒帧过滤寄存器组的结构  

<table><tr><td colspan="8">字节屏蔽寄存器0</td></tr><tr><td colspan="8">字节屏蔽寄存器1</td></tr><tr><td colspan="8">字节屏蔽寄存器2</td></tr><tr><td colspan="8">字节屏蔽寄存器3</td></tr><tr><td>保留</td><td>命令3</td><td>保留</td><td>命令2</td><td>保留</td><td>命令1</td><td>保留</td><td>命令0</td></tr><tr><td colspan="2">偏移3</td><td colspan="2">偏移2</td><td colspan="2">偏移1</td><td colspan="2">偏移0</td></tr><tr><td colspan="4">过滤器1</td><td colspan="4">过滤器0</td></tr><tr><td colspan="4">过滤器3</td><td colspan="4">过滤器2</td></tr></table>

由上表可以看出，字节屏蔽寄存器、命令、偏移好过滤器这四个域共同作用能确定一个帧是否是远程唤醒帧，实际上可以设置四种不同的满足要求的帧。4位命令域的最高位表示对什么样的帧起作用，1为只对多播地址有效，0为只对单播地址有效；命令域的第 2 位和第 1位保留，第 0位为使能位，置高表示启用这组过滤器；偏移域表示从帧头开始偏移多少个字节开始计算 CRC16的值，最小填12，实际生效的值为此域的值加一，如若偏移域为 1，则为从开头第13个字节开始计算CRC16的值，即刚好为网络层的第一个字节开始。32位字节屏蔽表示从偏移域定义的开头开始的 31个字节，哪些需要参与CRC16的计算，字节屏蔽寄存器的最高位必须为 0，最多有 31个字节全部参与计算。而过滤器域则存放了用户期望计算出的 CRC结果的值，MAC会将自己计算出的 CRC16的值与这个域之中的值进行比对，若果一致，则当前帧通过远程唤醒帧过滤器的过滤，被 MAC认定为（远程）唤醒帧，如果使能了唤醒帧中断和 PMT中断，则还会产生 PMT中断。

另外，根据根据PMT控制状态寄存器的位定义，如果置位了 GU位，那么通过帧过滤的单播帧也会被认为是唤醒帧。

#### 27.1.5.7 IEEE1588 PTP

#### 27.1.5.7.1 PTP 原理和实现

IEEE1588 标准定义了一套获取时间的精确协议，它的目标是实现 10 微秒误差之内的时间同步。原本 NTP 协议已经可以实现 200 微秒的级别的时间同步，但实际上这个级别还无法满足工业自动化领

域的时间同步需求，于是网络精密时钟同步委员会起草了 PTP协议，并在 2002 年底被 IEEE标准委员会通过，作为IEEE1588标准。

PTP 协议的实现需要主机和从机都能精确记录 MAC接收和发送以太网帧的时间，这需要主机和从机都有一套自己的高精度时间计数器。随后，支持PTP的主从机通过一套流程进行对时，从机可以以此得到自己和主机的时间差并进行修正。下图显示了主从机对时的流程：

![](images/40c9959d59e8a88982742c9a0ef33bb31a369d53704486629810cc691b3d22e0.jpg)  
图 27-9 IEEE1588 PTP 协议同步报文时序图

分步描述：

主机向从机发送 syn 消息，从机接收到此消息，记录下接收到 syn 消息时的本地时间 t2；  
$\bullet$ 主机向从机发送 follow_up 消息，包含主机发送 syn 消息时的主机时间 t1；  
$\bullet$ 从机向主机发送 delay_req 消息，从机记录下发送时间 t3；  
主机向从机发送 delay_resq 消息，包含 delay_req 消息的接收时间 t4；

实际使用中，主机是每隔两秒对外发一次 syn消息的，而每隔一个 syn消息都可以认为是上个syn消息的follow_up消息，他们都会附带上一次syn 消息的发送时间。从机经过这个过程可以得知主机网络到从机网络延迟时间 Tdelay，然后算出主机时间和从机时间的时间偏移。

$$
\text {T d e l a y} = \frac {(t 2 - t 1) + (t 4 - t 3)}{2}
$$

任一主机发出的时间减去偏移即是主机的时间。

PTP的同步流程一般通过UDP协议实现，当然用户也可以自定协议实现。PTP高度依赖内网的延迟稳定性。

#### 27.1.5.7.2 本地时间更新与校正

为了实现 PTP 的主机，设备本地是需要有一个高精度的时间计数器的，起码要精确到纳秒级。微控制器的千兆以太网计数器的 PTP 模块拥有一个 32 位以秒为单位的计数器，和一个 31 位的亚秒计数器，当亚秒计数器溢出时会引起秒计数器自增，因此本地时间的分辨率可以做到 0.46纳秒左右。

本地时间的更新方式分为两种，即粗调和精调。粗调方式的更新时机是由外部决定的，在需要进行时间更新时，将时间戳控制寄存器的时间更新位（PTPTSCR:TSSTU）置位，微控制器的 PTP 模块会将秒计数器和亚秒计数器减去或加上时间戳更新寄存器（TSHUR、TSLUR）的值。粗调是一种较为简单且方便的时间更新机制，但是精确较差。

精调的时间更新方式是更常用的。其更新流程如下：

![](images/87e6d2bbbd8d4ddb9f7e634defd8b118776c1da6906ec68481273c53b06c406b.jpg)  
图 27-10 使用精调方式更新时间的流程

使用精调模式的方式需要对系统主频和精调的流程有较清晰的了解。与粗调方式不同，精调方式更新事件的时机是累加器（32 位，寄存器列表中未列出。图中的 Accumlator register）溢出，累加器在每个系统主频时钟周期都会自增加数寄存器（PTPTSAR,图中的Addend register）的值，一旦溢出就会产生时间更新事件，即亚秒自增寄存器（PTPSSIR，图中的 Constant Value）的值会加到亚秒计数器（Subsecond register），完成时间更新。在亚秒计数器溢出时，秒计数器会自增。而实际上，亚秒计数器自增一位的时间为 1/(2^31)=0.46566128730…纳秒，用户需要自行保证累加器溢出花费的时间刚好等于亚秒自增寄存器的值乘以亚秒计数器自增一位的时间。

本地时间的校正较为简单，将时间戳控制寄存器的时间校正位（PTPTSCR:TSSTI）置位，则秒计数器和亚秒计数器的值会被时间戳更新计数器（TSHUR、TSLUR）的值替代。

### 27.1.6 DMA 操作

#### 27.1.6.1 概述

在以太网收发器中，数据从MII接口进入 FIFO，然后被 DMA转入 RAM中。即使是最大的普通以太网帧，数据部分达到最大的 1500 字节，也只需要几十微秒左右就能接收发送完毕，即使算上 MAC 的接收检测，DMA 转移时间和帧间隔时间，CPU 也需要在一百微秒内就要处理一个帧。以太网收发器的优势是速度快和吞吐量大，为了维持这个优势，必须把以太网帧接受发送过程中，需要CPU进行的干预尽可能得减少，这里就要使用以太网收发器专用 DMA。

以太网使用的DMA是32位的，它通过两种数据结构接受 CPU的管理：传统的控制和状态寄存器，接收和发送描述符。由于 DMA 是 32 位宽的，所以要求收发描述符队列在 RAM 中的基地址是 4 字节对齐的。收发缓冲区则没有对齐要求。

#### 27.1.6.2 DMA 描述符（DMA Desciptor）

用户使用传统外设主要是通过写寄存器的控制位实现，并且以读取状态寄存器的方式来获取外设的状态和返回信息。这些寄存器是独立于内核，SRAM和非易失性存储器之外的空间，是实际存在的，可以称之为“硬件寄存器”。传统的通讯外设，比如 UART 或SPI，都有一个数据寄存器来暂存收发的数据，有一组DMA将收到的所有数据统一保存到特定的地址空间。

而以太网收发器以其极高的数据传输速度、极大的数据吞吐量和独特的数据帧组织形式，使得它的操作不同于传统的通讯收发器。以太网收发器尽可能密集地接收大量的数据，它必须将收到的数据流以帧为单位存到单独的内存空间，自行完成接收和发送操作，使 CPU能以最少的操作就能读取并处理掉这些数据，释放出对应的内存空间，并保证 DMA控制器和CPU不会就内存的使用权限起冲突。内

存中被开辟用来暂存以太网帧的缓冲区单个大小一般设置为以太网帧的最大包长，IEEE802.3规定为1518个字节（包括源止硬件地址、长度或类型域和CRC32校验域共18个字节）；以太网帧收发缓冲区的数量根据实际交互的频度和微控制器内存资源由用户自行确定。

以太网帧的缓冲区以类似队列的形式组织起来，在接收方向，以太网帧通过 MAC被 DMA写入内存而入队，被 CPU 读取而出队；在发送方向，以太网帧被 CPU 写入内存而入队，被 DMA 读取压入发送FIFO而出队。队列的深度即为缓冲区的个数。和普通的通讯外设不同，以太网帧的缓冲区的起始地址和数量并不是固化在某个寄存器中，而是由一类特殊的数据结构管理，这类数据结构存储在内存中，单个此类数据结构单元管理一个缓冲区，里面保存着缓冲区的起始地址、长度、DMA控制器调用此缓冲区时需要进行的设置、DMA发送完成回写的发送状态以及下一个此类数据结构的地址。这种数据结构起到传统通讯外设控制寄存器、状态寄存器的作用，但是实际位置又在内存中，所以可以称之为“软件寄存器”，正式名称为“DMA 描述符”。

DMA 描述符分发送和接收两种，每种的格式固定。DMA 描述符的存储结构分为两种，一种是链表形式，即每个描述符的第四个字为下一个描述符的地址，DMA控制器会直接从 TDes3/RDes3读取下一个描述符；另一种是环式结构，所有的描述符必须紧密排列，DMA控制从当前描述符结束的位置取下一个描述符，当 DMA 控制器检测到 TDes4/RDes4 的 TER/RER 位置位时，即从描述符列表开头的位置取下一个描述符。描述符数组的地址必须4字节对齐，单个描述符的大小为 16字节。

![](images/af1901e061ca4d4819685a21a9ef47cc7d1f5e5e0dbafb0ad95b23ee190834d6.jpg)  
图27-11分别描述了环式结构和链式结构的一种缓冲区分配方案，供用户分配空间时参考。

#### 27.1.6.2.1 发送 DMA 描述符

表 27-11 表明了发送描述符的结构。

31

0

表 27-11 发送描述符的结构  

<table><tr><td>TDes0</td><td>OWM(31)</td><td colspan="2">CTRL(30:26)</td><td>TTSE(25)</td><td>保留</td><td colspan="2">控制(23:20)</td><td colspan="2">保留(19:18)</td><td>TTSS(17)</td><td>状态(16:0)</td></tr><tr><td>TDes1</td><td colspan="2">保留(31:29)</td><td colspan="4">缓冲区2字节计数(28:16)</td><td colspan="2">保留(15:13)</td><td colspan="3">缓冲区1字节计数(12:0)</td></tr><tr><td>TDes2</td><td colspan="11">缓冲区1地址/时间戳低位</td></tr><tr><td>TDes3</td><td colspan="11">缓冲区2地址/下一个描述符的地址/时间戳高位</td></tr></table>

由上表可以看出发送描述符由四个32位字组成，分别是 TDes0、TDes1、TDes2和 TDes3，其中TDes0用作控制和返回发送状态，TDes1用以指示发送长度，TDes2 用以表示发送缓冲区的位置或返回发送时间戳的低位，TDes3用以表示用于第二个发送缓冲区的地址（TCH未置位时）或下一描述符的地址（TCH置位时），时间戳使能时，发送后返回 IEEE1588 时间戳高位。各 32位字的描述如下。

表 27-12 TDes0 的各位定义  

<table><tr><td>31</td><td>30</td><td>29</td><td>28</td><td>27</td><td>26</td><td>25</td><td>24</td><td>23:22</td><td>21</td><td>19</td><td>19:18</td><td>17</td><td>16</td></tr><tr><td>OWN</td><td>IC</td><td>LS</td><td>FS</td><td>DC</td><td>DP</td><td>TTE</td><td>Res</td><td>CIC</td><td>TER</td><td>TCH</td><td>Res</td><td>TSS</td><td>IHE</td></tr></table>

<table><tr><td>15</td><td>14</td><td>13</td><td>12</td><td>11</td><td>10</td><td>9</td><td>8</td><td>7</td><td>6</td><td>5</td><td>4</td><td>3</td><td>2</td><td>1</td><td>0</td></tr><tr><td>ES</td><td>JT</td><td>FF</td><td>IPE</td><td>LCA</td><td>NC</td><td>LCO</td><td>EC</td><td>VF</td><td colspan="4">CC</td><td>ED</td><td>UF</td><td>DB</td></tr></table>

<table><tr><td>位</td><td>名称</td><td>描述</td></tr><tr><td>31</td><td>OWN</td><td>描述符归属位。此位指示此描述符被谁占用。0:此描述符归CPU所有,CPU可以修改此描述符的值;1:此描述符归DMA所有,CPU无权限修改此描述符。此位为0时,在CPU完成对描述符和缓冲区的操作后,由CPU置1;此位为1时,在DMA完成对描述符和缓冲区的操作后,由DMA自动写0。以此完成用户软件和硬件对描述符和收发缓冲区的操作交接。</td></tr><tr><td>30</td><td>IC</td><td>发送完成中断使能位。置此位后,在发送完当前帧之后,发送中断位标志位(ETH_DMASR:TS)将被置位。</td></tr><tr><td>29</td><td>LS</td><td>末段指示位。此位被置位表示此描述符指示的缓冲区里包含帧的结束部分。</td></tr><tr><td>28</td><td>FS</td><td>首段指示位。此位被置位表示此描述符指示的缓冲区里包含帧的开头部分。</td></tr><tr><td>27</td><td>DC</td><td>禁止自动CRC计算位。置此位后,DMA控制器不会计算以太网帧的CRC32检验值,也不会有值附加到帧末尾。此位只有在FS位被置位时有效。另外,DP位的设置优先级高于此位。</td></tr><tr><td>26</td><td>DP</td><td>禁止自动填充位。置此位后,DMA控制器不会为不足64字节的以太网帧添加自动填充。当此位为0时,DMA会自动为不足64字节的以太网帧添加填充和CRC校验值,忽视DC是否被置位。</td></tr><tr><td>25</td><td>TTE</td><td>时间戳发送使能位。在ETH_PTPTSCR:TSE置位的前提下,置此位后DMA控制器将在当面描述符指示的以太网帧打开IEEE1588时间戳功能。该位仅在FS被置位时有效。</td></tr><tr><td>24</td><td>Reserved</td><td>未使用。</td></tr><tr><td>23:22</td><td>CIC</td><td>校验和和插入控制域。00:禁止插入校验和;01:仅使能IP报头校验和的计算和插入;10:保留;11:使能IP报头校验和和有效负载检验和的计算和插入,使能计算伪报头检验和;</td></tr><tr><td>21</td><td>TER</td><td>发送描述符结束标志设置位。用户对此位置位指示DMA控</td></tr><tr><td></td><td></td><td>制器,当前发送描述符已经是发送描述符数组的最后一个描述符。DMA控制器下次会读取发送描述符数组最前面的一个描述符。</td></tr><tr><td>20</td><td>TCH</td><td>下一描述符地址有效指示位。该位置位表示第二个地址是下一个描述符的地址而不是下一个缓冲区的地址。此位置位时,TBS2域的值不起作用。该位只在FS位置位时有效。TER位的优先级高于此位。</td></tr><tr><td>19:18</td><td>Reserved</td><td>未使用。</td></tr><tr><td>17</td><td>TSS</td><td>发送时间戳捕获状态位。DMA控制器置此位表示,发送时间戳已经捕获到,并存放到TDes2和TDes3中。</td></tr><tr><td>16</td><td>IHE</td><td>IP报头错误状态位。DMA控制器会根据接收到的数据检查IP报头:对于IPv4, DMA控制器会检查报头长度域是否正确;对于IPv6, DMA控制器会检查报头是否为40个字节。另外,IP协议类型必须和以太网帧中类型/长度域一致。</td></tr><tr><td>15</td><td>ES</td><td>错误汇总位。如果发送帧时遇到错误,DMA控制器会置此位。在下列位有一位被置位时,ES位就会被置位:UF[TDES0:1]数据下溢错误位;IPE[TDES0:12]IP数据错误位;FF[TDES0:13]帧清空位;JT[TDES0:14]啰嗦超时位(Jabber timeout);IHE[TDES0:16]IP报头错误位。</td></tr><tr><td>14</td><td>JT</td><td>啰嗦超时位(Jabber timeout)。此位被置位时表示MAC发送端发生了啰嗦超时错误。该位只有在JD位(ETHMACCR:22)没有被置位时才会被置位。</td></tr><tr><td>13</td><td>FF</td><td>帧清空位。此位被置位表示由于CPU发出命令,DMA控制器把帧从FIFO清空。</td></tr><tr><td>12</td><td>IPE</td><td>IP包头错误指示位。MAC会把接收到TCP/UDP/ICMP包的IPv4或IPv6包头中的包总长度和实际包长度做对比,若不一致就会置此位。</td></tr><tr><td>11</td><td>LCA</td><td>载波丢失指示位。此位置位表示帧在发送时发生了载波丢失,即CSR信号存在无效状态。该位只在半工模式下起作用。</td></tr><tr><td>10</td><td>NC</td><td>无载波指示位。该位表示帧在发送时物理层的载波侦听信号无效。该位只在半工模式下起作用。</td></tr><tr><td>9</td><td>LCO</td><td>迟到冲突指示位。该位表示帧在发送完前导符之后出现冲突。该位只在半工模式下起作用。</td></tr><tr><td>8</td><td>EC</td><td>冲突过多指示位。该位表示帧发送时出现了16位以上的冲突。如果MACCR的RD位置位,该位表示只发送了一次冲突。该位只在半工模式下起作用。</td></tr><tr><td>7</td><td>VF</td><td>VLAN位。在发送VLAN帧时,此位会被置位。</td></tr><tr><td>6:3</td><td>CC</td><td>冲突计数器域。此域表示帧在发送时发生了多少次冲突。EC置位时无效。该域只在半工模式下起作用。</td></tr><tr><td>2</td><td>EC</td><td>顺延过多指示位。此位置位表示在MACCR的DC置位时,发送帧因为顺延超过24288位而导致发送失败。该位只在半工模式下起作用。</td></tr><tr><td>1</td><td>UF</td><td>UF数据下溢错误位。当DMA控制器发送时从指定的RAM取</td></tr><tr><td></td><td></td><td>数据发现缓冲区为空时，发送进入暂停状态，并置该位和DMASR寄存器的相关位。</td></tr><tr><td>0</td><td>DB</td><td>顺延指示位。此位置位表示发送过程由于载波占用而失败。该位只在半工模式下起作用。</td></tr></table>

表 27-13 TDes1 的各位定义  

<table><tr><td>31</td><td>30</td><td>29</td><td>28</td><td>27</td><td>26</td><td>25</td><td>24</td><td>23</td><td>22</td><td>21</td><td>20</td><td>19</td><td>18</td><td>17</td><td>16</td></tr><tr><td colspan="3">Reserved</td><td colspan="13">TBS2</td></tr></table>

<table><tr><td>15</td><td>14</td><td>13</td><td>12</td><td>11</td><td>10</td><td>9</td><td>8</td><td>7</td><td>6</td><td>5</td><td>4</td><td>3</td><td>2</td><td>1</td><td>0</td></tr><tr><td colspan="3">Reserved</td><td colspan="13">TBS1</td></tr></table>

<table><tr><td>位</td><td>名称</td><td>描述</td></tr><tr><td>31:29</td><td>Reserved</td><td>未使用。</td></tr><tr><td>28:16</td><td>TBS2</td><td>发送缓冲区2的大小。</td></tr><tr><td>15:13</td><td>Reserved</td><td>未使用。</td></tr><tr><td>12:0</td><td>TBS1</td><td>发送缓冲区1的大小。</td></tr></table>

表 27-14 TDes2 的各位定义  

<table><tr><td>31</td><td>30</td><td>29</td><td>28</td><td>27</td><td>26</td><td>25</td><td>24</td><td>23</td><td>22</td><td>21</td><td>20</td><td>19</td><td>18</td><td>17</td><td>16</td></tr><tr><td colspan="16">TBAD1/TTSL</td></tr></table>

<table><tr><td>15</td><td>14</td><td>13</td><td>12</td><td>11</td><td>10</td><td>9</td><td>8</td><td>7</td><td>6</td><td>5</td><td>4</td><td>3</td><td>2</td><td>1</td><td>0</td></tr><tr><td colspan="16">TBAD1/TTSL</td></tr></table>

<table><tr><td>位</td><td>名称</td><td>描述</td></tr><tr><td>31:0</td><td>TBAD1/TTSL</td><td>TDes2 全部 32 位作为一整个单元用来存放发送缓冲区 1 的地址。在启用 IEEE1588 模式中,在完成发送之后,TDes2 也用来存放 MAC 返回的时间戳,同时 MAC 会把 OWN 位 (TDes0:31) 清零。此域存放时间戳的低 32 位。</td></tr></table>

表 27-15 TDes3 的各位定义  

<table><tr><td>31</td><td>30</td><td>29</td><td>28</td><td>27</td><td>26</td><td>25</td><td>24</td><td>23</td><td>22</td><td>21</td><td>20</td><td>19</td><td>20</td><td>17</td><td>16</td></tr><tr><td colspan="16">TDAD2/TTSH</td></tr></table>

<table><tr><td>15</td><td>14</td><td>13</td><td>12</td><td>11</td><td>10</td><td>9</td><td>8</td><td>7</td><td>6</td><td>5</td><td>4</td><td>3</td><td>2</td><td>1</td><td>0</td></tr><tr><td colspan="16">TDAD2/TTSH</td></tr></table>

<table><tr><td>位</td><td>名称</td><td>描述</td></tr><tr><td>31:0</td><td>TDAD2/TTSH</td><td>此域用来存放下一个描述符的地址，在TCH未置位时，此域存放发送缓冲区2的地址。在启用IEEE1588模式中，在完成发送之后，TDes3用来存放MAC返回的时间戳，同时MAC会把OWN位(TDes0:31)清零。此域存放时间戳的高32位。</td></tr></table>

#### 27.1.6.2.2 接收 DMA 描述符

31

0

表 27-16 接收描述符的结构  

<table><tr><td>RDes0</td><td>OWM(31)</td><td colspan="5">状态(30:0)</td></tr><tr><td>RDes1</td><td>控制(31)</td><td>保留(30:29)</td><td>缓冲区2字节计数(28:16)</td><td>RER(15:14)</td><td>保留13</td><td>缓冲区1字节计数(12:0)</td></tr><tr><td>RDes2</td><td colspan="6">缓冲区1地址、时间戳低位</td></tr><tr><td>RDes3</td><td colspan="6">缓冲区2地址、下一个描述符的地址、时间戳高位</td></tr></table>

由上图可以看出接收描述符也是由四个 32位字组成的，其中第一个 32位字主要是返回接收时的状态，第二个32位字包含了接收的数据长度，第三个 32位字定义了接收缓冲区的地址或者返回时间戳的低位，第四个 32 位字做第二个缓冲区的地址（RCH 未置位）,下一缓冲区的地址（RCH 置位）或返回时间戳的高位。

接收描述符各字的各位意义如下：

表 27-17 RDes0 的各位定义  

<table><tr><td>31</td><td>30</td><td>29:16</td></tr><tr><td>OWN</td><td>AFM</td><td>FL</td></tr></table>

<table><tr><td>15</td><td>14</td><td>13</td><td>12</td><td>11</td><td>10</td><td>9</td><td>8</td><td>7</td><td>6</td><td>5</td><td>4</td><td>3</td><td>2</td><td>1</td><td>0</td></tr><tr><td>ES</td><td>DE</td><td>SAF</td><td>LE</td><td>OE</td><td>VLAN</td><td>FS</td><td>LS</td><td>IPHCE</td><td>LCO</td><td>PT</td><td>RWT</td><td>RE</td><td>DE</td><td>CE</td><td>PCE</td></tr></table>

<table><tr><td>位</td><td>名称</td><td>描述</td></tr><tr><td>31</td><td>OWN</td><td>描述符归属位。此位指示此描述符被谁占用。0:此描述符归CPU所有,CPU可以修改此描述符的值;1:此描述符归DMA所有,CPU无权限修改此描述符。此位为0时,在CPU完成对描述符和缓冲区的操作后,由CPU置1;此位为1时,在DMA完成对描述符和缓冲区的操作后,由DMA自动写0。以此完成用户软件和硬件对描述符和收发缓冲区的操作交接。</td></tr><tr><td>30</td><td>AFM</td><td>目标地址未通过标志位。如果接收的帧未通过MAC的目标地址过滤器,那么此标志位会被置位。</td></tr><tr><td>29:16</td><td>FL</td><td>帧长域。此域在ES位[RDes0:15]为0时有效。在LS位[RDes0:8]为1时,此域指示了DMA控制器接收到的帧的长度,包括CRC;在LS位为0时,此域表示目前为止DMA控制器发往内存的累计长度,单位都是字节。</td></tr><tr><td>15</td><td>ES</td><td>错误汇总位。当MAC检测到以下任一错误时,会置位此位:CE[RDes0:1]:CRC错误;RE[RDes0:3]:接收错误;RWT[RDes0:4]:看门狗超时;</td></tr><tr><td></td><td></td><td>IPHCE[RDes0:7]:巨型帧(注意,报 IPHCE 时需要分辨到底是巨型帧还是由 IP 头错误);OE[RDes0:11]:溢出错误;DE[RDes0:14]:描述符错误。</td></tr><tr><td>14</td><td>DE</td><td>描述符错误位。该位被置位表示由于描述符指示的缓冲区装不上当前帧而被切断,DMA 又不占用下一描述符。该位只在 LS 位[RDes0:8]被置位时才有效。</td></tr><tr><td>13</td><td>SAF</td><td>源地址过滤未通过标志位。此位被置位表示帧没有通过MAC 的源地址过滤器。</td></tr><tr><td>12</td><td>LE</td><td>长度错误位。此位被置位表示实际收到的帧长度和以太网类型/长度域中指示的长度不符合。此位只在 FT[RDes0:5]被置位时有效。</td></tr><tr><td>11</td><td>OE</td><td>溢出错误位。此位被置位表示由于接收 FIFO 溢出,接收到的帧被破坏。</td></tr><tr><td>10</td><td>VLAN</td><td>VLAN 标签位。该位被置位表示接收到的是 VLAN 帧。</td></tr><tr><td>9</td><td>FS</td><td>首描述符指示位。该位置位表示这个描述符包含帧的开头。</td></tr><tr><td>8</td><td>LS</td><td>尾描述符指示位。该位置位表示这个描述符包含帧的结尾。</td></tr><tr><td>7</td><td>IPHCE</td><td>IP 报头校验和错误标志位。该位置位表示 IPv4 或者 IPv6 报头存在错误,具体原因可能是:1、以太网帧类型/长度域指示的协议与实际的 IP 版本不一致;2、IP 报头校验和不对;3、IP 报头指示的长度不对。</td></tr><tr><td>6</td><td>LCO</td><td>迟到冲突指示位。该位置位表示产生了迟到冲突。该位只在半双工模式下起作用。</td></tr><tr><td>5</td><td>FT</td><td>帧类型指示位。此位为 1 时表示接收到的帧为以太网类型封装的帧(RFC 894)。此位为 0 时表示接收到的帧为IEEE802.3 类型封装的帧(RFC1042)。当帧长度小于 14 字节时,此位无效。注:FT 具有的特殊含义参考表 27-18</td></tr><tr><td>4</td><td>RWT</td><td>接收看门狗超时标志位。该位置位表示在接收当前帧时,看门狗超时,当前帧被截断。</td></tr><tr><td>3</td><td>RE</td><td>接收错误标志位。该位置位表示在接收帧的过程中,RX_DV有效时,RX_ERR 信号有效。</td></tr><tr><td>2</td><td>DE</td><td>Dribble 比特错误位。该位置位表示 MAC 接收到的帧的长度不是 8bit 的帧数倍,可能有漏掉周期的现象。</td></tr><tr><td>1</td><td>CE</td><td>CRC 错误。该位置位表示接收到的帧存在 CRC 校验错误。该位只在 LS 位[RDes0:8]置位时有效。</td></tr><tr><td>0</td><td>PCE</td><td>负载校验和错误。该位置位表示 MAC 接收到的TCP/UDP/ICMP 包与其校验和域标示的值不符。</td></tr></table>

可以看出 MAC 的接收流程中做了校验检验机制。实际上在以太网帧层（数据链路层）、网络层（IPv4/IPv6）和运输层（TCP/UDP/SCTP）都有对本层数据长度进行说明，对数据内容正确性采取某种手段的校验。RDES0 的第 0、5 和 7 位都提到了对数据的校验检验，下表进行归纳。

表 27-18 RDES:7/5/0 的值和接收到的帧状态的关系  

<table><tr><td>RDESO:7</td><td>RDESO:5</td><td>RDESO:0</td><td rowspan="2">帧状态</td></tr><tr><td>IPHCE</td><td>FT</td><td>PCE</td></tr><tr><td>1</td><td>1</td><td>1</td><td>为IP帧，IP层和运输层存在校验错误。</td></tr><tr><td>1</td><td>1</td><td>0</td><td>为IP帧，IP层存在校验错误。</td></tr><tr><td>1</td><td>0</td><td>1</td><td>为IP帧，运输层存在校验错误。</td></tr><tr><td>1</td><td>0</td><td>0</td><td>为IP帧，未检测到校验和错误。</td></tr><tr><td>0</td><td>1</td><td>1</td><td>非IP帧，未执行检验检测。</td></tr><tr><td>0</td><td>1</td><td>0</td><td>保留。</td></tr><tr><td>0</td><td>0</td><td>1</td><td>为IP帧，不支持的运输层协议，未检测到IP层校验和错误。</td></tr><tr><td>0</td><td>0</td><td>0</td><td>长度/类型域小于0x0600的帧。</td></tr></table>

表 27-19 RDes1 的各位定义  

<table><tr><td>31</td><td>30</td><td>29</td><td>28</td><td>27</td><td>26</td><td>25</td><td>24</td><td>23</td><td>22</td><td>21</td><td>20</td><td>19</td><td>18</td><td>17</td><td>16</td></tr><tr><td>DIC</td><td colspan="2">Reserved</td><td colspan="13">RBS2</td></tr></table>

<table><tr><td>15</td><td>14</td><td>13</td><td>12</td><td>11</td><td>10</td><td>9</td><td>8</td><td>7</td><td>6</td><td>5</td><td>4</td><td>3</td><td>2</td><td>1</td><td>0</td></tr><tr><td>RER</td><td>RCH</td><td>Res</td><td colspan="13">RBS1</td></tr></table>

<table><tr><td>位</td><td>名称</td><td>描述</td></tr><tr><td>31</td><td>DIC</td><td>关闭接收完成中断设置位。</td></tr><tr><td>30:29</td><td>Reserved</td><td>未使用。</td></tr><tr><td>28:16</td><td>RBS2</td><td>接收缓冲区2大小。</td></tr><tr><td>15</td><td>RER</td><td>末端接收描述符标志。指示当前描述符是最后一个描述符。DMA控制器会回到描述符对列基地址寄存器(ETH_DMARDLAR)去取下一个描述符。</td></tr><tr><td>14</td><td>RCH</td><td>下一接收描述符地址有效位。该位置位表示最后一个32位字中是下一个接收描述符的地址,否则则是第2个缓冲区的地址。</td></tr><tr><td>13</td><td>Reserved</td><td>未使用。</td></tr><tr><td>12:0</td><td>RBS1</td><td>接收缓冲区1大小。</td></tr></table>

表 27-20 RDes2 的各位定义  

<table><tr><td>31</td><td>30</td><td>29</td><td>28</td><td>27</td><td>26</td><td>25</td><td>24</td><td>23</td><td>22</td><td>21</td><td>20</td><td>19</td><td>18</td><td>17</td><td>16</td></tr><tr><td colspan="16">RBAD1/RTSL</td></tr></table>

<table><tr><td>15</td><td>14</td><td>13</td><td>12</td><td>11</td><td>10</td><td>9</td><td>8</td><td>7</td><td>6</td><td>5</td><td>4</td><td>3</td><td>2</td><td>1</td><td>0</td></tr><tr><td colspan="16">RBAD1/RTSL</td></tr></table>

<table><tr><td>位</td><td>名称</td><td>描述</td></tr><tr><td>31:0</td><td>RBAD/RTSL</td><td>RDes2 全部 32 位作为一整个单元用来存放接收缓冲区的地址。在启用 IEEE1588 模式中，在完成接收之后，RDes2</td></tr><tr><td></td><td></td><td>也用来存放 MAC 返回的时间戳，同时 MAC 会把 OWN 位 [RDes0:31]清零。</td></tr></table>

表 27-21 RDes3 的各位定义  

<table><tr><td>31</td><td>30</td><td>29</td><td>28</td><td>27</td><td>26</td><td>25</td><td>24</td><td>23</td><td>22</td><td>21</td><td>20</td><td>19</td><td>18</td><td>17</td><td>16</td></tr><tr><td colspan="16">RDAD2/RTSH</td></tr></table>

<table><tr><td>15</td><td>14</td><td>13</td><td>12</td><td>11</td><td>10</td><td>9</td><td>8</td><td>7</td><td>6</td><td>5</td><td>4</td><td>3</td><td>2</td><td>1</td><td>0</td></tr><tr><td colspan="16">RDAD2/RTSH</td></tr></table>

<table><tr><td>位</td><td>名称</td><td>描述</td></tr><tr><td>31:0</td><td>RDAD/RTSH</td><td>RCR 置位时用来存放下一个缓冲区的地址，否则用来存在第二个缓冲区的地址。在启用 IEEE1588 模式中，在完成接收之后，RDes3 用来存放 MAC 返回的时间戳，同时 MAC 会把 OWN 位 [RDes0:31] 清零。此域存放时间戳的高 32 位。</td></tr></table>

#### 27.1.6.3 数据缓存对齐

由于收发缓冲区和收发描述符都是由 DMA 控制器调用，并实质存在于 RAM 空间的，而 DMA 是 32位的，因此设置收发描述符队列都需要保证它们的起始地址在4 字节对齐的位置，但是设置缓冲区没有这个强制要求。

为了提高效率，一个以太网帧由一个缓冲区接收完毕是最恰当的，如将缓冲区设为 1518 个字节可以容纳下最大的以太网普通帧，包括源止地址域、长度类型域、数据填充域和 CRC校验域。需要注意的是，带标签的VLAN帧的最大帧长比一般的以太网帧长 4个字节，即 1522个字节，如果用户想做到4字节对齐，一个缓冲区可以为1524字节。

#### 27.1.6.4 DMA 收发配置

接收和发送的数据由DMA负责自动转存和推送，但是如果遇到致命错误，DMA会停止运转，并更新DMA状态寄存器。用户用需要初始化DMA后手动启动 DMA才能继续运转。

#### 27.1.6.4.1 发送 DMA 配置

DMA 控制器建立运转机制的步骤如下：

1） 设置发送缓冲区队列，将要发送的内容填入发送缓冲区。设置发送描述符队列，填好发送描述符的各域各位，并将OWN置位，将发送描述符的管理权交付给 DMA控制器；将描述符初始地址注册到 DMATDLAR 寄存器中；  
2） 置位 ST 位（DMAOMR:13），开启 DMA；  
3） 在运行模式下，DMA描述符会自动读取发送描述符的内容，根据其指示的地址和长度把数据推送到发送 FIFO。结束后 DMA 会按照链式结构读取紧接着的下一个发送描述符进行下一次发送。当DMA 控制器检测到 OWM 位未置位导致无权访问发送描述符或者其他正常的错误，就会终止传输，并将 TBUS 位（DMASR:2）或其他位（非 OWN 为 0 引起的错误）和 NIS 位（DMASR:16）置位。  
4） 不允许单个帧跨越多个描述符，一帧必须由一个描述符和一个缓冲区关联清楚。  
5） 如果 MAC 开启了 IEEE1588 PTP 模式，那么在 DMA 控制器把数据推送到 FIFO 之后，MAC 会把发送时间戳写到 TDes2 和 TDes3 中，并复位 OWN 位。  
6） 在发送完一个帧之后，如果发送描述符使能了发送完成中断（置位 TDes1:31），DMA 控制器就会置位发送完成中断标志位（DMASR:0），然后继续取下一个发送描述符。下图展示了默认发送流程。

![](images/276a3bdba27a09dd4d09151b01d16703b9b91dc89c5e2991145b24ec05cc8bb6.jpg)  
图 27-11 发送流程

#### 27.1.6.4.2 接收 DMA 配置

建立 DMA 接收流转机制的步骤如下：

1） 设置发送缓冲区队列和描述符队列，设置好接收描述符的各个域各个位；将描述符初始地址注册到DMARDLAR寄存器中；置位OWN位，将描述符使用权限交给 DMA控制器；  
2） 置位 SR 位，开始接收流程；  
3） 在接收流转机制运行时，DMA控制器获取下一个描述符，检查接收描述符配置，在 FIFO 接收到下一帧时，对帧内容进行过滤和识别等多项检查，将帧数据转发描述符指定的缓冲区中，写描述符状态域并获取下一个接收描述符，报接收完成中断。如果帧未通过过滤会被标记或丢弃，如果帧存在检验和错误、CRC错误或帧过短将会被标记，如果帧过长可能会被掐断或报接收看门狗超时错误。DMA 控制器遇到接收描述符不可用等致命错误将会停止接收流程，用户需要特别留意；  
4） 用户至少需要使能一个接收完成中断，在以太网中断函数中将使用过的接收描述符恢复到待命状态，以保证接收流程可以不间断地运行下去。用户可以在中断函数中将待处理的数据缓冲区地址传递出来，或处理一些打断接收流程的异常事件。  
5） 如果使能了 PTP 时间戳，再 DMA 控制器转存数据，写描述符状态域时，同时也会将当前的时间戳写进描述符的后两个字。用户应及时将时间戳读出并补全原写在后两个字的缓冲区地址和下一描述符地址。

下图展示了默认接收流转机制：

![](images/a7520b2d805d6cfbad9827222c288090fa063107043848d346c32effab121235.jpg)  
图 27-12 接收流程

### 27.1.7 中断

以太网收发器拥有两个中断向量，一个是以太网唤醒事件，另一个是普通的收发事件。当检测到唤醒帧或魔法帧的时候，会触发以太网唤醒事件。普通的以太网收发中断事件包括 DMA中断和ETH 中断。

#### 27.1.7.1 DMA 中断

DMA 中断大致可以分为两组，即正常的中断（NIS）和异常的中断（AIS），异常的中断一般意味着数据收发的异常，需要特别注意。用户在处理中断时需要检索所有的中断标志位，并将已经产生的中断源全部处理掉。下图是以太网收发器的中断示意图。

![](images/d234f8e1e0276be819a45137baccdf2a9edb33c534093201cdc2194196620246.jpg)  
图 27-13 中断示意图

DMA中断是以太网收发器中最重要的中断，一般的用户逻辑中都需要依靠中断接收帧，确认帧发送出去，或是及时处理被打断的收发逻辑。

#### 27.1.7.2 ETH 中断

ETH 中断主要包括 PTP 的时间闹钟触发，收发计数到了 MMC 寄存器的某种设定值，以及 PMT。ETH的用处主要是功能性的，相对 DMA 中断而言并不十分复杂和重要。用户可以使用 PTP 中断实现闹钟，使用MMC中断实现测速，或使用PMT中断实现远程唤醒。另外，ETH还支持当内置物理层连接状态发生改变时产生中断。

#### 27.1.7.3 PMT 中断

将PMT中断单独提出来重述一遍的原因是这个中断拥有独立的中断向量，使用时需注意。

## 27.1.8 寄存器描述

表27-22 以太网收发器相关寄存器列表  
MAC控制相关寄存器地址映射   

<table><tr><td>名称</td><td>偏移地址</td><td>描述</td><td>复位值</td></tr><tr><td>R32 ETH MACCR</td><td>0x40028000</td><td>MAC 控制寄存器</td><td>0x00008000</td></tr><tr><td>R32 ETH MACFFR</td><td>0x40028004</td><td>帧过滤寄存器</td><td>0x00000000</td></tr><tr><td>R32 ETH MACHTHR</td><td>0x40028008</td><td>哈希值列表寄存器高位</td><td>0x00000000</td></tr><tr><td>R32 ETH MACHTLR</td><td>0x4002800C</td><td>哈希值列表寄存器低位</td><td>0x00000000</td></tr><tr><td>R32 ETH MACMI IAR</td><td>0x40028010</td><td>MII 地址寄存器</td><td>0x00000000</td></tr><tr><td>R32 ETH MACMI IDR</td><td>0x40028014</td><td>MII 数据寄存器</td><td>0x00000000</td></tr><tr><td>R32 ETH MACFCR</td><td>0x40028018</td><td>MAC 流控寄存器</td><td>0x00000000</td></tr><tr><td>R32 ETH MACVLAN</td><td>0x4002801C</td><td>VLAN 标签寄存器</td><td>0x00000000</td></tr><tr><td>R32 ETH MACRWUFFR</td><td>0x40028028</td><td>唤醒帧过滤器寄存器</td><td>0x00000000</td></tr><tr><td>R32 ETH MACPMTCSR</td><td>0x4002802C</td><td>PMT 控制和状态寄存器</td><td>0x00000000</td></tr><tr><td>R32 ETH MACSR</td><td>0x40028038</td><td>MAC 中断状态寄存器</td><td>0x00000000</td></tr><tr><td>R32 ETH MACIMR</td><td>0x4002803C</td><td>MAC 中断屏蔽寄存器</td><td>0x00000000</td></tr><tr><td>R32 ETH MACAOHR</td><td>0x40028040</td><td>MAC 地址 0 寄存器高位</td><td>0x0010FFFF</td></tr><tr><td>R32 ETH MACAOLR</td><td>0x40028044</td><td>MAC 地址 0 寄存器低位</td><td>0xFFFFFF</td></tr><tr><td>R32 ETH_MACA1HR</td><td>0x40028048</td><td>MAC 地址 1 寄存器高位</td><td>0x0000FFF</td></tr><tr><td>R32 ETH_MACA1LR</td><td>0x4002804C</td><td>MAC 地址 1 寄存器低位</td><td>0xFFFFFFFFF</td></tr><tr><td>R32 ETH_MACA2HR</td><td>0x40028050</td><td>MAC 地址 2 寄存器高位</td><td>0x0000FFF</td></tr><tr><td>R32 ETH_MACA2LR</td><td>0x40028054</td><td>MAC 地址 2 寄存器低位</td><td>0xFFFFFFFFF</td></tr><tr><td>R32 ETH_MACA3HR</td><td>0x40028058</td><td>MAC 地址 3 寄存器高位</td><td>0x0000FFF</td></tr><tr><td>R32 ETH_MACA3LR</td><td>0x4002805C</td><td>MAC 地址 3 寄存器低位</td><td>0xFFFFFFFFF</td></tr></table>

MMC 控制相关寄存器地址映射，注意：地址非连续  

<table><tr><td>名称</td><td>偏移地址</td><td>描述</td><td>复位值</td></tr><tr><td>R32 ETH_MMCCR</td><td>0x40028100</td><td>MMC 控制寄存器</td><td>0x00000000</td></tr><tr><td>R32 ETH_MMCRIR</td><td>0x40028104</td><td>MMC 接收寄存器</td><td>0x00000000</td></tr><tr><td>R32 ETH_MMCTIR</td><td>0x40028108</td><td>MMC 发送中断寄存器</td><td>0x00000000</td></tr><tr><td>R32 ETH_MMCRIMR</td><td>0x4002810C</td><td>MMC 接收中断屏蔽寄存器</td><td>0x00000000</td></tr><tr><td>R32 ETH_MMCTIMR</td><td>0x40028110</td><td>MMC 发送中断屏蔽寄存器</td><td>0x00000000</td></tr><tr><td>R32 ETH_MMCTGFSCCR</td><td>0x4002814C</td><td>MMC 一次冲突发送好帧计数器</td><td>0x00000000</td></tr><tr><td>R32 ETH_MMCTGFMSCCR</td><td>0x40028150</td><td>MMC 多次冲突发送好帧计数器</td><td>0x00000000</td></tr><tr><td>R32 ETH_MMCTGFCR</td><td>0x40028168</td><td>MMC 发送好帧计数寄存器</td><td>0x00000000</td></tr><tr><td>R32 ETH_MMRCRFCER</td><td>0x40028194</td><td>MMC 存在 CRC 检验错误的帧接收计数寄存器</td><td>0x00000000</td></tr><tr><td>R32 ETH_MMRCFAECR</td><td>0x40028198</td><td>接收对齐错误帧计数器</td><td>0x00000000</td></tr><tr><td>R32 ETH_MMRCGUFCR</td><td>0x400281C4</td><td>MMC 接收奢单播帧计数寄存器</td><td>0x00000000</td></tr></table>

IEEE1588（PTP）相关寄存器地址映射   

<table><tr><td>名称</td><td>偏移地址</td><td>描述</td><td>复位值</td></tr><tr><td>R32 ETH PTPTSCR</td><td>0x40028700</td><td>PTP时间戳控制寄存器</td><td>0x00000000</td></tr><tr><td>R32 ETH PTPSSIR</td><td>0x40028704</td><td>PTP亚秒递增寄存器</td><td>0x00000000</td></tr><tr><td>R32 ETH PTPTSHR</td><td>0x40028708</td><td>PTP时间戳寄存器高位</td><td>0x00000000</td></tr><tr><td>R32 ETH PTPTSLR</td><td>0x4002870C</td><td>PTP时间戳寄存器低位</td><td>0x00000000</td></tr><tr><td>R32 ETH PTPTSHUR</td><td>0x40028710</td><td>PTP时间戳更新寄存器高位</td><td>0x00000000</td></tr><tr><td>R32 ETH PTPTSLUR</td><td>0x40028714</td><td>PTP时间戳更新寄存器低位</td><td>0x00000000</td></tr><tr><td>R32 ETH PTPTSAR</td><td>0x40028718</td><td>PTP时间戳加数寄存器</td><td>0x00000000</td></tr><tr><td>R32 ETH PTPTTHR</td><td>0x4002871C</td><td>PTP目标寄存器高位</td><td>0x00000000</td></tr><tr><td>R32 ETH PTPTTLR</td><td>0x40028720</td><td>PTP目标寄存器低位</td><td>0x00000000</td></tr></table>

DMA 相关寄存器地址映射注意：地址非连续  

<table><tr><td>名称</td><td>偏移地址</td><td>描述</td><td>复位值</td></tr><tr><td>R32 ETH_DMABMR</td><td>0x40029000</td><td>DMA总线模式寄存器</td><td>0x00002101</td></tr><tr><td>R32 ETH_DMATPDR</td><td>0x40029004</td><td>DMA发送查询寄存器</td><td>0x00000000</td></tr><tr><td>R32 ETH DMARPDR</td><td>0x40029008</td><td>DMA接收查询寄存器</td><td>0x00000000</td></tr><tr><td>R32 ETH DMARDLAR</td><td>0x4002900C</td><td>DMA接收描述符地址寄存器</td><td>0x00000000</td></tr><tr><td>R32 ETH DMATDLAR</td><td>0x40029010</td><td>DMA发送描述符地址寄存器</td><td>0x00000000</td></tr><tr><td>R32 ETH DMASR</td><td>0x40029014</td><td>DMA状态寄存器</td><td>0x00000000</td></tr><tr><td>R32 ETH DMAOMR</td><td>0x40029018</td><td>DMA操作模式寄存器</td><td>0x00000000</td></tr><tr><td>R32 ETH DMAIER</td><td>0x4002901C</td><td>DMA中断使能寄存器</td><td>0x00000000</td></tr><tr><td>R32 ETH DMAMFBOCR</td><td>0x40029020</td><td>DMA 丢失帧寄存器</td><td>0x00000000</td></tr><tr><td>R32 ETH DMACHTDR</td><td>0x40029048</td><td>DMA 当前发送描述符寄存器</td><td>0x00000000</td></tr><tr><td>R32 ETH DMACHRDR</td><td>0x4002904C</td><td>DMA 当前接收描述符寄存器</td><td>0x00000000</td></tr><tr><td>R32 ETH DMACHTBAR</td><td>0x40029050</td><td>DMA 当前发送缓存寄存器</td><td>0x00000000</td></tr><tr><td>R32 ETH DMACHRBAR</td><td>0x40029054</td><td>DMA 当前接收缓存寄存器</td><td>0x00000000</td></tr></table>

内部 10M 物理层相关寄存器地址  

<table><tr><td>名称</td><td>偏移地址</td><td>描述</td><td>复位值</td></tr><tr><td>BMCR</td><td>0x00</td><td>基本控制寄存器</td><td>0x2100</td></tr><tr><td>BMSR</td><td>0x01</td><td>基本状态寄存器</td><td>0x1809</td></tr><tr><td>PHY_SR</td><td>0x10</td><td>物理层状态寄存器</td><td>0x0000</td></tr><tr><td>PHY_MDIX</td><td>0x1E</td><td>自动翻转寄存器</td><td>0x0000</td></tr></table>

注：内部物理层寄存器的偏移地址在SMI接口中使用

### 27.1.8.1 MAC 控制相关寄存器各位域

#### 27.1.8.1.1 MAC 控制寄存器（R32_ETH_MACCR）

偏移地址： $0 \times 0 0$   

<table><tr><td>31</td><td>30</td><td>29</td><td>28</td><td>27</td><td>26</td><td>25</td><td>24</td><td>23</td><td>22</td><td>21</td><td>20</td><td>19</td><td>18</td><td>17</td><td>16</td></tr><tr><td colspan="3">TCD</td><td colspan="5">Reserved</td><td>WD</td><td>JD</td><td>PI</td><td>PR</td><td colspan="3">IFG</td><td>Reserved</td></tr><tr><td>15</td><td>14</td><td>13</td><td>12</td><td>11</td><td>10</td><td>9</td><td>8</td><td>7</td><td>6</td><td>5</td><td>4</td><td>3</td><td>2</td><td>1</td><td>0</td></tr><tr><td>FES</td><td>Reserved</td><td>LM</td><td>DM</td><td>IPCO</td><td colspan="2">Reserved</td><td>APCS</td><td colspan="4">Reserved</td><td>TE</td><td>RE</td><td>TCF</td><td>Reserved</td></tr></table>

<table><tr><td>位</td><td>名称</td><td>访问</td><td>描述</td><td>复位值</td></tr><tr><td>[31:29]</td><td>TCD</td><td>RW</td><td>发送时钟延迟域。此域用来延迟发送时钟。MAC控制器把经过CES位(ETH_MACCR[1])选择的发送时钟进行延时输出。延时时间计算公式:Tdaleny=TCD(ETH_MACCR[31:29])*0.5ns;</td><td>0</td></tr><tr><td>[28:24]</td><td>Reserved</td><td>RO</td><td>保留。</td><td>0</td></tr><tr><td>23</td><td>WD</td><td>RW</td><td>看门狗设置位。0:MAC打开看门狗,只能接受最大2048字节的以太网帧,超长部分会被切断;1:MAC关闭开门狗,能接收最大16384个字节的以太网帧;</td><td>0</td></tr><tr><td>22</td><td>JD</td><td>RW</td><td>Jabber设置位。0:如果用户试图发送超过2048字节以上长度的以太网帧,MAC会关闭发送器;1:MAC关闭Jabber定时器,最多能发送16384字节的以太网帧;</td><td>0</td></tr><tr><td>21</td><td>PI</td><td>RW</td><td>内置10MPHY发送驱动偏置电流设置位。0:额定驱动;1:节能发送;</td><td>0</td></tr><tr><td>20</td><td>PR</td><td>RW</td><td>内置10MPHY片内50欧电阻上拉开启设置位。0:片内50欧电阻断开;1:片内50欧电阻连接;</td><td>0</td></tr><tr><td>[19:17]</td><td>IFG</td><td>RW</td><td>帧间隙设置域。这里设置了发送两个帧之间的最短时间间隙。000:96位时间;001:88位时间;010:80位时间;011:72位时间;100:64位时间;101:56位时间;110:48位时间;111:40位时间。</td><td>000b</td></tr><tr><td>16</td><td>Reserved</td><td>RO</td><td>保留。</td><td>0</td></tr><tr><td>[15:14]</td><td>FES</td><td>RW</td><td>以太网速度设置域。00:10Mbit/s;01:100Mbit/s;10:1Gbit/s;11:保留,未使用。</td><td>00b</td></tr><tr><td>13</td><td>Reserved</td><td>RO</td><td>保留。</td><td>0</td></tr><tr><td>12</td><td>LM</td><td>RW</td><td>自循环模式使能位。置该位使能自循环模式。</td><td>0</td></tr><tr><td>11</td><td>DM</td><td>RW</td><td>双工模式使能位。置该位使能全双工模式。</td><td>0</td></tr><tr><td>10</td><td>IPCO</td><td>RW</td><td>IPv4校验检验使能位。0:关闭接收端IPv4的校验和检验功能,相应的PCE、PHCE标志位总是为0。见接收描述符各位定义;1:使能IPv4的校验检验,MAC控制器会对TCP、UDP和ICMP报头进行校验和检验。</td><td>0</td></tr><tr><td>[9:8]</td><td>Reserved</td><td>RO</td><td>保留。</td><td>0</td></tr><tr><td>7</td><td>APCS</td><td>RW</td><td>填充&amp;CRC自动剥离使能位。0:MAC不改变帧的内容;1:在接收长度小于等于1500字节的以太网帧时,MAC自动去除帧的填充字节和CRC域;在大于1500字节的以太网帧中,MAC不做更改。</td><td>0</td></tr><tr><td>[6:4]</td><td>Reserved</td><td>RO</td><td>保留。</td><td>0</td></tr><tr><td>3</td><td>TE</td><td>RW</td><td>发送使能位。0:MAC在发送完当前帧之后,关闭发送器,不再发送任何帧;1:使能MAC发送器。</td><td>0</td></tr><tr><td>2</td><td>RE</td><td>RW</td><td>接收使能位。0:MAC在接收完当前帧之后,关闭接收器,不再接收任何帧;1:使能MAC接收器。</td><td>0</td></tr><tr><td>1</td><td>TCF</td><td>RW</td><td>发送时钟翻转设置位。0:将TCEs选择的TXC直接作为芯片输出的GTX_CLK;</td><td>0</td></tr><tr><td></td><td></td><td></td><td>1:将 TCES 选择的 TXC 的反相作为芯片输出的 GTX_CLK。</td><td></td></tr><tr><td>0</td><td>Reserved</td><td>R0</td><td>保留。</td><td>0</td></tr></table>

#### 27.1.8.1.2 MAC 帧过滤寄存器（R32_ETH_MACFFR）

偏移地址： $0 \times 0 4$

<table><tr><td>31</td><td>30</td><td>29</td><td>28</td><td>27</td><td>26</td><td>25</td><td>24</td><td>23</td><td>22</td><td>21</td><td>20</td><td>19</td><td>18</td><td>17</td><td>16</td></tr><tr><td>RA</td><td colspan="15">Reserved</td></tr><tr><td>15</td><td>14</td><td>13</td><td>12</td><td>11</td><td>10</td><td>9</td><td>8</td><td>7</td><td>6</td><td>5</td><td>4</td><td>3</td><td>2</td><td>1</td><td>0</td></tr><tr><td colspan="4">Reserved</td><td>HPF</td><td>SAF</td><td>SAIF</td><td colspan="2">PCF</td><td>BFD</td><td>PAM</td><td>DAIF</td><td>HM</td><td>HU</td><td colspan="2">PM</td></tr></table>

<table><tr><td>位</td><td>名称</td><td>访问</td><td>描述</td><td>复位值</td></tr><tr><td>31</td><td>RA</td><td>RW</td><td>接收全部位。0:MAC只把通过了过滤器的帧转发到接收队列中;1:MAC把所有收到的帧转发到接收队列中,不管其是否通过了过滤器。</td><td>0</td></tr><tr><td>[30:11]</td><td>Reserved</td><td>RO</td><td>保留。</td><td>0</td></tr><tr><td>10</td><td>HPF</td><td>RW</td><td>HASH过滤或者完美过滤选择位。0:在HM或者HU置位的前提下,只要符合HASH过滤器,就能通过地址过滤;1:根据HM或者HU的值来确定单播/多播模式下使用什么过滤方式。</td><td>0</td></tr><tr><td>9</td><td>SAF</td><td>RW</td><td>源地址过滤选择位。0:MAC对未通过源MAC地址过滤的帧进行标记;1:MAC会直接丢弃未通过源MAC地址过滤的帧;</td><td>0</td></tr><tr><td>8</td><td>SAIF</td><td>RW</td><td>源地址过滤结果颠倒位。0:接收的帧的源地址如果和MAC地址寄存器中启用的源地址不一致则认为是未通过源地址过滤器;1:接收的帧的源地址如果和MAC地址寄存器中启用的源地址一致则认为是未通过源地址过滤器;</td><td>0</td></tr><tr><td>[7:6]</td><td>PCF</td><td>RW</td><td>流控帧通过控制域。00/01:MAC不转发任何流控帧给应用程序;10:MAC转发所有流控帧到应用程序中,包括未通过地址过滤器的流控帧;11:MAC只转发通过地址过滤器的流控帧;</td><td>00b</td></tr><tr><td>5</td><td>BFD</td><td>RW</td><td>广播帧接收控制位。0:接收所有广播帧;1:丢弃所有广播帧;</td><td>0</td></tr><tr><td>4</td><td>PAM</td><td>RW</td><td>通过全部多播帧控制位。0:多播帧能否通过过滤取决于HM的值;1:全部的多播帧都能通过地址过滤;</td><td>0</td></tr><tr><td>3</td><td>DAIF</td><td>RW</td><td>目的MAC地址过滤器过滤结果颠倒控制位。0:过滤器结果正常生效;1:对多播帧和单播帧,是否过滤器通过的结果进行</td><td>0</td></tr><tr><td></td><td></td><td></td><td>颠倒再生效;</td><td></td></tr><tr><td>2</td><td>HM</td><td>RW</td><td>多播帧过滤模式选择位。
0:进行完美地址过滤;
1:进行 HASH 地址过滤;</td><td>0</td></tr><tr><td>1</td><td>HU</td><td>RW</td><td>单播帧过滤模式选择位。
0:进行完美地址过滤;
1:进行 HASH 地址过滤;</td><td>0</td></tr><tr><td>0</td><td>PM</td><td>RW</td><td>混杂模式使能位。所有帧都能通过地址过滤器,且不标记过滤结果。</td><td>0</td></tr></table>

#### 27.1.8.1.3 哈希值列表寄存器（R32_ETH_MACHTHR、R32_ETH_MACHTLR）

偏移地址： $0 \times 0 8$

<table><tr><td>31</td><td>30</td><td>29</td><td>28</td><td>27</td><td>26</td><td>25</td><td>24</td><td>23</td><td>22</td><td>21</td><td>20</td><td>19</td><td>18</td><td>17</td><td>16</td></tr><tr><td colspan="16">HTH</td></tr><tr><td>15</td><td>14</td><td>13</td><td>12</td><td>11</td><td>10</td><td>9</td><td>8</td><td>7</td><td>6</td><td>5</td><td>4</td><td>3</td><td>2</td><td>1</td><td>0</td></tr><tr><td colspan="16">HTH</td></tr></table>

偏移地址： $0 \times 0 0$

<table><tr><td>31</td><td>30</td><td>29</td><td>28</td><td>27</td><td>26</td><td>25</td><td>24</td><td>23</td><td>22</td><td>21</td><td>20</td><td>19</td><td>18</td><td>17</td><td>16</td></tr><tr><td colspan="16">HTL</td></tr><tr><td>15</td><td>14</td><td>13</td><td>12</td><td>11</td><td>10</td><td>9</td><td>8</td><td>7</td><td>6</td><td>5</td><td>4</td><td>3</td><td>2</td><td>1</td><td>0</td></tr><tr><td colspan="16">HTL</td></tr></table>

<table><tr><td>位</td><td>名称</td><td>访问</td><td>描述</td><td>复位值</td></tr><tr><td>[31:0]</td><td>HTH</td><td>RW</td><td>哈希列表高32位</td><td>00000000h</td></tr><tr><td>[31:0]</td><td>HTL</td><td>RW</td><td>哈希列表低32位</td><td>00000000h</td></tr></table>

#### 27.1.8.1.4 MII 地址寄存器（R32_ETH_MACMIIAR）

偏移地址：0x10

<table><tr><td>31</td><td>30</td><td>29</td><td>28</td><td>27</td><td>26</td><td>25</td><td>24</td><td>23</td><td>22</td><td>21</td><td>20</td><td>19</td><td>18</td><td>17</td><td>16</td></tr><tr><td colspan="16">Reserved</td></tr><tr><td>15</td><td>14</td><td>13</td><td>12</td><td>11</td><td>10</td><td>9</td><td>8</td><td>7</td><td>6</td><td>5</td><td>4</td><td>3</td><td>2</td><td>1</td><td>0</td></tr><tr><td colspan="5">PA</td><td colspan="5">MR</td><td>Res</td><td colspan="3">CR</td><td>MW</td><td>MB</td></tr></table>

<table><tr><td>位</td><td>名称</td><td>访问</td><td>描述</td><td>复位值</td></tr><tr><td>[31:16]</td><td>Reserved</td><td>R0</td><td>保留。</td><td>0</td></tr><tr><td>[15:11]</td><td>PA</td><td>RW</td><td>物理层地址域。用户将需要操作的物理层地址写进此域。</td><td>0</td></tr><tr><td>[10:6]</td><td>MR</td><td>RW</td><td>物理层寄存器地址域。用户将需要操作的寄存器地址写进此域。</td><td>0</td></tr><tr><td>5</td><td>Reserved</td><td>R0</td><td>保留。</td><td>0</td></tr><tr><td>[4:2]</td><td>CR</td><td>RW</td><td>时钟范围设定域。用户请保持为000b,使MII频率为主频的42分频。</td><td>0</td></tr><tr><td>1</td><td>MW</td><td>RW</td><td>读写设定位。0:对物理层进行读操作;1:对物理层进行写操作;</td><td>0</td></tr><tr><td>0</td><td>MB</td><td>W1</td><td>MII忙标志位。该位由用户置位,表示命令硬件开始进行读或者写的操作,读写期间应保持物理地址,寄存器地址和数据域不变。在硬件将此位清零后,表示完成操作。</td><td>0</td></tr></table>

#### 27.1.8.1.5 MII 数据寄存器（R32_ETH_MACMIIDR）

偏移地址: $0 \times 1 4$

<table><tr><td>31</td><td>30</td><td>29</td><td>28</td><td>27</td><td>26</td><td>25</td><td>24</td><td>23</td><td>22</td><td>21</td><td>20</td><td>19</td><td>18</td><td>17</td><td>16</td></tr><tr><td colspan="16">Reserved</td></tr><tr><td>15</td><td>14</td><td>13</td><td>12</td><td>11</td><td>10</td><td>9</td><td>8</td><td>7</td><td>6</td><td>5</td><td>4</td><td>3</td><td>2</td><td>1</td><td>0</td></tr><tr><td colspan="16">MD</td></tr></table>

<table><tr><td>位</td><td>名称</td><td>访问</td><td>描述</td><td>复位值</td></tr><tr><td>[31:16]</td><td>Reserved</td><td>R0</td><td>保留。</td><td>0</td></tr><tr><td>[15:0]</td><td>MD</td><td>RW</td><td>MII操作数据域。此域用来存放将要通过MII接口从物理层读取的数据，或者存放向物理层写入的数据。</td><td>0</td></tr></table>

#### 27.1.8.1.6 MAC 流控寄存器（R32_ETH_MACFCR）

偏移地址: $0 \times 1 8$

<table><tr><td>31</td><td>30</td><td>29</td><td>28</td><td>27</td><td>26</td><td>25</td><td>24</td><td>23</td><td>22</td><td>21</td><td>20</td><td>19</td><td>18</td><td>17</td><td>16</td></tr><tr><td colspan="16">PT</td></tr><tr><td>15</td><td>14</td><td>13</td><td>12</td><td>11</td><td>10</td><td>9</td><td>8</td><td>7</td><td>6</td><td>5</td><td>4</td><td>3</td><td>2</td><td>1</td><td>0</td></tr><tr><td colspan="8">Reserved</td><td>ZQPD</td><td>Reser ved</td><td>PLT</td><td>UPFD</td><td>RFCE</td><td>TFCE</td><td colspan="2">FCB</td></tr></table>

<table><tr><td>位</td><td>名称</td><td>访问</td><td>描述</td><td>复位值</td></tr><tr><td>[31:16]</td><td>PT</td><td>RW</td><td>Pause间隔域。此域用来作为控制Pause时间域的值，单位为当前的MII接口发送64字节的耗时。</td><td>0</td></tr><tr><td>[15:8]</td><td>Reserved</td><td>RO</td><td>保留。</td><td>0</td></tr><tr><td>7</td><td>ZQPD</td><td>RW</td><td>关闭零值Pause功能控制位。此位被置位时，将关闭自动零值Pause控制帧的生成。</td><td>0</td></tr><tr><td>6</td><td>Reserved</td><td>RO</td><td>保留。</td><td>0</td></tr><tr><td>[5:4]</td><td>PLT</td><td>RW</td><td>自动重发Pause帧阈值。这个域的值应该小于PT的值。重发时间等于PT减去PLT所代表的时间。00:4个时间间隙;01:28个时间间隙;10:144个时间间隙;</td><td>00b</td></tr><tr><td></td><td></td><td></td><td>11:256个时间间隙;</td><td></td></tr><tr><td>3</td><td>UPFD</td><td>RW</td><td>单播Pause帧检测位。
0:MAC只接收带协议规范定义的唯一地址的Pause帧;
1:MAC同时检测Pause帧是否MAC地址寄存器0中定义的单播地址;</td><td>0</td></tr><tr><td>2</td><td>RFCE</td><td>RW</td><td>接收流控使能位。
0:MAC不解析Pause帧。
1:MAC解析Pause帧并关闭发送器一段时间。</td><td>0</td></tr><tr><td>1</td><td>TFCE</td><td>RW</td><td>发送流控使能位。
0:MAC关闭发送流控,不发送Pause帧;
1:MAC使能发送流控,可以发送Pause帧;</td><td>0</td></tr><tr><td>0</td><td>FCB</td><td>W1</td><td>流控忙标志位。对此位置位可以发送一个Pause帧,发送完成后由硬件清零。在对整个MACFCR寄存器进行操作时,需要保证FCB位为0。</td><td>0</td></tr></table>

#### 27.1.8.1.7 VLAN 标签寄存器（MACVLAN）

偏移地址： ${ 0 } { \times 1 } { 0 }$

<table><tr><td>31</td><td>30</td><td>29</td><td>28</td><td>27</td><td>26</td><td>25</td><td>24</td><td>23</td><td>22</td><td>21</td><td>20</td><td>19</td><td>18</td><td>17</td><td>16</td></tr><tr><td colspan="15">Reserved</td><td>VLANT</td></tr><tr><td>15</td><td>14</td><td>13</td><td>12</td><td>11</td><td>10</td><td>9</td><td>8</td><td>7</td><td>6</td><td>5</td><td>4</td><td>3</td><td>2</td><td>1</td><td>0</td></tr><tr><td colspan="16">VLANTI</td></tr></table>

<table><tr><td>位</td><td>名称</td><td>访问</td><td>描述</td><td>复位值</td></tr><tr><td>[31:17]</td><td>Reserved</td><td>R0</td><td>保留。</td><td>0</td></tr><tr><td>16</td><td>VLANT</td><td>WR</td><td>标签比较控制位。0:将VLAN帧的第15,16字节的全部16位数据都与VLANTI域进行比较;1:只使用VLAN帧的第15,16字节的[11:0]位数据都与VLANTI域的相应位进行比较;</td><td>0</td></tr><tr><td>[15:0]</td><td>VLANTI</td><td>WR</td><td>标签对比样本域。根据IEEE 802.1协议,VLAN帧的第[15:13]位是用户优先级,[12]是规范格式指示符,位[11:0]VLANT标识符域。如果VLANTI全为0,那么MAC将不再关心VLAN帧的第15,16字节,而当其第13,14字节为0x8100(注意大小端)时即判定为VLAN帧。</td><td>0</td></tr></table>

#### 27.1.8.1.8 唤醒帧过滤寄存器（R32_MACRWUFFR）

偏移地址： $_ { 0 \times 2 8 }$

<table><tr><td>31</td><td>30</td><td>29</td><td>28</td><td>27</td><td>26</td><td>25</td><td>24</td><td>23</td><td>22</td><td>21</td><td>20</td><td>19</td><td>18</td><td>17</td><td>16</td></tr><tr><td colspan="16">RWUFFR</td></tr><tr><td>15</td><td>14</td><td>13</td><td>12</td><td>11</td><td>10</td><td>9</td><td>8</td><td>7</td><td>6</td><td>5</td><td>4</td><td>3</td><td>2</td><td>1</td><td>0</td></tr><tr><td colspan="16">RWUFFR</td></tr></table>

<table><tr><td>位</td><td>名称</td><td>访问</td><td>描述</td><td>复位值</td></tr><tr><td>[31:0]</td><td>RWUFFR</td><td>RW</td><td>唤醒帧过滤寄存器实际上时八个不同的寄存器,对其进行连续八次的读操作可以读出全部寄存器,同样对其进行连续八次的写操作可以写进全部八个寄存器。这八个寄存器各个位的说明详见14.5.6.3节的描述。</td><td>0</td></tr></table>

#### 27.1.8.1.9 PMT 控制和状态寄存器（R32_ETH_MACPMTCSR）

偏移地址： $\mathtt { 0 } \mathtt { x } 2 0$

<table><tr><td>31</td><td>30</td><td>29</td><td>28</td><td>27</td><td>26</td><td>25</td><td>24</td><td>23</td><td>22</td><td>21</td><td>20</td><td>19</td><td>18</td><td>17</td><td>16</td></tr><tr><td>WFFRP</td><td colspan="15">Reserved</td></tr><tr><td>15</td><td>14</td><td>13</td><td>12</td><td>11</td><td>10</td><td>9</td><td>8</td><td>7</td><td>6</td><td>5</td><td>4</td><td>3</td><td>2</td><td>1</td><td>0</td></tr><tr><td></td><td colspan="5">Reserved</td><td>GU</td><td colspan="2">Reserved</td><td>WFR</td><td>MPR</td><td colspan="2">Reserved</td><td>WFE</td><td>MPE</td><td>PD</td></tr></table>

<table><tr><td>位</td><td>名称</td><td>访问</td><td>描述</td><td>复位值</td></tr><tr><td>31</td><td>WFFRP</td><td>W</td><td>寄存器复位控制位。置此位会将 PMTCSR 寄存器全部清零。此位会在一个系统周期自动清零。</td><td>0</td></tr><tr><td>[30:10]</td><td>Reserved</td><td>R0</td><td>保留。</td><td>0</td></tr><tr><td>9</td><td>GU</td><td>RW</td><td>全局单播位。置此位会使 MAC 将所有通过过滤器的单播帧都认为是唤醒帧。</td><td>0</td></tr><tr><td>[8:7]</td><td>Reserved</td><td>R0</td><td>保留。</td><td>0</td></tr><tr><td>6</td><td>WFR</td><td>RZ</td><td>唤醒帧接收标志位。当收到唤醒帧时,此位会被置位。读取自动清零。</td><td>0</td></tr><tr><td>5</td><td>MPR</td><td>RZ</td><td>魔法帧接收标志位。当收到魔法帧时,此位会被置位。读取自动清零。</td><td>0</td></tr><tr><td>[4:3]</td><td>Reserved</td><td>R0</td><td>保留。</td><td>0</td></tr><tr><td>2</td><td>WFE</td><td>RW</td><td>唤醒帧使能位。置此位允许在接收到唤醒帧时产生PMT 事件。</td><td>0</td></tr><tr><td>1</td><td>MPE</td><td>RW</td><td>魔法帧使能位。置此位允许在接收到魔法帧时产生PMT 事件。</td><td>0</td></tr><tr><td>0</td><td>PD</td><td>W</td><td>掉电控制位。置此位会使 MAC 进入掉电模式:丢弃其他所有帧,直到其收到了唤醒帧或魔法帧,唤醒后 MAC 自动清零。在置此位前需要将 WFE 或者 MPE 置位。</td><td>0</td></tr></table>

#### 27.1.8.1.10 MAC 中断状态寄存器（R32_ETH_MACSR）

偏移地址： ${ 0 } { \times } \ 3 { 8 }$

<table><tr><td>31</td><td>30</td><td>29</td><td>28</td><td>27</td><td>26</td><td>25</td><td>24</td><td>23</td><td>22</td><td>21</td><td>20</td><td>19</td><td>18</td><td>17</td><td>16</td></tr><tr><td colspan="16">Reserved</td></tr><tr><td>15</td><td>14</td><td>13</td><td>12</td><td>11</td><td>10</td><td>9</td><td>8</td><td>7</td><td>6</td><td>5</td><td>4</td><td>3</td><td>2</td><td>1</td><td>0</td></tr><tr><td colspan="5">Reserved</td><td>TSTS</td><td colspan="2">Reserved</td><td>MMCTS</td><td>MMCRS</td><td>MMCS</td><td>PMTS</td><td colspan="4">Reserved</td></tr></table>

<table><tr><td>位</td><td>名称</td><td>访问</td><td>描述</td><td>复位值</td></tr><tr><td>[31:10]</td><td>Reserved</td><td>R0</td><td>保留。</td><td>0</td></tr><tr><td>9</td><td>TSTS</td><td>RZ</td><td>时间戳触发中断标志位。当到达PTP系统时钟设定的时间时,此标志位会被置位。</td><td>0</td></tr><tr><td>[8:7]</td><td>Reserved</td><td>R0</td><td>保留。</td><td>0</td></tr><tr><td>6</td><td>MMCTS</td><td>R</td><td>MMC发送中断标志位。当MMC寄存器组中的MMCTIR寄存器产生任一中断时,该位置位。当MMC寄存器组中的MMCTIR寄存器全部清零时,此位也即清零。</td><td>0</td></tr><tr><td>5</td><td>MMCRS</td><td>R</td><td>MMC接收中断标志位。当MMC寄存器组中的MMCRIR寄存器产生任一中断时,该位置位。当MMC寄存器组中的MMCRIR寄存器全部清零时,此位也即清零。</td><td>0</td></tr><tr><td>4</td><td>MMCS</td><td>R</td><td>MMC状态标志位。当MMCTS或MMCRS置位时会触发此位置位;当MMCTS、MMCRS全部清零时此位即清零。</td><td>0</td></tr><tr><td>3</td><td>PMTS</td><td>R</td><td>PMT状态标志位。在掉电模式下,如果MAC收到魔法帧或者唤醒帧唤醒了MAC,那么此位会被置位;清除WFR和MPR两位后此位被清零。</td><td>0</td></tr><tr><td>[2:0]</td><td>Reserved</td><td>R0</td><td>保留。</td><td>0</td></tr></table>

#### 27.1.8.1.11 MAC 中断屏蔽寄存器（R32_ETH_MACIMR）

偏移地址： $\mathtt { 0 } \mathtt { x } \mathtt { 3 0 }$

<table><tr><td>31</td><td>30</td><td>29</td><td>28</td><td>27</td><td>26</td><td>25</td><td>24</td><td>23</td><td>22</td><td>21</td><td>20</td><td>19</td><td>18</td><td>17</td><td>16</td></tr><tr><td colspan="16">Reserved</td></tr><tr><td>15</td><td>14</td><td>13</td><td>12</td><td>11</td><td>10</td><td>9</td><td>8</td><td>7</td><td>6</td><td>5</td><td>4</td><td>3</td><td>2</td><td>1</td><td>0</td></tr><tr><td colspan="6">Reserved</td><td>TSTIM</td><td colspan="5">Reserved</td><td>PMTIM</td><td colspan="3">Reserved</td></tr></table>

<table><tr><td>位</td><td>名称</td><td>访问</td><td>描述</td><td>复位值</td></tr><tr><td>[31:10]</td><td>Reserved</td><td>R0</td><td>保留。</td><td>0</td></tr><tr><td>9</td><td>TSTIM</td><td>RW</td><td>时间戳中断屏蔽位。置此位将禁止产生时间戳中断。</td><td>0</td></tr><tr><td>[8:4]</td><td>Reserved</td><td>R0</td><td>保留。</td><td>0</td></tr><tr><td>3</td><td>PMTIM</td><td>RW</td><td>PMT中断屏蔽位。置此位将屏蔽PMT中断。</td><td>0</td></tr><tr><td>[2:0]</td><td>Reserved</td><td>R0</td><td>保留。</td><td>0</td></tr></table>

#### 27.1.8.1.12 MAC 地址寄存器 0 高 32 位（R32_ETH_MACA0HR）

偏移地址： $0 \times 4 0$

<table><tr><td>31</td><td>30</td><td>29</td><td>28</td><td>27</td><td>26</td><td>25</td><td>24</td><td>23</td><td>22</td><td>21</td><td>20</td><td>19</td><td>18</td><td>17</td><td>16</td></tr><tr><td>MO</td><td colspan="15">Reserved</td></tr><tr><td>15</td><td>14</td><td>13</td><td>12</td><td>11</td><td>10</td><td>9</td><td>8</td><td>7</td><td>6</td><td>5</td><td>4</td><td>3</td><td>2</td><td>1</td><td>0</td></tr><tr><td colspan="16">MACAOH</td></tr></table>

<table><tr><td>位</td><td>名称</td><td>访问</td><td>描述</td><td>复位值</td></tr><tr><td>31</td><td>M0</td><td>R0</td><td>总是为1。</td><td>1</td></tr><tr><td>[30:16]</td><td>Reserved</td><td>R0</td><td>保留。</td><td>0</td></tr><tr><td>[15:0]</td><td>MACAOH</td><td>RW</td><td>MAC地址的高16位，即47:32位。</td><td>FFFFH</td></tr></table>

#### 27.1.8.1.13 MAC 地址寄存器 0 低 32 位（R32_ETH_MACA0LR）

偏移地址： $0 \times 4 4$

<table><tr><td>31</td><td>30</td><td>29</td><td>28</td><td>27</td><td>26</td><td>25</td><td>24</td><td>23</td><td>22</td><td>21</td><td>20</td><td>19</td><td>18</td><td>17</td><td>16</td></tr><tr><td colspan="16">MACAOL</td></tr><tr><td>15</td><td>14</td><td>13</td><td>12</td><td>11</td><td>10</td><td>9</td><td>8</td><td>7</td><td>6</td><td>5</td><td>4</td><td>3</td><td>2</td><td>1</td><td>0</td></tr><tr><td colspan="16">MACAOL</td></tr></table>

<table><tr><td>位</td><td>名称</td><td>访问</td><td>描述</td><td>复位值</td></tr><tr><td>[31:0]</td><td>MACAOL</td><td>RW</td><td>MAC 地址的低 32 位，即 31:0 位。一般情况下，MAC 地址寄存器 0 的地址是 MAC 本身的地址。</td><td>FFFFFF
FFh</td></tr></table>

#### 27.1.8.1.14 MAC 地址寄存器 1 高 32 位（R32_ETH_MACA1HR）

偏移地址： $0 \times 4 8$

<table><tr><td>31</td><td>30</td><td>29</td><td>28</td><td>27</td><td>26</td><td>25</td><td>24</td><td>23</td><td>22</td><td>21</td><td>20</td><td>19</td><td>18</td><td>17</td><td>16</td></tr><tr><td>AE</td><td>SA</td><td colspan="6">MBC</td><td colspan="8">Reserved</td></tr><tr><td>15</td><td>14</td><td>13</td><td>12</td><td>11</td><td>10</td><td>9</td><td>8</td><td>7</td><td>6</td><td>5</td><td>4</td><td>3</td><td>2</td><td>1</td><td>0</td></tr><tr><td colspan="16">MACA1H</td></tr></table>

<table><tr><td>位</td><td>名称</td><td>访问</td><td>描述</td><td>复位值</td></tr><tr><td>31</td><td>AE</td><td>RW</td><td>地址过滤使能位。0:地址过滤器忽略MAC地址1;1:地址过滤器使用MAC地址1来进行完美过滤;</td><td>0</td></tr><tr><td>30</td><td>SA</td><td>RW</td><td>地址角色选择位。0:MAC地址1被用来和接收到的帧的目标地址对比;1:MAC地址1被用来和接收到的帧的源地址对比;</td><td>0</td></tr><tr><td>[29:24]</td><td>MBC</td><td>RW</td><td>屏蔽字控制域。MBC的各个位用来对应屏蔽MAC地址1的某个字节,用来禁止将MAC地址1的某个字节参与到接收帧的源地址或目标地址的比较中。R32 ETH_MACA1HR[29]置位则屏蔽MACA1H[15:8];R32 ETH_MACA1HR[28]置位则屏蔽MACA1H[7:0];R32 ETH_MACA1HR[27]置位则屏蔽MACA1L[31:24];R32 ETH_MACA1HR[26]置位则屏蔽MACA1L[23:16];R32 ETH_MACA1HR[25]置位则屏蔽MACA1L[15:8];R32 ETH_MACA1HR[24]置位则屏蔽MACA1L[7:0]。</td><td>0</td></tr><tr><td>[23:16]</td><td>Reserved</td><td>R0</td><td>保留。</td><td>0</td></tr><tr><td>[15:0]</td><td>MACA1H</td><td>RW</td><td>MAC 地址的高 16 位，即 47:32 位。</td><td>FFFFH</td></tr></table>

#### 27.1.8.1.15 MAC 地址寄存器 1 低 32 位（R32_ETH_MACA1LR）

偏移地址： $\mathtt { 0 } \mathtt { x } 4 0$

<table><tr><td>31</td><td>30</td><td>29</td><td>28</td><td>27</td><td>26</td><td>25</td><td>24</td><td>23</td><td>22</td><td>21</td><td>20</td><td>19</td><td>18</td><td>17</td><td>16</td></tr><tr><td colspan="16">MACA1L</td></tr><tr><td>15</td><td>14</td><td>13</td><td>12</td><td>11</td><td>10</td><td>9</td><td>8</td><td>7</td><td>6</td><td>5</td><td>4</td><td>3</td><td>2</td><td>1</td><td>0</td></tr><tr><td colspan="16">MACA1L</td></tr></table>

<table><tr><td>位</td><td>名称</td><td>访问</td><td>描述</td><td>复位值</td></tr><tr><td>[31:0]</td><td>MACA1L</td><td>RW</td><td>MAC 地址的低 32 位，即 31:0 位。</td><td>FFFFFF
FFh</td></tr></table>

#### 27.1.8.1.16 MAC 地址寄存器 2 高 32 位（R32_ETH_MACA2HR）

偏移地址： $_ { 0 \times 5 0 }$

<table><tr><td>31</td><td>30</td><td>29</td><td>28</td><td>27</td><td>26</td><td>25</td><td>24</td><td>23</td><td>22</td><td>21</td><td>20</td><td>19</td><td>18</td><td>17</td><td>16</td></tr><tr><td>AE</td><td>SA</td><td colspan="6">MBC</td><td colspan="8">Reserved</td></tr><tr><td>15</td><td>14</td><td>13</td><td>12</td><td>11</td><td>10</td><td>9</td><td>8</td><td>7</td><td>6</td><td>5</td><td>4</td><td>3</td><td>2</td><td>1</td><td>0</td></tr><tr><td colspan="16">MACA2H</td></tr></table>

<table><tr><td>位</td><td>名称</td><td>访问</td><td>描述</td><td>复位值</td></tr><tr><td>31</td><td>AE</td><td>RW</td><td>地址过滤使能位。0:地址过滤器忽略MAC地址2;1:地址过滤器使用MAC地址2来进行完美过滤;</td><td>0</td></tr><tr><td>30</td><td>SA</td><td>RW</td><td>地址角色选择位。0:MAC地址2被用来和接收到的帧的目标地址对比;1:MAC地址2被用来和接收到的帧的源地址对比;</td><td>0</td></tr><tr><td>[29:24]</td><td>MBC</td><td>RW</td><td>屏蔽字控制域。MBC的各个位用来对应屏蔽MAC地址2的某个字节,用来禁止将MAC地址2的某个字节参与到接收帧的源地址或目标地址的比较中。R32 ETH_MACA2HR[29]置位则屏蔽MACA2H[15:8];R32 ETH_MACA2HR[28]置位则屏蔽MACA2H[7:0];R32 ETH_MACA2HR[27]置位则屏蔽MACA2L[31:24];R32 ETH_MACA2HR[26]置位则屏蔽MACA2L[23:16];R32 ETH_MACA2HR[25]置位则屏蔽MACA2L[15:8];R32 ETH_MACA2HR[24]置位则屏蔽MACA2L[7:0]。</td><td>0</td></tr><tr><td>[23:16]</td><td>Reserved</td><td>RO</td><td>保留。</td><td>0</td></tr><tr><td>[15:0]</td><td>MACA2H</td><td>RW</td><td>MAC地址的高16位,即47:32位。</td><td>FFFFh</td></tr></table>

#### 27.1.8.1.17 MAC 地址寄存器 2 低 32 位（R32_ETH_MACA2LR）

偏移地址： $0 \times 5 4$

<table><tr><td>31</td><td>30</td><td>29</td><td>28</td><td>27</td><td>26</td><td>25</td><td>24</td><td>23</td><td>22</td><td>21</td><td>20</td><td>19</td><td>18</td><td>17</td><td>16</td></tr><tr><td colspan="16">MACA2L</td></tr><tr><td>15</td><td>14</td><td>13</td><td>12</td><td>11</td><td>10</td><td>9</td><td>8</td><td>7</td><td>6</td><td>5</td><td>4</td><td>3</td><td>2</td><td>1</td><td>0</td></tr><tr><td colspan="16">MACA2L</td></tr></table>

<table><tr><td>位</td><td>名称</td><td>访问</td><td>描述</td><td>复位值</td></tr><tr><td>31:0</td><td>MACA2L</td><td>RW</td><td>MAC 地址的低 32 位，即 31:0 位。</td><td>FFFFFF
FFh</td></tr></table>

#### 27.1.8.1.18 MAC 地址寄存器 3 高 32 位（R32_ETH_MACA3HR）

偏移地址： $_ { 0 \times 5 8 }$

<table><tr><td>31</td><td>30</td><td>29</td><td>28</td><td>27</td><td>26</td><td>25</td><td>24</td><td>23</td><td>22</td><td>21</td><td>20</td><td>19</td><td>18</td><td>17</td><td>16</td></tr><tr><td>AE</td><td>SA</td><td colspan="6">MBC</td><td colspan="8">Reserved</td></tr><tr><td>15</td><td>14</td><td>13</td><td>12</td><td>11</td><td>10</td><td>9</td><td>8</td><td>7</td><td>6</td><td>5</td><td>4</td><td>3</td><td>2</td><td>1</td><td>0</td></tr><tr><td colspan="16">MACA3H</td></tr></table>

<table><tr><td>位</td><td>名称</td><td>访问</td><td>描述</td><td>复位值</td></tr><tr><td>31</td><td>AE</td><td>RW</td><td>地址过滤使能位。0:地址过滤器忽略MAC地址3;1:地址过滤器使用MAC地址3来进行完美过滤;</td><td>0</td></tr><tr><td>30</td><td>SA</td><td>RW</td><td>地址角色选择位。0:MAC地址3被用来和接收到的帧的目标地址对比;1:MAC地址3被用来和接收到的帧的源地址对比;</td><td>0</td></tr><tr><td>[29:24]</td><td>MBC</td><td>RW</td><td>屏蔽字控制域。MBC的各个位用来对应屏蔽MAC地址3的某个字节,用来禁止将MAC地址3的某个字节参与到接收帧的源地址或目标地址的比较中。R32 ETH_MACA3HR[29]置位则屏蔽MACA3H[15:8];R32 ETH_MACA3HR[28]置位则屏蔽MACA3H[7:0];R32 ETH_MACA3HR[27]置位则屏蔽MACA2L[31:24];R32 ETH_MACA3HR[26]置位则屏蔽MACA2L[23:16];R32 ETH_MACA3HR[25]置位则屏蔽MACA3L[15:8];R32 ETH_MACA3HR[24]置位则屏蔽MACA3L[7:0]。</td><td>0</td></tr><tr><td>[23:16]</td><td>Reserved</td><td>RO</td><td>保留。</td><td>0</td></tr><tr><td>15:0</td><td>MACA3H</td><td>RW</td><td>MAC地址的高16位,即47:32位。</td><td>FFFFFFFFh</td></tr></table>

#### 27.1.8.1.19 MAC 地址寄存器 3 低 32 位（R32_ETH_MACA3LR）

偏移地址： $_ { 0 \times 5 0 }$

<table><tr><td>31</td><td>30</td><td>29</td><td>28</td><td>27</td><td>26</td><td>25</td><td>24</td><td>23</td><td>22</td><td>21</td><td>20</td><td>19</td><td>18</td><td>17</td><td>16</td></tr><tr><td colspan="16">MACA3L</td></tr><tr><td>15</td><td>14</td><td>13</td><td>12</td><td>11</td><td>10</td><td>9</td><td>8</td><td>7</td><td>6</td><td>5</td><td>4</td><td>3</td><td>2</td><td>1</td><td>0</td></tr><tr><td colspan="16">MACA3L</td></tr></table>

<table><tr><td>位</td><td>名称</td><td>访问</td><td>描述</td><td>复位值</td></tr><tr><td>[31:0]</td><td>MACA3L</td><td>RW</td><td>MAC 地址的低 32 位，即 31:0 位。</td><td>FFFFFF
FFh</td></tr></table>

### 27.1.8.2 MMC 相关寄存器各位域

#### 27.1.8.2.1 MMC 控制寄存器（R32_ETH_MMCCR）

偏移地址： $0 \times 0 1 0 0$

<table><tr><td>31</td><td>30</td><td>29</td><td>28</td><td>27</td><td>26</td><td>25</td><td>24</td><td>23</td><td>22</td><td>21</td><td>20</td><td>19</td><td>18</td><td>17</td><td>16</td></tr><tr><td colspan="16">Reserved</td></tr><tr><td>15</td><td>14</td><td>13</td><td>12</td><td>11</td><td>10</td><td>9</td><td>8</td><td>7</td><td>6</td><td>5</td><td>4</td><td>3</td><td>2</td><td>1</td><td>0</td></tr><tr><td colspan="12">Reserved</td><td>MCF</td><td>ROR</td><td>CSR</td><td>CR</td></tr></table>

<table><tr><td>位</td><td>名称</td><td>访问</td><td>描述</td><td>复位值</td></tr><tr><td>[31:4]</td><td>Reserved</td><td>R0</td><td>保留。</td><td>0</td></tr><tr><td>3</td><td>MCF</td><td>RW</td><td>计数器冻结控制位。置此位会将 MMC 所有的计数器值全部冻结。复此位即恢复各计数器计数。在冻结时置位 ROR 再读取任意计数器,会导致该计数器清零。</td><td>0</td></tr><tr><td>2</td><td>ROR</td><td>RW</td><td>读时复位控制位。置此位会使读取任一计数器之后清零计数器的值。</td><td>0</td></tr><tr><td>1</td><td>CSR</td><td>RW</td><td>计数器回转停止位。置此位会使计数器增正到最大值后停止而不自动归零。</td><td>0</td></tr><tr><td>0</td><td>CR</td><td>W</td><td>计数器复位控制位。置此位将复位全部计数器。此位在一个系统周期后将自动清零。</td><td>0</td></tr></table>

#### 27.1.8.2.2 MMC 接收中断寄存器（R32_ETH_MMRIR）

偏移地址： $0 \times 0 1 0 4$

<table><tr><td>31</td><td>30</td><td>29</td><td>28</td><td>27</td><td>26</td><td>25</td><td>24</td><td>23</td><td>22</td><td>21</td><td>20</td><td>19</td><td>18</td><td>17</td><td>16</td></tr><tr><td colspan="14">Reserved</td><td>RGUFS</td><td>Reser ved</td></tr><tr><td>15</td><td>14</td><td>13</td><td>12</td><td>11</td><td>10</td><td>9</td><td>8</td><td>7</td><td>6</td><td>5</td><td>4</td><td>3</td><td>2</td><td>1</td><td>0</td></tr><tr><td colspan="10">Reserved</td><td>RFCES</td><td colspan="5">Reserved</td></tr></table>

<table><tr><td>位</td><td>名称</td><td>访问</td><td>描述</td><td>复位值</td></tr><tr><td>[31:18]</td><td>Reserved</td><td>R0</td><td>保留。</td><td>0</td></tr><tr><td>17</td><td>RGUFS</td><td>RZ</td><td>接收好帧数量过半时会置此位。</td><td>0</td></tr><tr><td>[16:6]</td><td>Reserved</td><td>R0</td><td>保留。</td><td>0</td></tr><tr><td>5</td><td>RFCES</td><td>RZ</td><td>接收CRC校验错误的帧数量过半时会置此位。</td><td>0</td></tr><tr><td>[4:0]</td><td>Reserved</td><td>R0</td><td>保留。</td><td>0</td></tr></table>

#### 27.1.8.2.3 MMC 发送中断寄存器（R32_ETH_MMCTIR）

偏移地址： $0 \times 0 1 0 8$

<table><tr><td>31</td><td>30</td><td>29</td><td>28</td><td>27</td><td>26</td><td>25</td><td>24</td><td>23</td><td>22</td><td>21</td><td>20</td><td>19</td><td>18</td><td>17</td><td>16</td></tr><tr><td colspan="10">Reserved</td><td>TGFS</td><td colspan="5">Reserved</td></tr><tr><td>15</td><td>14</td><td>13</td><td>12</td><td>11</td><td>10</td><td>9</td><td>8</td><td>7</td><td>6</td><td>5</td><td>4</td><td>3</td><td>2</td><td>1</td><td>0</td></tr><tr><td colspan="16">Reserved</td></tr></table>

<table><tr><td>位</td><td>名称</td><td>访问</td><td>描述</td><td>复位值</td></tr><tr><td>[31:22]</td><td>Reserved</td><td>R0</td><td>保留。</td><td>0</td></tr><tr><td>21</td><td>TGFS</td><td>RZ</td><td>发送帧数过半时此位会置位。</td><td>0</td></tr><tr><td>[20:0]</td><td>Reserved</td><td>R0</td><td>保留。</td><td>0</td></tr></table>

#### 27.1.8.2.4 MMC 接收中断屏蔽寄存器（R32_ETH_MMRIMR）

偏移地址： $0 \times 0 1 0 0$

<table><tr><td>31</td><td>30</td><td>29</td><td>28</td><td>27</td><td>26</td><td>25</td><td>24</td><td>23</td><td>22</td><td>21</td><td>20</td><td>19</td><td>18</td><td>17</td><td>16</td></tr><tr><td colspan="14">Reserved</td><td>FGUFM</td><td>Reser ved</td></tr><tr><td>15</td><td>14</td><td>13</td><td>12</td><td>11</td><td>10</td><td>9</td><td>8</td><td>7</td><td>6</td><td>5</td><td>4</td><td>3</td><td>2</td><td>1</td><td>0</td></tr><tr><td colspan="10">Reserved</td><td>FRCRM</td><td colspan="5">Reserved</td></tr></table>

<table><tr><td>位</td><td>名称</td><td>访问</td><td>描述</td><td>复位值</td></tr><tr><td>[31:18]</td><td>Reserved</td><td>R0</td><td>保留。</td><td>0</td></tr><tr><td>17</td><td>FGUFM</td><td>RW</td><td>接收好帧过半中断屏蔽位。置此位将屏蔽接收好帧计数器值达到一半时产生的中断。</td><td>0</td></tr><tr><td>[16:6]</td><td>Reserved</td><td>R0</td><td>保留。</td><td>0</td></tr><tr><td>5</td><td>FRCRM</td><td>RW</td><td>接收CRC错误帧过半中断屏蔽位。置此位将屏蔽接收到CRC错误的帧计数器值达到一半时产生的中断。</td><td>0</td></tr><tr><td>[4:0]</td><td>Reserved</td><td>R0</td><td>保留。</td><td>0</td></tr></table>

#### 27.1.8.2.5 MMC 发送中断屏蔽寄存器（R32_ETH_MMCTIMR）

偏移地址： $0 \times 0 1 1 0$

<table><tr><td>31</td><td>30</td><td>29</td><td>28</td><td>27</td><td>26</td><td>25</td><td>24</td><td>23</td><td>22</td><td>21</td><td>20</td><td>19</td><td>18</td><td>17</td><td>16</td></tr><tr><td colspan="10">Reserved</td><td>TGFM</td><td colspan="5">Reserved</td></tr><tr><td>15</td><td>14</td><td>13</td><td>12</td><td>11</td><td>10</td><td>9</td><td>8</td><td>7</td><td>6</td><td>5</td><td>4</td><td>3</td><td>2</td><td>1</td><td>0</td></tr><tr><td colspan="16">Reserved</td></tr></table>

<table><tr><td>位</td><td>名称</td><td>访问</td><td>描述</td><td>复位值</td></tr><tr><td>[31:22]</td><td>Reserved</td><td>R0</td><td>保留。</td><td>0</td></tr><tr><td>21</td><td>TGFM</td><td>RW</td><td>发送好帧过半中断屏蔽位。置此位将屏蔽发送好帧计数器值达到一半时产生的中断。</td><td>0</td></tr><tr><td>[20:0]</td><td>Reserved</td><td>R0</td><td>保留。</td><td>0</td></tr></table>

#### 27.1.8.2.6 MMC 一次冲突后发生好帧计数器（R32_ETH_MMCTGFSCCR）

偏移地址： $0 \times 0 1 4 0$

<table><tr><td>31</td><td>30</td><td>29</td><td>28</td><td>27</td><td>26</td><td>25</td><td>24</td><td>23</td><td>22</td><td>21</td><td>20</td><td>19</td><td>18</td><td>17</td><td>16</td></tr><tr><td colspan="16">TGFSCCR</td></tr><tr><td>15</td><td>14</td><td>13</td><td>12</td><td>11</td><td>10</td><td>9</td><td>8</td><td>7</td><td>6</td><td>5</td><td>4</td><td>3</td><td>2</td><td>1</td><td>0</td></tr><tr><td colspan="16">TGFSCCR</td></tr></table>

<table><tr><td>位</td><td>名称</td><td>访问</td><td>描述</td><td>复位值</td></tr><tr><td>[31:0]</td><td>TGFSCCR</td><td>R0</td><td>这个域用来统计半双工模式下，发生帧时只遇到一次冲突的计数器。</td><td>0</td></tr></table>

#### 27.1.8.2.7 MMC 多次冲突后发生好帧计数器（R32_ETH_MMCTGFMSCCR）

偏移地址： $0 \times 0 1 5 0$

<table><tr><td>31</td><td>30</td><td>29</td><td>28</td><td>27</td><td>26</td><td>25</td><td>24</td><td>23</td><td>22</td><td>21</td><td>20</td><td>19</td><td>18</td><td>17</td><td>16</td></tr><tr><td colspan="16">TGFMSCCR</td></tr><tr><td>15</td><td>14</td><td>13</td><td>12</td><td>11</td><td>10</td><td>9</td><td>8</td><td>7</td><td>6</td><td>5</td><td>4</td><td>3</td><td>2</td><td>1</td><td>0</td></tr><tr><td colspan="16">TGFMSCCR</td></tr></table>

<table><tr><td>位</td><td>名称</td><td>访问</td><td>描述</td><td>复位值</td></tr><tr><td>[31:0]</td><td>TGFMSCCR</td><td>R0</td><td>这个域用来统计半双工模式下，发生帧时只遇到一次以上冲突的计数器。</td><td>0</td></tr></table>

#### 27.1.8.2.8 MMC 发送好帧计数寄存器（R32_ETH_MMCTGFCR）

偏移地址： $0 \times 0 1 6 8$

<table><tr><td>31</td><td>30</td><td>29</td><td>28</td><td>27</td><td>26</td><td>25</td><td>24</td><td>23</td><td>22</td><td>21</td><td>20</td><td>19</td><td>18</td><td>17</td><td>16</td></tr><tr><td colspan="16">TGFC</td></tr><tr><td>15</td><td>14</td><td>13</td><td>12</td><td>11</td><td>10</td><td>9</td><td>8</td><td>7</td><td>6</td><td>5</td><td>4</td><td>3</td><td>2</td><td>1</td><td>0</td></tr><tr><td colspan="16">TGFC</td></tr></table>

<table><tr><td>位</td><td>名称</td><td>访问</td><td>描述</td><td>复位值</td></tr><tr><td>[31:0]</td><td>TGFC</td><td>RO</td><td>MAC发送出去的正确帧的计数量。</td><td>0</td></tr></table>

#### 27.1.8.2.9 MMC 接收 CRC 有误帧计数寄存器（R32_ETH_MMCRFCECR）

偏移地址： $0 \times 0 1 9 4$

<table><tr><td>31</td><td>30</td><td>29</td><td>28</td><td>27</td><td>26</td><td>25</td><td>24</td><td>23</td><td>22</td><td>21</td><td>20</td><td>19</td><td>18</td><td>17</td><td>16</td></tr><tr><td colspan="16">RFCECR</td></tr><tr><td>15</td><td>14</td><td>13</td><td>12</td><td>11</td><td>10</td><td>9</td><td>8</td><td>7</td><td>6</td><td>5</td><td>4</td><td>3</td><td>2</td><td>1</td><td>0</td></tr><tr><td colspan="16">RFCECR</td></tr></table>

<table><tr><td>位</td><td>名称</td><td>访问</td><td>描述</td><td>复位值</td></tr><tr><td>[31:0]</td><td>RFCECR</td><td>RO</td><td>MAC接收到的存在CRC校验错误的帧的计数器。</td><td>0</td></tr></table>

#### 27.1.8.2.10 MMC 接收对齐错误计数器寄存器（R32_ETH_MMCRFAECR）

偏移地址： $0 \times 0 1 9 8$

<table><tr><td>31</td><td>30</td><td>29</td><td>28</td><td>27</td><td>26</td><td>25</td><td>24</td><td>23</td><td>22</td><td>21</td><td>20</td><td>19</td><td>18</td><td>17</td><td>16</td></tr><tr><td colspan="16">RFAECR</td></tr><tr><td>15</td><td>14</td><td>13</td><td>12</td><td>11</td><td>10</td><td>9</td><td>8</td><td>7</td><td>6</td><td>5</td><td>4</td><td>3</td><td>2</td><td>1</td><td>0</td></tr><tr><td colspan="16">RFAECR</td></tr></table>

<table><tr><td>位</td><td>名称</td><td>访问</td><td>描述</td><td>复位值</td></tr><tr><td>[31:0]</td><td>RFAECR</td><td>RO</td><td>MAC接收到的存在对齐错误的帧的计数器。</td><td>0</td></tr></table>

#### 27.1.8.2.11 MMC 接收好帧计数寄存器（R32_ETH_MMCRGUFCR）

偏移地址： $0 { \times } 0 1 0 4$

<table><tr><td>31</td><td>30</td><td>29</td><td>28</td><td>27</td><td>26</td><td>25</td><td>24</td><td>23</td><td>22</td><td>21</td><td>20</td><td>19</td><td>18</td><td>17</td><td>16</td></tr><tr><td colspan="16">RGUFCR</td></tr><tr><td>15</td><td>14</td><td>13</td><td>12</td><td>11</td><td>10</td><td>9</td><td>8</td><td>7</td><td>6</td><td>5</td><td>4</td><td>3</td><td>2</td><td>1</td><td>0</td></tr><tr><td colspan="16">RGUFCR</td></tr></table>

<table><tr><td>位</td><td>名称</td><td>访问</td><td>描述</td><td>复位值</td></tr><tr><td>[31:0]</td><td>RGUFCR</td><td>RO</td><td>MAC 接收到的正常的帧的数量。</td><td>0</td></tr></table>

### 27.1.8.3 IEEE 1588（PTP）相关寄存器各位域

#### 27.1.8.3.1 PTP 时间戳控制寄存器（R32_ETH_PTPTSCR）

偏移地址： $0 \times 0 7 0 0$

<table><tr><td>31</td><td>30</td><td>29</td><td>28</td><td>27</td><td>26</td><td>25</td><td>24</td><td>23</td><td>22</td><td>21</td><td>20</td><td>19</td><td>18</td><td>17</td><td>16</td></tr><tr><td colspan="16">Reserved</td></tr><tr><td>15</td><td>14</td><td>13</td><td>12</td><td>11</td><td>10</td><td>9</td><td>8</td><td>7</td><td>6</td><td>5</td><td>4</td><td>3</td><td>2</td><td>1</td><td>0</td></tr></table>

<table><tr><td>Reserved</td><td>TSARU</td><td>TSITE</td><td>TSSTU</td><td>TSSTI</td><td>TSFCU</td><td>TSE</td></tr></table>

<table><tr><td>位</td><td>名称</td><td>访问</td><td>描述</td><td>复位值</td></tr><tr><td>[31:6]</td><td>Reserved</td><td>R0</td><td>保留。</td><td>0</td></tr><tr><td>5</td><td>TSARU</td><td>RW</td><td>加数寄存器更新控制位。置此位后,加数寄存器的值将会加到累加器中。精调模式下使用此位。累加器自增完毕后此位自动清零。此位只能在为0时置位。</td><td>0</td></tr><tr><td>4</td><td>TSITE</td><td>RW</td><td>时间戳中断触发使能位。置此位后,当PTP系统时间达到设定目标时间寄存器的值时,将产生中断。</td><td>0</td></tr><tr><td>3</td><td>TSSTU</td><td>RW</td><td>系统时间更新控制位。置此位后,PTP系统时间将加上更新寄存器中的值。更新完毕后此位自动清零。</td><td>0</td></tr><tr><td>2</td><td>TSSTI</td><td>RW</td><td>时间戳初始化控制位。置此位后,PTP的系统时间将会被替换成更新寄存器中的值。更新完毕后此位自动清零。</td><td>0</td></tr><tr><td>1</td><td>TSFCU</td><td>RW</td><td>更新模式选择控制位。0:表示使用粗调模式;1:表示使用精调模式;</td><td>0</td></tr><tr><td>0</td><td>TSE</td><td>RW</td><td>附加时间戳使能位。0:不向描述符中添加时间戳;1:发送或接收完成后,向描述符中添加时间戳;</td><td>0</td></tr></table>

#### 27.1.8.3.2 PTP 亚秒递增寄存器（R32_ETH_PTPSSIR）

偏移地址： $0 { \times } 0 7 0 4$

<table><tr><td>31</td><td>30</td><td>29</td><td>28</td><td>27</td><td>26</td><td>25</td><td>24</td><td>23</td><td>22</td><td>21</td><td>20</td><td>19</td><td>18</td><td>17</td><td>16</td></tr><tr><td colspan="16">Reserved</td></tr><tr><td>15</td><td>14</td><td>13</td><td>12</td><td>11</td><td>10</td><td>9</td><td>8</td><td>7</td><td>6</td><td>5</td><td>4</td><td>3</td><td>2</td><td>1</td><td>0</td></tr><tr><td colspan="8">Reserved</td><td colspan="8">STSSI</td></tr></table>

<table><tr><td>位</td><td>名称</td><td>访问</td><td>描述</td><td>复位值</td></tr><tr><td>[31:8]</td><td>Reserved</td><td>R0</td><td>保留。</td><td>0</td></tr><tr><td>[7:0]</td><td>STSSI</td><td>RW</td><td>亚秒步进值。在粗调模式下,每个主频周期,PTP系统时间会自增这个值;在精调模式下,在累加器溢出时,PTP系统时间会自增这个值。</td><td>00h</td></tr></table>

#### 27.1.8.3.3 PTP 时间戳寄存器高位（R32_ETH_PTPTSHR）

偏移地址： $0 \times 0 7 0 8$

<table><tr><td>31</td><td>30</td><td>29</td><td>28</td><td>27</td><td>26</td><td>25</td><td>24</td><td>23</td><td>22</td><td>21</td><td>20</td><td>19</td><td>18</td><td>17</td><td>16</td></tr><tr><td colspan="16">STS</td></tr><tr><td>15</td><td>14</td><td>13</td><td>12</td><td>11</td><td>10</td><td>9</td><td>8</td><td>7</td><td>6</td><td>5</td><td>4</td><td>3</td><td>2</td><td>1</td><td>0</td></tr><tr><td colspan="16">STS</td></tr></table>

<table><tr><td>位</td><td>名称</td><td>访问</td><td>描述</td><td>复位值</td></tr><tr><td>[31:0]</td><td>TSHR</td><td>R0</td><td>PTP系统时间值，实时值，单位为秒。</td><td>0</td></tr></table>

#### 27.1.8.3.4 PTP 时间戳寄存器低位（R32_ETH_PTPTSLR）

偏移地址： $0 \times 0 7 0 0$

<table><tr><td>31</td><td>30</td><td>29</td><td>28</td><td>27</td><td>26</td><td>25</td><td>24</td><td>23</td><td>22</td><td>21</td><td>20</td><td>19</td><td>18</td><td>17</td><td>16</td></tr><tr><td>STPNS</td><td colspan="15">STSS</td></tr><tr><td>15</td><td>14</td><td>13</td><td>12</td><td>11</td><td>10</td><td>9</td><td>8</td><td>7</td><td>6</td><td>5</td><td>4</td><td>3</td><td>2</td><td>1</td><td>0</td></tr><tr><td colspan="16">STSS</td></tr></table>

<table><tr><td>位</td><td>名称</td><td>访问</td><td>描述</td><td>复位值</td></tr><tr><td>31</td><td>STPNS</td><td>R0</td><td>系统时间正负标志位。1表示当前时间为负，0表示当前时间为正。</td><td>0</td></tr><tr><td>[30:0]</td><td>STSS</td><td>R0</td><td>PTP系统时间值，实时值，单位为亚秒，即约0.46纳秒。STSS溢出时，STS自增1秒。</td><td>0</td></tr></table>

#### 27.1.8.3.5 PTP 时间戳更新寄存器高 32 位（R32_ETH_PTPTSHUR）

偏移地址： $0 \times 0 7 1 0$

<table><tr><td>31</td><td>30</td><td>29</td><td>28</td><td>27</td><td>26</td><td>25</td><td>24</td><td>23</td><td>22</td><td>21</td><td>20</td><td>19</td><td>18</td><td>17</td><td>16</td></tr><tr><td colspan="16">TSUS</td></tr><tr><td>15</td><td>14</td><td>13</td><td>12</td><td>11</td><td>10</td><td>9</td><td>8</td><td>7</td><td>6</td><td>5</td><td>4</td><td>3</td><td>2</td><td>1</td><td>0</td></tr><tr><td colspan="16">TSUS</td></tr></table>

<table><tr><td>位</td><td>名称</td><td>访问</td><td>描述</td><td>复位值</td></tr><tr><td>[31:0]</td><td>TSUS</td><td>RW</td><td>时间戳更新秒值，用来替换到PTP系统时间高位或者表示其在系统时间上加减的秒值。</td><td>0</td></tr></table>

#### 27.1.8.3.6 PTP 时间戳更新寄存器低 32 位（R32_ETH_PTPTSLUR）

偏移地址： $0 { \times } 0 7 1 4$

<table><tr><td>TSUPN
S</td><td colspan="14">TSUSS</td><td></td></tr><tr><td>15</td><td>14</td><td>13</td><td>12</td><td>11</td><td>10</td><td>9</td><td>8</td><td>7</td><td>6</td><td>5</td><td>4</td><td>3</td><td>2</td><td>1</td><td>0</td></tr><tr><td colspan="15">TSUSS</td><td></td></tr></table>

<table><tr><td>位</td><td>名称</td><td>访问</td><td>描述</td><td>复位值</td></tr><tr><td>31</td><td>TSUPNS</td><td>RW</td><td>时间戳更新正负标志位。当更新时间方式为使用TSUS和TSUSS直接替换PTP系统时间时,TSUPNS应为0;当更新时间方式为使用TSUS和TSUSS作在</td><td>0</td></tr><tr><td></td><td></td><td></td><td>原PTP系统时间值上的加减时,TSUPNS为1表示在PTP系统时间的基础上减去TSUS和TSUSS,为0表示在PTP系统时间的基础上加上TSUS和TSUSS。</td><td></td></tr><tr><td>[30:0]</td><td>TSUSS</td><td>RW</td><td>时间戳更新亚秒值,用来替换到PTP系统时间高位或者表示其在系统时间上加减的亚秒值。</td><td>0</td></tr></table>

#### 27.1.8.3.7 PTP 时间戳加数寄存器（R32_ETH_PTPTSAR）

偏移地址： $0 \times 0 7 1 8$

<table><tr><td>31</td><td>30</td><td>29</td><td>28</td><td>27</td><td>26</td><td>25</td><td>24</td><td>23</td><td>22</td><td>21</td><td>20</td><td>19</td><td>18</td><td>17</td><td>16</td></tr><tr><td colspan="16">TSA</td></tr><tr><td>15</td><td>14</td><td>13</td><td>12</td><td>11</td><td>10</td><td>9</td><td>8</td><td>7</td><td>6</td><td>5</td><td>4</td><td>3</td><td>2</td><td>1</td><td>0</td></tr><tr><td colspan="16">TSA</td></tr></table>

<table><tr><td>位</td><td>名称</td><td>访问</td><td>描述</td><td>复位值</td></tr><tr><td>[31:0]</td><td>TSA</td><td>RW</td><td>时间戳加数值。这个寄存器只在精调模式下才会被使用。TSA的值会在每个系统周期被加到累加器中,如果累加器溢出,则会触发精调模式下的系统时间更新。</td><td>0</td></tr></table>

#### 27.1.8.3.8 PTP 目标时间寄存器高 32 位（R32_ETH_PTPTTHR）

偏移地址： $0 { \times } 0 7 1 0$

<table><tr><td>31</td><td>30</td><td>29</td><td>28</td><td>27</td><td>26</td><td>25</td><td>24</td><td>23</td><td>22</td><td>21</td><td>20</td><td>19</td><td>18</td><td>17</td><td>16</td></tr><tr><td colspan="16">TTSH</td></tr><tr><td>15</td><td>14</td><td>13</td><td>12</td><td>11</td><td>10</td><td>9</td><td>8</td><td>7</td><td>6</td><td>5</td><td>4</td><td>3</td><td>2</td><td>1</td><td>0</td></tr><tr><td colspan="16">TTSH</td></tr></table>

<table><tr><td>位</td><td>名称</td><td>访问</td><td>描述</td><td>复位值</td></tr><tr><td>[31:0]</td><td>TTSH</td><td>RW</td><td>目标时间秒值。如果PTP系统时间达到或者超过这个值，且使能了相关中断，那么会产生一个中断。</td><td>0</td></tr></table>

#### 27.1.8.3.9 PTP 目标时间寄存器高 32 位（R32_ETH_PTPTTHR）

偏移地址： $0 \times 0 7 2 0$

<table><tr><td>31</td><td>30</td><td>29</td><td>28</td><td>27</td><td>26</td><td>25</td><td>24</td><td>23</td><td>22</td><td>21</td><td>20</td><td>19</td><td>18</td><td>17</td><td>16</td></tr><tr><td colspan="16">TTSL</td></tr><tr><td>15</td><td>14</td><td>13</td><td>12</td><td>11</td><td>10</td><td>9</td><td>8</td><td>7</td><td>6</td><td>5</td><td>4</td><td>3</td><td>2</td><td>1</td><td>0</td></tr><tr><td colspan="16">TTSL</td></tr></table>

<table><tr><td>位</td><td>名称</td><td>访问</td><td>描述</td><td>复位值</td></tr><tr><td>[31:0]</td><td>TTSL</td><td>RW</td><td>目标时间亚秒值。如果PTP系统时间达到或超过这个值，且使能了相关中断，那么会产生一个中断。</td><td>0</td></tr></table>

### 27.1.8.4 DMA 控制相关寄存器各位域

#### 27.1.8.4.1 DMA 总线模式寄存器（R32_ETH_DMABMR）

偏移地址： $0 \times 1 0 0 0$

<table><tr><td>31</td><td>30</td><td>29</td><td>28</td><td>27</td><td>26</td><td>25</td><td>24</td><td>23</td><td>22</td><td>21</td><td>20</td><td>19</td><td>18</td><td>17</td><td>16</td></tr><tr><td colspan="16">Reserved</td></tr><tr><td>15</td><td>14</td><td>13</td><td>12</td><td>11</td><td>10</td><td>9</td><td>8</td><td>7</td><td>6</td><td>5</td><td>4</td><td>3</td><td>2</td><td>1</td><td>0</td></tr><tr><td colspan="9">Reserved</td><td colspan="4">DSL</td><td>Reser ved</td><td colspan="2">SR</td></tr></table>

<table><tr><td>位</td><td>名称</td><td>访问</td><td>描述</td><td>复位值</td></tr><tr><td>[31:7]</td><td>Reserved</td><td>R0</td><td>保留。</td><td>0</td></tr><tr><td>[6:2]</td><td>DSL</td><td>RW</td><td>描述符跳跃长度:这些位定义了2个不以链式结构连接的描述符之间的跳跃距离,单位为字(32位)。地址跳跃是指从当前描述符的结尾到下一个描述符开头的地址差值。当DSL域为0时,在环形结构下,DMA认为描述符是相邻地连续排列的。</td><td>0</td></tr><tr><td>1</td><td>Reserved</td><td>R0</td><td>保留。</td><td>0</td></tr><tr><td>0</td><td>SR</td><td>RW</td><td>软件复位:置1时,MAC的DMA控制器复位MAC所有子系统的内部寄存器和逻辑电路。在MAC内部不同时钟域模块完成复位操作后,自动清除该位。在重新写MAC的寄存器前,应当确保该位为0。</td><td>1</td></tr></table>

#### 27.1.8.4.2 DMA 发送查询寄存器（R32_ETH_DMATPDR）

偏移地址： $0 \times 1 0 0 4$

<table><tr><td>31</td><td>30</td><td>29</td><td>28</td><td>27</td><td>26</td><td>25</td><td>24</td><td>23</td><td>22</td><td>21</td><td>20</td><td>19</td><td>18</td><td>17</td><td>16</td></tr><tr><td colspan="16">TPDR</td></tr><tr><td>15</td><td>14</td><td>13</td><td>12</td><td>11</td><td>10</td><td>9</td><td>8</td><td>7</td><td>6</td><td>5</td><td>4</td><td>3</td><td>2</td><td>1</td><td>0</td></tr><tr><td colspan="16">TPDR</td></tr></table>

<table><tr><td>位</td><td>名称</td><td>访问</td><td>描述</td><td>复位值</td></tr><tr><td>[31:0]</td><td>TPDR</td><td>RW</td><td>发送查询命令。用户通过向这个寄存器写任意值来启动被暂停的发送流程。重启发送流程后，这个寄存器会被自动清零。</td><td>0</td></tr></table>

#### 27.1.8.4.3 DMA 接收查询寄存器（R32_ETH_DMARPDR）

偏移地址： $0 \times 1 0 0 8$

<table><tr><td>31</td><td>30</td><td>29</td><td>28</td><td>27</td><td>26</td><td>25</td><td>24</td><td>23</td><td>22</td><td>21</td><td>20</td><td>19</td><td>18</td><td>17</td><td>16</td></tr><tr><td colspan="16">RPDR</td></tr><tr><td>15</td><td>14</td><td>13</td><td>12</td><td>11</td><td>10</td><td>9</td><td>8</td><td>7</td><td>6</td><td>5</td><td>4</td><td>3</td><td>2</td><td>1</td><td>0</td></tr><tr><td colspan="16">RPDR</td></tr></table>

<table><tr><td>位</td><td>名称</td><td>访问</td><td>描述</td><td>复位值</td></tr><tr><td>[31:0]</td><td>RPDR</td><td>RW</td><td>接收查询命令。接收流程可能会被各种意外而打断，需要用户向这个寄存器写任意值来重启接收流程。重启接收流程后，这个寄存器会被自动清零。</td><td>0</td></tr></table>

#### 27.1.8.4.4 DMA 接收描述符地址寄存器（R32_ETH_DMARDLAR）

偏移地址： $0 \times 1 0 0 0$

<table><tr><td>31</td><td>30</td><td>29</td><td>28</td><td>27</td><td>26</td><td>25</td><td>24</td><td>23</td><td>22</td><td>21</td><td>20</td><td>19</td><td>18</td><td>17</td><td>16</td></tr><tr><td colspan="16">RDLAR</td></tr><tr><td>15</td><td>14</td><td>13</td><td>12</td><td>11</td><td>10</td><td>9</td><td>8</td><td>7</td><td>6</td><td>5</td><td>4</td><td>3</td><td>2</td><td>1</td><td>0</td></tr><tr><td colspan="16">RDLAR</td></tr></table>

<table><tr><td>位</td><td>名称</td><td>访问</td><td>描述</td><td>复位值</td></tr><tr><td>[31:0]</td><td>RDLAR</td><td>RW</td><td>这个寄存器用来存储第一个接收 DMA 描述符的地址。注意描述符需要 16 字节对齐,所以这个寄存器的后 4 位应该为 0 。</td><td>0</td></tr></table>

#### 27.1.8.4.5 DMA 发送描述符地址寄存器（R32_ETH_DMATDLAR）

偏移地址： $0 \times 1 0 1 0$

<table><tr><td>31</td><td>30</td><td>29</td><td>28</td><td>27</td><td>26</td><td>25</td><td>24</td><td>23</td><td>22</td><td>21</td><td>20</td><td>19</td><td>18</td><td>17</td><td>16</td></tr><tr><td colspan="16">TDLAR</td></tr><tr><td>15</td><td>14</td><td>13</td><td>12</td><td>11</td><td>10</td><td>9</td><td>8</td><td>7</td><td>6</td><td>5</td><td>4</td><td>3</td><td>2</td><td>1</td><td>0</td></tr><tr><td colspan="16">TDLAR</td></tr></table>

<table><tr><td>位</td><td>名称</td><td>访问</td><td>描述</td><td>复位值</td></tr><tr><td>[31:0]</td><td>TDLAR</td><td>RW</td><td>这个寄存器用来存储第一个发送 DMA 描述符的地址。注意描述符需要 16 字节对齐,所以这个寄存器的后 4 位应该为 0 。</td><td>0</td></tr></table>

#### 27.1.8.4.6 DMA 状态寄存器（R32_ETH_DMASR）

偏移地址： $0 \times 1 0 1 4$

<table><tr><td>31</td><td>30</td><td>29</td><td>28</td><td>27</td><td>26</td><td>25</td><td>24</td><td>23</td><td>22</td><td>21</td><td>20</td><td>19</td><td>18</td><td>17</td><td>16</td></tr><tr><td>PLS</td><td>Reser ved</td><td>TSTS</td><td>PMTS</td><td>MMCS</td><td>Reser ved</td><td colspan="3">EBS</td><td colspan="3">TPS</td><td colspan="3">RPS</td><td>NIS</td></tr><tr><td>15</td><td>14</td><td>13</td><td>12</td><td>11</td><td>10</td><td>9</td><td>8</td><td>7</td><td>6</td><td>5</td><td>4</td><td>3</td><td>2</td><td>1</td><td>0</td></tr><tr><td>AIS</td><td>ERS</td><td>FBES</td><td colspan="2">Reserved</td><td>ETS</td><td>RWTS</td><td>RPSS</td><td>RBUS</td><td>RS</td><td>TUS</td><td>ROS</td><td>TJTS</td><td>TBUS</td><td>TPSS</td><td>TS</td></tr></table>

<table><tr><td>位</td><td>名称</td><td>访问</td><td>描述</td><td>复位值</td></tr><tr><td>31</td><td>PLS</td><td>RW1Z</td><td>内部10M物理层连接状态改变标志位。此位置位表示物理层连接上或者已断开。或写1清零。</td><td>0</td></tr><tr><td>30</td><td>Reserved</td><td>R0</td><td>保留。</td><td>0</td></tr><tr><td>29</td><td>TSTS</td><td>R0</td><td>时间戳触发状态标志位。在PTP部分产生中断事件时,此位会被置位,如果使能了PTP中断则会产生中断。清除PTP部分的全部标志位后,此位会自动清除。</td><td>0</td></tr><tr><td>28</td><td>PMTS</td><td>R0</td><td>PMT触发状态标志位。在PMT部分产生中断事件时,此位会被置位,如果使能了PMT中断则会产生中断。清除PMT部分的全部标志位后,此位会自动清除。</td><td>0</td></tr><tr><td>27</td><td>MMCS</td><td>R0</td><td>MMC触发状态标志位。在MMC部分产生中断事件时,此位会被置位,如果使能了MMC中断则会产生中断。清除MMC部分的全部标志位后,此位会自动清除。</td><td>0</td></tr><tr><td>26</td><td>Reserved</td><td>R0</td><td>保留。</td><td>0</td></tr><tr><td>[25:23]</td><td>EBS</td><td>R0</td><td>错误状态域。该域表示造成总线错误的类型。此域只在DMASR[13]位被置位的情况下才有效。DMASR[23]:0:发送DMA转发数据时出错;1:接收DMA转发数据时出错;DMASR[24]:0:读数据转发时出错;R01:写数据转发时出错;DMASR[25]:0:访问描述符时出错;1:访问数据缓存时出错;</td><td>0</td></tr><tr><td>[22:20]</td><td>TPS</td><td>R0</td><td>发送流程状态域。这个域用来表示当前发送DMA的状态。000:停止,接收到复位或者停止发送命令;001:运行,正在取发送描述符;010:运行,正在等待状态信息;011:运行,正在读取发送缓冲区数据并压进FIFO;100,101:保留;110:暂停,发送描述符不可用或者发送缓存数据下溢;111:运行,正在关闭发送描述符;</td><td>000b</td></tr><tr><td>[19:17]</td><td>RPS</td><td>R0</td><td>接收流程状态域。这个域用来表示当前接收DMA的状态。000:停止,接收到复位或者停止发送命令;</td><td>000b</td></tr><tr><td></td><td></td><td></td><td>001: 运行,正在取接收描述符;010: 保留;011: 运行,正在等待接收数据包;100: 暂停,接收描述符不可用;101: 运行,正在关闭接收描述符;110: 保留;111: 运行,正在把接收数据从FIFO压入内存中;</td><td></td></tr><tr><td>16</td><td>NIS</td><td>RW1Z</td><td>正常中断汇总位。在DMAIER 寄存器中使能的中断下,如果下列位任一被置位,NIS位同样会被置位。-DMASR[0]:发送中断;-DMASR[2]:发送缓存不可用;-DMASR[6]:接收中断;-DMASR[14]:早接收中断;-DMASR[31]:内部10M物理层连接状态改变NIS位为黏着位,当导致NIS位置1的对应中断状态位被清零时,必须通过写1的方式将NIS位也清零;</td><td>0</td></tr><tr><td>15</td><td>AIS</td><td>RW1Z</td><td>异常中断汇总位。在DMAIER 寄存器中使能的中断下,如果下列位任一被置位,AIS位同样会被置位。-DMASR[1]:发送流程停止;-DMASR[3]:发送啰嗦(Jabber)超时;-DMASR[4]:接收FIFO溢出;-DMASR[5]:发送数据下溢;-DMASR[7]:发送缓存不可用;-DMASR[8]:接收流程停止;-DMASR[9]:接收看门狗超时;-DMASR[10]:早发送;-DMASR[13]:总线错误;将以上所有的位清零,则AIS位将自动清零;</td><td>0</td></tr><tr><td>14</td><td>ERS</td><td>RW1Z</td><td>早接收状态位。该位被置位时,表示收到数据帧时DMA已经填满了第一个缓冲区,但是完整的帧还没接收完毕。RS置位后,ERS位自动清零。</td><td>0</td></tr><tr><td>13</td><td>FBES</td><td>RW1Z</td><td>总线错误位。该位被置位时,表示发送了总线错误。具体原因见[25:23]位。该位被置位后,相应的DMA控制器将关闭总线访问。</td><td>0</td></tr><tr><td>[12:11]</td><td>Reserved</td><td>R0</td><td>保留。</td><td>0</td></tr><tr><td>10</td><td>ETS</td><td>RW1Z</td><td>早发送状态位。置位时表示发送帧已经全部压入FIFO。</td><td>0</td></tr><tr><td>9</td><td>RWTS</td><td>RW1Z</td><td>看门狗超时标志位。置位时表示帧长已经超过了2048字节。</td><td>0</td></tr><tr><td>8</td><td>RPSS</td><td>RW1Z</td><td>接收流程停止状态位。该位置位表示接收流程已经停止。</td><td>0</td></tr><tr><td>7</td><td>RBUS</td><td>RW1Z</td><td>接收缓存不可用状态位。该位置位表示接收描述符权限归属CPU,DMA无法获得,接收流程已经暂停。用户需要释放描述符并向RPDR寄存器中填入一个值来恢复接收流程。DMA会在接收下一帧时重试获</td><td>0</td></tr><tr><td></td><td></td><td></td><td>取描述符。</td><td></td></tr><tr><td>6</td><td>RS</td><td>RW1Z</td><td>接收完成状态位。该位置位表示已经完成一帧的接收,帧信息也更新到描述符中。</td><td>0</td></tr><tr><td>5</td><td>TUS</td><td>RW1Z</td><td>发送数据下溢位。该位置位表示发送缓存在发送帧时发生了发送数据下溢。此时发送进程暂停并把数据下溢错误位置位。</td><td>0</td></tr><tr><td>4</td><td>ROS</td><td>RW1Z</td><td>接收状态溢出位。该位被置位表示接收缓冲发生了数据溢出。</td><td>0</td></tr><tr><td>3</td><td>TJTS</td><td>RW1Z</td><td>发送啰嗦(Jabber)定时器超时位。该位被置位表示发送器过于繁忙,发送已经停止,描述符的啰嗦(Jabber)超时位已经置位。</td><td>0</td></tr><tr><td>2</td><td>TBUS</td><td>RW1Z</td><td>发送缓存不可用状态位。该位被置位表示发送描述符被CPU占用,DMA无法获取,发送流程已经暂停。第[22:20]位显示了当前的发送状态。应用程序需要释放发送描述符。</td><td>0</td></tr><tr><td>1</td><td>TPSS</td><td>RW1Z</td><td>发送流程停止状态位。该位被置位表示发送流程已经停止。</td><td>0</td></tr><tr><td>0</td><td>TS</td><td>RW1Z</td><td>发送完成标志位。该位被置位表示已经完成一帧的发送,描述符归属权已经交还CPU。</td><td>0</td></tr></table>

#### 27.1.8.4.7 DMA 操作模式寄存器（R32_ETH_DMAOMR）

偏移地址： $0 \times 1 0 1 8$

<table><tr><td>31</td><td>30</td><td>29</td><td>28</td><td>27</td><td>26</td><td>25</td><td>24</td><td>23</td><td>22</td><td>21</td><td>20</td><td>19</td><td>18</td><td>17</td><td>16</td></tr><tr><td colspan="4">Reserved</td><td>DTCEF D</td><td colspan="5">Reserved</td><td>TSF</td><td>FTF</td><td colspan="4">Reserved</td></tr><tr><td>15</td><td>14</td><td>13</td><td>12</td><td>11</td><td>10</td><td>9</td><td>8</td><td>7</td><td>6</td><td>5</td><td>4</td><td>3</td><td>2</td><td>1</td><td>0</td></tr><tr><td>Reserved</td><td>ST</td><td colspan="5">Reserved</td><td>FEF</td><td>FUGF</td><td colspan="4">Reserved</td><td>SR</td><td colspan="2">Reserved</td></tr></table>

<table><tr><td>位</td><td>名称</td><td>访问</td><td>描述</td><td>复位值</td></tr><tr><td>[31:27]</td><td>Reserved</td><td>RO</td><td>保留。</td><td>0</td></tr><tr><td>26</td><td>DTCEFD</td><td>RW</td><td>不丢弃 TCP/IP 校验和错误帧控制位。0: 如果 FEF 位为 0, MAC 会丢弃所有有错误的帧;1: 发现 TCP/IP/ICMP/UDP 之类协议的检验和存在错误时, 不丢弃该帧;</td><td>0</td></tr><tr><td>[25:22]</td><td>Reserved</td><td>RO</td><td>保留。</td><td>0</td></tr><tr><td>21</td><td>TSF</td><td>RW</td><td>发送存储转发控制位。0: 发送流程写入 FIFO0 的数据达到确定值之后就会启动发送;1: 发送流程将完整的帧全部写入 FIFO0 后才会启动发送;</td><td>0</td></tr><tr><td>20</td><td>FTF</td><td>RW</td><td>发送 FIFO0 清空控制位。置此位将复位发送 FIFO0。</td><td>0</td></tr><tr><td>[19:14]</td><td>Reserved</td><td>RO</td><td>保留。</td><td>0</td></tr><tr><td>13</td><td>ST</td><td>RW</td><td>开始或停止发送控制位。</td><td>0</td></tr><tr><td></td><td></td><td></td><td>0:发送完当前帧之后,发送进程进入停止状态;1:把发送进程置为运行状态。</td><td></td></tr><tr><td>[12:8]</td><td>Reserved</td><td>R0</td><td>保留。</td><td>0</td></tr><tr><td>7</td><td>FEF</td><td>RW</td><td>转发错误帧控制位。0:除了过短的帧之外,所有的帧都会转发给DMA。1:接收FIFO会丢弃有错误的帧。</td><td>0</td></tr><tr><td>6</td><td>FUGF</td><td>RW</td><td>转发过短的帧控制位。0:接收FIFO丢弃所有长度小于64字节的帧;1:接收FIFO转发长度过短的帧。</td><td>0</td></tr><tr><td>[5:2]</td><td>Reserved</td><td>R0</td><td>保留。</td><td>0</td></tr><tr><td>1</td><td>SR</td><td>RW</td><td>开始或停止接收控制位。0:在转发完当前接收到的帧后,接收DMA进入停止模式,下一次传输从当前接收描述符位置开始;1:开启接收流程,DMA从当前位置取接收描述符或者从标识符表头位置取接收描述符。</td><td>0</td></tr><tr><td>0</td><td>Reserved</td><td>R0</td><td>保留。</td><td>0</td></tr></table>

#### 27.1.8.4.8 DMA 中断使能寄存器（R32_ETH_DMAIER）

偏移地址： $0 \times 1 0 1 0$

<table><tr><td>31</td><td>30</td><td>29</td><td>28</td><td>27</td><td>26</td><td>25</td><td>24</td><td>23</td><td>22</td><td>21</td><td>20</td><td>19</td><td>18</td><td>17</td><td>16</td></tr><tr><td>PLE</td><td colspan="14">Reserved</td><td>NISE</td></tr><tr><td>15</td><td>14</td><td>13</td><td>12</td><td>11</td><td>10</td><td>9</td><td>8</td><td>7</td><td>6</td><td>5</td><td>4</td><td>3</td><td>2</td><td>1</td><td>0</td></tr><tr><td>AISE</td><td>ERS</td><td>FBES</td><td>Reserved</td><td>ETIE</td><td>RWTIE</td><td>RPSIE</td><td>RBUIE</td><td>RIE</td><td>TUIE</td><td>ROIE</td><td>TJTIE</td><td>TBUIE</td><td>TPSIE</td><td>TIE</td><td></td></tr></table>

<table><tr><td>位</td><td>名称</td><td>访问</td><td>描述</td><td>复位值</td></tr><tr><td>31</td><td>PLE</td><td>RW</td><td>内部10M物理层连接状态改变中断使能位。</td><td>0</td></tr><tr><td>[30:17]</td><td>Reserved</td><td>RO</td><td>保留。</td><td>0</td></tr><tr><td>16</td><td>NISE</td><td>RW</td><td>正常中断使能位。使能该位将使能以下中断。-DMASR[0]:发送中断;-DMASR[2]:发送缓存不可用;-DMASR[6]:接收中断-DMASR[14]:早接收中断</td><td>0</td></tr><tr><td>15</td><td>AISE</td><td>RW</td><td>异常中断。使能该位将使能以下中断。-DMASR[1]:发送流程停止;-DMASR[3]:发送啰嗦(Jabber)超时;-DMASR[4]:接收FIFO溢出;-DMASR[5]:发送数据下溢;-DMASR[7]:发送缓存不可用;-DMASR[8]:接收流程停止;-DMASR[9]:接收看门狗超时;-DMASR[10]:早发送;-DMASR[13]:总线错误;</td><td>0</td></tr><tr><td>14</td><td>ERS</td><td>RW</td><td>早接收中断使能位。使能该位即可以产生早接收中断。NISE须置位。AISE须置位。</td><td>0</td></tr><tr><td>13</td><td>FBES</td><td>RW</td><td>总线致命错误中断使能位。使能该位即可以产生总线错误中断。AISE须置位。</td><td>0</td></tr><tr><td>[12:11]</td><td>Reserved</td><td>RO</td><td>保留。</td><td>0</td></tr><tr><td>10</td><td>ETIE</td><td>RW</td><td>早发送中断使能位。使能该位即可以产生早发送中断。AISE须置位。</td><td>0</td></tr><tr><td>9</td><td>RWTIE</td><td>RW</td><td>接收看门狗中断使能位。使能该位即可以产生接收看门狗超时中断。AISE须置位。</td><td>0</td></tr><tr><td>8</td><td>RPSIE</td><td>RW</td><td>接收流程停止中断使能位。使能该位即可以产生接受流程停止中断。</td><td>0</td></tr><tr><td>7</td><td>RBUIE</td><td>RW</td><td>接收缓存不可用中断使能位。使能该位即可以产生接受缓存不可用中断。AISE须置位。</td><td>0</td></tr><tr><td>6</td><td>RIE</td><td>RW</td><td>接收完成中断使能位。使能该位即可以产生接收完成中断。NISE须置位。</td><td>0</td></tr><tr><td>5</td><td>TUIE</td><td>RW</td><td>发送下溢中断使能。使能该位即可以产生发送下溢中断。AISE须置位。</td><td>0</td></tr><tr><td>4</td><td>ROIE</td><td>RW</td><td>接收溢出中断。使能该位即可以产生接收溢出中断。AISE须置位。</td><td>0</td></tr><tr><td>3</td><td>TJTIE</td><td>RW</td><td>发送啰嗦(Jabber)超时中断使能。使能该位即可以产生发送啰嗦(Jabber)超时中断。AISE须置位。</td><td>0</td></tr><tr><td>2</td><td>TBUIE</td><td>RW</td><td>发送缓存不可用中断使能。使能该位即可以产生发送缓存不可用中断。NISE须置位</td><td>0</td></tr><tr><td>1</td><td>TPSIE</td><td>RW</td><td>发送流程停止中断使能。使能该位即可以产生发送流程停止中断。AISE须置位。</td><td>0</td></tr><tr><td>0</td><td>TIE</td><td>RW</td><td>发送完成中断使能。使能该位即可以产生发送完成中断。NISE须置位。</td><td>0</td></tr></table>

#### 27.1.8.4.9 DMA 丢失帧寄存器（R32_ETH_DMAMFBOCR）

偏移地址： $0 \times 1 0 2 0$

<table><tr><td>31</td><td>30</td><td>29</td><td>28</td><td>27</td><td>26</td><td>25</td><td>24</td><td>23</td><td>22</td><td>21</td><td>20</td><td>19</td><td>18</td><td>17</td><td>16</td></tr><tr><td colspan="15">Reserved</td><td>OMFC</td></tr><tr><td>15</td><td>14</td><td>13</td><td>12</td><td>11</td><td>10</td><td>9</td><td>8</td><td>7</td><td>6</td><td>5</td><td>4</td><td>3</td><td>2</td><td>1</td><td>0</td></tr><tr><td colspan="16">MFC</td></tr></table>

<table><tr><td>位</td><td>名称</td><td>访问</td><td>描述</td><td>复位值</td></tr><tr><td>[31:17]</td><td>Reserved</td><td>R0</td><td>保留。</td><td>0</td></tr><tr><td>16</td><td>OMFC</td><td>RW</td><td>丢失帧计数器溢出位。</td><td>0</td></tr><tr><td>[15:0]</td><td>MFC</td><td>RW</td><td>丢失帧计数器。这个域表示由于接收缓冲区不可用导致的帧丢失的数量。</td><td>0</td></tr></table>

#### 27.1.8.4.10 DMA 当前发送描述符寄存器（R32_ETH_DMACHTDR）

偏移地址： $0 \times 1 0 4 8$

<table><tr><td>31</td><td>30</td><td>29</td><td>28</td><td>27</td><td>26</td><td>25</td><td>24</td><td>23</td><td>22</td><td>21</td><td>20</td><td>19</td><td>18</td><td>17</td><td>16</td></tr><tr><td colspan="16">HTDAR</td></tr><tr><td>15</td><td>14</td><td>13</td><td>12</td><td>11</td><td>10</td><td>9</td><td>8</td><td>7</td><td>6</td><td>5</td><td>4</td><td>3</td><td>2</td><td>1</td><td>0</td></tr><tr><td colspan="16">HTDAR</td></tr></table>

<table><tr><td>位</td><td>名称</td><td>访问</td><td>描述</td><td>复位值</td></tr><tr><td>[31:0]</td><td>HTDAR</td><td>RO</td><td>这个寄存器值指向目前正在使用的发送描述符。此寄存器由DMA负责实时更新。</td><td>0</td></tr></table>

#### 27.1.8.4.11 DMA 当前接收描述符寄存器（R32_ETH_DMACHRDR）

偏移地址： $0 \times 1 0 4 0$

<table><tr><td>31</td><td>30</td><td>29</td><td>28</td><td>27</td><td>26</td><td>25</td><td>24</td><td>23</td><td>22</td><td>21</td><td>20</td><td>19</td><td>18</td><td>17</td><td>16</td></tr><tr><td colspan="16">HRDAR</td></tr><tr><td>15</td><td>14</td><td>13</td><td>12</td><td>11</td><td>10</td><td>9</td><td>8</td><td>7</td><td>6</td><td>5</td><td>4</td><td>3</td><td>2</td><td>1</td><td>0</td></tr><tr><td colspan="16">HRDAR</td></tr></table>

<table><tr><td>位</td><td>名称</td><td>访问</td><td>描述</td><td>复位值</td></tr><tr><td>[31:0]</td><td>HRDAR</td><td>R0</td><td>这个寄存器值指向目前正在使用的接收描述符。此寄存器由DMA负责实时更新。</td><td>0</td></tr></table>

#### 27.1.8.4.12 DMA 当前发送缓冲区寄存器（R32_ETH_DMACHTBAR）

偏移地址： $0 \times 1 0 5 0$

<table><tr><td>31</td><td>30</td><td>29</td><td>28</td><td>27</td><td>26</td><td>25</td><td>24</td><td>23</td><td>22</td><td>21</td><td>20</td><td>19</td><td>18</td><td>17</td><td>16</td></tr><tr><td colspan="16">HTBAR</td></tr><tr><td>15</td><td>14</td><td>13</td><td>12</td><td>11</td><td>10</td><td>9</td><td>8</td><td>7</td><td>6</td><td>5</td><td>4</td><td>3</td><td>2</td><td>1</td><td>0</td></tr><tr><td colspan="16">HTBAR</td></tr></table>

<table><tr><td>位</td><td>名称</td><td>访问</td><td>描述</td><td>复位值</td></tr><tr><td>[31:0]</td><td>HTBAR</td><td>RO</td><td>这个寄存器值指向目前正在使用的发送缓冲区。此寄存器由DMA负责实时更新。</td><td>0</td></tr></table>

#### 27.1.8.4.13 DMA 当前接收缓冲区寄存器（R32_ETH_DMACHRBAR）

偏移地址： $0 \times 1 0 5 4$

<table><tr><td>31</td><td>30</td><td>29</td><td>28</td><td>27</td><td>26</td><td>25</td><td>24</td><td>23</td><td>22</td><td>21</td><td>20</td><td>19</td><td>18</td><td>17</td><td>16</td></tr><tr><td colspan="16">HRBAR</td></tr><tr><td>15</td><td>14</td><td>13</td><td>12</td><td>11</td><td>10</td><td>9</td><td>8</td><td>7</td><td>6</td><td>5</td><td>4</td><td>3</td><td>2</td><td>1</td><td>0</td></tr><tr><td colspan="16">HRBAR</td></tr></table>

<table><tr><td>位</td><td>名称</td><td>访问</td><td>描述</td><td>复位值</td></tr><tr><td>[31:0]</td><td>HRBAR</td><td>R0</td><td>这个寄存器值指向目前正在使用的接收缓冲区。此寄存器由DMA负责实时更新。</td><td>0</td></tr></table>

### 27.1.8.5 内部 10M 物理层相关寄存器地址

#### 27.1.8.5.1 基础控制寄存器（BMCR）

偏移地址： $0 \times 0 0$   

<table><tr><td>位</td><td>名称</td><td>访问</td><td>描述</td><td>复位值</td></tr><tr><td>15</td><td>RST</td><td>RW/SC</td><td>1: PHY 复位;0: 正常操作;</td><td>0</td></tr><tr><td>14</td><td>Loopback</td><td>RW</td><td>1: 使能回环;0: 正常操作;</td><td>0</td></tr><tr><td>13</td><td>Speed Selection</td><td>RO</td><td>0: 10Mb/s;</td><td>0</td></tr><tr><td>12</td><td>Auto-Negotiation</td><td>RW</td><td>1: 使能自动协商;0: 禁止自动协商;</td><td>1</td></tr><tr><td>[11:10]</td><td>Reserved</td><td>RO</td><td>保留;</td><td>0</td></tr><tr><td>9</td><td>Restart Auto-Negotiation</td><td>RW/SC</td><td>1: 重新自动协商;0: 正常操作;</td><td>0</td></tr><tr><td>8</td><td>Duplex Mode</td><td>RW</td><td>1: 全双工;0: 半双工;</td><td>1</td></tr><tr><td>7</td><td>Collision Test</td><td>RW</td><td>1: 冲突检测使能;0: 正常操作;</td><td>0</td></tr><tr><td>[6:0]</td><td>Reserved</td><td>RO</td><td>保留。</td><td>0</td></tr></table>

#### 27.1.8.5.2 基础状态寄存器（BMSR）

地址偏移： $0 \times 0 1$   

<table><tr><td>位</td><td>名称</td><td>访问</td><td>描述</td><td>复位</td></tr><tr><td>[15:6]</td><td>Reserved</td><td>R0</td><td>保留;</td><td>0</td></tr><tr><td>5</td><td>Auto-Negotiation Complete</td><td>R0</td><td>1:自动协商完成;0:自动协商未完成;</td><td>0</td></tr><tr><td>[4:3]</td><td>Reserved</td><td>R0</td><td>保留;</td><td>0</td></tr><tr><td>2</td><td>Link</td><td>R0</td><td>1:物理层建立链接;0:物理层未建立链接;</td><td>0</td></tr><tr><td>[1:0]</td><td>Reserved</td><td>R0</td><td>保留;</td><td>0</td></tr></table>

#### 27.1.8.5.3 自动协商链接方能力寄存器（R16_ETH_ANLPAR）

偏移地址： $0 \times 0 5$   

<table><tr><td>位</td><td>名称</td><td>访问</td><td>描述</td><td>复位值</td></tr><tr><td>15</td><td>NP</td><td>R0</td><td>下一页位。
1:发送协议细节数据页;
0:发送基本能力数据页。</td><td>0</td></tr><tr><td>14</td><td>ACK</td><td>R0</td><td>1:链接方确认接收到本地节点的能力数据字;
0:没有应答。</td><td>0</td></tr><tr><td>13</td><td>RF</td><td>R0</td><td>1:链接方正在指示一个远端故障;
0:链接方没有指示远端故障。</td><td>0</td></tr><tr><td>12</td><td>Reserved</td><td>R0</td><td>保留。</td><td>0</td></tr><tr><td>11</td><td>ASYPAUSE</td><td>R0</td><td>1:链接方支持非对称流控;0:链接方不支持非对称流控。启用自动协商时,此位反映链接方的能力。</td><td>0</td></tr><tr><td>10</td><td>PAUSE</td><td>R0</td><td>1:链接方支持流控;0:链接方不支持流控。启用自动协商时,此位反映链接方的能力。(只读)。</td><td>0</td></tr><tr><td>9</td><td>100Base-T4</td><td>R0</td><td>1:链接方支持100Base-T4;0:链接方不支持100Base-T4。</td><td>0</td></tr><tr><td>8</td><td>100Base-TX-FD</td><td>R0</td><td>1:链接方支持100Base-TX全双工;0:链接方不支持100Base-TX全双工。</td><td>0</td></tr><tr><td>7</td><td>100Base-TX</td><td>R0</td><td>1:链接方支持100Base-TX;0:链接方不支持100Base-TX。由并行检测建立了100Base-TX链接之后,该位同样会被置位。</td><td>0</td></tr><tr><td>6</td><td>10Base-T-FD</td><td>R0</td><td>1:链接方支持10Base-TX全双工;0:链接方不支持10Base-TX全双工。</td><td>0</td></tr><tr><td>5</td><td>10Base-T</td><td>R0</td><td>1:链接方支持10Base-T;0:链接方不支持10Base-T。由并行检测建立了10Base-T链接之后,该位同样会被置位。</td><td>0</td></tr><tr><td>[4:0]</td><td>SELECT</td><td>R0</td><td>链接方的二进制编码节点选择器,当前仅CSMA/CD &lt;00001&gt;被指定。</td><td>00001b</td></tr></table>

#### 27.1.8.5.4 物理层状态寄存器（PHYSR）

偏移地址： $0 \times 1 0$   

<table><tr><td>位</td><td>名称</td><td>访问</td><td>描述</td><td>复位值</td></tr><tr><td>[15:4]</td><td>Reserved</td><td>R0</td><td>保留;</td><td>0</td></tr><tr><td>3</td><td>Loopback_10M</td><td>R0</td><td>1: PHY工作于10M自循环0:正常模式</td><td>0</td></tr><tr><td>2</td><td>Full_10M</td><td>R0</td><td>1: PHY工作于10M全双工0:半双工模式</td><td>0</td></tr><tr><td>[1:0]</td><td>Reserved</td><td>R0</td><td>保留;</td><td>0</td></tr></table>

#### 27.1.8.5.5 自动翻转寄存器（MDIX）

偏移地址寄存器：0x1E  

<table><tr><td>位</td><td>名称</td><td>访问</td><td>描述</td><td>复位值</td></tr><tr><td>[15:4]</td><td>Reserved</td><td>R0</td><td>保留;</td><td>0</td></tr><tr><td>[3:2]</td><td>P/N极性</td><td>RW</td><td>1x:保留。
00:P/N极性正常
01:P/N极性颠倒</td><td>00b</td></tr><tr><td>[1:0]</td><td>T/R选择</td><td>RW</td><td>00:自动</td><td>00b</td></tr><tr><td></td><td></td><td></td><td>01: MDIX
1x: MDI</td><td></td></tr></table>

## 27.2 CH32F20x_D8W、CH32V20x_D8、CH32V20x_D8W 系列

### 27.2.1 以太网控制器简介

芯片集成了以太网控制器 MAC 和 PHY 以及 DMA，兼容 IEEE802.3 协议。内部 DMA 传输数据和接收数据到系统 RAM 中。PHY 物理层是一个 10Mbit/S 的以太网络收发器，提供部分网络 PHY 寄存器，可以设置发送和接收性能。

网络底层操作主要以子程序库提供应用支持，对寄存器不作详细介绍。主要特性：

支持全双工和半双工。  
支持短包填充设置。  
$\bullet$ 支持 CRC 设置和填充。  
支持巨型帧接收。  
支持不同的过滤模式组合。  
$\bullet$ 支持 pause 帧发送和设置。  
$\bullet$ 支持自动协商机制。  
$\bullet$ 支持 DMA，用于发送和接收数据。  
PHY 收发器兼容 10BASE-T，发送模块支持节能模式。  
$\bullet$ 内置 50 欧姆传输阻抗匹配电阻，也可选择外接。  
提供由 IEEE 分配的全球唯一 MAC 地址。

### 27.2.2 寄存器描述

表27-23 以太网控制器相关寄存器列表  

<table><tr><td>名称</td><td>偏移地址</td><td>描述</td><td>复位值</td></tr><tr><td>R8_ETH_EIE</td><td>0x40028003</td><td>中断使能寄存器</td><td>0x00</td></tr><tr><td>R8_ETH_EIR</td><td>0x40028004</td><td>中断标志寄存器</td><td>0x00</td></tr><tr><td>R8_ETH_ESTAT</td><td>0x40028005</td><td>状态寄存器</td><td>0x00</td></tr><tr><td>R8_ETH_ECON2</td><td>0x40028006</td><td>PHY模拟参数设置寄存器</td><td>0x06</td></tr><tr><td>R8_ETH_ECON1</td><td>0x40028007</td><td>收发控制寄存器</td><td>0x00</td></tr><tr><td>R32_ETH_TX</td><td>0x40028008</td><td>发送DMA控制寄存器</td><td>0xXXXXXXXXX</td></tr><tr><td>R16_ETH_ETXST</td><td>0x40028008</td><td>发送DMA缓存区起始地址寄存器</td><td>0xXXXX</td></tr><tr><td>R16_ETH_ETXLN</td><td>0x4002800A</td><td>发送长度寄存器</td><td>0xXXXX</td></tr><tr><td>R32_ETH_RX</td><td>0x4002800C</td><td>接收DMA控制寄存器</td><td>0x0000000</td></tr><tr><td>R16_ETH_ERXST</td><td>0x4002800C</td><td>接收DMA缓冲区起始地址寄存器</td><td>0x0000</td></tr><tr><td>R16_ETH_ERXLN</td><td>0x4002800E</td><td>接收长度寄存器</td><td>0x0000</td></tr><tr><td>R32_ETH_HTL</td><td>0x40028010</td><td>Hash表低字节寄存器</td><td>0x484EA033</td></tr><tr><td>R32_ETH_HTH</td><td>0x40028014</td><td>Hash表高字节寄存器</td><td>0x5000EF97</td></tr><tr><td>R32_ETH_MACON</td><td>0x40028018</td><td>接收过滤设置寄存器</td><td>0x10000000</td></tr><tr><td>R8_ETH_ERXFCON</td><td>0x40028018</td><td>接收包过滤控制寄存器</td><td>0x00</td></tr><tr><td>R8_ETH_MACON1</td><td>0x40028019</td><td>Mac层流控制寄存器</td><td>0x00</td></tr><tr><td>R8_ETH_MACON2</td><td>0x4002801A</td><td>Mac层封包控制寄存器</td><td>0x00</td></tr><tr><td>R8_ETH_MABBIPG</td><td>0x4002801B</td><td>最小包间间隔寄存器</td><td>0x10</td></tr><tr><td>R32 ETH_TIM</td><td>0x4002801C</td><td>流控制暂停帧时间寄存器</td><td>0xXXXXXXXXX</td></tr><tr><td>R16 ETH_EPAUS</td><td>0x4002801C</td><td>流控制暂停帧时间寄存器</td><td>0xXXXXXXXX</td></tr><tr><td>R16 ETH_MAMXFL</td><td>0x4002801E</td><td>最大接收包长度寄存器</td><td>0x0000</td></tr><tr><td>R16 ETH_MIRD</td><td>0x40028020</td><td>MII读数据寄存器</td><td>0x1100</td></tr><tr><td>R32 ETH_MIWR</td><td>0x40028024</td><td>MII写寄存器</td><td>0x00000000</td></tr><tr><td>R8 ETH_MIREGADR</td><td>0x40028024</td><td>MII地址寄存器</td><td>0x00</td></tr><tr><td>R8 ETH_MISTAT</td><td>0x40028025</td><td>MII状态寄存器</td><td>0x00</td></tr><tr><td>R16 ETH_MIWR</td><td>0x40028026</td><td>MII写数据寄存器</td><td>0x0000</td></tr><tr><td>R32 ETH_MAADRL</td><td>0x40028028</td><td>MAC地址低字节寄存器</td><td>0xXXXXXXXXX</td></tr><tr><td>R16 ETH_MAADRH</td><td>0x4002802C</td><td>MAC地址高字节寄存器</td><td>0xXXXXX</td></tr></table>

#### 27.2.2.1 中断使能寄存器（R8_ETH_EIE）

<table><tr><td>位</td><td>名称</td><td>访问</td><td>描述</td><td>复位值</td></tr><tr><td>7</td><td>RB ETH EIE INTIE</td><td>RW</td><td>以太网中断使能:0:关闭中断; 1:开启中断。</td><td>0</td></tr><tr><td>6</td><td>RB ETH EIE RXIE</td><td>RW</td><td>接收完成中断使能:0:关闭中断; 1:开启中断。</td><td>0</td></tr><tr><td>5</td><td>Reserved</td><td>RO</td><td>保留。</td><td>0</td></tr><tr><td>4</td><td>RB ETH EIE LINKIE</td><td>RW</td><td>Link变化中断使能:0:关闭中断; 1:开启中断。</td><td>0</td></tr><tr><td>3</td><td>RB ETH EIE TXIE</td><td>RW</td><td>发送完成中断使能:0:关闭中断; 1:开启中断。</td><td>0</td></tr><tr><td>2</td><td>RB ETH EIE R_EN50</td><td>RW</td><td>内置的50欧姆阻抗匹配电阻使能:0:片内电阻断开;1:片内电阻连接。</td><td>0</td></tr><tr><td>1</td><td>RB ETH EIE TXERIE</td><td>RW</td><td>发送错误中断使能:0:关闭中断; 1:开启中断。</td><td>0</td></tr><tr><td>0</td><td>RB ETH EIE RXERIE</td><td>RW</td><td>接收错误中断使能:0:关闭中断; 1:开启中断。</td><td>0</td></tr></table>

#### 27.2.2.2 中断标志寄存器（R8_ETH_EIR）

<table><tr><td>位</td><td>名称</td><td>访问</td><td>描述</td><td>复位值</td></tr><tr><td>7</td><td>Reserved</td><td>R0</td><td>保留。</td><td>0</td></tr><tr><td>6</td><td>RB ETH EIR RXIF</td><td>RW1</td><td>接收完成标志。</td><td>0</td></tr><tr><td>5</td><td>Reserved</td><td>R0</td><td>保留。</td><td>0</td></tr><tr><td>4</td><td>RB ETH EIR LINKIF</td><td>RW1</td><td>Link变化标志。</td><td>0</td></tr><tr><td>3</td><td>RB ETH EIR TXIF</td><td>RW1</td><td>发送完成标志。</td><td>0</td></tr><tr><td>2</td><td>Reserved</td><td>R0</td><td>保留。</td><td>0</td></tr><tr><td>1</td><td>RB ETH EIR TXERIF</td><td>RW1</td><td>发送错误标志。</td><td>0</td></tr><tr><td>0</td><td>RB ETH EIR RXERIF</td><td>RW1</td><td>接收错误标志。</td><td>0</td></tr></table>

#### 27.2.2.3 状态寄存器（R8_ETH_ESTAT）

<table><tr><td>位</td><td>名称</td><td>访问</td><td>描述</td><td>复位值</td></tr><tr><td>7</td><td>RB ETH ESTAT_INT</td><td>RW1</td><td>中断。</td><td>0</td></tr><tr><td>6</td><td>RB ETH ESTATBUFER</td><td>RW1</td><td>Buffer 错误。</td><td>0</td></tr><tr><td>5</td><td>RB ETH ESTAT RXCRCER</td><td>R0</td><td>接收CRC出错。</td><td>0</td></tr><tr><td>4</td><td>RB ETH ESTAT RXNIBBLE</td><td>R0</td><td>接收 nibble 错误。</td><td>0</td></tr><tr><td>3</td><td>RB ETH ESTAT RXMORE</td><td>R0</td><td>接收超过设定最大数据包。</td><td>0</td></tr><tr><td>2</td><td>RB ETH ESTAT RXBUSY</td><td>R0</td><td>接收进行中。</td><td>0</td></tr><tr><td>1</td><td>RB ETH ESTAT TXABRT</td><td>R0</td><td>发送被MCU打断。</td><td>0</td></tr><tr><td>0</td><td>Reserved</td><td>R0</td><td>保留。</td><td>0</td></tr></table>

#### 27.2.2.4 PHY 模拟参数设置寄存器（R8_ETH_ECON2）

<table><tr><td>位</td><td>名称</td><td>访问</td><td>描述</td><td>复位值</td></tr><tr><td>[7:4]</td><td>Reserved</td><td>R0</td><td>保留。</td><td>0</td></tr><tr><td>[3:1]</td><td>RB_ETH_ECON2_RXRB_ETH_ECON2_MUST</td><td>RW</td><td>保留，必须写入011。</td><td>011b</td></tr><tr><td>0</td><td>RB_ETH_ECON2_TX</td><td>RW</td><td>发送端节能驱动控制：0：额定驱动；1：节能驱动。</td><td>0</td></tr></table>

#### 27.2.2.5 收发控制寄存器（R8_ETH_ECON1）

<table><tr><td>位</td><td>名称</td><td>访问</td><td>描述</td><td>复位值</td></tr><tr><td>7</td><td>RB ETH_ECON1_TXRST</td><td>RW</td><td>发送模块复位:0: 不复位; 1: 复位发送模块。</td><td>0</td></tr><tr><td>6</td><td>RB ETH_ECON1_RXRST</td><td>RW</td><td>接收模块复位:0: 不复位; 1: 复位接收模块。</td><td>0</td></tr><tr><td>[5:4]</td><td>Reserved</td><td>RO</td><td>保留。</td><td>0</td></tr><tr><td>3</td><td>RB ETH_ECON1_TXRTS</td><td>RW</td><td>发送开始,发送完成后自动清零:1: 启动发送; 0: 无动作。</td><td>0</td></tr><tr><td>2</td><td>RB ETH_ECON1_RXEN</td><td>RW</td><td>接收使能控制:0: 关闭接收; 1: 开启接收。</td><td>0</td></tr><tr><td>[1:0]</td><td>Reserved</td><td>RO</td><td>保留。</td><td>0</td></tr></table>

#### 27.2.2.6 发送 DMA 缓存区地址寄存器（R16_ETH_ETXST）

<table><tr><td>位</td><td>名称</td><td>访问</td><td>描述</td><td>复位值</td></tr><tr><td>[15:0]</td><td>R16 ETH_ETXST</td><td>RW</td><td>发送 DMA 缓冲区起始地址。低 15 位有效，地址必须 4 字节对齐。</td><td>xxxxh</td></tr></table>

#### 27.2.2.7 发送长度（R16_ETH_ETXLN）

<table><tr><td>位</td><td>名称</td><td>访问</td><td>描述</td><td>复位值</td></tr><tr><td>[15:0]</td><td>R16 ETH ETXLN</td><td>RW</td><td>发送长度。</td><td>xxxxh</td></tr></table>

#### 27.2.2.8 接收 DMA 缓冲区地址寄存器（R16_ETH_ERXST）

<table><tr><td>位</td><td>名称</td><td>访问</td><td>描述</td><td>复位值</td></tr><tr><td>[15:0]</td><td>R16 ETH_ERXST</td><td>RW</td><td>接收DMA缓冲区起始地址。低15位有效，地址必须4字节对齐。</td><td>xxxxh</td></tr></table>

#### 27.2.2.9 接收长度寄存器（R16_ETH_ERXLN）

<table><tr><td>位</td><td>名称</td><td>访问</td><td>描述</td><td>复位值</td></tr><tr><td>[15:0]</td><td>R16.eth_ERXLN</td><td>R0</td><td>接收长度。</td><td>0</td></tr></table>

27.2.2.10 Hash 表低字节寄存器（R32_ETH_HTL）  

<table><tr><td>位</td><td>名称</td><td>访问</td><td>描述</td><td>复位值</td></tr><tr><td>[31:24]</td><td>R8 ETH EHT3</td><td>RW</td><td>Hash Table 字节 3。</td><td>xxh</td></tr><tr><td>[23:16]</td><td>R8 ETH EHT2</td><td>RW</td><td>Hash Table 字节 2。</td><td>xxh</td></tr><tr><td>[15:8]</td><td>R8 ETH EHT1</td><td>RW</td><td>Hash Table 字节 1。</td><td>xxh</td></tr><tr><td>[7:0]</td><td>R8 ETH EHT0</td><td>RW</td><td>Hash Table 字节 0。</td><td>xxh</td></tr></table>

27.2.2.11 Hash 表高字节寄存器（R32_ETH_HTH）  

<table><tr><td>位</td><td>名称</td><td>访问</td><td>描述</td><td>复位值</td></tr><tr><td>[31:24]</td><td>R8 ETH EHT7</td><td>RW</td><td>Hash Table 字节 7。</td><td>xxh</td></tr><tr><td>[23:16]</td><td>R8 ETH EHT6</td><td>RW</td><td>Hash Table 字节 6。</td><td>xxh</td></tr><tr><td>[15:8]</td><td>R8 ETH EHT5</td><td>RW</td><td>Hash Table 字节 5。</td><td>xxh</td></tr><tr><td>[7:0]</td><td>R8 ETH EHT4</td><td>RW</td><td>Hash Table 字节 4。</td><td>xxh</td></tr></table>

27.2.2.12 接收过滤设置寄存器（R8_ETH_ERXFCON）  

<table><tr><td>位</td><td>名称</td><td>访问</td><td>描述</td><td>复位值</td></tr><tr><td>7</td><td>RB ETH ERXFCON_UCEN</td><td>RW</td><td>单播匹配过滤设置:0:丢弃所有单播包;1:接收目标地址匹配的包。</td><td>0</td></tr><tr><td>6</td><td>Reserved</td><td>RO</td><td>保留。</td><td>0</td></tr><tr><td>5</td><td>RB ETH ERXFCON_CRCEN</td><td>RW</td><td>CRC校验过滤设置:0:丢弃CRC错误的包;1:接收CRC错误的包。</td><td>0</td></tr><tr><td>4</td><td>RB ETH ERXFCON_EN</td><td>RW</td><td>接收过滤使能:0:关闭接收过滤功能;1:开启接收过滤功能。</td><td>0</td></tr><tr><td>3</td><td>RB ETH ERXFCON_MPEN</td><td>RW</td><td>魔法包过滤设置:0:丢弃魔法包;1:接收魔法包。</td><td>0</td></tr><tr><td>2</td><td>RB ETH ERXFCON_HTEN</td><td>RW</td><td>hash 表匹配过滤设置:0:丢弃hash 表匹配的包;1:接收hash 表匹配的包。</td><td>0</td></tr><tr><td>1</td><td>RB ETH ERXFCON_MCEN</td><td>RW</td><td>组播包匹配过滤设置:0:丢弃所有组播包;1:接收所有组播包。</td><td>0</td></tr><tr><td>0</td><td>RB ETH ERXFCON_BCEN</td><td>RW</td><td>广播包匹配过滤设置:0:丢弃所有广播包;1:接收所有广播包。</td><td>0</td></tr></table>

27.2.2.13 Mac 层流控制寄存器（R8_ETH_MACON1）  

<table><tr><td>位</td><td>名称</td><td>访问</td><td>描述</td><td>复位值</td></tr><tr><td>[7:6]</td><td>Reserved</td><td>R0</td><td>保留。</td><td>0</td></tr><tr><td>[5:4]</td><td>RB ETH MACON1_FCEN</td><td>RW</td><td>pause 帧设置,全双工下有效00:停止发送暂停帧;01:发送一次暂停帧,然后停止发送;10:周期性发送暂停帧;11:发送0 timer 暂停帧,然后停止发送。</td><td>00b</td></tr><tr><td>3</td><td>RB ETH MACON1_TXPAUS</td><td>RW</td><td>发送 pause 帧使能控制:0:不发送 pause 帧;1:使能发送。</td><td>0</td></tr><tr><td>2</td><td>RB ETH MACON1_RXPAUS</td><td>RW</td><td>接收 pause 帧使能:0:不接收 pause 帧;1:使能接收。</td><td>0</td></tr><tr><td>1</td><td>RB ETH MACON1_PASSAL</td><td>RW</td><td>控制帧设置:0:控制帧将被过滤;1:没被过滤的控制帧将写入缓存。</td><td>0</td></tr><tr><td>0</td><td>RB ETH MACON1_MARXEN</td><td>RW</td><td>MAC层接收使能:0:MAC不接收数据;1:MAC接收使能。</td><td>0</td></tr></table>

#### 27.2.2.14 Mac 层封包控制寄存器（R8_ETH_MACON2）

<table><tr><td>位</td><td>名称</td><td>访问</td><td>描述</td><td>复位值</td></tr><tr><td>[7:5]</td><td>RB_ETH_MACON2PADCFG</td><td>RW</td><td>短包填充设置:7:所有短包填充0至64字节,再4字节CRC;6:不填充短包;5:检测到字段为8100h的VLAN网络包自动填充0至64字节,否则短包填充60字节0,填充后再4字节CRC;4:不填充短包;3:所有短包填充0至64字节,再4字节CRC;2:不填充短包;1:所有短包填充0至60字节,再4字节CRC;0:不填充短包。</td><td>0</td></tr><tr><td>4</td><td>RB_ETH_MACON2_TXCRCEN</td><td>RW</td><td>发送添加CRC控制:0:硬件不填充CRC;1:硬件填充CRC。</td><td>0</td></tr><tr><td>3</td><td>RB_ETH_MACON2_PHDREN</td><td>RW</td><td>特殊4字节不参与CRC校验。</td><td>0</td></tr><tr><td>2</td><td>RB_ETH_MACON2_HFRMEN</td><td>RW</td><td>允许接收巨型帧:0:不允许接收巨型帧;1:允许接收。</td><td>0</td></tr><tr><td>1</td><td>Reserved</td><td>RO</td><td>保留。</td><td>0</td></tr><tr><td>0</td><td>RB_ETH_MACON2_FULDPX</td><td>RW</td><td>以太网络通讯模式:0:半双工;1:全双工。</td><td>0</td></tr></table>

#### 27.2.2.15 最小包间间隔寄存器（R8_ETH_MABBIPG）

<table><tr><td>位</td><td>名称</td><td>访问</td><td>描述</td><td>复位值</td></tr><tr><td>7</td><td>Reserved</td><td>R0</td><td>保留。</td><td>0</td></tr><tr><td>[6:0]</td><td>R8 ETH MABBIPG</td><td>RW</td><td>最小包间间隔字节数。</td><td>0010000b</td></tr></table>

#### 27.2.2.16 流控制暂停帧时间寄存器（R16_ETH_EPAUS）

<table><tr><td>位</td><td>名称</td><td>访问</td><td>描述</td><td>复位值</td></tr><tr><td>[15:0]</td><td>R16 ETH_EPAUS</td><td>RW</td><td>流控制暂停帧时间。</td><td>XXXXh</td></tr></table>

#### 27.2.2.17 最大接收包长度寄存器（R16_ETH_MAMXFL）

<table><tr><td>位</td><td>名称</td><td>访问</td><td>描述</td><td>复位值</td></tr><tr><td>[15:0]</td><td>R16 ETH MAMXFL</td><td>RW</td><td>最大接收包长度。</td><td>0</td></tr></table>

#### 27.2.2.18 MII 读数据寄存器（R16_ETH_MIRD）

<table><tr><td>位</td><td>名称</td><td>访问</td><td>描述</td><td>复位值</td></tr><tr><td>[15:0]</td><td>R16 ETH MIRD</td><td>RW</td><td>MII读数据寄存器。</td><td>1100h</td></tr></table>

#### 27.2.2.19 MII 地址寄存器（R8_ETH_MIREGADR）

<table><tr><td>位</td><td>名称</td><td>访问</td><td>描述</td><td>复位值</td></tr><tr><td>[7:5]</td><td>Reserved</td><td>R0</td><td>保留。</td><td>0</td></tr><tr><td>[4:0]</td><td>RB_ETH_MIREGADR_MIRDL</td><td>RW</td><td>PHY 寄存器地址。</td><td>0</td></tr></table>

#### 27.2.2.20 MII 状态寄存器（R8_ETH_MISTAT）

<table><tr><td>位</td><td>名称</td><td>访问</td><td>描述</td><td>复位值</td></tr><tr><td>[7:1]</td><td>Reserved</td><td>R0</td><td>保留。</td><td>0</td></tr><tr><td>0</td><td>R8 ETH MII STA</td><td>R0</td><td>MII寄存器操作状态:1:写MII寄存器;0:读MII寄存器</td><td>0</td></tr></table>

#### 27.2.2.21 MII 写数据寄存器（R16_ETH_MIWR）

<table><tr><td>位</td><td>名称</td><td>访问</td><td>描述</td><td>复位值</td></tr><tr><td>[15:0]</td><td>R16 ETH MIWR</td><td>WO</td><td>MII写数据寄存器。</td><td>0</td></tr></table>

#### 27.2.2.22 MAC 地址寄存器（R32_ETH_MAADRL、R16_ETH_MAADRH）

<table><tr><td>位</td><td>名称</td><td>访问</td><td>描述</td><td>复位值</td></tr><tr><td>[31:0]</td><td>R32 ETH MAADRL</td><td>RW</td><td>MAC Address 字节 1~4。</td><td>x</td></tr><tr><td>[15:0]</td><td>R16 ETH MAADR H</td><td>RW</td><td>MAC Address 字节 5~6。</td><td>x</td></tr></table>

注：内部10M物理层相关寄存器内容详见27.1.8.5内部10M物理层相关寄存器地址部分。

### 27.2.3 操作指南

#### 1.初始化

（1）、配置安全寄存器进入安全模式，打开以太网络的时钟和电源；  
（2）、开启相应的中断，可选的，启动阻抗匹配电阻；  
（3）、配置接收过滤模式、CRC 功能、MAC 地址；  
（4）、设置缓存；  
（5）、启动接收，开启中断。

#### 2.发送数据

（1）、写入 R16_ETH_ETXLN 数据长度；  
（2）、写入 R16_ETH_ETXST 数据地址；  
（3）、使能 RB_ETH_ECON1_TXRTS 标志，启动发送。

#### 3.接收数据

（1）、预先设置好接收地址，使能接收；  
（2）、使用中断或查询到接收完成状态；  
（3）、读取 R16_ETH_ERXLN 接收长度；  
（4）、更新 R16_ETH_ERXST 接收地址

具体的应用请基于以太网协议栈库使用，并参考提供的网络应用示例。

# 第 28 章 SDIO 接口（SDIO）

本章模块描述适用于CH32F2x、CH32V2x和CH32V3x微控制器全系列产品。

本章单独所述“SDIO”，是指微控制器上一个为操作 SD 卡等外部存储卡或其他设备而设计的通信接口，是微控制器的一个外设。微控制器的 SDIO 直接挂载在 AHB 总线上，由 HCLK 直接提供时钟，能实现较高的通讯速度，微控制器的 SDIO 用作 SDIO 主机，被控制的设备也被统称 SDIO 设备。应用中一般使用 SDIO 来读写 SD 卡、TF 卡或 eMMC 颗粒，或控制其他使用 SDIO 作为通讯接口的设备，比如 WiFi/4G 模块。

## 28.1 主要特征

### 28.1.1 特征

支持 SD 卡、SDIO 卡和 MMC 卡  
支持 1 位、4 位和 8 位总线  
SDIO 的时钟最快可达到 HCLK 的一半  
兼容 MMC 规范 4.5   
$\bullet$ 兼容 SD 卡规范 2.0，SDIO 卡规范 2.0  
不兼容 SPI 或 QSPI

### 28.1.2 概述

微控制器的 SDIO 支持与 SD 卡或 MMC 卡等存储器通讯，需要明确的是，SDIO 仅仅是提供一组实现SD卡、MMC卡规范单次命令传输所需要的时钟，数据和命令控制时序，各命令间的先后组合需用用户通过程序自行确定。此外，对于各种存储卡而言，SDIO 仅仅只能实现读写功能，文件系统所提供的对文件的功能需要用户自行通过程序构建文件系统而实现。

SDIO 不同于 QSPI 接口，其没有片选引脚，并多出一个 CMD 引脚，CMD 可以认为是一个特殊数据线，专门用来传输命令和响应；SDIO有1 位、4 位和 8 位三种数据线宽度可选；SDIO的时钟在配置时一般工作在 400KHz 以下的频率，当正式进行数据传输时，则可以配成 SDIO 设备所支持的最大时钟，微控制器所支持的最大 SDIO 时钟输出为 HCLK 的一半，按照协议当 SDIO 设备接收的时钟大于某个阈值时，需要降低时钟线、数据线和命令线的波形峰值，以节省波形上升和下降的耗时。

与 SD 卡不同，SDIO 卡经常指代使用 SDIO 接口的 WIFI/蓝牙模块和 4G 模块等非存储设备。如果没有特殊说明，本章叙述的内容一定适用于 SD 卡，只适用于 SDIO 卡或 MMC 卡的内容将会特别指出。本章在叙述时，优先将SD卡视为潜在操作对象，其次为 SDIO卡，最后为 MMC卡。

## 28.2 接口和时钟

SDIO 通过 AHB 总线接收 CPU 对其的控制，SDIO 的寄存器中有一个 FIFO 接口，CPU 或 DMA 通过读写 FIFO 获取或发生数据。SDIO 由 HCLK 直接供给时钟，并拥有一个中断入口，支持多种中断源。由SDIO 直接控制的引脚有 SDIO_CK、SDIO_CMD、SDIO_D[7:0]这十个，通过几个引脚连接到 SDIO 设备上。SDIO 是一个主机主导的通讯接口，所有的传输必须由微控制器发起。

### 28.2.1 外设结构

SDIO 的结构如图 28-1 所示。

![](images/54f3df443c9297c6206a78c14423836ee8654842fc284edbbc66800771c76041.jpg)  
图 28-1 SDIO 的结构图

SDIO 是微控制器作为主机直接操作 SDIO 设备的外设，其大概由 AHB 接口、时钟控制部分、CMD线控制部分，数据线控制部分和控制寄存器部分这五个模块组成，SDIO是一个半双工的外设，CPU 向控制寄存器写入需要发送的命令和数据，命令线和数据线控制模块负责将命令或数据推到 IO 上，并加上 CRC。SDIO 的数据流由通用 DMA 负责完成从 RAM 到 SDIO 的 FIFO 的搬移，SDIO 的 FIFO 有 32 个32位的大小。

#### 28.2.2 引脚及其配置

SDIO 需要配置的引脚及其模式见表 28-1。

表 28-1 SDIO 的引脚配置  

<table><tr><td>GPIO</td><td>SDIO复用功能</td><td>需要配置成的引脚模式</td><td>需要配置成的引脚速度</td></tr><tr><td>PC[8:11]</td><td>SDIO_D[0:3]</td><td>推挽复用输出</td><td>50MHz</td></tr><tr><td>PC12</td><td>SDIO_CK</td><td>推挽复用输出</td><td>50MHz</td></tr><tr><td>PD2</td><td>SDIO_CMD</td><td>推挽复用输出</td><td>50MHz</td></tr><tr><td>PB8</td><td>SDIO_D4</td><td>推挽复用输出</td><td>50MHz</td></tr><tr><td>PB9</td><td>SDIO_D5</td><td>推挽复用输出</td><td>50MHz</td></tr><tr><td>PC6</td><td>SDIO_D6</td><td>推挽复用输出</td><td>50MHz</td></tr><tr><td>PC7</td><td>SDIO_D7</td><td>推挽复用输出</td><td>50MHz</td></tr></table>

#### 28.2.3 时钟

SDIO 的时钟引脚为 SDIO_CK，其输出时钟由 HCLK 分频得到，分频系数可配置为 2-261 之间的任意整数值。SDIO设备在初始化的时候一般只支持最高 $4 0 0 \mathsf { K H z }$ 时钟的单总线模式，在初始化后，主机一般会发起切换至低电压的操作，同时将时钟提升至微控制器和 SDIO 设备双方能接受的最大时钟。不同版本和速度等级的SD卡所支持时钟速度和切换流程有所区别，用户需要自行了解。

### 28.2.4 命令状态机

SDIO的命令工作流程遵循如下图的状态机。

![](images/93471f3dd35c4829759e2e137837d9917f527cc92ea77291250559ee45c0e8c7.jpg)  
图 28-2 命令状态机

### 28.2.5 数据状态机

SDIO 的数据工作流程遵循如下图的状态机。

![](images/95ec72b0a77f4bacaaa405acf2505fe121c47fc008e43c809161db9b5035eccd.jpg)  
图 28-3 数据状态机

## 28.3 SDIO 协议简述

SDIO 上的通讯以传输为最小单位，每个传输总是以主机在 CMD 线上发送命令为起始，有的命令发送之后，SDIO设备同样会在CMD线上发送一段数据回复主机，称为“响应”，有时还会伴随着数据

的传输，数据的传输是在D线上。命令和响应的格式是固定的，各域各位的定义根据不同的命令或响应而确定。响应和数据传输需要在命令或响应结束后规定的时间内发出或停止，否则将产生超时错误。

本小节的目的是以较小的篇幅让用户对使用 SDIO 所必要的一些规范的细节有初步的了解，并不保证详尽和更新及时。微控制器的SDIO也仅仅保证其实现了 SD 2.0，SDIO 2.0及 MMC 4.5规范的硬件操作基础，对于更高版本规范定义的功能，例如双边沿采样等，并不一定支持。用户在做具体开发时应以SD规范，SDIO规范和MMC规范为依据进行SDIO交互编程。

### 28.3.1 总线时序

SD 卡的传输都是以主机发起 CMD 发起的，SD 卡可能不回复响应，可能回复短响应和长响应，有的响应后面还会伴随数据传输。而在SDIO卡或MMC卡的通讯中，还可能会有 SDIO 设备主动报中断的情况。时序如下图组所示。

![](images/c6496e37d3e7ce5b0aef2e8d3d0dac4104860c3c0938f9b1103197b0c109d466.jpg)  
图 28-4 SDIO 的无响应时序和无数据时序（以 SD 卡为例）

![](images/e016ecb41339bf9fc93fef6aedffd037819ea1434f69b077608a0eca551f2a23.jpg)  
图 28-5 SDIO 的多块读时序（以 SD 卡为例）

![](images/6ab608dbd16aa2e0ca3deb90d4b5bb004e9ac3bc9cb2d7d0e5c38dd3eabf6aea.jpg)  
图 28-6 SDIO 的多块写时序（以 SD 卡为例）

![](images/93d1433453c31c5451cac4646b37fc6646c3b8848e7890633574f1d54aa9f085.jpg)  
图 28-7 SDIO 的数据流读时序（以 SDIO 卡为例）

![](images/cad8ebe2a659460df6564abeb05cf64731ad9f0214ba2bb9d93e6acef1f70668.jpg)  
图 28-8 SDIO 的数据流写时序（以 SDIO 卡为例）

### 28.3.2 命令

SDIO传输大部分以命令（CMD）开始，SDIO主机通过命令向设备告知自己的意图。命令的格式如下表。

表 28-2 命令格式  

<table><tr><td>位/域的名称</td><td>起始位</td><td>传输位</td><td>命令索引</td><td>命令参数</td><td>CRC7</td><td>结束位</td></tr><tr><td>位次/宽度</td><td>47</td><td>46</td><td>[45:40] / 6bits</td><td>[39:8] / 32bits</td><td>[7:1] / 7bits</td><td>0</td></tr><tr><td>值</td><td>0b</td><td>1b</td><td>X</td><td>X</td><td>X</td><td>1b</td></tr></table>

命令大致有四种：

1） 广播命令（bc）：发给总线上的所有的卡，没有响应返回；  
2） 带响应的广播命令（bcr）：发给总线上的所有的卡，有响应返回；  
3） 点对点命令（ac）：发给特定的卡，但没有数据传输；  
4） 带数据传输的点对点命令（adtc）：发给特定的卡，并附带数据传输；

下面是一些常用的命令。

#### 28.3.2.1 基础命令

基础命令是 SD 卡所支持的一些较基本的功能。

表 28-3 SD 卡的基础命令  

<table><tr><td>命令索引</td><td>类型</td><td>参数</td><td>回复类型</td><td>简写</td><td>描述</td></tr><tr><td>CMDO</td><td>bc</td><td>[31:0]填充位无意义;</td><td>无</td><td>GO_IDLE_STATE</td><td>复位所有的卡到</td></tr><tr><td></td><td></td><td></td><td></td><td></td><td>IDLE状态</td></tr><tr><td>CMD2</td><td>bcr</td><td>[31:0]填充位无意义;</td><td>R2</td><td>ALL_SEND_CID</td><td>所有卡回复CID</td></tr><tr><td>CMD3</td><td>bcr</td><td>[31:0]填充位无意义;</td><td>R6</td><td>SEND_RELATIVE_ADDR</td><td>回复新的RCA</td></tr><tr><td>CMD7</td><td>ac</td><td>[31:16]RCA;[15:0]填充位无意义;</td><td>R1b选中卡</td><td>SELECT/DESELECT_CARD</td><td>选中或者取消选选中某个卡</td></tr><tr><td>CMD8</td><td>bcr</td><td>[31:12]保留;[11:8]供电;[7:0]校验模式;</td><td>R7</td><td>SEND_IF_COND</td><td>发送SD卡接口条件。</td></tr><tr><td>CMD9</td><td>ac</td><td>[31:16]RCA;[15:0]填充位无意义;</td><td>R2</td><td>SEND_CSD</td><td>要求CSD。</td></tr><tr><td>CMD10</td><td>ac</td><td>[31:16]RCA;[15:0]填充位无意义;</td><td>R2</td><td>SEND_CID</td><td>要求CID。</td></tr><tr><td>CMD11</td><td>ac</td><td>[31:0]填充位无意义;</td><td>R1</td><td>VOLATGE_SWITCH</td><td>切换到1.8V电平。</td></tr><tr><td>CMD12</td><td>ac</td><td>[31:0]填充位无意义;</td><td>R1b</td><td>STOP_TRANSMISSION</td><td>强制卡停止传输;</td></tr><tr><td>CMD13</td><td>ac</td><td>[31:16]RCA;[15]发送任务状态寄存器;[14:0]填充位;</td><td>R1</td><td>SEND_STATUS/SEND_TASK_STATUS</td><td>发送状态或任务状态寄存器</td></tr><tr><td>CMD15</td><td>ac</td><td>[31:16]RCA;[15:0]填充位无意义;</td><td>无</td><td>GO_INACTIVE_STATE</td><td>要求卡进到INACTIVE模式;</td></tr></table>

#### 28.3.2.2 擦除命令

SD 卡也是 FLASH 的结构，写入前也需要擦除 FLASH，但是 SD 卡内部集成了擦除逻辑，在执行写命令前如果发现没有擦除会自动补上擦除操作。在很多情况下特别是大批量写之前，如果主动执行擦除有助于提高效率。

表 28-4 SD 卡的擦除命令[1]  

<table><tr><td>命令索引</td><td>类型</td><td>参数</td><td>回复类型</td><td>简写</td><td>描述</td></tr><tr><td>CMD32</td><td>ac</td><td>[31:0]开始擦除的地址[2]</td><td>R1</td><td>ERASE_WR_BLK_START</td><td>设定擦除的首地址</td></tr><tr><td>CMD33</td><td>ac</td><td>[31:0]结束擦除的地址[2]</td><td>R1</td><td>ERASE_WR_BLK_END</td><td>设定擦除的尾地址</td></tr><tr><td>CMD38</td><td>ac</td><td>[31:0]擦除模式</td><td>R1b</td><td>ERASE</td><td>参数为0即普通擦</td></tr></table>

注1：这里的擦除命令是SD协议规范定义的SD卡的擦除命令，SDIO卡和MMC卡的擦除命令与此有区别；2：目前常用的SDHC/SDXC（2GB到2TB）级别的卡写入的擦除地址必须是块地址，即512字节对齐；

#### 28.3.2.3 块传输的读命令

表 28-5 SD 卡的块读命令[1]  

<table><tr><td>命令索引</td><td>类型</td><td>参数</td><td>回复类型</td><td>简写</td><td>描述</td></tr><tr><td>CMD16</td><td>ac</td><td>[31:0]块长度</td><td>R1</td><td>SET_BLOCKLEN</td><td>写入块长度,512</td></tr><tr><td>CMD17</td><td>adtc</td><td>[31:0]单个块的地址</td><td>R1</td><td>READ_SINGLE_BLOCK</td><td>设定单个读的起</td></tr><tr><td></td><td></td><td>[1]</td><td></td><td></td><td>始地址</td></tr><tr><td>CMD18</td><td>adtc</td><td>[31:0]多个块的地址[1]</td><td>R1</td><td>READ_multiple_BLOCK</td><td>设定单个读的起始地址</td></tr><tr><td>CMD19</td><td>adtc</td><td>[31:0]保留位</td><td>R1</td><td>SEND_TUNING_BLOCK</td><td>发送表示模式变化的64字节序列</td></tr><tr><td>CMD20</td><td>ac</td><td>[31:28]保留位[27:0]速度控制位</td><td>R1b</td><td>SPEED_CLASS_CONTROL</td><td>速度控制控制</td></tr><tr><td>CMD22</td><td>ac</td><td>[31:6]保留位[5:0]扩展地址</td><td>R1</td><td>ADDRESS_EXTENSION</td><td>SDUC才会用用到。</td></tr><tr><td>CMD23</td><td>ac</td><td>[31:0]块计数器</td><td>R1</td><td>SET_BLOCK_COUNT</td><td>块计数器</td></tr></table>

注：1：目前常用的 SDHC/SDXC（2GB到2TB）级别的卡写入的擦除地址必须是块地址，即512字节对齐。

#### 28.3.2.4 块传输的写命令

表 28-6 SD 卡的块读命令[1]  

<table><tr><td>命令索引</td><td>类型</td><td>参数</td><td>回复类型</td><td>简写</td><td>描述</td></tr><tr><td>CMD16</td><td>ac</td><td>[31:0]块长度</td><td>R1</td><td>SET_BLOCKLEN</td><td>写入块长度,512</td></tr><tr><td>CMD24</td><td>adtc</td><td>[31:0]块的地址[1]</td><td>R1</td><td>WRITE_BLOCK</td><td>设定单个块写的起始地址</td></tr><tr><td>CMD25</td><td>adtc</td><td>[31:0]多个块的地址[1]</td><td>R1</td><td>WRITE_multiple_BLOCK</td><td>设定多个块写的起始地址</td></tr><tr><td>CMD27</td><td>adtc</td><td>[31:0]填充位</td><td>R1</td><td>PROGRAM_CSD</td><td>对CSD可编程的字编程</td></tr></table>

注：1：目前常用的 SDHC/SDXC（2GB到2TB）级别的卡写入的擦除地址必须是块地址，即512字节对齐。

### 28.3.3 响应

响应作为 SDIO 设备对主机的必要回复，同样也是在 CMD 线上传输的，且必须在规定的时间内回复。响应传输为高位在前低位在后，响应长度和各比特各域的定义由响应的类型具体决定，但所有的响应都是由一个固定为0的起始位开始，跟着一个为固定 0 的传输方向位[1]。所有的响应最后都有一个停止位，固定为 1[1]。大致有 7 种响应，SD 卡支持 R1 / R1b / R2 / R3 / R6 / R7，SDIO 卡还支持R4 / R5，格式见下文。

#### 28.3.3.1 R1 响应

普通响应，总长 48 位，有 CRC7 校验，卡状态域为 32 位。格式如下表。

表 28-7 R1 的格式  

<table><tr><td>位/域的名称</td><td>起始位</td><td>传输位</td><td>响应索引</td><td>卡状态</td><td>CRC7</td><td>结束位</td></tr><tr><td>位次/宽度</td><td>47</td><td>46</td><td>[45:40] / 6 bits</td><td>[39:8] / 32 bits</td><td>[7:1] / 7 bits</td><td>0</td></tr><tr><td>值</td><td>0b</td><td>0b</td><td>跟随CMD索引</td><td>X</td><td>X</td><td>1b</td></tr></table>

#### 28.3.3.2 R1b 响应

R1b 的格式和 R1 一致，但是可以在响应后添加繁忙信号，即钳住数据线 D2。主机收到繁忙信号（检测到SDIO_D2为低）后需要进行相应处理。

#### 28.3.3.3 R2 响应

应对特定几个命令的响应，总长136位，CRC7校验包含在卡状态域中，卡状态域为 128位。卡状态域存放CID寄存器或CSD寄存器的值，CID寄存器一般作为 CMD2/CMD10 的回复，CSD寄存器一般作为 CMD9 的回复，CID/CSD 寄存器的具体含义见 28.4.2 设备寄存器部分。需要注意的是 R2 只会回复CID/CSD 寄存器的[127:1]段，CID/CSD[0]固定为 1 的保留位[2]被结束位占据，结束位固定也为 1。格式如下表。

表 28-8 R2 的格式  

<table><tr><td>位/域的名称</td><td>起始位</td><td>传输位</td><td>命令索引</td><td>卡状态</td><td>结束位</td></tr><tr><td>位次/宽度</td><td>135</td><td>134</td><td>[133:128] / 6 bits</td><td>[127:1] / 127 bits</td><td>0</td></tr><tr><td>值</td><td>0b</td><td>0b</td><td>111111b</td><td>CID/CSD</td><td>1b</td></tr></table>

#### 28.3.3.4 R3 响应

回复 OCR 寄存器的专用响应，总长 48 位，没有 CRC7 校验。OCR 寄存器一般作为 ACMD41 的回复。格式如下表。

表 28-9 R3 的格式  

<table><tr><td>位/域的名称</td><td>起始位</td><td>传输位</td><td>命令索引</td><td>卡状态</td><td>保留</td><td>结束位</td></tr><tr><td>位次/宽度</td><td>47</td><td>46</td><td>[45:40] / 6bits</td><td>[39:8] / 32bits</td><td>[7:1] / 7bits</td><td>0</td></tr><tr><td>值</td><td>0b</td><td>0b</td><td>111111b</td><td>0CR</td><td>1111111b</td><td>1b</td></tr></table>

#### 28.3.3.5 R4 响应

应对 CMD5 的响应回复 OCR 及相关寄存器的专用响应，总长 48 位，有 CRC7 校验。R4 应用在 SDIO卡中，格式如下表。

表 28-10 R4 的格式  

<table><tr><td>位/域的名称</td><td>位次</td><td>宽度（单位：bit）</td><td>值</td></tr><tr><td>起始位</td><td>47</td><td>1</td><td>0b</td></tr><tr><td>传输位</td><td>46</td><td>1</td><td>0b</td></tr><tr><td>保留</td><td>[45:40]</td><td>6</td><td>111111b</td></tr><tr><td>卡就绪</td><td>39</td><td>1</td><td>X</td></tr><tr><td>IO功能数目</td><td>[38:36]</td><td>3</td><td>X</td></tr><tr><td>当前寄存器</td><td>[35]</td><td>1</td><td>X</td></tr><tr><td>填充位</td><td>[34:33]</td><td>2</td><td>00b</td></tr><tr><td>S18A</td><td>32</td><td>1</td><td>X</td></tr><tr><td>IO OCR</td><td>[31:8]</td><td>24</td><td>OCR</td></tr><tr><td>CRC校验域</td><td>[7:1]</td><td>7</td><td>1111111b</td></tr><tr><td>结束位</td><td>0</td><td>1</td><td>1b</td></tr></table>

注：MMC卡R4的格式和 SDI0卡不同。

#### 28.3.3.6 R5 响应

应对 CMD5 的专用响应，总长 48 位，有 CRC7 校验。R5 应用在 SDIO 卡中，格式如下表。

表 28-11 R5 的格式  

<table><tr><td>位/域的名称</td><td>起始位</td><td>传输位</td><td>命令索引</td><td>填充位</td><td>响应格式</td><td>读写数据</td><td>CRC7</td><td>结束位</td></tr><tr><td>位次</td><td>47</td><td>46</td><td>[45:40]</td><td>[39:24]</td><td>[23:16]</td><td>[15:8]</td><td>[7:1]</td><td>0</td></tr><tr><td>宽度</td><td>1</td><td>1</td><td>6</td><td>16</td><td>8</td><td>8</td><td>7</td><td>1</td></tr><tr><td>值</td><td>0b</td><td>0b</td><td>110100b</td><td>0000h</td><td>X</td><td>X</td><td>X</td><td>1b</td></tr></table>

注：MMC卡R5的格式和 SDI0卡不同。

#### 28.3.3.7 R6 响应

回复 RCA 的专用响应，总长 48 位，有 CRC7 校验，格式如下表。

表 28-12 R6 的格式  

<table><tr><td>位/域的名称</td><td>起始位</td><td>传输位</td><td>命令索引</td><td>卡的RCA</td><td>卡状态位</td><td>CRC7</td><td>结束位</td></tr><tr><td>位次</td><td>47</td><td>46</td><td>[45:40]</td><td>[39:24]</td><td>[23:8]</td><td>[7:1]</td><td>0</td></tr><tr><td>宽度</td><td>1</td><td>1</td><td>6</td><td>16</td><td>16</td><td>7</td><td>1</td></tr><tr><td>值</td><td>0b</td><td>0b</td><td>000011b</td><td>X</td><td>X</td><td>X</td><td>1b</td></tr></table>

#### 28.3.3.8 R7 响应

应对 CMD8 的专用响应，表明支持的电压的信息，总长 48 位，有 CRC7 校验，格式如下表。

表 28-13 R7 的格式  

<table><tr><td>位/域的名称</td><td>起始位</td><td>传输位</td><td>命令索引</td><td>保留位</td><td>PCIe 1V2支持</td><td>PCIe响应</td><td>接受的电压</td><td>检查回馈</td><td>CRC7</td><td>结束位</td></tr><tr><td>位次</td><td>47</td><td>46</td><td>[45:40]</td><td>[39:22]</td><td>21</td><td>20</td><td>[19:16]</td><td>[15:8]</td><td>[7:1]</td><td>0</td></tr><tr><td>宽度</td><td>1</td><td>1</td><td>6</td><td>18</td><td>1</td><td>1</td><td>4</td><td>8</td><td>7</td><td>1</td></tr><tr><td>值</td><td>0b</td><td>0b</td><td>001000b</td><td>00000h</td><td>X</td><td>X</td><td>X</td><td>X</td><td>X</td><td>1b</td></tr></table>

### 28.3.4 数据传输

数据传输在数据线SDIO_D上进行，有 $1 / 4 / 8$ 位三种宽度，数据传输时一般是一个时钟的起始位在最前面，每个字节的高位在前，低位在后。对于 SD 卡仅仅支持的块传输模式，每个块的数据传输结束后还跟着CRC校验。MMC卡还支持数据流传输模式，此时不附带 CRC。下图为数据传输的格式。

![](images/1101d878d6a313c9936cd99d25f10bf87a15b491e7f18805dcea3131c819ca38.jpg)  
图28-9 SD卡单总线数据传输字节的格式

![](images/5ed21d663d7113e3932229ec5e4906f457c41ed3d434730f650445f9c5fcdb84.jpg)  
图 28-10 SD 卡 4 总线数据传输字节的格式

![](images/04e0de5227a3eef7eca0ac68164bc89e0ed362f127c88d6ab3dbfc66510b065d.jpg)  
图 28-11 SD 卡单总线数据传输一个 512 位字的格式

![](images/13a77bd65cd3991d881221ce99385e1000898567df5327bfcbdb1e4269eeda50.jpg)  
图 28-12 SD 卡 4 总线数据传输一个 512 位字的格式

## 28.4 应用

### 28.4.1 设备初始化和设备寄存器

#### 28.4.1.1 OCR 寄存器

操作条件寄存器（Operation Conditions Register）存储了 SD 卡的一些其接收的供电电压的信息和相关的状态位。相关的位定义如下表。

表 28-14 OCR 寄存器位定义  

<table><tr><td>位次</td><td>位定义</td><td>描述</td></tr><tr><td>0-3</td><td>保留。</td><td rowspan="9">支持的VDD电压范围,单位为伏特。</td></tr><tr><td>4</td><td>保留。</td></tr><tr><td>5</td><td>保留。</td></tr><tr><td>6</td><td>保留。</td></tr><tr><td>7</td><td>为低电压范围保留</td></tr><tr><td>8</td><td>保留</td></tr><tr><td>9</td><td>保留</td></tr><tr><td>10</td><td>保留</td></tr><tr><td>11</td><td>保留</td></tr><tr><td>12</td><td>保留</td><td></td></tr><tr><td>13</td><td>保留</td><td></td></tr><tr><td>14</td><td>保留</td><td></td></tr><tr><td>15</td><td>2.7-2.8</td><td></td></tr><tr><td>16</td><td>2.8-2.9</td><td></td></tr><tr><td>17</td><td>2.9-3.0</td><td></td></tr><tr><td>18</td><td>3.0-3.1</td><td></td></tr><tr><td>19</td><td>3.1-3.2</td><td></td></tr><tr><td>20</td><td>3.2-3.3</td><td></td></tr><tr><td>21</td><td>3.3-3.4</td><td></td></tr><tr><td>22</td><td>3.4-3.5</td><td></td></tr><tr><td>23</td><td>3.5-3.6</td><td></td></tr><tr><td>24</td><td>接受切换到1.8V</td><td></td></tr><tr><td>25-26</td><td>保留</td><td></td></tr><tr><td>27</td><td>超过2TB支持状态位</td><td></td></tr><tr><td>28</td><td>保留</td><td></td></tr><tr><td>29</td><td>UHS-II卡状态位</td><td>此位被置位表示此卡支持UHS-II接口。</td></tr><tr><td>30</td><td>卡容量状态(CCS)</td><td>此位被置位表示卡容量大于2GB。</td></tr><tr><td>31</td><td>卡上电状态位(busy)</td><td>此位在卡上电完成后被置位。上电完成后,其他位才有意义。</td></tr></table>

#### 28.4.1.2 CID 寄存器

CID 存储了一些身份识别信息。

表 28-15 CID 寄存器各位各域的定义  

<table><tr><td>位/域名称</td><td>简写</td><td>宽度</td><td>位次</td></tr><tr><td>厂商 ID</td><td>MID</td><td>8</td><td>[127:120]</td></tr><tr><td>应用 ID</td><td>OID</td><td>16</td><td>[119:104]</td></tr><tr><td>产品名称</td><td>PNM</td><td>40</td><td>[103:64]</td></tr><tr><td>产品版本</td><td>PRV</td><td>8</td><td>[63:56]</td></tr><tr><td>产品序列号</td><td>PSN</td><td>32</td><td>[55:24]</td></tr><tr><td>保留</td><td>无</td><td>4</td><td>[23:20]</td></tr><tr><td>生产日期</td><td>MDT</td><td>12</td><td>[19:8]</td></tr><tr><td>CRC7</td><td>CRC</td><td>7</td><td>[7:1]</td></tr><tr><td>固定位, 固定为 1</td><td>无</td><td>1</td><td>[0]</td></tr></table>

#### 28.4.1.3 CSD 寄存器

CSD 寄存器存储了 SD 卡的特征数据。以目前最常用的 SDHC 和 SDXC 卡最常用的第二版的 CSD 为例，各位域的定义如下表。

表 28-16 CSD 寄存器各位域的含义  

<table><tr><td>名称</td><td>简写</td><td>宽度</td><td>值</td><td>读写</td><td>位次</td></tr><tr><td>CSD 版本</td><td>CSDSTRUCTURE</td><td>2</td><td>01b</td><td>R0</td><td>[127:126]</td></tr><tr><td>保留</td><td>无</td><td>6</td><td>00_0000b</td><td>R0</td><td>[125120</td></tr><tr><td>读数访问时 间</td><td>TAAC</td><td>8</td><td>0Eh</td><td>R0</td><td>[119:112]</td></tr><tr><td>用时钟周期</td><td>NSAC</td><td>8</td><td>00h</td><td>R0</td><td>[111:104]</td></tr><tr><td>表示的读数访问时间</td><td></td><td></td><td></td><td></td><td></td></tr><tr><td>最大数据发送速度</td><td>TRAN_SPEED</td><td>8</td><td>32h5Ah0Bh2Bh</td><td>RO</td><td>[103:96]</td></tr><tr><td>卡命令类</td><td>CCC</td><td>12</td><td>X1X1101101X1b</td><td>RO</td><td>[95:84]</td></tr><tr><td>读数据块最大长度</td><td>READ_BL_LEN</td><td>4</td><td>9</td><td>RO</td><td>[83:80]</td></tr><tr><td>允许块部分读</td><td>READ_BL_PARTIAL</td><td>1</td><td>0</td><td>RO</td><td>[79]</td></tr><tr><td>块写不对齐</td><td>WRITE_BLK_MISALIGN</td><td>1</td><td>0</td><td>RO</td><td>[78]</td></tr><tr><td>块读不对齐</td><td>READ_BLK_MISALIGN</td><td>1</td><td>0</td><td>RO</td><td>[77]</td></tr><tr><td>执行的DSR</td><td>DSR_IMP</td><td>1</td><td>X</td><td>RO</td><td>[76]</td></tr><tr><td>保留</td><td>无</td><td>6</td><td>00_0000b</td><td>RO</td><td>[75:70]</td></tr><tr><td>设备大小</td><td>C_SIZE</td><td>22</td><td>XXXXXXXXh</td><td>RO</td><td>[69:48]</td></tr><tr><td>保留</td><td>无</td><td>1</td><td>0</td><td>RO</td><td>[47]</td></tr><tr><td>单块擦使能</td><td>ERASE_BLK_EN</td><td>1</td><td>1</td><td>RO</td><td>[46]</td></tr><tr><td>擦扇区尺寸</td><td>SECTOR_SIZE</td><td>7</td><td>7Fh</td><td>RO</td><td>[45:39]</td></tr><tr><td>写保护组大小</td><td>WP_GRP_SIZE</td><td>7</td><td>0000000b</td><td>RO</td><td>[38:32]</td></tr><tr><td>写保护组使能</td><td>WP_GRP_ENABLE</td><td>1</td><td>0</td><td>RO</td><td>[31]</td></tr><tr><td>保留</td><td>无</td><td>2</td><td>00b</td><td>RO</td><td>[30:29]</td></tr><tr><td>写速度因素</td><td>R2W_FACTOR</td><td>3</td><td>010b</td><td>RO</td><td>[28:26]</td></tr><tr><td>最大写数据块长度</td><td>WRITE_BL_LEN</td><td>4</td><td>9</td><td>RO</td><td>[25:22]</td></tr><tr><td>允许块部分写</td><td>WRITE_BL_PARTIAL</td><td>1</td><td>0</td><td>RO</td><td>[21]</td></tr><tr><td>保留</td><td>无</td><td>5</td><td>00000b</td><td>RO</td><td>[20:16]</td></tr><tr><td>文件格式组</td><td>FILE_FORMAT_GRP</td><td>1</td><td>0</td><td>RO</td><td>[15]</td></tr><tr><td>复制标志</td><td>TMP_WRITE_PROTECT</td><td>1</td><td>X</td><td>RW OTP</td><td>[14]</td></tr><tr><td>永久写保护</td><td>PERM_WRITE_PROTECT</td><td>1</td><td>X</td><td>RW OTP</td><td>[13]</td></tr><tr><td>临时写保护</td><td>TMP_WRITE_PROTECT</td><td>1</td><td>X</td><td>RW</td><td>[12]</td></tr><tr><td>文件格式</td><td>FILE_FOMAT</td><td>2</td><td>00b</td><td>RO</td><td>[11:10]</td></tr><tr><td>保留</td><td>无</td><td>2</td><td>00b</td><td>RO</td><td>[9:8]</td></tr><tr><td>CRC</td><td>CRC</td><td>7</td><td>0000000b</td><td>RW</td><td>[7:1]</td></tr><tr><td>未使用,必须使用1</td><td>无</td><td>1</td><td>1b</td><td>RO</td><td>[0]</td></tr></table>

#### 28.4.1.4 RCA 寄存器

相对卡地址寄存器存储了卡的地址，为 16 位，默认值为 0.

### 28.4.2 电压切换

在 SD 卡初始化后期，需要进行接口电平切换，将 SD 卡的时钟线数据线和命令线的 IO 电平切换到1.8V水平。对于压摆率不是足够优秀的器件，使用更低的电平标准有助于提升频率。但是SD 的供电电压并不一定变化，只在较新版本的协议才出现了低电压供电的 SD卡。

切换电压的步骤如下图。

![](images/2929199b14edf4d7b511f6b80e525be3014a41e44b2ae21cec66af49a3fc841a.jpg)  
图28-13 电压切换序列

### 28.4.3 时钟切换

初始化时 SD 卡的时钟只有 400KHz，在电压完成切换之后可以将时钟提升至较高的水平，例如SDHC 卡 UHS-I 模式第一档速度，总线时钟可以达到 80MHz，鉴于微控制器的 IO 输出能力，应将时钟限制在 50MHz 之内。

## 28.5 中断

### 28.5.1 SDIO 中断

SDIO支持多种中断源，如中断使能寄存器（R32_SDIO_IER）所示的有 24种情况都可以触发中断，用户可酌情自行开启。

### 28.5.2 SDIO 设备中断

需要注意的是不光 SDIO 外设可以向 CPU 报中断，外接 SDIO 卡和 MMC 卡也能向 SDIO 外设报中断。在 4 位总线模式下，中断线是 D1，在 8 位总线模式下，中断线是 D7，低电平有效。如果在空闲状态下 SDIO 检测到 D1 或 D7 为低电平，应读取 SDIO 设备的状态寄存器或中断标志寄存器及时响应中断。CPU 通过 R32_SDIO_SR:22 位可以得到 SDIO 主机是否收到中断。

## 28.6 寄存器描述

表 28-17 SDIO 相关寄存器列表  

<table><tr><td>名称</td><td>访问地址</td><td>描述</td><td>复位值</td></tr><tr><td>R32_SDIO_POWER</td><td>0x40018000</td><td>电源寄存器</td><td>0x00000000</td></tr><tr><td>R32_SDIO_CLKCR</td><td>0x40018004</td><td>时钟寄存器</td><td>0x00000000</td></tr><tr><td>R32_SDIOArg</td><td>0x40018008</td><td>命令参数寄存器</td><td>0x00000000</td></tr><tr><td>R32_SDIO_CMD</td><td>0x4001800C</td><td>命令寄存器</td><td>0x00000000</td></tr><tr><td>R32_SDIO_REQCMD</td><td>0x40018010</td><td>响应寄存器</td><td>0x00000000</td></tr><tr><td>R128_SDIO_REQX</td><td>0x40018014</td><td>响应参数寄存器</td><td>0x00000000</td></tr><tr><td>R32_SDIO_TIMER</td><td>0x40018024</td><td>数据定时寄存器</td><td>0x00000000</td></tr><tr><td>R32_SDIO_DLEN</td><td>0x40018028</td><td>传输长度寄存器</td><td>0x00000000</td></tr><tr><td>R32_SDIO_DCTLR</td><td>0x4001802C</td><td>数据控制寄存器</td><td>0x00000000</td></tr><tr><td>R32_SDIO_DCOUNT</td><td>0x40018030</td><td>传输计数寄存器</td><td>0x00000000</td></tr><tr><td>R32_SDIO_sta</td><td>0x40018034</td><td>状态寄存器</td><td>0x00000000</td></tr><tr><td>R32_SDIO_ICR</td><td>0x40018038</td><td>中断清除寄存器</td><td>0x00000000</td></tr><tr><td>R32_SDIO_MASK</td><td>0x4001803C</td><td>中断使能寄存器</td><td>0x0000_0000</td></tr><tr><td>R32_SDIO_FIFOCNT</td><td>0x40018048</td><td>FIFO计数器</td><td>0x0000_0000</td></tr><tr><td>R32_SDIO_FIFO</td><td>0x40018080</td><td>FIFO寄存器</td><td>0x0000_0000</td></tr></table>

### 28.6.1 电源寄存器（R32_SDIO_POWER）

偏移地址： $0 \times 0 0$

<table><tr><td>31</td><td>30</td><td>29</td><td>28</td><td>27</td><td>26</td><td>25</td><td>24</td><td>23</td><td>22</td><td>21</td><td>20</td><td>19</td><td>18</td><td>17</td><td>16</td></tr><tr><td colspan="16">Reserved</td></tr><tr><td>15</td><td>14</td><td>13</td><td>12</td><td>11</td><td>10</td><td>9</td><td>8</td><td>7</td><td>6</td><td>5</td><td>4</td><td>3</td><td>2</td><td>1</td><td>0</td></tr><tr><td colspan="14">Reserved</td><td colspan="2">PWRCTRL</td></tr></table>

<table><tr><td>位</td><td>名称</td><td>访问</td><td>描述</td><td>复位值</td></tr><tr><td>[31:2]</td><td>Reserved</td><td>R0</td><td>保留。</td><td>0</td></tr><tr><td>[1:0]</td><td>PWRCTRL</td><td>RW</td><td>电源检测位:00:电源关闭,时钟停止;01:保留;10:保留的上电状态;11:上电状态,卡时钟开启;</td><td>00b</td></tr></table>

### 28.6.2 时钟寄存器（R32_SDIO_CLKCR）

偏移地址： $0 \times 0 4$

<table><tr><td>31</td><td>30</td><td>29</td><td>28</td><td>27</td><td>26</td><td>25</td><td>24</td><td>23</td><td>22</td><td>21</td><td>20</td><td>19</td><td>18</td><td>17</td><td>16</td></tr><tr><td colspan="16">Reserved</td></tr><tr><td>15</td><td>14</td><td>13</td><td>12</td><td>11</td><td>10</td><td>9</td><td>8</td><td>7</td><td>6</td><td>5</td><td>4</td><td>3</td><td>2</td><td>1</td><td>0</td></tr><tr><td>Res</td><td>HWFC_EN</td><td>NEGEDGE</td><td colspan="2">WIDBUS</td><td>BYPAS S</td><td>PWRSA V</td><td>CLKEN</td><td colspan="8">CLKDIV</td></tr></table>

<table><tr><td>位</td><td>名称</td><td>访问</td><td>描述</td><td>复位值</td></tr><tr><td>[31:15]</td><td>Reserved</td><td>RO</td><td>保留。</td><td>0</td></tr><tr><td>14</td><td>HWFC_EN</td><td>RW</td><td>硬件流控使能位。此位置位后, TXFIFOE 和 RXFIFOF 信号才起作用。0: 关闭硬件流控;1: 开启硬件流控;</td><td>0</td></tr><tr><td>13</td><td>NEGEDGE</td><td>RW</td><td>SDIO_CK 相位选择位。0: 在 HCLK 的上升沿产生 SDIO_CK;1: 在 HLCK 的下降沿生产 SDIO_CK;</td><td>0</td></tr><tr><td>[12:11]</td><td>WIDBUS</td><td>RW</td><td>总线宽度配置域。00: 1 位总线模式, 使用 SDIO_D0;01: 4 位总线模式, 使用 SDIO_D[3:0];10: 8 位总线模式, 使用 SDIO_D[7:0];11: 未使用;</td><td>00b</td></tr><tr><td>10</td><td>BYPASS</td><td>RW</td><td>时钟旁路使能位。0: SDIO_CK 通过分频器分频得出;1: SDIO_CK 直接接到 HCLK/2 上;</td><td>0</td></tr><tr><td>9</td><td>PWRSAV</td><td>RW</td><td>空闲时时钟状态配置位。此位置位后, 总线空闲时关闭 SDIO_CK 输出以节省电能。0: SDIO_CK 始终输出;</td><td>0</td></tr><tr><td></td><td></td><td></td><td>1: SDIO_CK 只在需要时输出;</td><td></td></tr><tr><td>8</td><td>CLKEN</td><td>RW</td><td>时钟使能位。
0: SDIO_CK 被禁止输出;
1: SDIO_CK 被允许输出;</td><td>0</td></tr><tr><td>[7:0]</td><td>CLKDIV</td><td>RW</td><td>时钟分频系数域,此域表示 SDIO_CK 与 HCLK 的关系。SDIO_CK=HCLK/(CLKDIV+2)。注意在初始化阶段,SDIO_CK 应低于 400KHz。</td><td>0</td></tr></table>

注：时钟配置寄存器用来控制 SDI0_CK相关的参数，需要注意的是本寄存器在读写数据期间到7个周期内不能更改。

### 28.6.3 命令参数寄存器（R32_SDIO_ARG）

偏移地址： $0 \times 0 8$   

<table><tr><td>31</td><td>30</td><td>29</td><td>28</td><td>27</td><td>26</td><td>25</td><td>24</td><td>23</td><td>22</td><td>21</td><td>20</td><td>19</td><td>18</td><td>17</td><td>16</td></tr><tr><td colspan="16">CMDARG</td></tr><tr><td>15</td><td>14</td><td>13</td><td>12</td><td>11</td><td>10</td><td>9</td><td>8</td><td>7</td><td>6</td><td>5</td><td>4</td><td>3</td><td>2</td><td>1</td><td>0</td></tr><tr><td colspan="16">CMDARG</td></tr></table>

<table><tr><td>位</td><td>名称</td><td>访问</td><td>描述</td><td>复位值</td></tr><tr><td>[31:0]</td><td>CMDARG</td><td>RW</td><td>命令的参数域。此域存放的是命令中的参数，会作为命令的一部分一起发生到CMD线上。</td><td>0</td></tr></table>

### 28.6.4 命令寄存器（R32_SDIO_CMD）

偏移地址： $0 \times 0 0$   

<table><tr><td>31</td><td>30</td><td>29</td><td>28</td><td>27</td><td>26</td><td>25</td><td>24</td><td>23</td><td>22</td><td>21</td><td>20</td><td>19</td><td>18</td><td>17</td><td>16</td></tr><tr><td colspan="16">Reserved</td></tr><tr><td>15</td><td>14</td><td>13</td><td>12</td><td>11</td><td>10</td><td>9</td><td>8</td><td>7</td><td>6</td><td>5</td><td>4</td><td>3</td><td>2</td><td>1</td><td>0</td></tr><tr><td>Res</td><td>ATACM D</td><td>NIEN</td><td>ENCMD Comp I</td><td>SDI0S uspen d</td><td>CPSMEN</td><td>WAITP END</td><td>WAITI NT</td><td>WAITRESP</td><td colspan="7">CMDINDEX</td></tr></table>

<table><tr><td>位</td><td>名称</td><td>访问</td><td>描述</td><td>复位值</td></tr><tr><td>[31:15]</td><td>Reserved</td><td>R0</td><td>保留。</td><td>0</td></tr><tr><td>14</td><td>ATACMD</td><td>RW</td><td>进行 CE-ATA 命令。如果置此位,CPSM 将转到CMD61。</td><td>0</td></tr><tr><td>13</td><td>NIEN</td><td>RW</td><td>不使能 CE-ATA 中断设置位。如果置此位,CE-ATA 将不产生中断。</td><td>0</td></tr><tr><td>12</td><td>ENCMDCompl</td><td>RW</td><td>使能 CMD 完成信号使能位。如果置此位,命令完成将产生信号。</td><td>0</td></tr><tr><td>11</td><td>SDIOSuspend</td><td>RW</td><td>暂停命令发生位。如果置此位,将发送一个暂停信号。仅适用于 SDIO 卡。</td><td>0</td></tr><tr><td>10</td><td>CPSMEN</td><td>RW</td><td>CPSM(命令通道状态机)使能位。如果置此位,将使能 CPSM。</td><td>0</td></tr><tr><td>9</td><td>WAITPEND</td><td>RW</td><td>命令等待控制位。如果设置此位,发送命令前,</td><td>0</td></tr><tr><td></td><td></td><td></td><td>CPSM会等待数据传输完成。</td><td></td></tr><tr><td>8</td><td>WAITINT</td><td>RW</td><td>命令等待中断控制位。如果置此位,CPSM会关闭超时控制并等待中断产生。</td><td>0</td></tr><tr><td>[7:6]</td><td>WAITRESP</td><td>RW</td><td>响应类型等位域。此域指示 CPSM期望收到的响应类型。00:无响应,等待CMDSENT标志;01:短响应,等待CMDREND或CCRCFAIL标志;10:无响应,等待CMDSENT标志;11:长响应,等待CMDREND或CCRCFAIL标志;</td><td>00b</td></tr><tr><td>[5:0]</td><td>CMDINDEX</td><td>RW</td><td>命令索引域。此域表明具体的命令值。</td><td>0</td></tr></table>

### 28.6.5 响应寄存器（R32_SDIO_RESPCMD）

偏移地址： $0 \times 1 0$

<table><tr><td>31</td><td>30</td><td>29</td><td>28</td><td>27</td><td>26</td><td>25</td><td>24</td><td>23</td><td>22</td><td>21</td><td>20</td><td>19</td><td>18</td><td>17</td><td>16</td></tr><tr><td colspan="16">Reserved</td></tr><tr><td>15</td><td>14</td><td>13</td><td>12</td><td>11</td><td>10</td><td>9</td><td>8</td><td>7</td><td>6</td><td>5</td><td>4</td><td>3</td><td>2</td><td>1</td><td>0</td></tr><tr><td colspan="10">Reserved</td><td colspan="6">RESPCMD</td></tr></table>

<table><tr><td>位</td><td>名称</td><td>访问</td><td>描述</td><td>复位值</td></tr><tr><td>[31:6]</td><td>Reserved</td><td>R0</td><td>保留。</td><td>0</td></tr><tr><td>[5:0]</td><td>RESPCMD</td><td>R0</td><td>这个域记录了收到的响应的索引值。</td><td>0</td></tr></table>

### 28.6.6 响应参数寄存器（R128_SDIO_RESPX）

#### 28.6.6.1 响应参数寄存器高 32 位（R128_SDIO_RESP1[127:96]）

偏移地址： $0 \times 1 4$

<table><tr><td>31</td><td>30</td><td>29</td><td>28</td><td>27</td><td>26</td><td>25</td><td>24</td><td>23</td><td>22</td><td>21</td><td>20</td><td>19</td><td>18</td><td>17</td><td>16</td></tr><tr><td colspan="16">CARDSTATUS1</td></tr><tr><td>15</td><td>14</td><td>13</td><td>12</td><td>11</td><td>10</td><td>9</td><td>8</td><td>7</td><td>6</td><td>5</td><td>4</td><td>3</td><td>2</td><td>1</td><td>0</td></tr><tr><td colspan="16">CARDSTATUS1</td></tr></table>

#### 28.6.6.2 响应参数寄存器次高 32 位（R128_SDIO_RESP2[95:64]）

偏移地址： $0 \times 1 8$

<table><tr><td>31</td><td>30</td><td>29</td><td>28</td><td>27</td><td>26</td><td>25</td><td>24</td><td>23</td><td>22</td><td>21</td><td>20</td><td>19</td><td>18</td><td>17</td><td>16</td></tr><tr><td colspan="16">CARDSTATUS2</td></tr><tr><td>15</td><td>14</td><td>13</td><td>12</td><td>11</td><td>10</td><td>9</td><td>8</td><td>7</td><td>6</td><td>5</td><td>4</td><td>3</td><td>2</td><td>1</td><td>0</td></tr><tr><td colspan="16">CARDSTATUS2</td></tr></table>

#### 28.6.6.3 响应参数寄存器次低 32 位（R128_SDIO_RESP3[63:32]）

偏移地址： ${ 0 } { \times 1 } { 0 }$

<table><tr><td>31</td><td>30</td><td>29</td><td>28</td><td>27</td><td>26</td><td>25</td><td>24</td><td>23</td><td>22</td><td>21</td><td>20</td><td>19</td><td>18</td><td>17</td><td>16</td></tr><tr><td colspan="16">CARDSTATUS3</td></tr><tr><td>15</td><td>14</td><td>13</td><td>12</td><td>11</td><td>10</td><td>9</td><td>8</td><td>7</td><td>6</td><td>5</td><td>4</td><td>3</td><td>2</td><td>1</td><td>0</td></tr><tr><td colspan="16">CARDSTATUS3</td></tr></table>

#### 28.6.6.4 响应参数寄存器低 32 位（R128_SDIO_RESP4[31:0]）

偏移地址： $_ { 0 \times 2 0 }$

<table><tr><td>31</td><td>30</td><td>29</td><td>28</td><td>27</td><td>26</td><td>25</td><td>24</td><td>23</td><td>22</td><td>21</td><td>20</td><td>19</td><td>18</td><td>17</td><td>16</td></tr><tr><td colspan="16">CARDSTATUS4</td></tr><tr><td>15</td><td>14</td><td>13</td><td>12</td><td>11</td><td>10</td><td>9</td><td>8</td><td>7</td><td>6</td><td>5</td><td>4</td><td>3</td><td>2</td><td>1</td><td>0</td></tr><tr><td colspan="16">CARDSTATUS4</td></tr></table>

<table><tr><td>位</td><td>名称</td><td>访问</td><td>描述</td><td>复位值</td></tr><tr><td>[127:0]</td><td>CARDSTATUSx</td><td>R0</td><td>当响应为长响应时,整128位均表示卡状态;当响应为短响应时,低32位表示卡状态。SDIO外设先收到卡状态的最高位,并从R128_SDIO_RESX最低位开始存储。</td><td>0</td></tr></table>

### 28.6.7 数据定时寄存器（R32_SDIO_DTIMER）

偏移地址： ${ 0 } { \times 2 4 }$

<table><tr><td>31</td><td>30</td><td>29</td><td>28</td><td>27</td><td>26</td><td>25</td><td>24</td><td>23</td><td>22</td><td>21</td><td>20</td><td>19</td><td>18</td><td>17</td><td>16</td></tr><tr><td colspan="16">DATATIME</td></tr><tr><td>15</td><td>14</td><td>13</td><td>12</td><td>11</td><td>10</td><td>9</td><td>8</td><td>7</td><td>6</td><td>5</td><td>4</td><td>3</td><td>2</td><td>1</td><td>0</td></tr><tr><td colspan="16">DATATIME</td></tr></table>

<table><tr><td>位</td><td>名称</td><td>访问</td><td>描述</td><td>复位值</td></tr><tr><td>[31:0]</td><td>DATATIME</td><td>RW</td><td>数据超时时长。以SD10_CK的周期数为单位。</td><td>0</td></tr></table>

### 28.6.8 传输长度寄存器（R32_SDIO_DLEN）

偏移地址： $_ { 0 \times 2 8 }$

<table><tr><td>31</td><td>30</td><td>29</td><td>28</td><td>27</td><td>26</td><td>25</td><td>24</td><td>23</td><td>22</td><td>21</td><td>20</td><td>19</td><td>18</td><td>17</td><td>16</td></tr><tr><td colspan="7">Reserved</td><td colspan="9">DATALENGTH</td></tr><tr><td>15</td><td>14</td><td>13</td><td>12</td><td>11</td><td>10</td><td>9</td><td>8</td><td>7</td><td>6</td><td>5</td><td>4</td><td>3</td><td>2</td><td>1</td><td>0</td></tr><tr><td colspan="16">DATALENGTH</td></tr></table>

<table><tr><td>位</td><td>名称</td><td>访问</td><td>描述</td><td>复位值</td></tr><tr><td>[31:25]</td><td>Reserved</td><td>R0</td><td>保留。</td><td>0</td></tr><tr><td>[24:0]</td><td>DATALENGTH</td><td>RW</td><td>传输数据长度域。此域的值在开启传输时会被加载到传输计数器。对于块传输,此域的值必须是块大小的整数倍,块大小由SDIO设备定义,存储在R32_SDIO_DCTLR[7:4]中,常见值为512B等。</td><td>0</td></tr></table>

### 28.6.9 数据控制寄存器（R32_SDIO_DCTRL）

偏移地址： $\mathtt { 0 } \mathtt { x } 2 0$

<table><tr><td>31</td><td>30</td><td>29</td><td>28</td><td>27</td><td>26</td><td>25</td><td>24</td><td>23</td><td>22</td><td>21</td><td>20</td><td>19</td><td>18</td><td>17</td><td>16</td></tr><tr><td colspan="16">Reserved</td></tr><tr><td>15</td><td>14</td><td>13</td><td>12</td><td>11</td><td>10</td><td>9</td><td>8</td><td>7</td><td>6</td><td>5</td><td>4</td><td>3</td><td>2</td><td>1</td><td>0</td></tr><tr><td colspan="3">Reserved</td><td>SDIOEN</td><td>RWMOD</td><td>RWSTOP</td><td>RWSTART</td><td colspan="4">DBLOCKSIZE</td><td>DMAEN</td><td>DTMODE</td><td>DTDIR</td><td>DTEN</td><td></td></tr></table>

<table><tr><td>位</td><td>名称</td><td>访问</td><td>描述</td><td>复位值</td></tr><tr><td>[31:12]</td><td>Reserved</td><td>RO</td><td>保留。</td><td>0</td></tr><tr><td>11</td><td>SDIOEN</td><td>RW</td><td>SDIO使能位。此位被置位后, DPSM才能进行运转。</td><td>0</td></tr><tr><td>10</td><td>RWMOD</td><td>RW</td><td>读等待模式。0: 停止SDIO_CK控制读等待;1: 使用SDIO_D2控制读等待;</td><td>0</td></tr><tr><td>9</td><td>RWSTOP</td><td>RW</td><td>读等待停止位。如果RWSTART位被置位,那么读等待将被停止。</td><td>0</td></tr><tr><td>8</td><td>RWSTART</td><td>RW</td><td>读等待开始位。置此位将执行读等待操作。</td><td>0</td></tr><tr><td>[7:4]</td><td>DBLOCKSIZE</td><td>RW</td><td>数据块长度域。此域存储了数据块的长度,采用块传输前必须定义块传输长度。此域可以写入的值为0到1110b之间,表示的块传输长度为2BLKLEN,即0到16384字节之间。</td><td>0</td></tr><tr><td>3</td><td>DMAEN</td><td>RW</td><td>DMA使能位。置此位使能DMA。</td><td>0</td></tr><tr><td>2</td><td>DTMODE</td><td>RW</td><td>传输模式设置位。置此位设置传输模式。0: 块传输;1: 流传输;</td><td>0</td></tr><tr><td>1</td><td>DTDIR</td><td>RW</td><td>传输方向设置位。置此位设置传输方向。0: 控制器到卡;1: 卡到控制器;</td><td>0</td></tr><tr><td>0</td><td>DTEN</td><td>RW</td><td>传输使能位。置此位开始数据传输。具体流程为,置此位后,DPSM进入Wait_S或Wait_R的流程(取决于传输方向),</td><td>0</td></tr></table>

### 28.6.10 传输计数寄存器（R32_SDIO_DCOUNT）

偏移地址： $\mathtt { 0 } { \times } 3 0$

<table><tr><td>31</td><td>30</td><td>29</td><td>28</td><td>27</td><td>26</td><td>25</td><td>24</td><td>23</td><td>22</td><td>21</td><td>20</td><td>19</td><td>18</td><td>17</td><td>16</td></tr><tr><td colspan="7">Reserved</td><td colspan="9">DATACOUNT</td></tr><tr><td>15</td><td>14</td><td>13</td><td>12</td><td>11</td><td>10</td><td>9</td><td>8</td><td>7</td><td>6</td><td>5</td><td>4</td><td>3</td><td>2</td><td>1</td><td>0</td></tr><tr><td colspan="16">DATACOUNT</td></tr></table>

<table><tr><td>位</td><td>名称</td><td>访问</td><td>描述</td><td>复位值</td></tr><tr><td>[31:25]</td><td>Reserved</td><td>R0</td><td>保留。</td><td>0</td></tr><tr><td>[24:0]</td><td>DATACOUNT</td><td>R0</td><td>传输数据计数器域。在启动传输时，发送长度</td><td>0</td></tr><tr><td></td><td></td><td></td><td>寄存器的值将会被加载到此计数器中，并随着传输过程递减。</td><td></td></tr></table>

### 28.6.11 状态寄存器（R32_SDIO_STA）

偏移地址： $0 \times 3 4$

<table><tr><td colspan="7">Reserved</td><td>CEAT
AEND</td><td>SDIOI
T</td><td>RXDAV
L</td><td>TXDAV
L</td><td>RXFI
FOE</td><td>TXFIF
OE</td><td>RXFIF
OF</td><td>TXFI
FOF</td><td></td></tr><tr><td>15</td><td>14</td><td>13</td><td>12</td><td>11</td><td>10</td><td>9</td><td>8</td><td>7</td><td>6</td><td>5</td><td>4</td><td>3</td><td>2</td><td>1</td><td>0</td></tr><tr><td>RXFI
FOHF</td><td>TXFI
FOHE</td><td>RXACT</td><td>TXACT</td><td>CMDACT</td><td>DBCK
END</td><td>STBI
TERR</td><td>DATA
END</td><td>CMDS
ENT</td><td>CMDRE
ND</td><td>RXOV
ERR</td><td>TXUND
ERR</td><td>DTIME
OUT</td><td>CTIME
EOUT</td><td>DCRCF
AIL</td><td>CCRCF
AIL</td></tr></table>

<table><tr><td>位</td><td>名称</td><td>访问</td><td>描述</td><td>复位值</td></tr><tr><td>[31:24]</td><td>Reserved</td><td>R0</td><td>保留。</td><td>0</td></tr><tr><td>23</td><td>CEATAEND</td><td>R0</td><td>此位被置位时,CMD61接收到CE-ATA完成信号。</td><td>0</td></tr><tr><td>22</td><td>SDIOIT</td><td>R0</td><td>此位被置位时,SDIO收到设备中断。</td><td>0</td></tr><tr><td>21</td><td>RXDAVL</td><td>R0</td><td>此位被置位时,接收FIFO数据可用。</td><td>0</td></tr><tr><td>20</td><td>TXDAVL</td><td>R0</td><td>此位被置位时,发送FIFO中的数据可用。</td><td>0</td></tr><tr><td>19</td><td>RXFIFOE</td><td>R0</td><td>此位被置位时,接收FIFO空。</td><td>0</td></tr><tr><td>18</td><td>TXFIFOE</td><td>R0</td><td>此位被置位时,发送FIFO空。</td><td>0</td></tr><tr><td>17</td><td>RXFIFOF</td><td>R0</td><td>此位被置位时,接收FIFO满。</td><td>0</td></tr><tr><td>16</td><td>TXFIFOF</td><td>R0</td><td>此位被置位时,发送FIFO满。</td><td>0</td></tr><tr><td>15</td><td>RXFIFOHF</td><td>R0</td><td>此位被置位时,接收FIFO半满。</td><td>0</td></tr><tr><td>14</td><td>TXFIFOHE</td><td>R0</td><td>此位被置位时,发送FIFO半空。</td><td>0</td></tr><tr><td>13</td><td>RXACT</td><td>R0</td><td>此位被置位时,正在接收数据。</td><td>0</td></tr><tr><td>12</td><td>TXACT</td><td>R0</td><td>此位被置位时,正在发送数据。</td><td>0</td></tr><tr><td>11</td><td>CMDACT</td><td>R0</td><td>此位被置位时,正在传输命令。</td><td>0</td></tr><tr><td>10</td><td>DBCKEND</td><td>R0</td><td>此位被置位时,已经发送或者接受了数据块且CRC校验通过。</td><td>0</td></tr><tr><td>9</td><td>STBITERR</td><td>R0</td><td>此位被置位时,在宽总线模式下,所有数据线都没有检测到起始信号。</td><td>0</td></tr><tr><td>8</td><td>DATAEND</td><td>R0</td><td>此位被置位时,数据传输结束(传输计数器为零)。</td><td>0</td></tr><tr><td>7</td><td>CMDSENT</td><td>R0</td><td>此位被置位时,命令已经发送出去。</td><td>0</td></tr><tr><td>6</td><td>CMDREND</td><td>R0</td><td>此位被置位时,已经收到响应,CRC检验成功。</td><td>0</td></tr><tr><td>5</td><td>RXOVERR</td><td>R0</td><td>此位被置位时,接收FIFO上溢。</td><td>0</td></tr><tr><td>4</td><td>TXUNDERR</td><td>R0</td><td>此位被置位时,发送FIFO下溢。</td><td>0</td></tr><tr><td>3</td><td>DTIMEOUT</td><td>R0</td><td>此位被置位时,数据超时。</td><td>0</td></tr><tr><td>2</td><td>CTIMEOUT</td><td>R0</td><td>此位被置位时,命令超时超过了64个SDIO_CK周期。</td><td>0</td></tr><tr><td>1</td><td>DCRCFAIL</td><td>R0</td><td>此位被置位时,已经发送或者接受了数据块但CRC校验失败。</td><td>0</td></tr><tr><td>0</td><td>CCRCFAIL</td><td>R0</td><td>此位被置位时,已经收到响应,但CRC检验失</td><td>0</td></tr><tr><td></td><td></td><td></td><td>败。</td><td></td></tr></table>

### 28.6.12 中断清除寄存器（R32_SDIO_ICR）

偏移地址： ${ 0 } { \times } \ 3 { 8 }$

<table><tr><td colspan="5">Reserved</td><td>CEATA
ENDC</td><td>SDIOI
TC</td><td colspan="7">Reserved</td><td></td><td></td></tr><tr><td>15</td><td>14</td><td>13</td><td>12</td><td>11</td><td>10</td><td>9</td><td>8</td><td>7</td><td>6</td><td>5</td><td>4</td><td>3</td><td>2</td><td>1</td><td>0</td></tr><tr><td>Reserved</td><td>DBCKE
NDC</td><td>STBIT
ERRC</td><td>DATA
ENDC</td><td>CMDSEN
TC</td><td>CMDR
ENDC</td><td>RXOV
ERRC</td><td>TXUND
ERRC</td><td>DTIME
OUTC</td><td>CTIME
OUTC</td><td>DCRCF
AILC</td><td>DCRCF
AILC</td><td>CCRCF
AILC</td><td>CCRCF
AILC</td><td>CCRCF
AILC</td><td></td></tr></table>

<table><tr><td>位</td><td>名称</td><td>访问</td><td>描述</td><td>复位值</td></tr><tr><td>[31:24]</td><td>Reserved</td><td>RO</td><td>保留。</td><td>0</td></tr><tr><td>23</td><td>CEATAENDC</td><td>RW</td><td>置此位清除状态寄存器中的CEATAEND位。</td><td>0</td></tr><tr><td>22</td><td>SDIOITC</td><td>RW</td><td>置此位清除状态寄存器中的SDIOIT位。</td><td>0</td></tr><tr><td>[21:11]</td><td>Reserved</td><td>RW</td><td>保留。</td><td>0</td></tr><tr><td>10</td><td>DBCKENDC</td><td>RW</td><td>置此位清除状态寄存器中的DBCKEND位。</td><td>0</td></tr><tr><td>9</td><td>STBITERRC</td><td>RW</td><td>置此位清除状态寄存器中的STBITERR位。</td><td>0</td></tr><tr><td>8</td><td>DATAENDC</td><td>RW</td><td>置此位清除状态寄存器中的DATAEND位。</td><td>0</td></tr><tr><td>7</td><td>CMDSENTC</td><td>RW</td><td>置此位清除状态寄存器中的CMDSENT位。</td><td>0</td></tr><tr><td>6</td><td>CMDRENDC</td><td>RW</td><td>置此位清除状态寄存器中的CMDREND位。</td><td>0</td></tr><tr><td>5</td><td>RXOVERRC</td><td>RW</td><td>置此位清除状态寄存器中的RXOVERR位。</td><td>0</td></tr><tr><td>4</td><td>TXUNDERRC</td><td>RW</td><td>置此位清除状态寄存器中的TXUNDERR位。</td><td>0</td></tr><tr><td>3</td><td>DTIMEOUTC</td><td>RW</td><td>置此位清除状态寄存器中的DTIMEOUT位。</td><td>0</td></tr><tr><td>2</td><td>CTIMEOUTC</td><td>RW</td><td>置此位清除状态寄存器中的CTIMEOUT位。</td><td>0</td></tr><tr><td>1</td><td>DCRCFAILC</td><td>RW</td><td>置此位清除状态寄存器中的DCRCFAIL位。</td><td>0</td></tr><tr><td>0</td><td>CCRCFAILC</td><td>RW</td><td>置此位清除状态寄存器中的CCRCFAIL位。</td><td>0</td></tr></table>

### 28.6.13 中断使能寄存器（R32_SDIO_MASK）

偏移地址： $\mathtt { 0 } \mathtt { x } \mathtt { 3 0 }$

<table><tr><td colspan="8">Reserved</td><td>CEATAE
NDIE</td><td>SDIOI
TIE</td><td>RXDAV
LIE</td><td>TXDAV
LIE</td><td>RXFI
FOEI
E</td><td>TXFIF
OEIE</td><td>RXFIF
OFIE</td><td>TXFI
FOI
E</td></tr><tr><td>15</td><td>14</td><td>13</td><td>12</td><td>11</td><td>10</td><td>9</td><td>8</td><td>7</td><td>6</td><td>5</td><td>4</td><td>3</td><td>2</td><td>1</td><td>0</td></tr><tr><td>RXFI
FOHF
IE</td><td>TXFI
FOHE
IE</td><td>RXACT
IE</td><td>TXACT
IE</td><td>CMDACT
IE</td><td>DBCK
ENDI
E</td><td>STBI
TERR
IE</td><td>DATA
ENDI
E</td><td>CMDS
ENTI
E</td><td>CMDRE
NDIE</td><td>RXOV
ERRI
E</td><td>TXUND
ERRIE</td><td>DTIME
OUTIE</td><td>CTIME
EOUT
IE</td><td>DCRCF
AILIE</td><td>CCRCF
AILIE</td></tr></table>

<table><tr><td>位</td><td>名称</td><td>访问</td><td>描述</td><td>复位值</td></tr><tr><td>[31:24]</td><td>Reserved</td><td>R0</td><td>保留。</td><td>0</td></tr><tr><td>23</td><td>CEATAENDIE</td><td>RW</td><td>置此位后,状态寄存器在置CEATAEND位时将产生中</td><td>0</td></tr><tr><td></td><td></td><td></td><td>断。</td><td></td></tr><tr><td>22</td><td>SDIOITIE</td><td>RW</td><td>置此位后,状态寄存器在置SDIOIT位时将产生中断。</td><td>0</td></tr><tr><td>21</td><td>RXDAVLIE</td><td>RW</td><td>置此位后,状态寄存器在置RXDAVL位时将产生中断。</td><td>0</td></tr><tr><td>20</td><td>TXDAVLIE</td><td>RW</td><td>置此位后,状态寄存器在置TXDAVL位时将产生中断。</td><td>0</td></tr><tr><td>19</td><td>RXFIFOEIE</td><td>RW</td><td>置此位后,状态寄存器在置RXFIFOE位时将产生中断。</td><td>0</td></tr><tr><td>18</td><td>TXFIFOEIE</td><td>RW</td><td>置此位后,状态寄存器在置TXFIFOE位时将产生中断。</td><td>0</td></tr><tr><td>17</td><td>RXFIFOFIE</td><td>RW</td><td>置此位后,状态寄存器在置RXFIFOF位时将产生中断。</td><td>0</td></tr><tr><td>16</td><td>TXFIFOFIE</td><td>RW</td><td>置此位后,状态寄存器在置TXFIFOF位时将产生中断。</td><td>0</td></tr><tr><td>15</td><td>RXFIFOHFIE</td><td>RW</td><td>置此位后,状态寄存器在置RXFIFOHF位时将产生中断。</td><td>0</td></tr><tr><td>14</td><td>TXFIFOHEIE</td><td>RW</td><td>置此位后,状态寄存器在置TXFIFOHE位时将产生中断。</td><td>0</td></tr><tr><td>13</td><td>RXACTIE</td><td>RW</td><td>置此位后,状态寄存器在置RXACT位时将产生中断。</td><td>0</td></tr><tr><td>12</td><td>TXACTIE</td><td>RW</td><td>置此位后,状态寄存器在置TXACT位时将产生中断。</td><td>0</td></tr><tr><td>11</td><td>CMDACTIE</td><td>RW</td><td>置此位后,状态寄存器在置CMDACT位时将产生中断。</td><td>0</td></tr><tr><td>10</td><td>DBCKENDIE</td><td>RW</td><td>置此位后,状态寄存器在置DBCKEND位时将产生中断。</td><td>0</td></tr><tr><td>9</td><td>STBITERRIE</td><td>RW</td><td>置此位后,状态寄存器在置STBITERR位时将产生中断。</td><td>0</td></tr><tr><td>8</td><td>DATAENDIE</td><td>RW</td><td>置此位后,状态寄存器在置DATAEND位时将产生中断。</td><td>0</td></tr><tr><td>7</td><td>CMDSENTIE</td><td>RW</td><td>置此位后,状态寄存器在置CMDSENT位时将产生中断。</td><td>0</td></tr><tr><td>6</td><td>CMDRENDIE</td><td>RW</td><td>置此位后,状态寄存器在置CMDREND位时将产生中断。</td><td>0</td></tr><tr><td>5</td><td>RXOVERRIE</td><td>RW</td><td>置此位后,状态寄存器在置RXOVERR位时将产生中断。</td><td>0</td></tr><tr><td>4</td><td>TXUNDERRIE</td><td>RW</td><td>置此位后,状态寄存器在置TXUNDERR位时将产生中断。</td><td>0</td></tr><tr><td>3</td><td>DTIMEOUTIE</td><td>RW</td><td>置此位后,状态寄存器在置DTIMEOUT位时将产生中断。</td><td>0</td></tr><tr><td>2</td><td>CTIMEOUTIE</td><td>RW</td><td>置此位后,状态寄存器在置CTIMEOUT位时将产生中断。</td><td>0</td></tr><tr><td>1</td><td>DCRCFAILIE</td><td>RW</td><td>置此位后,状态寄存器在置DCRCFAIL位时将产生中断。</td><td>0</td></tr><tr><td>0</td><td>CCRCFAILIE</td><td>RW</td><td>置此位后,状态寄存器在置CCRCFAIL位时将产生中断。</td><td>0</td></tr></table>

### 28.6.14 FIFO 计数寄存器（R32_SDIO_FIFOCNT）

偏移地址： $0 \times 4 8$

<table><tr><td>31</td><td>30</td><td>29</td><td>28</td><td>27</td><td>26</td><td>25</td><td>24</td><td>23</td><td>22</td><td>21</td><td>20</td><td>19</td><td>18</td><td>17</td><td>16</td></tr><tr><td colspan="16">FIFOCOUNT</td></tr><tr><td>15</td><td>14</td><td>13</td><td>12</td><td>11</td><td>10</td><td>9</td><td>8</td><td>7</td><td>6</td><td>5</td><td>4</td><td>3</td><td>2</td><td>1</td><td>0</td></tr><tr><td colspan="16">FIFOCOUNT</td></tr></table>

<table><tr><td>位</td><td>名称</td><td>访问</td><td>描述</td><td>复位值</td></tr><tr><td>[31:0]</td><td>FIFOCOUNT</td><td>RO</td><td>FIFO0包含还未写入FIFO0或者还未从FIFO0读出的字(32bit)数。在设置R32_SDIO_DCTLR:R32_SDIO_DCTLR时,如果DPSM空闲,FIFO计数器将从R32_SDIO_TLEN中加载传输长度值,如果此值不能被4整除,则最后的1至3个字节会被当做一个字处理。</td><td>0</td></tr></table>

### 28.6.15 FIFO（R32_SDIO_FIFO）

偏移地址： $\mathtt { 0 } \mathtt { x } 8 0$

<table><tr><td>31</td><td>30</td><td>29</td><td>28</td><td>27</td><td>26</td><td>25</td><td>24</td><td>23</td><td>22</td><td>21</td><td>20</td><td>19</td><td>18</td><td>17</td><td>16</td></tr><tr><td colspan="16">FIFODATA</td></tr><tr><td>15</td><td>14</td><td>13</td><td>12</td><td>11</td><td>10</td><td>9</td><td>8</td><td>7</td><td>6</td><td>5</td><td>4</td><td>3</td><td>2</td><td>1</td><td>0</td></tr><tr><td colspan="16">FIFODATA</td></tr></table>

<table><tr><td>位</td><td>名称</td><td>访问</td><td>描述</td><td>复位值</td></tr><tr><td>[31:0]</td><td>FIFODATA</td><td>RW</td><td>FIFO数据域。此域即为FIFO的数据。读写此域将读出接收到的数据或者发送待发的数据。SDIO的FIFO共32个字(一个字为32位)。</td><td>0</td></tr></table>

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

# 第 31 章 电子签名（ESIG）

本章模块描述适用于CH32F2x、CH32V2x和 $_ { C H 3 2 V 3 x }$ 微控制器全系列产品。

电子签名包含了芯片识别信息：闪存区容量和唯一身份标识。它由厂家在出厂时烧录到存储器模块的系统存储区域，可以通过 SWD（SDI）或者应用代码读取。

## 31.1 功能描述

闪存区容量：指示当前芯片用户应用程序可以使用大小。

唯一身份标识：96 位二进制码，对任意一个微控制器都是唯一的，用户只能读访问不能修改。此唯一标识信息可以用作微控制器（产品）的安全密码、加解密钥、产品序列号等，用来提高系统安全机制或表明身份信息。

以上内容用户都可以按 8/16/32 位进行读访问。

## 31.2 寄存器描述

表 31-1 ESIG 相关寄存器列表  

<table><tr><td>名称</td><td>访问地址</td><td>描述</td><td>复位值</td></tr><tr><td>R16_ESIG_FLACAP</td><td>0x1FFF7E0</td><td>闪存容量寄存器</td><td>0xXXXX</td></tr><tr><td>R32_ESIG_UNI ID1</td><td>0x1FFF7E8</td><td>UID 寄存器1</td><td>0xXXXXXXXXX</td></tr><tr><td>R32_ESIG_UNI ID2</td><td>0x1FFF7EC</td><td>UID 寄存器2</td><td>0xXXXXXXXXX</td></tr><tr><td>R32_ESIG_UNI ID3</td><td>0x1FFF7F0</td><td>UID 寄存器3</td><td>0xXXXXXXXXX</td></tr></table>

### 31.2.1 闪存容量寄存器（ESIG_FLACAP）

<table><tr><td>15</td><td>14</td><td>13</td><td>12</td><td>11</td><td>10</td><td>9</td><td>8</td><td>7</td><td>6</td><td>5</td><td>4</td><td>3</td><td>2</td><td>1</td><td>0</td></tr><tr><td colspan="16">F_SIZE[15:0]</td></tr></table>

<table><tr><td>位</td><td>名称</td><td>访问</td><td>描述</td><td>复位值</td></tr><tr><td>[15:0]</td><td>F_SIZE</td><td>R0</td><td>以Kbyte为单位的闪存容量。例:0x0080=128K字节</td><td>x</td></tr></table>

### 31.2.2 UID 寄存器（ESIG_UNIID1）

<table><tr><td>31</td><td>30</td><td>29</td><td>28</td><td>27</td><td>26</td><td>25</td><td>24</td><td>23</td><td>22</td><td>21</td><td>20</td><td>19</td><td>18</td><td>17</td><td>16</td></tr><tr><td colspan="16">U_ID[31:16]</td></tr><tr><td>15</td><td>14</td><td>13</td><td>12</td><td>11</td><td>10</td><td>9</td><td>8</td><td>7</td><td>6</td><td>5</td><td>4</td><td>3</td><td>2</td><td>1</td><td>0</td></tr><tr><td colspan="16">U_ID[15:0]</td></tr></table>

<table><tr><td>位</td><td>名称</td><td>访问</td><td>描述</td><td>复位值</td></tr><tr><td>[31:0]</td><td>U_ID[31:0]</td><td>R0</td><td>UID的0-31位。</td><td>x</td></tr></table>

### 31.2.3 UID 寄存器（ESIG_UNIID2）

<table><tr><td>31</td><td>30</td><td>29</td><td>28</td><td>27</td><td>26</td><td>25</td><td>24</td><td>23</td><td>22</td><td>21</td><td>20</td><td>19</td><td>18</td><td>17</td><td>16</td></tr><tr><td colspan="16">U_ID[63:48]</td></tr><tr><td>15</td><td>14</td><td>13</td><td>12</td><td>11</td><td>10</td><td>9</td><td>8</td><td>7</td><td>6</td><td>5</td><td>4</td><td>3</td><td>2</td><td>1</td><td>0</td></tr><tr><td colspan="16">U_ID[47:32]</td></tr></table>

<table><tr><td>位</td><td>名称</td><td>访问</td><td>描述</td><td>复位值</td></tr><tr><td>[31:0]</td><td>U_ID[63:32]</td><td>R0</td><td>UID的32-63位。</td><td>x</td></tr></table>

### 31.2.4 UID 寄存器（ESIG_UNIID3）

<table><tr><td>31</td><td>30</td><td>29</td><td>28</td><td>27</td><td>26</td><td>25</td><td>24</td><td>23</td><td>22</td><td>21</td><td>20</td><td>19</td><td>18</td><td>17</td><td>16</td></tr><tr><td colspan="16">U_ID[95:80]</td></tr><tr><td>15</td><td>14</td><td>13</td><td>12</td><td>11</td><td>10</td><td>9</td><td>8</td><td>7</td><td>6</td><td>5</td><td>4</td><td>3</td><td>2</td><td>1</td><td>0</td></tr><tr><td colspan="16">U_ID[79:64]</td></tr></table>

<table><tr><td>位</td><td>名称</td><td>访问</td><td>描述</td><td>复位值</td></tr><tr><td>[31:0]</td><td>U_ID[95:64]</td><td>R0</td><td>UID的64-95位。</td><td>x</td></tr></table>

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

# 第 33 章 扩展配置（EXTEN）

## 33.1 扩展配置

系统提供了 EXTEN 扩展配置单元（EXTEN_CTR 寄存器）。该单元使用 AHB 时钟，只在系统复位执行复位动作。主要包括以下几个扩展控制位功能：

1) 调节内置电压：LDOTRIM 和 ULLDOTRIM 字段选择默认值，在调节性能和功耗时可以修改其值。  
2) PLL时钟选择：HSIPRE字段配合原有的时钟配置寄存器，提供了 HSI时钟进行分频或不分频作为PLL的输入时钟的选择。  
3) Lock-up 功能监控：LKUPEN 字段启用，将打开系统的 Lock-up 情况监控，一旦发生 Lock-up 情况，系统将进行软件复位，并将LKUPRST字段置 1，读取后可以写 1 清除此标志。  
4) USBD 模块的内置电阻及传输速度控制：USB 全速设备控制器（USBD）通过 USBDPU 字段选择是否使用内置的上拉电阻（1.5KΩ），不启用时需要在 USB 的引脚接上拉电阻（低速模式接 UD-引脚，全速模式接 $u 0 + 3 1$ 脚）。USBDLS字段配置当前USB设备速度模式。  
5) ETH 模块 10M 以太网和 1000M 以太网 RGMII 接口是否启用控制位：通过 ETH_10M 启用 10M 以太网功能，通过 ETH_RGMII 启用 1000M 以太网 RGMII 接口。  
6) 低功耗模式下 HSE 振荡控制位：通过该位可控制在低功耗模式下，HSE 是否振荡。

注：不同型号扩展寄存器位定义不同，具体细节参考配置扩展控制寄存器（EXTEN_CTR）。

## 33.2 寄存器描述

表 33-1 EXTEN 相关寄存器列表  

<table><tr><td>名称</td><td>访问地址</td><td>描述</td><td>复位值</td></tr><tr><td>R32_EXTENDED_CTR</td><td>0x40023800</td><td>配置扩展控制寄存器</td><td>0x00000A00</td></tr></table>

### 33.2.1 配置扩展控制寄存器（EXTEN_CTR）

偏移地址： $0 \times 0 0$   

<table><tr><td colspan="14">Reserved</td><td></td><td></td></tr><tr><td>15</td><td>14</td><td>13</td><td>12</td><td>11</td><td>10</td><td>9</td><td>8</td><td>7</td><td>6</td><td>5</td><td>4</td><td>3</td><td>2</td><td>1</td><td>0</td></tr><tr><td>Reserved</td><td>HSE KPL P</td><td colspan="2">LDOTRIM [1:0]</td><td colspan="2">ULLDOTRIM [1:0]</td><td>LKUP RST</td><td>LKUP EN</td><td>Reser ved</td><td>HSI PRE</td><td>ETHRG MII</td><td>ETH10 M</td><td>USBD PU</td><td>USBD LS</td><td></td><td></td></tr></table>

<table><tr><td>位</td><td>名称</td><td>访问</td><td>描述</td><td>复位值</td></tr><tr><td>[31:13]</td><td>Reserved</td><td>R0</td><td>保留。</td><td>0</td></tr><tr><td>12</td><td>HSEKPLP</td><td>RW</td><td>低功耗模式下HSE振荡控制位:1:低功耗模式下,HSE保持振荡;0:低功耗模式下,HSE不振荡。注:适用于CH32V20x_D8、CH32V20x_D8W、CH32F20x_D8W。</td><td>0</td></tr><tr><td>[11:10]</td><td>LDOTRIM[1:0]</td><td>RW</td><td>调整数字内核电压值,LDO电压值。</td><td>10b</td></tr><tr><td>[9:8]</td><td>ULLDOTRIM[1:0]</td><td>RW</td><td>调整低功耗模式下,ULLDO电压值</td><td>10b</td></tr><tr><td>7</td><td>LKUPRST</td><td>RW1</td><td>LOCKUP复位标志:</td><td>0</td></tr><tr><td></td><td></td><td></td><td>1:发生LOCKUP导致系统复位,写1清除;0:正常。</td><td></td></tr><tr><td>6</td><td>LKUPEN</td><td>RW</td><td>LOCKUP监测功能:1:启用,系统发生lock-up时执行复位并将LOCKUP_RST置位;0:不启用。</td><td>1</td></tr><tr><td>5</td><td>Reserved</td><td>RO</td><td>保留。</td><td>0</td></tr><tr><td>4</td><td>HSIPRE</td><td>RW</td><td>HSI时钟是否分频:(只能在PLL关闭下写入)1:HSI时钟作为PLL输入时钟;0:HSI时钟经2分频作为PLL输入时钟。</td><td>0</td></tr><tr><td>3</td><td>ETHRGMI I</td><td>RW</td><td>1000M以太网RGMII接口是否启用和时钟使能:1:启用1000M以太网RGMII接口并使能时钟;0:不启用并关闭时钟。注:适用于CH32F20x_D8C、CH32V30x_D8C。</td><td>0</td></tr><tr><td>2</td><td>ETH10M</td><td>RW</td><td>10M以太网是否启用和时钟使能:1:启用10M以太网功能并使能时钟;0:不启用并关闭时钟。注:适用于CH32F20x_D8C、CH32V30x_D8C、CH32V20x_D8、CH32V20x_D8W、CH32F20x_D8WCH32F20x_D8。</td><td>0</td></tr><tr><td>1</td><td>USBDPU</td><td>RW</td><td>USBD内部上拉电阻是否启用:1:启用(外部不用接上拉电阻);0:不启用(外部要接上拉电阻)。</td><td>0</td></tr><tr><td>0</td><td>USBDLS</td><td>RW</td><td>USBD工作模式选择:1:低速模式;0:全速模式。</td><td>0</td></tr></table>

# 第 34 章 调试支持（DBG）

## 34.1 主要特征

此寄存器允许在调试状态下配置MCU。包括：

支持独立看门狗（IWDG）的计数器  
$\bullet$ 支持窗口看门狗（WWDG）的计数器  
$\bullet$ 支持定时器的计数器  
$\bullet$ 支持 I2CSMBus 的超时控制  
支持 bxCAN 通信

## 34.2 寄存器描述

### 34.2.1 RISC-V 调试 MCU 配置寄存器（DBGMCU_CR）

地址： $0 \times 7 0 0$ (CSR)

<table><tr><td colspan="8">Reserved</td><td>TIM10STOP</td><td>TIM9STOP</td><td>CAN2STOP</td><td>CAN1STOP</td><td>TIM8STOP</td><td>TIM7STOP</td><td>TIM6STOP</td><td>TIM5STOP</td></tr><tr><td>15</td><td>14</td><td>13</td><td>12</td><td>11</td><td>10</td><td>9</td><td>8</td><td>7</td><td>6</td><td>5</td><td>4</td><td>3</td><td>2</td><td>1</td><td>0</td></tr><tr><td>TIM4_STOP</td><td>TIM3_STOP</td><td>TIM2_STOP</td><td>TIM1_STOP</td><td>I2C2_SMBUS_TIME_OUT</td><td>I2C1_SMBUS_TIME_OUT</td><td>WWDG_STOP</td><td>IWDG_STOP</td><td colspan="5">Reserved</td><td>STAND_BY</td><td>STOP</td><td>SLEEP</td></tr></table>

<table><tr><td>位</td><td>名称</td><td>访问</td><td>描述</td><td>复位值</td></tr><tr><td>[31:24]</td><td>Reserved</td><td>RW</td><td>保留</td><td></td></tr><tr><td>23</td><td>TIM10_STOP</td><td>RW</td><td>定时器10调试停止位。当内核进入调试状态时计数器停止工作。1: 定时器10的计数器停止工作;0: 定时器10的计数器仍然正常工作。</td><td>0</td></tr><tr><td>22</td><td>TIM9_STOP</td><td>RW</td><td>定时器9调试停止位。当内核进入调试状态时计数器停止工作。1: 定时器9的计数器停止工作;0: 定时器9的计数器仍然正常工作。</td><td>0</td></tr><tr><td>21</td><td>CAN2_STOP</td><td>RW</td><td>CAN2调试停止位。当内核进入调试状态时CAN2停止运行。1: CAN2的接收寄存器不继续接收数据;0: CAN2仍然正常运行。</td><td>0</td></tr><tr><td>20</td><td>CAN1_STOP</td><td>RW</td><td>CAN1调试停止位。当内核进入调试状态时CAN1停止运行。1: CAN1的接收寄存器不继续接收数据;0: CAN1仍然正常运行。</td><td>0</td></tr><tr><td>19</td><td>TIM8_STOP</td><td>RW</td><td>定时器8调试停止位。当内核进入调试状态时计数器停止工作。1: 定时器8的计数器停止工作;</td><td>0</td></tr><tr><td></td><td></td><td></td><td>0:定时器8的计数器仍然正常工作。</td><td></td></tr><tr><td>18</td><td>TIM7_STOP</td><td>RW</td><td>定时器7调试停止位。当内核进入调试状态时计数器停止工作。1:定时器7的计数器停止工作;0:定时器7的计数器仍然正常工作。</td><td>0</td></tr><tr><td>17</td><td>TIM6_STOP</td><td>RW</td><td>定时器6调试停止位。当内核进入调试状态时计数器停止工作。1:定时器6的计数器停止工作;0:定时器6的计数器仍然正常工作。</td><td>0</td></tr><tr><td>16</td><td>TIM5_STOP</td><td>RW</td><td>定时器5调试停止位。当内核进入调试状态时计数器停止工作。1:定时器5的计数器停止工作;0:定时器5的计数器仍然正常工作。</td><td>0</td></tr><tr><td>15</td><td>TIM4_STOP</td><td>RW</td><td>定时器4调试停止位。当内核进入调试状态时计数器停止工作。1:定时器4的计数器停止工作;0:定时器4的计数器仍然正常工作。</td><td>0</td></tr><tr><td>14</td><td>TIM3_STOP</td><td>RW</td><td>定时器3调试停止位。当内核进入调试状态时计数器停止工作。1:定时器3的计数器停止工作;0:定时器3的计数器仍然正常工作。</td><td>0</td></tr><tr><td>13</td><td>TIM2_STOP</td><td>RW</td><td>定时器2调试停止位。当内核进入调试状态时计数器停止工作。1:定时器2的计数器停止工作;0:定时器2的计数器仍然正常工作。</td><td>0</td></tr><tr><td>12</td><td>TIM1_STOP</td><td>RW</td><td>定时器1调试停止位。当内核进入调试状态时计数器停止工作。1:定时器1的计数器停止工作;0:定时器1的计数器仍然正常工作。</td><td>0</td></tr><tr><td>11</td><td>I2C2_SMBUS_TIMEOUT</td><td>RW</td><td>SMBUS超时模式调试停止位。当内核进入调试状态时停止SMBUS超时模式。1:冻结SMBUS的超时控制;0:与正常模式操作相同。</td><td>0</td></tr><tr><td>10</td><td>I2C1_SMBUS_TIMEOUT</td><td>RW</td><td>SMBUS超时模式调试停止位。当内核进入调试状态时停止SMBUS超时模式。1:冻结SMBUS的超时控制;0:与正常模式操作相同。</td><td>0</td></tr><tr><td>9</td><td>WWDG_STOP</td><td>RW</td><td>窗口看门狗调试停止位。当内核进入调试状态时调试窗口看门狗停止工作。1:窗口看门狗计数器停止工作;0:窗口看门狗计数器仍然正常工作。</td><td>0</td></tr><tr><td>8</td><td>IWDG_STOP</td><td>RW</td><td>独立看门狗调试停止位。当内核进入调试状态时看门狗停止工作。1:看门狗计数器停止工作;0:看门狗计数器仍然正常工作。</td><td>0</td></tr><tr><td>[7:3]</td><td>Reserved</td><td>RW</td><td>保留</td><td>0</td></tr><tr><td>2</td><td>STANDBY</td><td>RW</td><td>调试待机模式位。
1: (FCLK开, HCLK开)数字电路部分不下电, FCLK和 HCLK时钟由内部RL振荡器提供时钟。另外, 微控制器通过产生系统复位来退出STANDBY模式和复位是一样的;
0: (FCLK关, HCLK关)整个数字电路部分都断电。从软件的观点看, 退出STANDBY模式与复位是一样的(除了一些状态位指示了微控制器刚从STANDBY状态退出)。</td><td>0</td></tr><tr><td>1</td><td>STOP</td><td>RW</td><td>调试停止模式位。
1: (FCLK开, HCLK开)在停止模式时, FCLK和 HCLK时钟由内部RC振荡器提供。当退出停止模式时,软件必需重新配置时钟系统启动PLL, 晶振等(与配置此比特位为0时的操作一样);
0: (FCLK关, HCLK关)在停止模式时,时钟控制器禁止一切时钟(包括HCLK和FCLK)。当从STOP模式退出时,时钟的配置和复位之后的配置一样(微控制器由8MHz的内部RC振荡器(HIS)提供时钟)。因此,软件必需重新配置时钟控制系统启动PLL, 晶振等。</td><td>0</td></tr><tr><td>0</td><td>SLEEP</td><td>RW</td><td>调试睡眠模式位。
1: (FCLK开, HCLK开)在睡眠模式时, FCLK和 HCLK时钟都由原先配置好的系统时钟提供;
0: (FCLK开, HCLK关)在睡眠模式时, FCLK由原先已配置好的系统时钟提供, HCLK则关闭。由于睡眠模式不会复位已配置好的时钟系统,因此从睡眠模式退出时,软件不需要重新配置时钟系统。</td><td>0</td></tr></table>

注：适用于CH32V2x、CH32V3×系列。当系统进入调试模式之后，芯片具有某外设，调试模块则具有配置该外设的功能，调试MCU配置寄存器则具有该外设所对应的配置位。

### 34.2.2 ARM 调试 MCU 配置寄存器（DBGMCU_CR）

地址：0xE0042004

<table><tr><td colspan="9">Reserved</td><td>TIM10_STOP</td><td>TIM9_STOP</td><td>CAN2_STOP</td><td>TIM8_STOP</td><td>TIM7_STOP</td><td>TIM6_STOP</td><td>TIM5_STOP</td><td>I2C2_SMBUS_TIME_OUT</td></tr><tr><td>15</td><td>14</td><td>13</td><td>12</td><td>11</td><td>10</td><td>9</td><td>8</td><td>7</td><td>6</td><td>5</td><td>4</td><td>3</td><td>2</td><td>1</td><td>0</td><td></td></tr><tr><td>I2C1_SMBUS_TIME_OUT</td><td>CAN1_STOP</td><td>TIM4_STOP</td><td>TIM3_STOP</td><td>TIM2_STOP</td><td>TIM1_STOP</td><td>WWDG_STOP</td><td>IWDG_STOP</td><td colspan="2">TRACE_MODE</td><td>TRACE_IOEN</td><td colspan="2">Reserved</td><td>STAND_BY</td><td>STOP</td><td>SLEEP</td><td></td></tr></table>

<table><tr><td>位</td><td>名称</td><td>访问</td><td>描述</td><td>复位值</td></tr><tr><td>[31:24]</td><td>Reserved</td><td>RW</td><td>保留</td><td></td></tr><tr><td>23</td><td>TIM10_STOP</td><td>RW</td><td>定时器 10 调试停止位。当内核进入调试状态时计</td><td>0</td></tr><tr><td></td><td></td><td></td><td>数器停止工作。1:定时器10的计数器停止工作;0:定时器10的计数器仍然正常工作。</td><td></td></tr><tr><td>22</td><td>TIM9_STOP</td><td>RW</td><td>定时器9调试停止位。当内核进入调试状态时计数器停止工作。1:定时器9的计数器停止工作;0:定时器9的计数器仍然正常工作。</td><td>0</td></tr><tr><td>21</td><td>CAN2_STOP</td><td>RW</td><td>CAN2调试停止位。当内核进入调试状态时CAN2停止运行。1:CAN2的接收寄存器不继续接收数据;0:CAN2仍然正常运行。</td><td>0</td></tr><tr><td>20</td><td>TIM8_STOP</td><td>RW</td><td>定时器8调试停止位。当内核进入调试状态时计数器停止工作。1:定时器8的计数器停止工作;0:定时器8的计数器仍然正常工作。</td><td>0</td></tr><tr><td>19</td><td>TIM7_STOP</td><td>RW</td><td>定时器7调试停止位。当内核进入调试状态时计数器停止工作。1:定时器7的计数器停止工作;0:定时器7的计数器仍然正常工作。</td><td>0</td></tr><tr><td>18</td><td>TIM6_STOP</td><td>RW</td><td>定时器6调试停止位。当内核进入调试状态时计数器停止工作。1:定时器6的计数器停止工作;0:定时器6的计数器仍然正常工作。</td><td>0</td></tr><tr><td>17</td><td>TIM5_STOP</td><td>RW</td><td>定时器5调试停止位。当内核进入调试状态时计数器停止工作。1:定时器5的计数器停止工作;0:定时器5的计数器仍然正常工作。</td><td>0</td></tr><tr><td>16</td><td>I2C2_SMBUS_TIMEOUT</td><td>RW</td><td>SMBUS超时模式调试停止位。当内核进入调试状态时停止SMBUS超时模式。1:冻结SMBUS的超时控制;0:与正常模式操作相同。</td><td>0</td></tr><tr><td>15</td><td>I2C1_SMBUS_TIMEOUT</td><td>RW</td><td>SMBUS超时模式调试停止位。当内核进入调试状态时停止SMBUS超时模式。1:冻结SMBUS的超时控制;0:与正常模式操作相同。</td><td>0</td></tr><tr><td>14</td><td>CAN1_STOP</td><td>RW</td><td>CAN1调试停止位。当内核进入调试状态时CAN1停止运行。1:CAN1的接收寄存器不继续接收数据;0:CAN1仍然正常运行。</td><td>0</td></tr><tr><td>13</td><td>TIM4_STOP</td><td>RW</td><td>定时器4调试停止位。当内核进入调试状态时计数器停止工作。1:定时器4的计数器停止工作;0:定时器4的计数器仍然正常工作。</td><td>0</td></tr><tr><td>12</td><td>TIM3_STOP</td><td>RW</td><td>定时器3调试停止位。当内核进入调试状态时计数器停止工作。1:定时器3的计数器停止工作;</td><td>0</td></tr><tr><td></td><td></td><td></td><td>0:定时器3的计数器仍然正常工作。</td><td></td></tr><tr><td>11</td><td>TIM2_STOP</td><td>RW</td><td>定时器2调试停止位。当内核进入调试状态时计数器停止工作。1:定时器2的计数器停止工作;0:定时器2的计数器仍然正常工作。</td><td>0</td></tr><tr><td>10</td><td>TIM1_STOP</td><td>RW</td><td>定时器1调试停止位。当内核进入调试状态时计数器停止工作。1:定时器1的计数器停止工作;0:定时器1的计数器仍然正常工作。</td><td>0</td></tr><tr><td>9</td><td>WWDG_STOP</td><td>RW</td><td>窗口看门狗调试停止位。当内核进入调试状态时调试窗口看门狗停止工作。1:窗口看门狗计数器停止工作;0:窗口看门狗计数器仍然正常工作。</td><td>0</td></tr><tr><td>8</td><td>IWDG_STOP</td><td>RW</td><td>独立看门狗调试停止位。当内核进入调试状态时看门狗停止工作。1:看门狗计数器停止工作;0:看门狗计数器仍然正常工作。</td><td>0</td></tr><tr><td>[7:6]</td><td>TRACE_MODE</td><td>RW</td><td>跟踪引脚分配控制位。该位和TRACE_IOEN配合使用。当TRACE_IOEN=0时:xx:不分配跟踪引脚(默认状态)。当TRACE_IOEN=1时:00:跟踪引脚使用异步模式;01:跟踪引脚使用同步模式,并且数据长度为1;10:跟踪引脚使用同步模式,并且数据长度为2;11:跟踪引脚使用同步模式,并且数据长度为4。</td><td>xx</td></tr><tr><td>5</td><td>TRACE_IOEN</td><td>RW</td><td>跟踪引脚分配使能位。该位和TRACE_MODE配合使用。0:不分配跟踪引脚(默认状态);1:分配跟踪引脚。</td><td>0</td></tr><tr><td>[4:3]</td><td>Reserved</td><td>RW</td><td>保留</td><td>0</td></tr><tr><td>2</td><td>STANDBY</td><td>RW</td><td>调试待机模式位。1:(FCLK开,HCLK开)数字电路部分不下电,FCLK和HCLK时钟由内部RL振荡器提供时钟。另外,微控制器通过产生系统复位来退出STANDBY模式和复位是一样的;0:(FCLK关,HCLK关)整个数字电路部分都断电。从软件的观点看,退出STANDBY模式与复位是一样的(除了一些状态位指示了微控制器刚从STANDBY状态退出)。</td><td>0</td></tr><tr><td>1</td><td>STOP</td><td>RW</td><td>调试停止模式位。1:(FCLK开,HCLK开)在停止模式时,FCLK和HCLK时钟由内部RC振荡器提供。当退出停止模式时,软件必需重新配置时钟系统启动PLL,晶振等(与配置此比特位为0时的操作一样);0:(FCLK关,HCLK关)在停止模式时,时钟控制器</td><td>0</td></tr><tr><td></td><td></td><td></td><td>禁止一切时钟(包括HCLK和FCLK)。当从STOP模式退出时,时钟的配置和复位之后的配置一样(微控制器由8MHz的内部RC振荡器(HIS)提供时钟)。因此,软件必需重新配置时钟控制系统启动PLL,晶振等。</td><td></td></tr><tr><td>0</td><td>SLEEP</td><td>RW</td><td>调试睡眠模式位。1:(FCLK开,HCLK开)在睡眠模式时,FCLK和HCLK时钟都由原先配置好的系统时钟提供;0:(FCLK开,HCLK关)在睡眠模式时,FCLK由原先已配置好的系统时钟提供,HCLK则关闭。由于睡眠模式不会复位已配置好的时钟系统,因此从睡眠模式退出时,软件不需要重新配置时钟系统。</td><td>0</td></tr></table>

注：适用于 ${ C H 3 2 F 2 x }$ 系列。当系统进入调试模式之后，芯片具有某外设，调试模块则具有配置该外设的功能，调试MCU配置寄存器则具有该外设所对应的配置位。
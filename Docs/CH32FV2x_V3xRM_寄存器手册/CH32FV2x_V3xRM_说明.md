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


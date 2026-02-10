## CH224 CH221

### USB PD等多快充协议受电芯片

CH224支持PD3.2EPR28V、PDPPS协议握手、BC1.2等，内置LDO,待机功耗低,提供I²C接口,MCU可精细配置请求电压,读取PD协议功率。芯片集成输出电压检测,过温/过压保护等功能,外围精简,可广泛应用于各类电子设备拓展高功率输入场合。CH221为CH224的精简版。

![](images/8add6deea58c6df33c169fe5955f45889cb961c249a628cb750d92bfd230beda.jpg)  
应用框图 \ Block Diagram

#### 产品特点 \ Features

> 支持4V至30V输入电压  
> 支持PD3.2 EPR、AVS、PPS、SPR协议及BC1.2等升压快充协议  
> 支持eMarker模拟，自动检测VCONN  
支持多种方式动态调整请求电压  
支持400KHz速率I²C通信  
> 芯片内置高压LDO，静态功耗低   
> 单芯片集成度高，外围精简，成本低  
> 内置过压保护模块OVP  
封装形式：DFN10、ESSOP10、QFN20、SOT23-6

选型指南 \ Model Selection Guide  

<table><tr><td rowspan="2">型号</td><td colspan="3">PD协议</td><td colspan="2">A口协议</td><td rowspan="2">功耗(20V时)</td><td rowspan="2">I²C配置</td><td rowspan="2">封装尺寸</td></tr><tr><td>EPR</td><td>PPS</td><td>其他PD协议</td><td>QC3.0</td><td>其他特点</td></tr><tr><td>CH224Q</td><td>28V</td><td>✓</td><td>✓</td><td>✓</td><td>✓</td><td>30mW</td><td>✓</td><td>小(DFN10 2*2)</td></tr><tr><td>CH224A</td><td>28V</td><td>✓</td><td>✓</td><td>✓</td><td>✓</td><td>30mW</td><td>✓</td><td>大(ESSOP10)</td></tr><tr><td>CH224D</td><td>×</td><td>✓</td><td>✓</td><td>✓</td><td>✓</td><td>100mW</td><td>×</td><td>中(QFN20 3*3)</td></tr><tr><td>CH224K</td><td>×</td><td>×</td><td>✓</td><td>×</td><td>✓</td><td>328mW</td><td>×</td><td>大(ESSOP10)</td></tr><tr><td>CH221K</td><td>×</td><td>×</td><td>✓</td><td>×</td><td>×</td><td>328mW</td><td>×</td><td>中(SOT23-6)</td></tr></table>

### 其他多快充协议受电芯片\Others

CH223：支持USBPD3.0/2.0快充协议，可通过I²C接口获取PD相关数据、修改电压档位，提供两个可控高压开漏输出引脚，FB引脚支持增量开环电流调节模式，可用于DC-DC或外置电压调节器。  
CH220：USB PD快充协议转发芯片，可在两个Type-C接口间进行USB PD协议转发，单芯片支持电压和电流限制、功率扣除和过流保护。

### USB PD等多快充协议芯片

CH235S为ESSOP10封装的Type-C单口快充协议芯片，支持PD3.0/2.0、PPS等Type-C快充协议和BC1.2等Type-A快充协议。CH235S支持TL431等各类电压基准或DC-DC系统的FB灌电流调节，支持线缆补偿，集成VBUS检测与放电功能，并且提供欠压、过压、过流、过温保护功能。

CH238P内置NMOS,输入电压提升至20V,支持FB灌电流调节和光耦反馈,QFN16封装。


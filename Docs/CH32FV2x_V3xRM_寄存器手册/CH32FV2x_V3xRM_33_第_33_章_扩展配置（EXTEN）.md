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


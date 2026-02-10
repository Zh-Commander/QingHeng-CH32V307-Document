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


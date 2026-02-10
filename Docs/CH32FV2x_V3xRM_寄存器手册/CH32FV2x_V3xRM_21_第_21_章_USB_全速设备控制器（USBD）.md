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


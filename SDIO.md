# CH32V307 SDIO存储卡接口使用指南

## 1. 外设概述

SDIO（Secure Digital Input Output）是CH32V307的多功能存储卡接口，支持SD卡、SDIO卡和MMC/eMMC卡。主要特性包括：

- **多卡类型支持**：SD卡（标准/高容量）、SDIO卡、MMC/eMMC卡
- **多种工作模式**：轮询模式、DMA模式、中断模式
- **灵活总线宽度**：支持1位、4位、8位数据总线
- **高速传输**：支持高时钟频率传输
- **完整协议栈**：内置SD/MMC协议处理器
- **FATFS集成**：支持FAT32文件系统
- **错误检测**：内置CRC校验和错误检测机制

## 2. 关键代码片段

### 2.1 SDIO GPIO引脚配置

```c
// SDIO引脚映射（CH32V307）
// 数据线：PC8(D0), PC9(D1), PC10(D2), PC11(D3), PB8(D4), PB9(D5), PC6(D6), PC7(D7)
// 时钟线：PC12(SCK/SDCK)
// 命令线：PD2(CMD)

void SDIO_GPIO_Init(void)
{
    GPIO_InitTypeDef GPIO_InitStructure = {0};

    // 使能GPIO和SDIO时钟
    RCC_APB2PeriphClockCmd(RCC_APB2Periph_GPIOC | RCC_APB2Periph_GPIOD, ENABLE);
    RCC_AHBPeriphClockCmd(RCC_AHBPeriph_SDIO | RCC_AHBPeriph_DMA2, ENABLE);

    // 配置数据线PC8-PC11为复用推挽输出
    GPIO_InitStructure.GPIO_Pin = GPIO_Pin_8 | GPIO_Pin_9 | GPIO_Pin_10 | GPIO_Pin_11;
    GPIO_InitStructure.GPIO_Mode = GPIO_Mode_AF_PP;
    GPIO_InitStructure.GPIO_Speed = GPIO_Speed_50MHz;
    GPIO_Init(GPIOC, &GPIO_InitStructure);

    // 配置时钟线PC12
    GPIO_InitStructure.GPIO_Pin = GPIO_Pin_12;
    GPIO_InitStructure.GPIO_Mode = GPIO_Mode_AF_PP;
    GPIO_InitStructure.GPIO_Speed = GPIO_Speed_50MHz;
    GPIO_Init(GPIOC, &GPIO_InitStructure);

    // 配置命令线PD2
    GPIO_InitStructure.GPIO_Pin = GPIO_Pin_2;
    GPIO_InitStructure.GPIO_Mode = GPIO_Mode_AF_PP;
    GPIO_InitStructure.GPIO_Speed = GPIO_Speed_50MHz;
    GPIO_Init(GPIOD, &GPIO_InitStructure);

    // 注意：除时钟线外，所有数据线和命令线需要外部上拉47K电阻
}
```

### 2.2 SDIO外设初始化

```c
// SDIO初始化配置
void SDIO_Init_Configuration(void)
{
    SDIO_InitTypeDef SDIO_InitStructure = {0};

    // 初始化时钟分频（识别模式时钟必须<400kHz）
    SDIO_InitStructure.SDIO_ClockDiv = SDIO_INIT_CLK_DIV;  // 例如0xB2
    SDIO_InitStructure.SDIO_ClockEdge = SDIO_ClockEdge_Rising;
    SDIO_InitStructure.SDIO_ClockBypass = SDIO_ClockBypass_Disable;
    SDIO_InitStructure.SDIO_ClockPowerSave = SDIO_ClockPowerSave_Disable;
    SDIO_InitStructure.SDIO_BusWide = SDIO_BusWide_1b;      // 初始为1位总线
    SDIO_InitStructure.SDIO_HardwareFlowControl = SDIO_HardwareFlowControl_Disable;

    SDIO_Init(&SDIO_InitStructure);
    SDIO_SetPowerState(SDIO_PowerState_ON);  // 使能SDIO电源
    SDIO_ClockCmd(ENABLE);                   // 使能SDIO时钟
}

// 设置SDIO时钟频率
void SDIO_Clock_Set(u8 clkdiv)
{
    SDIO_ClockCmd(DISABLE);
    SDIO_InitStructure.SDIO_ClockDiv = clkdiv;
    SDIO_Init(&SDIO_InitStructure);
    SDIO_ClockCmd(ENABLE);
}
```

### 2.3 SD卡初始化流程

```c
// SD卡初始化函数
SD_Error SD_Init(void)
{
    SD_Error errorstatus = SD_OK;
    u32 count = 0;

    // 1. 初始化GPIO和SDIO外设
    SD_LowLevel_Init();

    // 2. 上电序列
    errorstatus = SD_PowerON();
    if(errorstatus != SD_OK) return errorstatus;

    // 3. 识别卡
    errorstatus = SD_InitializeCards();
    if(errorstatus != SD_OK) return errorstatus;

    // 4. 获取卡信息
    errorstatus = SD_GetCardInfo(&SDCardInfo);
    if(errorstatus != SD_OK) return errorstatus;

    // 5. 选择卡
    errorstatus = SD_SelectDeselect((u32)(SDCardInfo.RCA << 16));
    if(errorstatus != SD_OK) return errorstatus;

    // 6. 配置总线宽度（4位模式）
    errorstatus = SD_EnableWideBusOperation(SDIO_BusWide_4b);
    if(errorstatus != SD_OK) return errorstatus;

    // 7. 设置高速模式时钟
    SDIO_Clock_Set(SDIO_TRANSFER_CLK_DIV);  // 例如0x00

    return SD_OK;
}
```

### 2.4 eMMC卡初始化

```c
// eMMC卡初始化（与SD卡类似但有扩展CSD）
SD_Error eMMC_Init(void)
{
    SD_Error errorstatus = SD_OK;

    // 1. 基本初始化（与SD卡相同）
    errorstatus = SD_Init();
    if(errorstatus != SD_OK) return errorstatus;

    // 2. 读取扩展CSD寄存器
    u8 ext_csd[512];
    errorstatus = eMMC_ReadExtCSD(ext_csd);
    if(errorstatus != SD_OK) return errorstatus;

    // 3. 解析扩展CSD获取eMMC特性
    E.CardType = ext_csd[EXT_CSD_DEVICE_TYPE];
    E.eMMC_ExtCsd.SecCount = (ext_csd[EXT_CSD_SEC_COUNT+0]<<0) |
                             (ext_csd[EXT_CSD_SEC_COUNT+1]<<8) |
                             (ext_csd[EXT_CSD_SEC_COUNT+2]<<16)|
                             (ext_csd[EXT_CSD_SEC_COUNT+3]<<24);

    // 4. 切换传输模式（如果需要）
    errorstatus = eMMC_Change_Tran_Mode();

    return errorstatus;
}
```

### 2.5 数据读写操作

```c
// 读取单个块（512字节）
SD_Error SD_ReadBlock(u8 *buf, long long addr, u16 blksize)
{
    SD_Error errorstatus = SD_OK;

    // 检查参数
    if(buf == NULL) return SD_INVALID_PARAMETER;
    if(blksize > 2048) return SD_INVALID_PARAMETER;

    // 发送读取命令
    SDIO_CmdInitStructure.SDIO_Argument = (u32)addr;
    SDIO_CmdInitStructure.SDIO_CmdIndex = SD_CMD_READ_SINGLE_BLOCK;
    SDIO_CmdInitStructure.SDIO_Response = SDIO_Response_Short;
    SDIO_CmdInitStructure.SDIO_Wait = SDIO_Wait_No;
    SDIO_CmdInitStructure.SDIO_CPSM = SDIO_CPSM_Enable;
    SDIO_SendCommand(&SDIO_CmdInitStructure);

    // 等待命令响应
    errorstatus = CmdResp1Error(SD_CMD_READ_SINGLE_BLOCK);
    if(errorstatus != SD_OK) return errorstatus;

    // 配置数据读取
    SDIO_DataInitStructure.SDIO_DataTimeOut = SD_DATATIMEOUT;
    SDIO_DataInitStructure.SDIO_DataLength = blksize;
    SDIO_DataInitStructure.SDIO_DataBlockSize = SDIO_DataBlockSize_512b;
    SDIO_DataInitStructure.SDIO_TransferDir = SDIO_TransferDir_ToSDIO;
    SDIO_DataInitStructure.SDIO_TransferMode = SDIO_TransferMode_Block;
    SDIO_DataInitStructure.SDIO_DPSM = SDIO_DPSM_Enable;
    SDIO_DataConfig(&SDIO_DataInitStructure);

    // 读取数据
    for(u16 i = 0; i < blksize/4; i++)
    {
        while(!SDIO_GetFlagStatus(SDIO_FLAG_RXDAVL));
        *((u32*)buf) = SDIO_ReadData();
        buf += 4;
    }

    // 检查数据状态
    while(SDIO_GetFlagStatus(SDIO_FLAG_RXACT));
    if(SDIO_GetFlagStatus(SDIO_FLAG_DTIMEOUT)) return SD_DATA_TIMEOUT;
    if(SDIO_GetFlagStatus(SDIO_FLAG_DCRCFAIL)) return SD_DATA_CRC_FAIL;

    return SD_OK;
}

// 写入单个块
SD_Error SD_WriteBlock(u8 *buf, long long addr, u16 blksize)
{
    // 类似读取流程，方向相反
    // ...
}
```

### 2.6 DMA模式数据传输

```c
// DMA模式配置
void SD_DMA_Init(void)
{
    DMA_InitTypeDef DMA_InitStructure = {0};

    // 使能DMA2时钟
    RCC_AHBPeriphClockCmd(RCC_AHBPeriph_DMA2, ENABLE);

    // 配置DMA通道4（SDIO接收）
    DMA_InitStructure.DMA_PeripheralBaseAddr = (u32)&SDIO->FIFO;
    DMA_InitStructure.DMA_MemoryBaseAddr = (u32)Buffer;
    DMA_InitStructure.DMA_DIR = DMA_DIR_PeripheralSRC;
    DMA_InitStructure.DMA_BufferSize = BufferSize;
    DMA_InitStructure.DMA_PeripheralInc = DMA_PeripheralInc_Disable;
    DMA_InitStructure.DMA_MemoryInc = DMA_MemoryInc_Enable;
    DMA_InitStructure.DMA_PeripheralDataSize = DMA_PeripheralDataSize_Word;
    DMA_InitStructure.DMA_MemoryDataSize = DMA_MemoryDataSize_Word;
    DMA_InitStructure.DMA_Mode = DMA_Mode_Normal;
    DMA_InitStructure.DMA_Priority = DMA_Priority_High;
    DMA_InitStructure.DMA_M2M = DMA_M2M_Disable;
    DMA_Init(DMA2_Channel4, &DMA_InitStructure);

    // 使能DMA传输完成中断
    DMA_ITConfig(DMA2_Channel4, DMA_IT_TC, ENABLE);
    NVIC_EnableIRQ(DMA2_Channel4_5_IRQn);

    // 使能SDIO DMA请求
    SDIO_DMACmd(ENABLE);
}
```

### 2.7 FATFS磁盘驱动实现

```c
// diskio.c中的FATFS底层驱动
DSTATUS disk_initialize(BYTE pdrv)
{
    DSTATUS stat = STA_NOINIT;

    if(pdrv) return STA_NOINIT;  // 只支持驱动器0

    // 初始化SD卡
    if(SD_Init() == SD_OK)
    {
        stat &= ~STA_NOINIT;
    }

    return stat;
}

DRESULT disk_read(BYTE pdrv, BYTE *buff, LBA_t sector, UINT count)
{
    DRESULT res = RES_ERROR;

    if(pdrv || !buff) return RES_PARERR;

    // 转换为字节偏移
    u32 sector_addr = sector * 512;

    // 读取数据
    for(UINT i = 0; i < count; i++)
    {
        if(SD_ReadDisk(buff, sector_addr + i, 1) != 0)
            return RES_ERROR;
        buff += 512;
    }

    return RES_OK;
}

DRESULT disk_write(BYTE pdrv, const BYTE *buff, LBA_t sector, UINT count)
{
    // 类似disk_read，方向相反
    // ...
}
```

## 3. 配置数据结构

### 3.1 SD卡信息结构

```c
typedef struct {
    SD_CSD SD_csd;          // CSD寄存器内容
    SD_CID SD_cid;          // CID寄存器内容
    long long CardCapacity; // 卡容量（字节）
    u32 CardBlockSize;      // 块大小（字节）
    u16 RCA;               // 相对卡地址
    u8 CardType;           // 卡类型
} SD_CardInfo;

// CSD寄存器结构
typedef struct {
    u8 CSDStruct;          // CSD结构
    u8 SysSpecVersion;     // 系统规格版本
    u8 Reserved1;          // 保留
    u8 TAAC;               // 数据读取访问时间
    u8 NSAC;               // 数据读取时间在CLK周期
    u8 MaxBusClkFrec;      // 最大总线时钟频率
    u16 CardComdClasses;   // 卡命令类
    u8 RdBlockLen;         // 最大读取数据块长度
    u8 PartBlockRead;      // 部分块读取允许
    u8 WrBlockMisalign;    // 写块错位
    u8 RdBlockMisalign;    // 读块错位
    u8 DSRImpl;            // DSR实现
    u32 CSize;            // 设备大小
    // ... 更多字段
} SD_CSD;
```

### 3.2 eMMC卡信息结构

```c
typedef struct {
    eMMC_CSD eMMC_csd;            // CSD寄存器
    eMMC_EXT_CSD eMMC_ExtCsd;     // 扩展CSD寄存器
    eMMC_CID eMMC_cid;            // CID寄存器
    u32 SectorNums;               // 扇区数量
    u32 CardBlockSize;            // 块大小
    u16 RCA;                     // 相对卡地址
    u8 CardType;                 // 卡类型
} eMMC_CardInfo;

// eMMC扩展CSD结构
typedef struct {
    u32 SecCount;                // 扇区数量
    u8 SupportedCardCommand;     // 支持的卡命令
    u8 ExtendedCSDRevision;      // 扩展CSD版本
    u8 CommandSet;              // 命令集
    u8 CommandSetRevision;      // 命令集版本
    u8 PowerClass;              // 电源类
    u8 HighSpeedTiming;         // 高速时序
    u8 BusWidthMode;            // 总线宽度模式
    // ... 更多字段
} eMMC_EXT_CSD;
```

### 3.3 SDIO命令结构

```c
// SDIO命令索引定义
#define SD_CMD_GO_IDLE_STATE           0   // CMD0
#define SD_CMD_ALL_SEND_CID            2   // CMD2
#define SD_CMD_SEND_RELATIVE_ADDR      3   // CMD3
#define SD_CMD_SET_DSR                 4   // CMD4
#define SD_CMD_SWITCH_FUNC             6   // CMD6
#define SD_CMD_SELECT_DESELECT_CARD    7   // CMD7
#define SD_CMD_SEND_IF_COND            8   // CMD8
#define SD_CMD_SEND_CSD                9   // CMD9
#define SD_CMD_SEND_CID                10  // CMD10
#define SD_CMD_STOP_TRANSMISSION       12  // CMD12
#define SD_CMD_SET_BLOCKLEN            16  // CMD16
#define SD_CMD_READ_SINGLE_BLOCK       17  // CMD17
#define SD_CMD_READ_MULTIPLE_BLOCK     18  // CMD18
#define SD_CMD_WRITE_BLOCK             24  // CMD24
#define SD_CMD_WRITE_MULTIPLE_BLOCK    25  // CMD25
#define SD_CMD_APP_CMD                 55  // CMD55
#define SD_CMD_READ_OCR                58  // CMD58
#define SD_CMD_CRC_ON_OFF              59  // CMD59

// ACMD命令（以CMD55开头）
#define SD_ACMD_SET_BUS_WIDTH          6   // ACMD6
#define SD_ACMD_SD_STATUS              13  // ACMD13
#define SD_ACMD_SEND_NUM_WR_BLOCKS     22  // ACMD22
#define SD_ACMD_SET_WR_BLK_ERASE_COUNT 23  // ACMD23
#define SD_ACMD_SD_SEND_OP_COND        41  // ACMD41
```

### 3.4 错误代码定义

```c
typedef enum {
    // 特殊错误
    SD_CMD_CRC_FAIL                    = (1),   // 命令响应CRC错误
    SD_DATA_CRC_FAIL                   = (2),   // 数据CRC错误
    SD_CMD_RSP_TIMEOUT                 = (3),   // 命令响应超时
    SD_DATA_TIMEOUT                    = (4),   // 数据超时
    SD_TX_UNDERRUN                     = (5),   // 发送欠载
    SD_RX_OVERRUN                      = (6),   // 接收过载
    SD_START_BIT_ERR                   = (7),   // 起始位错误
    SD_CMD_OUT_OF_RANGE                = (8),   // 命令超出范围
    SD_ADDR_MISALIGNED                 = (9),   // 地址未对齐
    SD_BLOCK_LEN_ERR                   = (10),  // 块长度错误
    SD_ERASE_SEQ_ERR                   = (11),  // 擦除序列错误
    SD_BAD_ERASE_PARAM                 = (12),  // 错误的擦除参数
    SD_WRITE_PROT_VIOLATION            = (13),  // 写保护违规
    SD_LOCK_UNLOCK_FAILED              = (14),  // 锁定/解锁失败
    SD_COM_CRC_FAILED                  = (15),  // 卡通信CRC失败
    SD_ILLEGAL_CMD                     = (16),  // 非法命令
    SD_CARD_ECC_FAILED                 = (17),  // 卡ECC失败
    SD_CC_ERROR                        = (18),  // 卡控制器错误
    SD_ERROR                           = (19),  // 一般错误
    SD_UNDERRUN                        = (20),  // 内部下溢
    SD_OVERRUN                         = (21),  // 内部上溢
    SD_CID_CSD_OVERWRITE               = (22),  // CID/CSD覆盖错误
    SD_WP_ERASE_SKIP                   = (23),  // 写保护擦除跳过
    SD_CARD_ECC_DISABLED               = (24),  // 卡ECC禁用
    SD_ERASE_RESET                     = (25),  // 擦除复位
    SD_AKE_SEQ_ERROR                   = (26),  // AKE序列错误
    SD_INVALID_VOLTRANGE               = (27),  // 无效电压范围
    SD_ADDR_OUT_OF_RANGE               = (28),  // 地址超出范围
    SD_SWITCH_ERROR                    = (29),  // 切换错误
    SD_SDIO_DISABLED                   = (30),  // SDIO禁用
    SD_SDIO_FUNCTION_BUSY              = (31),  // SDIO功能忙
    SD_SDIO_FUNCTION_FAILED            = (32),  // SDIO功能失败
    SD_SDIO_UNKNOWN_FUNCTION           = (33),  // SDIO未知功能

    // 标准错误
    SD_INTERNAL_ERROR,                         // 内部错误
    SD_NOT_CONFIGURED,                         // 未配置
    SD_REQUEST_PENDING,                        // 请求挂起
    SD_REQUEST_NOT_APPLICABLE,                 // 请求不适用
    SD_INVALID_PARAMETER,                      // 无效参数
    SD_UNSUPPORTED_FEATURE,                    // 不支持的特性
    SD_UNSUPPORTED_HW,                         // 不支持的硬件
    SD_ERROR                                   // 通用错误

} SD_Error;
```

## 4. 通用配置步骤

### 4.1 SD卡初始化步骤

1. **GPIO时钟使能**：
   ```c
   RCC_APB2PeriphClockCmd(RCC_APB2Periph_GPIOC | RCC_APB2Periph_GPIOD, ENABLE);
   RCC_AHBPeriphClockCmd(RCC_AHBPeriph_SDIO | RCC_AHBPeriph_DMA2, ENABLE);
   ```

2. **GPIO引脚配置**：
   ```c
   // 配置数据线、时钟线、命令线为复用推挽输出，50MHz速度
   // 除时钟线外，所有引脚需要外部47K上拉电阻
   ```

3. **SDIO外设初始化**：
   ```c
   SDIO_InitTypeDef SDIO_InitStructure = {0};
   SDIO_InitStructure.SDIO_ClockDiv = SDIO_INIT_CLK_DIV;  // 初始化时钟分频
   SDIO_InitStructure.SDIO_ClockEdge = SDIO_ClockEdge_Rising;
   SDIO_InitStructure.SDIO_ClockBypass = SDIO_ClockBypass_Disable;
   SDIO_InitStructure.SDIO_ClockPowerSave = SDIO_ClockPowerSave_Disable;
   SDIO_InitStructure.SDIO_BusWide = SDIO_BusWide_1b;     // 初始1位总线
   SDIO_InitStructure.SDIO_HardwareFlowControl = SDIO_HardwareFlowControl_Disable;
   SDIO_Init(&SDIO_InitStructure);
   ```

4. **SD卡上电和识别**：
   ```c
   SD_PowerON();              // 发送CMD0使卡进入空闲状态
   SD_InitializeCards();      // 发送CMD2、CMD3获取卡CID和RCA
   ```

5. **获取卡信息**：
   ```c
   SD_GetCardInfo(&SDCardInfo);  // 获取CSD、CID、容量等信息
   ```

6. **选择卡和配置总线**：
   ```c
   SD_SelectDeselect((u32)(SDCardInfo.RCA << 16));  // 选择卡
   SD_EnableWideBusOperation(SDIO_BusWide_4b);      // 配置4位总线
   ```

7. **设置高速传输时钟**：
   ```c
   SDIO_Clock_Set(SDIO_TRANSFER_CLK_DIV);  // 设置传输时钟
   ```

### 4.2 eMMC卡特殊配置步骤

1. **执行标准SD初始化**：
   ```c
   SD_Error status = SD_Init();
   ```

2. **读取扩展CSD寄存器**：
   ```c
   u8 ext_csd[512];
   eMMC_ReadExtCSD(ext_csd);  // 读取扩展CSD寄存器
   ```

3. **解析扩展CSD信息**：
   ```c
   // 获取扇区数量、卡类型、支持特性等信息
   E.eMMC_ExtCsd.SecCount = (ext_csd[EXT_CSD_SEC_COUNT+0]<<0) |
                           (ext_csd[EXT_CSD_SEC_COUNT+1]<<8) |
                           (ext_csd[EXT_CSD_SEC_COUNT+2]<<16)|
                           (ext_csd[EXT_CSD_SEC_COUNT+3]<<24);
   ```

4. **切换传输模式**：
   ```c
   eMMC_Change_Tran_Mode();  // 切换到高速传输模式
   ```

5. **配置8位总线宽度**：
   ```c
   SD_EnableWideBusOperation(SDIO_BusWide_8b);  // eMMC支持8位总线
   ```

### 4.3 DMA模式配置步骤

1. **使能DMA时钟**：
   ```c
   RCC_AHBPeriphClockCmd(RCC_AHBPeriph_DMA2, ENABLE);
   ```

2. **配置DMA通道**：
   ```c
   DMA_InitTypeDef DMA_InitStructure = {0};
   DMA_InitStructure.DMA_PeripheralBaseAddr = (u32)&SDIO->FIFO;
   DMA_InitStructure.DMA_MemoryBaseAddr = (u32)Buffer;
   DMA_InitStructure.DMA_DIR = DMA_DIR_PeripheralSRC;  // 接收方向
   DMA_InitStructure.DMA_BufferSize = BufferSize;
   DMA_InitStructure.DMA_PeripheralInc = DMA_PeripheralInc_Disable;
   DMA_InitStructure.DMA_MemoryInc = DMA_MemoryInc_Enable;
   DMA_InitStructure.DMA_PeripheralDataSize = DMA_PeripheralDataSize_Word;
   DMA_InitStructure.DMA_MemoryDataSize = DMA_MemoryDataSize_Word;
   DMA_InitStructure.DMA_Mode = DMA_Mode_Normal;
   DMA_InitStructure.DMA_Priority = DMA_Priority_High;
   DMA_InitStructure.DMA_M2M = DMA_M2M_Disable;
   DMA_Init(DMA2_Channel4, &DMA_InitStructure);
   ```

3. **配置DMA中断**：
   ```c
   DMA_ITConfig(DMA2_Channel4, DMA_IT_TC, ENABLE);  // 传输完成中断
   NVIC_EnableIRQ(DMA2_Channel4_5_IRQn);
   ```

4. **使能SDIO DMA请求**：
   ```c
   SDIO_DMACmd(ENABLE);
   ```

5. **在中断中处理传输完成**：
   ```c
   void DMA2_Channel4_5_IRQHandler(void)
   {
       if(DMA_GetITStatus(DMA2_IT_TC4))
       {
           DMA_ClearITPendingBit(DMA2_IT_TC4);
           // 处理传输完成
       }
   }
   ```

### 4.4 FATFS集成步骤

1. **添加FATFS源码到工程**：
   ```
   fatfs/
   ├── ff.c          # FATFS核心文件
   ├── ff.h          # FATFS头文件
   ├── ffconf.h      # FATFS配置
   └── diskio.c      # 磁盘I/O接口
   ```

2. **实现diskio.c接口**：
   ```c
   // 实现disk_initialize, disk_read, disk_write等函数
   // 调用SD_ReadDisk和SD_WriteDisk函数
   ```

3. **配置ffconf.h**：
   ```c
   #define _USE_MKFS        1  // 启用格式化功能
   #define _FS_FAT32       1  // 启用FAT32支持
   #define _USE_LFN        2  // 使用长文件名（2=支持LFN）
   #define _VOLUMES        1  // 支持的卷数量
   ```

4. **在代码中使用FATFS**：
   ```c
   FATFS fs;           // 文件系统对象
   FIL file;           // 文件对象
   FRESULT fr;         // 返回结果

   // 挂载文件系统
   fr = f_mount(&fs, "0:", 1);

   // 打开文件
   fr = f_open(&file, "test.txt", FA_CREATE_ALWAYS | FA_WRITE);

   // 写入文件
   f_write(&file, "Hello SD Card!", 14, &bytes_written);

   // 关闭文件
   f_close(&file);
   ```

## 5. 示例工程详解

### 5.1 SDIO示例工程概览

SDIO文件夹包含4个子示例工程：

1. **SDIO_eMMC** - eMMC卡读写示例
   - 位置：`SDIO/SDIO_eMMC/User/main.c`
   - 功能：演示eMMC卡初始化、扩展CSD读取、全扇区读写测试
   - 特性：支持8位总线宽度，读取扩展CSD寄存器

2. **SDIO_Mode** - SDIO主从模式通信示例
   - 位置：`SDIO/SDIO_Mode/User/main.c`
   - 功能：演示SDIO主从模式通信，可配置为主机或从机
   - 特性：支持中断模式，演示命令/数据传输流程

3. **SDIO_SD** - SD卡读写示例
   - 位置：`SDIO/SDIO_SD/User/main.c`
   - 功能：演示SD卡初始化、卡信息获取、全扇区读写测试
   - 特性：支持1位/4位总线宽度，完整的错误处理

4. **SDIO_SD_FATFS** - SD卡+FATFS文件系统示例
   - 位置：`SDIO/SDIO_SD_FATFS/User/main.c`
   - 功能：演示FATFS文件系统在SD卡上的使用
   - 特性：文件读写、目录操作、格式化功能

### 5.2 关键功能对比

| 示例工程 | 主要功能 | 关键API函数 | 适用场景 |
|----------|----------|-------------|----------|
| **SDIO_eMMC** | eMMC卡读写 | `eMMC_Init()`, `eMMC_ReadExtCSD()` | eMMC存储设备 |
| **SDIO_Mode** | 主从通信 | `SDIO_SendCommand()`, `SDIO_DataConfig()` | SDIO设备通信 |
| **SDIO_SD** | SD卡读写 | `SD_Init()`, `SD_ReadDisk()`, `SD_WriteDisk()` | SD卡存储 |
| **SDIO_SD_FATFS** | 文件系统 | `f_mount()`, `f_open()`, `f_write()` | 文件存储应用 |

### 5.3 硬件连接要求

**SDIO引脚连接**：
- **数据线D0-D7**：需要外部47K上拉电阻
- **时钟线SCK**：不需要上拉电阻
- **命令线CMD**：需要外部47K上拉电阻

**典型连接电路**：
```
CH32V307           SD卡插座
  PC8 (D0) -------- D0 (上拉47K)
  PC9 (D1) -------- D1 (上拉47K)
  PC10(D2) -------- D2 (上拉47K)
  PC11(D3) -------- D3 (上拉47K)
  PC12(SCK) ------- CLK
  PD2 (CMD) ------- CMD (上拉47K)
  GND ------------ GND
  3.3V ----------- VCC
```

**电源要求**：
- SD卡工作电压：2.7-3.6V
- 建议使用LDO提供稳定3.3V电源
- 电源滤波电容：10uF + 0.1uF

### 5.4 性能特性

- **最大时钟频率**：SD卡识别阶段≤400kHz，传输阶段可达50MHz
- **数据传输速率**：理论最大25MB/s（4位总线，50MHz时钟）
- **块大小**：支持512字节、1024字节、2048字节块大小
- **总线宽度**：支持1位、4位、8位（eMMC）数据总线
- **工作模式**：轮询模式、中断模式、DMA模式

## 6. 常见问题

### Q1: SD卡初始化失败，返回SD_CMD_RSP_TIMEOUT？
A: 检查以下配置：
1. **时钟频率**：识别阶段时钟必须≤400kHz，检查`SDIO_INIT_CLK_DIV`设置
2. **上拉电阻**：数据线和命令线需要47K上拉电阻
3. **电源稳定性**：确保3.3V电源稳定，无噪声干扰
4. **硬件连接**：检查所有引脚连接是否正确
5. **卡兼容性**：某些SD卡可能需要特定的初始化序列

### Q2: 数据传输出现CRC错误？
A: 可能原因：
1. **时钟不稳定**：增加时钟分频值降低频率
2. **信号质量差**：检查PCB布线，确保信号完整
3. **电源噪声**：加强电源滤波，增加去耦电容
4. **时序问题**：调整SDIO时钟相位和采样点

### Q3: eMMC卡无法读取扩展CSD寄存器？
A: 解决方案：
1. **发送CMD8**：在初始化序列中发送CMD8（Send Interface Condition）
2. **检查电压**：确认eMMC工作在正确电压范围
3. **总线宽度**：eMMC可能需要8位总线宽度才能正常工作
4. **时序参数**：调整eMMC特定的时序参数

### Q4: FATFS格式化失败或无法挂载？
A: 诊断步骤：
1. **检查SD卡初始化**：确保`SD_Init()`返回成功
2. **验证读写功能**：使用`SD_ReadDisk`/`SD_WriteDisk`测试基本功能
3. **检查扇区大小**：确保FATFS配置的扇区大小与SD卡匹配（通常512字节）
4. **重新格式化**：使用`f_mkfs("0:", FM_FAT32, 0, work, sizeof(work))`重新格式化

### Q5: DMA模式传输数据损坏？
A: 优化建议：
1. **内存对齐**：确保缓冲区32位对齐（4字节边界）
2. **缓存一致性**：如果使用缓存，确保DMA传输前后刷新缓存
3. **DMA配置**：检查DMA源/目标地址、数据大小配置
4. **中断处理**：确保及时处理DMA传输完成中断

## 7. API参考

### 7.1 SDIO初始化API

| 函数名 | 功能描述 | 参数说明 |
|--------|----------|----------|
| `SDIO_DeInit` | SDIO外设复位 | 无 |
| `SDIO_Init` | SDIO初始化 | SDIO_InitTypeDef结构体指针 |
| `SDIO_StructInit` | 初始化结构体为默认值 | SDIO_InitTypeDef结构体指针 |
| `SDIO_ClockCmd` | SDIO时钟使能/禁用 | FunctionalState：ENABLE/DISABLE |
| `SDIO_SetPowerState` | 设置SDIO电源状态 | SDIO_PowerState：ON/OFF |

### 7.2 SDIO命令API

| 函数名 | 功能描述 | 参数说明 |
|--------|----------|----------|
| `SDIO_SendCommand` | 发送SDIO命令 | SDIO_CmdInitTypeDef结构体指针 |
| `SDIO_CmdStructInit` | 初始化命令结构体 | SDIO_CmdInitTypeDef结构体指针 |
| `SDIO_GetCommandResponse` | 获取命令响应 | 返回响应寄存器值 |
| `SDIO_GetResponse` | 获取指定响应 | SDIO_RESP寄存器索引 |

### 7.3 SDIO数据API

| 函数名 | 功能描述 | 参数说明 |
|--------|----------|----------|
| `SDIO_DataConfig` | 配置数据参数 | SDIO_DataInitTypeDef结构体指针 |
| `SDIO_DataStructInit` | 初始化数据结构体 | SDIO_DataInitTypeDef结构体指针 |
| `SDIO_GetDataCounter` | 获取剩余数据计数 | 返回剩余数据字节数 |
| `SDIO_ReadData` | 从FIFO读取数据 | 返回读取的数据 |
| `SDIO_WriteData` | 向FIFO写入数据 | Data：要写入的数据 |

### 7.4 SDIO状态和中断API

| 函数名 | 功能描述 | 参数说明 |
|--------|----------|----------|
| `SDIO_GetFlagStatus` | 获取标志状态 | SDIO_FLAG：标志位 |
| `SDIO_ClearFlag` | 清除标志 | SDIO_FLAG：标志位 |
| `SDIO_GetITStatus` | 获取中断状态 | SDIO_IT：中断标志 |
| `SDIO_ClearITPendingBit` | 清除中断挂起位 | SDIO_IT：中断标志 |
| `SDIO_ITConfig` | 中断配置 | SDIO_IT：中断，NewState：状态 |

### 7.5 高级SD卡API

| 函数名 | 功能描述 | 参数说明 |
|--------|----------|----------|
| `SD_Init` | SD卡初始化 | 返回SD_Error状态 |
| `SD_DeInit` | SD卡反初始化 | 无 |
| `SD_ReadDisk` | 读取磁盘扇区 | buf：缓冲区，sector：扇区号，cnt：扇区数 |
| `SD_WriteDisk` | 写入磁盘扇区 | buf：缓冲区，sector：扇区号，cnt：扇区数 |
| `SD_GetCardInfo` | 获取卡信息 | SD_CardInfo结构体指针 |
| `SD_GetCardState` | 获取卡状态 | 返回卡状态 |

### 7.6 DMA相关API

| 函数名 | 功能描述 | 参数说明 |
|--------|----------|----------|
| `SDIO_DMACmd` | DMA请求使能 | FunctionalState：ENABLE/DISABLE |
| `SDIO_GetFIFOCount` | 获取FIFO数据计数 | 返回FIFO中的数据字数 |
| `SDIO_SetSDIOOperation` | 设置SDIO操作 | SDIO_Operation：操作模式 |

## 8. 注意事项

1. **时钟配置**：
   - 识别模式时钟必须≤400kHz
   - 传输模式可提高时钟频率，但需确保信号完整性
   - 如果通信不稳定，增加时钟分频值

2. **硬件设计**：
   - 数据线和命令线需要外部47K上拉电阻
   - 电源必须稳定，建议使用LDO和足够的滤波电容
   - PCB布线时注意信号完整性，避免长走线

3. **软件配置**：
   - 初始化序列必须按照SD/MMC规范执行
   - 正确处理各种错误状态和超时
   - 使用DMA模式时注意内存对齐和缓存一致性

4. **兼容性**：
   - 不同厂商的SD卡可能有细微差异
   - eMMC卡需要特殊的初始化序列
   - 测试时使用多种品牌和容量的卡

5. **性能优化**：
   - 使用DMA模式减少CPU开销
   - 配置合适的时钟分频平衡速度和稳定性
   - 使用多块读写减少命令开销

6. **电源管理**：
   - SD卡插入/拔出时可能有较大的电流波动
   - 考虑热插拔保护电路
   - 在低功耗应用中合理管理SDIO电源

## 9. 性能优化建议

1. **时钟优化**：
   ```c
   // 识别阶段使用低时钟
   #define SDIO_INIT_CLK_DIV     0xB2  // ~400kHz

   // 传输阶段使用高时钟
   #define SDIO_TRANSFER_CLK_DIV 0x00  // 最高频率

   // 如果不稳定，适当增加分频值
   #define SDIO_TRANSFER_CLK_DIV 0x02  // 降低频率提高稳定性
   ```

2. **DMA优化**：
   ```c
   // 使用双缓冲减少等待时间
   u32 buffer1[BLOCK_SIZE/4];
   u32 buffer2[BLOCK_SIZE/4];

   // 当DMA传输buffer1时，CPU处理buffer2中的数据
   // 实现流水线操作
   ```

3. **命令优化**：
   ```c
   // 使用多块读写减少命令开销
   SD_ReadMultiBlocks(buffer, start_sector, BLOCK_SIZE, block_count);
   SD_WriteMultiBlocks(buffer, start_sector, BLOCK_SIZE, block_count);
   ```

4. **缓存优化**：
   ```c
   // 实现读写缓存减少实际SD卡访问
   #define CACHE_SIZE 16  // 16个扇区缓存
   u8 read_cache[CACHE_SIZE * 512];
   u32 cache_dirty = 0;  // 缓存脏标志
   ```

5. **错误恢复**：
   ```c
   // 实现自动重试机制
   for(int retry = 0; retry < MAX_RETRY; retry++)
   {
       error = SD_ReadDisk(buffer, sector, count);
       if(error == SD_OK) break;

       // 重试前进行一些恢复操作
       SD_DeInit();
       Delay_Ms(10);
       SD_Init();
   }
   ```

## 10. 典型应用场景

### 10.1 数据采集存储

```c
// 数据采集到SD卡
void DataLogger_Application(void)
{
    // 初始化SD卡
    if(SD_Init() != SD_OK)
    {
        printf("SD Card initialization failed!\r\n");
        return;
    }

    // 创建数据文件
    FIL file;
    FRESULT fr;
    fr = f_open(&file, "data.csv", FA_CREATE_ALWAYS | FA_WRITE);

    // 采集循环
    while(1)
    {
        // 采集传感器数据
        SensorData data = ReadAllSensors();

        // 格式化数据
        char buffer[256];
        sprintf(buffer, "%lu,%.2f,%.2f,%.2f\r\n",
                GetTickCount(), data.temperature,
                data.humidity, data.pressure);

        // 写入SD卡
        UINT bytes_written;
        f_write(&file, buffer, strlen(buffer), &bytes_written);

        // 定期同步文件系统
        static uint32_t last_sync = 0;
        if(GetTickCount() - last_sync > 5000)  // 每5秒同步一次
        {
            f_sync(&file);
            last_sync = GetTickCount();
        }

        Delay_Ms(1000);  // 每秒采集一次
    }

    f_close(&file);
}
```

### 10.2 固件更新

```c
// 从SD卡更新固件
void FirmwareUpdate_FromSD(void)
{
    printf("Checking for firmware update...\r\n");

    // 初始化SD卡
    if(SD_Init() != SD_OK)
    {
        printf("SD Card not found\r\n");
        return;
    }

    // 挂载文件系统
    FATFS fs;
    if(f_mount(&fs, "0:", 1) != FR_OK)
    {
        printf("Failed to mount filesystem\r\n");
        return;
    }

    // 检查固件文件
    FILINFO fno;
    if(f_stat("firmware.bin", &fno) != FR_OK)
    {
        printf("No firmware file found\r\n");
        f_unmount("0:");
        return;
    }

    printf("Found firmware file: %s, size: %lu bytes\r\n",
           fno.fname, fno.fsize);

    // 读取固件文件
    FIL file;
    if(f_open(&file, "firmware.bin", FA_READ) != FR_OK)
    {
        printf("Failed to open firmware file\r\n");
        f_unmount("0:");
        return;
    }

    // 验证固件（校验和、版本号等）
    if(!ValidateFirmware(&file))
    {
        printf("Firmware validation failed\r\n");
        f_close(&file);
        f_unmount("0:");
        return;
    }

    // 开始更新
    printf("Starting firmware update...\r\n");
    Flash_ProgramFirmware(&file);

    f_close(&file);
    f_unmount("0:");

    printf("Firmware update complete, rebooting...\r\n");
    NVIC_SystemReset();  // 重启系统
}
```

### 10.3 多媒体文件播放

```c
// 从SD卡播放音频文件
void AudioPlayer_Application(void)
{
    // 初始化SD卡和音频解码器
    SD_Init();
    AudioDecoder_Init();

    // 扫描音乐文件
    DIR dir;
    FILINFO fno;

    if(f_opendir(&dir, "0:/Music") == FR_OK)
    {
        printf("Music files:\r\n");

        while(f_readdir(&dir, &fno) == FR_OK && fno.fname[0] != 0)
        {
            // 检查文件扩展名
            if(strstr(fno.fname, ".mp3") || strstr(fno.fname, ".wav"))
            {
                printf("  %s\r\n", fno.fname);
            }
        }

        f_closedir(&dir);
    }

    // 播放音乐
    FIL audio_file;
    if(f_open(&audio_file, "0:/Music/song.mp3", FA_READ) == FR_OK)
    {
        printf("Playing: song.mp3\r\n");

        // 使用DMA读取音频数据
        u8 audio_buffer[AUDIO_BUFFER_SIZE];
        UINT bytes_read;

        while(f_read(&audio_file, audio_buffer, AUDIO_BUFFER_SIZE, &bytes_read) == FR_OK && bytes_read > 0)
        {
            // 解码并播放音频
            AudioDecoder_FeedData(audio_buffer, bytes_read);

            // 如果缓冲区满，等待播放
            while(AudioDecoder_IsBufferFull())
            {
                Delay_Ms(1);
            }
        }

        f_close(&audio_file);
    }

    printf("Playback finished\r\n");
}
```

## 11. 总结

CH32V307的SDIO接口提供了完整的SD/MMC/eMMC卡支持，具有灵活的配置选项和强大的性能。通过合理配置GPIO、时钟参数和工作模式，可以实现稳定高效的存储解决方案。开发时需要注意硬件设计、初始化序列、错误处理和性能优化等关键环节。SDIO功能特别适用于数据采集、固件存储、多媒体应用等需要大容量存储的场景。
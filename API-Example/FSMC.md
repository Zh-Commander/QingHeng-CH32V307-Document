# CH32V307 FSMC灵活的静态存储器控制器使用指南

## 1. 外设概述

FSMC（Flexible Static Memory Controller，灵活的静态存储器控制器）是CH32V307的外部存储器接口，支持多种类型的存储器和外设。主要特性包括：

- **多种存储器类型**：SRAM、PSRAM、NOR Flash、NAND Flash
- **多种接口模式**：8位/16位数据总线，复用/非复用地址总线
- **多个存储区**：支持4个NOR/PSRAM存储区和4个NAND/PC卡存储区
- **灵活的时序**：可编程的等待状态、建立时间、保持时间
- **高级功能**：ECC校验（NAND Flash）、写保护、掉电模式

## 2. 关键代码片段

### 2.1 FSMC SRAM初始化

```c
void FSMC_SRAM_Init(void)
{
    FSMC_NORSRAMInitTypeDef FSMC_NORSRAMInitStructure = {0};
    FSMC_NORSRAMTimingInitTypeDef readWriteTiming = {0};
    GPIO_InitTypeDef GPIO_InitStructure = {0};

    // 1. 使能GPIO和FSMC时钟
    RCC_APB2PeriphClockCmd(RCC_APB2Periph_GPIOB | RCC_APB2Periph_GPIOD | RCC_APB2Periph_GPIOE, ENABLE);
    RCC_AHBPeriphClockCmd(RCC_AHBPeriph_FSMC, ENABLE);

    // 2. 配置FSMC数据线（PD0-PD15）
    GPIO_InitStructure.GPIO_Pin = GPIO_Pin_0 | GPIO_Pin_1 | GPIO_Pin_8 | GPIO_Pin_9 |
                                  GPIO_Pin_10 | GPIO_Pin_14 | GPIO_Pin_15;
    GPIO_InitStructure.GPIO_Mode = GPIO_Mode_AF_PP;
    GPIO_InitStructure.GPIO_Speed = GPIO_Speed_50MHz;
    GPIO_Init(GPIOD, &GPIO_InitStructure);

    // 3. 配置FSMC地址线（PD11, PD12, PD13）
    GPIO_InitStructure.GPIO_Pin = GPIO_Pin_11 | GPIO_Pin_12 | GPIO_Pin_13;
    GPIO_Init(GPIOD, &GPIO_InitStructure);

    // 4. 配置FSMC控制线（PD4-NOE, PD5-NWE, PD7-NE1）
    GPIO_InitStructure.GPIO_Pin = GPIO_Pin_4 | GPIO_Pin_5 | GPIO_Pin_7;
    GPIO_Init(GPIOD, &GPIO_InitStructure);

    // 5. 配置FSMC地址线高位（PE2-PE7）
    GPIO_InitStructure.GPIO_Pin = GPIO_Pin_2 | GPIO_Pin_3 | GPIO_Pin_4 | GPIO_Pin_5 |
                                  GPIO_Pin_6 | GPIO_Pin_7;
    GPIO_Init(GPIOE, &GPIO_InitStructure);

    // 6. 配置FSMC控制线（PB7-NE2）
    GPIO_InitStructure.GPIO_Pin = GPIO_Pin_7;
    GPIO_Init(GPIOB, &GPIO_InitStructure);

    // 7. 配置时序参数
    readWriteTiming.FSMC_AddressSetupTime = 0x00;       // 地址建立时间
    readWriteTiming.FSMC_AddressHoldTime = 0x00;        // 地址保持时间
    readWriteTiming.FSMC_DataSetupTime = 0x03;          // 数据建立时间
    readWriteTiming.FSMC_BusTurnAroundDuration = 0x00;  // 总线转换时间
    readWriteTiming.FSMC_CLKDivision = 0x00;            // 时钟分频
    readWriteTiming.FSMC_DataLatency = 0x00;            // 数据延迟
    readWriteTiming.FSMC_AccessMode = FSMC_AccessMode_A; // 访问模式A

    // 8. 配置FSMC NOR/SRAM参数
    FSMC_NORSRAMInitStructure.FSMC_Bank = FSMC_Bank1_NORSRAM1;  // 使用存储区1
    FSMC_NORSRAMInitStructure.FSMC_DataAddressMux = FSMC_DataAddressMux_Enable;  // 地址数据复用
    FSMC_NORSRAMInitStructure.FSMC_MemoryType = FSMC_MemoryType_SRAM;  // 存储器类型：SRAM
    FSMC_NORSRAMInitStructure.FSMC_MemoryDataWidth = FSMC_MemoryDataWidth_16b;  // 数据宽度：16位
    FSMC_NORSRAMInitStructure.FSMC_BurstAccessMode = FSMC_BurstAccessMode_Disable;  // 禁用突发访问
    FSMC_NORSRAMInitStructure.FSMC_WaitSignalPolarity = FSMC_WaitSignalPolarity_Low;  // 等待信号极性低
    FSMC_NORSRAMInitStructure.FSMC_WrapMode = FSMC_WrapMode_Disable;  // 禁用回绕模式
    FSMC_NORSRAMInitStructure.FSMC_WaitSignalActive = FSMC_WaitSignalActive_BeforeWaitState;  // 等待信号有效
    FSMC_NORSRAMInitStructure.FSMC_WriteOperation = FSMC_WriteOperation_Enable;  // 写操作使能
    FSMC_NORSRAMInitStructure.FSMC_WaitSignal = FSMC_WaitSignal_Disable;  // 禁用等待信号
    FSMC_NORSRAMInitStructure.FSMC_ExtendedMode = FSMC_ExtendedMode_Disable;  // 禁用扩展模式
    FSMC_NORSRAMInitStructure.FSMC_WriteBurst = FSMC_WriteBurst_Disable;  // 禁用写突发
    FSMC_NORSRAMInitStructure.FSMC_ReadWriteTimingStruct = &readWriteTiming;  // 读写时序
    FSMC_NORSRAMInitStructure.FSMC_WriteTimingStruct = &readWriteTiming;  // 写时序（与读相同）

    // 9. 初始化FSMC
    FSMC_NORSRAMInit(&FSMC_NORSRAMInitStructure);

    // 10. 使能FSMC存储区
    FSMC_NORSRAMCmd(FSMC_Bank1_NORSRAM1, ENABLE);
}
```

### 2.2 LCD内存映射定义

```c
// LCD地址结构体定义
typedef struct {
    vu16 LCD_REG;  // 寄存器地址
    vu16 LCD_RAM;  // 数据地址
} LCD_TypeDef;

// LCD基地址定义
// Bank1 - sector1 - 地址偏移: 0x0003FFFE
#define LCD_BASE        ((u32)(0x60000000 | 0x0003FFFE))
#define LCD             ((LCD_TypeDef *) LCD_BASE)

// LCD寄存器读写宏定义
#define LCD_WR_REG(data)    {LCD->LCD_REG = data;}
#define LCD_WR_DATA(data)   {LCD->LCD_RAM = data;}
#define LCD_RD_DATA()       (LCD->LCD_RAM)

// LCD初始化序列
void LCD_Init(void)
{
    // 初始化GPIO
    LCD_GPIO_Init();

    // 复位LCD
    LCD_RST = 0;
    Delay_Ms(100);
    LCD_RST = 1;
    Delay_Ms(50);

    // 发送初始化命令序列
    LCD_WR_REG(0x11);  // Sleep out
    Delay_Ms(120);

    LCD_WR_REG(0x36);  // Memory Access Control
    LCD_WR_DATA(0x00);

    LCD_WR_REG(0x3A);  // Pixel Format Set
    LCD_WR_DATA(0x55);  // 16 bits per pixel

    // ... 更多初始化命令

    LCD_WR_REG(0x29);  // Display ON
}
```

### 2.3 SRAM读写函数

```c
// SRAM地址定义
#define Bank1_SRAM1_ADDR    ((u32)0x60000000)

// 写入缓冲区到SRAM
void FSMC_SRAM_WriteBuffer(u8* pBuffer, u32 WriteAddr, u32 n)
{
    for(; n != 0; n--) {
        *(vu8*)(Bank1_SRAM1_ADDR + WriteAddr) = *pBuffer;  // 写入一个字节
        WriteAddr++;
        pBuffer++;
    }
}

// 从SRAM读取缓冲区
void FSMC_SRAM_ReadBuffer(u8* pBuffer, u32 ReadAddr, u32 n)
{
    for(; n != 0; n--) {
        *pBuffer++ = *(vu8*)(Bank1_SRAM1_ADDR + ReadAddr);  // 读取一个字节
        ReadAddr++;
    }
}

// 写入16位数据到SRAM
void FSMC_SRAM_WriteHalfWord(u16 data, u32 WriteAddr)
{
    *(vu16*)(Bank1_SRAM1_ADDR + WriteAddr) = data;
}

// 从SRAM读取16位数据
u16 FSMC_SRAM_ReadHalfWord(u32 ReadAddr)
{
    return *(vu16*)(Bank1_SRAM1_ADDR + ReadAddr);
}

// SRAM测试函数
u8 FSMC_SRAM_Test(void)
{
    u16 i, data;
    u16 write_data[1024], read_data[1024];

    // 生成测试数据
    for(i = 0; i < 1024; i++) {
        write_data[i] = i;
    }

    // 写入数据
    for(i = 0; i < 1024; i++) {
        FSMC_SRAM_WriteHalfWord(write_data[i], i * 2);
    }

    // 读取数据
    for(i = 0; i < 1024; i++) {
        read_data[i] = FSMC_SRAM_ReadHalfWord(i * 2);
    }

    // 比较数据
    for(i = 0; i < 1024; i++) {
        if(read_data[i] != write_data[i]) {
            printf("SRAM Test Error at address: 0x%04X\r\n", i * 2);
            return 1;  // 测试失败
        }
    }

    printf("SRAM Test OK!\r\n");
    return 0;  // 测试成功
}
```

### 2.4 NAND Flash操作

```c
// NAND Flash初始化
u8 NAND_Init(void)
{
    FSMC_NAND_InitTypeDef FSMC_NAND_InitStructure;
    FSMC_NAND_PCCARDTimingInitTypeDef ComSpaceTiming, AttSpaceTiming;
    GPIO_InitTypeDef GPIO_InitStructure;

    // 使能时钟
    RCC_APB2PeriphClockCmd(RCC_APB2Periph_GPIOD | RCC_APB2Periph_GPIOE | RCC_APB2Periph_GPIOG, ENABLE);
    RCC_AHBPeriphClockCmd(RCC_AHBPeriph_FSMC, ENABLE);

    // 配置GPIO...

    // 配置公共空间时序
    ComSpaceTiming.FSMC_SetupTime = 0;
    ComSpaceTiming.FSMC_WaitSetupTime = 3;
    ComSpaceTiming.FSMC_HoldSetupTime = 0;
    ComSpaceTiming.FSMC_HiZSetupTime = 0;

    // 配置属性空间时序
    AttSpaceTiming.FSMC_SetupTime = 0;
    AttSpaceTiming.FSMC_WaitSetupTime = 3;
    AttSpaceTiming.FSMC_HoldSetupTime = 0;
    AttSpaceTiming.FSMC_HiZSetupTime = 0;

    // 配置NAND Flash
    FSMC_NAND_InitStructure.FSMC_Bank = FSMC_Bank2_NAND;
    FSMC_NAND_InitStructure.FSMC_Waitfeature = FSMC_Waitfeature_Enable;
    FSMC_NAND_InitStructure.FSMC_MemoryDataWidth = FSMC_MemoryDataWidth_8b;
    FSMC_NAND_InitStructure.FSMC_ECC = FSMC_ECC_Enable;
    FSMC_NAND_InitStructure.FSMC_ECCPageSize = FSMC_ECCPageSize_2048Bytes;
    FSMC_NAND_InitStructure.FSMC_TCLRSetupTime = 0x00;
    FSMC_NAND_InitStructure.FSMC_TARSetupTime = 0x00;
    FSMC_NAND_InitStructure.FSMC_CommonSpaceTimingStruct = &ComSpaceTiming;
    FSMC_NAND_InitStructure.FSMC_AttributeSpaceTimingStruct = &AttSpaceTiming;

    FSMC_NAND_Init(&FSMC_NAND_InitStructure);
    FSMC_NAND_ECC_Enable(FSMC_Bank2_NAND);
    FSMC_NANDCmd(FSMC_Bank2_NAND, ENABLE);

    // 读取NAND ID
    NAND_ReadID();

    return 0;
}

// 带ECC的页写入
u8 NAND_WritePagewithEcc(u32 PageNum, u16 ColNum, u8 *pBuffer, u16 NumByteToWrite)
{
    // 等待NAND Flash就绪
    while(NAND_ReadStatus() != NSTA_READY);

    // 发送页编程命令
    NAND_WriteCmd(NAND_CMD_PAGEPROGRAM);

    // 发送地址
    NAND_WriteAddr(ColNum & 0xFF);
    NAND_WriteAddr((ColNum >> 8) & 0xFF);
    NAND_WriteAddr(PageNum & 0xFF);
    NAND_WriteAddr((PageNum >> 8) & 0xFF);
    NAND_WriteAddr((PageNum >> 16) & 0xFF);

    // 写入数据
    for(u16 i = 0; i < NumByteToWrite; i++) {
        NAND_WriteData(pBuffer[i]);
    }

    // 获取ECC值
    NAND_ECC = FSMC_GetECC(FSMC_Bank2_NAND);

    // 确认编程
    NAND_WriteCmd(NAND_CMD_PAGEPROGRAM_CONFIRM);

    // 等待编程完成
    while(NAND_ReadStatus() != NSTA_READY);

    // 检查ECC
    NAND_ECC_Check();

    return 0;
}
```

## 3. 配置数据结构

### 3.1 FSMC_NORSRAMTimingInitTypeDef（时序初始化结构体）

```c
typedef struct
{
    uint32_t FSMC_AddressSetupTime;       // 地址建立时间
    uint32_t FSMC_AddressHoldTime;        // 地址保持时间
    uint32_t FSMC_DataSetupTime;          // 数据建立时间
    uint32_t FSMC_BusTurnAroundDuration;  // 总线转换时间
    uint32_t FSMC_CLKDivision;            // 时钟分频
    uint32_t FSMC_DataLatency;            // 数据延迟
    uint32_t FSMC_AccessMode;             // 访问模式
} FSMC_NORSRAMTimingInitTypeDef;
```

### 3.2 FSMC_NORSRAMInitTypeDef（NOR/SRAM初始化结构体）

```c
typedef struct
{
    uint32_t FSMC_Bank;                    // 存储区选择
    uint32_t FSMC_DataAddressMux;          // 地址数据复用
    uint32_t FSMC_MemoryType;              // 存储器类型
    uint32_t FSMC_MemoryDataWidth;         // 存储器数据宽度
    uint32_t FSMC_BurstAccessMode;         // 突发访问模式
    uint32_t FSMC_WaitSignalPolarity;      // 等待信号极性
    uint32_t FSMC_WrapMode;                // 回绕模式
    uint32_t FSMC_WaitSignalActive;        // 等待信号有效
    uint32_t FSMC_WriteOperation;          // 写操作使能
    uint32_t FSMC_WaitSignal;              // 等待信号
    uint32_t FSMC_ExtendedMode;            // 扩展模式
    uint32_t FSMC_WriteBurst;              // 写突发
    FSMC_NORSRAMTimingInitTypeDef* FSMC_ReadWriteTimingStruct;  // 读写时序
    FSMC_NORSRAMTimingInitTypeDef* FSMC_WriteTimingStruct;      // 写时序
} FSMC_NORSRAMInitTypeDef;
```

### 3.3 存储器地址映射

```c
// Bank1 NOR/SRAM 存储区地址映射
#define FSMC_Bank1_NORSRAM1_ADDR    ((uint32_t)0x60000000)  // 存储区1
#define FSMC_Bank1_NORSRAM2_ADDR    ((uint32_t)0x64000000)  // 存储区2
#define FSMC_Bank1_NORSRAM3_ADDR    ((uint32_t)0x68000000)  // 存储区3
#define FSMC_Bank1_NORSRAM4_ADDR    ((uint32_t)0x6C000000)  // 存储区4

// Bank2 NAND Flash 存储区
#define FSMC_Bank2_NAND_ADDR        ((uint32_t)0x70000000)

// Bank3 NAND Flash 存储区
#define FSMC_Bank3_NAND_ADDR        ((uint32_t)0x80000000)

// Bank4 PC卡存储区
#define FSMC_Bank4_PCCARD_ADDR      ((uint32_t)0x90000000)
```

## 4. 通用配置步骤

### 4.1 SRAM配置步骤
1. **时钟使能**：
   ```c
   RCC_APB2PeriphClockCmd(RCC_APB2Periph_GPIOB | RCC_APB2Periph_GPIOD | RCC_APB2Periph_GPIOE, ENABLE);
   RCC_AHBPeriphClockCmd(RCC_AHBPeriph_FSMC, ENABLE);
   ```

2. **GPIO配置**：
   - 数据线：PD0-PD15（16位模式）
   - 地址线：PD11-PD13，PE2-PE7
   - 控制线：PD4（NOE）、PD5（NWE）、PD7（NE1）

3. **时序参数配置**：
   - 地址建立时间：`FSMC_AddressSetupTime`
   - 数据建立时间：`FSMC_DataSetupTime`
   - 访问模式：`FSMC_AccessMode_A`

4. **FSMC参数配置**：
   - 存储区：`FSMC_Bank1_NORSRAM1`
   - 存储器类型：`FSMC_MemoryType_SRAM`
   - 数据宽度：`FSMC_MemoryDataWidth_16b`

5. **初始化使能**：
   ```c
   FSMC_NORSRAMInit(&FSMC_NORSRAMInitStructure);
   FSMC_NORSRAMCmd(FSMC_Bank1_NORSRAM1, ENABLE);
   ```

### 4.2 LCD配置步骤（8位模式）
1. **时钟使能**：
   ```c
   RCC_APB2PeriphClockCmd(RCC_APB2Periph_GPIOD | RCC_APB2Periph_GPIOE, ENABLE);
   RCC_AHBPeriphClockCmd(RCC_AHBPeriph_FSMC, ENABLE);
   ```

2. **GPIO配置**（8位数据线）：
   - 数据线：PD0-PD7
   - 控制线：PD4（NOE）、PD5（NWE）、PD7（NE1）
   - RS信号：PD11（命令/数据选择）

3. **时序参数配置**（LCD通常需要较慢的时序）：
   ```c
   readWriteTiming.FSMC_AddressSetupTime = 0x02;
   readWriteTiming.FSMC_DataSetupTime = 0x05;
   ```

4. **FSMC参数配置**：
   ```c
   FSMC_NORSRAMInitStructure.FSMC_MemoryDataWidth = FSMC_MemoryDataWidth_8b;
   ```

5. **LCD初始化序列**：
   - 发送复位命令
   - 配置显示参数
   - 打开显示

### 4.3 NAND Flash配置步骤
1. **时钟使能**（需要更多GPIO）：
   ```c
   RCC_APB2PeriphClockCmd(RCC_APB2Periph_GPIOD | RCC_APB2Periph_GPIOE | RCC_APB2Periph_GPIOG, ENABLE);
   ```

2. **GPIO配置**（NAND需要更多控制线）：
   - 数据线：PD0-PD7
   - 控制线：CLE、ALE、CE、RE、WE、R/B
   - 具体引脚参考数据手册

3. **时序参数配置**（NAND需要更精确的时序）：
   ```c
   ComSpaceTiming.FSMC_SetupTime = 0;
   ComSpaceTiming.FSMC_WaitSetupTime = 3;
   ComSpaceTiming.FSMC_HoldSetupTime = 0;
   ```

4. **FSMC NAND配置**：
   ```c
   FSMC_NAND_InitStructure.FSMC_Bank = FSMC_Bank2_NAND;
   FSMC_NAND_InitStructure.FSMC_ECC = FSMC_ECC_Enable;
   FSMC_NAND_InitStructure.FSMC_ECCPageSize = FSMC_ECCPageSize_2048Bytes;
   ```

5. **NAND初始化**：
   - 读取NAND ID
   - 检查坏块
   - 初始化ECC

## 5. 示例工程详解

### 5.1 LCD示例（16位FSMC接口TFT LCD）
- **位置**：`FSMC/LCD/User/main.c`
- **功能**：16位FSMC接口驱动TFT LCD
- **特点**：
  - 16位并行接口
  - 高速数据传输
  - 支持多种分辨率LCD
  - 包含完整LCD驱动
- **适用场景**：
  - 图形显示界面
  - 工业HMI
  - 仪器仪表显示

### 5.2 LCD_8bit示例（8位FSMC接口LCD）
- **位置**：`FSMC/LCD_8bit/User/main.c`
- **功能**：8位FSMC接口驱动LCD（ST7789驱动）
- **特点**：
  - 8位并行接口
  - 节省GPIO引脚
  - 支持SPI接口LCD控制器
  - 软件模拟16位数据
- **适用场景**：
  - 低成本显示方案
  - GPIO资源受限的应用

### 5.3 SRAM示例（SRAM存储器操作）
- **位置**：`FSMC/SRAM/User/main.c`
- **功能**：外部SRAM存储器读写操作
- **特点**：
  - 16位SRAM接口
  - 高速数据存取
  - 包含读写测试函数
  - 支持大容量缓冲区
- **适用场景**：
  - 数据采集缓冲
  - 图像处理缓冲
  - 大容量临时存储

### 5.4 NORFLASH示例（NOR Flash存储器操作）
- **位置**：`FSMC/NORFLASH/User/main.c`
- **功能**：外部NOR Flash存储器操作
- **特点**：
  - NOR Flash读写操作
  - 支持扇区擦除
  - 包含坏块管理
  - 数据校验功能
- **适用场景**：
  - 程序存储扩展
  - 数据存储
  - 固件升级

### 5.5 NANDFLASH示例（NAND Flash存储器操作）
- **位置**：`FSMC/NANDFLASH/User/main.c`
- **功能**：外部NAND Flash存储器操作（带ECC）
- **特点**：
  - NAND Flash读写操作
  - 硬件ECC校验
  - 坏块管理
  - 磨损均衡支持
- **适用场景**：
  - 大容量数据存储
  - 文件系统支持
  - 数据记录仪

## 6. 常见问题

### Q1: FSMC初始化失败？
A: 检查以下配置：
1. GPIO时钟是否使能
2. FSMC时钟是否使能
3. GPIO模式是否正确（复用推挽输出）
4. 引脚映射是否正确
5. 时序参数是否合理

### Q2: SRAM/LCD读写数据错误？
A: 可能原因：
1. 时序参数不匹配（建立时间、保持时间）
2. 数据宽度配置错误
3. 存储器地址映射错误
4. 电源不稳定
5. 信号线干扰

### Q3: NAND Flash ECC错误？
A: 处理方法：
1. 检查ECC是否使能
2. 确认ECC页大小配置
3. 检查NAND Flash是否损坏
4. 重新读取数据并计算ECC
5. 使用软件ECC作为备份

### Q4: 多存储区如何选择？
A: 存储区选择原则：
1. NE1（片选1）：存储区1，地址0x60000000
2. NE2（片选2）：存储区2，地址0x64000000
3. NE3（片选3）：存储区3，地址0x68000000
4. NE4（片选4）：存储区4，地址0x6C000000
根据硬件连接选择对应存储区

### Q5: 性能优化建议？
A: 优化方向：
1. 合理设置时序参数（在不违反规格前提下尽量快）
2. 使用突发传输模式（如果存储器支持）
3. 优化数据结构，减少访问次数
4. 使用DMA传输大数据块
5. 合理使用Cache（如果MCU支持）

## 7. API参考

| 函数名 | 功能描述 | 参数说明 |
|--------|----------|----------|
| `FSMC_NORSRAMInit` | NOR/SRAM初始化 | FSMC_NORSRAMInitStruct：初始化结构体 |
| `FSMC_NORSRAMCmd` | 使能/禁用NOR/SRAM存储区 | FSMC_Bank：存储区，NewState：新状态 |
| `FSMC_NANDInit` | NAND Flash初始化 | FSMC_NAND_InitStruct：初始化结构体 |
| `FSMC_NANDCmd` | 使能/禁用NAND存储区 | FSMC_Bank：存储区，NewState：新状态 |
| `FSMC_NAND_ECC_Enable` | 使能NAND ECC | FSMC_Bank：存储区 |
| `FSMC_GetECC` | 获取ECC值 | FSMC_Bank：存储区 |
| `FSMC_ITConfig` | 使能/禁用FSMC中断 | FSMC_IT：中断源，NewState：新状态 |
| `FSMC_GetFlagStatus` | 获取标志位状态 | FSMC_Bank：存储区，FSMC_FLAG：标志位 |
| `FSMC_ClearFlag` | 清除标志位 | FSMC_Bank：存储区，FSMC_FLAG：标志位 |

## 8. 注意事项

1. **时序匹配**：FSMC时序必须与外部存储器时序匹配
2. **信号完整性**：高速信号需要注意PCB布局和阻抗匹配
3. **电源管理**：外部存储器需要稳定供电
4. **热插拔**：不支持热插拔，必须先断电再插拔
5. **地址对齐**：访问地址需要根据数据宽度对齐
6. **中断使用**：合理使用FSMC中断，避免频繁中断影响性能

## 9. 性能优化建议

1. **时序优化**：在不违反规格的前提下，尽量减小建立和保持时间
2. **突发传输**：如果存储器支持突发传输，可以显著提高连续访问性能
3. **数据宽度**：根据应用需求选择合适的数据宽度（8位/16位）
4. **内存管理**：合理规划存储区使用，减少存储区切换开销
5. **Cache使用**：如果MCU支持Cache，合理配置Cache策略

## 10. 典型应用场景

### 10.1 图形显示系统
```c
// 图形显示系统初始化
void GraphicDisplay_Init(void)
{
    // 1. 初始化FSMC接口
    FSMC_LCD_Init();

    // 2. 初始化LCD
    LCD_Init();

    // 3. 初始化图形库
    GUI_Init();

    // 4. 初始化触摸屏
    TouchScreen_Init();

    // 5. 显示启动界面
    Display_SplashScreen();
}

// 图形绘制函数
void Graphic_Draw(void)
{
    // 设置绘制颜色
    GUI_SetColor(GUI_RED);

    // 绘制矩形
    GUI_DrawRect(10, 10, 100, 100);

    // 绘制文本
    GUI_SetFont(&GUI_Font16_ASCII);
    GUI_DispStringAt("Hello World!", 50, 50);

    // 绘制图像
    GUI_DrawBitmap(&bitmap_image, 150, 50);
}
```

### 10.2 数据采集系统
```c
// 数据采集系统初始化
void DataAcquisition_Init(void)
{
    // 1. 初始化FSMC SRAM接口
    FSMC_SRAM_Init();

    // 2. 初始化ADC
    ADC_Init();

    // 3. 初始化DMA
    DMA_Init();

    // 4. 配置采样参数
    Configure_Sampling(1000, 1024);  // 1kHz采样率，1024点

    // 5. 启动数据采集
    Start_DataAcquisition();
}

// 数据处理函数
void Data_Process(void)
{
    // 从SRAM读取采集数据
    FSMC_SRAM_ReadBuffer(data_buffer, 0, DATA_BUFFER_SIZE);

    // 数据处理（滤波、FFT等）
    Data_Filter(data_buffer, DATA_BUFFER_SIZE);

    // 计算结果存储回SRAM
    FSMC_SRAM_WriteBuffer(processed_data, DATA_BUFFER_SIZE, RESULT_SIZE);

    // 显示或传输结果
    Display_Results(processed_data);
}
```

### 10.3 文件存储系统
```c
// 文件存储系统初始化
void FileStorage_Init(void)
{
    // 1. 初始化FSMC NAND接口
    FSMC_NAND_Init();

    // 2. 初始化文件系统
    FATFS_Init();

    // 3. 挂载文件系统
    f_mount(&fs, "", 1);

    // 4. 检查文件系统
    Check_FileSystem();

    // 5. 创建日志文件
    Create_LogFile();
}

// 数据存储函数
void Data_Storage(void)
{
    // 打开文件
    f_open(&file, "data.log", FA_WRITE | FA_OPEN_ALWAYS);

    // 写入数据
    f_write(&file, data_buffer, data_length, &bytes_written);

    // 关闭文件
    f_close(&file);

    // 检查写入结果
    if(bytes_written != data_length) {
        // 处理写入错误
        Handle_WriteError();
    }
}
```

## 11. 总结

CH32V307的FSMC外设为外部存储器和外设接口提供了强大的支持。通过合理配置FSMC参数，可以实现高速、可靠的外部存储器访问。FSMC支持多种存储器类型和接口模式，可以满足不同应用场景的需求。开发时需要注意时序匹配、信号完整性和性能优化等关键环节，确保外部存储器系统稳定可靠工作。
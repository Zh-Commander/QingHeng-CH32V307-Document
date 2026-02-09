# CH32V307 SPI外设使用指南

## 1. 外设概述

SPI（Serial Peripheral Interface，串行外设接口）是CH32V307的高速全双工同步串行通信接口，主要用于与外部设备进行高速数据交换。主要特性包括：

- **多种工作模式**：主模式、从模式
- **多种数据格式**：8位或16位数据帧格式
- **灵活的时钟配置**：时钟极性和相位可编程
- **多线工作模式**：单线、双线、全双工
- **高级功能**：硬件CRC计算、DMA传输、硬件NSS管理
- **高速传输**：最高支持系统时钟频率的1/2

## 2. 关键代码片段

### 2.1 SPI基本配置（主模式）

```c
// GPIO配置
GPIO_InitTypeDef GPIO_InitStructure = {0};
SPI_InitTypeDef SPI_InitStructure = {0};

// 使能时钟
RCC_APB2PeriphClockCmd(RCC_APB2Periph_GPIOA | RCC_APB2Periph_SPI1, ENABLE);

// SCK和MOSI引脚配置为复用推挽输出
GPIO_InitStructure.GPIO_Pin = GPIO_Pin_5 | GPIO_Pin_7;
GPIO_InitStructure.GPIO_Mode = GPIO_Mode_AF_PP;
GPIO_InitStructure.GPIO_Speed = GPIO_Speed_50MHz;
GPIO_Init(GPIOA, &GPIO_InitStructure);

// MISO引脚配置为浮空输入
GPIO_InitStructure.GPIO_Pin = GPIO_Pin_6;
GPIO_InitStructure.GPIO_Mode = GPIO_Mode_IN_FLOATING;
GPIO_Init(GPIOA, &GPIO_InitStructure);

// SPI参数配置
SPI_InitStructure.SPI_Direction = SPI_Direction_2Lines_FullDuplex; // 双线全双工
SPI_InitStructure.SPI_Mode = SPI_Mode_Master;                     // 主模式
SPI_InitStructure.SPI_DataSize = SPI_DataSize_8b;                // 8位数据
SPI_InitStructure.SPI_CPOL = SPI_CPOL_Low;                       // 时钟极性低
SPI_InitStructure.SPI_CPHA = SPI_CPHA_1Edge;                     // 时钟相位第1边沿
SPI_InitStructure.SPI_NSS = SPI_NSS_Soft;                        // 软件NSS
SPI_InitStructure.SPI_BaudRatePrescaler = SPI_BaudRatePrescaler_64; // 64分频
SPI_InitStructure.SPI_FirstBit = SPI_FirstBit_MSB;               // MSB先发送
SPI_InitStructure.SPI_CRCPolynomial = 7;                         // CRC多项式

SPI_Init(SPI1, &SPI_InitStructure);
SPI_Cmd(SPI1, ENABLE);  // 使能SPI
```

### 2.2 单线半双工模式（1Lines_half-duplex示例）

```c
// 单线发送模式配置
SPI_InitStructure.SPI_Direction = SPI_Direction_1Line_Tx;  // 单线发送
SPI_InitStructure.SPI_Mode = SPI_Mode_Master;             // 主模式
SPI_InitStructure.SPI_DataSize = SPI_DataSize_16b;        // 16位数据
SPI_InitStructure.SPI_CPOL = SPI_CPOL_High;               // 时钟极性高
SPI_InitStructure.SPI_CPHA = SPI_CPHA_1Edge;              // 时钟相位第1边沿
SPI_InitStructure.SPI_NSS = SPI_NSS_Soft;                 // 软件NSS
SPI_InitStructure.SPI_BaudRatePrescaler = SPI_BaudRatePrescaler_64; // 64分频
SPI_InitStructure.SPI_FirstBit = SPI_FirstBit_MSB;        // MSB先发送

// 中断处理函数
void SPI1_IRQHandler(void)
{
#if (SPI_MODE == HOST_MODE)
    if( SPI_I2S_GetITStatus( SPI1, SPI_I2S_IT_TXE ) != RESET )
    {
        SPI_I2S_SendData( SPI1, TxData[Txval++] );
        if( Txval == 18 )
        {
            SPI_I2S_ITConfig( SPI1, SPI_I2S_IT_TXE , DISABLE );
        }
    }
#elif (SPI_MODE == SLAVE_MODE)
    RxData[Rxval++] = SPI_I2S_ReceiveData( SPI1 );
#endif
}
```

### 2.3 双线全双工模式（2Lines_FullDuplex示例）

```c
// 双线全双工配置
SPI_InitStructure.SPI_Direction = SPI_Direction_2Lines_FullDuplex; // 双线全双工
SPI_InitStructure.SPI_Mode = SPI_Mode_Master;                     // 主模式
SPI_InitStructure.SPI_DataSize = SPI_DataSize_16b;                // 16位数据
SPI_InitStructure.SPI_CPOL = SPI_CPOL_Low;                        // 时钟极性低
SPI_InitStructure.SPI_CPHA = SPI_CPHA_1Edge;                      // 时钟相位第1边沿
SPI_InitStructure.SPI_NSS = SPI_NSS_Soft;                         // 软件NSS
SPI_InitStructure.SPI_BaudRatePrescaler = SPI_BaudRatePrescaler_64; // 64分频
SPI_InitStructure.SPI_FirstBit = SPI_FirstBit_LSB;                // LSB先发送

// 轮询发送接收
while((i < 18) || (j < 18))
{
    if(i < 18)
    {
        if(SPI_I2S_GetFlagStatus(SPI1, SPI_I2S_FLAG_TXE) != RESET)
        {
            SPI_I2S_SendData(SPI1, TxData[i]);
            i++;
        }
    }

    if(j < 18)
    {
        if(SPI_I2S_GetFlagStatus(SPI1, SPI_I2S_FLAG_RXNE) != RESET)
        {
            RxData[j] = SPI_I2S_ReceiveData(SPI1);
            j++;
        }
    }
}
```

### 2.4 硬件NSS模式（FullDuplex_HardNSS示例）

```c
// 硬件NSS模式配置
SPI_InitStructure.SPI_NSS = SPI_NSS_Hard;  // 硬件NSS模式

#if (SPI_MODE == HOST_MODE)
    SPI_SSOutputCmd( SPI1, ENABLE );       // 主模式使能NSS输出
#endif

// GPIO配置差异
#if (SPI_MODE == HOST_MODE)
    // 主模式：PA4配置为AF_PP（推挽输出）
    GPIO_InitStructure.GPIO_Mode = GPIO_Mode_AF_PP;
#elif (SPI_MODE == SLAVE_MODE)
    // 从模式：PA4配置为IPD（下拉输入）
    GPIO_InitStructure.GPIO_Mode = GPIO_Mode_IPD;
#endif
```

### 2.5 SPI CRC校验（SPI_CRC示例）

```c
// 使能CRC计算
SPI_CalculateCRC( SPI1, ENABLE );

// 主模式发送CRC
SPI_I2S_SendData( SPI1, TxData[i] );
SPI_TransmitCRC( SPI1 );  // 发送CRC值

// 从模式接收CRC
SPI_TransmitCRC( SPI1 );  // 准备接收CRC
if( SPI_I2S_GetFlagStatus( SPI1, SPI_I2S_FLAG_RXNE ) != RESET )
{
    RxData[i] = SPI_I2S_ReceiveData( SPI1 );
    i++;
}

// 获取CRC值
crcval = SPI_GetCRC( SPI1, SPI_CRC_Rx );
```

### 2.6 SPI DMA传输（SPI_DMA示例）

```c
// DMA初始化
void DMA_Tx_Init(DMA_Channel_TypeDef *DMA_CHx, u32 ppadr, u32 memadr, u16 bufsize)
{
    DMA_InitStructure.DMA_PeripheralBaseAddr = ppadr;      // 外设地址
    DMA_InitStructure.DMA_MemoryBaseAddr = memadr;         // 内存地址
    DMA_InitStructure.DMA_DIR = DMA_DIR_PeripheralDST;     // 方向：内存到外设
    DMA_InitStructure.DMA_BufferSize = bufsize;            // 缓冲区大小
    DMA_InitStructure.DMA_PeripheralInc = DMA_PeripheralInc_Disable; // 外设地址不递增
    DMA_InitStructure.DMA_MemoryInc = DMA_MemoryInc_Enable; // 内存地址递增
    DMA_InitStructure.DMA_PeripheralDataSize = DMA_PeripheralDataSize_HalfWord;
    DMA_InitStructure.DMA_MemoryDataSize = DMA_MemoryDataSize_HalfWord;
    DMA_InitStructure.DMA_Mode = DMA_Mode_Normal;          // 普通模式
    DMA_InitStructure.DMA_Priority = DMA_Priority_VeryHigh;
    DMA_InitStructure.DMA_M2M = DMA_M2M_Disable;           // 禁用内存到内存
}

// 使能SPI DMA请求
#if (SPI_MODE == HOST_MODE)
    SPI_I2S_DMACmd( SPI1, SPI_I2S_DMAReq_Tx, ENABLE );  // 使能TX DMA请求
    SPI_I2S_DMACmd( SPI1, SPI_I2S_DMAReq_Rx, ENABLE );  // 使能RX DMA请求
#endif
```

### 2.7 SPI操作FLASH（SPI_FLASH示例）

```c
// SPI FLASH专用配置
SPI_InitStructure.SPI_DataSize = SPI_DataSize_8b;        // 8位数据（FLASH标准）
SPI_InitStructure.SPI_CPOL = SPI_CPOL_High;              // 时钟极性高
SPI_InitStructure.SPI_CPHA = SPI_CPHA_2Edge;             // 时钟相位第2边沿
SPI_InitStructure.SPI_BaudRatePrescaler = SPI_BaudRatePrescaler_4; // 4分频（高速）
SPI_InitStructure.SPI_FirstBit = SPI_FirstBit_MSB;       // MSB先发送

// SPI字节读写函数
u8 SPI1_ReadWriteByte(u8 TxData)
{
    // 等待发送缓冲区空
    while(SPI_I2S_GetFlagStatus(SPI1, SPI_I2S_FLAG_TXE) == RESET);

    // 发送数据
    SPI_I2S_SendData(SPI1, TxData);

    // 等待接收缓冲区非空
    while(SPI_I2S_GetFlagStatus(SPI1, SPI_I2S_FLAG_RXNE) == RESET);

    // 读取数据
    return SPI_I2S_ReceiveData(SPI1);
}

// FLASH操作函数
u8 SPI_Flash_ReadID(void);
void SPI_Flash_Read(u32 ReadAddr, u8* pBuffer, u16 NumByteToRead);
void SPI_Flash_Write_Page(u32 WriteAddr, u8* pBuffer, u16 NumByteToWrite);
void SPI_Flash_Erase_Sector(u32 SectorAddr);
```

## 3. 配置数据结构

### 3.1 SPI_InitTypeDef（SPI初始化结构体）
```c
typedef struct
{
    uint16_t SPI_Direction;           // 数据传输方向
    uint16_t SPI_Mode;                // 工作模式（主/从）
    uint16_t SPI_DataSize;            // 数据大小（8位/16位）
    uint16_t SPI_CPOL;                // 时钟极性
    uint16_t SPI_CPHA;                // 时钟相位
    uint16_t SPI_NSS;                 // NSS管理（硬件/软件）
    uint16_t SPI_BaudRatePrescaler;   // 波特率预分频
    uint16_t SPI_FirstBit;            // 位顺序（MSB/LSB）
    uint16_t SPI_CRCPolynomial;       // CRC多项式
} SPI_InitTypeDef;
```

## 4. 通用配置步骤

### 4.1 SPI主模式配置步骤
1. **使能时钟**：
   ```c
   RCC_APB2PeriphClockCmd(RCC_APB2Periph_GPIOA | RCC_APB2Periph_SPI1, ENABLE);
   ```

2. **GPIO配置**：
   - SCK：`GPIO_Mode_AF_PP`（复用推挽输出）
   - MOSI：`GPIO_Mode_AF_PP`（复用推挽输出）
   - MISO：`GPIO_Mode_IN_FLOATING`（浮空输入）
   - NSS：根据模式选择（软件NSS或硬件NSS）

3. **SPI参数配置**：
   ```c
   SPI_InitStructure.SPI_Direction = SPI_Direction_2Lines_FullDuplex;
   SPI_InitStructure.SPI_Mode = SPI_Mode_Master;
   SPI_InitStructure.SPI_DataSize = SPI_DataSize_8b;
   SPI_InitStructure.SPI_CPOL = SPI_CPOL_Low;
   SPI_InitStructure.SPI_CPHA = SPI_CPHA_1Edge;
   SPI_InitStructure.SPI_NSS = SPI_NSS_Soft;
   SPI_InitStructure.SPI_BaudRatePrescaler = SPI_BaudRatePrescaler_64;
   SPI_InitStructure.SPI_FirstBit = SPI_FirstBit_MSB;
   SPI_InitStructure.SPI_CRCPolynomial = 7;
   ```

4. **初始化SPI**：
   ```c
   SPI_Init(SPI1, &SPI_InitStructure);
   SPI_Cmd(SPI1, ENABLE);
   ```

### 4.2 SPI从模式配置步骤
1. **使能时钟**
2. **GPIO配置**（与主模式类似）
3. **SPI参数配置**：
   ```c
   SPI_InitStructure.SPI_Mode = SPI_Mode_Slave;
   SPI_InitStructure.SPI_NSS = SPI_NSS_Hard;  // 从模式通常使用硬件NSS
   ```
4. **初始化SPI**

### 4.3 通信模式选择
.
| 模式 | 配置值 | 描述 |
|------|--------|------|
| 单线发送 | `SPI_Direction_1Line_Tx` | 单线半双工发送模式 |
| 单线接收 | `SPI_Direction_1Line_Rx` | 单线半双工接收模式 |
| 双线全双工 | `SPI_Direction_2Lines_FullDuplex` | 标准全双工SPI |
| 双线只接收 | `SPI_Direction_2Lines_RxOnly` | 双线只接收模式 |

### 4.4 时钟配置
- **SPI_CPOL**（时钟极性）：
  - `SPI_CPOL_Low`：时钟空闲时为低电平
  - `SPI_CPOL_High`：时钟空闲时为高电平
- **SPI_CPHA**（时钟相位）：
  - `SPI_CPHA_1Edge`：数据在第一个时钟边沿采样
  - `SPI_CPHA_2Edge`：数据在第二个时钟边沿采样

### 4.5 波特率预分频
```c
SPI_BaudRatePrescaler_2     // 2分频
SPI_BaudRatePrescaler_4     // 4分频
SPI_BaudRatePrescaler_8     // 8分频
SPI_BaudRatePrescaler_16    // 16分频
SPI_BaudRatePrescaler_32    // 32分频
SPI_BaudRatePrescaler_64    // 64分频
SPI_BaudRatePrescaler_128   // 128分频
SPI_BaudRatePrescaler_256   // 256分频
```

## 5. 示例工程详解

### 5.1 1Lines_half-duplex（单线半双工）
- **位置**：`SPI/1Lines_half-duplex/User/main.c`
- **功能**：单线半双工SPI通信，主从模式数据传输
- **适用场景**：简单的主从通信，节省IO引脚
- **特点**：中断方式，单线通信

### 5.2 2Lines_FullDuplex（双线全双工）
- **位置**：`SPI/2Lines_FullDuplex/User/main.c`
- **功能**：双线全双工SPI通信，主从同时收发
- **适用场景**：标准SPI全双工通信
- **特点**：轮询方式，双线通信

### 5.3 FullDuplex_HardNSS（硬件NSS全双工）
- **位置**：`SPI/FullDuplex_HardNSS/User/main.c`
- **功能**：硬件NSS模式下的全双工SPI通信
- **适用场景**：需要硬件自动片选控制的场景
- **特点**：硬件NSS控制，自动片选

### 5.4 SPI_CRC（CRC校验）
- **位置**：`SPI/SPI_CRC/User/main.c`
- **功能**：SPI CRC校验功能
- **适用场景**：数据完整性要求高的通信
- **特点**：硬件CRC计算，提高通信可靠性

### 5.5 SPI_DMA（DMA传输）
- **位置**：`SPI/SPI_DMA/User/main.c`
- **功能**：SPI DMA传输功能
- **适用场景**：大数据量传输，减少CPU占用
- **特点**：DMA传输，高效数据交换

### 5.6 SPI_FLASH（FLASH操作）
- **位置**：`SPI/SPI_FLASH/User/main.c`
- **功能**：SPI操作W25Qxx系列FLASH存储器
- **适用场景**：外部存储设备读写
- **特点**：FLASH专用驱动，支持读写擦除操作

### 5.7 SPI_LCD（LCD显示）
- **位置**：`SPI/SPI_LCD/User/main.c`
- **功能**：结合SPI FLASH和SPI LCD显示图像
- **适用场景**：图形显示应用
- **特点**：双SPI外设，FLASH存储图像，LCD显示

## 6. 常见问题

### Q1: SPI时钟配置注意事项？
A: SPI时钟必须正确配置，确保不超过外设最大频率：
- 检查系统时钟频率
- 合理选择预分频值
- 确保时钟极性和相位与从设备匹配

### Q2: NSS管理如何选择？
A:
- **软件NSS**：需要手动控制片选信号，灵活性高
- **硬件NSS**：自动控制片选信号，简化软件设计

### Q3: 数据对齐问题？
A: 注意MSB/LSB配置，确保主从设备一致：
- `SPI_FirstBit_MSB`：高位先发送
- `SPI_FirstBit_LSB`：低位先发送

### Q4: 中断处理注意事项？
A:
- 及时清除中断标志，避免重复进入中断
- 合理设置中断优先级
- 中断服务函数尽量简短

### Q5: DMA配置注意事项？
A:
- 注意DMA通道与SPI外设的映射关系
- 正确配置数据大小和对齐
- DMA传输完成后及时禁用DMA通道

### Q6: CRC使用注意事项？
A:
- CRC多项式需要主从设备一致
- 使能CRC计算后，数据传输会自动包含CRC
- 接收端需要检查CRC错误

### Q7: FLASH操作注意事项？
A:
- 注意FLASH的页大小和扇区大小限制
- 写入前需要先擦除
- 擦除和写入操作需要等待完成

## 7. API参考

| 函数名 | 功能描述 | 参数说明 |
|--------|----------|----------|
| `SPI_Init` | SPI初始化 | SPIx：SPI外设，SPI_InitStruct：初始化结构体 |
| `SPI_Cmd` | 使能/禁用SPI | SPIx：SPI外设，NewState：新状态 |
| `SPI_I2S_ITConfig` | 使能/禁用SPI中断 | SPIx：SPI外设，SPI_I2S_IT：中断源，NewState：新状态 |
| `SPI_I2S_DMACmd` | 使能/禁用SPI DMA | SPIx：SPI外设，SPI_I2S_DMAReq：DMA请求，NewState：新状态 |
| `SPI_I2S_SendData` | 发送数据 | SPIx：SPI外设，Data：要发送的数据 |
| `SPI_I2S_ReceiveData` | 接收数据 | SPIx：SPI外设 |
| `SPI_I2S_GetFlagStatus` | 获取标志位状态 | SPIx：SPI外设，SPI_I2S_FLAG：标志位 |
| `SPI_I2S_ClearFlag` | 清除标志位 | SPIx：SPI外设，SPI_I2S_FLAG：标志位 |
| `SPI_I2S_GetITStatus` | 获取中断状态 | SPIx：SPI外设，SPI_I2S_IT：中断源 |
| `SPI_I2S_ClearITPendingBit` | 清除中断挂起位 | SPIx：SPI外设，SPI_I2S_IT：中断源 |
| `SPI_CalculateCRC` | 使能/禁用CRC计算 | SPIx：SPI外设，NewState：新状态 |
| `SPI_TransmitCRC` | 发送CRC值 | SPIx：SPI外设 |
| `SPI_GetCRC` | 获取CRC值 | SPIx：SPI外设，SPI_CRC：CRC类型 |
| `SPI_SSOutputCmd` | 使能/禁用NSS输出 | SPIx：SPI外设，NewState：新状态 |

## 8. 总结

CH32V307的SPI外设功能强大，支持多种工作模式和高级功能。通过合理配置SPI参数，可以实现高速可靠的数据通信。在使用SPI时，需要注意时钟配置、NSS管理、中断处理、DMA使用等关键步骤，确保SPI功能正常工作。不同应用场景可以选择不同的SPI模式，如简单通信使用单线模式，高速通信使用全双工模式，大数据传输使用DMA模式。
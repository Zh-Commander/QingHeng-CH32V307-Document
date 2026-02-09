# CH32V307 CRC外设使用指南

本文档基于CH32V307 EVT示例代码分析，总结CRC外设的使用方法和配置要点。

## 一、概述

CRC（Cyclic Redundancy Check）循环冗余校验是一种常用的数据校验方法。CH32V307提供两种CRC计算方式：
1. **独立CRC计算单元**：专用的CRC硬件计算单元
2. **SPI硬件CRC**：集成在SPI外设中的CRC功能

## 二、独立CRC计算单元

### 2.1 硬件架构
独立CRC计算单元使用固定的CRC-32多项式：`0x4C11DB7`
- 数据宽度：32位
- 输入数据反转：无
- 输出数据反转：无
- 初始值：`0xFFFFFFFF`
- 结果异或值：`0x00000000`

### 2.2 寄存器结构
```c
typedef struct
{
  __IO uint32_t DATAR;    // 数据寄存器
  __IO uint8_t  IDATAR;   // 独立数据寄存器（ID寄存器）
  uint8_t   RESERVED0;
  uint16_t  RESERVED1;
  __IO uint32_t CTLR;     // 控制寄存器
} CRC_TypeDef;
```

**关键寄存器位**：
- `CRC_CTLR_RESET`：重置CRC数据寄存器
- `CRC_DATAR_DR`：数据寄存器位（32位）
- `CRC_IDATAR`：独立数据寄存器（8位）

### 2.3 CRC_Calculation示例

**文件路径**：`CRC\CRC_Calculation\User\main.c`

**用途**：演示如何使用独立的CRC计算单元进行32位CRC计算。

#### 2.3.1 关键代码

```c
// 1. 启用CRC时钟
RCC_AHBPeriphClockCmd(RCC_AHBPeriph_CRC, ENABLE);

// 2. 检查CRC是否在使用中（通过ID寄存器）
uint8_t CRC_Is_Used()
{
    if(CRC_GetIDRegister() == 0xaa)
        return 1;
    else
        return 0;
}

// 3. 重置CRC数据寄存器
void CRC_ResetDR(void);

// 4. 计算数据块的CRC值
uint32_t RecalculateCRC(uint32_t *SRC_BUF, uint32_t size)
{
    uint32_t temp;
    if(CRC_GetIDRegister() == 0xaa)
        CRC_ResetDR();
    temp = CRC_CalcBlockCRC((u32 *)SRC_BUF, size);
    CRC_SetIDRegister(0xaa);
    return temp;
}

// 5. 设置ID寄存器（用于标记CRC是否被使用）
void CRC_SetIDRegister(uint8_t IDValue);
```

#### 2.3.2 测试数据
```c
uint32_t SRC_BUF[32] = {
    0x00001021, 0x20423063, 0x408450a5, 0x60c670e7,
    0x9128a149, 0xb16ac18b, 0xd1ace1cd, 0xf1eef10f,
    0x12323243, 0x32746592, 0x52b542a5, 0x729762a7,
    0x92b983a9, 0xa2babacb, 0xcadadbea, 0xeadbfced,
    0x089d19ae, 0x299a2bab, 0x4c9d5dae, 0x6e9f7faf,
    0x80a19182, 0xa2a3b3c4, 0xc4c5d5e6, 0xe6c7f7f8,
    0x08f929fa, 0x4afb5bfc, 0x6cfd7dfe, 0x8eef9fff,
    0x0a1f2b3f, 0x4c5f6d7f, 0x8e9fabcf, 0x0ed1f2f3
};
```

#### 2.3.3 预期结果
对于上述测试数据，CRC计算结果应为：`0x199AC3CA`

#### 2.3.4 完整示例
```c
#include "ch32v30x.h"

uint32_t SRC_BUF[32] = { /* 同上 */ };

int main(void)
{
    uint32_t crc_result;

    SystemCoreClockUpdate();
    Delay_Init();
    USART_Printf_Init(115200);

    printf("CRC Calculation Test\n");

    // 启用CRC时钟
    RCC_AHBPeriphClockCmd(RCC_AHBPeriph_CRC, ENABLE);

    // 检查CRC状态
    if(CRC_Is_Used())
    {
        printf("CRC is in use, resetting...\n");
        CRC_ResetDR();
    }

    // 计算CRC
    crc_result = RecalculateCRC(SRC_BUF, 32);
    printf("CRC Result: 0x%08X\n", crc_result);

    // 验证结果
    if(crc_result == 0x199AC3CA)
        printf("CRC Check PASS\n");
    else
        printf("CRC Check FAIL\n");

    while(1);
}
```

## 三、SPI硬件CRC

### 3.1 SPI CRC特性
- CRC多项式：可配置（默认7）
- 数据宽度：8位或16位
- 硬件自动计算和验证
- 支持主从模式
- 提供发送和接收CRC寄存器

### 3.2 SPI_CRC示例

**文件路径**：`SPI\SPI_CRC\User\main.c`

**用途**：演示SPI通信中的硬件CRC校验功能，支持主从模式。

#### 3.2.1 主模式配置

```c
// SPI初始化时配置CRC多项式
SPI_InitStructure.SPI_CRCPolynomial = 7;

// 启用CRC计算
SPI_CalculateCRC(SPI1, ENABLE);

// 发送数据
if(SPI_I2S_GetFlagStatus(SPI1, SPI_I2S_FLAG_TXE) != RESET)
{
    SPI_I2S_SendData(SPI1, TxData[i]);
    SPI_TransmitCRC(SPI1);  // 发送CRC值
    i++;
}

// 获取CRC值
crcval = SPI_GetCRC(SPI1, SPI_CRC_Tx);  // 发送CRC
crcval = SPI_GetCRC(SPI1, SPI_CRC_Rx);  // 接收CRC
```

#### 3.2.2 从模式配置

```c
// 从模式SPI初始化
SPI_InitStructure.SPI_Mode = SPI_Mode_Slave;

// 启用CRC计算
SPI_CalculateCRC(SPI2, ENABLE);

// 接收数据并验证CRC
while(SPI_I2S_GetFlagStatus(SPI2, SPI_I2S_FLAG_RXNE) == RESET);
RxData = SPI_I2S_ReceiveData(SPI2);

// 检查CRC错误标志
if(SPI_I2S_GetFlagStatus(SPI2, SPI_I2S_FLAG_CRCERR) != RESET)
{
    printf("CRC Error!\n");
    SPI_I2S_ClearFlag(SPI2, SPI_I2S_FLAG_CRCERR);
}
```

#### 3.2.3 完整的主从通信示例

**主设备代码**：
```c
// 初始化SPI为主模式
SPI_InitStructure.SPI_Direction = SPI_Direction_2Lines_FullDuplex;
SPI_InitStructure.SPI_Mode = SPI_Mode_Master;
SPI_InitStructure.SPI_DataSize = SPI_DataSize_8b;
SPI_InitStructure.SPI_CPOL = SPI_CPOL_Low;
SPI_InitStructure.SPI_CPHA = SPI_CPHA_1Edge;
SPI_InitStructure.SPI_NSS = SPI_NSS_Soft;
SPI_InitStructure.SPI_BaudRatePrescaler = SPI_BaudRatePrescaler_256;
SPI_InitStructure.SPI_FirstBit = SPI_FirstBit_MSB;
SPI_InitStructure.SPI_CRCPolynomial = 7;  // CRC多项式
SPI_Init(SPI1, &SPI_InitStructure);

// 启用CRC计算
SPI_CalculateCRC(SPI1, ENABLE);

// 发送数据
uint8_t tx_data[] = {0x01, 0x02, 0x03, 0x04};
for(int i=0; i<4; i++)
{
    SPI_I2S_SendData(SPI1, tx_data[i]);
    while(SPI_I2S_GetFlagStatus(SPI1, SPI_I2S_FLAG_TXE) == RESET);
}

// 发送CRC
SPI_TransmitCRC(SPI1);

// 获取发送的CRC值
uint16_t tx_crc = SPI_GetCRC(SPI1, SPI_CRC_Tx);
printf("Transmitted CRC: 0x%04X\n", tx_crc);
```

**从设备代码**：
```c
// 初始化SPI为从模式
SPI_InitStructure.SPI_Mode = SPI_Mode_Slave;
SPI_InitStructure.SPI_CRCPolynomial = 7;
SPI_Init(SPI2, &SPI_InitStructure);

// 启用CRC计算
SPI_CalculateCRC(SPI2, ENABLE);

// 接收数据
uint8_t rx_data[4];
for(int i=0; i<4; i++)
{
    while(SPI_I2S_GetFlagStatus(SPI2, SPI_I2S_FLAG_RXNE) == RESET);
    rx_data[i] = SPI_I2S_ReceiveData(SPI2);
}

// 接收CRC
while(SPI_I2S_GetFlagStatus(SPI2, SPI_I2S_FLAG_RXNE) == RESET);
uint16_t rx_crc = SPI_I2S_ReceiveData(SPI2);

// 检查CRC错误
if(SPI_I2S_GetFlagStatus(SPI2, SPI_I2S_FLAG_CRCERR) != RESET)
{
    printf("CRC Error Detected!\n");
    SPI_I2S_ClearFlag(SPI2, SPI_I2S_FLAG_CRCERR);
}
else
{
    uint16_t calc_crc = SPI_GetCRC(SPI2, SPI_CRC_Rx);
    printf("Received CRC: 0x%04X, Calculated CRC: 0x%04X\n", rx_crc, calc_crc);
}
```

## 四、API函数详解

### 4.1 独立CRC单元API

#### 4.1.1 `void CRC_ResetDR(void)`
**功能**：重置CRC数据寄存器为初始值`0xFFFFFFFF`
**使用示例**：
```c
// 开始新的CRC计算前重置
CRC_ResetDR();
```

#### 4.1.2 `uint32_t CRC_CalcCRC(uint32_t Data)`
**功能**：计算单个32位数据的CRC
**参数**：
- `Data`：要计算CRC的32位数据
**返回值**：计算后的CRC值
**使用示例**：
```c
uint32_t data = 0x12345678;
uint32_t crc = CRC_CalcCRC(data);
```

#### 4.1.3 `uint32_t CRC_CalcBlockCRC(uint32_t pBuffer[], uint32_t BufferLength)`
**功能**：计算数据块的CRC
**参数**：
- `pBuffer`：数据缓冲区指针
- `BufferLength`：数据长度（32位字个数）
**返回值**：计算后的CRC值
**使用示例**：
```c
uint32_t buffer[10] = { /* 数据 */ };
uint32_t crc = CRC_CalcBlockCRC(buffer, 10);
```

#### 4.1.4 `uint32_t CRC_GetCRC(void)`
**功能**：获取当前CRC值
**返回值**：当前CRC值
**使用示例**：
```c
uint32_t current_crc = CRC_GetCRC();
```

#### 4.1.5 `void CRC_SetIDRegister(uint8_t IDValue)`
**功能**：设置ID寄存器值
**参数**：
- `IDValue`：ID值（8位）
**使用示例**：
```c
// 标记CRC正在使用
CRC_SetIDRegister(0xAA);
```

#### 4.1.6 `uint8_t CRC_GetIDRegister(void)`
**功能**：获取ID寄存器值
**返回值**：ID寄存器值
**使用示例**：
```c
uint8_t id = CRC_GetIDRegister();
if(id == 0xAA)
    printf("CRC is in use\n");
```

### 4.2 SPI CRC API

#### 4.2.1 `void SPI_TransmitCRC(SPI_TypeDef* SPIx)`
**功能**：发送CRC值
**参数**：
- `SPIx`：SPI外设指针（SPI1、SPI2等）
**使用示例**：
```c
// 发送数据后发送CRC
SPI_TransmitCRC(SPI1);
```

#### 4.2.2 `void SPI_CalculateCRC(SPI_TypeDef* SPIx, FunctionalState NewState)`
**功能**：启用或禁用CRC计算
**参数**：
- `SPIx`：SPI外设指针
- `NewState`：新状态（ENABLE或DISABLE）
**使用示例**：
```c
// 启用CRC计算
SPI_CalculateCRC(SPI1, ENABLE);

// 禁用CRC计算
SPI_CalculateCRC(SPI1, DISABLE);
```

#### 4.2.3 `uint16_t SPI_GetCRC(SPI_TypeDef* SPIx, uint8_t SPI_CRC)`
**功能**：获取CRC值
**参数**：
- `SPIx`：SPI外设指针
- `SPI_CRC`：CRC类型
  - `SPI_CRC_Tx`：发送CRC
  - `SPI_CRC_Rx`：接收CRC
**返回值**：CRC值
**使用示例**：
```c
uint16_t tx_crc = SPI_GetCRC(SPI1, SPI_CRC_Tx);
uint16_t rx_crc = SPI_GetCRC(SPI1, SPI_CRC_Rx);
```

#### 4.2.4 `uint16_t SPI_GetCRCPolynomial(SPI_TypeDef* SPIx)`
**功能**：获取CRC多项式
**参数**：
- `SPIx`：SPI外设指针
**返回值**：CRC多项式值
**使用示例**：
```c
uint16_t poly = SPI_GetCRCPolynomial(SPI1);
printf("CRC Polynomial: 0x%04X\n", poly);
```

## 五、使用注意事项

### 5.1 独立CRC单元注意事项

#### 5.1.1 多项式固定
```c
// CH32V307的CRC多项式固定为0x4C11DB7，不可更改
// 与其他CRC32实现（如CRC32/MPEG-2）不同
```

#### 5.1.2 数据对齐
```c
// CRC计算要求32位数据对齐
uint32_t aligned_buffer[10];  // 正确
uint8_t unaligned_buffer[40]; // 错误，需要转换为uint32_t指针
```

#### 5.1.3 ID寄存器用途
```c
// ID寄存器可用于标记CRC状态
// 典型用法：0xAA表示CRC正在使用，0x00表示空闲
CRC_SetIDRegister(0xAA);  // 标记为使用中
// ... CRC计算 ...
CRC_SetIDRegister(0x00);  // 标记为空闲
```

#### 5.1.4 重置时机
```c
// 开始新的CRC计算前必须重置
CRC_ResetDR();
// 或者通过ID寄存器判断是否需要重置
if(CRC_GetIDRegister() == 0xAA)
    CRC_ResetDR();
```

### 5.2 SPI CRC注意事项

#### 5.2.1 多项式配置
```c
// SPI CRC多项式可配置，默认值为7
// 多项式寄存器为16位，实际使用低8位
SPI_InitStructure.SPI_CRCPolynomial = 7;  // 常用值
```

#### 5.2.2 CRC使能时机
```c
// 必须在SPI初始化后，通信开始前使能CRC
SPI_Init(SPI1, &SPI_InitStructure);
SPI_CalculateCRC(SPI1, ENABLE);  // 正确时机

// 通信过程中不要改变CRC使能状态
```

#### 5.2.3 数据宽度匹配
```c
// CRC计算的数据宽度必须与SPI数据宽度匹配
SPI_InitStructure.SPI_DataSize = SPI_DataSize_8b;   // 8位数据
// CRC计算基于8位数据

SPI_InitStructure.SPI_DataSize = SPI_DataSize_16b;  // 16位数据
// CRC计算基于16位数据
```

#### 5.2.4 错误处理
```c
// 必须及时检查和处理CRC错误
if(SPI_I2S_GetFlagStatus(SPI2, SPI_I2S_FLAG_CRCERR) != RESET)
{
    printf("CRC Error!\n");
    SPI_I2S_ClearFlag(SPI2, SPI_I2S_FLAG_CRCERR);
    // 重传或其他错误处理
}
```

## 六、常见问题解答

### 6.1 CRC计算结果验证

**问题**：如何验证CRC计算结果是否正确？
**解答**：
```c
// 使用已知数据测试
uint32_t test_data[] = {0x12345678, 0x9ABCDEF0};
uint32_t expected_crc = 0x0C1B0A0D;  // 预期结果

CRC_ResetDR();
uint32_t calculated_crc = CRC_CalcBlockCRC(test_data, 2);

if(calculated_crc == expected_crc)
    printf("CRC Calculation Correct\n");
else
    printf("CRC Calculation Error: 0x%08X != 0x%08X\n",
           calculated_crc, expected_crc);
```

### 6.2 SPI CRC错误处理

**问题**：SPI CRC错误频繁发生怎么办？
**解答**：
1. 检查SPI时钟频率是否过高
2. 验证主从设备CRC多项式配置是否一致
3. 检查数据宽度配置是否匹配
4. 确认SPI模式（CPOL/CPHA）配置一致
5. 检查硬件连接是否可靠

### 6.3 性能考虑

**问题**：CRC计算对系统性能有什么影响？
**解答**：
1. **独立CRC单元**：硬件计算，对CPU性能影响小
2. **SPI硬件CRC**：自动计算，不增加CPU负担
3. **软件CRC**：占用CPU资源，速度慢

**建议**：优先使用硬件CRC，特别是大数据量场景。

### 6.4 兼容性问题

**问题**：CH32V307的CRC与其他设备兼容吗？
**解答**：
1. **独立CRC单元**：使用固定多项式0x4C11DB7，与标准CRC32不同
   - 如果需要兼容，可能需要软件实现
2. **SPI CRC**：多项式可配置，可与其他设备匹配
   - 配置相同多项式即可兼容

## 七、应用场景

### 7.1 独立CRC单元应用

#### 7.1.1 固件完整性校验
```c
// 计算固件CRC并验证
uint32_t Calculate_Firmware_CRC(uint32_t *firmware_start, uint32_t size)
{
    CRC_ResetDR();
    return CRC_CalcBlockCRC(firmware_start, size);
}

bool Verify_Firmware_CRC(uint32_t *firmware_start, uint32_t size,
                         uint32_t expected_crc)
{
    uint32_t calculated_crc = Calculate_Firmware_CRC(firmware_start, size);
    return (calculated_crc == expected_crc);
}
```

#### 7.1.2 通信数据校验
```c
// 通信数据包CRC校验
typedef struct {
    uint32_t header;
    uint8_t data[100];
    uint32_t crc;
} DataPacket;

bool Verify_Packet_CRC(DataPacket *packet)
{
    // 计算除crc字段外的所有数据
    CRC_ResetDR();
    uint32_t header_crc = CRC_CalcCRC(packet->header);

    // 计算数据部分CRC（需要转换为uint32_t指针）
    uint32_t *data_ptr = (uint32_t *)packet->data;
    uint32_t data_size = sizeof(packet->data) / 4;  // 转换为32位字数
    uint32_t data_crc = CRC_CalcBlockCRC(data_ptr, data_size);

    // 组合CRC（简化示例，实际需要更复杂的计算）
    uint32_t calculated_crc = header_crc ^ data_crc;

    return (calculated_crc == packet->crc);
}
```

### 7.2 SPI CRC应用

#### 7.2.1 可靠SPI通信
```c
// 带CRC校验的SPI数据传输
bool SPI_Transmit_With_CRC(SPI_TypeDef* SPIx, uint8_t *data, uint16_t size)
{
    // 启用CRC
    SPI_CalculateCRC(SPIx, ENABLE);

    // 发送数据
    for(int i=0; i<size; i++)
    {
        SPI_I2S_SendData(SPIx, data[i]);
        while(SPI_I2S_GetFlagStatus(SPIx, SPI_I2S_FLAG_TXE) == RESET);
    }

    // 发送CRC
    SPI_TransmitCRC(SPIx);

    // 检查从设备响应
    // ... 等待从设备确认

    return true;
}
```

#### 7.2.2 存储器数据校验
```c
// 读写SPI Flash时使用CRC校验
bool SPI_Flash_Write_With_CRC(uint32_t address, uint8_t *data, uint16_t size)
{
    // 发送写命令
    SPI_Flash_Write_Enable();

    // 发送地址和数据
    uint8_t cmd[4] = {0x02,  // 页写命令
                     (address >> 16) & 0xFF,
                     (address >> 8) & 0xFF,
                     address & 0xFF};

    // 启用CRC
    SPI_CalculateCRC(SPI1, ENABLE);

    // 发送命令和地址
    for(int i=0; i<4; i++)
    {
        SPI_I2S_SendData(SPI1, cmd[i]);
        while(SPI_I2S_GetFlagStatus(SPI1, SPI_I2S_FLAG_TXE) == RESET);
    }

    // 发送数据
    for(int i=0; i<size; i++)
    {
        SPI_I2S_SendData(SPI1, data[i]);
        while(SPI_I2S_GetFlagStatus(SPI1, SPI_I2S_FLAG_TXE) == RESET);
    }

    // 发送CRC
    SPI_TransmitCRC(SPI1);

    // 等待写完成
    SPI_Flash_Wait_Busy();

    return true;
}
```

## 八、参考资源

### 8.1 文件路径
1. **CRC计算示例**：`F:\WCH_Project\资料\CH32V307EVT_评估版原理图和示例\EVT\EXAM\CRC\CRC_Calculation\User\main.c`
2. **SPI CRC示例**：`F:\WCH_Project\资料\CH32V307EVT_评估版原理图和示例\EVT\EXAM\SPI\SPI_CRC\User\main.c`
3. **CRC外设头文件**：`F:\WCH_Project\资料\CH32V307EVT_评估版原理图和示例\EVT\EXAM\SRC\Peripheral\inc\ch32v30x_crc.h`
4. **CRC外设源文件**：`F:\WCH_Project\资料\CH32V307EVT_评估版原理图和示例\EVT\EXAM\SRC\Peripheral\src\ch32v30x_crc.c`
5. **SPI CRC相关头文件**：`F:\WCH_Project\资料\CH32V307EVT_评估版原理图和示例\EVT\EXAM\SRC\Peripheral\inc\ch32v30x_spi.h`
6. **SPI CRC相关源文件**：`F:\WCH_Project\资料\CH32V307EVT_评估版原理图和示例\EVT\EXAM\SRC\Peripheral\src\ch32v30x_spi.c`

### 8.2 调试技巧
1. **独立CRC调试**：
   - 使用已知数据验证CRC计算
   - 检查ID寄存器状态
   - 确认数据对齐

2. **SPI CRC调试**：
   - 检查主从设备CRC多项式是否一致
   - 验证SPI模式配置
   - 使用逻辑分析仪观察SPI波形

3. **性能测试**：
   - 比较硬件CRC与软件CRC速度
   - 测试不同数据量下的CRC计算时间
   - 验证CRC错误检测能力

## 总结

CH32V307提供两种CRC计算方式：独立的CRC计算单元和SPI硬件CRC。独立CRC单元使用固定的CRC-32多项式，适合通用数据校验；SPI硬件CRC集成在SPI外设中，适合SPI通信数据校验。

关键配置要点：
1. **独立CRC单元**：注意多项式固定、数据对齐、ID寄存器使用
2. **SPI CRC**：注意多项式配置、使能时机、错误处理

建议开发者在实际应用中根据需求选择合适的CRC方式，并注意兼容性和性能考虑。
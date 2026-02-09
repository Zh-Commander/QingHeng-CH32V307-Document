# CH32V307 DMA外设使用指南

本文档基于CH32V307 EVT示例代码分析，总结DMA外设的使用方法和配置要点。

## 一、DMA概述

直接内存访问（DMA）是一种无需CPU干预的数据传输机制，可以显著提高系统性能，特别适用于大数据量传输场景。

### 主要特性：
- 支持7个独立通道
- 每个通道可配置优先级
- 支持内存到内存、内存到外设、外设到内存传输
- 支持循环模式和普通模式
- 支持8位、16位、32位数据传输
- 支持地址递增/固定

## 二、DMA通道分配

| 通道 | DMA1 | DMA2 | 外设映射 |
|------|------|------|----------|
| 通道1 | ✓ | ✓ | ADC1, SPI2_RX, I2S2_RX |
| 通道2 | ✓ | ✓ | SPI1_RX, USART3_TX |
| 通道3 | ✓ | ✓ | SPI1_TX, USART3_RX, DAC1 |
| 通道4 | ✓ | ✓ | SPI2_TX, I2S2_TX, DAC2 |
| 通道5 | ✓ | ✓ | SPI2_RX, I2S2_RX, TIM1_CH1 |
| 通道6 | ✓ | ✓ | SPI2_TX, I2S2_TX, USART2_RX |
| 通道7 | ✓ | - | USART2_TX |

## 三、DMA配置结构体详解

```c
typedef struct {
  uint32_t DMA_PeripheralBaseAddr;  // 外设基地址
  uint32_t DMA_MemoryBaseAddr;      // 内存基地址
  uint32_t DMA_DIR;                 // 传输方向
  uint32_t DMA_BufferSize;          // 缓冲区大小
  uint32_t DMA_PeripheralInc;       // 外设地址递增
  uint32_t DMA_MemoryInc;           // 内存地址递增
  uint32_t DMA_PeripheralDataSize;  // 外设数据宽度
  uint32_t DMA_MemoryDataSize;      // 内存数据宽度
  uint32_t DMA_Mode;                // 工作模式
  uint32_t DMA_Priority;            // 通道优先级
  uint32_t DMA_M2M;                 // 内存到内存模式
} DMA_InitTypeDef;
```

## 四、DMA使用示例

### 4.1 内存到内存传输（DMA_MEM2MEM示例）

**用途**：演示内存到内存数据传输

**关键配置**：
```c
DMA_InitStructure.DMA_PeripheralBaseAddr = (u32)(SRC_BUF);
DMA_InitStructure.DMA_MemoryBaseAddr = (u32)DST_BUF;
DMA_InitStructure.DMA_DIR = DMA_DIR_PeripheralSRC;
DMA_InitStructure.DMA_BufferSize = Buf_Size * 4;  // 32个32位数据
DMA_InitStructure.DMA_PeripheralInc = DMA_PeripheralInc_Enable;
DMA_InitStructure.DMA_MemoryInc = DMA_MemoryInc_Enable;
DMA_InitStructure.DMA_PeripheralDataSize = DMA_PeripheralDataSize_Byte;
DMA_InitStructure.DMA_MemoryDataSize = DMA_MemoryDataSize_Byte;
DMA_InitStructure.DMA_Mode = DMA_Mode_Normal;
DMA_InitStructure.DMA_Priority = DMA_Priority_VeryHigh;
DMA_InitStructure.DMA_M2M = DMA_M2M_Enable;  // 关键：使能M2M模式
```

**应用场景**：大数据块复制、缓冲区初始化

### 4.2 ADC DMA采样（ADC_DMA示例）

**用途**：ADC采样数据通过DMA传输到内存

**关键配置**：
```c
DMA_InitStructure.DMA_PeripheralBaseAddr = (u32)&ADC1->RDATAR;
DMA_InitStructure.DMA_MemoryBaseAddr = (u32)TxBuf;
DMA_InitStructure.DMA_DIR = DMA_DIR_PeripheralSRC;  // ADC到内存
DMA_InitStructure.DMA_BufferSize = 1024;  // 1024个半字
DMA_InitStructure.DMA_PeripheralInc = DMA_PeripheralInc_Disable;  // ADC地址固定
DMA_InitStructure.DMA_MemoryInc = DMA_MemoryInc_Enable;  // 内存地址递增
DMA_InitStructure.DMA_PeripheralDataSize = DMA_PeripheralDataSize_HalfWord;
DMA_InitStructure.DMA_MemoryDataSize = DMA_MemoryDataSize_HalfWord;
DMA_InitStructure.DMA_Mode = DMA_Mode_Normal;
```

**ADC配置**：
```c
ADC_DMACmd(ADC1, ENABLE);  // 使能ADC DMA
ADC_SoftwareStartConvCmd(ADC1, ENABLE);  // 启动转换
```

**应用场景**：连续数据采集、实时信号处理

### 4.3 DAC DMA输出（DAC_DMA示例）

**用途**：内存数据通过DMA传输到DAC输出

**关键配置**：
```c
DMA_InitStructure.DMA_PeripheralBaseAddr = (u32)&(DAC->R12BDHR1);
DMA_InitStructure.DMA_MemoryBaseAddr = (u32)&dacbuff16bit;
DMA_InitStructure.DMA_DIR = DMA_DIR_PeripheralDST;  // 内存到DAC
DMA_InitStructure.DMA_BufferSize = 8;
DMA_InitStructure.DMA_PeripheralInc = DMA_PeripheralInc_Disable;
DMA_InitStructure.DMA_MemoryInc = DMA_MemoryInc_Enable;
DMA_InitStructure.DMA_PeripheralDataSize = DMA_PeripheralDataSize_HalfWord;
DMA_InitStructure.DMA_MemoryDataSize = DMA_MemoryDataSize_HalfWord;
DMA_InitStructure.DMA_Mode = DMA_Mode_Circular;  // 循环模式
```

**DAC配置**：
```c
DAC_InitType.DAC_Trigger = DAC_Trigger_T8_TRGO;  // TIM8触发
DAC_DMACmd(DAC_Channel_1, ENABLE);
```

**应用场景**：波形生成、音频输出

### 4.4 USART DMA通信（USART_DMA示例）

**用途**：USART通过DMA进行全双工通信

**关键配置**：
```c
// USART2 TX配置（内存到外设）
DMA_InitStructure.DMA_PeripheralBaseAddr = (u32)(&USART2->DATAR);
DMA_InitStructure.DMA_MemoryBaseAddr = (u32)TxBuffer1;
DMA_InitStructure.DMA_DIR = DMA_DIR_PeripheralDST;
DMA_InitStructure.DMA_BufferSize = TxSize1;
DMA_InitStructure.DMA_PeripheralInc = DMA_PeripheralInc_Disable;
DMA_InitStructure.DMA_MemoryInc = DMA_MemoryInc_Enable;
DMA_InitStructure.DMA_PeripheralDataSize = DMA_PeripheralDataSize_Byte;
DMA_InitStructure.DMA_MemoryDataSize = DMA_MemoryDataSize_Byte;
DMA_InitStructure.DMA_Mode = DMA_Mode_Normal;

// USART2 RX配置（外设到内存）
DMA_InitStructure.DMA_PeripheralBaseAddr = (u32)(&USART2->DATAR);
DMA_InitStructure.DMA_MemoryBaseAddr = (u32)RxBuffer1;
DMA_InitStructure.DMA_DIR = DMA_DIR_PeripheralSRC;
DMA_InitStructure.DMA_BufferSize = RxSize1;
```

**USART配置**：
```c
USART_DMACmd(USART2, USART_DMAReq_Tx, ENABLE);
USART_DMACmd(USART2, USART_DMAReq_Rx, ENABLE);
```

**应用场景**：高速串口通信、数据透传

### 4.5 TIM DMA PWM控制（TIM_DMA示例）

**用途**：通过DMA更新TIM1的PWM占空比

**关键配置**：
```c
DMA_InitStructure.DMA_PeripheralBaseAddr = ppadr;  // TIM1_CH1CVR地址
DMA_InitStructure.DMA_MemoryBaseAddr = memadr;     // 占空比数组
DMA_InitStructure.DMA_DIR = DMA_DIR_PeripheralDST; // 内存到外设
DMA_InitStructure.DMA_BufferSize = bufsize;        // 3个半字
DMA_InitStructure.DMA_PeripheralInc = DMA_PeripheralInc_Disable;
DMA_InitStructure.DMA_MemoryInc = DMA_MemoryInc_Enable;
DMA_InitStructure.DMA_PeripheralDataSize = DMA_PeripheralDataSize_HalfWord;
DMA_InitStructure.DMA_MemoryDataSize = DMA_MemoryDataSize_HalfWord;
DMA_InitStructure.DMA_Mode = DMA_Mode_Circular;    // 循环模式
```

**TIM配置**：
```c
TIM_DMACmd(TIM1, TIM_DMA_Update, ENABLE);
TIM_Cmd(TIM1, ENABLE);
```

**应用场景**：PWM波形控制、LED调光

### 4.6 SPI DMA通信（SPI_DMA示例）

**用途**：SPI全双工DMA通信

**关键配置**：
```c
// SPI1 TX配置（内存到外设）
DMA_InitStructure.DMA_DIR = DMA_DIR_PeripheralDST;
DMA_InitStructure.DMA_Mode = DMA_Mode_Normal;
DMA_InitStructure.DMA_Priority = DMA_Priority_VeryHigh;

// SPI1 RX配置（外设到内存）
DMA_InitStructure.DMA_DIR = DMA_DIR_PeripheralSRC;
DMA_InitStructure.DMA_Mode = DMA_Mode_Circular;
DMA_InitStructure.DMA_Priority = DMA_Priority_High;
```

**SPI配置**：
```c
SPI_I2S_DMACmd(SPI1, SPI_I2S_DMAReq_Tx, ENABLE);
SPI_I2S_DMACmd(SPI1, SPI_I2S_DMAReq_Rx, ENABLE);
```

**应用场景**：高速SPI通信、存储器访问

### 4.7 I2C DMA通信（I2C_DMA示例）

**用途**：I2C通过DMA进行主从通信

**关键配置**：
```c
DMA_InitStructure.DMA_PeripheralBaseAddr = ppadr;  // I2C数据寄存器
DMA_InitStructure.DMA_MemoryBaseAddr = memadr;
DMA_InitStructure.DMA_DIR = DMA_DIR_PeripheralDST; // 内存到外设
DMA_InitStructure.DMA_BufferSize = bufsize;
DMA_InitStructure.DMA_PeripheralInc = DMA_PeripheralInc_Disable;
DMA_InitStructure.DMA_MemoryInc = DMA_MemoryInc_Enable;
DMA_InitStructure.DMA_PeripheralDataSize = DMA_PeripheralDataSize_Byte;
DMA_InitStructure.DMA_MemoryDataSize = DMA_MemoryDataSize_Byte;
DMA_InitStructure.DMA_Mode = DMA_Mode_Normal;
```

**I2C配置**：
```c
I2C_DMACmd(I2C1, I2C_DMAReq_Tx, ENABLE);
```

**应用场景**：I2C设备通信、传感器数据读取

### 4.8 I2S DMA音频传输（I2S_DMA示例）

**用途**：I2S音频数据DMA传输

**关键配置**：
```c
// I2S2 TX配置（内存到外设）
DMA_InitStructure.DMA_DIR = DMA_DIR_PeripheralDST;
DMA_InitStructure.DMA_PeripheralBaseAddr = (u32)&SPI2->DATAR;

// I2S3 RX配置（外设到内存）
DMA_InitStructure.DMA_DIR = DMA_DIR_PeripheralSRC;
DMA_InitStructure.DMA_PeripheralBaseAddr = (u32)&SPI3->DATAR;
```

**I2S配置**：
```c
SPI_I2S_DMACmd(SPI2, SPI_I2S_DMAReq_Tx, ENABLE);
SPI_I2S_DMACmd(SPI3, SPI_I2S_DMAReq_Rx, ENABLE);
```

**应用场景**：音频播放、录音

### 4.9 DVP DMA图像传输（DVP_TFTLCD示例）

**用途**：DVP摄像头数据通过DMA传输到LCD

**关键配置**：
```c
DMA_InitStructure.DMA_PeripheralBaseAddr = ppadr;  // DVP数据地址
DMA_InitStructure.DMA_MemoryBaseAddr = memadr;     // LCD显存地址
DMA_InitStructure.DMA_DIR = DMA_DIR_PeripheralSRC; // DVP到LCD
DMA_InitStructure.DMA_PeripheralInc = DMA_PeripheralInc_Enable;
DMA_InitStructure.DMA_MemoryInc = DMA_MemoryInc_Disable;
DMA_InitStructure.DMA_PeripheralDataSize = DMA_PeripheralDataSize_HalfWord;
DMA_InitStructure.DMA_MemoryDataSize = DMA_MemoryDataSize_HalfWord;
DMA_InitStructure.DMA_Mode = DMA_Mode_Normal;
DMA_InitStructure.DMA_M2M = DMA_M2M_Enable;
```

**应用场景**：图像采集、视频处理

### 4.10 WS2812 LED控制（WS2812_LED示例）

**用途**：WS2812 LED控制通过SPI和TIM DMA

**关键配置**：
```c
// SPI1 DMA TX配置
DMA_InitStructure.DMA_PeripheralBaseAddr = (u32)&SPI1->DATAR;
DMA_InitStructure.DMA_MemoryBaseAddr = (u32)SPI1_DMA_TX_Buf;
DMA_InitStructure.DMA_DIR = DMA_DIR_PeripheralDST;

// TIM1 DMA配置
DMA_InitStructure.DMA_PeripheralBaseAddr = (u32)&TIM1->CH1CVR;
DMA_InitStructure.DMA_MemoryBaseAddr = (u32)PWM_Duty;
DMA_InitStructure.DMA_DIR = DMA_DIR_PeripheralDST;
```

**应用场景**：LED控制、灯光效果

## 五、DMA编程步骤

### 5.1 基本步骤
```c
// 1. 使能DMA时钟
RCC_AHBPeriphClockCmd(RCC_AHBPeriph_DMA1, ENABLE);

// 2. 初始化DMA结构体
DMA_InitTypeDef DMA_InitStructure;
DMA_StructInit(&DMA_InitStructure);  // 初始化默认值

// 3. 配置DMA通道
DMA_InitStructure.DMA_PeripheralBaseAddr = PeripheralAddr;
DMA_InitStructure.DMA_MemoryBaseAddr = MemoryAddr;
DMA_InitStructure.DMA_DIR = Direction;
DMA_InitStructure.DMA_BufferSize = BufferSize;
// ... 其他配置
DMA_Init(DMA_Channel, &DMA_InitStructure);

// 4. 使能DMA通道
DMA_Cmd(DMA_Channel, ENABLE);

// 5. 配置外设DMA请求
Peripheral_DMACmd(Peripheral, Peripheral_DMAReq, ENABLE);

// 6. 等待传输完成或使用中断
while(DMA_GetFlagStatus(DMA_FLAG_TC) == RESET);
// 或
DMA_ITConfig(DMA_Channel, DMA_IT_TC, ENABLE);
NVIC_EnableIRQ(DMA_Channel_IRQn);
```

### 5.2 中断配置
```c
// 使能传输完成中断
DMA_ITConfig(DMA1_Channel1, DMA_IT_TC, ENABLE);

// 配置NVIC
NVIC_InitTypeDef NVIC_InitStructure;
NVIC_InitStructure.NVIC_IRQChannel = DMA1_Channel1_IRQn;
NVIC_InitStructure.NVIC_IRQChannelPreemptionPriority = 0;
NVIC_InitStructure.NVIC_IRQChannelSubPriority = 0;
NVIC_InitStructure.NVIC_IRQChannelCmd = ENABLE;
NVIC_Init(&NVIC_InitStructure);
```

### 5.3 中断处理函数
```c
void DMA1_Channel1_IRQHandler(void)
{
    if(DMA_GetITStatus(DMA1_IT_TC1))
    {
        // 传输完成处理
        DMA_ClearITPendingBit(DMA1_IT_TC1);
    }
}
```

## 六、常见问题与调试

### 6.1 地址对齐问题
- **问题**：DMA传输失败或数据错误
- **解决**：
  1. 确保外设和内存地址正确对齐
  2. 检查地址是否为NULL或非法地址
  3. 验证地址递增配置与实际需求一致

### 6.2 缓冲区大小计算
- **问题**：数据传输不完整或溢出
- **解决**：
  1. 确认缓冲区大小单位与数据宽度一致
  2. 对于字节传输：BufferSize = 数据个数
  3. 对于半字传输：BufferSize = 数据个数
  4. 对于字传输：BufferSize = 数据个数

### 6.3 传输完成标志清除
- **问题**：中断重复触发或标志不清除
- **解决**：
  1. 中断处理中必须清除相应标志
  2. 查询方式使用后也要清除标志
  3. 注意不同通道的标志位不同

### 6.4 中断优先级配置
- **问题**：DMA中断不触发或触发不及时
- **解决**：
  1. 合理配置NVIC优先级
  2. 避免中断嵌套过深
  3. 确保中断使能正确

### 6.5 循环模式配置
- **问题**：循环模式不工作或数据覆盖
- **解决**：
  1. 确认DMA_Mode设置为DMA_Mode_Circular
  2. 缓冲区大小要足够容纳循环数据
  3. 注意循环模式下的中断触发时机

## 七、性能优化建议

### 7.1 使用循环模式减少CPU干预
```c
DMA_InitStructure.DMA_Mode = DMA_Mode_Circular;
```
**优点**：自动重复传输，减少CPU中断处理
**适用场景**：连续数据流、实时信号处理

### 7.2 合理设置DMA优先级
```c
DMA_InitStructure.DMA_Priority = DMA_Priority_VeryHigh;
```
**建议**：
- 实时性要求高的通道设为最高优先级
- 多个DMA通道时合理分配优先级
- 避免优先级冲突导致阻塞

### 7.3 使用双缓冲区技术
```c
// 双缓冲区交替使用
uint8_t Buffer1[BUFFER_SIZE];
uint8_t Buffer2[BUFFER_SIZE];
uint8_t *CurrentBuffer = Buffer1;
uint8_t *NextBuffer = Buffer2;

// DMA完成中断中切换缓冲区
void DMA_IRQHandler(void)
{
    if(DMA_GetITStatus(DMA_IT_TC))
    {
        // 处理CurrentBuffer数据
        ProcessData(CurrentBuffer);

        // 切换缓冲区
        uint8_t *temp = CurrentBuffer;
        CurrentBuffer = NextBuffer;
        NextBuffer = temp;

        // 重新配置DMA使用新缓冲区
        DMA_SetMemoryAddress(DMA_Channel, (uint32_t)NextBuffer);

        DMA_ClearITPendingBit(DMA_IT_TC);
    }
}
```
**优点**：避免数据覆盖，提高传输效率
**适用场景**：高速数据采集、实时处理

### 7.4 注意数据对齐以提高效率
- **32位对齐**：地址为4的倍数，性能最佳
- **16位对齐**：地址为2的倍数
- **字节对齐**：无特殊要求

### 7.5 优化缓冲区大小
- **太小**：增加中断频率，CPU负担重
- **太大**：增加延迟，占用内存多
- **建议**：根据数据速率和实时性要求折中

## 八、API函数参考

### 8.1 初始化函数
- `void DMA_Init(DMA_Channel_TypeDef* DMAy_Channelx, DMA_InitTypeDef* DMA_InitStruct)`
- `void DMA_StructInit(DMA_InitTypeDef* DMA_InitStruct)`
- `void DMA_DeInit(DMA_Channel_TypeDef* DMAy_Channelx)`

### 8.2 控制函数
- `void DMA_Cmd(DMA_Channel_TypeDef* DMAy_Channelx, FunctionalState NewState)`
- `void DMA_SetCurrDataCounter(DMA_Channel_TypeDef* DMAy_Channelx, uint16_t DataNumber)`
- `uint16_t DMA_GetCurrDataCounter(DMA_Channel_TypeDef* DMAy_Channelx)`

### 8.3 中断函数
- `void DMA_ITConfig(DMA_Channel_TypeDef* DMAy_Channelx, uint32_t DMA_IT, FunctionalState NewState)`
- `FlagStatus DMA_GetFlagStatus(uint32_t DMA_FLAG)`
- `void DMA_ClearFlag(uint32_t DMA_FLAG)`
- `ITStatus DMA_GetITStatus(uint32_t DMA_IT)`
- `void DMA_ClearITPendingBit(uint32_t DMA_IT)`

### 8.4 地址设置函数
- `void DMA_SetPeripheralAddress(DMA_Channel_TypeDef* DMAy_Channelx, uint32_t Address)`
- `void DMA_SetMemoryAddress(DMA_Channel_TypeDef* DMAy_Channelx, uint32_t Address)`

## 总结

DMA是CH32V307中非常重要的外设，可以显著提高系统性能。关键配置要点包括传输方向、数据宽度、地址递增、工作模式和优先级设置。不同外设的DMA配置有所差异，需要根据具体应用场景进行调整。

建议开发者在设计DMA传输时重点关注以下几点：
1. 正确配置通道映射和优先级
2. 合理设置缓冲区大小和工作模式
3. 注意地址对齐和数据宽度匹配
4. 完善错误处理和中断管理
5. 根据实际需求优化性能参数
# CH32V307 DAC外设使用指南

本文档基于CH32V307 EVT示例代码分析，总结DAC外设的使用方法和配置要点。

## 一、DAC外设概述

CH32V307 DAC模块特性：
- 12位数字到模拟转换器
- 双DAC通道（DAC1: PA4, DAC2: PA5）
- 支持多种触发模式
- 内置噪声和三角波形生成器
- 支持DMA数据传输
- 可配置输出缓冲器

## 二、DAC基本配置

### 2.1 时钟使能
```c
RCC_APB1PeriphClockCmd(RCC_APB1Periph_DAC, ENABLE);
RCC_APB2PeriphClockCmd(RCC_APB2Periph_GPIOA, ENABLE);
```

### 2.2 GPIO配置
```c
GPIO_InitStructure.GPIO_Pin = GPIO_Pin_4 | GPIO_Pin_5;
GPIO_InitStructure.GPIO_Mode = GPIO_Mode_AIN;  // 模拟输入模式
GPIO_Init(GPIOA, &GPIO_InitStructure);
```

### 2.3 DAC初始化结构体
```c
DAC_InitTypeDef DAC_InitType = {
    .DAC_Trigger = DAC_Trigger_None,          // 触发模式
    .DAC_WaveGeneration = DAC_WaveGeneration_None, // 波形生成
    .DAC_LFSRUnmask_TriangleAmplitude = DAC_LFSRUnmask_Bit0, // LFSR掩码/三角波幅度
    .DAC_OutputBuffer = DAC_OutputBuffer_Disable, // 输出缓冲
};
```

## 三、触发模式详解

### 3.1 无触发模式（直接输出）
```c
DAC_InitType.DAC_Trigger = DAC_Trigger_None;
DAC_Init(DAC_Channel_1, &DAC_InitType);
DAC_SetChannel1Data(DAC_Align_12b_R, 2048);  // 直接设置输出值
```

### 3.2 软件触发
```c
DAC_InitType.DAC_Trigger = DAC_Trigger_Software;
DAC_Init(DAC_Channel_1, &DAC_InitType);
DAC_SoftwareTriggerCmd(DAC_Channel_1, ENABLE);
```

### 3.3 定时器触发
```c
DAC_InitType.DAC_Trigger = DAC_Trigger_T8_TRGO;  // TIM8触发
DAC_Init(DAC_Channel_1, &DAC_InitType);
// 配置TIM8输出触发
TIM_SelectOutputTrigger(TIM8, TIM_TRGOSource_Update);
```

### 3.4 外部中断触发
```c
DAC_InitType.DAC_Trigger = DAC_Trigger_Ext_IT9;  // EXTI Line9触发
DAC_Init(DAC_Channel_1, &DAC_InitType);
// 配置EXTI Line9
EXTI_InitStructure.EXTI_Line = EXTI_Line9;
EXTI_InitStructure.EXTI_Trigger = EXTI_Trigger_Rising;
EXTI_Init(&EXTI_InitStructure);
```

## 四、波形生成功能

### 4.1 无波形生成（默认）
```c
DAC_InitType.DAC_WaveGeneration = DAC_WaveGeneration_None;
```

### 4.2 噪声波形生成
```c
DAC_InitType.DAC_WaveGeneration = DAC_WaveGeneration_Noise;
DAC_InitType.DAC_LFSRUnmask_TriangleAmplitude = DAC_LFSRUnmask_Bits11_0;  // 12位LFSR
DAC_Init(DAC_Channel_1, &DAC_InitType);
```

### 4.3 三角波形生成
```c
DAC_InitType.DAC_WaveGeneration = DAC_WaveGeneration_Triangle;
DAC_InitType.DAC_LFSRUnmask_TriangleAmplitude = DAC_TriangleAmplitude_4095;  // 最大幅度
DAC_Init(DAC_Channel_1, &DAC_InitType);
DAC->CTLR |= 0x04;  // 设置TEN=1（三角波使能）
```

## 五、输出缓冲配置

### 5.1 禁用输出缓冲（推荐）
```c
DAC_InitType.DAC_OutputBuffer = DAC_OutputBuffer_Disable;
```
**优点**：输出阻抗高，驱动能力弱，但响应速度快
**缺点**：驱动能力有限

### 5.2 使能输出缓冲
```c
DAC_InitType.DAC_OutputBuffer = DAC_OutputBuffer_Enable;
```
**优点**：驱动能力强，输出阻抗低
**缺点**：响应速度慢，功耗较高

## 六、数据对齐方式

### 6.1 12位右对齐
```c
DAC_SetChannel1Data(DAC_Align_12b_R, 0x0FFF);  // 最大值4095
```

### 6.2 12位左对齐
```c
DAC_SetChannel1Data(DAC_Align_12b_L, 0xFFF0);  // 高12位有效
```

### 6.3 8位右对齐
```c
DAC_SetChannel1Data(DAC_Align_8b_R, 0xFF);     // 最大值255
```

## 七、DMA支持

### 7.1 DAC DMA通道映射
- DAC1 → DMA2通道3
- DAC2 → DMA2通道4

### 7.2 DMA初始化配置
```c
// DAC DMA示例
DMA_InitStructure.DMA_PeripheralBaseAddr = (u32)&(DAC->R12BDHR1);
DMA_InitStructure.DMA_MemoryBaseAddr = (u32)&dacbuff16bit;
DMA_InitStructure.DMA_DIR = DMA_DIR_PeripheralDST;  // 内存到外设
DMA_InitStructure.DMA_BufferSize = 8;
DMA_InitStructure.DMA_PeripheralInc = DMA_PeripheralInc_Disable;
DMA_InitStructure.DMA_MemoryInc = DMA_MemoryInc_Enable;
DMA_InitStructure.DMA_PeripheralDataSize = DMA_PeripheralDataSize_HalfWord;
DMA_InitStructure.DMA_MemoryDataSize = DMA_MemoryDataSize_HalfWord;
DMA_InitStructure.DMA_Mode = DMA_Mode_Circular;  // 循环模式
DMA_Init(DMA2_Channel3, &DMA_InitStructure);

DAC_DMACmd(DAC_Channel_1, ENABLE);
```

### 7.3 双通道DMA数据传输
```c
// 使用RD12BDHR寄存器同时设置双通道
uint32_t dual_data = (DAC2_data << 16) | DAC1_data;
DAC->RD12BDHR = dual_data;

// 或使用API函数
DAC_SetDualChannelData(DAC_Align_12b_R, DAC1_data, DAC2_data);
```

## 八、示例工程详解

### 8.1 DAC_DMA示例
**用途**：演示使用DMA进行DAC数据转换，通过TIM8_TRGO事件触发DAC转换

**关键配置**：
```c
DAC_InitType.DAC_Trigger = DAC_Trigger_T8_TRGO;
DAC_InitType.DAC_WaveGeneration = DAC_WaveGeneration_None;
DAC_InitType.DAC_OutputBuffer = DAC_OutputBuffer_Disable;
DAC_Init(DAC_Channel_1, &DAC_InitType);
DAC_DMACmd(DAC_Channel_1, ENABLE);
```

**应用场景**：连续波形输出，音频信号生成

### 8.2 DAC_Exit_9_Trig示例
**用途**：演示通过EXTI_9 (PB9)外部中断触发DAC转换

**关键配置**：
```c
DAC_InitType.DAC_Trigger = DAC_Trigger_Ext_IT9;
DAC_InitType.DAC_WaveGeneration = DAC_WaveGeneration_None;
DAC_InitType.DAC_OutputBuffer = DAC_OutputBuffer_Disable;
DAC_Init(DAC_Channel_1, &DAC_InitType);
```

**应用场景**：事件触发输出，响应外部信号

### 8.3 DAC_Noise_Generation示例
**用途**：演示DAC噪声波形生成功能

**关键配置**：
```c
DAC_InitType.DAC_Trigger = DAC_Trigger_Software;
DAC_InitType.DAC_WaveGeneration = DAC_WaveGeneration_Noise;
DAC_InitType.DAC_LFSRUnmask_TriangleAmplitude = DAC_LFSRUnmask_Bits11_0;
DAC_InitType.DAC_OutputBuffer = DAC_OutputBuffer_Disable;
DAC_Init(DAC_Channel_1, &DAC_InitType);
```

**应用场景**：噪声信号生成，随机数生成

### 8.4 DAC_Normal_OUT示例
**用途**：演示DAC基本输出功能，无触发直接输出

**关键配置**：
```c
DAC_InitType.DAC_Trigger = DAC_Trigger_None;
DAC_InitType.DAC_WaveGeneration = DAC_WaveGeneration_None;
DAC_InitType.DAC_LFSRUnmask_TriangleAmplitude = DAC_LFSRUnmask_Bit0;
DAC_InitType.DAC_OutputBuffer = DAC_OutputBuffer_Disable;
DAC_Init(DAC_Channel_1, &DAC_InitType);
```

**应用场景**：静态电压输出，参考电压生成

### 8.5 DAC_Timer_Trig示例
**用途**：演示通过TIM8定时器触发DAC转换

**关键配置**：
```c
DAC_InitType.DAC_Trigger = DAC_Trigger_T8_TRGO;
DAC_InitType.DAC_WaveGeneration = DAC_WaveGeneration_None;
DAC_InitType.DAC_OutputBuffer = DAC_OutputBuffer_Disable;
DAC_Init(DAC_Channel_1, &DAC_InitType);
```

**应用场景**：定时波形输出，周期性信号生成

### 8.6 DAC_Triangle_Generation示例
**用途**：演示DAC三角波形生成功能

**关键配置**：
```c
DAC_InitType.DAC_Trigger = DAC_Trigger_Software;
DAC_InitType.DAC_WaveGeneration = DAC_WaveGeneration_Triangle;
DAC_InitType.DAC_LFSRUnmask_TriangleAmplitude = DAC_TriangleAmplitude_4095;
DAC_InitType.DAC_OutputBuffer = DAC_OutputBuffer_Disable;
DAC_Init(DAC_Channel_1, &DAC_InitType);
DAC->CTLR |= 0x04;  /* TEN = 1 */
```

**应用场景**：三角波信号生成，扫描信号源

### 8.7 DualDAC_SineWave示例
**用途**：演示双DAC通道同时输出正弦波

**关键配置**：
```c
// 双通道初始化
DAC_InitType.DAC_Trigger = DAC_Trigger_T4_TRGO;
DAC_InitType.DAC_WaveGeneration = DAC_WaveGeneration_None;
DAC_InitType.DAC_LFSRUnmask_TriangleAmplitude = DAC_LFSRUnmask_Bit0;
DAC_InitType.DAC_OutputBuffer = DAC_OutputBuffer_Disable;

DAC_Init(DAC_Channel_1, &DAC_InitType);
DAC_Init(DAC_Channel_2, &DAC_InitType);

// DMA配置
DAC_DMACmd(DAC_Channel_1, ENABLE);
DAC_DMACmd(DAC_Channel_2, ENABLE);
```

**应用场景**：立体声音频输出，双通道信号生成

### 8.8 DualDAC_Triangle示例
**用途**：演示双DAC通道输出不同幅度和频率的三角波

**关键配置**：
```c
// 通道1：最大幅度三角波
DAC_InitType.DAC_LFSRUnmask_TriangleAmplitude = DAC_TriangleAmplitude_4095;
DAC_Init(DAC_Channel_1, &DAC_InitType);

// 通道2：一半幅度三角波
DAC_InitType.DAC_LFSRUnmask_TriangleAmplitude = DAC_TriangleAmplitude_2047;
DAC_Init(DAC_Channel_2, &DAC_InitType);
```

**应用场景**：多通道信号生成，对比测试

## 九、常见问题与调试

### 9.1 输出无电压问题排查
1. 检查时钟使能：`RCC_APB1PeriphClockCmd(RCC_APB1Periph_DAC, ENABLE)`
2. 检查GPIO模式：必须为`GPIO_Mode_AIN`
3. 检查DAC使能：`DAC_Cmd(DAC_Channel_1, ENABLE)`
4. 检查触发模式：如果是触发模式，确保触发事件发生

### 9.2 触发不工作问题
1. 确认触发源配置正确
2. 检查触发使能：`DAC_TriggerCmd(DAC_Channel_1, ENABLE)`
3. 对于外部触发，检查EXTI配置
4. 对于定时器触发，检查TIM配置和输出触发设置

### 9.3 DMA传输异常处理
1. 检查DMA通道映射是否正确
2. 确认缓冲区大小和地址对齐
3. 检查DMA模式（普通模式/循环模式）
4. 验证DMA中断配置（如果需要）

### 9.4 波形失真问题
1. 检查输出缓冲配置
2. 确认数据更新速率不超过DAC转换速率
3. 检查电源噪声和滤波
4. 验证参考电压稳定性

## 十、最佳实践

### 10.1 时钟配置建议
```c
// 确保DAC时钟稳定
RCC_APB1PeriphClockCmd(RCC_APB1Periph_DAC, ENABLE);
// GPIO时钟也必须使能
RCC_APB2PeriphClockCmd(RCC_APB2Periph_GPIOA, ENABLE);
```

### 10.2 输出缓冲选择指南
- **高精度应用**：禁用输出缓冲，减少失真
- **高驱动能力需求**：使能输出缓冲
- **低功耗应用**：禁用输出缓冲
- **高速响应需求**：禁用输出缓冲

### 10.3 触发模式选择建议
- **静态输出**：无触发模式
- **定时更新**：定时器触发
- **事件响应**：外部中断触发
- **连续输出**：DMA+定时器触发

### 10.4 功耗优化技巧
1. 不使用时禁用DAC：`DAC_Cmd(DAC_Channel_1, DISABLE)`
2. 禁用输出缓冲降低功耗
3. 降低更新频率减少功耗
4. 使用低功耗模式时注意DAC状态保持

## 十一、API函数参考

### 11.1 初始化函数
- `void DAC_Init(uint32_t DAC_Channel, DAC_InitTypeDef* DAC_InitStruct)`
- `void DAC_StructInit(DAC_InitTypeDef* DAC_InitStruct)`

### 11.2 控制函数
- `void DAC_Cmd(uint32_t DAC_Channel, FunctionalState NewState)`
- `void DAC_DMACmd(uint32_t DAC_Channel, FunctionalState NewState)`
- `void DAC_SoftwareTriggerCmd(uint32_t DAC_Channel, FunctionalState NewState)`

### 11.3 数据设置函数
- `void DAC_SetChannel1Data(uint32_t DAC_Align, uint16_t Data)`
- `void DAC_SetChannel2Data(uint32_t DAC_Align, uint16_t Data)`
- `void DAC_SetDualChannelData(uint32_t DAC_Align, uint16_t Data2, uint16_t Data1)`
- `uint16_t DAC_GetDataOutputValue(uint32_t DAC_Channel)`

### 11.4 波形生成函数
- `void DAC_WaveGenerationCmd(uint32_t DAC_Channel, uint32_t DAC_Wave, FunctionalState NewState)`
- `void DAC_SetLFSRUnmask(uint32_t DAC_Channel, uint32_t DAC_LFSRUnmask)`
- `void DAC_SetTriangleAmplitude(uint32_t DAC_Channel, uint32_t DAC_TriangleAmplitude)`

## 十二、寄存器摘要

### 12.1 关键寄存器说明
- **DAC_CTLR**：控制寄存器
  - EN1/EN2：通道使能位
  - TEN1/TEN2：三角波使能位
  - TSEL1/TSEL2：触发选择位
  - WAVE1/WAVE2：波形生成控制位
  - MAMP1/MAMP2：幅度控制位

- **DAC_SWTR**：软件触发寄存器
  - SWTRIG1/SWTRIG2：软件触发位

- **DAC_R12BDHR**：双通道数据保持寄存器
  - DACC2DHR：DAC2数据
  - DACC1DHR：DAC1数据

### 12.2 编程注意事项
1. 写数据寄存器前确保DAC已使能
2. 改变触发模式前先禁用DAC
3. DMA传输时注意数据对齐
4. 双通道操作时注意寄存器访问顺序

## 总结

CH32V307的DAC外设功能强大，支持多种工作模式和波形生成功能。通过合理配置可以满足从简单的静态电压输出到复杂的波形生成等各种应用需求。关键配置要点包括触发模式选择、波形生成配置、输出缓冲设置和DMA支持。

建议开发者根据具体应用场景选择合适的示例工程作为参考，重点关注触发模式、波形生成和DMA配置等关键参数。
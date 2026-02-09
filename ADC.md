# CH32V307 ADC外设使用指南

本文档基于CH32V307 EVT示例代码分析，总结ADC外设的使用方法和配置要点。

## 一、ADC基础配置

### 1.1 时钟配置
```c
RCC_APB2PeriphClockCmd(RCC_APB2Periph_GPIOA, ENABLE);
RCC_APB2PeriphClockCmd(RCC_APB2Periph_ADC1, ENABLE);
RCC_ADCCLKConfig(RCC_PCLK2_Div8);  // ADC时钟 = PCLK2/8
```

### 1.2 GPIO配置
```c
GPIO_InitStructure.GPIO_Pin = GPIO_Pin_1;
GPIO_InitStructure.GPIO_Mode = GPIO_Mode_AIN;  // 模拟输入模式
GPIO_Init(GPIOA, &GPIO_InitStructure);
```

### 1.3 ADC初始化流程
```c
ADC_DeInit(ADC1);  // 复位ADC

ADC_InitTypeDef ADC_InitStructure = {
    .ADC_Mode = ADC_Mode_Independent,      // 工作模式
    .ADC_ScanConvMode = DISABLE,           // 扫描模式
    .ADC_ContinuousConvMode = DISABLE,     // 连续转换模式
    .ADC_ExternalTrigConv = ADC_ExternalTrigConv_None, // 外部触发
    .ADC_DataAlign = ADC_DataAlign_Right,  // 数据对齐
    .ADC_NbrOfChannel = 1,                 // 通道数量
};
ADC_Init(ADC1, &ADC_InitStructure);

// 校准流程
ADC_BufferCmd(ADC1, DISABLE);
ADC_ResetCalibration(ADC1);
while(ADC_GetResetCalibrationStatus(ADC1));
ADC_StartCalibration(ADC1);
while(ADC_GetCalibrationStatus(ADC1));
Calibrattion_Val = Get_CalibrationValue(ADC1);

ADC_Cmd(ADC1, ENABLE);  // 使能ADC
```

## 二、单ADC工作模式示例

### 2.1 ADC_DMA - DMA采样示例
**用途**：通过DMA连续采样ADC数据
**配置要点**：
- ADC通道：ADC1通道1 (PA1)
- 模式：独立模式，连续转换
- 触发：软件触发
- DMA：使用DMA1通道1，从ADC1->RDATAR传输到内存缓冲区
- 采样次数：1024次连续采样
- 采样时间：239.5个周期

**关键代码**：
```c
ADC_DMACmd(ADC1, ENABLE);
DMA_Tx_Init(DMA1_Channel1, (u32)&ADC1->RDATAR, (u32)TxBuf, 1024);
ADC_RegularChannelConfig(ADC1, ADC_Channel_1, 1, ADC_SampleTime_239Cycles5);
ADC_SoftwareStartConvCmd(ADC1, ENABLE);
```

### 2.2 AnalogWatchdog - 模拟看门狗示例
**用途**：检测ADC转换数据是否在设定范围内
**配置要点**：
- ADC通道：ADC1通道1 (PA1)
- 看门狗阈值：高阈值3500，低阈值2000
- 中断：当ADC值超出范围时触发AWD中断

**关键代码**：
```c
ADC_AnalogWatchdogThresholdsConfig(ADC1, 3500, 2000);
ADC_AnalogWatchdogSingleChannelConfig(ADC1, ADC_Channel_1);
ADC_AnalogWatchdogCmd(ADC1, ADC_AnalogWatchdog_SingleRegEnable);
ADC_ITConfig(ADC1, ADC_IT_AWD, ENABLE);
```

### 2.3 Auto_Injection - 自动注入模式示例
**用途**：自动注入组转换
**配置要点**：
- 规则组通道：ADC1通道1 (PA1)
- 注入组通道：ADC1通道3 (PA3)
- 自动注入：使能自动注入转换

**关键代码**：
```c
ADC_RegularChannelConfig(ADC1, ADC_Channel_1, 1, ADC_SampleTime_239Cycles5);
ADC_InjectedChannelConfig(ADC1, ADC_Channel_3, 1, ADC_SampleTime_239Cycles5);
ADC_AutoInjectedConvCmd(ADC1, ENABLE);
```

### 2.4 Discontinuous_mode - 间断模式示例
**用途**：使用TIM1触发间断模式注入组转换
**配置要点**：
- 注入组通道：ADC1通道1 (PA1)、通道3 (PA3)、通道4 (PA4)
- 触发源：TIM1_CC4事件
- 间断模式：使能注入组间断模式
- 每次触发转换通道数：1个通道

**关键代码**：
```c
ADC_InjectedSequencerLengthConfig(ADC1, 3);
ADC_ExternalTrigInjectedConvConfig(ADC1, ADC_ExternalTrigInjecConv_T1_CC4);
ADC_DiscModeChannelCountConfig(ADC1, 1);
ADC_InjectedDiscModeCmd(ADC1, ENABLE);
```

### 2.5 ExtLines_Trigger - 外部线路触发ADC转换示例
**用途**：外部线路触发ADC转换
**配置要点**：
- 通道：ADC1通道2 (PA2) - 注入组
- 触发源：EXTI线15事件 (PA15高电平)
- 外部触发：ADC_ExternalTrigInjecConv_Ext_IT15_TIM8_CC4

**关键代码**：
```c
ADC_ExternalTrigInjectedConvConfig(ADC1, ADC_ExternalTrigInjecConv_Ext_IT15_TIM8_CC4);
ADC_ExternalTrigInjectedConvCmd(ADC1, ENABLE);
EXTI_InitStructure.EXTI_Line = EXTI_Line15;
EXTI_InitStructure.EXTI_Trigger = EXTI_Trigger_Rising;
```

### 2.6 TIM_Trigger - TIM触发ADC转换示例
**用途**：TIM触发ADC转换
**配置要点**：
- 通道：ADC1通道1 (PA1) - 注入组
- 触发源：TIM2_CC1事件
- 定时器：TIM2配置为PWM输出模式

**关键代码**：
```c
ADC_ExternalTrigInjectedConvConfig(ADC1, ADC_ExternalTrigInjecConv_T2_CC1);
ADC_ExternalTrigInjectedConvCmd(ADC1, ENABLE);
TIM_SelectOutputTrigger(TIM2, TIM_TRGOSource_Update);
```

## 三、双ADC工作模式示例

### 3.1 DualADC_AlternateTrigger - 双ADC交替触发示例
**用途**：双ADC交替触发采样
**配置要点**：
- ADC1通道：通道1 (PA1) - 注入组
- ADC2通道：通道3 (PA3) - 注入组
- 模式：交替触发模式 (ADC_Mode_AlterTrig)
- 触发源：TIM2_CC1触发ADC1，ADC1转换完成后触发ADC2

**关键代码**：
```c
ADC_InitStructure.ADC_Mode = ADC_Mode_AlterTrig;
ADC_ExternalTrigInjectedConvConfig(ADC1, ADC_ExternalTrigInjecConv_T2_CC1);
ADC_ExternalTrigInjectedConvConfig(ADC2, ADC_ExternalTrigInjecConv_None);
```

### 3.2 DualADC_RegSimul - 双ADC规则同时采样示例
**用途**：双ADC规则组同时采样
**配置要点**：
- 规则组：ADC1通道2 (PA2), ADC2通道3 (PA3)
- 模式：规则同时模式 (ADC_Mode_RegSimult)
- 数据获取：通过DMA中断

**关键代码**：
```c
ADC_InitStructure.ADC_Mode = ADC_Mode_RegSimult;
ADC_RegularChannelConfig(ADC1, ADC_Channel_2, 1, ADC_SampleTime_239Cycles5);
ADC_RegularChannelConfig(ADC2, ADC_Channel_3, 1, ADC_SampleTime_239Cycles5);
ADC_DMACmd(ADC1, ENABLE);
```

### 3.3 DualADC_InjectionSimul - 双ADC注入同时采样示例
**用途**：双ADC注入组同时采样
**配置要点**：
- ADC1通道：通道1 (PA1) - 注入组
- ADC2通道：通道3 (PA3) - 注入组
- 模式：注入同时模式 (ADC_Mode_InjecSimult)
- 触发：软件触发

**关键代码**：
```c
ADC_InitStructure.ADC_Mode = ADC_Mode_InjecSimult;
ADC_InjectedChannelConfig(ADC1, ADC_Channel_1, 1, ADC_SampleTime_239Cycles5);
ADC_InjectedChannelConfig(ADC2, ADC_Channel_3, 1, ADC_SampleTime_239Cycles5);
```

### 3.4 DualADC_Combined_RegInjectionSimul - 双ADC规则+注入+同时采样示例
**用途**：双ADC规则组和注入组同时采样
**配置要点**：
- 规则组：ADC1通道2 (PA2), ADC2通道4 (PA4)
- 注入组：ADC1通道3 (PA3), ADC2通道5 (PA5)
- 模式：规则+注入同时模式 (ADC_Mode_RegInjecSimult)
- 数据获取：规则组通过DMA中断，注入组通过ADC中断

**关键代码**：
```c
ADC_InitStructure.ADC_Mode = ADC_Mode_RegInjecSimult;
ADC_DMACmd(ADC1, ENABLE);
ADC_ITConfig(ADC1, ADC_IT_JEOC, ENABLE);
ADC_ITConfig(ADC2, ADC_IT_JEOC, ENABLE);
```

### 3.5 DualADC_FastInterleaved - 双ADC快速交替采样示例
**用途**：双ADC快速交替采样
**配置要点**：
- 通道：ADC1和ADC2都使用通道1 (PA1)
- 模式：快速交替模式 (ADC_Mode_FastInterl)
- 采样时间：1.5个周期（快速采样）
- 数据获取：通过ADC中断

**关键代码**：
```c
ADC_InitStructure.ADC_Mode = ADC_Mode_FastInterl;
ADC_RegularChannelConfig(ADC1, ADC_Channel_1, 1, ADC_SampleTime_1Cycles5);
ADC_RegularChannelConfig(ADC2, ADC_Channel_1, 1, ADC_SampleTime_1Cycles5);
ADC_ITConfig(ADC1, ADC_IT_EOC, ENABLE);
```

### 3.6 DualADC_SlowInterleaved - 双ADC慢速交替采样示例
**用途**：双ADC慢速交替采样
**配置要点**：
- 通道：ADC1和ADC2都使用通道2 (PA2)
- 模式：慢速交替模式 (ADC_Mode_SlowInterl)
- 采样时间：13.5个周期
- 数据获取：通过ADC中断

**关键代码**：
```c
ADC_InitStructure.ADC_Mode = ADC_Mode_SlowInterl;
ADC_RegularChannelConfig(ADC1, ADC_Channel_2, 1, ADC_SampleTime_13Cycles5);
ADC_RegularChannelConfig(ADC2, ADC_Channel_2, 1, ADC_SampleTime_13Cycles5);
ADC_ITConfig(ADC1, ADC_IT_EOC, ENABLE);
```

## 四、特殊功能示例

### 4.1 Internal_Temperature - 内部温度传感器示例
**用途**：内部温度传感器采集
**配置要点**：
- 通道：ADC通道16 (内部温度传感器)
- 温度传感器：使能温度传感器和内部参考电压
- 缓冲区：使能ADC缓冲区
- 电压转换：将ADC值转换为电压和温度值

**关键代码**：
```c
ADC_TempSensorVrefintCmd(ENABLE);
ADC_BufferCmd(ADC1, ENABLE);   //enable buffer
ADC_val = Get_ADC_Average(ADC_Channel_TempSensor, 10);
val_mv = (ADC_val*3300/4096);
TempSensor_Volt_To_Temper(val_mv);
```

### 4.2 Temperature_External_channel - 温度传感器和外部通道交替采样示例
**用途**：温度传感器和外部通道交替采样
**配置要点**：
- 温度传感器：ADC通道16
- 外部通道：ADC通道3 (PA3)
- 交替采样：先采样温度传感器，再采样外部通道
- 温度传感器控制：采样时使能，采样后禁用

**关键代码**：
```c
ADC_TempSensorVrefintCmd(ENABLE);
adc_val = Get_ConversionVal(Get_ADC_Val(ADC_Channel_TempSensor));
ADC_TempSensorVrefintCmd(DISABLE);
adc_val = Get_ConversionVal(Get_ADC_Val(ADC_Channel_3));
```

## 五、数据获取方式

### 5.1 查询方式
```c
while(ADC_GetFlagStatus(ADC1, ADC_FLAG_EOC) == RESET);
ADC_GetConversionValue(ADC1);
```

### 5.2 中断方式
```c
ADC_ITConfig(ADC1, ADC_IT_EOC, ENABLE);
NVIC_EnableIRQ(ADC1_2_IRQn);
```

### 5.3 DMA方式
```c
ADC_DMACmd(ADC1, ENABLE);
DMA_InitStructure.DMA_PeripheralBaseAddr = (u32)&ADC1->RDATAR;
DMA_InitStructure.DMA_MemoryBaseAddr = (u32)Buffer;
DMA_InitStructure.DMA_DIR = DMA_DIR_PeripheralSRC;
DMA_Init(DMA1_Channel1, &DMA_InitStructure);
DMA_Cmd(DMA1_Channel1, ENABLE);
```

## 六、校准与补偿

### 6.1 校准流程
```c
ADC_ResetCalibration(ADCx);
while(ADC_GetResetCalibrationStatus(ADCx));
ADC_StartCalibration(ADCx);
while(ADC_GetCalibrationStatus(ADCx));
Calibrattion_Val = Get_CalibrationValue(ADCx);
```

### 6.2 数据补偿函数
```c
u16 Get_ConversionVal(s16 val) {
    if((val+Calibrattion_Val)<0|| val==0) return 0;
    if((Calibrattion_Val+val)>4095||val==4095) return 4095;
    return (val+Calibrattion_Val);
}
```

## 七、常见问题与解决方案

1. **校准值获取异常**：检查ADC时钟和电源稳定性
2. **触发不响应**：确认触发源配置和使能状态
3. **DMA传输不完整**：检查DMA缓冲区大小和地址对齐
4. **中断不触发**：确认中断使能和优先级配置
5. **双ADC数据同步问题**：检查主从ADC配置和触发延迟

## 八、示例工程索引

| 示例名称 | 文件夹路径 | 主要功能 |
|---------|-----------|---------|
| ADC_DMA | ADC/ADC_DMA | DMA连续采样 |
| AnalogWatchdog | ADC/AnalogWatchdog | 模拟看门狗 |
| Auto_Injection | ADC/Auto_Injection | 自动注入模式 |
| Discontinuous_mode | ADC/Discontinuous_mode | 间断模式 |
| DualADC_AlternateTrigger | ADC/DualADC_AlternateTrigger | 双ADC交替触发 |
| DualADC_Combined_RegInjectionSimul | ADC/DualADC_Combined_RegInjectionSimul | 规则+注入同时模式 |
| DualADC_FastInterleaved | ADC/DualADC_FastInterleaved | 快速交替采样 |
| DualADC_InjectionSimul | ADC/DualADC_InjectionSimul | 注入同时采样 |
| DualADC_RegSimul | ADC/DualADC_RegSimul | 规则同时采样 |
| DualADC_SlowInterleaved | ADC/DualADC_SlowInterleaved | 慢速交替采样 |
| ExtLines_Trigger | ADC/ExtLines_Trigger | 外部线路触发 |
| Internal_Temperature | ADC/Internal_Temperature | 内部温度传感器 |
| Temperature_External_channel | ADC/Temperature_External_channel | 温度传感器与外部通道 |
| TIM_Trigger | ADC/TIM_Trigger | TIM触发 |

## 总结

CH32V307的ADC外设功能丰富，支持多种工作模式和触发方式。通过合理配置可以满足不同应用场景的需求，从简单的单次转换到复杂的双ADC同步采样。关键配置要点包括时钟设置、通道选择、触发模式、数据对齐和校准补偿。

建议开发者根据具体应用需求选择合适的示例工程作为起点，逐步修改配置参数以适应实际应用场景。
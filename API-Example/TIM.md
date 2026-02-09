# CH32V307 TIM（定时器）外设使用指南

## 1. 外设概述

定时器（TIM）是CH32V307的重要外设之一，用于定时、计数、PWM生成、输入捕获、输出比较等功能。CH32V307包含多种类型的定时器：

- **高级定时器**：TIM1、TIM8，支持互补输出、死区时间、刹车功能
- **通用定时器**：TIM2、TIM3、TIM4、TIM5，支持输入捕获、输出比较、PWM生成
- **基本定时器**：TIM6、TIM7，仅支持基本定时功能

定时器主要特性：
- 16位向上/向下自动重装载计数器
- 16位可编程预分频器（1-65536）
- 多种工作模式：定时、计数、PWM、输入捕获、输出比较
- 编码器接口模式
- 外部触发同步
- DMA请求生成
- 中断/DMA事件：更新、触发、输入捕获、输出比较

## 2. 关键代码片段

### 2.1 定时器基本配置（TIM_INT示例）

```c
// 定时器时基初始化
TIM_TimeBaseInitTypeDef TIM_TimeBaseInitStructure = {0};

TIM_TimeBaseInitStructure.TIM_Period = arr;           // 自动重装载值
TIM_TimeBaseInitStructure.TIM_Prescaler = psc;        // 预分频值
TIM_TimeBaseInitStructure.TIM_ClockDivision = TIM_CKD_DIV1;  // 时钟分频
TIM_TimeBaseInitStructure.TIM_CounterMode = TIM_CounterMode_Up;  // 向上计数模式
TIM_TimeBaseInitStructure.TIM_RepetitionCounter = 50; // 重复计数器（仅高级定时器）

TIM_TimeBaseInit(TIM1, &TIM_TimeBaseInitStructure);
```

### 2.2 定时器中断配置

```c
// 中断配置
NVIC_InitTypeDef NVIC_InitStructure = {0};

NVIC_InitStructure.NVIC_IRQChannel = TIM1_UP_IRQn;    // 中断通道
NVIC_InitStructure.NVIC_IRQChannelPreemptionPriority = 0;  // 抢占优先级
NVIC_InitStructure.NVIC_IRQChannelSubPriority = 1;    // 子优先级
NVIC_InitStructure.NVIC_IRQChannelCmd = ENABLE;       // 使能中断
NVIC_Init(&NVIC_InitStructure);

// 使能定时器更新中断
TIM_ITConfig(TIM1, TIM_IT_Update, ENABLE);
TIM_Cmd(TIM1, ENABLE);  // 使能定时器

// 中断服务函数
void TIM1_UP_IRQHandler(void) __attribute__((interrupt("WCH-Interrupt-fast")));
void TIM1_UP_IRQHandler(void)
{
    if(TIM_GetITStatus(TIM1, TIM_IT_Update)==SET)
    {
        printf("--------updata\r\n");
    }
    TIM_ClearITPendingBit( TIM1, TIM_IT_Update );
}
```

### 2.3 PWM输出配置（PWM_Output示例）

```c
// GPIO配置为复用推挽输出
GPIO_InitStructure.GPIO_Pin = GPIO_Pin_8;
GPIO_InitStructure.GPIO_Mode = GPIO_Mode_AF_PP;
GPIO_InitStructure.GPIO_Speed = GPIO_Speed_50MHz;
GPIO_Init(GPIOA, &GPIO_InitStructure);

// PWM模式配置
TIM_OCInitTypeDef TIM_OCInitStructure = {0};

TIM_OCInitStructure.TIM_OCMode = TIM_OCMode_PWM1;  // PWM模式1
TIM_OCInitStructure.TIM_OutputState = TIM_OutputState_Enable;
TIM_OCInitStructure.TIM_Pulse = ccp;               // 占空比设置
TIM_OCInitStructure.TIM_OCPolarity = TIM_OCPolarity_High;
TIM_OC1Init(TIM1, &TIM_OCInitStructure);

// 使能预装载寄存器
TIM_OC1PreloadConfig(TIM1, TIM_OCPreload_Enable);
TIM_ARRPreloadConfig(TIM1, ENABLE);
```

### 2.4 输入捕获配置（Input_Capture示例）

```c
// 输入捕获配置
TIM_ICInitTypeDef TIM_ICInitStructure = {0};

TIM_ICInitStructure.TIM_Channel = TIM_Channel_1;
TIM_ICInitStructure.TIM_ICPrescaler = TIM_ICPSC_DIV1;  // 输入捕获预分频
TIM_ICInitStructure.TIM_ICFilter = 0x00;               // 输入滤波器
TIM_ICInitStructure.TIM_ICPolarity = TIM_ICPolarity_Rising;  // 上升沿触发
TIM_ICInitStructure.TIM_ICSelection = TIM_ICSelection_DirectTI;  // 直接输入
TIM_ICInit(TIM1, &TIM_ICInitStructure);

// PWM输入捕获配置（自动测量周期和占空比）
TIM_PWMIConfig(TIM1, &TIM_ICInitStructure);

// 从机模式配置
TIM_SelectInputTrigger(TIM1, TIM_TS_TI1FP1);
TIM_SelectSlaveMode(TIM1, TIM_SlaveMode_Reset);
TIM_SelectMasterSlaveMode(TIM1, TIM_MasterSlaveMode_Enable);
```

### 2.5 DMA与定时器结合（TIM_DMA示例）

```c
// DMA配置
DMA_InitTypeDef DMA_InitStructure = {0};

DMA_InitStructure.DMA_PeripheralBaseAddr = ppadr;      // 外设地址
DMA_InitStructure.DMA_MemoryBaseAddr = memadr;         // 内存地址
DMA_InitStructure.DMA_DIR = DMA_DIR_PeripheralDST;     // 方向：内存到外设
DMA_InitStructure.DMA_BufferSize = bufsize;            // 缓冲区大小
DMA_InitStructure.DMA_PeripheralInc = DMA_PeripheralInc_Disable;  // 外设地址不递增
DMA_InitStructure.DMA_MemoryInc = DMA_MemoryInc_Enable;           // 内存地址递增
DMA_InitStructure.DMA_PeripheralDataSize = DMA_PeripheralDataSize_HalfWord;  // 半字
DMA_InitStructure.DMA_MemoryDataSize = DMA_MemoryDataSize_HalfWord;
DMA_InitStructure.DMA_Mode = DMA_Mode_Circular;        // 循环模式
DMA_Init(DMA1_Channel3, &DMA_InitStructure);

// 使能定时器DMA请求
TIM_DMACmd(TIM1, TIM_DMA_Update, ENABLE);
```

### 2.6 编码器接口配置（Encoder示例）

```c
// 编码器接口配置
TIM_EncoderInterfaceConfig(TIM3, TIM_EncoderMode_TI12,
                           TIM_ICPolarity_Rising, TIM_ICPolarity_Rising);

// 编码器模式：
// TIM_EncoderMode_TI1  - 仅在TI1计数
// TIM_EncoderMode_TI2  - 仅在TI2计数
// TIM_EncoderMode_TI12 - 在TI1和TI2都计数
```

### 2.7 互补输出与死区时间（ComplementaryOutput_DeadTime示例）

```c
// 互补输出配置
TIM_OCInitStructure.TIM_OutputNState = TIM_OutputNState_Enable;  // 使能互补输出
TIM_OCInitStructure.TIM_OCNPolarity = TIM_OCNPolarity_Low;       // 互补输出极性
TIM_OCInitStructure.TIM_OCIdleState = TIM_OCIdleState_Set;       // 空闲状态
TIM_OCInitStructure.TIM_OCNIdleState = TIM_OCNIdleState_Reset;   // 互补空闲状态

// 死区时间配置
TIM_BDTRInitTypeDef TIM_BDTRInitStructure = {0};

TIM_BDTRInitStructure.TIM_DeadTime = 0xFF;  // 死区时间值（0x00-0xFF）
TIM_BDTRInitStructure.TIM_Break = TIM_Break_Disable;  // 刹车功能
TIM_BDTRInitStructure.TIM_AutomaticOutput = TIM_AutomaticOutput_Enable;  // 自动输出
TIM_BDTRInit(TIM1, &TIM_BDTRInitStructure);
```

## 3. 配置数据结构

### 3.1 TIM_TimeBaseInitTypeDef（时基初始化结构体）
```c
typedef struct
{
  uint16_t TIM_Prescaler;         // 预分频值 (0x0000-0xFFFF)
  uint16_t TIM_CounterMode;       // 计数模式
  uint16_t TIM_Period;            // 自动重装载值 (0x0000-0xFFFF)
  uint16_t TIM_ClockDivision;     // 时钟分频
  uint8_t TIM_RepetitionCounter;  // 重复计数器（仅高级定时器）
} TIM_TimeBaseInitTypeDef;
```

### 3.2 TIM_OCInitTypeDef（输出比较初始化结构体）
```c
typedef struct
{
  uint16_t TIM_OCMode;        // 输出比较模式
  uint16_t TIM_OutputState;   // 输出状态
  uint16_t TIM_OutputNState;  // 互补输出状态（仅高级定时器）
  uint16_t TIM_Pulse;         // 脉冲值（占空比）
  uint16_t TIM_OCPolarity;    // 输出极性
  uint16_t TIM_OCNPolarity;   // 互补输出极性（仅高级定时器）
  uint16_t TIM_OCIdleState;   // 空闲状态（仅高级定时器）
  uint16_t TIM_OCNIdleState;  // 互补空闲状态（仅高级定时器）
} TIM_OCInitTypeDef;
```

### 3.3 TIM_ICInitTypeDef（输入捕获初始化结构体）
```c
typedef struct
{
  uint16_t TIM_Channel;      // 通道选择
  uint16_t TIM_ICPolarity;   // 输入极性
  uint16_t TIM_ICSelection;  // 输入选择
  uint16_t TIM_ICPrescaler;  // 输入预分频
  uint16_t TIM_ICFilter;     // 输入滤波器 (0x0-0xF)
} TIM_ICInitTypeDef;
```

### 3.4 TIM_BDTRInitTypeDef（刹车和死区时间结构体）
```c
typedef struct
{
  uint16_t TIM_OSSRState;        // 运行模式关闭状态选择
  uint16_t TIM_OSSIState;        // 空闲模式关闭状态选择
  uint16_t TIM_LOCKLevel;        // 锁定级别
  uint16_t TIM_DeadTime;         // 死区时间 (0x00-0xFF)
  uint16_t TIM_Break;            // 刹车使能
  uint16_t TIM_BreakPolarity;    // 刹车极性
  uint16_t TIM_AutomaticOutput;  // 自动输出使能
} TIM_BDTRInitTypeDef;
```

## 4. 通用配置步骤

### 4.1 基本配置流程
1. **使能时钟**
   ```c
   RCC_APB2PeriphClockCmd(RCC_APB2Periph_TIM1, ENABLE);  // 高级定时器
   RCC_APB1PeriphClockCmd(RCC_APB1Periph_TIM2, ENABLE);  // 通用定时器
   ```

2. **GPIO配置**（如果使用外部引脚）
   ```c
   GPIO_InitStructure.GPIO_Pin = GPIO_Pin_8;
   GPIO_InitStructure.GPIO_Mode = GPIO_Mode_AF_PP;      // 复用推挽输出
   GPIO_InitStructure.GPIO_Speed = GPIO_Speed_50MHz;
   ```

3. **时基初始化**
   ```c
   TIM_TimeBaseInitStructure.TIM_Period = arr;          // 自动重装载值
   TIM_TimeBaseInitStructure.TIM_Prescaler = psc;       // 预分频值
   TIM_TimeBaseInitStructure.TIM_ClockDivision = TIM_CKD_DIV1;
   TIM_TimeBaseInitStructure.TIM_CounterMode = TIM_CounterMode_Up;
   TIM_TimeBaseInit(TIM1, &TIM_TimeBaseInitStructure);
   ```

4. **功能模式配置**（PWM、输入捕获、输出比较等）
5. **中断/DMA配置**（如果需要）
6. **使能定时器**
   ```c
   TIM_Cmd(TIM1, ENABLE);
   ```

### 4.2 定时计算
- **定时周期** = (TIM_Prescaler + 1) × (TIM_Period + 1) / TIMx_CLK
- **PWM频率** = TIMx_CLK / [(TIM_Prescaler + 1) × (TIM_Period + 1)]
- **PWM占空比** = TIM_Pulse / (TIM_Period + 1)

### 4.3 输出比较模式
CH32V307支持四种输出比较模式：
1. **定时模式**（TIM_OCMode_Timing）：不影响输出引脚电平
2. **有效模式**（TIM_OCMode_Active）：匹配时输出有效电平
3. **无效模式**（TIM_OCMode_Inactive）：匹配时输出无效电平
4. **翻转模式**（TIM_OCMode_Toggle）：匹配时输出电平翻转

### 4.4 PWM模式
- **PWM模式1**：向上计数时，CNT<CCR时通道为有效电平
- **PWM模式2**：向上计数时，CNT>CCR时通道为有效电平

## 5. 示例工程详解

### 5.1 TIM_INT（定时器中断）
- **位置**：`TIM/TIM_INT/User/main.c`
- **功能**：基本定时中断，定时更新事件触发中断
- **适用场景**：定时任务、系统心跳、周期性事件

### 5.2 PWM_Output（PWM输出）
- **位置**：`TIM/PWM_Output/User/main.c`
- **功能**：PWM信号生成，控制输出占空比
- **适用场景**：电机控制、LED调光、功率控制

### 5.3 Input_Capture（输入捕获）
- **位置**：`TIM/Input_Capture/User/main.c`
- **功能**：测量外部信号频率和脉宽
- **适用场景**：频率测量、脉冲宽度测量、编码器信号处理

### 5.4 TIM_DMA（DMA传输）
- **位置**：`TIM/TIM_DMA/User/main.c`
- **功能**：DMA与定时器结合，高效传输数据
- **适用场景**：批量数据更新、波形生成、高速数据传输

### 5.5 Output_Compare（输出比较）
- **位置**：`TIM/Output_Compare_Mode/User/main.c`
- **功能**：精确时间控制，输出比较模式
- **适用场景**：精确时间控制、脉冲生成、事件触发

### 5.6 Encoder（编码器接口）
- **位置**：`TIM/Encoder/User/main.c`
- **功能**：编码器接口，测量旋转角度和速度
- **适用场景**：电机位置检测、旋转测量、位置反馈

### 5.7 ComplementaryOutput（互补输出）
- **位置**：`TIM/ComplementaryOutput_DeadTime/User/main.c`
- **功能**：互补输出与死区时间控制
- **适用场景**：电机驱动、H桥控制、功率逆变器

### 5.8 One_Pulse（单脉冲输出）
- **位置**：`TIM/One_Pulse/User/main.c`
- **功能**：单脉冲模式，外部触发生成单脉冲
- **适用场景**：触发脉冲、延时控制、精确脉冲生成

### 5.9 Clock_Select（时钟源选择）
- **位置**：`TIM/Clock_Select/User/main.c`
- **功能**：外部时钟源选择
- **适用场景**：外部时钟同步、频率测量

### 5.10 Synchro_Timer（定时器同步）
- **位置**：`TIM/Synchro_Timer/User/main.c`
- **功能**：定时器同步工作
- **适用场景**：复杂定时任务、多定时器协调

## 6. 常见问题

### Q1: 定时器时钟源如何选择？
A: CH32V307定时器时钟源：
- 高级定时器（TIM1、TIM8）挂载在APB2总线
- 通用定时器（TIM2-TIM5）挂载在APB1总线
- 基本定时器（TIM6、TIM7）挂载在APB1总线

### Q2: PWM输出频率和占空比如何计算？
A:
- PWM频率 = TIMx_CLK / [(TIM_Prescaler + 1) × (TIM_Period + 1)]
- 占空比 = TIM_Pulse / (TIM_Period + 1)

### Q3: 输入捕获测量不准确怎么办？
A: 检查以下配置：
1. 输入捕获预分频设置
2. 输入滤波器配置
3. 触发边沿设置（上升沿/下降沿）
4. 时钟分频设置

### Q4: 互补输出和死区时间如何配置？
A: 仅高级定时器（TIM1、TIM8）支持互补输出和死区时间：
1. 配置TIM_OutputNState使能互补输出
2. 配置TIM_BDTRInitStructure设置死区时间
3. 死区时间值范围0x00-0xFF，根据开关器件特性选择

### Q5: 编码器接口如何工作？
A: 编码器接口支持三种模式：
1. TIM_EncoderMode_TI1：仅在TI1计数
2. TIM_EncoderMode_TI2：仅在TI2计数
3. TIM_EncoderMode_TI12：在TI1和TI2都计数

## 7. API参考

| 函数名 | 功能描述 | 参数说明 |
|--------|----------|----------|
| `TIM_TimeBaseInit` | 定时器时基初始化 | TIMx：定时器，TIM_TimeBaseInitStruct：时基结构体 |
| `TIM_OCxInit` | 输出比较初始化 | TIMx：定时器，TIM_OCInitStruct：输出比较结构体 |
| `TIM_ICInit` | 输入捕获初始化 | TIMx：定时器，TIM_ICInitStruct：输入捕获结构体 |
| `TIM_EncoderInterfaceConfig` | 编码器接口配置 | TIMx：定时器，TIM_EncoderMode：编码器模式，ICPolarity1/2：极性 |
| `TIM_BDTRInit` | 刹车和死区时间配置 | TIMx：定时器，TIM_BDTRInitStruct：刹车结构体 |
| `TIM_Cmd` | 使能/禁用定时器 | TIMx：定时器，NewState：新状态 |
| `TIM_ITConfig` | 使能/禁用定时器中断 | TIMx：定时器，TIM_IT：中断源，NewState：新状态 |
| `TIM_DMACmd` | 使能/禁用定时器DMA | TIMx：定时器，TIM_DMASource：DMA源，NewState：新状态 |
| `TIM_GetITStatus` | 获取中断状态 | TIMx：定时器，TIM_IT：中断源 |
| `TIM_ClearITPendingBit` | 清除中断标志 | TIMx：定时器，TIM_IT：中断源 |
| `TIM_GetCounter` | 获取计数器值 | TIMx：定时器 |
| `TIM_SetCounter` | 设置计数器值 | TIMx：定时器，Counter：计数器值 |
| `TIM_SetComparex` | 设置比较值 | TIMx：定时器，Comparex：比较值 |

## 8. 总结

CH32V307的定时器外设功能强大，支持多种工作模式和应用场景。通过合理配置定时器参数，可以实现精确的定时、PWM生成、输入捕获等功能。在使用定时器时，需要注意时钟源选择、中断配置、DMA使用等关键步骤，确保定时器功能正常工作。
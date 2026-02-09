# CH32V307 EXTI外部中断使用指南

本文档基于CH32V307 EVT示例代码分析，总结EXTI外部中断的使用方法和配置要点。

## 一、概述

EXTI（External Interrupt/Event Controller）外部中断/事件控制器用于处理GPIO引脚的外部中断和事件。

### 主要特性：
- 支持20个外部中断/事件线
- 每个中断线可独立配置
- 支持上升沿、下降沿或双边沿触发
- 支持中断模式和事件模式
- 部分中断线连接到特定外设（PVD、RTC、USB等）

## 二、EXTI线映射

### 2.1 GPIO引脚映射
| EXTI线 | GPIO引脚源 | 中断通道 |
|--------|------------|----------|
| EXTI_Line0 | PA0-PG0 | EXTI0_IRQn |
| EXTI_Line1 | PA1-PG1 | EXTI1_IRQn |
| EXTI_Line2 | PA2-PG2 | EXTI2_IRQn |
| EXTI_Line3 | PA3-PG3 | EXTI3_IRQn |
| EXTI_Line4 | PA4-PG4 | EXTI4_IRQn |
| EXTI_Line5-9 | PA5-PG5至PA9-PG9 | EXTI9_5_IRQn（共享） |
| EXTI_Line10-15 | PA10-PG10至PA15-PG15 | EXTI15_10_IRQn（共享） |

### 2.2 特殊功能线
| EXTI线 | 连接外设 | 中断通道 |
|--------|----------|----------|
| EXTI_Line16 | PVD输出 | PVD_IRQn |
| EXTI_Line17 | RTC闹钟 | RTC_Alarm_IRQn |
| EXTI_Line18 | USB唤醒 | USBHS_WKUP_IRQn |
| EXTI_Line19 | 以太网唤醒 | ETH_IRQn |
| EXTI_Line20 | 保留 | - |

## 三、基本配置步骤

### 3.1 完整配置流程
```c
void EXTI_Configuration(void)
{
    GPIO_InitTypeDef GPIO_InitStructure = {0};
    EXTI_InitTypeDef EXTI_InitStructure = {0};
    NVIC_InitTypeDef NVIC_InitStructure = {0};

    // 1. 使能时钟
    RCC_APB2PeriphClockCmd(RCC_APB2Periph_AFIO | RCC_APB2Periph_GPIOA, ENABLE);

    // 2. 配置GPIO
    GPIO_InitStructure.GPIO_Pin = GPIO_Pin_0;      // PA0
    GPIO_InitStructure.GPIO_Mode = GPIO_Mode_IPU;  // 上拉输入
    GPIO_InitStructure.GPIO_Speed = GPIO_Speed_50MHz;
    GPIO_Init(GPIOA, &GPIO_InitStructure);

    // 3. 配置GPIO到EXTI映射
    GPIO_EXTILineConfig(GPIO_PortSourceGPIOA, GPIO_PinSource0);

    // 4. 配置EXTI线
    EXTI_InitStructure.EXTI_Line = EXTI_Line0;
    EXTI_InitStructure.EXTI_Mode = EXTI_Mode_Interrupt;  // 中断模式
    EXTI_InitStructure.EXTI_Trigger = EXTI_Trigger_Falling;  // 下降沿触发
    EXTI_InitStructure.EXTI_LineCmd = ENABLE;
    EXTI_Init(&EXTI_InitStructure);

    // 5. 配置NVIC中断
    NVIC_InitStructure.NVIC_IRQChannel = EXTI0_IRQn;
    NVIC_InitStructure.NVIC_IRQChannelPreemptionPriority = 1;
    NVIC_InitStructure.NVIC_IRQChannelSubPriority = 2;
    NVIC_InitStructure.NVIC_IRQChannelCmd = ENABLE;
    NVIC_Init(&NVIC_InitStructure);
}
```

### 3.2 中断模式 vs 事件模式

#### 中断模式（EXTI_Mode_Interrupt）
```c
EXTI_InitStructure.EXTI_Mode = EXTI_Mode_Interrupt;
```
- 产生CPU中断
- 需要中断服务程序
- 用于需要CPU处理的外部事件

#### 事件模式（EXTI_Mode_Event）
```c
EXTI_InitStructure.EXTI_Mode = EXTI_Mode_Event;
```
- 仅触发外设，不产生CPU中断
- 不需要中断服务程序
- 用于触发ADC/DAC等外设操作

## 四、触发边沿配置

### 4.1 触发方式
```c
// 上升沿触发
EXTI_InitStructure.EXTI_Trigger = EXTI_Trigger_Rising;

// 下降沿触发
EXTI_InitStructure.EXTI_Trigger = EXTI_Trigger_Falling;

// 双边沿触发
EXTI_InitStructure.EXTI_Trigger = EXTI_Trigger_Rising_Falling;
```

### 4.2 应用场景
- **按键检测**：通常使用下降沿触发（按键按下）
- **外部触发ADC**：使用事件模式和上升沿触发
- **电源监测**：使用双边沿触发监测电压变化

## 五、中断服务程序

### 5.1 基本中断处理
```c
// EXTI0中断服务程序
void EXTI0_IRQHandler(void)
{
    if(EXTI_GetITStatus(EXTI_Line0) != RESET)
    {
        // 处理中断
        printf("EXTI0 Triggered\n");

        // 清除中断标志
        EXTI_ClearITPendingBit(EXTI_Line0);
    }
}
```

### 5.2 共享中断处理
```c
// EXTI9_5中断服务程序（处理EXTI5-EXTI9）
void EXTI9_5_IRQHandler(void)
{
    // 检查并处理EXTI5
    if(EXTI_GetITStatus(EXTI_Line5) != RESET)
    {
        printf("EXTI5 Triggered\n");
        EXTI_ClearITPendingBit(EXTI_Line5);
    }

    // 检查并处理EXTI6
    if(EXTI_GetITStatus(EXTI_Line6) != RESET)
    {
        printf("EXTI6 Triggered\n");
        EXTI_ClearITPendingBit(EXTI_Line6);
    }

    // 类似处理EXTI7、EXTI8、EXTI9...
}

// EXTI15_10中断服务程序（处理EXTI10-EXTI15）
void EXTI15_10_IRQHandler(void)
{
    // 类似处理EXTI10-EXTI15
}
```

## 六、示例工程详解

### 6.1 EXTI0基础示例
**用途**：基本外部中断演示，PA0下降沿触发中断

**关键配置**：
```c
// GPIO配置：PA0上拉输入
GPIO_InitStructure.GPIO_Mode = GPIO_Mode_IPU;

// EXTI配置：EXTI_Line0，中断模式，下降沿触发
EXTI_InitStructure.EXTI_Line = EXTI_Line0;
EXTI_InitStructure.EXTI_Mode = EXTI_Mode_Interrupt;
EXTI_InitStructure.EXTI_Trigger = EXTI_Trigger_Falling;

// NVIC配置：抢占优先级1，子优先级2
NVIC_InitStructure.NVIC_IRQChannel = EXTI0_IRQn;
NVIC_InitStructure.NVIC_IRQChannelPreemptionPriority = 1;
NVIC_InitStructure.NVIC_IRQChannelSubPriority = 2;
```

**应用场景**：按键中断、外部信号检测

### 6.2 ADC外部触发示例
**用途**：使用EXTI事件触发ADC转换

**关键配置**：
```c
// GPIO配置：PA15下拉输入
GPIO_InitStructure.GPIO_Mode = GPIO_Mode_IPD;

// EXTI配置：EXTI_Line15，事件模式，上升沿触发
EXTI_InitStructure.EXTI_Line = EXTI_Line15;
EXTI_InitStructure.EXTI_Mode = EXTI_Mode_Event;
EXTI_InitStructure.EXTI_Trigger = EXTI_Trigger_Rising;

// ADC配置：外部线路触发
ADC_ExternalTrigInjectedConvConfig(ADC1, ADC_ExternalTrigInjecConv_Ext_IT15_TIM8_CC4);
ADC_ExternalTrigInjectedConvCmd(ADC1, ENABLE);
```

**应用场景**：同步ADC采样、外部触发数据采集

### 6.3 DAC外部触发示例
**用途**：使用EXTI事件触发DAC转换

**关键配置**：
```c
// GPIO配置：PB9下拉输入
GPIO_InitStructure.GPIO_Mode = GPIO_Mode_IPD;

// EXTI配置：EXTI_Line9，事件模式，上升沿触发
EXTI_InitStructure.EXTI_Line = EXTI_Line9;
EXTI_InitStructure.EXTI_Mode = EXTI_Mode_Event;
EXTI_InitStructure.EXTI_Trigger = EXTI_Trigger_Rising;

// DAC配置：外部中断触发
DAC_InitType.DAC_Trigger = DAC_Trigger_Ext_IT9;
```

**应用场景**：外部触发波形输出、同步DAC更新

### 6.4 PVD电压监测示例
**用途**：电源电压检测器中断

**关键配置**：
```c
// EXTI配置：EXTI_Line16，中断模式，下降沿触发
EXTI_InitStructure.EXTI_Line = EXTI_Line16;
EXTI_InitStructure.EXTI_Mode = EXTI_Mode_Interrupt;
EXTI_InitStructure.EXTI_Trigger = EXTI_Trigger_Falling;

// PVD配置：电压检测级别
PWR_PVDLevelConfig(PWR_PVDLevel_MODE0);  // 约2.7V
PWR_PVDCmd(ENABLE);
```

**应用场景**：电池电压监测、掉电保护

### 6.5 低功耗唤醒示例
**用途**：从停止/睡眠模式唤醒

**停止模式唤醒配置**：
```c
// 配置EXTI0中断
EXTI_Configuration();

// 进入停止模式
PWR_EnterSTOPMode(PWR_Regulator_LowPower, PWR_STOPEntry_WFI);

// 唤醒后系统时钟需要重新配置
SystemInit();
```

**睡眠模式唤醒配置**：
```c
// 配置EXTI0中断
EXTI_Configuration();

// 进入睡眠模式
__WFI();  // 等待中断
```

**应用场景**：低功耗应用、电池供电设备

## 七、特殊功能配置

### 7.1 双边沿触发
```c
// PVD电压判断示例
EXTI_InitStructure.EXTI_Trigger = EXTI_Trigger_Rising_Falling;
```
**应用**：监测电压上升和下降，用于精确的电压阈值检测

### 7.2 共享中断优先级
```c
// EXTI9_5共享中断配置
NVIC_InitStructure.NVIC_IRQChannel = EXTI9_5_IRQn;
NVIC_InitStructure.NVIC_IRQChannelPreemptionPriority = 1;
NVIC_InitStructure.NVIC_IRQChannelSubPriority = 0;
```
**注意**：共享中断中需要检查具体的中断线

### 7.3 操作系统中的EXTI
```c
// HarmonyOS中的EXTI配置
NVIC_InitStructure.NVIC_IRQChannelPreemptionPriority = 0;
NVIC_InitStructure.NVIC_IRQChannelSubPriority = 4;
```
**注意**：操作系统中需要合理设置中断优先级

## 八、GPIO输入模式选择

### 8.1 输入模式对比
| 模式 | 常量 | 描述 | 适用场景 |
|------|------|------|----------|
| 浮空输入 | `GPIO_Mode_IN_FLOATING` | 无上下拉电阻 | 外部已带上/下拉 |
| 上拉输入 | `GPIO_Mode_IPU` | 内部上拉电阻 | 按键检测（按下接地） |
| 下拉输入 | `GPIO_Mode_IPD` | 内部下拉电阻 | 按键检测（按下接VCC） |
| 模拟输入 | `GPIO_Mode_AIN` | 模拟输入 | ADC采样 |

### 8.2 推荐配置
- **按键检测**：`GPIO_Mode_IPU`（按键按下接地）或 `GPIO_Mode_IPD`（按键按下接VCC）
- **外部触发**：根据外部信号类型选择，通常使用 `GPIO_Mode_IPU` 或 `GPIO_Mode_IPD`
- **模拟信号**：`GPIO_Mode_AIN`（用于ADC触发）

## 九、中断优先级配置

### 9.1 NVIC优先级分组
```c
// 常用优先级分组2
NVIC_PriorityGroupConfig(NVIC_PriorityGroup_2);
```
分组2：2位抢占优先级，2位子优先级

### 9.2 优先级设置原则
1. **实时性要求高的中断**：设置高抢占优先级
2. **相关中断**：相同抢占优先级，不同子优先级
3. **共享中断**：合理设置优先级，避免阻塞其他中断

### 9.3 示例配置
```c
// 高优先级中断（如通信）
NVIC_InitStructure.NVIC_IRQChannelPreemptionPriority = 0;  // 最高抢占优先级
NVIC_InitStructure.NVIC_IRQChannelSubPriority = 0;

// 中等优先级中断（如定时器）
NVIC_InitStructure.NVIC_IRQChannelPreemptionPriority = 1;
NVIC_InitStructure.NVIC_IRQChannelSubPriority = 0;

// 低优先级中断（如按键）
NVIC_InitStructure.NVIC_IRQChannelPreemptionPriority = 2;
NVIC_InitStructure.NVIC_IRQChannelSubPriority = 0;
```

## 十、调试技巧

### 10.1 中断不触发排查
1. **检查时钟使能**
   ```c
   RCC_APB2PeriphClockCmd(RCC_APB2Periph_AFIO, ENABLE);  // AFIO时钟必须使能
   RCC_APB2PeriphClockCmd(RCC_APB2Periph_GPIOx, ENABLE); // GPIO时钟必须使能
   ```

2. **检查GPIO模式**
   - 必须配置为输入模式
   - 根据外部电路选择上拉/下拉

3. **检查EXTI配置**
   - EXTI线是否正确映射
   - 触发边沿是否正确
   - EXTI线是否使能

4. **检查NVIC配置**
   - 中断通道是否正确
   - 中断是否使能
   - 优先级设置是否合理

5. **检查中断标志**
   ```c
   // 在中断服务程序中检查并清除标志
   if(EXTI_GetITStatus(EXTI_Line0) != RESET)
   {
       EXTI_ClearITPendingBit(EXTI_Line0);
   }
   ```

### 10.2 中断响应延迟优化
1. **使用快速中断**
   ```c
   void EXTI0_IRQHandler(void) __attribute__((interrupt("WCH-Interrupt-fast")));
   ```

2. **精简中断服务程序**
   - 只做必要的处理
   - 复杂处理放到主循环
   - 使用标志位通信

3. **合理设置优先级**
   - 避免中断嵌套过深
   - 高优先级中断处理时间要短

### 10.3 抗干扰设计
1. **硬件滤波**
   - 增加RC滤波电路
   - 使用施密特触发器

2. **软件消抖**
   ```c
   void EXTI0_IRQHandler(void)
   {
       if(EXTI_GetITStatus(EXTI_Line0) != RESET)
       {
           Delay_Ms(10);  // 10ms消抖
           if(GPIO_ReadInputDataBit(GPIOA, GPIO_Pin_0) == 0)  // 确认状态
           {
               // 有效触发
           }
           EXTI_ClearITPendingBit(EXTI_Line0);
       }
   }
   ```

## 十一、API函数参考

### 11.1 EXTI初始化函数
- `void EXTI_Init(EXTI_InitTypeDef* EXTI_InitStruct)` - 初始化EXTI
- `void EXTI_StructInit(EXTI_InitTypeDef* EXTI_InitStruct)` - 结构体默认初始化
- `void EXTI_GenerateSWInterrupt(uint32_t EXTI_Line)` - 产生软件中断

### 11.2 GPIO映射函数
- `void GPIO_EXTILineConfig(uint8_t GPIO_PortSource, uint8_t GPIO_PinSource)` - GPIO到EXTI映射

### 11.3 中断状态函数
- `ITStatus EXTI_GetITStatus(uint32_t EXTI_Line)` - 获取中断状态
- `void EXTI_ClearITPendingBit(uint32_t EXTI_Line)` - 清除中断挂起位
- `FlagStatus EXTI_GetFlagStatus(uint32_t EXTI_Line)` - 获取标志位状态
- `void EXTI_ClearFlag(uint32_t EXTI_Line)` - 清除标志位

### 11.4 NVIC函数
- `void NVIC_Init(NVIC_InitTypeDef* NVIC_InitStruct)` - 初始化NVIC
- `void NVIC_SetPriority(IRQn_Type IRQn, uint32_t priority)` - 设置优先级
- `uint32_t NVIC_GetPriority(IRQn_Type IRQn)` - 获取优先级

## 十二、最佳实践

### 12.1 配置顺序
1. 使能时钟（AFIO和GPIO）
2. 配置GPIO输入模式
3. 配置GPIO到EXTI映射
4. 配置EXTI参数
5. 配置NVIC中断
6. 编写中断服务程序

### 12.2 中断服务程序规范
```c
void EXTI0_IRQHandler(void)
{
    // 1. 检查中断标志
    if(EXTI_GetITStatus(EXTI_Line0) != RESET)
    {
        // 2. 清除中断标志（尽早清除）
        EXTI_ClearITPendingBit(EXTI_Line0);

        // 3. 中断处理（尽量简短）
        g_exti0_flag = 1;  // 设置标志位

        // 4. 复杂处理放到主循环
        // ...
    }
}
```

### 12.3 低功耗应用注意事项
1. **唤醒源配置**：在进入低功耗模式前配置好EXTI
2. **时钟恢复**：从停止模式唤醒后需要重新配置系统时钟
3. **中断标志**：唤醒后检查中断标志，确认唤醒原因

### 12.4 多EXTI线管理
```c
// 使用结构体管理多个EXTI线
typedef struct {
    uint32_t exti_line;
    IRQn_Type irq_channel;
    void (*handler)(void);
} EXTI_Config;

EXTI_Config exti_configs[] = {
    {EXTI_Line0, EXTI0_IRQn, EXTI0_Handler},
    {EXTI_Line1, EXTI1_IRQn, EXTI1_Handler},
    // ...
};

// 统一初始化函数
void EXTI_Config_All(void)
{
    for(int i = 0; i < sizeof(exti_configs)/sizeof(exti_configs[0]); i++)
    {
        EXTI_InitTypeDef EXTI_InitStructure = {0};
        EXTI_InitStructure.EXTI_Line = exti_configs[i].exti_line;
        EXTI_InitStructure.EXTI_Mode = EXTI_Mode_Interrupt;
        EXTI_InitStructure.EXTI_Trigger = EXTI_Trigger_Falling;
        EXTI_InitStructure.EXTI_LineCmd = ENABLE;
        EXTI_Init(&EXTI_InitStructure);
    }
}
```

## 总结

EXTI是CH32V307中重要的外部事件处理机制，支持中断和事件两种模式。关键配置要点包括GPIO模式选择、EXTI线映射、触发边沿配置和NVIC优先级设置。正确使用EXTI可以实现高效的外部事件响应，特别是在低功耗应用中作为唤醒源。

建议开发者根据具体应用场景选择合适的配置模式，并注意中断服务程序的优化和抗干扰设计。
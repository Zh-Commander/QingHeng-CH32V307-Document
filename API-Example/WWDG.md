# CH32V307 WWDG窗口看门狗使用指南

## 1. 外设概述

WWDG（Window Watchdog，窗口看门狗）是CH32V307中基于APB1总线时钟的看门狗定时器，用于监测软件执行异常。主要特性包括：

- **窗口机制**：必须在特定时间窗口内喂狗，既不能太早也不能太晚
- **早期唤醒中断**：计数器达到0x40时产生中断，提供预警机会
- **可配置窗口值**：灵活设置喂狗时间窗口
- **APB1时钟源**：使用PCLK1作为时钟源
- **硬件复位**：未在窗口内喂狗时自动产生系统复位
- **中断支持**：支持中断处理，可在复位前进行紧急处理
- **调试模式**：调试模式下可暂停计数

## 2. 关键代码片段

### 2.1 WWDG基本配置示例

```c
#include "ch32v30x.h"
#include "debug.h"

// WWDG初始化配置
void WWDG_Config(uint8_t tr, uint8_t wr, uint32_t prv)
{
    // 使能WWDG时钟
    RCC_APB1PeriphClockCmd(RCC_APB1Periph_WWDG, ENABLE);

    // 设置计数器初始值（必须大于0x40）
    WWDG_SetCounter(tr);

    // 设置预分频器
    WWDG_SetPrescaler(prv);

    // 设置窗口值（必须在0x40和0x7F之间）
    WWDG_SetWindowValue(wr);

    // 使能WWDG
    WWDG_Enable(WWDG_CNT);

    // 清除早期唤醒中断标志
    WWDG_ClearFlag();

    // 配置NVIC中断
    WWDG_NVIC_Config();

    // 使能早期唤醒中断
    WWDG_EnableIT();
}

// NVIC中断配置
void WWDG_NVIC_Config(void)
{
    NVIC_InitTypeDef NVIC_InitStructure;

    // 配置WWDG中断优先级
    NVIC_InitStructure.NVIC_IRQChannel = WWDG_IRQn;
    NVIC_InitStructure.NVIC_IRQChannelPreemptionPriority = 0;
    NVIC_InitStructure.NVIC_IRQChannelSubPriority = 0;
    NVIC_InitStructure.NVIC_IRQChannelCmd = ENABLE;
    NVIC_Init(&NVIC_InitStructure);
}
```

### 2.2 WWDG喂狗操作示例

```c
// 喂狗函数
void WWDG_Feed(void)
{
    // 重新加载计数器值为0x7F
    WWDG_SetCounter(WWDG_CNT);
}

// 主程序中的喂狗逻辑
int main(void)
{
    uint8_t wwdg_tr, wwdg_wr;

    // 系统初始化
    SystemCoreClockUpdate();
    Delay_Init();
    USART_Printf_Init(115200);
    printf("SystemClk:%d\r\n", SystemCoreClock);

    // 配置WWDG
    // 参数说明：
    // 0x7F - 计数器初值（127）
    // 0x5F - 窗口值（95）
    // WWDG_Prescaler_8 - 8分频
    WWDG_Config(0x7f, 0x5f, WWDG_Prescaler_8);

    // 获取窗口值
    wwdg_wr = WWDG->CFGR & 0x7F;

    while(1)
    {
        Delay_Ms(10);

        // 读取当前计数器值
        wwdg_tr = WWDG->CTLR & 0x7F;

        // 检查是否在窗口内
        if(wwdg_tr < wwdg_wr)
        {
            // 在窗口内喂狗
            WWDG_Feed();
            printf("Feed dog at counter: 0x%02x\r\n", wwdg_tr);
        }
        else
        {
            // 不在窗口内，不喂狗
            printf("Counter: 0x%02x, Window: 0x%02x\r\n", wwdg_tr, wwdg_wr);
        }
    }
}
```

### 2.3 WWDG中断处理示例

```c
// WWDG早期唤醒中断处理函数
void WWDG_IRQHandler(void) __attribute__((interrupt("WCH-Interrupt-fast")));
void WWDG_IRQHandler(void)
{
    // 检查中断标志
    if(WWDG_GetFlagStatus() != RESET)
    {
        // 清除中断标志
        WWDG_ClearFlag();

        // 紧急处理
        printf("WWDG Early Wakeup Interrupt! Counter: 0x%02x\r\n",
               WWDG_GetCounter() & 0x7F);

        // 可以在此处保存关键数据或进行紧急恢复
        Emergency_Recovery();
    }
}
```

### 2.4 WWDG时间计算示例

```c
// WWDG超时时间计算函数
void Calculate_WWDG_Timeout(void)
{
    uint32_t pclk1_freq;
    float wwdg_clk, time_per_count, window_time, timeout_time;

    // 获取PCLK1频率（假设48MHz）
    pclk1_freq = 48000000;

    // 计算WWDG时钟频率
    // 预分频器：8分频
    // 固定分频：4096分频
    wwdg_clk = pclk1_freq / 8.0 / 4096.0;

    // 计算每个计数周期的时间
    time_per_count = 1.0 / wwdg_clk * 1000.0;  // 毫秒

    // 计算窗口时间（0x40~0x5F）
    window_time = (0x5F - 0x40) * time_per_count;

    // 计算超时时间（0x7F~0x3F）
    timeout_time = (0x7F - 0x3F) * time_per_count;

    printf("WWDG Configuration:\r\n");
    printf("  PCLK1 Frequency: %d Hz\r\n", pclk1_freq);
    printf("  WWDG Clock: %.2f Hz\r\n", wwdg_clk);
    printf("  Time per count: %.3f ms\r\n", time_per_count);
    printf("  Window time: %.2f ms (0x40~0x5F)\r\n", window_time);
    printf("  Timeout time: %.2f ms (0x7F~0x3F)\r\n", timeout_time);
}
```

## 3. 配置数据结构

### 3.1 WWDG寄存器定义

```c
// WWDG寄存器结构（ch32v30x.h）
typedef struct
{
  __IO uint32_t CTLR;  // 控制寄存器
  __IO uint32_t CFGR;  // 配置寄存器
  __IO uint32_t STATR; // 状态寄存器
} WWDG_TypeDef;

#define WWDG_BASE          (0x40002C00)  // WWDG基地址
#define WWDG               ((WWDG_TypeDef *)WWDG_BASE)

// 控制寄存器（CTLR）位定义
#define WWDG_CTLR_WDGA     ((uint8_t)0x80)  // 看门狗激活位
#define WWDG_CTLR_T        ((uint8_t)0x7F)  // 计数器值

// 配置寄存器（CFGR）位定义
#define WWDG_CFGR_W        ((uint8_t)0x7F)  // 窗口值
#define WWDG_CFGR_WDGTB    ((uint8_t)0x180) // 预分频器位
#define WWDG_CFGR_EWI      ((uint8_t)0x200) // 早期唤醒中断使能

// 状态寄存器（STATR）位定义
#define WWDG_STATR_EWIF    ((uint8_t)0x01)  // 早期唤醒中断标志
```

### 3.2 预分频器枚举

```c
// WWDG预分频器定义（ch32v30x_wwdg.h）
#define IS_WWDG_PRESCALER(PRESCALER) (((PRESCALER) == WWDG_Prescaler_1) || \
                                      ((PRESCALER) == WWDG_Prescaler_2) || \
                                      ((PRESCALER) == WWDG_Prescaler_4) || \
                                      ((PRESCALER) == WWDG_Prescaler_8))

typedef enum
{
  WWDG_Prescaler_1 = 0x00,  // 1分频
  WWDG_Prescaler_2 = 0x01,  // 2分频
  WWDG_Prescaler_4 = 0x02,  // 4分频
  WWDG_Prescaler_8 = 0x03   // 8分频
} WWDG_Prescaler_TypeDef;
```

### 3.3 中断号定义

```c
// WWDG中断号（ch32v30x.h）
#define WWDG_IRQn          ((IRQn_Type)16)  // WWDG中断号

// 中断优先级分组
typedef enum
{
  NVIC_PriorityGroup_0 = 0x700,  // 0位抢占优先级，4位子优先级
  NVIC_PriorityGroup_1 = 0x600,  // 1位抢占优先级，3位子优先级
  NVIC_PriorityGroup_2 = 0x500,  // 2位抢占优先级，2位子优先级
  NVIC_PriorityGroup_3 = 0x400,  // 3位抢占优先级，1位子优先级
  NVIC_PriorityGroup_4 = 0x300   // 4位抢占优先级，0位子优先级
} NVIC_PriorityGroup;
```

### 3.4 时间参数结构

```c
// WWDG时间参数结构
typedef struct
{
    uint32_t pclk1_freq;      // PCLK1频率（Hz）
    uint8_t  prescaler;       // 预分频器值（1,2,4,8）
    uint8_t  window_value;    // 窗口值（0x40~0x7F）
    uint8_t  counter_init;    // 计数器初值（0x40~0x7F）
    float    wwdg_clock;      // WWDG时钟频率（Hz）
    float    time_per_count;  // 每个计数周期时间（ms）
    float    window_time;     // 窗口时间（ms）
    float    timeout_time;    // 超时时间（ms）
} WWDG_TimeParams;
```

## 4. 通用配置步骤

### 4.1 WWDG基本配置步骤

1. **使能WWDG时钟**：
   ```c
   RCC_APB1PeriphClockCmd(RCC_APB1Periph_WWDG, ENABLE);
   ```

2. **配置预分频器**：
   ```c
   // 选择预分频器值（1,2,4,8）
   WWDG_SetPrescaler(WWDG_Prescaler_8);
   ```

3. **设置窗口值**：
   ```c
   // 窗口值必须在0x40和0x7F之间
   WWDG_SetWindowValue(0x5F);
   ```

4. **设置计数器初值**：
   ```c
   // 计数器初值必须大于窗口值且小于0x7F
   WWDG_SetCounter(0x7F);
   ```

5. **使能WWDG**：
   ```c
   WWDG_Enable(WWDG_CNT);
   ```

6. **配置中断（可选）**：
   ```c
   WWDG_ClearFlag();
   WWDG_EnableIT();
   NVIC_EnableIRQ(WWDG_IRQn);
   ```

### 4.2 喂狗操作步骤

```c
// 安全的喂狗函数
void Safe_WWDG_Feed(void)
{
    uint8_t current_counter, window_value;

    // 获取当前计数器值
    current_counter = WWDG_GetCounter() & 0x7F;

    // 获取窗口值
    window_value = WWDG->CFGR & 0x7F;

    // 检查是否在窗口内
    if(current_counter < window_value)
    {
        // 在窗口内，可以喂狗
        WWDG_SetCounter(WWDG_CNT);
        printf("WWDG fed at counter: 0x%02x\r\n", current_counter);
    }
    else
    {
        // 不在窗口内，不喂狗
        printf("WWDG feed skipped! Counter: 0x%02x, Window: 0x%02x\r\n",
               current_counter, window_value);
    }
}

// 周期性喂狗
void Periodic_WWDG_Feed(void)
{
    static uint32_t last_feed_time = 0;
    uint32_t current_time = GetTickCount();

    // 每20ms喂狗一次
    if(current_time - last_feed_time >= 20)
    {
        Safe_WWDG_Feed();
        last_feed_time = current_time;
    }
}
```

### 4.3 中断配置步骤

```c
// 完整的WWDG中断配置
void WWDG_Interrupt_Configuration(void)
{
    NVIC_InitTypeDef NVIC_InitStructure;

    // 1. 配置中断优先级分组
    NVIC_PriorityGroupConfig(NVIC_PriorityGroup_2);

    // 2. 配置WWDG中断
    NVIC_InitStructure.NVIC_IRQChannel = WWDG_IRQn;
    NVIC_InitStructure.NVIC_IRQChannelPreemptionPriority = 0;  // 最高抢占优先级
    NVIC_InitStructure.NVIC_IRQChannelSubPriority = 0;         // 最高子优先级
    NVIC_InitStructure.NVIC_IRQChannelCmd = ENABLE;
    NVIC_Init(&NVIC_InitStructure);

    // 3. 清除中断标志
    WWDG_ClearFlag();

    // 4. 使能早期唤醒中断
    WWDG_EnableIT();

    printf("WWDG interrupt configured\r\n");
}

// 中断处理函数
void WWDG_IRQHandler(void) __attribute__((interrupt("WCH-Interrupt-fast")));
void WWDG_IRQHandler(void)
{
    if(WWDG_GetFlagStatus() != RESET)
    {
        WWDG_ClearFlag();

        // 紧急处理代码
        Emergency_Handler();

        // 可以在此处喂狗避免复位
        // WWDG_SetCounter(WWDG_CNT);
    }
}
```

### 4.4 时间参数计算步骤

```c
// 计算WWDG时间参数
WWDG_TimeParams Calculate_WWDG_Timing(uint32_t pclk1_freq,
                                       uint8_t prescaler,
                                       uint8_t window_value,
                                       uint8_t counter_init)
{
    WWDG_TimeParams params;
    uint8_t prescaler_value;

    // 设置输入参数
    params.pclk1_freq = pclk1_freq;
    params.prescaler = prescaler;
    params.window_value = window_value;
    params.counter_init = counter_init;

    // 确定预分频器值
    switch(prescaler)
    {
        case 0: prescaler_value = 1; break;  // WWDG_Prescaler_1
        case 1: prescaler_value = 2; break;  // WWDG_Prescaler_2
        case 2: prescaler_value = 4; break;  // WWDG_Prescaler_4
        case 3: prescaler_value = 8; break;  // WWDG_Prescaler_8
        default: prescaler_value = 8; break;
    }

    // 计算WWDG时钟频率
    // 固定分频：4096
    params.wwdg_clock = (float)pclk1_freq / prescaler_value / 4096.0;

    // 计算每个计数周期的时间
    params.time_per_count = 1.0 / params.wwdg_clock * 1000.0;  // 毫秒

    // 计算窗口时间（窗口值到0x40）
    params.window_time = (window_value - 0x40) * params.time_per_count;

    // 计算超时时间（计数器初值到0x3F）
    params.timeout_time = (counter_init - 0x3F) * params.time_per_count;

    return params;
}

// 使用示例
void Display_WWDG_Timing(void)
{
    WWDG_TimeParams timing;

    // 计算时间参数
    timing = Calculate_WWDG_Timing(48000000, 3, 0x5F, 0x7F);

    printf("WWDG Timing Parameters:\r\n");
    printf("  PCLK1 Frequency: %d Hz\r\n", timing.pclk1_freq);
    printf("  Prescaler: %d\r\n", 1 << timing.prescaler);
    printf("  Window Value: 0x%02x\r\n", timing.window_value);
    printf("  Counter Init: 0x%02x\r\n", timing.counter_init);
    printf("  WWDG Clock: %.2f Hz\r\n", timing.wwdg_clock);
    printf("  Time per count: %.3f ms\r\n", timing.time_per_count);
    printf("  Window time: %.2f ms\r\n", timing.window_time);
    printf("  Timeout time: %.2f ms\r\n", timing.timeout_time);
}
```

## 5. 示例工程详解

### 5.1 WWDG示例工程

- **位置**：`WWDG/WWDG/User/main.c`
- **功能**：演示WWDG窗口看门狗的基本使用，包括初始化、喂狗操作和中断处理
- **特点**：
  - 使用PCLK1（48MHz）作为时钟源
  - 配置8分频预分频器
  - 设置窗口值为0x5F（95）
  - 计数器初值为0x7F（127）
  - 包含早期唤醒中断处理
  - 串口输出调试信息

### 5.2 工程结构说明

```
WWDG/
└── WWDG/
    ├── .cproject                    # Eclipse项目配置
    ├── .project                     # Eclipse项目文件
    ├── .template                    # 模板文件
    ├── WWDG.wvproj                  # WCH IDE项目文件
    └── User/
        ├── ch32v30x_conf.h          # 外设配置文件
        ├── ch32v30x_it.c            # 中断服务程序
        ├── ch32v30x_it.h            # 中断头文件
        ├── main.c                   # 主程序文件
        ├── system_ch32v30x.c        # 系统初始化
        └── system_ch32v30x.h        # 系统头文件
```

### 5.3 关键功能点

1. **窗口时间计算**：
   ```c
   // 当PCLK1=48MHz，8分频时：
   // WWDG时钟 = 48MHz / 8 / 4096 ≈ 1.465kHz
   // 每个计数周期 ≈ 0.682ms
   // 窗口时间（0x40~0x5F）≈ (95-64)×0.682 ≈ 21.8ms
   // 超时时间（0x7F~0x3F）≈ (127-63)×0.682 ≈ 43.7ms
   ```

2. **喂狗逻辑**：
   ```c
   // 必须在计数器值小于窗口值时喂狗
   if(wwdg_tr < wwdg_wr)
   {
       WWDG_Feed();  // 喂狗
   }
   ```

3. **中断处理**：
   ```c
   // 早期唤醒中断在计数器达到0x40时触发
   void WWDG_IRQHandler(void)
   {
       WWDG_ClearFlag();  // 清除中断标志
       // 进行紧急处理
   }
   ```

4. **调试输出**：
   ```c
   // 输出计数器值和窗口值
   printf("Counter: 0x%02x, Window: 0x%02x\r\n", wwdg_tr, wwdg_wr);
   ```

### 5.4 硬件连接要求

WWDG是内部定时器，无需外部引脚连接。但需要注意：

- **时钟源**：使用APB1总线时钟（PCLK1）
- **时钟配置**：确保系统时钟正确配置
- **中断响应**：考虑中断处理函数的执行时间
- **调试模式**：调试模式下WWDG可能暂停，影响测试

## 6. 常见问题

### Q1: WWDG不产生复位？
A: 检查以下配置：
1. 是否使能了WWDG（WDGA位）
2. 计数器初值是否大于窗口值
3. 是否在窗口内喂狗
4. 时钟配置是否正确
5. 调试模式下WWDG可能暂停

### Q2: 喂狗时机不正确？
A: 可能原因：
1. 窗口值设置不合理
2. 喂狗函数调用时机错误
3. 系统时钟频率变化
4. 中断响应延迟

解决方案：
- 仔细计算窗口时间
- 使用定时器精确控制喂狗间隔
- 监控计数器值确定最佳喂狗时机

### Q3: 早期唤醒中断不触发？
A: 检查以下配置：
1. 是否使能了早期唤醒中断（EWI位）
2. 是否配置了NVIC中断
3. 中断优先级设置是否正确
4. 是否清除了中断标志
5. 计数器是否达到0x40

### Q4: WWDG与IWDG有什么区别？
A: 主要区别：

| 特性 | WWDG | IWDG |
|------|------|------|
| **时钟源** | PCLK1（APB1总线时钟） | LSI（40kHz内部低速时钟） |
| **复位条件** | 未在窗口内喂狗 | 超时未喂狗 |
| **窗口机制** | 有（必须在特定时间窗口内喂狗） | 无（任何时间喂狗都可以） |
| **中断支持** | 有（早期唤醒中断） | 无 |
| **可靠性** | 受主时钟影响 | 完全独立，可靠性更高 |
| **功耗** | 依赖主时钟 | 低功耗模式下仍可工作 |
| **典型应用** | 需要精确时序控制的应用 | 需要高可靠性的安全应用 |

### Q5: 如何选择合适的窗口值和计数器初值？
A: 选择原则：
1. **窗口值**：0x40~0x7F之间，应大于0x40
2. **计数器初值**：必须大于窗口值且小于0x7F
3. **窗口时间**：根据应用需求确定，典型值10-50ms
4. **超时时间**：应大于正常喂狗间隔，小于最大允许无响应时间

示例配置：
```c
// 窗口时间约20ms，超时时间约40ms
WWDG_SetWindowValue(0x5F);   // 窗口值95
WWDG_SetCounter(0x7F);       // 计数器初值127
```

## 7. API参考

### 7.1 WWDG寄存器操作API

| 函数名 | 功能描述 | 参数说明 | 返回值 |
|--------|----------|----------|--------|
| `WWDG_SetCounter` | 设置计数器值 | `Counter`：计数器值（0x40~0x7F） | 无 |
| `WWDG_GetCounter` | 获取计数器值 | 无 | 计数器值 |
| `WWDG_SetPrescaler` | 设置预分频器 | `WWDG_Prescaler`：预分频器值 | 无 |
| `WWDG_SetWindowValue` | 设置窗口值 | `WindowValue`：窗口值（0x40~0x7F） | 无 |
| `WWDG_Enable` | 使能WWDG | `Counter`：计数器值 | 无 |

### 7.2 中断控制API

| 函数名 | 功能描述 | 参数说明 | 返回值 |
|--------|----------|----------|--------|
| `WWDG_EnableIT` | 使能早期唤醒中断 | 无 | 无 |
| `WWDG_DisableIT` | 禁用早期唤醒中断 | 无 | 无 |
| `WWDG_GetFlagStatus` | 获取中断标志状态 | 无 | `FlagStatus` |
| `WWDG_ClearFlag` | 清除中断标志 | 无 | 无 |

### 7.3 系统控制API

| 函数名 | 功能描述 | 参数说明 |
|--------|----------|----------|
| `RCC_APB1PeriphClockCmd` | APB1外设时钟控制 | `RCC_APB1Periph`：外设，`NewState`：状态 |
| `NVIC_EnableIRQ` | 使能中断 | `IRQn`：中断号 |
| `NVIC_DisableIRQ` | 禁用中断 | `IRQn`：中断号 |
| `NVIC_SetPriority` | 设置中断优先级 | `IRQn`：中断号，`priority`：优先级 |

### 7.4 常用配置常量

```c
// 计数器常量
#define WWDG_CNT           0x7F  // 计数器重载值

// 预分频器常量
#define WWDG_Prescaler_1   0x00  // 1分频
#define WWDG_Prescaler_2   0x01  // 2分频
#define WWDG_Prescaler_4   0x02  // 4分频
#define WWDG_Prescaler_8   0x03  // 8分频

// 窗口值范围
#define WWDG_WINDOW_MIN    0x40  // 最小窗口值
#define WWDG_WINDOW_MAX    0x7F  // 最大窗口值

// 计数器范围
#define WWDG_COUNTER_MIN   0x40  // 最小计数器值
#define WWDG_COUNTER_MAX   0x7F  // 最大计数器值
```

## 8. 注意事项

1. **窗口机制**：必须在计数器值小于窗口值时喂狗，既不能太早也不能太晚
2. **计数器初值**：必须设置在0x40~0x7F范围内，且大于窗口值
3. **窗口值**：必须设置在0x40~0x7F范围内
4. **中断处理**：早期唤醒中断在计数器达到0x40时触发
5. **复位条件**：如果计数器达到0x3F，系统将复位
6. **调试模式**：调试模式下WWDG可能暂停，影响测试结果
7. **时钟稳定性**：WWDG依赖于PCLK1时钟，时钟异常会影响WWDG
8. **中断优先级**：WWDG中断应设置为较高优先级，确保及时响应

## 9. 性能优化建议

1. **窗口时间优化**：
   - 根据应用需求精确计算窗口时间
   - 考虑中断响应时间和喂狗函数执行时间
   - 留出足够的安全余量

2. **喂狗策略优化**：
   - 在主循环关键位置喂狗
   - 避免在长时间阻塞的操作中喂狗
   - 实现分层次的喂狗机制

3. **中断处理优化**：
   - 中断处理函数尽量简短
   - 避免在中断中进行复杂计算
   - 考虑使用DMA减轻CPU负担

4. **功耗优化**：
   - 不使用WWDG时禁用
   - 在低功耗模式下考虑WWDG行为
   - 动态调整喂狗频率

## 10. 典型应用场景

### 10.1 实时任务监控

```c
// 使用WWDG监控关键任务执行
void Critical_Task_Monitoring(void)
{
    // 任务开始前重置WWDG
    WWDG_SetCounter(WWDG_CNT);

    // 执行关键任务
    Execute_Critical_Task();

    // 任务完成后喂狗
    if(WWDG_GetCounter() < WWDG_WINDOW_VALUE)
    {
        WWDG_SetCounter(WWDG_CNT);
    }
    else
    {
        // 任务执行时间过长，进行错误处理
        Handle_Task_Timeout();
    }
}
```

### 10.2 系统健康监测

```c
// 系统健康监测框架
void System_Health_Monitoring(void)
{
    static uint32_t last_health_check = 0;
    uint32_t current_time = GetTickCount();

    // 定期检查系统健康状态
    if(current_time - last_health_check >= HEALTH_CHECK_INTERVAL)
    {
        // 检查关键系统参数
        if(Check_System_Parameters())
        {
            // 系统正常，喂狗
            Safe_WWDG_Feed();
        }
        else
        {
            // 系统异常，不喂狗，让WWDG复位系统
            printf("System health check failed!\r\n");
            // 记录错误信息
            Log_Error_Information();
        }

        last_health_check = current_time;
    }
}
```

### 10.3 安全关键应用

```c
// 安全关键应用的WWDG配置
void Safety_Critical_Application(void)
{
    // 配置严格的窗口时间
    WWDG_Config(0x7F, 0x50, WWDG_Prescaler_8);  // 窗口时间约10ms

    // 关键操作必须在窗口内完成
    while(1)
    {
        // 读取当前计数器值
        uint8_t counter = WWDG_GetCounter() & 0x7F;

        // 执行安全关键操作
        Safety_Critical_Operation();

        // 检查是否仍在窗口内
        if(counter < 0x50)
        {
            // 在窗口内，喂狗
            WWDG_SetCounter(WWDG_CNT);
        }
        else
        {
            // 超出窗口，系统可能异常
            Enter_Safe_State();
            // 不喂狗，让WWDG复位系统
        }
    }
}
```

## 11. WWDG与IWDG对比表

| 对比项 | WWDG（窗口看门狗） | IWDG（独立看门狗） |
|--------|-------------------|-------------------|
| **时钟源** | PCLK1（APB1总线时钟） | LSI（40kHz内部时钟） |
| **复位条件** | 未在窗口内喂狗 | 超时未喂狗 |
| **窗口机制** | 有，必须在特定时间窗口内喂狗 | 无，任何时间喂狗都可以 |
| **中断支持** | 支持早期唤醒中断 | 不支持中断 |
| **配置灵活性** | 高，可配置窗口值和预分频器 | 较低，配置选项有限 |
| **独立性** | 依赖系统时钟 | 完全独立，不受系统时钟影响 |
| **可靠性** | 受主时钟影响，时钟异常时可能失效 | 高可靠性，即使主时钟故障也能工作 |
| **功耗** | 依赖主时钟，功耗较高 | 低功耗，使用低速内部时钟 |
| **调试影响** | 调试模式下可能暂停 | 调试模式下继续运行 |
| **典型应用** | 需要精确时序控制的应用，实时任务监控 | 需要高可靠性的安全应用，系统保护 |

## 12. 总结

CH32V307的WWDG窗口看门狗提供了精确的时间窗口监控机制，适用于需要严格控制软件执行时序的应用场景。通过窗口机制，WWDG可以检测到过早或过晚的喂狗操作，从而发现软件执行异常。早期唤醒中断功能为系统提供了在复位前进行紧急处理的机会。

开发时需要注意窗口时间和喂狗时机的精确计算，合理配置窗口值和计数器初值。WWDG特别适合用于实时任务监控、系统健康监测和安全关键应用等场景。与IWDG相比，WWDG提供了更高的配置灵活性和精确的时序控制，但可靠性略低于完全独立的IWDG。
# CH32V307 SYSTICK系统定时器使用指南

## 1. 外设概述

SYSTICK（System Timer，系统定时器）是CH32V307内置的24位向下递减计数器，用于生成系统滴答中断。主要特性包括：

- **24位递减计数器**：最大计数值16,777,215
- **多种时钟源**：HCLK或HCLK/8
- **自动重载**：计数器递减到0时自动重载比较值
- **中断生成**：计数器递减到0时产生中断
- **简单配置**：最小化的寄存器配置
- **系统心跳**：常用于操作系统的时间片调度

## 2. 关键代码片段

### 2.1 SYSTICK基本配置示例

```c
#include "ch32v30x.h"
#include "debug.h"

uint32_t counter = 0;

// SYSTICK初始化配置
void SYSTICK_Init_Config(u_int64_t ticks)
{
    SysTick->SR &= ~(1 << 0);  // 清除状态标志位
    SysTick->CMP = ticks;       // 设置比较值
    SysTick->CNT = 0;           // 计数器清零
    SysTick->CTLR = 0xF;        // 配置控制寄存器

    NVIC_SetPriority(SysTicK_IRQn, 15);  // 设置中断优先级
    NVIC_EnableIRQ(SysTicK_IRQn);        // 使能SYSTICK中断
}

// SYSTICK中断处理函数
void SysTick_Handler(void) __attribute__((interrupt("WCH-Interrupt-fast")));
void SysTick_Handler(void)
{
    if(SysTick->SR == 1)
    {
        SysTick->SR = 0;  // 清除状态标志位
        printf("welcome to WCH\r\n");  // 打印欢迎信息
        counter++;                     // 计数器递增
        printf("Counter:%d\r\n",counter);  // 打印计数器值
    }
}

int main(void)
{
    SystemCoreClockUpdate();                    // 更新系统时钟频率
    NVIC_PriorityGroupConfig(NVIC_PriorityGroup_2);  // 配置中断优先级分组
    USART_Printf_Init(115200);                  // 初始化串口，波特率115200
    printf("SystemClk:%d\r\n",SystemCoreClock); // 打印系统时钟频率

    // 初始化SYSTICK，设置比较值为系统时钟-1
    // 当系统时钟为96MHz时，中断频率约为1Hz（96MHz/96MHz = 1秒）
    SYSTICK_Init_Config(SystemCoreClock-1);

    while(1)
    {
        // 主循环，等待中断
    }
}
```

### 2.2 SYSTICK延时函数实现

```c
// 基于SYSTICK的毫秒级延时函数
void Delay_Ms(uint32_t ms)
{
    uint32_t ticks = (SystemCoreClock / 1000) * ms - 1;

    SysTick->SR = 0;          // 清除状态标志
    SysTick->CMP = ticks;     // 设置比较值
    SysTick->CNT = 0;         // 计数器清零
    SysTick->CTLR = 0xF;      // 启动定时器

    // 等待定时器完成
    while((SysTick->SR & 0x01) == 0);

    SysTick->CTLR = 0;        // 停止定时器
}

// 基于SYSTICK的微秒级延时函数
void Delay_Us(uint32_t us)
{
    uint32_t ticks = (SystemCoreClock / 1000000) * us - 1;

    if(ticks > 0xFFFFFF) ticks = 0xFFFFFF;  // 限制最大值

    SysTick->SR = 0;          // 清除状态标志
    SysTick->CMP = ticks;     // 设置比较值
    SysTick->CNT = 0;         // 计数器清零
    SysTick->CTLR = 0xF;      // 启动定时器

    // 等待定时器完成
    while((SysTick->SR & 0x01) == 0);

    SysTick->CTLR = 0;        // 停止定时器
}
```

### 2.3 SYSTICK作为系统时钟节拍

```c
// 操作系统时钟节拍配置
#define TICKS_PER_SECOND    1000  // 每秒1000个节拍

volatile uint32_t system_tick = 0;

// 系统时钟节拍初始化
void SystemTick_Init(void)
{
    uint32_t ticks = SystemCoreClock / TICKS_PER_SECOND - 1;

    SysTick->SR = 0;          // 清除状态标志
    SysTick->CMP = ticks;     // 设置比较值
    SysTick->CNT = 0;         // 计数器清零
    SysTick->CTLR = 0xF;      // 启动定时器

    NVIC_SetPriority(SysTicK_IRQn, 0);  // 设置高优先级
    NVIC_EnableIRQ(SysTicK_IRQn);        // 使能中断
}

// 系统节拍中断处理
void SysTick_Handler(void) __attribute__((interrupt("WCH-Interrupt-fast")));
void SysTick_Handler(void)
{
    if(SysTick->SR == 1)
    {
        SysTick->SR = 0;      // 清除中断标志
        system_tick++;        // 系统节拍计数器递增

        // 操作系统调度器（示例）
        // OS_Scheduler();
    }
}

// 获取系统运行时间（毫秒）
uint32_t Get_System_Milliseconds(void)
{
    return system_tick;
}

// 阻塞延时（基于系统节拍）
void OS_Delay(uint32_t ms)
{
    uint32_t target_tick = system_tick + ms;
    while(system_tick < target_tick);
}
```

### 2.4 SYSTICK性能测量

```c
// 使用SYSTICK测量代码执行时间
uint32_t Measure_Execution_Time(void (*func)(void))
{
    uint32_t start_cnt, end_cnt, elapsed_ticks;

    // 配置SYSTICK为最大计数值
    SysTick->SR = 0;
    SysTick->CMP = 0xFFFFFF;
    SysTick->CNT = 0;
    SysTick->CTLR = 0xF;

    // 获取开始计数值
    start_cnt = SysTick->CNT;

    // 执行待测函数
    func();

    // 获取结束计数值
    end_cnt = SysTick->CNT;

    // 计算经过的时钟周期数
    elapsed_ticks = start_cnt - end_cnt;

    SysTick->CTLR = 0;  // 停止定时器

    // 转换为微秒
    return (elapsed_ticks * 1000000) / SystemCoreClock;
}

// 测量示例函数
void Example_Function(void)
{
    for(int i = 0; i < 1000; i++)
    {
        asm("nop");
    }
}

// 使用示例
void Performance_Test(void)
{
    uint32_t execution_time_us = Measure_Execution_Time(Example_Function);
    printf("Execution time: %d us\r\n", execution_time_us);
}
```

## 3. 配置数据结构

### 3.1 SYSTICK寄存器定义

```c
// SYSTICK寄存器结构（ch32v30x.h）
typedef struct
{
  __IO uint32_t CTLR;  // 控制寄存器
  __IO uint32_t SR;    // 状态寄存器
  __IO uint32_t CNT;   // 计数器寄存器
  __IO uint32_t CMP;   // 比较寄存器
} SysTick_TypeDef;

#define SysTick_BASE      (0xE000F000)  // SYSTICK基地址
#define SysTick           ((SysTick_TypeDef *)SysTick_BASE)
```

### 3.2 控制寄存器（CTLR）位定义

```c
// 控制寄存器位定义
#define SYSTICK_CTLR_STE       (1 << 0)  // 计数器使能
#define SYSTICK_CTLR_STIE      (1 << 1)  // 中断使能
#define SYSTICK_CTLR_STCLK     (1 << 2)  // 时钟源选择（0=HCLK/8，1=HCLK）
#define SYSTICK_CTLR_STRE      (1 << 3)  // 自动重载使能

// 完整配置值
#define SYSTICK_CONFIG         (SYSTICK_CTLR_STE | SYSTICK_CTLR_STIE | \
                               SYSTICK_CTLR_STCLK | SYSTICK_CTLR_STRE)
```

### 3.3 状态寄存器（SR）位定义

```c
// 状态寄存器位定义
#define SYSTICK_SR_CNTIF       (1 << 0)  // 计数完成中断标志
#define SYSTICK_SR_CNTE        (1 << 1)  // 计数使能状态

// 中断标志清除
#define SYSTICK_CLEAR_FLAG     (1 << 0)
```

### 3.4 中断号定义

```c
// SYSTICK中断号（ch32v30x.h）
#define SysTicK_IRQn           ((IRQn_Type)12)  // SYSTICK中断号

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

## 4. 通用配置步骤

### 4.1 SYSTICK基本配置步骤

1. **计算比较值**：
   ```c
   // 计算公式：中断周期 = (CMP + 1) / 系统时钟频率
   uint32_t ticks = SystemCoreClock / frequency - 1;
   ```

2. **初始化SYSTICK**：
   ```c
   void SYSTICK_Init(uint32_t compare_value)
   {
       SysTick->SR = 0;                // 清除状态标志
       SysTick->CMP = compare_value;   // 设置比较值
       SysTick->CNT = 0;               // 计数器清零
       SysTick->CTLR = 0xF;            // 配置控制寄存器
   }
   ```

3. **配置中断**：
   ```c
   void SYSTICK_Interrupt_Config(void)
   {
       NVIC_SetPriority(SysTicK_IRQn, 0);  // 设置中断优先级
       NVIC_EnableIRQ(SysTicK_IRQn);       // 使能中断
   }
   ```

4. **编写中断处理函数**：
   ```c
   void SysTick_Handler(void) __attribute__((interrupt("WCH-Interrupt-fast")));
   void SysTick_Handler(void)
   {
       if(SysTick->SR & SYSTICK_SR_CNTIF)
       {
           SysTick->SR = SYSTICK_CLEAR_FLAG;  // 清除中断标志
           // 处理中断任务
       }
   }
   ```

### 4.2 延时函数配置步骤

```c
// 毫秒延时函数配置
void Delay_Init(void)
{
    // 系统时钟已通过SystemCoreClockUpdate()更新
}

void Delay_Ms(uint32_t ms)
{
    uint32_t ticks = (SystemCoreClock / 1000) * ms - 1;

    SysTick->SR = 0;
    SysTick->CMP = ticks;
    SysTick->CNT = 0;
    SysTick->CTLR = 0xF;

    while((SysTick->SR & 0x01) == 0);
    SysTick->CTLR = 0;
}

void Delay_Us(uint32_t us)
{
    uint32_t ticks = (SystemCoreClock / 1000000) * us - 1;

    if(ticks > 0xFFFFFF) ticks = 0xFFFFFF;

    SysTick->SR = 0;
    SysTick->CMP = ticks;
    SysTick->CNT = 0;
    SysTick->CTLR = 0xF;

    while((SysTick->SR & 0x01) == 0);
    SysTick->CTLR = 0;
}
```

### 4.3 系统时钟节拍配置步骤

```c
// 操作系统时钟节拍配置
#define OS_TICK_RATE    1000  // 1000Hz

volatile uint32_t os_tick_count = 0;

void OS_Tick_Init(void)
{
    uint32_t ticks = SystemCoreClock / OS_TICK_RATE - 1;

    // 配置SYSTICK
    SysTick->SR = 0;
    SysTick->CMP = ticks;
    SysTick->CNT = 0;
    SysTick->CTLR = 0xF;

    // 配置中断
    NVIC_SetPriority(SysTicK_IRQn, 0);
    NVIC_EnableIRQ(SysTicK_IRQn);
}

// 时钟节拍中断
void SysTick_Handler(void) __attribute__((interrupt("WCH-Interrupt-fast")));
void SysTick_Handler(void)
{
    if(SysTick->SR & SYSTICK_SR_CNTIF)
    {
        SysTick->SR = SYSTICK_CLEAR_FLAG;
        os_tick_count++;

        // 唤醒延时任务
        // OS_Tick_Handler();
    }
}
```

### 4.4 性能测量配置步骤

```c
// 代码执行时间测量
uint32_t Profile_Code(void (*function)(void))
{
    uint32_t start_time, end_time, elapsed_cycles;

    // 保存当前SYSTICK配置
    uint32_t saved_ctlr = SysTick->CTLR;
    uint32_t saved_cmp = SysTick->CMP;
    uint32_t saved_cnt = SysTick->CNT;

    // 配置为最大计数值，自由运行
    SysTick->SR = 0;
    SysTick->CMP = 0xFFFFFF;
    SysTick->CNT = 0;
    SysTick->CTLR = 0x1;  // 只使能计数器，不使能中断

    start_time = SysTick->CNT;
    function();  // 执行被测函数
    end_time = SysTick->CNT;

    // 计算时钟周期数
    if(end_time <= start_time)
        elapsed_cycles = start_time - end_time;
    else
        elapsed_cycles = (0xFFFFFF - end_time) + start_time + 1;

    // 恢复原始配置
    SysTick->CTLR = 0;
    SysTick->CMP = saved_cmp;
    SysTick->CNT = saved_cnt;
    SysTick->CTLR = saved_ctlr;

    // 转换为时间单位
    return (elapsed_cycles * 1000000) / SystemCoreClock;  // 微秒
}
```

## 5. 示例工程详解

### 5.1 SYSTICK示例工程

- **位置**：`SYSTICK/SYSTICK_Interrupt/User/main.c`
- **功能**：演示SYSTICK中断基本使用，每秒产生一次中断
- **特点**：
  - 使用HCLK作为时钟源
  - 比较值设置为SystemCoreClock-1
  - 中断处理函数中打印信息和计数
  - 串口输出调试信息

### 5.2 工程结构说明

```
SYSTICK/
└── SYSTICK_Interrupt/
    ├── .cproject                    # Eclipse项目配置
    ├── .project                    # Eclipse项目文件
    ├── SYSTICK.wvproj              # WCH IDE项目文件
    └── User/
        ├── main.c                  # 主程序文件
        ├── ch32v30x_conf.h         # 外设配置文件
        ├── ch32v30x_it.c           # 中断服务程序
        ├── ch32v30x_it.h           # 中断头文件
        ├── system_ch32v30x.c       # 系统初始化
        └── system_ch32v30x.h       # 系统头文件
```

### 5.3 关键功能点

1. **中断频率计算**：
   ```c
   // 中断周期 = (CMP + 1) / 系统时钟频率
   // 当CMP = SystemCoreClock-1时，中断周期 = SystemCoreClock / 系统时钟频率 = 1秒
   ```

2. **中断优先级配置**：
   ```c
   NVIC_PriorityGroupConfig(NVIC_PriorityGroup_2);  // 配置优先级分组
   NVIC_SetPriority(SysTicK_IRQn, 15);              // 设置低优先级
   ```

3. **中断标志处理**：
   ```c
   if(SysTick->SR == 1)    // 检查中断标志
   {
       SysTick->SR = 0;    // 清除标志
       // 处理中断
   }
   ```

4. **系统时钟配置**：
   ```c
   // 使用96MHz HSE时钟
   #define SYSCLK_FREQ_96MHz_HSE  96000000
   uint32_t SystemCoreClock = SYSCLK_FREQ_96MHz_HSE;
   ```

### 5.4 硬件连接要求

SYSTICK是内部定时器，无需外部引脚连接。但需要注意：
- **时钟源选择**：HCLK或HCLK/8
- **系统时钟稳定性**：确保系统时钟稳定
- **中断响应时间**：考虑中断处理函数的执行时间

## 6. 常见问题

### Q1: SYSTICK中断不触发？
A: 检查以下配置：
1. SYSTICK时钟源是否正确使能
2. 比较值（CMP）是否设置正确
3. 中断是否使能（CTLR寄存器）
4. NVIC中断是否配置正确
5. 中断处理函数是否正确定义

### Q2: 中断频率不准确？
A: 可能原因：
1. 系统时钟频率不正确
2. 比较值计算错误
3. 中断处理函数执行时间过长
4. 其他高优先级中断抢占

### Q3: 如何测量短时间间隔？
A: 建议方案：
1. 使用SYSTICK的24位计数器直接读取CNT寄存器
2. 配置为最大计数值（0xFFFFFF）
3. 通过两次读取CNT值计算时间差
4. 考虑计数器溢出情况

### Q4: SYSTICK与其他定时器冲突？
A: SYSTICK是独立定时器：
1. 不占用通用定时器资源
2. 有自己的中断通道
3. 可以与其他定时器同时使用
4. 注意中断优先级分配

### Q5: 如何在低功耗模式下使用SYSTICK？
A: 配置建议：
1. 选择低功耗时钟源（如HCLK/8）
2. 调整比较值以适应低频率
3. 考虑使用唤醒中断
4. 动态调整SYSTICK配置

## 7. API参考

### 7.1 SYSTICK寄存器操作API

| 寄存器 | 功能描述 | 访问方式 |
|--------|----------|----------|
| `SysTick->CTLR` | 控制寄存器 | 读写 |
| `SysTick->SR` | 状态寄存器 | 读写 |
| `SysTick->CNT` | 计数器寄存器 | 读写 |
| `SysTick->CMP` | 比较寄存器 | 读写 |

### 7.2 中断配置API

| 函数名 | 功能描述 | 参数说明 |
|--------|----------|----------|
| `NVIC_EnableIRQ` | 使能中断 | IRQn：中断号（SysTicK_IRQn） |
| `NVIC_DisableIRQ` | 禁用中断 | IRQn：中断号 |
| `NVIC_SetPriority` | 设置中断优先级 | IRQn：中断号，priority：优先级 |
| `NVIC_GetPriority` | 获取中断优先级 | IRQn：中断号 |

### 7.3 系统时钟API

| 函数名 | 功能描述 | 参数说明 |
|--------|----------|----------|
| `SystemCoreClockUpdate` | 更新系统时钟变量 | 无 |
| `RCC_GetClocksFreq` | 获取各时钟域频率 | RCC_Clocks：时钟频率结构体指针 |

### 7.4 常用配置常量

```c
// 时钟源选择
#define SYSTICK_CLKSRC_HCLK     0x4  // HCLK时钟源
#define SYSTICK_CLKSRC_HCLK_DIV8 0x0  // HCLK/8时钟源

// 控制寄存器配置
#define SYSTICK_ENABLE          0x1  // 计数器使能
#define SYSTICK_INT_ENABLE      0x2  // 中断使能
#define SYSTICK_RELOAD_ENABLE   0x8  // 自动重载使能

// 完整配置
#define SYSTICK_CONFIG_FULL     (SYSTICK_ENABLE | SYSTICK_INT_ENABLE | \
                                SYSTICK_CLKSRC_HCLK | SYSTICK_RELOAD_ENABLE)
```

## 8. 注意事项

1. **中断处理时间**：SYSTICK中断处理函数应尽量简短
2. **优先级设置**：根据应用需求合理设置中断优先级
3. **计数器溢出**：24位计数器最大值为0xFFFFFF，注意溢出处理
4. **时钟源选择**：HCLK/8适用于低功耗场景，HCLK适用于精确计时
5. **多任务环境**：在操作系统中注意保护共享数据
6. **中断嵌套**：合理配置中断优先级防止嵌套问题
7. **调试影响**：调试模式下SYSTICK行为可能不同
8. **低功耗模式**：某些低功耗模式下SYSTICK可能停止

## 9. 性能优化建议

1. **中断优化**：
   - 使用最小必要的中断处理逻辑
   - 避免在中断中执行复杂计算
   - 考虑使用DMA减轻CPU负担

2. **精度优化**：
   - 使用HCLK作为时钟源提高精度
   - 校准系统时钟频率
   - 考虑中断响应时间的影响

3. **功耗优化**：
   - 不使用SYSTICK时禁用
   - 低功耗场景使用HCLK/8时钟源
   - 动态调整中断频率

4. **多任务优化**：
   - 使用SYSTICK作为操作系统时钟节拍
   - 实现高效的任务调度器
   - 优化上下文切换时间

## 10. 典型应用场景

### 10.1 操作系统时钟节拍

```c
// RTOS时钟节拍实现
void RTOS_Tick_Init(void)
{
    // 配置1ms时钟节拍
    uint32_t ticks = SystemCoreClock / 1000 - 1;

    SysTick->SR = 0;
    SysTick->CMP = ticks;
    SysTick->CNT = 0;
    SysTick->CTLR = 0xF;

    // 设置最高优先级
    NVIC_SetPriority(SysTicK_IRQn, 0);
    NVIC_EnableIRQ(SysTicK_IRQn);
}

// 时钟节拍中断
volatile uint32_t rtos_tick = 0;

void SysTick_Handler(void) __attribute__((interrupt("WCH-Interrupt-fast")));
void SysTick_Handler(void)
{
    if(SysTick->SR & 0x01)
    {
        SysTick->SR = 0;
        rtos_tick++;

        // 检查延时任务
        // Check_Delayed_Tasks();

        // 任务调度
        // Task_Scheduler();
    }
}
```

### 10.2 精确时间测量

```c
// 高精度时间测量
typedef struct {
    uint32_t start_cycle;
    uint32_t end_cycle;
    uint32_t elapsed_us;
} Time_Measurement;

void Start_Measurement(Time_Measurement *tm)
{
    // 配置SYSTICK为最大计数值，自由运行
    SysTick->SR = 0;
    SysTick->CMP = 0xFFFFFF;
    SysTick->CNT = 0;
    SysTick->CTLR = 0x1;  // 只使能计数器

    tm->start_cycle = SysTick->CNT;
}

void Stop_Measurement(Time_Measurement *tm)
{
    tm->end_cycle = SysTick->CNT;
    SysTick->CTLR = 0;  // 停止计数器

    // 计算经过的时钟周期
    uint32_t elapsed_cycles;
    if(tm->end_cycle <= tm->start_cycle)
        elapsed_cycles = tm->start_cycle - tm->end_cycle;
    else
        elapsed_cycles = (0xFFFFFF - tm->end_cycle) + tm->start_cycle + 1;

    // 转换为微秒
    tm->elapsed_us = (elapsed_cycles * 1000000) / SystemCoreClock;
}

// 使用示例
void Measure_Function_Performance(void)
{
    Time_Measurement tm;

    Start_Measurement(&tm);
    // 执行待测函数
    Complex_Calculation();
    Stop_Measurement(&tm);

    printf("Execution time: %d us\r\n", tm.elapsed_us);
}
```

### 10.3 周期性任务调度

```c
// 简单任务调度器
typedef struct {
    void (*task)(void);
    uint32_t period_ms;
    uint32_t last_run;
} Scheduled_Task;

#define MAX_TASKS 10
Scheduled_Task task_list[MAX_TASKS];
uint8_t task_count = 0;

// 添加任务
uint8_t Add_Scheduled_Task(void (*task)(void), uint32_t period_ms)
{
    if(task_count >= MAX_TASKS) return 0;

    task_list[task_count].task = task;
    task_list[task_count].period_ms = period_ms;
    task_list[task_count].last_run = 0;
    task_count++;

    return 1;
}

// 任务调度器
void Task_Scheduler(void)
{
    static uint32_t last_tick = 0;
    uint32_t current_tick = Get_System_Milliseconds();

    for(int i = 0; i < task_count; i++)
    {
        if(current_tick - task_list[i].last_run >= task_list[i].period_ms)
        {
            task_list[i].task();
            task_list[i].last_run = current_tick;
        }
    }
}

// SYSTICK中断中调用
void SysTick_Handler(void) __attribute__((interrupt("WCH-Interrupt-fast")));
void SysTick_Handler(void)
{
    if(SysTick->SR & 0x01)
    {
        SysTick->SR = 0;
        Task_Scheduler();  // 执行任务调度
    }
}
```

## 11. 总结

CH32V307的SYSTICK系统定时器是一个简单但功能强大的24位递减计数器，为系统提供了精确的时间基准。通过灵活的配置选项，可以满足从简单的延时功能到复杂的操作系统时钟节拍等各种应用需求。开发时需要注意中断处理效率、时钟源选择、优先级配置等关键环节，以获得最佳的系统性能和稳定性。
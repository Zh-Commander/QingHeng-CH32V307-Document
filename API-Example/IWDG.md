# CH32V307 IWDG独立看门狗使用指南

## 1. 外设概述

IWDG（Independent Watchdog，独立看门狗）是CH32V307的硬件看门狗定时器，用于检测和恢复软件故障。主要特性包括：

- **独立时钟源**：使用40kHz LSI（低速内部时钟），不受主时钟影响
- **可靠的复位机制**：超时后产生系统复位
- **简单配置**：预分频和重装载值配置
- **写保护机制**：防止意外修改配置
- **低功耗运行**：在低功耗模式下仍可工作

## 2. 关键代码片段

### 2.1 IWDG初始化函数

```c
void IWDG_Feed_Init(u16 prer, u16 rlr)
{
    IWDG_WriteAccessCmd(IWDG_WriteAccess_Enable);  // 使能写访问
    IWDG_SetPrescaler(prer);                       // 设置预分频值
    IWDG_SetReload(rlr);                           // 设置重装载值
    IWDG_ReloadCounter();                          // 重载计数器
    IWDG_Enable();                                 // 使能IWDG
}
```

### 2.2 主程序配置示例

```c
int main(void)
{
    SystemCoreClockUpdate();
    Delay_Init();
    USART_Printf_Init(115200);
    printf("SystemClk:%d\r\n", SystemCoreClock);

    // 配置IWDG：预分频32，重装载值4000，超时时间3.2秒
    IWDG_Feed_Init(IWDG_Prescaler_32, 4000);

    // GPIO配置：PA0作为输入
    GPIO_InitTypeDef GPIO_InitStructure = {0};
    RCC_APB2PeriphClockCmd(RCC_APB2Periph_GPIOA, ENABLE);
    GPIO_InitStructure.GPIO_Pin = GPIO_Pin_0;
    GPIO_InitStructure.GPIO_Mode = GPIO_Mode_IPD;  // 下拉输入
    GPIO_Init(GPIOA, &GPIO_InitStructure);

    while(1)
    {
        // 检查PA0状态，高电平时喂狗
        if(GPIO_ReadInputDataBit(GPIOA, GPIO_Pin_0) == SET)
        {
            IWDG_ReloadCounter();  // 喂狗操作
            printf("Feed dog\r\n");
        }
        Delay_Ms(10);
    }
}
```

### 2.3 IWDG超时时间计算

```c
// 超时时间计算公式
// IWDG时钟源：40kHz LSI
// 超时时间 = (重装载值 × 预分频值) / 40000 秒

// 示例：预分频32，重装载值4000
// 超时时间 = (4000 × 32) / 40000 = 3.2秒
```

## 3. 配置数据结构

### 3.1 IWDG寄存器结构

```c
typedef struct
{
  __IO uint32_t CTLR;  // 控制寄存器 (0x00)
  __IO uint32_t PSCR;  // 预分频寄存器 (0x04)
  __IO uint32_t RLDR;  // 重装载寄存器 (0x08)
  __IO uint32_t STATR; // 状态寄存器 (0x0C)
} IWDG_TypeDef;
```

### 3.2 预分频器选项

```c
#define IWDG_Prescaler_4            ((uint8_t)0x00)   // 4分频
#define IWDG_Prescaler_8            ((uint8_t)0x01)   // 8分频
#define IWDG_Prescaler_16           ((uint8_t)0x02)   // 16分频
#define IWDG_Prescaler_32           ((uint8_t)0x03)   // 32分频
#define IWDG_Prescaler_64           ((uint8_t)0x04)   // 64分频
#define IWDG_Prescaler_128          ((uint8_t)0x05)   // 128分频
#define IWDG_Prescaler_256          ((uint8_t)0x06)   // 256分频
```

### 3.3 状态标志

```c
#define IWDG_FLAG_PVU               ((uint16_t)0x0001)  // 预分频值更新中
#define IWDG_FLAG_RVU               ((uint16_t)0x0002)  // 重装载值更新中
```

## 4. 通用配置步骤

### 4.1 基本配置步骤

1. **使能写访问**：
   ```c
   IWDG_WriteAccessCmd(IWDG_WriteAccess_Enable);
   ```

2. **设置预分频值**：
   ```c
   IWDG_SetPrescaler(IWDG_Prescaler_32);
   ```

3. **设置重装载值**：
   ```c
   IWDG_SetReload(4000);  // 范围：0-0x0FFF
   ```

4. **重载计数器**：
   ```c
   IWDG_ReloadCounter();
   ```

5. **使能IWDG**：
   ```c
   IWDG_Enable();
   ```

### 4.2 喂狗操作

```c
// 定期调用喂狗函数
IWDG_ReloadCounter();

// 或在中断中喂狗
void TIM2_IRQHandler(void)
{
    if(TIM_GetITStatus(TIM2, TIM_IT_Update) != RESET)
    {
        IWDG_ReloadCounter();  // 定时喂狗
        TIM_ClearITPendingBit(TIM2, TIM_IT_Update);
    }
}
```

### 4.3 超时时间计算步骤

1. **确定应用需求**：根据系统响应时间确定合理的超时时间
2. **选择预分频值**：根据超时时间和重装载值范围选择
3. **计算重装载值**：`重装载值 = (超时时间 × 40000) / 预分频值`
4. **验证参数**：确保重装载值在0-4095范围内

## 5. 示例工程详解

### 5.1 IWDG示例工程

- **位置**：`IWDG/IWDG/User/main.c`
- **功能**：演示IWDG基本使用和喂狗操作
- **特点**：
  - 完整的IWDG初始化流程
  - GPIO输入检测喂狗条件
  - 串口输出状态信息
  - 3.2秒超时时间配置
- **适用场景**：
  - 系统故障检测
  - 防止程序跑飞
  - 提高系统可靠性

### 5.2 关键功能点

1. **超时时间配置**：3.2秒超时，适合大多数应用
2. **喂狗条件**：通过PA0引脚状态控制喂狗
3. **调试输出**：通过串口输出喂狗状态
4. **简单可靠**：代码简洁，易于理解和集成

## 6. 常见问题

### Q1: IWDG不工作或无法复位？
A: 检查以下配置：
1. 是否使能了写访问（`IWDG_WriteAccessCmd`）
2. 配置顺序是否正确（先预分频，后重装载）
3. 是否调用了`IWDG_Enable()`使能看门狗
4. LSI时钟是否正常工作

### Q2: 超时时间不准确？
A: 可能原因：
1. LSI时钟频率有偏差（典型40kHz，实际可能有±5%偏差）
2. 计算错误：超时时间 = (RLDR × 预分频值) / 40000
3. 预分频值选择不当

### Q3: 如何检测IWDG复位？
A: 通过RCC复位标志检测：
```c
if(RCC_GetFlagStatus(RCC_FLAG_IWDGRST) != RESET)
{
    printf("IWDG reset occurred\r\n");
    RCC_ClearFlag();  // 清除复位标志
}
```

### Q4: 与WWDG有什么区别？
A: 主要区别：
| 特性 | IWDG | WWDG |
|------|------|------|
| 时钟源 | 40kHz LSI | PCLK1 |
| 超时时间 | 较慢（毫秒级） | 较快（微秒级） |
| 窗口功能 | 无 | 有窗口限制 |
| 配置复杂性 | 简单 | 较复杂 |
| 适用场景 | 简单复位保护 | 需要窗口保护的应用 |

### Q5: 在低功耗模式下IWDG还能工作吗？
A: 是的，IWDG使用独立的LSI时钟，在睡眠、停机和待机模式下仍可工作（如果LSI使能）。

## 7. API参考

### 7.1 主要API函数

| 函数名 | 功能描述 | 参数说明 |
|--------|----------|----------|
| `IWDG_WriteAccessCmd` | 使能/禁用写访问 | IWDG_WriteAccess：写访问使能状态 |
| `IWDG_SetPrescaler` | 设置预分频值 | IWDG_Prescaler：预分频值 |
| `IWDG_SetReload` | 设置重装载值 | Reload：重装载值（0-0x0FFF） |
| `IWDG_ReloadCounter` | 重载计数器（喂狗） | 无 |
| `IWDG_Enable` | 使能IWDG | 无 |
| `IWDG_GetFlagStatus` | 获取状态标志 | IWDG_FLAG：标志位 |

### 7.2 写访问控制

```c
#define IWDG_WriteAccess_Enable     ((uint16_t)0x5555)
#define IWDG_WriteAccess_Disable    ((uint16_t)0x0000)
```

### 7.3 复位标志

```c
// RCC中的IWDG复位标志
#define RCC_FLAG_IWDGRST            ((uint8_t)0x7D)
```

## 8. 注意事项

1. **配置顺序**：必须按照正确顺序配置IWDG寄存器
2. **写保护**：修改配置前必须先使能写访问
3. **一次性配置**：一旦使能IWDG，只有系统复位才能禁用
4. **喂狗时机**：在超时前必须喂狗，否则系统复位
5. **LSI精度**：LSI时钟频率有偏差，设计时需考虑余量
6. **低功耗模式**：在低功耗模式下仍需定期喂狗

## 9. 性能优化建议

1. **合理设置超时时间**：根据系统最坏情况响应时间设置
2. **减少喂狗开销**：在主循环关键位置喂狗，避免频繁喂狗
3. **错误恢复策略**：在复位后恢复系统状态，记录复位原因
4. **配合其他机制**：与软件看门狗、心跳检测等机制配合使用

## 10. 典型应用场景

### 10.1 系统监控保护

```c
// 系统监控任务
void SystemMonitor_Task(void)
{
    // 检查系统关键状态
    if(CheckSystemStatus() == ERROR)
    {
        // 系统异常，不喂狗，触发复位
        printf("System error, waiting for IWDG reset\r\n");
    }
    else
    {
        // 系统正常，喂狗
        IWDG_ReloadCounter();
    }
}
```

### 10.2 任务执行监控

```c
// 多任务系统看门狗管理
typedef struct {
    uint32_t task_id;
    uint32_t last_checkin;
    uint32_t timeout;
} TaskWatchdog_t;

TaskWatchdog_t task_watchdogs[MAX_TASKS];

void Task_CheckIn(uint32_t task_id)
{
    task_watchdogs[task_id].last_checkin = GetTickCount();
}

void Watchdog_Manager(void)
{
    uint32_t current_time = GetTickCount();
    uint32_t all_tasks_ok = 1;

    // 检查所有任务
    for(int i = 0; i < MAX_TASKS; i++)
    {
        if((current_time - task_watchdogs[i].last_checkin) > task_watchdogs[i].timeout)
        {
            all_tasks_ok = 0;
            printf("Task %d timeout\r\n", i);
            break;
        }
    }

    // 所有任务正常则喂狗
    if(all_tasks_ok)
    {
        IWDG_ReloadCounter();
    }
}
```

### 10.3 安全关键应用

```c
// 安全关键系统看门狗管理
void SafetyCritical_System(void)
{
    // 初始化IWDG
    IWDG_Feed_Init(IWDG_Prescaler_32, 4000);  // 3.2秒超时

    // 主安全循环
    while(1)
    {
        // 执行安全检查
        SafetyCheckResult result = PerformSafetyCheck();

        switch(result)
        {
            case SAFETY_OK:
                IWDG_ReloadCounter();  // 安全状态正常，喂狗
                ExecuteNormalOperation();
                break;

            case SAFETY_WARNING:
                IWDG_ReloadCounter();  // 警告状态，仍喂狗但记录
                LogWarning("Safety warning detected");
                ExecuteDegradedOperation();
                break;

            case SAFETY_CRITICAL:
                // 严重错误，不喂狗，等待复位
                LogError("Critical safety error");
                EnterSafeState();
                // 不喂狗，IWDG将复位系统
                break;
        }

        Delay_Ms(100);
    }
}
```

## 11. 总结

CH32V307的IWDG提供了简单可靠的硬件看门狗功能，是提高系统可靠性的重要工具。通过合理配置超时时间和定期喂狗，可以有效检测和恢复软件故障。开发时需要注意配置顺序、写保护机制和喂狗时机，确保IWDG正常工作。IWDG特别适用于对可靠性要求高的嵌入式系统，配合适当的错误处理策略，可以显著提高系统的抗干扰能力和稳定性。
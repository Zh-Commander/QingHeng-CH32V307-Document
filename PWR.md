# CH32V307 PWR电源管理使用指南

## 1. 外设概述

PWR（Power Control，电源控制）是CH32V307的电源管理模块，用于控制系统功耗和低功耗模式。主要特性包括：

- **多种低功耗模式**：睡眠模式、停机模式、待机模式
- **RAM数据保持**：支持在待机模式下保持部分RAM数据
- **电源电压检测**：内置PVD（可编程电压检测器）
- **灵活唤醒源**：支持外部中断、WKUP引脚、PVD等唤醒
- **多供电模式**：支持VDD和VBAT供电
- **时钟管理**：可控制各时钟域开关以降低功耗
- **外设电源管理**：支持ETH-PHY等外设电源控制

## 2. 关键代码片段

### 2.1 睡眠模式示例

```c
// Sleep_Mode示例
int main(void)
{
    SystemCoreClockUpdate();
    Delay_Init();
    USART_Printf_Init(115200);
    printf("SystemClk:%d\r\n",SystemCoreClock);
    printf( "ChipID:%08x\r\n", DBGMCU_GetCHIPID() );

    // GPIO配置：PA0作为唤醒源
    GPIO_InitTypeDef GPIO_InitStructure = {0};
    EXTI_InitTypeDef EXTI_InitStructure = {0};
    NVIC_InitTypeDef NVIC_InitStructure = {0};

    RCC_APB2PeriphClockCmd(RCC_APB2Periph_GPIOA | RCC_APB2Periph_AFIO, ENABLE);
    GPIO_InitStructure.GPIO_Pin = GPIO_Pin_0;
    GPIO_InitStructure.GPIO_Mode = GPIO_Mode_IPU;  // 上拉输入
    GPIO_Init(GPIOA, &GPIO_InitStructure);

    // 配置EXTI0中断
    GPIO_EXTILineConfig(GPIO_PortSourceGPIOA, GPIO_PinSource0);
    EXTI_InitStructure.EXTI_Line = EXTI_Line0;
    EXTI_InitStructure.EXTI_Mode = EXTI_Mode_Interrupt;
    EXTI_InitStructure.EXTI_Trigger = EXTI_Trigger_Falling;
    EXTI_InitStructure.EXTI_LineCmd = ENABLE;
    EXTI_Init(&EXTI_InitStructure);

    NVIC_InitStructure.NVIC_IRQChannel = EXTI0_IRQn;
    NVIC_InitStructure.NVIC_IRQChannelPreemptionPriority = 1;
    NVIC_InitStructure.NVIC_IRQChannelSubPriority = 2;
    NVIC_InitStructure.NVIC_IRQChannelCmd = ENABLE;
    NVIC_Init(&NVIC_InitStructure);

    printf("Enter Sleep Mode\r\n");

    // 进入睡眠模式前禁用HSI以降低功耗
    if(((RCC->CFGR0 & RCC_SWS)==0x04)||((RCC->CFGR0 & RCC_PLLSRC) == RCC_PLLSRC))
    {
        RCC_HSICmd(DISABLE);
    }

    __WFI();  // 进入睡眠模式（等待中断唤醒）

    // 唤醒后恢复HSI
    RCC_HSICmd(ENABLE);

    printf("Wake up from Sleep Mode\r\n");

    while(1)
    {
        Delay_Ms(1000);
        printf("System running...\r\n");
    }
}
```

### 2.2 停机模式示例

```c
// Stop_Mode示例
int main(void)
{
    SystemCoreClockUpdate();
    Delay_Init();
    USART_Printf_Init(115200);
    printf("SystemClk:%d\r\n",SystemCoreClock);

    // 配置PA0作为唤醒源（与睡眠模式类似）
    // ... GPIO和EXTI配置代码

    // 使能PWR时钟
    RCC_APB1PeriphClockCmd(RCC_APB1Periph_PWR, ENABLE);

    printf("Enter Stop Mode\r\n");
    Delay_Ms(10);

    // 进入停机模式
    PWR_EnterSTOPMode(PWR_Regulator_LowPower, PWR_STOPEntry_WFI);

    // 唤醒后需要重新配置系统时钟
    SystemInit();

    printf("Wake up from Stop Mode\r\n");

    while(1)
    {
        Delay_Ms(1000);
        printf("System running...\r\n");
    }
}
```

### 2.3 待机模式示例

```c
// Standby_Mode示例
int main(void)
{
    SystemCoreClockUpdate();
    Delay_Init();
    USART_Printf_Init(115200);
    printf("SystemClk:%d\r\n",SystemCoreClock);

    // 检查唤醒标志
    if(PWR_GetFlagStatus(PWR_FLAG_WU) != RESET)
    {
        printf("Standby wake up reset\r\n");
        PWR_ClearFlag(PWR_FLAG_WU);  // 清除唤醒标志
    }

    // 配置WKUP引脚（PA0）
    GPIO_InitTypeDef GPIO_InitStructure = {0};
    RCC_APB2PeriphClockCmd(RCC_APB2Periph_GPIOA, ENABLE);
    GPIO_InitStructure.GPIO_Pin = GPIO_Pin_0;
    GPIO_InitStructure.GPIO_Mode = GPIO_Mode_IPD;  // 下拉输入
    GPIO_Init(GPIOA, &GPIO_InitStructure);

    // 使能PWR时钟和WKUP引脚
    RCC_APB1PeriphClockCmd(RCC_APB1Periph_PWR, ENABLE);
    PWR_WakeUpPinCmd(ENABLE);

    printf("Press KEY(PA0) to enter standby mode\r\n");
    while(GPIO_ReadInputDataBit(GPIOA, GPIO_Pin_0) == RESET);  // 等待按键

    printf("Enter Standby Mode\r\n");
    Delay_Ms(10);

    // 进入待机模式
    PWR_EnterSTANDBYMode();

    // 待机模式唤醒后系统会复位，不会执行到这里
    while(1);
}
```

### 2.4 RAM数据保持待机模式

```c
// Standby_RAM_Application示例
// 链接脚本中定义保持区域
// Ld/Link.ld:
/*
.keep_2kram ORIGIN(RAM) (NOLOAD):{
    *(.keep_2kram.*)
    *(.keep_2kram)
    ASSERT(SIZEOF(.keep_2kram) <= 2048, "keep_2kram too large!");
}

.keep_30kram ORIGIN(RAM)+2048 (NOLOAD):{
    *(.keep_30kram.*)
    *(.keep_30kram)
    ASSERT(SIZEOF(.keep_30kram) <= 28672, "keep_30kram too large!");
}
*/

// 代码中使用特殊section属性
#define KEEP_2kRAM   __attribute__((section(".keep_2kram")))
#define KEEP_30kRAM  __attribute__((section(".keep_30kram")))

// 定义保持变量
KEEP_2kRAM   u32 DataBuf0[256];    // 2K RAM区域
KEEP_30kRAM  u32 DataBuf1[1024];   // 30K RAM区域

int main(void)
{
    SystemCoreClockUpdate();
    Delay_Init();
    USART_Printf_Init(115200);

    // 检查是否从待机模式唤醒
    if(PWR_GetFlagStatus(PWR_FLAG_WU) != RESET)
    {
        printf("Standby wake up reset\r\n");
        printf("DataBuf0[0] = 0x%08X\r\n", DataBuf0[0]);
        printf("DataBuf1[0] = 0x%08X\r\n", DataBuf1[0]);
        PWR_ClearFlag(PWR_FLAG_WU);
    }
    else
    {
        // 首次运行，初始化保持数据
        DataBuf0[0] = 0x12345678;
        DataBuf1[0] = 0x87654321;
        printf("First run, init data\r\n");
    }

    // 配置WKUP引脚
    // ... 配置代码

    printf("Press KEY to enter standby with RAM retention\r\n");
    while(GPIO_ReadInputDataBit(GPIOA, GPIO_Pin_0) == RESET);

    printf("Enter Standby Mode with RAM retention\r\n");

    // 进入RAM保持的待机模式（VDD供电）
    PWR_EnterSTANDBYMode_RAM();

    // 或者使用VBAT供电
    // PWR_EnterSTANDBYMode_RAM_VBAT_EN();

    // 或者低电压模式
    // PWR_EnterSTANDBYMode_RAM_LV();
    // PWR_EnterSTANDBYMode_RAM_LV_VBAT_EN();

    while(1);
}
```

### 2.5 PVD电压检测示例

```c
// PVD_VoltageJudger示例
int main(void)
{
    SystemCoreClockUpdate();
    Delay_Init();
    USART_Printf_Init(115200);
    printf("SystemClk:%d\r\n",SystemCoreClock);

    // 使能PWR时钟
    RCC_APB1PeriphClockCmd(RCC_APB1Periph_PWR, ENABLE);

    // 配置PVD阈值（2.9V）
    PWR_PVDLevelConfig(PWR_PVDLevel_2V9);
    PWR_PVDCmd(ENABLE);

    // 配置EXTI Line16中断（PVD连接到EXTI Line16）
    EXTI_InitTypeDef EXTI_InitStructure = {0};
    NVIC_InitTypeDef NVIC_InitStructure = {0};

    EXTI_InitStructure.EXTI_Line = EXTI_Line16;
    EXTI_InitStructure.EXTI_Mode = EXTI_Mode_Interrupt;
    EXTI_InitStructure.EXTI_Trigger = EXTI_Trigger_Rising_Falling;  // 上升沿和下降沿
    EXTI_InitStructure.EXTI_LineCmd = ENABLE;
    EXTI_Init(&EXTI_InitStructure);

    NVIC_InitStructure.NVIC_IRQChannel = PVD_IRQn;
    NVIC_InitStructure.NVIC_IRQChannelPreemptionPriority = 0;
    NVIC_InitStructure.NVIC_IRQChannelSubPriority = 0;
    NVIC_InitStructure.NVIC_IRQChannelCmd = ENABLE;
    NVIC_Init(&NVIC_InitStructure);

    printf("PVD voltage judger start\r\n");

    while(1)
    {
        Delay_Ms(1000);
        printf("System running...\r\n");
    }
}

// PVD中断处理程序
void PVD_IRQHandler(void)
{
    if(EXTI_GetITStatus(EXTI_Line16) != RESET)
    {
        if(PWR_GetFlagStatus(PWR_FLAG_PVDO) != RESET)
        {
            printf("VDD voltage is lower than PVD threshold(2.9V)\r\n");
        }
        else
        {
            printf("VDD voltage is higher than PVD threshold(2.9V)\r\n");
        }
        EXTI_ClearITPendingBit(EXTI_Line16);
    }
}
```

## 3. 配置数据结构

### 3.1 PWR低功耗模式定义

```c
// 低功耗模式
typedef enum
{
    PWR_Mode_Sleep,     // 睡眠模式
    PWR_Mode_Stop,      // 停机模式
    PWR_Mode_Standby    // 待机模式
} PWR_ModeTypeDef;

// 停机模式调节器状态
#define PWR_Regulator_ON                ((uint32_t)0x00000000)  // 调节器开启
#define PWR_Regulator_LowPower          ((uint32_t)0x00000001)  // 调节器低功耗模式

// 停机模式进入方式
#define PWR_STOPEntry_WFI               ((uint8_t)0x01)  // WFI进入
#define PWR_STOPEntry_WFE               ((uint8_t)0x02)  // WFE进入
```

### 3.2 PVD电压阈值定义

```c
#define PWR_PVDLevel_2V2            PWR_PVDLevel_MODE0  // 2.2V
#define PWR_PVDLevel_2V3            PWR_PVDLevel_MODE1  // 2.3V
#define PWR_PVDLevel_2V4            PWR_PVDLevel_MODE2  // 2.4V
#define PWR_PVDLevel_2V5            PWR_PVDLevel_MODE3  // 2.5V
#define PWR_PVDLevel_2V6            PWR_PVDLevel_MODE4  // 2.6V
#define PWR_PVDLevel_2V7            PWR_PVDLevel_MODE5  // 2.7V
#define PWR_PVDLevel_2V8            PWR_PVDLevel_MODE6  // 2.8V
#define PWR_PVDLevel_2V9            PWR_PVDLevel_MODE7  // 2.9V
```

### 3.3 PWR标志定义

```c
#define PWR_FLAG_WU                  ((uint32_t)0x00000001)  // 唤醒标志
#define PWR_FLAG_SB                  ((uint32_t)0x00000002)  // 待机标志
#define PWR_FLAG_PVDO                ((uint32_t)0x00000004)  // PVD输出标志
```

### 3.4 RAM保持区域定义

```c
// RAM保持区域大小
#define PWR_RAM_RETENTION_2K         (2048)     // 2KB保持区域
#define PWR_RAM_RETENTION_30K        (30720)    // 30KB保持区域

// 供电模式
typedef enum
{
    PWR_Supply_VDD,      // VDD供电
    PWR_Supply_VBAT,     // VBAT供电
    PWR_Supply_VDD_LV,   // VDD供电+低电压模式
    PWR_Supply_VBAT_LV   // VBAT供电+低电压模式
} PWR_SupplyTypeDef;
```

## 4. 通用配置步骤

### 4.1 睡眠模式配置步骤

1. **配置唤醒源**（外部中断、定时器等）：
   ```c
   // 配置GPIO和EXTI中断
   GPIO_InitStructure.GPIO_Mode = GPIO_Mode_IPU;
   EXTI_InitStructure.EXTI_Trigger = EXTI_Trigger_Falling;
   NVIC_Init(&NVIC_InitStructure);
   ```

2. **优化功耗**：
   ```c
   // 禁用未使用的外设时钟
   // 配置未使用GPIO为下拉输入
   if(((RCC->CFGR0 & RCC_SWS)==0x04)||((RCC->CFGR0 & RCC_PLLSRC) == RCC_PLLSRC))
   {
       RCC_HSICmd(DISABLE);  // 禁用HSI
   }
   ```

3. **进入睡眠模式**：
   ```c
   __WFI();  // 或 __WFE()
   ```

4. **唤醒后恢复**：
   ```c
   RCC_HSICmd(ENABLE);  // 恢复HSI
   ```

### 4.2 停机模式配置步骤

1. **使能PWR时钟**：
   ```c
   RCC_APB1PeriphClockCmd(RCC_APB1Periph_PWR, ENABLE);
   ```

2. **配置唤醒源**：
   ```c
   // 配置外部中断、RTC、PVD等唤醒源
   ```

3. **进入停机模式**：
   ```c
   PWR_EnterSTOPMode(PWR_Regulator_LowPower, PWR_STOPEntry_WFI);
   ```

4. **唤醒后系统初始化**：
   ```c
   // 停机模式唤醒后需要重新配置系统时钟
   SystemInit();
   SystemCoreClockUpdate();
   ```

### 4.3 待机模式配置步骤

1. **使能PWR时钟和WKUP引脚**：
   ```c
   RCC_APB1PeriphClockCmd(RCC_APB1Periph_PWR, ENABLE);
   PWR_WakeUpPinCmd(ENABLE);
   ```

2. **检查唤醒标志**：
   ```c
   if(PWR_GetFlagStatus(PWR_FLAG_WU) != RESET)
   {
       printf("Wake from standby\r\n");
       PWR_ClearFlag(PWR_FLAG_WU);
   }
   ```

3. **进入待机模式**：
   ```c
   PWR_EnterSTANDBYMode();
   ```

### 4.4 RAM数据保持待机模式配置步骤

1. **定义RAM保持区域**（链接脚本）：
   ```ld
   .keep_2kram ORIGIN(RAM) (NOLOAD):{
       *(.keep_2kram.*)
       *(.keep_2kram)
   }
   .keep_30kram ORIGIN(RAM)+2048 (NOLOAD):{
       *(.keep_30kram.*)
       *(.keep_30kram)
   }
   ```

2. **代码中使用特殊属性**：
   ```c
   #define KEEP_2kRAM   __attribute__((section(".keep_2kram")))
   #define KEEP_30kRAM  __attribute__((section(".keep_30kram")))

   KEEP_2kRAM u32 retained_data[512];
   ```

3. **进入RAM保持待机模式**：
   ```c
   // VDD供电
   PWR_EnterSTANDBYMode_RAM();

   // VBAT供电
   // PWR_EnterSTANDBYMode_RAM_VBAT_EN();

   // 低电压模式
   // PWR_EnterSTANDBYMode_RAM_LV();
   // PWR_EnterSTANDBYMode_RAM_LV_VBAT_EN();
   ```

### 4.5 PVD配置步骤

1. **使能PWR时钟**：
   ```c
   RCC_APB1PeriphClockCmd(RCC_APB1Periph_PWR, ENABLE);
   ```

2. **配置PVD阈值**：
   ```c
   PWR_PVDLevelConfig(PWR_PVDLevel_2V9);  // 2.9V阈值
   ```

3. **使能PVD**：
   ```c
   PWR_PVDCmd(ENABLE);
   ```

4. **配置PVD中断**：
   ```c
   // PVD连接到EXTI Line16
   EXTI_InitStructure.EXTI_Line = EXTI_Line16;
   EXTI_InitStructure.EXTI_Trigger = EXTI_Trigger_Rising_Falling;
   EXTI_Init(&EXTI_InitStructure);

   NVIC_InitStructure.NVIC_IRQChannel = PVD_IRQn;
   NVIC_Init(&NVIC_InitStructure);
   ```

## 5. 示例工程详解

### 5.1 PWR低功耗模式示例

PWR文件夹包含9个子示例工程：

1. **Sleep_Mode** - 睡眠模式示例
   - 位置：`PWR/Sleep_Mode/User/main.c`
   - 功能：演示睡眠模式进入和外部中断唤醒

2. **Stop_Mode** - 停机模式示例
   - 位置：`PWR/Stop_Mode/User/main.c`
   - 功能：演示停机模式进入和唤醒

3. **Standby_Mode** - 待机模式示例
   - 位置：`PWR/Standby_Mode/User/main.c`
   - 功能：演示待机模式进入和WKUP引脚唤醒

4. **Standby_RAM_Mode** - 待机模式RAM数据保持（VDD供电）
   - 位置：`PWR/Standby_RAM_Mode/User/main.c`
   - 功能：演示待机模式下2K+30K RAM数据保持

5. **Standby_RAM_LV_Mode** - 待机模式RAM数据保持+低电压模式
   - 位置：`PWR/Standby_RAM_LV_Mode/User/main.c`
   - 功能：演示低电压模式下的RAM数据保持

6. **Standby_RAM_Application** - 待机模式RAM数据保持应用
   - 位置：`PWR/Standby_RAM_Application/User/main.c`
   - 功能：完整的RAM保持应用，支持VDD/VBAT供电

7. **PVD_VoltageJudger** - 电源电压检测器
   - 位置：`PWR/PVD_VoltageJudger/User/main.c`
   - 功能：演示PVD电压检测和中断处理

8. **PVD_Wakeup** - PVD唤醒停机模式
   - 位置：`PWR/PVD_Wakeup/User/main.c`
   - 功能：演示PVD中断唤醒停机模式

9. **CH32V317_LP_Mode** - CH32V317低功耗模式
   - 位置：`PWR/CH32V317_LP_Mode/User/main.c`
   - 功能：CH32V317特殊优化，包含ETH-PHY电源管理

### 5.2 功耗模式对比

| 模式 | 进入方式 | 唤醒方式 | 功耗 | 唤醒后状态 | RAM保持 |
|------|----------|----------|------|------------|---------|
| **睡眠** | `__WFI()` | 任意中断 | 最低 | 继续执行 | 是 |
| **停机** | `PWR_EnterSTOPMode` | 外部中断、PVD等 | 很低 | 需要时钟初始化 | 是 |
| **待机** | `PWR_EnterSTANDBYMode` | WKUP引脚、NRST | 最低 | 系统复位 | 否 |
| **待机+RAM** | `PWR_EnterSTANDBYMode_RAM` | WKUP引脚、NRST | 低 | 系统复位 | 是（部分） |

### 5.3 RAM保持特性

CH32V307支持在待机模式下保持部分RAM数据：
- **2K RAM区域**：地址 `0x20000000` ~ `0x20000000+2K`
- **30K RAM区域**：地址 `0x20000000+2K` ~ `0x20000000+2K+30K`
- **供电模式**：支持VDD和VBAT供电
- **电压模式**：支持正常电压和低电压模式

## 6. 常见问题

### Q1: 进入低功耗模式后无法唤醒？
A: 检查以下配置：
1. 唤醒源是否正确配置和使能
2. 中断优先级设置是否正确
3. 在停机模式下，唤醒后是否需要重新初始化时钟
4. WKUP引脚配置是否正确（上拉/下拉）

### Q2: RAM数据在待机模式后丢失？
A: 确保：
1. 使用正确的RAM保持函数（如`PWR_EnterSTANDBYMode_RAM()`）
2. 数据定义在正确的section中（`.keep_2kram`或`.keep_30kram`）
3. 链接脚本正确配置RAM保持区域
4. 供电稳定，没有掉电

### Q3: 低功耗模式下功耗仍然很高？
A: 优化建议：
1. 禁用未使用的外设时钟
2. 配置未使用GPIO为下拉输入
3. 在睡眠模式下禁用HSI时钟
4. 使用低电压模式（如果支持）
5. 关闭未使用的外设电源（如ETH-PHY）

### Q4: PVD检测不准确？
A: 可能原因：
1. PVD阈值设置不合适
2. 电源噪声干扰
3. 参考电压不稳定
4. 需要添加滤波电路

### Q5: 如何选择适合的低功耗模式？
A: 选择原则：
- **响应速度要求高**：睡眠模式（唤醒最快）
- **低功耗需求高**：停机模式（功耗较低）
- **最低功耗需求**：待机模式（功耗最低）
- **需要保存状态**：RAM保持待机模式

## 7. API参考

### 7.1 PWR控制API

| 函数名 | 功能描述 | 参数说明 |
|--------|----------|----------|
| `PWR_DeInit` | PWR模块复位 | 无 |
| `PWR_BackupAccessCmd` | 备份域访问使能 | NewState：新状态 |
| `PWR_PVDCmd` | PVD使能 | NewState：新状态 |
| `PWR_PVDLevelConfig` | 配置PVD阈值 | PWR_PVDLevel：电压阈值 |
| `PWR_WakeUpPinCmd` | WKUP引脚使能 | NewState：新状态 |

### 7.2 低功耗模式API

| 函数名 | 功能描述 | 参数说明 |
|--------|----------|----------|
| `PWR_EnterSTOPMode` | 进入停机模式 | PWR_Regulator：调节器状态，PWR_STOPEntry：进入方式 |
| `PWR_EnterSTANDBYMode` | 进入待机模式 | 无 |
| `PWR_EnterSTANDBYMode_RAM` | 进入RAM保持待机模式（VDD） | 无 |
| `PWR_EnterSTANDBYMode_RAM_LV` | 进入RAM保持待机模式（VDD+LV） | 无 |
| `PWR_EnterSTANDBYMode_RAM_VBAT_EN` | 进入RAM保持待机模式（VBAT） | 无 |
| `PWR_EnterSTANDBYMode_RAM_LV_VBAT_EN` | 进入RAM保持待机模式（VBAT+LV） | 无 |
| `PWR_EnterSTOPMode_RAM_LV` | 进入停机模式（RAM保持+LV） | PWR_Regulator：调节器状态，PWR_STOPEntry：进入方式 |

### 7.3 状态标志API

| 函数名 | 功能描述 | 参数说明 |
|--------|----------|----------|
| `PWR_GetFlagStatus` | 获取标志状态 | PWR_FLAG：标志位 |
| `PWR_ClearFlag` | 清除标志 | PWR_FLAG：标志位 |

### 7.4 系统控制API

| 函数名 | 功能描述 | 参数说明 |
|--------|----------|----------|
| `__WFI` | 等待中断 | 无 |
| `__WFE` | 等待事件 | 无 |
| `__SEV` | 发送事件 | 无 |

## 8. 注意事项

1. **时钟管理**：进入低功耗模式前合理管理时钟，退出后恢复
2. **GPIO状态**：配置未使用GPIO为模拟输入或下拉输入以减少功耗
3. **外设状态**：保存和恢复必要的外设状态
4. **唤醒时间**：考虑唤醒时间和系统响应要求
5. **电池寿命**：在电池供电应用中合理使用低功耗模式
6. **数据完整性**：确保关键数据在模式切换时不会丢失

## 9. 性能优化建议

1. **功耗分析**：使用电流表测量各模式下的实际功耗
2. **唤醒优化**：优化唤醒源配置，减少误唤醒
3. **时钟优化**：根据应用需求选择最低系统时钟频率
4. **外设管理**：不使用时关闭外设时钟和电源
5. **代码优化**：减少中断频率和CPU活跃时间
6. **电压优化**：在满足性能前提下使用最低工作电压

## 10. 典型应用场景

### 10.1 电池供电设备

```c
// 电池供电传感器节点
void BatteryPoweredSensorNode(void)
{
    // 初始化传感器和无线模块
    Sensor_Init();
    Wireless_Init();

    while(1)
    {
        // 采集数据
        SensorData data = Sensor_ReadData();

        // 发送数据
        Wireless_SendData(&data);

        // 进入停机模式，等待定时唤醒
        RCC_APB1PeriphClockCmd(RCC_APB1Periph_PWR, ENABLE);

        // 配置RTC唤醒
        RTC_SetWakeUpCounter(3276);  // 10秒后唤醒（假设RTC时钟32.768kHz）
        RTC_WakeUpCmd(ENABLE);

        // 进入停机模式
        PWR_EnterSTOPMode(PWR_Regulator_LowPower, PWR_STOPEntry_WFI);

        // 唤醒后重新初始化系统
        SystemInit();
    }
}
```

### 10.2 事件触发设备

```c
// 事件触发记录仪
void EventTriggeredLogger(void)
{
    // 初始化存储和传感器
    Storage_Init();
    MotionSensor_Init();

    // 配置运动传感器中断
    EXTI_ConfigureInterrupt(MOTION_SENSOR_PIN, EXTI_Trigger_Rising);

    printf("Entering stop mode, waiting for motion...\r\n");

    // 进入停机模式，等待运动触发
    RCC_APB1PeriphClockCmd(RCC_APB1Periph_PWR, ENABLE);
    PWR_EnterSTOPMode(PWR_Regulator_LowPower, PWR_STOPEntry_WFI);

    // 运动触发唤醒
    SystemInit();
    printf("Motion detected! Starting recording...\r\n");

    // 记录数据
    RecordData();

    // 完成后再次进入停机模式
    // ...
}
```

### 10.3 电源监控系统

```c
// 电源监控和保护系统
void PowerMonitoringSystem(void)
{
    // 初始化PVD
    RCC_APB1PeriphClockCmd(RCC_APB1Periph_PWR, ENABLE);
    PWR_PVDLevelConfig(PWR_PVDLevel_2V7);  // 2.7V阈值
    PWR_PVDCmd(ENABLE);

    // 配置PVD中断
    EXTI_InitStructure.EXTI_Line = EXTI_Line16;
    EXTI_InitStructure.EXTI_Trigger = EXTI_Trigger_Falling;  // 电压下降触发
    EXTI_Init(&EXTI_InitStructure);

    NVIC_InitStructure.NVIC_IRQChannel = PVD_IRQn;
    NVIC_Init(&NVIC_InitStructure);

    while(1)
    {
        // 正常系统操作
        ProcessNormalOperations();

        // 检查系统状态
        if(CheckSystemStatus() == POWER_CRITICAL)
        {
            // 电源临界，进入安全状态
            EnterSafeState();

            // 进入待机模式等待外部复位
            PWR_EnterSTANDBYMode();
        }

        Delay_Ms(100);
    }
}
```

## 11. CH32V317特殊注意事项

在`CH32V317_LP_Mode`示例中，需要注意ETH-PHY电源管理：

```c
// 进入低功耗模式前关闭ETH-PHY电源
void EnterLowPowerMode(void)
{
    // 1. 关闭ETH-PHY电源
    ETH_PHY_PowerDown();

    // 2. 配置未使用GPIO为下拉输入
    ConfigureUnusedGPIOs();

    // 3. 进入停机模式
    RCC_APB1PeriphClockCmd(RCC_APB1Periph_PWR, ENABLE);
    PWR_EnterSTOPMode(PWR_Regulator_LowPower, PWR_STOPEntry_WFI);

    // 4. 唤醒后恢复ETH-PHY
    SystemInit();
    ETH_PHY_PowerUp();
}

// ETH-PHY断电函数
void ETH_PHY_PowerDown()
{
    uint16_t regval;
    ETH_Configuration();
    Delay_Ms(100);

    // 使能PHY断电模式
    regval = ETH_ReadPHYRegister(gPHYAddress, 0);
    regval |= 0x1 << 11;  // 设置断电位
    ETH_WritePHYRegister(gPHYAddress, 0, regval);
}
```

## 12. 总结

CH32V307的PWR模块提供了完整的低功耗解决方案，支持多种功耗模式和灵活的唤醒机制。通过合理使用睡眠、停机、待机模式以及RAM数据保持功能，可以显著降低系统功耗，延长电池寿命。开发时需要注意时钟管理、GPIO状态、唤醒源配置等关键环节，确保低功耗模式稳定可靠工作。PWR功能特别适用于电池供电设备、物联网节点、便携式设备等对功耗要求高的应用场景。
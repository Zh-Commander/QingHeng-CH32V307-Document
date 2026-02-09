# CH32V307 RCC复位和时钟控制使用指南

## 1. 外设概述

RCC（Reset and Clock Control，复位和时钟控制）是CH32V307的时钟管理系统，负责系统时钟生成、分配和监控。主要特性包括：

- **多种时钟源**：HSI（8MHz内部高速）、HSE（外部高速3-25MHz）、LSI（~40kHz内部低速）、LSE（32.768kHz外部低速）、PLL（锁相环倍频）
- **灵活时钟分配**：可配置SYSCLK、HCLK、PCLK1、PCLK2、ADCCLK等时钟域
- **时钟安全系统**：支持CSS（Clock Security System）检测HSE故障
- **时钟输出**：支持MCO（微控制器时钟输出）功能
- **时钟校准**：支持HSI和LSI时钟频率校准
- **外设时钟管理**：灵活使能/禁用各外设时钟
- **低功耗时钟管理**：支持低功耗模式下的时钟配置

## 2. 关键代码片段

### 2.1 系统时钟配置（从system_ch32v30x.c提取）

```c
// 典型的系统时钟配置流程（HSE作为PLL源，输出72MHz）
void SystemInit(void)
{
    // 1. 使能HSE
    RCC->CTLR |= RCC_HSEON;
    while((RCC->CTLR & RCC_HSERDY) == 0);  // 等待HSE就绪

    // 2. 配置AHB、APB1、APB2预分频器
    RCC->CFGR0 |= RCC_HPRE_DIV1;     // HCLK = SYSCLK
    RCC->CFGR0 |= RCC_PPRE2_DIV1;    // PCLK2 = HCLK
    RCC->CFGR0 |= RCC_PPRE1_DIV2;    // PCLK1 = HCLK/2

    // 3. 配置PLL（HSE作为源，9倍频）
    RCC->CFGR0 &= ~(RCC_PLLSRC | RCC_PLLXTPRE | RCC_PLLMULL);
    RCC->CFGR0 |= RCC_PLLSRC_HSE | RCC_PLLXTPRE_HSE | RCC_PLLMULL9;

    // 4. 使能PLL
    RCC->CTLR |= RCC_PLLON;
    while((RCC->CTLR & RCC_PLLRDY) == 0);  // 等待PLL就绪

    // 5. 切换系统时钟源到PLL
    RCC->CFGR0 &= ~RCC_SW;
    RCC->CFGR0 |= RCC_SW_PLL;
    while((RCC->CFGR0 & RCC_SWS) != 0x08);  // 等待切换完成

    // 6. 更新SystemCoreClock变量
    SystemCoreClock = 72000000;  // 72MHz
}
```

### 2.2 获取系统时钟频率（Get_CLK示例）

```c
int main(void)
{
    SystemCoreClockUpdate();
    Delay_Init();
    USART_Printf_Init(115200);

    // 方法1：使用SystemCoreClock变量
    printf("SystemClk:%d\r\n", SystemCoreClock);

    // 方法2：使用RCC_GetClocksFreq()函数
    RCC_ClocksTypeDef RCC_ClocksStatus = {0};
    RCC_GetClocksFreq(&RCC_ClocksStatus);

    printf("SYSCLK_Frequency-%d\r\n", RCC_ClocksStatus.SYSCLK_Frequency);
    printf("HCLK_Frequency-%d\r\n", RCC_ClocksStatus.HCLK_Frequency);
    printf("PCLK1_Frequency-%d\r\n", RCC_ClocksStatus.PCLK1_Frequency);
    printf("PCLK2_Frequency-%d\r\n", RCC_ClocksStatus.PCLK2_Frequency);
    printf("ADCCLK_Frequency-%d\r\n", RCC_ClocksStatus.ADCCLK_Frequency);

    while(1)
    {
        Delay_Ms(1000);
        printf("System running...\r\n");
    }
}
```

### 2.3 HSI时钟校准（HSI_Calibration示例）

```c
// 使用HSE作为参考校准HSI
void HSI_Calibration_Test(void)
{
    uint32_t Meas_Freq = 0;
    uint8_t last_Adjust_val = 0;
    uint32_t rtc_cnt = 0;

    // 1. 使能HSE
    RCC_HSEConfig(RCC_HSE_ON);
    while(RCC_GetFlagStatus(RCC_FLAG_HSERDY) == RESET);

    // 2. 配置RTC使用HSE/128作为时钟
    RCC_RTCCLKConfig(RCC_RTCCLKSource_HSE_Div128);
    RCC_RTCCLKCmd(ENABLE);
    RTC_WaitForSynchro();

    // 3. 测量HSI频率
    for(last_Adjust_val = 0; last_Adjust_val < 16; last_Adjust_val++)
    {
        // 调整HSI校准值
        RCC_AdjustHSICalibrationValue(last_Adjust_val);

        // 等待RTC计数
        Delay_Ms(100);
        rtc_cnt = RTC_GetCounter();

        // 计算实际频率
        Meas_Freq = (uint32_t)((float)HSI_VALUE * (float)(HSE_VALUE / 128 / 10) / (float)rtc_cnt);

        printf("Adjust_val:%d, Meas_Freq:%d\r\n", last_Adjust_val, Meas_Freq);
    }
}
```

### 2.4 LSI时钟校准（LSI_Calibration示例）

```c
// 使用TIM5输入捕获测量LSI频率
uint32_t GetActualLSIFreq(void)
{
    uint32_t count = 0;
    TIM_ICInitTypeDef TIM_ICInitStructure = {0};

    // 1. 使能LSI
    RCC_LSICmd(ENABLE);
    while(RCC_GetFlagStatus(RCC_FLAG_LSIRDY) == RESET);

    // 2. 配置TIM5输入捕获
    RCC_APB1PeriphClockCmd(RCC_APB1Periph_TIM5, ENABLE);
    TIM_ICInitStructure.TIM_Channel = TIM_Channel_1;
    TIM_ICInitStructure.TIM_ICPolarity = TIM_ICPolarity_Rising;
    TIM_ICInitStructure.TIM_ICSelection = TIM_ICSelection_DirectTI;
    TIM_ICInitStructure.TIM_ICPrescaler = TIM_ICPSC_DIV1;
    TIM_ICInitStructure.TIM_ICFilter = 0x0;
    TIM_ICInit(TIM5, &TIM_ICInitStructure);

    // 3. 开始测量
    TIM_Cmd(TIM5, ENABLE);
    while(TIM_GetFlagStatus(TIM5, TIM_FLAG_CC1) == RESET);
    count = TIM_GetCapture1(TIM5);
    TIM_ClearFlag(TIM5, TIM_FLAG_CC1);

    // 4. 计算LSI频率
    return (uint32_t)((float)SystemCoreClock / (float)count);
}

int main(void)
{
    uint32_t LSI_Freq = 0;

    SystemCoreClockUpdate();
    Delay_Init();
    USART_Printf_Init(115200);

    // 获取LSI实际频率
    LSI_Freq = GetActualLSIFreq();
    printf("LSI_Freq: %d\r\n", LSI_Freq);

    // 设置RTC和WWDG预分频器
    RTC_SetPrescaler(LSI_Freq);
    WWDG_SetPrescaler(LSI_Freq);

    while(1);
}
```

### 2.5 MCO时钟输出（MCO示例）

```c
// 配置PA8作为MCO输出引脚
void MCO_Output_Config(void)
{
    GPIO_InitTypeDef GPIO_InitStructure = {0};

    // 1. 使能GPIOA时钟
    RCC_APB2PeriphClockCmd(RCC_APB2Periph_GPIOA, ENABLE);

    // 2. 配置PA8为复用推挽输出
    GPIO_InitStructure.GPIO_Pin = GPIO_Pin_8;
    GPIO_InitStructure.GPIO_Mode = GPIO_Mode_AF_PP;
    GPIO_InitStructure.GPIO_Speed = GPIO_Speed_50MHz;
    GPIO_Init(GPIOA, &GPIO_InitStructure);

    // 3. 选择MCO时钟源
    RCC_MCOConfig(RCC_MCO_SYSCLK);  // 输出系统时钟
    // RCC_MCOConfig(RCC_MCO_HSI);   // 输出HSI时钟
    // RCC_MCOConfig(RCC_MCO_HSE);   // 输出HSE时钟
    // RCC_MCOConfig(RCC_MCO_PLLCLK_Div2);  // 输出PLL/2
}

int main(void)
{
    SystemCoreClockUpdate();
    Delay_Init();
    USART_Printf_Init(115200);

    printf("MCO Test\r\n");
    printf("SystemClk:%d\r\n", SystemCoreClock);

    // 配置MCO输出
    MCO_Output_Config();

    printf("MCO output enabled on PA8\r\n");

    while(1);
}
```

### 2.6 HSE频率检测（HSE_CLK示例）

```c
// 使用TIM1测量HSE频率
void HSE_Frequency_Detection(void)
{
    uint32_t count = 0;
    float freq = 0;

    // 1. 使能HSE
    RCC_HSEConfig(RCC_HSE_ON);
    while(RCC_GetFlagStatus(RCC_FLAG_HSERDY) == RESET);

    // 2. 配置TIM1输入捕获（通道1）
    RCC_APB2PeriphClockCmd(RCC_APB2Periph_TIM1, ENABLE);
    TIM_ICInitTypeDef TIM_ICInitStructure = {0};
    TIM_ICInitStructure.TIM_Channel = TIM_Channel_1;
    TIM_ICInitStructure.TIM_ICPolarity = TIM_ICPolarity_Rising;
    TIM_ICInitStructure.TIM_ICSelection = TIM_ICSelection_DirectTI;
    TIM_ICInitStructure.TIM_ICPrescaler = TIM_ICPSC_DIV1;
    TIM_ICInitStructure.TIM_ICFilter = 0x0;
    TIM_ICInit(TIM1, &TIM_ICInitStructure);

    // 3. 测量频率
    TIM_Cmd(TIM1, ENABLE);
    Delay_Ms(100);  // 等待捕获
    count = TIM_GetCapture1(TIM1);

    // 4. 计算频率
    freq = (float)SystemCoreClock / (float)count;
    printf("HSE Frequency: %.2f MHz\r\n", freq / 1000000);
}
```

## 3. 配置数据结构

### 3.1 RCC_ClocksTypeDef时钟频率结构体

```c
typedef struct
{
  uint32_t SYSCLK_Frequency;  // SYSCLK时钟频率（Hz）
  uint32_t HCLK_Frequency;    // HCLK时钟频率（Hz）
  uint32_t PCLK1_Frequency;   // PCLK1时钟频率（Hz）
  uint32_t PCLK2_Frequency;   // PCLK2时钟频率（Hz）
  uint32_t ADCCLK_Frequency;  // ADCCLK时钟频率（Hz）
} RCC_ClocksTypeDef;
```

### 3.2 时钟源选择定义

```c
// 系统时钟源选择
#define RCC_SYSCLKSource_HSI        ((uint32_t)0x00000000)
#define RCC_SYSCLKSource_HSE        ((uint32_t)0x00000001)
#define RCC_SYSCLKSource_PLLCLK     ((uint32_t)0x00000002)

// PLL时钟源选择
#define RCC_PLLSource_HSI_Div2      ((uint32_t)0x00000000)
#define RCC_PLLSource_HSE_Div1      ((uint32_t)0x00010000)
#define RCC_PLLSource_HSE_Div2      ((uint32_t)0x00030000)

// MCO时钟源选择
#define RCC_MCO_NoClock             ((uint8_t)0x00)
#define RCC_MCO_SYSCLK              ((uint8_t)0x04)
#define RCC_MCO_HSI                 ((uint8_t)0x05)
#define RCC_MCO_HSE                 ((uint8_t)0x06)
#define RCC_MCO_PLLCLK_Div2         ((uint8_t)0x07)
```

### 3.3 预分频器定义

```c
// AHB预分频器（HCLK）
#define RCC_SYSCLK_Div1             ((uint32_t)0x00000000)
#define RCC_SYSCLK_Div2             ((uint32_t)0x00000080)
#define RCC_SYSCLK_Div4             ((uint32_t)0x00000090)
#define RCC_SYSCLK_Div8             ((uint32_t)0x000000A0)
#define RCC_SYSCLK_Div16            ((uint32_t)0x000000B0)
#define RCC_SYSCLK_Div64            ((uint32_t)0x000000C0)
#define RCC_SYSCLK_Div128           ((uint32_t)0x000000D0)
#define RCC_SYSCLK_Div256           ((uint32_t)0x000000E0)
#define RCC_SYSCLK_Div512           ((uint32_t)0x000000F0)

// APB1预分频器（PCLK1）
#define RCC_HCLK_Div1               ((uint32_t)0x00000000)
#define RCC_HCLK_Div2               ((uint32_t)0x00000400)
#define RCC_HCLK_Div4               ((uint32_t)0x00000500)
#define RCC_HCLK_Div8               ((uint32_t)0x00000600)
#define RCC_HCLK_Div16              ((uint32_t)0x00000700)

// APB2预分频器（PCLK2）
#define RCC_HCLK_Div1               ((uint32_t)0x00000000)
#define RCC_HCLK_Div2               ((uint32_t)0x00002000)
#define RCC_HCLK_Div4               ((uint32_t)0x00002800)
#define RCC_HCLK_Div8               ((uint32_t)0x00003000)
#define RCC_HCLK_Div16              ((uint32_t)0x00003800)
```

### 3.4 PLL倍频系数定义

```c
#define RCC_PLLMul_2                ((uint32_t)0x00000000)
#define RCC_PLLMul_3                ((uint32_t)0x00040000)
#define RCC_PLLMul_4                ((uint32_t)0x00080000)
#define RCC_PLLMul_5                ((uint32_t)0x000C0000)
#define RCC_PLLMul_6                ((uint32_t)0x00100000)
#define RCC_PLLMul_7                ((uint32_t)0x00140000)
#define RCC_PLLMul_8                ((uint32_t)0x00180000)
#define RCC_PLLMul_9                ((uint32_t)0x001C0000)
#define RCC_PLLMul_10               ((uint32_t)0x00200000)
#define RCC_PLLMul_11               ((uint32_t)0x00240000)
#define RCC_PLLMul_12               ((uint32_t)0x00280000)
#define RCC_PLLMul_13               ((uint32_t)0x002C0000)
#define RCC_PLLMul_14               ((uint32_t)0x00300000)
#define RCC_PLLMul_15               ((uint32_t)0x00340000)
#define RCC_PLLMul_16               ((uint32_t)0x00380000)
```

## 4. 通用配置步骤

### 4.1 系统时钟初始化步骤

1. **使能目标时钟源**：
   ```c
   // HSE示例
   RCC_HSEConfig(RCC_HSE_ON);
   while(RCC_GetFlagStatus(RCC_FLAG_HSERDY) == RESET);

   // HSI示例
   RCC_HSICmd(ENABLE);
   while(RCC_GetFlagStatus(RCC_FLAG_HSIRDY) == RESET);
   ```

2. **配置预分频器**：
   ```c
   RCC_HCLKConfig(RCC_SYSCLK_Div1);    // HCLK = SYSCLK
   RCC_PCLK1Config(RCC_HCLK_Div2);     // PCLK1 = HCLK/2
   RCC_PCLK2Config(RCC_HCLK_Div1);     // PCLK2 = HCLK
   RCC_ADCCLKConfig(RCC_PCLK2_Div6);   // ADCCLK = PCLK2/6
   ```

3. **配置PLL（如果需要）**：
   ```c
   RCC_PLLConfig(RCC_PLLSource_HSE_Div1, RCC_PLLMul_9);
   RCC_PLLCmd(ENABLE);
   while(RCC_GetFlagStatus(RCC_FLAG_PLLRDY) == RESET);
   ```

4. **切换系统时钟源**：
   ```c
   RCC_SYSCLKConfig(RCC_SYSCLKSource_PLLCLK);
   while(RCC_GetSYSCLKSource() != 0x08);  // 等待切换完成
   ```

5. **更新系统时钟变量**：
   ```c
   SystemCoreClockUpdate();
   ```

### 4.2 外设时钟使能步骤

```c
// APB2外设（高速外设）
RCC_APB2PeriphClockCmd(RCC_APB2Periph_GPIOA, ENABLE);
RCC_APB2PeriphClockCmd(RCC_APB2Periph_USART1, ENABLE);
RCC_APB2PeriphClockCmd(RCC_APB2Periph_ADC1, ENABLE);

// APB1外设（低速外设）
RCC_APB1PeriphClockCmd(RCC_APB1Periph_TIM2, ENABLE);
RCC_APB1PeriphClockCmd(RCC_APB1Periph_USART2, ENABLE);
RCC_APB1PeriphClockCmd(RCC_APB1Periph_I2C1, ENABLE);

// AHB外设
RCC_AHBPeriphClockCmd(RCC_AHBPeriph_DMA1, ENABLE);
RCC_AHBPeriphClockCmd(RCC_AHBPeriph_CRC, ENABLE);
```

### 4.3 时钟校准步骤

1. **HSI校准**：
   ```c
   // 使用HSE作为参考校准HSI
   RCC_AdjustHSICalibrationValue(calibration_value);  // 0-15

   // 或者自动校准
   RCC_HSICalibrationValueConfig(HSI_CalibrationValue);
   ```

2. **LSI校准**：
   ```c
   // 使能LSI
   RCC_LSICmd(ENABLE);
   while(RCC_GetFlagStatus(RCC_FLAG_LSIRDY) == RESET);

   // 测量LSI频率（使用TIM输入捕获等方法）
   uint32_t actual_lsi_freq = Measure_LSI_Frequency();

   // 设置RTC和WWDG预分频器
   RTC_SetPrescaler(actual_lsi_freq);
   WWDG_SetPrescaler(actual_lsi_freq);
   ```

### 4.4 MCO输出配置步骤

1. **配置GPIO引脚**：
   ```c
   // PA8作为MCO输出
   RCC_APB2PeriphClockCmd(RCC_APB2Periph_GPIOA, ENABLE);
   GPIO_InitStructure.GPIO_Pin = GPIO_Pin_8;
   GPIO_InitStructure.GPIO_Mode = GPIO_Mode_AF_PP;
   GPIO_InitStructure.GPIO_Speed = GPIO_Speed_50MHz;
   GPIO_Init(GPIOA, &GPIO_InitStructure);
   ```

2. **选择MCO时钟源**：
   ```c
   // 输出系统时钟
   RCC_MCOConfig(RCC_MCO_SYSCLK);

   // 输出HSI时钟
   // RCC_MCOConfig(RCC_MCO_HSI);

   // 输出HSE时钟
   // RCC_MCOConfig(RCC_MCO_HSE);

   // 输出PLL/2
   // RCC_MCOConfig(RCC_MCO_PLLCLK_Div2);
   ```

## 5. 示例工程详解

### 5.1 RCC示例工程概览

RCC文件夹包含7个子示例工程：

1. **Get_CLK** - 获取系统时钟频率
   - 位置：`RCC/Get_CLK/User/main.c`
   - 功能：演示获取SYSCLK、HCLK、PCLK1、PCLK2、ADCCLK频率的方法
   - 使用函数：`SystemCoreClockUpdate()`、`RCC_GetClocksFreq()`

2. **HSE_CLK** - HSE外部高速时钟配置和频率检测
   - 位置：`RCC/HSE_CLK/User/main.c`
   - 功能：演示HSE使能、状态检测和频率测量
   - 使用TIM1输入捕获测量HSE频率

3. **HSI_Calibration** - HSI内部高速时钟校准
   - 位置：`RCC/HSI_Calibration/User/main.c`
   - 功能：使用HSE作为参考校准HSI时钟
   - 演示RCC_AdjustHSICalibrationValue()函数使用

4. **HSI_PLL_Source** - 使用HSI或HSI/2作为PLL时钟源
   - 位置：`RCC/HSI_PLL_Source/User/main.c`
   - 功能：演示HSI作为PLL时钟源的配置
   - 包含完整的系统时钟配置流程

5. **LSI_Calibration** - LSI内部低速时钟校准
   - 位置：`RCC/LSI_Calibration/User/main.c`
   - 功能：使用TIM5输入捕获测量LSI频率
   - 校准RTC和WWDG预分频器

6. **MCO** - 微控制器时钟输出配置
   - 位置：`RCC/MCO/User/main.c`
   - 功能：演示PA8引脚作为MCO输出
   - 支持多种时钟源输出

### 5.2 关键功能对比

| 示例工程 | 主要功能 | 关键API函数 | 适用场景 |
|----------|----------|-------------|----------|
| **Get_CLK** | 时钟频率获取 | `RCC_GetClocksFreq()` | 系统调试、时钟监控 |
| **HSE_CLK** | HSE频率检测 | `RCC_HSEConfig()` | HSE稳定性测试 |
| **HSI_Calibration** | HSI时钟校准 | `RCC_AdjustHSICalibrationValue()` | 精度要求高的应用 |
| **HSI_PLL_Source** | PLL时钟源配置 | `RCC_PLLConfig()` | 无外部晶振的系统 |
| **LSI_Calibration** | LSI时钟校准 | `RCC_LSICmd()` | 低功耗定时应用 |
| **MCO** | 时钟信号输出 | `RCC_MCOConfig()` | 时钟信号测试、外部同步 |

### 5.3 时钟配置模式总结

CH32V307支持多种时钟配置模式：

1. **HSI模式**：内部8MHz时钟，无需外部元件，精度较低
2. **HSE模式**：外部晶振，精度高，频率范围3-25MHz
3. **PLL模式**：倍频时钟源，可获得更高系统频率
4. **混合模式**：如HSI作为PLL源，兼顾成本和性能

## 6. 常见问题

### Q1: 系统时钟配置后程序不运行？
A: 检查以下配置：
1. 时钟源是否正确使能（如HSE晶振是否起振）
2. PLL配置参数是否在芯片支持范围内
3. 时钟切换后是否等待切换完成
4. Flash等待状态是否根据系统频率正确配置

### Q2: HSE晶振不起振？
A: 可能原因：
1. 晶振负载电容不匹配
2. 晶振频率超出范围（3-25MHz）
3. 外部电路问题
4. 芯片损坏

### Q3: 如何选择合适的PLL倍频系数？
A: 选择原则：
1. 参考芯片数据手册的最大频率限制
2. 考虑目标外设的时钟需求（如USB需要48MHz）
3. 考虑Flash等待状态对性能的影响
4. 平衡性能和功耗

### Q4: 时钟频率测量不准确？
A: 优化建议：
1. 使用更高精度的参考时钟
2. 增加测量时间以减少误差
3. 多次测量取平均值
4. 确保测量期间系统时钟稳定

### Q5: 低功耗模式下时钟如何配置？
A: 配置要点：
1. 进入低功耗前降低系统频率
2. 禁用不必要的外设时钟
3. 选择合适的唤醒时钟源
4. 退出低功耗后恢复时钟配置

## 7. API参考

### 7.1 时钟配置API

| 函数名 | 功能描述 | 参数说明 |
|--------|----------|----------|
| `RCC_DeInit` | RCC模块复位 | 无 |
| `RCC_HSEConfig` | 配置HSE时钟 | RCC_HSE：HSE状态 |
| `RCC_HSICmd` | 使能/禁用HSI | NewState：新状态 |
| `RCC_PLLConfig` | 配置PLL | RCC_PLLSource：PLL源，RCC_PLLMul：倍频系数 |
| `RCC_PLLCmd` | 使能/禁用PLL | NewState：新状态 |
| `RCC_SYSCLKConfig` | 配置系统时钟源 | RCC_SYSCLKSource：时钟源 |
| `RCC_GetSYSCLKSource` | 获取系统时钟源 | 返回当前时钟源 |
| `RCC_HCLKConfig` | 配置HCLK预分频器 | RCC_SYSCLK：预分频值 |
| `RCC_PCLK1Config` | 配置PCLK1预分频器 | RCC_HCLK：预分频值 |
| `RCC_PCLK2Config` | 配置PCLK2预分频器 | RCC_HCLK：预分频值 |
| `RCC_ADCCLKConfig` | 配置ADCCLK预分频器 | RCC_PCLK2：预分频值 |

### 7.2 时钟状态API

| 函数名 | 功能描述 | 参数说明 |
|--------|----------|----------|
| `RCC_GetClocksFreq` | 获取各时钟域频率 | RCC_Clocks：时钟频率结构体指针 |
| `RCC_GetFlagStatus` | 获取标志状态 | RCC_FLAG：标志位 |
| `RCC_ClearFlag` | 清除标志 | RCC_FLAG：标志位 |
| `RCC_GetITStatus` | 获取中断状态 | RCC_IT：中断标志 |
| `RCC_ClearITPendingBit` | 清除中断挂起位 | RCC_IT：中断标志 |

### 7.3 时钟校准API

| 函数名 | 功能描述 | 参数说明 |
|--------|----------|----------|
| `RCC_AdjustHSICalibrationValue` | 调整HSI校准值 | HSICalibrationValue：校准值（0-15） |
| `RCC_HSICalibrationValueConfig` | 配置HSI校准值 | HSI_CalibrationValue：校准值 |
| `RCC_LSICmd` | 使能/禁用LSI | NewState：新状态 |

### 7.4 外设时钟控制API

| 函数名 | 功能描述 | 参数说明 |
|--------|----------|----------|
| `RCC_AHBPeriphClockCmd` | AHB外设时钟控制 | RCC_AHBPeriph：外设，NewState：状态 |
| `RCC_APB2PeriphClockCmd` | APB2外设时钟控制 | RCC_APB2Periph：外设，NewState：状态 |
| `RCC_APB1PeriphClockCmd` | APB1外设时钟控制 | RCC_APB1Periph：外设，NewState：状态 |
| `RCC_AHBPeriphResetCmd` | AHB外设复位控制 | RCC_AHBPeriph：外设，NewState：状态 |
| `RCC_APB2PeriphResetCmd` | APB2外设复位控制 | RCC_APB2Periph：外设，NewState：状态 |
| `RCC_APB1PeriphResetCmd` | APB1外设复位控制 | RCC_APB1Periph：外设，NewState：状态 |

### 7.5 特殊功能API

| 函数名 | 功能描述 | 参数说明 |
|--------|----------|----------|
| `RCC_MCOConfig` | 配置MCO输出 | RCC_MCO：时钟源选择 |
| `RCC_RTCCLKConfig` | 配置RTC时钟源 | RCC_RTCCLKSource：时钟源 |
| `RCC_RTCCLKCmd` | 使能/禁用RTC时钟 | NewState：新状态 |
| `RCC_BackupResetCmd` | 备份域复位控制 | NewState：新状态 |

## 8. 注意事项

1. **时钟切换顺序**：切换系统时钟源时需要等待切换完成
2. **PLL锁定时间**：使能PLL后需要等待PLL锁定
3. **外设时钟依赖**：使用外设前必须使能对应的时钟
4. **Flash等待状态**：系统频率超过一定值需要配置Flash等待状态
5. **时钟安全系统**：启用CSS后HSE故障会触发中断并切换到HSI
6. **低功耗模式**：不同低功耗模式下时钟行为不同
7. **校准精度**：HSI/LSI校准值受温度和电压影响
8. **MCO负载能力**：MCO输出驱动能力有限，需注意负载匹配

## 9. 性能优化建议

1. **动态频率调整**：根据任务需求动态调整系统频率
2. **外设时钟管理**：不使用时关闭外设时钟以降低功耗
3. **时钟门控**：合理使用时钟门控技术
4. **校准策略**：根据环境温度变化动态校准内部时钟
5. **时钟监控**：启用时钟安全系统监控关键时钟源
6. **电源管理**：时钟频率与电源电压匹配以获得最佳能效比

## 10. 典型应用场景

### 10.1 高性能应用（需要高时钟频率）

```c
// 配置72MHz系统时钟（HSE 8MHz -> PLL 9倍频）
void HighPerformance_ClockConfig(void)
{
    // 使能HSE
    RCC_HSEConfig(RCC_HSE_ON);
    while(RCC_GetFlagStatus(RCC_FLAG_HSERDY) == RESET);

    // 配置预分频器
    RCC_HCLKConfig(RCC_SYSCLK_Div1);    // HCLK = 72MHz
    RCC_PCLK1Config(RCC_HCLK_Div2);     // PCLK1 = 36MHz
    RCC_PCLK2Config(RCC_HCLK_Div1);     // PCLK2 = 72MHz
    RCC_ADCCLKConfig(RCC_PCLK2_Div6);   // ADCCLK = 12MHz

    // 配置PLL
    RCC_PLLConfig(RCC_PLLSource_HSE_Div1, RCC_PLLMul_9);
    RCC_PLLCmd(ENABLE);
    while(RCC_GetFlagStatus(RCC_FLAG_PLLRDY) == RESET);

    // 切换系统时钟
    RCC_SYSCLKConfig(RCC_SYSCLKSource_PLLCLK);
    while(RCC_GetSYSCLKSource() != 0x08);

    // 配置Flash等待状态（72MHz需要2个等待状态）
    FLASH_SetLatency(FLASH_Latency_2);

    SystemCoreClockUpdate();
}
```

### 10.2 低功耗应用（需要灵活时钟管理）

```c
// 低功耗时钟管理
void LowPower_ClockManagement(void)
{
    // 正常模式：使用HSI 8MHz
    RCC_HSICmd(ENABLE);
    while(RCC_GetFlagStatus(RCC_FLAG_HSIRDY) == RESET);
    RCC_SYSCLKConfig(RCC_SYSCLKSource_HSI);
    SystemCoreClockUpdate();

    // 进入低功耗前
    void EnterLowPowerMode(void)
    {
        // 降低系统频率
        RCC_HCLKConfig(RCC_SYSCLK_Div8);  // HCLK = 1MHz

        // 禁用不必要的外设时钟
        RCC_APB2PeriphClockCmd(RCC_APB2Periph_GPIOB, DISABLE);
        RCC_APB2PeriphClockCmd(RCC_APB2Periph_GPIOC, DISABLE);

        // 进入睡眠模式
        __WFI();

        // 唤醒后恢复时钟
        RCC_HCLKConfig(RCC_SYSCLK_Div1);  // HCLK = 8MHz
        SystemCoreClockUpdate();
    }
}
```

### 10.3 高精度定时应用（需要校准时钟）

```c
// 高精度时钟校准系统
void HighPrecision_ClockSystem(void)
{
    uint32_t hsi_cal_value = 0;
    uint32_t lsi_cal_value = 0;

    // 1. 使用HSE校准HSI
    RCC_HSEConfig(RCC_HSE_ON);
    while(RCC_GetFlagStatus(RCC_FLAG_HSERDY) == RESET);

    // 测量并校准HSI
    hsi_cal_value = Measure_HSI_Frequency();
    RCC_AdjustHSICalibrationValue(hsi_cal_value);

    // 2. 校准LSI用于RTC
    RCC_LSICmd(ENABLE);
    while(RCC_GetFlagStatus(RCC_FLAG_LSIRDY) == RESET);

    lsi_cal_value = Measure_LSI_Frequency();
    RTC_SetPrescaler(lsi_cal_value);

    // 3. 配置系统时钟（使用校准后的HSI）
    RCC_PLLConfig(RCC_PLLSource_HSI_Div2, RCC_PLLMul_16);  // HSI/2=4MHz, PLL=64MHz
    RCC_PLLCmd(ENABLE);
    while(RCC_GetFlagStatus(RCC_FLAG_PLLRDY) == RESET);

    RCC_SYSCLKConfig(RCC_SYSCLKSource_PLLCLK);
    SystemCoreClockUpdate();
}
```

## 11. 总结

CH32V307的RCC模块提供了完整灵活的时钟管理系统，支持多种时钟源、灵活的时钟分配和丰富的监控功能。通过合理配置系统时钟、外设时钟和特殊时钟功能，可以满足不同应用场景的需求。开发时需要注意时钟切换顺序、外设时钟依赖和性能优化等关键环节。RCC示例工程为开发者提供了全面的时钟配置参考，是学习CH32V307时钟系统的重要资料。
# CH32V307 RNG随机数生成器使用指南

## 1. 外设概述

RNG（Random Number Generator，随机数生成器）是CH32V307内置的硬件随机数生成模块，用于生成高质量的随机数。主要特性包括：

- **硬件随机源**：基于物理噪声源生成真随机数
- **32位随机数**：每次生成32位随机数据
- **自动种子生成**：内置种子生成机制
- **错误检测**：支持时钟错误和种子错误检测
- **中断支持**：支持错误中断处理
- **简单接口**：提供简单的API接口获取随机数

## 2. 关键代码片段

### 2.1 RNG基本使用示例

```c
#include "ch32v30x.h"
#include "debug.h"

int main(void)
{
    uint32_t random = 0;

    // 系统初始化
    SystemCoreClockUpdate();
    Delay_Init();
    USART_Printf_Init(115200);
    printf("SystemClk:%d\r\n", SystemCoreClock);
    printf("RNG Test\r\n");

    // 1. 使能RNG时钟
    RCC_AHBPeriphClockCmd(RCC_AHBPeriph_RNG, ENABLE);

    // 2. 使能RNG外设
    RNG_Cmd(ENABLE);

    while(1)
    {
        // 3. 等待数据就绪
        while(RNG_GetFlagStatus(RNG_FLAG_DRDY) == RESET)
        {
            // 等待随机数就绪
        }

        // 4. 读取随机数
        random = RNG_GetRandomNumber();

        // 5. 输出随机数
        printf("Random Number: 0x%08X\r\n", random);

        // 延迟500ms
        Delay_Ms(500);
    }
}
```

### 2.2 RNG中断模式配置

```c
// RNG中断服务程序
void RNG_IRQHandler(void) __attribute__((interrupt("WCH-Interrupt-fast")));
void RNG_IRQHandler(void)
{
    // 检查时钟错误中断
    if(RNG_GetITStatus(RNG_IT_CEI) != RESET)
    {
        printf("RNG Clock Error Interrupt\r\n");
        RNG_ClearITPendingBit(RNG_IT_CEI);

        // 处理时钟错误
        // 例如：重新配置时钟或切换到备用时钟源
    }

    // 检查种子错误中断
    if(RNG_GetITStatus(RNG_IT_SEI) != RESET)
    {
        printf("RNG Seed Error Interrupt\r\n");
        RNG_ClearITPendingBit(RNG_IT_SEI);

        // 处理种子错误
        // 例如：重新初始化RNG
    }
}

// RNG中断初始化
void RNG_Interrupt_Init(void)
{
    NVIC_InitTypeDef NVIC_InitStructure = {0};

    // 1. 使能RNG时钟
    RCC_AHBPeriphClockCmd(RCC_AHBPeriph_RNG, ENABLE);

    // 2. 使能RNG外设
    RNG_Cmd(ENABLE);

    // 3. 使能RNG中断
    RNG_ITConfig(ENABLE);

    // 4. 配置NVIC中断
    NVIC_InitStructure.NVIC_IRQChannel = RNG_IRQn;
    NVIC_InitStructure.NVIC_IRQChannelPreemptionPriority = 0;
    NVIC_InitStructure.NVIC_IRQChannelSubPriority = 0;
    NVIC_InitStructure.NVIC_IRQChannelCmd = ENABLE;
    NVIC_Init(&NVIC_InitStructure);

    printf("RNG Interrupt Mode Initialized\r\n");
}
```

### 2.3 RNG错误检测和处理

```c
// RNG错误状态检测
void RNG_Error_Check(void)
{
    // 检查时钟错误状态
    if(RNG_GetFlagStatus(RNG_FLAG_CECS) != RESET)
    {
        printf("RNG Clock Error Current Status\r\n");

        // 清除错误标志
        RNG_ClearFlag(RNG_FLAG_CECS);

        // 采取恢复措施
        // 例如：重新使能RNG或调整时钟配置
    }

    // 检查种子错误状态
    if(RNG_GetFlagStatus(RNG_FLAG_SECS) != RESET)
    {
        printf("RNG Seed Error Current Status\r\n");

        // 清除错误标志
        RNG_ClearFlag(RNG_FLAG_SECS);

        // 重新初始化RNG
        RNG_Recovery_Procedure();
    }
}

// RNG恢复过程
void RNG_Recovery_Procedure(void)
{
    // 1. 禁用RNG
    RNG_Cmd(DISABLE);

    // 2. 短暂延迟
    Delay_Ms(10);

    // 3. 重新使能RNG
    RNG_Cmd(ENABLE);

    printf("RNG Recovery Procedure Completed\r\n");
}
```

### 2.4 生成多个随机数

```c
// 生成指定数量的随机数
void Generate_Random_Numbers(uint32_t count)
{
    uint32_t i;
    uint32_t random_array[100];  // 假设最多100个随机数

    // 检查数量是否有效
    if(count > 100 || count == 0)
    {
        printf("Invalid count: %d (max 100)\r\n", count);
        return;
    }

    printf("Generating %d random numbers...\r\n", count);

    for(i = 0; i < count; i++)
    {
        // 等待数据就绪
        while(RNG_GetFlagStatus(RNG_FLAG_DRDY) == RESET)
        {
            // 可选的超时检测
            // if(timeout_condition) break;
        }

        // 读取随机数
        random_array[i] = RNG_GetRandomNumber();

        // 输出
        printf("[%03d] 0x%08X\r\n", i, random_array[i]);
    }

    printf("Random number generation completed\r\n");
}
```

## 3. 配置数据结构

### 3.1 RNG寄存器结构

```c
// RNG寄存器定义（ch32v30x.h）
typedef struct
{
  __IO uint32_t CR;  // 控制寄存器 (0x00)
  __IO uint32_t SR;  // 状态寄存器 (0x04)
  __IO uint32_t DR;  // 数据寄存器 (0x08)
} RNG_TypeDef;

// RNG基地址
#define RNG_BASE        (AHBPERIPH_BASE + 0x05000)
#define RNG             ((RNG_TypeDef *)RNG_BASE)
```

### 3.2 RNG控制寄存器（CR）位定义

```c
// 控制寄存器位定义
#define RNG_CR_RNGEN    ((uint32_t)0x00000004)  // RNG使能位
#define RNG_CR_IE       ((uint32_t)0x00000008)  // 中断使能位

// 控制寄存器操作
#define RNG_ENABLE      ((uint32_t)0x00000004)
#define RNG_DISABLE     ((uint32_t)0x00000000)
#define RNG_IT_ENABLE   ((uint32_t)0x00000008)
#define RNG_IT_DISABLE  ((uint32_t)0x00000000)
```

### 3.3 RNG状态寄存器（SR）位定义

```c
// 状态寄存器位定义
#define RNG_SR_DRDY     ((uint32_t)0x00000001)  // 数据就绪标志
#define RNG_SR_CECS     ((uint32_t)0x00000002)  // 时钟错误当前状态
#define RNG_SR_SECS     ((uint32_t)0x00000004)  // 种子错误当前状态
#define RNG_SR_CEIS     ((uint32_t)0x00000020)  // 时钟错误中断状态
#define RNG_SR_SEIS     ((uint32_t)0x00000040)  // 种子错误中断状态

// 状态标志掩码
#define RNG_FLAG_DRDY   ((uint8_t)0x0001)  // 数据就绪标志
#define RNG_FLAG_CECS   ((uint8_t)0x0002)  // 时钟错误当前状态
#define RNG_FLAG_SECS   ((uint8_t)0x0004)  // 种子错误当前状态

// 中断标志掩码
#define RNG_IT_CEI      ((uint8_t)0x20)    // 时钟错误中断
#define RNG_IT_SEI      ((uint8_t)0x40)    // 种子错误中断
```

### 3.4 RNG时钟配置定义

```c
// RNG时钟源定义（ch32v30x_rcc.h）
#define RCC_RNGCLKSource_SYSCLK   ((uint32_t)0x00)  // 系统时钟作为RNG时钟源
#define RCC_RNGCLKSource_PLL3_VCO ((uint32_t)0x01)  // PLL3 VCO作为RNG时钟源

// RNG时钟使能
#define RCC_AHBPeriph_RNG         ((uint32_t)0x00000200)  // RNG AHB外设时钟
```

## 4. 通用配置步骤

### 4.1 基本配置步骤（轮询模式）

1. **使能RNG时钟**：
   ```c
   RCC_AHBPeriphClockCmd(RCC_AHBPeriph_RNG, ENABLE);
   ```

2. **使能RNG外设**：
   ```c
   RNG_Cmd(ENABLE);
   ```

3. **等待数据就绪**：
   ```c
   while(RNG_GetFlagStatus(RNG_FLAG_DRDY) == RESET)
   {
       // 可选：添加超时机制
       // if(timeout_condition) break;
   }
   ```

4. **读取随机数**：
   ```c
   uint32_t random = RNG_GetRandomNumber();
   ```

5. **处理随机数**：
   ```c
   // 使用随机数
   printf("Random: 0x%08X\r\n", random);
   ```

### 4.2 中断模式配置步骤

1. **使能RNG时钟和外设**：
   ```c
   RCC_AHBPeriphClockCmd(RCC_AHBPeriph_RNG, ENABLE);
   RNG_Cmd(ENABLE);
   ```

2. **配置RNG中断**：
   ```c
   RNG_ITConfig(ENABLE);
   ```

3. **配置NVIC中断控制器**：
   ```c
   NVIC_InitTypeDef NVIC_InitStructure = {0};
   NVIC_InitStructure.NVIC_IRQChannel = RNG_IRQn;
   NVIC_InitStructure.NVIC_IRQChannelPreemptionPriority = 0;
   NVIC_InitStructure.NVIC_IRQChannelSubPriority = 0;
   NVIC_InitStructure.NVIC_IRQChannelCmd = ENABLE;
   NVIC_Init(&NVIC_InitStructure);
   ```

4. **编写中断服务程序**：
   ```c
   void RNG_IRQHandler(void) __attribute__((interrupt("WCH-Interrupt-fast")));
   void RNG_IRQHandler(void)
   {
       if(RNG_GetITStatus(RNG_IT_CEI) != RESET)
       {
           // 处理时钟错误
           RNG_ClearITPendingBit(RNG_IT_CEI);
       }

       if(RNG_GetITStatus(RNG_IT_SEI) != RESET)
       {
           // 处理种子错误
           RNG_ClearITPendingBit(RNG_IT_SEI);
       }
   }
   ```

### 4.3 错误处理步骤

1. **定期检查错误状态**：
   ```c
   // 检查时钟错误
   if(RNG_GetFlagStatus(RNG_FLAG_CECS) != RESET)
   {
       printf("Clock error detected\r\n");
       RNG_ClearFlag(RNG_FLAG_CECS);
   }

   // 检查种子错误
   if(RNG_GetFlagStatus(RNG_FLAG_SECS) != RESET)
   {
       printf("Seed error detected\r\n");
       RNG_ClearFlag(RNG_FLAG_SECS);
   }
   ```

2. **实现错误恢复机制**：
   ```c
   void RNG_Error_Recovery(void)
   {
       // 1. 禁用RNG
       RNG_Cmd(DISABLE);

       // 2. 短暂延迟
       Delay_Ms(10);

       // 3. 清除所有标志
       RNG_ClearFlag(RNG_FLAG_DRDY | RNG_FLAG_CECS | RNG_FLAG_SECS);

       // 4. 重新使能RNG
       RNG_Cmd(ENABLE);

       printf("RNG error recovery completed\r\n");
   }
   ```

### 4.4 随机数质量检测步骤

```c
// 简单的随机数质量测试
void RNG_Quality_Test(uint32_t sample_count)
{
    uint32_t i, random;
    uint32_t ones_count = 0;
    uint32_t zeros_count = 0;

    printf("Starting RNG quality test (%d samples)...\r\n", sample_count);

    for(i = 0; i < sample_count; i++)
    {
        // 等待并获取随机数
        while(RNG_GetFlagStatus(RNG_FLAG_DRDY) == RESET);
        random = RNG_GetRandomNumber();

        // 统计位分布
        for(int bit = 0; bit < 32; bit++)
        {
            if(random & (1 << bit))
                ones_count++;
            else
                zeros_count++;
        }
    }

    // 计算比例
    float ones_ratio = (float)ones_count / (ones_count + zeros_count);
    float zeros_ratio = (float)zeros_count / (ones_count + zeros_count);

    printf("Test results:\r\n");
    printf("  Total bits: %d\r\n", ones_count + zeros_count);
    printf("  Ones: %d (%.2f%%)\r\n", ones_count, ones_ratio * 100);
    printf("  Zeros: %d (%.2f%%)\r\n", zeros_count, zeros_ratio * 100);

    // 理想情况下应该接近50%/50%
    if(fabs(ones_ratio - 0.5) < 0.05)
        printf("  Result: PASS - Good randomness\r\n");
    else
        printf("  Result: FAIL - Poor randomness\r\n");
}
```

## 5. 示例工程详解

### 5.1 RNG示例工程

- **位置**：`RNG/RNG/User/main.c`
- **功能**：演示RNG基本使用，轮询模式获取随机数
- **特点**：
  - 简单的轮询实现
  - 500ms间隔生成随机数
  - 串口输出随机数值
  - 基本的错误状态检查

### 5.2 关键功能点

1. **时钟使能**：正确使能RNG的AHB时钟
2. **外设使能**：使用`RNG_Cmd(ENABLE)`使能RNG
3. **数据就绪检查**：轮询检查`RNG_FLAG_DRDY`标志
4. **随机数读取**：使用`RNG_GetRandomNumber()`获取32位随机数
5. **错误处理**：基本的错误状态检查

### 5.3 硬件连接要求

RNG不需要外部引脚连接，是完全的内部硬件模块。但需要注意：
- 正确的时钟配置（系统时钟或PLL3 VCO）
- 稳定的电源供应
- 适当的工作温度范围

### 5.4 性能特性

- **随机数生成速率**：取决于时钟频率和硬件特性
- **随机性质量**：基于物理噪声源的真随机数
- **功耗**：相对较低，适合嵌入式应用
- **启动时间**：需要初始化时间生成初始种子

## 6. 常见问题

### Q1: RNG不生成随机数或一直返回0？
A: 检查以下配置：
1. RNG时钟是否使能（`RCC_AHBPeriphClockCmd(RCC_AHBPeriph_RNG, ENABLE)`）
2. RNG外设是否使能（`RNG_Cmd(ENABLE)`）
3. 是否等待数据就绪标志（`RNG_FLAG_DRDY`）
4. 时钟配置是否正确（RNG需要正确的时钟源）

### Q2: RNG生成速度太慢？
A: 可能原因：
1. 时钟频率过低
2. 种子生成需要时间
3. 硬件限制
优化建议：
- 确保使用合适的时钟源
- 批量生成随机数而不是单个获取
- 考虑使用软件伪随机数生成器配合硬件随机数

### Q3: 如何提高随机数质量？
A: 建议措施：
1. 确保稳定的电源供应
2. 避免高温工作环境
3. 定期重置RNG以刷新种子
4. 使用后处理算法（如哈希函数）增强随机性
5. 组合多个随机数生成更长的随机序列

### Q4: RNG错误中断频繁触发？
A: 可能原因：
1. 时钟不稳定或频率超出范围
2. 电源噪声过大
3. 硬件故障
解决步骤：
1. 检查时钟配置和稳定性
2. 优化电源设计
3. 实现错误恢复机制
4. 考虑使用备用随机数源

### Q5: 如何测试RNG的随机性？
A: 测试方法：
1. 统计位分布（0和1的比例应接近50%/50%）
2. 运行频数测试（测试0-255每个值出现的频率）
3. 运行序列测试（测试连续位模式）
4. 使用专业的随机性测试套件（如NIST测试套件）

## 7. API参考

### 7.1 RNG控制API

| 函数名 | 功能描述 | 参数说明 |
|--------|----------|----------|
| `RNG_Cmd` | 使能/禁用RNG外设 | NewState：ENABLE或DISABLE |
| `RNG_ITConfig` | 使能/禁用RNG中断 | NewState：ENABLE或DISABLE |
| `RNG_GetRandomNumber` | 获取32位随机数 | 返回32位随机数 |

### 7.2 状态标志API

| 函数名 | 功能描述 | 参数说明 |
|--------|----------|----------|
| `RNG_GetFlagStatus` | 获取标志状态 | RNG_FLAG：标志位（DRDY/CECS/SECS） |
| `RNG_ClearFlag` | 清除标志 | RNG_FLAG：标志位 |
| `RNG_GetITStatus` | 获取中断状态 | RNG_IT：中断标志（CEI/SEI） |
| `RNG_ClearITPendingBit` | 清除中断挂起位 | RNG_IT：中断标志 |

### 7.3 时钟配置API（在RCC模块中）

| 函数名 | 功能描述 | 参数说明 |
|--------|----------|----------|
| `RCC_AHBPeriphClockCmd` | 使能/禁用AHB外设时钟 | RCC_AHBPeriph：外设，NewState：状态 |
| `RCC_RNGCLKConfig` | 配置RNG时钟源 | RCC_RNGCLKSource：时钟源 |

### 7.4 中断配置API（在NVIC模块中）

| 函数名 | 功能描述 | 参数说明 |
|--------|----------|----------|
| `NVIC_EnableIRQ` | 使能中断 | IRQn：中断号（RNG_IRQn=63） |
| `NVIC_DisableIRQ` | 禁用中断 | IRQn：中断号 |
| `NVIC_SetPriority` | 设置中断优先级 | IRQn：中断号，priority：优先级 |

### 7.5 常用标志和中断定义

```c
// 状态标志
#define RNG_FLAG_DRDY   ((uint8_t)0x0001)  // 数据就绪
#define RNG_FLAG_CECS   ((uint8_t)0x0002)  // 时钟错误当前状态
#define RNG_FLAG_SECS   ((uint8_t)0x0004)  // 种子错误当前状态

// 中断标志
#define RNG_IT_CEI      ((uint8_t)0x20)    // 时钟错误中断
#define RNG_IT_SEI      ((uint8_t)0x40)    // 种子错误中断

// 中断号
#define RNG_IRQn        ((IRQn_Type)63)    // RNG全局中断
```

## 8. 注意事项

1. **时钟要求**：RNG需要稳定的时钟源，时钟频率应在芯片规格范围内
2. **初始化时间**：RNG使能后需要时间生成初始种子，首次读取可能较慢
3. **错误处理**：建议实现错误检测和恢复机制，特别是对于安全关键应用
4. **随机性质量**：硬件RNG生成的是真随机数，但可能受环境因素影响
5. **电源稳定性**：电源噪声可能影响随机数质量，确保电源稳定
6. **温度影响**：极端温度可能影响RNG性能
7. **安全考虑**：对于加密应用，建议对RNG输出进行后处理
8. **中断处理**：中断服务程序应尽量简短，及时清除中断标志

## 9. 性能优化建议

1. **批量读取**：如果需要多个随机数，可以连续读取而不是等待每个随机数
2. **时钟优化**：选择合适的时钟源和频率平衡速度和功耗
3. **错误预防**：定期检查错误状态，预防性维护
4. **缓存机制**：实现随机数缓存，减少实时等待时间
5. **混合模式**：结合硬件RNG和软件PRNG，提高效率和随机性
6. **功耗管理**：不使用时禁用RNG以降低功耗
7. **温度监控**：在高温环境下监控RNG性能

## 10. 典型应用场景

### 10.1 加密密钥生成

```c
// 生成加密密钥
void Generate_Encryption_Key(uint8_t *key, uint32_t key_length)
{
    uint32_t i, random;
    uint32_t words_needed = (key_length + 3) / 4;  // 向上取整到4字节

    printf("Generating %d-byte encryption key...\r\n", key_length);

    for(i = 0; i < words_needed; i++)
    {
        // 获取随机数
        while(RNG_GetFlagStatus(RNG_FLAG_DRDY) == RESET);
        random = RNG_GetRandomNumber();

        // 复制到密钥数组
        uint32_t bytes_to_copy = 4;
        if(i == words_needed - 1 && (key_length % 4) != 0)
            bytes_to_copy = key_length % 4;

        memcpy(&key[i * 4], &random, bytes_to_copy);
    }

    printf("Encryption key generated\r\n");
}
```

### 10.2 随机延迟和退避

```c
// 生成随机延迟（用于冲突避免等场景）
uint32_t Generate_Random_Delay(uint32_t max_delay_ms)
{
    uint32_t random;

    // 获取随机数
    while(RNG_GetFlagStatus(RNG_FLAG_DRDY) == RESET);
    random = RNG_GetRandomNumber();

    // 计算延迟（0到max_delay_ms-1毫秒）
    return random % max_delay_ms;
}

// 使用示例
void Random_Backoff_Procedure(void)
{
    uint32_t retry_count = 0;
    uint32_t max_retries = 10;

    while(retry_count < max_retries)
    {
        // 尝试操作
        if(Perform_Critical_Operation())
        {
            printf("Operation successful\r\n");
            return;
        }

        // 生成随机退避时间
        uint32_t delay_ms = Generate_Random_Delay(100);  // 0-99ms
        printf("Retry %d failed, waiting %d ms\r\n", retry_count + 1, delay_ms);

        Delay_Ms(delay_ms);
        retry_count++;
    }

    printf("Operation failed after %d retries\r\n", max_retries);
}
```

### 10.3 游戏和模拟应用

```c
// 模拟骰子投掷
uint32_t Roll_Dice(uint32_t sides)
{
    uint32_t random;

    if(sides < 2 || sides > 100)
    {
        printf("Invalid number of sides: %d\r\n", sides);
        return 0;
    }

    // 获取随机数
    while(RNG_GetFlagStatus(RNG_FLAG_DRDY) == RESET);
    random = RNG_GetRandomNumber();

    // 计算骰子结果（1到sides）
    return (random % sides) + 1;
}

// 随机选择数组元素
void *Random_Select_Element(void *array[], uint32_t count)
{
    uint32_t random, index;

    if(count == 0)
        return NULL;

    // 获取随机数
    while(RNG_GetFlagStatus(RNG_FLAG_DRDY) == RESET);
    random = RNG_GetRandomNumber();

    // 计算索引
    index = random % count;

    return array[index];
}
```

## 11. 总结

CH32V307的RNG模块提供了硬件真随机数生成功能，适用于加密、安全、游戏和各种需要随机性的应用场景。通过简单的API接口，开发者可以轻松获取高质量的随机数。开发时需要注意时钟配置、错误处理和随机数质量等关键环节。RNG示例工程展示了基本的使用方法，开发者可以根据具体需求扩展错误处理、中断模式和性能优化等功能。
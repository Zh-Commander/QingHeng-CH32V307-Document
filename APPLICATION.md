# CH32V307 APPLICATION应用示例指南

## 1. 示例概述

APPLICATION文件夹包含高级应用示例，展示了CH32V307在实际应用场景中的综合使用。当前包含WS2812 LED控制示例，演示了两种不同的控制方式。

### 主要特性
- **多控制方式**：支持SPI和PWM两种WS2812控制方式
- **DMA传输**：使用DMA实现高效数据传输
- **灵活配置**：可配置LED数量和颜色效果
- **性能优化**：针对WS2812时序要求优化

## 2. 文件夹结构

```
APPLICATION/
└── WS2812_LED/                    # WS2812 LED控制示例
    └── User/
        ├── main.c                 # 主程序
        ├── ch32v30x_it.c         # 中断服务程序
        ├── ch32v30x_it.h         # 中断头文件
        ├── ch32v30x_conf.h       # 外设配置头文件
        ├── system_ch32v30x.c     # 系统初始化
        └── system_ch32v30x.h     # 系统头文件
```

## 3. 关键代码片段

### 3.1 SPI模式初始化

```c
void SPI_1Lines_HalfDuplex_Init(void)
{
    GPIO_InitTypeDef GPIO_InitStructure = {0};
    SPI_InitTypeDef SPI_InitStructure = {0};

    // 使能GPIO和SPI时钟
    RCC_APB2PeriphClockCmd(RCC_APB2Periph_GPIOA | RCC_APB2Periph_SPI1, ENABLE);

    // 配置MOSI引脚（PA7）
    GPIO_InitStructure.GPIO_Pin = GPIO_Pin_7;
    GPIO_InitStructure.GPIO_Mode = GPIO_Mode_AF_PP;
    GPIO_InitStructure.GPIO_Speed = GPIO_Speed_50MHz;
    GPIO_Init(GPIOA, &GPIO_InitStructure);

    // SPI参数配置
    SPI_InitStructure.SPI_Direction = SPI_Direction_1Line_Tx;      // 单线发送模式
    SPI_InitStructure.SPI_Mode = SPI_Mode_Master;                  // 主模式
    SPI_InitStructure.SPI_DataSize = SPI_DataSize_8b;              // 8位数据
    SPI_InitStructure.SPI_CPOL = SPI_CPOL_High;                    // 时钟极性高
    SPI_InitStructure.SPI_CPHA = SPI_CPHA_1Edge;                   // 时钟相位第1边沿
    SPI_InitStructure.SPI_NSS = SPI_NSS_Soft;                      // 软件NSS
    SPI_InitStructure.SPI_BaudRatePrescaler = SPI_BaudRatePrescaler_32; // 32分频
    SPI_InitStructure.SPI_FirstBit = SPI_FirstBit_MSB;             // MSB先发送
    SPI_InitStructure.SPI_CRCPolynomial = 7;                       // CRC多项式

    SPI_Init(SPI1, &SPI_InitStructure);
    SPI_Cmd(SPI1, ENABLE);
}
```

### 3.2 PWM模式初始化

```c
void TIM1_Init(void)
{
    GPIO_InitTypeDef GPIO_InitStructure = {0};
    TIM_OCInitTypeDef TIM_OCInitStructure = {0};
    TIM_TimeBaseInitTypeDef TIM_TimeBaseInitStructure = {0};

    // 使能GPIO和TIM1时钟
    RCC_APB2PeriphClockCmd(RCC_APB2Periph_GPIOA | RCC_APB2Periph_TIM1, ENABLE);

    // 配置TIM1 CH1引脚（PA8）
    GPIO_InitStructure.GPIO_Pin = GPIO_Pin_8;
    GPIO_InitStructure.GPIO_Mode = GPIO_Mode_AF_PP;
    GPIO_InitStructure.GPIO_Speed = GPIO_Speed_50MHz;
    GPIO_Init(GPIOA, &GPIO_InitStructure);

    // TIM时基配置
    TIM_TimeBaseInitStructure.TIM_Period = 10 - 1;                 // 自动重装载值
    TIM_TimeBaseInitStructure.TIM_Prescaler = SystemCoreClock / 8000000 - 1; // 预分频器
    TIM_TimeBaseInitStructure.TIM_ClockDivision = TIM_CKD_DIV1;    // 时钟分割
    TIM_TimeBaseInitStructure.TIM_CounterMode = TIM_CounterMode_Up; // 向上计数模式
    TIM_TimeBaseInit(TIM1, &TIM_TimeBaseInitStructure);

    // PWM输出配置
    TIM_OCInitStructure.TIM_OCMode = TIM_OCMode_PWM1;              // PWM模式1
    TIM_OCInitStructure.TIM_OutputState = TIM_OutputState_Enable;  // 输出使能
    TIM_OCInitStructure.TIM_Pulse = 0;                             // 初始占空比
    TIM_OCInitStructure.TIM_OCPolarity = TIM_OCPolarity_High;      // 输出极性高
    TIM_OC1Init(TIM1, &TIM_OCInitStructure);

    // 预装载使能
    TIM_OC1PreloadConfig(TIM1, TIM_OCPreload_Enable);
    TIM_ARRPreloadConfig(TIM1, ENABLE);

    // 使能TIM1主输出
    TIM_CtrlPWMOutputs(TIM1, ENABLE);

    // 使能DMA请求
    TIM_DMACmd(TIM1, TIM_DMA_Update, ENABLE);

    // 使能TIM1
    TIM_Cmd(TIM1, ENABLE);
}
```

### 3.3 DMA配置

```c
void DMA_Tx_Init(DMA_Channel_TypeDef *DMA_CHx, u32 ppadr, u32 memadr, u16 bufsize)
{
    DMA_InitTypeDef DMA_InitStructure = {0};

    DMA_InitStructure.DMA_PeripheralBaseAddr = ppadr;              // 外设基地址
    DMA_InitStructure.DMA_MemoryBaseAddr = memadr;                 // 内存基地址
    DMA_InitStructure.DMA_DIR = DMA_DIR_PeripheralDST;             // 传输方向：内存到外设
    DMA_InitStructure.DMA_BufferSize = bufsize;                    // 缓冲区大小
    DMA_InitStructure.DMA_PeripheralInc = DMA_PeripheralInc_Disable; // 外设地址不递增
    DMA_InitStructure.DMA_MemoryInc = DMA_MemoryInc_Enable;       // 内存地址递增
    DMA_InitStructure.DMA_PeripheralDataSize = DMA_PeripheralDataSize_Byte; // 外设数据宽度：字节
    DMA_InitStructure.DMA_MemoryDataSize = DMA_MemoryDataSize_Byte; // 内存数据宽度：字节
    DMA_InitStructure.DMA_Mode = DMA_Mode_Normal;                  // 普通模式
    DMA_InitStructure.DMA_Priority = DMA_Priority_VeryHigh;        // 优先级：非常高
    DMA_InitStructure.DMA_M2M = DMA_M2M_Disable;                   // 禁用内存到内存模式

    DMA_Init(DMA_CHx, &DMA_InitStructure);
}
```

### 3.4 颜色数据生成

```c
// WS2812时序要求（基于8MHz系统时钟）：
// 0码：高电平0.4μs，低电平0.85μs
// 1码：高电平0.8μs，低电平0.45μs
// RESET：低电平>50μs

// 生成WS2812数据流
void WS2812_GenerateData(uint8_t *buffer, uint32_t led_count, uint8_t r, uint8_t g, uint8_t b)
{
    uint32_t i, j;
    uint8_t *ptr = buffer;
    uint32_t color = (g << 16) | (r << 8) | b;  // GRB顺序

    for (i = 0; i < led_count; i++) {
        uint32_t mask = 0x800000;  // 从高位开始

        for (j = 0; j < 24; j++) {
            if (color & mask) {
                // 1码：高电平0.8μs，低电平0.45μs
                *ptr++ = 0xF8;  // 高电平持续时间（SPI模式）
                *ptr++ = 0x00;  // 低电平持续时间
            } else {
                // 0码：高电平0.4μs，低电平0.85μs
                *ptr++ = 0xC0;  // 高电平持续时间（SPI模式）
                *ptr++ = 0x00;  // 低电平持续时间
            }
            mask >>= 1;
        }
    }
}
```

## 4. 配置数据结构

### 4.1 WS2812配置参数

```c
// WS2812配置结构
typedef struct {
    uint16_t led_count;          // LED数量
    uint8_t mode;                // 控制模式（SPI/PWM）
    uint8_t brightness;          // 亮度（0-255）
    uint32_t *color_buffer;      // 颜色数据缓冲区
    uint32_t buffer_size;        // 缓冲区大小
} WS2812_ConfigTypeDef;

// 控制模式定义
#define WS2812_MODE_SPI          0
#define WS2812_MODE_PWM          1
```

## 5. 通用配置步骤

### 5.1 SPI模式配置步骤
1. **时钟配置**：
   - 使能GPIOA和SPI1时钟
   - 配置系统时钟频率（确保SPI时钟满足WS2812时序要求）

2. **GPIO配置**：
   - 配置PA7为复用推挽输出（MOSI）

3. **SPI配置**：
   - 配置为单线发送模式
   - 设置数据格式为8位
   - 配置时钟极性和相位
   - 设置波特率预分频

4. **DMA配置**：
   - 初始化DMA通道
   - 配置传输方向（内存到外设）
   - 设置缓冲区地址和大小

5. **数据生成**：
   - 根据WS2812时序要求生成数据流
   - 将颜色数据转换为SPI数据格式

### 5.2 PWM模式配置步骤
1. **时钟配置**：
   - 使能GPIOA和TIM1时钟

2. **GPIO配置**：
   - 配置PA8为复用推挽输出（TIM1 CH1）

3. **TIM配置**：
   - 配置时基参数（周期和预分频）
   - 配置PWM输出模式
   - 使能预装载和主输出

4. **DMA配置**：
   - 初始化DMA通道
   - 配置传输方向（内存到TIM CCR）
   - 设置缓冲区地址和大小

5. **数据生成**：
   - 根据WS2812时序要求生成PWM占空比数据
   - 将颜色数据转换为PWM脉宽数据

## 6. 示例工程详解

### 6.1 WS2812_LED示例
- **位置**：`APPLICATION/WS2812_LED/User/main.c`
- **功能**：控制WS2812 RGB LED灯带
- **特点**：
  - 支持SPI和PWM两种控制方式
  - 使用DMA实现高效数据传输
  - 可配置LED数量和颜色效果
  - 包含完整的时序控制逻辑
- **适用场景**：
  - LED装饰照明
  - 状态指示灯
  - 可视化效果显示

### 6.2 控制模式对比
| 特性 | SPI模式 | PWM模式 |
|------|---------|---------|
| 引脚需求 | MOSI引脚 | PWM输出引脚 |
| 硬件要求 | SPI外设 | TIM外设 |
| 时序精度 | 高 | 中等 |
| 系统资源 | SPI+DMA | TIM+DMA |
| 适用范围 | 大多数场景 | 需要PWM输出的场景 |

## 7. 常见问题

### Q1: WS2812时序不准确导致颜色异常？
A: 检查以下配置：
1. 系统时钟频率是否正确
2. SPI/PWM时钟分频设置
3. 时序参数是否符合WS2812规格书要求
4. DMA传输是否影响时序精度

### Q2: LED数量较多时数据传输不完整？
A: 可能原因：
1. DMA缓冲区大小不足
2. 数据传输速度过快
3. RESET时间不足
4. 电源供电不足

### Q3: SPI模式和PWM模式如何选择？
A: 根据应用需求选择：
- **SPI模式**：时序精度高，适用于大多数场景
- **PWM模式**：适用于需要PWM输出的其他应用

### Q4: 如何控制LED亮度？
A: 亮度控制方法：
1. 调节颜色分量值（R、G、B）
2. 使用PWM模式时调节占空比
3. 实现亮度渐变效果

## 8. API参考

| 函数名 | 功能描述 | 参数说明 |
|--------|----------|----------|
| `SPI_1Lines_HalfDuplex_Init` | SPI单线半双工初始化 | 无 |
| `TIM1_Init` | TIM1 PWM初始化 | 无 |
| `DMA_Tx_Init` | DMA发送初始化 | DMA_CHx：DMA通道，ppadr：外设地址，memadr：内存地址，bufsize：缓冲区大小 |
| `WS2812_GenerateData` | 生成WS2812数据流 | buffer：输出缓冲区，led_count：LED数量，r：红色分量，g：绿色分量，b：蓝色分量 |
| `WS2812_SendData_SPI` | SPI模式发送数据 | data：数据缓冲区，size：数据大小 |
| `WS2812_SendData_PWM` | PWM模式发送数据 | data：数据缓冲区，size：数据大小 |

## 9. 注意事项

1. **时序要求**：WS2812对时序要求严格，必须精确控制高低电平时间
2. **电源管理**：LED数量较多时需要足够的电源供应，建议每30个LED增加电源注入
3. **数据格式**：WS2812使用GRB数据顺序，不是常见的RGB顺序
4. **RESET时间**：帧之间需要>50μs的低电平RESET时间
5. **DMA缓冲区**：需要确保DMA缓冲区足够大，能够容纳所有LED的数据
6. **中断影响**：高优先级中断可能影响时序精度，需要合理设置中断优先级

## 10. 应用建议

1. **简单应用**：使用SPI模式，配置简单，时序精度高
2. **复杂效果**：可以实现呼吸灯、彩虹渐变、跑马灯等多种效果
3. **性能优化**：使用DMA传输减少CPU占用，提高系统响应速度
4. **扩展功能**：可以结合ADC、按键等外设实现交互式灯光效果
5. **节能设计**：不需要显示时关闭LED以降低功耗

## 11. 总结

CH32V307的WS2812 LED控制示例展示了微控制器在LED照明控制领域的应用能力。通过合理配置SPI或PWM外设，结合DMA传输，可以实现高效、稳定的LED控制。开发时需要注意WS2812的时序要求、电源管理和数据格式等关键环节，确保LED显示效果符合预期。
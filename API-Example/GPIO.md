# CH32V307 GPIO外设使用指南

## 1. 外设概述

GPIO（General Purpose Input/Output，通用输入输出）是单片机最基本的外设之一，用于控制引脚的电平状态和读取外部信号。CH32V307的GPIO具有以下特性：

- **8种工作模式**：模拟输入、浮空输入、下拉输入、上拉输入、开漏输出、推挽输出、复用开漏输出、复用推挽输出
- **多种输出速度**：10MHz、2MHz、50MHz
- **外部中断功能**：所有GPIO引脚均可配置为外部中断源
- **引脚重映射**：支持外设功能引脚重映射
- **锁定功能**：可锁定引脚配置防止意外修改

## 2. 关键代码片段

### 2.1 GPIO输出配置（GPIO_Toggle示例）

```c
// GPIO初始化函数
void GPIO_Toggle_INIT(void)
{
    GPIO_InitTypeDef GPIO_InitStructure = {0};

    // 1. 使能GPIOA时钟
    RCC_APB2PeriphClockCmd(RCC_APB2Periph_GPIOA, ENABLE);

    // 2. 配置GPIO参数
    GPIO_InitStructure.GPIO_Pin = GPIO_Pin_0;      // PA0引脚
    GPIO_InitStructure.GPIO_Mode = GPIO_Mode_Out_PP; // 推挽输出模式
    GPIO_InitStructure.GPIO_Speed = GPIO_Speed_50MHz; // 50MHz速度

    // 3. 初始化GPIO
    GPIO_Init(GPIOA, &GPIO_InitStructure);
}

// 主函数中的GPIO操作
while(1)
{
    Delay_Ms(250);
    // 翻转GPIO输出状态
    GPIO_WriteBit(GPIOA, GPIO_Pin_0, (i == 0) ? (i = Bit_SET) : (i = Bit_RESET));
}
```

### 2.2 GPIO外部中断配置（EXTI0示例）

```c
void EXTI0_INT_INIT(void)
{
    GPIO_InitTypeDef GPIO_InitStructure = {0};
    EXTI_InitTypeDef EXTI_InitStructure = {0};
    NVIC_InitTypeDef NVIC_InitStructure = {0};

    // 1. 使能AFIO和GPIOA时钟
    RCC_APB2PeriphClockCmd(RCC_APB2Periph_AFIO | RCC_APB2Periph_GPIOA, ENABLE);

    // 2. 配置GPIO为输入上拉模式
    GPIO_InitStructure.GPIO_Pin = GPIO_Pin_0;
    GPIO_InitStructure.GPIO_Mode = GPIO_Mode_IPU; // 输入上拉模式
    GPIO_Init(GPIOA, &GPIO_InitStructure);

    // 3. 配置GPIO与EXTI线映射
    GPIO_EXTILineConfig(GPIO_PortSourceGPIOA, GPIO_PinSource0);

    // 4. 配置EXTI参数
    EXTI_InitStructure.EXTI_Line = EXTI_Line0;
    EXTI_InitStructure.EXTI_Mode = EXTI_Mode_Interrupt; // 中断模式
    EXTI_InitStructure.EXTI_Trigger = EXTI_Trigger_Falling; // 下降沿触发
    EXTI_InitStructure.EXTI_LineCmd = ENABLE;
    EXTI_Init(&EXTI_InitStructure);

    // 5. 配置NVIC中断
    NVIC_InitStructure.NVIC_IRQChannel = EXTI0_IRQn;
    NVIC_InitStructure.NVIC_IRQChannelPreemptionPriority = 1;
    NVIC_InitStructure.NVIC_IRQChannelSubPriority = 2;
    NVIC_InitStructure.NVIC_IRQChannelCmd = ENABLE;
    NVIC_Init(&NVIC_InitStructure);
}

// 中断服务函数
void EXTI0_IRQHandler(void)
{
    if(EXTI_GetITStatus(EXTI_Line0)!=RESET)
    {
        EXTI_ClearITPendingBit(EXTI_Line0); // 清除中断标志
    }
}
```

### 2.3 GPIO复用功能配置（USART示例）

```c
// USART2 TX引脚配置为复用推挽输出
GPIO_InitStructure.GPIO_Pin = GPIO_Pin_2;
GPIO_InitStructure.GPIO_Speed = GPIO_Speed_50MHz;
GPIO_InitStructure.GPIO_Mode = GPIO_Mode_AF_PP; // 复用推挽输出
GPIO_Init(GPIOA, &GPIO_InitStructure);

// USART2 RX引脚配置为浮空输入
GPIO_InitStructure.GPIO_Pin = GPIO_Pin_3;
GPIO_InitStructure.GPIO_Mode = GPIO_Mode_IN_FLOATING; // 浮空输入
GPIO_Init(GPIOA, &GPIO_InitStructure);
```

## 3. 配置数据结构

### 3.1 GPIO模式枚举（GPIOMode_TypeDef）
```c
typedef enum
{
    GPIO_Mode_AIN = 0x0,           // 模拟输入
    GPIO_Mode_IN_FLOATING = 0x04,  // 浮空输入
    GPIO_Mode_IPD = 0x28,          // 下拉输入
    GPIO_Mode_IPU = 0x48,          // 上拉输入
    GPIO_Mode_Out_OD = 0x14,       // 开漏输出
    GPIO_Mode_Out_PP = 0x10,       // 推挽输出
    GPIO_Mode_AF_OD = 0x1C,        // 复用开漏输出
    GPIO_Mode_AF_PP = 0x18         // 复用推挽输出
} GPIOMode_TypeDef;
```

### 3.2 GPIO速度枚举（GPIOSpeed_TypeDef）
```c
typedef enum
{
    GPIO_Speed_10MHz = 1,
    GPIO_Speed_2MHz,
    GPIO_Speed_50MHz
} GPIOSpeed_TypeDef;
```

### 3.3 GPIO初始化结构体
```c
typedef struct
{
    uint16_t GPIO_Pin;             // 引脚选择
    GPIOSpeed_TypeDef GPIO_Speed;  // 输出速度
    GPIOMode_TypeDef GPIO_Mode;    // 工作模式
} GPIO_InitTypeDef;
```

## 4. 通用配置步骤

### 4.1 基本输出模式配置
1. **使能GPIO时钟**：`RCC_APB2PeriphClockCmd(RCC_APB2Periph_GPIOx, ENABLE)`
2. **定义初始化结构体**：`GPIO_InitTypeDef GPIO_InitStructure = {0};`
3. **配置引脚参数**：
   - `GPIO_InitStructure.GPIO_Pin = GPIO_Pin_x;`
   - `GPIO_InitStructure.GPIO_Mode = GPIO_Mode_Out_PP;` (推挽输出)
   - `GPIO_InitStructure.GPIO_Speed = GPIO_Speed_50MHz;`
4. **初始化GPIO**：`GPIO_Init(GPIOx, &GPIO_InitStructure);`

### 4.2 输入模式配置
1. **使能GPIO时钟**
2. **配置输入模式**：
   - 浮空输入：`GPIO_Mode_IN_FLOATING`
   - 上拉输入：`GPIO_Mode_IPU`
   - 下拉输入：`GPIO_Mode_IPD`
3. **初始化GPIO**

### 4.3 GPIO中断配置
1. **使能AFIO和GPIO时钟**：`RCC_APB2PeriphClockCmd(RCC_APB2Periph_AFIO | RCC_APB2Periph_GPIOx, ENABLE)`
2. **配置GPIO为输入模式**
3. **配置GPIO与EXTI线映射**：`GPIO_EXTILineConfig(GPIO_PortSourceGPIOx, GPIO_PinSourcex)`
4. **配置EXTI参数**：
   - 中断线：`EXTI_Linex`
   - 模式：`EXTI_Mode_Interrupt` 或 `EXTI_Mode_Event`
   - 触发方式：`EXTI_Trigger_Rising`/`Falling`/`Rising_Falling`
5. **配置NVIC中断**：设置优先级和使能中断通道
6. **编写中断服务函数**：清除中断标志

### 4.4 复用功能配置
1. **使能外设和GPIO时钟**
2. **配置GPIO为复用模式**：
   - 复用推挽输出：`GPIO_Mode_AF_PP`
   - 复用开漏输出：`GPIO_Mode_AF_OD`
3. **配置AFIO重映射（如果需要）**：`GPIO_PinRemapConfig()`

## 5. 常用操作函数

### 5.1 输出操作
- `GPIO_SetBits(GPIOx, GPIO_Pin)` - 设置引脚为高电平
- `GPIO_ResetBits(GPIOx, GPIO_Pin)` - 设置引脚为低电平
- `GPIO_WriteBit(GPIOx, GPIO_Pin, BitVal)` - 写入单个引脚
- `GPIO_Write(GPIOx, PortVal)` - 写入整个端口

### 5.2 输入操作
- `GPIO_ReadInputDataBit(GPIOx, GPIO_Pin)` - 读取输入引脚状态
- `GPIO_ReadInputData(GPIOx)` - 读取整个端口输入数据
- `GPIO_ReadOutputDataBit(GPIOx, GPIO_Pin)` - 读取输出引脚状态
- `GPIO_ReadOutputData(GPIOx)` - 读取整个端口输出数据

### 5.3 其他功能
- `GPIO_PinLockConfig(GPIOx, GPIO_Pin)` - 锁定引脚配置
- `GPIO_PinRemapConfig(uint32_t GPIO_Remap, FunctionalState NewState)` - 引脚重映射

## 6. 示例工程详解

### 6.1 GPIO_Toggle示例
- **位置**：`GPIO/GPIO_Toggle/User/main.c`
- **功能**：基本的GPIO输出控制，PA0引脚电平翻转
- **适用场景**：LED控制、继电器控制、简单的信号输出
- **关键配置**：推挽输出模式，50MHz速度

### 6.2 EXTI0示例
- **位置**：`EXTI/EXTI0/User/main.c`
- **功能**：GPIO外部中断，PA0引脚下降沿触发中断
- **适用场景**：按键检测、外部事件触发、实时响应
- **关键配置**：输入上拉模式，EXTI中断配置，NVIC优先级配置

## 7. 常见问题

### Q1: GPIO时钟使能失败怎么办？
A: 检查GPIO所属的总线，GPIOA-GPIOE在APB2总线上，使用`RCC_APB2PeriphClockCmd`函数使能时钟。

### Q2: 外部中断无法触发？
A: 检查以下配置：
1. GPIO时钟和AFIO时钟是否使能
2. GPIO与EXTI线映射是否正确
3. EXTI触发边沿配置是否正确
4. NVIC中断是否使能
5. 中断服务函数是否正确清除中断标志

### Q3: 复用功能无法工作？
A: 确保：
1. GPIO配置为正确的复用模式（AF_PP或AF_OD）
2. 对应外设的时钟已使能
3. 引脚重映射配置正确（如果需要）

### Q4: 输出速度如何选择？
A: 根据实际需求选择：
- 10MHz：低速应用，降低EMI
- 2MHz：中等速度
- 50MHz：高速应用，注意信号完整性

## 8. API参考

| 函数名 | 功能描述 | 参数说明 |
|--------|----------|----------|
| `GPIO_Init` | GPIO初始化 | GPIOx：GPIO端口，GPIO_InitStruct：初始化结构体 |
| `GPIO_SetBits` | 设置引脚高电平 | GPIOx：GPIO端口，GPIO_Pin：引脚号 |
| `GPIO_ResetBits` | 设置引脚低电平 | GPIOx：GPIO端口，GPIO_Pin：引脚号 |
| `GPIO_WriteBit` | 写入单个引脚 | GPIOx：GPIO端口，GPIO_Pin：引脚号，BitVal：位值 |
| `GPIO_Write` | 写入整个端口 | GPIOx：GPIO端口，PortVal：端口值 |
| `GPIO_ReadInputDataBit` | 读取输入引脚 | GPIOx：GPIO端口，GPIO_Pin：引脚号 |
| `GPIO_ReadInputData` | 读取整个端口输入 | GPIOx：GPIO端口 |
| `GPIO_ReadOutputDataBit` | 读取输出引脚 | GPIOx：GPIO端口，GPIO_Pin：引脚号 |
| `GPIO_ReadOutputData` | 读取整个端口输出 | GPIOx：GPIO端口 |
| `GPIO_PinLockConfig` | 锁定引脚配置 | GPIOx：GPIO端口，GPIO_Pin：引脚号 |
| `GPIO_PinRemapConfig` | 引脚重映射 | GPIO_Remap：重映射配置，NewState：新状态 |
| `GPIO_EXTILineConfig` | GPIO与EXTI线映射 | GPIO_PortSource：GPIO端口源，GPIO_PinSource：引脚源 |

## 9. 总结

CH32V307的GPIO外设功能丰富，支持多种工作模式和中断功能。通过合理配置GPIO模式、速度和中断参数，可以满足各种应用需求。在使用GPIO时，需要注意时钟使能、引脚重映射和中断配置等关键步骤，确保GPIO功能正常工作。
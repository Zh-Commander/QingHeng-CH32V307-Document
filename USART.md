# CH32V307 USART外设使用指南

## 1. 外设概述

USART（Universal Synchronous/Asynchronous Receiver/Transmitter，通用同步异步收发器）是CH32V307的重要通信外设，支持全双工异步串行通信。主要特性包括：

- **多工作模式**：异步模式、同步模式、单线半双工模式、多处理器通信模式
- **多种通信方式**：轮询、中断、DMA
- **硬件流控制**：支持RTS/CTS硬件流控制
- **灵活的波特率**：支持多种波特率设置
- **多种数据格式**：8位/9位数据位，1位/2位停止位，奇偶校验
- **高级功能**：多缓冲区通信、LIN模式、IrDA模式、智能卡模式

## 2. 关键代码片段

### 2.1 USART基本配置

```c
// 1. 使能时钟
RCC_APB1PeriphClockCmd(RCC_APB1Periph_USART2 | RCC_APB1Periph_USART3, ENABLE);
RCC_APB2PeriphClockCmd(RCC_APB2Periph_GPIOA | RCC_APB2Periph_GPIOB, ENABLE);

// 2. GPIO配置
GPIO_InitTypeDef GPIO_InitStructure = {0};

// TX引脚配置为复用推挽输出
GPIO_InitStructure.GPIO_Pin = GPIO_Pin_2;
GPIO_InitStructure.GPIO_Speed = GPIO_Speed_50MHz;
GPIO_InitStructure.GPIO_Mode = GPIO_Mode_AF_PP;
GPIO_Init(GPIOA, &GPIO_InitStructure);

// RX引脚配置为浮空输入
GPIO_InitStructure.GPIO_Pin = GPIO_Pin_3;
GPIO_InitStructure.GPIO_Mode = GPIO_Mode_IN_FLOATING;
GPIO_Init(GPIOA, &GPIO_InitStructure);

// 3. USART参数配置
USART_InitTypeDef USART_InitStructure = {0};

USART_InitStructure.USART_BaudRate = 115200;
USART_InitStructure.USART_WordLength = USART_WordLength_8b;
USART_InitStructure.USART_StopBits = USART_StopBits_1;
USART_InitStructure.USART_Parity = USART_Parity_No;
USART_InitStructure.USART_HardwareFlowControl = USART_HardwareFlowControl_None;
USART_InitStructure.USART_Mode = USART_Mode_Tx | USART_Mode_Rx;

// 4. 初始化USART
USART_Init(USART2, &USART_InitStructure);
USART_Cmd(USART2, ENABLE);
```

### 2.2 轮询方式通信（USART_Polling示例）

```c
// 轮询发送数据
USART_SendData(USART2, TxBuffer[TxCnt++]);
while(USART_GetFlagStatus(USART2, USART_FLAG_TXE) == RESET)
{
    /* 等待发送完成 */
}

// 轮询接收数据
while(USART_GetFlagStatus(USART3, USART_FLAG_RXNE) == RESET)
{
    /* 等待接收完成 */
}
RxBuffer[RxCnt++] = (USART_ReceiveData(USART3));
```

### 2.3 中断方式通信（USART_Interrupt示例）

```c
// 使能接收中断
USART_ITConfig(USART2, USART_IT_RXNE, ENABLE);

// NVIC配置
NVIC_InitTypeDef NVIC_InitStructure = {0};

NVIC_InitStructure.NVIC_IRQChannel = USART2_IRQn;
NVIC_InitStructure.NVIC_IRQChannelPreemptionPriority = 1;
NVIC_InitStructure.NVIC_IRQChannelSubPriority = 1;
NVIC_InitStructure.NVIC_IRQChannelCmd = ENABLE;
NVIC_Init(&NVIC_InitStructure);

// 中断处理函数
void USART2_IRQHandler(void)
{
    if(USART_GetITStatus(USART2, USART_IT_RXNE) != RESET)
    {
        RxBuffer1[RxCnt1++] = USART_ReceiveData(USART2);
        if(RxCnt1 == TxSize2)
        {
            USART_ITConfig(USART2, USART_IT_RXNE, DISABLE);
            Rxfinish1 = 1;
        }
    }
}
```

### 2.4 DMA方式通信（USART_DMA示例）

```c
// DMA初始化
DMA_InitTypeDef DMA_InitStructure = {0};

DMA_DeInit(DMA1_Channel7);
DMA_InitStructure.DMA_PeripheralBaseAddr = (u32)(&USART2->DATAR);
DMA_InitStructure.DMA_MemoryBaseAddr = (u32)TxBuffer1;
DMA_InitStructure.DMA_DIR = DMA_DIR_PeripheralDST;
DMA_InitStructure.DMA_BufferSize = TxSize1;
DMA_InitStructure.DMA_PeripheralInc = DMA_PeripheralInc_Disable;
DMA_InitStructure.DMA_MemoryInc = DMA_MemoryInc_Enable;
DMA_InitStructure.DMA_PeripheralDataSize = DMA_PeripheralDataSize_Byte;
DMA_InitStructure.DMA_MemoryDataSize = DMA_MemoryDataSize_Byte;
DMA_InitStructure.DMA_Mode = DMA_Mode_Normal;
DMA_InitStructure.DMA_Priority = DMA_Priority_VeryHigh;
DMA_InitStructure.DMA_M2M = DMA_M2M_Disable;
DMA_Init(DMA1_Channel7, &DMA_InitStructure);

// 使能USART的DMA请求
USART_DMACmd(USART2, USART_DMAReq_Tx | USART_DMAReq_Rx, ENABLE);

// 等待DMA传输完成
while(DMA_GetFlagStatus(DMA1_FLAG_TC7) == RESET)
{
    /* 等待USART2 TX DMA1传输完成 */
}
```

### 2.5 硬件流控制配置（USART_HardwareFlowControl示例）

```c
// 配置RTS和CTS引脚
GPIO_InitStructure.GPIO_Pin = GPIO_Pin_12; /* RTS-->A.12 */
GPIO_InitStructure.GPIO_Mode = GPIO_Mode_AF_PP;
GPIO_InitStructure.GPIO_Speed = GPIO_Speed_50MHz;
GPIO_Init(GPIOA, &GPIO_InitStructure);

GPIO_InitStructure.GPIO_Pin = GPIO_Pin_11; /* CTS-->A.11 */
GPIO_InitStructure.GPIO_Mode = GPIO_Mode_IN_FLOATING;
GPIO_Init(GPIOA, &GPIO_InitStructure);

// 使能硬件流控制
USART_InitStructure.USART_HardwareFlowControl = USART_HardwareFlowControl_RTS_CTS;
```

### 2.6 半双工模式配置（USART_HalfDuplex示例）

```c
// GPIO配置为开漏输出
GPIO_InitStructure.GPIO_Pin = GPIO_Pin_2;
GPIO_InitStructure.GPIO_Mode = GPIO_Mode_AF_OD;
GPIO_InitStructure.GPIO_Speed = GPIO_Speed_50MHz;
GPIO_Init(GPIOA, &GPIO_InitStructure);

// 使能半双工模式
USART_HalfDuplexCmd(USART2, ENABLE);
```

### 2.7 IDLE中断+DMA接收不定长数据（USART_Idle_Recv示例）

```c
// 使能IDLE中断
USART_ITConfig(USART1, USART_IT_IDLE, ENABLE);

// 中断处理函数
void USART1_IRQHandler(void)
{
    if (USART_GetITStatus(USART1, USART_IT_IDLE) != RESET)
    {
        // 计算接收到的数据长度
        uint16_t rxlen = (RX_BUFFER_LEN - USART_RX_CH->CNTR);
        uint8_t oldbuffer = USART_DMA_CTRL.DMA_USE_BUFFER;

        USART_DMA_CTRL.DMA_USE_BUFFER = !oldbuffer;

        // 切换DMA缓冲区
        DMA_Cmd(USART_RX_CH, DISABLE);
        DMA_SetCurrDataCounter(USART_RX_CH, RX_BUFFER_LEN);
        USART_RX_CH->MADDR = (uint32_t)(USART_DMA_CTRL.Rx_Buffer[USART_DMA_CTRL.DMA_USE_BUFFER]);
        DMA_Cmd(USART_RX_CH, ENABLE);

        USART_ReceiveData(USART1); // 清除IDLE标志
        ring_buffer_push_huge(USART_DMA_CTRL.Rx_Buffer[oldbuffer], rxlen);
    }
}
```

### 2.8 多处理器通信模式（USART_MultiProcessorCommunication示例）

```c
// 设置地址
USART_SetAddress(USART2, 0x1);
USART_SetAddress(USART3, 0x2);

// 配置唤醒方式
USART_WakeUpConfig(USART3, USART_WakeUp_AddressMark);
USART_ReceiverWakeUpCmd(USART3, ENABLE); /* USART3进入静默模式 */

// 发送地址帧（第9位为1表示地址帧）
USART_SendData(USART2, 0x102); /* 发送USART3地址 */
```

### 2.9 同步模式配置（USART_SynchronousMode示例）

```c
// 时钟配置
USART_ClockInitTypeDef USART_ClockInitStructure = {0};

USART_ClockInitStructure.USART_Clock = USART_Clock_Enable;
USART_ClockInitStructure.USART_CPOL = USART_CPOL_High;         /* 时钟高电平有效 */
USART_ClockInitStructure.USART_CPHA = USART_CPHA_2Edge;        /* 数据在第二个边沿捕获 */
USART_ClockInitStructure.USART_LastBit = USART_LastBit_Enable; /* 最后一位数据的时钟脉冲输出到SCLK引脚 */
USART_ClockInit(USART2, &USART_ClockInitStructure);
```

## 3. 配置数据结构

### 3.1 USART_InitTypeDef（USART初始化结构体）
```c
typedef struct
{
    uint32_t USART_BaudRate;            // 波特率
    uint16_t USART_WordLength;          // 数据位长度
    uint16_t USART_StopBits;            // 停止位
    uint16_t USART_Parity;              // 校验位
    uint16_t USART_HardwareFlowControl; // 硬件流控制
    uint16_t USART_Mode;                // 工作模式
} USART_InitTypeDef;
```

### 3.2 USART_ClockInitTypeDef（时钟初始化结构体）
```c
typedef struct
{
    uint16_t USART_Clock;    // 时钟使能
    uint16_t USART_CPOL;     // 时钟极性
    uint16_t USART_CPHA;     // 时钟相位
    uint16_t USART_LastBit;  // 最后一位时钟
} USART_ClockInitTypeDef;
```

## 4. 通用配置步骤

### 4.1 基本配置步骤
1. **时钟使能**：
   - 使能USART外设时钟（USART1在APB2，USART2/3在APB1）
   - 使能GPIO端口时钟

2. **GPIO配置**：
   - TX引脚：`GPIO_Mode_AF_PP`（复用推挽输出）
   - RX引脚：`GPIO_Mode_IN_FLOATING`（浮空输入）
   - 半双工模式：`GPIO_Mode_AF_OD`（开漏输出）
   - 硬件流控制：RTS和CTS引脚配置

3. **USART参数配置**：
   - 波特率：115200（常用）
   - 数据位：8位或9位
   - 停止位：1位
   - 校验位：无、奇校验或偶校验
   - 硬件流控制：无、RTS/CTS
   - 工作模式：发送、接收或两者

4. **中断配置**（如果需要）：
   - 使能相应中断（RXNE、TXE、IDLE等）
   - 配置NVIC优先级
   - 编写中断服务函数

5. **DMA配置**（如果需要）：
   - 初始化DMA通道
   - 配置外设地址、内存地址、数据长度等
   - 使能USART的DMA请求

6. **使能USART**：
   - `USART_Cmd(USARTx, ENABLE)`

### 4.2 波特率计算
CH32V307 USART波特率计算公式：
```
波特率 = fPCLKx / (16 * USARTDIV)
```
其中：
- fPCLKx：USART时钟频率
- USARTDIV：USART分频值，包含整数和小数部分

## 5. 示例工程详解

### 5.1 USART_Polling（轮询方式）
- **位置**：`USART/USART_Polling/User/main.c`
- **功能**：轮询方式串口通信
- **适用场景**：简单应用，数据量小，对实时性要求不高
- **特点**：代码简单，占用CPU资源

### 5.2 USART_Interrupt（中断方式）
- **位置**：`USART/USART_Interrupt/User/main.c`
- **功能**：中断方式串口通信
- **适用场景**：实时性要求高，需要及时响应数据
- **特点**：响应及时，不阻塞主程序

### 5.3 USART_DMA（DMA方式）
- **位置**：`USART/USART_DMA/User/main.c`
- **功能**：DMA方式串口通信
- **适用场景**：大数据量传输，需要释放CPU资源
- **特点**：高效，不占用CPU资源

### 5.4 USART_HardwareFlowControl（硬件流控制）
- **位置**：`USART/USART_HardwareFlowControl/User/main.c`
- **功能**：硬件流控制串口通信
- **适用场景**：高速可靠通信，防止数据丢失
- **特点**：需要额外引脚（RTS、CTS），提高通信可靠性

### 5.5 USART_HalfDuplex（半双工模式）
- **位置**：`USART/USART_HalfDuplex/User/main.c`
- **功能**：半双工模式串口通信
- **适用场景**：单线通信，节省引脚
- **特点**：需要上拉电阻，GPIO配置为开漏模式

### 5.6 USART_Idle_Recv（IDLE中断+DMA接收）
- **位置**：`USART/USART_Idle_Recv/User/main.c`
- **功能**：IDLE中断+DMA接收不定长数据
- **适用场景**：接收不定长数据包
- **特点**：灵活，高效处理变长数据包

### 5.7 USART_MultiProcessorCommunication（多处理器通信）
- **位置**：`USART/USART_MultiProcessorCommunication/User/main.c`
- **功能**：多处理器通信模式
- **适用场景**：一主多从通信
- **特点**：地址寻址，节能模式

### 5.8 USART_Printf（调试输出）
- **位置**：`USART/USART_Printf/User/main.c`
- **功能**：printf调试输出
- **适用场景**：程序调试
- **特点**：方便调试信息输出

### 5.9 USART_SynchronousMode（同步模式）
- **位置**：`USART/USART_SynchronousMode/User/main.c`
- **功能**：同步模式（USART作为SPI主设备）
- **适用场景**：USART作为SPI主设备
- **特点**：需要时钟引脚，兼容SPI设备

## 6. 常见问题

### Q1: USART时钟源如何选择？
A: CH32V307 USART时钟源：
- USART1挂载在APB2总线
- USART2和USART3挂载在APB1总线

### Q2: 波特率设置不准确怎么办？
A: 检查以下配置：
1. 系统时钟配置是否正确
2. USART时钟分频设置
3. 波特率计算公式是否正确

### Q3: 中断方式通信数据丢失？
A: 可能原因：
1. 中断优先级配置不合理
2. 中断服务函数处理时间过长
3. 缓冲区溢出
4. 数据接收速度过快

### Q4: DMA传输不工作？
A: 检查以下配置：
1. DMA通道与USART外设的映射关系
2. DMA初始化参数是否正确
3. USART DMA请求是否使能
4. DMA传输完成标志是否正确处理

### Q5: 硬件流控制无法工作？
A: 检查以下配置：
1. RTS和CTS引脚配置是否正确
2. 硬件流控制是否使能
3. 对方设备是否支持硬件流控制

### Q6: 半双工模式通信异常？
A: 检查以下配置：
1. GPIO是否配置为开漏输出模式
2. 总线上是否接有上拉电阻
3. 半双工模式是否使能

## 7. API参考

| 函数名 | 功能描述 | 参数说明 |
|--------|----------|----------|
| `USART_Init` | USART初始化 | USARTx：USART外设，USART_InitStruct：初始化结构体 |
| `USART_Cmd` | 使能/禁用USART | USARTx：USART外设，NewState：新状态 |
| `USART_ITConfig` | 使能/禁用USART中断 | USARTx：USART外设，USART_IT：中断源，NewState：新状态 |
| `USART_DMACmd` | 使能/禁用USART DMA | USARTx：USART外设，USART_DMAReq：DMA请求，NewState：新状态 |
| `USART_SendData` | 发送数据 | USARTx：USART外设，Data：要发送的数据 |
| `USART_ReceiveData` | 接收数据 | USARTx：USART外设 |
| `USART_GetFlagStatus` | 获取标志位状态 | USARTx：USART外设，USART_FLAG：标志位 |
| `USART_ClearFlag` | 清除标志位 | USARTx：USART外设，USART_FLAG：标志位 |
| `USART_GetITStatus` | 获取中断状态 | USARTx：USART外设，USART_IT：中断源 |
| `USART_ClearITPendingBit` | 清除中断挂起位 | USARTx：USART外设，USART_IT：中断源 |
| `USART_HalfDuplexCmd` | 使能/禁用半双工模式 | USARTx：USART外设，NewState：新状态 |
| `USART_SetAddress` | 设置USART地址 | USARTx：USART外设，USART_Address：地址 |
| `USART_WakeUpConfig` | 配置唤醒方式 | USARTx：USART外设，USART_WakeUp：唤醒方式 |

## 8. 总结

CH32V307的USART外设功能丰富，支持多种工作模式和通信方式。通过合理配置USART参数，可以实现可靠的串口通信。在使用USART时，需要注意时钟配置、中断处理、DMA使用等关键步骤，确保通信功能正常工作。不同应用场景可以选择不同的通信方式，如简单应用使用轮询方式，实时性要求高的应用使用中断方式，大数据量传输使用DMA方式。
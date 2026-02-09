# CH32V307 I2C外设使用指南

## 1. 外设概述

I2C（Inter-Integrated Circuit，内部集成电路）是CH32V307的双线串行通信接口，用于连接低速外部设备。主要特性包括：

- **多工作模式**：主模式、从模式
- **多种地址格式**：7位地址、10位地址
- **多种通信方式**：轮询、中断、DMA
- **灵活的时钟速度**：标准模式（100kHz）、快速模式（400kHz）
- **高级功能**：PEC（包错误校验）、SMBus协议支持
- **多主模式**：支持多主设备竞争检测

## 2. 关键代码片段

### 2.1 I2C基本配置

```c
// I2C初始化函数
void IIC_Init(u32 bound, u16 address) {
    I2C_InitTypeDef I2C_InitTSturcture = { 0 };

    I2C_InitTSturcture.I2C_ClockSpeed = bound;          // 时钟速度
    I2C_InitTSturcture.I2C_Mode = I2C_Mode_I2C;         // I2C模式
    I2C_InitTSturcture.I2C_DutyCycle = I2C_DutyCycle_16_9; // 占空比（快速模式）
    I2C_InitTSturcture.I2C_OwnAddress1 = address;       // 自身地址
    I2C_InitTSturcture.I2C_Ack = I2C_Ack_Enable;        // 应答使能
    I2C_InitTSturcture.I2C_AcknowledgedAddress = I2C_AcknowledgedAddress_7bit; // 7位地址

    I2C_Init(I2C1, &I2C_InitTSturcture);
    I2C_Cmd(I2C1, ENABLE);  // 使能I2C
}
```

### 2.2 7位地址主模式发送（轮询方式）

```c
// 等待总线空闲
while(I2C_GetFlagStatus(I2C1, I2C_FLAG_BUSY) != RESET);

// 发送起始信号
I2C_GenerateSTART(I2C1, ENABLE);

// 等待主模式选择事件
while(!I2C_CheckEvent(I2C1, I2C_EVENT_MASTER_MODE_SELECT));

// 发送7位地址（写模式）
I2C_Send7bitAddress(I2C1, 0x02, I2C_Direction_Transmitter);

// 等待发送器模式选择事件
while(!I2C_CheckEvent(I2C1, I2C_EVENT_MASTER_TRANSMITTER_MODE_SELECTED));

// 发送数据
for(i=0; i<6; i++) {
    while(I2C_GetFlagStatus(I2C1, I2C_FLAG_TXE) == RESET);
    I2C_SendData(I2C1, TxData[i]);
}

// 等待字节传输完成
while(!I2C_CheckEvent(I2C1, I2C_EVENT_MASTER_BYTE_TRANSMITTED));

// 发送停止信号
I2C_GenerateSTOP(I2C1, ENABLE);
```

### 2.3 7位地址从模式接收（轮询方式）

```c
// 等待地址匹配事件
while(!I2C_CheckEvent(I2C1, I2C_EVENT_SLAVE_RECEIVER_ADDRESS_MATCHED));

// 接收数据
while(i < 6) {
    while(I2C_GetFlagStatus(I2C1, I2C_FLAG_RXNE) == RESET);
    RxData[p][i] = I2C_ReceiveData(I2C1);
    i++;
}

// 等待停止信号
while(I2C_GetFlagStatus(I2C1, I2C_FLAG_STOPF) == RESET);
I2C1->CTLR1 &= I2C1->CTLR1;  // 清除停止标志
```

### 2.4 中断方式通信（I2C_7bit_Interrupt_Mode示例）

```c
// 中断配置
NVIC_InitTypeDef NVIC_InitStructure = {0};

NVIC_InitStructure.NVIC_IRQChannel = I2C1_EV_IRQn;
NVIC_InitStructure.NVIC_IRQChannelPreemptionPriority = 1;
NVIC_InitStructure.NVIC_IRQChannelSubPriority = 0;
NVIC_InitStructure.NVIC_IRQChannelCmd = ENABLE;
NVIC_Init(&NVIC_InitStructure);

// 使能I2C中断
I2C_ITConfig(I2C1, I2C_IT_BUF, ENABLE);
I2C_ITConfig(I2C1, I2C_IT_EVT, ENABLE);
I2C_ITConfig(I2C1, I2C_IT_ERR, ENABLE);

// 中断处理函数
void I2C1_EV_IRQHandler(void) {
#if (I2C_MODE == HOST_MODE)
    if(I2C_GetITStatus(I2C1, I2C_IT_SB) != RESET) {
        // 起始位中断
        if(master_sate == 0) {
            master_sate = 1;
            I2C_Send7bitAddress(I2C1, 0x02, I2C_Direction_Transmitter);
        }
    }
    else if(I2C_GetITStatus(I2C1, I2C_IT_ADDR) != RESET) {
        // 地址发送完成中断
        ((void)I2C_ReadRegister(I2C1, I2C_Register_STAR2)); // 清除标志
    }
    else if(I2C_GetITStatus(I2C1, I2C_IT_TXE) != RESET) {
        // 发送缓冲区空中断
        if(master_send_len<6) {
            I2C_SendData(I2C1, TxData[master_send_len]);
            master_send_len++;
        }
    }
    else if(I2C_GetITStatus(I2C1, I2C_IT_RXNE) != RESET) {
        // 接收缓冲区非空中断
        if(master_recv_len<6) {
            RxData[master_recv_len] = I2C_ReceiveData(I2C1);
            master_recv_len++;
        }
    }
#endif
}
```

### 2.5 10位地址模式（I2C_10bit_Mode示例）

```c
// 10位地址配置
I2C_InitTSturcture.I2C_AcknowledgedAddress = I2C_AcknowledgedAddress_10bit;

// 10位地址发送流程
// 发送起始信号后
while(!I2C_CheckEvent(I2C1, I2C_EVENT_MASTER_MODE_SELECT));

// 发送10位地址的第一部分（11110 + 地址高2位 + 写标志）
I2C_Send7bitAddress(I2C1, 0xF0, I2C_Direction_Transmitter);

// 等待10位地址模式事件
while(!I2C_CheckEvent(I2C1, I2C_EVENT_MASTER_MODE_ADDRESS10));

// 发送地址的低8位
I2C_Send7bitAddress(I2C1, 0x02, I2C_Direction_Transmitter);
```

### 2.6 DMA传输模式（I2C_DMA示例）

```c
// DMA发送初始化
void DMA_Tx_Init(DMA_Channel_TypeDef *DMA_CHx, u32 ppadr, u32 memadr, u16 bufsize) {
    DMA_InitTypeDef DMA_InitStructure = {0};

    DMA_InitStructure.DMA_PeripheralBaseAddr = ppadr;      // 外设地址：I2C数据寄存器
    DMA_InitStructure.DMA_MemoryBaseAddr = memadr;         // 内存地址：发送数据缓冲区
    DMA_InitStructure.DMA_DIR = DMA_DIR_PeripheralDST;     // 方向：内存到外设
    DMA_InitStructure.DMA_BufferSize = bufsize;            // 传输数据量
    DMA_InitStructure.DMA_PeripheralInc = DMA_PeripheralInc_Disable; // 外设地址不递增
    DMA_InitStructure.DMA_MemoryInc = DMA_MemoryInc_Enable; // 内存地址递增
    DMA_InitStructure.DMA_PeripheralDataSize = DMA_PeripheralDataSize_Byte;
    DMA_InitStructure.DMA_MemoryDataSize = DMA_MemoryDataSize_Byte;
    DMA_InitStructure.DMA_Mode = DMA_Mode_Normal;          // 普通模式
    DMA_InitStructure.DMA_Priority = DMA_Priority_VeryHigh;
    DMA_InitStructure.DMA_M2M = DMA_M2M_Disable;           // 禁用内存到内存
    DMA_Init(DMA_CHx, &DMA_InitStructure);
}

// I2C DMA使能
I2C_DMACmd(I2C1, ENABLE);

// DMA传输流程
DMA_Tx_Init(DMA1_Channel6, (u32)&I2C1->DATAR, (u32)TxData, Tize);
I2C_GenerateSTART(I2C1, ENABLE);
while(!I2C_CheckEvent(I2C1, I2C_EVENT_MASTER_MODE_SELECT));
I2C_Send7bitAddress(I2C1, 0x02, I2C_Direction_Transmitter);
while(!I2C_CheckEvent(I2C1, I2C_EVENT_MASTER_TRANSMITTER_MODE_SELECTED));
DMA_Cmd(DMA1_Channel6, ENABLE);
```

### 2.7 EEPROM操作（I2C_EEPROM示例）

```c
// 写一个字节到EEPROM
void AT24CXX_WriteOneByte(u16 WriteAddr, u8 DataToWrite) {
    // 发送起始信号
    I2C_GenerateSTART(I2C2, ENABLE);
    while(!I2C_CheckEvent(I2C2, I2C_EVENT_MASTER_MODE_SELECT));

    // 发送EEPROM设备地址（写模式）
    I2C_Send7bitAddress(I2C2, 0XA0, I2C_Direction_Transmitter);
    while(!I2C_CheckEvent(I2C2, I2C_EVENT_MASTER_TRANSMITTER_MODE_SELECTED));

    // 发送数据地址
#if(Address_Lenth == Address_8bit)
    I2C_SendData(I2C2, (u8)(WriteAddr & 0x00FF));
#elif(Address_Lenth == Address_16bit)
    I2C_SendData(I2C2, (u8)(WriteAddr >> 8));
    while(!I2C_CheckEvent(I2C2, I2C_EVENT_MASTER_BYTE_TRANSMITTED));
    I2C_SendData(I2C2, (u8)(WriteAddr & 0x00FF));
#endif

    // 发送数据
    I2C_SendData(I2C2, DataToWrite);
    while(!I2C_CheckEvent(I2C2, I2C_EVENT_MASTER_BYTE_TRANSMITTED));

    // 发送停止信号
    I2C_GenerateSTOP(I2C2, ENABLE);
}

// 从EEPROM读取一个字节
u8 AT24CXX_ReadOneByte(u16 ReadAddr) {
    u8 temp = 0;

    // 发送起始信号和设备地址（写模式）
    I2C_GenerateSTART(I2C2, ENABLE);
    while(!I2C_CheckEvent(I2C2, I2C_EVENT_MASTER_MODE_SELECT));
    I2C_Send7bitAddress(I2C2, 0XA0, I2C_Direction_Transmitter);
    while(!I2C_CheckEvent(I2C2, I2C_EVENT_MASTER_TRANSMITTER_MODE_SELECTED));

    // 发送数据地址
    I2C_SendData(I2C2, (u8)(ReadAddr & 0x00FF));
    while(!I2C_CheckEvent(I2C2, I2C_EVENT_MASTER_BYTE_TRANSMITTED));

    // 发送重复起始信号
    I2C_GenerateSTART(I2C2, ENABLE);
    while(!I2C_CheckEvent(I2C2, I2C_EVENT_MASTER_MODE_SELECT));

    // 发送设备地址（读模式）
    I2C_Send7bitAddress(I2C2, 0XA0, I2C_Direction_Receiver);
    while(!I2C_CheckEvent(I2C2, I2C_EVENT_MASTER_RECEIVER_MODE_SELECTED));

    // 禁用应答，准备接收最后一个字节
    I2C_AcknowledgeConfig(I2C2, DISABLE);

    // 接收数据
    while(I2C_GetFlagStatus(I2C2, I2C_FLAG_RXNE) == RESET);
    temp = I2C_ReceiveData(I2C2);

    // 发送停止信号
    I2C_GenerateSTOP(I2C2, ENABLE);

    return temp;
}
```

### 2.8 PEC错误校验（I2C_PEC示例）

```c
// 使能PEC计算
I2C_CalculatePEC(I2C1, ENABLE);

// 从模式错误中断配置
#if(I2C_MODE == SLAVE_MODE)
    NVIC_InitStructure.NVIC_IRQChannel = I2C1_ER_IRQn;
    NVIC_InitStructure.NVIC_IRQChannelPreemptionPriority = 2;
    NVIC_InitStructure.NVIC_IRQChannelSubPriority = 0;
    NVIC_InitStructure.NVIC_IRQChannelCmd = ENABLE;
    NVIC_Init(&NVIC_InitStructure);

    I2C_ITConfig(I2C1, I2C_IT_ERR, ENABLE);
#endif

// PEC传输
if(i == 6) {  // 发送完6个数据后
    if(I2C_GetFlagStatus(I2C1, I2C_FLAG_TXE) != RESET) {
        I2C_TransmitPEC(I2C1, ENABLE);  // 发送PEC字节
        i++;
    }
}

// PEC错误中断处理
void I2C1_ER_IRQHandler(void) {
    if(I2C_GetITStatus(I2C1, I2C_IT_PECERR) != RESET) {
        printf("PECEER\r\n");  // PEC错误
        I2C_ClearITPendingBit(I2C1, I2C_IT_PECERR);
    }
}
```

## 3. 配置数据结构

### 3.1 I2C_InitTypeDef（I2C初始化结构体）
```c
typedef struct
{
    uint32_t I2C_ClockSpeed;          // 时钟速度（Hz）
    uint16_t I2C_Mode;                // 工作模式（I2C/SMBus）
    uint16_t I2C_DutyCycle;           // 占空比（快速模式）
    uint16_t I2C_OwnAddress1;         // 自身地址1
    uint16_t I2C_Ack;                 // 应答使能
    uint16_t I2C_AcknowledgedAddress; // 应答地址格式（7位/10位）
} I2C_InitTypeDef;
```

## 4. 通用配置步骤

### 4.1 硬件引脚配置
```c
// 1. 使能GPIO和AFIO时钟
RCC_APB2PeriphClockCmd(RCC_APB2Periph_GPIOB | RCC_APB2Periph_AFIO, ENABLE);

// 2. 引脚重映射（如果需要）
GPIO_PinRemapConfig(GPIO_Remap_I2C1, ENABLE);

// 3. 使能I2C时钟
RCC_APB1PeriphClockCmd(RCC_APB1Periph_I2C1, ENABLE);

// 4. 配置GPIO为开漏输出模式
GPIO_InitStructure.GPIO_Pin = GPIO_Pin_8 | GPIO_Pin_9; // SCL, SDA
GPIO_InitStructure.GPIO_Mode = GPIO_Mode_AF_OD;  // 复用开漏输出
GPIO_InitStructure.GPIO_Speed = GPIO_Speed_50MHz;
GPIO_Init(GPIOB, &GPIO_InitStructure);
```

### 4.2 I2C参数配置
```c
I2C_InitTypeDef I2C_InitStructure = {0};

I2C_InitStructure.I2C_ClockSpeed = 80000;  // 80kHz时钟
I2C_InitStructure.I2C_Mode = I2C_Mode_I2C;  // I2C模式
I2C_InitStructure.I2C_DutyCycle = I2C_DutyCycle_16_9;  // 快速模式占空比
I2C_InitStructure.I2C_OwnAddress1 = 0x02;  // 自身地址
I2C_InitStructure.I2C_Ack = I2C_Ack_Enable;  // 应答使能
I2C_InitStructure.I2C_AcknowledgedAddress = I2C_AcknowledgedAddress_7bit;  // 7位地址

I2C_Init(I2C1, &I2C_InitStructure);
I2C_Cmd(I2C1, ENABLE);  // 使能I2C
```

### 4.3 主模式发送流程
1. 等待总线空闲（`I2C_FLAG_BUSY`）
2. 发送起始信号（`I2C_GenerateSTART`）
3. 等待主模式选择事件（`I2C_EVENT_MASTER_MODE_SELECT`）
4. 发送从设备地址（`I2C_Send7bitAddress`）
5. 等待发送器模式选择事件（`I2C_EVENT_MASTER_TRANSMITTER_MODE_SELECTED`）
6. 发送数据（`I2C_SendData`）
7. 等待字节传输完成（`I2C_EVENT_MASTER_BYTE_TRANSMITTED`）
8. 发送停止信号（`I2C_GenerateSTOP`）

### 4.4 从模式接收流程
1. 等待地址匹配事件（`I2C_EVENT_SLAVE_RECEIVER_ADDRESS_MATCHED`）
2. 接收数据（`I2C_ReceiveData`）
3. 等待停止信号（`I2C_FLAG_STOPF`）
4. 清除停止标志

## 5. 示例工程详解

### 5.1 I2C_7bit_Mode（7位地址轮询模式）
- **位置**：`I2C/I2C_7bit_Mode/User/main.c`
- **功能**：7位地址，轮询方式I2C通信
- **适用场景**：初学者学习，简单通信场景
- **特点**：代码简单，易于理解

### 5.2 I2C_7bit_Interrupt_Mode（7位地址中断模式）
- **位置**：`I2C/I2C_7bit_Interrupt_Mode/User/main.c`
- **功能**：7位地址，中断方式I2C通信
- **适用场景**：需要高效处理I2C通信的场景
- **特点**：中断方式，效率高

### 5.3 I2C_10bit_Mode（10位地址模式）
- **位置**：`I2C/I2C_10bit_Mode/User/main.c`
- **功能**：10位地址I2C通信
- **适用场景**：需要连接大量I2C设备的系统
- **特点**：10位地址，支持更多设备

### 5.4 I2C_DMA（DMA传输模式）
- **位置**：`I2C/I2C_DMA/User/main.c`
- **功能**：DMA方式I2C传输
- **适用场景**：大数据量传输，需要释放CPU资源
- **特点**：DMA传输，CPU占用低

### 5.5 I2C_EEPROM（EEPROM操作）
- **位置**：`I2C/I2C_EEPROM/User/main.c`
- **功能**：EEPROM读写操作
- **适用场景**：存储设备操作，如AT24Cxx系列EEPROM
- **特点**：完整的EEPROM驱动，支持读写擦除

### 5.6 I2C_PEC（PEC错误校验）
- **位置**：`I2C/I2C_PEC/User/main.c`
- **功能**：PEC错误校验
- **适用场景**：对数据传输可靠性要求高的场景
- **特点**：PEC校验，提高通信可靠性

## 6. 常见问题

### Q1: I2C引脚配置注意事项？
A: I2C引脚必须配置为开漏输出模式（`GPIO_Mode_AF_OD`），并且总线上需要外部上拉电阻（通常4.7kΩ）。

### Q2: I2C时钟速度如何选择？
A: CH32V307 I2C支持：
- 标准模式：最高100kHz
- 快速模式：最高400kHz
- 快速模式+：最高1MHz（需要硬件支持）

### Q3: I2C地址冲突怎么办？
A: 确保I2C总线上设备地址不冲突：
- 检查设备地址设置
- 使用地址扫描工具检测
- 考虑使用10位地址模式

### Q4: I2C通信超时如何处理？
A: 在实际应用中应添加超时机制：
```c
uint32_t timeout = 100000;  // 超时计数器
while(I2C_GetFlagStatus(I2C1, I2C_FLAG_BUSY) != RESET && timeout--);
if(timeout == 0) {
    // 超时处理
}
```

### Q5: I2C中断优先级如何配置？
A: 合理设置中断优先级：
- 事件中断（I2Cx_EV_IRQn）：处理正常通信事件
- 错误中断（I2Cx_ER_IRQn）：处理通信错误
- 避免中断嵌套影响通信时序

### Q6: DMA传输完成后需要做什么？
A: DMA传输完成后需要：
1. 禁用DMA通道
2. 清除DMA传输完成标志
3. 处理传输完成事件

### Q7: EEPROM写入需要注意什么？
A: EEPROM写入需要：
1. 页写入不能跨页
2. 写入前需要先擦除（某些型号）
3. 写入操作需要延时等待内部写入完成

## 7. 常用I2C事件标志

| 事件标志 | 描述 | 适用模式 |
|---------|------|---------|
| `I2C_EVENT_MASTER_MODE_SELECT` | 主模式选择完成 | 主模式 |
| `I2C_EVENT_MASTER_TRANSMITTER_MODE_SELECTED` | 主发送器模式选择完成 | 主发送模式 |
| `I2C_EVENT_MASTER_RECEIVER_MODE_SELECTED` | 主接收器模式选择完成 | 主接收模式 |
| `I2C_EVENT_MASTER_BYTE_TRANSMITTED` | 主字节传输完成 | 主发送模式 |
| `I2C_EVENT_SLAVE_RECEIVER_ADDRESS_MATCHED` | 从接收器地址匹配 | 从接收模式 |
| `I2C_EVENT_SLAVE_TRANSMITTER_ADDRESS_MATCHED` | 从发送器地址匹配 | 从发送模式 |
| `I2C_FLAG_BUSY` | 总线忙标志 | 所有模式 |
| `I2C_FLAG_TXE` | 发送缓冲区空 | 发送模式 |
| `I2C_FLAG_RXNE` | 接收缓冲区非空 | 接收模式 |
| `I2C_FLAG_STOPF` | 停止标志 | 从模式 |

## 8. API参考

| 函数名 | 功能描述 | 参数说明 |
|--------|----------|----------|
| `I2C_Init` | I2C初始化 | I2Cx：I2C外设，I2C_InitStruct：初始化结构体 |
| `I2C_Cmd` | 使能/禁用I2C | I2Cx：I2C外设，NewState：新状态 |
| `I2C_GenerateSTART` | 生成起始条件 | I2Cx：I2C外设，NewState：新状态 |
| `I2C_GenerateSTOP` | 生成停止条件 | I2Cx：I2C外设，NewState：新状态 |
| `I2C_Send7bitAddress` | 发送7位地址 | I2Cx：I2C外设，Address：地址，Direction：方向 |
| `I2C_SendData` | 发送数据 | I2Cx：I2C外设，Data：数据 |
| `I2C_ReceiveData` | 接收数据 | I2Cx：I2C外设 |
| `I2C_AcknowledgeConfig` | 配置应答 | I2Cx：I2C外设，NewState：新状态 |
| `I2C_GetFlagStatus` | 获取标志位状态 | I2Cx：I2C外设，I2C_FLAG：标志位 |
| `I2C_ClearFlag` | 清除标志位 | I2Cx：I2C外设，I2C_FLAG：标志位 |
| `I2C_ITConfig` | 使能/禁用I2C中断 | I2Cx：I2C外设，I2C_IT：中断源，NewState：新状态 |
| `I2C_GetITStatus` | 获取中断状态 | I2Cx：I2C外设，I2C_IT：中断源 |
| `I2C_ClearITPendingBit` | 清除中断挂起位 | I2Cx：I2C外设，I2C_IT：中断源 |
| `I2C_DMACmd` | 使能/禁用I2C DMA | I2Cx：I2C外设，NewState：新状态 |
| `I2C_CalculatePEC` | 使能/禁用PEC计算 | I2Cx：I2C外设，NewState：新状态 |
| `I2C_TransmitPEC` | 发送PEC | I2Cx：I2C外设，NewState：新状态 |
| `I2C_GetPEC` | 获取PEC值 | I2Cx：I2C外设 |

## 9. 总结

CH32V307的I2C外设功能完善，支持多种工作模式和高级功能。通过合理配置I2C参数，可以实现可靠的低速串行通信。在使用I2C时，需要注意引脚配置、时钟速度、中断处理、DMA使用等关键步骤，确保I2C功能正常工作。不同应用场景可以选择不同的I2C模式，如简单通信使用轮询方式，高效通信使用中断方式，大数据传输使用DMA方式。
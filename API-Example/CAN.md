# CH32V307 CAN外设使用指南

本文档基于CH32V307 EVT示例代码分析，总结CAN外设的使用方法和配置要点。

## 一、概述

CAN（Controller Area Network）是一种广泛应用于汽车电子和工业控制领域的串行通信协议。CH32V307的CAN外设支持CAN 2.0A和2.0B规范。

### 主要特性：
- 支持CAN 2.0A和2.0B规范
- 双CAN接口（CAN1和CAN2，部分型号）
- 波特率最高1Mbps
- 支持标准帧（11位ID）和扩展帧（29位ID）
- 硬件过滤器支持
- 多种工作模式
- 时间触发通信模式

## 二、硬件连接

### 2.1 引脚映射
- **CAN1**：
  - TX：PB9
  - RX：PB8
- **CAN2**（仅CH32V30x_D8C型号）：
  - TX：PB13
  - RX：PB12

### 2.2 外部CAN收发器
CAN控制器需要外部收发器（如TJA1050）连接到物理总线。典型连接方式：
```
CH32V307 CAN_TX → 收发器TXD
CH32V307 CAN_RX ← 收发器RXD
收发器CANH → 总线CAN_H
收发器CANL → 总线CAN_L
```

## 三、软件配置基础

### 3.1 时钟初始化
```c
RCC_APB2PeriphClockCmd(RCC_APB2Periph_AFIO | RCC_APB2Periph_GPIOB, ENABLE);
RCC_APB1PeriphClockCmd(RCC_APB1Periph_CAN1, ENABLE);
```

### 3.2 GPIO配置
```c
GPIO_InitTypeDef GPIO_InitStructure;

// CAN1引脚配置
GPIO_InitStructure.GPIO_Pin = GPIO_Pin_8 | GPIO_Pin_9;  // PB8(RX), PB9(TX)
GPIO_InitStructure.GPIO_Mode = GPIO_Mode_AF_PP;         // 复用推挽输出
GPIO_InitStructure.GPIO_Speed = GPIO_Speed_50MHz;
GPIO_Init(GPIOB, &GPIO_InitStructure);

// 引脚重映射（如果需要）
GPIO_PinRemapConfig(GPIO_Remap1_CAN1, ENABLE);
```

### 3.3 CAN初始化结构体
```c
CAN_InitTypeDef CAN_InitStructure;
CAN_InitStructure.CAN_TTCM = DISABLE;     // 时间触发通信模式
CAN_InitStructure.CAN_ABOM = DISABLE;     // 自动离线管理
CAN_InitStructure.CAN_AWUM = DISABLE;     // 自动唤醒模式
CAN_InitStructure.CAN_NART = ENABLE;      // 非自动重传
CAN_InitStructure.CAN_RFLM = DISABLE;     // 接收FIFO锁定模式
CAN_InitStructure.CAN_TXFP = DISABLE;     // 发送FIFO优先级
CAN_InitStructure.CAN_Mode = CAN_Mode_Normal;  // 工作模式
CAN_InitStructure.CAN_SJW = CAN_SJW_1tq;  // 同步跳跃宽度
CAN_InitStructure.CAN_BS1 = CAN_BS1_6tq;  // 时间段1
CAN_InitStructure.CAN_BS2 = CAN_BS2_5tq;  // 时间段2
CAN_InitStructure.CAN_Prescaler = 16;     // 预分频器
```

## 四、CAN初始化配置

### 4.1 波特率计算
CAN波特率计算公式：
```
Bps = Fpclk1 / ((tbs1 + 1 + tbs2 + 1 + 1) * brp)
```
其中：
- `Fpclk1`：APB1时钟频率（单位：Hz）
- `tbs1`：时间段1的时间份额数
- `tbs2`：时间段2的时间份额数
- `brp`：波特率预分频器值

**250Kbps配置示例**：
```c
// Bps = Fpclk1/((6+1+5+1+1)*16) = 250Kbps
CAN_Mode_Init(CAN_SJW_1tq, CAN_BS2_5tq, CAN_BS1_6tq, 16, CAN_Mode_Normal);
```

**常用波特率配置**：
| 波特率 | SJW | BS1 | BS2 | Prescaler |
|--------|-----|-----|-----|-----------|
| 1Mbps  | 1tq | 5tq | 2tq | 4         |
| 500Kbps| 1tq | 6tq | 5tq | 4         |
| 250Kbps| 1tq | 6tq | 5tq | 8         |
| 125Kbps| 1tq | 6tq | 5tq | 16        |
| 100Kbps| 1tq | 6tq | 5tq | 20        |

### 4.2 工作模式选择

#### 4.2.1 正常模式（CAN_Mode_Normal）
```c
CAN_InitStructure.CAN_Mode = CAN_Mode_Normal;
```
**特点**：正常通信模式，可以发送和接收数据

#### 4.2.2 环回模式（CAN_Mode_LoopBack）
```c
CAN_InitStructure.CAN_Mode = CAN_Mode_LoopBack;
```
**特点**：自发自收，用于测试和调试

#### 4.2.3 静默模式（CAN_Mode_Silent）
```c
CAN_InitStructure.CAN_Mode = CAN_Mode_Silent;
```
**特点**：只能接收不能发送，用于监听总线

#### 4.2.4 静默环回模式（CAN_Mode_Silent_LoopBack）
```c
CAN_InitStructure.CAN_Mode = CAN_Mode_Silent_LoopBack;
```
**特点**：自发自收且不影响总线，用于内部测试

### 4.3 时间触发模式（CAN_TTCM）
```c
CAN_InitStructure.CAN_TTCM = ENABLE;
CAN_TTComModeCmd(CAN1, ENABLE);
```
**特点**：
- 支持时间触发通信
- 提供时间戳功能
- 适合实时性要求高的应用

## 五、过滤器配置

### 5.1 过滤器模式

#### 5.1.1 标识符屏蔽位模式（CAN_FilterMode_IdMask）
```c
CAN_FilterInitStructure.CAN_FilterMode = CAN_FilterMode_IdMask;
```
**特点**：使用屏蔽位过滤，可以过滤一组ID

#### 5.1.2 标识符列表模式（CAN_FilterMode_IdList）
```c
CAN_FilterInitStructure.CAN_FilterMode = CAN_FilterMode_IdList;
```
**特点**：使用列表过滤，只接受列表中的ID

### 5.2 过滤器尺度

#### 5.2.1 32位过滤器（CAN_FilterScale_32bit）
```c
CAN_FilterInitStructure.CAN_FilterScale = CAN_FilterScale_32bit;
```
**特点**：使用两个32位寄存器存储一个扩展ID或两个标准ID

#### 5.2.2 16位过滤器（CAN_FilterScale_16bit）
```c
CAN_FilterInitStructure.CAN_FilterScale = CAN_FilterScale_16bit;
```
**特点**：使用两个32位寄存器存储四个标准ID

### 5.3 过滤器初始化示例

#### 5.3.1 标准帧过滤器配置
```c
CAN_FilterInitTypeDef CAN_FilterInitStructure;
CAN_FilterInitStructure.CAN_FilterNumber = 0;                    // 过滤器编号0
CAN_FilterInitStructure.CAN_FilterMode = CAN_FilterMode_IdMask;  // 屏蔽位模式
CAN_FilterInitStructure.CAN_FilterScale = CAN_FilterScale_32bit; // 32位尺度
CAN_FilterInitStructure.CAN_FilterIdHigh = 0x62E0;               // StdId: 0x317的高16位
CAN_FilterInitStructure.CAN_FilterIdLow = 0;                     // 低16位
CAN_FilterInitStructure.CAN_FilterMaskIdHigh = 0xFFE0;           // 屏蔽位高16位
CAN_FilterInitStructure.CAN_FilterMaskIdLow = 0x0006;            // 屏蔽位低16位
CAN_FilterInitStructure.CAN_FilterFIFOAssignment = CAN_Filter_FIFO0; // 分配到FIFO0
CAN_FilterInitStructure.CAN_FilterActivation = ENABLE;           // 使能过滤器
CAN_FilterInit(&CAN_FilterInitStructure);
```

#### 5.3.2 扩展帧过滤器配置
```c
CAN_FilterInitStructure.CAN_FilterIdHigh = 0x9092;    // ExtId: 0x12124567的高16位
CAN_FilterInitStructure.CAN_FilterIdLow = 0x2B3C;     // ExtId: 0x12124567的低16位
CAN_FilterInitStructure.CAN_FilterMaskIdHigh = 0xFFFF; // 全匹配
CAN_FilterInitStructure.CAN_FilterMaskIdLow = 0xFFFE;  // IDE位必须为1（扩展帧）
```

### 5.4 软件过滤器实现
```c
struct CANFilterStruct_t {
    union {
        union {
            struct { // 扩展帧访问
                uint32_t :1;
                uint32_t RTR :1;
                uint32_t IDE :1;
                uint32_t ExID :29;
            } Access_Ex;
            struct { // 标准帧访问
                uint32_t :1;
                uint32_t RTR :1;
                uint32_t IDE :1;
                uint32_t :18;
                uint32_t StID :11;
            } Access_St;
        };
        union {
            struct {
                uint16_t FR_16_L;
                uint16_t FR_16_H;
            };
            uint32_t FR_32;
        };
    } FR[2];
    union {
        struct {
            uint16_t en :1;    // 使能位
            uint16_t mode :4;  // 过滤器模式
            uint16_t scale :3; // 过滤器尺度
        };
        uint16_t ctrl_byte;
    };
};
```

## 六、数据收发

### 6.1 发送数据结构
```c
CanTxMsg TxMessage;
TxMessage.StdId = 0x317;               // 标准ID
TxMessage.ExtId = 0x12124567;          // 扩展ID
TxMessage.IDE = CAN_Id_Standard;       // 帧类型：标准帧
// TxMessage.IDE = CAN_Id_Extended;    // 帧类型：扩展帧
TxMessage.RTR = CAN_RTR_Data;          // 帧格式：数据帧
// TxMessage.RTR = CAN_RTR_Remote;     // 帧格式：远程帧
TxMessage.DLC = 8;                     // 数据长度：8字节
for(i=0; i<8; i++)
    TxMessage.Data[i] = i;             // 数据内容
```

### 6.2 发送函数
```c
u8 CAN_Send_Msg(u8 *msg, u8 len)
{
    u8 mbox;
    u16 i = 0;
    CanTxMsg TxMessage;

    TxMessage.StdId = 0x317;           // 标准ID
    TxMessage.ExtId = 0x12124567;      // 扩展ID
    TxMessage.IDE = CAN_Id_Standard;   // 标准帧
    TxMessage.RTR = CAN_RTR_Data;      // 数据帧
    TxMessage.DLC = len;               // 数据长度

    for(i=0; i<len; i++)
        TxMessage.Data[i] = msg[i];    // 复制数据

    mbox = CAN_Transmit(CAN1, &TxMessage);  // 发送
    i = 0;
    while((CAN_TransmitStatus(CAN1, mbox) != CANTXOK) && (i<0xFF))
        i++;

    if(i >= 0xFF)
        return 1;  // 发送失败
    return 0;      // 发送成功
}
```

### 6.3 接收函数
```c
u8 CAN_Receive_Msg(u8 *buf)
{
    u32 i;
    CanRxMsg RxMessage;

    if(CAN_MessagePending(CAN1, CAN_FIFO0) == 0)  // 检查是否有数据
        return 0;

    CAN_Receive(CAN1, CAN_FIFO0, &RxMessage);  // 接收数据

    for(i=0; i<RxMessage.DLC; i++)
        buf[i] = RxMessage.Data[i];  // 复制数据

    return RxMessage.DLC;  // 返回数据长度
}
```

## 七、中断处理

### 7.1 中断类型
- **接收中断**：`CAN_IT_FMP0`、`CAN_IT_FMP1`
- **发送中断**：`CAN_IT_TME`
- **错误中断**：`CAN_IT_EWG`、`CAN_IT_EPV`、`CAN_IT_BOF`、`CAN_IT_LEC`、`CAN_IT_ERR`
- **唤醒中断**：`CAN_IT_WKU`
- **睡眠中断**：`CAN_IT_SLK`

### 7.2 中断配置
```c
// 使能接收中断
CAN_ITConfig(CAN1, CAN_IT_FMP0, ENABLE);

// 配置NVIC
NVIC_InitTypeDef NVIC_InitStructure;
NVIC_InitStructure.NVIC_IRQChannel = USB_LP_CAN1_RX0_IRQn;  // CAN1接收中断
NVIC_InitStructure.NVIC_IRQChannelPreemptionPriority = 0;
NVIC_InitStructure.NVIC_IRQChannelSubPriority = 0;
NVIC_InitStructure.NVIC_IRQChannelCmd = ENABLE;
NVIC_Init(&NVIC_InitStructure);
```

### 7.3 中断处理函数
```c
// 接收中断处理
void USB_LP_CAN1_RX0_IRQHandler(void)
{
    CanRxMsg RxMessage;
    u8 buf[8];
    u8 rx_len;

    if(CAN_GetITStatus(CAN1, CAN_IT_FMP0) != RESET)
    {
        CAN_Receive(CAN1, CAN_FIFO0, &RxMessage);

        // 处理接收到的数据
        rx_len = RxMessage.DLC;
        for(u8 i=0; i<rx_len; i++)
            buf[i] = RxMessage.Data[i];

        // 清除中断标志
        CAN_ClearITPendingBit(CAN1, CAN_IT_FMP0);
    }
}

// 发送中断处理
void USB_HP_CAN1_TX_IRQHandler(void)
{
    if(CAN_GetITStatus(CAN1, CAN_IT_TME) != RESET)
    {
        // 发送完成处理
        CAN_ClearITPendingBit(CAN1, CAN_IT_TME);
    }
}
```

## 八、示例工程详解

### 8.1 Networking示例（网络通信示例）

**文件路径**：`CAN\Networking\User\main.c`

**用途**：演示CAN正常模式下的标准帧和扩展帧数据收发，支持软件过滤器和硬件过滤器。

**主要功能**：
- 标准帧和扩展帧数据收发
- 软件过滤器和硬件过滤器配置
- 中断和轮询两种接收方式
- 波特率计算和配置

**关键代码**：
```c
// CAN初始化函数
void CAN_Mode_Init(u8 tsjw, u8 tbs2, u8 tbs1, u16 brp, u8 mode)
{
    CAN_InitTypeDef CAN_InitStructure;
    CAN_FilterInitTypeDef CAN_FilterInitStructure;

    // 时钟和GPIO初始化
    RCC_APB2PeriphClockCmd(RCC_APB2Periph_AFIO | RCC_APB2Periph_GPIOB, ENABLE);
    RCC_APB1PeriphClockCmd(RCC_APB1Periph_CAN1, ENABLE);
    GPIO_PinRemapConfig(GPIO_Remap1_CAN1, ENABLE);

    // CAN参数配置
    CAN_InitStructure.CAN_TTCM = DISABLE;
    CAN_InitStructure.CAN_ABOM = DISABLE;
    CAN_InitStructure.CAN_AWUM = DISABLE;
    CAN_InitStructure.CAN_NART = ENABLE;
    CAN_InitStructure.CAN_RFLM = DISABLE;
    CAN_InitStructure.CAN_TXFP = DISABLE;
    CAN_InitStructure.CAN_Mode = mode;
    CAN_InitStructure.CAN_SJW = tsjw;
    CAN_InitStructure.CAN_BS1 = tbs1;
    CAN_InitStructure.CAN_BS2 = tbs2;
    CAN_InitStructure.CAN_Prescaler = brp;
    CAN_Init(CAN1, &CAN_InitStructure);

    // 过滤器配置
    CAN_FilterInitStructure.CAN_FilterNumber = 0;
    CAN_FilterInitStructure.CAN_FilterMode = CAN_FilterMode_IdMask;
    CAN_FilterInitStructure.CAN_FilterScale = CAN_FilterScale_32bit;
    CAN_FilterInitStructure.CAN_FilterIdHigh = 0x62E0;  // StdId: 0x317
    CAN_FilterInitStructure.CAN_FilterIdLow = 0;
    CAN_FilterInitStructure.CAN_FilterMaskIdHigh = 0xFFE0;
    CAN_FilterInitStructure.CAN_FilterMaskIdLow = 0x0006;
    CAN_FilterInitStructure.CAN_FilterFIFOAssignment = CAN_Filter_FIFO0;
    CAN_FilterInitStructure.CAN_FilterActivation = ENABLE;
    CAN_FilterInit(&CAN_FilterInitStructure);

    // 中断配置
    CAN_ITConfig(CAN1, CAN_IT_FMP0, ENABLE);
    NVIC_EnableIRQ(USB_LP_CAN1_RX0_IRQn);
}
```

### 8.2 TestMode示例（测试模式示例）

**文件路径**：`CAN\TestMode\User\main.c`

**用途**：演示CAN测试模式，包括静默模式、环回模式和静默环回模式，支持双CAN接口。

**主要功能**：
- 所有测试模式演示
- 双CAN接口支持（部分型号）
- 软件过滤器实现
- 全通过滤器配置

**关键代码**：
```c
// 测试模式初始化函数
void CAN_Test_Mode_Init(u8 tsjw, u8 tbs2, u8 tbs1, u16 brp, u8 mode)
{
    // 支持双CAN接口
#if defined (CH32V30x_D8C)
    RCC_APB1PeriphClockCmd(RCC_APB1Periph_CAN2, ENABLE);
#endif

    // CAN模式配置
    CAN_InitStructure.CAN_Mode = mode; // 测试模式

    // 过滤器配置（全通过滤器）
    CAN_FilterInitStructure.CAN_FilterIdHigh = 0;
    CAN_FilterInitStructure.CAN_FilterIdLow = 0;
    CAN_FilterInitStructure.CAN_FilterMaskIdHigh = 0;
    CAN_FilterInitStructure.CAN_FilterMaskIdLow = 0x0006;
}

// 双CAN接口支持
#if defined (CH32V30x_D8C)
// CAN2发送函数
u8 CAN2_Send_Msg(u8 *msg, u8 len)
{
    mbox = CAN_Transmit(CAN2, &CanTxStructure);
    // ...
}

// CAN2接收函数
u8 CAN2_Receive_Msg(u8 *buf)
{
    if(CAN_MessagePending(CAN2, CAN_FIFO0) == 0)
        return 0;
    // ...
}
#endif
```

### 8.3 Time-triggered示例（时间触发通信示例）

**文件路径**：`CAN\Time-triggered\User\main.c`

**用途**：演示CAN时间触发通信模式，支持扩展帧和定时传输。

**主要功能**：
- 时间触发通信模式
- 扩展帧通信
- 定时传输功能
- 发送和接收中断

**关键代码**：
```c
// 时间触发模式初始化
void CAN_Mode_Init(u8 tsjw, u8 tbs2, u8 tbs1, u16 brp, u8 mode)
{
    // 启用时间触发通信模式
    CAN_InitStructure.CAN_TTCM = ENABLE;

    // 扩展帧过滤器配置
    CAN_FilterInitStructure.CAN_FilterMode = CAN_FilterMode_IdMask;
    CAN_FilterInitStructure.CAN_FilterScale = CAN_FilterScale_32bit;
    CAN_FilterInitStructure.CAN_FilterIdHigh = 0x9092;    // ExtId: 0x12124567的高16位
    CAN_FilterInitStructure.CAN_FilterIdLow = 0x2B3C;     // ExtId: 0x12124567的低16位
    CAN_FilterInitStructure.CAN_FilterMaskIdHigh = 0xFFFF;
    CAN_FilterInitStructure.CAN_FilterMaskIdLow = 0xFFFE;

    // 启用时间触发通信
    CAN_TTComModeCmd(CAN1, ENABLE);

    // 中断配置
#if (CAN_MODE == TX_MODE)
    CAN_ITConfig(CAN1, CAN_IT_TME, ENABLE);
    NVIC_InitStructure.NVIC_IRQChannel = USB_HP_CAN1_TX_IRQn;
#elif (CAN_MODE == RX_MODE)
    CAN_ITConfig(CAN1, CAN_IT_FMP0, ENABLE);
    NVIC_InitStructure.NVIC_IRQChannel = USB_LP_CAN1_RX0_IRQn;
#endif
}
```

## 九、常见问题与调试

### 9.1 波特率计算错误
**症状**：CAN通信不稳定或无法通信
**解决**：
1. 确认APB1时钟频率（Fpclk1）
2. 检查波特率计算公式
3. 验证时间份额配置（SJW、BS1、BS2、Prescaler）
4. 使用示波器测量实际波特率

### 9.2 过滤器配置问题
**症状**：无法接收预期数据或接收不到数据
**解决**：
1. 检查过滤器模式（IdMask/IdList）
2. 验证ID和屏蔽位设置
3. 确认过滤器使能状态
4. 检查FIFO分配是否正确

### 9.3 中断不触发
**症状**：中断处理函数不执行
**解决**：
1. 检查中断使能配置
2. 验证NVIC优先级设置
3. 确认中断标志清除时机
4. 检查中断向量表配置

### 9.4 数据收发失败
**症状**：发送或接收数据失败
**解决**：
1. 检查CAN工作模式
2. 验证总线连接和终端电阻
3. 确认外部收发器工作正常
4. 检查CAN控制器状态寄存器

### 9.5 双CAN接口问题
**症状**：CAN2无法正常工作
**解决**：
1. 确认芯片型号支持CAN2
2. 检查CAN2时钟使能
3. 验证CAN2引脚配置
4. 检查CAN2过滤器配置

## 十、最佳实践

### 10.1 过滤器配置建议
1. **合理使用屏蔽位**：根据需要过滤的ID范围设置屏蔽位
2. **优先级分配**：重要消息使用高优先级过滤器
3. **FIFO分配**：不同类型消息分配到不同FIFO
4. **软件过滤补充**：硬件过滤器不足时使用软件过滤

### 10.2 中断使用建议
1. **合理分配优先级**：根据实时性要求设置中断优先级
2. **中断处理精简**：中断处理函数尽量简短
3. **标志及时清除**：中断退出前清除相应标志
4. **错误处理完善**：实现完整的错误处理机制

### 10.3 错误处理机制
```c
// 错误状态检查
void CAN_Error_Handler(void)
{
    uint32_t error_status = CAN_GetErrorStatus(CAN1);

    if(error_status & CAN_ErrorStatus_EWG)
        printf("Error Warning\n");
    if(error_status & CAN_ErrorStatus_EPV)
        printf("Error Passive\n");
    if(error_status & CAN_ErrorStatus_BOF)
        printf("Bus Off\n");
    if(error_status & CAN_ErrorStatus_LEC)
    {
        switch(CAN_GetLastErrorCode(CAN1))
        {
            case CAN_ERRORCODE_NoError:
                break;
            case CAN_ERRORCODE_StuffError:
                printf("Stuff Error\n");
                break;
            case CAN_ERRORCODE_FormError:
                printf("Form Error\n");
                break;
            case CAN_ERRORCODE_AckError:
                printf("Ack Error\n");
                break;
            case CAN_ERRORCODE_BitRecessiveError:
                printf("Bit Recessive Error\n");
                break;
            case CAN_ERRORCODE_BitDominantError:
                printf("Bit Dominant Error\n");
                break;
            case CAN_ERRORCODE_CRCError:
                printf("CRC Error\n");
                break;
        }
    }
}
```

### 10.4 性能优化建议
1. **合理设置波特率**：根据通信距离和干扰情况选择
2. **使用时间触发模式**：提高实时性要求高的应用性能
3. **优化过滤器配置**：减少不必要的消息处理
4. **合理使用中断**：平衡实时性和CPU负载

## 十一、API函数参考

### 11.1 初始化函数
- `void CAN_Init(CAN_TypeDef* CANx, CAN_InitTypeDef* CAN_InitStruct)`
- `void CAN_FilterInit(CAN_FilterInitTypeDef* CAN_FilterInitStruct)`
- `void CAN_StructInit(CAN_InitTypeDef* CAN_InitStruct)`

### 11.2 控制函数
- `void CAN_Cmd(CAN_TypeDef* CANx, FunctionalState NewState)`
- `void CAN_Sleep(CAN_TypeDef* CANx)`
- `void CAN_WakeUp(CAN_TypeDef* CANx)`
- `void CAN_TTComModeCmd(CAN_TypeDef* CANx, FunctionalState NewState)`

### 11.3 数据收发函数
- `uint8_t CAN_Transmit(CAN_TypeDef* CANx, CanTxMsg* TxMessage)`
- `void CAN_TransmitStatus(CAN_TypeDef* CANx, uint8_t TransmitMailbox)`
- `void CAN_CancelTransmit(CAN_TypeDef* CANx, uint8_t Mailbox)`
- `void CAN_Receive(CAN_TypeDef* CANx, uint8_t FIFONumber, CanRxMsg* RxMessage)`
- `void CAN_FIFORelease(CAN_TypeDef* CANx, uint8_t FIFONumber)`
- `uint8_t CAN_MessagePending(CAN_TypeDef* CANx, uint8_t FIFONumber)`

### 11.4 中断函数
- `void CAN_ITConfig(CAN_TypeDef* CANx, uint32_t CAN_IT, FunctionalState NewState)`
- `ITStatus CAN_GetITStatus(CAN_TypeDef* CANx, uint32_t CAN_IT)`
- `void CAN_ClearITPendingBit(CAN_TypeDef* CANx, uint32_t CAN_IT)`

### 11.5 状态函数
- `uint8_t CAN_GetFlagStatus(CAN_TypeDef* CANx, uint32_t CAN_FLAG)`
- `void CAN_ClearFlag(CAN_TypeDef* CANx, uint32_t CAN_FLAG)`
- `uint32_t CAN_GetErrorStatus(CAN_TypeDef* CANx)`
- `uint8_t CAN_GetLastErrorCode(CAN_TypeDef* CANx)`

## 十二、寄存器摘要

### 12.1 关键寄存器说明
- **CAN_MCR**：主控制寄存器
  - INRQ：初始化请求
  - SLEEP：睡眠模式
  - TXFP：发送FIFO优先级
  - RFLM：接收FIFO锁定模式
  - NART：非自动重传
  - AWUM：自动唤醒模式
  - ABOM：自动离线管理
  - TTCM：时间触发通信模式

- **CAN_MSR**：主状态寄存器
  - INAK：初始化确认
  - SLAK：睡眠确认
  - ERRI：错误中断
  - WKUI：唤醒中断
  - SLAKI：睡眠确认中断
  - TXM：发送模式
  - RXM：接收模式
  - SAMP：上次采样值
  - RX：接收电平

- **CAN_TSR**：发送状态寄存器
  - RQCPx：邮箱x请求完成
  - TXOKx：邮箱x发送成功
  - ALSTx：邮箱x仲裁丢失
  - TERRx：邮箱x发送错误
  - ABRQx：邮箱x中止请求
  - CODE：邮箱号

- **CAN_RF0R/RF1R**：接收FIFO寄存器
  - FMPx：FIFOx消息 pending
  - FULLx：FIFOx满
  - FOVRx：FIFOx溢出
  - RFOMx：释放FIFOx输出邮箱

### 12.2 编程注意事项
1. **初始化顺序**：先配置过滤器，再使能CAN
2. **模式切换**：切换模式前需进入初始化模式
3. **中断清理**：中断处理中必须清除相应标志
4. **错误处理**：定期检查错误状态并处理
5. **双CAN操作**：注意CAN1和CAN2的寄存器地址差异

## 总结

CH32V307的CAN外设功能完善，支持多种工作模式和灵活的过滤器配置。关键配置要点包括波特率计算、工作模式选择、过滤器配置和中断管理。三个示例工程分别展示了网络通信、测试模式和时间触发通信等典型应用场景。

建议开发者在实际应用中重点关注以下几点：
1. 准确计算波特率参数
2. 合理配置硬件过滤器
3. 完善错误处理机制
4. 根据应用需求选择合适的工作模式
5. 优化中断处理提高系统实时性
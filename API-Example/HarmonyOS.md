# CH32V307 HarmonyOS鸿蒙操作系统使用指南

## 1. 外设概述

HarmonyOS（鸿蒙操作系统）是华为推出的分布式操作系统。CH32V307提供了完整的HarmonyOS LiteOS-m移植，支持在RISC-V架构上运行鸿蒙操作系统。主要特性包括：

- **实时内核**：基于LiteOS-m的实时操作系统内核
- **任务管理**：支持多任务创建、调度和管理
- **内存管理**：动态内存分配和栈管理
- **中断管理**：优化的中断处理机制
- **IPC通信**：信号量、互斥锁、消息队列等进程间通信机制
- **外设集成**：完整集成CH32V307外设驱动
- **调试支持**：串口调试输出和任务信息显示

## 2. 关键代码片段

### 2.1 HarmonyOS初始化

```c
LITE_OS_SEC_TEXT_INIT int main(void)
{
    unsigned int ret;

    // 1. 系统初始化
    NVIC_PriorityGroupConfig(NVIC_PriorityGroup_2);
    SystemCoreClockUpdate();
    Delay_Init();
    USART_Printf_Init(115200);

    printf("SystemClk:%d\r\n",SystemCoreClock);
    printf("ChipID:%08x\r\n", DBGMCU_GetCHIPID());

    // 2. HarmonyOS内核初始化
    ret = LOS_KernelInit();

    // 3. 创建示例任务
    taskSample();

    // 4. 启动内核调度
    if (ret == LOS_OK)
    {
        LOS_Start();
    }

    while (1) {
        __asm volatile("nop");
    }
}
```

### 2.2 任务创建示例

```c
UINT32 taskSample(VOID)
{
    UINT32  uwRet;
    UINT32 taskID1,taskID2;
    TSK_INIT_PARAM_S stTask={0};

    // 任务1配置
    stTask.pfnTaskEntry = (TSK_ENTRY_FUNC)taskSampleEntry1;
    stTask.uwStackSize  = 0X500;      // 任务栈大小
    stTask.pcName       = "taskSampleEntry1";
    stTask.usTaskPrio   = 6;          // 任务优先级
    uwRet = LOS_TaskCreate(&taskID1, &stTask);

    // 任务2配置
    stTask.pfnTaskEntry = (TSK_ENTRY_FUNC)taskSampleEntry2;
    stTask.uwStackSize  = 0X500;
    stTask.pcName       = "taskSampleEntry2";
    stTask.usTaskPrio   = 7;          // 更高优先级
    uwRet = LOS_TaskCreate(&taskID2, &stTask);

    // 初始化外部中断
    EXTI0_INT_INIT();

    return LOS_OK;
}
```

### 2.3 任务函数示例

```c
void taskSampleEntry1(void)
{
    while(1)
    {
        LED1_TOGGLE();                // 翻转LED1
        printf("taskSampleEntry1 Running!\r\n");
        LOS_TaskDelay(500);           // 延时500ms
    }
}

void taskSampleEntry2(void)
{
    while(1)
    {
        LED2_TOGGLE();                // 翻转LED2
        printf("taskSampleEntry2 Running!\r\n");
        LOS_TaskDelay(1000);          // 延时1000ms
    }
}
```

### 2.4 HarmonyOS中断处理

```c
// WCH RISC-V快速中断处理（需要特殊属性）
void EXTI0_IRQHandler(void) __attribute__((interrupt("WCH-Interrupt-fast")));
void EXTI0_IRQHandler(void)
{
    // 1. 获取中断栈指针
    GET_INT_SP();

    // 2. 进入中断处理
    HalIntEnter();

    // 3. 检查中断标志
    if(EXTI_GetITStatus(EXTI_Line0)!=RESET)
    {
        // 获取当前栈指针
        g_VlaueSp= __get_SP();
        printf("Run at EXTI:");
        printf("interruption sp:%08x\r\n",g_VlaueSp);

        // 显示任务信息
        HalDisplayTaskInfo();

        // 清除中断标志
        EXTI_ClearITPendingBit(EXTI_Line0);
    }

    // 4. 退出中断处理
    HalIntExit();

    // 5. 释放中断栈
    FREE_INT_SP();
}
```

### 2.5 外部中断配置

```c
void EXTI0_INT_INIT(void)
{
    GPIO_InitTypeDef  GPIO_InitStructure={0};
    EXTI_InitTypeDef EXTI_InitStructure={0};
    NVIC_InitTypeDef NVIC_InitStructure={0};

    // 1. 使能时钟
    RCC_APB2PeriphClockCmd(RCC_APB2Periph_AFIO|RCC_APB2Periph_GPIOA,ENABLE);

    // 2. GPIO配置（PA0作为输入）
    GPIO_InitStructure.GPIO_Pin = GPIO_Pin_0;
    GPIO_InitStructure.GPIO_Mode = GPIO_Mode_IPU;  // 上拉输入
    GPIO_Init(GPIOA, &GPIO_InitStructure);

    // 3. EXTI线配置
    GPIO_EXTILineConfig(GPIO_PortSourceGPIOA,GPIO_PinSource0);

    // 4. EXTI配置
    EXTI_InitStructure.EXTI_Line = EXTI_Line0;
    EXTI_InitStructure.EXTI_Mode = EXTI_Mode_Interrupt;
    EXTI_InitStructure.EXTI_Trigger = EXTI_Trigger_Falling;  // 下降沿触发
    EXTI_InitStructure.EXTI_LineCmd = ENABLE;
    EXTI_Init(&EXTI_InitStructure);

    // 5. NVIC中断配置
    NVIC_InitStructure.NVIC_IRQChannel = EXTI0_IRQn;
    NVIC_InitStructure.NVIC_IRQChannelPreemptionPriority = 0;
    NVIC_InitStructure.NVIC_IRQChannelSubPriority = 4;
    NVIC_InitStructure.NVIC_IRQChannelCmd = ENABLE;
    NVIC_Init(&NVIC_InitStructure);
}
```

## 3. 配置数据结构

### 3.1 HarmonyOS系统配置（target_config.h）

```c
// 系统时钟配置
#define OS_SYS_CLOCK                                        (SystemCoreClock)
#define LOSCFG_BASE_CORE_TICK_PER_SECOND                    (1000UL)  // 系统时钟频率1kHz
#define LOSCFG_BASE_CORE_TICK_HW_TIME                       1         // 硬件定时器
#define LOSCFG_BASE_CORE_TICK_WTIMER                        0         // 不使用WTIMER

// 任务配置
#define LOSCFG_BASE_CORE_TSK_LIMIT                          12        // 最大任务数
#define LOSCFG_BASE_CORE_TSK_IDLE_STACK_SIZE                (0x500U)  // 空闲任务栈大小
#define LOSCFG_BASE_CORE_TSK_DEFAULT_STACK_SIZE             (0x2D0U)  // 默认任务栈大小
#define LOSCFG_BASE_CORE_TSK_MIN_STACK_SIZE                 (0x100U)  // 最小任务栈大小

// 内存配置
#define LOSCFG_SYS_HEAP_SIZE                                0x04000UL // 系统堆大小(16KB)
#define LOSCFG_MEM_BESTFIT                                   1         // 最佳匹配算法

// IPC配置
#define LOSCFG_BASE_IPC_SEM                                 1         // 使能信号量
#define LOSCFG_BASE_IPC_MUX                                 1         // 使能互斥锁
#define LOSCFG_BASE_IPC_QUEUE                               1         // 使能消息队列
#define LOSCFG_BASE_IPC_EVENT                               1         // 使能事件

// 定时器配置
#define LOSCFG_BASE_CORE_SWTMR                              1         // 使能软件定时器
#define LOSCFG_BASE_CORE_SWTMR_LIMIT                        16        // 最大软件定时器数
```

### 3.2 任务初始化参数结构体

```c
typedef struct tagTskInitParam {
    TSK_ENTRY_FUNC pfnTaskEntry;      // 任务入口函数
    UINT16         usTaskPrio;        // 任务优先级
    UINT16         uwStackSize;       // 任务栈大小
    CHAR           *pcName;           // 任务名称
    UINT32         uwResved;          // 保留字段
} TSK_INIT_PARAM_S;
```

### 3.3 CH32V307系统配置

```c
// 系统时钟配置（system_ch32v30x.c）
#define SYSCLK_FREQ_96MHz_HSE  96000000  // 使用96MHz系统时钟

// 中断优先级分组
#define NVIC_PriorityGroup_2    ((uint32_t)0x02)  // 2位抢占优先级，2位子优先级
```

## 4. 通用配置步骤

### 4.1 HarmonyOS系统初始化步骤

1. **系统时钟初始化**：
   ```c
   NVIC_PriorityGroupConfig(NVIC_PriorityGroup_2);
   SystemCoreClockUpdate();
   Delay_Init();
   USART_Printf_Init(115200);
   ```

2. **HarmonyOS内核初始化**：
   ```c
   ret = LOS_KernelInit();
   if (ret != LOS_OK) {
       // 初始化失败处理
   }
   ```

3. **创建应用程序任务**：
   ```c
   taskSample();  // 创建示例任务
   ```

4. **启动内核调度**：
   ```c
   if (ret == LOS_OK) {
       LOS_Start();
   }
   ```

### 4.2 任务创建步骤

1. **定义任务初始化参数**：
   ```c
   TSK_INIT_PARAM_S stTask={0};
   stTask.pfnTaskEntry = (TSK_ENTRY_FUNC)taskFunction;
   stTask.uwStackSize  = 0x500;
   stTask.pcName       = "TaskName";
   stTask.usTaskPrio   = priority;
   ```

2. **创建任务**：
   ```c
   UINT32 taskID;
   uwRet = LOS_TaskCreate(&taskID, &stTask);
   if (uwRet != LOS_OK) {
       // 任务创建失败处理
   }
   ```

3. **任务函数实现**：
   ```c
   void taskFunction(void)
   {
       while(1) {
           // 任务处理逻辑
           LOS_TaskDelay(delay_ms);  // 任务延时
       }
   }
   ```

### 4.3 中断处理配置步骤

1. **中断处理函数声明**（WCH特殊要求）：
   ```c
   void IRQ_Handler(void) __attribute__((interrupt("WCH-Interrupt-fast")));
   ```

2. **中断处理函数实现**：
   ```c
   void IRQ_Handler(void)
   {
       GET_INT_SP();      // 获取中断栈
       HalIntEnter();     // 进入中断

       // 中断处理逻辑

       HalIntExit();      // 退出中断
       FREE_INT_SP();     // 释放中断栈
   }
   ```

3. **中断控制器配置**：
   ```c
   // 配置NVIC优先级
   NVIC_InitStructure.NVIC_IRQChannel = IRQn;
   NVIC_InitStructure.NVIC_IRQChannelPreemptionPriority = preempt_priority;
   NVIC_InitStructure.NVIC_IRQChannelSubPriority = sub_priority;
   NVIC_InitStructure.NVIC_IRQChannelCmd = ENABLE;
   NVIC_Init(&NVIC_InitStructure);
   ```

## 5. 示例工程详解

### 5.1 HarmonyOS示例

- **位置**：`HarmonyOS/LiteOS_m/User/main.c`
- **功能**：HarmonyOS LiteOS-m在CH32V307上的移植示例
- **特点**：
  - 完整的HarmonyOS内核初始化
  - 多任务创建和管理
  - 优化的中断处理机制
  - 串口调试输出支持
  - 任务信息显示功能
- **适用场景**：
  - 需要在CH32V307上运行HarmonyOS的应用
  - 多任务实时系统开发
  - 需要操作系统支持的应用

### 5.2 关键功能点

1. **任务调度**：支持基于优先级的抢占式调度
2. **中断处理**：优化的WCH RISC-V中断处理机制
3. **内存管理**：动态内存分配和栈管理
4. **IPC通信**：信号量、互斥锁、消息队列等
5. **时间管理**：软件定时器和延时功能

## 6. 常见问题

### Q1: HarmonyOS内核初始化失败？
A: 检查以下配置：
1. 系统时钟配置是否正确
2. 内存堆配置是否足够
3. 中断优先级分组是否正确设置
4. 编译工具链是否支持RISC-V架构

### Q2: 任务创建失败？
A: 可能原因：
1. 任务栈大小不足
2. 超出最大任务数限制
3. 内存堆空间不足
4. 任务优先级配置错误

### Q3: 中断处理异常？
A: 检查以下配置：
1. 中断处理函数是否正确使用`__attribute__((interrupt("WCH-Interrupt-fast")))`
2. 是否使用了`GET_INT_SP()`和`FREE_INT_SP()`宏
3. 中断优先级配置是否正确
4. 中断标志是否清除

### Q4: 系统运行不稳定？
A: 优化建议：
1. 调整任务栈大小
2. 优化任务优先级
3. 增加系统堆大小
4. 检查中断嵌套深度

### Q5: 编译链接错误？
A: 检查以下配置：
1. 链接脚本是否正确配置内存布局
2. 启动文件是否适配CH32V307
3. 编译器选项是否正确设置
4. 是否包含必要的库文件

## 7. API参考

### 7.1 HarmonyOS LiteOS API

| 函数名 | 功能描述 | 参数说明 |
|--------|----------|----------|
| `LOS_KernelInit` | 内核初始化 | 无 |
| `LOS_Start` | 启动内核调度 | 无 |
| `LOS_TaskCreate` | 创建任务 | taskID：任务ID指针，initParam：初始化参数 |
| `LOS_TaskDelay` | 任务延时 | tick：延时时间（系统节拍数） |
| `LOS_TaskDelete` | 删除任务 | taskID：任务ID |
| `HalIntEnter` | 进入中断处理 | 无 |
| `HalIntExit` | 退出中断处理 | 无 |
| `HalDisplayTaskInfo` | 显示任务信息 | 无 |

### 7.2 WCH RISC-V特殊API

| 宏/函数 | 功能描述 | 说明 |
|---------|----------|------|
| `GET_INT_SP()` | 获取中断栈指针 | WCH RISC-V专用 |
| `FREE_INT_SP()` | 释放中断栈 | WCH RISC-V专用 |
| `__attribute__((interrupt("WCH-Interrupt-fast")))` | 快速中断属性 | 中断处理函数必须使用 |

### 7.3 CH32V307外设API

| 函数名 | 功能描述 | 参数说明 |
|--------|----------|----------|
| `SystemCoreClockUpdate` | 更新系统时钟 | 无 |
| `NVIC_PriorityGroupConfig` | 配置中断优先级分组 | PriorityGroup：优先级分组 |
| `USART_Printf_Init` | 串口初始化 | bound：波特率 |
| `EXTI_Init` | 外部中断初始化 | EXTI_InitStruct：初始化结构体 |
| `GPIO_Init` | GPIO初始化 | GPIO_InitStruct：初始化结构体 |

## 8. 注意事项

1. **中断处理特殊性**：WCH RISC-V处理器需要特殊的中断处理属性
2. **栈管理**：需要合理配置任务栈大小，避免栈溢出
3. **内存分配**：动态内存分配来自系统堆，需要合理配置堆大小
4. **中断优先级**：合理配置中断优先级，避免优先级反转
5. **实时性**：注意任务执行时间和中断响应时间，确保实时性要求
6. **调试支持**：充分利用串口调试输出，便于问题定位

## 9. 性能优化建议

1. **任务栈优化**：根据任务实际需求合理分配栈大小
2. **优先级配置**：合理配置任务优先级，提高系统响应性
3. **中断优化**：中断处理函数尽量简短，减少中断关闭时间
4. **内存管理**：避免频繁的内存分配和释放，减少内存碎片
5. **系统节拍**：根据应用需求合理配置系统节拍频率
6. **功耗管理**：在空闲任务中进入低功耗模式

## 10. 典型应用场景

### 10.1 多任务数据采集系统
```c
// 数据采集任务
void DataAcquisitionTask(void)
{
    while(1) {
        // 采集传感器数据
        ReadSensorData();

        // 发送到数据处理任务
        SendDataToProcessTask();

        LOS_TaskDelay(10);  // 10ms采集间隔
    }
}

// 数据处理任务
void DataProcessTask(void)
{
    while(1) {
        // 等待数据
        WaitForData();

        // 处理数据
        ProcessData();

        // 存储或发送结果
        SaveOrSendResult();
    }
}
```

### 10.2 实时控制系统
```c
// 控制任务
void ControlTask(void)
{
    while(1) {
        // 读取控制输入
        ReadControlInput();

        // 执行控制算法
        ExecuteControlAlgorithm();

        // 输出控制信号
        OutputControlSignal();

        LOS_TaskDelay(1);  // 1ms控制周期
    }
}

// 监控任务
void MonitorTask(void)
{
    while(1) {
        // 监控系统状态
        MonitorSystemStatus();

        // 异常处理
        HandleExceptions();

        // 显示状态信息
        DisplayStatus();

        LOS_TaskDelay(100);  // 100ms监控周期
    }
}
```

### 10.3 通信网关系统
```c
// 串口接收任务
void UartReceiveTask(void)
{
    while(1) {
        // 接收串口数据
        ReceiveUartData();

        // 解析协议
        ParseProtocol();

        // 转发到网络任务
        ForwardToNetworkTask();
    }
}

// 网络发送任务
void NetworkSendTask(void)
{
    while(1) {
        // 等待发送数据
        WaitForSendData();

        // 封装网络协议
        PackageNetworkProtocol();

        // 发送数据
        SendNetworkData();
    }
}
```

## 11. 总结

CH32V307的HarmonyOS移植示例展示了在RISC-V架构MCU上运行鸿蒙操作系统的完整方案。通过合理配置HarmonyOS内核参数和CH32V307外设，可以实现高效的多任务实时系统。开发时需要注意WCH RISC-V处理器的特殊中断处理要求、合理的内存管理和任务调度配置，确保系统稳定可靠运行。HarmonyOS提供了丰富的任务管理、IPC通信和时间管理功能，可以满足复杂的嵌入式应用需求。
# CH32V307 TencentOS Tiny实时操作系统使用指南

## 1. 外设概述

TencentOS Tiny是腾讯开源的实时操作系统（RTOS），专为资源受限的物联网设备设计。在CH32V307上的移植实现了完整的RTOS功能。主要特性包括：

- **轻量级内核**：最小内核镜像可小于1KB
- **多任务管理**：支持任务创建、删除、挂起、恢复
- **任务通信**：信号量、互斥锁、消息队列、事件标志
- **内存管理**：静态内存池和动态内存堆管理
- **时间管理**：软件定时器、任务延时
- **RISC-V优化**：专为RISC-V架构优化的上下文切换
- **低功耗支持**：Tickless模式支持
- **组件丰富**：支持文件系统、网络协议栈等组件

## 2. 关键代码片段

### 2.1 TencentOS Tiny基本使用示例

```c
#include "ch32v30x.h"
#include "debug.h"
#include "tos_k.h"

// 任务栈定义
#define TASK1_STK_SIZE       1024
k_task_t task1;
__aligned(4) uint8_t task1_stk[TASK1_STK_SIZE];

#define TASK2_STK_SIZE       1024
k_task_t task2;
__aligned(4) uint8_t task2_stk[TASK2_STK_SIZE];

// 任务1入口函数
void task1_entry(void *arg)
{
    while (1)
    {
        printf("###I am task1\r\n");
        tos_task_delay(2000);  // 延迟2秒
    }
}

// 任务2入口函数
void task2_entry(void *arg)
{
    while (1)
    {
        printf("***I am task2\r\n");
        tos_task_delay(1000);  // 延迟1秒
    }
}

int main(void)
{
    // 系统初始化
    USART_Printf_Init(115200);
    SystemCoreClockUpdate();
    printf("SystemClk:%d\r\n",SystemCoreClock);
    printf("ChipID:%08x\r\n", DBGMCU_GetCHIPID());
    printf("Welcome to TencentOS tiny(%s)\r\n", TOS_VERSION);

    // 内核初始化
    tos_knl_init();

    // 创建任务
    // 参数说明：任务句柄, 任务名, 入口函数, 参数, 优先级, 栈空间, 栈大小, 时间片
    tos_task_create(&task1, "task1", task1_entry, NULL, 3, task1_stk, TASK1_STK_SIZE, 0);
    tos_task_create(&task2, "task2", task2_entry, NULL, 3, task2_stk, TASK2_STK_SIZE, 0);

    // 启动内核调度
    tos_knl_start();

    // 以下代码不会执行
    printf("should not run at here!\r\n");
    while(1) { asm("nop"); }
}
```

### 2.2 信号量使用示例

```c
#include "tos_k.h"

// 定义信号量
k_sem_t sem_example;

// 任务1：生产者
void producer_task(void *arg)
{
    while(1)
    {
        // 生产数据...
        printf("Producer: data produced\r\n");

        // 释放信号量
        tos_sem_post(&sem_example);

        tos_task_delay(1000);  // 延迟1秒
    }
}

// 任务2：消费者
void consumer_task(void *arg)
{
    while(1)
    {
        // 等待信号量（最多等待2000个tick）
        if(tos_sem_pend(&sem_example, 2000) == K_ERR_NONE)
        {
            printf("Consumer: data consumed\r\n");
            // 消费数据...
        }
        else
        {
            printf("Consumer: wait timeout\r\n");
        }
    }
}

// 初始化函数
void semaphore_example_init(void)
{
    // 初始化二进制信号量，初始值为0
    tos_sem_create(&sem_example, 0);

    // 创建生产者任务
    tos_task_create(&producer, "producer", producer_task, NULL, 2, producer_stk, 1024, 0);

    // 创建消费者任务
    tos_task_create(&consumer, "consumer", consumer_task, NULL, 3, consumer_stk, 1024, 0);
}
```

### 2.3 互斥锁使用示例

```c
#include "tos_k.h"

// 定义互斥锁和共享资源
k_mutex_t mutex_example;
int shared_counter = 0;

// 任务1：递增计数器
void increment_task(void *arg)
{
    while(1)
    {
        // 获取互斥锁
        if(tos_mutex_pend(&mutex_example) == K_ERR_NONE)
        {
            // 临界区开始
            shared_counter++;
            printf("Task1: counter = %d\r\n", shared_counter);
            // 临界区结束

            // 释放互斥锁
            tos_mutex_post(&mutex_example);
        }

        tos_task_delay(500);  // 延迟500ms
    }
}

// 任务2：递减计数器
void decrement_task(void *arg)
{
    while(1)
    {
        // 获取互斥锁
        if(tos_mutex_pend(&mutex_example) == K_ERR_NONE)
        {
            // 临界区开始
            shared_counter--;
            printf("Task2: counter = %d\r\n", shared_counter);
            // 临界区结束

            // 释放互斥锁
            tos_mutex_post(&mutex_example);
        }

        tos_task_delay(700);  // 延迟700ms
    }
}

// 互斥锁示例初始化
void mutex_example_init(void)
{
    // 初始化互斥锁
    tos_mutex_create(&mutex_example);

    // 创建任务
    tos_task_create(&task_inc, "inc", increment_task, NULL, 2, inc_stk, 1024, 0);
    tos_task_create(&task_dec, "dec", decrement_task, NULL, 3, dec_stk, 1024, 0);
}
```

### 2.4 消息队列使用示例

```c
#include "tos_k.h"

// 定义消息队列
#define QUEUE_SIZE    10
k_queue_t queue_example;
uint8_t queue_buffer[QUEUE_SIZE * sizeof(int)];

// 任务1：消息发送者
void sender_task(void *arg)
{
    int message = 0;

    while(1)
    {
        // 准备消息
        message++;
        printf("Sender: sending message %d\r\n", message);

        // 发送消息到队列（非阻塞）
        if(tos_queue_post(&queue_example, &message, sizeof(message)) == K_ERR_NONE)
        {
            printf("Sender: message %d sent successfully\r\n", message);
        }
        else
        {
            printf("Sender: queue full\r\n");
        }

        tos_task_delay(1000);  // 延迟1秒
    }
}

// 任务2：消息接收者
void receiver_task(void *arg)
{
    int received_message;
    size_t msg_size;

    while(1)
    {
        // 从队列接收消息（最多等待2000个tick）
        if(tos_queue_pend(&queue_example, &received_message, &msg_size, 2000) == K_ERR_NONE)
        {
            printf("Receiver: got message %d (size: %d)\r\n", received_message, msg_size);
        }
        else
        {
            printf("Receiver: queue empty or timeout\r\n");
        }
    }
}

// 消息队列示例初始化
void queue_example_init(void)
{
    // 初始化消息队列
    tos_queue_create(&queue_example, queue_buffer, QUEUE_SIZE, sizeof(int));

    // 创建任务
    tos_task_create(&sender, "sender", sender_task, NULL, 2, sender_stk, 1024, 0);
    tos_task_create(&receiver, "receiver", receiver_task, NULL, 3, receiver_stk, 1024, 0);
}
```

### 2.5 软件定时器使用示例

```c
#include "tos_k.h"

// 定义软件定时器
k_timer_t timer_once;      // 单次定时器
k_timer_t timer_periodic;  // 周期定时器

// 定时器回调函数
void timer_once_callback(void *arg)
{
    printf("One-shot timer expired!\r\n");
    // 执行单次任务...
}

void timer_periodic_callback(void *arg)
{
    static int count = 0;
    count++;
    printf("Periodic timer tick %d\r\n", count);
    // 执行周期性任务...
}

// 定时器示例初始化
void timer_example_init(void)
{
    // 创建单次定时器（2000ms后触发一次）
    tos_timer_create(&timer_once, 2000, 0, timer_once_callback, NULL, TIMER_OPT_ONE_SHOT);

    // 创建周期定时器（每1000ms触发一次）
    tos_timer_create(&timer_periodic, 1000, 1000, timer_periodic_callback, NULL, TIMER_OPT_PERIODIC);

    // 启动定时器
    tos_timer_start(&timer_once);
    tos_timer_start(&timer_periodic);

    printf("Timers started\r\n");
}
```

## 3. 配置数据结构

### 3.1 TencentOS Tiny配置头文件

```c
// tos_config.h - 主要配置参数
#define TOS_CFG_CPU_TICK_PER_SECOND     1000u  // 系统时钟频率：1000Hz
#define TOS_CFG_CPU_CLOCK               (SystemCoreClock)  // CPU时钟频率

// 任务配置
#define TOS_CFG_TASK_PRIO_MAX           10u    // 最大任务优先级数
#define TOS_CFG_TASK_DYNAMIC_STACK_EN   0u     // 动态任务栈支持

// 调度配置
#define TOS_CFG_ROUND_ROBIN_EN          0u     // 时间片轮转调度
#define TOS_CFG_ROUND_ROBIN_SLICE       5u     // 时间片大小（tick数）

// 内核组件配置
#define TOS_CFG_EVENT_EN                1u     // 事件功能
#define TOS_CFG_MMHEAP_EN               1u     // 内存堆管理
#define TOS_CFG_MMHEAP_POOL_SIZE        0x4000 // 内存堆大小
#define TOS_CFG_MUTEX_EN                1u     // 互斥锁
#define TOS_CFG_SEM_EN                  1u     // 信号量
#define TOS_CFG_TIMER_EN                1u     // 软件定时器
#define TOS_CFG_QUEUE_EN                1u     // 消息队列
#define TOS_CFG_MSG_POOL_SIZE           10u    // 消息池大小

// 调试配置
#define TOS_CFG_OBJ_DYNAMIC_CREATE_EN   0u     // 动态对象创建
#define TOS_CFG_TASK_STACK_DRAUGHT_DEPTH_DETACT_EN 0u  // 栈溢出检测
```

### 3.2 任务控制块结构

```c
// 任务控制块（简化版）
typedef struct k_task_st {
    knl_obj_t           knl_obj;        // 内核对象
    char               *name;           // 任务名
    k_stack_t          *sp;             // 栈指针
    k_task_entry_t      entry;          // 任务入口函数
    void               *arg;            // 任务参数
    k_stack_t          *stk_base;       // 栈基地址
    size_t              stk_size;       // 栈大小
    k_list_t            stat_list;      // 状态链表
    k_prio_t            prio;           // 优先级
    k_tick_t            tick_expires;   // 超时时间
    k_sem_t            *pending_sem;    // 挂起的信号量
    k_mutex_t          *pending_mutex;  // 挂起的互斥锁
    k_event_t          *pending_event;  // 挂起的事件
    k_tick_t            timeslice_reload; // 时间片重载值
    k_tick_t            timeslice;      // 剩余时间片
} k_task_t;
```

### 3.3 RISC-V寄存器上下文结构

```c
// RISC-V上下文保存结构（port_s.S中使用）
typedef struct port_context_st {
    // 寄存器保存顺序
    uint32_t mepc;      // 机器异常程序计数器
    uint32_t mstatus;   // 机器状态寄存器
    uint32_t ra;        // 返回地址 (x1)
    uint32_t sp;        // 栈指针 (x2)
    uint32_t gp;        // 全局指针 (x3)
    uint32_t tp;        // 线程指针 (x4)
    uint32_t t0;        // 临时寄存器 (x5)
    uint32_t t1;        // 临时寄存器 (x6)
    uint32_t t2;        // 临时寄存器 (x7)
    uint32_t s0;        // 保存寄存器 (x8)
    uint32_t s1;        // 保存寄存器 (x9)
    uint32_t a0;        // 参数/返回值 (x10)
    uint32_t a1;        // 参数 (x11)
    uint32_t a2;        // 参数 (x12)
    uint32_t a3;        // 参数 (x13)
    uint32_t a4;        // 参数 (x14)
    uint32_t a5;        // 参数 (x15)
    uint32_t a6;        // 参数 (x16)
    uint32_t a7;        // 参数 (x17)
    uint32_t s2;        // 保存寄存器 (x18)
    uint32_t s3;        // 保存寄存器 (x19)
    uint32_t s4;        // 保存寄存器 (x20)
    uint32_t s5;        // 保存寄存器 (x21)
    uint32_t s6;        // 保存寄存器 (x22)
    uint32_t s7;        // 保存寄存器 (x23)
    uint32_t s8;        // 保存寄存器 (x24)
    uint32_t s9;        // 保存寄存器 (x25)
    uint32_t s10;       // 保存寄存器 (x26)
    uint32_t s11;       // 保存寄存器 (x27)
    uint32_t t3;        // 临时寄存器 (x28)
    uint32_t t4;        // 临时寄存器 (x29)
    uint32_t t5;        // 临时寄存器 (x30)
    uint32_t t6;        // 临时寄存器 (x31)
} port_context_t;
```

### 3.4 系统错误码定义

```c
// 错误码枚举
typedef enum k_err_en {
    K_ERR_NONE                      = 0u,      // 无错误

    // 内核错误
    K_ERR_KNL_NOT_RUNNING           = 100u,    // 内核未运行
    K_ERR_KNL_RUNNING               = 101u,    // 内核已运行

    // 任务错误
    K_ERR_TASK_PRIO_INVALID         = 200u,    // 无效的任务优先级
    K_ERR_TASK_STK_SIZE_INVALID     = 201u,    // 无效的任务栈大小
    K_ERR_TASK_STK_OVERFLOW         = 202u,    // 任务栈溢出
    K_ERR_TASK_SUSPENDED            = 203u,    // 任务已挂起
    K_ERR_TASK_DELAY                = 204u,    // 任务延迟

    // 信号量错误
    K_ERR_SEM_OVERFLOW              = 300u,    // 信号量溢出
    K_ERR_SEM_UNAVAILABLE           = 301u,    // 信号量不可用

    // 互斥锁错误
    K_ERR_MUTEX_NOT_OWNER           = 400u,    // 非互斥锁拥有者
    K_ERR_MUTEX_NESTING_OVERFLOW    = 401u,    // 互斥锁嵌套溢出

    // 队列错误
    K_ERR_QUEUE_EMPTY               = 500u,    // 队列空
    K_ERR_QUEUE_FULL                = 501u,    // 队列满

    // 定时器错误
    K_ERR_TIMER_INACTIVE            = 600u,    // 定时器未激活
    K_ERR_TIMER_STATE_INVALID       = 601u,    // 无效的定时器状态

    // 超时错误
    K_ERR_PEND_TIMEOUT              = 700u,    // 等待超时
    K_ERR_PEND_ABNORMAL             = 701u,    // 等待异常

    // 内存错误
    K_ERR_OUT_OF_MEMORY             = 800u,    // 内存不足
    K_ERR_MMHEAP_INVALID_POOL_ADDR  = 801u,    // 无效的内存池地址

    // 对象错误
    K_ERR_OBJ_PTR_NULL              = 900u,    // 对象指针为空
    K_ERR_OBJ_INVALID               = 901u,    // 无效的对象
    K_ERR_OBJ_TYPE_NOT_MATCH        = 902u     // 对象类型不匹配
} k_err_t;
```

## 4. 通用配置步骤

### 4.1 TencentOS Tiny移植步骤

1. **系统时钟配置**：
   ```c
   // 在system_ch32v30x.c中配置系统时钟
   #define SYSCLK_FREQ_96MHz_HSE  96000000
   uint32_t SystemCoreClock = SYSCLK_FREQ_96MHz_HSE;
   ```

2. **SysTick定时器配置**：
   ```c
   // 在port_s.S中配置SysTick中断
   void SysTick_Handler(void)
   {
       if (tos_knl_is_running()) {
           tos_knl_irq_enter();
           tos_tick_handler();
           tos_knl_irq_leave();
       }
   }
   ```

3. **链接脚本配置**：
   ```ld
   /* Link.ld - 内存布局 */
   MEMORY
   {
       FLASH (rx) : ORIGIN = 0x08000000, LENGTH = 256K
       RAM (xrw) : ORIGIN = 0x20000000, LENGTH = 64K
   }

   /* 栈和堆配置 */
   _stack_size = 0x1000;
   _heap_size = 0x2000;
   ```

4. **启动文件修改**：
   ```assembly
   /* startup_ch32v30x_D8.S - RISC-V启动代码 */
   .section .text.vector
   .align 2
   .globl vector_table
   vector_table:
       j _start                          // 复位向量
       .word 0
       .word 0
       .word eclic_msip_handler          // 机器软件中断
       .word 0
       .word 0
       .word eclic_mtip_handler          // 机器定时器中断
       .word 0
       .word 0
       .word eclic_bwei_handler          // 总线错误写
       .word eclic_pmovi_handler         // 指令预取错误
       .word 0
       .word SysTick_Handler             // SysTick中断
       .word 0
       // ... 其他中断向量
   ```

### 4.2 内核初始化步骤

```c
// 完整的TencentOS Tiny初始化流程
int main(void)
{
    // 1. 硬件初始化
    SystemCoreClockUpdate();
    NVIC_PriorityGroupConfig(NVIC_PriorityGroup_2);
    USART_Printf_Init(115200);
    printf("SystemClk:%d\r\n", SystemCoreClock);

    // 2. 内核初始化
    tos_knl_init();

    // 3. 外设初始化（可选）
    // tos_hal_init();

    // 4. 创建系统任务
    // - 系统监控任务
    // - 日志任务
    // - 通信任务等

    // 5. 创建应用任务
    tos_task_create(&app_task1, "app1", app_task1_entry, NULL, 3, app1_stk, 1024, 0);
    tos_task_create(&app_task2, "app2", app_task2_entry, NULL, 4, app2_stk, 1024, 0);

    // 6. 启动内核调度
    tos_knl_start();

    // 7. 永不返回
    while(1);
}
```

### 4.3 任务创建步骤

```c
// 任务创建详细步骤
void create_tasks_example(void)
{
    k_err_t err;

    // 1. 定义任务栈（4字节对齐）
    #define TASK_STACK_SIZE 1024
    __aligned(4) uint8_t task_stack[TASK_STACK_SIZE];
    k_task_t task_handle;

    // 2. 定义任务参数结构（可选）
    typedef struct {
        uint32_t param1;
        uint32_t param2;
    } task_param_t;

    task_param_t param = {
        .param1 = 100,
        .param2 = 200
    };

    // 3. 创建任务
    err = tos_task_create(&task_handle,          // 任务句柄
                          "example_task",        // 任务名
                          task_entry_function,   // 入口函数
                          &param,                // 参数
                          3,                     // 优先级（0-9，越小越高）
                          task_stack,            // 栈空间
                          TASK_STACK_SIZE,       // 栈大小
                          10);                   // 时间片（tick数）

    // 4. 检查创建结果
    if (err != K_ERR_NONE) {
        printf("Task creation failed: %d\r\n", err);
    } else {
        printf("Task created successfully\r\n");
    }
}
```

### 4.4 中断处理配置

```c
// TencentOS Tiny中断处理配置
// 1. 快速中断处理函数声明
void TIM2_IRQHandler(void) __attribute__((interrupt("WCH-Interrupt-fast")));

// 2. 中断处理函数实现
void TIM2_IRQHandler(void)
{
    // 进入中断处理
    tos_knl_irq_enter();

    // 检查中断标志
    if (TIM_GetITStatus(TIM2, TIM_IT_Update) != RESET) {
        // 清除中断标志
        TIM_ClearITPendingBit(TIM2, TIM_IT_Update);

        // 中断处理逻辑
        // ...

        // 通知任务（可选）
        // tos_sem_post(&sem_timer);
    }

    // 离开中断处理
    tos_knl_irq_leave();
}

// 3. 中断优先级配置
void interrupt_priority_config(void)
{
    NVIC_InitTypeDef NVIC_InitStructure = {0};

    // 配置TIM2中断
    NVIC_InitStructure.NVIC_IRQChannel = TIM2_IRQn;
    NVIC_InitStructure.NVIC_IRQChannelPreemptionPriority = 1;
    NVIC_InitStructure.NVIC_IRQChannelSubPriority = 0;
    NVIC_InitStructure.NVIC_IRQChannelCmd = ENABLE;
    NVIC_Init(&NVIC_InitStructure);
}
```

## 5. 示例工程详解

### 5.1 TencentOS示例工程

- **位置**：`TencentOS/TencentOS/User/main.c`
- **功能**：演示TencentOS Tiny基本功能，创建两个周期性任务
- **特点**：
  - 完整的TencentOS Tiny移植
  - RISC-V架构优化
  - 多任务调度演示
  - 串口调试输出

### 5.2 工程结构说明

```
TencentOS/
└── TencentOS/
    ├── Startup/
    │   └── startup_ch32v30x_D8.S      # RISC-V启动文件
    ├── TencentOS_Tiny/
    │   ├── arch/                      # 架构相关代码
    │   │   └── risc-v/
    │   │       └── rv32/
    │   │           ├── gcc/
    │   │           │   ├── port_s.S   # 上下文切换汇编
    │   │           │   └── port_config.h
    │   │           └── common/
    │   ├── kernel/                    # 内核源代码
    │   ├── cmsis/                     # CMSIS兼容层
    │   └── TOS_CONFIG/
    │       └── tos_config.h           # 内核配置文件
    ├── User/
    │   ├── main.c                     # 主程序文件
    │   ├── ch32v30x_it.c              # 中断服务程序
    │   ├── ch32v30x_it.h              # 中断头文件
    │   ├── ch32v30x_conf.h            # 外设配置文件
    │   ├── system_ch32v30x.c          # 系统初始化
    │   └── system_ch32v30x.h          # 系统头文件
    ├── .cproject                      # Eclipse项目配置
    ├── .project                       # Eclipse项目文件
    └── TencentOS.wvproj               # WCH IDE项目文件
```

### 5.3 关键功能点

1. **系统时钟配置**：
   ```c
   // 使用96MHz HSE时钟
   #define SYSCLK_FREQ_96MHz_HSE  96000000
   uint32_t SystemCoreClock = SYSCLK_FREQ_96MHz_HSE;
   ```

2. **SysTick中断处理**：
   ```c
   // 在启动文件中配置SysTick中断向量
   .word SysTick_Handler  // SysTick中断
   ```

3. **任务栈对齐**：
   ```c
   // RISC-V要求栈8字节对齐，TencentOS要求4字节对齐
   __aligned(4) uint8_t task_stk[TASK_STK_SIZE];
   ```

4. **内核版本信息**：
   ```c
   // 打印TencentOS Tiny版本
   printf("Welcome to TencentOS tiny(%s)\r\n", TOS_VERSION);
   ```

5. **任务调度策略**：
   ```c
   // 配置为抢占式调度，无时间片轮转
   #define TOS_CFG_ROUND_ROBIN_EN 0u
   ```

### 5.4 RISC-V架构优化

```assembly
/* port_s.S - RISC-V上下文切换汇编 */
port_sched_start:
    // 获取当前任务栈指针
    lw      t0, k_curr_task
    lw      sp, (t0)

    // 恢复上下文
    j       restore_context

save_context:
    // 保存所有寄存器到任务栈
    addi    sp, sp, -CONTEXT_SIZE
    sw      ra, __reg_ra_OFFSET(sp)
    sw      sp, __reg_sp_OFFSET(sp)
    // ... 保存其他寄存器

restore_context:
    // 从任务栈恢复寄存器
    lw      ra, __reg_ra_OFFSET(sp)
    lw      sp, __reg_sp_OFFSET(sp)
    // ... 恢复其他寄存器
    addi    sp, sp, CONTEXT_SIZE

    // 返回到任务
    mret
```

## 6. 常见问题

### Q1: 任务栈溢出如何检测？
A: TencentOS Tiny提供栈溢出检测功能：

1. **编译时检测**：
   ```c
   #define TOS_CFG_TASK_STACK_DRAUGHT_DEPTH_DETACT_EN 1u
   ```

2. **运行时检测**：
   ```c
   // 在任务创建时检查栈大小
   if (stk_size < TOS_CFG_TASK_STACK_MIN_SIZE) {
       return K_ERR_TASK_STK_SIZE_INVALID;
   }
   ```

3. **手动检测**：
   ```c
   // 定期检查栈使用情况
   size_t stack_used = tos_task_stack_used(task_handle);
   size_t stack_free = tos_task_stack_free(task_handle);
   printf("Stack used: %d, free: %d\r\n", stack_used, stack_free);
   ```

### Q2: 如何优化中断响应时间？
A: 优化建议：

1. **中断处理函数简洁**：
   ```c
   void IRQ_Handler(void)
   {
       tos_knl_irq_enter();
       // 只做必要的处理
       tos_knl_irq_leave();
   }
   ```

2. **使用信号量/事件通知任务**：
   ```c
   // 在中断中释放信号量
   tos_sem_post(&sem_from_isr);

   // 在任务中处理复杂逻辑
   void task_handler(void *arg)
   {
       while(1) {
           tos_sem_pend(&sem_from_isr, TOS_TIME_FOREVER);
           // 处理复杂逻辑
       }
   }
   ```

3. **合理设置中断优先级**：
   ```c
   // 高优先级中断使用更高的抢占优先级
   NVIC_SetPriority(SysTicK_IRQn, 0);  // 最高优先级
   ```

### Q3: 任务优先级如何设置？
A: 优先级设置原则：

1. **优先级范围**：0-9（0最高，9最低）
2. **系统任务**：高优先级（0-2）
3. **应用任务**：中优先级（3-6）
4. **后台任务**：低优先级（7-9）
5. **避免优先级反转**：
   ```c
   // 使用优先级继承互斥锁
   #define TOS_CFG_MUTEX_PRIO_INHERIT_EN 1u
   ```

### Q4: 如何实现低功耗？
A: TencentOS Tiny低功耗支持：

1. **Tickless模式**：
   ```c
   #define TOS_CFG_PWR_MGR_EN 1u
   #define TOS_CFG_TICKLESS_EN 1u
   ```

2. **空闲任务钩子**：
   ```c
   void idle_task_hook(void)
   {
       // 进入低功耗模式
       __WFI();
   }

   tos_knl_set_idle_task_hook(idle_task_hook);
   ```

3. **动态频率调整**：
   ```c
   // 根据系统负载调整CPU频率
   void adjust_cpu_frequency_based_on_load(void)
   {
       uint32_t cpu_load = tos_get_cpu_usage();

       if (cpu_load < 30) {
           // 降低频率
           RCC_SYSCLKConfig(RCC_SYSCLKSource_HSI);
       } else if (cpu_load > 70) {
           // 提高频率
           RCC_SYSCLKConfig(RCC_SYSCLKSource_PLLCLK);
       }
   }
   ```

### Q5: 内存不足如何处理？
A: 内存管理策略：

1. **静态内存分配**：
   ```c
   // 使用静态内存池
   uint8_t memory_pool[4096];
   tos_mmheap_create(memory_pool, sizeof(memory_pool));
   ```

2. **内存使用监控**：
   ```c
   void monitor_memory_usage(void)
   {
       size_t total = tos_mmheap_total_size_get();
       size_t used = tos_mmheap_used_size_get();
       size_t free = tos_mmheap_free_size_get();

       printf("Memory: total=%d, used=%d, free=%d\r\n", total, used, free);

       if (free < 1024) {
           printf("Warning: low memory!\r\n");
       }
   }
   ```

3. **内存泄漏检测**：
   ```c
   #define TOS_CFG_MMHEAP_DBG_EN 1u  // 启用内存调试
   ```

## 7. API参考

### 7.1 内核管理API

| 函数名 | 功能描述 | 参数说明 |
|--------|----------|----------|
| `tos_knl_init` | 内核初始化 | 无 |
| `tos_knl_start` | 启动内核调度 | 无 |
| `tos_knl_is_running` | 检查内核是否运行 | 返回运行状态 |
| `tos_knl_irq_enter` | 进入中断处理 | 无 |
| `tos_knl_irq_leave` | 离开中断处理 | 无 |

### 7.2 任务管理API

| 函数名 | 功能描述 | 参数说明 |
|--------|----------|----------|
| `tos_task_create` | 创建任务 | 任务句柄、名称、入口函数、参数、优先级、栈空间、栈大小、时间片 |
| `tos_task_destroy` | 销毁任务 | 任务句柄 |
| `tos_task_delay` | 任务延时 | 延时tick数 |
| `tos_task_delay_until` | 延时到指定时间 | 目标tick数 |
| `tos_task_suspend` | 挂起任务 | 任务句柄 |
| `tos_task_resume` | 恢复任务 | 任务句柄 |
| `tos_task_prio_change` | 改变任务优先级 | 任务句柄、新优先级 |
| `tos_task_yield` | 任务让步 | 无 |

### 7.3 信号量API

| 函数名 | 功能描述 | 参数说明 |
|--------|----------|----------|
| `tos_sem_create` | 创建信号量 | 信号量句柄、初始值 |
| `tos_sem_destroy` | 销毁信号量 | 信号量句柄 |
| `tos_sem_pend` | 等待信号量 | 信号量句柄、超时时间 |
| `tos_sem_post` | 释放信号量 | 信号量句柄 |
| `tos_sem_post_all` | 释放所有等待的信号量 | 信号量句柄 |

### 7.4 互斥锁API

| 函数名 | 功能描述 | 参数说明 |
|--------|----------|----------|
| `tos_mutex_create` | 创建互斥锁 | 互斥锁句柄 |
| `tos_mutex_destroy` | 销毁互斥锁 | 互斥锁句柄 |
| `tos_mutex_pend` | 获取互斥锁 | 互斥锁句柄、超时时间 |
| `tos_mutex_post` | 释放互斥锁 | 互斥锁句柄 |

### 7.5 消息队列API

| 函数名 | 功能描述 | 参数说明 |
|--------|----------|----------|
| `tos_queue_create` | 创建消息队列 | 队列句柄、缓冲区、容量、元素大小 |
| `tos_queue_destroy` | 销毁消息队列 | 队列句柄 |
| `tos_queue_pend` | 从队列接收消息 | 队列句柄、缓冲区、消息大小指针、超时时间 |
| `tos_queue_post` | 向队列发送消息 | 队列句柄、消息、消息大小 |
| `tos_queue_flush` | 清空队列 | 队列句柄 |

### 7.6 软件定时器API

| 函数名 | 功能描述 | 参数说明 |
|--------|----------|----------|
| `tos_timer_create` | 创建定时器 | 定时器句柄、初始延迟、周期、回调函数、参数、选项 |
| `tos_timer_destroy` | 销毁定时器 | 定时器句柄 |
| `tos_timer_start` | 启动定时器 | 定时器句柄 |
| `tos_timer_stop` | 停止定时器 | 定时器句柄 |
| `tos_tick_handler` | 系统tick处理 | 无 |

### 7.7 内存管理API

| 函数名 | 功能描述 | 参数说明 |
|--------|----------|----------|
| `tos_mmheap_create` | 创建内存堆 | 内存池地址、大小 |
| `tos_mmheap_destroy` | 销毁内存堆 | 内存池地址 |
| `tos_mmheap_alloc` | 分配内存 | 大小 |
| `tos_mmheap_free` | 释放内存 | 内存指针 |
| `tos_mmheap_realloc` | 重新分配内存 | 原指针、新大小 |

## 8. 注意事项

1. **栈大小设置**：根据任务需求合理设置栈大小，避免溢出或浪费内存
2. **中断优先级**：合理配置中断优先级，避免优先级反转和中断延迟
3. **临界区保护**：在访问共享资源时使用互斥锁或信号量
4. **任务优先级**：避免设置过多高优先级任务，导致低优先级任务饥饿
5. **内存管理**：在资源受限环境中注意内存使用，及时释放不再使用的内存
6. **中断处理**：中断处理函数应尽量简短，复杂处理交给任务
7. **时间管理**：注意任务延时和定时器的精度，考虑系统tick频率
8. **低功耗优化**：合理使用tickless模式和空闲任务钩子降低功耗

## 9. 性能优化建议

1. **任务栈优化**：
   - 根据实际使用情况调整栈大小
   - 使用`tos_task_stack_used()`监控栈使用
   - 避免在栈上分配大数组

2. **调度优化**：
   - 合理设置任务优先级
   - 考虑使用时间片轮转调度
   - 避免频繁的任务切换

3. **内存优化**：
   - 使用静态内存分配避免碎片
   - 合理设置内存池大小
   - 定期监控内存使用情况

4. **中断优化**：
   - 缩短中断处理时间
   - 使用DMA减轻CPU负担
   - 合理设置中断优先级

5. **功耗优化**：
   - 启用tickless模式
   - 在空闲任务中进入低功耗模式
   - 动态调整CPU频率

## 10. 典型应用场景

### 10.1 物联网设备数据采集

```c
// 物联网设备多任务数据采集系统
void iot_device_example(void)
{
    // 创建传感器数据采集任务
    tos_task_create(&sensor_task, "sensor", sensor_collection_task, NULL, 2, sensor_stk, 1024, 0);

    // 创建数据处理任务
    tos_task_create(&process_task, "process", data_processing_task, NULL, 3, process_stk, 1024, 0);

    // 创建通信任务
    tos_task_create(&comm_task, "comm", communication_task, NULL, 4, comm_stk, 1024, 0);

    // 创建系统监控任务
    tos_task_create(&monitor_task, "monitor", system_monitor_task, NULL, 5, monitor_stk, 1024, 0);

    // 使用消息队列传递数据
    tos_queue_create(&sensor_queue, sensor_queue_buf, 10, sizeof(sensor_data_t));
    tos_queue_create(&process_queue, process_queue_buf, 10, sizeof(processed_data_t));

    // 使用信号量同步任务
    tos_sem_create(&data_ready_sem, 0);
    tos_sem_create(&send_ready_sem, 0);
}

// 传感器采集任务
void sensor_collection_task(void *arg)
{
    sensor_data_t data;

    while(1) {
        // 采集传感器数据
        collect_sensor_data(&data);

        // 发送到处理队列
        tos_queue_post(&sensor_queue, &data, sizeof(data));

        // 通知处理任务
        tos_sem_post(&data_ready_sem);

        // 延时1秒
        tos_task_delay(1000);
    }
}
```

### 10.2 工业控制系统

```c
// 工业控制多任务系统
void industrial_control_example(void)
{
    // 创建实时控制任务（高优先级）
    tos_task_create(&control_task, "control", realtime_control_task, NULL, 0, control_stk, 2048, 0);

    // 创建人机界面任务（中优先级）
    tos_task_create(&hmi_task, "hmi", hmi_task_entry, NULL, 3, hmi_stk, 2048, 0);

    // 创建数据记录任务（低优先级）
    tos_task_create(&log_task, "log", data_logging_task, NULL, 6, log_stk, 2048, 0);

    // 创建报警处理任务（高优先级）
    tos_task_create(&alarm_task, "alarm", alarm_handling_task, NULL, 1, alarm_stk, 1024, 0);

    // 使用事件标志组同步任务
    tos_event_create(&system_events);

    // 使用互斥锁保护共享资源
    tos_mutex_create(&io_mutex);
    tos_mutex_create(&data_mutex);
}

// 实时控制任务
void realtime_control_task(void *arg)
{
    control_params_t params;
    k_err_t err;

    while(1) {
        // 等待控制周期事件（100ms）
        err = tos_event_pend(&system_events, EVENT_CONTROL_CYCLE, TOS_OPT_EVENT_PEND_ANY, 100);

        if (err == K_ERR_NONE) {
            // 获取IO访问权限
            tos_mutex_pend(&io_mutex);

            // 读取输入
            read_inputs(&params);

            // 执行控制算法
            execute_control_algorithm(&params);

            // 写入输出
            write_outputs(&params);

            // 释放IO访问权限
            tos_mutex_post(&io_mutex);

            // 发送数据到记录任务
            tos_queue_post(&data_queue, &params, sizeof(params));
        }
    }
}
```

### 10.3 智能家居网关

```c
// 智能家居网关系统
void smart_home_gateway_example(void)
{
    // 创建网络通信任务
    tos_task_create(&network_task, "network", network_communication_task, NULL, 2, network_stk, 4096, 0);

    // 创建设备管理任务
    tos_task_create(&device_task, "device", device_management_task, NULL, 3, device_stk, 2048, 0);

    // 创建规则引擎任务
    tos_task_create(&rule_task, "rule", rule_engine_task, NULL, 4, rule_stk, 2048, 0);

    // 创建用户界面任务
    tos_task_create(&ui_task, "ui", user_interface_task, NULL, 5, ui_stk, 2048, 0);

    // 创建系统维护任务
    tos_task_create(&maintenance_task, "maint", system_maintenance_task, NULL, 6, maint_stk, 1024, 0);

    // 使用软件定时器实现定时任务
    tos_timer_create(&device_scan_timer, 5000, 5000, device_scan_callback, NULL, TIMER_OPT_PERIODIC);
    tos_timer_create(&status_report_timer, 30000, 30000, status_report_callback, NULL, TIMER_OPT_PERIODIC);
    tos_timer_create(&backup_timer, 3600000, 3600000, backup_callback, NULL, TIMER_OPT_PERIODIC);

    // 启动定时器
    tos_timer_start(&device_scan_timer);
    tos_timer_start(&status_report_timer);
    tos_timer_start(&backup_timer);
}

// 设备扫描回调
void device_scan_callback(void *arg)
{
    // 扫描网络设备
    scan_network_devices();

    // 更新设备列表
    update_device_list();

    // 通知设备管理任务
    tos_sem_post(&device_update_sem);
}
```

## 11. 总结

TencentOS Tiny是一个轻量级、高性能的实时操作系统，在CH32V307 RISC-V微控制器上提供了完整的RTOS功能。通过合理的任务设计、资源管理和优化配置，可以构建稳定可靠的嵌入式系统。开发时需要注意任务优先级分配、栈大小设置、中断处理优化等关键环节，同时充分利用TencentOS Tiny提供的丰富组件和优化特性，提高系统性能和可靠性。
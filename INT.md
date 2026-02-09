# CH32V307 INT中断控制器使用指南

## 1. 外设概述

INT（中断控制器）是CH32V307的中断管理系统，负责管理所有外设中断和异常处理。CH32V307采用RISC-V架构，具有灵活的中断处理机制。主要特性包括：

- **多层次优先级**：支持4位组优先级和1位子优先级，共9位优先级配置
- **快速中断支持**：通过硬件压栈实现快速中断响应
- **中断嵌套**：支持多级中断嵌套，硬件压栈最多支持3级嵌套
- **向量表灵活性**：支持Flash和RAM中的向量表
- **VTF功能**：向量表快速中断，减少中断响应时间
- **丰富的中断源**：支持128个中断向量，涵盖所有外设
- **中断屏蔽**：支持全局中断屏蔽和单个中断使能/禁用
- **软件中断**：支持软件触发中断

## 2. 关键代码片段

### 2.1 中断优先级配置

```c
// 中断优先级配置示例（Interrupt_Nest）
// 格式：(组优先级 << 5) | (子优先级 << 4)
// 优先级数值越小，优先级越高（0为最高优先级）

// 配置多个中断的优先级
NVIC_SetPriority(WWDG_IRQn,  (7<<5) | (0x01<<4));  // 组优先级7，子优先级1
NVIC_SetPriority(PVD_IRQn,   (6<<5) | (0x01<<4));  // 组优先级6，子优先级1
NVIC_SetPriority(TAMPER_IRQn,(5<<5) | (0x01<<4));  // 组优先级5，子优先级1
NVIC_SetPriority(RTC_IRQn,   (4<<5) | (0x01<<4));  // 组优先级4，子优先级1
NVIC_SetPriority(FLASH_IRQn, (3<<5) | (0x01<<4));  // 组优先级3，子优先级1
NVIC_SetPriority(RCC_IRQn,   (2<<5) | (0x01<<4));  // 组优先级2，子优先级1
NVIC_SetPriority(EXTI0_IRQn, (1<<5) | (0x01<<4));  // 组优先级1，子优先级1
NVIC_SetPriority(EXTI1_IRQn, (0<<5) | (0x01<<4));  // 组优先级0，子优先级1，最高优先级

// 使能中断
NVIC_EnableIRQ(WWDG_IRQn);
NVIC_EnableIRQ(PVD_IRQn);
NVIC_EnableIRQ(TAMPER_IRQn);
NVIC_EnableIRQ(RTC_IRQn);
NVIC_EnableIRQ(FLASH_IRQn);
NVIC_EnableIRQ(RCC_IRQn);
NVIC_EnableIRQ(EXTI0_IRQn);
NVIC_EnableIRQ(EXTI1_IRQn);
```

### 2.2 快速中断声明

```c
// 快速中断声明（使用硬件压栈）
// 必须使用__attribute__((interrupt("WCH-Interrupt-fast")))属性

// 窗口看门狗中断
void WWDG_IRQHandler(void) __attribute__((interrupt("WCH-Interrupt-fast")));
void WWDG_IRQHandler(void)
{
    // 清除中断标志
    WWDG_ClearFlag();

    printf("WWDG interrupt\r\n");

    // 触发下一个中断（演示中断嵌套）
    NVIC_SetPendingIRQ(PVD_IRQn);
}

// 电源电压检测中断
void PVD_IRQHandler(void) __attribute__((interrupt("WCH-Interrupt-fast")));
void PVD_IRQHandler(void)
{
    printf("PVD interrupt\r\n");

    // 触发下一个中断
    NVIC_SetPendingIRQ(TAMPER_IRQn);
}

// 入侵检测中断
void TAMPER_IRQHandler(void) __attribute__((interrupt("WCH-Interrupt-fast")));
void TAMPER_IRQHandler(void)
{
    printf("TAMPER interrupt\r\n");

    // 触发下一个中断
    NVIC_SetPendingIRQ(RTC_IRQn);
}

// 实时时钟中断
void RTC_IRQHandler(void) __attribute__((interrupt("WCH-Interrupt-fast")));
void RTC_IRQHandler(void)
{
    printf("RTC interrupt\r\n");

    // 触发下一个中断
    NVIC_SetPendingIRQ(FLASH_IRQn);
}

// Flash中断
void FLASH_IRQHandler(void) __attribute__((interrupt("WCH-Interrupt-fast")));
void FLASH_IRQHandler(void)
{
    printf("FLASH interrupt\r\n");

    // 触发下一个中断
    NVIC_SetPendingIRQ(RCC_IRQn);
}

// 复位和时钟控制中断
void RCC_IRQHandler(void) __attribute__((interrupt("WCH-Interrupt-fast")));
void RCC_IRQHandler(void)
{
    printf("RCC interrupt\r\n");

    // 触发下一个中断
    NVIC_SetPendingIRQ(EXTI0_IRQn);
}

// 外部中断0
void EXTI0_IRQHandler(void) __attribute__((interrupt("WCH-Interrupt-fast")));
void EXTI0_IRQHandler(void)
{
    printf("EXTI0 interrupt\r\n");

    // 触发下一个中断
    NVIC_SetPendingIRQ(EXTI1_IRQn);
}

// 外部中断1（最高优先级）
void EXTI1_IRQHandler(void) __attribute__((interrupt("WCH-Interrupt-fast")));
void EXTI1_IRQHandler(void)
{
    printf("EXTI1 interrupt\r\n");
    printf("Interrupt nesting test complete\r\n");
}
```

### 2.3 VTF（向量表快速中断）配置

```c
// VTF中断配置示例（Interrupt_VTF）
// 设置VTF中断，将中断处理函数地址存储在VTF表中

// SysTick中断处理函数
void SysTick_Handler(void)
{
    printf("SysTick interrupt via VTF\r\n");
}

// 配置VTF中断
void VTF_Interrupt_Config(void)
{
    // 设置VTF中断
    // 参数：中断处理函数地址，中断号，VTF表索引，使能状态
    SetVTFIRQ((u32)SysTick_Handler, SysTicK_IRQn, 0, ENABLE);

    // 使能SysTick中断
    NVIC_EnableIRQ(SysTicK_IRQn);

    // 配置SysTick定时器
    SysTick->SR = 0;        // 清除状态寄存器
    SysTick->CNT = 0;       // 计数器清零
    SysTick->CMP = 0x20;    // 比较值
    SysTick->CTLR = 0x7;    // 控制寄存器：使能、自动重装载、中断使能
}
```

### 2.4 RAM向量表配置

```c
// RAM向量表示例（VectorInRAM）
// 将中断向量表存储在RAM中，便于动态修改

// 软件中断处理函数
void __attribute__((interrupt("WCH-Interrupt-fast"))) SW_Handler(void)
{
    // 清除软件中断挂起标志
    NVIC_ClearPendingIRQ(Software_IRQn);

    printf("Software interrupt from RAM vector table\r\n");
}

// RAM向量表初始化
void RAM_VectorTable_Init(void)
{
    // 使能软件中断
    NVIC_EnableIRQ(Software_IRQn);

    // 设置软件中断优先级
    NVIC_SetPriority(Software_IRQn, (0<<5) | (0x01<<4));

    // 触发软件中断，测试RAM向量表
    NVIC_SetPendingIRQ(Software_IRQn);
}

// 主函数中的RAM向量表测试
int main(void)
{
    SystemCoreClockUpdate();
    Delay_Init();
    USART_Printf_Init(115200);

    printf("RAM Vector Table Test\r\n");
    printf("SystemClk:%d\r\n", SystemCoreClock);

    // 初始化RAM向量表
    RAM_VectorTable_Init();

    while(1)
    {
        // 主循环
    }
}
```

### 2.5 中断嵌套测试

```c
// 中断嵌套测试函数
void Interrupt_Nest_Test(void)
{
    SystemCoreClockUpdate();
    Delay_Init();
    USART_Printf_Init(115200);

    printf("Interrupt Nesting Test\r\n");
    printf("SystemClk:%d\r\n", SystemCoreClock);

    // 配置中断优先级
    NVIC_SetPriority(WWDG_IRQn,  (7<<5) | (0x01<<4));
    NVIC_SetPriority(PVD_IRQn,   (6<<5) | (0x01<<4));
    NVIC_SetPriority(TAMPER_IRQn,(5<<5) | (0x01<<4));
    NVIC_SetPriority(RTC_IRQn,   (4<<5) | (0x01<<4));
    NVIC_SetPriority(FLASH_IRQn, (3<<5) | (0x01<<4));
    NVIC_SetPriority(RCC_IRQn,   (2<<5) | (0x01<<4));
    NVIC_SetPriority(EXTI0_IRQn, (1<<5) | (0x01<<4));
    NVIC_SetPriority(EXTI1_IRQn, (0<<5) | (0x01<<4));

    // 使能中断
    NVIC_EnableIRQ(WWDG_IRQn);
    NVIC_EnableIRQ(PVD_IRQn);
    NVIC_EnableIRQ(TAMPER_IRQn);
    NVIC_EnableIRQ(RTC_IRQn);
    NVIC_EnableIRQ(FLASH_IRQn);
    NVIC_EnableIRQ(RCC_IRQn);
    NVIC_EnableIRQ(EXTI0_IRQn);
    NVIC_EnableIRQ(EXTI1_IRQn);

    // 触发第一个中断（最低优先级）
    printf("Start interrupt nesting test...\r\n");
    NVIC_SetPendingIRQ(WWDG_IRQn);
}
```

## 3. 配置数据结构

### 3.1 中断优先级配置格式

```c
// 中断优先级位域定义
// 优先级寄存器使用9位配置：
//  位[8:5]：组优先级（4位，0-15）
//  位[4]：子优先级（1位，0-1）
//  位[3:0]：保留

// 优先级配置宏
#define NVIC_PRIORITY_GROUP_0   0x00  // 组优先级0，子优先级0
#define NVIC_PRIORITY_GROUP_1   0x10  // 组优先级0，子优先级1
#define NVIC_PRIORITY_GROUP_2   0x20  // 组优先级1，子优先级0
#define NVIC_PRIORITY_GROUP_3   0x30  // 组优先级1，子优先级1
// ... 依此类推，共32种组合

// 常用优先级配置
#define HIGHEST_PRIORITY        ((0<<5) | (0x00<<4))   // 最高优先级
#define HIGH_PRIORITY           ((1<<5) | (0x00<<4))   // 高优先级
#define MEDIUM_PRIORITY         ((3<<5) | (0x00<<4))   // 中等优先级
#define LOW_PRIORITY            ((7<<5) | (0x00<<4))   // 低优先级
#define LOWEST_PRIORITY         ((15<<5) | (0x01<<4))  // 最低优先级
```

### 3.2 VTF配置结构

```c
// VTF表项结构
typedef struct {
    uint32_t handler_addr;    // 中断处理函数地址
    IRQn_Type irq_number;     // 中断号
    uint8_t vtf_index;        // VTF表索引（0-7）
    FunctionalState enabled;  // 使能状态
} VTF_ConfigTypeDef;

// VTF表大小
#define VTF_TABLE_SIZE    8   // 支持8个VTF中断
```

### 3.3 中断向量表结构

```c
// 中断向量表结构（汇编定义）
// 位于启动文件 startup_ch32v30x_D8.S

.section    .vector,"ax",@progbits
.align  1
_vector_base:
    .option norvc;
    .word   _start                   /* 复位向量 */
    .word   0                        /* 保留 */
    .word   NMI_Handler              /* NMI中断 */
    .word   HardFault_Handler        /* 硬件错误中断 */
    .word   0                        /* 保留 */
    .word   Ecall_M_Mode_Handler     /* 机器模式ecall */
    .word   0                        /* 保留 */
    .word   Ecall_U_Mode_Handler     /* 用户模式ecall */
    .word   Break_Point_Handler      /* 断点中断 */
    .word   0                        /* 保留 */
    .word   SysTick_Handler          /* SysTick中断 */

    /* 外设中断 */
    .word   WWDG_IRQHandler          /* 窗口看门狗中断 */
    .word   PVD_IRQHandler           /* 电源电压检测中断 */
    .word   TAMPER_IRQHandler        /* 入侵检测中断 */
    .word   RTC_IRQHandler           /* 实时时钟中断 */
    .word   FLASH_IRQHandler         /* Flash中断 */
    .word   RCC_IRQHandler           /* 复位和时钟控制中断 */
    .word   EXTI0_IRQHandler         /* 外部中断0 */
    .word   EXTI1_IRQHandler         /* 外部中断1 */
    // ... 总共128个中断向量
```

## 4. 通用配置步骤

### 4.1 基本中断配置步骤

1. **系统初始化**：
   ```c
   SystemCoreClockUpdate();
   Delay_Init();
   USART_Printf_Init(115200);
   ```

2. **配置中断优先级**：
   ```c
   // 设置中断优先级（数值越小优先级越高）
   NVIC_SetPriority(IRQn, priority);
   ```

3. **使能中断**：
   ```c
   NVIC_EnableIRQ(IRQn);
   ```

4. **编写中断服务程序**：
   ```c
   // 快速中断（硬件压栈）
   void IRQ_Handler(void) __attribute__((interrupt("WCH-Interrupt-fast")));
   void IRQ_Handler(void)
   {
       // 清除中断标志
       // 中断处理逻辑
   }

   // 普通中断（软件压栈）
   void IRQ_Handler(void) __attribute__((interrupt()));
   void IRQ_Handler(void)
   {
       // 清除中断标志
       // 中断处理逻辑
   }
   ```

5. **触发中断（如果需要）**：
   ```c
   NVIC_SetPendingIRQ(IRQn);  // 软件触发中断
   ```

### 4.2 VTF中断配置步骤

1. **定义中断处理函数**：
   ```c
   void VTF_Handler(void)
   {
       // VTF中断处理逻辑
   }
   ```

2. **配置VTF中断**：
   ```c
   SetVTFIRQ((u32)VTF_Handler, IRQn, vtf_index, ENABLE);
   ```

3. **使能中断**：
   ```c
   NVIC_EnableIRQ(IRQn);
   ```

4. **配置外设触发中断**（如果需要）：
   ```c
   // 配置外设产生中断
   Peripheral_ITConfig(ENABLE);
   ```

### 4.3 RAM向量表配置步骤

1. **修改链接脚本**（Link_vector.ld）：
   ```ld
   .vector :
   {
       *(.vector);
       . = ALIGN(64);
   } >RAM AT>FLASH  /* 向量表放在RAM，但初始内容在Flash */
   ```

2. **使用专用启动文件**：
   - 使用`startup_ch32v30x_D8_vector.S`或`startup_ch32v30x_D8C_vector.S`

3. **在应用程序中初始化向量表**：
   ```c
   // 系统初始化时会自动从Flash复制向量表到RAM
   ```

4. **动态修改中断向量**（如果需要）：
   ```c
   // 可以在运行时修改RAM中的向量表
   uint32_t *vector_table = (uint32_t *)0x20000000;  // RAM起始地址
   vector_table[IRQn + 16] = (uint32_t)new_handler;  // 修改中断向量
   ```

### 4.4 中断嵌套配置步骤

1. **规划中断优先级**：
   - 确定各个中断的优先级关系
   - 高优先级中断可以打断低优先级中断

2. **配置中断优先级**：
   ```c
   // 从低到高配置优先级
   NVIC_SetPriority(LOW_IRQn, LOW_PRIORITY);
   NVIC_SetPriority(MEDIUM_IRQn, MEDIUM_PRIORITY);
   NVIC_SetPriority(HIGH_IRQn, HIGH_PRIORITY);
   ```

3. **使能所有中断**：
   ```c
   NVIC_EnableIRQ(LOW_IRQn);
   NVIC_EnableIRQ(MEDIUM_IRQn);
   NVIC_EnableIRQ(HIGH_IRQn);
   ```

4. **在中断服务程序中触发其他中断**（用于测试）：
   ```c
   void LOW_IRQHandler(void) __attribute__((interrupt("WCH-Interrupt-fast")));
   void LOW_IRQHandler(void)
   {
       // 触发更高优先级中断
       NVIC_SetPendingIRQ(MEDIUM_IRQn);
   }
   ```

## 5. 示例工程详解

### 5.1 Interrupt_Nest示例（中断嵌套）

- **位置**：`INT/Interrupt_Nest/User/main.c`
- **功能**：演示8级中断嵌套，从最低优先级到最高优先级依次触发
- **特点**：
  - 完整的优先级配置示例
  - 快速中断声明和使用
  - 中断嵌套测试
  - 串口输出中断执行顺序
- **适用场景**：
  - 学习中断优先级和嵌套机制
  - 测试中断响应时间
  - 验证中断嵌套逻辑

### 5.2 Interrupt_VTF示例（向量表快速中断）

- **位置**：`INT/Interrupt_VTF/User/main.c`
- **功能**：演示VTF（向量表快速中断）功能
- **特点**：
  - VTF中断配置
  - SysTick定时器中断
  - 减少中断响应时间
  - 专用中断向量表
- **适用场景**：
  - 对中断响应时间要求高的应用
  - 实时控制系统
  - 需要快速中断响应的场景

### 5.3 VectorInRAM示例（RAM中的向量表）

- **位置**：`INT/VectorInRAM/User/main.c`
- **功能**：演示将中断向量表放在RAM中
- **特点**：
  - 自定义链接脚本
  - 专用启动文件
  - RAM中的向量表
  - 软件中断测试
- **适用场景**：
  - 需要动态修改中断向量的应用
  - 实时更新中断处理程序
  - 灵活的固件升级

### 5.4 关键差异对比

| 特性 | Interrupt_Nest | Interrupt_VTF | VectorInRAM |
|------|---------------|---------------|-------------|
| 主要功能 | 中断嵌套测试 | 快速中断响应 | 动态向量表 |
| 中断类型 | 快速中断 | VTF中断 | 软件中断 |
| 向量表位置 | Flash | Flash | RAM |
| 优先级配置 | 完整优先级 | 单中断优先级 | 软件中断优先级 |
| 适用场景 | 学习测试 | 实时响应 | 动态更新 |

## 6. 常见问题

### Q1: 中断服务程序没有执行？
A: 检查以下配置：
1. 中断是否使能（`NVIC_EnableIRQ()`）
2. 中断优先级是否正确配置
3. 中断服务程序名称是否与向量表一致
4. 中断标志是否清除
5. 全局中断是否使能（`__enable_irq()`）

### Q2: 中断嵌套不正确？
A: 可能原因：
1. 中断优先级配置错误
2. 使用了硬件压栈但嵌套超过3级
3. 中断服务程序中没有正确保存和恢复上下文
4. 中断屏蔽设置不正确

### Q3: VTF中断不工作？
A: 检查以下配置：
1. VTF中断是否正确配置（`SetVTFIRQ()`）
2. VTF表索引是否超出范围（0-7）
3. 中断处理函数地址是否正确
4. 中断是否使能

### Q4: RAM向量表无法正常跳转？
A: 检查以下配置：
1. 链接脚本是否正确配置
2. 启动文件是否使用支持RAM向量表的版本
3. 向量表是否正确从Flash复制到RAM
4. 向量表地址是否对齐（64字节对齐）

### Q5: 中断响应时间过长？
A: 优化建议：
1. 使用快速中断（`WCH-Interrupt-fast`）
2. 使用VTF中断减少跳转时间
3. 优化中断服务程序，减少执行时间
4. 合理设置中断优先级，避免嵌套过深

## 7. API参考

### 7.1 NVIC中断控制API

| 函数名 | 功能描述 | 参数说明 |
|--------|----------|----------|
| `NVIC_EnableIRQ` | 使能中断 | IRQn：中断号 |
| `NVIC_DisableIRQ` | 禁用中断 | IRQn：中断号 |
| `NVIC_SetPriority` | 设置中断优先级 | IRQn：中断号，priority：优先级 |
| `NVIC_GetPriority` | 获取中断优先级 | IRQn：中断号 |
| `NVIC_SetPendingIRQ` | 设置中断挂起 | IRQn：中断号 |
| `NVIC_ClearPendingIRQ` | 清除中断挂起 | IRQn：中断号 |
| `NVIC_GetPendingIRQ` | 获取中断挂起状态 | IRQn：中断号 |

### 7.2 VTF中断API

| 函数名 | 功能描述 | 参数说明 |
|--------|----------|----------|
| `SetVTFIRQ` | 设置VTF中断 | addr：处理函数地址，IRQn：中断号，num：VTF索引，NewState：使能状态 |

### 7.3 系统中断API

| 函数名 | 功能描述 | 参数说明 |
|--------|----------|----------|
| `NVIC_SystemReset` | 系统复位 | 无 |
| `__disable_irq` | 全局禁用中断 | 无 |
| `__enable_irq` | 全局使能中断 | 无 |
| `__WFI` | 等待中断 | 无 |
| `__WFE` | 等待事件 | 无 |

### 7.4 中断控制器寄存器

| 寄存器 | 功能描述 | 地址 |
|--------|----------|------|
| `PFIC->ISR[ ]` | 中断使能寄存器 | 0xE000E100 |
| `PFIC->IPR[ ]` | 中断优先级寄存器 | 0xE000E400 |
| `PFIC->ITHRESDR` | 中断阈值寄存器 | 0xE000E448 |
| `PFIC->CFGR` | 配置寄存器 | 0xE000E14C |
| `PFIC->GISR` | 全局中断状态寄存器 | 0xE000E150 |

## 8. 注意事项

1. **中断属性**：中断服务程序必须使用`__attribute__((interrupt()))`或`__attribute__((interrupt("WCH-Interrupt-fast")))`
2. **硬件压栈限制**：使用快速中断时，硬件压栈最多支持3级嵌套
3. **向量表对齐**：中断向量表需要64字节对齐
4. **中断清除**：在中断服务程序中必须清除相应的中断标志
5. **执行时间**：中断服务程序应尽量简短，避免长时间操作
6. **优先级规划**：合理规划中断优先级，避免优先级反转
7. **资源保护**：共享资源在中断和主程序之间需要保护
8. **调试支持**：合理使用串口输出调试信息，但注意中断中的输出可能影响实时性

## 9. 性能优化建议

1. **快速中断选择**：对响应时间要求高的中断使用快速中断
2. **VTF应用**：频繁触发的中断使用VTF功能
3. **优先级优化**：合理设置中断优先级，减少中断延迟
4. **中断合并**：将多个相关的中断合并处理，减少中断次数
5. **DMA使用**：大数据传输使用DMA，减少中断处理
6. **中断屏蔽**：在关键代码段合理使用中断屏蔽
7. **栈空间优化**：合理分配中断栈空间，避免栈溢出
8. **中断标志处理**：及时清除中断标志，避免重复触发

## 10. 典型应用场景

### 10.1 实时控制系统
```c
// 实时控制系统中断配置
void RealTimeControl_Interrupt_Config(void)
{
    // 1. 配置高速ADC采样中断（最高优先级）
    NVIC_SetPriority(ADC1_IRQn, HIGHEST_PRIORITY);
    NVIC_EnableIRQ(ADC1_IRQn);

    // 2. 配置PWM输出中断（高优先级）
    NVIC_SetPriority(TIM1_IRQn, HIGH_PRIORITY);
    NVIC_EnableIRQ(TIM1_IRQn);

    // 3. 配置通信中断（中等优先级）
    NVIC_SetPriority(USART1_IRQn, MEDIUM_PRIORITY);
    NVIC_EnableIRQ(USART1_IRQn);

    // 4. 配置状态监控中断（低优先级）
    NVIC_SetPriority(SysTick_IRQn, LOW_PRIORITY);
    NVIC_EnableIRQ(SysTick_IRQn);

    // 5. 使用VTF优化ADC中断响应
    SetVTFIRQ((u32)ADC1_IRQHandler, ADC1_IRQn, 0, ENABLE);
}
```

### 10.2 通信网关系统
```c
// 通信网关中断配置
void CommunicationGateway_Interrupt_Config(void)
{
    // 1. 配置以太网接收中断（最高优先级）
    NVIC_SetPriority(ETH_IRQn, HIGHEST_PRIORITY);
    NVIC_EnableIRQ(ETH_IRQn);

    // 2. 配置USB数据传输中断（高优先级）
    NVIC_SetPriority(USBHS_IRQn, HIGH_PRIORITY);
    NVIC_EnableIRQ(USBHS_IRQn);

    // 3. 配置串口接收中断（中等优先级）
    NVIC_SetPriority(USART2_IRQn, MEDIUM_PRIORITY);
    NVIC_EnableIRQ(USART2_IRQn);

    // 4. 配置CAN总线中断（中等优先级）
    NVIC_SetPriority(CAN1_IRQn, MEDIUM_PRIORITY);
    NVIC_EnableIRQ(CAN1_IRQn);

    // 5. 使用RAM向量表支持动态协议切换
    // 可以在运行时切换不同协议的中断处理程序
}
```

### 10.3 数据采集系统
```c
// 数据采集系统中断配置
void DataAcquisition_Interrupt_Config(void)
{
    // 1. 配置定时器触发中断（最高优先级）
    NVIC_SetPriority(TIM2_IRQn, HIGHEST_PRIORITY);
    NVIC_EnableIRQ(TIM2_IRQn);

    // 2. 配置DMA传输完成中断（高优先级）
    NVIC_SetPriority(DMA1_Channel1_IRQn, HIGH_PRIORITY);
    NVIC_EnableIRQ(DMA1_Channel1_IRQn);

    // 3. 配置外部触发中断（中等优先级）
    NVIC_SetPriority(EXTI0_IRQn, MEDIUM_PRIORITY);
    NVIC_EnableIRQ(EXTI0_IRQn);

    // 4. 配置存储完成中断（低优先级）
    NVIC_SetPriority(SDIO_IRQn, LOW_PRIORITY);
    NVIC_EnableIRQ(SDIO_IRQn);

    // 5. 使用中断嵌套处理多通道数据采集
}
```

## 11. 总结

CH32V307的中断控制器提供了灵活强大的中断管理功能，支持多层次优先级、快速中断、中断嵌套和向量表动态配置。通过合理配置中断优先级和使用快速中断机制，可以满足各种实时性要求不同的应用场景。开发时需要注意中断服务程序的编写规范、优先级规划和性能优化，确保中断系统稳定可靠工作。INT示例代码为开发者提供了完整的中断使用参考，是学习CH32V307中断系统的重要资料。
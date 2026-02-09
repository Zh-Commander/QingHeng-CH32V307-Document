# CH32V307 PMP物理内存保护使用指南

## 1. 外设概述

PMP（Physical Memory Protection，物理内存保护）是RISC-V架构中的内存保护机制，用于定义内存区域的访问权限和边界。CH32V307作为RISC-V架构的MCU，实现了PMP功能，主要特性包括：

- **内存区域保护**：定义内存区域的读、写、执行权限
- **多种保护模式**：支持TOR、NA4、NAPOT三种保护模式
- **权限控制**：可配置读(R)、写(W)、执行(X)权限
- **锁定功能**：支持区域锁定，锁定后配置不可更改
- **硬件强制**：硬件级内存访问控制，提高系统安全性
- **机器模式配置**：PMP配置必须在机器模式下进行

## 2. 关键代码片段

### 2.1 PMP模式定义和配置

```c
// PMP模式定义
#define PMP_MODE_TOR    0x00000008    // Top of Range模式
#define PMP_MODE_NA4    0x00000010    // Naturally Aligned 4-byte模式
#define PMP_MODE_NAPOT  0x00000018    // Naturally Aligned Power-of-Two模式

// 权限定义
#define PMP_AUTHOR_R    0x00000001    // 读权限
#define PMP_AUTHOR_W    0x00000002    // 写权限
#define PMP_AUTHOR_X    0x00000004    // 执行权限

// 锁定位
#define PMP_LOCK        0x00000080    // 锁定位

// 默认使用NAPOT模式
#define PMP_MODE        PMP_MODE_NAPOT
```

### 2.2 PMP保护范围设置函数

```c
// 设置PMP保护范围
void PMP_Set_Range(uint32_t grop, uint32_t md, uint32_t sd, uint32_t ed)
{
    // 在机器模式下配置PMP寄存器
    // grop: PMP组号（0-3）
    // md: 模式（TOR/NA4/NAPOT）和权限
    // sd: 起始地址
    // ed: 结束地址（对于NAPOT模式表示大小：2^(ed+2)）

    // 使用内联汇编写入CSR寄存器
    switch(grop)
    {
        case 0:
            // 写入pmpaddr0和pmpcfg0寄存器
            __asm volatile("csrw pmpaddr0, %0" : : "r"(sd));
            __asm volatile("csrw pmpcfg0, %0" : : "r"(md));
            break;
        case 1:
            // 写入pmpaddr1和pmpcfg0寄存器
            __asm volatile("csrw pmpaddr1, %0" : : "r"(sd));
            __asm volatile("csrw pmpcfg0, %0" : : "r"(md));
            break;
        case 2:
            // 写入pmpaddr2和pmpcfg0寄存器
            __asm volatile("csrw pmpaddr2, %0" : : "r"(sd));
            __asm volatile("csrw pmpcfg0, %0" : : "r"(md));
            break;
        case 3:
            // 写入pmpaddr3和pmpcfg0寄存器
            __asm volatile("csrw pmpaddr3, %0" : : "r"(sd));
            __asm volatile("csrw pmpcfg0, %0" : : "r"(md));
            break;
    }
}
```

### 2.3 软件中断处理程序（机器模式配置）

```c
// 软件中断处理程序 - 在机器模式下配置PMP
__attribute__((interrupt("WCH-Interrupt-fast"))) void SW_Handler()
{
    // 保存现场
    register int mepc asm ("mepc");
    register int mcause asm ("mcause");

    // 检查中断原因
    if((mcause & 0x3FF) == 3)
    {
        // 软件中断，配置PMP
        switch(PMP_MODE)
        {
            case PMP_MODE_TOR:
                // TOR模式：保护ProtectSec[0x0] - ProtectSec[0x200]区域
                PMP_Set_Range(1, PMP_MODE_TOR | PMP_AUTHOR_R,
                              (uint32_t)(ProtectSec), (uint32_t)&ProtectSec[0x200]);
                break;

            case PMP_MODE_NA4:
                // NA4模式：保护单个4字节区域
                PMP_Set_Range(0, PMP_MODE_NA4 | PMP_AUTHOR_R,
                              (uint32_t)(ProtectSec), (uint32_t)0);
                break;

            case PMP_MODE_NAPOT:
                // NAPOT模式：保护2^(2+2)=16字节区域
                PMP_Set_Range(0, PMP_MODE_NAPOT | PMP_AUTHOR_R,
                              (uint32_t)(ProtectSec), (uint32_t)2);
                break;
        }
    }

    // 清除软件中断挂起标志
    NVIC_ClearPendingIRQ(Software_IRQn);

    // 恢复现场
    mepc += 4;
}
```

### 2.4 主函数中的PMP测试

```c
// 受PMP保护的内存区域
__attribute__ ((aligned(64))) uint8_t ProtectSec[512];

int main(void)
{
    uint8_t i = 0;
    uint32_t FinalOprateAddress = 0;

    SystemCoreClockUpdate();
    Delay_Init();
    USART_Printf_Init(115200);
    printf("SystemClk:%d\r\n", SystemCoreClock);
    printf("ChipID:%08x\r\n", DBGMCU_GetCHIPID());

    // 初始化保护区域
    for(i = 0; i < 200; i++)
    {
        ProtectSec[i] = i;
    }

    // 触发软件中断进入机器模式配置PMP
    NVIC_EnableIRQ(Software_IRQn);
    NVIC_SetPendingIRQ(Software_IRQn);

    // 测试读取受保护内存
    for(i = 0; i < 200; i++)
    {
        printf("No.%d:0x%02x ", i, ProtectSec[i]);
        if((i + 1) % 8 == 0)
        {
            printf("\r\n");
        }
    }
    printf("\r\n");

    // 测试写入受保护内存（将触发硬错误）
    printf("Start write the protect section\r\n");
    for(i = 0; i < 200; i++)
    {
        FinalOprateAddress = (uint32_t)&ProtectSec[i];
        ProtectSec[i] = i + 0x80;  // 这将触发PMP保护错误
    }

    while(1)
    {
        Delay_Ms(1000);
        printf("Main loop running\r\n");
    }
}
```

### 2.5 硬错误处理程序

```c
// 硬错误处理程序
void HardFault_Handler(void)
{
    extern uint32_t FinalOprateAddress;

    printf("I'm in the hardfault\r\n");
    printf("Last Oprate Address is %#p\r\n", FinalOprateAddress);

    // 根据MCAUSE寄存器判断错误类型
    switch (__get_MCAUSE())
    {
        case 1:
            printf("The address you excuse is in pmp range!!\r\n");
            break;
        case 7:
            printf("The address you Store is in pmp range!!\r\n");
            break;
        case 5:
            printf("The address you read is in pmp range!!\r\n");
            break;
        default:
            printf("Other hardfault\r\n");
            break;
    }

    while(1);  // 停止执行
}
```

## 3. 配置数据结构

### 3.1 PMP模式说明

| 模式 | 描述 | 地址寄存器格式 |
|------|------|---------------|
| **TOR** (Top of Range) | 保护一个地址范围，从上一个PMP条目的地址到当前地址 | 结束地址 |
| **NA4** (Naturally Aligned 4-byte) | 保护一个4字节对齐的区域 | 4字节对齐的地址 |
| **NAPOT** (Naturally Aligned Power-of-Two) | 保护一个2的幂次方大小的区域 | 区域大小编码 |

### 3.2 NAPOT模式大小编码

```c
// NAPOT模式大小编码规则：
// 大小 = 2^(N+2) 字节，其中N是编码值
// 例如：
// N=0 -> 4字节 (2^(0+2) = 4)
// N=1 -> 8字节 (2^(1+2) = 8)
// N=2 -> 16字节 (2^(2+2) = 16)
// N=3 -> 32字节 (2^(3+2) = 32)
// ...
// N=31 -> 2^33字节 (8GB)
```

### 3.3 PMP配置寄存器位域

```c
// pmpcfg寄存器位域（每个PMP条目8位）
// 位[7]: L (Lock) - 锁定位
// 位[6:5]: A (Address Matching) - 地址匹配模式
//     00: OFF (禁用)
//     01: TOR (Top of Range)
//     10: NA4 (4字节对齐)
//     11: NAPOT (2的幂次方对齐)
// 位[4:3]: 保留
// 位[2]: X (eXecute) - 执行权限
// 位[1]: W (Write) - 写权限
// 位[0]: R (Read) - 读权限
```

## 4. 通用配置步骤

### 4.1 PMP配置基本流程

1. **定义保护区域**：
   ```c
   // 定义需要保护的内存区域（64字节对齐）
   __attribute__ ((aligned(64))) uint8_t ProtectedRegion[256];
   ```

2. **触发软件中断进入机器模式**：
   ```c
   NVIC_EnableIRQ(Software_IRQn);
   NVIC_SetPendingIRQ(Software_IRQn);
   ```

3. **在软件中断处理程序中配置PMP**：
   ```c
   // 使用内联汇编写入CSR寄存器
   __asm volatile("csrw pmpaddr0, %0" : : "r"(address));
   __asm volatile("csrw pmpcfg0, %0" : : "r"(config));
   ```

4. **配置硬错误处理程序**：
   ```c
   // 当访问受保护内存时，会触发硬错误中断
   void HardFault_Handler(void)
   {
       // 处理PMP保护错误
   }
   ```

### 4.2 TOR模式配置示例

```c
// TOR模式：保护从StartAddr到EndAddr的区域
void PMP_Configure_TOR(uint32_t pmp_index, uint32_t start_addr, uint32_t end_addr, uint8_t permissions)
{
    uint32_t config = 0;

    // 构建配置：TOR模式 + 权限 + 可选锁定
    config = PMP_MODE_TOR | permissions;

    // 对于TOR模式，需要配置两个PMP条目：
    // 第一个条目：设置起始地址边界
    // 第二个条目：设置结束地址边界

    // 配置第一个条目（边界地址）
    __asm volatile("csrw pmpaddr%d, %0" : : "r"(pmp_index), "r"(start_addr));
    __asm volatile("csrw pmpcfg0, %0" : : "r"(config & 0xFF));

    // 配置第二个条目（结束地址）
    __asm volatile("csrw pmpaddr%d, %0" : : "r"(pmp_index+1), "r"(end_addr));
    __asm volatile("csrw pmpcfg0, %0" : : "r"(config >> 8));
}
```

### 4.3 NA4模式配置示例

```c
// NA4模式：保护单个4字节对齐的区域
void PMP_Configure_NA4(uint32_t pmp_index, uint32_t address, uint8_t permissions)
{
    uint32_t config = 0;

    // 构建配置：NA4模式 + 权限
    config = PMP_MODE_NA4 | permissions;

    // 确保地址4字节对齐
    address = address & ~0x3;

    // 配置PMP
    __asm volatile("csrw pmpaddr%d, %0" : : "r"(pmp_index), "r"(address));
    __asm volatile("csrw pmpcfg0, %0" : : "r"(config));
}
```

### 4.4 NAPOT模式配置示例

```c
// NAPOT模式：保护2的幂次方大小的区域
void PMP_Configure_NAPOT(uint32_t pmp_index, uint32_t base_address, uint32_t size_power, uint8_t permissions)
{
    uint32_t config = 0;
    uint32_t pmpaddr_value = 0;

    // 构建配置：NAPOT模式 + 权限
    config = PMP_MODE_NAPOT | permissions;

    // 计算PMPADDR寄存器值
    // NAPOT编码：base_address | ((1 << (size_power-1)) - 1)
    pmpaddr_value = (base_address >> 2) | ((1 << (size_power-1)) - 1);

    // 配置PMP
    __asm volatile("csrw pmpaddr%d, %0" : : "r"(pmp_index), "r"(pmpaddr_value));
    __asm volatile("csrw pmpcfg0, %0" : : "r"(config));
}
```

## 5. 示例工程详解

### 5.1 PMP示例工程

- **位置**：`PMP/PMP/User/main.c`
- **功能**：演示三种PMP模式的使用和错误处理
- **特点**：
  - 支持TOR、NA4、NAPOT三种PMP模式
  - 演示内存保护错误触发和处理
  - 使用软件中断进入机器模式配置PMP
  - 完整的硬错误处理机制
- **适用场景**：
  - 提高系统安全性
  - 防止非法内存访问
  - 内存区域隔离保护

### 5.2 关键功能点

1. **模式选择**：通过`PMP_MODE`宏选择测试模式
2. **保护区域**：定义`ProtectSec`数组作为受保护区域
3. **机器模式配置**：通过软件中断切换到机器模式配置PMP
4. **错误触发**：故意访问受保护内存触发硬错误
5. **错误诊断**：通过MCAUSE寄存器判断错误类型

### 5.3 测试流程

1. **初始化系统时钟和串口**
2. **初始化保护区域数据**
3. **触发软件中断配置PMP**
4. **读取受保护内存**（正常操作）
5. **写入受保护内存**（触发PMP保护错误）
6. **进入硬错误处理程序**，显示错误信息

## 6. 常见问题

### Q1: PMP配置不生效？
A: 检查以下配置：
1. 是否在机器模式下配置PMP（通过软件中断）
2. PMP寄存器地址是否正确
3. 内存区域是否对齐（64字节对齐）
4. 权限配置是否正确

### Q2: 如何判断PMP保护错误类型？
A: 通过MCAUSE寄存器值判断：
- `1`: 指令访问错误（执行权限）
- `5`: 加载访问错误（读权限）
- `7`: 存储访问错误（写权限）

### Q3: PMP支持多少个保护区域？
A: CH32V307支持多个PMP条目（具体数量参考芯片手册），示例中使用了4个条目（0-3）

### Q4: 锁定功能有什么作用？
A: 锁定功能：
1. 防止PMP配置被意外修改
2. 锁定后，只有系统复位才能更改配置
3. 提高系统安全性，防止恶意修改保护规则

### Q5: 三种PMP模式有什么区别？
A: 主要区别：
| 模式 | 保护粒度 | 适用场景 |
|------|---------|----------|
| **TOR** | 地址范围 | 保护连续内存区域 |
| **NA4** | 4字节 | 保护单个变量或小结构体 |
| **NAPOT** | 2的幂次方 | 灵活的大小配置 |

## 7. RISC-V CSR寄存器参考

### 7.1 PMP相关CSR寄存器

| 寄存器 | 功能描述 | 访问权限 |
|--------|----------|----------|
| `pmpcfg0` | PMP配置寄存器0 | M-mode RW |
| `pmpcfg1` | PMP配置寄存器1 | M-mode RW |
| `pmpcfg2` | PMP配置寄存器2 | M-mode RW |
| `pmpcfg3` | PMP配置寄存器3 | M-mode RW |
| `pmpaddr0-15` | PMP地址寄存器0-15 | M-mode RW |

### 7.2 错误相关CSR寄存器

| 寄存器 | 功能描述 | 访问权限 |
|--------|----------|----------|
| `mcause` | 机器模式异常原因 | M-mode R |
| `mepc` | 机器模式异常程序计数器 | M-mode RW |
| `mtval` | 机器模式异常值 | M-mode RW |

### 7.3 CSR访问指令

```c
// 读取CSR寄存器
uint32_t read_csr(uint32_t csr)
{
    uint32_t value;
    __asm volatile("csrr %0, %1" : "=r"(value) : "i"(csr));
    return value;
}

// 写入CSR寄存器
void write_csr(uint32_t csr, uint32_t value)
{
    __asm volatile("csrw %0, %1" : : "i"(csr), "r"(value));
}

// 设置CSR寄存器位
void set_csr(uint32_t csr, uint32_t mask)
{
    __asm volatile("csrs %0, %1" : : "i"(csr), "r"(mask));
}

// 清除CSR寄存器位
void clear_csr(uint32_t csr, uint32_t mask)
{
    __asm volatile("csrc %0, %1" : : "i"(csr), "r"(mask));
}
```

## 8. 注意事项

1. **机器模式要求**：PMP配置必须在机器模式下进行
2. **内存对齐**：受保护内存区域需要适当对齐
3. **权限规划**：合理规划内存区域的读、写、执行权限
4. **锁定谨慎**：锁定PMP配置后无法修改，需谨慎使用
5. **错误处理**：必须实现硬错误处理程序处理PMP保护错误
6. **性能影响**：PMP检查会增加内存访问延迟

## 9. 性能优化建议

1. **合理分区**：根据应用需求合理划分内存保护区域
2. **减少条目**：尽量减少PMP条目数量，减少硬件开销
3. **区域合并**：合并相邻的相同权限区域，减少PMP条目使用
4. **权限优化**：根据实际需求配置最小必要权限
5. **错误处理优化**：优化硬错误处理程序，减少错误处理时间

## 10. 典型应用场景

### 10.1 代码保护

```c
// 保护关键代码区域，防止非法执行
void Protect_CodeSection(void)
{
    // 保护代码区域，只允许执行，不允许读写
    PMP_Configure_TOR(0, CODE_START, CODE_END, PMP_AUTHOR_X);

    // 或者使用NAPOT模式保护代码段
    PMP_Configure_NAPOT(1, (uint32_t)&CriticalFunction, 4, PMP_AUTHOR_X);
}
```

### 10.2 数据保护

```c
// 保护敏感数据区域，防止非法访问
void Protect_DataSection(void)
{
    // 保护密钥存储区域，只允许读，不允许写和执行
    PMP_Configure_NA4(2, (uint32_t)&EncryptionKey, PMP_AUTHOR_R);

    // 保护配置数据，允许读写，不允许执行
    PMP_Configure_TOR(3, CONFIG_START, CONFIG_END,
                     PMP_AUTHOR_R | PMP_AUTHOR_W);
}
```

### 10.3 内存隔离

```c
// 在多任务系统中隔离不同任务的内存空间
void Task_MemoryIsolation(uint32_t task_id, uint32_t task_mem_start, uint32_t task_mem_size)
{
    uint32_t pmp_index = task_id * 2;  // 每个任务使用2个PMP条目

    // 任务1：保护自己的内存区域
    PMP_Configure_TOR(pmp_index, task_mem_start,
                     task_mem_start + task_mem_size,
                     PMP_AUTHOR_R | PMP_AUTHOR_W | PMP_AUTHOR_X);

    // 任务2：无法访问其他任务的内存区域
    // 通过PMP配置实现内存空间隔离
}
```

## 11. 关于"可编程数学处理器"的澄清

根据代码分析，PMP在CH32V307中指的是**物理内存保护**（Physical Memory Protection），而不是可编程数学处理器。PMP是RISC-V架构的标准功能，用于内存访问控制和系统安全性提升。

如果您需要数学处理相关的功能，请参考：
- **FPU**（浮点运算单元）示例：浮点数学运算
- **CRC**（循环冗余校验）示例：数据校验计算
- **RNG**（随机数生成器）示例：随机数生成

这些才是与数学运算相关的硬件模块。PMP是内存保护机制，用于提高系统的安全性和稳定性。

## 12. 总结

CH32V307的PMP功能提供了硬件级的内存保护机制，是RISC-V架构的重要安全特性。通过合理配置PMP，可以实现内存区域隔离、访问权限控制和系统安全加固。开发时需要注意机器模式配置、内存对齐和错误处理等关键环节。PMP特别适用于对安全性要求高的嵌入式系统，如物联网设备、工业控制和金融设备等。
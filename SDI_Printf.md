# CH32V307 SDI_Printf串行调试接口打印使用指南

## 1. 外设概述

SDI_Printf（Serial Debug Interface Printf）是CH32V307通过调试接口实现printf输出功能的特殊调试方式。主要特性包括：

- **无需硬件串口**：通过调试器内存映射寄存器实现数据通信
- **LinkE专用**：仅支持WCH-LinkE调试器（不支持Link/LinkRISC-V）
- **调试接口输出**：数据通过调试接口传输到WCH-LinkUtility显示
- **内存映射寄存器**：使用固定的内存地址实现数据传输
- **软件版本要求**：需要WCH-LinkUtility 1.8及以上版本
- **配置灵活**：通过宏定义选择SDI打印或UART打印

## 2. 关键代码片段

### 2.1 SDI_Printf基本使用示例

```c
#include "debug.h"
#include "ch32v30x.h"

int main(void)
{
    uint32_t i = 0;

    // 系统初始化
    SystemCoreClockUpdate();
    Delay_Init();

#if (SDI_PRINT == SDI_PR_OPEN)
    // 启用SDI打印功能
    SDI_Printf_Enable();
#else
    // 使用UART打印（传统方式）
    USART_Printf_Init(115200);
#endif

    printf("SystemClk:%d\r\n", SystemCoreClock);
    printf("SDI_Printf Test\r\n");

    while(1)
    {
        printf("-DEBUG-PR-%08x\r\n", i++);
        Delay_Ms(1000);
    }
}
```

### 2.2 SDI_Printf核心实现（debug.c）

```c
// 调试接口内存映射地址
#define DEBUG_DATA0_ADDRESS  ((volatile uint32_t*)0xE0000380)
#define DEBUG_DATA1_ADDRESS  ((volatile uint32_t*)0xE0000384)

// 启用SDI打印功能
void SDI_Printf_Enable(void)
{
    *(DEBUG_DATA0_ADDRESS) = 0;  // 初始化数据寄存器
    Delay_Init();
    Delay_Ms(1);
}

// _write函数实现（SDI模式）
#if (SDI_PRINT == SDI_PR_OPEN)
#include <stdio.h>
#include <errno.h>
#include <sys/stat.h>

int _write(int file, char *buf, int size)
{
    int i = 0;
    int writeSize = size;

    do {
        // 等待数据寄存器可用（DEBUG_DATA0_ADDRESS为0表示可写）
        while((*(DEBUG_DATA0_ADDRESS) != 0u)) {
            // 等待
        }

        if(writeSize > 7) {
            // 传输7字节数据
            *(DEBUG_DATA1_ADDRESS) = (*(buf+i+3)) | (*(buf+i+4)<<8) |
                                     (*(buf+i+5)<<16) | (*(buf+i+6)<<24);
            *(DEBUG_DATA0_ADDRESS) = (7u) | (*(buf+i)<<8) |
                                     (*(buf+i+1)<<16) | (*(buf+i+2)<<24);
            i += 7;
            writeSize -= 7;
        } else {
            // 传输剩余数据（小于等于7字节）
            *(DEBUG_DATA1_ADDRESS) = (*(buf+i+3)) | (*(buf+i+4)<<8) |
                                     (*(buf+i+5)<<16) | (*(buf+i+6)<<24);
            *(DEBUG_DATA0_ADDRESS) = (writeSize) | (*(buf+i)<<8) |
                                     (*(buf+i+1)<<16) | (*(buf+i+2)<<24);
            writeSize = 0;
        }
    } while (writeSize);

    return size;
}
#endif
```

### 2.3 SDI_Printf配置宏（debug.h）

```c
/* SDI Printf Definition */
#define SDI_PR_CLOSE   0      // 关闭SDI打印功能
#define SDI_PR_OPEN    1      // 开启SDI打印功能

#ifndef SDI_PRINT
#define SDI_PRINT   SDI_PR_CLOSE  // 默认关闭SDI打印
#endif

// SDI打印使能函数声明
void SDI_Printf_Enable(void);

// UART打印初始化函数声明（替代方案）
void USART_Printf_Init(uint32_t baudrate);
```

### 2.4 项目配置示例

```c
// 方法1：在代码中直接定义
#define SDI_PRINT SDI_PR_OPEN

// 方法2：通过编译器预定义
// gcc编译选项：-DSDI_PRINT=SDI_PR_OPEN
// MDK/Keil：在Options for Target -> C/C++ -> Preprocessor Symbols中添加
// SDI_PRINT=SDI_PR_OPEN

// 方法3：在debug.h中修改默认值
// 修改 #define SDI_PRINT SDI_PR_CLOSE 为 #define SDI_PRINT SDI_PR_OPEN
```

### 2.5 传统UART打印实现

```c
#if (SDI_PRINT == SDI_PR_CLOSE)
// 使用UART1实现printf输出
void USART_Printf_Init(uint32_t baudrate)
{
    GPIO_InitTypeDef GPIO_InitStructure = {0};
    USART_InitTypeDef USART_InitStructure = {0};

    // 使能GPIOA和USART1时钟
    RCC_APB2PeriphClockCmd(RCC_APB2Periph_GPIOA | RCC_APB2Periph_USART1, ENABLE);

    // 配置USART1 Tx (PA9)
    GPIO_InitStructure.GPIO_Pin = GPIO_Pin_9;
    GPIO_InitStructure.GPIO_Mode = GPIO_Mode_AF_PP;
    GPIO_InitStructure.GPIO_Speed = GPIO_Speed_50MHz;
    GPIO_Init(GPIOA, &GPIO_InitStructure);

    // 配置USART1 Rx (PA10)
    GPIO_InitStructure.GPIO_Pin = GPIO_Pin_10;
    GPIO_InitStructure.GPIO_Mode = GPIO_Mode_IN_FLOATING;
    GPIO_Init(GPIOA, &GPIO_InitStructure);

    // 配置USART1参数
    USART_InitStructure.USART_BaudRate = baudrate;
    USART_InitStructure.USART_WordLength = USART_WordLength_8b;
    USART_InitStructure.USART_StopBits = USART_StopBits_1;
    USART_InitStructure.USART_Parity = USART_Parity_No;
    USART_InitStructure.USART_HardwareFlowControl = USART_HardwareFlowControl_None;
    USART_InitStructure.USART_Mode = USART_Mode_Rx | USART_Mode_Tx;
    USART_Init(USART1, &USART_InitStructure);

    // 使能USART1
    USART_Cmd(USART1, ENABLE);
}
#endif
```

## 3. 配置数据结构

### 3.1 SDI内存映射寄存器定义

```c
// 调试接口寄存器地址（内存映射）
#define DEBUG_BASE_ADDRESS    0xE0000000  // 调试接口基地址
#define DEBUG_DATA0_OFFSET    0x0380      // 数据寄存器0偏移
#define DEBUG_DATA1_OFFSET    0x0384      // 数据寄存器1偏移

#define DEBUG_DATA0_ADDRESS   ((volatile uint32_t*)(DEBUG_BASE_ADDRESS + DEBUG_DATA0_OFFSET))
#define DEBUG_DATA1_ADDRESS   ((volatile uint32_t*)(DEBUG_BASE_ADDRESS + DEBUG_DATA1_OFFSET))

// 寄存器位定义
typedef struct
{
    uint32_t data_length : 8;    // 数据长度（0-7字节）
    uint32_t data_byte0  : 8;    // 数据字节0
    uint32_t data_byte1  : 8;    // 数据字节1
    uint32_t data_byte2  : 8;    // 数据字节2
} DEBUG_DATA0_REG;

// DEBUG_DATA1寄存器直接存储数据字节3-6
```

### 3.2 SDI工作模式定义

```c
// SDI工作模式
typedef enum
{
    SDI_MODE_DISABLED = 0,    // SDI功能禁用
    SDI_MODE_PRINTF   = 1,    // SDI打印模式
    SDI_MODE_DEBUG    = 2     // SDI调试模式（保留）
} SDI_ModeTypeDef;

// 打印输出模式
typedef enum
{
    OUTPUT_MODE_UART,         // UART输出模式
    OUTPUT_MODE_SDI           // SDI输出模式
} Output_ModeTypeDef;
```

### 3.3 传输状态定义

```c
// 传输状态
#define SDI_STATUS_READY      0x00000000  // 准备就绪
#define SDI_STATUS_BUSY       0x00000001  // 忙状态
#define SDI_STATUS_ERROR      0x00000002  // 错误状态

// 最大传输长度
#define SDI_MAX_PACKET_SIZE   7           // 单次最大传输7字节
#define SDI_MIN_PACKET_SIZE   1           // 最小传输1字节
```

## 4. 通用配置步骤

### 4.1 SDI_Printf启用步骤

1. **定义SDI_PRINT宏**：
   ```c
   // 在项目配置中定义SDI_PRINT为SDI_PR_OPEN
   #define SDI_PRINT SDI_PR_OPEN

   // 或者通过编译器预定义
   // gcc: -DSDI_PRINT=SDI_PR_OPEN
   // Keil: 在C/C++选项卡的Define框中添加 SDI_PRINT=SDI_PR_OPEN
   ```

2. **包含必要头文件**：
   ```c
   #include "debug.h"     // SDI_Printf定义
   #include "ch32v30x.h"  // 芯片外设定义
   #include <stdio.h>     // printf函数
   ```

3. **初始化系统**：
   ```c
   SystemCoreClockUpdate();  // 更新系统时钟
   Delay_Init();            // 初始化延时函数
   ```

4. **启用SDI打印功能**：
   ```c
   SDI_Printf_Enable();     // 启用SDI打印
   ```

5. **使用printf输出**：
   ```c
   printf("SystemClk: %d\r\n", SystemCoreClock);
   printf("SDI_Printf Test\r\n");
   ```

### 4.2 项目配置步骤（不同IDE）

**WCH IDE配置**：
1. 打开项目属性
2. 选择"C/C++ Build" -> "Settings"
3. 在"Tool Settings" -> "GCC C Compiler" -> "Preprocessor"
4. 添加预定义宏：`SDI_PRINT=SDI_PR_OPEN`

**Keil MDK配置**：
1. 打开"Options for Target"
2. 选择"C/C++"选项卡
3. 在"Preprocessor Symbols"的"Define"框中添加：`SDI_PRINT=SDI_PR_OPEN`

**Eclipse配置**：
1. 右键项目 -> Properties
2. 选择"C/C++ Build" -> "Settings"
3. 在"Tool Settings" -> "Cross GCC Compiler" -> "Preprocessor"
4. 添加定义：`-DSDI_PRINT=SDI_PR_OPEN`

### 4.3 硬件连接要求

1. **调试器要求**：
   - 必须使用WCH-LinkE调试器
   - 不支持WCH-Link或WCH-LinkRISC-V

2. **软件要求**：
   - WCH-LinkUtility 1.8及以上版本
   - 正确安装CH32V307设备支持包

3. **连接步骤**：
   ```
   1. 使用Type-C线连接WCH-LinkE到PC
   2. 连接WCH-LinkE的SWD接口到目标板
   3. 打开WCH-LinkUtility软件
   4. 选择正确的设备和接口
   5. 下载程序到目标板
   6. 在WCH-LinkUtility中查看SDI输出
   ```

### 4.4 数据传输流程

```c
// SDI数据传输流程图
/*
    开始
      │
      ▼
   检查DEBUG_DATA0寄存器
      │
      ├── 如果 != 0 ──┐
      │               │
      ▼               ▼
     等待         寄存器忙
      │               │
      ▼               ▼
   准备数据      超时检测
      │               │
      ▼               ▼
   写入DEBUG_DATA1 ──┘
      │
      ▼
   写入DEBUG_DATA0
      │
      ▼
   更新指针和长度
      │
      ▼
   还有数据？─────是────┐
      │               │
      ▼               │
      否              │
      │               │
      ▼               │
    完成◄─────────────┘
*/
```

## 5. 示例工程详解

### 5.1 SDI_Printf示例工程

- **位置**：`SDI_Printf/SDI_Printf/User/main.c`
- **功能**：演示SDI_Printf基本使用，交替输出系统信息和调试数据
- **特点**：
  - 支持SDI打印和UART打印两种模式
  - 通过宏定义灵活切换输出方式
  - 包含完整的错误处理和延时机制
  - 演示printf格式化输出

### 5.2 工程结构说明

```
SDI_Printf/
├── .cproject                    # Eclipse项目配置
├── .project                    # Eclipse项目文件
├── SDI_Printf.wvproj           # WCH IDE项目文件
└── User/
    ├── ch32v30x_conf.h         # 外设配置文件
    ├── ch32v30x_it.c           # 中断服务程序
    ├── ch32v30x_it.h           # 中断头文件
    ├── debug.c                 # SDI_Printf核心实现
    ├── debug.h                 # SDI_Printf头文件
    ├── main.c                  # 主程序文件
    ├── system_ch32v30x.c       # 系统初始化
    └── system_ch32v30x.h       # 系统头文件
```

### 5.3 关键功能点

1. **模式选择机制**：
   ```c
   #if (SDI_PRINT == SDI_PR_OPEN)
       SDI_Printf_Enable();
   #else
       USART_Printf_Init(115200);
   #endif
   ```

2. **数据传输机制**：
   - 使用内存映射寄存器实现数据传输
   - 单次最多传输7字节数据
   - 自动分块处理长字符串

3. **同步机制**：
   - 通过检查DEBUG_DATA0寄存器状态实现同步
   - 避免数据覆盖和丢失

4. **兼容性设计**：
   - 保留传统UART打印方式
   - 通过宏定义无缝切换

### 5.4 硬件连接要求

SDI_Printf不需要额外的硬件连接，但需要：
- **WCH-LinkE调试器**：必须使用LinkE版本
- **正确接线**：SWD接口正确连接（SWDIO、SWCLK、GND）
- **电源供应**：目标板正常供电

**注意**：SDI_Printf功能与芯片保护功能冲突，启用SDI打印时不能使用读保护、写保护等安全功能。

## 6. 常见问题

### Q1: SDI_Printf没有输出？
A: 检查以下配置：
1. 是否使用WCH-LinkE调试器（不支持Link/LinkRISC-V）
2. WCH-LinkUtility版本是否≥1.8
3. SDI_PRINT宏是否定义为SDI_PR_OPEN
4. 是否调用了SDI_Printf_Enable()函数
5. 调试器连接是否正常

### Q2: 编译时报错"undefined reference to SDI_Printf_Enable"？
A: 确保：
1. 正确包含debug.h头文件
2. debug.c文件已添加到工程中
3. 编译路径设置正确

### Q3: SDI输出速度慢？
A: SDI_Printf特性：
1. 每次最多传输7字节，长字符串需要分块传输
2. 传输速度受调试接口限制
3. 适合低频调试输出，不适合高速数据流

优化建议：
- 减少不必要的调试输出
- 使用简短的调试信息
- 对于大量数据输出，考虑使用UART方式

### Q4: 能否同时使用SDI打印和UART打印？
A: 不能同时使用，需要通过SDI_PRINT宏选择一种模式：
- `SDI_PRINT = SDI_PR_OPEN`：使用SDI打印
- `SDI_PRINT = SDI_PR_CLOSE`：使用UART打印

### Q5: SDI_Printf与芯片保护功能冲突？
A: 是的，启用SDI_Printf时：
1. 不能启用读保护（RDP）功能
2. 不能启用写保护（WRP）功能
3. 其他保护功能也可能受影响

解决方法：
- 调试阶段使用SDI_Printf
- 正式发布时禁用SDI_Printf，启用保护功能

### Q6: 如何判断SDI传输是否正常？
A: 诊断方法：
1. 检查DEBUG_DATA0寄存器状态
2. 在_write函数中添加调试输出
3. 使用WCH-LinkUtility监控数据传输
4. 检查返回值和处理错误情况

## 7. API参考

### 7.1 SDI_Printf控制API

| 函数名 | 功能描述 | 参数说明 | 返回值 |
|--------|----------|----------|--------|
| `SDI_Printf_Enable` | 启用SDI打印功能 | 无 | 无 |
| `USART_Printf_Init` | 初始化UART打印功能 | baudrate：波特率 | 无 |

### 7.2 系统重定向API

| 函数名 | 功能描述 | 参数说明 | 返回值 |
|--------|----------|----------|--------|
| `_write` | 标准输出重定向函数 | file：文件描述符，buf：数据缓冲区，size：数据大小 | 实际写入字节数 |
| `_read` | 标准输入重定向函数 | file：文件描述符，buf：数据缓冲区，size：数据大小 | 实际读取字节数 |

### 7.3 配置宏定义

| 宏定义 | 功能描述 | 可选值 |
|--------|----------|--------|
| `SDI_PRINT` | SDI打印使能控制 | `SDI_PR_CLOSE`(0)：禁用，`SDI_PR_OPEN`(1)：启用 |
| `DEBUG_DATA0_ADDRESS` | 调试数据寄存器0地址 | `0xE0000380` |
| `DEBUG_DATA1_ADDRESS` | 调试数据寄存器1地址 | `0xE0000384` |

### 7.4 常量定义

```c
// SDI状态常量
#define SDI_STATE_IDLE     0  // 空闲状态
#define SDI_STATE_BUSY     1  // 忙状态
#define SDI_STATE_ERROR    2  // 错误状态

// 传输常量
#define SDI_MAX_CHUNK      7  // 最大数据块大小
#define SDI_TIMEOUT_MS     100 // 超时时间（毫秒）
```

## 8. 注意事项

1. **调试器限制**：仅支持WCH-LinkE，不支持其他型号
2. **软件版本**：需要WCH-LinkUtility 1.8及以上版本
3. **保护功能冲突**：启用SDI_Printf时不能使用芯片保护功能
4. **传输性能**：SDI传输速度有限，不适合高速数据流
5. **数据完整性**：确保正确处理数据分块和同步
6. **错误处理**：实现适当的超时和错误处理机制
7. **资源占用**：SDI_Printf使用特定内存地址，避免冲突
8. **兼容性**：与传统UART打印不兼容，需要宏定义切换

## 9. 性能优化建议

1. **减少输出频率**：只在必要时输出调试信息
2. **缩短消息长度**：使用简短的调试消息
3. **批量输出**：积累多个消息后一次性输出
4. **条件编译**：使用条件编译控制调试输出
5. **分级输出**：实现不同级别的调试输出
6. **异步输出**：考虑使用DMA或中断方式提高效率
7. **缓冲机制**：实现输出缓冲区减少等待时间
8. **性能监控**：监控SDI传输性能，优化瓶颈

## 10. 典型应用场景

### 10.1 调试信息输出

```c
// 调试信息分级输出
#define DEBUG_LEVEL_NONE    0
#define DEBUG_LEVEL_ERROR   1
#define DEBUG_LEVEL_WARN    2
#define DEBUG_LEVEL_INFO    3
#define DEBUG_LEVEL_DEBUG   4

#ifndef CURRENT_DEBUG_LEVEL
#define CURRENT_DEBUG_LEVEL DEBUG_LEVEL_INFO
#endif

void Debug_Print(int level, const char *format, ...)
{
    if(level <= CURRENT_DEBUG_LEVEL)
    {
        va_list args;
        va_start(args, format);

        // 添加调试级别前缀
        switch(level)
        {
            case DEBUG_LEVEL_ERROR: printf("[ERROR] "); break;
            case DEBUG_LEVEL_WARN:  printf("[WARN]  "); break;
            case DEBUG_LEVEL_INFO:  printf("[INFO]  "); break;
            case DEBUG_LEVEL_DEBUG: printf("[DEBUG] "); break;
        }

        vprintf(format, args);
        printf("\r\n");
        va_end(args);
    }
}

// 使用示例
Debug_Print(DEBUG_LEVEL_INFO, "System initialized, clock: %d Hz", SystemCoreClock);
Debug_Print(DEBUG_LEVEL_ERROR, "Sensor read failed, error code: %d", error_code);
```

### 10.2 系统状态监控

```c
// 周期性输出系统状态
void System_Status_Monitor(void)
{
    static uint32_t last_report_time = 0;
    uint32_t current_time = GetTickCount();

    // 每5秒报告一次系统状态
    if(current_time - last_report_time >= 5000)
    {
        printf("=== System Status Report ===\r\n");
        printf("System Clock: %d Hz\r\n", SystemCoreClock);
        printf("Free Heap: %d bytes\r\n", GetFreeHeapSize());
        printf("CPU Usage: %d%%\r\n", GetCPUUsage());
        printf("Uptime: %d seconds\r\n", current_time / 1000);
        printf("===========================\r\n");

        last_report_time = current_time;
    }
}
```

### 10.3 调试命令接口

```c
// 简单的调试命令解析
void Debug_Command_Handler(char *command)
{
    if(strcmp(command, "help") == 0)
    {
        printf("Available commands:\r\n");
        printf("  help    - Show this help\r\n");
        printf("  status  - Show system status\r\n");
        printf("  reset   - Reset system\r\n");
        printf("  version - Show firmware version\r\n");
    }
    else if(strcmp(command, "status") == 0)
    {
        printf("System status:\r\n");
        printf("  Clock: %d Hz\r\n", SystemCoreClock);
        printf("  Voltage: %.2f V\r\n", ReadVoltage());
        printf("  Temperature: %.1f °C\r\n", ReadTemperature());
    }
    else if(strcmp(command, "version") == 0)
    {
        printf("Firmware Version: %s\r\n", FIRMWARE_VERSION);
        printf("Build Date: %s\r\n", BUILD_DATE);
    }
    else
    {
        printf("Unknown command: %s\r\n", command);
        printf("Type 'help' for available commands\r\n");
    }
}
```

## 11. 与传统UART打印对比

| 特性 | SDI_Printf | UART打印 |
|------|------------|----------|
| **硬件需求** | 仅需调试器 | 需要UART引脚和外设 |
| **连接方式** | 调试接口 | 串口线连接 |
| **软件需求** | WCH-LinkUtility | 串口终端软件 |
| **传输速度** | 较慢（调试接口限制） | 快（可配置波特率） |
| **引脚占用** | 不占用GPIO | 占用TX/RX引脚 |
| **保护功能** | 冲突（不能启用） | 兼容（可正常使用） |
| **适用范围** | 调试阶段 | 调试和生产阶段 |
| **设置复杂度** | 简单（宏定义切换） | 中等（需配置引脚和参数） |

## 12. 总结

SDI_Printf为CH32V307提供了一种便捷的调试输出方式，通过调试接口实现printf功能，无需占用硬件串口资源。特别适合在调试阶段使用，可以简化硬件连接和调试流程。开发时需要注意调试器兼容性、软件版本要求和保护功能冲突等关键环节。对于正式产品，建议使用传统的UART打印方式，以获得更好的兼容性和性能。
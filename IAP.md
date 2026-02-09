# CH32V307 IAP在应用编程使用指南

## 1. 外设概述

IAP（In Application Programming，在应用编程）是单片机的重要功能，允许在应用程序运行过程中对Flash存储器进行编程，实现固件的在线升级。CH32V307提供了完整的IAP解决方案，主要特性包括：

- **双程序结构**：独立的Bootloader和应用程序，支持安全跳转
- **多种升级模式**：命令模式和IO模式，灵活适应不同应用场景
- **多重通信接口**：支持UART和USB两种固件传输方式
- **安全校验机制**：数据校验和累加和校验，确保固件完整性
- **高效的Flash操作**：支持快速编程模式，提高升级速度
- **可靠的跳转机制**：软件中断或系统复位实现安全程序切换
- **完整的工具链**：提供Windows端IAP工具和详细使用说明

## 2. 关键代码片段

### 2.1 IAP Bootloader主程序

```c
int main(void)
{
    SystemCoreClockUpdate();
    Delay_Init();
    USART_Printf_Init(115200);
    printf("IAP\r\n");

#if UPGRADE_MODE == UPGRADE_MODE_COMMAND
    // 命令模式：检查跳转标志
    if(*(uint32_t*)FLASH_Base  != 0xe339e339 )  // 检查应用程序是否存在
    {
        if(*(uint32_t*)CalAddr != CheckNum)     // 检查是否需要跳转
        {
            IAP_2_APP();  // 跳转到应用程序
            while(1);
        }
    }
#elif UPGRADE_MODE == UPGRADE_MODE_IO
    // IO模式：检查PA0引脚状态
    if(PA0_Check() == 0)  // PA0为低电平时进入IAP
    {
        IAP_2_APP();  // 跳转到应用程序
        while(1);
    }
#endif

    // 初始化通信接口
    USART3_CFG(460800);
    USBHS_Device_Init(ENABLE);
    USBFS_Init();

    while(1)
    {
        // 处理UART接收
        if(USART_GetFlagStatus(USART3, USART_FLAG_RXNE) != RESET){
            UART_Rx_Deal();
        }
        IWDG_ReloadCounter();

#if UPGRADE_MODE == UPGRADE_MODE_COMMAND
        // 检查升级结束标志
        if (End_Flag) {
            IAP_2_APP();  // 跳转到应用程序
            while(1);
        }
#endif
    }
}
```

### 2.2 IAP到应用程序跳转函数

```c
void IAP_2_APP(void)
{
    // 关闭USB和UART
    USBHS_Device_Init(DISABLE);
    NVIC_DisableIRQ(USBHS_IRQn);
    NVIC_DisableIRQ(USART3_IRQn);

    // 关闭IWDG
    IWDG_WriteAccessCmd(IWDG_WriteAccess_Disable);

    printf("jump APP\r\n");

    // 关闭外设时钟
    GPIO_DeInit(GPIOA);
    GPIO_DeInit(GPIOB);
    USART_DeInit(USART3);
    RCC_APB2PeriphClockCmd(RCC_APB2Periph_GPIOA, DISABLE);
    RCC_APB2PeriphClockCmd(RCC_APB2Periph_GPIOB, DISABLE);
    RCC_APB1PeriphClockCmd(RCC_APB1Periph_USART3, DISABLE);

    Delay_Ms(10);
    NVIC_DisableIRQ(USBFS_IRQn);

    // 触发软件中断实现跳转
    NVIC_EnableIRQ(Software_IRQn);
    NVIC_SetPendingIRQ(Software_IRQn);
}
```

### 2.3 应用程序主程序

```c
int main(void)
{
    SystemCoreClockUpdate();
    Delay_Init();
    USART_Printf_Init(115200);
    printf("APP\r\n");

    // 初始化ADC等应用功能
    ADC_Function_Init();

    // 初始化通信接口
    USART3_CFG(460800);
    USBHS_Device_Init(ENABLE);
    USBFS_Init();
    USART3_IT_CFG();

    while(1)
    {
        IWDG_ReloadCounter();

        // 检查是否需要跳转到IAP
        if(*(uint32_t*)CalAddr == CheckNum)
        {
             Delay_Ms(10);
             APP_2_IAP();  // 跳转到IAP
             while(1);
        }
    }
}
```

### 2.4 应用程序到IAP跳转函数

```c
void APP_2_IAP(void)
{
    NVIC_SystemReset();  // 系统复位，重新从IAP启动
    while(1){
    }
}
```

### 2.5 UART通信协议处理

```c
void UART_Rx_Deal(void)
{
    static u8 RxNum = 0;
    u16 Check = 0;
    u8 len = 0;
    u8 i = 0;

    RxBuf[RxNum] = USART_ReceiveData(USART3);

    // 帧头检测 (0xAA 0x55)
    if(RxNum == 0 && RxBuf[0] != 0xAA)
        RxNum = 0;
    else if(RxNum == 1 && RxBuf[1] != 0x55)
        RxNum = 0;
    else
        RxNum++;

    // 完整帧处理
    if(RxNum >= 10)  // 最小帧长度10字节
    {
        len = RxBuf[3];  // 数据长度

        // 帧尾检测 (0x55 0xAA)
        if(RxBuf[4+len+4] == 0x55 && RxBuf[4+len+5] == 0xAA)
        {
            // 累加和校验
            for(i = 0; i < len + 4; i++)
            {
                Check += RxBuf[4 + i];
            }

            if((RxBuf[4+len] == (Check & 0xFF)) &&
               (RxBuf[4+len+1] == (Check >> 8)))
            {
                // 校验通过，处理命令
                IAP_DealCommand((isp_cmd*)(&RxBuf[2]));
            }
        }
        RxNum = 0;
    }
}
```

### 2.6 IAP命令处理核心

```c
void IAP_DealCommand(isp_cmd *isp_cmd_t)
{
    u8 i, s = ERR_OK;

    switch (isp_cmd_t->UART.Cmd)
    {
        case CMD_IAP_PROM:  // 编程命令
            // 接收数据到缓冲区
            for (i = 0; i < Lenth; i++) {
                Fast_Program_Buf[CodeLen + i] = isp_cmd_t->program.data[i];
            }
            CodeLen += Lenth;

            // 缓冲区满256字节时执行编程
            if (CodeLen >= 256) {
                FLASH_Unlock_Fast();              // 解锁Flash
                FLASH_ErasePage_Fast(Program_addr); // 擦除页
                CH32_IAP_Program(Program_addr, (u32*) Fast_Program_Buf); // 编程
                CodeLen -= 256;
                Program_addr += 0x100;            // 地址增加256字节
            }
            break;

        case CMD_IAP_ERASE:  // 擦除命令
            FLASH_Unlock_Fast();  // 解锁Flash准备擦除
            break;

        case CMD_IAP_VERIFY:  // 校验命令
            // 数据校验
            for (i = 0; i < Lenth; i++) {
                if (isp_cmd_t->verify.data[i] != *(u8*) (Verify_addr + i)) {
                    s = ERR_ERROR;  // 校验失败
                    break;
                }
            }
            Verify_addr += Lenth;  // 更新校验地址
            break;

        case CMD_IAP_END:  // 结束命令
            End_Flag = 1;  // 设置升级结束标志
            break;

        default:
            break;
    }

    // 发送响应
    Send_Response(s);
}
```

## 3. 配置数据结构

### 3.1 IAP关键宏定义

```c
// 内存地址定义
#define FLASH_Base        0x08005000      // 应用程序起始地址
#define CalAddr           (0x08078000-4)  // 跳转标志地址
#define CheckNum          (0x5aa55aa5)    // 跳转标志值

// 通信命令定义
#define CMD_IAP_PROM      0x80            // 编程命令
#define CMD_IAP_ERASE     0x81            // 擦除命令
#define CMD_IAP_VERIFY    0x82            // 校验命令
#define CMD_IAP_END       0x83            // 结束命令
#define CMD_JUMP_IAP      0x84            // 跳转到IAP命令

// 升级模式定义
#define UPGRADE_MODE_COMMAND   0          // 命令模式
#define UPGRADE_MODE_IO        1          // IO模式
#define UPGRADE_MODE   UPGRADE_MODE_COMMAND  // 当前使用的模式

// Flash页大小
#define FLASH_PAGE_SIZE   0x400           // 1KB页大小

// 应用程序存在标志
#define APP_EXIST_FLAG    0xe339e339      // 应用程序存在标志
```

### 3.2 通信协议数据结构

```c
typedef union __attribute__ ((aligned(4)))_ISP_CMD {
    struct {
        u8 Cmd;           // 命令字节
        u8 Len;           // 数据长度
        u8 data[64];      // 数据缓冲区
    } UART;               // UART通用格式

    struct {
        u8 Cmd;           // 命令字节
        u8 Len;           // 数据长度
        u8 data[62];      // 编程数据
    } program;            // 编程命令格式

    struct {
        u8 Cmd;           // 命令字节
        u8 Len;           // 数据长度
        u8 addr[4];       // 地址字段（4字节）
        u8 data[56];      // 校验数据
    } verify;             // 校验命令格式

    struct {
        u8 buf[64+4];     // 原始缓冲区
    } other;              // 其他格式
} isp_cmd;                // IAP命令联合体
```

### 3.3 全局变量定义

```c
// IAP状态变量
volatile u8 End_Flag = 0;          // 升级结束标志
volatile u32 Program_addr = 0;     // 编程地址
volatile u32 Verify_addr = 0;      // 校验地址
volatile u16 CodeLen = 0;          // 编程数据长度

// 编程缓冲区
__attribute__ ((aligned(4))) u32 Fast_Program_Buf[64];  // 256字节对齐缓冲区

// 接收缓冲区
u8 RxBuf[128];                     // UART接收缓冲区
```

## 4. 通用配置步骤

### 4.1 内存布局规划

1. **Bootloader区域**：
   - 起始地址：0x08000000
   - 大小：20KB（0x00005000）
   - 功能：IAP程序，负责固件升级和跳转

2. **应用程序区域**：
   - 起始地址：0x08005000
   - 大小：228KB（0x00039000）
   - 功能：用户应用程序

3. **跳转标志区域**：
   - 地址：0x08077FFC（Flash末尾-4）
   - 大小：4字节
   - 功能：存储跳转标志，用于APP→IAP跳转控制

### 4.2 Bootloader配置步骤

1. **链接脚本配置**：
   ```ld
   MEMORY
   {
       FLASH (rx) : ORIGIN = 0x00000000, LENGTH = 20K    # Bootloader区域
       RAM (xrw) : ORIGIN = 0x20000000, LENGTH = 32K
   }
   ```

2. **系统初始化**：
   ```c
   SystemCoreClockUpdate();
   Delay_Init();
   USART_Printf_Init(115200);
   printf("IAP\r\n");
   ```

3. **跳转条件检查**：
   ```c
   // 检查应用程序是否存在
   if(*(uint32_t*)FLASH_Base != APP_EXIST_FLAG) {
       // 检查跳转标志
       if(*(uint32_t*)CalAddr != CheckNum) {
           IAP_2_APP();  // 跳转到应用程序
       }
   }
   ```

4. **通信接口初始化**：
   ```c
   USART3_CFG(460800);          // UART3初始化
   USBHS_Device_Init(ENABLE);   // USB高速初始化
   USBFS_Init();                // USB全速初始化
   ```

### 4.3 应用程序配置步骤

1. **链接脚本配置**：
   ```ld
   MEMORY
   {
       FLASH (rx) : ORIGIN = 0x00005000, LENGTH = 228K   # 应用程序区域
       RAM (xrw) : ORIGIN = 0x20000000, LENGTH = 32K
   }
   ```

2. **应用程序主程序**：
   ```c
   int main(void)
   {
       SystemCoreClockUpdate();
       Delay_Init();
       USART_Printf_Init(115200);
       printf("APP\r\n");

       // 应用程序初始化
       App_Init();

       while(1) {
           // 应用程序主循环
           App_MainLoop();

           // 检查跳转到IAP标志
           if(*(uint32_t*)CalAddr == CheckNum) {
               APP_2_IAP();  // 跳转到IAP
           }
       }
   }
   ```

3. **跳转到IAP函数**：
   ```c
   void APP_2_IAP(void)
   {
       // 写入跳转标志
       FLASH_Unlock_Fast();
       FLASH_ErasePage_Fast(CalAddr);
       FLASH_ProgramWord(CalAddr, CheckNum);
       FLASH_Lock_Fast();

       // 系统复位
       NVIC_SystemReset();
   }
   ```

### 4.4 Flash操作步骤

1. **Flash解锁**：
   ```c
   FLASH_Unlock_Fast();  // 快速解锁Flash
   ```

2. **页擦除**：
   ```c
   FLASH_ErasePage_Fast(addr);  // 擦除指定地址的Flash页
   ```

3. **数据编程**：
   ```c
   // 单字编程
   FLASH_ProgramWord(addr, data);

   // 快速页编程
   CH32_IAP_Program(addr, buffer);
   ```

4. **Flash加锁**：
   ```c
   FLASH_Lock_Fast();  // 快速加锁Flash
   ```

### 4.5 通信协议配置

1. **帧格式定义**：
   ```
   [同步头1] [同步头2] [命令] [长度] [地址] [数据] [校验低] [校验高] [同步尾2] [同步尾1]
     0xAA      0x55     Cmd   Len   Addr   Data   CRC_L   CRC_H   0x55      0xAA
   ```

2. **波特率设置**：
   ```c
   #define UART_BAUDRATE  460800  // 升级波特率
   ```

3. **校验算法**：
   ```c
   u16 CalculateChecksum(u8 *data, u8 len)
   {
       u16 sum = 0;
       for(u8 i = 0; i < len; i++) {
           sum += data[i];
       }
       return sum;
   }
   ```

## 5. 示例工程详解

### 5.1 IAP Bootloader工程

- **位置**：`IAP/USB_UART/CHV30x_IAP/`
- **功能**：IAP引导程序，负责固件升级和程序跳转
- **特点**：
  - 支持命令模式和IO模式两种升级方式
  - 提供UART和USB两种通信接口
  - 实现完整的Flash编程流程
  - 包含安全校验机制
  - 支持应用程序跳转
- **适用场景**：
  - 需要在线升级的嵌入式系统
  - 远程固件更新应用
  - 产品现场升级维护

### 5.2 应用程序工程

- **位置**：`IAP/USB_UART/CHV30x_APP/`
- **功能**：用户应用程序，可通过IAP进行升级
- **特点**：
  - 独立的应用程序区域
  - 支持跳转到IAP功能
  - 包含应用功能示例（ADC等）
  - 与Bootloader配合实现安全升级
- **适用场景**：
  - 实际产品应用
  - 需要升级功能的应用
  - 演示应用程序开发

### 5.3 Windows端IAP工具

- **文件**：`WCHMcuIAP_WinAPP.exe`
- **功能**：PC端IAP升级工具
- **特点**：
  - 图形化操作界面
  - 支持固件文件选择
  - 显示升级进度和状态
  - 支持UART和USB连接
- **使用方式**：
  1. 选择固件文件（.bin或.hex格式）
  2. 选择通信端口（串口或USB）
  3. 配置通信参数
  4. 开始升级

### 5.4 使用说明文档

- **文件**：`CH32V30x_IAP使用说明.pdf`
- **内容**：
  - IAP系统架构说明
  - 硬件连接要求
  - 软件配置步骤
  - 升级操作流程
  - 故障排除指南
- **重要性**：详细的使用说明，确保正确使用IAP功能

## 6. 常见问题

### Q1: IAP无法跳转到应用程序？
A: 检查以下配置：
1. 应用程序是否存在（检查FLASH_Base地址内容）
2. 跳转标志是否正确
3. IAP跳转函数是否正确关闭外设
4. 应用程序的链接脚本起始地址是否正确

### Q2: Flash编程失败？
A: 可能原因：
1. Flash未正确解锁
2. 编程地址未对齐
3. Flash页未先擦除
4. 编程数据长度不正确
5. Flash保护使能

### Q3: 通信协议校验失败？
A: 检查以下配置：
1. 波特率设置是否一致
2. 帧格式是否正确
3. 校验算法是否匹配
4. 数据长度是否正确
5. 同步头尾是否匹配

### Q4: 应用程序无法跳转到IAP？
A: 检查以下配置：
1. 跳转标志是否正确写入
2. 应用程序是否正确读取跳转标志
3. APP_2_IAP函数是否正确实现
4. 系统复位是否正常工作

### Q5: 升级过程中断导致系统无法启动？
A: 处理方法：
1. 强制进入IAP模式（IO模式下拉PA0）
2. 重新发送完整固件
3. 确保升级过程不受干扰
4. 使用看门狗防止系统死锁

## 7. API参考

### 7.1 Flash操作API

| 函数名 | 功能描述 | 参数说明 |
|--------|----------|----------|
| `FLASH_Unlock_Fast` | 快速解锁Flash | 无 |
| `FLASH_Lock_Fast` | 快速加锁Flash | 无 |
| `FLASH_ErasePage_Fast` | 快速擦除Flash页 | Page_Address：页地址 |
| `FLASH_ProgramWord` | 字编程Flash | Address：地址，Data：数据 |
| `CH32_IAP_Program` | IAP编程函数 | adr：地址，buf：数据缓冲区 |

### 7.2 跳转控制API

| 函数名 | 功能描述 | 参数说明 |
|--------|----------|----------|
| `IAP_2_APP` | IAP跳转到应用程序 | 无 |
| `APP_2_IAP` | 应用程序跳转到IAP | 无 |
| `NVIC_SystemReset` | 系统复位 | 无 |
| `NVIC_EnableIRQ` | 使能中断 | IRQn：中断号 |
| `NVIC_SetPendingIRQ` | 设置中断挂起 | IRQn：中断号 |

### 7.3 通信处理API

| 函数名 | 功能描述 | 参数说明 |
|--------|----------|----------|
| `USART3_CFG` | 配置USART3 | baudrate：波特率 |
| `UART_Rx_Deal` | UART接收处理 | 无 |
| `IAP_DealCommand` | IAP命令处理 | isp_cmd_t：命令指针 |
| `Send_Response` | 发送响应 | status：响应状态 |

### 7.4 系统初始化API

| 函数名 | 功能描述 | 参数说明 |
|--------|----------|----------|
| `SystemCoreClockUpdate` | 更新系统时钟 | 无 |
| `Delay_Init` | 延时初始化 | 无 |
| `USART_Printf_Init` | 串口打印初始化 | bound：波特率 |
| `USBHS_Device_Init` | USB高速设备初始化 | NewState：新状态 |
| `USBFS_Init` | USB全速初始化 | 无 |

## 8. 注意事项

1. **内存规划**：合理规划Bootloader和应用程序的内存空间
2. **中断处理**：跳转前正确关闭所有中断和外设
3. **Flash操作**：遵循解锁-擦除-编程-加锁的正确顺序
4. **通信可靠性**：确保通信过程的可靠性，添加重传机制
5. **电源稳定**：升级过程中保证电源稳定，避免断电
6. **看门狗使用**：合理使用看门狗防止系统死锁
7. **版本管理**：实现固件版本管理，避免错误升级
8. **安全机制**：添加固件校验和安全认证机制

## 9. 性能优化建议

1. **快速编程**：使用快速编程模式提高升级速度
2. **批量操作**：尽量批量处理Flash操作，减少擦除编程次数
3. **缓冲区优化**：合理设置缓冲区大小，平衡内存使用和效率
4. **通信优化**：使用较高的波特率提高数据传输速度
5. **校验优化**：在传输过程中进行实时校验，避免全部传输完才发现错误
6. **进度反馈**：提供升级进度反馈，方便用户了解升级状态
7. **断点续传**：实现断点续传功能，提高升级可靠性
8. **压缩传输**：对固件进行压缩传输，减少传输数据量

## 10. 典型应用场景

### 10.1 远程固件升级
```c
// 远程升级系统初始化
void RemoteUpgrade_Init(void)
{
    // 1. 初始化网络模块
    Network_Init();

    // 2. 初始化IAP功能
    IAP_Init();

    // 3. 启动升级服务
    Start_Upgrade_Service();
}

// 远程升级处理
void RemoteUpgrade_Process(void)
{
    // 检查远程升级命令
    if(Check_Remote_Command()) {
        // 设置跳转标志
        Set_Jump_Flag();

        // 系统复位进入IAP
        NVIC_SystemReset();
    }
}
```

### 10.2 USB设备升级
```c
// USB设备升级初始化
void USB_Upgrade_Init(void)
{
    // 1. 初始化USB设备
    USB_Device_Init();

    // 2. 配置USB大容量存储设备模式
    USB_MSC_Init();

    // 3. 实现固件文件传输
    Implement_Firmware_Transfer();
}

// USB升级处理
void USB_Upgrade_Process(void)
{
    // 检查USB连接状态
    if(USB_Connected()) {
        // 检测固件文件
        if(Detect_Firmware_File()) {
            // 执行升级流程
            Execute_Upgrade_Process();
        }
    }
}
```

### 10.3 无线升级系统
```c
// 无线升级系统初始化
void WirelessUpgrade_Init(void)
{
    // 1. 初始化无线模块（WiFi/蓝牙）
    Wireless_Module_Init();

    // 2. 配置无线通信协议
    Wireless_Protocol_Config();

    // 3. 启动无线升级服务
    Start_Wireless_Upgrade_Service();
}

// 无线升级处理
void WirelessUpgrade_Process(void)
{
    // 接收无线升级数据
    if(Receive_Wireless_Data()) {
        // 存储到Flash
        Store_To_Flash();

        // 校验数据完整性
        if(Verify_Data_Integrity()) {
            // 设置升级完成标志
            Set_Upgrade_Complete_Flag();
        }
    }
}
```

## 11. 总结

CH32V307的IAP系统提供了完整的在应用编程解决方案，支持灵活的升级方式和可靠的操作流程。通过合理的Bootloader和应用程序设计，可以实现安全可靠的固件升级功能。开发时需要注意内存规划、Flash操作、通信协议和跳转机制等关键环节，确保IAP系统稳定可靠工作。IAP功能为产品维护和功能升级提供了便利，是嵌入式系统开发中不可或缺的重要功能。
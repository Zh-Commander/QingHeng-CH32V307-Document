# CH32V307 BKP备份寄存器使用指南

## 1. 外设概述

BKP（Backup Registers，备份寄存器）是CH32V307的独立寄存器区域，用于在系统掉电或复位时保存关键数据。主要特性包括：

- **独立供电**：通过VBAT引脚供电，主电源掉电时数据仍可保持
- **数据保持**：系统复位或待机模式下数据不丢失
- **入侵检测**：支持入侵检测引脚（TAMPER），检测到入侵事件时自动清除备份寄存器
- **多个寄存器**：提供多个32位备份寄存器
- **低功耗**：在待机模式下仍可保持数据

## 2. 关键代码片段

### 2.1 BKP基本初始化

```c
// BKP和PWR时钟使能
RCC_APB1PeriphClockCmd(RCC_APB1Periph_PWR | RCC_APB1Periph_BKP, ENABLE);

// 使能备份寄存器访问
PWR_BackupAccessCmd(ENABLE);

// 清除BKP标志位
BKP_ClearFlag();

// 写入备份寄存器
BKP_WriteBackupRegister(BKP_DR1, 0x9880);
BKP_WriteBackupRegister(BKP_DR2, 0x5678);
BKP_WriteBackupRegister(BKP_DR3, 0xABCD);
BKP_WriteBackupRegister(BKP_DR4, 0x3456);

// 读取备份寄存器
printf("BKP_DR1:%08x\r\n", BKP->DATAR1);
printf("BKP_DR2:%08x\r\n", BKP->DATAR2);
printf("BKP_DR3:%08x\r\n", BKP->DATAR3);
printf("BKP_DR4:%08x\r\n", BKP->DATAR4);
```

### 2.2 入侵检测初始化（TAMPER示例）

```c
void BKP_Tamper_Init(void)
{
    NVIC_InitTypeDef NVIC_InitStructure = {0};

    // 1. 使能PWR和BKP时钟
    RCC_APB1PeriphClockCmd(RCC_APB1Periph_PWR | RCC_APB1Periph_BKP, ENABLE);

    // 2. 禁用入侵检测引脚（先禁用再配置）
    BKP_TamperPinCmd(DISABLE);

    // 3. 使能备份寄存器访问
    PWR_BackupAccessCmd(ENABLE);

    // 4. 清除所有BKP标志
    BKP_ClearFlag();

    // 5. 写入测试数据到备份寄存器
    BKP_WriteBackupRegister(BKP_DR1, 0x9880);
    BKP_WriteBackupRegister(BKP_DR2, 0x5678);
    BKP_WriteBackupRegister(BKP_DR3, 0xABCD);
    BKP_WriteBackupRegister(BKP_DR4, 0x3456);

    // 6. 打印备份寄存器值
    printf("BKP_DR1:%08x\r\n", BKP->DATAR1);
    printf("BKP_DR2:%08x\r\n", BKP->DATAR2);
    printf("BKP_DR3:%08x\r\n", BKP->DATAR3);
    printf("BKP_DR4:%08x\r\n", BKP->DATAR4);

    // 7. 配置入侵检测引脚电平
    // TPAL:0 - PC13配置为输入下拉（高电平触发）
    BKP_TamperPinLevelConfig(BKP_TamperPinLevel_High);
    // TPAL:1 - PC13配置为输入上拉（低电平触发）
    // BKP_TamperPinLevelConfig(BKP_TamperPinLevel_Low);

    // 8. NVIC中断配置
    NVIC_InitStructure.NVIC_IRQChannel = TAMPER_IRQn;
    NVIC_InitStructure.NVIC_IRQChannelPreemptionPriority = 1;
    NVIC_InitStructure.NVIC_IRQChannelSubPriority = 0;
    NVIC_InitStructure.NVIC_IRQChannelCmd = ENABLE;
    NVIC_Init(&NVIC_InitStructure);

    // 9. 使能BKP中断
    BKP_ITConfig(ENABLE);

    // 10. 使能入侵检测引脚
    BKP_TamperPinCmd(ENABLE);
}
```

### 2.3 入侵检测中断处理函数

```c
// 入侵检测中断服务程序
void TAMPER_IRQHandler(void)
{
    // 检查入侵检测中断标志
    if(BKP_GetITStatus() == SET)
    {
        #if 0
        // 调试信息输出
        printf("TAMPER_IRQHandler\r\n");
        printf("BKP_DR1:%08x\r\n", BKP->DATAR1);
        printf("BKP_DR2:%08x\r\n", BKP->DATAR2);
        printf("BKP_DR3:%08x\r\n", BKP->DATAR3);
        printf("BKP_DR4:%08x\r\n", BKP->DATAR4);
        #endif

        // 入侵检测事件处理
        // 注意：入侵检测事件会自动清除所有备份寄存器内容
    }

    // 清除中断挂起位
    BKP_ClearITPendingBit();
}
```

### 2.4 备份寄存器读写封装函数

```c
// 写入数据到备份寄存器
uint8_t BKP_WriteData(uint8_t reg_index, uint32_t data)
{
    if(reg_index >= BKP_DR_MAX) {
        return 1; // 寄存器索引错误
    }

    // 使能备份寄存器访问
    PWR_BackupAccessCmd(ENABLE);

    // 写入数据
    switch(reg_index) {
        case 0:
            BKP_WriteBackupRegister(BKP_DR1, data);
            break;
        case 1:
            BKP_WriteBackupRegister(BKP_DR2, data);
            break;
        case 2:
            BKP_WriteBackupRegister(BKP_DR3, data);
            break;
        case 3:
            BKP_WriteBackupRegister(BKP_DR4, data);
            break;
        // 可以继续扩展更多寄存器
        default:
            return 2; // 不支持的寄存器
    }

    return 0; // 成功
}

// 从备份寄存器读取数据
uint32_t BKP_ReadData(uint8_t reg_index)
{
    uint32_t data = 0;

    if(reg_index >= BKP_DR_MAX) {
        return 0xFFFFFFFF; // 寄存器索引错误
    }

    // 使能备份寄存器访问
    PWR_BackupAccessCmd(ENABLE);

    // 读取数据
    switch(reg_index) {
        case 0:
            data = BKP_ReadBackupRegister(BKP_DR1);
            break;
        case 1:
            data = BKP_ReadBackupRegister(BKP_DR2);
            break;
        case 2:
            data = BKP_ReadBackupRegister(BKP_DR3);
            break;
        case 3:
            data = BKP_ReadBackupRegister(BKP_DR4);
            break;
        // 可以继续扩展更多寄存器
        default:
            data = 0xFFFFFFFF; // 不支持的寄存器
    }

    return data;
}
```

## 3. 配置数据结构

### 3.1 备份寄存器地址定义

```c
// 备份寄存器物理地址定义
#define BKP_BASE_ADDR         0x40006C00

// 备份寄存器偏移地址
#define BKP_DR1_OFFSET        0x04
#define BKP_DR2_OFFSET        0x08
#define BKP_DR3_OFFSET        0x0C
#define BKP_DR4_OFFSET        0x10
#define BKP_DR5_OFFSET        0x14
#define BKP_DR6_OFFSET        0x18
#define BKP_DR7_OFFSET        0x1C
#define BKP_DR8_OFFSET        0x20
#define BKP_DR9_OFFSET        0x24
#define BKP_DR10_OFFSET       0x28

// 最大备份寄存器数量
#define BKP_DR_MAX            10
```

### 3.2 入侵检测配置枚举

```c
// 入侵检测引脚电平配置
typedef enum {
    BKP_TamperPinLevel_Low = 0x00,   // 低电平触发（引脚上拉）
    BKP_TamperPinLevel_High = 0x01   // 高电平触发（引脚下拉）
} BKP_TamperPinLevel_TypeDef;

// 入侵检测引脚配置
typedef struct {
    BKP_TamperPinLevel_TypeDef level;    // 触发电平
    FunctionalState interrupt_enable;    // 中断使能
    uint8_t priority;                    // 中断优先级
} BKP_TamperConfigTypeDef;
```

## 4. 通用配置步骤

### 4.1 备份寄存器基本使用步骤
1. **时钟使能**：
   ```c
   RCC_APB1PeriphClockCmd(RCC_APB1Periph_PWR | RCC_APB1Periph_BKP, ENABLE);
   ```

2. **使能备份寄存器访问**：
   ```c
   PWR_BackupAccessCmd(ENABLE);
   ```

3. **清除标志位**：
   ```c
   BKP_ClearFlag();
   ```

4. **数据读写**：
   ```c
   // 写入数据
   BKP_WriteBackupRegister(BKP_DRx, data);

   // 读取数据
   data = BKP_ReadBackupRegister(BKP_DRx);
   ```

### 4.2 入侵检测配置步骤
1. **时钟使能**（同上）
2. **使能备份寄存器访问**（同上）
3. **配置入侵检测引脚电平**：
   ```c
   BKP_TamperPinLevelConfig(BKP_TamperPinLevel_High); // 高电平触发
   // 或
   BKP_TamperPinLevelConfig(BKP_TamperPinLevel_Low);  // 低电平触发
   ```

4. **NVIC中断配置**：
   ```c
   NVIC_InitStructure.NVIC_IRQChannel = TAMPER_IRQn;
   NVIC_InitStructure.NVIC_IRQChannelPreemptionPriority = priority;
   NVIC_InitStructure.NVIC_IRQChannelSubPriority = 0;
   NVIC_InitStructure.NVIC_IRQChannelCmd = ENABLE;
   NVIC_Init(&NVIC_InitStructure);
   ```

5. **使能BKP中断**：
   ```c
   BKP_ITConfig(ENABLE);
   ```

6. **使能入侵检测引脚**：
   ```c
   BKP_TamperPinCmd(ENABLE);
   ```

7. **实现中断服务程序**：
   ```c
   void TAMPER_IRQHandler(void)
   {
       if(BKP_GetITStatus() == SET) {
           // 处理入侵事件
           BKP_ClearITPendingBit();
       }
   }
   ```

## 5. 示例工程详解

### 5.1 BKP示例
- **位置**：`BKP/BKP/User/main.c`
- **功能**：备份寄存器基本读写和入侵检测功能
- **特点**：
  - 演示备份寄存器的读写操作
  - 实现入侵检测功能
  - 包含完整的中断处理
  - 支持入侵检测电平配置
- **适用场景**：
  - 系统配置参数存储
  - 运行状态保存
  - 安全检测应用

### 5.2 关键功能点
1. **数据保持**：系统复位或待机模式下数据不丢失
2. **入侵检测**：通过PC13引脚检测入侵事件
3. **自动清除**：入侵事件自动清除备份寄存器内容
4. **中断支持**：支持入侵检测中断

## 6. 常见问题

### Q1: 备份寄存器数据丢失？
A: 检查以下配置：
1. VBAT引脚是否正常供电
2. 是否发生入侵检测事件（会清除所有备份寄存器）
3. 备份寄存器访问是否使能
4. 系统是否经历了待机模式唤醒（某些模式下数据可能不保持）

### Q2: 入侵检测不工作？
A: 可能原因：
1. PC13引脚配置不正确
2. 入侵检测电平配置错误
3. 中断未正确使能或配置
4. 硬件连接问题

### Q3: 备份寄存器读写失败？
A: 检查以下步骤：
1. PWR和BKP时钟是否使能
2. 备份寄存器访问是否使能（`PWR_BackupAccessCmd(ENABLE)`）
3. 寄存器地址是否正确
4. 系统是否处于复位状态

### Q4: 多备份寄存器如何使用？
A: 建议用法：
1. DR1-DR4：存储系统关键参数
2. DR5-DR8：存储用户配置
3. DR9-DR10：存储运行状态标志
4. 使用结构体封装数据，提高可读性

### Q5: 入侵检测和RTC冲突？
A: 注意点：
1. PC13引脚同时用于入侵检测和RTC输出
2. 需要根据应用需求选择功能
3. 不能同时使用两种功能

## 7. API参考

| 函数名 | 功能描述 | 参数说明 |
|--------|----------|----------|
| `BKP_WriteBackupRegister` | 写入备份寄存器 | BKP_DRx：备份寄存器编号，Data：要写入的数据 |
| `BKP_ReadBackupRegister` | 读取备份寄存器 | BKP_DRx：备份寄存器编号，返回值：读取的数据 |
| `BKP_TamperPinLevelConfig` | 配置入侵检测引脚电平 | BKP_TamperPinLevel：电平配置（High/Low） |
| `BKP_TamperPinCmd` | 使能/禁用入侵检测引脚 | NewState：新状态（ENABLE/DISABLE） |
| `BKP_ITConfig` | 使能/禁用BKP中断 | NewState：新状态（ENABLE/DISABLE） |
| `BKP_GetITStatus` | 获取中断状态 | 返回值：中断状态（SET/RESET） |
| `BKP_ClearITPendingBit` | 清除中断挂起位 | 无 |
| `BKP_ClearFlag` | 清除所有BKP标志 | 无 |
| `PWR_BackupAccessCmd` | 使能/禁用备份寄存器访问 | NewState：新状态（ENABLE/DISABLE） |

## 8. 注意事项

1. **电源要求**：备份寄存器需要VBAT引脚供电才能保持数据
2. **引脚冲突**：入侵检测引脚（PC13）与RTC输出功能共享
3. **中断处理**：入侵检测中断需要及时处理，避免多次触发
4. **数据安全**：重要数据建议在多个备份寄存器中存储副本
5. **调试影响**：调试过程中某些操作可能影响备份寄存器数据
6. **低功耗模式**：在低功耗模式下，备份寄存器访问可能受限

## 9. 应用建议

1. **系统配置存储**：存储网络参数、设备ID、校准参数等
2. **运行状态保存**：保存系统运行时间、错误计数、工作模式等
3. **安全监控**：使用入侵检测功能实现物理安全监控
4. **数据恢复**：系统异常复位后从备份寄存器恢复关键数据
5. **版本管理**：存储固件版本信息、升级标志等

## 10. 典型应用场景

### 10.1 设备配置管理
```c
// 设备配置结构体
typedef struct {
    uint32_t device_id;
    uint32_t network_addr;
    uint32_t calibration_data;
    uint32_t working_mode;
} DeviceConfigTypeDef;

// 保存设备配置
void SaveDeviceConfig(DeviceConfigTypeDef *config)
{
    PWR_BackupAccessCmd(ENABLE);
    BKP_WriteBackupRegister(BKP_DR1, config->device_id);
    BKP_WriteBackupRegister(BKP_DR2, config->network_addr);
    BKP_WriteBackupRegister(BKP_DR3, config->calibration_data);
    BKP_WriteBackupRegister(BKP_DR4, config->working_mode);
}
```

### 10.2 系统状态监控
```c
// 系统状态结构体
typedef struct {
    uint32_t run_time_seconds;
    uint32_t error_count;
    uint32_t reset_reason;
    uint32_t last_operation;
} SystemStatusTypeDef;

// 更新系统状态
void UpdateSystemStatus(SystemStatusTypeDef *status)
{
    PWR_BackupAccessCmd(ENABLE);
    BKP_WriteBackupRegister(BKP_DR5, status->run_time_seconds);
    BKP_WriteBackupRegister(BKP_DR6, status->error_count);
    BKP_WriteBackupRegister(BKP_DR7, status->reset_reason);
    BKP_WriteBackupRegister(BKP_DR8, status->last_operation);
}
```

### 10.3 安全检测系统
```c
// 入侵检测初始化
void SecuritySystem_Init(void)
{
    // 配置入侵检测为高电平触发
    BKP_TamperPinLevelConfig(BKP_TamperPinLevel_High);

    // 配置中断
    NVIC_InitStructure.NVIC_IRQChannel = TAMPER_IRQn;
    NVIC_InitStructure.NVIC_IRQChannelPreemptionPriority = 0; // 最高优先级
    NVIC_Init(&NVIC_InitStructure);

    // 使能入侵检测
    BKP_ITConfig(ENABLE);
    BKP_TamperPinCmd(ENABLE);
}

// 入侵检测处理
void TAMPER_IRQHandler(void)
{
    if(BKP_GetITStatus() == SET) {
        // 记录入侵事件
        LogSecurityEvent(SECURITY_EVENT_TAMPER);

        // 发送警报
        SendSecurityAlert();

        // 清除中断
        BKP_ClearITPendingBit();
    }
}
```

## 11. 总结

CH32V307的BKP外设提供了可靠的数据保持和物理安全检测功能。通过合理使用备份寄存器，可以在系统异常时保存关键数据，提高系统可靠性。入侵检测功能为安全敏感应用提供了物理层面的保护。开发时需要注意电源管理、引脚冲突和数据安全等关键环节，确保BKP功能正常工作。
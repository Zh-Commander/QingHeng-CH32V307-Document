# CH32V307 RTC外设使用指南

## 1. 外设概述

RTC（Real-Time Clock，实时时钟）是CH32V307的独立定时器，用于提供准确的日历和时间功能。主要特性包括：

- **独立供电**：可通过VBAT引脚单独供电，系统掉电时仍可运行
- **多种时钟源**：LSE（32.768kHz外部晶振）、LSI（~40kHz内部RC）、HSE/128
- **日历功能**：支持从1970年1月1日到2099年12月31日的日期范围
- **闹钟功能**：可设置闹钟时间，支持闹钟中断
- **唤醒功能**：可作为唤醒源从低功耗模式唤醒系统
- **校准功能**：支持软件校准，提高时钟精度
- **备份寄存器**：16个32位备份寄存器，用于保存用户数据

## 2. 关键代码片段

### 2.1 日历数据结构

```c
typedef struct
{
    vu8 hour;      // 小时 (0-23)
    vu8 min;       // 分钟 (0-59)
    vu8 sec;       // 秒 (0-59)

    vu16 w_year;   // 年 (1970-2099)
    vu8  w_month;  // 月 (1-12)
    vu8  w_date;   // 日 (1-31)
    vu8  week;     // 星期 (0-6, 0=周日)
} _calendar_obj;

_calendar_obj calendar;  // 全局日历对象
```

### 2.2 RTC初始化（RTC_Calendar示例）

```c
u8 RTC_Init(void)
{
    u8 temp = 0;

    // 1. 使能PWR和BKP时钟
    RCC_APB1PeriphClockCmd(RCC_APB1Periph_PWR | RCC_APB1Periph_BKP, ENABLE);
    PWR_BackupAccessCmd(ENABLE);

    // 2. 清除中断标志
    RTC_ClearITPendingBit(RTC_IT_ALR);
    RTC_ClearITPendingBit(RTC_IT_SEC);

    // 3. 复位备份寄存器
    BKP_DeInit();

    // 4. 配置LSE时钟源
    RCC_LSEConfig(RCC_LSE_ON);
    while(RCC_GetFlagStatus(RCC_FLAG_LSERDY) == RESET && temp < 250)
    {
        temp++;
        Delay_Ms(20);
    }
    if(temp >= 250) return 1; // 超时失败

    // 5. 配置RTC时钟源为LSE
    RCC_RTCCLKConfig(RCC_RTCCLKSource_LSE);
    RCC_RTCCLKCmd(ENABLE);

    // 6. 等待RTC操作完成
    RTC_WaitForLastTask();
    RTC_WaitForSynchro();

    // 7. 使能秒中断
    RTC_ITConfig(RTC_IT_SEC, ENABLE);
    RTC_WaitForLastTask();

    // 8. 进入配置模式设置预分频器
    RTC_EnterConfigMode();
    RTC_SetPrescaler(32767);  // LSE=32.768kHz, 32767+1=32768分频，得到1Hz时钟
    RTC_WaitForLastTask();

    // 9. 设置初始时间
    RTC_Set(2019, 10, 8, 13, 58, 55);
    RTC_ExitConfigMode();

    // 10. 写入备份寄存器标志
    BKP_WriteBackupRegister(BKP_DR1, 0XA1A1);

    // 11. 配置NVIC中断
    RTC_NVIC_Config();
    RTC_Get();

    return 0;
}
```

### 2.3 时间设置函数

```c
u8 RTC_Set(u16 syear, u8 smon, u8 sday, u8 hour, u8 min, u8 sec)
{
    u16 t;
    u32 seccount = 0;

    // 参数检查 (1970-2099)
    if(syear < 1970 || syear > 2099) return 1;

    // 计算从1970年1月1日到目标时间的总秒数
    for(t = 1970; t < syear; t++)
    {
        if(Is_Leap_Year(t))
            seccount += 31622400;  // 闰年秒数
        else
            seccount += 31536000;  // 平年秒数
    }

    smon -= 1;
    for(t = 0; t < smon; t++)
    {
        seccount += (u32)mon_table[t] * 86400;
        if(Is_Leap_Year(syear) && t == 1) // 闰年2月
            seccount += 86400;
    }

    seccount += (u32)(sday - 1) * 86400;
    seccount += (u32)hour * 3600;
    seccount += (u32)min * 60;
    seccount += sec;

    // 设置RTC计数器
    RCC_APB1PeriphClockCmd(RCC_APB1Periph_PWR | RCC_APB1Periph_BKP, ENABLE);
    PWR_BackupAccessCmd(ENABLE);
    RTC_SetCounter(seccount);
    RTC_WaitForLastTask();

    return 0;
}
```

### 2.4 时间读取函数

```c
u8 RTC_Get(void)
{
    static u16 daycnt = 0;
    u32 timecount = 0;
    u32 temp = 0;
    u16 temp1 = 0;

    // 获取RTC计数器值（从1970年1月1日开始的秒数）
    timecount = RTC_GetCounter();

    // 计算日期部分
    temp = timecount / 86400;  // 总天数
    if(daycnt != temp)  // 日期变化时才重新计算
    {
        daycnt = temp;
        temp1 = 1970;

        // 计算年份
        while(temp >= 365)
        {
            if(Is_Leap_Year(temp1))
            {
                if(temp >= 366) temp -= 366;
                else break;
            }
            else temp -= 365;
            temp1++;
        }
        calendar.w_year = temp1;

        // 计算月份
        temp1 = 0;
        while(temp >= 28)
        {
            if(Is_Leap_Year(calendar.w_year) && temp1 == 1)
            {
                if(temp >= 29) temp -= 29;
                else break;
            }
            else
            {
                if(temp >= mon_table[temp1]) temp -= mon_table[temp1];
                else break;
            }
            temp1++;
        }
        calendar.w_month = temp1 + 1;
        calendar.w_date = temp + 1;
    }

    // 计算时间部分
    temp = timecount % 86400;
    calendar.hour = temp / 3600;
    calendar.min = (temp % 3600) / 60;
    calendar.sec = (temp % 3600) % 60;

    // 计算星期
    calendar.week = RTC_Get_Week(calendar.w_year, calendar.w_month, calendar.w_date);

    return 0;
}
```

### 2.5 RTC中断处理

```c
void RTC_IRQHandler(void)
{
    if(RTC_GetITStatus(RTC_IT_SEC) != RESET) /* 秒中断 */
    {
        RTC_Get();  // 每秒更新日历数据
    }
    if(RTC_GetITStatus(RTC_IT_ALR) != RESET) /* 闹钟中断 */
    {
        RTC_ClearITPendingBit(RTC_IT_ALR);
        RTC_Get();
    }

    RTC_ClearITPendingBit(RTC_IT_SEC | RTC_IT_OW);
    RTC_WaitForLastTask();
}
```

### 2.6 RTC校准功能（RTC_Calibrations示例）

```c
void RTC_Calibration(uint16_t FastSecPer30days)
{
    float Deviation = 0.0;
    u8 CalibStep = 0;

    // PPM_PER_SEC = 10^6/(30d*24h*3600s) = 0.3858025
    // PPM_PER_STEP = 10^6/2^20 = 0.9536743
    Deviation = FastSecPer30days * PPM_PER_SEC;
    Deviation /= PPM_PER_STEP;

    CalibStep = (u8)Deviation;
    if (Deviation >= (CalibStep + 0.5))
        CalibStep += 1;
    if (CalibStep > 127)
        CalibStep = 127;

    // 设置校准值
    BKP_SetRTCCalibrationValue(CalibStep);
    printf("Calibration cab: %d\n", CalibStep);
}
```

### 2.7 闰年判断函数

```c
// 月份天数表（平年）
const u8 mon_table[12] = {31, 28, 31, 30, 31, 30, 31, 31, 30, 31, 30, 31};

// 判断是否为闰年
u8 Is_Leap_Year(u16 year)
{
    if((year % 400 == 0) || ((year % 4 == 0) && (year % 100 != 0)))
        return 1;
    else
        return 0;
}
```

### 2.8 星期计算函数

```c
// 根据年月日计算星期（0=周日，1=周一，...，6=周六）
u8 RTC_Get_Week(u16 year, u8 month, u8 day)
{
    u16 temp2;
    u8 yearH, yearL;

    yearH = year / 100;
    yearL = year % 100;
    // 如果月份是1月或2月，则看作上一年的13月或14月
    if(month == 1 || month == 2)
    {
        month += 12;
        yearL--;
        if(yearL < 0)
        {
            yearL = 99;
            yearH--;
        }
    }

    // 蔡勒公式
    temp2 = (u16)(yearL + yearL / 4 + yearH / 4 - 2 * yearH + 13 * (month + 1) / 5 + day - 1);

    // 处理负数情况
    while(temp2 < 7) temp2 += 7;
    temp2 %= 7;

    return temp2;
}
```

## 3. 配置数据结构

### 3.1 预分频器计算
- **LSE (32.768kHz)**: 预分频值 = 32768 - 1 = 32767 → 1Hz时钟
- **LSI (~40kHz)**: 预分频值 = 40000 - 1 = 39999 → ~1Hz时钟
- **HSE/128**: 预分频值根据具体HSE频率计算
- **公式**: `RTC时钟频率 = 输入频率 / (预分频值 + 1)`

### 3.2 时间范围
- **起始时间**: 1970年1月1日 00:00:00
- **结束时间**: 2099年12月31日 23:59:59
- **最大计数值**: 32位秒计数器，约136年

## 4. 通用配置步骤

### 4.1 基本配置流程
1. **使能相关时钟**
   ```c
   RCC_APB1PeriphClockCmd(RCC_APB1Periph_PWR | RCC_APB1Periph_BKP, ENABLE);
   ```

2. **使能备份寄存器访问**
   ```c
   PWR_BackupAccessCmd(ENABLE);
   ```

3. **配置RTC时钟源**（三选一）
   - LSE（外部低速晶振，32.768kHz，最精确）
   - LSI（内部低速RC振荡器，~40kHz，精度较低）
   - HSE/128（外部高速时钟分频）

4. **初始化RTC**
   ```c
   RCC_RTCCLKConfig(RCC_RTCCLKSource_LSE);
   RCC_RTCCLKCmd(ENABLE);
   RTC_WaitForLastTask();
   RTC_WaitForSynchro();
   ```

5. **配置预分频器**
   ```c
   RTC_EnterConfigMode();
   RTC_SetPrescaler(32767);  // 对于32.768kHz LSE，得到1Hz时钟
   RTC_ExitConfigMode();
   ```

6. **设置初始时间**
   ```c
   RTC_SetCounter(seconds_since_1970);
   ```

7. **配置中断（可选）**
   ```c
   RTC_ITConfig(RTC_IT_SEC | RTC_IT_ALR, ENABLE);
   NVIC_Configuration();
   ```

### 4.2 闹钟配置步骤
1. 计算闹钟时间的秒数（从1970年1月1日开始）
2. 设置闹钟值
   ```c
   RTC_SetAlarm(seccount);
   ```
3. 使能闹钟中断
   ```c
   RTC_ITConfig(RTC_IT_ALR, ENABLE);
   ```

### 4.3 校准配置步骤
1. 测量RTC实际频率
2. 计算校准值
3. 设置校准值
   ```c
   BKP_SetRTCCalibrationValue(CalibStep);
   ```

## 5. 示例工程详解

### 5.1 RTC_Calendar示例
- **位置**：`RTC/RTC_Calendar/User/main.c`
- **功能**：基本的日历和时间显示
- **特点**：
  - 使用标准的32767预分频值
  - 每秒通过串口输出时间
  - 包含完整的日期计算（闰年、星期等）
- **适用场景**：需要基本时间显示和记录的应用

### 5.2 RTC_Calibrations示例
- **位置**：`RTC/RTC_Calibrations/User/main.c`
- **功能**：RTC时钟精度校准
- **特点**：
  - 使用TIM1定时器测量RTC精度
  - 实现自动校准算法
  - 使用32766预分频值（故意偏快）
  - 支持ppm级精度校准
- **适用场景**：对时间精度要求高的应用（如数据记录仪、计时设备）

### 5.3 关键差异对比

| 特性 | RTC_Calendar | RTC_Calibrations |
|------|-------------|------------------|
| 预分频值 | 32767 | 32766（故意偏快） |
| 中断处理 | 简单时间更新 | 复杂校准逻辑 |
| 额外外设 | 无 | TIM1定时器 |
| 输出信息 | 时间日期 | 校准参数和结果 |
| 适用精度 | 一般精度 | 高精度需求 |

## 6. 常见问题

### Q1: RTC时钟源如何选择？
A: 根据应用需求选择：
- **LSE**：最精确，需要外部32.768kHz晶振
- **LSI**：精度较低但无需外部元件
- **HSE/128**：精度取决于外部高速晶振

### Q2: RTC初始化失败？
A: 检查以下配置：
1. PWR和BKP时钟是否使能
2. 备份寄存器访问是否使能
3. LSE时钟是否就绪（等待RCC_FLAG_LSERDY）
4. RTC操作是否等待完成（RTC_WaitForLastTask）

### Q3: 时间设置不准确？
A: 可能原因：
1. 预分频器配置错误
2. 时钟源不稳定
3. 需要进行校准

### Q4: 闹钟功能不工作？
A: 检查以下配置：
1. 闹钟时间计算是否正确
2. 闹钟中断是否使能
3. NVIC中断优先级配置
4. 中断标志是否清除

### Q5: 备份寄存器数据丢失？
A: 确保：
1. 系统复位期间VBAT引脚供电正常
2. 正确使能备份寄存器访问
3. 数据写入后验证读取

### Q6: 低功耗模式下RTC不工作？
A: 检查：
1. RTC时钟源在低功耗模式下是否仍工作
2. VBAT供电是否正常
3. 唤醒源配置是否正确

## 7. API参考

| 函数名 | 功能描述 | 参数说明 |
|--------|----------|----------|
| `RTC_Init` | RTC初始化 | 无 |
| `RTC_SetCounter` | 设置RTC计数器 | Counter：计数器值（从1970年1月1日开始的秒数） |
| `RTC_GetCounter` | 获取RTC计数器 | 返回值：计数器值 |
| `RTC_SetPrescaler` | 设置预分频器 | PrescalerValue：预分频值 |
| `RTC_SetAlarm` | 设置闹钟 | AlarmValue：闹钟值（秒数） |
| `RTC_ITConfig` | 配置中断 | RTC_IT：中断源，NewState：新状态 |
| `RTC_EnterConfigMode` | 进入配置模式 | 无 |
| `RTC_ExitConfigMode` | 退出配置模式 | 无 |
| `RTC_WaitForLastTask` | 等待操作完成 | 无 |
| `RTC_WaitForSynchro` | 等待同步 | 无 |
| `RTC_GetITStatus` | 获取中断状态 | RTC_IT：中断源 |
| `RTC_ClearITPendingBit` | 清除中断标志 | RTC_IT：中断源 |
| `BKP_SetRTCCalibrationValue` | 设置RTC校准值 | CalibrationValue：校准值（0-127） |
| `BKP_WriteBackupRegister` | 写入备份寄存器 | BKP_DRx：寄存器编号，Data：数据 |
| `BKP_ReadBackupRegister` | 读取备份寄存器 | BKP_DRx：寄存器编号 |

## 8. 注意事项

1. **备份寄存器访问**：必须先使能PWR时钟和备份寄存器访问
2. **时钟源稳定性**：LSE需要稳定的外部晶振，注意晶振负载电容匹配
3. **中断优先级**：合理设置RTC中断优先级，避免影响其他关键任务
4. **低功耗模式**：RTC在待机模式下仍可运行，需确保VBAT供电
5. **时间范围限制**：支持1970年1月1日到2099年12月31日
6. **校准精度**：校准值范围0-127，每步对应约0.954ppm

## 9. 应用建议

1. **普通应用**：使用RTC_Calendar示例，满足基本时间需求
2. **高精度应用**：使用RTC_Calibrations示例，进行精度校准
3. **低功耗应用**：结合PWR外设使用，实现系统唤醒
4. **数据记录应用**：利用备份寄存器保存关键数据
5. **定时任务应用**：使用闹钟功能实现定时执行任务

## 10. 总结

CH32V307的RTC外设功能完善，支持日历、闹钟、校准等多种功能。通过合理配置RTC参数，可以实现准确的时间管理和低功耗唤醒。开发时需要注意时钟源选择、备份寄存器访问、中断处理等关键环节。对于精度要求高的应用，建议使用校准功能提高时间精度。
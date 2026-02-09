# CH32V307 TOUCHKEY触摸按键使用指南

## 1. 外设概述

TOUCHKEY（触摸按键）是CH32V307内置的电容式触摸按键检测模块，通过ADC检测电容变化实现触摸检测。主要特性包括：

- **电容检测原理**：基于RC充放电时间测量电容变化
- **多通道支持**：支持多个触摸按键通道
- **内置缓冲器**：集成信号缓冲器提高检测稳定性
- **自动校准**：支持基线自动校准
- **抗干扰设计**：硬件滤波减少环境干扰
- **低功耗模式**：支持低功耗检测模式
- **灵活配置**：可调充电/放电时间适应不同硬件设计

## 2. 关键代码片段

### 2.1 TOUCHKEY基本使用示例

```c
#include "ch32v30x.h"
#include "debug.h"

// 触摸按键初始化函数
void Touch_Key_Init(void)
{
    GPIO_InitTypeDef GPIO_InitStructure = {0};
    ADC_InitTypeDef  ADC_InitStructure = {0};

    // 1. 使能GPIOA和ADC1时钟
    RCC_APB2PeriphClockCmd(RCC_APB2Periph_GPIOA, ENABLE);
    RCC_APB2PeriphClockCmd(RCC_APB2Periph_ADC1, ENABLE);
    RCC_ADCCLKConfig(RCC_PCLK2_Div8);  // ADC时钟 = PCLK2/8

    // 2. 配置PA2为模拟输入（触摸按键通道2）
    GPIO_InitStructure.GPIO_Pin = GPIO_Pin_2;
    GPIO_InitStructure.GPIO_Mode = GPIO_Mode_AIN;  // 模拟输入模式
    GPIO_Init(GPIOA, &GPIO_InitStructure);

    // 3. 配置ADC参数
    ADC_InitStructure.ADC_Mode = ADC_Mode_Independent;      // 独立模式
    ADC_InitStructure.ADC_ScanConvMode = DISABLE;           // 单次转换
    ADC_InitStructure.ADC_ContinuousConvMode = DISABLE;     // 单次转换模式
    ADC_InitStructure.ADC_ExternalTrigConv = ADC_ExternalTrigConv_None;  // 软件触发
    ADC_InitStructure.ADC_DataAlign = ADC_DataAlign_Right;  // 数据右对齐
    ADC_InitStructure.ADC_NbrOfChannel = 1;                 // 1个通道
    ADC_Init(ADC1, &ADC_InitStructure);

    // 4. 使能ADC
    ADC_Cmd(ADC1, ENABLE);

    // 5. 使能触摸按键和缓冲器
    // CTLR1[26] = 1: 使能触摸按键功能
    // CTLR1[24] = 1: 使能缓冲器
    TKey1->CTLR1 |= (1 << 26) | (1 << 24);
}

// 触摸按键ADC读取函数
u16 Touch_Key_Adc(u8 ch)
{
    // 1. 配置ADC通道
    ADC_RegularChannelConfig(ADC1, ch, 1, ADC_SampleTime_7Cycles5);

    // 2. 设置充电时间（影响灵敏度）
    TKey1->IDATAR1 = 0x10;  // 充电时间 = 16个ADC时钟周期

    // 3. 设置放电时间（影响检测范围）
    TKey1->RDATAR = 0x8;    // 放电时间 = 8个ADC时钟周期

    // 4. 等待转换完成
    while(!ADC_GetFlagStatus(ADC1, ADC_FLAG_EOC))
        ;

    // 5. 返回ADC值
    return (uint16_t)TKey1->RDATAR;
}

int main(void)
{
    u16 ADC_val;

    // 系统初始化
    SystemCoreClockUpdate();
    Delay_Init();
    USART_Printf_Init(115200);
    printf("SystemClk:%d\r\n", SystemCoreClock);
    printf("ChipID:%08x\r\n", DBGMCU_GetCHIPID());

    // 触摸按键初始化
    Touch_Key_Init();

    while(1)
    {
        // 读取触摸按键值（通道2）
        ADC_val = Touch_Key_Adc(ADC_Channel_2);
        printf("TouckKey Value:%d\r\n", ADC_val);

        // 延时500ms
        Delay_Ms(500);
    }
}
```

### 2.2 多通道触摸按键检测

```c
// 多通道触摸按键配置
#define TOUCH_KEY_COUNT  4  // 4个触摸按键

// 触摸按键通道定义
typedef enum {
    TOUCH_KEY_CH1 = ADC_Channel_0,  // PA0
    TOUCH_KEY_CH2 = ADC_Channel_1,  // PA1
    TOUCH_KEY_CH3 = ADC_Channel_2,  // PA2
    TOUCH_KEY_CH4 = ADC_Channel_3,  // PA3
} TouchKey_Channel;

// 触摸按键数据结构
typedef struct {
    TouchKey_Channel channel;
    uint16_t baseline;      // 基线值（无触摸时的值）
    uint16_t threshold;     // 触发阈值
    uint16_t current_value; // 当前值
    uint8_t state;          // 当前状态
    uint8_t debounce_count; // 消抖计数
} TouchKey_Data;

// 全局触摸按键数组
TouchKey_Data touch_keys[TOUCH_KEY_COUNT];

// 多通道触摸按键初始化
void MultiTouchKey_Init(void)
{
    GPIO_InitTypeDef GPIO_InitStructure = {0};
    ADC_InitTypeDef ADC_InitStructure = {0};
    int i;

    // 1. 使能时钟
    RCC_APB2PeriphClockCmd(RCC_APB2Periph_GPIOA, ENABLE);
    RCC_APB2PeriphClockCmd(RCC_APB2Periph_ADC1, ENABLE);
    RCC_ADCCLKConfig(RCC_PCLK2_Div8);

    // 2. 配置所有触摸按键引脚为模拟输入
    GPIO_InitStructure.GPIO_Pin = GPIO_Pin_0 | GPIO_Pin_1 | GPIO_Pin_2 | GPIO_Pin_3;
    GPIO_InitStructure.GPIO_Mode = GPIO_Mode_AIN;
    GPIO_Init(GPIOA, &GPIO_InitStructure);

    // 3. 配置ADC
    ADC_InitStructure.ADC_Mode = ADC_Mode_Independent;
    ADC_InitStructure.ADC_ScanConvMode = ENABLE;           // 扫描模式
    ADC_InitStructure.ADC_ContinuousConvMode = ENABLE;     // 连续转换
    ADC_InitStructure.ADC_ExternalTrigConv = ADC_ExternalTrigConv_None;
    ADC_InitStructure.ADC_DataAlign = ADC_DataAlign_Right;
    ADC_InitStructure.ADC_NbrOfChannel = TOUCH_KEY_COUNT;  // 4个通道
    ADC_Init(ADC1, &ADC_InitStructure);

    // 4. 配置扫描序列
    ADC_RegularChannelConfig(ADC1, TOUCH_KEY_CH1, 1, ADC_SampleTime_7Cycles5);
    ADC_RegularChannelConfig(ADC1, TOUCH_KEY_CH2, 2, ADC_SampleTime_7Cycles5);
    ADC_RegularChannelConfig(ADC1, TOUCH_KEY_CH3, 3, ADC_SampleTime_7Cycles5);
    ADC_RegularChannelConfig(ADC1, TOUCH_KEY_CH4, 4, ADC_SampleTime_7Cycles5);

    // 5. 使能ADC和触摸按键
    ADC_Cmd(ADC1, ENABLE);
    TKey1->CTLR1 |= (1 << 26) | (1 << 24);

    // 6. 初始化触摸按键数据结构
    for(i = 0; i < TOUCH_KEY_COUNT; i++) {
        touch_keys[i].channel = (TouchKey_Channel)(ADC_Channel_0 + i);
        touch_keys[i].baseline = 0;
        touch_keys[i].threshold = 50;  // 默认阈值
        touch_keys[i].current_value = 0;
        touch_keys[i].state = 0;
        touch_keys[i].debounce_count = 0;
    }

    // 7. 校准基线（采集无触摸时的值）
    Calibrate_TouchKeys();
}

// 读取所有触摸按键
void Read_All_TouchKeys(void)
{
    uint16_t adc_value;
    int i;

    for(i = 0; i < TOUCH_KEY_COUNT; i++) {
        // 设置充电和放电时间
        TKey1->IDATAR1 = 0x10;
        TKey1->RDATAR = 0x8;

        // 配置当前通道
        ADC_RegularChannelConfig(ADC1, touch_keys[i].channel, 1, ADC_SampleTime_7Cycles5);

        // 等待转换完成
        while(!ADC_GetFlagStatus(ADC1, ADC_FLAG_EOC))
            ;

        // 读取ADC值
        adc_value = (uint16_t)TKey1->RDATAR;
        touch_keys[i].current_value = adc_value;

        // 检测触摸状态
        Detect_Touch_State(&touch_keys[i]);
    }
}

// 触摸状态检测
void Detect_Touch_State(TouchKey_Data *key)
{
    int16_t delta = key->baseline - key->current_value;

    if(delta > key->threshold) {
        // 检测到触摸
        key->debounce_count++;
        if(key->debounce_count > 3) {  // 消抖确认
            key->state = 1;
            key->debounce_count = 0;
        }
    } else {
        // 无触摸
        key->debounce_count = 0;
        key->state = 0;
    }
}
```

### 2.3 触摸按键自动校准

```c
// 自动校准功能
#define CALIBRATION_SAMPLES  100  // 校准采样次数
#define CALIBRATION_MARGIN   20   // 校准余量

// 自动校准函数
void Auto_Calibrate_TouchKeys(void)
{
    uint32_t sum[TOUCH_KEY_COUNT] = {0};
    uint16_t sample;
    int i, j;

    printf("Starting calibration...\r\n");
    printf("Please do not touch keys for 3 seconds\r\n");

    // 延时3秒，等待用户释放按键
    Delay_Ms(3000);

    // 采集校准样本
    for(j = 0; j < CALIBRATION_SAMPLES; j++) {
        for(i = 0; i < TOUCH_KEY_COUNT; i++) {
            // 读取当前通道
            TKey1->IDATAR1 = 0x10;
            TKey1->RDATAR = 0x8;
            ADC_RegularChannelConfig(ADC1, touch_keys[i].channel, 1, ADC_SampleTime_7Cycles5);
            while(!ADC_GetFlagStatus(ADC1, ADC_FLAG_EOC))
                ;

            sample = (uint16_t)TKey1->RDATAR;
            sum[i] += sample;
        }
        Delay_Ms(10);  // 采样间隔
    }

    // 计算基线值
    for(i = 0; i < TOUCH_KEY_COUNT; i++) {
        touch_keys[i].baseline = sum[i] / CALIBRATION_SAMPLES;
        touch_keys[i].threshold = touch_keys[i].baseline * 0.1;  // 阈值为基线值的10%

        // 确保阈值不小于最小值
        if(touch_keys[i].threshold < 20) {
            touch_keys[i].threshold = 20;
        }

        printf("Key %d: Baseline=%d, Threshold=%d\r\n",
               i, touch_keys[i].baseline, touch_keys[i].threshold);
    }

    printf("Calibration completed\r\n");
}

// 动态基线跟踪
void Dynamic_Baseline_Tracking(void)
{
    static uint32_t baseline_update_timer = 0;
    uint32_t current_time = GetTickCount();
    int i;

    // 每5秒更新一次基线
    if(current_time - baseline_update_timer > 5000) {
        baseline_update_timer = current_time;

        for(i = 0; i < TOUCH_KEY_COUNT; i++) {
            // 只有在无触摸时更新基线
            if(touch_keys[i].state == 0) {
                // 缓慢调整基线（低通滤波）
                touch_keys[i].baseline = touch_keys[i].baseline * 0.9 +
                                        touch_keys[i].current_value * 0.1;

                // 重新计算阈值
                touch_keys[i].threshold = touch_keys[i].baseline * 0.1;

                // 限制阈值范围
                if(touch_keys[i].threshold < 20) touch_keys[i].threshold = 20;
                if(touch_keys[i].threshold > 100) touch_keys[i].threshold = 100;
            }
        }
    }
}
```

### 2.4 触摸按键高级功能

```c
// 触摸按键配置结构
typedef struct {
    uint8_t charge_time;     // 充电时间
    uint8_t discharge_time;  // 放电时间
    uint8_t sample_time;     // 采样时间
    uint8_t filter_strength; // 滤波强度
} TouchKey_Config;

// 触摸按键事件回调
typedef void (*TouchKey_Callback)(uint8_t key_index, uint8_t event);

// 触摸按键事件定义
typedef enum {
    TOUCH_EVENT_PRESS,      // 按下
    TOUCH_EVENT_RELEASE,    // 释放
    TOUCH_EVENT_LONG_PRESS, // 长按
    TOUCH_EVENT_DOUBLE_TAP  // 双击
} TouchKey_Event;

// 高级触摸按键管理器
typedef struct {
    TouchKey_Data keys[TOUCH_KEY_COUNT];
    TouchKey_Config config;
    TouchKey_Callback callback;
    uint32_t long_press_timeout;  // 长按超时时间（ms）
    uint32_t double_tap_timeout;  // 双击超时时间（ms）
} TouchKey_Manager;

TouchKey_Manager touch_manager;

// 初始化高级触摸按键管理器
void TouchKey_Manager_Init(TouchKey_Config *config, TouchKey_Callback callback)
{
    int i;

    // 保存配置
    touch_manager.config = *config;
    touch_manager.callback = callback;
    touch_manager.long_press_timeout = 1000;  // 默认1秒长按
    touch_manager.double_tap_timeout = 300;   // 默认300ms双击

    // 初始化硬件
    GPIO_InitTypeDef GPIO_InitStructure = {0};
    ADC_InitTypeDef ADC_InitStructure = {0};

    RCC_APB2PeriphClockCmd(RCC_APB2Periph_GPIOA, ENABLE);
    RCC_APB2PeriphClockCmd(RCC_APB2Periph_ADC1, ENABLE);
    RCC_ADCCLKConfig(RCC_PCLK2_Div8);

    GPIO_InitStructure.GPIO_Pin = GPIO_Pin_0 | GPIO_Pin_1 | GPIO_Pin_2 | GPIO_Pin_3;
    GPIO_InitStructure.GPIO_Mode = GPIO_Mode_AIN;
    GPIO_Init(GPIOA, &GPIO_InitStructure);

    ADC_InitStructure.ADC_Mode = ADC_Mode_Independent;
    ADC_InitStructure.ADC_ScanConvMode = ENABLE;
    ADC_InitStructure.ADC_ContinuousConvMode = ENABLE;
    ADC_InitStructure.ADC_ExternalTrigConv = ADC_ExternalTrigConv_None;
    ADC_InitStructure.ADC_DataAlign = ADC_DataAlign_Right;
    ADC_InitStructure.ADC_NbrOfChannel = TOUCH_KEY_COUNT;
    ADC_Init(ADC1, &ADC_InitStructure);

    // 配置扫描序列
    ADC_RegularChannelConfig(ADC1, ADC_Channel_0, 1, config->sample_time);
    ADC_RegularChannelConfig(ADC1, ADC_Channel_1, 2, config->sample_time);
    ADC_RegularChannelConfig(ADC1, ADC_Channel_2, 3, config->sample_time);
    ADC_RegularChannelConfig(ADC1, ADC_Channel_3, 4, config->sample_time);

    ADC_Cmd(ADC1, ENABLE);
    TKey1->CTLR1 |= (1 << 26) | (1 << 24);

    // 初始化按键数据
    for(i = 0; i < TOUCH_KEY_COUNT; i++) {
        touch_manager.keys[i].channel = (TouchKey_Channel)(ADC_Channel_0 + i);
        touch_manager.keys[i].baseline = 0;
        touch_manager.keys[i].threshold = 50;
        touch_manager.keys[i].current_value = 0;
        touch_manager.keys[i].state = 0;
        touch_manager.keys[i].press_start_time = 0;
        touch_manager.keys[i].last_release_time = 0;
        touch_manager.keys[i].tap_count = 0;
    }

    // 自动校准
    Auto_Calibrate_TouchKeys();
}

// 触摸按键处理任务
void TouchKey_Processing_Task(void)
{
    static uint32_t last_process_time = 0;
    uint32_t current_time = GetTickCount();
    int i;

    // 每10ms处理一次
    if(current_time - last_process_time < 10) {
        return;
    }
    last_process_time = current_time;

    // 读取所有按键
    Read_All_TouchKeys();

    // 处理每个按键
    for(i = 0; i < TOUCH_KEY_COUNT; i++) {
        Process_TouchKey_Events(&touch_manager.keys[i], i, current_time);
    }

    // 动态基线跟踪
    Dynamic_Baseline_Tracking();
}

// 处理触摸按键事件
void Process_TouchKey_Events(TouchKey_Data *key, uint8_t index, uint32_t current_time)
{
    int16_t delta = key->baseline - key->current_value;
    uint8_t is_touched = (delta > key->threshold) ? 1 : 0;

    // 状态机处理
    switch(key->state) {
        case 0:  // 空闲状态
            if(is_touched) {
                key->state = 1;  // 进入按下状态
                key->press_start_time = current_time;
                key->debounce_count = 1;

                // 检查双击
                if(current_time - key->last_release_time < touch_manager.double_tap_timeout) {
                    key->tap_count++;
                    if(key->tap_count >= 2) {
                        // 触发双击事件
                        if(touch_manager.callback) {
                            touch_manager.callback(index, TOUCH_EVENT_DOUBLE_TAP);
                        }
                        key->tap_count = 0;
                    }
                } else {
                    key->tap_count = 1;
                }
            }
            break;

        case 1:  // 按下状态
            if(is_touched) {
                key->debounce_count++;
                if(key->debounce_count > 3) {
                    // 确认按下

                    // 检查长按
                    if(current_time - key->press_start_time > touch_manager.long_press_timeout) {
                        key->state = 2;  // 进入长按状态
                        // 触发长按事件
                        if(touch_manager.callback) {
                            touch_manager.callback(index, TOUCH_EVENT_LONG_PRESS);
                        }
                    }
                }
            } else {
                key->debounce_count = 0;
                key->state = 0;  // 返回空闲状态

                // 触发释放事件
                if(touch_manager.callback) {
                    touch_manager.callback(index, TOUCH_EVENT_RELEASE);
                }
                key->last_release_time = current_time;
            }
            break;

        case 2:  // 长按状态
            if(!is_touched) {
                key->state = 0;  // 返回空闲状态
                // 触发释放事件
                if(touch_manager.callback) {
                    touch_manager.callback(index, TOUCH_EVENT_RELEASE);
                }
                key->last_release_time = current_time;
                key->tap_count = 0;
            }
            break;
    }
}
```

### 2.5 触摸按键滤波器实现

```c
// 数字滤波器实现
typedef struct {
    uint16_t buffer[8];  // 滤波器缓冲区
    uint8_t index;       // 当前索引
    uint16_t sum;        // 当前和
} MovingAverage_Filter;

// 初始化移动平均滤波器
void Filter_Init(MovingAverage_Filter *filter)
{
    int i;
    for(i = 0; i < 8; i++) {
        filter->buffer[i] = 0;
    }
    filter->index = 0;
    filter->sum = 0;
}

// 应用移动平均滤波器
uint16_t Filter_Apply(MovingAverage_Filter *filter, uint16_t new_sample)
{
    // 减去最旧的值
    filter->sum -= filter->buffer[filter->index];

    // 添加新值
    filter->buffer[filter->index] = new_sample;
    filter->sum += new_sample;

    // 更新索引
    filter->index = (filter->index + 1) & 0x07;  // 循环缓冲区

    // 返回平均值
    return filter->sum >> 3;  // 除以8
}

// 中值滤波器
uint16_t Median_Filter(uint16_t *samples, uint8_t count)
{
    uint16_t temp;
    uint8_t i, j;

    // 冒泡排序
    for(i = 0; i < count - 1; i++) {
        for(j = i + 1; j < count; j++) {
            if(samples[i] > samples[j]) {
                temp = samples[i];
                samples[i] = samples[j];
                samples[j] = temp;
            }
        }
    }

    // 返回中值
    return samples[count / 2];
}

// 触摸按键带滤波的读取
uint16_t Read_TouchKey_With_Filter(uint8_t channel, MovingAverage_Filter *filter)
{
    uint16_t raw_value;

    // 设置充电和放电时间
    TKey1->IDATAR1 = 0x10;
    TKey1->RDATAR = 0x8;

    // 配置通道
    ADC_RegularChannelConfig(ADC1, channel, 1, ADC_SampleTime_7Cycles5);

    // 等待转换完成
    while(!ADC_GetFlagStatus(ADC1, ADC_FLAG_EOC))
        ;

    // 读取原始值
    raw_value = (uint16_t)TKey1->RDATAR;

    // 应用滤波器
    return Filter_Apply(filter, raw_value);
}
```

## 3. 配置数据结构

### 3.1 TKey寄存器定义

```c
// TKey寄存器结构（ch32v30x.h）
typedef struct
{
  __IO uint32_t CTLR1;     // 控制寄存器1
  __IO uint32_t CTLR2;     // 控制寄存器2
  __IO uint32_t SMPR1;     // 采样时间寄存器1
  __IO uint32_t SMPR2;     // 采样时间寄存器2
  __IO uint32_t IOFR1;     // 注入通道数据偏移寄存器1
  __IO uint32_t IOFR2;     // 注入通道数据偏移寄存器2
  __IO uint32_t IOFR3;     // 注入通道数据偏移寄存器3
  __IO uint32_t IOFR4;     // 注入通道数据偏移寄存器4
  __IO uint32_t WDHTR;     // 看门狗高阈值寄存器
  __IO uint32_t WDLTR;     // 看门狗低阈值寄存器
  __IO uint32_t RSQR1;     // 规则序列寄存器1
  __IO uint32_t RSQR2;     // 规则序列寄存器2
  __IO uint32_t RSQR3;     // 规则序列寄存器3
  __IO uint32_t ISQR;      // 注入序列寄存器
  __IO uint32_t IDATAR1;   // 注入数据寄存器1（用于充电时间）
  __IO uint32_t IDATAR2;   // 注入数据寄存器2
  __IO uint32_t IDATAR3;   // 注入数据寄存器3
  __IO uint32_t IDATAR4;   // 注入数据寄存器4
  __IO uint32_t RDATAR;    // 规则数据寄存器（用于放电时间和ADC值）
  __IO uint32_t ISTATR;    // 状态寄存器
  __IO uint32_t CCTLR;     // 通用控制寄存器
} TKey_TypeDef;

#define TKey1            ((TKey_TypeDef *)ADC1_BASE)
```

### 3.2 TKey控制寄存器位定义

```c
// CTLR1控制寄存器位定义
#define TKEY_CTLR1_TKEN         (1 << 26)  // 触摸按键使能
#define TKEY_CTLR1_BUFEN        (1 << 24)  // 缓冲器使能
#define TKEY_CTLR1_ADON         (1 << 0)   // ADC使能

// 触摸按键相关配置位
#define TKEY_MODE_ENABLE        (TKEY_CTLR1_TKEN | TKEY_CTLR1_BUFEN)
#define TKEY_MODE_DISABLE       0

// IDATAR1寄存器（充电时间配置）
// 位[15:0]: 充电时间计数值
// 值越大，充电时间越长，灵敏度越高

// RDATAR寄存器（放电时间和ADC值）
// 位[15:0]: 放电时间计数值/ADC转换结果
```

### 3.3 ADC通道定义

```c
// ADC通道定义（ch32v30x.h）
#define ADC_Channel_0            ((uint8_t)0x00)  // PA0
#define ADC_Channel_1            ((uint8_t)0x01)  // PA1
#define ADC_Channel_2            ((uint8_t)0x02)  // PA2
#define ADC_Channel_3            ((uint8_t)0x03)  // PA3
#define ADC_Channel_4            ((uint8_t)0x04)  // PA4
#define ADC_Channel_5            ((uint8_t)0x05)  // PA5
#define ADC_Channel_6            ((uint8_t)0x06)  // PA6
#define ADC_Channel_7            ((uint8_t)0x07)  // PA7
#define ADC_Channel_8            ((uint8_t)0x08)  // PB0
#define ADC_Channel_9            ((uint8_t)0x09)  // PB1
#define ADC_Channel_10           ((uint8_t)0x0A)  // PC0
#define ADC_Channel_11           ((uint8_t)0x0B)  // PC1
#define ADC_Channel_12           ((uint8_t)0x0C)  // PC2
#define ADC_Channel_13           ((uint8_t)0x0D)  // PC3
#define ADC_Channel_14           ((uint8_t)0x0E)  // PC4
#define ADC_Channel_15           ((uint8_t)0x0F)  // PC5
```

### 3.4 采样时间定义

```c
// ADC采样时间定义
#define ADC_SampleTime_1Cycles5   ((uint8_t)0x00)  // 1.5个ADC时钟周期
#define ADC_SampleTime_7Cycles5   ((uint8_t)0x01)  // 7.5个ADC时钟周期
#define ADC_SampleTime_13Cycles5  ((uint8_t)0x02)  // 13.5个ADC时钟周期
#define ADC_SampleTime_28Cycles5  ((uint8_t)0x03)  // 28.5个ADC时钟周期
#define ADC_SampleTime_41Cycles5  ((uint8_t)0x04)  // 41.5个ADC时钟周期
#define ADC_SampleTime_55Cycles5  ((uint8_t)0x05)  // 55.5个ADC时钟周期
#define ADC_SampleTime_71Cycles5  ((uint8_t)0x06)  // 71.5个ADC时钟周期
#define ADC_SampleTime_239Cycles5 ((uint8_t)0x07)  // 239.5个ADC时钟周期
```

### 3.5 触摸按键配置结构

```c
// 完整的触摸按键配置结构
typedef struct {
    // 硬件配置
    uint8_t adc_channel;           // ADC通道
    uint8_t gpio_pin;              // GPIO引脚
    GPIO_TypeDef* gpio_port;       // GPIO端口

    // 时间参数
    uint16_t charge_time;          // 充电时间（IDATAR1值）
    uint16_t discharge_time;       // 放电时间（RDATAR值）
    uint8_t sample_time;           // 采样时间

    // 检测参数
    uint16_t baseline;             // 基线值
    uint16_t threshold;            // 触发阈值
    uint16_t hysteresis;           // 迟滞值
    uint8_t debounce_count;        // 消抖计数

    // 滤波器配置
    uint8_t filter_type;           // 滤波器类型
    uint8_t filter_size;           // 滤波器大小
    uint16_t filter_coeff;         // 滤波器系数

    // 高级功能
    uint32_t long_press_timeout;   // 长按超时时间（ms）
    uint32_t double_tap_timeout;   // 双击超时时间（ms）
    uint8_t enable_auto_calib;     // 自动校准使能
    uint8_t enable_dynamic_baseline; // 动态基线使能
} TouchKey_Config_t;
```

## 4. 通用配置步骤

### 4.1 单通道触摸按键配置步骤

1. **硬件连接**：
   - 连接触摸电极到PA2（通道2）
   - 添加适当的对地电容（典型值10-47pF）
   - 确保电极与周围导体有足够的间距

2. **软件配置**：
   ```c
   void TouchKey_Single_Channel_Setup(void)
   {
       // 1. 使能时钟
       RCC_APB2PeriphClockCmd(RCC_APB2Periph_GPIOA, ENABLE);
       RCC_APB2PeriphClockCmd(RCC_APB2Periph_ADC1, ENABLE);
       RCC_ADCCLKConfig(RCC_PCLK2_Div8);

       // 2. 配置GPIO为模拟输入
       GPIO_InitTypeDef GPIO_InitStructure = {0};
       GPIO_InitStructure.GPIO_Pin = GPIO_Pin_2;
       GPIO_InitStructure.GPIO_Mode = GPIO_Mode_AIN;
       GPIO_Init(GPIOA, &GPIO_InitStructure);

       // 3. 配置ADC
       ADC_InitTypeDef ADC_InitStructure = {0};
       ADC_InitStructure.ADC_Mode = ADC_Mode_Independent;
       ADC_InitStructure.ADC_ScanConvMode = DISABLE;
       ADC_InitStructure.ADC_ContinuousConvMode = DISABLE;
       ADC_InitStructure.ADC_ExternalTrigConv = ADC_ExternalTrigConv_None;
       ADC_InitStructure.ADC_DataAlign = ADC_DataAlign_Right;
       ADC_InitStructure.ADC_NbrOfChannel = 1;
       ADC_Init(ADC1, &ADC_InitStructure);

       // 4. 使能ADC
       ADC_Cmd(ADC1, ENABLE);

       // 5. 使能触摸按键功能
       TKey1->CTLR1 |= (1 << 26) | (1 << 24);

       printf("Touch Key (PA2) initialized\r\n");
   }
   ```

3. **参数调优**：
   ```c
   void TouchKey_Calibration(void)
   {
       uint16_t baseline;
       uint16_t touched_value;
       uint16_t sensitivity;

       printf("Calibration started...\r\n");
       printf("1. Do not touch the key\r\n");
       Delay_Ms(3000);

       // 测量基线值
       baseline = TouchKey_Read(ADC_Channel_2);
       printf("Baseline: %d\r\n", baseline);

       printf("2. Touch and hold the key\r\n");
       Delay_Ms(3000);

       // 测量触摸值
       touched_value = TouchKey_Read(ADC_Channel_2);
       printf("Touched value: %d\r\n", touched_value);

       // 计算灵敏度
       sensitivity = baseline - touched_value;
       printf("Sensitivity: %d\r\n", sensitivity);

       // 设置阈值（灵敏度的50%）
       uint16_t threshold = sensitivity / 2;
       printf("Recommended threshold: %d\r\n", threshold);
   }
   ```

### 4.2 多通道扫描配置步骤

```c
void TouchKey_Multi_Channel_Setup(void)
{
    // 1. 使能时钟
    RCC_APB2PeriphClockCmd(RCC_APB2Periph_GPIOA, ENABLE);
    RCC_APB2PeriphClockCmd(RCC_APB2Periph_ADC1, ENABLE);
    RCC_ADCCLKConfig(RCC_PCLK2_Div6);  // 更快的时钟用于多通道

    // 2. 配置所有触摸按键引脚
    GPIO_InitTypeDef GPIO_InitStructure = {0};
    GPIO_InitStructure.GPIO_Pin = GPIO_Pin_0 | GPIO_Pin_1 | GPIO_Pin_2 | GPIO_Pin_3;
    GPIO_InitStructure.GPIO_Mode = GPIO_Mode_AIN;
    GPIO_Init(GPIOA, &GPIO_InitStructure);

    // 3. 配置ADC为扫描模式
    ADC_InitTypeDef ADC_InitStructure = {0};
    ADC_InitStructure.ADC_Mode = ADC_Mode_Independent;
    ADC_InitStructure.ADC_ScanConvMode = ENABLE;
    ADC_InitStructure.ADC_ContinuousConvMode = ENABLE;
    ADC_InitStructure.ADC_ExternalTrigConv = ADC_ExternalTrigConv_None;
    ADC_InitStructure.ADC_DataAlign = ADC_DataAlign_Right;
    ADC_InitStructure.ADC_NbrOfChannel = 4;  // 4个通道
    ADC_Init(ADC1, &ADC_InitStructure);

    // 4. 配置扫描序列
    ADC_RegularChannelConfig(ADC1, ADC_Channel_0, 1, ADC_SampleTime_55Cycles5);
    ADC_RegularChannelConfig(ADC1, ADC_Channel_1, 2, ADC_SampleTime_55Cycles5);
    ADC_RegularChannelConfig(ADC1, ADC_Channel_2, 3, ADC_SampleTime_55Cycles5);
    ADC_RegularChannelConfig(ADC1, ADC_Channel_3, 4, ADC_SampleTime_55Cycles5);

    // 5. 配置DMA（可选，用于高效数据传输）
    #ifdef USE_DMA
    DMA_InitTypeDef DMA_InitStructure = {0};
    RCC_AHBPeriphClockCmd(RCC_AHBPeriph_DMA1, ENABLE);
    DMA_InitStructure.DMA_PeripheralBaseAddr = (uint32_t)&ADC1->RDATAR;
    DMA_InitStructure.DMA_MemoryBaseAddr = (uint32_t)adc_buffer;
    DMA_InitStructure.DMA_DIR = DMA_DIR_PeripheralSRC;
    DMA_InitStructure.DMA_BufferSize = 4;
    DMA_InitStructure.DMA_PeripheralInc = DMA_PeripheralInc_Disable;
    DMA_InitStructure.DMA_MemoryInc = DMA_MemoryInc_Enable;
    DMA_InitStructure.DMA_PeripheralDataSize = DMA_PeripheralDataSize_HalfWord;
    DMA_InitStructure.DMA_MemoryDataSize = DMA_MemoryDataSize_HalfWord;
    DMA_InitStructure.DMA_Mode = DMA_Mode_Circular;
    DMA_InitStructure.DMA_Priority = DMA_Priority_High;
    DMA_InitStructure.DMA_M2M = DMA_M2M_Disable;
    DMA_Init(DMA1_Channel1, &DMA_InitStructure);
    DMA_Cmd(DMA1_Channel1, ENABLE);
    ADC_DMACmd(ADC1, ENABLE);
    #endif

    // 6. 使能ADC和触摸按键
    ADC_Cmd(ADC1, ENABLE);
    TKey1->CTLR1 |= (1 << 26) | (1 << 24);

    printf("Multi-channel touch keys initialized\r\n");
}
```

### 4.3 触摸按键优化配置

```c
// 触摸按键优化配置函数
void TouchKey_Optimized_Setup(TouchKey_Config_t *config)
{
    // 根据配置设置充电和放电时间
    void Set_Charge_Discharge_Times(uint16_t charge_time, uint16_t discharge_time)
    {
        // 充电时间影响灵敏度
        // 值越大，充电时间越长，灵敏度越高
        TKey1->IDATAR1 = charge_time;

        // 放电时间影响检测范围
        // 值越大，放电时间越长，测量范围越大
        TKey1->RDATAR = discharge_time;
    }

    // 根据环境条件自动调整参数
    void Auto_Adjust_Parameters(void)
    {
        uint16_t baseline = Measure_Baseline();
        uint16_t noise_level = Measure_Noise_Level();

        // 根据噪声水平调整阈值
        if(noise_level > 50) {
            // 高噪声环境，增加阈值
            config->threshold = baseline * 0.15;
        } else {
            // 低噪声环境，降低阈值
            config->threshold = baseline * 0.08;
        }

        // 根据温度调整参数（如果支持温度传感器）
        #ifdef TEMP_SENSOR_ENABLED
        float temperature = Read_Temperature();
        if(temperature > 40.0) {
            // 高温时减少充电时间
            config->charge_time = config->charge_time * 0.9;
        } else if(temperature < 10.0) {
            // 低温时增加充电时间
            config->charge_time = config->charge_time * 1.1;
        }
        #endif

        // 限制参数范围
        if(config->charge_time < 0x08) config->charge_time = 0x08;
        if(config->charge_time > 0x40) config->charge_time = 0x40;
        if(config->discharge_time < 0x04) config->discharge_time = 0x04;
        if(config->discharge_time > 0x20) config->discharge_time = 0x20;
    }

    // 应用优化配置
    Set_Charge_Discharge_Times(config->charge_time, config->discharge_time);
    Auto_Adjust_Parameters();
}
```

### 4.4 低功耗触摸按键配置

```c
// 低功耗触摸按键配置
void TouchKey_LowPower_Setup(void)
{
    // 1. 降低ADC时钟频率
    RCC_ADCCLKConfig(RCC_PCLK2_Div16);  // 较低时钟频率

    // 2. 使用较短的采样时间
    ADC_RegularChannelConfig(ADC1, ADC_Channel_2, 1, ADC_SampleTime_7Cycles5);

    // 3. 配置为单次转换模式
    ADC_InitTypeDef ADC_InitStructure = {0};
    ADC_InitStructure.ADC_ContinuousConvMode = DISABLE;  // 单次模式

    // 4. 使用外部触发（定时器）以减少CPU干预
    ADC_InitStructure.ADC_ExternalTrigConv = ADC_ExternalTrigConv_T1_CC1;

    // 5. 配置中断唤醒
    ADC_ITConfig(ADC1, ADC_IT_EOC, ENABLE);
    NVIC_EnableIRQ(ADC1_2_IRQn);

    // 6. 进入低功耗模式前保存状态
    void Enter_LowPower_Mode(void)
    {
        // 保存当前配置
        uint32_t saved_ctlr1 = TKey1->CTLR1;
        uint32_t saved_idatar1 = TKey1->IDATAR1;

        // 进入低功耗模式
        PWR_EnterSTOPMode(PWR_Regulator_LowPower, PWR_STOPEntry_WFI);

        // 唤醒后恢复配置
        TKey1->CTLR1 = saved_ctlr1;
        TKey1->IDATAR1 = saved_idatar1;
    }

    printf("Low-power touch key configured\r\n");
}
```

## 5. 示例工程详解

### 5.1 TOUCHKEY示例工程

- **位置**：`TOUCHKEY/TKey/User/main.c`
- **功能**：演示单通道触摸按键基本功能，PA2通道
- **特点**：
  - 简单的单通道触摸按键检测
  - 实时输出ADC值到串口
  - 500ms采样间隔
  - 基本的初始化配置

### 5.2 工程结构说明

```
TOUCHKEY/
└── TKey/
    ├── .cproject                    # Eclipse项目配置
    ├── .project                     # Eclipse项目文件
    ├── TOUCHKEY.wvproj              # WCH IDE项目文件
    └── User/
        ├── main.c                   # 主程序文件
        ├── ch32v30x_it.c            # 中断服务程序
        ├── ch32v30x_it.h            # 中断头文件
        ├── ch32v30x_conf.h          # 外设配置文件
        ├── system_ch32v30x.c        # 系统初始化
        └── system_ch32v30x.h        # 系统头文件
```

### 5.3 关键功能点

1. **触摸按键初始化流程**：
   - 使能GPIOA和ADC1时钟
   - 配置PA2为模拟输入模式
   - 配置ADC为独立模式、单次转换
   - 使能触摸按键和缓冲器

2. **ADC读取流程**：
   - 设置充电时间（IDATAR1 = 0x10）
   - 设置放电时间（RDATAR = 0x8）
   - 配置ADC通道
   - 等待转换完成标志
   - 读取RDATAR寄存器值

3. **硬件连接**：
   - 触摸电极连接到PA2
   - 串口用于调试输出（PA9-TX, PA10-RX）
   - 系统时钟使用HSE 96MHz

4. **性能特性**：
   - 采样频率：2Hz（500ms间隔）
   - ADC分辨率：12位
   - 检测灵敏度：可调充电/放电时间

### 5.4 参数调整指南

```c
// 灵敏度调整
void Adjust_Sensitivity(void)
{
    // 增加充电时间提高灵敏度（更容易触发）
    TKey1->IDATAR1 = 0x20;  // 原始值0x10

    // 减少充电时间降低灵敏度（更难触发）
    // TKey1->IDATAR1 = 0x08;

    // 调整放电时间改变检测范围
    TKey1->RDATAR = 0x10;   // 原始值0x8

    // 调整采样时间改善噪声抑制
    ADC_RegularChannelConfig(ADC1, ADC_Channel_2, 1, ADC_SampleTime_55Cycles5);
}
```

## 6. 常见问题

### Q1: 触摸按键检测不灵敏或误触发？
A: 调整以下参数：
1. **充电时间（IDATAR1）**：增加充电时间提高灵敏度，减少则降低灵敏度
2. **放电时间（RDATAR）**：调整放电时间改变检测范围
3. **阈值设置**：根据实际环境调整触发阈值
4. **硬件设计**：检查电极大小、对地电容、走线长度

### Q2: 如何提高抗干扰能力？
A: 抗干扰措施：
1. **硬件滤波**：在电极上并联10-47pF对地电容
2. **软件滤波**：实现移动平均或中值滤波器
3. **增加采样时间**：使用更长的ADC采样时间
4. **屏蔽设计**：在电极周围添加接地屏蔽环
5. **基线跟踪**：实现动态基线调整

### Q3: 多个触摸按键互相干扰？
A: 解决多通道干扰：
1. **增加间距**：电极间保持足够距离（至少2倍电极直径）
2. **时分复用**：不同时采样相邻通道
3. **屏蔽电极**：在相邻电极间添加接地隔离
4. **软件补偿**：检测到多个按键时进行优先级处理
5. **调整参数**：为每个通道独立设置充电/放电时间

### Q4: 如何实现低功耗触摸检测？
A: 低功耗优化：
1. **降低采样率**：根据应用需求降低采样频率
2. **单次转换模式**：使用单次转换代替连续转换
3. **定时唤醒**：使用定时器触发采样
4. **休眠模式**：在采样间隔进入低功耗模式
5. **优化参数**：使用最小的充电/放电时间

### Q5: 触摸按键响应延迟大？
A: 优化响应时间：
1. **减少滤波器长度**：使用更短的滤波器窗口
2. **降低消抖计数**：减少消抖确认次数
3. **提高采样率**：增加采样频率
4. **优化算法**：使用更高效的检测算法
5. **硬件优化**：减小电极电容

## 7. API参考

### 7.1 触摸按键控制API

| 函数名 | 功能描述 | 参数说明 |
|--------|----------|----------|
| `TKey1->CTLR1 |= TKEY_CTLR1_TKEN` | 使能触摸按键功能 | 无 |
| `TKey1->CTLR1 |= TKEY_CTLR1_BUFEN` | 使能缓冲器 | 无 |
| `TKey1->IDATAR1 = value` | 设置充电时间 | value：充电时间值（0x00-0xFF） |
| `TKey1->RDATAR = value` | 设置放电时间/读取ADC值 | value：放电时间值（0x00-0xFF） |

### 7.2 ADC配置API

| 函数名 | 功能描述 | 参数说明 |
|--------|----------|----------|
| `ADC_Init` | 初始化ADC | ADCx：ADC实例，ADC_InitStruct：初始化结构 |
| `ADC_RegularChannelConfig` | 配置规则通道 | ADCx：ADC实例，ADC_Channel：通道，Rank：序列位置，ADC_SampleTime：采样时间 |
| `ADC_Cmd` | 使能/禁用ADC | ADCx：ADC实例，NewState：新状态 |
| `ADC_GetFlagStatus` | 获取ADC标志状态 | ADCx：ADC实例，ADC_FLAG：标志位 |
| `ADC_GetConversionValue` | 获取ADC转换值 | ADCx：ADC实例 |

### 7.3 GPIO配置API

| 函数名 | 功能描述 | 参数说明 |
|--------|----------|----------|
| `GPIO_Init` | 初始化GPIO | GPIOx：GPIO端口，GPIO_InitStruct：初始化结构 |
| `RCC_APB2PeriphClockCmd` | 使能APB2外设时钟 | RCC_APB2Periph：外设，NewState：新状态 |

### 7.4 时钟配置API

| 函数名 | 功能描述 | 参数说明 |
|--------|----------|----------|
| `RCC_ADCCLKConfig` | 配置ADC时钟 | RCC_PCLK2_Divx：分频系数 |
| `SystemCoreClockUpdate` | 更新系统时钟变量 | 无 |

## 8. 注意事项

1. **硬件设计**：
   - 电极大小影响灵敏度，典型直径8-12mm
   - 对地电容影响响应时间，典型值10-47pF
   - 走线尽量短，远离噪声源
   - 使用屏蔽线或地线隔离

2. **环境因素**：
   - 温度变化影响电容值，需要温度补偿
   - 湿度变化影响介电常数
   - 电磁干扰可能引起误触发

3. **软件优化**：
   - 实现自动校准适应不同环境
   - 添加数字滤波器减少噪声影响
   - 实现动态阈值调整

4. **电源稳定性**：
   - 使用稳定的电源供电
   - 添加电源滤波电容
   - 避免电源噪声耦合

5. **机械结构**：
   - 面板厚度影响灵敏度
   - 面板材料影响介电常数
   - 安装方式影响检测一致性

## 9. 性能优化建议

1. **灵敏度优化**：
   ```c
   // 根据应用场景调整参数
   if (application == HIGH_SENSITIVITY) {
       TKey1->IDATAR1 = 0x20;  // 高灵敏度
       TKey1->RDATAR = 0x04;   // 短放电时间
   } else if (application == NOISY_ENVIRONMENT) {
       TKey1->IDATAR1 = 0x08;  // 低灵敏度
       TKey1->RDATAR = 0x10;   // 长放电时间
   }
   ```

2. **响应时间优化**：
   ```c
   // 平衡响应时间和稳定性
   uint8_t debounce_time = 20;  // 20ms消抖时间
   uint8_t filter_length = 4;   // 4点移动平均
   uint16_t sample_rate = 50;   // 50Hz采样率
   ```

3. **功耗优化**：
   ```c
   // 低功耗配置
   void LowPower_Configuration(void)
   {
       // 降低采样率
       Set_Sample_Rate(10);  // 10Hz

       // 使用单次转换模式
       ADC_InitStructure.ADC_ContinuousConvMode = DISABLE;

       // 在空闲时关闭触摸按键
       if (idle_time > 1000) {
           TKey1->CTLR1 &= ~TKEY_CTLR1_TKEN;
       }
   }
   ```

4. **抗干扰优化**：
   ```c
   // 综合抗干扰措施
   void AntiInterference_Measures(void)
   {
       // 1. 硬件滤波
       TKey1->CTLR1 |= TKEY_CTLR1_BUFEN;  // 使能缓冲器

       // 2. 软件滤波
       Implement_Median_Filter(5);  // 5点中值滤波

       // 3. 动态阈值
       Implement_Dynamic_Threshold();

       // 4. 频率跳变（如果支持）
       if (detect_interference) {
           Change_Sampling_Frequency();
       }
   }
   ```

## 10. 典型应用场景

### 10.1 家用电器触摸控制

```c
// 家电触摸面板控制
void HomeAppliance_Touch_Control(void)
{
    // 定义家电功能
    typedef enum {
        FUNCTION_POWER,
        FUNCTION_MODE,
        FUNCTION_TEMP_UP,
        FUNCTION_TEMP_DOWN,
        FUNCTION_TIMER,
        FUNCTION_LOCK
    } Appliance_Function;

    // 触摸按键映射
    TouchKey_Config_t touch_panel[] = {
        {ADC_Channel_0, GPIO_Pin_0, GPIOA, 0x10, 0x08, ADC_SampleTime_55Cycles5, 0, 50, 10, 5},  // 电源
        {ADC_Channel_1, GPIO_Pin_1, GPIOA, 0x10, 0x08, ADC_SampleTime_55Cycles5, 0, 50, 10, 5},  // 模式
        {ADC_Channel_2, GPIO_Pin_2, GPIOA, 0x10, 0x08, ADC_SampleTime_55Cycles5, 0, 50, 10, 5},  // 温度+
        {ADC_Channel_3, GPIO_Pin_3, GPIOA, 0x10, 0x08, ADC_SampleTime_55Cycles5, 0, 50, 10, 5},  // 温度-
        {ADC_Channel_4, GPIO_Pin_4, GPIOA, 0x10, 0x08, ADC_SampleTime_55Cycles5, 0, 50, 10, 5},  // 定时
        {ADC_Channel_5, GPIO_Pin_5, GPIOA, 0x10, 0x08, ADC_SampleTime_55Cycles5, 0, 50, 10, 5},  // 锁定
    };

    // 触摸事件处理
    void Touch_Event_Handler(uint8_t key_index, uint8_t event)
    {
        switch(key_index) {
            case 0:  // 电源键
                if(event == TOUCH_EVENT_PRESS) {
                    Toggle_Power();
                } else if(event == TOUCH_EVENT_LONG_PRESS) {
                    Emergency_Shutdown();
                }
                break;

            case 1:  // 模式键
                if(event == TOUCH_EVENT_PRESS) {
                    Cycle_Operation_Mode();
                }
                break;

            case 2:  // 温度+
                if(event == TOUCH_EVENT_PRESS) {
                    Increase_Temperature();
                } else if(event == TOUCH_EVENT_LONG_PRESS) {
                    Rapid_Increase_Temperature();
                }
                break;

            case 3:  // 温度-
                if(event == TOUCH_EVENT_PRESS) {
                    Decrease_Temperature();
                } else if(event == TOUCH_EVENT_LONG_PRESS) {
                    Rapid_Decrease_Temperature();
                }
                break;

            case 4:  // 定时键
                if(event == TOUCH_EVENT_DOUBLE_TAP) {
                    Set_Timer(60);  // 1小时
                } else if(event == TOUCH_EVENT_PRESS) {
                    Set_Timer(30);  // 30分钟
                }
                break;

            case 5:  // 锁定键
                if(event == TOUCH_EVENT_LONG_PRESS) {
                    Toggle_Child_Lock();
                }
                break;
        }
    }

    // 初始化触摸面板
    for(int i = 0; i < 6; i++) {
        Initialize_TouchKey(&touch_panel[i]);
    }

    // 设置回调函数
    Set_TouchKey_Callback(Touch_Event_Handler);
}
```

### 10.2 工业控制触摸界面

```c
// 工业触摸界面
void Industrial_Touch_Interface(void)
{
    // 工业环境特殊要求
    void Industrial_Environment_Setup(void)
    {
        // 1. 高抗干扰配置
        TKey1->IDATAR1 = 0x08;  // 较低灵敏度
        TKey1->RDATAR = 0x10;   // 较长放电时间

        // 2. 强滤波
        Implement_Strong_Filter(10, 0.7);  // 10点滤波，系数0.7

        // 3. 高阈值
        Set_Detection_Threshold(100);  // 较高阈值

        // 4. 长消抖时间
        Set_Debounce_Time(100);  // 100ms消抖

        // 5. 环境自适应
        Enable_Environment_Adaptation();
    }

    // 安全关键触摸处理
    void Safety_Critical_Touch_Handling(uint8_t key_index, uint8_t event)
    {
        static uint8_t safety_confirmation = 0;

        switch(key_index) {
            case SAFETY_STOP_BUTTON:
                if(event == TOUCH_EVENT_LONG_PRESS) {
                    // 长按2秒急停
                    Emergency_Stop();
                } else if(event == TOUCH_EVENT_DOUBLE_TAP) {
                    // 双击确认安全操作
                    safety_confirmation = 1;
                }
                break;

            case START_BUTTON:
                if(event == TOUCH_EVENT_PRESS && safety_confirmation) {
                    Start_Machine();
                    safety_confirmation = 0;
                }
                break;

            case RESET_BUTTON:
                if(event == TOUCH_EVENT_LONG_PRESS) {
                    // 长按3秒复位
                    Factory_Reset();
                }
                break;
        }
    }

    // 触摸界面状态管理
    typedef struct {
        uint8_t current_screen;
        uint8_t selected_item;
        uint8_t input_mode;
        uint32_t last_interaction;
    } TouchUI_State;

    TouchUI_State ui_state = {0};

    // 触摸手势识别
    void Touch_Gesture_Recognition(void)
    {
        // 滑动手势检测
        static uint16_t last_values[4] = {0};
        static uint32_t last_times[4] = {0};
        static uint8_t gesture_buffer[10] = {0};

        // 检测滑动方向
        // 实现手势识别逻辑...
    }
}
```

### 10.3 便携设备触摸控制

```c
// 便携设备触摸控制
void Portable_Device_Touch_Control(void)
{
    // 低功耗触摸配置
    void Portable_Device_Setup(void)
    {
        // 1. 优化功耗
        Set_Sample_Rate(20);  // 20Hz采样率
        Enable_WakeOn_Touch();  // 触摸唤醒

        // 2. 自适应灵敏度
        Enable_Auto_Sensitivity_Adjust();

        // 3. 运动检测（如果支持加速度计）
        #ifdef ACCELEROMETER_ENABLED
        Enable_Motion_Awake();
        #endif

        // 4.  proximity检测（如果支持接近传感器）
        #ifdef PROXIMITY_SENSOR_ENABLED
        Enable_Proximity_Detection();
        #endif
    }

    // 手势控制
    typedef enum {
        GESTURE_NONE,
        GESTURE_SWIPE_LEFT,
        GESTURE_SWIPE_RIGHT,
        GESTURE_SWIPE_UP,
        GESTURE_SWIPE_DOWN,
        GESTURE_TAP,
        GESTURE_DOUBLE_TAP,
        GESTURE_LONG_PRESS,
        GESTURE_PINCH,
        GESTURE_SPREAD
    } TouchGesture;

    // 多点触摸支持（如果硬件支持）
    #ifdef MULTI_TOUCH_SUPPORT
    void MultiTouch_Handling(void)
    {
        // 检测多个触摸点
        TouchPoint points[MAX_TOUCH_POINTS];

        // 处理多点触摸手势
        Process_MultiTouch_Gestures(points, MAX_TOUCH_POINTS);

        // 应用响应
        Apply_MultiTouch_Response(points, MAX_TOUCH_POINTS);
    }
    #endif

    // 触摸反馈
    void Provide_Touch_Feedback(uint8_t key_index, uint8_t event)
    {
        // 1. 视觉反馈（LED）
        if(event == TOUCH_EVENT_PRESS) {
            LED_Blink(key_index, 100);  // 100ms闪烁
        }

        // 2. 声音反馈（蜂鸣器）
        if(event == TOUCH_EVENT_PRESS) {
            Buzzer_Beep(1000, 50);  // 1kHz，50ms
        }

        // 3. 振动反馈（振动电机）
        if(event == TOUCH_EVENT_PRESS) {
            Vibrate(50);  // 振动50ms
        }

        // 4. 屏幕反馈（如果连接屏幕）
        #ifdef DISPLAY_ENABLED
        Highlight_Button(key_index);
        #endif
    }
}
```

## 11. 总结

CH32V307的TOUCHKEY触摸按键功能提供了灵活可靠的电容式触摸检测解决方案。通过合理的硬件设计和软件配置，可以实现从简单按钮到复杂手势识别的各种触摸应用。开发时需要注意灵敏度调整、抗干扰设计、功耗优化等关键环节，同时充分利用TOUCHKEY模块的硬件特性和软件算法的优势，构建稳定可靠的触摸交互系统。
# CH32V307 VoiceRcgExam语音识别示例使用指南

## 1. 外设概述

VoiceRcgExam（语音识别示例）是CH32V307基于语音识别算法的孤立词语音识别实现。主要特性包括：

- **孤立词识别**：支持4个关键词（"up", "down", "left", "right"）的识别
- **双音频输入模式**：支持ES8388音频编解码器（I2S接口）和ADC模拟音频输入
- **MFCC特征提取**：使用Mel频率倒谱系数进行语音特征提取
- **动态时间规整**：使用DTW或类似算法进行模式匹配
- **Flash存储模型**：训练的特征参数存储在Flash中
- **训练/识别模式**：支持语音训练和实时识别两种模式
- **噪声处理**：包含环境噪声分析和有效语音段检测

## 2. 关键代码片段

### 2.1 语音识别参数配置

```c
// VoiceRcg.h 中的参数定义
#define DataSampleFrq       8000    // 采样频率8kHz
#define SampleTime          2000    // 总采样时间2秒
#define SampleDataLen       ((DataSampleFrq/1000)*SampleTime)  // 16000个样本
#define SampleNoiseTime     300     // 噪声采样时间300ms

// MFCC特征参数结构
typedef struct __attribute__((packed))
{
    uint16_t magic;             // 魔术字检查（0x55AA）
    uint16_t seg_cnt;           // 分段数量
    float    chara_para[120*12];// 特征参数（120帧×12维MFCC）
} vr_chara_para;
```

### 2.2 音频数据采集（I2S模式）

```c
// Get_Data.c 中的I2S配置
void I2S_Data_Get(uint16_t *DataBuff)
{
    I2S_InitTypeDef I2S_InitStructure;

    // I2S配置：主接收模式，8kHz采样，16位数据
    I2S_InitStructure.I2S_Mode = I2S_Mode_MasterRx;
    I2S_InitStructure.I2S_AudioFreq = I2S_AudioFreq_8k;
    I2S_InitStructure.I2S_DataFormat = I2S_DataFormat_16b;

    I2S_Init(I2S2, &I2S_InitStructure);
    I2S_Cmd(I2S2, ENABLE);
}

// DMA传输完成中断处理
void DMA1_Channel5_IRQHandler(void)
{
    // 将有符号16位音频数据转换为无符号16位
    for(uint16_t i=0;i<SampleDataLen;i++)
    {
        V_Data[i]=(uint16_t)((int16_t)V_Data[i]+32768);
    }
    g_data_ready=1;  // 设置数据就绪标志
}
```

### 2.3 语音训练流程

```c
// main.c 中的训练函数
uint8_t parameters_practice(void)
{
    uint32_t addr = charamls_start_addr;
    uint16_t i;

    // 训练4个关键词，每个关键词2个样本
    for (i = 0; i < kw_num*charaml_per_kw; i++)
    {
        printf("please speak:%s \r\n",key_words[i/charaml_per_kw]);

        // 录音
        voice_record();

        // 保存特征参数到Flash
        save_chara_para_mdl(V_Data,addr);
        addr += size_per_chara;
    }

    return 0;
}
```

### 2.4 语音识别流程

```c
// 语音识别核心函数
void voice_recongition(void)
{
    uint32_t chara_para_addr;
    vr_chara_para *chara_para_mdl;
    dtg_para_type dtg_para;
    vr_active_segment active_segment;
    vr_chara_para chara_para;
    uint32_t i,keyword_min=0;
    float min_dis=1e10,cur_dis;

    // 1. 录音
    voice_record();

    // 2. 环境噪声分析
    environment_noise(V_Data,SampleNoiseLen,&dtg_para);

    // 3. 有效段检测
    active_segment_detect(V_Data, SampleDataLen, &active_segment, &dtg_para);

    // 4. 计算MFCC特征
    calc_mfcc_chara_para(&active_segment,&chara_para,&dtg_para);

    // 5. 与存储的模型进行匹配
    for(chara_para_addr=charamls_start_addr, i=0;
        chara_para_addr<charamls_end_addr;
        chara_para_addr+=size_per_chara, i++)
    {
        chara_para_mdl=(vr_chara_para *)chara_para_addr;
        cur_dis=calc_chara_para_match_dis(&chara_para,chara_para_mdl);

        if(cur_dis<min_dis)  // 找到最小距离
        {
           min_dis=cur_dis;
           keyword_min=i;
        }
    }

    // 6. 输出识别结果
    if(min_dis<MATCH_DIS_THRESHOLD)
    {
        printf("successful, word:%s\r\n",key_words[keyword_min/charaml_per_kw]);
    }
    else
    {
        printf("no match\r\n");
    }
}
```

### 2.5 ES8388音频编解码器配置

```c
// es8388.c 中的初始化函数
void ES8388_Init(void)
{
    // I2C配置
    I2C_InitTypeDef I2C_InitStructure;
    I2C_InitStructure.I2C_ClockSpeed = 400000;  // 400kHz
    I2C_InitStructure.I2C_Mode = I2C_Mode_I2C;
    I2C_InitStructure.I2C_DutyCycle = I2C_DutyCycle_2;
    I2C_InitStructure.I2C_OwnAddress1 = 0x00;
    I2C_InitStructure.I2C_Ack = I2C_Ack_Enable;
    I2C_InitStructure.I2C_AcknowledgedAddress = I2C_AcknowledgedAddress_7bit;
    I2C_Init(I2C2, &I2C_InitStructure);
    I2C_Cmd(I2C2, ENABLE);

    // ES8388寄存器配置
    ES8388_Write_Reg(0x00, 0x80);  // 复位
    ES8388_Write_Reg(0x01, 0x06);  // 电源管理
    ES8388_Write_Reg(0x02, 0xF3);  // 左通道输入选择
    ES8388_Write_Reg(0x03, 0xF3);  // 右通道输入选择
    ES8388_Write_Reg(0x04, 0x0C);  // 左通道音量
    ES8388_Write_Reg(0x05, 0x0C);  // 右通道音量
    ES8388_Write_Reg(0x06, 0x7C);  // ADC控制
    ES8388_Write_Reg(0x07, 0x02);  // 主模式，I2S格式
    ES8388_Write_Reg(0x08, 0x00);  // 采样率8kHz
}
```

## 3. 配置数据结构

### 3.1 语音识别参数结构

```c
// 检测参数结构
typedef struct
{
    uint16_t sample_frq;      // 采样频率
    uint16_t sample_len;      // 采样长度
    uint16_t nfft_len;        // FFT长度
    uint16_t nfft_num;        // FFT数量
    uint16_t sample_noise_len;// 噪声采样长度
    float    win_len_sec;     // 窗口长度（秒）
    float    win_shift_sec;   // 窗口偏移（秒）
    float    fbank_num;       // 滤波器组数量
    uint16_t mfcc_dim;        // MFCC维度
} dtg_para_type;

// 有效段结构
typedef struct
{
    uint16_t start_index;     // 开始索引
    uint16_t end_index;       // 结束索引
    uint16_t seg_len;         // 段长度
} vr_active_segment;

// 特征参数头部
typedef struct
{
    uint16_t magic;           // 魔术字（0x55AA）
    uint16_t seg_cnt;         // 段数量
} vr_chara_para_header;
```

### 3.2 Flash存储配置

```c
// Flash存储地址定义
#define charamls_start_addr  (charamls_end_addr-charamls_size)
#define charamls_end_addr    0x08070800      // Flash结束地址
#define charamls_size        (64*1024)       // 64KB存储空间
#define size_per_chara       (8*1024)        // 每个特征参数8KB
#define charaml_per_kw       2               // 每个关键词2个样本
#define kw_num              4               // 4个关键词

// 关键词定义
const char *key_words[4] = {"up", "down", "left", "right"};

// 匹配距离阈值
#define MATCH_DIS_THRESHOLD  100.0f
```

### 3.3 I2S和DMA配置结构

```c
// I2S初始化结构
typedef struct
{
    uint16_t I2S_Mode;         // I2S工作模式
    uint16_t I2S_Standard;     // I2S标准
    uint16_t I2S_DataFormat;   // 数据格式
    uint16_t I2S_MCLKOutput;   // 主时钟输出
    uint32_t I2S_AudioFreq;    // 音频频率
    uint16_t I2S_CPOL;         // 时钟极性
} I2S_InitTypeDef;

// DMA初始化结构
typedef struct
{
    uint32_t DMA_PeripheralBaseAddr;  // 外设基地址
    uint32_t DMA_MemoryBaseAddr;      // 内存基地址
    uint32_t DMA_DIR;                 // 数据传输方向
    uint32_t DMA_BufferSize;          // 缓冲区大小
    uint32_t DMA_PeripheralInc;       // 外设地址增量
    uint32_t DMA_MemoryInc;           // 内存地址增量
    uint32_t DMA_PeripheralDataSize;  // 外设数据宽度
    uint32_t DMA_MemoryDataSize;      // 内存数据宽度
    uint32_t DMA_Mode;                // DMA模式
    uint32_t DMA_Priority;            // 优先级
    uint32_t DMA_M2M;                 // 内存到内存模式
} DMA_InitTypeDef;
```

## 4. 通用配置步骤

### 4.1 硬件连接步骤

1. **音频输入连接**：
   ```c
   // ES8388连接方案：
   // PB10: I2C2_SCL (ES8388配置)
   // PB11: I2C2_SDA (ES8388配置)
   // PB12: I2S2_WS (ES8388 LRCK)
   // PB13: I2S2_CK (ES8388 BCLK)
   // PB15: I2S2_SD (ES8388 DOUT)
   // PC6: I2S2_MCK (ES8388 MCLK)
   // PA8: ES8388控制引脚

   // 备用ADC方案：
   // PA1: ADC输入引脚（模拟麦克风）
   ```

2. **按键连接**：
   ```c
   // PE2: 训练模式触发按键（上拉输入）
   ```

3. **串口连接**：
   ```c
   // PA9: USART1_TX (调试输出)
   // PA10: USART1_RX (可选)
   ```

### 4.2 系统初始化步骤

```c
void System_Init(void)
{
    // 1. 更新系统时钟
    SystemCoreClockUpdate();

    // 2. 配置中断优先级分组
    NVIC_PriorityGroupConfig(NVIC_PriorityGroup_2);

    // 3. 初始化延时函数
    Delay_Init();

    // 4. 初始化串口（921600波特率）
    USART_Printf_Init(921600);
    printf("SystemClk:%d\r\n", SystemCoreClock);

    // 5. 初始化ES8388音频编解码器
    ES8388_Init();

    // 6. 初始化I2S接口
    I2S_Data_Get_Init();

    // 7. 初始化DMA传输
    DMA_For_I2S_Init();

    // 8. 检查Flash中的训练模型
    Check_Trained_Model();
}
```

### 4.3 语音训练步骤

```c
void Voice_Training_Procedure(void)
{
    // 1. 等待训练按键按下
    while(GPIO_ReadInputDataBit(GPIOE, GPIO_Pin_2) != 0);
    Delay_Ms(20);  // 按键消抖
    while(GPIO_ReadInputDataBit(GPIOE, GPIO_Pin_2) == 0);

    // 2. 提示开始训练
    printf("Starting voice training...\r\n");

    // 3. 执行参数训练
    if(parameters_practice() == 0)
    {
        printf("Training completed successfully!\r\n");
    }
    else
    {
        printf("Training failed!\r\n");
    }
}
```

### 4.4 语音识别步骤

```c
void Voice_Recognition_Procedure(void)
{
    static uint32_t last_recognition_time = 0;
    uint32_t current_time = GetTickCount();

    // 每1秒进行一次语音识别
    if(current_time - last_recognition_time >= 1000)
    {
        printf("Listening...\r\n");
        voice_recongition();
        last_recognition_time = current_time;
    }
}
```

### 4.5 主程序流程

```c
int main(void)
{
    // 系统初始化
    System_Init();

    // 主循环
    while(1)
    {
        // 检查是否需要训练
        if(GPIO_ReadInputDataBit(GPIOE, GPIO_Pin_2) == 0)
        {
            Voice_Training_Procedure();
        }
        else
        {
            // 正常识别模式
            Voice_Recognition_Procedure();
        }

        // 其他任务处理
        Delay_Ms(10);
    }
}
```

## 5. 示例工程详解

### 5.1 VoiceRcgExam工程结构

- **位置**：`VoiceRcgExam/VoiceRcgExam/User/main.c`
- **功能**：演示孤立词语音识别的完整流程，包括训练和识别
- **特点**：
  - 支持ES8388音频编解码器和ADC两种输入方式
  - 包含完整的MFCC特征提取算法
  - 使用Flash存储训练模型
  - 提供训练和识别两种操作模式

### 5.2 工程文件说明

```
VoiceRcgExam/
└── VoiceRcgExam/
    ├── .cproject                    # Eclipse项目配置
    ├── .project                     # Eclipse项目文件
    ├── .template                    # 模板文件
    ├── VoiceRcgExam.wvproj          # WCH IDE项目文件
    └── User/
        ├── ch32v30x_conf.h          # 外设配置文件
        ├── ch32v30x_it.c            # 中断服务程序
        ├── ch32v30x_it.h            # 中断头文件
        ├── es8388.c                 # ES8388音频编解码器驱动
        ├── es8388.h                 # ES8388头文件
        ├── Get_Data.c               # 音频数据获取实现
        ├── Get_Data.h               # 音频数据获取头文件
        ├── libVoiceRcg.a            # 语音识别算法库（静态库）
        ├── Link.ld                  # 链接脚本
        ├── main.c                   # 主程序文件
        ├── system_ch32v30x.c        # 系统初始化
        ├── system_ch32v30x.h        # 系统初始化头文件
        └── VoiceRcg.h               # 语音识别算法头文件
```

### 5.3 语音识别算法库

**libVoiceRcg.a**包含以下核心函数：

1. **环境噪声分析**：
   ```c
   void environment_noise(uint16_t *noise_data, uint16_t noise_len, dtg_para_type *dtg_para);
   ```

2. **有效段检测**：
   ```c
   void active_segment_detect(uint16_t *data, uint16_t data_len,
                              vr_active_segment *active_segment, dtg_para_type *dtg_para);
   ```

3. **MFCC特征计算**：
   ```c
   void calc_mfcc_chara_para(vr_active_segment *active_segment,
                             vr_chara_para *chara_para, dtg_para_type *dtg_para);
   ```

4. **特征匹配距离计算**：
   ```c
   float calc_chara_para_match_dis(vr_chara_para *chara_para1, vr_chara_para *chara_para2);
   ```

5. **特征参数保存**：
   ```c
   void save_mfcc_chara_para(vr_chara_para *chara_para, uint32_t flash_addr);
   ```

### 5.4 内存布局配置

```c
// Link.ld 中的内存配置
MEMORY
{
  FLASH (rx) : ORIGIN = 0x00000000, LENGTH = 192K
  RAM (rwx) : ORIGIN = 0x20000000, LENGTH = 128K
}

// 特征参数存储区域
SECTIONS
{
  .charamls 0x08070800 :
  {
    . = ALIGN(4);
    KEEP(*(.charamls))
    . = ALIGN(4);
  } >FLASH
}
```

## 6. 常见问题

### Q1: 语音识别准确率低？
A: 可能原因：
1. 环境噪声过大
2. 训练和识别时说话方式不一致
3. 麦克风质量或位置问题
4. 关键词长度不合适

解决方案：
- 在相对安静的环境下训练和识别
- 训练和识别时使用相似的语速和音量
- 确保麦克风正常工作
- 关键词长度建议在0.5-1.5秒之间

### Q2: 训练模型保存失败？
A: 检查以下配置：
1. Flash地址是否在有效范围内
2. Flash是否已擦除
3. 特征参数大小是否正确
4. Flash寿命是否耗尽

### Q3: 音频数据采集异常？
A: 诊断步骤：
1. 检查ES8388/I2S硬件连接
2. 验证I2C配置是否正确
3. 检查采样率设置
4. 确认DMA传输配置

### Q4: 识别响应延迟大？
A: 优化建议：
1. 减少采样时间（如从2秒减少到1.5秒）
2. 优化MFCC计算算法
3. 使用更高效的匹配算法
4. 增加CPU时钟频率

### Q5: 系统资源不足？
A: 资源占用情况：
- RAM：音频缓冲区约32KB
- Flash：特征参数存储64KB
- CPU：MFCC计算需要较多运算资源

优化方案：
- 使用压缩算法减少存储空间
- 优化特征维度
- 使用定点数运算替代浮点运算

## 7. API参考

### 7.1 语音识别库API

| 函数名 | 功能描述 | 参数说明 | 返回值 |
|--------|----------|----------|--------|
| `environment_noise` | 环境噪声分析 | `noise_data`: 噪声数据，`noise_len`: 噪声长度，`dtg_para`: 检测参数 | 无 |
| `active_segment_detect` | 有效段检测 | `data`: 音频数据，`data_len`: 数据长度，`active_segment`: 有效段，`dtg_para`: 检测参数 | 无 |
| `calc_mfcc_chara_para` | 计算MFCC特征 | `active_segment`: 有效段，`chara_para`: 特征参数，`dtg_para`: 检测参数 | 无 |
| `calc_chara_para_match_dis` | 计算特征匹配距离 | `chara_para1`: 特征参数1，`chara_para2`: 特征参数2 | 匹配距离（float） |
| `save_mfcc_chara_para` | 保存特征参数 | `chara_para`: 特征参数，`flash_addr`: Flash地址 | 无 |

### 7.2 音频采集API

| 函数名 | 功能描述 | 参数说明 | 返回值 |
|--------|----------|----------|--------|
| `I2S_Data_Get_Init` | 初始化I2S数据采集 | 无 | 无 |
| `voice_record` | 录音函数 | 无 | 无 |
| `DMA_For_I2S_Init` | 初始化I2S DMA传输 | 无 | 无 |

### 7.3 Flash操作API

| 函数名 | 功能描述 | 参数说明 |
|--------|----------|----------|
| `save_chara_para_mdl` | 保存特征参数模型 | `data`: 音频数据，`addr`: Flash地址 |
| `Check_Trained_Model` | 检查训练模型 | 无 |

### 7.4 配置常量

```c
// 采样参数
#define DataSampleFrq       8000    // 8kHz采样率
#define SampleTime          2000    // 2秒采样时间
#define SampleDataLen       16000   // 总样本数

// 识别参数
#define kw_num              4       // 关键词数量
#define charaml_per_kw      2       // 每个关键词样本数
#define MATCH_DIS_THRESHOLD 100.0f  // 匹配距离阈值

// 存储参数
#define size_per_chara      8192    // 每个特征8KB
#define charamls_size       65536   // 总存储64KB
```

## 8. 注意事项

1. **Flash寿命**：频繁训练会减少Flash寿命（约10万次擦写周期）
2. **环境噪声**：需要在相对安静的环境下训练和识别
3. **说话人依赖**：训练和识别应为同一说话人
4. **电源稳定性**：音频电路对电源噪声敏感
5. **内存使用**：确保足够的RAM空间（音频缓冲区32KB）
6. **实时性**：识别过程需要一定计算时间，考虑系统实时性要求
7. **模型更新**：更新训练模型时需先擦除Flash
8. **硬件兼容性**：不同麦克风可能需要调整增益参数

## 9. 性能优化建议

1. **算法优化**：
   - 使用定点数运算替代浮点运算
   - 优化MFCC计算，减少计算量
   - 使用查表法加速三角函数计算

2. **内存优化**：
   - 使用环形缓冲区减少内存占用
   - 压缩特征参数减少Flash使用
   - 动态分配内存，避免静态大数组

3. **速度优化**：
   - 降低采样率（如从8kHz降到6kHz）
   - 减少特征维度（如从12维降到8维）
   - 使用更快的匹配算法

4. **功耗优化**：
   - 在不进行识别时进入低功耗模式
   - 动态调整CPU频率
   - 优化外设使用，关闭不需要的外设

## 10. 典型应用场景

### 10.1 语音控制设备

```c
// 语音控制家电示例
void Voice_Control_Application(void)
{
    // 识别关键词
    uint8_t command = Recognize_Command();

    // 执行相应操作
    switch(command)
    {
        case CMD_UP:
            Increase_Temperature();
            break;
        case CMD_DOWN:
            Decrease_Temperature();
            break;
        case CMD_LEFT:
            Previous_Channel();
            break;
        case CMD_RIGHT:
            Next_Channel();
            break;
        default:
            // 未识别
            break;
    }
}
```

### 10.2 语音门禁系统

```c
// 语音门禁系统
void Voice_Access_Control(void)
{
    // 训练阶段：录入授权用户语音
    if(Is_Training_Mode())
    {
        Train_Authorized_User();
    }

    // 识别阶段：验证用户身份
    else
    {
        if(Verify_User_Identity())
        {
            Unlock_Door();
            printf("Access granted!\r\n");
        }
        else
        {
            printf("Access denied!\r\n");
        }
    }
}
```

### 10.3 语音交互玩具

```c
// 语音交互玩具
void Voice_Interactive_Toy(void)
{
    // 检测语音活动
    if(Detect_Voice_Activity())
    {
        // 进行语音识别
        uint8_t response = Process_Voice_Command();

        // 根据识别结果做出响应
        switch(response)
        {
            case RESPONSE_HAPPY:
                Play_Happy_Sound();
                Blink_LEDs();
                break;
            case RESPONSE_SAD:
                Play_Sad_Sound();
                break;
            // ... 其他响应
        }
    }
}
```

## 11. 总结

CH32V307的VoiceRcgExam语音识别示例展示了在资源受限的嵌入式系统上实现孤立词语音识别的完整方案。通过MFCC特征提取和模板匹配算法，实现了对4个关键词的识别功能。该示例支持ES8388音频编解码器和ADC两种输入方式，使用Flash存储训练模型，并提供了训练和识别两种操作模式。

开发时需要注意环境噪声的影响、Flash寿命管理、内存资源分配等关键环节。语音识别功能可以广泛应用于智能家居、语音控制、身份验证等各种嵌入式场景，为产品增加语音交互能力提供了可行的技术方案。
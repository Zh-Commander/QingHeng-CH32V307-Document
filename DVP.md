# CH32V307 DVP数字视频端口使用指南

## 1. 外设概述

DVP（Digital Video Port，数字视频端口）是CH32V307的摄像头接口外设，用于连接数字摄像头模块（如OV2640）。主要特性包括：

- **多种数据格式**：支持RGB565、JPEG等图像格式
- **双缓冲DMA**：支持双缓冲DMA传输，提高数据传输效率
- **灵活配置**：可配置图像分辨率、帧率、数据格式
- **中断支持**：提供多种中断源（帧开始、行完成、帧完成等）
- **硬件接口**：支持标准DVP接口时序

## 2. 关键代码片段

### 2.1 DVP基本初始化

```c
void DVP_Init(void)
{
    NVIC_InitTypeDef NVIC_InitStructure = {0};

    // 1. 使能DVP时钟
    RCC_AHBPeriphClockCmd(RCC_AHBPeriph_DVP, ENABLE);

    // 2. 清除数据模式掩码
    DVP->CR0 &= ~RB_DVP_MSK_DAT_MOD;

    // 3. 配置工作模式
#if (DVP_Work_Mode == RGB565_MODE)
    // RGB565模式配置
    /* VSYNC、HSYNC - High level active */
    DVP->CR0 |= RB_DVP_D8_MOD | RB_DVP_V_POLAR;
    DVP->CR1 &= ~((RB_DVP_ALL_CLR) | RB_DVP_RCV_CLR);
    DVP->ROW_NUM = RGB565_ROW_NUM;               // 行数（例如320）
    DVP->COL_NUM = RGB565_COL_NUM;               // 列数（例如480，列数×2）

    DVP->DMA_BUF0 = RGB565_DVPDMAaddr0;          // DMA缓冲区0地址
    DVP->DMA_BUF1 = RGB565_DVPDMAaddr1;          // DMA缓冲区1地址
#endif

#if (DVP_Work_Mode == JPEG_MODE)
    // JPEG模式配置
    DVP->CR0 |= RB_DVP_D8_MOD;
    DVP->CR1 &= ~((RB_DVP_ALL_CLR) | RB_DVP_RCV_CLR);
    DVP->ROW_NUM = JPEG_ROW_NUM;                 // 行数（例如768）
    DVP->COL_NUM = JPEG_COL_NUM;                 // 列数（例如1024）

    DVP->DMA_BUF0 = JPEG_DVPDMAaddr0;            // DMA缓冲区0地址
    DVP->DMA_BUF1 = JPEG_DVPDMAaddr1;            // DMA缓冲区1地址
#endif

    // 4. 设置帧捕获率（100%）
    DVP->CR1 &= ~RB_DVP_FCRC;
    DVP->CR1 |= DVP_RATE_100P;  // 100%帧率

    // 5. 使能中断
    DVP->IER |= RB_DVP_IE_STP_FRM;    // 帧开始中断
    DVP->IER |= RB_DVP_IE_FIFO_OV;    // FIFO溢出中断
    DVP->IER |= RB_DVP_IE_FRM_DONE;   // 帧完成中断
    DVP->IER |= RB_DVP_IE_ROW_DONE;   // 行完成中断
    DVP->IER |= RB_DVP_IE_STR_FRM;    // 帧开始中断（备用）

    // 6. 配置NVIC中断
    NVIC_InitStructure.NVIC_IRQChannel = DVP_IRQn;
    NVIC_InitStructure.NVIC_IRQChannelPreemptionPriority = 0;
    NVIC_InitStructure.NVIC_IRQChannelSubPriority = 0;
    NVIC_InitStructure.NVIC_IRQChannelCmd = ENABLE;
    NVIC_Init(&NVIC_InitStructure);

    // 7. 使能DMA
    DVP->CR1 |= RB_DVP_DMA_EN;

    // 8. 使能DVP
    DVP->CR0 |= RB_DVP_ENABLE;
}
```

### 2.2 DVP中断处理函数

```c
// DVP中断服务程序
void DVP_IRQHandler(void)
{
    // 1. 行完成中断处理
    if(DVP->IFR & RB_DVP_IF_ROW_DONE)
    {
        DVP->IFR &= ~RB_DVP_IF_ROW_DONE;  // 清除中断标志

#if (DVP_Work_Mode == RGB565_MODE)
        if(addr_cnt % 2)    // buf1完成
        {
            addr_cnt++;
            // 发送DVP数据到LCD
            DMA_Cmd(DMA2_Channel5, DISABLE);
            DMA_SetCurrDataCounter(DMA2_Channel5, lcddev.width);
            DMA2_Channel5->PADDR = RGB565_DVPDMAaddr0;  // 切换到缓冲区0
            DMA_Cmd(DMA2_Channel5, ENABLE);
        }
        else                // buf0完成
        {
            addr_cnt++;
            // 发送DVP数据到LCD
            DMA_Cmd(DMA2_Channel5, DISABLE);
            DMA_SetCurrDataCounter(DMA2_Channel5, lcddev.width);
            DMA2_Channel5->PADDR = RGB565_DVPDMAaddr1;  // 切换到缓冲区1
            DMA_Cmd(DMA2_Channel5, ENABLE);
        }
        href_cnt++;
#endif
    }

    // 2. 帧完成中断处理
    if(DVP->IFR & RB_DVP_IF_FRM_DONE)
    {
        DVP->IFR &= ~RB_DVP_IF_FRM_DONE;  // 清除中断标志

#if (DVP_Work_Mode == RGB565_MODE)
        addr_cnt = 0;
        href_cnt = 0;
        printf("Frame done\r\n");
#endif

#if (DVP_Work_Mode == JPEG_MODE)
        // JPEG模式下，一帧数据接收完成
        jpeg_frame_complete = 1;
#endif
    }

    // 3. FIFO溢出中断处理
    if(DVP->IFR & RB_DVP_IF_FIFO_OV)
    {
        DVP->IFR &= ~RB_DVP_IF_FIFO_OV;
        printf("FIFO overflow!\r\n");
        // 需要处理数据丢失情况
    }

    // 4. 帧开始中断处理
    if(DVP->IFR & RB_DVP_IF_STR_FRM)
    {
        DVP->IFR &= ~RB_DVP_IF_STR_FRM;
        printf("Frame start\r\n");
    }
}
```

### 2.3 摄像头参数定义（ov.h）

```c
// RGB565像素格式参数（320×240）
#define RGB565_ROW_NUM               320
#define RGB565_COL_NUM               480     // 列数×2（RGB565每个像素2字节）
#define OV2640_RGB565_HEIGHT         320
#define OV2640_RGB565_WIDTH          240

// JPEG像素格式参数（1024×768）
#define OV2640_JPEG_HEIGHT           768
#define OV2640_JPEG_WIDTH            1024

// 摄像头寄存器定义
#define OV2640_ID                    0x7FA2  // 摄像头ID

// 图像格式枚举
typedef enum {
    IMAGE_FORMAT_RGB565 = 0,
    IMAGE_FORMAT_JPEG,
    IMAGE_FORMAT_YUV,
    IMAGE_FORMAT_RAW
} ImageFormatTypeDef;

// 分辨率枚举
typedef enum {
    RESOLUTION_QVGA = 0,     // 320×240
    RESOLUTION_VGA,          // 640×480
    RESOLUTION_SVGA,         // 800×600
    RESOLUTION_XGA,          // 1024×768
    RESOLUTION_SXGA          // 1280×1024
} ResolutionTypeDef;
```

### 2.4 摄像头初始化函数

```c
// OV2640摄像头初始化
uint8_t OV2640_Init(void)
{
    uint16_t i = 0;
    uint16_t reg;

    // 1. 读取摄像头ID
    OV2640_ReadReg(OV2640_CHIPID_HIGH, &reg);
    i = reg << 8;
    OV2640_ReadReg(OV2640_CHIPID_LOW, &reg);
    i |= reg;

    if(i != OV2640_ID) {
        printf("OV2640 ID Error: 0x%04x\r\n", i);
        return 1; // 摄像头ID错误
    }

    printf("OV2640 ID OK: 0x%04x\r\n", i);

    // 2. 初始化摄像头寄存器序列
    for(i = 0; i < sizeof(ov2640_init_reg_tb1) / sizeof(ov2640_init_reg_tb1[0]); i++) {
        OV2640_WriteReg(ov2640_init_reg_tb1[i][0], ov2640_init_reg_tb1[i][1]);
    }

    // 3. 根据工作模式配置摄像头
#if (DVP_Work_Mode == RGB565_MODE)
    // RGB565模式配置
    for(i = 0; i < sizeof(ov2640_rgb565_reg_tb) / sizeof(ov2640_rgb565_reg_tb[0]); i++) {
        OV2640_WriteReg(ov2640_rgb565_reg_tb[i][0], ov2640_rgb565_reg_tb[i][1]);
    }
#endif

#if (DVP_Work_Mode == JPEG_MODE)
    // JPEG模式配置
    for(i = 0; i < sizeof(ov2640_jpeg_reg_tb) / sizeof(ov2640_jpeg_reg_tb[0]); i++) {
        OV2640_WriteReg(ov2640_jpeg_reg_tb[i][0], ov2640_jpeg_reg_tb[i][1]);
    }
#endif

    return 0; // 初始化成功
}
```

### 2.5 DMA缓冲区配置

```c
// DVP DMA缓冲区定义
#if (DVP_Work_Mode == RGB565_MODE)
    // RGB565模式缓冲区（320×240×2字节）
    uint8_t RGB565_DVPDMAaddr0[RGB565_ROW_NUM * 2] __attribute__ ((aligned(4)));
    uint8_t RGB565_DVPDMAaddr1[RGB565_ROW_NUM * 2] __attribute__ ((aligned(4)));

    // DMA传输缓冲区指针
    volatile uint32_t addr_cnt = 0;
    volatile uint32_t href_cnt = 0;
#endif

#if (DVP_Work_Mode == JPEG_MODE)
    // JPEG模式缓冲区（1024×768字节）
    uint8_t JPEG_DVPDMAaddr0[JPEG_ROW_NUM * JPEG_COL_NUM] __attribute__ ((aligned(4)));
    uint8_t JPEG_DVPDMAaddr1[JPEG_ROW_NUM * JPEG_COL_NUM] __attribute__ ((aligned(4)));

    // JPEG帧完成标志
    volatile uint8_t jpeg_frame_complete = 0;
    volatile uint32_t jpeg_data_length = 0;
#endif
```

## 3. 配置数据结构

### 3.1 DVP配置结构体

```c
// DVP配置结构体
typedef struct {
    uint16_t width;                     // 图像宽度
    uint16_t height;                    // 图像高度
    ImageFormatTypeDef format;          // 图像格式
    uint8_t frame_rate;                 // 帧率（百分比）
    uint8_t vsync_polarity;             // VSYNC极性
    uint8_t hsync_polarity;             // HSYNC极性
    uint8_t *dma_buffer0;               // DMA缓冲区0
    uint8_t *dma_buffer1;               // DMA缓冲区1
    uint32_t buffer_size;               // 缓冲区大小
} DVP_ConfigTypeDef;

// DVP中断配置结构体
typedef struct {
    uint8_t frame_start_enable;         // 帧开始中断使能
    uint8_t row_done_enable;            // 行完成中断使能
    uint8_t frame_done_enable;          // 帧完成中断使能
    uint8_t fifo_overflow_enable;       // FIFO溢出中断使能
    uint8_t interrupt_priority;         // 中断优先级
} DVP_InterruptConfigTypeDef;
```

### 3.2 摄像头配置结构体

```c
// 摄像头配置结构体
typedef struct {
    ResolutionTypeDef resolution;       // 分辨率
    ImageFormatTypeDef format;          // 图像格式
    uint8_t brightness;                 // 亮度
    uint8_t contrast;                   // 对比度
    uint8_t saturation;                 // 饱和度
    uint8_t sharpness;                  // 锐度
    uint8_t white_balance;              // 白平衡
    uint8_t exposure;                   // 曝光
} CameraConfigTypeDef;
```

## 4. 通用配置步骤

### 4.1 DVP配置步骤
1. **时钟使能**：
   ```c
   RCC_AHBPeriphClockCmd(RCC_AHBPeriph_DVP, ENABLE);
   ```

2. **GPIO配置**（DVP使用特定引脚，需要根据数据手册配置）：
   ```c
   // DVP引脚包括：D[0:7], VSYNC, HSYNC, PCLK等
   GPIO_InitStructure.GPIO_Pin = GPIO_Pin_0 | GPIO_Pin_1 | ... | GPIO_Pin_7;
   GPIO_InitStructure.GPIO_Mode = GPIO_Mode_IN_FLOATING;  // 输入模式
   GPIO_Init(GPIOx, &GPIO_InitStructure);
   ```

3. **DVP参数配置**：
   - 清除数据模式掩码
   - 配置数据格式（RGB565/JPEG）
   - 设置图像分辨率
   - 配置帧捕获率

4. **DMA缓冲区配置**：
   - 分配双缓冲DMA内存
   - 设置DMA缓冲区地址
   - 确保内存对齐（4字节对齐）

5. **中断配置**：
   - 使能需要的中断源
   - 配置NVIC中断优先级
   - 实现中断服务程序

6. **使能DVP**：
   ```c
   DVP->CR1 |= RB_DVP_DMA_EN;  // 使能DMA
   DVP->CR0 |= RB_DVP_ENABLE;  // 使能DVP
   ```

### 4.2 摄像头配置步骤
1. **摄像头初始化**：
   - 读取摄像头ID验证连接
   - 写入初始化寄存器序列
   - 根据图像格式配置摄像头

2. **图像参数设置**：
   - 设置分辨率
   - 设置图像格式
   - 调整图像质量参数（亮度、对比度等）

3. **开始捕获**：
   ```c
   OV2640_Start();  // 启动摄像头捕获
   ```

### 4.3 图像处理步骤
1. **RGB565模式**：
   - 通过DMA接收图像数据
   - 双缓冲切换处理
   - 直接显示到LCD或进行图像处理

2. **JPEG模式**：
   - 接收完整的JPEG帧
   - 保存到文件系统
   - 通过网络传输
   - 解码显示

## 5. 示例工程详解

### 5.1 DVP_TFTLCD示例（RGB565模式）
- **位置**：`DVP/DVP_TFTLCD/User/main.c`
- **功能**：DVP摄像头数据通过FSMC显示到TFT LCD
- **特点**：
  - RGB565图像格式
  - 实时显示摄像头画面
  - 使用FSMC接口的TFT LCD
  - 双缓冲DMA传输
- **适用场景**：
  - 实时视频监控
  - 视频对讲系统
  - 图像采集显示

### 5.2 DVP_UART示例（JPEG模式）
- **位置**：`DVP/DVP_UART/User/main.c`
- **功能**：DVP摄像头数据通过UART输出JPEG图像
- **特点**：
  - JPEG图像压缩格式
  - 通过串口传输图像数据
  - 支持图像质量调整
  - 帧完整性检查
- **适用场景**：
  - 无线图像传输
  - 远程监控
  - 图像存储
  - 网络摄像头

### 5.3 工作模式对比
| 特性 | RGB565模式 | JPEG模式 |
|------|------------|----------|
| 数据格式 | 原始RGB565 | JPEG压缩 |
| 数据量 | 较大（每个像素2字节） | 较小（压缩比可调） |
| 图像质量 | 无损 | 有损压缩 |
| 处理需求 | 可直接显示 | 需要解码显示 |
| 传输带宽 | 要求高 | 要求低 |
| 适用场景 | 本地显示 | 远程传输/存储 |

## 6. 常见问题

### Q1: 图像显示异常（颜色错误、花屏）？
A: 检查以下配置：
1. DVP数据格式配置是否正确
2. 摄像头输出格式是否匹配
3. DMA缓冲区大小是否足够
4. 内存对齐是否正确（需要4字节对齐）
5. 时序参数是否匹配

### Q2: 帧率不稳定或丢帧？
A: 可能原因：
1. 系统时钟频率不足
2. DMA传输带宽不够
3. 中断处理时间过长
4. 缓冲区切换不及时
5. 摄像头输出帧率不稳定

### Q3: FIFO溢出错误？
A: 处理方法：
1. 增加DMA缓冲区大小
2. 提高DMA传输优先级
3. 优化中断处理程序
4. 降低图像分辨率或帧率
5. 检查系统时钟配置

### Q4: 摄像头初始化失败？
A: 排查步骤：
1. 检查I2C总线连接
2. 验证摄像头ID
3. 检查电源供电
4. 确认寄存器配置序列正确
5. 检查时钟信号

### Q5: JPEG图像传输不完整？
A: 检查点：
1. 串口波特率是否足够
2. 帧完整性检查机制
3. 缓冲区溢出保护
4. 数据传输协议
5. 接收端处理能力

## 7. API参考

| 函数名 | 功能描述 | 参数说明 |
|--------|----------|----------|
| `DVP_Init` | DVP初始化 | 无 |
| `DVP_DeInit` | DVP反初始化 | 无 |
| `DVP_Cmd` | 使能/禁用DVP | NewState：新状态 |
| `DVP_DMACmd` | 使能/禁用DVP DMA | NewState：新状态 |
| `DVP_ITConfig` | 使能/禁用DVP中断 | DVP_IT：中断源，NewState：新状态 |
| `DVP_GetITStatus` | 获取中断状态 | DVP_IT：中断源 |
| `DVP_ClearITPendingBit` | 清除中断挂起位 | DVP_IT：中断源 |
| `OV2640_Init` | OV2640摄像头初始化 | 无 |
| `OV2640_Start` | 启动摄像头捕获 | 无 |
| `OV2640_Stop` | 停止摄像头捕获 | 无 |
| `OV2640_SetFormat` | 设置图像格式 | format：图像格式 |
| `OV2640_SetResolution` | 设置分辨率 | resolution：分辨率 |

## 8. 注意事项

1. **内存对齐**：DMA缓冲区需要4字节对齐：`__attribute__ ((aligned(4)))`
2. **时序匹配**：DVP时序需要与摄像头输出时序匹配
3. **电源管理**：摄像头模块需要稳定供电
4. **散热考虑**：高帧率运行时注意散热
5. **带宽计算**：合理计算所需带宽，避免系统过载
6. **中断优先级**：合理设置中断优先级，避免影响其他关键任务

## 9. 性能优化建议

1. **双缓冲优化**：使用双缓冲DMA减少数据拷贝
2. **内存管理**：合理分配内存，避免内存碎片
3. **中断优化**：中断服务程序尽量简短
4. **DMA优化**：合理设置DMA传输参数
5. **算法优化**：根据应用需求选择合适的图像处理算法

## 10. 典型应用场景

### 10.1 视频监控系统
```c
// 视频监控系统初始化
void VideoSurveillance_Init(void)
{
    // 1. 初始化摄像头
    OV2640_Init();
    OV2640_SetFormat(IMAGE_FORMAT_JPEG);
    OV2640_SetResolution(RESOLUTION_VGA);

    // 2. 初始化DVP
    DVP_Init();

    // 3. 初始化网络模块
    Network_Init();

    // 4. 启动视频捕获
    OV2640_Start();
}

// 视频帧处理
void VideoFrame_Process(void)
{
    if(jpeg_frame_complete) {
        // 压缩图像数据
        CompressImage(JPEG_DVPDMAaddr0, jpeg_data_length);

        // 通过网络传输
        SendVideoFrame(JPEG_DVPDMAaddr0, jpeg_data_length);

        // 重置标志
        jpeg_frame_complete = 0;
    }
}
```

### 10.2 图像识别系统
```c
// 图像识别系统初始化
void ImageRecognition_Init(void)
{
    // 1. 初始化摄像头（使用RGB565格式）
    OV2640_Init();
    OV2640_SetFormat(IMAGE_FORMAT_RGB565);
    OV2640_SetResolution(RESOLUTION_QVGA);

    // 2. 初始化DVP
    DVP_Init();

    // 3. 初始化图像处理算法
    ImageProcessing_Init();

    // 4. 启动图像捕获
    OV2640_Start();
}

// 图像处理线程
void ImageProcessing_Task(void)
{
    // 等待一帧数据完成
    while(!frame_ready) {
        rt_thread_delay(1);
    }

    // 图像预处理
    ImagePreprocess(RGB565_DVPDMAaddr0);

    // 特征提取
    ExtractFeatures();

    // 识别处理
    RecognitionProcess();

    // 显示结果
    DisplayResult();
}
```

### 10.3 条码扫描系统
```c
// 条码扫描系统
void BarcodeScanner_Init(void)
{
    // 1. 初始化摄像头
    OV2640_Init();
    OV2640_SetFormat(IMAGE_FORMAT_RGB565);
    OV2640_SetResolution(RESOLUTION_QVGA);

    // 2. 初始化DVP
    DVP_Init();

    // 3. 初始化条码识别库
    BarcodeRecognition_Init();

    // 4. 初始化提示音和指示灯
    Buzzer_Init();
    LED_Init();

    // 5. 启动扫描
    OV2640_Start();
}

// 条码识别处理
void Barcode_Process(void)
{
    // 获取一帧图像
    CaptureImage();

    // 条码检测
    if(DetectBarcode()) {
        // 解码条码
        DecodeBarcode();

        // 提示音和指示灯
        Buzzer_Beep();
        LED_Blink();

        // 输出结果
        DisplayBarcodeInfo();
    }
}
```

## 11. 总结

CH32V307的DVP外设为数字摄像头应用提供了完整的硬件支持。通过合理配置DVP参数和摄像头模块，可以实现高质量的图像采集和处理。DVP支持多种图像格式和工作模式，可以满足不同应用场景的需求。开发时需要注意时序匹配、内存管理和性能优化等关键环节，确保图像采集系统稳定可靠工作。
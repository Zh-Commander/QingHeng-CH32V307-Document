# CH32V307 USB外设使用指南

## 1. 外设概述

CH32V307提供两种USB控制器：
- **USBFS**：全速USB控制器，支持12Mbps速率
- **USBHS**：高速USB控制器，支持480Mbps速率

主要特性包括：
- **多种工作模式**：设备模式、主机模式、OTG模式
- **丰富的设备类支持**：HID、CDC、MSC、UAC、自定义类等
- **灵活的端点配置**：最多支持8个双向端点
- **高级功能**：DMA传输、双缓冲、CRC校验、低功耗模式
- **网络适配器支持**：ECM、NCM、RNDIS协议（仅USBHS）

## 2. USB目录结构

```
USB/
├── USBFS/                    # 全速USB控制器
│   ├── DEVICE/              # 设备模式示例
│   │   ├── CH372Device/              # 模拟CH372自定义设备
│   │   ├── CH372Device_Double_Buffering/ # 双缓冲CH372设备
│   │   ├── CompatibilityHID/          # 兼容性HID设备
│   │   ├── SimulateCDC/              # 虚拟串口（CDC类）
│   │   ├── SimulateCDC-HID/          # CDC+HID复合设备
│   │   ├── CompositeKM/              # 键盘鼠标复合设备
│   │   ├── MSC_U-Disk/               # U盘模拟设备（MSC类）
│   │   ├── MSC_CD-ROM/               # CD-ROM模拟设备
│   │   ├── UAC10_Microphone/         # UAC 1.0麦克风设备
│   │   └── UAC10_Headphone/          # UAC 1.0耳机设备
│   ├── HOST/                # 主机模式示例
│   │   ├── HOST_Udisk/              # U盘主机操作
│   │   ├── HOST_KM/                 # 键盘鼠标主机
│   │   ├── HOST_MTP_FileSystem/     # MTP文件系统主机
│   │   └── HOST_IAP/                # USB主机IAP（通过U盘升级）
│   └── Udisk_Lib/           # U盘操作库
└── USBHS/                    # 高速USB控制器
    ├── DEVICE/              # 设备模式示例
    │   ├── USB_ECM_NetWorkAdaptor/  # CDC-ECM网络适配器
    │   ├── USB_NCM_NetWorkAdaptor/  # CDC-NCM网络适配器
    │   └── USB_RNDIS_NetWorkAdaptor/ # RNDIS网络适配器
    └── HOST/                # 主机模式示例
        # 结构与USBFS类似
```

## 3. 关键代码片段

### 3.1 USB时钟配置

```c
void USBFS_RCC_Init(void)
{
    // 根据系统时钟配置USBFS时钟为48MHz
    if(SystemCoreClock == 144000000)
    {
        RCC_USBFSCLKConfig(RCC_USBFSCLKSource_PLLCLK_Div3);  // 144/3=48MHz
    }
    else if(SystemCoreClock == 96000000)
    {
        RCC_USBFSCLKConfig(RCC_USBFSCLKSource_PLLCLK_Div2);  // 96/2=48MHz
    }
    else if(SystemCoreClock == 48000000)
    {
        RCC_USBFSCLKConfig(RCC_USBFSCLKSource_PLLCLK_Div1);  // 48/1=48MHz
    }
    RCC_AHBPeriphClockCmd(RCC_AHBPeriph_USBFS, ENABLE);
}
```

### 3.2 USB设备初始化

```c
void USBFS_Device_Init(FunctionalState sta)
{
    if(sta)
    {
        // 复位USB控制器
        USBFSH->BASE_CTRL = USBFS_UC_RESET_SIE | USBFS_UC_CLR_ALL;
        Delay_Us(10);
        USBFSH->BASE_CTRL = 0x00;

        // 使能中断
        USBFSD->INT_EN = USBFS_UIE_SUSPEND | USBFS_UIE_BUS_RST | USBFS_UIE_TRANSFER;

        // 使能设备功能
        USBFSD->BASE_CTRL = USBFS_UC_DEV_PU_EN | USBFS_UC_INT_BUSY | USBFS_UC_DMA_EN;

        // 端点初始化
        USBFS_Device_Endp_Init();

        // 使能设备端口
        USBFSD->UDEV_CTRL = USBFS_UD_PD_DIS | USBFS_UD_PORT_EN;

        // 使能USB中断
        NVIC_EnableIRQ(USBFS_IRQn);
    }
}
```

### 3.3 端点配置

```c
void USBFS_Device_Endp_Init(void)
{
    // 端点模式配置（示例：端点1接收，端点2发送）
    USBFSD->UEP4_1_MOD = USBFS_UEP4_TX_EN | USBFS_UEP1_RX_EN;
    USBFSD->UEP2_3_MOD = USBFS_UEP2_TX_EN | USBFS_UEP3_RX_EN;
    USBFSD->UEP5_6_MOD = USBFS_UEP6_TX_EN | USBFS_UEP5_RX_EN;

    // DMA地址配置
    USBFSD->UEP0_DMA = (uint32_t)USBFS_EP0_Buf;
    USBFSD->UEP1_DMA = (uint32_t)Data_Buffer;
    USBFSD->UEP2_DMA = (uint32_t)Data_Buffer;

    // 端点控制寄存器配置
    USBFSD->UEP0_RX_CTRL = USBFS_UEP_R_RES_ACK;  // 端点0接收ACK
    USBFSD->UEP1_RX_CTRL = USBFS_UEP_R_RES_ACK;  // 端点1接收ACK
    USBFSD->UEP0_TX_CTRL = USBFS_UEP_T_RES_NAK;  // 端点0发送NAK
    USBFSD->UEP2_TX_CTRL = USBFS_UEP_T_RES_NAK;  // 端点2发送NAK
}
```

### 3.4 USB描述符配置（HID设备示例）

```c
// 设备描述符
const uint8_t MyDevDescr[] =
{
    0x12,       // bLength: 18字节
    0x01,       // bDescriptorType: 设备描述符
    0x10, 0x01, // bcdUSB: USB 1.10
    0x00,       // bDeviceClass: 由接口描述符定义
    0x00,       // bDeviceSubClass
    0x00,       // bDeviceProtocol
    DEF_USBD_UEP0_SIZE,   // bMaxPacketSize0: 64字节
    0x86, 0x1A, // idVendor: 0x1A86（沁恒）
    0x37, 0x55, // idProduct: 0x5537
    0x00, 0x02, // bcdDevice: 版本2.00
    0x01,       // iManufacturer: 厂商字符串索引
    0x02,       // iProduct: 产品字符串索引
    0x03,       // iSerialNumber: 序列号字符串索引
    0x01,       // bNumConfigurations: 1个配置
};

// HID报告描述符
const uint8_t HID_ReportDescr[] =
{
    0x05, 0x01,     // USAGE_PAGE (Generic Desktop)
    0x09, 0x06,     // USAGE (Keyboard)
    0xA1, 0x01,     // COLLECTION (Application)
    // ... 详细报告描述符
    0xC0            // END_COLLECTION
};
```

### 3.5 环形缓冲区结构

```c
#define DEF_Ring_Buffer_Max_Blks      16
#define DEF_RING_BUFFER_SIZE          (DEF_Ring_Buffer_Max_Blks * DEF_USBD_FS_PACK_SIZE)

typedef struct __PACKED _RING_BUFF_COMM
{
    volatile uint8_t LoadPtr;      // 加载指针
    volatile uint8_t DealPtr;      // 处理指针
    volatile uint8_t RemainPack;   // 剩余数据包数
    volatile uint8_t PackLen[DEF_Ring_Buffer_Max_Blks];  // 数据包长度数组
    volatile uint8_t StopFlag;     // 停止标志
} RING_BUFF_COMM, *pRING_BUFF_COMM;
```

### 3.6 USB中断处理

```c
void USBFS_IRQHandler(void) __attribute__((interrupt("WCH-Interrupt-fast")));
void USBFS_IRQHandler(void)
{
    uint8_t intflag = USBFSD->INT_FG;

    if(intflag & USBFS_UIF_TRANSFER)  // 传输完成中断
    {
        uint8_t ep_status = USBFSD->INT_ST & USBFS_UIS_TOKEN_MASK;
        uint8_t ep_num = (USBFSD->INT_ST & USBFS_UIS_ENDP_MASK) >> 4;

        switch(ep_status)
        {
            case USBFS_UIS_TOKEN_IN:  // IN传输完成
                // 处理发送完成
                break;

            case USBFS_UIS_TOKEN_OUT: // OUT传输完成
                // 处理接收完成
                uint8_t rx_len = USBFSD->RX_LEN;
                // 读取接收到的数据
                break;
        }
        USBFSD->INT_FG = USBFS_UIF_TRANSFER;  // 清除中断标志
    }

    if(intflag & USBFS_UIF_BUS_RST)  // 总线复位中断
    {
        USBFSD->INT_FG = USBFS_UIF_BUS_RST;  // 清除中断标志
        USBFS_Device_Endp_Init();  // 重新初始化端点
    }

    if(intflag & USBFS_UIF_SUSPEND)  // 挂起中断
    {
        USBFSD->INT_FG = USBFS_UIF_SUSPEND;  // 清除中断标志
        // 进入低功耗模式
    }
}
```

## 4. 通用配置步骤

### 4.1 USB设备模式配置步骤
1. **时钟配置**：配置USB时钟为48MHz
2. **GPIO配置**：配置DP/DM引脚
3. **描述符配置**：定义设备、配置、接口、端点描述符
4. **端点初始化**：配置端点模式、DMA地址、控制寄存器
5. **中断使能**：使能USB中断和NVIC中断
6. **设备使能**：使能USB设备物理层和端口

### 4.2 USB主机模式配置步骤
1. **时钟配置**：配置USB主机时钟
2. **主机初始化**：初始化USB主机控制器
3. **设备枚举**：检测连接设备，读取描述符
4. **驱动加载**：根据设备类加载相应驱动
5. **数据传输**：进行控制/批量/中断传输

## 5. 设备类详解

### 5.1 HID设备（人机接口设备）
- **特点**：无需驱动，即插即用，中断传输
- **应用场景**：键盘、鼠标、游戏手柄、自定义HID设备
- **关键配置**：HID报告描述符、中断端点

### 5.2 CDC设备（通信设备类）
- **特点**：虚拟串口，批量传输，兼容性好
- **应用场景**：USB转串口、调制解调器、数据采集
- **关键配置**：CDC类描述符、批量端点

### 5.3 MSC设备（大容量存储类）
- **特点**：U盘功能，大容量存储，文件系统支持
- **应用场景**：U盘、移动硬盘、数据记录仪
- **关键配置**：SCSI命令处理、块设备接口

### 5.4 UAC设备（音频设备类）
- **特点**：实时音频传输，同步端点
- **应用场景**：麦克风、扬声器、音频接口
- **关键配置**：音频描述符、同步端点、音频格式

### 5.5 网络适配器（仅USBHS）
- **ECM**：CDC-ECM兼容的10M/100M/1G以太网适配器
- **NCM**：CDC-NCM网络适配器
- **RNDIS**：RNDIS网络适配器（Windows兼容）

## 6. 示例工程详解

### 6.1 CH372Device（自定义设备）
- **位置**：`USB/USBFS/DEVICE/CH372Device`
- **功能**：模拟CH372芯片，支持端点1/3/5下行，端点2/4/6上行数据传输
- **特点**：环形缓冲区管理，批量传输

### 6.2 SimulateCDC（虚拟串口）
- **位置**：`USB/USBFS/DEVICE/SimulateCDC`
- **功能**：USB转串口设备，使用USART2(PA2/PA3)
- **特点**：CDC类协议实现，批量数据传输

### 6.3 CompositeKM（键盘鼠标复合设备）
- **位置**：`USB/USBFS/DEVICE/CompositeKM`
- **功能**：同时模拟键盘和鼠标的HID复合设备
- **特点**：GPIO按键扫描，USART2接收特定键盘数据

### 6.4 MSC_U-Disk（U盘模拟）
- **位置**：`USB/USBFS/DEVICE/MSC_U-Disk`
- **功能**：模拟USB大容量存储设备
- **存储介质**：支持SPI Flash或内部Flash
- **特点**：FAT文件系统，SCSI命令处理

### 6.5 HOST_Udisk（U盘主机）
- **位置**：`USB/USBFS/HOST_Udisk`
- **功能**：USB主机操作U盘
- **支持操作**：字节操作、扇区操作、文件夹创建、文件枚举、长文件名

### 6.6 USB_ECM_NetWorkAdaptor（网络适配器）
- **位置**：`USB/USBHS/DEVICE/USB_ECM_NetWorkAdaptor`
- **功能**：模拟CDC-ECM兼容的10M/100M/1G以太网适配器
- **特点**：支持Linux/Android/Unix系统，内置MAC+PHY

## 7. 常见问题

### Q1: USB时钟配置不正确？
A: USBFS时钟必须为48MHz，根据系统时钟选择合适的分频：
- 144MHz系统时钟：使用3分频
- 96MHz系统时钟：使用2分频
- 48MHz系统时钟：使用1分频

### Q2: 端点配置失败？
A: 检查以下配置：
1. 端点模式是否正确（TX/RX/双向）
2. DMA地址是否4字节对齐
3. 端点控制寄存器配置是否正确
4. 端点缓冲区大小是否足够

### Q3: 描述符配置错误？
A: 注意描述符的格式和顺序：
1. 设备描述符 → 配置描述符 → 接口描述符 → 端点描述符
2. 字符串描述符使用Unicode编码
3. 端点地址和属性需要正确配置

### Q4: 中断处理不工作？
A: 检查中断配置：
1. USB中断是否使能
2. NVIC中断优先级配置
3. 中断标志是否及时清除
4. 中断服务函数是否正确声明

### Q5: DMA传输失败？
A: 检查DMA配置：
1. DMA缓冲区是否4字节对齐
2. DMA通道是否与USB端点匹配
3. DMA传输方向是否正确
4. DMA传输完成标志是否正确处理

## 8. 注意事项

1. **内存对齐**：USB缓冲区需要4字节对齐：`__attribute__ ((aligned(4)))`
2. **中断优先级**：USB中断为快速中断，合理设置优先级
3. **电源管理**：支持USB挂起和唤醒功能，需配置唤醒源
4. **端点分配**：合理分配端点资源，避免冲突
5. **描述符大小**：确保描述符大小不超过端点缓冲区
6. **时钟稳定性**：USB时钟需要稳定，避免频繁切换

## 9. 性能优化建议

1. **使用双缓冲**：提高数据传输效率
2. **合理使用DMA**：大数据量传输使用DMA模式
3. **端点优化**：根据数据传输特点选择合适的端点模式
4. **缓冲区管理**：使用环形缓冲区提高数据吞吐量
5. **中断优化**：减少中断服务函数处理时间

## 10. 开发调试技巧

1. **使用USB分析仪**：监控USB通信数据包
2. **串口调试**：通过串口输出调试信息
3. **LED指示**：使用LED指示USB状态
4. **描述符检查工具**：使用USB描述符检查工具验证描述符
5. **逐步调试**：从简单的设备类开始，逐步增加功能

## 11. 总结

CH32V307的USB外设功能强大，支持多种工作模式和设备类。通过合理配置USB参数和端点资源，可以实现各种USB应用。开发时需要注意时钟配置、描述符定义、中断处理和DMA使用等关键环节。USBFS适用于全速应用，USBHS适用于高速应用和网络适配器功能。
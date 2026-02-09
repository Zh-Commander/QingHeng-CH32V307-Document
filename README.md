# 沁恒 CH32V307 开发文档仓库

![CH32V307](https://img.shields.io/badge/MCU-CH32V307-blue) ![RISC-V](https://img.shields.io/badge/Architecture-RISC--V-green) ![License](https://img.shields.io/badge/License-MIT-yellow)

本仓库包含沁恒微电子CH32V307系列RISC-V微控制器的完整开发资料，包括数据手册、外设驱动示例、开发工具指南、评估板文档等全方位资源。

## 📋 芯片概述

CH32V307是沁恒微电子推出的高性能RISC-V架构微控制器，主要特性如下：

- **内核**：青稞V4F处理器，支持单精度浮点指令集
- **主频**：最高144MHz，零等待运行
- **存储器**：256KB SRAM，128KB/256KB Flash
- **丰富外设**：
  - 8组USART串口、2组I2C、3组SPI
  - 4组电机PWM高级定时器
  - USB2.0高速PHY（480Mbps）
  - 千兆以太网MAC控制器
  - SDIO、DVP数字图像接口
  - 4组模拟运放、双ADC、双DAC
- **操作系统支持**：FreeRTOS、RT-Thread、TencentOS、HarmonyOS
- **开发工具**：MounRiver Studio、WCH-Link调试器

## 📁 仓库结构

```
QingHeng-CH32V307-Document/
├── API-Example/          # 外设驱动API示例和核心文档
├── Docs/                 # 开发工具和评估板文档
├── .gitattributes       # Git配置文件
├── LICENSE              # MIT许可证文件
└── README.md           # 本文件
```

### 1. API-Example 目录

包含48个Markdown文档，涵盖所有外设模块：

#### 外设驱动文档
- `ADC.md` - 模数转换器
- `CAN.md` - CAN总线控制器
- `DAC.md` - 数模转换器
- `DMA.md` - 直接内存访问
- `ETH.md` - 以太网控制器
- `GPIO.md` - 通用输入输出
- `I2C.md` - I2C总线接口
- `SPI.md` - SPI总线接口
- `USART.md` - 串口通信
- `USB.md` - USB控制器（全速/高速）
- `TIM.md` - 定时器
- `RTC.md` - 实时时钟

#### 系统功能文档
- `RCC.md` - 时钟与复位控制器
- `PWR.md` - 电源管理
- `FLASH.md` - Flash存储器操作
- `EXTI.md` - 外部中断
- `SYSTICK.md` - 系统滴答定时器

#### 高级功能文档
- `FPU.md` - 浮点运算单元
- `DVP.md` - 数字视频端口
- `SDIO.md` - SDIO接口
- `TOUCHKEY.md` - 触摸按键
- `VoiceRcgExam.md` - 语音识别示例

#### 操作系统支持
- `FreeRTOS.md` - FreeRTOS实时操作系统移植
- `RT-Thread.md` - RT-Thread实时操作系统移植
- `TencentOS.md` - 腾讯物联网操作系统
- `HarmonyOS.md` - 鸿蒙操作系统支持

#### 核心参考文档
- `CH32V307_DataSheet.md` - 芯片数据手册（199KB）
- `CH32FV2x_V3xRM_RegisterDocumentation.md` - 寄存器文档（1.3MB）
- `PRODUCT_GUIDE_ModelDescriptionManual.md` - 产品选型手册

### 2. Docs 目录

包含13个开发工具和评估板相关文档：

#### 评估板文档
- `CH32V30x评估板说明书.md` - 中文评估板说明书
- `CH32V30x Evaluation Board Reference-EN.md` - 英文评估板参考手册
- `CH32V30x_IAP使用说明.md` - 在应用编程使用说明

#### 开发工具文档
- `WCH-Link使用说明.md` - WCH-Link调试器中文使用说明
- `WCH-LinkUserManual-EN.md` - WCH-Link英文用户手册
- `MounRiver Studio使用说明书--逐飞科技 V1.6.md` - MounRiver Studio IDE使用说明
- `MounRiver_Help.md` - MounRiver帮助文档

#### 网络协议栈文档
- `WCHNET使用文档.md` - WCHNET协议栈使用文档
- `WCHNET Protocol Stack Library Application Note.md` - WCHNET协议栈库应用笔记
- `WCHNET IAP升级方案使用教程.md` - WCHNET IAP升级教程
- `WCHNET WEB配置说明.md` - WCHNET WEB配置说明
- `以太网例程使用注意事项.md` - 以太网例程注意事项

#### 学习资料
- `RISC-V-Reader-Chinese-v2p1.md` - RISC-V手册中文版（438KB）

## 🚀 快速开始

### 环境准备

1. **安装开发环境**
   - 下载并安装 [MounRiver Studio](http://www.mounriver.com/)
   - 或使用GCC编译工具链 + 任意IDE

2. **准备调试器**
   - 使用WCH-Link调试器（支持RISC-V和ARM双模式）
   - 确保驱动安装正确

3. **克隆本仓库**
   ```bash
   git clone https://github.com/[username]/QingHeng-CH32V307-Document.git
   cd QingHeng-CH32V307-Document
   ```

### 第一个示例：GPIO控制

查看 `API-Example/GPIO.md` 文档，了解基本的GPIO配置和使用方法：

```c
#include "ch32v30x.h"

void GPIO_Config(void) {
    GPIO_InitTypeDef GPIO_InitStructure;

    // 使能GPIOB时钟
    RCC_APB2PeriphClockCmd(RCC_APB2Periph_GPIOB, ENABLE);

    // 配置PB0为推挽输出
    GPIO_InitStructure.GPIO_Pin = GPIO_Pin_0;
    GPIO_InitStructure.GPIO_Mode = GPIO_Mode_Out_PP;
    GPIO_InitStructure.GPIO_Speed = GPIO_Speed_50MHz;
    GPIO_Init(GPIOB, &GPIO_InitStructure);
}

int main(void) {
    GPIO_Config();

    while(1) {
        GPIO_SetBits(GPIOB, GPIO_Pin_0);  // 置高
        Delay_Ms(500);
        GPIO_ResetBits(GPIOB, GPIO_Pin_0); // 置低
        Delay_Ms(500);
    }
}
```

### 编译与下载

1. 在MounRiver Studio中创建新工程
2. 添加相应的库文件和头文件
3. 编译工程
4. 连接WCH-Link调试器
5. 下载程序到CH32V307评估板

## 🔧 开发环境配置

### MounRiver Studio

MounRiver Studio是沁恒官方推荐的集成开发环境，特点如下：

- 基于Eclipse开发，界面友好
- 内置GCC编译工具链
- 支持代码补全、语法高亮
- 集成调试器支持
- 一键下载和调试功能

详细安装和使用说明请参考 `Docs/MounRiver Studio使用说明书--逐飞科技 V1.6.md`

### WCH-Link调试器

WCH-Link是沁恒推出的多功能调试器：

- **支持架构**：RISC-V和ARM双模式
- **连接方式**：SWD/JTAG接口
- **功能特性**：
  - 程序下载和调试
  - 串口通信功能
  - 电压检测
  - 固件升级

详细使用说明请参考 `Docs/WCH-Link使用说明.md`

## 📚 学习路径建议

### 初学者路径
1. 阅读 `CH32V307_DataSheet.md` 了解芯片基本特性
2. 查看 `GPIO.md` 学习最基本的IO控制
3. 学习 `USART.md` 掌握串口通信
4. 实践 `TIM.md` 中的定时器应用

### 进阶开发路径
1. 学习 `DMA.md` 提高数据传输效率
2. 掌握 `ETH.md` 或 `USB.md` 高级外设
3. 参考 `FreeRTOS.md` 实现多任务系统
4. 学习 `WCHNET使用文档.md` 进行网络开发

### 项目实战路径
1. 结合多种外设实现综合应用
2. 参考评估板文档进行硬件设计
3. 使用操作系统管理复杂任务
4. 实现IAP功能进行远程升级

## 🔗 资源链接

### 官方资源
- [沁恒微电子官网](http://www.wch.cn/)
- [MounRiver Studio下载](http://www.mounriver.com/)
- [WCH-Link工具下载](http://www.wch.cn/downloads/WCH-LinkUtility_ZIP.html)

### 社区资源
- [沁恒技术论坛](http://www.wch.cn/bbs/)
- [GitHub开源项目](https://github.com/search?q=CH32V307)
- [立创EDA元件库](https://lceda.cn/)

### 相关文档
- [RISC-V官方手册](https://riscv.org/technical/specifications/)
- [FreeRTOS文档](https://www.freertos.org/)
- [RT-Thread文档](https://www.rt-thread.org/)

## 🤝 贡献指南

欢迎提交Issue和Pull Request来完善本仓库：

1. Fork本仓库
2. 创建功能分支 (`git checkout -b feature/AmazingFeature`)
3. 提交更改 (`git commit -m 'Add some AmazingFeature'`)
4. 推送到分支 (`git push origin feature/AmazingFeature`)
5. 开启Pull Request

### 文档规范
- 使用Markdown格式编写
- 代码示例应完整且可运行
- 图片资源放在相应目录的`images/`文件夹中
- 保持中英文文档同步更新

## 📄 许可证

本仓库内容遵循MIT许可证，详见 [LICENSE](LICENSE) 文件。

## 📞 支持与反馈

如有问题或建议，可通过以下方式联系：

- 提交GitHub Issue
- 访问沁恒官方技术支持
- 参与技术论坛讨论

---

**Happy Coding!** 🚀

*最后更新：2026年2月*


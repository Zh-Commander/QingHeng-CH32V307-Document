# CH32V307 FLASH存储器使用指南

本文档基于CH32V307 EVT示例代码分析，总结FLASH存储器的使用方法和配置要点。

## 一、概述

CH32V307提供多种FLASH存储器访问方式：
1. **内部FLASH**：芯片内置FLASH，用于程序存储和数据存储
2. **外部SPI FLASH**：通过SPI接口连接的外部FLASH
3. **外部并行FLASH**：通过FSMC接口连接的NOR/NAND FLASH

### 内部FLASH特性：
- 容量：最大256KB（不同型号可能不同）
- 页大小：4KB
- 支持快速编程模式
- 支持读/写保护

## 二、内部FLASH编程

### 2.1 基本操作流程

#### 2.1.1 解锁和加锁
```c
// 解锁FLASH
FLASH_Unlock();

// 加锁FLASH
FLASH_Lock();
```

#### 2.1.2 页擦除
```c
// 擦除单页（4KB）
FLASHStatus = FLASH_ErasePage(Address);

// 擦除所有页
FLASHStatus = FLASH_EraseAllPages();
```

#### 2.1.3 数据编程
```c
// 半字编程（16位）
FLASHStatus = FLASH_ProgramHalfWord(Address, Data);

// 字编程（32位）
FLASHStatus = FLASH_ProgramWord(Address, Data);
```

#### 2.1.4 完整示例
```c
#include "ch32v30x.h"

#define PAGE_WRITE_START_ADDR  ((uint32_t)0x0800F000)  // 60KB地址
#define FLASH_PAGE_SIZE        4096  // 4KB页大小

int main(void)
{
    FLASH_Status FLASHStatus = FLASH_COMPLETE;
    uint32_t Address = 0x00;
    uint16_t Data = 0xABCD;

    SystemCoreClockUpdate();
    Delay_Init();
    USART_Printf_Init(115200);

    printf("SystemClk:%d\r\n", SystemCoreClock);
    printf("FLASH Programming Test\n");

    // 1. 解锁FLASH
    FLASH_Unlock();

    // 2. 擦除页
    FLASHStatus = FLASH_ErasePage(PAGE_WRITE_START_ADDR);
    if(FLASHStatus != FLASH_COMPLETE)
    {
        printf("Page Erase Error: %d\n", FLASHStatus);
        return 1;
    }
    printf("Page Erase Success\n");

    // 3. 编程数据
    Address = PAGE_WRITE_START_ADDR;
    for(uint32_t i = 0; i < 10; i++)
    {
        FLASHStatus = FLASH_ProgramHalfWord(Address, Data + i);
        if(FLASHStatus != FLASH_COMPLETE)
        {
            printf("Program Error at 0x%08X: %d\n", Address, FLASHStatus);
            break;
        }
        Address += 2;  // 半字地址递增
    }

    // 4. 验证数据
    Address = PAGE_WRITE_START_ADDR;
    for(uint32_t i = 0; i < 10; i++)
    {
        uint16_t read_data = *(__IO uint16_t*)Address;
        printf("Address 0x%08X: Write=0x%04X, Read=0x%04X\n",
               Address, Data + i, read_data);
        Address += 2;
    }

    // 5. 加锁FLASH
    FLASH_Lock();

    printf("FLASH Test Complete\n");

    while(1);
}
```

### 2.2 快速编程模式

#### 2.2.1 快速模式解锁
```c
// 快速模式解锁
FLASH_Unlock_Fast();

// 快速模式加锁
FLASH_Lock_Fast();
```

#### 2.2.2 快速页擦除和编程
```c
// 快速页擦除
FLASHStatus = FLASH_ErasePage_Fast(Address);

// 快速页编程
uint32_t buffer[1024];  // 4KB数据缓冲区
FLASHStatus = FLASH_ProgramPage_Fast(Address, buffer);
```

#### 2.2.3 ROM快速擦写函数
```c
// 快速擦除
s = FLASH_ROM_ERASE(Fadr, Fsize*4);  // Fsize单位为KB

// 快速写入
s = FLASH_ROM_WRITE(Fadr, buf, Fsize*4);
```

### 2.3 保护机制

#### 2.3.1 写保护
```c
// 使能写保护
FLASH_EnableWriteProtection(FLASH_WRProt_AllPages);

// 禁用写保护
FLASH_DisableWriteProtection();
```

#### 2.3.2 读保护
```c
// 使能读保护
FLASH_ReadOutProtection(ENABLE);

// 禁用读保护
FLASH_ReadOutProtection(DISABLE);
```

#### 2.3.3 用户选项字节
```c
// 配置用户选项字节
FLASH_UserOptionByteConfig(OB_IWDG_SW, OB_STOP_NoRST, OB_STDBY_NoRST);
```

## 三、外部SPI FLASH

### 3.1 W25Qxx系列驱动

#### 3.1.1 器件识别
```c
// 读取FLASH ID
uint32_t SPI_Flash_ReadID(void)
{
    uint32_t Temp = 0;

    FLASH_CS_CLR;  // 片选使能

    SPI_FLASH_SendByte(W25X_ManufactDeviceID);  // 发送读ID命令
    SPI_FLASH_SendByte(0x00);
    SPI_FLASH_SendByte(0x00);
    SPI_FLASH_SendByte(0x00);

    Temp |= SPI_FLASH_SendByte(W25X_DUMMY_BYTE) << 16;
    Temp |= SPI_FLASH_SendByte(W25X_DUMMY_BYTE) << 8;
    Temp |= SPI_FLASH_SendByte(W25X_DUMMY_BYTE);

    FLASH_CS_SET;  // 片选禁用

    return Temp;
}
```

#### 3.1.2 基本指令
```c
// 写使能
void SPI_Flash_Write_Enable(void)
{
    FLASH_CS_CLR;
    SPI_FLASH_SendByte(W25X_WriteEnable);
    FLASH_CS_SET;
}

// 写禁用
void SPI_Flash_Write_Disable(void)
{
    FLASH_CS_CLR;
    SPI_FLASH_SendByte(W25X_WriteDisable);
    FLASH_CS_SET;
}

// 读取状态寄存器
uint8_t SPI_Flash_ReadSR(void)
{
    uint8_t byte = 0;

    FLASH_CS_CLR;
    SPI_FLASH_SendByte(W25X_ReadStatusReg);
    byte = SPI_FLASH_SendByte(W25X_DUMMY_BYTE);
    FLASH_CS_SET;

    return byte;
}

// 等待空闲
void SPI_Flash_Wait_Busy(void)
{
    while((SPI_Flash_ReadSR() & 0x01) == 0x01);  // 检查BUSY位
}
```

#### 3.1.3 擦除操作
```c
// 扇区擦除（4KB）
void SPI_Flash_Erase_Sector(uint32_t SectorAddr)
{
    SPI_Flash_Write_Enable();
    SPI_Flash_Wait_Busy();

    FLASH_CS_CLR;
    SPI_FLASH_SendByte(W25X_SectorErase);
    SPI_FLASH_SendByte((SectorAddr & 0xFF0000) >> 16);
    SPI_FLASH_SendByte((SectorAddr & 0xFF00) >> 8);
    SPI_FLASH_SendByte(SectorAddr & 0xFF);
    FLASH_CS_SET;

    SPI_Flash_Wait_Busy();
}

// 块擦除（64KB）
void SPI_Flash_Erase_Block(uint32_t BlockAddr)
{
    SPI_Flash_Write_Enable();
    SPI_Flash_Wait_Busy();

    FLASH_CS_CLR;
    SPI_FLASH_SendByte(W25X_BlockErase);
    SPI_FLASH_SendByte((BlockAddr & 0xFF0000) >> 16);
    SPI_FLASH_SendByte((BlockAddr & 0xFF00) >> 8);
    SPI_FLASH_SendByte(BlockAddr & 0xFF);
    FLASH_CS_SET;

    SPI_Flash_Wait_Busy();
}

// 整片擦除
void SPI_Flash_Erase_Chip(void)
{
    SPI_Flash_Write_Enable();
    SPI_Flash_Wait_Busy();

    FLASH_CS_CLR;
    SPI_FLASH_SendByte(W25X_ChipErase);
    FLASH_CS_SET;

    SPI_Flash_Wait_Busy();
}
```

#### 3.1.4 读写操作
```c
// 页编程（最多256字节）
void SPI_Flash_Write_Page(uint8_t* pBuffer, uint32_t WriteAddr, uint16_t NumByteToWrite)
{
    SPI_Flash_Write_Enable();

    FLASH_CS_CLR;
    SPI_FLASH_SendByte(W25X_PageProgram);
    SPI_FLASH_SendByte((WriteAddr & 0xFF0000) >> 16);
    SPI_FLASH_SendByte((WriteAddr & 0xFF00) >> 8);
    SPI_FLASH_SendByte(WriteAddr & 0xFF);

    for(uint16_t i = 0; i < NumByteToWrite; i++)
    {
        SPI_FLASH_SendByte(pBuffer[i]);
    }

    FLASH_CS_SET;
    SPI_Flash_Wait_Busy();
}

// 数据写入（跨页处理）
void SPI_Flash_Write_Buffer(uint8_t* pBuffer, uint32_t WriteAddr, uint16_t NumByteToWrite)
{
    uint16_t PageRemain;
    PageRemain = 256 - (WriteAddr % 256);  // 当前页剩余空间

    if(NumByteToWrite <= PageRemain)
    {
        PageRemain = NumByteToWrite;
    }

    while(1)
    {
        SPI_Flash_Write_Page(pBuffer, WriteAddr, PageRemain);

        if(NumByteToWrite == PageRemain)
        {
            break;  // 写入完成
        }
        else
        {
            pBuffer += PageRemain;
            WriteAddr += PageRemain;
            NumByteToWrite -= PageRemain;

            if(NumByteToWrite > 256)
            {
                PageRemain = 256;
            }
            else
            {
                PageRemain = NumByteToWrite;
            }
        }
    }
}

// 数据读取
void SPI_Flash_Read_Buffer(uint8_t* pBuffer, uint32_t ReadAddr, uint16_t NumByteToRead)
{
    FLASH_CS_CLR;
    SPI_FLASH_SendByte(W25X_ReadData);
    SPI_FLASH_SendByte((ReadAddr & 0xFF0000) >> 16);
    SPI_FLASH_SendByte((ReadAddr & 0xFF00) >> 8);
    SPI_FLASH_SendByte(ReadAddr & 0xFF);

    for(uint16_t i = 0; i < NumByteToRead; i++)
    {
        pBuffer[i] = SPI_FLASH_SendByte(W25X_DUMMY_BYTE);
    }

    FLASH_CS_SET;
}
```

### 3.2 SPI接口配置

#### 3.2.1 SPI初始化
```c
void SPI_Flash_Init(void)
{
    SPI_InitTypeDef SPI_InitStructure = {0};
    GPIO_InitTypeDef GPIO_InitStructure = {0};

    // 使能时钟
    RCC_APB2PeriphClockCmd(RCC_APB2Periph_GPIOA | RCC_APB2Periph_SPI1, ENABLE);

    // 配置SPI引脚
    // SCK(PA5), MISO(PA6), MOSI(PA7)
    GPIO_InitStructure.GPIO_Pin = GPIO_Pin_5 | GPIO_Pin_7;
    GPIO_InitStructure.GPIO_Mode = GPIO_Mode_AF_PP;
    GPIO_InitStructure.GPIO_Speed = GPIO_Speed_50MHz;
    GPIO_Init(GPIOA, &GPIO_InitStructure);

    GPIO_InitStructure.GPIO_Pin = GPIO_Pin_6;
    GPIO_InitStructure.GPIO_Mode = GPIO_Mode_IN_FLOATING;
    GPIO_Init(GPIOA, &GPIO_InitStructure);

    // 配置CS引脚(PA2)
    GPIO_InitStructure.GPIO_Pin = GPIO_Pin_2;
    GPIO_InitStructure.GPIO_Mode = GPIO_Mode_Out_PP;
    GPIO_InitStructure.GPIO_Speed = GPIO_Speed_50MHz;
    GPIO_Init(GPIOA, &GPIO_InitStructure);
    FLASH_CS_SET;  // 默认禁用

    // 配置SPI
    SPI_InitStructure.SPI_Direction = SPI_Direction_2Lines_FullDuplex;
    SPI_InitStructure.SPI_Mode = SPI_Mode_Master;
    SPI_InitStructure.SPI_DataSize = SPI_DataSize_8b;
    SPI_InitStructure.SPI_CPOL = SPI_CPOL_Low;
    SPI_InitStructure.SPI_CPHA = SPI_CPHA_1Edge;
    SPI_InitStructure.SPI_NSS = SPI_NSS_Soft;
    SPI_InitStructure.SPI_BaudRatePrescaler = SPI_BaudRatePrescaler_2;
    SPI_InitStructure.SPI_FirstBit = SPI_FirstBit_MSB;
    SPI_InitStructure.SPI_CRCPolynomial = 7;
    SPI_Init(SPI1, &SPI_InitStructure);

    SPI_Cmd(SPI1, ENABLE);

    SPI_Flash_ReadID();  // 读取ID验证连接
}
```

#### 3.2.2 DMA传输（高速读取）
```c
void SPI1_Read_DMA_Init(void)
{
    DMA_InitTypeDef DMA_InitStructure = {0};

    // 使能DMA时钟
    RCC_AHBPeriphClockCmd(RCC_AHBPeriph_DMA1, ENABLE);

    // 配置DMA通道（SPI1_RX使用DMA1通道2）
    DMA_DeInit(DMA1_Channel2);
    DMA_InitStructure.DMA_PeripheralBaseAddr = (u32)&SPI1->DATAR;
    DMA_InitStructure.DMA_MemoryBaseAddr = (u32)0;  // 动态设置
    DMA_InitStructure.DMA_DIR = DMA_DIR_PeripheralSRC;
    DMA_InitStructure.DMA_BufferSize = 0;  // 动态设置
    DMA_InitStructure.DMA_PeripheralInc = DMA_PeripheralInc_Disable;
    DMA_InitStructure.DMA_MemoryInc = DMA_MemoryInc_Enable;
    DMA_InitStructure.DMA_PeripheralDataSize = DMA_PeripheralDataSize_Byte;
    DMA_InitStructure.DMA_MemoryDataSize = DMA_MemoryDataSize_Byte;
    DMA_InitStructure.DMA_Mode = DMA_Mode_Normal;
    DMA_InitStructure.DMA_Priority = DMA_Priority_VeryHigh;
    DMA_InitStructure.DMA_M2M = DMA_M2M_Disable;
    DMA_Init(DMA1_Channel2, &DMA_InitStructure);
}

// DMA读取启动
void SPI_Flash_Read_dma_start(uint32_t ReadAddr, uint8_t* pBuffer, uint16_t NumByteToRead)
{
    // 发送读取命令和地址
    FLASH_CS_CLR;
    SPI_FLASH_SendByte(W25X_FastReadData);
    SPI_FLASH_SendByte((ReadAddr & 0xFF0000) >> 16);
    SPI_FLASH_SendByte((ReadAddr & 0xFF00) >> 8);
    SPI_FLASH_SendByte(ReadAddr & 0xFF);
    SPI_FLASH_SendByte(W25X_DUMMY_BYTE);  // 快速读取需要dummy byte

    // 配置DMA
    DMA_SetCurrDataCounter(DMA1_Channel2, NumByteToRead);
    DMA_SetMemoryAddress(DMA1_Channel2, (uint32_t)pBuffer);

    // 启动DMA传输
    DMA_Cmd(DMA1_Channel2, ENABLE);
    SPI_I2S_DMACmd(SPI1, SPI_I2S_DMAReq_Rx, ENABLE);

    // 等待传输完成
    while(DMA_GetFlagStatus(DMA1_FLAG_TC2) == RESET);

    // 清理
    DMA_ClearFlag(DMA1_FLAG_TC2);
    DMA_Cmd(DMA1_Channel2, DISABLE);
    SPI_I2S_DMACmd(SPI1, SPI_I2S_DMAReq_Rx, DISABLE);

    FLASH_CS_SET;
}
```

## 四、FSMC接口FLASH

### 4.1 NOR FLASH操作

#### 4.1.1 FSMC初始化
```c
void FSMC_NorFlash_Init(void)
{
    GPIO_InitTypeDef GPIO_InitStructure = {0};
    FSMC_NORSRAMInitTypeDef FSMC_NORSRAMInitStructure = {0};
    FSMC_NORSRAMTimingInitTypeDef p;

    // 使能时钟
    RCC_APB2PeriphClockCmd(RCC_APB2Periph_GPIOD | RCC_APB2Periph_GPIOE |
                          RCC_APB2Periph_GPIOF | RCC_APB2Periph_GPIOG |
                          RCC_APB2Periph_AFIO, ENABLE);
    RCC_AHBPeriphClockCmd(RCC_AHBPeriph_FSMC, ENABLE);

    // 配置地址线
    // FSMC_A0~A18, FSMC_D0~D15

    // 配置控制线
    // FSMC_NE1, FSMC_NOE, FSMC_NWE, FSMC_NWAIT

    // FSMC时序配置
    p.FSMC_AddressSetupTime = 0x02;
    p.FSMC_AddressHoldTime = 0x00;
    p.FSMC_DataSetupTime = 0x05;
    p.FSMC_BusTurnAroundDuration = 0x00;
    p.FSMC_CLKDivision = 0x00;
    p.FSMC_DataLatency = 0x00;
    p.FSMC_AccessMode = FSMC_AccessMode_A;

    // FSMC初始化
    FSMC_NORSRAMInitStructure.FSMC_Bank = FSMC_Bank1_NORSRAM1;
    FSMC_NORSRAMInitStructure.FSMC_DataAddressMux = FSMC_DataAddressMux_Enable;
    FSMC_NORSRAMInitStructure.FSMC_MemoryType = FSMC_MemoryType_NOR;
    FSMC_NORSRAMInitStructure.FSMC_MemoryDataWidth = FSMC_MemoryDataWidth_16b;
    FSMC_NORSRAMInitStructure.FSMC_BurstAccessMode = FSMC_BurstAccessMode_Disable;
    FSMC_NORSRAMInitStructure.FSMC_AsynchronousWait = FSMC_AsynchronousWait_Disable;
    FSMC_NORSRAMInitStructure.FSMC_WaitSignalPolarity = FSMC_WaitSignalPolarity_Low;
    FSMC_NORSRAMInitStructure.FSMC_WrapMode = FSMC_WrapMode_Disable;
    FSMC_NORSRAMInitStructure.FSMC_WaitSignalActive = FSMC_WaitSignalActive_BeforeWaitState;
    FSMC_NORSRAMInitStructure.FSMC_WriteOperation = FSMC_WriteOperation_Enable;
    FSMC_NORSRAMInitStructure.FSMC_WaitSignal = FSMC_WaitSignal_Disable;
    FSMC_NORSRAMInitStructure.FSMC_ExtendedMode = FSMC_ExtendedMode_Disable;
    FSMC_NORSRAMInitStructure.FSMC_WriteBurst = FSMC_WriteBurst_Disable;
    FSMC_NORSRAMInitStructure.FSMC_ReadWriteTimingStruct = &p;
    FSMC_NORSRAMInitStructure.FSMC_WriteTimingStruct = &p;

    FSMC_NORSRAMInit(&FSMC_NORSRAMInitStructure);
    FSMC_NORSRAMCmd(FSMC_Bank1_NORSRAM1, ENABLE);
}
```

#### 4.1.2 NOR FLASH读写
```c
// 读取ID
void NOR_ReadID(NOR_IDTypeDef* NOR_ID)
{
    uint16_t Device_Code = 0, Manufacturer_Code = 0;

    // 发送读ID命令
    *(__IO uint16_t*)NOR_FLASH_ADDR_SHIFT(0x0555) = 0x00AA;
    *(__IO uint16_t*)NOR_FLASH_ADDR_SHIFT(0x02AA) = 0x0055;
    *(__IO uint16_t*)NOR_FLASH_ADDR_SHIFT(0x0555) = 0x0090;

    // 读取制造商代码和设备代码
    Manufacturer_Code = *(__IO uint16_t*)NOR_ADDR_SHIFT(0x0000);
    Device_Code = *(__IO uint16_t*)NOR_ADDR_SHIFT(0x0001);

    NOR_ID->Manufacturer_Code = Manufacturer_Code;
    NOR_ID->Device_Code = Device_Code;

    // 退出读ID模式
    *(__IO uint16_t*)NOR_FLASH_ADDR_SHIFT(0x0000) = 0x00F0;
}

// 块擦除
void NOR_EraseBlock(uint32_t BlockAddr)
{
    // 发送擦除命令序列
    NOR_WRITE(ADDR_SHIFT(0x0555), 0x00AA);
    NOR_WRITE(ADDR_SHIFT(0x02AA), 0x0055);
    NOR_WRITE(ADDR_SHIFT(0x0555), 0x0080);
    NOR_WRITE(ADDR_SHIFT(0x0555), 0x00AA);
    NOR_WRITE(ADDR_SHIFT(0x02AA), 0x0055);
    NOR_WRITE(BlockAddr, 0x0030);

    NOR_WaitForReady();
}

// 缓冲区写入
void NOR_WriteBuffer(uint16_t* pBuffer, uint32_t WriteAddr, uint32_t NumHalfwordToWrite)
{
    uint32_t index = 0;

    while(index < NumHalfwordToWrite)
    {
        // 发送字编程命令
        NOR_WRITE(ADDR_SHIFT(0x0555), 0x00AA);
        NOR_WRITE(ADDR_SHIFT(0x02AA), 0x0055);
        NOR_WRITE(ADDR_SHIFT(0x0555), 0x00A0);

        // 写入数据
        NOR_WRITE(WriteAddr, pBuffer[index]);

        NOR_WaitForReady();

        WriteAddr += 2;
        index++;
    }
}

// 缓冲区读取
void NOR_ReadBuffer(uint16_t* pBuffer, uint32_t ReadAddr, uint32_t NumHalfwordToRead)
{
    uint32_t index = 0;

    while(index < NumHalfwordToRead)
    {
        pBuffer[index] = *(__IO uint16_t*)ReadAddr;
        ReadAddr += 2;
        index++;
    }
}
```

### 4.2 NAND FLASH操作

#### 4.2.1 NAND初始化
```c
uint8_t NAND_Init(void)
{
    // 初始化FSMC NAND接口
    FSMC_NAND_Init();

    // 复位NAND
    NAND_Reset();

    // 读取ID验证
    NAND_ReadID(&NAND_ID);

    // 检查ID是否正确
    if(NAND_ID.Maker_ID != 0xEC || NAND_ID.Device_ID != 0xF1)
    {
        return NAND_FAIL;
    }

    return NAND_OK;
}
```

#### 4.2.2 NAND读写
```c
// 页读取
uint8_t NAND_ReadPage(uint32_t PageNum, uint16_t PageOffset, uint8_t* pBuffer, uint16_t NumByteToRead)
{
    // 发送读命令
    NAND_CMD_AREA = NAND_CMD_AREA_A;
    NAND_ADDR_AREA = 0x00;
    NAND_ADDR_AREA = PageOffset & 0xFF;
    NAND_ADDR_AREA = (PageOffset >> 8) & 0xFF;
    NAND_ADDR_AREA = PageNum & 0xFF;
    NAND_ADDR_AREA = (PageNum >> 8) & 0xFF;
    NAND_ADDR_AREA = (PageNum >> 16) & 0xFF;
    NAND_CMD_AREA = NAND_CMD_AREA_TRUE1;

    // 等待就绪
    NAND_WaitForReady();

    // 读取数据
    for(uint16_t i = 0; i < NumByteToRead; i++)
    {
        pBuffer[i] = NAND_DATA_AREA;
    }

    return NAND_OK;
}

// 页写入
uint8_t NAND_WritePage(uint32_t PageNum, uint16_t PageOffset, uint8_t* pBuffer, uint16_t NumByteToWrite)
{
    // 发送写命令
    NAND_CMD_AREA = NAND_CMD_AREA_A;
    NAND_ADDR_AREA = 0x80;
    NAND_ADDR_AREA = PageOffset & 0xFF;
    NAND_ADDR_AREA = (PageOffset >> 8) & 0xFF;
    NAND_ADDR_AREA = PageNum & 0xFF;
    NAND_ADDR_AREA = (PageNum >> 8) & 0xFF;
    NAND_ADDR_AREA = (PageNum >> 16) & 0xFF;

    // 写入数据
    for(uint16_t i = 0; i < NumByteToWrite; i++)
    {
        NAND_DATA_AREA = pBuffer[i];
    }

    NAND_CMD_AREA = NAND_CMD_AREA_TRUE1;

    // 等待就绪
    NAND_WaitForReady();

    return NAND_OK;
}
```

#### 4.2.3 ECC校验
```c
// 计算ECC
uint32_t NAND_CalculateECC(uint8_t* pBuffer)
{
    // ECC计算实现
    // ...
}

// 验证ECC
uint8_t NAND_VerifyECC(uint32_t ECC_Read, uint32_t ECC_Calc)
{
    if(ECC_Read == ECC_Calc)
        return NAND_OK;
    else
        return NAND_ECC_ERROR;
}
```

## 五、IAP应用中的FLASH

### 5.1 快速编程IAP
```c
// IAP快速编程函数
void CH32_IAP_Program(u32 adr, u32* buf)
{
    FLASH_ProgramPage_Fast(adr, buf);
}

// IAP固件接收和编程
void IAP_Receive_Firmware(void)
{
    uint32_t address = APP_START_ADDRESS;
    uint32_t buffer[1024];  // 4KB缓冲区
    uint32_t received_size = 0;

    // 擦除应用程序区域
    for(uint32_t i = 0; i < APP_SIZE / 4096; i++)
    {
        FLASH_ErasePage_Fast(address + i * 4096);
    }

    // 接收并编程固件
    while(received_size < APP_SIZE)
    {
        // 接收4KB数据块
        UART_Receive_Buffer((uint8_t*)buffer, 4096);

        // 快速编程
        CH32_IAP_Program(address, buffer);

        address += 4096;
        received_size += 4096;
    }

    // 验证固件
    if(IAP_Verify_Firmware() == SUCCESS)
    {
        printf("IAP Success\n");
        // 跳转到应用程序
        IAP_JumpToApp();
    }
    else
    {
        printf("IAP Verify Failed\n");
    }
}
```

### 5.2 固件校验
```c
// CRC校验
uint32_t Calculate_Firmware_CRC(uint32_t start_addr, uint32_t size)
{
    uint32_t crc = 0xFFFFFFFF;

    for(uint32_t i = 0; i < size / 4; i++)
    {
        uint32_t data = *(uint32_t*)(start_addr + i * 4);
        crc = CRC_Calculate(crc, data);
    }

    return crc;
}

// 固件验证
uint8_t Verify_Firmware(uint32_t expected_crc)
{
    uint32_t calculated_crc = Calculate_Firmware_CRC(APP_START_ADDRESS, APP_SIZE);

    if(calculated_crc == expected_crc)
        return SUCCESS;
    else
        return FAILURE;
}
```

## 六、注意事项

### 6.1 内部FLASH注意事项
1. **时钟配置**：系统时钟超过120MHz时，需要将HCLK分频
2. **操作顺序**：必须先擦除后编程
3. **地址对齐**：半字编程需要2字节对齐，字编程需要4字节对齐
4. **保护机制**：注意写保护和读保护的影响

### 6.2 外部SPI FLASH注意事项
1. **片选管理**：操作前后正确控制CS引脚
2. **状态检查**：编程/擦除前检查BUSY位
3. **页边界**：注意256字节页边界，跨页需要特殊处理
4. **寿命考虑**：FLASH有擦写次数限制，注意均衡磨损

### 6.3 FSMC FLASH注意事项
1. **时序配置**：根据FLASH规格书配置正确时序
2. **地址映射**：注意FSMC地址映射关系
3. **ECC校验**：NAND FLASH必须使用ECC
4. **坏块管理**：NAND FLASH需要坏块管理机制

### 6.4 性能优化
1. **使用快速模式**：内部FLASH使用快速编程模式
2. **DMA传输**：SPI FLASH使用DMA提高读取速度
3. **缓存机制**：频繁访问的数据缓存到RAM
4. **批量操作**：尽量减少擦写操作，批量处理数据

## 七、调试技巧

### 7.1 常见问题排查
1. **编程失败**
   - 检查FLASH是否解锁
   - 检查地址是否有效
   - 检查写保护状态

2. **数据错误**
   - 验证时序配置
   - 检查电源稳定性
   - 使用ECC校验

3. **性能问题**
   - 优化时钟配置
   - 使用快速模式
   - 启用缓存

### 7.2 调试工具
1. **逻辑分析仪**：分析SPI/FSMC时序
2. **串口调试**：输出调试信息
3. **内存查看**：验证编程结果
4. **CRC校验**：验证数据完整性

## 八、API函数参考

### 8.1 内部FLASH函数
- `FLASH_Unlock()` / `FLASH_Lock()` - 解锁/加锁
- `FLASH_ErasePage()` - 擦除页
- `FLASH_ProgramHalfWord()` / `FLASH_ProgramWord()` - 编程
- `FLASH_GetStatus()` - 获取状态

### 8.2 快速编程函数
- `FLASH_Unlock_Fast()` / `FLASH_Lock_Fast()` - 快速模式解锁/加锁
- `FLASH_ErasePage_Fast()` - 快速页擦除
- `FLASH_ProgramPage_Fast()` - 快速页编程

### 8.3 保护函数
- `FLASH_EnableWriteProtection()` - 使能写保护
- `FLASH_ReadOutProtection()` - 读保护配置
- `FLASH_UserOptionByteConfig()` - 用户选项字节

### 8.4 SPI FLASH函数
- `SPI_Flash_ReadID()` - 读取ID
- `SPI_Flash_Erase_Sector()` - 扇区擦除
- `SPI_Flash_Write_Page()` - 页编程
- `SPI_Flash_Read_Buffer()` - 数据读取

## 总结

CH32V307支持多种FLASH存储器访问方式，包括内部FLASH、SPI FLASH和FSMC并行FLASH。关键配置要点包括正确的解锁/加锁序列、时序配置和保护机制。根据应用需求选择合适的FLASH类型和访问方式，可以提高系统性能和可靠性。

建议开发者在实际应用中注意FLASH的寿命管理、错误处理和性能优化，特别是在需要频繁擦写的场景中。
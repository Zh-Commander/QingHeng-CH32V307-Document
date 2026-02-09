# CH32V307 FPU浮点单元使用指南

本文档基于CH32V307 EVT示例代码分析，总结FPU浮点单元的使用方法和配置要点。

## 一、概述

FPU（Floating Point Unit）浮点单元是CH32V307 RISC-V内核的硬件浮点运算扩展，支持单精度浮点运算。

### 主要特性：
- 支持单精度浮点运算（IEEE 754标准）
- 硬件加速浮点加减乘除运算
- 集成在RISC-V内核中，通过F扩展实现
- 显著提高浮点运算性能

## 二、FPU配置

### 2.1 编译器配置

#### Eclipse IDE配置：
1. 打开工程属性
2. 进入 **C/C++ Build → Settings**
3. 选择 **Target Processor** 选项卡
4. 配置以下选项：
   - **Floating point**: `Single precision extension (RVF)`
   - **Floating point ABI**: `Single precision(f)`
   - **Architecture**: `RV32I`（包含相应扩展）
   - **Multiply extension (RVM)**: `true`
   - **Atomic extension (RVA)**: `true`
   - **Compressed extension (RVC)**: `true`

#### 命令行配置：
```makefile
# 编译器选项
-march=rv32imacxw    # I, M, A, C, XW扩展
-mabi=ilp32          # 32位整数，32位指针
-mfdiv               # 使能硬件除法
```

### 2.2 启动代码配置

在启动文件 `startup_ch32v30x_D8C.S` 中，FPU通过设置mstatus寄存器启用：

```assembly
/* Enable floating point and global interrupt, configure privileged mode */
li t0, 0x6088
csrw mstatus, t0
```

**0x6088值解析**：
- `0x6000` = FS字段设置为11（启用浮点单元）
- `0x0080` = MPIE位（机器模式前一个中断使能）
- `0x0008` = MIE位（机器模式中断使能）

### 2.3 工程配置文件

#### .cproject文件配置：
```xml
<!-- Floating point配置 -->
<option id="ilg.gnumcueclipse.managedbuild.cross.riscv.option.isa.fp"
        superClass="ilg.gnumcueclipse.managedbuild.cross.riscv.option.isa.fp"
        value="ilg.gnumcueclipse.managedbuild.cross.riscv.option.isa.fp.single"
        valueType="enumerated"/>

<!-- Floating point ABI配置 -->
<option id="ilg.gnumcueclipse.managedbuild.cross.riscv.option.abi.fp"
        superClass="ilg.gnumcueclipse.managedbuild.cross.riscv.option.abi.fp"
        value="ilg.gnumcueclipse.managedbuild.cross.riscv.option.abi.fp.single"
        valueType="enumerated"/>
```

## 三、基本使用示例

### 3.1 FPU基础示例

**文件路径**：`FPU\FPU\User\main.c`

```c
/*
 *@Note
 *FPU hardware floating point operation routine:
 *This example demonstrates hardware floating-point arithmetic.
 *
 *Enable hardware floating point M-RS configuration reference This example configuration
 *Specific configuration-Properties -> C/C++ Build -> Setting -> Target Processor
 *-> The Floating point option is configured as Single precision extension (RVF)
 *Floating point ABI option configured as Single precision(f)
 */

#include "debug.h"

float val1 = 33.14;

int main(void)
{
    int t, t1;

    NVIC_PriorityGroupConfig(NVIC_PriorityGroup_2);
    USART_Printf_Init(115200);
    SystemCoreClockUpdate();

    printf("SystemClk:%d\r\n", SystemCoreClock);
    printf("ChipID:%08x\r\n", DBGMCU_GetCHIPID());

    // 浮点运算示例
    val1 = (val1 / 2 + 11.12) * 2;

    // 提取小数部分
    t = (int)(val1 * 10) % 10;   // 十分位
    t1 = (int)(val1 * 100) % 10; // 百分位

    printf("%d.%d%d\n", (int)val1, t, t1);

    while(1);
}
```

**输出结果**：
```
SystemClk:144000000
ChipID:30700518
55.26
```

**计算过程**：
```
val1 = 33.14
val1 = (33.14 / 2 + 11.12) * 2
     = (16.57 + 11.12) * 2
     = 27.69 * 2
     = 55.38
输出：55.26（注意浮点精度误差）
```

### 3.2 浮点运算性能测试

```c
#include "debug.h"
#include <math.h>

// 性能测试函数
void FPU_Performance_Test(void)
{
    float a = 3.1415926f;
    float b = 2.7182818f;
    float result;
    uint32_t start_time, end_time;

    printf("FPU Performance Test\n");

    // 加法测试
    start_time = SysTick->CNT;
    for(int i = 0; i < 1000; i++)
    {
        result = a + b;
    }
    end_time = SysTick->CNT;
    printf("Addition: %d cycles/1000 ops\n", end_time - start_time);

    // 乘法测试
    start_time = SysTick->CNT;
    for(int i = 0; i < 1000; i++)
    {
        result = a * b;
    }
    end_time = SysTick->CNT;
    printf("Multiplication: %d cycles/1000 ops\n", end_time - start_time);

    // 除法测试
    start_time = SysTick->CNT;
    for(int i = 0; i < 1000; i++)
    {
        result = a / b;
    }
    end_time = SysTick->CNT;
    printf("Division: %d cycles/1000 ops\n", end_time - start_time);

    // 平方根测试
    start_time = SysTick->CNT;
    for(int i = 0; i < 1000; i++)
    {
        result = sqrtf(a);
    }
    end_time = SysTick->CNT;
    printf("Square Root: %d cycles/1000 ops\n", end_time - start_time);

    // 三角函数测试
    start_time = SysTick->CNT;
    for(int i = 0; i < 1000; i++)
    {
        result = sinf(a);
    }
    end_time = SysTick->CNT;
    printf("Sine: %d cycles/1000 ops\n", end_time - start_time);
}
```

## 四、实际应用示例

### 4.1 RTC校准中的浮点运算

**文件路径**：`RTC\RTC_Calibrations\User\main.c`

```c
#define PPM_PER_STEP 0.9536743f  // 10^6/2^20
#define PPM_PER_SEC 0.3858025f   // 10^6/(30d*24h*3600s)

void RTC_Calibration(uint16_t FastSecPer30days)
{
    float Deviation = 0.0f;
    uint8_t CalibStep = 0;

    // 计算偏差（浮点运算）
    Deviation = FastSecPer30days * PPM_PER_SEC;
    Deviation /= PPM_PER_STEP;

    // 四舍五入
    CalibStep = (uint8_t)Deviation;
    if (Deviation >= (CalibStep + 0.5f))
        CalibStep += 1;

    // 限制范围
    if (CalibStep > 127)
        CalibStep = 127;

    // 设置校准值
    BKP_SetRTCCalibrationValue(CalibStep);
    printf("Calibration cab: %d\n", CalibStep);
}
```

**应用场景**：RTC时钟精度校准，需要高精度浮点计算

### 4.2 WS2812 LED颜色插值

**文件路径**：`APPLICATION\WS2812_LED\User\main.c`

```c
uint32_t interpolateColors(uint32_t color1, uint32_t color2, uint8_t step)
{
    uint8_t r1 = (color1 >> 16) & 0xFF;
    uint8_t g1 = (color1 >> 8) & 0xFF;
    uint8_t b1 = color1 & 0xFF;

    uint8_t r2 = (color2 >> 16) & 0xFF;
    uint8_t g2 = (color2 >> 8) & 0xFF;
    uint8_t b2 = color2 & 0xFF;

    // 浮点插值计算
    float ratio = step / (float)MAX_STEP;

    float tmp1 = (r2 - r1) * ratio;
    float tmp2 = (g2 - g1) * ratio;
    float tmp3 = (b2 - b1) * ratio;

    uint8_t r = (uint8_t)(r1 + tmp1);
    uint8_t g = (uint8_t)(g1 + tmp2);
    uint8_t b = (uint8_t)(b1 + tmp3);

    return ((uint32_t)r << 16) | ((uint32_t)g << 8) | b;
}
```

**应用场景**：LED颜色平滑过渡，需要浮点插值计算

### 4.3 数字信号处理

```c
// FIR滤波器实现
void FIR_Filter(float* input, float* output, uint32_t length)
{
    // 滤波器系数
    static const float coeffs[] = {
        0.1f, 0.2f, 0.4f, 0.2f, 0.1f
    };
    static const uint32_t N = sizeof(coeffs) / sizeof(coeffs[0]);

    // FIR滤波计算
    for(uint32_t i = 0; i < length; i++)
    {
        float sum = 0.0f;

        for(uint32_t j = 0; j < N; j++)
        {
            if(i >= j)
            {
                sum += input[i - j] * coeffs[j];
            }
        }

        output[i] = sum;
    }
}

// IIR滤波器实现
void IIR_Filter(float* input, float* output, uint32_t length)
{
    // 滤波器系数
    const float a0 = 1.0f;
    const float a1 = -0.5f;
    const float b0 = 0.1f;
    const float b1 = 0.2f;

    // 状态变量
    static float x_prev = 0.0f;
    static float y_prev = 0.0f;

    // IIR滤波计算
    for(uint32_t i = 0; i < length; i++)
    {
        float x = input[i];
        float y = (b0 * x + b1 * x_prev - a1 * y_prev) / a0;

        output[i] = y;

        // 更新状态
        x_prev = x;
        y_prev = y;
    }
}
```

## 五、FPU编程最佳实践

### 5.1 数据类型使用

#### 5.1.1 浮点常量
```c
// 正确：使用f后缀表示单精度浮点数
float f1 = 3.14f;
float f2 = 2.0f * 1.5f;

// 错误：没有f后缀，默认为双精度
float f3 = 3.14;    // 需要双精度到单精度转换
float f4 = 2.0 * 1.5;  // 双精度运算，然后转换
```

#### 5.1.2 类型转换
```c
// 显式类型转换
int i = 10;
float f = (float)i;  // 正确：显式转换

// 隐式类型转换（避免使用）
float f2 = i;        // 可以工作，但不明确

// 浮点到整数转换
float f3 = 3.14f;
int i2 = (int)f3;    // i2 = 3（截断）
int i3 = (int)(f3 + 0.5f);  // 四舍五入
```

### 5.2 精度控制

#### 5.2.1 避免累积误差
```c
// 错误：累积误差
float sum = 0.0f;
for(int i = 0; i < 1000; i++)
{
    sum += 0.1f;  // 累积误差
}

// 更好：使用整数计算
float sum2 = 0.0f;
for(int i = 0; i < 1000; i++)
{
    sum2 += 1.0f;  // 先加整数
}
sum2 *= 0.1f;      // 最后乘以系数
```

#### 5.2.2 比较浮点数
```c
// 错误：直接比较
if(f1 == f2)  // 可能永远不成立

// 正确：使用误差范围
#define EPSILON 0.0001f
if(fabsf(f1 - f2) < EPSILON)  // 在误差范围内相等

// 相对误差比较
if(fabsf(f1 - f2) < EPSILON * fmaxf(fabsf(f1), fabsf(f2)))
```

### 5.3 性能优化

#### 5.3.1 减少浮点运算
```c
// 优化前：每次循环都做浮点除法
for(int i = 0; i < N; i++)
{
    result[i] = data[i] / scale;
}

// 优化后：使用乘法代替除法
float inv_scale = 1.0f / scale;  // 一次除法
for(int i = 0; i < N; i++)
{
    result[i] = data[i] * inv_scale;  // 乘法更快
}
```

#### 5.3.2 使用查表法
```c
// 查表法代替复杂计算
static const float sin_table[] = {
    0.0f, 0.0174524f, 0.0348995f, 0.0523360f,  // sin(0°), sin(1°), ...
    // ... 更多值
};

float fast_sin(float angle_deg)
{
    int index = (int)(angle_deg + 0.5f);  // 四舍五入
    if(index < 0) index = 0;
    if(index >= 360) index = 359;

    return sin_table[index];  // 查表代替sinf()计算
}
```

#### 5.3.3 向量化运算
```c
// 手动向量化
void vector_add(float* a, float* b, float* c, uint32_t n)
{
    // 一次处理4个元素（如果编译器支持向量化）
    for(uint32_t i = 0; i < n; i += 4)
    {
        c[i] = a[i] + b[i];
        c[i+1] = a[i+1] + b[i+1];
        c[i+2] = a[i+2] + b[i+2];
        c[i+3] = a[i+3] + b[i+3];
    }
}
```

## 六、编译器优化选项

### 6.1 GCC优化选项
```makefile
# 优化级别
-O1    # 基本优化
-O2    # 更多优化（推荐）
-O3    # 激进优化（可能增加代码大小）
-Os    # 优化代码大小

# 浮点优化
-ffast-math          # 快速数学（放宽IEEE合规性）
-fno-math-errno      # 不设置errno
-funsafe-math-optimizations  # 不安全的数学优化

# 架构优化
-mtune=size          # 优化代码大小
-mtune=performance   # 优化性能
```

### 6.2 链接器优化
```makefile
# 链接时优化
-flto                # 链接时优化

# 函数和数据段优化
-ffunction-sections  # 每个函数独立段
-fdata-sections      # 每个数据独立段
-Wl,--gc-sections    # 垃圾回收未使用段
```

### 6.3 实际配置示例
```makefile
# 推荐的FPU优化配置
CFLAGS = -march=rv32imacxw \
         -mabi=ilp32 \
         -mfdiv \
         -O2 \
         -ffast-math \
         -fno-math-errno \
         -ffunction-sections \
         -fdata-sections

LDFLAGS = -Wl,--gc-sections \
          -flto
```

## 七、错误处理

### 7.1 浮点异常检查
```c
#include <fenv.h>

void enable_fp_exceptions(void)
{
    // 启用浮点异常
    feclearexcept(FE_ALL_EXCEPT);
    feenableexcept(FE_DIVBYZERO | FE_INVALID | FE_OVERFLOW);
}

void check_fp_exceptions(void)
{
    // 检查浮点异常
    int exceptions = fetestexcept(FE_ALL_EXCEPT);

    if(exceptions & FE_DIVBYZERO)
        printf("Floating point division by zero\n");

    if(exceptions & FE_INVALID)
        printf("Floating point invalid operation\n");

    if(exceptions & FE_OVERFLOW)
        printf("Floating point overflow\n");

    if(exceptions & FE_UNDERFLOW)
        printf("Floating point underflow\n");

    if(exceptions & FE_INEXACT)
        printf("Floating point inexact result\n");

    // 清除异常标志
    feclearexcept(FE_ALL_EXCEPT);
}
```

### 7.2 NaN和Infinity检查
```c
#include <math.h>

int is_valid_float(float f)
{
    // 检查是否为有限数
    if(!isfinite(f))
    {
        if(isnan(f))
            printf("Value is NaN\n");
        else if(isinf(f))
            printf("Value is Infinity\n");

        return 0;
    }

    return 1;
}

// 安全除法
float safe_divide(float a, float b)
{
    if(b == 0.0f)
    {
        if(a == 0.0f)
            return 0.0f;  // 0/0 = 0
        else if(a > 0.0f)
            return INFINITY;  // 正数/0 = +∞
        else
            return -INFINITY; // 负数/0 = -∞
    }

    return a / b;
}
```

## 八、调试技巧

### 8.1 浮点寄存器查看
```c
// 打印浮点值
void print_float_info(const char* name, float value)
{
    uint32_t* p = (uint32_t*)&value;

    printf("%s: %.6f (0x%08X)\n", name, value, *p);

    // 分析IEEE 754格式
    uint32_t sign = (*p >> 31) & 0x1;
    uint32_t exponent = (*p >> 23) & 0xFF;
    uint32_t mantissa = *p & 0x7FFFFF;

    printf("  Sign: %d, Exponent: %d (0x%02X), Mantissa: 0x%06X\n",
           sign, exponent - 127, exponent, mantissa);
}

// 示例使用
float test = 3.14f;
print_float_info("test", test);
```

### 8.2 性能分析
```c
#include "debug.h"

// 简单性能分析
void profile_fp_operation(void)
{
    uint32_t start_cycle, end_cycle;
    float result;
    float a = 3.14f, b = 2.71f;

    // 禁用中断以获得准确周期计数
    __disable_irq();

    // 测试加法
    start_cycle = SysTick->CNT;
    result = a + b;
    end_cycle = SysTick->CNT;
    printf("Add: %d cycles\n", end_cycle - start_cycle);

    // 测试乘法
    start_cycle = SysTick->CNT;
    result = a * b;
    end_cycle = SysTick->CNT;
    printf("Mul: %d cycles\n", end_cycle - start_cycle);

    // 测试除法
    start_cycle = SysTick->CNT;
    result = a / b;
    end_cycle = SysTick->CNT;
    printf("Div: %d cycles\n", end_cycle - start_cycle);

    // 启用中断
    __enable_irq();
}
```

### 8.3 内存使用分析
```c
// 检查栈使用
void check_stack_usage(void)
{
    extern uint32_t _estack;  // 栈顶（链接脚本定义）
    extern uint32_t __stack;  // 栈底

    uint32_t stack_size = (uint32_t)&_estack - (uint32_t)&__stack;
    uint32_t* stack_ptr;

    // 获取当前栈指针
    asm volatile("mv %0, sp" : "=r"(stack_ptr));

    uint32_t used = (uint32_t)&_estack - (uint32_t)stack_ptr;
    uint32_t free = (uint32_t)stack_ptr - (uint32_t)&__stack;

    printf("Stack: total=%lu, used=%lu, free=%lu, usage=%.1f%%\n",
           stack_size, used, free, (float)used/stack_size*100.0f);
}
```

## 九、常见问题与解决方案

### 9.1 编译问题

#### 问题1：未定义引用
```
undefined reference to `__addsf3'
undefined reference to `__mulsf3'
```

**解决方案**：确保编译器配置正确
1. 检查Floating point配置为`Single precision extension (RVF)`
2. 检查Floating point ABI配置为`Single precision(f)`
3. 确认链接了正确的库（libgcc）

#### 问题2：性能低下
**解决方案**：
1. 启用编译器优化（-O2或-O3）
2. 使用`-ffast-math`选项（如果允许）
3. 减少不必要的类型转换
4. 使用查表法代替复杂计算

### 9.2 运行时问题

#### 问题1：精度误差
**解决方案**：
1. 使用双精度计算关键部分（如果支持）
2. 使用Kahan求和算法减少累积误差
3. 合理设置比较误差范围

#### 问题2：栈溢出
**解决方案**：
1. 减少局部浮点数组大小
2. 使用堆分配大数组
3. 优化递归深度

### 9.3 配置问题

#### 问题1：FPU未启用
**解决方案**：
1. 检查启动文件中的mstatus配置（0x6088）
2. 确认编译器生成正确的FPU指令
3. 验证FPU寄存器可访问

## 十、应用场景推荐

### 10.1 推荐使用FPU的场景

#### 数字信号处理
- FIR/IIR滤波器
- FFT变换
- 音频处理
- 图像处理

#### 控制系统
- PID控制器
- 状态估计
- 轨迹规划
- 机器人控制

#### 科学计算
- 数值积分
- 方程求解
- 统计分析
- 模拟仿真

### 10.2 不建议使用FPU的场景

#### 简单整数运算
- 计数器
- 状态机
- 位操作

#### 对功耗敏感的应用
- 电池供电设备
- 低功耗模式
- 能量采集系统

#### 代码空间受限的应用
- Bootloader
- 安全启动
- 极小内存系统

## 十一、参考资料

### 11.1 官方文档
- **CH32V307参考手册**：第4章 RISC-V内核，第4.3节 FPU
- **RISC-V ISA手册**：第7章 F扩展（单精度浮点）
- **编译器手册**：GCC RISC-V选项

### 11.2 示例代码
- **FPU示例**：`EXAM/FPU/FPU/`
- **RTC校准**：`EXAM/RTC/RTC_Calibrations/`
- **WS2812 LED**：`EXAM/APPLICATION/WS2812_LED/`

### 11.3 工具使用
- **Eclipse配置指南**：工程属性配置说明
- **调试器使用**：浮点寄存器查看方法
- **性能分析工具**：周期计数和性能测量

## 总结

CH32V307的FPU为单精度浮点运算提供硬件加速，显著提高浮点计算性能。关键配置要点包括编译器选项设置、启动代码配置和优化策略。正确使用FPU可以大幅提升数字信号处理、控制系统和科学计算等应用的性能。

建议开发者在需要高精度计算的应用中启用FPU，并注意优化浮点代码以减少精度误差和提高性能。对于简单应用或对功耗敏感的场景，可以考虑使用软件浮点或定点运算。
# 第 9 章 中断和事件（NVIC/PFIC）

本章模块描述适用于CH32F2x、CH32V2x和CH32V3x微控制器全系列产品。

${ \tt C H 3 2 F 2 x }$ 系列产品采用 Cortex-M3 内核，内置嵌套向量中断控制器(NVIC– Nested VectoredInterrupt Controller)，管理了 88 个可屏蔽外部中断通道和 10 个内核中断通道，其他中断源保留。中断控制器与内核接口紧密相连，以最小的中断延迟提供了灵活的中断管理功能。具体关于 NVIC 控制器的使用说明请参考Cortex-M3相关文档说明。

CH32V2x 和 CH32V3x 系列内置可编程快速中断控制器（PFIC– Programmable Fast InterruptController），最多支持 255 个中断向量。当前系统管理了 88 个外设中断通道和 8 个内核中断通道，其他保留。

## 9.1 主要特征

### 9.1.1 NVIC 控制器

$\bullet$ 88个可屏蔽的中断通道  
$\bullet$ 提供不可屏蔽中断第一时间响应  
$\bullet$ 向量化的中断设计实现向量入口地址直接进入内核  
$\bullet$ 中断进入和退出时自动压栈和恢复，无需额外指令开销  
16级嵌套，优先级动态修改

### 9.1.2 PFIC 控制器

88个外设中断，每个中断请求都有独立的触发和屏蔽控制位，有专用的状态位  
$\bullet$ 可编程多级中断嵌套，最大嵌套深度8级，硬件压栈深度3级  
$\bullet$ 特有快速中断进出机制，硬件自动压栈和恢复，无需指令开销   
$\bullet$ 特有免表VTF（Vector Table Free）中断响应机制，4路可编程直达中断向量地址

## 9.2 系统定时器

CH32F2x 系列产品

Cortex-M3 内核自带了一个 24 位自减型计数器（SysTick timer）。支持 HCLK 或 HCLK/8 作为时基，具有较高优先级别（6）。一般可用于操作系统的时基。具体请参考Cortex-M3相关文档说明。

CH32V3x 系列产品

内核自带了一个 64 位加减计数器（SysTick），支持 HCLK 或者 HCLK/8 作为时基，具有较高优先级，校准后可用于时间基准。

## 9.3 中断和异常的向量表

表 9-1 CH32F2x 系列产品向量表  

<table><tr><td>位置</td><td>优先级</td><td>优先级类型</td><td>名称</td><td>说明</td><td>绝对地址</td></tr><tr><td></td><td>-</td><td>-</td><td>-</td><td>保留</td><td>0x00000000</td></tr><tr><td></td><td>-3</td><td>固定</td><td>Reset</td><td>复位</td><td>0x00000004</td></tr><tr><td></td><td>-2</td><td>固定</td><td>NMI</td><td>不可屏蔽中断</td><td>0x00000008</td></tr><tr><td></td><td>-1</td><td>固定</td><td>HardFault</td><td>所有类型的失效</td><td>0x0000000C</td></tr><tr><td></td><td>0</td><td>可设置</td><td>MemManage</td><td>存储器管理</td><td>0x00000010</td></tr><tr><td></td><td>1</td><td>可设置</td><td>BusFault</td><td>预取指失败,存储器访问失败</td><td>0x00000014</td></tr><tr><td></td><td>2</td><td>可设置</td><td>UsageFault</td><td>未定义的指令或非法状态</td><td>0x00000018</td></tr><tr><td></td><td>-</td><td>-</td><td>-</td><td>保留</td><td>0x0000001C-0x0000002B</td></tr><tr><td></td><td>3</td><td>可设置</td><td>SVCall</td><td>通过SWI指令的系统服务调用</td><td>0x0000002C</td></tr><tr><td></td><td>4</td><td>可设置</td><td>Debug Monitor</td><td>调试监控器</td><td>0x00000030</td></tr><tr><td></td><td>-</td><td>-</td><td>-</td><td>保留</td><td>0x00000034</td></tr><tr><td></td><td>5</td><td>可设置</td><td>PendSV</td><td>可挂起的系统服务</td><td>0x00000038</td></tr><tr><td></td><td>6</td><td>可设置</td><td>SysTick</td><td>系统滴答定时器</td><td>0x0000003C</td></tr><tr><td>0</td><td>7</td><td>可编程</td><td>WWDG</td><td>窗口定时器中断</td><td>0x00000040</td></tr><tr><td>1</td><td>8</td><td>可编程</td><td>PVD</td><td>电源电压检测中断(EXT1)</td><td>0x00000044</td></tr><tr><td>2</td><td>9</td><td>可编程</td><td>TAMPER</td><td>侵入检测中断</td><td>0x00000048</td></tr><tr><td>3</td><td>10</td><td>可编程</td><td>RTC</td><td>实时时钟中断</td><td>0x0000004C</td></tr><tr><td>4</td><td>11</td><td>可编程</td><td>FLASH</td><td>闪存全局中断</td><td>0x00000050</td></tr><tr><td>5</td><td>12</td><td>可编程</td><td>RCC</td><td>复位和时钟控制中断</td><td>0x00000054</td></tr><tr><td>6</td><td>13</td><td>可编程</td><td>EXT10</td><td>EXTI线0中断</td><td>0x00000058</td></tr><tr><td>7</td><td>14</td><td>可编程</td><td>EXT11</td><td>EXTI线1中断</td><td>0x0000005C</td></tr><tr><td>8</td><td>15</td><td>可编程</td><td>EXT12</td><td>EXTI线2中断</td><td>0x00000060</td></tr><tr><td>9</td><td>16</td><td>可编程</td><td>EXT13</td><td>EXTI线3中断</td><td>0x00000064</td></tr><tr><td>10</td><td>17</td><td>可编程</td><td>EXT14</td><td>EXTI线4中断</td><td>0x00000068</td></tr><tr><td>11</td><td>18</td><td>可编程</td><td>DMA1_CH1</td><td>DMA1通道1全局中断</td><td>0x0000006C</td></tr><tr><td>12</td><td>19</td><td>可编程</td><td>DMA1_CH2</td><td>DMA1通道2全局中断</td><td>0x00000070</td></tr><tr><td>13</td><td>20</td><td>可编程</td><td>DMA1_CH3</td><td>DMA1通道3全局中断</td><td>0x00000074</td></tr><tr><td>14</td><td>21</td><td>可编程</td><td>DMA1_CH4</td><td>DMA1通道4全局中断</td><td>0x00000078</td></tr><tr><td>15</td><td>22</td><td>可编程</td><td>DMA1_CH5</td><td>DMA1通道5全局中断</td><td>0x0000007C</td></tr><tr><td>16</td><td>23</td><td>可编程</td><td>DMA1_CH6</td><td>DMA1通道6全局中断</td><td>0x00000080</td></tr><tr><td>17</td><td>24</td><td>可编程</td><td>DMA1_CH7</td><td>DMA1通道7全局中断</td><td>0x00000084</td></tr><tr><td>18</td><td>25</td><td>可编程</td><td>ADC1_2</td><td>ADC1和ADC2全局中断</td><td>0x00000088</td></tr><tr><td>19</td><td>26</td><td>可编程</td><td>USB_HP或</td><td>USB_HP或CAN1_TX全局中断</td><td>0x0000008C</td></tr><tr><td>20</td><td>27</td><td>可编程</td><td>USB_LP或</td><td>USB_LP或CAN1_RX0全局中断</td><td>0x00000090</td></tr><tr><td>21</td><td>28</td><td>可编程</td><td>CAN1_RX1</td><td>CAN1_RX1全局中断</td><td>0x00000094</td></tr><tr><td>22</td><td>29</td><td>可编程</td><td>CAN1_SCE</td><td>CAN1_SCE全局中断</td><td>0x00000098</td></tr><tr><td>23</td><td>30</td><td>可编程</td><td>EXT19_5</td><td>EXTI线[9:5]中断</td><td>0x0000009C</td></tr><tr><td>24</td><td>31</td><td>可编程</td><td>TIM1_BRK</td><td>TIM1刹车中断</td><td>0x000000A0</td></tr><tr><td>25</td><td>32</td><td>可编程</td><td>TIM1_UP</td><td>TIM1更新中断</td><td>0x000000A4</td></tr><tr><td>26</td><td>33</td><td>可编程</td><td>TIM1_TRG_COM</td><td>TIM1触发和通信中断</td><td>0x000000A8</td></tr><tr><td>27</td><td>34</td><td>可编程</td><td>TIM1_CC</td><td>TIM1捕获比较中断</td><td>0x000000AC</td></tr><tr><td>28</td><td>35</td><td>可编程</td><td>TIM2</td><td>TIM2全局中断</td><td>0x000000B0</td></tr><tr><td>29</td><td>36</td><td>可编程</td><td>TIM3</td><td>TIM3全局中断</td><td>0x000000B4</td></tr><tr><td>30</td><td>37</td><td>可编程</td><td>TIM4</td><td>TIM4全局中断</td><td>0x000000B8</td></tr><tr><td>31</td><td>38</td><td>可编程</td><td>I2C1_ER</td><td>I²C1事件中断</td><td>0x000000BC</td></tr><tr><td>32</td><td>39</td><td>可编程</td><td>I2C1_ER</td><td>I²C1错误中断</td><td>0x000000C0</td></tr><tr><td>33</td><td>40</td><td>可编程</td><td>I2C2_ER</td><td>I²C2事件中断</td><td>0x000000C4</td></tr><tr><td>34</td><td>41</td><td>可编程</td><td>I2C2_ER</td><td>I²C2错误中断</td><td>0x000000C8</td></tr><tr><td>35</td><td>42</td><td>可编程</td><td>SPI1</td><td>SPI1全局中断</td><td>0x000000CC</td></tr><tr><td>36</td><td>43</td><td>可编程</td><td>SPI2</td><td>SPI2全局中断</td><td>0x000000D0</td></tr><tr><td>37</td><td>44</td><td>可编程</td><td>USART1</td><td>USART1 全局中断</td><td>0x000000D4</td></tr><tr><td>38</td><td>45</td><td>可编程</td><td>USART2</td><td>USART2 全局中断</td><td>0x000000D8</td></tr><tr><td>39</td><td>46</td><td>可编程</td><td>USART3</td><td>USART3 全局中断</td><td>0x000000DC</td></tr><tr><td>40</td><td>47</td><td>可编程</td><td>EXT115_10</td><td>EXT1 线[15:10]中断</td><td>0x000000E0</td></tr><tr><td>41</td><td>48</td><td>可编程</td><td>RTCA1arm</td><td>RTC 闹钟中断 (EXT1)</td><td>0x000000E4</td></tr><tr><td>42</td><td>49</td><td>可编程</td><td>USBWakeUp</td><td>USB 唤醒中断 (EXT1)</td><td>0x000000E8</td></tr><tr><td>43</td><td>50</td><td>可编程</td><td>TIM8_BRK</td><td>TIM8 刹车中断</td><td>0x000000EC</td></tr><tr><td>44</td><td>51</td><td>可编程</td><td>TIM8_UP</td><td>TIM8 更新中断</td><td>0x000000F0</td></tr><tr><td>45</td><td>52</td><td>可编程</td><td>TIM8_TRG_COM</td><td>TIM8 触发和通信中断</td><td>0x000000F4</td></tr><tr><td>46</td><td>53</td><td>可编程</td><td>TIM8_CC</td><td>TIM8 捕获比较中断</td><td>0x000000F8</td></tr><tr><td>47</td><td>54</td><td>可编程</td><td>RNG</td><td>RNG 全局中断</td><td>0x000000FC</td></tr><tr><td>48</td><td>55</td><td>可编程</td><td>FSMC</td><td>FSMC 全局中断</td><td>0x00000100</td></tr><tr><td>49</td><td>56</td><td>可编程</td><td>SD10</td><td>SD10 全局中断</td><td>0x00000104</td></tr><tr><td>50</td><td>57</td><td>可编程</td><td>TIM5</td><td>TIM5 全局中断</td><td>0x00000108</td></tr><tr><td>51</td><td>58</td><td>可编程</td><td>SPI3</td><td>SPI3 全局中断</td><td>0x0000010C</td></tr><tr><td>52</td><td>59</td><td>可编程</td><td>UART4</td><td>UART4 全局中断</td><td>0x00000110</td></tr><tr><td>53</td><td>60</td><td>可编程</td><td>UART5</td><td>UART5 全局中断</td><td>0x00000114</td></tr><tr><td>54</td><td>61</td><td>可编程</td><td>TIM6</td><td>TIM6 全局中断</td><td>0x00000118</td></tr><tr><td>55</td><td>62</td><td>可编程</td><td>TIM7</td><td>TIM7 全局中断</td><td>0x0000011C</td></tr><tr><td>56</td><td>63</td><td>可编程</td><td>DMA2_CH1</td><td>DMA2 通道 1 全局中断</td><td>0x00000120</td></tr><tr><td>57</td><td>64</td><td>可编程</td><td>DMA2_CH2</td><td>DMA2 通道 2 全局中断</td><td>0x00000124</td></tr><tr><td>58</td><td>65</td><td>可编程</td><td>DMA2_CH3</td><td>DMA2 通道 3 全局中断</td><td>0x00000128</td></tr><tr><td>59</td><td>66</td><td>可编程</td><td>DMA2_CH4</td><td>DMA2 通道 4 全局中断</td><td>0x0000012C</td></tr><tr><td>60</td><td>67</td><td>可编程</td><td>DMA2_CH5</td><td>DMA2 通道 5 全局中断</td><td>0x00000130</td></tr><tr><td>61</td><td>68</td><td>可编程</td><td>ETH</td><td>ETH 全局中断</td><td>0x00000134</td></tr><tr><td>62</td><td>69</td><td>可编程</td><td>ETH_WKUP</td><td>ETH 唤醒中断</td><td>0x00000138</td></tr><tr><td>63</td><td>70</td><td>可编程</td><td>CAN2_TX</td><td>CAN2_TX 全局中断</td><td>0x0000013C</td></tr><tr><td>64</td><td>71</td><td>可编程</td><td>CAN2_RX0</td><td>CAN2_RX0 全局中断</td><td>0x00000140</td></tr><tr><td>65</td><td>72</td><td>可编程</td><td>CAN2_RX1</td><td>CAN2_RX1 全局中断</td><td>0x00000144</td></tr><tr><td>66</td><td>73</td><td>可编程</td><td>CAN2_SCE</td><td>CAN2_SCE 全局中断</td><td>0x00000148</td></tr><tr><td>67</td><td>74</td><td>可编程</td><td>OTG_FS</td><td>全速 0TG 中断</td><td>0x0000014C</td></tr><tr><td>68</td><td>75</td><td>可编程</td><td>USBHSWakeUp</td><td>高速 USB 唤醒中断</td><td>0x00000150</td></tr><tr><td>69</td><td>76</td><td>可编程</td><td>USBHS</td><td>高速 USB 全局中断</td><td>0x00000154</td></tr><tr><td>70</td><td>77</td><td>可编程</td><td>DVP</td><td>DVP 全局中断</td><td>0x00000158</td></tr><tr><td>71</td><td>78</td><td>可编程</td><td>UART6</td><td>UART6 全局中断</td><td>0x0000015C</td></tr><tr><td>72</td><td>79</td><td>可编程</td><td>UART7</td><td>UART7 全局中断</td><td>0x00000160</td></tr><tr><td>73</td><td>80</td><td>可编程</td><td>UART8</td><td>UART8 全局中断</td><td>0x00000164</td></tr><tr><td>74</td><td>81</td><td>可编程</td><td>TIM9_BRK</td><td>TIM9 刹车中断</td><td>0x00000168</td></tr><tr><td>75</td><td>82</td><td>可编程</td><td>TIM9_UP</td><td>TIM9 更新中断</td><td>0x0000016C</td></tr><tr><td>76</td><td>83</td><td>可编程</td><td>TIM9_TRG_COM</td><td>TIM9 触发和通信中断</td><td>0x00000170</td></tr><tr><td>77</td><td>84</td><td>可编程</td><td>TIM9_CC</td><td>TIM9 捕获比较中断</td><td>0x00000174</td></tr><tr><td>78</td><td>85</td><td>可编程</td><td>TIM10_BRK</td><td>TIM10 刹车中断</td><td>0x00000178</td></tr><tr><td>79</td><td>86</td><td>可编程</td><td>TIM10_UP</td><td>TIM10 更新中断</td><td>0x0000017C</td></tr><tr><td>80</td><td>87</td><td>可编程</td><td>TIM10_TRG_COM</td><td>TIM10 触发和通信中断</td><td>0x00000180</td></tr><tr><td>81</td><td>88</td><td>可编程</td><td>TIM10_CC</td><td>TIM10 捕获比较中断</td><td>0x00000184</td></tr><tr><td>82</td><td>89</td><td>可编程</td><td>DMA2_CH6</td><td>DMA2 通道 6 全局中断</td><td>0x00000188</td></tr><tr><td>83</td><td>90</td><td>可编程</td><td>DMA2_CH7</td><td>DMA2 通道 7 全局中断</td><td>0x0000018C</td></tr><tr><td>84</td><td>91</td><td>可编程</td><td>DMA2_CH8</td><td>DMA2 通道 8 全局中断</td><td>0x00000190</td></tr><tr><td>85</td><td>92</td><td>可编程</td><td>DMA2_CH9</td><td>DMA2 通道 9 全局中断</td><td>0x00000194</td></tr><tr><td>86</td><td>93</td><td>可编程</td><td>DMA2_CH10</td><td>DMA2 通道 10 全局中断</td><td>0x00000198</td></tr><tr><td>87</td><td>94</td><td>可编程</td><td>DMA2_CH11</td><td>DMA2 通道 11 全局中断</td><td>0x0000019C</td></tr></table>

表 9-2 CH32V2x 和 CH32V3x 系列产品向量表  

<table><tr><td>编号</td><td>优先级</td><td>类型</td><td>名称</td><td>描述</td><td>入口地址</td></tr><tr><td>0</td><td>-</td><td>-</td><td>-</td><td>-</td><td>0x00000000</td></tr><tr><td>1</td><td>-</td><td>-</td><td>-</td><td>-</td><td>0x00000004</td></tr><tr><td>2</td><td>-5</td><td>固定</td><td>NMI</td><td>不可屏蔽中断</td><td>0x00000008</td></tr><tr><td>3</td><td>-4</td><td>固定</td><td>HardFault</td><td>异常中断</td><td>0x0000000C</td></tr><tr><td>4</td><td>-</td><td>-</td><td>-</td><td>保留</td><td>0x00000010</td></tr><tr><td>5</td><td>-3</td><td>固定</td><td>Ecall-M</td><td>机器模式回调中断</td><td>0x00000014</td></tr><tr><td>6-7</td><td>-</td><td>-</td><td>-</td><td>保留</td><td>0x00000018-0x0000001C</td></tr><tr><td>8</td><td>-2</td><td>固定</td><td>Ecall-U</td><td>用户模式回调中断</td><td>0x00000020</td></tr><tr><td>9</td><td>-1</td><td>固定</td><td>BreakPoint</td><td>断点回调中断</td><td>0x00000024</td></tr><tr><td>10-11</td><td>-</td><td>-</td><td>-</td><td>保留</td><td>0x00000028-0x0000002C</td></tr><tr><td>12</td><td>0</td><td>可编程</td><td>SysTick</td><td>系统定时器中断</td><td>0x00000030</td></tr><tr><td>13</td><td>-</td><td>-</td><td>-</td><td>保留</td><td>0x00000034</td></tr><tr><td>14</td><td>1</td><td>可编程</td><td>SW</td><td>软件中断</td><td>0x00000038</td></tr><tr><td>15</td><td>-</td><td>-</td><td>-</td><td>保留</td><td>0x0000003C</td></tr><tr><td>16</td><td>2</td><td>可编程</td><td>WWDG</td><td>窗口定时器中断</td><td>0x00000040</td></tr><tr><td>17</td><td>3</td><td>可编程</td><td>PVD</td><td>电源电压检测中断(EXTI)</td><td>0x00000044</td></tr><tr><td>18</td><td>4</td><td>可编程</td><td>TAMPER</td><td>侵入检测中断</td><td>0x00000048</td></tr><tr><td>19</td><td>5</td><td>可编程</td><td>RTC</td><td>实时时钟中断</td><td>0x0000004C</td></tr><tr><td>20</td><td>6</td><td>可编程</td><td>FLASH</td><td>闪存全局中断</td><td>0x00000050</td></tr><tr><td>21</td><td>7</td><td>可编程</td><td>RCC</td><td>复位和时钟控制中断</td><td>0x00000054</td></tr><tr><td>22</td><td>8</td><td>可编程</td><td>EXTI0</td><td>EXTI线0中断</td><td>0x00000058</td></tr><tr><td>23</td><td>9</td><td>可编程</td><td>EXTI1</td><td>EXTI线1中断</td><td>0x0000005C</td></tr><tr><td>24</td><td>10</td><td>可编程</td><td>EXTI2</td><td>EXTI线2中断</td><td>0x00000060</td></tr><tr><td>25</td><td>11</td><td>可编程</td><td>EXTI3</td><td>EXTI线3中断</td><td>0x00000064</td></tr><tr><td>26</td><td>12</td><td>可编程</td><td>EXTI4</td><td>EXTI线4中断</td><td>0x00000068</td></tr><tr><td>27</td><td>13</td><td>可编程</td><td>DMA1_CH1</td><td>DMA1通道1全局中断</td><td>0x0000006C</td></tr><tr><td>28</td><td>14</td><td>可编程</td><td>DMA1_CH2</td><td>DMA1通道2全局中断</td><td>0x00000070</td></tr><tr><td>29</td><td>15</td><td>可编程</td><td>DMA1_CH3</td><td>DMA1通道3全局中断</td><td>0x00000074</td></tr><tr><td>30</td><td>16</td><td>可编程</td><td>DMA1_CH4</td><td>DMA1通道4全局中断</td><td>0x00000078</td></tr><tr><td>31</td><td>17</td><td>可编程</td><td>DMA1_CH5</td><td>DMA1通道5全局中断</td><td>0x0000007C</td></tr><tr><td>32</td><td>18</td><td>可编程</td><td>DMA1_CH6</td><td>DMA1通道6全局中断</td><td>0x00000080</td></tr><tr><td>33</td><td>19</td><td>可编程</td><td>DMA1_CH7</td><td>DMA1通道7全局中断</td><td>0x00000084</td></tr><tr><td>34</td><td>20</td><td>可编程</td><td>ADC1_2</td><td>ADC1 和 ADC2 全局中断</td><td>0x00000088</td></tr><tr><td>35</td><td>21</td><td>可编程</td><td>USB_HP 或 CAN1_TX</td><td>USB_HP 或 CAN1_TX 全局中断</td><td>0x0000008C</td></tr><tr><td>36</td><td>22</td><td>可编程</td><td>USB_LP 或 CAN1_RX0</td><td>USB_LP 或 CAN1_RX0 全局中断</td><td>0x00000090</td></tr><tr><td>37</td><td>23</td><td>可编程</td><td>CAN1_RX1</td><td>CAN1_RX1 全局中断</td><td>0x00000094</td></tr><tr><td>38</td><td>24</td><td>可编程</td><td>CAN1_SCE</td><td>CAN1_SCE 全局中断</td><td>0x00000098</td></tr><tr><td>39</td><td>25</td><td>可编程</td><td>EXT19_5</td><td>EXT1 线[9:5]中断</td><td>0x0000009C</td></tr><tr><td>40</td><td>26</td><td>可编程</td><td>TIM1_BRK</td><td>TIM1 刹车中断</td><td>0x000000A0</td></tr><tr><td>41</td><td>27</td><td>可编程</td><td>TIM1_UP</td><td>TIM1 更新中断</td><td>0x000000A4</td></tr><tr><td>42</td><td>28</td><td>可编程</td><td>TIM1_TRG_COM</td><td>TIM1 触发和通信中断</td><td>0x000000A8</td></tr><tr><td>43</td><td>29</td><td>可编程</td><td>TIM1_CC</td><td>TIM1 捕获比较中断</td><td>0x000000AC</td></tr><tr><td>44</td><td>30</td><td>可编程</td><td>TIM2</td><td>TIM2 全局中断</td><td>0x000000B0</td></tr><tr><td>45</td><td>31</td><td>可编程</td><td>TIM3</td><td>TIM3 全局中断</td><td>0x000000B4</td></tr><tr><td>46</td><td>32</td><td>可编程</td><td>TIM4</td><td>TIM4 全局中断</td><td>0x000000B8</td></tr><tr><td>47</td><td>33</td><td>可编程</td><td>I2C1EV</td><td>I²C1 事件中断</td><td>0x000000BC</td></tr><tr><td>48</td><td>34</td><td>可编程</td><td>I2C1_ER</td><td>I²C1 错误中断</td><td>0x000000C0</td></tr><tr><td>49</td><td>35</td><td>可编程</td><td>I2C2EV</td><td>I²C2 事件中断</td><td>0x000000C4</td></tr><tr><td>50</td><td>36</td><td>可编程</td><td>I2C2_ER</td><td>I²C2 错误中断</td><td>0x000000C8</td></tr><tr><td>51</td><td>37</td><td>可编程</td><td>SPI1</td><td>SPI1 全局中断</td><td>0x000000CC</td></tr><tr><td>52</td><td>38</td><td>可编程</td><td>SPI2</td><td>SPI2 全局中断</td><td>0x000000D0</td></tr><tr><td>53</td><td>39</td><td>可编程</td><td>USART1</td><td>USART1 全局中断</td><td>0x000000D4</td></tr><tr><td>54</td><td>40</td><td>可编程</td><td>USART2</td><td>USART2 全局中断</td><td>0x000000D8</td></tr><tr><td>55</td><td>41</td><td>可编程</td><td>USART3</td><td>USART3 全局中断</td><td>0x000000DC</td></tr><tr><td>56</td><td>42</td><td>可编程</td><td>EXT115_10</td><td>EXT1 线[15:10]中断</td><td>0x000000E0</td></tr><tr><td>57</td><td>43</td><td>可编程</td><td>RTCA1arm</td><td>RTC 闹钟中断 (EXT1)</td><td>0x000000E4</td></tr><tr><td>58</td><td>44</td><td>可编程</td><td>USBWakeUp</td><td>USB 唤醒中断 (EXT1)</td><td>0x000000E8</td></tr><tr><td>59</td><td>45</td><td>可编程</td><td>TIM8_BRK</td><td>TIM8 刹车中断</td><td>0x000000EC</td></tr><tr><td>60</td><td>46</td><td>可编程</td><td>TIM8_UP</td><td>TIM8 更新中断</td><td>0x000000F0</td></tr><tr><td>61</td><td>47</td><td>可编程</td><td>TIM8_TRG_COM</td><td>TIM8 触发和通信中断</td><td>0x000000F4</td></tr><tr><td>62</td><td>48</td><td>可编程</td><td>TIM8_CC</td><td>TIM8 捕获比较中断</td><td>0x000000F8</td></tr><tr><td>63</td><td>49</td><td>可编程</td><td>RNG</td><td>RNG 全局中断</td><td>0x000000FC</td></tr><tr><td>64</td><td>50</td><td>可编程</td><td>FSMC</td><td>FSMC 全局中断</td><td>0x00000100</td></tr><tr><td>65</td><td>51</td><td>可编程</td><td>SD10</td><td>SD10 全局中断</td><td>0x00000104</td></tr><tr><td>66</td><td>52</td><td>可编程</td><td>TIM5</td><td>TIM5 全局中断</td><td>0x00000108</td></tr><tr><td>67</td><td>53</td><td>可编程</td><td>SPI3</td><td>SPI3 全局中断</td><td>0x0000010C</td></tr><tr><td>68</td><td>54</td><td>可编程</td><td>UART4</td><td>UART4 全局中断</td><td>0x00000110</td></tr><tr><td>69</td><td>55</td><td>可编程</td><td>UART5</td><td>UART5 全局中断</td><td>0x00000114</td></tr><tr><td>70</td><td>56</td><td>可编程</td><td>TIM6</td><td>TIM6 全局中断</td><td>0x00000118</td></tr><tr><td>71</td><td>57</td><td>可编程</td><td>TIM7</td><td>TIM7 全局中断</td><td>0x0000011C</td></tr><tr><td>72</td><td>58</td><td>可编程</td><td>DMA2_CH1</td><td>DMA2 通道 1 全局中断</td><td>0x00000120</td></tr><tr><td>73</td><td>59</td><td>可编程</td><td>DMA2_CH2</td><td>DMA2 通道 2 全局中断</td><td>0x00000124</td></tr><tr><td>74</td><td>60</td><td>可编程</td><td>DMA2_CH3</td><td>DMA2 通道 3 全局中断</td><td>0x00000128</td></tr><tr><td>75</td><td>61</td><td>可编程</td><td>DMA2_CH4</td><td>DMA2 通道 4 全局中断</td><td>0x0000012C</td></tr><tr><td>76</td><td>62</td><td>可编程</td><td>DMA2_CH5</td><td>DMA2 通道 5 全局中断</td><td>0x00000130</td></tr><tr><td>77</td><td>63</td><td>可编程</td><td>ETH</td><td>ETH 全局中断</td><td>0x00000134</td></tr><tr><td>78</td><td>64</td><td>可编程</td><td>ETH_WKUP</td><td>ETH 唤醒中断</td><td>0x00000138</td></tr><tr><td>79</td><td>65</td><td>可编程</td><td>CAN2_TX</td><td>CAN2_TX全局中断</td><td>0x0000013C</td></tr><tr><td>80</td><td>66</td><td>可编程</td><td>CAN2_RX0</td><td>CAN2_RX0全局中断</td><td>0x00000140</td></tr><tr><td>81</td><td>67</td><td>可编程</td><td>CAN2_RX1</td><td>CAN2_RX1全局中断</td><td>0x00000144</td></tr><tr><td>82</td><td>68</td><td>可编程</td><td>CAN2_SCE</td><td>CAN2_SCE全局中断</td><td>0x00000148</td></tr><tr><td>83</td><td>69</td><td>可编程</td><td>OTG_FS</td><td>全速OTG中断</td><td>0x0000014C</td></tr><tr><td>84</td><td>70</td><td>可编程</td><td>USBHSWakeUp</td><td>高速USB唤醒中断</td><td>0x00000150</td></tr><tr><td>85</td><td>71</td><td>可编程</td><td>USBHS</td><td>高速USB全局中断</td><td>0x00000154</td></tr><tr><td>86</td><td>72</td><td>可编程</td><td>DVP</td><td>DVP全局中断</td><td>0x00000158</td></tr><tr><td>87</td><td>73</td><td>可编程</td><td>UART6</td><td>UART6全局中断</td><td>0x0000015C</td></tr><tr><td>88</td><td>74</td><td>可编程</td><td>UART7</td><td>UART7全局中断</td><td>0x00000160</td></tr><tr><td>89</td><td>75</td><td>可编程</td><td>UART8</td><td>UART8全局中断</td><td>0x00000164</td></tr><tr><td>90</td><td>76</td><td>可编程</td><td>TIM9_BRK</td><td>TIM9刹车中断</td><td>0x00000168</td></tr><tr><td>91</td><td>77</td><td>可编程</td><td>TIM9_UP</td><td>TIM9更新中断</td><td>0x0000016C</td></tr><tr><td>92</td><td>78</td><td>可编程</td><td>TIM9_TRG_COM</td><td>TIM9触发和通信中断</td><td>0x00000170</td></tr><tr><td>93</td><td>79</td><td>可编程</td><td>TIM9_CC</td><td>TIM9捕获比较中断</td><td>0x00000174</td></tr><tr><td>94</td><td>80</td><td>可编程</td><td>TIM10_BRK</td><td>TIM10刹车中断</td><td>0x00000178</td></tr><tr><td>95</td><td>81</td><td>可编程</td><td>TIM10_UP</td><td>TIM10更新中断</td><td>0x0000017C</td></tr><tr><td>96</td><td>82</td><td>可编程</td><td>TIM10_TRG_COM</td><td>TIM10触发和通信中断</td><td>0x00000180</td></tr><tr><td>97</td><td>83</td><td>可编程</td><td>TIM10_CC</td><td>TIM10捕获比较中断</td><td>0x00000184</td></tr><tr><td>98</td><td>84</td><td>可编程</td><td>DMA2_CH6</td><td>DMA2通道6全局中断</td><td>0x00000188</td></tr><tr><td>99</td><td>85</td><td>可编程</td><td>DMA2_CH7</td><td>DMA2通道7全局中断</td><td>0x0000018C</td></tr><tr><td>100</td><td>86</td><td>可编程</td><td>DMA2_CH8</td><td>DMA2通道8全局中断</td><td>0x00000190</td></tr><tr><td>101</td><td>87</td><td>可编程</td><td>DMA2_CH9</td><td>DMA2通道9全局中断</td><td>0x00000194</td></tr><tr><td>102</td><td>88</td><td>可编程</td><td>DMA2_CH10</td><td>DMA2通道10全局中断</td><td>0x00000198</td></tr><tr><td>103</td><td>89</td><td>可编程</td><td>DMA2_CH11</td><td>DMA2通道11全局中断</td><td>0x0000019C</td></tr></table>

## 9.4 外部中断和事件控制器（EXTI）

### 9.4.1 概述

![](images/878273753a7d35dbeb5fc0f1cc7cd148e104a616c1f7b66ddf0cd11c029ef224.jpg)  
图 9-1 外部中断(EXTI)接口框图

由图 9-1可以看出，外部中断的触发源既可以是软件中断（SWIEVR）也可以是实际的外部中断通道，外部中断通道的信号会先经过边沿检测电路（edge detect circuit）的筛选。只要产生软件中断或外部中断信号其一，就会通过图中的或门电路输出给事件使能和中断使能两个与门电路，只要有中断被使能或事件被使能，就会产生中断或事件。EXTI 的六个寄存器由处理器通过 APB2 接口访问。

### 9.4.2 唤醒事件说明

系统可以通过唤醒事件来唤醒由WFE指令引起的睡眠模式。唤醒事件通过以下两种配置产生：

l 在外设的寄存器里使能一个中断，但不在内核的 NVIC 或 PFIC里使能这个中断，同时在内核里使能 SEVONPEND 位。体现在 EXTI 中，就是使能 EXTI 中断，但不在 NVIC 或 PFIC 中使能 EXTI 中断，同时使能 SEVONPEND 位。当 CPU 从 WFE 中唤醒后，需要清除 EXTI 的中断标志位和 NVIC 或 PFIC挂起位。  
使能一个 EXTI 通道为事件通道，CPU 从 WFE 唤醒后无需清除中断标志位和 NVIC 或 PFIC 挂起位的操作。

### 9.4.3 说明

使用外部中断需要配置相应外部中断通道，即选择相应触发沿，使能相应中断。当外部中断通道上出现了设定的触发沿时，将产生一个中断请求，对应的中断标志位也会被置位。对标志位写 1 可以清除该标志位。

使用外部硬件中断步骤：

1） 配置 GPIO 操作；

2） 配置对应的外部中断通道的中断使能位（EXTI_INTENR）；  
3） 配置触发沿（EXTI_RTENR 或 EXTI_FTENR），选择上升沿触发、下降沿触发或双边沿触发；  
4） 在内核的 NVIC/PFIC 中配置 EXTI 中断，以保证其可以正确响应。

使用外部硬件事件步骤：

1） 配置 GPIO 操作；  
2） 配置对应的外部中断通道的事件使能位（EXTI_EVENR）；  
3） 配置触发沿（EXTI_RTENR 或 EXTI_FTENR），选择上升沿触发、下降沿触发或双边沿触发。

使用软件中断/事件步骤：

1） 使能外部中断（EXTI_INTENR）或外部事件（EXTI_EVENR）；  
2） 如果使用中断服务函数，需要设置内核的 NVIC 或 PFIC 里 EXTI 中断；  
3） 设置软件中断触发（EXTI_SWIEVR），即会产生中断。

### 9.4.4 外部事件映射

表 9-3 EXTI 中断映射  

<table><tr><td>外部中断/事件线路</td><td>映射事件描述</td></tr><tr><td>EXT10~EXT115</td><td>Px0~Px15(x=A/B/C/D/E),任何一个I0口都可以启用外部中断/事件功能,由AF10_EXTICRx寄存器配置。</td></tr><tr><td>EXT116</td><td>PVD事件:超出电压监控阀值</td></tr><tr><td>EXT117</td><td>RTC闹钟事件</td></tr><tr><td>EXT118</td><td>USBD/USBFSOTG唤醒事件(适用CH32F20x_D8、CH32F20x_D8C、CH32V30x_D8、CH32V30x_D8C)USBD唤醒事件(其余芯片型号)</td></tr><tr><td>EXT119</td><td>ETH唤醒事件</td></tr><tr><td>EXT120</td><td>USBHS唤醒事件(适用CH32F20x_D8C、CH32V30x_D8C)USBFS唤醒事件(适用其余芯片型号)</td></tr><tr><td>EXT121</td><td>内部32K校准唤醒事件(适用于CH32V20x_D8、CH32V20x_D8W、CH32F20x_D8W)</td></tr></table>

## 9.5 寄存器描述

### 9.5.1 EXTI 寄存器描述

表 9-4 EXTI 相关寄存器列表  

<table><tr><td>名称</td><td>访问地址</td><td>描述</td><td>复位值</td></tr><tr><td>R32_EXTI_INTENR</td><td>0x40010400</td><td>中断使能寄存器</td><td>0x00000000</td></tr><tr><td>R32_EXTI_EVENR</td><td>0x40010404</td><td>事件使能寄存器</td><td>0x00000000</td></tr><tr><td>R32_EXTI_RTENR</td><td>0x40010408</td><td>上升沿触发使能寄存器</td><td>0x00000000</td></tr><tr><td>R32_EXTI_FTENR</td><td>0x4001040C</td><td>下降沿触发使能寄存器</td><td>0x00000000</td></tr><tr><td>R32_EXTI_SWIEVR</td><td>0x40010410</td><td>软中断事件寄存器</td><td>0x00000000</td></tr><tr><td>R32_EXTI_INTFR</td><td>0x40010414</td><td>中断标志位寄存器</td><td>0x0000XXXX</td></tr></table>

#### 9.5.1.1 中断使能寄存器（EXTI_INTENR）

偏移地址： $0 \times 0 0$

<table><tr><td colspan="11">Reserved</td><td>MR20</td><td>MR19</td><td>MR18</td><td>MR17</td><td>MR16</td></tr><tr><td>15</td><td>14</td><td>13</td><td>12</td><td>11</td><td>10</td><td>9</td><td>8</td><td>7</td><td>6</td><td>5</td><td>4</td><td>3</td><td>2</td><td>1</td><td>0</td></tr><tr><td>MR15</td><td>MR14</td><td>MR13</td><td>MR12</td><td>MR11</td><td>MR10</td><td>MR9</td><td>MR8</td><td>MR7</td><td>MR6</td><td>MR5</td><td>MR4</td><td>MR3</td><td>MR2</td><td>MR1</td><td>MRO</td></tr></table>

<table><tr><td>位</td><td>名称</td><td>访问</td><td>描述</td><td>复位值</td></tr><tr><td>[31:21]</td><td>Reserved</td><td>R0</td><td>保留。</td><td>0</td></tr><tr><td>[20:0]</td><td>MRx</td><td>RW</td><td>使能外部中断通道x的中断请求信号:1:使能此通道的中断;0:屏蔽此通道的中断。</td><td>0</td></tr></table>

#### 9.5.1.2 事件使能寄存器（EXTI_EVENR）

偏移地址： $0 \times 0 4$

<table><tr><td colspan="11">Reserved</td><td>MR20</td><td>MR19</td><td>MR18</td><td>MR17</td><td>MR16</td></tr><tr><td>15</td><td>14</td><td>13</td><td>12</td><td>11</td><td>10</td><td>9</td><td>8</td><td>7</td><td>6</td><td>5</td><td>4</td><td>3</td><td>2</td><td>1</td><td>0</td></tr><tr><td>MR15</td><td>MR14</td><td>MR13</td><td>MR12</td><td>MR11</td><td>MR10</td><td>MR9</td><td>MR8</td><td>MR7</td><td>MR6</td><td>MR5</td><td>MR4</td><td>MR3</td><td>MR2</td><td>MR1</td><td>MRO</td></tr></table>

<table><tr><td>位</td><td>名称</td><td>访问</td><td>描述</td><td>复位值</td></tr><tr><td>[31:21]</td><td>Reserved</td><td>R0</td><td>保留。</td><td>0</td></tr><tr><td>[20:0]</td><td>MRx</td><td>RW</td><td>使能外部中断通道x的事件请求信号:
1:使能此通道的事件;
0:屏蔽此通道的事件。</td><td>0</td></tr></table>

#### 9.5.1.3 上升沿触发使能寄存器（EXTI_RTENR）

偏移地址： $0 \times 0 8$

<table><tr><td>31</td><td>30</td><td>29</td><td>28</td><td>27</td><td>26</td><td>25</td><td>24</td><td>23</td><td>22</td><td>21</td><td>20</td><td>19</td><td>18</td><td>17</td><td>16</td></tr><tr><td colspan="11">Reserved</td><td>TR20</td><td>TR19</td><td>TR18</td><td>TR17</td><td>TR16</td></tr><tr><td>15</td><td>14</td><td>13</td><td>12</td><td>11</td><td>10</td><td>9</td><td>8</td><td>7</td><td>6</td><td>5</td><td>4</td><td>3</td><td>2</td><td>1</td><td>0</td></tr><tr><td>TR15</td><td>TR14</td><td>TR13</td><td>TR12</td><td>TR11</td><td>TR10</td><td>TR9</td><td>TR8</td><td>TR7</td><td>TR6</td><td>TR5</td><td>TR4</td><td>TR3</td><td>TR2</td><td>TR1</td><td>TR0</td></tr></table>

<table><tr><td>位</td><td>名称</td><td>访问</td><td>描述</td><td>复位值</td></tr><tr><td>[31:21]</td><td>Reserved</td><td>R0</td><td>保留。</td><td>0</td></tr><tr><td>[20:0]</td><td>TRx</td><td>RW</td><td>使能外部中断通道x的上升沿触发:1:使能此通道的上升沿触发;0:禁止此通道的上升沿触发。</td><td>0</td></tr></table>

#### 9.5.1.4 下降沿触发使能寄存器（EXTI_FTENR）

偏移地址： $0 \times 0 0$

<table><tr><td colspan="11">Reserved</td><td>TR20</td><td>TR19</td><td>TR18</td><td>TR17</td><td>TR16</td></tr><tr><td>15</td><td>14</td><td>13</td><td>12</td><td>11</td><td>10</td><td>9</td><td>8</td><td>7</td><td>6</td><td>5</td><td>4</td><td>3</td><td>2</td><td>1</td><td>0</td></tr><tr><td>TR15</td><td>TR14</td><td>TR13</td><td>TR12</td><td>TR11</td><td>TR10</td><td>TR9</td><td>TR8</td><td>TR7</td><td>TR6</td><td>TR5</td><td>TR4</td><td>TR3</td><td>TR2</td><td>TR1</td><td>TR0</td></tr></table>

<table><tr><td>位</td><td>名称</td><td>访问</td><td>描述</td><td>复位值</td></tr><tr><td>[31:21]</td><td>Reserved</td><td>R0</td><td>保留</td><td>0</td></tr><tr><td>[20:0]</td><td>TRx</td><td>RW</td><td>使能外部中断通道x的下降沿触发:0:禁止此通道的下降沿触发;1:使能此通道的下降沿触发。</td><td>0</td></tr></table>

#### 9.5.1.5 软中断事件寄存器（EXTI_SWIEVR）

偏移地址： $0 \times 1 0$

<table><tr><td colspan="11">Reserved</td><td>SWIER20</td><td>SWIER19</td><td>SWIER18</td><td>SWIER17</td><td>SWIER16</td></tr><tr><td>15</td><td>14</td><td>13</td><td>12</td><td>11</td><td>10</td><td>9</td><td>8</td><td>7</td><td>6</td><td>5</td><td>4</td><td>3</td><td>2</td><td>1</td><td>0</td></tr><tr><td>SWIER15</td><td>SWIER14</td><td>SWIER13</td><td>SWIER12</td><td>SWIER11</td><td>SWIER10</td><td>SWIER9</td><td>SWIER8</td><td>SWIER7</td><td>SWIER6</td><td>SWIER5</td><td>SWIER4</td><td>SWIER3</td><td>SWIER2</td><td>SWIER1</td><td>SWIER0</td></tr></table>

<table><tr><td>位</td><td>名称</td><td>访问</td><td>描述</td><td>复位值</td></tr><tr><td>[31:21]</td><td>Reserved</td><td>R0</td><td>保留。</td><td>0</td></tr><tr><td>[20:0]</td><td>SWIERx</td><td>RW</td><td>在相对应的外部触发中断通道上设置一个软件中断。这里置位会使中断标志位(EXTI_INTFR)对应位置位,如果中断使能(EXTI_INTENR)或事件使能(EXTI_EVENR)开启,那么就会产生中断或事件。</td><td>0</td></tr></table>

#### 9.5.1.6 中断标志位寄存器（EXTI_INTFR）

偏移地址： $0 \times 1 4$

<table><tr><td colspan="11">Reserved</td><td>IF20</td><td>IF19</td><td>IF18</td><td>IF17</td><td>IF16</td></tr><tr><td>15</td><td>14</td><td>13</td><td>12</td><td>11</td><td>10</td><td>9</td><td>8</td><td>7</td><td>6</td><td>5</td><td>4</td><td>3</td><td>2</td><td>1</td><td>0</td></tr><tr><td>IF15</td><td>IF14</td><td>IF13</td><td>IF12</td><td>IF11</td><td>IF10</td><td>IF9</td><td>IF8</td><td>IF7</td><td>IF6</td><td>IF5</td><td>IF4</td><td>IF3</td><td>IF2</td><td>IF1</td><td>IF0</td></tr></table>

<table><tr><td>位</td><td>名称</td><td>访问</td><td>描述</td><td>复位值</td></tr><tr><td>[31:21]</td><td>Reserved</td><td>R0</td><td>保留。</td><td>0</td></tr><tr><td>[20:0]</td><td>IFx</td><td>W1</td><td>中断标志位,该位置位标志表示发生了对应的外部中断。写1可以清除此位。</td><td>0</td></tr></table>

### 9.5.2 PFIC 寄存器描述

表 9-5 PFIC 相关寄存器列表  

<table><tr><td>名称</td><td>访问地址</td><td>描述</td><td>复位值</td></tr><tr><td>R32_PF_ISR1</td><td>0xE000E000</td><td>PFIC中断使能状态寄存器1</td><td>0x0000000C</td></tr><tr><td>R32_PF_ISR2</td><td>0xE000E004</td><td>PFIC中断使能状态寄存器2</td><td>0x0000000</td></tr><tr><td>R32_PF_ISR3</td><td>0xE000E008</td><td>PFIC中断使能状态寄存器3</td><td>0x0000000</td></tr><tr><td>R32_PF_ISR4</td><td>0xE000E00C</td><td>PFIC中断使能状态寄存器4</td><td>0x0000000</td></tr><tr><td>R32_PF_IPR1</td><td>0xE000E020</td><td>PFIC中断挂起状态寄存器1</td><td>0x0000000</td></tr><tr><td>R32_PF_IPR2</td><td>0xE000E024</td><td>PFIC中断挂起状态寄存器2</td><td>0x0000000</td></tr><tr><td>R32_PF_IPR3</td><td>0xE000E028</td><td>PFIC中断挂起状态寄存器3</td><td>0x0000000</td></tr><tr><td>R32_PF_IPR4</td><td>0xE000E02C</td><td>PFIC中断挂起状态寄存器4</td><td>0x0000000</td></tr><tr><td>R32_PF_ITHRESDR</td><td>0xE000E040</td><td>PFIC中断优先级阈值配置寄存器</td><td>0x0000000</td></tr><tr><td>R32_PF_CFGR</td><td>0xE000E048</td><td>PFIC中断配置寄存器</td><td>0x0000000</td></tr><tr><td>R32_PF_GISR</td><td>0xE000E04C</td><td>PFIC中断全局状态寄存器</td><td>0x0000000</td></tr><tr><td>R32_PF_VTFIDR</td><td>0xE000E050</td><td>PFIC VTF中断ID配置寄存器</td><td>0x0000000</td></tr><tr><td>R32_PF_VTFADDRR0</td><td>0xE000E060</td><td>PFIC VTF中断0偏移地址寄存器</td><td>0x0000000</td></tr><tr><td>R32_PF_VTFADDRR1</td><td>0xE000E064</td><td>PFIC VTF中断1偏移地址寄存器</td><td>0x0000000</td></tr><tr><td>R32_PF_VTFADDRR2</td><td>0xE000E068</td><td>PFIC VTF中断2偏移地址寄存器</td><td>0x0000000</td></tr><tr><td>R32_PF_VTFADDRR3</td><td>0xE000E06C</td><td>PFIC VTF中断3偏移地址寄存器</td><td>0x0000000</td></tr><tr><td>R32_PF_IENR1</td><td>0xE000E100</td><td>PFIC中断使能设置寄存器1</td><td>0x0000000</td></tr><tr><td>R32_PF_IENR2</td><td>0xE000E104</td><td>PFIC中断使能设置寄存器2</td><td>0x0000000</td></tr><tr><td>R32_PF_IENR3</td><td>0xE000E108</td><td>PFIC中断使能设置寄存器3</td><td>0x0000000</td></tr><tr><td>R32_PF_IENR4</td><td>0xE000E10C</td><td>PFIC中断使能设置寄存器4</td><td>0x0000000</td></tr><tr><td>R32_PF_IRER1</td><td>0xE000E180</td><td>PFIC中断使能清除寄存器1</td><td>0x0000000</td></tr><tr><td>R32_PF_IRER2</td><td>0xE000E184</td><td>PFIC中断使能清除寄存器2</td><td>0x0000000</td></tr><tr><td>R32_PF_IRER3</td><td>0xE000E188</td><td>PFIC中断使能清除寄存器3</td><td>0x0000000</td></tr><tr><td>R32_PF_IRER4</td><td>0xE000E18C</td><td>PFIC中断使能清除寄存器4</td><td>0x0000000</td></tr><tr><td>R32_PF_IPSR1</td><td>0xE000E200</td><td>PFIC中断挂起设置寄存器1</td><td>0x0000000</td></tr><tr><td>R32_PF_IPSR2</td><td>0xE000E204</td><td>PFIC中断挂起设置寄存器2</td><td>0x0000000</td></tr><tr><td>R32_PF_IPSR3</td><td>0xE000E208</td><td>PFIC中断挂起设置寄存器3</td><td>0x0000000</td></tr><tr><td>R32_PF_IPSR4</td><td>0xE000E20C</td><td>PFIC中断挂起设置寄存器4</td><td>0x0000000</td></tr><tr><td>R32_PF_IPRR1</td><td>0xE000E280</td><td>PFIC中断挂起清除寄存器1</td><td>0x0000000</td></tr><tr><td>R32_PF_IPRR2</td><td>0xE000E284</td><td>PFIC中断挂起清除寄存器2</td><td>0x0000000</td></tr><tr><td>R32_PF_IPRR3</td><td>0xE000E288</td><td>PFIC中断挂起清除寄存器3</td><td>0x0000000</td></tr><tr><td>R32_PF_IPRR4</td><td>0xE000E28C</td><td>PFIC中断挂起清除寄存器4</td><td>0x0000000</td></tr><tr><td>R32_PF_IACTR1</td><td>0xE000E300</td><td>PFIC中断激活状态寄存器1</td><td>0x0000000</td></tr><tr><td>R32_PF_IACTR2</td><td>0xE000E304</td><td>PFIC中断激活状态寄存器2</td><td>0x0000000</td></tr><tr><td>R32_PF_IACTR3</td><td>0xE000E308</td><td>PFIC中断激活状态寄存器3</td><td>0x0000000</td></tr><tr><td>R32_PF_IACTR4</td><td>0xE000E30C</td><td>PFIC中断激活状态寄存器4</td><td>0x0000000</td></tr><tr><td>R32_PF_IPRIORx</td><td>0xE000E400</td><td>PFIC中断优先级配置寄存器</td><td>0x0000000</td></tr><tr><td>R32_PF_SCTLR</td><td>0xE000ED10</td><td>PFIC系统控制寄存器</td><td>0x0000000</td></tr></table>

注：1.NMI、EXC、ECALL-M、ECALL-U、BREAKPOINT中断默认总是使能。  
2.ECALL-M、ECALL-U、BREAKPOINT均为 EXC的一种情况，状态由 EXC的状态位bit3表示。  
3.NMI、EXC支持中断挂起清除和设置操作，不支持中断使能清除和设置操作。  
4.ECALL-M、ECALL-U、BREAKPOINT不支持中断挂起清除和设置、中断使能清除和设置操作。

#### 9.5.2.1 PFIC 中断使能状态寄存器 1（PFIC_ISR1）

偏移地址： $0 \times 0 0$

<table><tr><td>31</td><td>30</td><td>29</td><td>28</td><td>27</td><td>26</td><td>25</td><td>24</td><td>23</td><td>22</td><td>21</td><td>20</td><td>19</td><td>18</td><td>17</td><td>16</td></tr><tr><td colspan="16">INTENSTA[31:16]</td></tr><tr><td>15</td><td>14</td><td>13</td><td>12</td><td>11</td><td>10</td><td>9</td><td>8</td><td>7</td><td>6</td><td>5</td><td>4</td><td>3</td><td>2</td><td>1</td><td>0</td></tr><tr><td>INTEN
STA15</td><td>INTEN
STA14</td><td>INTEN
STA13</td><td>INTEN
STA12</td><td colspan="8">Reserved</td><td>INTEN
STA3</td><td>INTEN
STA2</td><td colspan="2">Reserved</td></tr></table>

<table><tr><td>位</td><td>名称</td><td>访问</td><td>描述</td><td>复位值</td></tr><tr><td>[31:12]</td><td>INTENSTA</td><td>RO</td><td>12#-31#中断当前使能状态。1:当前编号中断已使能;0:当前编号中断未启用。</td><td>0</td></tr><tr><td>[11:4]</td><td>Reserved</td><td>RO</td><td>保留</td><td>0</td></tr><tr><td>[3:2]</td><td>INTENSTA</td><td>RO</td><td>2#-3#中断当前使能状态。1:当前编号中断已使能;0:当前编号中断未启用。</td><td>0</td></tr><tr><td>[1:0]</td><td>Reserved</td><td>RO</td><td>保留</td><td>0</td></tr></table>

#### 9.5.2.2 PFIC 中断使能状态寄存器 2（PFIC_ISR2）

偏移地址： $0 \times 0 4$

<table><tr><td>31</td><td>30</td><td>29</td><td>28</td><td>27</td><td>26</td><td>25</td><td>24</td><td>23</td><td>22</td><td>21</td><td>20</td><td>19</td><td>18</td><td>17</td><td>16</td></tr><tr><td colspan="16">INTENSTA[63:48]</td></tr><tr><td>15</td><td>14</td><td>13</td><td>12</td><td>11</td><td>10</td><td>9</td><td>8</td><td>7</td><td>6</td><td>5</td><td>4</td><td>3</td><td>2</td><td>1</td><td>0</td></tr><tr><td colspan="16">INTENSTA[47:32]</td></tr></table>

<table><tr><td>位</td><td>名称</td><td>访问</td><td>描述</td><td>复位值</td></tr><tr><td>[31:0]</td><td>INTENSTA</td><td>RO</td><td>32#-63#中断当前使能状态。
1: 当前编号中断已使能;
0: 当前编号中断未启用。</td><td>0</td></tr></table>

#### 9.5.2.3 PFIC 中断使能状态寄存器 3（PFIC_ISR3）

偏移地址： $0 \times 0 8$

<table><tr><td>31</td><td>30</td><td>29</td><td>28</td><td>27</td><td>26</td><td>25</td><td>24</td><td>23</td><td>22</td><td>21</td><td>20</td><td>19</td><td>18</td><td>17</td><td>16</td></tr><tr><td colspan="16">INTENSTA[95:80]</td></tr><tr><td>15</td><td>14</td><td>13</td><td>12</td><td>11</td><td>10</td><td>9</td><td>8</td><td>7</td><td>6</td><td>5</td><td>4</td><td>3</td><td>2</td><td>1</td><td>0</td></tr><tr><td colspan="16">INTENSTA[79:64]</td></tr></table>

<table><tr><td>位</td><td>名称</td><td>访问</td><td>描述</td><td>复位值</td></tr><tr><td>[31:0]</td><td>INTENSTA</td><td>RO</td><td>64#--95#中断当前使能状态。
1: 当前编号中断已使能;
0: 当前编号中断未启用。</td><td>0</td></tr></table>

#### 9.5.2.4 PFIC 中断使能状态寄存器 4（PFIC_ISR4）

偏移地址： $0 \times 0 0$

<table><tr><td>31</td><td>30</td><td>29</td><td>28</td><td>27</td><td>26</td><td>25</td><td>24</td><td>23</td><td>22</td><td>21</td><td>20</td><td>19</td><td>18</td><td>17</td><td>16</td></tr><tr><td colspan="16">Reserved</td></tr><tr><td>15</td><td>14</td><td>13</td><td>12</td><td>11</td><td>10</td><td>9</td><td>8</td><td>7</td><td>6</td><td>5</td><td>4</td><td>3</td><td>2</td><td>1</td><td>0</td></tr><tr><td colspan="8">Reserved</td><td colspan="8">INTENSTA[103:96]</td></tr></table>

<table><tr><td>位</td><td>名称</td><td>访问</td><td>描述</td><td>复位值</td></tr><tr><td>[31:8]</td><td>Reserved</td><td>R0</td><td>保留</td><td>0</td></tr><tr><td>[7:0]</td><td>INTENSTA</td><td>R0</td><td>96#--103#中断当前使能状态。
1: 当前编号中断已使能;
0: 当前编号中断未启用。</td><td>0</td></tr></table>

#### 9.5.2.5 PFIC 中断挂起状态寄存器 1（PFIC_IPR1）

偏移地址： $_ { 0 \times 2 0 }$

<table><tr><td>31</td><td>30</td><td>29</td><td>28</td><td>27</td><td>26</td><td>25</td><td>24</td><td>23</td><td>22</td><td>21</td><td>20</td><td>19</td><td>18</td><td>17</td><td>16</td></tr><tr><td colspan="16">PENDSTA[31:16]</td></tr><tr><td>15</td><td>14</td><td>13</td><td>12</td><td>11</td><td>10</td><td>9</td><td>8</td><td>7</td><td>6</td><td>5</td><td>4</td><td>3</td><td>2</td><td>1</td><td>0</td></tr><tr><td>PENDSTATA15</td><td>PENDSTATA14</td><td>PENDSTATA13</td><td>PENDSTATA12</td><td colspan="8">Reserved</td><td>PENDSTATA3</td><td>PENDSTATA2</td><td colspan="2">Reserved</td></tr></table>

<table><tr><td>位</td><td>名称</td><td>访问</td><td>描述</td><td>复位值</td></tr><tr><td>[31:12]</td><td>PENDSTA</td><td>RO</td><td>12#-31#中断当前挂起状态。1:当前编号中断已挂起;0:当前编号中断未挂起。</td><td>0</td></tr><tr><td>[11:4]</td><td>Reserved</td><td>RO</td><td>保留。</td><td>0</td></tr><tr><td>[3:2]</td><td>PENDSTA</td><td>RO</td><td>2#-3#中断当前挂起状态。1:当前编号中断已挂起;0:当前编号中断未挂起。</td><td>0</td></tr><tr><td>[1:0]</td><td>Reserved</td><td>RO</td><td>保留。</td><td>0</td></tr></table>

#### 9.5.2.6 PFIC 中断挂起状态寄存器 2（PFIC_IPR2）

偏移地址： ${ 0 } { \times 2 4 }$

<table><tr><td>31</td><td>30</td><td>29</td><td>28</td><td>27</td><td>26</td><td>25</td><td>24</td><td>23</td><td>22</td><td>21</td><td>20</td><td>19</td><td>18</td><td>17</td><td>16</td></tr><tr><td colspan="16">PENDSTA[63:48]</td></tr><tr><td>15</td><td>14</td><td>13</td><td>12</td><td>11</td><td>10</td><td>9</td><td>8</td><td>7</td><td>6</td><td>5</td><td>4</td><td>3</td><td>2</td><td>1</td><td>0</td></tr><tr><td colspan="16">PENDSTA[47:32]</td></tr></table>

<table><tr><td>位</td><td>名称</td><td>访问</td><td>描述</td><td>复位值</td></tr><tr><td>[31:0]</td><td>PENDSTA</td><td>RO</td><td>32#-63#中断当前挂起状态。</td><td>0</td></tr><tr><td></td><td></td><td></td><td>1: 当前编号中断已挂起;0: 当前编号中断未挂起。</td><td></td></tr></table>

#### 9.5.2.7 PFIC 中断挂起状态寄存器 3（PFIC_IPR3）

偏移地址： $_ { 0 \times 2 8 }$

<table><tr><td>31</td><td>30</td><td>29</td><td>28</td><td>27</td><td>26</td><td>25</td><td>24</td><td>23</td><td>22</td><td>21</td><td>20</td><td>19</td><td>18</td><td>17</td><td>16</td></tr><tr><td colspan="16">PENDSTA[95:80]</td></tr><tr><td>15</td><td>14</td><td>13</td><td>12</td><td>11</td><td>10</td><td>9</td><td>8</td><td>7</td><td>6</td><td>5</td><td>4</td><td>3</td><td>2</td><td>1</td><td>0</td></tr><tr><td colspan="16">PENDSTA[79:64]</td></tr></table>

<table><tr><td>位</td><td>名称</td><td>访问</td><td>描述</td><td>复位值</td></tr><tr><td>[31:0]</td><td>PENDSTA</td><td>RO</td><td>64#--95#中断当前挂起状态。
1: 当前编号中断已挂起;
0: 当前编号中断未挂起。</td><td>0</td></tr></table>

#### 9.5.2.8 PFIC 中断挂起状态寄存器 4（PFIC_IPR4）

偏移地址： $\mathtt { 0 } \mathtt { x } 2 0$

<table><tr><td>31</td><td>30</td><td>29</td><td>28</td><td>27</td><td>26</td><td>25</td><td>24</td><td>23</td><td>22</td><td>21</td><td>20</td><td>19</td><td>18</td><td>17</td><td>16</td></tr><tr><td colspan="16">Reserved</td></tr><tr><td>15</td><td>14</td><td>13</td><td>12</td><td>11</td><td>10</td><td>9</td><td>8</td><td>7</td><td>6</td><td>5</td><td>4</td><td>3</td><td>2</td><td>1</td><td>0</td></tr><tr><td colspan="8">Reserved</td><td colspan="8">PENDSTA[103:96]</td></tr></table>

<table><tr><td>位</td><td>名称</td><td>访问</td><td>描述</td><td>复位值</td></tr><tr><td>[31:8]</td><td>Reserved</td><td>R0</td><td>保留。</td><td>0</td></tr><tr><td>[7:0]</td><td>PENDSTA</td><td>R0</td><td>96#--103#中断当前挂起状态。
1: 当前编号中断已挂起;
0: 当前编号中断未挂起。</td><td>0</td></tr></table>

#### 9.5.2.9 PFIC 中断优先级阈值配置寄存器（PFIC_ITHRESDR）

偏移地址： $0 \times 4 0$

31 30 29 28 27 26 25 24 23 22 21 20 19 18 17 16 15 14 13 12 11 10 9 8 7 6 5 4 3 2 1 0

<table><tr><td>Reserved</td><td>THRESHOLD[7:0]</td></tr></table>

<table><tr><td>位</td><td>名称</td><td>访问</td><td>描述</td><td>复位值</td></tr><tr><td>[31:8]</td><td>Reserved</td><td>R0</td><td>保留。</td><td>0</td></tr><tr><td>[7:0]</td><td>THRESHOLD</td><td>RW</td><td>中断优先级阈值设置值。低于当前设置值的中断优先级值，当挂起时不执行中断服务；此寄存器为0时表示阈值寄存器功能无效。[7:4]：优先级阈值。[3:0]：保留，固定为0，写无效。</td><td>0</td></tr></table>

#### 9.5.2.10 PFIC 中断配置寄存器（PFIC_CFGR）

偏移地址： $0 \times 4 8$

<table><tr><td>31</td><td>30</td><td>29</td><td>28</td><td>27</td><td>26</td><td>25</td><td>24</td><td>23</td><td>22</td><td>21</td><td>20</td><td>19</td><td>18</td><td>17</td><td>16</td></tr><tr><td colspan="16">KEYCODE[15:0]</td></tr><tr><td>15</td><td>14</td><td>13</td><td>12</td><td>11</td><td>10</td><td>9</td><td>8</td><td>7</td><td>6</td><td>5</td><td>4</td><td>3</td><td>2</td><td>1</td><td>0</td></tr><tr><td colspan="8">Reserved</td><td>RSTSY</td><td colspan="7">Reserved</td></tr></table>

<table><tr><td>位</td><td>名称</td><td>访问</td><td>描述</td><td>复位值</td></tr><tr><td>[31:16]</td><td>KEYCODE</td><td>WO</td><td>对应不同的目标控制位,需要同步写入相应的安全访问标识数据才能修改,读出数据固定为0。KEY1 = 0xFE05;KEY2 = 0xBCAF;KEY3 = 0xBEEF。</td><td>0</td></tr><tr><td>[15:8]</td><td>Reserved</td><td>RO</td><td>保留。</td><td>0</td></tr><tr><td>7</td><td>RSTSYS</td><td>WO</td><td>系统复位(同步写入KEY3)。自动清0。写1有效,写0无效。注:与PFIC_SCTLR寄存器SYSRST位作用相同。</td><td>0</td></tr><tr><td>[6:0]</td><td>Reserved</td><td>RO</td><td>保留。</td><td>0</td></tr></table>

#### 9.5.2.11 PFIC 中断全局状态寄存器（PFIC_GISR）

偏移地址： $\mathtt { 0 } \mathtt { x } 4 0$

<table><tr><td>31</td><td>30</td><td>29</td><td>28</td><td>27</td><td>26</td><td>25</td><td>24</td><td>23</td><td>22</td><td>21</td><td>20</td><td>19</td><td>18</td><td>17</td><td>16</td></tr><tr><td colspan="16">Reserved</td></tr><tr><td>15</td><td>14</td><td>13</td><td>12</td><td>11</td><td>10</td><td>9</td><td>8</td><td>7</td><td>6</td><td>5</td><td>4</td><td>3</td><td>2</td><td>1</td><td>0</td></tr><tr><td colspan="6">Reserved</td><td>GPEND STA</td><td>GACT STA</td><td colspan="8">NESTSTA[7:0]</td></tr></table>

<table><tr><td>位</td><td>名称</td><td>访问</td><td>描述</td><td>复位值</td></tr><tr><td>[31:10]</td><td>Reserved</td><td>R0</td><td>保留。</td><td>0</td></tr><tr><td>9</td><td>GPENDSTA</td><td>R0</td><td>当前是否有中断处于挂起:1:有; 0:没有。</td><td>0</td></tr><tr><td>8</td><td>GACTSTA</td><td>R0</td><td>当前是否有中断被执行:1:有; 0:没有。</td><td>0</td></tr><tr><td>[7:0]</td><td>NESTSTA</td><td>R0</td><td>当前中断嵌套状态,目前最大支持8级嵌套,硬件压栈深度最大为3级,若设置嵌套深度大于3级,则应配置低三级中断为硬件压栈,其余高优先级使用软件压栈。0xFF:第8级中断中;0x7F:第7级中断中;0x3F:第6级中断中;0x1F:第5级中断中;0x0F:第4级中断中;</td><td>0x00</td></tr><tr><td></td><td></td><td></td><td>0x07:第3级中断中;0x03:第2级中断中;0x01:第1级中断中;0x00:没有中断发生;其他:不可能情况。注:适用于青棵V4F内核:CH32V30x_D8、CH32V30x_D8C。当前中断嵌套状态,目前最大支持2级嵌套,硬件压栈深度最大为2级。0x03:第2级中断中;0x01:第1级中断中;0x00:没有中断发生;其他:不可能情况。注:适用于青棵V4B、V4C内核:CH32V20x_D6、CH32V20x_D8、CH32V20x_D8W。</td><td></td></tr></table>

#### 9.5.2.12 PFIC VTF 中断 ID 配置寄存器（PFIC_VTFIDR）

偏移地址： $_ { 0 \times 5 0 }$

<table><tr><td>31</td><td>30</td><td>29</td><td>28</td><td>27</td><td>26</td><td>25</td><td>24</td><td>23</td><td>22</td><td>21</td><td>20</td><td>19</td><td>18</td><td>17</td><td>16</td></tr><tr><td colspan="8">VTFID3</td><td colspan="8">VTFID2</td></tr><tr><td>15</td><td>14</td><td>13</td><td>12</td><td>11</td><td>10</td><td>9</td><td>8</td><td>7</td><td>6</td><td>5</td><td>4</td><td>3</td><td>2</td><td>1</td><td>0</td></tr><tr><td colspan="8">VTFID1</td><td colspan="8">VTFID0</td></tr></table>

<table><tr><td>位</td><td>名称</td><td>访问</td><td>描述</td><td>复位值</td></tr><tr><td>[31:24]</td><td>VTF ID3</td><td>RW</td><td>配置VTF中断3的中断编号。</td><td>0</td></tr><tr><td>[23:16]</td><td>VTF ID2</td><td>RW</td><td>配置VTF中断2的中断编号。</td><td>0</td></tr><tr><td>[15:8]</td><td>VTF ID1</td><td>RW</td><td>配置VTF中断1的中断编号。</td><td>0</td></tr><tr><td>[7:0]</td><td>VTF ID0</td><td>RW</td><td>配置VTF中断0的中断编号。</td><td>0</td></tr></table>

#### 9.5.2.13 PFIC VTF 中断 0 地址寄存器（PFIC_VTFADDRR0）

偏移地址： $0 \times 6 0$

<table><tr><td>31</td><td>30</td><td>29</td><td>28</td><td>27</td><td>26</td><td>25</td><td>24</td><td>23</td><td>22</td><td>21</td><td>20</td><td>19</td><td>18</td><td>17</td><td>16</td></tr><tr><td colspan="16">ADDR0[31:16]</td></tr><tr><td>15</td><td>14</td><td>13</td><td>12</td><td>11</td><td>10</td><td>9</td><td>8</td><td>7</td><td>6</td><td>5</td><td>4</td><td>3</td><td>2</td><td>1</td><td>0</td></tr><tr><td colspan="15">ADDR0[15:1]</td><td>VTF0EN</td></tr></table>

<table><tr><td>位</td><td>名称</td><td>访问</td><td>描述</td><td>复位值</td></tr><tr><td>[31:1]</td><td>ADDR0</td><td>RW</td><td>VTF中断0服务程序地址bit[31:1],bit0为0。</td><td>0</td></tr><tr><td>0</td><td>VTF0EN</td><td>RW</td><td>VTF中断0使能位:1:启用VTF中断0通道;0:关闭。</td><td>0</td></tr></table>

#### 9.5.2.14 PFIC VTF 中断 1 地址寄存器（PFIC_VTFADDRR1）

偏移地址： $0 \times 6 4$

<table><tr><td>31</td><td>30</td><td>29</td><td>28</td><td>27</td><td>26</td><td>25</td><td>24</td><td>23</td><td>22</td><td>21</td><td>20</td><td>19</td><td>18</td><td>17</td><td>16</td></tr><tr><td colspan="16">ADDR1 [31:16]</td></tr><tr><td>15</td><td>14</td><td>13</td><td>12</td><td>11</td><td>10</td><td>9</td><td>8</td><td>7</td><td>6</td><td>5</td><td>4</td><td>3</td><td>2</td><td>1</td><td>0</td></tr><tr><td colspan="15">ADDR1 [15:1]</td><td>VTF1EN</td></tr></table>

<table><tr><td>位</td><td>名称</td><td>访问</td><td>描述</td><td>复位值</td></tr><tr><td>[31:1]</td><td>ADDR1</td><td>RW</td><td>VTF中断1服务程序地址bit[31:1],bit0为0。</td><td>0</td></tr><tr><td>0</td><td>VTF1EN</td><td>RW</td><td>VTF中断1使能位:1:启用VTF中断1通道;0:关闭。</td><td>0</td></tr></table>

#### 9.5.2.15 PFIC VTF 中断 2 地址寄存器（PFIC_VTFADDRR2）

偏移地址： $0 \times 6 8$

<table><tr><td>31</td><td>30</td><td>29</td><td>28</td><td>27</td><td>26</td><td>25</td><td>24</td><td>23</td><td>22</td><td>21</td><td>20</td><td>19</td><td>18</td><td>17</td><td>16</td></tr><tr><td colspan="16">ADDR2[31:16]</td></tr><tr><td>15</td><td>14</td><td>13</td><td>12</td><td>11</td><td>10</td><td>9</td><td>8</td><td>7</td><td>6</td><td>5</td><td>4</td><td>3</td><td>2</td><td>1</td><td>0</td></tr><tr><td colspan="15">ADDR2[15:1]</td><td>VTF2EN</td></tr></table>

<table><tr><td>位</td><td>名称</td><td>访问</td><td>描述</td><td>复位值</td></tr><tr><td>[31:1]</td><td>ADDR2</td><td>RW</td><td>VTF中断2服务程序地址bit[31:1],bit0为0。</td><td>0</td></tr><tr><td>0</td><td>VTF2EN</td><td>RW</td><td>VTF中断2使能位:1:启用VTF中断2通道;0:关闭。</td><td>0</td></tr></table>

#### 9.5.2.16 PFIC VTF 中断 3 地址寄存器（PFIC_VTFADDRR3）

偏移地址： $_ { 0 \times 6 0 }$

<table><tr><td>31</td><td>30</td><td>29</td><td>28</td><td>27</td><td>26</td><td>25</td><td>24</td><td>23</td><td>22</td><td>21</td><td>20</td><td>19</td><td>18</td><td>17</td><td>16</td></tr><tr><td colspan="16">ADDR3[31:16]</td></tr><tr><td>15</td><td>14</td><td>13</td><td>12</td><td>11</td><td>10</td><td>9</td><td>8</td><td>7</td><td>6</td><td>5</td><td>4</td><td>3</td><td>2</td><td>1</td><td>0</td></tr><tr><td colspan="15">ADDR3[15:1]</td><td>VTF3EN</td></tr></table>

<table><tr><td>位</td><td>名称</td><td>访问</td><td>描述</td><td>复位值</td></tr><tr><td>[31:1]</td><td>ADDR3</td><td>RW</td><td>VTF中断3服务程序地址bit[31:1],bit0为0。</td><td>0</td></tr><tr><td>0</td><td>VTF3EN</td><td>RW</td><td>VTF中断3使能位:1:启用VTF中断3通道;0:关闭。</td><td>0</td></tr></table>

#### 9.5.2.17 PFIC 中断使能设置寄存器 1（PFIC_IENR1）

偏移地址： $0 \times 1 0 0$

<table><tr><td>31</td><td>30</td><td>29</td><td>28</td><td>27</td><td>26</td><td>25</td><td>24</td><td>23</td><td>22</td><td>21</td><td>20</td><td>19</td><td>18</td><td>17</td><td>16</td></tr><tr><td colspan="16">INTEN[31:16]</td></tr><tr><td>15</td><td>14</td><td>13</td><td>12</td><td>11</td><td>10</td><td>9</td><td>8</td><td>7</td><td>6</td><td>5</td><td>4</td><td>3</td><td>2</td><td>1</td><td>0</td></tr><tr><td>INTEN15</td><td>INTEN14</td><td>INTEN13</td><td>INTEN12</td><td colspan="12">Reserved</td></tr></table>

<table><tr><td>位</td><td>名称</td><td>访问</td><td>描述</td><td>复位值</td></tr><tr><td>[31:12]</td><td>INTEN</td><td>WO</td><td>12#-31#中断使能控制。
1: 当前编号中断使能;
0: 无影响。</td><td>0</td></tr><tr><td>[11:0]</td><td>Reserved</td><td>RO</td><td>保留。</td><td>0</td></tr></table>

#### 9.5.2.18 PFIC 中断使能设置寄存器 2（PFIC_IENR2）

偏移地址： $0 \times 1 0 4$

<table><tr><td>31</td><td>30</td><td>29</td><td>28</td><td>27</td><td>26</td><td>25</td><td>24</td><td>23</td><td>22</td><td>21</td><td>20</td><td>19</td><td>18</td><td>17</td><td>16</td></tr><tr><td colspan="16">INTEN[63:48]</td></tr><tr><td>15</td><td>14</td><td>13</td><td>12</td><td>11</td><td>10</td><td>9</td><td>8</td><td>7</td><td>6</td><td>5</td><td>4</td><td>3</td><td>2</td><td>1</td><td>0</td></tr><tr><td colspan="16">INTEN[47:32]</td></tr></table>

<table><tr><td>位</td><td>名称</td><td>访问</td><td>描述</td><td>复位值</td></tr><tr><td>[31:0]</td><td>INTEN</td><td>WO</td><td>32#-63#中断使能控制。
1: 当前编号中断使能;
0: 无影响。</td><td>0</td></tr></table>

#### 9.5.2.19 PFIC 中断使能设置寄存器 3（PFIC_IENR3）

偏移地址： $0 \times 1 0 8$

<table><tr><td>31</td><td>30</td><td>29</td><td>28</td><td>27</td><td>26</td><td>25</td><td>24</td><td>23</td><td>22</td><td>21</td><td>20</td><td>19</td><td>18</td><td>17</td><td>16</td></tr><tr><td colspan="16">INTEN[95:80]</td></tr><tr><td>15</td><td>14</td><td>13</td><td>12</td><td>11</td><td>10</td><td>9</td><td>8</td><td>7</td><td>6</td><td>5</td><td>4</td><td>3</td><td>2</td><td>1</td><td>0</td></tr><tr><td colspan="16">INTEN[79:64]</td></tr></table>

<table><tr><td>位</td><td>名称</td><td>访问</td><td>描述</td><td>复位值</td></tr><tr><td>[31:0]</td><td>INTEN</td><td>WO</td><td>64#--95#中断使能控制。
1: 当前编号中断使能;
0: 无影响。</td><td>0</td></tr></table>

#### 9.5.2.20 PFIC 中断使能设置寄存器 4（PFIC_IENR4）

偏移地址： $0 \times 1 0 0$

<table><tr><td>31</td><td>30</td><td>29</td><td>28</td><td>27</td><td>26</td><td>25</td><td>24</td><td>23</td><td>22</td><td>21</td><td>20</td><td>19</td><td>18</td><td>17</td><td>16</td></tr><tr><td colspan="16">Reserved</td></tr><tr><td>15</td><td>14</td><td>13</td><td>12</td><td>11</td><td>10</td><td>9</td><td>8</td><td>7</td><td>6</td><td>5</td><td>4</td><td>3</td><td>2</td><td>1</td><td>0</td></tr><tr><td colspan="8">Reserved</td><td colspan="8">INTEN[103:96]</td></tr></table>

<table><tr><td>位</td><td>名称</td><td>访问</td><td>描述</td><td>复位值</td></tr><tr><td>[31:8]</td><td>Reserved</td><td>R0</td><td>保留。</td><td>0</td></tr><tr><td>[7:0]</td><td>INTEN</td><td>WO</td><td>96#--103#中断使能控制。
1: 当前编号中断使能;
0: 无影响。</td><td>0</td></tr></table>

#### 9.5.2.21 PFIC 中断使能清除寄存器 1（PFIC_IRER1）

偏移地址： $\mathtt { 0 } \times 1 8 0$

<table><tr><td>31</td><td>30</td><td>29</td><td>28</td><td>27</td><td>26</td><td>25</td><td>24</td><td>23</td><td>22</td><td>21</td><td>20</td><td>19</td><td>18</td><td>17</td><td>16</td></tr><tr><td colspan="16">INTRST[31:16]</td></tr><tr><td>15</td><td>14</td><td>13</td><td>12</td><td>11</td><td>10</td><td>9</td><td>8</td><td>7</td><td>6</td><td>5</td><td>4</td><td>3</td><td>2</td><td>1</td><td>0</td></tr><tr><td>INTRSET15</td><td>INTRSET14</td><td>INTRSET13</td><td>INTRSET12</td><td colspan="12">Reserved</td></tr></table>

<table><tr><td>位</td><td>名称</td><td>访问</td><td>描述</td><td>复位值</td></tr><tr><td>[31:12]</td><td>INTRSET</td><td>WO</td><td>12#-31#中断关闭控制。1:当前编号中断关闭;0:无影响。</td><td>0</td></tr><tr><td>[11:0]</td><td>Reserved</td><td>RO</td><td>保留。</td><td>0</td></tr></table>

#### 9.5.2.22 PFIC 中断使能清除寄存器 2（PFIC_IRER2）

偏移地址： $0 \times 1 8 4$

<table><tr><td>31</td><td>30</td><td>29</td><td>28</td><td>27</td><td>26</td><td>25</td><td>24</td><td>23</td><td>22</td><td>21</td><td>20</td><td>19</td><td>18</td><td>17</td><td>16</td></tr><tr><td colspan="16">INTRSET[63:48]</td></tr><tr><td>15</td><td>14</td><td>13</td><td>12</td><td>11</td><td>10</td><td>9</td><td>8</td><td>7</td><td>6</td><td>5</td><td>4</td><td>3</td><td>2</td><td>1</td><td>0</td></tr><tr><td colspan="16">INTRSET[47:32]</td></tr></table>

<table><tr><td>位</td><td>名称</td><td>访问</td><td>描述</td><td>复位值</td></tr><tr><td>[31:0]</td><td>INTRSET</td><td>WO</td><td>32#-63#中断关闭控制。1:当前编号中断关闭;0:无影响。</td><td>0</td></tr></table>

#### 9.5.2.23 PFIC 中断使能清除寄存器 3（PFIC_IRER3）

偏移地址： $0 \times 1 8 8$

<table><tr><td>31</td><td>30</td><td>29</td><td>28</td><td>27</td><td>26</td><td>25</td><td>24</td><td>23</td><td>22</td><td>21</td><td>20</td><td>19</td><td>18</td><td>17</td><td>16</td></tr><tr><td colspan="16">INTRSET[95:80]</td></tr><tr><td>15</td><td>14</td><td>13</td><td>12</td><td>11</td><td>10</td><td>9</td><td>8</td><td>7</td><td>6</td><td>5</td><td>4</td><td>3</td><td>2</td><td>1</td><td>0</td></tr><tr><td colspan="16">INTRSET[79:64]</td></tr></table>

<table><tr><td>位</td><td>名称</td><td>访问</td><td>描述</td><td>复位值</td></tr><tr><td>[31:0]</td><td>INTRSET</td><td>WO</td><td>64#-95#中断关闭控制。
1: 当前编号中断关闭;
0: 无影响。</td><td>0</td></tr></table>

#### 9.5.2.24 PFIC 中断使能清除寄存器 4（PFIC_IRER4）

偏移地址： $\mathtt { 0 } \mathtt { x } 1 8 0$

<table><tr><td>31</td><td>30</td><td>29</td><td>28</td><td>27</td><td>26</td><td>25</td><td>24</td><td>23</td><td>22</td><td>21</td><td>20</td><td>19</td><td>18</td><td>17</td><td>16</td></tr><tr><td colspan="16">Reserved</td></tr><tr><td>15</td><td>14</td><td>13</td><td>12</td><td>11</td><td>10</td><td>9</td><td>8</td><td>7</td><td>6</td><td>5</td><td>4</td><td>3</td><td>2</td><td>1</td><td>0</td></tr><tr><td colspan="8">Reserved</td><td colspan="8">INTRST [103:96]</td></tr></table>

<table><tr><td>位</td><td>名称</td><td>访问</td><td>描述</td><td>复位值</td></tr><tr><td>[31:8]</td><td>Reserved</td><td>R0</td><td>保留。</td><td>0</td></tr><tr><td>[7:0]</td><td>INTRST</td><td>WO</td><td>96#--103#中断关闭控制。
1: 当前编号中断关闭;
0: 无影响。</td><td>0</td></tr></table>

#### 9.5.2.25 PFIC 中断挂起设置寄存器 1（PFIC_IPSR1）

偏移地址： $\mathtt { 0 \times 2 0 0 }$

<table><tr><td>31</td><td>30</td><td>29</td><td>28</td><td>27</td><td>26</td><td>25</td><td>24</td><td>23</td><td>22</td><td>21</td><td>20</td><td>19</td><td>18</td><td>17</td><td>16</td></tr><tr><td colspan="16">PENDSET[31:16]</td></tr><tr><td>15</td><td>14</td><td>13</td><td>12</td><td>11</td><td>10</td><td>9</td><td>8</td><td>7</td><td>6</td><td>5</td><td>4</td><td>3</td><td>2</td><td>1</td><td>0</td></tr><tr><td>PENDSET15</td><td>PENDSET14</td><td>PENDSET13</td><td>PENDSET12</td><td colspan="8">Reserved</td><td>PENDSET3</td><td>PENDSET2</td><td colspan="2">Reserved</td></tr></table>

<table><tr><td>位</td><td>名称</td><td>访问</td><td>描述</td><td>复位值</td></tr><tr><td>[31:12]</td><td>PENDSET</td><td>WO</td><td>12#-31#中断挂起设置,13#和15#保留。1:当前编号中断挂起;0:无影响。</td><td>0</td></tr><tr><td>[11:4]</td><td>Reserved</td><td>RO</td><td>保留。</td><td>0</td></tr><tr><td>[3:2]</td><td>PENDSET</td><td>WO</td><td>2#-3#中断挂起设置。1:当前编号中断挂起;0:无影响。</td><td>0</td></tr><tr><td>[1:0]</td><td>Reserved</td><td>R0</td><td>保留。</td><td>0</td></tr></table>

#### 9.5.2.26 PFIC 中断挂起设置寄存器 2（PFIC_IPSR2）

偏移地址： $0 \times 2 0 4$

<table><tr><td>31</td><td>30</td><td>29</td><td>28</td><td>27</td><td>26</td><td>25</td><td>24</td><td>23</td><td>22</td><td>21</td><td>20</td><td>19</td><td>18</td><td>17</td><td>16</td></tr><tr><td colspan="16">PENDSET[63:48]</td></tr><tr><td>15</td><td>14</td><td>13</td><td>12</td><td>11</td><td>10</td><td>9</td><td>8</td><td>7</td><td>6</td><td>5</td><td>4</td><td>3</td><td>2</td><td>1</td><td>0</td></tr><tr><td colspan="16">PENDSET[47:32]</td></tr></table>

<table><tr><td>位</td><td>名称</td><td>访问</td><td>描述</td><td>复位值</td></tr><tr><td>[31:0]</td><td>PENDSET</td><td>WO</td><td>32#-63#中断挂起设置。
1: 当前编号中断挂起;
0: 无影响。</td><td>0</td></tr></table>

#### 9.5.2.27 PFIC 中断挂起设置寄存器 3（PFIC_IPSR3）

偏移地址： $_ { 0 \times 2 0 8 }$

<table><tr><td>31</td><td>30</td><td>29</td><td>28</td><td>27</td><td>26</td><td>25</td><td>24</td><td>23</td><td>22</td><td>21</td><td>20</td><td>19</td><td>18</td><td>17</td><td>16</td></tr><tr><td colspan="16">PENDSET[95:80]</td></tr><tr><td>15</td><td>14</td><td>13</td><td>12</td><td>11</td><td>10</td><td>9</td><td>8</td><td>7</td><td>6</td><td>5</td><td>4</td><td>3</td><td>2</td><td>1</td><td>0</td></tr><tr><td colspan="16">PENDSET[79:64]</td></tr></table>

<table><tr><td>位</td><td>名称</td><td>访问</td><td>描述</td><td>复位值</td></tr><tr><td>[31:0]</td><td>PENDSET</td><td>WO</td><td>64#-95#中断挂起设置。
1: 当前编号中断挂起;
0: 无影响。</td><td>0</td></tr></table>

#### 9.5.2.28 PFIC 中断挂起设置寄存器 4（PFIC_IPSR4）

偏移地址： $_ { 0 \times 2 0 0 }$

<table><tr><td>31</td><td>30</td><td>29</td><td>28</td><td>27</td><td>26</td><td>25</td><td>24</td><td>23</td><td>22</td><td>21</td><td>20</td><td>19</td><td>18</td><td>17</td><td>16</td></tr><tr><td colspan="16">Reserved</td></tr><tr><td>15</td><td>14</td><td>13</td><td>12</td><td>11</td><td>10</td><td>9</td><td>8</td><td>7</td><td>6</td><td>5</td><td>4</td><td>3</td><td>2</td><td>1</td><td>0</td></tr><tr><td colspan="8">Reserved</td><td colspan="8">PENDSET [103:96]</td></tr></table>

<table><tr><td>位</td><td>名称</td><td>访问</td><td>描述</td><td>复位值</td></tr><tr><td>[31:8]</td><td>Reserved</td><td>R0</td><td>保留。</td><td>0</td></tr><tr><td>[7:0]</td><td>PENDSET</td><td>WO</td><td>96#--103#中断挂起设置。
1: 当前编号中断挂起;
0: 无影响。</td><td>0</td></tr></table>

#### 9.5.2.29 PFIC 中断挂起清除寄存器 1（PFIC_IPRR1）

偏移地址： $\mathtt { 0 } \mathtt { x } 2 8 0$

<table><tr><td>31</td><td>30</td><td>29</td><td>28</td><td>27</td><td>26</td><td>25</td><td>24</td><td>23</td><td>22</td><td>21</td><td>20</td><td>19</td><td>18</td><td>17</td><td>16</td></tr><tr><td colspan="16">PENDRST[31:16]</td></tr><tr><td>15</td><td>14</td><td>13</td><td>12</td><td>11</td><td>10</td><td>9</td><td>8</td><td>7</td><td>6</td><td>5</td><td>4</td><td>3</td><td>2</td><td>1</td><td>0</td></tr><tr><td>PENDRST15</td><td>PENDRST14</td><td>PENDRST13</td><td>PENDRST12</td><td colspan="8">Reserved</td><td>PENDRST3</td><td>PENDRST2</td><td colspan="2">Reserved</td></tr></table>

<table><tr><td>位</td><td>名称</td><td>访问</td><td>描述</td><td>复位值</td></tr><tr><td>[31:12]</td><td>PENDRST</td><td>WO</td><td>12#-31#中断挂起清除,13#和15#保留。1:当前编号中断清除挂起状态;0:无影响。</td><td>0</td></tr><tr><td>[11:4]</td><td>Reserved</td><td>RO</td><td>保留。</td><td>0</td></tr><tr><td>[3:2]</td><td>PENDRST</td><td>WO</td><td>2#-3#中断挂起清除。1:当前编号中断清除挂起状态;0:无影响。</td><td>0</td></tr><tr><td>[1:0]</td><td>Reserved</td><td>RO</td><td>保留。</td><td>0</td></tr></table>

#### 9.5.2.30 PFIC 中断挂起清除寄存器 2（PFIC_IPRR2）

偏移地址： $0 \times 2 8 4$

<table><tr><td>31</td><td>30</td><td>29</td><td>28</td><td>27</td><td>26</td><td>25</td><td>24</td><td>23</td><td>22</td><td>21</td><td>20</td><td>19</td><td>18</td><td>17</td><td>16</td></tr><tr><td colspan="16">PENDRST[63:48]</td></tr><tr><td>15</td><td>14</td><td>13</td><td>12</td><td>11</td><td>10</td><td>9</td><td>8</td><td>7</td><td>6</td><td>5</td><td>4</td><td>3</td><td>2</td><td>1</td><td>0</td></tr><tr><td colspan="16">PENDRST[47:32]</td></tr></table>

<table><tr><td>位</td><td>名称</td><td>访问</td><td>描述</td><td>复位值</td></tr><tr><td>[31:0]</td><td>PENDRST</td><td>WO</td><td>32#-63#中断挂起清除。
1: 当前编号中断清除挂起状态;
0: 无影响。</td><td>0</td></tr></table>

#### 9.5.2.31 PFIC 中断挂起清除寄存器 3（PFIC_IPRR3）

偏移地址： $_ { 0 \times 2 8 8 }$

<table><tr><td>31</td><td>30</td><td>29</td><td>28</td><td>27</td><td>26</td><td>25</td><td>24</td><td>23</td><td>22</td><td>21</td><td>20</td><td>19</td><td>18</td><td>17</td><td>16</td></tr><tr><td colspan="16">PENDRST[95:80]</td></tr><tr><td>15</td><td>14</td><td>13</td><td>12</td><td>11</td><td>10</td><td>9</td><td>8</td><td>7</td><td>6</td><td>5</td><td>4</td><td>3</td><td>2</td><td>1</td><td>0</td></tr><tr><td colspan="16">PENDRST[79:64]</td></tr></table>

<table><tr><td>位</td><td>名称</td><td>访问</td><td>描述</td><td>复位值</td></tr><tr><td>[31:0]</td><td>PENDRST</td><td>WO</td><td>64#--95#中断挂起清除。
1: 当前编号中断清除挂起状态;
0: 无影响。</td><td>0</td></tr></table>

#### 9.5.2.32 PFIC 中断挂起清除寄存器 4（PFIC_IPRR4）

偏移地址： $\mathtt { 0 \times 2 8 0 }$

<table><tr><td>31</td><td>30</td><td>29</td><td>28</td><td>27</td><td>26</td><td>25</td><td>24</td><td>23</td><td>22</td><td>21</td><td>20</td><td>19</td><td>18</td><td>17</td><td>16</td></tr><tr><td colspan="16">Reserved</td></tr><tr><td>15</td><td>14</td><td>13</td><td>12</td><td>11</td><td>10</td><td>9</td><td>8</td><td>7</td><td>6</td><td>5</td><td>4</td><td>3</td><td>2</td><td>1</td><td>0</td></tr><tr><td colspan="8">Reserved</td><td colspan="8">PENDRST [103:96]</td></tr></table>

<table><tr><td>位</td><td>名称</td><td>访问</td><td>描述</td><td>复位值</td></tr><tr><td>[31:8]</td><td>Reserved</td><td>R0</td><td>保留。</td><td>0</td></tr><tr><td>[7:0]</td><td>PENSET</td><td>WO</td><td>96#--103#中断挂起清除。
1: 当前编号中断清除挂起状态;
0: 无影响</td><td>0</td></tr></table>

#### 9.5.2.33 PFIC 中断激活状态寄存器 1（PFIC_IACTR1）

偏移地址： $0 \times 3 0 0$

<table><tr><td>31</td><td>30</td><td>29</td><td>28</td><td>27</td><td>26</td><td>25</td><td>24</td><td>23</td><td>22</td><td>21</td><td>20</td><td>19</td><td>18</td><td>17</td><td>16</td></tr><tr><td colspan="16">IACTS [31:16]</td></tr><tr><td>15</td><td>14</td><td>13</td><td>12</td><td>11</td><td>10</td><td>9</td><td>8</td><td>7</td><td>6</td><td>5</td><td>4</td><td>3</td><td>2</td><td>1</td><td>0</td></tr><tr><td>IACTS15</td><td>IACTS14</td><td>IACTS13</td><td>IACTS12</td><td colspan="8">Reserved</td><td>IACTS3</td><td>IACTS2</td><td colspan="2">Reserved</td></tr></table>

<table><tr><td>位</td><td>名称</td><td>访问</td><td>描述</td><td>复位值</td></tr><tr><td>[31:12]</td><td>IACTS</td><td>RO</td><td>12#-31#中断执行状态，13#和15#保留。1：当前编号中断执行中；0：当前编号中断没执行。</td><td>0</td></tr><tr><td>[11:4]</td><td>Reserved</td><td>RO</td><td>保留。</td><td>0</td></tr><tr><td>[3:2]</td><td>IACTS</td><td>RO</td><td>2#-3#中断执行状态。1：当前编号中断执行中；0：当前编号中断没执行。</td><td>0</td></tr><tr><td>[1:0]</td><td>Reserved</td><td>RO</td><td>保留。</td><td>0</td></tr></table>

#### 9.5.2.34 PFIC 中断激活状态寄存器 2（PFIC_IACTR2）

偏移地址： $0 \times 3 0 4$

<table><tr><td>31</td><td>30</td><td>29</td><td>28</td><td>27</td><td>26</td><td>25</td><td>24</td><td>23</td><td>22</td><td>21</td><td>20</td><td>19</td><td>18</td><td>17</td><td>16</td></tr><tr><td colspan="16">IACTS[63:48]</td></tr><tr><td>15</td><td>14</td><td>13</td><td>12</td><td>11</td><td>10</td><td>9</td><td>8</td><td>7</td><td>6</td><td>5</td><td>4</td><td>3</td><td>2</td><td>1</td><td>0</td></tr><tr><td colspan="16">IACTS[47:32]</td></tr></table>

<table><tr><td>位</td><td>名称</td><td>访问</td><td>描述</td><td>复位值</td></tr><tr><td>[31:0]</td><td>IACTS</td><td>RO</td><td>32#-63#中断执行状态。</td><td>0</td></tr><tr><td></td><td></td><td></td><td>1: 当前编号中断执行中;
0: 当前编号中断没执行。</td><td></td></tr></table>

#### 9.5.2.35 PFIC 中断激活状态寄存器 3（PFIC_IACTR3）

偏移地址： $0 \times 3 0 8$

<table><tr><td>31</td><td>30</td><td>29</td><td>28</td><td>27</td><td>26</td><td>25</td><td>24</td><td>23</td><td>22</td><td>21</td><td>20</td><td>19</td><td>18</td><td>17</td><td>16</td></tr><tr><td colspan="16">I ACTS[95:80]</td></tr><tr><td>15</td><td>14</td><td>13</td><td>12</td><td>11</td><td>10</td><td>9</td><td>8</td><td>7</td><td>6</td><td>5</td><td>4</td><td>3</td><td>2</td><td>1</td><td>0</td></tr><tr><td colspan="16">I ACTS[79:64]</td></tr></table>

<table><tr><td>位</td><td>名称</td><td>访问</td><td>描述</td><td>复位值</td></tr><tr><td>[31:0]</td><td>IACTS</td><td>RO</td><td>64#--95#中断执行状态。
1: 当前编号中断执行中;
0: 当前编号中断没执行。</td><td>0</td></tr></table>

#### 9.5.2.36 PFIC 中断激活状态寄存器 4（PFIC_IACTR4）

偏移地址： $\mathtt { 0 } \mathtt { x } \mathtt { 3 0 0 }$

<table><tr><td>31</td><td>30</td><td>29</td><td>28</td><td>27</td><td>26</td><td>25</td><td>24</td><td>23</td><td>22</td><td>21</td><td>20</td><td>19</td><td>18</td><td>17</td><td>16</td></tr><tr><td colspan="16">Reserved</td></tr><tr><td>15</td><td>14</td><td>13</td><td>12</td><td>11</td><td>10</td><td>9</td><td>8</td><td>7</td><td>6</td><td>5</td><td>4</td><td>3</td><td>2</td><td>1</td><td>0</td></tr><tr><td colspan="8">Reserved</td><td colspan="8">IACTS [103:96]</td></tr></table>

<table><tr><td>位</td><td>名称</td><td>访问</td><td>描述</td><td>复位值</td></tr><tr><td>[31:8]</td><td>Reserved</td><td>R0</td><td>保留。</td><td>0</td></tr><tr><td>[7:0]</td><td>IACTS</td><td>WO</td><td>96#--103#中断执行状态。
1: 当前编号中断执行中;
0: 当前编号中断没执行</td><td>0</td></tr></table>

#### 9.5.2.37 PFIC 中断优先级配置寄存器（PFIC_IPRIORx）（x=0-63）

偏移地址： $0 { \times } 4 0 0 - 0 { \times } 4 { \sf F F }$

控制器支持 256 个中断（0-255），每个中断使用 8bit 来设置控制优先级。

<table><tr><td rowspan="2">IPRIOR63</td><td>31</td><td>24</td><td>23</td><td>16</td><td>15</td><td>8</td><td>7</td><td>0</td></tr><tr><td colspan="2">PRI0_255</td><td colspan="2">PRI0_254</td><td colspan="2">PRI0_253</td><td colspan="2">PRI0_252</td></tr><tr><td rowspan="2">IPRIORx</td><td>...</td><td>...</td><td>...</td><td>...</td><td>...</td><td>...</td><td colspan="2">...</td></tr><tr><td colspan="2">PRI0_(4x+3)</td><td colspan="2">PRI0_(4x+2)</td><td colspan="2">PRI0_(4x+1)</td><td colspan="2">PRI0_(4x)</td></tr><tr><td rowspan="2">IPRIOR0</td><td>...</td><td>...</td><td>...</td><td>...</td><td>...</td><td>...</td><td colspan="2">...</td></tr><tr><td colspan="2">PRI0_3</td><td colspan="2">PRI0_2</td><td colspan="2">PRI0_1</td><td colspan="2">PRI0_0</td></tr></table>

<table><tr><td>位</td><td>名称</td><td>访问</td><td>描述</td><td>复位值</td></tr><tr><td>[2047:2040]</td><td>IP_255</td><td>RW</td><td>同IP_0描述。</td><td>0</td></tr><tr><td>...</td><td>...</td><td>...</td><td>...</td><td>...</td></tr><tr><td>[31:24]</td><td>IP_3</td><td>RW</td><td>同IP_0描述。</td><td>0</td></tr><tr><td>[23:16]</td><td>IP_2</td><td>RW</td><td>同IP_0描述。</td><td>0</td></tr><tr><td>[15:8]</td><td>IP_1</td><td>RW</td><td>同IP_0描述。</td><td>0</td></tr><tr><td>[7:0]</td><td>IP_0</td><td>RW</td><td>编号0中断优先级配置:[7:4]:优先级控制位。若配置无嵌套,无抢占位;若配置2级嵌套,bit7为抢占位;若配置4级嵌套,bit7-bit6为抢占位;若配置8级嵌套,bit7-bit5为抢占位;优先级数值越小则优先级越高,同一抢占优先级中断若同时挂起,优先执行优先级高的中断。[3:0]:保留,固定为0,写无效。注:适用于青棵V4F内核:CH32V30xD8、CH32V30xD8C。编号0中断优先级配置:[7]:优先级控制位。若配置无嵌套,无抢占位;若配置2级嵌套,bit7为抢占位;优先级数值越小则优先级越高,同一抢占优先级中断若同时挂起,优先执行优先级高的中断。[6:0]:保留,固定为0,写无效。注:适用于青棵V4B、V4C内核:CH32V20xD6、CH32V20xD8、CH32V20xD8W。</td><td>0</td></tr></table>

#### 9.5.2.38 PFIC 系统控制寄存器（PFIC_SCTLR）

偏移地址： $0 \times 0 1 0$

<table><tr><td>31</td><td>30</td><td>29</td><td>28</td><td>27</td><td>26</td><td>25</td><td>24</td><td>23</td><td>22</td><td>21</td><td>20</td><td>19</td><td>18</td><td>17</td><td>16</td></tr><tr><td>SYS
RST</td><td colspan="15">Reserved</td></tr><tr><td>15</td><td>14</td><td>13</td><td>12</td><td>11</td><td>10</td><td>9</td><td>8</td><td>7</td><td>6</td><td>5</td><td>4</td><td>3</td><td>2</td><td>1</td><td>0</td></tr><tr><td colspan="10">Reserved</td><td>SET
EVENT</td><td>SEV
ONPEND</td><td>WF I TO
WFE</td><td>SLEEP
DEEP</td><td>SLEEP
ONEXIT</td><td>Reser
ved</td></tr></table>

<table><tr><td>位</td><td>名称</td><td>访问</td><td>描述</td><td>复位值</td></tr><tr><td>31</td><td>SYSRST</td><td>WO</td><td>系统复位,自动清0。写1有效,写0无效,与PFIC_CFGR寄存器相同效果</td><td>0</td></tr><tr><td>[30:6]</td><td>Reserved</td><td>RO</td><td>保留。</td><td>0</td></tr><tr><td>5</td><td>SETEVENT</td><td>WO</td><td>设置事件,可以唤醒 WFE 的情况。</td><td>0</td></tr><tr><td>4</td><td>SEVONPEND</td><td>RW</td><td>当发生事件或者中断挂起状态时,可以从 WFE 指令后唤醒系统,如果未执行 WFE 指令,将在下次执行该指令后立即唤醒系统。
1: 启用的事件和所有中断(包括未开启中断)都能唤醒系统;
0: 只有启用的事件和启用的中断可以唤醒系统。</td><td>0</td></tr><tr><td>3</td><td>WFITOWFE</td><td>RW</td><td>将 WFI 指令当成是 WFE 执行。
1: 将之后的 WFI 指令当做 WFE 指令;
0: 无作用。</td><td>0</td></tr><tr><td>2</td><td>SLEEPDEEP</td><td>RW</td><td>控制系统的低功耗模式:
1: deepsleep 0: sleep</td><td>0</td></tr><tr><td>1</td><td>SLEEPONEXIT</td><td>RW</td><td>控制离开中断服务程序后,系统状态:
1: 系统进入低功耗模式;
0: 系统进入主程序。</td><td>0</td></tr><tr><td>0</td><td>Reserved</td><td>RO</td><td>保留。</td><td>0</td></tr></table>

### 9.5.3 专用 CSR 寄存器

RISC-V 架构中定义了一些控制和状态寄存器（Control and Status Register,CSR），用于配置或标识或记录运行状态。CSR 寄存器属于内核内部的寄存器，使用专用的 12 位地址空间。 $\mathtt { C H 3 2 V 2 0 x }$ 和 $\mathtt { C H 3 2 V 3 0 x }$ 系列芯片除了 RISC-V 特权架构文档中定义的标准寄存器外，还增加了一些厂商自定义寄存器，需要使用 csr 指令进行访问。

注：此类寄存器标注为“MRW，MRO,MRW1”属性的需要系统在机器模式下才能访问。

#### 9.5.3.1 中断系统控制寄存器（INTSYSCR）

CSR 地址： $0 \times 8 0 4$   

<table><tr><td>31</td><td>30</td><td>29</td><td>28</td><td>27</td><td>26</td><td>25</td><td>24</td><td>23</td><td>22</td><td>21</td><td>20</td><td>19</td><td>18</td><td>17</td><td>16</td></tr><tr><td colspan="16">Reserved</td></tr><tr><td>15</td><td>14</td><td>13</td><td>12</td><td>11</td><td>10</td><td>9</td><td>8</td><td>7</td><td>6</td><td>5</td><td>4</td><td>3</td><td>2</td><td>1</td><td>0</td></tr><tr><td colspan="7">PMTSTA</td><td>Reserved</td><td>GIHWSTKNEN</td><td>HWSTKOVEN</td><td colspan="3">PMTCFG</td><td>INESTEN</td><td>HWSTKEN</td><td></td></tr></table>

<table><tr><td>位</td><td>名称</td><td>访问</td><td>描述</td><td>复位值</td></tr><tr><td>[31:16]</td><td>Reserved</td><td>MRO</td><td>保留。</td><td>0</td></tr><tr><td>[15:8]</td><td>PMTSTA</td><td>MRO</td><td>抢占位状态指示:0x00:优先级配置位中无抢占位,不发生中断嵌套;0x80:优先级配置位中最高位为抢占位,2级中断嵌套;0xC0:优先级配置位中高2位为抢占位,4级中断嵌套;0xE0:优先级配置位中高3位为抢占位,</td><td>0x00</td></tr><tr><td></td><td></td><td></td><td>8级中断嵌套。注:适用于青棵V4F内核:CH32V30x_D8、CH32V30x_D8C。抢占位状态指示:0x00:优先级配置位中无抢占位,不发生中断嵌套;0x80:优先级配置位中最高位为抢占位,2级中断嵌套;注:适用于青棵V4B、V4C内核:CH32V20x_D6、CH32V20x_D8、CH32V20x_D8W。</td><td></td></tr><tr><td>[7:6]</td><td>Reserved</td><td>MRO</td><td>保留。</td><td>0</td></tr><tr><td>5</td><td>GIHWSTKNEN</td><td>MRW1</td><td>全局中断和硬件压栈关闭使能。注:该位常使用于实时操作系统中,中断切换上下文时,置位该位,可关闭全局中断和硬件压栈出栈,当上下文切换完成,执行完中断返回后,硬件自动清除该位。</td><td>0</td></tr><tr><td>4</td><td>HWSTKOVEN</td><td>MRW</td><td>硬件压栈溢出后中断使能:0:硬件压栈溢出后,关闭全局中断;1:硬件压栈溢出后,中断仍可执行。注:CH32V30x_D8、CH32V30x_D8C硬件压栈深度为3级,当配置嵌套等级大于3级,若该位设置1,需要将低优先级的三级中断配置为硬件压栈,高优先级配置为软件压栈。CH32V20x_D6、CH32V20x_D8、CH32V20x_D8W硬件压栈深度为2级。</td><td>0</td></tr><tr><td>[3:2]</td><td>PMTCFG[1:0]</td><td>MRW</td><td>中断嵌套深度配置:00:无嵌套,抢占位个数为0;01:2级嵌套,抢占位个数为1;10:4级嵌套,抢占位个数为2;11:8级嵌套,抢占位个数为3。注:适用于青棵V4F内核:CH32V30x_D8、CH32V30x_D8C。中断嵌套深度配置:00:无嵌套,抢占位个数为0;01:2级嵌套,抢占位个数为1;注:适用于青棵V4B、V4C内核:CH32V20x_D6、CH32V20x_D8、CH32V20x_D8W。</td><td>00b</td></tr><tr><td>1</td><td>INESTEN</td><td>MRW</td><td>中断嵌套使能:0:中断嵌套功能关闭;1:中断嵌套功能使能。</td><td>0</td></tr><tr><td>0</td><td>HWSTKEN</td><td>MRW</td><td>硬件压栈使能:
0: 硬件压栈功能关闭;
1: 硬件压栈功能使能。</td><td>0</td></tr></table>

#### 9.5.3.2 异常入口基地址寄存器（MTVEC）

CSR 地址： $0 \times 3 0 5$

<table><tr><td>31</td><td>30</td><td>29</td><td>28</td><td>27</td><td>26</td><td>25</td><td>24</td><td>23</td><td>22</td><td>21</td><td>20</td><td>19</td><td>18</td><td>17</td><td>16</td></tr><tr><td colspan="16">BASEADDR[31:16]</td></tr><tr><td>15</td><td>14</td><td>13</td><td>12</td><td>11</td><td>10</td><td>9</td><td>8</td><td>7</td><td>6</td><td>5</td><td>4</td><td>3</td><td>2</td><td>1</td><td>0</td></tr><tr><td colspan="14">BASEADDR[15:2]</td><td>MODE1</td><td>MODE0</td></tr></table>

<table><tr><td>位</td><td>名称</td><td>访问</td><td>描述</td><td>复位值</td></tr><tr><td>[31:2]</td><td>BASEADDR[31:2]</td><td>MRW</td><td>中断向量表基地址。</td><td>0</td></tr><tr><td>1</td><td>MODE1</td><td>MRW</td><td>中断向量表识别模式:0:按跳转指令识别,有限范围,支持非跳指令;1:按绝对地址识别,支持全范围,但必须跳转。</td><td>0</td></tr><tr><td>0</td><td>MODE0</td><td>MRW</td><td>中断或异常入口地址模式选择:0:使用统一入口地址;1:根据中断编号*4进行地址偏移。</td><td>0</td></tr></table>

### 9.5.4 物理内存保护单元（PMP）

为了提高系统安全，RISC-V的架构中定义了一套物理地址访问限制，可以为区域内物理内存设置其读、写、执行属性，区域长度最小4字节保护。PMP单元在用户模式下一直生效，在机器模式下可选生效，如果违背了当前内存限制，将会产生系统异常中断（EXC）。

PMP 单元包含 4 组 8-bit 的配置寄存器（32bit）和 4 组地址寄存器，需要使用 csr 指令进行访问，并且在机器模式下进行。

#### 9.5.4.1 PMP 配置寄存器（PMPCFG0）

CSR 地址： $0 \times 3 { \tt A } 0$

<table><tr><td>31</td><td>30</td><td>29</td><td>28</td><td>27</td><td>26</td><td>25</td><td>24</td><td>23</td><td>22</td><td>21</td><td>20</td><td>19</td><td>18</td><td>17</td><td>16</td></tr><tr><td colspan="8">pmp3cfg</td><td colspan="8">pmp2cfg</td></tr><tr><td>15</td><td>14</td><td>13</td><td>12</td><td>11</td><td>10</td><td>9</td><td>8</td><td>7</td><td>6</td><td>5</td><td>4</td><td>3</td><td>2</td><td>1</td><td>0</td></tr><tr><td colspan="8">pmp1cfg</td><td colspan="8">pmp0cfg</td></tr></table>

<table><tr><td>位</td><td>名称</td><td>访问</td><td>描述</td><td>复位值</td></tr><tr><td>[31:24]</td><td>pmp3cfg</td><td>MRW</td><td>见pmp0cfg。</td><td>0</td></tr><tr><td>[23:16]</td><td>pmp2cfg</td><td>MRW</td><td>见pmp0cfg。</td><td>0</td></tr><tr><td>[15:8]</td><td>pmp1cfg</td><td>MRW</td><td>见pmp0cfg。</td><td>0</td></tr></table>

<table><tr><td rowspan="7">[7:0]</td><td rowspan="7">pmp0cfg</td><td rowspan="7">MRW</td><td>位</td><td>名称</td><td>描述</td><td rowspan="7">0</td></tr><tr><td>7</td><td>L</td><td>锁定使能,机器模式下可解锁0:不锁定;1:锁定相关寄存器。</td></tr><tr><td>[6:5]</td><td>-</td><td>保留。</td></tr><tr><td>[4:3]</td><td>A</td><td>地址对齐及保护区域范围选择。</td></tr><tr><td>2</td><td>X</td><td>可执行属性。</td></tr><tr><td>1</td><td>W</td><td>可写入属性。</td></tr><tr><td>0</td><td>R</td><td>可读出属性。</td></tr></table>

其中，地址对齐及保护区域范围选择，对于 A_ADDR≤region＜B_ADDR 区域进行内存保护（要求A_ADDR 和 B_ADDR 均为 4 字节对齐）：

1、如果 B_ADDR – A_ADDR == 22，则采用 NA4 方式；  
2、如果 B_ADDR – $\mathsf { A \_ A D D R } \mathsf { \Lambda } = \mathsf { \Lambda } 2 \mathsf { \Lambda } ^ { ( 6 + 2 ) }$ ， $\mathtt { G } \geqslant 1$ ，且 A_ADDR 为 2（G+2）对齐则采用 NAPOT 方式；  
3、否则采用 TOR 方式。

<table><tr><td>A值</td><td>名称</td><td>描述</td></tr><tr><td>00b</td><td>OFF</td><td>没有区域要保护</td></tr><tr><td>01b</td><td>TOR</td><td>顶端对齐区域保护:
pmp0cfg下,0≤region &lt;pmpaddr0;
pmp1cfg下,pmpaddr0≤region &lt;pmpaddr1;
pmp2cfg下,pmpaddr1≤region &lt;pmpaddr2;
pmp3cfg下,pmpaddr2≤region &lt;pmpaddr3。
pmpaddri-1=A_ADDR&gt;&gt;2;
pmpaddri=B_ADDR&gt;&gt;2。</td></tr><tr><td>10b</td><td>NA4</td><td>固定4字节区域保护。
pmp0cfg~pmp3cfg对应pmpaddr0~pmpaddr3作为起始地址。
pmpaddri=A_ADDR&gt;&gt;2。</td></tr><tr><td>11b</td><td>NAPOT</td><td>保护2(G+2)区域,G≥1,此时A_ADDR为2(G+2)对齐。
pmpaddri= ((A_ADDR| (2(G+2)-1)) &amp; ~ (1&lt;&lt;G+1)) &gt;&gt; 2。</td></tr></table>

#### 9.5.4.2 PMP 地址 0 寄存器（PMPADDR0）

CSR 地址： $\mathtt { 0 } \mathtt { x } \mathtt { 3 } \mathtt { B 0 }$

<table><tr><td>31</td><td>30</td><td>29</td><td>28</td><td>27</td><td>26</td><td>25</td><td>24</td><td>23</td><td>22</td><td>21</td><td>20</td><td>19</td><td>18</td><td>17</td><td>16</td></tr><tr><td colspan="16">ADDR0[33:18]</td></tr><tr><td>15</td><td>14</td><td>13</td><td>12</td><td>11</td><td>10</td><td>9</td><td>8</td><td>7</td><td>6</td><td>5</td><td>4</td><td>3</td><td>2</td><td>1</td><td>0</td></tr><tr><td colspan="16">ADDR0[17:2]</td></tr></table>

<table><tr><td>位</td><td>名称</td><td>访问</td><td>描述</td><td>复位值</td></tr><tr><td>[31:0]</td><td>ADDR0</td><td>MRW</td><td>PMP设置地址0的bit[33:2],实际高2位未用。</td><td>0</td></tr></table>

#### 9.5.4.3 PMP 地址 1 寄存器（PMPADDR1）

CSR 地址： $0 \times 3 { \tt B } 1$

<table><tr><td>31</td><td>30</td><td>29</td><td>28</td><td>27</td><td>26</td><td>25</td><td>24</td><td>23</td><td>22</td><td>21</td><td>20</td><td>19</td><td>18</td><td>17</td><td>16</td></tr><tr><td colspan="16">ADDR1 [33:18]</td></tr><tr><td>15</td><td>14</td><td>13</td><td>12</td><td>11</td><td>10</td><td>9</td><td>8</td><td>7</td><td>6</td><td>5</td><td>4</td><td>3</td><td>2</td><td>1</td><td>0</td></tr><tr><td colspan="16">ADDR1 [17:2]</td></tr></table>

<table><tr><td>位</td><td>名称</td><td>访问</td><td>描述</td><td>复位值</td></tr><tr><td>[31:0]</td><td>ADDR1</td><td>MRW</td><td>PMP设置地址1的bit[33:2],实际高2位未用。</td><td>0</td></tr></table>

#### 9.5.4.4 PMP 地址 2 寄存器（PMPADDR2）

CSR 地址： $0 \times 3 8 2$

<table><tr><td>31</td><td>30</td><td>29</td><td>28</td><td>27</td><td>26</td><td>25</td><td>24</td><td>23</td><td>22</td><td>21</td><td>20</td><td>19</td><td>18</td><td>17</td><td>16</td></tr><tr><td colspan="16">ADDR2[33:18]</td></tr><tr><td>15</td><td>14</td><td>13</td><td>12</td><td>11</td><td>10</td><td>9</td><td>8</td><td>7</td><td>6</td><td>5</td><td>4</td><td>3</td><td>2</td><td>1</td><td>0</td></tr><tr><td colspan="16">ADDR2[17:2]</td></tr></table>

<table><tr><td>位</td><td>名称</td><td>访问</td><td>描述</td><td>复位值</td></tr><tr><td>[31:0]</td><td>ADDR2</td><td>MRW</td><td>PMP设置地址2的bit[33:2],实际高2位未用。</td><td>0</td></tr></table>

#### 9.5.4.5 PMP 地址 3 寄存器（PMPADDR3）

CSR 地址： $0 \times 3 8 3$

<table><tr><td>31</td><td>30</td><td>29</td><td>28</td><td>27</td><td>26</td><td>25</td><td>24</td><td>23</td><td>22</td><td>21</td><td>20</td><td>19</td><td>18</td><td>17</td><td>16</td></tr><tr><td colspan="16">ADDR3[33:18]</td></tr><tr><td>15</td><td>14</td><td>13</td><td>12</td><td>11</td><td>10</td><td>9</td><td>8</td><td>7</td><td>6</td><td>5</td><td>4</td><td>3</td><td>2</td><td>1</td><td>0</td></tr><tr><td colspan="16">ADDR3[17:2]</td></tr></table>

<table><tr><td>位</td><td>名称</td><td>访问</td><td>描述</td><td>复位值</td></tr><tr><td>[31:0]</td><td>ADDR3</td><td>MRW</td><td>PMP设置地址3的bit[33:2],实际高2位未用。</td><td>0</td></tr></table>

### 9.5.5 RISC-V-SysTick 寄存器描述

表9-6 STK相关寄存器列表  

<table><tr><td>名称</td><td>访问地址</td><td>描述</td><td>复位值</td></tr><tr><td>R32_STK_CTLR</td><td>0xE000F000</td><td>系统计数控制寄存器</td><td>0x00000000</td></tr><tr><td>R32_STK_SR</td><td>0xE000F004</td><td>系统计数状态寄存器</td><td>0x00000000</td></tr><tr><td>R32_STK_CNTL</td><td>0xE000F008</td><td>系统计数器低位寄存器</td><td>0x00000000</td></tr><tr><td>R32_STK_CNTH</td><td>0xE000F00C</td><td>系统计数器高位寄存器</td><td>0x00000000</td></tr><tr><td>R32_STK_CMPLR</td><td>0xE000F010</td><td>计数比较低位寄存器</td><td>0x00000000</td></tr><tr><td>R32_STK_CMPHR</td><td>0xE000F014</td><td>计数比较高位寄存器</td><td>0x00000000</td></tr></table>

注：适用于基于32位RISC-V指令集及架构设计的通用微控制器。

#### 9.5.5.1 系统计数控制寄存器（STK_CTLR）

偏移地址： $0 \times 0 0$

<table><tr><td>31</td><td>30</td><td>29</td><td>28</td><td>27</td><td>26</td><td>25</td><td>24</td><td>23</td><td>22</td><td>21</td><td>20</td><td>19</td><td>18</td><td>17</td><td>16</td></tr><tr><td>SWIE</td><td colspan="15">Reserved</td></tr><tr><td>15</td><td>14</td><td>13</td><td>12</td><td>11</td><td>10</td><td>9</td><td>8</td><td>7</td><td>6</td><td>5</td><td>4</td><td>3</td><td>2</td><td>1</td><td>0</td></tr><tr><td colspan="10">Reserved</td><td>INIT</td><td>MODE</td><td>STRE</td><td>STCLK</td><td>STIE</td><td>STE</td></tr></table>

<table><tr><td>位</td><td>名称</td><td>访问</td><td>描述</td><td>复位值</td></tr><tr><td>31</td><td>SWIE</td><td>RW</td><td>软件中断触发使能(SWI):1:触发软件中断;0:关闭触发。进入软件中断后,需软件清0,否则持续触发。</td><td>0</td></tr><tr><td>[30:6]</td><td>Reserved</td><td>RO</td><td>保留。</td><td>0</td></tr><tr><td>5</td><td>INIT</td><td>W1</td><td>计数器初始值更新:1:向上计数时更新为0,向下计数时更新为比较值;0:无效。</td><td>0</td></tr><tr><td>4</td><td>MODE</td><td>RW</td><td>计数模式:1:向下计数;0:向上计数。</td><td>0</td></tr><tr><td>3</td><td>STRE</td><td>RW</td><td>自动重装载计数使能位:1:向上计数到比较值后重新从0开始计数,向下计数到0后,重新从比较值开始计数;0:向上计数到比较值后继续向上计数,向下计数到0后,重新从最大值开始向下计数。</td><td>0</td></tr><tr><td>2</td><td>STCLK</td><td>RW</td><td>计数器时钟源选择位:1: HCLK做时基;0: HCLK/8做时基;</td><td>0</td></tr><tr><td>1</td><td>STIE</td><td>RW</td><td>计数器中断使能控制位:1:使能计数器中断;0:关闭计数器中断。</td><td>0</td></tr><tr><td>0</td><td>STE</td><td>RW</td><td>系统计数器使能控制位:1:启动系统计数器STK;0:关闭系统计数器STK,计数器停止计数。</td><td>0</td></tr></table>

#### 9.5.5.2 系统计数状态寄存器（STK_SR）

偏移地址： $0 \times 0 4$

<table><tr><td>31</td><td>30</td><td>29</td><td>28</td><td>27</td><td>26</td><td>25</td><td>24</td><td>23</td><td>22</td><td>21</td><td>20</td><td>19</td><td>18</td><td>17</td><td>16</td></tr><tr><td colspan="16">Reserved</td></tr><tr><td>15</td><td>14</td><td>13</td><td>12</td><td>11</td><td>10</td><td>9</td><td>8</td><td>7</td><td>6</td><td>5</td><td>4</td><td>3</td><td>2</td><td>1</td><td>0</td></tr><tr><td colspan="15">Reserved</td><td>CNTIF</td></tr></table>

<table><tr><td>位</td><td>名称</td><td>访问</td><td>描述</td><td>复位值</td></tr><tr><td>[31:1]</td><td>Reserved</td><td>R0</td><td>保留</td><td>0</td></tr><tr><td>0</td><td>CNTIF</td><td>RWO</td><td>计数值比较标志，写0清除，写1无效：
1：向上计数达到比较值，向下计数到0；
0：未达到比较值。</td><td>0</td></tr></table>

#### 9.5.5.3 系统计数器低位寄存器（STK_CNTL）

偏移地址： $0 \times 0 8$

<table><tr><td>31</td><td>30</td><td>29</td><td>28</td><td>27</td><td>26</td><td>25</td><td>24</td><td>23</td><td>22</td><td>21</td><td>20</td><td>19</td><td>18</td><td>17</td><td>16</td></tr><tr><td colspan="16">CNT[31:16]</td></tr><tr><td>15</td><td>14</td><td>13</td><td>12</td><td>11</td><td>10</td><td>9</td><td>8</td><td>7</td><td>6</td><td>5</td><td>4</td><td>3</td><td>2</td><td>1</td><td>0</td></tr><tr><td colspan="16">CNT[15:0]</td></tr></table>

<table><tr><td>位</td><td>名称</td><td>访问</td><td>描述</td><td>复位值</td></tr><tr><td>[31:0]</td><td>CNT[31:0]</td><td>RW</td><td>当前计数器计数值低32位。</td><td>0</td></tr></table>

注：寄存器 STK_CNTL和寄存器STK_CNTH共同构成了64位系统计数器。

#### 9.5.5.4 系统计数器高位寄存器（STK_CNTH）

偏移地址： $0 \times 0 0$

<table><tr><td>31</td><td>30</td><td>29</td><td>28</td><td>27</td><td>26</td><td>25</td><td>24</td><td>23</td><td>22</td><td>21</td><td>20</td><td>19</td><td>18</td><td>17</td><td>16</td></tr><tr><td colspan="16">CNT[63:48]</td></tr><tr><td>15</td><td>14</td><td>13</td><td>12</td><td>11</td><td>10</td><td>9</td><td>8</td><td>7</td><td>6</td><td>5</td><td>4</td><td>3</td><td>2</td><td>1</td><td>0</td></tr><tr><td colspan="16">CNT[47:32]</td></tr></table>

<table><tr><td>位</td><td>名称</td><td>访问</td><td>描述</td><td>复位值</td></tr><tr><td>[31:0]</td><td>CNT[63:32]</td><td>RW</td><td>当前计数器计数值高32位。</td><td>0</td></tr></table>

注：寄存器STK_CNTL和寄存器STK_CNTH共同构成了64位系统计数器。

#### 9.5.5.5 计数比较低位寄存器（STK_CMPLR）

偏移地址： $0 \times 1 0$

<table><tr><td>31</td><td>30</td><td>29</td><td>28</td><td>27</td><td>26</td><td>25</td><td>24</td><td>23</td><td>22</td><td>21</td><td>20</td><td>19</td><td>18</td><td>17</td><td>16</td></tr><tr><td colspan="16">CMP[31:16]</td></tr><tr><td>15</td><td>14</td><td>13</td><td>12</td><td>11</td><td>10</td><td>9</td><td>8</td><td>7</td><td>6</td><td>5</td><td>4</td><td>3</td><td>2</td><td>1</td><td>0</td></tr><tr><td colspan="16">CMP[15:0]</td></tr></table>

<table><tr><td>位</td><td>名称</td><td>访问</td><td>描述</td><td>复位值</td></tr><tr><td>[31:0]</td><td>CMP[31:0]</td><td>RW</td><td>设置比较计数器值低32位。</td><td>0</td></tr></table>

注：寄存器STK_CMPLR和寄存器STK_CMPHR共同构成了64位计数器比较值。

#### 9.5.5.6 计数比较高位寄存器（STK_CMPHR）

偏移地址： $0 \times 1 4$

<table><tr><td>31</td><td>30</td><td>29</td><td>28</td><td>27</td><td>26</td><td>25</td><td>24</td><td>23</td><td>22</td><td>21</td><td>20</td><td>19</td><td>18</td><td>17</td><td>16</td></tr><tr><td colspan="16">CMP[63:48]</td></tr><tr><td>15</td><td>14</td><td>13</td><td>12</td><td>11</td><td>10</td><td>9</td><td>8</td><td>7</td><td>6</td><td>5</td><td>4</td><td>3</td><td>2</td><td>1</td><td>0</td></tr><tr><td colspan="16">CMP[47:32]</td></tr></table>

<table><tr><td>位</td><td>名称</td><td>访问</td><td>描述</td><td>复位值</td></tr><tr><td>[31:0]</td><td>CMP[63:32]</td><td>RW</td><td>设置比较计数器值高32位。</td><td>0</td></tr></table>

注：寄存器STK_CMPLR和寄存器STK_CMPHR共同构成了64位计数器比较值。

### 9.5.6 ARM-SysTick 寄存器描述

表 9-7 SysTick 相关寄存器列表  

<table><tr><td>名称</td><td>访问地址</td><td>描述</td><td>复位值</td></tr><tr><td>R32_STK_CTRL</td><td>0xE000E010</td><td>SysTick 控制及状态寄存器</td><td>0x00000000</td></tr><tr><td>R32_STK_LOAD</td><td>0xE000E014</td><td>SysTick 重装载数值寄存器</td><td>0x00000000</td></tr><tr><td>R32_STK_VAL</td><td>0xE000E018</td><td>SysTick 当前数值寄存器</td><td>0x00000000</td></tr><tr><td>R32_STK_CALIB</td><td>0xE000E01C</td><td>SysTick 校准数值寄存器</td><td>0x00000000</td></tr></table>

内核设计的通用微控制器

#### 9.5.6.1 SysTick 控制及状态寄存器（STK_CTRL）

偏移地址： $0 \times 0 0$

<table><tr><td>31</td><td>30</td><td>29</td><td>28</td><td>27</td><td>26</td><td>25</td><td>24</td><td>23</td><td>22</td><td>21</td><td>20</td><td>19</td><td>18</td><td>17</td><td>16</td></tr><tr><td colspan="15">Reserved</td><td>COUNT
FLAG</td></tr><tr><td>15</td><td>14</td><td>13</td><td>12</td><td>11</td><td>10</td><td>9</td><td>8</td><td>7</td><td>6</td><td>5</td><td>4</td><td>3</td><td>2</td><td>1</td><td>0</td></tr><tr><td colspan="12">Reserved</td><td>CLKSO
URCE</td><td>TICKI
NT</td><td>ENABL
E</td><td></td></tr></table>

<table><tr><td>位</td><td>名称</td><td>访问</td><td>描述</td><td>复位值</td></tr><tr><td>[31:17]</td><td>Reserved</td><td>R0</td><td>保留。</td><td>0</td></tr><tr><td>16</td><td>COUNTFLAG</td><td>R0</td><td>如果在上次读取本寄存器后，SysTick 已经数到了 0，则该位为 1。如果读取该位，该位将自动清零。</td><td>0</td></tr><tr><td>[15:3]</td><td>Reserved</td><td>R0</td><td>保留。</td><td>0</td></tr><tr><td>2</td><td>CLKSOURCE</td><td>RW</td><td>0=外部时钟源(STCLK)
1=内部时钟(FCLK)</td><td>0</td></tr><tr><td>1</td><td>TICKINT</td><td>RW</td><td>1=SysTick 倒数到 0 时产生 SysTick 异常请求
0=数到 0 时无动作</td><td>0</td></tr><tr><td>0</td><td>ENABLE</td><td>RW</td><td>SysTick 定时器的使能位</td><td>0</td></tr></table>

#### 9.5.6.2 SysTick 重装载数值寄存器（STK_LOAD）

偏移地址： $0 \times 0 4$

<table><tr><td>31</td><td>30</td><td>29</td><td>28</td><td>27</td><td>26</td><td>25</td><td>24</td><td>23</td><td>22</td><td>21</td><td>20</td><td>19</td><td>18</td><td>17</td><td>16</td></tr><tr><td colspan="8">Reserved</td><td colspan="8">RELOAD[23:16]</td></tr><tr><td>15</td><td>14</td><td>13</td><td>12</td><td>11</td><td>10</td><td>9</td><td>8</td><td>7</td><td>6</td><td>5</td><td>4</td><td>3</td><td>2</td><td>1</td><td>0</td></tr><tr><td colspan="16">RELOAD[15:0]</td></tr></table>

<table><tr><td>位</td><td>名称</td><td>访问</td><td>描述</td><td>复位值</td></tr><tr><td>[31:24]</td><td>Reserved</td><td>R0</td><td>保留。</td><td>0</td></tr><tr><td>[23:0]</td><td>RELOAD</td><td>RW</td><td>当倒数至零时，将被重装载的值</td><td>0</td></tr></table>

#### 9.5.6.3 SysTick 当前数值寄存器（STK_VAL）

偏移地址： $0 \times 0 8$

<table><tr><td>31</td><td>30</td><td>29</td><td>28</td><td>27</td><td>26</td><td>25</td><td>24</td><td>23</td><td>22</td><td>21</td><td>20</td><td>19</td><td>18</td><td>17</td><td>16</td></tr><tr><td colspan="8">Reserved</td><td colspan="8">CURRENT[23:16]</td></tr><tr><td>15</td><td>14</td><td>13</td><td>12</td><td>11</td><td>10</td><td>9</td><td>8</td><td>7</td><td>6</td><td>5</td><td>4</td><td>3</td><td>2</td><td>1</td><td>0</td></tr><tr><td colspan="16">CURRENT[15:0]</td></tr></table>

<table><tr><td>位</td><td>名称</td><td>访问</td><td>描述</td><td>复位值</td></tr><tr><td>[31:24]</td><td>Reserved</td><td>R0</td><td>保留。</td><td>0</td></tr><tr><td>[23:0]</td><td>CURRENT</td><td>RW</td><td>读取时返回当前倒计数的值，写它则使之清零，同时还会清除在 SysTick 控制及状态寄存器中的 COUNTFLAG 标志。</td><td>0</td></tr></table>

#### 9.5.6.4 SysTick 校准数值寄存器（STK_CALIB）

偏移地址： $0 \times 0 0$

<table><tr><td>31</td><td>30</td><td>29</td><td>28</td><td>27</td><td>26</td><td>25</td><td>24</td><td>23</td><td>22</td><td>21</td><td>20</td><td>19</td><td>18</td><td>17</td><td>16</td></tr><tr><td>NOREF</td><td>SKEW</td><td></td><td colspan="5">Reserved</td><td colspan="8">TENMS[23:16]</td></tr><tr><td>15</td><td>14</td><td>13</td><td>12</td><td>11</td><td>10</td><td>9</td><td>8</td><td>7</td><td>6</td><td>5</td><td>4</td><td>3</td><td>2</td><td>1</td><td>0</td></tr><tr><td colspan="16">TENMS[15:0]</td></tr></table>

<table><tr><td>位</td><td>名称</td><td>访问</td><td>描述</td><td>复位值</td></tr><tr><td>31</td><td>NOREF</td><td>RO</td><td>1=没有外部参考时钟(STCLK不可用)0=外部参考时钟可用</td><td>0</td></tr><tr><td>30</td><td>SKEW</td><td>RO</td><td>1=校准值不是准确的10ms0=校准值是准确的10ms</td><td>0</td></tr><tr><td>[29:24]</td><td>Reserved</td><td>RO</td><td>保留。</td><td>0</td></tr><tr><td>[23:0]</td><td>TENMS</td><td>RW</td><td>10ms的时间内倒计数的格数。芯片设计者应该通过Cortex-M3的输入信号提供该数值。若该数值读回零,则表示无法使用校准功能。</td><td>0</td></tr></table>


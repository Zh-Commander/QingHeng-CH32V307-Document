# NAND控制器波形和时序

测试条件：NAND操作区域，选择16位数据宽度，使能ECC计算电路，512字节页面大小，其他时序配置为设置寄存器 $\mathsf { F S M C \_ P C R 2 = 0 \times 0 0 0 2 0 0 5 E }$ ，FSMC_PMEM2=0x01020301，FSMC_PATT2=0x01020301。

![](images/edb0bab3d0ef95a37813e7e0b9d2117a819282cad2ae1d2c10c2dcbeeb442978.jpg)  
图 4-20 NAND 控制器读操作波形

![](images/16bb96bf98ef12fdc58ef2270ff23315cebcac9b96d4752d6ef20cde1b9b2087.jpg)  
图 4-21 NAND 控制器写操作波形

![](images/d628b3ba944eb5da3e60519d116dc7f89a373c4317effeea6b1001d7907c393c.jpg)  
图4-22 NAND控制器在通用存储空间的读操作波形

![](images/9af6ba1d49d30aa0cb7530a7768e5eaa5c73dab901479de013e65265131842cd.jpg)  
图 4-23 NAND 控制器在通用存储空间的写操作波形

表4-36 NAND闪存读写周期的时序特性  

<table><tr><td>符号</td><td>参数</td><td>最小值</td><td>最大值</td><td>单位</td></tr><tr><td>td(D-NWE)</td><td>FSMC_NWE高之前至FSMC_D[15:0]数据有效</td><td>4thCLK</td><td></td><td rowspan="11">ns</td></tr><tr><td>tw(NOE)</td><td>FSMC_NOE低时间</td><td>4thCLK</td><td></td></tr><tr><td>tsu(D-NOE)</td><td>FSMC_NOE高之前至FSMC_D[15:0]数据有效</td><td>20</td><td></td></tr><tr><td>th(NOE-D)</td><td>FSMC_NOE高之后至FSMC_D[15:0]数据有效</td><td>15</td><td></td></tr><tr><td>tw(NWE)</td><td>FSMC_NWE低时间</td><td>4thCLK</td><td></td></tr><tr><td>tv(NWE-D)</td><td>FSMC_NWE低至FSMC_D[15:0]数据有效</td><td>0</td><td></td></tr><tr><td>th(NWE-D)</td><td>FSMC_NWE高至FSMC_D[15:0]数据无效</td><td>2thCLK</td><td></td></tr><tr><td>td(ALE-NWE)</td><td>FSMC_NWE低之前至FSMC_ALE有效</td><td>2thCLK</td><td></td></tr><tr><td>th(NWE-ALE)</td><td>FSMC_NWE高至FSMC_ALE无效</td><td>2thCLK</td><td></td></tr><tr><td>td(ALE-NOE)</td><td>FSMC_NOE低之前至FSMC_ALE有效</td><td>2thCLK</td><td></td></tr><tr><td>th(NOE-ALE)</td><td>FSMC_NOE高至FSMC_ALE无效</td><td>4thCLK</td><td></td></tr></table>


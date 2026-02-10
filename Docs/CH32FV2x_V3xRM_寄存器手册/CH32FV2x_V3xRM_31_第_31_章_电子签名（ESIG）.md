# 第 31 章 电子签名（ESIG）

本章模块描述适用于CH32F2x、CH32V2x和 $_ { C H 3 2 V 3 x }$ 微控制器全系列产品。

电子签名包含了芯片识别信息：闪存区容量和唯一身份标识。它由厂家在出厂时烧录到存储器模块的系统存储区域，可以通过 SWD（SDI）或者应用代码读取。

## 31.1 功能描述

闪存区容量：指示当前芯片用户应用程序可以使用大小。

唯一身份标识：96 位二进制码，对任意一个微控制器都是唯一的，用户只能读访问不能修改。此唯一标识信息可以用作微控制器（产品）的安全密码、加解密钥、产品序列号等，用来提高系统安全机制或表明身份信息。

以上内容用户都可以按 8/16/32 位进行读访问。

## 31.2 寄存器描述

表 31-1 ESIG 相关寄存器列表  

<table><tr><td>名称</td><td>访问地址</td><td>描述</td><td>复位值</td></tr><tr><td>R16_ESIG_FLACAP</td><td>0x1FFF7E0</td><td>闪存容量寄存器</td><td>0xXXXX</td></tr><tr><td>R32_ESIG_UNI ID1</td><td>0x1FFF7E8</td><td>UID 寄存器1</td><td>0xXXXXXXXXX</td></tr><tr><td>R32_ESIG_UNI ID2</td><td>0x1FFF7EC</td><td>UID 寄存器2</td><td>0xXXXXXXXXX</td></tr><tr><td>R32_ESIG_UNI ID3</td><td>0x1FFF7F0</td><td>UID 寄存器3</td><td>0xXXXXXXXXX</td></tr></table>

### 31.2.1 闪存容量寄存器（ESIG_FLACAP）

<table><tr><td>15</td><td>14</td><td>13</td><td>12</td><td>11</td><td>10</td><td>9</td><td>8</td><td>7</td><td>6</td><td>5</td><td>4</td><td>3</td><td>2</td><td>1</td><td>0</td></tr><tr><td colspan="16">F_SIZE[15:0]</td></tr></table>

<table><tr><td>位</td><td>名称</td><td>访问</td><td>描述</td><td>复位值</td></tr><tr><td>[15:0]</td><td>F_SIZE</td><td>R0</td><td>以Kbyte为单位的闪存容量。例:0x0080=128K字节</td><td>x</td></tr></table>

### 31.2.2 UID 寄存器（ESIG_UNIID1）

<table><tr><td>31</td><td>30</td><td>29</td><td>28</td><td>27</td><td>26</td><td>25</td><td>24</td><td>23</td><td>22</td><td>21</td><td>20</td><td>19</td><td>18</td><td>17</td><td>16</td></tr><tr><td colspan="16">U_ID[31:16]</td></tr><tr><td>15</td><td>14</td><td>13</td><td>12</td><td>11</td><td>10</td><td>9</td><td>8</td><td>7</td><td>6</td><td>5</td><td>4</td><td>3</td><td>2</td><td>1</td><td>0</td></tr><tr><td colspan="16">U_ID[15:0]</td></tr></table>

<table><tr><td>位</td><td>名称</td><td>访问</td><td>描述</td><td>复位值</td></tr><tr><td>[31:0]</td><td>U_ID[31:0]</td><td>R0</td><td>UID的0-31位。</td><td>x</td></tr></table>

### 31.2.3 UID 寄存器（ESIG_UNIID2）

<table><tr><td>31</td><td>30</td><td>29</td><td>28</td><td>27</td><td>26</td><td>25</td><td>24</td><td>23</td><td>22</td><td>21</td><td>20</td><td>19</td><td>18</td><td>17</td><td>16</td></tr><tr><td colspan="16">U_ID[63:48]</td></tr><tr><td>15</td><td>14</td><td>13</td><td>12</td><td>11</td><td>10</td><td>9</td><td>8</td><td>7</td><td>6</td><td>5</td><td>4</td><td>3</td><td>2</td><td>1</td><td>0</td></tr><tr><td colspan="16">U_ID[47:32]</td></tr></table>

<table><tr><td>位</td><td>名称</td><td>访问</td><td>描述</td><td>复位值</td></tr><tr><td>[31:0]</td><td>U_ID[63:32]</td><td>R0</td><td>UID的32-63位。</td><td>x</td></tr></table>

### 31.2.4 UID 寄存器（ESIG_UNIID3）

<table><tr><td>31</td><td>30</td><td>29</td><td>28</td><td>27</td><td>26</td><td>25</td><td>24</td><td>23</td><td>22</td><td>21</td><td>20</td><td>19</td><td>18</td><td>17</td><td>16</td></tr><tr><td colspan="16">U_ID[95:80]</td></tr><tr><td>15</td><td>14</td><td>13</td><td>12</td><td>11</td><td>10</td><td>9</td><td>8</td><td>7</td><td>6</td><td>5</td><td>4</td><td>3</td><td>2</td><td>1</td><td>0</td></tr><tr><td colspan="16">U_ID[79:64]</td></tr></table>

<table><tr><td>位</td><td>名称</td><td>访问</td><td>描述</td><td>复位值</td></tr><tr><td>[31:0]</td><td>U_ID[95:64]</td><td>R0</td><td>UID的64-95位。</td><td>x</td></tr></table>


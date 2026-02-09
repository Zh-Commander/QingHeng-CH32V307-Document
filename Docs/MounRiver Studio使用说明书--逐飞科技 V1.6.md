# MounRiver Studio 环境使用说明

## 目录

### 目录..

### 1. 使用 MounRiver Studio 导入工程. 4

1.1. 导入工程 . . 4  
1.2. 重命名工程..  
1.3. 编译工程并进行调试.. . 6  
1.4. 修改了工程名称点击下载提示HEX不存在，请检查. . 10

1.4.1. 下载提示 does not exist.Please check. .10   
1.4.2. 解决方案 . ： ... .10

### 2. 编译、调试与下载. 12

2.1. 编译工程 . 12  
2.2. 仅下载程序.. 12   
2.3.调试工程 . 13  
2.4. 单步调试与断点功能. 3  
2.5. 变量查看功能. 14  
2.6. 内存查看功能. 15  
2.7. 开启多核编译. ... 17

### 3. 工作空间管理. .19

3.1. 切换工程 . . 19

3.2. 打开或关闭工程. . 20  
3.3. 删除工程 . . 21  
3.4. 新增文件 . . 21

### 4. 环境设置 .24

4.1. 字体设置 . . 24   
4.2. 拼写报错屏蔽设置. . 25

### 5. 常见问题. .26

5.1. 如何将 MRS 设置为中文。.. . 26  
5.2. 使用在线调试报错....... . 26  
5.3. 弹出 Luanch failed.Binary not found . 27   
5.4. 核心板被锁，如何解锁..... 27

5.4.1. 核心板被锁定有以下几种状态： .27  
5.4.2. 如何解锁： .28

5.5. 无法进入在线调试.. . 29

5.5.1. 工程目录有中文路径。 ... ..... ....... ..... . 29  
5.5.2. 在线调试的时候显示 …….... .30

5.6. 无法添加 math 库. .. 31   
5.7. 点击下载提示 Target file”obj\xxxx.hex”does not exist.Please check..... . 31

5.7.1. 下载提示 does not exist.Please check. ...... . 31   
5.7.2. 解决方案 .. 32

### 6. MounRiver Studio IDE 下载方式 . .35

6.1. 百度云盘下载.. . 35

6.2. 官网下载 . . 35  
7. 文档版本 .36

### 1.使用 MounRiver Studio 导入工程

安装完成MounRiver Studio（在后面的描述中简称为MRS）后并打开，安装的时候请务必保证 ADS 安装路径没有中文与空格，只允许使用英文、数字与下划线！初次打开需要选定工作空间路径，请注意工作空间路径不要包含中文以及空格，只允许使用英文、数字与下划线！

本章节介绍如何使用MRS导入现有工程，以及如何导入CH32V103R8T6工程并进行调试编译。本文仅作为参考，仅针对 CH32V103R8T6 芯片，以及逐飞科技 CH32V103R8T6 开源库。请严格按照手册执行，避免出现额外的问题。

#### 1.1.导入工程

这里我们已 CH32V103R8T6 开源库为例，打开下载好的开源库文件夹。

![](images/e7fd7baab0f8ae3ed1c4eb7014e76801578f8758f7908d0ae95a34c49dc2ab81.jpg)

双击打开 Seekfree_CH32V103R8T6_Opensource_Library.wvproj。

![](images/6f8ec04069c43a97b4580f8c7e56a17c7047e3f2c42a6b03977a87b6465352be.jpg)

#### 1.2.重命名工程

回到 MRS，此时在 MRS 中可以看到导入的工程，双击打开导入的工程，此时可以进行正常的编译调试。如果需要重命名工程，可以按照如下进行操作：

![](images/d6625d2d5745a7883db17adf47f70887c26f5792f81c9e748767bd29c7d23127.jpg)

在弹出的窗口重命名工程后即可继续操作。

#### 1.3.编译工程并进行调试

![](images/543f4de40c6dd56c384fbcb137ce2273002bc2afd1c67f990c4d8e344fda7348.jpg)

![](images/49049968611af55ca3771fabf34bf14754c5e098ba9a1a59d34b71d17a942a09.jpg)

在进行调试工程前请务必确认已经正确连接调试下载器！并且连接好了核心板！

在进行调试工程前请务必确认已经正确连接调试下载器！并且连接好了核心板！

在进行调试工程前请务必确认已经正确连接调试下载器！并且连接好了核心板！

工程编译完成后，右键单击工程，找到 Debug 选项菜单旁边的下三角形，在菜单中找到Debug Configuration 选项。

![](images/9e59dbe9d2bdf1d5b73c65125d9a46c6a8c43cdb3d5dccb9fd6e2802262de52b.jpg)

![](images/eae9afe91ac42a17328ed9d3a31596c5b54ef4903483d6fa2a6a619bdb3383b1.jpg)

![](images/2c8f9e7dd9f303f463f00959b9972e605f9845786e20a55590a20b036a76a196.jpg)

此时 IDE 会进行界面的切换，进入 Debug 窗口。

![](images/764317dd2c586d0216331678a3eec2c5e341652b903e287e06a3e2f5fd1bbac0.jpg)

完成初次调试后，再次调试可以按照如下进行操作，直接进入调试界面。

![](images/3047ef132604c4548f22c874ffa72a60053a61a941583031234eb99154bf16a7.jpg)

至此导入以及编译调试整个流程完成。

#### 1.4.修改了工程名称点击下载提示 HEX 不存在，请检查

#### 1.4.1.下载提示 does not exist.Please check

提示 Target file”obj\xxxx.hex”does not exist.Please check。这个是修改了工程文件夹名称没有修改 HEX 路径。

![](images/0ddb84fa3dfdad6f22f1be8200504867eba88b6206204aade7a6c3b62ad5551d.jpg)

#### 1.4.2.解决方案

![](images/20f3a3281332a645163e4b82164aff5492bcfa1906a94bd65d12359f0d6c4a91.jpg)

![](images/074193c88668f54b624d1e16f98a1f617e72b2c21da36620478ed3c57c85e9b1.jpg)

### 2.编译、调试与下载

#### 2.1.编译工程

编译工程步骤与其他环境差别不大，方式为：

1. 通过右键工程选择 Build Project 选项；  
2. 上方选项栏中 Build 或 Rebuild 选项 ；

下方Console选项卡会输出编译步骤以及最终耗时，Problems会输出错误以及警告信息。

![](images/0b790337c810823b956e503d8a44e382edd41ca46c64dc95540fb19f24034095.jpg)

#### 2.2.仅下载程序

MRS这个IDE支持仅下载程序到Flash的功能，第一次打开工程，务必配置下载模式。配置方式如下：

workspace-Seekfree_CH32V103R8T6_OpensourceLibrary/Libraries/wchlibraries/Startup/startup_ch32v10x.S-MounRiverStudio   
File Edit Project Flash Run Window Help

![](images/a2336a6198c85e4b0b6d981d0e3d63ae97d37612d3b67409bdd34598698ea6d5.jpg)

![](images/4c1d3be3b9e1716a7834c4a25beec55773e0cecfbd118cc162c2db8db3af912e.jpg)

配置完成后，再点击 按钮，即可进行下载。

#### 2.3.调试工程

在进行调试工程前请务必确认已经正确连接调试下载器！并且连接好了核心板！

在进行调试工程前请务必确认已经正确连接调试下载器！并且连接好了核心板！

在进行调试工程前请务必确认已经正确连接调试下载器！并且连接好了核心板！

调试工程可点击工具栏的 Debug 按键，或者点击 DEBUG 旁边的小箭头然后选择Launch为后缀的选项，进入调试。

初次调试时务必进行进入 DEBUG 配置选项进行配置一次。

#### 2.4.单步调试与断点功能

设置断点可以通过在需要设置断点的行数左侧双击设置断点。

![](images/484c278e7c07af734d10611338f1aecf0b584bf0cce9c214143e2aa7c628da3b.jpg)

右键该位置可以进行断点的相关操作，例如取消、启用、屏蔽等。

![](images/998c607683c3bfa1f6f8ec3270e8cf71a88922db4f81671df37212db671297b9.jpg)

#### 2.5.变量查看功能

在调试界面内，右键单击要监控的变量选择Add Watch Expression，添加方式如下：

![](images/f31030421ef1ca36df59ef51e2e2b638896ed9728b6bd6a660489d57642ded78.jpg)

#### 2.6.内存查看功能

MRS同样提供了内存查看的工具，可以直接查看指定地址下的数据情况，在下方<默认下方>的 Memory 选项卡可以通过 Monitors 功能添加映射地址，在 New Renderings...选项卡可以添加不同的数据格式，具体操作如下图： →

![](images/0c679993fa32dae142ce911296304b7f49d1addf3e4c1b660be893221b4a7955.jpg)

在New Renderings...选项卡可以针对一个地址添加不同的数据格式，具体操作如下图：

![](images/6892b4f8db9d5bfd834e6a05701c07cec1012500ac7f1d8fc963bce66dec958a.jpg)

#### 2.7.开启多核编译

![](images/84cb201f8b80bc534e6a7bd119a7ed90000a2e94ad940b6b1096cb0c0d01de5e.jpg)

![](images/414dd57cbee38e069b5d805b9872e6e3915b30b7a7ac51c1e4f7b4bbba87da8e.jpg)

### 3.工作空间管理

#### 3.1.切换工程

当你的工作空间中留存有许多的工程时，需要注意右边的Project Explorer下面选择的是哪一个工程，同时，Console窗口中也会提示当前活动工程是什么，下面表示当前该工程处于活动状态，编译、调试等操作会针对于该工程进行。

![](images/c6c7473db2bb943899751fe3fee09957fe5ba7de5b62b6d48f52b39a1d20a27f.jpg)

当我们需要切换工程的时候，只需要点击该工程的任意文件即可切换活动工程。

![](images/e354b60072d74ad054ff222186dfe697a45610e06b06574eb80fb4bff72b5753.jpg)

#### 3.2.打开或关闭工程

工程处于打开状态时，图标显示为带折叠箭头的打开的文件夹，工程处于关闭状态时，图标显示为不带折叠箭头的关闭的文件夹。

![](images/a54634fec25b5f226881345d37bafdda751d9bee5c5358ac52d7a843b787f1b4.jpg)

工作空间内工程处于打开状态时，使用 Build ALL操作时会对所有打开工程进行编译操作，请务必注意。

关闭工程的操作为：右键工程->Close Project。打开工程的操作为：双击工程。

![](images/4c692f95a3b31aaf81a0a42ac2f4981463503e67cadc5f17d821ff12c79e3b26.jpg)

#### 3.3.删除工程

如果当前工作空间内有工程不需要进行修改、调试时，可以将其移除工程，操作为：右键工程->Delete。此时会弹出 Delete Resources 窗口。

如果勾选 Delete project contents on disk(cannot be undone)选项会从工作空间中移除该工程并且从磁盘<硬盘>中彻底删除该工程，请谨慎操作！

![](images/15e2c0adf676fed6d17f298211f50bf5781d1d8d0f94c5cf79763b71c4fab170.jpg)

如果不勾选该选项，则只从工作空间删除该工程，可以在双击目录下的xxxx.wvproj文件进行导入。

#### 3.4.新增文件

如果需要向工程中添加文件，需要注意包含路径的问题，我们的库中已经设置好了相应的包

![](images/595a8ebe20d38b062655e6d4799b784c4a8641330ce2e5f6ee1a8450f425f931.jpg)  
含路径，包含路径查看如下：

![](images/fbb3ab83ce30c96868fcf66cd8c31076e602da2741990aa7c9db4ea464711006.jpg)

在已添加包含路径的文件夹中添加文件的方式操作极为简单：

![](images/8c6f3735e8fee8a23b8712edf6ecb405281eec5cccb6b6dc8939363545c7f4ce.jpg)

### 4.环境设置

#### 4.1.字体设置

MRS 默认的字体是非等宽的字体，这样会导致代码对齐变得困难，并且格式变得混乱，显得代码格式杂乱，不美观，不方便查找与编写。

所以我们需要重新设置字体，使用等宽字体保证代码的美观性与可阅读性。

![](images/83eededf7f8b7ee6d6d99edc485d1e30685d64960c17aa242d069b7267f1ef75.jpg)

![](images/656c2c0b0495a436bf3c7c1269491d47c1e94681af49607bbc7d95f48bf7e8ab.jpg)

#### 4.2.拼写报错屏蔽设置

在ADS 中默认对注释也有拼写检查，会在注释里提示拼写错误：

![](images/81b436f7c96a506f6d57203df7119afc182d06f68942eca277070c07a802cc64.jpg)

按照下述方法将拼写报错关闭即可。

![](images/3f50b64a82312b428b89ee076fffd9c24d11a72329b2b6e633a5030065e2be3b.jpg)

![](images/c337c35278027a8cdfe46420560fdab4ac80debb97d97ca63bdbb480cede071c.jpg)

完成后不再提示拼写错误：

60 //获取gpio状态  
61gpio_status $\equiv$ gpio_get(P21_2);  
62 //将gpio状态打印到FSS窗口  
63printf("gpio_status:%d\n",gpio_status);  
64

### 5.常见问题

#### 5.1.如何将 MRS 设置为中文。

workspace - Seekfree_CH32V103R8T6_Opensource_Library/USER/main.c - MounRiver Studio

![](images/5225ce34d84f869b3dbf5556b45c746c8221a90def3eb06b1ed3a12725a2d55b.jpg)

#### 5.2.使用在线调试报错

![](images/956acffd5b387465ba072f7262ba3759620822c7e9ece017e5ed08baf0fc1a01.jpg)

该报错为 ERROR:WLink Open Error，也就是下载器没有连接到电脑上。

#### 5.3.弹出 Luanch failed.Binary not found

![](images/679a4d830d830345cc56b285505aca53fdb7acdad5b26308bcbd0507e20b923d.jpg)

查看 1.3 章节重新配置一次 Debug 选项，即可。

#### 5.4.核心板被锁，如何解锁

#### 5.4.1.核心板被锁定有以下几种状态：

点击下载时候：

Download Output Console

09:32:35:036 >> Send Chip Type Success

09:32:35:036 >> Starting to Check Read-Protect Status...

09:32:35:132 >> WCH-Link failed to connect with chip

WCH-Link failed to connect with chip

09:32:35:132 >> Starting to Close Device..

09:32:35:132 >> Close Device Success

-End-

点击在线调试时候：

```txt
<terminated> 9-PWM Demo Built-in Launch [GDB OpenOCD Debugging] openocd.exe  
Licensed under GNU GPL v2  
For bug reports, read  
http://openocd.org/doc/doxygen/bugs.html  
Info: only one transport option; autoselect 'jtag  
Ready for Remote Connections  
Started by GNU MCU Eclipse  
Info: Listening on port 6666 for tcl connections  
Info: Listening on port 4444 for telnet connection  
Error: WCH-Link failed to connect with chip 
```

#### 5.4.2.如何解锁：

一、首先打开核心板例程(开源库库例程)2-LED Blink Demo，  
二、然后将核心板断电，然后按住BOT按键，再给核心板上电，上电后再松开BOT按键。或者按住核心板上的 BOT 按键，再按 RST 按键，松开 BOT 按键,再松开 RST 按键。  
三、打开 IDE 进行如下操作：

![](images/29b3b964f8ba744888fa22ecfafa4d4036e6064d24fb3d21b1854c6b9228d9da.jpg)

![](images/2f1ed978a9fd8f2f7a97bf51148efa4b1f6dabccf0eb8350345c0c47ded119fe.jpg)

操作完成后，点击下载 按钮，将工程里面的代码下载进核心板，核心板开始以 500ms的频率将LED翻转一次。

#### 5.5.无法进入在线调试

#### 5.5.1.工程目录有中文路径。

无法在"Edesktop\\326330434263265\CH32V3RTasteoardexample\5-MotorControRV87DemoLbraries\chbaris\Startup\startup ch32v10x.S"找到一个源文件。定位该文件或者编辑源代码查找路径以包含它的位置

查看反汇编..

定位文件...

编辑源代码查找路径..

Configure when this editor is shown 首选项...

无法在"D:\51\32455\30\15\742263043326\26\3143236\Seekfree_CH32V3ROpensuebay\USER\main.c"找到一个源文件。定位该文件或者编辑源代码查找路径以包含它的位置。

查看反汇编...

定位文件..

编辑源代码查找路径.

Configure when this editor is shown

首选项...

在线调试中出现一堆\xxx\xxx\xxx......说明你的工程目录下面有中文或者中文标点符号，此时无法正常调试。不管是工程，还是 MounRiver Studio 软件都不允许有任何中文路径。

移动工程的时候，务必先删除 MounRiver Studio 软件里面的工程，然后执行双击一次 <删除临时文件.bat> 程序。最后再重新打开工程。

#### 5.5.2.在线调试的时候显示

```txt
<terminated> 9-PWM Demo Built-in Launch [GDB OpenOCD Debugging] openocd.exe  
Licensed under GNU GPL v2  
For bug reports, read  
http://openocd.org/doc/doxygen/bugs.html  
Info: only one transport option; autoselect 'jtag'  
Ready for Remote Connections  
Started by GNU MCU Eclipse  
Info: Listening on port 6666 for tcl connections  
Info: Listening on port 4444 for telnet connections  
Error: WCH-Link failed to connect with chip 
```

一、将核心板断电，然后按住 BOT 按键，再给核心板上电，上电后再松开 BOT 按键。（核心板插在主板上面，请将主板也断电）

二、再一次点击 DEBUG。

#### 5.6.无法添加 math 库

![](images/00489813d94cf73f449d1df45b7e5d3253f455ba83f3bdc741929adfe1317c6d.jpg)

#### 5.7.点击下载提示 Target file”obj\xxxx.hex”does not exist.Please check

#### 5.7.1.下载提示 does not exist.Please check

提示 Target file”obj\xxxx.hex”does not exist.Please check。这个是修改了工程文件夹名称

#### 没有修改 HEX 路径。

![](images/e5fc29147c18aee5f3f5c852e7d0d69644d316e6a7e51652eb059afb56165419.jpg)

#### 5.7.2.解决方案

![](images/84df9c0b12f1129ef3b4e6eff4ce42e585acc8ca2a66270aa5502eb87ad69b83.jpg)

![](images/c734eb8855505c7faeb1cbafd9c0d416b5308a3b5b4ab9f8cc27e461c2cdf064.jpg)

![](images/3f1900ae21e43f0356b6a319c378e8be96fc4023036b3261d92dbe6090040efe.jpg)

### 6.MounRiver Studio IDE 下载方式

#### 6.1.百度云盘下载

链接：https://pan.baidu.com/s/1TvcVJDRFae2TUZg-ke9IZg

提取码：celd

#### 6.2.官网下载

打开该 http://www.mounriver.com，找到下载按钮即可下载。这里我们推荐使用 MRS 1.3.1版本的IDE。

### 7.文档版本

<table><tr><td>版本号</td><td>日期</td><td>内容变更</td></tr><tr><td>V1.0</td><td>2020-12-17</td><td>初始版本</td></tr><tr><td>V1.1</td><td>2020-12-19</td><td>修改MounRiver Studio 下载方式</td></tr><tr><td>V1.2</td><td>2020-12-23</td><td>修改打开或者关闭工程</td></tr><tr><td>V1.3</td><td>2021-01-07</td><td>修改解锁说明</td></tr><tr><td>V1.4</td><td>2021-01-14</td><td>增加解锁方式</td></tr><tr><td>V1.5</td><td>2021-01-26</td><td>增加无法进入在线调试说明</td></tr><tr><td>V1.6</td><td>2021-05-07</td><td>增加 HEX 不存在解决方案
增加开启多核编译方案</td></tr></table>
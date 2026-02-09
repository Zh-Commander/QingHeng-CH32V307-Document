# WCHNET WEB 配置程序说明

## 1. 程序功能

本程序实现的是 WCHNET WEB 网页配置的功能，具体如下：

### 1.1 用户登录界面

浏览器地址栏里输入WCHNETWEB的IP地址即可打开登陆界面。在用户输入正确的用户名和密码后跳转到WCHNET WEB的配置界面。默认用户名为“admin”，密码为“123”，也可以通过配置界面来修改用户名与密码，配置的参数在重启单片机后生效。

![](images/953226039c823ed9b055f706ce870a4984f5872ff3d6310f092fc97b58006761.jpg)

### 1.2 网络配置界面

这是本程序的主要功能，程序一共提供了 3 个配置子网页：基础设置、端口设置、密码设置。通过主界面左侧的按钮可以自由切换每个子网页，在子网页中可以获取 WCHNET WEB当前的状态参数，同时可以重新配置WCHNET WEB的网络参数与工作模式，配置的参数在重启单片机后生效。

![](images/6942ad57ce599173c589a17b16c4a93d8b1857f8a514611ceed087f853665d6e.jpg)

### 1.3 支持手动恢复出厂设置

当用户忘记配置的参数（例如用户名与密码）导致 WCHNET WEB 不能正常工作时，可以通过 PB6来恢复默认配置，低电平有效。

## 2. 程序说明

### 2.1 WEB 界面配置通讯协议

基础设置界面（Html_basic）  

<table><tr><td>配置项</td><td>配置名</td><td>配置值</td></tr><tr><td>设备MAC</td><td>__PMAC</td><td>__AMAC</td></tr><tr><td>设备IP</td><td>__PSIP</td><td>__ASIP</td></tr><tr><td>子网掩码</td><td>__PMSK</td><td>__AMSK</td></tr><tr><td>网关</td><td>__PGAT</td><td>__AGAT</td></tr></table>

端口设置界面(Html_port)   

<table><tr><td>配置项</td><td>配置名</td><td>配置值</td></tr><tr><td>网络模式</td><td>__PMOD</td><td>__AMOD</td></tr><tr><td>本地端口</td><td>__PSPT</td><td>__ASPT</td></tr><tr><td>目的IP</td><td>__PDIP</td><td>__ADIP</td></tr><tr><td>目的端口号</td><td>__PDPT</td><td>__ADPT</td></tr></table>

密码管理界面（Html_user）  

<table><tr><td>配置项</td><td>配置名</td><td>配置值</td></tr><tr><td>用户名</td><td>_PUSE</td><td>_AUSE</td></tr><tr><td>密码</td><td>_PPAS</td><td>_APAS</td></tr></table>

### 2.2 浏览器请求 Request 报文解析

浏览器 request 报文结构组成：

【Method】空格【Request-URL】空格【HTTP-Version】CRLF

CRLF

【Message-Body】

【method】：主要有 POST 与 GET 两种请求方式，配置界面都是发送 post 请求

【Request-URL】：浏览器想要获取的内容，该内容以数组常量的形式保存在单片机 flash 中。

【HTTP-Version】：本程序使用的是 HTTP/1.1 版本

CRLF:回车换行符。

【Message-Body】：消息主体，本例程里，只有 POST请求才会包含有消息主体，主体内容就是配置界面（Html_basic、Html_port、Html_user）提交的配置信息,格式为：

配置名 1=配置值 1&配置名 2=配置值 2&配置名 3=配置值 3……

例如：密码管理界面上提交用户名：admin；密码：admin。则浏览器 post 请求里的消息主体是：__PUSE=admin&__PPAS=admin。更多内容可以参考 HTTP 协议（RFC2616）

### 2.3 WEB 服务器响应报文解析

面对浏览器的一次请求，WCHNET WEB 服务器需要先发送 http-response 响应报文，然后才发送URL 资源文件。响应报文的格式可以参照 HTTP 协议（RFC2616），本程序中已经在 HTTPS.H 文件中定义好相关响应报文。

### 2.4 Html 网页内容

本程序采用的html网页以“html+css+javascript”的形式编写，结构简单，以字符串的形式保存在单片机 flash里(Html_main……)。图片格式（PNG、GIF）文件都是以十六进制数组常量保存。

### 2.5 WCHNET WEB 发送 URL 资源文件注意事项

WCHNET WEB发送网页时要分为两种情况考虑：

1.发送 URL 里没有配置信息，例如：图片文件或者一部分网页（“关于沁恒”）,WCHNET WEB 直接将该URL文件从FLASH里复制到单片机RAM里进行发送。  
2.发送 URL 里包含有配置信息，例如三个配置子网页：“基础设置”，“端口设置”，“密码设置”。发送之前 WCHNET WEB 需要先将 flash 里的 URL 文件与程序里的一个配置参数表进行对比替换，该参数表是一个结构体数组，每个结构体包含两部分：被替换的字符串（“__Axxx”）与替换后的最新值。替换过程中需要将htmlURL文件里“__A”开头的字符串替换掉。
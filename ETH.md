# CH32V307 ETH以太网外设开发指南

本文档基于CH32V307 EVT示例代码分析，总结以太网外设的使用方法和配置要点。

## 一、概述

CH32V307内置以太网控制器，支持10/100Mbps速率，提供完整的TCP/IP协议栈（WCHNET）。

### 主要特性：
- 支持10/100Mbps自适应
- 支持RMII接口
- 内置MAC和DMA控制器
- 完整的TCP/IP协议栈
- 支持多种网络协议

## 二、硬件连接

### 2.1 引脚映射
```
RMII接口：
  REF_CLK  → PA1
  MDIO     → PA2
  MDC      → PC1
  TXD0     → PB12
  TXD1     → PB13
  TX_EN    → PB11
  RXD0     → PC4
  RXD1     → PC5
  CRS_DV   → PA7
```

### 2.2 外部PHY要求
需要外接PHY芯片（如LAN8720A），典型连接方式：
```
CH32V307 RMII接口 ↔ PHY芯片 ↔ RJ45接口
```

## 三、软件配置基础

### 3.1 网络参数配置
```c
// 基本网络参数
u8 MACAddr[6];                    // MAC地址
u8 IPAddr[4] = {192, 168, 1, 10}; // IP地址
u8 GWIPAddr[4] = {192, 168, 1, 1}; // 网关
u8 IPMask[4] = {255, 255, 255, 0}; // 子网掩码
```

### 3.2 以太网库初始化
```c
// 1. 获取MAC地址
WCHNET_GetMacAddr(MACAddr);

// 2. 初始化TIM2（网络时钟）
TIM2_Init();

// 3. 以太网库初始化
u8 i = ETH_LibInit(IPAddr, GWIPAddr, IPMask, MACAddr);
if(i == 0)
    printf("ETH_LibInit Success\n");
else
    printf("ETH_LibInit Error: %d\n", i);
```

### 3.3 TIM2初始化（网络时钟）
```c
void TIM2_Init(void)
{
    TIM_TimeBaseInitTypeDef TIM_TimeBaseStructure = {0};
    RCC_APB1PeriphClockCmd(RCC_APB1Periph_TIM2, ENABLE);

    TIM_TimeBaseStructure.TIM_Period = SystemCoreClock / 1000000;
    TIM_TimeBaseStructure.TIM_Prescaler = WCHNETTIMERPERIOD * 1000 - 1;
    TIM_TimeBaseStructure.TIM_ClockDivision = 0;
    TIM_TimeBaseStructure.TIM_CounterMode = TIM_CounterMode_Up;
    TIM_TimeBaseInit(TIM2, &TIM_TimeBaseStructure);
    TIM_ITConfig(TIM2, TIM_IT_Update, ENABLE);

    TIM_Cmd(TIM2, ENABLE);
    TIM_ClearITPendingBit(TIM2, TIM_IT_Update);
    NVIC_EnableIRQ(TIM2_IRQn);
}
```

## 四、网络任务处理

### 4.1 主循环结构
```c
int main(void)
{
    // 初始化代码...

    while(1)
    {
        WCHNET_MainTask();           // 网络主任务

        if(WCHNET_QueryGlobalInt())  // 查询全局中断
        {
            WCHNET_HandleGlobalInt(); // 处理全局中断
        }

        // 其他任务处理...
    }
}
```

### 4.2 中断处理
```c
void WCHNET_HandleGlobalInt(void)
{
    u8 intstat = WCHNET_GetGlobalInt();

    if(intstat & GINT_STAT_UNREACH)    // 不可达中断
    {
        printf("GINT_STAT_UNREACH\n");
        WCHNET_ClearGlobalInt(GINT_STAT_UNREACH);
    }

    if(intstat & GINT_STAT_IP_CONFLI)  // IP冲突
    {
        printf("GINT_STAT_IP_CONFLI\n");
        WCHNET_ClearGlobalInt(GINT_STAT_IP_CONFLI);
    }

    if(intstat & GINT_STAT_PHY_CHANGE) // PHY状态变化
    {
        printf("GINT_STAT_PHY_CHANGE\n");
        WCHNET_ClearGlobalInt(GINT_STAT_PHY_CHANGE);
    }

    if(intstat & GINT_STAT_SOCKET)     // Socket中断
    {
        // 处理各个Socket中断
        for(u8 i = 0; i < WCHNET_MAX_SOCKET_NUM; i++)
        {
            u8 socketint = WCHNET_GetSocketInt(i);
            if(socketint)
                WCHNET_HandleSockInt(i, socketint);
        }
    }
}
```

## 五、Socket编程

### 5.1 Socket创建
```c
// 创建TCP Socket
SOCKET_CFG socket_cfg;
socket_cfg.sock_type = SOCKET_TYPE_TCP;
socket_cfg.local_port = 80;  // 本地端口
socket_cfg.recv_buf_len = RECE_BUF_LEN;

u8 sock_id = WCHNET_SocketCreat(&socket_cfg);
if(sock_id == 0xFF)
    printf("Socket Creat Failed\n");
```

### 5.2 TCP客户端连接
```c
// 连接服务器
u8 dest_ip[4] = {192, 168, 1, 100};
u16 dest_port = 8080;

if(WCHNET_SocketConnect(sock_id, dest_ip, dest_port) == 0)
    printf("Connect Success\n");
else
    printf("Connect Failed\n");
```

### 5.3 TCP服务器监听
```c
// 开始监听
if(WCHNET_SocketListen(sock_id) == 0)
    printf("Listen Success\n");
else
    printf("Listen Failed\n");
```

### 5.4 数据发送
```c
// 发送数据
u8 send_buf[] = "Hello Server";
u16 send_len = sizeof(send_buf);

if(WCHNET_SocketSend(sock_id, send_buf, &send_len) == 0)
    printf("Send Success: %d bytes\n", send_len);
else
    printf("Send Failed\n");
```

### 5.5 数据接收
```c
// 接收数据
u8 recv_buf[RECE_BUF_LEN];
u16 recv_len = RECE_BUF_LEN;

if(WCHNET_SocketRecv(sock_id, recv_buf, &recv_len) == 0)
{
    printf("Received: %d bytes\n", recv_len);
    // 处理接收到的数据
}
```

## 六、网络协议示例

### 6.1 DHCP自动获取IP
```c
// 启动DHCP
WCHNET_DHCPStart();

// 设置主机名
WCHNET_DHCPSetHostname("WCHNET");

// DHCP回调函数
void DHCP_Callback(u8 status, u8* ip, u8* gateway, u8* mask, u8* dns)
{
    if(status == DHCP_SUCCESS)
    {
        printf("DHCP Success\n");
        printf("IP: %d.%d.%d.%d\n", ip[0], ip[1], ip[2], ip[3]);
        printf("Gateway: %d.%d.%d.%d\n", gateway[0], gateway[1], gateway[2], gateway[3]);
        printf("Mask: %d.%d.%d.%d\n", mask[0], mask[1], mask[2], mask[3]);
        printf("DNS: %d.%d.%d.%d\n", dns[0], dns[1], dns[2], dns[3]);
    }
    else
    {
        printf("DHCP Failed: %d\n", status);
    }
}
```

### 6.2 DNS域名解析
```c
// 初始化DNS
WCHNET_InitDNS();

// 域名解析
u8 hostname[] = "www.wch.cn";
u8 ip_addr[4];

if(WCHNET_HostNameGetIp(hostname, ip_addr, DNS_Callback) == 0)
    printf("DNS Query Sent\n");
else
    printf("DNS Query Failed\n");

// DNS回调函数
void DNS_Callback(u8* hostname, u8* ip, u8 status)
{
    if(status == DNS_SUCCESS)
    {
        printf("DNS Success: %s -> %d.%d.%d.%d\n",
               hostname, ip[0], ip[1], ip[2], ip[3]);
    }
    else
    {
        printf("DNS Failed for: %s\n", hostname);
    }
}
```

### 6.3 UDP通信
```c
// 创建UDP Socket
socket_cfg.sock_type = SOCKET_TYPE_UDP;
socket_cfg.local_port = 1234;

u8 udp_sock = WCHNET_SocketCreat(&socket_cfg);

// UDP发送数据
u8 dest_ip[4] = {192, 168, 1, 100};
u16 dest_port = 5678;
u8 udp_data[] = "UDP Test";

WCHNET_SocketUdpSendTo(udp_sock, udp_data, sizeof(udp_data),
                       dest_ip, dest_port);
```

## 七、高级功能示例

### 7.1 Web服务器
```c
// Web服务器配置
#define HTTP_SERVER_PORT 80

// 创建HTTP服务器Socket
socket_cfg.sock_type = SOCKET_TYPE_TCP;
socket_cfg.local_port = HTTP_SERVER_PORT;

u8 http_sock = WCHNET_SocketCreat(&socket_cfg);
WCHNET_SocketListen(http_sock);

// HTTP请求处理
void Handle_HTTP_Request(u8 sock_id, u8* request, u16 len)
{
    // 解析HTTP请求
    // 生成HTTP响应
    u8 response[] = "HTTP/1.1 200 OK\r\n"
                   "Content-Type: text/html\r\n"
                   "\r\n"
                   "<html><body><h1>Hello WCHNET</h1></body></html>";

    WCHNET_SocketSend(sock_id, response, sizeof(response));
}
```

### 7.2 MQTT客户端
```c
// MQTT连接参数
u8 mqtt_broker[] = "mqtt.wch.cn";
u8 mqtt_username[] = "user";
u8 mqtt_password[] = "pass";

// 连接MQTT服务器
MQTT_Connect(mqtt_broker, 1883, mqtt_username, mqtt_password);

// 订阅主题
MQTT_Subscribe("topic/test", QOS0);

// 发布消息
MQTT_Publish("topic/test", "Hello MQTT", QOS0);

// MQTT回调函数
void MQTT_Callback(u8* topic, u8* payload, u32 len)
{
    printf("MQTT Message: %s -> %.*s\n", topic, len, payload);
}
```

### 7.3 邮件发送
```c
// SMTP配置
SMTP_Config smtp_cfg;
smtp_cfg.server = "smtp.qq.com";
smtp_cfg.port = 465;
smtp_cfg.username = "user@qq.com";
smtp_cfg.password = "password";
smtp_cfg.ssl_enable = 1;

// 邮件内容
MAIL_Content mail;
mail.from = "user@qq.com";
mail.to = "receiver@example.com";
mail.subject = "Test Email";
mail.body = "This is a test email from WCHNET";

// 发送邮件
SMTP_SendMail(&smtp_cfg, &mail);
```

## 八、串口网络透传

### 8.1 单串口透传
```c
// UART与TCP Socket映射
void UART_TCP_Transparent(u8 uart_id, u8 sock_id)
{
    // UART接收 → TCP发送
    if(UART_ReceiveReady(uart_id))
    {
        u8 uart_buf[256];
        u16 uart_len = UART_Receive(uart_id, uart_buf, sizeof(uart_buf));
        WCHNET_SocketSend(sock_id, uart_buf, &uart_len);
    }

    // TCP接收 → UART发送
    u8 tcp_buf[256];
    u16 tcp_len = sizeof(tcp_buf);
    if(WCHNET_SocketRecv(sock_id, tcp_buf, &tcp_len) == 0)
    {
        UART_Send(uart_id, tcp_buf, tcp_len);
    }
}
```

### 8.2 8串口服务器
```c
// 8个串口映射到8个Socket
#define UART_NUM 8
#define SOCKET_NUM 8

u8 uart_socket_map[UART_NUM] = {0, 1, 2, 3, 4, 5, 6, 7};

void MultiUART_Server(void)
{
    for(u8 i = 0; i < UART_NUM; i++)
    {
        UART_TCP_Transparent(i, uart_socket_map[i]);
    }
}
```

## 九、以太网IAP升级

### 9.1 IAP服务器端
```c
// 接收固件数据并编程
void ETH_IAP_Server(void)
{
    u8 firmware_buf[1024];
    u32 write_addr = APP_START_ADDRESS;

    while(1)
    {
        u16 recv_len = sizeof(firmware_buf);
        if(WCHNET_SocketRecv(iap_sock, firmware_buf, &recv_len) == 0)
        {
            // 擦除FLASH页
            FLASH_ErasePage(write_addr);

            // 编程FLASH
            FLASH_Program(write_addr, firmware_buf, recv_len);

            write_addr += recv_len;

            // 发送确认
            u8 ack[] = "ACK";
            WCHNET_SocketSend(iap_sock, ack, sizeof(ack));
        }
    }
}
```

### 9.2 IAP客户端
```c
// 发送固件数据
void ETH_IAP_Client(u8* firmware, u32 size)
{
    u32 offset = 0;
    u8 ack_buf[4];

    while(offset < size)
    {
        u16 send_len = (size - offset) > 1024 ? 1024 : (size - offset);

        // 发送数据块
        WCHNET_SocketSend(iap_sock, &firmware[offset], &send_len);

        // 等待确认
        u16 ack_len = sizeof(ack_buf);
        WCHNET_SocketRecv(iap_sock, ack_buf, &ack_len);

        offset += send_len;
    }
}
```

## 十、调试与优化

### 10.1 常见问题排查
1. **网络连接失败**
   - 检查PHY芯片供电和复位
   - 验证RMII接口连接
   - 检查MAC地址获取

2. **数据传输异常**
   - 检查Socket状态
   - 验证缓冲区大小
   - 检查网络中断处理

3. **性能问题**
   - 优化网络任务调度
   - 调整缓冲区大小
   - 使用DMA传输

### 10.2 性能优化建议
1. **缓冲区优化**
   ```c
   // 根据应用调整缓冲区大小
   #define RECE_BUF_LEN 2048  // TCP接收缓冲区
   #define UDP_RECE_BUF_LEN 1472  // UDP接收缓冲区（MTU大小）
   ```

2. **任务调度优化**
   ```c
   // 合理分配网络任务处理时间
   while(1)
   {
       WCHNET_MainTask();  // 网络任务
       vTaskDelay(1);      // 让出CPU时间
   }
   ```

3. **中断优化**
   ```c
   // 减少中断处理时间
   void TIM2_IRQHandler(void) __attribute__((interrupt("WCH-Interrupt-fast")));
   ```

## 十一、API函数参考

### 11.1 初始化函数
- `WCHNET_GetMacAddr()` - 获取MAC地址
- `ETH_LibInit()` - 以太网库初始化
- `TIM2_Init()` - 网络定时器初始化

### 11.2 Socket函数
- `WCHNET_SocketCreat()` - 创建Socket
- `WCHNET_SocketConnect()` - TCP连接
- `WCHNET_SocketListen()` - TCP监听
- `WCHNET_SocketSend()` - 发送数据
- `WCHNET_SocketRecv()` - 接收数据
- `WCHNET_SocketClose()` - 关闭Socket

### 11.3 网络服务函数
- `WCHNET_DHCPStart()` - 启动DHCP
- `WCHNET_InitDNS()` - 初始化DNS
- `WCHNET_HostNameGetIp()` - 域名解析

### 11.4 中断处理函数
- `WCHNET_GetGlobalInt()` - 获取全局中断状态
- `WCHNET_GetSocketInt()` - 获取Socket中断状态
- `WCHNET_HandleGlobalInt()` - 处理全局中断

## 总结

CH32V307的以太网外设功能强大，配合WCHNET协议栈可以轻松实现各种网络应用。关键配置要点包括PHY初始化、网络参数配置、Socket编程和中断处理。建议开发者根据具体应用场景选择合适的示例工程作为起点，逐步完善网络功能。
# WCHNET Protocol Stack Library Application Note

Version: 1D

http://wch.cn

# 1. Overview

With the popularity of the Internet of Things, more and more MCU systems require network communication.

The WCHNET devices with on-chip Ethernet MAC(some chips have built-in 10M PHY) support 10M Ethernet (some chips also support 100M/1000M speed), full-duplex, half-duplex, automatic negotiation, line automatic conversion and other functions, and can directly interact with network terminals such as PCs and embedded devices.

The WCHNET protocol stack library provides a TCP/IP subroutine library that integrates Ethernet protocol stacks such as TCP, UDP, ARP, RARP, ICMP, and IGMP. It can support TCP, UDP and IPRAW modes.

# 2. Parameter description

## 2.1 Configuration

Call WCHNET_ConfigLIB for library configuration. Parameters are as follows:

<table><tr><td>Name</td><td>Macro definition</td><td>Bit definition</td><td>Description</td></tr><tr><td>TxBufSize</td><td>Reserved</td><td>0-31</td><td>Size of MAC transmit buffer</td></tr><tr><td>TCPMss</td><td>WCHNET.tcp_MSS</td><td>0-31</td><td>Size of TCP MSS</td></tr><tr><td>HeapSize</td><td>WCHNET_MEM_heap_SIZE</td><td>0-31</td><td>Size of heap memory</td></tr><tr><td>ARPTableNum</td><td>WCHNET_NUM_ARP_TABLE</td><td>0-31</td><td>Number of ARP tables</td></tr><tr><td rowspan="4">MiscConfig0</td><td>CFG0.tcp_SEND.Copy</td><td>0</td><td>TCP transmit buffer copy
1: Copy. 0: Non-copy.</td></tr><tr><td>CFG0.tcp_RECV.Copy</td><td>1</td><td>TCP receive copy optimization, for internal debug</td></tr><tr><td>CFG0.tcp_old_DELETE</td><td>2</td><td>Delete old TCP connection
1: Enabled. 0: Disabled.</td></tr><tr><td>CFG0_IP_REASS_PBUFS</td><td>3-7</td><td>Number of reassembled IP packets</td></tr><tr><td rowspan="2"></td><td>CFG0_TCP_DEALY_ACK_DISABLE</td><td>8</td><td>TCP delayed acknowledgement
1: Disabled. 0: Enabled.</td></tr><tr><td>Reserved</td><td>9-31</td><td>-</td></tr><tr><td rowspan="9">MiscConfig1</td><td>WCHNET_MAX_SOCKET_NUM</td><td>0-7</td><td>Number of sockets</td></tr><tr><td>Reserved</td><td>8-12</td><td>-</td></tr><tr><td>WCHNET_PING_ENABLE</td><td>13</td><td>PING enable
1: Enabled. 0: Disabled.</td></tr><tr><td>TCP_RETRY_COUNT</td><td>14-18</td><td>TCP retry count</td></tr><tr><td>TCP_RETRY_PERIOD</td><td>19-23</td><td>TCP retry period
The unit is 50 milliseconds.</td></tr><tr><td>Reserved</td><td>24</td><td>-</td></tr><tr><td>SOCKET_SEND_RETRY</td><td>25</td><td>Failure and retry sending
1: Enabled. 0: Disabled.</td></tr><tr><td>HARDWARE_CHECKSUM_CONFIG</td><td>26</td><td>Hardware checksum checking and insertion configuration,
1: Enabled. 0: Disabled.</td></tr><tr><td>FINE DHCP_PERIOD</td><td>27-31</td><td>DHCP Fine Query Period</td></tr><tr><td>led_link</td><td>led_callback</td><td>-</td><td>PHY Link status indicator</td></tr><tr><td>led_data</td><td>led_callback</td><td>-</td><td>Ethernet communication indicator</td></tr><tr><td>net_send</td><td>eth_tx_set</td><td></td><td>Ethernet Tx configuration</td></tr><tr><td>net_send</td><td>eth_rx_set</td><td></td><td>Ethernet Rx configuration</td></tr><tr><td>CheckValid</td><td>WCHNET_CFG_VALID</td><td>0-31</td><td>Configuration value valid flag. A fixed value.</td></tr></table>

## 2.2 SocketInf

SocketInf is a socket information list, defined as follows:

SOCK_INF SocketInf[WCHNET_MAX_SOCKET_NUM]

The address is aligned with 4 bytes. WCHNET_MAX_SOCKET_NUM is the number of sockets. SocketInf stores the information of each socket. Please refer to the definition of SOCK_INF for its

information members. This variable is read and written inside the library. If it is not necessary, please do not write to it in the application program (refers to the user program that calls the library function, which is called the application program in this manual, the same below).

## 2.3 Memp_Memory

The pool allocated memory used internally by WCHNET is mainly used for data reception buffer. For calculation of its size, please refer to the macro definition of WCHNET_MEMP_SIZE in wchnet.h.

## 2.4 Mem_Heap_Memory

The heap-allocated memory used internally by WCHNET is mainly used for data transmission buffer. For calculation of its size, please refer to the macro definition of WCHNET_RAM_HEAP_SIZE in wchnet.h.

## 2.5 Mem_ArpTable

ARP buffer table is used to record the correspondence between IP addresses and MAC addresses. The size of the ARP buffer table can be configured.

## 2.6 MemNum, MemSize

MemNum and MemSize are arrays generated according to user configuration. WCHNET uses these two arrays to manage memory allocation, and they cannot be modified.

# 3. Subroutines

## 3.1 General table of library subroutines

<table><tr><td>Classification</td><td>Function name</td><td>General description</td></tr><tr><td rowspan="9">Basic functions</td><td>WCHNET_Init</td><td>Library initialization</td></tr><tr><td>WCHNET_GetVer</td><td>Get library version</td></tr><tr><td>WCHNET_NetInput</td><td>Ethernet data input</td></tr><tr><td>WCHNET_PeriodicHandle</td><td>Handle periodic tasks</td></tr><tr><td>WCHNET_ETHIsr</td><td>Ethernet interrupt service function</td></tr><tr><td>WCHNET_GetPHYStatus</td><td>Get PHY status</td></tr><tr><td>WCHNET 查询GlobalInt</td><td>Query global interrupt</td></tr><tr><td>WCHNET_GetGlobalInt</td><td>Read and clear global interrupt</td></tr><tr><td>WCHNET_Aton</td><td>ASCII address to network address</td></tr><tr><td rowspan="3"></td><td>WCHNET_Ntoa</td><td>Network address to ASCII address</td></tr><tr><td>WCHNET_ConfigLIB</td><td>Library parameter configuration</td></tr><tr><td>WCH_GetMacAddr</td><td>Get MAC address</td></tr><tr><td rowspan="13">socket functions</td><td>WCHNET_GetSocketInt</td><td>Get socket interrupt and clear</td></tr><tr><td>WCHNET_SocketCreat</td><td>Create socket</td></tr><tr><td>WCHNET_SocketClose</td><td>Close socket</td></tr><tr><td>WCHNET_SocketRecvLen</td><td>Get socket reception length</td></tr><tr><td>WCHNET_SocketRecv</td><td>socket receive data</td></tr><tr><td>WCHNET_SocketSend</td><td>socket send data</td></tr><tr><td>WCHNET_SocketListen</td><td>TCP listen</td></tr><tr><td>WCHNET_SocketConnect</td><td>TCP connect</td></tr><tr><td>WCHNETMODIFYRecvBuf</td><td>Modify reception buffer</td></tr><tr><td>WCHNET_SocketUdpSendTo</td><td>UDP send, specify the target IP and target port</td></tr><tr><td>WCHNET 查询Unack</td><td>Query unsuccessfully sent packets</td></tr><tr><td>WCHNET_SetSocketTTL</td><td>Set TTL of socket</td></tr><tr><td>WCHNET_RetrySendUnack</td><td>Start TCP retry sending immediately</td></tr><tr><td rowspan="3">DHCP functions</td><td>WCHNET DHCPStart</td><td>DHCP start</td></tr><tr><td>WCHNET DHCPStop</td><td>DHCP stop</td></tr><tr><td>WCHNET DHCPSetHostname</td><td>Configure DHCP host name</td></tr><tr><td rowspan="3">DNS functions</td><td>WCHNET_InitDNS</td><td>Initialize DNS</td></tr><tr><td>WCHNET_DNSStop</td><td>DNS stop</td></tr><tr><td>WCHNET域名GetIp</td><td>Get IP address base on host name</td></tr><tr><td rowspan="2">KEEPLIVE functions</td><td>WCHNET_ConfigKeepLive</td><td>Configure library KEEP LIVE parameter</td></tr><tr><td>WCHNET_SocketSetKeepLive</td><td>Configure socketKEEPLIVE enable</td></tr></table>

Interrupt:

The global interrupt and socket interrupt of the library are actually just a sign of the variable, not

the hardware interrupt generated by the WCHNET devices.

## 3.2 WCHNET_Init

<table><tr><td>Prototype</td><td>uint8_t WCHNET_Init(const uint8_t *ip, const uint8_t *gwip, const uint8_t *mask, const uint8_t *macaddr)</td></tr><tr><td>Input</td><td>Ip        - IP address pointer
Gwip        - Gateway address pointer
Mask        - Subnet mask pointer
Macaddr - MAC address pointer</td></tr><tr><td>Output</td><td>None</td></tr><tr><td>Return</td><td>0 - Success. Others - Error. Please refer to wchnet.h for specific error codes.</td></tr><tr><td>Function</td><td>Library initialization.</td></tr></table>

Subnet mask pointer can be set to NULL. If it is set to NULL, the library selects 255.255.255.0 as subnet mask.

## 3.3 WCHNET_GetVer

<table><tr><td>Prototype</td><td>uint8_t WCHNET_GetVer(void)</td></tr><tr><td>Input</td><td>None</td></tr><tr><td>Output</td><td>None</td></tr><tr><td>Return</td><td>Library version</td></tr><tr><td>Function</td><td>To get library version.</td></tr></table>

## 3.4 WCHNET_NetInput

<table><tr><td>Prototype</td><td>void WCHNET_NealInput(void)</td></tr><tr><td>Input</td><td>None</td></tr><tr><td>Output</td><td>None</td></tr><tr><td>Return</td><td>None</td></tr><tr><td>Function</td><td>Ethernet data input. Always called in the main program, or called after the reception interrupt is detected.</td></tr></table>

## 3.5 WCHNET_PeriodicHandle

<table><tr><td>Prototype</td><td>void WCHNET_PeriodicHandle(void)</td></tr><tr><td>Input</td><td>None</td></tr><tr><td>Output</td><td>None</td></tr><tr><td>Return</td><td>None</td></tr><tr><td>Function</td><td>Handle periodic tasks in the protocol stack.</td></tr></table>

## 3.6 WCHNET_ETHIsr

<table><tr><td>Prototype</td><td>void WCHNET_ETHIsr(void)</td></tr><tr><td>Input</td><td>None</td></tr><tr><td>Output</td><td>None</td></tr><tr><td>Return</td><td>None</td></tr><tr><td>Function</td><td>Ethernet interrupt service function. Called after Ethernet interrupt is generated.</td></tr></table>

## 3.7 WCHNET_GetPHYStatus

<table><tr><td>Prototype</td><td>uint8_t WCHNET_GetPHYStatus(void)</td></tr><tr><td>Input</td><td>None</td></tr><tr><td>Output</td><td>None</td></tr><tr><td>Return</td><td>PHY status</td></tr><tr><td>Function</td><td>Get current status of PHY. Main status:0x09 – PHY disconnects0x2D – PHY establishes connection and negotiation is completed.</td></tr></table>

## 3.8 WCHNET_QueryGlobalInt

<table><tr><td>Prototype</td><td>uint8_t WCHNET 查询 GlobalInt(void)</td></tr><tr><td>Input</td><td>None</td></tr><tr><td>Output</td><td>None</td></tr><tr><td>Return</td><td>Global interrupt status</td></tr><tr><td>Function</td><td>Query global interrupt status. For specific status codes, please refer to wchnet.h.</td></tr></table>

## 3.9 WCHNET_GetGlobalInt

<table><tr><td>Prototype</td><td>uint8_t WCHNET_GetGlobalInt(void)</td></tr><tr><td>Input</td><td>None</td></tr><tr><td>Output</td><td>None</td></tr><tr><td>Return</td><td>Global interrupt status</td></tr><tr><td>Function</td><td>Read global interrupt and clear it. For specific status codes, please refer to wchnet.h.</td></tr></table>

## 3.10 WCHNET_Aton

<table><tr><td>Prototype</td><td>uint8_t WCHNET_Aton(const char *cp, uint8_t *addr)</td></tr><tr><td>Input</td><td>*cp - ASCII address to be converted, such as “192.168.1.2”*addr - First address of the memory stored in the converted network address</td></tr><tr><td>Output</td><td>*addr - Converted network address, such as 0xC0A80102</td></tr><tr><td>Return</td><td>0 - Success. Others - Failure.</td></tr><tr><td>Function</td><td>Convert ASCII address to network address.</td></tr></table>

## 3.11 WCHNET_Ntoa

<table><tr><td>Prototype</td><td>uint8_t *WCHNET_Ntoa( uint8_t *ipaddr)</td></tr><tr><td>Input</td><td>*ipaddr – socketID value</td></tr><tr><td>Output</td><td>None</td></tr><tr><td>Return</td><td>Converted ASCII address</td></tr><tr><td>Function</td><td>Convert network address to ASCII address.</td></tr></table>

## 3.12 WCHNET_ConfigLIB

<table><tr><td>Prototype</td><td>uint8_t WCHNET_ConfigLIB(struct_WCH_CFG *cfg)</td></tr><tr><td>Input</td><td>cfg –Configuration parameter</td></tr><tr><td>Output</td><td>None</td></tr><tr><td>Return</td><td>0 – Success. Others – Failure.</td></tr><tr><td>Function</td><td>Library parameter configuration.</td></tr></table>

## 3.13 WCHNET_GetMacAddr

<table><tr><td>Prototype</td><td>void WCH_GetMac uint8_t *macaddr)</td></tr><tr><td>Input</td><td>*macaddr – Memory address</td></tr><tr><td>Output</td><td>mac address</td></tr><tr><td>Return</td><td>None</td></tr><tr><td>Function</td><td>Get MAC address.</td></tr></table>

## 3.14 WCHNET_GetSocketInt

<table><tr><td>Prototype</td><td>uint8_t WCHNET_Get SOCKETInt uint8_t socketid)</td></tr><tr><td>Input</td><td>socketid – socketID value</td></tr><tr><td>Output</td><td>None</td></tr><tr><td>Return</td><td>Return socket interrupt. For specific status codes, please refer to wchnet.h.</td></tr><tr><td>Function</td><td>Get socket interrupt, and clear socket interrupt.</td></tr></table>

## 3.15 WCHNET_SocketCreat

<table><tr><td>Prototype</td><td>uint8_t WCHNET_SocketCreat( uint8_t *socketid,SOCKINF *socinf)</td></tr><tr><td>Input</td><td>*socketid – Memory address
socinf – Create socket configuration parameter</td></tr><tr><td>Output</td><td>*socketid – socketID value</td></tr><tr><td>Return</td><td>Execution status. For specific status codes, please refer to wchnet.h.</td></tr><tr><td>Function</td><td>Create socket.</td></tr></table>

socketinf is only passed as a variable. WCHNET_SocketCreat analyzes the list information. If the information is valid, it will find a free list (n) from SocketInf[WCHNET_MAX_SOCKET_NUM],

copy socketinf into SocketInf[n], lock SocketInf[n] and create corresponding UDP/TCP/IPRAW connection. If the creation is successful, write n to socketed, and a success code is returned.

When creating UDP, TCP client or IPRAW, the receive buffer and the size of it should be allocated before creation. The allocation method of the TCP server is different. The WCHNET_ModifyRecvBuf function should be called to allocate the receive buffer after receiving the successful connection interrupt.

For details, please refer to the related routines.

## 3.16 WCHNET_SocketClose

<table><tr><td>Prototype</td><td>uint8_t WCHNET_SocketClose( uint8_t socketid, uint8_t mode)</td></tr><tr><td>Input</td><td>socketid – socketID value
mode – socket is a TCP connection, and the parameter is closed.</td></tr><tr><td>Output</td><td>None</td></tr><tr><td>Return</td><td>Execution status. For specific status codes, please refer to wchnet.h.</td></tr><tr><td>Function</td><td>Close socket.</td></tr></table>

In UDP and IPRAW modes, “mode” is invalid. Call this function to close socket immediately.

In TCP mode, “mode” can be:

TCP_CLOSE_NORMAL: Normal. Close after 4 handshakes. The closing speed is slower.

TCP_CLOSE_RST: Reset connection. WCHNET sends RST to the target for reset. The closing speed is faster.

TCP_CLOSE_ABANDON: Abandon directly. No information is sent to the target. Close socket. The closing speed is the fastest.

Call this function, and it may generally take a certain period of time to close. This is mainly because the library needs a certain period of time to terminate the TCP connection. As long as the SINT_STAT_TIM_OUT or SINT_STAT_DISCONNECT interrupt is generated, the socket must be in the closed state.

## 3.17 WCHNET_SocketRecvLen

<table><tr><td>Prototype</td><td>uint32_t WCHNET_SocketRecvLen( uint8_t socketid, uint32_t *bufaddr)</td></tr><tr><td>Input</td><td>socketid – socketID value
*bufaddr – Memory address</td></tr><tr><td>Output</td><td>*bufaddr – First address of socket data</td></tr><tr><td>Return</td><td>Length of the received data</td></tr><tr><td>Function</td><td>Get the length of the data received by socket.</td></tr></table>

This function is mainly used to get the length of the data received by socket and the address of the receive buffer. The application program can directly use the address output by this function, and can use the data in the internal receive buffer without copying. This can save RAM to a certain extent. If bufaddr is set to NULL, this function only returns the length of the data received by socket.

## 3.18 WCHNET_SocketRecv

<table><tr><td>Prototype</td><td>uint8_t WCHNET_SocketRecv( uint8_t socketid, uint8_t *buf, uint32_t *len)</td></tr><tr><td>Input</td><td>socketid – socketID value
*buf – First address of the received data
*len - Memory address and length of data expected to be read</td></tr><tr><td>Output</td><td>*buf - The read data content that is written
*len - The actual read length of read</td></tr><tr><td>Return</td><td>Execution status. For specific status codes, please refer to wchnet.h.</td></tr><tr><td>Function</td><td>Socket receives data.</td></tr></table>

This function copies the data in the socket receive buffer into buf, and the actual length of the copied data will be written into len.

WCHNET supports two modes to receive data. One is the interrupt mode, and the other is the callback mode.

Interrupt mode: After WCHNET receives data, an interrupt is generated. User can read the received data through the WCHNET_SocketRecvLen and WCHNET_SocketRecv functions. IPRAW, UDP and TCP all can receive data in interrupt mode. If buf is not NULL, WCHNET_SocketRecv copies the data of the internal buffer to buf. If buf is NULL, the application layer reads the data in a non-copying way. The function is called only for the pointer offset, and *len is equal to the length of remaining data.

Callback mode: Only valid in UDP mode. WCHNET notifies the application layer to receive data by calling back the AppCallBack function in the SocketInf structure after receiving the data. AppCallBack is implemented by the application layer, and the application layer must read all data in this function. Otherwise WCHNET will forcibly clear it. If the callback mode is not needed, AppCallBack must be cleared to 0 when creating socket. The prototype of the callback function is as follows:

<table><tr><td>Prototype</td><td>void(*AppCallBack)(structSCOKINF*socinf, uint32tipaddr, uint16_tport, uint8_t *buf, uint32_tlen)</td></tr><tr><td>Input</td><td>Socinf – Through this parameter, WCHNET transfers the socket information list to the application layer, and the application layer can know the socket information. ipaddr - Source IP address of the message port - Source port of the message buf - Buffer address len - Data length</td></tr><tr><td>Output</td><td>None</td></tr><tr><td>Return</td><td>None</td></tr><tr><td>Function</td><td>Receive callback function in UDP mode.</td></tr></table>

## 3.19 WCHNET_SocketSend

<table><tr><td>Prototype</td><td>uint8_t WCHNET_SocketSend( uint8_t socketid, uint8_t *buf, uint32_t *len)</td></tr><tr><td>Input</td><td>socketid – socketID value
*buf – First address of the sent data
*len - Memory address and length of data expected to be sent</td></tr><tr><td>Output</td><td>*len – Length of the data that is sent actually</td></tr><tr><td>Return</td><td>Execution status. For specific status codes, please refer to wchnet.h.</td></tr><tr><td>Function</td><td>Socket sends data.</td></tr></table>

This function copies the data from buf into the internal stack send buffer, sends the data, and outputs the actual length of the data sent via len. If too much data is sent, this function returns 0 (success) does not mean that all the data will be sent, so the application layer needs to check len in the actual processing in order to determine the actual length of the data sent.

## 3.20 WCHNET_SocketUdpSendTo

<table><tr><td>Prototype</td><td>uint8_t WCHNET_SocketUdpSendTo( uint8_t socketid, uint8_t *buf, uint32_t *slen, uint8_t *sip, uint16_t port)</td></tr><tr><td>Input</td><td>socketid – socketID value
*buf – Address of the sent data
*slen – Address of the sent length</td></tr><tr><td></td><td>*sip – Target IP address
port – Target port number</td></tr><tr><td>Output</td><td>*slen – Length sent actually</td></tr><tr><td>Return</td><td>Execution status. For specific status codes, please refer to wchnet.h.</td></tr><tr><td>Function</td><td>UDP send, specify the target IP and target port</td></tr></table>

In UDP mode, the difference between WCHNET_SocketSend and WCHNET_SocketUdpSendTo is that the former can only send data to the target IP and port specified when the socket is created, while the latter can send data to any IP and port. WCHNET_SocketUdpSendTo is generally used in UDP server mode.

## 3.21 WCHNET_SocketListen

<table><tr><td>Prototype</td><td>uint8_t WCHNET_SocketListen( uint8_t socketid)</td></tr><tr><td>Input</td><td>socketid – socketID value</td></tr><tr><td>Output</td><td>None</td></tr><tr><td>Return</td><td>Execution status. For specific status codes, please refer to wchnet.h.</td></tr><tr><td>Function</td><td>TCP listen. Used in TCPSERVER mode.</td></tr></table>

If the application layer needs to establish a TCP SERVER, first use WCHNET_SocketCreat to create a TCP, and then call this function to make TCP enter the Listen mode. TCP in Listen mode does not receive or send data, but only listens for TCP connections. Once a client connects to this server, the library will automatically allocate a socket and generate the SINT_STAT_CONNECT connection interrupt. So the listened TCP does not need to allocate receive buffer.

## 3.22 WCHNET_SocketConnect

<table><tr><td>Prototype</td><td>uint8_t WCHNET_SocketConnect( uint8_t socketid)</td></tr><tr><td>Input</td><td>socketid – socketID value</td></tr><tr><td>Output</td><td>None</td></tr><tr><td>Return</td><td>Execution status. For specific status codes, please refer to wchnet.h.</td></tr><tr><td>Function</td><td>TCP connect. Used in TCP Client mode.</td></tr></table>

If the application layer needs to establish a TCP Client, first use WCHNET_SocketCreat to create a TCP, and then call this function to connect. After successful connection, the

SINT_STAT_CONNECT connection interrupt is generated. If the remote end is not online or the port is not open, the library will automatically retry a certain number of times, and if it is still

unsuccessful, the SINT_STAT_TIM_OUT timeout interrupt will be generated.

## 3.23 WCHNET_ModifyRecvBuf

<table><tr><td>Prototype</td><td>void WCHNET_DecyRecvBuf( uint8_t socketid, uint32_t bufaddr, uint32_t bufsize)</td></tr><tr><td>Input</td><td>socketid – socketID value
bufaddr – Address of the receive buffer
bufsize – Size of the receive buffer</td></tr><tr><td>Output</td><td>None</td></tr><tr><td>Return</td><td>None</td></tr><tr><td>Function</td><td>Modify socket receive buffer.</td></tr></table>

In order to make the application layer process data conveniently and flexibly, the library allows to dynamically modify the address and size of the socket receive buffer. Before the receive buffer is modified, it is better to call WCHNET_SocketRecvLen to check whether there is any remaining data in the buffer. Once WCHNET_ModifyRecvBuf is called, the original buffer data will be cleared. In TCP mode, if the connection has been established, call WCHNET_ModifyRecvBuf, and the library will notify the current window size to the remote end.

## 3.24 WCHNET_SetSocketTTL

<table><tr><td>Prototype</td><td>uint8_t WCHNET_SetSocketTTL( uint8_t socketid, uint8_t ttl)</td></tr><tr><td>Input</td><td>socketid – socketID value
ttl – TTL value</td></tr><tr><td>Output</td><td>None</td></tr><tr><td>Return</td><td>Execution status. For specific status codes, please refer to wchnet.h.</td></tr><tr><td>Function</td><td>Set socket TTL.</td></tr></table>

Note: TTL cannot be 0, and it default to 128.

## 3.25 WCHNET_QueryUnack

<table><tr><td>Prototype</td><td>uint8_t WCHNET 查询 Unack( uint8_t socketid, uint32_t *addrlist, uint16_t lsl en)</td></tr><tr><td>Input</td><td>socketid – socketID value</td></tr><tr><td></td><td>*addlist – First address of the stored memory
islen – Length of the stored memory</td></tr><tr><td>Output</td><td>*addlist – Address list of the data packets that are not sent successfully</td></tr><tr><td>Return</td><td>Number of unsent and unacknowledged segments</td></tr><tr><td>Function</td><td>Query the packets that are not sent successfully.</td></tr></table>

Unack Segment: TCP message that is not sent successfully.

WCHNET_QueryUnack is used to query for the number of messages that are not sent successfully by socket and the address of the messages. Two methods:

(1) Query for the number of “Unack Segment”. The addrlist is NULL. WCHNET_QueryUnack returns the number of “socket Unack Segment”.   
(2) Query for the Unack Segment information. WCHNET_QueryUnack writes the address of these data messages to addrlist.

The application layer can also query whether there is an Unack Segment by looping WCHNET_QueryUnack(sockinf,NULL,0). If it queries for an Unack Segment, call WCHNET_QueryUnack again to get the information:

```txt
while(1) { if(WCHNET 查询Unack(sockinf, NULL, 0)) { WCHNET 查询Unack(sockinf, addrlist, sizeof(addrlist)); } /*Other tasks*/ } 
```

## 3.26 WCHNET_RetrySendUnack

<table><tr><td>Prototype</td><td>void WCHNET_RetrySendUnack( uint8_t socketid)</td></tr><tr><td>Input</td><td>socketid – socketID value</td></tr><tr><td>Output</td><td>None</td></tr><tr><td>Return</td><td>None</td></tr><tr><td>Function</td><td>Start TCP retry sending immediately.</td></tr></table>

WCHNET_RetrySendUnack is valid only in TCP mode, used to retry sending the messages that are not sent successfully. The application program can check whether the socket data has been successfully sent through WCHNET_QueryUnack. If necessary, call WCHNET_RetrySendUnack to retry sending the data messages immediately. Normally, the application layer is not required to re-send, and WCHNET will retry automatically.

## 3.27 WCHNET_DHCPStart

<table><tr><td>Prototype</td><td>uint8_t WCHNET DHCPStart( dhcp_callback dhcp)</td></tr><tr><td>Input</td><td>dhcp – Application layer callback function</td></tr><tr><td>Output</td><td>None</td></tr><tr><td>Return</td><td>0 – Success. Others – Failure.</td></tr><tr><td>Function</td><td>Start DHCP.</td></tr></table>

When DHCP succeeds or fails, the library calls the dhcp_callback function, to notify the application layer of the DHCP status. WCHNET passes two parameters to this function, the one is DHCP status (0-Suceed. Others-Fail). When DHCP succeeds, user can get a pointer through the other parameter, and the address pointed to by the pointer stores the IP address, gateway address, subnet mask, primary DNS and secondary DNS in turn, a total of 20 bytes. Note that the pointer is invalid after returned as the dhcp_callback temporary variable.

If there is no DHCP server in the current network, a timeout of about 10 seconds will occur. After the timeout, call the dhcp_callback function to notify the application layer. In this case, DHCP will not stop and will always search for the DHCP Server. User can call WCHNET_DHCPStop to stop DHCP.

### Note:

(1) Start DHCP after WCHNET_Init succeeds (necessary).   
(2) Create socket after DHCP succeeds (recommended).   
(3) DHCP function occupies a UDP socket.

If DHCP fails, communicate via the IP address used in WCHNET_Init.

## 3.28 WCHNET_DHCPStop

<table><tr><td>Prototype</td><td>uint8_t WCHNET DHCPStop(void)</td></tr><tr><td>Input</td><td>None</td></tr><tr><td>Output</td><td>None</td></tr><tr><td>Return</td><td>0 – Success. Others – Failure.</td></tr><tr><td>Function</td><td>Stop DHCP.</td></tr></table>

## 3.29 WCHNET_DHCPSetHostname

<table><tr><td>Prototype</td><td>uint8_t WCHNET_DHCPSetHostname(char *name)</td></tr><tr><td>Input</td><td>*name – First address of DHCP host name</td></tr><tr><td>Output</td><td>None</td></tr><tr><td>Return</td><td>0 – Success. Others – Failure.</td></tr><tr><td>Function</td><td>Configure DHCP host name.</td></tr></table>

## 3.30 WCHNET_InitDNS

<table><tr><td>Prototype</td><td>void WCHNET_InitDNS( uint8_t *dnsip, uint16_t port )</td></tr><tr><td>Input</td><td>*dnsip – dns server IP address
port – dns server port number</td></tr><tr><td>Output</td><td>None</td></tr><tr><td>Return</td><td>None</td></tr><tr><td>Function</td><td>Initialize DNS.</td></tr></table>

If DNS is used, a UDP socket is occupied. Call this function after WCHNET_Init, to start DNS.

## 3.31 WCHNET_DNSStop

<table><tr><td>Prototype</td><td>void WCHNET_DNSStop (void)</td></tr><tr><td>Input</td><td>None</td></tr><tr><td>Output</td><td>None</td></tr><tr><td>Return</td><td>None</td></tr><tr><td>Function</td><td>Stop DNS.</td></tr></table>

After this function is executed, the occupancy of the UDP socket will be released. If it is required to use DNS, call WCHNET_InitDNS again.

## 3.32 WCHNET_HostNameGetIp

<table><tr><td>Prototype</td><td>uint8_t WCHNET_HostNameGetIp( const char *hostname, uint8_t *addr, dns_cal11back found, void *arg )</td></tr><tr><td>Input</td><td>hostname - Host domain name
*addr - First address of the memory that stores the parsed ip
found - Callback function
arg - found parameter</td></tr><tr><td>Output</td><td>addr: Output the host IP address. Only valid when the function returns 0. Must be 4-byte aligned.</td></tr><tr><td>Return</td><td>Execution status. For specific status codes, please refer to wchnet.h.</td></tr><tr><td>Function</td><td>Get host IP address.</td></tr></table>

found: Function pointer of the application layer. Its basic prototype is as follows: typedef void (*dns_callback)(const char *name, uint8_t *ipaddr, void*callback_arg); This function is used to get the IP address of the host. “hostname” represents host name. “addr” represents IP address pointer. If the IP address corresponding to hostname is already in DNS buffer, it returns a success code directly, and outputs the IP address corresponding to the host to addr. If the IP address is not in the buffer, the DNS module initiates the DNS transaction, and asks the DNS server. After failure or success, it calls the found function, and the arg failure parameter is NULL.

arg is a parameter required by found, and it can be NULL. But found cannot be NULL.

Note:

(1) addr must be 4-byte aligned.   
(2) The maximum length of the hostname string cannot be greater than 63.

## 3.33 WCHNET_ConfigKeepLive

<table><tr><td>Prototype</td><td>void WCHNET_ConfigKeepLive(struct KEEP_CFG *cfg)</td></tr><tr><td>Input</td><td>*cfg - KEEPLIVE configuration parameter</td></tr><tr><td>Output</td><td>None</td></tr><tr><td>Return</td><td>None</td></tr><tr><td>Function</td><td>Configure library KEEP LIVE parameter.</td></tr></table>

This function is used to configure the KEEPLIVE parameter in WCHNET. The struct_KEEP_CFG structure is defined in wchnet.h.

```c
struct _KEEP_CFG
{
    uint32_t KLIIdle;
    uint32_t KLIIntvl;
    uint32_t KLCount;
}; 
```

KLIdle: Idle time. The KEEPLIVE detection is started after the TCP connection is idle for a certain period of time. The unit is milliseconds. The default value is 20000. The time precision is the TCP retry interval.

KLIntvl: Interval. KEEPLIVE detection timeout interval. The unit is milliseconds. The default

value is 15000.

KLCount: Count. KEEPLIVE detection count. The default value is 9.

This setting is a global setting for the library, and is valid for TCP connections that enable KEEPLIVE. When there is no data transfer between the two parties within the KLIdle time, KEEPLIVE is started. If the other party has no response after KEEPLIVE sends KLCount times, the connection is considered invalid and it will be disconnected. This function should be called after the library is initialized. The time precision is the TCP retry period.

## 3.34 WCHNET_SocketSetKeepLive

<table><tr><td>Prototype</td><td>uint8_t WCHNET_SocketSetKeepLive( uint8_t socketid, uint8_t enable )</td></tr><tr><td>Input</td><td>socketid – socketID value
enable – 1: Enabled. 0: Disabled.</td></tr><tr><td>Output</td><td>None</td></tr><tr><td>Return</td><td>0 – Success. Others – Failure.</td></tr><tr><td>Function</td><td>Configure socket KEEP LIVE enable.</td></tr></table>

This function is used to enable or disable socket KEEPLIVE. If “enable” is 1, KEEPLIVE is enabled. If “enable” is 0, KEEPLIVE is disabled. After socket is created, KEEPLIVE defaults to be disabled. After the TCP client enables socket, call this function to enabled KEEPLIVE. For the TCP server, call this function to enable KEEPLIVE after SINT_STAT_CONNECT is generated. When KEEPLIVE is enabled, the parameter of KEEPLIVE is read. After that, the KEEPLIVE parameter change does not affect the KEEPLIVE parameter of the started socket.

# 4. Guidance

## 4.1 Initilization

WCHNET_Init is the library initialization function. For the usage of WCHNET_Init, please refer to Section 3.2. After WCHNET is initialized, the application layer needs to enable the Ethernet interrupt, and call the WCHNET_ETHIsr interrupt service function in the corresponding interrupt function. In addition, an external clock needs to be provided for the library function for periodic tasks such as refreshing the ARP list, TCP timeout, etc. Call the WCHNET_TimeIsr function to update time. The parameter passed by this function is the time difference of the last call, in milliseconds.

To sum up, after calling WCHNET_Init to initialize the library, the application layer needs to call ETH_Init to initialize the Ethernet physical layer.

## 4.2 Configuration

The configuration information is passed to the library through WCHNET_ConfigLIB. For detailed configuration information, please refer to Section 2.1. It must be called before WCHNET_Init.

This section mainly introduces the meaning and method of configuration such as IPRAW, UDP, TCP, and memory allocation.

(1) WCHNET_NUM_IPRAW

Used to configure the number of IPRAW (IP raw sockets) connections. The minimum value is 1. For IPRAW communication.

(2) WCHNET_NUM_UDP

Used to configure the number of UDP connections. The minimum value is 1. For UDP communication.

(3) WCHNET_NUM_TCP

Used to configure the number of TCP connections. The minimum value is 1. For TCP communication.

(4) WCHNET_NUM_TCP_LISTEN

Used to configure the number of TCP listeners. The minimum value is 1. The socket for TCP listening is only used for listening. Once a TCP connection is listened, a TCP connection will be allocated immediately, occupying the number of WCHNET_NUM_TCP.

(5) WCHNET_MAX_SOCKET_NUM

Used to configure the number of sockets, equal to the sum of WCHNET_NUM_IPRAW, WCHNET_NUM_UDP, WCHNET_NUM_TCP and WCHNET_NUM_TCP_LISTEN.

(6) WCHNET_NUM_PBUF

Used to configure the number of PBUF structures. The PBUF structure is mainly used to manage memory allocation, including applying for UDP, TCP, IPRAW memory and reception/transmission memory. If the application requires more socket connections and there is a large amount of data to send and receive, this value must be set larger.

(7) WCHNET_NUM_POOL_BUF

Number of POOL BUF. POOL BUF is mainly used for data reception. If a large amount of data needs to be received, this value should be set larger.

(8) WCHNET_NUM_TCP_SEG

Number of TCP Segments. Every time WCHNET sends a TCP packet, it must first apply for a TCP Segment. If the number of TCP connections is large and the amount of data to be sent and received is large, this value should be set larger. For example, there are currently 4 TCP connections, and each receive buffer is set to 2 TCP_MSS, assuming that an ACK is performed every time a packet of data is received, the WCHNET_NUM_TCP_SEG should be configured to be greater than $( 4 ^ { * } 2 )$ , which is only the most serious case. In fact, every time an ACK (or sent data) is received, the Segments of this data will be released.

(9) WCHNET_NUM_IP_REASSDATA

Number of reassembled IP packets. The size of each packet is WCHNET_SIZE_POOL_BUF, and the minimum value can be set to 1.

(10) WCHNET_TCP_MSS

Length of the maximum TCP segment. The maximum value is 1460 and the minimum is 60. Considering transfer and resources, it is recommended that this value should not be less than 536 bytes.

### (11) WCHNET_MEM_HEAP_SIZE

Heap allocation memory size, mainly used for some memory allocation of indeterminate length, such as sending data. If TCP has a large batch of data to send and receive, this value should be set larger. If the application layer memory needs to be used when sending data, please refer to CFG0_TCP_SEND_COPY in this section.

### (12) WCHNET_NUM_ARP_TABLE

ARP buffer. Used to store IP and MAC. The minimum value can be set to 1 and the maximum value is 0x7F. If WCHNET needs to communicate with 4 PCs, two of which send and receive data in bulk, it is recommended to set it to 4. If it is less than 2, it will seriously affect the communication efficiency.

### (13) CFG0_TCP_SEND_COPY

Valid only for TCP communication.

When CFG0_TCP_SEND_COPY is 1, the send copy function is enabled. WCHNET copies the data in application layer to the internal heap memory, and then packs and sends it.

When CFG0_TCP_SEND_COPY is 0, the send copy function is disabled. The library does not allocate new memory to dump the data, but use the data pointer as a function parameter to directly transfer it into the library for sending data, so the data pointed to by this data pointer cannot change until the packet is acknowledged.

### (14) CFG0_TCP_RECV_COPY

For debug. Enabled by default. If this value is 1, the speed is faster.

### (15) CFG0_TCP_OLD_EDLETE

When CFG0_TCP_OLD_EDLETE is 1 and WCHNET applies for no TCP connection, older TCP connections will be deleted. Disabled by default.

## 4.3 Interrupt

The interrupts are divided to global interrupts and socket interrupts. The states of the global interrupts are defined in the following table:

<table><tr><td>Bit</td><td>Name</td><td>Description</td></tr><tr><td>[5:7]</td><td>-</td><td>Reserved</td></tr><tr><td>4</td><td>GINT_STAT_SOCKET</td><td>socket interrupt</td></tr><tr><td>3</td><td>-</td><td>Reserved</td></tr><tr><td>2</td><td>GINT_STAT_PHY_CHANGE</td><td>PHY status change interrupt</td></tr><tr><td>1</td><td>-</td><td>Reserved</td></tr><tr><td>0</td><td>GINT_STAT_UNREACH</td><td>Unreachable interrupt</td></tr></table>

$\textcircled{1}$ GINT_STAT_UNREACH: Unreachable interrupt. When the library receives the ICMP unreachable interrupt packet, it saves the IP address, port, and protocol type of the unreachable

IP packet into the unreachable information list, and then this interrupt is generated. The application program can query the UnreachCode, UnreachProto and UnreachPort in the _NET_SYS structure to get unreachable information.

$\textcircled{2}$ GINT_STAT_PHY_CHANGE: PHY change interrupt. It is generated when the PHY connection of WCHNET changes, for example, the PHY state changes from a connected state to a disconnected state or from a disconnected state to a connected state. The application can get the current PHY status through WCHNET_GetPHYStatus.   
$\textcircled{3}$ GINT_STAT_SOCK: Socket interrupt. When the socket has an interrupt event, the library generates this interrupt, and the application can get the interrupt status of the socket through WCHNET_GetSocketInt.

The states of socket interrupts are defined in the following table:

<table><tr><td>Bit</td><td>Name</td><td>Description</td></tr><tr><td>7</td><td>-</td><td>Reserved</td></tr><tr><td>6</td><td>SINT_STAT_TIM_OUT</td><td>Timeout</td></tr><tr><td>5</td><td>-</td><td>Reserved</td></tr><tr><td>4</td><td>SINT_STAT_DISCONNECT</td><td>TCP disconnect</td></tr><tr><td>3</td><td>SINT_STAT_connect</td><td>TCP connect</td></tr><tr><td>2</td><td>SINT_STAT_RECV</td><td>Receive buffer not empty</td></tr><tr><td>[0:1]</td><td>-</td><td>Reserved</td></tr></table>

$\textcircled{1}$ SINT_STAT_RECV: Receive buffer not empty interrupt. It is generated after socket receives data. After the application layer receives this interrupt, WCHNET_SocketRecvLen can be used to get the length of the received data, and WCHNET_SocketRecv can be used to read the data in the receive buffer according to the length.   
$\textcircled{2}$ SINT_STAT_CONNECT: TCP connect interrupt. Only valid in TCP mode. It is generated after TCP connects successfully. The application layer can only transfer data after that.   
$\textcircled{3}$ SINT_STAT_DISCONNECT: TCP disconnect interrupt. Only valid in TCP mode. It is generated after TCP disconnects. The application layer must no longer transfer data after that.   
$\textcircled{4}$ SINT_STAT_TIM_OUT: In TCP mode, this interrupt is generated when a timeout occurs in the process of TCP connection, disconnection, sending data, etc. It is also generated when the connection is closed internally by the library if some exception occurs. In TCP mode, the socket is disabled once this interrupt is generated, and the related configuration of the socket is cleared. So if the application layer needs to use the socket again, it must re-initialize and connect or listen.

Note: The interrupts described in this section are flags of the variable. For the convenience of expression and easy understanding, they are described as interrupts in the library and this document, actually not the hardware interrupts of the MCU.

## 4.4 Socket

Socket types supported by the library: IPRAW, UDP, TCP client and TCP server.

## 4.5 IPRAW

Procedures to create an IPRAW socket:

$\textcircled{1}$ Set the protocol field, that is SourPort in IPRAW mode.   
$\textcircled{2}$ Set the target IP address.   
$\textcircled{3}$ Set the start address and the length of the receive buffer.   
$\textcircled{4}$ Set the protocol type to PROTO_TYPE_IP_RAW.   
$\textcircled{5}$ Call the WCHNET_SocketCreat function, and pass the above settings to this function.

WCHNET_SocketCreat will find a free information list in the socket information list, and copy the above configuration to it. If no free list is found, it returns error after the creation is successful, and the free information table will be output to the application layer.

IP message structure:

<table><tr><td>Target MAC</td><td>Source MAC</td><td>Type</td><td>IP header</td><td>IPRAW data</td><td>CRC32</td></tr><tr><td>6 Bytes</td><td>6 Bytes</td><td>2 Bytes</td><td>20 Bytes</td><td>Max 1480 bytes</td><td>4 Bytes</td></tr></table>

The application layer can call WCHNET_SocketSend send data, and the maximum length of an IPRAW packet allowed to be sent is 1480 bytes.The actual data transmission situation can be judged according to the return value of the function and the length of the returned data sent.

When the library receives the IP data packet, it first checks whether the protocol field and the protocol field set by the socket are the same. If they are the same, the IPRAW data packet is copied to the receive buffer and a SINT_STAT_RECV interrupt is generated. After the application layer receives this interrupt, call WCHNET_SocketRecvLen to get the effective length of the current socket buffer. According to the length, the application layer calls WCHNET_SocketRecv to read the data in the socket receive buffer. The application can read all the data at one time, or read it in multiple times. Since flow control cannot be performed in IPRAW mode, it is recommended that the application layer read all data immediately after querying the receive data interrupt, to avoid being overwritten by subsequent data.

Notes on protocol field settings:

The priority of the library processing IPRAW is higher than that of UDP and TCP. If the IP protocol field is set to 17 (UDP) or 6 (TCP), there may be a possibility of conflict with other sockets, which should be avoided. Two cases are listed below:

$\textcircled{1}$ Socket0 is in IPRAW mode, the IP protocol field is 17, and socket1 is in UDP mode. In UDP mode, the protocol field of the IP packet is also 17, and this causes the data for socket1 communication to be intercepted by socket0 and the data cannot be received.

$\textcircled{2}$ Socket0 is in IPRAW mode, the IP protocol field is 6, and socket1 is in TCP mode. In TCP mode, the protocol field of the IP packet is also 6, and this causes the data for socket1 communication to be intercepted by socket0 and the data cannot be received.

The library supports the reassembled IP, but the length of the maximum reassembled packet cannot be greater than the length of the receive buffer.

## 4.6 UDP client

Procedures to create a UDP socket:

$\textcircled{1}$ Set the source port.   
$\textcircled{2}$ Set the target port.   
$\textcircled{3}$ Set the target IP address.   
$\textcircled{4}$ Set the start address and the length of the receive buffer.   
$\textcircled{5}$ Set the protocol type to PROTO_TYPE_UDP.   
$\textcircled{6}$ Call the WCHNET_SocketCreat function, and pass the above settings to it.

WCHNET_SocketCreat will find a free information list in the socket information list, and copy the above configuration to it. If no free list is found, an error will be returned. If it is created successfully, a success will be returned, and the free information table will be output to the application layer.

UDP message structure:

<table><tr><td>Target MAC</td><td>Source MAC</td><td>Type</td><td>IP header</td><td>UDP header</td><td>UDP data</td><td>CRC32</td></tr><tr><td>6 Bytes</td><td>6 Bytes</td><td>2 Bytes</td><td>20 Bytes</td><td>8 Bytes</td><td>Max 1472 Bytes</td><td>4 Bytes</td></tr></table>

UDP is a simple and unreliable transport layer protocol for data messages. The transfer speed is fast, but the data cannot be guaranteed to reach the target. The application layer must ensure the reliability and stability of the transfer.

The application layer can call WCHNET_SocketSend send data, and the maximum length of an UDP packet allowed to be sent is 1472 bytes.The actual data transmission situation can be judged according to the return value of the function and the length of the returned data sent.

When the library receives the UDP data packet, the UDP data packet is copied to the receive buffer and a SINT_STAT_RECV interrupt is generated. After the application layer receives this interrupt, call WCHNET_SocketRecvLen to get the effective length of the current socket buffer. According to the length, the application layer calls WCHNET_SocketRecv to read the data in the socket receive buffer. The application can read all the data at one time, or read it in multiple times. Since flow control cannot be performed in UDP mode, it is recommended that the application layer read all data immediately after querying the receive data interrupt, to avoid being overwritten by subsequent data.

## 4.7 UDP server

UDP server can receive the local port address sent by any IP address.

Procedures to create a UDP socket:

$\textcircled{1}$ Set the source port.   
$\textcircled{2}$ Set the target IP address. The target address is 255.255.255.255.   
$\textcircled{3}$ Set the start address and the length of the receive buffer.   
$\textcircled{4}$ Set the protocol type to PROTO_TYPE_UDP.   
$\textcircled{5}$ Set the entry address of the receive callback function.   
$\textcircled{6}$ Call the WCHNET_SocketCreat function, and pass the above settings to it.

In UDP server mode, WCHNET adds the AppCallBack pointer in SocketInf, to distinguish the source IP and source port of the data packet. After UDP receives the data, it notifies the application layer of the source IP and source port of the data packet through AppCallBack. In the AppCallBack function, the application layer should read all the data, and WCHNET initializes all related variables of the receive buffer after calling back the AppCallBack.

If data is not received through callback, please be sure to initialize AppCallBack in SocketInf to 0.

## 4.8 TCP client

Procedures to create a TCP client socket:

$\textcircled{1}$ Set the source port.   
$\textcircled{2}$ Set the target port.   
$\textcircled{3}$ Set the target IP address.   
$\textcircled{4}$ Set the start address and the length of the receive buffer.   
$\textcircled{5}$ Set the protocol type to PROTO_TYPE_TCP.   
$\textcircled{6}$ Call the WCHNET_SocketCreat function, and pass the above settings to it.   
$\textcircled{7}$ Call the WCHNET_SocketConnect function, and TCP initiates connection.

WCHNET_SocketCreat will find a free information list in the socket information list, and copy the above configuration to it. If no free list is found, an error will be returned. If it is created successfully, a success will be returned, and the free information table will be output to the application layer.

After calling WCHNET_SocketConnect, the library actively initiates a connection request to the remote end. After connected successfully, a SINT_STAT_CONNECT connection interrupt is generated. If the remote end is not online or other exceptions occur, the library automatically retries. Both the number of retries and the retry period can be set in the application layer. If it is still unsuccessful, the library automatically closes the socket, and a SINT_STAT_TIM_OUT timeout interrupt is generated. Only after the connection interrupt is generated, the application layer can use this socket to send/receive data.

TCP message structure:   

<table><tr><td>Target MAC</td><td>Source MAC</td><td>Type</td><td>IP header</td><td>TCP header</td><td>TCP data</td><td>CRC32</td></tr><tr><td>6 Bytes</td><td>6 Bytes</td><td>2 Bytes</td><td>20 Bytes</td><td>20 Bytes</td><td>Max. 1460Bytes</td><td>4 Bytes</td></tr></table>

TCP provides reliable byte stream service for connection.

Unack Segment: TCP messages that are not sent successfully.

Two sending methods in WCHNET TCP mode:

1: Copy. Copy the user's data to Mem_Heap_Memory for sending. The total length of the data is not limited. If the length is greater than WCHNET_TCP_MSS, WCHNET divides the data into several TCP packets with the size of WCHNET_TCP_MSS for sending. It is generally used when the number of sockets is small and the amount of data to be sent is relatively small. The application layer only needs to call the WCHNET_SocketSend function.   
2: Non-copy. Send directly using the user buffer. The maximum length of data is WCHNET_TCP_MSS. It is generally used in the case of a large number of sockets, a large amount of data to be sent, and strict requirements on RAM.

Note when using the non-copy method:

Call WCHNET_SocketSend(sockeid,tcpdata,&len) to send, len must not be greater than TCP_MSS, tcpdata cannot be a buffer allocated locally or in the stack, and the application layer can no longer use the tcpdata buffer after calling WCHNET_SocketSend until WCHNET notifies the application layer that the data segment of this buffer is successfully sent.

After the application layer calls WCHNET_SocketSend to send data, the actual data transmission situation can be judged according to the return value of the function and the length of the returned data sent.

WCHNET notifies the application layer that the data segment is successfully sent through AppCallBack. The prototype of AppCallBack is as follows:

void (*pSockRecv)( struct _SCOK_INF *, uint32_t, uint16_t, uint8_t *, uint32_t);

In TCP mode, AppCallBack is used to notify the application layer of the number of socket Unack Segments, socinf is the socket information, and len is the number of Unack Segments. After the application layer gets the number, call WCHNET_QueryUnack to get the information of these messages. If tcpdata is not sent successfully, WCHNET writes tcpdata (buffer address) into addrlist. For details about WCHNET_QueryUnack, please refer to Section 3.25. For the configuration of the sending method, please refer to Section 4.2.

In TCP mode, if the data transmission fails, a SINT_STAT_TIM_OUT interrupt will be generated, and the application layer should close the socket.

When the library receives the TCP data packet, the TCP data packet is copied to the receive buffer and a SINT_STAT_RECV interrupt is generated. After the application layer receives this interrupt, call WCHNET_SocketRecvLen to get the effective length of the current socket buffer. According to the length, the application layer calls WCHNET_SocketRecv to read the data in the socket receive buffer. The application can read all the data at one time, or read it in multiple times. In TCP mode, every time the application layer calls WCHNET_SocketRecv, the library will copy the received data to the receive buffer of the application layer, and then notify the remote end of the current window size. For details about WCHNET_SocketRecv, please refer to Section 3.18.

## 4.9 TCP server

Procedures to create a TCP server socket:

$\textcircled{1}$ Set the source port.   
$\textcircled{2}$ Set the protocol type to PROTO_TYPE_TCP.

$\textcircled{3}$ Call the WCHNET_SocketCreat function, and pass the above settings to it.   
$\textcircled{4}$ Call the WCHNET_SocketListen function, and TCP starts listening.

Through the above procedures, a listening socket can be established. This socket only listens for client connections and itself does not send/receive data, so there is no need to set a receive buffer.

If a client connects successfully, the listening socket will find a free list from the socket information list. If no free list is found, it disconnects. If a free list is found, it initializes this list and write the information such as target IP, source port and target port into this list, and generate a SINT_STAT_CONNECT connection interrupt. After the application layer software receives this interrupt, it should immediately call WCHNET_ModifyRecvBuf to allocate a receive buffer for this connection. If the application software establishes multiple servers, it can determine which server is connected by querying the source port in the socket information list.

For the data structure, the process of sending data and receiving data, please refer to the TCP client mode.
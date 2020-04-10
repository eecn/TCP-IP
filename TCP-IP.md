计算机之间沟通的工具。   
TCP/IP（Transmission Control Protocol/Internet Protocol）是传输控制协议和网络协议的简称，它定义了电子设备如何连入因特网，以及数据如何在它们之间传输的标准。   
TCP/IP 不是一个协议，而是一个协议族的统称，里面包括了 IP 协议、ICMP 协议、TCP 协议、以及 http、ftp、pop3 协议等。网络中的计算机都采用这套协议族进行互联。   
### 网络协议栈架构
 OSI 七层模型:物理层、数据链路层、网络层、传输层、会话层、表示层、应用层。   
 TCP/IP四层模型:网际接口层、网络层、传输层、应用层    
 (1)应用层：应用程序通过这一层访问网络，常见 FTP、HTTP、DNS 和 TELNET 协议；

(2)传输层：TCP 协议和 UDP 协议；

(3)网络层：IP 协议，ARP、RARP 协议，ICMP 协议等；

(4)网络接口层：是 TCP/IP 协议的基层，负责数据帧的发送和接收。

IP地址四组32bit, `ifconfig -a` 查看Linux系统的IP地址。    

**域名** 域名服务系统 DNS(Domain Name System) 域名后缀com,net,org等
使用`ping www.github.com`或`nslookup www.baidu.com` 可以查看域名对应的IP   

**MAC**(Media Access Control)地址，物理地址、硬件地址，用来定义互联网中设备的位置。   
TCP/IP 层次模型中，网络层管理 IP 地址，链路层则负责 MAC 地址。因此每个网络位置会有一个专属于它的 IP 地址，而每个主机会有一个专属于它 MAC 地址。
**端口号** 16bit端口号识别，一个IP地址的端口可以有2^16(65536)个。
IP地址是用来发现和查找网络中的地址，但是不同程序如何互相通信则需要端口号识别。   
服务器的很多默认程序的端口号都是熟知的端口号。

**封装** 发送数据时，数据会在协议层自顶向下传递，通过每一层都会对数据增加以下首部或尾部信息，这样的信息称为协议数据单元(Protocol Data Unit，缩写为PDU),在分层协议系统里，指定的协议层上传输数据单元包含该层的协议控制信息和用户信息。   
>物理层（一层）PDU指数据位（Bit）
>数据链路层（二层）PDU指数据帧（Frame）
>网络层（三层）PDU指数据包（Packet）
>传输层（四层）PDU指数据段（Segment）
>第五层以上为数据（data）   
>![TCP](https://github.com/eecn/TCP-IP/blob/master/img/tcp.png)
**分用** 主机接收数据帧时，数据就从协议层底向上升，通过每一层时，检查并去掉对应层次的报文首部或尾部，与封装过程正好相反。   
**RFC** Request for Comment文档是所有以太网协议的正式标准。由 IETF 标准协会制定。   

## 链路层
网络层协议的数据单元是IP数据报 ，而数据链路层的工作就是把网络层交下来的 IP 数据报 封装为 帧(frame)发送到链路上，以及把接收到的帧中的数据取出并上交给网络层。
主要功能有:   
>将数据封装为帧（frame），帧是数据链路层的传送单位；
>控制帧的传输，包括处理传输差错，调节发送速率与接收方相匹配；
>在两个网络实体之间提供数据链路通路的建立、维持和释放的管理。
>![数据帧的结构](https://github.com/eecn/TCP-IP/blob/master/img/dataFrame.png)
#### 控制帧的传输
1.差错控制   
>反馈重发 计时器 序号
2.流量控制   
>流量控制实际上是对发送方数据流量的控制，使其发送速率不超过接收方的速率   
### 以太网
 CSMA/CD标准
 ### PPP点对点协议
 同等单元之间传输数据设计的链路层协议，设计目的主要是用来通过 拨号或专线 方式建立 点对点 连接发送数据，使其成为各种主机、网桥和路由器之间简单连接的一种共通的解决方案。   
 ### SLIP与PPP
>SLIP 的全称为 Serial Line IP(串行线路 IP)。它是一种对 IP 数据报进行封装的简单形式。    
>PPP
### MTU最大传输单元 
>接口MTU---指定的接口所允许发送的最大数据长度
>路径MTU---两台通信主机路径中最小的MTU值
>`netstat -in`可以查看网络接口的MTU   

## IP网络协议
### IP数据报
IP协议位于网络层，它是TCP/IP协议族中最为**核心**的协议，所有的TCP、UDP、ICMP及IGMP数据都以**IP数据报**格式传输。IP协议提供的是**不可靠** 、 **无连接**的数据报传送服务。
不可靠: IP协议不能保证数据报能成功地到达目的地，它仅提供传输服务。   
无连接: IP协议对每个数据报的处理是相互独立的。   
>![IP数据报格式](https://github.com/eecn/TCP-IP/blob/master/img/IPdata.png)   
>版本号(IPv4,IPv6)4bit, 首部长度4bit，服务类型，总长度16bit，标识16bit，标志3bit，偏移13bit，生存时间8bit，协议8bit，首部校验和，源IP,目的IP,选项。   
`sudo tcpdump -ntx -c 1` 抓包
### IP地址分类
网络号+主机号   
IP地址可被分为A,B,C,D,E五类   其中A,B,C类的网络号长度为8,16,24位。   
![IP地址](https://github.com/eecn/TCP-IP/blob/master/img/ipABC.png)     
### 子网划分
 IP = 网络号 + 子网号 + 主机号
![IP子网](https://github.com/eecn/TCP-IP/blob/master/img/ipSubnet.png)    
子网掩码来确定IP地址哪些位是主机号   
子网掩码中的 1 标识了 IP 地址中相应的网络号和子网号，0 标识了主机号。将 IP 地址和子网掩码进行 逻辑与运算 ，结果就能区分网络号和子网号。   
### IP路由选择
如果发送方与接收方直接相连（点对点）或都在一个共享网络上（以太网），那么 IP 数据报就能**直接**送达。 而大多数情况则是发送方与接收方通过若干个路由器(router)连接，那么数据报就需要经过若干个路由器的转发才能送达。
`route -n`查看路由表

### NAT技术
NAT（Network Address Translation，网络地址转换）内网IP转换成全球IP连接因特网。
`ifconfig eth0` 查看内网IP

### IP未来
IPv6 的地址长度是128位，通常将这128位的地址按每16位划分为一个段，将每个段转换成十六进制数字，并用冒号隔开，比如：2000:0000:0000:0000:0001:2345:6789:abcd 就是一个 IPv6 地址。地址众多，足够使用。


## 网络层其它协议

### ARP地址解析协议
Address Resolution Protocol   
功能:当主机通过数据链路发送数据的时候，IP数据报会先被封装为一个数据帧,而MAC地址会被添加到数据帧的报头。ARP便是在这个过程中通过目标主机的IP地址，查询目标主机的MAC地址。   
ARP缓存表: `arp -a` 查看arp缓存表   
ARP代理 Proxy ARP  ARP欺骗   
### RARP逆向地址解析协议
Reverse Address Resolution Protocol:将MAC地址转换为IP地址   
### ICMP控制报文协议
Internet Control Message Protocol 在通讯过程中发生各种问题时，ICMP 将问题反馈。    
![ICMP](https://github.com/eecn/TCP-IP/blob/master/img/ICMP.png)     
### ping程序
ping ping 程序是对两台主机之间连通性进行测试的基本工具，它只是利用ICMP回显请求和回显应答报文，而不用经过传输层（TCP/UDP）。
`ping IP 地址`
### TTL
Time To Live，该字段指定IP包被路由器丢弃之前允许通过的最大网段数量。利用ping命令返回的TTL值也可以判断操作系统类型。   
### traceroute程序
traceroute 程序是用来侦测主机到目的主机之间所经路由情况的重要工具。   
### IGMP组管理协议
管理多播组成员的一种协议。   

## 传输层协议 UDP
网络层通信的两端是主机，从传输层来看则是主机的进程在参与数据交换。   
![传输层](https://github.com/eecn/TCP-IP/blob/master/img/TransportLayer.png)    
TCP/IP 协议栈传输层有两个重要协议——UDP和TCP，不同的应用进程在传输层使用TCP或UDP之一。    
### 端口
系统端口号  登记端口号  短暂端口号   
`netstat -luant`命令列出了监听中的端口   
### UDP概述
User Datagram Protocol用户数据报协议。   
无连接、尽最大努力交付、面向报文、没有拥塞控制、支持多种交互方式   
![UDP](https://github.com/eecn/TCP-IP/blob/master/img/UDP.png)     
### UDP报文
UDP 数据报可分为两部分：UDP 报头和数据部分。   
![UDP报头](https://github.com/eecn/TCP-IP/blob/master/img/UDP2.png)    
### tcpdump抓取UDP报头

## TCP协议
### TCP简述
可靠、面向连接的、点对点、全双工、面向字节流   
`netstat -s`查看数据包统计信息   
### TCP报文段结构
TCP报文段可分为两部分：报头和数据部分    
![TCP报文段结构](https://github.com/eecn/TCP-IP/blob/master/img/TCP2.png)    
### 连接的建立与释放
客户端 服务端 三次握手   
![TCP三次握手](https://github.com/eecn/TCP-IP/blob/master/img/TCP3.png)     
数据传输结束后，双方释放连接   
![TCP释放连接](https://github.com/eecn/TCP-IP/blob/master/img/TCP4.png)     
### TCP可靠传输的实现
超时重发机制   
连续ARQ协议   
流量控制和拥塞控制   
### tcpdump抓取TCP报文段

## 应用层
传输层的UDP报文和TCP报文段的数据部分就是应用层交付的数据。   
不同类型的网络应用有不同的通信规则，因此应用层协议是多种多样的，比如DNS、FTP、Telnet、SMTP、HTTP、RIP、NFS等协议都是用于解决其各自的一类问题。   
### DNS协议
Domain Name Service域名服务,基于UDP,端口号为53   
DNS服务器  ---提供的是域名与IP地址的对应关系，
而ARP提供的是IP地址和MAC地址的对应关系。
host命令进行DNS查询   
DNS报文 DNS缓存 hosts文件

### FTP协议
File Transfer Protocol 文件传输协议基于TCP 端口号20(数据)和21(控制)   
### HTTP协议
HTTP (HyperText Transfer Protocol 超文本传输协议) 基于 TCP，使用端口号 80 或 8080。   
Telnet 协议是 TCP/IP 协议族中的一员，是 Internet 远程登录服务的标准协议和主要方式，它基于 TCP 协议，使用端口 23。   
TFTP（ Trivial File Transfer Protocol ）是 TCP/IP 协议族中的一个用来在客户机与服务器之间进行简单文件传输的协议，提供不复杂、开销不大的文件传输服务，它基于 UDP 协议，使用端口 69 。   
SMTP（Simple Mail Transfer Protocol）即简单邮件传输协议，它是一组用于由源地址到目的地址传送邮件的规则，由它来控制信件的中转方式,它使用 TCP 协议，使用端口 25 。   
POP3（Post Office Protocol Version 3 ）即邮局协议版本3，是 TCP/IP 协议族中的一员 ，主要用于支持使用客户端远程管理在服务器上的电子邮件，使用 TCP 协议，使用端口 110 。   

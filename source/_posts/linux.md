---
title: TCP/IP简介
author:
	name: 覃浩
	avatar: https://cdn.jsdelivr.net/gh/queen999/ImageHosting//imagesavatar.jpg
	url: https://www.zhengyuanyuan520.com
categories: 计算机网络
date: 2020-10-05 13:14:14
music:
  type: song  
  id: 1426649237
comments: true
---

知识点

- IP 地址
- 域名
- MAC 地址
- 端口号
- 封装和分用

<!-- more -->

提到网络协议栈结构，最著名的当属 OSI 七层模型，但是 TCP/IP 协议族的结构则稍有不同，它们之间的层次结构有如图对应关系：

![](https://cdn.jsdelivr.net/gh/queen999/ImageHosting/images/1548669082626.png)

可见 TCP/IP 被分为 4 层，每层承担的任务不一样，各层的协议的工作方式也不一样，每层封装上层数据的方式也不一样：

- 应用层：应用程序通过这一层访问网络，常见 FTP、HTTP、DNS 和 TELNET 协议；
- 传输层：TCP 协议和 UDP 协议；
- 网络层：IP 协议，ARP、RARP 协议，ICMP 协议等；
- 网络接口层：是 TCP/IP 协议的基层，负责数据帧的发送和接收。

> TCP/IP（Transmission Control Protocol/Internet Protocol）是传输控制协议和网络协议的简称，它定义了电子设备如何连入因特网，以及数据如何在它们之间传输的标准。
>
> TCP/IP 不是一个协议，而是一个协议族的统称，里面包括了 IP 协议、ICMP 协议、TCP 协议、以及 http、ftp、pop3 协议等。网络中的计算机都采用这套协议族进行互联。

# IP地址

网络上每一个节点都必须有一个独立的 IP 地址，通常使用的 IP 地址是一个 32bit 的数字，被 `.` 分成 4 组，例如，`255.255.255.255` 就是一个 IP 地址。有了 IP 地址，用户的计算机就可以发现并连接互联网中的另外一台计算机。

在 终端输入 `ifconfig -a` 命令查看自己的 IP 地址：

```shell
ifconfig -a
```

![](https://cdn.jsdelivr.net/gh/queen999/ImageHosting/images/61acdc3e0704fdf5fb97876dec817ff3-0)

# 域名

用 12 位数字组成的 IP 地址很难记忆，在实际应用时，用户一般不需要记住 IP 地址，互联网给每个 IP 地址起了一个别名，习惯上称作域名。

域名与计算机的 IP 地址相对应，并把这种对应关系存储在域名服务系统 DNS(Domain Name System) 中，这样用户只需记住域名就可以与指定的计算机进行通信了。

常见的域名包括 com、net 和 org 三种顶级域名后缀，除此之外每个国家还有自己国家专属的域名后缀（比如我国的域名后缀为 cn）。目前经常使用的域名诸如百度（[www.baidu.com](https://www.baidu.com/)）、Linux 组织（[www.lwn.net](https://lwn.net/)）等等。

我们可以使用命令 `nslookup` 或者 `ping` 来查看与域名相对应的 IP 地址，由于实验楼网络限制，我们可以使用 `ping github.com`（如果 `github` 也 ping 不通，那么可以使用 `ping labfile.oss.aliyuncs.com`，如果你是会员账户，那么也可以 ping 其他的域名）查看。

例如：

![](https://cdn.jsdelivr.net/gh/queen999/ImageHosting/images/20201016215115.png)

# MAC地址

MAC（Media Access Control）地址，或称为物理地址、硬件地址，用来定义互联网中设备的位置。

在 TCP/IP 层次模型中，网络层管理 IP 地址，链路层则负责 MAC 地址。因此每个网络位置会有一个专属于它的 IP 地址，而每个主机会有一个专属于它 MAC 地址。

# 端口号

IP 地址是用来发现和查找网络中的地址，但是不同程序如何互相通信呢？这就需要端口号来识别了。如果把 IP 地址比作一间房子，端口就是出入这间房子的门。真正的房子只有几个门，但是端口采用 16 比特的端口号标识，一个 IP 地址的端口可以有 65536（即：216）个之多！

服务器的默认程序一般都是通过人们所熟知的端口号来识别的。例如，对于每个 TCP/IP 实现来说，SMTP（简单邮件传输协议）服务器的 TCP 端口号都是 `25`，FTP（文件传输协议）服务器的 TCP 端口号都是 `21`，TFTP（简单文件传输协议）服务器的 UDP 端口号都是 `69`。任何 TCP/IP 实现所提供的服务都用众所周知的 `1－1023` 之间的端口号。这些人们所熟知的端口号由 Internet 端口号分配机构（Internet Assigned Numbers Authority，IANA）来管理。

常用协议对应端口号：

- SSH 22
- FTP 20 和 21
- Telnet 23
- SMTP 25
- TFTP 69
- HTTP 80
- SNMP 161
- Ping 使用 ICMP，无具体端口号

# 封装和分用

**封装**：当应用程序发送数据的时候，数据在协议层次当中自顶向下通过每一层，每一层都会对数据增加一些首部或尾部信息，这样的信息称之为协议数据单元（Protocol Data Unit，缩写为 PDU），在分层协议系统里，在指定的协议层上传送的数据单元，包含了该层的协议控制信息和用户信息。如下图所示：

- 物理层（一层）PDU 指数据位（Bit）
- 数据链路层（二层）PDU 指数据帧（Frame）
- 网络层（三层）PDU 指数据包（Packet）
- 传输层（四层）PDU 指数据段（Segment）
- 第五层以上为数据（data）

![](https://dn-simplecloud.shiyanlou.com/uid/8797/1548670748600.png)

**分用**：当主机收到一个数据帧时，数据就从协议层底向上升，通过每一层时，检查并去掉对应层次的报文首部或尾部，与封装过程正好相反。

# RFC

RFC（Request for Comment）文档是所有以太网协议的正式标准，并在其官网上面公布，由 IETF 标准协会制定。大量的 RFC 并不是正式的标准，出版的目的只是为了提供信息。RFC 的篇幅不一，从几页到几百页不等。每一种协议都用一个数字来标识，如 RFC 3720 是 iSCSI 协议的标准，数字越大意味着 RFC 的内容越新或者是对应的协议（标准）出现的比较晚。
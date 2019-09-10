---
title: TCP/IP协议
comments: true
toc: true
date: 2019-09-10 08:41:44
categories: HTTP
tags: TCP/IP
---

# 起源

## HTTP的来历

CERN(欧洲核子研究组织)的蒂姆 • 伯纳斯 - 李(Tim BernersLee)博士提出了一种能让远隔两地的研究者们共享知识的设想。

最初的设想是通过超文本(HyperText)标记,各地互联连接成可相互参阅的<font color=#f07c82>WWW(World Wide Web, 万维网)</font>。

目前WWW构建的关键技术为：

+ 基于SGML(Standard Generalized Markup Language,标准通用标记语言)的<font color=#f07c82>HTML （HyperText Markup Language, 超文本标记语言）</font>。

+ 指定文档所在的地址<font color=#f07c82>URL (Uniform Resource Locator, 统一资源定位符) </font>。

各个Web服务器通过万维网互相连接，以 HTML 标准编辑网页（存储信息），用 URL 实现信息的查找，最后通过Web浏览器实现界面的渲染。

## 如何控制信息的传输？

然而，Web服务器有各种各样的类型及硬件接口，显示平台也有各种各样的类型，如何实现平台之间信息的互相交流呢？ —— <font color=#f07c82>统一协议控制</font>。答案是通过统一的<font color=#f07c82>协议 (Protocol)</font>。

#　TCP/TP 协议簇

TCP/IP　有说法是专指TCP和IP协议。

这里指<font color=#f07c82>互联网相关的各类协议簇的总称</font>，例如：IP, PPPoE, TCP, FTP, HTTP, FDDI, IEEE 802.3, SNMP, UDP, DNS, ICMP　等等。

也就是说　HTTP　是　TCP/IP　协议的子集。

## 分层管理

在逻辑上，TCP/IP分为四个层次：<font color=#f07c82>应用层、传输层、网络层和数据链路层</font>。

名称 | 作用
:-:|:-:
应用层 | 客户端实现信息的封装与解析。该层使用的协议如 HTTP、DNS (Domain Name System, 域名系统)、FTP (File Transfer Protocol, 文件传输协议)等。
传输层 | 提供网络连接中两台计算机之间的数据传输。使用的协议：TCP (Transmission Control Protocol) 和 UDP (User Data Protocol, 用户数据协议)。
网络层 | 将网络中流动的数据包（网络传输最小数据单位）通过规定的路径（传输线路）到达对方的计算机，并把数据传给对方。
数据链路层 | 肉眼可见的硬件层，包括网卡、光纤等。

{% asset_img tcp.png %}

## 数据传输过程

1. 用户根据HTTP协议发送一个数据请求（应用层）。

2. 获得应用层数据后，传输层（TCP协议）为了方便，将数据（HTTP请求报文）进行分割，每个报文打上标记序号及端口号，然后转发给网络层。

3. 网络层将通信目的地的DNS增加到数据中，然后转发给数据链路层。

4. 数据链路层将数据发给目的地终端。

{% asset_img transform.png %}

在解析数据时，按照数据包装的逆序层层解包（**去除每一层打上的首部信息**），最终获得请求的HTTP报头。


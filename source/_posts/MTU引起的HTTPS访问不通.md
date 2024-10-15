---
title: MTU引起的HTTPS访问不通
date: 2024-05-19 13:54:41
tags:
    - MTU
    - 隧道
    - HTTPS
topic: 网络故障分析
---
要了解这个问题为什么会出现，得先知道几个概念
## 一些概念科普
### IPv4 报文分段和重组
- 尽管 IPv4 数据报的最大长度为 65535 字节，但大多数传输链路强制执行更小的最大数据包长度限制（即 MTU）。MTU值取决于传输链路。
- IP报文中的DF字段设置为1代表报文不进行分片
- 报文的分段和重组会影响传输效率
### MTU
- **最大传输单元（Maximum Transmission Unit，MTU）**
- 报文转发时的最大尺寸，超过最大传输单元，要么进行分片，要么丢弃
### TCP MSS
- MSS（Maximum Segment Size，最大报文长度），是TCP协议定义的一个选项，MSS选项用于在TCP连接建立时，收发双方协商通信时每一个报文段所能承载的最大数据长度
- TCP会在建立连接时，也就是三次握手的时候，协商出来MSS的值，以小值为优先

### PMTU
- 一种动态发现因特网上任意一条路径的[最大传输单元](https://link.zhihu.com/?target=https%3A//baike.baidu.com/item/%25E6%259C%2580%25E5%25A4%25A7%25E4%25BC%25A0%25E8%25BE%2593%25E5%258D%2595%25E5%2585%2583/9730690)(MTU)的技术。不过如果中间设备禁了ICMP协议报文后，这种技术就无效了

### 隧道
- 隧道是一种逻辑接口，它提供了一种将乘客数据包封装在传输协议内的方法，这种架构旨在为点对点封装方案的实施提供服务。
	- 隧道主要的应用是：允许在 WAN 或 Internet 中建立虚拟专用网络 (VPN)。
- 常见的隧道协议有以下几种
	- GRE
	- IPSEC
- 正常数据包和隧道数据包:
![image.png](https://tuchuang-1258743955.cos.ap-beijing.myqcloud.com/image/20240515151517.png)

咱们可以了解到通过隧道的报文，要比正常的数据报文多封装一些信息

## 问题分析
当终端与服务器通信时，由于两端的接口Mtu是1500，那么他们的TCP MSS协商，正常会协商为1460，如图（BGP建立时的tcp三次握手）：
![image.png](https://tuchuang-1258743955.cos.ap-beijing.myqcloud.com/image/20240516104147.png)
但是，如果中间经过IPsec隧道，加上IPsec报头的话，1460 字节的数据包经过 IPv4sec 加密，增加了 52 字节的开销（IPv4sec 报头、报尾和另外的 IPv4 报头）。现在 IPv4sec 需要发送 1512 字节的数据包。由于出站MTU为1500，因此必须对此数据包进行分段，如果报文中设定了不分片，或者中间的网络设备不支持分片，那么这样的报文将会丢弃，再看下面这张图，是HTTPS 证书认证的报文，这个报文是不分片的。
![image.png](https://tuchuang-1258743955.cos.ap-beijing.myqcloud.com/image/20240516105732.png)
综上所述，我们应该已经理解了为什么经过ipsec访问https的业务会出现不通的现象，原因就是因为TCP-MSS协商的MTU为1460，加上ipesc的报文头部本来就超过默认mtu了，再加上https的tls报文不分片，所有导致了这个情况。

> HTTPS的报文为什么不分片
> - 在HTTPS中，IP报头中的DF字段（Don't Fragment）设置为1的主要原因是确保HTTPS数据包在传输过程中不会被分片。HTTPS使用TLS（Transport Layer Security）来加密通信内容，TLS协议的一种重要特性是完整性保护，即在传输过程中保证数据的完整性，防止被篡改。
> - 当DF字段设置为1时，它指示路由器在转发IP数据报时不分片。这就确保了HTTPS的数据包在传输过程中保持完整，不会被分割成更小的片段，从而保证了TLS加密机制的正确性和安全性。因为如果数据包被分片，那么在重新组装时可能会出现片段丢失或者篡改，从而影响了整个通信的完整性和安全性。
> - 所以，通过设置DF字段为1，可以确保HTTPS数据包的完整性和安全性，同时也提高了网络通信的效率，减少了可能的数据包重组导致的性能损失。

## 问题解决-中间设备介入TCP MSS的协商
* 修改mtu，服务器和终端设备，让其mss协商值降低，但是如果服务器和终端很多的话，那修改起来就相当麻烦
* 由于PMTUD可能存在ICMP差错报文被过滤的情况，很多中间设备的接口支持adjust tcp mss设置功能，思科路由器一般是在接口模式下使用命令“ip tcp adjust-mss 1400 ”来做设置，其他的品牌产品的相关设置大家可在实际工作环境下自查相关品牌和产品的使用手册。
* 这个功能主要是通过由中间设备修改经过其转发的TCP SYN报文中的MSS值，让中间设备参与进TCP 三次握手时SYN报文的MSS协商来避免分片。
Linux通过修改iptables，来干预AZ两端的tcp-mss协商，具体命令如下:
```
sudo iptables -t mangle -I PREROUTING 1 -i cl18955 -p tcp -m tcp --tcp-flags SYN,RST SYN -j TCPMSS --set-mss 1320  
sudo iptables -t mangle -I POSTROUTING 1 -o cl18955 -p tcp -m tcp --tcp-flags SYN,RST SYN -j TCPMSS --set-mss 1320
```

## 参考
- https://zhuanlan.zhihu.com/p/139537936
- https://www.cisco.com/c/zh_cn/support/docs/ip/generic-routing-encapsulation-gre/25885-pmtud-ipfrag.html
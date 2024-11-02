---
title: Netowk09-ARP
date: 2024-05-13 21:33:11
tags:
- network
- 基础知识
categories: 数据通信
cover: https://tuchuang-1258743955.cos.ap-beijing.myqcloud.com/image/20240513213344.png
---
## 前言
上一章学习到了IPv4地址，然后咱们先来再来学习一个地址，叫做MAC地址
## MAC地址介绍
- **MAC地址**（**英语：Media Access Control Address）**，直译为**媒体访问控制地址**，也称为**局域网地址**（LAN Address），**以太网地址**（Ethernet Address）或**物理地址**（Physical Address），它是一个用来确认网络设备位置的地址。
- IP地址工作在三层，而mac地址则工作在二层
- mac地址是唯一的，每一个网络设备，比如服务器上的一个网卡，交换机上的一个端口，都会有一个mac地址，是由产商分配的
### MAC地址的格式
- MAC地址共48位（6个字节），以[十六进制](https://zh.wikipedia.org/wiki/%E5%8D%81%E5%85%AD%E9%80%B2%E4%BD%8D "十六进制")表示。第一个byte的最低有效比特(LSB)为单播地址(0)/多播地址(1)，第一个byte从最低有效比特数去第2个bit为广域地址(0)/区域地址(1)。前3~24位由[IEEE](https://zh.wikipedia.org/wiki/IEEE "IEEE")决定如何分配给每一家制造商，且不重复，后24位由实际生产该网络设备的厂商自行指定且不重复。
- ff:ff:ff:ff:ff:ff则作为广播地址。
- 01:xx:xx:xx:xx:xx是多播地址，01:00:5e:xx:xx:xx是[IPv4](https://zh.wikipedia.org/wiki/IPv4 "IPv4")多播地址。
### 有了mac地址，还要ip地址做什么？
- 因为mac地址虽然是全球唯一的，但是并不能确定每一个mac地址的具体位置，网络设备都是查表转发，而一张表中不可能把全球的mac地址都放上去，并且路径都固定好，所以依靠mac地址是能做到远程数据通信和全球互联的。
## ARP协议的定义
- ARP协议可以协助设备学习到对端设备的mac地址和IP地址的对应关系
- 一个接口可以配置多个IPv4地址，但是mac地址只有一个，所以我们需要将IPv4地址和mac地址对应起来，这样才能找到我们通信的目标。
## ARP报文格式
![](https://tuchuang-1258743955.cos.ap-beijing.myqcloud.com/image/20221204152605.png)
![image.png](https://tuchuang-1258743955.cos.ap-beijing.myqcloud.com/image/20240513213155.png)

## 原理
### 交互机制
![](https://tuchuang-1258743955.cos.ap-beijing.myqcloud.com/image/20221204152714.png)
- ARP协议有两种报文，Request和Reply报文
- 设备在进行通信时，会查arp缓存表进行封装报文，如果arp缓存中没有目的设备时，会广播ARP Request报文来找寻目的设备，*此报文二层封装目的mac地址为全FF，代表这是一个广播报文，arp封装的目的mac地址为全00，代表未知mac地址
	- ![](https://tuchuang-1258743955.cos.ap-beijing.myqcloud.com/image/20221204152135.png)
- 目的设备在收到ARP Request的报文时，会单播回复一个Reply报文
	- ![](https://tuchuang-1258743955.cos.ap-beijing.myqcloud.com/image/20221204152341.png)


## 免费ARP
- 一般是路由器在配置IP地址时会自动发送一个ARP报文，来探测是否有IP冲突的现象，此时*arp封装的源IP和目的IP都是自己*
	-  ![](https://tuchuang-1258743955.cos.ap-beijing.myqcloud.com/image/20221204151856.png)

## 参考文献
- [MAC地址 - 维基百科，自由的百科全书 (wikipedia.org)](https://zh.wikipedia.org/wiki/MAC%E5%9C%B0%E5%9D%80)
- [什么是ARP？它是如何进行地址解析的？ - 华为 (huawei.com)](https://info.support.huawei.com/info-finder/encyclopedia/zh/ARP.html)
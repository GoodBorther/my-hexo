---
title: Network13-DHCP
date: 2024-06-03 21:40:22
tags:
- network
- 基础知识
categories: 数据通信
cover: https://tuchuang-1258743955.cos.ap-beijing.myqcloud.com/image/20240603214149.png
---
{% quot 动态主机配置协议DHCP（Dynamic Host Configuration Protocol） icon:default %}
## 简单的背景介绍
> 为什么要使用DHCP协议
- 其实在我看来，这是一个很简单的问题，因为IPv4协议虽然研发出来了，但怎么配置IP地址是一个问题，如果全部手动配置的话，那工作量实在太大，所以才有了动态地址分配。也就是DHCP。
- DHCP于1993年10月成为标准协议，其前身是BOOTP协议，[RFC2131](https://www.rfc-editor.org/rfc/rfc2131)定义了DHCP协议的标准

## DHCP如何工作
- 基本介绍
	- DHCP采用C/S架构模式
	- DHCP协议采用UDP作为传输协议，DHCP客户端发送请求消息到DHCP服务器的68号端口，DHCP服务器回应应答消息给DHCP客户端的67号端口。
- DHCP的工作模式可以分为两种
	- 一种是有中继的模式，客户端不直接找服务端，而是找中继
	- DHCP的服务端和客户端直接通信
- DHCP的报文交互也非常简单
	1. 发现阶段，客户端广播发送DHCP DISCOVER报文
	2. 提供阶段，服务器回应DHCP OFFER报文
	3. 选择阶段，客户端广播发送DHCP REQUEST报文
	4. 确认阶段，服务器回应DHCP ACK报文
	   **这四个阶段怎么实现的，我想先不用关心，因为大多数时候也用不着，除了选择阶段，按理服务器都给你回OFFER报文了，你还选择什么，其实呢，这个是用于如果有多个dhcp服务器的情况下进行选择的，其实一般组网，也就一个dhcp服务器**
- DHCP除了可以动态分配，还可以静态分配，也就是给某个主机分配固定的IP地址

## DHCP的租期
- 在DHCP中有一个租期的概念，也就是服务端把地址给你的，但不是永久给你的，到了一定期限，你还得向我续租才行
- 租期主要是为了避免客户端下线后IP地址继续被占用
- 租期报文的交互（这也不用记住，知道就行了，用的时候再查）
	1. 当租期达到50%（T1）时，DHCP客户端会自动以单播的方式向DHCP服务器发送DHCP REQUEST报文，请求更新IP地址租期。如果收到DHCP服务器回应的DHCP ACK报文，则租期更新成功（即租期从0开始计算）；如果收到DHCP NAK报文，则重新发送DHCP DISCOVER报文请求新的IP地址。
	2. 当租期达到87.5%（T2）时，如果仍未收到DHCP服务器的应答，DHCP客户端会自动以广播的方式向DHCP服务器发送DHCP REQUEST报文，请求更新IP地址租期。如果收到DHCP服务器回应的DHCP ACK报文，则租期更新成功（即租期从0开始计算）；如果收到DHCP NAK报文，则重新发送DHCP DISCOVER报文请求新的IP地址。
	3. 如果租期时间到时都没有收到服务器的回应，客户端停止使用此IP地址，重新发送DHCP DISCOVER报文请求新的IP地址。

今天就先到这里，下一篇通过实验来讲解dhcp和dhcp中继

## 参考
- [什么是DHCP？为什么要用DHCP？ - 华为 (huawei.com)](https://info.support.huawei.com/info-finder/encyclopedia/zh/DHCP.html)
- [动态主机配置协议_百度百科 (baidu.com)](https://baike.baidu.com/item/%E5%8A%A8%E6%80%81%E4%B8%BB%E6%9C%BA%E9%85%8D%E7%BD%AE%E5%8D%8F%E8%AE%AE/10778663)
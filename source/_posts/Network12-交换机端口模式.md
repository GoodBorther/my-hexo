---
title: Network12-交换机端口模式
date: 2024-05-16 21:37:12
tags:
- network
- 基础知识
categories: 数据通信
cover: https://tuchuang-1258743955.cos.ap-beijing.myqcloud.com/image/20240516213750.png
---
- 交换机的端口有三种工作模式
- 交换机收到的报文，有两种情况，不带VlanTag的数据包和带VlanTag的数据包，那么端口收到这两种报文怎么处理呢，可以有下面三种处理方式
## Access
- 用于对接计算机的接口
- Access接口只能属于一个Vlan
>华为产品文档：
 Access接口一般用于和不能识别Tag的用户终端（如用户主机、服务器等）相连，或者不需要区分不同VLAN成员时使用。Access接口大部分情况只能收发Untagged帧，且只能为Untagged帧添加唯一VLAN的Tag。交换机内部只处理Tagged帧，所以Access接口需要给收到的数据帧添加VLAN Tag，也就必须配置缺省VLAN。配置缺省VLAN后，该Access接口也就加入了该VLAN。当Access接口收到带有Tag的帧，并且帧中VID与PVID相同时，Access接口也能接收并处理该帧。为了防止用户私自更改接口用途，接入其他交换设备，可以配置接口丢弃入方向带Tag的报文。
## Trunk
- 交换机与交换机之间相连的链路
>华为产品文档：
>Trunk接口一般用于连接交换机、路由器、AP以及可同时收发Tagged帧和Untagged帧的语音终端。它可以允许多个VLAN的帧带Tag通过，但只允许一个VLAN的帧从该类接口上发出时不带Tag（即剥除Tag）
## Hybrid
- 灵活，但是配置并不方便，既可以连接终端，可以作为交换机之间的干道
>华为产品文档：
>Hybrid接口既可以用于连接不能识别Tag的用户终端（如用户主机、服务器等）和网络设备（如Hub、傻瓜交换机），也可以用于连接交换机、路由器以及可同时收发Tagged帧和Untagged帧的语音终端、AP。它可以允许多个VLAN的帧带Tag通过，且允许从该类接口发出的帧根据需要配置某些VLAN的帧带Tag（即不剥除Tag）、某些VLAN的帧不带Tag（即剥除Tag）。
Hybrid接口和Trunk接口在很多应用场景下可以通用，但在某些应用场景下，必须使用Hybrid接口。比如在灵活QinQ中，服务提供商网络的多个VLAN的报文在进入用户网络前，需要剥离外层VLAN Tag，此时Trunk接口不能实现该功能，因为Trunk接口只能使该接口缺省VLAN的报文不带VLAN Tag通过。

## Qinq
- 用于私网与公网的对接
>华为产品文档：
>QinQ接口（802.1Q-in-802.1Q）使用QinQ协议，一般用于私网与公网之间的连接。它可以给帧加上双层Tag，即在原来Tag的基础上，给帧加上一个新的Tag，从而可以支持多达4094×4094个VLAN，满足网络对VLAN数量的需求。外层的Tag通常被称作公网Tag，用来标识公网的VLAN；内层Tag通常被称作私网Tag，用来标识私网的VLAN。QinQ接口也被称为Dot1q-tunnel接口。
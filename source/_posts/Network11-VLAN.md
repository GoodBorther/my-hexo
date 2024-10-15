---
title: Network11-VLAN
date: 2024-05-15 22:50:35
tags:
- network
- 基础知识
categories: 数据通信
cover: https://tuchuang-1258743955.cos.ap-beijing.myqcloud.com/image/20240515225113.png
---
## 简介
- 虚拟局域网

## 作用
- 隔绝广播域
	- 广播域的概念
		- 广播是一种信息的传播方式，指网络中的某一设备同时向网络中所有的其它设备发送数据，这个数据所能广播到的范围即为广播域(Broadcast Domain)。
		- 简单点说，广播域就是指网络中所有能接收到同样广播消息的设备的集合。


## 可用范围
- 2~4096
- Vlan1为接口默认Vlan

## 缺省Vlan
- 又称为PVID
- 当交换机收到Untagged帧时，就需要给该帧添加Tag，添加什么Tag，就由接口上的缺省VLAN决定。

### 作用（摘抄至华为产品文档）:

- 当接口接收数据帧时，如果接口收到一个Untagged帧，交换机会根据PVID给此数据帧添加等于PVID的Tag，然后再交给交换机内部处理；如果接口收到一个Tagged帧，交换机则不会再给该帧添加接口上PVID对应的Tag。
- 当接口发送数据帧时，如果发现此数据帧的Tag的VID值与PVID相同，则交换机会将Tag去掉，然后再从此接口发送出去。

每个接口都有一个缺省VLAN。缺省情况下，所有接口的缺省VLAN均为VLAN1，但用户可以根据需要进行配置：

- 对于Access接口，缺省VLAN就是它允许通过的VLAN，修改接口允许通过的VLAN即可更改接口的缺省VLAN。
- 对于Trunk接口和Hybrid接口，一个接口可以允许多个VLAN通过，但是只能有一个缺省VLAN，修改接口允许通过的VLAN不会更改接口的缺省VLAN。
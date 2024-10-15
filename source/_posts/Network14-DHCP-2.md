---
title: Network14-DHCP-2
date: 2024-06-04 20:54:09
tags:
- network
- 基础知识
- dhcp
categories: 数据通信
cover: https://tuchuang-1258743955.cos.ap-beijing.myqcloud.com/image/20240603214149.png
---
## DHCP服务器的基础配置
在华为路由器上配置DHCP服务，拓扑如下：
![image.png](https://tuchuang-1258743955.cos.ap-beijing.myqcloud.com/image/20240604200318.png)
咱们的目的是让PC1的网卡能够通过dhcp自动获取IP，具体配置如下：
AR1：
```bash
[dhcp-server]dhcp enable
interface GigabitEthernet0/0/0
 ip address 192.168.0.254 255.255.255.0  ##配置接口地址，默认网关为接口本地址，地址池就是192.167.0.0/24
 dhcp select interface ## 在接口启用dhcp功能
 dhcp server lease day 30 hour 0 minute 0  ## 设置dhcp租期为30天
 dhcp server dns-list 8.8.8.8 ## 设置获取到的dns为8.8.8.8
```

PC1:
配置网口ipv4配置为dhcp自动获取:
![image.png](https://tuchuang-1258743955.cos.ap-beijing.myqcloud.com/image/20240604201033.png)

在命令行查看获取到的ipv4参数是否和预期一致:
![image.png](https://tuchuang-1258743955.cos.ap-beijing.myqcloud.com/image/20240604201325.png)

只需要简单的几行配置，就在华为路由器上实现了一个dhcp服务器的基础功能，下面我们来看下dhcp中继是怎么个事。

## DHCP中继
先说一下DHCP Relay，也就是dhcp中继是为了解决什么问题吧
其实总结起来简单的一句话就是，当dhcp服务器和客户端不在一个子网时，为了解决跨网段时，连接的终端设备也能正常获取到指定的dhcp服务器下发的地址，如图所示:
![image.png](https://tuchuang-1258743955.cos.ap-beijing.myqcloud.com/image/20240604201834.png)
下面我们来实现一下DHCP中继，拓扑图如下：
![image.png](https://tuchuang-1258743955.cos.ap-beijing.myqcloud.com/image/20240604203509.png)
### 需求如下
- AR1为dhcp服务器
- LSW1为dhcp中继设备
- PC1获取到192.168.0.0/24网段的地址
### 具体实现配置如下
AR1:
```
[dhcp-server]dhcp  enable
[dhcp-server]interface GigabitEthernet0/0/0
[dhcp-server-GigabitEthernet0/0/0]ip address 100.100.100.1 255.255.255.252
[dhcp-server-GigabitEthernet0/0/0]dhcp select global
[dhcp-server]ip route-static 0.0.0.0 0 100.100.100.2
[dhcp-server]ip pool pool1 ##配置地址池
[dhcp-server] gateway-list 192.168.0.254 
[dhcp-server]network 192.168.0.0 mask 255.255.255.0 
[dhcp-server]ip route-static 0.0.0.0 0.0.0.0 100.100.100.2 ##配置默认路由
```

LSW1：
```
[SwitchA]vlan ba 10 20
[SwitchA]dhcp enable
[SwitchA]dhcp server group dhcpgroup1
[SwitchA]dhcp-server 100.100.100.1 0
[SwitchA]interface Vlanif10
[SwitchA-Vlanif10] ip address 192.168.0.254 255.255.255.0
[SwitchA-Vlanif10] dhcp select relay
[SwitchA-Vlanif10] dhcp relay server-select dhcpgroup1
[SwitchA]int vlan 20
[SwitchA-Vlanif20]ip address 100.100.100.2 255.255.255.252
[SwitchA]int GigabitEthernet 0/0/2
[SwitchA-GigabitEthernet0/0/2] port link-type access
[SwitchA-GigabitEthernet0/0/2] port default vlan 10
[SwitchA]int GigabitEthernet 0/0/1
[SwitchA-GigabitEthernet0/0/1] port link-type access
[SwitchA-GigabitEthernet0/0/1] port default vlan 20
```

查看PC1有无正常获取地址
![image.png](https://tuchuang-1258743955.cos.ap-beijing.myqcloud.com/image/20240604205331.png)

## 参考
- [华为企业产品技术支持网站 - 华为 (huawei.com)](https://support.huawei.com/enterprise/zh/doc/EDOC1100198436/be771137)
- [S300, S500, S2700, S3700, S5700, S6700, S7700, S7900, S9700系列交换机 典型配置案例 - 华为 (huawei.com)](https://support.huawei.com/enterprise/zh/doc/EDOC1000069491/6946084b)
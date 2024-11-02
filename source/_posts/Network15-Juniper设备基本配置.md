---
title: Network15-Juniper设备基本配置
date: 2024-10-31 14:04:44
tags:
- network
- 基础知识
categories: [数据通信]
cover: https://tuchuang-1258743955.cos.ap-beijing.myqcloud.com/image/20241031140642.png
---
## 背景
- 公司有一台Juniper MX480的路由器承载了全球的路由条目，所以这篇文章整理了Juniper的常用命令，方便后续维护
- PS：Juniper设备的配置命令还是很规范的，也很容易看懂
## 查看路由条目
```
show route 
```
## 查看设备配置，以命令的方式
```
 show configuration |display set
```
## 配置静态路由
```
ipv4
set routing-options static route 10.10.10.0/24 next-hop 192.168.0.1
ipv6
set routing-options rib inet6.0 static route 2222:ccc0:1::/59 next-hop fc00:8c20:1833:1000::ad5:0002
```
## 路由策略配置（一眼懂）
```
set policy-options policy-statement test term 5 from route-filter 0.0.0.0/0 exact
set policy-options policy-statement test term 5 from route-filter 192.168.10.0/24 exact
set policy-options policy-statement test term 5 then reject
set policy-options policy-statement test term 10 from route-filter 172.16.0.0/15 exact
set policy-options policy-statement test term 10 from route-filter 172.17.10.0/24 exact
set policy-options policy-statement test term 30 then accept
```
## BGP配置
```
set routing-options autonomous-system 65534 ##设置本地的AS号
set protocols bgp group ebgp type external 
set protocols bgp group ebgp peer-as 6939 ##邻居的AS号
set protocols bgp group ebgp neighbor 192.168.0.1 import test ##绑定路由策略
set protocols bgp group ebgp neighbor 192.168.0.1 export test_export
set protocols bgp group ebgp neighbor 4111:0472:1231:447a:0000:0000:0000:0001 export ipv6
```

## 查看所有的BGP邻居状态
```
show bgp summary
```
## 查看光模块收发光
```
show interfaces diagnostics optics xe-0/2/2
```



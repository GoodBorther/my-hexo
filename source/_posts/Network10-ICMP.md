---
title: Network10-ICMP
date: 2024-05-14 22:27:43
tags:
- network
- 基础知识
categories: 数据通信
cover: https://tuchuang-1258743955.cos.ap-beijing.myqcloud.com/image/20240514222818.png
---
## ICMP简介
- **互联网控制消息协议**（英语：**I**nternet **C**ontrol **M**essage **P**rotocol，缩写：**ICMP**），在 [RFC 792](https://tools.ietf.org/html/rfc792) 中定义。
- ICMP协议的主要作用，是检测互联网的各种故障，提供可能发生在通信环境中的各种问题反馈。通过这些信息，使管理者可以对所发生的问题作出诊断，然后采取适当的措施解决。
- ICMP是基于IP协议的，并不依赖于传输层协议TCP或者UDP，可以直接工作在三层。
- Ping和traceroute都是基于ICMP实现的网络程序。
## ICMP的报文结构
### 报头
ICMP报头从IP报头的第160位开始（IP首部20字节）（除非使用了IP报头的可选部分）。
![image.png](https://tuchuang-1258743955.cos.ap-beijing.myqcloud.com/image/20240514221709.png)
- **Type** - ICMP的类型,标识生成的错误报文；
- **Code** - 进一步划分ICMP的类型,该字段用来查找产生错误的原因.；例如，ICMP的目标不可达类型可以把这个位设为1至15等来表示不同的意思。
- **Checksum** - Internet校验和（[RFC 1071](https://tools.ietf.org/html/rfc1071)），用于进行错误检查，该校验和是从ICMP头和以该字段替换为0的数据计算得出的。
- **Rest of Header** - 报头的其余部分，四字节字段，内容根据ICMP类型和代码而有所不同。
### 填充数据

{% note color:cyan 这个是重点哦！ %}

填充的数据紧接在ICMP报头的后面（以8位为一组）：
- [Linux](https://zh.wikipedia.org/wiki/Linux "Linux")的"ping"工具填充的ICMP除了8个8位组的报头以外，默认情况下还另外填充数据使得总大小为64字节。
- [Windows](https://zh.wikipedia.org/wiki/Microsoft_Windows "Microsoft Windows")的"ping.exe"填充的ICMP除了8个8位组的报头以外，默认情况下还另外填充数据使得总大小为40字节。

|类型|代码|状态|描述|查询|差错|
|---|---|---|---|---|---|
|0 - [响应回显](https://zh.wikipedia.org/wiki/Ping "Ping")|0||Echo响应 (被程序[ping](https://zh.wikipedia.org/wiki/Ping "Ping")使用）|●||
|1 and 2||未分配|保留||●|
|3 - 目的不可达|0||目标网络不可达||●|
|1||目标主机不可达||●|
|2||目标协议不可达||●|
|3||目标端口不可达||●|
|4||要求分段并设置[DF flag](https://zh.wikipedia.org/wiki/IPv4#%E6%8A%A5%E6%96%87%E7%BB%93%E6%9E%84 "IPv4")标志||●|
|5||源路由失败||●|
|6||未知的目标网络||●|
|7||未知的目标主机||●|
|8||源主机隔离（作废不用）||●|
|9||禁止访问的网络||●|
|10||禁止访问的主机||●|
|11||对特定的TOS 网络不可达||●|
|12||对特定的TOS 主机不可达||●|
|13||由于过滤 网络流量被禁止||●|
|14||主机越权||●|
|15||优先权终止生效||●|
|4 - 源端关闭|0|弃用|源端关闭（拥塞控制）||●|
|5 - 重定向|0||重定向网络||●|
|1||重定向主机||●|
|2||基于TOS 的网络重定向||●|
|3||基于TOS 的主机重定向||●|
|6||弃用|备用主机地址|||
|7||未分配|保留|||
|8 - [请求回显](https://zh.wikipedia.org/wiki/Ping "Ping")|0||Echo请求|●||
|9 - 路由器通告|0||路由通告|●||
|10 - 路由器请求|0||路由器的发现/选择/请求|●||
|11 - ICMP 超时|0||TTL 超时||●|
|1||分片重组超时||●|
|12 - 参数问题：错误IP头部|0||IP 报首部参数错误||●|
|1||丢失必要选项||●|
|2||不支持的长度|||
|13 - 时间戳请求|0||时间戳请求|●||
|14 - 时间戳应答|0||时间戳应答|●||
|15 - 信息请求|0|弃用|信息请求|●||
|16 - 信息应答|0|弃用|信息应答|●||
|17 - 地址掩码请求|0|弃用|地址掩码请求|●||
|18 - 地址掩码应答|0|弃用|地址掩码应答|●||
|19||保留|因安全原因保留|||
|20 至 29||保留|_Reserved_ for robustness experiment|||
|30 - Traceroute|0|弃用|信息请求|||
|31||弃用|数据报转换出错|||
|32||弃用|手机网络重定向|||
|33||弃用|[Where-Are-You](https://zh.wikipedia.org/w/index.php?title=Where-Are-You&action=edit&redlink=1 "Where-Are-You（页面不存在）")（originally meant for [IPv6](https://zh.wikipedia.org/wiki/IPv6 "IPv6")）|||
|34||弃用|[Here-I-Am](https://zh.wikipedia.org/w/index.php?title=Where-Are-You&action=edit&redlink=1 "Where-Are-You（页面不存在）")（originally meant for IPv6）|||
|35||弃用|Mobile Registration Request|||
|36||弃用|Mobile Registration Reply|||
|37||弃用|Domain Name Request|||
|38||弃用|Domain Name Reply|||
|39||弃用|SKIP Algorithm Discovery Protocol, [Simple Key-Management for Internet Protocol](https://zh.wikipedia.org/w/index.php?title=Simple_Key-Management_for_Internet_Protocol&action=edit&redlink=1 "Simple Key-Management for Internet Protocol（页面不存在）")|||
|40|||[Photuris](https://zh.wikipedia.org/w/index.php?title=Photuris_(protocol)&action=edit&redlink=1 "Photuris (protocol)（页面不存在）"), Security failures|||
|41||实验性的|ICMP for experimental mobility protocols such as [Seamoby](https://zh.wikipedia.org/w/index.php?title=Seamoby&action=edit&redlink=1 "Seamoby（页面不存在）") [RFC4065]|||
|42 到 255||保留|保留|||
|235||实验性的|RFC3692（ [RFC 4727](https://tools.ietf.org/html/rfc4727 "rfc:4727")）|||
|254||实验性的|RFC3692（ [RFC 4727](https://tools.ietf.org/html/rfc4727 "rfc:4727")）|||
|255||保留|保留|||

## 一些ICMP的基础功能
### Icmp重定向（默认开启）

+ 当路由不是最优路径时，可以帮助主机选路，但不建议开启此功能
	+ 容易被人利用实现中间人攻击
	+ 占用主机资源，因为此功能，只能生成基于主机地址的路由
- 要求
	- 不是路由转发，网关和主机必须在同一网段
	- 必须是单播报文

### icmp差错检测

+ ping程序实现，两种报文，其实就是返回咱们上文提到的ICMP的填充数据，帮助我们判断网络中的故障
	+ request
	+ reply

### icmp错误报告

+ tracert来实现，可以检测路由环路（不被限制的情况下）机制如下
	+ 连续发送三次ttl=1的报文，如果距离目标地址还有一跳，那么还会发送ttl=2的报文，以此类推
	+ 使用udp协议封装，随机使用40000以上的端口号
## 参考
[互联网控制消息协议 - 维基百科，自由的百科全书 (wikipedia.org)](https://zh.wikipedia.org/wiki/%E4%BA%92%E8%81%94%E7%BD%91%E6%8E%A7%E5%88%B6%E6%B6%88%E6%81%AF%E5%8D%8F%E8%AE%AE)

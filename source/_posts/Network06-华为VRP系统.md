---
title: Network06-华为VRP系统
date: 2024-05-09 21:38:13
tags:
- network
- 基础知识
categories: 数据通信
cover: https://tuchuang-1258743955.cos.ap-beijing.myqcloud.com/image/20240509213905.png
---
## 前言
在之后的学习中，我们都将使用华为的设备进行网络实验，笔者当前也是在考取华为的DatacomIE证书，不过到现在还没考笔试。。。。懒矣。
## 简介
- 通用路由平台VRP（Versat Routing Platform）是华为公司数据通信产品的通用操作系统平台。它以IP业务为核心，采用组件化的体系结构，在实现丰富功能特性的同时，还提供了基于应用的可裁剪和可扩展的功能，使得路由器和交换机的运行效率大大增加。熟悉VRP操作系统并且熟练掌握VRP配置是高效管理华为网络设备的必备基础​
## VRP系统功能
- 实现统一的用户界面和管理界面
- 实现控制平面功能，并定义转发平面接口规范
- 实现各产品转发平面与VRP控制平面之间的交互
- 屏蔽各产品链路层对于网络层的差异
## VRP的发展
![](https://tuchuang-1258743955.cos.ap-beijing.myqcloud.com/image/Pasted%20image%2020211219153845.png)
## 文件系统简介
- 文件系统是指对存储器中文件、目录的管理，功能包括查看、创建、重命名和删除目录，拷贝、移动、重命名和删除文件等​
- 掌握文件系统的基本操作，对于网络工程师高效管理设备的配置文件和VRP系统文件至关重要​
## 存储设备简介
- SDRAM： 
	- 同步动态随机存储器是系统运行内存，相当于电脑的内存​
- Flash：
	- 属于非易失存储器，断电后，不会丢失数据。主要存放系统软件，配置文件等；补丁文件和PAF文件由维护文件上传，一般存储与flash或SD Card​
- NVRAM： 
	- 属于非易失存储器，断电后，不会丢失数据。主要存放系统软件，配置文件等；补丁文件和PAF文件由维护文件上传，一般存储与flash或SD Card​
 - SDCard：
	 - 断电后，不会丢失数据，存储容量较大，一般出现在主控板上，可以存放系统文件，配置文件，日志等
 - USB: 
	 - USB是接口，用于外接大容量存储设备，主要用于设备升级，传输数据​
## 设备初始化过程
- 设备上电后，首先运行BootROM软件，初始化硬件并显示设备的硬件参数，然后运行系统软件，最后从默认存储路径中读取配置文件进行设备的初始化操作​
## 设备管理方式
- web管理
	- web网关方式通过图化的操作界面，实现对设备直观方便地进行管理和维护，但是此方式仅口可以实现对设备部分操作​
	- 使用https协议​

 - Cli方式
	 - 通过命令行登入设备
	 - 使用console或者网关口
	 - 使用telnet或者ssh协议
## VRP用户界面
- console用户界面： 
	- Console接口是典型的配置接口。使用Console线直接连接至计算机的串口，利用终端仿真程序（一般使用Windows自带的“超级终端”）在本地配置路由器。 路由器的Console接口多为RJ-45接口，并标记有CONSOLE字样​
- Vty用户界面：
	- VTY（Virtual Type Terminal） 虚拟终端链接，一种网络设备的连接方式，一台网络设备可以有多个VTY虚拟线路（不同设备支持VTY数量不等），每个线路可配置不同的协议，如配置Telnet、SSH等，每个线路也可设置不同的访问网络设备的权限​
## VRP用户级别
- ​VRP提供基本的权限控制，可以实现不同级别的用户执行不同的级别的命令，用以限制不同用户对设备的操作

| 用户等级 | 命令等级 | 名称 | 说明 |
| --- | --- | --- | --- |
|0|0|参观级别|可使用网络诊断工具（ping，tracert）、从本设备出发访问外部设备的命令（telnet客户端命令）、部分display命令​|
|1|0 and 1|监控级别|用于系统维护，可以使用display等命令|
|2|0,1 and 2|配置级|可使用业务配置命令，包括路由、各个网络层次的命令，向用户直接提供网络服务|
|3-15|0,1,2 and 3|管理级|可使用系统基本运行的命令，对业务提供支撑作用，包括文件系统、FTP、TFTP下载、命令级别设置命令以及用于业故障诊断的debugging命令等|
## 命令结构
![](https://tuchuang-1258743955.cos.ap-beijing.myqcloud.com/image/Pasted%20image%2020211219154403.png)
## 命令行视图
![](https://tuchuang-1258743955.cos.ap-beijing.myqcloud.com/image/Pasted%20image%2020211219154416.png)
## 常用命令

### undo命令行

- 在命令前加undo关键字，即为undo命令行​

### 常见文件系统操作命令

- 查看当前目录 `pwd`

- 显示当前目录下的文件信息 `dir`

- 查看文本文件的具体内容 `more`

- 修改用户当前界面的工作目录 `cd`

- 创建新的目录 `mkdir`

- 删除空目录 `rmdir`

- 复制文件 `copy`

- 移动文件 `move`

- 重命名文件 `rename`

- 删除文件 `delete`

- 恢复删除的文件  `undelete`

- 彻底删除回收站的文件  `reset recycle-bin` ​

### 基本配置命令

- 配置设备名称  `sysname naem`

- 配置用户登录后超时时间（默认时间为5分钟

- 路由器支持配置超时时间。具体操作步骤如下：

- 在用户界面视图下  

- 执行命令`idle-timeout minutes [ seconds ]`来设置用户界面断连的超时时间。  

- 如果配置了AAA认证，执行完命令idle-timeout后，还须在AAA视图下再执行local-user user-name idle-timeout minutes [seconds ]命令来使能用户闲置断连功能，才能使得登录到该VTY界面的用户在没做任何操作的情况下达到超时时间后自动断开连接。通常情下，推荐设置用户界面断连的超时时间在10～15分钟之间。  

>说明：  
>设置执行命令idle-timeout 0，其中0即关闭VTY用户界面的超时断连功能。  
>如果VTY用户界面没有设置闲置断连功能，则有可能导致其他用户无法获得空闲连接。


- 设置系统时钟  `clock timezone time-zone-name{add|minus}`  
- 用来对本地时区信息进行设置  `clock datetime [utc] HH:MM:SS YYYY-MM-DD`
- 用来设置设备当前或UTC日期和时间  `clock daylight-saving-time`
- 配置命令等级  `command-privilege level LEVEL view VIEW-NAME COMMAND-KEY`
-  配置用户通过password方式登录设备:​

```

user-interface vty 0 4

set authentication password cipher information

```

- 配置接口地址:

```

  interface INT

  ip add IPADDR NETMASK

```

- 查看当前运行的配置文件 `display cu`

- 配置文件保存  `save`

- 查看保存的配置  `display save-configur` ​
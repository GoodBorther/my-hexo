---
title: Linux系统安装
date: 2024-05-29 23:39:11
tags:
    - Linux
    - Centos
categories: Devops
---
## Linux系统安装

### 准备环境：

* Vmware16
* Centos7.x镜像

### 创建虚拟机：

* 创建新的虚拟机

![](https://tuchuang-1258743955.cos.ap-beijing.myqcloud.com/image/20220604180113.png)

* 一路下一步，点击稍后安装操作系统

![](https://tuchuang-1258743955.cos.ap-beijing.myqcloud.com/image/20220604180132.png)

* 选择我们要安装的操作系统

![](https://tuchuang-1258743955.cos.ap-beijing.myqcloud.com/image/20221111123346.png)

* 选择虚拟硬盘存放的路径

![](https://tuchuang-1258743955.cos.ap-beijing.myqcloud.com/image/20221111123404.png)

* 处理器数量，根据自己本机已经需要来选

![](https://tuchuang-1258743955.cos.ap-beijing.myqcloud.com/image/20221111123419.png)

* 内存大小，根据需要来即可

![](https://tuchuang-1258743955.cos.ap-beijing.myqcloud.com/image/20221111123441.png)


* 使用网络地址转换或者桥接模式都可以（选择网络地址转换的话，后续外部网络发送变化，不影响虚拟机的ip地址）：

![](https://tuchuang-1258743955.cos.ap-beijing.myqcloud.com/image/20220604180241.png)

* 一路下一步即可创建完成
* 将我们的iso挂载上，之后点击开启虚拟机即可进入系统安装界面

![](https://tuchuang-1258743955.cos.ap-beijing.myqcloud.com/image/20220604180303.png)

### 安装系统

* 选择第一项，第二项会扫描整个光盘，时间花费较长：

![](https://tuchuang-1258743955.cos.ap-beijing.myqcloud.com/image/20220604180439.png)

* 选择语言，点击继续即可：

![](https://tuchuang-1258743955.cos.ap-beijing.myqcloud.com/image/20220604180501.png)

* 我们需要配置时区和安装所使用的磁盘：

![](https://tuchuang-1258743955.cos.ap-beijing.myqcloud.com/image/20220604180514.png)

* 时区：

![](https://tuchuang-1258743955.cos.ap-beijing.myqcloud.com/image/20220604180531.png)

* 安装磁盘选择，直接点击done即可：

![](https://tuchuang-1258743955.cos.ap-beijing.myqcloud.com/image/20220604180545.png)

* 配置网络，按照下图操作即可：

![](https://tuchuang-1258743955.cos.ap-beijing.myqcloud.com/image/20220604180557.png)

![](https://tuchuang-1258743955.cos.ap-beijing.myqcloud.com/image/20220604180610.png)

* 点击开始安装即可
* 我们需要设置个root密码，自己来设置即可：

![](https://tuchuang-1258743955.cos.ap-beijing.myqcloud.com/image/20220604180625.png)

![](https://tuchuang-1258743955.cos.ap-beijing.myqcloud.com/image/20220604180638.png)

* 点击done后等待片刻后，点击右下角reboot进行重启，重启之后系统正常应为如下界面：

![](https://tuchuang-1258743955.cos.ap-beijing.myqcloud.com/image/20220604180650.png)

输入账户root，以及密码即可进入系统

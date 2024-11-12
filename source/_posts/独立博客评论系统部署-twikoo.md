---
title: 独立博客评论系统部署-twikoo
date: 2024-11-04 17:17:28
tags: blog
categories: Think
---
网站评论系统twikoo部署在自己的博客中~
<!-- more -->
## Twikoo简介
- 一个简洁、安全、免费的网站评论系统
- 官网：https://twikoo.js.org/
## 如何把Twikoo部署在自己的Hexo博客上？
1. 安装Twikoo到自己的服务器上
> 作者采用Docker安装
```
mkdir -pv /twikoo/data
docker run --name twikoo -e TWIKOO_THROTTLE=1000 -p 8080:8080 -v /twikoo/data:/app/data -d docker.wanpeng.top/imaegoo/twikoo
```
2. 配置Nginx https反代，在https的server配置段里面加一个location就可以了，非常简单~
```
    location /twikoo/ {
        proxy_pass http://81.70.102.187:8080;
```

3. 修改自己hexo主题的配置文件，作者的修改如下
```
comments:
  service: twikoo
  twikoo:
    envId: https://www.mengge.site/twikoo/ # vercel函数
```
4. 大功告成

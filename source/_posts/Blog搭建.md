---
title: Blog搭建
date: 2024-01-01 14:30:41
tags: blog
categories: Think
---
## 方案
- Hexo + 腾讯云 云开发托管静态网站 + CDN加速
## 环境准备
- Git
	- 下载链接：[Git for Windows](https://gitforwindows.org/)
- Scoop：windows上的包管理器
	- 下载链接：[ScoopInstaller/Scoop: A command-line installer for Windows. (github.com)](https://github.com/ScoopInstaller/Scoop)
- 下载完上面的两个工具之后再使用Scoop来下载nodejs
	- Nodejs：`scoop install nodejs`
- 使用npm来下载hexo
	- Hexo：`npm i -g @cloudbase/cli hexo-cli`
	- 搜索插件: `npm i -S hexo-generator-json-content`
## 初始化Hexo
> 参考：[云开发 CloudBase 搭建 Hexo-示例教程-文档中心-腾讯云 (tencent.com)](https://cloud.tencent.com/document/product/876/47006)
1. 在本地新建一个文件夹，比如blog，再在此目录下执行`hexo init` 来初始化，执行`hexo s`可以打开本地服务，在浏览器输入localhost:4000，可以看到自己部署的博客
2. 创建云开发环境（**腾讯云对于新用户免费使用一个月**），参考官方文档即可，写的很详细

搭建完成后，通过云开发的默认域名就可以访问了
![image.png](https://tuchuang-1258743955.cos.ap-beijing.myqcloud.com/image/20240101141246.png)

## 将域名指定为自己申请的域名
> 因为笔者自己在腾讯云注册了域名和证书，而且默认域名太复杂，所以需要这一步
1. 添加自定义域名，**注意这里的CName是自动生成的哦，我们下面会用到**
![image.png](https://tuchuang-1258743955.cos.ap-beijing.myqcloud.com/image/20240101141815.png)

2.  在自己的域名解析中添加两条Cname记录即可，记录值上图中的cname域名
![image.png](https://tuchuang-1258743955.cos.ap-beijing.myqcloud.com/image/20240101141643.png)

3. 静等30分钟域名生效之后就可以使用自己的域名来访问了，自带CDN加速
## Hexo优化
### 主题
这里笔者使用的主题是[Volantis](https://volantis.js.org/)，下载安装按照官方文档即可，下载完成后，在博客工作目录下新建一个文件_config.volantis.yml
### 页脚加入ICP备案信息
编辑`_config.volantis.yml` ，内容如下图所示
![image.png](https://tuchuang-1258743955.cos.ap-beijing.myqcloud.com/image/20240101142439.png)
### 自定义封面和个人信息
具体可以参考官方文档[开始使用 - Volantis](https://volantis.js.org/v6/getting-started/)，图片可以直接使用网上的url，本地的图片如何引入，还没研究
![image.png](https://tuchuang-1258743955.cos.ap-beijing.myqcloud.com/image/20240101142608.png)
在工作目录的`_config.yml`文件中，可以定义自己的信息
![image.png](https://tuchuang-1258743955.cos.ap-beijing.myqcloud.com/image/20240101142657.png)
### 更改网站字体
参考
- [在 Hexo Fluid 主题中使用霞鹜文楷 - 竹林里有冰的博客 (zhul.in)](https://zhul.in/2023/11/28/use-lxgw-wenkai-in-hexo-fluid/)
做完上面的操作之后，可以使用`hexo s`在本地验证下是否有问题，没有问题的话，使用`cloudbase hosting deploy public -e ENVID`发布到腾讯云上

### 增加评论系统
这里采用的方案是twikoo
1. 使用腾讯云云函数来搭建，参考[Hexo添加Twikoo评论插件-腾讯云开发者社区-腾讯云 (tencent.com)](https://cloud.tencent.com/developer/article/2063344)
2. 配置好之后，在主题的配置文件中调用一下就可以了

## 一些常用的操作
### 新增文章
```
hexo new [layout] title`或 `hexo n [layout] title
```
### 新增分类
参考
- [Hexo+Github博客教程：03添加分类 - 知乎 (zhihu.com)](https://zhuanlan.zhihu.com/p/50787870#:~:text=Hexo%2BGithub%E5%8D%9A%E5%AE%A2%E6%95%99%E7%A8%8B%EF%BC%9A03%E6%B7%BB%E5%8A%A0%E5%88%86%E7%B1%BB%201%201%E3%80%81%E5%88%9B%E5%BB%BA%E2%80%9C%E5%88%86%E7%B1%BB%E2%80%9D%E9%80%89%E9%A1%B9%202%201.1%20%E7%94%9F%E6%88%90%E2%80%9C%E5%88%86%E7%B1%BB%E2%80%9D%E9%A1%B5%E5%B9%B6%E6%B7%BB%E5%8A%A0tpye%E5%B1%9E%E6%80%A7%20%E6%89%93%E5%BC%80%E5%91%BD%E4%BB%A4%E8%A1%8C%EF%BC%8C%E8%BF%9B%E5%85%A5%E5%8D%9A%E5%AE%A2%E6%89%80%E5%9C%A8%E6%96%87%E4%BB%B6%E5%A4%B9%E3%80%82%20%E6%89%A7%E8%A1%8C%E5%91%BD%E4%BB%A4,-%20jQuery%20-%20%E8%A1%A8%E6%A0%BC%20-%20%E8%A1%A8%E5%8D%95%E9%AA%8C%E8%AF%81%20%E5%B0%B1%E6%98%AF%E8%BF%99%E7%AF%87%E6%96%87%E7%AB%A0%E7%9A%84%E6%A0%87%E7%AD%BE%E4%BA%86%20)

```
hexo new page categories
```

### 新增标签
```
hexo new page tags
```

### 发布
```
cloudbase hosting deploy public -e ENV_ID
```
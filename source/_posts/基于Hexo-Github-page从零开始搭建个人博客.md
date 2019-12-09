---
title: "基于Hexo+Github page从零开始搭建个人博客"
catalog: true
date: 2019-012-9 11:38:24
subtitle: ""
header-img: "Demo.png"
tags:
- Hexo
catagories:
- 博客搭建
---


本文介绍一种基于Hexo框架和Github Page服务搭建博客的方法，其中还将原始的Github page指向了自有域名
本章结构
* [github安装与配置](#github)
* [自有域名购买与配置](#domain)
* [Hexo框架使用](#hexo)

<div id="github"></div>  

# github配置
---
安装git,ssh
``` bash
$ sudo apt-get install git
$ sudo apt-get install ssh
```
配置git用户名和邮箱
``` bash
$ git config --global user.name "your name"
$ git config--global user.email "your email"
```
配置SSH，参考 ，如下：
创建SSH key
``` bash
$ ssh-keygen -t rsa -b 4096 -C "your email"
```
将~/.ssh/id_rsa.pub内容添加到Github-Setting-SSH keys
验证github SSH连接
``` bash
$ ssh -T git@github.com
```
more Info [Github SSH文档](https://help.github.com/en/github/authenticating-to-github/connecting-to-github-with-ssh).

配置完Github后，创建Github库xxx.github.io，其中Github Page库的名称必须为xx.github.io

<div id="domain"></div>  

# 使用自有域名
---
1. 万网或godaddy购买域名，由于国内备案麻烦谨慎购买.cn域名，除.com常见的域名有.cc，.me，.top，.club等。
2. 万网域名自动使用hichina云解析(若使用godaddy，为了加快解析速度，防止被墙，可将 DNS 服务器可设置为为DNSPod)
3. 添加两条解析，使得访问“xxx.com”和“www.xxx.com”时均能够解析域名
	主机记录@，记录类型CNAME，解析线路默认，记录值littlepeanut.github.io.，TTL1小时；主机记录www，记录类型A，解析线路默认，记录值185.199.109.153，TTL1小时(主机IP可直接ping域名得到)
4. 今后可根据需要添加二级域名
5. 在博客中source和public中添加CNAME，记录域名littlepeanut.top，将本地和Github Page上的Hexo绑定到自有域名
6. 若在Github Page中使用自有域名，一般在一天后才能在库的Setting中开启HTTPS

<div id="hexo"></div>  

# Hexo使用
---
Hexo是一个常见的静态博客框架.  
首先参考 [Hexo中文文档](https://hexo.io/zh-cn/docs/)安装Hexo，注意文档中提到在安装时尽量不要使用sudo  
安装npm和Node.js，验证
``` bash
$ node -v 
$ npm -v
$ npx -v
```
下面初始化博客
``` bash
$ hexo init myblog
```
安装git管理扩展
``` bash
$ npm i hexo-deployer-git
```
新建文章，存放在~/source/_posts
``` bash
$ hexo n "article title" == hexo new "article title"
```
生成静态网页
``` bash
$ hexo g
```
启动服务，可本地预览
``` bash
$ hexo s
```
部署
``` bash
hexo d
```
修改配置文件_config.yml，首先更改url和root，若不修改，即使在本地服务网页能够正常显示，部署后仍无法识别css等文件，具体修改规则如下：  
如果域名是https://xxx.com：
```
url: https://xxx.com/
root: /  
```
如果域名是https://xxx.com/blog：
```
url: https://xxx.com/blog
root: /blog  
```
如果域名是https://yourID.github.io/blog：
```
url: https://yourID.github.io/blog
root: /blog  
```
如果域名是https://yourID.github.io：
```
url: https://yourID.github.io/blog
root: /yourID.github.io/  
```
同样还须在_config.yml中设置部署的git库地址  
```
deploy:
  type: git
  repo: https://github.com/feiwww/littlepeanut.github.io.git
  branch: master
```
其他常用Hexo命令
``` bash
$ npm update hexo -g #升级 
$ hexo server #Hexo会监视文件变动并自动更新，无须重启服务器
$ hexo server -s #静态模式
$ hexo server -p 5000 #更改端口
$ hexo server -i 192.168.1.1 #自定义 IP
$ hexo clean #清除缓存，若是网页正常情况下可以忽略这条命令
```
在一般情况下更新博客时，重新生成下静态博客并部署到Github上即可，使用的操作如下：
``` bash
$ hexo clean
$ hexo g
$ hexo d
```
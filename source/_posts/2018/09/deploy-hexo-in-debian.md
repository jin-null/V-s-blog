---
title: 在debian上部署hexo
tag: 
  - hexo 
  - debian
---
接触了一下Hexo，大概是比laravel 博客要简单了很多很多很多。
作为个人博客只需要配置就行了，简单的业务逻辑根本用不上laravel（顺便数据库也省了）
于是打算直接部署上腾讯云的机器上。

## 云服务器配置

### 腾讯云服务器
趁着活动买了2年的腾讯云服务，但是基本上是闲置，无聊的时候就重装下系统
<!--more-->
配置信息：
> 操作系统:	Debian 9.0 64位

> CPU:	1 核

>内存:	2 GB

>公网带宽: 	1 Mbps



### github配置

github上新建一个空的项目V`s blog

然后git到本地
``` bash
$ git clone https://github.com/jin-null/V-s-blog.git
```
将本地的Hexo文件剪切到空的V-s-blog

``` bash
$ cd V-s-blog
```

生成静态文件 

``` bash
$ hexo generate

可以简写

$ hexo g
```


然后push到github

``` bash
$ git push
```
### 部署到云服务器

``` bash
$ cd /home/web
$ git clone https://github.com/jin-null/V-s-blog.git
```


### 配置nginx

安装nginx
``` bash
$ apt-get update 
$ apt-get upgrade 
$ apt-get install nginx  
$ service nginx start  
```
配置 

```bash
$ cd /etc/nginx/sites-enabled 
```
修改default文件

```bash
 ...
    root /home/web/V-s-blog/public;
    
    server_name www.yununo.com; 
 ...
```
保存修改后重启nginx
```bash
$ service nginx restart
```
访问 [](http://)www.yunuuo.com

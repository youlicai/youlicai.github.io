---
title: Github搭建个人博客（一）
date: 2018-08-02 11:44:54
categories: 
- 其他
tags: 
- Github
---

## 一、环境准备 ##
### 安装Node.js ###
[https://nodejs.org/en/download/current/](https://nodejs.org/en/download/current/)<br/>
安装过程不介绍了，安装完成后，打开cmd

![](https://i.imgur.com/kJrRDJd.png)

表示安装成功，否则检查下node.js的环境变量是否正确。
### 安装Git ###

[https://git-scm.com/download/win](https://git-scm.com/download/win)


## 二、Github配置 ##

### 创建一个项目 ###
![](https://i.imgur.com/swEXRyn.png)

 **Repository格式：用户名.github.io。**

### 生成添加秘钥 ###
$ ssh-keygen -t rsa -C "Github的注册邮箱地址"

一路Enter过来就好，待秘钥生成完毕，会得到两个文件id_rsa和id_rsa.pub，用带格式的记事本打开id_rsa.pub，Ctrl + a复制里面的所有内容，然后进入

![](https://i.imgur.com/ATlSq5P.png)

将复制的内容粘贴到Key的输入框，随便写好Title里面的内容，点击Add SSH key按钮即可。

创建完成后先放一边，后面会用到。


## 三、Hexo 使用 ##

命令在gitbash中使用

![](https://i.imgur.com/FPYmUKx.png)
### 安装 HEXO ###
$ npm install hexo -g

![](https://i.imgur.com/drZM5nu.png)

安装完成后 hexo -v 

![](https://i.imgur.com/WaLAlzg.png)

安装成功！
### 初始化框架文件 ###
$ hexo init 

![](https://i.imgur.com/32CXCNo.png)

看到 **Start blogging with Hexo！** 结尾，表示初始化成功。
### 生成网站静态文件到默认设置的 public 文件夹 ###
$ hexo g

![](https://i.imgur.com/hpsld9n.png)
### 启动本地服务器 ###
$ hexo s

![](https://i.imgur.com/WaZzCPJ.png)

现在可以本地测试了。
![](https://i.imgur.com/b63cMum.png)


## 四、关联Hexo和Github ##

### 找到_config.yml文件，修改如下： ###
<pre>
deploy:
  type: git
  repo: https://github.com/youlicai/youlica.github.com.git
  branch: master
</pre>

### $ hexo d 发布 ###
可能会出现以下几个情况：

1.

![](https://i.imgur.com/7Kt6F1L.png)

解决方案：执行如下语句后， 再部署即可：
$ npm install hexo-deployer-git --save

2.

![](https://i.imgur.com/MTuHpER.png)

可将_config.yml中的repo修改为如下标准格式：
<pre>
deploy:
  type: git
  repo: https://用户名:密码@github.com/youlicai/youlica.github.com.git
  branch: master
</pre>


![](https://i.imgur.com/8DSK27X.png)

**发布成功！**
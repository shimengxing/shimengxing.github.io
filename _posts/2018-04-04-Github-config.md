---
layout: post
title: MAC配置多个Github账号
date: 2018-04-04 
tags: Github
categories: 工具
---
## 场景

> 使用github的时候，需要给该账号添加一个SSH key才能访问。但是如果想在一台机器使用多个github账号（比如个人账号和工作账号）。这个时候怎么指定push到哪个账号的仓库上去呢？

## 解决方法
    
假设已经拥有工作账号且已经使用正常，现在想添加另一个个人使用账号。

### 1.为工作账号生成SSH Key

```
#在.ssh目录下执行
$ ssh-keygen -t rsa -C "your-email-address"
```
存储key的时候，不要覆盖现有的id_rsa，使用一个新的名字，比如id_rsa_personal

```
#当执行上述命令出现，就可以在冒号后面输入新的名字：id_rsa_personal
Enter file in which to save the key (/Users/xxx/.ssh/id_rsa): 
```
### 2.在personal github中添加ssh

```
#登录github，选择Settings-->SSH and GPG Keys 添加ssh
#title:任意输入
#key：打开你生成的id_rsa_personal.pub文件，将其中所有的内容拷贝至此
```
### 3.添加并识别新的SSH keys私钥

```
#因为默认只读取id_rsa，为了让SSH识别新的私钥，需将其添加到SSH agent中
$ ssh-agent bash
$ ssh-add ~/.ssh/id_rsa_personal
```

### 4.修改~/.ssh/config以添加多个ssh配置

```
#如果.ssh目录下没有config文件，则新建config文件
$ vi config

#加上以下内容
#该文件用于配置私钥对应的服务器
#default github 
Host github.com
  HostName github.com
  IdentityFile ~/.ssh/id_rsa

Host personal.github.com
  HostName github.com
  IdentityFile ~/.ssh/id_rsa_personal
  
#Hose:别名
#HostName:git托管的平台url。
```
### 5.验证连接

```
#ssh -T git@别名
$ ssh -T git@personal.github.com
Hi xxx! You've successfully authenticated, but GitHub does not provide shell access.
```
如果不能连接不成功的话

```
#查看出错信息
$ ssh -vT git@personal.github.com
```
### 6.应用
对于personal下的项目，假设其ssh链接为：

```
$ git@github.com:xxx/xxx.git
```

如果我们直接执行

```
$ git clone git@github.com:xxx/xxx.git
```
git会根据~/.ssh/config配置下的Host去寻找授权信息，比如当前链接的Host为personal.github.com，那么git就会使用~/.ssh/id_rsa_personal进行授权.

由于这是personal项目，授权就会失败。正确做法，把项目链接

```
$ git@github.com:xxx/xxx.git
```
修改成

```   
$ git@personal.github.com:xxx/xxx.git
```
即可。现在git就会根据personal.github.com找到~/.ssh/id_rsa_personal，并使用其作为授权文件。

### Q&A
如果此时直接使用新添加的personal账号，发现会报错，原因是这台设备现在还是使用着原先的工作帐号，需要切换账号

*控制台 cd ..会到根目录，vi .gitconfig文件，修改里面的Name和Email*

或者使用命令

```
$ git config --global user.name "你的昵称"
$ git config --global user.email "你的邮箱地址"
```
此时，就可以愉快的使用两个账号了。


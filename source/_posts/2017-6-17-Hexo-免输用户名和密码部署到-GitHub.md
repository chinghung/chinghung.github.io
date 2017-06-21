---
layout: 设置
title: Hexo 免输用户名和密码部署到 GitHub
date: 2017-06-17 15:41:17
categories: 博客搭建
tags: [Hexo，部署，免密码]
---
## 通过设置 SSH 使 Hexo 部署时免输入用户名和密码
每次写完文章，使用 <code>hexo deploy</code> 命令部署 Hexo 到 GitHub 的时候，都要输入用户名和密码，这个过程显得非常繁琐，特别是在用户名和密码比较长的时候。<!--more-->通过网上的一些教程发现，可以使用 SSH 密钥取代默认 https 的认证方式，达到跳过输入用户名和密码的效果。

## 生成 SSH 密钥
首先快捷键 <code>Ctrl + Alt + T</code> 打开终端，输入以下命令进入 <code>.ssh</code> 目录
```
$ cd ~/.ssh
# Checks to see if there is a directory named ".ssh" in your user directory
```
使用 <code>ssh-keygen</code> 生成密钥
```
$ ssh-keygen -t rsa -C "your_email@example.com"
# Creates a new ssh key using the provided email
Generating public/private rsa key pair.
Enter file in which to save the key (/home/user/.ssh/id_rsa):
```
直接 enter 跳过，使用默认的文件名，接下来显示
```
Enter passphrase (empty for no passphrase): [Type a passphrase]
Enter same passphrase again: [Type passphrase again]
```
按照提示输入密码，  
密钥生成成功之后会显示以下内容
```
Your identification has been saved in /home/user/.ssh/id_rsa.
Your public key has been saved in /home/user/.ssh/id_rsa.pub.
The key fingerprint is:
…………………此处是密钥内容…………………… your_email@example.com
```

## 设置 GitHub 中的 SSH keys
这里可以选择在用户设置中添加密钥（Settings –> SSH GPG keys）或者往单个项目中添加密钥。这里以用户设置中添加 SSH 密钥为例。  

- 向用户设置中添加：打开 github 首页，点击右上角的用户头像，选择 <code>Settings</code> 左边找到 <code>SSH and GPG keys</code> ，选择 <code>New SSH key</code> ,将 <code>.ssh</code> 目录下 <code>id_rsa.pub</code> 文件里的全部内容复制进去，点击 <code>Add SSH key</code> 完成密钥的添加。
- 向单个项目中添加：打开 <code>username.github.io</code> 的 <code>repository</code> ，在菜单中选择 <code>Settings</code> -> <code>Deploy keys</code> -> <code>Add deploy key</code> ，后面的步骤和上面一样。  

输入以下命令测试是否成功
```
$ ssh -T git@github.com
```
如果出现以下内容则表示配置成功
```
Hi username! You've successfully authenticated, but GitHub does not provide shell access.
```

## 把 Hexo 的认证方式改为 SSH
打开 <code>Hexo</code> 的配置文件 <code>_config.yml</code> ，把 <code>deploy</code> 下的 <code>repo</code> 改为 
```
repo: git@github.com:username/username.github.io
```
最后，在终端执行命令 <code>$ hexo clean && hexo g -d</code> ，检验是否成功。  

😃😃😃



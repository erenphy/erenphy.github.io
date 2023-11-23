---
layout:     post
title:      vscode 远程SSH开发
subtitle:   vscode配置远程访问服务器
date:       2023-11-16
author:     汤汤
header-img: img/post-bg-ios9-web.jpg
catalog: true
tags:
    - 编辑器
    - SSH
    - github
---

> 三番四次回顾，每次换新的主机都要重新鼓捣一遍

## vscode 安装插件
###### remotessh插件
###### 同步
> 注册登录GitHub账号实现同步vscode挺好使的
> 但是vscode想要在编程
> 主机上需要安装c\python\cpp等编辑器，太麻烦了，还是远程服务器好用


## ssh 免密远程访问
###### 本地配置SSH/.config
+ 可以在vscode 的 remotessh插件中，点击设置，选择user目录下的.ssh/config
+ 设置 host
###### 本地 ssh-keygen
+ `ssh-keygen`生成 "C:\Users\ouc\.ssh\id_rsa"和"C:\Users\ouc\.ssh\id_rsa.pub"
> 这里有个坑，我一开始没理解明白，在远程服务器上执行了`ssh-keygen`一失足成千古恨，下午再git push的时候就push不上了，私钥变了，只能重新生成公私钥，配置github 
+ 一边将公钥上传到远程服务器 `scp -P portnum C:\Users\ouc\.ssh\id_rsa.pub your-remote-hostname@xx.xx.xx.xx:~/.ssh/`
+ 一边将私钥地址保存在.SSH/.config文件里 `IdentityFile C:\Users\ouc\.ssh\id_rsa`
###### 远程 认证
+ 用本地上传的公钥生成认证密钥
> `cat id_rsa.pub >> authorized-keys`

> 完成以上工作，重启vscode，远程访问服务器不需要输入登录密码了就
>


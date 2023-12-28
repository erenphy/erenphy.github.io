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
    - 图床
    - marp
    - OUC主题PPT模板
    - PPT模板
---

> 三番四次回顾，每次换新的主机都要重新鼓捣一遍


## vscode 安装插件
###### marp插件
> vscode制作ppt
> 安装插件后，只需要在新建md文件的最前面输入以下声明即可
```markdown  
---  
marp: true  
---  
```
+ ppt页面分隔
  + 可以在文中用`---`进行页面分割
  + 也可以设置全局指令`headingDivider:NUM`通过识别md文件的标题进行页面分割
    + 这里的NUM表示用几级标题进行页面分割
    + 若想让页面同时被多个级别的标题分割，可以通过`headingDiviver:[2,3,4]`
+ `paginate`显示幻灯片页码
  + 一般不在标题页面作为页码1，只需要将指令移到第二页即可
+ `header`和`footer`设置页眉页脚 
+ 图片
  + 大小控制：在图片名称位置配置：w:300 h:300，如`![w:300 h:300我的图片名称](我的图片地址)`
  + 背景图：`![bg]()`
  + 多重背景图：`![bg]()`<br>`![bg]()`
  + 背景图拆分：`![bg left]()`或`![bg right]()`
  + 背景色：`![bg](颜色参数)`

###### OUC主题marp
  + 在[awesome MARP](https://github.com/favourhong/Awesome-Marp)的基础上，设计了包含中国海洋大学OUC元素的marp-[ppt模板](https://github.com/erenphy/oucmarptheme.git)   
  
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




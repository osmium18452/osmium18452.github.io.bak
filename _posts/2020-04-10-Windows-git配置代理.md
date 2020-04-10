---
layout:     post
title:      "Windows 下给git配置代理"
subtitle:   ""
date:       2020-04-10
author:     "lwb"
header-img: "img/post-bg-desk.jpg"
tags:
    - git
    - tech
    - proxy
---

给 git 设置代理这事从接触 GitHub 开始就一直在折腾，但是就是死活配不上，那23KB/s的下载速度实在是蛋疼，但是又不是不能用就一直懒得没细研究，直到
前几天实在受不了了，上网上查了两天才弄明白。
这里记一下，免得以后再去找。

#HTTP/HTTPS 代理

##配置

`git config --global http.proxy 'http://127.0.0.1:1080`

`git config --global https.proxy 'https://127.0.0.1:1080`

如果用的 ssr 的话，

`git config --global http.proxy 'socks5://127.0.0.1:1080`

`git config --global https.proxy 'socks5://127.0.0.1:1080`

## 解除

`git config --global --unset http.proxy`

`git config --global --unset https.proxy`

#SSH 代理

一开始在网上看到是 `ProxyCommand nc -v -x 127.0.0.1:1080 %h %p`, 但是这么配置了以后就提示没有nc这个指令，上网一查发现nc是Linux的一个用
来做代理（？）的程序，于是乎只能另找。
最后在[这里](https://walkedby.com/sshwindowsproxy/) 发现要用git目录里面的connect。

总而言之步骤如下：

1.  在 C:\users\username\.ssh下面建一个config文件。
2.  在config里面写上一行：`ProxyCommand "C:\Program Files\Git\mingw64\bin\connect.exe" -S 127.0.0.1:1080 %h %p`
3.  这时候再去拉repo的时候会发现他会让你输密码，如果你的socks代理没密码的话就直接回车。
    但是这有个蛋疼的问题就是他每次都会让你输密码，而且在git bash里面还会提示密码错误，如果不想麻烦，就直接去环境变量里面是一个socks5的密码变量：
    
    ![socks5 password environment variable](/img/git-proxy/socks-password.png)
    

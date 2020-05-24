---
layout:     post
title:      "Set Git Proxy for Windows"
subtitle:   ""
date:       2020-04-10
author:     "lwb"
header-img: "img/post-bg-desk.jpg"
tags:
    - git
    - proxy
---

Due to China's GFW it's a problem to pull repos from and push commits to github, especially when you need to pull a very
large repo from it.
10~30 Kb/s bandwidth recalled me the time when I was in kindergarten around 2004, a time that most videos could olny be 
played in 360p.
Therefore, a proxy is critical for you to use the github in China.

There're two way to pull/push repos/commits from/to github: HTTPS and SSH.
Here we'll introduce both. 

# HTTP/HTTPS Proxy

## Enable

`git config --global http.proxy 'http://127.0.0.1:1080'`

`git config --global https.proxy 'https://127.0.0.1:1080'`

if you uses SSR,

`git config --global http.proxy 'socks5://127.0.0.1:1080'`

`git config --global https.proxy 'socks5://127.0.0.1:1080'`

## Disable

`git config --global --unset http.proxy`

`git config --global --unset https.proxy`

# SSH Proxy

At first i found this on google:`ProxyCommand nc -v -x 127.0.0.1:1080 %h %p`. 
However, when I write it into my `.ssh/config`, there's and error saying cannot find command nc.
So i searched the internet and found nc is available on Linux (and my pc is a Windows).
For Windows i found [this](https://walkedby.com/sshwindowsproxy/).
It says that we need to use the connect tool in the director where you install the git in.

Anyway, here're the steps:

1.  Create a file named `config` under `C:\users\username\.ssh`.

2.  Write `ProxyCommand "C:\Program Files\Git\mingw64\bin\connect.exe" -S 127.0.0.1:1080 %h %p` in the `config`.

3.  At this time if you pull repos using `git pull` it will remind you to enter your password.
    If you don't have any password for your socks proxy, just hit the return button.
    If you don't want to enter your password everytime, then set a environment variable named `SOCKS5_PASSWORD`.
    The value of the variable should be your socks password or anything if you don't have.
    ![socks5 password environment variable](/img/git-proxy/socks-password.png)

4.  Restart your cmd.exe, pull a large repo (like node.js `git clone git@github.com:nodejs/node.git`) to test the proxy.
    OJBK.
    
# P.S.

今天ssr的1080端口不知道为什么被占用了，换了个端口以后git push的时候一直提醒我 failed to connect socks5://127.0.0.1:1080，用
`git config --global https.proxy 'socks5://127.0.0.1:1088'` 改端口后还是不行，一直都在connect 1080。
后来去.gitconfig里一看，竟然有个
```
[http "https://github.com"]
	proxy = socks5://127.0.0.1:1080
```
一口老血，把这里改成1088就连上了。
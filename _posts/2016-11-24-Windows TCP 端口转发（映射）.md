---
layout: post
title: Windows TCP 端口转发（映射）
author: HelloDBA
tags: [Windows,Nat]
category: Nat
---

在某些环境中，我们需要通过一台服务器访问另外一台服务器某个端口,例如：

需要通过192.168.1.2 的 2433 端口访问192.168.1.6的2433端口，只需在192.168.1.2上要执行一条命令就可以做到

```
PS C:\> netsh interface portproxy add v4tov4 listenport=2433 listenaddress=192.168.1.2 connectport=2433 connectaddress=192.168.1.6

```
这时访问192.168.1.2的2433端口其实是访问的后端192.168.1.6的2433端口。

取消端口转发，我们只需将上面命令的**add**变换成**delete**就可以了。

通过命令还可以查看将那些端口做了转发（映射）：

```
PS C:\> netsh interface portproxy show all

侦听 ipv4:                 连接到 ipv4:

地址            端口        地址            端口
--------------- ----------  --------------- ----------
*               2433        192.168.1.6     2433
*               2434        192.168.1.16    2433

```

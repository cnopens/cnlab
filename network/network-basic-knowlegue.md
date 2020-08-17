#network-basic-knowlegue.md
---

***WSS、SSL 和 https 之间的关系***

SSL

SSL(Secure Socket Layer，安全套接层) 简单来说是一种加密技术, 通过它, 我们可以在通信的双方上建立一个安全的通信链路, 因此数据交互的双方可以安全地通信, 而不需要担心数据被窃取. 关于 SSL 的深入知识, 可以看这篇文章: SSL/TLS协议运行机制的概述

WSS

WSS 是 Web Socket Secure 的简称, 它是 WebSocket 的加密版本. 我们知道 WebSocket 中的数据是不加密的, 但是不加密的数据很容易被别有用心的人窃取, 因此为了保护数据安全, 人们将 WebSocket 与 SSL 结合, 实现了安全的 WebSocket 通信, 即 WebSocket Secure.
所以说 WSS 是使用 SSL 进行加密了的 WebSocket 通信技术.

HTTPS

其实 HTTPS 和 WSS 类似, HTTP 之于 HTTPS 就像 WebSocket 之于 WebSocket Secure.
HTTP 协议本身也是明文传输, 因此为了数据的安全性, 人们利用 SSL 作为加密通道, 在 SSL 之上传递 HTTP 数据, 因此 SSL 加密通道上运行的 HTTP 协议就被称为 HTTPS 了.
总结

SSL 是基础, 在 SSL 上运行 WebSocket 协议就是 WSS; 在 SSL 上运行 HTTP 协议就是 HTTPS.


***网络常用命令***

Linux下查看网关方法：
```
    route -n

    ip route show

    traceroute www.prudentwoo.com -s 100 第一行就是自己的默认网关

    netstat -r

    more /etc/network/interfaces Debian/Ubuntu Linux

    more /etc/sysconfig/network-scripts/ifcfg-eth0 Red Hat
```
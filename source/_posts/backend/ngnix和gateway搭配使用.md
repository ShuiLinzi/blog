---
title: nginx和gateway配合使用
date: 2021-09-21
tags: 
- Java
categories:
- "后端"
---
:::primary
你知道当你把网址输入到浏览器按下回车键后，背后发生了多少事吗？
:::
<!-- more -->

## ngnix的作用
- 反向代理
- 负载均衡
- HTTP服务器（动静分离）
- 正向代理

## 反向代理
反向代理（Reverse Proxy）方式是指以代理服务器来接受internet上的连接请求，然后将请求转发给内部网络上的服务器，并将从服务器上得到的结果返回给internet上请求连接的客户端，此时代理服务器对外就表现为一个反向代理服务器。
<br><br>
简单来说就是真实的服务器不能直接被外部网络访问，所以需要一台代理服务器，而代理服务器能被外部网络访问的同时又跟真实服务器在同一个网络环境，当然也可能是同一台服务器，端口不同而已。<br><br>
可以保证内网的安全，可以使用反向代理提供WAF功能，阻止web攻击<br><br>
反向代理的流程

![反向代理.png](/blog/img/backend/反向代理.png)
正向代理

![正向代理.png](/blog/img/backend/正向代理.png)

## 简单的反向代理demo
原理：首先在host文件 把一个域名映射到虚拟机的80端口，然后请求来到虚拟机后，符合虚拟机里面的nginx的配置，然后nginx帮我们代理到目标地址


```xml
server {
    listen       80;
    server_name  gulimall.com; #监听此域名的80端口

    #charset koi8-r;
    #access_log  /var/log/nginx/log/host.access.log  main;

    location / {
      proxy_pass http://192.168.163.1:10000; #代理到目标的地址
    }
}
```
## nginx和gateway配合使用流程分析
整个图示的流程
输入网址，映射到服务器上面的80端口，然后nginx反向代理给内网的gateway网关地址，gateway在根据请求的路由，跳转到不同的微服务
>注意nginx代理会丢失一部分信息，比如host头

<br><br>
![](/blog/img/backend/nginx.png)

## 最终产生的效果
请求接口，请求页面 使用同一个地址

- nginx直接代理给网关，网关判断
    - 如果是/api/***,转交给对应的服务器
    - 如果满足域名，转交给对应的服务，显示页面

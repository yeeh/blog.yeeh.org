---
layout: post
title: vultr安装outline
date: 2018-6-2 23:02:00 +0800
categories: 闲
tags: vultr outline
---

> outline出来有段日子了，看了下教程，已经是超级简单。

### outline组成：

server：服务端

manager：管理工具，省却敲命令，走的是http api

client：全局的，不算太好用。

> vultr之前囤了一日一美 vps，2.5刀一个月，500g流量，正合适。

### 先开bbr

```sh
wget –no-check-certificate https://github.com/teddysun/across/raw/master/bbr.sh
chmod +x bbr.sh
./bbr.sh
```
装完重启。

### 关防火墙打开端口
```sh
service firewalld stop
```

### 装outline server

```sh
bash -c "$(wget -qO- https://raw.githubusercontent.com/Jigsaw-Code/outline-server/master/src/server_manager/install_scripts/install_server.sh)"
```

### 报错的话先装docker后启动docker，之后再装outline server

```sh
curl -sS https://get.docker.com/ | sh

service docker start
```





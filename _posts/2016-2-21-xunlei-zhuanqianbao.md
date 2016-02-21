---
layout: post
title: 迅雷赚钱宝pro利用小米路由内置硬盘
date: 2015-12-21 10:00:00 +0800
categories: 闲
tags: 迅雷,赚钱宝,小米,路由,nas
---

迅雷出品的赚钱宝是利用家中的闲置带宽赚钱，看宽带上行速度每天有几块钱，但是有个不好的地方在于必须要插usb硬盘，nas上很多空余空间根本利用不起来，要是使之能用nas的硬盘那就好了，特别是前些日子刚把小米路由二代内置硬盘换成了3T。

赚钱宝pro（二代）就是个openwrt＋赚钱宝应用，挂载nas只要拿到root权限即可，大概步骤如下：

1.拿到root，二代换了root账号密码，且最新版用升级大法搞不定默认密码了，于是直接淘宝上买了个算号的，8块钱把序列号和mac发过去，几分钟就拿到root密码，顺利登陆。

2.增加一个root权限的帐户，我是加了个admin帐户，默认是root权限。这主要是为了防止以后迅雷改root帐户密码，省的再算一回密码。况且内网用，问题不大。

```sh
adduser admin
//输入两次密码
vi /etc/passwd
//先按 i 进入编辑模式
//修改admin一行为下面的
admin:x:0:0::/root:/bin/ash
//按esc 再输入:wq退出
```

3.用新建帐户看看能登陆否，能的话继续。

4.在赚钱宝插入u盘，登陆。

```sh
df -h
//看挂载点，二代一般是/media/sda2，记住挂载点
ps
//查找迅雷进程，看thunder/bin/dcdn_client 0这个进程号
kill 进程号
//先中断
umount /media/sda2
//解除硬盘挂载
mount -t cifs //10.0.0.200/XiaoMi/xunlei /media/sda2 -o username=,password=,rw
//挂载硬盘
//如果报没权限，就在nas设备上把权限开启
df -h 
//查看是否挂载成功
```

5.如果已成功，那么后面就是把挂载程序加到启动项目。二代文件都拆散了，经过咨询，/etc/rc.d/S21thunder种增加sleep和mount这2行，保存重启即可。

```sh
START=21boot() {        /thunder/bin/run.sh &        sleep 10        mount -t cifs //192.168.1.125/sda1/1507 /media/sda5 -o username=,password=,rw}
```
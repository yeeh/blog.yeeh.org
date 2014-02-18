---
layout: post
title: mybook live添加迅雷远程
date: 2014-1-3 01:00:00 +0800
categories: 手
tags: 迅雷, mybook live,远程
---

今天一群里贴了个迅雷新出的神器网址g.xunlei.com，瞄了下是出了个linux远程下载插件，给各种linux设备加上远程下载的功能，比如nas，电视盒子，openwrt，ddwrt的各种路由器。

本来我已有磊科nw762这个远程下载器，但是无奈nw762的局域网共享速度非常不给力，才3.4mb/s，看个1080p能急死人。于是打算把mybook live加上远程支持，话说mybook live的局域网是千兆，共享完全看路由器性能，性能还算给力。

办法挺简单的，下对应设备的程序，在linux上装上即可。但mybook live内置的是debian，不是centos，所以又光荣踩坑了。

先把mybook live的ssh开了，在路由器ip/UI/ssh里可以直接打开ssh，直接ssh上去，输入密码进入。我的mybook live 3t的硬件配置是800m cpu，内存256mb。

```
df -h
```

看了下下载盘符是/DataVolume，系统设立的目录在shares里，于是把程序装到/DataVolume/xunlei/，之后直接

```
cd /DataVolume/xunlei/
chmod 755 * -R
./portal
``` 

运行

```
initing...

try stopping xunlei service...

setting xunlei runtime env...
bind(3): errno = 98.
port: 9001 is usable.

YOUR CONTROL PORT IS: 9001


starting xunlei service...

...

THE ACTIVE CODE IS: ******

go to http://yuancheng.xunlei.com, bind your device with the active code.
finished.

```
这就是成功了。在yuancheng.xunlei.com里输入激活码，捆绑上就可以。

新建个sh文件,放在/etc/init.d/目录下，我命名为xunlei，无后缀，内容如下

```
#!/bin/sh

START=99
start(){
        mount -o bind /shares/Public /DataVolume/TDDOWNLOAD
        /DataVolume/xunlei/portal
}
stop(){
        /DataVolume/xunlei/portal -s
}
restart(){
       stop
       start
}

case "$1" in
    start)
        start
    ;;
    stop)
        stop
    ;;
    restart)
        restart
    ;;
    cleanup)
    ;;
    *)
        echo $"Usage: $0 {start|stop|restart}"
        exit 1
esac

exit $?
```

mount -o bind /shares/Public /DataVolume/TDDOWNLOAD 是因为迅雷远程下载的默认目录是/DataVolume/TDDOWNLOAD，但是下载目录没有自动记忆功能，如果不虚拟，那么每次安排下载任务需要留意更改目录，还不如虚拟下省事。

然后设置成开机自启

```
update-rc.d xunlei defaults 99 1     
```

重启下mybook live，大功告成。

踩坑主要是开始把迅雷放到root下面去，那个盘符只有1g大小，下什么片都不够的。迅雷默认是程序放在哪个盘符，就下到哪个盘符，比如mybook live，就要放到/Datavolume/目录下，且yuancheng.xunlei.com的下载目录，要设置成C:/shares/*****，比如我就是设置成了C:/shares/Public/,这样在局域网共享里才能看到。

linux蛮有意思的。

---
layout: post
title:  "linuxshell备忘录"
date:   2007-07-23 22:20:49
categories: 默认分类
tags:
---

* content
{:toc}

1.如何查询当前硬盘的使用情况
           #df
2.如何显示当前开起的进程
            #ps –a
3.如何关闭进程
            #kill –9 进程ID
4.telnet 远程登录时，权限更改
           #su root
           password:your password
5. 硬盘分区
至少要一个/分区 和 一个 /swap分区（一般为内存大小的两倍），Iinux会自动创建/bin，/data，/usr，/boot …..等目录，但这样在使用的过程中，时间长了以后会产生磁盘碎片，等等。
最好的是为/boot，/data，/usr，/var，/temp等也都分好单独的区，然后将linux挂接上去。
        

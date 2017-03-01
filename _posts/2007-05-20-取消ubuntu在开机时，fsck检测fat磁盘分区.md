---
layout: post
title:  "取消ubuntu在开机时，fsck检测fat磁盘分区"
date:   2007-05-20 16:36:01
categories: 默认分类
tags:
---

* content
{:toc}

因为我有双系统，每次ubuntu开机时都会去检测fat分区，而且会出现一些错误，使得开机速度变得很慢；  在网上搜索了下，把/etc/fstab的pass列(最后一列)改成0，就可以取消检测了。  如果想调大fsck开机检测的频率，可以使用tune2fssudo tune2fs -c 60 /dev/hda1sudo tune2fs -c 60 /dev/hda5
        

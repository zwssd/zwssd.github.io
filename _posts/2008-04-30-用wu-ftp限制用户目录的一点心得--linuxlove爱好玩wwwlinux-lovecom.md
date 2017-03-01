---
layout: post
title:  "用wu-ftp限制用户目录的一点心得--linuxlove爱好玩www.linux-love.com"
date:   2008-04-30 09:01:10
categories: linux服务器
tags:
---

* content
{:toc}

一直为不能吧ftp用户限制在自己的目录伤脑筋，这两天到处找资料看，问网友自己试验，总算成功了，写出来大家看看吧。用wu-ftp限制用户目录的一点心得 
2001-05-28 15:39 发布者：gemini 阅读次数：1661 
　　一直为不能吧ftp用户限制在自己的目录伤脑筋，这两天到处找资料看，问网友自己试验，总算成功了，写出来大家看看吧。 
　　首先要d个wu-ftp咯！我在做的过程中发现wu-ftp的版本不同，做出来的效果也不同。 
例如 /ftp/--->user1 
　　　　　--->user2 
　　　　　---〉user3 
　　用wu-ftp2.4做的时候，要在每个用户的目录下例如是/ftp/user1/...建立etc.bin.dev.usr几个目录，才能把用户限制在自己的目录里，并且能正常显示出目录内容。 
　　而用wu-ftp2.6做的时候，只需要在/ftp下建立etc.bin.dev.usr就可以把达到目的了。不过不知道是不是我做的有什么地方不对，导致这种差异，谁知道的告诉我！ 好啦，d好软件，就直接编译一下， 
在/etc/inetd.conf里把原先的in.ftpd用生成的ftpd代替，后面要加个-a参数哦，表示读取配置文件（好像是这个意思） 
接下来要编辑ftpaccess文件。（其实都有模版的，只要照着需要改一下就可以了） 
class user guest，real,anonymous（名字随便取） 
real表示server上真实的用户，也就是passwd里有的用户 
anonymous表示匿名用户，这个不用说了吧? 
guest可以自定义。如果你不做anonymouse ftp最好把其他的去掉，只留这个 
我个人认为。 guestgroup ftpuser 
定义guest用户的范围。就是server里属于ftpuser这个组的用户都是guest用户。 restricted-uid * 
这一句好重要，限制了guest用户在自己的目录里。 　　其他的看着模版作。然后存盘就可以了！这个时候应该就已经可以限制住用户了，但是用户登陆上来以后，看不到自己的目录内容，也就是ls用不了。 
　　这时就要mkdir上面那几个目录了，usr,dev,bin,etc 具体位置就是上面说的了！ 几个目录的内容如下： 
~/etc: TIMEZONE* group netconfig passwd 
~/dev: null tcp ticotsord udp zero(得拥mknod命令作） 
~/bin: ls* 
~/usr: bin(ln -s ../bin) lib/(目录) share/(目录) 
~/usr/lib: ld.so* ld.so.1* libc.so* libc.so.1* 
libdl.so* libdl.so.1* libintl.so* libintl.so.1* 
libmp.so* libmp.so.1* libnsl.so* libnsl.so.1* 
libw.so* libw.so.1* libsocket.so* libsocket.so.1* 
nss_dns.so.1* nss_files.so.1* nss_nis.so.1* 
nss_nisplus.so.1* straddr.so* straddr.so.2* 
(拷贝这些文件时，非常容易死机，我也不知道为什么，最好用光盘启动系统，从光盘上拷) ~/usr/share/lib/zoneinfo: GMT-8 US/(目录) 
长长的一串目录照建阿！ 
~/usr/share/lib/zoneinfo/US: Pacific 　　好了，在ftp下或者ftp/userXX下建立相应的目录，并从系统相同的目录下拷贝相应的文件进这些目录就可以了！保持目录结构和属性。 最后还要修改/etc/passwd文件 
在passwd文件的标示用户主目录的域改一下，例如： 
mail:x:1001:100::/aquser/mail:/bin/sh（原来的） 
用wu-ftpd2.4的改称： 
mail:x:1001:100::/aquser/mail/./:/bin/sh 
用wu-ftpd2.6的改称： 
mail:x:1001:100::/aquser/./mail:/bin/sh 
（注：其实如果单要限制用户目录功能的话，在ftpaccess 最后加个restricted-uid *就可以了）本篇文章来源于 Linux爱好网|www.linux-love.com 原文链接：http://www.linux-love.com/server/20080408/yongwu-ftpxianzhiyonghumuludeyidianxinde_7577.shtml
        

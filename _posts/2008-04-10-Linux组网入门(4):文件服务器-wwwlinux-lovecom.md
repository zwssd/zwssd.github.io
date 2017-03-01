---
layout: post
title:  "Linux组网入门(4):文件服务器-www.linux-love.com"
date:   2008-04-10 10:16:32
categories: linux服务器
tags:
---

* content
{:toc}

在一个网络上，可能不只有LINUX系统，还会存在着象Windows NT、Windows95等其它操作系统。如何让这些安装不同操作系统的机器进行文件级的资源共享呢？下面我们就一起来探讨这个问题。Linux组网入门(4):文件服务器 原创 02-04-17 17:38 1565p fjxufeng
在一个网络上，可能不只有LINUX系统，还会存在着象Windows NT、Windows95等其它操作系统。如何让这些安装不同操作系统的机器进行文件级的资源共享呢？下面我们就一起来探讨这个问题。9.1 让LINUX成为文件服务器安装Samba服务器9.1.1 什么是Samba
Samba可以想象成一个局域网上的文件服务器。它可以为在同一个了网中的客户(如Win95、WinNT等)提供文件服务和打印服务。也就是说，Samba服务器可以让LINUX实现象Novell Netware文件服务器提供的功能。9.1.2 Samba工作原理
Samba的工作原理是，让NETBIOS（Windows95网络邻居的通讯协议）和SMA（Server Message Block）这两个协议运行于TCP/IP通信协议之上，并且使用Windows 95的NETBEUI协议让LINUX可以在网络邻居上被Windows 95看到。
其中最重要的就是SMB协议（Server Message Block），这是一个用于不同计算机之间共享打印机、串行口和通讯抽象（如命名管道、邮件插槽等）的协议。SMB协议是一个非常重要的协议，在所有 的Microsoft Windows系列操作系统中广为应用。
Samba是SMB服务器在类UNIX系统上的实现。它是开放源代码的GPL自由软件。目前Samba可以在几乎所有的UNIX变种上运行。9.1.3 安装Samba服务器
在RedHat LINUX操作系统中，只要在安装的时候选择了Samba，那么它就会在安装LINUX的同时安装Samba。如果没有选择的话，也可以在光盘上找到Samba的RPM安装包，使用RPM安装它就可以了。9.1.4 配置Samba
配置Samba的工作其实就是对它的配置文件smb.conf进行相应的设置。Smb.conf关系着Samba服务器的权限设置，以及共享的目录、打印机和机器所属的工作组等各种细致的选项。
文件smb.conf的语法非常明确。文件被分成段，每一段的名字用一个方括号括起来。在每一段内用名称=值的格式来设置参数。最前面加分号表示该句为注释。在后面的讲述中，我们只说明最常用到的最基本的一些部分，而更加深入的设置，请大家阅读这个文件的注释段。
整个配置文件中最基本是三个特殊段。
1． Global段：配置服务器在整个过程中用到的参数，并为其它段提供缺省值。
[global]
workgroup=MYGROUP; 
hosts allow = 192.168.1. 192.168.2. 127.
printcap name = /etc/printcap
load printers = yeslog 
file = /var/log/samba/log.%m 
1） 第一句workgroup用来指定机器在网络邻居所处的工作组。默认值为MYGROUP，大家可以根据自己的喜爱进行相应的修改。
2） 而hosts allow是一个用来指定在局域网中哪些机器可以使用Samba服务的描述。一般情况，无须设置，所以最前面用一个；开始，表示将这句注释掉。
3） 而第三句则是告诉Samba，打印机名称的位置。
4） 第四行load printers = yes则是告诉Samba服务器，允许浏览所有的打印机。
5） 最后一句则是指定了log日志文件的存放地址。
2．Homes段：这个段是用来表示允许客户机连接的用户主目录。在smb.conf文件中没有这个目录的特定内容。当发出服务请求时，就在smb.conf文件的其它部分寻找这种特定的服务。如果没有发现这种服务，并且提供了homes段时，就搜索密码文件去发现用户的主目录。通过分解Homes段，Samba使用户主目录作为共享而使用。下面是这个段的最基本的几个设置。
[homes]
comment=Home Directory
browseable=no
writable=yes 
1) 其中comment提定客户机在服务器上可以使用的共享。
2) browseable则设置Samba在网络浏览表是否显示目录，建议改为browseable=yes。
3) 最后一句则是表示是否具有写权限。
3． Printers段:设置打印机的共享状况。样板如下表所示：
[printers]
comment=All printers
browseable=no
printable=yes 
建议将browseable=no改为browseable=yes。
一般地，在默认的smb.conf文件中已经做了最基本的设置，不加修改就可以应用在多种情况之中。所以建议初学者可以不用修改它。当然如果必要的话，可以参考注释语句进行一些尝试。
也就是说，如果大家不对smb.conf作修改，客户机已经能够使用最基本的Samba服务.9.1.4 启动Samba服务器
启动Samba服务器的方法有两种。一种是standalone方式，另一种是Inted方式:
启动方式 占有资源 反应速度
Standalone方式 多 快
Inted方式 少 慢
我们建议使用Inted方式启动Samba服务器，要注意的是，我们不能同时采用两种方式运行，否则将造成Samba服务工作不政常。而默认状态下也是使用这种启动方式。
1． 使用Inted方式启动
安装Samba时，会在/etc/services文件中增加类似的以下几行：
netbios-ns 137/tcp
netbios-ns 137/udp
netbios-dgm 138/tcp
netbios-dgm 138/udp
netbios-ssn 139/tcp
netbios-ssn 139/udp 
而在/etc/inetd.conf文件中也新增了以下几行：
netbios-ssn stream tcp nowait root /usr/sbin/smbd 
smbdnetbios-ns dgram udp wait root /usr/sbin/nmdb nmdb 
如果你想要用下一种方式启动，请在它们前面加上一个注释符号#，然后执行inetd命令使修改生效。
2． 使用Standalone方式启动
如果你要使用这种方式启动，请在/etc/rc.d/rc.local文件中加入以下几行：
echo Startting Samba Server……
/usr/local/samba/bin/smbd D -d1
/usr/local/samba/bin/nmbd D d1 n LINUX9.1.5 使用Samba服务本篇文章来源于 Linux爱好网|www.linux-love.com 原文链接：http://www.linux-love.com/server/20080408/Linuxzuwangrumen-4--wenjianfuwuqi_7580.shtml以Windows 9x为例，我们只要打开网上邻居，就可以发现一个新的工作组MYGROUP，
下面还有这台LINUX主机。我们就可以使用Samba服务了。9.2 网络文件系统NFS
请在安装LINUX的时候选中NFS，让LINUX系统将这项服务安装到系统中来。接着我们就可以十分容易地使用它了。9.2.1 共享LINUX的文件
通过NFS共享LINUX的文件很简单，只要修改/etc/exports文件就可以了。例如，我们想将/home/nfstest这个目录共享给202.101.55.5这台机器，并且赋予它读、写权限，那么只要将如下信息写入/etc/exports这个文件中去就可以了。
/home/nfstest 202.101.55.5(rw)9.2.2 在LINUX中将共享的文件挂进来
接着，如果我们可以在202.101.55.5这台机器(202.101.55.1)上将/home/nfstest外挂进来。我们只要简单地执行命令：
mount t nfs 202.101.55.1:/home/nfstest /mnt/nfstest 
这样就将202.101.55.1上的/home/nfstest目录挂到了202.101.55.5的/mnt/nfstest目录下了。
本篇文章来源于 Linux爱好网|www.linux-love.com 原文链接：http://www.linux-love.com/server/20080408/Linuxzuwangrumen-4--wenjianfuwuqi_7580_2.shtml
        

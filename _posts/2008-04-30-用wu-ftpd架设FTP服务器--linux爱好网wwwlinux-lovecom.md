---
layout: post
title:  "用wu-ftpd架设FTP服务器--linux爱好网www.linux-love.com"
date:   2008-04-30 09:02:10
categories: linux服务器
tags:
---

* content
{:toc}

wu-ftpd的安装非常容易，大多数版本的Linux中都包含了wu-ftpd的rpm软件包，你可以在安装Linux时指定装入。如果你想自行编译源代码，也可以到ftp://ftp.wu-ftpd.org下载最新版本的源代码包。用wu-ftpd架设FTP服务器 
2001-04-13 08:46 一、前言 当我们架设的网站需要提供下载功能时，除了使用http的方式连接外，也可以另外提供ftp服务供用户直接连线下载。事实上，ftp是个存在已久的服务，它的设计是用来传输两台电脑之间的数据，以避免太多的远端执行。如果要传送的文件比较大时，若以http的方式连线传输会占用一些网站的资源(例如可连线的人数)，这时就要用到ftp了。ftp是一个以TCP/IP为基础的应用程序，所以一般的ftp服务程序都会以内嵌于inetd的执行方式。 
ftp分为两个部分，一个是服务器端的程序，一个是用户端的。在Unix上的ftp服务程序非常多，不同的操作系统所内建的版本也都不一样，常见的有wu-ftpd、proftpd、Troll ftpd、ncftpd和Bero ftpd等等。其中最常用的最受欢迎的的是wu-ftpd，它是当初由华盛顿大学wuarchive.wustl.edu开发出来的，是一个以效率以及稳定性为考量的程序，它提供了原始码以及开放学术单位免费使用。 二、安装与设定 wu-ftpd的安装非常容易，大多数版本的Linux中都包含了wu-ftpd的rpm软件包，你可以在安装Linux时指定装入。如果你想自行编译源代码，也可以到ftp://ftp.wu-ftpd.org下载最新版本的源代码包。 
安装好以后，可以用ckconfig命令来检查是否已经正确安装。在/etc/passwd中可以指定ftp用户的登入目录。 
wu-ftpd主要有以下6个配置文件： 
ftpaccess(主要配置文件，控制存取权限) 
ftpconvertions(配置文件压缩/解压缩转换) 
ftpgroups(设定ftp自己定义的群组) 
ftphosts(设定个别的用户权限) 
ftpservers(设定不同IP/Domain Name以对应到不同的虚拟主机) 
ftpusers(设定哪些帐号不能用ftp连线) 
下面我们来一一介绍。 
⒈/etc/ftpaccess(wu-ftpd的主要配置文件) 
class--定义群组，用法如下： 
class<种类><用户地址>[<用户地址>……] 
由class定义的群组用户才可以连线进来，可以使用多层式的class来规范哪些群组的用户能够从哪些地方上来。这里有三个重要的种类，real、anonymous个guest。real如果没有列在定义中，那么这台机器中任何真实的一般用户都无法用自己的帐号连上来。anonymous如果没有在定义，就表示不让没有帐号的的人连上来。如果有定义guest，那么guest群组的人就可以上来。另外<用户地址>是指ftp上来的用户会用到的IP地址，则可自行设定。以下是一些例子： 
class all real,guest,anonymous * 
定义了一个名为all的class，包含三种人，所有IP的连线用户(也就是所有人都包括了) 
class local real localhost loopback 
local这个class说，只有real的用户可以从本机机器连上来 
class remote guest,anonymous * 
remote这个class包含了从任何地方上来的guest和anonymous用户，但是real用户不算 
class rmtuser real !*.example.com 
rmtuser这个class包含了从外面来的(除了example.com)真实用户 
autogroup--自动对应群组，用法如下： 
autogroup[……] 
当你定义好的那些同属于一个class的用户，一旦连线上来就会被对应到一个相应的群组下面，这样你就可以用Unix的文件权限对某一群人做限制。 
deny--拒绝某些地址连线，用法如下： 
deny<拒绝连线的地址><信息文件> 
禁止某些机器连线，并显示<信息文件>。例如： 
deny 210.62.146.*:255.255.255.254 /etc/reject.msg 
guestgroup--设定访客群 
guestuser--设定访客帐号 
realgroup--设定真实群组 
realuser--设定真实帐号 
nice--设定给某些class多少优先权，用法如下： 
nice 
在Linux中，nice的值是-20(最优先)到19(最后处理)，这里你可以指定负的值来提高某class的优先顺序。 
defumask--设定某class的umask，用法如下： 
defumask[] 
umask是建立文件时该文件的的权限掩码 
tcpwindow--设定tcpwindow的大小 
keepalive--设定是否使用TCP SO_KEEPALIVE来控制断线情形 
timeout--设定连线超时，用法如下： 
timeout accept<秒> 
接受连线超时，预设120秒 
timeout connect<秒> 
连线建立超时，预设120秒 
timeout data<秒> 
数据传送超时，预设1200秒 
timeout idle<秒> 
用户发呆超时，预设900秒 
file-limit--限制某class只能传几个文件，用法如下： 
file-limit[][] 
对某个class限制存取文件的数目，包含了in(上传)、out(下载)，total raw代表整个传输的结果，不光是数据文件。例如： 
file-limit out 20 lvfour 
限制lvfour这个class的用户最多只能下载20个文件 
byte-limit--限制某class只能传几个字节，用法跟file-limit相似 
limit-time--限制一个连线只能持续多久，用法如下： 
limit-time{*|anonymous|guest}<分钟> 
为了避免有人挂在站上不下来，可以用这个方法限制用户的上线时间，例如： 
limit-time guest 5 
让guest帐号的用户只能用5分钟 
limit--限制某class能同时几人上线，用法如下： 
limit<连线数目><时间区段><额满信息文件> 
设定某个class在某一时间区段内最多能够几人同时上线，后面是当超过连线数目时要显示的信息。例如： 
limit all 32 Any /home/ftp/etc/toomanyuser.msg 
限制所有连线在任何时间只能有32个用户，超过则拒绝连线并显示信息 
limit levellone 5 Any2300-0600 /home/ftp/etc/toomanyuser.msg 
限制levellone这个class的用户在23:00到6:00这段时间内只能有5人连线 
noretrieve--设定哪些文件不可下载 
noretrieve[absolute/relative][ 
absolute或relative指文件是用绝对路径还是相对路径 
allow=retrieve--设定哪些文件可以下载 
allow[absolute/relative][ 
loginfails--设置登入错误可尝试的次数本篇文章来源于 Linux爱好网|www.linux-love.com 原文链接：http://www.linux-love.com/server/20080408/yongwu-ftpdjiasheFTPfuwuqi_7578.shtml
        

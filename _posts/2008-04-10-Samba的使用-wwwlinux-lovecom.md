---
layout: post
title:  "Samba的使用-www.linux-love.com"
date:   2008-04-10 11:02:32
categories: linux服务器
tags:
---

* content
{:toc}

在我们常用的局域网中，有许多计算机的操作系统是Windows，并且有一部分的系统是Linux。如何使用两个不同的系统之间实现文件的共享，这个问题经常会发生。除了使用FTP、NFS、Telnet之外，在Linux上构架SMB服务也是一种非常高效、简捷的选择。Samba的使用http://www.linuxaid.com.cn 01-06-13 18:32 1956p dracula
在我们常用的局域网中，有许多计算机的操作系统是Windows，并且有一部分的系统是Linux。如何使用两个不同的系统之间实现文件的共享，这个问题经常会发生。除了使用FTP、NFS、Telnet之外，在Linux上构架SMB服务也是一种非常高效、简捷的选择。一、SMB的介绍
SMBServer Messages Block，信息服务块，是一种在局域网上共享文件和打印机的一种协议，它为局域网内的Windows和Unix系统提供文件及打印机等资源的共享服务。
SMB的工作原理是让Netbios与SMB这两种协议运行在TCP/IP的通信协议上，并且使用Netbios nameserver让Linux机器在Windows的网络领导上被看到，这样就可以在系统间相互访问了。这种协议最早是由Microsoft和Intel在1987年开发实现的。
在Linux上运行SMB的软件很多，最常用的是samba和TAS(Total Net Advanced Server)。Tas是由Syntax公司开发的产品，是一种强大的client/server系统，用来解决不同操作系统间的共享问题。它能对不同类型的SMB客户提供基于SMB的连接，包括LAN Manager,LAN Server,DEC pathworks等。但这么强大的功能，自然价格也不便宜。
samba是用来实现SMB的又一种软件，由澳大利亚的Andrew Tridgell开发，是一种在Linux(unix)环境下运行的免费软件。通过它，Linux和Windows之间可以进行文件的传输，实现打印机的共享：
·共享Linux磁盘给Win95/NT
·共享Win95/NT磁盘给Linux机器
·共享Linux打印机给win95/NT
·共享win95/NT打印机给Linux机器。二、Samba的安装
Samba这套软件中包含了Samba服务器程序和samba客户程序，将samba安装配置好以后，可以在网络间实现资源的共享。随着RedHat 6.x发布的samba的版本是2.0.3，挂起光盘，然后使用RPM命令：
#mount /dev/cdrom /mnt/cdrom
#cd /mnt/cdrom/RedHat?RPMS
#rpm -Uvh samba-2.0.3-8.i386.rpm
你也可以在ftp://ftp.samba.org/pub/samba下载新的版本，如果下载的是RPM的版本，就照上面的方法安装。如果得到是tar软件包，则必须手动编译了。
在得到samba-2.x.x.tar.gz后，随便将其拷贝到一个目录中.ie./tmp。然后使用下面的命令将其展开：
#tar zxvf samba-2.x.x.tar.gz
然后会产生samba-2.x.x这个目录，请先阅读目录中的README文件，如果想看man page的话，可以到docs/manpages目录中执行：
#man ./filename
这样，可以得到一些信息。
由于samba可适用于多种Unix系统，所以下一步工作就是将其source/目录中的Makefile文件修改成你需要的，找出其中定义Linux的部分，然后将前面的#号删除。如果要在装有shadw passwor和quota的Linux系统中安装samba，则应该将Makefile文件中的下面部分的注释号#去掉：
#FIAGSM=-03 -M486 -DLINUX-DSHADOW_PWD-DQUOTAS
#LIBSM=-lshadow
并且在srouce目录中执行：
#ln -s /usr/include/linux/quota.h /usr/include/sys/quota.h
#ln -sf /usr/src/quota-1.50 quota
如果在/usr/src目录下没有quota-1.50请自己去下载其源程序，因为在编译samba时要使用到quota的源程序。
注意，无论选取哪一种系统，都应该保证Makefile中只有一种操作系统起作用，否则samba将无法正确安装。
修改好了Makefile文件之后，只要在source目录中以root权限执行make命令就可以了开始编译了:
#make
成功之后，会得到好几个可执行文件。但是，运行samba服务器程序或客户程序最需要的应该是"smbd"和"nmbd"，它们是一个samba服务器的守护进程和samba客户机的守护进程。接下来执行：
#make install（make revert（删除旧版本并安装新的版本））
命令来安装samba。
一般安装在/usr/bin中。而两个守护进程则被放到/usr/sbin目录下。三、samba的配置
samba的配置有几中方式，一种是用普通的文字编辑器如vi,joe等将/etc/samba目录下的smb.conf文件进行修改。另一种方法是使用X-Windows下的SWAT对samba进行配置。后一种方法比较方便，并且比较适合于初学者，所以着重介绍。
SWAT在Samba 2.x.x及之后版本中已经存在了它是：Samba Web Administration Tool的缩写，它可以大大方便Samba的配置。
在使用SWAT之前，你必须先配置好你的X-windows，使它可以正常工作。然后编辑在/etc目录下的services文件，找到:
#swat 901/tcp
然后将它前面的#号删除。保存services文件。
这一步是将swat的服务器端口901打开，以提供swat的服务及访问。如果这一步不作，那么在X-windows下将不能使用swat。
如果你使用的是Redhat 7.x则
可以输入命令：
#setup
在system services设置中找到swat，然后在前打*号，以下一个步骤可以跳过了。修改/etc/inetd.conf，在其中加入：
swat stream tcp nowait.400 root /usr/local/samba/bin/swat swat
这行的意思是说明swat程序的位置以及驱动的相关参数。
重新激活inetd:用ps -auxww|grep inetd找出inetd的PID,然后给予一个HUP的信号使其重新激活inetd:
ps -auxww|grep inetd
kill -HUP xxx(PID号）
或者并且重新启动。
然后在X-Windows下，找开浏览器（netscape）然后在地址栏中输入:http://localhost:901/，然后回车。
然后请用root帐号登录。
由于Swat界面非常友好，所以一般用户用鼠标就可以设置了。
下面以一个简单的范例来解释它的用法：
（1）让一个目录被共享，但用户只能有读的权限，并且只有guest帐号的用户可以访问；本篇文章来源于 Linux爱好网|www.linux-love.com 原文链接：http://www.linux-love.com/server/20080408/Sambadeshiyong_7579.shtml（2）让一个Linux的打印机被网络中所有的win95/nt机器共享。
首先，先选择GLOBALS主图标按钮，然后在其中的workgroup中输入你所在的局域网中，需要访问你的SMB服务器的机器的工作组名称，比如：mysite。
接下来的netbios name：这是你的Linux主机在win95/NT中的网络邻居中显示出的名称：比如：host Server String:这是对SMB服务器的描述，随便输入一些内容：比如：It's a samba server。
interface:这是网络界面的设置。注意格式为x.x.x.x/x.x.x.x前面是你的Linux主机的IP地址，/之后是子网掩码。
比如：192.168.1.100/255.255.255.0
接下来的几行可以不管，如果需要进一步深入了解，请察看samba-howto。
然后是guest account:这里是来宾帐号，你就输入guest。
host allow和host deny中设置的分别是你特别指定的允许和不允许访问SMB服务器的机器的IP地址，注意其格式为：x.x.x.x,x.x.x.x。
如：192.168.1.22,192.168.1.33
接下来几行不要管，然后是socket optons:
如果你输入：
TCP_NODELAY SO_RCVBUF=8192 SO_SNDBUF=8192
这样可以提高系统的效率，很显然，这是对于TCP/IP协议而言的。请看一下smb的man，这不再深究。
然后点击commit changes，保存结果。
然后再点击主图标按钮SHARES
然后输入你所要共享的资源的名称，随便输入一个比如：share
然后Create Share建立资源。
comment中输入该共享资源的名称。 
Path：在此输入共享资源的绝对路径。如：/home/guest
guest accout：在此输入工作站的guest帐号名称，该帐号必须存在！你可以通过：linuxconf来设置帐号，请查看有关资料，这里不再多讲。
read only下拉列表框：表示是否只允许读取，如果是Yes选项，则无法写入或删除文件。
guest ok下接列表：表示是否允许其它用户以guest方式登录，也就是登录者不一定要有密码就可以使用该共享资源。如果该共享资源是私人拥有，而且不希望其它人登入，最好设为No。
host alow文本框：允许访问你的机器的IP地址，以,分开，也可以203.71.204.来定义整个C Class网络，如果设有host alow则只有这些机器可以登录，其它一
律拒绝！
host deny：不允许访问你的机器的IP地址，格式同上，若设有deny则除该机器之外，则一律允许.
browseable:表示是否让该资源被windows的网上邻居浏览到。
available：则是代表是否开放外界联机这个共享资源。
然后Commit change确认保存。
然后是PRINTERS主图标按钮
如果你已经在print tools中成功地设置了打印机，则在会已经存在一个打印资源名，然后选择choose printer。
comment：是对打印机的描述，随便输入一些，比如：printer。
path：你的打印机spool的路径：比如：/var/spool/samba。
guest accout：意义同上面的share设置
guest ok：意义同上面的share设置
hosts allow,hosts deny:意义同上面的share设置
print ok：yes
print name:你的打印机名
browseable和available意义同上面的share设置。 
STATUS主图标按钮是表明了当前Samba运行的状态，用户可以它来启动和停止 samba进程。
VIEW图标按钮是浏览了/usr/local/samba/lib/smb.conf的内容。四、运行Samba
启动samba有两种方式，一种是以stand-alone方式运行，另一种是由inetd启动。二者各有其优缺点，如果采用stand-alone方式运行，运行速度较快，但是守护
进程将一直会产生子进程来服务，这样，资源被占用地较多，如果你的机器资源足够，并且访问的要求非常高，那么你可以考虑这个高效率的方式。而以inetd方式运行
samba的话，情形正好相反，占用系统资源较少，但是速度相对较慢。两个方法你只可以选择其中的一种，不能都使用，如果这样，会出现错误。
（1）使用inetd启动samba
为了以inetd方式启动samba需要修改/etc/inetd.conf和/etc/services文件。首先编辑/etc/inetd.conf文件，在该文件中加入下面两行：
netbios-ssn stream tcp nowait root /usr/sbin/smbd smbd
netbios-ns dgram udp wait root /usr/sbin/nmbd nmbd
修改/etc/services 需要加入下面六行数据：
netbios-ns 137/tcp nbns
netbios-ns 137/udp nbns
netbios-dgm 138/tcp nbdgm
netbios-dgm 138/udp nbdgm
netbios-ssn 139/tcp nbssn
netbios-ssn 139/udp nbssn
因为安装samba的时候，系统已经自动在/etc/services文件中添加好了，所以不需要再动手添加，但是，如果打算以stand-alone方式运行samba的话，就应该在这几行前面各加一个#注释起来。
然后杀死inetd进程，并重新启动它。
（2）以stand-alone方式启动samba
只要将/etc/rc.d/rc.local文件中国入：
/usr/local/samba/bin/smbd -D -d1
/usr/local/samba/bin/nmbd -D -d1 -n samba
注意，smbd和nmbd的路径一定要正确，请用:whereis smbd和whereis nmbd来找一下。经过这样的设置samba就基本上可以运行了。本篇文章来源于 Linux爱好网|www.linux-love.com 原文链接：http://www.linux-love.com/server/20080408/Sambadeshiyong_7579_2.shtml
        

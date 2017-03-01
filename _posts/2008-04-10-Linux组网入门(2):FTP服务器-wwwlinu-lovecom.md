---
layout: post
title:  "Linux组网入门(2):FTP服务器-www.linu-love.com"
date:   2008-04-10 10:15:32
categories: linux服务器
tags:
---

* content
{:toc}

在众多的网络应用中，FTP（File Transfer Protocol）有着非常重要的地位。在Internet中一个十分重要的资源就是软件资源。而各种各样的软件资源大多数都是放在FTP服务器中的。可以说，FTP与WEB服务几乎占据了整个Internet应用的80%以上。Linux组网入门(2):FTP服务器 原创 02-04-17 17:08 2050p fjxufeng
在众多的网络应用中，FTP（File Transfer Protocol）有着非常重要的地位。在Internet中一个十分重要的资源就是软件资源。而各种各样的软件资源大多数都是放在FTP服务器中的。可以说，FTP与WEB服务几乎占据了整个Internet应用的80%以上。
FTP服务可以根据服务对象的不同分为两类：一类是系统FTP服务器，它只允许系统上的合法用户使用；另一类是匿名FTP服务器，Anonymous FTP Server，它使用任何人都可以登录到FTP服务器上去获取文件。5.1 选择和安装FTP服务器软件
如果你在安装LINUX系统的时候，在选择启动进程的时候选择了ftpd这一项的话，安装完LINUX系统后，它已经将一个默认的FTP服务器安装到系统中去了。我们已经可以利用它来实现系统FTP服务器的功能了。我们只需在此基础上根据我们的需要进行一些个性化设定就可以了。
在绝大多数的LINUX发行版本中都选用的是Washington University FTP，它是一个著名的FTP服务器软件，一般简称为wu-ftp。它功能强大，能够很好地运行于众多的UNIX操作系统，例如：IBM AIX、FreeBSD、HP-UX、NeXTstep、Dynix、SunOS、Solaris等。所以Internet上的FTP服务器，一大半以上采用了它。
wu-ftp拥有许多强大的功能，很适于吞吐量较大的FTP服务器的管理要求：
1） 可以在用户下载文件的同时对文件做自动的压缩或解压缩操作；
2） 可以对不同网络上的机器做不同的存取限制；
3） 可以记录文件上载和下载时间；
4） 可以显示传输时的相关信息，方便用户及时了解目前的传输动态；
5） 可以设置最大连接数，提高了效率，有效地控制了负载。5.2 wu-ftp的组成
安装了wu-ftp后，你将在/bin目录下看到以下五个可执行文件：
ftpd FTP服务器程序
ftpshut 用于关闭FTP服务器程序
ftpcount 显示目前在线人数
ftpwho 查看目前FTP服务器的连接情况
ckconfig 检查FTP服务器的设置是否正确
除了这些可执行文件以外，它还在/etc和/var目录下生成了七个配置文件：
/etc/ftpusers
/etc/ftpaccess
/var/run/ftp.pids
/etc/ftpconversions
/var/log/xferlog
/etc/ftpgroups
/etc/ftphosts
系统安装了wu-ftp后，会建立一个特殊的用户ftp，并在/home目录下建立了一个ftpd目录，当用户以匿名登录上来时，将会自动定位于这个目录下。在这个目录下一般会建立几个子目录。
/home/ftpd/bin:存放一些供FTP用户使用的可执行文件
/home/ftpd/etc:存放一些供FTP用户使用的配置文件 
/home/ftpd/pub:存放供下载的信息
/home/ftpd/incoming:存放供上载信息的空间5.3 wu-ftp的配置
5.3.1 查看、修改/etc/inetd.conf文件
/etc/inetd.conf文件是LINUX系统的超级服务器inetd的配置文件。它负责监听多个TCP/IP端口。当它收到请求，就根据配置文件派生一个相应的服务器。通过使用超级服务器，其他服务就可以只在需要时才派生，从而大大节省了系统资源。
而wu-ftp就是利用超极服务器inetd来监听请求的。当超级服务器inetd收到了客户端的FTP请求时，就根据配置文件打开一个FTP服务进程。所以我们如果要使用wu-ftp，就必须确认在超级服务器inetd的配置文件inetd.conf中有这样一句：
ftp stream tcp nowait root /usr/sbin/tcpd wu.ftpd
以便当超级服务器收到FTP请求的时候，能够派生一个wu-ftp的FTP服务进程。（注：要确认是否有这样一行时，可以使用文件内容查找命令来确认：
cat /etc/inetd.conf | grep ftp 如果没有，则用手工加入或手工修改。5.3.2 wu-ftpd的命令选项
wu-ftpd就是wu-ftp的服务进程。它可以不带参数执行，也可以带参数执行。下面简单介绍一下wu-ftpd的执行参数。
-d 当FTP服务器发生错误时，将错误入系统的syslog中；
-l 将每次FTP客户端进行连接的入系统的syslog中；
-t 设置FTP客户端连接几分钟无操作就切断连接；
-a 使wu-ftp使用/etc/ftpaccess的设定；
-A 使wu-ftp不使用/etc/ftpaccess的设定；
-L 将FTP客户端连线后所执行的程序记录在系统的syslog中；
-I 将FTP客户端上载文件的日志记录在/usr/adm.xferlog文件中；
-o 将FTP客户端下载文件的日志记录在/usr/adm/xferlog文件中。
通过对以上参数的理解，我们建议，将上面系统安装时的那条默认配置改为：
ftp stream tcp nowait root /usr/sbin/tcpd wu.ftpd a I5.3.3 提供自动压缩、解压缩的功能
如果想让FTP服务器有自动压缩、解压缩的功能，必须先将一些压缩、解压缩的命令文件如tar、gzip、gunzip、compress、uncompress等命令文件拷贝到/home/ftpd/bin目录下。5.3.4 关于/etc/ftpaccess的设置
这个配置文件是FTP服务器上最重要的配置文件，它直接关系到你的FTP服务器能否正常工作，还有许多权限上的设置。下面是一个典型的配置实例。
loginfails 3
class local real *
class remote anonymous guest *
limit remote 100 Any /etc/ftpd/toomany.
msgmessage /etc/ftpd/welcome.msg login
compress yes local remote
tar yes local remote
private yes
passwd-check rfc822 warn
log commands real
log transfer anonymous guest inbound outbound
log transfer real inbound
shutdown /etc/ftpd/shut.msg
delete no anonymous,guest本篇文章来源于 Linux爱好网|www.linux-love.com 原文链接：http://www.linux-love.com/server/20080408/Linuxzuwangrumen-2--FTPfuwuqi_7582.shtmloverwrite no anonymous,guest
rename no anonymous
chmod no anonymous,guest
umask no anonymous
upload /home/ftpd * no
upload /home/ftpd /bin no
upload /home/ftpd /etc no
upload /home/ftpd /pub yes real 0644 dirs
upload /home/ftpd /incoming yes real guest anonymous 0644 dirs
alias in /incoming
email guest@xxx.net
email guest@yyy.net
deny *.com.tw /etc/ftpd/deny.msg
下面我们逐句进行讲解，并给出每条设置的含义，以便大家触类旁通，以便根据自己FTP服务器的具体情况进行合理的设置。
1． 格式：loginfails [次数]
功能：设定当用户登录到FTP服务器时，允许用户输错密码的次数。
实例：loginfails 3：密码输入错误三次就切断连接。
2． 格式：class [类名] [real/guest/anonymous] [IP地址]
功能：这个指令的功能设定FTP服务器上用户的类别。并可对客户端的IP地址进行限制，允许某部分的IP地址或全部的IP地址访问。而在FTP服务器上的用户基本上可以分为以下三类：
real 在该FTP服务器有合法帐号的用户；
guest 有记录的匿名用户；
anonymous 权限最低的匿名用户
实例：class local real *：定义一个名为local的类，它包含了在任何地方登录（*代表所有IP地址）的real用户。
class remote anonymous guest *:定义一个名为remote的类，它包含了在任何地方登录的anonymous用户和guest用户。
3． 格式：limit [类别] [人数] [时间] [文件名]
功能：这个指令的功能为设置指定的时间内指定的类别允许连接的指定人数上限。当达到人数上限的时候，显示指定文件的内容。
实例：limit remote 100 Any /etc/ftpd/toomany.msg：在任何时间内，remote类的访问用户达到100人时，将不再允许无法产生新的连接，当第101位客户要连接时，连接将失败，并象用户出示文件/etc/ftpd/toomany.msg的内容。
4． 格式：message [文件名称] [指令] 
功能：当用户执行所指定的指令时，系统将指定的文件内容显示出来。
实例：message /etc/ftpd/welcome.msg login：当用户执行login命令时，也就是登录到FTP服务器上的时候，系统将显示文件/etc/ftpd/welcome.msg的内容。
5． 格式：compress [yes/no] [类别] 
功能：设置哪一个类别的用户可以使用compress（压缩）功能。
实例：compress yes local remote：允许local和remote两个类别的用户都能使用compress(压缩)功能。
6． 格式：tar [yes/no] [类别] 
功能：设置哪一个类别的用户可以使用tar（归档）功能。
实例：tar yes local remote：允许local和remote两类的用户都能使用tar功能。
7． 格式：private [yes/no] 
功能：设定是否支持群组对文件的取用。
实例：private yes：支持群组对文件的取用。
8． 格式：passwd-check [none/trivial/rfc822] [enforce/warn]
功能：设定对匿名用户anonymous的密码使用方式。
none 表示不做密码验证，任何密码都可以登录；
trival 表示只要输入的密码中含有字符@就可以登录；
rfc822 表示密码一定要符合RFC822中所规定的E-Mail格式才能登录；
enfore 表示输入的密码不符合以上指定的格式就不让登录；
warn 表示密码不符合规定时只出现警告信息，仍然能够登录。
实例：passwd-check rfc822 warn：希望能够得到符合规定的E-Mail作为密码，但如果不是，也允许登录。
9． 格式：log command [real/guest/anonymous] 
功能：设置哪些用户登录后的操作记录在文件/usr/adm/xferlog中。
实例：log command real：当real用户登录后，将他的操作记录下来。由于其它用户权限较低，所以操作不会引起太大的安全隐患，所以一般只需记下real用户的操作就可以了。
10． 格式：log transfers [real/guest/anonymous] [inbound/outbound]
功能：设置哪些用户的上载（inbound）和下载（outbound）操作做日志。
实例：log transfer anonymous guest inbound outbound：对于匿名用户要更加的关注它们的文件操作，所以无论上载、下载都进行记录。
log transfer real inbound：对于合法用户则只记录他的上载记录。
11． 格式：shutdown [文件名]
功能：FTP服务器关闭的时间可以设置在后面所指定的文件中，当设置的时间一到，便无法登录FTP服务器了，要恢复的话只有将这个文件删掉。而这个文件必须由指令/bin/ftpshut来生成。
实例：shutdown /etc/ftpd/shut.msg
12． 格式：delete [yes/no] [real/anonymous/guest]
功能：设置是否允许指定用户使用delete命令删除文件。默认是允许。
实例：delete no anonymous,guest：为了更好地管理FTP服务器，一般情况下，我们不允许匿名用户执行delete命令。
13． 格式：overwrite [yes/no] [real/anonymous/guest]
功能：设置是否允许指定用户覆盖同名文件。默认是允许。
实例：overwrite no anonymous,guest：为了更好地管理FTP服务器，一般情况下，我们不允许匿名用户覆盖同名文件。
14． 格式：rename [yes/no] [real/anonymous/guest]
功能：设置是否允许指定用户使用rename命令来为文件改名。默认是允许。
实例：delete no anonymous：为了更好地管理FTP服务器，一般情况下，我们不允许匿名用户执行rename命令改变文件名。而对有记录的匿名用户则适当的放宽，允许他们使用改名命令。
15． 格式：chmod [yes/no] [real/anonymous/guest]
功能：设置是否允许指定用户使用chmod命令更改文件权限。默认是允许。
实例：delete no anonymous，guest：为了更好地管理FTP服务器，一般情况下，我们不允许匿名用户执行chmod命令更改文件权限。
16． 格式：umask [yes/no] [real/anonymous/guest]
功能：设置是否允许指定用户使用umask命令。默认是允许。
实例：delete no anonymous：为了更好地管理FTP服务器，一般情况下，我们不允许匿名用户执行umask命令。本篇文章来源于 Linux爱好网|www.linux-love.com 原文链接：http://www.linux-love.com/server/20080408/Linuxzuwangrumen-2--FTPfuwuqi_7582_2.shtml17． 格式：upload [根目录] [上载目录] [yes/no] [用户] [权限] [dirs/nodirs]
功能：对可以上载的目录进行更加详细的设置。
实例：upload /home/ftpd * no：表示在子目录/home/ftpd下不允许上载；upload /home/ftpd /bin no：表示在子目录/home/ftpd/bin下不允许上载；upload /home/ftpd /etc no：表示在子目录/home/ftpd/etc下不允许上载；upload /home/ftpd /pub yes real 0644 dirs：允许用服务器上的合法用户在子目录/home/ftpd/pub目录下能上载权限为0644(也就是-rw-r--r--)的文件，而且在这个目录下可以新建子目录。 upload /home/ftpd /incoming yes real guest anonymous 0644 dirs：允许所有的用户在子目录/home/ftpd/incoming下能上载权限为0644的文件，而且在这个目录下可以新建子目录。
18． 格式：alias [目录别名] [目录名]
功能：给指定目录设置一个别名，在切换目录时就可以使用较短的目录别名。
实例：alias inc： /incoming：为子目录incoming设置一个别名inc：。
19． 格式：email [guest的E-Mail地址]
功能：只要将某些E-Mail地址设置在这个地方，那么这些用户登录到FTP服务器时，他的身份将为guest，一般权限比real低一些，比anonymous高。
实例：email guest@xxx.net email guest@yyy.net：这里仅是一个示例，实际上可以包含多个符合规范的E-Mail地址。
20． 格式：deny [IP地址/域名] [说明文件]
功能：这个设置可以限制哪一些IP地址或域名的用户无法登入FTP服务器。
实例：deny *.com.tw /etc/ftpd/deny.msg：设置凡是域名是以.com.tw结束的域名，都禁止其访问。而将/etc/ftpd/deny.msg的内容显示给用户看。5.3.5 设置/etc/ftpuser,禁止某些用户登录
有时我们需要禁止一些用户使用FTP服务。其实这个设置是十分简单的，只需要将要禁止的用户帐号写入文件/etc/ftpuser中。由于从系统的安全考虑，一般我们是不希望权限过大的用户和一些与命令名相同的用户进入FTP服务器。所以在缺省的配置中，一般以下用户已经被列入了黑名单。
root uucp news bin adm nobody lp sync shutdown halt mail5.3.6 设置/etc/ftphosts,禁止某些来自指定机器上的登录
如果你需要拒绝来自某些主机的登录，一种方法就是在/etc/ftpaccess中设置deny命令，另一种更加简单的方法就是在/etc/ftphosts中写入你要禁止的主机的IP地址或域名。5.3.7 使新的配置生效
到此为止，我们已经能够根据自己的需要对FTP服务器配置进行必要的修改和调整。而让我们重新配置后，就必须使其生效。一般的，对/etc/ftpaccess的配置是直接作用于设置后的下一次FTP服务进程。而其它的则要对inetd进程重新启动。5.4 wu-ftp相关的其他一些命令的使用5.4.1 连接数统计命令ftpcount
我们可以使用ftpcount命令十分清楚地统计出当前连接到FTP服务器上的用户数目，并且同时列出上限。命令输出如下所示：
Service class local 0 Users(20maximum)
Service class remote 5 Users(100maximum)5.4.2 在线用户查看命令ftpwho
我们可以使用ftpwho命令十分清楚地列出当前连接的用户的详细情况。5.4.2 FTP关闭文件生成命令ftpshut
我们可以使用ftpshut命令生成一个在/etc/ftpaccess中设置的shut.msg文件，用于关机设定。ftpshut命令的格式为：
Ftpshut <-l min> <-d min> time <说明>
-l 这个参数设定在关闭FTP服务器功能前多少分钟时停止用户的连接；
-d 这个参数设定在关闭FTP服务器功能前多少分钟时切断用户连接；
time 指定关闭FTP服务器的时间。例如6：20分则写为0620；本篇文章来源于 Linux爱好网|www.linux-love.com 原文链接：http://www.linux-love.com/server/20080408/Linuxzuwangrumen-2--FTPfuwuqi_7582_3.shtml
        

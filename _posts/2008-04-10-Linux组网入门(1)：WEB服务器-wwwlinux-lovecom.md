---
layout: post
title:  "Linux组网入门(1)：WEB服务器-www.linux-love.com"
date:   2008-04-10 10:12:32
categories: linux服务器
tags:
---

* content
{:toc}

现在在Internet上最热门的服务之一就是WWW（World Wide Web）服务。如果你想通过主页向世界介绍自己或自己的公司，就必须将主页放在一个WEB服务器上，当然你可以使用一些免费的主页空间来发布Linux组网入门(1)：WEB服务器原创 02-04-17 17:08 2624p fjxufeng
现在在Internet上最热门的服务之一就是WWW（World Wide Web）服务。如果你想通过主页向世界介绍自己或自己的公司，就必须将主页放在一个WEB服务器上，当然你可以使用一些免费的主页空间来发布。但是如果你有条件，你可以注册一个域名，申请一个IP地址，然后让你的ISP将这个IP地址解析到你的LINUX主机上。然后，在LINUX主机上架设一个WEB服务器。你就可以将主页存放在这个自己的WEB服务器上，通过它把自己的主页向外发布。4.1 选择和安装WEB服务器软件
目前，在世界各地有许多公司和学术团体，根据不同的计算机系统，开发出不同的服务器，如Apache、CERN、Microsoft Internet Information System、NCSA、WebSite等。它们各有所长。而在许多LINUX的发行版本中，已经集成了一个免费的、使用广泛的、技术成熟的WEB服务器软件Apache。
笔者经过实际的试用，发觉Apache与LINUX的配合还是十分理想的，所以在此笔者就具体地介绍一下Apache在Red Hat Linux 6.0下的配置与实现。
如果我们在安装LINUX的选择启动进程中选中httpd选项。这样Apache就会将自动完成安装，并且能够满足日常的应用需要，我们只需要进行一些更具体的设置工作就行了。4.2 Apache的组成
在Red Hat Linux 6.0中，Apache将自己的所有配置文件和日志文件放在了/etc/httpd目录下，其中/etc/httpd/conf下为配置文件，/etc/httpd/log下为日志文件。
同时，它将建立/home/httpd目录，并在其下建立三个子目录：html/：在这个目录下存放HTML（主页）文件；cgi-bin/：在这个目录下可以存放一些CGI程序；icons/：在这个目录下是服务器自带的一些图标。4.3 Apache的设置
Apache服务器软件的配置文件主要有：access.conf：用于设置系统中的存取方式和环境；httpd.conf：用于设置服务器启动的基本环境；srm.conf：主要用于做文件资源上的设定；mime.type：记录Apache服务器所能识别的MIME格式。
在具体讲解之前，我们必须告诉大家，LINUX系统已经在安装时就采用了一系列的缺省值，而大家可以根据下面的讲解来理解这些设置的意义，然后根据自己的实际情况做一些细微的调整，以更加适合于你的具体应用。
4.3.1 access.conf的配置
当我们使用vi来打开它的时候，我们会发现，就象LINUX一样，内容十分繁多，看得人头晕眼花的。请大家一定要明确，凡是最前面是以#号开头的，表示这一行是注释语句，是帮助大家理解文件内容的，而不是配置文件本身。在下面的讲解中，我们也将把这些注释语句略去不说。
该文件的第一段非注释部分如下：
<Directory /home/httpd/html>
Option Indexes Includes ExecCGI FollowSymLink
AllowOverride None
Order allow , deny
allow from all</Directory>
大家应该注意到，这一个部分是以<Directory /home/httpd/html>开始，以</Directory>结束的。这表示在其中间的部分都是针对指定目录/home/httpd/html而言的。
1．Option命令有很多的参数，各个参数的意义如下所示：
All:准许以下所有功能（MultiViews除外）； 
MultiViews:准许内容协商的Multiviews；
Indexes:若该目录下无index文件，则准许显示该目录下的文件以供选择； 
IncludesNOEXEC:准许SSI(Server-side Includes)，但不可使用#exec和#include功能；
Includes:准许SSI；
FollowSymLinks:准许符号链接到其他目录；
ExecCGI:准许该目录下可以使用CGI。
2．而AllowOverride命令则是用来决定是否准许在access.conf文件中设定的权限是否可以被在文件.htaccess中设定的权限覆盖。它有两个参数：
All 准许覆盖；None 不准许覆盖。
3．Order命令：用来设定谁能从这个服务器取得控制。它也有两个参数：
allow 可以取得控制；deny 禁止取得控制。
现在我们一起来看看关于目录/home/httpd/html的设置的含义：它使得这个目录，如果不存在index.htm文件时，列出目录信息以供选择，准许SSI，允许执行CGI程序，开启了动态连接。它不允许再使用在文件.htaccess中设定来覆盖这里所设置的权限。使所有的人都可以取得控制。
该文件的第二段非注释部分如下：
<Directory /home/httpd/cgi-bin>
Option ExecCGI
AllowOverride None
</Directory>
这个表示目录/home/httpd/cgi的设置为，当前目录下可以执行CGI程序。不允许再使用在文件.htaccess中设定来覆盖这里所设置的权限。
需要说明的是，不同的LINUX系统中，可以在这个文件中看到的信息不完全相同，但是根据这里给出的信息，大家可以参照命令的解释自行理解文件中的设置，以及根据自己的需要进行相应的修改。4.3.2 httpd.conf的配置
这个文件中有许多设定命令，用来设置服务器的运行环境。以下是一些常用的部分：
1． ServerType命令，用来设定服务器的启动方式。它的命令格式如下：
命令格式: ServerType [standalone/inted]
standalone参数表示WEB服务进程以一个单独的守候进程的方式在后台侦听是否有客户端的请求，如果有就生成一个子进程来为其服务。
inetd参数表示WEB服务不是以一个单独的守候进程的形式支持。而是由Inetd这个超级服务器守候进程进行代劳，当它收到一个客户端的WEB服务请求的时候，再启动一个WEB服务进程为其服务。
在此建议使用standalone参数。
2． Port命令，为服务器的服务指定端口号（套接字）。一般来说，WEB服务使用知名端口号80，如果你设定了别的端口号，别人再使用你的WEB服务时，就必须输入http：//xxx.xxx.xxx：端口号，这样是不方便的。所以，建议这里设置为Port 80。本篇文章来源于 Linux爱好网|www.linux-love.com 原文链接：http://www.linux-love.com/server/20080408/Linuxzuwangrumen-1--WEBfuwuqi_7583.shtml命令格式: Port 端口号例：Port 80
3． Server Admin命令，用来设置WEB管理员的E-Mail地址。这个地址会出现在系统连接出错的时候，以便访问者能够将情况及时地告知WEB管理员。
命令格式: Server Admin [you E-Mail address]
例：Server Admin admin@xxx.com
4． BindAddress命令，用来设定要从哪个地址来接受服务。
命令格式: BindAddress [*/IP/FQDN]
例：BindAddress IP 表示只接受输入IP地址的访问者
BindAddress FQDN 表示只接受输入域名地址的访问者
BindAddress * 表示接受以上两种方式的访问者
5． ErrorLog命令，用来指定错误记录文件名称和路径。
命令格式: ErrorLog [log filename]例：ErrorLog /var/httpd/error.log
6． CacheNegotiatedDocs命令，让代理服务器将数据留在缓存中。在很多情况下，默认为不让代理服务器将数据留在缓存中的，所以这条设定命令是被注释掉的。
7． Timeout命令，只要客户端超过这里设定的秒数还没有完成一个请求的话，服务端将终止这次请求服务。如果网络速度较慢的话，建议在此设置较大的数值。以给客户端更多机会。
命令格式: Timeout [second]例：Timeout 120
8． KeepAlive命令，设置是否开启连续请求的功能。
命令格式: KeepAlive [on/off]
9． MinSpareServer命令，用于设置WEB服务进程的最小空闲个数。当WEB服务进程空闲个数小于此设置时，系统将会自动打开更多的服务进程以使得空闲的WEB服务进程的最小空闲个数。
命令格式: MinSpareServer [number]例：MinSpareServer 5
要注意的是，这个数字太大的话，则空闲的进程在浪费系统资源，大大减少了整个系统的资源。如果太小，则有可能造成频繁的连接使得系统应接不瑕。设置的原则是，如果这个服务器是专用的WEB服务器，则将这个值尽量地设大，否则就设置得够用就可以。
10． MaxSpareServer命令，这个命令则是设置WEB服务进程的最大空闲个数。
命令格式: MaxSpareServer [number]例：MaxSpareServer 10
这个命令与前一个相配合，可以使得WEB服务进程在内存中所占资源最合理。
11．StartServers命令，用来设置刚开启WEB服务器时生成几个服务进程。
命令格式: StartServers [number]例：StartServers 5
12．MaxClients命令，用来设置接受客户端请求的最大数目，以使得维护系统稳定性，避免系统负载过大。
命令格式: MaxClients [number]例：MaxClients 1504.3.3 srm.conf的配置
这个文件主要用来指定主页文档的位置。下面介绍三个最常用的命令。
1． Do*****entRoot命令，用来指定主文档的地址。
命令格式: Do*****entRoot [Path]例：Do*****entRoot /home/httpd/html
2． UserDir命令，用来指定个人主页的位置。如果你有一个用户test，那么它主目录是/home/test，当客户端输入 http://yourdomain/~test，系统就会到对应的目录/home/test/UserDir/中去寻找。其中UserDir就是在UserDir命令中设置的指定目录。
命令格式: UserDir [Path]例： UserDir Public_html
3． DirectoryIndex命令，用来声明首页文件名称。一般地，我们使用index.html或index.htm作为首页的文件名。如果这样设置后，那么客户端发出WEB服务请求时，将首先调入的主页是在指定目录下文件index.html或index.htm。
命令格式: DirecotryIndex [filename]例：DirecotryIndex index.html4.3.4 使新的配置生效
在上面，我们可能已经根据新的需求更改了相应的配置选项，如果我们要使得这个新的配置立即生效。我们就必须重新启动WEB服务进程。
在LINUX中，我们可以十分方便地使用命令行来使得WEB服务进程重启。
/etc/rc.d/init.d/httpd restart4.4 为用户开辟个人主页空间
如果我们利用了LINUX系统架设了一台WEB服务器，我们不仅可以存放公司的主页，而且还可以为公司的每一个员工提供一块个人主页的空间。
1． 首先，为需要个人主页空间的员工在LINUX上开设一个帐号。这样，它就拥有了一个用户主目录/home/用户帐号名。
addusr 用户帐号名passwd 用户帐号名
2． 在用户主目录下建立一个目录public_html，然后为其设置相应的权限。
cd ~用户帐号名mkdir public_htmlchmod 755 public_html
3． 确认在srm.conf文件中的UserDir命令设置的是public_html目录。
4． 让员工将自己的个人主页上传到自己用户主目录下的public_html目录中。
5． 现在就可以使用http://www.company.com/~用户帐号名来访问员工的个人主页了。4.5 用Apache实现虚拟主机服务4.5.1 什么是虚拟主机服务
所谓的虚拟主机服务就是指将一台机器虚拟成多台WEB服务器。举个例子来说，一家公司想从事提供主机代管服务，它为其它企业提供WEB服务。那么它肯定不是为每一家企业都各准备一台物理上的服务器，而是用一台功能较强大的大型服务器，然后用虚拟主机的形式，提供多个企业的WEB服务，虽然所有的WEB服务就是这台服务器提供的，但是让访问者看起来却是在不同的服务器上获得WEB服务一样。
具体地说，就是，我们可以利用虚拟主机服务将两个不同公司www.company1.com与www.company2.com的主页内容都存放在同一台主机上。而访问者只需输入公司的域名就可以访问到它想得到的主页内容。
用Apache设置虚拟主机服务通常可以采用两种方案：基于IP地址的虚拟主机和基于名字的虚拟主机，下面我们分别介绍一下它们的实现方法。以便大家在具体的应用中能够选择最合适的实现方法。4.5.2 设置实现基于IP地址的虚拟主机服务
1. 实现前提
这种方式需要在机器上设置IP别名，也就是在一台机器的网卡上绑定多个IP地址去为多个虚拟主机服务。而且要使用这项功能还要确定在你的LINUX内核中必须支持IP别名的设置，否则你还必须重新编译内核。
下面举一个拥有两个虚拟主机的服务设置，以供参考。
2．配置步骤
假设，我们用来实现虚拟主机服务的机器，首先已经为自己提供了WEB服务，现在将为新的一家公司www.company1.com提供虚拟主机服务。本篇文章来源于 Linux爱好网|www.linux-love.com 原文链接：http://www.linux-love.com/server/20080408/Linuxzuwangrumen-1--WEBfuwuqi_7583_2.shtml1） 规划IP地址：为虚拟主机申请新的IP地址。（假设本机IP地址为202.101.2.1）
www.company1.com 202.101.2.2
2) 让ISP作好相应的域名解析工作。
3) 为网卡设置IP别名：
/sbin/ifconfig eth0:0 202.101.2.2 netmask 255.255.255.0
4) 重新设置/etc/httpd/conf/httpd.conf,在文件中加入：
<VirtualHost 202.101.2.2>
ServerAdmin webmaster@yourdomain.com
Do*****entRoot /home/httpd/www.company1.com
ServerName www.company1.com
ErrorLog /var/log/httpd/www.company1.com/error.log
</VirtualHost>
5）建立相应的目录。
mkdir /home/httpd/www.company1.com
mkdir /var/log/httpd/www.company1.com/error.log
6)将相应的主页内容存放在相应的目录中即可。
3．不利因素
这种虚拟主机的实现方法有一个严重的不足，那就是，每增加一个虚拟主机，就必须增加一个IP地址。而由于IP地址空间已经十分紧张，所以通常情况下是无法取得这么多的IP地址的。而且从某种意义上说，这也是一种IP地址浪费。4.5.3 设置实现基于名字的虚拟主机服务
而基于名字的虚拟主机服务，是比较适合使用的一种方案。因为它不需要更多的IP地址，而且配置简单，无须什么特殊的软硬件支持。现代的浏览器大都支持这种虚拟主机的实现方法。当然，这也就是指一些早期的客户端浏览器也许不支持这种虚拟主机的实现方法。
正是以上原因，我们没有理由不使用基于名字的虚拟主机服务而使用基于IP地址的虚拟主机服务。
配置基于名字的虚拟主机服务需要修改配置文件： 
/etc/httpd/conf/httpd.conf，在这个配置文件中增加以下内容。
NameVirtualHost 202.101.2.1<VirtualHost 202.101.2.1>
ServerAdmin webmaster@yourdomain.com
Do*****entRoot /home/httpd/www.company1.com
ServerName www.company1.com
ErrorLog /var/log/httpd/www.company1.com/error.log
</VirtualHost><VirtualHost 202.101.2.1>
ServerAdmin webmaster@yourdomain.com
Do*****entRoot /home/httpd/www.company2.com
ServerName www.company2.com
ErrorLog /var/log/httpd/www.company2.com/error.log
</VirtualHost>也就是在基于IP地址的配置基础上增加一句：NameVirtualHost 202.101.2.1而已。在本例中，为了体现只需要增加一次，所以特别地设置了两个虚拟主机服务。
最后也是建立相应的目录，将主页内容放到相应的目录中去就可以了。
本篇文章来源于 Linux爱好网|www.linux-love.com 原文链接：http://www.linux-love.com/server/20080408/Linuxzuwangrumen-1--WEBfuwuqi_7583_3.shtml
        

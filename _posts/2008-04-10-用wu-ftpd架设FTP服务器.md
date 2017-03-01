---
layout: post
title:  "用wu-ftpd架设FTP服务器"
date:   2008-04-10 11:07:10
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
loginfails--设置登入错误可尝试的次数本篇文章来源于 Linux爱好网|www.linux-love.com 原文链接：http://www.linux-love.com/server/20080408/yongwu-ftpdjiasheFTPfuwuqi_7578.shtml当用户连线时可能打错ID或密码，这个设定可以让他打错几次以后就断线，避免有人用穷举法猜测密码。 
private--设定线上是否可以执行SITE GROUP/SITE GPASS 
当开放SITE GROUP与SITE GPASS指令时，可以用这两个指令切换到/etc/ftpgroup的群组。一般而言我们不会用到这个功能，以避免安全漏洞。 
greeting--显示Server的版本信息，用法如下： 
greeting 
当用户登入画面显示的server信息，full是预设值，包含版本号以及hostname，brief只有hostname，而terse只有FTP server ready的信息。 
barnner--设定未进入Login画面之前用户看到的信息，用法如下： 
banner<文件路径> 
这里叙述了在用户登入时，在还没打ID/Password之前要出现的信息。文件路径指的是相对于真实的路径，而不是相对于ftp的根目录。 
host--设定ftp主机名 
email--指定ftp管理者的email地址 
message--信息文件的设定，用法如下： 
message<文件>{<何时>{……}} 
这里的文件的路径是相对于ftp的根目录的，何时是指当你做了什么动作之后的反应，有几个选择： 
login(登入时) 
cwd=<目录>(进入某目录时) 
class 名称是前面已经定义过的，允许你的信息只对哪些人发出。 
而信息文件的内容除了文字以外，还可以使用以下一些事先定义好的代号： 
%T(本机时间) 
%F(目前分区所剩余的空间) 
%C(目前所在的目录) 
%E(管理者的E-mail) 
%R(客户端主机名称) 
%L(本机主机名称) 
%U(用户名称) 
%M(与我相同class用户允许多少人连线) 
%N(与我相同class用户目前有多少人连线) 
%B(绝对磁盘限制大小，目前分区(单位blocks)) 
%b(preferred磁盘限制大小，目前分区(单位blocks)) 
%Q(目前已使用的blocks) 
%I(最大可使用的inodes( 1)) 
%i(Preferred inodes限制) 
%q(目前使用的indoes) 
%H(超量使用磁盘空间的时间限制) 
%h(超量使用文件数目的时间限制) 
readme--通知用户哪些README文件已经更新 
log commands--记录用户所使用过的命令，用法如下： 
log commands<用户种类> 
log transfers--记录用户所传输的文件，用法如下： 
log transfers<用户种类><传输方向> 
设定有哪些类型的用户传输文件需要记录，包含了inbound(用户上传)和outbound(用户下载)，例如： 
log transfers anonymous,guest inbound,outbound 
log security--记录安全性，用法如下： 
log security<用户种类> 
特别用于记录某类用户关于noretrive、notar等有关安全性的记录 
log syslog--记录到系统的syslog文件 
alias--设定目录别名，用法如下： 
alias<别名字符串><目录> 
cdpath--设定cd更换目录搜索顺序 
compress,tar--设定是否自动压缩，用法如下： 
compress[……] 
tar[……] 
定义哪些人可以执行压缩以及tar 
shutdown--通知用户要关站了 
shutdown<信息文件> 
如果信息文件存在的话，当这个文件指定的某时间以后，就会拒绝连线并切断已有的连线，等时间一到就关机。这个信息文件的格式如下： 
<年><月><日><时><分><拒绝倒数><断线倒数><文字> 
daemon address--指定只监听某个IP地址，用法如下： 
daemon address 当你有许多IP的时候，使用这个选项将会取消其它任何虚拟FTP主机的设定。不设定的话，监听所有IP。 
virtual--设定虚拟FTP站台 
wu-ftpd提供了虚拟主机的功能，也就是说，在同一台机器上提供了不同FTP站台，以主机名称或IP来区分；当然你要用名称的话，还需要跟DNS配合才行。virtual有很多个设定： 
virtual
<路径> 可以是主机名或IP地址 
root指的是ftp的根目录，banner是欢迎信息，logfile指的是这个虚拟站台的log文件 
以下是一些例子： 
virtual virtual.com.bj root /home/ftp2 
virtual virtual.com.bj banner /etc/vftpbanner.2 
virtual virtual.com.bj logfile /etc/viftplog.2 
virtual
<字母> 
用户可以查到hostname跟管理者email，以下是一些例子： 
virtual 210.62.146.50 hostname virtual.site.com.bj 
virtual vritual.site.com.bj email ftpown@virtual.site.com.bj 
virtual
allow<用户>[<用户>……] 
virtual 
deny<用户>[<用户>……] 
很明显，以上两个选项是设定是否允许连线的，以下是一些例子： 
virtual virtual.site.com.bj allow * 
virtual virtual.site.com.bj deny badman 
virtual
private 
本虚拟站台拒绝anonymous用户 
defaultserver deny <用户>[<用户>……] 
defaultserver allow <用户>[<用户>……] 
当我们使用了虚拟主机，原先的deny，allow设定不知道要设哪个server，所以会无效，用defaultserver代表原来的主机 
defaultserver private 
主站台拒绝anonymous用户 
passive address--转换IP数值 
passive address<外部IP>/cidr 
passive ports--passive的ports范围 
passive ports 
pasv-allow--允许使用pasv 
pasv-allow[<地址>……] 
port-allow--允许使用port 
port-allow[<地址>……] 
mailserver--指定Upload通知的mail服务器 
incmail--指定anonymous upload的email通知地址 
virtual incmail--指定虚拟主机anonymous upload的email通知地址 
defaultserver incmail--指定预设主机anonymous upload的email通知地址 
mailfrom--通知的寄信人upload 
virtual mailfrom--虚拟主机upload通知的寄信人 
defaultserver mailfrom--预设主机upload通知的寄信人 
chmod--设定是否可以改变文件权限 
delete--设定是否可以删除文件 
overwrite--覆盖文件 
rename--重命名文件 
umask--允许设定umask 
passwd-check--设定anonymous FTP的密码检查程度，用法如下： 
passwd-check() 
设定对anonymous ftp用户的密码是否检查，none表示不检查，trivial为包含@的任意密码，rfc822则表示密码要遵循RFC822格式，enforce表示密码检查不过不允许进入，warn表示密码检查不过只出现警告信息。本篇文章来源于 Linux爱好网|www.linux-love.com 原文链接：http://www.linux-love.com/server/20080408/yongwu-ftpdjiasheFTPfuwuqi_7578_2.shtmldeny=email--拒绝特定的email当密码 
path-filer--摄定哪些文件名不可使用 
path-filer<错误信息文件><允许字符><不允许字符> 
upload--设定upload权限 
upload[absloute/relative][ >[dirs/nodirs][d_mode] 
用来对我们要设定的目录做权限设定： 
absoulte/relative使用绝对路径或是相对路径 
 
root-dir指的是对哪些root-dir的人，也就是chroot后的登入目录，应用这个规则 
设定的目录指的就是我们要限制的目录 
yes/no指得是能否在此目录下开新文件 
owner,group指出是开出来的文件拥有者及群组 
Mode指的是文件权限 
dirs/nodirs指的是能否开新目录 
d_mode设定建立新目录时目录的权限，如果不设定会根据mode来设定 
thoughput--控制下载速度 
thoughput<子目录列表><文件><远端地址列表> 
对远端的地址，控制他抓某个子目录下的某些文件时的速度，例如： 
thoughput /e/ftp * * oo - * 
thoughput /e/ftp /sw* * 1024 0.5 * 
thoughput /e/ftp sw* readme oo - * 
thoughput /e/ftp sw* * oo - *.foo.com 
以上的设定你是否能够看出来呢？oo表示不限制bytes/sec，-或是1.0都是代表一倍。第一行的意思是说，在/e/ftp下面的文件不限制下载速度；第二行说，在/sw*下面的任何文件限速为1024bytes/sec* 
0.5=512bytes/sec；第三行又把readme文件的限速取消；最后一行则对*.foo.com开放全速。 
anonymous-root--对某class设定匿名用户的根目录 
anonymous-root[] 
guest-root--预设一个guest用户根目录 
guest-root[] 
其中用于指定uid的范围 
deny-uid，deny-gid--拒绝某段UID(GID)范围 
allow-uid，allow-gid--允许某段UID(GID)范围 
restricted-uid，restricted-gid--限制用户不能离开他的登录目录 
unrestricted-uid，unrestricted-gid--用户可以离开他的登录目录 
dns refuse_mismatch--设定DNS查到名称与用户设定不符的动作 
dns refuse_mismatch<信息文件>[override] 
当用户使用未注册IP时，拒绝他的连线，override则是不理会错误而让他连线，信息文件则是我们要给用户看的。 
dns refuse_no_reverse--设定无反查记录拒绝连线 
dns refuse_no_reverse<信息文件>[override] 
当用户的IP反查无记录时，拒绝他的连线 
dns resolveoptions--设定DNS解析选项 
dns resolveoptions[options] 
这里可以设定DNS解析选项 
⒉/etc/ftphosts 
ftphosts文件其实跟ftpaccess里面的access，deny很像，它是特别用来设定某些ID的连线，它没有class定义，所以必须是真实用户。 
allow|deny<用户><地址>[<地址>……] 
以下是一些例子： 
allow rose 140.0.0/8 
deny jack 140.123.0.0:255.255.0.0 
允许rose从140.*.*.*进来，拒绝jack从140.123.*.*上来 
⒊/etc/ftpservers 
这个文件控制了当你有不同的IP/hostname的时候，进来的连线使用哪一个配置文件。例如： 
10.196.145.10 /etc/ftpd/ftpaccess.somedomain/ 
10.196.145.200 /etc/ftpd/ftpaccess.someotherdomain/ 
some.domain internal 
10.196.145.20 /etc/ftpd/config/faqs.org/ 
ftp.some.domain /etc/ftpd/config/faqs.org/ 
⒋/etc/ftpusers 
在这个文件里记录的用户禁止使用FTP 
⒌/etc/ftpgroups 
给SITE GROUP指令使用，线上切换group。SITE EXEC容易造成安全漏洞，一般我们都不开放。 
⒍/etc/ftpconversions 
用来做tar、compress、gzip等动作指令配置文件，只要用预设即可，如果你不开放即时压缩打包，也可以把内容清除。本篇文章来源于 Linux爱好网|www.linux-love.com 原文链接：http://www.linux-love.com/server/20080408/yongwu-ftpdjiasheFTPfuwuqi_7578_3.shtml
        

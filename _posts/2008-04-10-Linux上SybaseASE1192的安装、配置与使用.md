---
layout: post
title:  "Linux上SybaseASE11.9.2的安装、配置与使用"
date:   2008-04-10 11:19:10
categories: 数据库
tags:
---

* content
{:toc}

在开篇之前，先讲题外话，说一说我为什么选择Linux Sybase，兴许大家会有些共鸣。 我不是计算机科班出身，也不是IT业中人，只是个电脑爱好者，玩游戏，装程序，上网瞎逛，DOS，Win31，Win95，WinNT，样样都捣鼓一下。虽说也学了一些杂七杂八的东西，但看着家里先后花了近两万块捧回来的老中青三台电脑（从486、Pentium到赛扬）一天天地贬值，到如今连三千块也不值，心里不由想到该学一些有用的本领了，也算是对得起自己的巨额投资......Linux上Sybase ASE11.9.2的安装、配置与使用之新手上路篇
胡国勇（huguoyong@163.net）
在开篇之前，先讲题外话，说一说我为什么选择Linux Sybase，兴许大家会有些共鸣。 我不是计算机科班出身，也不是IT业中人，只是个电脑爱好者，玩游戏，装程序，上网瞎逛，DOS，Win31，Win95，WinNT，样样都捣鼓一下。虽说也学了一些杂七杂八的东西，但看着家里先后花了近两万块捧回来的老中青三台电脑（从486、Pentium到赛扬）一天天地贬值，到如今连三千块也不值，心里不由想到该学一些有用的本领了，也算是对得起自己的巨额投资。 
学什么好呢？数据库容易入门，用途又广，网络社会又来了，就学数据库在网络上的应用吧。于是我就选择了Visual Foxpro开发前端客户程序，后台使用SQL数据库管理系统这种流行的客户机/服务器模式来学。SQL数据库有很多，选哪个厂家，什么平台呢？开始我想学WinNT MS SQL SERVER，挺流行的，参考书又多，可是哪两个软件价格惊人，虽然有D版，但版权管得越来越严，咱还是用正版软件吧免费操作系统就用Linux，SQL数据库就选Sybase了。为什么呢？1、Sybase是世界著名的数据库厂商，对Linux很支持，Sybase ASE for Linux就推出了多个版本，其网站产品下载、技术手册、疑难解答挺齐全的，遇上问题容易找到解决办法。2、Sybase ASE与MS SQL SERVER是近亲，MS SQL SERVER的早期版本就是Sybase公司为微软公司开发的。两者体系相近，管理方式、命令、函数、工具差不多，你看一看两家的技术文档就知道了（我曾经买了一套MS SQL SERVER 6.5的技术手册）。学会了Sybase ASE，转头去学MS SQL SERVER，应该比较容易上手吧。 
对于我来说，Sybase ASE和Linux都是刚入门，很多地方还是一知半解、迷迷糊糊，主要靠自己去摸索，去走出一条路来。文中讲的是我的学习收获，不是标准答案，其中错误肯定不少，请大家指正。下面就讲讲我学Sybase ASE for Linux的过程，仅供参考。 一、我学Sybase ASE for Linux用到的资源（一）软件 
1、Sybase Adaptive Server Enterprise 11.9.2（以下称ASE11.9.2）的RPM包，共7个文件： 
（1）sybase-ase-11.9.2-3.i386.rpm 
（2）sybase-chinese-11.9.2-3.i386.rpm 
（3）sybase-common-11.9.2-3.i386.rpm 
（4）sybase-doc-11_9_2-1_i386.rpm 
（5）sybase-monserver-11.9.2-3.i386.rpm 
（6）sybase-openclient-11.1.1-3.i386.rpm 
（7）sybase-sqlremote-6.0.2-1.i386.rpm 
我是从Sybase公司的网站上面下载的，需要注册一下才能下载。网址是： 
http://www.sybase.com 
2、ASE11.9.2的补丁，共两个文件： 
（1）EBF9820.tgz补丁程序 
（2）EBF9820.ltr文档 
我是从Sybase中国公司的网站上面下载的，网址是： 
http://www.sybase.com.cn 
3、Windows平台的ASE管理工具 
（1）asentlnx.exe。这是从Sybase公司的网站下载的。安装后有如下工具： 
①Sybase Central管理ASE数据库的图形界面工具 
②SQL Modeler强大的客户/服务器应用程序设计工具（可惜我不会用！） 
③Dsedit设置客户端接口文件（sql.ini）的图形界面工具 
④OC OS Config Utility设置客户端机器的环境变量、网络驱动程序等的工具 
⑤Sybase System11 ODBC驱动程序 
（2）sybase11.exe。这是我从http://www.linuxbyte.net网站下载的。安装后有如下工具： 
①Sybase SQL Server Manager管理ASE数据库的图形界面工具 
②Sybase Monitor Client监控SQL Server的图形界面工具 
③SQLEDIT设置客户端接口文件（sql.ini）的图形界面工具 
④SYBPING测试客户端与SQL Server的连通情况的图形界面工具 
⑤WISQL32执行SQL查询的图形界面工具 
Sybase SQL Server Manager和WISQL32在我的系统中执行有些小问题，不知何故。 
4、图形界面的SQL查询工具 
（1）WinSQL Lite 3.8。这是Windows平台上的软件，推荐使用，是免费版，可在http://www.indus-soft.com/winsql下载，需要在网站上注册，取得一个注册码。WinSQL还有一个功能更强大的专业版，但购买一个单用户许可证要149美金。 
（2）WISQL32。上面已讲过。 
（3）sybquery-0.2.0.tar.gz。这是X-Windows上的软件。可在http://www.linuxbyte.net网站下载。 
（4）jisql。这是Sybase公司开发的基于Java的图形界面的SQL查询工具，可在Windows平台和Linux平台运行。可惜我一直没能用起来。 
5、Ribo工具。这是Sybase公司开发的基于Java的图形界面工具，用来捕获和显示使用TDS表格式数据流协议的客户端和服务器之间传递的数据，可在Windows平台和Linux平台运行。我不懂怎么用。 
6、Windows平台的终端工具。我选用NetTerm4.2登录Linux，命令可复制、粘贴，可回卷屏幕翻查，方便多了。可在华军软件网站（http://www.newhua.com）下载。（二）技术文档 
1、英文文档：在/opt/sybase-11.9.2/doc/PDF下面有三十个文档（其中两个重复）： 
（1）aserb.pdfLinux、Intel平台的Sybase Adaptive Server Enterprise 11.9.2版本公告 
（2）ocsrb.pdfLinux、Intel平台的Open Client及Server产品（11.1.1版）版本公告 
（3）asem1192.pdfSybase Adaptive Server Enterprise Monitor 11.9.2版本公告 
（4）asefun92.pdfSybase Adaptive Server Enterprise 11.9.2新功能 
（5）whatsup.pdfSybase ASE的新特点 
（6）uxinstall.pdfLinux、Intel平台的Sybase ASE安装指南 
（7）aseuxcfg.pdf配置Unix平台的Adaptive Server Enterprise 
（8）u_config.pdfUnix平台的Open Client与Server配置指南本篇文章来源于 Linux爱好网|www.linux-love.com 原文链接：http://www.linux-love.com/database/20080408/LinuxshangSybase-ASE11-9-2deanzhuang-peizhiyushiyongzhixinshoushanglupian_7628.shtml（9）sag.pdfSybase ASE系统管理指南 
（10）perform.pdfSybase ASE性能优化指南 
（11）mngmonas.pdfSybase ASE的管理与监控 
（12）utprogux.pdfSybase ASE的Unix工具组 
（13）sqlref_1.pdfSybase ASE参考手册（卷1：命令与函数） 
（14）sqlref_2.pdfSybase ASE参考手册（卷2：存储过程） 
（15）sqlref_3.pdfSybase ASE参考手册（卷3：数据类型与系统表） 
（16）svrtsg1.pdfSybase ASE故障解除及错误信息手册（卷1） 
（17）svrtsg2.pdfSybase ASE故障解除及错误信息手册（卷2） 
（18）svrtsg3.pdfSybase ASE故障解除及错误信息手册（卷3） 
（19）navigate.pdfSybase ASE任务导航与命令速查 
（20）mg_gde.pdf迁移到Sybase ASE11.5 
（21）sqlug.pdfSybase ASE Transact-SQL用户指南 
（22）monbook.pdfSybase ASE Monitor Server用户指南 
（23）histserv.pdfASE Monitor Historical Server用户指南 
（24）clilib.pdfSybase Adaptive Server Monitor的Client Library程序员指南 
（25）glossary.pdfSybase Adaptive Server Enterprise词汇表 
（26）omnintro.pdfOmniConnect简介 
（27）omni_ug.pdfOmniConnect Component Intergration Services用户指南 
（28）connim.pdf、imug.pdfInfoMaker用户指南 
（29）sqlremote.pdf使用SQL Remote进行数据复制 
2、中文文档：在Sybase中国公司网站下载，共6个文档，虽然是关于ASE12的，仍有很大参考价值： 
（1）Sybase ASE12中文参考手册(卷1：构件块) 
（2）Sybase ASE12中文参考手册(卷2：命令) 
（3）Sybase ASE12中文参考手册(卷3：过程) 
（4）Sybase ASE12中文参考手册(卷4：表格及手册索引) 
（5）使用Adaptive Server分布式事务管理功能 
（6）Adaptive Server Enterprise中的Java（三）网站、论坛、新闻组。当你遇到问题时，它们就是你的救星。 
1、网站：首选Sybase公司的网站，资料非常丰富，可就是英文的，真头疼，慢慢看吧。Sybase中国公司的网站上也有一些技术支持。其次是http://www.linuxbyte.net网站，Linux上的数据库软件真不少，技术资料也多，每天要去。还有http://www.edbarlow.com（Sybase应用软件）、http://perso.wanadoo.fr/dbadevil/（Sybase软件、技术资料）、http://www.mbay.net/~mpeppler（一位Sybase专家的主页，有很多关于Sybase的链接，ASE on Linux FAQ）等等。 
2、论坛：当然是中国Linux论坛了（http://www.linuxforum.net）。Linux平台数据库版一天去好几趟。 
3、新闻组：有关Linux上的ASE的新闻组服务器：forums.sybase.com，外国高手很多。（四）书籍 
1、《Sybase数据库速成培训》，鲍永刚、龙冬云编，电子工业出版社出版。该书简明介绍了Sybase数据库的组成、数据库系统管理和T-SQL语言，并用大量例子说明建立和管理数据库的操作过程。此书堪称价廉物美，把它吃透，Sybase就算基本入门了。 
2、《SYBASE SQL Server 11参考大全》，R.兰金斯等著，希望图书创作室译，宇航出版社出版。此书翻译得一般，我一般将它当作百科全书来翻查。 
3、《14天自学教程SQL》，Bryan Morgan、Jeff Perkins著，邓洪涛、陈越译，清华大学出版社出版，是关系数据库理论和SQL语言的入门书。 二、我安装ASE11.9.2的过程（一）软硬件要求 
Linux上的ASE11.9.2需要Linux核心版本为2.2.5、glibc-2.07-29或以上，使用TCP/IP协议，内存推荐128M以上，磁盘空间需要200M以上。 
我最牛的机子配置是赛扬300A、192M、10G，就用它来装了ASE11.9.2。先把Redhat7.2装好，主机名是DBSERVER，IP地址是192.168.0.1，在硬盘上划出1G的分区作为ASE11.9.2的数据库存储空间，格式化成ex3文件系统，挂在根目录下的/db。 
本来，ASE11.9.2手册中说在正式应用中数据库设备必须使用裸设备（Raw Device），并推荐使用硬盘分区建立数据库设备，强调不能使用操作系统文件，否则系统出现故障后难以恢复（因为操作系统高速缓存不会马上把数据写入磁盘，一旦系统崩溃，内存中的数据丢失，就破坏了数据库的参照完整性）。但据ASE11.9.2的版本公告讲，Linux上的ASE11.9.2不支持裸设备，为保证系统能正常恢复，Linux上的ASE11.9.2使用O_SYNC标志打开数据库设备，以保证数据尽快写入磁盘，但是这样会影响系统的性能。（二）使用RPM工具把ASE产品解包复制到硬盘中 
1、在Linux控制台模式下以root用户登录。 
2、装载光盘（我把所有软件刻成一张光盘）： 
#mount -t iso9660 /dev/cdrom /mnt/cdrom 
3、首先解包sybase-common-11.9.2-3.i386.rpm。 
#rpm -hiv /mnt/cdrom/sybase-common-11.9.2-3.i386.rpm 
4、解包复制其他产品。 
#rpm -hiv /mnt/cdrom/sybase-ase-11.9.2-3.i386.rpm 
#rpm -hiv /mnt/cdrom/sybase-chinese-11.9.2-3.i386.rpm 
#rpm -hiv /mnt/cdrom/sybase-openclient-11.1.1-3.i386.rpm 
#rpm -hiv /mnt/cdrom/sybase-doc-11_9_2-1_i386.rpm 
#rpm -hiv /mnt/cdrom/sybase-monserver-11.9.2-3.i386.rpm 
#rpm -hiv /mnt/cdrom/sybase-sqlremote-6.0.2-1.i386.rpm 
5、卸载光盘。 
#umount /dev/cdrom 
6、RPM工具在解包时创建了sybase用户和sybase组。此时sybase用户的帐号是锁住的，必须将其解锁并更改密码。然后将/db的读写权限只授予sybase用户。 
7、修改系统内存配置。在root用户登录文件中加入以下语句（以bash用户，.bash_profile为例。更改系统内存值为60M）：echo "62914560" > /proc/sys/kernel/shmmax 
8、重新启动系统。 
9、在Linux控制台模式下以sybase用户登录，会自动执行一文件设置环境变量等。在/db下建一目录/sybsystem。 
10、如果你使用网络，请配置好网卡。即使你不使用网络，也要在loopback状态下检查网络配置是否正确，方法如下：在主机上用telnet localhost命令登录，不必退出，用同样的命令再登录一次，然后用两次exit命令退出系统。如果执行正常，网络配置就OK了。（三）在X-Windows中使用srvbuild工具配置ASE产品 
1、用sybase用户登录X-Windows，执行sybase安装目录（/opt/sybase-11.9.2）下/bin/srvbuild命令。本篇文章来源于 Linux爱好网|www.linux-love.com 原文链接：http://www.linux-love.com/database/20080408/LinuxshangSybase-ASE11-9-2deanzhuang-peizhiyushiyongzhixinshoushanglupian_7628_2.shtml2、在srvbuild窗口中，选择要安装Server类型。我把四种Server都选上。 
3、给Server命名。我将Adaptive Server命名为TEST，相应地，Backup Server自动命名为TEST_back，Monitor Server命名为TEST_mon，XP Server命名为TEST_XP。点击OK按钮，进入各Server的配置过程。 
4、配置Adaptive Server。填写或选择以下内容： 
Master device path（主设备路径）：/db/sybsystem/master.dat 
Master device size（MB）（主设备大小)：60 
Master database size（MB）（主数据库大小）：20 
Sybsystemprocs device path（系统存储过程设备路径）：/db/sybsystem/systemprocs.dat 
Sybsystemprocs device size（MB）（系统存储过程设备大小）：60 
Sybsystemprocs database size（MB）（系统存储过程数据库大小）：60 
Error log path（错误日志路径）：/opt/sybase-11.9.2/install/TEST.log 
Transport type（传输协议类型）：tcp 
Host name（主机名）：192.168.0.1 
Port number（监听端口号）：4100 
点击OK按钮，进入下一配置过程。 
5、配置Backup Server。填写或选择以下内容： 
Error log path：/opt/sybase-11.9.2/install/TEST_back.log 
Tape configuration file：/opt/sybase-11.9.2/backup_tape.cfg 
Language：（不填） 
Character set：（不填） 
Maximum number of network connections：25 
Maximum number of server connections：20 
Transport type：tcp 
Host name：192.168.0.1 
Port number：4200 
点击OK按钮，进入下一配置过程。 
6、配置Monitor Server。填写或选择以下内容： 
Maximum number of connections：5 
Error log path：/opt/sybase-11.9.2/install/TEST_mon.log 
Configuration file path：/opt/sybase-11.9.2/install/TEST_mon.cfg 
Share memory directory：/opt/sybase-11.9.2 
Transport type：tcp 
Host name：192.168.0.1 
Port number：4300 
点击OK按钮，进入下一配置过程。 
7、配置XP Server。填写或选择以下内容： 
Transport type：tcp 
Host name：192.168.0.1 
Port number：4400 
点击Build Server按钮，开始创建Server，这时出现一个窗口，你可以看到整个创建过程。如果有显示以下类似信息，表示创建Server成功： 
…… 
Server TEST was successfully created. 
Done. 
…… 
8、创建Server成功后，系统就会问你是否将Server本地化（Localize），即是用另外一种语言代替默认的us_english language，以及改变默认的iso_1字符集和Binary索引顺序。我的选择是NO。为什么呢？我曾经把中文（eucgb）设为默认字符集，反而不支持中文大字集，因为eucgb是基于GB2312标准的。我查了Sybase的手册中一些关于本地化的说明，得出的印象是，在ASE中有Unicode转换机制，可以转换来自不同字符集的服务器或客户端的数据。我的应用也证明，使用ASE的默认的语言、字符集、索引顺序来处理中文是可行的。 
9、安装成功后要做的几件事。首先在Linux控制台模式下以sybase用户登录。 
①确认Server是否在运行。使用$SYBASE/install/下的showserver命令（$SYBASE表示sybase的安装目录），应该可看见系统有几个sybase相关进程。或者用$SYBASE/bin/下的isql -Usa -P -STEST命令来登录Server，应该可以看见isql的提示符1>，再键入exit就可以退出了。 
②设置sa帐户的口令。装好Server后，系统自动建立sa用户，即系统管理员，对整个系统拥有最大的权力，但这时sa的口令是空的，必须马上更改。 
$SYBASE/bin/isql -Usa -P -STEST 
1>sp_password null,新口令 
2>go 
③关闭主设备缺省状态。否则用户的数据库会安装在主设备上。 
$SYBASE/bin/isql -Usa -STEST 
Password：（输入新口令） 
1>sp_diskdefault master，defaultoff 
2>go（四）安装语法数据库和示例数据库 
先建立一个放置语法数据库和示例数据库的数据库设备，大小为10M，并设置为缺省状态。 
$SYBASE/bin/isql -Usa -STEST 
Password：（输入口令） 
1>disk init name = "sybsyntaxdev", 
2>physname = "/db/sybsystem/sybsyntaxdev.dat", 
3>vdevno = 2,size = 5120 
4>go 
1>sp_diskdefault sybsyntaxdev，defaulton 
2>go 
1、安装sybsyntax语法数据库。这是通过$SYBASE/scripts/ins_syn_sql这个脚本文件来安装的。但ins_syn_sql需要修改一下，去掉开头用来指定缺省数据库设备的一段语句，加入create database sybsyntax一句（具体请参考《Linux、Intel平台的Sybase ASE安装指南》7-14页、7－15页）。然后执行以下命令： 
$SYBASE/bin/isql -Usa -P口令 -STEST < $SYBASE/scripts/ins_syn_sql 
语法数据库安装好后，可用系统存储过程sp_syntax查询Transact-SQL语言、系统存储过程、Sybase工具的使用帮助。例如要查询select命令的用法： 
$SYBASE/bin/isql -Usa -STEST 
Password：（输入口令） 
1>sp_syntax "select" 
2>go 
2、安装pubs2、pubs3示例数据库。技术文档中的例子就是来自示例数据库。执行以下命令： 
$SYBASE/bin/isql -Usa -P口令 -STEST < $SYBASE/scripts/installpubs2 
$SYBASE/bin/isql -Usa -P口令 -STEST < $SYBASE/scripts/installpubs3（五）安装ASE补丁 
据Sybase公司讲，EBF9820.tgz修正了ASE11.9.2已知的一些问题，建议尽快安装。 
1、先关闭Server。 
$SYBASE/bin/isql -Usa -STEST 
Password：（输入口令） 
1>shutdown SYB_BACKUP &&关闭Backup Server 
2>go 
1>shutdown &&关闭Adaptive Server 
2>go 
$SYBASE/bin/isql -Usa -P口令 -STEST_mon 
1>sms_shutdown &&关闭Monitor Server 
2>go 
2、在Linux控制台模式下以root用户登录。 
#mkdir /tmp/SWR &&建立放置补丁的临时目录 
#mount -t iso9660 /dev/cdrom /mnt/cdrom &&装载光盘 
#cp /mnt/cdrom/EBF9820.tgz /tmp/SWR &&将补丁复制到临时目录 
#cd /tmp/SWR 
#gunzip -S .tgz EBF9820.tgz 
#tar xvf EBF9820.tar本篇文章来源于 Linux爱好网|www.linux-love.com 原文链接：http://www.linux-love.com/database/20080408/LinuxshangSybase-ASE11-9-2deanzhuang-peizhiyushiyongzhixinshoushanglupian_7628_3.shtml#rpm -hiv /tmp/SWR/ebf9820/RPMS/sybase-SWR-9820-1.i386.rpm 
重新设置sybase用户对$SYBASE的读写权限。 
退出root用户登录。 
3、以sybase用户登录，启动Adaptive Server。 
$SYBASE/install/startserver -f RUN_TEST呵呵，费了好大的劲啊，总算装好了。怎么用呢？
三、我使用ASE11.9.2Sybase数据库管理系统体系庞大，功能完整，我刚入门，很多新概念没有消化、很多东西不知道是做什么用，只能讲讲自己已经用上的。我日常的使用是这样，Linux主机在打开电源后就不管了，事情都是在Windows平台上做的，或是用NetTerm登录Linux，做一些管理任务和使用isql工具连接ASE进行数据库操作，或是用WinSQL工具进行数据库操作，或是用Sybase Central管理数据库。（一）Server的启动与关闭 
先说说四种Server的作用。Adaptive Server Enterprise是基于客户机/服务器体系结构的关系数据库管理系统，其余三种Server是辅助它的。Backup Server负责数据库的备份（转储）和恢复（加载）。Monitor Server负责提供ASE的运行情况和性能统计数据。XP Server负责管理和执行扩展存储过程（ESPs）。 
1、Server的启动 
有两种方式：一是在Linux控制台模式发出命令启动Adaptive Server、Backup Server、Monitor Server，二是Linux启动时自动启动以上三种Server。XP Server是由Adaptive Server在调用扩展存储过程时启动的。 
我一般是这样启动的，以sybase用户登录（对主设备所在的/db有读写权限），发出如下命令： 
$SYBASE/install/startserver -f RUN_TEST &&启动Adaptive Server 
$SYBASE/install/startserver -f RUN_TEST_back &&启动Backup Server 
以上两个命令也可合起来：$SYBASE/install/startserver -f RUN_TEST -f RUN_TEST_back，这样就同时启动了Adaptive Server和Backup Server。 
启动Monitor Server，使用命令：monserver -STEST -MTEST_mon -Usa -P口令 
Servedr启动后，要定时查看日志（在$SYBASE/install目录下的TEST*.log文件），以便发现问题及时解决。 
2、Server的关闭 
以sybase用户登录，执行以下命令： 
$SYBASE/bin/isql -Usa -STEST 
Password：（输入口令） 
1>shutdown SYB_BACKUP &&关闭Backup Server 
2>go 
1>shutdown &&关闭Adaptive Server 
2>go 
关闭Backup Server后，至少等30秒后才可以重新启动它。 
在缺省状态下，Monitor Server会监测到Adaptive Server停止运行，然后自动关闭。你也可以手动关闭Monitor Server，执行以下命令： 
$SYBASE/bin/isql -Usa -STEST_mon 
Password：（输入口令） 
1>sms_shutdown 
2>go（二）isql的使用 
$SYBASE的bin子目录中有一些实用工具，其中最有用的是isql，利用它可连接Server进行数据库操作。其语法如下： 
isql -U登录名 -P口令 
进入系统后，系统显示序号和大于号提示： 
1> 
这时用户可以输入命令，每个命令既可在一行内输入，也可在多行内输入，每行结束时按回车键。一个命令输入完毕时，在新的一行输入go并按回车键，这时命令开始执行并在屏幕显示执行结果。上面已经有很多使用isql的例子了。isql是在Linux控制台模式下的命令行工具，使用起来毕竟不太方便（如果用NetTerm登录Linux，再使用isql，就比较好一点）。我常用的是Windows平台上的WinSQL软件。（三）Sybase的有关概念 
1、数据库设备（Device）：Sybase的数据库和事务日志都是建立在数据库设备上的，它可以是物理磁盘、磁盘分区或操作系统文件。使用disk init命令建立数据库设备，使用diskdefault命令指定缺省数据库设备，并且可以指定多个缺省数据库设备。建立数据库时不指定数据库设备，则在缺省数据库设备上建立。例如执行命令： 
$SYBASE/bin/isql -Usa -STEST 
Password：（输入口令） 
1>disk init name = "userdev", &&设备名字为userdev 
2>physname = "/db/sybsystem/userdev.dat", &&设备文件名为userdev.dat 
3>vdevno = 3, &&设备号为3 
4>size = 51200 &&大小为100M（51200块，1块＝2k） 
5>go 
1>sp_diskdefault userdev，defaulton &&指定为缺省数据库设备 
2>go 
2、数据库（Database）：是表及其相关数据和操作规则及完整性约束条件的集合，包括以下数据库对象：表（Tables）、参照完整性约束、核对完整性约束、规则、缺省值、存储过程、触发器、视图。因此，数据库是一个容器，只有先建数据库，才能建表。一个数据库可以放在多个数据库设备上，一个数据库设备可以放置多个数据库。具体内容请看看讲关系数据库的书。 
3、事务日志：对数据库的每次修改，都可被自动记录在一个系统表中，这个系统表就叫事务日志。任何修改总是先记录日志，然后才做实际的修改。事务日志保证了在出现故障时可以将数据库恢复到出错前的状态。数据库的事务日志最好不要跟数据库放在同一设备上。 
4、用户：Sybase的用户分为两种，一种是SQL服务器用户（登录账号），另一种是数据库用户。SQL服务器用户sa是系统管理员，对整个系统有操作权。其他SQL服务器用户都是由系统管理员创建，只有SQL服务器用户才可登录进入系统。数据库用户首先必须是SQL服务器用户，当一个SQL服务器用户创建了一个数据库或被增加为某一数据库的用户时，他才成为相应数据库的数据库用户。（四）Sybase的Windows平台客户端软件的使用 
以asentlnx.exe为例。 
1、安装 
在Windows平台上，执行asentlnx.exe，解压缩出一大堆文件到临时目录。执行临时目录中的setup.exe，一直Next下去就行了。装好后在开始菜单建有Sybase程序组，里面有Sybase Central、Dsedit等工具。我的客户端软件是装在C:Sybase目录下的。 
2、配置客户端的接口文件 
客户端软件要与数据库服务器（Server）通讯，首先得知道局域网中服务器的地址。这就需要我们为客户端软件提供一本通讯录接口文件，即是C:Sybaseinisql.ini文件。这个接口文件记录了与服务器通讯所使用的协议、地址、端口、服务类型等信息。而编写这本通讯录的工具就是Dsedit。 
通过Dsedit，可以在sql.ini中为多个Server建立entry（接口）。例如，我们要为名叫TEST的Server建立entry，可以这样操作：本篇文章来源于 Linux爱好网|www.linux-love.com 原文链接：http://www.linux-love.com/database/20080408/LinuxshangSybase-ASE11-9-2deanzhuang-peizhiyushiyongzhixinshoushanglupian_7628_4.shtml（1）启动Dsedit，出现一个窗口，点击OK按钮就可以了。 
（2）在Server Object菜单栏中选择Add，出现Input Server Name对话框，输入TEST，点击OK按钮。 
（3）在DSEDIT1-InterfaceDriver窗口中，在左边的Server框中选择TEST行，在右边的框中选Server Address行，右击，选择快捷菜单中的Modify Attribute...项，出现Network Address Attribute窗口，点击Add按钮，出现Input Network Address For Protocol对话框，点击Add按钮，Protocol项选TCP，Network Address项填入192.168.0.1，4100，点击OK按钮，退回DSEDIT1-InterfaceDriver窗口中， 
（4）在右边的框中选Server Address行，右击，选择快捷菜单中的Ping Server项，出现Ping窗口，点击Ping命令按钮，如果出现Open Connection to server at (192.168.0.1,4100) succeeds…的提示，表示配置成功了。 
3、使用Sybase Central 
Sybase Central是用于管理数据库及相关产品的Windows平台工具，可用它管理服务器、数据库中的对象（表、视图、存储过程等等），还能完成通常的创建数据库、表、用户等管理任务。Sybase Central通过提供类似Windows 95资源管理器的易于使用的图形用户界面，简化了这些任务，例如，删除数据库表，只要在主窗口中选中它并单击删除。通过提供向导，Sybase Central 帮助您完成更复杂的任务，向导一步一步地指导你完成任务。有了它，你可以基本摆脱使用isql工具发出SQL命令来管理数据库，要知道用Create table之类的命令是很累人的，不过建议你还是要研究这些命令哦，这可是基础啊，因为弄明白了这些命令的参数，才能用好Sybase Central！ 
第一次启动Sybase Central，可能会遇到点麻烦，系统会提示Unable to load language DLL "scsslgzh"。这主要是缺少提供中文支持的DLL文件，你可以将C:Sybaseasep目录中的scsslgen.dll文件改名为scsslgzh.dll，Sybase Central就可以正常启动了。
四、我的ASE＋VFP客户机/服务器应用
经过摸索，ASE＋VFP客户机/服务器这种模式已在我的单位初步应用起来了。现在ASE上有一个应用数据库，VFP开发的前端程序可同时在多台机器上运行。我正准备把单位这些年VFP下的数据往ASE上迁移，再修改一下原来的应用程序，使之能适应网络上的多用户环境。（一）Sybase System11 ODBC驱动程序与数据源 
在Windows平台上，Sybase公司的软件有自己的专用文件与ASE进行连接和交互操作，而其他公司的软件怎样与ASE连接和交互操作呢？一条途径是通过Sybase公司提供的ODBC（公开数据库接口）驱动程序。通过这个ODBC驱动程序，我们可建立数据源（Data Source）,供应用程序使用，使之能够处理ASE上的数据。 
下面讲怎么建立一个数据源。在Sybase程序组中启动ODBC Data Source Administrator（或在控制面板启动ODBC Data Source项），点击Drivers选项卡，应该有Sybase System 11一行，这是我们安装asentlnx.exe时装上的。选择User DSN选项卡，点击Add按钮，出现Create New Data Source窗口，选择Sybase System 11一行，点击完成按钮，跟着出现ODBC Sybase Driver Setup窗口。在General选项卡中，在Data Source Name栏填入数据源的名字，例如DBSERVER，在Server Name栏填入你要连接到Adaptive Server的名字，例如TEST，在Database Name栏填入默认要连接的数据库名字，然后点击确定按钮就好了。（二）远程视图与SPT 
数据源建好后，VFP应用程序就可以用它来访问和更新服务器上的数据了。在VFP中，可以使用远程视图和SPT两种方法访问远程数据。使用远程视图是最简单、方便的方法，你可以象使用VFP本地表一样使用远程视图。SPT（SQL pass-through）是直接把SQL语句发送给服务器执行，能够在很大程度上提高客户机/服务器应用程序的性能。我目前对ASE还不熟，开发程序时只是使用远程视图。我单位原来VFP下的数据就是使用远程视图，通过自编的一个小程序升迁到服务器上的（本来Sybase提供了一个批拷贝工具bcp，用于表与操作系统文件之间的数据导出导入的，但我用起来总有问题）。 
本篇文章来源于 Linux爱好网|www.linux-love.com 原文链接：http://www.linux-love.com/database/20080408/LinuxshangSybase-ASE11-9-2deanzhuang-peizhiyushiyongzhixinshoushanglupian_7628_5.shtml
        

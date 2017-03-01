---
layout: post
title:  "kdevelopforC++快速入门"
date:   2007-06-19 18:22:31
categories: 默认分类
tags:
---

* content
{:toc}

  sinox下面kdevelop for C++快速入门 
   
  上次我写了vc不适合开发软件这篇文章,并提到kdevelop这个开发工具.那么这个开发工 
  具是否比vc先进呢?下面你将找到答案.你将发现在sinox下面开发软件是那么简单方便! 
  即使你不开发c++程序,你也要用kdevelop!kdevelop是sinox下面标准可视化开发工具! 
   
  这是本站为sinox用户设计的kdevelop学习教程,旨在帮助sinox用户迅速掌握kdevelop 
  开发工具,这是国内最完整的kdevelop中文入门教程. 
   
  kdevelop for C++是一个可视化开发工具,跟C++Builder,Delphi类似. 
   
  运行kdevelop的IDE for c/c++.kdevelop版本是3.2.2(使用kde3.4.2) 
   
  1.建立工程 
  选择工程菜单的新建工程 
  选择c++/Kde/Appliation Framework,输入应用程序名称和所在路径.然后next直到完成 
  ,选项自己根据需要设置. 
   
  2.编译执行 
  现在可以执行编译菜单的执行程序,他会用automake配置程序,然后编译执行. 
  这是时会出现如下错误! 
  UNAME_VERSION = Sinox 1.2-RELEASE #0: Mon Jan 9 01:22:17 UTC 2006 
  root@sinox.shouji88.com:/usr/obj/usr/src/sys/GENERIC 
  configure: error: cannot guess build type; you must specify one无法猜测编译 
  操作系统类型,请指定一个! 
  *** 退出状态：1 *** 
  这是因为这个kdevelop是在freebsd下编译开发的,automake针对freebsd设置编译选项. 
  而sinox还没有自己的编译选项!所以必须加入! 
  我用替换方法把configure和admin/config.guess两个文件freebsd区分大小写替换 
  ,configure用freebsd,config.guess用FreeBSD替换. 
  这两个文件是通用的,下次复制到新的项目即可!你要在sinox编译其他开源软件也可能 
  要这样做. 
  我运行了模板simple KDE Application和simple Designed Based KDE Application获 
  得通过.Appliation Framework没通过. 
  源代码在src,翻译在po.通过工程菜单运行添加翻译,在po目录下生成一个 
  zh_CN.GB2312.po,双击用Kbabel打开.关于翻译以后再补充.翻译函数用i18N. 
  源代码中main.cpp为主程序,有main函数. 
   
  3.添加界面 
  我从simple Designed Based KDE Application建立一个项目.从文件菜单新建建立一个 
  文件,可以是ui(user interface用户界面),cpp,h. 
  我选择Dialog wid buttons(Bottom)ui,输入form1,在src生成一个form1.ui文件,双击 
  打开,这是一个表单界面. 
  修改表单标题,并加入一个pushButton,并双击建立clicked事件. 
  现在要给这个表单界面创建一个类.从而实现编成. 
  选中form1.ui,右键菜单执行create or select implementation,出现建立新类.我创建 
  新类 form1,然后生成form1.h和form1.cpp两个文件. 
  signal和slot, signal是发出事件,slot是接收处理事件. connect是把事件发往槽,让 
  slot处理.界面中connects是连接事件, slots是事件函数的状态. 
  Widget是窗口基类. 
  ui的界面控件不少,可以帮助迅速开发界面程序.ui生成一个xml文件文件. 
  数据库开发请参考文档Creating Dadabase Application,具有数据感知控件,编程也非 
  常简单. 
  ui编辑可以创建数据库连接,工程->database connections.这样会创建数据库连接,程 
  序中直接用.测试你能否连接数据库. 
  我没有看到可用的驱动程序,可能需要安装.因此设置连接出现driver is not loaded错 
  误.mysql用QMYSQL3驱动程序,默认端口3306 
   
  4.事件编程 
  现在加入按钮pushButton.选中加入的按钮,选择属性框的 signal handler事件,双击 
  clicked出现一个pushButton2_clicked事件!双击clicked事件或者它左边的工具图标 
  (有时侯无法创建代码,只能双击clicked),进入代码编程.它在.h,.cpp自动加入事件代 
  码!这个很类似delphi吧. 
  编译运行,按钮怎么没有出现?选中form1.ui,右键菜单执行create or select 
  implementation,选择使用已有的类型,确定.界面的更新需要重新更新到代码!代码更新 
  不需要这样做.清理工程,重新编译运行. 
  所以最好先把界面设计好,再写程序. 
   
  5.使用控件变量 
  UI中控件都有一个名字,比如textLabel2,我们可以在事件中直接给它赋值 
  textLabel2->setText( "pushbutton2" ); 
  可是我们在.h,.cpp文件中没有看到它的定义,怎么回事呢?在编译生成的.moc文件中我 
  看到事件的定义和处理,但是还是没有看到控件变量定义. 
  我终于在构造函数看到了. 
  button = new QPushButton( this, "button" ); 
  button->setGeometry( QRect( 11, 124, 198, 27 ) ); 
   
  6.断点调试 
  断点设置后就可以调试执行,单步执行,用gdb调试,可以反汇编,堆栈框架,跟其他C++开 
  发工具类似. 
   
  7.控件类和非可视化类 
  这些类使用方法需要参考资料学习,kdevelop可不是智能感知的,不能提示各种方法和类 
  ,所以还需要下点工夫熟悉qt类. 
  如果是非可视化开发,请遵照c++开发方法,使用 C++类库,没有任何特殊性.qt的类前面 
  有个Q字母. 
   
  8.文档帮助 
  右边分页文档可以查看各种语言帮助,不过是英文的.阅读里面的例子代码.你会更快的 
  进步. 
   
  9.总结 
  kedevelop for C++更象一个c++builder的可视化开发工具,比vc高级( VC用ID标记控件 
  ,非常混乱),界面开发更容易.他用gcc编译. 
  当然kedevelop不仅仅能够作为C++开发工具,而是各种计算机语言的开发工具 
  ,php,java,perl等等. 
  有了这个工具,在 sinox下面开发软件不再困难!可以跟windows媲美! 
  本站写的第一篇入门教程,只是抛砖引玉,希望大家以后写出更好的教程文章. 
  当然更希望作家能编写和出版有关kdevelop书籍,那将帮助sinox下软件开发迅速发展. 
   
  下面是研究笔记. 
  由于很多开源软件都可以在不同的unix/linux上编译,它针对不同操作系统的配置文件 
  如果没有sinox设置就会无法编译.这这时候我们怎么做呢?有两种方法,一种是增加 
  sinox操作系统的编译选项,另一种的修改freebsd的编译选项为sinox. 
  具体修改的文件是configure和admin/config.guess这两个文件!在这两个文件中把如下 
  内容改为sinox! 
  它是这些 
  *:FreeBSD:*:*|*:GNU/FreeBSD:*:*) 
  改为 
  *:Sinox:*:*|*:GNU/FreeBSD:*:*) 
   
  以及 
  freebsd*) 
  改为 
  sinox*) 
   
  记住只是一行前面*)前面的freebsd改动,里面的不要改动(你试试,看看会这么样?比如 
  freebsd-elf*)) 
  我想配置通过就算成功了! 
   
  ./configure 
  checking build system type... admin/config.guess: unable to guess system 
  type 
   
  This script, last modified 2003-07-02, has failed to recognize 
  the operating system you are using. It is advised that you 
  download the most up to date version of the config scripts from 
   
  ftp://ftp.gnu.org/pub/gnu/config/ 
   
  If the version you run (admin/config.guess) is already up to date, please 
  send the following data and any information you think might be 
  pertinent to in order to provide the needed 
  information to handle your system. 
   
  config.guess timestamp = 2003-07-02 
   
  uname -m = i386 
  uname -r = 1.2-RELEASE 
  uname -s = Sinox 
  uname -v = Sinox 1.2-RELEASE #0: Mon Jan 9 01:22:17 UTC 2006 
  root@sinox.shouji88.com:/usr/obj/usr/src/sys/GENERIC 
   
  /usr/bin/uname -p = i386 
  /bin/uname -X = 
   
  hostinfo = 
  /bin/universe = 
  /usr/bin/arch -k = 
  /bin/arch = 
  /usr/bin/oslevel = 
  /usr/convex/getsysinfo = 
   
  UNAME_MACHINE = i386 
  UNAME_RELEASE = 1.2-RELEASE 
  UNAME_SYSTEM = Sinox 
  UNAME_VERSION = Sinox 1.2-RELEASE #0: Mon Jan 9 01:22:17 UTC 2006 
  root@sinox.shouji88.com:/usr/obj/usr/src/sys/GENERIC 
  configure: error: cannot guess build type; you must specify one 
  sinox# ./configure 
  checking build system type... admin/config.guess: unable to guess system 
  type 
   
  This script, last modified 2003-07-02, has failed to recognize 
  the operating system you are using. It is advised that you 
  download the most up to date version of the config scripts from 
   
  ftp://ftp.gnu.org/pub/gnu/config/ 
   
  If the version you run (admin/config.guess) is already up to date, please 
  send the following data and any information you think might be 
  pertinent to in order to provide the needed 
  information to handle your system. 
   
  config.guess timestamp = 2003-07-02 
   
  uname -m = i386 
  uname -r = 1.2-RELEASE 
  uname -s = Sinox 
  uname -v = Sinox 1.2-RELEASE #0: Mon Jan 9 01:22:17 UTC 2006 
  root@sinox.shouji88.com:/usr/obj/usr/src/sys/GENERIC 
   
  /usr/bin/uname -p = i386 
  /bin/uname -X = 
   
  hostinfo = 
  /bin/universe = 
  /usr/bin/arch -k = 
  /bin/arch = 
  /usr/bin/oslevel = 
  /usr/convex/getsysinfo = 
   
  UNAME_MACHINE = i386 
  UNAME_RELEASE = 1.2-RELEASE 
  UNAME_SYSTEM = Sinox 
  UNAME_VERSION = Sinox 1.2-RELEASE #0: Mon Jan 9 01:22:17 UTC 2006 
  root@sinox.shouji88.com:/usr/obj/usr/src/sys/GENERIC 
  configure: error: cannot guess build type; you must specify one 
  sinox# ./configure 修改以后 
  checking build system type... i386-unknown-freebsd1.2 
  checking host system type... i386-unknown-freebsd1.2 
  checking target system type... i386-unknown-freebsd1.2 
  checking for a BSD-compatible install... /usr/bin/install -c 
  checking for -p flag to install... yes 
        

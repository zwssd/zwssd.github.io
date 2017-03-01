---
layout: post
title:  "Xmame玩neogeo游戏家用机模式！"
date:   2007-06-20 21:49:36
categories: 默认分类
tags:
---

* content
{:toc}

通过修改xmame的源码，将其玩NG游戏时的家用模式调出来！
给在Linux下想玩NG游戏的人另一种选择！
另：Mame源码里已经把家用机模式的代码给删除了，而xmame只是屏蔽而已！大概他们想的都是这个项目的目的是模拟街机，所以不需要这个功能！
Xmame玩neogeo游戏家用机模式！
如果你是Linux的fans,又恰好是kof的fans的话,那就要来看看本文了！偶不玩大游戏，休闲的时候也只是打打拳皇。尤其打kof98。
以前用win的时候有很多模拟器可以选择，其中winkawaks可能是最好的吧。它还有个家用机模式，里面有很多选项可以选择！
自从用了linux就少玩了！后来知道有xmame可以玩！默认的时候只能运行起kof97时也是让人兴奋了好几天！kof98却是没能运行起来。
后来才知道xmame对rom的命名比较严格，有一点不对就六亲不认了！呵呵！后来的办法是把kof98
roms里的文件对着xmame源码里的驱动一个一个改名字！这样才能玩起kof98来！linuxfans上的flysail兄弟在三年前发了一个
kof2002的驱动，我对着来改，也能运行kof2002了！所以有机会看一下xmame的源码。
当我的xmame基本上能玩上所有的kof版本时，我的一个新的愿望就出现了！就是xmame能不能调用neogeo的家用机模式啊？为了这我找遍
了几个大的开源论坛，google也试过了！最后才知道xmame在早前的版本中是有家用机模式的，但后来的版本把这个功能给屏蔽了！大概是0.37这个
版本开始吧！一开始我也想去找比较早的版本来装算了！但下载了好几个才发现已经不能通过编译了！毕竟我现在系统上的库已经换了很多代了！于是只有硬着头皮
去找源码看看，看看能不能找出点什么！果然，在
xmame*/src/drivers/neogeo.c这个文件中还是有家用机和街机这个选项的！不过代码已经给注释掉了！这给了偶一丝希望。看来
xmame的开发者并不是没写这个功能，而只是把它屏蔽而已！
但相关联的地方不止这一处，要找到其它相关的代码注释行可真是让人费煞苦心了！经过一晚上的反复试验，终于给偶找出了其屏蔽的代码！这下玩起kof来就更
爽了！呵呵！
好了！废话这么多，下面就说说怎样做！我一向认为授之于鱼不如授之于渔。并且也是很简单的两步而已！所以就没打成补丁什么的了！
基于xmame-0.90. （其它版本应该大同小异）
修改xmame-0.90/src/drivers/neogeo.c 文件 在1064行开始:
#if 0
PORT_DIPNAME( 0×03, 0×02,”Territory” )
PORT_DIPSETTING( 0×00,DEF_STR( Japan ) )
PORT_DIPSETTING( 0×01,DEF_STR( USA ) )
PORT_DIPSETTING( 0×02,DEF_STR( Europe ) )
/* PORT_DIPNAME( 0×04, 0×04,”Machine Mode” ) */
/* PORT_DIPSETTING( 0×00,”Home” ) */
/* PORT_DIPSETTING( 0×04,”Arcade” ) */
PORT_DIPNAME( 0×60, 0×60,”Game Slots” ) /* Stored at 0×47 of NVRAM */
PORT_DIPSETTING( 0×60,”2″ )
/* PORT_DIPSETTING( 0×40,”2″ ) */
PORT_DIPSETTING( 0×20,”4″ )
PORT_DIPSETTING( 0×00,”6″ )
#endif
改为:
#if 1
PORT_DIPNAME( 0×03, 0×02,”Territory” )
PORT_DIPSETTING( 0×00,DEF_STR( Japan ) )
PORT_DIPSETTING( 0×01,DEF_STR( USA ) )
PORT_DIPSETTING( 0×02,DEF_STR( Europe ) )
PORT_DIPNAME( 0×04, 0×04,”Machine Mode” )
PORT_DIPSETTING( 0×00,”Home” )
PORT_DIPSETTING( 0×04,”Arcade” )
PORT_DIPNAME( 0×60, 0×60,”Game Slots” ) /* Stored at 0×47 of NVRAM */
PORT_DIPSETTING( 0×60,”2″ )
PORT_DIPSETTING( 0×40,”2″ )
PORT_DIPSETTING( 0×20,”4″ )
PORT_DIPSETTING( 0×00,”6″ )
#endif
实质就是把#if 0 改成 #if 1, 把Machine Mode那一段的注释去掉！在此文件中还三段包含Machine Mode代码也是一样改法！
确定全部改完后就保存退出此文件！
修改 xmame-0.90/src/machine/neogeo.c
把第 34 ,45 ,197行的#if 0 都改成#if 1. 把51行的#ifndef CONSOLE 改成#ifdef CONSOLE .
至此可以保存编译！ ！问题是不能街机和家用机切换，得出来的可执行文件直接就进入家用机模式了。哎，想不到靠蒙也能蒙出来！
        

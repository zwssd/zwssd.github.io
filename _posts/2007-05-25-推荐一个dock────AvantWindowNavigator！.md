---
layout: post
title:  "推荐一个dock────AvantWindowNavigator！"
date:   2007-05-25 14:53:03
categories: 默认分类
tags:
---

* content
{:toc}

前几天在Ubuntu中文论坛上看到这个dock，用了几天觉得还不错就推荐一下！以前用过kiba-dock，我觉得那个也不错，不过我没怎么用过，呵呵，我觉得太夸张了点！    这个东西的主页在这里：http://awn.wetpaint.com/，上面有安装方法的，这里我就简单的说下在Edgy下的安装方法：sudo gedit /etc/apt/sources.list  添加源：  deb http://download.tuxfamily.org/syzygy42/ edgy avant-window-navigator
  deb-src http://download.tuxfamily.org/syzygy42/ edgy avant-window-navigator  然后要把钥匙，嘎嘎  wget http://download.tuxfamily.org/syzygy42/8434D43A.gpg -O- | sudo apt-key add -  升级软件包：  sudo apt-get update  sudo apt-get upgrade  然后就装喽：  sudo apt-get install avant-window-navigator  装好好在应用程序的附件里面就有Avant Window Navigator了。试试吧！  不过我这样装了后不行，后来用svn源里新的那个版本就可以用了，我是用新力得搜的，把那个带有svn字样的装上就可以了！  来张截图：   注：Feisty的话，就把源改成  ## Avant Window Navigator
  deb http://download.tuxfamily.org/syzygy42/ feisty avant-window-navigator
  deb-src http://download.tuxfamily.org/syzygy42/ feisty avant-window-navigator
        

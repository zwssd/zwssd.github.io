---
layout: post
title:  "Ubuntu7.04下安装compizfusion"
date:   2007-08-08 21:02:51
categories: 默认分类
tags:
---

* content
{:toc}

beryl让我们Linux体验到了超越VISTA的炫目特效。现在compiz 又与beryl合并，从此就诞生了compiz  fusion，即compiz 与 beryl的合并后的一个溶合体。Compiz  Fusion在综合了Compiz及Beryl特效的基础上，还提供更多的功能，同时与beryl相比，所需的系统资源更少，效果更为出色。
  下面，我们就来安装compiz fusion:
  1、得到密匙
  代码:  sudo wget http://download.tuxfamily.org/3v1deb/DD800CD9.gpg -O- | sudo apt-key add -  2、添加源：
  代码:  deb http://download.tuxfamily.org/3v1deb feisty eyecandy
  deb-src http://download.tuxfamily.org/3v1deb feisty eyecandy  3、更新：
  代码:  sudo apt-get update
  sudo apt-get dist-upgrade  4、安装Compiz及Compiz fusion
  代码:  sudo apt-get install compiz compiz-gnome
  sudo apt-get install compizconfig-settings-manager
  sudo apt-get install compiz-fusion-*  5、运行Compiz Fusion
  现在你就可以享受Fusion带来的惊喜啦，同时你也可以关闭Compiz及Beryl啦，在“系统-首选项-会话”新建一个新的会话如下：
  代码:  compiz –replace  6、Compiz 使用emerald 主题：
  代码:  sudo apt-get install emerald
  emerald –replace  这样，系统就可以在开机后自动加载Compiz Fusion啦。
  为防止风险，我是删除了beryl。
  在“系统-首选项”里有Compiz Fusion的设置程序Compizconfig Settings Manager,运行Compizconfig settings Manager，可以看到非常多的选项.特效演示是视频：http://www.youtube.com/watch?v=E4Fbk52Mk1w&v3
        

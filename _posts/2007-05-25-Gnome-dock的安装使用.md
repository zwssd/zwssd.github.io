---
layout: post
title:  "Gnome-dock的安装使用"
date:   2007-05-25 13:34:06
categories: 默认分类
tags:
---

* content
{:toc}

Gnome-dock 是一款模仿 Mac-OS Dock 的程序。Gnome-dock 只要具有 Gtk 环境就可以像 Engage 一样工作在 KDE 下。在我的机器上 Gnome-dock 需要 Beryl 才能运行，但是运行时还存在抖动现象，暂时不知道解决方案。    Gnome-dock 具有的特性如下：    * mimics the apple universal dock in animation
  * smooth graphics including composite transparency, SVG icons, glitz/cairo
  * animations are all but flawless (minor tweeks and ticks are still around)
  * auto-hide, only works on the bottom of the screen
  * bouncing icons
  * animated background (although it may be buggy)
  * beautifully rendered text    运行 Gnome-dock 需要的条件的如下：    # Cairo 1.2.0 (with glitz support = –enable-glitz)
  # A recent librsvg (> 2.14.4 will do it, i’m using a rawhide librsvg2-2.15.90-1.fc6 package on fedora)
  # glitz >= 0.5.3 (i’m using 0.5.6)
  # Xorg/Xcompmgr - I have a utility Gnome Settings Visualeffects for  configuring xcompmgr, which needs work if anyone is up for it.
  # Xgl/Compiz      安装步骤：（ubuntu 的 deb 包是我自己编译的，大家可以选择安装。）  1.安装 Cairo。[tar.gz|ubuntu-6.06.1-deb]  2.安装 glitz。[tar.gz|[ubuntu-6.06.1-deb]  3.安装 librsvg。  4.下载 gnome-dock。解压文件后，进入目录执行：  
  make clean
  make
    修改目录下 start-cairo-dock.sh 文件：  
  #!/bin/bash
  cd /home/latteye/soft/cairo-dock##此处为你gnome-dock文件夹地址##
  LD_LIBRARY_PATH=/usr/local/lib:$LD_LIBRARY_PATH ./cairo-dock --no-glitz
    5.执行 start-cairo-dock.sh 即可运行 gnome-dock。  修改 gnome-dock 上的图标需要修改源代码。打开目录下 cairo-dock.c 文件，找到如下代码，按格式添加后重新编译即可：    static Icon g_aIcons[] =
  {
      {"gnome-fs-home.svg", "Home", "nautilus /home/latteye"},
      {"clock.svg", "Clock", "/usr/bin/cairo-clock"},
      {"web-browser.svg", "Browser", "firefox"},
      {"email.svg", "E-Mail", "evolution -c mail"},
      {"im.svg", "IM", "gaim"},
      {"development.svg", "IDE", "eclipse"},
      {"search.svg", "Search", "beagle-search"},
      {"terminal.svg", "Terminal", "gnome-terminal"},
      {"lockscreen.svg", "Lock", "gnome-screensaver-command --lock"},
      {"stop.svg", "Kill", "xkill"},
      {"logout.svg", "Logout", "gnome-session-save --kill"},
      {"user-trash-full.svg", "Trash", "nautilus trash:///"}
  };
        

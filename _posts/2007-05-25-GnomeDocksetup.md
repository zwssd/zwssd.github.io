---
layout: post
title:  "GnomeDocksetup"
date:   2007-05-25 20:26:53
categories: 默认分类
tags:
---

* content
{:toc}

From what I understand, the project is not actively developed, but what  we have for the moment is great. It is prettier than kiba-dock, more  functional and also more "OS X-like". It takes more time to customize,  but I think it's worth it. Here's how I installed it on Edgy with AIGLX:
  
  1. Install these packages
     Code:   sudo apt-get install librsvg2-bin librsvg2-common librsvg2-dev libglitz-glx1 libglitz-glx1-dev  
  2. Make sure you have build-essential to be able to compile cairo.
  
  3. Download Cairo 1.2.6
  
  http://cairographics.org/releases/cairo-1.2.6.tar.gz
  
  4. Install Cairo 1.2.6
  
     Code:   ./configure --enable-warnings --enable-glitz --disable-quartz --disable-atsui --disable-xcb --disable-win32 --disable-gtk-doc     Code:   make     Code:   sudo make install  5. Download cairo-dock
  
  http://www.gnome-dock.org/prerelease...-0.0.1b.tar.gz (doesn't seem to work anymore)
  Try this instead: cairo-dock.tar.gz
  
  6. Extract the tarball in /opt
  
  7. Install cairo-dock
  
     Code:   make clean     Code:   make  Also, change your virtual vertical size in Beryl from 1 to 2,  otherwise it will probably bug. That's a weird fix found by someone in  this thread, but it works.
  
  To open the dock, use
  
     Code:   ./cairo-dock --no-glitz  To add launchers, play with cairo-dock.c and redo
     Code:   make  once you've saved the file.
  
  Also, you'll probably have to change Vertical Virtual Size in Beryl from 1 to 2 to avoid a bug.
  
  At the moment, it only accepts icons in .svg, but it's fairly easy to convert .png to .svg using InkScape. Good luck!
  
  
        

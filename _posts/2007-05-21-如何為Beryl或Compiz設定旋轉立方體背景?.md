---
layout: post
title:  "如何為Beryl或Compiz設定旋轉立方體背景?"
date:   2007-05-21 12:26:10
categories: 默认分类
tags:
---

* content
{:toc}

Beryl或Compiz其中一超炫功能就是當切換工作區(Viewport)時可以製造如Mac OS X般的轉動正立方體動畫效果。你可以自訂轉動時的背景 - Skydome，亦可令背景跟隨正立方體轉動。
 預備Skydome圖像   Skydome圖像只可以為PNG檔，長闊需要是1024像素的倍數，但不可以大於GL_MAX_TEXTURE_SIZE。（你可以打 "glxinfo -l | grep GL_MAX_TEXTURE_SIZE"）  如果你想Animated  Skydome（即背景隨正立方體轉動效果），Skydome圖像長闊比例最好4:1（對應四個面)。不過由於Beryl會把背景縮窄，最好把一幅圖剪裁  成長闊4:1的比例 (如 4096 x 1024)，再縮放成2048 x 1024像素大小。    [编辑] 設定 Compiz   [编辑] skydome   啟動skydom背景功能：    gconftool-2 -t bool -s /apps/compiz/plugins/cube/screen0/options/skydome true
  [编辑] skydome_animated   設定為true後，轉動正立方體時skydome背景會跟隨轉動：    gconftool-2 -t bool -s /apps/compiz/plugins/cube/screen0/options/skydome_animated true
  [编辑] skydome_image   設定為Skydome背景圖像檔案的絕對路徑：    gconftool-2 -t bool -s /apps/compiz/plugins/cube/screen0/options/skydome_image "/home/jrandom/Document/My Skydome Image.png"
  [编辑] Beryl   執行Beryl Setting Manager，在『桌面立方體』(Cube) 中，選取『背景』(Skydome)      如果想轉動正立方體時skydome背景會跟隨轉動，請一同選取『動態背景』(Animated Skydome)  記得在『文件名』(Filename) 分頁中，在『背景圖像』(Skydome Image) 指定你想要的Skydome背景圖像檔案    
        

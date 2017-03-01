---
layout: post
title:  "Ubuntu的beryl设置"
date:   2007-05-19 11:14:23
categories: 默认分类
tags:
---

* content
{:toc}

Ubuntu 的 beryl设置
感觉Compiz可设置的特效太少了(可能是设置软件没安装的问题吧)，还是习惯用beryl。
feisty下安装beryl很方便：
$ sudo apt-get -y install beryl beryl-manager emerald-themes
安装完在 系统--首选项--会话 中加入beryl-manager 即可。
下次开机会默认启用Beryl。要选择Beryl或Compiz，可右键单击任务栏Beryl图标，在"选择窗口管理器"中选择。
升级到beryl0.3beta版 （以前edgy时用的就是它，升级到feisty当然不会落下它）。
添加SVN源 ( 需要用edgy源，feisty的不行 )
$ sudo gedit /etc/apt/sources.list
往里面添加：
deb http://download.tuxfamily.org/3v1deb edgy beryl-svn
保存后，执行：
$ wget http://download.tuxfamily.org/3v1deb/DD800CD9.gpg -O- | sudo apt-key add -
$ sudo apt-get update
过几分钟 更新管理器 就会提示有更新。更新完重新登陆下就行了。
另：sudo apt-get install beryl-plugins-unsupported    进行安装其他的一些效果，如下雪，很不错的！
from：linuxdesktop.cn
beryl 桌面使用方法
加入源：
sudo gedit /etc/apt/sources.list
deb http://ubuntu.beryl-project.org/ feisty main
下载KEY：
wget http://ubuntu.beryl-project.org/root@lupine.me.uk.gpg -O- | sudo apt-key add -
更新：
sudo apt-get update
安装beryl：
sudo apt-get install beryl beryl-manager emerald-themes
重启后执行命令：
sudo beryl-manager(或者开始菜单中启动)
上面的步骤前提是你的显卡驱动已经正确安装.
beryl 桌面使用方法
分类: 技术前沿  |  标签:  电脑
全局选项：
Alt + 鼠标滚轮 上/下使窗口 透明/不透明
程序切换：
Alt + Tab：在当前工作台中切换窗口
Ctrl + Alt + Tab：在所有工作台中切换窗口
窗口排列（编排并显示所有窗口）：上/下
左下角（关键区域）：所有工作台（点击一个窗口缩放它到前台）
右上角（关键区域）：当前工作台
显示桌面（看当前立体面的桌面）：
右下角（关键区域）：开/关
立方体旋转：
Ctrl + Alt + 左/右方向键：立体地切换桌面
Ctrl + Shift + Alt + 左/右方向键：把活动窗口移到左/右工作台
Ctrl + Alt + 鼠标左键并拖曳：手动旋转立方体
缩放：
Win + 鼠标右键：缩放一次
Win + 鼠标滚轮 上/下：手动缩放大/小
移动窗口：
Alt + 鼠标左键并拖曳：移动窗口
Ctrl + Shift + 鼠标左键：迅速移动窗口（会粘住边框）
调整窗口大小：
Alt + 鼠标中键
水波效果：
Ctrl + Win + 移动鼠标：关标在水上移动（默认无效）
Shift + F9：雨点降落在你的屏幕上
模糊效果：
在透明窗口下添加一些模糊（会使计算机变慢）
动画效果：
当创建或者关闭窗口时使用动画效果（对菜单也有效，不过你要选择“未知”，只选“菜单”没用）
反色效果：
Win + m：屏幕反色
Win + n：当前窗口反色
反射效果：
给装饰添加一些纹理（当透明时大多数可见）
屏幕截图：
Win + 鼠标左键并拖曳：将所选区域截图（图片保存在桌面）
焦点轨迹效果：
更旧的窗口更加透明
摆动效果：
使窗口、菜单等像棉花糖
亮度和饱和度：
Ctrl + 鼠标滚轮 上/下：增加/减少 饱和度（对桌面也有效）
Shfit + 鼠标滚轮 上/下：增加/减少 亮度（对桌面也有效）
窗口对齐：
Win + 小键盘1...9：在屏幕中快速对齐一个窗口（1＝左下，2＝中下，3＝右下......） 
        

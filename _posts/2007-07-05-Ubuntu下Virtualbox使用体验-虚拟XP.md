---
layout: post
title:  "Ubuntu下Virtualbox使用体验-虚拟XP"
date:   2007-07-05 19:52:45
categories: 默认分类
tags:
---

* content
{:toc}

很早之前电脑里就只剩下了一个系统-Ubuntu。虽然不喜欢WIN，但是有很多工作我还得必须去WIN下完成。比如网银，比如QQ……
  正好Virtualbox已经发布1.4版了。而且听说还相当稳定。于是无聊间开始装Virtualbox了！原文来自[何必呢]  一、安装 virtualBox。
  1、下载 virtualBox。
  2、安装，切换到已经下载的 virtualBox 包目录开始安装：
  sudo dpkg -i 下载的文件名.deb
  3、添加使用用户到 vboxusers 用户组，vboxusers 是安装 vbox 时自动建立的组：
  sudo adduser ksky vboxusers
  (ksky是我的用户名，需要改为你的用户名)  至此，安装完成。在“应用程序-系统工具”里找到 innotek VirtualBox 或者终端运行命令VirtualBox打开VirtualBox。  一、安装 XP。
  1、下载 深度精简XP 5.7。（我用的是5.5的， 看着新版出了就用新版吧，这个是最适合安装在虚拟机上的系统，强烈推荐！！）
  2、打开 virtualBox 点击新建 ，然后一步步设置。我内存768M，所有我分给客户机256M内存。
  3、建好后点击设置，光驱设置里面选择刚下载的XP镜像。然后双击新建的客户机开始安装。安装过程同普通安装XP相同。  安装完成XP后，启动客户机进入XP系统。点击设备-安装虚拟专用电脑辅助工具包。至此驱动安装完成。
  关闭客户机。然后Vbox选择设置-声音-选择ALSA那个。
  Virtualbox1.4下有自带的共享文件功能。在设置里选择共享，添加固定共享文件输入名字。然后启动客户机，打开我的电脑，选择 “工具” — “映射网络驱动器”
  在“文件夹”处填写：\\vboxsvr\ksky (ksky 是刚才我建的那个共享文件夹名）
  点击完成之后，我们即可在我的电脑里像使用本地磁盘一样使用该共享文件夹。
  
  经过何必呢的若干天测试，除显卡有点问题外，其他一切于普通安装XP没有什么不同。迅雷，QQ，PPLive，梦幻西游等等使用全部正常。
  呵呵，现在应该没有不抛弃XP的理由吧？怎么样？大家一起来UBUNTU吧！
  下面上图！点击图片浏览大图！
  设置完成
  
  开始登陆客户机
  
  设置共享！
  
  最终效果！
  
        

---
layout: post
title:  "利用wine把photoshop移值到ubuntu7.04"
date:   2007-06-18 17:15:24
categories: 默认分类
tags:
---

* content
{:toc}

1、安装wine   sudo apt-get install wine2、建立wine file 文件目录结构  sudo wine 3、复制windows的整个adobe目录到ubuntu的以下目录   /home/YOURNAME/.wine/drive_c/Program Files/ (红色部分为你的用户名,.wine目录是隐藏的，在nautilus中选“查看-显示隐藏文件”就可以显示了)   4、在windows中导出注册表键值  在windows中的“运行”输入 regedit  找到 HKEY_LOCAL_MACHINE/Software/Adobe键值，按右键导出,命名为adobe.reg然后用记事本打开,再另存为ascii编码格式的adodb.reg(这点很重要，否则在ubuntu下用wine导入不成功，运行photoshop会显示注册表信息出错)     5、在ubuntu中导入注册表键值  sudo wine regedit adobe.reg     6、建立运行快捷方式！用"菜单管理器"增加运行快捷方式！运行命令如下:env WINEPREFIX="/home/YourName/.wine"   wine "C:\Program Files\Adobe\Photoshop 7.0\Photoshop.exe"   7、运行测试photoshop原文链接:http://blog.publicidadpixelada.com/how-to-adobe-photoshop-cs2-on-ubuntu-10-steps注意：recode 是用来转换编码的一个小程序。如果你在window中用记事本把.reg另存为ascii编码的文件就可以不用这个程序!
        

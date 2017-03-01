---
layout: post
title:  "使用eclipse调试gae(googleappengine)2009-02-4"
date:   2011-03-09 11:07:35
categories: python
tags:
---

* content
{:toc}

google发布了gae（google  appengine），允许开发者直接在上面做各种网站和互联网运用。gae的主页以helloworld，example以及api的方式介绍了gae  的开发模式，对于开发来说，gae提供的文档明显不够。好在gae是搭建于python之上的，很多python的东东都是适用于gae的。
 gae的介绍中，对于程序的调试涉及甚少，以至于很多入门者不知如何调试程序，特别是那些一直依赖于集成开发环境（IDE）的同志更是如此。从这点上看，我觉得gae在这方面做的非常之不够。
 本文将介绍如何使用elipse搭建gae开发环境，如何进行单步跟踪调试。
 1.准备工作1）假定你已经装了python，elipse和google app engine，这个步骤不在本文描述之内
 2）在eclipse上安装pydev
 从 Eclipse 中选择 Help > Software Updates > Update Manager，启动 Install/Update 视角。在左下角的 Feature Updates 视图中，将 PyDev 插件更新站点作为新的 SiteBookmark 添加到“Sites to Visit”文件夹下。Eclipse 的 PyDev 更新站点 URL 为http://pydev.sf.net/updates/。现在，Feature Updates 编辑器中应该显示出“PyDev”这一特性。在Feature Updates 编辑器中，展开 PyDev > Other，选择其中显示的 PyDev 特性（至少应该是 0.4.1）。然后选择 “Install Now”安装该特性。Eclipse 将下载 PyDev 插件，并将其安装到 Eclipse 中。 如下图所示：
 在pydev安装完毕后，根据要求，重新启动eclipse
 2.配置pydev1）eclipse重新启动后，点菜单Windows -> Preferences 
 
 2）点击dialog中的PyDev -> Interpreter - Python 部分
 
 3）点击“Python interpreters" ”正右边的New按钮
 
 
 
 4）找到你的python的安装目录，选中python.exe,比如我的是C:\Python25\python.exe
 
 5)此时pydev后搜索相关的dll，显示如下页面
 
 
 
 6)按ok接收系统pythonpath条目
 
 7)按ok确认修改
 3.开始你的第一个project1)在eclipse 的package explorer区域的右键下拉菜单中点击New -> Other
 
 2)在弹出的对话框中，选pydev类中的Pydev project 
 
 3)点next，在下一个窗口中，输入project name(helloworld)，注意project type选Python 2.5 (非常重要)
 
 
 
 4)点finish,eclipse会切换到eclipse view
 
 5)右键单击helloworld project，在下拉菜单中选 Properties
 
 6)在属性对话框中选择PyDev - PYTHONPATH ，添加gae的lib
 
 7)点击external source folder一栏右边的Add source folder 添加如下folder
 
 
 
 8)选ok确认修改
 
 9)在src目录下书写代码（或者将已经写好的代码放到src目录下）
 
 
 4.配置run configuration和debug configuration1）Run -> Open Run Dialog
 
 2）选择Python Run ，加入一个新的configuration
 
 & 
 3）命名configuration
 在“Project”栏，加入你的google app engine python project
 "Main Module"栏, 键入你的dev_appserver.py的路径（比如我是C:\Program Files\Google\google_appengine\dev_appserver.py）. 
 切换到argument属性页，program arguments一栏键入${workspace_loc:helloworld/src/}作为第一个参数，注意参数中的helloworld要根据自己的情况改变。在这个参数后面，你可以加入 Dev Webserver documentation page. 列出的可用的参数。
 
 5）点apply保存修改
 
 6）点击debug的configuration设置，可以看到已经自动设好了
 
 7)可以运行及调试了，点run，可以看到控制台有如下输出
 
 
 点debug，设置断点，在ie或者opera中登录http://localhost:8080，就可以进行单步跟踪
 
 
 如有任何疑问，可以回帖
        

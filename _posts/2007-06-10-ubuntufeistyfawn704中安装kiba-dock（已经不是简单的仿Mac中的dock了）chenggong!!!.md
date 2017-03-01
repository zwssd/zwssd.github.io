---
layout: post
title:  "ubuntufeistyfawn7.04中安装kiba-dock（已经不是简单的仿Mac中的dock了）chenggong!!!"
date:   2007-06-10 12:42:49
categories: 默认分类
tags:
---

* content
{:toc}

我是小菜鸟，希望为刚用ubuntu的新人装饰自己的桌面省去点麻烦，就把自己装kiba-dock的过程总结了一下，请高手不要笑话我。  
    
  东拼西凑的，改了一些东西。  
    
    
  一：  确定基本依赖关系  
      Dependencies  
    
    
      * Cairo 1.2.0 (Optional: SVG support = --enable-svg and Glitz support = --enable-glitz)  
      * Optional: A recent librsvg (> 2.14.4)  
      * Optional: glitz >= 0.5.3  
      * Xorg with Composite Extension enabled  
      * Composite Manager (xcompmgr, kcompmgr, beryl, compiz).  
      * The memory plugin requires libgtop  
      * The trash plugin requires libgnomevfs  
    
        下面的代码我根据自己的总结了一下，发现真的好多人跟我的问题类似，把要下载的包给全，省得走弯路。  
      在终端中分别执行如下代码：  
    
            代码:             
  $ sudo aptitude install fakeroot automake1.9 build-essential  libpango1.0-dev libgtk2.0-dev libgconf2-dev  libglitz-glx-dev   librsvg2-dev libglade2-dev libxcomposite-dev subversion libtool  libgtop2-dev libdbus-glib-1-dev gaim-dev python-gtk2-dev  
  $ sudo aptitude install automake1.4  
  $ sudo aptitude install libgnome-desktop-dev         #(这条很重要，我就是卡在这里体验百思不得其解的痛苦)  
    
    
    二   准备工作结束后，分别执行下列下载安装代码：  
         
    
            代码:           $ mkdir kiba-dock  
  $ cd kiba-dock  
  $ svn co http://svn.kiba-dock.org/akamaru/ akamaru  
  $ svn co http://svn.kiba-dock.org/kibadock/trunk kibadock  
  $ svn co http://svn.kiba-dock.org/kibaplugins/trunk kibaplugins  
  $ svn co http://svn.kiba-dock.org/kibadbusplugins kibadbusplugins  
  $ svn co http://svn.kiba-dock.org/gaimplugin/trunk gaimplugin         
    
#如果用checkinstall的话，记得标记一下所生包的版本号。如果觉得麻烦直接sudo make  install。我本人还是建议将下面的的sudo checkinstall换成sudo make  install，当然对checkinstall熟悉的人另当别论，我在用checkinstall是总是多少会有一点小意外。  
    
    
            代码:           $ cd akamaru/  
  $./autogen.sh  
  $make  
  $sudo checkinstall  
    
  $cd ../kibadock/  
  $./autogen.sh  
  $make  
  $sudo checkinstall  
    
  $cd ../kibaplugins/  
  $./autogen.sh  
  $make  
  $sudo checkinstall  
    
  $cd ../kibadbusplugins/  
  $./autogen.sh  
  $make  
  $sudo checkinstall  
    
  $cd ../gaimplugin/  
  $./autogen.sh  
  $make  
  $sudo checkinstall               #(注意新版本gaim可能换了名字，具体去看kiba官方的论坛)  
    
    
      上面在进入每个路径去执行装的时候，可以不用$ sudo checkinstall来生成软件包，可以用$ sudo make install来安装，我觉的这样更妥当，呵呵，还简单.  
      
    
  如果嫌麻烦，下面的附件中的easyKiba是一个安装升级的脚本，在确定其他依赖包安装齐全的情况下，可是用easyKiba来安装，升级，卸载kiba-dock，方法是在当前用户路径下建立一个文件夹，将easyKiba拷贝到该文件夹，然后在终端中执行          代码:           $ ./easyKiba        ，会有相应的提示，很方便的，记住安装完成后不要删除该文件夹，以后要升级的时候进入文件夹内执行          代码:           $ ./easyKiba -u       来升级。  
    三： 安装结束后记得创建四个链接。  
            代码:             
  $ sudo ln -s /usr/local/bin/kiba-dock /usr/bin/kiba-dock  
    
  $ sudo ln -s /usr/local/bin/gset-kiba /usr/bin/gset-kiba  
    
  $ sudo ln -s /usr/local/lib/libakamaru.so.0 /usr/lib/libakamaru.so.0  
    
  $ sudo ln -s /usr/local/lib/kiba-dock/liblauncher.so /usr/lib/kiba-dock/liblauncher.so         
    
    
      这时可以在终端输入：  
            代码:             
  $ kiba-dock         
    
    
  查看一下kiba-dock，怎么样？跟原来的不太一样吧，多了许多插件。当然，你可以点击: 应用程序->附件->Kiba-dock 来打开kiba-dock。  
    
    
  ＃＃＃＃＃＃＃＃＃＃＃＃＃＃＃＃＃＃＃＃＃＃＃＃＃＃＃＃＃＃＃＃＃＃＃＃＃＃＃＃＃＃＃＃＃＃＃＃＃＃＃＃＃＃＃＃＃＃＃＃＃＃＃＃＃＃＃＃＃＃＃＃＃＃＃  
    
    
    
  顺便附上kiba-dock官方wiki中的安装方法，虽然我的没成功，但并不代表别人不能用这个方法成功安装kiba。下面是  
  32位系统的安装方法，当然上面的方法也适用，我感觉上面的更保险一点。  
    
  Quote:  
  Ubuntu  
    
Trevi?o manages a repo with kiba-dock, These can be accessed by  adding the following to your sources.list file: Note: Currently, there  are only 32 bit (x86) deb packages available, 64 bit users can use SVN.  
    
     1. Open a Terminal Window.  
     2. Type the following:  
            代码:             
  # sudo gedit /etc/apt/sources.list  
           
     1. Add the following lines to the end of the file:  
    
  deb http://download.tuxfamily.org/3v1deb feisty eyecandy  
  deb-src http://download.tuxfamily.org/3v1deb feisty eyecandy  
    
     1. Save and Exit Gedit.  
     2. Run from the terminal window:  
    
            代码:           # wget http://download.tuxfamily.org/3v1deb/DD800CD9.gpg -O- | sudo apt-key add -  
  # sudo apt-get update  
  # sudo apt-get install kiba-dock  
  # sudo apt-get install kiba-dock-dev  
  # sudo apt-get install kiba-plugins  
           
PLEASE NOTE: this is a daily svn repo, which means you're dealing  with snapshots of the latest and greatest, but not always the most  stable!  
    
  说明：在kiba中右键单击图标打开物理特效的时候，实际上kiba会在屏幕上进行加层，这时你如果想用beryl中用按住鼠标中键来翻转屏幕是做不到的。当然加快捷件的翻转桌面除外，那个还是好用的。  
    
  在最后我祝大家玩的开心，呵呵。
        

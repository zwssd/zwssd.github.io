---
layout: post
title:  "从硬盘启动DesktopDapper[不刻盘不安装却根安装好的一样"
date:   2007-05-22 10:51:49
categories: 默认分类
tags:
---

* content
{:toc}

Ubuntu 桌面版光盘是一个 Live CD,   
  它把安装好的 Ubuntu Linux（大约2G的内容）压缩在一张光盘中。  
  Live CD 通常用来给人体验的，但 Ubuntu 却加入了一个安装程序，使安装更简单方便。  
    
  但由于各种原因，有时使用 Live CD 还不方便：  
  1.用 Live CD 首先要有光驱，但如果你没有光驱，或光驱老化了；  
  2.如果是从网上下载的ISO文件，那就要有刻录机；  
  3.你有了上述条件，但你只是想体验一两次，又不想刻光盘；  
  4.或者你刻了数张光盘，但能正常用的几乎没有；  
  5.光盘能用，但速度太慢等；  
    
  在这样的情况下，你就得用从硬盘启动 Live CD，在 Linux 和 Windows 下都能从硬盘起动，  
  只要用修改后的 initrd.gz（一个引导配置文件） 文件就行了（加入了查找ISO文件与从ISO文件中读取）。  
    
  Ubuntu 桌面版光盘里已经带了一个压缩了的 Ubuntu Linux,压缩后再用就会慢一点，  
  但现在的计算机使人根本感觉不到。只要使 Live CD 能保存就根安装了的 Ubuntu Linux 没多大的区别。   
  所以我就用 ubuntu.fs 文件作 Live CD 根分区，swap.fs 作 Live CD 交换分区，  
  这样就使 Live CD，成为一个完全可用的系统，且不用安装。  
    
  其特点是:  
  1.ISO上的文件系统压缩过，比安装好的 Ubuntu Linux 节约 1.5GB 的空间；  
  2.系统修改的文件放在 ubuntu.fs 文件中，所以不用担心损坏系统；  
  3.用 ubuntu.fs 与 swap.fs 文件就不用分区，新手可更加自信的操作；  
  4.用一个新的 ubuntu.fs 文件就可以恢复到 Live CD 状态下；  
  5.可发布 ubuntu.fs 来共享自己的环境与设置；  
  6.光盘经过优化，启动快，且操作简单,三步就好  
    
  在配置较好的电脑上就可以用 Dubuntu Linux, 差一点的或笔记本就可以 Hiweed Linux  
  本人用 1G 的MP4 ，在13分钟就可以搞定一台机子，因为复制ISO文件到硬盘就要13分钟，  
  在其间的设置，不到1分钟，简单方便，有什么理由不用呢？  
    
  再加上 Linux 操作系统本身就很好用，新手又有什么理由不试试呢？  
    
  具体操作如下(三步就好)：  
    
  1.通用设置  
    
  A.下载 Ubuntu 桌面版 ISO 映象,建议用 Dubuntu 与 Hiweed 的 ISO;  
  B.下载修改后的引导文件,不同的光盘要用不同的引导文件; ( http://ftp.ubuntu.org.cn/gnix_oag )  
  C.把下载的桌面版光盘映象(*.iso)文件放到任意盘的根目录(不要放入NTFS中);  
  D.从本压缩包中 ./文件系统/ubuntu.fs 中选一个文件，解压到任意盘的根目录，并重命名为 ubuntu.fs;  
       或者在ext2|ext3|reiserfs|jfs|xfs|minix 分区的根目录新建一个 ubuntu.fs 文件夹;  
  E.从本压缩包中 ./文件系统/swap.fs 中选一个文件，解压到任意盘的根目录，并重命名为 swap.fs;  
    
    
  2.引导设置  
    
  A.在 WINXP 中,把本压缩包中 boot文件夹、grldr文件复制到 C:\ 下,  
       在 c:\boot.ini文件后面添加 c:\grldr="GNU Ubuntu Linux",  
       把 timeout=0 改成 timeout=3 (可参照 boot.ini.txt 文件修改);  
    
  B.在有 grub 的 Linux 中,把本压缩包中 boot文件夹中的 vmlinuz initrd.gz 复制到硬盘  
       在 /boot/grub/menu.lst 后面加以下内容:  (*号根据自己的来改)  
    title GNU Ubuntu Linux  
    kernel (hd*,*)/*/vmlinuz boot=casper ramdisk_size=1048576 root=/dev/ram rw quiet splash  debian-installer/locale=zh_CN  
    initrd (hd*,*)/*/initrd.gz  
    
  3.重新起动就行了,进入 linux 后,简单的设置一下  
     检查交换分区文件的使用 swapon -s  
     设定主机名,设定用户名,设定网络,/etc/fstab 等...  
    
  制作人: gnix_oag  
  Email: gnix.oag@gmail.com  
    
  新的中文制定文法：    
            
        iso 文件用官方 Live CD，  
        用本方法使用此 ISO  
        进行中文软件的安装与系统设置   
        最后发布 ubuntu.fs 文件即可  
        
       ubuntu.fs文件压缩后可比重新做个 ISO 要少得多  
    
    
        

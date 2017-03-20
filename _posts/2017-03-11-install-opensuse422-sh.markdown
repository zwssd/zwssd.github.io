---
layout: post
title:  "安装 openSUSE Leap 42.2 之后要做的事"
date:   2017-03-11 09:27:00
categories: linux
tags: opensuse
---

* content
{:toc}

 阿里云源 http://mirrors.aliyun.com/

    首先禁用系统的源 >> sudo zypper mr -da

<!--excerpt-->


    添加源:
        >>sudo zypper addrepo -f http://mirrors.aliyun.com/opensuse/update/leap/42.2/non-oss/ openSUSE-42.2-Update-Non-Oss
        >>sudo zypper addrepo -f http://mirrors.aliyun.com/opensuse/distribution/leap/42.2/repo/oss/ openSUSE-42.2-Oss
        >>sudo zypper addrepo -f http://mirrors.aliyun.com/opensuse/distribution/leap/42.2/repo/non-oss/ openSUSE-42.2-Non-Oss
        >>sudo zypper addrepo -f http://mirrors.aliyun.com/packman/openSUSE_Leap_42.2/ openSUSE-42.2-packman

    刷新源 >> sudo zypper ref

    最后愉快的去更新系统吧 >> sudo zypper up


    sudo zypper in filezilla gvim fcitx fcitx-table-cn-wubi-pinyin chromium chromium-pepper-flash vlc qbittorrent git gitg
    
debian 8.7

yuan

    deb http://mirrors.aliyun.com/debian jessie main contrib non-free
    deb-src http://mirrors.aliyun.com/debian jessie main contrib non-free

    deb http://mirrors.aliyun.com/debian jessie-updates main contrib non-free
    deb-src http://mirrors.aliyun.com/debian jessie-updates main contrib non-free

    deb http://mirrors.aliyun.com/debian-security jessie/updates main contrib non-free
    deb-src http://mirrors.aliyun.com/debian-security jessie/updates main contrib non-free
    
update:

    sudo apt-get update
    sudo apt-get upgrade


sudo apt-get install filezilla vim-gtk fcitx fcitx-table-wbpy chromium vlc qbittorrent git gitg

安装下面3个包

# apt-get install firmware-linux-nonfree libgl1-mesa-dri xserver-xorg-video-ati

C.下载并安装Debian WiKi说明的驱动包

# dpkg -i /home/user/Downloads/xserver-xorg-video-radeon_7.5.0-1_amd64.deb

D.相关依赖（我并不确定是否要安装这些，Debian WiKi说上一步的DEB包和下面的包有依赖关系，但是我没安装也可以）

如果你的驱动不正常，可以试着安装一下下面的依赖

# apt-get install libc6 libdrm-radeon1 libpciaccess0 firmware-linux xserver-xorg-core xorg-video-abi-18 libudev1

E.重启系统

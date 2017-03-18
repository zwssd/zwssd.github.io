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

    首先禁用系统的源 >> sudo zypper mr -da\n\n\n\n


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

---
layout: post
title:  "我终于又找到你了，“情感交流天地” "
date:   2005-07-04 17:13:23
categories: 默认分类
tags: 情感
---

* content
{:toc}
 
 阿里云源 http://mirrors.aliyun.com/

    首先禁用系统的源 >> sudo zypper mr -da

    添加源:
        >>sudo zypper addrepo -f http://mirrors.aliyun.com/opensuse/update/leap/42.2/non-oss/ openSUSE-42.2-Update-Non-Oss
        >>sudo zypper addrepo -f http://mirrors.aliyun.com/opensuse/distribution/leap/42.2/repo/oss/ openSUSE-42.2-Oss
        >>sudo zypper addrepo -f http://mirrors.aliyun.com/opensuse/distribution/leap/42.2/repo/non-oss/ openSUSE-42.2-Non-Oss
        >>sudo zypper addrepo -f http://mirrors.aliyun.com/packman/openSUSE_Leap_42.2/ openSUSE-42.2-packman

    刷新源 >> sudo zypper ref

    最后愉快的去更新系统吧 >> sudo zypper up
    
    
    sudo zypper in filezilla gvim fcitx fcitx-table-cn-wubi-pinyin chromium chromium-pepper-flash vlc qbittorrent git gitg

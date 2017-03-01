---
layout: post
title:  "Ubuntu系统下更新sources.list"
date:   2007-05-20 09:56:40
categories: 默认分类
tags:
---

* content
{:toc}


  在终端执行以下命令（第一条是备份现有服务器列表，第二条是使用Gedit编辑，您也可以使用自己喜爱的编辑器编辑。）
sudo cp /etc/apt/sources.list /etc/apt/sources.list_backup
sudo gedit /etc/apt/sources.list
从以下各服务器列表内容中选择一段替换文件中的所有内容（请根据自己网络环境设置更新服务器，以达到最快的速度。）
    * Archive.ubuntu.com 更新服务器(欧洲)：
#
deb http://archive.ubuntu.com/ubuntu/ dapper main restricted universe multiverse
deb http://archive.ubuntu.com/ubuntu/ dapper-security main restricted universe multiverse
deb http://archive.ubuntu.com/ubuntu/ dapper-updates main restricted universe multiverse
deb http://archive.ubuntu.com/ubuntu/ dapper-backports main restricted universe multiverse
deb http://archive.ubuntu.org.cn/ubuntu-cn/ dapper main restricted universe multiverse
    * Ubuntu.cn99.com 更新服务器(江苏省常州市电信，推荐电信用户使用。）： 
deb http://ubuntu.cn99.com/ubuntu/ dapper main restricted universe multiverse
deb http://ubuntu.cn99.com/ubuntu/ dapper-updates main restricted universe multiverse
deb http://ubuntu.cn99.com/ubuntu/ dapper-security main restricted universe multiverse
deb http://ubuntu.cn99.com/ubuntu/ dapper-backports main restricted universe multiverse
deb http://ubuntu.cn99.com/ubuntu-cn/ dapper main restricted universe multiverse
    * Mirror.lupaworld.com 更新服务器(浙江省杭州市电信，亚洲地区官方更新服务器，推荐全国用户使用。）： 
deb http://cn.archive.ubuntu.com/ubuntu dapper main restricted universe multiverse
deb http://cn.archive.ubuntu.com/ubuntu dapper-security main restricted universe multiverse
deb http://cn.archive.ubuntu.com/ubuntu dapper-updates main restricted universe multiverse
deb http://cn.archive.ubuntu.com/ubuntu dapper-backports main restricted universe multiverse
deb http://mirror.lupaworld.com/ubuntu/ubuntu-cn dapper main restricted universe multiverse
deb http://security.ubuntu.com/ubuntu dapper-security main restricted universe multiverse
deb-src http://cn.archive.ubuntu.com/ubuntu dapper main restricted universe multiverse
deb-src http://cn.archive.ubuntu.com/ubuntu dapper-security main restricted universe multiverse
deb-src http://cn.archive.ubuntu.com/ubuntu dapper-updates main restricted universe multiverse
deb-src http://cn.archive.ubuntu.com/ubuntu dapper-backports main restricted universe multiverse
deb-src http://security.ubuntu.com/ubuntu dapper-security main restricted universe multiverse
    * 上海市 上海交通大学 更新服务器（教育网，推荐校园网和网通用户使用。）(目前已无法使用)： 
deb http://ftp.sjtu.edu.cn/ubuntu/ dapper main multiverse restricted universe
deb http://ftp.sjtu.edu.cn/ubuntu/ dapper-backports main multiverse restricted universe
deb http://ftp.sjtu.edu.cn/ubuntu/ dapper-proposed main multiverse restricted universe
deb http://ftp.sjtu.edu.cn/ubuntu/ dapper-security main multiverse restricted universe
deb http://ftp.sjtu.edu.cn/ubuntu/ dapper-updates main multiverse restricted universe
deb http://ftp.sjtu.edu.cn/ubuntu-cn/ dapper main multiverse restricted universe
    * 北京市清华大学 更新服务器（教育网，推荐校园网和网通用户使用。)： 
deb http://mirror9.net9.org/ubuntu/ dapper main multiverse restricted universe
deb http://mirror9.net9.org/ubuntu/ dapper-backports main multiverse restricted universe
deb http://mirror9.net9.org/ubuntu/ dapper-proposed main multiverse restricted universe
deb http://mirror9.net9.org/ubuntu/ dapper-security main multiverse restricted universe
deb http://mirror9.net9.org/ubuntu/ dapper-updates main multiverse restricted universe
deb http://mirror9.net9.org/ubuntu-cn/ dapper main multiverse restricted universe
    * 中国 台湾省台湾大学 更新服务器（推荐网通用户使用，电信PING平均响应速度41MS。）(强烈推荐,此源比较完整,较少出现同步问题) 
deb http://ubuntu.csie.ntu.edu.tw/ubuntu/ dapper main restricted universe multiverse
deb-src http://ubuntu.csie.ntu.edu.tw/ubuntu/ dapper main restricted universe multiverse
deb http://ubuntu.csie.ntu.edu.tw/ubuntu/ dapper-updates main restricted universe multiverse
deb-src http://ubuntu.csie.ntu.edu.tw/ubuntu/ dapper-updates main restricted universe multiverse
deb http://ubuntu.csie.ntu.edu.tw/ubuntu/ dapper-backports main restricted universe multiverse
deb-src http://ubuntu.csie.ntu.edu.tw/ubuntu/ dapper-backports main restricted universe multiverse
deb http://ubuntu.csie.ntu.edu.tw/ubuntu/ dapper-security main restricted universe multiverse
deb-src http://ubuntu.csie.ntu.edu.tw/ubuntu/ dapper-security main restricted universe multiverse
deb http://ftp.sjtu.edu.cn/ubuntu-cn/ dapper main multiverse restricted universe
    * Mirror.vmmatrix.net 更新服务器（上海市电信，推荐电信网通用户使用。）(目前已无法使用): 
deb http://mirror.vmmatrix.net/ubuntu/ dapper main restricted universe multiverse
deb-src http://mirror.vmmatrix.net/ubuntu/ dapper main restricted universe multiverse
deb http://mirror.vmmatrix.net/ubuntu/ dapper-updates main restricted universe multiverse
deb-src http://mirror.vmmatrix.net/ubuntu/ dapper-updates main restricted universe multiverse
deb http://mirror.vmmatrix.net/ubuntu/ dapper-backports main restricted universe multiverse
deb-src http://mirror.vmmatrix.net/ubuntu/ dapper-backports main restricted universe multiverse
deb http://mirror.vmmatrix.net/ubuntu/ dapper-security main restricted universe multiverse
deb-src http://mirror.vmmatrix.net/ubuntu/ dapper-security main restricted universe multiverse
deb http://ftp.sjtu.edu.cn/ubuntu-cn/ dapper main multiverse restricted universe
ubuntu.cnsite.org 更新服务器 (福建省福州市 电信):
deb http://ubuntu.cnsite.org/ubuntu/ dapper main restricted universe multiverse
deb-src http://ubuntu.cnsite.org/ubuntu/ dapper main restricted universe multiverse
deb http://ubuntu.cnsite.org/ubuntu/ dapper-updates main restricted universe multiverse
deb-src http://ubuntu.cnsite.org/ubuntu/ dapper-updates main restricted universe multiverse
deb http://ubuntu.cnsite.org/ubuntu/ dapper-backports main restricted universe multiverse
deb-src http://ubuntu.cnsite.org/ubuntu/ dapper-backports main restricted universe multiverse
deb http://ubuntu.cnsite.org/ubuntu/ dapper-security main restricted universe multiverse
deb-src http://ubuntu.cnsite.org/ubuntu/ dapper-security main restricted universe multiverse
deb http://ubuntu.cnsite.org/ubuntu-cn/ dapper main multiverse restricted universe
    *
到以下网址可以自定义产生若干源：http://www.ubuntulinux.nl/source-o-matic
以下网址有极其全面的源，以供补充：http://italy.copybase.ch/blog/lista-repository-
sourceslist-ottimizzata-per-ubuntu-kubuntu-linux/ 
# 保存编辑好的文件，执行以下命令更新。
保存编辑好的文件，执行以下命令更新。
sudo apt-get update
sudo apt-get dist-upgrade
        

---
layout: post
title:  "新手看招实用的UbuntuLinux命令大全（2）"
date:   2007-06-14 00:45:08
categories: 默认分类
tags:
---

* content
{:toc}

Ubuntu一些很必要的命令
安装
查看软件xxx安装内容
dpkg -L xxx
查找软件
apt-cache search 正则表达式
查找文件属于哪个包
dpkg -S filename
apt-file search filename
查询软件xxx依赖哪些包
apt-cache depends xxx
查询软件xxx被哪些包依赖
apt-cache rdepends xxx
增加一个光盘源
sudo apt-cdrom add
系统升级
sudo apt-get update
sudo apt-get upgrade
sudo apt-get dist-upgrade
清除所以删除包的残余配置文件
dpkg -l |grep ^rc|awk '{print $2}' |tr ["/n"] [" "]|sudo xargs dpkg -P -
编译时缺少h文件的自动处理
sudo auto-apt run ./configure
查看安装软件时下载包的临时存放目录
ls /var/cache/apt/archives
备份当前系统安装的所有包的列表
dpkg --get-selections | grep -v deinstall > ~/somefile
从上面备份的安装包的列表文件恢复所有包
dpkg --set-selections < ~/somefile
sudo dselect
清理旧版本的软件缓存
sudo apt-get autoclean
清理所有软件缓存
sudo apt-get clean
删除系统不再使用的孤立软件
sudo apt-get autoremove
系统
查看内核
uname -a
查看Ubuntu版本
cat /etc/issue
查看内核加载的模块
lsmod
查看PCI设备
lspci
查看USB设备
lsusb
查看网卡状态
sudo ethtool eth0
查看CPU信息
cat /proc/cpuinfo
显示当前硬件信息
lshw
硬盘
查看硬盘的分区
sudo fdisk -l
查看IDE硬盘信息
sudo hdparm -i /dev/hda
查看STAT硬盘信息
sudo hdparm -I /dev/sda
或
sudo apt-get install blktool
sudo blktool /dev/sda id
查看硬盘剩余空间
df -h
df -H
查看目录占用空间
du -hs 目录名
优盘没法卸载
sync
fuser -km /media/usbdisk
内存
查看当前的内存使用情况
free -m
进程
查看当前有哪些进程
ps -A
中止一个进程
kill 进程号(就是ps -A中的第一列的数字)
或者 killall 进程名
强制中止一个进程(在上面进程中止不成功的时候使用)
kill -9 进程号
或者 killall -9 进程名
图形方式中止一个程序
xkill 出现骷髅标志的鼠标，点击需要中止的程序即可
查看当前进程的实时状况
top
查看进程打开的文件
lsof -p
ADSL
配置 ADSL
sudo pppoeconf
ADSL手工拨号
sudo pon dsl-provider
激活 ADSL
sudo /etc/ppp/pppoe_on_boot
断开 ADSL
sudo poff
查看拨号日志
sudo plog
网络
根据IP查网卡地址
arping IP地址
查看当前IP地址
ifconfig eth0 |awk '/inet addr/ {split($2,x,":");print x[2]}'
查看当前外网的IP地址
w3m -no-cookie -dump www.ip138.com|grep -o '[0-9]/{1,3/}/.[0-9]/{1,3/}/.[0-9]/{1,3/}/.[0-9]/{1,3/}'
w3m -no-cookie -dump ip.loveroot.com|grep -o '[0-9]/{1,3/}/.[0-9]/{1,3/}/.[0-9]/{1,3/}/.[0-9]/{1,3/}'
查看当前监听80端口的程序
lsof -i :80
查看当前网卡的物理地址
arp -a | awk '{print $4}'
ifconfig eth0 | head -1 | awk '{print $5}'
立即让网络支持nat
sudo echo 1 > /proc/sys/net/ipv4/ip_forward
sudo iptables -t nat -I POSTROUTING -j MASQUERADE
查看路由信息
netstat -rn
sudo route -n
手工增加删除一条路由
sudo route add -net 192.168.0.0 netmask 255.255.255.0 gw 172.16.0.1
sudo route del -net 192.168.0.0 netmask 255.255.255.0 gw 172.16.0.1
修改网卡MAC地址的方法
sudo ifconfig eth0 down #关闭网卡
sudo ifconfig eth0 hw ether 00:AA:BB:CCD:EE #然后改地址
sudo ifconfig eth0 up #然后启动网卡
统计当前IP连接的个数
netstat -na|grep ESTABLISHED|awk '{print $5}'|awk -F: '{print $1}'|sort|uniq -c|sort -r -n
netstat -na|grep SYN|awk '{print $5}'|awk -F: '{print $1}'|sort|uniq -c|sort -r -n
统计当前20000个IP包中大于100个IP包的IP地址
tcpdump -tnn -c 20000 -i eth0 | awk -F "." '{print $1"."$2"."$3"."$4}' | sort | uniq -c | sort -nr | awk ' $1 > 100 '
屏蔽IPV6
echo "blacklist ipv6" | sudo tee /etc/modprobe.d/blacklist-ipv6
服务
添加一个服务
sudo update-rc.d 服务名 defaults 99
删除一个服务
sudo update-rc.d 服务名 remove
临时重启一个服务
/etc/init.d/服务名 restart
临时关闭一个服务
/etc/init.d/服务名 stop
临时启动一个服务
/etc/init.d/服务名 start
设置
配置默认Java使用哪个
sudo update-alternatives --config java
修改用户资料
sudo chfn userid
给apt设置代理
export http_proxy=http://xx.xx.xx.xx:xxx
修改系统登录信息
sudo vim /etc/motd
中文
转换文件名由GBK为UTF8
sudo apt-get install convmv
convmv -r -f cp936 -t utf8 --notest --nosmart *
转换文件内容由GBK为UTF8
iconv -f gbk -t utf8 $i > newfile
转换 mp3 标签编码
sudo apt-get install python-mutagen
find . -iname “*.mp3” -execdir mid3iconv -e GBK {} /;
控制台下显示中文
sudo apt-get install zhcon
使用时，输入zhcon即可
文件
快速查找某个文件
whereis filenamefind 目录 -name 文件名
查看文件类型
file filename
显示xxx文件倒数6行的内容
tail -n 6 xxx
让tail不停地读地最新的内容
tail -n 10 -f /var/log/apache2/access.log
查看文件中间的第五行（含）到第10行（含）的内容
sed -n '5,10p' /var/log/apache2/access.log
查找包含xxx字符串的文件
grep -l -r xxx .
查找关于xxx的命令
apropos xxx
man -k xxx
通过ssh传输文件
scp -rp /path/filename username@remoteIP:/path #将本地文件拷贝到服务器上
scp -rp username@remoteIP:/path/filename /path #将远程文件从服务器下载到本地
查看某个文件被哪些应用程序读写
lsof 文件名
把所有文件的后辍由rm改为rmvb
rename 's/.rm$/.rmvb/' *
把所有文件名中的大写改为小写
rename 'tr/A-Z/a-z/' *
删除特殊文件名的文件，如文件名：--help.txt
rm -- --help.txt 或者 rm ./--help.txt
查看当前目录的子目录
ls -d */. 或 echo */.
将当前目录下最近30天访问过的文件移动到上级back目录
find . -type f -atime -30 -exec mv {} ../back /;
将当前目录下最近2小时到8小时之内的文件显示出来
find . -mmin +120 -mmin -480 -exec more {} /;
删除修改时间在30天之前的所有文件
find . -type f -mtime +30 -mtime -3600 -exec rm {} /;
查找guest用户的以avi或者rm结尾的文件并删除掉
find . -name '*.avi' -o -name '*.rm' -user 'guest' -exec rm {} /;
查找的不以java和xml结尾,并7天没有使用的文件删除掉
find . ! -name *.java ! -name ‘*.xml’ -atime +7 -exec rm {} /;
统计当前文件个数
ls /usr/bin|wc -w
显示当前目录下2006-01-01的文件名
ls -l |grep 2006-01-01 |awk '{print $8}'
压缩
解压缩 xxx.tar.gz
tar -zxvf xxx.tar.gz
解压缩 xxx.tar.bz2
tar -jxvf xxx.tar.bz2
压缩aaa bbb目录为xxx.tar.gz
tar -zcvf xxx.tar.gz aaa bbb
压缩aaa bbb目录为xxx.tar.bz2
tar -jcvf xxx.tar.bz2 aaa bbb
Nautilus
显示隐藏文件
Ctrl+h
显示地址栏
Ctrl+l
特殊 URI 地址
* computer:/// - 全部挂载的设备和网络
* network:/// - 浏览可用的网络
* burn:/// - 一个刻录 CDs/DVDs 的数据虚拟目录
* smb:/// - 可用的 windows/samba 网络资源
* x-nautilus-desktop:/// - 桌面项目和图标
* file:/// - 本地文件
* trash:/// - 本地回收站目录
* ftp:// - FTP 文件夹
* ssh:// - SSH 文件夹
* fonts:/// - 字体文件夹，可将字体文件拖到此处以完成安装
* themes:/// - 系统主题文件夹
查看已安装字体
在nautilus的地址栏里输入”fonts:///“，就可以查看本机所有的fonts
程序
详细显示程序的运行信息
strace -f -F -o outfile 
日期和时间
设置日期
#date -s mm/dd/yy
设置时间
#date -s HH:MM
将时间写入CMOS
hwclock --systohc
读取CMOS时间
hwclock --hctosys
控制台
不同控制台间切换
Ctrl + ALT + ←
Ctrl + ALT + →
指定控制台切换
Ctrl + ALT + Fn(n:1~7)
控制台下滚屏
SHIFT + pageUp/pageDown
控制台抓图
setterm -dump n(n:1~7)
数据库
mysql的数据库存放在地方
/var/lib/mysql
从mysql中导出和导入数据
mysqldump 数据库名 > 文件名 #导出数据库
mysqladmin create 数据库名 #建立数据库
mysql 数据库名 < 文件名 #导入数据库
忘了mysql的root口令怎么办
sudo /etc/init.d/mysql stop
sudo mysqld_safe --skip-grant-tables &
sudo mysqladmin -u user password 'newpassword''
sudo mysqladmin flush-privileges
修改mysql的root口令
sudo mysqladmin -uroot -p password '你的新密码'
其它
下载网站文档
wget -r -p -np -k http://www.21cn.com
· -r：在本机建立服务器端目录结构；
· -p: 下载显示HTML文件的所有图片；
· -np：只下载目标站点指定目录及其子目录的内容；
· -k: 转换非相对链接为相对链接。
如何删除Totem电影播放机的播放历史记录
rm ~/.recently-used
如何更换gnome程序的快捷键
点击菜单，鼠标停留在某条菜单上，键盘输入任意你所需要的键，可以是组合键，会立即生效；
如果要清除该快捷键，请使用backspace.
        

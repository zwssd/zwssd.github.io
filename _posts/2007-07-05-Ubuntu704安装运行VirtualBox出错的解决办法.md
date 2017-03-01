---
layout: post
title:  "Ubuntu7.04安装运行VirtualBox出错的解决办法"
date:   2007-07-05 22:08:35
categories: 默认分类
tags:
---

* content
{:toc}

ATAL: Error inserting vboxdrv (/lib/modules/2.6.20-15-generic/kernel/ubuntu/misc/vbox/vboxdrv.ko): Invalid argument     * Modprobe vboxdrv failed. Please use 'dmesg' to find out why. 
如果安装时显示上面出错信息
                        sudo gedit /boot/grub/menu.lst            
禁止 NMI watchdog,在 kernel 命令行加上 nmi_watchdog=0
我的menu.lst
                         ......
kernel        /boot/vmlinuz-2.6.20-15-generic root=UUID=5bc0c3bc-6b8b-41a5-93fb-1348396c3d1a ro quiet splash nmi_watchdog=0 locale=zh_CN
......            
然后保存 退出 重启!
如果运行时遇到 VirtualBox kernal driver not accessible,permission problem.
                        sudo chmod 777 /dev/vboxdrv            
上面的只是临时的办法
 按照VirtualBox的安全设置，如果要使用VirtualBox需要将您的使用的用户添加到vboxusers组中：  
                        sudo usermod -G vboxusers -a your_account
            
如若提示vboxusers组还未建立，则
                        sudo dpkg-reconfigure virtualbox
            
若想使用usb设备，则会出现：Not permitted to open the USB device, check usbfs options.
 首先建立usbfs组                         sudo addgroup usbfs
            
注意usbfs组的id号，假如是1002 
然后修改/etc/fstab
                        sudo gedit /etc/fstab            
添加一行
                        none /proc/bus/usb usbfs devgid=1002,devmode=664 0 0            把当前帐号加入到usbfs组中                        sudo usermod -G usbfs -a your_account
            
重启X
Trackback: http://tb.blog.csdn.net/TrackBack.aspx?PostId=1598613
        

---
layout: post
title:  "usbmouseforubuntu7.04"
date:   2007-05-22 10:01:30
categories: 默认分类
tags:
---

* content
{:toc}


使用dmesg|tail如下：
[17184303.900000] usb 1-4 USB disconnect, address 2
[17184307.900000] ohci_hcd 0000:00:0b.0: IRQ INTR_SF lossage 
搜了一下发现有人也有这种情况！
具体方法如下：
gedit/boot/grub/menu.lst
kernel /vmlinuz-2.6.15-27-386 root=/dev/hda7 ro noapic irqpoll quiet splash
initrd /initrd.img-2.6.15-27-386 
说acpi是高级电源管理，要关掉才行！
        

---
layout: post
title:  "vmwareplayer的xp的vmx文件"
date:   2007-06-07 22:20:46
categories: 默认分类
tags:
---

* content
{:toc}

两个硬盘,一个是 WindowsXP.vmdk ,一个是 Other.vmdk ，一个光驱 CDROM.iso
 
   代码:
 
 
   sudo apt-get install qemu
qemu-img create -f vmdk WindowsXP.vmdk 4G
qemu-img create -f vmdk Other.vmdk 4G
 
 
   代码:
 
 
   #!/usr/bin/vmware
displayName = "Windows XP"
guestOS = "winxppro"
memsize = "256"
ide0:0.fileName = "WindowsXP.vmdk"
ide0:1.fileName = "Other.vmdk"
ide1:0.fileName = "CDROM.iso"
# DEFAULT SETTINGS UNDER THIS LINE
config.version = "8"
virtualHW.version = "3"
MemAllowAutoScaleDown = "TRUE"
MemTrimRate = "-1"
uuid.location = "56 4d 21 a2 6d 0a ca 11-76 a8 cb 50 9c 1c ad 4b"
uuid.bios = "56 4d 21 a2 6d 0a ca 11-76 a8 cb 50 9c 1c ad 4b"
uuid.action = "create"
checkpoint.vmState = "WindowsXP.vmss"
ethernet0.present = "TRUE"
ethernet0.connectionType = "nat"
ethernet0.addressType = "generated"
ethernet0.generatedAddress = "00:0c:29:1c:ad:4b"
ethernet0.generatedAddressOffset = "0"
usb.present = "TRUE"
usb.generic.autoconnect = "FALSE"
sound.present = "TRUE"
sound.virtualdev = "es1371"
scsi0.present = "FALSE"
scsi0.virtualdev = "lsilogic"
scsi0:0.present = "FALSE"
scsi0:0.deviceType = "disk"
scsi0:0.mode = "persistent"
scsi0:0.redo = ""
scsi0:0.writeThrough = "FALSE"
scsi0:0.startConnected = "FALSE"
scsi0:1.present = "FALSE"
scsi0:1.deviceType = "disk"
scsi0:1.mode = "persistent"
scsi0:1.redo = ""
scsi0:1.writeThrough = "FALSE"
scsi0:1.startConnected = "FALSE"
floppy0.present = "FALSE"
ide0:0.present = "TRUE"
ide0:1.present = "TRUE"
ide1:1.present = "FALSE"
ide1:0.present = "TRUE"
ide1:0.deviceType = "cdrom-image"
ide1:0.autodetect = "TRUE"
ide1:0.startConnected = "TRUE"
ide0:0.redo = ""
tools.syncTime = "TRUE"
ide0:1.redo = ""
logging = "FALSE"
isolation.tools.hgfs.disable = "FALSE"
isolation.tools.dnd.disable = "TRUE"
isolation.tools.copy.enable = "TRUE"
isolation.tools.paste.enabled = "TRUE"
hints.hideAll = "TRUE"
priority.grabbed = "normal"
priority.ungrabbed = "normal"
usb.autoConnect.device0 = ""
        

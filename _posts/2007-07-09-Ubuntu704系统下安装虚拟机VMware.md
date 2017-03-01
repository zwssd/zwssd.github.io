---
layout: post
title:  "Ubuntu7.04系统下安装虚拟机VMware"
date:   2007-07-09 14:34:43
categories: 默认分类
tags:
---

* content
{:toc}

                 一、安装依赖包 sudo apt-get install libx11-6 libx11-dev libxtst6 xinetd sudo apt-get install linux-headers-`uname -r` build-essential 二、从vmware官方网站下载最新版vmware-server for linux(ver:1.0.2) http://www.vmware.com/download/server/ 记得要注册取得授权号码哟 三、解压并安装 tar zxvf VMware-server-1.0.2-39867.tar.gz cd vmware-server-distrib sudo vmware-install.pl 可以直接一路默认下去就好，但这不会安装成功，会出现以下错误： Building the vmmon module. Using 2.6.x kernel build system. make: Entering directory `/tmp/vmware-config0/vmmon-only' make -C /lib/modules/2.6.20-15-generic/build/include/.. SUBDIRS=$PWD SRCROOT=$PWD/. modules make[1]: Entering directory `/usr/src/linux-headers-2.6.20-15-generic' CC [M] /tmp/vmware-config0/vmmon-only/linux/driver.o In file included from /tmp/vmware-config0/vmmon-only/linux/driver.c:80: /tmp/vmware-config0/vmmon-only/./include/compat_kernel.h:21: 错误： expected declaration specifiers or ‘...’ before ‘compat_exit’ /tmp/vmware-config0/vmmon-only/./include/compat_kernel.h:21: 错误： expected declaration specifiers or ‘...’ before ‘exit_code’ /tmp/vmware-config0/vmmon-only/./include/compat_kernel.h:21: 警告： 在 ‘_syscall1’ 的声明中，类型默认为 ‘int’ make[2]: *** [/tmp/vmware-config0/vmmon-only/linux/driver.o] 错误 1 make[1]: *** [_module_/tmp/vmware-config0/vmmon-only] 错误 2 make[1]: Leaving directory `/usr/src/linux-headers-2.6.20-15-generic' make: *** [vmmon.ko] 错误 2 make: Leaving directory `/tmp/vmware-config0/vmmon-only' Unable to build the vmmon module. For more information on how to troubleshoot module-related problems, please visit our Web site at "http://www.vmware.com/download/modules/modules.html" and "http://www.vmware.com/support/reference/linux/prebuilt_modules_linux.html". Execution aborted. 不要理它！我们去下载patch搞定 四、下载并安装patch包 wget http://jaws.go2linux.org/archivos/vmware-any-any-update109.tar.gzcd vmware-any-any-update109sudo ./runme.pl 接下来一路next就好(会出现一些警告错误，说有函数在使用前未初始化)   
        

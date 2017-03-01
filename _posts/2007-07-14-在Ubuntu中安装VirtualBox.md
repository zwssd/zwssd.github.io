---
layout: post
title:  "在Ubuntu中安装VirtualBox"
date:   2007-07-14 09:05:13
categories: 默认分类
tags:
---

* content
{:toc}

是一款很不错的虚拟机软件，其最令人称道的地方就在于所虚拟系统的性能比较接近于实机，感觉十分快速。另外，它还是发布在 GPL 许可协议之下的开源软件，所以你可以自由地加以使用。  安装 VirtualBox  在 Ubuntu 中安装 VirtualBox 可以按照以下的步骤进行操作：  下载安装包。建议下载 Deb 包，方便直接在 Ubuntu 中安装。你需要根据所使用的 Ubuntu 版本来有选择地进行下载。如果是  Dapper 版，就下载适用于 Dapper 的安装包。其他适用于 Ubuntu 的版本还包括 Edgy 和  Feisty。我们这里以下载适用于 Ubuntu Feisty 的 VirtualBox 1.3.8 版来介绍。你可以从这里找到对应的版本。准备依赖。VirtualBox 的正常使用需要 libxalan110 和 libxerces27 这两个包。所以，你要先行安装它们，可以使用下面的指令：
  sudo apt-get install libxalan110 libxerces27安装编译工具及相关包。在安装过程中，要编译 VirtualBox 所用的内核模块。为此，你需要准备基本的编译工具及包，你可以使用下列指令来安装它们：
  sudo apt-get install build-essential linux-headers-`uname -r`现在，转到所保存 VirtualBox 安装包的目录，通过下面的指令来安装它：
  sudo dpkg -i VirtualBox_1.3.8_Ubuntu_feisty_i386.deb
  注意，我们这里是以 Ubuntu Feisty 为例，如果你在其他 Ubuntu 版本中安装，请使用相应的安装包代替。在安装的过程中，VirtualBox 会要求你接受许可协议。另外，安装程序也会创建 vboxusers 用户组，并编译所需的内核模块。现在，你还不能启动 VirtualBox，因为你的当前用户还不属于 vboxusers 用户组。你可以使用下面的指令来将当前的用户添加到 vboxusers 用户组中：
  sudo adduser xu vboxusers
  同样的，请使用你的用户名代替指令中的“xu”。  VirtualBox 默认是安装到 /opt/VirtualBox-x.x.x 目录，其中 x.x.x 代表 VirtualBox 的版本号。你可以直接在终端中敲入 VirtualBox 启动程序。  
  VirtualBox 主窗口  
  在 VirtualBox 中运行 Windows 2000  
        

---
layout: post
title:  "Ubuntu下Totem播放Rmvb没有声音的问题"
date:   2007-06-12 18:52:29
categories: 默认分类
tags:
---

* content
{:toc}

Ubuntu 7.04自带的电影播放器totem是无法播放rmvb文件的，不幸的是，rmvb文件是网络上下载电影的主流格式。 
如果第一次用自带的totem打开rmvb文件，在无法打开文件的同时会提示寻找插件，但安装后，totem是只有声音没有图像。 
使用linux的一个好处就是，google的使用率明显增加了，经过搜索ubuntu中文论坛并总结，解决如下： 
1、在新立得软件管理中搜索totem，卸载系统自带的totem播放器。 
2、安装gstreamer的解码器。 
sudo apt-get install gstreamer0.10-pitfdll gstreamer0.10-ffmpeg gstreamer0.10-plugins-bad gstreamer0.10-plugins-bad-multiverse gstreamer0.10-plugins-ugly gstreamer0.10-plugins-ugly-multiverse 
3、安装xine及解码器。 
sudo apt-get install libxine-extracodecs totem-xine ffmpeg lame faad sox mjpegtools libxine-main1 
4、安装w32codecs。 
sudo apt-get install w32codecs 
如果显示找不到，说明7.04的源里面还没有，到下面的地址下载安装： 
http://www.debian-multimedia.org/pool/main/w/w32codecs/ 
选择w32codecs_20061022-0.0_i386.deb 就可以自动安装了。 
5、至此，totem可以播放rmvb了。但不幸的是，有了图像，没有了声音。 
6、再次google，解决办法如下： 
编辑 ~/.xine/catalog.cache文件： 
sudo gedit ~/.xine/catalog.cache 
找到： 
[/usr/lib/xine/plugins/1.1.4/xineplug_decode_real_audio.so] 
把 decoder_priority 后面的数字修改为 10 
保存退出。 
（责任编辑：凌云通） 
        

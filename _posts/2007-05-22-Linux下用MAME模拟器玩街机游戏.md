---
layout: post
title:  "Linux下用MAME模拟器玩街机游戏"
date:   2007-05-22 20:46:23
categories: 默认分类
tags:
---

* content
{:toc}

    也许是因为我显卡的原因才使得安装运行XMAME变得和大家不一样，网上找的方法大部分都不行，直到找到wargames兄的一个贴子xmame goes to black screen then nothing?（http://www.ubuntuforums.org/archive/index.php/t-104222.html），才真正的解决了问题，    先说下我的Ubuntu环境，只装一个GCC编译器和ATI Radeon 9550的显卡驱动，这些安装的都是源里的，其它的一些设置也都是参照UbuntuChina Wiki(http://wiki.ubuntu.org.cn/)。    这里安装的是XMAME-SDL版，因为安装XMAME-X11版一运行就出现黑屏。直接从源里安装软件。    sudo apt-get install xmame-sdl    然后在终端就可以直接输入xmame命令运行程序了，不过还得设置xmame的运行环境。在运行程序之前行运行，先生成一个xmame的配置文件~/.xmame/xmamerc    xmame --showconfig > ~/.xmame/xmamerc    在这个文件里有很多xmame的选项(fullscreen, widthscale，heightscale，rompath，snapshot_directory等）    这些选项里我觉得rompath必须设置好，如果要玩KOF的话，里面必须有一个neogeo.zip文件，要是没有加载的时候就会提示说找不到有些文件，还有的是这文件直接就是在rompath路径下，不要解压出来。     另外一个要说是snapshot_directory，在设置这个的时候，我把它设置为/home/proming/.xmame/snap，可是按  F12就是没有截图产生，因为在/home/proming/.xmame/下根本就没有snap文件夹，后来试着自己手动创建了一个，玩游戏的时候按截  图，在snap下就有生成的图片。所以在设置这个路径的时候要设置在一个实际存在的目录下。        其它的一些选项，搜下就有很多介绍了。    希望这篇文章对那些出现同样问题的人有帮助，如果有需要neogeo.zip或者其它帮助的可是直接留言或者E我：promingx@gmail.com。
    
    下面的是KOF97的截图：
  
  
另外附上我的配置文件：
### xmame running parameters ###
### Video Related ###
video-mode              0
fullscreen              0
arbheight               0
widthscale              2
heightscale             2
effect                  0
autodouble              1
frameskipper            1
throttle                1
frames_to_run           0
sleepidle               1
autoframeskip           1
maxautoframeskip        8
frameskip               0
brightness              1.000000
pause_brightness        0.650000
gamma                   1.000000
norotate                0
ror                     0
rol                     0
autoror                 0
autorol                 0
flipx                   0
flipy                   0
### Use additional game artwork? ###
artwork                 1
use_backdrops           1
use_overlays            1
use_bezels              1
artwork_crop            0
artwork_scale           1
### Vector Games Related ###
beam                    1.000000
flicker                 0.000000
intensity               1.500000
antialias               1
translucency            1
hardware-vectors        1
### Aspect ratio handling ###
keepaspect              1
perfectaspect           0
displayaspectratio      1.333333
### SDL Related ###
doublebuf               1
grabinput               0
alwaysusemouse          0
cursor                  1
### Video Mode Selection Related ###
### Input device options ###
joytype                 0
analogstick             0
joydevname              /dev/js
ugcicoin                0
# lircrc                <NULL> (not set)
steadykey               0
a2d_deadzone            0.300000
# ctrlr                 <NULL> (not set)
digital                 none
usbpspad                0
rapidfire               0
### Sound Related ###
samples                 1
samplefreq              44100
bufsize                 3.000000
volume                  -3
# audiodevice           <NULL> (not set)
# mixerdevice           <NULL> (not set)
### Digital sound related ###
# dsp-plugin            <NULL> (not set)
timer                   0
### Sound mixer related ###
# sound-mixer-plugin    <NULL> (not set)
### File I/O-related ###
rompath                 /media/g/roms
samplepath              /usr/share/games/xmame/samples
inipath                 /etc/xmame/ini
cfg_directory           /home/proming/.xmame/cfg
nvram_directory         /home/proming/.xmame/nvram
memcard_directory       /home/proming/.xmame/memcard
input_directory         /home/proming/.xmame/inp
hiscore_directory       /home/proming/.xmame/hi
state_directory         /home/proming/.xmame/sta
artwork_directory       /usr/share/games/xmame/artwork
snapshot_directory      /home/proming/.xmame/snap
diff_directory          /home/proming/.xmame/diff
ctrlr_directory         /etc/xmame/ctrlr
cheat_file              /usr/share/games/xmame/cheat.dat
hiscore_file            /usr/share/games/xmame/hiscore.dat
# record                <NULL> (not set)
# playback              <NULL> (not set)
### MAME Related ###
defaultgame             pacman
language                english
fuzzycmp                1
cheat                   0
skip_disclaimer         0
skip_gameinfo           0
bios                    default
# state                 <NULL> (not set)
autosave                0
### Frontend Related ###
clones                  1
### Internal verification list commands (only for developers) ###
### Rom Identification Related ###
### General Options ###
loadconfig              1
        

---
layout: post
title:  "用python抓取海盗湾(https://thepiratebay.vg)的热门资源列表的磁链"
date:   2015-05-20 14:24:07
categories: python
tags:
---

* content
{:toc}

#!/bin/python
# -*- coding: utf-8 -*-
#获得海盗湾的种子地址
import pycurl
import StringIO
from bs4 import BeautifulSoup
import re
def headerCookie(buf):
    print buf
def getEnterRandCode():
    #得到登录验证码图
    curl = pycurl.Curl()
    f = StringIO.StringIO()
    curl.setopt(pycurl.URL, "https://thepiratebay.vg/top/201")
    curl.setopt(pycurl.WRITEFUNCTION, f.write)
    curl.setopt(pycurl.SSL_VERIFYPEER, 0)
    curl.setopt(pycurl.SSL_VERIFYHOST, 0)
    curl.setopt(pycurl.TIMEOUT, 300) #连接超时设置
    curl.setopt(pycurl.USERAGENT, "Mozilla/4.0 (compatible; MSIE 6.0; Windows NT 5.1; SV1; .NET CLR 1.1.4322)") #模拟浏览器
    curl.setopt(pycurl.HEADERFUNCTION, headerCookie)
    curl.setopt(pycurl.COOKIEJAR,"cookie_file_name")
    curl.perform()
    backinfo = ''
    print curl.getinfo(pycurl.RESPONSE_CODE)
    if curl.getinfo(pycurl.RESPONSE_CODE) == 200:
        backinfo = f.getvalue()
    #print f.getvalue()
    curl.close()
    # 生成一个soup对象，doc就是步骤二中提到的
    #backinfo = '<!DOCTYPE html><html lang="en"><head></head><body><tr><td class="vertTh"><center><a href="/browse/200" title="More from this category">Video</a><br />(<a href="/browse/201" title="More from this category">Movies</a>)</center></td><td><div class="detName"><a href="/torrent/11922860/Kingsman.The.Secret.Service.2014.HDRip.XviD.AC3-EVO" class="detLink" title="Details for Kingsman.The.Secret.Service.2014.HDRip.XviD.AC3-EVO">Kingsman.The.Secret.Service.2014.HDRip.XviD.AC3-EVO</a></div><a href="magnet:?xt=urn:btih:e28616251f07abcef6288f6034661b5da821f9bf&dn=Kingsman.The.Secret.Service.2014.HDRip.XviD.AC3-EVO&tr=udp%3A%2F%2Ftracker.openbittorrent.com%3A80&tr=udp%3A%2F%2Fopen.demonii.com%3A1337&tr=udp%3A%2F%2Ftracker.coppersurfer.tk%3A6969&tr=udp%3A%2F%2Fexodus.desync.com%3A6969" title="Download this torrent using magnet"><img src="/static/img/icon-magnet.gif" alt="Magnet link" /></a><img src="/static/img/icon_comment.gif" alt="This torrent has 13 comments." title="This torrent has 13 comments." /></font></td><td align="right">12508</td><td align="right">11010</td></tr></body></html>'
    soup = BeautifulSoup(backinfo)
    paper_name = ''
    for elem in soup.html.body.find_all('a', {'title' : 'Download this torrent using magnet'}):
        paper_name += elem.get('href')+'\n'
    
    file_object = open('/home/david/Downloads/a.html', 'w')
    file_object.write(paper_name)
    file_object.close( )
    print paper_name
getEnterRandCode()
        

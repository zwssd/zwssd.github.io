---
layout: post
title:  "用python实现chinaren（中国人）幼儿园（校友录）照片的批量下载"
date:   2011-04-15 14:15:37
categories: python
tags:
---

* content
{:toc}

啥都不说了，看代码。小孩子自从上了幼儿园以后，老师就将好多孩子在幼儿园的照片发到了网上，由于数量很多，一张一张下载起来太费劲了，故作此程序，希望能给自己带来方便的同时也给大家带来方便。
好，看代码：<!--excerpt-->
#!/usr/bin/python 
#coding=UTF-8 
#Last Change:  2010-11-16 16:14:17 
 
import re
import os
import sys
import time
import glob 
import string 
import socket 
import getopt 
import urllib 
import urllib2 
import threading 
from sgmllib import SGMLParser 
 
import cookielib 
 
if __name__ == "__main__": 
    #登陆discuz论坛
    cj = cookielib.CookieJar()
    opener = urllib2.build_opener(urllib2.HTTPCookieProcessor(cj))
    loginurl="http://kids.sohu.com/login.do"
    loginsubmiturl="http://kids.sohu.com/login2.do"
    myvalues = {
            'email':'***',
            'password':'***',
            'sLoginInput':'1',
            'persistentcookie':'1'
    }
    data = urllib.urlencode(myvalues)
    myrequest = urllib2.Request(loginsubmiturl, data)
    rq = opener.open(myrequest)
    urls  =  [] 
    urls2 = [] 
    #chinaren幼儿园url地址 
    photourlstart = 'http://kids.sohu.com/album/list.do?type=list&typeid=' 
    photourlend = '&groupid=1&pid=646812&reload=1&p=' 
    #存储的目录 
    songdir = "/home/david/Downloads/chinarenphoto/"; 
    #songdir = "d:/photo/chinarenphoto/"; 
    #读取共有多少个专辑
    photolist = 'http://kids.sohu.com/album/index.do?type=classpic&classuuid=646812'
    responselist = urllib2.Request(photolist) 
    responselist = opener.open(responselist)
    htmllist = responselist.read()
    patternlist = '<span id="ctimeT_(.+?)">'
    urlslist = re.findall(patternlist, htmllist,re.S) 
    #print urlslist
    
    for myphotolist in urlslist:
        photolist = photourlstart+myphotolist+photourlend
        #读取专辑共多少页
        mypagenum = 0
        responsepage = urllib2.Request(photolist+'1') 
        responsepage = opener.open(responsepage)
        htmlpage = responsepage.read() 
        #print htmlpage
        patternpage = '<strong>(.+?)</strong>'
        urlspage = re.findall(patternpage, htmlpage,re.S) 
        patternpage = '<b>(.+?)</b>'
        urlspage = re.findall(patternpage, urlspage[0],re.S) 
        #print urlspage
        for mypage in urlspage:
            mypagenum = mypage
        #print mypagenum
        #开始读取图片地址并下载
        for urlpage in range(1,int(mypagenum)+1):
            #response = urllib2.urlopen(photolist) 
            response = urllib2.Request(photolist+str(urlpage)) 
            response = opener.open(response)
            html = response.read() 
            #print html 
            #pattern = '<img src=(.+?) onmouseover'
            pattern = 'onmouseover="AlbumAdmin.onMouseChg\((.+?),1\)'
            urls = re.findall(pattern, html,re.S) 
            myurltest = '' 
            for urltest in urls: 
                filename = songdir + urltest + '.jpg'
                urltest = 'http://up2.upload.chinaren.com/classpic/812/646/646812/'+urltest+'.jpg'
                print filename 
                urllib.urlretrieve(urltest, filename)
        

---
layout: post
title:  "Diabloimage：托管于GoogleAppEngine的相册程序安装全解2009-10-22"
date:   2011-03-09 11:54:23
categories: 技术文章
tags:
---

* content
{:toc}

恋吧昨晚发过“基于Google App Engine的大菠萝相册”diabloimage 0.05 博文，下面再啰嗦一遍。Diabloimage是一款托管于Google App Engine的图片相册程序，它利用GAE的数据库存储文件，而且上传后的图片不加水印也不压缩，还可以进行随意外链，如果你拥有一个独立域名的话，甚至还可以绑定上你的域名。
 
 利用Diabloimage，我们可以很方便的在GAE平台上创建自己的相册程序。
 
 首先我们要免费申请一个GAE平台的账号，申请地址 ，注册之后创建一个App应用，记下创建的Application Identifier
 
 第二步：下载GAE开发包和最新版Python，安装这两个程序
 
 第三步：在http://code.google.com/p/diabloimage/downloads/list下载最新的Diablo image图片程序。解压之后，打开修改其中的app.yml文件
 当然我用的是SDUpload 0.1这个第三方上传工具。一样的原理，不过这个工具更简单！具体的可以参见恋吧以前发过的博文“用第三方上传工具SDUpload上传文件到Google App Engine” 
 
 application: 
 version: 1
 runtime: python
 api_version: 1
 
 handlers:
 - url: /favicon.ico
 static_files: static/favicon.ico
 upload: static/favicon.ico
 mime_type: image/x-icon
 expiration : "1d"
 
 - url: /robots.txt
 static_files: static/robots.txt
 upload: static/robots.txt
 
 - url: /static
 static_dir: static
 expiration : "1d"
 secure: optional
 
 - url: /admin/.*
 script: admin.py
 secure: optional
 
 - url: .*
 script: main.py
 secure: optional在第一行中冒号后面填入创建的app id，保存退出
 
 第四步：上传整个SRC文件夹。在“运行”中输入“cmd”打开DOS窗口，用“CD”命令进入程序文件夹的上级目录，利用如下命令进行上传
 
 
 appcfg.py update src
 
 在上传过程中，将会提示输入你的google账号以及密码，注意：在输入密码的时候，不会显示星号
 
 这样来，你就拥有了一个托管于GAE平台的自由相册程序，相册的访问地址就是 http://你的appid.appspot.com
 
 你可以点击这里查看作者的演示。
 
 我在上传的时候遇到过这样一个问题：
 
 Error parsing yaml file:
 Unexpected attribute 'secure' for object
 pinfo.URLMap'>.
 in "src\app.yaml", line 20, column 11如果你也出现了这样的错误，只需要把app.yml文件中三处“sercue: optional”删除即可，而如果你只想自己一个人上传图片，可以在
 script: admin.py
 后面加入一行
 login: admin
 
 恋吧自己没有试过，不加这一行代码，是不是每个人都可以上传呢？望有兴趣的朋友试下反馈下消息！还有几个关于Google App Engine的文章：
 用免费的Google App Engine建强大的CMS网站   
 
 用免费的Google App Engine建立强大的Blog网站  
 
 在Google Appengine上安装SDblog
        

---
layout: post
title:  "ubuntudesktop安装编译器"
date:   2007-06-19 19:30:41
categories: 默认分类
tags:
---

* content
{:toc}

ubuntu desktop 默认是不装编译器的
如果直接sudo apt-get install gcc也是不行的，因为gcc还是需要整个编译工具链的。
应该如此sudo apt-get install build-essential
        

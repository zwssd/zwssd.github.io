---
layout: post
title:  "mustbesetuidroot?"
date:   2007-06-19 17:18:30
categories: 默认分类
tags:
---

* content
{:toc}

这个问题我也遇见过，好像原因是sudo必须属于root。
解决:
su
chown root /usr/bin/sudo
chgrp root /usr/bin/sudo
chmod 4111 /usr/bin/sudo
        

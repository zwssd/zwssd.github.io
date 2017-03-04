---
layout: post
title:  "解决Debian中由于&quot;StartingMTA...&quot;造成启动慢的问题2009-5-22"
date:   2011-03-09 11:24:11
categories: linux
tags:
---

* content
{:toc}

解决Debian中由于"Starting MTA..."造成启动慢的问题 我用的是Debian Lenny,我发现每次启动到 "Starting MTA..." 时都得等上很久，有时候甚至得一分多钟，太难受了。  今天在网上找到了一个解决办法，分享一下。  出现这种情况的原因:   MTA(message transfer agent,默认装的是Exim) 在启动时会进行DNS lookups(DNS查找)  操作,而如果是拔号上网或是像我用Reijie的话,系统会尝试进行网络连接(即使是连接失败),这将会尝试很长一段时间,所以造成启动慢的问题. (  这里有详细的解释: Exim 4 for Debian 中的 2.1.1.10. Keep number of DNS queries  minimal (Dial-on-Demand) )   解决办法: 1. 编辑文件: /etc/exim4/update-exim4.conf.conf , 找到 dc_minimaldns 字段,并设置为: dc_minimaldns='true';  2. 重新设置 exim. 运行:sudo dpkg-reconfigure exim4-config  到时选择 Yes 即可。 ====================分割线========================== 如果用不到邮件路由，可以用sysv-rc-conf或者rcconf禁用掉exim服务即可
        

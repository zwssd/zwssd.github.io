---
layout: post
title:  "ubuntuadsl上网掉线我的解决"
date:   2007-05-23 17:42:25
categories: 默认分类
tags:
---

* content
{:toc}

在ubuntu下用pppoeconf , pon, poff进行adsl上网 的操作，但经常会出现每隔一，两分钟就断线的情况。用plog查看日志显示为modem has hungup。今天经过尝试，在我这里基本解决了问题。
首先，关于pppoe的配置都在/etc/ppp/options这个文件里。其中有这样两个option:
# If this option is given, pppd will send an LCP echo-request frame to the# peer every n seconds. Normally the peer should respond to the echo-request# by sending an echo-reply. This option can be used with the# lcp-echo-failure option to detect that the peer is no longer connected.lcp-echo-interval 30# If this option is given, pppd will presume the peer to be dead if n# LCP echo-requests are sent without receiving a valid LCP echo-reply.# If this happens, pppd will terminate the connection.  Use of this# option requires a non-zero value for the lcp-echo-interval parameter.# This option can be used to enable pppd to terminate after the physical# connection has been broken (e.g., the modem has hung up) in# situations where no hardware modem control lines are available.lcp-echo-failure 4
这说明了plog显示modem hungup 的原因（虽然不知道是不是正确）也是连接时常断掉的原因
所以我将lcp-echo-failure 的数值改为了 100。到现在为止都能够长时间的保持连接。尚未出现连续中断的现象，希望对大家有用
        

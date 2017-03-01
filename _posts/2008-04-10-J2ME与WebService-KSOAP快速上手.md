---
layout: post
title:  "J2ME与WebService-KSOAP快速上手"
date:   2008-04-10 11:24:10
categories: 编程设计
tags:
---

* content
{:toc}

本文用一个简明的例子，阐述了如何在手机上用KSOAP API来访问本地服务器上的Web Service。并且给出了详细的操作步骤和全部的Server，Client源代码。1. 服务端这次要发布的web service非常简单。它的功能是把从客户端传入的字符串中的小写字母转变成大写字母，再返回给客户端。Soap 服务器采用apache的AXIS（可以从http://ws.apache.org/axis/下载），应用服务器可以选用各种servlet 容器，我这里采用的是weblogic。1.1 实现类的源代码// StringProcessor.java
package com.jagie.j2me.ws;public class StringProcessor {
public StringProcessor() {
}public String process(String name){
return name.toUpperCase();
}}1.2 发布步骤1．准备一个目录作为web application的发布目录,我这里的这个目录叫jagiews,这个目录的全路径中最好不要有空格和中文。我的发布目录结构如下： 2．编译StringProcessor.java,把生成的StringProcessor.class置于: \jagiews\WEB-INF\classes\com\jagie\j2me\ws目录下。3.在jagiews\WEB-INF\lib 文件夹中置入以下axis服务器需要的jar文件 axis.jar，axis-ant.jar，commons-discovery.jar，commons-logging.jar，jaxrpc.jar，log4j-1.2.8.jar，saaj.jar ，wsdl4j.jar。这些文件可以在http://ws.apache.org/axis/下载，如图所示： 4.在jagiews\WEB-INF目录下增加2个发布描述文件：server-config.wsdd，web.xml。#server-config.wsdd
xmlns:java="http://xml.apache.org/axis/wsdd/providers/java"> value="org.apache.axis.attachments.AttachmentsImpl"/>      
type="java:org.apache.axis.transport.local.LocalResponder"/>
type="java:org.apache.axis.handlers.http.URLMapper"/>
type="java:org.apache.axis.providers.java.RPCProvider"/>
type="java:org.apache.axis.handlers.SimpleAuthenticationHandler"/>
type="java:org.apache.axis.providers.java.MsgProvider"/>  http://xml.apache.org/axis/wsdd/   value="com.jagie.j2me.ws.StringProcessor"/>      
# web.xml PUBLIC "-//Sun Microsystems, Inc.//DTD Web Application 2.3//EN"
"http://java.sun.com/j2ee/dtds/web-app_2.2.dtd">
Apache-AxisAxisServlet
Apache-Axis Servletorg.apache.axis.transport.http.AxisServlet本篇文章来源于 Linux爱好网|www.linux-love.com 原文链接：http://www.linux-love.com/programme/20080405/J2MEyuWeb-Service-KSOAPkuaisushangshou_7004.shtmlAdminServlet
Axis Admin Servletorg.apache.axis.transport.http.AdminServlet100 SOAPMonitorService
SOAPMonitorServiceorg.apache.axis.monitor.SOAPMonitorService
SOAPMonitorPort
5001100 AxisServlet
/servlet/AxisServlet AxisServlet
*.jws AxisServlet
/services/* SOAPMonitorService
/SOAPMonitor AdminServlet
/servlet/AdminServlet http://www.w3.org/TR/2003/WD-wsdl12-20030303/#ietf-draft
for now we go with the basic 'it's XML' responsewsdl
text/xml
xsd
text/xml 5.开启你的application server，把目录jagiews发布为一个名叫jagiews的web application。6.测试:打开浏览器，输入网址（这里使用的是weblogic,其他的服务器请酌情修改）: http://localhost:7001/jagiews/se ... d=process&name=qqqq，如果浏览器能在返回的xml文档中显示字符串"QQQQ"，恭喜你，你的web service发布成功了。如果发布不成功，请按以上发布步骤检查一下。2. 客户端客户端自然是用MIDlet了，不过用什么方式来访问web service呢？其实有3种访问方式直接用HttpConnection访问 http://localhost:7001/jagiews/se ... d=process&name=qqqq，得到xml的返回数据，然后用kxml(http://kxml.enhydra.org/)解析，得到返回值。 
如果你的手机支持MIDP2.0的话，可以考虑使用JSR172。 
用ksoap api。 
这里讲述第三种方式。使用之前，你需要从 http://ksoap.enhydra.org/software/downloads/index.html下载稳定的ksoap包，置于你的classpath中。2.1 客户端源代码2.1.1 WSClientMIDlet.javapackage com.jagie.j2me.ws;import javax.microedition.midlet.*;
import javax.microedition.lcdui.*;/**
* 
Title:
* 
Description:
* 
Copyright: Copyright (c) 2004
* 
Company:
* @author not attributable
* @version 1.0
*/public class WSClientMIDlet
extends MIDlet {
static WSClientMIDlet instance;public WSClientMIDlet() {
instance = this;
}public void startApp() {
Display display=Display.getDisplay(this);
DisplayForm displayable = new DisplayForm();
display.setCurrent(displayable);}public void pauseApp() {
}public void destroyApp(boolean unconditional) {
}public static void quitApp() {
instance.destroyApp(true);
instance.notifyDestroyed();
instance = null;
}}2.1.2 DisplayForm.javapackage com.jagie.j2me.ws;import javax.microedition.lcdui.*;
/**
* 
Title:
* 
Description:
* 
Copyright: Copyright (c) 2004
* 
Company:
* @author not attributable
* @version 1.0
*/public class DisplayForm
extends Form
implements CommandListener, Runnable {
private TextField textField1;
private Thread t;public DisplayForm() {
super("字符转换webservice测试");try {
jbInit();
}
catch (Exception e) {
e.printStackTrace();
}
}private void jbInit() throws Exception {
// Set up this Displayable to listen to command events本篇文章来源于 Linux爱好网|www.linux-love.com 原文链接：http://www.linux-love.com/programme/20080405/J2MEyuWeb-Service-KSOAPkuaisushangshou_7004_2.shtmltextField1 = new TextField("", "", 15, TextField.ANY);
this.setCommandListener(this);
textField1.setLabel("待处理的字符串是：");
textField1.setConstraints(TextField.ANY);
textField1.setInitialInputMode("Tester");
setCommandListener(this);
// add the Exit command
addCommand(new Command("Exit", Command.EXIT, 1));
addCommand(new Command("Process", Command.OK, 1));
this.append(textField1);
}public void commandAction(Command command, Displayable displayable) {if (command.getCommandType() == Command.EXIT) {
WSClientMIDlet.quitApp();
}
else if (command.getCommandType() == Command.OK) {
t = new Thread(this);
t.start();
}
}public void run() {
String s1 = textField1.getString();
String s2 = new StringProcessorStub().process(s1);
StringItem resultItem = new StringItem("处理后的字符串是:", s2);
this.append(resultItem);}}2.1.3 StringProcessorStub.javapackage com.jagie.j2me.ws;import org.ksoap.*;
import org.ksoap.transport.HttpTransport;/**
* 
Title:
* 
Description:
* 
Copyright: Copyright (c) 2004
* 
Company:
* @author not attributable
* @version 1.0
*/public class StringProcessorStub {
public StringProcessorStub() {
}public String process(String name) {
String result = null;
try {SoapObject rpc = new SoapObject
("http://localhost:7001/jagiews/services/StringProcess", "process");rpc.addProperty("name", name);HttpTransport ht = new HttpTransport
("http://localhost:7001/jagiews/services/StringProcess", "");result = (String) ht.call(rpc);}
catch (Exception e) {
e.printStackTrace();
}return result;}
}测试客户端现在，试着在你的ide里运行WSClientMIDlet，如果调用成功，则出现以下画面： 总结有了ksoap，手机上调用web service就很容易了。不过要注意的是，使用网络连接这种费时操作的时候，一定要单独开线程进行，不要直接写在commandAction()方法里，否则出现画面被锁住的情况。参考资料http://ksoap.enhydra.org/softwar ... StockQuoteDemo.java本篇文章来源于 Linux爱好网|www.linux-love.com 原文链接：http://www.linux-love.com/programme/20080405/J2MEyuWeb-Service-KSOAPkuaisushangshou_7004_3.shtml
        

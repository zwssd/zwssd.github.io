---
layout: post
title:  "基于wxpython的im工具"
date:   2015-06-26 12:29:57
categories: python
tags:
---

* content
{:toc}

server#!/usr/bin/python
# encoding: utf-8
from asyncore import dispatcher
from asynchat import async_chat
import socket, asyncore
LOCAL = '127.0.0.1'
PORT = 6666 #端口
class EndSession(Exception):
<!--excerpt-->
    """
    自定义会话结束时的异常
    """
    pass
class CommandHandler:
    """
    命令处理类
    """
    def unknown(self, session, cmd):
        '响应未知命令'
        session.push('Unknown command: %s' % cmd)
    def handle(self, session, line):
        '命令处理'
        if not line.strip():
            return
        parts = line.split(' ', 1)
        cmd = parts[0]
        try:
            line = parts[1].strip()
        except IndexError:
            line = ''
        print 'cmd:'+str('do_' + cmd)
        print 'line:'+str(line)
        meth = getattr(self, 'do_' + cmd, None)
        try:
            meth(session, line)
        except TypeError:
            self.unknown(session, cmd)
class Room(CommandHandler):
    """
    包含多个用户的环境，负责基本的命令处理和广播
    """
    def __init__(self, server):
        self.server = server
        self.sessions = []
    def add(self, session):
        '一个用户进入房间'
        self.sessions.append(session)
    def remove(self, session):
        '一个用户离开房间'
        self.sessions.remove(session)
    def broadcast(self, line):
        '向所有的用户发送指定消息'
        for session in self.sessions:
            session.push(line)
    def do_logout(self, session, line):
        '退出房间'
        raise EndSession
class LoginRoom(Room):
    """
    刚登录的用户的房间
    """
    def add(self, session):
        '用户连接成功的回应'
        Room.add(self, session)
        session.push('Connect Success')
    def do_login(self, session, line):
        '登录命令处理'
        name = line.strip()
        if not name:
            session.push('UserName Empty')
        elif name in self.server.users:
            session.push('UserName Exist')
        else:
            session.name = name
            session.enter(self.server.main_room)
            print self.server.main_room
class ChatRoom(Room):
    """
    聊天用的房间
    """
    def add(self, session):
        '广播新用户进入'
        session.push('Login Success')
        self.broadcast(session.name + ' has entered the room.|')
        self.server.users[session.name] = session
        Room.add(self, session)
    def remove(self, session):
        '广播用户离开'
        Room.remove(self, session)
        self.broadcast(session.name + ' has left the room.|')
    def do_say(self, session, line):
        '客户端发送消息'
        self.broadcast(session.name + ': ' + line + '|')
    def do_look(self, session, line):
        '查看在线用户'
        session.push('Online Users:|')
        for other in self.sessions:
            session.push(other.name + '|')
class LogoutRoom(Room):
    """
    用户退出时的房间
    """
    def add(self, session):
        '从服务器中移除'
        try:
            del self.server.users[session.name]
        except KeyError:
            pass
class ChatSession(async_chat):
    """
    负责和单用户通信
    """
    def __init__(self, server, sock):
        async_chat.__init__(self, sock)
        self.server = server
        self.set_terminator('|')
        self.data = []
        self.name = None
        self.enter(LoginRoom(server))
    def enter(self, room):
        '从当前房间移除自身，然后添加到指定房间'
        try:
            cur = self.room
        except AttributeError:
            pass
        else:
            cur.remove(self)
        self.room = room
        room.add(self)
    def collect_incoming_data(self, data):
        '接受客户端的数据'
        self.data.append(data)
    def found_terminator(self):
        '当客户端的一条数据结束时的处理'
        line = ''.join(self.data)
        self.data = []
        try:
            self.room.handle(self, line)
        except EndSession:
            self.handle_close()
    def handle_close(self):
        async_chat.handle_close(self)
        self.enter(LogoutRoom(self.server))
class ChatServer(dispatcher):
    """
    聊天服务器
    """
    def __init__(self, port):
        dispatcher.__init__(self)
        self.create_socket(socket.AF_INET, socket.SOCK_STREAM)
        self.set_reuse_addr()
        self.bind((LOCAL, port))
        self.listen(15)
        print LOCAL
        print port
        print '服务器已经就绪......'
        self.users = {}
        self.main_room = ChatRoom(self)
    def handle_accept(self):
        conn, addr = self.accept()
        ChatSession(self, conn)
if __name__ == '__main__':
    s = ChatServer(PORT)
    try:
        asyncore.loop()
    except KeyboardInterrupt:
        print 
client:#!/usr/bin/python
# encoding: utf-8
import wx
import telnetlib
from time import sleep
import thread
class LoginFrame(wx.Frame):
    """
    登录窗口
    """
    def __init__(self, parent, id, title, size):
        '初始化，添加控件并绑定事件'
        wx.Frame.__init__(self, parent, id, title)
        self.SetSize(size)
        self.Center()
        self.serverAddressLabel = wx.StaticText(self, label = "Server Address", pos = (10, 50), size = (120, 25))
        self.userNameLabel = wx.StaticText(self, label = "UserName", pos = (40, 100), size = (120, 25))
        self.serverAddress = wx.TextCtrl(self,-1,'127.0.0.1:6666', pos = (120, 47), size = (150, 25))
        self.userName = wx.TextCtrl(self,-1,'abc', pos = (120, 97), size = (150, 25))
        self.loginButton = wx.Button(self, label = 'Login', pos = (80, 145), size = (130, 30))
        self.loginButton.Bind(wx.EVT_BUTTON, self.login)
        self.Show()
    def login(self, event):
        '登录处理'
        try:
            serverAddress = self.serverAddress.GetLineText(0).split(':')
            con.open(serverAddress[0], port = int(serverAddress[1]), timeout = 10)
            response = con.read_some()
            if response != 'Connect Success':
                self.showDialog('Error', 'Connect Fail!', (95, 20))
                return
            con.write('login ' + str(self.userName.GetLineText(0)) + '|')
            response = con.read_some()
            if response == 'UserName Empty':
                self.showDialog('Error', 'UserName Empty!', (135, 20))
            elif response == 'UserName Exist':
                self.showDialog('Error', 'UserName Exist!', (135, 20))
            else:
                self.Close()
                ChatFrame(None, -1, title = 'David Chat Client', size = (550, 450))
        except Exception:
            self.showDialog('Error', 'Connect Fail!', (95, 20))
    def showDialog(self, title, content, size):
        '显示错误信息对话框'
        dialog = wx.Dialog(self, title = title, size = size)
        dialog.Center()
        wx.StaticText(dialog, label = content)
        dialog.ShowModal()
class ChatFrame(wx.Frame):
    """
    聊天窗口
    """
    def __init__(self, parent, id, title, size):
        '初始化，添加控件并绑定事件'
        wx.Frame.__init__(self, parent, id, title, size)
        print 'CF'
        self.SetSize(size)
        self.Center()
        self.chatFrame = wx.TextCtrl(self, pos = (5, 5), size = (490, 310), style = wx.TE_MULTILINE | wx.TE_READONLY)
        self.message = wx.TextCtrl(self, pos = (5, 320), size = (300, 25))
        self.sendButton = wx.Button(self, label = "Send", pos = (310, 320), size = (58, 25))
        self.usersButton = wx.Button(self, label = "Users", pos = (373, 320), size = (58, 25))
        self.closeButton = wx.Button(self, label = "Close", pos = (436, 320), size = (58, 25))
        self.sendButton.Bind(wx.EVT_BUTTON, self.send)
        self.usersButton.Bind(wx.EVT_BUTTON, self.lookUsers)
        self.closeButton.Bind(wx.EVT_BUTTON, self.close)
        thread.start_new_thread(self.receive, ())
        self.Show()
    def send(self, event):
        '发送消息'
        message = str(self.message.GetLineText(0)).strip()
        if message != '':
            con.write('say ' + message + ' \n|')
            self.message.Clear()
    def lookUsers(self, event):
        '查看当前在线用户'
        con.write('look \n|')
    def close(self, event):
        '关闭窗口'
        con.write('logout \n|')
        con.close()
        self.Close()
    def receive(self):
        '接受服务器的消息'
        while True:
            sleep(0.6)
            result = con.read_very_eager()
            if result != '':
                self.chatFrame.AppendText(result)
'程序运行'
if __name__ == '__main__':
    app = wx.App()
    con = telnetlib.Telnet()
    LoginFrame(None, -1, title = "Login", size = (280, 200))
    app.MainLoop() 
        

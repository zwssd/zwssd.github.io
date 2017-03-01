---
layout: post
title:  "python制作IRC聊天机器人AsimpleIRCBOTwritteninPython2009-12-15"
date:   2011-03-10 09:45:24
categories: python
tags:
---

* content
{:toc}

已经修改好的可用版（还行简单）：   #!/usr/bin/env python  import socket  ## IRC Details, change these irc_network = 'irc.freenode.net' bot_owner = 'zwssd_bot' irc_channel = '#ubuntu-cn' #irc_channel = '#zwssdbot-test' irc_port = 7000  irc_sock = socket.socket( socket.AF_INET, socket.SOCK_STREAM ) irc_sock.connect( ( irc_network, irc_port ) )  irc_sock.send( 'NICK zwssdbot\r\n' ) irc_sock.send( 'User Test Test Test :Python2\r\n' ) irc_sock.send( 'PRIVMSG :MSG NickServ IDENTIFY 80\r\n' ) irc_sock.send( 'JOIN ' + irc_channel +'\r\n' )  i=0 while True:     irc_data = str( irc_sock.recv( 1024 ) )     print i     i+=1     if irc_data=='':         break     print irc_data     """if irc_data.find( '!hi' ) and irc_data[irc_data.find( ':' ) + 1:irc_data.find( '!' )] == bot_owner:         irc_sock.send( ':zwssdbot!n=Test@210.51.173.157 PRIVMSG ' + irc_channel + ' hi\r\n' )"""     if (irc_data.find( 'bot say!' )!=-1 and irc_data.find( 'zwssd' )!=-1):         irc_sock.send( ':zwssdbot!n=Test@210.51.173.157 PRIVMSG ' + irc_channel + ' :hi evey body! my name is zwssdbot! zwssd is my father!\r\n' )      This article shows you how to make a simple IRC bot in python.
 First question: why the hell not eggdrop? Answer: I don't know TCL,  and I like to customize my scripts; plus it's a heavy program, and I  hardly need 1% of the features it provides. I have been using python for  a few months now and absoulutely love it. Creating bots in python is  quite a simple job really. So lets start making it. 
 
 Importing the libraries:  import sys 
 import socket 
 import string 
 import os     #not necassary but later on I am going to use a few features from this   With that done, now we need to give a configuration:   HOST='mesa.az.us.undernet.org'     #The server we want to connect to 
 PORT=6667                          #The connection port which is usually 6667 
 NICK='pybotv000'                   #The bot's nickname 
 IDENT='pybot'                       
 REALNAME='s1ash' 
 OWNER='ne0n-'                      #The bot owner's nick 
 CHANNELINIT='#test198'             #The default channel for the bot 
 readbuffer=''                      #Here we store all the messages from server   Warning:Keep the nickname quite unique because then you'll have to use a method to change the nick if it is already taken.   Connecting to the server:   s=socket.socket( )                                       #Create the socket 
 s.connect((HOST, PORT))                                  #Connect to server 
 s.send('NICK '+NICK+'n')                                #Send the nick to server 
 s.send('USER '+IDENT+' '+HOST+' bla :'+REALNAME+'n')    #Identify to server   The main part of the bot:   Now, we are sitting on the server but not doing anything. To  do stuff we setup an infinite loop that listens to the messages and  handles them appropriately.   while 1: 
 
         line=s.recv(500)                                            #recieve server messages 
     print line                                                  #server message is output 
     if line.find('Welcome to the UnderNet IRC Network')!=-1:    #This is Crap(I wasn't sure about it but it works) 
         s.send('JOIN '+CHANNELINIT+'n')                    #Join a channel 
     if line.find('PRIVMSG')!=-1:                                #Call a parsing function 
         parsemsg(line) 
         line=line.rstrip()                                          #remove trailing 'rn' 
         line=line.split() 
         if(line[0]=='PING'):                                        #If server pings then pong 
             s.send('PONG '+line[1]+'n')   We recieve the server input in a variable line; if you want  to see the servers messages, use print line. Once we connect to the  server, we join a channel. Now whenever we recieve any PRIVMSG, we call a  function which does the appropriate action. The next few lines are used  to reply to a servers PING. Until this point, the bot just sits idle in  a channel.   To make it active we use the parsemsg function.   The parsemsg function:   def parsemsg(msg): 
     complete=msg[1:].split(':',1)                       #Parse the message into useful data 
     info=complete[0].split(' ') 
     msgpart=complete[1] 
     sender=info[0].split('!') 
     if msgpart[0]=='`' and sender[0]==OWNER:        #Treat all messages starting with '`' as command 
         cmd=msgpart[1:].split(' ') 
         if cmd[0]=='op': 
             s.send('MODE '+info[2]+' +o '+cmd[1]+'n') 
         if cmd[0]=='deop': 
             s.send('MODE '+info[2]+' -o '+cmd[1]+'n') 
         if cmd[0]=='voice': 
             s.send('MODE '+info[2]+' +v '+cmd[1]+'n') 
         if cmd[0]=='devoice': 
             s.send('MODE '+info[2]+' -v '+cmd[1]+'n') 
         if cmd[0]=='sys': 
             syscmd(msgpart[1:],info[2]) 
          
     if msgpart[0]=='-' and sender[0]==OWNER :  #Treat msgs with - as explicit command to send to server 
         cmd=msgpart[1:] 
         s.send(cmd+'n') 
         print 'cmd='+cmd   This is the most interesting part, PRIVMSG are usually of this form:   :nick!username@host PRIVMSG channel/nick :Message   First, we seperate out all the data and nick part. The actual message is stored in variable msgpart.   If a message starts with ` it is trated as a user commands these can be used as public op/deop/ voice/devoice   commands. One spesial command is sysproc which is a special command that executes a command on the   bot's system and also displays the output(That's where os comes in).If we use a '-' we instruct the bot to send   RAW data to server, this is quite a handy feature using this you can almost use the bot as an IRC client   well not really.   The syscmd function:   def syscmd(commandline,channel): 
     cmd=commandline.replace('sys ','') 
     cmd=cmd.rstrip() 
     os.system(cmd+' >temp.txt') 
     a=open('temp.txt') 
     ot=a.read() 
     ot.replace('n','|') 
     a.close() 
     s.send('PRIVMSG '+channel+' :'+ot+'n') 
     return 0   This piece of code takes the command and executes it printing the output to ot.txt. Then ot.txt is   read and displayed to the given channel.Multiline output is shown by using '|'. 
  You can easily add features to this bot and also CTCP replies but that's upto you.   I know the code is very ugly. The complete listing is on my osi drive : 
 http://www.osix.net:80/modules/folder/index.php?tid=10807&action=vf
        

---
layout: post
title:  "编程游戏codecombat从GitHub上下载后该如何安装"
date:   2016-03-20 12:13:19
categories: 编程设计
tags:
---

* content
{:toc}

我已经搭建了一个
测试地址：http://www.icodegame.com:3000
采用官方9月7日最新的数据库文件，并且已经修复所有装备图片、怪物声音等。
欢迎大家测试！
--------------------------------
这是我建的QQ群：192252941
---------------这是一个Linux自动安装脚本------------------
#!/bin/bash
sleep 5s
sudo apt-get update
sleep 5s
sudo apt-get -y install make build-essential curl git zlib1g-dev python2.7 libkrb5-dev
sleep 5s
sudo mkdir -p coco
cd coco
sudo git clone https://github.com/codecombat/codecombat.git
sleep 5s
sudo wget http://nodejs.org/dist/v5.9.0/node-v5.9.0.tar.gz
sudo tar xfz node-v5.9.0.tar.gz
cd node-v5.9.0
sudo ./configure
sudo make
sudo make install
cd ~/coco
sudo curl -L https://npmjs.org/install.sh | sudo sh
node -v
sleep 5s
npm -v
sleep 5s
cd ~/coco/codecombat
sudo npm config set registry https://registry.cnpmjs.org
sudo npm config set python python2.7
sudo npm install -g bower --allow-root
sudo npm install -g brunch
sudo npm install -g node-gyp
sudo npm install --phantomjs_cdnurl=http://cnpmjs.org/downloads
sleep 5s
sudo bower --allow-root install
sudo bower --allow-root update
sudo brunch build --env fast
sleep 5s
cd ~/coco && mkdir -p mongodl
cd mongodl
sudo curl -O https://fastdl.mongodb.org/linux/mongodb-linux-x86_64-ubuntu1404-3.2.4.tgz
sudo tar xfz mongodb-linux-x86_64-ubuntu1404-3.2.4.tgz
sudo cp mongodb-linux-x86_64-ubuntu1404-3.2.4/bin/* /usr/local/bin
sleep 5s
cd ~/coco && mkdir -p db
cd db
sudo wget http://analytics.codecombat.com:8080/dump.tar.gz
sudo tar xzvf dump.tar.gz
sleep 5s
cd ~/coco && mkdir -p log
sudo ./codecombat/bin/coco-mongodb >~/coco/log/mongodb.log 2>&1 &
echo Wait 10 seconds
sleep 10s
cd db && sudo mongorestore --drop dump
sleep 5s
cd ~/coco
cat <<- EOF > run-coco.sh
#!/bin/bash
echo ----------Run brunch and nodemon
cd ~/coco/codecombat
nohup sudo npm run dev >~/coco/log/brunch_nodemon.log 2>&1 &
echo ----------brunch and nodemon ok!
EOF
chmod 777 run-coco.sh
sleep 5s
cd ~/coco
cat <<- EOF > run-mongodb.sh
#!/bin/bash
echo ----------Run mongodb
nohup sudo ~/coco/codecombat/bin/coco-mongodb >~/coco/log/mongodb.log 2>&1 &
echo ----------mongodb ok
EOF
chmod 777 run-mongodb.sh
cat <<- EOF > stop-mongodb.sh
#!/bin/bash
echo ----------Stop mongodb
sudo mongo admin --port 27017 --eval "db.shutdownServer()"
echo ----------Stop Mongodb ok!
EOF
chmod 777 stop-mongodb.sh
echo -------------------------------------------------------------------------
echo ----------ok!
echo -------------------------------------------------------------------------
        

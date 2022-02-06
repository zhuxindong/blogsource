---
title: ubuntu安装redis并设置密码，远程访问等
date: 2020-05-05 15:20:53
tags: Redis
---





### 1.安装Redis服务器端

```
sudo apt-get install redis-server
```



开启服务：

```shell
sudo service redisd start
```

关闭服务：

```shell
sudo service redisd stop
```



通过启动命令检查Redis服务器状态

```shell
sudo /etc/init.d/redis-server status
```



进入redis，测试是否安装成功

redis-cli



查看所有的key列表

keys *



设置、获取、删除参数

set name ‘val’

get name

del name



### 2.设置用户名密码

编辑redis.conf   设置访问密码为redis

```shell
sudo vi /etc/redis/redis.conf
```

#取消注释requirepass

```shell
requirepass redis
```



登陆Redis服务器，输入密码

```shell
redis-cli -a redis
```



### 3.Redis服务器不允许远程访问，注释bind 重启服务设置允许远程访问

```shell
sudo vi /etc/redis/redis.conf
```

```shell
#bind 127.0.0.1
```



```shell
sudo /etc/init.d/redis-server restart
```

 **重启**



### 4.检查redis服务占用

```shell
ps -ef | grep redis （注释前）
redis     2644     1  0 07:11 ?        00:00:47 /usr/bin/redis-server 127.0.0.1:6379

ps -ef | grep redis （注释后）
redis    13536     1  0 14:33 ?        00:00:00 /usr/bin/redis-server *:6379
```



远程测试登陆其他服务器

~ redis-cli -a redisredis -h 192.168.1.10

redis192.168.1.10:6379>

远程访问正常。

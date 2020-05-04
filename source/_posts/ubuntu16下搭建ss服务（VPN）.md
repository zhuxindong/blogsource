---
title: ubuntu16下搭建ss服务（VPN）
date: 2020-05-04 10:54:26
tags: SS
---

# ubuntu16下搭建ss服务（VPN）

##基本设置

###0、更新软件源

```bash
sudo apt-get update
```

###1、安装pip3

```bash
sudo apt-get install python3-pip
```

###2、先用python3安装shadowsocks：

```bash
sudo pip3 install shadowsocks
```

###3、编辑配置文件

```bash
sudo vim /etc/shadowsocks.json
```

配置文件按照以下设置：

```json
{
    "server":"你的主机的IP地址。 好像0.0.0.0 也可以",
    "server_port": 9999,
    "password":"password",
    "timeout":600,
    "method":"aes-256-cfb",
    "port_password":
	{
		"5200":"password",
		"5201":"password",
		"5202":"password",
		"5203":"password",
		"5204":"password",
		"5205":"password",
		"1314":"password"
	}
}
```

###4、启动服务（以后台方式启动）

```bash
sudo ssserver -c /etc/shadowsocks.json -d start
```

##高级进阶设置

**通过ss-bash流量管理脚本来管理各个端口的流量使用情况**

###0、如果shadowsocks正在运行，请先停止服务

```bash
sudo ssserver -c /etc/shadowsocks.json -d stop
```

 ###1、安装必要的软件

```bash
sudo apt-get install bc git
```

###2、下载ssbash流量管理脚本

```bash
sudo git clone https://github.com/hellofwy/ss-bash.git
```

###3、配置相关规则

```bash
#进入ssbash的目录
cd ss-bash/

#首次运行时，先新建用户
#例如新用户端口为8388，密码为passwd，流量限制为10GB，执行：
sudo ./ssadmin.sh add 8388 passwd 10G

#如果想继续添加端口，按照上面的规则来就行了
```

> ssadmin.sh用法说明 和 ss-bash目录下的相关文件说明：
>
> 1. **ssadmin.sh用法说明**
>
>     | 用法： |
>     |   | 显示版本： |
>     |   | ssadmin.sh -v|v|version |
>     |   | 显示帮助： |
>     |   | ssadmin.sh \[-h|h|help\] |
>     |   | 启动ss: |
>     |   | ssadmin.sh start |
>     |   | 停止ss： |
>     |   | ssadmin.sh stop |
>     |   | 查看ss状态： |
>     |   | ssadmin.sh status |
>     |   | 重启ss： |
>     |   | ssadmin.sh restart |
>     |   | 软重启ss： |
>     |   | ssadmin.sh soft_restart |
>     |   | 在不影响现有连接的情况下重启ss服务。用于ss服务参数修改， |
>     |   | 和手动直接修改配置文件后，重启ss服务。 |
>     |   | 添加用户： |
>     |   | ssadmin.sh add port passwd limit |
>     |   | port：端口号, 0<port<=65535 |
>     |   | passwd：密码, 不能有空格，引号等字符 |
>     |   | limit：流量限制，可以用K/M/G/T、KB/MB/GB/TB等（不区 |
>     |   | 分大小写）。支持小数。比如10.5G、10.5GB等。 |
>     |   | 1KB=1024 bytes，以此类推。 |
>     |   | 示例： ssadmin.sh add 3333 abcde 10.5G |
>     |   | 显示用户流量信息： |
>     |   | ssadmin.sh show port |
>     |   | 显示所有用户流量信息： |
>     |   | ssadmin.sh show |
>     |   | 显示用户密码信息： |
>     |   | ssadmin.sh showpw port |
>     |   | 显示所有用户密码信息： |
>     |   | ssadmin.sh showpw |
>     |   | 删除用户： |
>     |   | ssadmin.sh del port |
>     |   | 修改用户： |
>     |   | ssadmin.sh change port passwd limit |
>     |   | 修改用户密码： |
>     |   | ssadmin.sh cpw port passwd |
>     |   | 修改用户流量限制： |
>     |   | ssadmin.sh clim port limit |
>     |   | 修改所有用户流量限制： |
>     |   | ssadmin.sh change\_all\_limit limit |
>     |   | 用户流量使用量置零： |
>     |   | ssadmin.sh rused limit |
>     |   | 所有用户流量使用量置零： |
>     |   | ssadmin.sh reset\_all\_used |
>     |   | 用户流量限制置零： |
>     |   | ssadmin.sh rlim port |
>     |   | 全部用户流量限制置零： |
>     |   | ssadmin.sh reset\_all\_limit |
>     |   | 显示已添加的iptables规则： |
>     |   | ssadmin.sh lrules |
>
>     \-\-\-\-\-\-\-\-  
>     \-\-\-\-\-\-\-\-
> 2. **ss-bash目录下的相关文件说明**
>
> -   ssadmin.sh - 管理程序，所有命令通过该程序执行
>
> -   sscounter.sh - 流量统计程序。由ssadmin.sh自动调用执行，注意：不要手动运行该程序
>
> -   sslib.sh - 包含一些参数配置和流量统计函数。由ssadmin.sh自动调用执行，注意：不要手动运行该程序
>
> -   ssmlt.template - ssserver的配置文件
>
>
> 程序运行后，会产生以下文件：
>
> -   ssmlt.json - 根据用户列表和ssmlt.template生成的ssserver实际使用的配置文件
>
> -   ssusers - 用户列表，包括端口、密码、流量限制参数。ssadmin.sh showpw 命令，显示该文件内容。
>
> -   sstraffic - 用户流量使用情况，包括流量限制，已用流量，剩余流量等。ssadmin.sh show 命令，显示该文件内容。
>
> -   traffic.log - 用户流量记录，供程序内部使用。
>
> -   其它文件 \- .tmp、.lock、.pid等文件、文件夹tmp及其中文件为程序内部使用文件，请不要手动删除。
>

 ###4、启动ssserver

```bash
sudo ./ssadmin.sh start
```

###5、设置每月初流量自动清零

```
#设置ubuntu定时任务
sudo vim /etc/crontab

#添加如下任务：
0  0    1 * *   root    echo date MONTHLY_RESET >> ss_log && /root/ss-bash/ssadmin.sh reset_all_used

```

ps：注意修改目录，根据自己的实际情况而定
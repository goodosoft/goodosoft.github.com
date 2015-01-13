---
layout: post
title: "初识saltstack"
description: "saltstack入门学习"
category: [learn]
tags: [saltstack]
---
{% include JB/setup %}

### 配置文件位置：  
> The configuration files will be installed to /etc/salt and are named after the respective components,/etc/salt/master and /etc/salt/minion.

### 检查有多少客户端需要批准  
> salt-key -L

### 管理所有客户端（批量添加）
> salt-key -A

### 测试是否添加成功
> salt '*' test.ping

### 配置master过程
在172.30.100.11安装了master
修改配置文件，设置master的根目录：
mkdir -p /srv/salt/

### 配置client过程

#### 在以下机器安装minion：
> 172.30.100.12  
> 172.30.100.13  
> 192.168.1.9（redhat4.4装不上）  
> 192.168.222.131  
> 192.168.1.175  
> 192.168.223.173  
> 192.168.222.161  
> 192.168.1.125  
> 192.168.223.65  

#### 修改客户端> 配置：
> sed -i "/#master: salt/a master: 172.30.100.11" /etc/salt/minion  
> sed -i "/#id:/a id: $(ifconfig |grep 'inet addr'|head -1|sed 's/^.*addr://g'|sed 's/Bcast.*$//g')" /etc/salt/minion  
> service salt-minion restart  


### 启动服务：
> 服务端启动方式：service salt-master start  
> 客户端启动方式：service salt-minion start  
> 日志查看路径：（有问题可查日志获取出错信息）  
> 服务端：/var/log/salt/master  
> 客户端：/var/log/salt/minion  

### 查看一下默认的采集信息：
> salt '172.30.100.12' grains.items

### saltstack实战  

#### 例子1:   
> salt '*' cmd.run "df -h"  
> 查看所有机器的磁盘空间  
 
#### 例子2:  
- 在所有客户端机器上安装zabbix agent;  
- 实现agent自动安装;  
- agent配置文件根据机器的信息自动配置;   
- 在zabbix server段创建自动发现规则批量添加主机;  

### 安装过程:
> QUICK INSTALLMany popular distributions will be able to install the salt minion by executing the bootstrap script:  
> wget -O - http://bootstrap.saltstack.org | sudo sh  
>   
> Run the following script to install just the Salt Master:  
> curl -L http://bootstrap.saltstack.org | sudo sh -s -- -M -N  
>   
>   
> The script should also make it simple to install a salt master, if desired.  
> ENABLING EPEL ON RHELIf EPEL is not enabled on your system, you can use the following commands to enable it.  
> For RHEL 5:  
> rpm -Uvh http://mirror.pnl.gov/epel/5/i386/epel-release-5-4.noarch.rpm  
> 
> For RHEL 6:  
> rpm -Uvh http://ftp.linux.ncsu.edu/pub/epel/6/i386/epel-release-6-8.noarch.rpm  
>   
> Stable ReleaseSalt is packaged separately for the minion and the master. It is necessary only to install the appropriate package for the role the machine will play. Typically, there will be one master and multiple minions.  
> On the salt-master, run this:  
> yum install salt-master  
>   
> On each salt-minion, run this:  
> yum install salt-minion  
>   
>   
> POST-INSTALLATION TASKSMaster  
> To have the Master start automatically at boot time:  
> chkconfig salt-master on  
>   
> To start the Master:  
> service salt-master start  
>   
> Minion  
> To have the Minion start automatically at boot time:  
> chkconfig salt-minion on  
>   
> To start the Minion:  
> service salt-minion start  

### 参考资料：
- [深入SaltStack](http://www.saltstack.cn/projects/cssug-kb/wiki/dive-into-saltstack)
- [运维自动化之salt学习笔记](http://blog.halfss.com/blog/2013/05/22/yun-wei-zi-dong-hua-zhi-saltxue-xi-bi-ji/)




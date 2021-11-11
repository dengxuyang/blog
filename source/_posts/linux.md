---
title: linux常用命令
date: 2021-03-05 13:43:03
tags:
categories:
- linux
---
整理学习过程中linux的知识，因为我博客就架在linux服务器，所以整理一下常用命令。防止之后忘记！
## 1.查看nginx运行状态
```
sudo systemctl status nginx
```
## 2.启动ngnix
```
sudo systemctl start nginx
```
## 3.重启与强制重启nginx
* 对于主要配置更改，您可以强制完全重启Nginx。这将强制关闭整个服务和子流程，然后重新启动整个程序包。
```
//重启
sudo systemctl reload nginx
//强制重启 
sudo systemctl restart nginx
```
## 4.nginx退出
```
sudo nginx -s quit
```
## 5.linux开放指定端口命令
* 查看端口是否打开
```
telnet ip 端口
```
* 开启防火墙   
```
    systemctl start firewalld
```

* 开放指定端口  
```
 firewall-cmd --zone=public --add-port=1935/tcp --permanent  
```
     
 * 命令含义：  
--zone #作用域  
--add-port=1935/tcp  #添加端口，格式为：端口/通讯协议  
--permanent  #永久生效，没有此参数重启后失效  

* 重启防火墙  
```
firewall-cmd --reload  
```

4、查看端口号  
```
netstat -ntlp   //查看当前所有tcp端口·  

netstat -ntulp |grep 1935   //查看所有1935端口使用情况·
```
## 6.vim的基本使用命令
使用vim 文件名进入文件 i进入插入模式 
```
:w 保存文件但不退出vi
:w file 将修改另外保存到file中，不退出vi
:w! 强制保存，不推出vi
:wq 保存文件并退出vi
:wq! 强制保存文件，并退出vi
:q 不保存文件，退出vi
:q! 不保存文件，强制退出vi
:e! 放弃所有修改，从上次保存文件开始再编辑
```
## 7.查看磁盘占用
```
 df -h
```
## 虚拟机开启 ens33 查看ip
```
nmcli c up ens33
ifconfig
```
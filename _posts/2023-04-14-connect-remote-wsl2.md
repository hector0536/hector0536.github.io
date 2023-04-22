---
title: 远程连接wsl2
key: 2023-04-03-connect-remote-wsl2
tags:
- 技术 
- ssh
- 远程
- wsl
---

使用ssh远程连接win10或者win11上的wsl2，注意此方法针对wsl2有效。具体步骤如下：

<!--more-->

## 1. wsl2中安装ssh

```shell
sudo apt install ssh
```

## 2. 配置wsl2开机启动ssh

$\quad$ 2.1 win+R进入运行窗口并输入shell:startup，回车

$\quad$ 2.2 在启动目录中创建wsl_ubuntu.vbs并编辑内容：

```
Set ws = WScript.CreateObject("WScript.Shell")        
ws.run "wsl -d Ubuntu -u root /etc/init.d/ssh start", vbhide
```

## 3. 配置windows防火墙，开放22端口

$\quad$ 3.1	控制面板->windows defender防火墙->高级设置->入站规则->新建规则

$\quad$ 3.2	添加22端口、TCP协议、允许接入

![Image](https://raw.githubusercontent.com/hector0536/hector0536.github.io/main/assets/images/connect-remote-wsl2.png)

##  4.	windows配置端口转发，将外部网络的22端口转到内部网络ipv6的22端口

$\quad$ 4.1	以管理员模式打开powershell,添加端口转发

```
netsh interface portproxy add v4tov6 listenaddress=0.0.0.0 listenport=22 connectaddress=::1 connectport=22
```

$\quad$ 4.2 其他管理转发规则的命令：

```
netsh interface portproxy show all
netsh interface portproxy delete v4tov6 listenaddress=0.0.0.0 listenport=22
```

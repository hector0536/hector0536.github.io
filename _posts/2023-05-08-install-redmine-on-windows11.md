---
title: 在window11上部署redmine
key: 2023-05-08-install-redmine-on-windows11
tags:
- 技术 
- redmine
---

使用bitnami的windows安装包一键部署redmine，以及遇到的问题。

<!--more-->

## 一键部署

### 1. 软件包下载

bitnami一键安装包下载地址：https://downloads.bitnami.com/files/stacks/redmine/5.0.3-0/bitnami-redmine-5.0.3-0-windows-x64-installer.exe

### 2. 安装

#### 2.1 选择语言

![Image](https://raw.githubusercontent.com/hector0536/hector0536.github.io/main/assets/images/install-redmine-on-windows11-1.png)

#### 2.2 创建管理员账户

![Image](https://raw.githubusercontent.com/hector0536/hector0536.github.io/main/assets/images/install-redmine-on-windows11-2.png)

这里的密码并不是邮箱的密码，是需要设置一个管理员登录redmine的密码

#### 2.3 配置邮箱用于发送邮件

![Image](https://raw.githubusercontent.com/hector0536/hector0536.github.io/main/assets/images/install-redmine-on-windows11-3.png)

这里需要勾选邮箱配置，如果不是gmail邮箱就选择自定义，然后点击前进。

![Image](https://raw.githubusercontent.com/hector0536/hector0536.github.io/main/assets/images/install-redmine-on-windows11-4.png)

这里用户名是邮箱的完整名称。  
密码是邮箱的登录密码，有一些邮箱如果开启了安全认证，会单独提供一个客户端专用密码，如果是这种情况需要填写邮箱客户端专用密码。  
SMTP主机以及端口等信息需要根据不同的邮箱去设置。

注意：在配置前需要先将邮箱的SMTP服务打开。

#### 2.4 安装完成

安装完成后，apache服务默认是监听在80端口，我们可以使用浏览器进入redmine的登录页面。同时软件包会安装一个管理工具，从开始页面打开，可以进行服务的停止重启等管理，如下图所示。

![Image](https://raw.githubusercontent.com/hector0536/hector0536.github.io/main/assets/images/install-redmine-on-windows11-5.png)

登录成功后，通过“管理-->配置-->邮件通知”来配置邮件发件人地址，然后进行保存。

![Image](https://raw.githubusercontent.com/hector0536/hector0536.github.io/main/assets/images/install-redmine-on-windows11-6.png)

保存成功后点击发送测试邮件，如下图所示。如果安装时填写的邮箱收到了测试邮件说明配置成功，我的是第一次没有成功，后面有解决办法。

![Image](https://raw.githubusercontent.com/hector0536/hector0536.github.io/main/assets/images/install-redmine-on-windows11-7.png)

![Image](https://raw.githubusercontent.com/hector0536/hector0536.github.io/main/assets/images/install-redmine-on-windows11-8.png)



## 遇到的问题

### 发送测试邮件失败

问题描述：

点击发送测试邮件之后，页面返回错误：

Proxy Error  
The proxy server received an invalid response from an upstream server.
The proxy server could not handle the request POST

Reason: Error reading from remote server

解决办法：

修改配置文件：F:\Bitnami\redmine-5.0.3-0\apps\redmine\htdocs\config\configuration.yml

```
# default configuration options for all environments
default:
  # Outgoing emails configuration
  # See the examples below and the Rails guide for more configuration options:
  # http://guides.rubyonrails.org/action_mailer_basics.html#action-mailer-configuration
  email_delivery:
    delivery_method: :smtp
    smtp_settings:
      tls: true   #原来是false
      address: "smtp.exmail.qq.com"  #原来没有引号
      port: 465
      domain: "example.net"   #原来没有引号
      authentication: :login
      enable_starttls_auto: true
      user_name: "lihan@sdcit.edu.cn"  #原来没有引号
      password: "********"    #原来没有引号
```

修改保存之后通过管理工具重启redmine的两个服务。



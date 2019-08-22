---

title: SSH远端服务器使用GUI

date: 2019-08-22 19:44:47
tags: [How]
categories: [Tools]

---

<!-- vim-markdown-toc GFM -->

* [SSH设置](#ssh设置)
    * [SSH多个连接共享](#ssh多个连接共享)
    * [SSH远程GUI](#ssh远程gui)
    * [本地操作远程文件](#本地操作远程文件)
    * [主机别名](#主机别名)
    * [xauth警告](#xauth警告)
* [完整的配置](#完整的配置)
* [安装中文字体](#安装中文字体)
* [遇到的问题](#遇到的问题)

<!-- vim-markdown-toc -->

<!-- more -->

# SSH设置

modify ~/.ssh/config

## SSH多个连接共享

ControlPersist 可以设置长连接的时间,即使所有的窗口退出, 连接仍保持.

```
ControlPersist 4h
ControlMaster auto
ControlPath /tmp/ssh_mux_%h_%p_%r
```

## SSH远程GUI

```
ForwardX11 yes
X11Forwarding yes
```

## 本地操作远程文件

```shell
sshfs user@ip:/path/to/remote_dir local_dir
```

## 主机别名

User指定默认用那个用户名进行操作

```
Host alias_name
    HostName        real_host
    User            dc2-user
```

## xauth警告

- 生成文件, 如果有备份删除: `touch ~/.Xauthority`
- X11 display over ssh: `xauth generate :0 . trusted`
- (不需要)生成本地主机的Key:  `xauth add ${HOST}:0 . $(xxd -l 16 -p /dev/urandom)`

# 完整的配置

```
KexAlgorithms=+diffie-hellman-group1-sha1
ControlMaster auto
ControlPath /tmp/ssh_mux_%h_%p_%r
ControlPersist 6h
GSSAPIAuthentication no

XAuthLocation /usr/bin/xauth

Host *
    ForwardAgent yes
    ForwardX11 yes
    ForwardX11Trusted yes

Host cauchy
    HostName        xxx.xxx.xxx.xxx
    User            xxx-user

# mount: sshfs cauchy:work/code code

Host github.com
    HostName                   github.com
    PreferredAuthentications   publickey
    User                       git
    IdentityFile               /home/lidong/.ssh/id_rsa_qrsblog
```

# 安装中文字体

```shell
sudo apt install -y --no-install-recommends fonts-wqy-microhei
sudo apt install -y --no-install-recommends ttf-wqy-zenhei
```


# 遇到的问题

- [Ubuntu16.04系统下汉字显示为方框解决办法][1]
- [Ubuntu添加中文字体][2]
- [高效使用SSH的16个技巧][3]
- [xauth not creating .Xauthority file][4]


[1]: https://www.cnblogs.com/zlslch/p/6971112.html
[2]: https://www.cnblogs.com/Jimc/p/10302267.html
[3]: http://www.linuxboy.net/yunweigl/131467.html
[4]: https://superuser.com/questions/806637/xauth-not-creating-xauthority-file

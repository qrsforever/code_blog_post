---

title: shadowsocks配置

date: 2019-12-13 12:05
tags: [How, System, Ubuntu]
categories: [Story]

---


<!-- more -->

## 支持chacha20加密

    sudo apt install libsodium -y


## 安装shadowsocks客户端

    sudo pip3 install https://github.com/shadowsocks/shadowsocks/archive/master.zip --isolated -v -U

    or

    sudo pip3 install shadowsocks


**注意, slocal,ssserver 两个命令的安装路径(~/.local/bin by pip.conf configure), 记得设置PATH**

**或者pip install指定--isolated**

## 配置

```
{
  "server":"x.x.x.x",           # ss 服务器 地址
  "server_port":14131,          # ss 服务器端口
  "local_address": "127.0.0.1", # 本地ip
  "local_port":1080,            # 本地端口
  "password":"password",        # 连接 ss 密码
  "timeout":300,                # 等待超时
  "method":"aes-256-cfb",       # 加密方式
  "fast_open": false,           # fast_open：true或false。开启fast_open以降低延迟，但要求Linux内核在3.7+。
  "workers": 1                  # 工作线程数
}
```


## 启动服务

> vim /etc/systemd/system/shadowsocks.service

```
[Unit]
Description=Shadowsocks

[Service]
TimeoutStartSec=0
ExecStart=/usr/local/bin/sslocal -c /etc/shadowsocks/shadowsocks.json

[Install]
WantedBy=multi-user.target
```

> systemctl

```
sudo systemctl enable shadowsocks.service
sudo systemctl start shadowsocks.service
sudo systemctl status shadowsocks.service
```

## 结果

```
● shadowsocks.service - Shadowsocks
   Loaded: loaded (/etc/systemd/system/shadowsocks.service; enabled; vendor preset: enabled)
   Active: active (running) since Fri 2019-12-13 12:51:03 CST; 4s ago
 Main PID: 12018 (sslocal)
    Tasks: 1 (limit: 4915)
   CGroup: /system.slice/shadowsocks.service
           └─12018 /usr/bin/python3 /usr/local/bin/sslocal -c /etc/shadowsocks/shadowsocks.json

Dec 13 12:51:03 omega systemd[1]: Started Shadowsocks.
Dec 13 12:51:03 omega sslocal[12018]: INFO: loading config from /etc/shadowsocks/shadowsocks.json
Dec 13 12:51:03 omega sslocal[12018]: 2019-12-13 12:51:03 INFO     loading libsodium from libsodium.so.23
Dec 13 12:51:03 omega sslocal[12018]: 2019-12-13 12:51:03 INFO     starting local at 127.0.0.1:1080
```

## SS中继

- sudo apt install privoxy

```json
forward-socks5   /               127.0.0.1:1080 .
listen-address  0.0.0.0:8118
# local network do not use proxy
forward         192.168.*.*/     .
forward            10.*.*.*/     .
forward           127.*.*.*/     .
****
```

sudo /etc/init.d/privoxy restart

## 参考

https://dylanyang.top/post/2019/05/15/centos7%E5%AE%89%E8%A3%85%E9%85%8D%E7%BD%AEshadowsocks%E5%AE%A2%E6%88%B7%E7%AB%AF/

http://woshishagua.com/?p=89

---

title: 彻底删除Ubuntu snap

date: 2019-10-10 22:20:06
tags: [System]
categories: [Story]

---

[RAWCODE](https://raw.githubusercontent.com/qrsforever/code_blog_post/master/Story/System/remove_snap.md)

<!-- more -->

# 步骤

1. 从PATH中删除当前路径".", 否则删除snapd出错

    export PATH=/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin

2. 查看已经snap安装的软件, 并卸载

    snap list; sudo snap remove xxx

3. 删除cache, 否则删除snapd可能会出错

    sudo rm -rf /var/cache/snapd

4. 删除snapd

    sudo apt purge snapd

5. 删除安装包

    rm -rf ~/snap

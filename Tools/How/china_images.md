---

title: 国内镜像

date: 2019-07-18 10:31:07
tags: [How]
categories: [Tools]

---

<!-- vim-markdown-toc GFM -->

* [apt](#apt)
* [mvn](#mvn)
* [pip](#pip)
* [npm](#npm)
* [gradle](#gradle)

<!-- vim-markdown-toc -->

<!-- more -->

# apt

modify `/etc/apt/sources.list`

```
deb http://mirrors.aliyun.com/ubuntu/ vivid main restricted universe multiverse
deb http://mirrors.aliyun.com/ubuntu/ vivid-security main restricted universe multiverse
deb http://mirrors.aliyun.com/ubuntu/ vivid-updates main restricted universe multiverse
deb http://mirrors.aliyun.com/ubuntu/ vivid-proposed main restricted universe multiverse
deb http://mirrors.aliyun.com/ubuntu/ vivid-backports main restricted universe multiverse
deb-src http://mirrors.aliyun.com/ubuntu/ vivid main restricted universe multiverse
deb-src http://mirrors.aliyun.com/ubuntu/ vivid-security main restricted universe multiverse
deb-src http://mirrors.aliyun.com/ubuntu/ vivid-updates main restricted universe multiverse
deb-src http://mirrors.aliyun.com/ubuntu/ vivid-proposed main restricted universe multiverse
deb-src http://mirrors.aliyun.com/ubuntu/ vivid-backports main restricted universe multiverse
```

# mvn

set your project's settings.xml

```xml
<?xml version="1.0" encoding="UTF-8"?>
<settings>
    <localRepository>/home/lidong/.m2/repository</localRepository>
    <mirrors>
        <!-- 使用国内仓库 -->
        <mirror>
            <id>alimaven</id>
            <name>aliyun maven</name>
            <url>http://maven.aliyun.com/nexus/content/groups/public/</url>
            <mirrorOf>central</mirrorOf>
        </mirror>
    </mirrors>
    <profiles>
        <profile>
            <id>nexus</id>
            <repositories>
                <repository>
                    <id>nexus</id>
                    <name>local private nexus</name>
                    <url>http://maven.oschina.net/content/groups/public/</url>
                    <releases>
                        <enabled>true</enabled>
                    </releases>
                    <snapshots>
                        <enabled>false</enabled>
                    </snapshots>
                </repository>
            </repositories>

            <pluginRepositories>
                <pluginRepository>
                    <id>nexus</id>
                    <name>local private nexus</name>
                    <url>http://maven.oschina.net/content/groups/public/</url>
                    <releases>
                        <enabled>true</enabled>
                    </releases>
                    <snapshots>
                        <enabled>false</enabled>
                    </snapshots>
                </pluginRepository>
            </pluginRepositories>
        </profile>
    </profiles>
    <activeProfiles>
        <!-- <activeProfile>nexus</activeProfile> -->
    </activeProfiles>
</settings>
```

# pip

modify `~/.config/pip/pip.conf`

```conf
[global]
# index-url = https://pypi.tuna.tsinghua.edu.cn/simple
index-url = https://mirrors.aliyun.com/pypi/simple/

[install]
install-option=--prefix=~/.local
trusted-host = mirrors.aliyun.com
```

# npm

    npm config set registry https://registry.npm.taobao.org
    npm config get registry


# gradle

set your project's build.gradle

```gradle
buildscript {
    repositories {
        maven{ url 'http://maven.aliyun.com/nexus/content/groups/public/' }
        maven{ url 'http://maven.aliyun.com/nexus/content/repositories/jcenter'}
    }
    dependencies {
        classpath 'com.android.tools.build:gradle:3.3.0-alpha13'
    }
}
allprojects {
    repositories {
        maven{ url 'http://maven.aliyun.com/nexus/content/groups/public/'}
        maven{ url 'http://maven.aliyun.com/nexus/content/repositories/jcenter'}
    }
}
task clean(type: Delete) {
    delete rootProject.buildDir
}
```

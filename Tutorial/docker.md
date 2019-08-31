---

title: Dorcker

date: 2019-08-28 21:23:39
tags: [How]
categories: [Tutorial]

---


<!-- vim-markdown-toc GFM -->

* [Install](#install)
* [What](#what)
    * [Dockerfile](#dockerfile)
    * [Containers](#containers)
    * [Networks](#networks)
    * [Restart Policy](#restart-policy)
* [Basic Command](#basic-command)
* [Tips & Tricks](#tips--tricks)
    * [Alias](#alias)
    * [Remove all containers with status=exited](#remove-all-containers-with-statusexited)
    * [Stop all containers](#stop-all-containers)
    * [Run and attach to container](#run-and-attach-to-container)
    * [Pass environment variables to docker](#pass-environment-variables-to-docker)
    * [Remove all docker images](#remove-all-docker-images)
    * [Bind local folder the docker folder on docker run](#bind-local-folder-the-docker-folder-on-docker-run)
    * [See logs in container](#see-logs-in-container)
    * [Backup/Restore of container](#backuprestore-of-container)
    * [Copy and paste files](#copy-and-paste-files)
    * [Binding ports](#binding-ports)
    * [Adding Metadata](#adding-metadata)
    * [Named/Path Based Volumes](#namedpath-based-volumes)
        * [Named Volumn](#named-volumn)
        * [Path Volumn](#path-volumn)
* [TODO](#todo)
* [References](#references)

<!-- vim-markdown-toc -->

<!-- more -->

# Install

docker-ce:

[官网](https://docs.docker.com/install/linux/docker-ce/debian/)

command:

```shell
wget -qO- https://get.docker.com/ | sh
# without sudo run
sudo usermod -aG docker $USER
# or sudo gpasswd -a $USER docker
newgrp docker
```

nvidia-docker:

[官网](https://nvidia.github.io/nvidia-docker/)

command:

```shell
curl -s -L https://nvidia.github.io/nvidia-docker/gpgkey | \
         sudo apt-key add -

distribution=$(. /etc/os-release;echo $ID$VERSION_ID)

curl -s -L https://nvidia.github.io/nvidia-docker/$distribution/nvidia-docker.list | \
         sudo tee /etc/apt/sources.list.d/nvidia-docker.list
         sudo apt-get update

sudo apt-get install nvidia-docker2
sudo pkill -SIGHUP dockerd
sudo /etc/init.d/docker restart
```

-----------------------------------------------------------------

# What

## Dockerfile

> A Dockerfile is a file that you create which in turn produces a Docker image when you build it.
>
> A Dockerfile is a __recipe__ for building Docker images, and the act of running
> a separate build command produces the Docker image from that __recipe__.
>
> In the world of object oriented programming, you often deal with classes. You can think of a Docker
> image as a class, where as a Docker container is an instance of that class. [^dockerfile]

**将docker image比作class, container比作instance不准确, 但足够理解**

[^dockerfile]: https://nickjanetakis.com/blog/differences-between-a-dockerfile-docker-image-and-docker-container

## Containers

> A Docker container is just a process / service that runs directly on your machine. It is slightly
different than a regular process because the Docker daemon along with the Linux kernel do a few things to
ensure it runs in total isolation.
>
> A virtual machine is typically used to isolate an entire system. [^container]

[^container]: https://nickjanetakis.com/blog/docker-tip-1-docker-containers-are-isolated-processes-not-virtual-machines


## Networks

Display docker networks supported now: `docker network  ls`

NETWORK ID      |   NAME        |       DRIVER       |      SCOPE
:----------:    |   :----:      |       :----:       |      :---:
69dc7a923543    |   bridge      |       bridge       |      local
ad480746c437    |   host        |       host         |      local
a5eec5e58ba1    |   none        |       null         |      local


- none

    没有网络,挂在这个网络下的容器除了**lo**,没有其他任何网卡, 用于隔离.

- host

    共享Docker宿主机的网络栈,即容器的网络配置与host宿主机完全一样, 如果容器对网络传输效率有较高要求即**性
    能高**,则可以选择host网络, 但需要注意**端口冲突**问题

- bridge

    Docker默认方式

    Docker在安装时会在宿主机上创建名为docker0的网桥,所谓网桥相当于一个**虚拟交换机**.容器和docker0之间通过
    veth进行连接,veth相当于一根虚拟网线,连接容器和虚拟交换机,这样就使得docker0与容器连通了.


例子:

- `docker run --rm -d --network host --name my_nginx nginx`


[自定义网络](https://www.jb51.net/article/120069.htm)
[官网](https://docs.docker.com/network/network-tutorial-standalone/)

## Restart Policy

有时开机自动启动container, 或者某个container挂掉之后能够自动重启.

如何选择最好的重启策略

Policy  |  Descriptor
:-----: | :----------
none | 不自动重启(默认)
on-failures | 当错误退出时重启
always | container停止时就重启, 系统reboot也会启动
unless-stopped | 手动stop后, 系统reboot不会启动, 其他和always一样

**有时我们停止container或者docker,或者reboot系统是为了某些更新, 所以不需要重启container,此时unless-stopped
很有用**

例如:

- `docker run -dit --restart unless-stopped redis`
- `docker run --restart on-failure:10 redis` 10: 尝试重启最大次数10次

[更多](https://blog.codeship.com/ensuring-containers-are-always-running-with-dockers-restart-policy/)

[官网](https://docs.docker.com/config/containers/start-containers-automatically/)

-----------------------------------------------------------------

# Command

## Download image

cmd: `docker pull debian`

## Rename image name

cmd: `docker image tag <source:tag> <target:tag>`

只是建立了软链接, 源image不会被删除, 如果强制(**-f**)删除source:tag源, target:tag也会删除

## Display image or container information

cmd: `docker inspect 4cd81e734bd8 --format  '\{\{json .ContainerConfig.Labels\}\}' | python -m json.tool`

cmd: `docker inspect 4cd81e734bd8 --format  '\{\{json .ContainerConfig.Labels\}\}' | jq`

output:

```json
 {
   "com.nvidia.cuda.version": "9.0.176",
   "com.nvidia.cudnn.version": "7.4.2.24",
   "com.nvidia.volumes.needed": "nvidia_driver",
   "maintainer": "colorai@colorai.com",
   "org.label-schema.build-date": "2019-08-30T13:46:14Z",
   "org.label-schema.description": "Computer Vision Backend for Cauchy",
   "org.label-schema.docker.cmd": "docker run -d --name colorai/cauchycv_visdom --restart unless-stopped --volume /data:/data --network host --hostname colorai--runtime nvidia -shm-size=2g --ulimit memlock=-1 --ulimit stack=67108864 colorai/cauchycv_visdom:0.2.6 python cauchy_services.py --port 8339",
   "org.label-schema.name": "colorai/cauchycv_visdom",
   "org.label-schema.schema-version": "1.0",
   "org.label-schema.url": "https://www.colorai.com/index.php?r=front",
   "org.label-schema.vcs-branch": "master",
   "org.label-schema.vcs-ref": "1efa1f1",
   "org.label-schema.vcs-url": "git@gitee.com:colorai/sig_cauchy_cv.git",
   "org.label-schema.vendor": "ColorAI",
   "org.label-schema.version": "1efa1f1"
 }
```

## Display information using index

cmd: `docker inspect 98a757ed7816 --format '\{\{index .ContainerConfig.Labels "org.label-schema.docker.cmd"\}\}'`

如果读取的key有'.'等复杂的情况, 可使用'index'命令

## Filter label and format it

cmd: `docker images --filter "label=org.label-schema.vendor=ColorAI" --format "\{\{.Repository\}\}:\{\{.Tag\}\}"`

output:

```
colorai/cauchycv_visdom:0.2.6
colorai/cauchycv_visdom:0.2.5
```

## Display container running state

cmd: `docker inspect 3dd6dccbce14 --format '\{\{json .State\}\}' | python -m json.tool`

```json
{
    "Dead": false,
    "Error": "",
    "ExitCode": 0,
    "FinishedAt": "0001-01-01T00:00:00Z",
    "OOMKilled": false,
    "Paused": false,
    "Pid": 27140,
    "Restarting": false,
    "Running": true,
    "StartedAt": "2019-08-30T14:38:14.321067445Z",
    "Status": "running"
}
```

## Display container by image, tag and status

cmd: `docker container ls --filter ancestor=colorai/cauchycv_visdom:0.2.6 --filter status=running`

## Display the command in runtime

cmd: `docker container ls --format "\{\{.ID\}\}: \{\{.Command\}\}" --no-trunc`
cmd: `docker container ls --format "table \{\{.ID\}\}\t\{\{.Labels\}\}"`

## Alias command

```shell
alias di='docker images'
alias dri='docker rmi'

alias dc='docker container ls -a'
alias drc='docker container rm'
alias dsc='docker container stop'

alias dv='docker volume ls'
alias drv='docker volume rm'

alias dip='docker inspect --format="\{\{range .NetworkSettings.Networks\}\}\{\{.IPAddress\}\}\{\{end\}\}"'
alias dit='docker run -it'

alias din='docker inspect'
alias dlg='docker logs --follow'

dsh()
{
    args=($@)
    container=$1
    bashcmd=${args[@]: 1:$# }
    docker exec $container bash -c "$bashcmd"
}
```

## Remove all containers with status=exited

cmd: `docker rm $(docker ps -q -f status=exited)`

## Stop all containers

cmd: `docker stop $(docker ps -aq)`

## Run and attach to container

Every time with using docker run will create new container with specified image.

Use option --rm so container will removed after it finishes.

cmd: `docker run -it --rm <image_name> <commoand>`

## Pass environment variables to docker

cmd: `docker run -it -e TEST=1234 --env TEST1=3456 --rm alpine /bin/ash`

cmd: `docker run -it --env-file ./env.list alpine /bin/ash`

evn.file:

```sh
TEST=1234
TEST1=5678
```

## Remove all docker images

cmd: `docker rmi $(docker images -q)`

## Bind local folder the docker folder on docker run

cmd: `docker run -it -v /LOCAL_PATH:/CONTAINER_PATH <container_image>`

## See logs in container

cmd: `docker logs --follow <CONTAINER>`

cmd: `docker logs --timestamps --tail 1000 <CONTAINER> 2>&1 | grep -i error

## Backup/Restore of container

cmd:
```shell
docker commit -p <CONTAINER_ID> <YOUR_BACKUP_NAME>

docker save -o <CONTAINER_FILE>.tar <YOUR_BACKUP_NAME>

docker load -i <CONTAINER_FILE>.tar

```

## Copy and paste files

1. at compile(build) time

Dockerfile

```shell
COPY script.sh /tmp
ADD script.sh /tmp
ADD scripts.tar.gz /tmp
ADD http://www.example.com/script.sh /tmp
```

2. at runtime

```
# from host
docker cp script.sh container_name:/tmp/
docker exec -it container_name bash -c 'tree -a /tmp'


# to host
docker cp container_name:/tmp/. .
```

## Binding ports

host port: 8080

container port: 80

cmd: `sudo docker run --name docker-nginx -p 8080:80 -d -v /docker-nginx/html:/usr/share/nginx/html nginx`

## Adding Metadata

Docker Labels allow you to specify metadata for Docker objects such as Images, Containers, Volumes etc,
that will be packaged in to their specific formats. We are interested in how we can leverage Labels for
Docker Images.

```shell
# Dockerfile
LABEL version="1.0" maintainer="colorai <colorai@colorai.com>"
LABEL build_date="2017-09-05"

# Command
docker build . --label "version=1.0" --label "maintaner=colorai <colorai@colorai.com>"
```

使用`docker inspect`可查看metadata

> 实例: 标准形式[label-schema](http://label-schema.org/rc1/)
>
> > ```sh
> > DATE=$(date -u +'%Y-%m-%dT%H:%M:%SZ')
> > VERSION=$(git describe --tags --always)
> > URL=$(git config --get remote.origin.url)
> > COMMIT=$(git rev-parse HEAD | cut -c 1-7)
> > BRANCH=$(git rev-parse --abbrev-ref HEAD)
> >
> > REPOSITORY="test"
> > TAG="0.2.$(git rev-list HEAD | wc -l | awk '{print $1}')"
> >
> > docker build --tag $REPOSITORY:$TAG \
> >              --build-arg REPOSITORY=$REPOSITORY \
> >              --build-arg TAG=$TAG \
> >              --build-arg DATE=$DATE \
> >              --build-arg VERSION=$VERSION \
> >              --build-arg URL=$URL \
> >              --build-arg COMMIT=$COMMIT \
> >              --build-arg BRANCH=$BRANCH \
> > ```
> > Dockerfile:
> >
> > > ```
> > > LABEL maintainer="colorai@colorai.com"
> > >
> > > ARG VENDOR="ColorAI"
> > > ARG REPOSITORY
> > > ARG TAG
> > > ARG DATE
> > > ARG VERSION
> > > ARG URL
> > > ARG COMMIT
> > > ARG BRANCH
> > >
> > > LABEL org.label-schema.schema-version="1.0" \
> > >       org.label-schema.build-date=$DATE \
> > >       org.label-schema.name=$REPOSITORY \
> > >       org.label-schema.description="Computer Vision Backend for Cauchy" \
> > >       org.label-schema.url=https://www.colorai.com/index.php?r=front \
> > >       org.label-schema.vcs-url=$URL \
> > >       org.label-schema.vcs-ref=$COMMIT \
> > >       org.label-schema.vcs-branch=$BRANCH \
> > >       org.label-schema.vendor=$VENDOR \
> > >       org.label-schema.version=$VERSION \
> > >       org.label-schema.docker.cmd="docker run -d --name framework \
> > > --restart unless-stopped --volume /data:/data --network host --hostname colorai \
> > > --runtime nvidia --shm-size=2g --ulimit memlock=-1 --ulimit stack=67108864 \
> > > $REPOSITORY:$TAG python cauchy_services.py --port 8339"
> > > ```

## Named/Path Based Volumes

用于实现**容器与host共享数据**, volume数据可以被永久的保存,即使使用它的容器已经销毁.

list: `docker volume ls [-q]`

delete: `docker volume rm mydata`

delete all: `docker volume prune`

### Named Volumn

cmd: `docker run -it -v mydata:/data debian`, `docker volume inspect mydata`

output:

```json
[
    {
        "CreatedAt": "2019-08-29T14:42:54+08:00",
        "Driver": "local",
        "Labels": null,
        "Mountpoint": "/var/lib/docker/volumes/mydata/_data",
        "Name": "mydata",
        "Options": null,
        "Scope": "local"
    }
]
```

### Path Volumn

cmd: `docker run -it -v /hostdata:/targetdata debian`


[参考](https://my.oschina.net/665544/blog/1933032)
[简书](https://www.jianshu.com/p/655c934b4e4c)

-----------------------------------------------------------------

# TODO

1. permissions: --user

# References

- <https://nickjanetakis.com/blog/tag/docker-tips-tricks-and-tutorials>
- <https://blog.codeship.com/ensuring-containers-are-always-running-with-dockers-restart-policy>
- <https://www.mankier.com/1/docker-container-ls>
- <https://www.centos.bz/2017/01/docker-ps-list-containers>

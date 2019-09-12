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
* [Alias](#alias)
* [Command](#command)
    * [Search image](#search-image)
    * [Download image](#download-image)
    * [Rename image name](#rename-image-name)
    * [Display image or container information](#display-image-or-container-information)
    * [Display information using index](#display-information-using-index)
    * [Filter label and format it](#filter-label-and-format-it)
    * [Display container running state](#display-container-running-state)
    * [Display container by image, tag and status](#display-container-by-image-tag-and-status)
    * [Display the command in runtime](#display-the-command-in-runtime)
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
* [Expose port](#expose-port)
* [Composer](#composer)
    * [install](#install-1)
    * [yaml](#yaml)
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

# Alias

```{.bash .numberLines startFrom="1"}
alias di='docker images'
dirm()
{
    if [[ x$1 == x ]]
    then
        docker images
        echo -ne "\n[RM] Input ID: "
        read image
    else
        image=$@
    fi
    docker rmi $image
}

alias dc='docker container ls -a'
dcrm()
{
    if [[ x$1 == x ]]
    then
        docker container ls -a
        echo -ne "\n[RM] Input ID: "
        read container
    else
        container=$@
    fi
    docker container stop $container
    docker container rm $container
}

alias dcrma='docker container prune'
alias dcstart='docker container start'
alias dcstop='docker container stop'

alias dv='docker volume ls'
alias dvrm='docker volume rm'

drun()
{
    args=($@)
    image=$1
    if [[ x$image == x ]] || [[ x$image != x && "$\{\#image}" -ne "12"
    then
        bashcmd=$image
        docker images
        echo -ne "\n[RUN] Input ID: "
        read image
    else
        bashcmd=${args[@]: 1:$\#\}
    fi
    docker run -it -d $image $bashcmd
}

dsh()
{
    if [[ x$1 == x ]]
    then
        docker container ls
        echo -ne "\n[SH] Input ID: "
        read container
    else
        container=$1
    fi
    docker exec -it $container bash
}


dlog()
{
    if [[ x$1 == x ]]
    then
        docker container ls
        echo -ne "\n[LOG] Input ID: "
        read container
    else
        container=$1
    fi
    docker logs --timestamps --follow $container
}

dexe()
{
    args=($@)
    container=$1
    if [ "$\{\#container}" -eq "12" ]
    then
        bashcmd=${args[@]: 1:$\#\}
        docker exec $container bash -c "$bashcmd"
    else
        docker container ls
        echo -ne "\n[SH] Input ID: "
        read container
        echo ""
        docker exec $container bash -c "$@"
    fi
}

dip()
{
    container=$1
    if [ "$\{\#container}" -ne "12" ]
    then
        docker container ls
        echo -ne "\n[IP] Input ID: "
        read container
    fi
    docker inspect --format="\{\{range .NetworkSettings.Networks\}\}\{\{.IPAddress\}\}\{\{end\}\}" $container
}
```

-----------------------------------------------------------------

# Command

## Search image

cmd: `docker search hadoop`

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

### Path Volumn (Mount)

cmd: `docker run -it -v /hostdata:/targetdata debian`


[参考](https://my.oschina.net/665544/blog/1933032)
[简书](https://www.jianshu.com/p/655c934b4e4c)


# Expose port

[非常详细的讲解](https://blog.csdn.net/qq_17639365/article/details/86655177)

推荐直接使用手动映射, 清晰明了.

参数:

- `-P`: 自动映射
- `-p`: 手动映射, `-p host_port:container_port`

场景: (假设端口80)

1. 情况一:暴露端口80,不使用映射

- [ ] 主机地址:端口80
- [x] 容器地址:端口80

2. 情况二:暴露端口80,使用自动映射-P (假设随机映射的端口为8000)

- [ ] 主机地址:端口80
- [x] 主机地址:端口8000
- [x] 容器地址:端口80

3. 情况三:暴露端口80,使用手动映射-p (假设手动映射的端口为8888:80)

- [x] 主机地址:端口8888
- [x] 容器地址:端口80

4. 情况四:不暴露端口,不使用映射

- [ ] 主机地址:端口80
- [x] 容器地址:端口80

5. 情况五:不暴露端口,使用自动映射-P (无自动映射的端口)

- [ ] 主机地址:端口80
- [x] 容器地址:端口80

6. 情况六:不暴露端口,使用手动映射-p (假设手动映射的端口为8888:80)

- [x] 主机地址:端口8888
- [x] 容器地址:端口80

-----------------------------------------------------------------

# Composer

## install

[see docs.docker.com](https://docs.docker.com/compose/install/)

## yaml

```
version                         # 指定 compose 文件的版本
    services                    # 定义所有的 service 信息, services 下面的第一级别的 key 是一个 service 的名称
        build                   # 指定包含构建上下文的路径, 或作为一个对象,该对象具有 context 和指定的 dockerfile 文件以及 args 参数值
            context             # context: 指定 Dockerfile 文件所在的路径
            dockerfile          # dockerfile: 指定 context 指定的目录下面的 Dockerfile 的名称(默认为 Dockerfile)
            args                # args: Dockerfile 在 build 过程中需要的参数 (等同于 docker container build --build-arg 的作用)
                - xxx=xxx
            cache_from          # v3.2中新增的参数, 指定缓存的镜像列表 (等同于 docker container build --cache_from 的作用)
            labels              # v3.3中新增的参数, 设置镜像的元数据 (等同于 docker container build --labels 的作用)
            shm_size            # v3.5中新增的参数, 设置容器 /dev/shm 分区的大小 (等同于 docker container build --shm-size 的作用)
        command                 # 覆盖容器启动后默认执行的命令, 支持 shell 格式和 [] 格式
        configs                 # TODO
        cgroup_parent           # TODO
        container_name          # 指定容器的名称 (等同于 docker run --name 的作用)
        credential_spec         # TODO
        deploy                  # v3 版本以上, 指定与部署和运行服务相关的配置, deploy 部分是 docker stack 使用的, docker stack 依赖 docker swarm
            endpoint_mode           # v3.3 版本中新增的功能, 指定服务暴露的方式
                vip                 # Docker 为该服务分配了一个虚拟 IP(VIP), 作为客户端的访问服务的地址
                dnsrr               # DNS轮询, Docker 为该服务设置 DNS 条目, 使得服务名称的 DNS 查询返回一个 IP 地址列表, 客户端直接访问其中的一个地址
            labels                  # 指定服务的标签,这些标签仅在服务上设置
            mode                    # 指定 deploy 的模式
                global              # 每个集群节点都只有一个容器
                replicated          # 用户可以指定集群中容器的数量(默认)
            placement               # 不知道怎么用
            replicas                # deploy 的 mode 为 replicated 时, 指定容器副本的数量
            resources               # 资源限制
                limits              # 设置容器的资源限制
                    cpus: "0.5"     # 设置该容器最多只能使用 50% 的 CPU
                    memory: 50M     # 设置该容器最多只能使用 50M 的内存空间
                reservations        # 设置为容器预留的系统资源(随时可用)
                    cpus: "0.2"     # 为该容器保留 20% 的 CPU
                    memory: 20M     # 为该容器保留 20M 的内存空间
            restart_policy          # 定义容器重启策略, 用于代替 restart 参数
                condition           # 定义容器重启策略(接受三个参数)
                    none            # 不尝试重启
                    on-failure      # 只有当容器内部应用程序出现问题才会重启
                    any             # 无论如何都会尝试重启(默认)
                delay               # 尝试重启的间隔时间(默认为 0s)
                max_attempts        # 尝试重启次数(默认一直尝试重启)
                window              # 检查重启是否成功之前的等待时间(即如果容器启动了, 隔多少秒之后去检测容器是否正常, 默认 0s)
            update_config           # 用于配置滚动更新配置
                parallelism         # 一次性更新的容器数量
                delay               # 更新一组容器之间的间隔时间
                failure_action      # 定义更新失败的策略
                    continue        # 继续更新
                    rollback        # 回滚更新
                    pause           # 暂停更新(默认)
                monitor             # 每次更新后的持续时间以监视更新是否失败(单位: ns|us|ms|s|m|h) (默认为0)
                max_failure_ratio   # 回滚期间容忍的失败率(默认值为0)
                order               # v3.4 版本中新增的参数, 回滚期间的操作顺序
                    stop-first      # 旧任务在启动新任务之前停止(默认)
                    start-first     # 首先启动新任务, 并且正在运行的任务暂时重叠
            rollback_config         # v3.7 版本中新增的参数, 用于定义在 update_config 更新失败的回滚策略
                parallelism         # 一次回滚的容器数, 如果设置为0, 则所有容器同时回滚
                delay               # 每个组回滚之间的时间间隔(默认为0)
                failure_action      # 定义回滚失败的策略
                    continue        # 继续回滚
                    pause           # 暂停回滚
                monitor             # 每次回滚任务后的持续时间以监视失败(单位: ns|us|ms|s|m|h) (默认为0)
                max_failure_ratio   # 回滚期间容忍的失败率(默认值0)
                order               # 回滚期间的操作顺序
                    stop-first      # 旧任务在启动新任务之前停止(默认)
                    start-first     # 首先启动新任务, 并且正在运行的任务暂时重叠

        devices                 # 指定设备映射列表 (等同于 docker run --device 的作用)
        depends_on              # 定义容器启动顺序 (此选项解决了容器之间的依赖关系, 此选项在 v3 版本中 使用 swarm 部署时将忽略该选项)
                #####################################################################
                # 示例:
                #     默认情况下使用 docker-compose up web 这样的方式启动 web 服务时,也会启动 redis 和 db 两个服务.
                #     version: '3'
                #     services:
                #         web:
                #             build: .
                #             depends_on:
                #                 - db
                #                 - redis
                #         redis:
                #             image: redis
                #         db:
                #             image: postgres
                #####################################################################
        dns                     # 设置 DNS 地址(等同于 docker run --dns 的作用)
        dns_search              # 设置 DNS 搜索域(等同于 docker run --dns-search 的作用)
        tmpfs                   # v2 版本以上, 挂载目录到容器中, 作为容器的临时文件系统(等同于 docker run --tmpfs 的作用, 在使用 swarm 部署时将忽略该选项)
        entrypoint              # 覆盖容器的默认 entrypoint 指令 (等同于 docker run --entrypoint 的作用)
        env_file                # 从指定文件中读取变量设置为容器中的环境变量, 可以是单个值或者一个文件列表, 如果多个文件中的变量重名则后面的变量覆盖前面的变量, environment 的值覆盖 env_file 的值
            - ./xxx.env
        environment             # 设置环境变量, environment 的值可以覆盖 env_file 的值 (等同于 docker run --env 的作用)
            - xxx_env=xxx_val
        expose                  # 暴露端口, 但是不能和宿主机建立映射关系, 类似于 Dockerfile 的 EXPOSE 指令
        external_links          # 连接不在 docker-compose.yml 中定义的容器或者不在 compose 管理的容器(docker run 启动的容器, 在 v3 版本中使用 swarm 部署时将忽略该选项)
        extra_hosts             # 添加 host 记录到容器中的 /etc/hosts 中 (等同于 docker run --add-host 的作用)
        healthcheck             # v2.1 以上版本, 定义容器健康状态检查, 类似于 Dockerfile 的 HEALTHCHECK 指令
            test                # 检查容器检查状态的命令, 该选项必须是一个字符串或者列表, 第一项必须是 NONE, CMD 或 CMD-SHELL, 如果其是一个字符串则相当于 CMD-SHELL 加该字符串
                NONE            # 禁用容器的健康状态检测
                CMD             # test: ["CMD", "curl", "-f", "http://localhost"]
                CMD-SHELL       # test: ["CMD-SHELL", "curl -f http://localhost || exit 1"] 或者 test: curl -f https://localhost || exit 1
            interval: 1m30s     # 每次检查之间的间隔时间
            timeout: 10s        # 运行命令的超时时间
            retries: 3          # 重试次数
            start_period: 40s   # v3.4 以上新增的选项, 定义容器启动时间间隔
            disable: true       # true 或 false, 表示是否禁用健康状态检测和 test: NONE 相同
        image                   # 指定 docker 镜像, 可以是远程仓库镜像 本地镜像
        init                    # v3.7 中新增的参数, true 或 false 表示是否在容器中运行一个 init, 它接收信号并传递给进程
        isolation               # 隔离容器技术, 在 Linux 中仅支持 default 值
        labels                  # 使用 Docker 标签将元数据添加到容器, 与 Dockerfile 中的 LABELS 类似
        links                   # 链接到其它服务中的容器, 该选项是 docker 历史遗留的选项, 目前已被用户自定义网络名称空间取代, 最终有可能被废弃 (在使用 swarm 部署时将忽略该选项)
        logging                 # 设置容器日志服务
            driver              # 指定日志记录驱动程序, 默认 json-file (等同于 docker run --log-driver 的作用)
            options             # 指定日志的相关参数 (等同于 docker run --log-opt 的作用)
                max-size        # 设置单个日志文件的大小, 当到达这个值后会进行日志滚动操作
                max-file        # 日志文件保留的数量
        network_mode            # 指定网络模式 (等同于 docker run --net 的作用, 在使用 swarm 部署时将忽略该选项)
        networks                # 将容器加入指定网络 (等同于 docker network connect 的作用), networks 可以位于 compose 文件顶级键和 services 键的二级键
            aliases             # 同一网络上的容器可以使用服务名称或别名连接到其中一个服务的容器
            ipv4_address        # IP V4 格式
            ipv6_address        # IP V6 格式
                #####################################################################
                # 示例:
                #     version: '3.7'
                #     services:
                #         test:
                #             image: nginx:1.14-alpine
                #             container_name: mynginx
                #             command: ifconfig
                #             networks:
                #                 app_net:
                #                 ipv4_address: 172.16.238.10
                #     networks:
                #         app_net:
                #             driver: bridge
                #             ipam:
                #                 driver: default
                #                 config:
                #                     - subnet: 172.16.238.0/24
                #####################################################################
        pid: 'host'             # 共享宿主机的 进程空间(PID)
        ports                   # 建立宿主机和容器之间的端口映射关系, ports 支持两种语法格式
                #####################################################################
                # SHORT 语法格式示例:
                #     - "3000"                  # 暴露容器的 3000 端口, 宿主机的端口由 docker 随机映射一个没有被占用的端口
                #     - "3000-3005"             # 暴露容器的 3000 到 3005 端口, 宿主机的端口由 docker 随机映射没有被占用的端口
                #     - "8000:8000"             # 容器的 8000 端口和宿主机的 8000 端口建立映射关系
                #     - "9090-9091:8080-8081"
                #     - "127.0.0.1:8001:8001"   # 指定映射宿主机的指定地址的
                #     - "127.0.0.1:5000-5010:5000-5010"
                #     - "6060:6060/udp"         # 指定协议
                # LONG 语法格式示例:(v3.2)
                #     ports:
                #           target: 80          # 容器端口
                #           published: 8080     # 宿主机端口
                #           protocol: tcp       # 协议类型
                #           mode: host          # host 在每个节点上发布主机端口,  ingress 对于群模式端口进行负载均衡
                #####################################################################
        secrets                 # 不知道怎么用
        security_opt            # 为每个容器覆盖默认的标签 (在使用 swarm 部署时将忽略该选项)
        stop_grace_period       # 指定在发送了 SIGTERM 信号之后, 容器等待多少秒之后退出(默认 10s)
        stop_signal             # 指定停止容器发送的信号 (默认为 SIGTERM 相当于 kill PID; SIGKILL 相当于 kill -9 PID; 在使用 swarm 部署时将忽略该选项)
        sysctls                 # 设置容器中的内核参数 (在使用 swarm 部署时将忽略该选项)
        ulimits                 # 设置容器的 limit
        userns_mode             # 如果Docker守护程序配置了用户名称空间, 则禁用此服务的用户名称空间 (在使用 swarm 部署时将忽略该选项)
        volumes                 # 定义容器和宿主机的卷映射关系, 其和 networks 一样可以位于 services 键的二级键和 compose 顶级键, 如果需要跨服务间使用则在顶级键定义, 在 services 中引用
                #####################################################################
                # SHORT 语法格式示例:
                #     volumes:
                #         - /var/lib/mysql                # 映射容器内的 /var/lib/mysql 到宿主机的一个随机目录中
                #         - /opt/data:/var/lib/mysql      # 映射容器内的 /var/lib/mysql 到宿主机的 /opt/data
                #         - ./cache:/tmp/cache            # 映射容器内的 /var/lib/mysql 到宿主机 compose 文件所在的位置
                #         - ~/configs:/etc/configs/:ro    # 映射容器宿主机的目录到容器中去, 权限只读
                #         - datavolume:/var/lib/mysql     # datavolume 为 volumes 顶级键定义的目录, 在此处直接调用
                # LONG 语法格式示例:(v3.2)
                #     version: "3.2"
                #     services:
                #         web:
                #             image: nginx:alpine
                #             ports:
                #                 - "80:80"
                #             volumes:
                #                 - type: volume          # mount 的类型, 必须是 bind volume 或 tmpfs
                #                     source: mydata      # 宿主机目录
                #                     target: /data       # 容器目录
                #                     volume:             # 配置额外的选项, 其 key 必须和 type 的值相同
                #                         nocopy: true    # volume 额外的选项, 在创建卷时禁用从容器复制数据
                #                 - type: bind            # volume 模式只指定容器路径即可, 宿主机路径随机生成; bind 需要指定容器和数据机的映射路径
                #                     source: ./static
                #                     target: /opt/app/static
                #                     read_only: true     # 设置文件系统为只读文件系统
                #####################################################################
                volumes:
                    mydata:     # 定义在 volume, 可在所有服务中调用
        restart                 # 定义容器重启策略(在使用 swarm 部署时将忽略该选项, 在 swarm 使用 restart_policy 代替 restart)
            no                  # 禁止自动重启容器(默认)
            always              # 无论如何容器都会重启
            on-failure          # 当出现 on-failure 报错时, 容器重新启动

        domainname              # TODO
        hostname                # TODO
        ipc                     # TODO
        mac_address             # TODO
        privileged              # TODO
        read_only               # TODO
        stdin_open              # TODO
        tty                     # TODO
        user                    # TODO
        working_dir             # TODO
        shm_size                # TODO
                #####################################################################
                # 对于值为时间的可接受的值(单位: us, ms, s, m, h):
                #     2.5s
                #     10s
                #     1m30s
                #     2h32m
                #     5h34m56s
                # 对于值为大小的可接受的值(单位: b, k, m, g 或者 kb, mb, gb):
                #     2b
                #     1024kb
                #     2048k
                #     300m
                #     1gb
                #####################################################################
    networks                    # 定义 networks 信息
        driver                  # 指定网络模式, 大多数情况下, 它 bridge 于单个主机和 overlay Swarm 上
            bridge              # Docker 默认使用 bridge 连接单个主机上的网络
            overlay             # overlay 驱动程序创建一个跨多个节点命名的网络
            host                # 共享主机网络名称空间(等同于 docker run --net=host)
            none                # 等同于 docker run --net=none
        driver_opts             # v3.2以上版本, 传递给驱动程序的参数, 这些参数取决于驱动程序
        attachable              # driver 为 overlay 时使用, 如果设置为 true 则除了服务之外,独立容器也可以附加到该网络; 如果独立容器连接到该网络,则它可以与其他 Docker 守护进程连接到的该网络的服务和独立容器进行通信
        ipam                    # 自定义 IPAM 配置. 这是一个具有多个属性的对象, 每个属性都是可选的
            driver              # IPAM 驱动程序, bridge 或者 default
            config              # 配置项
               subnet           # CIDR格式的子网,表示该网络的网段
       external                 # 外部网络, 如果设置为 true 则 docker-compose up 不会尝试创建它, 如果它不存在则引发错误
       name                     # v3.5 以上版本, 为此网络设置名称
```

# TODO

1. permissions: --user

# References

- <https://nickjanetakis.com/blog/tag/docker-tips-tricks-and-tutorials>
- <https://blog.codeship.com/ensuring-containers-are-always-running-with-dockers-restart-policy>
- <https://www.mankier.com/1/docker-container-ls>
- <https://www.centos.bz/2017/01/docker-ps-list-containers>
- <https://blog.csdn.net/u010918487/article/details/89452230>
- <https://takacsmark.com/docker-compose-tutorial-beginners-by-example-basics>
- <https://www.cnblogs.com/hongdada/p/9488349.html>

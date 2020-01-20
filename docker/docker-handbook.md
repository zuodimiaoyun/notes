#### 获取镜像
- ```
  docker pull [option] [Docker Registry 地址[:端口号]/]仓库名[:标签]
  ```
- Docker Registry 默认 Docker Hub
- 仓库名由 用户名/软件名 组成，Docker Hub 默认用户名为library
#### 镜像加速
##### 国内镜像地址
- Azure 中国镜像 https://dockerhub.azk8s.cn
- 七牛云加速器 https://reg-mirror.qiniu.com
- 阿里云 https://registry.aliyuncs.com
##### 配置
- 在/etc/docker/daemon.json中配置：
```
{
    "registry-mirrors": [
        "https://dockerhub.azk8s.cn",
        "https://reg-mirror.qiniu.com"
        "https://registry.aliyuncs.com"
    ]
}
```
- systemctl daemon-reload
- systemctl restart docker
        
- docker info 检查是否配置成功
- 国内无法直接获取 gcr.io/* 镜像，我们可以将 gcr.io/\<repo-name\>/\<image-name\>:\<version\> 替换为 gcr.azk8s.cn/\<repo-name\>/\<image-name\>:\<version\> ,例如 docker pull gcr.azk8s.cn/google_containers/hyperkube-amd64:v1.9.2
#### 运行镜像容器
```
docker run -it --rm ubuntu:18.04 bash
```
- -it
  - 两个参数 -i是交互式操作 -t是终端
- --rm
  - 表示容器退出后将其删除，不加这个参数的话，退出容器后不会删除，需要手动执行docker rm来释放
- ubuntu:18.04
  - 这里指用ubuntu:18.04镜像作为基础来启动容器
- bash
  - 放在镜像名后的是【命令】，这里我们希望有个交互式shell，用的是bash
- -p 
  - <宿主端口>:<容器端口>
  
#### 列出镜像
- docker image ls
  - docker hub上的镜像大小是镜像压缩后的大小，从仓库获取镜像需要这个流量，ls列出的大小是镜像展开后的大小，即磁盘占用空间
- docker system df
  - 镜像、容器、数据卷占用的空间
##### 虚悬镜像
- docker官方维护镜像发布新版本后，原来同名的镜像的名称和标签会被替换成<none>,被称为虚悬镜像，已失去价值可删除
- docker image ls -f dangling=true 列出虚悬镜像
- docker image prune 删除虚悬镜像
##### 中间层镜像
- docker image ls 默认只列出顶层镜像
- docker image ls -a 列出所有镜像，包括中间层镜像
##### 列出所需镜像
- docker image ls 仓库名[:标签]
- docker image ls -f since[before]=mongo:3.2 列出mongo:3.2之后[之- 前]建立的镜像
- docker image ls -f label=com.example.version=0.1 如果在构建镜像的- 时候定义了LABEL，也可以通过LABEL来过滤
- -f=--filter
- docker image ls -q 只列出ID
- docker image -ls --format "{{.ID}}: {{.Repository}}" 格式化输出结- 果
- docker image ls --format "table {{.ID}}\t{{.Repository}}\t{- {.Tag}}" 以表格形式格式化输出
#### 删除镜像
- docker image rm [选项] <镜像1> [<镜像2> ...]
- 镜像参数可以是 镜像名、镜像短ID、镜像长ID、镜像摘要
#### Docker 版本
- 在 Docker CE 18.09 版本中默认使用的是 overlay2 驱动。
- 之前版本对于CentOS/RHEL系统来说使用 devicemapper + loop-lvm实现驱- 动，性能稳定性差
#### 进入容器
- docker exec -it 容器名 bash
#### 删除容器
- docker rm $(docker stop $(docker ps -q)) 查询当前正在运行的容器，停止容器，删除容器
#### 制作镜像
1. 新建Dockerfile:
```
FROM debian:stretch

RUN buildDeps='gcc libc6-dev make wget' \
    && apt-get update \
    && apt-get install -y $buildDeps \
    && wget -O redis.tar.gz "http://download.redis.io/releases/redis-5.0.3.tar.gz" \
    && mkdir -p /usr/src/redis \
    && tar -xzf redis.tar.gz -C /usr/src/redis --strip-components=1 \
    && make -C /usr/src/redis \
    && make -C /usr/src/redis install \
    && rm -rf /var/lib/apt/lists/* \
    && rm redis.tar.gz \
    && rm -r /usr/src/redis \
    && apt-get purge -y --auto-remove $buildDeps
    # 一层镜像，清理编译时依赖以及产生的中间文件
```
2. 构建
- 上下文构建
```
docker build -t nginx:v3 .
# . 是docker上下文路径，会被打包上传至docker服务端  
# .dockerignore可以过滤打包给docker服务端的内容
```
- 指定Dockerfile构建
```
docker build -t xxx . -f ../Dockerfile.php
```
- 指定url构建
```
docker build https://github.com/twang2218/gitlab-ce-zh.git#:11.1 
# 会默认从git clone代码切换到master分支，进入/11.1/目录进行构建
```
- 通过压缩包构建
```
docker build http://server/context.tar.gz
# 把压缩包当作上下文构建
```
- 从标准输入无上下文构建
```
docker build - < Dockerfile
```
- 从标准输入中读取上下文压缩包进行构建
```
docker build - < context.tar.gz
```
#### Dockerfile命令
##### COPY
- COPY [--chown=<user>:<group>] <源路径>... <目标路径> 会保留文件元数据，目标路径不存在会自动创建，目标路径可以是相对或绝对路径
- COPY --from=nginx:latest /etc/nginx/nginx.conf /nginx.conf 复制镜像中的问题
- COPY --from=0 /etc/nginx/nginx.conf /nginx.conf 复制第一阶段构建中的文件
##### ADD 更高级的复制文件
- ADD ubuntu-xenial-core-cloudimg-amd64-root.tar.gz / 会自动解压
- ADD 指令会令镜像构建缓存失效，从而可能会令镜像构建变得比较缓慢，除了解压场景不应该使用ADD而应该使用COPY
##### CMD 容器启动时运行的程序和命令
- CMD echo $HOME 等同于 CMD [ "sh", "-c", "echo $HOME" ]
- 容器中没有后台的概念，容器运行的主进程退出，容器就会退出
  - CMD service nginx start 容器会直接退出
  - CMD ["nginx", "-g", "daemon off;"] 容器不会直接退出
##### ENTRYPOINT
- 替代CMD，接受CMD内容当作参数
- redis镜像官方示例
```
Dockerfile 

    FROM alpine:3.4
    ...
    RUN addgroup -S redis && adduser -S -G redis redis
    ...
    ENTRYPOINT ["docker-entrypoint.sh"]

    EXPOSE 6379
    CMD [ "redis-server" ]

docker-entrypoint.sh

    #!/bin/sh
    ...
    # allow the container to be started with `--user`
    if [ "$1" = 'redis-server' -a "$(id -u)" = '0' ]; then
        chown -R redis .
        exec su-exec redis "$0" "$@"
    fi

    exec "$@"
```
##### ENV 设置环境变量
- ENV \<key\> \<value\>
- ENV \<key1\>=\<value1\> \<key2\>=\<value2\>...
  - 定义环境变量时使用\来换行，value中有空格需要用“”括起来
##### ARG 构建参数（同环境变量）
- ARG <参数名>[=<默认值>]
  - docker build --build-arg <参数名>=<值> 时覆盖（1.13版本之后可传入ARG中没有定义的参数）
##### VOLUME 定义匿名卷，为了使动态文件不写入容器的存储层
- VOLUME ["<路径1>", "<路径2>"...]
- VOLUME <路径>
```
VOLUME /data
docker run -d -v mydata:/data xxxx
```
##### EXPOSE 声明端口
- EXPOSE <端口1> [<端口2>...]
  - 指令是声明运行时容器提供服务端口，只是声明，真正映射需要在运行时通过 -p <宿主端口>:<容器端口> 完成映射
##### WORKDIR 指定工作目录
- WORKDIR <工作目录路径>
##### USER 指定当前用户
- 切换user建议使用gosu ：https://github.com/tianon/gosu
##### HEALTHCHECK 健康检查（Docker 1.12 引入）
- HEALTHCHECK [选项] CMD <命令>：设置检查容器健康状况的命令
- HEALTHCHECK NONE：如果基础镜像有健康检查指令，使用这行可以屏蔽掉其健康检查指令
  - --interval=<间隔>：两次健康检查的间隔，默认为 30 秒；
  - --timeout=<时长>：健康检查命令运行超时时间，如果超过这个时间，本次健康检查就被视为失败，默认 30 秒；
  - --retries=<次数>：当连续失败指定次数后，则将容器状态视为 unhealthy，默认 3 次。
- 和 CMD, ENTRYPOINT 一样，HEALTHCHECK 只可以出现一次，如果写了多个，只有最后一个生效。
- 命令的返回值决定了该次健康检查的成功与否：0：成功；1：失败；2：保留，不要使用这个值。
```
FROM nginx
RUN apt-get update && apt-get install -y curl && rm -rf /var/lib/apt/lists/*
HEALTHCHECK --interval=5s --timeout=3s \
CMD curl -fs http://localhost/ || exit 1
docker inspect --format '{{json .State.Health}}' web | python -m json.tool
```
##### ONBUILD 为他人做嫁衣裳
- ONBUILD <其它指令>
- ONBUILD 是一个特殊的指令，它后面跟的是其它指令，比如 RUN, COPY 等，而这些指令，在当前镜像构建时并不会被执行。只有当以当前镜像为基础镜像，去构建下一级镜像的时候才会被执行。
#### 多阶段构建

#### docker image cache 失效原则
1. 如果命令有--no-cache=true 则不使用缓存
2. 对比命令字符是否相同，如果不相同则缓存失效
3. 对于ADD/COPY命令会对比文件的checksum是否相同，不同则缓存失效
4. 对于apt-get -y update等指令不会去比较下载的文件，只会按字符串比较命令 同1
5. 如果前步骤缓存失效，后续步骤缓存都失效

#### 查看docker镜像的构建过程
```
docker history <image-name>
```
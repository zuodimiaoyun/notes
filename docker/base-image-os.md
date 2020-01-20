#### Alpine
- musl libc 而非 glibc
- 需安装：telnet(busybox-extras)，curl，vim
- 官方包索引站：https://pkgs.alpinelinux.org/packages
#### Debian
- stretch、buster
- 需安装：wget curl vim telnet net-tools procps file gnupg
##### Ubuntu
- 需安装：wget,curl,vim,telnet,net-tools

#### java server run time

```
FROM debian:latest

RUN buildDeps='wget curl vim telnet net-tools procps file' \
        && apt-get update \
	    && apt-get install -y $buildDeps \
        && echo "alias ls='ls --color=tty'" >>  ~/.bashrc \
        && echo "alias ll='ls -l'" >>  ~/.bashrc \
        && wget -q http://10.129.18.22:4567/repository/packages/software/java/server-jre-8u231-linux-x64.tar.gz| \
        && mkdir -p /usr/lib/java \
        && tar -zxf /server-jre-8u231-linux-x64.tar.gz| -C /usr/lib/java \
        && rm server-jre-8u231-linux-x64.tar.gz \
        && rm -rf /var/lib/apt/lists/* \

ENV PATH=$PATH:/usr/lib/java/jdk1.8.0_231/bin

```

#### dotnet core runtime

```
FROM debian:latest
RUN buildDeps='wget curl vim telnet net-tools procps file' \
        && apt-get update \
	    && apt-get install -y $buildDeps \
        && echo "alias ls='ls --color=tty'" >>  ~/.bashrc \
        && echo "alias ll='ls -l'" >>  ~/.bashrc \
        && apt-get install -y gnupg \
        && wget -qO- https://packages.microsoft.com/keys/microsoft.asc | gpg --dearmor > microsoft.asc.gpg \
        && mv microsoft.asc.gpg /etc/apt/trusted.gpg.d/ \
        && wget -q https://packages.microsoft.com/config/debian/10/prod.list \
        && mv prod.list /etc/apt/sources.list.d/microsoft-prod.list \
        && chown root:root /etc/apt/trusted.gpg.d/microsoft.asc.gpg \
        && chown root:root /etc/apt/sources.list.d/microsoft-prod.list \
        && apt-get -y install apt-transport-https \
        && apt-get update \
        && apt-get -y install aspnetcore-runtime-2.2 \
        && rm -rf /var/lib/apt/lists/*
```

#### java sdk 
```
FROM debian:latest
RUN buildDeps='wget curl vim telnet net-tools procps file' \
        && apt-get update \
	    && apt-get install -y $buildDeps \
        && echo "alias ls='ls --color=tty'" >>  ~/.bashrc \
        && echo "alias ll='ls -l'" >>  ~/.bashrc \
        && wget -q http://10.129.18.22:4567/repository/packages/software/java/jdk-8u231-linux-x64.tar.gz \
        && mkdir -p /usr/lib/java \
        && tar -zxf /jdk-8u231-linux-x64.tar.gz -C /usr/lib/java \
        && rm jdk-8u231-linux-x64.tar.gz
        && rm -rf /var/lib/apt/lists/* \
ENV PATH=$PATH:/usr/lib/java/jdk1.8.0_231/bin
```

#### dotnet core sdk
```
FROM debian:latest
RUN buildDeps='wget curl vim telnet net-tools procps file' \
        && apt-get update \
	    && apt-get install -y $buildDeps \
        && echo "alias ls='ls --color=tty'" >>  ~/.bashrc \
        && echo "alias ll='ls -l'" >>  ~/.bashrc \
        && apt-get install -y gnupg \
        && wget -qO- https://packages.microsoft.com/keys/microsoft.asc | gpg --dearmor > microsoft.asc.gpg \
        && mv microsoft.asc.gpg /etc/apt/trusted.gpg.d/ \
        && wget -q https://packages.microsoft.com/config/debian/10/prod.list \
        && mv prod.list /etc/apt/sources.list.d/microsoft-prod.list \
        && chown root:root /etc/apt/trusted.gpg.d/microsoft.asc.gpg \
        && chown root:root /etc/apt/sources.list.d/microsoft-prod.list \
        && apt-get -y install apt-transport-https \
        && apt-get update \
        && apt-get -y install dotnet-sdk-2.2 \
        && rm -rf /var/lib/apt/lists/*
```
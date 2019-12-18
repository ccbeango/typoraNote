# Docker入门记录

## Docker基本概念

## Docker的命令

记录Docker一些常用的基本命令，便于个人快速查找使用。

具体选项可通过`docker COMMAND --help`命令查看。

### 获取镜像

```shell
$ docker pull [选项] [Docker Registry 地址[:端口号]/]仓库名[:标签]
```

镜像名称格式：

* Docker镜像仓库地址格式一般是`<域名/IP>[:端口号]`。默认地址是Docker Hub的地址。
* 仓库名：分为两段，即`<用户名>/<软件名>`。对于Docker Hub用户，如果不给出用户名，默认为`library`，也就是官方镜像。

作为开发我一般在使用此命令时，只会指定软件名，需要指定版本时，加上标签进行版本指定；未指定标签默认会使用`lastest`。例如：

```shell
$ docker pull mysql 
Using default tag: latest
latest: Pulling from library/mysql
d599a449871e: Pull complete 
f287049d3170: Pull complete 
08947732a1b0: Pull complete 
96f3056887f2: Pull complete 
871f7f65f017: Pull complete 
1dd50c4b99cb: Pull complete 
5bcbdf508448: Pull complete 
a59dcbc3daa2: Pull complete 
13e6809ab808: Pull complete 
2148d51b084d: Pull complete 
93982f7293d7: Pull complete 
e736330a6d9c: Pull complete 
Digest: sha256:c93ba1bafd65888947f5cd8bd45deb7b996885ec2a16c574c530c389335e9169
Status: Downloaded newer image for mysql:latest
docker.io/library/mysql:latest
```

未指定镜像仓库地址，用户名，默认会获取Docker Hub的官方镜像。

从下载过程中可以看出，镜像是分层存储的，镜像是由多层存储所构成。下载也是一层层地去下载，并非单一文件。下载过程中给出了每一层的ID的前12位。并且下载结束后，给出该镜像完整的`sha256`的摘要，以确保下载一致性。

### 运行

```shell
$ docker run [OPTIONS] IMAGE [COMMAND] [ARG...]
```

比如运行获取的镜像：

```shell
$ docker run -it --rm mysql:latest bin/bash

root@86fc09fa6128:/# cat /etc/os-release 
PRETTY_NAME="Debian GNU/Linux 9 (stretch)"
NAME="Debian GNU/Linux"
VERSION_ID="9"
VERSION="9 (stretch)"
VERSION_CODENAME=stretch
ID=debian
HOME_URL="https://www.debian.org/"
SUPPORT_URL="https://www.debian.org/support"
BUG_REPORT_URL="https://bugs.debian.org/"
```

参数解释：

* `-i`：指的是交互式操作(interaction?)。
* `-t`：指的是终端；我们这里打算进入`bash`执行一些命令并查看返回结果，因此需要交互式终端。

* `--rm`：容器退出后随之将其删除。只是为了随便执行个命令，看看结果，只用此参数可以避免空间浪费。如果未加此参数，之后需要删除，可以使用`docker run`命令。
* `mysql:latest`：是指用的`mysql:latest`镜像为基础启动容器。
* `bin/bash`：放在镜像名之后的是命令，这里希望有个交互式shell，因此用的是`bin/bash`。这里的命令可以是你想执行的且容器支持的命令。

### 列出镜像

```shell
$ docker image ls
REPOSITORY          TAG                 IMAGE ID            CREATED             SIZE
mongo               latest              9979235fc504        5 days ago          364MB
mysql               latest              d435eee2caa5        3 weeks ago         456MB

```

列表包含了`仓库名`、`标签`、`镜像ID`、`创建时间`以及`所占用的空间`。一个镜像可以对应多个标签，镜像ID相同的对应的是同一个镜像。

#### 镜像体积

这里显示的镜像所占用的空间是解压展开的大小，准确说是展开后的各层所占用空间的总和。而Docker Hub上显示的是压缩后的体积。

另外，`docker image ls`列表中的镜像体积总和并非是所有镜像实际硬盘消耗。由于Dokcer镜像是多层存储结构，并且可以继承、复用，因此不同镜像可能会因为使用相同的基础镜像，从而拥有共同的层。由于Docker使用`Union FS???`，相同的层只需要保存一份即可，因此实际镜像硬盘所占用空间很可能要比这个列表镜像大小的总和要小的多。

可以通过以下命令来快捷查看镜像、容器、数据卷所占用的空间。

```shell
$ docker system df
```

#### 虚悬镜像

有一些特殊镜像，这个镜像既没有仓库名，也没有标签，均为`<none>`。

```shell
<none> <none> 00285df0df87 5 days ago  342 MB
```

这种情况是随着是，随着官方镜像维护，发布了新版本后，重新 `docker pull mongo:3.2 `时，`mongo:3.2` 这个镜像名被转移到了新下载的镜像身上，而旧的镜像上的这个名称则被取消，从而成为了<none> 。除了 docker pull 可能导致这种情况， docker build 也同样可以导致这种现象。由于新旧镜像同名，旧镜像名称被取消，从而出现仓库名、标签均为 <none> 的镜像。这类无标签镜像也被称为 虚悬镜像(dangling image) ，可以用下面的命令专门显示这类镜像：

```shell
$ docker image ls -f dangling=true
REPOSITORY 	TAG 	IMAGE ID 			CREATED 		SIZE
<none>     <none> 00285df0df87 	5 days ago 	342 MB
```

一般来说，虚悬镜像已经失去了存在的价值，可以随意删除的，可以使用一下命令删除：

```shell
$ docker image prune
```

#### 中间层镜像

Docker为了加速镜像构建和重复利用资源，Docker会利用中间层镜像。

默认的`docker image ls`列表中只会显示顶层镜像，如果希望显示包括中间层镜像再内的所有镜像的话，需要加`-a`参数。

```shell
$ docker image ls -a
```

这样会看到很多无标签的镜像，于之前的虚悬镜像不同，这些无标签的镜像很多都是中间层镜像，是其他镜像所依赖的镜像。如果删除，可能会导致上层镜像因为依赖丢失而出错。删除依赖它们的镜像后，这些依赖的中间层镜像也会被连带删除。

#### 列出部分镜像


#Docker命令

* docker attach命令-登录一个已经在执行的容器
```
#Usage: docker attach [OPTIONS] CONTAINER
#--no-stdin=false 默认参数false 不要附加stdin（输入）
#--sig-proxy=true 默认true Proxify所有接收信号流程(即使在non-tty模式).
$ ID=$(sudo docker run -d ubuntu /usr/bin/top -b)
$ sudo docker attach $ID
```

* docker restart命令-重启一个容器或多个容器
```
#Usage: docker restart [OPTIONS] CONTAINER [CONTAINER...]
#-t, --time=10 Seconds to wait for stop before killing the container
```

* docker build 命令-建立一个新的image
```
http://www.simapple.com/319.html
Usage: docker build [OPTIONS] PATH | URL | -
Build a new image from the source code at PATH

  -c, --cpu-shares=0    设置容器CPU权重，在CPU共享场景使用  
  --cpuset-cpus=        CPUs in which to allow execution (0-3, 0,1)
  -f, --file=           Name of the Dockerfile (Default is 'PATH/Dockerfile')
  --force-rm=false      Always remove intermediate containers
  --help=false          Print usage
  -m, --memory=         指定容器的内存上限
  --memory-swap=        Total memory (memory + swap), '-1' to disable swap
  --no-cache=false      构建过程不使用缓存
  --pull=false          Always attempt to pull a newer version of the image
  -q, --quiet=false     抑制生成过程中的输出Suppress the verbose output generated by the containers
  --rm=true             构建成功后移除中间容器；
  -t, --tag=            为创建的容器打上标签
```

* docker commit命令-提交一个新的image
```
Usage: docker commit [OPTIONS] CONTAINER [REPOSITORY[:TAG]]
Create a new image from a container's changes

  -a, --author=       Author (e.g., "John Hannibal Smith <hannibal@a-team.com>")
  -c, --change=[]     Apply Dockerfile instruction to the created image
  --help=false        Print usage
  -m, --message=      Commit message
  -p, --pause=true    Pause container during commit
```

* docker cp命令-将容器中的文件拷贝到主机上
```
Usage: docker cp CONTAINER:PATH HOSTPATH
```

* docker daemon命令-docker运行可指定项详解
* docker diff命令-较一个容器不同版本提交的文件差异
* docker events命令-获取sever中的实时事件
```
从服务器拉取个人动态，可选择时间区间。
docker events [options]
docker events --since="20141020"
docker events --until="20120310"
```

* docker export命令-导出一个容器
```
将指定的容器保存成 tar 归档文件， docker import 的逆操作。导出后导入（exported-imported)）的容器会丢失所有的提交历史，无法回滚。
docker export <container>
docker export nginx-01 > export.tar
```

* docker history命令-显示一个image的历史
```
--no-trunc 显示完整的提交记录；
-q 仅列出提交记录ID。
```

* docker images命令-列出image
```
-a 列出所有镜像（含过程镜像）；
-f 过滤镜像，如： -f ['dangling=true'] 只列出满足 dangling=true 条件的镜像；
--no-trunc 可显示完整的镜像ID；
-q 仅列出镜像ID。
--tree 以树状结构列出镜像的所有提交历史。
```

* docker import命令-导入已有的image,本地已有的image或远程的 支持压缩包 (.tar, .tar.gz, .tgz, .bzip, .tar.xz, or .txz)
```
从归档文件（支持远程文件）创建一个镜像， export 的逆操作，可为导入镜像打上标签。导出后导入（exported-imported)）的容器会丢失所有的提交历史，无法回滚。
导入远程的包:
$ sudo docker import http://example.com/exampleimage.tgz
导入本地文件
$ cat exampleimage.tgz | sudo docker import- exampleimagelocal:new
导入本地目录:
$ sudo tar -c .| sudo docker import- exampleimagedir
```

* docker info命令-展示docker的信息
* docker inspect命令-显示更底层的容器或image信息
```
检查镜像或者容器的参数，默认返回 JSON 格式。
docker instpect nginx:latest
docker inspect nginx-container
-f 指定返回值的模板文件。
```

* docker kill命令-杀死docker进程
```
-s "KILL" 自定义发送至容器的信号。
```

* docker load命令-加载image
```
从 tar 镜像归档中载入镜像， docker save 的逆操作。保存后再加载（saved-loaded）的镜像不会丢失提交历史和层，可以回滚。
docker load [options]
docker load < debian.tar
docker load -i "debian.tar"
-i "debian.tar" 指定载入的镜像归档。
```

* docker login命令-登录docker注册服务器
```
Usage: docker login [OPTIONS] [SERVER]
登陆你在docker服务器注册的账号 index.docker.io
 -e="": email
 -p="": password
 -u="": username

 如果你想登陆你的私有的仓库你可以添加的你服务器名称
 docker login localhost:8080
```

* docker logout [server] 运行后从指定服务器登出，默认为官方服务器。
* docker logs命令-获取容器的日志
```
docker logs [options] <container>
docker logs -f -t --tail="10" insane_babbage
获取容器运行时的输出日志。
-f 跟踪容器日志的最近更新；
-t 显示容器日志的时间戳；
--tail="10" 仅列出最新10条容器日志。
```

* docker pause|unpause 命令-暂停|取消暂停 容器中的所有进程
```
docker pause|unpause <container>
```

* docker port命令 端口转发
* docker ps命令-列出所有容器
```
-a 查看所有的镜像 默认情况下只查看正在运行的镜像
--before="nginx" 列出在某一容器之前创建的容器，接受容器名称和ID作为参数；
--since="nginx" 列出在某一容器之后创建的容器，接受容器名称和ID作为参数；
-f [exited=<int>] 列出满足 exited=<int> 条件的容器；
-l 仅列出最新创建的一个容器；
--no-trunc 显示完整的容器ID；
-n=4 列出最近创建的4个容器；
-q 仅列出容器ID；
-s 显示容器大小。
```

* docker pull命令-从远端拉取一个image
```
$ docker pull registry.hub.docker.com/debian
```

* docker push命令-推送image到注册服务器
* docker rmi命令-删除image
```
-f 强行移除该镜像，即使其正被使用；
--no-prune 不移除该镜像的过程镜像，默认移除。
```

* docker rm命令-删除一个或多个容器
```
-f 强行移除该容器，即使其正在运行；
-l 移除容器间的网络连接，而非容器本身；
-v 移除与容器关联的空间。
```

* docker search命令-搜索images
```
--automated 只列出 automated build 类型的镜像；
--no-trunc 可显示完整的镜像描述；
-s 40 列出收藏数不小于40的镜像。
```

* docker restart命令-重启一个容器或多个容器
* docker start命令-启动一个容器
* docker stop命令-停止一个容器
```
-a 待完成
-i 启动一个容器并进入交互模式；
-t 10 停止或者重启容器的超时时间（秒），超时后系统将杀死进程。
```

* docker run命令-运行一个新的容器

```
docker run [options] <image> [command] [arg...]
 -a, --attach=[]            指定标准输入输出内容类型，可选 STDIN/ STDOUT / STDERR 三项；附加标准输入、输出或者错误输出
 --add-host=[]              Add a custom host-to-IP mapping (host:ip)
 -c, --cpu-shares=0         共享CPU格式（相对重要）
 --cap-add=[]               Add Linux capabilities 添加权限
 --cap-drop=[]              Drop Linux capabilities 删除权限
 --cgroup-parent=           Optional parent cgroup for the container
 --cidfile=                 将容器的ID标识写入文件
 --cpuset-cpus=             --cpuset="0-2" or --cpuset="0,1,2" 绑定容器到指定CPU运行；
 -d, --detach=false         分离模式，在后台运行容器，并且打印出容器ID
 --device=[]                添加主机设备给容器，相当于设备直通
 --dns=[]                   --dns 8.8.8.8 指定容器使用的DNS服务器，默认和宿主一致；
 --dns-search=[]            example.com 指定容器DNS搜索域名，默认和宿主一致
 -e, --env=[]               username="ritchie" 设置环境变量
 --entrypoint=              覆盖镜像设置默认的入口点
 --env-file=[]              从指定文件读入环境变量；
 --expose=[]                让你主机没有开放的端口
 -h, --hostname=            指定容器的hostname；容器的主机名称
 --help=false               Print usage
 -i, --interactive=false    保持输入流开放即使没有附加输入流 以交互模式运行容器，通常与 -t 同时使用；
 --ipc=                     IPC namespace to use
 -l, --label=[]             Set meta data on a container
 --label-file=[]            Read in a line delimited file of labels
 --link=[]                  连接到另一个容器 (name:alias)
 --log-driver=              Logging driver for container
 --lxc-conf=[]              添加自定义 -lxc-conf="lxc.cgroup.cpuset.cpus = 0,1"
 -m, --memory=              内存限制 (格式: <number><optional unit>, unit单位 = b, k, m or g)
 --mac-address=             Container MAC address (e.g. 92:d0:c6:0a:29:33)
 --memory-swap=             Total memory (memory + swap), '-1' to disable swap
 --name=                    分配容器的名称，如果没有指定就会随机生成一个
 --net=bridge               指定容器的网络连接类型，支持 bridge / host / none container:<name|id> 四种类型；
 -P, --publish-all=false    Publish all exposed ports to random ports
 -p, --publish=[]           匹配镜像内的网络端口号
 --pid=                     PID namespace to use
 --privileged=false         给容器扩展的权限（容器已经有了所有主机能做的事情，这个参数的存在允许特殊的用例，像docker内运行docker）
 --read-only=false          Mount the container's root filesystem as read only
 --restart=no               Restart policy to apply when a container exits
 --rm=false                 当容器退出时自动删除容器 (不能跟 -d一起使用)
 --security-opt=[]          Security Options
 --sig-proxy=true           代理接收所有进程信号 (even in non-tty mode)
 -t, --tty=false            为容器重新分配一个伪输入终端，通常与 -i 同时使用；
 -u, --user=                用户名或者ID (format: <name|uid>[:<group|gid>])
 --ulimit=[]                Ulimit options
 -v, --volume=[]            创建一个挂载绑定：[host-dir]:[container-dir]:[rw|ro]. 如果容器目录丢失，docker会创建一个新的卷 （给容器挂载存储卷，挂载到容器的某个目录 ）
 --volumes-from=[]          挂载容器所有的卷（给容器挂载其他容器上的卷，挂载到容器的某个目录）
 -w, --workdir=             工作目录内的容器 指定容器的工作目录
```
* docker save命令-打包image
```
将指定镜像保存成 tar 归档文件， docker load 的逆操作。保存后再加载（saved-loaded）的镜像不会丢失提交历史和层，可以回滚。
docker save -i "debian.tar"
docker save > "debian.tar"
-o "debian.tar" 指定保存的镜像归档。
```
* docker tag命令-为image打标签
```
docker tag [options] <image>[:tag] [repository/][username/]name[:tag]
-f 覆盖已有标记。
```
* docker top命令-显示容器中的运行的进程
```
查看一个正在运行容器进程，支持 ps 命令参数。
docker top <running_container> [ps options]
```

* docker version命令-显示版本信息
* docker wait命令-阻塞运行直到容器停止

Header 1
========
Header 2
-------
# Header 1 #
## Header 2 ##
###### Header 6
1.  Foo
2.  Bar

*   A list item.

    With multiple paragraphs.
*   Bar

*   Abacus
    * answer
*   Bubbles
    1.  bunk
    2.  bupkis
        * BELITTLER
    3. burper
*   Cunning

---
* * *
- - - -


docker search xxx   查找镜像

docker search xx   搜索镜像

docker pull centos   下载镜像

docker images查看本地已有那些镜像

docker run q1101151 mkdir /home/test 

docker run learn/tutorial yum install -y ping 

docker run -i -t centos /bin/bash   进入镜像

#docker ps －a 或者 -l 获得 CONTAINER id

# 列出当前所有正在运行的container
$docker ps
# 列出所有的container
$docker ps -a
# 列出最近一次启动的container
$docker ps -l
#保持镜像
docker commit <CONTAINER ID> q1101151
#提交镜像
docker push q1101151
杀死所有正在运行的容器
docker kill $(docker ps -a -q)  
docker rm $(docker ps -a -q)   删除所有已经停止的容器
docker rmi $(docker images -q -f dangling=true) 删除所有未打 dangling 标签的镜像
docker rmi $(docker images -q)删除所有镜像
docker run learn/tutorial echo "hello word"#在docker容器中运行hello world!

为这些命令创建别名

# ~/.bash_aliases
# 杀死所有正在运行的容器.
alias dockerkill='docker kill $(docker ps -a -q)'
# 删除所有已经停止的容器.
alias dockercleanc='docker rm $(docker ps -a -q)'
# 删除所有未打标签的镜像.
alias dockercleani='docker rmi $(docker images -q -f dangling=true)'
# 删除所有已经停止的容器和未打标签的镜像.
alias dockerclean='dockercleanc || true && dockercleani'

docker login -u<用户名>-p<密码>-e<Email>
docker login login succeeded 登陆成功

# 删除所有容器$docker rm `docker ps -a -q`

# 删除单个容器; -f, --force=false; -l, --link=false Remove the specified link and not the underlying container; -v, --volumes=false Remove the volumes associated to the container$docker rm Name/ID# 停止、启动、杀死一个容器$docker stop Name/ID$docker start Name/ID$docker kill Name/ID# 从一个容器中取日志; -f, --follow=false Follow log output; -t, --timestamps=false Show timestamps$docker logs Name/ID# 列出一个容器里面被改变的文件或者目录，list列表会显示出三种事件，A 增加的，D 删除的，C 被改变的$docker diff Name/ID# 显示一个运行的容器里面的进程信息$docker top Name/ID# 从容器里面拷贝文件/目录到本地一个路径$docker cp Name:/container_path to_path
$docker cp ID:/container_path to_path

# 重启一个正在运行的容器; -t, --time=10 Number of seconds to try to stop for before killing the container, Default=10$docker restart Name/ID# 附加到一个运行的容器上面; --no-stdin=false Do not attach stdin; --sig-proxy=true Proxify all received signal to the process$docker attach ID

# 保存镜像到一个tar包; -o, --output="" Write to an file$docker save image_name -o file_path
# 加载一个tar包格式的镜像; -i, --input="" Read from a tar archive file$docker load -i file_path

# 机器a$docker save image_name > /home/save.tar
# 使用scp将save.tar拷到机器b上，然后：$docker load < /home/save.tar

#set PATH=%PATH%;\Git\bin

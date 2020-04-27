yum源管理
cd /etc/yum.repos.d
wget http://mirrors.163.com/.help/CentOS7-Base-163.repo

安装

sudo yum install -y yum-utils \
  device-mapper-persistent-data \
  lvm2

sudo yum install docker-ce docker-ce-cli containerd.io




docker run -i -t <image_name/continar_id> /bin/bash  启动容器并启动bash（交互方式）

docker run -d -it  image_name   启动容器以后台方式运行(更通用的方式）

docker ps   列出当前所有正在运行的container

docker ps -a  列出所有的container

docker ps -l   列出最近一次启动的container

docker images  列出本地所有的镜像

docker rmi imagesID   删除指定的镜像id

docker rm CONTAINER ID   删除指定的CONTAINER id

docker diff 镜像名    查看容器的修改部分

docker kill CONTAINER ID   杀掉正在运行的容器

docker logs 容器ID/name   可以查看到容器主程序的输出

docker pull image_name    下载image

docker push image_name   发布docker镜像

docker version   查看docker版本

docker info   查看docker系统的信息

docker inspect 容器的id 可以查看更详细的关于某一个容器的信息

docker run -d  image-name   后台运行镜像
启动镜像：
docker run -itd --name=dfc-nacos -p 8848:8848  -v /Users/yumeng/DockerShares/nacos/:/usr/local/share/ dfc-nacos:1.0

docker search 镜像名    查找公共的可用镜像

docker stop 容器名/容器 ID      终止运行的容器

docker restart 容器名/容器 ID    重启容器 
docker restart 6943f8d25230d7bdfedebb16cf02dd811622fa1aea482cee9decbd32a3fbbdc0

docker commit  提交，创建个新镜像

docker build [OPTIONS] PATH | URL | -   利用 Dockerfile 创建新镜像

---
title: docker CDH 搭建伪分布式集群
categories: 操作教程
---

1. 安装docker.
1. 导入CDH镜像  
   1. 先从官网下载后缀为.tar.gz的镜像，然后解压.  
        - sudo wget https://downloads.cloudera.com/demo_vm/docker/cloudera-quickstart-vm-5.13.0-0-beta-docker.tar.gz
        - tar -xzvf cloudera-quickstart-vm-5.13.0-0-beta-docker.tar.gz
   2. 导入镜像.  
        - docker import cloudera-quickstart-vm-5.13.0-0-beta-docker.tar
1. 查看镜像ID.  
    docker images 保存镜像ID (IMAGE ID):34c21fe31da2 
1. 创建CDH的容器.  
    docker run --name=cdh --hostname=quickstart.cloudera --privileged=true -d -i -p 8020:8020 -p 8022:8022 -p 7180:7180 -p 21050:21050 -p 50070:50070 -p 50075:50075 -p 50010:50010 -p 50020:50020 -p 8890:8890 -p 60010:60010 -p 10002:10002 -p 25010:25010 -p 25020:25020 -p 18088:18088 -p 8088:8088 -p 19888:19888 -p 7187:7187 -p 11000:11000 镜像ID /bin/bash -c '/usr/bin/docker-quickstart && /home/cloudera/cloudera-manager --express && service ntpd start'  
    (这种方式启动的弊端是 -t参数 会启动一个shell界面，从而阻塞了后面启动cloudera-manager和ntpd服务。可以改成-d 启动时间有点长，需要手动连接shell.docker )
1. 当创建完成后,第二次启动.  
    docker start cdh
1. 删除容器.  
    docker rm [容器ID] (ID 可以通过 docker ps -n 5(自己设置) 查看）.
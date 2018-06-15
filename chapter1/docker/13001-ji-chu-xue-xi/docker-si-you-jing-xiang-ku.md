**1、安装镜像库**
    
    #下载镜像
    docker pull registry
    #创建映射数据目录 
    mkdir -pv /opt/data/registry
    
    #启动是由镜像库
    docker run -d -p 5000:5000 -v /opt/data/registry/:/var/lib/registry  --name docker-hub registry

**2、镜像管理**
- 推送

    
    #打tag    
    docker tag centos:latest 127.0.0.1:5000/centos:latest
    #上传镜像
    docker push 127.0.0.1:5000/centos:latest

- 下载
    
    
    #删除tag
    docker image rm 127.0.0.1:5000/ubuntu:latest
    #从镜像库下载镜像
    docker pull 127.0.0.1:5000/ubuntu:latest
- 查看镜像


    curl 127.0.0.1:5000/v2/_catalog

**3、指定docker 默认镜像源**


    vim /etc/docker/daemon.json
    {
      "registry-mirror": [
        "https://registry.docker-cn.com"
      ],
      "insecure-registries": [
        "127.0.0.1:5000"
      ]
    }
    
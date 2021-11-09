# docker-compose

### 概述

```
docker compose 是docker 官方的开源项目，负责对docker 容器集群的快速编排

在日常工作中，经常会碰到需要多个容器相互配合来完成某项任务的情况
例如：
	要实现一个web项目，除了web项目本身，往往还需要再加上后端的数据库，甚至还包括负载均衡容器等
	
	
docker compose 允许用户通过一个单独的docker-compose.yml模版文件（YAML格式）来定义一组相关联的应用容器作为一个项目
```

compose 中重要的两个概念：

- 服务（service）：一个应用的容器，实际上可以包括若干运行相同镜像的容器实例
- 项目（project）：由一组关联的应用容器组成的一个完整的业务单元在 docker-compose.yml文件中定义

### 一、安装docker-compose

#### 1、二进制安装

```shell
#下载
curl -L "https://github.com/docker/compose/releases/download/1.29.2/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose

#配置执行权限
chmod +x /usr/local/bin/docker-compose

#配置软连接
ln -s /usr/local/bin/docker-compose /usr/bin/docker-compose
```



#### 2、pip安装

```shell
#安装
pip install docker-compose

#升级
docker-compose migrate-to-labels

#卸载
pip uninstall docker-compose

```

### 二、docker-compose 简单示例

#### 1、创建项目

- 初始化目录

```shell
mkdir -pv demo-docker-compose && cd demo-docker-compose
```

- 创建示例代码

```python
import time

import redis
from flask import Flask

app = Flask(__name__)
cache = redis.Redis(host='redis', port=6379)

def get_hit_count():
    retries = 5
    while True:
        try:
            return cache.incr('hits')
        except redis.exceptions.ConnectionError as exc:
            if retries == 0:
                raise exc
            retries -= 1
            time.sleep(0.5)

@app.route('/')
def hello():
    count = get_hit_count()
    return 'Hello World! I have been seen {} times.\n'.format(count)
```

- 编辑requirements.txt

```shell
vim requirements.txt
flask
redis
```

#### 2、创建Dockerfile

```dockerfile
FROM python:3.7-alpine
WORKDIR /code
ENV FLASK_APP=app.py
ENV FLASK_RUN_HOST=0.0.0.0
ADD . /code
RUN pip install -r requirements.txt
CMD ["flask","run"]
```

#### 3、配置compose

```yaml
version: "3.9"
services:
  web:
    build: .
    ports:
      - "5000:5000"
      
  redis:
    image: "redis"
```



#### 4、测试验证

```shell
#启动docker-compose up -d#测试访问curl http://localhost:5000Hello World! I have been seen 1 times.
```

### 三、docker-compose命令使用

#### 1、命令帮助

```shell
# docker-composeDefine and run multi-container applications with Docker.Usage:  docker-compose [-f <arg>...] [--profile <name>...] [options] [--] [COMMAND] [ARGS...]  docker-compose -h|--helpOptions:  -f, --file FILE             Specify an alternate compose file                              (default: docker-compose.yml)  -p, --project-name NAME     Specify an alternate project name                              (default: directory name)  --profile NAME              Specify a profile to enable  -c, --context NAME          Specify a context name  --verbose                   Show more output  --log-level LEVEL           Set log level (DEBUG, INFO, WARNING, ERROR, CRITICAL)  --ansi (never|always|auto)  Control when to print ANSI control characters  --no-ansi                   Do not print ANSI control characters (DEPRECATED)  -v, --version               Print version and exit  -H, --host HOST             Daemon socket to connect to  --tls                       Use TLS; implied by --tlsverify  --tlscacert CA_PATH         Trust certs signed only by this CA  --tlscert CLIENT_CERT_PATH  Path to TLS certificate file  --tlskey TLS_KEY_PATH       Path to TLS key file  --tlsverify                 Use TLS and verify the remote  --skip-hostname-check       Don't check the daemon's hostname against the                              name specified in the client certificate  --project-directory PATH    Specify an alternate working directory                              (default: the path of the Compose file)  --compatibility             If set, Compose will attempt to convert keys                              in v3 files to their non-Swarm equivalent (DEPRECATED)  --env-file PATH             Specify an alternate environment fileCommands:  build              Build or rebuild services  config             Validate and view the Compose file  create             Create services  down               Stop and remove resources  events             Receive real time events from containers  exec               Execute a command in a running container  help               Get help on a command  images             List images  kill               Kill containers  logs               View output from containers  pause              Pause services  port               Print the public port for a port binding  ps                 List containers  pull               Pull service images  push               Push service images  restart            Restart services  rm                 Remove stopped containers  run                Run a one-off command  scale              Set number of containers for a service  start              Start services  stop               Stop services  top                Display the running processes  unpause            Unpause services  up                 Create and start containers  version            Show version information and quit
```

#### 2、参数解释

| 参数    | 解释                                                         | 备注                           |
| ------- | ------------------------------------------------------------ | ------------------------------ |
| build   | 构建项目中的容器服务                                         | 一般在更新代码后，重新构建服务 |
| config  | 验证compose文件格式是否正确，若正确则显示配置，若格式错误显示错误原因 |                                |
| create  | 为服务创建容器                                               |                                |
| down    | 停止up命令所启动的容器，并移除网络                           |                                |
| events  | 查看事件                                                     |                                |
| exec    | 进入指定的容器                                               |                                |
| help    | 查看帮助                                                     |                                |
| images  | 列出compose文件中包含的镜像                                  |                                |
| kill    | 通过SIGKILL信号来强制停止服务容器                            |                                |
| logs    | 查看日志                                                     |                                |
| pause   | 暂停容器                                                     |                                |
| port    | 打印容器端口映射的公共端口                                   |                                |
| ps      | 列出项目目前的容器                                           |                                |
| pull    | 拉去服务依赖的镜像                                           |                                |
| push    | 推送服务依赖的镜像到Docker镜像仓库                           |                                |
| restart | 重启项目中的服务                                             |                                |
| rm      | 删除所有（停止状态）服务                                     | 建议先执行dock er-compose stop |
| run     | 在指定服务上运行命令                                         |                                |
| scale   | 指定容器运行的数量                                           |                                |
| start   | 启动已经存在的服务容器                                       |                                |
| stop    | 停止处于运行状态的容器，但不删除                             |                                |
| top     | 查看各个服务容器内运行的精彩                                 |                                |
| unpause | 回复处于暂停状态的容器                                       |                                |
| up      | 创建服务、启动服务、并关联服务相关的容器等等                 |                                |
| version | 打印版本信息                                                 |                                |

#### 3、常用命令

```

```



### 四、docker-compose模版文件

#### build

指定`dockerfile`所在文件夹路径（可以是绝对路径），compose会利用它自动构建这个镜像，然后使用这个镜像

```yaml
version: "3.9"services:	webapp:		build: .
```

#### cap_add,cap_drop

指定容器的内核能力分配



#### command

覆盖容器启动后默认执行的命令

```yaml
version: "3.9"services:	webapp:		image: nginx:1.7.9		command: echo "hello world"
```

#### configs

略



#### cgroup_parent

指定父`cgroup`组，意味着将继承该组的资源限制



#### container_name

指定容器名称，默认将会使用 `项目名_服务名称_序号` 的格式

```yaml
version: "3.9"services:	webapp:	  container_name: "my_nginx"		image: nginx:1.7.9		command: echo "hello world"
```

#### deploy

swarm mode 场景使用



#### devices

指定设备映射关系



#### depends_on

解决容器依赖、启动先后的问题，例如：先启动redis、db再启动web

```yaml
version: "3.9"services:  web:    image: php    depends_on:      - redis      - db    redis:    image: redis    db:    image: mysql
```

#### dns

自定义`dns`服务器

```yaml
version: "3.9"services:  web:     image: nginx    command: "ping -c 1 www.baidu.com"    dns: 202.106.0.20
```

#### dns_search

指定dns 搜索域

#### tmpfs

挂载一个tmpfs文件系统到容器



#### env_file

指定env_file环境文件

```
env_file:  - .env  - .env2  - .env3#如果包含多个环境变量，冲突以后面为准
```

#### environment

指定环境变量

```yaml
environment：  ENV: test  environment:  - ENV=test
```

#### expose

声明端口，但是不映射到宿主机，只被链接的服务访问

```yaml
expose:  - "80"  - "443"
```

#### external_links

略



#### extra_hosts

类似Docker 中的 --add-host 参数，指定额外的hosts名称映射信息

```yaml
extra_hosts:  - "www.baidu.com:1.1.1.1"  - "www.taobao.com:2.2.2.2"
```

#### healthcheck

健康检查

```yaml
healthcheck:  test: ["CMD","curl","-f","http://localhost"]  interval: 1m30s  timeout: 10s  retries:3
```

#### image

指定镜像地址

```yaml
image: nginximage: nginx:1.7.9image: http://image.test.com/nginx:1.7.9
```

#### labels

为容器添加Docker元数据（metadata）信息，例如可以为容器添加辅助说明信息

```yaml
labels:  com.startupteam.description: "webapp for a startup team"  com.startupteam.department: "devops department"  com.startupteam.release: "rc3 for v1.0"
```

#### links

略

#### logging

配置日志选项

#### network_mode

设置网络模式

```yaml
network_mode: "bridge"network_mode: "host"network_mode: "none"network_mode: "service:[service name]"network_mode: "container:[container name/id]"
```

#### networks

配置容器连接的网络

```yaml
version: "3"services:  some-service:    networks:     - some-network     - other-networknetworks:  some-network:  other-network:
```

#### pid

跟主机系统共享进程命名空间，打开该选项的容器之间，以及容器和宿主机之间可以通过进程id来互相访问和操作

```
pid: "host"
```



#### ports

端口映射

```yaml
ports: - "3000"    #只指定容器端口，宿主机会随机选择端口 - "3000:80" #容器80 端口映射到宿主机3000 - "1.1.1.1:3000:80" #映射容器80端口到宿主机指定ip+端口上
```



#### secrets

存储敏感数据

#### security_opt

指定容器模版标签（label） 机制的默认属性（用户、角色、类型、级别等）

#### stop_signal

设置另一个信号来停止容器，默认情况下使用SIGTERM 停止容器

```yaml
stop_signal: SIGUSR1
```

#### sysctls

配置容器设置内核参数

```yaml
sysctls:  net.core.somaxconn: 1024  net.ipv4.tcp_syncookies: 0sysctls:  - net.core.somaxconn=1024  - net.ipv4.tcp_syncookies=0
```



#### ulimits

指定容器的文件句柄

```yaml
ulimits:  nproc: 65535  nofile:    soft: 65535    hard: 65535
```

#### volumes

数据卷映射

```yaml
vloumes:  - /var/lib/mysql  #  - test:/tmp/test  #映射相对路径指定权限  - ~/test:/tmp/test:ro #映射指定权限
```

数据卷名称 映射

```yaml
version: "3.9"services:  nginx:    image: nginx    ports:      - "80:80"      - "443:443"    volumes:      - mydata:/usr/share/nginx/html      volumes:  mydata:
```

#### 其他指令

`domainname, entrypoint, hostname, ipc, mac_address, privileged, read_only, shm_size, restart, stdin_open, tty, user, working_dir` 等指令，基本跟 `docker run` 中对应参数的功能一致。

```yaml
#指定服务容器启动后执行的入口文件。entrypoint: /code/entrypoint.sh#指定容器中运行应用的用户名。user: nginx#指定容器中工作目录。working_dir: /code#指定容器中搜索域名、主机名、mac 地址等。domainname: your_website.comhostname: testmac_address: 08-00-27-00-0C-0A#允许容器中运行一些特权命令。privileged: true#指定容器退出后的重启策略为始终重启。该命令对保持服务始终运行十分有效，在生产环境中推荐配置为 always 或者 unless-stopped。restart: always#以只读模式挂载容器的 root 文件系统，意味着不能对容器内容进行修改。read_only: true#打开标准输入，可以接受外部输入。stdin_open: true#模拟一个伪终端。tty: true
```



#### 1、环境变量使用

```shell
#配置compose文件vim env.ymlversion: "3.9"services:  nginx:    image: "nginx:${NGINX_VERSION}"#配置环境配置vim .envNGINX_VERSION=1.7.9#启动composedocker-compose -f env.yml#验证结果docker-compose  run nginx nginx -vCreating env_nginx_run ... donenginx version: nginx/1.7.9
```

> 注意： 
>
> ​       默认环境文件是 .env
>
> ​       可以配置多个环境变量
>
> ​		例如：
>
> ​					开发：  .env.dev.  预发布： .env.pre.  生产环境默认用 .env
>
> ​		通过 docker-compose  --env-file  指定环境启动



### 五、示例

#### 1、LNMP环境

#### 2、Redis集群


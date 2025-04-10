## 查看所有容器
docker ps



---


## 一、Docker简介
Docker是一种开源的应用容器引擎，让开发者可以打包他们的应用以及依赖包到一个可移植的容器中，然后发布到任何流行的Linux机器上，也可以实现虚拟化 。

## 二、基本操作

### 启动与关闭Docker服务
- 启动Docker服务：`systemctl start docker`
- 关闭Docker服务：`systemctl stop docker`
- 重启Docker服务：`systemctl restart docker`
- 设置随系统启动自动启动Docker：`systemctl enable docker`

### 查看Docker状态
- 查看Docker运行状态：`systemctl status docker`
- 查看Docker版本信息：`docker version`
- 获取Docker详细信息：`docker info`

## 三、镜像管理

### 拉取镜像
从Docker Hub拉取镜像：
```shell
docker pull 镜像名:tag
```
如果未指定tag，默认拉取latest标签 。

### 列出本地镜像
列出所有本地镜像（包含中间映像层）：
```shell
docker images -a
```
只显示镜像ID：
```shell
docker images -q
```

### 删除镜像
删除单个镜像：
```shell
docker rmi -f 镜像ID/镜像名
```
删除多个镜像：
```shell
docker rmi -f 镜像名1:TAG 镜像名2:TAG
```
删除全部镜像：
```shell
docker rmi -f $(docker images -qa)
```

## 四、容器管理

### 创建并启动容器
创建一个新的容器并启动它：
```shell
docker run [OPTIONS] IMAGE [COMMAND] [ARG...]
```
例如，以守护进程模式启动nginx容器，并将主机的80端口映射到容器的80端口：
```shell
docker run -d -p 80:80 nginx
```

### 列出容器
查看所有容器列表（包括正在运行和已停止的）：
```shell
docker ps -a
```
格式化输出
```shell
docker ps --format "{{.ID}}\t{{.Names}}"
- `.ID`: 容器ID。
- `.Image`: 容器基于的镜像名称或ID。
- `.Command`: 启动容器时运行的命令。
- `.CreatedAt`: 容器创建的时间点。
- `.RunningFor`: 自容器启动以来经过的时间。
- `.Ports`: 容器暴露的端口及其映射情况。
- `.Status`: 容器的状态（如 running、paused、exited 等）。
- `.Size`: 容器磁盘大小。
- `.Names`: 容器名称。
- `.Labels`: 分配给容器的所有标签。
- `.Label`: 指定标签的值，例如 `{{.Label "com.docker.swarm.cpu"}}` 获取名为 `com.docker.swarm.cpu` 的标签值。
- `.Mounts`: 在该容器中挂载的卷名称 

表格
docker ps --format "table {{.ID}}\t{{.Names}}"
json
docker ps --format "{{json .}}"
列表
docker ps --format "{{.ID}}: {{.Status}}"
条件
docker ps --format "{{if eq .Status \"Up\"}}UP: {{.Names}}{{else}}DOWN: {{.Names}}{{end}}"
永久化更改默认输出
`.docker/config.json` 文件，并添加 `"psFormat"` 键值对

```
使用 `--filter` 或 `-f` 选项可以根据特定条件筛选容器。例如，仅显示状态为“exited”的容器：
```shell
docker ps -a --filter "status=exited"
```
通过添加 `--latest` 或 `-l` 标志，你可以仅获取最新创建的容器的信息
```shell
docker ps -l
```
如果想同时获得它的详细信息，可以结合 `--no-trunc` 使用以避免截断长字符串
```shell
docker ps --no-trunc
```


### 停止、重启和启动容器
停止容器：
```shell
docker stop 容器ID/容器名
```
重启容器：
```shell
docker restart 容器ID/容器名
```
启动已停止的容器：
```shell
docker start 容器ID/容器名
```

### 进入容器
进入一个正在运行的容器内部：
```shell
docker exec -it 容器ID /bin/bash
```

## 五、网络管理

### 列出所有网络
查看当前Docker中的所有网络：
```shell
docker network ls
```

### 创建和删除网络
创建一个新的网络：
```shell
docker network create <network>
```
删除一个网络：
```shell
docker network rm <network>
```

## 六、卷管理

### 列出所有卷
查看所有卷：
```shell
docker volume ls
```

### 创建和删除卷
创建一个新的卷：
```shell
docker volume create <volume>
```
删除一个卷：
```shell
docker volume rm <volume>
```

## 七、查看容器日志
### 列出日志
```
docker logs
	--details     显示更多的信息
	--follow -f  跟踪
	--since       显示自某个timestamp之后的日志,或相对时间,如42m
	--tail,-n      从日志末尾显示的行数，默认为all
	--timestamps,-t   显示时间戳
	--until        显示自某个timestamp之前的日志，或相对时间，如42m（即42分钟）

```
### 使用grep过滤
```
docker logs CONTAINER_ID | grep -10 'error' # 打印匹配行的前后10行    
docker logs CONTAINER_ID | grep -C 10 'error' # 打印匹配行的前后10行    
docker logs CONTAINER_ID | grep -A 10 -B 10 'error' # 打印匹配行的前后10行
docker logs CONTAINER_ID | grep -A 10 'error' # 打印匹配行的后10行
docker logs CONTAINER_ID | grep -B 10 'error' # 打印匹配行的前10行
```

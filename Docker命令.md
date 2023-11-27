#### Docker镜像命令

```
docker --help  查看所有的命令集帮助
docker pull image   拉取一个镜像  
docker pull --help  查看pull命令的使用规则
docker push image   推送一个镜像到仓库

docker images 查看所有的镜像
docker rmi image  删除一个镜像

docker save --help 查看保存镜像的用法，根据用法使用
docker load --help 查看加载镜像的用大，根据用法使用

```

#### Docker容器命令

```
首先在dockerhub.com官网去查看相应镜像的运行
运行命令：docker run --name containerName -p 80:80 -d nginx
命令解读：
docker run: 创建并运行一个容器
--name: 给容器取一个名字 名字必须唯一
-p: 端口映射，将宿主机端口和容器端口映射 左边为宿主机端口,右边为容器端口
-d: 后台运行容器
nginx: 镜像名称 如：nginx

docker ps 查看容器进程
docker logs mn 查看mn容器的日志 添加-f参数可以持续查看日志

进入容器内部，执行一个命令：
docker exec -it mn bash
命令解读： 
docker exec： 进入容器内部，执行一个命令
-it: 给当前进入的容器创建一个标准的输入、输出终端，允许我们与容器交互
mn:容器的名称
bash:进入容器后执行的命令，bash是一个linux终端交互命令

docker stop mn 停止mn容器进程
docker rm mn  删除mn容器 添加参数-f强制删除

```

#### Docker数据卷命令

数据卷的作用：将容器与数据分离，解耦合，方便操作容器内数据，保证数据安全

```
基本语法：
docker volume [COMMAND]
COMMAND:
ceate		创建一个volume（数据卷）
inspect		显示一个或多个Volume信息
ls			列出所有的volume
prune		删除未使用的volume
rm			删除一个或多个指定的volume

运行容器时使用-v 参数来挂载数据卷如：-v 数据挂在目录：容器目录
如数据卷不存在，运行时会自动创建
```


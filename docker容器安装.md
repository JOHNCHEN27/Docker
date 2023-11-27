# Linux基于Docker安装软件



## 1.安装Docker 18.09.0的详细步骤 

**步骤1：安装依赖库**

------

sudo yum install -y yum-utils device-mapper-persistent-data lvm2

**步骤2：添加Docker源**

------

sudo yum-config-manager --add-repo https://download.docker.com/linux/centos/docker-ce.repo

推荐下面的阿里云加速镜像

sudo yum-config-manager --add-repo http://mirrors.aliyun.com/docker-ce/linux/centos/docker-ce.repo

**步骤3：安装Docker**

------

sudo yum install docker-ce-18.09.0 docker-ce-cli-18.09.0 containerd.io

**步骤4：启动Docker**

------

sudo systemctl start docker

------

**步骤5：设置Docker开机自启**

------

**sudo systemctl enable docker**

------



## 2.在Docker中安装MySQL 8

**步骤1：拉取MySQL 8的镜像文件**

------

docker pull mysql:8

------

**步骤2：使用Docker创建MySQL容器,创建MySQL数据存储目录并挂载到容器中**

------

mkdir -p /mysql/conf

mkdir -p /mysql/data

mkdir -p /mysql/init 

------

docker run -p 3306:3306 --name mysql -v /mysql/data:/var/lib/mysql -v /mysql/conf:/etc/mysql/conf.d  -v /mysql/init:/docker-entrypoint-initdb.d -e MYSQL_ROOT_PASSWORD=root -d mysql:8

------

说明：

参数`-p 3306:3306`表示将Docker容器的3306端口映射到宿主机的3306端口。参数`--name mysql`表示给容器起个名字叫mysql，方便管理。

参数`-v /data/mysql:/var/lib/mysql`表示将宿主机的/data/mysql目录挂载到容器的/var/lib/mysql目录上，这样可以实现数据持久化。

参数**-v /etc/mysql:/etc/mysql/conf.d** 表示将宿主机的/etc/mysql目录挂在到容器的/etc/mysql/conf.d目录上

参数**-v /root/mysql/init:/docker-entrypoint-initdb.d**表示将宿主机的/root/mysql/init目录挂在到容器的初始化目录 /docker-entrypoint-initdb.d

参数`-e MYSQL_ROOT_PASSWORD=root`表示设置MySQL的root用户的密码为root。



**步骤3：进入MySQL容器执行相关操作**

------

docker exec -it mysql bash

说明：进入容器后可以在容器内使用MySQL客户端来操作MySQL数据库。

------

mysql -u root -p回车

------

输入你的密码（我的密码是:root）

------

exit

说明：退出mysql窗口

------

exit

说明：退出mysql容器

------

**Docker中MySQL卸载：**

1、停止并删除mysql容器：	

```
1、docker ps
#你将会看到类似如下输出
CONTAINER ID   IMAGE        COMMAND                  CREATED        STATUS        PORTS      NAMES
abcdef123456   mysql:latest  "docker-entrypoint.s…"   2 hours ago    Up 2 hours    3306/tcp   mysql
```

其中mysql是容器名字：使用如下命令来停止和删除容器

```
docker stop mysql
docker rm mysql
```

2、删除mysql镜像

```
docker images 查看所有镜像
#你将会看到类似如下输出：
REPOSITORY          TAG       IMAGE ID       CREATED        SIZE
mysql               latest    abcdef123456   2 weeks ago    562MB
```

mysql是镜像的名字，使用如下命令来删除mysql：

```
docker rmi mysql
#或者根据镜像id删除
docker rmi abcdef123456
```

3、删除Mysql数据卷

```
docker volume ls  查看数据卷
#你将会看到类似如下输出：
DRIVER    VOLUME NAME
local     abcdef123456
```

使用以下命令删除数据卷：

```
docker volume rm abcdef123456
```

注意：如果找不到数据卷，可以用远程连接工具找到mysql的data文件目录，删除mysql的数据即可

## 3.在Docker中安装Nacos 1.4.1

**步骤1：拉取Nacos 1.4.1的镜像文件**

------

docker pull nacos/nacos-server:1.4.1

------



**步骤2：使用Docker创建Nacos容器，在宿主机上创建Nacos数据存储目录并挂载到容器中**

------

mkdir /data/nacos

------

docker run -p 8848:8848 --name nacos -v /data/nacos:/home/nacos/nacos-server-1.4.1/nacos-logs -d nacos/nacos-server:1.4.1

------

说明：

参数`-p 8848:8848`表示将Docker容器的8848端口映射到宿主机的8848端口。

参数`--name nacos`表示给容器起个名字叫nacos，方便管理。

参数`-v /data/nacos:/home/nacos/nacos-server-1.4.1/nacos-logs`表示将宿主机的/data/nacos目录挂载到容器的/home/nacos/nacos-server-1.4.1/nacos-logs目录上，这样可以实现数据持久化。

**步骤3：设置Nacos开机自启**

------

docker update --restart=always nacos

------



## 4.在Docker中安装RabbitMQ 3.8.34

**步骤1：拉取RabbitMQ 3.8.34的镜像文件**



------

docker pull rabbitmq:3.8.34-management

------



**步骤2：使用Docker创建RabbitMQ容器**

------

mkdir /data/rabbitmq

------

docker run -p 15672:15672 -p 5672:5672 --name rabbitmq -v /data/rabbitmq:/var/lib/rabbitmq -d rabbitmq:3.8.34-management

------



说明：

参数`-p 15672:15672 -p 5672:5672`表示将Docker容器的15672和5672端口映射到宿主机的15672和5672端口。

参数`--name rabbitmq`表示给容器起个名字叫rabbitmq，方便管理。

参数`-v /data/rabbitmq:/var/lib/rabbitmq`表示将宿主机的/data/rabbitmq目录挂载到容器的/var/lib/rabbitmq目录上，这样可以实现数据持久化。



说明：

步骤3：设置RabbitMQ开机自启



------

docker update --restart=always rabbitmq

------



## 5.在Docker中安装Redis 6.2.7



**步骤1：拉取Redis 6.2.7的镜像文件**



------

docker pull redis:6.2.7

------



**步骤2：使用Docker创建Redis容器；在宿主机上创建Redis数据存储目录并挂载到容器中**



------

mkdir /data/redis

------

docker run -p 6379:6379 --name redis -v /data/redis:/data -d redis:6.2.7 redis-server --appendonly yes

------



说明：

参数`-p 6379:6379`表示将Docker容器的6379端口映射到宿主机的6379端口。

参数`--name redis`表示给容器起个名字叫redis，方便管理。

参数`-v /data/redis:/data`表示将宿主机的/data/redis目录挂载到容器的/data目录上，这样可以实现数据持久化。

参数`redis-server --appendonly yes`表示开启Redis的数据持久化功能。



**步骤3：设置Redis开机自启**



------

docker update --restart=always redis

------



## 6.在Docker中安装XXL-Job-Admin 2.3.1



**步骤1：拉取XXL-Job-Admin 2.3.1的镜像文件**



------

docker pull xuxueli/xxl-job-admin:2.3.1

------



**步骤2：使用Docker创建XXL-Job-Admin容器；在宿主机上创建XXL-Job-Admin数据存储目录并挂载到容器中**



------

mkdir /data/xxl-job-admin

------

docker run -p 8088:8088 --name xxl-job-admin -v /data/xxl-job-admin:/data/applogs -d xuxueli/xxl-job-admin:2.3.1

上面部署出错，使用下面方式部署：

```bash
docker run -e PARAMS="--spring.datasource.url=jdbc:mysql://mysql:3306/xxl_job_2.3.1?useUnicode=true&characterEncoding=UTF-8&autoReconnect=true&serverTimezone=Asia/Shanghai --spring.datasource.username=root --spring.datasource.password=root" -p 8088:8080 -v /data/xxl-job-admin:/data/applogs --name xxl-job-admin  -d --link mysql:mysql xuxueli/xxl-job-admin:2.3.1
```

------



说明：

需要指定数据库名以及相应的账号密码，如果docker中安装了mysql容器，可用容器名称代替

参数`-p 8080:8080`表示将Docker容器的8080端口映射到宿主机的8080端口。

参数`--name xxl-job-admin`表示给容器起个名字叫xxl-job-admin，方便管理。

参数`-v /data/xxl-job-admin:/data/applogs`表示将宿主机的/data/xxl-job-admin目录挂载到容器的/data/applogs目录上，这样可以实现日志持久化。



**步骤3：设置XXL-Job-Admin开机自启**



------

docker update --restart=always xxl-job-admin

------



## 7.在Docker中安装Minio RELEASE.2022-09-07



**步骤1：拉取Minio RELEASE.2022-09-07的镜像文件**



------

docker pull minio/minio

------



**步骤2：使用Docker创建Minio容器；在宿主机上创建Minio数据存储目录并挂载到容器中**



------

mkdir /data/minio

docker run -p 9000:9000 --name minio -v /data/minio:/data -d minio/minio server /data

------



说明：参数`-p 9000:9000`表示将Docker容器的9000端口映射到宿主机的9000端口。参数`--name minio`表示给容器起个名字叫minio，方便管理。参数`server /data`表示在/data目录下启动Minio服务。参数`-v /data/minio:/data`表示将宿主机的/data/minio目录挂载到容器的/data目录上，这样可以实现数据持久化。



**步骤3：设置Minio开机自启**



------

docker update --restart=always minio

------



## 8.在Docker中安装Elasticsearch 7.12.1



**步骤1：拉取Elasticsearch 7.12.1的镜像文件**



------

docker pull elasticsearch:7.12.1

------



**步骤2：使用Docker创建Elasticsearch容器；在宿主机上创建Elasticsearch数据存储目录并挂载到容器中**

------

mkdir /data/elasticsearch

------

docker run -p 9200:9200 -p 9300:9300 --name elasticsearch -v /data/elasticsearch:/usr/share/elasticsearch/data -e "discovery.type=single-node" -d elasticsearch:7.12.1

------



说明：

参数`-p 9200:9200 -p 9300:9300`表示将Docker容器的9200和9300端口映射到宿主机的9200和9300端口。

参数`--name elasticsearch`表示给容器起个名字叫elasticsearch，方便管理。

参数`-e "discovery.type=single-node"`表示设置Elasticsearch为单节点模式。

参数`-v /data/elasticsearch:/usr/share/elasticsearch/data`表示将宿主机的/data/elasticsearch目录挂载到容器的/usr/share/elasticsearch/data目录上，这样可以实现数据持久化。



**步骤4：设置Elasticsearch开机自启**



------

docker update --restart=always elasticsearch

------



## 9.在Docker中安装Kibana 7.12.1



**步骤1：拉取Kibana 7.12.1的镜像文件**



------

docker pull kibana:7.12.1

------



**步骤2：使用Docker创建Kibana容器**



------

docker run -p 5601:5601 --name kibana -d kibana:7.12.1

------



说明：参数`-p 5601:5601`表示将Docker容器的5601端口映射到宿主机的5601端口。参数`--name kibana`表示给容器起个名字叫kibana，方便管理。



**步骤3：设置Kibana开机自启**



------

docker update --restart=always kibana

------

## 10.在Docker中安装nginx 1.12.2的步骤如下：



**步骤1. 拉取nginx:1.12.2的镜像文件；创建一个数据存储目录，比如/data/nginx**



docker pull nginx:1.12.2

------

mkdir /data/nginx/conf

------

mkdir /data/nginx/logs

------

mkdir /data/nginx/html

------



**步骤2. 启动一个nginx 1.12.2的容器并将数据存储目录挂载到容器内部的/data目录中**



docker run -d --name nginx -p 80:80 -v /data/nginx/html:/usr/share/nginx/html -v /data/nginx/conf:/etc/nginx -v /data/nginx/logs:/var/log/nginx -d nginx:1.12.2



**步骤3. 设置应用开机自启**



------

docker update --restart=always nginx

------



以上步骤完成后，就可以启动nginx容器并访问ip:80了。



nginx挂载目录这里有同学可能会有疑问，平时我们打包的vue文件都生成在dist中，为什么没有挂载/data/nginx/dist目录呢？

因为data/nginx/dist 目录和 /data/nginx/html 目录的作用是一样的，用于存放Nginx服务提供的静态文件。

所以你可以选择任意一个目录挂载到 Docker 容器中，而无需挂载两个目录同时使用。

在步骤1语句4仅仅挂载了 /data/nginx/html 目录，这是因为在 Nginx 默认配置中，静态文件的根目录是 /usr/share/nginx/html，

所以我们需要挂载主机中的 /data/nginx/html 目录到容器中的 /usr/share/nginx/html 目录。

如果更喜欢使用 /data/nginx/dist 目录，则可以将上面命令中的 /data/nginx/html 目录改为 /data/nginx/dist 目录，

并将 Nginx 配置文件中的 root /usr/share/nginx/html 改为 root /usr/share/nginx/dist。



--------------------------------------------------------

以下针对部分软件不通过docker安装的方法；目前更新有mysql8、minio。

mysql8安装步骤

1在oracel官网下载对应版本mysql


2上传至对应服务器（也可以先解压再上传）




3然后通过命令rpm -ivh 安装

例如：rpm -ivh mysql-community-client-plugins-8.0.31-1.el7.x86_64.rpm

备注！！！以下为安装顺序

1.   mysql-community-common

2.  mysql-community-client-plugins

3.  mysql-community-libs

4.  mysql-community-client

5.  mysql-community-icu-data-files

6.  mysql-community-server

出现报错




执行命令下载依赖

yum install net-tools

 yum install -y perl-Module-Install.noarch

 然后进行安装

 rpm -ivh mysql-community-server-8.0.31-1.el7.x86_64.rpm

命令初始化mysql

#mysqld --initialize --console

#chown -R mysql:mysql/ var/lib/mysql/

启动mysql服务

获取mysql临时密码

#cat /var/log/mysqld.log|grep localhost




进入mysqlmysql -r root -p密码

例如：mysql -r root -pOqndr5( ih(bG

进入mysql服务

修改密码命令 alter user '用户名'@'localhost' identified by'密码' 举例：alter user 'root'@'localhost' identified by'root'

开启mysql远程连接

更改 "mysql" 数据库里的 "user" 表里的 "host" 项，从"localhost"改称"%"

use mysql;

update user set host = '%' where user = 'root';

select host, user from user;




退出mysql

exit;

#systemctl start mysqld

设置mysql开机自启

#systemctl enable mysqld

加载配置使配置生效

#systemctl daemon-reload

建立软连接

查看mysql安装路径

[root@Centos6x64 /]# whereis mysql




mysql: /usr/bin/mysql /usr/lib64/mysql /usr/share/mysql /usr/share/man/man1/mysql.1.gz

#查询运行文件所在路径

[root@Centos6x64 /]# which mysql

/usr/bin/mysql

具体用法是：ln -s 源文件 目标文件。

ln -s /usr/local/mysql/bin/mysql /usr/bin

这样我们就对/usr/bin目录下的mysql命令创建了软连接

然后可通过 /usr/bin/mysql -u用户名 -p密码 连接Mysql

-------------------------------------------------

# 下面是minio安装步骤

进入linux服务器并创建文件夹(可以自己选择一个位置，我这就直接在根目录创建了)

mkdir minio




进入创建的文件夹

cd /minio

在线下载安装包

wget https://dl.minio.io/server/minio/release/linux-amd64/minio

创建minio的log文件

touch minio.log

赋予minio文件夹777权限

chmod 777 minio

启动minio

./minio server /minio/data

控制台提示密码过于简单;下面进入配置文件设置你的账号密码(举例账号：@zxcMinio123 密码：@zxcMinio123)

vim /etc/profile

添加以下代码

set minio account

export MINIO_ROOT_USER=@zxcMinio123

export MINIO_ROOT_PASSWORD=@zxcMinio123




保存退出；

esc+:wq!

重载配置文件

source /etc/profile



后台启动minio



 nohup ./minio server /minio/data --address ":9000"  --console-address ":9001" > /minio/minio.log 2>&1 &



说明: 监听宿主机上的9000端口，并且设置控制台端口为9001

这里的61234端口可以是其他的，你上面运行命令是什么端口下面访问就写什么(情况1:本地虚拟机安装记得开放对应的这个运行端口或者你直接关闭虚拟机防火墙；情况2：云服务器安装记得安全组开放这个端口【记得设置复杂的账号密码；或者限制访问的ip为你自己电脑的ip;别问为什么：因为我的云服务器mysql由于设置传统root账户root密码被攻击过，不过我是自己玩，没有重要数据攻击了也没事，这里给大家提个醒】) 作者：穿靴子的猫bili https://www.bilibili.com/read/cv23342314/?spm_id_from=333.999.0.0&jump_opus=1 出处：bilibili
### 一、linux

1、版本：VMwareworkstation_full_12.1.0.2487.1453173744.exe，key:UU148-6VF44-M80JY-YYMQV-WF89D

2、centos7

3、安装（只是安装自己玩没有必要分区）

![1](C:\Users\spd\Desktop\book\book\01_environment\1.png)
![2](C:\Users\spd\Desktop\book\book\01_environment\2.png)
![3](C:\Users\spd\Desktop\book\book\01_environment\3.png)
![4](C:\Users\spd\Desktop\book\book\01_environment\4.png)
![5](C:\Users\spd\Desktop\book\book\01_environment\5.png)
![6](C:\Users\spd\Desktop\book\book\01_environment\6.png)
![7](C:\Users\spd\Desktop\book\book\01_environment\7.png)
![8](C:\Users\spd\Desktop\book\book\01_environment\8.png)
![9](C:\Users\spd\Desktop\book\book\01_environment\9.png)
![10](C:\Users\spd\Desktop\book\book\01_environment\10.png)
![11](C:\Users\spd\Desktop\book\book\01_environment\11.png)


![14](C:\Users\spd\Desktop\book\book\01_environment\14.png)
![15](C:\Users\spd\Desktop\book\book\01_environment\15.png)



命令行设置

```
ONBOOT=yes
```

永久关闭防火墙

```
systemctl disable firewalld.service
```

linux常用命令

```
4.	linux常用指令介绍
4.1.	文件目录操作命令
ls 显示文件和目录列表 　
    -l 列出文件的详细信息
    -a 列出当前目录所有文件，包含隐藏文件
mkdir 创建目录 　 
-p 父目录不存在情况下先生成父目录
cd 切换目录
touch 生成一个空文件   
echo 生成一个带内容文件     echo abcd>a.txt
cat、tac 显示文本文件内容
cp 复制文件或目录
rm 删除文件
	-r 同时删除该目录下的所有文件
	-f 强制删除文件或目录
     删除文件夹 rmdir 文件夹不能为空
删除文件夹   rm -rf   目录名字-r 就是向下递归，不管有多少级目录，一并删除-f 就是直接强行删除，不作任何提示的意思
mv 移动文件或目录、文件
mv  aaa bbb 将aaa改名为bbb
find 在文件系统中查找指定的文件
    -name  文件名
wc 统计文本文档的行数，字数，字符数   
grep 在指定的文本文件中查找指定的字符串
rmdir 删除空目录
tree 显示目录目录改名树 
pwd 显示当前工作目录 
ln   建立链接文件
more、less 分页显示文本文件内容 
head,tail分别显示文件开头和结尾内容
tar -zcvf a.tar.gz a.txt
tar -zxvf a.tar.gz
wget
curl

4.2.	系统管理命令
stat 显示指定文件的相关信息,比ls命令显示内容更多 
who  显示在线登录用户 
hostname 显示主机名称 
uname显示系统信息 d
top  显示当前系统中耗费资源最多的进程 
ps   显示瞬间的进程状态
du   显示指定的文件（目录）已使用的磁盘空间的总量 
df   显示文件系统磁盘空间的使用情况 
free 显示当前内存和交换空间的使用情况 
ifconfig 显示网络接口信息 
ping 测试网络的连通性 
netstat 显示网络状态信息 
man 命令帮助信息查询
clear 清屏
kill 杀死一个进程
tcpdump -s O -l -i any -w -port 8080 | string

四剑客
1、grep -C 'abc' a.txt
2、 find指令用于查找符合条件的文件
示例：
find / -name “ins*” 查找文件名称是以ins开头的文件
find / -name “ins*” –ls 
find / –user itcast –ls 查找用户itcast的文件
find / –user itcast –type d –ls 查找用户itcast的目录
find /-perm -777 –type d-ls 查找权限是777的文件

3、sed(spd/cai)增删改查(-i参数保存到原文件)
sed -n '1p' sed.txt
sed -n '1,5p' sed.txt
sed -n '/2:00/p' sed.txt
sed -n '/2:00/,/3:00/p' sed.txt

sed -n '1d' sed.txt
sed -n '1,5d' sed.txt
sed -n '/2:00/d' sed.txt
sed -n '/2:00/,/3:00/d' sed.txt

sed -n 's/^west/north/gp' sed.txt 

第一行和最后一行添加内容
sed -n 'i abc' sed.txt
sed -n '$a abc' sed.txt 
    
五、循环语句
#!/bin/bash
# while
IFS='='
while read site sitedomain
do
	weget ${sitedomain --no-check-certficate -O /dev/null -o /home/spd/hosting/log/${site}}.log
done < /home/spd/config

# for
for name in `cat config`
do
	weget ${sitedomain --no-check-certficate -O /dev/null -o /home/spd/hosting/log/${site}}.log
done

curl "http://test007.hosting-test-aws/agcconnect.link" -kvo /dev/null
curl "http://wisecontent-hostingtest.hwcloudtest.cn/index.html" -H "x-hw-dl-host:test007.hosting-test-aws.agcconnect.link" -kvo /dev/null

# 文件切割
split -b 400M 408.zip 1_
```




### 二、安装docker



```
1、卸载旧的版本
yum remove docker \
           docker-client \
           docker-client-latest \
           docker-common \
           docker-latest \
           docker-latest-logrotate \
           docker-logrotate \
           docker-engine    

2、安装需要的安装包
yum install -y yum-utils

3、设置镜像仓库
yum-config-manager \
   --add-repo \
   http://mirrors.aliyun.com/docker-ce/linux/centos/docker-ce.repo # 百度搜的阿里云镜像
   
4、更新yum包的索引
yum makecache fast  # CentOS 7使用此命令
yum makecache         # CentOS 8使用此命令

5、安装docker
yum install docker-ce docker-ce-cli containerd.io

6、启动docker
systemctl start docker
systemctl restart docker
systemctl stop docker

7、查看安装docker的版本
docker -v

8、使用阿里云镜像加速
sudo mkdir -p /etc/docker
sudo tee /etc/docker/daemon.json <<-'EOF'
{
  "registry-mirrors": ["https://xxx.mirror.aliyuncs.com"]
}
EOF
查看不回加速的配置文件
 [root@localhost ~]# cat /etc/docker/daemon.json 
{
  "registry-mirrors": ["https://xxx.mirror.aliyuncs.com"]
}

重新加载配置文件，执行命令
sudo systemctl daemon-reload
重启docker
sudo systemctl restart docker

一、Docker 进程相关命令
 启劢docker服务:
systemctl start docker 
 停止docker服务:
systemctl stop docker 
 重启docker服务:
systemctl restart docker
 查看docker服务状态:
systemctl status docker 
 设置开机启劢docker服务:
systemctl enable docker
 
二、 Docker 镜像相关命令
 查看镜像: 查看本地所有的镜像
docker images
docker images –q # 查看所用镜像的id
 搜索镜像:从网络中查找需要的镜像
docker search 镜像名称
 拉取镜像:从Docker仓库下载镜像到本地，镜像名称格式为 名称:版本号，如果版本号丌挃定则是最新的版本。
 如果丌知道镜像版本，可以去docker hub 搜索对应镜像查看。
docker pull 镜像名称
 删除镜像: 删除本地镜像
docker rmi 镜像id # 删除指定本地镜像
docker rmi `docker images -q` # 删除所有本地镜像

三、Docker 容器相关命令
 查看容器
docker ps # 查看正在运行的容器
docker ps –a # 查看所有容器
 创建并启劢容器
docker run 参数
参数说明：
• -i：保持容器运行。通常不 -t 同时使用。加入it这两个参数后，容器创建后自劢迚入容器中，退出容器后，容器自劢关闭。
• -t：为容器重新分配一个伪输入终端，通常不 -i 同时使用。
• -d：以守护（后台）模式运行容器。创建一个容器在后台运行，需要使用docker exec 迚入容器。退出后，容器丌会关闭。
• -it 创建的容器一般称为交亏式容器，-id 创建的容器一般称为守护式容器
• --name：为创建的容器命名
 迚入容器
docker exec 参数 # ，容器不会关闭
 停止容器
docker stop 容器名称
 启劢容器
docker start 容器名称
 删除容器：如果容器是运行状态则删除失败，需要停止容器才能删除
docker rm 容器名称
 查看容器信息
docker inspect 容器名
```



### 三、安装mysql

```
1、查看镜像mysql
docker search mysql
2、下载对应的mysql版本
docker pull mysql:5.6
3、查看下载的镜像
docker images
4、安装mysql（-v是数据卷本地目录映射到容器中的目录）
docker run -id \
-p 3306:3306 \
--name=mysql \
-v $PWD/conf:/etc/mysql/conf.d \
-v $PWD/logs:/logs \
-v $PWD/data:/var/lib/mysql \
-e MYSQL_ROOT_PASSWORD=123456 \
mysql

5、查看安装好的mysql进行
docker ps | grep mysql
6、进入安装好的msyql容器
docker exec -it mysql bash
7、登录mysql
mysql -h localhost -u root -p123456
8、查看mysql的数据库
show databases;

6、常用sql
CREATE DATABASE spd;
SHOW DATABASES;
USE spd;
DROP DATABASE spd;

CREATE TABLE student(
	id INT PRIMARY KEY AUTO_INCREMENT,
	NAME VARCHAR(10),
	age VARCHAR(10)
);

SHOW TABLES;
SHOW CREATE TABLE student;
DROP TABLE student;

ALTER TABLE student ADD image VARCHAR(20);
ALTER TABLE student MODIFY image VARCHAR(40);
ALTER TABLE student DROP image;
RENAME TABLE student TO employee;

INSERT INTO student(NAME,age) VALUES('张三','13');
SELECT * FROM student;
UPDATE employee SET age=14 WHERE id = 1;
DELETE FROM student WHERE id = 1;
DROP TABLE student;
TRUNCATE student;
-- select ... from ... where ... group by... having... order by ... limit; 
CREATE TABLE orders(
	id INT,
	product VARCHAR(20),
	price FLOAT
);
INSERT INTO orders(id,product,price) VALUES(1,'电视',900);
INSERT INTO orders(id,product,price) VALUES(2,'洗衣机',100);
INSERT INTO orders(id,product,price) VALUES(3,'洗衣粉',90);
INSERT INTO orders(id,product,price) VALUES(4,'桔子',9);
INSERT INTO orders(id,product,price) VALUES(5,'洗衣粉',90);
INSERT INTO orders(id,product,price) VALUES(6,'电视',900);

USE day16;
CREATE TABLE dept(
	did INT PRIMARY KEY AUTO_INCREMENT,
	dname VARCHAR(30)
);
CREATE TABLE emp(
	eid INT PRIMARY KEY AUTO_INCREMENT,
	ename VARCHAR(20),
	salaly DOUBLE,
	dno INT
);
SHOW TABLES;

INSERT INTO dept VALUES(NULL,'研发部');
INSERT INTO dept VALUES(NULL,'销售部');
INSERT INTO dept VALUES(NULL,'人事部');
INSERT INTO dept VALUES(NULL,'扯淡部');
INSERT INTO dept VALUES(NULL,'牛宝宝部');
INSERT INTO emp VALUES(NULL,'班长',10000,1);
INSERT INTO emp VALUES(NULL,'美美',10000,2);
INSERT INTO emp VALUES(NULL,'小凤',10000,3);
INSERT INTO emp VALUES(NULL,'如花',10000,2);
INSERT INTO emp VALUES(NULL,'芙蓉',10000,1);
INSERT INTO emp VALUES(NULL,'东东',800,NULL);
INSERT INTO emp VALUES(NULL,'波波',1000,NULL);
ALTER TABLE emp ADD FOREIGN KEY emp(dno) REFERENCES dept(did);
SELECT * FROM dept;
SELECT * FROM emp;

* 添加外键
语法：alter table emp add foreign key 当前表名(dno) references 关联的表(did);
alter table emp add foreign key emp(dno) references dept(did);
SELECT * FROM dept,emp WHERE dept.did=emp.dno;
SELECT * FROM dept INNER JOIN emp ON dept.did=emp.dno;
SELECT * FROM dept LEFT JOIN emp ON dept.did=emp.dno;
SELECT * FROM dept RIGHT JOIN emp ON dept.did=emp.dno;

SELECT * FROM emp;

SELECT * FROM emp LIMIT 2,4;
SELECT * FROM emp ORDER BY salaly DESC;
SELECT * FROM emp ORDER BY salaly ASC;
-- select product,sum(price) from orders group by product having sum(price) > 100;
-- select ... from ... where ... group by... having... order by ... limit; 
```

### 四、安装tomcat

```
一、拉取镜像文件
docker pull tomcat #最新版本的tomcat
二、查看拉取的文件
docker images
三、安装镜像文件（-v是数据卷本地目录映射到容器中的目录）
docker run -id --name=tomcat \
-p 8080:8080 \
-v $PWD:/usr/local/tomcat/webapps \
tomcat

二、访问tomcat
http://192.168.188.130:8080/webapps/index.html
```

### 五、安装nginx

```
三、部署Nginx
1. 搜索nginx镜像
2. 拉取nginx镜像
3. 创建容器，设置端口映射、目录映射
docker search nginx
docker pull nginx
# 在/root目录下创建nginx目录用于存储nginx数据信息
mkdir ~/nginx
cd ~/nginx
mkdir conf
cd conf
# 在~/nginx/conf/下创建nginx.conf文件,粘贴下面内容
vim nginx.conf


user nginx;
worker_processes 1;
error_log /var/log/nginx/error.log warn;
pid /var/run/nginx.pid;
events {
worker_connections 1024;
}
http {
include /etc/nginx/mime.types;
default_type application/octet-stream;
log_format main '$remote_addr - $remote_user [$time_local] "$request" '
'$status $body_bytes_sent "$http_referer" '
'"$http_user_agent" "$http_x_forwarded_for"';
access_log /var/log/nginx/access.log main;
sendfile on;
#tcp_nopush on;
keepalive_timeout 65;
#gzip on;
include /etc/nginx/conf.d/*.conf;:
}

安装
docker run -id --name=nginx \
-p 80:80 \
-v $PWD/conf/nginx.conf:/etc/nginx/nginx.conf \
-v $PWD/logs:/var/log/nginx \
-v $PWD/html:/usr/share/nginx/html \
nginx

参数说明：
-p 80:80：将容器的 80端口映射到宿主机的 80 端口。
-v $PWD/conf/nginx.conf:/etc/nginx/nginx.conf：将主机当前目录下的 /conf/nginx.conf 挂载到容
器的 :/etc/nginx/nginx.conf。配置目录
-v $PWD/logs:/var/log/nginx：将主机当前目录下的 logs 目录挂载到容器的/var/log/nginx。日志目
录
4. 使用外部机器访问nginx
http://192.168.188.130/index.html
```

### 六、安装redis

```
1、查找镜像redis
docker search redis
2、拉取redis
docker pull redis
3、查看拉取的redis
docker images
4、安装redis
docker run -id --name=redis -p 6379:6379 redis:5.0
5、外部机器连接redis
./redis-cli.exe -h 192.168.149.135 -p 6379
```

### 七、dockerfile

1、编写docker_file文件

```
FROM JAVA:8
MAINTAINER shanpd <shanpd.sina.com>
ADD springboot-hello-0.0.1-SNAPSHOT.jar app.jar
CDM java -jar app.jar
```

2、构建镜像文件

```
docker build -f ./spingboot_docker -t app .
```

3、创建sprintboot的docker容器

```
docker run -id --name=app -p 9090:8080 app
```

4、访问springboot 服务

```
http://192.168.188.130:9090/hello
```

5、docker_compose

```
DockerCompose是一个编排多容器分布式部署的工具，提供命令集管理容器化应用的完整开发周期，包括构建，启动和停止，使用步骤
1、利用Dockerfile定义支行环境镜像
2、使用docker-compose.yml定义组成应用的各服务
3、支行docker-compose up 启动应用


一、安装Docker Compose
# Compose目前已经完全支持Linux、Mac OS和Windows，在我们安装Compose之前，需要先安装Docker。下面我 们以
编译好的二进制包方式安装在Linux系统中。
curl ‐L https://github.com/docker/compose/releases/download/1.22.0/docker‐compose‐`uname ‐s`‐
`uname ‐m` ‐o /usr/local/bin/docker‐compose
# 设置文件可执行权限
chmod +x /usr/local/bin/docker‐compose
# 查看版本信息
docker‐compose ‐version

二、卸载Docker Compose
# 二进制包方式安装的，删除二进制文件即可
rm /usr/local/bin/docker‐compose


三、 使用docker compose编排nginx+springboot项目
1. 创建docker-compose目录
mkdir ~/docker‐compose
cd ~/docker‐compose

2. 编写 docker-compose.yml 文件
version: '3'
services:
nginx:
image: nginx
ports:
‐ 80:80
links:
‐ app
volumes:
‐ ./nginx/conf.d:/etc/nginx/conf.d
app:
image: app
expose:
‐ "8080"

3. 创建./nginx/conf.d目录
mkdir ‐p ./nginx/conf.d

4. 在./nginx/conf.d目录下 编写itheima.conf文件
server {
listen 80;
access_log off;
location / {
proxy_pass http://app:8080;
}
}

5. 在~/docker-compose 目录下 使用docker-compose 启动容器
docker‐compose up

6. 测试访问
http://192.168.149.135/hello
```


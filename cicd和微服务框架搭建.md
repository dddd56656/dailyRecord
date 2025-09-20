# 前期准备:绑定数据源

```
sudo cp -a /etc/yum.repos.d/CentOS-Base.repo /etc/yum.repos.d/CentOS-Base.repo.bak

sudo curl -o /etc/yum.repos.d/CentOS-Base.repo http://mirrors.aliyun.com/repo/Centos-7.repo

sudo yum clean all
sudo yum makecache



sudo yum install ntpdate

ntpdate us.pool.ntp.org

crontab -e

*/1 * * * * /usr/sbin/ntpdate us.pool.ntp.org;
```

# 离线安装docker

```
mkdir -p /opt/lagou/software
--软件安装包存放目录
mkdir -p /opt/lagou/servers
--软件安装目录
cd /opt/lagou/software
sudo yum install -y yum-utils
sudo yum-config-manager --add-repo http://mirrors.aliyun.com/docker-ce/linux/centos/docker-ce.repo
yum makecache fast
yum install --downloadonly --downloaddir=/opt/lagou/software docker-ce docker-ce-cli containerd.io
yum localinstall *.rpm



```

```
一，更改docker的镜像源
1.修改：vi /etc/docker/daemon.json 文件，新增下面的内容 （一般建议新增2-3个）
{
  "registry-mirrors": ["http://docker.ketches.cn"]
}


sudo systemctl start docker
```

必须的镜像，mysql，gogs，Jenkins，nexus



(不需要进行下面的安装操作，简历使用神领物流已经配置好了镜像，直接load即可)离线安装cicd必须软件

#Jenkins

```
#Jenkins

docker run -d \                           # 以守护进程模式运行 Docker 容器
-p 8090:8080 \                            # 映射宿主机的 8090 端口到容器的 8080 端口（Jenkins web UI）
-p 50000:50000 \                          # 映射宿主机的 50000 端口到容器的 50000 端口（Jenkins代理通信端口）
-v /usr/local/src/jenkins:/var/jenkins_home \ # 挂载宿主机的 /usr/local/src/jenkins 目录到容器的 /var/jenk
ns_home（Jenkins 数据存储目录）
-v /maven:/maven \                        # 挂载宿主机的 /maven 目录到容器内部的 /maven（存放 Maven 配置和库的目录）
-v /etc/localtime:/etc/localtime \        # 挂载宿主机的时区配置到容器，保证容器的时间与宿主机同步
-v /usr/bin/docker:/usr/bin/docker \      # 挂载宿主机的 Docker 可执行文件到容器中，允许在容器内调用 Docker 命令
-v /var/run/docker.sock:/var/run/docker.sock \ # 挂载 Docker 套接字，使容器内的 Jenkins 可以管理宿主机上的 Docker
--name jenkins \                          # 设置容器的名称为 "jenkins"
-e TZ=Asia/Shanghai \                     # 设置环境变量 TZ，指定时区为 Asia/Shanghai
jenkins/jenkins:lts-jdk11                 # 使用镜像 jenkins/jenkins:lts-jdk11 启动容器

# 设置权限，否则会出现docker打包无权限
chmod a+rw /var/run/docker.sock           # 更改 /var/run/docker.sock 的权限，赋予所有用户读写权限

docker run -d \                 # 以守护进程模式运行 Docker 容器
--name=gogs \                   # 设置容器名称为 gogs
-p 10022:22 \                   # 映射容器的 22 端口到宿主机的 10022 端口，用于 SSH 访问
-p 10880:3000 \                 # 映射容器的 3000 端口到宿主机的 10880 端口，用于 Web 访问
-v gogs-data:/data \            # 挂载卷 gogs-data 到容器的 /data 目录，存储 Gogs 数据
--restart=always \              # 设置容器异常退出时总是重启
-e TZ=Asia/Shanghai \           # 设置环境变量，定义容器的时区为上海
gogs/gogs:0.12                  # 使用 gogs/gogs:0.12 镜像启动容器


docker run -d \                   # 以守护进程模式运行 Docker 容器
-p 8081:8081 \                    # 映射容器的 8081 端口到宿主机的 8081 端口，用于 Nexus Web 界面访问
--name nexus \                    # 设置容器名称为 nexus
--restart=always \                # 设置容器异常退出时总是重启
-v nexus-data:/sonatype-work \    # 挂载卷 nexus-data 到容器的 /sonatype-work 目录，存储 Nexus 数据
sonatype/nexus:2.15.1             # 使用 sonatype/nexus:2.15.1 镜像启动容器



```

```
docker run \
  -d \
  --name mysql \
  --restart=always \
  -e MYSQL_ROOT_PASSWORD=123 \
  -p 3306:3306 \
  -v /usr/local/src/mysql/conf:/etc/mysql/conf.d \
  -v /usr/local/src/mysql/data:/var/lib/mysql \
  mysql:8.0.29


docker run -d \
-p 8090:8080 \
-p 50000:50000 \
-v /usr/local/src/jenkins:/var/jenkins_home \
-v  /maven:/maven \
-v /etc/localtime:/etc/localtime \
-v /usr/bin/docker:/usr/bin/docker \
-v /var/run/docker.sock:/var/run/docker.sock \
--name jenkins \
-e TZ=Asia/Shanghai \
jenkins/jenkins:lts-jdk11

docker run -d \
--name=gogs \
-p 10022:22 \
-p 10880:3000 \
-v gogs-data:/data \
--restart=always \
-e TZ=Asia/Shanghai \
gogs/gogs:0.12

docker run -d \
-p 8081:8081 \
--name nexus \
--restart=always \
-v nexus-data:/sonatype-work \
sonatype/nexus:2.15.1



chmod a+rw /var/run/docker.sock

```

安装jdk11环境

```
上传文件jdk-11.0.24_linux-x64_bin.tar.gz
chmod 755 jdk-11.0.24_linux-x64_bin.tar.gz
解压文件到/opt/lagou/servers目录下

tar -zxvf jdk-11.0.24_linux-x64_bin.tar.gz -C /opt/lagou/servers

cd /opt/lagou/servers
ll


vi /etc/profile


export JAVA_HOME=/opt/lagou/servers/jdk-11.0.24
export CLASSPATH=.:${JAVA_HOME}/jre/lib/rt.jar:${JAVA_HOME}/lib/dt.jar:${JAVA_HOME}/lib/tools.jar
export PATH=$PATH:${JAVA_HOME}/bin


source /etc/profile
java -version
```

```
http://192.168.49.121:8081/nexus

http://192.168.49.121:8090/


http://192.168.49.121:10880/

root 123
```


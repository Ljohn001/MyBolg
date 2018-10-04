# 数据同步组件otter环境搭建

## Otter介绍

  阿里otter工具地址：<https://github.com/alibaba/otter/wiki>

​    otter为阿里的一款增量数据同步工具，基于数据库增量日志解析，准实时同步到本机房或异地机房的mysql/oracle数据库. 一个分布式数据库同步系统。

## 工作原理

![](/home/ljohn/文档/Markdown/pictures/otter.jpeg)

 

详细信息参考：

https://github.com/alibaba/otter/wiki/Manager_Quickstart

https://github.com/alibaba/otter/wiki/Node_Quickstart

## 环境搭建

## 1、mysql数据源开启binlog

​    源库mysql需要开启binlog，因为otter是基于canal的，而canal是基于binlog的。

```
vi my.cnf

[mysqld]
server-id=1
log-bin=/var/lib/mysql/mysql-bin
binlog_format=row
```

## 2、zookeeper搭建

1、下载地址：<https://mirrors.tuna.tsinghua.edu.cn/apache/kafka/>
2、kafka官方文档：<http://kafka.apache.org/>
3、解压缩：tar xf kafka_2.11-2.0.0.tgz && cd kafka_2.11-2.0.0
4、ZooKeeper

```
启动，bin/zookeeper-server-start.sh config/zookeeper.properties
关闭，bin/zookeeper-server-stop.sh config/zookeeper.properties
后台启动，bin/zookeeper-server-start.sh config/zookeeper.properties 1>logs/zk.log 2>&1 &
```
## 3、otter配置

### 环境准备

1. otter manager依赖于mysql进行配置信息的存储，所以需要预先安装mysql，并初始化otter manager的系统表结构斌咯个

​    a. 安装mysql，这里不展开，网上一搜一大把

​    b. 初始化otter manager系统表：

下载：

```
wget https://raw.github.com/alibaba/otter/master/manager/deployer/src/main/resources/sql/otter-manager-schema.sql 
```

载入：

```
source otter-manager-schema.sql
```

2. 整个otter架构依赖了zookeeper进行多节点调度，所以需要预先安装zookeeper，不需要初始化节点，otter程序启动后会自检.

​    a. manager需要在otter.properties中指定一个就近的zookeeper集群机器

### 启动manager

1. 下载otter manager

直接下载 ，可访问：https://github.com/alibaba/otter/releases ，会列出所有历史的发布版本包下载方式，比如以x.y.z版本为例子：

```
wget https://github.com/alibaba/otter/releases/download/otter-x.y.z/manager.deployer-x.y.z.tar.gz
```

2. 解压缩

```
mkdir /opt/otter/manager
tar zxvf manager.deployer-$version.tar.gz  -C /opt/otter/manager
ls /opt/otter/manager
bin  conf  lib  webapp
```

3. 配置修改

```
## otter manager domain name
otter.domainName = 127.0.0.1
## otter manager http port
otter.port = 8080
## jetty web config xml
otter.jetty = jetty.xml

## otter manager database config
otter.database.driver.class.name = com.mysql.jdbc.Driver
otter.database.driver.url = jdbc:mysql://127.0.0.1:3306/otter
otter.database.driver.username = root
otter.database.driver.password = root

## otter communication port
otter.communication.manager.port = 1099

## otter communication pool size
otter.communication.pool.size = 10

## default zookeeper address
otter.zookeeper.cluster.default = 127.0.0.1:2181
## default zookeeper sesstion timeout = 60s
otter.zookeeper.sessionTimeout = 60000

## otter arbitrate connect manager config
otter.manager.address = ${otter.domainName}:${otter.communication.manager.port}

## should run in product mode , true/false
otter.manager.productionMode = true

## self-monitor enable or disable
otter.manager.monitor.self.enable = true
## self-montir interval , default 120s
otter.manager.monitor.self.interval = 120
## auto-recovery paused enable or disable
otter.manager.monitor.recovery.paused = true
# manager email user config
otter.manager.monitor.email.host = smtp.gmail.com
otter.manager.monitor.email.username =
otter.manager.monitor.email.password =
otter.manager.monitor.email.stmp.port = 465
```

4.启动

```
cd /opt/otter/manager/bin/
sh startup.sh
```

5.验证

```
root@gp2:/opt/otter/manager# tailf logs/manager.log 
Java HotSpot(TM) 64-Bit Server VM warning: ignoring option PermSize=96m; support was removed in 8.0
Java HotSpot(TM) 64-Bit Server VM warning: ignoring option MaxPermSize=256m; support was removed in 8.0
Java HotSpot(TM) 64-Bit Server VM warning: UseCMSCompactAtFullCollection is deprecated and will likely be removed in a future release.
2018-08-22 14:56:46.918 [] INFO  com.alibaba.otter.manager.deployer.OtterManagerLauncher - ## start the manager server.
2018-08-22 14:56:53.871 [] INFO  com.alibaba.otter.manager.deployer.JettyEmbedServer - ##Jetty Embed Server is startup!
2018-08-22 14:56:53.871 [] INFO  com.alibaba.otter.manager.deployer.OtterManagerLauncher - ## the manager server is running now ......
```

![](/home/ljohn/文档/Markdown/pictures/otter_login.png)

> 访问：http://127.0.0.1:8080/login.htm，初始密码为：admin/admin，即可完成登录. 目前：匿名用户只有只读查看的权限，登录为管理员才可以有操作权限



### 配置manager

1. otter node会受otter manager进行管理，所以需要预先安装otter manager

2. 完成manager安装后，需要在manager页面为node定义配置信息，并生一个唯一id.

​    a. 首先访问manager页面的机器管理页面，点击添加机器按钮

![img](https://camo.githubusercontent.com/b7e6d09d878424a6e96c0f66958ee5969742696a/687474703a2f2f646c322e69746579652e636f6d2f75706c6f61642f6174746163686d656e742f303038382f313835392f66393432306564642d326364392d333361332d616531362d3234313339633430303461662e706e67)
几点说明：

- 机器名称：可以随意定义，方便自己记忆即可
- 机器ip：对应node节点将要部署的机器ip，如果有多ip时，可选择其中一个ip进行暴露. (此ip是整个集群通讯的入口，实际情况千万别使用127.0.0.1，否则多个机器的node节点会无法识别)
- 机器端口：对应node节点将要部署时启动的数据通讯端口，建议值：2088
- 下载端口：对应node节点将要部署时启动的数据下载端口，建议值：9090
- 外部ip ：对应node节点将要部署的机器ip，存在的一个外部ip，允许通讯的时候走公网处理。
- zookeeper集群：为提升通讯效率，不同机房的机器可选择就近的zookeeper集群.

node这种设计，是为解决单机部署多实例而设计的，允许单机多node指定不同的端口

​    b. 机器添加完成后，跳转到机器列表页面，获取对应的机器序号nid

![img](https://camo.githubusercontent.com/bcb84482f1e7798519f722a5af7b260e14e2afd9/687474703a2f2f646c322e69746579652e636f6d2f75706c6f61642f6174746163686d656e742f303038382f313836312f30366535643133342d343537362d336135322d626263382d3834323331373336303365632e706e67)

**通过这两部操作，获取到了node节点对应的唯一标示，称之为node id，简称：nid. 记录该nid，后续启动nid时会使用**

3. node节点进行跨机房传输时，会使用到HTTP多线程传输技术，目前主要依赖了aria2c做为其下载客户端，后续会推出java版本.

​    a. aria2 官方首页： <http://aria2.sourceforge.net/>

​    b. 下载页面： <http://sourceforge.net/projects/aria2/files/stable/>

当前测试过多个HTTP多线程下载客户端，比如wget,curl,axel,oget,proz,aria2c，测试结果aria2c下载效率最快，基本可以压满网卡.

注意：下载完成或者编译完成后，将对应的aria2c包加入到PATH路径即可.

### 启动node

1. 下载otter node

   直接下载 ，可访问：https://github.com/alibaba/otter/releases ，会列出所有历史的发布版本包下载方式，比如以x.y.z版本为例子：

```
wget https://github.com/alibaba/otter/releases/download/otter-x.y.z/node.deployer-x.y.z.tar.gz
```

2. 解压缩

```
mkdir /opt/otter/node
tar zxvf node.deployer-$version.tar.gz  -C /opt/otter/node  
```

3. 配置修改

​    a. nid配置 (将环境准备中添加机器后获取到的序号，保存到conf目录下的nid文件，比如我添加的机器对应序号为1)

```
echo 1 > conf/nid
```

​    b. otter.properties配置修改

```
# otter node root dir
otter.nodeHome = ${user.dir}/../

## otter node dir
otter.htdocs.dir = ${otter.nodeHome}/htdocs
otter.download.dir = ${otter.nodeHome}/download
otter.extend.dir= ${otter.nodeHome}/extend

## default zookeeper sesstion timeout = 60s
otter.zookeeper.sessionTimeout = 60000

## otter communication pool size
otter.communication.pool.size = 10

## otter arbitrate & node connect manager config
otter.manager.address = 127.0.0.1:1099
```

4.启动

```
cd /opt/otter/node/bin/
sh startup.sh
```

5.验证
# coin-exchange1

#### 介绍
数字货币交易所 项目描述：提供行情K线，钱包，法币兑换，币币交易，合约，期权，代理商，API接入等功能，参照币严交易所 技术架构：后端Java spring cloud 框架,+ mysql, mongodb, kafka ，前端VUE+ios/ android


####   项目源码地址 ：

代码量太多不好上传，链接是开源版的，我们提供商业版的，

可以参考

https://gitee.com/bizzan/coin-exchange?_from=gitee_search



#### 软件架构
软件架构说明

#### 软件截图
![输入图片说明](%E5%BE%AE%E4%BF%A1%E5%9B%BE%E7%89%87_20220524004232.jpg)

#### 
开源的项目 总会存在不足 ，需要商业化的定制开发 ，开源联系我们

手机微信 18665802636

QQ 75039960


#### 项目说明
特色
特色1: 基于内存撮合引擎，与传统基于数据库撮合更快
特色2: 前后端分离，基于Token的Api授权机制
特色3: 基于SpringCloud微服务架构，扩展更容易
特色4: MySQL、MongoDB、Redis多种数据存储方式，只为更快
特色5: Kafka发布订阅消息队列，让订单更快流转
特色6: 主流币种对接区块链接口齐全，开箱即用
特色7: 冷热钱包分离，两种提现方式，保证安全
特色8: 机器人系统，同步行情，维护深度，防止搬砖
特色9: 原生App，Java和ObjectC提供原生体验
特色10: 交易所设计者提供技术支持，部署+二开无忧
特色11: 支持添加自定义平台币及其他币种



===============开发说明文档=====================================

## 本地开发说明

本文档适合有一定基础的人看，需要具备SpringCloud/SringBoot开发基础、Vue/Npm前端开发基础、Mysql基础、Mongodb基础、Redis基础、Linux基础。一般而言，大部分Java程序员都具备这些能力。

同时，如果需要连接区块链，对区块链钱包进行操作/数据获取，你还需要具备一些区块链相关的基础知识，包括但不限于：搭建比特币/以太坊等节点、区块链节点RPC访问、比特币钱包基础、以太坊钱包基础等。一般而言，你不需要特别精通区块链的运行过程，只需要对区块链的运行原理有一定的了解。

本项目前后端分离，如果你是一个全栈程序员，这个项目应该很快就能调试通。
有什么问题可以添加QQ：75039960，我会给予一定的技术援助（小白问题请绕过）。

## 关于Framework开发

00_Framework文件夹下的项目是所有服务的集合，通过SpringCloud的微服务开发模式进行开发，你可以通过Eclipse打开整个工程项目，我的开发工具版本如下：

> Eclipse Java EE IDE for Web Developers.

> Version: Photon Release (4.8.0)

> Build id: 20180619-1200

首先，为开发工具安装Lombok插件。

用Eclipse打开项目后，你的开发工程目录应该如下图所示：
![开发界面](https://images.gitee.com/uploads/images/2020/0324/104050_1d27dea9_2182501.png "QQ截图20200324104038.png")

编译项目时，请先编译core以及xxx-core项目，否则会提示缺少类（本项目使用JPA QueryDsl实现表操作）。

每个项目都有独立的服务端口（配置文件中），你可以直接通过（IP：端口）调用服务，也可以通过网关（Cloud项目，7000端口）调用服务，也可以通过配置Nginx反向代理进行服务调用（生产环境使用此种方式）

## 关于后台管理Web端开发

配置文件在src/config/api.js，你可以在这里配置你的Admin服务IP端口，因为后台管理web只会调用一个Admin服务，所以，你可以选择直接指定到Admin服务IP：端口。

同时，你还需要修改src/service/http.js文件中的访问配置。

启动方式：通过npm run dev即可热启动项目，通过npm run build可编译出部署文件。


## 关于前台Web端开发

前台Web端（PC）因为要访问不同的服务，像个人信息需要调用Ucenter提供的服务，交易需要调用Exchange-api的服务，所以，你需要通过网关的方式为前端提供服务。
配置文件在：src/main.js，你可通过修改Vue.prototype.rootHost及Vue.prototype.host来实现对后端服务的调用。

## 关于数据库（MySQL）
数据库文件在00_Framework/sql文件夹下，提供了db_patch，这个文件仅提供了一些基础数据（菜单树、管理员权限等），其他的数据库表会在jar包首次运行的时候自动更新到数据库，配置项在：


> #jpa

> spring.jpa.show-sql=true

> spring.data.jpa.repositories.enabled=true

> spring.jpa.hibernate.ddl-auto=update

如果你不希望数据库表与Java类实体动态更新，你可选择修改配置项：spring.jpa.hibernate.ddl-auto=update

## 环境配置

系统运行依赖于Mongodb、Redis、MySQL、kafka、阿里云OSS，所以你需要准备好这些基础服务。

## 常见编译问题（Error）

一、启动ucenter-api时，提示下列错误：
Error creating bean with name 'enableRedisKeyspaceNotificationsInitializer' defined in class path resource
原因：使用了注解：@EnableRedisHttpSession，这个注解是用来开启Redis来集式式管理Session。而在使用这种方式的时候，是需要Redis开启Keyspace Notifications功能的，默认是关闭的。这个功能有一个参数来控制它，notify-keyspace-events，把它的值改为Egx。
腾讯云中的redis参数notify-keyspace-events修改方法：进入实例->参数，修改即可。


2、运行jar包提示Failed to get nested archive for entry BOOT.......
提示缺少spark.xxxx.DB类，这是因为编译时未将spark-core.jar拷贝到压缩包中，这个时候需要将pom.xml文件中修改一下，追加：
				<configuration>
				    <includeSystemScope>true</includeSystemScope>
				</configuration>
追加位置：<artifactId>spring-boot-maven-plugin</artifactId>，与<executions>平级
然后删除target下的jar包，重新生成，最好run maven clean以后再maven install


3、org.apache.shiro.authz.UnauthorizedException: Subject does not have permission [system:announcement:deletes]
这种错误是操作没有权限导致，检查操作的方法与数据库中的是否一致。
第一步，通过浏览器F12查看请求的链接，一般显示404
第二步，检查数据库中admin_permission表中是否有要访问的链接

3、market.jar启动失败，提示connection refuesed之类的错误，首先用netstat -tunlp查看端口6005（也就是exchange）是否有监听，如果没有，则说明exchange未完全启动。
导致exchange启动慢的原因主要是初始化时，对未完成的订单加载慢。初始化时，需要从mangodb加载订单成交详情，这是非常巨大的数据。

## 一些配置项说明

1、market/application.properties
	# 二级推荐人币币手续费佣金是否发放(true：开放    false：不开放)
	second.referrer.award=false

2、ucenter-api/application.properties
	# system(发送邮件使用)
	spark.system.work-id=1
	spark.system.data-center-id=1
	spark.system.host=
	spark.system.name=
        *      #推荐注册奖励，1=被推荐人必须实名认证才可获得奖励，否则无限制，要与admin模块里面的配置保持统一
	commission.need.real-name=1
	#推荐注册奖励是否开启二级奖励（1=开启，0=关闭）
	commission.promotion.second-level=0
	#个人推广链接的前缀，随着登录接口返回给客户端。客户端那边与推广码连接，组成个人推广链接。如果有推广注册功能必填
	person.promote.prefix=http://www.bizzan.com/#/register?agent=
	#转账接口地址
	transfer.url=
	transfer.key=
	transfer.smac=

## Nginx配置

 **参考网址：** 

https://blog.csdn.net/qq_36628908/article/details/80243713

nginx文档目录：usr/local/nginx/html/、usr/local/nginx/conf/

 **注意事项：** 

1. api.xxxx.com 与 www.xxxx.com需要转发不同服务

2. 需要api.xxxx.com支持websocket


===========================

依赖环境

```
yum install -y wget  
yum install -y vim-enhanced  
yum install -y make cmake gcc gcc-c++  
yum install -y pcre pcre-devel
yum install -y zlib zlib-devel
yum install -y openssl openssl-devel
```


下载nginx-1.12.2.tar.gz
`wget http://nginx.org/download/nginx-1.12.2.tar.gz`

编译安装 

```
tar -zxvf nginx-1.12.2.tar.gz 
cd nginx-1.12.2
```



```
./configure \
--prefix=/usr/local/nginx \
--pid-path=/var/run/nginx/nginx.pid \
--lock-path=/var/lock/nginx.lock \
--error-log-path=/var/log/nginx/error.log \
--http-log-path=/var/log/nginx/access.log \
--with-http_gzip_static_module \
--http-client-body-temp-path=/var/temp/nginx/client \
--http-proxy-temp-path=/var/temp/nginx/proxy \
--http-fastcgi-temp-path=/var/temp/nginx/fastcgi \
--http-uwsgi-temp-path=/var/temp/nginx/uwsgi \
--with-http_stub_status_module \
--with-http_ssl_module \
--http-scgi-temp-path=/var/temp/nginx/scgi \
--with-stream --with-stream_ssl_module

make 
make install
```


# 关于访问服务
你可以通过Eruka的服务调度中心看到每个服务（http://localhost:7000），或者通过zuul统一网关调用服务，为了方便，我本地开发的方式是用nginx反向代理各种服务，配置参考如下：

```
    server_name locahost;
    location /market {
        client_max_body_size    5m;
        proxy_pass http://localhost:6004;
        proxy_set_header Host $http_host;
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header X-Scheme $scheme;
        proxy_set_header Upgrade $http_upgrade;
        proxy_set_header Connection "upgrade";
    }
    location /exchange {
        client_max_body_size    5m;
        proxy_pass http://localhost:6003;
        proxy_set_header Host $host;
        proxy_set_header X-Real-IP $remote_addr;
    }
    location /uc {
        client_max_body_size    5m;
        proxy_pass http://localhost:6001;
        proxy_set_header Host $host;
        proxy_set_header X-Real-IP $remote_addr;
    }
    location /admin {
        client_max_body_size    5m;
        proxy_pass http://localhost:6010;
        proxy_set_header Host $host;
        proxy_set_header X-Real-IP $remote_addr;
    }
    location /chat {
        client_max_body_size    5m;
        proxy_pass http://localhost:6008;
        proxy_set_header Host $host;
        proxy_set_header Upgrade $http_upgrade;
        proxy_set_header Connection "upgrade";
        proxy_set_header X-Real-IP $remote_addr;
    }
```


####  部署教程 视频   链接 ：  https://www.weixiaolive.com/post/615.html

#### 交易所部署文档   https://b.weixiaolive.com/bizzan/index.html

####  mongodb  安装 
3.1 MongoDB安装与配置
基础软件，你可以安装在任意一台性能好一点的服务器上（例如8核16G），并且硬盘不要太小，最好100G左右，因为会有频繁的读写，所以建议用SSD硬盘
我这里演示的部署方式，都是单机的，你也可以选择集群方式部署，那样可以大幅提升性能，但是也需要更多的服务器来支撑集群部署。

我大概的意思就是，如果你老板跟你说“劳资的系统怎么这么慢”，你大可以一巴掌抽过去（或者抽你自己），跟他说“加钱”。

Mongodb主要用于存储行情数据、钱包充值记录、机器人配置。

安装MongoDB
1、下载&启动MongoDB
wget https://fastdl.mongodb.org/linux/mongodb-linux-x86_64-rhel62-4.0.19.tgz
tar -xvf mongodb-linux-x86_64-rhel62-4.0.19.tgz

/data/mongodb/目录下新建文件mongodb.conf
cd进入mongodb的bin目录，执行：./mongod -f /data/mongodb/mongodb.conf

2、测试Mongodb：
cd到bin目录下，执行连接命令：./mongo 127.0.0.1:27017

3、配置文件示例（mongodb.conf）：
dbpath=/data/mongodb/data
logpath=/data/mongodb/log/mongodb.log
logappend = true
port=27017
fork=true
auth=true
bind_ip=0.0.0.0
创建集合及账户
1、创建管理员账户
use admin
db.createUser({ user: "root", pwd: "123456789", roles: [{ role: "userAdminAnyDatabase", db: "admin" }] })
db.auth("root","123456789")
2、创建行情集合&账户
use bitrade
db.createUser({ user: "bizzan", pwd: "123456789", roles: [{ role: "dbOwner", db: "bitrade" }] })
3、创建钱包集合&账户
use wallet
db.createUser({ user: "bizzan", pwd: "123456789", roles: [{ role: "dbOwner", db: "wallet" }], mechanisms : ["SCRAM-SHA-1"]  })
4、创建机器人集合&账户
use robot
db.createUser({ user: "bizzan", pwd: "123456789", roles: [{ role: "dbOwner", db: "robot" }] })
db.auth("bizzan","123456789")


#####2.1 服务器配置推荐（购买建议）
我建议的配置并不一定是你一定要采用的配置，对于资金实力不足的，你可以适当减配一些；对于资金实力充足的，也可以适当增配。以下是我建议的服务器配置以及需要的三方服务：

操作系统：Centos 6.8 或 Centos6.9

1、服务器一（4核8G，硬盘采用云服务商的默认40G即可），Web服务器，带宽5M～10M，或者更大，用于存放前端Web资源。该服务器需安装Nginx用于域名转发。

2、服务器二（8核16G，100G硬盘SSD），带宽1M（本机主要用于内网），用于搭建Kafka、MongoDB、Redis基础服务。

3、服务器三（8核16G，默认硬盘大小40G即可），带宽1M（本机主要用于内网），用于部署00_framework下的微服务。该服务器需安装Nginx用于转发微服务。

4、服务器四（8核16G，800G硬盘SSD），带宽1M（本机主要用于内网），用于部署BTC、USDT、ETH的钱包节点，硬盘最好是SSD。运行wallet_rpc下的微服务。

5、服务器五（4核8G，100G硬盘），带宽1M（本机主要用于内网），直接购买云服务商提供的MySQL数据库服务即可。

6、阿里云OSS（默认40G即可，几十块钱一年）

7、短信服务，系统支持多种短信，但是你也可以很方便的添加你自己的短信平台

8、邮件服务（可有可无，系统通过邮件注册已经修改为可直接注册，无需验证）

最佳购买建议
一、资金不充裕
腾讯云（香港、新加坡等），按量付费模式。

二、资金充裕
阿里云（香港、新加坡、德国、美国等），包年包月模式。

总结
以上就是需要的服务器配置以及三方服务。其实所有微服务以及Kafka之类的软件都可以运行在一台服务器上，但是在生产环境上，我建议使用多台，主要为了隔离数据，比如钱包节点跟Kafka之类的基础软件隔离，服务（各种jar包）跟数据隔离。

其实MySQL数据库建议使用更加稳定的云服务，当然你也可以自己搭建MySQL（一定程度上可以节省成本，但是为提高维护成本）。

#### 2.2 部署环境与软件版本
本地开发环境
Node V10.15.1
NPM V6.4.1
JDK V1.8.0_131
IDEA 2019.2 Ultimate Edition
服务器部署环境
Centos 6.8 / 6.9
Java openjdk version “1.8.0_212”
Kafka 2.11-2.2.1
Zookeeper 3.4.14
Mongodb 4.0.19
Redis 3.2.12
MySQL 5.6
ETH 1.9.1
BTC/USDT 0.8.2


####  Zookeeper与Kafka安装与配置

注意：本文档为单机版zookeeper及kafka安装文档，如果你需要多服务器部署，请参考官方文档配置。
Kafka依赖Zookeeper，至于Zookeeper是干什么的，我觉得应该是一个很基础的问题，自行百度。

安装zookeeper
1、下载zookeeper（软件版本：zookeeper-3.4.14.tar.gz）
2、复制到/data/kafka/目录下，tar -xvf解压
tar -xvf zookeeper-3.4.14.tar.gz
3、新建zookeeper数据目录/data/kafka/zookeeper
mkdir zookeeper
3、修改conf/zoo_sample.cfg文件，重命名为zoo.cfg
> 上传zoo.cfg到conf文件夹下
4、命令行启动：
> screen -S zookeeper
> ./zkServer.sh start-foreground
5、测试连接
bin/zkCli.sh -server 10.140.0.12:2181
配置文件示例（zoo.cfg）：
tickTime=2000
initLimit=10
syncLimit=5
dataDir=/data/kafka/zookeeper
clientPort=2181
安装kafka
1、下载kafka（软件版本：kafka_2.11-2.2.1.tgz）
2、上传到/data/kafka/kafka/目录下，tar -xvf解压
3、新建log文件夹：/data/kafka/kafka/log，更改权限chmod 777 log
4、修改conf/server.properties配置文件
修改点：advertised.listeners==PLAINTEXT://本机IP:9092

5、命令行启动：
> screen -S kafka
> bin/kafka-server-start.sh config/server.properties
配置文件示例（server.properties）：
broker.id=1
############################# Socket Server Settings #############################
listeners=PLAINTEXT://:9092
advertised.listeners=PLAINTEXT://172.19.0.6:9092
num.network.threads=3
num.io.threads=8
socket.send.buffer.bytes=102400
socket.receive.buffer.bytes=102400
socket.request.max.bytes=104857600
############################# Log Basics #############################
log.dirs=/data/kafka/log
num.partitions=1
num.recovery.threads.per.data.dir=1
############################# Internal Topic Settings  #############################
offsets.topic.replication.factor=1
transaction.state.log.replication.factor=1
transaction.state.log.min.isr=1
############################# Log Retention Policy #############################
log.retention.hours=168
log.segment.bytes=1073741824
log.retention.check.interval.ms=300000
############################# Zookeeper #############################
zookeeper.connect=localhost:2181
zookeeper.connection.timeout.ms=6000
session.timeout.ms=60000
############################# Group Coordinator Settings #############################
group.initial.rebalance.delay.ms=0

##### 3.3 Redis安装与配置
1、安装Redis
yum install redis -y

2、修改配置文件
复制redis.conf文件到/etc/目录下, 配置文件中可修改密码：requirepass(默认：123456789)

3、启动Redis：
redis-server /etc/redis.conf

配置文件示例（redis.conf）:
protected-mode no
port 6379
tcp-backlog 511
timeout 0
tcp-keepalive 300
daemonize yes
supervised no
pidfile /var/run/redis_6379.pid
loglevel notice
logfile /var/log/redis/redis.log
databases 16
save 900 1
save 300 10
save 60 10000
stop-writes-on-bgsave-error yes
rdbcompression yes
rdbchecksum yes
dbfilename dump.rdb
dir /var/lib/redis
slave-serve-stale-data yes
slave-read-only yes
repl-diskless-sync no
repl-diskless-sync-delay 5
repl-disable-tcp-nodelay no
slave-priority 100
requirepass 123456789
appendonly no
appendfilename "appendonly.aof"
appendfsync everysec
no-appendfsync-on-rewrite no
auto-aof-rewrite-percentage 100
auto-aof-rewrite-min-size 64mb
aof-load-truncated yes
lua-time-limit 5000
slowlog-log-slower-than 10000
slowlog-max-len 128
latency-monitor-threshold 0
notify-keyspace-events ""
hash-max-ziplist-entries 512
hash-max-ziplist-value 64
list-max-ziplist-size -2
list-compress-depth 0
set-max-intset-entries 512
zset-max-ziplist-entries 128
zset-max-ziplist-value 64
hll-sparse-max-bytes 3000
activerehashing yes
client-output-buffer-limit normal 0 0 0
client-output-buffer-limit slave 256mb 64mb 60
client-output-buffer-limit pubsub 32mb 8mb 60
hz 10
aof-rewrite-increme

#####3.4 MySQL安装
兼容版本：5.6 / 5.7
配置建议：4核8G，数据盘40G

关于自建MySQL服务
建议购买云MySQL服务，有人在服务器上自建MySQL也可以，但是怎么说呢，运维一套MySQL没你想象的那么简单，尤其是对交易所这种对稳定性要求很高的系统，如果你自建，一旦遇到什么问题，你估计会蒙蔽。但是如果你拥有5到10年的DBA经验，那你就自建吧；如果你只是个曾经开发用过或者自己业余时间捣鼓过MySQL的工程师，建议你不要自己搭建了。

#### 2.4 screen安装及使用
有些人喜欢用nohub命令执行后台任务，有些人喜欢用screen执行“后台任务”，两者没什么特别大的区别，都能开心的跑起来。在我看来，可能就是查看日志的方式的差别，nohub命令运行的jar服务，需要你用cd命令切换到日志目录，然后用tail -f -n 50之类的命令单独查看日志；而screen运行的jar服务，可能只需要screen -r xxxx就可以看到实时的日志。
你习惯用哪个就用哪个，我不喜欢限制别人的思维，所以，我这是在网上复制了下面的文档，爱看就看，不爱看拉倒！

B资源

简介
Screen是一款由GNU计划开发的用于命令行终端切换的自由软件。用户可以通过该软件同时连接多个本地或远程的命令行会话，并在其间自由切换。GNU Screen可以看作是窗口管理器的命令行界面版本。它提供了统一的管理多个会话的界面和相应的功能。

在Screen环境下，所有的会话都独立的运行，并拥有各自的编号、输入、输出和窗口缓存。用户可以通过快捷键在不同的窗口下切换，并可以自由的重定向各个窗口的输入和输出。

安装
[root@VM-0-8-centos ~]# yum  install screen.x86_64
基本语法
$> screen [-AmRvx -ls -wipe][-d <作业名称>][-h <行数>][-r <作业名称>][-s ][-S <作业名称>]
-A 　将所有的视窗都调整为目前终端机的大小。
-d   <作业名称> 　将指定的screen作业离线。
-h   <行数> 　指定视窗的缓冲区行数。
-m 　即使目前已在作业中的screen作业，仍强制建立新的screen作业。
-r   <作业名称> 　恢复离线的screen作业。
-R 　先试图恢复离线的作业。若找不到离线的作业，即建立新的screen作业。
-s 　指定建立新视窗时，所要执行的shell。
-S   <作业名称> 　指定screen作业的名称。
-v 　显示版本信息。
-x 　恢复之前离线的screen作业。
-ls或--list 　显示目前所有的screen作业。
-wipe 　检查目前所有的screen作业，并删除已经无法使用的screen作业
常用screen参数
screen -S yourname -> 新建一个叫yourname的session
screen -ls         -> 列出当前所有的session
screen -r yourname -> 回到yourname这个session
screen -d yourname -> 远程detach某个session
screen -d -r yourname -> 结束当前session并回到yourname这个session
键盘指令（C-a = Ctrl + A）
C-a ? -> 显示所有键绑定信息
C-a c -> 创建一个新的运行shell的窗口并切换到该窗口
C-a n -> Next，切换到下一个 window 
C-a p -> Previous，切换到前一个 window 
C-a 0..9 -> 切换到第 0..9 个 window
Ctrl+a [Space] -> 由视窗0循序切换到视窗9
C-a C-a -> 在两个最近使用的 window 间切换 
C-a x -> 锁住当前的 window，需用用户密码解锁
C-a d -> detach，暂时离开当前session，将目前的 screen session (可能含有多个 windows) 丢到后台执行，并会回到还没进 screen 时的状态，此时在 screen session 里，每个 window 内运行的 process (无论是前台/后台)都在继续执行，即使 logout 也不影响。 
C-a z -> 把当前session放到后台执行，用 shell 的 fg 命令则可回去。
C-a w -> 显示所有窗口列表
C-a t -> time，显示当前时间，和系统的 load 
C-a k -> kill window，强行关闭当前的 window
C-a [ -> 进入 copy mode，在 copy mode 下可以回滚、search、复制就像用使用 vi 一样
    C-b Backward，PageUp 
    C-f Forward，PageDown 
    H(大写) High，将光标移至左上角 
    L Low，将光标移至左下角 
    0 移到行首 
    $ 行末 
    w forward one word，以字为单位往前移 
    b backward one word，以字为单位往后移 
    Space 第一次按为标记区起点，第二次按为终点 
    Esc 结束 copy mode 
C-a ] -> paste，把刚刚在 copy mode 选定的内容贴上
示例操作
[root@VM-0-8-centos exchange]# screen -S cloud
新建会话

[root@VM-0-8-centos exchange]#java -jar cloud.jar
运行java程序

Ctrl + A + D

挂起会话（里面的Java程序仍然在运行）

[root@VM-0-8-centos exchange]# screen -ls
There are screens on:
        8776.wallet     (Detached)
        7231.exchangeapi        (Detached)
        1594.cloud      (Detached)
        2270.exchange   (Detached)
        6253.market     (Detached)
        8002.ucenter    (Detached)
6 Sockets in /var/run/screen/S-root.
查看会话列表

[root@VM-0-8-centos exchange]# screen -r cloud
返回会话

####  nginx  
4.1 微服务转发Nginx安装与配置
此Nginx安装于framework下的微服务所在的服务器。

安装Nginx
yum install nginx.x86_64

配置文件及资源目录：
安装完成后，nginx的默认目录如下：

配置文件目录：/etc/nginx/conf.d
资源文件目录：/usr/share/nginx/html/

修改配置文件
上传配置文件：default.conf

启动Nginx
service nginx start/stop

配置文件示例（default.conf）
server {
    listen       8801; 
    server_name  localhost;
    location / {
        root   /usr/share/nginx/html;
        index  index.html index.htm;
    }
    server_name locahost;
    location /market {
        client_max_body_size    5m;
        proxy_pass http://localhost:6004;
        proxy_set_header Host $http_host;
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header X-Scheme $scheme;
        proxy_set_header Upgrade $http_upgrade;
        proxy_set_header Connection "upgrade";
    }
    location /exchange {
        client_max_body_size    5m;
        proxy_pass http://localhost:6003;
        proxy_set_header Host $host;
        proxy_set_header X-Real-IP $remote_addr;
    }
    location /uc {
        client_max_body_size    5m;
        proxy_pass http://localhost:6001;
        proxy_set_header Host $host;
        proxy_set_header X-Real-IP $remote_addr;
    }
    location /admin {
        client_max_body_size    5m;
        proxy_pass http://localhost:6010;
        proxy_set_header Host $host;
        proxy_set_header X-Real-IP $remote_addr;
    }
    location /chat {
        client_max_body_size    5m;
        proxy_pass http://localhost:6008;
        proxy_set_header Host $host;
        proxy_set_header Upgrade $http_upgrade;
        proxy_set_header Connection "upgrade";
        proxy_set_header X-Real-IP $remote_addr;
    }
    error_page   500 502 503 504  /50x.html;
    location = /50x.html {
        root   /usr/share/nginx/html;
    }

#####  4.2 域名转发Nginx配置（https）
此Nginx安装于Web服务器。

依赖环境
yum install -y wget

yum install -y vim-enhanced

yum install -y make cmake gcc gcc-c++

yum install -y pcre pcre-devel

yum install -y zlib zlib-devel

yum install -y openssl openssl-devel

1、下载Nginx
wget http://nginx.org/download/nginx-1.12.2.tar.gz

2、编译安装
tar -zxvf nginx-1.12.2.tar.gz
cd nginx-1.12.2

./configure --prefix=/usr/local/nginx --pid-path=/var/run/nginx/nginx.pid --lock-path=/var/lock/nginx.lock --error-log-path=/var/log/nginx/error.log --http-log-path=/var/log/nginx/access.log --with-http_gzip_static_module --http-client-body-temp-path=/var/temp/nginx/client --http-proxy-temp-path=/var/temp/nginx/proxy --http-fastcgi-temp-path=/var/temp/nginx/fastcgi --http-uwsgi-temp-path=/var/temp/nginx/uwsgi --with-http_stub_status_module --with-http_ssl_module --http-scgi-temp-path=/var/temp/nginx/scgi --with-stream --with-stream_ssl_module

make
make install

3、上传ssl证书至/etc/nginx/ssl/目录下，申请两个域名的免费证书即可：
www.xxx.com
api.xxx.com

4、修改配置文件
参考示例

5、启动nginx
进入/usr/local/nginx/sbin/，执行
./nginx

修改配置文件后，重载配置文件
./nginx -s reload

配置文件参考（nginx.conf）：
#user  nobody;
worker_processes  2;
#error_log  logs/error.log;
#error_log  logs/error.log  notice;
#error_log  logs/error.log  info;
pid        logs/nginx.pid;
events {
    worker_connections  1024;
}
#监听socket端口
stream{
    upstream market_server{
        hash $remote_addr consistent;
        server 172.19.0.8:28901;
    }
    server {
        listen 28901;
        proxy_pass market_server;
    }
    upstream otc_heart_server{
        hash $remote_addr consistent;
        server 172.19.0.8:28902;
    }
    server {
        listen 28902;
        proxy_pass otc_heart_server;
    }
}
http {
    include       mime.types;
    default_type  application/octet-stream;
    sendfile        on;
    keepalive_timeout  65;
    server {
        listen              80;
        server_name         api.xxxx.com;
        rewrite ^(.*)$ https://$host$1 permanent;
        location / {
            add_header 'Access-Control-Allow-Origin' '*' always;
            proxy_pass      http://172.19.0.8:8801;
            proxy_set_header Host $http_host;
            proxy_set_header X-Real-IP $remote_addr;
            proxy_set_header X-Scheme $scheme;
            proxy_set_header Upgrade $http_upgrade;
            proxy_set_header Connection "upgrade";
            break;
            gzip on;
            gzip_http_version 1.1;
            gzip_comp_level 3;
            gzip_types text/plain application/json application/x-javascript application/css application/xml application/xml+rss text/javascript application/x-httpd-php image/jpeg image/gif image/png image/x-ms-bmp;
        }
    }
    server {
        listen       80;
        server_name  www.xxxx.com;
        #charset koi8-r;
        #access_log  logs/host.access.log  main;
        rewrite ^(.*)$ https://$host$1 permanent;
        location / {
            root   html;
            index  index.html index.htm;
            try_files $uri $uri/ /index.html;
            gzip on;
            gzip_http_version 1.1;
            gzip_comp_level 3;
            gzip_types text/plain application/json application/x-javascript application/css application/xml application/xml+rss text/javascript application/x-httpd-php image/jpeg image/gif image/png image/x-ms-bmp;
        }
        location ~ .*.(eot|ttf|ttc|otf|eot|woff|woff2|svg)(.*) {
            add_header Access-Control-Allow-Origin http://www.xxxx.com;
        }
        # redirect server error pages to the static page /50x.html
        #
        error_page   500 502 503 504  /50x.html;
        location = /50x.html {
            root   html;
        }
    }
    server {
        listen       80;
        server_name  xxxx.com;
        rewrite ^(.*)$ https://$host$1 permanent;
        #charset koi8-r;
        #access_log  logs/host.access.log  main;
        location / {
            root   html;
            index  index.html index.htm;
            try_files $uri $uri/ /index.html;
        }
        location ~ .*.(eot|ttf|ttc|otf|eot|woff|woff2|svg)(.*) {
            add_header Access-Control-Allow-Origin http://www.xxxx.com;
            gzip on;
            gzip_http_version 1.1;
            gzip_comp_level 3;
            gzip_types text/plain application/json application/x-javascript application/css application/xml application/xml+rss text/javascript application/x-httpd-php image/jpeg image/gif image/png image/x-ms-bmp;
        }
        # redirect server error pages to the static page /50x.html
        #
        error_page   500 502 503 504  /50x.html;
        location = /50x.html {
            root   html;
        }
    }
    server {
        listen               443 ssl;
        server_name          www.xxxx.com;
        ssl                  on;
        ssl_certificate      /etc/nginx/ssl/1_www.xxxx.com_bundle.crt;
        ssl_certificate_key  /etc/nginx/ssl/2_www.xxxx.com.key;
        ssl_session_cache    shared:SSL:1m;
        ssl_session_timeout  5m;
        ssl_protocols        TLSv1 TLSv1.1 TLSv1.2;
        ssl_ciphers          ECDH:AESGCM:HIGH:!RC4:!DH:!MD5:!3DES:!aNULL:!eNULL;
        ssl_prefer_server_ciphers  on;
        location / {
            root   html;
            index  index.html index.htm;
            try_files $uri $uri/ /index.html;
            gzip on;
            gzip_http_version 1.1;
            gzip_comp_level 3;
            gzip_types text/plain application/json application/x-javascript application/css application/xml application/xml+rss text/javascript application/x-httpd-php image/jpeg image/gif image/png image/x-ms-bmp;
        }
    }
    server {
        listen               443 ssl;
        server_name          xxxx.com;
        ssl                  on;
        ssl_certificate      /etc/nginx/ssl/1_www.xxxx.com_bundle.crt;
        ssl_certificate_key  /etc/nginx/ssl/2_www.xxxx.com.key;
        ssl_session_cache    shared:SSL:1m;
        ssl_session_timeout  5m;
        ssl_protocols        TLSv1 TLSv1.1 TLSv1.2;
        ssl_ciphers          ECDH:AESGCM:HIGH:!RC4:!DH:!MD5:!3DES:!aNULL:!eNULL;
        ssl_prefer_server_ciphers  on;
        location / {
            root   html;
            index  index.html index.htm;
            try_files $uri $uri/ /index.html;
            gzip on;
            gzip_http_version 1.1;
            gzip_comp_level 3;
            gzip_types text/plain application/json application/x-javascript application/css application/xml application/xml+rss text/javascript application/x-httpd-php image/jpeg image/gif image/png image/x-ms-bmp;
        }
    }
    server {
        listen               443 ssl;
        server_name          api.xxxx.com;
        ssl                  on;
        ssl_certificate      /etc/nginx/ssl/1_api.xxxx.com_bundle.crt;
        ssl_certificate_key  /etc/nginx/ssl/2_api.xxxx.com.key;
        ssl_session_cache    shared:SSL:1m;
        ssl_session_timeout  5m;
        ssl_protocols        TLSv1 TLSv1.1 TLSv1.2;
        ssl_ciphers          ECDH:AESGCM:HIGH:!RC4:!DH:!MD5:!3DES:!aNULL:!eNULL;
        ssl_prefer_server_ciphers  on;
        location / {
            proxy_pass      http://172.19.0.8:8801;
            proxy_set_header Host $http_host;
            proxy_set_header X-Real-IP $remote_addr;
            proxy_set_header X-Scheme $scheme;
            proxy_set_header Upgrade $http_upgrade;
            proxy_set_header Connection "upgrade";
            break;
            gzip on;
            gzip_http_version 1.1;
            gzip_comp_level 3;
            gzip_types text/plain application/json application/x-javascript application/css application/xml application/xml+rss text/javascript application/x-httpd-php image/jpeg image/gif image/png image/x-ms-bmp;
        }
    }
}


####   五、钱包节点部署

BTC and USDT节点部署
1、挂载800G硬盘到服务器
建立区块链节点的时候，需要挂载大容量硬盘。首先通过云控制台挂载硬盘，挂载后，进入SSH命令行界面：

查看可用硬盘列表：
fdisk -l

格式化硬盘：
mkfs.ext4 /dev/vdb

挂载硬盘到/data目录：
mount /dev/vdb /data

2、下载&安装&配置Omni运行包
cd /data

mkdir usdt

cd usdt

wget https://github.com/OmniLayer/omnicore/releases/download/v0.8.2/omnicore-0.8.2-x86_64-linux-gnu.tar.gz

创建区块数据目录：
mkdir data

创建配置文件：
touch bitcoin.conf

进入omni程序目录：
cd /data/usdt/omnicore-0.5.0/bin/

启动节点（可用nohub或screen方式运行）：
./omnicored -conf=/blockchain/usdt/bitcoin.conf -reindex

控制台提示信息如下：

2019-08-13 03:20:56 Initializing Omni Core v0.5.0 [main]
2019-08-13 03:20:56 Loading trades database: OK
2019-08-13 03:20:56 Loading send-to-owners database: OK
2019-08-13 03:20:56 Loading tx meta-info database: OK
2019-08-13 03:20:56 Loading smart property database: OK
2019-08-13 03:20:56 Loading master transactions database: OK
2019-08-13 03:20:56 Loading fee cache database: OK
2019-08-13 03:20:56 Loading fee history database: OK
2019-08-13 03:20:56 Loading persistent state: NONE (no usable previous state found)
2019-08-13 03:20:56 Omni Core initialization completed
查看同步进度：
./omnicore-cli -rpcconnect=127.0.0.1 -rpcuser=bizzan -rpcpassword=123456789 -rpcport=8334 getblockchaininfo

返回控制台信息提示如下：

{
  "chain": "main",
  "blocks": 295978,
  "headers": 589877,
  "bestblockhash": "0000000000000000060a02b55752c56edeeafe25a47c1abfdb65468bf1e5c985",
  "difficulty": 6119726089.128147,
  "mediantime": 1397562780,
  "verificationprogress": 0.06151967312105129,
  "chainwork": "000000000000000000000000000000000000000000003fb9da1c8bfa17a8f117",
配置文件示例（bitcoin.conf）：
# 数据存储目录（此路径为上面建立的数据储存路径的完整路径）
datadir=/data/usdt/data
# 使用测试网络（0：正式网，1：测试网）
testnet=0
# 告知 Bitcoin-Qt 和 bitcoind 接受JSON-RPC命令（是否启用命令和接受RPC服务）
server=1
# 设置 gen=1 以尝试比特币挖矿
gen=0
# 启用交易索引
txindex=1
# 后台执行（是否后台执行）
daemon=0
# 监听 RPC 链接,正式默认端口8333 测试默认18333（最好设置好，免得不清楚）
rpcport=8333
#RPC服务账号和密码，不设置的话是有默认密码的，本文没去深究默认，直接用自己设置的
rpcuser=bizzan
rpcpassword=123456789
#允许那些IP访问RPC接口，以下写法为默认所有ip都可访问，请自己修改成你自己的IP地址
rpcallowip=0.0.0.0/0
rpcconnect=127.0.0.1
 
ETH节点部署
下载ETH节点
wget https://gethstore.blob.core.windows.net/builds/geth-linux-amd64-1.9.15-0f77f34b.tar.gz

启动ETH节点
将下载tar包移动到/data/eth/geth-linux-amd64-1.9.1-b7b2f60f/目录下，执行命令：
./geth --datadir /blockchain/eth/data --cache 1024 --rpc --rpcport 8545 --rpcaddr 127.0.0.1 --syncmode "light"

ETH节点RPC常用操作
打开控制台
$ ./geth attach rpc:”/data/eth/data/geth.ipc”
上述命令表示用本地RPC方式连接ETH节点

创建账户,参数是密码
在控制台输入下面命令：

personal.newAccount(“your password”)
“0x0fec688d601909d28faf6fe18cf6230d08b698b6”

默认第一个创建的用户为主用户

查看用户：查看用户：
eth.accounts
[“0x0fec688d601909d28faf6fe18cf6230d08b698b6”]

eth.accounts
[“0x0fec688d601909d28faf6fe18cf6230d08b698b6”]

账户的排序反映了他们创建的时间。 密钥文件存储在DATADIR / keystore下，可以通过复制其中包含的文件在客户端之间传输。 这些文件使用密码加密，如果它们包含任何数量的以太网，则应备份。 但是，请注意，如果您传输个别密钥文件，则提交的帐户顺序可能会发生变化，您可能无法在同一位置结束同一帐户。 因此请注意，只要您不将外部密钥文件复制到您的密钥存储区，只依赖帐户索引即可。账户的排序反映了他们创建的时间。 密钥文件存储在DATADIR / keystore下，可以通过复制其中包含的文件在客户端之间传输。 这些文件使用密码加密，如果它们包含任何数量的以太网，则应备份。 但是，请注意，如果您传输个别密钥文件，则提交的帐户顺序可能会发生变化，您可能无法在同一位置结束同一帐户。 因此请注意，只要您不将外部密钥文件复制到您的密钥存储区，只依赖帐户索引即可。

解锁帐户
personal.unlockAccount(“0xf9ab190a9c56fd0d945eac9659c0c9519b13c64e”)

或者
这里是给第一个账户解锁

user1=eth.accounts[0]
personal.unlockAccount(user1)

查看帐号余额
eth.getBalance(eth.accounts[0])

getBalance()返回值的单位是wei，wei是以太币的最小单位，1个以太币=10的18次方个wei。要查看有多少个以太币，可以用web3.fromWei()将返回值换算成以太币：

web3.fromWei(eth.getBalance(eth.accounts[0]),’ether’)
340

单位转换： Ether–> Wei

web3.toWei(1)

单位转换： Wei –> Ether

web3.fromWei(10000000000000000)

转账
转账前需要解锁帐号，就像输入银行卡号密码

eth.sendTransaction({“from”:”0x67128734480a0741595538d9d726f33addf83978”, “to”:”0x29a9a6bcf1ce7101ab93a029e2692298fc15e076”, “gas”:31000,”gasPrice”:web3.toWei(300,’gwei’),”value”:”1”})

eth.sendTransaction({“from”:”0x67128734480a0741595538d9d726f33addf83978”, “to”:”0x29a9a6bcf1ce7101ab93a029e2692298fc15e076”, “value”:”10000000000000000000000”})
“0x533d3c770aed09ede826c92e7460fd38d78a101752a7b3b25e4470d8594e77bb”

查看当前区块总数：
eth.blockNumber
69

通过区块号查看区块里打包的交易信息
eth.getBlock(6)
{
difficulty: 2,
extraData: “0xd783010803846765746887676f312e392e32856c696e75780000000000000000cff7302b0c5515614e52f1584ff3f6aceb10dfa6e2facb347bfe3c023878d3857fa48774a98c721bcc1fb2419a177d577a0926e9f51d037095ba53257f7f307701”,
gasLimit: 6246618,
gasUsed: 21000,
hash: “0x5e2506ce385e38bbe23765a24ec25f9742e4a3a5af7cd071088081535a6a0dd2”,
logsBloom: “0x00000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000”,
miner: “0x0000000000000000000000000000000000000000”,
mixHash: “0x0000000000000000000000000000000000000000000000000000000000000000”,
nonce: “0x0000000000000000”,
number: 6,
parentHash: “0x716da23fef7103042762025aabc83f7075fca516ab9dc6d436daa58b8350953b”,
receiptsRoot: “0x056b23fbba480696b65fe5a59b8f2148a1299103c4f57df839233af2cf4ca2d2”,
sha3Uncles: “0x1dcc4de8dec75d7aab85b567b6ccd41ad312451b948a7413f0a142fd40d49347”,
size: 713,
stateRoot: “0x7c93a037c0750ddca8330d9c84912c622cd355c6978fc4267ffe2c8517a32469”,
timestamp: 1520398599,
totalDifficulty: 13,
transactions: [“0x54325698db1fbc85799b2f72070cddc457932abf0eef0d30d4fb2710ddafa941”],
transactionsRoot: “0x712b5bba767dd0ecaeebbbeefdf097647a0f219f11f16e4a8a3d768b59ae442e”,
uncles: []
}

通过交易hash查看交易
eth.getTransaction(“0x54325698db1fbc85799b2f72070cddc457932abf0eef0d30d4fb2710ddafa941”)
{
blockHash: “0x5e2506ce385e38bbe23765a24ec25f9742e4a3a5af7cd071088081535a6a0dd2”,
blockNumber: 6,
from: “0x67128734480a0741595538d9d726f33addf83978”,
gas: 31000,
gasPrice: 300000000000,
hash: “0x54325698db1fbc85799b2f72070cddc457932abf0eef0d30d4fb2710ddafa941”,
input: “0x”,
nonce: 5,
r: “0xe14faca3d11a47ec4617927c84a04936dbaf783cc2187794e04299ce04352404”,
s: “0x6da16b4e07a4fc721273d3b09da1c8b29ad4ce8022a99eb3f8317247cf7f5386”,
to: “0x29a9a6bcf1ce7101ab93a029e2692298fc15e076”,
transactionIndex: 0,
v: “0xa95”,
value: 1
}

查看交易状态查看交易状态
txpool.status
{
pending: 0,
queued: 0
}

常见错误
异常现象：
使用geth客户端，当执行personal.unlockAccount()或在程序中调用personal_unlockAccount接口时，会出现：account unlock with HTTP access is forbidden异常。

异常分析
出于安全考虑，默认禁止了HTTP通道解锁账户，相关issue：https://github.com/ethereum/go-ethereum/pull/17037

解决方法
如果已经了解打开此功能的风险，可通启动命令中添加参数：
--allow-insecure-unlock

示例

./geth —datadir /blockchain/eth/data —cache 1024 —rpc —rpcport 8545 —rpcaddr 127.0.0.1 —syncmode “light” —allow-insecure-unlock

##### 六、   系统部署详解
#### 6.1  Java项目配置修改
如果你通读了我项目主页的文档，通过架构图就应该知道，每个Java的微服务有什么作用，以下仅仅是关于配置修改的建议，你可以自己灵活处理。

配置文件：
application.properties

该文件位于每个java模块的resource目录下（对于懂SpringBoot开发的人来说，这句话就是废话，但是为了防止仍然有人不知道，就这么写了。。。）

修改内容：
eureka.client.serviceUrl.defaultZone=http://172.19.0.8:7000/eureka/
这个配置的IP地址改成Cloud.jar文件运行的服务器所在IP地址即可

spring.datasource.url=jdbc:mysql://172.19.0.5:3306/bizzan?characterEncoding=utf-8&serverTimezone=GMT%2B8&useSSL=false
spring.datasource.driver-class-name=com.mysql.jdbc.Driver
spring.datasource.username=bizzan
spring.datasource.password=123456789
非常显然，数据库的配置，无需多说。

spring.kafka.bootstrap-servers=172.19.0.6:9092
Kafka的IP配置，改个IP地址的事

spring.data.mongodb.uri=mongodb://bizzan:123456@172.19.0.6:27017/bitrade
spring.data.mongodb.database=bitrade
MongoDB的配置

sms.driver=diyi
sms.gateway=
sms.username=111111
sms.password=xxxxxxxxxxxxxxxxxx
sms.sign=BBEX
sms.internationalGateway=
sms.internationalUsername=
sms.internationalPassword=
短信配置相关，其实不用解释太多，为了满足不同客户的需求，已经开发了不少支持各种短信平台的Provider了，你可以在core项目下的provider文件下的support下找到各种支持的实现。

以上是主要的配置，另外还有阿里云OSS的Accesskey & AccessSercu


####   6.2 前端项目运行与打包
由于系统采用了前后端分离的开发模式，所以所有前端都是通过Ajax的方式请求服务接口（Api）的数据的。

Web_Font项目
修改接口，位于main.js文件中，至于main.js在哪，我就不说了，感觉说了你会误会我侮辱你的能力：

Vue.prototype.rootHost = "https://www.xxxx.io";
Vue.prototype.host = "https://api.xxxx.io";
本地开发者模式运行，这种方式方便你调试

[root@VM-0-8-centos 05_Web_Front]# npm run dev
打包项目，会在项目根目录下的dist文件夹下看到你打包的结果，扔到Nginx的html目录即可。

[root@VM-0-8-centos 05_Web_Front]# npm run build
Web_Admin项目
修改接口，位于http.js文件中：

export const BASEURL = axios.defaults.baseURL = 'http://11.22.33.44
[========]
:6010/';
本地开发者模式运行

[root@VM-0-8-centos 04_Web_Admin]# npm run dev
打包项目，会在项目根目录下的dist文件夹下看到你打包的结果，扔到Nginx的html目录即可。

[root@VM-0-8-centos 04_Web_Admin]# npm run build
下面这句话我犹豫了好久才写出来，感觉的确会侮辱到你的能力：

admin项目打包出来的东西，不要也扔到font项目打包出来所扔的那个nginx的html目录了。



####  6.3、  Java微服务运行
由于使用了Java的SpringCloud微服务架构，所以你会在项目里看到N多的Java项目，很多没有接触过的人，大部分是懵逼的。如果你开始阅读这篇文档，我相信你已经把前面服务器的准备工作做好了。下面我们大概看一下这么多jar包如何规划在服务器上运行：

第一台：WEB服务等 内网：172.22.0.13
运行内容：前端 (www.xxxx.com / api.xxxx.com) / Nginx(SSL证书解析)
Nginx路径：/usr/local/nginx/html

第二台：交易引擎等 微服务 内网：172.22.0.3
运行内容1：cloud.jar / exchange.jar / market.jar / ucenter.jar / exchange-api.jar / wallet.jar
运行内容2：nginx微服务转发(/usr/share/nginx/html)

第三台：Kafka/DB/Redis数据库服务等 内网：172.22.0.12
运行内容1：zookeeper / kafka / mongodb / redis
运行内容2：admin.jar / admin前端资源（/usr/share/nginx/html）
运行内容3：er_robot_market.jar / er_robot_normal.jar

第四台：BTC/ETH钱包节点服务等 内网：172.22.0.14
运行内容：USDT节点 / ETH节点 / wallet_rpc下jar

第五台：MySQL云服务器
运行内容：业务数据库

Jar微服务启动顺序
1、cloud.jar - 微服务注册中心
2、exchange.jar - 撮合引擎（需等待cloud完全启动）
3、market.jar - 行情引擎（需等待exchange完全启动）
4、xxx.jar（其他jar包，需等待market完全启动）


#### 七、、常见问题

#### 7.1、 服务无效问题（端口问题）
用阿里云、腾讯云或者宝塔的朋友们，一定要注意端口，微服务本身的端口要打开，基础软件的端口也要打开。不要一遇到服务无法访问就问：“吴彦祖啊，我这个服务怎么没法访问？”。

具体怎么打开，这个就需要一点基础意识了。

希望你也在开端口的时候注意以下，有些端口对内网服务器打开就行了。

以下主要是导致端口无法访问的配置

阿里云安全组
腾讯云安全组
服务器防火墙
宝塔端口封锁
一些微服务端口
6001
6002
6003
6004
6005
6006
6007
6008
6009
6010
7000
7001
7002
7003
7004
7005
7006
7007
7008
7009
10000
20000

基础软件端口
6379
3306
21707
9092

钱包节点端口
8333
8334
…

我只能记住这么多，不够的你再找找。

#####7.2、后台登录，验证码错误，Session无效问题

后台管理有时会出现无法登录的情况，要么验证码错误，要么无法通过第二步登录。

打开Chrome的调试控制台，我们会看到如下黄色警告：

A cookie associated with a cross-site resource at http://49.234.13.106/ was set without the `SameSite` attribute. It has been blocked, as Chrome now only delivers cookies with cross-site requests if they are set with `SameSite=None` and `Secure`. You can review cookies in developer tools under Application>Storage>Cookies and see more details at https://www.chromestatus.com/feature/5088147346030592 and https://www.chromestatus.com/feature/5633521622188032.
一般遇到这种情况的，你用的浏览器一般是Chrome，并且版本很高，因为Chorme在77版本之后，加入了上面说的配置：SameSite。如果你不配置这个东西，就让你死活无法携带cookie，这样session就没法使用了。

B资源新版chrome跨域问题：cookie之SameSite属性

作者在文中提供了多种解决方案，其中禁用Chrome浏览器的Samesite方法，是最快的。。。毕竟只是后台管理有这个问题，就辛苦一下自己了。

####   7.3、 Nginx新增TCP转发支持

这不是一个通用的问题，是我个人为了开发遇到的问题。

背景
针对APP的Socket推送需要开放28901端口，这个在微服务服务器上是自动监听的，APP也能收到Socket推送。
但是我在开发合约的时候，把合约的Jar放在另一台服务器上，那么监听的就是另一台服务器端口，为了统一Socket端口，就需要用nginx做端口转发。

配置方法
Nginx端口转发配置很简单，在nginx.conf文件新增如下片段即可：

stream {
    server {
       listen 38901; 
       proxy_connect_timeout 2s;
       proxy_timeout 10s;
       proxy_pass 172.17.0.4:38901;    
    }
    server {
       listen 38985; 
       proxy_connect_timeout 2s;
       proxy_timeout 10s;
       proxy_pass 172.17.0.4:38985;    
    }
    server {
       listen 48901; 
       proxy_connect_timeout 2s;
       proxy_timeout 10s;
       proxy_pass 172.17.0.4:48901;    
    }
    server {
       listen 48985; 
       proxy_connect_timeout 2s;
       proxy_timeout 10s;
       proxy_pass 172.17.0.4:48985;    
    }
}
但是如果需要Nginx支持TCP转发，需要安装stream模块，安装步骤如下：

1. 查看nginx版本模块
[root@VM_0_15_centos nginx]# nginx -V
nginx version: nginx/1.16.0
built by gcc 4.8.5 20150623 (Red Hat 4.8.5-36) (GCC)
built with OpenSSL 1.0.2k-fips 26 Jan 2017
TLS SNI support enabled
configure arguments: —prefix=/etc/nginx —sbin-path=/usr/sbin/nginx —modules-path=/usr/lib64/nginx/modules —conf-path=/etc/nginx/nginx.conf —error-log-path=/var/log/nginx/error.log —http-log-path=/var/log/nginx/access.log —pid-path=/var/run/nginx.pid —lock-path=/var/run/nginx.lock —http-client-body-temp-path=/var/cache/nginx/client_temp —http-proxy-temp-path=/var/cache/nginx/proxy_temp —http-fastcgi-temp-path=/var/cache/nginx/fastcgi_temp —http-uwsgi-temp-path=/var/cache/nginx/uwsgi_temp —http-scgi-temp-path=/var/cache/nginx/scgi_temp —user=nginx —group=nginx —with-compat —with-file-aio —with-threads —with-http_addition_module —with-http_auth_request_module —with-http_dav_module —with-http_flv_module —with-http_gunzip_module —with-http_gzip_static_module —with-http_mp4_module —with-http_random_index_module —with-http_realip_module —with-http_secure_link_module —with-http_slice_module —with-http_ssl_module —with-http_stub_status_module —with-http_sub_module —with-http_v2_module —with-mail —with-mail_ssl_module —with-stream —with-stream_realip_module —with-stream_ssl_module —with-stream_ssl_preread_module —with-cc-opt=’-O2 -g -pipe -Wall -Wp,-D_FORTIFY_SOURCE=2 -fexceptions -fstack-protector-strong —param=ssp-buffer-size=4 -grecord-gcc-switches -m64 -mtune=generic -fPIC’ —with-ld-opt=’-Wl,-z,relro -Wl,-z,now -pie’ —with-stream

2. 下载一个同版本可编译的Nginx
cd /opt
wget http://nginx.org/download/nginx-1.16.0.tar.gz
tar xf nginx-1.12..tar.gz && cd nginx-1.16.0

3. 备份原Nginx文件
mv /usr/sbin/nginx /usr/sbin/nginx.bak
cp -r /etc/nginx{,.bak}

4. 重新编译Nginx
根据第1步查到已有的模块，加上本次需新增的模块: —with-stream

cd /opt/nginx-1.12.
./configure —prefix=/etc/nginx —sbin-path=/usr/sbin/nginx —modules-path=/usr/lib64/nginx/modules —conf-path=/etc/nginx/nginx.conf —error-log-path=/var/log/nginx/error.log —http-log-path=/var/log/nginx/access.log —pid-path=/var/run/nginx.pid —lock-path=/var/run/nginx.lock —http-client-body-temp-path=/var/cache/nginx/client_temp —http-proxy-temp-path=/var/cache/nginx/proxy_temp —http-fastcgi-temp-path=/var/cache/nginx/fastcgi_temp —http-uwsgi-temp-path=/var/cache/nginx/uwsgi_temp —http-scgi-temp-path=/var/cache/nginx/scgi_temp —user=nginx —group=nginx —with-compat —with-file-aio —with-threads —with-http_addition_module —with-http_auth_request_module —with-http_dav_module —with-http_flv_module —with-http_gunzip_module —with-http_gzip_static_module —with-http_mp4_module —with-http_random_index_module —with-http_realip_module —with-http_secure_link_module —with-http_slice_module —with-http_ssl_module —with-http_stub_status_module —with-http_sub_module —with-http_v2_module —with-mail —with-mail_ssl_module —with-stream —with-stream_realip_module —with-stream_ssl_module —with-stream_ssl_preread_module —with-cc-opt=’-O2 -g -pipe -Wall -Wp,-D_FORTIFY_SOURCE=2 -fexceptions -fstack-protector-strong —param=ssp-buffer-size=4 -grecord-gcc-switches -m64 -mtune=generic -fPIC’ —with-ld-opt=’-Wl,-z,relro -Wl,-z,now -pie’ —with-stream

以上编译时，如出现缺少依赖，一般需要安装以下模块，安装完再次编译：

yum -y install libxml2 libxml2-dev libxslt-devel
yum -y install gd-devel
yum -y install perl-devel perl-ExtUtils-Embed
yum -y install GeoIP GeoIP-devel GeoIP-data
yum -y install pcre-devel
yum -y install openssl openssl-devel gperftools

5. 编译通过，继续验证
继续输入：make
make完成后不要继续输入“make install”，以免现在的nginx出现问题
以上完成后，会在objs目录下生成一个nginx文件，先验证：

/opt/nginx-1.16.0/objs/nginx -t
/opt/nginx-1.16.0/objs/nginx -V

6. 替换Nginx文件并重启
cp /opt/nginx-1.16.0/objs/nginx /usr/sbin/
nginx -s reload

7. 检查
[root@pre ~]# nginx -V
https://b.weixiaolive.com/post/533.html

####   八、一些业务逻辑说明

用户注册业务逻辑
![输入图片说明](image-%E7%94%A8%E6%88%B7%E6%B3%A8%E5%86%8C%E9%80%BB%E8%BE%91.png)

可能遇到的问题是什么？
对于懂得程序设计的人来说，用户注册业务逻辑可以自行脑补出来，必然是生成用户记录和用户相关的一些初始化记录（member表和member_wallet表）。
这里需要注意的就是第五步，通过Kafka来实现钱包记录的异步生成，有些同学在部署过程中会遇到注册成功，但是没有生成【用户-钱包】记录，看自己的资产管理，还是什么记录都没有，那么问题的可能性有两个：要么你的wallet.jar没跑起来，要么你的kafka没有正常运行。

为什么使用Kafka异步生成记录？
本系统初期在用户注册时候，就会生成BTC、ETH等币种的钱包地址，这是一个需要调用节点RPC的操作，也是一个耗时的操作，当同一时间有很多用户注册的时候，这里会成为一个性能的瓶颈，所以使用kafka通知的方式，实现了异步。
当然，后面我对这部分的业务逻辑又进行了优化，用户注册成功以后，并不是立即生成钱包地址，而是当用户点击某个币种的充值按钮的时候才生成，这样会大幅降低钱包节点的负担，同时也为运维人员管理钱包减少了不必要的复杂度。

用户注册成功的意义
用户注册是检查你的系统是否通畅的关键一步，当你打通了这一步，说明你的短信、数据库、Kafka、Ucenter-api.jar、wallet.jar等都是正常运行的了，你就可以安然的进行其他更加复杂的操作。



#### 模块介绍
cloud
提供SpringCloud微服务注册中心功能，为基础模块，必须部署
依赖服务：无
ucenter-api
提供用户相关的接口（如登录、注册、资产列表）,该模块为基础为基础模块，必须部署
依赖服务：mysql,kafka,redis,mongodb,短信接口，邮箱账号
otc-api
提供场外交易功能接口，没有场外交易的可以不部署
依赖服务：mysql,redis,mongodb,短信接口
exchange-api
提供币币交易接口，没有币币交易的项目可以不部署
依赖服务：mysql,redis,mongodb,kafka
chat
提供实时通讯接口，基础模块，需要部署
依赖服务：mysql,redis,mongodb
admin
提供管理后台的所有服务接口，必须部署
依赖服务：mysql,redis,mongodb
wallet
提供充币、提币、获取地址等钱包服务，为基础模块，必须部署
依赖服务：mysql,mongodb,kafka,cloud
market
提供币种价格、k线、实时成交等接口服务，场外交易不需要部署
依赖服务：mysql,redis,mongodb,kafka,cloud
exchange
提供撮合交易服务，场外交易不需要部署
依赖服务：mysql,mongodb,kafka
重点业务介绍
后端框架的核心模块为 exchange,market模块。

其中exhcnge模块完全采用Java内存处理队列,大大加快处理逻辑,中间不牵涉数据库操作,保证处理速度快,其中项目启动后采用继承ApplicationListener方式，自动运行；

启动后自动加载未处理的订单,重新加载到JVM中，从而保证数据的准确，exchange将订单处理后，将成交记录发送到market;

market模块主要都是数据库操作，将用户变化信息持久化到数据库中。主要难点在于和前端交互socket推送，socket推送采用两种方式，web端socket采用SpringSocket，移动端采用Netty推送,其中netty推送通过定时任务处理。


#### jar包启动顺序
nohup  java  -jar  /web/app/cloud.jar  >/dev/null 2>&1 &
nohup  java  -jar  /web/app/exchange.jar  >/dev/null 2>&1 &
nohup  java  -jar  /web/app/market.jar  >/dev/null 2>&1 &
nohup  java  -jar  /web/app/exchange-api.jar  >/dev/null 2>&1 &
nohup  java  -jar  /web/app/ucenter-api.jar  >/dev/null 2>&1 &
nohup  java  -jar  /web/app/otc-api.jar  >/dev/null 2>&1 & 
nohup  java  -jar  /web/app/chat.jar  >/dev/null 2>&1 & 
nohup  java  -jar  /web/app/wallet.jar  >/dev/null 2>&1 & 
nohup  java  -jar  /web/app/admin.jar  >/dev/null 2>&1 &



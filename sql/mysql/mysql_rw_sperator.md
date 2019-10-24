# mysql读写分离

## MySql 读写分离
由于业务增长需求，需要在世界各地大量访问数据。为了实现数据的快速访问，针对数
据中大量的读取需求，使用主从分布以及读写分离的分布方式。
读写分离：访问数据时使用中间代理将数据操作进行读写分离，根据访问类型分别访问不同
数据库。

## 雅虎360的开源代理Atlas
当有服务需要访问数据时，其首先访问最近的mysql代理`Atlas`，mysql代理根据操作具体类型
再分别将请求发送到主从数据库，读请求发送到从库，写请求发送到主库。

## 安装配置
1. Atlas 第三方库依赖
```
apt-get install glib2.0
apt-get install libevent-dev
apt-get install libjemalloc-dev
apt-get install libmysqlclient-dev
```
2. Atlas 目录
```
bin     #可执行程序目录
conf    #配置文件目录
log     #日志目录
```

## 配置文件
`Atlas`配置文件`conf/db_proxy.cnf`配置如下：
```
admin-username = username   # 管理接口的用户名
admin-password = password   # 管理接口的密码

# Atlas 后端连接的MySQL 主库的IP 和端口，可设置多项，用逗号分隔
proxy-backend-addresses = ip:port        

# Atlas 后端连接的MySQL 从库的IP和端口，@后面的数字代表权重，用来作负载均衡，若省略则默认为1，可设置多项，用逗号分隔
proxy-read-only-backend-addresses = ip:port@1   

# 用户名与其对应的加密过的MySQL 密码，密码使用PREFIX/bin 目录下的加密程序encrypt加密，下行的user1 和user2 为示例，将其替换为你的MySQL 的用户名和加密密码！
pwds = user1:xxx, user2:xxx

# 设置Atlas 的运行方式，设为true 时为守护进程方式，设为false 时为前台方式，一般开发调试时设为false，线上运行时设为true,true 后面不能有空格。
daemon = true

# 设置Atlas 的运行方式，设为true 时Atlas 会启动两个进程，一个为monitor，一个为worker，monitor 在worker 意外退出后会自动将其重启，设为false时只有worker，没有monitor，一般开发调试时设为false，线上运行时设为true,true 后面不能有空格。
keepalive = true

# 工作线程数，对Atlas 的性能有很大影响，可根据情况适当设置，推荐设置为CPU 核数
event-threads = 8

# 日志级别，分为message、warning、critical、error、debug 五个级别
log-level = message

# 日志存放的路径
log-path = /usr/local/mysql-proxy/log

# SQL 日志的开关，可设置为OFF、ON、REALTIME，OFF 代表不记录SQL 日志，ON 代表记录SQL 日志，REALTIME 代表记录SQL 日志且实时写入磁盘，默认为OFF
sql-log = REALTIME

# 慢日志输出设置。当设置了该参数时，则日志只输出执行时间超过sql-log-slow（单位：ms)的日志记录。不设置该参数则输出全部日志。
sql-log-slow = 10

# 实例名称，用于同一台机器上多个Atlas 实例间的区分
instance = db_proxy

# Atlas 监听的工作接口IP 和端口
proxy-address = 127.0.0.1:1234

#Atlas 监听的管理接口IP 和端口
admin-address = 127.0.0.1:2345
```

## 中间代理操作
1. 启动Atlas
```
./mysql-proxyd db_proxy start   # mysql-proxyd：程序名，db_proxy：实例名
```
2. 重启Atlas
```
./mysql-proxyd db_proxy restart
```
3.停止Atlas
```
./mysql-proxyd db_proxy stop
```
4. 加密mysql 密码
```
./encrypt password  # 加密后的密码作为配置文件中pwds字段的加密密码
```

5、启动Atlas 之后进入管理界面查看状态
```
mysql -uusername -ppassword -h127.0.0.1 -P2345
> SELECT * FROM BACKENDS;
```

6. 创建mysql 访问账号
```
> GRANT ALL ON *.* to 'xxx'@'%' IDENTIFIED BY 'xxx';
> FLUSH PRIVILEGES;
```
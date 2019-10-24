# mysql主从配置

## 主从分布
由于业务增长需求，需要在世界各地大量访问数据。为了实现数据的快速访问，针对数据中大量的读取需求，采用主从分布以及读写分离的分布方式。
`mysql`数据库使用一主多从的配置，配置完成后，数据修改只在主库`master`执行，所有其它的从库`slave`只负责对`master`数据的备份。

## 主从配置
```
符号‘#’标识shell 命令行下命令，符号‘>’标识mysql 下命令。
```
### master 配置
1. 修改配置文件/etc/mysql/my.cnf
```
[mysqld]
server-id = 1 （任意自然数n，只要保证两台MySQL 主机不重复就可以了）
log-bin = mysql-bin （开启二进制日志）
log-bin-index = mysql-bin.index
binlog-do-db = iviewsdb （指定mysql 的binlog 日志记录哪个db）
#binlog-ignore-db = mysql （指定mysql 的binlog 日志忽略哪个db）
expire_logs_days = 10 （表明距离当前时间正好n 天前的二进制文件会被系统自动删除，
二进制文件千万不要手动删除）
max_binlog_size = 100M （单个日志文件最大字节设置最大100MB）
sync_binlog = 100 （数据100 条时刷新到磁盘）
innodb_flush_log_at_trx_commit = 2 （数据刷新到磁盘，flush 每秒执行一次）
```
2. 配置完成重启`mysql`
```
# service mysql restart
```
3. 创建一个用于`slave`用户连接的用户
```
> GRANT REPLICATION SLAVE ON *.* TO 'xxx'@'%' IDENTIFIED BY 'xxx';
> FLUSH PRIVILEGES;
```
4. mysql备份
    1. 全局读锁定
    ```
    > FLUSH TABLES WITH READ LOCK;
    ```
    2. 导出指定数据库的数据
    ```
    # mysqldump -uroot -p123456 --databases iviewsdb > ./iviewsdb.sql
    ```
    2. 查看日志状态
    ```
    > SHOW MASTER STATUS;
    ```
    3. 解除锁定
    ```
    > UNLOCK TABLES;
    ```
5. 查看当主库中当前的SLAVE 列表
```
> SHOW SLAVE HOSTS;
```

### slave配置
1. 修改/etc/mysql/my.cnf
```
[mysqld]
server-id = 2 （任意自然数n，只要保证两台MySQL 主机不重复就可以了）
sync_binlog = 100 （数据100 条时刷新到磁盘）
innodb_flush_log_at_trx_commit = 2 （数据刷新到磁盘，flush 每秒执行一次）
```
2. 配置完成后重启mysql
```
# service mysql restart
```
3. 创建并导入数据库
```
> CREATE DATABASE iviewsdb DEFAULT CHARSET UTF8;
# mysql -uroot -p123456 iviewsdb < iviewsdb.sql
```
4. 设置备份
```
> CHANGE MASTER TO MASTER_HOST='xxx.xxx.xxx.xxx',
MASTER_PORT=3306,
MASTER_USER='slave_user_name',
MASTER_PASSWORD='slave_user_passwd',
MASTER_LOG_FILE='mysql-bin.000001',
MASTER_LOG_POS=0;
```
5. 开启备份
```
> START SLAVE;
```
6. 查看备份状态
```
> SHOW SLAVE STATUS\G;
```
7. 停止备份
```
> STOP SLAVE;
```
8. 清空备份
```
> RESET SLAVE ALL;
```
## mysql官方文档
1. innodb lock
https://dev.mysql.com/doc/refman/5.7/en/innodb-locking.html
2. Phantom Rows
https://dev.mysql.com/doc/refman/5.7/en/innodb-nex…
3. Clustered and Secondary Indexes
https://dev.mysql.com/doc/refman/8.0/en/innodb-ind…

## 技术博客
1. 加锁原理
http://hedengcheng.com/?p=771#_Toc374698322

## mysql：
select CURRENT_TIMESTAMP; 查看当前时间 '2018-11-15 19:45:53'
select UNIX_TIMESTAMP(); 查看时间戳 1542282369


## 查看mysql安装源
/etc/yum.repos.d
yum repolist all | grep mysql

## 安装mariadb-server
yum install -y mariadb-server

## 启动服务
systemctl start mariadb.service

## 添加到开机启动
systemctl enable mariadb.service

## 创建表
CREATE TABLE `question` (
  `id` bigint(20) NOT NULL AUTO_INCREMENT COMMENT '自增id',
  `star` int(11) NOT NULL DEFAULT '1' COMMENT '星级 1 2 3 4',
  `title` varchar(255) NOT NULL COMMENT '标题',
  PRIMARY KEY (`id`),
  UNIQUE KEY `i_title` (`title`) USING BTREE COMMENT '题目索引'
) ENGINE=InnoDB AUTO_INCREMENT=10 DEFAULT CHARSET=utf8mb4 COMMENT='题库主表';

## 删除表中数据，并且自动增加字段的默认初始值重新从1开始
TRUNCATE TABLE

## 删除整张表
DROP TABLE 

## 查询最后插入的ID
SELECT LAST_INSERT_ID(id);




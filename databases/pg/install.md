# postgre数据库

1. 安装
brew install postgresql

2. 卸载
brew reinstall postgresql

3. 格式化编码
initdb /usr/local/var/postgres/ -E utf8
initdb /usr/local/var/postgres/ -E utf8 --locale=zh_CN.UTF-8


4. 启动、停止
pg_ctl -D /usr/local/var/postgres -l /usr/local/var/postgres/server.log start
pg_ctl -D /usr/local/var/postgres stop -s -m fast

5. 创建用户
createuser develop -P

6. 创建数据库
命令行： createdb develop_data -O develop -E utf8 -e
数据库： create database develop_data;

7. 导出sql创建命令
pg_dump -U develop develop_data -W -s -t develop_table >> table.sql


8. 连接数据库
psql -U develop -d develop_data -h 127.0.0.1

9. 查询`schemaname`中表
select tablename from pg_tables where schemaname='storage';

10. 查看postgres权限
\du

11. 查看扩展
\dFp


12. 更新自增ID值
SELECT SETVAL('my_table_id_seq',(SELECT MAX(id) from my_table));



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

12. 安装相关插件
1. `pg_trgm`模糊（like）搜索索引（可对拼音、数字进行分词）
- 安装`pg_trm`插件
	create extension pg_trgm;
- 创建表的相关索引
	CREATE INDEX t1_c1_gin_idx ON t1 USING gin(c1 gin_trgm_ops);

2. `zhparser`中文索引（可对中文进行分词）
- 安装`zhparser`插件
	create extension zhparser;
- 创建使用`zhparser`作为解析器的全文搜索的相关配置
	CREATE TEXT SEARCH CONFIGURATION zhcfg (PARSER = zhparser);
- 往全文搜索配置中增加token映射
	ALTER TEXT SEARCH CONFIGURATION zhcfg ADD MAPPING FOR n,v,a,i,e,l WITH simple;
- 创建表的相关索引
	CREATE INDEX t1_c1_fts_idx ON t1 USING GIN(TO_TSVECTOR('zhcfg', c1))

3. 相关函数
- 查看表数据大小
	select count(*) from t1;
	select pg_table_size(`table`);
- 查询索引大小
	select pg_relation_size('t1_c1_gin_idx');
- 拆词结果查询
	select show_trgm('123');
- 中文插件模糊分词匹配
	select to_tsvector('zhcfg', '活塞环套'), plainto_tsquery('zhcfg', '活塞环'), to_tsvector('zhcfg', '活塞环套') @@ plainto_tsquery('zhcfg', '活塞环') AS flag;

4. 创建索引（带有附加条件的方式）
	- 创建唯一索引
		CREATE UNIQUE INDEX IF NOT EXISTS name_key
		ON students (name, (score > 0)) WHERE name != ''

	- 创建`pg_trgm`索引
		CREATE EXTENSION IF NOT EXISTS pg_trgm;
		CREATE INDEX IF NOT EXISTS parts_mnemonic ON parts USING gin (mnemonic gin_trgm_ops);

	- 创建`zhparser`索引
		CREATE EXTENSION IF NOT EXISTS zhparser;
		-- DROP TEXT SEARCH CONFIGURATION IF exists zhcfg;
		-- CREATE TEXT SEARCH CONFIGURATION zhcfg (PARSER = zhparser);
		-- ALTER TEXT SEARCH CONFIGURATION zhcfg ADD MAPPING FOR n,v,a,i,e,l WITH simple;
		CREATE INDEX IF NOT EXISTS veh_name_index ON public.veh_models
		USING GIN(TO_TSVECTOR('zhcfg', name));

# postgres 索引创建


## `pg_trgm`插件
- 可用于创建模糊搜索索引（可对拼音、数字进行分词），提升模糊搜索效率（like）

1. 安装`pg_trm`插件
create extension pg_trgm;
2. 创建表的相关索引
CREATE INDEX t1_c1_gin_idx ON t1 USING gin(c1 gin_trgm_ops);


## `zhparser`中文索引（可对中文进行分词）
1. 安装`zhparser`插件
create extension zhparser;
2. 创建使用`zhparser`作为解析器的全文搜索的相关配置
CREATE TEXT SEARCH CONFIGURATION zhcfg (PARSER = zhparser);
3. 往全文搜索配置中增加token映射
ALTER TEXT SEARCH CONFIGURATION zhcfg ADD MAPPING FOR n,v,a,i,e,l WITH simple;
4. 创建表的相关索引
CREATE INDEX t1_c1_fts_idx ON t1 USING GIN(TO_TSVECTOR('zhcfg', c1))

## 相关函数
1. `pg_table_size`查看表数据大小
select count(*) from t1;
select pg_table_size(`table`);
2. `pg_relation_size`查询索引大小
select pg_relation_size('t1_c1_gin_idx');
3. `show_trgm`查看拆词结果（仅针对`pg_trgm`插件）
select show_trgm('123');
4. `to_tsvector`查询中文拆词结果（仅针对`zhparser`插件）
select to_tsvector('zhcfg', '活塞环套');
5. `plainto_tsquery`获取中文拆词字符串
select plainto_tsquery('zhcfg', '活塞环');
```
 '活塞环 & 活塞'
```
6. 中文模糊匹配
select to_tsvector('zhcfg', '活塞环套') @@ plainto_tsquery('zhcfg', '活塞环') AS flag;


## 针对`table`创建索引
1. 创建唯一索引（带有附加条件的方式）
CREATE UNIQUE INDEX IF NOT EXISTS name_key ON students (name, (score > 0)) WHERE name != ''

2. 创建`pg_trgm`索引
CREATE EXTENSION IF NOT EXISTS pg_trgm;
CREATE INDEX IF NOT EXISTS parts_mnemonic ON parts USING gin (mnemonic gin_trgm_ops);

3. 创建`zhparser`索引
CREATE EXTENSION IF NOT EXISTS zhparser;
-- DROP TEXT SEARCH CONFIGURATION IF exists zhcfg;
-- CREATE TEXT SEARCH CONFIGURATION zhcfg (PARSER = zhparser);
-- ALTER TEXT SEARCH CONFIGURATION zhcfg ADD MAPPING FOR n,v,a,i,e,l WITH simple;
CREATE INDEX IF NOT EXISTS veh_name_index ON public.veh_models USING GIN(TO_TSVECTOR('zhcfg', name));

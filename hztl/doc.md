# hztl

## code path
```
git clone https://gitlab.hztl3.xyz/go/basic.git
git clone https://gitlab.hztl3.xyz/go/accounts.git
git clone https://gitlab.hztl3.xyz/go/goods-stocks.git
git clone https://gitlab.hztl3.xyz/go/erp.git
git clone https://gitlab.hztl3.xyz/go/admin.git
git clone https://gitlab.hztl3.xyz/go/libs.git
git clone https://gitlab.hztl3.xyz/go/chats.git
git clone https://gitlab.hztl3.xyz/go/im.git
git clone https://gitlab.hztl3.xyz/go/wants.git

git clone git@gitlab.hztl3.xyz:go/basic.git
git clone git@gitlab.hztl3.xyz:go/accounts.git
git clone git@gitlab.hztl3.xyz:go/goods-stocks.git
git clone git@gitlab.hztl3.xyz:go/erp.git
git clone git@gitlab.hztl3.xyz:go/admin.git
git clone git@gitlab.hztl3.xyz:go/libs.git
git clone git@gitlab.hztl3.xyz:go/chats.git
git clone git@gitlab.hztl3.xyz:go/im.git
git clone git@gitlab.hztl3.xyz:go/wants.git
```


## xiaomei
```
go get -u -v github.com/lovego/xiaomei
go get -u -v github.com/lovego/gospec

xiaomei godoc run 启动一个godoc 本地服务器
http://localhost:1234/pkg/database/sql/driver/#Valuer


启动qa2
xiaomei app deploy qa2
xiaomei --help
执行程序
xiaomei app exec
进入数据库
xiaomei psql qa2
单元测试
xiaomei cover
go test -run xxx
检查代码规范
xiaomei app compile
指定环境运行程序，主要用于生成sql表
GOENV=test xiaomei app exec
```

## sql 相关操作
```
ALTER TABLE parts ADD cid int8 NOT NULL default 0;
CREATE UNIQUE INDEX IF NOT EXISTS example_cid_key ON parts (cid) WHERE cid < 2;

ALTER TABLE parts DROP group_id;
ALTER TABLE parts ADD group_id int8 NOT NULL default 0;
ALTER TABLE parts ADD stype varchar(100) NOT NULL default '';
ALTER TABLE parts ADD rate numeric(15,6) NOT NULL default 0;

select id, models && ARRAY['ab','12'] from parts where id=1;
UPDATE parts set cid = COALESCE((select cid from replacements where id=log_id), 0);
```

## 表拷贝，并更新子增长值
```
TRUNCATE parts RESTART IDENTITY;
INSERT INTO parts SELECT id,'',name FROM replacements;
SELECT SETVAL('parts_id_seq', (SELECT MAX(id) FROM replacements));
```

# 程序
```
jiarong.tian@hztl3.com

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



```
连接qa2服务器
ssh develop@124.239.245.250

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


xiaomei app copy2vendor hztl3.com/libs/accounts/account_sets/perms/tree


http://localhost:1234/pkg/database/sql/driver/#Valuer
xiaomei godoc run 启动一个godoc 本地服务器

go get -u -v github.com/lovego/xiaomei
go get -u -v github.com/lovego/gospec


docker logs -f -t --since="2018-02-08" --tail=100 CONTAINER_ID
docker logs erp-qa2-app.5003 -f
docker logs -f erp-qa2-app.4004


go get -u -v github.com/lovego/xiaomei
更新一下xiaomei, 增加了 psql recreate 命令，删除数据库，重新创建数据库和表。

mac安装gnu shell
brew install coreutils


穿透网络
程序
ssh -NL 21768:127.0.0.1:21768 develop@124.239.245.250
数据迁移
ssh -NL 1433:192.168.100.229:1433 develop@124.239.245.250
sqlcmd -S 192.168.100.229,1433 -d NB_BJ_0100 -U readonly -P 'x$256wr'
```

```
样例
ALTER TABLE financials.statements ADD cooperator_id int8 NOT NULL default 0;
CREATE UNIQUE INDEX IF NOT EXISTS statements_account_set_id_company_id_cooperator_id_key
ON financials.statements (account_set_id, company_id, cooperator_id) WHERE status < 2;
```

select id,cus_veh_models, cus_veh_models && ARRAY['ab','12'] from goods.parts where id=1;

```
TRUNCATE goods.part_groups RESTART IDENTITY;
INSERT INTO goods.part_groups SELECT id,account_set_id,name,'',created_by,created_at,created_by,created_at FROM goods.replacements;
SELECT SETVAL('goods.part_groups_id_seq',(SELECT MAX(id) FROM goods.part_groups));

TRUNCATE goods.part_group_details RESTART IDENTITY;
INSERT INTO goods.part_group_details(group_id,account_set_id,code,brand,production_place,remark,created_by,created_at,updated_by,updated_at) SELECT group_id,account_set_id,code,brand,production_place,'',created_by,created_at,updated_by,updated_at FROM goods.parts WHERE group_id>0 AND is_deleted=false;
SELECT SETVAL('goods.part_group_details_id_seq',(SELECT MAX(id) FROM goods.part_group_details));

ALTER TABLE goods.parts DROP group_id;
ALTER TABLE goods.parts ADD group_id int8 NOT NULL default 0;



ALTER TABLE financials.company_fund_accounts DROP payment_method;
ALTER TABLE financials.logs ADD enhanced_settlement_type varchar(100) NOT NULL default '';
ALTER TABLE financials.receivable_payables ADD tax_rate numeric(15,6) NOT NULL default 0;




ALTER TABLE financials.log_subs ADD account_set_id int8 NOT NULL default 0;
ALTER TABLE financials.receivable_payable_details ADD account_set_id int8 NOT NULL default 0;
ALTER TABLE financials.statement_details ADD account_set_id int8 NOT NULL default 0;
ALTER TABLE financials.statement_subs ADD account_set_id int8 NOT NULL default 0;

update financials.log_subs set account_set_id = COALESCE((select account_set_id from financials.logs where id=log_id), 0);
update financials.receivable_payable_details set account_set_id = COALESCE((select account_set_id from financials.receivable_payables where id=payment_id), 0);
update financials.statement_details set account_set_id = COALESCE((select account_set_id from financials.statements where id=statement_id), 0);
update financials.statement_subs set account_set_id = COALESCE((select account_set_id from financials.statements where id=statement_id), 0);


ALTER TABLE financials.receivable_payable_details ADD company_id int8 NOT NULL default 0;
ALTER TABLE financials.statement_details ADD company_id int8 NOT NULL default 0;
ALTER TABLE financials.statement_subs ADD company_id int8 NOT NULL default 0;

update financials.receivable_payable_details set company_id = COALESCE((select company_id from financials.receivable_payables where id=payment_id), 0);
update financials.statement_details set company_id = COALESCE((select company_id from financials.statements where id=statement_id), 0);
update financials.statement_subs set company_id = COALESCE((select company_id from financials.statements where id=statement_id), 0);
```













# migrate

## mac
1. 安装
 - 编译安装
 	+ go get -u -d github.com/mattes/migrate/cli
	+ go get -u -d github.com/go-sql-driver/mysql
	+ go build -tags 'mysql' -o /usr/local/bin/migrate github.com/mattes/migrate/cli
 - 直接安装
 	+brew install golang-migrate

## 创建`sqlschema`目录
1. 文件命名规则： 1_init.up.sql、2_update.up.sql、3_add.up.sql
2. 文件内容：
```
CREATE TABLE `currency` (
  `id`          integer NOT NULL AUTO_INCREMENT,
  `date`        varchar(255) NOT NULL,
  `type`        varchar(255) NOT NULL,
  `rate`        decimal(18,6) NOT NULL,
  PRIMARY KEY (`id`),
  UNIQUE KEY `date_type_key` (`date`, `type`)
) ENGINE=InnoDB DEFAULT CHARSET=utf8mb4;
```

## 运行 migrations
1. migrate to version x (use with caution)
migrate -database="mysql://user_name:user_passwd@tcp(127.0.0.1:3306)/db_name" -source=file:// goto x

# migrate version up by x (e.g. n -> n + x)
migrate -database="mysql://root:jJKcblxDIniDEEJB@tcp(127.0.0.1:3306)/newd_dev" -source=file:// up x

# migrate version down by x (e.g. n -> n - x)
migrate -database="mysql://root:jJKcblxDIniDEEJB@tcp(127.0.0.1:3306)/newd_dev" -source=file:// down x

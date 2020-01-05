# CMD

## 常用命令
- 创建用户账号		CREATE USER ben IDENTIFIED BY 'p@$word';
- 重命名账户		RENAME USER ben TO myuser;
- 删除账户		DROP USER myuser;
- 查看用户		SELECT host, user FROM mysql.user;
- 显示用户权限		SHOW GRANTS FOR myuser@localhost;
- 设置用户权限 	GRANT SELECT ON database.* TO myuser;
- 设置用户权限		GRANT ALL ON database.* TO myuser;
- 移除用户权限		REVOKE SELECT ON database.* FROM myuser;
- 移除用户权限 	REVOKE ALL ON database.* FROM myuser;
- 修改用户密码 	SET PASSWORD = Password('new p@$word');

## 非常用命令
- 查看表状态		ANALYZE TABLE my_table;
- 查看表状态		CHECK TABLE my_table;
- 查看变量		SHOW VARIABLES;
- 查看状态 		SHOW STATUS;






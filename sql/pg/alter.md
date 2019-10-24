# ALTER

## 表的重命名
ALTER TABLE table_name RENAME TO new_name;

## 增加一列
ALTER TABLE table_name ADD column_name datatype { NOT NULL default ''};

## 删除一列
ALTER TABLE table_name DROP column_name;

## 更改列的名字
ALTER TABLE table_name RENAME column_name to new_column_name;

## 更改列的数据类型
ALTER TABLE table_name ALTER column_name TYPE datatype;

## 字段的notnull设置
ALTER TABLE table_name ALTER column_name {SET|DROP} NOT NULL;

## 给列添加default
ALTER TABLE table_name ALTER column_name SET DEFAULT expression;

## 添加唯一约束
ALTER TABLE table_name ADD CONSTRAINT constraint_name UNIQUE(id);

## 删除约束
ALTER TABLE table_name DROP CONSTRAINT constraint_name;


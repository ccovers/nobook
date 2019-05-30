# POSTGRESSQL

## UPSERT特性
当插入SQL违反约束的情况下，支持定义动作，而不抛出错误
```
INSERT ...
ON CONFLICT DO UPDATE/IGNORE/NOTHING ...
RETURN ...
```

## EXCLUDED 关键字
当插入SQL违反约束的情况下，需要更新时，代表更新的语句
```
INSERT INTO table AS t(id, name, num)
ON CONFLICT DO UPDATE
SET name=EXCLUDED.name, num=t.num+EXCLUDED.num
```

# TO_CHAR 函数
将类型转换为字符串
```
SELECT TO_CHAR(bill_date, 'YYYY-MM-DD') AS bill_date,
FROM table
WHERE id IN (1, 2, 3)
ORDER BY created_at DESC;
```


# COALESCE 函数
依次参考各参数表达式，遇到非null值即停止并返回该值。
```
 COALESCE(expression_1, expression_2, ...,expression_n)

 SELECT COALESCE(name, 'opgo') AS name FROM table;
```

# CONCAT 函数
字符串拼接，连接多个字符串（字段）
```
CONCAT(char c1, char c2, ..., char cn)

SELECT CONCAT(id, ':', name) AS key FROM table;
SELECT id+':'+name AS key FROM table;
```

# SELECT DISTINCT ON (expression[...])
把记录数据根据`[...]`的值进行分组，分组之后仅返回每一组的第一行。若未指定`ORDER BY`语句，返回的第一条是不确定的，若使用了`ORDER BY`语句，那么[...]里面的值必须靠近`ORDER BY`子句最左边
```
SELECT DISTINCT ON(name)
	id, name
FROM table
ORDER BY name, id
```



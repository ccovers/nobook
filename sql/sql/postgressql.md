# POSTGRESSQL

## 优化
```
EXPLAIN ANALYZE SELECT * FROM table;
```

## UPSERT特性
当插入SQL违反约束的情况下，支持定义动作，而不抛出错误
```
INSERT ...
ON CONFLICT DO UPDATE/IGNORE/NOTHING ...
RETURNING ...

INSERT INTO table AS t(id, name, created_at) VALUES (1, 'dong', NOW()),(2, 'hong', NOW())
ON CONFLICT(id) WHERE id < 100 DO UPDATE
SET name=EXCLUDED.name, created_at=EXCLUDED.created_at;
```

## EXCLUDED 关键字
当插入SQL违反约束的情况下，需要更新时，代表更新的语句
```
INSERT INTO table AS t(id, name, num)
ON CONFLICT DO UPDATE
SET name=EXCLUDED.name, num=t.num+EXCLUDED.num
```

## TO_CHAR 函数
将类型转换为字符串
```
SELECT TO_CHAR(bill_date, 'YYYY-MM-DD') AS bill_date,
FROM table
WHERE id IN (1, 2, 3)
ORDER BY created_at DESC;
```

## COALESCE 函数
依次参考各参数表达式，遇到非null值即停止并返回该值。
```
 COALESCE(expression_1, expression_2, ...,expression_n)

 SELECT COALESCE(name, 'opgo') AS name FROM table;
```

## CONCAT 函数
字符串拼接，连接多个字符串（字段）
```
CONCAT(char c1, char c2, ..., char cn)

SELECT CONCAT(id, ':', name) AS key FROM table;
SELECT id+':'+name AS key FROM table;
```

## SELECT DISTINCT ON (expression[...])
把记录数据根据`[...]`的值进行分组，分组之后仅返回每一组的第一行。若未指定`ORDER BY`语句，返回的第一条是不确定的，若使用了`ORDER BY`语句，那么[...]里面的值必须靠近`ORDER BY`子句最左边
```
SELECT DISTINCT ON(name)
	id, name
FROM table
ORDER BY name, id
```

## WITH
- POSITION(UPPER('明') IN UPPER(name)) 判断给定字符在字段中的位置
- LENGTH(name) 判断字段长度
```
WITH temp(id, name, phone, address)
	AS (VALUES(1,'小明',15982195424,'四川'),(2,'小红',15982195424,'四川'))
SELECT * FROM temp;


SELECT * FROM
(VALUES(1,'小明啊',15982195424,'四川'),(2,'小明',15982195424,'四川'),(3,'小红',15982195424,'四川'))
AS t(id, name, phone, address)
ORDER BY POSITION(UPPER('明') IN UPPER(name)),LENGTH(name);
```

## FILTER
过滤
```
SELECT SUM(amount) FILTER (WHERE payment_type = 0) AS amount FROM logs;
```

## CASE WHEN THEN
根据条件选择合适的值
```
SELECT
	id, name,
	(
		CASE
			WHEN flag=0 then amount
			WHEN flag=1 then 0
		END
	) AS income,
	FROM
		recharge
	ORDER BY created_at ASC;
```

## OVER
针对数据排序并每行获取统计的累计值（前面所有行之和）
```
SELECT
	SUM(amount) OVER(PARTITION BY name ORDER BY created_at) AS balance
	FROM
		recharge
	ORDER BY name, created_at ASC;
```

## 递归查询 RECURSIVE
* table
```
table veh_models {
	id int64
	parent_id int64
	name string
}
```
* 查询所有父节点
```
	WITH RECURSIVE T AS (
		SELECT A.id, A.parent_id, A.name, ARRAY[A.id] AS path
		FROM veh_models A
		WHERE A.id = 3

		UNION ALL

		SELECT
			K.id, K.parent_id, K.name, K.id || T.path
		FROM veh_models K
		INNER JOIN T ON T.parent_id = K.id
	)
	SELECT * FROM T
	ORDER BY id;
```

* 查询所有子节点
```
	WITH RECURSIVE T AS(
		SELECT A.id, A.parent_id, A.name, ARRAY[A.id] AS path
		FROM veh_models AS A
		WHERE A.id = 3

		UNION ALL

 		SELECT
 			K.id, K.parent_id, K.name, T.path || K.id
		FROM veh_models K
		INNER JOIN T ON T.id = K.parent_id
	)
	SELECT * FROM T
	ORDER BY id;
```

* 查询所有子节点及路径、深度
```
	WITH RECURSIVE T AS (
    	SELECT A.id, A.parent_id, A.name, ARRAY[id] AS path, 1 AS depath
   		FROM veh_models AS A
    	WHERE A.id = 3

    	UNION ALL

    	SELECT K.id, K.parent_id, K.name, K.id || T.path, T.depath + 1 AS depath
    	FROM veh_models K
    	JOIN T ON T.id = K.parent_id
    )
    SELECT * FROM T
	ORDER BY path;
```

* 查询所有路径
```
	WITH RECURSIVE T AS (
		SELECT A.id, A.name, CAST(A.name AS VARCHAR(4000)) AS name_full_path
		FROM veh_models A
		WHERE A.parent_id = 0

		UNION ALL

		SELECT
			K.id, K.name,
			CAST(T.name_full_path || '/' || K.name AS VARCHAR (4000)) AS ame_full_path
		FROM veh_models K
		INNER JOIN T ON T.id = K.parent_id
	)
	SELECT * FROM T
	ORDER BY id;
```


* 表拷贝，并更新自增长值
```
TRUNCATE parts RESTART IDENTITY;
INSERT INTO parts SELECT id,'',name FROM replacements;
SELECT SETVAL('parts_id_seq', (SELECT MAX(id) FROM replacements));
```
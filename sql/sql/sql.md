# SQL语句

## 从一张表拷贝数据更新另一张表
```
UPDATE stock_new
SET qty=stock.qty
FROM (
	SELECT property, SUM(qty) AS qty
	FROM stocks
	GROUP BY name
)  AS stock
WHERE name=stock.name

UPDATE stock_new
SET qty=stock.qty
FROM
	(
		SELECT name,SUM(qty) FROM (VALUES (1,'a',10),(2,'b',100),(3,'b',10)) AS stock(id, name, qty) GROUP BY name
	) AS stock
WHERE name=stock.name
```

## 插入数据从另一张表查询
```
INSERT INTO table(id, code, name) VALUES(1, 'BBB', '大灯');

INSERT INTO table SELECT id, CONCAT('code_', name), name FROM table_breif;
```

## 查询并设置表的自增键
```
SELECT * FROM table_id_seq ;
SELECT SETVAL('table_id_seq', (SELECT MAX(id) FROM table));
```

## 联表查询
```
WITH TEMP AS (
SELECT id, name, created_at FROM table
UNION ALL 
SELECT id, name, created_at FROM table_breif
)
SELECT * FROM TEMP
ORDER BY created_at ASC;
```

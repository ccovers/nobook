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
# SQL语句

## 从一张表拷贝数据更新另一张表
```
select sums.qty,stock.qty
from stock_sums AS sums
LEFT JOIN
	(
		select property, SUM(qty) AS qty
		from stocks
		group by property
	) AS stock ON sums.property=stock.property

UPDATE stock_sums
SET qty=stock.qty
FROM (
		select property, SUM(qty) AS qty
		from stocks
		group by property
	)  AS stock
WHERE property=stock.property
```
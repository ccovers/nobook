# POSTGRESSQL

## UPSERT特性
当插入SQL违反约束的情况下，支持定义动作，而不抛出错误
INSERT ...
ON CONFLICT DO UPDATE/IGNORE/NOTHING ...
RETURN ...

## EXCLUDED
当插入SQL违反约束的情况下，需要更新时，代表更新的语句
INSERT INTO table AS t(id, name, num)
ON CONFLICT DO UPDATE
SET name=EXCLUDED.name, num=t.num+EXCLUDED.num


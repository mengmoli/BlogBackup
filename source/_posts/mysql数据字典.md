---
title: mysql查询数据字典
date: 2018-01-18
tags:
- mysql
---
### 用一条sql语句查询数据库生成数据字典
<!-- more -->
```sql
USE information_schema;

SELECT
	字段,
	字段说明,
	PK,
	数据类型,
	允许为空,
	默认值
FROM
	(
		SELECT
			CONCAT('数据表:', MAX(C.TABLE_NAME)) AS '字段',
			MAX(C.TABLE_NAME) AS '表名',
			MAX(T.TABLE_COMMENT) AS '字段说明',
			'' AS 'PK',
			'' AS '数据类型',
			'' AS '允许为空',
			'' AS '默认值'
		FROM
			information_schema. COLUMNS AS C
		INNER JOIN information_schema. TABLES AS T ON C.TABLE_SCHEMA = T.TABLE_SCHEMA
		AND C.TABLE_NAME = T.TABLE_NAME
		WHERE
			C.TABLE_SCHEMA = '替换成数据库名字'
		GROUP BY
			T.TABLE_NAME
		UNION ALL
			SELECT
				MAX(C.COLUMN_NAME) AS '字段',
				MAX(C.TABLE_NAME) AS '表名',
				MAX(C.COLUMN_COMMENT) AS '字段说明',
				MAX(C.EXTRA) AS 'PK',
				MAX(C.COLUMN_TYPE) AS '数据类型',
				MAX(C.IS_NULLABLE) AS '允许为空',
				MAX(C.COLUMN_DEFAULT) AS '默认值'
			FROM
				information_schema. COLUMNS AS C
			WHERE
				C.TABLE_SCHEMA = '替换成数据库名字'
			GROUP BY
				C.TABLE_NAME ASC,
				C.ORDINAL_POSITION ASC
	UNION ALL
			SELECT
			'' AS '字段',
			MAX(C.TABLE_NAME) AS '表名',
			'' AS '字段说明',
			'' AS 'PK',
			'' AS '数据类型',
			'' AS '允许为空',
			'' AS '默认值'
		FROM
			information_schema. COLUMNS AS C
		WHERE
			C.TABLE_SCHEMA = '替换成数据库名字'
		GROUP BY
			C.TABLE_NAME
	) S
ORDER BY
	表名 ASC
```
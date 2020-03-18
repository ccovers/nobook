CREATE TABLE `school_table` (
  `id` bigint(255) NOT NULL AUTO_INCREMENT COMMENT '自增ID',
  `student` varchar(256) NOT NULL COMMENT '学生',
  `subject` varchar(256) NOT NULL COMMENT '学科',
  `score` int(11) NOT NULL COMMENT '学分',
  `create_time` datetime NOT NULL DEFAULT CURRENT_TIMESTAMP COMMENT '创建时间',
  PRIMARY KEY (`id`),
  UNIQUE KEY `i_stu_subj` (`student`,`subject`)
) ENGINE=InnoDB AUTO_INCREMENT=1 DEFAULT CHARSET=utf8mb4 COMMENT='考试统计'

+----+---------+---------+-------+---------------------+
| id | student | subject | score | create_time         |
+----+---------+---------+-------+---------------------+
|  1 | 小明    | 语文    |    60 | 2018-12-21 22:47:37 |
|  2 | 小花    | 语文    |    70 | 2018-12-21 22:47:46 |
|  3 | 小强    | 语文    |    90 | 2018-12-21 22:47:57 |
|  4 | 小东    | 语文    |    90 | 2018-12-21 22:48:19 |
|  5 | 小明    | 数学    |    80 | 2018-12-21 22:49:29 |
|  6 | 小强    | 数学    |    80 | 2018-12-21 22:49:39 |
|  7 | 小东    | 数学    |    85 | 2018-12-21 22:49:50 |
|  8 | 小花    | 数学    |    90 | 2018-12-21 22:49:59 |
|  9 | 小明    | 英语    |    85 | 2018-12-21 22:50:12 |
| 10 | 小东    | 英语    |    86 | 2018-12-21 22:50:22 |
| 11 | 小强    | 英语    |    90 | 2018-12-21 22:50:31 |
| 12 | 小花    | 英语    |    95 | 2018-12-21 22:50:41 |
+----+---------+---------+-------+---------------------+

查询每学科最大分数的学生:
SELECT a.student,a.subject,a.score FROM school_table AS a JOIN (SELECT subject,max(score) AS max_score FROM school_table GROUP BY subject) AS b ON a.subject = b.subject AND a.score = b.max_score;
# 深入理解 MYSQL

## 问题
1. 为什么不建议使用订单号作为主键
2. 为什么要在需要排序的字段上加索引
3. `for update`的记录不存在会导致锁住全表
4. `redolog`和`binlog`有什么区别
5. `MYSQL`如何回滚一条`sql`
6. `char(50)`和`varchar(50)`效果是一样的吗


## 索引知识回顾
对于`MYSQL`数据库而言，数据是存储在文件里的，而为了能够快速定位到某张表里的某条记录进行查询和修改，我们需要将这些数据以一定的数据结构进行存储，这个数据结构就是我们说的索引。能够支持快速查找的数据结构有：顺序数组、哈希、搜索树。

数据要求插入的时候保证有序，如此，利用二分查找法达到`O(log(N))`的时间复杂度，对范围查询支持也很好，但是插入的时候如果不是在数组尾部，就需要挪动后面的所有数据，时间复杂度为`O(N)`，所以有序数组只适合存储静态数据，例如几乎很少变动的配置数据或者历史数据。
操作系统读取文件的流程，磁盘 IO 是一个相对很慢的操作，为了提高读取速度，我们应该尽量减少磁盘 IO 操作，而操作系统一般以　４kb 为一个数据页读取数据，而 MySQL 一般为 16kb 作为一个数据块，已经读取的数据块会在内存进行缓存，如果多次数据读取在同一个数据块，则只需要一次磁盘 IO ，而如果顺序一致的记录在文件中也是顺序存储的，就可以一次读取多个数据块

哈希表通过一个特定的哈希函数将 key 值转换为一个固定的地址，然后将对应的　value 放到这个位置，如果发生哈希碰撞就在这个位置拉出一个链表，由于哈希函数的离散特性，所以经过哈希函数处理后的 key 将失去原有的顺序，所以哈希结构的索引无法满足范围查询，只适合等值查询的情况例如一些缓存的场景。

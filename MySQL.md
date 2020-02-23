# MySQL

[toc]

## 连接方式是半双工的

## 执行顺序

### 连接器

###  查询缓存（5.8被取消，因为缓存失效太容易）

### 分析器

### 优化器

### 执行器

## 存储引擎

### MyISAM

> 表锁效率低，所以只读的时候使用

### InnoDB

> 支持事务、外键、表锁 、行锁、MVCC(可以并发读取)、采用聚簇索引来减少IO

### Memory

> 放到内存，重启丢失，读取快，临时存数据

### CVS

> 用逗号分隔开，解析起来比较搞笑，导入导出

### Archive

> 归档，不支持修改



## 一条更新语句是如何执行的？

![](https://tva1.sinaimg.cn/large/0082zybply1gbx72trp1hj30tl0m2wrm.jpg)



![](https://tva1.sinaimg.cn/large/0082zybply1gbx7iziec1j30xp0gtguw.jpg)





### Innodb 五个表空间(在磁盘上)

#### System Tablespace（系统）

#### File-Per-Table Tablespaces（独占） .ibd()

#### General Tablespaces

#### Temporary Tablespacces  (tmp)

#### undo Tablespaces （回滚，记录事务发生之前的数据）(和 redo 一起事务相关)

#### 

![](https://tva1.sinaimg.cn/large/0082zybply1gby6i0wz3lj30uf0iuqbb.jpg)

![](https://tva1.sinaimg.cn/large/0082zybply1gby6iq956nj30jt080tb7.jpg)

![](https://tva1.sinaimg.cn/large/0082zybply1gby7a5qlf2j30k908gjuc.jpg)

> 三层 就 2000多万行
>
> * B树能解决的 B+树都能解决
> * 扫库 扫表能力更强（从叶子节点顺序扫就可以）
> * 磁盘读写能力更强
> * 排序能力更强
> * 效率更加稳定

## 为什么索引不用红黑树

> 红黑树只能保证 最长路径不超过最短路径的两倍，不适合做索引
>
> 只适合在内存，不适合在磁盘

### 索引类型和索引方法



#### Temporary Tablespace



> InnoDB 里把磁盘数据加载到内存的最小单位是page,
>
> buffer pool(默认128MB),读取先到buffer pool中，没有再从磁盘读取
>
> 后台很多线程 把buffer pool写入到磁盘，这个动作叫 <b>刷脏</b>

### binlog

> 记录DDL DML逻辑的日志 主从 数据恢复

## WAL （write aheading logging）防止数据库宕机

> 先写日志再写磁盘 redo log



![](https://tva1.sinaimg.cn/large/0082zybply1gbx6gc9vmaj30fm07ojsd.jpg)



> redo log是顺序IO，到DB file是随机IO，顺序IO比较高
>
> 只在innodb中使用
>
> 大小  48MB



## [磁盘相关](http://www.pianshen.com/article/1618619122/)



![](https://tva1.sinaimg.cn/large/0082zybply1gbx6c482udj30er0bmn0u.jpg)

* 最小单位是扇区：通常512B 有的是4k
* 操作系统和内存打交道 最小单位是 页 page
* 操作系统和磁盘打交道最小单位是 块 block ，windows下叫簇

## SQL 语句

### 创建索引

```sql
CREATE INDEX index_name
ON table_name (column_name)

// 全文索引
fulltext index
select * from fulltext_test where match(content) against('咕泡学院' IN NATURAL LANGUAGE MODE);

```

## 分库分表相关问题

* 事务
* 排序、翻页、函数计算
* 全局主键
  * 数据库表 for update + 1
  * UUID 36个字符  16进制的数字，缺点 长度太长，无序
  * Redis 生成
  * 雪花算法 41 时间 10 机器id 12 序号
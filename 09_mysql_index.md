索引
==
- 是什么? 特殊文件,表空间的组成部分,包含数据表所有记录的引用指针.(通俗说法,一本书的"目录") ps:笔记整理
- 为什么? 数据量大,查询速度慢,对多个数据表经常查询的字段建立索引,优化查询.(修改表时,由于存在大量数据,创建索引过程需要时间.)
- 怎么做? 创建/查询/删除索引操作

```
普通索引
唯一索引
全文索引
主键索引
单列索引/多列索引
组合索引(最左前缀)
```

```
//普通索引 index
create index index_name on tinytable(name);  -- 创建
create table tinytable(
  id int not null auto_increment,
  name char(255),
  primary key (id),
  index [index_name](name)
  );
alter table tinytable add index [index_name] on (name);

show create table tinytable\G                -- 查询
show index from tinytable;

drop index index_name on tinytable;          -- 删除
alter table tinytable drop index index_name;
```

```
//唯一索引 unique
create unique index index_name on tinytable(name);
create table tinytable(
  id int auto_increment,
  name char(255),
  primary key (id),
  unique [index_name] (name)
  );
alter table tinytable add unique [index_name] on (name);
```

```
//全文索引 fulltext
//mysql 3.23.23版本开始支持全文索引和全文检索,仅用于myisam表.(切记对大容量数据表生成全文索引是非诚消耗时间消耗空间的做法.)
create fulltext index index_content on tinytable(content);
create table tinytable(
  id int auto_increment,
  name char(255),
  content text,
  primary key(id),
  unique [index_name](name),
  fulltext [index_content](content)
  );
alter table tinytable add fulltext index_content(content);  
```

```
//主键索引
alter table tinytable add primary key (id);
```

```
//单列索引/多列索引
//多个单列索引和单个多列索引 查询效果不同. 因为mysql只能使用一个索引进行查询, 会从多个索引中选择一个最为严格的索引.
```

```
//组合索引
//建立组合索引,相当于建立了如下索引: x,y 和 x (最左前缀,从最左开始组合,没有y索引.)
create index index_name on tinytable(x,y);
alter table tinytable add index index_x_y(x,y);
```

滥用索引
--
- 索引提高了查询时的速度,同时降低了更新表的速度.(如 insert/update/delete操作,因为不仅要保存数据,还好保存索引文件)
- 建立索引文件会占用磁盘空间.(空间换时间)

注意事项和优化
--
1. 聚集索引和非聚集索引
```
//待添加..
```
2. 索引不会包含有null值的列
```
只要列中包含有NULL值都将不会被包含在索引中，
复合索引中只要有一列含有NULL值，
那么这一列对于此复合索引就是无效的。
所以我们在数据库设计时不要让字段的默认值为NULL。
```
3. 使用短索引
```
对串列进行索引，如果可能应该指定一个前缀长度。
例如，如果有一个CHAR(255)的列，如果在前10个或20个字符内，
多数值是惟一的，那么就不要对整个列进行索引。
短索引不仅可以提高查询速度而且可以节省磁盘空间和I/O操作。
```
4. 索引列排序
```
MySQL查询只使用一个索引，
因此如果where子句中已经使用了索引的话，
那么order by中的列是不会使用索引的。
因此数据库默认排序可以符合要求的情况下不要使用排序操作；
尽量不要包含多个列的排序，如果需要最好给这些列创建复合索引。
```
5. like语句操作
```
like "%aaa%" 不会使用索引, 而like "aaa%"可以使用索引。
```
6. 不要在(索引)列上进行运算
```
select * from users where year(add_date)<2007;
每行进行运算,导致索引失效而进行全表搜索.
可以修改为
select * from users where add_date<'2007-01-01';
```

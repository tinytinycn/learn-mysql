约束
==
对一个字段的取值进行限制

主键约束 primary key
--
- 唯一标识一行数据;
- 唯一,不重复;
- 非空,不能取null值;
- 自动生成索引
```
create table tb(
  id int primary key,    -- 新建表时,添加主键
  ...
  );
create table tb(
  id int primary key,
  ...,
  primary key(id)
  );
create table tb(
  id int primary key,
  ip ...,
  ...,
  primary key(id,ip)     -- 双主键,添加(组合主键)
  );
alter table tb
  add primary key(id);   -- 修改表时,添加
desc tb;
show create table tb\G   -- 查看主键
alter table tb
  drop primary key(id);  -- 删除主键
create table tb(
  id int primary key auto_increment,  -- 新建表时,设置主键自增**
  ...,
  );  
create table tb(
  id int primary key,
  ...,
  );
alter table tb
  modify id int auto_increment;         -- 修改表时,设置主键自增(前提时已经设置主键)
```
- 自增主键 auto_increment

// 自增主键,不用管该值的连续性
// 自增主键不能回退
// 新增数据的主键查询 last_insert_id()函数,只获取当前数据库连接所插入的id值.
```
select last_insert_id();
insert into lianxi(st_id,tel)
  values(last_insert_id,'13900008888');
```

外键约束 foreign key .. references ..
--
- 限制数据的一致性
```
//创建外键
create table tb(
    mid int,
    ...,
    foreign key(mid)                         -- 创建外键,引用tb2表的id主键
    references tb2(id)
  );
create table tb(
  mid int,
  ...
  );
alter table table
  add foreign key(mid) references tb2(id);   -- 修改表,创建外键
show create table tb;
alter table tb
  drop foreign key 约束名;                    -- 外键会自动创建索引,删除外键不会自动删除外键的索引,必须另外手动删除
alter table tb
  drop index 索引名;
```
非空约束 not null
--
```
//添加非空约束
create table tb(
  id int primary key,
  name varchar(20) not null                      -- 新建表时,添加非空约束
  );
alter table tb modify name varchar(20) not null; -- 添加非空约束
alter table tb modify name varchar(20) null;     -- 删除非空约束,取消非空约束
```

唯一约束 unique
--
- 字段取值不能重复,可以允许重复的null值
- 会生成索引
```
create table tb(
  username varchar(32) unique not null,      -- 添加唯一约束
  Email varchar(128) unique,
  addr varchar(255),
  tel  varchar(11),
  unique(addr,tel)                           -- 添加组合唯一约束,表示字段组合不重复
  ...
  );
alter table table
  modify email varchar(128) unique;          -- 添加唯一约束
alter table tb
  add unique key(email,tel);
show create table tb\G;
alter table tb drop index 约束名;  
```

检索约束
--
- mysql不支持

默认值 default
--
```
create table tb(
  status tinyint default 0,
  ...
  );
```

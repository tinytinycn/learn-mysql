操作数据
==
- 插入数据 insert into ...
```
insert into users(id, username, age)
  values(1,'tiny',24);
insert into users                      -- 单行插入
  values(1, 'tiny', 24);
insert into users                      -- 多行插入
  values(...),(...),(...);
insert into users(...)                     -- 查询结果插入表中
  select ... from ...;
insert into users(name)                    -- 自身表中查询,并插入(实现批量插入数据)
  select name from users;
create table tb                            -- 批量插入数据到一张新表
  as select ... from ...;
注意:
''          //两个' 转义成一个' ,字符串中包含单引号' 时使用.
sql注入攻击  //....where password=' 1'or '1'='1' '; 阻止攻击方法,将用户填写的一个单引号' 替换成两个单引号''.
```
- 修改数据 update ... set ...
```
update users set username='tiny2',age=23
  where id=1;
update user set password=md5('123456')
  where id=9727;
update user  set age=age+1,class=3
  where name='tiny';
update user set class=4
  order by age desc limit 2;     -- 降序,前2条
```
- 删除数据 delete from ...
```
delete from users where id=1;               -- 删除表中数据.自增字段继续向后增加.
truncate table users;                       -- 删除表中所有数据,清空表.自增字段重新开始
```

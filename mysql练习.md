ddl/dml/dql/dcl

--------------------------------
用户
mysql -uroot -p
mysql -h192.168.1.1 -P3306 -uroot -p
create user 'root'@'192.168.1.%' identified by '1234';
grant all privileges on *.* to 'root'@'192.168.1.%';
drop user 'root'@'%';

--------------------------------
库
use test;
use mysql3;

create database tinydb;
create database tinydb charset=utf-8;
create database tinydb charset=gbk;

alter database tinydb charset=gbk;

drop database tinydb;

show databases;
show schemas;

----------------------------------
表
create table tinytable(
    id int,
    name varchar(20),
    money decimal(8,20
)engine=innodb charset=utf8;

alter table tinytable engine=myisam charset=gbk;
rename table tinytable to tinytable2;

alter table ... add ... first;
alter table ... add ... after;
alter table tinytable add id int first;
alter table tinytable add (
    gender char(1),
    tel char(11)
);
alter table tinytable change gender sex varchar(10);
alter table tinytable modify tel char(20);
alter table tinytable modify tel varchar(20) after age;
alter table tinytable drop sex;

show tables;
show create table tb\G;
desc tinytable;

---------------------------------------
索引
create index index_name on tb(name);
create index index_name on tb(name,addr);
create unique index index_name on tb(name);
drop index from tb;
show create table tb\G;

alter table tb drop index index_name;
alter table tb add index index_name(name,addr);
alter table tb add primary key(id);
alter table tb add unique index_name(name);

show create table tb\G;
show index from tb;

create table tb(
    id int auto_increment,
    name varchar(255),
    content text,
    primary key(id),
    unique index_name(name),
    fulltext index_content(content)
);

--------------------------------
 增删改
 insert into users(id, name, age) values(1, 'tiny', 25);
 insert into users(...) select ... from ...;
 create table tb as select ... from ...;

 update users set name='tiny2', age=24 where id=1;
 
 delete from users where id=1;//DML可回滚
 truncate table users;//DDL不可回滚
 
---------------------------------
 唯一/主键/外键/非空/检查 约束
 create table tb(name varchar(20) unique not null, ...);
 alter table tb modify email varchar(20) unique;
 alter table tb add unique key(email, tel);
 alter table tb drop index 约束名;

 create table tb(id int primary key, ...);
 create table tb(id int, ... , primary key(id));
 alter table tb add primary key(id);
 alter table tb drop primary key(id);

 create table tb(id int primary key auto_increment, ...);
 alter table tb modify id int auto_increment;//前提已设置主键

 create table tb(id int, ..., foreign key(id) references tb2(id));
 alter table tb add foreign key(id) references tb2(id);
 alter table tb drop foreign key 约束名;
 alter table tb drop index 索引名;//必须手动删除索引

 create table tb(id int primary key,name varchar(20) not null);
 alter table tb modify name varchar(20) not null;
 alter talbe tb modify name varchar(20) null;

 mysql不支持检查索引;

------------------------------------
 查
 select ... from ...;
 select distinct ... from ...;
 select ... as ... from ...;
 select ... from ... where ... group by ... having ... order by ... limit 0,5;//第1行前5条记录

------------------------------------
 多行函数
 count()/max()/min()/avg()/sum() //不能和普通字段一起查询
 字符串函数
 length('xxx')/concat('xxx','vvv')/trim(' xx ')/substring('abc',2)

------------------------------------
 子查询
 where子查询/from子查询(行内视图)/字段子查询/exits子查询

------------------------------------
 视图
 create view v_name as select ... from ...;
 show tables;
 desc v_name;
 show create view v_name;
 drop view v_name;

-------------------------------------
 事务
 start transaction;
 set autocommit=0;
 commit;
 rollback;

-------------------------------------
 set @var='abc';
 set @@tx_isolation='';
 select @var;
 存储过程
 delimiter //
 create procedure p1()
    begin
     select * from users;
    end;
    //
 delimiter ;

 call p1();
 drop procedure p1;
 show create procedure p1\G
 show procedure status\G
 show procedure status where db='tinydb'\G

-------------------------------------
 函数
 create function f(arg) returns int
  begin
   ...
   return 0;
  end;

-------------------------------------
 触发器
 create trigger t1 before/after insert
  on tb for each row
  begin
    ...
  end;

  use information_schema;
  select * from triggers\G


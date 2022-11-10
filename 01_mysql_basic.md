目录
--
* mysql/mariadb 安装
* sql 结构化查询语言
* mysql 使用
* 数据类型
* 数据搜索引擎
* 索引
* 表之间的关系
* 事务
* 测试数据
* 数据库备份和恢复

----
mysql/mariadb 安装
==
- 安装mysql 社区版本
- 安装mariadb
```
yum -y install mariadb-server mariadb mariadb-devel
systemctl start mariadb
```

sql 结构化查询语言
==
- DDL 数据定义语言 create/drop/truncate
- DML 数据操作语言 insert/update/delete/
- DQL 数据查询语言 select/
- DCL 数据控制语言 grant授权.分配权限

mysql 使用
==
- 命令行客户端:(登录)
```
mysql -h192.168.xxx.xxx -P3306 -uroot -p  //指定远程服务器,端口
mysql -uroot -p                           //指定user用户root,密码
password: ****
```
- 其他客户端:(登录)

- 创建用户
```
create user 'root'@'192.168.12.%' identified by '1234'; //指定用户地址和密码
grant all privileges on *.* to 'root'@'192.168.12.%'; //授予所有权限给用户
```
- 删除用户
```
drop user 'username'@'%'  //删除用户
```

- 进入数据库
```
use test;
use mysql3;
```
- 创建数据库
```
create database tinydb  
create database tinydb charset=utf8;
create database tinydb charset=gbk;
```
- 查看数据库
```
show databases;
show schemas;
```
- 修改数据库
```
alter database tinydb charset=gbk;  //一般只能修改数据库的字符编码
```
- 删除数据库
```
drop database tinydb;  //所有数据表全部删除,不可恢复
```

- 创建表
```
create table tinytable(
  id int,
  name varchar(20),
  money decimal(8,2)
  )engine=innodb charset=utf8;
```
- 查看表
```
show tables;
desc tinytable;
```
- 修改表
```
rename table tinytable to tinytable2;            //修改表名
alter table tinytable engine=myisam chaset=gbk;  //修改属性

alter table ... add ... first        //加到第一个字段
alter table ... add ... after        //加到某个字段之后
alter table tinytable add id int first;  
alter table tinytable add(                             //添加字段
  gender char(1),
  tel char(11)
  );
alter table tinytable change gender sex varchar(10);   //修改字段名
alter table tinytable modify tel char(20);             //修改字段类型
alert table tinytable modify tel varchar(20) after age;//修改字段顺序
alter table tinytable drop sex;                        //删除字段
desc tinytable;
```
- 查看表中的数据
```
select * from user;
```
- 删除表
```
drop table tinytable;   //不可恢复
```

数据类型
==
- 字符串

```
char          //定长字符串,最多255字节.定长查询效率更高.
char(20)    //定长20.若超出长度可能会报错,也可能会截断.若长度不足,补空格.
varchar       //变长字符串,字节量总和超过65535字节报错,小于255字节加一位字节记录字符串长度,大于255字节加两位字节记录字符串长度; 另外字段允许null值,需额外1个字节记录其值.gbk编码,最长65535/2长度,uft8编码最长65535/3长度.
varchar(12) //变长12.
text          //65535字节,只占用表总字节量的10个字节.
blob          //超大对象数据,使用流开读写blob字段数据.通常不用它保存文本.
enum          //枚举
  gender enum('F','M')
set           //
  funny set('lol','dota2','sc2')

```
- 数字
```
添加 unsigned标记 表示无符号
tinyint unsigned  //0~255
tinyint           //-127~128
zerofill      //位数不足,补零
tinyint       //1字节
smallint      //2字节
int           //4字节
int(3)      //括号内数字,影响显示格式,不影响数字范围,只显示3位
int(4)
bigint        //8字节
float         //4字节
double        //8字节
double(6,2) //括号内数组,影响显示格式,不影响数字范围
decimal/numeric
decimal(8,2)  //括号内数字,影响数字范围,保存精确浮点数,整数位6位,小数位2位,整数位超出范围报错,小数位超出范围四舍五入.
```
- 日期
```
date          //年月日
time          //时分秒
datetime      //年月日时分秒
timestamp     //时间戳,同 datetime.最大到2038 年.修改一行数据时,第一个timestamp字段会自动修改为系统当前时间.(一般不使用)
                也可以使用 bigint 整数表示时间
```


数据搜索引擎
==
- innodb 默认,支持事务/外键,提供行级锁
- myisam 不支持事务/外键,数据访问效率更高,只提供表级锁
- memory 内存表
```
create table tb_item(
  id bigint(20),
  cid bigint(10),
  brand varchar(50),
  model varchar(50),
  title varchar(100),
  sell_point varchar(500),
  price bigint,
  num int(10),
  barcode varchar(30),
  image varchar(500),
  status tinyint(4),
  created datetime,
  updated datetime
  )engine=innodb charset=utf8;

```
索引
==
- 数据存储位置目录:B-Tree,哈希索引
- 提高查询效率,首先考虑创建索引
```
create index index_name on tb(name);     -- 创建索引
create index index_name on tb(name,addr);
show create table tb\G;                  -- 查看索引
alter table tb drop index index_name;    -- 删除索引
```


表之间的关系
==
一对一
- 具有唯一约束unique的外键约束
- 既是主键,也是外键的字段

一对多
- 在多方添加外键

多对多
- 需要一个中间表,添加两个外键字段,分别引用两张表的主键

事务
==
innodb 支持事务
- 原子性
- 一致性
- 隔离性
- 持久性

测试数据
==
```
mysql>  source d:\hr_mysql.sql
```
数据库备份和恢复
==
- 备份(window命令行执行)
```
mysqldump -uroot -p --default-character-set=utf8
  数据库名>d:\a\backup.sql
```
- 恢复(提前新建一个新空库在服务器)
```
drop database if exists jtds;
create database jtds charset=utf8;    -- 建库

mysql -uroot -p --default-character-set=utf8 数据库名<d:\a\backup.sql
```

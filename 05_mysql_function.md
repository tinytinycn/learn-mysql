mysql 函数
==
字符串
```
char_length('a中')          //字符数
length('a中')               //字节数
concat('a','b','cde','fff') //字符串连接，其他数据库可用 || 连接字符串，'abc' || 'def'
concat_ws(';','abc','def','ggg')        //用分隔符连接字符串    
instr('abcdefgdef','def')               //返回第一个子串的位置，从1开始，找不到返回0
locate('abc', '---abc---abc---abc-')    //返回第一个子串的位置，从1开始，找不到返回0
locate('abc', '---abc---abc---abc-',5)  //从指定位置向后找
insert('abcdefghijkl',2, 11, '---')     //用子串取代从2位置开始的11个字符
lower('AdFfLJf')            //变为小写
upper('AdFfLJf')            //变为大写
left('AdFfLJf',3)           //返回最左边的三个字符
right('AdFfLJf',3)          //返回最右边的三个字符
lpad('abc', 8, '*')         //左侧填充，指定长度比源字符串少，相当于left
rpad('abc', 8, '*')         //右侧填充，指定长度比源字符串少，相当于left
trim('  a  bc   ')          //去除两端空格
substring('abcdefghijklmn', 3)          //从3位置开始的所有字符
substring('abcdefghijklmn', 3, 6)       //从3位置开始的6个字符
repeat('abc', 3)                        //重复三遍abc
REPLACE('Hello MySql','My','Your')      //子串替换
REVERSE('Hello')                        //翻转字符串
SPACE(10)                               //返回10个空格
```

数字
```
ceil(-3.14)          //向上取整
floor(3.94)          //向下取整,舍掉小数
format(391.536, 2)   //数字格式化为字符串，###,###.###，四舍五入，第二个参数为小数位数
round(673.4974)      //四舍五入
round(673.4974, 2)   //四舍五入到小数点后两位
round(673.4974, -2)  //四舍五入到百
TRUNCATE(234.31, 1)  //舍去至小数点后1位
abs()                //绝对值
```

日期
```
NOW()         //返回当前的日期和时间
CURDATE()     //返回当前的日期
CURTIME()     //返回当前的时间
DATE(时间)    //提取日期或日期/时间表达式的日期部分
TIME(时间)    //提取日期或日期/时间表达式的时间部分
EXTRACT(字段 From 日期)   //返回日期/时间按的单独部分
  字段的合法值：
        MICROSECOND
        SECOND
        MINUTE
        HOUR
        DAY
        WEEK
        MONTHs
        QUARTER
        YEAR
        SECOND_MICROSECOND
        MINUTE_MICROSECOND
        MINUTE_SECOND
        HOUR_MICROSECOND
        HOUR_SECOND
        HOUR_MINUTE
        DAY_MICROSECOND
        DAY_SECOND
        DAY_MINUTE
        DAY_HOUR
        YEAR_MONTH
DATE_ADD(日期, INTERVAL 数量 字段)   //给日期添加指定的时间间隔
  字段的合法值同上

DATE_SUB(日期, INTERVAL 数量 字段)   //从日期减去指定的时间间隔
DATEDIFF(日期1, 日期2)              //返回两个日期之间的天数
DATE_FORMAT(日期, 格式)             //用不同的格式显示日期/时间
  格式字符：  %Y-%m-%d %H:%i:%s
              %d/%m/%Y
              %Y年%m月%d日
          %a  缩写星期名
          %b  缩写月名
          %c  月，数值
          %D  带有英文前缀的月中的天
          %d  月的天，数值(00-31)
          %e  月的天，数值(0-31)
          %f  微秒
          %H  小时 (00-23)
          %h  小时 (01-12)
          %I  小时 (01-12)
          %i  分钟，数值(00-59)
          %j  年的天 (001-366)
          %k  小时 (0-23)
          %l  小时 (1-12)
          %M  月名
          %m  月，数值(00-12)
          %p  AM 或 PM
          %r  时间，12-小时（hh:mm:ss AM 或 PM）
          %S  秒(00-59)
          %s  秒(00-59)
          %T  时间, 24-小时 (hh:mm:ss)
          %U  周 (00-53) 星期日是一周的第一天
          %u  周 (00-53) 星期一是一周的第一天
          %V  周 (01-53) 星期日是一周的第一天，与 %X 使用
          %v  周 (01-53) 星期一是一周的第一天，与 %x 使用
          %W  星期名
          %w  周的天 （0=星期日, 6=星期六）
          %X  年，其中的星期日是周的第一天，4 位，与 %V 使用
          %x  年，其中的星期一是周的第一天，4 位，与 %v 使用
          %Y  年，4 位
          %y  年，2 位
LAST_DAY(日期) - 返回当月最后一天
```

NULL 相关
```
IFNULL(数据1,数据2)          //数据1是null返回数据2；不是null返回数据1
                            //返回第一个不是null值的数据x
coalesce(数据1,数据2,......) //从左向右第一个不是null的数据,返回第一不是null的数据x
```

加密
```
md5()                       //常用加密方式,返回32位长度的16进制数字字符串
sha()                       //比md5()更安全
```

多行函数
- 多行数据交给函数处理，产生一个计算结果
- 不能直接与普通字段一起查询*
```
count()   //计数，数量
max()     //最大值
min()     //自小值
avg()     //平均
sum()     //求合
```

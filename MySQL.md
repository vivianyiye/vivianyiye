# 1 数据库总览 :

1.关系型数据库 ( SQL )
MySQL , Oracle , SQL Server , SQLite , DB2 , ...
关系型数据库通过外键关联来建立表与表之间的关系

2.非关系型数据库 ( NOSQL )
Redis , MongoDB , ...
非关系型数据库通常指数据以对象的形式存储在数据库中，而对象之间的关系通过每个对象自身的属性来决定

![image](https://user-images.githubusercontent.com/75358006/135716940-37312a55-13a5-42d0-a1fd-0597517c1809.png)

![image](https://user-images.githubusercontent.com/75358006/135768552-e89fcebf-1809-4fde-96f8-6a5c413e9a96.png)


几个基本的数据库操作命令 :
```
连接数据库语句 : mysql -h 服务器主机地址 -u 用户名 -p 用户密码   连接数据库

update user set password=password('123456')where user='root';  修改密码

flush privileges; -- 刷新数据库
-------------------
-- 所有的语句都用分号结尾

show databases;  显示所有数据库

use dbname；打开某个数据库

show tables; 显示数据库mysql中所有的表
describe user; 显示表mysql数据库中user表的列信息，表结构

create database name; 创建数据库

exit; 退出Mysql

-- 表示注释
/*
*/多行注释

```

## 数据库xx语言
![image](https://user-images.githubusercontent.com/75358006/136699058-1b338a0f-e2b0-4850-b5e7-95359f18f11b.png)


# 2 操作数据库
数据库 > 表 > 表中数据

<font color="red"> MySQL关键字不区分大小写 </font>

### 2.1 操作数据库（能看懂、了解就好）

1.创建: create database [if not exists] 数据库名;

2.删除: drop database [if exists] 数据库名;

3.使用: use 数据库名;
```
如果表名或者字段名有特殊字符，就需要`school`
```

4.查看: show databases;

### 2.2 数据值和列类型

```
数值类型
```
![image](https://user-images.githubusercontent.com/75358006/136737755-4500d236-2abe-4479-a537-3259cb8a3b61.png)

```
字符串
```
![image](https://user-images.githubusercontent.com/75358006/136737904-1bcb504e-e914-4c7e-bbaf-e28dd952fa5f.png)

```
时间日期
```
![image](https://user-images.githubusercontent.com/75358006/136738071-64f1a92a-d139-4d23-adee-184c1d225c28.png)


理解为 "没有值" 或 "未知值"

不要用NULL进行算术运算 , 结果仍为NULL

### 2.3 数据字段属性

UnSigned

- 无符号的

- 声明该数据列不允许负数 

**ZEROFILL**

- 0填充的
- 不足位数的用0来填充 , 如int(3),5则为005

**Auto_InCrement**

- 自动增长的 , 每添加一条数据 , 自动在上一个记录数上加 1(默认)

- 通常用于设置**主键** , 且为整数类型

- 可定义起始值和步长

- - 当前表设置步长(AUTO_INCREMENT=100) : 只影响当前表
  - SET @@auto_increment_increment=5 ; 影响所有使用自增的表(全局)

**NULL 和 NOT NULL**

- 默认为NULL , 即没有插入该列的数值
- 如果设置为NOT NULL , 则该列必须有值

**DEFAULT**

- 默认的
- 用于设置默认值
- 例如,性别字段,默认为"男" , 否则为 "女" ; 若无指定该列的值 , 则默认值为"男"的值

拓展

每一个表，都必须存在以下五个字段，表示一个记录存在意义

1. id 主键
2. version 乐观锁
3. is_delete 伪删除
4. gmt_create 创建时间
5. gmt_update 修改时间

### 2.4 创建数据库表（重点）

```
-- 目标 : 创建一个school数据库
-- 创建学生表(列,字段)
-- 学号int 登录密码varchar(20) 姓名,性别varchar(2),出生日期(datatime),家庭住址,email

-- 创建表之前 , 一定要先选择数据库
-- 字符串使用单引号括起来
-- 所有语句后加英文逗号，最后一个不用加

CREATE TABLE IF NOT EXISTS `student` (
`id` int(4) NOT NULL AUTO_INCREMENT COMMENT '学号',
`name` varchar(30) NOT NULL DEFAULT '匿名' COMMENT '姓名',
`pwd` varchar(20) NOT NULL DEFAULT '123456' COMMENT '密码',
`sex` varchar(2) NOT NULL DEFAULT '男' COMMENT '性别',
`birthday` datetime DEFAULT NULL COMMENT '生日',
`address` varchar(100) DEFAULT NULL COMMENT '地址',
`email` varchar(50) DEFAULT NULL COMMENT '邮箱',
PRIMARY KEY (`id`)
) ENGINE=InnoDB DEFAULT CHARSET=utf8
```

格式

```
CREATE TABLE [IF NOT EXISTS] '表名'(
   '字段名' 列类型 [属性] [索引] [注释],
   ...
   '字段名' 列类型 [属性] [索引] [注释]
)[表类型][字符集类型][注释]
```

常用命令

```sql
-- 查看数据库的定义
SHOW CREATE DATABASE school;
-- 查看数据表的定义
SHOW CREATE TABLE student;
-- 显示表结构
DESC student;  -- 设置严格检查模式(不能容错了)SET
```

### 2.5 数据表的类型

```sql
-- 关于数据库引擎
/*
INNODB 默认使用
MYISAM 早些年使用
*/
```

|                      | MYISAM       | INNODB        |
| -------------------- | ------------ | ------------- |
| 事务支持（）         | 不支持       | 支持          |
| 数据行锁定           | 不支持，表锁 | 支持，行锁    |
| 外键约束             | 不支持       | 支持          |
| 全文索引             | 支持         | 不支持        |
| 表空间的大小（内存） | 较小         | 较大，约为2倍 |

经验 ( 适用场合 )  :

- 适用 MyISAM : 节约空间、速度
- 适用 InnoDB : 安全性高 , 支持事务处理及多表多用户操作

> 数据表的存储位置

- MySQL数据表以文件方式存放在data目录下，本质是文件的存储

MySQL引擎在物理文件上的区别

- InnoDB类型数据表只有一个 *.frm文件 , 以及上一级目录的ibdata1文件
- MyISAM类型数据表对应三个文件 :
  - \* . frm -- 表结构定义文件
  - \* . MYD -- 数据文件 ( data )
  - \* . MYI -- 索引文件 ( index )



> 设置数据库表的字符集编码

```sql
CHARSET=utf8
```

不设置的话，会是mysql默认的字符编码Latin1，不支持中文

也可以在my.ini中配置默认的编码

```sql
character-set-server=utf8
```

### 2.6 修改删除表

> 修改（ALTER）

```sql
修改表名 : ALTER TABLE 旧表名 RENAME AS 新表名

添加字段 : ALTER TABLE 表名 ADD 字段名 列属性[属性]

修改字段（重命名，修改约束） :

- ALTER TABLE 表名 MODIFY 字段名 列类型[属性]  -- 修改约束
- ALTER TABLE 表名 CHANGE 旧字段名 新字段名 列类型[属性]  -- 字段重命名

删除字段 :  ALTER TABLE 表名 DROP 字段名
```



> 删除（DELETE）

DROP TABLE [IF EXISTS] 表名

- IF EXISTS为可选 , 判断是否存在该数据表
- 如删除不存在的数据表会抛出错误



**所有的创建和删除操作尽量加上判断，以免报错**



注意点：

```
1. 可用反引号（`）为标识符（库名、表名、字段名、索引、别名）包裹，以避免与关键字重名！中文也可以作为标识符！

2. 每个库目录存在一个保存当前数据库的选项文件db.opt。

3. 注释：
  多行注释 /* 注释内容 */
  单行注释 -- 注释内容       (标准SQL注释风格，要求双破折号后加一空格符（空格、TAB、换行等）)
   
4. 模式通配符：
  _   任意单个字符
  %   任意多个字符，甚至包括零字符
  单引号需要进行转义 \'
   
5. CMD命令行内的语句结束符可以为 ";", "\G", "\g"，仅影响显示结果。其他地方还是用分号结束。delimiter 可修改当前对话的语句结束符。

6. SQL对大小写不敏感 （关键字），建议小写

7. 清除已有语句：\c
```



## 3 MySQL数据管理

### 3.1 外键（了解即可）

> 创建外键

建表时指定外键约束

```sql
-- 创建外键的方式一 : 创建子表同时创建外键
-- 学生表的gradeid字段，要去引用年级表的gradeid
-- 1.定义外键key
-- 2.给这个外键添加约束（执行引用）references

-- 年级表 (id\年级名称)
CREATE TABLE `grade` (
`gradeid` INT(10) NOT NULL AUTO_INCREMENT COMMENT '年级ID',
`gradename` VARCHAR(50) NOT NULL COMMENT '年级名称',
PRIMARY KEY (`gradeid`)
) ...

-- 学生信息表 (学号,姓名,性别,年级,手机,地址,出生日期,邮箱,身份证号)
CREATE TABLE `student` (
`studentno` INT(4) NOT NULL COMMENT '学号',
...
`gradeid` INT(10) DEFAULT NULL COMMENT '年级',
...
PRIMARY KEY (`studentno`),
KEY `FK_gradeid` (`gradeid`),
CONSTRAINT `FK_gradeid` FOREIGN KEY (`gradeid`) REFERENCES `grade` (`gradeid`)
) ...
```

建表后修改

```sql
-- 创建外键方式二 : 创建子表完毕后,修改子表添加外键创建表的时候没有外键关系
ALTER TABLE `student`
ADD CONSTRAINT `FK_gradeid` FOREIGN KEY (`gradeid`) REFERENCES `grade` (`gradeid`);
```

**结论**

- 以上操作都是物理外键，数据库级别的外键，不建议使用（避免数据库过多造成困扰，了解即可）
- 如果想使用多张表的数据和外键，交给程序去实现

### 3.2 DML语言（全部记住）

**数据库意义** ： 数据存储、数据管理

- - INSERT (添加数据语句)
  - UPDATE (更新数据语句)
  - DELETE (删除数据语句)



### 3.3 添加（insert）

```sql
-- 基本语法 : INSERT INTO 表名[(字段1,字段2,字段3,...)] VALUES('值1','值2','值3')
INSERT INTO grade(gradename) VALUES ('大一');

-- 主键自增,可以省略主键
INSERT INTO grade VALUES ('大二');

-- 主键不自增，不可以省略
查询：INSERT INTO grade VALUE ('大二')
错误：Column count doesn`t match value count at row 1

-- 结论：'字段1,字段2...'该部分可省略 , 但添加的值务必与表结构,数据列,顺序相对应,且数量一致.

-- 一次插入多条数据
INSERT INTO grade(gradename) VALUES ('大三'),('大四');
```



### 3.4 修改（update）

```sql
-- 基本语法：UPDATE 表名 SET column_name=value [,column_name2=value2,...] [WHERE condition];
UPDATE grade SET gradename = '高中' WHERE gradeid = 1;

-- 不指定的条件下，会改动所有的表！
UPDATE grade SET gradename = '高中';

-- 修改多个属性，用逗号隔开
UPDATE grade SET gradename = '高中', email = '12333@qq.com' WHERE gradeid = 1;

-- 函数值
UPDATE grade SET birthday = CURRENT_TIME WHERE name = 'yiye';
```

WHERE子句判断符

![图片](https://mmbiz.qpic.cn/mmbiz_png/uJDAUKrGC7LCI6xGKJ7bKiaBudOSBHd9dyJWPxp3H9GicphPXMEvCwtUyKX3vibUCESqSaDnKnLzlwYpcRTJsdUIg/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

### 3.5 删除（delete）

```sql
-- 基本语法：DELETE FROM 表名 [WHERE condition];

-- 删除最后一个数据
DELETE FROM grade WHERE gradeid = 5

-- 基本语法：TRUNCATE [TABLE] table_name;

-- 清空年级表
TRUNCATE grade
```

**注意：delete和truncate区别**

- 相同 : 都能删除数据 , 不删除表结构 , 但TRUNCATE速度更快

- 不同 :

- - 使用TRUNCATE TABLE 重新设置AUTO_INCREMENT计数器，自增列归零
  - 使用TRUNCATE TABLE不会对事务有影响 （事务后面会说）

```sql
-- 结论:
-- truncate删除数据,自增当前值会恢复到初始值重新开始;不会记录日志.

-- 同样使用DELETE清空不同引擎的数据库表数据.重启数据库服务后
-- InnoDB : 自增列从初始值重新开始 (存储在内存中,断电即失)
-- MyISAM : 自增列依然从上一个自增数据基础上开始 (存在文件中,不会丢失)
```

# 4 DQL查询数据（最重点）

### 4.1 DQL

( Data Query Language 数据查询语言 )

- 查询数据库数据 , 如**SELECT**语句
- 简单的单表查询或多表的复杂查询和嵌套查询
- 数据库语言中最核心,最重要的语句
- 使用频率最高的语句

```sql
CREATE DATABASE IF NOT EXISTS `school`;
-- 创建一个school数据库
USE `school`;-- 创建学生表
DROP TABLE IF EXISTS `student`;
CREATE TABLE `student`(
	`studentno` INT(4) NOT NULL COMMENT '学号',
    `loginpwd` VARCHAR(20) DEFAULT NULL,
    `studentname` VARCHAR(20) DEFAULT NULL COMMENT '学生姓名',
    `sex` TINYINT(1) DEFAULT NULL COMMENT '性别，0或1',
    `gradeid` INT(11) DEFAULT NULL COMMENT '年级编号',
    `phone` VARCHAR(50) NOT NULL COMMENT '联系电话，允许为空',
    `address` VARCHAR(255) NOT NULL COMMENT '地址，允许为空',
    `borndate` DATETIME DEFAULT NULL COMMENT '出生时间',
    `email` VARCHAR (50) NOT NULL COMMENT '邮箱账号允许为空',
    `identitycard` VARCHAR(18) DEFAULT NULL COMMENT '身份证号',
    PRIMARY KEY (`studentno`),
    UNIQUE KEY `identitycard`(`identitycard`),
    KEY `email` (`email`)
)ENGINE=MYISAM DEFAULT CHARSET=utf8;

-- 创建年级表
DROP TABLE IF EXISTS `grade`;
CREATE TABLE `grade`(
	`gradeid` INT(11) NOT NULL AUTO_INCREMENT COMMENT '年级编号',
  `gradename` VARCHAR(50) NOT NULL COMMENT '年级名称',
    PRIMARY KEY (`gradeid`)
) ENGINE=INNODB AUTO_INCREMENT = 6 DEFAULT CHARSET = utf8;

-- 创建科目表
DROP TABLE IF EXISTS `subject`;
CREATE TABLE `subject`(
	`subjectno`INT(11) NOT NULL AUTO_INCREMENT COMMENT '课程编号',
    `subjectname` VARCHAR(50) DEFAULT NULL COMMENT '课程名称',
    `classhour` INT(4) DEFAULT NULL COMMENT '学时',
    `gradeid` INT(4) DEFAULT NULL COMMENT '年级编号',
    PRIMARY KEY (`subjectno`)
)ENGINE = INNODB AUTO_INCREMENT = 19 DEFAULT CHARSET = utf8;

-- 创建成绩表
DROP TABLE IF EXISTS `result`;
CREATE TABLE `result`(
	`studentno` INT(4) NOT NULL COMMENT '学号',
    `subjectno` INT(4) NOT NULL COMMENT '课程编号',
    `examdate` DATETIME NOT NULL COMMENT '考试日期',
    `studentresult` INT (4) NOT NULL COMMENT '`grade``grade`考试成绩',
    KEY `subjectno` (`subjectno`)
)ENGINE = INNODB DEFAULT CHARSET = utf8;

-- 插入学生数据 其余自行添加 这里只添加了2行
INSERT INTO `student` (`studentno`,`loginpwd`,`studentname`,`sex`,`gradeid`,`phone`,`address`,`borndate`,`email`,`identitycard`)
VALUES
(1000,'123456','张伟',0,2,'13800001234','北京朝阳','1980-1-1','text123@qq.com','123456198001011234'),
(1001,'123456','赵强',1,3,'13800002222','广东深圳','1990-1-1','text111@qq.com','123456199001011233');

-- 插入成绩数据  这里仅插入了一组，其余自行添加
INSERT INTO `result`(`studentno`,`subjectno`,`examdate`,`studentresult`)
VALUES
(1000,1,'2013-11-11 16:00:00',85),
(1000,2,'2013-11-12 16:00:00',70),
(1000,3,'2013-11-11 09:00:00',68),
(1000,4,'2013-11-13 16:00:00',98),
(1000,5,'2013-11-14 16:00:00',58);

-- 插入年级数据
INSERT INTO `grade` (`gradeid`,`gradename`) VALUES(1,'大一'),(2,'大二'),(3,'大三'),(4,'大四'),(5,'预科班');

-- 插入科目数据
INSERT INTO `subject`(`subjectno`,`subjectname`,`classhour`,`gradeid`)VALUES
(1,'高等数学-1',110,1),
(2,'高等数学-2',110,2),
(3,'高等数学-3',100,3),
(4,'高等数学-4',130,4),
(5,'C语言-1',110,1),
(6,'C语言-2',110,2),
(7,'C语言-3',100,3),
(8,'C语言-4',130,4),
(9,'Java程序设计-1',110,1),
(10,'Java程序设计-2',110,2),
(11,'Java程序设计-3',100,3),
(12,'Java程序设计-4',130,4),
(13,'数据库结构-1',110,1),
(14,'数据库结构-2',110,2),
(15,'数据库结构-3',100,3),
(16,'数据库结构-4',130,4),
(17,'C#基础',130,1);
```



### 4.2 指定查询字段

> SELECT语法

```sql
SELECT [ALL | DISTINCT]
{* | table.* | [table.field1[as alias1][,table.field2[as alias2]][,...]]}
FROM table_name [as table_alias]
  [left | right | inner join table_name2]  -- 联合查询
  [WHERE ...]  -- 指定结果需满足的条件
  [GROUP BY ...]  -- 指定结果按照哪几个字段来分组
  [HAVING]  -- 过滤分组的记录必须满足的次要条件
  [ORDER BY ...]  -- 指定查询记录按一个或多个条件排序
  [LIMIT {[offset,]row_count | row_countOFFSET offset}];
   -- 指定查询的记录从哪条至哪条
```

**注意 : [ ] 括号代表可选的 , { }括号代表必选得**



### 指定查询字段

```
-- 查询表中所有的数据列结果 , 采用 **" \* "** 符号; 但是效率低，不推荐 .

-- 查询所有学生信息
SELECT * FROM student;

-- 查询指定列(学号 , 姓名)
SELECT studentno,studentname FROM student;
```



> AS 子句作为别名

作用：

- 可给数据列、表或计算或总结的结果取一个新别名

```
-- 这里是为列取别名(当然as关键词可以省略)
SELECT studentno AS 学号,studentname AS 姓名 FROM student;

-- 使用as也可以为表取别名
SELECT studentno AS 学号,studentname AS 姓名 FROM student AS s;

-- 使用as,为查询结果取一个新名字
-- CONCAT()函数拼接字符串
SELECT CONCAT('姓名:',studentname) AS 新姓名 FROM student;
```



> DISTINCT关键字的使用

作用 : 去掉SELECT查询返回的结果中重复的数据 ( 返回所有列的值都相同 ) , 只返回一条

```sql
-- # 查看哪些同学参加了考试(学号) 去除重复项
SELECT * FROM result; -- 查看考试成绩
SELECT studentno FROM result; -- 查看哪些同学参加了考试
SELECT DISTINCT studentno FROM result; -- 了解:DISTINCT 去除重复项 , (默认是ALL)
```



> 使用表达式的列

数据库中的表达式 : 一般由文本值 , 列值 , NULL , 函数和操作符等组成

应用场景 :

- SELECT语句返回结果列中使用

- SELECT语句中的ORDER BY , HAVING等子句中使用

- DML语句中的 where 条件语句中使用表达式

  ```sql
  -- selcet查询中可以使用表达式
  SELECT @@auto_increment_increment; -- 查询自增步长
  SELECT VERSION(); -- 查询版本号
  SELECT 100*3-1 AS 计算结果; -- 表达式
  
  -- 学员考试成绩集体提分一分查看
  SELECT studentno,StudentResult+1 AS '提分后' FROM result;
  ```

- 避免SQL返回结果中包含 ' . ' , ' * ' 和括号等干扰开发语言程序.

数据库中的表达式：文本值，列，null，函数，计算表达式，系统变量..



### 4.3 where条件子句

作用：用于检索数据表中 符合条件 的记录

搜索条件可由一个或多个逻辑表达式组成 , 结果是布尔值，一般为真或假.

> 逻辑操作符

![图片](https://mmbiz.qpic.cn/mmbiz_png/uJDAUKrGC7LwfjFbCQXic0pcE21lUFGvDT2GTsOdcj7nOuoXTIgEfrNMN8YGygWdrFUTLe41xNqchhfGdq6CHtw/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)



```sql
-- 满足条件的查询(where)
SELECT Studentno,StudentResult FROM result;

-- 查询考试成绩在95-100之间的，AND也可以写成 &&
SELECT Studentno,StudentResult FROM result
WHERE StudentResult>=95 AND StudentResult<=100;

-- 模糊查询(对应的词:精确查询)
SELECT Studentno,StudentResult FROM result
WHERE StudentResult BETWEEN 95 AND 100;

-- 除了1000号同学,要其他同学的成绩
SELECT studentno,studentresult FROM result
WHERE studentno!=1000;

-- 使用NOT
SELECT studentno,studentresult FROM result
WHERE NOT studentno=1000;
```



> 模糊查询 ：比较操作符

![图片](https://mmbiz.qpic.cn/mmbiz_png/uJDAUKrGC7LwfjFbCQXic0pcE21lUFGvDk8xl58oP6ch67ZibicU1fn2O7Lk4uLZyiaG8p8Zhkl4oF1GUibbPF0iaxIQ/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)



```sql
-- 模糊查询 between and \ like \ in \ null

-- LIKE --
-- 查询姓刘的同学的学号及姓名
-- like结合使用的通配符 : % (代表0到任意个字符) _ (一个字符)
SELECT studentno,studentname FROM student
WHERE studentname LIKE '刘%';

-- 查询姓刘的同学,后面只有一个字的
SELECT studentno,studentname FROM student
WHERE studentname LIKE '刘_';

-- 查询姓刘的同学,后面只有两个字的
SELECT studentno,studentname FROM student
WHERE studentname LIKE '刘__';

-- 查询姓名中含有 嘉 字的
SELECT studentno,studentname FROM student
WHERE studentname LIKE '%嘉%';

-- 查询姓名中含有特殊字符的需要使用转义符号 '\'
-- 自定义转义符关键字: ESCAPE ':'

-- IN --
-- 查询学号为1000,1001,1002的学生姓名
SELECT studentno,studentname FROM student
WHERE studentno IN (1000,1001,1002);

-- 查询地址在北京,南京,河南洛阳的学生
SELECT studentno,studentname,address FROM student
WHERE address IN ('北京','南京','河南洛阳');

-- NULL 空 --
-- 查询出生日期没有填写的同学
-- 不能直接写=NULL , 这是代表错误的 , 用 is null
SELECT studentname FROM student
WHERE BornDate IS NULL;

-- 查询出生日期填写的同学
SELECT studentname FROM student
WHERE BornDate IS NOT NULL;

-- 查询没有写家庭住址的同学(空字符串不等于null)
SELECT studentname FROM student
WHERE Address='' OR Address IS NULL;
```



### 4.4 联表查询

> join对比

![图片](https://mmbiz.qpic.cn/mmbiz_png/uJDAUKrGC7LwfjFbCQXic0pcE21lUFGvDowQf1HHaYIicELYKnU9kDeaFHnfx0GYW6AsEwoTySywn91ia8Wz2sXiaA/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

七种Join：

![图片](https://mmbiz.qpic.cn/mmbiz_png/uJDAUKrGC7LwfjFbCQXic0pcE21lUFGvDw5aZLehIYzwLprCfqdxSjsm2wficHrSEzJiaJBGaKWpatQ7sISib9MgCQ/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

```sql
/* 
内连接 inner join
   查询两个表中的结果集中的交集
外连接 outer join
   左外连接 left join
       (以左表作为基准,右边表来一一匹配,匹配不上的,返回左表的记录,右表以NULL填充)
   右外连接 right join
       (以右表作为基准,左边表来一一匹配,匹配不上的,返回右表的记录,左表以NULL填充)
       
等值连接和非等值连接
-- join（连接的表） on（判断的条件）连接查询
-- where 等值查询

思路
(1):分析需求,确定查询的列来源于两个类,student result,连接查询
(2):确定使用哪种连接查询? 7种 (内连接)
确定交叉点（这两个表中的哪个数据是相同的）
判断的条件：学生表中studentNo = 成绩表 studentNo
*/
-- 查询参加了考试的同学信息(学号,学生姓名,科目编号,分数)
SELECT s.studentno,studentname,subjectno,StudentResult
FROM student s
INNER JOIN result r
ON r.studentno = s.studentno

-- 右连接(也可实现)
SELECT s.studentno,studentname,subjectno,StudentResult
FROM student s
RIGHT JOIN result r
ON r.studentno = s.studentno

-- 等值连接
SELECT s.studentno,studentname,subjectno,StudentResult
FROM student s , result r
WHERE r.studentno = s.studentno

-- 左连接 (查询了所有同学,不考试的也会查出来)
SELECT s.studentno,studentname,subjectno,StudentResult
FROM student s
LEFT JOIN result r
ON r.studentno = s.studentno

-- 查一下缺考的同学(左连接应用场景)
SELECT s.studentno,studentname,subjectno,StudentResult
FROM student s
LEFT JOIN result r
ON r.studentno = s.studentno
WHERE StudentResult IS NULL

-- 思考题:查询参加了考试的同学信息(学号,学生姓名,科目名,分数)
SELECT s.studentno,studentname,subjectname,StudentResult
FROM student s
INNER JOIN result r
ON r.studentno = s.studentno
INNER JOIN `subject` sub
ON sub.subjectno = r.subjectno 

-- 查询学员及所属的年级(学号,学生姓名,年级名)
SELECT studentno AS 学号,studentname AS 学生姓名,gradename AS 年级名称
FROM student s
INNER JOIN grade g
ON s.`GradeId` = g.`GradeID`

-- 查询科目及所属的年级(科目名称,年级名称)
SELECT subjectname AS 科目名称,gradename AS 年级名称
FROM SUBJECT sub
INNER JOIN grade g
ON sub.gradeid = g.gradeid

-- 查询 数据库结构-1 的所有考试结果(学号 学生姓名 科目名称 成绩)
SELECT s.studentno,studentname,subjectname,StudentResult
FROM student s
INNER JOIN result r
ON r.studentno = s.studentno
INNER JOIN `subject` sub
ON r.subjectno = sub.subjectno
WHERE subjectname='数据库结构-1'
```

| 操作       | 描述                                         |
| ---------- | -------------------------------------------- |
| Inner join | 如果表中至少有一个匹配，就返回行             |
| left join  | 即使右表中没有匹配，也会从左表中返回所有的值 |
| right join |                                              |

> 自连接（了解即可）

```sql
/*
自连接
   数据表与自身进行连接

需求:从一个包含栏目ID , 栏目名称和父栏目ID的表中
    查询父栏目名称和其他子栏目名称
*/

-- 创建一个表
CREATE TABLE `category` (
`categoryid` INT(10) UNSIGNED NOT NULL AUTO_INCREMENT COMMENT '主题id',
`pid` INT(10) NOT NULL COMMENT '父id',
`categoryName` VARCHAR(50) NOT NULL COMMENT '主题名字',
PRIMARY KEY (`categoryid`)
) ENGINE=INNODB AUTO_INCREMENT=9 DEFAULT CHARSET=utf8

-- 插入数据
INSERT INTO `category` (`categoryid`, `pid`, `categoryName`)
VALUES('2','1','信息技术'),
('3','1','软件开发'),
('4','3','数据库'),
('5','1','美术设计'),
('6','3','web开发'),
('7','5','ps技术'),
('8','2','办公信息');

-- 编写SQL语句,将栏目的父子关系呈现出来 (父栏目名称,子栏目名称)
-- 核心思想:把一张表看成两张一模一样的表,然后将这两张表连接查询(自连接)
SELECT a.categoryName AS '父栏目',b.categoryName AS '子栏目'
FROM category AS a,category AS b
WHERE a.`categoryid`=b.`pid`
```

### 4.5 分页和排序

> 排序（ORDER BY）

```sql
/* 语法 : ORDER BY
   ORDER BY 语句用于根据指定的列对结果集进行排序。
   ORDER BY 语句默认按照ASC升序对记录进行排序。
   如果您希望按照降序对记录进行排序，可以使用 DESC 关键字。 
*/
-- 查询 数据库结构-1 的所有考试结果(学号 学生姓名 科目名称 成绩)
-- 按成绩降序排序
SELECT s.studentno,studentname,subjectname,StudentResult
FROM student s
INNER JOIN result r
ON r.studentno = s.studentno
INNER JOIN `subject` sub
ON r.subjectno = sub.subjectno
WHERE subjectname='数据库结构-1'
ORDER BY StudentResult DESC
```

> 分页（）

```sql
/* 语法 : SELECT * FROM table LIMIT [offset,] rows | rows OFFSET offset
limit 查询起始值（下标），页面的大小
好处 : (用户体验,网络传输,查询压力)

推导:
   第一页 : limit 0,5
   第二页 : limit 5,5
   ......
   第N页 : limit (pageNo-1)*pageSzie,pageSzie
   [pageNo:页码,pageSize:单页面显示条数]
   
*/
-- 每页显示5条数据
SELECT s.studentno,studentname,subjectname,StudentResult
FROM student s
INNER JOIN result r
ON r.studentno = s.studentno
INNER JOIN `subject` sub
ON r.subjectno = sub.subjectno
WHERE subjectname='数据库结构-1'
ORDER BY StudentResult DESC , studentno
LIMIT 0,5

-- 查询 JAVA第一学年 课程成绩前10名并且分数大于80的学生信息(学号,姓名,课程名,分数)
SELECT s.studentno,studentname,subjectname,StudentResult
FROM student s
INNER JOIN result r
ON r.studentno = s.studentno
INNER JOIN `subject` sub
ON r.subjectno = sub.subjectno
WHERE subjectname='JAVA第一学年'
ORDER BY StudentResult DESC
LIMIT 0,10
```

### 4.6 子查询

```sql
/*
   在查询语句中的WHERE条件子句中,又嵌套了另一个查询语句
   嵌套查询可由多个子查询组成,求解的方式是由里及外;
   子查询返回的结果一般都是集合,故而建议使用IN关键字;
*/

-- 查询 数据库结构-1 的所有考试结果(学号,科目编号,成绩),并且成绩降序排列
-- 方法一:使用连接查询
SELECT studentno,r.subjectno,StudentResult
FROM result r
INNER JOIN `subject` sub
ON r.`SubjectNo`=sub.`SubjectNo`
WHERE subjectname = '数据库结构-1'
ORDER BY studentresult DESC;

-- 方法二:使用子查询(执行顺序:由里及外)
SELECT studentno,subjectno,StudentResult
FROM result
WHERE subjectno=(
   SELECT subjectno FROM `subject`
   WHERE subjectname = '数据库结构-1'
)
ORDER BY studentresult DESC;

-- 查询课程为 高等数学-2 且分数不小于80分的学生的学号和姓名
-- 方法一:使用连接查询
SELECT s.studentno,studentname
FROM student s
INNER JOIN result r
ON s.`StudentNo` = r.`StudentNo`
INNER JOIN `subject` sub
ON sub.`SubjectNo` = r.`SubjectNo`
WHERE subjectname = '高等数学-2' AND StudentResult>=80

-- 方法二:使用连接查询+子查询
-- 分数不小于80分的学生的学号和姓名
SELECT r.studentno,studentname FROM student s
INNER JOIN result r ON s.`StudentNo`=r.`StudentNo`
WHERE StudentResult>=80

-- 在上面SQL基础上,添加需求:课程为 高等数学-2
SELECT r.studentno,studentname FROM student s
INNER JOIN result r ON s.`StudentNo`=r.`StudentNo`
WHERE StudentResult>=80 AND subjectno=(
   SELECT subjectno FROM `subject`
   WHERE subjectname = '高等数学-2'
)

-- 方法三:使用子查询
-- 分步写简单sql语句,然后将其嵌套起来
SELECT studentno,studentname FROM student WHERE studentno IN(
   SELECT studentno FROM result WHERE StudentResult>=80 AND subjectno=(
       SELECT subjectno FROM `subject` WHERE subjectname = '高等数学-2'
  )
)

/*
练习题目:
   查 C语言-1 的前5名学生的成绩信息(学号,姓名,分数)
   使用子查询,查询郭靖同学所在的年级名称
*/
```

# 5 MySQL函数

### 5.1 常用函数

**数据函数**

```sql
SELECT ABS(-8);  /*绝对值*/
SELECT CEILING(9.4); /*向上取整*/
SELECT FLOOR(9.4);   /*向下取整*/
SELECT RAND();  /*随机数：返回一个0-1之间的随机数*/
SELECT SIGN(0); /*符号函数：负数返回-1,正数返回1,0返回0*/
```

**字符串函数**

```sql
SELECT CHAR_LENGTH('狂神说坚持就能成功'); /*返回字符串包含的字符数*/
SELECT CONCAT('我','爱','程序');  /*合并字符串,参数可以有多个*/
SELECT INSERT('我爱编程helloworld',1,2,'超级热爱');  /*替换字符串,从某个位置开始替换某个长度,我爱-->超级热爱*/
SELECT LOWER('KuangShen'); /*小写*/
SELECT UPPER('KuangShen'); /*大写*/
SELECT INSTR('kuangsheen','h')/*返回第一次出现的字串索引*/
SELECT LEFT('hello,world',5);   /*从左边截取*/
SELECT RIGHT('hello,world',5);  /*从右边截取*/
SELECT REPLACE('狂神说坚持就能成功','坚持','努力');  /*替换字符串,坚持-->努力*/
SELECT SUBSTR('狂神说坚持就能成功',4,6); /*截取字符串,截取的开始位置和截取长度*/
SELECT REVERSE('狂神说坚持就能成功'); /*反转*/
 
-- 查询姓周的同学,改成邹
SELECT REPLACE(studentname,'周','邹') AS 新名字
FROM student WHERE studentname LIKE '周%';
```

**日期和时间函数（重要）**

```sql
SELECT CURRENT_DATE();
SELECT CURDATE();   /*获取当前日期*/
--
SELECT NOW();   /*获取当前日期和时间*/
SELECT LOCALTIME();   /*获取本地时间*/
SELECT SYSDATE();   /*获取系统时间*/

-- 获取年月日,时分秒
SELECT YEAR(NOW());
SELECT MONTH(NOW());
SELECT DAY(NOW());
SELECT HOUR(NOW());
SELECT MINUTE(NOW());
SELECT SECOND(NOW());
```

**系统信息函数**

```sql
SELECT VERSION();  /*版本*/
SELECT USER();     /*用户*/
```

### 聚合函数（更常用）

| 函数名称 | 描述                                                         |
| -------- | ------------------------------------------------------------ |
| COUNT()  | 返回满足Select条件的记录总和数，如 select count(*) 【不建议使用 *，效率低】 |
| SUM()    | 返回数字字段或表达式列作统计，返回一列的总和。               |
| AVG()    | 通常为数值字段或表达列作统计，返回一列的平均值               |
| MAX()    | 可以为数值字段，字符字段或表达式列作统计，返回最大的值。     |
| MIN()    | 可以为数值字段，字符字段或表达式列作统计，返回最小的值。     |

```sql
 -- 聚合函数
 /*COUNT:非空的*/
 SELECT COUNT(studentname) FROM student;
 SELECT COUNT(*) FROM student;
 SELECT COUNT(1) FROM student;  /*推荐*/
 
 -- 从含义上讲，count(1) 与 count(*) 都表示对全部数据行的查询。
 -- count(字段) 会统计该字段在表中出现的次数，忽略字段为null 的情况。即不统计字段为null 的记录。
 -- count(*) 包括了所有的列，相当于行数，在统计结果的时候，包含字段为null 的记录；
 -- count(1) 用1代表代码行，在统计结果的时候，包含字段为null 的记录 。
 /*
 很多人认为count(1)执行的效率会比count(*)高，原因是count(*)会存在全表扫描，而count(1)可以针对一个字段进行查询。其实不然，count(1)和count(*)都会对全表进行扫描，统计所有记录的条数，包括那些为null的记录，因此，它们的效率可以说是相差无几。而count(字段)则与前两者不同，它会统计该字段不为null的记录条数。
 
 下面它们之间的一些对比：
 
 1）在表没有主键时，count(1)比count(*)快
 2）有主键时，主键作为计算条件，count(主键)效率最高；
 3）若表格只有一个字段，则count(*)效率较高。
 */
 
 SELECT SUM(StudentResult) AS 总和 FROM result;
 SELECT AVG(StudentResult) AS 平均分 FROM result;
 SELECT MAX(StudentResult) AS 最高分 FROM result;
 SELECT MIN(StudentResult) AS 最低分 FROM result;
```

**题目：**

```sql
-- 查询不同课程的平均分,最高分,最低分
-- 前提:根据不同的课程进行分组
SELECT subjectname,AVG(studentresult) AS 平均分,MAX(StudentResult) AS 最高分,MIN(StudentResult) AS 最低分
FROM result AS r
INNER JOIN `subject` AS s
ON r.subjectno = s.subjectno
GROUP BY r.subjectno
HAVING 平均分>80;
 
/*
where写在group by前面.
要是放在分组后面的筛选
要使用HAVING..
因为having是从前面筛选的字段再筛选，而where是从数据表中的>字段直接进行的筛选的
*/
```

> MD5 加密（密码加密）（函数的用法）

**一、MD5简介**

MD5即Message-Digest Algorithm 5（信息-摘要算法5），用于确保信息传输完整一致。是计算机广泛使用的杂凑算法之一（又译摘要算法、哈希算法），主流编程语言普遍已有MD5实现。将数据（如汉字）运算为另一固定长度值，是杂凑算法的基础原理。

**二、实现数据加密**

新建一个表 testmd5

```sql
 CREATE TABLE `testmd5` (
  `id` INT(4) NOT NULL,
  `name` VARCHAR(20) NOT NULL,
  `pwd` VARCHAR(50) NOT NULL,
  PRIMARY KEY (`id`)
 ) ENGINE=INNODB DEFAULT CHARSET=utf8
```

插入一些数据

```sql
 INSERT INTO testmd5 VALUES(1,'kuangshen','123456'),(2,'qinjiang','456789')
```

如果我们要对pwd这一列数据进行加密，语法是：

```sql
 update testmd5 set pwd = md5(pwd);
```

如果单独对某个用户(如kuangshen)的密码加密：

```sql
 INSERT INTO testmd5 VALUES(3,'kuangshen2','123456')
 update testmd5 set pwd = md5(pwd) where name = 'kuangshen2';
```

插入新的数据自动加密

```sql
 INSERT INTO testmd5 VALUES(4,'kuangshen3',md5('123456'));
```

查询登录用户信息（md5对比使用，查看用户输入加密后的密码进行比对）

```sql
 SELECT * FROM testmd5 WHERE `name`='kuangshen' AND pwd=MD5('123456');
```



![image-20211019203856812](C:\Users\vivianye7\AppData\Roaming\Typora\typora-user-images\image-20211019203856812.png)

![image](https://user-images.githubusercontent.com/75358006/138011894-2b638c36-ea79-4db1-ac60-097f55140b39.png)


# 6 事务

### 6.1 什么是事务

- 要么都成功，要么都失败

- 将一组SQL放在一个批次中执行

> 事务原则：ACID原则，即原子性，一致性，隔离性，持久性 （脏读，幻读）

**原子性(Atomic)**

- 整个事务中的所有操作，要么全部完成，要么全部不完成，不可能停滞在中间某个环节。事务在执行过程中发生错误，会被回滚（ROLLBACK）到事务开始前的状态，就像这个事务从来没有执行过一样。

**一致性(Consist)**

- 事务前后的数据完整性必须始终保持处于一致的状态，不管在任何给定的时间并发事务有多少。也就是说：如果事务是并发多个，系统也必须如同串行事务一样操作。

**持久性(Durable)** -- 事务提交

- 事务一旦提交则不可逆，被持久化到数据库中

**隔离性(Isolated)**

- 多个用户并发访问数据库时，数据库为每一个用户开启的事务，不能被其他事务的操作数据所干扰



> 隔离所导致的一些问题

脏读：指一个事务读取了另外一个事务未提交的数据

不可重复读：在一个事务内多次读取的数据结果不同

虚度（幻读）：在一个事务内读取到了别的事务插入的数据，导致前后读取不一致

> 事务

```sql
-- 注意:
--- 1.MySQL中默认是自动提交
--- 2.使用事务时应先关闭自动提交

-- 手动处理事务：使用set语句来改变自动提交模式
SET autocommit = 0;   /*关闭*/
SET autocommit = 1;   /*开启*/

-- 事务开启,标记事务的起始点,一组事务
START TRANSACTION  

-- 提交一个事务给数据库：持久化（成功！）
COMMIT

-- 将事务回滚,数据回到本次事务的初始状态（失败！）
ROLLBACK

-- 事务结束,还原MySQL数据库的自动提交
SET autocommit =1;

-- 存档
SAVEPOINT 保存点名称 -- 设置一个事务保存点
ROLLBACK TO SAVEPOINT 保存点名称 -- 回滚到保存点
RELEASE SAVEPOINT 保存点名称 -- 删除保存点
```

> 测试

```sql
/*
课堂测试题目

A在线买一款价格为500元商品,网上银行转账.
A的银行卡余额为2000,然后给商家B支付500.
商家B一开始的银行卡余额为10000

创建数据库shop和创建表account并插入2条数据
*/

CREATE DATABASE `shop`
CHARACTER SET utf8 COLLATE utf8_general_ci;
USE `shop`;

CREATE TABLE `account` (
`id` INT(11) NOT NULL AUTO_INCREMENT,
`name` VARCHAR(32) NOT NULL,
`cash` DECIMAL(9,2) NOT NULL,
PRIMARY KEY (`id`)
) ENGINE=INNODB DEFAULT CHARSET=utf8

INSERT INTO account (`name`,`cash`)
VALUES('A',2000.00),('B',10000.00)

-- 转账实现
SET autocommit = 0; -- 关闭自动提交
START TRANSACTION;  -- 开始一个事务,标记事务的起始点
UPDATE account SET cash=cash-500 WHERE `name`='A';
UPDATE account SET cash=cash+500 WHERE `name`='B';
COMMIT; -- 提交事务
# rollback;
SET autocommit = 1; -- 恢复自动提交
```



# 7 索引

https://blog.codinglabs.org/articles/theory-of-mysql-index.html MySQL索引背后的数据结构及算法原理

> MySQL官方对索引的定义为：索引（Index）是帮助MySQL高效获取数据的数据结构。
>
> 提取句子主干，就可以得到索引的本质：索引是数据结构。

### 7.1 索引的分类

> 在一个表中，主键索引只能有一个，唯一索引可以有多个

- 主键索引 (Primary Key)
  - 唯一的标识，主键不可重复且唯一
- 唯一索引 (Unique)
  - 避免重复的列出现（保证唯一性），但其实可以重复，多个列都可以标识为唯一索引
- 常规索引 (Index/Key)
- 全文索引 (FullText)
  - 在特定数据库引擎下才有，MYISAM
  - 快速定位数据

```sql
/* 索引的使用
#方法一：创建表时
  　　CREATE TABLE 表名 (
               字段名1 数据类型 [完整性约束条件…],
               字段名2 数据类型 [完整性约束条件…],
               [UNIQUE | FULLTEXT | SPATIAL ]   INDEX | KEY
               [索引名] (字段名[(长度)] [ASC |DESC])
               );


#方法二：CREATE在已存在的表上创建索引
       CREATE [UNIQUE | FULLTEXT | SPATIAL ] INDEX 索引名
                    ON 表名 (字段名[(长度)] [ASC |DESC]) ;


#方法三：ALTER TABLE在已存在的表上创建索引
       ALTER TABLE 表名 ADD [UNIQUE | FULLTEXT | SPATIAL ] INDEX
                            索引名 (字段名[(长度)] [ASC |DESC]) ;
                           
                           
#删除索引：DROP INDEX 索引名 ON 表名字;
#删除主键索引: ALTER TABLE 表名 DROP PRIMARY KEY;

*/
/*显示索引信息*/
SHOW INDEX FROM student;

/*增加全文索引*/
ALTER TABLE `school`.`student` ADD FULLTEXT INDEX `studentname` (`StudentName`);

/*EXPLAIN : 分析SQL语句执行性能*/
/*非全文索引*/
EXPLAIN SELECT * FROM student WHERE studentno='1000';

/*使用全文索引*/
-- 全文搜索通过 MATCH() 函数完成。
-- 搜索字符串作为 against() 的参数被给定。搜索以忽略字母大小写的方式执行。对于表中的每个记录行，MATCH() 返回一个相关性值。即，在搜索字符串与记录行在 MATCH() 列表中指定的列的文本之间的相似性尺度。
EXPLAIN SELECT *FROM student WHERE MATCH(studentname) AGAINST('love');

/*
开始之前，先说一下全文索引的版本、存储引擎、数据类型的支持情况

MySQL 5.6 以前的版本，只有 MyISAM 存储引擎支持全文索引；
MySQL 5.6 及以后的版本，MyISAM 和 InnoDB 存储引擎均支持全文索引;
只有字段的数据类型为 char、varchar、text 及其系列才可以建全文索引。
测试或使用全文索引时，要先看一下自己的 MySQL 版本、存储引擎和数据类型是否支持全文索引。
*/
```

> 拓展：测试索引

**建表app_user：**

```sql
CREATE TABLE `app_user` (
`id` bigint(20) unsigned NOT NULL AUTO_INCREMENT,
`name` varchar(50) DEFAULT '' COMMENT '用户昵称',
`email` varchar(50) NOT NULL COMMENT '用户邮箱',
`phone` varchar(20) DEFAULT '' COMMENT '手机号',
`gender` tinyint(4) unsigned DEFAULT '0' COMMENT '性别（0:男；1：女）',
`password` varchar(100) NOT NULL COMMENT '密码',
`age` tinyint(4) DEFAULT '0' COMMENT '年龄',
`create_time` datetime DEFAULT CURRENT_TIMESTAMP,
`update_time` timestamp NULL DEFAULT CURRENT_TIMESTAMP ON UPDATE CURRENT_TIMESTAMP,
PRIMARY KEY (`id`)
) ENGINE=InnoDB DEFAULT CHARSET=utf8mb4 COMMENT='app用户表'
```

**批量插入数据：100w**

```sql
DROP FUNCTION IF EXISTS mock_data;
DELIMITER $$ -- 写函数之前必须要写，标志
CREATE FUNCTION mock_data() -- 模拟数据
RETURNS INT -- 返回值类型

BEGIN
    DECLARE num INT DEFAULT 1000000;
    DECLARE i INT DEFAULT 0;
    WHILE i < num DO
      INSERT INTO app_user(`name`, `email`, `phone`, `gender`, `password`, `age`)
       VALUES(CONCAT('用户', i), '24736743@qq.com', CONCAT('18', FLOOR(RAND()*(999999999-100000000)+100000000)),FLOOR(RAND()*2),UUID(), FLOOR(RAND()*100));
      SET i = i + 1;
    END WHILE;
    RETURN i;
END;

SELECT mock_data();
```

**索引效率测试**

```sql
-- 无索引
SELECT * FROM app_user WHERE name = '用户9999'; -- 查看耗时
SELECT * FROM app_user WHERE name = '用户9999';
SELECT * FROM app_user WHERE name = '用户9999';

EXPLAIN SELECT * FROM app_user WHERE name = '用户9999'; -- 约1s

-- 创建索引
CREATE INDEX idx_app_user_name ON app_user(name);

-- 测试普通索引
EXPLAIN SELECT * FROM app_user WHERE name = '用户9999'; -- 约0.001s
```

索引在小数据量的时候影响不大，但是在大数据量的时候影响比较大

### 7.3 索引原则

> 索引准则

- 索引不是越多越好
- 不要对经常变动的数据加索引
- 小数据量的表建议不要加索引
- 索引一般应加在查找条件的字段

> 索引的数据结构

```sql
-- 我们可以在创建上述索引的时候，为其指定索引类型，分两类
hash类型的索引：查询单条快，范围查询慢
btree类型的索引：b+树，层数越多，数据量指数级增长（我们就用它，因为innodb默认支持它）

-- 不同的存储引擎支持的索引类型也不一样
InnoDB 支持事务，支持行级别锁定，支持 B-tree、Full-text 等索引，不支持 Hash 索引；
MyISAM 不支持事务，支持表级别锁定，支持 B-tree、Full-text 等索引，不支持 Hash 索引；
```

# 8 权限管理和备份

### 8.1 用户管理

> 使用SQLyog 创建用户，并授予权限演示

![image-20211021143952131](C:\Users\vivianye7\AppData\Roaming\Typora\typora-user-images\image-20211021143952131.png)

> 基本命令

本质：读这张表进行增删改查

```sql
/* 用户和权限管理 */ ------------------
用户信息表：mysql.user

-- 刷新权限
FLUSH PRIVILEGES

-- 增加用户 CREATE USER kuangshen IDENTIFIED BY '123456'
CREATE USER 用户名 IDENTIFIED BY [PASSWORD] 密码(字符串)
  - 必须拥有mysql数据库的全局CREATE USER权限，或拥有INSERT权限。
  - 只能创建用户，不能赋予权限。
  - 用户名，注意引号：如 'user_name'@'192.168.1.1'
  - 密码也需引号，纯数字密码也要加引号
  - 要在纯文本中指定密码，需忽略PASSWORD关键词。要把密码指定为由PASSWORD()函数返回的混编值，需包含关键字PASSWORD

-- 重命名用户 RENAME USER kuangshen TO kuangshen2
RENAME USER old_user TO new_user

-- 设置/修改密码
SET PASSWORD = PASSWORD('密码')    -- 为当前用户设置密码
SET PASSWORD FOR 用户名 = PASSWORD('密码')    -- 为指定用户设置密码

-- 删除用户 DROP USER kuangshen2
DROP USER 用户名

-- 分配权限/添加用户
GRANT 权限列表 ON 表名 TO 用户名 [IDENTIFIED BY [PASSWORD] 'password']
  - all privileges 表示所有权限，除了给别人授权grant
  - *.* 表示所有库的所有表
  - 库名.表名 表示某库下面的某表

-- 查看权限   SHOW GRANTS FOR root@localhost;
SHOW GRANTS FOR 用户名
-- 查看当前用户权限
SHOW GRANTS; 
SHOW GRANTS FOR CURRENT_USER; 
SHOW GRANTS FOR CURRENT_USER();

-- 撤消权限
REVOKE 权限列表 ON 表名 FROM 用户名
REVOKE ALL PRIVILEGES, GRANT OPTION FROM 用户名    -- 撤销所有权限
```

### 8.2 MySQL备份

数据库备份必要性：

- 保证重要数据不丢失
- 数据转移

MySQL数据库备份方法

- 直接拷贝数据库文件和相关配置文件
- 数据库管理工具,如SQLyog
  - 在想要导出的表或者库中，右键选择备份或导出

![image-20211022141348664](C:\Users\vivianye7\AppData\Roaming\Typora\typora-user-images\image-20211022141348664.png)

- 使用命令行mysqldump备份工具

![图片](https://mmbiz.qpic.cn/mmbiz_png/uJDAUKrGC7Jf7deolwQa44rXvicIhXZ0NzgWJWeyYYcf1Dy3ibfN66SiaZQmqTF3Hv8HBjr1zIowXh201pEjUzyJw/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

```sql
-- 导出
1. 导出一张表 -- mysqldump -uroot -p123456 school student >D:/a.sql
　　mysqldump -u用户名 -p密码 库名 表名 > 文件名(D:/a.sql)
2. 导出多张表 -- mysqldump -uroot -p123456 school student result >D:/a.sql
　　mysqldump -u用户名 -p密码 库名 表1 表2 表3 > 文件名(D:/a.sql)
3. 导出所有表 -- mysqldump -uroot -p123456 school >D:/a.sql
　　mysqldump -u用户名 -p密码 库名 > 文件名(D:/a.sql)
4. 导出一个库 -- mysqldump -uroot -p123456 -B school >D:/a.sql
　　mysqldump -u用户名 -p密码 -B 库名 > 文件名(D:/a.sql)

可以-w携带备份条件

-- 导入
1. 在登录mysql的情况下，先切换到数据库的表：-- source D:/a.sql
　　source 备份文件地址
2. 在不登录的情况下
　　mysql -u用户名 -p密码 库名 < 备份文件
```

假设你要备份数据库，防止数据丢失

**mysqldump客户端**

作用 :

- 转储数据库
- 搜集数据库进行备份
- 将数据转移到另一个SQL服务器,不一定是MySQL服务器



# 9 规范数据库设计

### 9.1 为什么需要设计

> **当数据库比较复杂时我们需要设计数据库**

**糟糕的数据库设计 :** 

- 数据冗余,存储空间浪费
- 数据更新和插入的麻烦、异常（屏蔽使用物理外键）
- 程序性能差

**良好的数据库设计 :** 

- 节省数据的存储空间
- 能够保证数据的完整性
- 方便进行数据库应用系统的开发
- 

> **软件项目开发周期中数据库设计 :**

- 需求分析阶段: 分析客户的业务和数据处理需求
- 概要设计阶段:设计数据库的E-R模型图 , 确认需求信息的正确和完整.



> **设计数据库步骤（个人博客）**

- 收集信息，分析需求

  - 用户表（用户登录注销，用户的个人信息，写博客，创建分类）
  - 分类表（文章分类，谁创建的）
  - 文章表（文章信息）
  - 评论表（）
  - 友情链接表（友链信息）
  - 自定义表（系统信息，某个关键的字，一些主题）key: value

- - 与该系统有关人员进行交流 , 座谈 , 充分了解用户需求 , 理解数据库需要完成的任务.

- 标识实体[Entity] （把需求落地到每个字段）

- 

- - 标识数据库要管理的关键对象或实体,实体一般是名词

- 标识每个实体需要存储的详细信息[Attribute]

- 标识实体之间的关系[Relationship]

  - 写博客：user -> blog
  - 创建分类：user -> category
  - 关注：user -> user
  - 友链：link
  - 评论：user->user->blog

# 9.2 三大范式

**问题 : 为什么需要数据规范化?**

不合规范的表设计会导致的问题：

- 信息重复

- 更新异常

- 插入异常

- - 无法正确表示信息

- 删除异常

- - 丢失有效信息

> 三大范式

**第一范式 (1st NF)**

原子性：如果每列都是不可再分的最小数据单元,则满足第一范式

**第二范式(2nd NF)**

前提：满足第一范式（1NF）

完全依赖：第二范式要求每个表只描述一件事情

**第三范式(3rd NF)**

前提：满足第一范式和第二范式

如果一个关系满足第二范式,并且除了主键以外的其他列都不传递依赖于主键列,则满足第三范式.

直接依赖：第三范式需要确保数据表中的每一列数据都和主键直接相关，而不能间接相关。

> **规范化和性能的关系**

- 为满足商业化的目标和需求 , 数据库性能比规范化数据库更重要

- 在数据规范化的同时 , 要综合考虑数据库的性能

- 故意给某些表添加冗余的字段,以大量减少需要从中搜索信息所需的时间（多表查询 -> 单表查询）

- 故意插入一些计算列,以方便查询（大数据量 -> 小数据量：索引）

# 10 JDBC（重要）

### 10.1 数据库驱动

驱动：声卡、显卡、数据库

![image-20211025154505094](C:\Users\vivianye7\AppData\Roaming\Typora\typora-user-images\image-20211025154505094.png)

我们的程序会通过数据库驱动，和数据库打交道！

### 10.2 JDBC

SUN公司为了简化开发人员的操作（对数据库的统一操作），提供了一个（Java操作数据库）规范，俗称JDBC

这些规范的实现由具体的厂商去做

对于开发人员来说，只需要掌握JDBC接口的操作即可

![image-20211025154757656](C:\Users\vivianye7\AppData\Roaming\Typora\typora-user-images\image-20211025154757656.png)

java.sql  javax.sql

还需要导入一个数据库驱动包 mysql-connector-java-5.1.47.jar

### 10.3 第一个JDBC程序

> 创建测试数据库

```sql
CREATE DATABASE `jdbcStudy` CHARACTER SET utf8 COLLATE utf8_general_ci;

USE `jdbcStudy`;

CREATE TABLE `users`(
 `id` INT PRIMARY KEY,
 `NAME` VARCHAR(40),
 `PASSWORD` VARCHAR(40),
 `email` VARCHAR(60),
 birthday DATE
);

INSERT INTO `users`(`id`,`NAME`,`PASSWORD`,`email`,`birthday`)
VALUES('1','zhangsan','123456','zs@sina.com','1980-12-04'),
('2','lisi','123456','lisi@sina.com','1981-12-04'),
('3','wangwu','123456','wangwu@sina.com','1979-12-04')
```

1、创建一个普通项目

2、导入数据库驱动

![image-20211025160202008](C:\Users\vivianye7\AppData\Roaming\Typora\typora-user-images\image-20211025160202008.png)

3、编写测试代码

```java
package com.kuang.lesson01;

import java.sql.*;

//我的第一个JDBC程序
public class JDBCFirstDemo {
    public static void main(String[] args) throws ClassNotFoundException, SQLException {
        //1.加载驱动
        Class.forName("com.mysql.jdbc.Driver");//固定写法，加载驱动
        
        //2.输入用户信息和url
        //userUnicode=true 支持中文编码 & characterEncoding=utf8 字符编码 &useSSL=true 使用安全的连接
        String url =  "jdbc:mysql://localhost:3306/jdbcstudy?userUnicoderEn=true&charactecoding=utf8&useSSL=true";
        String username = "root";
        String password = "123456";
        
        //3.连接数据库DriverManager，返回数据库对象
        //Connection 代表数据库
        Connection connection = DriverManager.getConnection(url, username, password);//驱动管理获取连接
        
        //4.执行SQL的对象statement
        //Statement 执行sql的对象
        Statement statement = connection.createStatement();
        
        //5.执行SQL的对象 去执行SQL，可能存在结果，查看返回结果
        String sql = "SELECT * FROM users";
		//返回的结果集，结果集中封装了全部的查询出来的结果
        ResultSet resultSet = statement.executeQuery(sql);
        while(resultSet.next()){
            System.out.println("id=" + resultSet.getObject("id"));
            System.out.println("name=" + resultSet.getObject("name"));
            System.out.println("password=" + resultSet.getObject("password"));
            System.out.println("email=" + resultSet.getObject("email"));
            System.out.println("birth=" + resultSet.getObject("birthday"));

        }
        
        //6.释放连接
        resultSet.close();
        statement.close();
        connection.close();

    }
}
```

> DriverManager

```java
//DriverManager.registerDriver(new com.mysql.jdbc.Driver());
Class.forName("com.mysql.jdbc.Driver");//固定写法，加载驱动

//connection 代表数据库
Connection connection = DriverManager.getConnection(url, username, password);//驱动管理获取连接
//数据库设置自动提交
//事务提交
//事务回滚
connection.rollback();
connection.commit();
connection.setAutoCommit();
```

> URL

```sql
String url =  "jdbc:mysql://localhost:3306/jdbcstudy?userUnicoderEn=true&charactecoding=utf8&useSSL=true";

//mysql -- 3306
//协议://主机地址:端口号/数据库名?参数1&参数2&参数3
//oracle -- 1521
//jdbc:oracle:thin:@localhost:1521:sid
```



> Statement 执行sql的对象 PrepareStatement执行SQL的对象

```java
String sql = "SELECT * FROM users";//编写SQL

statement.executeQuery();//查询操作返回resultSet
statement.execute();//执行任何SQL
statement.executeUpdate();//更新、插入
```

> ResultSet查询的结果集：封装了所有的查询结果

获得指定的数据类型

```java
resultSet.getObject();//在不知道列类型的情况下使用
resultSet.getString();//如果知道列的类型，就使用指定类型
```

遍历，指针

```java
resultSet.beforeFirst();//光标移动到最前面，从最开始查数据
resultSet.afterLast();//移动到最后面
resultSet.next();//移动到下一行
resultSet.previous();//移动到前一行
resultSet.absolute(row);//移动到指定行
```

> 释放资源

```java
resultSet.close();
statement.close();
connection.close();//耗资源，用完关掉
```

### 10.4 Statement对象

Jdbc中的statement对象用于向数据发送SQL语句，想完成对数据库的增删改查，只需要通过这个对象向数据库发送增删改查语句即可。

1. Statement对象的executeUpdate方法，用于向数据库发送增、删、改的sql语句，excuteUpdate执行完后，将会返回一个整数（即增删改查语句导致了数据库几行数据发生了变化）
2. Statement对象的executeQuery方法，用于向数据库发送查询语句，返回代表查询结果的ResultSet对象



> CRUD操作-create

使用executeUpdate(String sql)方法完成数据添加操作，示例操作：

```java
Statement st = conn.createStatement();
String sql = "insert into user(...) value(...)";
int num = st.executeUpdate(sql);
if(num > 0)
	System.out.println("插入成功！");
```

> CRUD操作 – delete

```java
Statement st = conn.createStatement();
String sql = "delete from user where id = 1";
int num = st.executeUpdate(sql);
if(num>0){
	System.out.println("删除成功！");
}
```

CRUD操作 – update

```java
Statement st = conn.createStatement();
String sql = "update user set name = '' where name = '' ";
int num = st.executeUpdate(sql);
if(num>0){
	System.out.println("修改成功！");
}
```

> CRUD操作 – read

```java
Statement st = conn.createStatement();
String sql = " select * from user where id = 1 ";
int num = st.executeQuery(sql);
while(rs.next()){
	//根据获取列表的数据类型，分别调用rs的相应方法映射到JAVA对象中
}
```

> 提取工具类

1、提取工具类

```java
package com.kuang.lesson02.utils;

import java.io.IOException;
import java.io.InputStream;
import java.sql.*;
import java.util.Properties;

public class JDBCUtils {

    private static String driver = null;
    private static String url = null;
    private static String username = null;
    private static String password = null;

    static{
        
        try{
            InputStream in = JDBCUtils.class.getClassLoader().getResourceAsStream("db.properties");
            Properties properties = new Properties();//拿到流
            properties.load(in);//加载流

            driver = properties.getProperty("driver");
            url = properties.getProperty("url");
            username = properties.getProperty("username");
            password = properties.getProperty("password");

            //1.驱动只用加载一次
            Class.forName(driver);


        } catch (Exception e) {
            e.printStackTrace();
        }

    }
    //获取连接
    public static Connection getConnection() throws SQLException {
        return DriverManager.getConnection(url, username, password);
    }

    //释放连接资源
    public static void release(Connection conn, Statement st, ResultSet rs){
        if(rs != null){
            try {
                rs.close();
            } catch (SQLException e) {
                e.printStackTrace();
            }
        }
        if(st != null){
            try {
                st.close();
            } catch (SQLException e) {
                e.printStackTrace();
            }
        }
        if(conn != null){
            try {
                conn.close();
            } catch (SQLException e) {
                e.printStackTrace();
            }
        }
    }

}
```

2、编写增删改的方法，executeUpdate

```
package com.kuang.lesson02;

import com.kuang.lesson02.utils.JDBCUtils;

import java.sql.Connection;
import java.sql.ResultSet;
import java.sql.SQLException;
import java.sql.Statement;

public class TestInsert {
    public static void main(String[] args) {

        Connection conn = null;
        Statement st = null;
        ResultSet rs = null;

        try{
            conn = JDBCUtils.getConnection();//获取数据库连接
            st = conn.createStatement();//获得SQL的执行对象
            String sql = "INSERT INTO `users`(`id`,`NAME`,`PASSWORD`,`email`,`birthday`)" +
                    "VALUES('4','zhangsan','123456','zs@sina.com','1980-12-04')";
            //String sql = "delete from user where id = 1";
            //String sql = "update user set name = '' where name = ''";

            int i = st.executeUpdate(sql);//返回的结果是受影响的行数
            if(i >0){
                System.out.println("插入成功");
            }

        } catch (SQLException e) {
            e.printStackTrace();
        } finally{
            JDBCUtils.release(conn,st,rs);
        }

    }
}
```

3、查询

![image-20211026171445216](C:\Users\vivianye7\AppData\Roaming\Typora\typora-user-images\image-20211026171445216.png)



> SQL注入的问题

sql存在漏洞，会被攻击导致数据泄露

本质：SQL会被拼接 or

```java
package com.kuang.lesson02;

import com.kuang.lesson02.utils.JDBCUtils;

import java.sql.Connection;
import java.sql.ResultSet;
import java.sql.SQLException;
import java.sql.Statement;

public class SQLInjection {

    public static void main(String[] args) {
        //login("kuangshen","123456");
        login(" ' or ' 1=1 " ," ' or ' 1=1 ");
    }



    public static void login(String username,String password) {

        Connection conn = null;
        Statement st = null;
        ResultSet rs = null;

        try{
            conn = JDBCUtils.getConnection();//获取数据库连接
            st = conn.createStatement();//获得SQL的执行对象
            String sql = "SELECT * FROM users WHERE `NAME` = '" + username + "' AND `password` ='" + password + "'";

            rs = st.executeQuery(sql);//返回的结果是受影响的行数
            while(rs.next()){
                System.out.println(rs.getString("NAME"));
                System.out.println(rs.getString("password"));
            }

        } catch (SQLException e) {
            e.printStackTrace();
        } finally{
            JDBCUtils.release(conn,st,rs);
        }

    }
}
```

### 10.5 PreparedStatement对象

PreparedStatement可以防止SQL注入，效率更好

1、新增

```java
package com.kuang.lesson03;

import com.kuang.lesson02.utils.JDBCUtils;

import java.sql.Connection;
import java.sql.Date;
import java.sql.PreparedStatement;
import java.sql.SQLException;

public class TestInsert {
    public static void main(String[] args) {

        Connection conn = null;
        PreparedStatement st = null;

        try{
            conn = JDBCUtils.getConnection();

            //区别
            //使用？占位符代替参数
            String sql = "INSERT INTO `users`(`id`,`NAME`,`PASSWORD`,`email`,`birthday`) values(?,?,?,?,?)";
            //PreparedStatement 防止SQL注入的本质，把传递进来的参数当做字符
            //假设其中存在转义字符，比如'，会被直接转移
            st = conn.prepareStatement(sql);//预编译SQL，先写SQL，然后不执行

            //手动给参数赋值，参数下表，具体的值
            st.setInt(1,5);//id
            st.setString(2,"yeying");
            st.setString(3,"123456789");
            st.setString(4,"123456@qq.com");

            //注意点：sql.Date 数据库 java.sql.Date()
            // util.Date Java new Date().getTime() 获得时间戳
            st.setDate(5,new Date(new java.util.Date().getTime()));

            //执行
            int i = st.executeUpdate();
            if(i >0){
                System.out.println("插入成功");
            }



        } catch (SQLException e) {
            e.printStackTrace();
        } finally {
            JDBCUtils.release(conn,st,null);
        }

    }
}
```

2、删除

3、更新

4、查询

```java
package com.kuang.lesson03;

import com.kuang.lesson02.utils.JDBCUtils;

import java.sql.Connection;
import java.sql.PreparedStatement;
import java.sql.ResultSet;
import java.sql.SQLException;

public class TestSelect {

    public static void main(String[] args) {

        Connection conn = null;
        PreparedStatement st = null;
        ResultSet rs = null;

        try{
            conn = JDBCUtils.getConnection();

            String sql = "select * from users where id = ?";//编写SQL

            st = conn.prepareStatement(sql);//预编译

            st.setInt(1,2);//传递参数

            //执行
            st.executeQuery();

            if(rs.next()){
                System.out.println(rs.getString("NAME"));
            }

        } catch (SQLException e) {
            e.printStackTrace();
        } finally{
            JDBCUtils.release(conn,st,rs);
        }

    }
}
```

5、防止SQL注入

```java
package com.kuang.lesson03;

import com.kuang.lesson02.utils.JDBCUtils;

import java.sql.*;

public class SQLInjection {

    public static void main(String[] args) {
        //login("yeying","123456789");
        login(" '' or 1=1 " ,"123456");//防止了SQL注入
    }



    public static void login(String username,String password) {

        Connection conn = null;
        PreparedStatement st = null;
        ResultSet rs = null;

        try{
            conn = JDBCUtils.getConnection();//获取数据库连接
            //PreparedStatement 防止SQL注入的本质，把传递进来的参数当做字符
            //假设其中存在转义字符，就直接忽略，如'会被直接转义，
            String sql = "SELECT * FROM users WHERE `NAME` = ? AND `password` = ?";

            st = conn.prepareStatement(sql);
            st.setString(1,username);
            st.setString(2,password);


            rs = st.executeQuery();//查询完毕会返回一个结果集
            while(rs.next()){
                System.out.println(rs.getString("NAME"));
                System.out.println(rs.getString("password"));
            }

        } catch (SQLException e) {
            e.printStackTrace();
        } finally{
            JDBCUtils.release(conn,st,rs);
        }

    }
}
```

### 10.6 使用idea连接数据库

![image-20211027103107952](C:\Users\vivianye7\AppData\Roaming\Typora\typora-user-images\image-20211027103107952.png)

连接成功后，可以选择数据库



### 10.7 事务

> ACID原则

隔离性的问题

1、开启事务

2、一组业务执行完毕，提交事务

3、可以再catch语句中显式的定义回滚语句，但默认失败就会回滚

```java
package com.kuang.lesson04;

import com.kuang.lesson02.utils.JDBCUtils;

import java.sql.Connection;
import java.sql.PreparedStatement;
import java.sql.ResultSet;
import java.sql.SQLException;

public class TestTransaction2 {

    public static void main(String[] args) {

        Connection conn = null;
        PreparedStatement st = null;
        ResultSet rs = null;

        try{
            conn = JDBCUtils.getConnection();

            //关闭数据库的自动提交，自动会开启
            conn.setAutoCommit(false);//开启事务

            String sql1 = "update account set money = money-100 where name = 'A'";//编写SQL
            st = conn.prepareStatement(sql1);
            st.executeUpdate();

            int x = 1/0;//报错

            String sql2 = "update account set money = money+100 where name = 'B'";//编写SQL
            st = conn.prepareStatement(sql2);//预编译
            st.executeUpdate();

            //业务完毕，提交事务
            conn.commit();
            System.out.println("成功");


        } catch (SQLException e) {
            //如果失败默认回滚
            try{
                conn.rollback();//如果失败则回滚事务
            } catch (SQLException e1) {
                e1.printStackTrace();
            }
            e.printStackTrace();
        } finally{
            JDBCUtils.release(conn,st,rs);
        }

    }
}
```

### 10.9 数据库连接池

数据库连接 - 执行完毕 -释放 十分浪费系统资源

**池化技术：准备一些预先的资源，过来就连接预先准备好的**

按照常用连接数，来设置

最小连接数

最大连接数（业务最高承载上限）

等待超时：100ms



编写连接池，实现一个接口 DataSource（方法：获得连接）

> 开源数据源实现（拿来即用）

DBCP

C3P0

Druid：阿里巴巴



使用了这些数据库连接池之后，我们在项目开发中就不需要编写数据库的代码了



> DBCP

需要用到的jar包

commons-dbcp-1.4 ; commons-pool-1.6



> C3P0

需要用到的jar包

c3p0-0.9.5.5.jar ; mchange-commons-java-0.2.19.jar



> 结论

无论使用什么数据源，本质还是一样的，DataSource接口不会变，方法就不会变



![image-20211027214102772](C:\Users\vivianye7\AppData\Roaming\Typora\typora-user-images\image-20211027214102772.png)


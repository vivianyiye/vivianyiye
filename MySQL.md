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





































































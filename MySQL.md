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































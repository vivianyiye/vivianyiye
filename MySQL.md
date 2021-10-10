# 1 数据库总览 :

1.关系型数据库 ( SQL )
MySQL , Oracle , SQL Server , SQLite , DB2 , ...
关系型数据库通过外键关联来建立表与表之间的关系

2.非关系型数据库 ( NOSQL )
Redis , MongoDB , ...
非关系型数据库通常指数据以对象的形式存储在数据库中，而对象之间的关系通过每个对象自身的属性来决定

![image](https://user-images.githubusercontent.com/75358006/135716940-37312a55-13a5-42d0-a1fd-0597517c1809.png)

![image](https://user-images.githubusercontent.com/75358006/135768552-e89fcebf-1809-4fde-96f8-6a5c413e9a96.png)

4.新建一个数据库school

```
连接数据库语句 : mysql -h 服务器主机地址 -u 用户名 -p 用户密码  -- 连接数据库
update user set password=password('123456')where user='root'; -- 修改密码
flush privileges; -- 刷新数据库
-------------------
-- 所有的语句都用分号结尾
show databases; == 显示所有数据库

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

<font color=FF0000> mySQL关键字不区分大小写 </font>

### 2.1 操作数据库

1.创建

2.删除

3.使用















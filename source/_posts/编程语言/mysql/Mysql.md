---
categories:
  - 编程语言
  - mysql
---
# Mysql

### 登录

`mysql -u root -p `

### 常用命令

 - show databases； 查看所有数据库
 - use 数据库名；使用某个数据库
 - show tables； 显示当前选中数据库中的所有表
 - show tables form 数据库名； 显示某数据库中的表（不改变所选中数据库）
 - select database() ；查看所选中的数据库
 - desc 表名； 查看某表的结构
 - select * from 表名； 查看某表的全部记录
 - select version()；查看数据库版本（登陆前：mysql --version/mysql -V）
 - create table 表名( 列名 列类型...)；创建表
 - drop database if exists 库名;
 - drop table 表名;
 - 删除某表中所有记录：[mysql删除表中所有数据_MySQL删除或清空表中数据的方法_别听我说的胡话的博客-CSDN博客](https://blog.csdn.net/weixin_42511024/article/details/113125780)
 - 
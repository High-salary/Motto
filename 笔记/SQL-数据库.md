
## SQL

SQL 是用于访问和处理数据库的标准的计算机语言。

这类数据库包括：MySQL、SQL Server、Access、Oracle、Sybase、DB2 等等。
    
SQL，指结构化查询语言，全称是 Structured Query Language。
SQL 是一种标准 

##### 一些最重要的 SQL 命令
- SELECT - 从数据库中提取数据
- UPDATE - 更新数据库中的数据
- DELETE - 从数据库中删除数据
- INSERT INTO - 向数据库中插入新数据
- CREATE DATABASE - 创建新数据库
- ALTER DATABASE - 修改数据库
- CREATE TABLE - 创建新表
- ALTER TABLE - 变更（改变）数据库表
- DROP TABLE - 删除表
- CREATE INDEX - 创建索引（搜索键）
- DROP INDEX - 删除索引


#### mysql 查询语句 

##### 分页查询
```
 select * from table 
 select * from table where id=123
 select * from table where id > 15 limit 10
 //                         10起始条数  15 要查寻的条数
 select * from table limit 10,15 
 select * from table where id=10 limit 10,20
 ```
 
##### 模糊查询
```
select * from table where name like '%张%' limit 3
select * from table where name like '%张%' limit 3,5
```
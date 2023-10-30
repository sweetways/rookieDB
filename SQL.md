# SQL 

## 1. 基础知识

1. Two SubLanguages

- DDL – Data Definition Language

  create table等语句

  - Define and modify schema

- DML – Data Manipulation Language 

  即select、update等语句

  - Queries can be written intuitively.

- RDBMS responsible for efficient evaluation.
  - Choose and run algorithms for declarative queries
    - Choice of algorithm must not affect query answer.

2. Primary Key column(s)

- 不可重复
- 可以多个列共同组成——（firstname，lastname)

3. 外键约束——Foreign key

   一个表中的 FOREIGN KEY 指向另一个表中的 UNIQUE KEY(唯一约束的键)。

   - 作用：
     - FOREIGN KEY 约束用于预防破坏表之间连接的行为。
     - FOREIGN KEY 约束也能防止非法数据插入外键列，因为它必须是它指向的那个表中的值之一。

   

   ```mysql
   CREATE TABLE Orders
   (
   O_Id int NOT NULL,
   OrderNo int NOT NULL,
   P_Id int,
   PRIMARY KEY (O_Id),
   FOREIGN KEY (P_Id) REFERENCES Persons(P_Id)
   )
   ```

   1. 在创建Orders时，表的P_id上添加外键约束

   2. 当 "Orders" 表已被创建时，如需在 "P_Id" 列创建 FOREIGN KEY 约束，请使用下面的 SQL：

   ```mysql
   ALTER TABLE Orders
   ADD FOREIGN KEY (P_Id)
   REFERENCES Persons(P_Id)
   ```

   3. 撤销外键约束：

   ```mysql
   ALTER TABLE Orders
   Drop FOREIGN KEY (P_Id)
   ```

## 2. 视图

**CREATE VIEW**  *view_name*

**AS**  *select_statement*

作用：

- 让开发更加便捷
- 为了安全

**数据库中只存放了视图的定义，而并没有存放视图中的数据，这些数据存放在原来的表中。**

- CTE

  with 临时表 as(select 语句)，可以在后续使用
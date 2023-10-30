# Mysql

## 一、基础查询

笔记地址：[SQL Part 1 - Basic Queries | Database Systems (cs186berkeley.net)](https://cs186berkeley.net/notes/note1/)

1. order by

   ```mysql
   SELECT S.name, S.gpa, S.age*2 AS a2
   From Student AS S
   WHERE S.dept = "CS"
   ORDER BY S.gpa DESC, S.name ASC, a2;
   # default is asc
   ```

2. DISTINCT

   返回值中不包含重复

   ```MYSQL
   SELECT DISTINCT S.name, S.gpa, S.age*2 AS a2
   From Student AS S
   WHERE S.dept = "CS"
   
   ```

3. LIMIT

   通常和order by一起使用

   > 因为不适用order by ,select返回的是一个无序的集合,此时使用limit往往会随机返回K条数据

   ```MYSQL
   SELECT S.name, S.gpa, S.age*2 AS a2
   From Student AS S
   WHERE S.dept = "CS"
   ORDER BY S.gpa DESC, S.name ASC, a2;
   limit 3
   ```

4. Aggregates

   ```mysql
   SELECT [DISTINCT] AVG(S.gpa)
   FROM Student S
   WHERE S.dept = 'CS';
   ```

   Other: SUM,COUNT,MAX,MIN

5. GROUP BY

   ```MYSQL
   SELECT [DISTINCT] AVG(S.gpa) , S.dept
   FROM Student S
   GROUP BY S.dept
   ```

   可以根据多个列创建group

6. HAVING

   用来筛选在group by后不需要的列,要想使用必须有group by

   ```mysql
   SELECT [DISTINCT] AVG(S.gpa) , S.dept
   FROM Student S
   GROUP BY S.dept
   having count(*) > 2
   ```

   > 1. count(*)---包括所有列，返回表中的记录数，相当于统计表的行数，在统计结果的时候，不会忽略列值为NULL的记录。
   >
   > 2. count(1)---忽略所有列，1表示一个固定值，也可以用count(2)、count(3)代替，在统计结果的时候，不会忽略列值为NULL的记录。
   >
   > 3. count(列名)---只包括列名指定列，返回指定列的记录数，在统计结果的时候，会忽略列值为NULL的记录（不包括空字符串和0），即列值为NULL的记录不统计在内。
   >
   > 4. count(distinct 列名)---只包括列名指定列，返回指定列的不同值的记录数，在统计结果的时候，在统计结果的时候，会忽略列值为NULL的记录（不包括空字符串和0），即列值为NULL的记录不统计在内。
   >    

## 二、Join&Subqueries

笔记地址：[SQL Part 2 - Joins & Subqueries | Database Systems (cs186berkeley.net)](https://cs186berkeley.net/notes/note2/)

### 1. Join&子查询

- 查询语句执行流程

![1698569588926](C:\Users\12480\AppData\Roaming\Typora\typora-user-images\1698569588926.png)

1. Cross Join

   where 里的逻辑and 或者 or对应了intersect 和 union all

   但是在使用and时会造成无效查询：

   ```mysql
   SELECT R.sid
   FROM Boats B, Reserves R
   WHERE R.bid = B.bid
   AND (B.color = 'red' AND B.color = 'green');
   ```

   虽然它和以下查询等效：

   ```mysql
   SELECT R.sid
   FROM Boats B, Reserves R
   WHERE R.bid = B.bid and B.color = 'red'
   INTERSECT 
   SELECT R.sid
   FROM Boats B, Reserves R
   WHERE R.bid = B.bid and B.color = 'green';
   ```

   但显然第二种更合理

2. Union all

   把集合中的个数也加上了

   S  = {A,A,A,A,B,B,C,D}

   R = {A,A,B,B,B,B,C,E}

   S UNION ALL R = {A,A,A,A,A,A,B,B,B,B,B,B,C,C,D,E}

3. Intersect all

   S INTERSECT ALL R = {min(S,R)A , ...} = {A,A,B,B,C}

4. Except all

   S EXCEPT ALL R = {A(4-2), B(2-4), C(1-1)...} = {A,A,D}

### 2. 视图

**CREATE VIEW** *view_name*

**AS** *select_statement*

作用：

- 让开发更加便捷
- 为了安全

**数据库中只存放了视图的定义，而并没有存放视图中的数据，这些数据存放在原来的表中。**

- CTE

  with 临时表 as(select 语句)，可以在后续使用
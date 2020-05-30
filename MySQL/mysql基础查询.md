/*
select 查询列表 from  表名;

特点：
1、查询列表可以是：表中的字段、常量值、表达式、函数
2、查询的结果是一个虚拟的表格
*/

```sql
USE myemployees;
```

#1.查询表中的单个字段

```sql
SELECT last_name FROM employees;
```

#2.查询表中的多个字段

```sql
SELECT last_name,salary,email FROM employees;
```

#3.查询表中所有字段

```sql
SELECT * FROM employees;
```

#4.查询常量值

```sql
SELECT 100;
SELECT 'john';
```

#5.查询表达式

```sql
SELECT 100%98
```

#6.查询函数

```sql
SELECT ABS(-10);
```

#7.起别名
/*
①便于理解
②如果要查询的字段有重名的情况，使用别名可以区分开来

as可以换成空格
*/

```sql
SELECT 100%98 AS 结果;
SELECT last_name AS 姓,first_name AS 名 FROM employees;
```

#案列：查询salary，显示结果位 out put

```sql
SELECT salary AS "out put" FROM employees;
```

#8.去重
#关键字distinct

#案例:查询员工表中涉及到的所有的部门编号

```sql
SELECT DISTINCT department_id FROM employees;
```

#9.+的作用

/*
java中的+
1.运算符 1+1
2.连接符 str+str

mysql中的+
只有运算符功能

```sql
select 100+90; #190 两个操作数都是数值型，做加法运算*
select '123'+90; #213 其中一个位字符型，试图将字符型装换成数值型，若成功，则继续做加法运算
select 'join'+90; #90 如果失败，则将字符型数值转换成0
select null+10; #null 只要其中一方为null，则结果一定是null
```

*/
#案例：查询员工名和姓连接成一个字段，并显示为姓名

```sql
SELECT CONCAT(last_name,first_name) AS 姓名 FROM employees;
```


# Python操作MySQL数据库

## 一、基本概念

#### 1、使用python操作数据库的流程

pymysql 是用于Python链接Mysql数据库的接口，使用python操作数据库的流程：

<img src="C:\Users\86133\AppData\Roaming\Typora\typora-user-images\image-20200530184200844.png" alt="image-20200530184200844" style="zoom:80%;" />

首先依次创建Connection对象（数据库连接对象）用于打开数据库连接，创建Cursor对象（游标对象）用于执行查询和获取结果；然后执行SQL语句对数据库进行增删改查等操作并提交事务，此过程如果出现异常则使用回滚技术使数据库恢复到执行SQL语句之前的状态；最后，依次销毁Cursor对象和Connection对象。

#### 2 、Connection对象

Connection对象即为数据库连接对象，在python中可以使用pymysql.connect()方法创建Connection对象，该方法的常用参数如下：host：连接的数据库服务器主机名，默认为本地主机（localhost）；user：用户名；默认为当前用户root；passwd：密码，无默认值；db：数据库名称，无默认值；port：指定数据库服务器的连接端口，默认为3306（通常可省略）；

Connection对象常用的方法如下：

cursor()：使用当前连接创建并返回游标 ；

commit()：提交当前事务 

rollback()：回滚当前事务 

close()：关闭当前连接

#### 3、cursor对象

Cursor对象即为游标对象，用于执行查询和获取结果，在python中可以使用db.cursor()创建，db为Connection对象。Cursor对象常用的方法和属性如下：

execute()：执行数据库查询或命令，将结果从数据库获取到客户端；

cursor.fetchone():获取游标所在处的一行数据，返回元组，没有返回None；

cursor.fetchmany(size):接受size行返回结果行。如果size大于返回的结果行的数量，则会返回cursor.arraysize条数据；

cursor. fetchall():接收全部的返回结果行；

close()：关闭当前游标对象。

#### 4.事务

事务是数据库理论中一个比较重要的概念，指访问和更新数据库的一个程序执行单元，在开发时，我们以以下三种方式使用事务：

正常结束事务：conn.commit()；

异常结束事务：conn.rollback()；

关闭自动commit：设置 conn.autocommit(False)。

## 二、使用python实现对MySQL数据库的增删改查等操作

在python中操作MySQL数据库时，要使用的模块是：Python3中：pymysql（pip3 install pymysql）

所使用的环境为：vscode 1.45.1，SQLyog Community-MySQL GUI v13.1.6

#### 1、数据库的连接

```python
import pymysql
 
# 打开数据库连接
db = pymysql.connect('localhost', 'root', '123456')
# 使用cursor()方法创建一个游标对象
cursor = db.cursor()
# 使用execute()方法执行SQL查询，（）中使用MySQL命令
cursor.execute('SELECT VERSION()')
# 使用fetchone()方法获取单条数据
data = cursor.fetchone()
# 打印
print('database version: %s' % data)
# 关闭数据库连接
db.close()

```

#结果为：database version: 5.7.29-log

#### 2、创建数据库

```python
import pymysql

# 打开数据库连接
db = pymysql.connect('localhost', 'root', '123456')
# 使用cursor()方法创建一个游标对象
cursor = db.cursor()
# 使用execute()方法执行SQL，如果存在pythondb库则删除
cursor.execute(
    'DROP DATABASE IF EXISTS pythondb'
)
# 使用execute()方法创建数据库
cursor.execute('CREATE DATABASE pythondb')
# 关闭游标
cursor.close()
# 关闭数据库连接
db.close()
```

创建成功pythondb数据库

![image-20200530211957214](C:\Users\86133\AppData\Roaming\Typora\typora-user-images\image-20200530211957214.png)

#### 3、创建数据表

```python
import pymysql

# 打开数据库连接
db = pymysql.connect("localhost", "root", "123456", "pythondb")   # 加上库名pythondb
# 使用cursor()方法创建一个游标对象，游标对象用于执行查询和获取结果
cursor = db.cursor()
# 使用预处理语句创建表
sql='''CREATE TABLE `employee` (
  `first_name` varchar(255) DEFAULT NULL COMMENT '姓',
  `last_name` varchar(255) DEFAULT NULL COMMENT '名',
  `age` int(11) DEFAULT NULL COMMENT '年龄',
  `sex` varchar(255) DEFAULT NULL COMMENT '性别',
  `income` varchar(255) DEFAULT NULL COMMENT '收入'
) ENGINE=InnoDB DEFAULT CHARSET=utf8;
'''
# 使用execute()方法执行SQL语句
cursor.execute(sql)
# 关闭游标
cursor.close()
# 正常结束事务
db.commit()
# 关闭数据库连接
db.close()
```

创建成功数据表employee；可在表中看到各表项。

![image-20200530212647874](C:\Users\86133\AppData\Roaming\Typora\typora-user-images\image-20200530212647874.png)

#### 4、数据库插入操作

```python
import pymysql

# 打开数据库连接
db = pymysql.connect("localhost", "root", "123456", "pythondb")
# 使用cursor()方法创建一个游标对象，游标对象用于执行查询和获取结果
cursor = db.cursor()
# sql语句向数据表中插入数据
sql="""INSERT INTO employee(first_name,
         last_name, age, sex, income)
         VALUES ('赵', '丽颖', 38, '女', 15000),('关','晓彤',23,'女',20000),('江','疏影',33,'女',25000),('陈','赫',33,'男',20000)
"""
# 使用execute()方法执行SQL语句
cursor.execute(sql)
# 关闭游标
cursor.close()
# 正常结束事务
db.commit()
# 关闭数据库连接
db.close()
```

插入数据成功，则在SQLyog中可查看到插入的数据：

![image-20200530213441465](C:\Users\86133\AppData\Roaming\Typora\typora-user-images\image-20200530213441465.png)

#### 5、数据库查询操作

代码示例：使用fetchall()

```python
import pymysql

# 打开数据库连接
db = pymysql.connect('localhost', 'root', '123456', 'pythondb')
# 使用cursor()方法创建一个游标对象，游标对象用于执行查询和获取结果
cursor = db.cursor()
# 使用sql语句查询数据表
sql = "SELECT * FROM employee"
# 异常处理
try:
    cursor.execute(sql)    # 使用execute()方法执行SQL语句
    results = cursor.fetchall()   # 取所有数据
    for row in results:
        print(row)
        first_name = row[0]
        last_name = row[1]
        age = row[2]
        sex = row[3]
        income = row[4]
        print(first_name, last_name, age, sex, income)
except:
    print('Uable to fetch data!')
# 关闭数据库连接
db.close()
```

运行结果如图：

![image-20200530214536634](C:\Users\86133\AppData\Roaming\Typora\typora-user-images\image-20200530214536634.png)

使用fetchone()：

```python
import pymysql

# 打开数据库连接
db = pymysql.connect('localhost', 'root', '123456', 'pythondb')
# 使用cursor()方法创建一个游标对象，游标对象用于执行查询和获取结果
cursor = db.cursor()
# 使用sql语句查询数据表
sql = "SELECT * FROM employee"
# 异常处理
try:
    cursor.execute(sql)
    results = cursor.fetchone()    # 取游标处数据
    print(results)
except:
    print('Uable to fetch data!')
# 关闭数据库连接
db.close()
```

运行结果如图：

![image-20200530214846986](C:\Users\86133\AppData\Roaming\Typora\typora-user-images\image-20200530214846986.png)

使用fetchmany()：

```python
import pymysql

# 打开数据库连接
db = pymysql.connect('localhost', 'root', '123456', 'pythondb')
cursor = db.cursor()
sql = "SELECT * FROM employee"
try:
    cursor.execute(sql)
    results1 = cursor.fetchmany(2)   # 取多行数据
    for res in results1:
        print(res)
except:
    print('Uable to fetch data!')
db.close()
```

运行结果为：

![image-20200530220004735](C:\Users\86133\AppData\Roaming\Typora\typora-user-images\image-20200530220004735.png)

#### 6、数据库更新操作

```python
import pymysql

# 打开数据库连接
db = pymysql.connect('localhost', 'root', '123456', 'pythondb')
# 使用cursor()方法创建一个游标对象，游标对象用于执行查询和获取结果
cursor = db.cursor()
# 使用sql语句查询数据表
cursor.execute("select * from employee")
print('更新前的数据为：')
for res in cursor.fetchall():
    print (res)
sql = 'UPDATE employee SET age = age + 5 WHERE sex = "男"'
# 异常处理
try:
    cursor.execute(sql)
    # 执行SQL语句
    cursor.execute("select * from employee")
    print('更新后的数据为：')
    for res in cursor.fetchall():
        print (res)
    # 提交到数据库执行
    db.commit()
except:
    # 发生错误时回滚
    db.rollback()
db.close()
```

运行结果为：

![image-20200530220258692](C:\Users\86133\AppData\Roaming\Typora\typora-user-images\image-20200530220258692.png)

#### 7、数据库删除操作

```python
import pymysql

# 打开数据库连接
db = pymysql.connect('localhost', 'root', '123456', 'pythondb')
# 使用cursor()方法创建一个游标对象，游标对象用于执行查询和获取结果
cursor = db.cursor()
# 查询数据表
cursor.execute("select * from employee")
print('删除前的数据为：')
for res in cursor.fetchall():
      print (res)
sql = 'DELETE FROM employee WHERE income > 5000 AND age >35'
try:
    cursor.execute(sql)
    print('删除后的数据为：')
    cursor.execute("select * from employee")
    for res in cursor.fetchall():
        print (res)
    db.commit()
except:
     # 发生错误时回滚
    db.rollback()
db.close()
```

运行结果为：

![image-20200530220501990](C:\Users\86133\AppData\Roaming\Typora\typora-user-images\image-20200530220501990.png)

#### 8、事务回滚

```python
import pymysql

db=pymysql.connect('localhost','root','123456','pythondb')
cursor=db.cursor()
cursor.execute("select * from employee")
print('修改前的数据为：')
# 取全部数据
for res in cursor.fetchall():
      print (res)

print ('*'*40)
# 更新数据
cursor.execute("update employee set age=28 where age=23")
cursor.execute("select * from employee")
print('修改后的数据为：')
for res in cursor.fetchall():
      print (res)
print ('*'*40)
# 事务回滚
db.rollback()
cursor.execute("select * from employee")
print('回滚事务后的数据为：')
for res in cursor.fetchall():
      print (res)

cursor.close()
db.commit()
db.close()
```

结果如图：

![image-20200530220751126](C:\Users\86133\AppData\Roaming\Typora\typora-user-images\image-20200530220751126.png)

可发现数据回滚到更新前的数据。

#### 9、数据库删除

```python
import pymysql

db = pymysql.connect('localhost',"root","123456","pythondb")
cursor=db.cursor()
# 删除数据表
cursor.execute('DROP TABLE employee')
# 删除数据库
cursor.execute('DROP DATABASE pythondb')
cursor.close()
db.close()
```

代码运行成功：则删除完毕数据库

在SQLyog中查看不到pythondb数据库

![image-20200530221134493](C:\Users\86133\AppData\Roaming\Typora\typora-user-images\image-20200530221134493.png)


查询数据
#############

数据库表
--------

在传统的关系数据库中，特定的数据模型将存储在一个数据表中, 数据可能跨表存储
和交互, 后面我们会讲到joins. 现在我们来看看当前数据库中有那些表,
在Postgre中我们可以通过命令 `\\d`:

.. code-block:: sql

   craig=# \d
            List of relations
    Schema |   Name    | Type  | Owner 
   --------+-----------+-------+-------
    public | products  | table | craig
    public | purchases | table | craig
    public | users     | table | craig
   (3 rows)

查询数据
-------------

首先列出一些数据, 在这里我们从users开始, 这是构建查询的一部分
(知道你的数据从哪里来). 下一部分是审查有那些数据可以查询. 
我们再次执行 `\\d` 命令, 这次我们在后面追加一个表名 `\\d users` :

.. code-block:: sql

   craig=# \d users
                    Table "public.users"
      Column   |            Type             | Modifiers 
   ------------+-----------------------------+-----------
    id         | integer                     | 
    first_name | character varying(50)       | 
    last_name  | character varying(50)       | 
    email      | character varying(255)      | 
    created_at | timestamp without time zone | 
    updated_at | timestamp without time zone | 
    last_login | timestamp without time zone | 

在这里我们可以看到有各种数据, 首先我们只想要查询用户的first_name,
last_name, 和 email这么几个字段信息, 现在我们知道想要的数据通过
下列信息可以构造查询:

* 我们想要查询的数据库表
* 我们想从数据表中查询哪些数据

下面是执行此操作的语法, 它包含你要查询的字段数据, 从哪里查询, 
以分号结束查询语句:

.. code-block:: sql

   craig=# SELECT first_name, last_name, email 
   craig-# FROM users;
      first_name | last_name |           email           
   ------------+-----------+---------------------------
    Craig      | Kerstiens | craig.kerstiens@gmail.com
   (1 row)

这是很棒的的我们想要读取数据库表中所有的数据, 但是想读取任何大小
的数据集是不可行的. 这种情况下我们可能要限制返回的数据量来做过滤.
嗯, 我们来开始学习过滤.


过滤数据
--------------

要过滤数据我们可以使用一个过滤器组合, 通过 `WHERE` 条件来描述一个过滤器,
例如我们想要查询数据库表中2012年以后创建的所有用户的email：

.. code-block:: sql

   SELECT email, created_at
   FROM users
   WHERE created_at >= '2012-01-01'

我们可以使用 `AND` 或者 `OR` 结合其他过滤条件, 例如我们想要查询2012年1月
份创建的所有用户的:

.. code-block:: sql

   SELECT email, created_at
   FROM users
   WHERE created_at >= '2012-01-01'
   AND created_at < '2012-02-01'

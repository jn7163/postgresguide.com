Psql
####

什么是psql?
------------

`Psql` 是Postgres的交互终端,它有很多丰富的选项供你使用. 但是我们先关注一些重要的
参数，然后怎样连接:

- -h 要连接的主机
- -U 连接的用户
- -P 要连接的端口 (默认端口是 5432)

.. code-block:: console

   psql -h localhost -U username databasename

另一个选项是使用完整的字符串然后让psql去解析它:

.. code-block:: console

    psql "dbname=dbhere host=hosthere user=userhere password=pwhere port=5432 sslmode=require"

一旦你连接上了你就可以查询了， 除了基本的查询，你还可以使用其他命令, 执行 `\\?`
将为你列出所有看用的命令, 通过几个关键的命令可以展示下列技巧.

技巧
----

列出当前数据库中的数据库表
~~~~~~~~~~~~~~~~~~~~~~~~~~~

.. code-block:: console

   # \d
         List of relations
    Schema |   Name    | Type  | Owner 
   --------+-----------+-------+-------
    public | employees | table | craig
   (1 row)


描述表
~~~~~~~~~~~~~~~~

.. code-block:: console

   # \d employees 
              Table "public.employees"
     Column   |         Type          | Modifiers 
   -----------+-----------------------+-----------
    id        | integer               | 
    last_name | character varying(50) | 
    salary    | integer               | 
   Indexes:
       "idx_emps" btree (salary)

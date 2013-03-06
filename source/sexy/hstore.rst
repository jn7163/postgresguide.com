HStore数据类型
==================

这是什么?
----------

HStore 一种Postgres内置的键值对结构的数据类型, 你可以使用类似于其他编程语言中的字典结构保存在一条记录中的某一列中.

开启HStore
---------------

在你的数据库上执行下面的命令来启用HStore:

.. code-block:: sql

   CREATE EXTENSION hstore;


创建一个HStore的字段
-------------------------

在创建表是只要指定这个字段的数据类型为hstore就可以了:

.. code-block:: sql

   CREATE TABLE products (
     id serial PRIMARY KEY,
     name varchar,
     attributes hstore
   );

插入数据
--------------

把字段的值用单引号包含一来， 不同的就是要知道创建字典的一些额外的结构:

.. code-block:: sql

  INSERT INTO products (name, attributes) VALUES (
   'Geek Love: A Novel',
   'author    => "Katherine Dunn",
    pages     => 368,
    category  => fiction'
   );

读取数据
---------------

.. code-block:: sql

   SELECT name, attributes->'device' as device 
   FROM products 
   WHERE attributes->'edition'= 'ebook'

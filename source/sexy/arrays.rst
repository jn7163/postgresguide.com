数组(Arrays)
================

Postgres 允许把字段定义为可变长度的数组. 数据的类型可以是内置的类型, 用户自定义的类型或者枚举类型.

在创建表时声明数组字段:

.. code-block:: sql

   CREATE TABLE rock_band
   (
      name text,
      members text[]
   )

上面的语句将创建一张 `rock_band` 表, 它定义了一个text类型的字段 `name` 来表示乐队的名称, 还有一个 `members` 字段以二维数组的类型来保存队员的名字.

插入数组值
----------------------

.. code-block:: sql

   INSERT INTO rock_band
   VALUES
   ('Led Zeppelin',
   '{"Page", "Plant", "Jones", "Bonham"}'
   )

注意： 数组元素值是用双引号引起来的, 如果是单引号就会出错的.

查询出来将是这样的:

.. code-block:: sql

    postgres=# select * from rock_band;
	 	name 	|      	members     	 
	--------------+---------------------------
	 Led Zeppelin | {Page,Plant,Jones,Bonham}
	(1 row)

另一种方法是插入的时候使用数组的构造器:

.. code-block:: sql

    INSERT INTO rock_band
	VALUES
	('Pink Floyd',
	ARRAY['Barrett', 'Gilmour']
	)

当使用数组构造器的时候, 数组元素是用单引号.

.. code-block:: sql

    postgres=# select * from rock_band;
	 	name 	|      	members     	 
	--------------+---------------------------
	 Led Zeppelin | {Page,Plant,Jones,Bonham}
	 Pink Floyd   | {Barrett,Gilmour}
	(2 rows)

访问数组类型
----------------

数组类型的值可以通过下标和切片的方式访问:

.. code-block:: sql

  postgres=# select name from rock_band where members[2] = 'Plant';
      name	 
  --------------
   Led Zeppelin
  (1 row)

  postgres=# select members[1:2] from rock_band;
    	members 	 
  -------------------
   {Page,Plant}
   {Barrett,Gilmour}
  (2 rows)

修改数组值
----------------

数组字段可以更新某个数组元素或者整个值:

更新单个元素:

.. code-block:: sql

   postgres=# UPDATE rock_band set members[2] = 'Waters' where name = 'Pink Floyd';
   UPDATE 1
   postgres=# select * from rock_band where name = 'Pink Floyd';
      	name	| 	members 	 
   ------------+------------------
    Pink Floyd | {Barrett,Waters}
   (1 row)

更新整个字段值:

.. code-block:: sql

   postgres=# UPDATE rock_band set members = '{"Mason", "Wright", "Gilmour"}' where name = 'Pink Floyd';
   UPDATE 1 
   postgres=# select * from rock_band where name = 'Pink Floyd';	
   name        |    	members    	 
   ------------+------------------------
    Pink Floyd | {Mason,Wright,Gilmour}
   (1 row)

在数组中搜索
-------------------

要在数组中查找某个特定元素值, 可以使用ANY关键词.

.. code-block:: sql

   postgres=# select name from rock_band where 'Mason' = ANY(members);
       name    
   ------------
    Pink Floyd
   (1 row)

   postgres=# select name from rock_band where 'Barrett' = ANY(members);
    name
   ------
   (0 rows)

要查找数组中所有值都匹配某个值, 可以使用ALL.

.. note::
    Article contributed by
    `Chandrakant Gopalan <http://cgopalan.tumblr.com>`_.

  

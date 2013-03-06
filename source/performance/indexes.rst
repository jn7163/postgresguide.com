索引 (Indexes)
#######

什么是索引?
----------------

索引是一个特定的结构组织了一个参考使得你的数据更容易查找. 在Postgres中这是把你想建立索引的值的一个参考的组合起来的一个副本.
当访问数据的时候, Postgres将更容易的通过索引（如果存在）或者使用序列扫描. 序列扫描会在返回数据把所有数据搜一遍.

优缺点
---------------

索引非常适合需要更快的访问你的数据. 大多数情况下添加索引到某个字段将加快查询, 然后缺点是你插入数据就会变慢.
基本上当你写入一个有索引的数据是它需要写入两个地方以及对索引的排序. 此外某些类型的字段建立索引更有效, 比如
数字和时间(text的成本就比较高的).


实例
---------------

让我们来给数据库表上的字段建立索引:

.. image:: http://f.cl.ly/items/2I0a2u3z1x1Q0h2t3f1M/Untitled%202.png
   :height: 300

.. code-block:: sql

   CREATE INDEX idx_salary ON employees(salary);

你可以一次在一个或者多个字段上建立索引. 如果你通常对多个列过滤, 那么你可以对这两个列索引:

.. code-block:: sql

   CREATE INDEX idx_salary ON employees(last_name, salary);

技巧
-----------

并发方式建立索引
~~~~~~~~~~~~~~~~~~~~~~~~~

当Postgres建立你的索引的时候， 和其他数据库一样, 在建立索引的时候是会锁表的.
对于小数据量来说没什么关系， 但是通常可能是我们对一个大数据量的表加索引,
这意味着要获得性能改进应用必须收到停机一段时间. 至少那一张表会受影响.
Postgres有能力在创建索引的时候不锁表, 通过使用 `CREATE INDEX CONCURRENTLY` ,
例如:

.. code-block:: sql

   CREATE INDEX CONCURRENTLY idx_salary ON employees(last_name, salary);


当你的索引比你聪明的时候
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
在所有索引没有被Postgres使用的情况, 大多数情况下你应该相信Postgres, 例如当你查询的结果占所有数据的大部分时候, 
它可能不使用索引，因为只扫描全表一次最简单,而不是使用索引做额外的查找.

计数(Count)
~~~~~~~~~~~~~~~

另一个情况是Postgres中通过需要扫描来计数count(*)的成本比较高. 没有别的办法来来对行数计数并返回结果除了扫描全部数据.

Foreign Keys and Indexes
~~~~~~~~~~~~~~~~~~~~~~~~

有些ORM中当你创建一个外键的时候也会给你建个索引, 说明Postgres在创建外键的时候不会为你建立索引, 如果不用ORM的话它的步骤是分开的.

进阶
---------------

如果你要进一步了解,在这里推荐一本很棒的书《 `PostgreSQL 9.0 High Performance <http://www.amazon.com/gp/product/184951030X/ref=as_li_qf_sp_asin_tl?ie=UTF8&tag=mypred-20&linkCode=as2&camp=1789&creative=9325&creativeASIN=184951030X>`_ 》,
Greg Smith, 是这本书的作者也是我的一个朋友, 他对Postgres性能调优很有研究, 关于Postgres性能没有更多的参考文献.

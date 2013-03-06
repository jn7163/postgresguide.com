执行计划 (Execution Plan)
===========================

什么执行计划?
-------------------------

Postgres 有一个强大的功能可以想你展示数据库内容怎样执行一个查询. 这里所谓的 `执行计划` 是
通过 **explain** 展示出来的, 了解这个将让你知道怎样通过索引来优化数据库的性能, 对于大多数
人来说困难难点就是读懂这些输出, 虽然大部分开发者了解其中的一些关键点.

关于解释 (Explain)
-------------------

每一个查询执行的时候都有一个执行计划, 有三种形式通过 **explain** 来输出这些信息:

- 常规形式 (只显示可能发生了什么)
- Analyze  (分析实际查询中会发生什么)
- Verbose  (显示完整的内部执行计划树, 适用与高级用户)

大多数情况下， **explain** 是用来分析 `SELECT` 语句, 但是你可以用在:

- INSERT
- UPDATE
- DELETE
- EXECUTE
- DECLARE

使用Explain
-------------

例如这个查询: 

.. code-block:: sql
   
   SELECT last_name FROM employees where salary >= 50000;

我们可以查看Postgres将要怎么执行:

.. code-block:: sql
   
   EXPLAIN SELECT last_name FROM employees where salary >= 50000;
                             QUERY PLAN                          
   --------------------------------------------------------------
    Seq Scan on employees  (cost=0.00..16.50 rows=173 width=118)
      Filter: (salary >= 50000)

我们同时可以查看执行路径和执行时间：

.. code-block:: sql
   
   EXPLAIN ANALYZE SELECT last_name FROM employees where salary >= 50000;
                                               QUERY PLAN                                               
   --------------------------------------------------------------------------------------------------------
    Seq Scan on employees  (cost=0.00..16.50 rows=173 width=118) (actual time=0.018..0.018 rows=0 loops=1)
      Filter: (salary >= 50000)
    Total runtime: 0.053 ms


理解执行计划
-----------------------------

也许执行计划最难的就是理解它们的意思. 首先这是一个简短的描述, 不是一个完整的参考, 但是我们把它作为理解查询和优化数据库的出发点.

如果你在有200万行数据的数据表上执行 `EXPLAIN ANALYZE` 分析, 那么你将看到如下输出:

.. image:: http://cl.ly/1f3o2w3x1a41402B2g1R/1.%20psql-1.png
   :width: 650

但是我们来看一下到底是什么意思

.. image:: http://f.cl.ly/items/2F1A2T0a3h1v1d2u213O/1.%20psql-2.png
   :width: 650

这里有两组数值, 通常你所查看出现的序列扫描, 但是更重要的是上面这三个数值是什么意思, 启动花了多少时间、最长时间和返回的行数.
在这种情况下, 因为我们跑的是解释分析(EXPLAIN ANALYZE), 我们不仅要看第一组的估算数值，但是时间运行的数值在第二组.

.. image:: http://cl.ly/3i1x2D3R3w3D1I0R1h3W/1.%20psql-4.png
   :width: 650

在这里我们看到在序列扫描上花了很多的时间, 因此我们可以添加索引来试试看:

.. code-block:: sql

   CREATE INDEX idx_emps on employees (salary);

通过这样做我们的查询时间从295毫秒减少到了1.7毫秒

.. image:: http://cl.ly/1j1B0w2X2k0c281M2K3E/1.%20psql-10.png
   :width: 650

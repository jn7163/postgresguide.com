日期和时间
============================

一个常见的活动与任何数据库或者编程语言都要同时间和日期打交道. Postgres有几个时间和日期数据类型, 可以很好的简化你的工作.

数据类型
---------

- Date - 日期 (2012-04-25)
- Time - 时间 (13:00:00.00)
- Timestamp - 日期和时间 (2012-04-25 13:00:00.00)
- Time with Timezone - 时间加时区 (13:00:00.00 PST)
- Timestamp with Timezone (2012-04-25 13:00:00.00 PST)
- Interval - 时间段 (4 days)

.. note::
    请记住时间段这个函数 `interval` , 它是一个很有用的工具用来做一些针对特定时间范围的查询

输入格式
-------------

很幸运的Postgres可以接受多种格式的时间和日期, 官方文档中很详细的描述了各种格式所以我们这里将跳过它们: `Date input format <http://www.postgresql.org/docs/9.1/static/datatype-datetime.html#DATATYPE-DATETIME-DATE-TABLE>`_,  `Time input format <http://www.postgresql.org/docs/9.1/static/datatype-datetime.html#DATATYPE-DATETIME-TIME-TABLE>`_, 和 `Timezone input format <http://www.postgresql.org/docs/9.1/static/datatype-datetime.html#DATATYPE-TIMEZONE-TABLE>`_.

技巧
----

截取时间戳(Truncating timestamps)
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

很多时候我们想要查询某些数据并用日期的形式分组查询. Postgres提供了一个 `date_trunc` 函数来很方便的解决这个问题, 我们通过一个例子来展示找出每天注册了多少个用户:

.. code-block:: sql

   SELECT count(*), date_trunc('day', created_at)
   FROM users
   GROUP BY 2
   ORDER BY 2 DESC;

你可以在文档中找到更多关于截取的使用方法
`Postgres Documentation <http://www.postgresql.org/docs/8.1/static/functions-datetime.html#FUNCTIONS-DATETIME-TRUNC>`_.


时间段(Intervals)
~~~~~~~~~~~~~~~~~~~~

如下面的例子, 时间段(interval)是一个用来查询两个时间范围的实用工具, 类似于上面的例子中，
你可能想要查询过去24小时内注册的用户数:

.. code-block:: sql

   SELECT count(*)
   FROM users
   WHERE created_at >= (now() - '1 day'::INTERVAL);

另一种语法的时间段使用方法:

.. code-block:: sql

   SELECT count(*)
   FROM users
   WHERE created_at >= (now() - interval '1 month');

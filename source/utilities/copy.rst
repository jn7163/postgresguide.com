复制
######

什么是复制?
------------

Postgres 附带了几个强大的实用工具用来移动你的数据. 明显的pg_dump 和 pg_restore
分别是用来对数据库备份和还原. 一个类似的实用工具我们谈的比较少但是同样很有价值
它就是Postgres的copy工具. 复制允许你使用 `copy` 命令复制数据进出数据库.它支持
的模式有:

  - 二进制
  - tab 分割
  - csv 分割

无论是批量数据加载测试，做一些轻量级的数据转换，或者是提取一些数据发给某个人, 
每个开发人员都在利用它的一些点.

实践
--------------

提取员工表的所有字段数据到制表符分割的文件中:

.. code-block:: sql

   \copy (SELECT * FROM employees) TO '~/employees.tsv';

提取员工表的所有字段数据到csv格式文件中:

.. code-block:: sql

   \copy (SELECT * FROM employees) TO '~/employees.csv' WITH (FORMAT CSV);


提取员工表的所有字段数据到一个二进制文件(注意Binary前后的引号):

.. code-block:: sql

   \copy (SELECT * FROM employees) TO '~/employees.dat' WITH (FORMAT "Binary");

同样的加载数据到数据库表中:

.. code-block:: sql

   \copy employees FROM '~/employees.tsv';
   \copy employees FROM '~/employees.csv' WITH CSV;
   \copy employees FROM '~/employees.dat' WITH BINARY;

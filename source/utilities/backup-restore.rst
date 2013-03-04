备份与还原
##################

这是什么？
----------

希望读此文章的人清楚什么是关于你的数据库的备份与还原. 但是，如果你完全是一个
新手，备份就是你数据库的完整的Schema和数据, 还原就是通过备份数据还原到你的
数据库中或者其他数据库中.

注: 备份和还原是针对整个数据库或者整个表, 而不是为了提取数据, 这种情况下你应该是副本.

备份
------

`pg_dump <http://www.postgresql.org/docs/8.4/static/app-pgdump.html>`_ 是一个
用来备份的实用工具, 它提供了很多选项当你备份的时候可以用到.它们包括:

- 纯文本格式(可读、文件大) vs. 二进制格式(不可读和很小) vs. 压缩包(理想的还原)
- 所有你的数据库或者特定schemas或表数据

让我们实用一些备份 - 如果你忘了数据库名你可以进入psql把它们列出来:

.. code-block:: sql

    psql -l

然后执行备份:

.. code-block:: sql

    pg_dump database_name_here > database.sql

以上操作将会创建一个纯文本的数据库备份文件. 要创建一个更适合存储的备份可以
通过以下其中一个命令:

.. code-block:: sql

    pg_dump -Fc database_name_here > database.bak # compressed binary format
    pg_dump -Ft database_name_here > database.tar # compressed tarball


还原
-------

在还原的时候， 有一些因素要考虑一下:

- 数据库是否已经存在
- 备份文件的格式

.. code-block:: sql

    pg_restore -Fc database.bak # restore compressed binary format
    pg_restore -Ft database.tar # restore tarball

如果你的数据库已经存在你只需要执行以上命令就可以了，但是，如果你要在还原中新建
数据库你得增加一个参数执行类似于下面的命令:

.. code-block:: sql

    pg_restore -Fc -C database.bak # restore compressed binary format
    pg_restore -Ft -C database.tar # restore tarball


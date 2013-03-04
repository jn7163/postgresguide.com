用户管理
#########

新建用户
------------

一旦你安装好了Postgres, 你就可以通过 `psql -h localhost` 连接到你的数据库中开
始工作, 当然首先要做的应该为自己创建一个帐户.


.. code-block:: sql

    craig=# CREATE USER craig WITH PASSWORD 'Password';
    CREATE ROLE

新用户 `craig` 被创建密码为 `Password`.

下一步我们创建一个数据库并且授权给用户 `craig`

.. code-block:: sql

    craig=# CREATE DATABASE pgguide;
    CREATE DATABASE

现在一个新的数据库 `pgguide` 被创建好了. 现在我们将授权给 `craig`

.. code-block:: sql

    craig=# GRANT ALL PRIVILEGES ON DATABASE pgguide to craig;
    GRANT

现在 `craig` 在 `pgguide` 这个数据库上拥有所有的权限. 这些权限种类是: SELECT,
INSERT, UPDATE, DELETE, RULE, REFERENCES, TRIGGER, CREATE, TEMPORARY,
EXECUTE, and USAGE.

.. code-block:: sql

    craig=# GRANT SELECT ON DATABASE pgguide to craig;
    GRANT

`GRANT SELECT` 表示只允许 `craig` 在数据库 `pgguide` 上做 `select` 查询.


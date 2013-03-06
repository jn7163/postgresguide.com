条件约束(Conditional Constraints)
===================================

有时候你可能想要对数据进行约束, 但只希望他们在某些情况下存在. 让我们来看个例子, 你不想物理的删除用户， 而且想做逻辑删除, 如果用户几个月后又回来了那么
通过 `局部索引(Partial Indexes) <http://www.postgresql.org/docs/9.1/static/indexes-partial.html>`_ 来解决这个问题，只对非删除的用户做唯一索引.

.. code-block:: sql

    CREATE UNIQUE INDEX user_email ON users (id) WHERE deleted_at IS NULL;


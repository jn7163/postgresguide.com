Joins
#####

什么是Joins?
--------------

Joins 是当你要查询的数据分别在两张不同的表中, 组合的方法取决于使用不用的
Join类型,有很多种方法可以连接数据，我们将逐个使用一个最初的例子来完成和使用Join.

所有的这些例子都将基于我们这个schema例子

.. code-block:: sql

   craig=# \d
            List of relations
    Schema |   Name    | Type  | Owner 
   --------+-----------+-------+-------
    public | products  | table | craig
    public | purchases | table | craig
    public | users     | table | craig
   (3 rows)


连接(Joing)一些数据
-------------------

让我们以找到最近购买的商品为例开始，为此显然需要的数据从我们的产品表和购买记录
表中查询, 我们来看看这两张表都有什么字段：

.. code-block:: sql

   \d products
                Table "public.products"
      Column    |          Type          | Modifiers 
   -------------+------------------------+-----------
    id          | integer                | 
    title       | character varying(255) | 
    description | text                   | 
    price       | numeric(10,2)          | 

.. code-block:: sql

   	\d purchases 
	     Table "public.purchases"
	   Column   |  Type   | Modifiers 
	------------+---------+-----------
	 id         | integer | 
	 user_id    | integer | 
	 product_id | integer | 
	 quantity   | integer |

当两个表通过某个key建立了连接. 后续我们将会介绍更多, 现在最重要的是我们可以看到
购买记录表中的 `product_id` 是为了和产品表的 `id` 建立关联关系. 与此我们可以构造
我们的查询语句比如查询5个购买.

.. code-block:: sql

   SELECT 
      products.title, 
      purchases.quantity
   FROM 
      products,
      purchases
   WHERE
      products.id = purchases.product_id
   LIMIT 5;

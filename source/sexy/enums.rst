枚举类型(Enum)
==================

Postgres提供了一个枚举类型用来保存一个枚举值, 比如你需要有一个字段只保存 'Email'、'SMS' and 'Phone', 你可以在第一次的时候定义它.

.. code-block:: sql

   CREATE TYPE e_contact_method AS ENUM ('Email', 'Sms', 'Phone')

然后创建表的时候就可以使用这个类型

.. code-block:: sql

   CREATE TABLE contact_method_info (
      contact_name text,
      contact_method e_contact_method,
      value text
   )

使用枚举类型
-------------

.. code-block:: sql

   INSERT INTO contact_method_info
   VALUES ('Jeff', 'Email', 'jeff@mail.com')

   # select * from contact_method_info;
   contact_name | contact_method |     value     
   --------------+----------------+---------------
   Jeff         | Email          | jeff@mail.com
   (1 row)

你不能插入一个 `e_contact_method` 不包含的值到字段 `contact_method` 中

.. code-block:: sql

   # INSERT INTO contact_method_info VALUES ('Jeff', 'Fax', '4563456');
   ERROR:  invalid input value for enum e_contact_method: "Fax"
   LINE 1: INSERT INTO contact_method_info VALUES ('Jeff', 'Fax', '4563...

查看和修改枚举类型的值
------------------------------

你可以把枚举类型中的值都列出来:

.. code-block:: sql

   # select t.typname, e.enumlabel from pg_type t, pg_enum e 
     where t.oid = e.enumtypid;
       typname      | enumlabel
     ------------------+-----------
   e_contact_method | Email
   e_contact_method | Sms
   e_contact_method | Phone
   (3 rows)


你也可以追加枚举值到某个已经存在的枚举中:

.. code-block:: sql

   ALTER TYPE e_contact_method
    ADD VALUE 'Facebook' AFTER 'Phone';

   # select t.typname, e.enumlabel from pg_type t, pg_enum e 
     where t.oid = e.enumtypid;
       typname      | enumlabel
   ------------------+-----------
   e_contact_method | Email
   e_contact_method | Sms
   e_contact_method | Phone
   e_contact_method | Facebook
   (4 rows)

枚举值可以在任意位置插入因为它是顺序排列的

.. code-block:: sql

   ALTER TYPE e_contact_method
    ADD VALUE 'Twitter' BEFORE 'Sms';

   # select t.typname, e.enumlabel, e.enumsortorder from pg_type t, pg_enum e 
     where t.oid = e.enumtypid order by e.enumsortorder;
       typname      | enumlabel | enumsortorder
   ------------------+-----------+---------------
   e_contact_method | Email     |             1
   e_contact_method | Twitter   |           1.5
   e_contact_method | Sms       |             2
   e_contact_method | Phone     |             3
   e_contact_method | Facebook  |             4
   (5 rows)

到目前为止，Postgres没有提供从枚举中删除值的方法. 

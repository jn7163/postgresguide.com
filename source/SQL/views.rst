视图(Views)
###############

什么是视图？
--------------

根据 `维基百科 <http://en.wikipedia.org/wiki/View_%28database%29>`_
"一个视图包含的查询可以作为关系数据库中的虚拟表或者是文件数据库中一个查询
结果或者Map-and-Reduce组成的一个文件集的"

简单来说，视图是简单的自动连接逻辑表的基础数据块. 它实际上并不重复或存在作为
查看逻辑窗体中的数据.

为什么要使用视图
----------------

视图可用于许多情况. 视图可以简化你的数据模型当你提供给他人使用的时候,同时也
可以简化你自己对数据的使用. 如果你发现自己经常连接两个数据集以类似的方式
重复多次那么使用视图可以缓解.

当和其他对SQL不熟悉的人一起工作时使用视图是提供非标准数据的一个好方法.


实践
----------------

让我们看看这个例子中的表:

.. image:: http://f.cl.ly/items/072Q3Y073Z0o413b3N2x/Untitled%202-1.png
   :height: 220

.. image:: http://f.cl.ly/items/2Q470O2S2f2v1u091r3h/Untitled%202-2.png
   :height: 300

.. image:: http://f.cl.ly/items/2I0a2u3z1x1Q0h2t3f1M/Untitled%202.png
   :height: 300

要获取员工和他们的部门信息往往需要编写一个看上去类似与这样的查询:

.. code-block:: sql

   SELECT 
     employees.last_name, 
     employees.salary, 
     departments.department
   FROM 
     employees, 
     employee_departments,
     departments
   WHERE 
     employees.id = employee_departments.employee_id
     AND departments.id = employee_departments.department_id

在这里其实不太复杂, 虽然每次它不会特别麻烦当你要获取这些信息的时候, 我们可以
创建一个视图来大大的简化并将为你自动连接这些数据.

.. code-block:: sql
   
   CREATE OR REPLACE VIEW employee_view AS
   SELECT 
     employees.last_name, 
     employees.salary, 
     departments.department
   FROM 
     employees, 
     employee_departments,
     departments
   WHERE 
     employees.id = employee_departments.employee_id
     AND departments.id = employee_departments.department_id

现在你可以简单的直接查询一个新的数据库表:

.. code-block:: sql

   SELECT *
   FROM employee_view

并且它会产生和上面查询一样的的数据:

.. code-block:: sql

   last_name    salary   department
   Jones        45000    Accounting 
   Adams        50000    Sales
   Johnson      40000    Marketing
   Williams     37000    Accounting
   Smith        55000    Sales

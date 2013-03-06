Postgres 性能
====================

一旦你部署好了你的网站和数据库架构, 并获取用户数据, 你应该积极的去关注数据,
但也应该积极的去优化数据库性能, 开发者都应该可以掌握数据库优化, 就像掌握code一样
以至于把应用做得更好, 下面的内容主要是从分析你的数据库性能开始并进行改进,
如果你要进一步了解,在这里推荐一本很棒的书《 `PostgreSQL 9.0 High Performance <http://www.amazon.com/gp/product/184951030X/ref=as_li_qf_sp_asin_tl?ie=UTF8&tag=mypred-20&linkCode=as2&camp=1789&creative=9325&creativeASIN=184951030X>`_ 》,
Greg Smith, 是这本书的作者也是我的一个朋友, 他对Postgres性能调优很有研究, 
关于Postgres性能没有更多的参考文献.

.. toctree::
   :maxdepth: 2

   indexes
   explain
   conditional

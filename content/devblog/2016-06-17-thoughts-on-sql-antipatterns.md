---
title: Thoughts on SQL Antipatterns
author: Ruslan Bes
type: post
date: 2016-06-17T10:30:44+00:00
url: /2016/06/17/thoughts-on-sql-antipatterns/
dsq_thread_id:
  - 4917544628
categories:
  - PHP
tags:
  - ActiveRecord
  - Model
  - SQL

---
Just watched the presentation about [SQL Antipatterns][1] and got one thought to share.

The presentation is good in general and I had encountered virtually all of the situations and in the end we came to the same solutions as the author recommends.

I have one disagreement about the  [last idea][2] with &#8220;Model has an ActiveRecord&#8221;. It has a very significant downside.

The power of web frameworks is that you can easily create CRUD actions. You only need to provide the name of the db table and define some rules over input values (validators). You get this power because Models there inherit ActiveRecord class (Yii framework) and the ActiveRecord itself has already fixed rules how to CRUD records through PDO.

If you change the Model to represent multiple db tables as in this presentation then you suddenly lose that ability and have to handcraft the CRUD logic yourself. So you lose all the power of the framework.

There is a nice solution to this problem but you need PostgreSQL or Oracle (maybe there is an analogue in MySQL, I&#8217;m not sure). These DBMS allow creating &#8220;editable views&#8221; which behave just like physical tables but you define the CRUD logic inside the database. So your app thinks it&#8217;s a plain table and can leverage the full power of MVC framework while business logic stays in the database where it should be.

The implementation is as simple as that:

  1. Create a view with the things you need
  2. Create triggers on UPDATE, INSERT and DELETE for that view that will handle the business logic

&nbsp;

 [1]: http://www.slideshare.net/billkarwin/sql-antipatterns-strike-back
 [2]: http://www.slideshare.net/billkarwin/sql-antipatterns-strike-back/239
---
title: Naming conventions in database schema
author: Ruslan Bes
type: post
date: 2020-10-05T18:39:10+00:00
url: /2020/10/05/naming-conventions-in-database-schema/
classic-editor-remember:
  - block-editor
categories:
  - Software architecture
tags:
  - SQL

---
In one of the tools I&#8217;m using in my work there is a database and the interesting thing about it is that the field names in all the tables end with an underscore: KEY\_, ID\_, NAME_ and so on. 

I was confused at the beginning but I think I know now why it was done like that. 

Thing is it&#8217;s an open-source tool and the original developers naturally wanted people to use it without any obstacle. 

So this naming convention is done to avoid naming conflicts with any popular DBMS and with SQL standard itself (At least KEY is a reserved word). Besides you can also construct SQL queries without bothering to enclose the table and column names in any kind of quotes and just use them as is: select KEY_ from &#8230; .

Although it looks like a workaround I consider this an example of a good architectural decision. It makes the whole system less dependent on the underlying software and also makes it more user-friendly.
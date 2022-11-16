---
title: 'ERROR 2006 (HY000): MySQL server has gone away'
author: Ruslan Bes
type: post
date: 2012-05-17T16:55:27+00:00
url: /2012/05/17/error-2006-hy000-mysql-server-has-gone-away/
dsq_thread_id:
  - 4854563885
categories:
  - Software engineering
tags:
  - ERROR 1153 (08S01)
  - ERROR 2006 (HY000)
  - mysqldump
  - Sypex Dumper

---
Today while creating local copy of one of my WordPress sites I got a following message when extracting the Mysql dump:  
`ERROR 2006 (HY000) at line 1086: MySQL server has gone away`  
Hmmmmm&#8230;  
Well this first dump was created by <a title="Sypex Dumper" href="http://sypex.net/" target="_blank" rel="noopener noreferrer">Sypex Dumper</a> so I tried getting another dump from mysqldump and that one went well. Being curious of what happened with the first dump I played a little bit with the dumpfile and got another message:  
`ERROR 1153 (08S01) at line 1086: Got a packet bigger than 'max_allowed_packet' bytes`  
This is much more interesting. The <a title="packet too large" href="http://dev.mysql.com/doc/refman/5.5/en/packet-too-large.html" target="_blank" rel="noopener noreferrer">packet too large</a> error happens when you send a packet to the server that is larger than the size of `max_allowed_packet` directive in my.ini. By default it&#8217;s only **1 Megabyte** so if you have long sql-queries in your dump (for instance if you have TEXT fields in the database) you&#8217;ll probably get this error. Ok, changing the server value have helped, but now we are getting that first mystic error again. What&#8217;s wrong?  
The thing is that besides the server directive there is also the max\_allowed\_packet limit on the client side and it&#8217;s 16M for mysql client by default. So we have to change our restore call to something like:  
`shell> mysql --max_allowed_packet=200M db_name < path_to_dump.sql`  
and one again ensure that my.ini section has the same or bigger value:  
`[mysqld]<br />
max_allowed_packet=200M`  
Now after mysqld restart everything will work fine.
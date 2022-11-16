---
title: yes command in Linux
author: Ruslan Bes
type: post
date: 2018-05-29T20:11:40+00:00
url: /2018/05/29/yes-command-in-linux/
dsq_thread_id:
  - 6699311940
categories:
  - 'Tips &amp; tricks'
tags:
  - yes

---
There is an interesting command in Linux:

`yes y`

It generates an infinite stream of letters &#8216;y&#8217; and presses enter after each of it  
This way you can install multiple packages in one line:

`yes y | yum install sudo mc nano`

I didn&#8217;t know it and got used to install every package separately.

By a strange coincidence at this very moment I&#8217;m reading a chapter of &#8220;Effective Java&#8221; about Infinite Streams.

P.S. another way to achieve this is like this:

`yum install -y sudo mc nano`
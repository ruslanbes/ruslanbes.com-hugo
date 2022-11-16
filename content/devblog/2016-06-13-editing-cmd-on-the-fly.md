---
title: Editing cmd on the fly
author: Ruslan Bes
type: post
date: 2016-06-13T09:29:54+00:00
url: /2016/06/13/editing-cmd-on-the-fly/
dsq_thread_id:
  - 4914304461
categories:
  - 'Tips &amp; tricks'

---
A piece of advice: if you started running a Windows cmd script and it&#8217;s currently in progress, and you got an awesome idea and decided to edit the cmd-file, then don&#8217;t save that file.

Apparently when Windows executes cmd-file it doesn&#8217;t put it into memory and just saves the pointer to the next line to execute. So when you edit it, that pointer starts pointing to some strange place.

This is kinda obvious idea but after working with PHP for some many years I get used to its behavior and that I can safely edit files even when they are executed (normally during debugging) since they were compiled into RAM and executed from there. I think Python behaves same way as PHP.
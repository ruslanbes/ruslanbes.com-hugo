---
title: HyperLogLog
author: Ruslan Bes
type: post
date: 2018-09-16T14:01:06+00:00
url: /2018/09/16/hyperloglog/
dsq_thread_id:
  - 6914970203
categories:
  - Software engineering
tags:
  - algorithm

---
During [Netconomy Summit][1] I learned about an interesting algorithm called [HyperLogLog][2]. The idea is like this — suppose you have an array of a billion strings. Some of them are unique, some are duplicates. The problem is to figure out the number of unique strings.  


The algorithm goes like this: for each new string we calculate a pseudorandom hash and look at its binary form. We look **how many leading zeroes** the hash had. And save a maximum number we have seen so far.  


In the end, we can simply calculate 2-power-<maximum number> and that is our approximation.

Of course, such an approximation can be way off so here is part two of the algorithm: instead of **one hash** we calculate a **1000 different hashes** and average out the result.  


By tweaking the parameters we can get a reasonable result.

Now why we would need that at all? Suppose you have a very popular site with millions of visits and what you&#8217;re interested in is how many unique users have it.

 [1]: https://www.linkedin.com/feed/update/urn:li:activity:6446325014756237312
 [2]: https://en.wikipedia.org/wiki/HyperLogLog
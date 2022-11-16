---
title: xrange in Python
author: Ruslan Bes
type: post
date: 2015-04-30T15:43:46+00:00
url: /2015/04/30/xrange-in-python/
dsq_thread_id:
  - 5102908738
categories:
  - Python
tags:
  - integer
  - xrange

---
The Qualification round in **Google code jam** taught me a new thing about python.

Imagine following program for getting asleep:

<pre class="brush: python; title: ; notranslate" title=""># asleep.py

import sys, time
sheep = int(sys.argv[1:].pop())
print "Counting sheep up to", sheep

for i in xrange(sheep):
	time.sleep(1)
	if i &gt; 4: break
	print i+1,
print "Fell asleep"
</pre>

and you run it:

<pre class="brush: bash; gutter: false; title: ; notranslate" title="">$ python asleep.py 1111111
Counting sheep up to 1111111
1 2 3 4 5 Fell asleep</pre>

It looks fine because `xrange` does not create the whole list `[0, 1, ..., 1111110]` but instead works as &#8220;generator&#8221; that requests the next value on demand. 

But what happens if we raise the sheep count?

<pre class="brush: bash; gutter: false; title: ; notranslate" title="">$ python asleep.py 11111111111
Counting sheep up to 11111111111
Traceback (most recent call last):
  File "asleep.py", line 7, in &lt;module&gt;
    for i in xrange(sheep):
OverflowError: Python int too large to convert to C long</pre>

That&#8217;s interesting. It turns out that when you run the line

<pre class="brush: python; gutter: false; title: ; notranslate" title="">###
sheep = int(sys.argv[1:].pop())
###
</pre>

the type of `sheep` variable may vary:

<pre class="brush: python; gutter: false; title: ; notranslate" title="">###
sheep = int(sys.argv[1:].pop())
print sheep.__class__
###
</pre>

<pre class="brush: bash; gutter: false; title: ; notranslate" title="">$ python asleep.py 1111111
&lt;type 'int'&gt;</pre>

<pre class="brush: bash; gutter: false; title: ; notranslate" title="">$ python asleep.py 11111111111
&lt;type 'long'&gt;</pre>

Basically, Python checks the input value and if it&#8217;s inside the range `[-sys.maxint - 1, sys.maxint]` it returns `int`, otherwise converts it to `long`. Same thing happens if you run a code like:

<pre class="brush: python; title: ; notranslate" title="">a = sys.maxint
print a.__class__ # &lt;type 'int'&gt;
a += 1
print a.__class__ # &lt;type 'long'&gt;
</pre>

So simply use a while loop and you&#8217;re safe:

<pre class="brush: python; title: ; notranslate" title="">import sys, time
sheep = int(sys.argv[1:].pop())
print "Counting sheep up to", sheep


i = 0
while i &lt; sheep:
    time.sleep(1)
    if i &gt; 4: break
    print i+1,
    i+=1
print "Fell asleep"
</pre>
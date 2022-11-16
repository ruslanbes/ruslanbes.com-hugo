---
title: Find indexes of duplicate items in python
author: Ruslan Bes
type: post
date: 2014-04-25T16:14:55+00:00
url: /2014/04/25/find-indexes-of-duplicate-items-in-python/
dsq_thread_id:
  - 4832890939
categories:
  - Python
tags:
  - algorithm
  - duplicates
  - list

---
Pretty common question when dealing with lists (arrays, vectors or whatever is used in you favorite programming language) is to find duplicates and list the as a hashmap.  
Example (Python). From the list like this

<pre class="brush: python; title: ; notranslate" title="">lst = [5,5,4,4,4,3]</pre>

get a dict

<pre class="brush: python; title: ; notranslate" title="">dupls = {5 =&gt; [0,1], 4 =&gt; [2,3,4]} </pre>

The question was raised on the SO [here][1] and got some standard solutions which basically do the job with O(n) time.

I wanted to suggest another solution, which is slower — O(n*log(n)) but it allows generating an interesting structure.  
The solution for original problem is this:

<pre class="brush: python; title: ; notranslate" title="">def dupl_rbespal(c):
    alreadyAdded = False
    dupl_c = dict()
    sorted_ind_c = sorted(range(len(c)), key=lambda x: c[x]) # sort incoming list but save the indexes of sorted items

    for i in xrange(len(c) - 1): # loop over indexes of sorted items
        if c[sorted_ind_c[i]] == c[sorted_ind_c[i+1]]: # if two consecutive indexes point to the same value, add it to the duplicates
            if not alreadyAdded:
                dupl_c[ c[sorted_ind_c[i]] ] = [sorted_ind_c[i], sorted_ind_c[i+1]]
                alreadyAdded = True
            else:
                dupl_c[ c[sorted_ind_c[i]] ].append( sorted_ind_c[i+1] )
        else:
            alreadyAdded = False
    return dupl_c</pre>

Now what I want to get is a following structure:

From the list like this

<pre class="brush: python; title: ; notranslate" title="">lst = [5,5,4,4,4,3]</pre>

get a dict

<pre class="brush: python; title: ; notranslate" title="">dupls = {0 =&gt; 1, 2 =&gt; 3, 3 =&gt; 4 } </pre>

That is I want to get a dict where key points to the next index with the same value.  
With a slight change of the above algorithm we get:

<pre class="brush: python; title: ; notranslate" title="">def nextDuplicates(c):
    dupl_c = dict()
    sorted_ind_c = sorted(range(len(c)), key=lambda x: c[x])
    for i in xrange(len(c) - 1):
        if c[sorted_ind_c[i]] == c[sorted_ind_c[i+1]]:
            dupl_c[ sorted_ind_c[i] ] = sorted_ind_c[i+1]
    return dupl_c</pre>

So it&#8217;s even shorter than the original one.

 [1]: http://stackoverflow.com/questions/5419204/index-of-duplicates-items-in-a-python-list/23294343#23294343
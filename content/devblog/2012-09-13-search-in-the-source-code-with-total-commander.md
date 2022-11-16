---
title: Search in the source code with Total commander
author: Ruslan Bes
type: post
date: 2012-09-13T07:31:28+00:00
url: /2012/09/13/search-in-the-source-code-with-total-commander/
dsq_thread_id:
  - 4833537626
classic-editor-remember:
  - classic-editor
categories:
  - 'Tips &amp; tricks'
tags:
  - git
  - LinkedIn
  - SVN
  - Total Commander

---
You can use Total commander to search the text in the project files. Very often it will be faster than the one from your IDE. Besides you can &#8220;Feed to listbox&#8221; the results and do the search inside them.

<p style="text-align: left;">
  Most likely you&#8217;ll want to <strong>skip the .svn or .git</strong> <strong>folders</strong> with your search. That is easy to do.<br /> In Total Commander go to your project directory, hit Alt+F7 and put in the &#8220;Search for&#8221; field:<strong> *.* | .git\ .svn\<br /> </strong>Then you use the &#8220;Find text&#8221; field.
</p>
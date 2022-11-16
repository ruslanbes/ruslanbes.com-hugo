---
title: indexOf implementation for WScript and CScript
author: Ruslan Bes
type: post
date: 2013-04-24T16:39:27+00:00
url: /2013/04/24/indexof-implementation-for-wscript-and-cscript/
dsq_thread_id:
  - 4869552974
categories:
  - Software engineering
tags:
  - CScript
  - indexOf
  - WScript

---
One of the problems with <a href="http://en.wikipedia.org/wiki/Windows_Script_Host" target="_blank" rel="noopener noreferrer">WScript</a> is that it implements not the normal JavaScript we all use in web but rather JScript, which is actually JavaScript 1.5. One of the function which do not exist in version 1.5 is Array.indexOf

Here is a patch that allows using it:

<pre class="brush: jscript; title: ; notranslate" title="">if ( typeof(Array.prototype.indexOf) === 'undefined' ) {
    Array.prototype.indexOf = function(item, start) {        
        var length = this.length
        start = typeof(start) !== 'undefined' ? start : 0
        for (var i = start; i &lt; length; i++) {
            if (this[i] === item) return i
        }
        return -1
    }
}</pre>
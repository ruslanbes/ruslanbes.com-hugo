---
title: Danger in using switch
author: Ruslan Bes
type: post
date: 2013-03-12T13:48:08+00:00
url: /2013/03/12/danger-in-using-switch/
categories:
  - PHP
  - Software engineering
tags:
  - if-else
  - switch

---
One more caveat in using **switch**-statement is when you tend to use it like **if**-statement. Here is an example:

<pre class="brush: php; title: ; notranslate" title="">switch ($president) {
  case "Putin" || "Medvedev":
    $next_president = ($president == "Putin") ? "Medvedev" : "Putin";
  break;
  case "Stalin":
    $next_president = "Stalin";
    break;
}</pre>

Now. If you run this code you&#8217;ll get following results:  
1. $president = &#8221;; // $next_president = NULL  
2. $president = &#8216;Obama&#8217;; // $next_president = &#8220;Putin&#8221;  
3. $president = &#8216;Stalin&#8217;; // $next_president = &#8220;Putin&#8221;

What happened?

Here is what compiler does to this code.  
Line3: `"Putin" || "Medvedev"` is converted to `TRUE || TRUE` and then shortened to `TRUE`. So we get:

&nbsp;

<pre class="brush: php; title: ; notranslate" title="">switch ($president) {
  case TRUE:
    $next_president = ($president == "Putin") ? "Medvedev" : "Putin";
  break;
  case "Stalin":
    $next_president = "Stalin";
    break;
}</pre>

Now whenever string `$president` is compared with boolean `TRUE` it is also converted to boolean. So any non-empty string will match the first case statement and the line 5 will never be reached.  
The correct way to use **or**-s is:

<pre class="brush: php; title: ; notranslate" title="">switch ($president) {
  case "Putin": 
  case "Medvedev": 
    $next_president = ($president == "Putin") ? "Medvedev" : "Putin";
  break;
  case "Stalin":
    $next_president = "Stalin";
    break;
}</pre>
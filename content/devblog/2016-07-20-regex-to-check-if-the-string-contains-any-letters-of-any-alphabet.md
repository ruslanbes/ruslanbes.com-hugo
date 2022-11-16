---
title: Regex to check if the string contains any letters of any alphabet
author: Ruslan Bes
type: post
date: 2016-07-20T10:18:28+00:00
url: /2016/07/20/regex-to-check-if-the-string-contains-any-letters-of-any-alphabet/
dsq_thread_id:
  - 5019122559
categories:
  - PHP
tags:
  - regex
  - unicode

---
There is a trivial task to check if the string contains any letters:

<pre class="brush: php; title: ; notranslate" title="">$contains_letters = preg_match('/[A-Za-z]+/', $str);</pre>

But what if the `$str` is a multibyte string and may contain umlauts or Cyrillic letters? That&#8217;s what I had recently and I nearly resorted to the ugly solution like this:

<pre class="brush: php; title: ; notranslate" title="">$really_contains_letters = (mb_strtoupper($str) == $str && mb_strtolower($str) == $str);</pre>

Thankfully it&#8217;s not so bad, PHP supports analyzing [unicode characters][1] using regex, so the simple:

<pre class="brush: php; title: ; notranslate" title="">$really_contains_letters = preg_match('/\pL/', $str);</pre>

would do the trick. And I expect it would be faster since it can stop before reading the whole string.

 [1]: http://php.net/manual/en/regexp.reference.unicode.php
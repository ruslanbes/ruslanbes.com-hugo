---
title: How to merge paths in PHP properly
author: Ruslan Bes
type: post
date: 2016-07-19T22:11:28+00:00
url: /2016/07/20/how-to-properly-merge-paths-in-php/
dsq_thread_id:
  - 5096746280
categories:
  - PHP
  - 'Tips &amp; tricks'
tags:
  - paths
  - slashes

---
_Updated on 20.07.2016 with a small fix._

Let&#8217;s talk about PHP again.

Suppose you store the base directory path to your PHP app in the `$basedir` var, then you have uploads directory relative to it and configured in `$uploads` var and inside that dir every user has its own dir named after his `$user_id` and finally you know that user has uploaded a picture inside it, the relative path to it is &#8220;/my-pictures/me.jpg&#8221;. 

How do you create an absolute path to that image? The PHP itself allows only one way of concatenating strings:

<pre class="brush: php; title: ; notranslate" title="">$full_path = $basedir . '/' . $uploads . '/' . $user_id . '/' . '/my-pictures/me.jpg';</pre>

Will that work? The true answer is: **most of the time**.  
If you are using this technique there is a high chance you get a link like this in the end: [https://ruslanbes.com/demos/basedir/my-uploads/666//</strong>my-pictures/me.jpg][1]  
See this double slash thingy? Luckily Apache webserver will understand that you meant one slash and replace it for you, but if you have specific rewrite rules in .htaccess or in you php app this might be a problem and you&#8217;ll get 404 error. Another thing happens when you rely on slashes inside your vars and end up with the link like <https://ruslanbes.com/demos/basedirmy-uploads/666/my-pictures/me.jpg>. All these things are quite annoying, so in my projects I usually follow two simple rules.

Rule 1: All paths should be stored without trailing slashes. If we allow user to enter the path somewhere, we trim the trailing slash immediately before saving that into the database. 

Rule 2: It is generally recommended to merge paths using special function instead of concatenation. Here it is:

<pre class="brush: php; auto-links: false; title: ; notranslate" title="">&lt;?php

/**
 * Merge several parts of URL or filesystem path in one path
 * Examples:
 *  echo merge_paths('stackoverflow.com', 'questions');           // 'stackoverflow.com/questions' (slash added between parts)
 *  echo merge_paths('usr/bin/', '/perl/');                       // 'usr/bin/perl/' (double slashes are removed)
 *  echo merge_paths('en.wikipedia.org/', '/wiki', ' Sega_32X');  // 'en.wikipedia.org/wiki/Sega_32X' (accidental space fixed)
 *  echo merge_paths('etc/apache/', '', '/php');                  // 'etc/apache/php' (empty path element is removed)
 *  echo merge_paths('/', '/webapp/api');                         // '/webapp/api' slash is preserved at the beginnnig
 *  echo merge_paths('http://google.com', '/', '/');              // 'http://google.com/' slash is preserved at the end
 * @param string $path1
 * @param string $path2
 */
function merge_paths($path1, $path2){
    $paths = func_get_args();
    $last_key = func_num_args() - 1;
    array_walk($paths, function(&$val, $key) use ($last_key) {
        switch ($key) {
            case 0:
                $val = rtrim($val, '/ ');
                break;
            case $last_key:
                $val = ltrim($val, '/ ');
                break;
            default:
                $val = trim($val, '/ ');
                break;
        }
    });

    $first = array_shift($paths);
    $last = array_pop($paths);
    $paths = array_filter($paths); // clean empty elements to prevent double slashes
    array_unshift($paths, $first);
    $paths[] = $last;
    return implode('/', $paths);
}
</pre>

You can test it on [eval.in][2]. You may safely include it in your project and put to some common library. I hope something like that will be included into PHP core in future as the problem is very common.

 [1]: https://ruslanbes.com/demos/basedir/my-uploads/666//my-pictures/me.jpg
 [2]: https://eval.in/608533
---
title: isset or not isset
author: Ruslan Bes
type: post
date: 2014-09-18T13:54:07+00:00
url: /2014/09/18/isset-or-not-isset/
dsq_thread_id:
  - 5159083126
categories:
  - PHP
tags:
  - isset

---
If you work with PHP for a long time, you probably get used to the code like this:

<pre class="brush: php; title: ; notranslate" title="">&lt;?php 
if (isset($_POST['username']) && isset($_POST['password']) {
	$this-&gt;login($_POST['username'], $_POST['password'])
}</pre>

the magic of [isset][1] is that it is short and explicitly tells what it does — checks if the $var has some &#8220;meaningful&#8221; value.

By using it zillion times, you might get used to it as a quick check if the key exists in array as in the above example. But here is the caveat. Suppose you have a hash-array containing user profile fields like &#8220;Name&#8221;, &#8220;Website&#8221;, and blablabla and at the end &#8211; &#8220;About me&#8221;. So you would like to print it like this:

<pre>Name: Ford Prefect
Website: http://h2g2.com
...
-------
Something about me: Always have a towel</pre>

What you could come up for this is something like that:

<pre class="brush: php; title: ; notranslate" title="">&lt;?php
if (isset($profile['About me'])) {
	$aboutMe = $profile['About me'];
	unset($profile['About me']);
}

foreach ($profile as $field =&gt; $value) {
	echo $field . ': ' . $value . "\n";
}
echo "-------\n";
if (isset($aboutMe)) {
	echo "Something about me: " . $aboutMe;
}</pre>

Will it work?

Well, not exactly. Think of what happens if &#8220;About me&#8221; is NULL. 

[This][2] is what happens:

<pre>Name: Ford Prefect
Website: http://h2g2.com
About me: 
-------</pre>

So the &#8220;About me&#8221; suddenly jumps from under the line over it. Why did it happen? I thought we explicitly removed the &#8220;About me&#8221; from array. 

The problem is in the very first line. In reality `if (isset($profile['About me']))` returns `false` not only if the key does not exist in array, but also in the case where it exists but its value is NULL. So the correct way would be to use [array\_key\_exists][3] function and then: [problem is solved][4].

Now this example was pretty naive but I recently found same error in [Array2XML][5] convertor and there it produced exceptions because of not deleted tags. So be careful.

 [1]: http://php.net/manual/en/function.isset.php
 [2]: https://eval.in/195024
 [3]: http://at1.php.net/manual/en/function.array-key-exists.php
 [4]: https://eval.in/195026
 [5]: http://www.lalit.org/lab/convert-php-array-to-xml-with-attributes/
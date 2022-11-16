---
title: Another reason to use Vagrant for PHP development
author: Ruslan Bes
type: post
date: 2014-08-19T13:26:32+00:00
url: /2014/08/19/another-reason-to-use-vagrant-for-php-development/
categories:
  - PHP
tags:
  - Bug
  - DateTime::diff
  - Vagrant

---
I always insist people are using the same or maximally similar environment as the production server has, that is the same db version, PHP version and OS. Here is another reason why you should do that and **NOT** use XAMPP for serious PHP development.

In PHP 5 there is a nifty new function that allows easy computing of the interval between two dates. Not surprisingly it is called `<span class="html">DateTime::diff</span>`. The description of this function is accompanied by following example:

<pre class="brush: php; title: ; notranslate" title="">&lt;?php
$datetime1 = new DateTime('2009-10-11');
$datetime2 = new DateTime('2009-10-13');
$interval = $datetime1-&gt;diff($datetime2);
echo $interval-&gt;format('%R%a days'); // +2 days
?&gt;
</pre>

The only problem is that the result will be `+6015 days` on **some** Windows PCs. It so happens that this function has a [bug on Windows platforms][1] which is marked as _won&#8217;t fix_ because it is related to the Visual C compiler. And so the solution for this bug is simply to use VC9. This is something one of my junior developers got today and decided to workaround it by writing his own date-diff function. Of course if someone else who uses Linux platform for development used the normal diff function it would have raised one of those _worksforme_ errors which are really annoying to solve.

The moral of the story — if you are a lead-developer then spend some hours and create a fixed [Vagrant][2] virtual environment for all your developers and force them to use it. Or use any other ways to achieve the same result.

 [1]: https://bugs.php.net/bug.php?id=51184
 [2]: https://www.vagrantup.com/
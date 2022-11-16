---
title: Why reverse condition check is not practical
author: Ruslan Bes
type: post
date: 2016-04-30T10:18:32+00:00
url: /2016/04/30/why-reverse-condition-check-is-not-practical/
dsq_thread_id:
  - 4832506070
categories:
  - PHP
tags:
  - if-else

---
Small note about &#8220;reversed&#8221; condition checks like these:

<pre class="brush: php; title: ; notranslate" title="">if (CONSTANT == $variable) {
  // do something
}
</pre>

and why their benefit is so small compared to the side-effects.

It&#8217;s stated that these checks are better because they prevent you from doing an obvious mistake:

<pre class="brush: php; title: ; notranslate" title="">if ($variable = CONSTANT) {
  // oops, assigment
}
</pre>

Here comes the side-effects though. Suppose you have a code like this:

<pre class="brush: php; title: ; notranslate" title="">if ('admin' == $username) {
  // allow some admin actions
}
</pre>

It looks okay, you prevented the &#8220;accidental assignment&#8221; error until one day you decide to refactor this into:

<pre class="brush: php; title: ; notranslate" title="">if ('admin' == $user-&gt;getUsername()) {
  // allow some admin actions
}
</pre>

Now should you leave it reversed or not? The danger of assignment is gone since `$user->getUsername()` is not an lvalue. Suppose you leave it like this just to make all assignments similar. But then you do another refactoring and end up with:

<pre class="brush: php; title: ; notranslate" title="">if (User::getAdminUsername() == $user-&gt;getUsername()) {
  // allow some admin actions
}
</pre>

NOW what? At what point it becomes so silly that it won&#8217;t bring any value to your code? It&#8217;s always **more** readable when you write `if ($a == 'b')` since that is how you mind works, it first evaluates something you don&#8217;t know and then tries to match it with something you do know. And if you are so obsessed with assignment error, just write a simple SVN/Git hook that blocks the commit if that happens.

Sometimes you see people using `if ($variable = CONSTANT)` intentionally to save the space combining two actions into one. While that **is** possible it&#8217;s also possible someone else from your team finds this code while debugging, considers it a source of a bug and &#8220;fixes&#8221; it. Besides, &#8220;someone else&#8221; might be you in a year. So if you write that kind of code, please add a comment right next to it.
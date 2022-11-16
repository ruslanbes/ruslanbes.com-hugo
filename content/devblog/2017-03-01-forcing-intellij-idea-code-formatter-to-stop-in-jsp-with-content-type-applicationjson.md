---
title: Forcing IntelliJ IDEA Code Formatter to stop in JSP with content-type “application/json”
author: Ruslan Bes
type: post
date: 2017-02-28T23:03:37+00:00
url: /2017/03/01/forcing-intellij-idea-code-formatter-to-stop-in-jsp-with-content-type-applicationjson/
dsq_thread_id:
  - 5592633862
categories:
  - Java
tags:
  - IntelliJ IDEA
  - JSON
  - JSP

---
I recently opened a [ticket][1] on the IntelliJ IDEA tracker listing a bug in a very specific situation.

Suppose you have a JSP that contains JSON data. Suppose also that a part of that data is a formatted HTML code and you don&#8217;t want auto-formatter to touch it. It doesn&#8217;t really matter what&#8217;s inside but you want that part of your code remain the same.

In the simplest form you could have something like that:

<pre class="brush: java; title: ; notranslate" title="">&lt;%@ page contentType="application/json" %&gt;

&lt;%--@formatter:off--%&gt;
{
"please":     "don't",
"format":   "this",
"code":         "!!!1111"
}
</pre>

Notice that I used `@formatter:off` control structure. But despite that being used, IntelliJ IDEA will still format your code. This happens by the way only for the `contentType="application/json"`, the normal JSPs are unaffected. The reason why that happens is listed I believe in the ticket I submitted.

There is however a trick to force the Formatter to work:

<pre class="brush: java; title: ; notranslate" title="">&lt;%@ page contentType="application/json" %&gt;
&lt;%@ taglib prefix="c" uri="http://java.sun.com/jsp/jstl/core" %&gt;
&lt;c:if test="false&gt;
// @formatter:off
&lt;/c:if&gt;
{
"please":     "don't",
"format":   "this",
"code":         "!!!1111"
}
</pre>

The first part of the solution is to use the normal Java comments to force the Code Formatter to stop. That works fine. But since we&#8217;re inside JSP, that means the comment will be sent to a user in plain text. We don&#8217;t want that so the second part will be to wrap it with 

<pre class="brush: java; title: ; notranslate" title="">&lt;c:if test="false&gt;
&lt;/c:if&gt;
</pre>

tags.

The affected IntelliJ IDEA version is 2016.3.4 and I hope it will be fixed in future versions.

 [1]: https://youtrack.jetbrains.com/issue/IDEA-168627
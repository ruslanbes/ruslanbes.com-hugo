---
title: Git partial clone (how to clone a single git directory)
author: Ruslan Bes
type: post
date: 2020-02-19T21:52:35+00:00
url: /2020/02/19/git-partial-clone-how-to-clone-a-single-git-directory/
classic-editor-remember:
  - block-editor
categories:
  - 'Tips &amp; tricks'

---
It turns out in git you can clone only a part of repository, which is very useful if you&#8217;re working on huge git repos and only need a couple of folders (e.g. `docs`)

The feature is called **sparse checkout** and is available since 2012. [This Stackoverflow answer][1] gives a pretty good overview. My post is just a summary from it.

The solution from the SO answer is like this — suppose you have a 50G repo and you only need a small need directory `some/dir` to clone and work locally. Here is what you do (With git 1.7.0+)

<pre class="wp-block-code"><code>$ mkdir &lt;repo>
$ cd &lt;repo>
$ git init
$ git remote add -f origin &lt;url>
$ git config core.sparseCheckout true
$ echo "some/dir/" >> .git/info/sparse-checkout
$ cat .git/info/sparse-checkout
some/dir
$ git pull origin master</code></pre>

Since git 2.25.0 (Jan 2020) you can do it even more intuitively with [git sparse-checkout][2]:

<pre class="wp-block-code"><code>$ mkdir &lt;repo>
$ cd &lt;repo>
$ git init
$ git remote add -f origin &lt;url>
$ git sparse-checkout init
$ git sparse-checkout set "some/dir/"
$ git sparse-checkout list
some/dir
$ git pull origin master</code></pre>

One thing to note here is that the syntax of `.git/info/sparse-checkout` is the same as of `.gitignore` so you can negate selections and use wildcards.

Another thing to note is that you can save even more space and still even be able to push the changes.

Instead of doing

<pre class="wp-block-code"><code>$ git remote add -f origin </code></pre>

Which fetches the whole git tree and all the history you can do a shallow clone and get only the latest version.

<pre class="wp-block-code"><code>$ mkdir &lt;repo>
$ cd &lt;repo>
$ git init
$ git remote add origin &lt;url>
$ git sparse-checkout init
$ git sparse-checkout set "some/dir/"
$ git sparse-checkout list
some/dir
$ git pull --depth=1 origin master</code></pre>

But why would you need all this stuff? 

Imagine you have a CI job that downloads your big repository on every push and does something that needs only a part of it (regenerates the sphinx documentation) and then cleans up after itself.

 [1]: https://stackoverflow.com/questions/600079/how-do-i-clone-a-subdirectory-only-of-a-git-repository/52269934#52269934
 [2]: https://www.git-scm.com/docs/git-sparse-checkout
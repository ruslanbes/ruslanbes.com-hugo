---
title: Remove Debugbreak() calls before committing files to SVN
author: Ruslan Bes
type: post
date: 2013-04-24T22:26:02+00:00
url: /2013/04/25/remove-debugbreak-calls-before-committing-files-to-svn/
dsq_thread_id:
  - 5194863423
categories:
  - PHP
tags:
  - DBG
  - Debugbreak
  - PhpEd
  - SVN
  - TortoiseSVN

---
Since I work mostly on Windows, I develop all my PHP projects in NuSphere PhpEd IDE. And I&#8217;m using TortoiseSVN for version control.

The problem is that sometimes after I debugged files locally with `debugbreak()` calls I forget to remove them before committing to preview and production repositories and so Fatal Errors there usually pisses off our QAs and PMs.

I devised a small WSH Script and set it as a pre-commit hook so that all files are cleaned from `debugbreak()` right before they are committed.

Just in case someone else suffers from this problem I put it on GitHub along with install instructions. Enjoy!  
<https://github.com/ruslanbes/remove-debugbreaks>
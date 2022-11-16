---
title: Mass compile less files into css
author: Ruslan Bes
type: post
date: 2015-12-16T16:42:16+00:00
url: /2015/12/16/mass-compile-less-files-into-css/
dsq_thread_id:
  - 5381815983
categories:
  - Web development
tags:
  - css
  - less
  - linux

---
Suppose you are using [less][1] and want to add the build step that does compile less into css over the whole project. 

Here are the steps:

1. Install [lessc][2]

2. Install [compresser][3] if you need it

Now create following shell scripts and add them into your build process:

3. `compile-one.sh` compiles single less file (change `--clean-css` options to what you need)

<pre class="brush: bash; title: ; notranslate" title="">#!/bin/sh
if [ $1 ]; 
  then 
    _basename=$(basename $1 .less)
    _dirname=$(dirname $1) 
  else
    echo "Usage: sh compile-one.sh &lt;full_path_to_less&gt;"
    echo "Example: sh compile-one.sh path/to/styles.less"
    exit
fi 

echo "Compiling "$1
lessc $1 $_dirname"/"$_basename".css" --clean-css="--s1 --advanced"
</pre>

4. `find-all-and-compile.sh` compiles all less files in `project/styles/dir` and its descendants. styles.less -> styles.css

<pre class="brush: bash; title: ; notranslate" title="">#!/bin/sh
find project/styles/dir -name *.less -exec sh compile-one.sh {} \;
</pre>

 [1]: http://lesscss.org/ "Less"
 [2]: http://lesscss.org/usage/#command-line-usage
 [3]: https://github.com/less/less-plugin-clean-css
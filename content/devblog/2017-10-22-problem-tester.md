---
title: Problem tester for coding contests
author: Ruslan Bes
type: post
date: 2017-10-22T20:49:07+00:00
url: /2017/10/22/problem-tester/
dsq_thread_id:
  - 6233783572
categories:
  - Python
tags:
  - contest
  - problem-tester

---
Rewritten my [problem-tester][1] project from scratch in Python 3. Here are the improvements.

## Debugging

The problem with the old solution was that it ran shell-exec to generate the output. That meant debugging wasn&#8217;t possible.

Now you can debug your scripts with PyCharm thanks to a feature of Python 3.

With Python 3 it is possible to redirect STDIN and STOUT by doing

<pre class="brush: python; title: ; notranslate" title="">fh_in = open(f_in, 'r')
fh_out = open(f_out, 'w')

sys.stdin = fh_in
sys.stdout = fh_out
</pre>

We now need to include the problem file and run it in a loop, passing &#8220;.in&#8221; and &#8220;out&#8221; files. Which wasn&#8217;t that simple.

The first challenge was to import a python module dynamically, not knowing its name in advance. Thankfully there is [an answer on the SO][2] regarding that, I just needed to adjust it a little.

The next problem was to reload the module and run it freshly for every new input. The best solution I found was to delete it from `sys.modules` and import again. I&#8217;m yet to check if it has any side-effects but so far it looks good.

<pre class="brush: python; title: ; notranslate" title="">def run_problem(mod):
    # remove module
    if mod in sys.modules:
        del sys.modules[mod]
    # dynamically import it again. 
    problem = __import__(mod, globals(), locals(), ['main'], 0)

    # if the module contains 'def main' execute it. 
    # If not, we assume the script was in global scope, importing it forced a run
    # Nothing to do
    if 'main' in dir(problem):
        problem.main()
</pre>

## Validating

Sometimes there may be several solutions to a problem, like here: <http://codeforces.com/problemset/problem/873/D>. In this case you can write a `validator.py` script that tests the solution.

## Comparing with another solution

If you have a solution that works, you can compare put it with the name `solver.py`. It will generate &#8220;ok.out&#8221; files.

Link: <https://github.com/ruslanbes/problem-tester>

 [1]: https://github.com/ruslanbes/problem-tester
 [2]: https://stackoverflow.com/questions/301134/dynamic-module-import-in-python/301146#301146
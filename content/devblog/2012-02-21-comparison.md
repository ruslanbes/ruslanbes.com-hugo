---
title: Variable comparison
author: Ruslan Bes
type: post
date: 2012-02-21T14:11:07+00:00
url: /2012/02/21/comparison/
categories:
  - PHP
tags:
  - code style

---
From time to time I bounce into a code like:

`if ('vodka' == drink.type && 40 == drink.spiritAmount) then {...}`

Some people consider it&#8217;s a good programming style and that it should supposedly prevent the noobies&#8217; error with accidental variable assignment. No it&#8217;s actually not. The good programming style is _when your code can be understood not only by a compiler, but by another human_. Humans, you know, build the sentences using the normal order: &#8220;If the bottle says &#8216;Vodka&#8217; but amount of spirit is not 40% then it&#8217;s a fake&#8221;. Okay? Find me someone who normally says something like &#8220;If the 161 is a train number that you&#8217;ve seen on the &#8216;arrived&#8217; screen then it&#8217;s time to board&#8221;.

Besides, if you use this reverse-style with the equality operator, you should probably use it with all other comparison operators, wouldn&#8217;t you? That is to write `if (10 <= number_of_entries)` and so on. So in order to fight one small lame error, which happens once in a year you&#8217;re gonna pervert human&#8217;s mind and ask smart people to think in a reversed way. I think it&#8217;s not worth it.

You wanna fight this error? Fine. Write a pre-commit hook that will execute regex-search and just stop the commit if it finds this bug. There is absolutely zero reasons for a single equation to appear inside the &#8220;if (&#8230;)&#8221; brackets. But please stop messing around with human&#8217;s mind. Programmers have better things to remember.

Of course if you join the project and there are already code conventions which claim this style, you must follow them no matter do you like them or not. Or if you build your application on top of an opensource framework that follows that style.

But when I create a new application, I personally choose not to use it.
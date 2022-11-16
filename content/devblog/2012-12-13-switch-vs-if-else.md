---
title: Switch vs if-else
author: Ruslan Bes
type: post
date: 2012-12-13T17:41:36+00:00
url: /2012/12/13/switch-vs-if-else/
dsq_thread_id:
  - 6837624339
categories:
  - PHP
tags:
  - code style
  - if-else
  - switch

---
Despite the fact that [PHP Manual suggests to use switch statement to match single variable][1] I would argue that switch is **in general** better for any if-else code that has at least three branches. That is, suppose that you have:

<pre class="brush: php; title: ; notranslate" title="">// this is a magical script that does all work for you
if (today_is_weekend()) {
    relax();
} elseif (today_is_friday() || tomorrow_is_a_holiday()) {
    work_half_time();
} else {
    work_full_time();
}</pre>

then you should start rewriting you code into switch because it will look much more simple and clean.

But you probably won&#8217;t. Because you are afraid of switch statements for their easy-to-forget `break`-s. Or because it&#8217;s an obsolete construction that only C++ nerds know of. Or just because you don&#8217;t know how to rewrite a code in such way that it uses switch.  
Let&#8217;s look first how you **can** do it:

<pre class="brush: php; title: ; notranslate" title="">// this is a magical script that does all work for you
switch (true) {
    case today_is_weekend():
        relax();
    break;
    case today_is_friday():
    case tomorrow_is_a_holiday():
        work_half_time();
    default:
        work_full_time();
}</pre>

Is it looking nicer?  
No? Why?  
Okay, okay it&#8217;s now 10 lines instead of 7. And it&#8217;s incorrect because I missed a `break;` after line 7. But let&#8217;s look what we got instead.

Notice that each condition is now on a separate line so we can easily reconstruct the logic of the script. They are also aligned and there are plenty of space to the right for inline comments should they be needed. 

Let&#8217;s now look at more complicated example. We now take into account vacations and holidays and illnesses. Also suppose, that we have two types of companies: in one company state holidays are automatic day-offs, whilst in the other one employees may either work half-time or take a vacation. Actually it&#8217;s the moment when someone might start creating classes and objects but let&#8217;s for a moment look how switch will solve it.

<pre class="brush: php; title: ; notranslate" title="">switch (true) {
    case today_is_weekend():
    case its_holiday() && $MyCompany-&gt;off_on_holidays():    
    case $Me-&gt;in_a_vacation():
    case $Me-&gt;is_ill():
        relax();
    break;

    case today_is_friday():
    case tomorrow_is_a_holiday():
    case its_holiday() && ! $MyCompany-&gt;off_on_holidays():    
        work_half_time();
    break;        
    
    default:
        work_full_time();
}
</pre>

We just added three lines and &#8220;Boom&#8221; &#8211; it&#8217;s done. If-Else code:

<pre class="brush: php; title: ; notranslate" title="">if (today_is_weekend() 
	|| its_holiday() && $MyCompany-&gt;off_on_holidays()
	|| $Me-&gt;in_a_vacation()
	|| $Me-&gt;is_ill()
   ) {
    relax();
} elseif (today_is_friday() 
	|| tomorrow_is_a_holiday()
	|| its_holiday() && ! $MyCompany-&gt;off_on_holidays()
   ) {
    work_half_time();
} else {
    work_full_time();
}
</pre>

Which one is now easier to read? My opinion is that switch code is more elegant and easier to extend if you want to add more conditions.

 [1]: http://php.net/manual/en/control-structures.switch.php
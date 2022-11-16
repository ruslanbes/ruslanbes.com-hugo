---
title: Intellij IDEA code hints do not work
author: Ruslan Bes
type: post
date: 2016-08-07T20:59:12+00:00
url: /2016/08/07/intellij-idea-code-hints-do-not-work/
dsq_thread_id:
  - 5047939058
categories:
  - Java
tags:
  - IDEA

---
I recently started practicing in Java and after playing around a little bit with SublimeText I decided to try out the full featured IDE and installed the latest Intellij IDEA. 

So I got it installed, created a new project, added JDK and pointed to my existent folder with java files like this:  
<img loading="lazy" src="https://i0.wp.com/ruslanbes.com/devblog/rbes-content/uploads/2016/08/java-study.png?resize=474%2C708&#038;ssl=1" alt="java-study" width="474" height="708" class="aligncenter size-full wp-image-774" srcset="https://i0.wp.com/devblog.ruslanbes.com/rbes-content/uploads/2016/08/java-study.png?w=474&ssl=1 474w, https://i0.wp.com/devblog.ruslanbes.com/rbes-content/uploads/2016/08/java-study.png?resize=201%2C300&ssl=1 201w" sizes="(max-width: 474px) 100vw, 474px" data-recalc-dims="1" /> 

I expected that IDEA will automatically give code hints for imports and function calls but it didn&#8217;t worked. Whenever I used `import` I got this:  
<img loading="lazy" src="https://i2.wp.com/ruslanbes.com/devblog/rbes-content/uploads/2016/08/import.png?resize=238%2C70&#038;ssl=1" alt="import" width="238" height="70" class="aligncenter size-full wp-image-775" data-recalc-dims="1" /> 

And whenever I tried invoking code suggestions I got this:  
<img loading="lazy" src="https://i2.wp.com/ruslanbes.com/devblog/rbes-content/uploads/2016/08/codehint.png?resize=502%2C53&#038;ssl=1" alt="codehint" width="502" height="53" class="aligncenter size-full wp-image-776" srcset="https://i1.wp.com/devblog.ruslanbes.com/rbes-content/uploads/2016/08/codehint.png?w=502&ssl=1 502w, https://i1.wp.com/devblog.ruslanbes.com/rbes-content/uploads/2016/08/codehint.png?resize=300%2C32&ssl=1 300w" sizes="(max-width: 502px) 100vw, 502px" data-recalc-dims="1" /> 

I couldn&#8217;t figure out what&#8217;s wrong but finally I noticed the &#8220;src&#8221; folder and tried to move my files there:  
<img loading="lazy" src="https://i0.wp.com/ruslanbes.com/devblog/rbes-content/uploads/2016/08/java-study2.png?resize=477%2C613&#038;ssl=1" alt="java-study2" width="477" height="613" class="aligncenter size-full wp-image-777" srcset="https://i1.wp.com/devblog.ruslanbes.com/rbes-content/uploads/2016/08/java-study2.png?w=477&ssl=1 477w, https://i1.wp.com/devblog.ruslanbes.com/rbes-content/uploads/2016/08/java-study2.png?resize=233%2C300&ssl=1 233w" sizes="(max-width: 477px) 100vw, 477px" data-recalc-dims="1" /> 

Now everything worked fine:  
<img loading="lazy" src="https://i2.wp.com/ruslanbes.com/devblog/rbes-content/uploads/2016/08/codehint2.png?resize=561%2C255&#038;ssl=1" alt="codehint2" width="561" height="255" class="aligncenter size-full wp-image-778" srcset="https://i2.wp.com/devblog.ruslanbes.com/rbes-content/uploads/2016/08/codehint2.png?w=561&ssl=1 561w, https://i2.wp.com/devblog.ruslanbes.com/rbes-content/uploads/2016/08/codehint2.png?resize=300%2C136&ssl=1 300w" sizes="(max-width: 561px) 100vw, 561px" data-recalc-dims="1" /> 

It turned out in Java world it&#8217;s common to separate the sources and the other types of files like resources, config-files and compiled files. If you press Ctrl+Alt+Shift+S you&#8217;ll get the project settings window where you can configure your custom sources folder but it defaults to &#8220;src&#8221; as you can see:  
<img loading="lazy" src="https://i0.wp.com/ruslanbes.com/devblog/rbes-content/uploads/2016/08/sources.png?resize=705%2C276&#038;ssl=1" alt="sources" width="705" height="276" class="aligncenter size-full wp-image-779" srcset="https://i2.wp.com/devblog.ruslanbes.com/rbes-content/uploads/2016/08/sources.png?w=1025&ssl=1 1025w, https://i2.wp.com/devblog.ruslanbes.com/rbes-content/uploads/2016/08/sources.png?resize=300%2C118&ssl=1 300w, https://i2.wp.com/devblog.ruslanbes.com/rbes-content/uploads/2016/08/sources.png?resize=768%2C301&ssl=1 768w, https://i2.wp.com/devblog.ruslanbes.com/rbes-content/uploads/2016/08/sources.png?resize=1024%2C402&ssl=1 1024w" sizes="(max-width: 705px) 100vw, 705px" data-recalc-dims="1" /> 

It was a a little bit unusual for me as my favorite PHP editor (Nusphere) by default parses the whole project folder and makes code-hints available from everywhere.
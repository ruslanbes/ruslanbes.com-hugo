---
title: Easy “Picture of the day” widget in WordPress
author: Ruslan Bes
type: post
date: 2012-09-05T09:05:15+00:00
url: /2012/09/05/easy-picture-of-the-day-widget-in-wordpress/
dsq_thread_id:
  - 4841478698
categories:
  - WordPress
tags:
  - Advanced excerpt
  - Category Posts Widget
  - Picture of the day

---
_**Note:** Unfortunately this method conflicts with &#8220;Advanced excerpt&#8221; plugin. Probably you would want to disable it. Or you may uncheck &#8220;Show post excerpt&#8221; in widget settings._

There is a very easy way to implement &#8220;picture of the day&#8221; widget in WordPress. It requires zero coding and besides it is based on the core functionality of the WordPress. So, no hacks.  
Here is a recipe for you.

  1. Install [Category Posts Widget][1] and activate it. It will show posts from a certain category in a sidebar.
  2. So now we have to create a category, let&#8217;s give it the title — &#8220;Image of the day&#8221;.
  3. We put the Category Posts widget to the sidebar and configure it to show the last post from &#8220;Image of the day&#8221; category. We disable excerpt but enable thumbnail
  4. That&#8217;s all. Create a post, set the format to &#8220;Image&#8221; if your theme allows to do so (makes the post to be looking nicely), then upload the image, insert it to the post and **set** **it as Featured Image**. Don&#8217;t forget to write custom excerpt if you checked the appropriate checkbox. That&#8217;s all.

 [1]: http://wordpress.org/extend/plugins/category-posts/
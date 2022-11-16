---
title: Orphaned types in hybris
author: Ruslan Bes
type: post
date: 2019-03-11T19:33:19+00:00
url: /2019/03/11/orphaned-types-in-hybris/
classic-editor-remember:
  - block-editor
categories:
  - SAP Commerce (hybris)
tags:
  - hybris-6

---
How often does the following happen: you work on some feature branch, create new properties or even new items, push your code, create a Pull Request and then in the meantime checkout a fresh `develop` branch and start working on something new? 

Now you have a database with items and properties that do not exist yet. If those are related to the CMS components, chances are your frontend simply stops working and start throwing strange Exceptions. 

Hybris can help you to solve this issue. Go to **hac** > **Maintenance** > **Cleanup** and use the **Type system** tab: <figure class="wp-block-image">

[<img loading="lazy" width="705" height="348" src="https://i2.wp.com/devblog.ruslanbes.com/rbes-content/uploads/2019/03/2017-11-29_15h55_30.png?resize=705%2C348&#038;ssl=1" alt="" class="wp-image-1313" srcset="https://i2.wp.com/devblog.ruslanbes.com/rbes-content/uploads/2019/03/2017-11-29_15h55_30.png?w=949&ssl=1 949w, https://i2.wp.com/devblog.ruslanbes.com/rbes-content/uploads/2019/03/2017-11-29_15h55_30.png?resize=300%2C148&ssl=1 300w, https://i2.wp.com/devblog.ruslanbes.com/rbes-content/uploads/2019/03/2017-11-29_15h55_30.png?resize=768%2C379&ssl=1 768w" sizes="(max-width: 705px) 100vw, 705px" data-recalc-dims="1" />][1]<figcaption>Orphaned types</figcaption></figure> 

Check it right now and you may be surprised to find that there is something in there. If there is something, consider running a cleanup in advance. Essentially hybris will drop the tables for those types.

A clean database looks like this: <figure class="wp-block-image">

[<img loading="lazy" width="705" height="356" src="https://i1.wp.com/devblog.ruslanbes.com/rbes-content/uploads/2019/03/2017-11-29_15h51_40.png?resize=705%2C356&#038;ssl=1" alt="" class="wp-image-1314" srcset="https://i1.wp.com/devblog.ruslanbes.com/rbes-content/uploads/2019/03/2017-11-29_15h51_40.png?w=972&ssl=1 972w, https://i1.wp.com/devblog.ruslanbes.com/rbes-content/uploads/2019/03/2017-11-29_15h51_40.png?resize=300%2C152&ssl=1 300w, https://i1.wp.com/devblog.ruslanbes.com/rbes-content/uploads/2019/03/2017-11-29_15h51_40.png?resize=768%2C388&ssl=1 768w" sizes="(max-width: 705px) 100vw, 705px" data-recalc-dims="1" />][2]<figcaption> Clean db. Notice the &#8220;There are no orphaned types&#8221; message </figcaption></figure>

 [1]: https://i2.wp.com/devblog.ruslanbes.com/rbes-content/uploads/2019/03/2017-11-29_15h55_30.png?ssl=1
 [2]: https://i1.wp.com/devblog.ruslanbes.com/rbes-content/uploads/2019/03/2017-11-29_15h51_40.png?ssl=1
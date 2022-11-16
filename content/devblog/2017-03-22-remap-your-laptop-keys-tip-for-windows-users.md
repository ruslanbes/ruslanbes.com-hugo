---
title: Remap your laptop keys (tip for Windows users)
author: Ruslan Bes
type: post
date: 2017-03-22T21:34:21+00:00
url: /2017/03/22/remap-your-laptop-keys-tip-for-windows-users/
dsq_thread_id:
  - 5656534872
categories:
  - 'Tips &amp; tricks'
tags:
  - apache-pro
  - dell-precision
  - keyboard
  - remap-keys
  - SharpKeys
  - Windows

---
If you have a PC laptop, chances are there are bunch of useless buttons hiding somewhere on the top-right corner of the keyboard.  
Something like this:

<img loading="lazy" src="https://i2.wp.com/ruslanbes.com/devblog/rbes-content/uploads/2017/03/2017-03-17-20.37.44-2.jpg?resize=500%2C315&#038;ssl=1" alt="Dell Precision 7510 multimedia keys" width="500" height="315" class="aligncenter size-full wp-image-843" srcset="https://i1.wp.com/devblog.ruslanbes.com/rbes-content/uploads/2017/03/2017-03-17-20.37.44-2.jpg?w=500&ssl=1 500w, https://i1.wp.com/devblog.ruslanbes.com/rbes-content/uploads/2017/03/2017-03-17-20.37.44-2.jpg?resize=300%2C189&ssl=1 300w" sizes="(max-width: 500px) 100vw, 500px" data-recalc-dims="1" />  
That&#8217;s the layout on the professional Dell Precision 7510 and here the multimedia buttons are just unnecessary.

Or they may be just confused  
<img loading="lazy" src="https://i0.wp.com/ruslanbes.com/devblog/rbes-content/uploads/2017/03/2017-03-22-20.51.47-2.jpg?resize=500%2C375&#038;ssl=1" alt="MSI GE70 2PE Apache Pro keys" width="500" height="375" class="aligncenter size-full wp-image-844" srcset="https://i0.wp.com/devblog.ruslanbes.com/rbes-content/uploads/2017/03/2017-03-22-20.51.47-2.jpg?w=500&ssl=1 500w, https://i0.wp.com/devblog.ruslanbes.com/rbes-content/uploads/2017/03/2017-03-22-20.51.47-2.jpg?resize=300%2C225&ssl=1 300w" sizes="(max-width: 500px) 100vw, 500px" data-recalc-dims="1" />  
(Layout on the MSI GE70 2PE Apache Pro)

You might think, that if you press the two rightmost buttons you get &#8220;Home&#8221; and &#8220;End&#8221; and if you press them holding &#8220;Fn&#8221; you get &#8220;PgUp&#8221;/&#8221;PgDn&#8221; but in reality it&#8217;s vice versa. And it&#8217;s kinda annoying. I need to move across the words much more often than I need to scroll the page.

To remap the keys on Windows we can use the [SharpKeys][1]. The UI is trivial &#8211; select the key you want to change and the target key (or just turn it off).  
<img loading="lazy" src="https://i2.wp.com/ruslanbes.com/devblog/rbes-content/uploads/2017/03/2017-03-22-22_21_42-SharpKeys_-Add-New-Key-Mapping.png?resize=561%2C461&#038;ssl=1" alt="SharpKeys - Add New Key Mapping" width="561" height="461" class="aligncenter size-full wp-image-845" srcset="https://i1.wp.com/devblog.ruslanbes.com/rbes-content/uploads/2017/03/2017-03-22-22_21_42-SharpKeys_-Add-New-Key-Mapping.png?w=561&ssl=1 561w, https://i1.wp.com/devblog.ruslanbes.com/rbes-content/uploads/2017/03/2017-03-22-22_21_42-SharpKeys_-Add-New-Key-Mapping.png?resize=300%2C247&ssl=1 300w" sizes="(max-width: 561px) 100vw, 561px" data-recalc-dims="1" />  
Hit &#8220;Write to Registry&#8221;, reboot your PC and you&#8217;re done. The new layout is now hardcoded into Windows, there is no need to launch any background apps.

Despite what author says on the site the app does allow you to swap keys. Here is my mapping for the MSI Apache Pro laptop:  
<img loading="lazy" src="https://i2.wp.com/ruslanbes.com/devblog/rbes-content/uploads/2017/03/SharpKeys-Settings.png?resize=705%2C521&#038;ssl=1" alt="SharpKeys Settings" width="705" height="521" class="aligncenter size-full wp-image-846" srcset="https://i0.wp.com/devblog.ruslanbes.com/rbes-content/uploads/2017/03/SharpKeys-Settings.png?w=738&ssl=1 738w, https://i0.wp.com/devblog.ruslanbes.com/rbes-content/uploads/2017/03/SharpKeys-Settings.png?resize=300%2C222&ssl=1 300w" sizes="(max-width: 705px) 100vw, 705px" data-recalc-dims="1" /> 

Just note that after some major Windows updates the keys may be reset to defaults, so keep the app just in case.

 [1]: https://sharpkeys.codeplex.com/
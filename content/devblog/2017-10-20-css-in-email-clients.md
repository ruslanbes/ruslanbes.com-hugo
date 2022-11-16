---
title: CSS in email clients
author: Ruslan Bes
type: post
date: 2017-10-20T19:49:10+00:00
url: /2017/10/20/css-in-email-clients/
dsq_thread_id:
  - 6229315255
categories:
  - Web development
tags:
  - css
  - Email
  - HTML
  - Outlook

---
When designing nice modern HTML emails keep in mind that the support for CSS in email clients is much more limited than in browsers. This is especially true in the Outlook for Windows. Even in the version 2016, it doesn&#8217;t support many common CSS styles.

I&#8217;ll show you a small example. This is an email from Amazon Developer. Let&#8217;s say we want to make the same header:

<img loading="lazy" src="https://i0.wp.com/ruslanbes.com/devblog/rbes-content/uploads/2017/10/amazn.png?resize=705%2C101&#038;ssl=1" alt="" width="705" height="101" class="aligncenter size-full wp-image-892" srcset="https://i1.wp.com/devblog.ruslanbes.com/rbes-content/uploads/2017/10/amazn.png?w=724&ssl=1 724w, https://i1.wp.com/devblog.ruslanbes.com/rbes-content/uploads/2017/10/amazn.png?resize=300%2C43&ssl=1 300w" sizes="(max-width: 705px) 100vw, 705px" data-recalc-dims="1" /> 

So we have a box with an image on the left, some text on the right, some border styling at the bottom. Also, notice the gradient on the top and the bottom border &#8211; that&#8217;s an optical illusion by the way. The border color is unchanged, the gradient is in the box&#8217;s background

This is an implementation in CSS that most probably will work well in your browser:



<a href="https://ruslanbes.com/devblog/rbes-content/uploads/2017/10/amzn2.html" target="_blank">Open link</a>. It should look pretty similar to the image.

Let&#8217;s dive into the source:

<pre class="brush: xml; title: ; notranslate" title="">&lt;div class="mail"&gt;
	&lt;div class="wrapper" style="border-bottom:4px solid #000111; width:640px; margin: 0 auto;"&gt;
		&lt;div class="amzn" style="height:75px; background-image:url('https://ruslanbes.com/devblog/rbes-content/uploads/2017/10/amzn_bg.jpg'); border-top:3px solid #474F57; border-bottom:3px solid #474F57;"&gt;
			&lt;div class="logo" style="width:150px; margin-left:20px; margin-top:20px; float:left;"&gt;
				&lt;img class="amzn_logo" style="width:150px;" src="https://ruslanbes.com/devblog/rbes-content/uploads/2017/10/amzn_logo.png" alt="Amazon developer"&gt;
			&lt;/div&gt;
			&lt;div class="contact" style="font-family:'Helvetica Neue',helvetica,Arial,sans-serif; font-size: 13px; color:white; float:right; margin-right:20px;margin-top:30px;"&gt;
				&lt;span class="contact_us" style="padding-right: 8px; margin-right: 8px; border-right: 1px solid white;"&gt;Contact Us&lt;/span&gt;&lt;span class="support"&gt;Support&lt;/span&gt;
			&lt;/div&gt;
		&lt;/div&gt;
	&lt;/div&gt;
&lt;/div&gt;
</pre>

`wrapper` div sets positioning and the black border on the bottom.

`amzn` div sets the inner rectangle. It also takes the background gradient and the borders.

Inside it, we have two divs with floating.

The right div is split into two spans. The vertical separator, `|` is implemented as a border between the spans. We could use the pipe character itself but then it&#8217;ll render depending on the system fonts. The border is agnostic to that.

Notice that all styles are inline. That&#8217;s the first thing to keep in mind &#8211; inline styles have less chance being corrupted by an email client

Let&#8217;s send this email to the Gmail account:

<img loading="lazy" width="705" height="181" class="aligncenter size-large wp-image-914" src="https://i2.wp.com/ruslanbes.com/devblog/rbes-content/uploads/2017/10/gmail-received.png?resize=705%2C181&#038;ssl=1" srcset="https://i1.wp.com/devblog.ruslanbes.com/rbes-content/uploads/2017/10/gmail-received.png?w=1260&ssl=1 1260w, https://i1.wp.com/devblog.ruslanbes.com/rbes-content/uploads/2017/10/gmail-received.png?resize=300%2C77&ssl=1 300w, https://i1.wp.com/devblog.ruslanbes.com/rbes-content/uploads/2017/10/gmail-received.png?resize=768%2C197&ssl=1 768w, https://i1.wp.com/devblog.ruslanbes.com/rbes-content/uploads/2017/10/gmail-received.png?resize=1024%2C263&ssl=1 1024w" sizes="(max-width: 705px) 100vw, 705px" data-recalc-dims="1" /> 

And same Email to Outlook:  
<img loading="lazy" width="705" height="465" class="aligncenter size-large wp-image-917" src="https://i0.wp.com/ruslanbes.com/devblog/rbes-content/uploads/2017/10/outlook.png?resize=705%2C465&#038;ssl=1" srcset="https://i1.wp.com/devblog.ruslanbes.com/rbes-content/uploads/2017/10/outlook.png?w=1301&ssl=1 1301w, https://i1.wp.com/devblog.ruslanbes.com/rbes-content/uploads/2017/10/outlook.png?resize=300%2C198&ssl=1 300w, https://i1.wp.com/devblog.ruslanbes.com/rbes-content/uploads/2017/10/outlook.png?resize=768%2C506&ssl=1 768w, https://i1.wp.com/devblog.ruslanbes.com/rbes-content/uploads/2017/10/outlook.png?resize=1024%2C675&ssl=1 1024w" sizes="(max-width: 705px) 100vw, 705px" data-recalc-dims="1" /> 

Yep, that&#8217;s it. Outlook for Windows doesn&#8217;t support `background-image`.

But that&#8217;s not fair, let&#8217;s replace it with `background-color`. Maybe everything else is fine?

<img loading="lazy" width="705" height="214" class="aligncenter size-large wp-image-919" src="https://i0.wp.com/ruslanbes.com/devblog/rbes-content/uploads/2017/10/amazn2.png?resize=705%2C214&#038;ssl=1" srcset="https://i1.wp.com/devblog.ruslanbes.com/rbes-content/uploads/2017/10/amazn2.png?w=1301&ssl=1 1301w, https://i1.wp.com/devblog.ruslanbes.com/rbes-content/uploads/2017/10/amazn2.png?resize=300%2C91&ssl=1 300w, https://i1.wp.com/devblog.ruslanbes.com/rbes-content/uploads/2017/10/amazn2.png?resize=768%2C233&ssl=1 768w, https://i1.wp.com/devblog.ruslanbes.com/rbes-content/uploads/2017/10/amazn2.png?resize=1024%2C311&ssl=1 1024w" sizes="(max-width: 705px) 100vw, 705px" data-recalc-dims="1" /> 

Nah, not really.

I won&#8217;t go into details of how to fix this email. But the basic approach should be to use `<table>` and `<td>` for blocks. Use `cellspacing` and `cellpadding` for positioning and use a pipe symbol. I don&#8217;t know a solution for background image.

To summarize:

  * Background image is gone
  * Logo is unscaled
  * Floating is ignored
  * Using borders for the pipe char didn&#8217;t work
  * Margins and padding are completely wrong
  * Bottom borders are wrong

Also, Outlook adds some of its own CSS classes, renames your classes or removes them completely. It seems it does it as an optimization step if it sees that the class doesn&#8217;t have any supported styles.

Interestingly if you try Outlook for Mac it will work much better.

### Lessons learned

  1. When designing an email template, pay attention to how complex the email is. Keep in mind, you mostly have to use simple CSS. Colors are not a problem, positioning is a challenge.
  2. Check the resources about CSS compatibility in emails 
      * <a href="https://www.campaignmonitor.com/css/" target="_blank" rel="noopener">CSS Support Guide for Email Clients</a>
      * <a href="https://templates.mailchimp.com/resources/email-client-css-support/" target="_blank" rel="noopener">Email Client CSS Support</a>
  3. Testing in Outlook 
      * <del datetime="2017-11-27T19:59:52+00:00">The easiest way is to save an email as an HTML file and open in Microsoft Word, change something and save. Word seem to use the same engine</del>
      * <del datetime="2017-11-27T19:59:52+00:00">Another way &#8211; open the email in Chrome, copy to Gmail (this will copy the HTML) and send to your Outlook account</del>
      * **Update:** Use [Litmus][1]. It allows testing in most popular email clients

 [1]: https://litmus.com/
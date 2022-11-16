---
title: Passing Zend Certified PHP Engineer Exam in 2016
author: Ruslan Bes
type: post
date: 2016-05-26T14:00:12+00:00
url: /2016/05/26/passing-zend-certified-php-engineer-exam-in-2016/
dsq_thread_id:
  - 4868986526
categories:
  - PHP
tags:
  - ZCPE
  - Zend

---
This article is also available [in russian][1].

On 5.5.2016 I passed the Zend Certified PHP Engineer exam on the PHP 5.5 version.  
<img src="https://i1.wp.com/ruslanbes.com/blog/pictures/ZCPE-logo-XS.jpg?w=705&#038;ssl=1" alt="ZCE" data-recalc-dims="1" />  
I want to tell a little bit about what it is and how does it go.

## Why bother getting certified?

The main reason is to <del datetime="2016-05-23T14:49:34+00:00">show-off</del> refresh your knowledge and ensure you haven&#8217;t missed anything. Working several years on the same project and with the same framework (Yii) creates illusion that you know how to do things, and some of the internal PHP functions remain unknown to you (did you know that PHP supports file upload progress out of the box? I didn&#8217;t). For instance I was forced to check SimpleXML, DomDocument and PDO. We have our own wrappers for these things and I never used them directly.

The second reason is that having the certificate is a good thing when applying for a job offer as it is a good filter telling that the candidate is not completely new to PHP.

## So what am I getting from it and how do I get certified?

Officially if you pass the exam, you are added to the [Zend Yellow Pages][2] and then you can put a logo on your business card, CV and so on.

The exam consists of the 70 questions and you have 90 minutes to pass. All questions are in English.

There are 3 types of the questions:

  * Single choice (&#8220;Who wants to be a millionaire?&#8221;-style). They were 60% of the test
  * Multiple choice. Usually pick 2 or 3 answers. 35%
  * Free text. 5% and most of the time it&#8217;s recalling some function names

Topics:

  * Basics
  * Data types and formats
  * String manipulation
  * Arrays
  * Files
  * Functions
  * OOP
  * Databases
  * Security
  * Web features

I haven&#8217;t found a passing grade but it&#8217;s about 70-75%

## Picking the exam date

Good news is — you can try the exam for free on the [Online Test Centre][3] site. I did in on 3.3.2016 and got 68%. Good enough, let&#8217;s pick a date. I went to the [Zend.com][4] and bought a voucher and &#8220;PHP Study guide&#8221;. It costs $225 or €207. After that I have registered on [Peason Vue][5], picked nearest Test Center and entered Voucher code. Actually it was enough to buy only the examination on the Pearson Vue, since the Study Guide was mostly useless, but I&#8217;m getting ahead of myself. At that time I had 2 months to prepare.

## Preparations

First thing to look at is the &#8220;PHP Study guide&#8221; that you can buy from Zend. To be honest I haven&#8217;t found it especially useful and also some of &#8220;Test your knowledge&#8221; questions had wrong answers. So I abandoned it and started googling. In turned out that there are at least two sites with very good dumps of the PHP 5.5 questions: [one][6], [two][7]. I studied one of the dumps and then also learned dumps of 5.3 and 5.0 questions. In the end in my real exam I got only 1 question of 70 that I didn&#8217;t know. It&#8217;s both good and bad.

It&#8217;s good because you can find interesting tricks and study features you didn&#8217;t know or you thought they work differently. All questions are pretty interesting and most of them can be solved by exclusion. And when your logic turned out to be wrong, you can check the manual.

But it&#8217;s also bad since it means anyone can simply remember all those answers and pass the test without even knowing PHP. And that defeats the whole purpose. Before I went to the real exam I had an idea to create a video explaining all of the questions and the logic behind it since they are in public access anyway. But Zend threatens some bad thing for discussing the questions so I&#8217;m considering <del datetime="2016-05-25T20:17:28+00:00">which jail I prefer</del>.

In the end I think Zend has to refactor this exam by either changing questions like &#8220;what is the output of this code?&#8221; into dynamically generated (passing different values) or just prepare 10-20 variations of each question so that it&#8217;s not possible to remember them all.

But I digress. The most active phase of my study was the last week when I studied 50 questions a day and marked the ones I answered wrong. Next morning I checked only the wrong ones.  
<img loading="lazy" src="https://i0.wp.com/ruslanbes.com/devblog/rbes-content/uploads/2016/05/2016-05-06-15.52.34.jpg?resize=705%2C396&#038;ssl=1" alt="Studying Zend exam" width="705" height="396" class="aligncenter size-full wp-image-787" srcset="https://i2.wp.com/devblog.ruslanbes.com/rbes-content/uploads/2016/05/2016-05-06-15.52.34.jpg?w=1000&ssl=1 1000w, https://i2.wp.com/devblog.ruslanbes.com/rbes-content/uploads/2016/05/2016-05-06-15.52.34.jpg?resize=300%2C169&ssl=1 300w, https://i2.wp.com/devblog.ruslanbes.com/rbes-content/uploads/2016/05/2016-05-06-15.52.34.jpg?resize=768%2C432&ssl=1 768w" sizes="(max-width: 705px) 100vw, 705px" data-recalc-dims="1" /> 

What I didn&#8217;t like is too many questions about streams and contexts and all of them applied to `file_get_contents($url)`. Hey, it&#8217;s 2016 already, we have [Guzzle][8] and lots of curl wrappers, am I missing something? And two pages later you get the question &#8220;How do you disable url support in `file_get_contents()`&#8220;. Strange.

In the meantime every week I tried the test exam. You can guess when I started reading question dumps:  
<img src="https://i2.wp.com/ruslanbes.com/blog/pictures/2016-05-06-15_08_05-Exam-history-_-OnlineTestCentre.png?w=705&#038;ssl=1" alt="History of test exams " data-recalc-dims="1" />  
Actually the topmost exam should&#8217;ve been 100% but I got one [ambiguous][9] question.

## The exam day

On the exam day you get up, walk 50 meters and end up in a test center. Well, it was lucky for me it was across the street, but usually you should be there 15 minutes before the exam. Pearson Vue says you need two ids, at least one of them with a photo but it was enough for me having just one with photo. There will be a pen and piece of paper, you may not use your own. For the rest you got PC and mouse and 90 minutes for 70 questions.

There was a possibility to leave comments to the questions, which was nice. Questions may be marked for review if you were unsure or didn&#8217;t answer them. I didn&#8217;t use that since all of the questions were familiar. It took me 50 minutes to answer them all. After that you get the list of questions once again and you can once again check everything and answer the missed questions. Click &#8220;Continue&#8221;, get a message that you successfully passed the exam and wait for the confirmation to be printed. You get the certificate in a couple of weeks by post.

So, try it. In current conditions passing this exam became extremely easy, and you can get it before companies did realize that.  
<img src="https://i1.wp.com/ruslanbes.com/blog/pictures/zendcert.jpg?w=705&#038;ssl=1" alt="Zend Certificate" data-recalc-dims="1" />

 [1]: https://ruslanbes.com/blog/all/sdayom-ekzamen-zend-certified-php-engineer/
 [2]: https://www.zend.com/en/yellow-pages/ZEND028721
 [3]: http://onlinetestcentre.com/200-550.html
 [4]: https://shop.zend.com/en/zend-php-certification-guide-pdf.html
 [5]: http://www.pearsonvue.com/zend/
 [6]: http://gratisexam.com/zend/200-550-exam/
 [7]: http://www.aiotestking.com/zend/category/exam-200-550-zend-certified-php-developer/
 [8]: http://docs.guzzlephp.org/en/latest/
 [9]: http://www.aiotestking.com/zend/what-is-the-output-of-this-code/
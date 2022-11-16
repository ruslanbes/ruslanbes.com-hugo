---
title: Why backdoors are bad
author: Ruslan Bes
type: post
date: 2018-07-03T22:25:22+00:00
excerpt: There are people who argue that backdoors in hardware and software, that allows the government to access data bypassing the standard passwords are a good thing. Those are governments, they argue, I want to use passwords to protect myself from bad guys, but I also want the government to be able to break the devices of bad guys.
url: /2018/07/04/why-backdoors-are-bad/
dsq_thread_id:
  - 6771332270
categories:
  - Security
tags:
  - backdoor

---
<img loading="lazy" class="aligncenter size-large wp-image-982" src="https://i0.wp.com/ruslanbes.com/devblog/rbes-content/uploads/2018/07/StockSnap_8AD1BRM0AC-1200x799.jpg?resize=705%2C469&#038;ssl=1" alt="" width="705" height="469" srcset="https://i2.wp.com/devblog.ruslanbes.com/rbes-content/uploads/2018/07/StockSnap_8AD1BRM0AC.jpg?resize=1200%2C799&ssl=1 1200w, https://i2.wp.com/devblog.ruslanbes.com/rbes-content/uploads/2018/07/StockSnap_8AD1BRM0AC.jpg?resize=300%2C200&ssl=1 300w, https://i2.wp.com/devblog.ruslanbes.com/rbes-content/uploads/2018/07/StockSnap_8AD1BRM0AC.jpg?resize=768%2C511&ssl=1 768w, https://i2.wp.com/devblog.ruslanbes.com/rbes-content/uploads/2018/07/StockSnap_8AD1BRM0AC.jpg?w=1410&ssl=1 1410w, https://i2.wp.com/devblog.ruslanbes.com/rbes-content/uploads/2018/07/StockSnap_8AD1BRM0AC.jpg?w=2115&ssl=1 2115w" sizes="(max-width: 705px) 100vw, 705px" data-recalc-dims="1" />  
([Image source][1])  
There are people who argue that backdoors in hardware and software, that allows the government to access data bypassing the standard passwords are a good thing. Those are governments, they argue, I want to use passwords to protect myself from bad guys, but I also want the government to be able to break the devices of bad guys.

Yes. Right. The problem is the devices you use and those used by &#8220;bad guys&#8221; are the same devices. It would be nice if every drug dealer would go exclusively with Motorola Moto G6 phones, but that doesn&#8217;t happen for some reason. So if there&#8217;s a backdoor, it affects **all** people &#8211; both good and bad.

Now let&#8217;s imagine we allowed backdoors widespread. We have them in software like Windows and iOS and hardware like laptops, modems, phones, microchips and so on. But the keys are controlled by the manufacturers and NSA. So far so good, criminals are caught, everyone&#8217;s happy.

Then one day some future Edward Snowden leaks the backdoor that allows bypassing ATA password on Dell laptops.

Or maybe it&#8217;s stolen through a security hole.

Or some employee carelessly left his computer unlocked.

Or have chosen a [poor password][2].

Or trusted [someone who was good at social engineering][3].

Or maybe it [already exists][4]

Oops&#8230;

So the backdoor makes its way to the hacker community and to some not so good people. Then those people use this backdoor to unlock a stolen laptop of some Microsoft official. Now they have access to the backdoor to Windows Server. They use it to hack into the server infrastructure of Huawei. There they learn backdoors to Huawei modems, routers, and phones. And so on.

This whole thing can spiral out of control pretty fast. Once one part of the system is compromised you can generate cascade effect that will get you access to everything. This is known as the [Single point of failure][5] and among people wearing T-Shirts saying &#8220;**`rm -rf /*` is awesome&#8221;**  is widely considered as **a very bad thing**.

And now to make matters worse think about how fast the IoT device number grows. Having backdoors in there may allow people to very literally do some nasty physical things to other people while staying invisible and very far away. And if there&#8217;s one thing everyone desired since being introduced to the magic of the Internet it is to punch that sucker through the monitor.

Now, of course, this is some dramatical simplification. And also after learning about the leak Dell can issue a security patch that changes encryption and makes it useless, but still, there will be some window of opportunity.

Imagine a key master in your town got drunk and left the key to his house in the bar. You took it and entered his home, where you found the key from his office. Now you got to his office and lo and behold, you see hanging on the wall the keys from every building in your town each one being carefully labeled.

Do you want to live in a town like this?

 [1]: https://stocksnap.io/photo/8AD1BRM0AC
 [2]: https://haveibeenpwned.com/Passwords
 [3]: https://en.wikipedia.org/wiki/Kevin_Mitnick
 [4]: https://security.utexas.edu/education-outreach/BreakingATA
 [5]: https://en.wikipedia.org/wiki/Single_point_of_failure
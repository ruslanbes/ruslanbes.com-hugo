---
title: Updating Java plugin for Firefox Portable
author: Ruslan Bes
type: post
date: 2012-09-04T12:14:14+00:00
url: /2012/09/04/updating-java-plugin-for-firefox-portable/
dsq_thread_id:
  - 4833370673
categories:
  - 'Tips &amp; tricks'
tags:
  - Firefox portable
  - Java
  - Jportable

---
<img loading="lazy" class="alignright" title="Firefox Portable" src="https://i1.wp.com/cdn.portableapps.com/firefox.png?resize=128%2C128" alt="" width="128" height="128" data-recalc-dims="1" />There are two methods to upgrade Java Plugin in Firefox Portable. First is the [recommended one][1] — download [JPortable][2] and put it in the same PortableApps dir where your Firefox resides, that is you should get following structure:

  * PortableApps/CommonFiles/Java — here you have JPortable
  * PortableApps/FirefoxPortable — here you have Firefox

There is another way to accomplish the same task. It is useful for instance if you have dozen of portable Firefoxes installed. Suppose you&#8217;re a web developer and you want to test your website or firefox extension against multiple versions of Firefox. In this case you can go [here][3] download and install jxpiinstall.exe. Then you automatically get the latest Java plugin in all Firefoxes you have installed.

 [1]: http://portableapps.com/support/firefox_portable#plugins
 [2]: http://portableapps.com/apps/utilities/java_portable "JPortable"
 [3]: http://www.java.com/en/download/inc/windows_upgrade_xpi.jsp
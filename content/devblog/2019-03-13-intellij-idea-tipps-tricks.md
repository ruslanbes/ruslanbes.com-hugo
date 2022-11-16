---
title: 'IntelliJ IDEA Tips & Tricks'
author: Ruslan Bes
type: post
date: 2019-03-13T19:25:37+00:00
url: /2019/03/13/intellij-idea-tipps-tricks/
classic-editor-remember:
  - block-editor
categories:
  - Java
  - Productivity
  - Software engineering
  - 'Tips &amp; tricks'
tags:
  - IntelliJ IDEA

---
This is a small post written after a presentation on [Java User Group Graz][1]&nbsp;regarding IntelliJ tricks.

The presentation is available here&nbsp;[on Github][2]&nbsp;and I want to summarize some interesting things that I didn&#8217;t know / forgot:

  1. `Shift + Shift`: &#8220;Search everywhere&#8221;. Nice and very powerful shortcut. Search in class names / file names / methods. Didn&#8217;t know.
  2. If you are stuck with too many editor tabs, then&#8230;&nbsp; turn them off (Set tab position to&nbsp;`None`). Use&nbsp;`Ctrl+E` (recent files) instead. As a bonus you can search for the name of the file there (just type it)
  3. Close all tool windows, use&nbsp;`Alt+1..9`&nbsp;to show them temporarily and&nbsp;`Shift+Esc`&nbsp;to close them.
  4. Instead of &#8220;Project Structure&#8221; tool window (&nbsp;`Alt+1`&nbsp;) use the navigation bar:&nbsp;`Alt+Home`&nbsp;and then use arrows to navigate.
  5. Now you have the whole screen for editing, but chances are the code fills only the left part. You can use&nbsp;`Split Screen Vertically`&nbsp;to have two files opened simultaneously (or the same one). There&#8217;s no shortcut for that, use&nbsp;`Ctrl+Shift+A`&nbsp;and search for &#8220;split&#8221;
  6. In the &#8220;Project Structure&#8221; tool window go to settings and enable&nbsp;`Autoscroll to Source`&nbsp;and&nbsp;`Autoscroll from source`&nbsp;. Almost always useful.
  7. Useful plugins:&nbsp;`Key Promoter X` &#8211; detects what you do with a mouse and gives a notification window if you can do it with a keyboard shortcut.
  8. If you have a variable&nbsp;`a`&nbsp;then try typing&nbsp;`a.null`&nbsp;and pressing Enter. What you get is:
  9. `if`&nbsp;`(a == null) { }`
 10. Similarly&nbsp;`a.notnull`&nbsp;creates:`if`&nbsp;`(a != null) { }`  
    

But there are many more, check&nbsp;`Live Templates`&nbsp;in settings

 [1]: https://www.meetup.com/Java-User-Group-Graz/events/247255514/
 [2]: https://github.com/dilbernd/idea-productivity/blob/master/talk.md
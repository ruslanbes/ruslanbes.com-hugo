---
title: Thoughts about ACL
author: Ruslan Bes
type: post
date: 2013-01-07T09:01:02+00:00
url: /2013/01/07/thoughts-about-acl/
categories:
  - Software engineering
tags:
  - ACL
  - Permission

---
We tried using [ACL][1] in one of our project with no success. The problem with it from my point of view is that it just fails when working with list of objects. Large list of objects. Here are some queries that are difficult to be run fast if the list of objects is at least 10000 items (which is a normal case for forum posts):

  1. Some objects are available to read by any user if they have appropriate attribute (`public=1`). Get a full list of objects which are available to read. As a bonus — how would you maintain the invariance of this permission? You have about 10000 users and about 10000 objects.
  2. Get a list of objects available to read and a list of objects available to edit. Show a list of the queried objects and add links &#8220;edit&#8221; near those objects you can edit. There are many place in our application where we need it better be a universal solution.
  3. There are objects of types Orders and of type Users. We need a report containing join of the underlying tables. To get into report either Order or User should be accessible by a current user. If you think you can just create two &#8220;IN&#8221; conditions then I have bad news for you &#8211; [sometimes it doesn&#8217;t work][2].
  4. Get a list of objects that are accessible by a small group of users. How would you plan to join their rights?

Add to this, each page load may take up to 10 permission checks. So they better be fast. And I have no idea how to make those queries get a good performance using the ACL.

So before picking the permission system for your application try to think how would you solve those task.

BTW, in our project we did solved it but our solution works for a strictly defined business domain and it may or may not work for you. Our general idea was that every object in the system (User, Forum, Forum post, News) directly or indirectly belongs to certain usergroup and we can express that relationship in a simple `tablename.usergroup_id = X` check or `INNER JOIN sometable ON ... AND sometable.usergroup_id = X`. The second idea was that for every permission we could easily get a list of usergroups where you have that permission. Or get a sub-query that will select that list. So to perform permission check we just added that subquery into the where string and link it to the appropriate usergroup_id field. For most objects this works automatically.

This turns out not to be flawless. First we discovered that problem I mentioned earlier when some object besides usergroup check required also attribute check, but that was relatively easy incorporated into our system. There was hovewer a bigger problem when we realized some permission are object-agnostic and rather used to turn on and off some functionality. So we couldn&#8217;t bind those to usergroup and were forced to do nasty SQL hacks. So if you follow this way, be careful. Unfortunately there is no silver bullet in rights system management

 [1]: http://en.wikipedia.org/wiki/ACL "ACL"
 [2]: http://stackoverflow.com/questions/400255/how-to-put-more-than-1000-values-into-an-oracle-in-clause
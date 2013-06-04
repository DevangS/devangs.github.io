---
layout: post
title: Blocking The Facebook Newsfeed
category: productivity 
---

Why I Did It
============

I often find myself wanting to check my Facebook to see if I have any new messages or notifications. Regardless of their existence, I always end up wasting a significant amount of time simply scrolling through my news feed, every so often stopping at an item of interest. 

This adds no real value to my daily life because one of my friends will always inadvertently notify me in some form or the other about important updates in my friends' lives. The other updates are mere distractions that allow me to spend less time getting work done. As a result, I wanted to be able to block the news feed, but still access messages, notifications, and of course be able to specifically go to a friends page who I want to see updates for. 

How To Do It
============
I did this by blocking the DIVs for Facebook by marking them as ads in [AdBlock]. You can add the DIVs by right-clicking on the AdBlock icon -> Options -> Customize -> Manually Edit Filters.  Then add the following line: 
   > www.facebook.com##DIV[id="pagelet_home_stream"]

I also added the following lines to block the left and right side pane, because I thought it cluttered up the interface. 

   > www.facebook.com##DIV[id="sideNav"][class="uiFutureSideNav"]
   > www.facebook.com##DIV[id="pagelet_ego_pane_w"]
   > www.facebook.com##DIV[id="sideNav"][class="uiFutureSideNav uiNarrowSideNav"]

The result looks something like this: ![Imgur](http://i.imgur.com/TydXWMZ.png?1)

[AdBlock]: https://chrome.google.com/webstore/detail/adblock/gighmmpiobklfepjocnamgkkbiglidom

---
layout: post
title: Spring Project - Scheduling
date: '2017-03-19T12:58:24-05:00'
tags: []
tumblr_url: http://mattpiazza.com/post/158592273905/spring-project-scheduling
---
In the world of computer science, there are many difficult problems under the ‘scheduling’ umbrella - interrupts, multi-threading, locks, and so on. However, in the real world, there are seemingly simple scheduling tasks that human people engage in (either via manual arrangement or with the help of #software). The specific, human example I’m thinking of is scheduling part-time employees in a food & bev environment.

My brother, DJ, is helping open a few restaurants while managing a coffee shop. I have often seen him come home, open up google calendar, and begin to drag and drop 4, 6, and 8 hour blocks while calculating, in his head, which roles (dishwasher, barista, etc) need to be filled when, who can fill them, and who has requested time off. Of course, being an annoying software boy, I made fun of him for not using some product that could take in the resource pool, constraints, and desired staffing and output a decent schedule.

It turns out there are 2 SaaS products that aim to solve this problem and many full-blown HR systems that allow admins to schedule staff. These 2 standalone options are extrememly overpriced (and probably include features that are only used by a small minority of organizations).

When I Work is a beautiful product that has iOS and Android apps ready to download. It’s about $1 per user. They have a hard-to-find free tier that is significantly limited (< 6 users, no hallmark features, etc). It seemed like an OK choice for a very-small place. But! It only allows schedules to be planned 2 weeks ahead of time (in the free version only, I think).

Schedulefly is not as sleek as When I Work, but it is at least as powerful (plus you can schedule arbitrarily far into the future as far as I can tell). But it rather expensive - with the cheapest plan starting at $30 per month.

Goal

Write a simple-to-use scheduling web app exclusively using free cloud services. I want to avoid charging money for the majority of users. Sure, it’d be fun to make some money with this, but I think that’s where our friends at Schedulefly went wrong. If I can use free-tier AWS/GCE resources, then the only ‘cost’ of running it will be my time.

Minimum requirements:

employees have skills that correlate with roles they can play for a shift of work (running the register, busboy, etc)
employees can describe their availabilty and a maximum number of hours they’re willing to work per week
admins (managers?) can describe which skills each employee has
admins describe how many of each role they want at any time during a day
admins can reassign shifts after the system has assigned them
Table stakes for a real product:

admin-approved shift swaps
notifications of shift changes
templates for typical week schedules
multiple restaurant management (it can probably be more general than restaurants, pretty much any place with multiple parttime employees like retail storefronts and so on)

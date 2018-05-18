---
layout: post
title: "“Complex Logic” in Go Templates"
date: '2017-03-23T22:17:09-05:00'
tags: []
tumblr_url: http://mattpiazza.com/post/158761834585/complex-logic-in-go-templates
---


Last week, I was looking to do something in a Go template that I obviously should not have been trying to do. In my dive through stackoverflow and the mailing lists, I came across a pretty funny quip: Accipiter Nisus said “Whether or not assigning a new value to a variable is considered complex logic might be up for discussion.” And, of course, I sat at my desk laughing at the dystopian hellscape through which we each wade every day.

I don’t entirely agree with the idea but, in this context, it’s right. I was trying to solve the wrong problem. Or the right problem in a sub-optimal way due to a misunderstanding of the tools I had at my disposal.

The problem I was trying to solve was a pretty straightforward web programming task - after a ‘delete’ action, remove an element (and its children) from the page. I already had an AJAX endpoint to handle the delete request and I wanted to add some client-side stuff to handle the clicks. My naive javascript skills told me to add unique IDs to each element that users could click (so I could getElementById later). That’s when I started trying to add some sort of accumulator to the template ({{i := 0}}, etc) which is not possible in Go’s templating engine.

My solution was to set the IDs of those interactive elements via javascript after the page loaded. It is pretty bad. I worked through some of a javascript tutorial like 3 days after I wrote this shitty code and now I think I know a more idiomatic way to do it: using document.querySelecorAll to grab all of the interactive things I wanted to work with. Then add an event listener on the appropriate child element (delete button) that makes the AJAX request and removes the outer element on success. This approach is somewhat limited by scoping issues, for example, if you had:

<div class="”deletable”">
Matt <button type="”button”">delete</button>
<div>


The click listener on the button should not remove this, but it could remove this.parentNode or the handler could be bound to the div element.addEventListener(“click”, deleteHandler.bind(outerDiv));

Making use of javascript’s this keyword and iterating across the nodelist returned by querySelectorAll are thousands of times better than trying to cajole a template to do something it wasn’t designed to do.

The moral of the story is that you should learn javascript.

tl;dr: if you’re having trouble doing seemingly simple things in your html templates, you’re probably approaching the problem incorrectly.

---
layout: post
title: ringzer0team challenge 113 writeup
date: '2015-11-15T19:02:56-06:00'
tags:
- ringzer0teamctf
- wargame
- hacking
tumblr_url: http://mattpiazza.com/post/133300808080/ringzer0team-challenge-113-writeup
---
big ups to John Anderson (lampwins) for having the curious mind and php know-how to crush this challenge in a matter of minutes – I was just along for the ride really

this challenge came from ringzer0team.com’s set of web challenges. we have been running through them with the cyber security club for the past few weeks for an upcoming web pen-testing engagement (maybe you’ll be able to read about it here later ;) ).

the scenario was that you’re resetting the password to the admin console of a website (the challenge lives here). they provide the source code (php) for the password resetting mechanism. it can be summed up thusly: use the current time to seed a PRNG, grab a ‘random’ number between 1000000000000000 and 9999999999999999 to use as a password reset token. then the page will accept GETs with a url parameter k which is compared to the previously generated token. if they match then your new password is printed to the screen, otherwise you get nothing. there is also a 1hr expiration for each token.

the biggest hint for this challenge is that the current time is included in the page telling you that your password has been reset:


[spoilers follow]

and we know that that time is used to seen the PRNG. once we convert that to unix time (seconds since 1 Jan 1970, UTC) we have the seed and can replicate the line {% highlight php %}  $token = rand(1000000000000000,9999999999999999); {% endhighlight %}

bonus: php one liner (once you have the timestamp): {% highlight bash %}php -r "srand(1447631726); echo rand(1000000000000000,9999999999999999);" {% endhighlight %}

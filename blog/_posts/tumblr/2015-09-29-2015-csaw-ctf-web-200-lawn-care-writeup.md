---
layout: post
title: '2015 CSAW CTF: web 200 lawn care writeup'
date: '2015-09-29T23:35:43-05:00'
tags:
- ctf
- csaw
- hacking
tumblr_url: http://mattpiazza.com/post/129753260235/2015-csaw-ctf-web-200-lawn-care-writeup
---
I love lawn care. not that I do any, per se. but the the first challenge I attempted with my team for the college of charleston’s cyber security club, was based on a nice, healthy lawn. overall, it was an easy challenge that took a bit of fiddling up front to find the vulnerability. when you first enter the challenge you are presented with the following: 
the most interesting things off the bat are the links to careers in the footer for indications of what technologies are in use here as well as the username & password fields at the top. clicking the green grow button makes grass grow through the magic of javascript. the careers page was mostly jokes, but it did include about three interesting bullet points: javascript, git, and erlang. my first thought was sql injection, but that was wrong. time to tap ctrl-u (command-option-u) and view that source. yes, yes, javascript as they mentioned on the careers page. the first block of js on the page does three things:

{% highlight javascript %}
        document.getElementById(''login_form'').onsubmit = function() {
            var pass_field = document.getElementById(''password''); 
            pass_field.value = CryptoJS.MD5(pass_field.value).toString(CryptoJS.enc.Hex);
    };
    $.ajax(''.git/refs/heads/master'').done(function(version){$(''#version'').html(''Version: '' +  version.substring (0,6))});
    initGrass();
{% endhighlight %}



first, it assigns a handler that hashes (md5) the password when submitted. client-side hashing, as the saying goes.I’m no js professional, but then I think it fills the element at anchor #version with the first six characters from .git/refs/heads/masterlastly it calls initGrass() to start the gameoh wait. is that a reference to the .git hidden directory? (yes). I traipse over to a local git repo of mine and run tree. an interesting file I see whose existence I can easily verify on the lawncare game is .git/COMMIT_EDITMSG. off to COMMIT_EDITMSG I go. I think I''ll just put mah source code riiiiighhhht here. Perfectly safe place for some source code. it saysok well their code is somewhere on here. but how do I get it? is it in /src or something? OH WAIT it’s a dang git repo, that I can probably clone. git clone http://54.165.252.74:8089/.git does the trickin the repo there are 4 files to which I previously did not have access. the rest are js and html. the 4 new ones are __HINT__, premium.php, sign_up.php, & validate_pass.phpI start in sign_up.php and saw an easy sqli vulnerability: $query = “SELECT username FROM users WHERE username LIKE ’$user’;”; obviously the next logical thing to do is to put a ’%’ in as my username during the signup process. I do that and get: so closethen I pop into validate_pass.php to find the juicy vulnerability:
you can see there on line 7 that the hash is being pulled out of some users table and compared to the hash that was passed in from the client. first, it immediately fails if the lengths are not equivalent. but we know that an md5 hash is 16 bytes or 32 hex characters, so that is an odd check. finally it iterates through both hashes it has in order to compare the strings. so when would the length of these two hashes be the same, but the values of the hashes be easily computable or guessable?? ah, when the hashes lengths are both 0! if we pass in a username that is not in the users  table, then we can assign an empty string to $hash – take your pick of obscure username. on the client side we also have to pass in an empty string as the password hash. two things ‘prevent’ us from doing this: the field is labelled as required in the html so chrome will not let us submit the login form unless that field is populated or the required label is removed. then we have the javascript that is hashing the password on submit. every input to an md5 hashing function will produce 16 bytes of output regardless of the size of the input, so entering an empty password will not result in an empty hash. we can either delete the javascript from the page or remove the identifier by which it is selecting the password field (ie: getElementById). then we can submit a bogus username and a blank password to capture the flag (curl would also be a quick and easy way around the client-side validation/hashing)neat-o team

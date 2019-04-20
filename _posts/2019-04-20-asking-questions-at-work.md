One of my favorite parts about working in a team is answering questions.
Someone – a peer developer or maybe an internal user – experiences unexpected
behavior and they want to understand what’s happening so they ask around. When
those questions come to me I almost always do the same thing: read the code.
This was particularly challenging at PhishLabs since the layout of the legacy
Symfony code was a bit byzantine (after about a year of recursive greps, I finally
figured out where the most troublesome code lived). I like these sorts of questions
for a few reasons: helping people out makes me happy and I value opportunities to
learn about new thorns or behavior in the system. Learning about these new behaviors
is critical to wiping out outdated or incorrect assumptions which makes future
development and troubleshooting quicker. I also don’t mind interruptions. However,
a lot of code writers abhor interruptions and – especially when you’re a new hire –
you don’t want to feel like you’re the person constantly interrupting folks (I mean
you should ask as many questions as you can to get up to speed, but the feeling persists).


So then what are the optimal steps to asking questions at work? Step one is to read
the code. If you’re not already familiar with the execution paths or code layout, then
add some printlns or if you’re lucky enough to have a highly instrumented codebase,
read some execution traces. grep will get you pretty far too: the error text or log
message that results from this newfound behavior is a great starting place. From there
I usually go down the call stack until I find the source of the behavior.


Finding the source of your woes can be a bittersweet moment. It may not immediately
answer your question like a coworker or architect might, but hey you found it on your
own and now understand one full execution path that much better! If you’re working in
a brittle or legacy system then you may fear “fixing” the thing you’ve found. Oftentimes
more searching will be required to see if any other code depends on the unintuitive behavior.
If not change away! If yes then figure out how to satisfy both needs, by refactoring the old
code, for example. That sounds like a lot of work to me but it’ll pay off immensely the next
time you’re poked by this section of code. I fear to work again in a codebase where these 
behaviors are splintered and molded instead of living with one version of each function or
component. At the point when you’re considering a midsized refactor you should be bringing
up design questions with folks on your team that are familiar with (or responsible for) the
potentially impacted systems.


What have you accomplished by searching out this answer on your own? You’ve increased your
understanding of the codebase making your future easier. You’ve obviated any assumptions or
understandings you had that no longer apply. And, if it’s important to your teammates, you’ve
avoided interrupting anyone for what they may see as a “given” or a base assumption (of course,
any “given” is up for debate when it’s central to the strange behavior you’re witnessing). Better
yet, your comprehension of this ever-widening portion of the codebase means you can now be a
resource to others on your team and answer their questions with only a quick re-read of the code.


I highly recommend Bryan Cantrill’s talk about [Debugging Under Fire](https://www.youtube.com/watch?v=30jNsCVLpAE).
It’s really about asking the right questions and inching closer to the truth during an outage.
But I think the same thing applies when you’re living in your normal world at work.

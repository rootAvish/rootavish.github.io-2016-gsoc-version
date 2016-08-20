----
 template: article.jade
 author: rootavish
 date: 2016-08-23
 title: Bad Design, Finalising & a Lookback
----

Hi, since this is the final one, and because I'm somebody who always has their
Eureka moments in the nick of time, this is going to be a long one. But I think
you've had enough of short and sweet from my side.  

If you don't like to read much, I suggest you turn back now.

<span class="more"></span>

So, as always, to re-iterate the goal  of this project is a sweet and simple
one: move signals away from `PyDispatcher` in Scrapy. As you know, the
approach that we chose to follow to do the same was to go the Django way and
use `django.dispatch` as a starting point.  

For the last couple of weeks, y'all have been hearing about my fabled bench-
marking suite. The reason why I never shared a link to the same in my blog posts
was because I could never decide on whether the way I'm presenting those is
right, but with deadline coming up, I finalised on the `perf` module that is
used in `Djangobench`. Before we move any furhter, [here's a link to the
benchmark suite](https://github.com/rootavish/scrapysignalbench) and [here's a
sample output of those benchmarks]. Now that that's out of the way, the rest
of the post is going to concentrate on how we got there, and a problem that
I encountered in the final weeks due to over-engineering on my part.

As you can see, at this moment the `receiver_no_kwargs` benchmark is on average
about 1.5 times faster than how it previously worked. Now, let me tell you about
a little something, `robust_apply` is a nuisance, so the original authors of
`django.dispatch` had the brilliant idea of completely breaking backward
compatability and getting rid of receivers that do not take variable keyword
arguments. That however is not an option that I could follow. So being an
amateur who hasn't had to deal with breaking compatability upto this point, I
decided to not introduce the same in `scrapy.dispatch`, rather I tried to work
up some magic inside `scrapy.signalmanger`, specifically the following "hack":

```python
if receiver.__repr__() not in self._patched_receivers:
    self._patched_receivers[receiver.__repr__()] = \
            lambda sender, **kw: _robust_apply(receiver, sender, **kw)
```

Right, so as you can see, the flow here was `crawler` -> `SignalManager` ->
`Dispatch` -> back to proxy in SignalManager. The method call overhead that was
now introduced into the mix spelt disaster for this benchmark. Disaster. Not
only that, sending receivers that were not proxied in this way was 0.5X slower
than the raw dispatcher-dispatcher benchmark. Owing to this over engineered
mess, I took on the task of writing this part again, with just a couple weeks
of the coding period left to go. Due to the constant support that I've had
from my mentor Jakob along the way, I'm relieved to say I was able to
accomplish the same(which you already knew if you took the time to go through
the benchmarks at the start :P).  

Another important design decision this week was the one to deprecate `scrapy.
utils.signal`. You'd think that is something that would be trivial since we're
moving to `scrapy.dispatch` but the original plan was for the methods in there
to serve as pass through methods between `scrapy.signalmanager` and 
`scrapy.dispatch`.That however is no longer the case and our benchmark for
receivers that accept kwargs shows that there is little to no overhead between
that and raw signal performance.  

So, with unit tests done, benchmarks done, optimizations done, it came down to
the documentation. Now, I didn't realize that all Python documentation ever is
down using ReStructured Text. So I used up a couple of days to get the documen-
tation done, however I'm pleased to tell you that even though I've not shared
the same as of yet, it too is done.  


However if you did look through the benchmarks you would have noticed that the
connection time of signals to receivers has actually increased instead of
decreasing. Well, most of that time is taken up in resolving whether a receiver
accepts keyword arguments, and raising a deprecation warning. So even though it 
reads as being 2.5X slower than before, that time is actually negligible. Plus,
in Scrapy unlike django, it's not connecting to signals that's the problem,
it's sending them multiple times.

Looking back, I would say at lest from where  I stand the project was a success.
I was able to learn a ton of new stuff, and make something cool. At the end of
the day, that's all that matters right? Also, I got to work with some wonderful
people at Scrapinghub, specially my mentor Jakob, who put more thought into code
review than I guess I put into writing it :), but then again, I digress. I
would like to extend this longer, but I'll be posting the three page document
I'll be submitting to Google on here too, so I guess you can read about it then.
Until then, Rootavish signing off.

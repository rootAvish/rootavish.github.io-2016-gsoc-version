---
template: article.jade
author: rootavish
date: 2016-06-24
title: Midterm Evaluations
---

### What's done

In the time that has passed since I last posted one of these, I managed to get a prototype of scrapy to work using the new Signals API. This introduced two very significant API changes into Scrapy.

<span class="more"></span>

* All Signals now need to be objects of the `scrapy.dispatch.Signal` class instead of the generic python `object`
* All signal handlers must now receive `**kwargs`

The first change would not affect the existing extensions/3^rd party plugins much since declaring new signals is not something for the most part extensions do, and using `PyDispatcher` to call the signals instead of the `SignalManager` class has long been deprecated in Scrapy. To accomodate this, the Scrapy `SignalManager` has not yet been phased out and would still be functional, although possibly deprecated depending on how the performance benchmarks work out, and whether avoiding the overhead for the method calls is required.  

The second of these changes however, affects the majority of these extensions and requires that we accomodate in someway. The solution required accomodating the `RobustApply` method of PyDispatcher in Scrapy, this method however would considerably affect the performance of the module, and so in order to have the faster signals one would be required to use handlers with keyword arguments.  

The API was also modified to accomodate twisted `deferred` objects to be returned, and the error handling changed to use the `Failure` class from `twisted.deferred`.  

The new module has also been unit tested for the most part, with some tests borrowed from Django since they're the original authors of this signals API.
I'm currently working on the benchmark suite, writing spiders that use non-standard signals and calls to test the performance. Eariler, signaling through `send_catch_log` used to be the biggest bottleneck requiring 5X the time required for HTML parsing. Any improvements we can do on that, the better although ideally I would like if we could make it so that signals are no longer the bottleneck to the crawling.  


** The following section is under construction. **

### What needs to be done

Following the midterms, the highest priority would be to complete the bechmark suite so we know the viability of the approach we have used thus far and where to proceed from here. In case the results obtained are satisfactory, we shall then continue to make backward compatibility fixes and re-writing algortihms that are still not as efficient as they can be and look to maximize performance. We can continue on to provide full backward compatibility with object() like signals, however that would come with the trade-off that the performance of them would be more or less same as that of what was previously achieveable from the API.  

Another major requirement would be for me to write good documentation of these parts, since these are essential to anybody writing an extension. We would also need to be on the lookout for regressions, if any.


~ Avishkar

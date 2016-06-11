---
template: article.jade
author: rootavish
date: 2016-06-10
title: Rewriting Scrapy Signals, Part I
---

In this report, I shall present a brief summary of the work done upto this point. To iterate, the end goal of the project is to improve the efficiency
of Scrapy's signaling API. For this purpose the idea was to use Django's signaling mechanism which claims to make the signals 90% faster. Consequently, according to plan the first step was to port `django.dispatch`, modify it to our needs and create the `scrapy.dispatch`. About a week and a half was spent on that front in first understanding the way that library is written, and what would changes needed to go into making the same work with Scrapy, and then actually making those changes. `django.dispatch` refactored much of the code of `PyDispatcher` into a `Signal` class. It also introduced a caching mechanism for receivers and introduced the `weakref` module into the code.    

<span class="more"></span>

The `send_robust` method was used as a starting point for the `send_robust_deferred` call to incorporate methods returning deferred calls, this was required for re-writing the `scrapy.utils.signal` module which has the `send_catch_log` and `send_catch_log_deferred` modules. These methods are relative inefficient and tend to bottleneck the scraping process, and were one of the reasons behind the re-write of signals. The next step involved was to re-define the core signals at the heart of Scrapy as instances of `scrapy.Signal` instead of generic `object`class. The `signalManager` class also needed to be changed however the API of the same was kept consistent for backward compatibiliy.


You can track the progress and check out how the project is coming along [here](https://github.com/rootavish/scrapy/tree/signal-rewrite) and [here](https://github.com/scrapy/scrapy/pull/2030) although I must admit I do not push frequently and most commits are either local or small test pieces written outside the mainline code. I shall however, be pushing a working prototype by the end of Monday so that's there to look out for.
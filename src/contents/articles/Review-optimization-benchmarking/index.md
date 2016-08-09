----
 template: article.jade
 author: rootavish
 date: 2016-08-07
 title: Code Review, Optimizations and Formal Benchmarking
----

Hi,

Firstly, sorry for the extremely late blog post this time around, however I was
waiting on my mentor's comments before I gave another status report because I
wanted his take on where we are progress wise and the status of the pull
request. So without further ado, let's get into it.

<span class="more"></span>

The majority of the last two week were spent in writing unit tests and cleaning
the code wherever possible, removing any outdated constructs and pushing code
for review. I also formalized the benchmarking suite using the `djangobench`
code as a starting point as mentioned previously, however there were some fin-
ishing touches left the last time which were completed in this code cycle.

The test coverage of the patch is complete and we have 100% diff coverage, all
seems well there.

Finishing with the benchmarking, I started looking into documentation as due to
the refactor a re-write of the Signal API documentation is in order, even though
we still do have full backward compatability support. Also, now since the code
review comments are in I'll be looking to work on those issues and get them sorted
out at the earliest. Also, we agreed that the benchmarks would not make sense
as a part of `scrapy bench` since that would require that we keep the old
dependencies as a part of the project, which would make no sense as they are
no longer required. The best solution that we came up with for the same is to
write the benchmarks elsewhere however then just include a link to them somewhere
in the PR itself so we have a history of the same maintained.

All in all, we're happy with how the project has turned out, and we'll probably
be seeing the PR merged into the mainline sometime in the future.

I'll update this post as soon as I can think of more stuff I want to write :)

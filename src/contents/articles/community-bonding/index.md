---
template: article.jade
author: rootavish
date: 2016-05-22
title: Community Bonding
---
Hey there, this is the first in the series of posts aimed at documenting my experience as a GSoC 2016 student for regular reporting to the org,
and for my own reference to have something to look back on in retrospect. For the first post, let me start with spouting off a little about myself,
and then I'll talk about how the experience has been to date.

<span class="more"></span>
I'm a(or rather was) a final year student of Computer Engineering at a college in New Delhi, and I've been a part of the GSoC program back in 2014,
where I worked for [MATE desktop](http://mate-desktop.org). For the last year or so, I've been dabbling in data science and machine learning, and came
to know of Scrapy when I used it for one such project early on to scrape opinions from an e-commerce site. Fast forward a year and casually browsing through
PSF's list of GSoC organizations I got to know that these guys were also participating and decided to give it a shot. Given how hectic the schedule was for me back
in February, I would be lying if I said that the reason for my selection was me being some sort of a big-shot programmer who came in all guns blazing. I approached
this as passively as one possibly could, it was due to the efforts of the ScrapingHub suborg admin Paul who showed interst in my proposal, gave me an interview
and put me to work on a bug which also made me familiar with the actual inner workings of Scrapy that I was able to gain footing on the project.  

My project for this summer is going to deal with re-factoring the Scrapy signaling API, in an effort to move away from the PyDispatcher library which would greatly
enchance the performance of signals. Django moved away from PyDispatcher in 2001, and they reportedly observed an increase of upto 90% in efficiency. I intend to build
off their work and assume we would see similar results in Scrapy.  

The Scrapy community are a really active lot, and my "community bonding" started just a couple days into the announcement of me being selected for the project, which is good
because I've had exams for the past couple of weeks or so and was AFK for a major part of them(of course informing my mentors about the same first). I had a video chat with my
mentor Jakob where we figured out how reporting etc. would work for the summer, our next chat is scheduled for the 24^th, 25^th where we shall discuss how the actual implementation
of the project will be and what plans I have for the same. I also finalised the work on a bug I was working on and had submitted in the form of a patch as a part of my proposal.
The Scrapy community is really responsive, and I'm honored to be a part of it and to be working with all the people here. I hope my work is up to their standards at the end of this.
Since there is not much Technical content to write about at this point, with the coding period having not yet started so we'll keep this one short, I'll bore you with the
technical details in the next one. Thanks for reading, the next post will go up on Sunday the 28th. Signing off.

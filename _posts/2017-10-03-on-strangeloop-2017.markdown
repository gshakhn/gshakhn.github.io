---
title:  "On Strange Loop 2017 AKA Another Fun Conference"
date:   2017-10-03 19:45:00
---

I recently got back from Strange Loop 2017. I started going back in 2011 or 2012 and went every year except 2016. The first few years, I was way in over my head. Later on, I was still over my head, but less so. The talks can be a bit esoteric, but I've gotten better at picking talks that are semi-relevant to my work and that I can understand. Since I sort of understood most of the talks I went to, I figured I'd blog about the ones I could.

if you're interested in the videos or slides, you can find them [here](https://github.com/strangeloop/StrangeLoop2017/wiki/Coverage). I only have brief reactions below, so I encourage you to watch the source material.


## [REDUX: ARCHITECTING AND SCALING A NEW WEB APP AT THE NEW YORK TIMES](https://www.thestrangeloop.com/2017/redux-architecting-and-scaling-a-new-web-app-at-the-new-york-times.html)

I've been meaning to get back into a side project that uses React. This talk gave me some inspiration on how to use React + Redux. I'm using Scala.js for the project with [scalajs-react](https://github.com/japgolly/scalajs-react). I'm not sure how much of the Redux bit will be directly applicable, but the general concepts will be useful.

## [JUST-SO STORIES FOR AI: EXPLAINING BLACK-BOX PREDICTIONS](https://www.thestrangeloop.com/2017/just-so-stories-for-ai-explaining-black-box-predictions.html)

Stripe uses machine learning models to detect fraud. There are some simple models which a human can easily understand. The better models make decisions that are hard to explain. When a transaction is marked as fraud, it's useful to show the merchant why. Sam Ritchie explained on how they created post-hoc explanations for the model to make it more "trustworthy". Humans also make post-hoc explanations for their decisions, so making the computer do it isn't that different.

## [WHAT HAPPENED TO DISTRIBUTED PROGRAMMING LANGUAGES?](https://www.thestrangeloop.com/2017/what-happened-to-distributed-programming-languages.html)

Heather Miller went into some historical distributed programming languages. There were a few quotes about the distributed programming from a few decades ago that were just as applicable today. I also learned that while [Barbara Liskov](https://en.wikipedia.org/wiki/Barbara_Liskov) is famous for the [Liskov Substitution principle](https://en.wikipedia.org/wiki/Liskov_substitution_principle), she also worked on Argus, one of the earlier distributed languages.

CS is a relatively young field, but it's humbling to realize that the giants from a few decades ago were grappling with the same problems we are today. They came up with solutions that we are still using today.

## [THE VOLDEMORT EFFECT](https://speakerdeck.com/litonico/the-voldemort-effect)

Very often, I encounter some type level programming or some JVM/JIT optimizations that I don't understand. I refer to these as "magic". This talk reminded me that everything has an explanation and encouraged me to dig deeper into those cases so I can fully understand it.

## [BUILDING A MEMEX](https://github.com/strangeloop/StrangeLoop2017/blob/master/selected-talks/StrangeLoop2017%20-%20Building%20a%20Memex.key)

[One reason]({% post_url 2016-03-28-on-journaling %}) for this blog is for me to remember stuff. I've been trying to write a small project or two for private journaling/note-keeping and document organization. Sadly, my interest in such a project constantly waxes and wanes, so little progress has been done.

Andrew Louis's talk really inspired me. He's recording a ton of stuff about himself (texts he's sent, where he's been, what's he's reading, etc.) and making them easily searchable. I'm looking forward to an open source and/or self-hosted version. Meanwhile, it gives me some encouragement to work on my own side project.

## [RIOT MESSAGING SERVICE - MESSAGE PLAYERS LIKE A PRO](https://www.thestrangeloop.com/2017/riot-messaging-service---message-players-like-a-pro.html)

Michal Ptaszek discussed the architecture for sending messages to millions of players. This [blog post](https://engineering.riotgames.com/news/riot-messaging-service) covers it better than I could in a few paragraphs.

## [STOP RATE LIMITING! CAPACITY MANAGEMENT DONE RIGHT](https://www.thestrangeloop.com/2017/stop-rate-limiting-capacity-management-done-right.html)

Jon Moore introduced me to [Little's Law](https://en.wikipedia.org/wiki/Little%27s_law). Through a live demo, he showed why rate limiting on a client by client basis is suboptimal. It's better to limit on a more holistic level. If you have a single client, there is no reason to limit it to less than the entire system allows. Spread the rate limiting across all clients so the sum of the requests doesn't exceed system capacity.

## [WHY WE BUILT OUR OWN DISTRIBUTED COLUMN STORE](https://www.thestrangeloop.com/2017/why-we-built-our-own-distributed-column-store.html)

Sam Stokes went into how Honeycomb built their own data store sitting on top of Kafka and the file system. The queries at Honeycomb lend themselves well to a column oriented database. They based their system on the [Scuba system](http://db.disi.unitn.eu/pages/VLDBProgram/pdf/industry/p767-wiener.pdf) from Facebook. I've added that paper to my ever growing to-read list.

## [THE PERFORMANCE ENGINEER'S GUIDE TO JAVA HOTSPOT VIRTUAL MACHINE'S EXECUTION ENGINE](https://www.thestrangeloop.com/2017/the-performance-engineers-guide-to-java-hotspot-virtual-machines-execution-engine.html)

Monica Beckwith's presentation was very dense. She mentioned many things that the JVM does under the hood that I was unaware of. I didn't fully grok all the topics, but I'm definitely going to read up on the topics she mentioned.
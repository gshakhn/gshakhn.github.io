---
layout: post
title:  "On HackerRank AKA and now for something completely different"
date:   2016-02-07 16:40:00
---

A few months ago, I went to a recruiting event for my employer. As part of the event, the candidates participated in a programming competition. We interviewed one of the winners, and I remember being impressed with the problem and the solution the winner came up with. I don't recall the exact problem or solution, but it involved an interesting algorithm with an appropriate data structure. I had a hard time following the explanation, and that made me question why and do some reflection.

# ArrayList vs LinkedList

When I first graduated, I remember focusing on performance and debating whether to use an `ArrayList` or a `LinkedList` when adding a new field to a class. As I progressed in my career, I just settled on using `ArrayList` for everything since the mental computation involved in figuring out whether I needed a `LinkedList` instead wasn't worth it.

Usually the field would be on a Hibernate entity. Choosing `ArrayList` or `LinkedList` only affected the instantiation of new Hibernate entities. When loading from the database, Hibernate would ignore my carefully selected and pick some Persistent collection that implemented `List`.

If the field wasn't on a Hibernate entity, the choice would matter more, but only if the computation involved was focused more on what `LinkedList` prefers (inserts vs. random access). I don't recall coming across a case where it actually mattered. I don't know if it's the nature of the work I do or if I'm just bad at recognizing when I'm using a completely inappropriate data structure. I'm hoping it's the former and `ArrayList` is good enough for my use cases.

# Importance of algorithmic skills

When I was in school, I participated in a programming competition. Did reasonably well and had fun, but haven't done one since. Consequently, it feels like my algorithmic skills have atrophied. These days, I'm more concerned about:

1. Figuring out the requirements to a problem. This involves collaborating with my team and product owner to figure out scope.
2. Making sure the code is maintainable. The project I'm currently working on will hopefully be used for years, so it's important that a future developer can come along, understand the code, and expand it without putting in too much effort.

I enjoy the work I do, but there isn't as much thinking about algorithms / data structures for optimizing performance. There is work in figuring out the right algorithm to solve the business problem, but that's a different type of problem than performance. Performance can be a business requirement, but I haven't seen it stated as such frequently. I prefer a slow program that gets the right answer instead of a fast program that gets the wrong answer. Having both is awesome. I've found that  when the program is right and the algorithm isn't completely stupid, it's usually fast enough. Can it be made faster? Probably. Is it worth doing? Usually not. If it needs to be sped up, then the profiler comes out in order to find the bottlenecks.

Having said that, I still miss trying to find the right algorithm / data structure for the problem. One of my coworkers introduced me to HackerRank recently and I've been scratching the itch for the last few days. The problems are well written with exact requirements. The inputs are well defined, so you know ahead of time what the range for an inputted number will be. The code will be run once and never updated, so it doesn't have to be maintainable. The requirements don't change halfway through the problem. The only constraint is figuring out the right algorithm / data structures to use.

I realize that those types of requirements are unrealistic in the real world, but it's a bit refreshing to just crank out some code. Don't get me wrong. I value collaborating with my team to figure out the requirements for a project that will have to maintainable for years. But sometimes you want to do something completely different.

---
title:  "On note taking AKA my long term memory sucks"
date:   2017-03-25 13:00:00
---

### New Job Challenges

Starting a new job late last year has made me reflect on memory a bit. I was with my previous employer for 6+ years. Over those years, I had learned the technology, processes, who to go to for help on X, etc. I had worked on a many parts of the codebase, so I knew the foundations and could make deductive leaps. For example, when looking at a class new to me and trying to figure out the database structure, I knew where to find the database migrations and the common columns we used across tables.

Being put in a new environment shook those foundations. I can lean on some of my experiences from my previous job to help, but I'm working with new people on a new domain in new languages. In retrospect, that challenge is obvious, but I didn't realize the scope of it when I switched. The change made me recognize how much I've been semi-coasting on 6+ years of built up knowledge at my previous employer. I definitely appreciate the challenge in learning new things, but it's terrifying at the same time. I've been taking a bunch of notes trying to rebuild that foundation.

### Young me

Back in high school, I was a horrible note taker. For more logic based subjects, I would mostly pay attention in class to the fundamentals and try to retain those instead of worrying about details. If I remembered and understood the first principles, I could re-derive whatever was needed. I barely took any notes. [^1] For more fact based subjects like history, I would take pages of notes, cram for the test, and promptly forget most of the details.

In college, the main difference was having a laptop in class to type notes on instead. That made things much more legible. [^2] I was hoping to save the notes so I could reference them years later if needed. Unfortunately, briefly looking at Dropbox finds nothing. They might be in some backups or an old hard drive, but they're not obviously discoverable. A bunch of word documents stored locally isn't a great note retention strategy.

### Current me

#### Work notes

For notes related to work, I've setup a personal page on the internal wiki server to store notes. I'm journaling my daily activity. I primarily do this to make standup easier and it makes time recording at month end simpler.

Additionally, I have another page as a cheat sheet for commands. My memorization of arbitrary Linux commands is horrible. If I run a command every day, my fingers remember it. If I run it less often, I always Google the same question and go to the same links. For more complicated commands that aren't on a single Stack Overflow page, I stick it in the wiki so I don't have to go through the re-derivation process again. [^3] Why waste future me's time?

#### Personal notes

Looking through this blog history, I've realized that I'm primarily using it to keep notes for myself. Whether it's my thought processes on [why to journal]({% post_url 2016-03-28-on-journaling %}), or [how to use Wireshark and how my VPN is setup]({% post_url 2016-08-02-on-connecting-to-pokemon-go-on-a-vpn %})), I've been using the blog as a reference. Hopefully the combination of my personal machine, Dropbox, CrashPlan, GitHub, and GitLab will preserve these notes better than some random word docs on an old laptop.

I've done a bad job at maintaining the blog. Since the last post 7 months ago, I've redone how the blog works, rebuild my VPN server, and a bunch of other things for my side project. I'm mostly depending on shell history to remember what I did. Even if I backup my shell history, the context of why I ran a particular command will be lost. In the end, what I did doesn't really matter as much as why I did it. Writing down my thought process helps clarify it in the present and explains it to the future.

[^1]: For linear algebra senior year, my notes for a few weeks of work would fit on the back of the syllabus for that section. This partially was due to the fact that the we were allowed a single page of notes during the test

[^2]: I've never had good handwriting, and it's only gotten worse over the years since I'm mostly writing on a computer these days. If I have to write by hand for more than a few minutes, I've found that my hand starts to cramp.

[^3]: Writing this post made me realize that in school, I would be happy to re-derive equations from fundamentals. In comparison, re-deriving arbitrary Linux commands is frustrating. I keep going back to the man pages and Google to figure out all the arguments. I haven't spent enough time fully grokking each utility and the flags to make writing them easy.  Part of me wants to spent more time on this, but another part says it's a waste to remember arbitrary facts when Google exists.
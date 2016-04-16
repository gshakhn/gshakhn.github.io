---
title:  "On starting a blog AKA why am I doing this?"
date:   2016-01-28 23:30:00
excerpt: "So I finally decided to start a blog."
---

*2016-04-16 Edit: I've update the blog's look. See the [new post]({% post_url 2016-04-16-on-a-new-blog-look %}) for details.*

So I finally decided to start a blog. Been thinking about it for a while, but never actually got around to it. Finally got some motivation after starting to read [Soft Skills](https://www.manning.com/books/soft-skills). Still not sure what I'll focus on, but figured I should at least get something up.

Most of the below is going to be a disorganized mess, but I need to start with something. It's not going to be pretty in the beginning (or maybe ever?). The flow won't be great. There will be bad grammar or missing words. I'll try to remember to run spell check before I post though. Setting a goal for myself to write at least a blog post a week, but knowing myself, I'll abandon that goal in the pursuit of the next shiny (or just vegging out and watching Netflix or playing Diablo).

This blog post isn't necessarily directed at anyone. Mostly writing for myself, but if you somehow find this (via Google or on GitHub) and find something useful (even if it's just a laugh), awesome.

Topics I might cover may include:

* Random tech topics. Whenever I work on something interesting, I tend to forget about what I learned eventually. This might be a good place to write it down. The stuff I learn comes from somewhere, whether it's man pages, documentation, or someone elses blog post, so nothing here will be original. I'll try to credit sources when I remember to, assuming I remember what the sources even were.
* Random topics I find interesting. I'm a fan of personal finance (though /r/personalfinance isn't as good as it was a few years ago. Get off my lawn), so that might be a topic at some point.
* Random musings about life, the universe, and everything. A relatively broad topic. Probably focused on my philosophy on life, if I ever figure it out. Hopefully this blog will help with that. I tend to be a relatively private person, so putting random thoughts on the Internet is a bit weird. At some point I'll probably post something I'll regret. Then I'll delete it. It's not as if the Internet has a memory or I'm using some sort of source control system...


## Blog technical notes for those curious (and wanting to read this blog post for details instead of the other thousand "How to setup a blog guides")

There are plenty of blogging solutions out there. I chose to use Jekyll hosted on GitHub for a few reasons:

* One of my coworkers set up an internal blog using Jekyll. It was easy to write posts with, so I had a good experience with it.
* It was relatively straightforward to get everything setup. Took a bit longer than I expected, but wasn't a huge pain. Most of the pain was caused by myself second guessing the choice to use Jekyll and searching for other solutions instead of actually solving the problem.  See below for details on what the end result was.
* Free hosting on GitHub. I'm stingy, especially for something that I may abandon in the very near future. Plus it might be automatically indexed by Google, though it'll probably be low in the list. I'll need a more unique name. Need to write a [Googlewhack](https://en.wikipedia.org/wiki/Googlewhack). Until I come up with a better domain, <http://gshakhn.com> will work.

# Getting Jekyll setup

The [Jekyll](http://jekyllrb.com/) site has some documentation for getting Jekyll installed and running. However, the installation requires installing a gem. I haven't setup rvm or rbenv yet on my machine, and I didn't want to install the gem globally. Been using docker recently for stuff and having fun. A combination of the [Jekyll docker documentation](https://github.com/jekyll/docker/blob/master/README.md) and [another blog post](http://salizzar.net/2014/11/06/creating-a-github-jekyll-blog-using-docker/) led to the following commands (using [fish](http://fishshell.com/), so you may have to adapt slightly for bash).

    mkdir gshakhn.github.io
    cd gshakhn.github.io
    docker run --rm --label=jekyll --volume=(pwd):/srv/jekyll -it -p 192.168.99.100:4000:4000 jekyll/jekyll:pages ruby -S jekyll new . --force
    docker run --rm --label=jekyll --volume=(pwd):/srv/jekyll -it -p 192.168.99.100:4000:4000 -e POLLING=true jekyll/jekyll:pages

192.168.99.100 is the IP address of the VM docker-machine created. Yours might be different.

# Getting GitHub Pages setup

The [GitHub Pages](https://pages.github.com/) documentation is relatively straightforward, much better than this, and more up to date. TLDR that is probably outdated.

1. Create a repo called yourusername.github.io.
2. Push the Jekyll code from above to it.

One thing that is missing from the above site is the `.gitignore` file. I just copied the [official GitHub one](https://github.com/github/gitignore/blob/master/Jekyll.gitignore).

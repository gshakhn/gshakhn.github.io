---
title:  "On running X11 apps in Docker on OS X via Xephyr AKA spending an evening on a project that will go out of date within a month"
date:   2016-05-23 22:10:00
---

TLDR: I'm running X11 apps in Docker on docker-machine and seeing the windows in an Xephyr window running in XQuartz. Crazy? Maybe. Will it be obsolete once Docker on Mac comes out? We'll see. Fun side project? Yep!

# Background

In [ScalaJS and browser testing]({% post_url 2016-04-16-on-running-scalajs-tests-in-a-browser %}), I mentioned running tests headlessly on OS X. I was able to get this working ([repo](https://github.com/gshakhn/sbt-firefox-chromium) has some documentation, but I still need to blog about it.) Running things headlessly works when tests are doing what you expect. When things go bad though, I have to use a browser in a real X11 session so I can debug JS with a UI.

At work, we run automated tests against Firefox ESR. I prefer to use the latest Firefox for regular browsing. That requires having two versions of Firefox installed. I avoid that problem by testing against the latest Firefox locally, but that means that my local testing doesn't match the CI server. That's usually fine, but when it's not, it's really annoying.

In an ideal world, I would have a single version of the browser running in Docker headlessly, but with an option to run in a real X11 session, both on Mac and on Linux. Got bored one evening and decided to hack it out.

# The actual guide

## Prereqs

You need to have XQuartz and Xephyr installed. `brew cask install xquartz` should install both.

You also need a Dockerfile that has the X11 app you want to run. The following example runs Firefox, but your needs will probably vary.

    FROM alpine
    ENV FIREFOX_VERSION=38.3.0-r1
    RUN apk --no-cache add \
      bash \
      dbus \
      firefox=$FIREFOX_VERSION \
      xvfb
    CMD /usr/bin/firefox

You can build the above image by running `docker build -t docker-firefox .` while in a folder that has the above Dockerfile.

## Commands to run

    Xephyr :1 -ac -screen 800x600 &
    docker run -e DISPLAY=192.168.99.1:1 -t docker-firefox

# Sources

This [blog post](http://fabiorehm.com/blog/2014/09/11/running-gui-apps-with-docker/) and this [GitHub issue](https://github.com/docker/docker/issues/8710) gave me a bunch of background.

# Caveats

* This might change when Docker for Mac comes out. When running apps in Docker in the regular X11 session, you need to share the X11 socket with the Docker container. That was a bit difficult with Virtualbox in the way too, so I worked around it with Xephyr. Not sure if Xephyr will work with Docker on Mac, though I'm guessing it will. Running things in Xephyr may not be desirable though if we can just access the main X11 session.
* A [Stack Overflow answer](http://stackoverflow.com/questions/16296753/can-you-run-gui-apps-in-a-docker-container) mentioned that running X11 apps in docker on the main X session bypasses Docker sandboxing. I'm unsure if using Xephyr exposes the same risks. Alternatives are running X over SSH or VNCing into an Xvfb session running in the Docker container.



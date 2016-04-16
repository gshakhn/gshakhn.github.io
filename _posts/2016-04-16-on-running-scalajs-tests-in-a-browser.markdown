---
title:  "On running ScalaJS tests in a browser AKA hacking sbt until it does what I want (I think)"
date:   2016-04-16 10:20:00
---

TLDR: I'm running ScalaJS tests in Firefox/Chrome due to a PhantomJS [bug](https://github.com/scala-js/scala-js/issues/1555). The relevant code is in [these](https://github.com/gshakhn/private-wiki/commit/7a23f660d3eefb1d32013cbb5d32f2bcb8c89abe) [three](https://github.com/gshakhn/private-wiki/commit/06c5b8499f975f2aabe2e27f562241dab06503d5) [commits](https://github.com/gshakhn/private-wiki/commit/a309deb289c67af6d14b9790b95199e8c0cbdc59).

# Background

I found some motivation/energy to work on [private-wiki](https://github.com/gshakhn/private-wiki) recently. Part of that project requires having some sort of text editor. I decided to play around with [react-quill](https://github.com/zenoamaro/react-quill). After spending some time getting it setup in build.sbt and creating a [scalajs-react](https://github.com/japgolly/scalajs-react/) wrapper (future blog post?), I was able to see the editor in the UI. Unfortunately, there was an [error](https://github.com/zenoamaro/react-quill/pull/73) in the browser console. The issue has been fixed, but it's not in the latest release (0.2.1). Annoying, but UI seems to work, so not a huge deal. Except that logging a console error in PhantomJS [blows up](https://github.com/scala-js/scala-js/issues/1555). That means I can't run tests in PhantomJS.

After reading the issue and the couple of issues linked to it, I decided to run the tests in a real browser instead. [japgolly](https://github.com/japgolly) has put together an [awesome tutorial](http://japgolly.blogspot.com/2016/03/scalajs-firefox-chrome-sbt.html) on how to do this. His sbt files lets you do `firefox:test`, which runs the tests in Firefox.  However, I'm not a huge fan of copying every single config key from the `Test` config. If there is a new one added at some point, I probably won't notice it. My goal was to try to copy the `Test` config as much as possible without actually copying every single key manually. After hacking at sbt for a bit, I came up with the solution below.

# Code

**Warning**: I had to try a few different variations in sbt before I got something that worked. I tried a few that I thought would have worked, but didn't. I think the solution I came up with works, but I obviously don't understand sbt, so I may be missing something fundamental. If you know sbt and think the following is wrong, please let me know.

I added [these](https://github.com/gshakhn/private-wiki/commit/7a23f660d3eefb1d32013cbb5d32f2bcb8c89abe) [three](https://github.com/gshakhn/private-wiki/commit/06c5b8499f975f2aabe2e27f562241dab06503d5) [commits](https://github.com/gshakhn/private-wiki/commit/a309deb289c67af6d14b9790b95199e8c0cbdc59) to my project. In blog form:

Add the following to `plugins.sbt`:

    libraryDependencies += "org.scala-js" %% "scalajs-env-selenium" % "0.1.2"

In `build.sbt`, add the following:

    lazy val FirefoxTest = config("firefox") extend Test
    lazy val ChromeTest = config("chrome") extend Test

In the client project, add:

    client
      .configs(FirefoxTest, ChromeTest)
      .settings( inConfig(FirefoxTest)(Defaults.testTasks) : _*)
      .settings( inConfig(FirefoxTest)(ScalaJSPluginInternal.scalaJSTestSettings) : _*)
      .settings( inConfig(ChromeTest)(Defaults.testTasks) : _*)
      .settings( inConfig(ChromeTest)(ScalaJSPluginInternal.scalaJSTestSettings) : _*)
      .settings( inConfig(FirefoxTest)(
        jsEnv := new org.scalajs.jsenv.selenium.SeleniumJSEnv(org.scalajs.jsenv.selenium.Firefox)))
      .settings( inConfig(ChromeTest)(
        jsEnv := new org.scalajs.jsenv.selenium.SeleniumJSEnv(org.scalajs.jsenv.selenium.Chrome)))

If you're using Travis CI, add the following to `.travis.yml`:

    before_script:
      - "export DISPLAY=:99.0"
      - "sh -e /etc/init.d/xvfb start"
      - sleep 3 # give xvfb some time to start

Make sure to prefix the client tests with `firefox` in `script`. You'll get something like:

    script:
      - sbt ++$TRAVIS_SCALA_VERSION 'clean' 'set scalaJSStage in Global := FastOptStage' "client/firefox:test"
      - sbt ++$TRAVIS_SCALA_VERSION 'clean' 'set scalaJSStage in Global := FullOptStage' "client/firefox:test"
      - sbt ++$TRAVIS_SCALA_VERSION "server/test"

# Caveats

There are a few problems with the above. In no particular order:

* I'm not sure how much time Travis should be sleeping after starting Xvfb. I got the 3 seconds from their [docs](https://docs.travis-ci.com/user/gui-and-headless-browsers/). A shorter time might work, but 3 seconds isn't too much compared to how long the build takes, so I'm not too worried.
* Running tests in Firefox is slower than PhantomJS. I haven't actually measured it, but it felt noticeably slower. That may be because I couldn't do anything else while Firefox was starting up and taking over the screen.
* Can't run headless tests on Macs. japgolly mentions using Xvfb for headless testing. I've used it at work on Linux successfully as well. At home, I'm working on a Mac, and firefox-x11 is [unavailable in brew](https://github.com/Homebrew/legacy-homebrew/issues/9150). It was available in macports for a bit, but it's [no longer maintained](https://trac.macports.org/ticket/42087). Having Firefox keep flashing on my screen is going to get annoying, so I'll have to pursue some options. Main idea so far is to run the tests in Docker. If I get that working, I'll write another blog post.
* No Chrome in Travis CI. I was going to do this, but then noticed [this](https://github.com/scala-js/scala-js-env-selenium/issues/39) issue in scala-js-env-selenium. I'd like to run tests in Chrome in Travis CI at some point though. Maybe use Docker for that too?
* Failing tests don't actually fail. I wrote a failing test, but ScalaTest didn't report the error to sbt, so the build passed. For private-wiki, this only happened in Chrome. When I tried to [minimally reproduce](https://github.com/gshakhn/scalajs-selenium-scalatest-failure) it, it failed to fail in both Firefox and Chrome. Bizarre and a bit worrying.

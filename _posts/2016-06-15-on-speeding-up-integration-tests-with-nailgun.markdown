---
title:  "On speeding up integration tests with Nailgun AKA waiting ~15 seconds for a test annoys me"
date:   2016-06-15 07:45:00
---

TLDR: Using [Nailgun](http://www.martiansoftware.com/nailgun/) can speed up integration level tests, eliminating the need for fake swords.

# Background

At work, we use [FitNesse](http://fitnesse.org/) combined with [HSQLDB](http://hsqldb.org/) to do integration level testing. For those unfamiliar with FitNesse, it's a wiki/table based acceptance testing framework. You write your tests in a table format that the FitNesse wiki server parses. It then calls fixture methods in your code. An example from their [docs](http://fitnesse.org/FitNesse.UserGuide.WritingAcceptanceTests.SliM.ScriptTable):

    |script             |login dialog driver|Bob           |xyzzy|
    |login with username|Bob                |and password  |xyzzy|
    |check              |login message      |Bob logged in.      |

The above table will create an instance of the `LoginDialogDriver` class and call the `loginWithUsernameAndPassword` and `loginMessage` methods.

The way it works internally is there is one JVM hosting the wiki, allowing you to edit it. Whenever you run the test, it spawns another JVM to actually run the test in. The second JVM has the fixtures and your production code.

# Pain Point

For us, the first thing the second JVM does is "start up" the application with an embedded HSQLDB. Among other things, this involves parsing the Hibernate configuration and creating the appropriate tables. Unfortunately, this takes ~15 seconds. That makes running tests extremely slow and annoying. I'm always tempted to get some fake swords to pass the time.

One of my coworkers sped this up a ton by reusing the same DB + Hibernate configuration between tests, so subsequent tests take ~2 seconds. This means that when you're running a suite (multiple tests), the first test will take ~15 seconds, but the rest will take ~2 seconds each. That's awesome for a CI environment, but it doesn't help much in a local environment. When you run a test, edit it, and run it again, you're paying that ~15 second cost each time you run the test.

# Solution

If only we could reuse the above optimization across multiple test runs. We would need some sort of persistent JVM to cache the DB + Hibernate configuration in. That's where Nailgun comes in. The Nailgun server can run in the background, and the FitNesse server just needs to call the Nailgun client instead of spawning a second JVM when running tests. The Nailgun JVM will cache things so that subsequent test runs can be speedy.

Steps:

1. Install Nailgun. Make sure that `ng` or `ng-nailgun` is on your `$PATH`.

2. Figure out the classpath that you run FitNesse tests in. The classpath is computed dynamically based on the `!path` statements in the test. We can convert the `!path` statements into a java classpath argument by running the following:

        grep "!path" content.txt | awk '{ print $2 }' | sed 's/\.jar//g' | tr '\n' ':'

3. Start the Nailgun server with the above classpath + the Nailgun server jar.

        java -cp CLASSPATH_FROM_ABOVE com.martiansoftware.nailgun.NGServer

4. Start the FitNesse wiki server.

5. Modify the `COMMAND_PATTERN` FitNesse uses to start the second JVM. Inside the main content.txt, there will be something similar to: 

        java -cp %p %m 

    According to the [docs](http://www.fitnesse.org/FitNesse.UserGuide.WritingAcceptanceTests.CustomizingTestExecution), `%p` is the classpath and %m is the main class. We've already figured out the classpath and started the Nailgun server with it, so the important thing is `%m`. Change the above line to:

        ng %m

6. Run the test. The first run will be a bit slow since nothing is cached yet.

7. Run the test again. Speedup!

# Caveats

* This will only speed up tests if you're sharing some cached state between test runs. If you're creating a brand new DB for each test, this won't give you much improvement. It might speed things up slightly due to not having to start a JVM each time, but that tends to be unnoticeable in comparison to DB creation.
* If you've already modified the `COMMAND_PATTERN` for your particular application (e.g. more memory usage), you'll need to apply the same changes to the Nailgun server start command.
* If you want to debug the FitNesse test, you'll have to add the JVM debug arguments to the Nailgun server start command. `REMOTE_DEBUG_COMMAND` is no longer needed and you don't have to click Debug in the wiki.
* If you tend to run multiple tests concurrently, this probably won't work. In regular FitNesse, if you run 2 tests concurrently, each will spawn a separate JVM and shouldn't interfere with each other (unless you have a shared resource (e.g. external DB)). Since you're in the same JVM with Nailgun, depending on how your app is structured, tests might fight for the same in memory resource. So running tests concurrently might blow up in your face. On the bright side, since the tests are faster now, you may have less of a need to run them concurrently.
* If you run the same test over and over, it might actually get faster due to JIT magic. I've noticed that tests get faster and faster if I keep running them inside the same Nailgun JVM. I haven't confirmed that JIT is the reason, but it's a plausible hypothesis.

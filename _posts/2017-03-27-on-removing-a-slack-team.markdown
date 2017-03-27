---
title:  "On removing a slack team you're no longer part of AKA Slack App Catch-22"
date:   2017-03-27 07:30:00
---

Imagine your account was removed from a Slack team. That team still has an icon in the app though. It's useless to you now, so you want to remove it. You right click and remove the team, but to your surprise, it reappears when you reopen the app. Based on some [comments online](http://www.9giantsteps.com/2016/02/04/how-to-remove-slack-teams-from-the-mac-slack-app-sidebar/), it looks like you have to be logged in to a team you're no longer part of in order to remove the team from the app. That's a fun puzzle.
 
Luckily, there is a workaround by mucking around in the Slack preferences.

1. Close the Slack app.
2. `cd` into `~/Library` and `grep` for the team name. You'll find it in a few places, but `~/Library/Containers/com.tinyspeck.slackmacgap/Data/Library/Application Support/Slack/storage/slack-teams` looks promising.
3. View that file to verify that the team is there. If you can read unformatted JSON easily, I'm impressed. If you're like me and can't, use `jq` or your tool of choice to format it.
4. Remove the node that has the team from that file with your editor of choice.
5. Reopen Slack app.
6. No longer see the team! Catch-22 bypassed.
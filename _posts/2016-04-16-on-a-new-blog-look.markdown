---
title:  "On a new blog look AKA I'm easily distracted"
date:   2016-04-16 15:20:00
---

After posting about [ScalaJS and browser testing]({% post_url 2016-04-16-on-running-scalajs-tests-in-a-browser %}), I realized that getting feedback on the code snippets would be useful. There was no way to obviously contact me on the blog, other than a Twitter message. Anyone looking at the post wouldn't be able to see the responses easily. This problem must have been solved before. Comments!

After doing some googling, Disqus seemed like a reasonable option. The [criticisms on Wikipedia](https://en.wikipedia.org/wiki/Disqus#Criticism_and_privacy_concerns) are a bit scary from a privacy point of view, but it definitely was the easiest option. Of course, I'm going to be tied to vendor this way. Vendor lock-in is so easy when the user isn't interested in putting more work into an alternative.  We'll see if that bytes me eventually.

While googling about Jekyll + GitHub Pages + Comments, I stumbled upon [minimal-mistakes](https://mmistakes.github.io/minimal-mistakes/), which is a Jekyll template with built in Disqus commenting. I could spend a few minutes following the [official Disqus instructions](https://help.disqus.com/customer/portal/articles/472138-jekyll-installation-instructions) to add comments to the existing blog, or I could spend an hour changing the blog's look. Guess what I picked.

Switching over to use the new style took a bit of effort. The steps I took were:

1. Clone the old blog's repo.
2. Add the [minimal-mistakes repo](https://github.com/mmistakes/minimal-mistakes) as another origin.
3. Create a new branch based on minimal-mistakes's `master`.
4. Make sure I can run the blog locally. See below for some problems I hit.
5. Modify the config with my info.
6. Get rid of the default navigation links in `_data/navigation.yml`. They were pointing to  minimal-mistaks's docs.
7. [`git cherry-pick`](http://stackoverflow.com/questions/1670970/how-to-cherry-pick-multiple-commits) all of the post commits from the old blog.
8. Go through some migration problems. See below.
9. Delete the old `master` branch.
10. Cut a new `master` branch based on the new blog post.
11. Force push to GitHub. Force pushes are a bit scary, but if you want to rewrite history, it's the only way. If you cloned this blog's repo before and the force push destroyed your local checkout, sorry. If you had cloned this repo though, I'm curious on why. Let me know in the comments! :smile:

# Problems

1. **Docker**. I had some trouble using the docker command for Jekyll from the [first]({% post_url 2016-01-28-on-starting-a-blog %}) post. After some frustration, I realized that the minimal-mistakes is using a Gemfile The Jekyll docker image had its own set of gems. I had to change the docker command to:

        docker run --rm --label=jekyll --volume=(pwd):/srv/jekyll -it -p 192.168.99.100:4000:4000 -e POLLING=true jekyll/jekyll:pages bundle exec jekyll serve

2. **The layouts were different**. The old Jekyll blog used the `post` layout, while minimal-mistakes uses `single`. Needed to update the posts to use the new layout. The minimal-mistakes docs mentioned using [Front Matter defaults](https://jekyllrb.com/docs/configuration/#front-matter-defaults), so I just got rid of the `layout` setting in every post and specified it in the defaults.

3. **Permalinks**. The old blog used the [`date`](https://jekyllrb.com/docs/permalinks/) permalink style, while minimal-mistakes had another. In order to keep the old links working, I changed it back to `date`

4. **Slower**. It feels like Jekyll is a bit slower to auto-regenerate blog posts on change. Haven't timed it, but it seems noticeable. Annoying, but I'll put up with it.

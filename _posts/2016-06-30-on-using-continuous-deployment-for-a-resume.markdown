---
title:  "On setting up continuous deployment for a resume AKA spending a few hours automating a task I spend 5 minutes a year on"
date:   2016-06-30 08:00:00
---

TLDR: I setup continuous deployment for my resume so it auto builds and auto publishes online via GitLab CI. The code is [here](https://gitlab.com/gshakhn/resume/blob/master/.gitlab-ci.yml) and [here](https://gitlab.com/gshakhn/gshakhn.gitlab.io/blob/master/.gitlab-ci.yml).

# Background

I recently switched over this blog to be hosted by GitLab Pages instead of GitHub Pages. Main reason was to implement SSL on a custom domain. You should be seeing this blog at https://www.gshakhn.com if everything went well. Was going to blog about that change, but I mostly just followed [this tutorial](https://about.gitlab.com/2016/04/11/tutorial-securing-your-gitlab-pages-with-tls-and-letsencrypt/).

The switch over to GitLab Pages made me play with GitLab CI a bit. I've been primarily using GitHub and Travis CI for other personal projects, so I was curious to see how GitLab CI worked. I had mentioned to a coworker that I used LaTeX in Docker to build my resume, and he had joked about auto building it with GitLab CI. Building it is super easy manually and I don't do it that often, so it's an obvious candidate for automation. That or I just wanted to play with GitLab CI a bit more.

In the end, I got GitLab CI setup to build my resume and auto-publish it to this blog.

# Code

## Resume project

The `.gitlab-ci.yml` file for the [resume project](https://gitlab.com/gshakhn/resume):

    stages:
     - deploy
     - trigger-other-builds
    
    pages:
      stage: deploy
      image: schickling/latex
      script:
        - mkdir public
        - pdflatex -output-directory public resume.tex
      artifacts:
        paths:
          - public
      only:
         - master
    
    trigger-gshakhn-com:
      stage: trigger-other-builds
      script: 
        - curl -X POST -F token=$GSHAKHN_TOKEN -F ref=master https://gitlab.com/api/v3/projects/$GSHAKHN_PROJECT_ID/trigger/builds

The `pages` job just processes the LaTeX file and generates a PDF. It uses the `schickling/latex` image so I don't have to install LaTeX myself. (It does take a while for GitLab CI to download the image though.) The resulting PDF is then hosted on the project's GitLab Pages.

The `trigger-gshakhn-com` job uses [GitLab Triggers](http://docs.gitlab.com/ce/ci/triggers/README.html) to trigger a build of the blog. `GSHAKHN_TOKEN` and `GSHAKHN_PROJECT_ID` are [Secure Variables](http://docs.gitlab.com/ee/ci/variables/README.html) I setup in the resume project to reference the Trigger in the blog project. This creates a pipeline so that whenever the resume gets updated, the blog gets updated as well.

## Blog project

The `.gitlab-ci.yml` file for the [blog project](https://gitlab.com/gshakhn/gshakhn.gitlab.io):

    pages:
      stage: deploy
      image: jekyll/jekyll:pages
       script:
         - wget http://gshakhn.gitlab.io/resume/resume.pdf
         - bundle exec jekyll build -d public/
       artifacts:
         paths:
           - public
      only:
         - master

It's pretty much the same as the GitLab tutorial, except:

1. I use the `jekyll/jekyll:pages` image. It already has a bunch of gems pre-installed.

2. I use `wget` to grab the latest version of my resume. Jekyll then packages the file with the rest of the blog.

---
title: "Continuous Deployment - Using Travis-CI to Build GitHub Pages"
date: 2018-02-20T10:24:14+01:00
tags: ["Travis-CI", "DevOps", "CI/CD", "HUGO", "Deployment", ]
draft: false
---

A static site generator (i.e HUGO) is a tool that renders content into html, css and javascript. The content is written in markdown (or html) and contains some metadata like dates, title or tags, categories, etc.

In theory, the workflow to make a new post is quite simple, just throw in a new markdown file in the right place, rebuild your site, and deploy it to your server. But deploying the static site could be a hell. We can do better.

We'll use Travis CI to build our site and deploy it afterwards to the master branch of our repository.
To allow Travis CI access to our repository we need to [create a personal access token](https://help.github.com/articles/creating-a-personal-access-token-for-the-command-line/), which is used in the [repository settings](https://docs.travis-ci.com/user/environment-variables#Defining-Variables-in-Repository-Settings).

The following travis.yml file works fine with Hugo.:

```
language: go

# use the latest version of golang
go:
  - master

# use the latest version of hugo
install:
  - go get github.com/spf13/hugo

# build your website on travis
script:
  - hugo

# deploy your static files to GitHub Pages after a successful build.
deploy:
  # set default static site output dir for hugo
  local_dir: public
  # set the slug of the repo you want to deploy your site to i.e.
  repo: offtechnologies/offtechnologies.github.io
  # github pages branch to deploy to
  target_branch: master
  provider: pages
  skip-cleanup: true
  # You will need to provide a personal access token
  github-token: $GITHUB_TOKEN
  keep-history: true
  on:
    branch: master

```
Commit and push our changes to GitHub:
```
git add .travis.yaml
git commit -m "add Travis CI config"
git push -u origin master
```
After a while Travis CI should recognise that there was a recent push to the repository and it should start to build your site with Hugo and deploy it afterwards. This should now happen each time you make changes to your repository at GitHub.


Credits:

* [Travis CI](https://docs.travis-ci.com/user/deployment/pages/)
* [Mario Martelli](http://schnuddelhuddel.de/deployment-of-a-hugo-site-on-github-pages/)
* [Martin Kaptein](https://www.martinkaptein.com/blog/hugo-with-travis-ci-on-gh-pages/)

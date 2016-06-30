---
title:  "Starting my Jekyll Website!"
header:
  teaser: "https://farm5.staticflickr.com/4076/4940499208_b79b77fb0a_z.jpg"
tags:
  - project_notebook
---

I am forking [mmistakes/minimal-mistakes][gh1] to have a prebuilt website theme
that I can put my content in.

## Procedure to setup repo

1. I forked the repo on the github website.
2. Cloned into mm_site
3. Moved existing gh-site to new branch.

```bash
git clone https://github.com/NGenetzky/minimal-mistakes.git mm_site
cd mm_site/
git checkout -b gh-pages origin/gh-pages
git branch mmistakes-pages
```

## Idea to keep website separate from content

I would like to keep the website in a repository separate from the repository
that contains the content. I would like to use git submodules to do this.
[@jryio][jryio] wrote a [post][jry] about this. By keeping them separated
I can hopefully keep from getting locked into a particular theme or tool to
display my content.


[gh1]: https://github.com/mmistakes/minimal-mistakes
[jryio]: https://twitter.com/jryio
[jry]: http://jry.io/posts/make-your-jekyll-blog-awesome-with-git-submodules/
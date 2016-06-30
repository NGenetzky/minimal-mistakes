---
layout: single
title:  "Particle"
header:
  teaser: "unsplash-gallery-image-2-th.jpg"
  <!--teaser: "https://farm5.staticflickr.com/4076/4940499208_b79b77fb0a_z.jpg"-->
tags:
  - tags
  - project_notebook
date:   2014-11-22
published: false
---

# Particle

## Preparing a repository for github (remove auth tokens)

I need to put a respository online that contains sensitive information. I looked
at git-filter methods of removing this content. While that may work in the future,
I have created a complete solution in bash that will allow me to do it for now.
I am working on a bash script to do this but have decided to put it off for now.

## Trying to get voice commands from my phone to Particle

Successfully works if I do the filtering on the phone side. If I send the comamnd
straight through from the voice regonition to the Particle, then it doesn't work
very well. I am going to try to have my phone post a Particle.Publish instead;
the hope is then I could use a python app or something to watch the stream and 
interrpret it.

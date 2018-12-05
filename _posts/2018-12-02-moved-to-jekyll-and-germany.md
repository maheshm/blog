---
categories: [blog]
layout: post
title: Moved away from Drupal to Jekyll, a new change in Germany!
---

It was started quite some time ago. I wanted to move away from Drupal. Had tried to move to newer versions of Drupal, but it was a maintenance nightmare for me. Idea to move to Wordpress was also no different. At Cybrilla the [TILs](https://borgs.cybrilla.com/tils/) were in Jekyll, but it was after embracing Emacs completely that I felt the urge to move further away from CMS.

As will all of my ventures this also was shelved, reconsidered, worked on for sometime, and went in that loop several times before I got fed up and took an entire weekend to finish it off finally. Now my blogs are just a commit away! And without moving even a bit outside Emacs!

First road block was in theme selection which was rather a mental block or commitment issue to stick on with one! Once that was fixed I had a much easier task of importing all the data from Drupal that proved to be easier that I thought. A quick tip - the type of nodes in Drupal 6 is 'blog', 'story' and 'page'. I had to do DB look up to find this and was not so in the Jekyll documentation for migration from Drupal 6. Then I did a few restructuring to suite the flow of the content. Fixing some of the UI stuff was a PIA as the theme was not using any CSS framework. I will migrate it to `milligram` soon. The last task was ensuring smooth deployment. Git hook gave a bit of a trouble by not finding the `bundle`. I have always had trouble with `rvm` and scripts trying to find it. In this case all I had to do was fix the she-bang with a login bash:

`#!/bin/bash -l`

Took the blog live by around 0215 Hrs CEST with a simple `git push deploy master`. That felt really good!

Now the even better part - deploying from Emacs. This post should come directly from Emacs. This was long pending indeed!

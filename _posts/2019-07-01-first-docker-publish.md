---
categories: [tech-blog]
layout: post
title: Fist Docker publish
---

I was trying to do some memory profiling of a Rails app and wanted to patch the Ruby MRI as mentioned (here)[https://github.com/skaes/rvm-patchsets]. As the app was already on Docker it was kind of easier to have an image that I could use directly but unfortunately that was not there. I created and pushed my first (Docker image)[https://hub.docker.com/r/maheshmukundan/railsexpress]. I will try to add more Ruby versions than 2.6.2 and keep supporting that. I am a bit confused with how to maintain it in the repo, but have pushed it (here)[https://github.com/maheshm/railsexpress]

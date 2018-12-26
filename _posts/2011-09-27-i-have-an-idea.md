---
categories: [tech-blog]
layout: post
title: I have an idea!
created: 1317126936
---
After a long time I see space to contribute back to the community. Not that I didnt have anything to contribute in past, but none were related to what I was working. Now there seem to be some space for me to contribute.

Sunspot is a thing that can be used to talk to your lucene indices via Solr. And Resque is a thing that allows you to create background ruby process assosiated to your rails app. Now there is a need for background processing, so resque has to be there anyway. So when there needs to be a reindexing.. Ok, wait. Sunspot uses only one command for reindexing. So my idea is not valid. I just thought we can do reindexing as a background process. But that is handled by Solr. S.H.I.T! :(

Also thinking of creating a FB like thing using Diaspora for employees' to update about what they are doing right now. People will be more interested to update their status than sending a mail or using stuff like basecamp or Mantis!! I got about resque from there. Redis seems like an easier thing than memcached. Dont know about the performance thing. But i dont think that matters, for I was thinking to use it for storing the listids after sorting.

Oh! Almost forgot. I also have this idea of using a function that would tell us whether a particular record can be matched against the query or not, rather than just using a Boolean comparison. Dont know if its possible for an RPC or something of that sort! May be a simple interprocess request is also fine. But i have Java on one side to play with. Yes, I am talking about lucene itself! :(

Already lost interest on these ideas! Hmm..

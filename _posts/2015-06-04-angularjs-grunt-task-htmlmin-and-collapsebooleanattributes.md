---
excerpt: "I wanted to insert all the partials and views as a template for my Angular
  app. Luckily there is a Grunt task that does this. A small thing I wanted to do
  before this was making the HTML small. Remove the whitespaces, remove comments etc.
  htmlmin is exactly for this. I got a configuration for this:\r\n<code>\r\nhtmlmin:
  {\r\n      dist: {\r\n        options: {\r\n          collapseWhitespace: true,\r\n
  \         conservativeCollapse: true,\r\n          collapseBooleanAttributes: true,\r\n
  \         removeCommentsFromCDATA: true,\r\n          removeOptionalTags: true\r\n
  \       },\r\n        files: [{\r"
categories: [tech-blog]
layout: post
title: AngularJS - grunt task htmlmin and collapseBooleanAttributes
created: 1433398190
---
I wanted to insert all the partials and views as a template for my Angular app. Luckily there is a Grunt task that does this. A small thing I wanted to do before this was making the HTML small. Remove the whitespaces, remove comments etc. htmlmin is exactly for this. I got a configuration for this:
<code>
htmlmin: {
      dist: {
        options: {
          collapseWhitespace: true,
          conservativeCollapse: true,
          collapseBooleanAttributes: true,
          removeCommentsFromCDATA: true,
          removeOptionalTags: true
        },
        files: [{
          expand: true,
          cwd: '<%= yeoman.app %>',
          src: ['**/*.html','*.html'],
          dest: '<%= yeoman.dist %>/.tmp/html/app'
        }]
      }
    },
</code>
Funny thing that happened with this was that I had a directive called date-picker that had an attribute called default which accepts the default string for the drop down selection. When I ran this script the value that was being passed via default was getting truncated. After doubting few other tasks, I finally zeroed down the problem to htmlmin task. It was the option collapseBooleanAttributes: true that created the problem. default accepts boolean value in the unextended HTML world and this was optimised my htmlmin.

So ensure that if you are using directives and you are over-riding default HTML attributes, htmlmin might give you surprises.

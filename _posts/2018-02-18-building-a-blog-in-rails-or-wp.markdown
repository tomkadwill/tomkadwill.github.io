---
layout:     post
title:      "Building a blog in Rails or WordPress"
date:       2018-02-18 15:41:00 +0000
tags:       Rails WordPress
---

Recently I was tasked with adding a blog to an existing Ruby on Rails project. I decided that it should be a separate code base because there was no need to communicate with the main Rails codebase. I also decided to write the application using Ruby on Rails. My reasons were:
* I did not want to introduce a new technology because I thought the cost would out-weight the benefits
* The blog needed an API for the iOS and Andriod apps to access. I know how to build APIs in Rails.

Over time, I've come to think that I made the wrong decision. WordPress, the popular blogging platform, is build for this type of project. Since v4.7 it has supported REST APIs. To access the API of a WordPress blog, append `/wp-json/wp/v2/` and then the API name. For example. to get all the posts on Tim Ferriss' blog, the url would be [https://tim.blog/wp-json/wp/v2/posts](https://tim.blog/wp-json/wp/v2/posts).

The drawback is that the company would have to support another tech stack. This would cost the company time. However, I think this would be offset by all the features WordPress includes, out of the box. In addition, there are more WordPress developers than Ruby developers. Hiring WordPress developers, to support the application, would be easier than hiring Ruby devs.

### TL;DR

Next time you think about building a blog in Ruby on Rails, use WordPress instead. Even if your blog requires an API.

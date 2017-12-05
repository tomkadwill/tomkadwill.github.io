---
layout: post
title:  "Debugging performance issues with the bullet gem"
date:   2017-12-04 21:06:00 +0000 #TODO
---

The [bullet](https://github.com/flyerhzm/bullet) gem helps you increase your application's performance by reducing the number of queries it makes. Bullet helps with stuff like: removing N+1 queries, unnecessary eager loading and when you should be using counter cache.

There are already guides on installing and basic debugging, using bullet. Some examples are:

* [Bullet's README](https://github.com/flyerhzm/bullet)
* [Railscasts Episode](http://railscasts.com/episodes/372-bullet)
* [Semaphoreci's blog](https://semaphoreci.com/blog/2017/08/09/faster-rails-eliminating-n-plus-one-queries.html)

---
layout:     post
title:      "Benefits of Switching from Unicorn to Puma"
date:       2018-11-11 12:00:00 +0000
permalink:  'benefits-of-switching-from-unicorn-to-puma'
---

Recently I worked on an application that made many external API calls. The external APIs were slow and they were bringing
the site to it's knees. 500 timeout errors were a regular occurrence as Ruby was unable to serve incoming requests. The
application was running on Unicorn. Migrating to Puma completely fixed the application's problems. In this post
I'll talk about the benefits of switching to Puma and the reasons for switching.

### Forking

Unicorn is a UNIX prefork webserver. It works by creating multiple forks, all running a unicorn
process. UNIX forking works by spawning child process off of the master process. The parent and child processes both run in their own
memory and address space. Forking is easier to reason about than multi-threading because code doesn't need to be threadsafe.
[Unicorn's philosophy guide](https://bogomips.org/unicorn/PHILOSOPHY.html) describes it this way.

> Threads and Events Are Hard
>
> ...to many developers. Reasons for this is beyond the scope of this document. unicorn avoids concurrency within each worker
> process so you have fewer things to worry about when developing your application. Of course unicorn can use multiple worker
> processes to utilize multiple CPUs or spindles. Applications can still use threads internally, however.

Whilst forking allows developers to write simpler code, it has drawbacks. One is how it handles I/O (hitting the database
or making external API calls).

> unicorn is not suited for all applications. unicorn is optimized for applications that are CPU/memory/disk intensive and spend
> little time waiting on external resources (e.g. a database server or external API).

The reason Unicorn is not well suited to I/O constrained applications is that Ruby processes take up a significant amount of
memory. Most app servers are memory-constrained and too many forked processes will push the server past it's limit.
The number of workers should never be increased to the point where the application runs out of physical memory and hits swap.

### Multithreading

Puma is the default Rails web server. It is different from Unicorn because it supports both forking and multithreading.
Developers can configure multiple workers (the maximum number is constrained by RAM) and multiple threads within those workers
(the maximum number is constrained by CPU). Threads consume a fraction of the memory that workers consume. An application
can typically afford to have many more threads than workers. Each request is handled by a thread, when a thread
pauses for I/O another thread starts working. This is why multi-threading is more efficient at tackling slow I/O. The
rapid back and forth makes best use of RAM limitations and keeps the CPU busy.

Puma was designed for Ruby implementations that support full concurrency (Rubinus, JRuby, etc). With Puma, applications
written in these technologies can run across multiple CPUs. MRI has the Global Interpreter Lock (GIL) which
prevents Ruby from running multiple threads at the same time. However, as explained earlier, running MRI on Puma is beneficial
because it prevents threads getting 'locked-up' while waiting for I/O.

To help visualise this, imagine a Unicorn web server with 4 workers. Now imagine 5 incoming requests, all of which require a slow
external API call that takes 1 minute. The first 4 requests will be served by available workers but the 5th thread will
have to wait for 1 minute until the slow API call has finished and a worker is free to serve it.

Now imagine the same scenario with Puma. It has 4 workers each with 5 threads. The 4 workers will serve the first 4 incoming
requests and begin waiting for the slow API calls to return. During that time, another thread will start serving the 5th request.

### You should probably switch to Puma

Most Rails applications spend a significant amount of time writing/reading from a database and calling external APIs.
Anyone who is running Unicorn, without good reason, should switch to Puma. In my case, our application’s 500
timeout problems were completely fixed by switching to Puma.

### Further Reading
* [Unicorn's documentation](https://bogomips.org/unicorn/)
* [blog.scoutapp.com/articles/2017/02/10/which-ruby-app-server-is-right-for-you](http://blog.scoutapp.com/articles/2017/02/10/which-ruby-app-server-is-right-for-you)
* [stackoverflow.com/questions/25834333/what-exactly-is-a-pre-fork-web-server-model](https://stackoverflow.com/questions/25834333/what-exactly-is-a-pre-fork-web-server-model)
* [wikipedia.org/wiki/Fork_(system_call)](https://en.wikipedia.org/wiki/Fork_(system_call))
* [unix.stackexchange.com/questions/136637/why-do-we-need-to-fork-to-create-new-processes](https://unix.stackexchange.com/questions/136637/why-do-we-need-to-fork-to-create-new-processes)
* [blog.heroku.com/unicorn_rails](https://blog.heroku.com/unicorn_rails)

<div style="border-radius: 15px;background: #ffea92;padding: 20px;">
  If want some help building performant Rails Apps, please <a href="/contact">contact me</a>.
<div>
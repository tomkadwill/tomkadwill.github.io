---
layout:     post
title:      "Rails 502 Nginx gateway timeouts"
date:       2018-07-11 7:30:00 +0000
permalink:  'rails-502-nginx-gateway-timeouts'
---

My Rails projects was experiencing intermittent 502 Nginx Gatway Timeout errors. A particular controller was affected most but other controllers were also affected. In this blog post I’ll explain how I reproduced and then fixed the problem.

### Identifying the problem

First, I checked the Nginx logs and found:

```
upstream prematurely closed connection while reading response header from upstream
```

The error message isn’t very helpful.

I did some research into Nginx and came to the conclusion that it was not the problem. Google provides a few results for ‘how to fix 502 gateway timeout issues in Rails’, most advise tweaking your Nginx config to solve the problem. However, none of that worked for me, and for good reason. Nginx was doing it’s job correctly, it was killing requests that took too long. Read on to understand what I mean.

### Reproducing the problem

Next I tried to reproduce the error on UAT, which is an environment that mirrors production but with anonymised data. UAT is useful for replicating performance and infrastructure problems that require a production sized dataset.

To reproduce the error, I used [artillery.io](https://artillery.io/) to simulate production traffic. My use of Artillery was unscientific but effective. I tried a few different configuration options and eventually found one that worked.

I created the following configuration file, called `test.yml`:

```
config:
  target: 'https://api.my-project.com'
  phases:
    - duration: 180
      arrivalRate: 20
scenarios:
  - flow:
    - get:
        url: “/login”
```

Then I ran:

`artillery run test.yml`

With this configuration, Artillery hits the application for 180 seconds, with 20 servers, hitting every second on average. Using this, I was able to reproduce the problem consistently.

### Fixing the problem

I checked New Relic and noticed that the troublesome controller had slow response times (over 5 seconds). Even with low traffic, although there were no 502 errors, response times were slow. This led me to believe that improving the controller response time would alleviate the problem.

Using New Relic, I investigated where the controller was spending it’s time. I discovered that the majority of the response was spend on a single SQL query. I checked the application code and found this:

```ruby
datasource.datasource_jobs.any?
```

The problem is `datasource_jobs` has over 100,000 records. Ruby tries to initialize ActiveRecord objects for all of these records, just to check `.any?`. I was able to massively improve the controller's performance by re-writing it as:

```ruby
datasource.datasource_jobs.first.any?
```

After that, the 502 gateway errors disappeared. 🎉🎉

### Takeaway

Nginx serves 502 Gateway errors but Nginx was not the cause of the problem. The problem was that the application took too long to serve traffic and so Nginx started killing requests. The solution was to make the Rails controller more performant and lower response times.

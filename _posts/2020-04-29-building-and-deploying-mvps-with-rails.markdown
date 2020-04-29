---
layout:     post
title:      "Building and deploying MVPs with Ruby on Rails"
date:       2020-04-29 07:21:00 +0000
permalink:  'building-and-deploying-mvps-with-rails'
---

I use Ruby on Rails for [CommitCheck](https://commitcheck.com) and [PR Scheduler](https://prscheduler.com). Rails is a great framework for building and deploying web based SaaS applications. I want to explain why Rails is a good choice and how I use it to build applications.

### What makes Rails different

Rails was created by the team at [Basecamp](https://basecamp.com) by abstracting the framework out of their codebase. As such, it is best suited to building Basecamp style applications, which are [web based information systems](https://m.signalvnoise.com/horses-for-courses). Rails famously favours convention over configuration and comes packed full of features. These principles are controversial because many programmers prefer small, specialised packages that can be combined together. This approach has merit on big, complex systems but Rails shines when it comes to building MVPs, fast.

### The features that make development fast

#### 1. Naming conventions
Rails conventions speed up development. Frameworks like Sinatra, Flask and Express force you to think about the project structure. With Rails it’s all taken care of. The drawback of Rails is that it's less flexible, which can be a problem if you're not building conventional CRUD applications. Typically, though, Rails is a great fit for SaaS web apps.

#### 2. Built for MVC
Rails comes with everything needed to build MVC applications, which allows you to build web pages and API endpoints quickly. A large percentage of CommitCheck and PR Scheduler is Rails MVC code, I used very few external libraries.

#### 3. ActiveRecord
Rails ships with ActiveRecord, a mature and fully featured ORM. ActiveRecord allows you to build database backed applications with very little configuration. The framework has lots of syntactic sugar which reduces the learning curve and lessens the need to know SQL.

There are many criticisms made about ActiveRecord: It has a large interface which causes [complex and coupled applications](https://kellysutton.com/2019/10/29/taming-large-rails-codebases-with-private-activerecord-models.html), objects must be treated as 'special' objects and are thus [not well encapsulated](https://www.reddit.com/r/PHP/comments/3rwzgr/eli5_what_is_so_bad_about_active_record_orms/cws9ht7?utm_source=share&utm_medium=web2x), ActiveRecord [violates the Single Responsability Principle](https://www.mehdi-khalili.com/orm-anti-patterns-part-1-active-record), etc.

Of course, many Rails developers would refute these criticisms. Regardless, codebase quality and scalability are not a big concern for side projects. You need to build something useful, ActiveRecord will help you do that.

#### 4. Migrations and Scaffolding
Rails migrations are easy to use. Developers can generate and run migrations in a matter of minutes. Whilst other ORMs support migrations, most are not as easy to use.

In addition to migrations, Rails scaffolding generates boiler plate code and files. Just one command, `bin/rails g model User`, generates model, controller, migration, view and test files. Rails scaffolding really speeds up development time.

### How I built CommitCheck and PR Scheduler

Both projects are simple and have a small footprint. It’s a testament to Rails that so few lines are required to build complete web apps. When I started building the applications I focused on a single user journey. I believe that adding new features rarely works if the core concept doesn't resonate with users.

I also did very little error handling, early on. There's no point thinking about all the potential error cases when you don't have any users and may never have any. Instead I focused on optimising the 'happy path'. Only when I had acquired some users did I start handling error cases. I installed [Sentry](https://sentry.io) to capture any exceptions and began fixing them as they arose.

Testing is important and I added them from day one. each piece of functionality I added, I wrote a test for. I especially focused on integration tests. Having a solid suite of integration tests allows me to add features and refactor code without breaking things. Having a good test suite gives me the confidence to release new features quickly. Writing tests it well worth the upfront cost.

Here is a look at CommitCheck and PR Scheduler in more detail:

#### CommitCheck
```
+----------------------+--------+--------+---------+---------+-----+-------+
| Name                 |  Lines |    LOC | Classes | Methods | M/C | LOC/M |
+----------------------+--------+--------+---------+---------+-----+-------+
| Controllers          |    175 |    122 |       8 |      17 |   2 |     5 |
| Helpers              |      4 |      2 |       0 |       0 |   0 |     0 |
| Jobs                 |      4 |      2 |       1 |       0 |   0 |     0 |
| Models               |     59 |     38 |       6 |       3 |   0 |    10 |
| Mailers              |      6 |      4 |       1 |       0 |   0 |     0 |
| Channels             |     12 |      8 |       2 |       0 |   0 |     0 |
| JavaScripts          |     27 |      4 |       0 |       1 |   0 |     2 |
| Libraries            |     73 |     55 |       1 |       2 |   2 |    25 |
| Controller tests     |      0 |      0 |       0 |       0 |   0 |     0 |
| Helper tests         |      0 |      0 |       0 |       0 |   0 |     0 |
| Model tests          |      0 |      0 |       0 |       0 |   0 |     0 |
| Mailer tests         |      0 |      0 |       0 |       0 |   0 |     0 |
| Integration tests    |      0 |      0 |       0 |       0 |   0 |     0 |
| System tests         |      0 |      0 |       0 |       0 |   0 |     0 |
| Feature specs        |    203 |    141 |       0 |       0 |   0 |     0 |
| Model specs          |     60 |     48 |       0 |       0 |   0 |     0 |
| Request specs        |   1208 |   1086 |       0 |       0 |   0 |     0 |
| Service specs        |    130 |    105 |       0 |       0 |   0 |     0 |
+----------------------+--------+--------+---------+---------+-----+-------+
| Total                |   1961 |   1615 |      19 |      23 |   1 |    68 |
+----------------------+--------+--------+---------+---------+-----+-------+
  Code LOC: 235     Test LOC: 1380     Code to Test Ratio: 1:5.9
```

CommitCheck is just over 200 lines of application code. When I initially released the app it was just a UI for choosing a regex and logic for handling GitHub web hooks. There were many flaws with the initial release: It only supported repos with one contributor, it didn't support private repos, it marked build the status incorrectly, etc.

I added features and fixed bugs gradually, by listening to user demand. With each new scenario or bug fix I'd add tests. As you can see there are 1380 lines of test code for 235 lines of application code.

#### PR Scheduler
```
+----------------------+--------+--------+---------+---------+-----+-------+
| Name                 |  Lines |    LOC | Classes | Methods | M/C | LOC/M |
+----------------------+--------+--------+---------+---------+-----+-------+
| Controllers          |     48 |     31 |       5 |       4 |   0 |     5 |
| Helpers              |      4 |      2 |       0 |       0 |   0 |     0 |
| Jobs                 |      4 |      2 |       1 |       0 |   0 |     0 |
| Models               |     45 |     30 |       3 |       4 |   1 |     5 |
| Mailers              |      6 |      4 |       1 |       0 |   0 |     0 |
| Channels             |     12 |      8 |       2 |       0 |   0 |     0 |
| JavaScripts          |     28 |      4 |       0 |       1 |   0 |     2 |
| Libraries            |      8 |      6 |       0 |       0 |   0 |     0 |
| Controller tests     |      0 |      0 |       0 |       0 |   0 |     0 |
| Helper tests         |      0 |      0 |       0 |       0 |   0 |     0 |
| Model tests          |      0 |      0 |       0 |       0 |   0 |     0 |
| Mailer tests         |      0 |      0 |       0 |       0 |   0 |     0 |
| Integration tests    |      0 |      0 |       0 |       0 |   0 |     0 |
| System tests         |      0 |      0 |       0 |       0 |   0 |     0 |
| Request specs        |    504 |    461 |       0 |       0 |   0 |     0 |
| Service specs        |    346 |    297 |       0 |       0 |   0 |     0 |
+----------------------+--------+--------+---------+---------+-----+-------+
| Total                |   1005 |    845 |      12 |       9 |   0 |    91 |
+----------------------+--------+--------+---------+---------+-----+-------+
  Code LOC: 87     Test LOC: 758     Code to Test Ratio: 1:8.7
```

PR Scheduler is only 87 lines of code. When I initially released the app it was just a GitHub web hook that allowed users to schedule PRs via a comment. I've added some features but still today the app is very simple. I haven't build obvious things, like the ability to unschedule, because user demand is not as high as CommitCheck. I will invest more time when (and if) usage increases.

Each feature that I've build has good test coverage. There are 758 lines of test code for 87 lines of application code.

To reduce build time, I did not include a UI. PR Scheduler is just a landing page and Rails API server. All user interactions happen via comments on GitHub PRs. As a backend engineer, I try to reduce the size of the UI or piggyback on someone elses UI.

### Deploying Rails Apps

Once I built the prototypes for CommitCheck and PR Scheduler I needed to deploy them. I use the word prototypes to mean pre-MVP. The applications were certainly not ready for public release when I first deployed them, however, I’d gotten to a point where it was beneficial to have them on a server. Both projects are GitHub apps so it was important to test them with real GitHub web hooks.

For deploying Rails applications I'd recommend one of three options: [Heroku](https://www.heroku.com/), [Linode](https://www.linode.com/?r=0200bc8eedfdd23ca9e52179d3b40faea94a8550) or [AWS](https://aws.amazon.com/).

### Heroku

This is quickest and easiest way to deploy a Rails app. You can push code to Heroku with a single command and it will do everything for you. It will figure out that you are deploying a Rails app, install the gems, build your application and have it running on a custom domain. Heroku also makes it really easy to do things like: add a database, setup SSL, etc.

I’d recommend Heroku if you're new to Rails or if you just want to deploy something as quickly as possible.

### Linode

This is the service I'm using to deploy CommitCheck and PR Scheduler. The advantage over Heroku is fixed pricing and more control over the environment. With Heroku the infrastructure is abstracted away from you. You pay per application and don’t interact with the underlying server OS. With Linode you pay a fixed price per VPS. The VPS is your own server that you can do whatever you want with. In my case I have one $5 per month [Nanode](https://www.linode.com/products/nanodes/) instance running both CommitCheck and PR Scheduler.

The downside of Linode is that you’re trading time for money. I save money running my applications on Linode but I spend extra time configuring and maintaining the applications (eg: configuring Nginx, renewing SSL certificates, updating Ubuntu package versions).

### AWS

This is the giant of cloud services. If you predict that your application is going to grow into something big and complex then AWS may be the way to go. However, I would caution against it for MVPs. I previously hosted a project on AWS and found I spent a lot of time configuring the infrastructure and learning about numerous AWS services. In addition, my billing was uncertain. I didn’t like the idea of providing my credit card details and letting Amazon charge me whatever they wanted. Of course I could have calculated the pricing in advance but it’s not straight forward.
---
layout:     post
title:      "How I Use Trello"
date:       2019-10-10 20:16:00 +0000
permalink:  'how-i-use-trello'
---

I'm using Trello to manage my [CommitCheck](https://commitcheck.com), [PR Scheduler](https://prscheduler.com) and this blog. I've use tools like JIRA and Pivotal tracker for working with teams but Trello works great for me as a solo developer. In this post I'll describe my workflow and explain why Trello is a great took for solo devs and indie hackers.

Here is a screenshot of my Trello board:

TODO: screenshot here.

You can see that I have a few columns: Ideas, Ideas shipped, Icebox, TODO, In Progress and Done. Let me explain what the colunns are for and then I'll explain my workflow.

* **Ideas** is where I put any new project idea. Typically these are bigger project that's need breaking down into stories. CommitCheck is an example of one of these.
* **Ideas shipped** I use this list which ideas have been shipped. I don't really need this list, I could just move ideas straight to done but I keep this list for motivation. It helps me remember what I've shipped and is a good reference for what I've achieved. It also keeps me motivated to ship more projects in the future.
* **Icebox** This is where I put smaller ideas (those that are small enough to be worked on) but I'm not sure whether they should be worked on or not. For example, one of the cards is "Allow Regex to be run on commits/pr_description/pr_title using and/or". This was a feature requested by a user which they've now gone quiet on. I haven't had anyone else request this feature so I'm not yet sure whether it's worth working on.
* **TODO** This is essentially my backlog. It's a list of tickets that are ready to be worked on that I think are important to do. I try to prioritize this list so that I can just pick the next ticket off the top when I'm ready to work on something new.
* **In Progress** These are tickets that I'm currently working on.
* **Done** The last column is for tickets that I've built, shipped, validated and done.

### Projects

So here is how I do it. The first two columns I use for big ideas. When I have a new project idea I add it there. I try to set goals around how many new projects I ship so the **Ideas shipped** helps keeo me motivated and shows me whether I'm hitting my goals. At this point I should mention the coloured labels. Once I start working on a project, I assign it a coloured label. This label is used for all tickets so that I know which pieces of work correspond to which project. Most of the time it's obvious but it just helps me quickly, visually see what I'm working on.

### Tickets

The other columns are for actionable pieces of work. The same way that software teams break up pieces of work into tickets [TODO: is this from agile?], I try to do that with myself. I move things from the **Icebox** -> **TODO** When I know I definitely want to do the task. At this point I'll often add a checklist to breakdown the tickets further, basically acceptance criteria. Heres how the feature works in Trello:

TODO: add screenshot

When I've got time to pick up something new, I'll take the top ticket off the TODO column and drag it into **In Progress**. Ideally, I'd only have one ticket in progress at any one time but I'm not too strict about it and I often end up with more than one in progress. Especially because my projects have a small user base and so validating tickets is hard. For example, I currently have a ticket in progress for improving the user journey. Once I complete the work I plan to 'watch' a few users installing the app. I currently get 1-2 installations per week so validating this ticket will take a while. And, after validation I may need to do some more work if the results show that the feature is not working as I would like.

Once the ticket is complete, I've tested it and I'm happy with the results, I move it to Done.

### Fitting in with my other tools

How does Trello fit in with my other tools? I roughly follow [GTD](link). I use Wunderlist as my main inbox (yes I know Microsoft have sunsetted it and it's slowly dying by I haven't found an alternative yet). Trello provides a nice separation between personal and project work. When ideas pop into my head I add them to my wunderlist inbox. Then, at some point, I'll move person things into one of my Wunderlist lists and I'll move project work ideas over to Trello.
---
layout:     post
title:      "How I Use Trello"
date:       2019-10-10 20:16:00 +0000
permalink:  'how-i-use-trello'
---

I'm using Trello to manage [CommitCheck](https://commitcheck.com), [PR Scheduler](https://prscheduler.com) and this blog. I've use tools like JIRA and Pivotal tracker for working with teams but Trello works great for me as a solo developer. In this post I'll describe my workflow and explain why Trello is a good fit for solo devs and indie hackers.

Here is a screenshot of my Trello board:

![trello-screenshot](/assets/trello-screenshot.png)

You can see that I have a few columns: Ideas, Ideas shipped, Icebox, TODO, In Progress and Done. Let me explain what the colunns are for and then I'll explain my workflow.

* **Ideas** is where I put any new project idea. These are big projects that's need breaking down into smaller cards. CommitCheck is an example of one of these.
* **Ideas shipped** is a list of big projects that I've shipped. It's not necessary but it keeps me motivated.
* **Icebox** Is where I put tasks that are small enough to be worked on. At this stage I'm not sure how important they are or whether they should be worked on at all. For example, one of the cards is "Allow Regex to be run on commits/pr_description/pr_title using and/or". This feature was requested by a user who has now gone quiet on it. No one else has requested the feature so I'm not sure if it's worth working on.
* **TODO** Is my backlog. It's a list of tickets that are ready to be worked on, that I think are important. I try to prioritize the list so the top ticket is the most important.
* **In Progress** Are tickets that I'm currently working on.
* **Done** Are tickets that I've built, shipped and validated.

Now that you know what the columns are for I can explain my workflow.

### Projects

When I have a new project idea I add it to **Ideas**. I try to set goals around how many new projects I release so **Ideas shipped** helps keep me motivated and shows me my progress. At this point I should mention the coloured labels. When I start working on a project, I assign it a coloured label. This label is used for all tickets so I know which pieces of work correspond to which project. Most of the time it's obvious but it just helps me visually see what I'm working on.

### Tickets

The other columns are for actionable tasks. Agile software teams break up pieces of work into tickets [TODO: is this from agile?]. I try to do the same myself. I move tickets from the **Icebox** into **TODO** when I decide that it's definitely worth working on. At this point I'll often add a checklist to break the ticket down further. Here's is an example:

![trello-card-checklist](/assets/trello-card-checklist.png)

When I've got time to work on something new, I'll take the top ticket from **TODO** and drag it to **In Progress**. Ideally, I only have one ticket in progress at a time but I often end up with more. Sometimes I like to take a break from a particular task and work on something else. Also, validating tickets in production takes time. For example, I currently have a ticket in progress for improving CommitCheck's user journey. Once I complete the work I'll watch a few users install the app, to make sure the improvement worked. I currently get 1-2 installations per week so validating that ticket will take time.

Once a ticket is deployed and validated I move it to Done.

### Fitting in with my other tools

How does Trello fit in with my other tools? I roughly follow [GTD](https://www.amazon.co.uk/gp/product/0349408947/ref=as_li_qf_asin_il_tl?ie=UTF8&tag=kadwill-21&creative=6738&linkCode=as2&creativeASIN=0349408947&linkId=15a96f32b32c6dc5bfe30e69a19b8639). I use Wunderlist as my main inbox (yes I know Microsoft have sunsetted it and it's slowly dying by I haven't found an alternative yet). Trello allows me to separate personal and project work. When ideas pop into my head I add them to my wunderlist inbox. Then, at some point, I'll move person things into one of my Wunderlist folders and I'll move project ideas over to Trello.
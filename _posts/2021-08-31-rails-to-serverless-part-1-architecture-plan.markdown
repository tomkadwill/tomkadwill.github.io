---
layout:     post
title:      "Moving from Rails to serverless, Part 1: Architecture plan"
date:       2021-08-31 06:48:00 +0000
permalink:  'rails-to-serverless-part-1-architecture-plan'
---

Welcome to the Rails to serverless tutorial. In this series we'll walk through re-writing a Rails app as a serverless application, using AWS Lambda, API Gateway and DynamoDB. The topics in this series include:

1. [Architecture plan](/rails-to-serverless-part-1-architecture-plan)

I have a side project called PR Scheduler. It's a Ruby on Rails app that allows users to schedule a date & time for their PRs to be automatically merged. Recently I've been exploring AWS lambda and this app is a perfect candidate to be re-written with a serverless architecture. Most of the time the app is sitting there idle, waiting for a webhook request from Github.

Before I start building I want to have an idea of what I'm trying to build so I've drawn a rough architecture diagram:

# TODO: host this image
![prscheduler-serverless.png](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/dc1ccd2d-9e2c-4eba-8021-4c439ac276fe/prscheduler-serverless.png)

# TODO: add numbers to arch diagram to explain
- So, a GitHub user adds a comment on their PR saying "PR Scheduler schedule a merge to 28th Aug, 10am, 2021"
- A Github webhook will be triggered to my domain, which will be registered with an AWS API Gateway
- When my API Gateway received the request it will invoke a lambda function.s The lambda function will save the scheduled time in the DynamoDB database, along with the PR details.
    - The Lambda will also make a request back to Github to make a comment, to notify the user that PR Scheduler has scheduled their PR
- Then, in the background I'll have a lambda running on a cron or some kind of trigger which will be continually checking the database for any PRs that are next to be merged
- When the time is right, the lambda will make a call to GitHub request the PR to be merged

I hope that all makes sense. If this project sounds interesting to you, tune in to the next video where I'll start working on the first lambda. See you in the next one!
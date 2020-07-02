---
layout:             post
title:              "Rails API Tutorial, Part 1: Creating Rails API Project and Routing"
date:               2020-06-28 12:56:00 +0000
permalink:          'rails-api-tutorial-part-1-create-rails-api-project-and-routing'
last_modified_at:   2020-07-02 7:30:00 +0000
---

Welcome to the Rails 6 API tutorial. In this series we'll walk through building a backend API using Ruby on Rails. The topics in this series include:

1. [Creating a Rails API Project and Routing](/rails-api-tutorial-part-1-create-rails-api-project-and-routing)
2. [Basic Controller and Models](/rails-api-tutorial-part-2-basic-controllers-and-models)
3. [Building a POST Endpoint]()

<style>.embed-container { position: relative; padding-bottom: 56.25%; height: 0; overflow: hidden; max-width: 100%; } .embed-container iframe, .embed-container object, .embed-container embed { position: absolute; top: 0; left: 0; width: 100%; height: 100%; }</style><div class='embed-container'><iframe src='https://www.youtube.com/embed//6KqbPJtA5O8' frameborder='0' allowfullscreen></iframe></div>

#### My background

I'm a software engineer with approximately 10 years experience and I've been developing Ruby on Rails applications for the majority of that time. A lot of my time with Rails has been spent working on backend API applications. In this tutorial I want to share that knowledge.

#### The application

In this tutorial series we're going to build an book store, to complete with Amazon.

#### Installing Rails

This tutorial assumes you have Ruby/Rails installed on your machine. If you haven't done so already [GoRails has a good tutorial on how to do that](https://gorails.com/setup/osx/10.15-catalina).

#### Creating a new Rails API app

For a full list of what Rails API-only applications include, you can [read the docs](https://guides.rubyonrails.org/api_app.html). At a high level, Rails API does not include the view rendering logic but does include controllers, models, routing, etc.

Let's create a new Rails API-only application. I'm naming my application 'nile' but you can call it whatever you want:

```
$ rails new nile --api
```

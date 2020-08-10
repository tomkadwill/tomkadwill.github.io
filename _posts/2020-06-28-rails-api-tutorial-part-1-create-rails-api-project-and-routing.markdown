---
layout:             post
title:              "Rails API Tutorial, Part 1: Creating Rails API Project and Routing"
date:               2020-06-28 12:56:00 +0000
permalink:          'rails-api-tutorial-part-1-create-rails-api-project-and-routing'
last_modified_at:   2020-07-09 7:30:00 +0000
---

Welcome to the Rails 6 API tutorial. In this series we'll walk through building a backend API using Ruby on Rails. The topics in this series include:

1. [Creating a Rails API Project and Routing](/rails-api-tutorial-part-1-create-rails-api-project-and-routing)
2. [Basic Controller and Models](/rails-api-tutorial-part-2-basic-controllers-and-models)
3. [Building a POST Endpoint](/rails-api-tutorial-part-3-building-a-post-endpoint)
4. [HTTP Status Codes](/rails-api-tutorial-part-4-http-status-codes)
5. [Active Record Validations](/rails-api-tutorial-part-5-active-record-validations)
6. [Destroy Controller Action](/rails-api-tutorial-part-6-destroy-controller-action)

<style>.embed-container { position: relative; padding-bottom: 56.25%; height: 0; overflow: hidden; max-width: 100%; } .embed-container iframe, .embed-container object, .embed-container embed { position: absolute; top: 0; left: 0; width: 100%; height: 100%; }</style><div class='embed-container'><iframe src='https://www.youtube.com/embed//6KqbPJtA5O8' frameborder='0' allowfullscreen></iframe></div>

#### My background

I'm a software engineer with 10 years industry experience and I've been building Ruby on Rails applications for the majority of that time. A lot of my time with Rails has been spent working on backend API applications. In this tutorial I want to share that knowledge.

#### The application

In this tutorial series we're going to build an API backend for a book store.

#### Installing Rails

This tutorial assumes you have Ruby/Rails installed on your machine. If you haven't done so already [GoRails has a good tutorial on how to do that](https://gorails.com/setup/osx/10.15-catalina).

#### Creating a new Rails API app

For a full list of what Rails API-only applications include, you can [read the docs](https://guides.rubyonrails.org/api_app.html). At a high level, Rails API does not include the view rendering logic but does include controllers, models, routing, etc.

Let's create a new Rails API-only application. I'm naming my application "nile" but you can call it whatever you want. To create the application run the following command in the terminal:

```
$ rails new nile --api
```

The output should look something like this:

```
      create
      create  README.md
      create  Rakefile
      create  .ruby-version
      create  config.ru
      create  .gitignore
      create  Gemfile
         run  git init from "."
Initialized empty Git repository in /Users/tomkadwill/workspace/nile/.git/
      create  app
      create  app/assets/config/manifest.js
      create  app/assets/stylesheets/application.css
      create  app/channels/application_cable/channel.rb
      create  app/channels/application_cable/connection.rb
      create  app/controllers/application_controller.rb
      create  app/helpers/application_helper.rb
      .
      .
      .
```

Rails has created all of the project files for us and installed the default gems.

Now we can `cd` into the project and open it in our editor (I'm using [VSCode](https://code.visualstudio.com/) but you can use any good text editor):

```
$ cd nile
$ code .
```

#### Listing routes

Now that we have initialized our Rails project let's look at how we can list routes within our application. Routes map URLs to controllers. You can think of them as the entry point for users to interact with our application. Rails has a built in command which lists all the routes in our application:

```
$ bin/rails routes
```

#### Adding a route for listing books

TODO
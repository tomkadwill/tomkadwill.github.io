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
7. [Exception Handling in Controllers](/rails-api-tutorial-part-7-exception-handling-in-controllers)

<style>.embed-container { position: relative; padding-bottom: 56.25%; height: 0; overflow: hidden; max-width: 100%; } .embed-container iframe, .embed-container object, .embed-container embed { position: absolute; top: 0; left: 0; width: 100%; height: 100%; }</style><div class='embed-container'><iframe src='https://www.youtube.com/embed//6KqbPJtA5O8' frameborder='0' allowfullscreen></iframe></div>

#### My background

I'm a software engineer with 10 years industry experience and I've been building Ruby on Rails applications for much of that time. A lot of my time with Rails has been spent working on backend APIs. In this tutorial I want to share that knowledge.

#### The application

In this tutorial series we're going to build an API backend for a book store. The API should allow us to do things like list the books we have in store, add new books to the store and delete books from our store's database.

#### Installing Rails

This tutorial assumes you have Ruby/Rails installed on your machine. If you haven't done so already [GoRails has a good guide on how to do that](https://gorails.com/setup/osx/10.15-catalina).

#### Creating a new Rails API app

To build the application we're going to be using Rails API-only. This allows us to generate a specialised Rails application that has all the functionality required for building APIs. For a full list of what Rails API-only applications include, you can [read the docs](https://guides.rubyonrails.org/api_app.html). At a high level, Rails API-only does not include the view rendering logic but does include controllers, models, routing, etc.

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

#### Adding the first API route

Now that we have initialized the Rails project we can start working on the first API endpoint. To start with, we're going to be adding an endpoint to return all of the books we have in our store. The route should look like: `localhost:3000/books`. To do that we can add the following to `config/routes.rb`:

```ruby
Rails.application.routes.draw do
  # For details on the DSL available within this file, see https://guides.rubyonrails.org/routing.html
  get '/books' => 'books#index'
end
```

To see what this has done, we can run `rake routes` via the command line:

```
$ bin/rails routes
```

If you scroll to the top of the console output, you should see the new route at the top:

```
Prefix Verb   URI Pattern           Controller#Action
books  GET    /books(.:format)      books#index
```

Our route is correct but we can re-write it in a nicer way:

```ruby
Rails.application.routes.draw do
  # For details on the DSL available within this file, see https://guides.rubyonrails.org/routing.html
  resources :books
end
```

The `resources` method will generate all 7 restful resources. If we run `rake routes` again we see:

```
Prefix Verb   URI Pattern           Controller#Action
books  GET    /books(.:format)      books#index
       POST   /books(.:format)      books#create
book   GET    /books/:id(.:format)  books#show
       PATCH  /books/:id(.:format)  books#update
       PUT    /books/:id(.:format)  books#update
       DELETE /books/:id(.:format)  books#destroy
```

This is great because Rails is able to generate all the routes we need via a single method call. However, we don't need all of these routes so we can pair it down to just the one we need:

```ruby
Rails.application.routes.draw do
  # For details on the DSL available within this file, see https://guides.rubyonrails.org/routing.html
  resources :books, only: :index
end
```

Which will give us:

```
Prefix Verb   URI Pattern           Controller#Action
books  GET    /books(.:format)      books#index
```

#### Starting the development server

TODO - 7:00

#### Hitting our new route

TODO - 7:00
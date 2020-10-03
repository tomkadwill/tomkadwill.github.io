---
layout:     post
title:      "Rails API Tutorial, Part 2: Basic Controllers and Models"
date:       2020-06-30 07:33:00 +0000
permalink:  'rails-api-tutorial-part-2-basic-controllers-and-models'
---

Welcome to the Rails 6 API tutorial. In this series we'll walk through building a backend API using Ruby on Rails. The topics in this series include:

1. [Creating a Rails API Project and Routing](/rails-api-tutorial-part-1-create-rails-api-project-and-routing)
2. [Basic Controller and Models](/rails-api-tutorial-part-2-basic-controllers-and-models)
3. [Building a POST Endpoint](/rails-api-tutorial-part-3-building-a-post-endpoint)
4. [HTTP Status Codes](/rails-api-tutorial-part-4-http-status-codes)
5. [Active Record Validations](/rails-api-tutorial-part-5-active-record-validations)
6. [Destroy Controller Action](/rails-api-tutorial-part-6-destroy-controller-action)
7. [Exception Handling in Controllers](/rails-api-tutorial-part-7-exception-handling-in-controllers)

<style>.embed-container { position: relative; padding-bottom: 56.25%; height: 0; overflow: hidden; max-width: 100%; } .embed-container iframe, .embed-container object, .embed-container embed { position: absolute; top: 0; left: 0; width: 100%; height: 100%; }</style><div class='embed-container'><iframe src='https://www.youtube.com/embed//nCb_mqAKusg' frameborder='0' allowfullscreen></iframe></div>

In [part 1](/rails-api-tutorial-part-1-create-rails-api-project-and-routing) we added a route for `GET /books`. The next step is to add a controller for it. To do that we can use the Rails generator, via the command line we can do:

```
$ bin/rails g controller BooksController index
```

This will generate a controller called `BooksController` with an `index` action method.

This will also create an additional route which looks like `get books/index`. We don't need that because we've already written the route so the generated route can be deleted.

The generated controller can be found under `app/controller/books_controller.rb` and looks like this:

```ruby
class BooksController < ApplicationController
  def index
  end
end
```

TODO: continue from 2:22.
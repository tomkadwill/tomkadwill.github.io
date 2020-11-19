---
layout:             post
title:              "Rails API Tutorial, Part 2: Basic Controllers and Models"
date:               2020-06-30 07:33:00 +0000
permalink:          'rails-api-tutorial-part-2-basic-controllers-and-models'
last_modified_at:   2020-11-19 07:06:00 +0000
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

#### Generating a controller

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

As you can see, `BooksController` is a regular Ruby class that inherits from `ApplicationController`. The inheritance provides the class with methods for handling incoming requests, params, etc.

You'll notice that the class was also generated with an empty `index` method (Rails developers call this an 'index action'). Even though the method is empty it does have a purpose. Any requests which hit `BooksController#index` will be served a successful response, with a 204 status code.

#### Calling the controller action via cURL

We can test that via cURL:

```
$ curl http://localhost:3000/books -v
```

You should see that the cURL request returns a 204 status code. We can also look at the server logs:

```
Started GET "/books" for ::1 at 2020-10-27 19:31:35 +0100
Processed by BooksController#index as */*
Completed 204 No Content in 0ms (ActiveRecord: 0.0ms | Allocations: 32)
```

The server log shows that we made a GET request to `/books`, the request was processed by the `BooksController#index` method and it returned a 204.

#### Updating the controller action to return a list of objects

Currently `BooksController#index` returns a successful response code but it doesn't return any data. Let's update the method to return a list of book objects:

```ruby
class BooksController < ApplicationController
  def index
    render json: Book.all
  end
end
```

We'll use `render`, with the `json` option and pass in an ActiveRecord collection. In this case we use `Book.all` which will return all books in the database. We haven't defined the `Book` model yet but we'll do that next.

#### Generating a model

We can generate a model using the Rails generate command. We just need to provide a name for our model and a list of attributes:

```
rails g model Book title:string author:string
```

In this example we're generating a model called `Book` which has two fields, `title` and `author`, both of which are string fields. When we run the command Rails will generate a couple of files for us:
* a migration file - `db/migrate/20200620150063_create_books.rb`
* a model file - `app/models/book.rb`
* some test files

Here's a look inside the files that were created:

#### book.rb

```ruby
class Book < ApplicationRecord
end
```

As you can see there's not much going on in this file, at the moment. We're defining a `Book` class and inheriting from `ApplicationRecord`. Although we haven't added any of our own logic, to this file, it still gives us a lot of capabilities. By inheriting from `ApplicationRecord` we're giving `Book` all the power of Rails, by making it an `ActiveRecord` class.

Rails will map this Ruby class to the `book` table in our database. We can then use this model to access the database in our Rails application by calling methods. For example: `Book.all` will return all records.

#### migration file

```ruby
class CreateBooks < ActiveRecord::Migration[6.0]
  def change
    create_table :books do |t|
      t.string :title
      t.string :author

      t.timestamps
    end
  end
end
```

As well as the books model, we need to actually create a database table. To do that, Rails has generated a migration file for us which is like a blueprint for creating a database table. Here we're creating a table called `books` with two fields: `title` and `author`, both of which are string fields. The table also includes `created_at` and `updated_at` timestamps. This table will be used to store books in our system.

If we try to run the server now it will fail because we have migrations in our codebase that haven't been applied yet. So before we continue we should run the migrations. To do that, we can execute:

```
$ bin/rails db:migrate
```

This will apply the migration, creating the `books` table. You may be wondering which database Rails has updated. Rails ships with sqlite3 by default. When you run the migrations you may notice a new file appear called `development.sqlite3`. This is your development database.

TODO: continue from 9:07

<div style="border-radius: 15px;background: #ffea92;padding: 20px;">
  <p>☎️ <a href="https://superpeer.com/tomkadwill">Book a slot for 1-to-1 help or pair programming</a></p>
  <p>🙏 <a href="https://buymeacoff.ee/tomkadwill">If you found this useful and want to support me, please consider a small donation. Thank you!</a></p>
  <p>✉️ <a href="mailto:tomkadwill@gmail.com">If you want to work with me on a project, email tomkadwill@gmail.com</a></p>
<div>
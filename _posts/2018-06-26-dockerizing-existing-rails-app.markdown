---
layout:     post
title:      "Dockerizing an existing Postgres Rails App and using Docker Compose"
date:       2018-06-26 07:00:00 +0000
permalink:  'dockerizing-existing-rails-app'
---

In this guide I’ll explain how to dockerize an existing Rails application and use Docker Compose to run the application with a Postgres database.

### Dockerfile

First, add a `Dockerfile` at the root level of your Rails application. This is a simple Dockerfile for a Rails application:

```
FROM ruby:2.4.2

RUN apt-get update -qq && apt-get install -y build-essential libpq-dev nodejs

RUN mkdir /myapp
WORKDIR /myapp

COPY Gemfile /myapp/Gemfile
COPY Gemfile.lock /myapp/Gemfile.lock

RUN bundle install

COPY . /myapp
```

Change the first line to match the version of Ruby that you're using. Notice that the Dockerfile does not run any commands (eg: `rails server`) because this will be handled by Docker Compose.

### Docker Compose file

Next, create a `docker-compose.yml` file at the root level of your application. This is a file for running Rails and Postgres:

```
version: '3'
services:
  db:
    image: postgres:9.6-alpine
    volumes:
      - ./tmp/db:/var/lib/postgresql/data
  web:
    build: .
    command: bundle exec rails s -p 3000 -b '0.0.0.0'
    volumes:
      - .:/myapp
    ports:
      - "3000:3000"
    depends_on:
      - db
```

The Postgres version (`image: postgres:9.6-alpine`) is important. Initially, I couldn’t get my application to run because I was using the latest Postgres, which didn't match my application.

This Docker Compose file will create a database, called ‘db', and mount it to the local file system. This means that the database will be persisted, even when you terminate docker-compose. The docker-compose file will also create a Rails app, called ‘web’. The Rails app is also mounted, which means you can edit files locally and the changes will be reflected on the docker container. Docker Compose maps external port 3000 to the container port 3000 so you can access the container via [http://localhost:3000](http://localhost:3000). It also runs a command to start the rails server. You can modify this command if you are using something like foreman to start your application.

### Rails Database

The last step is to allow the Rails application to access the database. If you run Docker Compose right now, both containers will start but Rails will have a database connection error.

To fix this, modify your `config/database.yml` file so that it looks something like:

```
default: &default
   adapter: postgresql
   encoding: unicode
   host: db
   username: postgres
   pool: <%= ENV.fetch("RAILS_MAX_THREADS") { 5 } %>
   timeout: 5000
```

`host: db` allows the rails application to connect to the database container. I also specified the database username because the root role doesn’t exist on the `postgres:9.6-alpine` image.

With these changes to the database.yml file, you can start your application by running:

```
$ docker-compose build
$ docker-compose up
```

`docker-compose build` builds the images on your machine. `docker-compose up` starts the containers based on those images.

You should now be able to browse to [http://localhost:3000](http://localhost:3000) and see your application running.

#### References:
1. Docker Docs. Quickstart: Compose and Rails: [https://docs.docker.com/compose/rails/#define-the-project](https://docs.docker.com/compose/rails/#define-the-project)

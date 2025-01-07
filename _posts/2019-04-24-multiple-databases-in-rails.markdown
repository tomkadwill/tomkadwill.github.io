---
layout:     post
title:      "Multiple Databases in Rails"
date:       2019-04-24 06:35:00 +0000
permalink:  'multiple-databases-in-rails'
---

Rails supports multiple database connections which is important for scaling applications. This post explains the benefits and demonstrated how to implement it.

There are two ways to scale a database constrained application: vertically or horizontally. Vertical means increasing the performance of your database by upgrading the server. Horizontal means increasing the number of database servers. Rails multi database support enables horizontal scaling. This post will cover a specific type of horizontal scaling known as ‘single leader replication’, where you have one primary database used for read/write requests and one or more read replicas used only for read requests.

Assuming that your application has more read than write requests, which is more often the case, it can scale with replicas. Even a single read replica can improve performance. Imagine a database that can handle 100 concurrent requests. Assume that 10% are write requests. Serving read requests with replicas would remove 90% of the load from the primary database. This would allow the system to scale up to ~1000 concurrent requests. 

Horizontal scaling is typically more cost effective than vertical scaling because server upgrade costs do not increase linearly; they tend to inflate with each upgrade. Horizontal scaling is also more fault tolerant. If the primary instance dies a replica can replace it. Replicas can be spread across multiple locations so that a specific datacentre outage doesn’t take the whole system down.

### Multiple databases in Rails 6

Setting up multiple databases is much easier in Rails 6. [Eileen Uchitelle](https://twitter.com/eileencodes?lang=en) lead the work of moving GitHub’s multi database logic into Rails. She gave a talk at Paris.rb called [The Future of Rails 6: Scalable by Default](https://www.youtube.com/watch?v=8evXWvM4oXM). In the video she talks about the multi database changes and other scaling improvements. She also says that large companies are back porting more enterprise features into Rails. This means Rails will continue to get better at scaling, in the future.

Now that you’re excited about the future of Rails and the new multi database feature. Let’s look at how to setup a Rails application with multiple databases.

First, you need to add multiple databases to the database.yml file. Rails 6 supports a three-tiered structure:

```
production:
  primary:
    <<: *default
    database: db/production.sqlite3
  replica:
    <<: *default
    database: db/production_replica.sqlite3
```

After adding a new database to Rails you should run migrations so that all database schemas are up to date:  

```
bin/rails db:migrate
```

In Rails 6 the normal migration tasks (`bin/rails db:create`, `bin/rails db:drop` & `bin/rails db:migrate`) run across all databases in the environment. You can also run migration tasks on individual databases, eg: `bin/rails db:migrate:primary` or `bin/rails db:drop:replica`.

Now you can build models that utilise multiple databases: 

```ruby
class User < ApplicationRecord
  connects_to database: { writing: :primary, reading: :replica }
end
```

This tells `User` to use `primary` as the primary database and `replica` as the replica.

### Automatic read/write switching

At this point you can choose to enable automatic read/write switching across the whole project or only use it for specific cases. Let’s talk about automatic switching first. Each request is sent to the primary or replica based on whether it’s a read (GET/HEAD) or write (POST/PATCH/DELETE). Write requests are always sent to the primary. Read requests are usually sent to the replica, unless the primary recently received a write. By default, the time delay is 2 seconds. The reason for this behaviour is that it takes time for database updates to propagate to all replicas. During this time, read replica queries could return stale data. If you don’t need it, the replication time delay can be modified or switched off entirely.

Have a look at [the source code](https://github.com/rails/rails/pull/35073/files#diff-28138bbcb53d9f5b99aea2bda972215eR58) If you want to understand how the logic to route read/write requests works under the hood.

To enable automatic read/write switching, add the following configuration options to `config/application.rb`

```ruby
config.active_record.database_selector = { delay: 2.seconds }
config.active_record.database_resolver = ActiveRecord::Middleware::DatabaseSelector::Resolver
config.active_record.database_resolver_context = ActiveRecord::Middleware::DatabaseSelector::Resolver::Session
```

### Block Syntax

The other way to use multiple databases is the block syntax. This is used to change the database connection while executing a block of code. It can be used with or without automatic read/write switching. The block syntax looks like this:

```ruby
Transaction.connected_to(role: :reading) do
  Transaction.all
end
```

Let’s think about an example use case for the block syntax. In many applications the user profile page is hit frequently. It often displays data from other parts of the app, for example, purchases. Let’s assume that the user can visit the purchases page to get a real-time view. On the profile page replication lag is okay. To improve performance, you can use the block syntax to ensure the user profile page always reads purchases from the read replica.

### Connecting to a different database for a specific model

The three-tiered database.yml allows you to specify any number of database connections. Imagine you want users to be able to view ‘archived’ purchases. Archiving data prevents database tables growing too large. Archiving data saves money and maintains database performance. Let’s assume any purchase older than 6 months can be archived and moved to a separate database. You can build a new model for old purchases that connects to the archive database:

```
old_purchases:
  <<: *default
  database: db/production_old_purchases.sqlite3
```

```ruby
class OldPurchases < ApplicationRecord
  connects_to database: { writing: :old_purchases, reading: :old_purchases }

  belongs_to :user
end
```

### Development Setup

A multi database setup makes local development more difficult. I recommend configuring your database.yml so that all endpoints are the same for development. That way, you get a normal development experience. If you want to test something with your multi database setup you can run the Rails server in production mode:

```
development:
  primary:
    <<: *default
    database: db/development.sqlite3
  replica:
    <<: *default
    database: db/development.sqlite3
  old_transactions:
    <<: *default
    database: db/development.sqlite3
```

### Scaling

To scale a system you need more than one read slave. I asked Eileen whether there are any plans for Rails to support multiple read slaves. She said “At this time no Rails can’t spread the load for you. I’m not sure it should either as that’s really dependent on your setup. At GitHub we have a global load balancer that is a layer above the rails app that figures out where to send the traffic”. Interestingly, both [makara](https://github.com/taskrabbit/makara) and [octopus](https://github.com/thiagopradi/octopus), 3rd party multi database gems, support multiple readers; maybe it will be added to Rails in the future.  

Setting up multiple readers outside of Rails depends on your infrastructure. If you’re using AWS you could use a [Route53 weighted record set to distribute across your read replicas](https://aws.amazon.com/premiumsupport/knowledge-center/requests-rds-read-replicas/). This provides you with a single read replica endpoint that distributes requests to different RDS instances.

### References and links

* [A Primer on Database Replication](https://www.brianstorti.com/replication/) by Brian Storti
* [The Future of Rails 6: Scalable by Default](https://www.youtube.com/watch?v=8evXWvM4oXM) by Eileen Uchitelle
* [PostgreSQL rocks, except when it blocks: Understanding locks](https://www.citusdata.com/blog/2018/02/15/when-postgresql-blocks/) by Marco Slot
* [How can I distribute read requests across multiple Amazon RDS read replicas?](https://aws.amazon.com/premiumsupport/knowledge-center/requests-rds-read-replicas/) by AWS
* [Does AWS Aurora support read/write switching?](https://dba.stackexchange.com/a/117073) from StackOverflow
* [Difference between scaling horizontally and vertically for databases](https://stackoverflow.com/questions/11707879/difference-between-scaling-horizontally-and-vertically-for-databases) from StackOverflow
* [Makara gem](https://github.com/taskrabbit/makara)
* [Octopus gem](https://github.com/thiagopradi/octopus)
* Series of Rails 6 multi db PRs:
  * [github.com/rails/rails/pull/32274](https://github.com/rails/rails/pull/32274)
  * [github.com/rails/rails/pull/33637](https://github.com/rails/rails/pull/33637)
  * [github.com/rails/rails/pull/33770](https://github.com/rails/rails/pull/33770)
  * [github.com/rails/rails/pull/34052](https://github.com/rails/rails/pull/34052)
  * [github.com/rails/rails/pull/34491](https://github.com/rails/rails/pull/34491)
  * [github.com/rails/rails/pull/34495](https://github.com/rails/rails/pull/34495)
  * [github.com/rails/rails/pull/34505](https://github.com/rails/rails/pull/34505)
  * [github.com/rails/rails/pull/35073](https://github.com/rails/rails/pull/35073)





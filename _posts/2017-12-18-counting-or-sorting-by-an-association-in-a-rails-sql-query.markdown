---
layout: post
title:  "Counting Or Sorting By An Association In A Rails SQL Query"
date:   2017-12-18 18:45:00 +0000
---

Recently I was tasked with writing a query which had to be sorted by the count of an associated table. I needed to return users, ordered by the number of offers they have. I did some research and made an attempt at some code.

```ruby
User.includes(:offers)
    .order('COUNT(offers.id) DESC')
    .references(:offers)
```

Unfortunately, this code returns the following error (I'm using Postgres):

```
column "users.id" must appear in the GROUP BY clause or be used in an aggregate function
```

I didn't understand why the GROUP BY clause was required but I added the `#group()` method to my query.

```ruby
User.includes(:offers)
    .group('user.id')
    .order('COUNT(offers.id) DESC')
    .references(:offers)
```

Then I got another error.

```
column "offers.id" must appear in the GROUP BY clause or be used in an aggregate function
```

Hmm.. At that point I realised that I didn't fully understand what was going on but I decided to proceed anyway and add `offers.id` to the GROUP BY clause.

```ruby
User.includes(:offers, :wards)
    .group(['user.id', 'offers.id'])
    .order('COUNT(offers.id) DESC')
    .references(:offers)
```

This worked. Sort of. The query was returning records but some of my tests were still failing. I needed to dig deeper into `#group` and `#order`.

### Why is #group required?

This is nothing to do with rails, it has to do with the way SQL works. #group is required here because the query needs to group offers by users, for the count to work. Here is some SQL to demonstrate:

```SQL
CREATE TABLE users (
    id INT NOT NULL,
    name varchar(255),
    PRIMARY KEY (id)
);

CREATE TABLE offers (
    id INT,
    name varchar(255),
    users_id INT,
    FOREIGN KEY (users_id)
        REFERENCES users(id)
        ON DELETE CASCADE
);

INSERT INTO users (id, name)
VALUES (1, 'tom');

INSERT INTO users (id, name)
VALUES (2, 'mark');

INSERT INTO offers (id, name, users_id)
VALUES (1, 'offer 1', 1);

INSERT INTO offers (id, name, users_id)
VALUES (2, 'offer 2', 1);

INSERT INTO offers (id, name, users_id)
VALUES (3, 'offer 3', 2);
```

The code above sets up a `users` and `offers` table (a user can have many offers). It also adds some test data. If you want to follow along you can use [sqlfiddle](http://sqlfiddle.com). Now that the database is setup, I can write a query to return the number of offers for each user, using GROUP BY:

```SQL
SELECT COUNT(offers.id) AS offers_count, users.name AS user_name
    FROM users
    LEFT JOIN offers ON users.id = offers.users_id
    GROUP BY user_name
    ORDER BY offers_count;
```

| offers_count | user_name |
|-------------|-------------|
| 1           | mark        |
| 2           | tom         |


The results look good. The query correctly shows that Mark has one offer and Tom has two. To understand what GROUP BY is doing, I'll write the same query again, without it.

```SQL
SELECT COUNT(offers.id) AS offers_count, users.name AS user_name
    FROM users
    LEFT JOIN offers ON users.id = offers.users_id
    ORDER BY offers_count;
```

| offers_count | user_name |
|-------------|-------------|
| 3           | tom         |

As you can see, the query does not return the correct results. The query is just counting all of the offers in the database.

### Differences between MySQL and PostgreSQL
Unfortunately the story doesn't end there. There are some differences between MySQL and PostgreSQL which further complicate matters. [As this Rails issue thread explains](https://github.com/rails/rails/issues/1515), in Postgres the columns that appear in the FROM clause have to be the same ones that appear in the GROUP BY clause. This is difficult to conceptualise so I'm going to demonstrate with some more SQL.

I'll use the same setup as before, only this time let's imagine that I have a new requirement to also select the name of the offers. I'll also remove the ORDER BY to keep the example simple.

```SQL
SELECT COUNT(offers.id) AS offers_count, users.name AS user_name, offers.name AS offers_name
    FROM users
    LEFT JOIN offers ON users.id = offers.users_id
    GROUP BY user_name;
```

In MySQL 5.6 I get the following.

| offers_count | user_name | offers_name |
|-------------|-------------|------------|
| 1           | mark        | 	offer 3  |
| 2           | tom         |   offer 1  |

If I try the same thing in PostgresSQL 9.3, I get an error:

```
column "offers.name" must appear in the GROUP BY clause or be used in an aggregate function
```

To get this working I need to change the code to:

```SQL
SELECT COUNT(offers.id) AS offers_count, users.name AS user_name, offers.name AS offers_name
    FROM users
    LEFT JOIN offers ON users.id = offers.users_id
    GROUP BY user_name, offers_name;
```

| offers_count | user_name | offers_name |
|-------------|-------------|------------|
| 1           | tom        | 	offer 2  |
| 1           | mark         |   offer 3  |
| 1           | tom         |   offer 1  |

However, you'll notice that the results are not the same as results from MySQL. The only solution I've found is to re-write the query in a different way or to do some of the work in Ruby.

I hope this post helps anyone who is having issues with `#group` and `#order` in Rails.

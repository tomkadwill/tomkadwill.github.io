---
layout: post
title:  "Creating a Postgres User with Permissions for a Rails Application"
date:   2017-12-01 23:22:00 +0000
---

I recently started working on a new Rails application, which was using Postgres. The project had a `database.yml` file checked into source control. When trying to run the app, I got an error:

```
FATAL:  role "example" does not exist
```

This was because I did not have a role with the same name as the role specified in the `database.yml`. The following Postgres command creates a role (and password):

```
CREATE role example WITH password 'password';
```

After creating the role I tried to run the server again. Still no luck, I got a new error:

```
FATAL:  role "example" is not permitted to log in
```

This was because my newly created role has all permissions set to false. To give the role permission to login, I ran:

```
ALTER ROLE example WITH LOGIN;
```

At this point I was able to run the rails server but when I tried to run migrations I got a new error:

```
ERROR:  permission denied for relation schema_migrations
```

I needed to grant the role more permissions:

```
ALTER USER example CREATEDB;
```

lastly, to do DB rollbacks or delete operations I needed super user permissions for the role:

```
ALTER ROLE example SUPERUSER;
```

If you are working through these steps yourself then you can run the following command, at any point, to view your Postgres roles:

```
SELECT * FROM pg_roles;
```

{% include advertisements.html %}

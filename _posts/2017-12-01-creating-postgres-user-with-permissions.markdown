---
layout:     post
title:      "Creating a Postgres User with Permissions for a Rails Application"
date:       2017-12-01 23:22:00 +0000
---

Recently I worked on a Rails application that used Postgres. The project had a valid `database.yml` file but when I tried to run the app, I got an error:

```
FATAL:  role "example" does not exist
```

It's because I didn't have the role that's specified in the `database.yml` on my machine. The following Postgres command creates a role (and password):

```
CREATE role example WITH password 'password';
```

After creating the role I fired up the server again. Still no luck, I got a new error:

```
FATAL:  role "example" is not permitted to log in
```

Newly created Postgres roles have all permissions set to false. The role needed permission to login:

```
ALTER ROLE example WITH LOGIN;
```

At that point I was able to run the rails server but when I tried to run migrations I got another error:

```
ERROR:  permission denied for relation schema_migrations
```

To be able to make database changes, the role needed more permissions:

```
ALTER USER example CREATEDB;
```

Lastly, in order to perform DB rollbacks or delete operations the role needed super user permissions:

```
ALTER ROLE example SUPERUSER;
```

Protip: If you're modifying roles/permissions you can view Postgres roles, at any time, by running:

```
SELECT * FROM pg_roles;
```

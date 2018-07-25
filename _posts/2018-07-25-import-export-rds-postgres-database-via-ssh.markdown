---
layout:     post
title:      "How to Import/Export an RDS Postgres Database via SSH Tunnel"
date:       2018-07-25 8:39:00 +0000
comments:   true
permalink:  'import-export-rds-postgres-database-via-ssh'
---

Last week I was trying to export [CommitLint's](https://commitlint.com/) database, which is hosted on AWS Elastic Beanstalk. Connecting to the database requires a SSH tunnel. It took me a while to figure it out but I was able to, thanks to Michalis Antoniou's [blog post](https://medium.com/@michalisantoniou6/connect-to-an-aws-rds-using-an-ssh-tunnel-22f3bd597924). I just adapted it to work with Postgres.

### SSH tunnel
My RDS database doesn't have any ports exposed to allow clients to connect directly. Instead, I needed to connect via the application's EC2 instance. To do that, I needed to establish an SSH connection:

```
ssh -N -L localPort:rdsHost:remotePort user@remoteHost -i ~/path/to/key -v
```

```
localPort
the port your local database connects to. You can set this to any available port such as 1234, 3306 and so on.

rdsHost
your RDS endpoint (url)

remotePort
the port your remote database listens to for connections. For Postgres databases the default is 3306. For PostgreSQL databases, the default is 5432.

user@remoteHost
the username and the remote instance your tunnel will connect to the database through. These are the credentials you use to ssh into your web server (ec2)
```

Postgres example:

```
$ ssh -N -L 1234:aa2b0gzc77ba2ya8.bahsamb8blah.eu-west-2.rds.amazonaws.com:5432 ec2-user@41.277.174.112 -i ~/.ssh/aws-key -v
```

### Export

```
$ pg_dump -h localhost -U databaseUserName -f dump.sql -d databaseName -p port -v
```

```
-h
host to connect to. Use localhost to connect to the ssh tunnel

-U
specify the databaseUserName that you want to connect to. This can be found on the RDS configuration page

-d
specify the databaseName that you want to connect to. This can also be found on the RDS configuration page

-p
specify the post to connect to. This is the port that the ssh tunnel is open on. In my case 1234
```

Example for AWS Elastic Beanstalk:


```
$ pg_dump -h localhost -U ebroot -f dump.sql -d ebdb -p 1234 -v
```

### Import

```
$ psql databaseName < dump.sql -h localhost -U databaseUserName -p port
```

```
-h
host to connect to. Use localhost to connect to the ssh tunnel

-U
specify the databaseUserName that you want to connect to. This can be found on the RDS configuration page

-d
specify the databaseName that you want to connect to. This can also be found on the RDS configuration page

-p
specify the post to connect to. This is the port that the ssh tunnel is open on. In my case 1234
```

Example for AWS Elastic Beanstalk:

```
$ psql ebdb < dump.sql -h localhost -U ebroot -p 1234
```

### References
* [https://medium.com/@michalisantoniou6/connect-to-an-aws-rds-using-an-ssh-tunnel-22f3bd597924](https://medium.com/@michalisantoniou6/connect-to-an-aws-rds-using-an-ssh-tunnel-22f3bd597924)

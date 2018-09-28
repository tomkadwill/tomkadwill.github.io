---
layout:     post
title:      "How to Debug a Docker Container in AWS that will not start"
date:       2017-12-04 20:54:00 +0000
---

Recently I've been using Amazon's ECS service which allows developers to run Docker containers. I had a task to add a new service to an existing ECS cluster (a group of containers). I made good progress setting up the new service, it was registered in the ECS cluster but the container was failing to start. ECS was continuously shutting down and restarting the service. Unfortunately, information was very limited. Viewing the failed task told me the command that had failed: `Command ["bundle","exec","foreman","start","-f","Procfile"]`. It also gave me an exit code, which was not very helpful. The problem was that I didn't know why the command was failing and didn't have any log output to help me.

After some searching I found a couple of commands to help me solve this problem.

```
$ docker ps -a
```

`docker ps` is the command used to view running containers. By default this command only lists running containers. If you pass the `-a` flag it will list all containers, including those that failed to start.

```
$ docker logs ID_OF_CONTAINER
```

With the ID of the failing container, you can use `docker logs` to fetch the logs.

Using these two commands I was able to identify and fix the problem quickly.

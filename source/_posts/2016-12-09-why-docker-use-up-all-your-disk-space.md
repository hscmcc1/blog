title: Why docker use up all your disk space?
date: 2016-12-09
tags: 
- docker
---

I noticed that the docker containers are using more and more disk spaces on host machine. By looking at the disk usage inside docker container, you will not see any disk usage increase. But the aufs file system is increasing.

I found out that there are two days we can use to clean up the space:

1. Restart docker service
2. Delete the docker container and add it back again.

But these ways need stop your service and we still don't know the root cause.

After a deep search, I finally found that the log file is eating my disk space and docker will not clean or limit the file size by default. My docker container has plenty of logs output to console, so the log file will keep increasing. If I restarted the docker service or recreate docker container, the log is clean up. Fortunately, docker provide us a [option](https://docs.docker.com/engine/admin/logging/overview/) to limit the log size:

```
--log-opt max-size=[0-9]+[kmg]
--log-opt max-file=[0-9]+
--log-opt labels=label1,label2
--log-opt env=env1,env2
```

I recreated my container using max-size option to limit the log size to 100M when running docker.

```
docker run .... --log-opt max-size=100M
```

By doing this, the problem is solved. You can find out more options for log on that page to manage your log.
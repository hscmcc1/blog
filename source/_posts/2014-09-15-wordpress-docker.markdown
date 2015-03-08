---
author: hesicong
comments: true
date: 2014-09-15 12:12:51+00:00
layout: post
slug: wordpress-docker
title: Wordpress on docker
wordpress_id: 3314
categories:
- 其他
tags:
- docker
- wordpress
---

I was thinking for a long time to move my blog back to Wordpress. I tried ghost blog but found it was at beginning stage and not mature to use, although the markdown feature seems very attractive. Now Wordpress can write blog using markdown too.

It took me a day to move it back. One reason is I couldn't import markdown to Wordpress easily. It was lucky that I had a full backup of the database at Feb, 2014. Another reason is that I'd like to try some new technology, yes, docker.

I have being using linux server for several years as playground. One major problem I faced before was after a long time running, the server was installed a lot of softwares that were not needed. "apt-get remove" could not remove all the files totally. I tried vagrant but it is at machine level and too heavy. Then I found docker. It is a tool to address this problem. Docker images could be removed or installed easily. You can even have a base image then install and remove any software without damage host.

Installation of docker is supported by most major linux operation, you can find them at docker's main site.
To install a wordpress on docker, it is quite easy.

``` bash
mkdir -p /docker/data/wordpress/db
mkdir -p /docker/data/wordpress/site
docker run --name wordpress-mysql -e MYSQL_ROOT_PASSWORD=mypassword -v /docker/data/wordpress/db:/var/lib/mysql -d mysql
docker run --name wordpress --link wordpress-mysql:mysql -p 80:80 -v /docker/data/wordpress/site:/var/www/html -d wordpress
```

After several minutes, it is done. You can visit localhost to setup the site.

NOTICE: you must mount the volume to your host's directory to keep data, otherwise all the data will lost after you delete container.

Enjoy.

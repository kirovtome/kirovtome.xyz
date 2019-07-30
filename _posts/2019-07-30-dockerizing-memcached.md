---
layout: post
title: Dockerizing a Memcached Server
date: 2019-07-30
description: You’ll find this post in your `_posts` directory. Go ahead and edit it and re-build the site to see your changes. # Add post description (optional)
img: memcached-logo.jpg # Add image post (optional)
tags: [Memcached, Docker] # add tag
---

Recently i saw a post about dockerizing Redis, so i've decided to do a basic tutorial on how to setup a Memcached using Docker.


### What is Memcached

Memcached is an in-memory key-value store. It is used for caching small chunks of data like strings, objects, usually a results from database calls, API calls, etc.  Memcached belongs to the NoSQL family of data management solutions.  

More about [Memcached](https://memcached.org).  

#### Prerequirements

* Docker

### Launch Memcached on Docker

1. Download the latest memcached docker image:  
```console  
docker pull memcached
```  
2. Start a memcached docker container with the following command:  
```console
docker run -itd --name memcached -p 11211:11211 memcached  
```  
3. Check the status of the container:  
```console  
docker ps -a  
```  
4. Login via telnet:  
```console  
telnet localhost 11211  
```  
5. Issue a *get* command:  
```console  
get weather  
```  
Result:  
```console  
END  
```  
6. Let's store something:  
```console  
set weather 1 600 4  
cozy  
```  
Result:  
```console  
STORED  
```  
7. Get the value back:  
```console  
get weather  
```  
Result:  
```console  
VALUE weather 1 4  
cozy  
END  
```  
8. There are other commands that you could try, like *stats*:  
```console  
stats  
```  
Result:  
```console  
STAT pid 1
STAT uptime 605
STAT time 1564474346
STAT version 1.5.16
STAT libevent 2.1.8-stable
STAT pointer_size 64  
```  
9. Exit by typing *quit*.  
10. Stop the docker container:  
```console  
docker stop memcached  
```  
11. Check the status of the docker container:  
```console  
docker ps -a  
```    
12. Remove the docker container:  
```console  
docker rm memcached  
```  

So, why Memcached?  
Memcached could be used in scenarios where we want to cache small and static data, like handling a high traffic website. It is multithreaded, can be scaled up by givint it more computational resources and can handle a lot of read operations as well.
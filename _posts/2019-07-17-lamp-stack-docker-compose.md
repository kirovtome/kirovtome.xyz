---
layout: post
title: LAMP stack with Docker Compose
date: 2019-07-16
description: You’ll find this post in your `_posts` directory. Go ahead and edit it and re-build the site to see your changes. # Add post description (optional)
img: docker-compose_logo.png # Add image post (optional)
tags: [LAMP, Docker, Docker Compose, CentOS, PHP, Apache, MariaDB] # add tag
---

Let's create a LAMP stack with Docker Compose.

### What is LAMP

LAMP is an acronym of the names of four open-source components: the Linux operating system, the Apache HTTP server,
the MySQL database and the PHP programming language, or simple *"Linux, Apache, MySQL, PHP"*.

It's usually used for creating websites and web apps.

More about [LAMP](https://en.wikipedia.org/wiki/LAMP_(software_bundle)).

### What is Docker

Docker is a tool for packaging the application and it's dependencies in a virtual containers that can run anywhere. These containers using OS-level virtualization, which means are more lightweight, faster with better perfomance that the classic virtual machines.

More about [Docker](https://www.docker.com).

### What is Docker Compose

Docker Compose is a tool for defining and running multi-container Docker applications, like LAMP stack for example.  

More about [Docker Compose](https://docs.docker.com/compose/overview/).

### Scenario

We are going to use Docker Compose to spin up a LAMP stack.

1. Download and install Docker. There are a tons of online documentation how to do that, so i'm going to skip this step. Maybe i'll create blog post in the future :)

2. Because we want to do this as simple as possible, we're going to use 2 Docker images with specific versions:  
* mariadb:10.2.8
* php7.1-apache:1.0  
Note:** It's not a good practise using the latest tag for public Docker images, because we have to track every change to that Docker image and test if it's compactible with the environment we are developing.**  

3. The pdo_mysql extension is not installed as default in the php7.1-apache image, so we'll create a Dockerfile that will use this image and add the missing extension.

<script src="http://gist-it.appspot.com/https://github.com/kirovtome/docker-lamp-stack/blob/master/Dockerfile"></script>

4. Now, we are going to build this Dockerfile.  
```console  
docker build -t php7.1-apache-pdo-mysql:1.0 .  
```  

If you execute *docker images*, you should see the Docker image.

5. Let's create and configure the *docker-compose.yml* file.
```console
touch docker-compose.yml
vim docker-compose.yml
```  

<script src="http://gist-it.appspot.com/https://github.com/kirovtome/docker-lamp-stack/blob/master/docker-compose.yml"></script>

As you can see, there are 2 services: *db* and *web* defined with images: *mariadb:10.2.8* and the Docker image we've just created: *php7.1-apache-pdo-mysql:1.0*.  
Next, we are using the environment file *.env* so, we could handle sensitive information, like mysql_root password, and it's also easier to name our database name as well :)  

This environment file looks like this:  

MYSQL_ROOT_PASSWORD=
MYSQL_DATABASE=happs

Rename the file to .env, enter your local root password for the mariadb instance and database name, and you are good to go.

Back to the *docker-compose.yml* file. We are mapping ports:  
* 3306:3306 (map local port 3306 to db docker port 3306)  
* 8000:80 (map local port 8000 to web docker port 80)  

For the db container, we are also mounting a docker volume, so we could have a persistent state of the database.
And regarding the web container, we are binding a local directory *./php* where we are developing our php app.  

*But Tome, what's the difference between binding and using docker volumes?*  

 Docker volumes are preffered way to store a persistent data, and are fully managed by Docker, where bind mounts can be file or folder stored on the container host and processes outside of Docker can modify it, like for example if we are developing application, it will be frustrating to rebuild the images on every application change.  

 And the last thing we got is *depends_on* which express dependency between these two services.  

 6. Start the containers in detached mode:
 ```console
docker-compose up -d
 ```

 7. Create sample index.php into the bind mount directory *./php/happs* in this example, and test the connection.
 ```console
 curl localhost:8000
 ```  

 Output:  
 <html>
 <head>
  <title>PHP Test</title>
 </head>
 <body>
 <p>Hello World</p> 
 </body>
</html>


Repository link: https://github.com/kirovtome/docker-lamp-stack

Cheers!!
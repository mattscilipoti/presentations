<!-- $theme: gaia -->

# [Developing On Docker](https://remarkjs.com/remarkise?url=https%3A%2F%2Fraw.githubusercontent.com%2Fmattscilipoti%2Fpresentations%2Fmaster%2Fdeveloping_on_docker.md)
-- matt@scilipoti.name

![](https://pbs.twimg.com/profile_banners/1138959692/1469204360/1500x500)

---

## Expectations

- Not a tutorial, designed to whet your appetite
- Better understanding:
  - Docker
  - some of the vocabulary
  - why we might be interested

---

## What is Docker?

- Docker is the world's leading software containerization platform.
  - a set of tools that manage Containers.  
  - an ecosystem around Containerization.  
  - also a shift in philosophy.

---

## What is Containerization?

- Essentially virtualization, hosting an Operating System on another Operating System, with a different architectural approach.
  - You can create **many lightweight Linux** virtual servers on your MBP.
- As developers, we will use them to **serve things we depend on**: postgres, redis, SOLR, ELK stack, logstash, etc.  
- **That's just the start**.  As we'll see, they open up a whole new world of possibilities.

---

## What are Containers?

Docker containers are lightweight, based on open standards, and secure by default.

Linux containers are self-contained execution environments -- with their own,
- isolated CPU
- memory
- block I/O
- network resources

-- sharing the kernel of the host operating system.

---

### It feels like a virtual machine, but sheds all the weight and startup overhead of a guest operating system.

- they startup almost instantly
- consume very few resources
- can create hundreds on a laptop

---

> In a nutshell,

>they both provide lightweight servers to run our stuff in isolation.

---

## Hello World, Docker style ヾ(⌐■_■)ノ♪

``` shell
$ docker run hello-world
Unable to find image 'hello-world:latest' locally
latest: Pulling from library/hello-world
c04b14da8d14: Pull complete
Digest: sha256:0256e8a36e2070f7bf2d0b0763dbabdd67798512411de4cdcf9431a1feb60fd9
Status: Downloaded newer image for hello-world:latest

Hello from Docker!
This message shows that your installation appears to be working correctly.
...
```

---

``` shell
...
To generate this message, Docker took the following steps:
 1. The Docker client contacted the Docker daemon.
 2. The Docker daemon pulled the "hello-world" image from the Docker Hub.
 3. The Docker daemon created a new container from that image which runs the
    executable that produces the output you are currently reading.
 4. The Docker daemon streamed that output to the Docker client, which sent it
    to your terminal.

To try something more ambitious, you can run an Ubuntu container with:
 $ docker run -it ubuntu bash

Share images, automate workflows, and more with a free Docker Hub account:
 https://hub.docker.com

For more examples and ideas, visit:
 https://docs.docker.com/engine/userguide/

```

---

## cowsay?

``` shell
$ cowsay quack
```

``` shell
 _______
< quack >
 -------
        \   ^__^
         \  (oo)\_______
            (__)\       )\/\
                ||----w |
                ||     ||
```

---

`$ docker run docker/whalesay cowsay "B'more on Rails"`

 ``` shell
 _________________
< B'more on Rails >
 -----------------
    \
     \
      \
                    ##        .
              ## ## ##       ==
           ## ## ## ##      ===
       /""""""""""""""""___/ ===
  ~~~ {~~ ~~~~ ~~~ ~~~~ ~~ ~ /  ===- ~~~
       \______ o          __/
        \    \        __/
          \____\______/

```
---

## Why do we care?

### Docker allows you to package your application into a standardized unit for software development. They liken it to [how shipping containers revolutionized shipping](https://www.linux.com/news/docker-shipping-container-linux-code).

Let me just show you: [Passport Scheduler](https://github.com/mattscilipoti/passport_scheduler#passport-scheduler)

---

## Native installation on Linux, OSX, & Windows
- No Virtual Host required
- No VirtualBox
- No VMWare
- No resource hogs

## NOTE!

Use the install instructions on the Docker website.  NOT homebrew.

---

## [Passport Scheduler](https://github.com/mattscilipoti/passport_scheduler#passport-scheduler)

1. build and run the container

  ``` shell
  $ docker-compose build
  $ docker-compose run app rake db:setup
  $ docker-compose up
  ```

2. Browse to http://localhost.

### Testing

```
$ docker-compose run app rake
```

---

## If you build it...

`docker-compose build`: [watch it](http://recordit.co/8LsdPShWKf)

This is creating containers:
1. for our rails app
2. for postgres

In isolation.  On their own linux's.

---

## Why/How?
- [Dockerfile:](https://github.com/mattscilipoti/passport_scheduler/blob/master/Dockerfile) configures our Rails app
- [docker-compose.yml:](https://github.com/mattscilipoti/passport_scheduler/blob/master/docker-compose.yml) Composes our Rails app and Postgres

---

## What do you think this does?

`docker-compose run app rake db:setup` -- [watch it](http://recordit.co/T1GAYzi68p)

That should look familiar to any rails developer.
But... it is NOT in my local postgresql.  It's in a container.

---

## Start our application...

`docker-compose up`: [watch it](http://recordit.co/UFlCWuDJQ7)

## Test it

`docker-compose run app rspec --color`: [watch it](http://recordit.co/7LGlQRwTOk)

---

# **None** of that is installed "locally".

- Each lives in isolation.
- No cross dependency headaches.

---

## No longer need to install...
- RVM
- `brew install postgresql` or "postgresapp"
- gcc/xcode dev tools
- apache/nginx
- elasticsearch
- logstash
- etc.

(theoretically)

---

# This image can be deployed.
### This _exact same_ image.
### The one you ran locally.
### And tested.  And tested.

---

Yes, _this_ image.

At least...

In _theory_.

## ~

How do we manage:
- configuration?
- persistent containers (postgres)?

---

## When do _I_ use Docker?

- I'm still playing.
- Only in Development.  
  - Makes it dead simple for other developers.
  - Or mothballed apps
- My team (and SysAdmins) aren't ready to use this in Production... yet.
- Using it for Test environment is... slow.
  - Thinking about using it only for services.  Run `rails server`, `rspec`, etc. locally.

---

## Where are we now?

- it's moving fast
- complexity has shifted
  - manage logs
  - when postgresql fails: `brew install` vs. `docker`
  - we have docker images, containers, and processes
  - we start to think about Dockerfiles, snapshots, images, etc.

---  

  > When you decide to move your apps to the docker-managed environments you'll realise that some of the approaches you were using don't work anymore - [Dmitry Tsepelev](https://medium.com/@AnjLab/how-to-set-up-elk-for-rails-log-management-using-docker-and-docker-compose-a6edc290669f#.kci5rkjv2)

---

## Security is a first-class citizen
  - [docker-bench-security](https://github.com/docker/docker-bench-security) is a script that checks for dozens of common best-practices around deploying Docker containers in production.
  - hub will run this across all your servers... in production.
  - New tools to update your containers.

---

## Fun stuff!

- [nut](https://github.com/matthieudelaro/nut): the development environment, containerized.
  > Provided that you use Docker, you don't need to install anything on your computer. Not even Go!
- [awesome-sysadmin](https://github.com/kahun/awesome-sysadmin)
- Docker provides great tutorials.

---

# Next steps

- [Running a Rails Development Environment in Docker](https://blog.codeship.com/running-rails-development-environment-docker/).
- [Dockerfile best practices](https://docs.docker.com/engine/userguide/eng-image/dockerfile_best-practices/)

### Tips

- Use the install instructions on the Docker website.  NOT homebrew, for now.
- Don't get hung up on which OS will the base for each Container (Ubuntu, Ubuntu LTS, debian:wheezy, ruby:2.3.1, ruby-slim, alpine)
- If you have good seed data (for dev, demos, maybe testing) you can drop and recreate the postgresql container quickly and easily.
- Remove an image via `$ docker rmi <image-id_or_name>`
- Someday maybe: https://phusion.github.io/baseimage-docker/

---

## Follow up

- This presentation: [https://github.com/mattscilipoti/presentations](https://github.com/mattscilipoti/presentations/blob/master/developing_on_docker.md)
- matt@scilipoti.name
- github: [mattscilipoti](https://github.com/mattscilipoti)

## Thanks to:
- [Docker](https://www.docker.com/)
- [RemarkJS](https://remarkjs.com): a simple, in-browser, markdown-driven slideshow tool.
- [B'More on Rails](http://bmoreonrails.org/)

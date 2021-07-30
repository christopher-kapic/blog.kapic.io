---
title: How to deploy a Redis instance
date: 2021-06-19T08:30
thumb: redis_netlify_insecure.png
tags: 
    - Redis
    - VPS
    - Vultr
---

For part of my development of [TickerTab](https://tickertab.io/), I need to create an instance of [Redis](https://redis.io/), an in-memory key:value database for caching. Initially, when I created TickerTab, I had a NodeJS API, but I am refactoring to use Netlify's serverless functions. Since my functions are serverless, I cannot cache on the same server that the functions are run on (like I could with NodeJS), so I need a separate server.

This is how to set up an independent instance of Redis.

_Note: I don't know what I'm doing when it comes to security—if you know something I don't, send me an [email](mailto:christopherkapic@gmail.com?subject=REDIS_INSECURITY). Thanks!_

_Another note: I now have better ideas about how to handle caching for Netlify. Keep an eye out for another article about a better method._

## 1. Create a VPS

I like to use [Vultr](https://www.vultr.com/?ref=8752906) (_referral link_), and I decided to go with Ubuntu 18.04.

## 2. SSH into the VPS

Then run

    apt update; apt upgrade

## 3. Install Redis

Run

    sudo apt install redis-server

## 4. Update the configuration

Run

    sudo vim /etc/redis/redis.conf

And change the following:

```bash
# /etc/redis/redis.conf
# 
# previously:
# supervised no
# now:
supervised systemd

# previously:
bind 127.0.0.1 ::1
# now:
# bind 127.0.0.1 ::1
# NOTE: Generally, this is a big security risk,
# but since we are using serverless functions we
# don't know which IP address we will receive a
# connection from.

# previously:
# requirepass foobared
# now:
requirepass YOURSUPERSTRONGPASSWORD
# NOTE: Use a long password. If you don't, Redis
# is fast enough that a brute force attack would
# not be very difficult.
```

## 5. Run Redis

    systemctl restart redis.service

And that should do it. I think. If you have problems, [email](mailto:christopherkapic@gmail.com?subject=Redis_Question) me.
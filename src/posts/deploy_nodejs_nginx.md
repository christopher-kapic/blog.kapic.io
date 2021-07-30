---
title: How to deploy a NodeJS server with NGINX on a VPS
date: 2021-07-01T23:22
thumb: node_redis_deployment.png
tags: 
    - NodeJS
    - NGINX
    - VPS
    - Vultr
    - Redis
---

This is how I (currently, as of July 2, 2021) deploy NodeJS servers. Eventually I will figure out Docker, but for right now this is good enough. This tutorial also includes information for using [Redis](https://redis.io/) on the same VPS for caching.

## 1. Prepare a VPS
Create a VPS (I use [Vultr](https://www.vultr.com/?ref=8752906)–that's my referral link). I use Ubuntu; if you use another distro you will have to use your distro's package manager for installing software.

You'll probably want a domain name anyways, so point a domain (I will use example.com for the tutorial) at the VPS.

SSH into the VPS:

    ssh root@example.com

Update the machine:

    sudo apt update; sudo apt upgrade

Install programs that will be useful:

    sudo apt install nginx redis-server python3-certbot-nginx

Install a text editor (I prefer Neovim):

    sudo apt install neovim

## 2. Edit your NGINX files

    nvim /etc/nginx/sites-available/example.com

In the `proxy_pass` value, the port should be whichever port you plan to run your NodeJS server on.

```nginx
server {
    listen 80;
    server_name example.com www.example.com;
    location / {
        proxy_pass http://localhost:3000/;
    }
}
```

Run

    sudo ln -s /etc/nginx/sites-available/example.com /etc/nginx/sites-enabled

## 3. Configure Redis

Edit your `redis.conf` file:

    nvim /etc/redis/redis.conf

Find the `supervised` key; set the value to `systemd`.

Restart Redis:

    sudo systemctl restart redis.service
    sudo systemctl restart redis

## 4. Install NodeJS (via `nvm)

    curl -o- https://raw.githubusercontent.com/creationix/nvm/v0.33.11/install.sh | bash

Exit your SSH session and start a new one:

    ssh root@example.com

Install NodeJS:

    nvm install v15.11.0

## 5. Clone your repo

    git clone https://probablygithub.com/yourusername/yourrepo.git

## 6. Start your server

Change directory into your server directory:

    cd yourrepo.git

Install your packages:

    npm install

Install `pm2` globally:

    npm install pm2 -g

Start your server (change `server.js` to the path of your main file):

    pm2 start server.js
    pm2 startup
    pm2 save
    sudo reboot

## 7. Configure SSL

Use LetsEncrypt:

    sudo certbot --nginx -d example.com -d www.example.com

Enter the required information, and soon enough you will have SSL for your server.

Congrats! You have a deployed NodeJS server with Redis for caching and SSL through LetsEncrypt!

## Other considerations

You may want to create a different user so you are not running the server as `root`.

You may want to use `ufw` for added security. I would reference Brad Traversy's [deployment strategy](https://gist.github.com/bradtraversy/cd90d1ed3c462fe3bddd11bf8953a896).

If this tutorial is broken at any point in the process, let me know via [email](mailto:christopherkapic@gmail.com). Thank you!
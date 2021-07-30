---
title: Install Nextcloud with one click on Vultr
date: 2021-06-17T11:13
thumb: gatsby_netlify.png
tags: 
    - Vultr
    - Nextcloud
---

I wanted to get started with [Nextcloud](https://nextcloud.com/), an open source cloud alternative, but I wanted to save myself from figuring out all of the tiny details, so I decided to create a Nextcloud server with [Vultr](https://www.vultr.com/?ref=8752906) (_referral link_). The only problem is that Vultr's documentation about setting up Nextcloud was not up to date. So here is how I did it.

Missing steps:
 - apt install python3-certbot-nginx
 - certbot --nginx -d your.domain.com
 - install python3-pip
    sudo apt install python3-pip
 - 
---
title: There is a better alternative for serverless caching
date: 2021-07-14T23:21
thumb: serverlessredis.png
tags: 
    - Redis
    - Serverless
    - Upstash
---

The other day, I was really excited because I thought of a great idea for a service: I was going to create a SaaS that handled caching for serverless applications (targeting Netlify specifically, but other platforms could use it as well). In my mind, serverless caching was the one thing that was still lacking from modern serverless stacks, and it was a problem that I needed to solve for [TickerTab](https://tickertab.io). I wrote an [article](/grow_netlify_app_with_a_private_api) about how I planned on solving this problem, and funnily enough, I will probably be changing my solution.

Unfortunately for me (because I wanted to build this business idea), there already is a company providing serverless caching. Enter [Upstash](https://upstash.com), a serverless database for Redis. Upstash has a free tier for up to 10,000 daily commands, which seems amazing, and after that it is only $0.02/100,000 commands. If you want persistance, it is $0.25/GB/month. Not bad.

Technically speaking, Upstash probably has the insecurity problem I mentioned in my original article about working with a serverless cache, but it still might be worth using for the sake of simplicity. If I choose to migrate TickerTab, my new stack will be:
 - Netlify (Hosting, Identity, Functions) - Free until 1,000 MAU, then $100/month
 - Stripe (Billing) - 2.9% + $0.30 per successful charge
 - [EOD Historical Data](https://eodhistoricaldata.com/) - $40/month (for the data necessary for TickerTab)
 - Upstash - Free until >10,000 daily commands
 - Chat server - Starting at $10/month, hosted on [Vultr](https://www.vultr.com/?ref=8752906), and scaling as needed (manually)
 - Mailgun - I am not actually sure if I will need this for a pure operational cost, but I anticipate using it to send receipts for Stripe transactions.


In which case I will launch with an operational cost of $50/month until TickerTab grows large enough to have 1,000 MAUs (which will bump me up to $150/month).

The only other thing I might consider switching is my hosting provider. When I started developing TickerTab, Netlify seemed like the obvious choice, but as I have learned more, it might be the case that [Cloudflare](https://www.cloudflare.com/) is better. If I were to switch to Cloudflare for hosting and functions, I would use [Supabase](https://supabase.io) for authentication. However, I probably will not switch; I mostly just want to be done developing TickerTab since I have worked on it for so long.

To be honest, I still may try to compete with Upstash because I think there is a lot of opportunity in the market, but I will likely try my other ideas first. I still need to finish working on TickerTab, and I have another idea that could be _really good_.
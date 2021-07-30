---
title: How to authorize requests for a private API behind a Netlify app
date: 2021-07-05T08:17
thumb: netlify_scaling_and_auth.png
tags: 
    - Netlify
    - API
---

[Netlify](https://www.netlify.com/) is great for developing and hosting static sites. Their authentication service is straightforward relative to other options I have tried (you can get authentication in no time if you are willing to use their identity widget, which is not beautiful, but is good enough for prototyping). They offer cloud functions.

Long story short, Netlify makes it easy to get a lot of functionality for a website without dealing with many of the hurdles that developers had to deal with in the past.

However, one thing Netlify does not have an obvious solution for is authorizing requests for private APIs (unless you're on the business plan, in which case, just get your JWT and decode it as middleware on the API). For instance, I wrote an API that requires caching requests. Obviously, with a serverless framework, I cannot count on a Redis instance being available across multiple invocations of a function. Perhaps you saw my [previous article](/how-to-deploy-a-redis-instance) in which I propose a way to handle this situation (in not the most secure way—I was desperate). Now, I have four ways to handle this situation. There is also a [video](https://youtu.be/BxCbAXpgGVk) in which I explain the concepts, if you would prefer that.

## Method 1 (not that great)

![Option one](/assets/img/netlify_cache_1.png "Serverless function to decode JWT")

This method was my naive first choice when I started development of [TickerTab](https://tickertab.io/). It is bad for a couple of reasons:
 - Sometimes it did not work (I do not know if this was my fault or Netlify's)
 - If you are going to invoke a serverless function, you might as well do it as outlined in method 3.


## Method 2 (not that great)

![Option two](/assets/img/netlify_cache_2.png "Open cache")

This is the method I described in my [previous article](/how-to-deploy-a-redis-instance). It is bad because Redis is super fast, so a brute force attack is possible. Use a long password if you do not want to be hacked, or use a better method.


## Method 3 (better—and can be free)

![Option three](/assets/img/netlify_cache_3.png "Forward requests through serverless function")

This is the best solution I have if you want to stay on the free plan. It is essentially the same strategy you would use to authenticate requests to a third-party API, and you can do caching (or any other fancy things that require a true server) on a persistant machine.


## Method 4 (best, but paid)

![Option four](/assets/img/netlify_cache_4.png "Get the business plan")

Since Netlify Identity uses JWTs, the best solution is to get your secret key and decode the JWTs on your server. This option, however, requires the business plan, so it is fairly expensive. If you are just starting, I would suggest you use method 3, and once you can afford the more expensive plan, switch to method 4. You might have noticed that they are structually quite similar, and switching should require only a minimal amount of refactoring.
---
title: My Thoughts on SuperTokens
date: 2021-08-12T08:15
thumb: pending.png
tags: 
    - React
    - create-react-app
    - JWT
    - GoTrue
    - Authentication
    - SuperTokens
---

Authentication has been a topic of fascination for me over the last several months. I have been researching all of the available options that I can find. If you follow me on [YouTube](https://www.youtube.com/channel/UCuXgDzDJhNAwvzvc62GnYwA), then you might have seen my [video](https://www.youtube.com/watch?v=kPK4Cra_I5I) about authentication methods. Since creating that video, I stumbled upon [SuperTokens](https://supertokens.io), and I have to say... I am impressed.

That being said, SuperTokens is for a specific use case—web apps created with `create-react-app`. Technically, it works with other libraries as well, but I think other options are better for projects built with Gatsby, other JavaScript frameworks, or native applications—At least for now (I am actually a contributor for SuperTokens, and in the future I plan on working on bringing ease-of-use to Gatsby and maybe Expo for native applications).

SuperTokens is good for several reasons:
 - It is simple to set up (with `create-react-app`).
 - It is open source.
 - You can easily self-host it (I'm working on the CapRover One-Click App).
 - It integrates with your Node.js API.

Some of the (current) drawbacks of SuperTokens include:
 - There are not supported SDKs for other frameworks.
 - There are not supported SDKs for other backends.

Keeping in mind these limitations, this is the _perfect_ use case for SuperTokens:

**SaaS web application built with CRA on a subdomain**

Let me explain.

You probably do not want to use `create-react-app` for a landing page because you want to optimize SEO. So instead, you choose Next.js or Gatsby. But those are annoying to build apps in, so you use `create-react-app` for the app itself. Host the app on `https://app.yourdomain.com` and the landing page on `https://yourdomain.com`. Then **boom**. Easy authentication with better SEO. And your authentication is not bound by vendor lockin because you can selfhost it.

This is my current strategy for TickerTab.
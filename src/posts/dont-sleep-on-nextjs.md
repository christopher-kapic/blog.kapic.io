---
title: Don't Sleep on NextJS
date: 2021-08-27T11:00
thumb: nextjs.png
tags: 
    - React
    - NextJS
    - Vercel
---

[NextJS](https://nextjs.org/) is a React framework created by [Vercel](https://vercel.com/). When I first heard about it, I was consistently using [Gatsby](https://www.gatsbyjs.com/) for my projects, and sometimes [Create React App](https://create-react-app.dev/) when I needed client-side routing. Ben Awad made some videos about [Gatsby and NextJS](https://www.youtube.com/watch?v=VoscwJ6MGsU), and when I first saw them, I thought I was fine with Gatsby--I could not justify using Next.

However, upon recent exploration of the NextJS documentation, the framework is more powerful than I gave it credit for. Like Gatsby, Next uses a [pages](https://nextjs.org/docs/basic-features/pages) structure, assuming that each page should be statically generated. This is good in a world where SEO is king and you want to ensure that your content ranks well with the algorithms. If, however, you want to use server-side rendering, [you can easily do that](https://nextjs.org/docs/basic-features/data-fetching#getserversideprops-server-side-rendering). Furthermore, in order to get the speed of navigating between pages associated with a client-side router, Next prerenders pages with its custom [`<Link/>`](https://nextjs.org/docs/api-reference/next/link) tag. Essentially, in doing all of these things, NextJS has the features you might want from Gatsby _and_ Create React App! The only obvious problematic situation is if you are serving an application to users with slow internet; Create React App _might_ outperform NextJS when it comes to page navigation, but realistically... it probably isn't a good idea to ship React to users with slow internet anyways.

One other consideration is that you might not be able to use Create React App specific tools. One that comes to mind is [SuperTokens](https://supertokens.io/), whose React SDK was designed specifically around the standard Create React App router.

One last consideration for NextJS is what to use for a hosting solution. Since NextJS was developed by Vercel, integration with the platform is good. Netlify also has a solution for hosting NextJS applications, although I need to take a closer look at it to see how good it is. You can also deploy with Node servers with something like Kubernetes.

All of that being said, I plan on switching to NextJS for my personal projects. Eventually, I plan on migrating my blog from Eleventy to Next.

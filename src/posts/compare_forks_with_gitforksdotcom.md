---
title: Use Gitforks.com to Compare Forks of a Repo
date: 2021-07-27T13:47
thumb: github_gitforks.png
tags: 
    - Github
    - Open Source
    - React
    - Netlify
---

One thing that I have wanted to do several times is compare forks of a repository on Github to see who was actually making changes to a repository and who only forked a repository because they thought it would be a fun thing to do. Unfortunately (to the best of my knowledge), Github does not make this easy. In order to see how many commits ahead a repository is, you need to go to the webpage for that repository.

What I wanted was a quick list, where I could simply scan over it and see which forks had changes. So I built that.

Enter [Gitforks](https://gitforks.com/), a simple website written in React and hosted by Netlify (CDN and one serverless function). The website is _really_ simple, but it does what I need it to do: display forks and their commits ahead/behind.

As it stands now, it is not perfect. If I wanted to get fancy, I would add authentication and caching so I do not have to worry about hitting the Github API's rate limit. However, I have better things to do, and it works well enough. If I am honest, my goal is mostly just to have this tool for myself, although I think it would be cool if someone at Github saw this and added it as a feature to the site (please, do this; many of us would find it useful). I also hope that it might be useful to others, so give it a shot and let me know what you think.

I am also open to having open-source contributions. If you want to make an open source contribution, check out the [repo](https://github.com/christopher-kapic/gitforks). You can implement caching (this should not be that difficult) or authentication (I do not know how difficult this is for Github OAuth), or anything else that you think would make for a good addition to the site.

And for those who just find it a cool idea, I would love if you simply gave the [repo](https://github.com/christopher-kapic/gitforks) a star. Thank you!
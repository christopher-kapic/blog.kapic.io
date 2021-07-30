---
title: The case for a CapRover Instance
date: 2021-07-03T23:33
thumb: why_caprover.png
tags: 
    - CapRover
    - Docker
    - Containers
---

If you have not heard of [CapRover](https://caprover.com/), it is a self-hosted platform-as-a-service designed to simplify deploying containers; that is well and good, and perhaps you will actually use CapRover at some point to deploy something to production. But the real value is using it as a playground.

CapRover offers a list of one-click-apps, and unlike choosing from a selection of one-click-apps from a cloud VPS host, you do not need to provision a new machine for each app you deploy. Deployment is super easy, and if there is not a one-click-app available, you can submit a Dockerfile to create your app.

CapRover also offers _ridiculourly easy_ SSL deployment, so all of your self-hosted one-click-apps can have valid HTTPS certificates with minimal work on your end.

![One click apps](/assets/img/one-click-caprover.png "One click apps")

For context, here are some of my favorite one-click-apps available in the main [CapRover Repository](https://github.com/caprover/one-click-apps):

 - [Ackee](https://ackee.electerious.com/) - a self-hosted, privacy-respecting analytics tool. (An alternative is [Plausible](https://plausible.io/), but I have not been able to get the self-hosted version working. If you are willing to pay, their SaaS option is actually pretty good... and I'm not getting paid to say that.)

 - [code-server](https://github.com/cdr/code-server) - VS Code in your browser. There are a couple quirks, but it is _super helpful_ to have an accessible (from any computer with a web browser) IDE. Just try not to let yourself get lost in the weeds of maintaining any weird settings.

 - [Firefly III](https://www.firefly-iii.org/) - personal finance manager. To be honest, I have yet to spend much time figuring this one out, but it seems promising, and it also seems to be the most popular based on online forums.

 - [OnlyOffice](https://www.onlyoffice.com/) - Office suite in the browser. Fairly similar to Microsoft's products in functionality, but in your browser, and open source (not proprietary through Google).

Overall, there are still more one-click-apps for me to explore. There are also a couple that I have had trouble with... If you want Mastodon, do not host it with CapRover (as of July, 2021).

At the very least, CapRover will offer a little fun. Give it a shot.

Thanks, and you're welcome.
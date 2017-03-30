---
layout: post
title:  "Draft: Deploying a Discord Bot to AWS"
date:   2017-03-12 18:03:47 -0600
description: Keep your custom bot running 24/7 on a free ec2 instance
categories: nodejs
---



## Deploying to a free EC2 Instance on Amazon Web Services

Want your bot to be constanly online, but don't have a machine you can keep 
running 24/7? One option is to deploy a ec2 instance on AWS, which is included
in the free tier.


We will use forever to start our bot as a background process. Since
we'll be calling it from the command line be sure to install it with
the global flag:

```
npm install forever --global
```

Next, create a file named `startbot.sh` and add the following text:


```bash
#!/bin/bash
forever ~/demobot/main.js -o bot.log -e boterr.log
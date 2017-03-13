---
layout: post
title:  "Building a Discord Bot with Node.js"
date:   2017-03-10 18:03:47 -0600
description: Add some custom functionality to you discord server with node.js
categories: nodejs
---

This post will cover every step needed to get up and running with your own Discord bot. 

*This tutorial assumes minimal knowledge of node.js. All of the code 
used can be found in [this repository](www.google.com).*



### Creating a Discord Application

First you'll need to create a Discord app. Head over to 
[this site](https://discordapp.com/developers/applications/me) and create a new
app.



### Configuring our Node.js Project

Head over to the directory for your project and run `npm init`.
Next we'll install the npm packages needed for this project:
```
npm install --save discord.js@11.0.0 request@2.81.0 cheerio@0.22.0
```


### Adding Functionality to the Bot

```js
var woo

console.log("those are some fine cabbages")
```


### Deploying to AWS free EC2 Instance

If you want your bot to be constanly available but don't currently have
a machine you can keep running 24/7, deploying 
small ec2 instance
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
```

---
layout: post
title:  "Building a Discord Bot with Node.js"
date:   2017-03-27 18:03:47 -0600
description: Add some custom functionality to you discord server with node.js
categories: nodejs
---

This guide will cover every step needed to get up and running with your own Discord bot, 
aimed at anyone who is new to Node.js or just unsure of where to start.  


## Prerequisites

Since we'll be using Discord.js as our library for interacting with the Discord API, you
will need to have at least Node.js 6.0.0 or higher. If you are unsure of what version is 
currently installed running `node -v` will let you know.

This tutorial assumes minimal knowledge of node.js. All of the code 
used can be found in [this repository](https://github.com/metasloth/discord-nodejs-demo).



## Creating a Discord Application

First you'll need to create a Discord app. Head over to 
https://discordapp.com/developers/applications/me and choose "New App". 
Once your app is created, you'll want to click "Create App Bot User". 
This will give your bot a username and allow you to add it to servers.

Since Discord servers are free and easy to make, I recommend making one 
specifically for testing the bot. Once you do this, you can add the bot to 
your server by replacing "YOUR_CLIENT_ID" with the Client ID of your bot in 
the following url:
```
https://discordapp.com/oauth2/authorize?client_id=YOUR_CLIENT_ID&scope=bot&permissions=0 
```

Finally, in the App Bot User section, click to reveal your token. This 
token is used as the authentication for you bot to log in, so you'll want 
to keep it secret. If you are using git and want to keep it hidden in a 
public repo, store it in a file named `secret.json`, which should look 
something like this:
```js
{ "token": "YOUR_BOT_TOKEN" }
``` 
Then in your `.gitignore` file you can add `secret.json` on it's own line,
which will prevent git from staging the file. Using a json file will make 
it super simple to reference our token later on.



## Configuring our Node.js Project

Head over to the directory for your project and run `npm init` to populate
a basic `package.json` file. Next we'll install the npm packages needed 
for this project:
```
npm install --save discord.js@11.0.0 request@2.81.0 cheerio@0.22.0
```


Create a file named `main.js`, and add the following lines of code:

```js
const secret = require('./secret.json')
const Discord = require('discord.js')
const client = new Discord.Client()

client.on('message', msg => {
    if (msg.content === '!hi') {
        msg.reply('Hello there!')
    } 
})

client.on('ready', () => {
    console.log(`${client.user.username} is ready to chat!`)
})

// This will let us know of any API errors encountered
client.on('error', error => console.log(error))

client.login(secret.token)
```

We can now start our bot with `node main.js`, and if everything is working properly
you should be off to a riviting conversation with your bot.



## Conclusion

Hopefully this serves as a starting ground for anyone new to node.js and struggling 
to get up and running. If you run into trouble as you add more complexity to your bot, 
Discord.js has a pretty active Discord server wher you can usually get quick help!

I'll be adding a bit more depth to this guide shortly, including deploying your bot
to a free AWS ec2 instance so your bot can be running 24/7. 


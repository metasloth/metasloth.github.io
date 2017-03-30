---
layout: post
title:  "Making high quality gifs with FFmpeg"
date:   2017-03-29 18:03:47 -0600
description: Who needs 
categories: gifs
---

It’s quite impressive how prevalent gifs are in internet, considering the file format was introduced 30 years ago. That being said the lossless file format is just as inefficient as ever, and still a complete pain to work with. 



It’s also clear that not all gifs are created equal. Tumblr’s 1MB limit for uploading size made it notorious for low resolution, borderline still image gifs like this:
<div class="vidcenter">
<a href="http://imgur.com/TP9pRHC"><img src="http://i.imgur.com/TP9pRHC.gif" title="source: imgur.com" /></a>
</div>


Whereas browsing /r/highqualitygifs on reddit reveals masterpieces like this 41 second, 624 pixel wide piece of art:
<div class="vidcenter">
<video preload="auto" autoplay="autoplay" muted="muted" loop="loop" webkit-playsinline="">
                <source src="//i.imgur.com/E0l6vsB.mp4" type="video/mp4">
</video>
</div>

## The problem

You’d think a format introduced 30 years ago would be a breeze to work with at this point, but if you’ve ever tried to make one you were probably caught off guard by how frustrating the process is. Sites like gfycat and imgur offer to convert your videos to gifs, but there is a 10 second limit and ambiguous resolution changes. Photoshop is the most common choice for creators at /r/highqualitygifs, but that is a pretty expensive investment just to up your karma on reddit. 

A while ago I found [this post](http://blog.pkh.me/p/21-high-quality-gif-with-ffmpeg.html) which provided great insight into the gif’ing capabilities of FFmpeg, an open source, command-line video conversion program. I boiled down the essentials to a couple of bash scripts and creating gifs went from waiting on hoping Photoshop wouldn't run out for RAM to a painless ~60 second process. 

## Creating gifs with FFmpeg

Assuming you have a source video that’s too long, FFmpeg can losslessly shorten it for you almost instantly with most video formats. All you have to do is update the variables in this script and voila!

```bash
#! /bin/bash

# Remove spaces from filenames!
input="input.mp4"
output="shortened.mp4"

# Timestamps must be in HH:MM:SS.xxx format
timein="00:01:29.0"
length="00:00:16.0"

ffmpeg -i $input -ss $timein -c copy -t $length $output

echo "Finished."
```

Check to make sure your video looks good, then this next script should take you home:

```bash
#! /bin/bash

input="shortened.mp4"
output="output.gif"
palette="./tmp/palette.png"

# Update the fps and width here:
filters="fps=30,scale=720:-1:flags=lanczos"

echo -ne  "Creating palette..."
ffmpeg -v warning -i $input -vf "$filters,palettegen" -y $palette

echo -ne "\r\033[KCreating gif..."
ffmpeg -v warning -i $input -i $palette -lavfi "$filters [x]; [x][1:v] paletteuse" -y $output

echo " Finished."
```

Typically this process takes around a minute, but if you’re attempting a particularly long gif you may be waiting longer.


You can see the performance it has with high color variance and heavy animation in this example 
<div class="vidcenter">
<video autoplay="" loop="" style="max-width: 100%; min-height: 360px;"><source type="video/mp4" src="//i.imgur.com/6JRoICb.mp4"></video>
</div>


As well as the clarity when using less complicated color pallets and lower frame rates:
<div class="vidcenter">
<video autoplay="" loop="" style="max-width: 100%; min-height: 409.5px;"><source type="video/mp4" src="//i.imgur.com/oCrupnw.mp4"></video>
</div>

## Conclusion
Keep in mind the raw `.gif` file you create is usually larger than the source video itself, so I recommend using imgur to host your gifs. They convert all gifs to HTML5 videos, which are a fraction of the size. This will keep mobile users happy as long as your remember to share the link ending in `.gifv`.

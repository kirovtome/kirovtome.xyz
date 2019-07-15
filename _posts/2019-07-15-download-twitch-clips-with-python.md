---
layout: post
title: Download Twitch clips with Python
date: 2019-07-15 +1500
description: You’ll find this post in your `_posts` directory. Go ahead and edit it and re-build the site to see your changes. # Add post description (optional)
img: twitch_logo.jpg # Add image post (optional)
tags: [Python, Python3, Twitch] # add tag
---

As a casual Dota2 twitch viewer, i always wanted to save and/or upload some twitch highlight clips to YouTube.
So, i decided to create a basic Python script that will download the Twitch clips locally.

In order to do so, we'll have to install python3 and create a twitch user Client-ID.
There are a lot of tutorials on how to install Python3, so i'll just skip this step.

### Create Twitch user Client-ID

1. We need to register a Twitch account from [here](https://www.twitch.tv/signup).  
2. Go to the [Twitch dev site](https://dev.twitch.tv) and log into your account.
3. Go to your **Dashboard**.
4. On the **Console**, under **Applications**, choose **Register Your Application**.
5. On the **Register Your Application** page, complete the form and choose **Register**.  
![](/assets/img/screenshots/screenshot1.png)
6. Note the Client ID that you will use to configure the script.

### Configure the script
1. Download or clone the repository from [here](https://github.com/kirovtome/python-twitch-clips).  
2. Extract the repository.  
3. Insert the Twitch Client ID into the script by changing the variable 'client_id' value.  
4. Copy the Twitch clip URL.  
5. Execute the script by running the following command
```console
python3 dltwitchclips.py --clip <paste_clip_url_here>
```  
Note: **Make sure to install the Python3 modules before running the script.**  
6. The clip should we downloaded in the tmp/ relative directory to the script.  
Note: **The script will try to create a tmp directory if it's not present.**


This tutorial is for one clip only. In the next posts, i will present a full automated solution for the top watched clips.
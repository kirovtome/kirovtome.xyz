---
layout: post
title: YouTube subscriber count with Python
date: 2019-07-24
description: You’ll find this post in your `_posts` directory. Go ahead and edit it and re-build the site to see your changes. # Add post description (optional)
img: youtube-subs.jpg # Add image post (optional)
tags: [python, python3, youtube, subscribers, subs] # add tag
---

Today, i'm going to create a python script that will count YouTube subscribers for a given channel. Plain and simple.  

### Create YouTube API Key

1. Create a [Google account](https://accounts.google.com/signup).
2. Go to the [Google API Console](https://console.developers.google.com/cloud-resource-manager) and create a project by clicking **Create Project**, enter a name and click **Create**.
3. Next, go to [API & Services](https://console.cloud.google.com/apis) and click **ENABLE APIS AND SERVICES**.
4. In the search bar type *YouTube* and click on the first result *YouTube Data API v3*.
5. Click **ENABLE**.
6. Now, back to the [API & Services](https://console.cloud.google.com/apis) and from the left panel click *Credentials*.
7. Click *Create credentials*, *API key*.
8. On the popup window **API key created**, copy the key and save it somewhere.
9. Click **CLOSE**.

Official documentation: https://developers.google.com/youtube/v3/getting-started


### The Python script

#### Requirements

* Python3
* Google Account
* Google API key

#### Instructions

1. Save the following script as a python script. For example: *get-subs-number.py*
{% gist bfb80a64f23c78a5ba308e2df9c29d06 %}  
2. Insert the created Google API key into the *key* variable of the script.
3. Before running the script make sure that you have installed the Python3 modules.
4. Run the script by executing the following command. For example:  
```console
python3 get-subs-number.py --user pewdiepie  
```  
Result:  

![](/assets/img/screenshots/screenshot2.png)


### Improvements

Create a live counter.  
For example: Schedule API requests every 60 seconds and display it with fancy numbers. I'll do this scenario in the near future with a serverless AWS Lambda function.  

Cheers!!
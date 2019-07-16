---
layout: post
title: AWS Cloudfront automated deployment using Bitbucket Pipelines
date: 2019-07-16
description: You’ll find this post in your `_posts` directory. Go ahead and edit it and re-build the site to see your changes. # Add post description (optional)
img: bitbucket_logo.jpeg # Add image post (optional)
tags: [CI, CD, Bitbucket, Pipelines, Docker, AWS, Cloudfront, S3] # add tag
---

Today I'm going to talk about automating the AWS Cloudfront deployment using Bitbucket Pipelines with Pipes.

### What is AWS Cloudfront

AWS Cloudfront is a Content delivery network (CDN) service that delivers content like videos, images, applications to
Amazon Edge locations with low latency, high speed, etc,...  

*"But Tome, we got S3 buckets for uploading videos, images, serving static sites?!"*

Yeah sure, but you can't configure TLS sertificates if you want to serve static site in S3 yet, and you don't wanna public access to the bucket either because of the new Magecart malware :)

There is no 'but', configure CDN it's a one minute configuraiton.  
More about [AWS Cloudfront](https://aws.amazon.com/cloudfront/).

### What is Bitbucket Pipelines

Bitbucket Pipelines it's a CI/CD service, built into the Bitbucket that allows you to automatically build, test and deploy your code, based on a configuration file.

More about [Bitbucket Pipelines](https://bitbucket.org/product/features/pipelines).

### What is Bitbucket Pipes

Bitbucket Pipes are actually a Docker containers which you can use within your Bitbucket Pipeline.
There are a lot of Pipes [available](https://confluence.atlassian.com/bitbucket/build-test-and-deploy-with-pipelines-792496469.html), especially for AWS, Azure and Google Cloud Platforms.

### Scenario

For every merge in develop, i want to automatically deploy code to the S3 bucket and create AWS Cloudfront invalidation.

1. While using the docker image: node:10.15.3, execute the following commands in the first step:
```console
npm install
npm run build
```

2. Deploy to S3 using the docker image: atlassian/aws-s3-deploy:0.2.4.  
Note: **use Bitbucket environment variables located in Settings -> Repository variables**.

3. Create Cloudfront invalidaiton using docker image: centos:7 and install on the awscli on the fly. Although this could be any docker image, the best way to deploy a cloudfront invalidation is to have your own docker image, and use that image. Also, it could be nice if you don't mind extra 30 seconds of the deployment time.

{% gist fe7c323cde852117c4698effe3eade49 %}


### Improvements

1. Generalise each step using the **&** sign, so we could reuse the steps, create Bitbucket environments and deploy different branches on different environments.  
For example deploy:  
* develop branch -> dev environment
* master branch -> master environment  
  
But there is an issue regarding executing multiple steps in single environment:  
*The deployment environment 'dev' in your bitbucket-pipelines.yml file occurs multiple times in the pipeline. Please refer to our documentation for valid environments and their ordering.*  
The link of this issue is posted on this [link](https://community.atlassian.com/t5/Bitbucket-questions/The-deployment-environment-test-in-your-bitbucket-pipelines-yml/qaq-p/971584).  

I will cover this issue in the following future post regarding deploying multiple steps in multiple environments.  
Cheers!!
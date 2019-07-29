---
layout: post
title: Hello World with AWS Lambda and API Gateway
date: 2019-07-29
description: You’ll find this post in your `_posts` directory. Go ahead and edit it and re-build the site to see your changes. # Add post description (optional)
img: lambda-api-gateway.png # Add image post (optional)
tags: [AWS, Lambda, API, Gateway, Python, Python3] # add tag
---

Today, i'm going to show you how to use the API Gateway with the simple Lambda function created in the last post [here](https://kirovtome.blog/aws-lambda-api-gateway/).  

1. Configure the API Gateway
2. Test the function  
3. Execute a GET request and see what happens  

#### Prerequirements

* AWS Account
* curl / Postman app


### 1. Configure the API Gateway

1. Login to the [AWS Console](https://aws.amazon.com/console/).  
2. From the AWS Console, under *Networking and Content Delivery* click **API Gateway**.  
3. Click on already created *myNewAPI*.  
4. From Resources tab, select the **GET** method.  
5. From **Actions** dropdown menu, click **Delete Method**.  
<br />
![](/assets/img/screenshots/screenshot15.png)  
6. Click **Delete**.  
<br />
![](/assets/img/screenshots/screenshot16.png)  
7. Create new *GET* method.  
8. For **Lambda function**, type the previously created Lambda function *myHelloWord*.  
<br />
![](/assets/img/screenshots/screenshot17.png)  
9. Click **Save**.  
10. Click **OK**.  
11. Click **Method Request**.  
12. From **Request Validator**, choose *Validate query string parameters and headers*, and click on the tick mark.  
13. From **URL Query String Parameters**, click **Add query string**, enter *name*, click on the tick mark and check the **Required** checkbox.  
<br />
![](/assets/img/screenshots/screenshot18.png)  
14. Go back to the main *GET* window and click on **Integration Request**.  
15. From **Mapping Templates**, under **Request body passthrough** check the radio button *When there are templates defined (recommended)*.  
16. Click **Add mapping template**, and under **Content-Type** type *application/json*, and click the tick mark.  
<br />
![](/assets/img/screenshots/screenshot19.png)  
17. Under the text box add the following code:  
```console
{
    "name":  "$input.params('name')"
}
```  
18. Click **Save**.  
19. From the **Actions** dropdown menu, click **Deploy API**.  
20. From **Deployment stage**, choose *dev* and click **Deploy**. 
<br />
![](/assets/img/screenshots/screenshot20.png)  


### 2. Test the function

1. Now, go back to the **GET** method.  
2. Click on the **TEST** icon.  
3. On the **Method Test**, under **Query Strings**, enter *name=Tome*.  
<br />
![](/assets/img/screenshots/screenshot21.png)  
4. Click **Test**.  
5. The response should be something like this:  
<br />
![](/assets/img/screenshots/screenshot22.png)   

Official AWS API Gateway documentation: https://aws.amazon.com/api-gateway/


### 3. Execute a GET request and see what happens  

1. Open terminal and type the following command:  
```console  
curl -X GET https://7xvlo3lklj.execute-api.eu-west-1.amazonaws.com/dev/?name=Tome
```  
Output:  
```console  
"Cheers from Lambda Tome!"
```  
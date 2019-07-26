---
layout: post
title: Cheers from AWS Lambda Proxy Integration
date: 2019-07-26
description: You’ll find this post in your `_posts` directory. Go ahead and edit it and re-build the site to see your changes. # Add post description (optional)
img: lambda-api-gateway.png # Add image post (optional)
tags: [AWS, Lambda, API, Gateway, Proxy, Integration, Python, Python3] # add tag
---

Today, i'm going to create a simple Hello World with AWS Lambda and API Gateway Proxy Integration with the following steps:  
1. Create a simple Python function with AWS Lambda  
2. Test the function  
3. Create and configure API Gateway Proxy Integration  
4. Execute a GET request and see what happens  

#### Preeequirements

* AWS Account
* curl / Postman app


### 1. Create a simple Python function with AWS Lambda

1. Login to the [AWS Console](https://aws.amazon.com/console/).  
2. From **Services**, under **Compute** click on **Lambda**.  
3. Click on **Create function**.  
4. On **Function name** enter the name of the function we are going to create. On **Runtime** choose *Python 3.7*(the current version available in this time of writing).  
<br />
![](/assets/img/screenshots/screenshot2.png)  
5. On **Permissions**, click **Choose or create an execution role**, and from **Execution role** choose *Create a new role with basic Lambda permissions*.  
<br />
![](/assets/img/screenshots/screenshot3.png)  
6. Click **Create function**.  
7. It will redirect you to the Lambda Console Editor.  It looks something like this:  
<br />
![](/assets/img/screenshots/screenshot4.png)  

Official AWS Lambda documentation: https://aws.amazon.com/lambda/


### 2. Test the function

1. From the right top corner click on **Test** button.  
2. It will open up a new window named **Configure test event** that looks like this:  
<br />
![](/assets/img/screenshots/screenshot5.png)  
3. On the **Event name** give it a simple name like *myEventName*, and edit the text. For example:  
<br />
![](/assets/img/screenshots/screenshot6.png)  
4. Click **Create**.  
5. Now, edit the Lambda function to say for example: 'Cheers from Lambda Tome!'  
```console  
import json

def lambda_handler(event, context):
    # TODO implement
    varName = event['name']
    return {
        'statusCode': 200,
        'body': json.dumps('Cheers from Lambda ' + varName + "!")
    }
```  
6. Click **Save** and then click **Test**.  
7. The *Execution Result* should look like this:  
<br />
![](/assets/img/screenshots/screenshot7.png)  


### 3. Create and configure API Gateway Proxy Integration  

1. From the AWS Console, under *Networking and Content Delivery* click **API Gateway**.  
2. Click **Create API**.  
3. On the new window, enter a **API name** and leave everything as default.  
<br />
![](/assets/img/screenshots/screenshot8.png)  
4. Click **Create API**.  
5. Now, click **Actions** and choose *Create Method*.  
<br />
![](/assets/img/screenshots/screenshot9.png)  
6. Choose the "GET" method and click on the tick mark.  
<br />
![](/assets/img/screenshots/screenshot10.png)  
7. Now, choose *Lambda Function* as **Integration Type**, check the **Use Lambda Proxy Integration**, choose the region where the function was created and write the name of the function.  It should look like this:  
<br />
![](/assets/img/screenshots/screenshot11.png)  
8. Click **Save**.  
9. Click **OK**.  
10. From the **Actions** dropdown menu, choose **Deploy API**.  
<br />
![](/assets/img/screenshots/screenshot12.png)  
11. For **Deployment stage**, choose **[New stage]**.  
<br />
![](/assets/img/screenshots/screenshot13.png)  
12. For **Stage name**, enter *dev*.  
<br />
![](/assets/img/screenshots/screenshot14.png)  
13. Click **Deploy**.  
14. Notice the API **Invoke URL**.  
15. Before testing, we need to modify the *varName* variable to pass the query parameter. So, back to the lambda function:  
```console    
varName = event["queryStringParameters"]['name']
```  
16. Click **Save**.  

Official AWS API Gateway documentation: https://aws.amazon.com/api-gateway/


### 4. Execute a GET request and see what happens  

1. Open terminal and type the following command:  
```console  
curl -X GET https://7xvlo3lklj.execute-api.eu-west-1.amazonaws.com/dev/?name=Tome
```  
Output:  
```console  
"Cheers from Lambda Tome!"
```  


Next post will be the same example but without passing the API Gateway :)
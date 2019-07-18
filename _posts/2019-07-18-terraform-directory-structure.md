---
layout: post
title: Terraform Project Structure Best Practices
date: 2019-07-18
description: You’ll find this post in your `_posts` directory. Go ahead and edit it and re-build the site to see your changes. # Add post description (optional)
img: terraform_logo.png # Add image post (optional)
tags: [Terraform, Ifrastructure, IaC, Code, HashiCorp] # add tag
---

Today, i'm going to write about my thoughts on building a Terraform Project structure.

### What is Terraform

Terraform is an open-source IaC software tool for defining and easily provisioning infrastructure on-premises and on cloud.  
Terraform is using a configuration language known as HashiCorp Configuration Language (HCL).

More about [Terraform](https://www.terraform.io).


### What is IaC

Infrastructure as Code, known as IaC, is a method of defining, provisioning and managing infrastructure (networks, virtual machines, storage,..)
from machine-readable files.

More about [Infrastructure as code](https://en.wikipedia.org/wiki/Infrastructure_as_code).

### Project Structure

```
project_name
|
|____README.md
|
|____dev
|    |__main.tf
|    |__outputs.tf
|    |__variables.tf
|    |__provider.tf
|
|____stage
|    |__main.tf
|    |__outputs.tf
|    |__variables.tf
|    |__provider.tf
|    
|____prod
|    |__main.tf
|    |__outputs.tf
|    |__variables.tf
|    |__provider.tf
|
|____modules
|    |
|    |__vpc
|    |  |__main.tf
|    |  |__outputs.tf
|    |  |__variables.tf
|    |
|    |__ec2
|    |  |__main.tf
|    |  |__outputs.tf
|    |  |__variables.tf
|    |  |__securitygroups.tf
|    |
|    |__rds
|    |  |__main.tf
|    |  |__outputs.tf
|    |  |__variables.tf
|    |
|    |__cloudfront
|    |  |__main.tf
|    |  |__outputs.tf
|    |  |__variables.tf
```  

1. I'm creating directories per environment: "dev", "stage" and "prod". Also, there is a Terraform feature called Workspaces which allows multiple states(environments) to be associated with a single configuration. But, from my point of view, it's a better practice to keep these environments separated configured with different credentials and access controls.  
More about [Workspaces](https://www.terraform.io/docs/state/workspaces.html).

2. Next, there is the *modules* directory which contains the modules: vpc, ec2, rds and cloudfront.  
*What is a module Tome?*  
*Modules* are containers for multiple resources that are used together. For example in the *vpc* module i am using resources are subnets, route tables, internet gateways, and so on ...  

3. Each of these environments and modules contains the main three filenames:
* main.tf 
* outputs.tf
* variables.tf

*main.tf* is a primary file where resources are defined and created.  
*outputs.tf* is a file which contains the output values that will be highlighted to the user when Terraform is deployed. Also, you can use this file to pass around variables between modules.  
*variables.tf* is a file which contains the variables used for creating and managing resources.  

There is also a *provider.tf* file which contains the provider confgiuration, who basically provides **Infrastructure as a Service** (IaaS) like AWS, Microsft Azure, Google Cloud Platform, OpenStack), **Platform as a Service** (PaaS) like Heroku, or **Software as a Service** (SaaS) like Cloudflare.

4. terraform.tfstate file  

Terraform store the state of your infrastructure and configuration initially in a local file terraform.tfstate. My 2 cents on this is to always store the state on remote site whenever you are working in a team or not. S3 bucket remote state example:  
```console  
terraform {
  backend "s3"{
    bucket="terraform-state-dev"
    key="terraform.tfstate"
    region="eu-west-1"
  }
}
```

### Summary  

Having a good structured directory infrastructure can help you scale well on a long run.

Cheers!!
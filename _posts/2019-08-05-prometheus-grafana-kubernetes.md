---
layout: post
title: Deploy Prometheus and Grafana on Kubernetes
date: 2019-08-05
description: You’ll find this post in your `_posts` directory. Go ahead and edit it and re-build the site to see your changes. # Add post description (optional)
img: prometheus-grafana-logo.jpg # Add image post (optional)
tags: [Kubernetes, Prometheus, Grafana] # add tag
---

In this tutorial, i will deploy Prometheus and Grafana to monitor Kubernetes cluster.  

1. What is Kubernetes  
2. What is Prometheus  
3. What is Grafana  
4. Prerequisites  
5. Configure Prometheus  
6. Configure Grafana   


### 1. What is Kubernetes

Kubernetes is an open-source container orchestration tool for automating deployment, scaling and management of containerized apps.  

More about [Kubernetes](https://kubernetes.io).  


### 2. What is Prometheus  

Prometheus is an advanced open-source monitoring solution.  

More about [Prometheus](https://prometheus.io).  


### 3. What is Grafana  

Grafana is an open-source visualization tool that can be used on top of data stores, like ElasticSearch, Prometheus, etc.   

More about [Grafana](https://grafana.com).  



### 4. Prerequisites  

* Docker
* kubectl
* minikube
* helm


### 5. Configure Prometheus    

1. Create Kubernetes namespace for Prometheus  
```console  
kubectl create namespace prometheus
```  
2. Install Prometheus with *helm*:  
```console  
helm install stable/prometheus --name prometheus --namespace prometheus  
```  
3. Check if Prometheus is deployed as expected:  
```console  
kubectl get all -n prometheus
```  
4. Result should look like:  
![Prometheus deployment](/assets/img/screenshot23.png)  
5. Now, we want to access the Prometheus via browser, so we are going to use the *port-forward* command:  
```console  
kubectl port-forward -n prometheus deploy/prometheus-server 8080:9090
```  
6. We will open a browser window, and type:  
```console  
localhost:8080/targets  
```  
7. Result should look like this:  
![Prometheus targets](/assets/img/screenshot24.png)  


### 6. Configure Grafana  

1. Create Kubernetes namespace for Grafana  
```console  
kubectl create namespace grafana
```  
2. Install Prometheus with *helm*:  
```console  
helm install stable/grafana --name grafana --namespace grafana  
```  
3. Check if Grafana is deployed as expected:  
```console  
kubectl get all -n grafana
```  
4. Result should look like:  
![Grafana deployment](/assets/img/screenshot25.png)  
5. Now, we want to access the Grafana via browser, so we are going to use the *port-forward* command:  
```console  
kubectl port-forward -n grafana deploy/grafana 3000
```  
6. We will open a browser window, and type:  
```console  
localhost:3000   
```  
7. Result should look like this:  
![Grafana login](/assets/img/screenshot26.png)  
8. Login with username admin and password generated as output from the following command:  
```console  
kubectl get secret --namespace grafana grafana -o jsonpath="{.data.admin-password}" | base64 --decode ; echo  
```  
9. The Grafana login dashboard:  
![Grafana dashboard](/assets/img/screenshot27.png)  


There is a cool workshop by AWS about building Kubernetes projects on their Kubernetes service [EKS](https://aws.amazon.com/eks/) on the following link: https://eksworkshop.com  

My 2 cents on this is to build slowly and then start to scale up.  
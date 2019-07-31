---
layout: post
title: Getting Started with Firecracker
date: 2019-07-31
description: You’ll find this post in your `_posts` directory. Go ahead and edit it and re-build the site to see your changes. # Add post description (optional)
img: firecracker-logo.jpg # Add image post (optional)
tags: [Firecracker, AWS] # add tag
---

With today's IT modern cloud technologies, you have to choose between fast startup times containers, or VMs with strong hardware-virtualization and workload isolation.  But, today we have something in between, which is called Firecracker.   

1. What is Firecracker  
2. Download Firecracker  
3. Start Firecracker  
4. Configure and start a microVM  
5. Access the microVM  

**We will start, configure and manage everything with API calls. How cool that is!**  


### 1. What is Firecracker

Firecracker is an open source virtualization technology which enables you to deploy lightweight virtual machines with small footprints, called microVMs, or in other words, it takes the best from the both words. It provides security and isolation over traditional VMs with speed and flexibility of containers.  
Firecracker is basically a Kernel-based Virtual Machine developed by AWS, and currently it's running under the hood of [AWS Lambda](https://aws.amazon.com/lambda/) and [AWS Fargate](https://aws.amazon.com/fargate/).  

Firecracker diagram:  
![Firecracker diagram](/assets/img/firecracker-diagram.png)  

More about [Firecracker](https://firecracker-microvm.github.io).  


### 2. Download Firecracker  

**Prerequisites**: https://github.com/firecracker-microvm/firecracker/blob/master/docs/getting-started.md#prerequisites  

1. Download Firecracker:  
```console  
curl -L -o https://github.com/firecracker-microvm/firecracker/releases/download/v0.17.0/firecracker-v0.17.0  
```  
  
2. Set Firecracker binary executable:  
```console
chmod +x firecracker  
```  
  
3. Ensure that Firecracker can run by checking the version number:  
```console  
./firecracker -V  
```  


### 3. Start Firecracker  

1. Download the guest kernel and RootFS:  
```console  
curl -L -o hello-vmlinux.bin https://s3.amazonaws.com/spec.ccfc.min/img/hello/kernel/hello-vmlinux.bin  
curl -L -o hello-rootfs.ext4 https://s3.amazonaws.com/spec.ccfc.min/img/hello/fsfiles/hello-rootfs.ext4
```  
2. Start the Firecracker:  
```console  
./firecracker --api-sock /tmp/firecracker.sock  
```  


### 4. Configure and start a microVM

1. **Open a new window terminal.**  
2. Set the guest kernel:  
```console  
curl --unix-socket /tmp/firecracker.sock -i \  
-X PUT 'http://localhost/boot-source' \  
-H 'Accept: application/json' \  
-H 'Content-Type: application/json' \  
-d '{ 
    "kernel_image_path": "./hello-vmlinux.bin"    
}'  
```  
3. Set the guest rootfs:  
```console  
curl --unix-socket /tmp/firecracker.sock -i \  
-X PUT 'http://localhost/drives/rootfs' \  
-H 'Accept: application/json' \  
-H 'Content-Type: application/json' \  
-d '{  
    "drive_id": "rootfs",  
    "path_on_host": "./hello-rootfs.ext4",  
    "is_root_device": true,  
    "is_read_only": false  
}'  
```  
4. Configure Logging:  
```console  
curl --unix-socket /tmp/firecracker.sock -i \  
-X PUT 'http://localhost/logger' \  
-H 'Accept: application/json' \  
-H 'Content-Type: application/json' \  
-d '{  
    "log_fifo": "/dev/null",  
    "metrics_fifo": "/dev/null",  
    "level": Error,  
    "show_level": true,  
    "show_log_origin": false    
}'  
```  
5. Customize the microVM by configuring CPU and Memory allocated:  
```console  
curl --unix-socket /tmp/firecracker.sock -i \  
-X PUT 'http://localhost/machine-config' \  
-H 'Accept: application/json' \  
-H 'Content-Type: application/json' \  
-d '{  
    "vcpu_count": "2",  
    "mem_size_mib": 254   
}'  
```  
6. Start the microVM:  
```console  
curl --unix-socket /tmp/firecracker.sock -i \  
-X PUT 'http://localhost/actions' \  
-H 'Accept: application/json' \  
-H 'Content-Type: application/json' \  
-d '{  
    "action_type": "InstanceStart"     
}'  
```  

7. We can check the Firecracker parent process ID that will manage the instance by running:  
```console  
ps aux | grep firecracker  
```  

  Result:  
```console  
root      2825  0.6  2.5 145900 39424 pts/0    Sl+  12:27   0:02 ./firecracker --api-sock /tmp/firecracker.sock
```  

8. The microVM is running as a KVM instance that is also listed as a process:  
```console  
$ ps aux | grep firecracker  
root      2825  0.6  2.5 145900 39424 pts/0    Sl+  12:27   0:02 ./firecracker --api-sock /tmp/firecracker.sock  
```  

  Result:  
```console  
$ ps aux | grep kvm  
root      2896  0.0  0.0      0     0 ?        S    12:28   0:00 [kvm-pit/2825]  
```  


### 5. Access the microVM  

1. **Go back to the first terminal window.**  
2. Login as *root* with password *root*.  
3. We can check information about the instance:  
```console  
cat /proc/cpuinfo  
df -h  
free -mh  
uname -a  
```  

The default microVM have 1vCPU and 128MiB RAM. This can be customized.  

4. From the **second terminal** shutdown the instance with the action *InstanceHalt*:  
```console  
curl --unix-socket /tmp/firecracker.sock -i \  
-X PUT 'http://localhost/action' \  
-H 'Accept: application/json' \  
-H 'Content-Type: application/json' \  
-d '{  
    "action_type": "InstanceHalt"     
}' 
```  
5. Shutdown Firecracker:  
```console  
ps aux | grep -ie firecracker | aws '{print $2}' | xargs kill -9  
```  

This tutorial is based on an awesome made getting started tutorial by [katacoda](https://www.katacoda.com).  

Link to the tutorial [here](https://www.katacoda.com/firecracker-microvm/scenarios/getting-started).  

Official Getting Started with Firecracker [here](https://github.com/firecracker-microvm/firecracker/blob/master/docs/getting-started.md)  
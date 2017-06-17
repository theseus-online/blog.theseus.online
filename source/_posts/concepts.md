---
title:  Concepts
date:   2017/05/13
---
***Deployment***: In theseus, `deployment` is the template in which you can control how many containers you want to deploy, where to pull the image for each container, and which volume you want to mount to your container.  
***Pod***: The instance of `deployment`. It has three statuses: running, waiting and terminated. A deployment may have more than one pods, each pod can be treated as a logical host which has a unique internal IP address. Pod's structure is the same as the deployment which it born from.  
The relation between `deployment` and `pod` looks like this:  
![relation between deployment and pods](/images/deployment-pod-relation.png)  
***Volume***: There are two main uses for the volume: data persistence and data sharing. There are two kinds of volumes: persistent volume and empty-dir volume. The difference between them is that the persistent volume is always existed unless the user deletes them manually, while the empty-dir volume never lives longer than the pod it attached to. And the persistent volume can share data across both containers and pods while empty-dir volume can only share data across containers in the same pod. Here is a comparison between them:

| volume           | persistent volume | Empty Directory volume |
|------------------|-------------------|------------------------|
| life             | forever           | in the pod             |
| name             | can named by user | empty-dir-*            |
| backend          | NFS               | host local storage     |
| speed            | slow              | fast                   |
| across container | yes               | yes                    |
| across pod       | yes               | no                     |
***Service***: Service in theseus is used for service discovery and load balancing. For service discovery example, imagine you have two deployments A and B, they work together and need know each other’s IP address before running(think about etcd), the problem is that the deployment doesn’t have ip before the deployment instantiating. So a more easy way is to let them communicate with each other by service instead of IP since service can be controlled by the user while IP can not. The service can proxy any TCP/UDP package to target deployment’s instance(aka. pod). It works like this:  
![etcd example](/images/pods-communicate-by-service.png)  
***Ingress***: By service, you can find your deployment within theseus, but if you want to access your service from the internet, you should use an ingress. Ingress can map a domain to a service in theseus, then you can access the domain with a browser. Ingress supports both HTTP and HTTPS.

---
title: Network Isolation
date: 2017/05/12
---
### Introduction
In order to provide a secure network environment, we've isolated the internal network. You can't access other user's service unless he provides an ingress for that service. In another word, if you don't provide an ingress for a service, there is no need to config certification for it since only your pods can access it. This feature reduces boring certification work for you.

#### Test
To show how network isolation mechanism works, we use two Github account, `lucklove` and `ele828` to test it.  
Firstly, we create a deployment with account `lucklove`:
![A deployment running nginx in account lucklove](/images/test-pod-nginx-by-lucklove.png)  
Here we can see that it contains one pod with internal IP address `192.168.5.123`.  
Secondly, we deploy a container with account `ele828`, it's not important what's in the container this time, but to record the facts, we use `centos` as image and running custom command `/sbin/init`, then we open a shell on this container, test if it can access pod in account `lucklove`:
![Not accessable](/images/not-accessable-by-ele828-by-ip.png)  
What if `lucklove` provides a service and `ele828` accesses the service?  
![Service provided by lucklove](/images/test-service-by-lucklove.png)  
Access it in `ele828`'s pod:  
![Still not accessable](/images/not-accessable-by-ele828-by-service.png)  
What if `lucklove` provides an ingress for this service?  
![Ingress provided by lucklove](/images/test-ingress-by-lucklove.png)  
Access it again:  
![Accessable!](/images/accessable-by-ele828-by-ingress.png)  
As you see, if ingress provided, the service will be accessible by others, but it's from the internet, not from internal network of theseus. That's to say, not only theseus users can access it, it's accessible by the whole internet.  

What these look like in `lucklove`'s view? We open a shell in lucklove's another pod(with image `centos`, command `/sbin/init`), do the same thing as before.  
![All are accessable](/images/access-all-by-lucklove.png)

#### Principle
Theseus use `calico` as k8s CNI plugin. When a user login theseus at the first time, theseus will initialize his namespace which disables any cross pod access by default. Every time a user deploy a deployment, theseus will set a NetworkPolicy for it, which allows pods created by this deployment to be accessed by other pods owned by the same user. During this process, `calico` will watch all related events all the time, and set iptables to control if a pod can access another pod.

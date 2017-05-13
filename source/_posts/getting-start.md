---
title:  Getting Start
date:   2017/04/21
---
### Note
[Theseus](https://theseus.online) is still under development till now, this tutorial is used to collect requirements from early users(if you'd like be one of them). Do `NOT` deploy any production service in case of secure problem.

### Early Tutorial
Here I'll deploy a nginx to show how theseus works.
- Open your browser(eg. chrome) and open url `https://theseus.online`
- Click Login button on the header bar, here it will jump to github oauth page and ask you login your github account(yes, we don't have our own account system and there is no plan to have one).
- After you login your github account, the page should jump to theseus, and you can see there is a new tab named `Deploy`.
- On the `Deployments` slide tab, click the green `+` and it will route to a page, where we just input the app name as 'test' and the image name as 'test-nginx', and the image address should just be 'nginx', it will execute docker pull nginx in theseus. And then, click `Deploy`. After that, you should see this deployment on the deployment dashboard.
- To export our test deployment to the internet, we should add a service and an ingress. On the `Services` slide tab, click the green `+`. Then input the service name as 'test-service', and chose the `test` deployment as backend, and input port name as 'test-port'(it dosen't has much means now, but, just need one), and the protocol, chose TCP, and both srcPort and dstPort shuod be 80, it means map service's 80 TCP port to the deployment(aka 'test')'s TCP 80 port. Click `Deploy`. Again you should see it in service dashboard.
- The last step, setup the domain within Ingress. In ingress dashboard, click `+`, and input the ingress name as 'test-ingress', and input the domain you want to use. Obviously you should use your own domain, not others. I have a domain 'sigsegv.site', so I input 'www.sigsegv.site', because I use http instead of https for this site, so I don't check secure, then I chose backend service 'test-service', and it's port 'test-port'. Then, deploy. It's not done yet, you should cname your to `app.theseus.online`, it's important.  

After these, open the domain with your browser, you should see it.

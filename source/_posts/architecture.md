---
title: Architecture
date: 2017/04/27
---
This post will describe how theseus works, it's usefull for developers and who want to contribute.

### Table of Contents
- [Theseus Topological Graph](#Theseus-Topological-Graph)
  - [The Proxy](#The-Proxy)
  - [The Auth](#The-Auth)
  - [The Api](#The-Api)
  - [The Portal](#The-Portal)

### Theseus Topological Graph
![general architecture](/images/theseus-general-architecture.png)  
#### The Proxy
The proxy is a reverse proxy, it has two missions:
- proxy request to appropriate backend by different url.
- check if a request has validate cookie(signature) if it's target backend is `api`.

#### The Auth
The auth module's work is simple:
- when user request for login, it redirect user to github oauth page.
- when github redirect user back to callback url, it request github for user information, then it set cookie with user information and signature it. Besides， it should update user information to database(aka postgres).
- when user request for logout, it erase cookie.

#### The Api
The api's work contains two aspects:
- Map user request to k8s format, and proxy it to k8s api.
- Manage persistent volumes.

#### The Portal
The portal is the user interface, served as static files(html, js, css etc.).

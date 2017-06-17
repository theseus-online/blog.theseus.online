---
title: Architecture
date: 2017/04/27
---
This post will describe how theseus works, it's useful for developers and who want to contribute.

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
- The proxy is a reverse proxy, which has two missions.
- check if a request has validated cookie(signature) if its target backend is `api`.

#### The Auth
The auth module's work is simple:
- when a user request for login, it redirect the user to Github Oauth page.
- when GitHub redirect the user back to callback URL, it requests GitHub for user information, then it set a cookie with user information and signature it. Besides， it should update user information to the database(aka Postgres).
- when a user request for log out, it erases cookie.

#### The Api
The API's work contains two aspects:
- Map user request to k8s format, and proxy it to k8s api.
- Manage persistent volumes.

#### The Portal
The portal is the user interface, served as static files(HTML, JS, CSS etc.).

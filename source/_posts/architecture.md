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
    - [K8S Api Map](#K8S-Api-Map)
    - [Volume Management](#Volume-Management)
  - [The Portal](#The-Portal)

### Theseus Topological Graph
```
                                                          -----------
                                                    ----> | k8s api |
                                                    |     -----------
                                        --------    |
                                  ----> |  api | ---|
                                  |     --------    |
    -----------      ---------    |     ----------  |     -------------    ------------
    | browser | ---> | proxy | ---|---> | portal |  |---> | postgrest | -> | postgres |
    -----------      ---------    |     ----------  |     -------------    ------------
                                  |     --------    |
                                  ----> | auth | ---|
                                        --------    |
                                                    |     --------------
                                                    ----> | github api |
                                                          --------------
```
#### The Proxy
The proxy is a reverse proxy, it has two missions:
- proxy request to appropriate backend by different url.
- check if a request has validate cookie(signature) if it's dest backend is `api`.

#### The Auth
The auth module's work is simple:
- when user request for login, it redirect user to github oauth page.
- when github redirect user back to callback url, it request github for user information, then it set cookie with user information and signature it. Besides， it should update user information to database(aka postgres).
- when user request for logout, it erase cookie.

#### The Api
The api's work contains two aspects:
- Map user request to k8s format, and proxy it to k8s api.
- Manage persistant volumes.

##### K8S Api Map
Theseus api will map and proxy deployment, service and ingress request to k8s, so that the frontend don't need know if the backend is k8s or not.

##### Volume Management
User can mount two kinds of volumes to container: persistant volume and `Empty Directory` volume. The difference between them is that the persistant volume is always exists unless user delete them manually, but the `Empty Directory` volume never live longer than the deployment it attached to, in other words, when you create or update deployment, there is no way to choise a `Empty Directory` volume created before.  
Here is a compare between `Empty Directory` volume and persistant volume:

| volume  | persistant volume | Empty Directory volume |
|---------|-------------------|------------------------|
| life    | forever           | in the deployment      |
| name    | can named by user | empty-dir-*            |
| backend | NFS               | host local storage     |
| speed   | slow              | fast                   |

#### The Portal
The portal is the user interface, served as static files(html, js, css etc.).

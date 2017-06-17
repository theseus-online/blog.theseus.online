---
title:  Original Intention
date:   2017/05/10
---
This post is not about technical issues, it just describes the intention of designing [theseus](https://theseus.online)，


As we know, web programming is very popular at this time and most programmers are web developers. Every programmer knows that if you want your web service to be assessable from the internet, you need a server to run your service. It's hard to choose where to place your service. I've tried kinds of providers, none of them is 100% perfect.

The Virtual Host is the cheapest way, but it's never a flexible one, you can only deploy service written by specified language. The VPS is more flexible and still cheap enough, it allows you to deploy any services, and only costs few dollars per month, but it's a waste when your service just costs little of computer resource, for example, you have a service which just takes 100MB memory, but you can't find a VPS which provides only 100MB memory because there is no such VPS(at least 250MB), what's more, the system itself will take up resources. The Cloud Server is another choice, it provides higher quality service with a higher price and it's still a waste like VPS.

Theseus is designed to solve this, it should be able to deploy any kind of app, and the app takes resource on demand. Theseus should maximize the payload for users.

In conclusion, theseus is designed to provide individual web developers with available options other than Virtual Host, VPS and Cloud Server, it's more flexible than Virtual Host, cheaper than VPS and Cloud Server.

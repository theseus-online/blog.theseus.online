---
title:  Web Shell
date:   2017/05/11
---
### Introduce
Though it's not recommend to change running container's environment artificially, it's useful in some special cases to open a shell and get into container, for example:
data migration, troubleshooting etc. To archieve this, theseus provide a web shell in which you can excute any command in your container just like in local terminal. The web shell looks like this:
![web shell](/images/web-shell-top.png)

### UTF-8 Support
UTF-8 is allowed, but you should config your container locale to UTF-8(eg. en_US.UTF-8) or the UTF-8 code will be garbled.

### Secure
If you need input password in the shell(eg. use scp to migrate data form/to other place), just feel free to do that, we use WSS(WS-Security) to transfer data with server, so your password won't be seen by others.

### Implementation
#### Architecture
The architecture of web shell is like this:
```
-------------    wss     -------------------------    ws    -----------
| web-shell | ---------> | proxy(Authentication) | -------->| k8s-api |
-------------            -------------------------          -----------
```
#### web-shell
The web-shell component use xterm.js as ui. It receives data on websocket and try to decode every message as UTF-8, if succeed, write decoded data to xterm, else write raw data.

#### proxy
The proxy will check browser cookie and signature to confirm the user has access to target pod(by checking if username and pod namespace matches), if failed, response with 403. Note: the proxy don't check if target pod exists currently, if it does not exist, it will response with 400.

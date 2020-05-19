---
id: LocalServer
title: Launch Local Server
sidebar_label: Launch Local Server
original_id: LocalServer
---
There are two methods to launch local server: using docker or natively.

## Launch AIOU Server Using Docker
Launching AIOU server using docker is simple. It is the recommended way.    
The following command will launch a single-node native AIOU blockchain server.   
You can use it for debugging and testing.   
[Docker CE 18.06 or newer](https://docs.docker.com/install) is needed(older versions are not tested).

```
docker run --rm -p 30000-30003:30000-30003 aiouio/aiou-node
```
![server_output](assets/5-lucky-bet/Lucky-Bet-Operation/server_output.png)

## Launch AIOU Server Natively

After finishing [building AIOU](4-running-aiou-node/Building-AIOU.md), you can run the server.
```
iserver -f ./config/iserver.yml
```
![server_output](assets/5-lucky-bet/Lucky-Bet-Operation/server_output.png)


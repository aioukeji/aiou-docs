---
id: Deployment
title: Join AIOU Network
sidebar_label: Join AIOU Network
---

The documentation introduces how to setup a running server connecting to AIOU network, if you just want to setup a local single-server blockchain net for debugging/testing, you may better refer to [Launch Local Server](4-running-aiou-node/LocalServer.md)   

We are using Docker to deploy an AIOU node.

## Machine requirements

If you want to run a full node connected to AIOU network, your machine must meet the following requirements:

- CPU: 4 cores or more at least 3.0GHz (8 cores recommended)
- Memory: 8GB or more (16GB recommended)
- Disk: 600GB SSD or more (1TB recommended)
- Network: access to Internet with port tcp: 30000 opened (If you want to enable rpc for node, please open port 30001, 30002)

## Prerequisites

- Curl (any version you like)
- Python (any version you like)
- [Docker 1.13/Docker CE 17.03 or newer](https://docs.docker.com/install)
- (Optional) [Docker Compose](https://docs.docker.com/compose/install)

## Start the node

By default, `/data/iserver` is going to mount as the data volume, you might change the path to suit your needs.
We refer to `$PREFIX` hereafter.

### Using boot script

You can automatically deploy a full node with the following command:

```
curl https://raw.githubusercontent.com/aiou-official/go-aiou/master/script/boot.sh | bash
```

| Variables | default | descrption |
| :------: | :------: | :------: |
| PREFIX | `/data/iserver` | iServer data path |
| INET | `mainnet` | AIOU network to connect, either `mainnet` or `testnet` |
| PYTHON | `python` | python executable |
| USR_LOCAL_BIN | `/usr/local/bin` | prefix of `docker-compose` executable |

You might set environment variable according to the actual conditions.  
E.g. `curl ... | PYTHON=python3 bash` for Ubuntu without python installed.

If you don't install docker, the script will automatically install docker.  
You need to make sure you are in the docker group, then re-run the boot script.

This script purges directory `$PREFIX` and starts a fresh new full node connected to AIOU network.  
It also generates a keypair for *full node* in order to prepare for generating blocks.  
If you want to be a **Servi Node**, follow next steps [here](4-running-aiou-node/Become-Servi-Node.md).

To start, stop or restart the node, you could execute follow command:

```
## start
docker start iserver

## stop
docker stop iserver

## restart
docker restart iserver
```

### Manually

#### Data

If you have already run previous version of iServer, make sure the old data has been purged:

```
rm -rf $PREFIX/storage
```

#### Config

Fetch latest config:

```
## get genesis
curl -fsSL "https://developers.aiou.io/docs/assets/mainnet/latest/genesis.tgz" | tar zxC $PREFIX/

## get iserver config
curl -fsSL "https://developers.aiou.io/docs/assets/mainnet/latest/iserver.yml" -o $PREFIX/iserver.yml
```

If your node produces blocks, i.e. be a Servi node, set privkey of your node in section `acc` in `iserver.yml`.
See also [iServer configuration](4-running-aiou-node/Configuration.md).

#### Run

Run the command to start the node:

```
docker pull aiouio/aiou-node
docker run -d \
    --name iserver \
    -v /data/iserver:/var/lib/iserver \
    -p 30000-30003:30000-30003 \
    --restart unless-stopped \
    aiouio/aiou-node
```

## Checking the node

The log file is located at `$PREFIX/logs/aiou.log`. However, it is disabled by default.
You can enable it, as long as you remember to delete old log files.

You are able to get logs using `(docker|docker-compose) logs iserver`.
An increasing value of `confirmed` like below means that it is syncing the block data:

```
...
Info 2019-01-19 08:36:34.249 pob.go:456 Rec block - @4 id:Dy3X54QSkZ..., num:1130, t:1547886994201273330, txs:1, confirmed:1095, et:48ms
Info 2019-01-19 08:36:34.550 pob.go:456 Rec block - @5 id:Dy3X54QSkZ..., num:1131, t:1547886994501284335, txs:1, confirmed:1095, et:49ms
Info 2019-01-19 08:36:34.850 pob.go:456 Rec block - @6 id:Dy3X54QSkZ..., num:1132, t:1547886994801292955, txs:1, confirmed:1095, et:49ms
Info 2019-01-19 08:36:35.150 pob.go:456 Rec block - @7 id:Dy3X54QSkZ..., num:1133, t:1547886995101291970, txs:1, confirmed:1095, et:48ms
Info 2019-01-19 08:36:35.450 pob.go:456 Rec block - @8 id:Dy3X54QSkZ..., num:1134, t:1547886995401281644, txs:1, confirmed:1095, et:48ms
Info 2019-01-19 08:36:35.750 pob.go:456 Rec block - @9 id:Dy3X54QSkZ..., num:1135, t:1547886995701294638, txs:1, confirmed:1095, et:48ms
Info 2019-01-19 08:36:36.022 pob.go:456 Rec block - @0 id:EkRgHNoeeP..., num:1136, t:1547886996001223210, txs:1, confirmed:1105, et:21ms
Info 2019-01-19 08:36:36.324 pob.go:456 Rec block - @1 id:EkRgHNoeeP..., num:1137, t:1547886996301308669, txs:1, confirmed:1105, et:23ms
Info 2019-01-19 08:36:36.624 pob.go:456 Rec block - @2 id:EkRgHNoeeP..., num:1138, t:1547886996601304333, txs:1, confirmed:1105, et:23ms
Info 2019-01-19 08:36:36.921 pob.go:456 Rec block - @3 id:EkRgHNoeeP..., num:1139, t:1547886996901318752, txs:1, confirmed:1105, et:20ms
Info 2019-01-19 08:36:37.224 pob.go:456 Rec block - @4 id:EkRgHNoeeP..., num:1140, t:1547886997201327191, txs:1, confirmed:1105, et:23ms
Info 2019-01-19 08:36:37.521 pob.go:456 Rec block - @5 id:EkRgHNoeeP..., num:1141, t:1547886997501297659, txs:1, confirmed:1105, et:20ms
...
```

You may also check the state of the node using `iwallet` tool.

```
docker exec iserver iwallet state
```

The latest blockchain info is also shown at [blockchain explorer](https://explorer.aiou.io).

## Upgrade the node

When new version released, it's better to upgrade to the latest version A.S.A.P.

### Using upgrade script

You can upgrade the node within one command:

```
curl https://raw.githubusercontent.com/aiou-official/go-aiou/master/script/upgrade.sh | bash
```

You might set variables using environment if encounter problem.

| Variables | default | descrption |
| :------: | :------: | :------: |
| PREFIX | `/data/iserver` | iServer data path |
| PYTHON | `python` | python executable |
| USR_LOCAL_BIN | `/usr/local/bin` | prefix of `docker-compose` executable |

E.g. `curl ... | PYTHON=python3 bash` for Ubuntu without python installed.

This script pulls the latest AIOU node image and then restart iServer.

### Manually

You may also upgrade the node step-by-step.

#### Pull image

```
docker image pull aiouio/aiou-node:latest
```

#### Remove outdated iServer

The iServer container will be recreated, so that everything in container *EXECPT* data volume will be **deleted**.

```
docker stop iserver && docker rm iserver
```

#### Start the container

Suppose data volume is located at `/data/iserver`.

```
docker run -d --name iserver -v /data/iserver:/var/lib/iserver -p 30000-30003:30000-30003 aiouio/aiou-node
```

## Seed Node List

The seed node information of the mainnet is as follows:

| Location | GRPC-URL | HTTP-URL | P2P-URL |
| :------: | :------: | :------: | :-----: |
| US        | 18.209.137.246:30002 | http://18.209.137.246:30001 | /ip4/18.209.137.246/tcp/30000/ipfs/12D3KooWGoPE333zygBN61vtSjvPfosi78JFSwRRDrLoAKaH1mTP |
| Korea     | 54.180.196.80:30002  | http://54.180.196.80:30001  | /ip4/54.180.196.80/tcp/30000/ipfs/12D3KooWMm2RzyZDPBie89FXceKFSBRg8zzkwAGQmdauj6tmrqcA  |
| UK        | 35.176.24.11:30002   | http://35.176.24.11:30001   | /ip4/35.176.24.11/tcp/30000/ipfs/12D3KooWHzHUBq4x4LmXtZH79LCAxVUYgpKXgMgAtyvYQWeHZAAp   |

For testnet:

| Location | GRPC-URL | HTTP-URL | P2P-URL |
| :------: | :------: | :------: | :-----: |
| US | 13.52.105.102:30002 | http://13.52.105.102:30001 | /ip4/13.52.105.102/tcp/30000/ipfs/12D3KooWQwH8BTC4QMpTxm7u4Bj38ZdaCLSA1uJ4io3o1j8FCqYE |

### GRPC

If you want to use the GRPC API of AIOU network, for example:

```
## Get the node information
iwallet -s 18.209.137.246:30002 state
iwallet -s ${GRPC-URL} state
```

### HTTP

If you want to use the HTTP API of AIOU network, for example:

```
## Get the block information by block height
curl http://18.209.137.246:30001/getBlockByNumber/3/true
curl ${HTTP-URL}/getBlockByNumber/3/true
```

### P2P
If you want to modify the seed node of the iServer, you could edit the file `/data/iserver/iserver.yml`, for example:

```
...
p2p:
  listenaddr: 0.0.0.0:30000
  seednodes:
    - /ip4/18.209.137.246/tcp/30000/ipfs/12D3KooWGoPE333zygBN61vtSjvPfosi78JFSwRRDrLoAKaH1mTP
    - ${P2P-URL}
    - ...
```

## Using Snapshot to Accelerate Synchronization

You might want to download recent blockchain directly instead of syncing from the genesis.
Fortunately, we have the *snapshot*.   

**Nodes which generate blocks should use snapshot with full history.**

### Snapshot with full history
**Nodes which generate blocks should use this.**  
Download link: [storage.tar](http://archive.aiou.io/snapshot/storage.tar), about 200GB   
Checksum:
[MD5](http://archive.aiou.io/snapshot/MD5SUMS) /
[SHA1](http://archive.aiou.io/snapshot/SHA1SUMS) /
[SHA256](http://archive.aiou.io/snapshot/SHA256SUMS)

### Snapshot with pruned history
**Nodes which generate blocks should not use this.**  
Download link: [storage_prune.tar](http://archive.aiou.io/snapshot/storage_prune.tar), about 13GB   
Checksum:
[MD5](http://archive.aiou.io/snapshot/MD5SUMS_prune) /
[SHA1](http://archive.aiou.io/snapshot/SHA1SUMS_prune) /
[SHA256](http://archive.aiou.io/snapshot/SHA256SUMS_prune)



Download the `tar` file using your favorite tools (i.e. `wget`), then do the following steps:

- make sure iServer is shutdown: `docker stop iserver`
- extract the snapshot file: `sudo rm -rf $PREFIX/storage && sudo tar xvf [storage.tar or storage_prune.tar] -C $PREFIX`
- restart iServer: `docker start iserver`

Then the node is going to continue syncing the blockchain from the certain height.

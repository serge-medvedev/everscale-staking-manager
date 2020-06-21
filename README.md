[![sergemedvedev/freeton-validator](https://img.shields.io/docker/cloud/build/sergemedvedev/freeton-staking-manager.svg)](https://hub.docker.com/r/sergemedvedev/freeton-staking-manager)

# FreeTON Staking Manager

## HOWTO

- Refer to [config.js.example](config.js.example) to create the config file
- Make sure you have the client private key and the server public key, generated by _setup.sh_ script during validator engine initialization
- Build the image and run a container with [docker-compose](docker-compose.yml):
    ```yaml
    version: "2.3"
    services:
      freeton-staking-manager:
        image: sergemedvedev/freeton-staking-manager
        volumes:
          - ./config.js:/usr/src/app/config.js:ro
          - ./certs:/usr/src/app/certs:ro
          - ./db:/data/freeton-staking-manager
        ports:
          - "3000:3000"
        environment:
          DEBUG: "api,lib:*"
        restart: always
    ```
    ```console
    $ docker-compose up -d
    ```

## API Reference

### GET /runOnce
[DEPRECATED] Sends and recovers stakes (use crontab to schedule calls)

Example:
```console
$ curl localhost:3000/runOnce

### then check out docker logs for additional info
```
---

### POST /sendStake
Tries to send a stake

Example:
```console
$ curl -XPOST localhost:3000/sendStake
```
---

### POST /recoverStake
Tries to recover a stake

Example:
```console
$ curl -XPOST localhost:3000/recoverStake
```
---

### POST /nextStake?value=x
Sets the value of a stake to be sent in upcoming elections

> __x__ amount in tokens

Example:
```console
$ curl -XPOST localhost:3000/nextStake?value=20000
```
---

### POST /nextElections/:action
Allows to skip upcoming elections (no idea why one would need it)

> __:action__ "skip" or "participate"

Example:
```console
$ curl -XPOST localhost:3000/nextElections/skip
```
---

### GET /electionsHistory
Returns info (keys, stake, etc.) about elections the node participated in

Example:
```console
$ curl -s localhost:3000/electionsHistory | jq '.'
```
---

### GET /timediff
Returns the node's sync status

Example:
```console
$ curl localhost:3000/timediff
```
---

## TODO

- Add more HTTP API docs
---
title: BITQAP

language_tabs: # must be one of https://git.io/vQNgJ
  - shell
  - python
  - javascript

toc_footers:
  - <a href='#'>You need to generate Public-Private Key pair</a><br>
  - <a href='https://github.com/aze2201/bashCoin'>Github</a>

includes:
  - errors

search: true

code_clipboard: true

meta:
  - name: description
    content: Documentation for bitqap
---

# Introduction

Welcome to the bitqap project! You can use our bitqap interfaces to access endpoints.

We have language bindings in Shell, Python, and JavaScript! You can view code examples in the dark area to the right, and you can switch the programming language of the examples with the tabs in the top right.


# Topology
![Alt text](https://github.com/aze2201/bashCoin/blob/main/doc/img/TopologyBashCoin_v1.png?raw=true)

# Mining steps
![Alt text](https://github.com/aze2201/bashCoin/blob/main/doc/img/p2pCropped.gif?raw=true)

# Authentication

> To generate Private/Public Key pair use this shell code:


```shell
# csr creted for mining node to accept encrypyed connections only over HTTPS
openssl genrsa -aes128 -out username_private.pem 1024
openssl genrsa  -out user.key 2048
openssl req -key user.key -new -out user.csr
openssl rsa -in user.key  -pubout > user.pub
```

```python
not defined yet
```

```javascript
not defined yet
```

> Private/Public key required to do any action

bitqap uses self signed Public/Private key-pair to accept HTTPS calls only from another nodes or Wallets.
[developer portal](https://github.com/aze2201/bashCoin.

This part is still under development, because of python ssl.py module doesn't support enctyped key (autofill passphrase)

`Authorization: private-key`

<aside class="notice">
You must generate private/public key on your device and secure it.
</aside>

# bitqap messages

## Account information


```shell
cat cert/example.com.pub| sha256sum  | awk '{print $1}'
```

```python
not defined yet
```

```javascript
not defined yet
```

> The above command returns TXT structured like this:

```shell
497cea9f5af23c76922d7bfc6237d8e225248887defcc333f36ba853a1266848
```

Your account is SHA256 of public key. In future other accounts will use this info to coins. 
You can provide this account as URL or QR code 

### no WS Request


### message parameters

key | value | Description
--------- | ------- | -----------
N/A | N/A | N/A

<aside class="success">
Remember — keep your private key secret
</aside>

## start mining

```shell
cd bin
./bashCoin.sh '{"command":"mine","appType":"miner","messageType":"direct"}'
```


```python
python wsdump.py ws://127.0.0.1:8001
> {"command":"mine","appType":"miner","messageType":"direct"}
```


```javascript
not defined yet
```

> The above command returns JSON structured like this:

```json
{
  "command":"notification",
  "commandCode":"100",
  "appType":"miner",
  "messageType":"broadcast",
  "status":"0", 
  "timeUTC":"20211128102607",
  "difficulty":"1",
  "MINEDBLOCK":"163.blk.solved",
  "NEXTBLOCK":"164.blk"
}
```

Mine will insert the top transactions from the queue with the 100 highest into the next block and start calculating.


<aside class="warning">mine command can be sent only from localhost.</aside>

### WS Request

``webSocket = new WebSocket(url, protocols);
webSocket.send("Here's some text that the server is urgently awaiting!");
``

### message parameters

key | value | Description
--------- | ------- | -----------
command | mine |  mandatory
appType | miner/wallet | optional
messageType| direct/broadcast | mandatory

## check balance


```shell
cd bin
./bashCoin.sh '{"command":"checkbalance","ACCTNUM":"50416596951b715b7e8e658de7d9f751fb8b97ce4edf0891f269f64c8fa8e034","messageType":"direct"}'
```

```python
python wsdump.py ws://127.0.0.1:8001
> {"command":"checkbalance","ACCTNUM":"50416596951b715b7e8e658de7d9f751fb8b97ce4edf0891f269f64c8fa8e034","messageType":"direct"}
```

```javascript
not defined yet
```

> The above command returns JSON structured like this:

```json
  {
    "command": "checkbalance", 
    "commandCode": "200", 
    "messageType": "direct", 
    "status": "0", 
    "destinationSocket": "4", 
    "result": 
      {
        "publicKeyHASH256": "50416596951b715b7e8e658de7d9f751fb8b97ce4edf0891f269f64c8fa8e034", 
        "balance": "46"
      }
    }
```

This function will use UTXO method to calculate balance.

### WS Request

``webSocket = new WebSocket(url, protocols);
webSocket.send("Here's some text that the server is urgently awaiting!");
``

### message parameters

key | value | Description
--------- | ------- | -----------
command | checkbalance |  mandatory
messageType| direct/broadcast | mandatory
ACCTNUM| sha256 hash of Pub key | mandatory



## send coin



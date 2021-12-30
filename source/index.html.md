---
title: BITQAP

language_tabs: # must be one of https://git.io/vQNgJ
  - shell
  - python

toc_footers:
  - <a href='https://github.com/bitqap/bitqap'>GITHUB</a>

includes:
  - errors

search: true

code_clipboard: true

meta:
  - name: description
    content:  BITQAP crypto
---

# Introduction to BITQAP

Welcome to the BITQAP project! This is [blockchain](https://en.wikipedia.org/wiki/Blockchain) based software.
Currently this project under development.

Core block calculation app developed by [BASH](https://en.wikipedia.org/wiki/Bash_(Unix_shell)) language, and network interface developed on [python3](https://www.python.org/). BITQAP network uses Peer to Peer concept using [WebSocket](https://en.wikipedia.org/wiki/WebSocket ) protocol.

We have language bindings in Shell, Python. You can view code examples in the dark area to the right, and you can switch the programming language of the examples with the tabs in the top right.

# Topology

```shell
# download git project
git clone https://github.com/bitqap/bitqap.git

# start server
cd bitqap/bin
python3 socketGateway3.py


# start local miner app (socket) TEST MODE.
./startLocalApp.sh

# another session
./startExternalApp.sh
```

![Alt text](https://github.com/bitqap/bitqap/blob/main/doc/img/TopologyBashCoin_v1.png?raw=true)

BITQAP uses BITQAP protocol suite on top of the WebSocket and  [Linux Namad Pipe](https://en.wikipedia.org/wiki/Named_pipe) technology. **bashCoin.sh** is core script which is manages application (mine, validation, coin send, etc.)
**bashCoin.sh** binded to `NAMED_PIPE_BSH` (variable) pipe file over `./startLocalApp.sh` script.
Copy of `./startLocalApp.sh` script output writing to `NAMED_PIPE_EXT` (variable) pipe file also to broadcast external servers and `./startExternalApp.sh` script reads this message.
`NAMED_PIPE_BSH` and `NAMED_PIPE_EXT` file descriptor is 3

# Mining steps
![Alt text](https://github.com/bitqap/bitqap/blob/main/doc/img/p2pCropped.gif?raw=true)

Steps:

1. Node finds block hash in defined difficulty level.
2. Node shares block information as notification command to its neighbors `{"command": "notification",...}`.
3. Neighbor then requests block content from node.
4. The node shares the block content using BASE64 encoding.
5. Once the neighbor adds the BLOCK to the local BLOCKCHAIN (after validation), it also notifies its neighbors. So it propagates.


# Authentication

> To generate Private/Public Key pair use this shell code:


```shell
# csr creted for mining node to accept encrypyed connections only over HTTPS
openssl genrsa -aes128 -out username_private.pem 1024
openssl genrsa  -out user.key 2048
openssl req -key user.key -new -out user.csr
openssl rsa -in user.key  -pubout > user.pub
```


> Private/Public key required to do any action.

The **BITQAP** blockchain does not store any user information. Your private key is your everything.
Third-party wallets can bind your email to your private key.
However, in theory, the public and private keys are generated and stored on the end device (wallet).
Wallet can share the public key with third parties in sha256sum format or QR code format.
Wallet signs the transaction message with the private key stored on the end device. 
Only the Linux `shell` command for creating the public/private key is available in this documentation.


<aside class="notice">
You must generate private/public key on your device and secure it.
</aside>

# BITQAP messages

## Account information


```shell
# this is your userid. You can share by text or QR code.
cat cert/example.com.pub| sha256sum  | awk '{print $1}'
```


> The above command returns TXT structured like this:

```shell
497cea9f5af23c76922d7bfc6237d8e225248887defcc333f36ba853a1266848
```

Your account is sha256sum of Public key. In future other accounts will use this info to coins. 
You can provide this account as URL or QR code 

### no WS Request


### message parameters. 

key | value | Description
--------- | ------- | -----------
N/A | N/A | N/A

<aside class="success">
Remember — keep your private key secret
</aside>

## start mining

```shell
> cd bin
> ./bashCoin.sh '{"command":"mine","appType":"miner","messageType":"direct"}'
```


```python
python wsdump.py ws://127.0.0.1:8001
> {"command":"mine","appType":"miner","messageType":"direct"}
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

Steps:

1.  Node will collect transactions from pendings. By default it is 100 (based on config.ini file)
2.  Node will add it's REWARD transaction also which is include Pub Key and own signature in base64 format.
3.  Node will start calculating HASH (currently md5sum) by increasing NONCE.
4.  Once, HASH found it will construct file and will propagate.

<aside class="notice">
Code is supporting HIGH peformance MINING wich is writting in C programming. 
You can enable in `config.ini` by change value `CMINING` from 0 to 1.
</aside>

<aside class="warning">mine command can be sent only from localhost.</aside>

### WS Request

``
webSocket.send(<JSON>");
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
Once balance calculate it wil add to block as latest information and signature will included.
If TX found `<pubkey1>:<pubkey1>:<AMOUNT>:...<PUBKEY>:<SIGNATURE>` then will return balance.


### WS Request

``webSocket.send(<JSON>");``

### message parameters

key | value | Description
--------- | ------- | -----------
command | checkbalance |  mandatory
messageType| direct/broadcast | mandatory
ACCTNUM| sha256 hash of Pub key | mandatory

<aside class="warning">Return value still not distinguish Confirmed or Pending</aside>

## send coin
```shell
#full message

fullMessage='{"command": "getTransactionMessageForSign", "status": 0, "destinationSocket": "2", "result": {"forReciverData": "50416596951b715b7e8e658de7d9f751fb8b97ce4edf0891f269f64c8fa8e034:b1bd54c941aef5e0096c46fd21d971b3a3cf5325226afb89c0a9d6845a491af6:5:3:202111121313", "forSenderData": "50416596951b715b7e8e658de7d9f751fb8b97ce4edf0891f269f64c8fa8e034:50416596951b715b7e8e658de7d9f751fb8b97ce4edf0891f269f64c8fa8e034:38:0:202111121313"}}'

# key pair
privKey="/root/bashCo1/cert/example.com.key"
publKey="/root/bashCo1/cert/example.com.pub"

# orignal raw message need to be hashed for TxID
rawMsgRecv=$(echo $fullMessage | jq -r '.result' | jq -r '.forReciverData')
rawMsgSend=$(echo $fullMessage | jq -r '.result' | jq -r '.forSenderData')

# hash sha256
txIDRecv=$(echo ${rawMsgRecv}| sha256sum | awk '{print $1}')
txIDSend=$(echo ${rawMsgSend}| sha256sum | awk '{print $1}')

# raw message need to be signed (txID included)
OrigMSG_Recv=$(echo "TX$txIDRecv:$rawMsgRecv")
OrigMSG_Send=$(echo "TX$txIDSend:$rawMsgRecv")

# encoded Public key as base64 
pubKeyBase64=$(cat ${publKey}| base64 | tr '\n' ' ' | sed 's/ //g')

# signatures of both messages
signedMsgRecv=$(echo ${OrigMSG_Recv}| openssl dgst -sign ${privKey} -keyform PEM -sha256 | base64 | tr '\n' ' ' | sed 's/ //g')
signedMsgSend=$(echo ${OrigMSG_Send}| openssl dgst -sign ${privKey} -keyform PEM -sha256 | base64 | tr '\n' ' ' | sed 's/ //g')

# construct of messages
recv="$OrigMSG_Recv:$pubKeyBase64:$signedMsgRecv"
send="$OrigMSG_Send:$pubKeyBase64:$signedMsgSend"

# build JSON
result=[]
# add reciever msg
result=$(echo ${result}| jq --arg dt $recv '. + [ $dt ]')
# add sender msg
result=$(echo ${result}| jq --arg dt $send '. + [ $dt ]')

# Full messages
returnMesssage="{\"command\":\"pushSignedMessageToPending\", \"messageType\": \"direct\", \"result\":$result}"

echo $returnMesssage

```

```python
python wsdump.py ws://127.0.0.1:8001
>  { "command":"getTransactionMessageForSign","messageType":"direct","result":{"forReciverData":"50416596951b715b7e8e658de7d9f751fb8b97ce4edf0891f269f64c8fa8e034:b1bd54c941aef5e0096c46fd21d971b3a3cf5325226afb89c0a9d6845a491af6:5:3:202111121313","forSenderData":"50416596951b715b7e8e658de7d9f751fb8b97ce4edf0891f269f64c8fa8e034:50416596951b715b7e8e658de7d9f751fb8b97ce4edf0891f269f64c8fa8e034:38:0:202111121313"}}

# used shell section to build message. Unfortunatelly pytyon code is not ready to build signed message
> {"command":"pushSignedMessageToPending", "messageType": "direct", "result":["message1...","message2..."]}
```


> The above command returns JSON structured like this for `getTransactionMessageForSign` command. result message will be signed by Wallet and send back by `pushSignedMessageToPending` command. :

```json
{
  "command": "getTransactionMessageForSign", 
  "messageType": "direct", 
  "status": 0, 
  "destinationSocket": 2, 
  "result": 
    {
      "forReciverData": "50416596951b715b7e8e658de7d9f751fb8b97ce4edf0891f269f64c8fa8e034:b1bd54c941aef5e0096c46fd21d971b3a3cf5325226afb89c0a9d6845a491af6:5:3:202111121313", 
      "forSenderData": "50416596951b715b7e8e658de7d9f751fb8b97ce4edf0891f269f64c8fa8e034:50416596951b715b7e8e658de7d9f751fb8b97ce4edf0891f269f64c8fa8e034:38:0:202111121313"
    }, 
    "socketID": 1}
```

> The above command returns JSON structured like this for `pushSignedMessageToPending` command:

```json
{
  "command": "notification", 
  "tag": "FFFFx0", 
  "status": 0, 
  "commandCode": "402", 
  "exceptSocket": [3], 
  "messageType": "broadcast", 
  "result": 
    ["TX0aea1917ee5c4e1090f264ebb3d00269775b6c0a853f64c30abcae47b4edfd16","TXe6d2642c00c9a782b58a0d5b7f0f537282f9257b736cc97648d8589701a02a85"], 
  "socketID": 1
}
```

Send coint consist of 2 steps. 
`getTransactionMessageForSign` and `pushSignedMessageToPending` commands.

`getTransactionMessageForSign` is used for get message from Node (with balance info).
`pushSignedMessageToPending` is used for to sign message with Wallet privsate key and push to Node (pending transactions).

### WS Request

``webSocket.send(<JSON>");``

**getTransactionMessageForSign** message parameters

key | value | Description
--------- | ------- | -----------
command | getTransactionMessageForSign |  mandatory
messageType| direct | mandatory
result| list  | mandatory
forReciverData| text  | mandatory
forSenderData| text  | mandatory

**pushSignedMessageToPending** message parameters

key | value | Description
--------- | ------- | -----------
command | pushSignedMessageToPending |  mandatory
messageType| direct | mandatory
result| list  | mandatory

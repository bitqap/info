# Errors

<aside class="notice">
Error page still not updated. 
</aside>


Error Code | Meaning
---------- | -------
400 | Bad Request -- Your request is invalid.
401 | Unauthorized -- Your API key is wrong.
403 | Forbidden -- The kitten requested is hidden for administrators only.
404 | Not Found -- The specified kitten could not be found.
405 | Method Not Allowed -- You tried to access a kitten with an invalid method.
406 | Not Acceptable -- You requested a format that isn't json.
410 | Gone -- The kitten requested has been removed from our servers.
418 | I'm a teapot.
429 | Too Many Requests -- You're requesting too many kittens! Slow down!
500 | Internal Server Error -- We had a problem with our server. Try again later.
503 | Service Unavailable -- We're temporarily offline for maintenance. Please try again later.

You can add your own ERROR code to **bin/functMapFunc2Code.sh** file and call from functions.

```shell
mapERRORFunction2Code () {
        funcName=$1
        type=$2
        case "$funcName" in 
                mine)
                                code=500
                                name=mine
                                ;;
                mineGenesis)
                                code=501
                                name=mineGenesis
                                ;;
                checkAccountBal)
                                code=510
                                name=checkAccountBal
                                ;;
                getTransactionMessageForSign)
                                code=511
                                name=getTransactionMessageForSign
                                ;;
                pushSignedMessageToPending)
                                code=512
                                name=pushSignedMessageToPending
                                ;;
                AddBlockFromNetwork)
                                code=520
                                name=AddBlockFromNetwork
                                ;;
                listNewBlock)
                                code=521
                                name=listNewBlock
                                ;;
                provideBlocks)
                                code=522
                                name=provideBlocks
                                ;;
                validateNetworkBlockHash)
                                code=523
                                name=validateNetworkBlockHash
                                ;;
                updateNetworkInfo)
                                code=401
                                name=updateNetworkInfo
                                ;;
                *)
                                code=000
                                ;;
                esac
        echo $code
}

```

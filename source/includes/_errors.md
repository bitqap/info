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
        case "$funcName" in 
                mine)
                                code=500
                                ;;
                mineGenesis)
                                code=501
                                ;;
                checkAccountBal)
                                code=510
                                ;;
                getTransactionMessageForSign)
                                code=511
                                ;;
                pushSignedMessageToPending)
                                code=512
                                ;;
                AddBlockFromNetwork)
                                code=520
                                ;;
                listNewBlock)
                                code=521
                                ;;
                provideBlocks)
                                code=522
                                ;;
                validateNetworkBlockHash)
                                code=523
                                ;;
                updateNetworkInfo)
                                code=401
                                ;;
                *)
                                code=000
                                ;;
        esac
        echo $code
}

```

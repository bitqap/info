# Errors

<aside class="notice">
Error page still not updated. 
</aside>


Error Code | Meaning
---------- | -------
mine|000
mineGenesis|000
checkAccountBal|000
getTransactionMessageForSign|000
AddTransactionFromNetwork|000
pushSignedMessageToPending|000
askBlockContent|000
provideTXMessage|000
AddNewBlockFromNode|000
listNewBlock|000
provideBlockContent|000
validateNetworkBlockHash|000
askBlockContent|000
updateNetworkInfo|000

You can add your own ERROR code to **bin/functMapFunc2Code.sh** file and call from functions.


```shell

> cat bin/functMapFunc2Code.sh

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


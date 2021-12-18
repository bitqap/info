# Errors

<aside class="notice">
Error page still not updated. 
</aside>

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


Error Code | Meaning
---------- | -------
mine|1500
mineGenesis|1501
checkAccountBal|1510
getTransactionMessageForSign|1511
AddTransactionFromNetwork|1512
pushSignedMessageToPending|1513
askBlockContent|1514
provideTXMessage|1515
AddNewBlockFromNode|1520
listNewBlock|1521
provideBlockContent|1522
validateNetworkBlockHash|1523
askBlockContent|1514
updateNetworkInfo|1401fo|000

You can add your own ERROR code to **bin/functMapFunc2Code.sh** file and call from functions.

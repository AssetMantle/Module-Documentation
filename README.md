# Transaction.sh-Documentation :rocket:

Welcome to the Transaction.sh Documentation repo, Here you will find anything and everything about transactions. 

## Lets get started :newspaper:

- Step 1

Adding Keys to Transaction.sh
```shell
assetClient keys add <name_of_acc> --keyring-backend test
```
- Step 2

After adding Keys, adding and setting up the environment variables
```
NONCE="$RANDOM"
SLEEP=6
KEYRING="--keyring-backend os"
MODE="-b sync"
```
- Step 3

The next step is to create users 
```
ACCOUNT_NAME_1=account1$NONCE
ACCOUNT_NAME_2=account2$NONCE
ACCOUNT_NAME_3=account3$NONCE
ACCOUNT_NAME_4=account4$NONCE
assetClient keys add $ACCOUNT_NAME_1 $KEYRING
assetClient keys add $ACCOUNT_NAME_2 $KEYRING
assetClient keys add $ACCOUNT_NAME_3 $KEYRING
assetClient keys add $ACCOUNT_NAME_4 $KEYRING
```
- Step 4 

After adding users, adding the addresses to the accounts
```
TEST=$(assetClient keys show -a test $KEYRING)
ACCOUNT_1=$(assetClient keys show -a $ACCOUNT_NAME_1 $KEYRING)
ACCOUNT_2=$(assetClient keys show -a $ACCOUNT_NAME_2 $KEYRING)
ACCOUNT_3=$(assetClient keys show -a $ACCOUNT_NAME_3 $KEYRING)
ACCOUNT_4=$(assetClient keys show -a $ACCOUNT_NAME_4 $KEYRING)
```
- Step 5

Now load the coins by following this Snippet
```
assetClient tx send $TEST $ACCOUNT_1 10000stake -y $KEYRING $MODE
sleep $SLEEP
```
- Step 6

Now after loading the coins, sending coins to users could be done by
```
assetClient tx send $ACCOUNT_1 $ACCOUNT_3 110stake -y $KEYRING $MODE
sleep $SLEEP
assetClient tx send $ACCOUNT_3 $ACCOUNT_4 10stake -y $KEYRING $MODE
assetClient tx send $ACCOUNT_1 $ACCOUNT_2 100stake -y $KEYRING $MODE
sleep $SLEEP
```

- Step 7

Now recursively sending the coins
```
assetClient tx send $ACCOUNT_1 $ACCOUNT_3 100stake -y $KEYRING $MODE
assetClient tx send $ACCOUNT_3 $ACCOUNT_2 50stake -y $KEYRING $MODE
assetClient tx send $ACCOUNT_2 $ACCOUNT_4 5stake -y $KEYRING $MODE
assetClient tx send $ACCOUNT_4 $ACCOUNT_2 5stake -y $KEYRING $MODE
sleep $SLEEP
```

- Step 8
```


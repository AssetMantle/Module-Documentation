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
assetClient tx metas reveal -y --from $ACCOUNT_1 --metaFact "S|stringValue$NONCE"
assetClient tx metas reveal -y --from $ACCOUNT_2 --metaFact "I|identityValue$NONCE"
assetClient tx metas reveal -y --from $ACCOUNT_3 --metaFact "D|0.101010$NONCE"
assetClient tx metas reveal -y --from $ACCOUNT_4 --metaFact "H|1$NONCE"
sleep $SLEEP
```

- Step 9
How to mutate and mint assets?
```
ASSET_DEFINE_IMMUTABLE_1_ID="assetDefineImmutable1$NONCE"
ASSET_DEFINE_IMMUTABLE_1="$ASSET_DEFINE_IMMUTABLE_1_ID:S|"
ASSET_DEFINE_IMMUTABLE_META_1_ID="assetDefineImmutableMeta1$NONCE"
ASSET_DEFINE_IMMUTABLE_META_1="$ASSET_DEFINE_IMMUTABLE_META_1_ID:I|assetDefineImmutableMeta1$NONCE"
ASSET_DEFINE_MUTABLE_1_ID="assetDefineMutable1$NONCE"
ASSET_DEFINE_MUTABLE_1="$ASSET_DEFINE_MUTABLE_1_ID:D|"
ASSET_DEFINE_MUTABLE_META_1_ID="assetDefineMutableMeta1$NONCE"
ASSET_DEFINE_MUTABLE_META_1="$ASSET_DEFINE_MUTABLE_META_1_ID:H|"
assetClient tx assets define -y --from $ACCOUNT_1 --fromID $ACCOUNT_1_NUB_ID \
  --immutableTraits "$ASSET_DEFINE_IMMUTABLE_1" \
  --immutableMetaTraits "$ASSET_DEFINE_IMMUTABLE_META_1" \
  --mutableTraits "$ASSET_DEFINE_MUTABLE_1" \
  --mutableMetaTraits "$ASSET_DEFINE_MUTABLE_META_1" $KEYRING $MODE

ASSET_DEFINE_IMMUTABLE_2_ID="assetDefineImmutable2$NONCE"
ASSET_DEFINE_IMMUTABLE_2="$ASSET_DEFINE_IMMUTABLE_2_ID:S|"
ASSET_DEFINE_IMMUTABLE_META_2_ID="assetDefineImmutableMeta2$NONCE"
ASSET_DEFINE_IMMUTABLE_META_2="$ASSET_DEFINE_IMMUTABLE_META_2_ID:I|assetDefineImmutableMeta$NONCE"
ASSET_DEFINE_MUTABLE_2_ID="assetDefineMutable2$NONCE"
ASSET_DEFINE_MUTABLE_2="$ASSET_DEFINE_MUTABLE_2_ID:D|"
ASSET_DEFINE_MUTABLE_META_2_ID="assetDefineMutableMeta2$NONCE"
ASSET_DEFINE_MUTABLE_META_2="$ASSET_DEFINE_MUTABLE_META_2_ID:H|"
assetClient tx assets define -y --from $ACCOUNT_2 --fromID $ACCOUNT_2_NUB_ID \
  --immutableTraits "$ASSET_DEFINE_IMMUTABLE_2" \
  --immutableMetaTraits "$ASSET_DEFINE_IMMUTABLE_META_2" \
  --mutableTraits "$ASSET_DEFINE_MUTABLE_2" \
  --mutableMetaTraits "$ASSET_DEFINE_MUTABLE_META_2" $KEYRING $MODE

sleep $SLEEP
ASSET_DEFINE_CLASSIFICATION_1=$(echo $(assetClient q classifications classifications) | awk -v var="$ASSET_DEFINE_IMMUTABLE_META_1_ID" '{for(i=1;i<=NF;i++)if($i==var)print $(i-10)"."$(i-7)}')
assetClient tx assets mint -y --from $ACCOUNT_1 --fromID $ACCOUNT_1_NUB_ID --classificationID $ASSET_DEFINE_CLASSIFICATION_1 --toID $ACCOUNT_1_NUB_ID \
  --immutableProperties "$ASSET_DEFINE_IMMUTABLE_1""stringValue" \
  --immutableMetaProperties "$ASSET_DEFINE_IMMUTABLE_META_1" \
  --mutableProperties "$ASSET_DEFINE_MUTABLE_1""1.01" \
  --mutableMetaProperties "$ASSET_DEFINE_MUTABLE_META_1""123" \
  $KEYRING $MODE

ASSET_DEFINE_CLASSIFICATION_2=$(echo $(assetClient q classifications classifications) | awk -v var="$ASSET_DEFINE_IMMUTABLE_META_2_ID" '{for(i=1;i<=NF;i++)if($i==var)print $(i-10)"."$(i-7)}')
assetClient tx assets mint -y --from $ACCOUNT_2 --fromID $ACCOUNT_2_NUB_ID --classificationID $ASSET_DEFINE_CLASSIFICATION_2 --toID $ACCOUNT_2_NUB_ID \
  --immutableProperties "$ASSET_DEFINE_IMMUTABLE_2""stringValue" \
  --immutableMetaProperties "$ASSET_DEFINE_IMMUTABLE_META_2" \
  --mutableProperties "$ASSET_DEFINE_MUTABLE_2""1.01" \
  --mutableMetaProperties "$ASSET_DEFINE_MUTABLE_META_2""123" \
  $KEYRING $MODE

sleep $SLEEP
ASSET_MINT_1=$(echo $(assetClient q assets assets) | awk -v var="$ASSET_DEFINE_CLASSIFICATION_1" '{for(i=1;i<=NF;i++)if($i==var)print $i"|"$(i+3)}')
ASSET_MINT_2=$(echo $(assetClient q assets assets) | awk -v var="$ASSET_DEFINE_CLASSIFICATION_2" '{for(i=1;i<=NF;i++)if($i==var)print $i"|"$(i+3)}')

assetClient tx assets mutate -y --from $ACCOUNT_1 --fromID $ACCOUNT_1_NUB_ID --assetID $ASSET_MINT_1 \
  --mutableProperties "$ASSET_DEFINE_MUTABLE_1""1.012" \
  --mutableMetaProperties "$ASSET_DEFINE_MUTABLE_META_1""1234" $KEYRING $MODE
assetClient tx assets burn -y --from $ACCOUNT_2 --fromID $ACCOUNT_2_NUB_ID --assetID $ASSET_MINT_2 $KEYRING $MODE
sleep $SLEEP
#remint asset2
assetClient tx assets mint -y --from $ACCOUNT_2 --fromID $ACCOUNT_2_NUB_ID --classificationID $ASSET_DEFINE_CLASSIFICATION_2 --toID $ACCOUNT_2_NUB_ID \
  --immutableProperties "$ASSET_DEFINE_IMMUTABLE_2""stringValue" \
  --immutableMetaProperties "$ASSET_DEFINE_IMMUTABLE_META_2" \
  --mutableProperties "$ASSET_DEFINE_MUTABLE_2""1.01" \
  --mutableMetaProperties "$ASSET_DEFINE_MUTABLE_META_2""123" \
  $KEYRING $MODE
sleep $SLEEP
```
- Step 10
Reminting assets
```
assetClient tx assets mint -y --from $ACCOUNT_2 --fromID $ACCOUNT_2_NUB_ID --classificationID $ASSET_DEFINE_CLASSIFICATION_2 --toID $ACCOUNT_2_NUB_ID \
  --immutableProperties "$ASSET_DEFINE_IMMUTABLE_2""stringValue" \
  --immutableMetaProperties "$ASSET_DEFINE_IMMUTABLE_META_2" \
  --mutableProperties "$ASSET_DEFINE_MUTABLE_2""1.01" \
  --mutableMetaProperties "$ASSET_DEFINE_MUTABLE_META_2""123" \
  $KEYRING $MODE
sleep $SLEEP
```
- Step 11
Wrapping Unwrapping send coins
```
assetClient tx splits wrap -y --from $ACCOUNT_1 --fromID $ACCOUNT_1_NUB_ID --coins 20stake $KEYRING $MODE
assetClient tx splits wrap -y --from $ACCOUNT_2 --fromID $ACCOUNT_2_NUB_ID --coins 20stake $KEYRING $MODE
assetClient tx splits wrap -y --from $ACCOUNT_3 --fromID $ACCOUNT_3_NUB_ID --coins 20stake $KEYRING $MODE
sleep $SLEEP
assetClient tx splits unwrap -y --from $ACCOUNT_1 --fromID $ACCOUNT_1_NUB_ID --ownableID stake --split 1 $KEYRING $MODE
assetClient tx splits unwrap -y --from $ACCOUNT_2 --fromID $ACCOUNT_2_NUB_ID --ownableID stake --split 1 $KEYRING $MODE
assetClient tx splits unwrap -y --from $ACCOUNT_3 --fromID $ACCOUNT_3_NUB_ID --ownableID stake --split 1 $KEYRING $MODE
sleep $SLEEP
assetClient tx splits send -y --from $ACCOUNT_3 --fromID $ACCOUNT_3_NUB_ID --toID $ACCOUNT_3_NUB_ID --ownableID stake --split 1 $KEYRING $MODE
```
- Step 12
Order make transaction cancel 
```
RDER_MUTABLE_META_TRAITS="takerID:I|,exchangeRate:D|,expiry:H|,makerOwnableSplit:D|"
ORDER_DEFINE_IMMUTABLE_1_ID="orderDefineImmutable1$NONCE"
ORDER_DEFINE_IMMUTABLE_1="$ORDER_DEFINE_IMMUTABLE_1_ID:S|"
ORDER_DEFINE_IMMUTABLE_META_1_ID="orderDefineImmutableMeta1$NONCE"
ORDER_DEFINE_IMMUTABLE_META_1="$ORDER_DEFINE_IMMUTABLE_META_1_ID:I|orderDefineImmutableMeta1$NONCE"
ORDER_DEFINE_MUTABLE_1_ID="orderDefineMutable1$NONCE"
ORDER_DEFINE_MUTABLE_1="$ORDER_DEFINE_MUTABLE_1_ID:D|"
ORDER_DEFINE_MUTABLE_META_1_ID="orderDefineMutableMeta1$NONCE"
ORDER_DEFINE_MUTABLE_META_1="$ORDER_DEFINE_MUTABLE_META_1_ID:H|"
assetClient tx orders define -y --from $ACCOUNT_1 --fromID $ACCOUNT_1_NUB_ID \
  --immutableTraits "$ORDER_DEFINE_IMMUTABLE_1" \
  --immutableMetaTraits "$ORDER_DEFINE_IMMUTABLE_META_1" \
  --mutableTraits "$ORDER_DEFINE_MUTABLE_1" \
  --mutableMetaTraits "$ORDER_DEFINE_MUTABLE_META_1"",takerID:I|,exchangeRate:D|,expiry:H|,makerOwnableSplit:D|" $KEYRING $MODE

ORDER_DEFINE_IMMUTABLE_2_ID="orderDefineImmutable2$NONCE"
ORDER_DEFINE_IMMUTABLE_2="$ORDER_DEFINE_IMMUTABLE_2_ID:S|"
ORDER_DEFINE_IMMUTABLE_META_2_ID="orderDefineImmutableMeta2$NONCE"
ORDER_DEFINE_IMMUTABLE_META_2="$ORDER_DEFINE_IMMUTABLE_META_2_ID:I|orderDefineImmutableMeta2$NONCE"
ORDER_DEFINE_MUTABLE_2_ID="orderDefineMutable2$NONCE"
ORDER_DEFINE_MUTABLE_2="$ORDER_DEFINE_MUTABLE_2_ID:D|"
ORDER_DEFINE_MUTABLE_META_2_ID="orderDefineMutableMeta2$NONCE"
ORDER_DEFINE_MUTABLE_META_2="$ORDER_DEFINE_MUTABLE_META_2_ID:H|"
assetClient tx orders define -y --from $ACCOUNT_2 --fromID $ACCOUNT_2_NUB_ID \
  --immutableTraits "$ORDER_DEFINE_IMMUTABLE_2" \
  --immutableMetaTraits "$ORDER_DEFINE_IMMUTABLE_META_2" \
  --mutableTraits "$ORDER_DEFINE_MUTABLE_2" \
  --mutableMetaTraits "$ORDER_DEFINE_MUTABLE_META_2"",takerID:I|,exchangeRate:D|,expiry:H|,makerOwnableSplit:D|" $KEYRING $MODE

sleep $SLEEP
ORDER_DEFINE_CLASSIFICATION_1=$(echo $(assetClient q classifications classifications) | awk -v var="$ORDER_DEFINE_IMMUTABLE_META_1_ID" '{for(i=1;i<=NF;i++)if($i==var)print $(i-10)"."$(i-7)}')
assetClient tx orders make -y --from $ACCOUNT_1 --fromID $ACCOUNT_1_NUB_ID --classificationID $ORDER_DEFINE_CLASSIFICATION_1 --toID $ACCOUNT_1_NUB_ID \
  --makerOwnableID "$ASSET_MINT_1" --makerOwnableSplit "0.000000000000000001" --takerOwnableID stake \
  --immutableProperties "$ORDER_DEFINE_IMMUTABLE_1""stringValue" \
  --immutableMetaProperties "$ORDER_DEFINE_IMMUTABLE_META_1" \
  --mutableProperties "$ORDER_DEFINE_MUTABLE_1""1.01" \
  --mutableMetaProperties "$ORDER_DEFINE_MUTABLE_META_1""123,takerID:I|,exchangeRate:D|1" \
  $KEYRING $MODE

ORDER_DEFINE_CLASSIFICATION_2=$(echo $(assetClient q classifications classifications) | awk -v var="$ORDER_DEFINE_IMMUTABLE_META_2_ID" '{for(i=1;i<=NF;i++)if($i==var)print $(i-10)"."$(i-7)}')
assetClient tx orders make -y --from $ACCOUNT_2 --fromID $ACCOUNT_2_NUB_ID --classificationID $ORDER_DEFINE_CLASSIFICATION_2 --toID $ACCOUNT_2_NUB_ID \
  --makerOwnableID "$ASSET_MINT_2" --makerOwnableSplit "0.000000000000000001" --takerOwnableID "$ASSET_MINT_1" \
  --immutableProperties "$ORDER_DEFINE_IMMUTABLE_2""stringValue" \
  --immutableMetaProperties "$ORDER_DEFINE_IMMUTABLE_META_2" \
  --mutableProperties "$ORDER_DEFINE_MUTABLE_2""1.01" \
  --mutableMetaProperties "$ORDER_DEFINE_MUTABLE_META_2""123,takerID:I|,exchangeRate:D|1" \
  $KEYRING $MODE

sleep $SLEEP
ORDER_MAKE_1_ID=$(echo $(assetClient q orders orders) | awk -v var="$ORDER_DEFINE_IMMUTABLE_META_1_ID" '{for(i=1;i<=NF;i++)if($i==var)print $(i-19)"*"$(i-16)"*"$(i-13)"*"$(i-10)"*"$(i-7)}')
assetClient tx orders cancel -y --from $ACCOUNT_1 --fromID $ACCOUNT_1_NUB_ID --orderID "$ORDER_MAKE_1_ID" $KEYRING $MODE
ORDER_MAKE_2_ID=$(echo $(assetClient q orders orders) | awk -v var="$ORDER_DEFINE_IMMUTABLE_META_2_ID" '{for(i=1;i<=NF;i++)if($i==var)print $(i-19)"*"$(i-16)"*"$(i-13)"*"$(i-10)"*"$(i-7)}')
sleep $SLEEP
assetClient tx orders take -y --from $ACCOUNT_1 --fromID $ACCOUNT_1_NUB_ID --orderID "$ORDER_MAKE_2_ID" --takerOwnableSplit "0.000000000000000001" $KEYRING $MODE
```




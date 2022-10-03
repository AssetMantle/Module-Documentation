# Module-Documentation :rocket:

Welcome to the Transaction.sh Documentation repo, Here you will find anything and everything about transactions. 

## Lets get started :newspaper:

- Setup

For the setup follow these steps 

```

  rm -rf /root/.AssetMantle/Node/
  assetNode init test --chain-id load-test-1
  assetNode add-genesis-account test <amount> --keyring-backend test
  assetNode gentx --name test --amount <amount> --keyring-backend test
  assetNode collect-gentxs
  systemctl restart mantle-node.service
  systemctl restart mantle-client.service
```

- Adding Keys to Transaction

```
assetClient keys add <name_of_acc> --keyring-backend test
```

- Removal of Keys

```
rm -rf /root/.AssetMantle/Client/
```
- Transaction NubID

Nub ID can be something similar to a username

```
  # $1 = is anything you want
    assetClient tx identities nub --from <account-name> --nubID $1 --chain-id <account-name> --keyring-backend test --yes
```
- Defining ID
```
assetClient tx identities define --immutableProperties "testImmutable1:S|testMutableMeta1" --immutableMetaProperties "testImmutableMeta1:S|testImmutableMeta1234" --mutableProperties "testMutable1:S|testMutableMeta1" --mutableMetaProperties "testMutableMeta1:S|testMutableMeta1" --from=test --fromID "$1" --chain-id load-test-1 --keyring-backend test --yes
# TEST AND TEST 1 ARE ACCOUNT NAME
```
- ISSUING ID 
```
# $1 = classificationID|hshID ; $2 = load-test-1.hashID (class)
  assetClient tx identities issue --immutableProperties "testImmutable1:S|testMutableMeta1" --immutableMetaProperties "testImmutableMeta1:S|testImmutableMeta1234" --mutableProperties "testMutable1:S|testMutableMeta1" --mutableMetaProperties "testMutableMeta1:S|testMutableMeta1" --from=test --fromID "$1" --classificationID "$2" --to=mantle1v74ehyjesvek5ugjsfmw4jz3cxrldcrulm4avr --chain-id load-test-1 --keyring-backend test --yes 
 ```
- PROVISIONING TRANSACTION
```
# $1 = classificationID|hashID
  assetClient tx identities provision --from=test --to=$(assetClient keys show test2 --address --keyring-backend test) --identityID "$1" --chain-id load-test-1 --keyring-backend test --yes 
```
- UNPROVISIONING TRANSACTION
```
# $1 = classificationID|hashID
  assetClient tx identities unprovision --from=test --to=$(assetClient keys show test2 --address --keyring-backend test) --identityID "$1" --chain-id load-test-1 --keyring-backend test --yes
```
- TRANSACTION META REVEAL
```
 assetClient tx metas reveal --from=test --data "S|testMeta1" --keyring-backend test --chain-id load-test-1
```
- Defining Transaction Asset
```
  assetClient tx assets define --from=test --fromID "$1" --immutableProperties "immutable:S|Properties" --immutableMetaProperties "immutableMeta:S|Properties" --mutableProperties "mutable:S|Properties" --mutableMetaProperties "mutableMeta:S|Properties" --keyring-backend test --chain-id "load-test-1"
```
- Minting Transaction Asset
```
 assetClient tx assets mint --from=test --fromID "$1" --classificationID "$2" --toID "$3" --immutableProperties "immutable:S|Properties" --immutableMetaProperties "immutableMeta:S|Properties" --mutableProperties "mutable:S|Properties" --mutableMetaProperties "mutableMeta:S|Properties" --keyring-backend test --chain-id "load-test-1"
```





# Links to resources
* [Catalyst](https://cardano.ideascale.com/)
* [Koios](https://www.koios.rest/)
* [Mesh](https://mesh.martify.io/)
* [Testnets faucet](https://docs.cardano.org/cardano-testnet/tools/faucet)
* [Cardano Improvement Proposals](https://cips.cardano.org/)
* [Cardano GraphQL Voyager](https://input-output-hk.github.io/cardano-graphql/)
* [Preprod GraphQL Playground](http://graphql-api-iohk-preprod.gimbalabs.io/)

## additional resources
* [ESSENTIAL CARDANO](https://www.essentialcardano.io/)
* [Cardano World Repository](https://book.world.dev.cardano.org/)
* [Project Catalyst - Funded Projects Reporting](https://docs.google.com/spreadsheets/u/0/d/1bfnWFa94Y7Zj0G7dtpo9W1nAYGovJbswipxiHT4UE3g/htmlview#)
* [HeadlessUI](https://headlessui.com/)

# cardano-cli commands

## Fetch protocol.json
```Bash
cardano-cli query protocol-parameters --testnet-magic 1097911063 --out-file protocol.json
```

## My wallet addresses
```Bash
$(cat /home/nelsonksh/cardano-src/testnet-wallet-01/payment.addr)
$(cat /home/nelsonksh/cardano-src/testnet-wallet-02/payment.addr)
```

## Build an address
Link to guide: [Creating keys and addresses](https://developers.cardano.org/docs/stake-pool-course/handbook/keys-addresses/)

## Making a transaction
**Build a Transaction:**
```Bash
cardano-cli transaction build \
--babbage-era \
--testnet-magic 1097911063 \
--tx-in $TXIN \
--tx-out $ADDR+<Number of Lovelace> \
--change-address $SENDER \
--out-file tx.draft
```
**Sign a Transaction:**
```Bash
cardano-cli transaction sign \
--tx-body-file tx.draft \
--signing-key-file payment.skey \
--testnet-magic 1097911063 \
--out-file tx.signed
```
**Submit a Transaction:**
```Bash
cardano-cli transaction submit \
--testnet-magic 1097911063 \
--tx-file tx.signed
```


## Minting token
**Create policy script**
```Bash
touch my-policy.script
```
Generate keyHash
```Bash
cardano-cli address key-hash --payment-verification-key-file payment.vkey
```
Inside it should be
```text
{
    "keyHash": "<generated hashKey>",
    "type": "sig"
}
```
Then finally generate policy id
```Bash
cardano-cli transaction policyid --script-file my-policy.script
```

## Including metadata
Create a `metadata.json` file
```json
{
    "1337": "greetings from ppbl summer 2022"
}
```
* [CIP 25](https://cips.cardano.org/cips/cip25/) for NFT


## Build the minting transaction along with metadata
```Bash
cardano-cli transaction build \
--babbage-era \
--testnet-magic 1097911063 \
--tx-in $TXIN \
--tx-out $MINTER+"1500000 + 25 $POLICYID.$TOKENNAME1 + 25 $POLICYID.$TOKENNAME2" \
--mint "25 $POLICYID.$TOKENNAME1 + 25 $POLICYID.$TOKENNAME2" \
--mint-script-file $POLICYSCRIPT \
--change-address $MINTER \
--metadata-json-file $METADATA_JSON_FILE \
--protocol-params-file protocol.json \
--out-file mint-native-assets.raw
```


## Generate keyHash of an address
```Bash
cardano-cli address key-hash --payment-verification-key-file payment.vkey
```

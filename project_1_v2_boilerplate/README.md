# Create Your Own Private Blockchain

Installing dependencies:

```
npm install
```

Running the application:

```
node app.js
```

Running tests:

1- Check if the genesis block was created when running the app for the first time (hash and time values may vary):

```
curl http://127.0.0.1:8000/block/0
```
```json
{
    "hash": "$HASH",
    "height": 0,
    "body": "7b2264617461223a2247656e6573697320426c6f636b227d",
    "time": "1561304801",
    "previousBlockHash": null
}
```

2- Send the message:

```
curl -X POST -H 'Content-Type:application/json' -d '{"address": "$ADDR" }'  http://127.0.0.1:8000/requestValidation
```
```
$ADDR:$TIMESTAMP:starRegistry
```

3- Sign the message in your bitcoin wallet and copy the message signature

```
#unlock your wallet if it is encrypted
walletpassphrase "$PASSWORD" 360

#get an address
getnewaddress
#getnewaddress "" legacy #use legacy mode in case of using testnet

#sign the message
signmessage "$ADDR" "$ADDR:$TIMESTAMP:starRegistry"
```

4- Submit a star for the signed message using your `wallet address`, `message`, `signature` and `star object`:

```
curl \
-X POST \
-H 'Content-Type:application/json' \
-d '{"address": "$ADDR", "message": "$MSG", "signature": "$SIG", "star": {"dec": "68° 52' 56.9", "ra": "16h 29m 1.0s", "story": "Testing the story 4"} }' \
http://127.0.0.1:8000/submitstar
```  

It should work if the time you requested the message ownership and star submission if less than 5 minutes and a new block will be added into the blockchain.

The block can then be retrieved by its height:

```
curl http://127.0.0.1:8000/block/1
```
```json
{
    "hash": "$HASH",
    "height": 0,
    "body": "7b2264617461223a2247656e6573697320426c6f636b227d",
    "time": "1561304801",
    "previousBlockHash": null
}
```

You can also get all stars from a given wallet:

```
curl http://127.0.0.1:8000/blocks/$ADDR
```
```json
[
    {
        "dec":"68 52 56.9",
        "ra":"16h 29m 1.0s",
        "story":"Testing the story 4"
    }
]
```

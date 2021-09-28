# Contributing

## Development

Create `.env` file in the root directory with the environment variables below:

```
APP_ENV=development
APP_SECRET=
API_HOST=localhost
API_PORT=4445
MONGO_URL=mongodb://localhost:27017
MONGO_DB=dapsi
MONGO_POOL=50
PAGE_DEFAULT_LIMIT=25
PAGE_MAX_LIMIT=100
PRIVATE_KEY=
CONTRACT_ADDRESS=
ETH_ACCESS_KEY_ID=
ETH_SECRET_ACCESS_KEY=
ETH_RPC=
```

## Commands

Follow these commands to start:

```bash
# Migrate the database. Make sure that the metadata.wallet-address-nonce contains the latest nonce number of the executor wallet.
npm run upgrade
# Generate JWT token.
npm run generate:token $ETH_ADDRESS $CONTRACT_ID
# Start server
npm run start:rest
# See package.json for more.
```

Post a signature:

```bash
APP_SECRET="" npm run generate:token $WALLET $CONTRACT_ID
curl \
    -X POST \
    -H "Content-Type: application/json" \
    -H "Authorization: $JWT" \
    -d '{"hash": "0x...","signature":"0x..."}' \
    http://localhost:4445/contracts/611e7cef5f40de69f0b9f5b6/sign/{provider|client}
```

Verify a contract (id: 611e7cef5f40de69f0b9f5b6):

```bash
curl \
    -X GET \
    -H "Content-Type: application/json" \
    http://localhost:4445/contracts/611e7cef5f40de69f0b9f5b6/verify
```

Revoke an existing contract:

```bash
APP_SECRET="" npm run generate:revoke-token $CONTRACT_ID
curl \
    -X POST \
    -H "Content-Type: application/json" \
    -H "Authorization: $JWT" \
    http://localhost:4445/contracts/611e7cef5f40de69f0b9f5b6/revoke
```

### Initializing the contract

First deploy a new NFT contract.

Next, create a document in the `metadata` collection, holding the last last contract emitter nonce number. See the example below:

```mongo
db.metadata.insert([{
	"name" : "wallet-address-nonce",
	"value" : 0, // initial nonce for a fresh contract
}])
```

## Deployment

If everything is properly set up, we should be able to deploy lambda functions by running the commands below:

```sh
$ nvm use 14
$ npm i
$ npm run build
$ npm run deploy -- --stage test
```

We can remove an already deployed lambda function:

```sh
$ npm run deploy:remove -- --stage test
```

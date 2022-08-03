# Awesome AssetMantle

Hi, welcome to our developer documentation. Here, you will find everything about AssetMantle. Stay tuned, we will update this docs gradually.

## Examples

This section includes examples related to **mantle-1** chain. Gradually, we will update this section thoroughly.

### JavaScript

- Generate an address from mnemonics

  ```javascript
  // JavaScript Examples

  import { DirectSecp256k1HdWallet } from "@cosmjs/proto-signing";
  import { stringToPath } from "@cosmjs/crypto";

  const mnemonic = "<your mnemonic>";
  const wallet = await DirectSecp256k1HdWallet.fromMnemonic(
    mnemonic,
    { prefix: "mantle" },
    stringToPath("m/44'/118'/0'/0/0")
  );
  const account = await wallet.getAccounts();
  console.log(account[0].address);

  // result: Output Structure

  mantle1g9szh5sg0kps4k30343kswgvlus2wn7v2ecfkk;
  ```

- Send your transaction to mantle address

  ```javascript
  // JavaScript Examples

  import { DirectSecp256k1HdWallet } from "@cosmjs/proto-signing";
  import { stringToPath } from "@cosmjs/crypto";
  import {
    assertIsDeliverTxSuccess,
    SigningStargateClient,
    calculateFee,
    GasPrice,
    coins,
  } from "@cosmjs/stargate";

  const mnemonic = "<your mnemonics here>";
  const wallet = await DirectSecp256k1HdWallet.fromMnemonic(
    mnemonic,
    { prefix: "mantle" },
    stringToPath("m/44'/118'/0'/0/0")
  );
  const account = await wallet.getAccounts();
  const rpcURL = "https://rpc.assetmantle.one";
  const rpcClient = await SigningStargateClient.connectWithSigner(
    rpcURL,
    wallet
  );
  const recipient = "<reciver account address>";

  const tokens = coins(1000, "umntl");
  const gasFee = {
    amount: [
      {
        denom: "umntl",
        amount: "0",
      },
    ],
    gas: "200000",
  };

  const txHashOut = await rpcClient.sendTokens(
    account[0].address,
    recipient,
    tokens,
    gasFee,
    "Transaction"
  );
  assertIsDeliverTxSuccess(txHashOut);
  ```

- Create new account on **mantle-1** chain

  ```javascript
  // JavaScript Examples

  import { DirectSecp256k1HdWallet } from "@cosmjs/proto-signing";
  import { stringToPath } from "@cosmjs/crypto";

  const wallet = await DirectSecp256k1HdWallet.generate(
      24,
      { prefix: "mantle" },
      stringToPath("m/44'/118'/0'/0/0")
  );

  console.log(wallet.mnemonic);
  const account = await wallet.getAccounts();
  console.log(account[0].address);

  // result: Output Structure

  become whale old bleak goose leader vivid armed digital shuffle fire input donor perfect patch found illegal basic vanish feel man try fall cactus
  mantle1y7pmvzpgwfvvq6zupqjfcgntauc90ww73wxs8s
  ```

## Use Cases

So, this section includes use cases of **mantleNode**, **RPC Endpoint**, **Websockets**, **REST**, or **LCD Endpoint**.

> 🚀 NOTE: Please, make sure you download essential packages, to test below examples. _Packages:_ go, jq, gcc, make, curl, git, vim and mantleNode (from github).

### MantleNode

- Initialize the default configurations

  ```bash
  # --keyring-backend and --home flags are optional
  mantleNode init "<your-moniker-name>" --chain-id "mantle-1" --home "/home/node/.mantleNode
  ```

- Create keys on your remote system

  ```bash
  # --keyring-backend and --home flags are optional
  mantleNode keys add "<key-name>" --keyring-backend "os" --home "/home/node/.mantleNode"
  ```

- Recover your keys with mnemonics on your remote system

  ```bash
  # --keyring-backend and --home flags are optional
  mantleNode keys add "<key-name>" --recover --keyring-backend "os" --home "/home/node/.mantleNode"
  ```

- Or, if you want to import your keys from private keyfile

  ```bash
  # --keyring-backend and --home flags are optional
  mantleNode keys import "<key-name>" "<key-file>" --keyring-backend "os" --home "/home/node/.mantleNode"
  ```

- Query your account balance

  ```bash
  mantleNode query bank balances "<address>" --chain-id "mantle-1" --node "https://rpc.assetmantle.one:443" --output "json"
  ```

- Query your account (address, number, sequences, and pub_key)

  ```bash
  mantleNode query account "<address>" --chain-id "mantle-1" --node "https://rpc.assetmantle.one:443" --output "json"
  ```

### Websockets

- Subscribe an event which output new block events

  - Wesocket Example

  ```json
  {
    "jsonrpc": "2.0",
    "method": "subscribe",
    "params": ["tm.event='NewBlock'"],
    "id": 1
  }
  ```

  - Result Output Structure (snipped ...)

  ```json
  {
  "jsonrpc": "2.0",
  "id": 1,
  "result": {
      "query": "tm.event='NewBlock'",
      "data": {
      "type": "tendermint/event/NewBlock",
      "value": {
          "block": {
          "header": {
              "version": {
              "block": "11"
              },
              "chain_id": "mantle-1",
              "height": "1494200",
  ```

- Subscribe an event which output latest transaction events

  - Websocket Example

  ```json
  {
    "jsonrpc": "2.0",
    "method": "subscribe",
    "params": ["tm.event='Tx'"],
    "id": 1
  }
  ```

  - Result Output Structure (snipped ...)

  ```json
  {
  "jsonrpc": "2.0",
  "id": 1,
  "result": {
      "query": "tm.event='NewBlock'",
      "data": {
      "type": "tendermint/event/NewBlock",
      "value": {
          "block": {
          "header": {
              "version": {
              "block": "11"
              },
              "chain_id": "mantle-1"
  ```

- Subscribe an event which output latest incoming transactions to a particular address events

  - Websocket Example

  ```json
  {
    "jsonrpc": "2.0",
    "method": "subscribe",
    "params": [
      "tm.event = 'Tx' AND transfer.recipient = 'mantle1907n5d2xwy3av597y6347dsc2ktpl2d9u0j4tu'"
    ],
    "id": 1
  }
  ```

  - Result Output Structure (snipped ...)

    ```json
    {
        "jsonrpc": "2.0",
        "id": 1,
        "result": {
            "query": "tm.event = 'Tx' AND transfer.recipient = 'mantle1907n5d2xwy3av597y6347dsc2ktpl2d9u0j4tu'",
            "data": {
            "type": "tendermint/event/Tx",
            "value": {
                "TxResult": {
                "height": "1468096",
                "index": 2,
                "tx":
    ```

### Rest Endpoints

- Get the latest blocks

```bash
curl -sL -X GET "https://rest.assetmantle.one/blocks/latest" -H "accept: application/json" | jq "."
```

- Get a block at certain height

```bash
curl -sL -X GET "https://rest.assetmantle.one/blocks/1" -H "accept: application/json" | jq "."
```

- Get the latest validator set

```bash
curl -sL -X GET "https://rest.assetmantle.one/validatorsets/latest" -H "accept: application/json" | jq "."
```

- Get the validator set at certain height

```bash
curl -sL -X GET "https://rest.assetmantle.one/validatorsets/1" -H "accept: application/json" | jq "."
```

- Get a transaction by hash

```bash
curl -sL -X GET "https://rest.assetmantle.one/cosmos/tx/v1beta1/txs/41DDF1E326454CF6E6719D5E2D68DBE6AFE747C39FAF7BB3374029F36AA30EB0" -H "accept: application/json" | jq "."
```

- Total Supply of coins in a chain

```bash
curl -sL -X GET "https://rest.assetmantle.one/bank/total" -H "accept: application/json" | jq "."
```

- Total Supply of particular coin in a chain

```bash
curl -sL -X GET "https://rest.assetmantle.one/bank/total/umntl" -H "accept: application/json" | jq "."
```

### RPC Endpoints

## Resources

This section includes official, developer, community, and social resources.

### Official Resources

- AssetMantle Website — [https://assetmantle.one](https://assetmantle.one)
- AssetMantle Explorer — [https://explorer.assetmantle.one](https://explorer.assetmantle.one)
- AssetMantle MarketPlace — [https://marketplace.assetmantle.one](https://marketplace.assetmantle.one)
- AssetMantle Devnet — [https://devnet.assetmantle.one](https://devnet.assetmantle.one)
- AssetMantle Blog — [https://blog.assetmantle.one](https://blog.assetmantle.one)
- AssetMantle Documentation — [https://docs.assetmantle.one](https://docs.assetmantle.one)
- AssetMantle RPC — [https://rpc.assetmantle.one](https://rpc.assetmantle.one)
- AssetMantle REST or LCD — [https://rest.assetmantle.one](https://rest.assetmantle.one)

### Developer repositories

- MantleNode Repository — https://github.com/AssetMantle/node
- MantleWallet Repository — https://github.com/AssetMantle/wallet
- Devnet Repository — https://github.com/AssetMantle/webApp

### Official Endpoints

We provide following endpoints to help developers for integration

| Protocol | Endpoints                                 |
| :------: | :---------------------------------------- |
|   RPC    | `https://rpc.assetmantle.one`             |
|   REST   | `https://rest.assetmantle.one`            |
|   GRPC   | `grpc://grpc.assetmantle.one`             |
|   WSS    | `wss://rpc.assetmantle.one:443/websocket` |

### State-Sync

To use statesync, change the following under `config.toml`

```toml
[statesync]
enable = true
rpc_servers = "https://rpc.assetmantle.one:443,https://rpc.assetmantle.one:443"
trust_height = 0
trust_hash = ""
trust_period = "112h0m0s"
```

and use a trusted height from the rpc, recent height will work as well

```shell
curl -s https://rpc.assetmantle.one/status | jq '.result .sync_info | {trust_height: .latest_block_height, trust_hash: .latest_block_hash} | values'
```

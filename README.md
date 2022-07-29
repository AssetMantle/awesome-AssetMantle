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

  ```json
  // WebSockets Examples

  {
    "jsonrpc": "2.0",
    "method": "subscribe",
    "params": ["tm.event='NewBlock'"],
    "id": 1
  }

  // Result: Output Structure

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
    --- snip ---
  ```

- Subscribe an event which output latest transaction events

  ```json
  // WebSockets Examples

  {
    "jsonrpc": "2.0",
    "method": "subscribe",
    "params": ["tm.event='Tx'"],
    "id": 1
  }

  // Result: Output Structure

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
    --- snip ---
  ```

- Subscribe an event which output latest incoming transactions to a particular address events

  ```json
  // WebSockets Examples

  {
    "jsonrpc": "2.0",
    "method": "subscribe",
    "params": [
      "tm.event = 'Tx' AND transfer.recipient = 'mantle1907n5d2xwy3av597y6347dsc2ktpl2d9u0j4tu'"
    ],
    "id": 1
  }

  // Result: Output Structure

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
    --- snip ---
  ```

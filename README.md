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

  const mnemonic = "<your mnemonic";
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

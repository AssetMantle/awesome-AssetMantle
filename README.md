# awesome-AssetMantle

Resource centre for AssetMantle provided by the team and community

### Endpoints

We provide following endpoints to help developers for integration

| Protocol | Endpoints |
| :------: |:--------- |
| RPC | `https://rpc.assetmantle.one` |
| REST | `https://rest.assetmantle.one` |
| GRPC | `grpc://grpc.assetmantle.one` |
| WSS | `wss://rpc.assetmantle.one:443/websocket` |

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

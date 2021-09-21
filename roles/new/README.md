# New

Ansible role to deploy a new Decentralchain node.

For more information see [collection's README](../../README.md).

## Índice

- [Variables](#variables)
- [License](#license)
- [Author](#author)

## Variables

Supported variables are displayed along with
their default values (*./defaults/main.yml*):

```
# If build 'false' image is pulled from Dockerhub using dcc_docker_image_name.
dcc_docker_image_build: true
dcc_docker_image_name: 'runitcr/dcc'
dcc_repository: https://github.com/runitsolutions/DCC.git
dcc_repository_branch: 'docker-image'
# If wallet seed empty one is randomly generated.
dcc_wallet_seed: ''
dcc_wallet_password: 'myWalletSuperPassword'
dcc_log_level: 'error'
dcc_heap_size: '2G'
dcc_java_opts: >-
  -Dwaves.network.declared-address=0.0.0.0:{{ dcc_port }}
  -Dwaves.network.node-name=-{{ dcc_node_name }}
  -Dwaves.rest-api.api-key-hash={{ dcc_rest_api_key_hash }}
  -Dwaves.rest-api.bind-address=0.0.0.0
  -Dwaves.rest-api.port={{ dcc_port_api }}
dcc_network: 'testnet'
dcc_node_name: 'dcc'
dcc_rest_api_key_hash: 'mySuperApiKeyHash'
# Use 6868 for Mainnet, 6862 for Stagenet, 6863 for Testnet.
dcc_port: 6863
dcc_port_api: 6869
dcc_docker_volumes_path: /docker/waves
dcc_known_peers:
  ["testnet-node.decentralchain.io:6863", "testnet-node1.decentralchain.io:6863", "testnet-node2.decentralchain.io:6863"]
dcc_config_template: 'waves.conf.j2'
dcc_genesis_average_block_delay: '60s'
dcc_genesis_initial_base_target: 87
dcc_genesis_timestamp: 1
dcc_genesis_block_timestamp: '{{ dcc_genesis_timestamp }}'
dcc_genesis_signature:
  '3o8pqCkFtUCyGQNqik8ZWBns7AwBdtLabPVqnbRLneq4ZEfxNYnCpPfwMXpyrN3UpXQ8YSWUhkgvKgJaewaA1Th'
dcc_genesis_initial_balance: 10000000000000000
dcc_genesis_transactions: |
    {recipient = "31ZuVWB9frppJNyBaeFyhF3xGLdfVLPsiC", amount = 5000000000000000},
    {recipient = ""31ZibNeNgp45VgNDNZMjqPuCjqk5xCtTBo2"", amount = 5000000000000000}
dcc_extra_config: |
  extensions += "com.wavesplatform.events.BlockchainUpdates"
  extensions += "com.wavesplatform.dex.grpc.integration.DEXExtension"
  grpc.host = "127.0.0.1"
  dex.grpc.integration.host = "127.0.0.1"
```

## License

MIT.

## Autor

![Runitcr](../../img/author.png)

[Runitcr](https://runitcr.com).
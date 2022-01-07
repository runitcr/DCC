# New

Ansible role to deploy a new Decentralchain node.

For more information see [collection's README](../../README.md).

## Index

- [Variables](#variables)
- [License](#license)
- [Author](#author)

## Variables

Supported variables are displayed along with
their default values (*./defaults/main.yml*):

```
# If wallet seed empty one is randomly generated.
dcc_wallet_seed: ''
dcc_wallet_password: 'myWalletSuperPassword'
dcc_log_level: 'error'
dcc_heap_size: '2G'
dcc_java_opts: >-
  -Dwaves.network.declared-address={{ hostvars[inventory_hostname]['ansible_default_ipv4']['address'] }}:{{ dcc_port }}
  -Dwaves.network.node-name={{ dcc_node_name }}
  -Dwaves.rest-api.api-key-hash={{ _dcc_rest_api_key_hash }}
  -Dwaves.rest-api.bind-address=0.0.0.0
  -Dwaves.rest-api.port={{ dcc_port_api }}
dcc_network: 'testnet'
dcc_node_name: 'dcc'
dcc_rest_api_key_hash: 'mySuperApiKeyHash'
# Use 6868 for Mainnet, 6862 for Stagenet, 6863 for Testnet.
dcc_port: '6863'
dcc_port_api: '6869'
dcc_docker_volumes_path: /docker/waves
dcc_known_peers:
  '["testnet-node.decentralchain.io:6863", "testnet-node1.decentralchain.io:6863", "testnet-node2.decentralchain.io:6863"]'
dcc_config_template: 'waves.conf.j2'
dcc_genesis_average_block_delay: '60s'
dcc_genesis_initial_base_target: 87
dcc_genesis_timestamp: 1
dcc_genesis_block_timestamp: '{{ dcc_genesis_timestamp }}'
dcc_genesis_signature:
  '3k7jW4J97iJ3Q1obDFFSfzywfvPG4Lg7R8tWgk9yXN2NJt9HUUPKvXuuLNyh8jvBMrJcRUe4grV6rR5BgizmpY22'
dcc_genesis_initial_balance: 10000000000000000
dcc_genesis_transactions: |
    {recipient = "31ZuVWB9frppJNyBaeFyhF3xGLdfVLPsiC", amount = 5000000000000000},
    {recipient = "31ZibNeNgp45VgNDNZMjqPuCjqk5xCtTBo2", amount = 5000000000000000}
dcc_extra_config: |
  extensions += "com.wavesplatform.events.BlockchainUpdates"
  grpc.host = "0.0.0.0"
  dex.grpc.integration.host = "0.0.0.0"

docker_image_build: false
docker_image_name: 'runitcr/dcc'
docker_image_repository: https://github.com/runitsolutions/DCC.git
docker_image_repository_branch: 'version-1.3.5'
dockerhub_image_pull: true
dockerhub_user: 'runitcr'
dockerhub_password: 'mySuperHubPassword'
dockerhub_mail: 'runitcr@guerrillamail.com'
dockerhub_image_push: false
```

## License

MIT.

## Autor

![Runitcr](../../img/author.png)

[Runitcr](https://runitcr.com).
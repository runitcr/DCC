[![DCC Actions Status](https://github.com/runitcr/dcc/workflows/CI/badge.svg)](https://github.com/runitcr/dcc/actions)

# Decentralchain Node

[![Logo](img/logo.png)](https://github.com/runitcr/dcc)

Ansible collection to deploy and install [Decentralchain nodes](https://github.com/Decentral-America/DCC).

## Index

- [Install](#install).
- [Playbooks](#playbooks).
- [Dependencies](#dependencies).
- [Compatibility](#compatibility).
- [Development](#development).
  - [Installing LXD dependencies](#installing-lxd-dependencies).
  - [Installing Python dependencies](#installing-python-dependencies).
  - [Creating a virtual Python environment](#creating-a-virtual-python-environment).
  - [Running ansible-test](#running-ansible-test).
  - [Running Molecule tests](#running-molecule-tests).
  - [Building Docker image manually](#building-docker-image-manually).
  - [Using Ansible Vault](#using-ansible-vault).
- [License](#license).
- [Author](#author).

## Install

Install Python dependencies

```
pip install ansible cryptography docker flake8 molecule-lxd voluptuous yamllint
```

Install the collection:

```
ansible-galaxy collection install runitcr.dcc
```

Install the required Ansible roles:

```
ansible-galaxy install -r ~/.ansible/ansible_collections/runitcr/dcc/requirements.yml
```

## Playbooks

List of included playbooks and how to use them.

| Name | Description | Command |
| --- | --- | --- |
| [image](playbooks/image.yml) | Builds a Decentralchain node image and push it to Dockerhub. | `ansible-playbook runitcr.dcc.image \`<br>`--ask-vault-pass` |
| [new](playbooks/new.yml) | Deploys a new Decentralchain node. | `ansible-playbook runitcr.dcc.new \`<br>`-i  ~/.ansible/collections/ansible_collections/runitcr/dcc/inventory.yml \`<br>`-l test.runitcr.com \`<br>`-e ansible_python_interpreter=/usr/bin/python3` |
| [security](playbooks/security.yml) | Applies server hardening. Must be runned twice **on first run** to correctly install wazuh.agent role. | **First Run**<br>----------------<br>`ANSIBLE_REMOTE_PORT=22 ANSIBLE_REMOTE_USER=root ansible-playbook runitcr.dcc.security \`<br>`-i  ~/.ansible/collections/ansible_collections/runitcr/dcc/inventory.yml \`<br>`-l test.runitcr.com \`<br>`-e user_key_dir=/home/user/.ssh \`<br>`-e ansible_python_interpreter=/usr/bin/python3`<br>**All**<br>----------------<br>`ansible-playbook runitcr.dcc.security \`<br>`-i  ~/.ansible/collections/ansible_collections/runitcr/dcc/inventory.yml \`<br>`-l test.runitcr.com \`<br>`-e user_key_dir=/home/user/.ssh \`<br>`-e ansible_python_interpreter=/usr/bin/python3` |

## Dependencies

### Ansible

- [geerlingguy.docker](https://github.com/geerlingguy/ansible-role-docker).
- [geerlingguy.pip](https://github.com/geerlingguy/ansible-role-pip).
- [geerlingguy.security](https://github.com/geerlingguy/ansible-role-security).
- [wazuh.ansible-wazuh-agent](wazuh/ansible-wazuh-agent).

### Python

- [ansible](https://pypi.org/project/ansible).
- [flake8](https://pypi.org/project/flake8).
- [voluptuous](https://pypi.org/project/voluptuous).
- [yamllint](https://pypi.org/project/yamllint).

## Compatibility

- Ansible >= 2.10.
- Debian Bullseye.
- Ubuntu Focal.
- Python 3.

## Development

### LXD

In order to execule Molecule tests is necessary to have
the LXC/LXD system running properly, follow next steps
to configure it:

Install [Snapd](https://es.wikipedia.org/wiki/Snappy):

```
sudo apt update
sudo apt install -y snapd
```

Install Sanp core and [LXD](https://linuxcontainers.org/lxd/introduction):

```
snap install core &>/dev/null && snap install core
snap install lxd &>/dev/null && snap install lxd
```

Add to *~/.bashrc* the path to Snap as part of the
[PATH](https://rootsudo.wordpress.com/2014/04/06/el-path-la-ruta-de-linux-variables-de-entorno/comment-page-1):

```
grep -Fxq 'export PATH=$PATH:/snap/bin/' ~/.bashrc && echo 'export PATH=$PATH:/snap/bin/' >> ~/.bashrc
```

Add your user to the *lxd* group:

```
sudo adduser $(whoami) lxd
```

Create a *lxd-init-preseed* file:

```
printf 'config: {}
networks:
- config:
    ipv4.address: auto
    ipv6.address: auto
  description: ""
  name: lxdbr0
  type: ""
storage_pools:
- name: default
  driver: dir
profiles:
- config: {}
  description: ""
  devices:
    eth0:
      name: eth0
      network: lxdbr0
      type: nic
    root:
      path: /
      pool: default
      type: disk
  name: default
cluster: null' > lxd-init-preseed
```

Update the session:

```
su $(whoami)
```

Create default storage:

```
lxc storage create default dir source=/var/snap/lxd/common/lxd/storage-pools/default
```

Initialize LXD using *lxd-init-preseed* file:

```
cat lxd-init-preseed | lxd init --preseed
```

Verify everything went well:

```
lxd --version
```

### Installing Python dependencies

Install [docker and docker-compose](https://docs.docker.com/engiexitne/install/debian).

Install `Pip`:

```
sudo apt install -y python3-pip
```

Install `Pipenv`:

```
python3 -m pip install pipenv
```

### Creating a virtual Python environment

It's better to work with an isolated Python enviroment,
to create a virtual environment access the collection directory and
enable a virtual environment there:

```
python3 -m pipenv
```

Install the Python dependencies inside the environment:

```
pipenv install
```

You can destroy the environment with:

```
pipenv --rm
exit
```

### Running ansible-test

To execute sanity tests run:

```
ansible-test sanity
```

### Running Molecule tests

To execute molecule tests access the required role folder:

```
cd ./roles/new
```

Execute molecule tests:

```
molecule test
```

### Building Docker image manually

To manually build DCC docker image for testnet follow next steps.

Clone DCC repository:

```
git clone git@github.com:runitcr/DCC.git
```

Enter on the cloned directory:

```
cd DCC
```

Compile Docker image for testnet:

```
./build-with-docker.sh Testnet
```

Build Docker image:

```
docker build -t runitcr/dcc--build-arg WAVES_NETWORK=testnet docker
```

Run Docker image:

```
docker run -v ~/docker/waves/waves-data:/var/lib/waves -v ~/docker/waves/waves-config:/etc/waves -p 6870:6870 -p 6868:6868 -e JAVA_OPTS="-Dwaves.network.declared-address=0.0.0.0:6868 -Dwaves.network.node-name=-my-testnet-node -Dwaves.rest-api.api-key-hash=6nSftY1F5kurz23yLrT1r9YJpiEveBLEa9RB1SCChiqv -Dwaves.rest-api.bind-address=0.0.0.0 -Dwaves.rest-api.port=6870" -e WAVES_WALLET_SEED="TBXHUUcVx2n3Rgszpu5MCybRaR86JGmqCWp7XKh7czU57ox5dgjdX4K4" -e WAVES_WALLET_PASSWORD=myWalletSuperPassword -e WAVES_NETWORK=testnet -ti runitcr/dcc
```

### Using Ansible Vault

To encrypt a single variable value run:

```
ansible-vault encrypt_string 'my_password_value' -n my_password >> group_vars/new_vars.yml
```

To decrypt a single variable run:

```
ansible localhost -m debug -a var=my_password \
    -e @/home/user/.ansible/collections/ansible_collections/runit/dcc/group_vars/new_vars.yml \
    --ask-vault-pass
```

## License

MIT.

## Author

![Runitcr](img/author.png)

[Runitcr](https://runitcr.com).

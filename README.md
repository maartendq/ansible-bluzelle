# Ansible Bluzelle
This repository contains playbooks and roles to install/configure bluzelle validator/sentry for the testnet
Work In progress

## TODO
Investigate if possible to fetch tokens and self delegate the validator

## Requirements
Parts of this setup need to be run with root privileges.

## General

### Initial setup

Make sure you add all your hosts/IPs in inventories/site1/hosts.yml, there's a separate set for validators and sentries.

## Roles

### ansible-role-golang

This role is used for the installation of Golang and we're using all defaults here.
Original role can be found at https://github.com/fubarhouse/ansible-role-golang

### bluzelle

This role is used for installation and configuration of a validator or sentry.

#### Defaults

| Variable                     | Type   | Required  | Default     | Comment                                            |
|------------------------------|--------|-----------|-------------|----------------------------------------------------|
|  runuser                     | string | N/A       | blz         | User that will run software                        |
|  bluzelle_version            | string | N/A       | testnet     | Bluzelle version                                   |
|  chain_id                    | string | N/A       | bluzelle    | Bluzelle chain ID                                  |
|  moniker_name                | list() | Yes       | N/A         | Moniker name of your sentry/validator              |
|  self_delegator              | list() | N/A       | vuser       | User/self_delegator for your validator             |
|  validator_persistent_peers  | string | Yes       | N/A         | Persistent peers for your validator                |
|  sentry_persistent_peers     | string | Yes       | N/A         | Persistent peers for your Sentry                   |
|  sentry_private_peers        | string | N/A       | <empty>     | Private peers for your sentry                      |
|  max_num_inbound_peers       | string | N/A       | 200         | Maximum number of inbound peers                    |
|  max_num_outbound_peers      | string | N/A       | 100         | Maximum number of outbound peers                   |
|  minimum_gas_prices          | string | N/A       | 10.0ubnt    | Minimum gas price                                  |

#### Tasks

##### main.yml

Tasks to be executed on both validator and sentry

##### sentry.yml

Tasks specific to sentry configuration

##### validator.yml

Tasks specific to validator configuration

## Playbooks

There are 2 playbooks available, 1 for sentry installation and 1 for validator installation.
Both playbooks use the same roles but include specific tasks from bluzelle role.

## How to run

Sentry installation:

```
ansible-playbook sentry.yml -i inventories/site1/hosts.yml
```

Validator installation:

```
ansible-playbook validator.yml -i inventories/site1/hosts.yml
```

## Notes

### Validator user creation

The output of the key creation for your self_delegator of the validator is being written to a local file on your server.
This part is not really secure and open to improvement (Thinking of KMS), do take backups in a safe location and remove the file once you've done this.


k3s-setup-ansible
----
This repo contains files to assist with the installation and configuration of k3s on both server and agent nodes.

## Pre-requisites 

To run the ansible playbooks in this repository you will need:

* Password SSH logins setup with your cluster hosts
* Docker/Podman used to run the ansible playbooks from within the [pad92/ansible-alpine](https://hub.docker.com/r/pad92/ansible-alpine/) image

## Setup

Copy the ``inventory-sample.yaml`` to `inventory.yaml` and update the values as required.

## Using Ansible in Docker

The [docker-compose.yaml](./docker-compose.yaml) file can be used to assist with using the assists with using the 
[pad92/ansible-alpine](https://hub.docker.com/r/pad92/ansible-alpine/) image (especially on a Windows machine). 

The syntax for the use of this is:

```shell
docker-compose -f .\docker-compose.yaml run --rm ansible sh <SHELL_COMMANDS>
```

For example, to run the shell command `echo Hello World` use:

```shell
docker-compose -f .\docker-compose.yaml run --rm ansible sh echo Hello World
```

## Playbooks

### Test Playbook

There is a test playbook which can be used to check that everything is working as expected.

```powershell
docker-compose -f .\docker-compose.yaml run --rm ansible sh ansible-playbook -i inventory.yaml playbook/test.yaml
```

### Private Image registry

```powershell
docker-compose -f .\docker-compose.yaml run --rm ansible sh ansible-playbook -i inventory.yaml --check --diff playbook/private-registry.yaml
```


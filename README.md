k3s-setup-ansible
----
This repo contains files to assist with the installation and configuration of k3s on both server and agent nodes.

This project uses:
* [pad92/ansible-alpine](https://github.com/pad92/docker-ansible-alpine) - Docker image to run ansible
* [PyratLabs/ansible-role-k3s](https://github.com/PyratLabs/ansible-role-k3s?tab=readme-ov-file) - k3s Ansible role

## Pre-requisites 

To run the ansible playbooks in this repository you will need:

* Password SSH logins setup with your cluster hosts
* Docker/Podman used to run the ansible playbooks from within the [pad92/ansible-alpine](https://hub.docker.com/r/pad92/ansible-alpine/) image
* Access to the internet to use the [ansible-role-k3s](https://github.com/PyratLabs/ansible-role-k3s?tab=readme-ov-file)

## Setup

Copy the ``inventory-sample.yaml`` to `inventory.yaml` and update the values as required.

## Using Ansible in Docker

The [docker-compose.yaml](./docker-compose.yaml) file can be used to assist with using the assists with using the 
[pad92/ansible-alpine](https://hub.docker.com/r/pad92/ansible-alpine/) image (especially on a Windows machine). 

The [docker-compose.yaml](./docker-compose.yaml) file overrides the entrypoint file on the [pad92/ansible-alpine](https://github.com/pad92/docker-ansible-alpine) 
image allowing you to add the hosts' SSH key to the container, before the entrypoint script runs.

The syntax for the use of this is:

```shell
docker-compose -f .\docker-compose.yaml run --rm ansible sh <SHELL_COMMANDS>
```

For example, to run the shell command `echo Hello World` use:

```shell
docker-compose -f .\docker-compose.yaml run --rm ansible sh echo Hello World
```

## Playbooks

### Ping

```powershell
docker-compose -f .\docker-compose.yaml run --rm ansible sh `
ansible -i inventory.yaml -m ping all
```

### Test Playbook

There is a test playbook which can be used to check that everything is working as expected.

```powershell
docker-compose -f .\docker-compose.yaml run --rm ansible sh `
ansible-playbook -i inventory.yaml playbooks/test.yaml
```

### Retrieve K3S_TOKEN

```powershell
docker-compose -f .\docker-compose.yaml run --rm ansible sh `
ansible-playbook -i inventory.yaml --diff playbooks/retrieve-token.yaml
```

### Retrieve k3s Versions

```powershell
docker-compose -f .\docker-compose.yaml run --rm ansible sh `
ansible-playbook -i inventory.yaml --diff playbooks/retrieve-versions.yaml
```

### HA Setup

This is the main deal and can be used to add/remove nodes from an existing cluster.

```powershell
docker-compose -f .\docker-compose.yaml run --rm ansible sh `
ansible-playbook -i inventory.yaml --diff playbooks/ha-cluster.yaml
```
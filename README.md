# Home Server Ansible Setup

Guest VM is running Ubuntu 24.04 with SSH public key authentication enabled.

## Pre-requisites

1. Install `pipx` and `ansible` on Host

```bash
brew install pipx # OR sudo apt install pipx
pipx install --include-deps ansible # Follow output for PATH instructions
```

## Running

You'll be prompted for a GitHub PAT token. This is to allow Flux to bootstrap your cluster.

```bash
ansible-playbook -i inventory.yml playbook.yml -u <user>
```

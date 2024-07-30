# Pre-requisites

1. Install pipx and ansible

```bash
brew install pipx # OR sudo apt install pipx
pipx install --include-deps ansible # add /Users/<user>/.local/bin to your $PATH
```

# Running

```bash
ansible-playbook -i inventory.yml playbook.yml -u <user>
```

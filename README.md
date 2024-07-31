# Home Server Ansible Setup

Guest VM is running Ubuntu 24.04 with SSH public key authentication enabled.

## Pre-requisites

1. Install `pipx` and `ansible` on Host

```bash
brew install pipx  # OR sudo apt install pipx
pipx install --include-deps ansible  # Follow output for PATH instructions
```

## Running

You'll be prompted for a GitHub PAT token. This is to allow Flux to bootstrap your cluster.

```bash
ansible-playbook -i inventory.yml playbook.yml -u <user>
```

## Using

### Observability

On the guest, port forward the Grafana port:

```bash
kubectl port-forward -n observability service/kube-prom-stack-grafana --address 0.0.0.0 3000:80
```

On the host, tunnel the Grafana port to your local machine:

```bash
ssh -L 3000:localhost:3000 pi@192.168.0.234
```

On the host, open your browser to `http://localhost:3000` and login with `admin`/`prom-operator`.

There are several pre-built dashboards. Have a look for example at `Kubernetes / Compute Resources / Cluster`.

The observability stack includes Loki, which allows you to query logs. Have a look at the `Explore` view in Grafana and select a label filter.

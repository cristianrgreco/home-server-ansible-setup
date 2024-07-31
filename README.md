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

## Useful commands

```bash
# Show status and installed/available addons
microk8s status

# Watch status. Useful after git pushing to the bootstrap repo
flux get kustomizations --watch

# Get status of helm releases. Useful to see what's happening
# Note `helmreleases` can be shortened to `hr`
flux get helmreleases --all-namespaces

# Trigger a reconciliation without waiting for the next interval
flux reconcile hr -n minecraft minecraft

# Suspend a release so you can make temporary changes
flux suspend helmrelease -n minecraft minecraft

# Resumes a suspended helm release
flux resume helmrelease -n minecraft minecraft
```

## To do

1. Grafana configuration. We want to expose Grafana without port-forwarding.

Custom values can be provided like this:

```bash
microk8s enable observability --kube-prometheus-stack-values values.yml
```

Values are here: https://github.com/prometheus-community/helm-charts/blob/main/charts/kube-prometheus-stack/values.yaml

Noteworthy:
```
grafana.adminPassword
grafana.ingress.enabled
grafana.persistence
```

If we have to use ingress, and can't for example set the service type to `NodePort`, then we'll likely have to install nginx-ingress as well. Note that microk8s has an addon called 'ingress'.

2. Enable Minecraft backups

https://github.com/itzg/minecraft-server-charts/blob/master/charts/minecraft/values.yaml#L435
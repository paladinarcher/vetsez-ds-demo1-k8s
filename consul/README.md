# [Consul Chart](https://github.com/hashicorp/consul-helm)

## Installation
```
git clone https://github.com/hashicorp/consul-helm; \
    helm install -n consul -f values.yaml consul-helm; \
    rm -rf consul-helm;
```

## Uninstallation
```bash
helm delete --purge consul
```
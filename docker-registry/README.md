
## Generate Self-Signed Certificates
```bash
openssl req \
       -newkey rsa:2048 -nodes -keyout server.key \
       -x509 -days 365 -out server.crt
```

```bash
cfssl print-defaults config > ca-config.json;
cat <<EOF | cfssl genkey -initca - | cfssljson -bare ca;
{
  "hosts": [
    "ca.default.svc.cluster.local"
  ],
  "CN": "ca.default.svc.cluster.local",
  "key": {
    "algo": "rsa",
    "size": 2048
  }
}
EOF
cat <<EOF | cfssl gencert -ca ca.pem -ca-key ca-key.pem -config=ca-config.json -profile=www - | cfssljson -bare server
{
  "hosts": [
    "registry-docker-registry.default.svc.cluster.local",
    "localhost",
    "127.0.0.1"
  ],
  "CN": "registry-docker-registry.default.svc.cluster.local",
  "key": {
    "algo": "rsa",
    "size": 2048
  }
}
EOF
```

### Create TLS secret
```bash
kubectl create secret tls docker-registry-tls --key server-key.pem --cert server.pem
```

### Create ConfigMap with cert for clients
Create a ConfigMap in K8s that can be mounted into any container that needs to communicate with the docker registrys

```bash
cat <<EOF | kubectl apply -f -
apiVersion: v1
kind: ConfigMap
metadata:
  name: docker-registry-ca
  namespace: default
data:
  ca.crt: |
$(cat ca.pem | sed 's/^/    /')
EOF
```

## Allow Minikube to communicate with registry
This process will need to be run each time that Minikube is started.

Add the CA certficate to minikube.
```bash
cat ca.pem | minikube ssh "sudo mkdir -p /etc/docker/certs.d/registry-docker-registry.default.svc.cluster.local:32360 && sudo tee /etc/docker/certs.d/registry-docker-registry.default.svc.cluster.local:32360/ca.crt"
```

Add a host entry to minikube.
```bash
cat '127.0.0.1       registry-docker-registry.default.svc.cluster.local' | minikube ssh "sudo tee -a /etc/hosts"
```
# [Git2Consul Chart](https://github.com/breser/git2consul)
Helm chart for installing `git2consul` service intto Kubernetes

## Installation
To install the git2consul service, run the following command from this directory:
```
helm install -n git2consul -f values.yaml helmChart
```

## Git Authentication
During installation, a Secret will be created for SSH authentication to git. You will need to manual set the SSH private key value
for this Secret.

To add an SSH key for Git authentication:
* Run `cat /path/to/privateKey | base64` and copy the result
* Edit the  `{ReleaseName}-ssh` Secret object
* Update the value of the `id_rsa` key, pasting the base64 encoded private key value.



## Uninstallation
```bash
helm delete --purge git2consul
```
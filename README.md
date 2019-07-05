# Creating MiniKube cluster
This repository holds configurations for Helm charts for services deployed on Kubernetes.

## Install MiniKube
Download and install Minikube following the [instructions for your operating system](https://kubernetes.io/docs/tasks/tools/install-minikube/)

## Start the cluster
```bash
minikube start --memory 8192 --cpus 4 --disk-size 75g
```
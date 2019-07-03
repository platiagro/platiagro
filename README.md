# PlatIAgro

[![License](https://img.shields.io/badge/License-Apache%202.0-blue.svg)](https://opensource.org/licenses/Apache-2.0)
[![Gitter](https://badges.gitter.im/platiagro/community.svg)](https://gitter.im/platiagro/community?utm_source=badge&utm_medium=badge&utm_campaign=pr-badge)

----

AI Platform for pushing Ag-Tech forward.

----

## Requirements

Ensure that you have a [Kubernetes Cluster](https://kubernetes.io/docs/setup/) and [kubectl](https://kubernetes.io/docs/tasks/tools/install-kubectl/#install-kubectl) configured for running commands against the Kubernetes cluster.

## 1. Install the Controller and UI

```shell
kubectl create ns platiagro
kubectl apply -n platiagro -f https://raw.githubusercontent.com/platiagro/platiagro/master/manifests/install.yaml
```

## 2. Access the PlatIAgro UI

```shell
kubectl proxy
```

Then visit: http://127.0.0.1:8001/api/v1/namespaces/platiagro/services/platiagro-ui/proxy/

## Cleanup

To undeploy PlatIAgro, run:

```shell
kubectl delete -n platiagro -f https://raw.githubusercontent.com/platiagro/platiagro/master/manifests/install.yaml
kubectl delete namespace platiagro
```

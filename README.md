# PlatIAgro

[![License](https://img.shields.io/badge/License-Apache%202.0-blue.svg)](https://opensource.org/licenses/Apache-2.0)
[![Gitter](https://badges.gitter.im/platiagro/community.svg)](https://gitter.im/platiagro/community?utm_source=badge&utm_medium=badge&utm_campaign=pr-badge)

----

AI Platform for pushing Ag-Tech forward.

---
**NOTE**

As of today, Kubernetes version 1.14 is supported.

---

## Requirements

Ensure that you have a [Kubernetes Cluster](https://kubernetes.io/docs/setup/), [kubectl](https://kubernetes.io/docs/tasks/tools/install-kubectl/#install-kubectl), and [kfctl](https://www.kubeflow.org/docs/started/getting-started/#installing-command-line-tools) configured for running commands against the Kubernetes cluster.

## Install PlatIAgro

```shell
export APP="platiagro"
export CONFIG="https://raw.githubusercontent.com/platiagro/kubeflow/v0.0.1-kubeflow-v0.6.2/bootstrap/config/kfctl_platiagro.yaml"
kfctl init ${APP} --config=${CONFIG} -V
cd ${APP}
kubectl create namespace kubeflow-anonymous
kubectl -n kubeflow-anonymous create serviceaccount default-editor
kfctl generate all -V
kfctl apply all -V
sudo docker pull platiagro/datascience-notebook
sudo docker pull platiagro/autosklearn-notebook
```

Then visit: http://localhost:31380/

## Delete PlatIAgro

To undeploy PlatIAgro, run:

```shell
cd ${APP}
kfctl delete all -V
kubectl delete namespace istio-system
kubectl delete namespace kubeflow-anonymous
```

## Troubles

Please read [INSTALLATION.md](https://github.com/platiagro/platiagro/blob/master/INSTALLATION.md)

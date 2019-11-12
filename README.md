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
export KF_NAME=platiagro
export BASE_DIR=$(pwd)
export KF_DIR=${BASE_DIR}/${KF_NAME}
export CONFIG_URI="https://raw.githubusercontent.com/platiagro/manifests/platiagro-v0.0.2/kfdef/kfctl_platiagro.yaml"
mkdir -p ${KF_DIR}
cd ${KF_DIR}
kfctl apply -V -f ${CONFIG_URI}
sudo docker pull platiagro/datascience-1386e2046833-notebook-cpu:v0.5.0
curl "http://127.0.0.1:31380/kubeflow/api/workgroup/create" -H "content-type: application/json" --data '{"namespace":"anonymous"}'
```

Then visit: http://localhost:31380/

## Delete PlatIAgro

To undeploy PlatIAgro, run:

```shell
export CONFIG_FILE=${KF_DIR}/kfctl_platiagro.yaml
cd ${KF_DIR}
kfctl delete -f ${CONFIG_FILE}
kubectl delete profile --all
kubectl delete namespace istio-system knative-serving
```

## Troubles

Please read [INSTALLATION.md](https://github.com/platiagro/platiagro/blob/master/INSTALLATION.md)

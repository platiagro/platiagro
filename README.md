# PlatIAgro

[![License](https://img.shields.io/badge/License-Apache%202.0-blue.svg)](https://opensource.org/licenses/Apache-2.0)
[![Gitter](https://badges.gitter.im/platiagro/community.svg)](https://gitter.im/platiagro/community?utm_source=badge&utm_medium=badge&utm_campaign=pr-badge)

---

AI Platform for pushing Ag-Tech forward.

---

**NOTE**

As of today, Kubernetes versions 1.14 and 1.15 are supported.

---

## Requirements

Ensure that you have a [Kubernetes Cluster](https://kubernetes.io/docs/setup/), [kubectl](https://kubernetes.io/docs/tasks/tools/install-kubectl/#install-kubectl), and [kfctl](https://www.kubeflow.org/docs/started/getting-started/#installing-command-line-tools) configured for running commands against the Kubernetes cluster.

## Install PlatIAgro (HTTP)

```shell
export KF_NAME=platiagro
export BASE_DIR=$(pwd)
export KF_DIR=${BASE_DIR}/${KF_NAME}
export CONFIG_URI="https://raw.githubusercontent.com/platiagro/manifests/v0.1.0-kubeflow-v1.0-branch/kfdef/kfctl_platiagro.v0.1.0.yaml"
mkdir -p ${KF_DIR}
cd ${KF_DIR}
kfctl apply -V -f ${CONFIG_URI}
```

Then visit: http://<LOAD-BALANCER-IP>/

## Install PlatIAgro (HTTPS)

```shell
export KF_NAME=platiagro
export BASE_DIR=$(pwd)
export KF_DIR=${BASE_DIR}/${KF_NAME}
export CONFIG_URI="https://raw.githubusercontent.com/platiagro/manifests/v0.1.0-kubeflow-v1.0-branch/kfdef/kfctl_platiagro_tls.v0.1.0.yaml"
mkdir -p ${KF_DIR}
cd ${KF_DIR}
kfctl apply -V -f ${CONFIG_URI}
```

Then visit: https://<LOAD-BALANCER-IP>/

## Delete PlatIAgro

To undeploy PlatIAgro, run:

```shell
export CONFIG_FILE=${KF_DIR}/kfctl_platiagro.v0.1.0.yaml
cd ${KF_DIR}
kfctl delete -f ${CONFIG_FILE}
kubectl delete profile --all
kubectl delete namespace istio-system
```

## Troubles

Please read [INSTALLATION.md](https://github.com/platiagro/platiagro/blob/master/INSTALLATION.md)

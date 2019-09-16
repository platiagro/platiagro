# PlatIAgro

[![License](https://img.shields.io/badge/License-Apache%202.0-blue.svg)](https://opensource.org/licenses/Apache-2.0)
[![Gitter](https://badges.gitter.im/platiagro/community.svg)](https://gitter.im/platiagro/community?utm_source=badge&utm_medium=badge&utm_campaign=pr-badge)

----

AI Platform for pushing Ag-Tech forward.

----

## Requirements

Ensure that you have a [Kubernetes Cluster](https://kubernetes.io/docs/setup/), [kubectl](https://kubernetes.io/docs/tasks/tools/install-kubectl/#install-kubectl), and [kfctl](https://www.kubeflow.org/docs/started/getting-started/#installing-command-line-tools) configured for running commands against the Kubernetes cluster.

## Install PlatIAgro

```shell
export KFAPP="<your choice of application directory name>"
export CONFIG="https://raw.githubusercontent.com/platiagro/kubeflow/platiagro/bootstrap/config/kfctl_platiagro.yaml"
kfctl init ${KFAPP} --config=${CONFIG} -V
cd ${KFAPP}
kfctl generate all -V
kfctl apply all -V
```

Then visit: http://localhost:31380/

## Delete PlatIAgro

To undeploy PlatIAgro, run:

```shell
cd ${KFAPP}
kfctl delete all -V
```

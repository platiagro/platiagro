# PlatIAgro

[![License](https://img.shields.io/badge/License-Apache%202.0-blue.svg)](https://opensource.org/licenses/Apache-2.0)
[![Gitter](https://badges.gitter.im/platiagro/community.svg)](https://gitter.im/platiagro/community?utm_source=badge&utm_medium=badge&utm_campaign=pr-badge)

----

AI Platform for pushing Ag-Tech forward.

----

## Requirements

Ensure that you have a [Kubernetes Cluster](https://kubernetes.io/docs/setup/), [kubectl](https://kubernetes.io/docs/tasks/tools/install-kubectl/#install-kubectl), [Helm](https://github.com/helm/helm/blob/master/docs/install.md) and [kfctl](https://www.kubeflow.org/docs/started/getting-started/#installing-command-line-tools) configured for running commands against the Kubernetes cluster.

## Install PlatIAgro

```shell
export KFAPP="kubeflow"
export CONFIG="https://raw.githubusercontent.com/platiagro/kubeflow/platiagro/bootstrap/config/kfctl_platiagro.yaml"
kfctl init ${KFAPP} --config=${CONFIG} -V
cd ${KFAPP}
kubectl create namespace kubeflow-anonymous
kfctl generate all -V
kfctl apply all -V
helm install seldon-core-operator --name seldon-core --set istio.enabled=true --set istio.gateway=kubeflow-gateway --repo https://storage.googleapis.com/seldon-charts --set usageMetrics.enabled=true --namespace kubeflow
```

Then visit: http://localhost:31380/

## Delete PlatIAgro

To undeploy PlatIAgro, run:

```shell
cd ${KFAPP}
kfctl delete all -V
kubectl delete namespace istio-system
kubectl delete namespace kubeflow-anonymous
helm del --purge seldon-core
```

## Troubles

Please read [INSTALLATION.md](https://github.com/platiagro/platiagro/blob/master/INSTALLATION.md)

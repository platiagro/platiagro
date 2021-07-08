<img src="./images/platiagro.png" width="200">

AI Platform for pushing Ag-Tech forward.

---

Visit [our website](https://www.cpqd.com.br/inovacao/platiagro/) and [docs](https://platiagro.github.io/) to learn about this project.

---

## Installing PlatIAgro on an existing Kubernetes cluster

Ensure that you have the following prerequisites before installing PlatIAgro:

- `Kubernetes` (tested with version `1.18`) with a default [StorageClass](https://kubernetes.io/docs/concepts/storage/storage-classes/)
- `kustomize` (version `3.2.0`) ([download link](https://github.com/kubernetes-sigs/kustomize/releases/tag/v3.2.0))
    - :warning: PlatIAgro is built on top of [Kubeflow](https://www.kubeflow.org), and Kubeflow 1.3.0 is not compatible with the latest versions of of kustomize 4.x.
- `kubectl`

There are 4 installation modes:
```
platiagro           without authentication and ready for CPU.
platiagro-gpu       without authentication and ready for GPU.
platiagro-auth      with authentication and ready for CPU.
platiagro-gpu-auth  with authentication and ready for GPU.
```

Adjust the variable `INSTALL_DIR=platiagro`, then run the following commands:

```shell
export INSTALL_DIR=platiagro
git clone --single-branch --branch v0.2.0-kubeflow-v1.3-branch https://github.com/platiagro/manifests.git
cd manifests
while ! kustomize build $INSTALL_DIR | kubectl apply -f -; do echo "Retrying to apply resources"; sleep 10; done
```

Then visit http://[LOAD-BALANCER-HOST]/

## One-click installation on Google Kubernetes Engine (GKE)

Visit https://platiagro-gcp.herokuapp.com/ and follow the instructions.

## Useful Guides

- [Create a Kubernetes cluster on Ubuntu 18.04](./KUBERNETES-ON-UBUNTU.md)
- [Virtualization with kvm and libvirt](./VIRTUALIZATION.md)
- [Prepare your cluster to use NVIDIA GPUs](./NVIDIA-GPU.md)

## Releasing PlatIAgro components

Create a tag that matches the pattern v*, i.e. v1.0, v0.2.0, then push it.

`git tag v0.1.0 && git push origin tags/v0.1.0`

**This will trigger an action that creates tags and releases in all repositories.**

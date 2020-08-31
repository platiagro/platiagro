# Installation guide for Ubuntu 16.04/18.04/20.04, Debian Jessie/Stretch/Buster

## Installing NVIDIA drivers on Linux instances

This step depends on the GPU Type/Version.

See these instructions for AWS EC2 instances: https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/install-nvidia-driver.html#nvidia-GRID-driver

## Installing NVIDIA Container Toolkit

The steps below were copied from: https://github.com/NVIDIA/nvidia-docker

```shell
distribution=$(. /etc/os-release;echo $ID$VERSION_ID)
curl -s -L https://nvidia.github.io/nvidia-docker/gpgkey | sudo apt-key add -
curl -s -L https://nvidia.github.io/nvidia-docker/$distribution/nvidia-docker.list | sudo tee /etc/apt/sources.list.d/nvidia-docker.list

sudo apt-get update && sudo apt-get install -y nvidia-container-toolkit nvidia-container-runtime
sudo systemctl restart docker
```

## NVIDIA device plugin for Kubernetes

The steps below were copied from: https://github.com/NVIDIA/k8s-device-plugin#preparing-your-gpu-nodes

```shell
# Add the package repositories
distribution=$(. /etc/os-release;echo $ID$VERSION_ID)
curl -s -L https://nvidia.github.io/nvidia-docker/gpgkey | sudo apt-key add -
curl -s -L https://nvidia.github.io/nvidia-docker/$distribution/nvidia-docker.list | sudo tee /etc/apt/sources.list.d/nvidia-docker.list

sudo apt-get update && sudo apt-get install -y nvidia-docker2
sudo systemctl restart docker
```

Edit the file `/etc/docker/daemon.json`:

```shell
{
    "default-runtime": "nvidia",
    "runtimes": {
        "nvidia": {
            "path": "/usr/bin/nvidia-container-runtime",
            "runtimeArgs": []
        }
    }
}
```

## Install Kubernetes Cluster following [these steps](./INSTALLATION.md)

## Deploying NVIDIA GPU device plugin

```shell
kubectl create -f https://raw.githubusercontent.com/NVIDIA/k8s-device-plugin/v0.6.0/nvidia-device-plugin.yml
```

## Usage:

Specify the GPU type in the Pod spec:

```shell
apiVersion: v1
kind: Pod
metadata:
  name: cuda-vector-add
spec:
  restartPolicy: OnFailure
  containers:
    - name: cuda-vector-add
      # https://github.com/kubernetes/kubernetes/blob/v1.7.11/test/images/nvidia-cuda/Dockerfile
      image: "k8s.gcr.io/cuda-vector-add:v0.1"
      resources:
        limits:
          nvidia.com/gpu: 1 # requesting 1 GPU
```

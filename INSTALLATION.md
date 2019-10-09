# Install guide on Ubuntu 18.04

## Docker

```shell
sudo apt install docker.io
sudo systemctl start docker
sudo systemctl enable docker
```

## Helm

```shell
wget https://github.com/kubeflow/kubeflow/releases/download/v0.6.2/kfctl_v0.6.2_linux.tar.gz
tar -xvf kfctl_v0.6.2_linux.tar.gz
sudo mv kfctl /usr/local/bin/kfctl
```

## Kfctl

```shell
sudo apt install docker.io
sudo systemctl start docker
sudo systemctl enable docker
```

## Kubernetes

```shell
curl -s https://packages.cloud.google.com/apt/doc/apt-key.gpg | sudo apt-key add
sudo apt-add-repository "deb http://apt.kubernetes.io/ kubernetes-xenial main"
sudo apt-get install kubeadm=1.14.4-00 kubelet=1.14.4-00 kubectl=1.14.4-00
```

# Init

## Kubernetes

```shell
sudo sysctl net.bridge.bridge-nf-call-iptables=1
sudo swapoff -a
sudo kubeadm init

rm -rf $HOME/.kube
mkdir -p $HOME/.kube
sudo cp -i /etc/kubernetes/admin.conf $HOME/.kube/config
sudo chown $(id -u):$(id -g) $HOME/.kube/config
kubectl apply -f "https://cloud.weave.works/k8s/net?k8s-version=$(kubectl version | base64 | tr -d '\n')"
```

## Helm

```shell
kubectl taint nodes --all node-role.kubernetes.io/master-
kubectl create serviceaccount --namespace kube-system tiller
kubectl create clusterrolebinding tiller-cluster-rule --clusterrole=cluster-admin --serviceaccount=kube-system:tiller
kubectl patch deploy --namespace kube-system tiller-deploy -p '{"spec":{"template":{"spec":{"serviceAccount":"tiller"}}}}'      
helm init --service-account tiller
```

## Create local persistent volume (https://www.kubeflow.org/docs/other-guides/troubleshooting/)

```shell
sudo mkdir /mnt/pv1
sudo mkdir /mnt/pv2
sudo mkdir /mnt/pv3

kubectl create -f - <<EOF
kind: PersistentVolume
apiVersion: v1
metadata:
  name: pv-volume1
spec:
  storageClassName:
  capacity:
    storage: 10Gi
  accessModes:
    - ReadWriteOnce
  hostPath:
    path: "/mnt/pv1"
---
kind: PersistentVolume
apiVersion: v1
metadata:
  name: pv-volume2
spec:
  storageClassName:
  capacity:
    storage: 20Gi
  accessModes:
    - ReadWriteOnce
  hostPath:
    path: "/mnt/pv2"
---
kind: PersistentVolume
apiVersion: v1
metadata:
  name: pv-volume3
spec:
  storageClassName:
  capacity:
    storage: 20Gi
  accessModes:
    - ReadWriteOnce
  hostPath:
    path: "/mnt/pv3"
EOF
```

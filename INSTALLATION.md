# Install guide on Ubuntu 18.04

## Disable swap -- [reference](https://www.tecmint.com/disable-swap-partition-in-centos-ubuntu/)

```shell
sudo swapoff -a
```

Search for the swap line and comment the entire line by adding a # in front of the line

```shell
sudo vi /etc/fstab
sudo reboot
sudo mount -a
```

Check if the swap area has been completely and permanently disabled in your system.

```shell
free -h
blkid
lsblk
```

## Docker

```shell
sudo apt install docker.io
sudo systemctl start docker
sudo systemctl enable docker
```

## Kfctl

```shell
wget https://github.com/kubeflow/kfctl/releases/download/v1.0-rc.4/kfctl_v1.0-rc.3-1-g24b60e8_linux.tar.gz
tar -xvf kfctl_v1.0-rc.3-1-g24b60e8_linux.tar.gz
sudo mv kfctl /usr/local/bin/kfctl
```

## Kubernetes

```shell
curl -s https://packages.cloud.google.com/apt/doc/apt-key.gpg | sudo apt-key add
sudo apt-add-repository "deb http://apt.kubernetes.io/ kubernetes-xenial main"
sudo apt-get install kubeadm=1.15.7-00 kubelet=1.15.7-00 kubectl=1.15.7-00
```

# Init

## Kubernetes

```shell
sudo sysctl net.bridge.bridge-nf-call-iptables=1
sudo kubeadm init

rm -rf $HOME/.kube
mkdir -p $HOME/.kube
sudo cp -i /etc/kubernetes/admin.conf $HOME/.kube/config
sudo chown $(id -u):$(id -g) $HOME/.kube/config
kubectl apply -f "https://cloud.weave.works/k8s/net?k8s-version=$(kubectl version | base64 | tr -d '\n')"
kubectl create clusterrolebinding cluster-admin-binding --clusterrole=cluster-admin --user=admin --user=kubelet --group=system:serviceaccounts
kubectl taint nodes --all node-role.kubernetes.io/master-
```

## Create local persistent volume

Add the following bind mounts to `/etc/fstab`. **Then reboot the machine.**
```shell
/l/disk0/disk-0                           /mnt/disks/disk-0       none    defaults,bind   0 0
/l/disk0/disk-1                           /mnt/disks/disk-1       none    defaults,bind   0 0
/l/disk0/disk-2                           /mnt/disks/disk-2       none    defaults,bind   0 0
/l/disk0/disk-3                           /mnt/disks/disk-3       none    defaults,bind   0 0
/l/disk0/disk-4                           /mnt/disks/disk-4       none    defaults,bind   0 0
/l/disk0/disk-5                           /mnt/disks/disk-5       none    defaults,bind   0 0
```

Create local-storage-class and set default

```shell
cat <<EOF | kubectl apply -f -
kind: StorageClass
apiVersion: storage.k8s.io/v1
metadata:
  name: local-storage
provisioner: kubernetes.io/no-provisioner
reclaimPolicy: Delete
EOF

kubectl patch storageclass local-storage -p '{"metadata": {"annotations":{"storageclass.kubernetes.io/is-default-class":"true"}}}'

cat <<EOF | kubectl apply -f -
---
# Source: provisioner/templates/provisioner.yaml
apiVersion: v1
kind: ConfigMap
metadata:
  name: local-provisioner-config
  namespace: default
  labels:
    heritage: "Tiller"
    release: "release-name"
    chart: provisioner-2.3.2
data:
  storageClassMap: |
    local-storage:
       hostDir: /mnt/disks
       mountDir: /mnt/disks
       blockCleanerCommand:
         - "/scripts/shred.sh"
         - "2"
       volumeMode: Filesystem
---
apiVersion: apps/v1
kind: DaemonSet
metadata:
  name: local-volume-provisioner
  namespace: default
  labels:
    app: local-volume-provisioner
    heritage: "Tiller"
    release: "release-name"
    chart: provisioner-2.3.2
spec:
  selector:
    matchLabels:
      app: local-volume-provisioner
  template:
    metadata:
      labels:
        app: local-volume-provisioner
    spec:
      serviceAccountName: local-storage-admin
      containers:
        - image: "quay.io/external_storage/local-volume-provisioner:v2.3.2"
          name: provisioner
          securityContext:
            privileged: true
          env:
          - name: MY_NODE_NAME
            valueFrom:
              fieldRef:
                fieldPath: spec.nodeName
          - name: MY_NAMESPACE
            valueFrom:
              fieldRef:
                fieldPath: metadata.namespace
          - name: JOB_CONTAINER_IMAGE
            value: "quay.io/external_storage/local-volume-provisioner:v2.3.2"
          volumeMounts:
            - mountPath: /etc/provisioner/config
              name: provisioner-config
              readOnly: true
            - mountPath: /dev
              name: provisioner-dev
            - mountPath: /mnt/disks
              name: disks
              mountPropagation: "HostToContainer"
      volumes:
        - name: provisioner-config
          configMap:
            name: local-provisioner-config
        - name: provisioner-dev
          hostPath:
            path: /dev
        - name: disks
          hostPath:
            path: /mnt/disks

---
# Source: provisioner/templates/provisioner-service-account.yaml

apiVersion: v1
kind: ServiceAccount
metadata:
  name: local-storage-admin
  namespace: default
  labels:
    heritage: "Tiller"
    release: "release-name"
    chart: provisioner-2.3.2

---
# Source: provisioner/templates/provisioner-cluster-role-binding.yaml

apiVersion: rbac.authorization.k8s.io/v1
kind: ClusterRoleBinding
metadata:
  name: local-storage-provisioner-pv-binding
  labels:
    heritage: "Tiller"
    release: "release-name"
    chart: provisioner-2.3.2
subjects:
- kind: ServiceAccount
  name: local-storage-admin
  namespace: default
roleRef:
  kind: ClusterRole
  name: system:persistent-volume-provisioner
  apiGroup: rbac.authorization.k8s.io
---
apiVersion: rbac.authorization.k8s.io/v1
kind: ClusterRole
metadata:
  name: local-storage-provisioner-node-clusterrole
  labels:
    heritage: "Tiller"
    release: "release-name"
    chart: provisioner-2.3.2
rules:
- apiGroups: [""]
  resources: ["nodes"]
  verbs: ["get"]
---
apiVersion: rbac.authorization.k8s.io/v1
kind: ClusterRoleBinding
metadata:
  name: local-storage-provisioner-node-binding
  labels:
    heritage: "Tiller"
    release: "release-name"
    chart: provisioner-2.3.2
subjects:
- kind: ServiceAccount
  name: local-storage-admin
  namespace: default
roleRef:
  kind: ClusterRole
  name: local-storage-provisioner-node-clusterrole
  apiGroup: rbac.authorization.k8s.io
EOF
```

## Create load balancer

**Before running the command below, add the external IP address after `KUBEFLOW_MASTER_IP_ADDRESS=`**

```shell
kubectl apply -f https://raw.githubusercontent.com/google/metallb/v0.8.3/manifests/metallb.yaml

export KUBEFLOW_MASTER_IP_ADDRESS=

cat <<EOF | kubectl apply -f -
apiVersion: v1
kind: ConfigMap
metadata:
  namespace: metallb-system
  name: config
data:
  config: |
    address-pools:
    - name: default
      protocol: layer2
      addresses:
      - $KUBEFLOW_MASTER_IP_ADDRESS-$KUBEFLOW_MASTER_IP_ADDRESS
EOF
```
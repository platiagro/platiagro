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

## Create local persistent volume

Create volumes on the worker node

```shell
kubectl taint nodes --all node-role.kubernetes.io/master-
kubectl create serviceaccount --namespace kube-system tiller
kubectl create clusterrolebinding tiller-cluster-rule --clusterrole=cluster-admin --serviceaccount=kube-system:tiller
kubectl patch deploy --namespace kube-system tiller-deploy -p '{"spec":{"template":{"spec":{"serviceAccount":"tiller"}}}}'      
helm init --service-account tiller
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
       fsType: tmpfs
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
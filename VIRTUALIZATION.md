# Virtualization with kvm and libvirt

## Create

```shell
virt-install --name=platIA \
--cdrom=./ubuntu-18.04.3-desktop-amd64.iso \
--os-type linux \
--vcpus=4 \
--memory=10240 \
--disk size=100 \
--network type=direct,source=br0,source_mode=bridge,model=virtio
```

## Clone

```shell
virt-clone \
--original=platIA \
--name=platIA-clone \
--file=/var/lib/libvirt/images/platIA-clone.qcow2
```

## Useful commands

### List all

```shell
virsh list --all
```

### Start

```shell
virsh start platIA
```

### Shutdown

```shell
virsh shutdown platIA
```

### Edit configuration

```shell
virsh edit platIA
```

### Delete

```shell
virsh undefine platIA
```
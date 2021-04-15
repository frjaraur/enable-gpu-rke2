# RKE2 NVIDIA GPU Enablement Playbooks

This repository holds Ansible playbooks to automate the process of enabling NVIDIA GPU resources to RKE2 Based Clusters.

## What does this playbook do?

The intent of this playbook is to prepare RKE2 nodes with NVIDIA GPUs attached to a state where the resource can be advertised to the rest of the cluster.

This playbook runs through the following process:
```
- Installs prerequisite packages
- Installs the NVIDIA CUDA Repository
- Installs NVIDIA Driver, Blacklists Nouveau, regenerates initramfs, modifies GRUB
- Reboot machine (Waits up to 5 Minutes)
- Verifies RKE2 installation, applies ContainerD GPU Patch
- Restarts RKE2 Service/Agent
```

Keep in mind, the reboot process on the node *will* cause downtime in your cluster.  For that reason, it is suggested to slowly roll out this playbook to the relevant nodes as to minimize the blast radius.

## What does this playbook NOT do?

This playbook does *not* install the NVIDIA K8 device plugin for you automatically.  That is left to the user to install on their respective cluster.  

## What OS(s) are Currently Supported?

Currently intended for RHEL and CentOS 7 and 8 machines.

## Running this playbook
Add your machines to the `hosts.ini` file, then run the ansible-playbook command.
```sh
ansible-playbook -i hosts.ini site.yml
```

## Verify GPUs after install
To verify that your GPU is detected via the device driver, run the following:

```sh
ansible -i hosts.ini all -b -a "nvidia-smi"
```

## Add NVIDIA device plugin for Kubernetes via Helm
As always, check NVIDIA's official documentation for the latest instructions.  The values here are provided for convenience:

Please ensure that helm is installed.
```sh
helm repo add nvdp https://nvidia.github.io/k8s-device-plugin
helm repo update
helm install \
    --version=0.9.0 \
    --generate-name \
    nvdp/nvidia-device-plugin
```

For instructions on other options available via the helm install see
[NVIDIA/k8s-device-plugin](https://github.com/NVIDIA/k8s-device-plugin#deployment-via-helm) .


# ADDING GPU TO RKE2

## Running this playbook
Add your machines to the `hosts.ini` file, then run the ansible-playbook command.
```sh
ansible-playbook -i hosts.ini site.yml
```

## Verify GPUs
```sh
ansible -i hosts.ini all -b -a "nvidia-smi"
```

## Add NVIDIA device plugin for Kubernetes via Helm
Ensure that helm is installed.
```sh
helm repo add nvdp https://nvidia.github.io/k8s-device-plugin
helm repo update
helm install \
    --version=0.9.0 \
    --generate-name \
    nvdp/nvidia-device-plugin
```

For instructions on other options avaialable via the helm install see
[NVIDIA/k8s-device-plugin](https://github.com/NVIDIA/k8s-device-plugin#deployment-via-helm) .


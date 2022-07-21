<div align="center">

# My home cluster managed with Flux
_based on the amazing work from [k8s-at-home/flux-cluster-template](https://github.com/k8s-at-home/flux-cluster-template)_

<img src="https://camo.githubusercontent.com/5b298bf6b0596795602bd771c5bddbb963e83e0f/68747470733a2f2f692e696d6775722e636f6d2f7031527a586a512e706e67" align="center" width="144px" height="144px"/>
</div>
<br/>

<div align="center">

[![Discord](https://img.shields.io/discord/673534664354430999?style=for-the-badge&label=discord&logo=discord&logoColor=white)](https://discord.gg/k8s-at-home)
[![k3s](https://img.shields.io/badge/k3s-v1.24.3+k3s1-brightgreen?style=for-the-badge&logo=kubernetes&logoColor=white)](https://k3s.io/)

</div>

## 📖 Overview
My [k3s](https://k3s.io/) cluster consists of 3 Raspberry Pi 4 Model B 8GB server nodes running Fedora 36 Server and 1 x86 agent node. It is all provisioned using the [Ansible](https://www.ansible.com/) galaxy role [ansible-role-k3s](https://github.com/PyratLabs/ansible-role-k3s).

### Core Components

- [flux](https://toolkit.fluxcd.io/) - GitOps operator for managing Kubernetes clusters from a Git repository
- [kube-vip](https://kube-vip.io/) - Load balancer for the Kubernetes control plane nodes
- [metallb](https://metallb.universe.tf/) - Load balancer for Kubernetes services
- [cert-manager](https://cert-manager.io/) - Operator to request SSL certificates and store them as Kubernetes resources
- [calico](https://www.tigera.io/project-calico/) - Container networking interface for inter pod and service networking
- [external-dns](https://github.com/kubernetes-sigs/external-dns) - Operator to publish DNS records to Cloudflare (and other providers) based on Kubernetes ingresses
- [k8s_gateway](https://github.com/ori-edge/k8s_gateway) - DNS resolver that provides local DNS to your Kubernetes ingresses
- [ingress-nginx](https://kubernetes.github.io/ingress-nginx/) - Kubernetes ingress controller used for a HTTP reverse proxy of Kubernetes ingresses
- [local-path-provisioner](https://github.com/rancher/local-path-provisioner) - provision persistent local storage with Kubernetes

For provisioning the following tools will be used:

- [Fedora 36 Server](https://getfedora.org/en/server/download/) - Universal operating system that supports running all kinds of home related workloads in Kubernetes and has a faster release cycle
- [Ansible](https://www.ansible.com) - Provision Fedora Server and install k3s
- [Terraform](https://www.terraform.io) - Provision an already existing Cloudflare domain and certain DNS records to be used with your k3s cluster

## 📂 Repository structure

The Git repository contains the following directories under `cluster` and are ordered below by how Flux will apply them.

```sh
📁 cluster      # k8s cluster defined as code
├─📁 flux       # flux, gitops operator, loaded before everything
├─📁 crds       # custom resources, loaded before 📁 core and 📁 apps
├─📁 charts     # helm repos, loaded before 📁 core and 📁 apps
├─📁 config     # cluster config, loaded before 📁 core and 📁 apps
├─📁 core       # crucial apps, namespaced dir tree, loaded before 📁 apps
└─📁 apps       # regular apps, namespaced dir tree, loaded last
```

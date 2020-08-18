ansible-role-rke_rancher_clusters
==================

Create a new Kubernetes Cluster with RKE and deploys Rancher on it

Requirements
------------

Only uses python and ansible.


Role Variables
--------------

```yaml
---
rke_binary_version: v1.1.4
rke_binary_download_url: "https://github.com/rancher/rke/releases/download/{{ rke_binary_version }}/rke_linux-amd64"
rke_cluster_bin: "./rke-{{ rke_binary_version }}"
rke_cluster_kube_config: "kube_config_config-{{ inventory_hostname }}.yml"
rke_cluster_config: "config-{{ inventory_hostname }}.yml"

helm_binary_version: v3.2.4
helm_binary_download_url: "https://get.helm.sh/helm-{{ helm_binary_version }}-linux-amd64.tar.gz"
helm_binary: "./helm"
helm_home: ".helm"

kubectl_binary_version: v1.18.0
kubectl_binary_download_url: "https://storage.googleapis.com/kubernetes-release/release/{{ kubectl_binary_version }}/bin/linux/amd64/kubectl"
kubectl_binary: "./kubectl-{{ kubectl_binary_version }}"

rancher_repo: rancher-stable
rancher_repo_url: "https://releases.rancher.com/server-charts/stable"

kubernetes_version: v1.18.6-rancher1-1
rancher_version: v2.4.5

rancher_failover_ip: []

rancher_hostname: ""
rancher_admin_password: ""
rancher_admin_initial_password: admin
rancher_telemetry: out

## Due to https://bugzilla.redhat.com/show_bug.cgi?id=1527565
rke_ssh_user: centos
rke_ssh_agent_auth: true
rke_network_interface: eth0


# Cert Manager for Lets Encrypt Certs
certmanager_enabled: false
rancher_certmanager_version: 0.16.0
jetstack_repo: jetstack
jetstack_repo_url: https://charts.jetstack.io
certmanager_crd_url: "https://github.com/jetstack/cert-manager/releases/download/v{{ rancher_certmanager_version }}/cert-manager.crds.yaml"

rke_rancher_tls_crt: ""
rke_rancher_tls_key: ""
rke_rancher_tls_cacerts: ""
# rke_rancher_tls_self_signed needs to be true if the Rancher TLS cert is self-signed, then also the
# full CA chain needs to be provided via rke_rancher_tls_cacerts. 
# See https://rancher.com/blog/2020/transport-layer-security-p2 for more information
rke_rancher_tls_self_signed: false
```

Dependencies
------------

* none

License
-------

GPLv3

Author Information
------------------

* Sebastian Plattner (plattner@puzzle.ch)

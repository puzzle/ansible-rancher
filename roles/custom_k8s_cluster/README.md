custom_k8s_cluster
==================

Create a new custom K8S cluster on the Rancher host and add master and worker nodes

Requirements
------------

Only uses python and ansible.


Role Variables
--------------

```

# Rancher API Key
custom_k8s_cluster_api_key: ""
# Rancher API Host
custom_k8s_cluster_rancher_host: ""
custom_k8s_cluster_rancher_api: "https://{{ custom_k8s_cluster_rancher_host }}/v3"
custom_k8s_cluster_verify_ssl: yes

# Cluster Name, defaults to the inventory Name [custom_k8s_clusters] Ansible Group without the 'rancher-' Prefix
custom_k8s_cluster_name: "{{ inventory_hostname | regex_replace('rancher-') }}"


# Enable Pod Security Policy
custom_k8s_cluster_enable_psp: True
# Default Pod Security Policy
custom_k8s_clusters_default_psp: restricted
# Enable Network Segregation between Projects
custom_k8s_cluster_enable_network_policy: True
# Kubernetes Version
custom_k8s_cluster_kubernetes_version: v1.17.2-rancher1-2
# Ingress Provider (none, nginx)
custom_k8s_clusters_ingress_provider: none

# Rancher Agent to be used
custom_k8s_cluster_agent_version: v2.3.5

# Base command for the Ranger Agent
custom_k8s_cluster_docker_commmand_base: "docker run -d --privileged --restart=unless-stopped --net=host -v /etc/kubernetes:/etc/kubernetes -v /var/run:/var/run rancher/rancher-agent:{{ custom_k8s_cluster_agent_version}} --server https://{{ custom_k8s_cluster_rancher_host }}"


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

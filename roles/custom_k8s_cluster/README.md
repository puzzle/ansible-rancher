custom_k8s_cluster
==================

Create a new custom K8S cluster on the Rancher host and add master and worker nodes

Requirements
------------

Only uses python and ansible.


Role Variables
--------------

```yaml
---
# Rancher API Key
custom_k8s_cluster_api_key: ""
# Rancher API Host
custom_k8s_cluster_rancher_host: ""
custom_k8s_cluster_rancher_api: "https://{{ custom_k8s_cluster_rancher_host }}/v3"
custom_k8s_cluster_verify_ssl: yes
custom_k8s_cluster_self_signed_certificate: false
custom_k8s_cluster_ca_checksum_param: "{{ '--ca-checksum' if custom_k8s_cluster_self_signed_certificate else '' }}"

# Cluster Name, defaults to the inventory Name [custom_k8s_clusters] Ansible Group without the 'rancher_' Prefix
# As Cluster in Rancher cannot use _ in Names, we need to replace it with -
custom_k8s_cluster_name: "{{ inventory_hostname | regex_replace('rancher_') | regex_replace('_','-') }}"
custom_k8s_cluster_group_inventory_name: "{{ inventory_hostname | regex_replace('rancher_') }}"

# Enable Pod Security Policy
custom_k8s_cluster_enable_psp: true
# Default Pod Security Policy
custom_k8s_clusters_default_psp: "restricted"
# Enable Network Segregation between Projects
custom_k8s_cluster_enable_network_policy: true
# Kubernetes Version
custom_k8s_cluster_kubernetes_version: "v1.18.6-rancher1-1"
# Ingress Provider (none, nginx)
custom_k8s_clusters_ingress_provider: "none"

# Rancher Agent to be used
custom_k8s_cluster_agent_version: "v2.4.5"

# Base command for the Ranger Agent
custom_k8s_cluster_docker_commmand_base: "docker run -d --privileged --restart=unless-stopped --net=host -v /etc/kubernetes:/etc/kubernetes -v /var/run:/var/run rancher/rancher-agent:{{ custom_k8s_cluster_agent_version}} --server https://{{ custom_k8s_cluster_rancher_host }}"

# Custom K8s cluster vIP HA setup
# Useful when you want built-in HA for your custom K8s cluster ingress controller without external LB
custom_k8s_cluster_keepalived_enabled: false
# Specify if the keepalived setup should only use a private IP.
# If so, set "custom_k8s_cluster_keepalived_private_only" to "true"
# and leave all "*_public_*" configuration options down here empty.
custom_k8s_cluster_keepalived_private_only: false
# Specify if the keepalived setup should only use IPv4.
# If so, set "custom_k8s_cluster_keepalived_ipv4_only" to "true"
# and leave all "*_ipv6" configuration options down here empty.
custom_k8s_cluster_keepalived_ipv4_only: false
# Specify a node selector labels if keepalived containers should only run on certain nodes
# If left empty, the daemonset will deploy a replica per node. For example "vip_public" and "vip_private":
custom_k8s_cluster_keepalived_private_node_selector: "vip_private"
custom_k8s_cluster_keepalived_public_node_selector: "vip_public"
# Specify a node toleration label if keepalived containers should be running on tainted certain nodes
custom_k8s_cluster_keepalived_private_node_toleration: ""
custom_k8s_cluster_keepalived_public_node_toleration: "public_ingress"
# Specify where the custom K8s cluster is running. Currently supported environments are:
# - "local": Local keepalived setup
# - "cloudscale": Keepalived setup with cloudscale floating IP
custom_k8s_cluster_keepalived_setup_env: "local"
# If "custom_k8s_cluster_keepalived_setup_env" is set to "cloudscale", a cloudscale API token needs to be provided.
custom_k8s_cluster_keepalived_cloudscale_api_token: "{{ cloudscale_api_token }}"
# Keepalived service Docker image
custom_k8s_cluster_keepalived_image: "puzzle/keepalived:2.0.20"
# Keepalived IP address configuration
custom_k8s_cluster_keepalived_private_failover_track_interface_ip: eth0
custom_k8s_cluster_keepalived_private_failover_ip:
  - vip: "192.0.2.254"
    router_id: 1
    master: rancher01
    password: my-top-secret-password1-here
custom_k8s_cluster_keepalived_public_failover_track_interface_ip: eth0
custom_k8s_cluster_keepalived_public_failover_ip:
  - vip: "198.51.100.254"
    router_id: 2
    master: rancher01
    password: my-top-secret-password2-here
custom_k8s_cluster_keepalived_public_failover_track_interface_ipv6: eth0
custom_k8s_cluster_keepalived_public_failover_ipv6:
  - vip: "2001:db8::ffff"
    router_id: 3
    master: rancher01
    password: my-top-secret-password3-here
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

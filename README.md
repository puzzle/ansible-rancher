# Ansible Playbooks and Roles for Rancher

**Disclaimer!**: We use this as a base for our own and customer setup at puzzle. Heavy work in progress and a lot of things that can be improved.
Feel free to contribute. We are happy to assist.

These Ansible playbook and roles can be used to:
* create a [Rancher Control Plane](https://rancher.com/) using [rke](https://github.com/rancher/rke) and [helm](https://helm.sh/)
* add a custom Kubernetes cluster to an existing Rancher Control Plane

## Prerequisites
We recommend you to run this playbooks inside a [pipenv](https://pipenv.pypa.io/en/latest/).

All dependencies are managed using pipenv, to get a virtual environment use:
```bash
# Only if you don't have pipenv yet:
pip install --user pipenv
```

Switch to the virtual environment and install dependencies into:
```bash
pipenv shell --three
pipenv install
# Now you can run ansible-playbook commands inside this pipenv shell:
ansible-playbook ...
```

You can verify the installed dependencies using `pipenv graph` (inside the pipenv shell):
```bash
$ pipenv graph
ansible==2.9.12
  - cryptography [required: Any, installed: 3.2]
  <--- output truncated --->
  - PyYAML [required: Any, installed: 5.3.1]
jmespath==0.10.0
openshift==0.11.2
  - jinja2 [required: Any, installed: 2.11.2]
    - MarkupSafe [required: >=0.23, installed: 1.1.1]
  - kubernetes [required: ~=11.0.0, installed: 11.0.0]
    - certifi [required: >=14.05.14, installed: 2020.6.20]
  <--- output truncated --->
  - six [required: Any, installed: 1.15.0]
selinux==0.2.1
  - distro [required: >=1.3.0, installed: 1.5.0]
  - setuptools [required: >=39.0, installed: 50.3.2]
```

## Inventory

Check [inventories/site](./inventories/site) for a sample inventory.

There are two special ansible groups:
* `rke_rancher_clusters`: Hosts in this group represent a Rancher Control Plane instance
* `custom_k8s_clusters`: Hosts in this group represent a custom kubernetes cluster added to a Rancher Control Plane

Members (Nodes) of the Rancher Control Plane and the Kubernetes cluster are managed with the following ansible groups.

### Rancher Control Plane

For Rancher Control Plane: Assuming we have a Rancher Control Plane with the name `cluster_rancher`, we create the `cluster_rancher` host to the `rke_rancher_clusters` group and then add all nodes for this to the group `rke_cluster_rancher`, so the Rancher Control Plane name with a `rke_` prefix.

```ini
[rke_rancher_clusters]
cluster_rancher # Belongs to Ansible Group rke_cluster_rancher

[rke_cluster_rancher]
rancher01
rancher02
rancher03

```

Make sure to set at least the following vars:
* For the `cluster_rancher` special host, you have to set `rancher_hostname`, `rancher_admin_password` and `rancher_failover_ip` with its sub-configurations (see [roles/rke_rancher_clusters/README.md](roles/rke_rancher_clusters/README.md) for more information). Check [inventories/host_vars/cluster_rancher.yml](./inventories/host_vars/cluster_rancher.yml) and [roles/rke_rancher_clusters/defaults/main.yml](./roles/rke_rancher_clusters/defaults/main.yml]) for more details.
* Set the `k8s_roles` each of the member should have (for a Rancher Control Plane this is normally `controlplane`, `etcd`, `worker`). See [inventories/group_vars/rke_cluster_rancher.yml](./inventories/group_vars/rke_cluster_rancher.yml) as an example.


### Custom Kubernetes Cluster

For a custom Kubernetes cluster managed with a Rancher Control Plane: Assuming our cluster has the name `mycluster` we create a host `rancher_mycluster` in the `custom_k8s_clusters` group (so cluster name with a `rancher_` prefix). The member nodes of this cluster are then added to a group with the name `mycluster`. To use some dedicated roles on some nodes you can use other ansible groups which are children of the `mycluster` group. 


```ini
[custom_k8s_clusters]
rancher_mycluster

[mycluster:children]
mycluster_master
mycluster_worker

[mycluster_master]
master01

[mycluster_worker]
worker01
```

Make sure to set at least the following vars:
* For the `ranchr_mycluster` special host you have to set at least the `custom_k8s_cluster_api_key` & `custom_k8s_cluster_rancher_host` variable. Check [roles/custom_k8s_cluster/defaults/main.yml](./roles/custom_k8s_cluster/defaults/main.yml]) for more details and variables.
* Set the `k8s_roles` each member should have. See [inventories/group_vars/mycluster_master.yml](./inventories/group_vars/mycluster_master.yml) and [inventories/group_vars/mycluster_worker.yml](./inventories/group_vars/mycluster_worker.yml) as an example.

## Playbooks

### site.yml

Playbook to apply `docker`, `firewalld`, `rke_rancher_clusters` & `custom_rk8s_cluster`. Check [plays/prepare_k8s_nodes.yml](./plays/prepare_k8s_nodes.yml), [plays/deploy_rancher.yml](./plays/deploy_rancher.yml) & [plays/deploy_k8s_cluster.yml](./plays/deploy_k8s_cluster.yml) for details.

### cleanup_k8snode.yml

With this playbook to can cleanup a node which was already added to a kubernetes cluster. Based on https://rancher.com/docs/rancher/v2.x/en/cluster-admin/cleaning-cluster-nodes/

## Roles

### docker

Simple role to install Docker. Check [roles/docker/README.md](./roles/docker/README.md) for more details.

### firewalld

The role only configures firewalld depending on the `k8s_role` the node has (this behaviour can also be disabled if you want to). Based on https://rancher.com/docs/rancher/v2.x/en/installation/options/firewall/

### rke_rancher_clusters

Role to deploy a Rancher Control Plane with `rke` and `helm`. Check [roles/rke_rancher_clusters/README.md](./roles/rke_rancher_clusters/README.md) for more details.

### custom_rk8s_cluster

Role to create a custom Kubernetes cluster on a Rancher Control Plane and add nodes to the cluster. Check [roles/custom_k8s_cluster/README.md](./roles/custom_k8s_cluster/README.md) for more details.

### rancher_keepalived

Role to deploy keepalived Daemonsets on Rancher Control Plane **and** custom Kubernetes clusters. Provides one or multiple highly available virtual IPv4/IPv6 address(es) to the regarding cluster. Usually directly called from `rke_rancher_clusters` and `custom_rk8s_cluster`.

## License

GPLv3

## Author Information

* Sebastian Plattner
* Philip Schmid

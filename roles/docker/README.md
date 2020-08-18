ansible-role-docker
=============================

This Role is used to deploy docker engine on a CentOS/RHEL System.

ToDo

Requirements
------------

To be able to use this role you need the following:
* ansible

Role Variables
--------------

Most of the vars should be quite self exlanatory.

```yaml
---
docker_packages:
  - git
  - yum-utils
  - device-mapper-persistent-data
  - lvm2
docker_pkg_repo: "https://download.docker.com/linux/centos/docker-ce.repo"
Dependencies

docker_package: docker-ce
------------

None

Example Playbook
----------------

A simple example of how to deploy a docker engine

```yaml
- name: docker engine
  hosts: docker_nodes
  roles:
    - role: docker
```

License
-------

GPLv3

Author Information
------------------

* Sebastian Plattner (plattner@puzzle.ch)

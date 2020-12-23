ansible-role-firewalld
==================

Node preparation for hosts to be used with RKE

Requirements
------------

Only uses python and ansible.


Role Variables
--------------

```yaml
---
k8s_roles:
- controlplane
- etcd
- worker
enable_firewalld: true
manage_rancher_related_firewalld_rules: true
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

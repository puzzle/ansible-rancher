ansible-role-rke_node
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

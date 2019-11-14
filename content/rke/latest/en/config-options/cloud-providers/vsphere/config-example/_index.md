---
title: vSphere Configuration Example
weight: 1
---

Given the following:

- VMs in the cluster are running in the same datacenter `eu-west-1` managed by the vCenter `vc.example.com`.
- The vCenter has a user `provisioner` with password `secret` with the required roles assigned, see [Prerequisites](#prerequisites).
- The vCenter has a datastore named `ds-1` which should be used to store the VMDKs for volumes.
- A `kubernetes` folder exists in vCenter.

The corresponding configuration for the provider would then be as follows:

```yaml
(...)
cloud_provider:
  name: vsphere
  vsphereCloudProvider:
    virtual_center:
      vc.example.com:
        user: provisioner
        password: secret
        datacenters: eu-west-1
    workspace:
      server: vc.example.com
      folder: vm/kubernetes
      default-datastore: ds-1
      datacenter: eu-west-1

```
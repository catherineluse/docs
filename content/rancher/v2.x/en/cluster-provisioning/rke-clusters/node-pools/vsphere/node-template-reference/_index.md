---
title: vSphere Node Template Configuration Reference
weight: 2
---

The tables below describe the configuration options available in the vSphere node template:

- [Account access](#account-access)
- [Instance options](#instance-options)
- [Scheduling options](#scheduling-options)

# Account Access

| Parameter                | Required | Description |
|:------------------------:|:--------:|:------------------------------------------------------------:|
| vCenter or ESXi Server   |   *      | IP or FQDN of the vCenter or ESXi server used for managing VMs. |
| Port                     |   *      | Port to use when connecting to the server. Defaults to `443`.  |
| Username                 |   *      | vCenter/ESXi user to authenticate with the server. |
| Password                 |   *      | User's password. |

# Instance Options

| Parameter                | Required | Description |
|:------------------------:|:--------:|:------------------------------------------------------------:|
| CPUs                     |   *      | Number of vCPUS to assign to VMs. |
| Memory                   |   *      | Amount of memory to assign to VMs.  |
| Disk                     |   *      | Size of the disk (in MB) to attach to the VMs. |
| Cloud Init               |          | URL of a [RancherOS cloud-config]({{< baseurl >}}/os/v1.x/en/installation/configuration/) file to provision VMs with. This file allows further customization of the RancherOS operating system, such as network configuration, DNS servers, or system daemons.|
| OS ISO URL               |   *      | URL of a RancherOS vSphere ISO file to boot the VMs from. You can find URLs for specific versions in the [Rancher OS GitHub Repo](https://github.com/rancher/os). |
| Configuration Parameters |          | Additional configuration parameters for the VMs. These correspond to the [Advanced Settings](https://kb.vmware.com/s/article/1016098) in the vSphere console. Example use cases include providing RancherOS [guestinfo]({{< baseurl >}}/os/v1.x/en/installation/running-rancheros/cloud/vmware-esxi/#vmware-guestinfo) parameters or enabling disk UUIDs for the VMs (`disk.EnableUUID=TRUE`). |

# Scheduling Options

| Parameter                | Required | Description |
|:------------------------:|:--------:|:------------------------------------------------------------:|
| Data Center              |   *      | Name/path of the datacenter to create VMs in.          |
| Pool                     |          | Name/path of the resource pool to schedule the VMs in. If not specified, the default resource pool is used.  |
| Host                     |          | Name/path of the host system to schedule VMs in. If specified, the host system's pool will be used and the *Pool* parameter will be ignored. |
| Network                  |   *      | Name of the VM network to attach VMs to. |
| Data Store               |   *      | Datastore to store the VM disks. |
| Folder                   |          | Name/path of folder in the datastore to create the VMs in. Must already exist. |
